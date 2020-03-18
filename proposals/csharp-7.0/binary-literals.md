---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484428"
---
# <a name="binary-literals"></a><span data-ttu-id="ddb8b-101">İkili değişmez değerler</span><span class="sxs-lookup"><span data-stu-id="ddb8b-101">Binary literals</span></span>

<span data-ttu-id="ddb8b-102">Ve VB 'ye C# ikili değişmez değerler eklemek için görece yaygın bir istek vardır.</span><span class="sxs-lookup"><span data-stu-id="ddb8b-102">There’s a relatively common request to add binary literals to C# and VB.</span></span> <span data-ttu-id="ddb8b-103">Bit maskeleri (örn. bayrak numaralandırmalar) için bu, kullanışlı bir seçenektir, ancak yalnızca eğitim amaçlarıyla harika olacaktır.</span><span class="sxs-lookup"><span data-stu-id="ddb8b-103">For bitmasks (e.g. flag enums) this seems genuinely useful, but it would also be great just for educational purposes.</span></span>

<span data-ttu-id="ddb8b-104">İkili değişmez değerler şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="ddb8b-104">Binary literals would look like this:</span></span>

```csharp
int nineteen = 0b10011;
```

<span data-ttu-id="ddb8b-105">Sözdizimi ve anlam değerleri, `x`/`X`yerine `b`/`B` kullanmanın yanı sıra, yalnızca rakamlar `0` ve `1` ve 16 yerine temel 2 ' de yorumlanmakta olması dışında, onaltılı değişmez değerler ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="ddb8b-105">Syntactically and semantically they are identical to hexadecimal literals, except for using `b`/`B` instead of `x`/`X`, having only digits `0` and `1` and being interpreted in base 2 instead of 16.</span></span>

<span data-ttu-id="ddb8b-106">Bunları uygulamanın çok kısa bir maliyeti vardır ve bu dilin kullanıcılarına yönelik küçük bir ek yük vardır.</span><span class="sxs-lookup"><span data-stu-id="ddb8b-106">There’s little cost to implementing these, and little conceptual overhead to users of the language.</span></span>

## <a name="syntax"></a><span data-ttu-id="ddb8b-107">Sözdizimi</span><span class="sxs-lookup"><span data-stu-id="ddb8b-107">Syntax</span></span>

<span data-ttu-id="ddb8b-108">Dilbilgisi aşağıdaki gibi olacaktır:</span><span class="sxs-lookup"><span data-stu-id="ddb8b-108">The grammar would be as follows:</span></span>

```antlr
integer-literal:
    : ...
    | binary-integer-literal
    ;
binary-integer-literal:
    : `0b` binary-digits integer-type-suffix-opt
    | `0B` binary-digits integer-type-suffix-opt
    ;
binary-digits:
    : binary-digit
    | binary-digits binary-digit
    ;
binary-digit:
    : `0`
    | `1`
    ;
```
