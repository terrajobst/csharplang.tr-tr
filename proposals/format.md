---
ms.openlocfilehash: 1457c1eb018e12af30ce5b38be704bf8851d4b25
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "79485016"
---
# <a name="efficient-params-and-string-formatting"></a><span data-ttu-id="3d7c7-101">Verimli params ve String biçimlendirme</span><span class="sxs-lookup"><span data-stu-id="3d7c7-101">Efficient Params and String Formatting</span></span>

## <a name="summary"></a><span data-ttu-id="3d7c7-102">Özet</span><span class="sxs-lookup"><span data-stu-id="3d7c7-102">Summary</span></span>
<span data-ttu-id="3d7c7-103">Bu özellik birleşimi, biçimlendirme `string` değerlerinin ve `params` stili bağımsız değişkenlerinin geçirilme verimliliğini artırır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-103">This combination of features will increase the efficiency of formatting `string` values and passing of `params` style arguments.</span></span>

## <a name="motivation"></a><span data-ttu-id="3d7c7-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="3d7c7-104">Motivation</span></span>
<span data-ttu-id="3d7c7-105">Biçimlendirme `string` değerlerinin ayırma ek yükü, birçok metin tabanlı uygulamanın performansını ayırt edebilir: `struct` türlerin paketleme cezası, `params` için `object[]` ayırması ve `string` çağrıları sırasında ara `string.Format` ayırmaları.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-105">The allocation overhead of formatting `string` values can dominate the performance of many text based applications: from the boxing penalty of `struct` types, the `object[]` allocation for `params` and the intermediate `string` allocations during `string.Format` calls.</span></span> <span data-ttu-id="3d7c7-106">Verimlilik sağlamak için genellikle `params` ve `string` ilişkilendirme gibi üretkenlik özelliklerini iptal etmek ve standart olmayan, el kodlu çözümlere taşımak gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-106">In order to maintain efficiency such applications often need to abandon productivity features such as `params` and `string` interpolation and move to non-standard, hand coded solutions.</span></span> 

<span data-ttu-id="3d7c7-107">Örnek olarak MSBuild 'i göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-107">Consider MSBuild as an example.</span></span> <span data-ttu-id="3d7c7-108">Bu, performansı bilinçli geliştiriciler tarafından çok sayıda C# modern özellik kullanılarak yazılmıştır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-108">This is written using a lot of modern C# features by developers who are conscious of performance.</span></span> <span data-ttu-id="3d7c7-109">Henüz bir temsilcinin derlemesi örnek MSBuild, en az ayrıntı kullanarak 262MB `string` ayırma üretir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-109">Yet in one representative build sample MSBuild will generate 262MB of `string` allocation using minimal verbosity.</span></span> <span data-ttu-id="3d7c7-110">Ayırmaların 1/2 ' nin `string.Format`içindeki kısa süreli ayırmalar vardır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-110">Of that 1/2 of the allocations are short lived allocations inside `string.Format`.</span></span> <span data-ttu-id="3d7c7-111">Bu özellikler, .NET Desktop ' ın çoğunu kaldırır ve `Span<T>` kullanılabilirliği nedeniyle .NET Core üzerinde neredeyse sıfır olarak alır</span><span class="sxs-lookup"><span data-stu-id="3d7c7-111">These features would remove much of that on .NET Desktop and get it down to nearly zero on .NET Core due to the availability of `Span<T>`</span></span>

<span data-ttu-id="3d7c7-112">Burada açıklanan dil özellikleri kümesi, uygulamaların büyük bir olasılıkla uygulama kodu tabanında çok az veya hiç dalgalanmadan bu özellikleri kullanmaya devam etmesine imkan tanılarken, çoğu durumda istenmeyen ayırma ek yükünü ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-112">The set of language features described here will enable applications to continue using these features, with very little or no churn to their application code base, while removing the unintended allocation overhead in the majority of cases.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="3d7c7-113">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="3d7c7-113">Detailed Design</span></span> 
<span data-ttu-id="3d7c7-114">Bu sonuçlara ulaşmak için burada kullanılacak özellikler kümesi vardır:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-114">There are a set of features that will be used here to achieve these results:</span></span>

- <span data-ttu-id="3d7c7-115">Daha geniş bir koleksiyon türleri kümesini desteklemek için `params` genişletiliyor.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-115">Expanding `params` to support a broader set of collection types.</span></span>
- <span data-ttu-id="3d7c7-116">Geliştiricilerin `string` ilişkilendirmesinin nasıl elde edildiğini özelleştirmesine izin verme.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-116">Allowing for developers to customize how `string` interpolation is achieved.</span></span> 
- <span data-ttu-id="3d7c7-117">`string` enterpolasyonın daha verimli `string.Format` aşırı yüklemeye bağlanması için izin verme.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-117">Allowing for interpolated `string` to bind to more efficient `string.Format` overloads.</span></span>

