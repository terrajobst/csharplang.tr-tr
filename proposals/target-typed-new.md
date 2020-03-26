---
ms.openlocfilehash: f000dda7eeb1c4f17c26f94c326a12a9d0014288
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281976"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="36fb9-101">Hedef türü belirlenmiş `new` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="36fb9-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="36fb9-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="36fb9-102">[x] Proposed</span></span>
* <span data-ttu-id="36fb9-103">[x] [prototipi](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="36fb9-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="36fb9-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="36fb9-104">[ ] Implementation</span></span>
* <span data-ttu-id="36fb9-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="36fb9-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="36fb9-106">Özet</span><span class="sxs-lookup"><span data-stu-id="36fb9-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="36fb9-107">Tür bilindiğinde oluşturucular için tür belirtimi gerektirmeyin.</span><span class="sxs-lookup"><span data-stu-id="36fb9-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="36fb9-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="36fb9-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="36fb9-109">Türü çoğaltmadan alan başlatılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="36fb9-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="36fb9-110">Kullanımdan çıkarsanamıyor, türü dışarıda bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="36fb9-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="36fb9-111">Türü yazarken bir nesne oluşturun.</span><span class="sxs-lookup"><span data-stu-id="36fb9-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="specification"></a><span data-ttu-id="36fb9-112">Min</span><span class="sxs-lookup"><span data-stu-id="36fb9-112">Specification</span></span>
[design]: #detailed-design

<span data-ttu-id="36fb9-113">Yeni bir sözdizimsel form, *object_creation_expression* *target_typed_new* , *türün* isteğe bağlı olduğu kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="36fb9-113">A new syntactic form, *target_typed_new* of the *object_creation_expression* is accepted in which the *type* is optional.</span></span>

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    | target_typed_new
    ;
target_typed_new
    : 'new' '(' argument_list? ')' object_or_collection_initializer?
    ;
```

<span data-ttu-id="36fb9-114">*Target_typed_new* ifadesinin türü yok.</span><span class="sxs-lookup"><span data-stu-id="36fb9-114">A *target_typed_new* expression does not have a type.</span></span> <span data-ttu-id="36fb9-115">Ancak, ifadeden örtük dönüştürme olan yeni bir *nesne oluşturma dönüştürmesi* vardır. bu bir target_typed_new, her türe bir *target_typed_new* vardır.</span><span class="sxs-lookup"><span data-stu-id="36fb9-115">However, there is a new *object creation conversion* that is an implicit conversion from expression, that exists from a *target_typed_new* to every type.</span></span>

<span data-ttu-id="36fb9-116">Hedef türü `T`verildiğinde tür `T0`, `T` `System.Nullable`bir örneğidir `T`temel türüdür.</span><span class="sxs-lookup"><span data-stu-id="36fb9-116">Given a target type `T`, the type `T0` is `T`'s underlying type if `T` is an instance of `System.Nullable`.</span></span> <span data-ttu-id="36fb9-117">Aksi takdirde `T0` `T`.</span><span class="sxs-lookup"><span data-stu-id="36fb9-117">Otherwise `T0` is `T`.</span></span> <span data-ttu-id="36fb9-118">`T` türüne dönüştürülen *target_typed_new* ifadesinin anlamı, türü olarak `T0` belirten karşılık gelen bir *object_creation_expression* anlamı ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="36fb9-118">The meaning of a *target_typed_new* expression that is converted to the type `T` is the same as the meaning of a corresponding *object_creation_expression* that specifies `T0` as the type.</span></span>

<span data-ttu-id="36fb9-119">Bir *target_typed_new* birli veya ikili işlecin işleneni olarak kullanılıyorsa ya da bir *nesne oluşturma dönüştürmesinin*konusu olmadığı durumlarda kullanılıyorsa, derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="36fb9-119">It is a compile-time error if a *target_typed_new* is used as an operand of a unary or binary operator, or if it is used where it is not subject to an *object creation conversion*.</span></span>

> <span data-ttu-id="36fb9-120">**Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="36fb9-120">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="36fb9-121">Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir.</span><span class="sxs-lookup"><span data-stu-id="36fb9-121">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="36fb9-122">Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="36fb9-122">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // OK; same as (0, 0)
Action a = new(); // no constructor found
```

### <a name="miscellaneous"></a><span data-ttu-id="36fb9-123">Çeşitli</span><span class="sxs-lookup"><span data-stu-id="36fb9-123">Miscellaneous</span></span>

<span data-ttu-id="36fb9-124">Aşağıda, belirtimin sonuçları verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="36fb9-124">The following are consequences of the specification:</span></span>

- <span data-ttu-id="36fb9-125">`throw new()` izin veriliyor (hedef tür `System.Exception`)</span><span class="sxs-lookup"><span data-stu-id="36fb9-125">`throw new()` is allowed (the target type is `System.Exception`)</span></span>
- <span data-ttu-id="36fb9-126">Target-Typed `new` ikili işleçlerle kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="36fb9-126">Target-typed `new` is not allowed with binary operators.</span></span>
- <span data-ttu-id="36fb9-127">Target için tür olmadığında bu izin verilmez: Birli İşleçler, `foreach`koleksiyonu bir `using`, bir `await` ifadesinde, `sizeof`işlecinin sol işleneni olarak `fixed` işlecinin işleneni olarak, bir`new().field`bildiriminde bir`someDynamic.Method(new())`deyimindeki bir`new { Prop = new() }`) anonim tür özelliği (`is`) olarak, bir `??` deyimi içinde `lock`, işleci içindeki bir deyiminde, bir ifadesinde ,  ...</span><span class="sxs-lookup"><span data-stu-id="36fb9-127">It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...</span></span>
- <span data-ttu-id="36fb9-128">Ayrıca, `ref`olarak da izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="36fb9-128">It is also disallowed as a `ref`.</span></span>
- <span data-ttu-id="36fb9-129">Dönüştürme hedefleri olarak aşağıdaki tür türlere izin verilmez</span><span class="sxs-lookup"><span data-stu-id="36fb9-129">The following kinds of types are not permitted as targets of the conversion</span></span>
  - <span data-ttu-id="36fb9-130">**Sabit listesi türleri:** `new()` çalışacaktır (`new Enum()` varsayılan değer vermek için çalışır), ancak `new(1)` iş numaralama türlerinde bir oluşturucuya sahip değildir.</span><span class="sxs-lookup"><span data-stu-id="36fb9-130">**Enum types:** `new()` will work (as `new Enum()` works to give the default value), but `new(1)` will not work as enum types do not have a constructor.</span></span>
  - <span data-ttu-id="36fb9-131">**Arabirim türleri:** Bu, COM türleri için ilgili oluşturma ifadesiyle aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="36fb9-131">**Interface types:** This would work the same as the corresponding creation expression for COM types.</span></span>
  - <span data-ttu-id="36fb9-132">**Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="36fb9-132">**Array types:** arrays need a special syntax to provide the length.</span></span>    
  - <span data-ttu-id="36fb9-133">**dinamik:** `new dynamic()`izin vermedik, bu nedenle hedef tür olarak `dynamic` `new()` izin vermedik.</span><span class="sxs-lookup"><span data-stu-id="36fb9-133">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>
  - <span data-ttu-id="36fb9-134">**Tanımlama grupları:** Bunlar, temel alınan türü kullanarak bir nesne oluşturma ile aynı anlama sahiptir.</span><span class="sxs-lookup"><span data-stu-id="36fb9-134">**tuples:** These have the same meaning as an object creation using the underlying type.</span></span>
  - <span data-ttu-id="36fb9-135">*Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="36fb9-135">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>   

