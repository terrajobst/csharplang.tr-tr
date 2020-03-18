---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485016"
---
# <a name="efficient-params-and-string-formatting"></a>Verimli params ve String biçimlendirme

## <a name="summary"></a>Özet
Bu özellik birleşimi, biçimlendirme `string` değerlerinin ve `params` stili bağımsız değişkenlerinin geçirilme verimliliğini artırır.

## <a name="motivation"></a>Amacı
Biçimlendirme `string` değerlerinin ayırma ek yükü, birçok metin tabanlı uygulamanın performansını ayırt edebilir: `struct` türlerin paketleme cezası, `params` için `object[]` ayırması ve `string` çağrıları sırasında ara `string.Format` ayırmaları. Verimlilik sağlamak için genellikle `params` ve `string` ilişkilendirme gibi üretkenlik özelliklerini iptal etmek ve standart olmayan, el kodlu çözümlere taşımak gerekir. 

Örnek olarak MSBuild 'i göz önünde bulundurun. Bu, performansı bilinçli geliştiriciler tarafından çok sayıda C# modern özellik kullanılarak yazılmıştır. Henüz bir temsilcinin derlemesi örnek MSBuild, en az ayrıntı kullanarak 262MB `string` ayırma üretir. Ayırmaların 1/2 ' nin `string.Format`içindeki kısa süreli ayırmalar vardır. Bu özellikler, .NET Desktop ' ın çoğunu kaldırır ve `Span<T>` kullanılabilirliği nedeniyle .NET Core üzerinde neredeyse sıfır olarak alır

Burada açıklanan dil özellikleri kümesi, uygulamaların büyük bir olasılıkla uygulama kodu tabanında çok az veya hiç dalgalanmadan bu özellikleri kullanmaya devam etmesine imkan tanılarken, çoğu durumda istenmeyen ayırma ek yükünü ortadan kaldırır.

## <a name="detailed-design"></a>Ayrıntılı tasarım 
Bu sonuçlara ulaşmak için burada kullanılacak özellikler kümesi vardır:

- Daha geniş bir koleksiyon türleri kümesini desteklemek için `params` genişletiliyor.
- Geliştiricilerin `string` ilişkilendirmesinin nasıl elde edildiğini özelleştirmesine izin verme. 
- `string` enterpolasyonın daha verimli `string.Format` aşırı yüklemeye bağlanması için izin verme.

### <a name="extending-params"></a>Parametreleri genişletme
Dil, bir yöntem imzasında `params` `Span<T>`, `ReadOnlySpan<T>` ve `IEnumerable<T>`türlerine sahip olacak şekilde izin verir. Aynı çağırma kuralları `params T[]`için uygulanan bu yeni türler için de geçerlidir:

- Tek farkın `params` bir anahtar sözcük olduğu yerlerde aşırı yükleme yapılamıyor.
- , `T` veya tek bir `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` bağımsız değişkenine örtük olarak dönüştürülebilir bir dizi bağımsız değişken geçirerek çağırabilir.
- Metot imzasında son parametre olmalıdır.
- Vb... 

`Span<T>` ve `ReadOnlySpan<T>` türevlerini basitlik için aşağıda `Span<T>` olarak adlandırılacaktır. `ReadOnlySpan<T>` davranışının farklı olduğu durumlarda, açıkça bir şekilde çağrılacaktır. 

`params` `Span<T>` varyantlarından yararlanılması, derleyicinin `Span<T>` değeri için yedekleme depolama alanının nasıl ayırdığı konusunda büyük bir esneklik sağlar. `params T[]`, derleyici `params` yönteminin her çağrılması için yeni bir `T[]` ayırmalıdır. Çağıran tarafından saklanan ve parametresini kabul ettiğinden yeniden kullanım mümkün değildir. Bu, çok sayıda `params` çağırmaları olan yöntemlerde büyük bir inefficiency oluşmasına neden olabilir.

