---
ms.openlocfilehash: 11e9d21bda9e69be692c5c5f5aee80c2da1894ab
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484624"
---
# <a name="unmanaged-type-constraint"></a><span data-ttu-id="a4e8c-101">Yönetilmeyen tür kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="a4e8c-101">Unmanaged type constraint</span></span>

## <a name="summary"></a><span data-ttu-id="a4e8c-102">Özet</span><span class="sxs-lookup"><span data-stu-id="a4e8c-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="a4e8c-103">Yönetilmeyen kısıtlama özelliği dil zorlamasına, C# dil belirtiminde "yönetilmeyen türler" olarak bilinen türler sınıfına sahip olur.  Bu, başvuru türü olmayan ve herhangi bir iç içe geçme düzeyinde başvuru türü alanları içermeyen bir tür olarak 18,2 bölümünde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-103">The unmanaged constraint feature will give language enforcement to the class of types known as "unmanaged types" in the C# language spec.  This is defined in section 18.2 as a type which is not a reference type and doesn't contain reference type fields at any level of nesting.</span></span>  

## <a name="motivation"></a><span data-ttu-id="a4e8c-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="a4e8c-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="a4e8c-105">Birincil mosyon, ' de C#düşük düzey birlikte çalışma kodu yazmayı kolaylaştırmaktır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-105">The primary motivation is to make it easier to author low level interop code in C#.</span></span> <span data-ttu-id="a4e8c-106">Yönetilmeyen türler, birlikte çalışma kodu için temel yapı taşlarından biridir, ancak genel türlerde desteğin olmaması, tüm yönetilmeyen türler arasında yeniden kullanılabilir yordamlar oluşturmayı olanaksız hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-106">Unmanaged types are one of the core building blocks for interop code, yet the lack of support in generics makes it impossible to create re-usable routines across all unmanaged types.</span></span> <span data-ttu-id="a4e8c-107">Bunun yerine, geliştiriciler kitaplığındaki her yönetilmeyen tür için aynı Boiler levha kodunu yazmak üzere zorlanır:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-107">Instead developers are forced to author the same boiler plate code for every unmanaged type in their library:</span></span>

```csharp
int Hash(Point point) { ... } 
int Hash(TimeSpan timeSpan) { ... } 
```

<span data-ttu-id="a4e8c-108">Bu tür bir senaryoyu etkinleştirmek için dil yeni bir kısıtlama tanıtıyor: yönetilmeyen:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-108">To enable this type of scenario the language will be introducing a new constraint: unmanaged:</span></span>

```csharp
void Hash<T>(T value) where T : unmanaged
{
    ...
}
```

<span data-ttu-id="a4e8c-109">Bu kısıtlama yalnızca C# dil belirtiminde yönetilmeyen tür tanımına sığan türlerle karşılanabilecek. Bunun bir başka yolu da bir türün yönetilmeyen kısıtlamayı karşılamasının bir işaretçi olarak kullanılması.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-109">This constraint can only be met by types which fit into the unmanaged type definition in the C# language spec. Another way of looking at it is that a type satisfies the unmanaged constraint iff it can also be used as a pointer.</span></span> 

```csharp
Hash(new Point()); // Okay 
Hash(42); // Okay
Hash("hello") // Error: Type string does not satisfy the unmanaged constraint
```

<span data-ttu-id="a4e8c-110">Yönetilmeyen kısıtlama içeren tür parametreleri, yönetilmeyen türler için kullanılabilen tüm özellikleri kullanabilir: işaretçiler, sabit, vb...</span><span class="sxs-lookup"><span data-stu-id="a4e8c-110">Type parameters with the unmanaged constraint can use all the features available to unmanaged types: pointers, fixed, etc ...</span></span> 

```csharp
void Hash<T>(T value) where T : unmanaged
{
    // Okay
    fixed (T* p = &value) 
    { 
        ...
    }
}
```

