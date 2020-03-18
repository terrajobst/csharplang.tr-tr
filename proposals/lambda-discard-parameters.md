---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485156"
---
# <a name="lambda-discard-parameters"></a><span data-ttu-id="b2fc2-101">Lambda atma parametreleri</span><span class="sxs-lookup"><span data-stu-id="b2fc2-101">Lambda discard parameters</span></span>

## <a name="summary"></a><span data-ttu-id="b2fc2-102">Özet</span><span class="sxs-lookup"><span data-stu-id="b2fc2-102">Summary</span></span>

<span data-ttu-id="b2fc2-103">Atma (`_`), Lambdalar ve anonim yöntemlerin parametreleri olarak kullanılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-103">Allow discards (`_`) to be used as parameters of lambdas and anonymous methods.</span></span>
<span data-ttu-id="b2fc2-104">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="b2fc2-104">For example:</span></span>
- <span data-ttu-id="b2fc2-105">Lambdalar: `(_, _) => 0`, `(int _, int _) => 0`</span><span class="sxs-lookup"><span data-stu-id="b2fc2-105">lambdas: `(_, _) => 0`, `(int _, int _) => 0`</span></span>
- <span data-ttu-id="b2fc2-106">Anonim Yöntemler: `delegate(int _, int _) { return 0; }`</span><span class="sxs-lookup"><span data-stu-id="b2fc2-106">anonymous methods: `delegate(int _, int _) { return 0; }`</span></span>

## <a name="motivation"></a><span data-ttu-id="b2fc2-107">Amacı</span><span class="sxs-lookup"><span data-stu-id="b2fc2-107">Motivation</span></span>

<span data-ttu-id="b2fc2-108">Kullanılmayan parametrelerin adlandırılmış olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-108">Unused parameters do not need to be named.</span></span> <span data-ttu-id="b2fc2-109">Atma amacı net, yani kullanılmıyor/atılır.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-109">The intent of discards is clear, i.e. they are unused/discarded.</span></span>

## <a name="detailed-design"></a><span data-ttu-id="b2fc2-110">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="b2fc2-110">Detailed design</span></span>

<span data-ttu-id="b2fc2-111">[Yöntem parametreleri](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) `_`adlı birden fazla parametreye sahip bir lambda veya anonim metodun parametre listesinde, bu parametreler atma parametreleridir.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-111">[Method parameters](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) In the parameter list of a lambda or anonymous method with more than one parameter named `_`, such parameters are discard parameters.</span></span>
<span data-ttu-id="b2fc2-112">Note: tek bir parametre adı `_`, geriye doğru uyumluluk nedenleriyle normal bir parametredir.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-112">Note: if a single parameter is named `_` then it is a regular parameter for backwards compatibility reasons.</span></span>

<span data-ttu-id="b2fc2-113">Atma parametreleri herhangi bir kapsam için herhangi bir ad sunmaz.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-113">Discard parameters do not introduce any names to any scopes.</span></span>
<span data-ttu-id="b2fc2-114">Bu, `_` (alt çizgi) adlarının gizlenmesine neden olmadığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-114">Note this implies they do not cause any `_` (underscore) names to be hidden.</span></span>

<span data-ttu-id="b2fc2-115">[Basit adlar](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) `K` sıfırsa ve *simple_name* bir *blok* içinde görünürse ve *blok*(veya kapsayan *bloğun*) yerel değişken bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) bir yerel değişken, parametre (atma parametreleri hariç) veya `I`ad ile sabit içeriyorsa, *simple_name* söz konusu yerel değişkene, parametreye veya sabitine başvurur ve değişken veya değer olarak sınıflandırılmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b2fc2-115">[Simple names](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) If `K` is zero and the *simple_name* appears within a *block* and if the *block*'s (or an enclosing *block*'s) local variable declaration space ([Declarations](basic-concepts.md#declarations)) contains a local variable, parameter (with the exception of discard parameters) or constant with name `I`, then the *simple_name* refers to that local variable, parameter or constant and is classified as a variable or value.</span></span>

<span data-ttu-id="b2fc2-116">[Kapsamlar](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Atma parametreleri dışında, bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) içinde belirtilen bir parametre kapsamı, bu lambda_expression, atma parametrelerinden oluşan bir parametre olan *anonymous_function_body* , bu *anonymous_method_expression*bir *anonymous_method_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) tarafından belirtilen bir *blok* olan *lambda_expression* .</span><span class="sxs-lookup"><span data-stu-id="b2fc2-116">[Scopes](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) With the exception of discard parameters, the scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression* With the exception of discard parameters, the scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>

## <a name="related-spec-sections"></a><span data-ttu-id="b2fc2-117">İlgili belirtim bölümleri</span><span class="sxs-lookup"><span data-stu-id="b2fc2-117">Related spec sections</span></span>
- [<span data-ttu-id="b2fc2-118">Karşılık gelen parametreler</span><span class="sxs-lookup"><span data-stu-id="b2fc2-118">Corresponding parameters</span></span>](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
