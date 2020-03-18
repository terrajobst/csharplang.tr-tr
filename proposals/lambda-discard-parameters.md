---
ms.openlocfilehash: 6695664c3d5ca73f66e792b7ce2ec9993aceea05
ms.sourcegitcommit: 42ef673ecc883009c865f8384249881a546df216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2019
ms.locfileid: "79485156"
---
# <a name="lambda-discard-parameters"></a>Lambda atma parametreleri

## <a name="summary"></a>Özet

Atma (`_`), Lambdalar ve anonim yöntemlerin parametreleri olarak kullanılmasına izin verir.
Örneğin:
- Lambdalar: `(_, _) => 0`, `(int _, int _) => 0`
- Anonim Yöntemler: `delegate(int _, int _) { return 0; }`

## <a name="motivation"></a>Amacı

Kullanılmayan parametrelerin adlandırılmış olması gerekmez. Atma amacı net, yani kullanılmıyor/atılır.

## <a name="detailed-design"></a>Ayrıntılı tasarım

[Yöntem parametreleri](https://github.com/dotnet/csharplang/blob/master/spec/classes.md#method-parameters) `_`adlı birden fazla parametreye sahip bir lambda veya anonim metodun parametre listesinde, bu parametreler atma parametreleridir.
Note: tek bir parametre adı `_`, geriye doğru uyumluluk nedenleriyle normal bir parametredir.

Atma parametreleri herhangi bir kapsam için herhangi bir ad sunmaz.
Bu, `_` (alt çizgi) adlarının gizlenmesine neden olmadığını gösterir.

[Basit adlar](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#simple-names) `K` sıfırsa ve *simple_name* bir *blok* içinde görünürse ve *blok*(veya kapsayan *bloğun*) yerel değişken bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) bir yerel değişken, parametre (atma parametreleri hariç) veya `I`ad ile sabit içeriyorsa, *simple_name* söz konusu yerel değişkene, parametreye veya sabitine başvurur ve değişken veya değer olarak sınıflandırılmaktadır.

[Kapsamlar](https://github.com/dotnet/csharplang/blob/master/spec/basic-concepts.md#scopes) Atma parametreleri dışında, bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) içinde belirtilen bir parametre kapsamı, bu lambda_expression, atma parametrelerinden oluşan bir parametre olan *anonymous_function_body* , bu *anonymous_method_expression*bir *anonymous_method_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) tarafından belirtilen bir *blok* olan *lambda_expression* .

## <a name="related-spec-sections"></a>İlgili belirtim bölümleri
- [Karşılık gelen parametreler](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#corresponding-parameters)