<span data-ttu-id="a4e8c-111">Bu kısıtlama, yapılandırılmış veriler ve bayt akışları arasında verimli dönüştürmeler olmasını da mümkün kılar.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-111">This constraint will also make it possible to have efficient conversions between structured data and streams of bytes.</span></span> <span data-ttu-id="a4e8c-112">Bu, ağ yığınları ve serileştirme katmanlarında ortak olan bir işlemdir:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-112">This is an operation that is common in networking stacks and serialization layers:</span></span>

```csharp
Span<byte> Convert<T>(ref T value) where T : unmanaged 
{
    ...
}
```

<span data-ttu-id="a4e8c-113">Bu tür yordamlar, derleme zamanında provably güvende olduklarından ve serbest bırakıldığı için avantajlıdır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-113">Such routines are advantageous because they are provably safe at compile time and allocation free.</span></span>  <span data-ttu-id="a4e8c-114">Birlikte çalışma yazarları bugün bunu yapamayabilir (performans açısından kritik olan bir katmanda olmasına rağmen).</span><span class="sxs-lookup"><span data-stu-id="a4e8c-114">Interop authors today can not do this (even though it's at a layer where perf is critical).</span></span>  <span data-ttu-id="a4e8c-115">Bunun yerine, değerlerin doğru şekilde yönetilmeyen olduğunu doğrulamak için pahalı çalışma zamanı denetimleri olan yordamları ayırmaya güvenmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-115">Instead they need to rely on allocating routines that have expensive runtime checks to verify values are correctly unmanaged.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="a4e8c-116">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="a4e8c-116">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="a4e8c-117">Dilde `unmanaged`adlı yeni bir kısıtlama tanıtılacaktır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-117">The language will introduce a new constraint named `unmanaged`.</span></span> <span data-ttu-id="a4e8c-118">Bu kısıtlamayı karşılamak için, bir tür yapı olmalıdır ve türün tüm alanları aşağıdaki kategorilerden birine denk gelmelidir:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-118">In order to satisfy this constraint a type must be a struct and all the fields of the type must fall into one of the following categories:</span></span>

- <span data-ttu-id="a4e8c-119">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` veya `UIntPtr`türüne sahip olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-119">Have the type `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `IntPtr` or `UIntPtr`.</span></span>
- <span data-ttu-id="a4e8c-120">Herhangi bir `enum` türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-120">Be any `enum` type.</span></span>
- <span data-ttu-id="a4e8c-121">Bir işaretçi türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-121">Be a pointer type.</span></span>
- <span data-ttu-id="a4e8c-122">`unmanaged` kısıtlamasını sattı eden Kullanıcı tanımlı bir yapı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-122">Be a user defined struct that satsifies the `unmanaged` constraint.</span></span>

<span data-ttu-id="a4e8c-123">Derleyici tarafından oluşturulan örnek alanları, örneğin otomatik uygulanan özellikler, bu kısıtlamaları da karşılamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-123">Compiler generated instance fields, such as those backing auto-implemented properties, must also meet these constraints.</span></span> 

<span data-ttu-id="a4e8c-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-124">For example:</span></span>

```csharp
// Unmanaged type
struct Point 
{ 
    int X;
    int Y {get; set;}
}

// Not an unmanaged type
struct Student 
{ 
    string FirstName;
    string LastName;
}
``` 

<span data-ttu-id="a4e8c-125">`unmanaged` kısıtlaması `struct`, `class` veya `new()`ile birleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-125">The `unmanaged` constraint cannot be combined with `struct`, `class` or `new()`.</span></span> <span data-ttu-id="a4e8c-126">Bu kısıtlama, `unmanaged` `struct` anlamına gelir, bu nedenle diğer kısıtlamalar anlamlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-126">This restriction derives from the fact that `unmanaged` implies `struct` hence the other constraints do not make sense.</span></span>

