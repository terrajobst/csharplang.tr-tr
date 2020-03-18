---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484988"
---
# <a name="target-typed-default-literal"></a><span data-ttu-id="e5c89-101">Target türü "default" değişmez değeri</span><span class="sxs-lookup"><span data-stu-id="e5c89-101">Target-typed "default" literal</span></span>

* <span data-ttu-id="e5c89-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="e5c89-102">[x] Proposed</span></span>
* <span data-ttu-id="e5c89-103">[x] prototipi</span><span class="sxs-lookup"><span data-stu-id="e5c89-103">[x] Prototype</span></span>
* <span data-ttu-id="e5c89-104">[x] uygulama</span><span class="sxs-lookup"><span data-stu-id="e5c89-104">[x] Implementation</span></span>
* <span data-ttu-id="e5c89-105">[] Belirtimi</span><span class="sxs-lookup"><span data-stu-id="e5c89-105">[ ] Specification</span></span>

## <a name="summary"></a><span data-ttu-id="e5c89-106">Özet</span><span class="sxs-lookup"><span data-stu-id="e5c89-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e5c89-107">Hedef türü belirlenmiş `default` özelliği, türün Atlanmasının izin verdiği `default(T)` işlecinin daha kısa bir çeşitlemesi türüdür.</span><span class="sxs-lookup"><span data-stu-id="e5c89-107">The target-typed `default` feature is a shorter form variation of the `default(T)` operator, which allows the type to be omitted.</span></span> <span data-ttu-id="e5c89-108">Bunun yerine, türü hedef yazma tarafından algılanır.</span><span class="sxs-lookup"><span data-stu-id="e5c89-108">Its type is inferred by target-typing instead.</span></span> <span data-ttu-id="e5c89-109">Bunun yanında, `default(T)`gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="e5c89-109">Aside from that, it behaves like `default(T)`.</span></span>

## <a name="motivation"></a><span data-ttu-id="e5c89-110">Amacı</span><span class="sxs-lookup"><span data-stu-id="e5c89-110">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e5c89-111">Ana işlem, gereksiz bilgiler yazmamaktır.</span><span class="sxs-lookup"><span data-stu-id="e5c89-111">The main motivation is to avoid typing redundant information.</span></span>

<span data-ttu-id="e5c89-112">Örneğin, `void Method(ImmutableArray<SomeType> array)`çağrılırken *varsayılan* değişmez değer `M(default(ImmutableArray<SomeType>))`yerine `M(default)` izin verir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-112">For instance, when invoking `void Method(ImmutableArray<SomeType> array)`, the *default* literal allows `M(default)` in place of `M(default(ImmutableArray<SomeType>))`.</span></span>

<span data-ttu-id="e5c89-113">Bu, aşağıdaki gibi çeşitli senaryolarda geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="e5c89-113">This is applicable in a number of scenarios, such as:</span></span>

