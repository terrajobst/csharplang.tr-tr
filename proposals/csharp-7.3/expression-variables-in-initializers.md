---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484925"
---
# <a name="expression-variables-in-initializers"></a><span data-ttu-id="d0356-101">Başlatıcılarda ifade değişkenleri</span><span class="sxs-lookup"><span data-stu-id="d0356-101">Expression variables in initializers</span></span>

## <a name="summary"></a><span data-ttu-id="d0356-102">Özet</span><span class="sxs-lookup"><span data-stu-id="d0356-102">Summary</span></span>
[summary]: #summary

<span data-ttu-id="d0356-103">Alan başlatıcıları, özellik başlatıcıları, C# ctor-başlatıcıları ve sorgu yan tümcelerinde ifade değişkenleri (out değişkeni bildirimleri ve bildirim desenleri) içeren ifadelere izin vermek için 7 ' de tanıtılan özellikleri genişlettik.</span><span class="sxs-lookup"><span data-stu-id="d0356-103">We extend the features introduced in C# 7 to permit expressions containing expression variables (out variable declarations and declaration patterns) in field initializers, property initializers, ctor-initializers, and query clauses.</span></span>

## <a name="motivation"></a><span data-ttu-id="d0356-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="d0356-104">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="d0356-105">Bu, zaman eksikliğinden dolayı C# dilin bir yanındaki kaba kenarları tamamlar.</span><span class="sxs-lookup"><span data-stu-id="d0356-105">This completes a couple of the rough edges left in the C# language due to lack of time.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="d0356-106">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="d0356-106">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="d0356-107">Bir ctor başlatıcısı içindeki ifade değişkenlerinin bildirimini (değişken bildirimleri ve bildirim desenleri) engellemeye yönelik kısıtlamayı kaldırdık.</span><span class="sxs-lookup"><span data-stu-id="d0356-107">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a ctor-initializer.</span></span> <span data-ttu-id="d0356-108">Bu tür bir belirtilen değişken, oluşturucunun gövdesi boyunca kapsamdadır.</span><span class="sxs-lookup"><span data-stu-id="d0356-108">Such a declared variable is in scope throughout the body of the constructor.</span></span>

<span data-ttu-id="d0356-109">Bir alan veya özellik başlatıcısındaki ifade değişkenlerinin bildirimini (değişken bildirimleri ve bildirim desenleri) engellemeye yönelik kısıtlamayı kaldırdık.</span><span class="sxs-lookup"><span data-stu-id="d0356-109">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a field or property initializer.</span></span> <span data-ttu-id="d0356-110">Bu tür bir belirtilen değişken, başlatma ifadesi boyunca kapsamdadır.</span><span class="sxs-lookup"><span data-stu-id="d0356-110">Such a declared variable is in scope throughout the initializing expression.</span></span>

<span data-ttu-id="d0356-111">Lambda gövdesine çevrilmiş bir sorgu ifadesi yan tümcesindeki ifade değişkenlerinin bildirimini (değişken bildirimleri ve bildirim desenleri) engelleyen kısıtlamayı kaldırdık.</span><span class="sxs-lookup"><span data-stu-id="d0356-111">We remove the restriction preventing the declaration of expression variables (out variable declarations and declaration patterns) in a query expression clause that is translated into the body of a lambda.</span></span> <span data-ttu-id="d0356-112">Bu tür bir belirtilen değişken, sorgu yan tümcesinin bu ifadesi boyunca kapsamdadır.</span><span class="sxs-lookup"><span data-stu-id="d0356-112">Such a declared variable is in scope throughout that expression of the query clause.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="d0356-113">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="d0356-113">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="d0356-114">Yok.</span><span class="sxs-lookup"><span data-stu-id="d0356-114">None.</span></span>

## <a name="alternatives"></a><span data-ttu-id="d0356-115">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="d0356-115">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="d0356-116">Bu bağlamlarda belirtilen ifade değişkenleri için uygun kapsam belirgin değildir ve daha fazla LDM tartışmasını geri görür.</span><span class="sxs-lookup"><span data-stu-id="d0356-116">The appropriate scope for expression variables declared in these contexts is not obvious, and deserves further LDM discussion.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="d0356-117">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="d0356-117">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

- <span data-ttu-id="d0356-118">[] Bu değişkenler için uygun kapsam nedir?</span><span class="sxs-lookup"><span data-stu-id="d0356-118">[ ] What is the appropriate scope for these variables?</span></span>

## <a name="design-meetings"></a><span data-ttu-id="d0356-119">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="d0356-119">Design meetings</span></span>

<span data-ttu-id="d0356-120">Yok.</span><span class="sxs-lookup"><span data-stu-id="d0356-120">None.</span></span>
