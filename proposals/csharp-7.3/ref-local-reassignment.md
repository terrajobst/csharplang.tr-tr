---
ms.openlocfilehash: c1a77d9337ecd16f5ea1c30d18f6422552b0dfb2
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484603"
---
# <a name="ref-local-reassignment"></a>Ref yerel yeniden atama

7,3 C# ' de, bir ref yerel değişkeninin veya bir ref parametresinin başvurusunu yeniden bağlama desteği ekledik.

Aşağıdakileri `assignment_operator`s kümesine ekleyeceğiz.

```antlr
assignment_operator
    : '=' 'ref'
    ;
```

`=ref` işlecine ***ref atama işleci***denir. Bu bir *bileşik atama işleci*değildir. Sol işlenen bir başvuru yerel değişkenine, bir başvuru parametresine (`this`dışında) veya out parametresine bağlanan bir ifade olmalıdır. Sağ işlenen, sol işlenen ile aynı türde bir değer atayarak lvalue veren bir ifade olmalıdır.

Sağ işlenen, ref atamasının noktasında kesin olarak atanmalıdır.

Sol işlenen bir `out` parametresine bağlandığında, `out` parametresinin ref atama işlecinin başlangıcında kesinlikle atanmamadığı bir hatadır.

Sol işlenen yazılabilir bir başvuru ise (yani, `ref readonly` yerel veya `in` parametresi dışında bir şey içeriyorsa), sağ işlenen yazılabilir bir lvalue olmalıdır.

Ref atama işleci atanan türün lvalue değerini verir. Sol işlenen yazılabilir ise (örneğin, `ref readonly` veya `in`) yazılabilir.

Bu işlecin güvenlik kuralları şunlardır:

- Ref yeniden atama `e1 = ref e2`için, *ref-safe* `e2`, `e1`*ref ile güvenli çıkış* olarak en az geniş bir kapsam olmalıdır.

Ref *-to-kaçış* , [ref benzeri türler için güvenlik](../csharp-7.2/span-safety.md) bölümünde tanımlanmıştır
