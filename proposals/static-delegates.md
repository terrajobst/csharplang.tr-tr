---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484477"
---
# <a name="static-delegates"></a><span data-ttu-id="61c31-101">Statik temsilciler</span><span class="sxs-lookup"><span data-stu-id="61c31-101">Static Delegates</span></span>

* <span data-ttu-id="61c31-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="61c31-102">[x] Proposed</span></span>
* <span data-ttu-id="61c31-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="61c31-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="61c31-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="61c31-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="61c31-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="61c31-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="61c31-106">Özet</span><span class="sxs-lookup"><span data-stu-id="61c31-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="61c31-107">C# Dile genel amaçlı, hafif bir geri çağırma özelliği sağlayın.</span><span class="sxs-lookup"><span data-stu-id="61c31-107">Provide a general-purpose, lightweight callback capability to the C# language.</span></span>

## <a name="motivation"></a><span data-ttu-id="61c31-108">Amacı</span><span class="sxs-lookup"><span data-stu-id="61c31-108">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="61c31-109">Günümüzde, kullanıcıların `System.Delegate` türünü kullanarak geri çağrılar oluşturma yeteneği vardır.</span><span class="sxs-lookup"><span data-stu-id="61c31-109">Today, users have the ability to create callbacks using the `System.Delegate` type.</span></span> <span data-ttu-id="61c31-110">Ancak bunlar oldukça ağır (yığın ayırmayı gerektirme ve zincirleme geri çağırmaların birlikte her zaman işleme için).</span><span class="sxs-lookup"><span data-stu-id="61c31-110">However, these are fairly heavyweight (such as requiring a heap allocation and always having handling for chaining callbacks together).</span></span>

<span data-ttu-id="61c31-111">Ayrıca, `System.Delegate` yönetilmeyen işlev işaretçileriyle en iyi birlikte çalışabilirliği sağlamaz, bunun nedeni, blittable olmayan ve yönetilen/yönetilmeyen sınırın kesiştiği her zaman sıralama gerektirmektir.</span><span class="sxs-lookup"><span data-stu-id="61c31-111">Additionally, `System.Delegate` does not provide the best interop with unmanaged function pointers, namely due being non-blittable and requiring marshalling anytime it crosses the managed/unmanaged boundary.</span></span>

<span data-ttu-id="61c31-112">Birkaç küçük tnak ile, yerel kod ile hafif, genel amaçlı ve işlem temelli yeni bir temsilci türü sağlayabiliriz.</span><span class="sxs-lookup"><span data-stu-id="61c31-112">With a few minor tweaks, we could provide a new type of delegate that is lightweight, general-purpose, and interops well with native code.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="61c31-113">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="61c31-113">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="61c31-114">Biri, aşağıdaki aracılığıyla bir statik temsilci bildirir:</span><span class="sxs-lookup"><span data-stu-id="61c31-114">One would declare a static delegate via the following:</span></span>

```C#
static delegate int Func()
```

<span data-ttu-id="61c31-115">Bunun yanı sıra, çağrı kuralının, dize uzayları ve ayarlanan son hata davranışının denetlenebilmesi için `System.Runtime.InteropServices.UnmanagedFunctionPointer` benzer bir şekilde bildirime öznitelik ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="61c31-115">One could additionally attribute the declaration with something similar to `System.Runtime.InteropServices.UnmanagedFunctionPointer` so that the calling convention, string marshalling, and set last error behavior can be controlled.</span></span> <span data-ttu-id="61c31-116">Not: yalnızca Temsilcilerde kullanılabilir olduğundan `System.Runtime.InteropServices.UnmanagedFunctionPointer` kendisini kullanmak işe alınacaktır.</span><span class="sxs-lookup"><span data-stu-id="61c31-116">NOTE: Using `System.Runtime.InteropServices.UnmanagedFunctionPointer` itself will not work, as it is only usable on Delegates.</span></span>

<span data-ttu-id="61c31-117">Bildirim, aşağıdaki örneğe benzer şekilde derleyici tarafından bir iç gösterimine çevrilir</span><span class="sxs-lookup"><span data-stu-id="61c31-117">The declaration would get translated into an internal representation by the compiler that is similar to the following</span></span>

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

