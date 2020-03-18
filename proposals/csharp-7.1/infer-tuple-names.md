---
ms.openlocfilehash: 25e95b3ab8c384a7e66e59a7f9422cc9699074d7
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484722"
---
# <a name="infer-tuple-names-aka-tuple-projection-initializers"></a><span data-ttu-id="9502d-101">Tanımlama grubu adlarını (aka) çıkarın.</span><span class="sxs-lookup"><span data-stu-id="9502d-101">Infer tuple names (aka.</span></span> <span data-ttu-id="9502d-102">demet projeksiyon başlatıcıları)</span><span class="sxs-lookup"><span data-stu-id="9502d-102">tuple projection initializers)</span></span>

## <a name="summary"></a><span data-ttu-id="9502d-103">Özet</span><span class="sxs-lookup"><span data-stu-id="9502d-103">Summary</span></span>
[summary]: #summary

<span data-ttu-id="9502d-104">Birçok yaygın durumda, bu özellik demet öğesi adlarının atlanmasýný ve bunun yerine çıkarlanmasýný sağlar.</span><span class="sxs-lookup"><span data-stu-id="9502d-104">In a number of common cases, this feature allows the tuple element names to be omitted and instead be inferred.</span></span> <span data-ttu-id="9502d-105">Örneğin, `(f1: x.f1, f2: x?.f2)`yazmak yerine "F1" ve "F2" öğe adları `(x.f1, x?.f2)`çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="9502d-105">For instance, instead of typing `(f1: x.f1, f2: x?.f2)`, the element names "f1" and "f2" can be inferred from `(x.f1, x?.f2)`.</span></span>

<span data-ttu-id="9502d-106">Bu, oluşturma sırasında üye adlarının kullanılmasına izin veren anonim türlerin davranışını paraleldir.</span><span class="sxs-lookup"><span data-stu-id="9502d-106">This parallels the behavior of  anonymous types, which allow inferring member names during creation.</span></span> <span data-ttu-id="9502d-107">Örneğin, `new { x.f1, y?.f2 }` "F1" ve "F2" üyelerini bildirir.</span><span class="sxs-lookup"><span data-stu-id="9502d-107">For instance, `new { x.f1, y?.f2 }` declares members "f1" and "f2".</span></span>

<span data-ttu-id="9502d-108">Bu, LINQ içinde tanımlama grupları kullanırken özellikle yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="9502d-108">This is particularly handy when using tuples in LINQ:</span></span>

```csharp
// "c" and "result" have element names "f1" and "f2"
var result = list.Select(c => (c.f1, c.f2)).Where(t => t.f2 == 1); 
```

## <a name="detailed-design"></a><span data-ttu-id="9502d-109">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="9502d-109">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="9502d-110">Değişikliğin iki bölümü vardır:</span><span class="sxs-lookup"><span data-stu-id="9502d-110">There are two parts to the change:</span></span>

