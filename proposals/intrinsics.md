---
ms.openlocfilehash: ac4c8760e3b6a0934f01ae634f666af60aa1c0fe
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484568"
---
# <a name="compiler-intrinsics"></a>Derleyici İç Bilgileri

## <a name="summary"></a>Özet

Bu teklif, şu anda etkin şekilde erişilemeyen düşük düzeydeki Il işlem kodları 'ları, `ldftn`, `ldvirtftn`, `ldtoken` ve `calli`sunan dil yapıları sağlar. Bu düşük düzey Opcode 'ları yüksek performans kodunda önemli olabilir ve geliştiricilerin bunlara erişmek için etkili bir yol olması gerekir.

## <a name="motivation"></a>Amacı

Bu özelliğin ilgi çekici ve arka planı, aşağıdaki konuda açıklanmaktadır (özelliğin olası bir uygulamasıdır): 

https://github.com/dotnet/csharplang/issues/191

Bu alternatif tasarım teklifi, özgün teklifin prototip uygulamasını gözden geçirdikten sonra, önemli bir kod tabanı genelinde @msjabby. Bu tasarım @mjsabby, @tmat ve @jkotasönemli bir giriş ile gerçekleştirildi.

## <a name="detailed-design"></a>Ayrıntılı tasarım 

### <a name="allow-address-of-to-target-methods"></a>Hedef yöntemlere adrese izin ver

Yöntem gruplarına artık bir adres ifadesi için bağımsız değişken olarak izin verilir. Böyle bir ifadenin türü `void*`olacaktır. 

``` csharp
class Util { 
    public static void Log() { } 
}

// ldftn Util.Log
void* ptr = &Util.Log; 
```

Burada herhangi bir temsilci dönüştürme yok, yöntem grubundaki filtreleme üyeleri için tek mekanizma statik/örnek erişimine göre belirlenir. Bu, üyeleri ayıramadığından, bir derleme zamanı hatası oluşur.

``` csharp
class Util { 
    public void Log() { } 
    public void Log(string p1) { } 
    public static void Log(int i) { };
}

unsafe {
    // Error: Method group Log has more than one applicable candidate.
    void* ptr1 = &Log; 

    // Okay: only one static member to consider here.
    void* ptr2 = &Util.Log;
}
```

Bu bağlamdaki AddressOf ifadesi aşağıdaki şekilde uygulanacaktır:

- ldftn: yöntem sanal olmayan bir değer.
- ldvirtftn: yöntem sanal olduğunda.

Bu özelliğin kısıtlamaları:

- Örnek yöntemleri yalnızca bir değer üzerinde çağırma ifadesi kullanılırken belirtilebilir
- Yerel işlevler `&`kullanılamaz. Bu yöntemlerin uygulama ayrıntıları, dil tarafından kasıtlı olarak belirtilmez. Bu, statik ve örnek veya tam olarak hangi imzayı yayındıklarınızı içerir.

### <a name="handleof"></a>handleof

`handleof` bağlamsal anahtar sözcüğü `ldtoken` yönergesini kullanarak bir alanı, üyeyi veya türü eşdeğer `RuntimeHandle` türüne çevirir. İfadenin tam türü, `handleof`ad türüne bağlıdır:

- Alan: `RuntimeFieldHandle`
- Tür: `RuntimeTypeHandle`
- Yöntem: `RuntimeMethodHandle`

`handleof` bağımsız değişkenler `nameof`benzerdir. Bu, bir basit ad, tam ad, üye erişimi, belirtilen üye ile temel erişim veya belirtilen üyeyle bu erişim olmalıdır. Bağımsız değişken ifadesi bir kod tanımını tanımlar, ancak hiçbir şekilde değerlendirilmez.

`handleof` ifadesi çalışma zamanında değerlendirilir ve `RuntimeHandle`dönüş türüne sahiptir. Bu, güvenli kod ve güvenli olmayan bir şekilde çalıştırılabilir. 

``` 
RuntimeHandle stringHandle = handleof(string);
```

Bu özelliğin kısıtlamaları:

- `handleof` ifadesinde özellikler kullanılamaz.
- Kapsamda var olan bir `handleof` adı olduğunda `handleof` ifadesi kullanılamaz. Örneğin bir tür, ad alanı, vb...

### <a name="calli"></a>Calli

Derleyici, `.calli` yönergesine verimli bir şekilde çeviren yeni bir `extern` işlevi türü için destek ekler. Extern özniteliği aşağıdaki şeklin özniteliğiyle işaretlenir:

``` csharp
[AttributeUsage(AttributeTargets.Method)]
public sealed class CallIndirectAttribute : Attribute
{
    public CallingConvention CallingConvention { get; }
    public CallIndirectAttribute(CallingConvention callingConvention)
    {
        CallingConvention = callingConvention;
    }
}
```