<span data-ttu-id="61c31-118">Yani, `IntPtr` türünde tek bir üyeye sahip bir struct tarafından temsil edilir (böyle bir struct blittable ve herhangi bir yığın ayırması uygulamaz):</span><span class="sxs-lookup"><span data-stu-id="61c31-118">That is to say, it is internally represented by a struct that has a single member of type `IntPtr` (such a struct is blittable and does not incur any heap allocations):</span></span>
* <span data-ttu-id="61c31-119">Üye, geri çağırma olacak işlevin adresini içerir.</span><span class="sxs-lookup"><span data-stu-id="61c31-119">The member contains the address of the function that is to be the callback.</span></span>
* <span data-ttu-id="61c31-120">Tür, geri aramanın Yöntem imzasıyla eşleşen bir yöntem bildirir.</span><span class="sxs-lookup"><span data-stu-id="61c31-120">The type declares a method matching the method signature of the callback.</span></span>
* <span data-ttu-id="61c31-121">Yapının adı Kullanıcı tarafından oluşturulabilir (diğer dahili yedekleme yapıları ile yaptığımız gibi).</span><span class="sxs-lookup"><span data-stu-id="61c31-121">The name of the struct should not be user-constructible (as we do with other internally generated backing structures).</span></span>
 * <span data-ttu-id="61c31-122">Örneğin: sabit boyutlu arabellekler `<name>e__FixedBuffer` biçiminde bir ada sahip bir struct oluşturur (`<` ve `>` tanımlayıcının bir parçasıdır ve tanımlayıcı, hala Il 'de oluşturulabilir C#).</span><span class="sxs-lookup"><span data-stu-id="61c31-122">For example: fixed size buffers generate a struct with a name in the format of `<name>e__FixedBuffer` (`<` and `>` are part of the identifier and make the identifier not constructible in C#, but still useable in IL).</span></span>
* <span data-ttu-id="61c31-123">Yöntem bildiriminin adı, tüm statik temsilci türlerinde kullanılan iyi bilinen bir ad olmalıdır (Bu, derleyicinin imzayı belirlerken bakabileceği adı bilmesini sağlar).</span><span class="sxs-lookup"><span data-stu-id="61c31-123">The name of the method declaration should be a well known name used across all static delegate types (this allows the compiler to know the name to look for when determining the signature).</span></span>

<span data-ttu-id="61c31-124">Statik temsilcinin değeri yalnızca geri aramanın imzasıyla eşleşen bir statik metoda bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="61c31-124">The value of the static delegate can only be bound to a static method that matches the signature of the callback.</span></span>

<span data-ttu-id="61c31-125">Birlikte geri çağırmaları birlikte zincirleme desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="61c31-125">Chaining callbacks together is not supported.</span></span>

<span data-ttu-id="61c31-126">Geri aramanın çağrılması `calli` yönergesi tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="61c31-126">Invocation of the callback would be implemented by the `calli` instruction.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="61c31-127">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="61c31-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="61c31-128">Statik temsilciler, normal temsilciler kullanan var olan API 'Ler ile çalışmaz (bir diğeri aynı imzaya sahip olan normal bir temsilciyle aynı statik temsilciyi sarmalıdır).</span><span class="sxs-lookup"><span data-stu-id="61c31-128">Static Delegates would not work with existing APIs that use regular delegates (one would need to wrap said static delegate in a regular delegate of the same signature).</span></span>
* <span data-ttu-id="61c31-129">`System.Delegate`, `object` ve `IntPtr` alanları kümesi olarak dahili olarak temsil edildiği için (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), statik bir temsilcinin eşleşen Yöntem imzasına sahip bir `System.Delegate` örtük dönüştürülmesine izin vermek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="61c31-129">Given that `System.Delegate` is represented internally as a set of `object` and `IntPtr` fields (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), it would be possible to allow implicit conversion of a static delegate to a `System.Delegate` that has a matching method signature.</span></span> <span data-ttu-id="61c31-130">Ayrıca, bir statik temsilci olan tüm gereksinimlere `System.Delegate` sağlandığı için ters yönde açık bir dönüştürme sağlamak mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="61c31-130">It would also be possible to provide an explicit conversion in the opposite direction, provided the `System.Delegate` conformed to all the requirements of being a static delegate.</span></span>

<span data-ttu-id="61c31-131">Statik temsilciyi çekirdek çerçevede kullanıma hazır hale getirmek için ek çalışma gerekir.</span><span class="sxs-lookup"><span data-stu-id="61c31-131">Additional work would be needed to make Static Delegate readily usable in the core framework.</span></span>

## <a name="alternatives"></a><span data-ttu-id="61c31-132">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="61c31-132">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="61c31-133">TBD</span><span class="sxs-lookup"><span data-stu-id="61c31-133">TBD</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="61c31-134">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="61c31-134">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="61c31-135">Tasarımın bölümleri yine de TBD midir?</span><span class="sxs-lookup"><span data-stu-id="61c31-135">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="61c31-136">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="61c31-136">Design meetings</span></span>

<span data-ttu-id="61c31-137">Bu teklifi etkileyen tasarım notlarının bağlantısını yapın ve hangi değişiklikler üzerinde olduğuna ilişkin bir tümcede açıklama yapın.</span><span class="sxs-lookup"><span data-stu-id="61c31-137">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>


