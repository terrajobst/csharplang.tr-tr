---
ms.openlocfilehash: 5fbe0267b5b33b1a24dbdca493d118c576092573
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876918"
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

Sözcük temelli C# dilbilgisi, [sözcük temelli analiz](lexical-structure.md#lexical-analysis), [belirteçler](lexical-structure.md#tokens)ve [önceden işleme yönergeleriyle](lexical-structure.md#pre-processing-directives)sunulmaktadır. Sözlü dilbilgisinin Terminal sembolleri Unicode karakter kümesinin karakterleridir ve sözlü dilbilgisi karakterleri, belirteçlerin ([belirteçler](lexical-structure.md#tokens)), boşluk ([beyaz boşluk](lexical-structure.md#white-space)), yorumların ([açıklamaların](lexical-structure.md#comments)) ve ön işleme yönergeleri ([ön işleme yönergeleri](lexical-structure.md#pre-processing-directives)).

Bir C# programdaki her kaynak dosya, sözlü dilbilgisinde ([sözlü analiz](lexical-structure.md#lexical-analysis)) gelen *giriş* üretimine uygun olmalıdır.

### <a name="syntactic-grammar"></a>Sözdizimsel dilbilgisi

' Nin C# sözdizimsel dilbilgisi, bu bölümü izleyen bölümlerde ve appendıces ' de sunulur. Sözdizimsel dilbilgisinde Terminal sembolleri, sözlü dilbilgisi tarafından tanımlanan belirteçlerdir ve sözdizimsel dilbilgisi belirteçleri, belirteçlerin form C# programlarına nasıl birleştirildiğini belirtir.

Bir C# programdaki her kaynak dosya, sözdizimi dilbilgisinde ([derleme birimleri](namespaces.md#compilation-units)) *compilation_unit* üretimine uygun olmalıdır.

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

Beş temel öğe, C# kaynak dosyanın sözlü yapısını yapar: Satır sonlandırıcılar ([satır sonlandırıcılar](lexical-structure.md#line-terminators)), boşluk ([beyaz boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamalar](lexical-structure.md#comments)), belirteçler ([belirteçler](lexical-structure.md#tokens)) ve önceden işleme yönergeleri ([ön işleme yönergeleri](lexical-structure.md#pre-processing-directives)). Bu temel öğelerin yalnızca belirteçleri, bir C# programın sözdizimsel dilbilgisinde ([sözdizimsel dilbilgisi](lexical-structure.md#syntactic-grammar)) önemlidir.

Bir C# kaynak dosyanın sözcük işleme, dosyayı sözdizimsel Analize Giriş haline gelen bir belirteç dizisine düşürmektir. Satır sonlandırıcılar, boşluk ve açıklamalar ayrı belirteçlere sunabilir ve önceden işleme yönergeleri kaynak dosyanın bölümlerinin atlanmasına neden olabilir, ancak bu sözcük temelli öğelerin C# programın sözdizimsel yapısını etkilemez.

