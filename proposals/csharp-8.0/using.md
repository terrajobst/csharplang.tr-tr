---
ms.openlocfilehash: 91afbc3e3412049cd183c36c8035f1862c520413
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485058"
---
# <a name="pattern-based-using-and-using-declarations"></a>"model tabanlı kullanımı" ve "bildirimleri kullanma"

## <a name="summary"></a>Özet

Dil, Kaynak yönetiminin daha basit olması için `using` bildirimi etrafında iki yeni özellik ekler: `using`, `IDisposable` ek olarak bir atılabilir deseninin tanınması ve dile bir `using` bildirimi eklemektir.

## <a name="motivation"></a>Amacı

`using` ifade, bugün kaynak yönetimi için etkili bir araçtır, ancak tam olarak biraz sayıda sertifika gerektirir. Yönetilecek sayıda kaynağı olan Yöntemler, bir dizi `using` deyimleriyle sözdizimsel sürüklenmeden altına alabilir. Bu sözdizimi yükü, çoğu kodlama stili yönergelerinin bu senaryo için küme ayraçları etrafında açık bir özel duruma sahip olması yeterlidir. 

`using` bildirimi buradaki pek çok sertifikayı ortadan kaldırır ve kaynak yönetim bloklarını içeren C# diğer dillere göre değer alır. Ayrıca, model tabanlı `using`, geliştiricilerin buraya katılabileceğiniz tür kümesini genişletmesine imkan tanır. Birçok durumda, yalnızca bir `using` bildiriminde bir değer kullanımına izin vermek için var olan sarmalayıcı türleri oluşturma ihtiyacını ortadan kaldırır. 

Bu özellikler, geliştiricilerin `using` uygulanabilecek senaryoları basitleştirmesini ve genişletmesini sağlar.

## <a name="detailed-design"></a>Ayrıntılı tasarım 

### <a name="using-declaration"></a>using bildirimi

Dil, `using` yerel bir değişken bildirimine eklenmesine izin verir. Bu tür bir bildirim, aynı konumdaki `using` deyimindeki değişkeni bildirerek aynı etkiye sahip olacaktır.

```csharp
if (...) 
{ 
   using FileStream f = new FileStream(@"C:\users\jaredpar\using.md");
   // statements
}

// Equivalent to 
if (...) 
{ 
   using (FileStream f = new FileStream(@"C:\users\jaredpar\using.md")) 
   {
    // statements
   }
}
```

Bir yerel `using` ömrü, bildirildiği kapsamın sonuna genişletilir. `using` Yereller, bildirildiği ters sırada atılecektir. 

```csharp
{ 
    using var f1 = new FileStream("...");
    using var f2 = new FileStream("..."), f3 = new FileStream("...");
    ...
    // Dispose f3
    // Dispose f2 
    // Dispose f1
}
```

`goto`ya da başka bir denetim akışı yapısının `using` bildirimi çerçevesinde hiçbir kısıtlama yoktur. Bunun yerine, kod yalnızca eşdeğer `using` ifadesiyle olduğu gibi davranır:

```csharp
{
    using var f1 = new FileStream("...");
  target:
    using var f2 = new FileStream("...");
    if (someCondition) 
    {
        // Causes f2 to be disposed but has no effect on f1
        goto target;
    }
}
```

`using` yerel bildiriminde belirtilen yerel, örtük olarak salt okunurdur. Bu, bir `using` bildiriminde belirtilen Yereller davranışıyla eşleşir. 

`using` bildirimleri için dil dilbilgisi aşağıdaki gibi olacaktır:

```antlr
local-using-declaration:
  using type using-declarators

using-declarators:
  using-declarator
  using-declarators , using-declarator
  
using-declarator:
  identifier = expression
```

`using` bildirimi etrafındaki kısıtlamalar:

- Doğrudan bir `case` etiketinin içinde görünmeyebilir, bunun yerine `case` etiketi içindeki bir blok dahilinde olmalıdır.
- `out` değişken bildiriminin parçası olarak görünmeyebilir. 
- Her bildirimci için bir başlatıcıya sahip olmalıdır.
- Yerel türün `IDisposable` için örtük olarak dönüştürülebilir olması veya `using` deseninin yerine getirilmesi gerekir.

### <a name="pattern-based-using"></a>model tabanlı using

Dil bir atılabilir deseninin kavramını ekler: Bu, erişilebilir bir `Dispose` örnek yöntemi olan bir türdür. Atılabilir düzenine uyan türler, `IDisposable`uygulanması gerekmeden `using` ifadeye veya bildirimine katılabilir. 

```csharp
class Resource
{ 
    public void Dispose() { ... }
}

using (var r = new Resource())
{
    // statements
}
```

Bu, geliştiricilerin birçok yeni senaryoda `using` yararlanmasını sağlar:

- `ref struct`: Bu türler, arabirimler uygulayamaz ve bu nedenle `using` deyimlerine katılamaz.
- Uzantı yöntemleri, geliştiricilerin `using` deyimlerine katılmak için diğer derlemelerdeki türleri genişletmelerine izin verir.

Bir türün `IDisposable` örtük olarak dönüştürülebileceği ve ayrıca atılabilir düzenine uygun olduğu durumlarda, `IDisposable` tercih edilir. Bu, `foreach` karşı yaklaşımla (arabirim üzerinden tercih edilen model), geriye doğru uyumluluk için gereklidir.

Geleneksel bir `using` deyimden aynı kısıtlamalar burada da geçerlidir: `using` olarak belirtilen yerel değişkenler salt okunurdur, bir `null` değeri özel durumun oluşturulmasına neden olmaz, vb... Kod üretimi yalnızca, Dispose çağrılmadan önce `IDisposable` için bir dönüştürme olmayacak şekilde farklı olacaktır:

```csharp
{
      Resource r = new Resource();
      try {
            // statements
      }
      finally {
            if (resource != null) resource.Dispose();
      }
}
```

Atılabilir düzenine sığması için `Dispose` yöntemi erişilebilir, parametresiz ve `void` bir dönüş türüne sahip olmalıdır. Başka kısıtlama yoktur. Bu açıkça, uzantı yöntemlerinin burada kullanılabileceği anlamına gelir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="case-labels-without-blocks"></a>bloklar olmadan Case etiketleri

`using declaration` gerçek yaşam süresi etrafında karmaşıklıklar nedeniyle doğrudan bir `case` etiketinin içinde geçersizdir. Olası bir çözüm aynı yaşam süresine aynı konumda `out var`. Özellik uygulamasına yönelik ekstra karmaşıklık ve çözüm kolaylığı (yalnızca `case` etiketine bir blok eklemeniz), bu yolun yönlendirilmeyeceğini kabul etmedi.

## <a name="future-expansions"></a>Geleceğe yönelik genişletmeleri

### <a name="fixed-locals"></a>Sabit Yereller

`fixed` deyiminde, `using` yerellerine sahip bir özelliği olan `using` deyimlerinin tüm özellikleri vardır. Bu özelliği, Yereller de `fixed` genişletmek için dikkat edilmelidir. Yaşam süresi ve sıralama kuralları `using` için eşit ölçüde iyi bir şekilde uygulanmalıdır ve burada `fixed`.
