---
ms.openlocfilehash: 4e2a536bab00859b003e8d967cb1927a99a9fa21
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484540"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="dd78b-101">Hedef türü belirlenmiş `new` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="dd78b-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="dd78b-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="dd78b-102">[x] Proposed</span></span>
* <span data-ttu-id="dd78b-103">[x] [prototipi](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="dd78b-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="dd78b-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="dd78b-104">[ ] Implementation</span></span>
* <span data-ttu-id="dd78b-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="dd78b-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="dd78b-106">Özet</span><span class="sxs-lookup"><span data-stu-id="dd78b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="dd78b-107">Tür bilindiğinde oluşturucular için tür belirtimi gerektirmeyin.</span><span class="sxs-lookup"><span data-stu-id="dd78b-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="dd78b-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="dd78b-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="dd78b-109">Türü çoğaltmadan alan başlatılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="dd78b-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```
<span data-ttu-id="dd78b-110">Kullanımdan çıkarsanamıyor, türü dışarıda bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd78b-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```
<span data-ttu-id="dd78b-111">Türü yazarken bir nesne oluşturun.</span><span class="sxs-lookup"><span data-stu-id="dd78b-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```
## <a name="detailed-design"></a><span data-ttu-id="dd78b-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="dd78b-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="dd78b-113">*Object_creation_expression* sözdizimi, parantezler mevcut olduğunda *türü* isteğe bağlı hale getirmek için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="dd78b-114">Bu, *anonymous_object_creation_expression*belirsizliğin ele almak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```
<span data-ttu-id="dd78b-115">Hedef türü belirlenmiş bir `new` herhangi bir türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="dd78b-116">Sonuç olarak, aşırı yükleme çözümüne katkıda bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="dd78b-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="dd78b-117">Bu, genellikle öngörülemeyen son değişikliklerden kaçınmaktır.</span><span class="sxs-lookup"><span data-stu-id="dd78b-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="dd78b-118">Bağımsız değişken listesi ve başlatıcı ifadeleri tür saptandıktan sonra bağlanacak.</span><span class="sxs-lookup"><span data-stu-id="dd78b-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="dd78b-119">İfadenin türü, aşağıdakilerden biri olması gereken hedef türden çıkarsanamıyor:</span><span class="sxs-lookup"><span data-stu-id="dd78b-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="dd78b-120">**Herhangi bir struct türü**</span><span class="sxs-lookup"><span data-stu-id="dd78b-120">**Any struct type**</span></span>
- <span data-ttu-id="dd78b-121">**Herhangi bir başvuru türü**</span><span class="sxs-lookup"><span data-stu-id="dd78b-121">**Any reference type**</span></span>
- <span data-ttu-id="dd78b-122">Oluşturucu veya `struct` kısıtlaması olan **herhangi bir tür parametresi**</span><span class="sxs-lookup"><span data-stu-id="dd78b-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="dd78b-123">Aşağıdaki özel durumlarla birlikte:</span><span class="sxs-lookup"><span data-stu-id="dd78b-123">with the following exceptions:</span></span>

- <span data-ttu-id="dd78b-124">Sabit listesi türleri **:** tüm sabit listesi türleri sıfır sabiti içermez, bu nedenle açık numaralandırma üyesinin kullanılması istenebilir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="dd78b-125">**Arabirim türleri:** bu bir sunarak pazarların özelliğidir ve türün açıkça bahsetmek için tercih edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="dd78b-126">**Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="dd78b-127">**Struct varsayılan Oluşturucusu**: Bu, tüm ilkel türler ve çoğu değer türlerini kurallar.</span><span class="sxs-lookup"><span data-stu-id="dd78b-127">**Struct default constructor**: this rules out all primitive types and most value types.</span></span> <span data-ttu-id="dd78b-128">Bu tür türler için varsayılan değeri kullanmak isterseniz, bunun yerine `default` yazabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dd78b-128">If you wanted to use the default value of such types you could write `default` instead.</span></span>

<span data-ttu-id="dd78b-129">*Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="dd78b-129">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

> <span data-ttu-id="dd78b-130">**Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="dd78b-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="dd78b-131">Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="dd78b-132">Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

var x = new() == (1, 2); // ruled out by "use of struct default constructor"
var x = new(1, 2) == (1, 2) // "new" is redundant
```


> <span data-ttu-id="dd78b-133">**Açık sorun:** hedef tür olarak `Exception` `throw new()` izin vermemiz gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="dd78b-133">**Open Issue:** should we allow `throw new()` with `Exception` as the target-type?</span></span>

<span data-ttu-id="dd78b-134">Bugün `throw null`, ancak `throw default` değil (aynı etkiye sahip olabilir).</span><span class="sxs-lookup"><span data-stu-id="dd78b-134">We have `throw null` today, but not `throw default` (though it would have the same effect).</span></span> <span data-ttu-id="dd78b-135">Diğer taraftan, `throw new()` `throw new Exception(...)`için bir toplu olarak yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-135">On the other hand, `throw new()` could be actually useful as a shorthand for `throw new Exception(...)`.</span></span> <span data-ttu-id="dd78b-136">Geçerli belirtim tarafından zaten izin verildiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="dd78b-136">Note that it is already allowed by the current specification.</span></span> <span data-ttu-id="dd78b-137">`Exception` bir başvuru türüdür ve throw deyiminin belirtimi, ifadenin `Exception`dönüştürüldüğünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="dd78b-137">`Exception` is a reference type, and the specification for the throw statement says that the expression is converted to `Exception`.</span></span>

> <span data-ttu-id="dd78b-138">**Açık sorun:** Kullanıcı tanımlı karşılaştırma ve Aritmetik işleçlerle, hedef türü belirlenmiş `new` kullanımlarına izin vermemiz gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="dd78b-138">**Open Issue:** should we allow usages of a target-typed `new` with user-defined comparison and arithmetic operators?</span></span>

<span data-ttu-id="dd78b-139">Karşılaştırma için `default` yalnızca eşitlik (Kullanıcı tanımlı ve yerleşik) işleçlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="dd78b-139">For comparison, `default` only supports equality (user-defined and built-in) operators.</span></span> <span data-ttu-id="dd78b-140">Diğer işleçleri de `new()` destekletsin mi?</span><span class="sxs-lookup"><span data-stu-id="dd78b-140">Would it make sense to support other operators for `new()` as well?</span></span>

## <a name="drawbacks"></a><span data-ttu-id="dd78b-141">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="dd78b-141">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="dd78b-142">Yok.</span><span class="sxs-lookup"><span data-stu-id="dd78b-142">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="dd78b-143">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="dd78b-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="dd78b-144">Alan başlatılmasında yinelenmek için çok uzun olan türlerin büyük bir kısmının türü, türün kendisi değil tür *bağımsız değişkenleri* hakkında bilgi verebiliriz. yalnızca `new Dictionary(...)` (veya benzeri) gibi tür bağımsız değişkenleri ve bağımsız değişkenlerden ya da koleksiyon başlatıcıdan yerel olarak tür bağımsız değişkenleri çıkarsız.</span><span class="sxs-lookup"><span data-stu-id="dd78b-144">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="dd78b-145">UL</span><span class="sxs-lookup"><span data-stu-id="dd78b-145">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="dd78b-146">İfade ağaçlarında kullanımları planlıyoruz mı?</span><span class="sxs-lookup"><span data-stu-id="dd78b-146">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="dd78b-147">eşleşen</span><span class="sxs-lookup"><span data-stu-id="dd78b-147">(no)</span></span>
- <span data-ttu-id="dd78b-148">Özellik `dynamic` bağımsız değişkenlerle nasıl etkileşime girer?</span><span class="sxs-lookup"><span data-stu-id="dd78b-148">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="dd78b-149">(özel bir işleme yok)</span><span class="sxs-lookup"><span data-stu-id="dd78b-149">(no special treatment)</span></span>
- <span data-ttu-id="dd78b-150">IntelliSense `new()`ile nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="dd78b-150">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="dd78b-151">(yalnızca tek bir hedef türü olduğunda)</span><span class="sxs-lookup"><span data-stu-id="dd78b-151">(only when there is a single target-type)</span></span>
## <a name="design-meetings"></a><span data-ttu-id="dd78b-152">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="dd78b-152">Design meetings</span></span>

- [<span data-ttu-id="dd78b-153">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="dd78b-153">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