Enterpolasyonlu dize sabit değerleri ([enterpolasyonlu dize sabit değerleri](lexical-structure.md#interpolated-string-literals)), ilk olarak tek bir belirteç, sözcük temelli analiz tarafından üretilir, ancak her türlü dize sabit değerleri çözüldü. Elde edilen belirteçler daha sonra sözdizimsel Analize giriş olarak görev yapar.

Birçok sözlü dilbilgisi üretimi, bir kaynak dosyadaki bir karakter dizisiyle eşleşiyorsa, sözcük işleme her zaman mümkün olan en uzun sözlü öğeyi oluşturur. Örneğin, sözcük sırası `//` tek satırlık açıklamanın başlangıcında işlenir çünkü bu, sözlü öğe tek `/` bir belirteçten daha uzun.

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
*  Kaynak dosya boş değilse ve kaynak`U+000D`dosyanın son karakteri bir satır başı (`U+000D`), bir satır akışı (`U+000A`), satır ayırıcı`U+2028`()değilse,kaynakdosyanınsonunabirsatırbaşıkarakteri()eklenir.) ya da bir paragraf ayırıcı (`U+2029`).

### <a name="comments"></a>Açıklamalar

İki yorum biçimi desteklenir: tek satırlık açıklamalar ve sınırlandırılmış açıklamalar. ***Tek satır açıklamaları*** karakterlerle `//` başlar ve kaynak satırın sonuna kadar genişler. ***Sınırlandırılmış açıklamalar*** karakterlerle `/*` başlar ve karakterlerle `*/`biter. Sınırlandırılmış açıklamalar birden çok satıra yayılabilir.

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

Açıklamalar iç içe değildir. `/*` Karakter dizileri `*/` ve bir `//` `/*` yorum içinde özel anlamı yoktur ve karakter dizileri ve ayrılmış bir açıklama içinde özel bir anlamı yoktur. `//`

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

Birden çok çeviri gerçekleştirilmez. Örneğin, "" dize değişmez değeri`\u005Cu005C`"" yerine`\`"`\u005C`" ile eşdeğerdir. Unicode değeri `\u005C` "`\`" karakteridir.

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
`\u0066`"`f`" harfi için kaçış sırası olan, öğesinin birkaç kullanımını gösterir. Program şu şekilde eşdeğerdir
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

Bu bölümde verilen tanımlayıcılara yönelik kurallar tam olarak Unicode standart ek 31 tarafından Önerilenlere karşılık gelir, ancak alt çizgi bir başlangıç karakteri olarak izin verilir (C programlama dilinde geleneksel gibi), Unicode kaçış dizileri tanımlayıcılara izin verilir ve "`@`" karakterine, tanımlayıcı olarak kullanılacak anahtar kelimelerin etkinleştirilmesi için önek olarak izin verilir.

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

Geçerli tanımlayıcıların örnekleri şunlardır "`identifier1`", "`_identifier2`" ve "`@if`".

Uyumlu bir programdaki tanımlayıcı, Unicode standart ek 15 tarafından tanımlanan, Unicode normalleştirme biçimi C tarafından tanımlanan kurallı biçimde olmalıdır. Normalleştirme biçimi C 'de olmayan bir tanımlayıcı ile karşılaştığın davranışı uygulama tanımlı; Ancak, bir tanılama gerekli değildir.

"`@`" Ön eki, diğer programlama dilleriyle arabirim oluştururken yararlı olan tanımlayıcı olarak anahtar kelimelerin kullanılmasını sağlar. Karakter `@` aslında tanımlayıcının bir parçası değildir, bu nedenle tanımlayıcı, ön ek olmadan diğer dillerde normal tanımlayıcı olarak görülemeyebilir. `@` Ön eke sahip bir tanımlayıcıya, tam ***tanımlayıcı***denir. Anahtar sözcük olmayan tanımlayıcılar için önekkullanılmasınaizinverilir,ancakstilaçısındankesinlikleönerilmez.`@`

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
"" adlı bir parametre`class``bool`alan ""`static`adlı statik bir yöntemle "" adlı bir sınıf tanımlar. Anahtar kelimelerinde Unicode kaçışlara izin verilmemesine, "`cl\u0061ss`" belirtecinin bir tanımlayıcı olduğuna ve "`@class`" ile aynı tanımlayıcıda olduğuna unutmayın.

İki tanımlayıcı, aşağıdaki dönüşümler uygulandıktan sonra aynı olursa, sırasıyla aynı kabul edilir:

*  "`@`" Kullanılırsa, ' "öneki kaldırılır.
*  Her *unicode_escape_sequence* karşılık gelen Unicode karakteriyle dönüştürülür.
*  Herhangi bir *formatting_character*s kaldırılır.

Art arda iki alt çizgi karakteri (`U+005F`) içeren tanımlayıcılar, uygulama tarafından kullanılmak üzere ayrılmıştır. Örneğin, bir uygulama iki alt çizgi ile başlayan genişletilmiş anahtar sözcükler sağlayabilir.

### <a name="keywords"></a>anahtar sözcükler

***Anahtar sözcüğü*** , ayrılmış karakterlerin tanımlayıcı benzeri bir dizidir ve `@` karakter tarafından ön çıkması dışında bir tanımlayıcı olarak kullanılamaz.

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

Dilbilgisinde bazı yerlerde, belirli tanımlayıcıların özel anlamları vardır ancak anahtar sözcük değildir. Bu tür tanımlayıcılar bazen "bağlamsal anahtar sözcükler" olarak adlandırılır. Örneğin, bir özellik bildiriminde, "`get`" ve "`set`" tanımlayıcıları özel anlamlara ([erişimciler](classes.md#accessors)) sahiptir. Bu konumlarda `get` veya `set` dışında bir tanımlayıcıya izin verilmez, bu nedenle bu kullanım bu sözcüklerin tanımlayıcı olarak kullanımıyla çakışmaz. Örneğin, örtük olarak yazılan yerel değişken bildirimlerinde (`var`[yerel değişken bildirimleri](statements.md#local-variable-declarations)) "" tanımlayıcısına sahip gibi, bir bağlamsal anahtar sözcük, belirtilen adlarla çakışabilir. Bu gibi durumlarda, belirtilen ad tanımlayıcı anahtar sözcük olarak tanımlayıcı kullanılarak önceliklidir.

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

İki Boolean değişmez değer vardır: `true` ve. `false`

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

Bir *boolean_literal* `bool`türü.

#### <a name="integer-literals"></a>Tamsayı sabit değerleri

Tamsayı `int`sabit değerleri `uint` ,`long`, ve`ulong`türlerinin değerlerini yazmak için kullanılır. Tamsayı sabit değerlerinde iki olası biçim vardır: Decimal ve onaltılı.

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

*  Değişmez değerin soneki yoksa, bu türlerin değeri temsil edilebilir `int`:, `uint`, `long`, `ulong`.
*  Sabit `U` değer veya `u`tarafından sonekli ise, değeri gösterilebileceği bu türlerin birincileriyle sahiptir: `uint`, `ulong`.
*  Sabit `L` değer veya `l`tarafından sonekli ise, değeri gösterilebileceği bu türlerin birincileriyle sahiptir: `long`, `ulong`.
*  `UL`Sabit değer ,`LU` `ulong`,,,,, veya`lu`tarafından sonekli ise, türüdür. `Ul` `uL` `ul` `Lu` `lU`

Bir tamsayı değişmez değeri ile temsil edilen değer, `ulong` tür aralığının dışındaysa, bir derleme zamanı hatası oluşur.

`L`Stil olarak "" harfine`1``l` `long`"`l`" basamağıyla karışmak kolay olduğundan "" yerine "" yerine "" kullanılması önerilir.

En küçük `int` ve `long` değerlerin ondalık tamsayı değişmez değer olarak yazılmasına izin vermek için aşağıdaki iki kural bulunur:

* 2147483648 (2 ^ 31) değerine sahip bir *decimal_integer_literal* ve tek bir birli eksi işleç belirtecini ([birli eksi işleci](expressions.md#unary-minus-operator)) izleyen belirteç olarak hiçbir *integer_type_suffix* belirdiğinde, sonuç türünde `int`birsabittirdeğer-2147483648 (-2 ^ 31). Diğer tüm durumlarda, bu tür bir *decimal_integer_literal* türüdür `uint`.
* 9223372036854775808 (2 ^ 63) değerine sahip bir *decimal_integer_literal* ve *integer_type_suffix* veya *integer_type_suffix* `L` olmadığında ya `l` da tek bir birli eksi sonrasında belirteç olarak göründüğünde işleç belirteci ([birli eksi işleci](expressions.md#unary-minus-operator)), sonuç,-9223372036854775808 (-2 `long` ^ 63) değerine sahip bir sabit değerdir. Diğer tüm durumlarda, bu tür bir *decimal_integer_literal* türüdür `ulong`.

#### <a name="real-literals"></a>Gerçek sabit değerler

Gerçek sabit değerler, `float` `double`ve `decimal`türlerinin değerlerini yazmak için kullanılır.

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

*Real_type_suffix* belirtilmemişse, gerçek değişmez değerin `double`türü. Aksi halde gerçek tür soneki gerçek değişmez değerin türünü aşağıdaki gibi belirler:

*  `F` Ya da`f` türünde`float`olan gerçek bir sabit değer. Örneğin,,, ve `1f` `1.5f` `1e10f`değişmezdeğerleritüm türündedir `float`. `123.456F`
*  `D` Ya da`d` türünde`double`olan gerçek bir sabit değer. Örneğin,,, ve `1d` `1.5d` `1e10d`değişmezdeğerleritüm türündedir `double`. `123.456D`
*  `M` Ya da`m` türünde`decimal`olan gerçek bir sabit değer. Örneğin,,, ve `1m` `1.5m` `1e10m`değişmezdeğerleritüm türündedir `decimal`. `123.456M` Bu değişmez değer, tam değer `decimal` alınarak bir değere dönüştürülür ve gerekirse, banker 'in yuvarlanması ([ondalık türü](types.md#the-decimal-type)) kullanılarak en yakın gösterilebilir tablo değerine yuvarlanır. Değer yuvarlanmamışsa veya değer sıfır değilse (ikinci durumda, işaret ve ölçek 0 olacak şekilde), sabit değerde görünen herhangi bir ölçek korunur. Bu nedenle, değişmez `2.900m` değer, işaret `0`, katsayı `2900`ve Ölçekle `3`ondalık değerini oluşturacak şekilde ayrıştırılacaktır.

Belirtilen sabit değer belirtilen türde temsil edilemez, derleme zamanı hatası oluşur.

`float` Veya`double` türünde gerçek bir sabit değerin değeri, IEEE "en yakına yuvarla" modu kullanılarak belirlenir.

Gerçek bir sabit değerde, ondalık ayırıcıdan sonra her zaman ondalık basamakların gerekli olduğunu unutmayın. Örneğin, `1.3F` gerçek bir değişmez değerdir ancak `1.F` değildir.

#### <a name="character-literals"></a>Karakter sabit değerleri

Bir karakter sabiti tek bir karakteri temsil eder ve genellikle içinde `'a'`olduğu gibi tırnak içindeki bir karakterden oluşur.

Not: ANTLR dilbilgisi gösterimi aşağıdaki kafa karıştırıcı! ANTLR 'de yazdığınızda `\'` , tek bir teklife `'`göre gelir. Yazdığınızda `\\` , tek bir ters eğik çizgi `\`için temsil eder. Bu nedenle, bir karakter sabit değeri için ilk kural tek bir teklifle, ardından bir karakterle ve tek bir teklifle başladığı anlamına gelir. Ve olası basit `\'`kaçış dizileri ,`\a` ,,`\b`,,, ,,`\n`, ,`\t`,,,,, `\r` `\f` `\"` `\\` `\0` `\v`.

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

Bir karakter içindeki ters eğik çizgi karakteri (`\`) izleyen bir karakter şu karakterlerden biri olmalıdır `'` `0`:, `"`, `\`,, `a`,, `f` `b` , `n`, `r`, `t`, `u`, `U`, `x`, `v`. Aksi takdirde, bir derleme zamanı hatası oluşur.

Onaltılı kaçış sırası, "`\x`" öğesinden sonra onaltılık sayı tarafından oluşturulan değeri Içeren tek bir Unicode karakteri temsil eder.

Bir karakter sabiti ile temsil edilen değer değerinden `U+FFFF`büyükse, bir derleme zamanı hatası oluşur.

Bir karakter sabit değerinde Unicode karakter kaçış sırası ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)), aralığında `U+0000` `U+FFFF`olmalıdır.

