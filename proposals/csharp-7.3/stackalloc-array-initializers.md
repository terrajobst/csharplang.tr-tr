---
ms.openlocfilehash: 7ea62713416ef82034963aef06f3cb11703342ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484610"
---
# <a name="stackalloc-array-initializers"></a><span data-ttu-id="fe7c7-101">Stackalloc dizi başlatıcılar</span><span class="sxs-lookup"><span data-stu-id="fe7c7-101">Stackalloc array initializers</span></span>

## <a name="summary"></a><span data-ttu-id="fe7c7-102">Özet</span><span class="sxs-lookup"><span data-stu-id="fe7c7-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="fe7c7-103">Dizi başlatıcısı sözdiziminin `stackalloc`birlikte kullanılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-103">Allow array initializer syntax to be used with `stackalloc`.</span></span>

## <a name="motivation"></a><span data-ttu-id="fe7c7-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="fe7c7-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="fe7c7-105">Sıradan diziler, öğeleri oluşturma sırasında başlatılabilir.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-105">Ordinary arrays can have their elements initialized at creation time.</span></span> <span data-ttu-id="fe7c7-106">`stackalloc` durumunda buna izin vermek mantıklı bir şekilde görünür.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-106">It seems reasonable to allow that in `stackalloc` case.</span></span>

<span data-ttu-id="fe7c7-107">Bu tür sözdiziminin neden çok sık `stackalloc` izin verilmediğine ilişkin sorunuz.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-107">The question of why such syntax is not allowed with `stackalloc` arises fairly frequently.</span></span>  
<span data-ttu-id="fe7c7-108">Bkz. Örneğin, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span><span class="sxs-lookup"><span data-stu-id="fe7c7-108">See, for example, [#1112](https://github.com/dotnet/csharplang/issues/1112)</span></span>

## <a name="detailed-design"></a><span data-ttu-id="fe7c7-109">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="fe7c7-109">Detailed design</span></span>

<span data-ttu-id="fe7c7-110">Sıradan diziler aşağıdaki sözdizimi kullanılarak oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="fe7c7-110">Ordinary arrays can be created through the following syntax:</span></span>

```csharp
new int[3]
new int[3] { 1, 2, 3 }
new int[] { 1, 2, 3 }
new[] { 1, 2, 3 }
```

<span data-ttu-id="fe7c7-111">Yığın ayrılmış dizilerinin şu şekilde oluşturulmasını izin vereceğiz:</span><span class="sxs-lookup"><span data-stu-id="fe7c7-111">We should allow stack allocated arrays be created through:</span></span>  

```csharp
stackalloc int[3]               // currently allowed
stackalloc int[3] { 1, 2, 3 }
stackalloc int[] { 1, 2, 3 }
stackalloc[] { 1, 2, 3 }
```

<span data-ttu-id="fe7c7-112">Tüm durumların semantiği, dizilerle aynı şekilde kabaca aynıdır.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-112">The semantics of all cases is roughly the same as with arrays.</span></span>  
<span data-ttu-id="fe7c7-113">Örneğin: öğe türünün başlatıcıdan çıkarılmış olması ve "yönetilmeyen" bir tür olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-113">For example: in the last case the element type is inferred from the initializer and must be an "unmanaged" type.</span></span>

<span data-ttu-id="fe7c7-114">Not: Özellik hedefe bağlı değil `Span<T>`.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-114">NOTE: the feature is not dependent on the target being a `Span<T>`.</span></span> <span data-ttu-id="fe7c7-115">`T*` durumda olduğu gibi bir durumdur. bu nedenle, `Span<T>` durumunda bu durum koşulunu sağlamak mantıklı görünmüyor.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-115">It is just as applicable in `T*` case, so it does not seem reasonable to predicate it on `Span<T>` case.</span></span>  

## <a name="translation"></a><span data-ttu-id="fe7c7-116">Çeviri</span><span class="sxs-lookup"><span data-stu-id="fe7c7-116">Translation</span></span>

<span data-ttu-id="fe7c7-117">Naïve uygulama, diziyi bir dizi öğe temelinde atamalar aracılığıyla oluşturmadan hemen başlatabilir.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-117">The naive implementation could just initialize the array right after creation through a series of element-wise assignments.</span></span>  

<span data-ttu-id="fe7c7-118">Ayrıca, diziler ile aynı şekilde, öğelerin tümünün veya çoğunun blittable türler olduğu ve tüm sabit öğelerin önceden oluşturulmuş durumuna kopyalanarak daha verimli teknikler kullanıldığı durumları algılamak mümkün olabilir ve istenebilir.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-118">Similarly to the case with arrays, it might be possible and desirable to detect cases where all or most of the elements are blittable types and use more efficient techniques by copying over the pre-created state of all the constant elements.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="fe7c7-119">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="fe7c7-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

## <a name="alternatives"></a><span data-ttu-id="fe7c7-120">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="fe7c7-120">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="fe7c7-121">Bu bir kullanışlı özelliktir.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-121">This is a convenience feature.</span></span> <span data-ttu-id="fe7c7-122">Yalnızca bir şey yapmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-122">It is possible to just do nothing.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="fe7c7-123">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="fe7c7-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="fe7c7-124">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="fe7c7-124">Design meetings</span></span>

<span data-ttu-id="fe7c7-125">Henüz yok.</span><span class="sxs-lookup"><span data-stu-id="fe7c7-125">None yet.</span></span> 
