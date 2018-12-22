# <a name="lexical-structure"></a>Sözcük yapısı

## <a name="programs"></a>Programlar

C# ***program*** bir veya daha fazla oluşur ***kaynak dosyaları***, resmi olarak bilinen ***derleme biriminden*** ([derleme biriminden](namespaces.md#compilation-units)). Sıralı bir Unicode karakter kaynak dosyasıdır. Kaynak dosyaları, dosyaları ile bire bir ilişkisi genellikle bir dosya sisteminde vardır. ancak bu yazışma gerekli değildir. Düzeyde taşınabilirlik için dosya sistemindeki dosyaları UTF-8 ile kodlanmış önerilir kodlama.

Kavramsal olarak açıklamak gerekirse, bir program üç adımda derlenir:

1. Dönüştürme, bir dosyayı bir özel karakter topluluğunun ve kodlama düzeni, Unicode karakter dizisine dönüştürür.
2. Sözcük analizi giriş karakter Unicode akışı belirteçleri bir akışa dönüştürür.
3. Söz dizimi analizi, belirteçlerin akışı yürütülebilir koda çevirir.

## <a name="grammars"></a>Dilbilgisi

Bu belirtimi iki dilbilgisi kullanarak C# programlama dilinin söz dizimini gösterir. ***Sözcük Dilbilgisi*** ([sözcük Dilbilgisi](lexical-structure.md#lexical-grammar)) formunu satır sonlandırıcılar, boşluk, yorumlar, belirteçleri ve ön işleme yönergeleri için Unicode karakterler nasıl birleştirileceğini tanımlar. ***Söz dizimsel Dilbilgisi*** ([söz Dizimsel Dilbilgisi](lexical-structure.md#syntactic-grammar)) sözcük dilbilgisi kaynaklanan belirteçleri form C# programları nasıl birleştirileceğini tanımlar.

### <a name="grammar-notation"></a>Dilbilgisi gösterimi

Sözcük temelli ve söz dizimsel dilbilgisi ANTLR dilbilgisi aracının gösterimini kullanarak Backus-Naur formu içinde sunulur.

### <a name="lexical-grammar"></a>Sözcük dilbilgisi

Sözcük dilbilgisi C# içinde sunulan [sözcük analizi](lexical-structure.md#lexical-analysis), [belirteçleri](lexical-structure.md#tokens), ve [yönergeleri ön işleme](lexical-structure.md#pre-processing-directives). Sözcük dilbilgisi terminal simgelerini Unicode karakter kümesinin karakterlerdir ve karakter form belirteçleri nasıl birleştirildiğini sözcük dilbilgisi belirtir ([belirteçleri](lexical-structure.md#tokens)), boşluk ([boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamaları](lexical-structure.md#comments)) ve ön işleme yönergeleri ([yönergeleri ön işleme](lexical-structure.md#pre-processing-directives)).

Her kaynak dosyası bir C# programında uymalıdır *giriş* üretimi sözcük dilbilgisi ([sözcük analizi](lexical-structure.md#lexical-analysis)).

### <a name="syntactic-grammar"></a>Söz dizimi dilbilgisi

C# sözdizimi dilbilgisi bölümleri ve bu bölümün aşağıdaki ek sunulur. Söz dizimi dilbilgisi terminal simgelerini sözcük dilbilgisi tarafından tanımlanan belirteçleridir ve belirteçleri form C# programları nasıl birleştirildiğini söz dizimsel dilbilgisi belirtir.

Her kaynak dosyada bir C# programı için uygun olmalıdır *compilation_unit* söz dizimsel dilbilgisi üretimini ([derleme biriminden](namespaces.md#compilation-units)).

## <a name="lexical-analysis"></a>Sözcük analizi

*Giriş* üretim bir C# kaynak dosyası sözcük yapısını tanımlar. Her kaynak dosyasının bir C# programında, bu sözcük dilbilgisi üretim için uygun olmalıdır.

```antlr
input
    : input_section?
    ;

input_section
    : input_section_part+
    ;

input_section_part
    : input_element* new_line
    | pp_directive
    ;

input_element
    : whitespace
    | comment
    | token
    ;
```

Beş temel öğeleri olun sözcük yapısı, bir C# kaynak dosyası: Satır sonlandırıcılar ([satır sonlandırıcılar](lexical-structure.md#line-terminators)), boşluk ([boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamaları](lexical-structure.md#comments)), belirteçleri ([belirteçleri](lexical-structure.md#tokens)), ve yönergeleri ön işleme ([yönergeleri ön işleme](lexical-structure.md#pre-processing-directives)). Bu temel öğeleri, yalnızca belirteçleri söz dizimi bir C# programı dilbilgisi içinde önemli ([söz Dizimsel Dilbilgisi](lexical-structure.md#syntactic-grammar)).

Dosya, söz dizimi analizi giriş dönüşen belirteçleri dizisini azaltma C# kaynak dosyası sözcük işlenmesini oluşur. Satır sonlandırıcılar, boşluk ve yorum belirteçleri, ayırmak için görebilir ve ön işleme yönergeleri atlanacak kaynak dosyasının bölümlerini neden olabilir, ancak Aksi halde bu sözcük temelli öğeler söz dizimi bir C# programı yapısını üzerinde hiçbir etkiye sahip.