Basit bir kaçış dizisi, aşağıdaki tabloda açıklandığı gibi bir Unicode karakter kodlamasını temsil eder.


| __Kaçış sırası__ | __Karakter adı__ | __Unicode kodlaması__ |
|---------------------|--------------------|----------------------|
| `\'`                | Tek tırnak       | `0x0027`             | 
| `\"`                | Çift tırnak       | `0x0022`             | 
| `\\`| Ters eğik çizgi |`0x005C`             | 
| `\0`                | Null               | `0x0000`             | 
| `\a`                | Uyarı              | `0x0007`             | 
| `\b`                | Geri Al tuşu          | `0x0008`             | 
| `\f`                | Form akışı          | `0x000C`             | 
| `\n`                | Yeni satır           | `0x000A`             | 
| `\r`                | Satır başı    | `0x000D`             | 
| `\t`                | Yatay sekme     | `0x0009`             | 
| `\v`                | Dikey sekme       | `0x000B`             | 

Bir *character_literal* `char`türü.

#### <a name="string-literals"></a>Dize sabit değerleri

C#dize sabit değerlerinin iki biçimini destekler: ***normal dize sabit değerleri*** ve tam ***dize sabit değerleri***.

Normal bir dize sabit değeri, içinde `"hello"`olduğu gibi çift tırnak içine alınmış sıfır veya daha fazla karakterden oluşur ve hem basit kaçış dizilerini ( `\t` tab karakteri için gibi), onaltılı ve Unicode kaçış dizilerini içerebilir.

