---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509509"
---
# <a name="attributes-on-local-functions"></a><span data-ttu-id="be7df-101">Yerel işlevlerde öznitelikler</span><span class="sxs-lookup"><span data-stu-id="be7df-101">Attributes on local functions</span></span>

## <a name="attributes"></a><span data-ttu-id="be7df-102">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="be7df-102">Attributes</span></span>

<span data-ttu-id="be7df-103">Yerel işlev bildirimlerinin artık [özniteliklere](../spec/attributes.md)izin veriliyor.</span><span class="sxs-lookup"><span data-stu-id="be7df-103">Local function declarations are now permitted to have [attributes](../spec/attributes.md).</span></span> <span data-ttu-id="be7df-104">Yerel işlevlerde parametre ve tür parametrelerinin de özniteliklere sahip olmasına izin verilir.</span><span class="sxs-lookup"><span data-stu-id="be7df-104">Parameters and type parameters on local functions are also allowed to have attributes.</span></span>

<span data-ttu-id="be7df-105">Bir yönteme uygulandığında belirtilen anlamı olan öznitelikler, parametreleri veya tür parametreleri yerel bir işleve, parametrelerine veya tür parametrelerine uygulandığında aynı anlama sahip olacaktır.</span><span class="sxs-lookup"><span data-stu-id="be7df-105">Attributes with a specified meaning when applied to a method, its parameters, or its type parameters will have the same meaning when applied to a local function, its parameters, or its type parameters, respectively.</span></span>

<span data-ttu-id="be7df-106">Yerel bir işlev, `[ConditionalAttribute]`bir [koşullu yöntemle](../spec/attributes.md#the-conditional-attribute) aynı anlamda koşullu hale getirilebilir.</span><span class="sxs-lookup"><span data-stu-id="be7df-106">A local function can be made conditional in the same sense as a [conditional method](../spec/attributes.md#the-conditional-attribute) by decorating it with a `[ConditionalAttribute]`.</span></span> <span data-ttu-id="be7df-107">Koşullu yerel işlevin de `static`olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="be7df-107">A conditional local function must also be `static`.</span></span> <span data-ttu-id="be7df-108">Koşullu yöntemlerle ilgili tüm kısıtlamalar, dönüş türü `void`olması da dahil olmak üzere koşullu yerel işlevler için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="be7df-108">All restrictions on conditional methods also apply to conditional local functions, including that the return type must be `void`.</span></span>

## <a name="extern"></a><span data-ttu-id="be7df-109">Dış</span><span class="sxs-lookup"><span data-stu-id="be7df-109">Extern</span></span>

<span data-ttu-id="be7df-110">`extern` değiştiricisine artık yerel işlevlerde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="be7df-110">The `extern` modifier is now permitted on local functions.</span></span> <span data-ttu-id="be7df-111">Bu, yerel işlevi dış bir [yöntemle](../spec/classes.md#external-methods)aynı anlamda harici hale getirir.</span><span class="sxs-lookup"><span data-stu-id="be7df-111">This makes the local function external in the same sense as an [external method](../spec/classes.md#external-methods).</span></span>

<span data-ttu-id="be7df-112">Harici bir yönteme benzer şekilde, dış yerel işlevin *yerel işlev gövdesi* noktalı virgül olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="be7df-112">Similarly to an external method, the *local-function-body* of an external local function must be a semicolon.</span></span> <span data-ttu-id="be7df-113">Noktalı virgülle *yerel işlev gövdesine* yalnızca dış yerel işlevde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="be7df-113">A semicolon *local-function-body* is only permitted on an external local function.</span></span> 

<span data-ttu-id="be7df-114">Dış yerel işlev de `static`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="be7df-114">An external local function must also be `static`.</span></span>

## <a name="syntax"></a><span data-ttu-id="be7df-115">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="be7df-115">Syntax</span></span>

<span data-ttu-id="be7df-116">[Yerel işlevlerin dil bilgisi](csharp-7.0/local-functions.md#syntax-grammar) aşağıdaki şekilde değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="be7df-116">The [local functions grammar](csharp-7.0/local-functions.md#syntax-grammar) is modified as follows:</span></span>
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
