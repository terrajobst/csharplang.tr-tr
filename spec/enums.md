---
ms.openlocfilehash: 3b142d7dbda8a94a4cf2c973a2e380065dcbf5ee
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703962"
---
# <a name="enums"></a>Numaralandırmalar

***Sabit listesi türü*** , adlandırılmış sabitler kümesini bildiren ayrı bir değer türüdür ([değer türleri](types.md#value-types)).

Örnek

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

`Red`, `Green`ve `Blue`üyeleriyle `Color` adlı bir sabit listesi türü bildirir.

## <a name="enum-declarations"></a>Sabit Listesi bildirimleri

Sabit listesi bildirimi yeni bir sabit listesi türü bildirir. Bir numaralandırma bildirimi, anahtar sözcük `enum`başlar ve ad, erişilebilirlik, temel türü ve sabit listesinin üyelerini tanımlar.

```antlr
enum_declaration
    : attributes? enum_modifier* 'enum' identifier enum_base? enum_body ';'?
    ;

enum_base
    : ':' integral_type
    ;

enum_body
    : '{' enum_member_declarations? '}'
    | '{' enum_member_declarations ',' '}'
    ;
```

Her sabit listesi türünün, enum türünün ***temel alınan türü*** olarak adlandırılan karşılık gelen bir integral türü vardır. Bu temel tür, numaralandırmada tanımlanan tüm Numaralandırıcı değerlerini temsil edebilmelidir. Bir numaralandırma bildirimi, temel alınan `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`türünü açıkça bildirebilir. `char` temel alınan bir tür olarak kullanılamayacağını unutmayın. Temel alınan bir türü açıkça bildirmiyor bir numaralandırma bildirimi, temel alınan bir tür `int`sahiptir.

Örnek

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

temel alınan `long`bir sabit listesi bildirir. Bir geliştirici, örnekte olduğu gibi, `long` aralığı içinde olan ancak `int`aralığında olmayan değerlerin kullanımını etkinleştirmek ya da gelecekte bu seçeneği korumak için `long`temel alınan bir tür kullanmayı tercih edebilir.

## <a name="enum-modifiers"></a>Sabit Listesi değiştiricileri

*Enum_declaration* , isteğe bağlı olarak bir numaralandırma değiştiricileri sırası içerebilir:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Aynı değiştiricinin bir numaralandırma bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.

Bir numaralandırma bildiriminin değiştiricileri, bir sınıf bildirimiyle ([sınıf değiştiricileri](classes.md#class-modifiers)) aynı anlama sahiptir. Ancak, `abstract` ve `sealed` değiştiricilerine bir numaralandırma bildiriminde izin verilmeyeceğini unutmayın. Numaralandırmalar özet olamaz ve türetmeye izin vermez.

## <a name="enum-members"></a>Sabit listesi üyeleri

Enum türü bildiriminin gövdesi, sabit listesi türünün adlandırılmış sabitleri olan sıfır veya daha fazla numaralandırma üyesini tanımlar. İki numaralandırma üyesi aynı ada sahip olamaz.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Her sabit listesi üyesinin ilişkili bir sabit değeri vardır. Bu değerin türü, kapsayan Enum için temel alınan türdür. Her bir sabit listesi üyesinin sabit değeri, sabit listesinin temel alınan türünün aralığında olmalıdır. Örnek

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

`-1`, `-2`ve `-3` sabit değerleri temeldeki integral türü `uint`aralığında olmadığından, derleme zamanı hatası oluşur.

Birden çok enum üyesi aynı ilişkili değeri paylaşabilir. Örnek

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

iki sabit listesi üyesinin (`Blue` ve `Max`) aynı ilişkili değere sahip olan bir Enum gösterir.

Sabit Listesi üyesinin ilişkili değeri örtük veya açık olarak atanır. Enum üyesinin bildiriminde bir *constant_expression* başlatıcısı varsa, bu sabit ifadenin değeri örtük olarak numaralandırmanın temel alınan türüne dönüştürüldü, numaralandırma üyesinin ilişkili değeridir. Enum üyesinin bildiriminde Başlatıcı yoksa, ilişkili değeri örtük olarak aşağıdaki gibi ayarlanır:

*  Sabit listesi üyesi, numaralandırma türünde belirtilen ilk enum üyesiyse, ilişkili değeri sıfırdır.
*  Aksi takdirde, enum üyesinin ilişkili değeri, metin içeriğini eklemek önceki enum üyesinin ilişkili değeri bir ile arttırılarak elde edilir. Bu artış değeri, temel alınan tür tarafından gösterilebilen değer aralığı içinde olmalıdır, aksi takdirde bir derleme zamanı hatası oluşur.

Örnek

```csharp
using System;

enum Color
{
    Red,
    Green = 10,
    Blue
}

class Test
{
    static void Main() {
        Console.WriteLine(StringFromColor(Color.Red));
        Console.WriteLine(StringFromColor(Color.Green));
        Console.WriteLine(StringFromColor(Color.Blue));
    }

    static string StringFromColor(Color c) {
        switch (c) {
            case Color.Red: 
                return String.Format("Red = {0}", (int) c);

            case Color.Green:
                return String.Format("Green = {0}", (int) c);

            case Color.Blue:
                return String.Format("Blue = {0}", (int) c);

            default:
                return "Invalid color";
        }
    }
}
```

sabit listesi üye adlarını ve bunlarla ilişkili değerleri yazdırır. Çıktı:

```console
Red = 0
Green = 10
Blue = 11
```

Aşağıdaki nedenlerle:

*  Enum üyesi `Red` otomatik olarak sıfır değerine atanır (Başlatıcı olmadığından ve ilk numaralandırma üyesi olduğundan);
*  Enum üyesi `Green` açıkça `10`değer olarak verilir;
*  ve enum member `Blue`, metin içeriğini eklemek kendisinden önce gelen Üyeden bir daha büyük değere otomatik olarak atanır.

Enum üyesinin ilişkili değeri, doğrudan veya dolaylı olarak kendi ilişkili enum üyesinin değerini kullanamaz. Bu döngü kısıtlamasından farklı olarak, sabit listesi üye başlatıcıları, metinsel konumlarından bağımsız olarak diğer sabit listesi üye başlatıcılarına serbestçe başvurabilirler. Bir sabit listesi üye başlatıcısı içinde, diğer sabit listesi üyelerinin değerleri her zaman kendi temel türlerinin türüne sahip olarak değerlendirilir ve bu sayede diğer sabit listesi üyelerine başvuru yaparken yapılan yayınlar gerekli değildir.

Örnek

```csharp
enum Circular
{
    A = B,
    B
}
```

`A` ve `B` bildirimleri dairesel olduğundan derleme zamanı hatası oluşur. `A` açık olarak `B` ve `B` `A` örtük olarak bağlıdır.

Sabit listesi üyeleri, sınıfların içindeki alanlara tam olarak benzer şekilde adlandırılır ve kapsamlandırılır. Bir sabit listesi üyesinin kapsamı, kapsayan enum türünün gövdesidir. Bu kapsam içinde, sabit listesi üyeleri kendi basit adlarıyla adlandırılabilir. Diğer tüm koddan, bir sabit listesi üyesinin adı, sabit listesi türünün adı ile nitelenmelidir. Sabit listesi üyelerinin tanımlanmış bir erişilebilirliği yoktur--bir numaralandırma üyesine, içerdiği numaralandırma türü erişilebilir ise erişilebilir.

## <a name="the-systemenum-type"></a>System. Enum türü

Tür `System.Enum`, tüm numaralandırma türlerinin soyut temel sınıfıdır (Bu, sabit listesi türünün temel alınan türünden farklıdır) ve `System.Enum` devralınan Üyeler herhangi bir numaralandırma türünde kullanılabilir. Herhangi bir numaralandırma türünden `System.Enum`için bir paketleme dönüştürmesi ([kutulama dönüşümleri](types.md#boxing-conversions)) vardır ve `System.Enum` herhangi bir numaralandırma türüne bir kutudan çıkarma dönüştürmesi ([kutudan çıkarma dönüştürmeleri](types.md#unboxing-conversions)) bulunur.

`System.Enum` kendisinin bir *enum_type*olmadığı unutulmamalıdır. Bunun yerine, tüm *enum_type*öğeleri türetildiği bir *class_type* . Tür `System.Enum`, `System.ValueType` ([System. ValueType türü](types.md#the-systemvaluetype-type)) türünden devralınır, bu, sırasıyla tür `object`devralır. Çalışma zamanında, `System.Enum` türünde bir değer `null` veya herhangi bir numaralandırma türünün paketlenmiş bir değerine bir başvuru olabilir.

## <a name="enum-values-and-operations"></a>Sabit listesi değerleri ve işlemleri

Her enum türü farklı bir tür tanımlar; bir numaralandırma türü ile integral türü arasında veya iki sabit listesi türü arasında dönüştürme yapmak için açık bir numaralandırma dönüştürmesi ([açık numaralandırma dönüştürmeleri](conversions.md#explicit-enumeration-conversions)) gereklidir. Bir numaralandırma türünün üzerinde sürebelirlenen değerler kümesi, sabit listesi üyeleri tarafından sınırlı değildir. Özellikle, bir sabit listesinin temel alınan türünün herhangi bir değeri, sabit listesi türüne dönüşebilir ve bu sabit listesi türünün ayrı bir geçerli değeridir.

Sabit listesi üyeleri, kendi kapsayan enum türünde (diğer sabit listesi üye başlatıcıları dışında: [enum üyelerine](enums.md#enum-members)bakın) türü vardır. `v` ilişkili değere sahip `E` enum türünde belirtilen enum üyesinin değeri `(E)v`.

Aşağıdaki işleçler, enum türlerinin değerleri üzerinde kullanılabilir: `==`, `!=`, `<`, `>`, `<=`, `>=` ([numaralandırma karşılaştırma işleçleri](expressions.md#enumeration-comparison-operators)), ikili `+` ([toplama işleci](expressions.md#addition-operator)), Ikili `-` ([çıkarma işleci](expressions.md#subtraction-operator)), `^`, `&`, `|` ([numaralandırma mantıksal Işleçleri](expressions.md#enumeration-logical-operators)), `~` ([bit düzeyinde tamamlama işleci](expressions.md#bitwise-complement-operator)), `++` ve `--` ( [Sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).

Her numaralandırma türü otomatik olarak `System.Enum` sınıfından türetilir (Bu, sırasıyla `System.ValueType` ve `object`türetilir). Bu nedenle, bu sınıfın devralınmış yöntemleri ve özellikleri, bir sabit listesi türünün değerleri üzerinde kullanılabilir.
