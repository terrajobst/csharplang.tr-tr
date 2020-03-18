---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484589"
---
# <a name="fixed-sized-buffers"></a><span data-ttu-id="e6893-101">Sabit boyutlu arabellekler</span><span class="sxs-lookup"><span data-stu-id="e6893-101">Fixed Sized Buffers</span></span>

* <span data-ttu-id="e6893-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="e6893-102">[x] Proposed</span></span>
* <span data-ttu-id="e6893-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="e6893-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="e6893-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="e6893-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="e6893-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="e6893-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="e6893-106">Özet</span><span class="sxs-lookup"><span data-stu-id="e6893-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="e6893-107">C# Dile sabit boyutlu arabellekler bildirmek için genel amaçlı ve güvenli bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="e6893-107">Provide a general-purpose and safe mechanism for declaring fixed sized buffers to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="e6893-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="e6893-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="e6893-109">Günümüzde, kullanıcıların güvenli olmayan bağlamdaki sabit boyutlu arabellekler oluşturma yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="e6893-109">Today, users have the ability to create fixed-sized buffers in an unsafe-context.</span></span> <span data-ttu-id="e6893-110">Bununla birlikte, kullanıcının işaretçilerle uğraşmak, el ile sınır denetimleri gerçekleştirmesi ve yalnızca sınırlı bir tür kümesini (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`ve `double`) desteklemesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e6893-110">However, this requires the user to deal with pointers, manually perform bounds checks, and only supports a limited set of types (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`, and `double`).</span></span>

<span data-ttu-id="e6893-111">En yaygın şikayet, sabit boyutlu arabelleklerin güvenli kodda dizinlenemez.</span><span class="sxs-lookup"><span data-stu-id="e6893-111">The most common complaint is that fixed-size buffers cannot be indexed in safe code.</span></span> <span data-ttu-id="e6893-112">Daha fazla tür kullanmamamasından dolayı ikincisi.</span><span class="sxs-lookup"><span data-stu-id="e6893-112">Inability to use more types is the second.</span></span>

<span data-ttu-id="e6893-113">Birkaç küçük temks sayesinde, herhangi bir türü destekleyen, güvenli bir bağlamda kullanılabilen ve otomatik sınır denetimi gerçekleştirilen genel amaçlı sabit boyutlu arabellekler sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="e6893-113">With a few minor tweaks, we could provide general-purpose fixed-sized buffers which support any type, can be used in a safe context, and have automatic bounds checking performed.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="e6893-114">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="e6893-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="e6893-115">Bunlardan biri, aşağıdakiler aracılığıyla güvenli bir sabit boyutlu arabellek bildirir:</span><span class="sxs-lookup"><span data-stu-id="e6893-115">One would declare a safe fixed-sized buffer via the following:</span></span>

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

<span data-ttu-id="e6893-116">Bildirim, aşağıdaki örneğe benzer şekilde derleyici tarafından bir iç gösterimine çevrilir</span><span class="sxs-lookup"><span data-stu-id="e6893-116">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

<span data-ttu-id="e6893-117">Sabit boyutlu arabellekler artık `fixed`kullanımını gerektirmediğinden, herhangi bir öğe türüne izin vermek mantıklı olur.</span><span class="sxs-lookup"><span data-stu-id="e6893-117">Since such fixed-sized buffers no longer require use of `fixed`, it makes sense to allow any element type.</span></span>  

> <span data-ttu-id="e6893-118">NOTE: `fixed` hala desteklenmeye devam eder, ancak yalnızca öğe türü `blittable`</span><span class="sxs-lookup"><span data-stu-id="e6893-118">NOTE: `fixed` will still be supported, but only if the element type is `blittable`</span></span>

## <a name="drawbacks"></a><span data-ttu-id="e6893-119">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="e6893-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

* <span data-ttu-id="e6893-120">Geriye dönük uyumlulukla ilgili bazı sorunlar olabilir, ancak var olan sabit boyutlu arabelleklerin yalnızca temel türler seçimiyle birlikte çalışması durumunda, kullanıcının sabit arabelleği bir olarak kabul etmesi durumunda derleyicinin "yalnızca çalışır" olması gerekir. çağrısı.</span><span class="sxs-lookup"><span data-stu-id="e6893-120">There could be some challenges with backwards compatibility, but given that the existing fixed-sized buffers only work with a selection of primitive types, it should be possible for the compiler to continue "just-working" if the user treats the fixed-buffer as a pointer.</span></span>
* <span data-ttu-id="e6893-121">Uyumsuz yapıların eski derleyicisinden alanları gizlemek için biraz farklı `v2` kodlaması kullanması gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="e6893-121">Incompatible constructs may need to use slightly different `v2` encoding to hide the fields from old compiler.</span></span>
* <span data-ttu-id="e6893-122">Paket, genel türler için Il belirtiminde iyi tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="e6893-122">Packing is not well defined in IL spec for generic types.</span></span> <span data-ttu-id="e6893-123">Yaklaşım çalışırken, belgelenmemiş davranışta bordering olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e6893-123">While the approach should work, we will be bordering on undocumented behavior.</span></span> <span data-ttu-id="e6893-124">Belgelenir ve benzer Monode aynı davranışa sahip olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="e6893-124">We should make that documented and make sure other JITs like Mono have the same behavior.</span></span>
* <span data-ttu-id="e6893-125">Her uzunluk için ayrı bir tür belirtme (destekleniyorsa `readonly` alanları için bir diğeri) meta veriler üzerinde etki olur.</span><span class="sxs-lookup"><span data-stu-id="e6893-125">Specifying a separate type for every length (an possibly another for `readonly` fields, if supported) will have impact on metadata.</span></span> <span data-ttu-id="e6893-126">Belirtilen uygulamadaki farklı boyutlardaki dizi dizilerinin altına göre bağlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="e6893-126">It will be bound by the number of arrays of different sizes in the given app.</span></span>
* <span data-ttu-id="e6893-127">`ref` matematik bir şekilde doğrulanabilir değil (güvenli olmadığı için).</span><span class="sxs-lookup"><span data-stu-id="e6893-127">`ref` math is not formally verifiable (since it is unsafe).</span></span> <span data-ttu-id="e6893-128">Kullanımızın tamam olduğunu anlamak için doğrulama kurallarını güncelleştirmenin bir yolunu bulmamız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6893-128">We will need to find a way to update verification rules to know that our use is ok.</span></span>

## <a name="alternatives"></a><span data-ttu-id="e6893-129">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="e6893-129">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="e6893-130">Yapılarınızı el ile bildirin ve Dizin oluşturucular oluşturmak için güvenli olmayan kod kullanın.</span><span class="sxs-lookup"><span data-stu-id="e6893-130">Manually declare your structures and use unsafe code to construct indexers.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="e6893-131">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="e6893-131">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="e6893-132">`readonly`izin vermemiz gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="e6893-132">should we allow `readonly`?</span></span>  <span data-ttu-id="e6893-133">(ReadOnly Dizin Oluşturucu ile)</span><span class="sxs-lookup"><span data-stu-id="e6893-133">(with readonly indexer)</span></span>
- <span data-ttu-id="e6893-134">dizi başlatıcılara izin vermemiz gerekir mi?</span><span class="sxs-lookup"><span data-stu-id="e6893-134">should we allow array initializers?</span></span>
- <span data-ttu-id="e6893-135">`fixed` anahtar sözcüğü gerekli mi?</span><span class="sxs-lookup"><span data-stu-id="e6893-135">is `fixed` keyword necessary?</span></span>
- <span data-ttu-id="e6893-136">`foreach`?</span><span class="sxs-lookup"><span data-stu-id="e6893-136">`foreach`?</span></span>
- <span data-ttu-id="e6893-137">yalnızca yapılarda örnek alanlar mı?</span><span class="sxs-lookup"><span data-stu-id="e6893-137">only instance fields in structs?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="e6893-138">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="e6893-138">Design meetings</span></span>

<span data-ttu-id="e6893-139">Bu teklifi etkileyen tasarım notlarının bağlantısını yapın ve hangi değişiklikler üzerinde olduğuna ilişkin bir tümcede açıklama yapın.</span><span class="sxs-lookup"><span data-stu-id="e6893-139">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>