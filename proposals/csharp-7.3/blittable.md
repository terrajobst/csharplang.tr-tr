---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484624"
---
# <a name="unmanaged-type-constraint"></a>Yönetilmeyen tür kısıtlaması

## <a name="summary"></a>Özet
[summary]: #summary

Yönetilmeyen kısıtlama özelliği dil zorlamasına, C# dil belirtiminde "yönetilmeyen türler" olarak bilinen türler sınıfına sahip olur.  Bu, başvuru türü olmayan ve herhangi bir iç içe geçme düzeyinde başvuru türü alanları içermeyen bir tür olarak 18,2 bölümünde tanımlanmıştır.  

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Birincil mosyon, ' de C#düşük düzey birlikte çalışma kodu yazmayı kolaylaştırmaktır. Yönetilmeyen türler, birlikte çalışma kodu için temel yapı taşlarından biridir, ancak genel türlerde desteğin olmaması, tüm yönetilmeyen türler arasında yeniden kullanılabilir yordamlar oluşturmayı olanaksız hale getirir. Bunun yerine, geliştiriciler kitaplığındaki her yönetilmeyen tür için aynı Boiler levha kodunu yazmak üzere zorlanır:

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

Bu tür bir senaryoyu etkinleştirmek için dil yeni bir kısıtlama tanıtıyor: yönetilmeyen:

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

Bu kısıtlama yalnızca C# dil belirtiminde yönetilmeyen tür tanımına sığan türlerle karşılanabilecek. Bunun bir başka yolu da bir türün yönetilmeyen kısıtlamayı karşılamasının bir işaretçi olarak kullanılması. 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

Yönetilmeyen kısıtlama içeren tür parametreleri, yönetilmeyen türler için kullanılabilen tüm özellikleri kullanabilir: işaretçiler, sabit, vb... 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

Bu kısıtlama, yapılandırılmış veriler ve bayt akışları arasında verimli dönüştürmeler olmasını da mümkün kılar. Bu, ağ yığınları ve serileştirme katmanlarında ortak olan bir işlemdir:

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

Bu tür yordamlar, derleme zamanında provably güvende olduklarından ve serbest bırakıldığı için avantajlıdır.  Birlikte çalışma yazarları bugün bunu yapamayabilir (performans açısından kritik olan bir katmanda olmasına rağmen).  Bunun yerine, değerlerin doğru şekilde yönetilmeyen olduğunu doğrulamak için pahalı çalışma zamanı denetimleri olan yordamları ayırmaya güvenmeleri gerekir.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Dilde `unmanaged`adlı yeni bir kısıtlama tanıtılacaktır. Bu kısıtlamayı karşılamak için, bir tür yapı olmalıdır ve türün tüm alanları aşağıdaki kategorilerden birine denk gelmelidir:

- `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` veya `UIntPtr`türüne sahip olmalıdır.
- Herhangi bir `enum` türü olmalıdır.
- Bir işaretçi türü olmalıdır.
- `unmanaged` kısıtlamasını sattı eden Kullanıcı tanımlı bir yapı olmalıdır.

Derleyici tarafından oluşturulan örnek alanları, örneğin otomatik uygulanan özellikler, bu kısıtlamaları da karşılamalıdır. 

Örneğin:

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

`unmanaged` kısıtlaması `struct`, `class` veya `new()`ile birleştirilemez. Bu kısıtlama, `unmanaged` `struct` anlamına gelir, bu nedenle diğer kısıtlamalar anlamlı hale getirir.

`unmanaged` kısıtlaması CLR tarafından yalnızca dile göre zorlanmaz. Diğer dillerin yanlış kullanımını engellemek için, bu kısıtlamaya sahip Yöntemler bir mod-REQ tarafından korunur. Bu, diğer dillerin yönetilmeyen türler olmayan tür bağımsız değişkenlerini kullanmasını engeller.

Kısıtlamadaki belirteç `unmanaged` bir anahtar sözcük, ya da bağlamsal anahtar sözcük değil. Bunun yerine, bu konumda değerlendirildiğinden `var` gibidir ve şunları olur:

- `unmanaged`adlı Kullanıcı tanımlı veya başvurulan türe bağla: Bu, diğer adlandırılmış tür kısıtlaması işlendiği gibi değerlendirilir. 
- Hiçbir tür için bağlama: Bu, `unmanaged` kısıtlaması olarak yorumlanacaktır.

`unmanaged` adlı bir tür varsa ve geçerli bağlamda nitelendirme olmadan kullanılabiliyorsa, `unmanaged` kısıtlamasını kullanmanın bir yolu olmayacaktır. Bu, özelliği `var` çevreleyen kuralları ve aynı ada sahip kullanıcı tanımlı türleri paraleller. 

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bu özelliğin birincil dezavantajı, az sayıda geliştirici sunmesidir: genellikle düşük düzey kitaplık yazarları veya çerçeveleri.  Bu nedenle, az sayıda geliştirici için değerli bir dil süresi harcadığından. 

Bu çerçeveler, genellikle burada .NET uygulamalarının çoğunluğunun temelini oluşturur.  Bu nedenle, bu düzeyde WINS performansı/doğruluğu, .NET ekosistemi üzerinde Ripple etkiye sahip olabilir.  Bu, özelliği sınırlı kitlelerle hatta göz önünde bulundurmayı sağlar.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Göz önünde bulundurulması gereken birkaç seçenek vardır:

- Durum Quo: Özellik kendi birleşmesine göre hizalı değildir ve geliştiriciler örtük kabul etme davranışını kullanmaya devam eder.

## <a name="questions"></a>UL
[quesions]: #questions

### <a name="metadata-representation"></a>Meta veri gösterimi

F# Dil imza dosyasındaki kısıtlamayı kodluyor, bu da temsilinin C# yeniden kullanılamayacağı anlamına gelir. Bu kısıtlama için yeni bir özniteliğin seçilmesi gerekir. Ayrıca, bu kısıtlamayı içeren bir yöntemin bir mod-REQ tarafından korunması gerekir.

### <a name="blittable-vs-unmanaged"></a>Blittable vs. yönetilmeyen
Dil, yönetilmeyen anahtar sözcüğünü kullanan çok benzer bir özelliğe sahiptir. [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) F# Blittable adı Midori ' deki kullanım öğesinden gelir.  Burada önceliğe bakmak ve bunun yerine yönetilmeyen ' i kullanmak isteyebilir. 

**Çözüm** Yönetilmeyen bu dilin kullanımına karar 

### <a name="verifier"></a>'Nın

Genel tür parametrelerine işaretçiler kullanımını anlamak için doğrulayıcı/çalışma zamanının güncellenmesi gerekiyor mu?  Ya da yalnızca değişiklik olmayan şekilde çalışabilir mi?

**Çözüm** Değişiklik gerekmiyor. Tüm işaretçi türleri doğrulanamaz. 

## <a name="design-meetings"></a>Tasarım toplantıları

yok
