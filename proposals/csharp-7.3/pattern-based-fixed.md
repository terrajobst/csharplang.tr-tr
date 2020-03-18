---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484659"
---
# <a name="pattern-based-fixed-statement"></a><span data-ttu-id="2847b-101">Desen tabanlı `fixed` deyimi</span><span class="sxs-lookup"><span data-stu-id="2847b-101">Pattern-based `fixed` statement</span></span>

## <a name="summary"></a><span data-ttu-id="2847b-102">Özet</span><span class="sxs-lookup"><span data-stu-id="2847b-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="2847b-103">Türlerin `fixed` ifadelerine katılmasına izin veren bir model ortaya çıkarabilir.</span><span class="sxs-lookup"><span data-stu-id="2847b-103">Introduce a pattern that would allow types to participate in `fixed` statements.</span></span> 

## <a name="motivation"></a><span data-ttu-id="2847b-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="2847b-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="2847b-105">Dil, yönetilen verileri sabitleme ve temel alınan arabelleğe yerel bir işaretçi alma mekanizması sağlar.</span><span class="sxs-lookup"><span data-stu-id="2847b-105">The language provides a mechanism for pinning managed data and obtain a native pointer to the underlying buffer.</span></span>

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

<span data-ttu-id="2847b-106">`fixed` katılabilen türler kümesi sabit kodlanmış ve dizilerle ve `System.String`sınırlıdır.</span><span class="sxs-lookup"><span data-stu-id="2847b-106">The set of types that can participate in `fixed` is hardcoded and limited to arrays and `System.String`.</span></span> <span data-ttu-id="2847b-107">"Özel" türler, `ImmutableArray<T>`, `Span<T>``Utf8String` gibi yeni temel öğeler tanıtıldığında ölçeklenmez.</span><span class="sxs-lookup"><span data-stu-id="2847b-107">Hardcoding "special" types does not scale when new primitives such as `ImmutableArray<T>`, `Span<T>`, `Utf8String` are introduced.</span></span> 

<span data-ttu-id="2847b-108">Ayrıca, `System.String` için geçerli çözüm bir oldukça rigıd API 'sine dayanır.</span><span class="sxs-lookup"><span data-stu-id="2847b-108">In addition, the current solution for `System.String` relies on a fairly rigid API.</span></span> <span data-ttu-id="2847b-109">API 'nin şekli, `System.String`, UTF16 kodlu verileri nesne başlığından sabit bir uzaklığa katıştıran bitişik bir nesne olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="2847b-109">The shape of the API implies that `System.String` is a contiguous object that embeds UTF16 encoded data at a fixed offset from the object header.</span></span> <span data-ttu-id="2847b-110">Bu yaklaşım, temeldeki düzende değişiklik gerektirebilecek çeşitli tekliflerde sorun bulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2847b-110">Such approach has been found problematic in several proposals that could require changes to the underlying layout.</span></span> <span data-ttu-id="2847b-111">Yönetilmeyen birlikte çalışma amacına göre `System.String` nesnesini iç gösteriminden ayırarak daha esnek bir şekilde geçebilmek istenebilir.</span><span class="sxs-lookup"><span data-stu-id="2847b-111">It would be desirable to be able to switch to something more flexible that decouples `System.String` object from its internal representation for the purpose of unmanaged interop.</span></span> 

## <a name="detailed-design"></a><span data-ttu-id="2847b-112">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="2847b-112">Detailed design</span></span>
[design]: #detailed-design

## <a name="pattern"></a><span data-ttu-id="2847b-113">*Desen*</span><span class="sxs-lookup"><span data-stu-id="2847b-113">*Pattern*</span></span> ##
<span data-ttu-id="2847b-114">Uygun bir model tabanlı "fixed" gereksinimi şunlar için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="2847b-114">A viable pattern-based “fixed” need to:</span></span>
-   <span data-ttu-id="2847b-115">Örneği sabitlemek için yönetilen başvuruları sağlayın ve işaretçiyi başlatın (tercihen bu aynı başvurudur)</span><span class="sxs-lookup"><span data-stu-id="2847b-115">Provide the managed references to pin the instance and to initialize the pointer (preferably this is the same reference)</span></span>
-   <span data-ttu-id="2847b-116">Yönetilmeyen öğenin türünü ("String" için "Char") kesin bir şekilde iletin</span><span class="sxs-lookup"><span data-stu-id="2847b-116">Convey unambiguously the type of the unmanaged element   (i.e. “char” for “string”)</span></span>
-   <span data-ttu-id="2847b-117">Başvurabileceğiniz bir şey olmadığında "boş" durumunda davranış göstermelidir.</span><span class="sxs-lookup"><span data-stu-id="2847b-117">Prescribe the behavior in "empty" case when there is nothing to refer to.</span></span> 
-   <span data-ttu-id="2847b-118">API yazarları `fixed`dışında türün kullanımını çıkarmayan tasarım kararlarına doğru göndermemelidir.</span><span class="sxs-lookup"><span data-stu-id="2847b-118">Should not push API authors toward design decisions that hurt the use of the type outside of `fixed`.</span></span>

<span data-ttu-id="2847b-119">Yukarıdaki, özel olarak adlandırılmış bir başvuru döndüren üyeyi (`ref [readonly] T GetPinnableReference()`) tanımayı memnun hale getiriyorum.</span><span class="sxs-lookup"><span data-stu-id="2847b-119">I think the above could be satisfied by recognizing a specially named ref-returning member: `ref [readonly] T GetPinnableReference()`.</span></span>

<span data-ttu-id="2847b-120">`fixed` ifadesinin kullanılabilmesi için aşağıdaki koşulların karşılanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="2847b-120">In order to be used by the `fixed` statement the following conditions must be met:</span></span>