Verilen `Span<T>` çeşitleri `ref struct` aranan bağımsız değişkeni depolayamamalıdır. Bu nedenle derleyici, bağımsız değişkeni yeniden kullanma gibi eylemler gerçekleştirerek çağrı sitelerini iyileştirebilirler. Bu, `T[]`karşılaştırıldığında yinelenen çağırmaları çok verimli hale getirir. Bu dil, bu tür CallSites 'ın iyileştirilme konusunda belirli bir garanti vermez. Yalnızca derleyicinin bir `params Span<T>` yöntemi çağırırken `T[]` dışındaki değerleri kullanmak için boş olduğunu unutmayın. 

Bu tür bir olası uygulama aşağıda verilmiştir. Bir yöntem gövdesinde tüm `params` çağrılmasını göz önünde bulundurun. Derleyici en büyük `params` çağrısına eşit boyuta sahip bir dizi ayırabileceği gibi, dizi üzerinde uygun boyutta `Span<T>` örnekleri oluşturarak bu işlemi tüm etkinleştirmeleri için kullanabilirsiniz. Örneğin:

``` csharp
static class OneAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("jaredpar");
        Use("hello", "world");
        Use("a", "longer", "set");
    }
}
```

Derleyici `Go` gövdesini aşağıdaki gibi yayabilir:

``` csharp
    static void Go() {
        var args = new string[3];
        args[0] = "jaredpar";
        Use(new Span<string>(args, start: 0, length: 1));

        args[0] = "hello";
        args[1] = "world";
        Use(new Span<string>(args, start: 0, length: 2));

        args[0] = "a";
        args[1] = "longer";
        args[2] = "set";
        Use(new Span<string>(args, start: 0, length: 3));
   }
```

Bu, bir uygulamada ayrılan dizi sayısını önemli ölçüde azaltabilir. Çalışma zamanı, dizilerin daha akıllı yığın ayırması için yardımcı programlar sağlıyorsa, ayırmalar daha da azalabilir.

Bu iyileştirme, ancak her zaman uygulanamaz. Aranan `params` bağımsız değişkenini yakalayamaz olsa da, bir `ref` veya kendisi `ref struct` bir `out / ref` parametresi varsa, bu, çağıran içinde yine de yakalanamaz. 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

Bu durumlar, ancak statik olarak algılanabilir. Bir `ref` Return veya `out` ya da `ref`ile geçirilmiş bir `ref struct` parametresi olduğunda bu durum oluşabilir. Böyle bir durumda, derleyicinin her çağırma için yeni bir `T[]` ayırması gerekir. 

Bu belgenin sonunda birkaç olası iyileştirme stratejisi ele alınmıştır.

`IEnumerable<T>` Variant yalnızca kolaylık sağlaması olan bir aşırı yüktür. `IEnumerable<T>` sık kullanılan ve ayrıca çok sayıda `params` kullanımı olan senaryolarda faydalıdır. `T` bağımsız değişken biçiminde çağrıldığında, yedekleme depolama `params T[]` bugün yapıldığı gibi `T[]` olarak tahsis edilir.

### <a name="params-overload-resolution-changes"></a>params aşırı yüklemesi çözüm değişiklikleri
Bu teklif, dilin bundan önce `params` dört çeşitinin bulunduğu anlamına gelir. Yalnızca bir `params` bildirimlerinin türü üzerinde farklılık gösteren yöntemlerin aşırı yüklerini tanımlama yöntemleri için de erişilebilir. 

`StringBuilder.AppendFormat` `params object[]`ek olarak `params ReadOnlySpan<object>` aşırı yükleme ekleneceğini göz önünde bulundurun. Bu, çağıran kodda herhangi bir değişiklik gerektirmeden koleksiyon ayırmalarını azaltarak performansı önemli ölçüde iyileştirmenize olanak tanır. 

Bu, bu dilin kolaylaştırmasını sağlamak için, aşağıdaki aşırı yükleme çözümleme bağlama kuralı 'nı tanıtacaktır. Aday yöntemleri yalnızca `params` parametresi tarafından farklılık gösterdiği zaman, adaylar aşağıdaki sırada tercih edilir:

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

Bu sıra, genel durum için en az verimlidir.