Tam dize değişmez değeri, bir `@` karakteri ve ardından çift tırnak karakteri, sıfır veya daha fazla karakter ve bir kapanış çift tırnak karakteriyle oluşur. Basit bir örnek vardır `@"hello"`. Tam bir dize sabit değerinde, sınırlayıcılar arasındaki karakterler tam olarak yorumlanır ve tek özel durum bir *quote_escape_sequence*. Özellikle, basit kaçış dizileri ve onaltılı ve Unicode kaçış dizileri, tam dize değişmez değerlerinde işlenmez. Tam dize değişmez değeri birden çok satıra yayılabilir.

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

Bir regular_string_literal_character içinde ters eğik çizgi karakteri (`\`) izleyen bir karakter şu karakterlerden biri olmalıdır `'` `0`:, `"`, `\`,,, `b` `a` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`. Aksi takdirde, bir derleme zamanı hatası oluşur.

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
çeşitli dize sabit değerlerini gösterir. Son dize değişmez değeri `j`, birden çok satıra yayılan, tam bir dize sabit değeri. Yeni satır karakterleri gibi boşluk da dahil olmak üzere tırnak işaretleri arasındaki karakterler tam olarak korunur.

Bir onaltılık kaçış sırası, dizi onaltılık basamağa sahip olabilir, dize sabiti `"\x123"` 123 onaltılık değeri olan tek bir karakter içerir. Onaltılık değeri 12 ve ardından karakter 3 olan karakteri içeren bir dize oluşturmak için, bir tane yazabilir `"\x00123"` veya `"\x12" + "3"` bunun yerine.

Bir *string_literaL* `string`türü.

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
, `True` iki sabit değer aynı dize örneğine başvurduğundan.

#### <a name="interpolated-string-literals"></a>Ara değerli dize sabit değerleri

Enterpolasyonlu dize sabit değerleri dize sabit değerlerine benzerdir, ancak deyimlerde `}`olduğu gibi `{` ve ile ayrılmış olan delikleri içerir. Çalışma zamanında, ifadeler, metin biçimlerinin, delik oluşması durumunda dize üzerinde yer aldığı amaçla değerlendirilir. Dize ilişkilendirtiğinin sözdizimi ve semantiği bölümünde açıklanmaktadır ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)).