1. <span data-ttu-id="2847b-121">Bir tür için yalnızca bir tane belirtilen üye vardır.</span><span class="sxs-lookup"><span data-stu-id="2847b-121">There is only one such member provided for a type.</span></span>
1. <span data-ttu-id="2847b-122">`ref` veya `ref readonly`tarafından döndürülür.</span><span class="sxs-lookup"><span data-stu-id="2847b-122">Returns by `ref` or `ref readonly`.</span></span> <span data-ttu-id="2847b-123">(`readonly`, sabit/salt okunur türlerin yazarlarına, güvenli kodda kullanılabilecek yazılabilir API eklemeden bir model uygulayabilmesi için izin verilir)</span><span class="sxs-lookup"><span data-stu-id="2847b-123">(`readonly` is permitted so that authors of immutable/readonly types could implement the pattern without adding writeable API that could be used in safe code)</span></span>
1. <span data-ttu-id="2847b-124">T, yönetilmeyen bir türdür.</span><span class="sxs-lookup"><span data-stu-id="2847b-124">T is an unmanaged type.</span></span>
<span data-ttu-id="2847b-125">(`T*` işaretçi türü haline gelir.</span><span class="sxs-lookup"><span data-stu-id="2847b-125">(since `T*` becomes the pointer type.</span></span> <span data-ttu-id="2847b-126">Kısıtlama, "yönetilmeyen" kavramı genişletilmişse doğal olarak genişletilir.</span><span class="sxs-lookup"><span data-stu-id="2847b-126">The restriction will naturally expand if/when the notion of "unmanaged" is expanded)</span></span>
1. <span data-ttu-id="2847b-127">Sabitlemeye veri olmadığında yönetilen `nullptr` döndürür; büyük olasılıkla emptiyi hale getirmenin bir yolu.</span><span class="sxs-lookup"><span data-stu-id="2847b-127">Returns managed `nullptr` when there is no data to pin – probably the cheapest way to convey emptiness.</span></span>
<span data-ttu-id="2847b-128">(dizeler null sonlandırıldığı için "" dizesinin ' \ 0 ' öğesine bir başvuru döndürdüğünü unutmayın)</span><span class="sxs-lookup"><span data-stu-id="2847b-128">(note that “” string returns a ref to '\0' since strings are null-terminated)</span></span>

<span data-ttu-id="2847b-129">`#3` için alternatif olarak, boş çalışmalardaki sonucun tanımsız veya uygulamaya özgü olması için izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="2847b-129">Alternatively for the `#3` we can allow the result in empty cases be undefined or implementation-specific.</span></span> <span data-ttu-id="2847b-130">Bununla birlikte, API 'nin daha tehlikeli ve uygunsuz uyumluluk ve istenmeyen uyumlulukta olması olabilir.</span><span class="sxs-lookup"><span data-stu-id="2847b-130">That, however, may make the API more dangerous and prone to abuse and unintended compatibility burdens.</span></span> 

## <a name="translation"></a><span data-ttu-id="2847b-131">*Çeviri*</span><span class="sxs-lookup"><span data-stu-id="2847b-131">*Translation*</span></span> ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

<span data-ttu-id="2847b-132">Aşağıdaki sözde kod haline gelir (tüm ifade edilemez C#)</span><span class="sxs-lookup"><span data-stu-id="2847b-132">becomes the following pseudocode (not all expressible in C#)</span></span>

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a><span data-ttu-id="2847b-133">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="2847b-133">Drawbacks</span></span>
[drawbacks]: #drawbacks

- <span data-ttu-id="2847b-134">GetPinnableReference yalnızca `fixed`için kullanılmak üzere tasarlanmıştır, ancak güvenli kodda kullanımını hiçbir şey engellemez, bu yüzden uygulayıcıyı göz önünde bulundurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2847b-134">GetPinnableReference is intended to be used only in `fixed`, but nothing prevents its use in safe code, so implementor must keep that in mind.</span></span>

## <a name="alternatives"></a><span data-ttu-id="2847b-135">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="2847b-135">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="2847b-136">Kullanıcılar GetPinnableReference veya benzeri bir üyeyi ortaya çıkarabilir ve bunu şu şekilde kullanabilir</span><span class="sxs-lookup"><span data-stu-id="2847b-136">Users can introduce GetPinnableReference or similar member and use it as</span></span>
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

<span data-ttu-id="2847b-137">Alternatif çözüm isteniyorsa `System.String` için çözüm yoktur.</span><span class="sxs-lookup"><span data-stu-id="2847b-137">There is no solution for `System.String` if alternative solution is desired.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="2847b-138">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="2847b-138">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="2847b-139">[] "Boş" durumundaki davranış.</span><span class="sxs-lookup"><span data-stu-id="2847b-139">[ ] Behavior in "empty" state.</span></span><span data-ttu-id="2847b-140">`nullptr` veya `undefined`  - ?</span><span class="sxs-lookup"><span data-stu-id="2847b-140"> - `nullptr` or `undefined` ?</span></span> 
- <span data-ttu-id="2847b-141">[] Uzantı yöntemleri göz önünde bulundurulmalıdır mi?</span><span class="sxs-lookup"><span data-stu-id="2847b-141">[ ] Should the extension methods be considered ?</span></span> 
- <span data-ttu-id="2847b-142">[] `System.String`üzerinde bir model algılanırsa, bunun yerine gelmelidir mi?</span><span class="sxs-lookup"><span data-stu-id="2847b-142">[ ] If a pattern is detected on `System.String`, should it win over ?</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="2847b-143">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="2847b-143">Design meetings</span></span>

<span data-ttu-id="2847b-144">Henüz yok.</span><span class="sxs-lookup"><span data-stu-id="2847b-144">None yet.</span></span> 