İlişkilendirilmiş dize değişmez değerleri söz konusu olduğunda ([ilişkilendirilmiş dize değişmez değerleri](lexical-structure.md#interpolated-string-literals)) tek bir belirteç başlangıçta sözcük analizi tarafından oluşturulur ancak sürekli olarak sözcük temelli analize bağlı birkaç giriş öğelerine bozuk tüm ilişkilendirilmiş dize değişmez değerleri Çözüldü kadar. Sonuçta elde edilen belirteçleri sonra söz dizimi analizi giriş olarak sunar.

Kaynak dosyada bir karakter dizisi birkaç sözcük dilbilgisi üretim eşleşen sözcük işleme her zaman olası en uzun sözcük öğesi oluşturur. Örneğin, karakter dizisi `//` sözcük o öğenin tek bir uzun olduğu için tek satır açıklama başlangıcı olarak işlenir `/` belirteci.

### <a name="line-terminators"></a>Satır sonlandırıcılar

Satır sonlandırıcılar bir C# kaynak dosyası karakterlerini çizgiye bölün.

```antlr
new_line
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Carriage return character (U+000D) followed by line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;
```

Kaynak ile uyumluluk kod dosya sonu işaretleyicileri ekleyin düzenleme araçlarını ve bir kaynağı etkinleştirmek için bir dizi düzgün görüntülenmesi için dosya satırları sona için her kaynak dosyasının bir C# programı içinde sırayla aşağıdaki dönüştürmeleri uygulanır:

*  Kaynak dosyasının son karakterin bir denetim Z karakterini olup olmadığını (`U+001A`), bu karakter silinir.
*  Bir satır başı karakteri (`U+000D`), kaynak dosyası boş ise ve kaynak dosyasının son karakter bir satır başı değilse kaynak dosyasının sonuna eklenir (`U+000D`), satır besleme (`U+000A`), satır ayırıcı (`U+2028`), veya bir paragraf ayracı (`U+2029`).

### <a name="comments"></a>Açıklamalar

İki tür açıklamaları desteklenir: tek satırlı yorumlar ve sınırlandırılmış yorumlar. ***Tek satırlı yorumlar*** karakterleri ile Başlat `//` ve kaynak satır sonuna kadar genişletin. ***Ayrılmış açıklama*** karakterleri ile Başlat `/*` ve son karakterler `*/`. Ayrılmış açıklama birden fazla satırı kapsayabilir.

```antlr
comment
    : single_line_comment
    | delimited_comment
    ;

single_line_comment
    : '//' input_character*
    ;

input_character
    : '<Any Unicode character except a new_line_character>'
    ;

new_line_character
    : '<Carriage return character (U+000D)>'
    | '<Line feed character (U+000A)>'
    | '<Next line character (U+0085)>'
    | '<Line separator character (U+2028)>'
    | '<Paragraph separator character (U+2029)>'
    ;

delimited_comment
    : '/*' delimited_comment_section* asterisk* '/'
    ;

delimited_comment_section
    : '/'
    | asterisk* not_slash_or_asterisk
    ;

asterisk
    : '*'
    ;

not_slash_or_asterisk
    : '<Any Unicode character except / or *>'
    ;
```

Açıklamalar iç içe kullanmayın. Karakter sıraları `/*` ve `*/` içinde özel bir anlamı yoktur bir `//` yorum ve karakter sıraları `//` ve `/*` ayrılmış bir yorum içinde özel bir anlamı yoktur.

Yorumlar, karakter ve dize değişmez değerleri içinde işlenir.

Örnek
```csharp
/* Hello, world program
   This program writes "hello, world" to the console
*/
class Hello
{
    static void Main() {
        System.Console.WriteLine("hello, world");
    }
}
```
sınırlandırılmış bir açıklama içerir.

Örnek
```csharp
// Hello, world program
// This program writes "hello, world" to the console
//
class Hello // any name will do for this class
{
    static void Main() { // this method must be named "Main"
        System.Console.WriteLine("hello, world");
    }
}
```
birkaç tek satırlık açıklamaları gösterilmektedir.

### <a name="white-space"></a>Boşluk

Boşluk karakterleri (boşluk karakteri içeren) Zs Unicode sınıfıyla yanı sıra yatay sekme karakterinin, dikey sekme karakteri ve form besleme karakteri gibi tanımlanır.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Belirteçler

Belirteçlerin birkaç türü vardır: tanımlayıcılar, anahtar sözcükler, değişmez değerler, işleçler ve noktalama işaretçileri. Ayırıcılar belirteçleri görür ancak boşluk ve yorum belirteçleri, değildir.

```antlr
token
    : identifier
    | keyword
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | interpolated_string_literal
    | operator_or_punctuator
    ;
```

### <a name="unicode-character-escape-sequences"></a>Unicode karakter kaçış dizileri

Bir Unicode karakter kaçış dizisi, bir Unicode karakteri temsil eder. Unicode karakter kaçış dizileri tanımlayıcıları olarak işlenir ([tanımlayıcıları](lexical-structure.md#identifiers)), karakter değişmez değerleri ([karakter değişmez değerleri](lexical-structure.md#character-literals)) ve normal dize değişmez değerleri ([dize değişmez değerleri](lexical-structure.md#string-literals)). Bir Unicode karakter kaçış başka bir konuma (örneğin, bir işleç, noktalama işaretçisi veya anahtar oluşturmak için) işlenmiyor.

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Unicode çıkış dizisi onaltılık sayı aşağıdaki tarafından oluşturulmuş tek Unicode karakteri temsil eder "`\u`"veya"`\U`" karakter. C# bir 16 bitlik Unicode kod noktaları karakter ve dize değerlerini kodlamasını kullandığından bir Unicode karakter ile U + 10FFFF U + 10000 aralık içinde bir karakter sabiti içinde izin verilmez ve bir dize sabit değeri bir Unicode vekil çifti kullanarak gösterilir. Unicode karakterler 0x10FFFF yukarıda kod noktası ile desteklenmez.

Birden çok çevirileri gerçekleştirilmez. Örneğin, dize sabit değeri "`\u005Cu005C`"değerine eşdeğer olan"`\u005C`"yerine"`\`". Unicode değerini `\u005C` karakter "`\`".

Örnek
```csharp
class Class1
{
    static void Test(bool \u0066) {
        char c = '\u0066';
        if (\u0066)
            System.Console.WriteLine(c.ToString());
    }        
}
```
bazı kullanımlarını gösterir `\u0066`, harfi için kaçış sırası olduğu "`f`". Program eşdeğerdir
```csharp
class Class1
{
    static void Test(bool f) {
        char c = 'f';
        if (f)
            System.Console.WriteLine(c.ToString());
    }        
}
```

### <a name="identifiers"></a>Tanımlayıcılar

Alt çizgi (C programlama dilinde geleneksel olduğu gibi) ilk karakteri olarak Unicode kaçış dizileri izin dışında bu bölümde verilen tanımlayıcıları için kuralları tam olarak Unicode standart eki 31 tarafından önerilen karşılık gelir tanımlayıcılar, izin verilen ve "`@`" karakteri, tanımlayıcı olarak kullanılacak anahtar sözcükleri etkinleştirmek için önek olarak verilir.

```antlr
identifier
    : available_identifier
    | '@' identifier_or_keyword
    ;

available_identifier
    : '<An identifier_or_keyword that is not a keyword>'
    ;

identifier_or_keyword
    : identifier_start_character identifier_part_character*
    ;

identifier_start_character
    : letter_character
    | '_'
    ;

identifier_part_character
    : letter_character
    | decimal_digit_character
    | connecting_character
    | combining_character
    | formatting_character
    ;

letter_character
    : '<A Unicode character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    | '<A unicode_escape_sequence representing a character of classes Lu, Ll, Lt, Lm, Lo, or Nl>'
    ;

combining_character
    : '<A Unicode character of classes Mn or Mc>'
    | '<A unicode_escape_sequence representing a character of classes Mn or Mc>'
    ;

decimal_digit_character
    : '<A Unicode character of the class Nd>'
    | '<A unicode_escape_sequence representing a character of the class Nd>'
    ;

connecting_character
    : '<A Unicode character of the class Pc>'
    | '<A unicode_escape_sequence representing a character of the class Pc>'
    ;

formatting_character
    : '<A Unicode character of the class Cf>'
    | '<A unicode_escape_sequence representing a character of the class Cf>'
    ;
```

Yukarıda belirtilen Unicode karakter sınıfları hakkında daha fazla bilgi için Unicode standardı, sürüm 3.0, 4.5 bölümüne bakın.

Geçerli tanımlayıcıları örnekler "`identifier1`","`_identifier2`", ve "`@if`".

Uyumlu bir program bir tanımlayıcıda Unicode standart eki 15 tarafından tanımlandığı şekilde, Unicode normalleştirme Form C tarafından tanımlanan kurallı biçimde olmalıdır. Uygulama tanımlı davranış normalleştirme formu C'de değil bir tanımlayıcı ile karşılaşıldığında; Ancak, bir tanılama gerekli değildir.

Önek "`@`" diğer programlama dilleriyle arabirim zaman yararlı tanımlayıcı olarak anahtar sözcükler kullanılmasına olanak tanır. Karakter `@` tanımlayıcısı, diğer dillerde öneki olmadan normal bir tanımlayıcı olarak görülebilir şekilde tanımlayıcısı aslında bir parçası değil. Bir tanımlayıcıyla bir `@` önek çağrıldığında bir ***tam tanımlayıcı***. Kullanım `@` olmayan anahtar sözcükler tanımlayıcılar için önek izin, ancak stil sağlasa da kesinlikle önerilmez.

Örnek:
```csharp
class @class
{
    public static void @static(bool @bool) {
        if (@bool)
            System.Console.WriteLine("true");
        else
            System.Console.WriteLine("false");
    }    
}

class Class1
{
    static void M() {
        cl\u0061ss.st\u0061tic(true);
    }
}
```
adlı bir sınıf tanımlar "`class`"adlı statik bir yöntem ile"`static`"adlı bir parametre alan"`bool`". Unicode çıkışları olduğundan, anahtar, belirteç izin verilmez Not "`cl\u0061ss`"bir tanımlayıcıdır ve aynı tanımlayıcı olarak"`@class`".

Bunlar aşağıdaki dönüştürmeleri, sırayla uygulanır sonra aynıysa iki tanımlayıcı aynı kabul edilir:

*  Önek "`@`", kullandıysanız kaldırılır.
*  Her *unicode_escape_sequence* , karşılık gelen bir Unicode karakter dönüştürülür.
*  Tüm *formatting_character*s kaldırılır.

İki ardışık içeren tanımlayıcılar alt çizgi karakteri (`U+005F`) uygulama tarafından kullanım için ayrılmıştır. Örneğin, bir uygulama iki alt çizgi ile başlayan genişletilmiş anahtar sözcükleri sağlayabilir.

### <a name="keywords"></a>Anahtar Sözcükler

A ***anahtar sözcüğü*** ayrılmıştır ve tarafından zaman başında dışında bir tanımlayıcı olarak kullanılamaz, tanımlayıcı benzeri karakter dizisidir `@` karakter.

```antlr
keyword
    : 'abstract' | 'as'       | 'base'       | 'bool'      | 'break'
    | 'byte'     | 'case'     | 'catch'      | 'char'      | 'checked'
    | 'class'    | 'const'    | 'continue'   | 'decimal'   | 'default'
    | 'delegate' | 'do'       | 'double'     | 'else'      | 'enum'
    | 'event'    | 'explicit' | 'extern'     | 'false'     | 'finally'
    | 'fixed'    | 'float'    | 'for'        | 'foreach'   | 'goto'
    | 'if'       | 'implicit' | 'in'         | 'int'       | 'interface'
    | 'internal' | 'is'       | 'lock'       | 'long'      | 'namespace'
    | 'new'      | 'null'     | 'object'     | 'operator'  | 'out'
    | 'override' | 'params'   | 'private'    | 'protected' | 'public'
    | 'readonly' | 'ref'      | 'return'     | 'sbyte'     | 'sealed'
    | 'short'    | 'sizeof'   | 'stackalloc' | 'static'    | 'string'
    | 'struct'   | 'switch'   | 'this'       | 'throw'     | 'true'
    | 'try'      | 'typeof'   | 'uint'       | 'ulong'     | 'unchecked'
    | 'unsafe'   | 'ushort'   | 'using'      | 'virtual'   | 'void'
    | 'volatile' | 'while'
    ;
```

Belirli tanımlayıcıları, bazı yerlerde dilbilgisi içinde özel bir anlamı yoktur ancak anahtar sözcükler değildir. Tür tanımlayıcıları, bazen "bağlamsal anahtar sözcükler" da denir. Örneğin, bir özellik bildirimi içinde "`get`"ve"`set`" tanımlayıcıları specified ([erişimcileri](classes.md#accessors)). Dışında bir tanımlayıcı `get` veya `set` bu kullanım bu sözcükler tanımlayıcı olarak kullanımını ile çakışmaması bu konumda hiçbir zaman izin verilir. Diğer durumlarda, örneğin bir tanımlayıcıyla gibi "`var`" türü örtük olarak belirlenmiş yerel değişken bildirimlerinde ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)), bağlamsal anahtar sözcüğü ile bildirilen adlar çakışabilir. Bu gibi durumlarda, bildirilen ad tanımlayıcısı olarak bağlamsal anahtar sözcük kullanmak önceliklidir.

### <a name="literals"></a>Sabit değerler

A ***değişmez değer*** bir kaynak kodu bir değer gösterimidir.

```antlr
literal
    : boolean_literal
    | integer_literal
    | real_literal
    | character_literal
    | string_literal
    | null_literal
    ;
```

#### <a name="boolean-literals"></a>Boole sabit değerleri

İki boolean değişmez değer vardır: `true` ve `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Türü bir *boolean_literal* olduğu `bool`.

#### <a name="integer-literals"></a>Tamsayı sabit değerleri

Tamsayı sabit değerlerinde türlerindeki değerleri yazmak için kullanılan `int`, `uint`, `long`, ve `ulong`. Tamsayı sabit değerlerinde sahip iki olası form: ondalık ve onaltılık.

```antlr
integer_literal
    : decimal_integer_literal
    | hexadecimal_integer_literal
    ;

decimal_integer_literal
    : decimal_digit+ integer_type_suffix?
    ;

decimal_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    ;

integer_type_suffix
    : 'U' | 'u' | 'L' | 'l' | 'UL' | 'Ul' | 'uL' | 'ul' | 'LU' | 'Lu' | 'lU' | 'lu'
    ;

hexadecimal_integer_literal
    : '0x' hex_digit+ integer_type_suffix?
    | '0X' hex_digit+ integer_type_suffix?
    ;

hex_digit
    : '0' | '1' | '2' | '3' | '4' | '5' | '6' | '7' | '8' | '9'
    | 'A' | 'B' | 'C' | 'D' | 'E' | 'F' | 'a' | 'b' | 'c' | 'd' | 'e' | 'f';
```

Değişmez değer bir tamsayı türü şu şekilde belirlenir:

*  Değişmez değer sonek varsa, bu ilk hangi değeri gösterilebileceği bu tür vardır: `int`, `uint`, `long`, `ulong`.
*  Tarafından değişmez değer olup olmadığını `U` veya `u`, ilk hangi değeri gösterilebileceği bu tür vardır: `uint`, `ulong`.
*  Tarafından değişmez değer olup olmadığını `L` veya `l`, ilk hangi değeri gösterilebileceği bu tür vardır: `long`, `ulong`.
*  Tarafından değişmez değer olup olmadığını `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, veya `lu`, türü `ulong`.

Bir tamsayı sabit değeri tarafından temsil edilen değeri aralığının dışında ise `ulong` yazın, bir derleme zamanı hatası oluşur.

Birkaç stil önerilir, "`L`"kullanılabilir yerine"`l`" sabit değerleri türü yazarken `long`, harf karıştırılabilir kolay olduğundan "`l`"ile "basamak`1`".

En küçük olası izin vermek için `int` ve `long` yazılabilir ondalık tamsayı değişmez değer olarak şu iki kuralı mevcut değerleri:

* Olduğunda bir *decimal_integer_literal* 2147483648 değerle (2 ^ 31) ve hiçbir *integer_type_suffix* bir tekli işleç belirteci eksi hemen ardından belirteci olarak görünür ([birli eksi İşleç](expressions.md#unary-minus-operator)), sonuç türü bir sabittir `int` değeri -2147483648 ile (-2 ^ 31). Diğer durumlarda bu tür bir *decimal_integer_literal* türünde `uint`.
* Olduğunda bir *decimal_integer_literal* 9223372036854775808 değerle (2 ^ 63) ve hiçbir *integer_type_suffix* veya *integer_type_suffix* `L` veya `l` bir tekli işleç belirteci eksi hemen ardından belirteci olarak görünür ([birli eksi işleci](expressions.md#unary-minus-operator)), sonuç türü bir sabittir `long` -9223372036854775808 değerle (-2 ^ 63). Diğer durumlarda bu tür bir *decimal_integer_literal* türünde `ulong`.

#### <a name="real-literals"></a>Gerçek değişmez değerleri

Türlerindeki değerleri yazmak için kullanılan gerçek değişmez değerleri `float`, `double`, ve `decimal`.

```antlr
real_literal
    : decimal_digit+ '.' decimal_digit+ exponent_part? real_type_suffix?
    | '.' decimal_digit+ exponent_part? real_type_suffix?
    | decimal_digit+ exponent_part real_type_suffix?
    | decimal_digit+ real_type_suffix
    ;

exponent_part
    : 'e' sign? decimal_digit+
    | 'E' sign? decimal_digit+
    ;

sign
    : '+'
    | '-'
    ;

real_type_suffix
    : 'F' | 'f' | 'D' | 'd' | 'M' | 'm'
    ;
```

Hayır ise *real_type_suffix* belirtilirse, gerçek değişmez değer türü `double`. Aksi takdirde, gerçek tür soneki gerçek değişmez değer türü şu şekilde belirler:

*  Tarafından ve sonra gerçek değişmez değerin `F` veya `f` türünde `float`. Örneğin, değişmez değerler `1f`, `1.5f`, `1e10f`, ve `123.456F` türü tümü `float`.
*  Tarafından ve sonra gerçek değişmez değerin `D` veya `d` türünde `double`. Örneğin, değişmez değerler `1d`, `1.5d`, `1e10d`, ve `123.456D` türü tümü `double`.
*  Tarafından ve sonra gerçek değişmez değerin `M` veya `m` türünde `decimal`. Örneğin, değişmez değerler `1m`, `1.5m`, `1e10m`, ve `123.456M` türü tümü `decimal`. Bu sabit dönüştürülür bir `decimal` tam değer almak ve gerekirse, en yakın kullanarak gösterilebilir değere yuvarlama değer banker yuvarlama ([decimal türü](types.md#the-decimal-type)). Değişmez değer görünen herhangi bir ölçekte, değerin yuvarlanmış ya da değer sıfırdır (hangi ikinci durumda oturum ve ölçek 0 olacaktır) korunur. Bu nedenle, değişmez değer `2.900m` ondalık oturum oluşturmak için ayrıştırılacak `0`, katsayısı `2900`ve ölçek `3`.

Belirtilen hazır değerin belirtilen türdeki temsil edilemiyorsa, bir derleme zamanı hatası oluşur.

Gerçek bir sabit değer türündeki değeri `float` veya `double` IEEE kullanılarak belirlenir "yuvarlak yakın için" modu.

Bir gerçek değişmez değerin ondalık basamak her zaman ondalık ayırıcıdan sonra gerekli olduğunu unutmayın. Örneğin, `1.3F` gerçek sabit değer ancak olan `1.F` değil.

#### <a name="character-literals"></a>Karakter değişmez değerleri

Karakterin değişmez değeri tek bir karakteri temsil eder ve genellikle bir tırnak karakteri olarak oluşan `'a'`.

Not: ANTLR dilbilgisi gösterimi aşağıdaki karmaşık hale getirir! Siz yazarken ANTLR içinde `\'` için tek bir teklif anlaşıldığı `'`. Ve yazarken `\\` için tek bir ters eğik çizgi anlaşıldığı `\`. Bu nedenle tek tırnak işareti sonra bir karakter, sonra da tek bir tırnak işareti ile başlayan bir karakter sabiti için ilk kural anlamına gelir. Ve on olası basit çıkış sıraları `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

```antlr
character_literal
    : '\'' character '\''
    ;

character
    : single_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_character
    : '<Any character except \' (U+0027), \\ (U+005C), and new_line_character>'
    ;

simple_escape_sequence
    : '\\\'' | '\\"' | '\\\\' | '\\0' | '\\a' | '\\b' | '\\f' | '\\n' | '\\r' | '\\t' | '\\v'
    ;

hexadecimal_escape_sequence
    : '\\x' hex_digit hex_digit? hex_digit? hex_digit?;
```

Ters eğik çizgi karakterini izleyen bir karakter (`\`) içinde bir *karakter* şu karakterlerden biri olmalıdır: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Aksi takdirde, bir derleme zamanı hatası oluşur.

Onaltılık kaçış sırası onaltılık sayı aşağıdaki tarafından oluşturulmuş bir değerle tek bir Unicode karakterini temsil eder "`\x`".

Bir karakter sabiti temsil edilen değeri büyük olup olmadığını `U+FFFF`, bir derleme zamanı hatası oluşur.

Bir Unicode karakter kaçış dizisi ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)) aralığında bir karakter sabiti olmalıdır `U+0000` için `U+FFFF`.

Basit çıkış dizisi, bir Unicode karakter kodlamasını, aşağıdaki tabloda açıklandığı gibi temsil eder.


| __Kaçış sırası__ | __Karakter adı__ | __Unicode kodlama__ |
|---------------------|--------------------|----------------------|
| `\'`                | tek tırnak       | `0x0027`             | 
| `\"`                | çift tırnak işareti       | `0x0022`             | 
| `\\`                | Ters eğik çizgi          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Uyarı              | `0x0007`             | 
| `\b`                | Geri Al tuşu          | `0x0008`             | 
| `\f`                | form besleme          | `0x000C`             | 
| `\n`                | Yeni satır           | `0x000A`             | 
| `\r`                | satır başı    | `0x000D`             | 
| `\t`                | Yatay sekme     | `0x0009`             | 
| `\v`                | dikey sekme       | `0x000B`             | 

Türü bir *character_literal* olduğu `char`.

#### <a name="string-literals"></a>Dize sabit değerleri

C# dize değişmez değerlerinin iki biçimini destekler: ***normal dize değişmez değerleri*** ve ***verbatim dize değişmez değerleri***.

Normal bir dize sabit değeri olarak çift tırnak içine alınmış sıfır veya daha fazla karakterden oluşur `"hello"`ve her iki basit çıkış sıraları içerebilir (gibi `\t` karakter için sekmesinde) ve Unicode kaçış dizileri ve onaltılık.

Verbatim dize sabit değeri oluşan bir `@` karakter çift tırnak karakteri, sıfır veya daha fazla karakter ve bir kapanış çift tırnak karakteri tarafından izlenen. Basit bir örnektir `@"hello"`. Bir harfi harfine dizede sabit değer, aynen, sınırlayıcılar arasındaki karakterleri yorumlanan yalnızca özel durum olan bir *quote_escape_sequence*. Özellikle, basit kaçış dizileri ve onaltılık ve Unicode kaçış dizileri verbatim dize değişmez değerlerine işlenmez. Verbatim dize değişmez değeri birden fazla satırı kapsayabilir.

```antlr
string_literal
    : regular_string_literal
    | verbatim_string_literal
    ;

regular_string_literal
    : '"' regular_string_literal_character* '"'
    ;

regular_string_literal_character
    : single_regular_string_literal_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    ;

single_regular_string_literal_character
    : '<Any character except " (U+0022), \\ (U+005C), and new_line_character>'
    ;

verbatim_string_literal
    : '@"' verbatim_string_literal_character* '"'
    ;

verbatim_string_literal_character
    : single_verbatim_string_literal_character
    | quote_escape_sequence
    ;

single_verbatim_string_literal_character
    : '<any character except ">'
    ;

quote_escape_sequence
    : '""'
    ;
```

Ters eğik çizgi karakterini izleyen bir karakter (`\`) içinde bir *regular_string_literal_character* şu karakterlerden biri olmalıdır: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Aksi takdirde, bir derleme zamanı hatası oluşur.

Örnek
```csharp
string a = "hello, world";                   // hello, world
string b = @"hello, world";                  // hello, world

string c = "hello \t world";                 // hello      world
string d = @"hello \t world";                // hello \t world

string e = "Joe said \"Hello\" to me";       // Joe said "Hello" to me
string f = @"Joe said ""Hello"" to me";      // Joe said "Hello" to me

string g = "\\\\server\\share\\file.txt";    // \\server\share\file.txt
string h = @"\\server\share\file.txt";       // \\server\share\file.txt

string i = "one\r\ntwo\r\nthree";
string j = @"one
two
three";
```
dize değişmez değerleri çeşitli gösterir. Son dize sabit değeri, `j`, birden fazla satır kapsayan bir verbatim dizesi sabit değeri. Boşluk gibi yeni satır karakterleri dahil olmak üzere tırnak işaretleri arasındaki karakterleri verbatim korunur.

Değişken sayıda onaltılık basamak, dize sabit değeri bir onaltılık kaçış sırası olabileceğinden `"\x123"` 123 onaltılık değer ile tek bir karakter içeriyor. Onaltılık değeri 3 ardından 12 karakter içeren bir dize oluşturmak için bir yazabilirsiniz `"\x00123"` veya `"\x12" + "3"` yerine.

Türü bir *string_literal* olduğu `string`.

Her dize sabit değeri yeni bir dize örneğinde sonuçlanmaz. İki veya daha fazla dize değişmez değerleri, olduğunda göre dize eşitlik işlecini eşdeğer ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) görünmesini aynı programda, bu dize değişmez değerleri, aynı dize örneğine bakın. Örneğin, tarafından üretilen çıkış
```csharp
class Test
{
    static void Main() {
        object a = "hello";
        object b = "hello";
        System.Console.WriteLine(a == b);
    }
}
```
olan `True` iki değişmez değer için aynı dize örneğinde başvurduğundan.

#### <a name="interpolated-string-literals"></a>İlişkilendirilmiş dize değişmez değerleri

İlişkilendirilmiş dize değişmez değerleri, dize sabit değerleri için benzerdir, ancak tarafından ayrılmış açıkları içeren `{` ve `}`, burada görüntülerle deyimleri ortaya çıkabilir. Çalışma zamanında, metinsel formlarına delik gerçekleştiği yerde dizesine yerine sahip kullanılabilir olmasını sağlamak amacıyla ifadeler değerlendirilir. Sözdizimi ve semantiği dize ilişkilendirme bölümünde açıklanan ([Ara değerli dizeler](expressions.md#interpolated-strings)).

Dize değişmez değerleri gibi normal veya verbatim ilişkilendirilmiş dize değişmez değerleri olabilir. Normal ilişkilendirilmiş dize değişmez değerleri ayrılmıştır `$"` ve `"`, ve verbatim ilişkilendirilmiş dize değişmez değerleri ayrılmıştır `$@"` ve `"`.

Diğer sabitler gibi sözcük temelli analiz ilişkilendirilmiş bir dize sabitinin aşağıdaki dilbilgisinde göre tek bir belirteç başlangıçta sonuçlanır. Ancak, söz dizimi analizi önce birkaç belirteç boşluklarını kapsayan dizesi bölümleri için ilişkilendirilmiş bir dize sabitinin tek belirteç bölünür ve açıklarına oluşan giriş öğelerini sözcüksel olarak yeniden analysed. Bu sırayla işlenmesi, ancak varsa sözcüksel olarak düzeltin, sonunda bir dizi belirteçleri işlemek söz dizimi analizi için neden, daha ilişkilendirilmiş dize değişmez değerlerine oluşturabilir.

```antlr
interpolated_string_literal
    : '$' interpolated_regular_string_literal
    | '$' interpolated_verbatim_string_literal
    ;

interpolated_regular_string_literal
    : interpolated_regular_string_whole
    | interpolated_regular_string_start  interpolated_regular_string_literal_body interpolated_regular_string_end
    ;

interpolated_regular_string_literal_body
    : regular_balanced_text
    | interpolated_regular_string_literal_body interpolated_regular_string_mid regular_balanced_text
    ;

interpolated_regular_string_whole
    : '"' interpolated_regular_string_character* '"'
    ;

interpolated_regular_string_start
    : '"' interpolated_regular_string_character* '{'
    ;

interpolated_regular_string_mid
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '{'
    ;

interpolated_regular_string_end
    : interpolation_format? '}' interpolated_regular_string_characters_after_brace? '"'
    ;

interpolated_regular_string_characters_after_brace
    : interpolated_regular_string_character_no_brace
    | interpolated_regular_string_characters_after_brace interpolated_regular_string_character
    ;

interpolated_regular_string_character
    : single_interpolated_regular_string_character
    | simple_escape_sequence
    | hexadecimal_escape_sequence
    | unicode_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;

interpolated_regular_string_character_no_brace
    : '<Any interpolated_regular_string_character except close_brace_escape_sequence and any hexadecimal_escape_sequence or unicode_escape_sequence designating } (U+007D)>'
    ;

single_interpolated_regular_string_character
    : '<Any character except \" (U+0022), \\ (U+005C), { (U+007B), } (U+007D), and new_line_character>'
    ;

open_brace_escape_sequence
    : '{{'
    ;

    close_brace_escape_sequence
    : '}}'
    ;
    
regular_balanced_text
    : regular_balanced_text_part+
    ;

regular_balanced_text_part
    : single_regular_balanced_text_character
    | delimited_comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' regular_balanced_text ')'
    | '[' regular_balanced_text ']'
    | '{' regular_balanced_text '}'
    ;
    
single_regular_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B), } (U+007D) and new_line_character>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
    
interpolation_format
    : interpolation_format_character+
    ;
    
interpolation_format_character
    : '<Any character except \" (U+0022), : (U+003A), { (U+007B) and } (U+007D)>'
    ;
    
interpolated_verbatim_string_literal
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_literal_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_literal_body
    : verbatim_balanced_text
    | interpolated_verbatim_string_literal_body interpolated_verbatim_string_mid verbatim_balanced_text
    ;
    
interpolated_verbatim_string_whole
    : '@"' interpolated_verbatim_string_character* '"'
    ;
    
interpolated_verbatim_string_start
    : '@"' interpolated_verbatim_string_character* '{'
    ;
    
interpolated_verbatim_string_mid
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '{'
    ;
    
interpolated_verbatim_string_end
    : interpolation_format? '}' interpolated_verbatim_string_characters_after_brace? '"'
    ;
    
interpolated_verbatim_string_characters_after_brace
    : interpolated_verbatim_string_character_no_brace
    | interpolated_verbatim_string_characters_after_brace interpolated_verbatim_string_character
    ;
    
interpolated_verbatim_string_character
    : single_interpolated_verbatim_string_character
    | quote_escape_sequence
    | open_brace_escape_sequence
    | close_brace_escape_sequence
    ;
    
interpolated_verbatim_string_character_no_brace
    : '<Any interpolated_verbatim_string_character except close_brace_escape_sequence>'
    ;
    
single_interpolated_verbatim_string_character
    : '<Any character except \" (U+0022), { (U+007B) and } (U+007D)>'
    ;
    
verbatim_balanced_text
    : verbatim_balanced_text_part+
    ;

verbatim_balanced_text_part
    : single_verbatim_balanced_text_character
    | comment
    | '@' identifier_or_keyword
    | string_literal
    | interpolated_string_literal
    | '(' verbatim_balanced_text ')'
    | '[' verbatim_balanced_text ']'
    | '{' verbatim_balanced_text '}'
    ;
    
single_verbatim_balanced_text_character
    : '<Any character except / (U+002F), @ (U+0040), \" (U+0022), $ (U+0024), ( (U+0028), ) (U+0029), [ (U+005B), ] (U+005D), { (U+007B) and } (U+007D)>'
    | '</ (U+002F), if not directly followed by / (U+002F) or * (U+002A)>'
    ;
```

Bir *interpolated_string_literal* belirteci birden çok belirteçleri ve diğer öğeleri aşağıdaki gibi oluşumunu sırasına göre giriş olarak düşürülen *interpolated_string_literal*:

* Aşağıdaki örneği ayrı ayrı belirteçlerin düşürülen: başında `$` işareti *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* ve *interpolated_verbatim_string_end*.
* Oluşumlarını *regular_balanced_text* ve *verbatim_balanced_text* olarak bunlar arasında işlenmiş bir *input_section* ([sözcük analizi ](lexical-structure.md#lexical-analysis)) ve sonuçta elde edilen giriş öğeleri dizisi olarak düşürülen. Bu sırayla ilişkilendirilmiş dize sabit değeri belirteçleri düşürülen için de içerebilir.

Söz dizimi analizi yeniden birleştirmek belirteçlere bir *interpolated_string_expression* ([Ara değerli dizeler](expressions.md#interpolated-strings)).

TODO örnekleri


#### <a name="the-null-literal"></a>Null sabiti

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* örtük bir başvuru türüyle veya Null olabilen bir türle dönüştürülebilir.

### <a name="operators-and-punctuators"></a>İşleçler ve noktalama işaretçileri

İşleçler ve noktalama birkaç türü vardır. İşleçler, ifadelerde, bir veya daha fazla işlenen ilgili işlemler tanımlamak için kullanılır. Örneğin, ifade `a + b` kullanan `+` işleci iki işlenenleri eklemek için `a` ve `b`. Noktalama işaretçileri gruplama ve ayırma içindir.

```antlr
operator_or_punctuator
    : '{'  | '}'  | '['  | ']'  | '('   | ')'  | '.'  | ','  | ':'  | ';'
    | '+'  | '-'  | '*'  | '/'  | '%'   | '&'  | '|'  | '^'  | '!'  | '~'
    | '='  | '<'  | '>'  | '?'  | '??'  | '::' | '++' | '--' | '&&' | '||'
    | '->' | '==' | '!=' | '<=' | '>='  | '+=' | '-=' | '*=' | '/=' | '%='
    | '&=' | '|=' | '^=' | '<<' | '<<=' | '=>'
    ;

right_shift
    : '>>'
    ;

right_shift_assignment
    : '>>='
    ;
```

İçinde dikey çubuk *right_shift* ve *right_shift_assignment* üretim, diğer üretim söz dizimsel dilbilgisi, herhangi bir türde hiçbir karakter içinde aksine (bile belirtmek için kullanılır boşluk) simgeleri arasında izin verilir. Bu üretim özel için doğru işlemeyi etkinleştirmek için kabul edilir *type_parameter_list*s ([tür parametrelerindeki](classes.md#type-parameters)).

## <a name="pre-processing-directives"></a>Ön işleme yönergeleri

Ön işleme yönergeleri koşullu olarak bölümlerde rapor hata ve uyarı koşulları için kaynak dosyalarının atlamak için ve kaynak kodunun farklı bölgeleri ayırmak için olanağı sunar. "Ön işleme yönergeleri" terimi, yalnızca C ve C++ programlama dilleriyle tutarlılık için kullanılır. C# ' ta ayrı hiçbir ön işleme adım yoktur; ön işleme yönergeleri sözcük analizi aşamasının bir parçası işlenir.

```antlr
pp_directive
    : pp_declaration
    | pp_conditional
    | pp_line
    | pp_diagnostic
    | pp_region
    | pp_pragma
    ;
```

Aşağıdaki ön işleme yönergeleri kullanılabilir:

*  `#define` ve `#undef`, Kaldır, sırasıyla, koşullu derleme simgeleri ve tanımlamak için kullanılır ([bildirim yönergeleri](lexical-structure.md#declaration-directives)).
*  `#if`, `#elif`, `#else`, ve `#endif`, kaynak kodun bölümlerini koşullu olarak atlamak için kullanılır ([koşullu derleme yönergeleri](lexical-structure.md#conditional-compilation-directives)).
*  `#line`, hatalar ve Uyarılar için yayılan satır numaralarını denetlemek için kullanılan ([satır yönergeleri](lexical-structure.md#line-directives)).
*  `#error` ve `#warning`, hatalar ve uyarılar sırasıyla vermek için kullanılır ([tanılama yönergeleri](lexical-structure.md#diagnostic-directives)).
*  `#region` ve `#endregion`, açık kaynak kod bölümlerini işaretlemek için kullanılır ([bölge yönergeleri](lexical-structure.md#region-directives)).
*  `#pragma`, isteğe bağlı bağlamsal bilgi derleyiciye belirtmek için kullanılır ([Pragma yönergeleri](lexical-structure.md#pragma-directives)).

Ön işleme yönergesi her zaman ayrı bir kaynak kod satırı kaplar ve her zaman ile başlayan bir `#` karakter ve bir ön işleme yönergesi adı. Boşluk önce oluşabilir `#` karakter ve arasında `#` karakter ve yönerge adı.

Kaynak satır içeren bir `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, veya `#endregion` yönergesi ile tek satır açıklama sonu olabilecek. Ayrılmış Açıklama ( `/* */` açıklamaları stilini) ön işleme yönergeleri içeren kaynak satırları izin verilmez.

Ön işleme yönergeleri belirteçlerinin ve C# sözdizimi dilbilgisi parçası değildir. Ancak, ön işleme yönergeleri dahil etme veya belirteçleri dönüştürülmelerini dışlama için kullanılabilir ve bu şekilde bir C# programı anlamını etkileyebilir. Örneğin, derlendiğinde, program:
```csharp
#define A
#undef B

class C
{
#if A
    void F() {}
#else
    void G() {}
#endif

#if B
    void H() {}
#else
    void I() {}
#endif
}
```
Program belirteçleri tam aynı sırada sonuçları:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Bu nedenle, sözcük temelinde iki program sözdizimi kurallarına göre oldukça farklı ise, bunlar aynıdır.

### <a name="conditional-compilation-symbols"></a>Koşullu derleme simgeleri

Koşullu derleme tarafından sağlanan işlevselliği `#if`, `#elif`, `#else`, ve `#endif` yönergeleri ifadeleri ön işleme aracılığıyla kontrol edilir ([ifadeleri ön işleme](lexical-structure.md#pre-processing-expressions)) ve koşullu derleme simgeleri.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Koşullu derleme sembol iki durumda olabilir: ***tanımlanan*** veya ***tanımlanmamış***. Dış mekanizmayı (örneğin, bir komut satırı derleyicisi seçeneği) tarafından açıkça tanımlanmadığı sürece sözcük işleme bir kaynak dosyasının başında, koşullu derleme sembol tanımlı değil. Olduğunda bir `#define` yönergesi işlenir, bu yönerge adlı koşullu derleme sembol, kaynak dosyasında tanımlanan olur. Kadar sembol kalacağı bir `#undef` aynı sembolün işlenir veya kaynak dosyasının sonuna ulaşılana kadar yönergesi. Bu bir olduğu çıkarımında olan `#define` ve `#undef` yönergeleri bir kaynak dosyasında diğer kaynak dosyaları aynı programda üzerinde hiçbir etkisi vardır.

Bir ön işleme ifadesinde başvurulduğunda tanımlanmış koşullu derleme sembol Boole değerine sahip `true`, ve bir tanımsız koşullu derleme sembol Boole değerine sahip `false`. Gereksinimi yoktur ifadeleri önceden işleme'de başvurulan önce bildirilen koşullu derleme simgeleri açıkça olabilir. Bunun yerine, bildirilmemiş simgeleri yalnızca tanımsız olur ve böylece değerine sahip `false`.

Koşullu derleme simgeleri için ad alanı diğer adlandırılmış varlıklar bir C# programında ayrıdır. Koşullu derleme simgeleri yalnızca başvurulabilir içinde `#define` ve `#undef` yönergeleri ve ifadeleri önceden işliyor.

### <a name="pre-processing-expressions"></a>Ön işleme ifadeleri

İfadeleri ön işleme içinde gerçekleşebilir `#if` ve `#elif` yönergeleri. İşleçler `!`, `==`, `!=`, `&&` ve `||` ifadeleri, ön işleme izin verilir ve Gruplama için parantez kullanılabilir.

```antlr
pp_expression
    : whitespace? pp_or_expression whitespace?
    ;

pp_or_expression
    : pp_and_expression
    | pp_or_expression whitespace? '||' whitespace? pp_and_expression
    ;

pp_and_expression
    : pp_equality_expression
    | pp_and_expression whitespace? '&&' whitespace? pp_equality_expression
    ;

pp_equality_expression
    : pp_unary_expression
    | pp_equality_expression whitespace? '==' whitespace? pp_unary_expression
    | pp_equality_expression whitespace? '!=' whitespace? pp_unary_expression
    ;

pp_unary_expression
    : pp_primary_expression
    | '!' whitespace? pp_unary_expression
    ;

pp_primary_expression
    : 'true'
    | 'false'
    | conditional_symbol
    | '(' whitespace? pp_expression whitespace? ')'
    ;
```

Bir ön işleme ifadesinde başvurulduğunda tanımlanmış koşullu derleme sembol Boole değerine sahip `true`, ve bir tanımsız koşullu derleme sembol Boole değerine sahip `false`.

Her zaman önceden işleme ifadesi değerlendirmesi bir Boole değeri verir. Ön işleme bir ifade değerlendirme kuralları için sabit bir ifade aynıdır ([sabit ifadeler](expressions.md#constant-expressions)), şeylerdir başvurulabilir yalnızca kullanıcı tarafından tanımlanan varlıkları koşullu derleme simgeleri .

### <a name="declaration-directives"></a>Bildirim yönergeleri

Bildirim yönergeleri, koşullu derleme simge tanımlarını Kaldır veya tanımlamak için kullanılır.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

İşlenmesini bir `#define` yönergesi yönergesini izleyen kaynak satırıyla başlangıç tanımlı duruma, belirtilen koşullu derleme simge neden olur. Benzer şekilde, işlenmesini bir `#undef` yönergesi, yönergesini izleyen kaynak satırı ile başlayarak, tanımsız hale verilen koşullu derleme simge neden olur.

Tüm `#define` ve `#undef` yönergeleri kaynak dosyasında ilk önce gerçekleşmelidir *belirteci* ([belirteçleri](lexical-structure.md#tokens)) kaynak dosyada; Aksi takdirde bir derleme zamanı hatası oluşur. Sezgisel koşullarında `#define` ve `#undef` yönergeleri, kaynak dosyadaki tüm "gerçek kod" gelmelidir.

Örnek:
```csharp
#define Enterprise

#if Professional || Enterprise
    #define Advanced
#endif

namespace Megacorp.Data
{
    #if Advanced
    class PivotTable {...}
    #endif
}
```
geçerli olduğundan `#define` yönergeleri önünde ilk belirteç ( `namespace` anahtar sözcüğü) kaynak dosyadaki.

Aşağıdaki örnek bir derleme zamanı hatası nedeniyle sonuçlanır bir `#define` gerçek kod izler:
```csharp
#define A
namespace N
{
    #define B
    #if B
    class Class1 {}
    #endif
}
```

A `#define` zaten, burada bir araya giren olan tanımlı bir koşullu derleme simge tanımlayabilir `#undef` , sembol. Aşağıdaki örnek bir koşullu derleme sembolünü tanımlar `A` ve yeniden tanımlar.
```csharp
#define A
#define A
```

A `#undef` "tanımlanmamış bir koşullu derleme sembolün tanımını". Aşağıdaki örnekte bir koşullu derleme sembolünü tanımlar `A` ve sonra bu iki kez; ancak tanımsızı ikinci `#undef` hiçbir etkisi yoktur, hala geçerlidir.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Koşullu derleme yönergeleri

Koşullu derleme yönergeleri koşullu olarak dahil etmek veya hariç bir kaynak dosyası bölümleri için kullanılır.

```antlr
pp_conditional
    : pp_if_section pp_elif_section* pp_else_section? pp_endif
    ;

pp_if_section
    : whitespace? '#' whitespace? 'if' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_elif_section
    : whitespace? '#' whitespace? 'elif' whitespace pp_expression pp_new_line conditional_section?
    ;

pp_else_section:
    | whitespace? '#' whitespace? 'else' pp_new_line conditional_section?
    ;

pp_endif
    : whitespace? '#' whitespace? 'endif' pp_new_line
    ;

conditional_section
    : input_section
    | skipped_section
    ;

skipped_section
    : skipped_section_part+
    ;

skipped_section_part
    : skipped_characters? new_line
    | pp_directive
    ;

skipped_characters
    : whitespace? not_number_sign input_character*
    ;

not_number_sign
    : '<Any input_character except #>'
    ;
```

Sözdizimi tarafından belirtildiği gibi koşullu derleme yönergeleri oluşan, sırasıyla kümeleri olarak yazılmış olmalıdır bir `#if` yönergesi, sıfır veya daha fazla `#elif` yönergeleri, sıfır veya bir `#else` yönergesi ve bir `#endif` yönergesi. Kaynak kodu koşullu bölümler arasında yönergeleridir. Her bölümde hemen önceki yönergesi tarafından denetlenir. Bu yönergeler tam kümeleri form sağlanan koşullu bölüme kendi iç içe geçmiş koşullu derleme yönergeleri içeriyor olabilir.

A *pp_conditional* kapsanan en fazla birini seçer *conditional_section*s sözcük normal işlenmesi için:

*  *Pp_expression*sn `#if` ve `#elif` yönergeleri bir verir kadar sırayla değerlendirilir `true`. Bir ifade döndürürse `true`, *conditional_section* karşılık gelen yönergesi seçilir.
*  Tüm *pp_expression*s yield `false`ve eğer bir `#else` yönergesi mevcutsa, *conditional_section* , `#else` yönergesi seçilidir.
*  Aksi takdirde Hayır *conditional_section* seçilir.

Seçili *conditional_section*herhangi biri, işlenir, normal *input_section*: bölümünde yer alan kaynak kodu için sözcük dilbilgisi uyması gerekir; belirteçleri, kaynaktan oluşturulur kod bölümünde; ve ön işleme yönergeleri bölümünde önceden belirlenmiş etkileri vardır.

Kalan *conditional_section*varsa s olarak işlenir *skipped_section*s: ön işleme yönergeleri dışında kaynak bölümünde olmayan esaslarına sözcük dilbilgisi; yok belirteçleri bölümünde kaynak kodundan oluşturulan; ve bölümünde ön işleme yönergeleri sözcüksel olarak doğru olması gerekir, ancak Aksi halde işlenmez. İçinde bir *conditional_section* olarak işleniyor bir *skipped_section*, tüm iç içe geçmiş *conditional_section*s (bulunan iç içe geçmiş `#if`... `#endif` ve `#region`... `#endregion` oluşturur) olarak işlenir *skipped_section*s.

Aşağıdaki örnekte, koşullu derleme yönergelerinin iç içe yerleştirebilirsiniz gösterilmektedir:
```csharp
#define Debug       // Debugging on
#undef Trace        // Tracing off

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
            #if Trace
                WriteToLog(this.ToString());
            #endif
        #endif
        CommitHelper();
    }
}
```

Ön işleme yönergeleri dışında Atlanan kaynak kodu tabi sözcük analizi değil. Örneğin, aşağıdaki Sonlandırılmamış açıklama rağmen geçerli `#else` bölümü:
```csharp
#define Debug        // Debugging on

class PurchaseTransaction
{
    void Commit() {
        #if Debug
            CheckConsistency();
        #else
            /* Do something else
        #endif
    }
}
```

Ancak, ön işleme yönergeleri bile Atlanan bölümlerinde kaynak kodu sözcüksel olarak doğru olması için gerekli olduğunu unutmayın.

Çok satırlı giriş öğelerini içinde görüntülendiğinde ön işleme yönergeleri işlenmez. Örneğin, program:
```csharp
class Hello
{
    static void Main() {
        System.Console.WriteLine(@"hello, 
#if Debug
        world
#else
        Nebraska
#endif
        ");
    }
}
```
Çıktı sonuçları:
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

Özgü durumlarda işlenen ön işleme yönergeleri kümesi üzerinde değerlendirmesi bağlı olabilir *pp_expression*. Örnek:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
her zaman aynı belirteç akışı üretir (`class` `Q` `{` `}`) isteyip istemediğinizi bakılmaksızın `X` tanımlanır. Varsa `X` olan tanımlanan, yalnızca işlenen yönergeleridir `#if` ve `#endif`nedeniyle çok satırlı açıklama. Varsa `X` olan tanımlanmamışsa, sonra üç yönergeleri (`#if`, `#else`, `#endif`) yönerge kümesinin bir parçasıdır.

### <a name="diagnostic-directives"></a>Tanılama yönergeleri

Tanılama yönergeleri açıkça hata ve diğer derleme zamanı hataları ve Uyarıları aynı şekilde bildirilir uyarı iletilerini oluşturmak için kullanılır.

```antlr
pp_diagnostic
    : whitespace? '#' whitespace? 'error' pp_message
    | whitespace? '#' whitespace? 'warning' pp_message
    ;

pp_message
    : new_line
    | whitespace input_character* new_line
    ;
```

Örnek:
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
her zaman bir uyarı ("iadeden önce gereken kod İnceleme") oluşturur ve bir derleme zamanı hatası oluşturur ("bir derleme hem hata ayıklama hem de perakende bırakılamaz"), koşullu simgeleri `Debug` ve `Retail` tanımlanmış olan iki. Unutmayın bir *pp_message* rastgele metin içerebilir; özellikle, doğru biçimlendirilmiş belirteçleri, tek tırnak işareti Word gösterildiği içermesi gerekmez `can't`.

### <a name="region-directives"></a>Bölge yönergeleri

Bölge yönergeleri bölgeleri kaynak kodu açıkça işaretlemek için kullanılır.

```antlr
pp_region
    : pp_start_region conditional_section? pp_end_region
    ;

pp_start_region
    : whitespace? '#' whitespace? 'region' pp_message
    ;

pp_end_region
    : whitespace? '#' whitespace? 'endregion' pp_message
    ;
```

Hiçbir anlam bir bölgeye eklenir; bölgeleri kullanmak için programcı veya otomatikleştirilmiş araçları tarafından kaynak kodun bir bölümünü işaretlemek için içindir. Belirtilen iletiyi bir `#region` veya `#endregion` yönergesi benzer şekilde, hiçbir anlam içeriyor; yalnızca bölge tanımlamak için kullanılır. Eşleşen `#region` ve `#endregion` yönergeleri farklı sahip *pp_message*s.

Bir bölgenin sözcük işleme:
```csharp
#region
...
#endregion
```
Koşullu derleme yönergesi formun sözcük işleme için tam olarak karşılık gelmektedir:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Satır yönergeleri

Satır yönergeleri satır numaraları ve çıkışında uyarılar ve hatalar gibi derleyici tarafından bildirilen ve arayan bilgileri öznitelikleri tarafından kullanılan kaynak dosya adlarını değiştirmek için kullanılabilir ([arayan bilgileri öznitelikleri](attributes.md#caller-info-attributes)).

Satır yönergeleri, bazı diğer metin girişi C# kaynak kodu oluşturun meta programlama araçlarında en yaygın olarak kullanılır.

```antlr
pp_line
    : whitespace? '#' whitespace? 'line' whitespace line_indicator pp_new_line
    ;

line_indicator
    : decimal_digit+ whitespace file_name
    | decimal_digit+
    | 'default'
    | 'hidden'
    ;

file_name
    : '"' file_name_character+ '"'
    ;

file_name_character
    : '<Any input_character except ">'
    ;
```

Hiçbir `#line` yönergeleri, derleyici doğru satır numaraları ve kaynak dosya adlarında çıktısını bildirir. İşleme sırasında bir `#line` içeren yönergesi bir *line_indicator* olmayan `default`, derleyici belirtilen satır numarası (ve dosya adı belirtilmişse) sahip olarak yönergesi sonra satır değerlendirir.

A `#line default` yönergesi, tüm önceki #line direktifleri etkisini tersine çevirir. Tam olarak Hayır olarak ise sonraki satırları, doğru satır bilgilerini derleyici raporları `#line` yönergeleri işlenmemiş.

A `#line hidden` yönergesi dosya üzerinde hiçbir etkisi ve satır numaraları hatası bildirdi iletileri, ancak kaynağı seviyesinde hata ayıklamayı etkilemez. Hata ayıklama sırasında arasındaki tüm satırları bir `#line hidden` yönergesi ve sonraki `#line` yönergesi (olmayan `#line hidden`) sahip satır numarası bilgisi. Kod hata ayıklayıcı ile Adımlama, bu satırları tamamen atlanacak.

Unutmayın bir *file_name* kaçış karakterleri işlenmez; normal dize sabit değeri farklıdır "`\`" karakter yalnızca içinde bir sıradan bir ters eğik çizgi karakteri atayan bir *file_name*.

### <a name="pragma-directives"></a>Pragma yönergeleri

`#pragma` Ön işleme yönergesi, isteğe bağlı bağlamsal bilgi derleyiciye belirtmek için kullanılır. Bilgi sağlanan bir `#pragma` yönergesi hiçbir zaman programı semantiğini değiştirir.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C# sağlar `#pragma` Derleyici uyarılarını kontrol etmek için yönergeleri. Dil gelecek sürümlerinde ek dahil `#pragma` yönergeleri. Diğer C# Derleyicileri birlikte çalışabilirliği sağlamak için Microsoft C# derleyicisi için bilinmeyen derleme hatası kesmez `#pragma` yönergeleri; ancak uyarı oluşturma gibi yönergeleri yapın.

#### <a name="pragma-warning"></a>Pragma Uyarısı

`#pragma warning` Yönergesi devre dışı bırakın veya tümünü geri yüklemek için kullanılan veya belirli bir uyarı kümesini sonraki program metni bir derleme sırasında iletileri.

```antlr
pragma_warning_body
    : 'warning' whitespace warning_action
    | 'warning' whitespace warning_action whitespace warning_list
    ;

warning_action
    : 'disable'
    | 'restore'
    ;

warning_list
    : decimal_digit+ (whitespace? ',' whitespace? decimal_digit+)*
    ;
```

A `#pragma warning` uyarı listesini atlayan yönergesi, tüm uyarıları etkiler. A `#pragma warning` yönergesini içeren bir uyarı listesi listesinde belirtilen uyarıları etkiler.

A `#pragma warning disable` yönerge devre dışı bırakır tüm veya belirli uyarıları kümesi.

A `#pragma warning restore` tüm yönerge geri yükler veya derleme biriminde başında yürürlükte olan durumu uyarıları verilen kümesi. Belirli bir uyarı harici olarak devre dışı bırakılmışsa unutmayın, `#pragma warning restore` (olup tüm veya belirli uyarı), uyarı yeniden izin vermez.

Aşağıdaki örnek, kullanımını gösterir. `#pragma warning` geçici olarak uyarı devre dışı bırakmak için geçersiz zaman üyeleri başvuru, Microsoft C# derleyicisi uyarı numarasını kullanarak bildirilen.
```csharp
using System;

class Program
{
    [Obsolete]
    static void Foo() {}

    static void Main() {
#pragma warning disable 612
    Foo();
#pragma warning restore 612
    }
}
```