### <a name="extending-params"></a><span data-ttu-id="3d7c7-118">Parametreleri genişletme</span><span class="sxs-lookup"><span data-stu-id="3d7c7-118">Extending params</span></span>
<span data-ttu-id="3d7c7-119">Dil, bir yöntem imzasında `params` `Span<T>`, `ReadOnlySpan<T>` ve `IEnumerable<T>`türlerine sahip olacak şekilde izin verir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-119">The language will allow for `params` in a method signature to have the types `Span<T>`, `ReadOnlySpan<T>` and `IEnumerable<T>`.</span></span> <span data-ttu-id="3d7c7-120">Aynı çağırma kuralları `params T[]`için uygulanan bu yeni türler için de geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-120">The same rules for invocation will apply to these new types that apply to `params T[]`:</span></span>

- <span data-ttu-id="3d7c7-121">Tek farkın `params` bir anahtar sözcük olduğu yerlerde aşırı yükleme yapılamıyor.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-121">Can't overload where the only difference is a `params` keyword.</span></span>
- <span data-ttu-id="3d7c7-122">, `T` veya tek bir `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` bağımsız değişkenine örtük olarak dönüştürülebilir bir dizi bağımsız değişken geçirerek çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-122">Can invoke by passing a series of arguments that are implicitly convertible to `T` or a single `Span<T>` / 
`ReadOnlySpan<T>` / `IEnumerable<T>` argument.</span></span>
- <span data-ttu-id="3d7c7-123">Metot imzasında son parametre olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-123">Must be the last parameter in a method signature.</span></span>
- <span data-ttu-id="3d7c7-124">Vb...</span><span class="sxs-lookup"><span data-stu-id="3d7c7-124">Etc ...</span></span> 

<span data-ttu-id="3d7c7-125">`Span<T>` ve `ReadOnlySpan<T>` türevlerini basitlik için aşağıda `Span<T>` olarak adlandırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-125">The `Span<T>` and `ReadOnlySpan<T>` variants will be referred to as `Span<T>` below for simplicity.</span></span> <span data-ttu-id="3d7c7-126">`ReadOnlySpan<T>` davranışının farklı olduğu durumlarda, açıkça bir şekilde çağrılacaktır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-126">In cases where the behavior of `ReadOnlySpan<T>` differs it will be explicitly called out.</span></span> 

<span data-ttu-id="3d7c7-127">`params` `Span<T>` varyantlarından yararlanılması, derleyicinin `Span<T>` değeri için yedekleme depolama alanının nasıl ayırdığı konusunda büyük bir esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-127">The advantage the `Span<T>` variants of `params` provides is it gives the compiler great flexibility in how it allocates the backing storage for the `Span<T>` value.</span></span> <span data-ttu-id="3d7c7-128">`params T[]`, derleyici `params` yönteminin her çağrılması için yeni bir `T[]` ayırmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-128">With a `params T[]` the compiler must allocate a new `T[]` for every invocation of a `params` method.</span></span> <span data-ttu-id="3d7c7-129">Çağıran tarafından saklanan ve parametresini kabul ettiğinden yeniden kullanım mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-129">Re-use is not possible because it must assume the callee stored and reused the parameter.</span></span> <span data-ttu-id="3d7c7-130">Bu, çok sayıda `params` çağırmaları olan yöntemlerde büyük bir inefficiency oluşmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-130">This can lead to a large inefficiency in methods with lots of `params` invocations.</span></span>

<span data-ttu-id="3d7c7-131">Verilen `Span<T>` çeşitleri `ref struct` aranan bağımsız değişkeni depolayamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-131">Given `Span<T>` variants are `ref struct` the callee cannot store the argument.</span></span> <span data-ttu-id="3d7c7-132">Bu nedenle derleyici, bağımsız değişkeni yeniden kullanma gibi eylemler gerçekleştirerek çağrı sitelerini iyileştirebilirler.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-132">Hence the compiler can optimize the call sites by taking actions like re-using the argument.</span></span> <span data-ttu-id="3d7c7-133">Bu, `T[]`karşılaştırıldığında yinelenen çağırmaları çok verimli hale getirir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-133">This can make repeated invocations very efficient as compared to `T[]`.</span></span> <span data-ttu-id="3d7c7-134">Bu dil, bu tür CallSites 'ın iyileştirilme konusunda belirli bir garanti vermez.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-134">The language though will make no specific guarantees about how such callsites are optimized.</span></span> <span data-ttu-id="3d7c7-135">Yalnızca derleyicinin bir `params Span<T>` yöntemi çağırırken `T[]` dışındaki değerleri kullanmak için boş olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-135">Only note that the compiler is free to use values other than `T[]` when invoking a `params Span<T>` method.</span></span> 

