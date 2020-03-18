---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485086"
---
# <a name="static-lambdas"></a><span data-ttu-id="96eb3-101">Statik Lambdalar</span><span class="sxs-lookup"><span data-stu-id="96eb3-101">Static lambdas</span></span>

## <a name="summary"></a><span data-ttu-id="96eb3-102">Özet</span><span class="sxs-lookup"><span data-stu-id="96eb3-102">Summary</span></span>

<span data-ttu-id="96eb3-103">Kapsayan kapsamdan durum yakalamaya izin vermeyen lambdaları destekleme.</span><span class="sxs-lookup"><span data-stu-id="96eb3-103">Support lambdas that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="96eb3-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="96eb3-104">Motivation</span></span>

<span data-ttu-id="96eb3-105">Kapsayan bağlamdan yanlışlıkla yakalamadan kaçının.</span><span class="sxs-lookup"><span data-stu-id="96eb3-105">Avoid unintentionally capturing state from the enclosing context.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="96eb3-106">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="96eb3-106">Detailed design</span></span>

<span data-ttu-id="96eb3-107">`static` olan Lambdalar kapsayan kapsamdan durum yakalayamaz.</span><span class="sxs-lookup"><span data-stu-id="96eb3-107">A lambdas with `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="96eb3-108">Sonuç olarak, kapsayan kapsamdaki Yereller, parametreler ve `this` `static` lambda içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="96eb3-108">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` lambda.</span></span>

<span data-ttu-id="96eb3-109">`static` lambda, örtük veya açık bir `this` veya `base` başvurusundan örnek üyelerine başvuramaz.</span><span class="sxs-lookup"><span data-stu-id="96eb3-109">A `static` lambda cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="96eb3-110">`static` lambda, kapsayan kapsamdaki `static` üyelere başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="96eb3-110">A `static` lambda may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="96eb3-111">`static` lambda, kapsayan kapsamdaki `constant` tanımlarına başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="96eb3-111">A `static` lambda may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="96eb3-112">`static` lambda `nameof()`, kapsayan kapsamdaki Yereller, parametreler veya `this` ya da `base` başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="96eb3-112">`nameof()` in a `static` lambda may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="96eb3-113">Kapsayan kapsamdaki `private` üyeleri için erişilebilirlik kuralları `static` ve`static` olmayan Lambdalar için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="96eb3-113">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` lambdas.</span></span>

<span data-ttu-id="96eb3-114">`static` lambda tanımının meta verilerde `static` yöntemi olarak yayıldığından emin olmak için herhangi bir garanti yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="96eb3-114">No guarantee is made as to whether a `static` lambda definition is emitted as a `static` method in metadata.</span></span> <span data-ttu-id="96eb3-115">Bunu iyileştirmek için derleyici uygulamasına ayrıldınız.</span><span class="sxs-lookup"><span data-stu-id="96eb3-115">This is left up to the compiler implementation to optimize.</span></span>

<span data-ttu-id="96eb3-116">`static` olmayan yerel bir işlev veya lambda, kapsayan bir `static` lambağından durum yakalayabilir ancak kapsayan `static` lambda dışında durumu yakalayamaz.</span><span class="sxs-lookup"><span data-stu-id="96eb3-116">A non-`static` local function or lambda can capture state from an enclosing `static` lambda but cannot capture state outside the enclosing `static` lambda.</span></span>

<span data-ttu-id="96eb3-117">Bir `static` lambda, bir ifade ağacında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="96eb3-117">A `static` lambda can be used in an expression tree.</span></span>

<span data-ttu-id="96eb3-118">Geçerli bir programdaki lambda `static` değiştiricisini kaldırmak, programın anlamını değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="96eb3-118">Removing the `static` modifier from a lambda in a valid program does not change the meaning of the program.</span></span>
