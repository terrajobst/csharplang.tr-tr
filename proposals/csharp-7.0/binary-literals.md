---
ms.openlocfilehash: 6f6c24e826e9fe9b9e8c97549add1029f00bcf60
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484428"
---
# <a name="binary-literals"></a>İkili değişmez değerler

Ve VB 'ye C# ikili değişmez değerler eklemek için görece yaygın bir istek vardır. Bit maskeleri (örn. bayrak numaralandırmalar) için bu, kullanışlı bir seçenektir, ancak yalnızca eğitim amaçlarıyla harika olacaktır.

İkili değişmez değerler şuna benzer:

```csharp
int nineteen = 0b10011;
```

Sözdizimi ve anlam değerleri, `x`/`X`yerine `b`/`B` kullanmanın yanı sıra, yalnızca rakamlar `0` ve `1` ve 16 yerine temel 2 ' de yorumlanmakta olması dışında, onaltılı değişmez değerler ile aynıdır.

Bunları uygulamanın çok kısa bir maliyeti vardır ve bu dilin kullanıcılarına yönelik küçük bir ek yük vardır.

## <a name="syntax"></a>Sözdizimi

Dilbilgisi aşağıdaki gibi olacaktır:

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