Bu, geliştiricilerin aşağıdaki biçimde Yöntemler tanımlamasına olanak tanır:

``` csharp
[CallIndirect(CallingConvention.Cdecl)]
static extern int MapValue(string s, void *ptr);

unsafe {
    var i = MapValue("42", &int.Parse);
    Console.WriteLine(i);
}
```

Yöntem üzerinde `CallIndirect` özniteliği uygulanan kısıtlamalar:

- `DllImport` özniteliğine sahip olamaz.
- Genel olamaz.

## <a name="open-issues"></a>Açık sorunlar

### <a name="callingconvention"></a>CallingConvention

Tasarlanan `CallIndirectAttribute`, yönetilen çağırma kuralları için bir girişi olmayan `CallingConvention` enum kullanır. Numaralama, bu çağırma kuralını içerecek şekilde genişletilmelidir veya özniteliğin farklı bir yaklaşım yapması gerekir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="disambiguating-method-groups"></a>Yöntem gruplarını Kesinleştirme

Bir adres ifadesine geçirilen yöntem gruplarının belirsizliğini daha kolay hale getirmek için özellikler etrafında bazı tartışmalar vardı. Örneğin, sözdizimine imza öğeleri ekleme olasılığı vardır:

``` csharp
class Util {
    public static void Log() { ... }
    public static void Log(string) { ... }
}

unsafe {
    // Error: ambiguous Log
    void *ptr1 = &Util.Log;

    // Use Util.Log();
    void *ptr2 = &Util.Log();
}
```

Bu, etkileyici bir servis talebi yapıldığından veya basit bir sözdiziminin envisioned olmadığı için reddedildi. Ayrıca, çok basit bir ileri doğru iş vardır: basit olan ve istenen işleve çağrı yapmak için kodu C# kullanan başka bir yöntem belirleyin. 

``` csharp
class Workaround {
    public static void LocalLog() => Util.Log();
}
unsafe { 
    void* ptr = &Workaround.LocalLog;
}
```

Bu, `static` yerel işlevler dile girerken daha da kolay hale gelir. Sonra, geçici bir işlem, belirsiz bir işlem olan işlev için aynı işlevde tanımlanabilir:

``` csharp
unsafe { 
    static void LocalLog() => Util.Log();
    void* ptr = &Workaround.LocalLog;
}
```

### <a name="loadtypetokenint32"></a>LoadTypeTokenInt32

Meta veri belirteçlerinin derleme zamanında `int` değerler olarak yüklenmesine izin verilen orijinal teklif. Temelde, `handleof` aynı bağımsız değişkenlere sahip olan, ancak derleme zamanında `int` sabitine değerlendirilen `tokenof` vardır. 

Bu, Il yeniden yazarken önemli bir soruna neden olduğundan (.NET 'in birçok sürümü olduğu için) reddedildi. Bu tür reyazarlar genellikle meta veri tablolarını bu değerleri geçersiz kılacak şekilde işleyebilir. Bu tür reyazanların basit `int` değerler olarak depolandıklarında bu değerleri güncelleştirmesi için makul bir yol yoktur.

Meta veri girdileri için donuk bir tutamacı olmanın temel fikri, çalışma zamanı ekibi tarafından araştırmeye devam edecektir. 

## <a name="future-considerations"></a>Gelecekteki konular

### <a name="static-local-functions"></a>statik yerel işlevler

Bu, yerel işlevlerde `static` değiştiriciye izin veren [öneriyi](https://github.com/dotnet/csharplang/issues/1565) ifade eder. Bu tür bir işlevin, `static` olarak ve kaynak kodunda tam imzayla belirtilen şekilde yayınlanacağı garanti edilir. Bu tür bir işlev, yerel işlevlerin bugünkü sorunlarından hiçbirini içermediğinden `&` için geçerli bir bağımsız değişken olmalıdır.

### <a name="nativecallableattribute"></a>NativeCallableAttribute

CLR 'nin, yönetilen yöntemlerin yerel koddan doğrudan çağrılabilir şekilde oluşturulmasına izin veren bir özelliği vardır. Bu, `NativeCallableAttribute` yöntemlere eklenerek yapılır. Böyle bir yöntem yalnızca yerel koddan çağrılabilir ve bu nedenle yalnızca İmzada blittable türleri içermelidir. Yönetilen koddan çağırma, bir çalışma zamanı hatasına neden olur. 

Bu özellik, izin verilen şekilde bu teklifle birlikte kalıp düzenine neden olur:

- Yönetilen kodda tanımlanan bir işlevi, yönetilen veya yerel kodda ek yüke sahip olmayan bir işlev işaretçisi (adres aracılığıyla) olarak yerel koda geçirme. 
- Çalışma zamanı, Yönetilen koddaki bu gibi işlevler için site hatalarını, derleme zamanında çağrılmasını engellemek için kullanabilir.




