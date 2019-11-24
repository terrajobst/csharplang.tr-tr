---
ms.openlocfilehash: 4676bcd3f0a92260b4e5e20a0aa5b5ec00bf204e
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704068"
---
# <a name="lexical-structure"></a>Sözcük yapısı

## <a name="programs"></a>Programlar

C# ***Program*** , bilinen bir veya daha fazla ***kaynak dosyadan*** ***oluşur (derleme birimleri)*** .[](namespaces.md#compilation-units) Kaynak dosya, bir dizi Unicode karakterden oluşan sıralı bir dizidir. Kaynak dosyaların bir dosya sistemindeki dosyalarla genellikle bire bir yazışmaları vardır, ancak bu yazışma gerekli değildir. En fazla taşınabilirlik için, bir dosya sistemindeki dosyaların UTF-8 kodlaması ile kodlanması önerilir.

Kavramsal olarak bakıldığında, bir program üç adım kullanılarak derlenir:

1. Bir dosyayı belirli bir karakter topluluğunun ve kodlama düzeninden bir Unicode karakter dizisine dönüştüren dönüştürme.
2. Unicode giriş karakterlerinden oluşan bir akışı belirteç akışına çeviren sözcük temelli analiz.
3. Belirteçlerin akışını yürütülebilir koda çeviren sözdizimsel analiz.

## <a name="grammars"></a>Dilbilgisinde

Bu belirtim, C# programlama dilinin sözdizimini iki dilmars kullanarak gösterir. ***Sözlü dilbilgisi*** ([sözlü dilbilgisi](lexical-structure.md#lexical-grammar)), Unicode karakterlerinin form satır sonlandırıcılar, beyaz boşluk, açıklama, belirteç ve ön işleme yönergeleri ile nasıl birleştirildiğini tanımlar. ***Sözdizimsel*** dilbilgisi ([sözdizimsel dilbilgisi](lexical-structure.md#syntactic-grammar)), sözcük temelli dilbilgisinde elde edilen belirteçlerin form C# programlarında nasıl birleştirildiğini tanımlar.

### <a name="grammar-notation"></a>Dilbilgisi gösterimi

Sözlü ve sözdizimsel dilbilgisinde, ANTLR dilbilgisi aracının gösterimi kullanılarak Backus-Naur form sunulmaktadır.

### <a name="lexical-grammar"></a>Sözcük dilbilgisi

Sözcük temelli C# dilbilgisi, [sözcük temelli analiz](lexical-structure.md#lexical-analysis), [belirteçler](lexical-structure.md#tokens)ve [önceden işleme yönergeleriyle](lexical-structure.md#pre-processing-directives)sunulmaktadır. Sözcük tabanlı dilbilgisinin Terminal sembolleri, Unicode karakter kümesinin karakterleridir ve sözlü dilbilgisi, karakterlerin belirteç ([belirteçler](lexical-structure.md#tokens)), beyaz boşluk ([beyaz boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamalar](lexical-structure.md#comments)) ve önceden işleme yönergeleri ([ön işleme yönergeleri](lexical-structure.md#pre-processing-directives)) için nasıl birleştirildiğini belirtir.

Bir C# programdaki her kaynak dosya, sözlü dilbilgisinde ([sözlü analiz](lexical-structure.md#lexical-analysis)) gelen *giriş* üretimine uygun olmalıdır.

### <a name="syntactic-grammar"></a>Sözdizimsel dilbilgisi

' Nin C# sözdizimsel dilbilgisi, bu bölümü izleyen bölümlerde ve appendıces ' de sunulur. Sözdizimsel dilbilgisinde Terminal sembolleri, sözlü dilbilgisi tarafından tanımlanan belirteçlerdir ve sözdizimsel dilbilgisi belirteçleri, belirteçlerin form C# programlarına nasıl birleştirildiğini belirtir.

Bir C# programdaki her kaynak dosya, sözdizimsel dilbilgisi ([derleme birimleri](namespaces.md#compilation-units)) *compilation_unit* üretimine uymalıdır.

## <a name="lexical-analysis"></a>Sözcük temelli analiz

*Giriş* üretimi, C# kaynak dosyanın sözlü yapısını tanımlar. Bir C# programdaki her kaynak dosya, bu sözcük temelli dilbilgisi üretimine uygun olmalıdır.

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

Beş C# temel öğe, bir kaynak dosyanın sözlü yapısını yapar: satır sonlandırıcılar ([satır sonlandırıcılar](lexical-structure.md#line-terminators)), boşluk ([boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamalar](lexical-structure.md#comments)), belirteçler ([belirteçler](lexical-structure.md#tokens)) ve önceden işleme yönergeleri ([ön işleme yönergeleri](lexical-structure.md#pre-processing-directives)). Bu temel öğelerin yalnızca belirteçleri, bir C# programın sözdizimsel dilbilgisinde ([sözdizimsel dilbilgisi](lexical-structure.md#syntactic-grammar)) önemlidir.

Bir C# kaynak dosyanın sözcük işleme, dosyayı sözdizimsel Analize Giriş haline gelen bir belirteç dizisine düşürmektir. Satır sonlandırıcılar, boşluk ve açıklamalar ayrı belirteçlere sunabilir ve önceden işleme yönergeleri kaynak dosyanın bölümlerinin atlanmasına neden olabilir, ancak bu sözcük temelli öğelerin C# programın sözdizimsel yapısını etkilemez.