- <span data-ttu-id="e5c89-114">yerelleri bildirme (`ImmutableArray<SomeType> x = default;`)</span><span class="sxs-lookup"><span data-stu-id="e5c89-114">declaring locals (`ImmutableArray<SomeType> x = default;`)</span></span>
- <span data-ttu-id="e5c89-115">Üçlü işlemler (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span><span class="sxs-lookup"><span data-stu-id="e5c89-115">ternary operations (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)</span></span>
- <span data-ttu-id="e5c89-116">yöntemlere ve lambdalara dönme (`return default;`)</span><span class="sxs-lookup"><span data-stu-id="e5c89-116">returning in methods and lambdas (`return default;`)</span></span>
- <span data-ttu-id="e5c89-117">isteğe bağlı parametreler için varsayılan değerleri bildirme (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span><span class="sxs-lookup"><span data-stu-id="e5c89-117">declaring default values for optional parameters (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)</span></span>
- <span data-ttu-id="e5c89-118">dizi oluşturma ifadelerinde varsayılan değerleri ekleme (`var x = new[] { default, ImmutableArray.Create(y) };`)</span><span class="sxs-lookup"><span data-stu-id="e5c89-118">including default values in array creation expressions (`var x = new[] { default, ImmutableArray.Create(y) };`)</span></span>


## <a name="detailed-design"></a><span data-ttu-id="e5c89-119">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="e5c89-119">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="e5c89-120">*Varsayılan* değişmez değer olan yeni bir ifade tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="e5c89-120">A new expression is introduced, the *default* literal.</span></span> <span data-ttu-id="e5c89-121">Bu sınıflandırmayla bir ifade, *varsayılan değişmez değer dönüşümünden*örtük olarak herhangi bir türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-121">An expression with this classification can be implicitly converted to any type, by a *default literal conversion*.</span></span> 

<span data-ttu-id="e5c89-122">*Varsayılan* değişmez değerin çıkarımı, herhangi bir türe izin verilmesi (yalnızca başvuru türleri değil) dışında, *null* değişmez değeri ile aynı şekilde çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="e5c89-122">The inference of the type for the *default* literal works the same as that for the *null* literal, except that any type is allowed (not just reference types).</span></span>

<span data-ttu-id="e5c89-123">Bu dönüştürme, çıkarılan türün varsayılan değerini üretir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-123">This conversion produces the default value of the inferred type.</span></span>

<span data-ttu-id="e5c89-124">*Varsayılan* değişmez değer, çıkarılan türe bağlı olarak sabit bir değere sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-124">The *default* literal may have a constant value, depending on the inferred type.</span></span> <span data-ttu-id="e5c89-125">`const int x = default;` geçerlidir, ancak `const int? y = default;` değildir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-125">So `const int x = default;` is legal, but `const int? y = default;` is not.</span></span>

<span data-ttu-id="e5c89-126">*Varsayılan* değişmez değer, diğer işlenenin bir türü olduğu sürece eşitlik işleçlerinin işleneni olabilir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-126">The *default* literal can be the operand of equality operators, as long as the other operand has a type.</span></span> <span data-ttu-id="e5c89-127">`default == x` ve `x == default` geçerli ifadelerdir, ancak `default == default` geçersizdir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-127">So `default == x` and `x == default` are valid expressions, but `default == default` is illegal.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="e5c89-128">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="e5c89-128">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="e5c89-129">Küçük bir sakıncası, çoğu bağlamdaki *null* sabit değer yerine *varsayılan* değişmez değer kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-129">A minor drawback is that *default* literal can be used in place of *null* literal in most contexts.</span></span> <span data-ttu-id="e5c89-130">Özel durumların ikisi `throw null;` ve `null == null`, ancak *varsayılan* değişmez değer değil, *null* değişmez değer için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e5c89-130">Two of the exceptions are `throw null;` and `null == null`, which are allowed for the *null* literal, but not the *default* literal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e5c89-131">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="e5c89-131">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e5c89-132">Göz önünde bulundurulması gereken birkaç seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="e5c89-132">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="e5c89-133">Durum Quo: Özellik kendi birleşme üzerinde hizalı değil ve geliştiriciler, varsayılan işleci açık bir türle kullanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="e5c89-133">The status quo:  The feature is not justified on its own merits and developers continue to use the default operator with an explicit type.</span></span>
- <span data-ttu-id="e5c89-134">Null değişmez değer genişletiliyor: Bu, `Nothing`VB yaklaşımıdır.</span><span class="sxs-lookup"><span data-stu-id="e5c89-134">Extending the null literal: This is the VB approach with `Nothing`.</span></span> <span data-ttu-id="e5c89-135">`int x = null;`izin vereceğiz.</span><span class="sxs-lookup"><span data-stu-id="e5c89-135">We could allow `int x = null;`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="e5c89-136">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="e5c89-136">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="e5c89-137">[x], ya da işleç *olarak* *varsayılan* olarak izin verilmelidir *mi?*</span><span class="sxs-lookup"><span data-stu-id="e5c89-137">[x] Should *default* be allowed as the operand of the *is* or *as* operators?</span></span> <span data-ttu-id="e5c89-138">Cevap: izin verme `default is T`, izin verme `x is default`, `default as RefType` izin verme (her zaman null uyarılarla)</span><span class="sxs-lookup"><span data-stu-id="e5c89-138">Answer:  disallow `default is T`, allow `x is default`, allow `default as RefType` (with always-null warning)</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e5c89-139">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="e5c89-139">Design meetings</span></span>

- [<span data-ttu-id="e5c89-140">LDM 3/7/2017</span><span class="sxs-lookup"><span data-stu-id="e5c89-140">LDM 3/7/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [<span data-ttu-id="e5c89-141">LDM 3/28/2017</span><span class="sxs-lookup"><span data-stu-id="e5c89-141">LDM 3/28/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [<span data-ttu-id="e5c89-142">LDM 5/31/2017</span><span class="sxs-lookup"><span data-stu-id="e5c89-142">LDM 5/31/2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
