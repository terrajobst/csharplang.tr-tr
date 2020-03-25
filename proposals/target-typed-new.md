---
ms.openlocfilehash: 07b4afe4a3fcbf10c978f05e642dfd8a47d53ea5
ms.sourcegitcommit: 194a043db72b9244f8db45db326cc82de6cec965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80217209"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="a81ea-101">Hedef türü belirlenmiş `new` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="a81ea-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="a81ea-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="a81ea-102">[x] Proposed</span></span>
* <span data-ttu-id="a81ea-103">[x] [prototipi](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="a81ea-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="a81ea-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="a81ea-104">[ ] Implementation</span></span>
* <span data-ttu-id="a81ea-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="a81ea-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="a81ea-106">Özet</span><span class="sxs-lookup"><span data-stu-id="a81ea-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a81ea-107">Tür bilindiğinde oluşturucular için tür belirtimi gerektirmeyin.</span><span class="sxs-lookup"><span data-stu-id="a81ea-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="a81ea-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="a81ea-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a81ea-109">Türü çoğaltmadan alan başlatılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="a81ea-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="a81ea-110">Kullanımdan çıkarsanamıyor, türü dışarıda bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a81ea-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="a81ea-111">Türü yazarken bir nesne oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a81ea-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="a81ea-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="a81ea-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a81ea-113">*Object_creation_expression* sözdizimi, parantezler mevcut olduğunda *türü* isteğe bağlı hale getirmek için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="a81ea-114">Bu, *anonymous_object_creation_expression*belirsizliğin ele almak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="a81ea-115">Hedef türü belirlenmiş bir `new` herhangi bir türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="a81ea-116">Sonuç olarak, aşırı yükleme çözümüne katkıda bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="a81ea-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="a81ea-117">Bu, genellikle öngörülemeyen son değişikliklerden kaçınmaktır.</span><span class="sxs-lookup"><span data-stu-id="a81ea-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="a81ea-118">Bağımsız değişken listesi ve başlatıcı ifadeleri tür saptandıktan sonra bağlanacak.</span><span class="sxs-lookup"><span data-stu-id="a81ea-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="a81ea-119">İfadenin türü, aşağıdakilerden biri olması gereken hedef türden çıkarsanamıyor:</span><span class="sxs-lookup"><span data-stu-id="a81ea-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="a81ea-120">**Herhangi bir struct türü** (demet türleri dahil)</span><span class="sxs-lookup"><span data-stu-id="a81ea-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="a81ea-121">**Herhangi bir başvuru türü** (temsilci türleri dahil)</span><span class="sxs-lookup"><span data-stu-id="a81ea-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="a81ea-122">Oluşturucu veya `struct` kısıtlaması olan **herhangi bir tür parametresi**</span><span class="sxs-lookup"><span data-stu-id="a81ea-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="a81ea-123">Aşağıdaki özel durumlarla birlikte:</span><span class="sxs-lookup"><span data-stu-id="a81ea-123">with the following exceptions:</span></span>

- <span data-ttu-id="a81ea-124">Sabit listesi türleri **:** tüm sabit listesi türleri sıfır sabiti içermez, bu nedenle açık numaralandırma üyesinin kullanılması istenebilir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="a81ea-125">**Arabirim türleri:** bu bir sunarak pazarların özelliğidir ve türün açıkça bahsetmek için tercih edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="a81ea-126">**Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="a81ea-127">**dinamik:** `new dynamic()`izin vermedik, bu nedenle hedef tür olarak `dynamic` `new()` izin vermedik.</span><span class="sxs-lookup"><span data-stu-id="a81ea-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="a81ea-128">*Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="a81ea-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="a81ea-129">Hedef türü null yapılabilir bir değer türü olduğunda, hedef türü belirlenmiş `new` null yapılabilir tür yerine temel alınan türe dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="a81ea-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="a81ea-130">**Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="a81ea-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="a81ea-131">Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="a81ea-132">Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a81ea-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="a81ea-133">Çeşitli</span><span class="sxs-lookup"><span data-stu-id="a81ea-133">Miscellaneous</span></span>

<span data-ttu-id="a81ea-134">`throw new()` izin verilmiyor.</span><span class="sxs-lookup"><span data-stu-id="a81ea-134">`throw new()` is disallowed.</span></span>

<span data-ttu-id="a81ea-135">Target-Typed `new` ikili işleçlerle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="a81ea-135">Target-typed `new` is not allowed with binary operators.</span></span>

<span data-ttu-id="a81ea-136">Target için tür olmadığında bu izin verilmez: Birli İşleçler, `foreach`koleksiyonu bir `using`, bir `await` ifadesinde, `sizeof`işlecinin sol işleneni olarak `fixed` işlecinin işleneni olarak, bir`new().field`bildiriminde bir`someDynamic.Method(new())`deyimindeki bir`new { Prop = new() }`) anonim tür özelliği (`is`) olarak, bir `??` deyimi içinde `lock`, işleci içindeki bir deyiminde, bir ifadesinde ,  ...</span><span class="sxs-lookup"><span data-stu-id="a81ea-136">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>

<span data-ttu-id="a81ea-137">Ayrıca, `ref`olarak da izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="a81ea-137">It is also disallowed as a `ref`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a81ea-138">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="a81ea-138">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a81ea-139">Hedef türü bazı sorunlar vardı `new` yeni son değişiklikler kategorileri oluşturuyor, ancak zaten `null` ve `default`ve bu da önemli bir sorun olmadık.</span><span class="sxs-lookup"><span data-stu-id="a81ea-139">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a81ea-140">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="a81ea-140">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a81ea-141">Alan başlatılmasında yinelenmek için çok uzun olan türlerin büyük bir kısmının türü, türün kendisi değil tür *bağımsız değişkenleri* hakkında bilgi verebiliriz. yalnızca `new Dictionary(...)` (veya benzeri) gibi tür bağımsız değişkenleri ve bağımsız değişkenlerden ya da koleksiyon başlatıcıdan yerel olarak tür bağımsız değişkenleri çıkarsız.</span><span class="sxs-lookup"><span data-stu-id="a81ea-141">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="a81ea-142">UL</span><span class="sxs-lookup"><span data-stu-id="a81ea-142">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="a81ea-143">İfade ağaçlarında kullanımları planlıyoruz mı?</span><span class="sxs-lookup"><span data-stu-id="a81ea-143">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="a81ea-144">eşleşen</span><span class="sxs-lookup"><span data-stu-id="a81ea-144">(no)</span></span>
- <span data-ttu-id="a81ea-145">Özellik `dynamic` bağımsız değişkenlerle nasıl etkileşime girer?</span><span class="sxs-lookup"><span data-stu-id="a81ea-145">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="a81ea-146">(özel bir işleme yok)</span><span class="sxs-lookup"><span data-stu-id="a81ea-146">(no special treatment)</span></span>
- <span data-ttu-id="a81ea-147">IntelliSense `new()`ile nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="a81ea-147">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="a81ea-148">(yalnızca tek bir hedef türü olduğunda)</span><span class="sxs-lookup"><span data-stu-id="a81ea-148">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a81ea-149">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="a81ea-149">Design meetings</span></span>

- [<span data-ttu-id="a81ea-150">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="a81ea-150">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="a81ea-151">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="a81ea-151">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="a81ea-152">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="a81ea-152">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="a81ea-153">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="a81ea-153">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="a81ea-154">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="a81ea-154">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
