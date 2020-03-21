---
ms.openlocfilehash: 38740069a2e105f920fa275c443f4560055e2901
ms.sourcegitcommit: 9aa177443b83116fe1be2ab28e2c7291947fe32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "80108382"
---

# <a name="target-typed-new-expressions"></a><span data-ttu-id="6c67e-101">Hedef türü belirlenmiş `new` ifadeleri</span><span class="sxs-lookup"><span data-stu-id="6c67e-101">Target-typed `new` expressions</span></span>

* <span data-ttu-id="6c67e-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="6c67e-102">[x] Proposed</span></span>
* <span data-ttu-id="6c67e-103">[x] [prototipi](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span><span class="sxs-lookup"><span data-stu-id="6c67e-103">[x] [Prototype](https://github.com/alrz/roslyn/tree/features/target-typed-new)</span></span>
* <span data-ttu-id="6c67e-104">[] Uygulama</span><span class="sxs-lookup"><span data-stu-id="6c67e-104">[ ] Implementation</span></span>
* <span data-ttu-id="6c67e-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="6c67e-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="6c67e-106">Özet</span><span class="sxs-lookup"><span data-stu-id="6c67e-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="6c67e-107">Tür bilindiğinde oluşturucular için tür belirtimi gerektirmeyin.</span><span class="sxs-lookup"><span data-stu-id="6c67e-107">Do not require type specification for constructors when the type is known.</span></span> 

## <a name="motivation"></a><span data-ttu-id="6c67e-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="6c67e-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="6c67e-109">Türü çoğaltmadan alan başlatılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="6c67e-109">Allow field initialization without duplicating the type.</span></span>
```cs
Dictionary<string, List<int>> field = new() {
    { "item1", new() { 1, 2, 3 } }
};
```

<span data-ttu-id="6c67e-110">Kullanımdan çıkarsanamıyor, türü dışarıda bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c67e-110">Allow omitting the type when it can be inferred from usage.</span></span>
```cs
XmlReader.Create(reader, new() { IgnoreWhitespace = true });
```

<span data-ttu-id="6c67e-111">Türü yazarken bir nesne oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6c67e-111">Instantiate an object without spelling out the type.</span></span>
```cs
private readonly static object s_syncObj = new();
```

## <a name="detailed-design"></a><span data-ttu-id="6c67e-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="6c67e-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="6c67e-113">*Object_creation_expression* sözdizimi, parantezler mevcut olduğunda *türü* isteğe bağlı hale getirmek için değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-113">The *object_creation_expression* syntax would be modified to make the *type* optional when parentheses are present.</span></span> <span data-ttu-id="6c67e-114">Bu, *anonymous_object_creation_expression*belirsizliğin ele almak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-114">This is required to address the ambiguity with *anonymous_object_creation_expression*.</span></span>
```antlr
object_creation_expression
    : 'new' type? '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;
```

<span data-ttu-id="6c67e-115">Hedef türü belirlenmiş bir `new` herhangi bir türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-115">A target-typed `new` is convertible to any type.</span></span> <span data-ttu-id="6c67e-116">Sonuç olarak, aşırı yükleme çözümüne katkıda bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="6c67e-116">As a result, it does not contribute to overload resolution.</span></span> <span data-ttu-id="6c67e-117">Bu, genellikle öngörülemeyen son değişikliklerden kaçınmaktır.</span><span class="sxs-lookup"><span data-stu-id="6c67e-117">This is mainly to avoid unpredictable breaking changes.</span></span>

<span data-ttu-id="6c67e-118">Bağımsız değişken listesi ve başlatıcı ifadeleri tür saptandıktan sonra bağlanacak.</span><span class="sxs-lookup"><span data-stu-id="6c67e-118">The argument list and the initializer expressions will be bound after the type is determined.</span></span>

<span data-ttu-id="6c67e-119">İfadenin türü, aşağıdakilerden biri olması gereken hedef türden çıkarsanamıyor:</span><span class="sxs-lookup"><span data-stu-id="6c67e-119">The type of the expression would be inferred from the target-type which would be required to be one of the following:</span></span>

- <span data-ttu-id="6c67e-120">**Herhangi bir struct türü** (demet türleri dahil)</span><span class="sxs-lookup"><span data-stu-id="6c67e-120">**Any struct type** (including tuple types)</span></span>
- <span data-ttu-id="6c67e-121">**Herhangi bir başvuru türü** (temsilci türleri dahil)</span><span class="sxs-lookup"><span data-stu-id="6c67e-121">**Any reference type** (including delegate types)</span></span>
- <span data-ttu-id="6c67e-122">Oluşturucu veya `struct` kısıtlaması olan **herhangi bir tür parametresi**</span><span class="sxs-lookup"><span data-stu-id="6c67e-122">**Any type parameter** with a constructor or a `struct` constraint</span></span>

<span data-ttu-id="6c67e-123">Aşağıdaki özel durumlarla birlikte:</span><span class="sxs-lookup"><span data-stu-id="6c67e-123">with the following exceptions:</span></span>

- <span data-ttu-id="6c67e-124">Sabit listesi türleri **:** tüm sabit listesi türleri sıfır sabiti içermez, bu nedenle açık numaralandırma üyesinin kullanılması istenebilir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-124">**Enum types:** not all enum types contain the constant zero, so it should be desirable to use the explicit enum member.</span></span>
- <span data-ttu-id="6c67e-125">**Arabirim türleri:** bu bir sunarak pazarların özelliğidir ve türün açıkça bahsetmek için tercih edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-125">**Interface types:** this is a niche feature and it should be preferable to explicitly mention the type.</span></span>
- <span data-ttu-id="6c67e-126">**Dizi türleri:** dizilerin uzunluğu sağlamak için özel bir sözdizimi olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-126">**Array types:** arrays need a special syntax to provide the length.</span></span>
- <span data-ttu-id="6c67e-127">**dinamik:** `new dynamic()`izin vermedik, bu nedenle hedef tür olarak `dynamic` `new()` izin vermedik.</span><span class="sxs-lookup"><span data-stu-id="6c67e-127">**dynamic:** we don't allow `new dynamic()`, so we don't allow `new()` with `dynamic` as a target type.</span></span>

<span data-ttu-id="6c67e-128">*Object_creation_expression* izin verilmeyen tüm diğer türler Ayrıca, örneğin işaretçi türleri hariç tutulur.</span><span class="sxs-lookup"><span data-stu-id="6c67e-128">All the other types that are not permitted in the *object_creation_expression* are excluded as well, for instance, pointer types.</span></span>

<span data-ttu-id="6c67e-129">Hedef türü null yapılabilir bir değer türü olduğunda, hedef türü belirlenmiş `new` null yapılabilir tür yerine temel alınan türe dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="6c67e-129">When the target type is a nullable value type, the target-typed `new` will be converted to the underlying type instead of the nullable type.</span></span>

> <span data-ttu-id="6c67e-130">**Açık sorun:** temsilcilerin ve tanımlama gruplarının hedef tür olarak izin vermesi gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="6c67e-130">**Open Issue:** should we allow delegates and tuples as the target-type?</span></span>

<span data-ttu-id="6c67e-131">Yukarıdaki kurallar temsilcileri (bir başvuru türü) ve tanımlama gruplarını (bir struct türü) içerir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-131">The above rules include delegates (a reference type) and tuples (a struct type).</span></span> <span data-ttu-id="6c67e-132">Her iki tür de oluşturulabilir olsa da, tür ınable ise anonim bir işlev veya bir tanımlama grubu değişmez değeri zaten kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6c67e-132">Although both types are constructible, if the type is inferable, an anonymous function or a tuple literal can already be used.</span></span>
```cs
(int a, int b) t = new(1, 2); // "new" is redundant
Action a = new(() => {}); // "new" is redundant

(int a, int b) t = new(); // ruled out by "use of struct default constructor"
Action a = new(); // no constructor found

### Miscellaneous

`throw new()` is disallowed.

Target-typed `new` is not allowed with binary operators.

It is disallowed when there is no type to target: unary operators, collection of a `foreach`, in a `using`, in a deconstruction, in an `await` expression, as an anonymous type property (`new { Prop = new() }`), in a `lock` statement, in a `sizeof`, in a `fixed` statement, in a member access (`new().field`), in a dynamically dispatched operation (`someDynamic.Method(new())`), in a LINQ query, as the operand of the `is` operator, as the left operand of the `??` operator,  ...

It is also disallowed as a `ref`.

## Drawbacks
[drawbacks]: #drawbacks

There were some concerns with target-typed `new` creating new categories of breaking changes, but we already have that with `null` and `default`, and that has not been a significant problem.

## Alternatives
[alternatives]: #alternatives

Most of complaints about types being too long to duplicate in field initialization is about *type arguments* not the type itself, we could infer only type arguments like `new Dictionary(...)` (or similar) and infer type arguments locally from arguments or the collection initializer.

## Questions
[questions]: #questions

- Should we forbid usages in expression trees? (no)
- How the feature interacts with `dynamic` arguments? (no special treatment)
- How IntelliSense should work with `new()`? (only when there is a single target-type)

## Design meetings

- [LDM-2017-10-18](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-10-18.md#100)
- [LDM-2018-05-21](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-05-21.md)
- [LDM-2018-06-25](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-06-25.md)
- [LDM-2018-08-22](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-08-22.md#target-typed-new)
- [LDM-2018-10-17](https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-10-17.md)
