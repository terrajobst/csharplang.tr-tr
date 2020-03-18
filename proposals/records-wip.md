---
ms.openlocfilehash: 25756c1811d5e6dc97512ce70f99ab7fefa91c4a
ms.sourcegitcommit: 2a6dffb60718065ece95df75e1cc7eb509e48a8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "79485240"
---
# <a name="records-work-in-progress"></a>Süren Iş kayıtları

Diğer kayıt tekliflerinin aksine, bu bir teklif değildir, ancak kayıtlar özelliği için konsensus tasarım kararlarını kaydetmek üzere tasarlanan bir süren iş devam ediyor. Teknik ayrıntılar, soruları çözümlemek için gereken şekilde eklenecektir.

Bir kaydın sözdizimi aşağıdaki şekilde eklenmek üzere önerilir:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      parameter_list? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      parameter_list? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

Terminal dışı `attributes`, `data`yeni bir bağlamsal özniteliğe da izin verir.

Bir parametre listesi veya `data` değiştiricisi ile belirtilen bir sınıf (struct), biri kayıt türü olan bir kayıt sınıfı (kayıt yapısı) olarak adlandırılır.

Yalnızca bir parametre listesi ve `data` değiştiricisi olmadan bir kayıt türü bildirmek hatadır.

## <a name="members-of-a-record-type"></a>Bir kayıt türünün üyeleri

Sınıf gövdesinde belirtilen üyelere ek olarak, bir kayıt türü aşağıdaki ek üyelere sahiptir:

### <a name="primary-constructor"></a>Birincil Oluşturucu

Bir kayıt türünün, imzası tür bildiriminin değer parametrelerine karşılık gelen bir ortak Oluşturucusu vardır. Bu, türü için birincil Oluşturucu olarak adlandırılır ve örtük olarak belirtilen varsayılan oluşturucunun gizlenmesine neden olur. Bir birincil oluşturucuya ve sınıfta zaten aynı imzaya sahip bir oluşturucuya sahip olmak bir hatadır.
Çalışma zamanında birincil Oluşturucu 

1. sınıf gövdesinde görünen örnek alanı başlatıcıları 'nı yürütür; ve sonra bağımsız değişken olmadan temel sınıf oluşturucusunu çağırır.

1. değer parametrelerine karşılık gelen özellikler için derleyici tarafından oluşturulan yedekleme alanlarını başlatır (Bu özellikler derleyici tarafından sağlanmışsa), bkz. [sentezlenmiş Özellikler](#Synthesized Properties))


[] TODO: temel çağrı sözdizimi ve aşırı yükleme çözümlemesi aracılığıyla temel Oluşturucu seçme hakkında belirtim ekleyin

### <a name="properties"></a>Özellikler

Bir kayıt türü bildiriminin her bir kayıt parametresi için, Name ve Type değeri Parameter bildiriminden alınan karşılık gelen bir ortak özellik üyesi vardır. Bir get erişimcisine sahip ve bu ada ve türe sahip bir somut (soyut olmayan) özelliği yoksa, derleyici tarafından aşağıdaki gibi üretilir:

Bir kayıt yapısı veya kayıt sınıfı için:

* Ortak bir salt-al otomatik özelliği oluşturulur. Değeri, oluşturma sırasında karşılık gelen birincil Oluşturucu parametresinin değeri ile başlatılır. Her "eşleşen" devralınan soyut özelliğin get erişimcisi geçersiz kılındı.

### <a name="equality-members"></a>Eşitlik üyeleri

Kayıt türleri aşağıdaki yöntemler için birleştirilmiş uygulamalar üretir:

* korumalı veya Kullanıcı tarafından sağlanmadığı müddetçe `object.GetHashCode()` geçersiz kıl
* korumalı veya Kullanıcı tarafından sağlanmadığı müddetçe `object.Equals(object)` geçersiz kıl
* `T Equals(T)` yöntemi, `T` geçerli türdür

`T Equals(T)`, her birincil Oluşturucu parametresi ile aynı ada sahip özelliği diğer türün karşılık gelen özelliği ile karşılaştıran değer eşitlik gerçekleştirmek için belirtilir.
`object.Equals` eşdeğerini gerçekleştirir

```C#
override Equals(object o) => Equals(o as T);
```
