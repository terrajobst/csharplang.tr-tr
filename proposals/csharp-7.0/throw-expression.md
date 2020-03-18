---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484547"
---
# <a name="throw-expression"></a><span data-ttu-id="35bdf-101">Throw ifadesi</span><span class="sxs-lookup"><span data-stu-id="35bdf-101">Throw expression</span></span>

<span data-ttu-id="35bdf-102">Dahil edilecek ifade formları kümesini genişlettik</span><span class="sxs-lookup"><span data-stu-id="35bdf-102">We extend the set of expression forms to include</span></span>

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

<span data-ttu-id="35bdf-103">Tür kuralları aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="35bdf-103">The type rules are as follows:</span></span>

- <span data-ttu-id="35bdf-104">*Throw_expression* türü yok.</span><span class="sxs-lookup"><span data-stu-id="35bdf-104">A *throw_expression* has no type.</span></span>
- <span data-ttu-id="35bdf-105">*Throw_expression* örtük bir dönüştürme tarafından her türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="35bdf-105">A *throw_expression* is convertible to every type by an implicit conversion.</span></span>

<span data-ttu-id="35bdf-106">Bir *throw ifadesi* , `System.Exception` ' den türetilen bir sınıf türünün veya geçerli temel sınıfı olarak `System.Exception` (veya bir alt sınıf) olan bir tür parametre türünden `System.Exception`sınıf türünde bir değeri belirtmek için *null_coalescing_expression*değerlendirilirken üretilen değeri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="35bdf-106">A *throw expression* throws the value produced by evaluating the *null_coalescing_expression*, which must denote a value of the class type `System.Exception`, of a class type that derives from `System.Exception` or of a type parameter type that has `System.Exception` (or a subclass thereof) as its effective base class.</span></span> <span data-ttu-id="35bdf-107">İfadenin değerlendirmesi `null`üretirse bunun yerine bir `System.NullReferenceException` oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="35bdf-107">If evaluation of the expression produces `null`, a `System.NullReferenceException` is thrown instead.</span></span>

<span data-ttu-id="35bdf-108">Bir *throw ifadesinin* değerlendirilme çalışma zamanındaki davranış [bir *throw deyimi*için belirtilen şekilde](../../spec/statements.md#the-throw-statement)aynıdır.</span><span class="sxs-lookup"><span data-stu-id="35bdf-108">The behavior at runtime of the evaluation of a *throw expression* is the same [as specified for a *throw statement*](../../spec/statements.md#the-throw-statement).</span></span>

<span data-ttu-id="35bdf-109">Flow-Analysis kuralları aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="35bdf-109">The flow-analysis rules are as follows:</span></span>

- <span data-ttu-id="35bdf-110">Her bir sanal *değişken için*, *v* , *throw_expression*önce kesin olarak atanmış bir *throw_expression* FF *null_coalescing_expression* önüne kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="35bdf-110">For every variable *v*, *v* is definitely assigned before the *null_coalescing_expression* of a *throw_expression* iff it is definitely assigned before the *throw_expression*.</span></span>
- <span data-ttu-id="35bdf-111">Her bir sanal *değişken için*, *throw_expression*sonrasında *v* kesinlikle atanır.</span><span class="sxs-lookup"><span data-stu-id="35bdf-111">For every variable *v*, *v* is definitely assigned after *throw_expression*.</span></span>

<span data-ttu-id="35bdf-112">*Throw ifadesine* yalnızca aşağıdaki sözdizimsel bağlamlarda izin verilir:</span><span class="sxs-lookup"><span data-stu-id="35bdf-112">A *throw expression* is permitted in only the following syntactic contexts:</span></span>
- <span data-ttu-id="35bdf-113">Üçlü bir koşullu işlecin ikinci veya üçüncü işleneni olarak `?:`</span><span class="sxs-lookup"><span data-stu-id="35bdf-113">As the second or third operand of a ternary conditional operator `?:`</span></span>
- <span data-ttu-id="35bdf-114">Null birleştirme işlecinin ikinci işleneni olarak `??`</span><span class="sxs-lookup"><span data-stu-id="35bdf-114">As the second operand of a null coalescing operator `??`</span></span>
- <span data-ttu-id="35bdf-115">İfadesinin gövdesi olarak-Bodied lambda veya yöntem.</span><span class="sxs-lookup"><span data-stu-id="35bdf-115">As the body of an expression-bodied lambda or method.</span></span>
