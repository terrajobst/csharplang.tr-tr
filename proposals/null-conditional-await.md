---
ms.openlocfilehash: 3d10cacef98e802333c8cbe65edb93c19c74cabf
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484498"
---
# <a name="null-conditional-await"></a><span data-ttu-id="eea8b-101">null-koşullu await</span><span class="sxs-lookup"><span data-stu-id="eea8b-101">null-conditional await</span></span>

* <span data-ttu-id="eea8b-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="eea8b-102">[x] Proposed</span></span>
* <span data-ttu-id="eea8b-103">[] Prototipi: yok</span><span class="sxs-lookup"><span data-stu-id="eea8b-103">[ ] Prototype: None</span></span>
* <span data-ttu-id="eea8b-104">[] Uygulama: yok</span><span class="sxs-lookup"><span data-stu-id="eea8b-104">[ ] Implementation: None</span></span>
* <span data-ttu-id="eea8b-105">[] Belirtim: başlatıldı, aşağıdan</span><span class="sxs-lookup"><span data-stu-id="eea8b-105">[ ] Specification: Started, below</span></span>

## <a name="summary"></a><span data-ttu-id="eea8b-106">Özet</span><span class="sxs-lookup"><span data-stu-id="eea8b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="eea8b-107">`await? e`form ifadesini destekler, bu, null olmayan bir `e` bekler, aksi takdirde `null`sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="eea8b-107">Support an expression of the form `await? e`, which awaits `e` if it is non-null, otherwise it results in `null`.</span></span>

## <a name="motivation"></a><span data-ttu-id="eea8b-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="eea8b-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="eea8b-109">Bu ortak bir kodlama modelidir ve bu özellik, var olan null yayan ve null birleşim işleçleriyle iyi bir şekilde eşitleniyor.</span><span class="sxs-lookup"><span data-stu-id="eea8b-109">This is a common coding pattern, and this feature would have nice synergy with the existing null-propagating and null-coalescing operators.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="eea8b-110">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="eea8b-110">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="eea8b-111">*Await_expression*yeni bir form ekleyeceğiz:</span><span class="sxs-lookup"><span data-stu-id="eea8b-111">We add a new form of the *await_expression*:</span></span>

```antlr
await_expression
    : 'await' '?' unary_expression
    ;
```

<span data-ttu-id="eea8b-112">Null koşullu `await` işleci, yalnızca bu işlenen null olmadığında işleneni bekler.</span><span class="sxs-lookup"><span data-stu-id="eea8b-112">The null-conditional `await` operator awaits its operand only if that operand is non-null.</span></span> <span data-ttu-id="eea8b-113">Aksi takdirde işleci uygulamanın sonucu null olur.</span><span class="sxs-lookup"><span data-stu-id="eea8b-113">Otherwise the result of applying the operator is null.</span></span>

<span data-ttu-id="eea8b-114">Sonucun türü, [null koşullu işlecin kuralları](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator)kullanılarak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="eea8b-114">The type of the result is computed using the [rules for the null-conditional operator](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#null-conditional-operator).</span></span>

> <span data-ttu-id="eea8b-115">**Note:** `e` `Task`tür ise, `e` `null``await? e;` hiçbir şey yapmaz ve bu `e` `null`bekler.</span><span class="sxs-lookup"><span data-stu-id="eea8b-115">**NOTE:** If `e` is of type `Task`, then `await? e;` would do nothing if `e` is `null`, and await `e` if it is not `null`.</span></span>
>
> <span data-ttu-id="eea8b-116">`e`, `K` bir değer türü olduğu `Task<K>` tür ise, `await? e` `K?`türünde bir değer verir.</span><span class="sxs-lookup"><span data-stu-id="eea8b-116">If `e` is of type `Task<K>` where `K` is a value type, then `await? e` would yield a value of type `K?`.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="eea8b-117">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="eea8b-117">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="eea8b-118">Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.</span><span class="sxs-lookup"><span data-stu-id="eea8b-118">As with any language feature, we must question whether the additional complexity to the language is repaid in the additional clarity offered to the body of C# programs that would benefit from the feature.</span></span>

## <a name="alternatives"></a><span data-ttu-id="eea8b-119">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="eea8b-119">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="eea8b-120">Bazı demirbaş kod gerektirse de, bu işlecin kullanımları genellikle `(e == null) ? null : await e` veya `if (e != null) await e`gibi bir deyim ile değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="eea8b-120">Although it requires some boilerplate code, uses of this operator can often be replaced by an expression something like `(e == null) ? null : await e` or a statement like `if (e != null) await e`.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="eea8b-121">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="eea8b-121">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="eea8b-122">[] LDM incelemesi gerektiriyor</span><span class="sxs-lookup"><span data-stu-id="eea8b-122">[ ] Requires LDM review</span></span>

## <a name="design-meetings"></a><span data-ttu-id="eea8b-123">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="eea8b-123">Design meetings</span></span>

<span data-ttu-id="eea8b-124">Yok.</span><span class="sxs-lookup"><span data-stu-id="eea8b-124">None.</span></span>
