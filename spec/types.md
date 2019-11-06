---
ms.openlocfilehash: 088c4a77cecde490c556c44c239a3496f896582e
ms.sourcegitcommit: 4ddf18d000734c1b6d0a48127bf338086fc3f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616135"
---
# <a name="types"></a>Türler

C# Dilin türleri iki ana kategoriye ayrılmıştır: ***değer türleri*** ve ***başvuru türleri***. Değer türleri ve başvuru türleri, bir veya daha fazla ***tür parametresi***alacak ***Genel türler***olabilir. Tür parametreleri hem değer türlerini hem de başvuru türlerini belirleyebilir.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Türlerin son kategorisi, işaretçiler, yalnızca güvenli olmayan kodda kullanılabilir. Bu, [işaretçi türlerinde](unsafe-code.md#pointer-types)daha fazla ele alınmıştır.

Değer türleri, değer türlerinin değişkenlerinin verilerini doğrudan içerdiği başvuru türlerinden farklıdır, ancak başvuru türündeki değişkenler, ikinci olarak ***nesneler***olarak bilinmekte olan verilerine ***başvuruları*** depolar. Başvuru türleriyle, iki değişken aynı nesneye başvurabilir ve bu nedenle bir değişkende işlemler, diğer değişken tarafından başvurulan nesneyi etkilemek için mümkündür. Değer türleriyle, her birinin kendi verilerinin bir kopyasına sahip olduğu ve bir üzerindeki işlemlerin diğerini etkilemesi mümkün değildir.

C#tür sistemi, herhangi bir tür değeri bir nesne olarak işlenemeyeceği şekilde birleştirilmiştir. Her tür doğrudan C# veya dolaylı olarak `object` sınıf türünden türetilir ve `object` tüm türlerin en son temel sınıfıdır. Başvuru türlerinin değerleri, `object`türü olarak değerleri görüntüleyerek nesne olarak değerlendirilir. Değer türlerinin değerleri, kutulama ve kutudan çıkarma işlemleri ([kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma) gerçekleştirerek nesneler olarak değerlendirilir.

## <a name="value-types"></a>Değer türleri

Değer türü bir struct türü ya da bir numaralandırma türüdür. C#***basit türler***adlı önceden tanımlanmış bir yapı türleri kümesi sağlar. Basit türler, ayrılmış sözcükler aracılığıyla tanımlanır.

```antlr
value_type
    : struct_type
    | enum_type
    ;

struct_type
    : type_name
    | simple_type
    | nullable_type
    ;

simple_type
    : numeric_type
    | 'bool'
    ;

numeric_type
    : integral_type
    | floating_point_type
    | 'decimal'
    ;

integral_type
    : 'sbyte'
    | 'byte'
    | 'short'
    | 'ushort'
    | 'int'
    | 'uint'
    | 'long'
    | 'ulong'
    | 'char'
    ;

floating_point_type
    : 'float'
    | 'double'
    ;

nullable_type
    : non_nullable_value_type '?'
    ;

non_nullable_value_type
    : type
    ;

enum_type
    : type_name
    ;
```

Bir başvuru türü değişkeninin aksine, bir değer türü değişkeni, yalnızca değer türü null yapılabilir bir tür ise `null` değerini içerebilir.  Null yapılamayan her değer türü için, aynı değer kümesini ve `null`değeri belirten karşılık gelen null yapılabilir bir değer türü vardır.

Değer türünde bir değişkene atama, atanmakta olan değerin bir kopyasını oluşturur. Bu, atamadan bir başvuru türü değişkenine farklılık gösterir ve başvuruyu, başvuru tarafından tanımlanan nesneyi kopyalar.

### <a name="the-systemvaluetype-type"></a>System. ValueType türü

