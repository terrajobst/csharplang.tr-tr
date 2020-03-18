---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484701"
---
# <a name="conditional-ref-expressions"></a><span data-ttu-id="b6a8e-101">Koşullu başvuru ifadeleri</span><span class="sxs-lookup"><span data-stu-id="b6a8e-101">Conditional ref expressions</span></span>

<span data-ttu-id="b6a8e-102">Bir başvuru değişkenini bir veya başka bir ifadeye koşullu olarak bağlama deseninin Şu anda ifade edilemez C#.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-102">The pattern of binding a ref variable to one or another expression conditionally is not currently expressible in C#.</span></span>

<span data-ttu-id="b6a8e-103">Tipik geçici çözüm, şunun gibi bir yöntemi tanıtmaktadır:</span><span class="sxs-lookup"><span data-stu-id="b6a8e-103">The typical workaround is to introduce a method like:</span></span>

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

<span data-ttu-id="b6a8e-104">Tüm bağımsız değişkenlerin çağrı sitesinde değerlendirilmesi gerektiğinden, bu, Üçlü olarak tam bir değiştirme değildir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-104">Note that this is not an exact replacement of a ternary since all arguments must be evaluated at the call site.</span></span>

<span data-ttu-id="b6a8e-105">Aşağıdakiler beklendiği gibi çalışmayacak:</span><span class="sxs-lookup"><span data-stu-id="b6a8e-105">The following will not work as expected:</span></span>

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

<span data-ttu-id="b6a8e-106">Önerilen sözdizimi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="b6a8e-106">The proposed syntax would look like:</span></span>

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

<span data-ttu-id="b6a8e-107">Yukarıdaki "Choice" girişimi, ref Üçlü kullanılarak _doğru şekilde_ yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="b6a8e-107">The above attempt with "Choice" can be _correctly_ written using ref ternary as:</span></span>

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="b6a8e-108">Tercih edilen fark, sonuç ve alternatif ifadelere _gerçekten_ koşullu bir şekilde erişildiğine göre, ```arr == null``` bir kilitlenme görmemelidir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-108">The difference from Choice is that consequence and alternative expressions are accessed in a _truly_ conditional manner, so we do not see a crash if ```arr == null```</span></span>

<span data-ttu-id="b6a8e-109">Üçlü başvuru, hem alternatif hem de sonuç refs olan bir üçlü yerdir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-109">The ternary ref is just a ternary where both alternative and consequence are refs.</span></span> <span data-ttu-id="b6a8e-110">Doğal olarak, sonuç/alternatif işlenenlerinin LValues olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-110">It will naturally require that consequence/alternative operands are LValues.</span></span> <span data-ttu-id="b6a8e-111">Ayrıca, sonuç ve alternatifin birbirlerine dönüştürülebilir türleri olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-111">It will also require that consequence and alternative have types that are identity convertible to each other.</span></span>

<span data-ttu-id="b6a8e-112">İfadenin türü, normal üçlü için de bir şekilde hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-112">The type of the expression will be computed similarly to the one for the regular ternary.</span></span> <span data-ttu-id="b6a8e-113">Yani.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-113">I.E.</span></span> <span data-ttu-id="b6a8e-114">Sonuç ve alternatifin kimliği dönüştürülebilir, ancak farklı türlere sahip olması durumunda, varolan tür birleştirme kuralları geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-114">in a case if consequence and alternative have identity convertible, but different types, the existing type-merging rules will apply.</span></span>

<span data-ttu-id="b6a8e-115">Güvenle dönüş, koşullu işlenenleri karşı koruma olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-115">Safe-to-return will be assumed conservatively from the conditional operands.</span></span> <span data-ttu-id="b6a8e-116">Her birinin, tüm işlemi döndürmesi güvenli değilse, dönmek için güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-116">If either is unsafe to return the whole thing is unsafe to return.</span></span>

<span data-ttu-id="b6a8e-117">Ref Üçlü değeri bir LValue ve başvuruya göre geçirilebilir/atanabilir/döndürülebilir;</span><span class="sxs-lookup"><span data-stu-id="b6a8e-117">Ref ternary is an LValue and as such it can be passed/assigned/returned by reference;</span></span>

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

<span data-ttu-id="b6a8e-118">LValue olarak da atanabilir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-118">Being an LValue, it can also be assigned to.</span></span> 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

<span data-ttu-id="b6a8e-119">Ref Üçlü, normal (Ref değil) bağlamda de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-119">Ref ternary can be used in a regular (not ref) context as well.</span></span> <span data-ttu-id="b6a8e-120">Yalnızca normal bir üçlü kullansanız da bu yana ortak olmaz.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-120">Although it would not be common since you could as well just use a regular ternary.</span></span>

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

<span data-ttu-id="b6a8e-121">Uygulama notları:</span><span class="sxs-lookup"><span data-stu-id="b6a8e-121">Implementation notes:</span></span> 

<span data-ttu-id="b6a8e-122">Uygulamanın karmaşıklığı, orta ve büyük bir hata düzeltmesinin boyutu olarak görünebilir.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-122">The complexity of the implementation would seem to be the size of a moderate-to-large bug fix.</span></span> <span data-ttu-id="b6a8e-123">-I. E çok pahalı değil.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-123">- I.E not very expensive.</span></span>
<span data-ttu-id="b6a8e-124">Söz dizimi veya ayrıştırma için herhangi bir değişikliğe ihtiyacım olduğunu Düşünmiyorum.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-124">I do not think we need any changes to the syntax or parsing.</span></span>
<span data-ttu-id="b6a8e-125">Meta veri veya birlikte çalışma üzerinde hiçbir etkisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-125">There is no effect on metadata or interop.</span></span> <span data-ttu-id="b6a8e-126">Özelliği tamamen ifade tabanlıdır.</span><span class="sxs-lookup"><span data-stu-id="b6a8e-126">The feature is completely expression based.</span></span>
<span data-ttu-id="b6a8e-127">Hata ayıklama/PDB üzerinde herhangi bir etkisi yoktur</span><span class="sxs-lookup"><span data-stu-id="b6a8e-127">No effect on debugging/PDB either</span></span>
