---
ms.openlocfilehash: 4f66c0f60d05ed6509a1d0843318a71d1b36c351
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484708"
---
# <a name="pattern-matching-with-generics"></a><span data-ttu-id="72324-101">Genel türler ile desenler eşleme</span><span class="sxs-lookup"><span data-stu-id="72324-101">pattern-matching with generics</span></span>

* <span data-ttu-id="72324-102">[x] önerilir</span><span class="sxs-lookup"><span data-stu-id="72324-102">[x] Proposed</span></span>
* <span data-ttu-id="72324-103">[] Prototipi:</span><span class="sxs-lookup"><span data-stu-id="72324-103">[ ] Prototype:</span></span>
* <span data-ttu-id="72324-104">[] Uygulama:</span><span class="sxs-lookup"><span data-stu-id="72324-104">[ ] Implementation:</span></span>
* <span data-ttu-id="72324-105">[] Belirtimi:</span><span class="sxs-lookup"><span data-stu-id="72324-105">[ ] Specification:</span></span>

## <a name="summary"></a><span data-ttu-id="72324-106">Özet</span><span class="sxs-lookup"><span data-stu-id="72324-106">Summary</span></span>
[summary]: #summary

<span data-ttu-id="72324-107">[ C# Mevcut as işlecinin](../../spec/expressions.md#the-as-operator) belirtimi, bir açık tür olduğunda işlenenin türü ve belirtilen tür arasında dönüştürme olmaması için izin verir.</span><span class="sxs-lookup"><span data-stu-id="72324-107">The specification for the [existing C# as operator](../../spec/expressions.md#the-as-operator) permits there to be no conversion between the type of the operand and the specified type when either is an open type.</span></span> <span data-ttu-id="72324-108">Ancak, 7 C# ' de `Type identifier` deseninin, girişin türü ve verilen tür arasında bir dönüştürme olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="72324-108">However, in C# 7 the `Type identifier` pattern requires there be a conversion between the type of the input and the given type.</span></span>

<span data-ttu-id="72324-109">Bu işlemi C# rahat hale getirme ve `expression is Type identifier`değiştirme konusunda, `expression as Type` izin verildiğinde izin verilmesi için izin verilen koşullarda izin verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="72324-109">We propose to relax this and change `expression is Type identifier`, in addition to being permitted in the conditions when it is permitted in C# 7, to also be permitted when `expression as Type` would be allowed.</span></span> <span data-ttu-id="72324-110">Özellikle, yeni durumlar ifade türünün veya belirtilen türün açık bir tür olduğu durumlardır.</span><span class="sxs-lookup"><span data-stu-id="72324-110">Specifically, the new cases are cases where the type of the expression or the specified type is an open type.</span></span> 

## <a name="motivation"></a><span data-ttu-id="72324-111">Amacı</span><span class="sxs-lookup"><span data-stu-id="72324-111">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="72324-112">Model eşleme 'nin şu anda derlenmesinin "açık" olmasına neden olması gerektiği durumlar.</span><span class="sxs-lookup"><span data-stu-id="72324-112">Cases where pattern-matching should "obviously" be permitted currently fail to compile.</span></span> <span data-ttu-id="72324-113">Bkz. Örneğin, https://github.com/dotnet/roslyn/issues/16195.</span><span class="sxs-lookup"><span data-stu-id="72324-113">See, for example, https://github.com/dotnet/roslyn/issues/16195.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="72324-114">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="72324-114">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="72324-115">Paragraf eşleme belirtimindeki paragrafı değiştirdik (önerilen ekleme kalın olarak gösterilmiştir):</span><span class="sxs-lookup"><span data-stu-id="72324-115">We change the paragraph in the pattern-matching specification (the proposed addition is shown in bold):</span></span>

> <span data-ttu-id="72324-116">Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="72324-116">Certain combinations of static type of the left-hand-side and the given type are considered incompatible and result in compile-time error.</span></span> <span data-ttu-id="72324-117">`E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` ' den `T`' e bir kutudan çıkarma dönüştürmesi ya da **`E` ya da `T` bir açık tür**ise, tür ile *uyumlu* bir değer `T` olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="72324-117">A value of static type `E` is said to be *pattern compatible* with the type `T` if there exists an identity conversion, an implicit reference conversion, a boxing conversion, an explicit reference conversion, or an unboxing conversion from `E` to `T`**, or if either `E` or `T` is an open type**.</span></span> <span data-ttu-id="72324-118">`E` türündeki bir ifade ile eşleştirildiği tür deseninin türüyle uyumlu kalıp yoksa, derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="72324-118">It is a compile-time error if an expression of type `E` is not pattern compatible with the type in a type pattern that it is matched with.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="72324-119">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="72324-119">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="72324-120">Yok.</span><span class="sxs-lookup"><span data-stu-id="72324-120">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="72324-121">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="72324-121">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="72324-122">Yok.</span><span class="sxs-lookup"><span data-stu-id="72324-122">None.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="72324-123">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="72324-123">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

<span data-ttu-id="72324-124">Yok.</span><span class="sxs-lookup"><span data-stu-id="72324-124">None.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="72324-125">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="72324-125">Design meetings</span></span>

<span data-ttu-id="72324-126">LDM bu soruyu kabul eden ve keçeli bir hata çözme düzeyi değişikliği vardı.</span><span class="sxs-lookup"><span data-stu-id="72324-126">LDM considered this question and felt it was a bug-fix level change.</span></span> <span data-ttu-id="72324-127">Bunu ayrı bir dil özelliği olarak kabul ediyoruz çünkü yalnızca dil yayımlandıktan sonra değişikliği yapmak ileri bir uyumsuzluk sağlar.</span><span class="sxs-lookup"><span data-stu-id="72324-127">We are treating it as a separate language feature because just making the change after the language has been released would introduce a forward incompatibility.</span></span> <span data-ttu-id="72324-128">Önerilen değişikliğin kullanılması, programcının dil sürümü 7,1 ' i belirtmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="72324-128">Using the proposed change requires that the programmer specify language version 7.1.</span></span>
