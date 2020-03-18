---
ms.openlocfilehash: fdd858c895d56a7a6b410e6ea7be3032e4851fd6
ms.sourcegitcommit: 5a88d5432d32c690c6b870fc4be32cf26cadd76f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2019
ms.locfileid: "79484960"
---
# <a name="null-coalescing-assignment"></a><span data-ttu-id="a9178-101">null birleşim ataması</span><span class="sxs-lookup"><span data-stu-id="a9178-101">null coalescing assignment</span></span>

* <span data-ttu-id="a9178-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="a9178-102">[x] Proposed</span></span>
* <span data-ttu-id="a9178-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="a9178-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="a9178-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="a9178-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="a9178-105">[] Belirtimi: aşağıda</span><span class="sxs-lookup"><span data-stu-id="a9178-105">[ ] Specification: Below</span></span>

## <a name="summary"></a><span data-ttu-id="a9178-106">Özet</span><span class="sxs-lookup"><span data-stu-id="a9178-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a9178-107">Null ise bir değişkene bir değer atandığında ortak bir kodlama modelini basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="a9178-107">Simplifies a common coding pattern where a variable is assigned a value if it is null.</span></span>

<span data-ttu-id="a9178-108">Bu teklifin bir parçası olarak, türü, sol tarafta kullanılacak kısıtlanmış olmayan bir tür parametresi olan bir ifadeye izin vermek için `??` tür gereksinimlerini da gevyoruz.</span><span class="sxs-lookup"><span data-stu-id="a9178-108">As part of this proposal, we will also loosen the type requirements on `??` to allow an expression whose type is an unconstrained type parameter to be used on the left-hand side.</span></span>

## <a name="motivation"></a><span data-ttu-id="a9178-109">Amacı</span><span class="sxs-lookup"><span data-stu-id="a9178-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a9178-110">Formun kodunu görmek yaygındır.</span><span class="sxs-lookup"><span data-stu-id="a9178-110">It is common to see code of the form</span></span>

```csharp
if (variable == null)
{
    variable = expression;
}
```

<span data-ttu-id="a9178-111">Bu teklif, bu işlevi gerçekleştiren dile aşırı yüklenebilir olmayan bir ikili işleç ekler.</span><span class="sxs-lookup"><span data-stu-id="a9178-111">This proposal adds a non-overloadable binary operator to the language that performs this function.</span></span>

<span data-ttu-id="a9178-112">Bu özellik için en az sekiz ayrı topluluk isteği vardı.</span><span class="sxs-lookup"><span data-stu-id="a9178-112">There have been at least eight separate community requests for this feature.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a9178-113">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="a9178-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a9178-114">Yeni bir atama operatörü formu ekledik</span><span class="sxs-lookup"><span data-stu-id="a9178-114">We add a new form of assignment operator</span></span>

``` antlr
assignment_operator
    : '??='
    ;
```

