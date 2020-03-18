---
ms.openlocfilehash: 6f05bbc1365e59d737103b586db9d4a242c6d306
ms.sourcegitcommit: e9afb74cc1abd56db93b4b50bd5e6765e27c1c5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2019
ms.locfileid: "79485086"
---
# <a name="static-lambdas"></a>Statik Lambdalar

## <a name="summary"></a>Özet

Kapsayan kapsamdan durum yakalamaya izin vermeyen lambdaları destekleme.

## <a name="motivation"></a>Amacı

Kapsayan bağlamdan yanlışlıkla yakalamadan kaçının.

## <a name="detailed-design"></a>Ayrıntılı tasarım

`static` olan Lambdalar kapsayan kapsamdan durum yakalayamaz.
Sonuç olarak, kapsayan kapsamdaki Yereller, parametreler ve `this` `static` lambda içinde kullanılamaz.

`static` lambda, örtük veya açık bir `this` veya `base` başvurusundan örnek üyelerine başvuramaz.

`static` lambda, kapsayan kapsamdaki `static` üyelere başvurabilir.

`static` lambda, kapsayan kapsamdaki `constant` tanımlarına başvurabilir.

`static` lambda `nameof()`, kapsayan kapsamdaki Yereller, parametreler veya `this` ya da `base` başvurabilir.

Kapsayan kapsamdaki `private` üyeleri için erişilebilirlik kuralları `static` ve`static` olmayan Lambdalar için aynıdır.

`static` lambda tanımının meta verilerde `static` yöntemi olarak yayıldığından emin olmak için herhangi bir garanti yapılmaz. Bunu iyileştirmek için derleyici uygulamasına ayrıldınız.

`static` olmayan yerel bir işlev veya lambda, kapsayan bir `static` lambağından durum yakalayabilir ancak kapsayan `static` lambda dışında durumu yakalayamaz.

Bir `static` lambda, bir ifade ağacında kullanılabilir.

Geçerli bir programdaki lambda `static` değiştiricisini kaldırmak, programın anlamını değiştirmez.
