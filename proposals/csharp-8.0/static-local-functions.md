---
ms.openlocfilehash: fecd5a6c1e0f6c7a7a7beac0e4e60445281c7846
ms.sourcegitcommit: 1b488e76c2c07aafc377bc5e8a7197252c82f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "79485100"
---
# <a name="static-local-functions"></a>Statik yerel işlevler

## <a name="summary"></a>Özet

Kapsayan kapsamdan durum yakalamaya izin vermeyen yerel işlevleri destekler.

## <a name="motivation"></a>Amacı

Kapsayan bağlamdan yanlışlıkla yakalamadan kaçının.
`static` yönteminin gerekli olduğu senaryolarda yerel işlevlerin kullanılmasına izin verin.

## <a name="detailed-design"></a>Ayrıntılı tasarım

`static` belirtilen yerel bir işlev kapsayan kapsamdan durum yakalayamaz.
Sonuç olarak, kapsayan kapsamdaki Yereller, parametreler ve `this` `static` yerel bir işlev içinde kullanılamaz.

`static` Local işlevi örtük veya açık bir `this` veya `base` başvurusundan örnek üyelerine başvuramaz.

`static` yerel bir işlev, kapsayan kapsamdaki `static` üyelere başvurabilir.

`static` yerel bir işlev, kapsayan kapsamdaki `constant` tanımlarına başvurabilir.

`static` yerel işlevindeki `nameof()`, kapsayan kapsamdaki Yereller, parametreler veya `this` ya da `base` başvurabilir.

Kapsayan kapsamdaki `private` üyeleri için erişilebilirlik kuralları `static` ve`static` olmayan yerel işlevler için aynıdır.

Bir `static` yerel işlev tanımı, yalnızca bir temsilcisinde kullanılsa bile meta verilerde bir `static` yöntemi olarak yayınlanır.

`static` olmayan yerel bir işlev veya lambda, kapsayan bir `static` yerel işlevinden durum yakalayabilir ancak kapsayan `static` yerel işlevi dışında durumu yakalayamaz.

Bir `static` yerel işlev bir ifade ağacında çağrılamaz.

Yerel bir işleve yapılan çağrı, yerel işlevin `static`olup olmamasına bakılmaksızın `callvirt`yerine `call` olarak yayılır.

Yerel işlev içindeki bir çağrının aşırı yüklemesi, yerel işlevin `static`olup olmadığı etkilenmemiştir.

Geçerli bir programdaki yerel işlevden `static` değiştiricinin kaldırılması programın anlamını değiştirmez.

## <a name="design-meetings"></a>Tasarım toplantıları

https://github.com/dotnet/csharplang/blob/master/meetings/2018/LDM-2018-09-10.md#static-local-functions