1.  <span data-ttu-id="9502d-111">Açık adı olmayan her demet öğesi için bir aday adı çıkarımı deneyin:</span><span class="sxs-lookup"><span data-stu-id="9502d-111">Try to infer a candidate name for each tuple element which does not have an explicit name:</span></span>
    -   <span data-ttu-id="9502d-112">Anonim türler için ad çıkarımı ile aynı kuralların kullanılması.</span><span class="sxs-lookup"><span data-stu-id="9502d-112">Using same rules as name inference for anonymous types.</span></span>
        - <span data-ttu-id="9502d-113">' C#De, bu üç durumda izin verir: `y` (tanımlayıcı), `x.y` (basit üye erişimi) ve `x?.y` (koşullu erişim).</span><span class="sxs-lookup"><span data-stu-id="9502d-113">In C#, this allows three cases: `y` (identifier), `x.y` (simple member access) and `x?.y` (conditional access).</span></span>
        - <span data-ttu-id="9502d-114">VB 'de, bu `x.y()`gibi ek durumlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9502d-114">In VB, this allows for additional cases, such as `x.y()`.</span></span>
    -   <span data-ttu-id="9502d-115">Yasak veya zaten örtük olduklarından ayrılmış demet adlarını ( C#büyük/küçük harfe duyarlı, vb 'de büyük/küçük harfe duyarlı) reddetme.</span><span class="sxs-lookup"><span data-stu-id="9502d-115">Rejecting reserved tuple names (case-sensitive in C#, case-insensitive in VB), as they are either forbidden or already implicit.</span></span> <span data-ttu-id="9502d-116">Örneğin, `ItemN`, `Rest`ve `ToString`.</span><span class="sxs-lookup"><span data-stu-id="9502d-116">For instance, such as `ItemN`, `Rest`, and `ToString`.</span></span>
    -   <span data-ttu-id="9502d-117">Herhangi bir aday adı, tüm kayıt kapsamındaki (VB 'de C#büyük/küçük harfe duyarlı) yineleniyorlarsa, bu adayları düşürüyoruz.</span><span class="sxs-lookup"><span data-stu-id="9502d-117">If any candidate names are duplicates (case-sensitive in C#, case-insensitive in VB) within the entire tuple, we drop those candidates,</span></span>
2.  <span data-ttu-id="9502d-118">Dönüştürmeler sırasında (demet sabit değerlerinde adları bırakma ve bu bilgileri uyarma), çıkartılan adlar hiçbir uyarı oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="9502d-118">During conversions (which check and warn about dropping names from tuple literals), inferred names would not produce any warnings.</span></span> <span data-ttu-id="9502d-119">Bu, mevcut demet kodunun kesilmesini önler.</span><span class="sxs-lookup"><span data-stu-id="9502d-119">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="9502d-120">Yinelenenleri işleme kuralının anonim türler için olandan farklı olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="9502d-120">Note that the rule for handling duplicates is different than that for anonymous types.</span></span> <span data-ttu-id="9502d-121">Örneğin, `new { x.f1, x.f1 }` bir hata üretir, ancak `(x.f1, x.f1)` yine de izin verilir (herhangi bir Çıkarsanan ad olmadan).</span><span class="sxs-lookup"><span data-stu-id="9502d-121">For instance, `new { x.f1, x.f1 }` produces an error, but `(x.f1, x.f1)` would still be allowed (just without any inferred names).</span></span> <span data-ttu-id="9502d-122">Bu, mevcut demet kodunun kesilmesini önler.</span><span class="sxs-lookup"><span data-stu-id="9502d-122">This avoids breaking existing tuple code.</span></span>

<span data-ttu-id="9502d-123">Tutarlılık için, aynı durum, yinelenenleri kaldırma (içinde) tarafından üretilen diziler için de C#geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="9502d-123">For consistency, the same would apply to tuples produced by deconstruction-assignments (in C#):</span></span>

```csharp
// tuple has element names "f1" and "f2" 
var tuple = ((x.f1, x?.f2) = (1, 2));
```

<span data-ttu-id="9502d-124">Aynı zamanda, ifadenin adı ve büyük/küçük harf duyarsız ad karşılaştırmaları için VB 'ye özgü kuralları kullanarak VB tanımlama grupları için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9502d-124">The same would also apply to VB tuples, using the VB-specific rules for inferring name from expression and case-insensitive name comparisons.</span></span>

<span data-ttu-id="9502d-125">Dil sürümü " C# 7,0" olan 7,1 derleyicisini (veya sonraki bir sürümünü) kullanırken, öğe adları çıkarsolacaktır (özellik kullanılabilir olmasa da), ancak bunlara erişmeye çalışmak için bir kullanım sitesi hatası olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9502d-125">When using the C# 7.1 compiler (or later) with language version "7.0", the element names will be inferred (despite the feature not being available), but there will be a use-site error for trying to access them.</span></span> <span data-ttu-id="9502d-126">Bu, daha sonra uyumluluk sorununa (aşağıda açıklanmıştır) dönük yeni kod eklemelerini sınırlandırır.</span><span class="sxs-lookup"><span data-stu-id="9502d-126">This will limit additions of new code that would later face the compatibility issue (described below).</span></span>

## <a name="drawbacks"></a><span data-ttu-id="9502d-127">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="9502d-127">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="9502d-128">Bunun ana dezavantajı, bunun 7,0 'den C# bir uyumluluk kesmesi sunmaıdır:</span><span class="sxs-lookup"><span data-stu-id="9502d-128">The main drawback is that this introduces a compatibility break from C# 7.0:</span></span>

```csharp
Action y = () => M();
var t = (x: x, y);
t.y(); // this might have previously picked up an extension method called “y”, but would now call the lambda.
```

<span data-ttu-id="9502d-129">Uyumluluk Council bu kesmeyi kabul ettiği için, sınırlandırılacak ve (7,0 ' de C# ), başlıkların gönderildiği ve zaman penceresinin bu yana kabul edilebilir olduğunu belirledi.</span><span class="sxs-lookup"><span data-stu-id="9502d-129">The compatibility council found this break acceptable, given that it is limited and the time window since tuples shipped (in C# 7.0) is short.</span></span>

## <a name="references"></a><span data-ttu-id="9502d-130">Başvurular</span><span class="sxs-lookup"><span data-stu-id="9502d-130">References</span></span>
- [<span data-ttu-id="9502d-131">LDM 4 Nisan 2017</span><span class="sxs-lookup"><span data-stu-id="9502d-131">LDM April 4th 2017</span></span>](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-04-05.md#tuple-names)
- <span data-ttu-id="9502d-132">[GitHub tartışması](https://github.com/dotnet/csharplang/issues/370) (Bu sorunu getirmek için @alrz teşekkürler)</span><span class="sxs-lookup"><span data-stu-id="9502d-132">[Github discussion](https://github.com/dotnet/csharplang/issues/370) (thanks @alrz for bringing this issue up)</span></span>
- [<span data-ttu-id="9502d-133">Tanımlama grubu tasarımı</span><span class="sxs-lookup"><span data-stu-id="9502d-133">Tuples design</span></span>](https://github.com/dotnet/roslyn/blob/master/docs/features/tuples.md)
