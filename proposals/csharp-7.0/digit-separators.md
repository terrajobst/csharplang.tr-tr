---
ms.openlocfilehash: 5476f4438ad79a26b3615154f789d8ed04cb61aa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484505"
---
# <a name="digit-separators"></a><span data-ttu-id="7b9ee-101">Basamak ayıraçları</span><span class="sxs-lookup"><span data-stu-id="7b9ee-101">Digit separators</span></span>

<span data-ttu-id="7b9ee-102">Büyük sayısal sabit değerlerde rakamları gruplandırmakta olması harika bir şekilde okunabilir etki ve önemli bir kenar değildir.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-102">Being able to group digits in large numeric literals would have great readability impact and no significant downside.</span></span> 

<span data-ttu-id="7b9ee-103">İkili değişmez değerler (#215) eklemek, sayısal sabit değerlerin olasılığını artırır, bu nedenle iki özellik birbirini geliştirir.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-103">Adding binary literals (#215) would increase the likelihood of numeric literals being long, so the two features enhance each other.</span></span> 

<span data-ttu-id="7b9ee-104">Java ve diğerlerini takip edeceğiz ve basamaklı bir ayırıcı olarak `_` alt çizgi kullanın.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-104">We would follow Java and others, and use an underscore `_` as a digit separator.</span></span> <span data-ttu-id="7b9ee-105">Farklı Gruplandırmaların farklı senaryolarda ve özellikle de farklı sayısal tabanlarda anlamlı olabileceğinden, her yerde bir sayısal değişmez değer (ilk ve son karakter hariç) oluşabilir:</span><span class="sxs-lookup"><span data-stu-id="7b9ee-105">It would be able to occur everywhere in a numeric literal (except as the first and last character), since different groupings may make sense in different scenarios and especially for different numeric bases:</span></span>

```csharp
int bin = 0b1001_1010_0001_0100;
int hex = 0x1b_a0_44_fe;
int dec = 33_554_432;
int weird = 1_2__3___4____5_____6______7_______8________9;
double real = 1_000.111_1e-1_000;
```

<span data-ttu-id="7b9ee-106">Herhangi bir basamak dizisi alt çizgi ile ayrılabilir, büyük olasılıkla iki ardışık basamak arasında birden fazla alt çizgi olabilir.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-106">Any sequence of digits may be separated by underscores, possibly more than one underscore between two consecutive digits.</span></span> <span data-ttu-id="7b9ee-107">Bunlar, ondalık sayılar ve üsler halinde izin verilir, ancak önceki kuralın ardından, ondalık (`10_.0`), üs karakterinin yanında (`1.1e_1`) veya tür tanımlayıcısının (`10_f`) yanında görünmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-107">They are allowed in decimals as well as exponents, but following the previous rule, they may not appear next to the decimal (`10_.0`), next to the exponent character (`1.1e_1`), or next to the type specifier (`10_f`).</span></span> <span data-ttu-id="7b9ee-108">İkili ve onaltılık sabit değerlerde kullanıldığında, `0x` veya `0b`hemen sonra görünmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-108">When used in binary and hexadecimal literals, they may not appear immediately following the `0x` or `0b`.</span></span>

<span data-ttu-id="7b9ee-109">Sözdizimi basittir ve ayırıcıların anlam etkisi yoktur; bunlar yalnızca yok sayılır.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-109">The syntax is straightforward, and the separators have no semantic impact - they are simply ignored.</span></span>

<span data-ttu-id="7b9ee-110">Bu, geniş bir değere sahiptir ve uygulanması kolaydır.</span><span class="sxs-lookup"><span data-stu-id="7b9ee-110">This has broad value and is easy to implement.</span></span>