<span data-ttu-id="3d7c7-136">Bu tür bir olası uygulama aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-136">One such potential implementation is the following.</span></span> <span data-ttu-id="3d7c7-137">Bir yöntem gövdesinde tüm `params` çağrılmasını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-137">Consider all `params` invocation in a method body.</span></span> <span data-ttu-id="3d7c7-138">Derleyici en büyük `params` çağrısına eşit boyuta sahip bir dizi ayırabileceği gibi, dizi üzerinde uygun boyutta `Span<T>` örnekleri oluşturarak bu işlemi tüm etkinleştirmeleri için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-138">The compiler could allocate an array which has a size equal to the largest `params` invocation and use that for all of the invocations by creating appropriately sized `Span<T>` instances over the array.</span></span> <span data-ttu-id="3d7c7-139">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-139">For example:</span></span>

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

<span data-ttu-id="3d7c7-140">Derleyici `Go` gövdesini aşağıdaki gibi yayabilir:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-140">The compiler could choose to emit the body of `Go` as follows:</span></span>

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

<span data-ttu-id="3d7c7-141">Bu, bir uygulamada ayrılan dizi sayısını önemli ölçüde azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-141">This can significantly reduce the number of arrays allocated in an application.</span></span> <span data-ttu-id="3d7c7-142">Çalışma zamanı, dizilerin daha akıllı yığın ayırması için yardımcı programlar sağlıyorsa, ayırmalar daha da azalabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-142">Allocations can be even further reduced if the runtime provides utilities for smarter stack allocation of arrays.</span></span>

<span data-ttu-id="3d7c7-143">Bu iyileştirme, ancak her zaman uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-143">This optimization cannot always be applied though.</span></span> <span data-ttu-id="3d7c7-144">Aranan `params` bağımsız değişkenini yakalayamaz olsa da, bir `ref` veya kendisi `ref struct` bir `out / ref` parametresi varsa, bu, çağıran içinde yine de yakalanamaz.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-144">Even though the callee cannot capture the `params` argument it can still be captured in the caller when there is a `ref` or a `out / ref` parameter that is itself a `ref struct` type.</span></span> 

``` csharp
static class SneakyCapture {
    static ref int M(params Span<T> span) => ref span[0];

    static void Oops() {
        // This now holds onto the memory backing the Span<T> 
        ref int r = ref M(42);
    }
}
```

<span data-ttu-id="3d7c7-145">Bu durumlar, ancak statik olarak algılanabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-145">These cases are statically detectable though.</span></span> <span data-ttu-id="3d7c7-146">Bir `ref` Return veya `out` ya da `ref`ile geçirilmiş bir `ref struct` parametresi olduğunda bu durum oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-146">It potentially occurs whenever there is a `ref` return or a `ref struct` parameter passed by `out` or `ref`.</span></span> <span data-ttu-id="3d7c7-147">Böyle bir durumda, derleyicinin her çağırma için yeni bir `T[]` ayırması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-147">In such a case the compiler must allocate a fresh `T[]` for every invocation.</span></span> 

<span data-ttu-id="3d7c7-148">Bu belgenin sonunda birkaç olası iyileştirme stratejisi ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-148">Several other potential optimization strategies are discussed at the end of this document.</span></span>

<span data-ttu-id="3d7c7-149">`IEnumerable<T>` Variant yalnızca kolaylık sağlaması olan bir aşırı yüktür.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-149">The `IEnumerable<T>` variant is a merely a convenience overload.</span></span> <span data-ttu-id="3d7c7-150">`IEnumerable<T>` sık kullanılan ve ayrıca çok sayıda `params` kullanımı olan senaryolarda faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-150">It's useful in scenarios which have frequent uses of `IEnumerable<T>` but also have lots of `params` usage.</span></span> <span data-ttu-id="3d7c7-151">`T` bağımsız değişken biçiminde çağrıldığında, yedekleme depolama `params T[]` bugün yapıldığı gibi `T[]` olarak tahsis edilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-151">When invoked in `T` argument form the backing storage will be allocated as a `T[]` just as `params T[]` is done today.</span></span>

### <a name="params-overload-resolution-changes"></a><span data-ttu-id="3d7c7-152">params aşırı yüklemesi çözüm değişiklikleri</span><span class="sxs-lookup"><span data-stu-id="3d7c7-152">params overload resolution changes</span></span>
<span data-ttu-id="3d7c7-153">Bu teklif, dilin bundan önce `params` dört çeşitinin bulunduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-153">This proposal means the language now has four variants of `params` where before it had one.</span></span> <span data-ttu-id="3d7c7-154">Yalnızca bir `params` bildirimlerinin türü üzerinde farklılık gösteren yöntemlerin aşırı yüklerini tanımlama yöntemleri için de erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-154">It is sensible for methods to define overloads of methods that differ only on the type of a `params` declarations.</span></span> 

<span data-ttu-id="3d7c7-155">`StringBuilder.AppendFormat` `params object[]`ek olarak `params ReadOnlySpan<object>` aşırı yükleme ekleneceğini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-155">Consider that `StringBuilder.AppendFormat` would certainly add a `params ReadOnlySpan<object>` overload in addition to the `params object[]`.</span></span> <span data-ttu-id="3d7c7-156">Bu, çağıran kodda herhangi bir değişiklik gerektirmeden koleksiyon ayırmalarını azaltarak performansı önemli ölçüde iyileştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-156">This would allow it to substantially improve performance by reducing collection allocations without requiring any changes to the calling code.</span></span> 

