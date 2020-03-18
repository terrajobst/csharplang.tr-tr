---
ms.openlocfilehash: 032cb8590a0b6e83f8ab6201e10720f1b254c605
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484533"
---
# <a name="nullable-enhanced-common-type"></a><span data-ttu-id="a7125-101">Null yapılabilir-gelişmiş ortak tür</span><span class="sxs-lookup"><span data-stu-id="a7125-101">Nullable-Enhanced Common Type</span></span>

* <span data-ttu-id="a7125-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="a7125-102">[x] Proposed</span></span>
* <span data-ttu-id="a7125-103">[] Prototipi: yok</span><span class="sxs-lookup"><span data-stu-id="a7125-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="a7125-104">[] Uygulama: yok</span><span class="sxs-lookup"><span data-stu-id="a7125-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="a7125-105">[] Belirtimi: aşağıya bakın</span><span class="sxs-lookup"><span data-stu-id="a7125-105">[ ] Specification: See below</span></span>

## <a name="summary"></a><span data-ttu-id="a7125-106">Özet</span><span class="sxs-lookup"><span data-stu-id="a7125-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a7125-107">Geçerli ortak tür algoritması sonuçlarının sayaç sezgisel olduğu bir durum vardır ve programcının koda gereksiz bir şekilde saçılması gibi ne kadar fekine bir ekleme işlemi yaptığını ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="a7125-107">There is a situation in which the current common-type algorithm results are counter-intuitive, and results in the programmer adding what feels like a redundant cast to the code.</span></span> <span data-ttu-id="a7125-108">Bu değişiklik ile `condition ? 1 : null` gibi bir ifade `int?`türünde bir değer ve `x` `condition ? x : 1.0` gibi bir ifade `int?` türünde bir değere neden olur.`double?`</span><span class="sxs-lookup"><span data-stu-id="a7125-108">With this change, an expression such as `condition ? 1 : null` would result in a value of type `int?`, and an expression such as `condition ? x : 1.0` where `x` is of type `int?` would result in a value of type `double?`.</span></span>

## <a name="motivation"></a><span data-ttu-id="a7125-109">Amacı</span><span class="sxs-lookup"><span data-stu-id="a7125-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a7125-110">Bu, programcının gereksiz bir demirbaş kodu gibi ne kadar çok daha yaygın bir nedendir.</span><span class="sxs-lookup"><span data-stu-id="a7125-110">This is a common cause of what feels to the programmer like needless boilerplate code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a7125-111">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="a7125-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a7125-112">Aşağıdaki durumları etkilemek için [bir dizi ifadenin en iyi ortak türünü bulma](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) belirtimini değiştirdik:</span><span class="sxs-lookup"><span data-stu-id="a7125-112">We modify the specification for [finding the best common type of a set of expressions](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) to affect the following situations:</span></span>

- <span data-ttu-id="a7125-113">Bir ifade null yapılamayan bir değer türü `T` ve diğeri null bir sabit değerdir, sonuç `T?`türündedir.</span><span class="sxs-lookup"><span data-stu-id="a7125-113">If one expression is of a non-nullable value type `T` and the other is a null literal, the result is of type `T?`.</span></span>
- <span data-ttu-id="a7125-114">Bir ifade null yapılabilir bir değer türü `T?` ve diğeri bir değer türü `U`ve `U``T` ' den örtük bir dönüştürme varsa, sonuç `U?`türüdür.</span><span class="sxs-lookup"><span data-stu-id="a7125-114">If one expression is of a nullable value type `T?` and the other is of a value type `U`, and there is an implicit conversion from `T` to `U`, then the result is of type `U?`.</span></span>

<span data-ttu-id="a7125-115">Bu, dilin aşağıdaki yönlerini etkilemek için beklenir:</span><span class="sxs-lookup"><span data-stu-id="a7125-115">This is expected to affect the following aspects of the language:</span></span>