Dize sabit değerleri gibi, enterpolasyonlu dize değişmez değerleri normal veya tam olabilir. Enterpolasyonlu normal dize sabit `$"` değerleri `"`ve ile sınırlandırılır ve enterpolasyonlu değişmez `$@"` dize `"`değerleri ve ile sınırlandırılmıştır.

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

*İnterpolated_string_literal* belirteci, *interpolated_string_literal*içindeki oluşum sırasıyla birden fazla belirteç ve diğer girdi öğeleri olarak yeniden yorumlanır:

* Aşağıdakilerin oluşumları ayrı ayrı belirteçler olarak yeniden yorumlanır `$` : önde gelen işareti, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* ve *interpolated_verbatim_string_end*.
* Bunlar arasındaki *regular_balanced_text* ve *verbatim_balanced_text* örnekleri, bir *input_section* ([sözcük temelli analiz](lexical-structure.md#lexical-analysis)) olarak yeniden işlenir ve giriş öğelerinin ortaya çıkan sırası olarak yeniden yorumlanır. Bunlar, yeniden yorumlanan enterpolasyonlu dize sabit belirteçlerini içerebilir.

Sözdizimsel analiz, belirteçleri bir *interpolated_string_expression* ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)) olarak yeniden birleştirir.

Örnek TODO


#### <a name="the-null-literal"></a>Null değişmez değeri

```antlr
null_literal
    : 'null'
    ;
```

*Null_literal* örtük olarak bir başvuru türüne veya null yapılabilir türe dönüştürülebilir.

### <a name="operators-and-punctuators"></a>İşleçler ve noktaleyiciler

Birkaç tür işleç ve noktalama vardır. İşleçler, bir veya daha fazla işlenen ile ilgili işlemleri betimleyen ifadelerde kullanılır. Örneğin, ifadesi `a + b` iki `a` işleneni ve ' `+` `b`i eklemek için işlecini kullanır. Noktaleyiciler gruplandırma ve ayırma içindir.

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

*Right_shift* ve *right_shift_assignment* üretimlerinin dikey çubuğu, sözdizimsel dilbilgisinde diğer üretimlerin aksine, belirteçler arasında herhangi bir türde (çift boşluk değil) bir karakter yapılmasına izin verilmediğini belirtmek için kullanılır. Bu üretimler, *type_parameter_list*s 'nin doğru işlemesini etkinleştirmek için özel olarak değerlendirilir ([tür parametreleri](classes.md#type-parameters)).

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

