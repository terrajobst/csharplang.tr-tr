---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485100"
---
# <a name="static-local-functions"></a><span data-ttu-id="2a6c8-101">Statik yerel işlevler</span><span class="sxs-lookup"><span data-stu-id="2a6c8-101">Static local functions</span></span>

## <a name="summary"></a><span data-ttu-id="2a6c8-102">Özet</span><span class="sxs-lookup"><span data-stu-id="2a6c8-102">Summary</span></span>

<span data-ttu-id="2a6c8-103">Kapsayan kapsamdan durum yakalamaya izin vermeyen yerel işlevleri destekler.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-103">Support local functions that disallow capturing state from the enclosing scope.</span></span>

## <a name="motivation"></a><span data-ttu-id="2a6c8-104">Amacı</span><span class="sxs-lookup"><span data-stu-id="2a6c8-104">Motivation</span></span>

<span data-ttu-id="2a6c8-105">Kapsayan bağlamdan yanlışlıkla yakalamadan kaçının.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-105">Avoid unintentionally capturing state from the enclosing context.</span></span>
<span data-ttu-id="2a6c8-106">`static` yönteminin gerekli olduğu senaryolarda yerel işlevlerin kullanılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-106">Allow local functions to be used in scenarios where a `static` method is required.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="2a6c8-107">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="2a6c8-107">Detailed design</span></span>

<span data-ttu-id="2a6c8-108">`static` belirtilen yerel bir işlev kapsayan kapsamdan durum yakalayamaz.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-108">A local function declared `static` cannot capture state from the enclosing scope.</span></span>
<span data-ttu-id="2a6c8-109">Sonuç olarak, kapsayan kapsamdaki Yereller, parametreler ve `this` `static` yerel bir işlev içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-109">As a result, locals, parameters, and `this` from the enclosing scope are not available within a `static` local function.</span></span>

<span data-ttu-id="2a6c8-110">`static` Local işlevi örtük veya açık bir `this` veya `base` başvurusundan örnek üyelerine başvuramaz.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-110">A `static` local function cannot reference instance members from an implicit or explicit `this` or `base` reference.</span></span>

<span data-ttu-id="2a6c8-111">`static` yerel bir işlev, kapsayan kapsamdaki `static` üyelere başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-111">A `static` local function may reference `static` members from the enclosing scope.</span></span>

<span data-ttu-id="2a6c8-112">`static` yerel bir işlev, kapsayan kapsamdaki `constant` tanımlarına başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-112">A `static` local function may reference `constant` definitions from the enclosing scope.</span></span>

<span data-ttu-id="2a6c8-113">`static` yerel işlevindeki `nameof()`, kapsayan kapsamdaki Yereller, parametreler veya `this` ya da `base` başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-113">`nameof()` in a `static` local function may reference locals, parameters, or `this` or `base` from the enclosing scope.</span></span>

<span data-ttu-id="2a6c8-114">Kapsayan kapsamdaki `private` üyeleri için erişilebilirlik kuralları `static` ve`static` olmayan yerel işlevler için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-114">Accessibility rules for `private` members in the enclosing scope are the same for `static` and non-`static` local functions.</span></span>

<span data-ttu-id="2a6c8-115">Bir `static` yerel işlev tanımı, yalnızca bir temsilcisinde kullanılsa bile meta verilerde bir `static` yöntemi olarak yayınlanır.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-115">A `static` local function definition is emitted as a `static` method in metadata, even if only used in a delegate.</span></span>

<span data-ttu-id="2a6c8-116">`static` olmayan yerel bir işlev veya lambda, kapsayan bir `static` yerel işlevinden durum yakalayabilir ancak kapsayan `static` yerel işlevi dışında durumu yakalayamaz.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-116">A non-`static` local function or lambda can capture state from an enclosing `static` local function but cannot capture state outside the enclosing `static` local function.</span></span>

<span data-ttu-id="2a6c8-117">Bir `static` yerel işlev bir ifade ağacında çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-117">A `static` local function cannot be invoked in an expression tree.</span></span>

<span data-ttu-id="2a6c8-118">Yerel bir işleve yapılan çağrı, yerel işlevin `static`olup olmamasına bakılmaksızın `callvirt`yerine `call` olarak yayılır.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-118">A call to a local function is emitted as `call` rather than `callvirt`, regardless of whether the local function is `static`.</span></span>

<span data-ttu-id="2a6c8-119">Yerel işlev içindeki bir çağrının aşırı yüklemesi, yerel işlevin `static`olup olmadığı etkilenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-119">Overload resolution of a call within a local function not affected by whether the local function is `static`.</span></span>

<span data-ttu-id="2a6c8-120">Geçerli bir programdaki yerel işlevden `static` değiştiricinin kaldırılması programın anlamını değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="2a6c8-120">Removing the `static` modifier from a local function in a valid program does not change the meaning of the program.</span></span>

## <a name="design-meetings"></a><span data-ttu-id="2a6c8-121">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="2a6c8-121">Design meetings</span></span>

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