<span data-ttu-id="a4e8c-127">`unmanaged` kısıtlaması CLR tarafından yalnızca dile göre zorlanmaz.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-127">The `unmanaged` constraint is not enforced by CLR, only by the language.</span></span> <span data-ttu-id="a4e8c-128">Diğer dillerin yanlış kullanımını engellemek için, bu kısıtlamaya sahip Yöntemler bir mod-REQ tarafından korunur. Bu, diğer dillerin yönetilmeyen türler olmayan tür bağımsız değişkenlerini kullanmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-128">To prevent mis-use by other languages, methods which have this constraint will be protected by a mod-req. This will prevent other languages from using type arguments which are not unmanaged types.</span></span>

<span data-ttu-id="a4e8c-129">Kısıtlamadaki belirteç `unmanaged` bir anahtar sözcük, ya da bağlamsal anahtar sözcük değil.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-129">The token `unmanaged` in the constraint is not a keyword, nor a contextual keyword.</span></span> <span data-ttu-id="a4e8c-130">Bunun yerine, bu konumda değerlendirildiğinden `var` gibidir ve şunları olur:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-130">Instead it is like `var` in that it is evaluated at that location and will either:</span></span>

- <span data-ttu-id="a4e8c-131">`unmanaged`adlı Kullanıcı tanımlı veya başvurulan türe bağla: Bu, diğer adlandırılmış tür kısıtlaması işlendiği gibi değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-131">Bind to user defined or referenced type named `unmanaged`: This will be treated just as any other named type constraint is treated.</span></span> 
- <span data-ttu-id="a4e8c-132">Hiçbir tür için bağlama: Bu, `unmanaged` kısıtlaması olarak yorumlanacaktır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-132">Bind to no type: This will be interpreted as the `unmanaged` constraint.</span></span>

<span data-ttu-id="a4e8c-133">`unmanaged` adlı bir tür varsa ve geçerli bağlamda nitelendirme olmadan kullanılabiliyorsa, `unmanaged` kısıtlamasını kullanmanın bir yolu olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-133">In the case there is a type named `unmanaged` and it is available without qualification in the current context, then there will be no way to use the `unmanaged` constraint.</span></span> <span data-ttu-id="a4e8c-134">Bu, özelliği `var` çevreleyen kuralları ve aynı ada sahip kullanıcı tanımlı türleri paraleller.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-134">This parallels the rules surrounding the feature `var` and user defined types of the same name.</span></span> 

## <a name="drawbacks"></a><span data-ttu-id="a4e8c-135">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="a4e8c-135">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="a4e8c-136">Bu özelliğin birincil dezavantajı, az sayıda geliştirici sunmesidir: genellikle düşük düzey kitaplık yazarları veya çerçeveleri.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-136">The primary drawback of this feature is that it serves a small number of developers: typically low level library authors or frameworks.</span></span>  <span data-ttu-id="a4e8c-137">Bu nedenle, az sayıda geliştirici için değerli bir dil süresi harcadığından.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-137">Hence it's spending precious language time for a small number of developers.</span></span> 

<span data-ttu-id="a4e8c-138">Bu çerçeveler, genellikle burada .NET uygulamalarının çoğunluğunun temelini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-138">Yet these frameworks are often the basis for the majority of .NET applications out there.</span></span>  <span data-ttu-id="a4e8c-139">Bu nedenle, bu düzeyde WINS performansı/doğruluğu, .NET ekosistemi üzerinde Ripple etkiye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-139">Hence performance / correctness wins at this level can have a ripple effect on the .NET ecosystem.</span></span>  <span data-ttu-id="a4e8c-140">Bu, özelliği sınırlı kitlelerle hatta göz önünde bulundurmayı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-140">This makes the feature worth considering even with the limited audience.</span></span>

## <a name="alternatives"></a><span data-ttu-id="a4e8c-141">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="a4e8c-141">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="a4e8c-142">Göz önünde bulundurulması gereken birkaç seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="a4e8c-142">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="a4e8c-143">Durum Quo: Özellik kendi birleşmesine göre hizalı değildir ve geliştiriciler örtük kabul etme davranışını kullanmaya devam eder.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-143">The status quo:  The feature is not justified on its own merits and developers continue to use the implicit opt in behavior.</span></span>