<span data-ttu-id="a9178-115">[Bileşik atama işleçleri için var olan anlam kurallarını](../../spec/expressions.md#compound-assignment)izleyen bu, sol taraftaki boş değer değilse atamayı tam olarak gerçekleştirmemiz dışında.</span><span class="sxs-lookup"><span data-stu-id="a9178-115">Which follows the [existing semantic rules for compound assignment operators](../../spec/expressions.md#compound-assignment), except that we elide the assignment if the left-hand side is non-null.</span></span> <span data-ttu-id="a9178-116">Bu özelliğin kuralları aşağıdaki gibidir.</span><span class="sxs-lookup"><span data-stu-id="a9178-116">The rules for this feature are as follows.</span></span>

<span data-ttu-id="a9178-117">`a ??= b`, `A` `a`türüdür, `B` `b`türüdür ve `A0` null yapılabilir bir değer türü ise `A` temel türüdür:`A`</span><span class="sxs-lookup"><span data-stu-id="a9178-117">Given `a ??= b`, where `A` is the type of `a`, `B` is the type of `b`, and `A0` is the underlying type of `A` if `A` is a nullable value type:</span></span>

1. <span data-ttu-id="a9178-118">`A` yoksa veya null yapılamayan bir değer türü ise, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a9178-118">If `A` does not exist or is a non-nullable value type, a compile-time error occurs.</span></span>
2. <span data-ttu-id="a9178-119">`B` örtük olarak `A` veya `A0` 'e dönüştürülemez (`A0` varsa), bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a9178-119">If `B` is not implicitly convertible to `A` or `A0` (if `A0` exists), a compile-time error occurs.</span></span>
3. <span data-ttu-id="a9178-120">`A0` varsa ve `B` örtük olarak `A0`'e dönüştürülebilir ve `B` dinamik değilse, `a ??= b` türü `A0`olur.</span><span class="sxs-lookup"><span data-stu-id="a9178-120">If `A0` exists and `B` is implicitly convertible to `A0`, and `B` is not dynamic, then the type of `a ??= b` is `A0`.</span></span> <span data-ttu-id="a9178-121">`a ??= b` çalışma zamanında şu şekilde değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="a9178-121">`a ??= b` is evaluated at runtime as:</span></span>
   ```C#
   var tmp = a.GetValueOrDefault();
   if (!a.HasValue) { tmp = b; a = tmp; }
   tmp
   ```
   <span data-ttu-id="a9178-122">`a` hariç, yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a9178-122">Except that `a` is only evaluated once.</span></span>
4. <span data-ttu-id="a9178-123">Aksi takdirde, `a ??= b` türü `A`.</span><span class="sxs-lookup"><span data-stu-id="a9178-123">Otherwise, the type of `a ??= b` is `A`.</span></span> <span data-ttu-id="a9178-124">`a ??= b` çalışma zamanında `a ?? (a = b)`olarak değerlendirilir, ancak `a` yalnızca bir kez değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a9178-124">`a ??= b` is evaluated at runtime as `a ?? (a = b)`, except that `a` is only evaluated once.</span></span>


<span data-ttu-id="a9178-125">`??`'nin tür gereksinimlerinin bir kısmını, `a ?? b`verilen, `A` `a`türü olduğu belirtilen şekilde güncelleştirmekte olan belirtimi güncelleştiririz:</span><span class="sxs-lookup"><span data-stu-id="a9178-125">For the relaxation of the type requirements of `??`, we update the spec where it currently states that, given `a ?? b`, where `A` is the type of `a`:</span></span>

> 1. <span data-ttu-id="a9178-126">Varsa ve null yapılabilir bir tür ya da bir başvuru türü değilse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a9178-126">If A exists and is not a nullable type or a reference type, a compile-time error occurs.</span></span>

<span data-ttu-id="a9178-127">Bu gereksinimi şu şekilde rahatlıyoruz:</span><span class="sxs-lookup"><span data-stu-id="a9178-127">We relax this requirement to:</span></span>

1. <span data-ttu-id="a9178-128">Bir varsa ve null yapılamayan bir değer türü ise, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="a9178-128">If A exists and is a non-nullable value type, a compile-time error occurs.</span></span>

<span data-ttu-id="a9178-129">Bu, sınırlandırılmamış tür parametresi var olduğundan, null olabilen tür parametreleri üzerinde çalışır, ancak bir başvuru türü değildir.</span><span class="sxs-lookup"><span data-stu-id="a9178-129">This allows the null coalescing operator to work on unconstrained type parameters, as the unconstrained type parameter T exists, is not a nullable type, and is not a reference type.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="a9178-130">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="a9178-130">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a9178-131">Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9178-131">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a9178-132">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="a9178-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a9178-133">Programcı `(x = x ?? y)`, `if (x == null) x = y;`veya `x ?? (x = y)` el ile yazabilir.</span><span class="sxs-lookup"><span data-stu-id="a9178-133">The programmer can write `(x = x ?? y)`, `if (x == null) x = y;`, or `x ?? (x = y)` by hand.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="a9178-134">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="a9178-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="a9178-135">[] LDM incelemesi gerektiriyor</span><span class="sxs-lookup"><span data-stu-id="a9178-135">[ ] Requires LDM review</span></span>
- <span data-ttu-id="a9178-136">[] `&&=` ve `||=` işleçlerini da destekliyoruz.</span><span class="sxs-lookup"><span data-stu-id="a9178-136">[ ] Should we also support `&&=` and `||=` operators?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="a9178-137">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="a9178-137">Design meetings</span></span>

<span data-ttu-id="a9178-138">Yok.</span><span class="sxs-lookup"><span data-stu-id="a9178-138">None.</span></span>