*  `#define`ve `#undef`sırasıyla, koşullu derleme sembollerini ([bildirim yönergeleri](lexical-structure.md#declaration-directives)) tanımlamak ve bunları kaldırmak için kullanılır.
*  `#if`, `#elif`, ve`#endif`, kaynak kodu bölümlerini koşullu olarak atlamak için kullanılır ([koşullu derleme yönergeleri](lexical-structure.md#conditional-compilation-directives)). `#else`
*  `#line`, hatalar ve uyarılar için oluşturulan satır numaralarını denetlemek için kullanılan ([satır yönergeleri](lexical-structure.md#line-directives)).
*  `#error`ve `#warning`sırasıyla hata ve uyarı vermek için kullanılır ([tanı yönergeleri](lexical-structure.md#diagnostic-directives)).
*  `#region`Kaynak kodun ([bölge yönergeleri](lexical-structure.md#region-directives)) bölümlerini açıkça işaretlemek için de kullanılır. `#endregion`
*  `#pragma`derleyici için isteğe bağlı bağlamsal bilgileri belirtmek için kullanılan ([pragma yönergeleri](lexical-structure.md#pragma-directives)).

Bir ön işleme yönergesi her zaman ayrı bir kaynak kod satırı kaplar ve her zaman bir karakter ve `#` bir ön işleme yönergesi adıyla başlar. `#` Karakterden önce boşluk, karakter ve yönerge adı `#` arasında boşluk oluşabilir.

`#define` `#else` `#elif` `#undef` ,,`#endif`,,,, ,Veya`#endregion` yönergesini içeren bir kaynak satırı, teksatırlıkbiraçıklamailesonabaşlayabilir.`#if` `#line` Önceden işleme yönergelerini içeren `/* */` kaynak satırlarda sınırlandırılmış açıklamalara (yorumların stili) izin verilmez.

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

`#if`,, Ve `#else` `#elif` yönergeleri`#endif` tarafından sunulan koşullu derleme işlevselliği, ön işleme ifadeleri ([ön işleme ifadeleri](lexical-structure.md#pre-processing-expressions)) ve koşullu olarak denetlenir derleme sembolleri.

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

Koşullu derleme sembolünün iki olası durumu vardır: ***tanımlı*** veya ***tanımsız***. Bir kaynak dosyanın sözcük işleme başlangıcında, bir dış mekanizma (bir komut satırı derleyici seçeneği gibi) tarafından açıkça tanımlanmadığı sürece koşullu derleme simgesi tanımsızdır. Bir `#define` yönerge işlendiğinde, Bu yönergede adlı koşullu derleme sembolü bu kaynak dosyada tanımlanır. Sembol, aynı simgenin bir `#undef` yönergesi işlenene veya kaynak dosyanın sonuna ulaşılana kadar tanımlanmış olarak kalır. `#define` Bunun`#undef` bir etkisi, bir kaynak dosyasındaki yönergelerin aynı programdaki diğer kaynak dosyaları üzerinde hiçbir etkisi olmaması.

Bir ön işleme ifadesinde başvurulmadığında, tanımlı bir koşullu derleme sembolünün Boole değeri `true`vardır ve tanımsız bir koşullu derleme sembolü Boolean değerine `false`sahiptir. Koşullu derleme simgelerinin, ön işleme ifadelerinde başvurulmadan önce açıkça bildirilmesini gerektiren bir gereksinim yoktur. Bunun yerine, bildirilmemiş semboller yalnızca tanımsız olur ve bu nedenle değeri `false`vardır.

Koşullu derleme sembolleri için ad alanı farklıdır ve bir C# programdaki diğer tüm adlandırılmış varlıklardan ayrıdır. Koşullu derleme sembollerine yalnızca `#define` ve `#undef` yönergeleriyle ve önceden işleme ifadelerinde başvurulabilir.

### <a name="pre-processing-expressions"></a>Ön işleme ifadeleri

Ön işleme ifadeleri, ve `#if` `#elif` yönergeleri içinde oluşabilir. ,, `!` `!=` `==` Ve`||` işleçlerine ön işleme ifadelerinde izin verilir ve gruplama için parantezler kullanılabilir. `&&`

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

Bir ön işleme ifadesinde başvurulmadığında, tanımlı bir koşullu derleme sembolünün Boole değeri `true`vardır ve tanımsız bir koşullu derleme sembolü Boolean değerine `false`sahiptir.

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

Bir `#define` yönergeyi işleme, belirtilen koşullu derleme sembolünün, yönergeyi izleyen kaynak satırla başlayarak tanımlanmasını sağlar. Benzer şekilde, bir `#undef` yönergenin işlenmesi verilen koşullu derleme sembolünün, yönergeyi izleyen kaynak satırla başlayarak tanımsız hale gelmesine neden olur.

Kaynak `#define` dosyadaki `#undef` tüm ve yönergelerinin kaynak dosyadaki ilk *belirteçten* ([belirteçler](lexical-structure.md#tokens)) önce gerçekleşmesi gerekir; Aksi takdirde derleme zamanı hatası oluşur. Sezgisel koşullarda `#define` ve `#undef` yönergeler kaynak dosyadaki tüm "gerçek koddan" gelmelidir.

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
, kaynak dosyadaki ilk `#define` belirteçten `namespace` (anahtar sözcüğü) önce gelen yönergeler için geçerlidir.

Aşağıdaki örnek, bir derleme zamanı hatasına neden olur çünkü bu, `#define` gerçek bir kod izler:
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

, `#define` Zaten tanımlanmış olan bir koşullu derleme sembolünü tanımlayabilir, bu sembol için herhangi bir araya `#undef` girmeden emin olamaz. Aşağıdaki örnek bir koşullu derleme sembolünü `A` tanımlar ve sonra yeniden tanımlar.
```csharp
#define A
#define A
```

`#undef` Tanımlı olmayan bir koşullu derleme sembolünü "tanımlayabilir". Aşağıdaki örnek bir koşullu derleme sembolünü `A` tanımlar ve iki kez tanımlamaz; ikincisi `#undef` hiçbir etkiye sahip olmasa da, hala geçerli olur.
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

Sözdizimi tarafından gösterildiği gibi, koşullu derleme yönergeleri `#if` , sırasıyla, bir yönerge, sıfır veya daha fazla `#elif` yönerge, sıfır veya bir `#else` yönerge ve bir `#endif` yönerge olarak yazılmış şekilde yazılmalıdır. Yönergeler arasında kaynak kodunun koşullu bölümleri bulunur. Her bölüm hemen önceki yönerge tarafından denetlenir. Koşullu bir bölümün kendisi bu yönergeler için tüm kümeler için iç içe geçmiş koşullu derleme yönergeleri içerebilir.

Bir *pp_conditional* , normal sözcük işleme için kapsanan *conditional_section*s 'nin en çok birini seçer:

*  `#if` `true` Ve`#elif` yönergelerinin pp_expression s, bir alınana kadar sırayla değerlendirilir. Bir ifade `true`varsa, karşılık gelen yönergesinin *conditional_section* seçilidir.
*  Tüm *pp_expression*s `false`yield varsa ve bir `#else` yönerge varsa, `#else` yönergesinin *conditional_section* seçilidir.
*  Aksi takdirde, hiçbir *conditional_section* seçili değildir.

Seçili *conditional_section*, varsa normal *input_section*olarak işlenir: bölümünde yer alan kaynak kodu, sözlü dilbilgisine uymalıdır; belirteçler, bölümündeki kaynak koddan oluşturulur; ve bölümündeki ön işleme yönergelerinin tanımlanmış etkileri vardır.

Kalan *conditional_section*s, varsa, *skipped_section*s olarak işlenir: ön işleme yönergeleri haricinde, bölümdeki kaynak kodu sözlü dilbilgisine bağlı kalmamalıdır; bölümünde kaynak koddan hiçbir belirteç oluşturulmaz; ve bölümündeki ön işleme yönergeleri, sözcüksel doğru olmalıdır ancak başka türlü işlenmemelidir. Bir *skipped_section*olarak işlenmekte olan bir *conditional_section* içinde, iç içe geçmiş *conditional_section*s (iç içe `#if`geçmiş... içinde bulunur. `#endif` ve`#region`... yapılar) de skipped_section s olarak işlenir. `#endregion`

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

Ön işleme yönergeleri hariç, atlanan kaynak kodu sözcük temelli analize tabi değildir. Örneğin, aşağıdaki, `#else` bölümünde Sonlandırılmamış açıklamaya rağmen geçerlidir:
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
```
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
`class` , tanımlanmadığına `{` `}` `Q` bakılmaksızınherzamanaynıbelirteçakışını`X` () üretir. Tanımlı ise, çok satırlı yorum nedeniyle işlenen tek yönergeler `#endif`ve olur `#if`. `X` Tanımlanmamışsa, üç yönerge (`#if`, `#else`, `#endif`) yönerge kümesinin bir parçasıdır. `X`

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
her zaman bir uyarı üretir ("iade etmeden önce kod incelemesi gereklidir") ve koşullu semboller `Debug` ve `Retail` ikisi de tanımlanmışsa bir derleme zamanı hatası ("derleme hem hata ayıklama hem de perakende olamaz") oluşturur. Bir *pp_message* , rastgele metin içerebileceğini unutmayın; Özellikle, sözcükteki `can't`tek tırnak içinde gösterildiği gibi iyi biçimlendirilmiş belirteçler içermesi gerekmez.

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

Bir bölgeye hiçbir anlam anlamı eklenmez; bölgeler, programcı tarafından veya kaynak kodun bir bölümünü işaretlemek için otomatikleştirilmiş araçlar tarafından kullanılmak üzere tasarlanmıştır. Aynı şekilde `#region` veya `#endregion` yönergesinde belirtilen iletide anlam anlamı yoktur; yalnızca bölgeyi tanımlamak için kullanılır. Eşleştirme `#region` ve`#endregion` yönergelerin farklı *pp_message*s 'leri olabilir.

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

Hiçbir `#line` yönergesi yoksa, derleyici çıktıda gerçek satır numaralarını ve kaynak dosya adlarını raporlar. Olmayan `#line` bir`default` *line_indicator* içeren bir yönergeyi işlerken, derleyici, belirtilen satır numarası (ve belirtilmişse dosya adı) gibi yönergeden sonra satırı değerlendirir.

Bir `#line default` yönerge, önceki tüm #line yönergelerinin etkisini tersine çevirir. Derleyici, kesin olmayan `#line` bir yönergeler gibi, izleyen satırlar için gerçek satır bilgilerini raporlar.

Bir `#line hidden` yönergenin hata iletilerinde bildirilen dosya ve satır numaraları üzerinde hiçbir etkisi yoktur, ancak kaynak düzeyinde hata ayıklamayı etkiler. Hata ayıklarken, bir `#line hidden` yönergeyle sonraki `#line` `#line hidden`yönerge (olmayan) arasındaki tüm satırlarda satır numarası bilgisi yoktur. Hata ayıklayıcıda kod üzerinden adımla, bu satırlar tamamen atlanır.

Bir *dosya_adı* , kaçış karakterlerinin işlenmediği normal bir dize sabit değerinden farklı olduğunu unutmayın; "`\`" karakteri, bir *dosya_adı*içindeki normal ters eğik çizgi karakterini belirtir.

### <a name="pragma-directives"></a>Pragma yönergeleri

`#pragma` Ön işleme yönergesi, isteğe bağlı bağlamsal bilgileri derleyiciye belirtmek için kullanılır. Bir `#pragma` yönergede sağlanan bilgiler hiçbir şekilde program semantiğini değiştirmez.

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

C#derleyici `#pragma` uyarılarını denetlemek için yönergeler sağlar. Dilin gelecekteki sürümleri ek `#pragma` yönergeler içerebilir. Diğer C# derleyicilerle birlikte çalışabilirliği sağlamak için, Microsoft C# derleyicisi bilinmeyen `#pragma` yönergeler için derleme hataları vermez; bu yönergeler, ancak uyarılar oluşturur.

#### <a name="pragma-warning"></a>pragma uyarısı

`#pragma warning` Yönerge, sonraki program metninin derlenmesi sırasında tüm veya belirli bir uyarı iletisi kümesini devre dışı bırakmak veya geri yüklemek için kullanılır.

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

Uyarı `#pragma warning` listesini atlayacak bir yönerge tüm uyarıları etkiler. Uyarı `#pragma warning` listesi içeren bir yönerge yalnızca listede belirtilen uyarıları etkiler.

Bir `#pragma warning disable` yönerge, tüm veya verilen uyarı kümesini devre dışı bırakır.

Bir `#pragma warning restore` yönerge, derleme biriminin başlangıcında geçerli olan durum kümesini veya verilen uyarıları geri yükler. Belirli bir uyarı dışarıdan devre dışı bırakılmışsa, bir (tümü `#pragma warning restore` veya belirli bir uyarı için) bu uyarıyı yeniden etkinleştiremeyeceğini unutmayın.

Aşağıdaki örnek, kullanım dışı bırakılmış `#pragma warning` üyelere, Microsoft C# derleyicisinden gelen uyarı numarasını kullanarak başvurulduğunu bildiren uyarıyı geçici olarak devre dışı bırakmak için öğesinin kullanımını gösterir.
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
