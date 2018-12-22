# <a name="enums"></a>Numaralandırmalar

Bir ***sabit listesi türü*** ayrı değer türü ([değer türleri](types.md#value-types)), adlandırılmış sabitler kümesini bildirir.

Örnek

```csharp
enum Color
{
    Red,
    Green,
    Blue
}
```

adlı bir sabit listesi türü bildirir `Color` üyeleriyle `Red`, `Green`, ve `Blue`.

## <a name="enum-declarations"></a>Enum bildirimleri

Bir numaralandırma bildirimi yeni bir sabit listesi türü bildirir. With anahtar sözcüğü bir numaralandırma bildirimi başlar `enum`, adı, erişilebilirlik, temel alınan türü ve numaralandırma üyeleri tanımlar.

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

Her bir numaralandırma türünün adlı karşılık gelen bir integral türü olan ***temel alınan türü*** sabit listesi türü. Bu temel türünü numaralandırmada tanımlanan tüm Numaralayıcı değerleri temsil etmesi mümkün olması gerekir. Bir numaralandırma bildirimi bir temel alınan türü açıkça bildirebilir `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`. Unutmayın `char` bir temel türü kullanılamaz. Bir temel türü açıkça bildirmiyor bir numaralandırma bildirimi bir temel türüne sahip `int`.

Örnek

```csharp
enum Color: long
{
    Red,
    Green,
    Blue
}
```

temel alınan bir türü ile bir sabit listesi bildirir `long`. Bir geliştirici, temel alınan bir türünü kullanmayı tercih edebilirsiniz `long`aralığında olan değerleri kullanımını etkinleştirmek için örnekte gibi `long` ancak aralığını `int`, veya gelecek için bu seçeneği korumak için.

## <a name="enum-modifiers"></a>Enum değiştiriciler

Bir *enum_declaration* isteğe bağlı olarak bir dizi sabit listesi değiştiriciler içerebilir:

```antlr
enum_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;
```

Aynı değiştiricisi bir sabit listesi bildiriminde birden çok kez görünmesi için bir derleme zamanı hatasıdır.

Bu sınıf bildirimi aynı anlamı, bir numaralandırma bildirimi, değiştiricilere sahip ([sınıfı değiştiricileri](classes.md#class-modifiers)). Ancak, unutmayın, `abstract` ve `sealed` değiştiriciler bir sabit listesi bildiriminde izin verilmez. Numaralandırmalar, soyut olamaz ve türetme tanımaz.

## <a name="enum-members"></a>Numaralandırma üyeleri

Bir numaralandırma türü bildirimi gövdesi adlandırılmış sabitler sabit listesi türü, sıfır veya daha fazla numaralandırma üyeleri tanımlar. İki sabit listesi üye aynı ada sahip olabilir.

```antlr
enum_member_declarations
    : enum_member_declaration (',' enum_member_declaration)*
    ;

enum_member_declaration
    : attributes? identifier ('=' constant_expression)?
    ;
```

Her sabit listesi üyesi, ilişkili bir sabit değere sahip. Bu değer içeren numaralandırması için temeldeki türü türüdür. Her sabit listesi üyesi için sabit değerin numaralandırması için temeldeki türü aralığında olması gerekir. Örnek

```csharp
enum Color: uint
{
    Red = -1,
    Green = -2,
    Blue = -3
}
```

sonuçları bir derleme zamanı hatası nedeniyle sabit değerler `-1`, `-2`, ve `-3` integral türünün temel alınan bir aralıkta yer `uint`.

Birden çok sabit listesi üye aynı ilişkili değer paylaşabilir. Örnek

```csharp
enum Color 
{
    Red,
    Green,
    Blue,

    Max = Blue
}
```

içinde iki numaralandırma üyeleri--enum gösterir `Blue` ve `Max` --aynı değeri ilişkilendirdiniz.

İlişkili değer enum üyesini örtük veya açık olarak atanır. Sabit listesi üyesi bildirimi varsa bir *constant_expression* Başlatıcı, temel alınan numaralandırma türüne örtük olarak dönüştürülen bu sabit ifade değerini sabit listesi üyesi ilişkili değeridir. Sabit listesi üyesi bildirimi Başlatıcı varsa, ilişkili değeri örtük olarak, şu şekilde ayarlanır:

*  Sabit listesi üyesi enum türünde bildirilen ilk sabit listesi üyesi ise, ilişkili değeri sıfırdır.
*  Aksi takdirde, sabit listesi üyesi ilişkili değerini bir metin içeriğini eklemek önceki sabit listesi üyesi ilişkili değerini artırarak elde edilir. Daha fazla bu değer temel alınan türü tarafından temsil edilebilir değerler aralığında olması gerekir, aksi takdirde bir derleme zamanı hatası oluşur.

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

sabit listesi üye adları ve ilişkili değerleri yazdırır. Çıktı.

```
Red = 0
Green = 10
Blue = 11
```

Aşağıdaki nedenlerle:

*  sabit listesi üyesi `Red` değeri sıfır (bunu Başlatıcı içermiyor ve ilk sabit listesi üyesi olduğundan); otomatik olarak atanır
*  sabit listesi üyesi `Green` değeri açıkça verilir `10`;
*  ve sabit listesi üyesi `Blue` değeri metin içeriğini eklemek kendisinden üyenin büyük bir otomatik olarak atanır.

İlişkili değeri bir sabit listesi üyesi olabilir değil, doğrudan veya dolaylı olarak kendi ilişkili sabit listesi üyesi değerini kullanın. Bu döngü kısıtlama dışında sabit listesi üyesi başlatıcıları metinsel konumlarına bakmaksızın diğer sabit listesi üyesi başlatıcıları serbestçe başvurabilir. Böylece yayınları diğer enum üyelerine başvuru yaparken gerekli olmayan bir sabit listesi üyesi başlatıcısı içinde diğer sabit listesi üyelerinin değerlerini her zaman kendi temel türü türünü varmış gibi davranılır.

Örnek

```csharp
enum Circular
{
    A = B,
    B
}
```

sonuçları bir derleme zamanı hatası nedeniyle bildirimlerini `A` ve `B` döngüsel olan. `A` bağımlı `B` açıkça ve `B` bağlıdır `A` örtük olarak.

Numaralandırma üyeleri adlı ve sınıfları alanları tam olarak benzer bir şekilde kapsamlı. Sabit listesi üyesi kapsamını içeren enum türü gövdesidir. Bu kapsam içinde numaralandırma üyeleri için basit adlarına göre adlandırılır. Diğer tüm koddan bir sabit listesi üyesi adını numaralandırma türü adı ile nitelenmelidir. Numaralandırma üyeleri tüm bildirilen erişilebilirliği yok--sabit listesi üyesi, kapsayan bir enum türü erişilemiyorsa erişilebilir.

## <a name="the-systemenum-type"></a>System.Enum Türü

Türü `System.Enum` soyut temel sınıf (Bu, ayrı ve sabit listesi türü temel alınan türünden farklı) tüm sabit listesi türleri ve üyeleri devralınan `System.Enum` herhangi bir sabit listesi türü kullanılabilir. Kutulama dönüştürmesi ([kutulama dönüştürmeler](types.md#boxing-conversions)) herhangi bir sabit listesi Türü alanından mevcut `System.Enum`ve bir paket açma dönüşümünün ([kutudan çıkarma dönüştürme](types.md#unboxing-conversions)) öğesinden var. `System.Enum` herhangi bir sabit listesi türü için.

Unutmayın `System.Enum` kendisi değil bir *enum_type*. Bunun yerine, olan bir *class_type* tüm gelen *enum_type*s türetilir. Türü `System.Enum` tür tarafından devralındığında `System.ValueType` ([System.ValueType türü](types.md#the-systemvaluetype-type)), buna karşılık, tür tarafından devralındığında `object`. Çalışma zamanında, türünde bir değer `System.Enum` olabilir `null` veya paketlenmiş değere herhangi bir enum tür başvurusu.

## <a name="enum-values-and-operations"></a>Sabit listesi değerlerinin ve işlemler

Her bir numaralandırma türünün farklı bir tür tanımlar; Açık numaralandırma dönüştürme ([açık numaralandırma dönüştürmeler](conversions.md#explicit-enumeration-conversions)) enum türü ve integral türünde arasında veya iki sabit listesi türleri arasında dönüştürme için gereklidir. Enum türü alan değerleri kümesi numaralandırma üyelerini sınırlı değildir. Özellikle, bir sabit listesi türünü temel herhangi bir değerini numaralandırma türüne dönüştürülebilen ve sabit listesi türü farklı geçerli bir değer.

Numaralandırma üyeleri kendi içeren numaralandırma türünün sahiptir (dışındaki diğer sabit listesi üyesi başlatıcıları içinde: bkz [numaralandırma üyeleri](enums.md#enum-members)). Sabit listesi üyesi değerini sabit listesi türünde bildirilen `E` ilişkili değeriyle `v` olduğu `(E)v`.

Aşağıdaki işleçleri, değerleri sabit listesi türleri üzerinde kullanılabilir: `==`, `!=`, `<`, `>`, `<=`, `>=`  ([numaralandırma Karşılaştırma işleçleri](expressions.md#enumeration-comparison-operators)) , ikili `+`  ([Toplama işleci](expressions.md#addition-operator)), ikili `-`  ([çıkarma işleci](expressions.md#subtraction-operator)), `^`, `&` , `|`  ([Numaralandırma mantıksal işleçler](expressions.md#enumeration-logical-operators)), `~`  ([bit düzeyinde tamamlama işleci](expressions.md#bitwise-complement-operator)), `++` ve `--`  ([Sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).

Her sabit listesi türü otomatik olarak sınıfından türetilen `System.Enum` (, sırasıyla türetilir `System.ValueType` ve `object`). Bu nedenle, devralınan yöntemler ve özellikler bu sınıfın bir numaralandırma türünün değerleri üzerinde kullanılabilir.