Tüm değer türleri dolaylı olarak `System.ValueType`sınıfından devralır, bu, sırasıyla sınıf `object`devralır. Herhangi bir türün bir değer türünden türemesini mümkün değildir ve değer türleri örtülü olarak mühürlenebilir ([korumalı sınıflardır](classes.md#sealed-classes)).

`System.ValueType` kendisinin bir *value_type*olmadığı unutulmamalıdır. Bunun yerine, tüm *value_type*s 'nin otomatik olarak türetildiği bir *class_type* .

### <a name="default-constructors"></a>Varsayılan oluşturucular

Tüm değer türleri örtük olarak ***Varsayılan Oluşturucu***adlı ortak parametresiz bir örnek Oluşturucusu bildirir. Varsayılan Oluşturucu değer türü için ***varsayılan değer*** olarak bilinen sıfır ile başlatılmış bir örnek döndürür:

*  Tüm *Simple_Type*s için, varsayılan değer tüm sıfırlardan oluşan bir bit düzeniyle oluşturulan değerdir:
    * `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`ve `ulong`için varsayılan değer `0`.
    * `char`için varsayılan değer `'\x0000'`.
    * `float`için varsayılan değer `0.0f`.
    * `double`için varsayılan değer `0.0d`.
    * `decimal`için varsayılan değer `0.0m`.
    * `bool`için varsayılan değer `false`.
*  Bir *enum_type* `E`için, varsayılan değer `E`türüne dönüştürülür `0`.
*  Bir *struct_type*için varsayılan değer, tüm değer türü alanları varsayılan değerlerine ve tüm başvuru türü `null`alanlarına ayarlanarak oluşturulan değerdir.
*  Bir *nullable_type* için varsayılan değer `HasValue` özelliğinin false olduğu ve `Value` özelliğinin tanımsız olduğu bir örneğidir. Varsayılan değer null yapılabilir türün ***null değeri*** olarak da bilinir.

Diğer örnek Oluşturucu gibi, bir değer türünün varsayılan Oluşturucusu `new` işleci kullanılarak çağrılır. Verimlilik açısından, bu gereksinim aslında uygulamanın bir Oluşturucu çağrısı oluşturması için tasarlanmamıştır. Aşağıdaki örnekte, `i` ve `j` değişkenleri sıfır olarak başlatılır.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Her değer türünün örtük olarak ortak parametresiz bir örnek Oluşturucusu olduğundan, bir yapı türünün parametresiz bir oluşturucunun açık bir bildirimini içermesi mümkün değildir. Yapı türüne ancak parametreli örnek oluşturucuları ([oluşturucular](structs.md#constructors)) bildirme izni verilir.

### <a name="struct-types"></a>Yapı türleri

Yapı türü, sabitler, alanlar, Yöntemler, özellikler, Dizin oluşturucular, işleçler, örnek oluşturucular, statik oluşturucular ve iç içe geçmiş türleri bildirebilen bir değer türüdür. Struct türlerinin bildirimi [struct bildirimlerinde](structs.md#struct-declarations)açıklanmıştır.

### <a name="simple-types"></a>Basit türler

C#***basit türler***adlı önceden tanımlanmış bir yapı türleri kümesi sağlar. Basit türler ayrılmış sözcükler aracılığıyla tanımlanır, ancak aşağıdaki tabloda açıklandığı gibi, bu ayrılmış sözcükler `System` ad alanındaki önceden tanımlanmış yapı türleri için diğer adlardır.


| __Ayrılmış sözcük__ | __Diğer ad türü__ |
|-------------------|------------------|
| `sbyte`           | `System.SByte`   | 
| `byte`            | `System.Byte`    | 
| `short`           | `System.Int16`   | 
| `ushort`          | `System.UInt16`  | 
| `int`             | `System.Int32`   | 
| `uint`            | `System.UInt32`  | 
| `long`            | `System.Int64`   | 
| `ulong`           | `System.UInt64`  | 
| `char`            | `System.Char`    | 
| `float`           | `System.Single`  | 
| `double`          | `System.Double`  | 
| `bool`            | `System.Boolean` | 
| `decimal`         | `System.Decimal` | 

Basit bir tür bir struct türü olarak diğer ad olduğundan, her basit türün üyeleri vardır. Örneğin, `int`, `System.Int32` ve `System.Object`devralınmış Üyeler ve aşağıdaki deyimlerde belirtilen üyelere sahiptir:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Basit türler, bazı ek işlemlere izin veren diğer yapı türlerinden farklıdır:

*  Çoğu basit tür değerleri *değişmez* değerler ([değişmez](lexical-structure.md#literals)değerler) yazılarak oluşturulacak şekilde izin verir. Örneğin, `123` `int` türünde bir değişmez değerdir ve `'a'` `char`türünde bir sabit değerdir. C#, genel olarak yapı türlerinin değişmez değerleri için bir provizyon yapmaz ve diğer yapı türlerinin varsayılan olmayan değerleri, sonuçta bu yapı türlerinin örnek oluşturucular aracılığıyla her zaman oluşturulur.
*  Bir ifadenin işlenenleri tüm basit tür sabitlersiyse, derleyicinin derlemeyi derleme zamanında değerlendirmesi mümkündür. Böyle bir ifade, *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) olarak bilinir. Diğer yapı türleri tarafından tanımlanan işleçleri içeren ifadeler sabit ifadeler olarak kabul edilmez.
*  `const` bildirimleri aracılığıyla basit türlerin ([sabitler](classes.md#constants)) sabitlerini bildirmek mümkündür. Diğer yapı türlerinin sabitlerinin olması mümkün değildir, ancak `static readonly` alanlar tarafından benzer bir efekt sağlanır.
*  Basit türler içeren dönüştürmeler, diğer yapı türleri tarafından tanımlanan dönüştürme işleçleri değerlendirmesinde yer alabilir, ancak kullanıcı tanımlı bir dönüştürme işleci hiçbir şekilde Kullanıcı tanımlı başka bir işlecin değerlendirilmesine katılamaz ([değerlendirmesi Kullanıcı tanımlı dönüştürmeler](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Integral türleri

C#Dokuz integral türünü destekler: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`ve `char`. İntegral türleri aşağıdaki boyutlara ve değer aralıklarına sahiptir:

*  `sbyte` türü,-128 ile 127 arasında değerleri olan işaretli 8 bit tamsayıları temsil eder.
*  `byte` türü, 0 ile 255 arasında değerler içeren işaretsiz 8 bit tamsayılar temsil eder.
*  `short` türü,-32768 ile 32767 arasında değerleri olan işaretli 16 bit tamsayıları temsil eder.
*  `ushort` türü, 0 ile 65535 arasında değerler içeren işaretsiz 16 bit tamsayılar temsil eder.
*  `int` türü,-2147483648 ve 2147483647 değerlerini içeren imzalı 32 bitlik tamsayılar temsil eder.
*  `uint` türü, 0 ile 4294967295 arasındaki değerlere sahip işaretsiz 32 bitlik tamsayılar temsil eder.
*  `long` türü,-9223372036854775808 ve 9223372036854775807 değerlerini içeren imzalı 64 bitlik tamsayıları temsil eder.
*  `ulong` türü, 0 ile 18446744073709551615 arasında değerler içeren işaretsiz 64 bitlik tamsayılar temsil eder.
*  `char` türü, 0 ile 65535 arasında değerler içeren işaretsiz 16 bit tamsayılar temsil eder. `char` türü için olası değerler kümesi Unicode karakter kümesine karşılık gelir. `char`, `ushort`aynı gösterimine sahip olsa da, bir tür üzerinde izin verilen tüm işlemlere izin verilmez.

Tam sayı türü birli ve ikili işleçler her zaman imzalanmış 32 bit duyarlık, imzasız 32 bit duyarlık, imzalı 64 bit duyarlık veya imzasız 64-bit duyarlık ile çalışır:

*  Birli `+` ve `~` işleçleri için, işlenen `T`türüne dönüştürülür, burada `T`, işlenenin tüm olası değerlerini tam olarak temsil eden `int`, `uint`, `long`ve `ulong`. Daha sonra işlem `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T`.
*  Birli `-` işleci için, işlenen `T`türüne dönüştürülür; burada `T`, işlenenin tüm olası değerlerini tam olarak temsil eden `int` ve `long`. Daha sonra işlem `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T`. Birli `-` işleci `ulong`türündeki işlenenlere uygulanamaz.
*  İkili `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`ve `<=` işleçleri için işlenen, her iki işlenenin olası tüm değerlerini tam olarak temsil eden `int`, `uint`, `long`ve `ulong` ilk `T` `T`türüne dönüştürülür. Daha sonra işlem, `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T` (veya ilişkisel işleçler için `bool`). Bir işlenenin `long` türü ve diğeri de ikili işleçlerle `ulong` türünden olması için izin verilmez.
*  İkili `<<` ve `>>` işleçleri için, sol işlenen `T`türüne dönüştürülür; burada `T` ilki, `int`, `uint`ve `long`, işlenenin tüm olası değerlerini tam olarak temsil edebilir. Daha sonra işlem `T`türü duyarlık kullanılarak gerçekleştirilir ve sonucun türü `T`.

`char` türü bir integral türü olarak sınıflandırıldı, ancak diğer integral türlerinden iki şekilde farklı olur:

*  Diğer türlerden `char` türüne örtük dönüştürme yok. Özellikle, `sbyte`, `byte`ve `ushort` türleri `char` türünü kullanarak tam olarak gösterilemeyen değer aralıklarına sahip olsa da, `sbyte`, `byte`veya `ushort` ' ten örtülü dönüştürmeler yok `char`.
*  `char` türünün sabitleri, `char`türüne dönüştürme ile birlikte *character_literal*s veya as *integer_literal*s olarak yazılmalıdır. Örneğin, `(char)10` `'\x000A'`aynıdır.

`checked` ve `unchecked` işleçleri ve deyimleri, tam sayı türü aritmetik işlemler ve dönüştürmeler için taşma denetimini denetlemek için kullanılır ([denetlenen ve işaretlenmemiş işleçler](expressions.md#the-checked-and-unchecked-operators)). `checked` bağlamında, taşma bir derleme zamanı hatası üretir veya bir `System.OverflowException` oluşturulmasına neden olur. `unchecked` bağlamında, taşmalar yok sayılır ve hedef türüne uymayan tüm yüksek sıralı bitler atılır.

### <a name="floating-point-types"></a>Kayan nokta türleri

C#iki kayan nokta türünü destekler: `float` ve `double`. `float` ve `double` türleri, aşağıdaki değer kümelerini sağlayan 32 bitlik tek duyarlıklı ve 64 bit çift duyarlıklı IEEE 754 biçimleri kullanılarak temsil edilir:

*  Pozitif sıfır ve negatif sıfır. Çoğu durumda, pozitif sıfır ve negatif sıfır değeri basit değer sıfır ile aynı şekilde davranır, ancak belirli işlemler iki ([bölme işleci](expressions.md#division-operator)) arasında ayrım yapar.
*  Pozitif sonsuzluk ve negatif sonsuz. Sonsuz sayısı sıfır olmayan bir sayı sıfıra bölünerek bu işlemler tarafından oluşturulur. Örneğin, `1.0 / 0.0` pozitif sonsuz verir ve `-1.0 / 0.0` negatif sonsuz verir.
*  ***Bir sayı değil*** değeri genellikle kısaltılmış Nan. NaNs 'ler, sıfıra bölme gibi geçersiz kayan nokta işlemleri tarafından üretilir.
*  `s * m * 2^e`, `s` 1 veya-1 olduğu ve `m` ve `e` belirli kayan nokta türü tarafından belirlendiği şekilde, sıfır olmayan değerler kümesi: `float`, `0 < m < 2^24` ve `-149 <= e <= 104`ve `double`, `0 < m < 2^53` ve `-1075 <= e <= 970`. Normalleştirilmemiş kayan noktalı sayılar geçerli sıfır olmayan değerler olarak kabul edilir.

`float` türü, yaklaşık `1.5 * 10^-45` 7 basamaklı bir duyarlık ile `3.4 * 10^38` kadar olan değerleri temsil edebilir.

`double` türü, 15-16 basamaklı bir duyarlık ile `1.7 × 10^308` kadar yaklaşık `5.0 * 10^-324` değişen değerleri temsil edebilir.

Bir ikili işlecin işlenenlerinden biri kayan nokta türünde ise, diğer işlenen bir integral türü veya kayan nokta türünde olmalıdır ve işlem aşağıdaki gibi değerlendirilir:

*  İşlenenden biri integral türünde ise, bu işlenen diğer işlenenin kayan nokta türüne dönüştürülür.
*  Ardından, işlenenlerden biri `double`türünde ise, diğer işlenen `double`dönüştürülür, işlem en az `double` Aralık ve duyarlık kullanılarak yapılır ve sonucun türü `double` (veya ilişkisel işleçler için `bool`).
*  Aksi takdirde, işlem en az `float` Aralık ve duyarlık kullanılarak gerçekleştirilir ve sonucun türü `float` (veya ilişkisel işleçler için `bool`).

Atama işleçleri de dahil olmak üzere kayan nokta işleçleri hiçbir şekilde özel durum oluşturmaz. Bunun yerine, bazı durumlarda kayan nokta işlemleri, aşağıda açıklandığı gibi sıfır, sonsuz veya NaN üretir:

*  Kayan noktalı bir işlemin sonucu hedef biçim için çok küçükse, işlemin sonucu pozitif sıfır veya negatif sıfır olur.
*  Kayan noktalı bir işlemin sonucu hedef biçim için çok büyükse, işlemin sonucu pozitif sonsuzluk veya negatif sonsuzluk olur.
*  Kayan noktalı bir işlem geçersizse, işlemin sonucu NaN olur.
*  Kayan noktalı bir işlemin bir veya her iki işleneni de NaN ise, işlemin sonucu NaN olur.

Kayan nokta işlemleri, işlemin sonuç türünden daha yüksek bir duyarlıkla gerçekleştirilebilir. Örneğin, bazı donanım mimarileri, `double` türünden daha fazla Aralık ve duyarlık içeren bir "genişletilmiş" veya "uzun çift" kayan nokta türü destekler ve bu daha yüksek duyarlık türünü kullanarak tüm kayan nokta işlemlerini örtülü olarak gerçekleştirir. Yalnızca yüksek maliyetli performans, bu tür donanım mimarileri daha az duyarlılıkla kayan nokta işlemleri gerçekleştirmek için yapılabilir ve hem performans hem de duyarlık sağlamak için bir uygulama istemek yerine, daha yüksek bir duyarlık türüne C# izin verir Tüm kayan nokta işlemleri için kullanılabilir. Daha kesin sonuçlar sunmaya kıyasla, bu nadiren de ölçülebilir etkileri vardır. Bununla birlikte, çarpma 'nın `double` aralığın dışında kalan bir sonuç oluşturduğu `x * y / z`, ancak sonraki bölme geçici sonucu `double` aralığına geri getirirse, ifadenin bir daha yüksek Aralık biçimi sonsuz bir sonucun bir sonsuzluk yerine üretilmesine neden olabilir.

### <a name="the-decimal-type"></a>Ondalık tür

`decimal` türü, finansal ve parasal hesaplamalar için uygun olan 128 bitlik bir veri türüdür. `decimal` türü, 28-29 önemli bir basamakla `1.0 * 10^-28` yaklaşık olarak `7.9 * 10^28` kadar olan değerleri temsil edebilir.

`decimal` türü sınırlı değerler kümesi, işaret `s` 0 veya 1 olduğunda, katsayı `c` `0 <= *c* < 2^96`tarafından verildiği ve ölçek `e` `0 <= e <= 28`gibi bir `(-1)^s * c * 10^-e`formundadır. `decimal` türü, imzalanmış sıfırları, sonsuz veya NaN 'yi desteklemez. Bir `decimal`, on ' un bir kuvvetine ölçeklenerek 96 bitlik bir tamsayı olarak temsil edilir. `1.0m`sıfırdan küçük bir değer olan `decimal`s için, değer tamamen 28 ondalık hanesiz, ancak başka bir değer değildir. `1.0m`sıfırdan büyük veya buna eşit bir mutlak değere sahip `decimal`s için, değer 28 veya 29 haneye eşittir. `float` ve `double` veri türlerinin aksine, 0,1 gibi ondalık kesirli sayılar tam olarak `decimal` gösteriminde temsil edilebilir. `float` ve `double` temsilleri genellikle sonsuz kesirler olur ve bu temsiller, yuvarlama hatalarına karşı daha açıktır.

Bir ikili işlecin işlenenlerinden biri `decimal`türünde ise, diğer işlenenin bir integral türünde veya `decimal`türünde olması gerekir. İntegral tür işleneni varsa, işlem gerçekleştirilmeden önce `decimal` dönüştürülür.

`decimal` türündeki değerler üzerindeki bir işlemin sonucu, tam bir sonucun hesaplanmasının (ölçeği koruma, her operatör için tanımlanan şekilde koruma) ve ardından temsili sığacak şekilde yuvarlamasının sonuçlandığı anlamına gelir. Sonuçlar en yakın gösterilemeyen değere yuvarlanır ve bir sonuç iki gösterilemeyen değere eşit bir şekilde yakınsa, en az önemli basamak konumunda çift sayı olan değere ("banker 'in yuvarlanması" olarak bilinir). Sıfır sonucun her zaman 0 ve Ölçeği 0 olan bir imzası vardır.

Ondalık aritmetik işlem mutlak değerde `5 * 10^-29` değerinden küçük veya buna eşit bir değer üretirse, işlemin sonucu sıfır olur. Bir `decimal` aritmetik işlemi `decimal` biçimi için çok büyük bir sonuç üretirse, bir `System.OverflowException` oluşturulur.

`decimal` türünün daha fazla duyarlığı ancak kayan nokta türlerinden daha küçük bir aralığı vardır. Bu nedenle, kayan nokta türlerinden `decimal` dönüşümler, taşma özel durumları üretebilir ve `decimal` ' dan kayan nokta türlerine dönüşümler duyarlık kaybına neden olabilir. Bu nedenlerden dolayı, kayan nokta türleri ve `decimal`arasında örtük dönüştürmeler yoktur ve açık yayınlar olmadan, kayan nokta ve `decimal` işlenenlerini aynı ifadede karıştırmak mümkün değildir.

### <a name="the-bool-type"></a>Bool türü

`bool` türü, Boolean mantıksal miktarları temsil eder. `bool` türündeki olası değerler `true` ve `false`.

`bool` ve diğer türler arasında standart dönüştürme yok. Özellikle, `bool` türü benzersiz ve integral türlerden ayrıdır ve bir `bool` değeri bir tam sayı değeri yerine kullanılamaz ve tam tersi de geçerlidir.

C ve C++ dillerde sıfır tamsayı veya kayan nokta değeri ya da null işaretçisi, boolean değer `false`, sıfır olmayan bir tamsayı veya kayan nokta değeri veya null olmayan bir işaretçi, `true`Boole değerine dönüştürülebilir. ' C#De, bu tür dönüştürmeler, tam olarak bir integral veya kayan nokta değeri sıfıra karşılaştırılarak veya bir nesne başvurusunu `null`açıkça karşılaştırarak gerçekleştirilir.

### <a name="enumeration-types"></a>Numaralandırma türleri

Sabit listesi türü, adlandırılmış sabitleri olan ayrı bir türdür. Her numaralandırma türünün, `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`olması gereken temel bir türü vardır. Numaralandırma türünün değer kümesi, temel alınan türün değerleri kümesiyle aynıdır. Numaralandırma türünün değerleri, adlandırılmış sabitlerin değerleriyle sınırlı değildir. Numaralandırma türleri, numaralandırma bildirimleri ([enum bildirimleri](enums.md#enum-declarations)) aracılığıyla tanımlanır.

### <a name="nullable-types"></a>Boş değer atanabilir tipler

Null yapılabilir bir tür, ***temel türünün*** tüm değerlerini ve ek bir null değeri temsil edebilir. Null yapılabilir bir tür `T?`yazılır; burada `T` temel tür. Bu sözdizimi `System.Nullable<T>`için toplu bir özelliktir ve iki form birbirinin yerine kullanılabilir.

***Null yapılamayan bir değer türü*** , `System.Nullable<T>` dışında herhangi bir değer türü ve onun toplu `T?` (herhangi bir `T`için) ve null yapılamayan bir değer türü (yani, `struct` bir tür parametresi) olarak kısıtlanan herhangi bir tür parametresi. kısıtlama). `System.Nullable<T>` türü, `T` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) için değer türü kısıtlamasını belirtir, yani null yapılabilir bir türün temel alınan türünün hiçbir null yapılamayan değer türü olabileceği anlamına gelir. Null yapılabilir bir türün temel alınan türü null yapılabilir bir tür veya başvuru türü olamaz. Örneğin, `int??` ve `string?` geçersiz türlerdir.

Null yapılabilir bir `T?` tür örneğinin iki ortak salt okuma özelliği vardır:

*  `bool` türünde bir `HasValue` özelliği
*  `T` türünde bir `Value` özelliği

`HasValue` true olan bir örnek, null olmayan olarak kabul edilir. Null olmayan bir örnek bilinen bir değer içerir ve `Value` bu değeri döndürür.

`HasValue` false olan bir örnek null olarak kabul edilir. Null bir örnek tanımsız bir değere sahip. Null örneğinin `Value` okunmaya çalışılması `System.InvalidOperationException` oluşturulmasına neden olur. Null yapılabilir bir örneğin `Value` özelliğine erişme işlemi, ***sarmalama***olarak adlandırılır.

Varsayılan oluşturucuya ek olarak, her Nullable tür `T?` `T`türünde tek bir bağımsız değişken alan bir ortak oluşturucuya sahiptir. `T`türünde bir değer `x` verildiğinde, formun Oluşturucu çağrısı

```csharp
new T?(x)
```
`Value` özelliğinin `x``T?` null olmayan bir örneğini oluşturur. Verili bir değer için null değer olmayan bir türün null olmayan bir örneğini oluşturma işlemi ***sarmalama***olarak adlandırılır.

Örtük dönüştürmeler, `null` değişmez değerinden `T?` ([null değişmez değer dönüştürmeleri](conversions.md#null-literal-conversions)) ve `T` ile `T?` ([örtük Nullable dönüşümler](conversions.md#implicit-nullable-conversions)) arasında kullanılabilir.

## <a name="reference-types"></a>Başvuru türleri

Başvuru türü bir sınıf türü, arabirim türü, bir dizi türü veya temsilci türüdür.

```antlr
reference_type
    : class_type
    | interface_type
    | array_type
    | delegate_type
    ;

class_type
    : type_name
    | 'object'
    | 'dynamic'
    | 'string'
    ;

interface_type
    : type_name
    ;

array_type
    : non_array_type rank_specifier+
    ;

non_array_type
    : type
    ;

rank_specifier
    : '[' dim_separator* ']'
    ;

dim_separator
    : ','
    ;

delegate_type
    : type_name
    ;
```

Başvuru türü değeri, ikinci bir ***nesne***olarak bilinen türünün bir ***örneğine*** başvurudur. `null` özel değer tüm başvuru türleriyle uyumludur ve bir örneğin yokluğunu gösterir.

### <a name="class-types"></a>Sınıf türleri

Bir sınıf türü veri üyelerini (sabitler ve alanlar), işlev üyelerini (Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve iç içe geçmiş türleri içeren bir veri yapısını tanımlar. Sınıf türleri, türetilmiş sınıfların temel sınıfları genişletebilen ve özelleştireceği bir mekanizma olan devralmayı destekler. Sınıf türlerinin örnekleri, *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) kullanılarak oluşturulur.

Sınıf türleri [sınıflarda](classes.md)açıklanmıştır.

Önceden tanımlanmış bazı sınıf türleri, aşağıdaki tabloda açıklandığı C# gibi dilde özel anlamlara sahiptir.


| __Sınıf türü__     | __Açıklama__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Diğer tüm türlerin Ultimate temel sınıfı. Bkz. [nesne türü](types.md#the-object-type). | 
| `System.String`    | C# Dilin dize türü. [Dize türü](types.md#the-string-type)' ne bakın.         |
| `System.ValueType` | Tüm değer türlerinin temel sınıfı. Bkz. [System. ValueType türü](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Tüm sabit listesi türlerinin temel sınıfı. Bkz. [numaralandırmalar](enums.md).              |
| `System.Array`     | Tüm dizi türlerinin temel sınıfı. Bkz. [diziler](arrays.md).             |
| `System.Delegate`  | Tüm temsilci türlerinin temel sınıfı. Bkz. [Temsilciler](delegates.md).          |
| `System.Exception` | Tüm özel durum türlerinin temel sınıfı. Bkz. [özel durumlar](exceptions.md).         |

### <a name="the-object-type"></a>Nesne türü

`object` sınıf türü, diğer tüm türlerin en son temel sınıfıdır. Her tür doğrudan C# veya dolaylı olarak `object` sınıf türünden türetilir.

Anahtar sözcüğü `object` önceden tanımlanmış sınıf `System.Object`için yalnızca bir diğer addır.

### <a name="the-dynamic-type"></a>Dinamik tür

`object`gibi `dynamic` türü herhangi bir nesneye başvurabilir. İşleçler `dynamic`türündeki ifadelere uygulandığında, çözüm program çalıştırılıncaya kadar ertelenir. Bu nedenle, işleç başvurulan nesneye yasal olarak uygulanamazlar, derleme sırasında herhangi bir hata verilmez. Bunun yerine, işlecin çözümlenmesi çalışma zamanında başarısız olduğunda bir özel durum oluşturulur.

Amacı [, dinamik bağlamada](expressions.md#dynamic-binding)ayrıntılı olarak açıklanan dinamik bağlamaya izin versağlamaktır.

`dynamic`, aşağıdaki durumlar dışında `object` ile aynı kabul edilir:

*  `dynamic` türündeki ifadelerde işlemler, dinamik olarak bağlanabilir ([dinamik bağlama](expressions.md#dynamic-binding)).
*  Tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)), her ikisi de aday ise `object` `dynamic` tercih eder.

Bu denklik nedeniyle aşağıdakiler şunları içerir:

*  `object` ve `dynamic`arasında ve `dynamic` değiştirirken aynı olan oluşturulmuş türler arasında örtük bir kimlik dönüştürmesi vardır `object`
*  `object` ve öğesinden örtük ve açık dönüştürmeler, `dynamic`için de geçerlidir.
*  `dynamic` `object` ile değiştirme sırasında aynı olan yöntem imzaları aynı imza olarak değerlendirilir
*  Tür `dynamic`, çalışma zamanında `object` ayırt edilemez.
*  `dynamic` türünün bir ifadesi, ***dinamik ifade***olarak adlandırılır.

### <a name="the-string-type"></a>Dize türü

`string` türü, doğrudan `object`devralan kapalı bir sınıf türüdür. `string` sınıfının örnekleri Unicode karakter dizelerini temsil eder.

`string` türünün değerleri, dize değişmez değerleri ([dize sabit](lexical-structure.md#string-literals)değerleri) olarak yazılabilir.

Anahtar sözcüğü `string` önceden tanımlanmış sınıf `System.String`için yalnızca bir diğer addır.

### <a name="interface-types"></a>Arabirim türleri

Arabirim, bir sözleşmeyi tanımlar. Arabirim uygulayan bir sınıf veya yapı, sözleşmesine uymalıdır. Bir arabirim birden çok temel arabirimden devralınabilir ve bir sınıf ya da yapı birden çok arabirim uygulayabilir.

Arabirim türleri [arabirimlerde](interfaces.md)açıklanmıştır.

### <a name="array-types"></a>Dizi türleri

Dizi, hesaplanan dizinler üzerinden erişilen sıfır veya daha fazla değişken içeren bir veri yapısıdır. Bir dizide bulunan değişkenler, dizi öğeleri de denir, hepsi aynı türdür ve bu tür dizinin öğe türü olarak adlandırılır.

Dizi türleri [diziler](arrays.md)içinde açıklanmıştır.

### <a name="delegate-types"></a>Temsilci türleri

Bir temsilci, bir veya daha fazla yönteme başvuran bir veri yapısıdır. Örnek yöntemleri için de ilgili nesne örneklerine başvurur.

C veya C++ bir işlev işaretçisidir, ancak bir işlev işaretçisi yalnızca statik işlevlere başvurabilir, bir temsilci hem statik hem de örnek yöntemlerine başvurabilir. İkinci durumda, temsilci yalnızca metodun giriş noktasına bir başvuru değil, aynı zamanda yöntemi çağırmak için bir nesne örneğine de bir başvuru içerir.

Temsilci türleri [Temsilcilerde](delegates.md)açıklanmıştır.

## <a name="boxing-and-unboxing"></a>Kutulama ve kutudan çıkarma

Kutulama ve kutudan çıkarma kavramı, Merkezi C#'nin tür sistemidir. *Value_type* herhangi bir değerin `object`türüne ve türüne dönüştürülmesine izin vererek *value_type*s ve *reference_type*s arasında bir köprü sağlar. Kutulama ve kutudan çıkarma, herhangi bir türde bir değer olan tür sisteminin birleştirilmiş bir görünümünü, sonunda bir nesne olarak kabul edilebilir.

### <a name="boxing-conversions"></a>Paketleme dönüştürmeleri

Paketleme dönüştürmesi, bir *value_type* örtük olarak bir *reference_type*'e dönüştürülmesine izin verir. Aşağıdaki kutulama dönüştürmeleri mevcuttur:

*  Herhangi bir *value_type* türünden `object`.
*  Herhangi bir *value_type* türünden `System.ValueType`.
*  Herhangi bir *non_nullable_value_type* , *value_type*tarafından uygulanan herhangi bir *interface_type* .
*  Herhangi bir *nullable_type* , *nullable_type*temel türü tarafından uygulanan herhangi bir *interface_type* .
*  Herhangi bir *enum_type* türünden `System.Enum`.
*  Herhangi bir *nullable_type* , temel alınan *enum_type* `System.Enum`türüne sahiptir.
*  Bir tür parametresindeki örtük dönüştürmenin, çalışma zamanında bir değer türünden bir başvuru türüne ([tür parametreleri Içeren örtük dönüştürmeler](conversions.md#implicit-conversions-involving-type-parameters)) dönüştürülmesini istiyorsanız kutulama dönüştürmesi olarak yürütüleceğini unutmayın.

Bir *non_nullable_value_type* değerini paketleme bir nesne örneği ayırmayı ve *non_nullable_value_type* değerini bu örneğe kopyalamayı içerir.

Bir *nullable_type* değerinin kutulanması, `null` değer ise (`HasValue` `false`) veya temel alınan değeri sarmalamayı geri Kutulamadan ve Kutulamadan elde edilen null bir başvuru üretir.

Bir *non_nullable_value_type* değeri Kutulamak için gerçek işlem, aşağıdaki gibi bir genel ***paketleme sınıfının***mevcut olduğu gibi davranan, en iyi şekilde açıklanmıştır:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

`T` türündeki bir değerin `v` kutulenmesi, artık ifade `new Box<T>(v)`yürütmekten ve sonuçta elde edilen örneği `object`türünde bir değer olarak döndürmekten oluşur. Bu nedenle, deyimler
```csharp
int i = 123;
object box = i;
```
kavramsal olarak karşılık gelen
```csharp
int i = 123;
object box = new Box<int>(i);
```

Yukarıdaki `Box<T>` gibi bir paketleme sınıfı aslında yoktur ve paketlenmiş değerin dinamik türü aslında bir sınıf türü değildir. Bunun yerine, `T` türündeki paketlenmiş bir değer dinamik tür `T`ve `is` işleci kullanılarak dinamik bir tür denetimi yalnızca tür `T`başvurabilir. Örneğin,
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
, konsolundaki "`Box contains an int`" dizesini çıktısını alır.

Kutulama dönüştürme, kutulanmış değerin bir kopyasını yapmayı gerektirir. Bu, değerin aynı örneğe başvurmaya devam ettiği ve yalnızca daha az türetilmiş tür `object`olarak kabul edilen `object`türünde bir *reference_type* dönüştürmesinden farklıdır. Örneğin, bildirim verildiğinde
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Aşağıdaki deyimler
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
`box` `p` atamasında oluşan örtük paketleme işlemi, `p` değerinin kopyalanmasına neden olduğundan, konsolda 10 değerini çıktı olarak alır. `Point` `class` olarak bildirildiği için, `p` ve `box` aynı örneğe başvuracağından, 20 değeri çıktı olur.

### <a name="unboxing-conversions"></a>Kutudan çıkarma dönüştürmeleri

Kutudan çıkarma dönüştürmesi, bir *reference_type* açıkça bir *value_type*'e dönüştürülmesine izin verir. Aşağıdaki kutudan çıkarma dönüştürmeleri mevcuttur:

*  `object` türünden herhangi bir *value_type*.
*  `System.ValueType` türünden herhangi bir *value_type*.
*  Herhangi bir *interface_type* , *interface_type*uygulayan herhangi bir *non_nullable_value_type* .
*  Herhangi bir *interface_type* , temel alınan türü *interface_type*uygulayan herhangi bir *nullable_type* .
*  `System.Enum` türünden herhangi bir *enum_type*.
*  `System.Enum` türü, temel alınan bir *enum_type*ile herhangi bir *nullable_type* .
*  Bir tür parametresine açık dönüştürme, bir başvuru türünden bir değer türüne ([Açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions)) dönüştürme sona erdiğinde, bir paket açma dönüştürmesi olarak yürütülür.

Bir *non_nullable_value_type* kutudan çıkarma işlemi, nesne örneğinin verilen *non_nullable_value_type*'ın paketlenmiş bir değeri olup olmadığını kontrol ederek değeri örnekten dışarı kopyalamaktan oluşur.

Bir *nullable_type* öğesinin kutudan çıkarma işlemi, kaynak işleneni `null`veya nesne örneğini bir alt nesnenin temel alınan türüne *(Aksi takdirde* ) bir nesne örneğini kutudan çıkarma sonucu olarak, *nullable_type* null değerini üretir.

Önceki bölümde açıklanan sanal paketleme sınıfına işaret eden bir nesne için bir *value_type* `T` bir kutudan çıkarma dönüştürmesi, `((Box<T>)box).value`ifade `box` oluşur. Bu nedenle, deyimler
```csharp
object box = 123;
int i = (int)box;
```
kavramsal olarak karşılık gelen
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Belirli bir *non_nullable_value_type* , çalışma zamanında başarılı olmak için bir kutudan çıkarma dönüştürmesi için, kaynak işleneni değeri, bu *non_nullable_value_type*'ın paketlenmiş değerine bir başvuru olmalıdır. Kaynak işleneni `null`, bir `System.NullReferenceException` oluşturulur. Kaynak işleneni uyumsuz bir nesneye başvuru ise, bir `System.InvalidCastException` oluşturulur.

Belirli bir *nullable_type* , çalışma zamanında başarılı olmak için bir kutudan çıkarma dönüştürmesi için, kaynak işlenenin değeri `null` ya da *nullable_type*temeldeki *non_nullable_value_type* kutulanmış bir değere başvuru olmalıdır. Kaynak işleneni uyumsuz bir nesneye başvuru ise, bir `System.InvalidCastException` oluşturulur.

## <a name="constructed-types"></a>Oluşturulmuş türler

Genel tür bildirimi, tek başına, ***tür bağımsız değişkenleri***uygulama yoluyla birçok farklı tür oluşturmak için "Blueprint" olarak kullanılan ***ilişkisiz genel bir türü*** gösterir. Tür bağımsız değişkenleri, genel türün adının hemen ardından gelen açılı ayraç içinde (`<` ve `>`) yazılır. En az bir tür bağımsız değişkeni içeren bir türe ***oluşturulmuş tür***denir. Oluşturulmuş bir tür, bir tür adının görünebileceği dilde çoğu yerde kullanılabilir. İlişkisiz genel tür yalnızca bir *typeof_expression* içinde kullanılabilir ([typeof işleci](expressions.md#the-typeof-operator)).

Oluşturulan türler, ifadelerde basit adlar ([basit adlar](expressions.md#simple-names)) veya bir üyeye ([üye erişimi](expressions.md#member-access)) erişimi olarak da kullanılabilir.

Bir *namespace_or_type_name* değerlendirildiğinde, yalnızca doğru sayıda tür parametrelerine sahip genel türler kabul edilir. Bu nedenle, türlerin tür parametrelerine sahip olduğu sürece farklı türleri tanımlamak için aynı tanımlayıcıyı kullanmak mümkündür. Bu, genel ve genel olmayan sınıfları aynı programda karıştırdığınızda yararlı olur:

```csharp
namespace Widgets
{
    class Queue {...}
    class Queue<TElement> {...}
}

namespace MyApplication
{
    using Widgets;

    class X
    {
        Queue q1;            // Non-generic Widgets.Queue
        Queue<int> q2;       // Generic Widgets.Queue
    }
}
```

Bir *type_name* , doğrudan tür parametreleri belirtmese de, oluşturulmuş bir tür tanımlayabilir. Bu, bir türün genel sınıf bildiriminde iç içe kullanıldığı ve kapsayan bildirimin örnek türünün, ad araması için örtük olarak kullanıldığı bir durum olabilir ([genel sınıflarda Iç içe türler](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

Güvenli olmayan kodda, oluşturulan tür bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)) olarak kullanılamaz.

### <a name="type-arguments"></a>Tür bağımsız değişkenleri

Bir tür bağımsız değişkeni listesindeki her bağımsız değişken yalnızca bir *türdür*.

```antlr
type_argument_list
    : '<' type_arguments '>'
    ;

type_arguments
    : type_argument (',' type_argument)*
    ;

type_argument
    : type
    ;
```

Güvenli olmayan kodda ([güvenli olmayan kod](unsafe-code.md)), bir *type_argument* işaretçi türü olmayabilir. Her tür bağımsız değişkeni, karşılık gelen tür parametresindeki ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) tüm kısıtlamalara uymalıdır.

### <a name="open-and-closed-types"></a>Açık ve kapalı türler

Tüm türler ***Açık türler*** veya ***kapalı türler***olarak sınıflandırılabilirler. Açık tür, tür parametrelerini içeren bir türdür. Daha özel olarak:

*  Bir tür parametresi bir açık türü tanımlar.
*  Bir dizi türü, ve yalnızca kendi öğe türü açık bir tür ise açık bir türdür.
*  Oluşturulmuş bir tür, yalnızca bir veya daha fazla bağımsız değişkeni açık bir tür ise açık bir türdür. Oluşturulmuş iç içe bir tür, yalnızca bir veya daha fazla tür bağımsız değişkeni ya da kapsayan tür bağımsız değişkenleri bir açık tür ise açık bir türdür.

Kapalı bir tür, açık tür olmayan bir türdür.

Çalışma zamanında, genel bir tür bildirimi içindeki tüm kod, genel bildirime tür bağımsız değişkenleri uygulanarak oluşturulan kapalı oluşturulmuş bir tür bağlamında yürütülür. Genel tür içindeki her tür parametresi, belirli bir çalışma zamanı türüne bağlanır. Tüm deyimlerin ve ifadelerin çalışma zamanı işleme her zaman kapalı türlerle oluşur ve açık türler yalnızca derleme zamanı işleme sırasında oluşur.

Her kapatılan oluşturulmuş türün kendi statik değişkenleri kümesi vardır ve bu, başka bir kapalı oluşturulmuş türle paylaşılmaz. Çalışma zamanında açık bir tür olmadığından, bir açık türle ilişkili statik değişken yoktur. Aynı ilişkisiz genel türden oluşturulmuşsa ve ilgili tür bağımsız değişkenleri aynı türde ise, bu iki adet oluşturulmuş tür oluşturulur.

### <a name="bound-and-unbound-types"></a>Bağlı ve ilişkisiz türler

İlişkisiz terim ***türü*** genel olmayan bir türe veya ilişkisiz genel bir türe başvuruyor. Terim ***bağlantılı türü*** genel olmayan bir türe veya oluşturulmuş bir türe başvuruyor.

İlişkisiz bir tür, bir tür bildirimiyle belirtilen varlığı ifade eder. İlişkisiz genel tür bir tür değildir ve bir değişken, bağımsız değişken veya dönüş değeri ya da bir temel tür olarak kullanılamaz. İlişkisiz genel bir türün başvurabilecebileceği tek yapı `typeof` ifadedir ([typeof işleci](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Yer çağıran kısıtlamalar

Oluşturulmuş bir tür veya genel yönteme başvurulduğunda, sağlanan tür bağımsız değişkenleri genel tür veya yöntemde ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) belirtilen tür parametresi kısıtlamalarına göre denetlenir. Her bir `where` yan tümcesi için, adlandırılmış tür parametresine karşılık gelen `A` tür bağımsız değişkeni her bir kısıtlamaya göre aşağıdaki gibi denetlenir:

*  Kısıtlama bir sınıf türü, bir arabirim türü veya bir tür parametresi ise, kısıtlamada görünen tür parametrelerinin yerine sağlanan tür bağımsız değişkenleriyle bu kısıtlamayı `C`. Kısıtlamayı karşılamak için, tür `A`, aşağıdakilerden biri `C` türüne dönüştürülebilir olması gerekir:
    * Bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion))
    * Örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions))
    * Bir kutulama dönüştürmesi ([kutulama dönüşümleri](conversions.md#boxing-conversions)), türü null yapılamayan bir değer türüdür.
    * Bir tür parametresinden dolaylı başvuru, paketleme veya tür parametresi dönüştürme `C``A`.
*  Kısıtlama başvuru türü kısıtlamasıdır (`class`) `A` tür, aşağıdakilerden birini karşılamalıdır:
    * `A` bir arabirim türü, sınıf türü, temsilci türü veya dizi türüdür. `System.ValueType` ve `System.Enum` bu kısıtlamayı karşılayan başvuru türleri olduğunu unutmayın.
    * `A`, başvuru türü olarak bilinen bir tür parametresidir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
*  Kısıtlama değer türü kısıtlamasıdır (`struct`) `A` tür, aşağıdakilerden birini karşılamalıdır:
    * `A`, bir struct türü veya Enum türüdür, ancak null yapılabilir bir tür değildir. `System.ValueType` ve `System.Enum` bu kısıtlamayı karşılamayan başvuru türleri olduğunu unutmayın.
    * `A`, değer türü kısıtlamasına ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) sahip bir tür parametresidir.
*  Kısıtlama Oluşturucu kısıtlaması `new()`, tür `A` `abstract` olmamalı ve ortak parametresiz bir oluşturucuya sahip olmalıdır. Aşağıdakilerden biri doğruysa, bu karşılanır:
    * `A`, tüm değer türleri ortak bir varsayılan oluşturucuya ([Varsayılan oluşturucular](types.md#default-constructors)) sahip olduğundan bir değer türüdür.
    * `A`, Oluşturucu kısıtlamasına sahip bir tür parametresidir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
    * `A`, değer türü kısıtlamasına ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) sahip bir tür parametresidir.
    * `A`, `abstract` olmayan ve açıkça tanımlanmış ve parametresi olmayan bir `public` Oluşturucusu içeren bir sınıftır.
    * `A` `abstract` değildir ve varsayılan bir oluşturucuya ([Varsayılan oluşturucular](classes.md#default-constructors)) sahiptir.

Bir veya daha fazla tür parametresi kısıtlaması verilen tür bağımsız değişkenleri tarafından karşılanmıyorsa, derleme zamanı hatası oluşur.

Tür parametreleri devralınmadığından, kısıtlamalar hiçbir şekilde devralınmaz. Aşağıdaki örnekte `D`, `T` temel `B<T>`sınıf tarafından uygulanan kısıtlamayı karşılayacak şekilde, `T` tür parametresinde kısıtlamayı belirtmesi gerekir. Buna karşılık, `List<T>` herhangi bir `T`için `IEnumerable` uyguladığı için, sınıf `E` bir kısıtlama belirtmemelidir.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Tür parametreleri

Tür parametresi, parametrenin çalışma zamanında bağlandığı bir değer türü veya başvuru türü atayarak bir tanıtıcıdır.

```antlr
type_parameter
    : identifier
    ;
```

Bir tür parametresi birçok farklı gerçek tür bağımsız değişkeni ile örneklenmiş olduğundan, tür parametrelerinin farklı işlemleri ve kısıtlamaları diğer türlerden farklıdır. Bu güncelleştirmeler şunlardır:

*  Bir tür parametresi doğrudan bir taban sınıf ([temel sınıf](classes.md#base-class)) veya arabirim ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)) bildirmek için kullanılamaz.
*  Tür parametrelerine üye arama kuralları, varsa tür parametresine uygulanan kısıtlamalara bağlıdır. Bunlar [üye aramada](expressions.md#member-lookup)ayrıntılıdır.
*  Bir tür parametresi için kullanılabilir dönüştürmeler, varsa tür parametresine uygulanan kısıtlamalara bağlıdır. Bunlar tür parametreleri ve [Açık dinamik dönüştürmeleri](conversions.md#explicit-dynamic-conversions) [içeren örtük dönüştürmelerde](conversions.md#implicit-conversions-involving-type-parameters) ayrıntılandırılmıştır.
*  Tür parametresi bir başvuru türü ([tür parametreleri Içeren örtük dönüştürmeler](conversions.md#implicit-conversions-involving-type-parameters)) olarak bilinmediği sürece, sabit `null` tür parametresi tarafından verilen bir türe dönüştürülemez. Ancak, bunun yerine bir `default` ifadesi ([varsayılan değer ifadeleri](expressions.md#default-value-expressions)) kullanılabilir. Ayrıca, bir tür parametresi tarafından verilen bir değer, tür parametresi değer türü kısıtlamasına sahip olmadığı takdirde, `==` ve `!=` ([başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)) kullanılarak `null` karşılaştırılabilir.
*  `new` ifadesi ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) yalnızca tür parametresi bir *constructor_constraint* veya değer türü kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) tarafından kısıtlanmamışsa bir tür parametresiyle birlikte kullanılabilir.
*  Bir tür parametresi bir öznitelik içinde herhangi bir yerde kullanılamaz.
*  Bir statik üye veya iç içe bir tür tanımlamak için bir tür parametresi, üye erişimi ([üye erişimi](expressions.md#member-access)) veya tür adı ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) içinde kullanılamaz.
*  Güvenli olmayan kodda, bir tür parametresi bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)) olarak kullanılamaz.

Tür olarak, tür parametreleri yalnızca derleme zamanı yapısıdır. Çalışma zamanında, her tür parametresi genel tür bildirimine bir tür bağımsız değişkeni sağlanarak belirtilen bir çalışma zamanı türüne bağlanır. Bu nedenle, bir tür parametresiyle belirtilen değişkenin türü, çalışma zamanında, Kapalı oluşturulmuş bir tür ([açık ve kapalı türler](types.md#open-and-closed-types)) olur. Tüm deyimlerin ve tür parametreleri içeren ifadelerin çalışma zamanı yürütmesi, bu parametre için tür bağımsız değişkeni olarak sağlanan gerçek türü kullanır.

## <a name="expression-tree-types"></a>İfade ağacı türleri

***İfade ağaçları*** , lambda ifadelerinin yürütülebilir kod yerine veri yapıları olarak gösterilmesine izin verir. İfade ağaçları `System.Linq.Expressions.Expression<D>`form ***ifade ağacı türlerinin*** değerlerdir; burada `D` herhangi bir temsilci türüdür. Bu şartın geri kalanında, bu türlere toplu `Expression<D>`başvuracağız.

Lambda ifadesinden bir temsilci türü `D`bir dönüştürme varsa, ifade ağacı türü `Expression<D>`bir dönüştürme de mevcuttur. Bir lambda ifadesinin bir temsilci türüne dönüştürülmesi, lambda ifadesinin yürütülebilir koduna başvuran bir temsilci oluşturduğunda, bir ifade ağacı türüne dönüştürme lambda ifadesinin ifade ağacı gösterimini oluşturur.

İfade ağaçları, lambda ifadelerinin verimli bellek içi veri temsilleridir ve lambda ifadesinin yapısını saydam ve açık hale getirir.

Bir temsilci türü `D`benzer şekilde, `Expression<D>`, `D`ile aynı olan parametre ve dönüş türlerine sahip olarak kabul edilir.

Aşağıdaki örnek, hem yürütülebilir kod hem de bir ifade ağacı olarak bir lambda ifadesini temsil eder. `Func<int,int>`bir dönüştürme olduğundan, `Expression<Func<int,int>>`için de bir dönüştürme de mevcuttur:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Bu atamaları izleyerek temsilci `del` `x + 1`döndüren bir yönteme başvurur ve ifade ağacı `exp` ifade `x => x + 1`tanımlayan bir veri yapısına başvurur.

Genel tür `Expression<D>` tam tanımı ve bir lambda ifadesi bir ifade ağacı türüne dönüştürüldüğünde bir ifade ağacı oluşturmak için kesin kurallar, her ikisi de bu belirtim kapsamının dışındadır.

Açık hale getirmek için iki şey önemlidir:

*  Tüm lambda ifadeleri ifade ağaçlarına dönüştürülemez. Örneğin, deyim gövdeleriyle lambda ifadeleri ve atama ifadeleri içeren lambda ifadeleri temsil edilemez. Bu durumlarda, bir dönüştürme hala vardır ancak derleme zamanında başarısız olur. Bu özel durumlar [anonim işlev dönüştürmelerinde](conversions.md#anonymous-function-conversions)ayrıntılıdır.
*   `Expression<D>`, `D`türünde bir temsilci üreten bir örnek yöntemi `Compile` sunar:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Bu temsilciyi çağırmak, ifade ağacının gösterdiği kodun yürütülmesine neden olur. Bu nedenle, yukarıdaki tanımlar, del ve del2 eşdeğerdir ve aşağıdaki iki deyim aynı etkiye sahip olacaktır:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Bu kodu yürüttükten sonra, `i1` ve `i2` her ikisi de `2`değeri olur.

