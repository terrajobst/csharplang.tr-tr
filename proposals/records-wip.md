---
ms.openlocfilehash: 258ae6865c5b2c3103a0cdf7e1e5a2cdee11e740
ms.sourcegitcommit: 1e1c7c72b156e2fbc54d6d6ac8d21bca9934d8d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80281963"
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

## <a name="with-expression"></a>`with` ifadesi

`with` ifadesi aşağıdaki sözdizimini kullanan yeni bir ifadedir.

```antlr
with_expression
    : switch_expression
    | switch_expression 'with' anonymous_object_initializer
```

`with` bir ifade, `anonymous_object_initializer`listelenen özelliklerde yapılan değişikliklerle alıcı ifadesinin bir kopyasını oluşturmak için tasarlanan "bozucu olmayan" bir yıkıcı "sağlar.

Geçerli bir `with` ifadesi void olmayan bir türe sahip bir alıcıya sahip. Alıcı türü, uygun parametreler ve dönüş türü ile `With` adlı erişilebilir bir örnek yöntem içermelidir. Birden çok geçersiz kılma olmayan `With` yöntemi varsa bu bir hatadır. Birden çok `With` geçersiz kılma varsa, hedef yöntemi olan geçersiz kılma olmayan bir `With` yöntemi olmalıdır. Aksi takdirde, tam olarak bir `With` yöntemi olmalıdır.

`with` ifadesinin sağ tarafında, atamanın sol tarafındaki alıcının bir alanı veya özelliği ile bir dizi atamalarla birlikte bir `anonymous_object_initializer` ve sağ tarafta, sol taraftaki türe örtük olarak dönüştürülebilir bir rastgele ifade olan bir.

Hedef `With` yöntemi verildiğinde, dönüş türü alıcı ifadesi türünün türü veya temel bir tür olmalıdır. `With` yönteminin her parametresi için, aynı ada ve aynı türe sahip alıcı türünde bir erişilebilir karşılık gelen örnek alanı veya okunabilir özellik olmalıdır. WITH ifadesinin sağ tarafındaki her bir özellik veya alan, `With` yönteminde aynı ada sahip bir parametreye de karşılık gelmelidir.

Geçerli bir `With` yöntemi verildiğinde, bir `with` ifadesinin değerlendirmesi, `With` yöntemini, sol taraftaki özelliği ile aynı ada sahip parametre için değiştirilen `anonymous_object_initializer` ifadelerle çağırarak eşdeğerdir. `anonymous_object_initializer`belirli bir parametre için eşleşen bir özellik yoksa, bağımsız değişken alıcı üzerinde aynı ada sahip alanın veya özelliğin değerlendirmesinden oluşur.

Yan etkileri değerlendirmesi sırası, her bir ifade tam olarak bir kez hesaplandıktan sonra aşağıdaki gibidir:

1. Alıcı ifadesi

2. `anonymous_object_initializer`, sözcük temelli sırada olan ifadeler

3. `With` yöntemi parametrelerinin tanımı sırasına göre `With` yöntemi parametreleriyle eşleşen özelliklerin değerlendirilmesi.