### <a name="variant"></a>Varyantı
CoreFX, [VARIANT](https://github.com/dotnet/corefxlab/pull/2595)adlı yeni bir yönetilen tür prototipi oluşturma. Bu tür, heterojen değerler bekleyen ancak ek yükün parametre olarak `object` kullanarak kullanılmasını istemediğiniz API 'lerde kullanılmak üzere tasarlanmıştır. `Variant` türü evrensel depolama sağlar ancak en sık kullanılan türler için kutulama ayırmayı önler. `string.Format` gibi API 'lerde bu tür kullanmak, çoğu durumda paketleme yükünü ortadan kaldırabilir.

Bu türün kendisi dile özel olması gerekmez. Teklifin diğer bölümlerinin uygulama ayrıntısı haline geldiği halde bu belgede ayrı olarak tanıtılmaktadır. 

### <a name="efficient-interpolated-strings"></a>Etkin enterpolasyonlu dizeler
Enterpolasyonlu dizeler ' de C#popüler, ancak verimsiz bir özelliktir. `string`olarak enterpolasyonlu bir `string` kullanan en yaygın sözdizimi, `string.Format(string, params object[])` çağrısına çevrilir. Bu, tüm değer türleri için paketleme ayırmaları oluşturacak, uygulama büyük ölçüde biçimlendirme için `object.ToString` ve bağımsız değişken sayısı `string.Format`"hızlı" aşırı yükündeki parametre miktarını aştığında dizi ayırmaları) için, ara `string` ayırmaları kullanır. 

Dil, `string.Format`alternatif yüklerini göz önünde bulundurmanız için enterpolasyon azaltmayı değiştirecek. Tüm `string.Format(string, params)` biçimlerini göz önünde bulunduracaktır ve bağımsız değişken türlerini karşılayan "en iyi" aşırı yüklemeyi seçer.
"En iyi" `params` aşırı yüklemesi yukarıda açıklanan kurallara göre belirlenir. Bu `string`, artık `string.Format(string format, params ReadOnlySpan<Variant> args)`gibi çok verimli aşırı yüklemelerin bağlanacağı anlamına gelir. Çoğu durumda bu, tüm ara ayırmaları kaldırır.

### <a name="customizable-interpolated-strings"></a>Özelleştirilebilir enterpolasyonlu dizeler
Geliştiriciler, `FormattableString`ile enterpolasyonlu dizelerin davranışını özelleştirebilir. Bu, bir enterpolasyonlu dizeye giden verileri içerir: biçim `string` ve bağımsız değişkenler dizi olarak. Bu, yine de paketleme ve bağımsız değişken dizisi tahsisine ve `FormattableString` ayırmasının yanı sıra (bir `abstract class`). Bu nedenle, `string` biçimlendirme sırasında ayırma ağır olan uygulamalar için çok az kullanılır.

Enterpolasyonlu dize biçimlendirmesini verimli hale getirmek için dilin yeni bir tür tanıması gerekir: `System.ValueFormattableString`. Tüm enterpolasyonlu dizelerin bu türe bir hedef tür dönüştürmesi olacaktır. Bu, şu anda `FormattableString.Create` için, enterpolasyonlu dize çağrı `ValueFormattableString.Create` çağırarak uygulanır. Dil, en uygun `ValueFormattableString.Create` yöntemini ararken, bu belgede açıklanan tüm `params` seçeneklerini destekler. 

``` csharp
readonly struct ValueFormattableString {
    public static ValueFormattableString Create(Variant v) { ... } 
    public static ValueFormattableString Create(string s) { ... } 
    public static ValueFormattableString Create(string s, params ReadOnlySpan<Variant> collection) { ... } 
}

class ConsoleEx { 
    static void Write(ValueFormattableString f) { ... }
}

class Program { 
    static void Main() { 
        ConsoleEx.Write(42);
        ConsoleEx.Write($"hello {DateTime.UtcNow}");

        // Translates into 
        ConsoleEx.Write(ValueFormattableString.Create((Variant)42));
        ConsoleEx.Write(ValueFormattableString.Create(
            "hello {0}", 
            new Variant(DateTime.UtcNow));
    }
}
```

Bağımsız değişken bir enterpolasyonlu dize olduğunda `string` üzerinde `ValueFormattableString` tercih etmek için aşırı yükleme çözümleme kuralları değiştirilir. Bu, yalnızca `string` ve `ValueFormattableString`farklı olan aşırı yüklemeleri sağlamak için değerli olacağı anlamına gelir. Bu tür bir aşırı yükleme `FormattableString`, derleyici `string` sürümünü her zaman tercih ettiği için (Geliştirici açık bir atama kullanmadığı sürece) değerli değildir. 

## <a name="open-issues"></a>Açık sorunlar

### <a name="valueformattablestring-breaking-change"></a>ValueFormattableString bölünmesi değişikliği
`string` üzerinde aşırı yükleme çözümlemesi sırasında `ValueFormattableString` tercih etmek için yapılan değişiklik, bir son değişiklik olur. Bir geliştiricinin bugün `ValueFormattableString` adlı bir tür tanımlanması ve bunu `string`yöntem aşırı yüklemeleri içinde kullanabilmesi mümkündür. Bu önerilen değişiklik, bu özellik kümesi uygulandıktan sonra derleyicinin farklı bir aşırı yükleme seçmesini sağlar. 

Bunun olasılığı makul bir şekilde düşüktür. Türün tam adı `System.ValueFormattableString` gerekir ve `Create`adında `static` yöntemlere sahip olması gerekir. Geliştiricilerin `System` ad alanında herhangi bir tür tanımlamaktan kesinlikle önerilmez. bu kesme makul bir uzlaşmaya benzer şekilde görünür.

### <a name="expanding-to-more-types"></a>Daha fazla türe genişletme
Verilen alanda, `params` desteklendiği koleksiyonlar kümesine `IList<T>`, `ICollection<T>` ve `IReadOnlyList<T>` eklemeyi düşünmemiz gerekir. Uygulama açısından buradaki diğer işler üzerinde küçük miktarda ücret alınacaktır.

Bu dile daha karmaşıkmaya değip değmeyeceğine karar vermek için LDM gerekir. `IEnumerable<T>` eklenmesi, çok özel bir uyuşmazlıkları noktasını kaldırır. Bu `params` çözümü çok sayıda müşteri, bir `params` yöntemi çağırırken bir `IEnumerable<T>` `T[]` ayırmaya zorlandı. `IEnumerable<T>` eklenmesi bu da bunu düzeltir. Diğer arabirimlerin burada çözmesine yönelik belirli bir uçuşme noktası yoktur. 

## <a name="considerations"></a>Dikkat edilmesi gerekenler

### <a name="variant2-and-variant3"></a>Variant2 ve Variant3
CoreFX ekibinin, en fazla üç `Variant` bağımsız değişkeni için ayırma olmayan bir depolama türleri kümesi de vardır. Bunlar tek bir `Variant`, `Variant2` ve `Variant3`. Her birinin, ayırma ücretsiz `Span<Variant>` almak için bir çift yöntemi vardır: `CreateSpan` ve `KeepAlive`. Bu, çağrı sitesinin tamamen serbest ayrılabileceği en fazla üç bağımsız değişkeni `params Span<Variant>` anlamına gelir.

``` csharp
static class ZeroAllocation {
    static void Use(params Span<Variant> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` yöntemi aşağıdakilere indirgenmelidir:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Variant2 _v;
        _v.Variant1 = new Variant("hello");
        _v.Variant2 = new Variant("word");
        Use(_v.CreateSpan());
        _v.KeepAlive();
    }
}
```

Bu, `params Span<T>` çağrıları arasında `T[]` yeniden kullanmak için teklifin üzerine çok az iş gerektirir. Derleyicinin zaten geçici olarak geçici bir çağrı yönetmesi ve işi temizlemesi gerekir (bir durumda bir iç geçici olarak bir iç geçici olarak işaretlebilse bile). 

Note: `KeepAlive` işlevi yalnızca masaüstünüzde gereklidir. .NET Core 'da yöntem kullanılabilir olmayacaktır ve bu nedenle derleyici buna bir çağrı görmez.

### <a name="clr-stack-allocation-helpers"></a>CLR Stack ayırma yardımcıları
CLR yalnızca, bitişik belleğin yığın ayırması için yalnızca [güncelleştirilemez](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) sağlar. Bu yönerge yalnızca `unmanaged` türlerinde çalışacak şekilde sınırlıdır. Bu, `params 
 Span<T>`için yedekleme depolama alanını verimli bir şekilde ayırmak üzere bir evrensel çözüm olarak kullanılamayan anlamına gelir. 

Bu sınırlama bazı temel kısıtlamalar değildir, ancak bunun yerine daha fazla bir geçmiş vardır. CLR, evrensel yığın ayırması sağlayan yeni işlem kodları/iç bilgileri eklemeyi seçebilir. Bunlar daha sonra çoğu `params Span<T>` çağrısı için yedekleme depolama alanı ayırmak üzere kullanılabilir.

``` csharp
static class BetterAllocation {
    static void Use(params Span<string> spans) {
        ...
    }

