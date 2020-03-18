---
ms.openlocfilehash: 6ec94aaabb2c52393a87ee450dbae972b6a50bd5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484715"
---
# <a name="allow-digit-separator-after-0b-or-0x"></a>0b veya 0x sonrasında basamak ayırıcısına izin ver

7,2 C# ' de, sayı ayırıcılarının (alt çizgi karakteri) tamsayı sabit değerlerinde görünebilen yer kümesini genişlettik. [7,0 ' den başlayarak, bir sabit değerin rakamları arasında ayırıcılara izin verilir. C# ](../csharp-7.0/digit-separators.md) Bundan sonra, C# 7,2 ' de, ön eke sonra bir ikili veya onaltılı sabit değerin ilk önemli basamağına göre basamak ayırıcılarına izin veririz.

```csharp
    123      // permitted in C# 1.0 and later
    1_2_3    // permitted in C# 7.0 and later
    0x1_2_3  // permitted in C# 7.0 and later
    0b101    // binary literals added in C# 7.0
    0b1_0_1  // permitted in C# 7.0 and later

    // in C# 7.2, _ is permitted after the `0x` or `0b`
    0x_1_2   // permitted in C# 7.2 and later
    0b_1_0_1 // permitted in C# 7.2 and later
```

Ondalık tamsayı hazır bilgisine, önde gelen alt çizgi eklenmesine izin vermedik. `_123` gibi bir belirteç tanımlayıcı olur.