Enterpolasyonlu dize sabit değerleri ([enterpolasyonlu dize sabit değerleri](lexical-structure.md#interpolated-string-literals)), ilk olarak sözlü analiz tarafından üretilir, ancak tüm enterpolasyonlu tüm dize sabit değerleri çözümlenene kadar çok sayıda giriş öğesine bölünmüştür. Elde edilen belirteçler daha sonra sözdizimsel Analize giriş olarak görev yapar.

Birçok sözlü dilbilgisi üretimi, bir kaynak dosyadaki bir karakter dizisiyle eşleşiyorsa, sözcük işleme her zaman mümkün olan en uzun sözlü öğeyi oluşturur. Örneğin, `//` karakter sırası tek satırlık açıklamanın başlangıcında işlenir çünkü bu sözcük temelli öğe, tek bir `/` belirtecinden daha uzun.

### <a name="line-terminators"></a>Satır sonlandırıcılar

Satır sonlandırıcılar, bir C# kaynak dosyanın karakterlerini çizgilere böler.

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

Dosya sonu işaretçileri ekleyen kaynak kodu düzenlemeyle uyumluluk için ve kaynak dosyanın düzgün bir şekilde sonlandırılmış satırlar halinde görüntülenmesini sağlamak için, aşağıdaki dönüşümler bir C# programdaki her kaynak dosyasına sırayla uygulanır:

*  Kaynak dosyanın son karakteri bir Control-Z karakteri (`U+001A`) ise, bu karakter silinir.
*  Kaynak dosya boş değilse ve kaynak dosyanın son karakteri bir satır başı (`U+000D`), satır besleme (`U+000A`), satır ayırıcı (`U+2028`) veya paragraf ayırıcı (`U+2029`) değilse kaynak dosyanın sonuna bir satır başı karakteri (`U+000D`) eklenir.

### <a name="comments"></a>Açıklamalar

İki yorum biçimi desteklenir: tek satırlık açıklamalar ve sınırlandırılmış açıklamalar. ***Tek satır açıklamaları*** `//` karakterlerle başlayıp kaynak satırın sonuna kadar genişletilir. ***Sınırlandırılmış açıklamalar*** `/*` karakterlerle başlar ve `*/`karakterlerle biter. Sınırlandırılmış açıklamalar birden çok satıra yayılabilir.

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
    : '/*' delimited_comment_section* asterisk+ '/'
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

Açıklamalar iç içe değildir. `/*` ve `*/` karakter dizileri `//` açıklamasında özel bir anlamı yoktur ve `//` ve `/*`, sınırlı bir açıklama içinde özel bir anlamı yoktur.

Açıklamalar karakter ve dize değişmez değerleri içinde işlenmez.

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
Birkaç tek satırlık açıklama gösterir.

### <a name="white-space"></a>Boşluk

Boşluk karakteri, yatay sekme karakteri, dikey sekme karakteri ve form besleme karakteri olan Unicode sınıf ZS (boşluk karakterini de içerir) ile herhangi bir karakter olarak tanımlanır.

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a>Belirteçler

Birkaç tür belirteç vardır: tanımlayıcılar, anahtar sözcükler, sabit değerler, işleçler ve noktalama işaretleri. Boşluk ve açıklamalar belirteçler değildir, ancak belirteçler için ayırıcılar işlevi görür.

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

Unicode karakter kaçış dizisi bir Unicode karakteri temsil eder. Unicode karakter kaçış dizileri tanımlayıcılarda ([tanımlayıcılar](lexical-structure.md#identifiers)), karakter sabit değerlerinde ([karakter sabit](lexical-structure.md#character-literals)değerleri) ve normal dize değişmez değerlerinde ([dize sabit değerleri](lexical-structure.md#string-literals)) işlenir. Unicode karakter kaçış başka bir konumda işlenmez (örneğin, bir işleç, noktalama veya anahtar sözcük oluşturmak için).

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

Unicode kaçış sırası, "`\u`" veya "`\U`" karakterlerinden sonra onaltılık sayı tarafından oluşturulan tek Unicode karakteri temsil eder. , C# Karakter ve dize değerlerinde Unicode kod noktalarının 16 bitlik bir kodlamasını kullandığından, bir karakter sabit değerinde u + 10000-u + 10FFFF aralığındaki bir Unicode karaktere izin verilmez ve bir dize sabit değerinde Unicode vekil çifti kullanılarak temsil edilir. 0x10FFFF üzerinde kod noktaları olan Unicode karakterler desteklenmez.

Birden çok çeviri gerçekleştirilmez. Örneğin, "`\u005Cu005C`" dize değişmez değeri "`\`" yerine "`\u005C`" ile eşdeğerdir. `\u005C` Unicode değeri "`\`" karakteridir.

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
"`f`" harfinin kaçış sırası olan `\u0066`birkaç kullanımını gösterir. Program şu şekilde eşdeğerdir
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

Bu bölümde verilen tanımlayıcılara yönelik kurallar tam olarak Unicode standart ek 31 tarafından Önerilenlere karşılık gelir, ancak alt çizgi ilk karakter olarak izin verilir (C programlama dilinde geleneksel olduğu gibi), tanımlayıcılarda Unicode kaçış dizilerine izin verilir ve "`@`" karakteri, anahtar sözcüklerin tanımlayıcı olarak kullanılmasına olanak tanımak için önek olarak izin verilir.

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

Yukarıda belirtilen Unicode karakter sınıfları hakkında daha fazla bilgi için bkz. Unicode standart, sürüm 3,0, Bölüm 4,5.

Geçerli tanımlayıcıların örnekleri arasında "`identifier1`", "`_identifier2`" ve "`@if`" sayılabilir.

Uyumlu bir programdaki tanımlayıcı, Unicode standart ek 15 tarafından tanımlanan, Unicode normalleştirme biçimi C tarafından tanımlanan kurallı biçimde olmalıdır. Normalleştirme biçimi C 'de olmayan bir tanımlayıcı ile karşılaştığın davranışı uygulama tanımlı; Ancak, bir tanılama gerekli değildir.

"`@`" ön eki, diğer programlama dilleri ile arabirim oluştururken yararlı olan tanımlayıcı olarak anahtar kelimelerin kullanılmasını sağlar. `@` karakter aslında tanımlayıcının bir parçası değildir, bu nedenle tanımlayıcı, ön ek olmadan diğer dillerde normal tanımlayıcı olarak görülemeyebilir. `@` ön ekine sahip bir tanımlayıcıya, tam ***tanımlayıcı***denir. Anahtar sözcük olmayan tanımlayıcılar için `@` öneki kullanılmasına izin verilir, ancak stil nedeniyle kesinlikle önerilmez.

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
"`bool`" adlı bir parametre alan "`static`" adlı statik bir yöntemle "`class`" adlı bir sınıf tanımlar. Anahtar sözcüklerde Unicode kaçışlara izin verilmemesine, "`cl\u0061ss`" belirtecinin bir tanımlayıcı olduğuna ve "`@class`" ile aynı tanımlayıcıda olduğuna unutmayın.

İki tanımlayıcı, aşağıdaki dönüşümler uygulandıktan sonra aynı olursa, sırasıyla aynı kabul edilir:

*  Kullanıldıysa, "`@`" ön eki kaldırılır.
*  Her *unicode_escape_sequence* karşılık gelen Unicode karakteriyle dönüştürülür.
*  Tüm *formatting_character*kaldırılır.

Art arda iki alt çizgi karakteri (`U+005F`) içeren tanımlayıcılar, uygulama tarafından kullanılmak üzere ayrılmıştır. Örneğin, bir uygulama iki alt çizgi ile başlayan genişletilmiş anahtar sözcükler sağlayabilir.

### <a name="keywords"></a>Anahtar Sözcükler

***Anahtar sözcüğü*** , ayrılmış karakterlerin tanımlayıcı benzeri bir dizidir ve `@` karakteri tarafından ön çıkması dışında bir tanımlayıcı olarak kullanılamaz.

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

Dilbilgisinde bazı yerlerde, belirli tanımlayıcıların özel anlamları vardır ancak anahtar sözcük değildir. Bu tür tanımlayıcılar bazen "bağlamsal anahtar sözcükler" olarak adlandırılır. Örneğin, bir özellik bildiriminde, "`get`" ve "`set`" tanımlayıcılarının özel anlamları ([erişimciler](classes.md#accessors)) vardır. Bu konumlarda `get` veya `set` dışında bir tanımlayıcıya izin verilmez. bu nedenle bu kullanım, bu sözcüklerin tanımlayıcı olarak kullanımıyla çakışmaz. Başka durumlarda, örtük olarak yazılan yerel değişken bildirimlerinde ([yerel değişken bildirimleri](statements.md#local-variable-declarations)) "`var`" tanımlayıcısına sahip gibi, bir bağlamsal anahtar sözcük, belirtilen adlarla çakışabilir. Bu gibi durumlarda, belirtilen ad tanımlayıcı anahtar sözcük olarak tanımlayıcı kullanılarak önceliklidir.

### <a name="literals"></a>Sabit değerler

***Değişmez*** değer, bir değerin kaynak kod gösterimidir.

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

İki Boolean değişmez değer vardır: `true` ve `false`.

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

*Boolean_literal* türü `bool`.

#### <a name="integer-literals"></a>Tamsayı sabit değerleri

Tamsayı sabit değerleri `int`, `uint`, `long`ve `ulong`türlerin değerlerini yazmak için kullanılır. Tamsayı sabit değerlerinde iki olası biçim vardır: Decimal ve onaltılı.

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

Bir tamsayı sabit değerinin türü aşağıdaki şekilde belirlenir:

*  Değişmez değerin soneki yoksa, bu türlerin ilk değeri temsil edilebilir: `int`, `uint`, `long`, `ulong`.
*  Sabit değer `U` veya `u`tarafından sonmışsa, bu türlerin değeri temsil edilebilir: `uint`, `ulong`.
*  Sabit değer `L` veya `l`tarafından sonmışsa, bu türlerin değeri temsil edilebilir: `long`, `ulong`.
*  Sabit değer `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`veya `lu`tarafından düzeltildiğinde, `ulong`türüdür.

Bir tamsayı değişmez değeri ile temsil edilen değer `ulong` türü aralığının dışındaysa, bir derleme zamanı hatası oluşur.

Stil olarak, "`l`" harfinin "`1`" basamağıyla karıştırilmesi kolay olduğundan, bu `long`tür "`l`" yerine "`L`" kullanılması önerilir.

En küçük olası `int` ve `long` değerlerinin ondalık tamsayı değişmez değer olarak yazılmasına izin vermek için aşağıdaki iki kural bulunur:

* 2147483648 (2 ^ 31) değerine sahip bir *decimal_integer_literal* ve tek başına eksi işleç belirtecinin ([birli eksi işleci](expressions.md#unary-minus-operator)) hemen ardından belirteç olarak hiçbir *integer_type_suffix* belirdiğinde, sonuç,-2147483648 (-2 ^ 31) değerine sahip `int` türünde bir sabittir. Diğer tüm durumlarda, böyle bir *decimal_integer_literal* `uint`türüdür.
* 9223372036854775808 (2 ^ 63) değerine sahip bir *decimal_integer_literal* ve *integer_type_suffix* veya *integer_type_suffix* `L` veya `l`, tek başına eksi işleç belirtecinin ([birli eksi işleci](expressions.md#unary-minus-operator)) hemen ardından gelen belirteç olarak göründüğünde, sonuç,-9223372036854775808 değeri ile `long` türünde bir sabittir (-2 ^ 63). Diğer tüm durumlarda, böyle bir *decimal_integer_literal* `ulong`türüdür.

#### <a name="real-literals"></a>Gerçek sabit değerler

Gerçek sabit değerler `float`, `double`ve `decimal`türlerin değerlerini yazmak için kullanılır.

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

*Real_type_suffix* belirtilmemişse, gerçek değişmez değerin türü `double`. Aksi halde gerçek tür soneki gerçek değişmez değerin türünü aşağıdaki gibi belirler:

*  `F` veya `f` tarafından düzeltilen gerçek sabit değer `float`türündedir. Örneğin, `1f`, `1.5f`, `1e10f`ve `123.456F` değişmez değer `float`türüdür.
*  `D` veya `d` tarafından düzeltilen gerçek sabit değer `double`türündedir. Örneğin, `1d`, `1.5d`, `1e10d`ve `123.456D` değişmez değer `double`türüdür.
*  `M` veya `m` tarafından düzeltilen gerçek sabit değer `decimal`türündedir. Örneğin, `1m`, `1.5m`, `1e10m`ve `123.456M` değişmez değer `decimal`türüdür. Bu değişmez değer tam değeri alarak bir `decimal` değere dönüştürülür ve gerekirse, banker 'in yuvarlanması ([ondalık türü](types.md#the-decimal-type)) kullanılarak en yakın gösterilebilir tablo değerine yuvarlanır. Değer yuvarlanmamışsa veya değer sıfır değilse (ikinci durumda, işaret ve ölçek 0 olacak şekilde), sabit değerde görünen herhangi bir ölçek korunur. Bu nedenle, `2.900m` değişmez değeri, işaret `0`, katsayı `2900`ve ölçek `3`ile ondalık oluşturacak şekilde ayrıştırılacaktır.

Belirtilen sabit değer belirtilen türde temsil edilemez, derleme zamanı hatası oluşur.

`float` veya `double` türünde gerçek bir sabit değerin değeri, IEEE "en yakına yuvarla" modu kullanılarak belirlenir.

Gerçek bir sabit değerde, ondalık ayırıcıdan sonra her zaman ondalık basamakların gerekli olduğunu unutmayın. Örneğin, `1.3F` gerçek bir değişmez değerdir, ancak `1.F` değildir.

#### <a name="character-literals"></a>Karakter sabit değerleri

Bir karakter sabiti tek bir karakteri temsil eder ve genellikle `'a'`gibi tırnak içindeki bir karakterden oluşur.

Note: ANTLR dilbilgisi gösterimi aşağıdaki kafa karıştırıcı yapar! ANTLR 'de yazdığınızda `\'` tek tırnak `'`temsil eder. `\\` yazdığınızda, tek bir ters eğik çizgi `\`temsil eder. Bu nedenle, bir karakter sabit değeri için ilk kural tek bir teklifle, ardından bir karakterle ve tek bir teklifle başladığı anlamına gelir. Ve on bir olası basit kaçış dizisi `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.

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

Bir *karaktere* ters eğik çizgi karakteri (`\`) izleyen bir karakter şu karakterlerden biri olmalıdır: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Aksi takdirde, bir derleme zamanı hatası oluşur.

Bir onaltılık kaçış sırası, "`\x`" öğesinden sonra onaltılık sayı tarafından oluşturulan değeri içeren tek bir Unicode karakteri temsil eder.

Bir karakter sabiti ile temsil edilen değer `U+FFFF`değerinden büyükse, derleme zamanı hatası oluşur.

Bir karakter sabit değerinde Unicode karakter kaçış sırası ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)) `U+FFFF`için `U+0000` aralığında olmalıdır.

Basit bir kaçış dizisi, aşağıdaki tabloda açıklandığı gibi bir Unicode karakter kodlamasını temsil eder.


| __Kaçış sırası__ | __Karakter adı__ | __Unicode kodlaması__ |
|---------------------|--------------------|----------------------|
| `\'`                | Tek tırnak       | `0x0027`             | 
| `\"`                | Çift tırnak       | `0x0022`             | 
| `\\`                | Ters eğik çizgi          | `0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Uyarı              | `0x0007`             | 
| `\b`                | Geri Al tuşu          | `0x0008`             | 
| `\f`                | Form akışı          | `0x000C`             | 
| `\n`                | Yeni satır           | `0x000A`             | 
| `\r`                | Satır başı    | `0x000D`             | 
| `\t`                | Yatay sekme     | `0x0009`             | 
| `\v`                | Dikey sekme       | `0x000B`             | 

*Character_literal* türü `char`.

#### <a name="string-literals"></a>Dize sabit değerleri

C#dize sabit değerlerinin iki biçimini destekler: ***normal dize sabit değerleri*** ve tam ***dize sabit değerleri***.

Normal bir dize sabit değeri, `"hello"`gibi çift tırnaklarda bulunan sıfır veya daha fazla karakterden oluşur ve hem basit kaçış dizilerini (tab karakteri için `\t` gibi), onaltılı ve Unicode kaçış dizilerini içerebilir.

Tam dize değişmez değeri, bir `@` karakteri ve ardından çift tırnak karakteri, sıfır veya daha fazla karakter ve bir kapanış çift tırnak karakteri içerir. Basit bir örnek `@"hello"`. Tam bir dize sabit değerinde, sınırlayıcılar arasındaki karakterler tam olarak yorumlanır ve tek özel durum *quote_escape_sequence*. Özellikle, basit kaçış dizileri ve onaltılı ve Unicode kaçış dizileri, tam dize değişmez değerlerinde işlenmez. Tam dize değişmez değeri birden çok satıra yayılabilir.

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

Bir *regular_string_literal_character* ters eğik çizgi karakteri (`\`) izleyen bir karakter şu karakterlerden biri olmalıdır: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Aksi takdirde, bir derleme zamanı hatası oluşur.

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
çeşitli dize sabit değerlerini gösterir. `j`son dize değişmez değeri, birden çok satıra yayılan, tam bir dize sabit değeri. Yeni satır karakterleri gibi boşluk da dahil olmak üzere tırnak işaretleri arasındaki karakterler tam olarak korunur.

Bir onaltılık kaçış sırası değişken sayıda onaltılık basamağa sahip olduğundan, değişmez değer `"\x123"` on altılı 123 içeren tek bir karakter içerir. Onaltılık değeri 12 ve ardından karakter 3 olan bir karakter içeren bir dize oluşturmak için, bunun yerine `"\x00123"` veya `"\x12" + "3"` yazabilir.

*String_literaL* türü `string`.

Her dize sabit değeri, yeni bir dize örneği ile sonuçlanır. Dize eşitlik işlecine ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) göre eşdeğer olan iki veya daha fazla dize değişmez değeri aynı programda görünürse, bu dize değişmez değerleri aynı dize örneğine başvurur. Örneğin, tarafından oluşturulan çıkış
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
iki değişmez değer aynı dize örneğine başvurduğundan `True`.

#### <a name="interpolated-string-literals"></a>Ara değerli dize sabit değerleri

Enterpolasyonlu dize sabit değerleri dize sabit değerlerine benzerdir, ancak `{` ve `}`ile ayrılmış olan ve ifadeler meydana gelebilir. Çalışma zamanında, ifadeler, metin biçimlerinin, delik oluşması durumunda dize üzerinde yer aldığı amaçla değerlendirilir. Dize ilişkilendirtiğinin sözdizimi ve semantiği bölümünde açıklanmaktadır ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)).

Dize sabit değerleri gibi, enterpolasyonlu dize değişmez değerleri normal veya tam olabilir. Enterpolasyonlu normal dize sabit değerleri `$"` ve `"`tarafından sınırlandırılır ve enterpolasyonlu tam dize değişmez değerleri `$@"` ve `"`ile sınırlandırılmıştır.

Diğer değişmez değerler gibi, bir enterpolasyonlu dize sabit değerinin sözcük analizi, aşağıdaki Dilbilgisine göre ilk olarak tek bir belirtece neden olur. Ancak, sözdizimsel Analize göre, bir ara değer dizesinin tek belirteci, delikleri kapsayan dizenin bölümleri için birkaç belirtece bölünmüştür ve deliklere gerçekleşen giriş öğeleri, sözcüksel olarak analiz edilir. Bu işlem, işlenmek üzere daha fazla enterpolasyonlu dize değişmezleri üretebilir. ancak, sözcüksel doğru olursa, sonunda sözdizimsel çözümlemenin işlemesi için bir dizi belirtece yol açacaktır.

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

*İnterpolated_string_literal* belirteci, *interpolated_string_literal*oluşma sırasına göre aşağıdaki gibi birden fazla belirteç ve diğer giriş öğeleri olarak yeniden yorumlanır:

* Aşağıdakilerin oluşumları ayrı ayrı belirteçler olarak yeniden yorumlanır: önde gelen `$` işareti, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* ve *interpolated_verbatim_string_end*.
* Bunlar arasında *regular_balanced_text* ve *verbatim_balanced_text* örnekleri *input_section* ([sözcük temelli analiz](lexical-structure.md#lexical-analysis)) olarak yeniden işlenir ve giriş öğelerinin ortaya çıkan sırası olarak yeniden yorumlanır. Bunlar, yeniden yorumlanan enterpolasyonlu dize sabit belirteçlerini içerebilir.

Sözdizimsel analiz, belirteçleri bir *interpolated_string_expression* ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)) olarak yeniden birleştirir.

Örnek TODO


#### <a name="the-null-literal"></a>Null değişmez değeri

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* , örtülü olarak bir başvuru türüne veya null olabilir türe dönüştürülebilir.

### <a name="operators-and-punctuators"></a>İşleçler ve noktaleyiciler

Birkaç tür işleç ve noktalama vardır. İşleçler, bir veya daha fazla işlenen ile ilgili işlemleri betimleyen ifadelerde kullanılır. Örneğin, `a + b` ifadesi iki işleneni `a` ve `b`eklemek için `+` işlecini kullanır. Noktaleyiciler gruplandırma ve ayırma içindir.

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

*Right_shift* ve *right_shift_assignment* üretimlerde dikey çubuk, söz dizimi arasındaki diğer üretimlerin aksine, belirteçler arasında herhangi bir türde (hatta boşluk değil) bir karakter olmasına izin verileceğini belirtmek için kullanılır. Bu üretimler, *type_parameter_list*s ([tür parametrelerinin](classes.md#type-parameters)) doğru işlemesini etkinleştirmek için özel olarak değerlendirilir.

## <a name="pre-processing-directives"></a>Ön işleme yönergeleri

Ön işleme yönergeleri, kaynak dosyalarının bölümlerini koşullu olarak atlama, hata ve uyarı koşullarını raporlama ve kaynak kodunun farklı bölgelerini ayırıcı yapma yeteneği sağlar. "Ön işleme yönergeleri" terimi yalnızca C ve C++ programlama dilleri ile tutarlılık için kullanılır. ' C#De, ayrı bir ön işleme adımı yoktur; ön işleme yönergeleri, sözcük temelli analiz aşamasının bir parçası olarak işlenir.

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

Aşağıdaki ön işleme yönergeleri mevcuttur:

*  `#define` ve `#undef`, sırasıyla koşullu derleme sembollerini ([bildirim yönergeleri](lexical-structure.md#declaration-directives)) tanımlamak ve kaldırmak için kullanılır.
*  `#if`, `#elif`, `#else`ve `#endif`kaynak kodu bölümlerini koşullu olarak atlamak için kullanılır ([koşullu derleme yönergeleri](lexical-structure.md#conditional-compilation-directives)).
*  hatalar ve uyarılar için oluşturulan satır numaralarını denetlemek için kullanılan `#line`([satır yönergeleri](lexical-structure.md#line-directives)).
*  `#error` ve `#warning`sırasıyla hata ve uyarı vermek için kullanılır ([tanı yönergeleri](lexical-structure.md#diagnostic-directives)).
*  Kaynak kodun ([bölge yönergeleri](lexical-structure.md#region-directives)) bölümlerini açıkça işaretlemek için kullanılan `#region` ve `#endregion`.
*  derleyici için isteğe bağlı bağlamsal bilgileri belirtmek için kullanılan `#pragma`([pragma yönergeleri](lexical-structure.md#pragma-directives)).

Bir ön işleme yönergesi her zaman ayrı bir kaynak kod satırı kaplar ve her zaman bir `#` karakterle ve bir ön işleme yönergesi adıyla başlar. Boşluk, `#` karakterden önce ve `#` karakteri ile yönerge adı arasında gerçekleşebilir.

`#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`veya `#endregion` yönergesini içeren bir kaynak satırı, tek satırlık bir açıklama ile sona başlayabilir. Önceden işleme yönergelerini içeren kaynak satırlarda sınırlandırılmış açıklamalara (açıklamaların `/* */` stili) izin verilmez.

Ön işleme yönergeleri belirteç değildir ve sözdizimi dilbilgisinde bir C#parçası değildir. Ancak, önceden işleme yönergeleri, belirteç dizilerini dahil etmek veya hariç tutmak için kullanılabilir ve bu şekilde bir C# programın anlamını etkileyebilir. Örneğin, derlendiğinde program:
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
program ile aynı belirteçlerin tam dizisiyle sonuçlanır:
```csharp
class C
{
    void F() {}
    void I() {}
}
```

Bu nedenle, sözcüksel olarak iki program çok farklı, sözdizimsel olarak aynıdır.

### <a name="conditional-compilation-symbols"></a>Koşullu derleme sembolleri

`#if`, `#elif`, `#else`ve `#endif` yönergeleri tarafından sunulan koşullu derleme işlevselliği, ön işleme ifadeleri ([ön işleme ifadeleri](lexical-structure.md#pre-processing-expressions)) ve koşullu derleme sembolleri aracılığıyla denetlenir.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Koşullu derleme sembolünün iki olası durumu vardır: ***tanımlı*** veya ***tanımsız***. Bir kaynak dosyanın sözcük işleme başlangıcında, bir dış mekanizma (bir komut satırı derleyici seçeneği gibi) tarafından açıkça tanımlanmadığı sürece koşullu derleme simgesi tanımsızdır. Bir `#define` yönergesi işlendiğinde, Bu yönergede adlı koşullu derleme sembolü bu kaynak dosyada tanımlanır. Sembol, aynı simgenin bir `#undef` yönergesi işlenene veya kaynak dosyanın sonuna ulaşılana kadar tanımlanmış olarak kalır. Bunun bir etkisi, bir kaynak dosyadaki `#define` ve `#undef` yönergelerinin aynı programdaki diğer kaynak dosyaları üzerinde hiçbir etkisi olmaması.

Bir ön işleme ifadesinde başvuruluyorsa, tanımlı bir koşullu derleme sembolünün `true`Boolean değeri vardır ve tanımsız bir koşullu derleme sembolü Boolean değeri `false`olur. Koşullu derleme simgelerinin, ön işleme ifadelerinde başvurulmadan önce açıkça bildirilmesini gerektiren bir gereksinim yoktur. Bunun yerine, bildirilmemiş semboller yalnızca tanımsız olur ve bu nedenle değeri `false`olur.

Koşullu derleme sembolleri için ad alanı farklıdır ve bir C# programdaki diğer tüm adlandırılmış varlıklardan ayrıdır. Koşullu derleme sembollerine yalnızca `#define` ve `#undef` yönergeleriyle ve önceden işleme ifadelerinde başvurulabilir.

### <a name="pre-processing-expressions"></a>Ön işleme ifadeleri

`#if` ve `#elif` yönergelerinde ön işleme ifadeleri oluşabilir. `!`, `==`, `!=`, `&&` ve `||` işleçleri ön işleme ifadelerinde izin verilir ve gruplama için parantez kullanılabilir.

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

Bir ön işleme ifadesinde başvuruluyorsa, tanımlı bir koşullu derleme sembolünün `true`Boolean değeri vardır ve tanımsız bir koşullu derleme sembolü Boolean değeri `false`olur.

Bir ön işleme ifadesinin değerlendirmesi her zaman bir Boole değeri verir. Bir ön işleme ifadesi için değerlendirme kuralları bir sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) ile aynıdır, ancak başvurılabilen Kullanıcı tanımlı varlıkların koşullu derleme sembolleri vardır.

### <a name="declaration-directives"></a>Bildirim yönergeleri

Bildirim yönergeleri, koşullu derleme sembollerini tanımlamak veya tanımlamak için kullanılır.

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

`#define` yönergesinin işlenmesi, belirtilen koşullu derleme sembolünün, yönergeyi izleyen kaynak satırla başlayarak tanımlanmasına neden olur. Benzer şekilde, bir `#undef` yönergesinin işlenmesi verilen koşullu derleme sembolünün, yönergeyi izleyen kaynak satırla başlayarak tanımsız hale gelmesine neden olur.

Kaynak dosyadaki tüm `#define` ve `#undef` yönergeleri, kaynak dosyadaki ilk *belirteçten* ([belirteçler](lexical-structure.md#tokens)) önce gerçekleşmelidir; Aksi takdirde, derleme zamanı hatası oluşur. Sezgisel koşullarda, `#define` ve `#undef` yönergelerinin kaynak dosyadaki herhangi bir "gerçek kod" önünde olması gerekir.

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
, `#define` yönergeleri kaynak dosyadaki ilk belirteçten (`namespace` anahtar sözcüğü) önce gelmelidir.

Aşağıdaki örnek, `#define` gerçek kodu izlediği için derleme zamanı hatası ile sonuçlanır:
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

`#define`, o sembol için hiçbir `#undef` araya girmeden önce tanımlanmış olan bir koşullu derleme sembolünü tanımlayabilir. Aşağıdaki örnek `A` koşullu derleme sembolünü tanımlar ve sonra yeniden tanımlar.
```csharp
#define A
#define A
```

Bir `#undef` tanımlı olmayan koşullu bir derleme sembolünü "tanımlayabilir". Aşağıdaki örnek, `A` koşullu bir derleme sembolünü tanımlar ve ardından iki kez tanımlamıyor; İkinci `#undef` hiçbir etkisi olmasa da, hala geçerli olur.
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a>Koşullu derleme yönergeleri

Koşullu derleme yönergeleri, kaynak dosyanın bölümlerini koşullu olarak dahil etmek veya hariç tutmak için kullanılır.

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

Sözdizimi tarafından gösterildiği gibi, koşullu derleme yönergeleri, sırasıyla, bir `#if` yönerge, sıfır veya daha fazla `#elif` yönergeleri, sıfır veya bir `#else` yönergesi ve bir `#endif` yönergesi ile oluşan kümeler olarak yazılmalıdır. Yönergeler arasında kaynak kodunun koşullu bölümleri bulunur. Her bölüm hemen önceki yönerge tarafından denetlenir. Koşullu bir bölümün kendisi bu yönergeler için tüm kümeler için iç içe geçmiş koşullu derleme yönergeleri içerebilir.

*Pp_conditional* normal sözcük işleme için kapsanan *conditional_section*s 'nin en çok birini seçer:

*  `#if` ve `#elif` yönergelerinin *pp_expression*, tek bir `true`bir sıra olarak değerlendirilir. Bir ifade `true`veriyor ise, karşılık gelen yönergenin *conditional_section* seçilir.
*  Tüm *pp_expression*`false`ve bir `#else` yönergesi varsa, `#else` yönergesinin *conditional_section* seçilir.
*  Aksi takdirde, *conditional_section* seçili değildir.

Seçilen *conditional_section*, varsa normal *input_section*olarak işlenir: bölümünde yer alan kaynak kodu, sözlü dilbilgisine uymalıdır; belirteçler, bölümündeki kaynak koddan oluşturulur; ve bölümündeki ön işleme yönergelerinin tanımlanmış etkileri vardır.

Kalan *conditional_section*s, varsa, *skipped_section*s olarak işlenir: ön işleme yönergeleri haricinde, bölümdeki kaynak kodunun sözlü dilbilgisine bağlı olmaması gerekir; bölümünde kaynak koddan hiçbir belirteç oluşturulmaz; ve bölümündeki ön işleme yönergeleri, sözcüksel doğru olmalıdır ancak başka türlü işlenmemelidir. *Skipped_section*olarak işlenmekte olan bir *conditional_section* içinde, iç içe geçmiş *conditional_section*(iç içe geçmiş `#if`...`#endif` ve `#region`...`#endregion` yapılar), aynı zamanda *skipped_section*s olarak işlenir.

Aşağıdaki örnek, koşullu derleme yönergelerinin nasıl iç içe geçirebileceği gösterilmektedir:
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

Ön işleme yönergeleri hariç, atlanan kaynak kodu sözcük temelli analize tabi değildir. Örneğin, `#else` bölümünde Sonlandırılmamış açıklamaya karşın aşağıdakiler geçerlidir:
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

Ancak, bu ön işleme yönergelerinin kaynak kodun atlanan bölümlerinde bile, sözcüksel olarak doğru olması gerektiğini unutmayın.

Önceden işleme yönergeleri, çok satırlı giriş öğeleri içinde göründükleri zaman işlenmez. Örneğin, program:
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
çıkışın sonucu:
```console
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

Bu durumlarda, işlenen önceden işleme yönergelerinin kümesi, *pp_expression*değerlendirmesine bağlı olarak değişebilir. Örnek:
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
, `X` tanımlanmadığına bakılmaksızın her zaman aynı belirteç akışını (`class` `Q` `{` `}`) üretir. `X` tanımlanmışsa, çok satırlı yorum nedeniyle işlenen tek yönergeler `#if` ve `#endif`. `X` tanımlanmamışsa, üç yönerge (`#if`, `#else`, `#endif`) yönerge kümesinin bir parçasıdır.

### <a name="diagnostic-directives"></a>Tanılama yönergeleri

Tanılama yönergeleri, diğer derleme zamanı hatalarıyla ve uyarılarla aynı şekilde bildirilen hata ve uyarı iletilerini açık bir şekilde oluşturmak için kullanılır.

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
her zaman bir uyarı üretir ("iade etmeden önce kod incelemesi gereklidir") ve bir derleme zamanı hatası üretir ("bir derleme hem hata ayıklama hem de perakende olamaz") ve koşullu semboller `Debug` ve `Retail` ikisi tanımlıysa. *Pp_message* , rastgele metin içerebileceğini unutmayın; Özellikle, Word `can't`tek tırnak içinde gösterildiği gibi iyi biçimlendirilmiş belirteçler içermemelidir.

### <a name="region-directives"></a>Bölge yönergeleri

Bölge yönergeleri, kaynak kodu bölgelerini açıkça işaretlemek için kullanılır.

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

Bir bölgeye hiçbir anlam anlamı eklenmez; bölgeler, programcı tarafından veya kaynak kodun bir bölümünü işaretlemek için otomatikleştirilmiş araçlar tarafından kullanılmak üzere tasarlanmıştır. `#region` veya `#endregion` yönergesinde belirtilen ileti benzer bir anlam içermez; yalnızca bölgeyi tanımlamak için kullanılır. Eşleşen `#region` ve `#endregion` yönergelerinin farklı *pp_message*öğeleri olabilir.

Bir bölgenin sözcük işleme işlemi:
```csharp
#region
...
#endregion
```
formun koşullu bir derleme yönergesinin yalnızca sözcük işleme öğesine karşılık gelir:
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a>Çizgi yönergeleri

Satır yönergeleri, uyarı ve hatalar gibi, derleyici tarafından raporlanan ve arayan bilgi öznitelikleri ([çağıran bilgi öznitelikleri](attributes.md#caller-info-attributes)) tarafından kullanılan satır numaralarını ve kaynak dosya adlarını değiştirmek için kullanılabilir.

Satır yönergeleri en yaygın olarak, diğer bir metin girişinden C# kaynak kodu oluşturan meta programlama araçlarında kullanılır.

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

`#line` yönergesi yoksa, derleyici çıktıda gerçek satır numaralarını ve kaynak dosya adlarını raporlar. `default`olmayan bir *line_indicator* içeren bir `#line` yönergesini işlerken, derleyici, belirtilen satır numarası (ve belirtilmişse dosya adı) gibi yönergeden sonra satırı değerlendirir.

`#line default` yönergesi, önceki tüm #line yönergelerinin etkisini tersine çevirir. Derleyici, kesin olmayan `#line` yönergelerinin olmadığı gibi, sonraki satırlar için de gerçek satır bilgilerini raporlar.

`#line hidden` yönergesinin hata iletilerinde bildirilen dosya ve satır numaraları üzerinde hiçbir etkisi yoktur, ancak kaynak düzeyinde hata ayıklamayı etkiler. Hata ayıklarken, `#line hidden` yönergesi ve sonraki `#line` yönergesi (`#line hidden`olmayan) arasındaki tüm satırlarda satır numarası bilgisi yoktur. Hata ayıklayıcıda kod üzerinden adımla, bu satırlar tamamen atlanır.

*File_name* , kaçış karakterlerinin işlenmediği normal bir dize sabit değerinden farklı olduğunu unutmayın; "`\`" karakteri, bir *file_name*içindeki normal ters eğik çizgi karakterini belirtir.

### <a name="pragma-directives"></a>Pragma yönergeleri

`#pragma` ön işleme yönergesi, isteğe bağlı bağlamsal bilgileri derleyiciye belirtmek için kullanılır. `#pragma` yönergesinde sağlanan bilgiler hiçbir şekilde program semantiğini değiştirmez.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#Derleyici uyarılarını denetlemek için `#pragma` yönergeler sağlar. Dilin gelecekteki sürümleri ek `#pragma` yönergeler içerebilir. Diğer C# derleyicilerle birlikte çalışabilirliği sağlamak Için, Microsoft C# derleyicisi bilinmeyen `#pragma` yönergeleri için derleme hataları vermez; Bu tür yönergeler ancak uyarı oluşturur.

#### <a name="pragma-warning"></a>Pragma uyarısı

`#pragma warning` yönergesi, sonraki program metninin derlenmesi sırasında tüm veya belirli bir uyarı iletisi kümesini devre dışı bırakmak veya geri yüklemek için kullanılır.

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

Uyarı listesini atlayacak bir `#pragma warning` yönergesi tüm uyarıları etkiler. Uyarı listesi içeren `#pragma warning` yönergesi yalnızca listede belirtilen uyarıları etkiler.

`#pragma warning disable` yönergesi, tüm veya verilen uyarı kümesini devre dışı bırakır.

`#pragma warning restore` yönergesi, derleme biriminin başlangıcında geçerli olan durum kümesini veya verilen uyarıları geri yükler. Belirli bir uyarı dışarıdan devre dışı bırakılmışsa, bir `#pragma warning restore` (tümü veya belirli bir uyarı için) bu uyarıyı yeniden etkinleştiremeyeceğini unutmayın.

Aşağıdaki örnek, kullanımdan çıkarıldığı zaman, Microsoft C# derleyicisinden alınan uyarı numarası kullanılarak, kullanım dışı bırakılmış üyelere başvuruluyorsa bildirilen uyarıyı geçici olarak devre dışı bırakmak için `#pragma warning` kullanımını gösterir.
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