## <a name="questions"></a><span data-ttu-id="a4e8c-144">UL</span><span class="sxs-lookup"><span data-stu-id="a4e8c-144">Questions</span></span>
[quesions]: #questions

### <a name="metadata-representation"></a><span data-ttu-id="a4e8c-145">Meta veri gösterimi</span><span class="sxs-lookup"><span data-stu-id="a4e8c-145">Metadata Representation</span></span>

<span data-ttu-id="a4e8c-146">F# Dil imza dosyasındaki kısıtlamayı kodluyor, bu da temsilinin C# yeniden kullanılamayacağı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-146">The F# language encodes the constraint in the signature file which means C# cannot re-use their representation.</span></span> <span data-ttu-id="a4e8c-147">Bu kısıtlama için yeni bir özniteliğin seçilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-147">A new attribute will need to be chosen for this constraint.</span></span> <span data-ttu-id="a4e8c-148">Ayrıca, bu kısıtlamayı içeren bir yöntemin bir mod-REQ tarafından korunması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-148">Additionally a method which has this constraint must be protected by a mod-req.</span></span>

### <a name="blittable-vs-unmanaged"></a><span data-ttu-id="a4e8c-149">Blittable vs. yönetilmeyen</span><span class="sxs-lookup"><span data-stu-id="a4e8c-149">Blittable vs. Unmanaged</span></span>
<span data-ttu-id="a4e8c-150">Dil, yönetilmeyen anahtar sözcüğünü kullanan çok benzer bir özelliğe sahiptir. [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) F#</span><span class="sxs-lookup"><span data-stu-id="a4e8c-150">The F# language has a very [similar feature](https://docs.microsoft.com/dotnet/articles/fsharp/language-reference/generics/constraints) which uses the keyword unmanaged.</span></span> <span data-ttu-id="a4e8c-151">Blittable adı Midori ' deki kullanım öğesinden gelir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-151">The blittable name comes from the use in Midori.</span></span>  <span data-ttu-id="a4e8c-152">Burada önceliğe bakmak ve bunun yerine yönetilmeyen ' i kullanmak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-152">May want to look to precedence here and use unmanaged instead.</span></span> 

<span data-ttu-id="a4e8c-153">**Çözüm** Yönetilmeyen bu dilin kullanımına karar</span><span class="sxs-lookup"><span data-stu-id="a4e8c-153">**Resolution** The language decide to use unmanaged</span></span> 

### <a name="verifier"></a><span data-ttu-id="a4e8c-154">'Nın</span><span class="sxs-lookup"><span data-stu-id="a4e8c-154">Verifier</span></span>

<span data-ttu-id="a4e8c-155">Genel tür parametrelerine işaretçiler kullanımını anlamak için doğrulayıcı/çalışma zamanının güncellenmesi gerekiyor mu?</span><span class="sxs-lookup"><span data-stu-id="a4e8c-155">Does the verifier / runtime need to be updated to understand the use of pointers to generic type parameters?</span></span>  <span data-ttu-id="a4e8c-156">Ya da yalnızca değişiklik olmayan şekilde çalışabilir mi?</span><span class="sxs-lookup"><span data-stu-id="a4e8c-156">Or can it simply work as is without changes?</span></span>

<span data-ttu-id="a4e8c-157">**Çözüm** Değişiklik gerekmiyor.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-157">**Resolution** No changes needed.</span></span> <span data-ttu-id="a4e8c-158">Tüm işaretçi türleri doğrulanamaz.</span><span class="sxs-lookup"><span data-stu-id="a4e8c-158">All pointer types are simply unverifiable.</span></span> 

## <a name="design-meetings"></a><span data-ttu-id="a4e8c-159">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="a4e8c-159">Design meetings</span></span>

<span data-ttu-id="a4e8c-160">yok</span><span class="sxs-lookup"><span data-stu-id="a4e8c-160">n/a</span></span>