- <span data-ttu-id="a7125-116">[Üçlü ifade](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span><span class="sxs-lookup"><span data-stu-id="a7125-116">the [ternary expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#conditional-operator)</span></span>
- <span data-ttu-id="a7125-117">örtük olarak yazılan [dizi oluşturma ifadesi](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span><span class="sxs-lookup"><span data-stu-id="a7125-117">implicitly typed [array creation expression](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#array-creation-expressions)</span></span>
- <span data-ttu-id="a7125-118">Tür çıkarımı için [lambdanın dönüş türünü](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) erteleme</span><span class="sxs-lookup"><span data-stu-id="a7125-118">inferring the [return type of a lambda](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#inferred-return-type) for type inference</span></span>
- <span data-ttu-id="a7125-119">`M(1, null)`olarak `M<T>(T a, T b)` çağırma gibi genel türler içeren durumlar.</span><span class="sxs-lookup"><span data-stu-id="a7125-119">cases involving generics, such as invoking `M<T>(T a, T b)` as `M(1, null)`.</span></span>

<span data-ttu-id="a7125-120">Daha kesin olarak, belirtimin aşağıdaki bölümlerini (kalın yazı tipiyle eklenenler, üstü çizili) değiştiririz:</span><span class="sxs-lookup"><span data-stu-id="a7125-120">More precisely, we change the following sections of the specification (insertions in bold, deletions in strikethrough):</span></span>

> #### <a name="output-type-inferences"></a><span data-ttu-id="a7125-121">Çıkış türü ında</span><span class="sxs-lookup"><span data-stu-id="a7125-121">Output type inferences</span></span>
> 
> <span data-ttu-id="a7125-122">Aşağıdaki şekilde *`E` bir ifadeden* *bir tür* `T` bir *Çıkış türü çıkarımı* yapılır:</span><span class="sxs-lookup"><span data-stu-id="a7125-122">An *output type inference* is made *from* an expression `E` *to* a type `T` in the following way:</span></span>
> 
> *  <span data-ttu-id="a7125-123">`E`, `U` çıkartılan dönüş türü olan bir anonim işlevse[(çıkartılan dönüş türü](expressions.md#inferred-return-type)) ve `T`, dönüş türü `Tb`olan bir temsilci türü veya ifade ağacı türü ise *, `U` ile `Tb`* *arasında* daha *düşük bağlantılı bir çıkarım* ([alt sınır](expressions.md#lower-bound-inferences)) yapılır.</span><span class="sxs-lookup"><span data-stu-id="a7125-123">If `E` is an anonymous function with inferred return type  `U` ([Inferred return type](expressions.md#inferred-return-type)) and `T` is a delegate type or expression tree type with return type `Tb`, then a *lower-bound inference* ([Lower-bound inferences](expressions.md#lower-bound-inferences)) is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="a7125-124">Aksi takdirde, `E` bir yöntem grubu ise ve `T`, parametre türleri `T1...Tk` ve dönüş türü `Tb`olan bir temsilci türü ya da ifade ağacı türü `E` ve `T1...Tk` bir `U`dönüş türü ile tek bir yöntem ortaya çıkardı `U` *, `Tb`ile* arasında *daha düşük* bir çıkarma yapılır. *from*</span><span class="sxs-lookup"><span data-stu-id="a7125-124">Otherwise, if `E` is a method group and `T` is a delegate type or expression tree type with parameter types `T1...Tk` and return type `Tb`, and overload resolution of `E` with the types `T1...Tk` yields a single method with return type `U`, then a *lower-bound inference* is made *from* `U` *to* `Tb`.</span></span>
> *  <span data-ttu-id="a7125-125">\* \* Aksi takdirde, `E` Nullable değer türü `U?`olan bir ifadedir, *daha* *sonra `U` '* *den* `T` ve null bir *sınır* `T`eklenir.</span><span class="sxs-lookup"><span data-stu-id="a7125-125">\*\*Otherwise, if `E` is an expression with nullable value type `U?`, then a *lower-bound inference* is made *from* `U` *to* `T` and a *null bound* is added to `T`.</span></span> **
> *  <span data-ttu-id="a7125-126">Aksi takdirde, `E` `U`türünde bir ifade ise, `U` ' *den* `T` *'ye* *daha düşük bir çıkarım* yapılır.</span><span class="sxs-lookup"><span data-stu-id="a7125-126">Otherwise, if `E` is an expression with type `U`, then a *lower-bound inference* is made *from* `U` *to* `T`.</span></span>
> *  <span data-ttu-id="a7125-127">**Aksi takdirde, `E` `null`değeri olan sabit bir ifade ise, null bir *sınırı* `T` eklenir**</span><span class="sxs-lookup"><span data-stu-id="a7125-127">**Otherwise, if `E` is a constant expression with value `null`, then a *null bound* is added to `T`**</span></span> 
> *  <span data-ttu-id="a7125-128">Aksi takdirde, hiçbir Inse yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="a7125-128">Otherwise, no inferences are made.</span></span>

> #### <a name="fixing"></a><span data-ttu-id="a7125-129">Düzelttikten</span><span class="sxs-lookup"><span data-stu-id="a7125-129">Fixing</span></span>
> 
> <span data-ttu-id="a7125-130">Bir dizi sınırı olan `Xi` *sabit olmayan* bir tür değişkeni aşağıdaki gibi *düzeltilir* :</span><span class="sxs-lookup"><span data-stu-id="a7125-130">An *unfixed* type variable `Xi` with a set of bounds is *fixed* as follows:</span></span>
> 
> *  <span data-ttu-id="a7125-131">`Uj` *aday türleri* kümesi, `Xi`sınırları kümesindeki tüm türlerin kümesi olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a7125-131">The set of *candidate types* `Uj` starts out as the set of all types in the set of bounds for `Xi`.</span></span>
> *  <span data-ttu-id="a7125-132">Sonra her bir `Xi` için her birini sırayla inceliyoruz `U`: `U` aynı olmayan tüm türler `Uj` `Xi` aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="a7125-132">We then examine each bound for `Xi` in turn: For each exact bound `U` of `Xi` all types `Uj` which are not identical to `U` are removed from the candidate set.</span></span> <span data-ttu-id="a7125-133">Tüm `Xi` `U` her bir alt sınır için `U` örtük bir dönüştürme *olmadığı* `Uj` tüm türler için aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="a7125-133">For each lower bound `U` of `Xi` all types `Uj` to which there is *not* an implicit conversion from `U` are removed from the candidate set.</span></span> <span data-ttu-id="a7125-134">`U` örtük bir dönüştürme *olmadığı* `Uj` tüm türlerin `Xi` `U` her üst sınır için aday kümesinden kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="a7125-134">For each upper bound `U` of `Xi` all types `Uj` from which there is *not* an implicit conversion to `U` are removed from the candidate set.</span></span>
> *  <span data-ttu-id="a7125-135">Kalan aday türleri arasında `Uj`, tüm diğer aday türlerine örtük bir dönüştürme olan `V` benzersiz bir tür vardır ve ~~`Xi` `V`için düzeltilir.~~</span><span class="sxs-lookup"><span data-stu-id="a7125-135">If among the remaining candidate types `Uj` there is a unique type `V` from which there is an implicit conversion to all the other candidate types, then ~~`Xi` is fixed to `V`.~~</span></span>
>     -  <span data-ttu-id="a7125-136">**`V` bir değer türüdür ve `Xi`için bir *null sınırı* varsa `Xi` düzeltilir `V?`**</span><span class="sxs-lookup"><span data-stu-id="a7125-136">**If `V` is a value type and there is a *null bound* for `Xi`, then `Xi` is fixed to `V?`**</span></span>
>     -  <span data-ttu-id="a7125-137">**Aksi takdirde `Xi` `V` için düzeltilir**</span><span class="sxs-lookup"><span data-stu-id="a7125-137">**Otherwise   `Xi` is fixed to `V`**</span></span>
> *  <span data-ttu-id="a7125-138">Aksi takdirde tür çıkarımı başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a7125-138">Otherwise, type inference fails.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a7125-139">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="a7125-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a7125-140">Bu teklif tarafından sunulan bazı uyumsuzluklar olabilir.</span><span class="sxs-lookup"><span data-stu-id="a7125-140">There may be some incompatibilities introduced by this proposal.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a7125-141">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="a7125-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a7125-142">Yok.</span><span class="sxs-lookup"><span data-stu-id="a7125-142">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a7125-143">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="a7125-143">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="a7125-144">[] Bu teklifin tanıtılmasıyla ilgili uyumsuzluğun önem derecesi nedir ve nasıl dağıtılabilir?</span><span class="sxs-lookup"><span data-stu-id="a7125-144">[ ] What is the severity of incompatibility introduced by this proposal, if any, and how can it be moderated?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a7125-145">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="a7125-145">Design meetings</span></span>

<span data-ttu-id="a7125-146">Yok.</span><span class="sxs-lookup"><span data-stu-id="a7125-146">None.</span></span>