<span data-ttu-id="3d7c7-157">Bu, bu dilin kolaylaştırmasını sağlamak için, aşağıdaki aşırı yükleme çözümleme bağlama kuralı 'nı tanıtacaktır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-157">To facilitate this the language will introduce the following overload resolution tie breaking rule.</span></span> <span data-ttu-id="3d7c7-158">Aday yöntemleri yalnızca `params` parametresi tarafından farklılık gösterdiği zaman, adaylar aşağıdaki sırada tercih edilir:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-158">When the candidate methods differ only by the `params` parameter then the candidates will be preferred in the following order:</span></span>

1. `ReadOnlySpan<T>`
1. `Span<T>`
1. `T[]`
1. `IEnumerable<T>`

<span data-ttu-id="3d7c7-159">Bu sıra, genel durum için en az verimlidir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-159">This order is the most to the least efficient for the general case.</span></span>

### <a name="variant"></a><span data-ttu-id="3d7c7-160">Varyantı</span><span class="sxs-lookup"><span data-stu-id="3d7c7-160">Variant</span></span>
<span data-ttu-id="3d7c7-161">CoreFX, [VARIANT](https://github.com/dotnet/corefxlab/pull/2595)adlı yeni bir yönetilen tür prototipi oluşturma.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-161">CoreFX is prototyping a new managed type named [Variant](https://github.com/dotnet/corefxlab/pull/2595).</span></span> <span data-ttu-id="3d7c7-162">Bu tür, heterojen değerler bekleyen ancak ek yükün parametre olarak `object` kullanarak kullanılmasını istemediğiniz API 'lerde kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-162">This type is meant to be used in APIs which expect heterogeneous values but don't want the overhead brought on by using `object` as the parameter.</span></span> <span data-ttu-id="3d7c7-163">`Variant` türü evrensel depolama sağlar ancak en sık kullanılan türler için kutulama ayırmayı önler.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-163">The `Variant` type provides universal storage but avoids the boxing allocation for the most commonly used types.</span></span> <span data-ttu-id="3d7c7-164">`string.Format` gibi API 'lerde bu tür kullanmak, çoğu durumda paketleme yükünü ortadan kaldırabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-164">Using this type in APIs like `string.Format` can eliminate the boxing overhead in the majority of cases.</span></span>

<span data-ttu-id="3d7c7-165">Bu türün kendisi dile özel olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-165">This type itself is not necessarily special to the language.</span></span> <span data-ttu-id="3d7c7-166">Teklifin diğer bölümlerinin uygulama ayrıntısı haline geldiği halde bu belgede ayrı olarak tanıtılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-166">It is being introduced in this document separately though as it becomes an implementation detail of other parts of the proposal.</span></span> 

### <a name="efficient-interpolated-strings"></a><span data-ttu-id="3d7c7-167">Etkin enterpolasyonlu dizeler</span><span class="sxs-lookup"><span data-stu-id="3d7c7-167">Efficient interpolated strings</span></span>
<span data-ttu-id="3d7c7-168">Enterpolasyonlu dizeler ' de C#popüler, ancak verimsiz bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-168">Interpolated strings are a popular yet inefficient feature in C#.</span></span> <span data-ttu-id="3d7c7-169">`string`olarak enterpolasyonlu bir `string` kullanan en yaygın sözdizimi, `string.Format(string, params object[])` çağrısına çevrilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-169">The most common syntax, using an interpolated `string` as a `string`, translates into a `string.Format(string, params object[])` call.</span></span> <span data-ttu-id="3d7c7-170">Bu, tüm değer türleri için paketleme ayırmaları oluşturacak, uygulama büyük ölçüde biçimlendirme için `object.ToString` ve bağımsız değişken sayısı `string.Format`"hızlı" aşırı yükündeki parametre miktarını aştığında dizi ayırmaları) için, ara `string` ayırmaları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-170">That will incur boxing allocations for all value types, intermediate `string` allocations as the implementation largely uses `object.ToString` for formatting as well as array allocations once the number of arguments exceeds the amount of parameters on the "fast" overloads of `string.Format`.</span></span> 

<span data-ttu-id="3d7c7-171">Dil, `string.Format`alternatif yüklerini göz önünde bulundurmanız için enterpolasyon azaltmayı değiştirecek.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-171">The language will change its interpolation lowering to consider alternate overloads of `string.Format`.</span></span> <span data-ttu-id="3d7c7-172">Tüm `string.Format(string, params)` biçimlerini göz önünde bulunduracaktır ve bağımsız değişken türlerini karşılayan "en iyi" aşırı yüklemeyi seçer.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-172">It will consider all forms of `string.Format(string, params)` and pick the "best" overload which satisfies the argument types.</span></span>
<span data-ttu-id="3d7c7-173">"En iyi" `params` aşırı yüklemesi yukarıda açıklanan kurallara göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-173">The "best" `params` overload will be determined by the rules discussed above.</span></span> <span data-ttu-id="3d7c7-174">Bu `string`, artık `string.Format(string format, params ReadOnlySpan<Variant> args)`gibi çok verimli aşırı yüklemelerin bağlanacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-174">This means interpolated `string` can now bind to very efficient overloads like `string.Format(string format, params ReadOnlySpan<Variant> args)`.</span></span> <span data-ttu-id="3d7c7-175">Çoğu durumda bu, tüm ara ayırmaları kaldırır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-175">In many cases this will remove all intermediate allocations.</span></span>

### <a name="customizable-interpolated-strings"></a><span data-ttu-id="3d7c7-176">Özelleştirilebilir enterpolasyonlu dizeler</span><span class="sxs-lookup"><span data-stu-id="3d7c7-176">Customizable interpolated strings</span></span>
<span data-ttu-id="3d7c7-177">Geliştiriciler, `FormattableString`ile enterpolasyonlu dizelerin davranışını özelleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-177">Developers are able to customize the behavior of interpolated strings with `FormattableString`.</span></span> <span data-ttu-id="3d7c7-178">Bu, bir enterpolasyonlu dizeye giden verileri içerir: biçim `string` ve bağımsız değişkenler dizi olarak.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-178">This contains the data which goes into an interpolated string: the format `string` and the arguments as an array.</span></span> <span data-ttu-id="3d7c7-179">Bu, yine de paketleme ve bağımsız değişken dizisi tahsisine ve `FormattableString` ayırmasının yanı sıra (bir `abstract class`).</span><span class="sxs-lookup"><span data-stu-id="3d7c7-179">This though still has the boxing and argument array allocation as well as the allocation for `FormattableString` (it's an `abstract class`).</span></span> <span data-ttu-id="3d7c7-180">Bu nedenle, `string` biçimlendirme sırasında ayırma ağır olan uygulamalar için çok az kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-180">Hence it's of little use to applications which are allocation heavy in `string` formatting.</span></span>

<span data-ttu-id="3d7c7-181">Enterpolasyonlu dize biçimlendirmesini verimli hale getirmek için dilin yeni bir tür tanıması gerekir: `System.ValueFormattableString`.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-181">To make interpolated string formatting efficient the language will recognize a new type: `System.ValueFormattableString`.</span></span> <span data-ttu-id="3d7c7-182">Tüm enterpolasyonlu dizelerin bu türe bir hedef tür dönüştürmesi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-182">All interpolated strings will have a target type conversion to this type.</span></span> <span data-ttu-id="3d7c7-183">Bu, şu anda `FormattableString.Create` için, enterpolasyonlu dize çağrı `ValueFormattableString.Create` çağırarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-183">This will be implemented by translating the interpolated string into the call `ValueFormattableString.Create` exactly as is done for `FormattableString.Create` today.</span></span> <span data-ttu-id="3d7c7-184">Dil, en uygun `ValueFormattableString.Create` yöntemini ararken, bu belgede açıklanan tüm `params` seçeneklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-184">The language will support all `params` options described in this document when looking for the most suitable `ValueFormattableString.Create` method.</span></span> 

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

<span data-ttu-id="3d7c7-185">Bağımsız değişken bir enterpolasyonlu dize olduğunda `string` üzerinde `ValueFormattableString` tercih etmek için aşırı yükleme çözümleme kuralları değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-185">Overload resolution rules will be changed to prefer `ValueFormattableString` over `string` when the argument is an interpolated string.</span></span> <span data-ttu-id="3d7c7-186">Bu, yalnızca `string` ve `ValueFormattableString`farklı olan aşırı yüklemeleri sağlamak için değerli olacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-186">This means it will be valuable to have overloads which differ only on `string` and `ValueFormattableString`.</span></span> <span data-ttu-id="3d7c7-187">Bu tür bir aşırı yükleme `FormattableString`, derleyici `string` sürümünü her zaman tercih ettiği için (Geliştirici açık bir atama kullanmadığı sürece) değerli değildir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-187">Such an overload today with `FormattableString` is not valuable as the compiler will always prefer the `string` version (unless the developer uses an explicit cast).</span></span> 

## <a name="open-issues"></a><span data-ttu-id="3d7c7-188">Açık sorunlar</span><span class="sxs-lookup"><span data-stu-id="3d7c7-188">Open Issues</span></span>

### <a name="valueformattablestring-breaking-change"></a><span data-ttu-id="3d7c7-189">ValueFormattableString bölünmesi değişikliği</span><span class="sxs-lookup"><span data-stu-id="3d7c7-189">ValueFormattableString breaking change</span></span>
<span data-ttu-id="3d7c7-190">`string` üzerinde aşırı yükleme çözümlemesi sırasında `ValueFormattableString` tercih etmek için yapılan değişiklik, bir son değişiklik olur.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-190">The change to prefer `ValueFormattableString` during overload resolution over `string` is a breaking change.</span></span> <span data-ttu-id="3d7c7-191">Bir geliştiricinin bugün `ValueFormattableString` adlı bir tür tanımlanması ve bunu `string`yöntem aşırı yüklemeleri içinde kullanabilmesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-191">It is possible for a developer to have defined a type called `ValueFormattableString` today and use it in method overloads with `string`.</span></span> <span data-ttu-id="3d7c7-192">Bu önerilen değişiklik, bu özellik kümesi uygulandıktan sonra derleyicinin farklı bir aşırı yükleme seçmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-192">This proposed change would cause the compiler to pick a different overload once this set of features was implemented.</span></span> 

<span data-ttu-id="3d7c7-193">Bunun olasılığı makul bir şekilde düşüktür.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-193">The possibility of this seems reasonably low.</span></span> <span data-ttu-id="3d7c7-194">Türün tam adı `System.ValueFormattableString` gerekir ve `Create`adında `static` yöntemlere sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-194">The type would need the full name `System.ValueFormattableString` and it would need to have `static` methods named `Create`.</span></span> <span data-ttu-id="3d7c7-195">Geliştiricilerin `System` ad alanında herhangi bir tür tanımlamaktan kesinlikle önerilmez. bu kesme makul bir uzlaşmaya benzer şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-195">Given that developers are strongly discouraged from defining any type in the `System` namespace this break seems like a reasonable compromise.</span></span>

### <a name="expanding-to-more-types"></a><span data-ttu-id="3d7c7-196">Daha fazla türe genişletme</span><span class="sxs-lookup"><span data-stu-id="3d7c7-196">Expanding to more types</span></span>
<span data-ttu-id="3d7c7-197">Verilen alanda, `params` desteklendiği koleksiyonlar kümesine `IList<T>`, `ICollection<T>` ve `IReadOnlyList<T>` eklemeyi düşünmemiz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-197">Given we're in the area we should consider adding `IList<T>`, `ICollection<T>` and `IReadOnlyList<T>` to the set of collections for which `params` is supported.</span></span> <span data-ttu-id="3d7c7-198">Uygulama açısından buradaki diğer işler üzerinde küçük miktarda ücret alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-198">In terms of implementation it will cost a small amount over the other work here.</span></span>

<span data-ttu-id="3d7c7-199">Bu dile daha karmaşıkmaya değip değmeyeceğine karar vermek için LDM gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-199">LDM needs to decide if the complication to the language is worth it though.</span></span> <span data-ttu-id="3d7c7-200">`IEnumerable<T>` eklenmesi, çok özel bir uyuşmazlıkları noktasını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-200">The addition of `IEnumerable<T>` removes a very specific friction point.</span></span> <span data-ttu-id="3d7c7-201">Bu `params` çözümü çok sayıda müşteri, bir `params` yöntemi çağırırken bir `IEnumerable<T>` `T[]` ayırmaya zorlandı.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-201">Lacking this `params` solution many customers were forced to allocate `T[]` from an `IEnumerable<T>` when calling a `params` method.</span></span> <span data-ttu-id="3d7c7-202">`IEnumerable<T>` eklenmesi bu da bunu düzeltir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-202">The addition of `IEnumerable<T>` fixes this though.</span></span> <span data-ttu-id="3d7c7-203">Diğer arabirimlerin burada çözmesine yönelik belirli bir uçuşme noktası yoktur.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-203">There is no specific friction point that the other interfaces fix here.</span></span> 

## <a name="considerations"></a><span data-ttu-id="3d7c7-204">Dikkat edilmesi gerekenler</span><span class="sxs-lookup"><span data-stu-id="3d7c7-204">Considerations</span></span>

### <a name="variant2-and-variant3"></a><span data-ttu-id="3d7c7-205">Variant2 ve Variant3</span><span class="sxs-lookup"><span data-stu-id="3d7c7-205">Variant2 and Variant3</span></span>
<span data-ttu-id="3d7c7-206">CoreFX ekibinin, en fazla üç `Variant` bağımsız değişkeni için ayırma olmayan bir depolama türleri kümesi de vardır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-206">The CoreFX team also has a non-allocating set of storage types for up to three `Variant` arguments.</span></span> <span data-ttu-id="3d7c7-207">Bunlar tek bir `Variant`, `Variant2` ve `Variant3`.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-207">These are a single `Variant`, `Variant2` and `Variant3`.</span></span> <span data-ttu-id="3d7c7-208">Her birinin, ayırma ücretsiz `Span<Variant>` almak için bir çift yöntemi vardır: `CreateSpan` ve `KeepAlive`.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-208">All have a pair of methods for getting an allocation free `Span<Variant>` off of them: `CreateSpan` and `KeepAlive`.</span></span> <span data-ttu-id="3d7c7-209">Bu, çağrı sitesinin tamamen serbest ayrılabileceği en fazla üç bağımsız değişkeni `params Span<Variant>` anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-209">This means for a `params Span<Variant>` of up to three arguments the call site can be entirely allocation free.</span></span>

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

<span data-ttu-id="3d7c7-210">`Go` yöntemi aşağıdakilere indirgenmelidir:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-210">The `Go` method can be lowered to the following:</span></span>

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

<span data-ttu-id="3d7c7-211">Bu, `params Span<T>` çağrıları arasında `T[]` yeniden kullanmak için teklifin üzerine çok az iş gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-211">This requires very little work on top of the proposal to re-use `T[]` between `params Span<T>` calls.</span></span> <span data-ttu-id="3d7c7-212">Derleyicinin zaten geçici olarak geçici bir çağrı yönetmesi ve işi temizlemesi gerekir (bir durumda bir iç geçici olarak bir iç geçici olarak işaretlebilse bile).</span><span class="sxs-lookup"><span data-stu-id="3d7c7-212">The compiler already needs to manage a temporary per call and do clean up work after (even if in one case it's just marking an internal temp as free).</span></span> 

<span data-ttu-id="3d7c7-213">Note: `KeepAlive` işlevi yalnızca masaüstünüzde gereklidir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-213">Note: the `KeepAlive` function is only necessary on desktop.</span></span> <span data-ttu-id="3d7c7-214">.NET Core 'da yöntem kullanılabilir olmayacaktır ve bu nedenle derleyici buna bir çağrı görmez.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-214">On .NET Core the method will not be available and hence the compiler won't emit a call to it.</span></span>

### <a name="clr-stack-allocation-helpers"></a><span data-ttu-id="3d7c7-215">CLR Stack ayırma yardımcıları</span><span class="sxs-lookup"><span data-stu-id="3d7c7-215">CLR stack allocation helpers</span></span>
<span data-ttu-id="3d7c7-216">CLR yalnızca, bitişik belleğin yığın ayırması için yalnızca [güncelleştirilemez](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-216">The CLR only provides only [localloc](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.opcodes.localloc?redirectedfrom=MSDN&view=netframework-4.7.2) for stack allocation of contiguous memory.</span></span> <span data-ttu-id="3d7c7-217">Bu yönerge yalnızca `unmanaged` türlerinde çalışacak şekilde sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-217">This instruction is limited in that it only works for `unmanaged` types.</span></span> <span data-ttu-id="3d7c7-218">Bu, `params 
 Span<T>`için yedekleme depolama alanını verimli bir şekilde ayırmak üzere bir evrensel çözüm olarak kullanılamayan anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-218">This means it can't be used as a universal solution for efficiently allocating the backing storage for `params 
 Span<T>`.</span></span> 

<span data-ttu-id="3d7c7-219">Bu sınırlama bazı temel kısıtlamalar değildir, ancak bunun yerine daha fazla bir geçmiş vardır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-219">This limitation is not some fundamental restriction though but instead more an artifact of history.</span></span> <span data-ttu-id="3d7c7-220">CLR, evrensel yığın ayırması sağlayan yeni işlem kodları/iç bilgileri eklemeyi seçebilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-220">The CLR could choose to add new op codes / intrinsics which provide universal stack allocation.</span></span> <span data-ttu-id="3d7c7-221">Bunlar daha sonra çoğu `params Span<T>` çağrısı için yedekleme depolama alanı ayırmak üzere kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-221">These could then be used to allocate the backing storage for most `params Span<T>` calls.</span></span>

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

<span data-ttu-id="3d7c7-222">`Go` yöntemi aşağıdakilere indirgenmelidir:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-222">The `Go` method can be lowered to the following:</span></span>

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

<span data-ttu-id="3d7c7-223">Bu yaklaşım çok yığın verimli olsa da ek yığın kullanımına neden olur.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-223">While this approach is very heap efficient it does cause extra stack usage.</span></span> <span data-ttu-id="3d7c7-224">Derin bir yığını ve çok sayıda `params` kullanımı olan bir algoritmada, basit bir `T[]` ayırmanın başarılı olacağı bir `StackOverflowException` oluşturulmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-224">In an algorithm which has a deep stack and lots of `params` usage it's possible this could cause a `StackOverflowException` to be generated where a simple `T[]` allocation would succeed.</span></span> 

<span data-ttu-id="3d7c7-225">Ne C# yazık ki, çağrının `params`yığın veya yığın ayırmayı kullanması gerekip gerekmediğini bir şekilde belirleme konusunda bir eğitime yöntemi için ayarlanmıyor.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-225">Unfortunately C# is not set up for the type of inter-method analysis where it could make an educated determination of whether or not call should use stack or heap allocation of `params`.</span></span> <span data-ttu-id="3d7c7-226">Her çağrıyı yalnızca kendi kendine kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-226">It can only really consider each call on its own.</span></span>

<span data-ttu-id="3d7c7-227">CLR, çalışma zamanında bu tür bir belirleme yapmak için en iyi kurulumdır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-227">The CLR is best setup for making this type of determination at runtime.</span></span> <span data-ttu-id="3d7c7-228">Bu nedenle, çalışma zamanının evrensel yığın ayırma için iki yöntem sağlaması olasıdır:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-228">Hence we'd likely have the runtime provide two methods for universal stack allocation:</span></span>

1. <span data-ttu-id="3d7c7-229">`Span<T> StackAlloc<T>(int length)`: Bu, `T`herhangi bir tür üzerinde çalışmaları hariç `localloc` aynı davranış ve sınırlamalara sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-229">`Span<T> StackAlloc<T>(int length)`: this has the same behaviors and limitations of `localloc` except it can work on any type `T`.</span></span> 
1. <span data-ttu-id="3d7c7-230">`Span<T> MaybeStackAlloc<T>(int length)`: Bu çalışma zamanı, yığın veya yığın ayırmayı gerçekleştirerek bunu uygulamayı seçebilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-230">`Span<T> MaybeStackAlloc<T>(int length)`: this runtime can choose to implement this by doing a stack or heap allocation.</span></span> <span data-ttu-id="3d7c7-231">Çalışma zamanı daha sonra, `Span<T>` nasıl ayrılacağını anlamak için çağrıldığı Yürütme bağlamını kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-231">The runtime can then use the execution context in which it's called to determine how the `Span<T>` is allocated.</span></span> <span data-ttu-id="3d7c7-232">Çağıran, yığın ayrılmış gibi her zaman kabul eder.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-232">The caller though will always treat it as if it were stack allocated.</span></span>

<span data-ttu-id="3d7c7-233">Biri iki bağımsız değişken gibi basit durumlarda, C# derleyici her zaman `StackAlloc<T>` değişken kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-233">For very simple cases, like one to two arguments, the C# compiler could always use `StackAlloc<T>` variant.</span></span> <span data-ttu-id="3d7c7-234">Çoğu durumda yığın tükenmesi için önemli ölçüde katkıda bulunmak çok düşüktür.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-234">This is unlikely to significantly contribute to stack exhaustion in most cases.</span></span> <span data-ttu-id="3d7c7-235">Diğer durumlarda derleyici `MaybeStackAlloc<T>` kullanmayı seçebilir ve çalışma zamanının çağrıyı yapmasına izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-235">For other cases the compiler could choose to use `MaybeStackAlloc<T>` instead and let the runtime make the call.</span></span>

<span data-ttu-id="3d7c7-236">Nasıl seçtiğimiz, büyük olasılıkla gerçek dünya uygulamalarının daha ayrıntılı bir şekilde araştırılması ve incelenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-236">How we choose will likely require a deeper investigation and examination of real world apps.</span></span> <span data-ttu-id="3d7c7-237">Ancak bu yeni iç bilgiler varsa bize bu tür bir esneklik verecektir.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-237">But if these new intrinsics are available then it will give us this type of flexibility.</span></span>

### <a name="why-not-varargs"></a><span data-ttu-id="3d7c7-238">Neden varargs değil?</span><span class="sxs-lookup"><span data-stu-id="3d7c7-238">Why not varargs?</span></span> 
<span data-ttu-id="3d7c7-239">Mevcut [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) özelliği, olası bir çözüm olarak burada kabul edildi.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-239">The existing [varargs](https://docs.microsoft.com/en-us/cpp/windows/variable-argument-lists-dot-dot-dot-cpp-cli?view=vs-2017) feature was considered here as a possible solution.</span></span> <span data-ttu-id="3d7c7-240">Bu özellik, öncelikli olarak C++/CLI senaryolarına yöneliktir ve diğer senaryolar için bilinen delikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-240">This feature though is meant primarily for C++/CLI scenarios and has known holes for other scenarios.</span></span> <span data-ttu-id="3d7c7-241">Ayrıca, bunu UNIX 'e taşıma konusunda önemli bir maliyet de vardır.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-241">Additionally there is significant cost in porting this to Unix.</span></span> <span data-ttu-id="3d7c7-242">Bu nedenle, uygulanabilir bir çözüm olarak görülmedi.</span><span class="sxs-lookup"><span data-stu-id="3d7c7-242">Hence it wasn't seen as a viable solution.</span></span>

## <a name="related-issues"></a><span data-ttu-id="3d7c7-243">İlgili sorunlar</span><span class="sxs-lookup"><span data-stu-id="3d7c7-243">Related Issues</span></span>
<span data-ttu-id="3d7c7-244">Bu belirtim aşağıdaki sorunlarla ilgilidir:</span><span class="sxs-lookup"><span data-stu-id="3d7c7-244">This spec is related to the following issues:</span></span> 

- https://github.com/dotnet/csharplang/issues/1757
- https://github.com/dotnet/csharplang/issues/179
- https://github.com/dotnet/corefxlab/pull/2595

