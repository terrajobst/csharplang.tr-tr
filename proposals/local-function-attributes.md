---
ms.openlocfilehash: 0c8bc2b5072ea7f86189b41a1cdbf2a449661b05
ms.sourcegitcommit: 33a60a1db1d42d21d959acfeb127e647150173aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79509509"
---
# <a name="attributes-on-local-functions"></a>Yerel işlevlerde öznitelikler

## <a name="attributes"></a>Öznitelikler

Yerel işlev bildirimlerinin artık [özniteliklere](../spec/attributes.md)izin veriliyor. Yerel işlevlerde parametre ve tür parametrelerinin de özniteliklere sahip olmasına izin verilir.

Bir yönteme uygulandığında belirtilen anlamı olan öznitelikler, parametreleri veya tür parametreleri yerel bir işleve, parametrelerine veya tür parametrelerine uygulandığında aynı anlama sahip olacaktır.

Yerel bir işlev, `[ConditionalAttribute]`bir [koşullu yöntemle](../spec/attributes.md#the-conditional-attribute) aynı anlamda koşullu hale getirilebilir. Koşullu yerel işlevin de `static`olması gerekir. Koşullu yöntemlerle ilgili tüm kısıtlamalar, dönüş türü `void`olması da dahil olmak üzere koşullu yerel işlevler için de geçerlidir.

## <a name="extern"></a>Dış

`extern` değiştiricisine artık yerel işlevlerde izin verilir. Bu, yerel işlevi dış bir [yöntemle](../spec/classes.md#external-methods)aynı anlamda harici hale getirir.

Harici bir yönteme benzer şekilde, dış yerel işlevin *yerel işlev gövdesi* noktalı virgül olmalıdır. Noktalı virgülle *yerel işlev gövdesine* yalnızca dış yerel işlevde izin verilir. 

Dış yerel işlev de `static`olmalıdır.

## <a name="syntax"></a>Sözdizimi

[Yerel işlevlerin dil bilgisi](csharp-7.0/local-functions.md#syntax-grammar) aşağıdaki şekilde değiştirilmiştir:
```
local-function-header
    : attributes? local-function-modifiers? return-type identifier type-parameter-list?
        ( formal-parameter-list? ) type-parameter-constraints-clauses
    ;

local-function-modifiers
    : (async | unsafe | static | extern)*
    ;

local-function-body
    : block
    | arrow-expression-body
    | ';'
    ;
```
