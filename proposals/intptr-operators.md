---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484561"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a><span data-ttu-id="aec7b-101">İşleçler `System.IntPtr` ve `System.UIntPtr` için sunulmalıdır</span><span class="sxs-lookup"><span data-stu-id="aec7b-101">Operators should be exposed for `System.IntPtr` and `System.UIntPtr`</span></span>

* <span data-ttu-id="aec7b-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="aec7b-102">[x] Proposed</span></span>
* <span data-ttu-id="aec7b-103">[] Prototipi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="aec7b-103">[ ] Prototype: Not Started</span></span>
* <span data-ttu-id="aec7b-104">[] Uygulama: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="aec7b-104">[ ] Implementation: Not Started</span></span>
* <span data-ttu-id="aec7b-105">[] Belirtimi: başlatılmadı</span><span class="sxs-lookup"><span data-stu-id="aec7b-105">[ ] Specification: Not Started</span></span>

## <a name="summary"></a><span data-ttu-id="aec7b-106">Özet</span><span class="sxs-lookup"><span data-stu-id="aec7b-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="aec7b-107">CLR, `System.IntPtr` ve `System.UIntPtr` türleri (`native int`) için bir işleçler kümesini destekler.</span><span class="sxs-lookup"><span data-stu-id="aec7b-107">The CLR supports a set of operators for the `System.IntPtr` and `System.UIntPtr` types (`native int`).</span></span> <span data-ttu-id="aec7b-108">Bu işleçler, ortak dil altyapısı belirtiminin (`ECMA-335`) `III.1.5` görünebilirler.</span><span class="sxs-lookup"><span data-stu-id="aec7b-108">These operators can be seen in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="aec7b-109">Ancak, bu işleçler tarafından C#desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="aec7b-109">However, these operators are not supported by C#.</span></span>

<span data-ttu-id="aec7b-110">`System.IntPtr` ve `System.UIntPtr`tarafından desteklenen işleçlerin tam kümesi için dil desteği sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aec7b-110">Language support should be provided for the full set of operators supported by `System.IntPtr` and `System.UIntPtr`.</span></span> <span data-ttu-id="aec7b-111">Bu işleçler şunlardır: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span><span class="sxs-lookup"><span data-stu-id="aec7b-111">These operators are: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.</span></span>

## <a name="motivation"></a><span data-ttu-id="aec7b-112">Amacı</span><span class="sxs-lookup"><span data-stu-id="aec7b-112">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="aec7b-113">Günümüzde, kullanıcılar çeşitli araçları ve C# çerçeveleri kullanarak birden çok platformu hedefleyen uygulamaları kolayca yazabilir: `Xamarin`, `.NET Core`, `Mono`, vb...</span><span class="sxs-lookup"><span data-stu-id="aec7b-113">Today, users can easily write C# applications targeting multiple platforms using various tools and frameworks, such as: `Xamarin`, `.NET Core`, `Mono`, etc...</span></span>

<span data-ttu-id="aec7b-114">Platformlar arası kod yazarken, genellikle belirli bir hedef platformla etkileşim kuran birlikte çalışma kodunu belirli bir şekilde yazmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-114">When writing cross-platform code, it is often necessary to write interop code that interacts with a particular target platform in a specific manner.</span></span> <span data-ttu-id="aec7b-115">Bu, grafik kodu yazmayı, bazı sistem API 'sini çağırmayı veya var olan bir yerel kitaplıkla etkileşim kurmayı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-115">This could include writing graphics code, calling some System API, or interacting with an existing native library.</span></span>

<span data-ttu-id="aec7b-116">Bu birlikte çalışma kodu, genellikle işleyiciler, yönetilmeyen bellek veya hatta platforma özgü boyut tamsayılarla uğraşmak zorunda olur.</span><span class="sxs-lookup"><span data-stu-id="aec7b-116">This interop code often has to deal with handles, unmanaged memory, or even just platform-specific sized integers.</span></span>

<span data-ttu-id="aec7b-117">Çalışma zamanı, `native int` (`System.IntPtr`) ve `native unsigned int` (`System.UIntPtr`) ilkel türlerinde kullanılabilecek bir işleç kümesi tanımlayarak bu için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="aec7b-117">The runtime provides support for this by defining a set of operators that can be used on the `native int` (`System.IntPtr`) and `native unsigned int` (`System.UIntPtr`) primitive types.</span></span>

<span data-ttu-id="aec7b-118">C#Bu işleçleri hiçbir şekilde desteklemez ve kullanıcıların sorunu geçici olarak çözmek zorunda kalmaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-118">C# has never supported these operators and so users have to work around the issue.</span></span> <span data-ttu-id="aec7b-119">Bu genellikle kod karmaşıklığını artırır ve kod bakımlığını düşürür.</span><span class="sxs-lookup"><span data-stu-id="aec7b-119">This often increases code complexity and lowers code maintainability.</span></span>

<span data-ttu-id="aec7b-120">Bu nedenle, dilin bu gereksinimleri daha iyi desteklemesi için bu işleçleri desteklemeye başlaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-120">As such, the language should begin to support these operators to help advance the language to better support these requirements.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="aec7b-121">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="aec7b-121">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="aec7b-122">Desteklenen işleçlerin tam kümesi, ortak dil altyapısı belirtiminin (`ECMA-335`) `III.1.5` tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="aec7b-122">The full set of operators supported are defined in `III.1.5` of the Common Language Infrastructure specification (`ECMA-335`).</span></span> <span data-ttu-id="aec7b-123">Bu belirtim şurada bulunabilir: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span><span class="sxs-lookup"><span data-stu-id="aec7b-123">The specification is available here: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).</span></span>

