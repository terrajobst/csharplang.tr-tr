---
ms.openlocfilehash: a78567594d39fc4e204e12c4f2f247b8d6995c38
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484925"
---
# <a name="expression-variables-in-initializers"></a>Başlatıcılarda ifade değişkenleri

## <a name="summary"></a>Özet
[summary]: #summary

Alan başlatıcıları, özellik başlatıcıları, C# ctor-başlatıcıları ve sorgu yan tümcelerinde ifade değişkenleri (out değişkeni bildirimleri ve bildirim desenleri) içeren ifadelere izin vermek için 7 ' de tanıtılan özellikleri genişlettik.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bu, zaman eksikliğinden dolayı C# dilin bir yanındaki kaba kenarları tamamlar.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Bir ctor başlatıcısı içindeki ifade değişkenlerinin bildirimini (değişken bildirimleri ve bildirim desenleri) engellemeye yönelik kısıtlamayı kaldırdık. Bu tür bir belirtilen değişken, oluşturucunun gövdesi boyunca kapsamdadır.

Bir alan veya özellik başlatıcısındaki ifade değişkenlerinin bildirimini (değişken bildirimleri ve bildirim desenleri) engellemeye yönelik kısıtlamayı kaldırdık. Bu tür bir belirtilen değişken, başlatma ifadesi boyunca kapsamdadır.

Lambda gövdesine çevrilmiş bir sorgu ifadesi yan tümcesindeki ifade değişkenlerinin bildirimini (değişken bildirimleri ve bildirim desenleri) engelleyen kısıtlamayı kaldırdık. Bu tür bir belirtilen değişken, sorgu yan tümcesinin bu ifadesi boyunca kapsamdadır.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Yok.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Bu bağlamlarda belirtilen ifade değişkenleri için uygun kapsam belirgin değildir ve daha fazla LDM tartışmasını geri görür.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [] Bu değişkenler için uygun kapsam nedir?

## <a name="design-meetings"></a>Tasarım toplantıları

Yok.