## <a name="drawbacks"></a><span data-ttu-id="36fb9-136">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="36fb9-136">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="36fb9-137">Hedef türü bazı sorunlar vardı `new` yeni son değişiklikler kategorileri oluşturuyor, ancak zaten `null` ve `default`ve bu da önemli bir sorun olmadık.</span><span class="sxs-lookup"><span data-stu-id="36fb9-137">There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.</span></span>

## <a name="alternatives"></a><span data-ttu-id="36fb9-138">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="36fb9-138">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="36fb9-139">Alan başlatılmasında yinelenmek için çok uzun olan türlerin büyük bir kısmının türü, türün kendisi değil tür *bağımsız değişkenleri* hakkında bilgi verebiliriz. yalnızca `new Dictionary(...)` (veya benzeri) gibi tür bağımsız değişkenleri ve bağımsız değişkenlerden ya da koleksiyon başlatıcıdan yerel olarak tür bağımsız değişkenleri çıkarsız.</span><span class="sxs-lookup"><span data-stu-id="36fb9-139">Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.</span></span>

## <a name="questions"></a><span data-ttu-id="36fb9-140">UL</span><span class="sxs-lookup"><span data-stu-id="36fb9-140">Questions</span></span>
[questions]: #questions

- <span data-ttu-id="36fb9-141">İfade ağaçlarında kullanımları planlıyoruz mı?</span><span class="sxs-lookup"><span data-stu-id="36fb9-141">Should we forbid usages in expression trees?</span></span> <span data-ttu-id="36fb9-142">eşleşen</span><span class="sxs-lookup"><span data-stu-id="36fb9-142">(no)</span></span>
- <span data-ttu-id="36fb9-143">Özellik `dynamic` bağımsız değişkenlerle nasıl etkileşime girer?</span><span class="sxs-lookup"><span data-stu-id="36fb9-143">How the feature interacts with `dynamic` arguments?</span></span> <span data-ttu-id="36fb9-144">(özel bir işleme yok)</span><span class="sxs-lookup"><span data-stu-id="36fb9-144">(no special treatment)</span></span>
- <span data-ttu-id="36fb9-145">IntelliSense `new()`ile nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="36fb9-145">How IntelliSense should work with `new()`?</span></span> <span data-ttu-id="36fb9-146">(yalnızca tek bir hedef türü olduğunda)</span><span class="sxs-lookup"><span data-stu-id="36fb9-146">(only when there is a single target-type)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="36fb9-147">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="36fb9-147">Design meetings</span></span>

- [<span data-ttu-id="36fb9-148">LDM-2017-10-18</span><span class="sxs-lookup"><span data-stu-id="36fb9-148">LDM-2017-10-18</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [<span data-ttu-id="36fb9-149">LDM-2018-05-21</span><span class="sxs-lookup"><span data-stu-id="36fb9-149">LDM-2018-05-21</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [<span data-ttu-id="36fb9-150">LDM-2018-06-25</span><span class="sxs-lookup"><span data-stu-id="36fb9-150">LDM-2018-06-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [<span data-ttu-id="36fb9-151">LDM-2018-08-22</span><span class="sxs-lookup"><span data-stu-id="36fb9-151">LDM-2018-08-22</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [<span data-ttu-id="36fb9-152">LDM-2018-10-17</span><span class="sxs-lookup"><span data-stu-id="36fb9-152">LDM-2018-10-17</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
- [<span data-ttu-id="36fb9-153">LDM-2020-03-25</span><span class="sxs-lookup"><span data-stu-id="36fb9-153">LDM-2020-03-25</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2020/LDM-2020-03-25.md)
