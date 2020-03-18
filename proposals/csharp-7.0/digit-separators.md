---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484505"
---
# <a name="digit-separators"></a>Basamak ayıraçları

Büyük sayısal sabit değerlerde rakamları gruplandırmakta olması harika bir şekilde okunabilir etki ve önemli bir kenar değildir. 

İkili değişmez değerler (#215) eklemek, sayısal sabit değerlerin olasılığını artırır, bu nedenle iki özellik birbirini geliştirir. 

Java ve diğerlerini takip edeceğiz ve basamaklı bir ayırıcı olarak `_` alt çizgi kullanın. Farklı Gruplandırmaların farklı senaryolarda ve özellikle de farklı sayısal tabanlarda anlamlı olabileceğinden, her yerde bir sayısal değişmez değer (ilk ve son karakter hariç) oluşabilir:

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

Herhangi bir basamak dizisi alt çizgi ile ayrılabilir, büyük olasılıkla iki ardışık basamak arasında birden fazla alt çizgi olabilir. Bunlar, ondalık sayılar ve üsler halinde izin verilir, ancak önceki kuralın ardından, ondalık (`10_.0`), üs karakterinin yanında (`1.1e_1`) veya tür tanımlayıcısının (`10_f`) yanında görünmeyebilir. İkili ve onaltılık sabit değerlerde kullanıldığında, `0x` veya `0b`hemen sonra görünmeyebilir.

Sözdizimi basittir ve ayırıcıların anlam etkisi yoktur; bunlar yalnızca yok sayılır.

Bu, geniş bir değere sahiptir ve uygulanması kolaydır.
