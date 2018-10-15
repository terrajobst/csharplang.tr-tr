# <a name="types"></a>Türler

C# dili türlerini iki ana kategoriye ayrılır: ***değer türleri*** ve ***başvuru türleri***. Hem değer türleri ve başvuru türleri olabilir ***genel türler***, bir veya daha fazla almakta ***tür parametrelerindeki***. Tür parametreleri, her iki değer türleri belirlemek ve başvuru türleri.

```antlr
type
    : value_type
    | reference_type
    | type_parameter
    | type_unsafe
    ;
```

Son kategori türlerinin işaretçileri, yalnızca güvenli olmayan kod içinde kullanılabilir. Bunu ele alınan ayrıntılı olarak [işaretçi türleri](unsafe-code.md#pointer-types).

Değer türleri farklı başvuru türleri, değer türlerinin değişkenleri kendi verilerini doğrudan içerdiği değişkenleri başvuru türleri ise depolama ***başvuruları*** ikinci olarak bilinen verilerine, ***nesneleri***. Başvuru türleri ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur. Değer türleri ile her değişkenleri kendi veri kopyası vardır ve yapılan işlemlerin diğerini etkilemesi olanaklı birinde değil.

C# tür sistemi, herhangi bir türde bir değer bir nesne olarak davranılıp şekilde birleşiktir. C# ' de her tür doğrudan veya dolaylı olarak türetir `object` sınıf türü, ve `object` tüm türlerin ultimate temel sınıftır. Başvuru türlerindeki değerleri nesneler olarak değer türü olarak yalnızca görüntüleyerek edilir `object`. Değer türlerinin değerleri nesneler olarak kutulama ve kutudan çıkarma işlemleri gerçekleştirerek edilir ([kutulama ve kutudan çıkarma](types.md#boxing-and-unboxing)).

## <a name="value-types"></a>Değer türleri

Bir değer türü veya bir yapı türü, hem de bir numaralandırma türü değil. C# adlı önceden tanımlanmış bir yapı türleri kümesi sağlar ***basit türler***. Basit türler ayrılmış sözcükler tanımlanır.

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

Bir değişkeni başvuru türünde bir değer türünün bir değişkeni değer içerebilir `null` yalnızca değer türü boş değer atanabilir bir tür ise.  Her değer atanamayan değer türü için aynı değerleri artı değer kümesini belirten karşılık gelen null yapılabilir değer türü var. `null`.

Bir değer türü bir değişkene atama atanan değerin bir kopyasını oluşturur. Bu başvuru ancak başvuru tarafından tanımlanan nesnesi değil kopyalar bir başvuru türündeki bir değişkene atamadan farklıdır.

### <a name="the-systemvaluetype-type"></a>System.ValueType türü

Tüm değer türleri örtülü olarak sınıftan `System.ValueType`, sırasıyla sınıfından devralan `object`. Bir değer türünden türetilen herhangi bir tür için mümkün değildir ve değer türleri böylece türetme ([sınıflar korumalı](classes.md#sealed-classes)).

Unutmayın `System.ValueType` kendisi değil bir *value_type*. Bunun yerine, olan bir *class_type* tüm gelen *value_type*s otomatik olarak türetilmiş.

### <a name="default-constructors"></a>Varsayılan Oluşturucu

Tüm değer türleri örtülü olarak adlı bir Ortak parametresiz örnek oluşturucusu bildirme ***varsayılan oluşturucu***. Varsayılan oluşturucu olarak bilinen sıfır başlatılmayan bir örnek döndürür ***varsayılan değer*** değer türü için:

*  Tüm *simple_type*s, varsayılan değer: değerin sıfır bit deseni tarafından üretilen:
    * İçin `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, ve `ulong`, varsayılan değer `0`.
    * İçin `char`, varsayılan değer `'\x0000'`.
    * İçin `float`, varsayılan değer `0.0f`.
    * İçin `double`, varsayılan değer `0.0d`.
    * İçin `decimal`, varsayılan değer `0.0m`.
    * İçin `bool`, varsayılan değer `false`.
*  İçin bir *enum_type* `E`, varsayılan değer `0`, türe dönüştürülebilir `E`.
*  İçin bir *struct_type*, varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarlayarak üretilen `null`.
*  İçin bir *nullable_type* örneği, varsayılan değer: `HasValue` özelliği false ve `Value` özelliği tanımsız. Varsayılan değer olarak da bilinir: ***null değer*** boş değer atanabilir tür.

Diğer örnek oluşturucusu gibi bir değer türünün varsayılan oluşturucu kullanılarak çağrılan `new` işleci. Verimliliği nedeniyle, bu gereksinim aslında bir oluşturucu çağrı oluşturma uygulama sağlamak için tasarlanmamıştır. Değişkenleri aşağıdaki örnekte `i` ve `j` her ikisi de sıfır olarak başlatılır.

```csharp
class A
{
    void F() {
        int i = 0;
        int j = new int();
    }
}
```

Her değer türü örtük olarak bir Ortak parametresiz örnek oluşturucusu olduğundan, bir açık parametresiz bir oluşturucu bildirimi içeren bir yapı türü için mümkün değildir. Bir yapı türü, ancak parametreli örnek oluşturucuları bildirme izni ([oluşturucular](structs.md#constructors)).

### <a name="struct-types"></a>Yapı türleri

Bir yapı türü sabitleri, alanları, yöntemleri, özellikleri, Dizinleyicileri, işleçleri, örnek oluşturucuları, statik oluşturucular ve iç içe geçmiş türler bildirebilirsiniz bir değer türüdür. Yapı türleri bildirimi açıklanan [yapı bildirimleri](structs.md#struct-declarations).

### <a name="simple-types"></a>Basit türler

C# adlı önceden tanımlanmış bir yapı türleri kümesi sağlar ***basit türler***. Basit türler ayrılmış sözcükler tanımlanır, ancak yalnızca önceden tanımlanmış bir yapı türleri için diğer adlar şu ayrılmış sözcüklerdir `System` aşağıdaki tabloda açıklandığı gibi ad alanı.


| __Ayrılmış bir sözcük__ | __Diğer adlı türü__ |
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

Bir yapı türü diğer adlar basit bir tür olduğundan, her bir basit tür üyeleri var. Örneğin, `int` bildirilmiş üyeleri `System.Int32` ve üyeleri devralınan `System.Object`, ve aşağıdaki deyimleri izin verilir:

```csharp
int i = int.MaxValue;           // System.Int32.MaxValue constant
string s = i.ToString();        // System.Int32.ToString() instance method
string t = 123.ToString();      // System.Int32.ToString() instance method
```

Basit türler, bazı ek işlemler izin vermek, diğer yapı türlerden farklılık gösterir:

*  En basit türler değerleri yazılarak oluşturulmak izin *değişmez değerleri* ([değişmez değerleri](lexical-structure.md#literals)). Örneğin, `123` bir sabit değer türü olan `int` ve `'a'` bir sabit değer türü olan `char`. C# değişmez değerler için hiçbir sağlama yapı türleri, genel yapar ve diğer yapı türlerinin varsayılan olmayan değerleri sonuçta her zaman bu yapı türü örnek oluşturucuları oluşturulur.
*  Bir ifadenin işlenenleri tüm basit tür sabit olduğunda, derleyici derleme zamanında ifadeyi değerlendirmek mümkündür. Böyle bir ifade olarak da bilinen bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)). Diğer yapı türleri tarafından tanımlanan işleçleri içeren ifadeler sabit ifadeler olması için dikkate alınmaz.
*  Aracılığıyla `const` bildirimleri mümkündür basit türler sabitler ([sabitleri](classes.md#constants)). Diğer yapı tür sabitleri mümkün değildir, ancak benzer bir etkisi tarafından sağlanan `static readonly` alanları.
*  Basit türler içeren dönüştürmeler diğer yapı türleri tarafından tanımlanan dönüşüm işleçleri değerlendirmesini katılıyor, ancak kullanıcı tanımlı dönüştürme işleci hiçbir zaman içinde başka bir kullanıcı tanımlı işleç değerlendirme katılabilir ([değerlendirmesi Kullanıcı tanımlı dönüşümler](conversions.md#evaluation-of-user-defined-conversions)).

### <a name="integral-types"></a>Tam sayı türleri

C# dokuz tamsayı türlerini destekler: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, ve `char`. Aşağıdaki boyutları ve değerler aralığı tam sayı türleri vardır:

*  `sbyte` İmzalı -128 ile 127 arasında değerlerle 8 bit tamsayı türünü temsil eder.
*  `byte` Türünü işaretsiz 8 bit tam sayı 0 ile 255 arasında değerlerle temsil eder.
*  `short` Türü temsil imzalı -32768 ile 32767 arasında değerlerle 16 bitlik tamsayı.
*  `ushort` Türünü işaretsiz 16 bit tam sayı 0 ile 65535 arasında değerlerle temsil eder.
*  `int` İmzalı 32-bit tamsayı değerleri -2147483648 ile 2147483647 arasında olan türü temsil eder.
*  `uint` Türünü işaretsiz 32-bit tamsayı değerleri 0 ve 4294967295 arasında ile temsil eder.
*  `long` İmzalı -9223372036854775808 ile 9223372036854775807 arasındaki değerleri ile 64 bit tamsayı türü temsil eder.
*  `ulong` Türünü işaretsiz 64-bit tamsayı değerleri 0 ile 18446744073709551615 arasında ile temsil eder.
*  `char` Türünü işaretsiz 16 bit tam sayı 0 ile 65535 arasında değerlerle temsil eder. İçin olası değerler kümesini `char` UNICODE karakter kümesine karşılık gelen türü. Ancak `char` aynı gösterime sahip `ushort`, diğer tüm işlemler bir yola izin izin verilir.

İkili işleçler ve integral türünden birli imzalı 32-bit duyarlığa, işaretsiz 32-bit duyarlığa, imzalı 64-bit duyarlığa veya işaretsiz 64-bit kesinliği ile her zaman çalışır:

*  Birli için `+` ve `~` işleçleri, işlenenin türüne dönüştürülür `T`burada `T` rehberin `int`, `uint`, `long`, ve `ulong` tam olarak temsil edebilen tüm Olası değerler işlenenin. Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T`.
*  Birli için `-` işleci, işlenenin türüne dönüştürülür `T`burada `T` rehberin `int` ve `long` tam olarak temsil edebilen işlenenin tüm olası değerler. Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T`. Birli `-` işleci türündeki işlenenlere uygulanamaz `ulong`.
*  İkili için `+`, `-`, `*`, `/`, `%`, `&`, `^`, `|`, `==`, `!=`, `>`, `<`, `>=`, ve `<=` işleçler işlenenlerin türüne dönüştürülür `T`burada `T` rehberin `int`, `uint`, `long`, ve `ulong` tam olarak temsil edebilen tüm olası Her iki işlenen de değerleri. Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T` (veya `bool` operatörler için). Bir işlenen türünde olmasını verilmeyen `long` ve diğer türünde olmasını `ulong` ikili işleçlere sahip.
*  İkili için `<<` ve `>>` işleçleri, sol işlenen türüne dönüştürülür `T`burada `T` rehberin `int`, `uint`, `long`, ve `ulong` tam olarak temsil edebilen tüm Olası değerler işlenenin. Duyarlık türü kullanarak işlemi gerçekleştirilir `T`, ve sonuç türü `T`.

`char` Türü bir tam sayı türü olarak sınıflandırılan, ancak diğer tamsayı türlerinin iki şekilde farklıdır:

*  Diğer türlerinden herhangi bir örtük dönüştürme vardır `char` türü. Özellikle, olsa bile `sbyte`, `byte`, ve `ushort` türlerine sahip tam olarak gösterilebilir kullanarak olan değerleri aralığı `char` türüne örtük dönüşümlere `sbyte`, `byte`, veya `ushort` için `char` mevcut değil.
*  Sabitleri `char` türü olarak yazılmalıdır *character_literal*s veya as *integer_literal*yazmanız için bir yayın birlikte s `char`. Örneğin, `(char)10` aynı `'\x000A'`.

`checked` Ve `unchecked` işleçler ve ifadeler Tamsayı türünde aritmetik işlemler ve dönüştürmeler için denetleme taşma denetlemek için kullanılır ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)). İçinde bir `checked` bağlamı, taşma bir derleme zamanı hatası oluşturur veya neden olan bir `System.OverflowException` oluşturulması için. İçinde bir `unchecked` bağlamını, taşan yok sayılır ve hedef türüne uygun değil herhangi bir yüksek sıra bitleri atılır.

### <a name="floating-point-types"></a>Kayan nokta türleri

C# destekleyen iki kayan nokta türleri: `float` ve `double`. `float` Ve `double` türleri, aşağıdaki değerleri kümesi sağlayan 32-bit tek duyarlıklı ve 64-bit çift duyarlıklı IEEE 754 biçimlerini kullanarak gösterilir:

*  Pozitif sıfır ve sıfır negatif. Çoğu durumda, pozitif sıfır ve negatif sıfır aynı şekilde davranır basit değer sıfır, ancak belirli işlemleri ikisi arasında ayrım ([bölme işleci](expressions.md#division-operator)).
*  Pozitif sonsuz ve negatif sonsuz. Sonsuz, sıfır olmayan bir sayı sıfıra bölme olarak bu işlemler tarafından oluşturulur. Örneğin, `1.0 / 0.0` Pozitif sonsuz verir ve `-1.0 / 0.0` konusundaki negatif sonsuz.
*  ***Sayı değil*** genellikle kısaltılmış NaN değeri. NaN'ler sıfır olarak sıfıra bölme gibi geçersiz kayan nokta işlemleri tarafından oluşturulur.
*  Sıfır olmayan değerler biçiminin sınırlı kümesini `s * m * 2^e`burada `s` 1 veya -1'dir ve `m` ve `e` belirli kayan nokta türüne göre belirlenir: için `float`, `0 < m < 2^24` ve `-149 <= e <= 104`ve `double`, `0 < m < 2^53` ve `1075 <= e <= 970`. Normalleştirilmemiş bir kayan noktalı sayılar, geçerli sıfır olmayan değerler olarak kabul edilir.

`float` Türü yaklaşık aralığındaki değerleri temsil eden `1.5 * 10^-45` için `3.4 * 10^38` 7 basamak kesinliği ile.

`double` Türü yaklaşık aralığındaki değerleri temsil eden `5.0 * 10^-324` için `1.7 × 10^308` 15-16 basamak kesinliği ile.

İkili işlecinin işlenenleri kayan nokta türü ise, daha sonra diğer işlenen bir tamsayı türü veya bir kayan nokta türü olmalıdır ve işlem gibi değerlendirilir:

*  Ardından biri bir tamsayı türü ise, işlenen diğer işlenen kayan nokta türüne dönüştürülür.
*  Daha sonra ya da işlenen türü ise `double`, diğer işlenen dönüştürülür `double`, en az kullanma işlemin gerçekleştirilmesinden `double` aralık ve duyarlık ve sonuç türü `double` (veya `bool` için İlişkisel işleçler:).
*  Aksi takdirde işlemi en az kullanılarak gerçekleştirilir `float` aralık ve duyarlık ve sonuç türü `float` (veya `bool` operatörler için).

Atama İşleçleri kayan nokta işleçlerin, hiçbir zaman özel durum oluşturur. Bunun yerine, olağanüstü durumlarda, kayan nokta işlemleri sıfır, sonsuzluk ve NaN, aşağıda açıklandığı gibi oluşturmak:

*  Kayan noktalı bir işlemin sonucunu hedef biçimi depolayamayacak kadar küçükse, işlemin sonucunu pozitif sıfır veya negatif sıfır.
*  Kayan noktalı bir işlemin sonucunu hedef biçimi çok büyük ise, işlemin sonucunu, Pozitif sonsuz veya negatif sonsuz olur.
*  Kayan noktalı bir işlemin geçersiz ise, NaN işleminin sonucu olur.
*  İşlemin sonucu bir veya iki işlenenin de kayan noktalı bir işlemin NaN ise, NaN haline gelir.

Kayan nokta işlemleri, işlemin sonuç türü daha yüksek duyarlık ile gerçekleştirilebilir. Örneğin, bazı donanım mimarileri büyük aralık ve duyarlık daha ile bir "uzun" veya "long double" kayan nokta türü desteği `double` yazın ve örtük olarak bu daha yüksek duyarlılık türünü kullanan tüm kayan nokta işlemleri gerçekleştirin. Yalnızca ücret ödemeden aşırı performans gibi donanım mimarileri daha az hassas kayan nokta işlemleri gerçekleştirmek için yapılabilir ve C# daha yüksek duyarlılık türünün olmasını gerektiren bir uygulama hem performans hem de duyarlık kaybeder yerine sağlar tüm kayan nokta işlemleri için kullanılır. Daha kesin sonuç sağlama dışında bu nadiren herhangi ölçülebilir etkilere sahiptir. Bununla birlikte, formun ifadelerde `x * y / z`nerede çarpma dışında bir sonuç üretir `double` aralığı, ancak sonraki bölme geçici sonucu geri getirir `double` aralık ifade olgusu değerlendirilen daha yüksek bir aralık içinde sonsuzluk yerine üretilecek sınırlı bir sonuç biçimi neden olabilir.

### <a name="the-decimal-type"></a>Decimal türü

`decimal` Türüdür finansal ve parasal hesaplamalar için uygun bir 128-bit veri türü. `decimal` Türü arasında değişen değerlerini temsil eden `1.0 * 10^-28` için yaklaşık `7.9 * 10^28` 28-29 belirgin basamağı ile.

Sınırlı tür değerleri kümesi `decimal` biçimindedir `(-1)^s * c * 10^-e`burada oturum `s` 0 veya 1, katsayısı `c` tarafından verilen `0 <= *c* < 2^96`ve ölçek `e` olduğu gibi `0 <= e <= 28`. `decimal` İmzalı sıfır, sonsuz veya NaN'ın türü desteklemiyor. A `decimal` yaklaşık on kuvveti Ölçeklendirildi 96 bitlik bir tamsayı olarak temsil edilir. İçin `decimal`s mutlak bir değer ile kısa `1.0m`, 28 ondalık basamak için tam, ancak başka değerdir. İçin `decimal`mutlak bir değer büyüktür veya eşittir s `1.0m`, 28 ya da 29 basamağa kadar kesin bir değerdir. Contrary için `float` ve `double` veri türlerini ondalık kesirli sayılar gibi 0,1 gösterilebileceği tam olarak `decimal` gösterimi. İçinde `float` ve `double` temsilleri böyle sayılardır genellikle sonsuz kesir bu gösterimler yuvarlama daha açık hale getirme hataları.

İkili işlecinin işlenenleri birini türde ise `decimal`, diğer işlenen bir tamsayı türü veya tür olmalıdır `decimal`. Bir tamsayı türü işlenen mevcutsa dönüştürülür `decimal` işlemi gerçekleştirilmeden önce.

Türünün değerleri üzerinde bir işlemin sonucunu `decimal` olduğundan, tam bir sonucu (her işleç tanımlanmış olarak koruyucu ölçeklendirme) hesaplama ve ardından gösterimi uyacak şekilde yuvarlama sonuçlanır. Sonuç yuvarlanır en yakın gösterilebilir değere ve bir sonuç yakın iki temsil edilebilir değerler, bir çift sayı olan en az önemli basamak konumu (Bu bilinen "banker yuvarlama gibi") değerine eşit olduğunda. Sıfır sonuç her zaman 0'ın bir oturum ve ölçeği 0 sahiptir.

Eşit veya daha düşük bir değere ondalık bir aritmetik işlemi neden oluyorsa `5 * 10^-29` mutlak değeri, işlemin sonucunu sıfır olur. Varsa bir `decimal` aritmetik işlemi için çok büyük bir sonuç üretir `decimal` biçiminde bir `System.OverflowException` oluşturulur.

`decimal` Türünde daha fazla duyarlık ancak daha küçük aralığından daha kayan nokta türleri. Bu nedenle, kayan nokta türlerinden dönüşümler `decimal` taşması özel durumları ve dönüşümlerse üretebilir `decimal` kayan nokta türleri için duyarlık kaybına neden. Bu nedenlerle, kayan nokta türleri arasında örtük dönüştürme işlemi yok mevcut ve `decimal`, ve açık atamaları kayan nokta karıştırmak mümkün değildir ve `decimal` işlenenler aynı ifadede.

### <a name="the-bool-type"></a>Bool türü

`bool` Türünü, boolean mantıksal miktarlar temsil eder. Olası değerler türü `bool` olan `true` ve `false`.

Standart dönüştürme arasındaki farklara `bool` ve diğer türleri. Özellikle, `bool` türüdür ve tam sayı türlerinden ayrı ve `bool` değeri bir tamsayı değeri yerine ve bunun tersi de kullanılamaz.

C ve C++ dillerinde, sıfır ayrılmaz veya kayan nokta değeri ya da bir null işaretçiyse boolean değerine dönüştürülebilir `false`, ve sıfır olmayan bir tamsayı veya kayan nokta değer veya null olmayan bir işaretçi boolean değerine dönüştürülebilir `true`. C# içinde bu tür dönüştürmeler açıkça bir tamsayı veya kayan nokta değeri sıfıra karşılaştırma ya da açıkça bir nesne başvurusu karşılaştırma gerçekleştirilir `null`.

### <a name="enumeration-types"></a>Numaralandırma türleri

Bir numaralandırma türü, adlandırılmış sabitler ile farklı bir türdür. Her sabit listesi türünde olmalıdır bir temel türü `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long` veya `ulong`. Numaralandırma türü değerleri kümesi, temel alınan tür değerleri kümesi ile aynıdır. Numaralandırma türünün değerlerini adlandırılmış sabitlerin değerleri için sınırlı değildir. Numaralandırma türleri numaralandırma bildirimleri tanımlanır ([Enum bildirimleri](enums.md#enum-declarations)).

### <a name="nullable-types"></a>Boş değer atanabilir tipler

Boş değer atanabilir bir tür tüm değerleri temsil edebilen kendi ***temel alınan türü*** artı ek bir null değer. Boş değer atanabilir bir tür yazılan `T?`burada `T` temel alınan türü. İçin Toplu özellik bu sözdizimidir `System.Nullable<T>`, ve iki birbirlerinin yerine kullanılabilir.

A ***NULL olmayan değer türü*** tersine dışında herhangi bir değer türü olan `System.Nullable<T>` ve kendi toplu `T?` (herhangi `T`), bir NULL olmayan değer türü (diğer bir deyişle, tüm kısıtlanmış bir tür parametresi artı tür parametresi ile bir `struct` kısıtlama). `System.Nullable<T>` Türü için değer türü kısıtlaması belirtir `T` ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)), herhangi bir değer atanamayan değer türünün temel türünü null yapılabilir bir tür olabileceği anlamına gelir. Boş değer atanabilir bir tür temel türü, null yapılabilir bir tür veya bir başvuru türü olamaz. Örneğin, `int??` ve `string?` geçersiz türleridir.

Boş değer atanabilir bir tür örneği `T?` iki genel salt okunur özelliklere sahiptir:

*  A `HasValue` türünün özelliği `bool`
*  A `Value` türünün özelliği `T`

Bir örneğin `HasValue` olan NULL olmayan söyledi true. Bilinen bir değerle null olmayan bir örnek içerir ve `Value` değeri döndürür.

Bir örneğin `HasValue` olan false söyledi null olması. Null bir örnek, tanımlanmamış bir değere sahip. Okumaya `Value` null bir örneğini neden olan bir `System.InvalidOperationException` oluşturulması için. Erişme işlemi `Value` özelliği boş değer atanabilir bir örneği olarak adlandırılır ***açma***.

Varsayılan Oluşturucu, her bir boş değer atanabilir tür yanı sıra `T?` türünde tek bir bağımsız değişken bir ortak yapıcıya sahip `T`. Verilen bir değer `x` türü `T`, formun bir oluşturucu çağrı

```csharp
new T?(x)
```
null olmayan bir örneğini oluşturur `T?` hangi `Value` özelliği `x`. Belirli bir değeri olarak adlandırılır için boş değer atanabilir bir tür null olmayan bir örneğini oluşturma işlemini ***sarmalama***.

Örtük dönüştürmeleri web'da `null` literal `T?` ([Null sabit değer dönüştürme](conversions.md#null-literal-conversions)) ve `T` için `T?` ([boş değer atanabilir örtük dönüştürmelerin](conversions.md#implicit-nullable-conversions)).

## <a name="reference-types"></a>Başvuru türleri

Bir başvuru sınıfı türü, bir arabirim türü, bir dizi türü veya bir temsilci türü türüdür.

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

Bir başvuru bir başvuru türü değeri yanlış bir ***örneği*** türü olarak ikinci bilinen bir ***nesne***. Özel değeri `null` tüm başvuru türleri ile uyumludur ve olmaması örneğini gösterir.

### <a name="class-types"></a>Sınıf türleri

Bir sınıf türü, veri üyeleri (sabitleri ve alanları), işlev üyeleri (yöntemler, özellikler, olaylar, dizin oluşturucular, işleçler, örnek oluşturucuları, yok ediciler ve statik oluşturucular) ve iç içe geçmiş türler içeren bir veri yapısını tanımlar. Sınıf türleri, devralma, yapabildiği türetilmiş sınıfları genişletmek ve temel sınıflar uzmanlaşmış bir mekanizma destekler. Sınıf türlerinin örneklerini kullanılarak oluşturulur *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).

Sınıf türleri açıklanmıştır [sınıfları](classes.md).

Belirli önceden tanımlanmış sınıf türleri, aşağıdaki tabloda açıklandığı gibi C# dili, özel bir anlamı yoktur.


| __Sınıf türü__     | __Açıklama__                                         |
|--------------------|---------------------------------------------------------|
| `System.Object`    | Diğer tüm türlerin ultimate temel sınıf. Bkz: [nesne türü](types.md#the-object-type). | 
| `System.String`    | C# dilinin string türü. Bkz: [dize türü](types.md#the-string-type).         |
| `System.ValueType` | Tüm değer türlerinin bir taban sınıf. Bkz: [System.ValueType türü](types.md#the-systemvaluetype-type).          |
| `System.Enum`      | Tüm sabit listesi türleri temel sınıf. Bkz: [numaralandırmalar](enums.md).              |
| `System.Array`     | Tüm dizi türleri temel sınıf. Bkz: [diziler](arrays.md).             |
| `System.Delegate`  | Tüm temsilci türlerinin bir taban sınıf. Bkz: [Temsilciler](delegates.md).          |
| `System.Exception` | Tüm özel durum türlerini temel sınıfı. Bkz: [özel durumları](exceptions.md).         |

### <a name="the-object-type"></a>Nesne türü

`object` Sınıf türü, diğer tüm türlerin ultimate temel sınıftır. C# ' de her tür doğrudan veya dolaylı olarak türetir `object` sınıf türü.

Anahtar sözcüğü `object` yalnızca önceden tanımlanmış sınıf için bir diğer addır `System.Object`.

### <a name="the-dynamic-type"></a>Dinamik tür

`dynamic` Yazın, gibi `object`, herhangi bir nesneye başvurabilir. İşleçler türündeki ifadeler için uygulandığı zaman `dynamic`, bunların çözümü program çalıştırılıncaya kadar ertelenir. Bu nedenle, işlecin yasal başvurulan nesneye uygulanamaz, derleme sırasında herhangi bir hata verilir. Çözümleme işlecinin çalışma zamanında başarısız olduğunda, bunun yerine bir özel durum oluşturulur.

Amacı, ayrıntılı olarak açıklanan dinamik bağlama izin vermektir [dinamik bağlama](expressions.md#dynamic-binding).

`dynamic` aynı olarak kabul edilir `object` aşağıdaki yönden hariç:

*  Türündeki ifade üzerinde işlemler `dynamic` dinamik olarak bağlı olabilir ([dinamik bağlama](expressions.md#dynamic-binding)).
*  Anlam çıkarma ([anlam çıkarma](expressions.md#type-inference)) tercih eder `dynamic` üzerinden `object` hem de adayları olması gerekir.

Bu denkliğin nedeniyle aşağıdakileri içerir:

*  Bir örtük kimlik dönüştürme arasında `object` ve `dynamic`, aynı değiştirilirken olan oluşturulan türler arasında `dynamic` ile `object`
*  Örtük ve açık dönüştürmeler gelen ve giden `object` ve ondan de geçerli `dynamic`.
*  Aynı değiştirilirken olan yöntem imzaları `dynamic` ile `object` aynı imzaya olarak kabul edilir
*  Türü `dynamic` döndürsün olan `object` çalışma zamanında.
*  Türündeki bir ifade `dynamic` şeklinde adlandırılan bir ***dinamik ifade***.

### <a name="the-string-type"></a>Dize türü

`string` Türüdür doğrudan devralan bir korumalı sınıf türü `object`. Örneklerini `string` sınıfı Unicode karakter dizeleri temsil eder.

Değerleri `string` tür dize değişmez değer olarak yazılabilir ([dize değişmez değerleri](lexical-structure.md#string-literals)).

Anahtar sözcüğü `string` yalnızca önceden tanımlanmış sınıf için bir diğer addır `System.String`.

### <a name="interface-types"></a>Arabirim türleri

Bir arabirim bir sözleşmeyi tanımlar. Bir sınıf ya da bir arabirimi uygulayan yapı bu sözleşmeye uymalıdır. Birden fazla temel Ara birimden arabirim devralabilir ve bir sınıf veya yapı birden fazla arabirim uygulayabilir.

Arabirim türlerinde açıklanmıştır [arabirimleri](interfaces.md).

### <a name="array-types"></a>Dizi türleri

Bir dizi hesaplanan dizinlerini erişilen sıfır veya daha fazla değişken içeren bir veri yapısıdır. Dizinin öğeleri olarak da bilinen bir dizi içindeki tüm aynı türdeki değişkenlerdir ve bu tür dizinin öğe türü olarak adlandırılır.

Dizi türleri açıklanmıştır [diziler](arrays.md).

### <a name="delegate-types"></a>Temsilci türleri

Bir temsilci, bir veya daha fazla yönteme başvuran bir veri yapısıdır. Örneği için yöntemleri, ayrıca karşılık gelen nesne örneklerini anlamına gelmektedir.

En yakın bir Temsilcide C veya C++ işlev işaretçisi eşdeğerdir, ancak bir işlev işaretçisine, yalnızca statik işlevler başvurabilir gelirken, bir temsilci hem statik başvuru ve yöntemleri örneği. İkinci durumda, yalnızca metodun giriş noktası bir başvuru, aynı zamanda Pro instanci objektu yöntemi çağırmak için bir başvuru temsilci depolar.

Temsilci türleri açıklanmıştır [Temsilciler](delegates.md).

## <a name="boxing-and-unboxing"></a>Kutulama ve kutudan çıkarma

Kutulama ve kutudan çıkarma kavramı, C# tür sistemi taşımaktadır. Arasında bir köprü sağlar *value_type*s ve *reference_type*s tarafından herhangi bir değerini izin veren bir *value_type* türü gelen ve giden dönüştürülecek `object`. Kutulama ve kutudan çıkarma gibi herhangi bir türde bir değer sonuçta bir nesne olarak davranılıp tür sistemi birleşik bir görünümünü sağlar.

### <a name="boxing-conversions"></a>Kutulama dönüştürmeler

Kutulama dönüştürmesi izin veren bir *value_type* için örtük olarak dönüştürülmesine bir *reference_type*. Aşağıdaki kutulama dönüştürmeler mevcuttur:

*  Herhangi *value_type* türüne `object`.
*  Herhangi *value_type* türüne `System.ValueType`.
*  Herhangi *non_nullable_value_type* herhangi *INTERFACE_TYPE* tarafından uygulanan *value_type*.
*  Herhangi *nullable_type* herhangi *INTERFACE_TYPE* temel alınan türü tarafından uygulanan *nullable_type*.
*  Herhangi *enum_type* türüne `System.Enum`.
*  Herhangi *nullable_type* bir temel ile *enum_type* türüne `System.Enum`.
*  Çalışma zamanında bir değer türünden bir başvuru türüne dönüştürme yukarı sona ererse örtük bir dönüştürme türü parametresi bir paketleme dönüştürmesi yürütülecek unutmayın ([tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters)).

Değerini kutulama bir *non_nullable_value_type* nesne örneğini tahsis etme ve kopyalama oluşur *non_nullable_value_type* değerini söz konusu örneğine dönüştürür.

Değerini kutulama bir *nullable_type* null başvuru ise üretir `null` değeri (`HasValue` olan `false`), veya çözülme ve temeldeki değeri aksi kutulama sonucu.

Gerçek değerini kutulama işlemini bir *non_nullable_value_type* en iyi genel varlığını imagining tarafından açıklanan ***kutulama sınıfı***, şu şekilde bildirilir yokmuş gibi davranır:

```csharp
sealed class Box<T>: System.ValueType
{
    T value;

    public Box(T t) {
        value = t;
    }
}
```

Bir değer olarak kutulama `v` türü `T` ifadeyi yürüten artık oluşur `new Box<T>(v)`ve elde edilen örnek türünde bir değer döndüren `object`. Bu nedenle, deyimleri
```csharp
int i = 123;
object box = i;
```
kavramsal olarak karşılık gelir
```csharp
int i = 123;
object box = new Box<int>(i);
```

Kutulama sınıfı gibi `Box<T>` yukarıdaki gerçekten yok ve aslında bir sınıf türünün kutulanmış değer dinamik türü desteklenmez. Bunun yerine, kutulanmış bir değer türü `T` dinamik türünde `T`ve dinamik tür denetimi kullanılarak `is` işleci yalnızca türü başvuru `T`. Örneğin,
```csharp
int i = 123;
object box = i;
if (box is int) {
    Console.Write("Box contains an int");
}
```
dize çıkarır "`Box contains an int`" konsolunda.

Kutulama dönüştürmesi Kutulu değer kopyalayarak anlamına gelir. Bu dönüştürme farklı bir *reference_type* türüne `object`hangi değeri aynı örneğine başvurmak devam eder ve yalnızca daha az türetilmiş bir türü olarak kabul edilir `object`. Örneğin, bildirimi verilen
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
aşağıdaki deyimleri
```csharp
Point p = new Point(10, 10);
object box = p;
p.x = 20;
Console.Write(((Point)box).x);
```
' % s'değeri 10 konsolunda çünkü çıkış atamasını içinde oluşan örtük kutulama işlemi `p` için `box` değeri neden olur `p` kopyalanacak. Vardı `Point` edilmiş bildirilen bir `class` bunun yerine, ' % s'değeri 20 çıkış olurdu, çünkü `p` ve `box` aynı örneğine başvuru.

### <a name="unboxing-conversions"></a>Kutudan çıkarma dönüştürme

Bir paketi açma dönüştürmesi izin veren bir *reference_type* açıkça dönüştürülmesini bir *value_type*. Aşağıdaki kutudan çıkarma dönüştürme vardır:

*  Türünden `object` herhangi *value_type*.
*  Türünden `System.ValueType` herhangi *value_type*.
*  Herhangi *INTERFACE_TYPE* herhangi *non_nullable_value_type* uygulayan *INTERFACE_TYPE*.
*  Herhangi *INTERFACE_TYPE* herhangi *nullable_type* temel türü uygulayan *INTERFACE_TYPE*.
*  Türünden `System.Enum` herhangi *enum_type*.
*  Türünden `System.Enum` herhangi *nullable_type* bir temel ile *enum_type*.
*  Çalışma zamanında bir değer türüne başvuru türünden dönüştürme yukarı sona ererse açık bir dönüştürme için bir tür parametresi bir paketi açma dönüştürmesi yürütülecek unutmayın ([açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions)).

Bir kutudan çıkarma işlemi için bir *non_nullable_value_type* ilk nesne örneği, kutulanmış bir değer olan denetimini oluşur verilen *non_nullable_value_type*ve ardından değeri / kopyalama örneği.

Kutudan çıkarma için bir *nullable_type* null değerini üretir *nullable_type* kaynak işlenen `null`, ya da temel alınan türü için nesne örneği kutudan çıkarma Sarmalanan sonucu *nullable_type* Aksi takdirde.

Bir nesnenin bir paketi açma dönüştürmesi önceki bölümde açıklanan sanal kutulama sınıfa başvuran `box` için bir *value_type* `T` ifadeyi yürüten oluşur `((Box<T>)box).value`. Bu nedenle, deyimleri
```csharp
object box = 123;
int i = (int)box;
```
kavramsal olarak karşılık gelir
```csharp
object box = new Box<int>(123);
int i = ((Box<int>)box).value;
```

Bir kutudan çıkarma dönüştürme için bir verilen *non_nullable_value_type* çalışma zamanında başarılı olması için kaynak işlecinin değer, söz konusu paketlenmiş değere bir başvuru olmalıdır *non_nullable_value_type*. Kaynak işlenen `null`, `System.NullReferenceException` oluşturulur. Kaynak işlenen uyumsuz bir nesneye bir başvuru ise bir `System.InvalidCastException` oluşturulur.

Bir kutudan çıkarma dönüştürme için bir verilen *nullable_type* çalışma zamanında başarılı olması için kaynak işlecinin değer olmalıdır `null` veya arka plandaki paketlenmiş değere bir başvuru *non_nullable_value_type* , *nullable_type*. Kaynak işlenen uyumsuz bir nesneye bir başvuru ise bir `System.InvalidCastException` oluşturulur.

## <a name="constructed-types"></a>Oluşturulan türler

Bir genel tür bildirimi kendisi tarafından yapıttan bir ***genel tür ilişkisiz*** "plan" uygulama yoluyla, birçok farklı türler oluşturmak için kullanılan ***tür bağımsız değişkenleri***. Tür bağımsız değişkenleri köşeli ayraç içinde yazılan (`<` ve `>`) genel tür adını takip. En az bir tür bağımsız değişkeni bir tür olarak adlandırılan bir ***oluşturulan türü***. Oluşturulan bir tür içinde bir tür adı görünebilir dil birçok yerde kullanılabilir. İlişkisiz bir genel tür içinde yalnızca kullanılabilir bir *typeof_expression* ([typeof işleci](expressions.md#the-typeof-operator)).

Oluşturulan türler de kullanılabilir ifadelerinde basit adları ([basit adları](expressions.md#simple-names)) veya bir üye erişirken ([üye erişimi](expressions.md#member-access)).

Olduğunda bir *namespace_or_type_name* Değerlendirilmiş, yalnızca genel türlerinin doğru sayısıyla türü parametreleri olarak kabul edilir. Bu nedenle, farklı sayıda tür parametreleri türlerine sahip olduğu sürece farklı türlerini tanımlamak üzere aynı tanımlayıcıyı kullanın mümkündür. Bu, genel ve genel olmayan sınıflar aynı programda kullanırken yararlıdır:

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

A *type_name* oluşturulan tür, tür parametreleri doğrudan belirtmeyen olsa da tanımlayabilir. Burada bir tür genel bir sınıf bildirimi içinde iç içe geçmiş ve yönelik ad araması içeren bildirim örnek türü örtük olarak kullanılan oluşabilir ([türlerin genel sınıflardaki](classes.md#nested-types-in-generic-classes)):

```csharp
class Outer<T>
{
    public class Inner {...}

    public Inner i;                // Type of i is Outer<T>.Inner
}
```

Güvenli olmayan kod içinde oluşturulmuş bir türü olarak kullanılamaz bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)).

### <a name="type-arguments"></a>Tür bağımsız değişkenleri

Tür bağımsız değişken listesindeki her bağımsız değişken teknolojidir bir *türü*.

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

Güvenli olmayan kod ([güvenli olmayan kod](unsafe-code.md)), *type_argument* bir işaretçi türü olabilir. Her tür bağımsız değişkeni tür parametresi buna karşılık gelen tüm kısıtlamalar karşılaması gerekir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).

### <a name="open-and-closed-types"></a>Açık ve kapalı türleri

Tüm türleri olarak sınıflandırılabilir ***açın türleri*** veya ***kapalı türleri***. Tür parametreleri içeren bir tür bir açık türdür. Daha açık belirtmek gerekirse:

*  Bir tür parametresi açık bir tür tanımlar.
*  Öğe türünün bir açık türdür ve yalnızca, bir dizi türü bir açık türdür.
*  Bir veya daha fazla tür bağımsız değişkenlerinden bir açık türdür ve yalnızca, oluşturulan tür bir açık türdür. Bir veya daha fazla tür bağımsız değişkenlerinden veya tür bağımsız değişkenlerini içeren kendi türünde bir açık türdür ve yalnızca, oluşturulan iç içe geçmiş tür bir açık türdür.

Kapalı bir tür açık bir tür değil bir türdür.

Çalışma zamanında, tüm genel tür bildirimi içinde kod yürütülür tür bağımsız değişkenleri için genel bildirimi uygulama tarafından oluşturulan kapalı bir oluşturulmuş tür bağlamında. Her tür parametresi, genel tür içinde belirli bir çalışma zamanı türüne bağlıdır. Çalışma zamanı, tüm ifadeleri ve ifadeler her zaman kapalı türleriyle işleneceğini ve açık türler yalnızca derleme zamanı sırasında işlem oluşur.

Her kapalı bir oluşturulmuş tür kendi herhangi diğer kapalı oluşturulan türler ile paylaşılmayan statik değişkenler kümesine sahiptir. Açık bir tür çalışma zamanında mevcut olmadığından, açık bir türü ile ilişkili hiçbir statik değişkenler vardır. İki kapalı oluşturulan türler, bunlar aynı ilişkisiz genel türünden oluşturulduğu ve bunların karşılık gelen tür bağımsız değişkenleri aynı türdeyse aynı türde olan.

### <a name="bound-and-unbound-types"></a>İlişkili ve ilişkisiz türleri

Terim ***ilişkisiz türü*** genel olmayan bir tür veya ilişkisiz bir genel tür belirtir. Terim ***bağlı türü*** genel olmayan bir tür veya oluşturulan tür anlamına gelir.

İlişkisiz bir türü bir tür bildirimi tarafından bildirilen bir varlığa başvurur. İlişkisiz bir genel tür kendisi bir tür değildir ve bir değişken, bağımsız değişken veya dönüş değeri türünü veya bir taban türü olarak kullanılamaz. Hangi ilişkisiz bir genel tür başvurulabilir yalnızca yapıdır `typeof` ifade ([typeof işleci](expressions.md#the-typeof-operator)).

### <a name="satisfying-constraints"></a>Tatmin kısıtlamaları

Oluşturulan tür ya da genel yöntemin başvurduğu her sağlanan tür bağımsız değişkeni genel tür veya yöntem üzerinde bildirilen tür parametresi kısıtlamaları karşı denetlenir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)). Her `where` yan tümcesi, tür bağımsız değişkeni `A` karşılık gelen adlandırılmış tür parametresi işaretli karşı her kısıtlama şu şekilde:

*  Bir sınıf türü, bir arabirim türü veya tür parametresi kısıtlaması ise `C` kısıtlamasındaki görünen herhangi bir tür parametre kısıtlaması sağlanan tür bağımsız değişkenleri ile yerine temsil eder. Kısıtlamasını karşılamak için bunu yazın çalışması olmalıdır `A` türüne dönüştürülebilir olduğundan `C` aşağıdakilerden biri olarak:
    * Bir kimlik dönüştürme ([kimlik dönüştürme](conversions.md#identity-conversion))
    * Örtük bir başvuru dönüştürmesi ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions))
    * Kutulama dönüştürmesi ([kutulama dönüştürmeler](conversions.md#boxing-conversions)), tür A NULL olmayan bir değer türü olması şartıyla.
    * Bir tür parametresi arasında'örtük bir başvuru, kutulama veya tür parametresi dönüştürme `A` için `C`.
*  Başvuru türü kısıtlaması kısıtlaması varsa (`class`), türü `A` aşağıdakilerden birini karşılaması gerekir:
    * `A` bir arabirim türü, sınıf türü, temsilci türü veya dizi türü olabilir. Unutmayın `System.ValueType` ve `System.Enum` bu kısıtlamasını başvuru türleridir.
    * `A` bir başvuru türü olarak bilinen bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
*  Değer türü kısıtlaması kısıtlaması varsa (`struct`), türü `A` aşağıdakilerden birini karşılaması gerekir:
    * `A` bir yapı türü veya sabit listesi türünde, ancak null yapılabilir bir tür değildir. Unutmayın `System.ValueType` ve `System.Enum` bu kısıtlamasını karşılamaz başvuru türleridir.
    * `A` değer türü kısıtlamasına sahip bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
*  Kısıtlama Oluşturucu kısıtlaması ise `new()`, türü `A` olmamalıdır `abstract` ve ortak bir parametresiz oluşturucuya sahip olmalıdır. Aşağıdakilerden biri doğruysa uyulmuş olur:
    * `A` tüm değer türleri ortak varsayılan oluşturucuya sahip bir değer türü olduğu ([varsayılan oluşturucular](types.md#default-constructors)).
    * `A` Oluşturucu kısıtlaması sahip bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
    * `A` değer türü kısıtlamasına sahip bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
    * `A` olmayan bir sınıf olan `abstract` ve açıkça bildirilirse içeren `public` parametresiz oluşturucu.
    * `A` değil `abstract` ve varsayılan bir oluşturucu vardır ([varsayılan oluşturucular](classes.md#default-constructors)).

Bir veya daha fazla tür parametresinin kısıtlamalarını tarafından belirtilen tür bağımsız değişkenlerini tatmin edici değil, bir derleme zamanı hatası oluşur.

Tür parametreleri devralınmaz olduğundan, hiçbir zaman kısıtlamaları olan ya da devralındı. Aşağıdaki örnekte `D` kısıtlaması, tür parametresinin belirtilmesi gerekiyor `T` böylece `T` taban sınıfı tarafından uygulanan kısıtlamasına `B<T>`. Buna karşılık, sınıfının `E` bir kısıtlama nedeniyle belirtmemeniz gerekir `List<T>` uygulayan `IEnumerable` herhangi `T`.

```csharp
class B<T> where T: IEnumerable {...}

class D<T>: B<T> where T: IEnumerable {...}

class E<T>: B<List<T>> {...}
```

## <a name="type-parameters"></a>Tür parametreleri

Bir tür parametresi, bir değer türü veya parametresi çalışma zamanında bağlı olduğu başvuru türüne atamak bir tanımlayıcıdır.

```antlr
type_parameter
    : identifier
    ;
```

Bir tür parametresi, birçok farklı gerçek tür bağımsız değişkenleri ile oluşturulabilir olduğundan, Tür parametrelerine biraz daha farklı işlemler ve diğer türleri kısıtlamalara sahip. Bu güncelleştirmeler şunlardır:

*  Bir tür parametresi, doğrudan temel sınıf bildirmek için kullanılamaz ([temel sınıfı](classes.md#base-class)) veya arabirimi ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)).
*  Tür parametresi için parametreleri kısıtlamalar varsa, bağımlı türündeki üye araması kuralları uygulanır. İçinde ayrıntılı [üye araması](expressions.md#member-lookup).
*  Bağımlı bir tür parametresi için kısıtlamalar varsa, kullanılabilir dönüştürmeler için tür parametresi uygulanır. İçinde ayrıntılı [tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters) ve [açık dinamik dönüştürmeler](conversions.md#explicit-dynamic-conversions).
*  Değişmez değer `null` tür parametresi bir başvuru türü olması biliniyorsa dışında bir tür parametresi tarafından belirtilen bir türe dönüştürülemez ([tür parametreleri içeren örtük dönüştürmelerin](conversions.md#implicit-conversions-involving-type-parameters)). Ancak, bir `default` ifade ([varsayılan değer ifadeleri](expressions.md#default-value-expressions)) bunun yerine kullanılabilir. Ayrıca, bir değer türü parametre tarafından sağlanan bir türü ile karşılaştırılabilir `null` kullanarak `==` ve `!=` ([başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)) değer türü kısıtlaması tür parametresi sahip olmadığı sürece.
*  A `new` ifade ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) tarafından tür parametresi kısıtlı yalnızca bir tür parametreyle kullanılabilir bir *constructor_constraint* veya kısıtlama (türüdeğeri[ Tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)).
*  Bir tür parametresi, her yerde bir öznitelik içinde kullanılamaz.
*  Bir tür parametresi bir üye erişimi kullanılamaz ([üye erişimi](expressions.md#member-access)) veya tür adı ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) statik bir üyeye veya iç içe geçmiş bir tür tanımlamak için.
*  Güvenli olmayan kod içinde bir tür parametresi olarak kullanılamaz bir *unmanaged_type* ([işaretçi türleri](unsafe-code.md#pointer-types)).

Bir türü olarak yalnızca bir derleme zamanı yapısı tür parametreleri var. Çalışma zamanında, her tür parametresi tür bağımsız değişkeni genel tür bildirimine sağlanarak belirtilen çalışma zamanı türüne bağlıdır. Bu nedenle, kapalı bir oluşturulmuş tür olabilir bir tür parametresi gerçekleştirilse çalışma zamanı ile bir değişken türü bildirilen ([açık ve kapalı türleri](types.md#open-and-closed-types)). Çalışma zamanı yürütme tüm ifadeleri ve tür parametreleri içeren ifadeler sağlanmadı gerçek tür, parametre türü bağımsız değişkeni olarak kullanır.

## <a name="expression-tree-types"></a>İfade ağacı türleri

***İfade ağaçları*** lambda ifadeleri olarak yürütülebilir kod yerine veri yapılarını temsil edilmesini izin verir. İfade ağaçları olan değerleri ***ifade ağacı türleri*** formun `System.Linq.Expressions.Expression<D>`burada `D` herhangi bir temsilci türü. Bu belirtim geri kalanı için biz kısaltmalar kullanarak bu türlerine başvuracaktır `Expression<D>`.

Bir temsilci türüne dönüştürme bir lambda ifadesinden varsa `D`, bir dönüştürme da ifade ağaç türüne olduğunu `Expression<D>`. Dönüştürme bir temsilci türü için bir lambda ifadesinin lambda ifadesi için yürütülebilir koda başvuran bir temsilci davranırken bir ifade ağacı türüne dönüştürme, lambda ifadesi bir ifade ağacı temsilini oluşturur.

İfade ağaçları lambda ifadeleri verimli bellek içi verilerin temsillerini olan ve lambda ifadesinin yapısı saydam ve açık hale getirir.

Bir temsilci türü'olduğu gibi `D`, `Expression<D>` parametre ve dönüş türleri için kabul edilir, aynı olduğu `D`.

Aşağıdaki örnek, bir lambda ifadesi yürütülebilir kod hem bir ifade ağacı olarak temsil eder. Bir dönüştürme olmadığından `Func<int,int>`, dönüştürme için de mevcut `Expression<Func<int,int>>`:

```csharp
Func<int,int> del = x => x + 1;                    // Code

Expression<Func<int,int>> exp = x => x + 1;        // Data
```

Bu atamaları, temsilci aşağıdaki `del` döndüren bir yöntemin başvurduğu `x + 1`ve ifade ağacı `exp` ifade tanımlayan bir veri yapısı başvuran `x => x + 1`.

Genel türün tam tanımı `Expression<D>` bir lambda ifadesi bir ifade ağacı türüne dönüştürülürken bir ifade ağacı oluşturmak için kesin yanı sıra, her ikisi de bu belirtim kapsamı dışında kurallardır.

İki şey açık hale getirmek önemlidir:

*  Tüm lambda ifadeleri ifade ağaçlarına dönüştürülemez. Örneğin, deyim gövdesi olan lambda ifadeleri ve atama ifadeleri içeren lambda ifadeleri ifade edilemez. Bu gibi durumlarda, bir dönüştürme hala var, ancak derleme zamanında başarısız olur. Bu özel durum ayrıntıları [anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions).
*   `Expression<D>` bir örnek yöntemi sunar `Compile` bir temsilci türü üretir `D`:

    ```csharp
    Func<int,int> del2 = exp.Compile();
    ```

    Bu temsilci çağırma yürütülecek bir ifade ağacı tarafından temsil edilen koda neden olur. Bu nedenle, yukarıdaki tanımları göz önünde bulundurulduğunda, del ve del2 eşdeğerdir ve aşağıdaki iki deyimi aynı etkiye sahip olur:

    ```csharp
    int i1 = del(1);
    
    int i2 = del2(1);
    ```

    Bu kod çalıştırıldıktan sonra `i1` ve `i2` hem de değeri alacaktır. `2`.

