---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484603"
---
# <a name="ref-local-reassignment"></a><span data-ttu-id="b9a7d-101">Ref yerel yeniden atama</span><span class="sxs-lookup"><span data-stu-id="b9a7d-101">Ref Local Reassignment</span></span>

<span data-ttu-id="b9a7d-102">7,3 C# ' de, bir ref yerel değişkeninin veya bir ref parametresinin başvurusunu yeniden bağlama desteği ekledik.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-102">In C# 7.3, we add support for rebinding the referent of a ref local variable or a ref parameter.</span></span>

<span data-ttu-id="b9a7d-103">Aşağıdakileri `assignment_operator`s kümesine ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-103">We add the following to the set of `assignment_operator`s.</span></span>

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

<span data-ttu-id="b9a7d-104">`=ref` işlecine ***ref atama işleci***denir.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-104">The `=ref` operator is called the ***ref assignment operator***.</span></span> <span data-ttu-id="b9a7d-105">Bu bir *bileşik atama işleci*değildir.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-105">It is not a *compound assignment operator*.</span></span> <span data-ttu-id="b9a7d-106">Sol işlenen bir başvuru yerel değişkenine, bir başvuru parametresine (`this`dışında) veya out parametresine bağlanan bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-106">The left operand must be an expression that binds to a ref local variable, a ref parameter (other than `this`), or an out parameter.</span></span> <span data-ttu-id="b9a7d-107">Sağ işlenen, sol işlenen ile aynı türde bir değer atayarak lvalue veren bir ifade olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-107">The right operand must be an expression that yields an lvalue designating a value of the same type as the left operand.</span></span>

<span data-ttu-id="b9a7d-108">Sağ işlenen, ref atamasının noktasında kesin olarak atanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-108">The right operand must be definitely assigned at the point of the ref assignment.</span></span>

<span data-ttu-id="b9a7d-109">Sol işlenen bir `out` parametresine bağlandığında, `out` parametresinin ref atama işlecinin başlangıcında kesinlikle atanmamadığı bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-109">When the left operand binds to an `out` parameter, it is an error if that `out` parameter has not been definitely assigned at the beginning of the ref assignment operator.</span></span>

<span data-ttu-id="b9a7d-110">Sol işlenen yazılabilir bir başvuru ise (yani, `ref readonly` yerel veya `in` parametresi dışında bir şey içeriyorsa), sağ işlenen yazılabilir bir lvalue olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-110">If the left operand is a writeable ref (i.e. it designates anything other than a `ref readonly` local or  `in` parameter), then the right operand must be a writeable lvalue.</span></span>

<span data-ttu-id="b9a7d-111">Ref atama işleci atanan türün lvalue değerini verir.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-111">The ref assignment operator yields an lvalue of the assigned type.</span></span> <span data-ttu-id="b9a7d-112">Sol işlenen yazılabilir ise (örneğin, `ref readonly` veya `in`) yazılabilir.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-112">It is writeable if the left operand is writeable (i.e. not `ref readonly` or `in`).</span></span>

<span data-ttu-id="b9a7d-113">Bu işlecin güvenlik kuralları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b9a7d-113">The safety rules for this operator are:</span></span>

- <span data-ttu-id="b9a7d-114">Ref yeniden atama `e1 = ref e2`için, *ref-safe* `e2`, `e1`*ref ile güvenli çıkış* olarak en az geniş bir kapsam olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b9a7d-114">For a ref reassignment `e1 = ref e2`, the *ref-safe-to-escape* of `e2` must be at least as wide a scope as the *ref-safe-to-escape* of `e1`.</span></span>

<span data-ttu-id="b9a7d-115">Ref *-to-kaçış* , [ref benzeri türler için güvenlik](../csharp-7.2/span-safety.md) bölümünde tanımlanmıştır</span><span class="sxs-lookup"><span data-stu-id="b9a7d-115">Where *ref-safe-to-escape* is defined in [Safety for ref-like types](../csharp-7.2/span-safety.md)</span></span>
