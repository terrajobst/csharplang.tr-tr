---
ms.openlocfilehash: 392d932459ff0a4cb0d6d32c1606f73f9b913c68
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484456"
---
# <a name="covariant-return-types"></a><span data-ttu-id="5de2d-101">covaryant dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="5de2d-101">covariant return types</span></span>

* <span data-ttu-id="5de2d-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="5de2d-102">[x] Proposed</span></span>
* <span data-ttu-id="5de2d-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="5de2d-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="5de2d-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="5de2d-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="5de2d-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="5de2d-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="5de2d-106">Özet</span><span class="sxs-lookup"><span data-stu-id="5de2d-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="5de2d-107">_Birlikte değişken dönüş türlerini_destekler.</span><span class="sxs-lookup"><span data-stu-id="5de2d-107">Support _covariant return types_.</span></span> <span data-ttu-id="5de2d-108">Özellikle, geçersiz kılan bir metodun, geçersiz kıldıkları yöntemden daha fazla türetilmiş başvuru türüne sahip olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5de2d-108">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span>

## <a name="motivation"></a><span data-ttu-id="5de2d-109">Amacı</span><span class="sxs-lookup"><span data-stu-id="5de2d-109">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5de2d-110">Bu, kod içinde farklı yöntem adlarının, geçersiz kılınan yöntemle aynı türü döndürmesi gereken dil kısıtlaması etrafında çalışmak üzere oluşturulması gereken yaygın bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="5de2d-110">It is a common pattern in code that different method names have to be invented to work around the language constraint that overrides must return the same type as the overridden method.</span></span> <span data-ttu-id="5de2d-111">Roslyn kod tabanından bir örnek için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="5de2d-111">See below for an example from the Roslyn code base.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="5de2d-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="5de2d-112">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5de2d-113">_Birlikte değişken dönüş türlerini_destekler.</span><span class="sxs-lookup"><span data-stu-id="5de2d-113">Support _covariant return types_.</span></span> <span data-ttu-id="5de2d-114">Özellikle, geçersiz kılan bir metodun, geçersiz kıldıkları yöntemden daha fazla türetilmiş başvuru türüne sahip olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5de2d-114">Specifically, allow an overriding method to have a more derived reference type than the method it overrides.</span></span> <span data-ttu-id="5de2d-115">Bu, Yöntemler ve özellikler için geçerlidir ve sınıflar ve arabirimlerde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="5de2d-115">This would apply to methods and properties, and be supported in classes and interfaces.</span></span>

<span data-ttu-id="5de2d-116">Bu, fabrika düzeninde yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="5de2d-116">This would be useful in the factory pattern.</span></span> <span data-ttu-id="5de2d-117">Örneğin, Roslyn kod tabanında</span><span class="sxs-lookup"><span data-stu-id="5de2d-117">For example, in the Roslyn code base we would have</span></span>

``` cs
class Compilation ...
{
    virtual Compilation WithOptions(Options options)...
}
```

``` cs
class CSharpCompilation : Compilation
{
    override CSharpCompilation WithOptions(Options options)...
}
```

<span data-ttu-id="5de2d-118">Bu, derleyicinin geçersiz kılma yöntemini temel sınıf yöntemini gizleyen bir "yeni" sanal yöntemi olarak, türetilmiş sınıf yöntemi çağrısıyla birlikte temel sınıf yöntemini uygulayan bir _köprü yöntemiyle_ yaymasıdır.</span><span class="sxs-lookup"><span data-stu-id="5de2d-118">The implementation of this would be for the compiler to emit the overriding method as a "new" virtual method that hides the base class method, along with a _bridge method_ that implements the base class method with a call to the derived class method.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="5de2d-119">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="5de2d-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="5de2d-120">[] Her dil değişikliğinin kendisi için ödeme yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5de2d-120">[ ] Every language change must pay for itself.</span></span>
- <span data-ttu-id="5de2d-121">[] Derin devralma hiyerarşileri durumunda bile performansın makul olduğundan emin olunması gerekir</span><span class="sxs-lookup"><span data-stu-id="5de2d-121">[ ] We should ensure that the performance is reasonable, even in the case of deep inheritance hierarchies</span></span>
- <span data-ttu-id="5de2d-122">[] Eski derleyicilerden yeni Il kullanılırken bile, çeviri stratejisinin yapıtlarının dil semantiğini etkilemediğinden emin olunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5de2d-122">[ ] We should ensure that artifacts of the translation strategy do not affect language semantics, even when consuming new IL from old compilers.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5de2d-123">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="5de2d-123">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5de2d-124">Kaynak olarak, dil kurallarını izin vermek için biraz daha rahat olabilir</span><span class="sxs-lookup"><span data-stu-id="5de2d-124">We could relax the language rules slightly to allow, in source,</span></span>

```csharp
abstract class Cloneable
{
    public abstract Cloneable Clone();
}

class Digit : Cloneable
{
    public override Cloneable Clone()
    {
        return this.Clone();
    }

    public new Digit Clone() // Error: 'Digit' already defines a member called 'Clone' with the same parameter types
    {
        return this;
    }
}
```

## <a name="unresolved-questions"></a><span data-ttu-id="5de2d-125">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="5de2d-125">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="5de2d-126">[] Bu özelliği kullanmak için derlenen API 'Ler, dilin eski sürümlerinde çalışır mı?</span><span class="sxs-lookup"><span data-stu-id="5de2d-126">[ ] How will APIs that have been compiled to use this feature work in older versions of the language?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="5de2d-127">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="5de2d-127">Design meetings</span></span>

<span data-ttu-id="5de2d-128">Henüz yok.</span><span class="sxs-lookup"><span data-stu-id="5de2d-128">None yet.</span></span> <span data-ttu-id="5de2d-129"><https://github.com/dotnet/roslyn/issues/357>konusunda bazı tartışmalar vardı.</span><span class="sxs-lookup"><span data-stu-id="5de2d-129">There has been some discussion at <https://github.com/dotnet/roslyn/issues/357>.</span></span>