    static void Go() {
        Use("hello", "world");
    }
}
```

`Go` yöntemi aşağıdakilere indirgenmelidir:

``` csharp
static class ZeroAllocation {
    static void Go() {
        Span<T> span = RuntimeIntrinsic.StackAlloc<string>(length: 2);
        span[0] = "hello";
        span[1] = "world";
        Use(span);
    }
}
```

Bu yaklaşım çok yığın verimli olsa da ek yığın kullanımına neden olur. Derin bir yığını ve çok sayıda `params` kullanımı olan bir algoritmada, basit bir `T[]` ayırmanın başarılı olacağı bir `StackOverflowException` oluşturulmasına neden olabilir. 

Ne C# yazık ki, çağrının `params`yığın veya yığın ayırmayı kullanması gerekip gerekmediğini bir şekilde belirleme konusunda bir eğitime yöntemi için ayarlanmıyor. Her çağrıyı yalnızca kendi kendine kabul edebilir.

CLR, çalışma zamanında bu tür bir belirleme yapmak için en iyi kurulumdır. Bu nedenle, çalışma zamanının evrensel yığın ayırma için iki yöntem sağlaması olasıdır:

1. `Span<T> StackAlloc<T>(int length)`: Bu, `T`herhangi bir tür üzerinde çalışmaları hariç `localloc` aynı davranış ve sınırlamalara sahiptir. 
1. `Span<T> MaybeStackAlloc<T>(int length)`: Bu çalışma zamanı, yığın veya yığın ayırmayı gerçekleştirerek bunu uygulamayı seçebilir. Çalışma zamanı daha sonra, `Span<T>` nasıl ayrılacağını anlamak için çağrıldığı Yürütme bağlamını kullanabilir. Çağıran, yığın ayrılmış gibi her zaman kabul eder.

Biri iki bağımsız değişken gibi basit durumlarda, C# derleyici her zaman `StackAlloc<T>` değişken kullanabilir. Çoğu durumda yığın tükenmesi için önemli ölçüde katkıda bulunmak çok düşüktür. Diğer durumlarda derleyici `MaybeStackAlloc<T>` kullanmayı seçebilir ve çalışma zamanının çağrıyı yapmasına izin verebilir.

Nasıl seçtiğimiz, büyük olasılıkla gerçek dünya uygulamalarının daha ayrıntılı bir şekilde araştırılması ve incelenmesi gerekir. Ancak bu yeni iç bilgiler varsa bize bu tür bir esneklik verecektir.

### <a name="why-not-varargs"></a>Neden varargs değil? 
Mevcut [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) özelliği, olası bir çözüm olarak burada kabul edildi. Bu özellik, öncelikli olarak C++/CLI senaryolarına yöneliktir ve diğer senaryolar için bilinen delikleri vardır. Ayrıca, bunu UNIX 'e taşıma konusunda önemli bir maliyet de vardır. Bu nedenle, uygulanabilir bir çözüm olarak görülmedi.

## <a name="related-issues"></a>İlgili sorunlar
Bu belirtim aşağıdaki sorunlarla ilgilidir: 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

