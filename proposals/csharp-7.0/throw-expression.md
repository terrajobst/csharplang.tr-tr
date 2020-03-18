---
ms.openlocfilehash: 2532a24e867930d2f27614f19c77585dbce80dfa
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484547"
---
# <a name="throw-expression"></a>Throw ifadesi

Dahil edilecek ifade formları kümesini genişlettik

```antlr
throw_expression
    : 'throw' null_coalescing_expression
    ;

null_coalescing_expression
    : throw_expression
    ;
```

Tür kuralları aşağıdaki gibidir:

- *Throw_expression* türü yok.
- *Throw_expression* örtük bir dönüştürme tarafından her türe dönüştürülebilir.

Bir *throw ifadesi* , `System.Exception` ' den türetilen bir sınıf türünün veya geçerli temel sınıfı olarak `System.Exception` (veya bir alt sınıf) olan bir tür parametre türünden `System.Exception`sınıf türünde bir değeri belirtmek için *null_coalescing_expression*değerlendirilirken üretilen değeri oluşturur. İfadenin değerlendirmesi `null`üretirse bunun yerine bir `System.NullReferenceException` oluşturulur.

Bir *throw ifadesinin* değerlendirilme çalışma zamanındaki davranış [bir *throw deyimi*için belirtilen şekilde](../../spec/statements.md#the-throw-statement)aynıdır.

Flow-Analysis kuralları aşağıdaki gibidir:

- Her bir sanal *değişken için*, *v* , *throw_expression*önce kesin olarak atanmış bir *throw_expression* FF *null_coalescing_expression* önüne kesinlikle atanır.
- Her bir sanal *değişken için*, *throw_expression*sonrasında *v* kesinlikle atanır.

*Throw ifadesine* yalnızca aşağıdaki sözdizimsel bağlamlarda izin verilir:
- Üçlü bir koşullu işlecin ikinci veya üçüncü işleneni olarak `?:`
- Null birleştirme işlecinin ikinci işleneni olarak `??`
- İfadesinin gövdesi olarak-Bodied lambda veya yöntem.