* <span data-ttu-id="aec7b-124">İşleçlerin özeti aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-124">A summary of the operators is provided below for convenience.</span></span>
* <span data-ttu-id="aec7b-125">CLı belirtimi tarafından tanımlanan doğrulanamayan işleçler listelenmez ve şu anda bu teklifin bir parçası değildir (ancak bunları göz önünde bulundurmakla aynı olabilir).</span><span class="sxs-lookup"><span data-stu-id="aec7b-125">The unverifiable operators defined by the CLI spec are not listed and are not currently part of this proposal (although it may be worth considering these as well).</span></span>
* <span data-ttu-id="aec7b-126">Bir anahtar sözcük (`nint` ve `nuint`gibi) ve `System.IntPtr` için belirtilecek sabit değerler için bir yol sağlar ve `System.UIntPtr` (örneğin, 0n) bu teklifin bir parçası değildir (ancak bunları göz önünde bulundurabilse de).</span><span class="sxs-lookup"><span data-stu-id="aec7b-126">Providing a keyword (such as `nint` and `nuint`) nor providing a way to for literals to be declared for `System.IntPtr` and `System.UIntPtr` (such as 0n) is not part of this proposal (although it may be worth considering these as well).</span></span>

### <a name="unary-plus-operator"></a><span data-ttu-id="aec7b-127">Birli Plus Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-127">Unary Plus Operator</span></span>

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a><span data-ttu-id="aec7b-128">Birli eksi Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-128">Unary Minus Operator</span></span>

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a><span data-ttu-id="aec7b-129">Bit düzeyinde tamamlama Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-129">Bitwise Complement Operator</span></span>

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a><span data-ttu-id="aec7b-130">Atama İşleçleri</span><span class="sxs-lookup"><span data-stu-id="aec7b-130">Cast Operators</span></span>

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a><span data-ttu-id="aec7b-131">Çarpma Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-131">Multiplication Operator</span></span>

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a><span data-ttu-id="aec7b-132">Bölme Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-132">Division Operator</span></span>

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a><span data-ttu-id="aec7b-133">Kalan Işleç</span><span class="sxs-lookup"><span data-stu-id="aec7b-133">Remainder Operator</span></span>

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a><span data-ttu-id="aec7b-134">Toplama Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-134">Addition Operator</span></span>

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a><span data-ttu-id="aec7b-135">Çıkarma Işleci</span><span class="sxs-lookup"><span data-stu-id="aec7b-135">Subtraction Operator</span></span>

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a><span data-ttu-id="aec7b-136">Kaydırma İşleçleri</span><span class="sxs-lookup"><span data-stu-id="aec7b-136">Shift Operators</span></span>

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a><span data-ttu-id="aec7b-137">Tamsayı karşılaştırma Işleçleri</span><span class="sxs-lookup"><span data-stu-id="aec7b-137">Integer Comparison Operators</span></span>

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a><span data-ttu-id="aec7b-138">Tamsayı mantıksal Işleçleri</span><span class="sxs-lookup"><span data-stu-id="aec7b-138">Integer Logical Operators</span></span>

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a><span data-ttu-id="aec7b-139">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="aec7b-139">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="aec7b-140">Bu işleçlerin gerçek kullanımı küçük ve daha düşük düzeydeki kitaplıklar ya da birlikte çalışma kodu yazan son kullanıcılarla sınırlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-140">The actual use of these operators may be small and limited to end-users who are writing lower level libraries or interop code.</span></span> <span data-ttu-id="aec7b-141">Çoğu son kullanıcı büyük olasılıkla yerel boyutlu tamsayılar, işleyiciler ve birlikte çalışma kodu soyut olacak şekilde bu alt düzey kitaplıkları tüketiyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="aec7b-141">Most end-users would likely be consuming these lower level libraries themselves which would have the native sized integers, handles, and interop code abstracted away.</span></span> <span data-ttu-id="aec7b-142">Bu nedenle, işleçlerin kendilerine ihtiyacı yoktur.</span><span class="sxs-lookup"><span data-stu-id="aec7b-142">As such, they would not have need of the operators themselves.</span></span>

## <a name="alternatives"></a><span data-ttu-id="aec7b-143">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="aec7b-143">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="aec7b-144">Framework 'ü doğrudan Il 'de yazarak gerekli işleçleri uygular.</span><span class="sxs-lookup"><span data-stu-id="aec7b-144">Have the framework implement the required operators by writing them directly in IL.</span></span> <span data-ttu-id="aec7b-145">Ayrıca, çalışma zamanı Framework tarafından tanımlanan operatörler için iç destek sağlayabilir. bu nedenle, nihai performansı daha iyi en iyi hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="aec7b-145">Additionally, the runtime could provide intrinsic support for the operators defined by the framework, so as to better optimize the end performance.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="aec7b-146">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="aec7b-146">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="aec7b-147">Tasarımın bölümleri yine de TBD midir?</span><span class="sxs-lookup"><span data-stu-id="aec7b-147">What parts of the design are still TBD?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="aec7b-148">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="aec7b-148">Design meetings</span></span>

<span data-ttu-id="aec7b-149">Bu teklifi etkileyen tasarım notlarının bağlantısını yapın ve hangi değişiklikler üzerinde olduğuna ilişkin bir tümcede açıklama yapın.</span><span class="sxs-lookup"><span data-stu-id="aec7b-149">Link to design notes that affect this proposal, and describe in one sentence for each what changes they led to.</span></span>