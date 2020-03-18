---
ms.openlocfilehash: b0d0fa70d90f7493c6c23be576155a77cec36cf8
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485184"
---
# <a name="default-interface-methods"></a>varsayılan arabirim yöntemleri

* [x] önerilir
* [] Prototip: [devam ediyor](https://github.com/dotnet/roslyn/blob/master/docs/features/DefaultInterfaceImplementation.md)
* [] Uygulama: yok
* [] Belirtim: devam ediyor, aşağıda

## <a name="summary"></a>Özet
[summary]: #summary

_Sanal uzantı yöntemleri_ için destek ekleyin-somut uygulamalarla arabirimlerdeki Yöntemler. Bu tür bir arabirimi uygulayan bir sınıf veya yapının, sınıf veya yapı tarafından uygulanan ya da temel sınıflarından veya arabirimlerinden Devralındığı arabirim yöntemi için tek bir _en özel_ uygulamaya sahip olması gerekir. Sanal uzantı yöntemleri bir API yazarının, bu arabirimin var olan uygulamalarıyla kaynak veya ikili uyumluluğu bozmadan gelecek sürümlerde bir arabirime Yöntemler eklemesini sağlar.

Bunlar, Java 'nın ["varsayılan yöntemlere"](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)benzerdir.

(Olası uygulama tekniğinin temel alınarak), bu özellik CLı/CLR 'de ilgili desteği gerektirir. Bu özellikten faydalanan programlar platformun önceki sürümlerinde çalıştırılamaz.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bu özelliğe ilişkin asıl lamalar şunlardır

- Varsayılan arabirim yöntemleri bir API yazarının, bu arabirimin var olan uygulamalarıyla kaynak veya ikili uyumluluğu bozmadan gelecek sürümlerde bir arabirime Yöntemler eklemesini sağlar.
- Özelliği, benzer C# özellikleri destekleyen [Android (Java)](http://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html) ve [IOS (Swift)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID267)hedefleyen API 'lerle birlikte çalışabilmesine olanak sağlar.
- , Varsayılan arabirim uygulamalarını eklemek, "nitelikler" dil özelliğinin (<https://en.wikipedia.org/wiki/Trait_(computer_programming)>) öğelerini sağlar. Nitelikler güçlü bir programlama tekniği (<http://scg.unibe.ch/archive/papers/Scha03aTraits.pdf>) olarak kanıtlanmış bir yöntemdir.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Bir arabirimin sözdizimi izin verecek şekilde genişletilir

- sabitleri, işleçleri, statik oluşturucuları ve iç içe geçmiş türleri bildiren üye bildirimleri;
- bir yöntem veya Dizin Oluşturucu, özellik veya olay erişimcisi (yani "varsayılan" bir uygulama) için *gövde* .
- statik alanlar, Yöntemler, özellikler, Dizin oluşturucular ve olaylar bildiren üye bildirimleri;
- Açık arabirim uygulama söz dizimini kullanan üye bildirimleri; '
- Açık erişim değiştiricileri (varsayılan erişim `public`).

Gövdeler içeren Üyeler, arabirimin, geçersiz kılan bir uygulama sağlamayan sınıflarda ve yapılarda yöntemi için "varsayılan" bir uygulama sağlamasına izin verir.

Arabirimler örnek durumu içermeyebilir. Statik alanlara artık izin verildiğinde, arabirimlerde örnek alanlarına izin verilmez. Örnek otomatik özellikler, açıkça gizli bir alan bildirdiklerinde arabirimlerde desteklenmez.

Statik ve özel yöntemler, arabirimin ortak API 'sini uygulamak için kullanılan kodun faydalı yeniden düzenlemesi ve organizasyona izin verir.

Bir arabirimdeki yöntem geçersiz kılma açık arabirim uygulama sözdizimini kullanmalıdır.

Bir *variance_annotation*ile tanımlanmış bir tür parametresinin kapsamı içinde bir sınıf türü, yapı türü veya sabit listesi türü bildirmek hatadır.  Örneğin, aşağıdaki `C` bildirimi bir hatadır.

```csharp
interface IOuter<out T>
{
    class C { } // error: class declaration within the scope of variant type parameter 'T'
}
```

### <a name="concrete-methods-in-interfaces"></a>Arabirimlerde somut Yöntemler

Bu özelliğin en basit biçimi, gövde içeren bir yöntem olan bir arabirimde *somut bir yöntem* bildirebilmesidir.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
```

Bu arabirimi uygulayan bir sınıf somut metodunu gerçekleştirmemelidir.

```csharp
class C : IA { } // OK

IA i = new C();
i.M(); // prints "IA.M"
```

`C` sınıfındaki `IA.M` için nihai geçersiz kılma, `IA`içinde bildirildiği somut yöntemdir `M`. Bir sınıfın, arabirimlerinden üyeleri devralmasını unutmayın; Bu özellik tarafından değiştirilmez:

```csharp
new C().M(); // error: class 'C' does not contain a member 'M'
```

Bir arabirimin örnek üyesi içinde, `this` kapsayan arabirimin türü vardır.

### <a name="modifiers-in-interfaces"></a>Arabirimlerde değiştiriciler

Bir arabirimin sözdizimi, üyelerinde değiştiricilere izin vermek için gevşek olur. Aşağıdakiler izin verilir: `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern`ve `partial`.

> ***Todo***: başka değiştiricilerin var olduğunu denetleyin.

Bildirimi içeren bir arabirim üyesi, `sealed` veya `private` değiştiricisi kullanılmadığı müddetçe bir `virtual` üyesidir. `virtual` değiştiricisi, aksi takdirde örtük olarak `virtual`bir işlev üyesinde kullanılabilir. Benzer şekilde, `abstract` gövdeler olmadan arabirim üyelerinde varsayılan değer olsa da, bu değiştirici açıkça verilebilir. Sanal olmayan bir üye `sealed` anahtar sözcüğü kullanılarak bildirilebilecek.

Bir arabirimin `private` veya `sealed` işlev üyesinin gövdeye sahip olmaması hatadır. `private` işlev üyesi değiştirici `sealed`sahip olamaz.

Erişim değiştiricileri, izin verilen tüm üye türlerinin arabirim üyelerinde kullanılabilir. Erişim düzeyi `public` varsayılandır ancak açık olarak verilebilir.

> ***Açık sorun:*** `protected` ve `internal`gibi erişim değiştiricilerin kesin anlamlarını belirtmemiz ve bu bildirimleri hangi bildirimlerin (türetilmiş bir arabirimde) ve bunların (örneğin, arabirimini uygulayan bir sınıfta) (türetilen bir arabirimde) ve bunların nasıl uygulanacağını belirtmemiz gerekir.

Arabirimler, iç içe türler, Yöntemler, Dizin oluşturucular, özellikler, olaylar ve statik oluşturucular dahil `static` üyelerini bildirebilir. Tüm arabirim üyeleri için varsayılan erişim düzeyi `public`.

Arabirimler örnek oluşturucular, Yıkıcılar veya alanlar bildiremeyebilir.

> ***Kapatılan sorun:*** Bir arabirimde işleç bildirimlerine izin verilmelidir mi? Büyük olasılıkla dönüştürme işleçleri değil, diğerleri ne olacak? ***Karar***: işleçlere dönüştürme, eşitlik ve eşitsizlik işleçleri *haricinde* izin verilir.

> ***Kapatılan sorun:*** Temel arabirimlerin üyelerini gizleyen arabirim üye bildirimlerinde `new` izin verilmelidir mi? ***Karar***: Evet.

> ***Kapatılan sorun:*** Şu anda bir arabirim veya üyeleri üzerinde `partial` izin vermedik. Bu, ayrı bir teklif gerektirir. ***Karar***: Evet. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>

### <a name="overrides-in-interfaces"></a>Arabirimlerde geçersiz kılmalar

Geçersiz kılma bildirimleri (yani `override` değiştiricisini içeren olanlar), programcının derleyicinin veya çalışma zamanının başka bir yerde bulamadığı bir arabirimde bir sanal üyenin en belirli bir uygulamasını sağlamasına izin verir. Ayrıca, bir üst arabirimden bir Özet üyenin türetilmiş arabirimdeki varsayılan üyeye açılmasını sağlar. Bir geçersiz kılma bildiriminin, arabirimi adıyla bildirimi niteleyerek belirli bir temel arabirim yöntemini *açıkça* geçersiz kılmasına izin verilir (Bu durumda erişim değiştiricisine izin verilmez). Örtük geçersiz kılmalara izin verilmez.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); } // explicitly named
}
interface IC : IA
{
    override void M() { WriteLine("IC.M"); } // implicitly named
}
```

Arabirimlerdeki geçersiz kılma bildirimleri `sealed`bildirilmemiş olabilir.

Bir arabirimdeki ortak `virtual` işlev üyeleri, açıkça bir türetilmiş arabirimde geçersiz kılınabilir (geçersiz kılma bildiriminde adı, ilk olarak yöntemi tanımlayan arabirim türü ile niteleyerek ve bir erişim değiştiricisi hariç).

bir arabirimdeki `virtual` işlev üyeleri yalnızca türetilmiş arabirimlerde açıkça geçersiz kılınabilir (örtük değil) ve `public` olmayan üyeler yalnızca bir sınıf veya yapıda uygulanabilir (örtük değil). Her iki durumda da, geçersiz kılınan veya uygulanan üyenin geçersiz kılındığı yerde *erişilebilir* olması gerekir.

### <a name="reabstraction"></a>Reabstraction

Bir arabirimde belirtilen sanal (somut) bir yöntem, türetilmiş bir arabirimde Özet olması için geçersiz kılınabilir

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { } // error: class 'C' does not implement 'IA.M'.
```

`abstract` değiştiricisi, `IB.M` bildiriminde gerekli değildir (Bu, arabirimlerde varsayılandır), ancak büyük olasılıkla bir geçersiz kılma bildiriminde açık olması iyi bir uygulamadır.

Bu, bir yöntemin varsayılan uygulamasının uygunsuz olduğu ve sınıflar uygulanarak daha uygun bir uygulamanın sağlanması gereken türetilmiş arabirimlerde yararlıdır.

> ***Açık sorun:*** Yeniden izin verilmelidir mi?

### <a name="the-most-specific-override-rule"></a>En özel geçersiz kılma kuralı

Her arabirim ve sınıfın, türde veya doğrudan ve dolaylı arabirimlerde görüntülenen geçersiz kılmalar arasında her sanal üye için *en belirgin bir geçersiz kılmalara* sahip olması gerekir. *En özel geçersiz* kılma, tüm diğer geçersiz kılmada daha belirgin olan benzersiz bir geçersiz kılma. Geçersiz kılma yoksa, üyenin en belirgin geçersiz kılma kabul edilir.

Bir geçersiz kılma `M1` başka bir geçersiz kıltan *daha belirgin* kabul edilir `M2` `M1` tür `T1`bildiriminde bildirilirse, `M2` tür `T2`olarak bildirilirse ve her iki

1. `T1` doğrudan veya dolaylı arabirimleri arasında `T2` içerir ya da
2. `T2` bir arabirim türüdür, ancak `T1` bir arabirim türü değil.

Örneğin:

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    void IA.M() { WriteLine("IC.M"); }
}
interface ID : IB, IC { } // error: no most specific override for 'IA.M'
abstract class C : IB, IC { } // error: no most specific override for 'IA.M'
abstract class D : IA, IB, IC // ok
{
    public abstract void M();
}

```

En özel geçersiz kılma kuralı bir çakışmanın (Yani elmas devralım işleminden kaynaklanan bir belirsizlik), çakışmanın gerçekleştiği noktada programcı tarafından açıkça çözümlenmesini sağlar.

Arabirimlerde açık soyut geçersiz kılmaları destekliyoruz, bu işlemleri de sınıflarda yapabiliriz

```csharp
abstract class E : IA, IB, IC // ok
{
    abstract void IA.M();
}
```

> ***Açık sorun***: sınıflarda açık arabirim soyut geçersiz kılmalarını destekliyoruz.

Ayrıca, bir sınıf bildiriminde bir hata olduğundan, bazı arabirim yönteminin en belirgin geçersiz kılma işlemi bir arabirimde tanımlanmış soyut bir geçersiz kılma niteliğindedir. Bu, yeni terminoloji kullanılarak yeniden kullanılan mevcut bir kuraldır.

```csharp
interface IF
{
    void M();
}
abstract class F : IF { } // error: 'F' does not implement 'IF.M'
```

Bir arabirimde belirtilen bir sanal özelliğin, bir arabirimdeki `get` erişimcisi için en belirli bir geçersiz kılma ve farklı bir arabirimdeki `set` erişimcisi için en özel geçersiz kılma olması mümkündür. Bu, *en özel geçersiz kılma* kuralı ihlali olarak değerlendirilir.

### <a name="static-and-private-methods"></a>`static` ve `private` yöntemleri

Arabirimler artık yürütülebilir kod içerebileceğinden, özel ve statik yöntemlere soyut ortak kod de faydalı olabilir. Artık arabirimlerde bunlara izin veriyoruz.

> ***Kapatılan sorun***: özel yöntemleri desteklemelidir mi? Statik yöntemler destekliyoruz mi? **Karar: Evet**

> ***Açık sorun***: arabirim yöntemlerinin `protected` veya `internal` ya da başka bir erişime izin vermemiz gerekir mi? Öyleyse, semantik anlamı nedir? Bunlar varsayılan olarak `virtual` mı? Öyleyse, bunları sanal olmayan hale getirmenin bir yolu var mı?

> ***Açık sorun***: statik yöntemleri destekliyoruz, (statik) işleçlerini destekliyoruz.

### <a name="base-interface-invocations"></a>Temel arabirim etkinleştirmeleri

Varsayılan bir yöntemi olan bir arabirimden türetilen bir türdeki kod, bu arabirimin "temel" uygulamasını açıkça çağırabilir.

```csharp
interface I0
{
   void M() { Console.WriteLine("I0"); }
}
interface I1 : I0
{
   override void M() { Console.WriteLine("I1"); }
}
interface I2 : I0
{
   override void M() { Console.WriteLine("I2"); }
}
interface I3 : I1, I2
{
   // an explicit override that invoke's a base interface's default method
   void I0.M() { I2.base.M(); }
}

```

Örnek (statik olmayan) yönteminin, `base(Type).M`söz dizimi kullanılarak adlandırılarak doğrudan temel arabirimdeki erişilebilir bir örnek yönteminin uygulanmasını çağırmasına izin verilir. Bu, elmas devralma nedeniyle belirli bir temel uygulamaya temsilci ile çözülebildiğinden, bunun için gerekli olan bir geçersiz kılma sağlanması yararlı olur.

```csharp
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    override void IA.M() { WriteLine("IB.M"); }
}
interface IC : IA
{
    override void IA.M() { WriteLine("IC.M"); }
}

class D : IA, IB, IC
{
    void IA.M() { base(IB).M(); }
}
```

`virtual` veya `abstract` üyesine sözdizimi `base(Type).M`kullanılarak erişildiğinde, `Type` `M`için benzersiz bir *en özel geçersiz kılma* içermesi gerekir.

### <a name="binding-base-clauses"></a>Temel yan tümceleri bağlama

Arabirimler artık türler içerir.  Bu türler taban yan tümcesinde temel arabirimler olarak kullanılabilir.  Bir taban yan tümcesini bağlarken, bu türleri bağlamak için temel arabirimlerin kümesini bilmemiz gerekebilir (örneğin, bunlarda arama yapmak ve korumalı erişimi çözümlemek için).  Bir arabirimin temel yan tümcesinin anlamı, bu nedenle devre dışı olarak tanımlandı.  Döngüyü bölmek için, sınıflar için zaten var olan benzer bir kurala karşılık gelen yeni bir dil kuralı ekleyeceğiz.

Bir arabirimin *interface_base* anlamı belirlenirken, temel arabirimlerin geçici olarak boş olduğu varsayılır. Intuicanlı bu, bir taban yan tümcesinin anlamını özyinelemeli olarak bağımlı olmamasını sağlar. 

**Aşağıdaki kurallara sahip olmak için kullandık:**

"Bir sınıf B bir sınıftan türetildiği zaman, A için bir derleme zamanı hatası, B 'ye bağımlıdır. Bir sınıf doğrudan temel sınıfına (varsa) **bağlıdır** ve doğrudan iç içe geçmiş (varsa) ~~**sınıfa**~~ **bağlıdır** . Bu tanım verildiğinde, bir sınıfın bağımlı olduğu ~~**tüm sınıf kümesi**~~ , **doğrudan ilişkiye bağlı olarak değişir** . "

Bir arabirimin doğrudan veya dolaylı olarak kendisinden devraldığı derleme zamanı hatasıdır.
Bir arabirimin **temel arabirimleri** , açık temel arabirimler ve bunların temel arabirimlerdir. Diğer bir deyişle, temel arabirimlerin kümesi açık temel arabirimlerin, kendi açık temel arabirimlerinin ve benzeri tüm geçişli kapamelerden oluşur.

**Bunları aşağıdaki şekilde ayarlamamız:**

B sınıfı A sınıfından türediği zaman, A için bir derleme zamanı hatası, B 'ye bağımlıdır. Bir sınıf doğrudan temel sınıfına (varsa) **bağlıdır** ve doğrudan iç içe geçmiş (varsa) _**türüne**_ **bağlıdır** .

Bir arabirim arabirim bir arabirimini genişlettiği zaman, IA 'nin IB 'e bağlı olması için derleme zamanı hatası olur. Bir arabirim doğrudan temel arabirimlerine (varsa) **bağlıdır** ve doğrudan iç içe geçmiş (varsa) türüne **bağlıdır** .

Bu tanımlar verildiğinde, bir türün bağımlı olduğu tüm **türlerin** kümesi, **doğrudan ilişkiye bağlı olarak değişir** .

### <a name="effect-on-existing-programs"></a>Mevcut programlarla ilgili etki

Burada sunulan kuralların, var olan programların anlamı üzerinde hiçbir etkisi yoktur.

Örnek 1:

```csharp
interface IA
{
    void M();
}
class C: IA // Error: IA.M has no concrete most specific override in C
{
    public static void M() { } // method unrelated to 'IA.M' because static
}
```

Örnek 2:

```csharp
interface IA
{
    void M();
}
class Base: IA
{
    void IA.M() { }
}
class Derived: Base, IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

Aynı kurallar, varsayılan arabirim yöntemlerini içeren benzer duruma benzer sonuçlar verir:

```csharp
interface IA
{
    void M() { }
}
class Derived: IA // OK, all interface members have a concrete most specific override
{
    private void M() { } // method unrelated to 'IA.M' because private
}
```

> ***Kapatılan sorun***: Bu, belirtimin amaçlanan bir sonucu olduğunu onaylayın. **Karar: Evet**

### <a name="runtime-method-resolution"></a>Çalışma zamanı Yöntem çözümlemesi

> ***Kapatılan sorun:*** Spec, çalışma zamanı yöntemi çözüm algoritmasını arabirim varsayılan yöntemlerinin yüzündeki betimlemelidir. Sözdizimi semantiğinin uyumlu olduğundan emin olunması gerekir, örneğin, hangi tanımlanmış yöntemler, bir `internal` metodunu geçersiz kılmaz ve uygulamaz.

### <a name="clr-support-api"></a>CLR desteği API 'SI

Derleyicilerin, bu özelliği destekleyen bir çalışma zamanı için derlendikleri zamanı tespit etmek için, bu tür çalışma zamanları için kitaplıklar, <https://github.com/dotnet/corefx/issues/17116>açıklanan API aracılığıyla tanıtmak için değiştirilir. Ekliyoruz

```csharp
namespace System.Runtime.CompilerServices
{
    public static class RuntimeFeature
    {
        // Presence of the field indicates runtime support
        public const string DefaultInterfaceImplementation = nameof(DefaultInterfaceImplementation);
    }
}
```

> ***Açık sorun***: *clr* özelliği için en iyi ad midir? CLR özelliği, tıpkı çok daha fazlasını yapar (örneğin, koruma kısıtlamalarını yeniden hedefleyen, arabirimlerde geçersiz kılmaları destekler, vb.). Belki de "arabirimlerde somut Yöntemler" veya "nitelikler" gibi bir şey çağrılmalıdır mi?

### <a name="further-areas-to-be-specified"></a>Daha fazla belirtilecek alan

- [] Var olan arabirimlere varsayılan arabirim yöntemleri ve geçersiz kılmalar eklenerek oluşan kaynak ve ikili uyumluluk etkileri türlerini kataloglamak yararlı olacaktır.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bu teklif, CLR belirtimine yönelik eşgüdümlü bir güncelleştirme gerektirir (arabirimlerde ve Yöntem çözümlemede somut yöntemleri desteklemek için). Bu nedenle oldukça "pahalıdır" ve ayrıca, aynı zamanda da tahmin ettiğimiz diğer özelliklerle birlikte, CLR değişiklikleri gerektirdiğini de büyük bir şekilde yapmanız gerekebilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Yok.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- Açık sorulara, yukarıdaki teklifle ilgili olarak çağrılır.
- Ayrıca bkz. açık soruların listesi için <https://github.com/dotnet/csharplang/issues/406>.
- Ayrıntılı belirtim, çağrılacak kesin yöntemi seçmek için çalışma zamanında kullanılan çözüm mekanizmasını açıklamanız gerekir.
- Yeni derleyiciler tarafından üretilen ve eski derleyiciler tarafından tüketilen meta verilerin etkileşimi ayrıntılı olarak kullanıma hazır olmalıdır. Örneğin, kullandığımız meta veri gösteriminin, daha eski bir derleyici tarafından derlendiğinde, bu arabirimi uygulayan mevcut bir sınıfı bölmek için bir arabirimdeki varsayılan uygulamanın eklenmesine neden olmadığından emin olunması gerekir. Bu, kullanabilmemiz için meta veri gösterimini etkileyebilir.
- Tasarım, diğer diller için diğer diller ve mevcut derleyicilerle birlikte çalışabilirliği göz önünde bulundurmalıdır.

## <a name="resolved-questions"></a>Çözümlenen sorular

### <a name="abstract-override"></a>Soyut geçersiz kılma

Önceki taslak belirtimi devralınmış bir yöntemi "reabstract" olarak içeriyordu:

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { }
}
interface IC : IB
{
    override void M(); // make it abstract again
}
```

2017-03-20 için notlarım buna izin vermemeye karar verdiğimiz gösterildi. Ancak, bunun için en az iki kullanım durumu vardır:

1. Bu özelliğin bazı kullanıcılarının birlikte çalışabilme özelliği olan Java API 'Leri, bu tesise bağlıdır.
2. Bu, *nitelikleri* avantajlarıyla programlama. Reabstraction, "nitelikler" dil özelliğinin (https://en.wikipedia.org/wiki/Trait_(computer_programming))öğelerinden biridir. Sınıflarla şunlar için izin verilir:

```csharp
public abstract class Base
{
    public abstract void M();
}
public abstract class A : Base
{
    public override void M() { }
}
public abstract class B : A
{
    public override abstract void M(); // reabstract Base.M
}
```

Ne yazık ki bu kod, izin verilmediği sürece bir arabirim kümesi olarak yeniden düzenlenmiş (nitelikler) olamaz. *Gri olan Juned ilkesi*tarafından izin verilmelidir.

> ***Kapatılan sorun:*** Yeniden izin verilmelidir mi? Yes Notlarım yanlış. [LDM notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) , bir arabirimde reabstraction 'ın izin verileceğini varsayalım. Bir sınıfta değil.

### <a name="virtual-modifier-vs-sealed-modifier"></a>Sanal değiştirici ile korumalı değiştirici

[Aleksey Tsingauz](https://github.com/AlekseyTs)öğesinden:

> Bazı bunların izin verilmediği bir neden olmadıkça, arabirim üyelerinde açıkça belirtilen değiştiricilere izin vermeyi karardık. Bu, sanal değiştiricinin etrafında ilginç bir soru getirir. Varsayılan uygulama olan üyelerde gerekli olması gerekir mi?
>
> Şunu söyliyoruz:
>
> - uygulama yoksa ve ne sanal, ne de mühürlü belirtilmemişse, üyenin soyut olduğunu varsayıyoruz.
> - bir uygulama varsa ve soyut ya da Sealed belirtilmemişse, üyenin sanal olduğunu varsaydık.
> - korumalı değiştirici, bir yöntemi sanal veya soyut olmayan bir şekilde oluşturmak için gereklidir.
>
> Alternatif olarak, sanal bir üye için sanal değiştiricinin gerekli olduğunu varsayalım. Yani, açıkça sanal değiştirici ile işaretlenmemiş bir üye varsa, bu, sanal ya da soyut değildir. Bu yaklaşım, bir yöntem bir sınıftan arabirime taşındığında daha iyi deneyim verebilir:
>
> - soyut bir yöntem soyut kalır.
> - sanal bir yöntem sanal kalır.
> - değiştirici olmadan bir yöntem sanal veya soyut olarak kalır.
> - sealed değiştiricisi geçersiz kılma olmayan bir yönteme uygulanamaz.
>
> Ne düşünüyorsun?

> ***Kapatılan sorun:*** Somut bir Yöntem (uygulama ile) örtük olarak `virtual`mı? Yes

***Kararlar:*** LDM 2017-04-05:

1. sanal olmayan `sealed` veya `private`aracılığıyla açıkça ifade edilmelidir.
2. `sealed`, sanal olmayan gövdelerle arabirim örneği üyelerini oluşturmak için anahtar sözcüktür
3. Arabirimlerde tüm değiştiricilere izin vermek istiyoruz  
4. Arabirim üyeleri için varsayılan erişilebilirlik, iç içe türler dahil geneldir
5. Arabirimlerdeki özel işlev üyeleri örtülü olarak mühürlenmiş ve `sealed` bunlara izin verilmez.
6. Özel sınıflara (arabirimlerde) izin verilir ve mühürlenebilir ve bu, mühürlenmiş sınıf hissi içinde mühürlenmiş anlamına gelir.
7. İyi bir öneri olmadığından arabirimlerde veya üyelerinde kısmen hala izin verilmez.

### <a name="binary-compatibility-1"></a>İkili uyumluluk 1

Bir kitaplık varsayılan bir uygulama sağlıyorsa

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
}
class C : I2
{
}
```

`C` `I1.M` uygulamasının `I1.M`olduğunu anladık. `I2` içeren derleme aşağıdaki gibi değiştirilirse ve yeniden derlendiğinden ne olur?

```csharp
interface I2 : I1
{
    override void M() { Impl2 }
}
```

Ancak `C` yeniden derlenmez. Program çalıştırıldığında ne olur? `(C as I1).M()` çağrısı

1. `I1.M` çalışır
2. `I2.M` çalışır
3. Bir tür çalışma zamanı hatası oluşturur

***Karar:*** 2017-04-11 yapıldı: çalışma zamanında en belirgin, kesin olmayan bir geçersiz kılma olan `I2.M`çalıştırır.

### <a name="event-accessors-closed"></a>Olay erişimcileri (kapalı)

> ***Kapatılan sorun:*** "Piecewise" olayının geçersiz kılınabilmesi

Şu durumu göz önünde bulundurun:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        // error: "remove" accessor missing
    }
}
```

Olayın bu "kısmi" uygulamasına izin verilmez çünkü bir sınıfta olduğu gibi, bir olay bildiriminin sözdizimi yalnızca bir erişimciye izin vermez; her ikisi de (veya hiçbiri) sağlanmalıdır. Söz dizimi içindeki soyut kaldırma erişimcisinin bir gövde yokluğu tarafından örtük olarak soyut olmasını sağlayarak aynı şeyi gerçekleştirebilirsiniz:

```csharp
public interface I1
{
    event T e1;
}
public interface I2 : I1
{
    override event T
    {
        add { }
        remove; // implicitly abstract
    }
}
```

*Bunun yeni bir (önerilen) sözdizimi*olduğunu unutmayın. Geçerli dilbilgisinde, olay erişimcilerinin zorunlu bir gövdesi vardır.

> ***Kapatılan sorun:*** Bir olay erişimcisinin bir gövde atlanmasıyla soyut olması (örtük olarak), arabirimler ve özellik erişimcileri içindeki yöntemlerin bir gövdenin atlanmasıyla soyutlanma biçimini (örtük olarak) sağlar.

***Karar:*** (2017-04-18) Hayır, olay bildirimleri hem somut erişimciler (ya da hiçbirine) gerektirir.

### <a name="reabstraction-in-a-class-closed"></a>Bir sınıfta reabstraction (kapalı)

***Kapatılan sorun:*** Buna izin verildiğini doğrulamamız gerekir (Aksi takdirde, varsayılan bir uygulama eklemek bir son değişiklik olacaktır):

```csharp
interface I1
{
    void M() { }
}
abstract class C : I1
{
    public abstract void M(); // implement I1.M with an abstract method in C
}
```

***Karar:*** (2017-04-18) Evet, bir arabirim üye bildirimine gövde eklemek C 'yi kesmemelidir.

### <a name="sealed-override-closed"></a>Sealed geçersiz kılma (kapalı)

Önceki soru, `sealed` değiştiricinin bir arabirimdeki `override` uygulanabileceğini dolaylı olarak varsayar. Bu, taslak belirtimiyle çelişmektedir. Bir geçersiz kılmayı mühürme izin vermek istiyor musunuz? Mühürleme 'un kaynak ve ikili uyumluluk etkileri göz önünde bulundurulmalıdır.

> ***Kapatılan sorun:*** Bir geçersiz kılmanın mühürlerine izin veririz mi?

***Karar:*** (2017-04-18) arabirimlerde geçersiz kılmalarda `sealed` izin verilmez. Arabirim üyelerinde `sealed` tek kullanımı, ilk bildiriminde sanal olmamaları.

### <a name="diamond-inheritance-and-classes-closed"></a>Elmas devralma ve sınıflar (kapalı)

Teklifin taslağı, elmas devralma senaryolarındaki arabirim geçersiz kılmaları için sınıf geçersiz kılmalarını tercih eder:

> Her arabirim ve sınıfın, türde veya doğrudan ve dolaylı arabirimlerde görüntülenen geçersiz kılmalar arasındaki her arabirim yöntemi için *en belirli bir geçersiz kılmalara* sahip olması gerekir. *En özel geçersiz* kılma, tüm diğer geçersiz kılmada daha belirgin olan benzersiz bir geçersiz kılma. Geçersiz kılma yoksa, yöntemin kendisi en özel geçersiz kılma olarak kabul edilir.
>
> Bir geçersiz kılma `M1` başka bir geçersiz kıltan *daha belirgin* kabul edilir `M2` `M1` tür `T1`bildiriminde bildirilirse, `M2` tür `T2`olarak bildirilirse ve her iki
>
> 1. `T1` doğrudan veya dolaylı arabirimleri arasında `T2` içerir ya da
> 2. `T2` bir arabirim türüdür, ancak `T1` bir arabirim türü değil.

Senaryo budur

```csharp
interface IA
{
    void M();
}
interface IB : IA
{
    override void M() { WriteLine("IB"); }
}
class Base : IA
{
    void IA.M() { WriteLine("Base"); }
}
class Derived : Base, IB // allowed?
{
    static void Main()
    {
        Ia a = new Derived();
        a.M();           // what does it do?
    }
}
```

Bu davranışı onaylamanız gerekir (veya başka bir karar vermeniz gerekir)

> ***Kapatılan sorun:*** Karma sınıflar ve arabirimler için geçerli olduğu için, yukarıdaki taslak belirtimini, yukarıda *belirtilen geçersiz kılma* için (bir sınıf bir arabirim üzerinden önceliklidir) onaylayın. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#diamonds-with-classes>.

### <a name="interface-methods-vs-structs-closed"></a>Arabirim yöntemleri vs yapılar (kapalı)

Varsayılan arabirim yöntemleri ve yapılar arasında bazı talihsiz etkileşimleri vardır.

```csharp
interface IA
{
    public void M() { }
}
struct S : IA
{
}
```

Arabirim üyelerinin devralınmadığını unutmayın:

```csharp
var s = default(S);
s.M(); // error: 'S' does not contain a member 'M'
```

Sonuç olarak, istemci arabirim yöntemlerini çağırmak için yapıyı Box olmalıdır

```csharp
IA s = default(S); // an S, boxed
s.M(); // ok
```

Bu şekilde kutulama, `struct` türünün asıl avantajlarını erteler. Ayrıca, herhangi bir mutasyon yöntemi, yapının *paketlenmiş bir kopyasında* çalıştıkları için hiçbir şekilde görünür etkiye sahip olmaz:

```csharp
interface IB
{
    public void Increment() { P += 1; }
    public int P { get; set; }
}
struct T : IB
{
    public int P { get; set; } // auto-property
}

T t = default(T);
Console.WriteLine(t.P); // prints 0
(t as IB).Increment();
Console.WriteLine(t.P); // prints 0
```

> ***Kapatılan sorun:*** Bunun için neler yapabiliriz:
>
> 1. Bir `struct` varsayılan bir uygulamayı devralmayı yasaklaın. Tüm arabirim yöntemleri bir `struct`Özet olarak değerlendirilir. Daha sonra, daha sonra nasıl çalıştığını nasıl daha iyi hale getirebileceğine karar vereceğiz.
> 2. Kutulamayı önleyen bir tür kod oluşturma stratejisiyle birlikte gelir. `IB.Increment`gibi bir yöntemin içinde, `this` türü muhtemelen `IB`kısıtlı bir tür parametresine akmış olabilir. Bununla birlikte, çağıranın kutulamayı önlemek için, Özet olmayan yöntemler arabirimlerden devralınmalıdır. Bu, derleyici ve CLR uygulama çalışmasını önemli ölçüde artırabilir.
> 3. Bu konuda endişelenmeyin ve yalnızca bir Wart olarak bırakmayın.
> 4. Diğer fikirler?

***Karar:*** Bu konuda endişelenmeyin ve yalnızca bir Wart olarak bırakmayın. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#structs-and-default-implementations>.

### <a name="base-interface-invocations-closed"></a>Temel arabirim etkinleştirmeleri (kapalı)

Taslak belirtimi, Java tarafından önerilen temel arabirim etkinleştirmeleri için bir sözdizimi önerir: `Interface.base.M()`. En azından ilk prototip için bir sözdizimi seçmemiz gerekiyor. Sık kullanılanlardan `base<Interface>.M()`.

> ***Kapatılan sorun:*** Temel üye çağırma söz dizimi nedir?

***Karar:*** Söz dizimi `base(Interface).M()`. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>. Bu nedenle adlı arabirim bir temel arabirim olmalıdır, ancak doğrudan temel arabirim olması gerekmez.

> ***Açık sorun:*** Sınıf üyelerinde temel arabirim etkinleştirmeleri izin verilmelidir mi?

***Karar***: Evet. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md#base-invocation>

### <a name="overriding-non-public-interface-members-closed"></a>Genel olmayan arabirim üyelerini geçersiz kılma (kapalı)

Bir arabirimde, temel arabirimlerden ortak olmayan Üyeler `override` değiştiricisi kullanılarak geçersiz kılınır. Üyeyi içeren arabirime ad veren "açık" bir geçersiz kılma ise, erişim değiştiricisi atlanır.

> ***Kapatılan sorun:*** Arabirimi adı olmayan bir "örtük" geçersiz kılma ise, erişim değiştiricisi eşleşmelidir mi?

***Karar:*** Yalnızca ortak Üyeler örtük olarak geçersiz kılınabilir ve erişimin eşleşmesi gerekir. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

> ***Açık sorun:*** Erişim değiştiricisi gerekli, isteğe bağlı veya `override void IB.M() {}`gibi açık bir geçersiz kılma üzerinde atlanmış mi?

> ***Açık sorun:*** `override` gerekli, isteğe bağlı veya `void IB.M() {}`gibi açık bir geçersiz kılma üzerinde atlansın mı?

Bir sınıfta ortak olmayan bir arabirim üyesi nasıl uygulanır? Açıkça yapılması gerekir mi?

```csharp
interface IA
{
    internal void MI();
    protected void MP();
}
class C : IA
{
    // are these implementations?
    internal void MI() {}
    protected void MP() {}
}
```

> ***Kapatılan sorun:*** Bir sınıfta ortak olmayan bir arabirim üyesi nasıl uygulanır?

***Karar:*** Yalnızca ortak olmayan arabirim üyelerini açık bir şekilde uygulayabilirsiniz. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md#dim-implementing-a-non-public-interface-member-not-in-list>.

***Karar***: arabirim üyelerinde `override` anahtar sözcüğe izin verilmez. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>

### <a name="binary-compatibility-2-closed"></a>İkili uyumluluk 2 (kapalı)

Her türün ayrı bir derlemede olduğu aşağıdaki kodu göz önünde bulundurun

```csharp
interface I1
{
    void M() { Impl1 }
}
interface I2 : I1
{
    override void M() { Impl2 }
}
interface I3 : I1
{
}
class C : I2, I3
{
}
```

`C` `I1.M` uygulamasının `I2.M`olduğunu anladık. `I3` içeren derleme aşağıdaki gibi değiştirilirse ve yeniden derlendiğinden ne olur?

```csharp
interface I3 : I1
{
    override void M() { Impl3 }
}
```

Ancak `C` yeniden derlenmez. Program çalıştırıldığında ne olur? `(C as I1).M()` çağrısı

1. `I1.M` çalışır
2. `I2.M` çalışır
3. `I3.M` çalışır
4. 2 veya 3, belirleyici
5. Bir tür çalışma zamanı özel durumu oluşturur

***Karar***: bir özel durum (5) oluşturun. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#issues-in-default-interface-methods>.

### <a name="permit-partial-in-interface-closed"></a>Arabirimde `partial` izin veriyor musunuz? kapandı

Bu arabirimler, soyut sınıfların kullanılma yöntemine benzer şekilde kullanılabileceği için, `partial`bildirmek faydalı olabilir. Bu, özellikle de genel kullanım açısından yararlı olacaktır.

> ***Teklif:*** Arabirimlerin ve arabirimlerin üyelerinin `partial`bildirilmemiş olabileceği dil kısıtlamasını kaldırın.

***Karar***: Evet. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#permit-partial-in-interface>.

### <a name="main-in-an-interface-closed"></a>bir arabirimde `Main`? kapandı

> ***Açık sorun:*** Bir arabirimde programın giriş noktası olması için bir `static Main` yöntemi mi?

***Karar***: Evet. Bkz. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#main-in-an-interface>.

### <a name="confirm-intent-to-support-public-non-virtual-methods-closed"></a>Ortak sanal olmayan yöntemleri destekleme amacını onaylayın (kapalı)

Bir arabirimdeki sanal olmayan ortak yöntemlere izin vermek için lütfen kararmuzu onaylayın (veya ters çevirin)?

```csharp
interface IA
{
    public sealed void M() { }
}
```

> ***Yarı kapalı sorun:*** (2017-04-18) faydalı olacağını düşündük, ancak buna geri Dönecekiz. Bu, bir akıl modeli gidiş dönüşü bloğudur.

***Karar***: Evet. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#confirm-that-we-support-public-non-virtual-methods>.

### <a name="does-an-override-in-an-interface-introduce-a-new-member-closed"></a>Arabirimdeki bir `override` yeni bir üye sunuyor mu? kapandı

Geçersiz kılma bildiriminin yeni bir üye tanıtıp tanıtılmadığını gözlemlemek için birkaç yol vardır.

```csharp
interface IA
{
    void M(int x) { }
}
interface IB : IA
{
    override void M(int y) { }
}
interface IC : IB
{
    static void M2()
    {
        M(y: 3); // permitted?
    }
    override void IB.M(int z) { } // permitted? What does it override?
}
```

> ***Açık sorun:*** Arabirimdeki geçersiz kılma bildirimi yeni bir üye sunuyor mu? kapandı

Bir sınıfta, bir geçersiz kılma yöntemi bazı senslerde "görünür" olur. Örneğin, parametrelerinin adları geçersiz kılınan yöntemdeki parametrelerin adlarından önceliklidir. Her zaman en belirli bir geçersiz kılma olduğundan, bu davranışı arabirimlerde çoğaltmak mümkün olabilir. Ancak bu davranışı yinelemek istiyoruz musunuz?

Ayrıca, bir geçersiz kılma yöntemini "geçersiz kılmak" mümkündür. [Moot]

***Karar***: arabirim üyelerinde `override` anahtar sözcüğe izin verilmez. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#does-an-override-in-an-interface-introduce-a-new-member>.

### <a name="properties-with-a-private-accessor-closed"></a>Özel erişimcisi olan Özellikler (kapalı)

Özel üyelerin sanal olduğunu ve sanal ve özel ' in birleşimine izin verildiğimiz olduğunu varsayalım. Ancak özel bir erişimciye sahip bir özellik hakkında ne olacak?

```csharp
interface IA
{
    public virtual int P
    {
        get => 3;
        private set => { }
    }
}
```

Buna izin veriliyor mu? `set` erişimcisi burada `virtual` değil mi? Erişilebilir olduğunda geçersiz kılınabilir mi? Aşağıdakiler `get` erişimcisini örtük olarak uygular mi?

```csharp
class C : IA
{
    public int P
    {
        get => 4;
        set { }
    }
}
```

, IA nedeniyle aşağıdaki bir hata oluştu. P. set sanal değil ve erişilebilir durumda değil mi?

```csharp
class C : IA
{
    int IA.P
    {
        get => 4;
        set { }
    }
}
```

***Karar***: ilk örnek geçerli görünüyor, ancak en son bir örnektir. Bu, ' de C#zaten çalıştığı şekilde çözümlenir. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#properties-with-a-private-accessor>

### <a name="base-interface-invocations-round-2-closed"></a>Taban arabirim etkinleştirmeleri, Round 2 (kapalı)

Temel etkinleştirmeleri nasıl ele almak için önceki "çözümümüzü" Aslında yeterli ifade sağlanması gerekmez. Java 'dan farklı olarak, C# ve clr 'de bunu, yöntem bildirimini içeren arabirimi ve çağırmak istediğiniz uygulamanın konumunu belirtmeniz gerekir.

Arabirimlerde temel çağrılar için aşağıdaki sözdizimini önerdim. Bununla çok sevdim, ancak hangi sözdiziminin hızlı bir şekilde ifade edebileceklerini göstermektedir:

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I4 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>(I1).M(); // calls I3's implementation of I1.M
        base<I4>(I1).M(); // calls I4's implementation of I1.M
    }
    void I2.M()
    {
        base<I3>(I2).M(); // calls I3's implementation of I2.M
        base<I4>(I2).M(); // calls I4's implementation of I2.M
    }
}
```

Belirsizlik yoksa, bunu daha basit bir şekilde yazabilirsiniz

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I4 : I1 { void I1.M() { } }
interface I5 : I3, I4
{
    void I1.M()
    {
        base<I3>.M(); // calls I3's implementation of I1.M
        base<I4>.M(); // calls I4's implementation of I1.M
    }
}
```

Veya

```csharp
interface I1 { void M(); }
interface I2 { void M(); }
interface I3 : I1, I2 { void I1.M() { } void I2.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base(I1).M(); // calls I3's implementation of I1.M
    }
    void I2.M()
    {
        base(I2).M(); // calls I3's implementation of I2.M
    }
}
```

Veya

```csharp
interface I1 { void M(); }
interface I3 : I1 { void I1.M() { } }
interface I5 : I3
{
    void I1.M()
    {
        base.M(); // calls I3's implementation of I1.M
    }
}
```

***Karar***: `base(N.I1<T>).M(s)`karar verdi, daha sonra burada sorun olabileceğini belirten bir çağrı bağlama varsa bunu gizleme. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md#default-interface-implementations>

### <a name="warning-for-struct-not-implementing-default-method-closed"></a>Struct için bir uyarı varsayılan yöntemi uygulamamı? kapandı

@vancem, bir değer türü bildirimi bir arabirimden bu yöntemin bir uygulamasını devralmasını bile, bazı arabirim yöntemini geçersiz kılamazsa, bir uyarı üretmemiz gerektiğine dikkat edin. Çünkü kutulama ve az mayın kısıtlı çağrılara neden olur.

***Karar***: Bu, bir çözümleyici için daha uygun bir şey gibi görünüyor. Ayrıca, varsayılan arabirim yöntemi asla çağrılmasa ve hiçbir paketleme gerçekleşmeyeceğinden bile tetikleneceği için bu uyarının gürültülü olması gibi görünüyor. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#warning-for-struct-not-implementing-default-method>

### <a name="interface-static-constructors-closed"></a>Arabirim statik oluşturucuları (kapalı)

Arabirim statik oluşturucular ne zaman çalıştırılır?  Geçerli CLı taslağı, ilk statik yönteme veya alana erişildiğinde oluştuğunu önerir. Bunların hiçbiri yoksa, hiç çalıştırılmayabilir mi?

[2018-10-09 CLR ekibi, "değer türleri için yaptığımız şeyleri yansıtmaya (her örnek yöntemine erişim üzerinde cctor denetimi)"] öneriyor.

***Karar***: statik oluşturucular, örnek yöntemlerine giriş üzerinde de çalışır, statik Oluşturucu `beforefieldinit`, bu durumda statik oluşturucular ilk statik alana erişmeden önce çalıştırılır. <https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md#when-are-interface-static-constructors-run>

## <a name="design-meetings"></a>Tasarım toplantıları

[2017-03-08 LDM toplantısı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-08.md)
[2017-03-21 ldm](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-21.md) Meeting notları
[2017-03-23 toplantı "varsayılan arabirim yöntemleri için clr davranışı"](https://github.com/dotnet/csharplang/blob/master/meetings/2017/CLR-2017-03-23.md)
[2017-04-05 LDM meeting notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md)
[2017-04-11 LDM toplantısı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-11.md)
[2017-04-18 LDM](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-18.md) toplantısı [notları
2017-04-19 LDM toplantısı](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-19.md) notları
2017-05-17 LDM [toplantısı](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-17.md) [notları
2017-05-31](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md) LDM toplantısı notları
[2017-06-14 LDM Toplantı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-06-14.md)
[2018-10-17 LDM toplantı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
[2018-11-14 LDM toplantısı notları](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-11-14.md)
