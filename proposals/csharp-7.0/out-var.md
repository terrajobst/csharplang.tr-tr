---
ms.openlocfilehash: a0d80afc47e9f0073237db9b8d7a4f0b045c1b0b
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484512"
---
# <a name="out-variable-declarations"></a>Out değişken bildirimleri

*Out değişkeni bildirimi* özelliği, bir değişkenin `out` bağımsız değişken olarak geçirildiği konumda bildirilmesini sağlar.

```antlr
argument_value
    : 'out' type identifier
    | ...
    ;
```

Bu şekilde bildirilmiştir bir değişken *Out değişkeni*olarak adlandırılır. Değişken türü için `var` bağlamsal anahtar sözcüğünü kullanabilirsiniz. Kapsam, model eşleştirme ile tanıtılan bir *model değişkeni* ile aynı olacaktır.

Dil belirtimine göre (Bölüm 7.6.7 öğe erişimi) bir öğe erişimi (Dizin oluşturma ifadesi) bağımsız değişken listesi, ref veya out bağımsız değişkenleri içermez. Ancak, çeşitli senaryolar için derleyici tarafından izin verilir, örneğin `out`kabul eden meta verilerde belirtilen dizin oluşturucular.

Bir argument_value tarafından tanıtılan yerel bir değişken kapsamında, bu yerel değişkene, bildiriminden önce gelen metinsel bir konumda başvuruda bulunmak için derleme zamanı hatası vardır.

Ayrıca, kendi bildirimini içeren aynı bağımsız değişken listesinde örtük olarak yazılmış bir (§ 8.5.1) Out değişkenine başvurmak için de bir hatadır.

Aşırı yükleme çözümlemesi aşağıdaki şekilde değiştirilmiştir:

Yeni bir dönüştürme ekledik:

> Örtük olarak yazılmış bir değişken bildiriminden her türe bir *ifadeden dönüşüm* vardır.

Açabilirsiniz

> Açıkça yazılmış bir out değişkeni bağımsız değişkeninin türü, belirtilen türdür.

ve

> Örtük olarak yazılmış bir değişken bağımsız değişkeninin türü yok.

Örtük olarak yazılmış bir değişken bildiriminden *ifadeden dönüştürme* , deyimden başka herhangi bir *dönüşümden*daha iyi düşünülmez.

Örtük olarak yazılmış bir değişkenin türü, aşırı yükleme çözümlemesi tarafından seçilen yöntemin imzasında karşılık gelen parametrenin türüdür.

Yeni sözdizimi düğümü `DeclarationExpressionSyntax`, bildirimi bir out var bağımsız değişkeninde temsil edecek şekilde eklenir.
