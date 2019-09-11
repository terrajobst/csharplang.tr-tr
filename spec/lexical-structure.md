---
ms.openlocfilehash: 5fbe0267b5b33b1a24dbdca493d118c576092573
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876918"
---
# <a name="lexical-structure"></a><span data-ttu-id="c6437-101">Sözcük yapısı</span><span class="sxs-lookup"><span data-stu-id="c6437-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="c6437-102">Programlar</span><span class="sxs-lookup"><span data-stu-id="c6437-102">Programs</span></span>

<span data-ttu-id="c6437-103">C# ***Program*** , bilinen bir veya daha fazla ***kaynak dosyadan*** ***oluşur (derleme birimleri)*** .[](namespaces.md#compilation-units)</span><span class="sxs-lookup"><span data-stu-id="c6437-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="c6437-104">Kaynak dosya, bir dizi Unicode karakterden oluşan sıralı bir dizidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="c6437-105">Kaynak dosyaların bir dosya sistemindeki dosyalarla genellikle bire bir yazışmaları vardır, ancak bu yazışma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="c6437-106">En fazla taşınabilirlik için, bir dosya sistemindeki dosyaların UTF-8 kodlaması ile kodlanması önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="c6437-107">Kavramsal olarak bakıldığında, bir program üç adım kullanılarak derlenir:</span><span class="sxs-lookup"><span data-stu-id="c6437-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="c6437-108">Bir dosyayı belirli bir karakter topluluğunun ve kodlama düzeninden bir Unicode karakter dizisine dönüştüren dönüştürme.</span><span class="sxs-lookup"><span data-stu-id="c6437-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="c6437-109">Unicode giriş karakterlerinden oluşan bir akışı belirteç akışına çeviren sözcük temelli analiz.</span><span class="sxs-lookup"><span data-stu-id="c6437-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="c6437-110">Belirteçlerin akışını yürütülebilir koda çeviren sözdizimsel analiz.</span><span class="sxs-lookup"><span data-stu-id="c6437-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="c6437-111">Dilbilgisinde</span><span class="sxs-lookup"><span data-stu-id="c6437-111">Grammars</span></span>

<span data-ttu-id="c6437-112">Bu belirtim, C# programlama dilinin sözdizimini iki dilmars kullanarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6437-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="c6437-113">***Sözlü dilbilgisi*** ([sözlü dilbilgisi](lexical-structure.md#lexical-grammar)), Unicode karakterlerinin form satır sonlandırıcılar, beyaz boşluk, açıklama, belirteç ve ön işleme yönergeleri ile nasıl birleştirildiğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="c6437-114">***Sözdizimsel*** dilbilgisi ([sözdizimsel dilbilgisi](lexical-structure.md#syntactic-grammar)), sözcük temelli dilbilgisinde elde edilen belirteçlerin form C# programlarında nasıl birleştirildiğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="c6437-115">Dilbilgisi gösterimi</span><span class="sxs-lookup"><span data-stu-id="c6437-115">Grammar notation</span></span>

<span data-ttu-id="c6437-116">Sözlü ve sözdizimsel dilbilgisinde, ANTLR dilbilgisi aracının gösterimi kullanılarak Backus-Naur form sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c6437-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="c6437-117">Sözcük dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="c6437-117">Lexical grammar</span></span>

<span data-ttu-id="c6437-118">Sözcük temelli C# dilbilgisi, [sözcük temelli analiz](lexical-structure.md#lexical-analysis), [belirteçler](lexical-structure.md#tokens)ve [önceden işleme yönergeleriyle](lexical-structure.md#pre-processing-directives)sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c6437-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="c6437-119">Sözlü dilbilgisinin Terminal sembolleri Unicode karakter kümesinin karakterleridir ve sözlü dilbilgisi karakterleri, belirteçlerin ([belirteçler](lexical-structure.md#tokens)), boşluk ([beyaz boşluk](lexical-structure.md#white-space)), yorumların ([açıklamaların](lexical-structure.md#comments)) ve ön işleme yönergeleri ([ön işleme yönergeleri](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="c6437-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="c6437-120">Bir C# programdaki her kaynak dosya, sözlü dilbilgisinde ([sözlü analiz](lexical-structure.md#lexical-analysis)) gelen *giriş* üretimine uygun olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="c6437-121">Sözdizimsel dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="c6437-121">Syntactic grammar</span></span>

<span data-ttu-id="c6437-122">' Nin C# sözdizimsel dilbilgisi, bu bölümü izleyen bölümlerde ve appendıces ' de sunulur.</span><span class="sxs-lookup"><span data-stu-id="c6437-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="c6437-123">Sözdizimsel dilbilgisinde Terminal sembolleri, sözlü dilbilgisi tarafından tanımlanan belirteçlerdir ve sözdizimsel dilbilgisi belirteçleri, belirteçlerin form C# programlarına nasıl birleştirildiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c6437-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="c6437-124">Bir C# programdaki her kaynak dosya, sözdizimi dilbilgisinde ([derleme birimleri](namespaces.md#compilation-units)) *compilation_unit* üretimine uygun olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="c6437-125">Sözcük temelli analiz</span><span class="sxs-lookup"><span data-stu-id="c6437-125">Lexical analysis</span></span>

<span data-ttu-id="c6437-126">*Giriş* üretimi, C# kaynak dosyanın sözlü yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="c6437-127">Bir C# programdaki her kaynak dosya, bu sözcük temelli dilbilgisi üretimine uygun olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="c6437-128">Beş temel öğe, C# kaynak dosyanın sözlü yapısını yapar: Satır sonlandırıcılar ([satır sonlandırıcılar](lexical-structure.md#line-terminators)), boşluk ([beyaz boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamalar](lexical-structure.md#comments)), belirteçler ([belirteçler](lexical-structure.md#tokens)) ve önceden işleme yönergeleri ([ön işleme yönergeleri](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="c6437-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="c6437-129">Bu temel öğelerin yalnızca belirteçleri, bir C# programın sözdizimsel dilbilgisinde ([sözdizimsel dilbilgisi](lexical-structure.md#syntactic-grammar)) önemlidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="c6437-130">Bir C# kaynak dosyanın sözcük işleme, dosyayı sözdizimsel Analize Giriş haline gelen bir belirteç dizisine düşürmektir.</span><span class="sxs-lookup"><span data-stu-id="c6437-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="c6437-131">Satır sonlandırıcılar, boşluk ve açıklamalar ayrı belirteçlere sunabilir ve önceden işleme yönergeleri kaynak dosyanın bölümlerinin atlanmasına neden olabilir, ancak bu sözcük temelli öğelerin C# programın sözdizimsel yapısını etkilemez.</span><span class="sxs-lookup"><span data-stu-id="c6437-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="c6437-132">Enterpolasyonlu dize sabit değerleri ([enterpolasyonlu dize sabit değerleri](lexical-structure.md#interpolated-string-literals)), ilk olarak tek bir belirteç, sözcük temelli analiz tarafından üretilir, ancak her türlü dize sabit değerleri çözüldü.</span><span class="sxs-lookup"><span data-stu-id="c6437-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="c6437-133">Elde edilen belirteçler daha sonra sözdizimsel Analize giriş olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="c6437-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="c6437-134">Birçok sözlü dilbilgisi üretimi, bir kaynak dosyadaki bir karakter dizisiyle eşleşiyorsa, sözcük işleme her zaman mümkün olan en uzun sözlü öğeyi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6437-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="c6437-135">Örneğin, sözcük sırası `//` tek satırlık açıklamanın başlangıcında işlenir çünkü bu, sözlü öğe tek `/` bir belirteçten daha uzun.</span><span class="sxs-lookup"><span data-stu-id="c6437-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="c6437-136">Satır sonlandırıcılar</span><span class="sxs-lookup"><span data-stu-id="c6437-136">Line terminators</span></span>

<span data-ttu-id="c6437-137">Satır sonlandırıcılar, bir C# kaynak dosyanın karakterlerini çizgilere böler.</span><span class="sxs-lookup"><span data-stu-id="c6437-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="c6437-138">Dosya sonu işaretçileri ekleyen kaynak kodu düzenlemeyle uyumluluk için ve kaynak dosyanın düzgün bir şekilde sonlandırılmış satırlar halinde görüntülenmesini sağlamak için, aşağıdaki dönüşümler bir C# programdaki her kaynak dosyasına sırayla uygulanır:</span><span class="sxs-lookup"><span data-stu-id="c6437-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="c6437-139">Kaynak dosyanın son karakteri bir Control-Z karakteri (`U+001A`) ise, bu karakter silinir.</span><span class="sxs-lookup"><span data-stu-id="c6437-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="c6437-140">Kaynak dosya boş değilse ve kaynak`U+000D`dosyanın son karakteri bir satır başı (`U+000D`), bir satır akışı (`U+000A`), satır ayırıcı`U+2028`()değilse,kaynakdosyanınsonunabirsatırbaşıkarakteri()eklenir.) ya da bir paragraf ayırıcı (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="c6437-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="c6437-141">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="c6437-141">Comments</span></span>

<span data-ttu-id="c6437-142">İki yorum biçimi desteklenir: tek satırlık açıklamalar ve sınırlandırılmış açıklamalar.</span><span class="sxs-lookup"><span data-stu-id="c6437-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="c6437-143">***Tek satır açıklamaları*** karakterlerle `//` başlar ve kaynak satırın sonuna kadar genişler.</span><span class="sxs-lookup"><span data-stu-id="c6437-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="c6437-144">***Sınırlandırılmış açıklamalar*** karakterlerle `/*` başlar ve karakterlerle `*/`biter.</span><span class="sxs-lookup"><span data-stu-id="c6437-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="c6437-145">Sınırlandırılmış açıklamalar birden çok satıra yayılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="c6437-146">Açıklamalar iç içe değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-146">Comments do not nest.</span></span> <span data-ttu-id="c6437-147">`/*` Karakter dizileri `*/` ve bir `//` `/*` yorum içinde özel anlamı yoktur ve karakter dizileri ve ayrılmış bir açıklama içinde özel bir anlamı yoktur. `//`</span><span class="sxs-lookup"><span data-stu-id="c6437-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="c6437-148">Açıklamalar karakter ve dize değişmez değerleri içinde işlenmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="c6437-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="c6437-149">The example</span></span>
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
<span data-ttu-id="c6437-150">sınırlandırılmış bir açıklama içerir.</span><span class="sxs-lookup"><span data-stu-id="c6437-150">includes a delimited comment.</span></span>

<span data-ttu-id="c6437-151">Örnek</span><span class="sxs-lookup"><span data-stu-id="c6437-151">The example</span></span>
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
<span data-ttu-id="c6437-152">Birkaç tek satırlık açıklama gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6437-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="c6437-153">Boşluk</span><span class="sxs-lookup"><span data-stu-id="c6437-153">White space</span></span>

<span data-ttu-id="c6437-154">Boşluk karakteri, yatay sekme karakteri, dikey sekme karakteri ve form besleme karakteri olan Unicode sınıf ZS (boşluk karakterini de içerir) ile herhangi bir karakter olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="c6437-155">Belirteçler</span><span class="sxs-lookup"><span data-stu-id="c6437-155">Tokens</span></span>

<span data-ttu-id="c6437-156">Birkaç tür belirteç vardır: tanımlayıcılar, anahtar sözcükler, sabit değerler, işleçler ve noktalama işaretleri.</span><span class="sxs-lookup"><span data-stu-id="c6437-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="c6437-157">Boşluk ve açıklamalar belirteçler değildir, ancak belirteçler için ayırıcılar işlevi görür.</span><span class="sxs-lookup"><span data-stu-id="c6437-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="c6437-158">Unicode karakter kaçış dizileri</span><span class="sxs-lookup"><span data-stu-id="c6437-158">Unicode character escape sequences</span></span>

<span data-ttu-id="c6437-159">Unicode karakter kaçış dizisi bir Unicode karakteri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6437-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="c6437-160">Unicode karakter kaçış dizileri tanımlayıcılarda ([tanımlayıcılar](lexical-structure.md#identifiers)), karakter sabit değerlerinde ([karakter sabit](lexical-structure.md#character-literals)değerleri) ve normal dize değişmez değerlerinde ([dize sabit değerleri](lexical-structure.md#string-literals)) işlenir.</span><span class="sxs-lookup"><span data-stu-id="c6437-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="c6437-161">Unicode karakter kaçış başka bir konumda işlenmez (örneğin, bir işleç, noktalama veya anahtar sözcük oluşturmak için).</span><span class="sxs-lookup"><span data-stu-id="c6437-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="c6437-162">Unicode kaçış sırası, "`\u`" veya "`\U`" karakterlerinden sonra onaltılık sayı tarafından oluşturulan tek Unicode karakteri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6437-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="c6437-163">, C# Karakter ve dize değerlerinde Unicode kod noktalarının 16 bitlik bir kodlamasını kullandığından, bir karakter sabit değerinde u + 10000-u + 10FFFF aralığındaki bir Unicode karaktere izin verilmez ve bir dize sabit değerinde Unicode vekil çifti kullanılarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="c6437-164">0x10FFFF üzerinde kod noktaları olan Unicode karakterler desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="c6437-165">Birden çok çeviri gerçekleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-165">Multiple translations are not performed.</span></span> <span data-ttu-id="c6437-166">Örneğin, "" dize değişmez değeri`\u005Cu005C`"" yerine`\`"`\u005C`" ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="c6437-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="c6437-167">Unicode değeri `\u005C` "`\`" karakteridir.</span><span class="sxs-lookup"><span data-stu-id="c6437-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="c6437-168">Örnek</span><span class="sxs-lookup"><span data-stu-id="c6437-168">The example</span></span>
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
<span data-ttu-id="c6437-169">`\u0066`"`f`" harfi için kaçış sırası olan, öğesinin birkaç kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6437-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="c6437-170">Program şu şekilde eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="c6437-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="c6437-171">Tanımlayıcılar</span><span class="sxs-lookup"><span data-stu-id="c6437-171">Identifiers</span></span>

<span data-ttu-id="c6437-172">Bu bölümde verilen tanımlayıcılara yönelik kurallar tam olarak Unicode standart ek 31 tarafından Önerilenlere karşılık gelir, ancak alt çizgi bir başlangıç karakteri olarak izin verilir (C programlama dilinde geleneksel gibi), Unicode kaçış dizileri tanımlayıcılara izin verilir ve "`@`" karakterine, tanımlayıcı olarak kullanılacak anahtar kelimelerin etkinleştirilmesi için önek olarak izin verilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="c6437-173">Yukarıda belirtilen Unicode karakter sınıfları hakkında daha fazla bilgi için bkz. Unicode standart, sürüm 3,0, Bölüm 4,5.</span><span class="sxs-lookup"><span data-stu-id="c6437-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="c6437-174">Geçerli tanımlayıcıların örnekleri şunlardır "`identifier1`", "`_identifier2`" ve "`@if`".</span><span class="sxs-lookup"><span data-stu-id="c6437-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="c6437-175">Uyumlu bir programdaki tanımlayıcı, Unicode standart ek 15 tarafından tanımlanan, Unicode normalleştirme biçimi C tarafından tanımlanan kurallı biçimde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="c6437-176">Normalleştirme biçimi C 'de olmayan bir tanımlayıcı ile karşılaştığın davranışı uygulama tanımlı; Ancak, bir tanılama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="c6437-177">"`@`" Ön eki, diğer programlama dilleriyle arabirim oluştururken yararlı olan tanımlayıcı olarak anahtar kelimelerin kullanılmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="c6437-178">Karakter `@` aslında tanımlayıcının bir parçası değildir, bu nedenle tanımlayıcı, ön ek olmadan diğer dillerde normal tanımlayıcı olarak görülemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="c6437-179">`@` Ön eke sahip bir tanımlayıcıya, tam ***tanımlayıcı***denir.</span><span class="sxs-lookup"><span data-stu-id="c6437-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="c6437-180">Anahtar sözcük olmayan tanımlayıcılar için önekkullanılmasınaizinverilir,ancakstilaçısındankesinlikleönerilmez.`@`</span><span class="sxs-lookup"><span data-stu-id="c6437-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="c6437-181">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c6437-181">The example:</span></span>
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
<span data-ttu-id="c6437-182">"" adlı bir parametre`class``bool`alan ""`static`adlı statik bir yöntemle "" adlı bir sınıf tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="c6437-183">Anahtar kelimelerinde Unicode kaçışlara izin verilmemesine, "`cl\u0061ss`" belirtecinin bir tanımlayıcı olduğuna ve "`@class`" ile aynı tanımlayıcıda olduğuna unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6437-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="c6437-184">İki tanımlayıcı, aşağıdaki dönüşümler uygulandıktan sonra aynı olursa, sırasıyla aynı kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="c6437-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="c6437-185">"`@`" Kullanılırsa, ' "öneki kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="c6437-186">Her *unicode_escape_sequence* karşılık gelen Unicode karakteriyle dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="c6437-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="c6437-187">Herhangi bir *formatting_character*s kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="c6437-188">Art arda iki alt çizgi karakteri (`U+005F`) içeren tanımlayıcılar, uygulama tarafından kullanılmak üzere ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6437-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="c6437-189">Örneğin, bir uygulama iki alt çizgi ile başlayan genişletilmiş anahtar sözcükler sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="c6437-190">anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="c6437-190">Keywords</span></span>

<span data-ttu-id="c6437-191">***Anahtar sözcüğü*** , ayrılmış karakterlerin tanımlayıcı benzeri bir dizidir ve `@` karakter tarafından ön çıkması dışında bir tanımlayıcı olarak kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="c6437-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="c6437-192">Dilbilgisinde bazı yerlerde, belirli tanımlayıcıların özel anlamları vardır ancak anahtar sözcük değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="c6437-193">Bu tür tanımlayıcılar bazen "bağlamsal anahtar sözcükler" olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="c6437-194">Örneğin, bir özellik bildiriminde, "`get`" ve "`set`" tanımlayıcıları özel anlamlara ([erişimciler](classes.md#accessors)) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c6437-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="c6437-195">Bu konumlarda `get` veya `set` dışında bir tanımlayıcıya izin verilmez, bu nedenle bu kullanım bu sözcüklerin tanımlayıcı olarak kullanımıyla çakışmaz.</span><span class="sxs-lookup"><span data-stu-id="c6437-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="c6437-196">Örneğin, örtük olarak yazılan yerel değişken bildirimlerinde (`var`[yerel değişken bildirimleri](statements.md#local-variable-declarations)) "" tanımlayıcısına sahip gibi, bir bağlamsal anahtar sözcük, belirtilen adlarla çakışabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="c6437-197">Bu gibi durumlarda, belirtilen ad tanımlayıcı anahtar sözcük olarak tanımlayıcı kullanılarak önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="c6437-198">Sabit değerler</span><span class="sxs-lookup"><span data-stu-id="c6437-198">Literals</span></span>

<span data-ttu-id="c6437-199">***Değişmez*** değer, bir değerin kaynak kod gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="c6437-200">Boole sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="c6437-200">Boolean literals</span></span>

<span data-ttu-id="c6437-201">İki Boolean değişmez değer vardır: `true` ve. `false`</span><span class="sxs-lookup"><span data-stu-id="c6437-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="c6437-202">Bir *boolean_literal* `bool`türü.</span><span class="sxs-lookup"><span data-stu-id="c6437-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="c6437-203">Tamsayı sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="c6437-203">Integer literals</span></span>

<span data-ttu-id="c6437-204">Tamsayı `int`sabit değerleri `uint` ,`long`, ve`ulong`türlerinin değerlerini yazmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="c6437-205">Tamsayı sabit değerlerinde iki olası biçim vardır: Decimal ve onaltılı.</span><span class="sxs-lookup"><span data-stu-id="c6437-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="c6437-206">Bir tamsayı sabit değerinin türü aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="c6437-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="c6437-207">Değişmez değerin soneki yoksa, bu türlerin değeri temsil edilebilir `int`:, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="c6437-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="c6437-208">Sabit `U` değer veya `u`tarafından sonekli ise, değeri gösterilebileceği bu türlerin birincileriyle sahiptir: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="c6437-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="c6437-209">Sabit `L` değer veya `l`tarafından sonekli ise, değeri gösterilebileceği bu türlerin birincileriyle sahiptir: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="c6437-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="c6437-210">`UL`Sabit değer ,`LU` `ulong`,,,,, veya`lu`tarafından sonekli ise, türüdür. `Ul` `uL` `ul` `Lu` `lU`</span><span class="sxs-lookup"><span data-stu-id="c6437-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="c6437-211">Bir tamsayı değişmez değeri ile temsil edilen değer, `ulong` tür aralığının dışındaysa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="c6437-212">`L`Stil olarak "" harfine`1``l` `long`"`l`" basamağıyla karışmak kolay olduğundan "" yerine "" yerine "" kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="c6437-213">En küçük `int` ve `long` değerlerin ondalık tamsayı değişmez değer olarak yazılmasına izin vermek için aşağıdaki iki kural bulunur:</span><span class="sxs-lookup"><span data-stu-id="c6437-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="c6437-214">2147483648 (2 ^ 31) değerine sahip bir *decimal_integer_literal* ve tek bir birli eksi işleç belirtecini ([birli eksi işleci](expressions.md#unary-minus-operator)) izleyen belirteç olarak hiçbir *integer_type_suffix* belirdiğinde, sonuç türünde `int`birsabittirdeğer-2147483648 (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="c6437-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="c6437-215">Diğer tüm durumlarda, bu tür bir *decimal_integer_literal* türüdür `uint`.</span><span class="sxs-lookup"><span data-stu-id="c6437-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="c6437-216">9223372036854775808 (2 ^ 63) değerine sahip bir *decimal_integer_literal* ve *integer_type_suffix* veya *integer_type_suffix* `L` olmadığında ya `l` da tek bir birli eksi sonrasında belirteç olarak göründüğünde işleç belirteci ([birli eksi işleci](expressions.md#unary-minus-operator)), sonuç,-9223372036854775808 (-2 `long` ^ 63) değerine sahip bir sabit değerdir.</span><span class="sxs-lookup"><span data-stu-id="c6437-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="c6437-217">Diğer tüm durumlarda, bu tür bir *decimal_integer_literal* türüdür `ulong`.</span><span class="sxs-lookup"><span data-stu-id="c6437-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="c6437-218">Gerçek sabit değerler</span><span class="sxs-lookup"><span data-stu-id="c6437-218">Real literals</span></span>

<span data-ttu-id="c6437-219">Gerçek sabit değerler, `float` `double`ve `decimal`türlerinin değerlerini yazmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="c6437-220">*Real_type_suffix* belirtilmemişse, gerçek değişmez değerin `double`türü.</span><span class="sxs-lookup"><span data-stu-id="c6437-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="c6437-221">Aksi halde gerçek tür soneki gerçek değişmez değerin türünü aşağıdaki gibi belirler:</span><span class="sxs-lookup"><span data-stu-id="c6437-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="c6437-222">`F` Ya da`f` türünde`float`olan gerçek bir sabit değer.</span><span class="sxs-lookup"><span data-stu-id="c6437-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="c6437-223">Örneğin,,, ve `1f` `1.5f` `1e10f`değişmezdeğerleritüm türündedir `float`. `123.456F`</span><span class="sxs-lookup"><span data-stu-id="c6437-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="c6437-224">`D` Ya da`d` türünde`double`olan gerçek bir sabit değer.</span><span class="sxs-lookup"><span data-stu-id="c6437-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="c6437-225">Örneğin,,, ve `1d` `1.5d` `1e10d`değişmezdeğerleritüm türündedir `double`. `123.456D`</span><span class="sxs-lookup"><span data-stu-id="c6437-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="c6437-226">`M` Ya da`m` türünde`decimal`olan gerçek bir sabit değer.</span><span class="sxs-lookup"><span data-stu-id="c6437-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="c6437-227">Örneğin,,, ve `1m` `1.5m` `1e10m`değişmezdeğerleritüm türündedir `decimal`. `123.456M`</span><span class="sxs-lookup"><span data-stu-id="c6437-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="c6437-228">Bu değişmez değer, tam değer `decimal` alınarak bir değere dönüştürülür ve gerekirse, banker 'in yuvarlanması ([ondalık türü](types.md#the-decimal-type)) kullanılarak en yakın gösterilebilir tablo değerine yuvarlanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="c6437-229">Değer yuvarlanmamışsa veya değer sıfır değilse (ikinci durumda, işaret ve ölçek 0 olacak şekilde), sabit değerde görünen herhangi bir ölçek korunur.</span><span class="sxs-lookup"><span data-stu-id="c6437-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="c6437-230">Bu nedenle, değişmez `2.900m` değer, işaret `0`, katsayı `2900`ve Ölçekle `3`ondalık değerini oluşturacak şekilde ayrıştırılacaktır.</span><span class="sxs-lookup"><span data-stu-id="c6437-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="c6437-231">Belirtilen sabit değer belirtilen türde temsil edilemez, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="c6437-232">`float` Veya`double` türünde gerçek bir sabit değerin değeri, IEEE "en yakına yuvarla" modu kullanılarak belirlenir.</span><span class="sxs-lookup"><span data-stu-id="c6437-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="c6437-233">Gerçek bir sabit değerde, ondalık ayırıcıdan sonra her zaman ondalık basamakların gerekli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6437-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="c6437-234">Örneğin, `1.3F` gerçek bir değişmez değerdir ancak `1.F` değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="c6437-235">Karakter sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="c6437-235">Character literals</span></span>

<span data-ttu-id="c6437-236">Bir karakter sabiti tek bir karakteri temsil eder ve genellikle içinde `'a'`olduğu gibi tırnak içindeki bir karakterden oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="c6437-237">Not: ANTLR dilbilgisi gösterimi aşağıdaki kafa karıştırıcı!</span><span class="sxs-lookup"><span data-stu-id="c6437-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="c6437-238">ANTLR 'de yazdığınızda `\'` , tek bir teklife `'`göre gelir.</span><span class="sxs-lookup"><span data-stu-id="c6437-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="c6437-239">Yazdığınızda `\\` , tek bir ters eğik çizgi `\`için temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6437-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="c6437-240">Bu nedenle, bir karakter sabit değeri için ilk kural tek bir teklifle, ardından bir karakterle ve tek bir teklifle başladığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="c6437-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="c6437-241">Ve olası basit `\'`kaçış dizileri ,`\a` ,,`\b`,,, ,,`\n`, ,`\t`,,,,, `\r` `\f` `\"` `\\` `\0` `\v`.</span><span class="sxs-lookup"><span data-stu-id="c6437-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="c6437-242">Bir karakter içindeki ters eğik çizgi karakteri (`\`) izleyen bir karakter şu karakterlerden biri olmalıdır `'` `0`:, `"`, `\`,, `a`,, `f` `b` , `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="c6437-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="c6437-243">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="c6437-244">Onaltılı kaçış sırası, "`\x`" öğesinden sonra onaltılık sayı tarafından oluşturulan değeri Içeren tek bir Unicode karakteri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6437-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="c6437-245">Bir karakter sabiti ile temsil edilen değer değerinden `U+FFFF`büyükse, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="c6437-246">Bir karakter sabit değerinde Unicode karakter kaçış sırası ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)), aralığında `U+0000` `U+FFFF`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="c6437-247">Basit bir kaçış dizisi, aşağıdaki tabloda açıklandığı gibi bir Unicode karakter kodlamasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6437-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="c6437-248">__Kaçış sırası__</span><span class="sxs-lookup"><span data-stu-id="c6437-248">__Escape sequence__</span></span> | <span data-ttu-id="c6437-249">__Karakter adı__</span><span class="sxs-lookup"><span data-stu-id="c6437-249">__Character name__</span></span> | <span data-ttu-id="c6437-250">__Unicode kodlaması__</span><span class="sxs-lookup"><span data-stu-id="c6437-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="c6437-251">Tek tırnak</span><span class="sxs-lookup"><span data-stu-id="c6437-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="c6437-252">Çift tırnak</span><span class="sxs-lookup"><span data-stu-id="c6437-252">Double quote</span></span>       | `0x0022`             | 
| <span data-ttu-id="c6437-253">`\\`| Ters eğik çizgi |`0x005C`</span><span class="sxs-lookup"><span data-stu-id="c6437-253">`\\`                | Backslash          | `0x005C`</span></span>             | 
| `\0`                | <span data-ttu-id="c6437-254">Null</span><span class="sxs-lookup"><span data-stu-id="c6437-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="c6437-255">Uyarı</span><span class="sxs-lookup"><span data-stu-id="c6437-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="c6437-256">Geri Al tuşu</span><span class="sxs-lookup"><span data-stu-id="c6437-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="c6437-257">Form akışı</span><span class="sxs-lookup"><span data-stu-id="c6437-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="c6437-258">Yeni satır</span><span class="sxs-lookup"><span data-stu-id="c6437-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="c6437-259">Satır başı</span><span class="sxs-lookup"><span data-stu-id="c6437-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="c6437-260">Yatay sekme</span><span class="sxs-lookup"><span data-stu-id="c6437-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="c6437-261">Dikey sekme</span><span class="sxs-lookup"><span data-stu-id="c6437-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="c6437-262">Bir *character_literal* `char`türü.</span><span class="sxs-lookup"><span data-stu-id="c6437-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="c6437-263">Dize sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="c6437-263">String literals</span></span>

<span data-ttu-id="c6437-264">C#dize sabit değerlerinin iki biçimini destekler: ***normal dize sabit değerleri*** ve tam ***dize sabit değerleri***.</span><span class="sxs-lookup"><span data-stu-id="c6437-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="c6437-265">Normal bir dize sabit değeri, içinde `"hello"`olduğu gibi çift tırnak içine alınmış sıfır veya daha fazla karakterden oluşur ve hem basit kaçış dizilerini ( `\t` tab karakteri için gibi), onaltılı ve Unicode kaçış dizilerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="c6437-266">Tam dize değişmez değeri, bir `@` karakteri ve ardından çift tırnak karakteri, sıfır veya daha fazla karakter ve bir kapanış çift tırnak karakteriyle oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="c6437-267">Basit bir örnek vardır `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="c6437-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="c6437-268">Tam bir dize sabit değerinde, sınırlayıcılar arasındaki karakterler tam olarak yorumlanır ve tek özel durum bir *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="c6437-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="c6437-269">Özellikle, basit kaçış dizileri ve onaltılı ve Unicode kaçış dizileri, tam dize değişmez değerlerinde işlenmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="c6437-270">Tam dize değişmez değeri birden çok satıra yayılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="c6437-271">Bir regular_string_literal_character içinde ters eğik çizgi karakteri (`\`) izleyen bir karakter şu karakterlerden biri olmalıdır `'` `0`:, `"`, `\`,,, `b` `a` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="c6437-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="c6437-272">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="c6437-273">Örnek</span><span class="sxs-lookup"><span data-stu-id="c6437-273">The example</span></span>
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
<span data-ttu-id="c6437-274">çeşitli dize sabit değerlerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6437-274">shows a variety of string literals.</span></span> <span data-ttu-id="c6437-275">Son dize değişmez değeri `j`, birden çok satıra yayılan, tam bir dize sabit değeri.</span><span class="sxs-lookup"><span data-stu-id="c6437-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="c6437-276">Yeni satır karakterleri gibi boşluk da dahil olmak üzere tırnak işaretleri arasındaki karakterler tam olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="c6437-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="c6437-277">Bir onaltılık kaçış sırası, dizi onaltılık basamağa sahip olabilir, dize sabiti `"\x123"` 123 onaltılık değeri olan tek bir karakter içerir.</span><span class="sxs-lookup"><span data-stu-id="c6437-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="c6437-278">Onaltılık değeri 12 ve ardından karakter 3 olan karakteri içeren bir dize oluşturmak için, bir tane yazabilir `"\x00123"` veya `"\x12" + "3"` bunun yerine.</span><span class="sxs-lookup"><span data-stu-id="c6437-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="c6437-279">Bir *string_literaL* `string`türü.</span><span class="sxs-lookup"><span data-stu-id="c6437-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="c6437-280">Her dize sabit değeri, yeni bir dize örneği ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="c6437-281">Dize eşitlik işlecine ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) göre eşdeğer olan iki veya daha fazla dize değişmez değeri aynı programda görünürse, bu dize değişmez değerleri aynı dize örneğine başvurur.</span><span class="sxs-lookup"><span data-stu-id="c6437-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="c6437-282">Örneğin, tarafından oluşturulan çıkış</span><span class="sxs-lookup"><span data-stu-id="c6437-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="c6437-283">, `True` iki sabit değer aynı dize örneğine başvurduğundan.</span><span class="sxs-lookup"><span data-stu-id="c6437-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="c6437-284">Ara değerli dize sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="c6437-284">Interpolated string literals</span></span>

<span data-ttu-id="c6437-285">Enterpolasyonlu dize sabit değerleri dize sabit değerlerine benzerdir, ancak deyimlerde `}`olduğu gibi `{` ve ile ayrılmış olan delikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="c6437-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="c6437-286">Çalışma zamanında, ifadeler, metin biçimlerinin, delik oluşması durumunda dize üzerinde yer aldığı amaçla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="c6437-287">Dize ilişkilendirtiğinin sözdizimi ve semantiği bölümünde açıklanmaktadır ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="c6437-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="c6437-288">Dize sabit değerleri gibi, enterpolasyonlu dize değişmez değerleri normal veya tam olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="c6437-289">Enterpolasyonlu normal dize sabit `$"` değerleri `"`ve ile sınırlandırılır ve enterpolasyonlu değişmez `$@"` dize `"`değerleri ve ile sınırlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6437-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="c6437-290">Diğer değişmez değerler gibi, bir enterpolasyonlu dize sabit değerinin sözcük analizi, aşağıdaki Dilbilgisine göre ilk olarak tek bir belirtece neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6437-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="c6437-291">Ancak, sözdizimsel Analize göre, bir ara değer dizesinin tek belirteci, delikleri kapsayan dizenin bölümleri için birkaç belirtece bölünmüştür ve deliklere gerçekleşen giriş öğeleri, sözcüksel olarak analiz edilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="c6437-292">Bu işlem, işlenmek üzere daha fazla enterpolasyonlu dize değişmezleri üretebilir. ancak, sözcüksel doğru olursa, sonunda sözdizimsel çözümlemenin işlemesi için bir dizi belirtece yol açacaktır.</span><span class="sxs-lookup"><span data-stu-id="c6437-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="c6437-293">*İnterpolated_string_literal* belirteci, *interpolated_string_literal*içindeki oluşum sırasıyla birden fazla belirteç ve diğer girdi öğeleri olarak yeniden yorumlanır:</span><span class="sxs-lookup"><span data-stu-id="c6437-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="c6437-294">Aşağıdakilerin oluşumları ayrı ayrı belirteçler olarak yeniden yorumlanır `$` : önde gelen işareti, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* ve *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="c6437-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="c6437-295">Bunlar arasındaki *regular_balanced_text* ve *verbatim_balanced_text* örnekleri, bir *input_section* ([sözcük temelli analiz](lexical-structure.md#lexical-analysis)) olarak yeniden işlenir ve giriş öğelerinin ortaya çıkan sırası olarak yeniden yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="c6437-296">Bunlar, yeniden yorumlanan enterpolasyonlu dize sabit belirteçlerini içerebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="c6437-297">Sözdizimsel analiz, belirteçleri bir *interpolated_string_expression* ([enterpolasyonlu dizeler](expressions.md#interpolated-strings)) olarak yeniden birleştirir.</span><span class="sxs-lookup"><span data-stu-id="c6437-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="c6437-298">Örnek TODO</span><span class="sxs-lookup"><span data-stu-id="c6437-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="c6437-299">Null değişmez değeri</span><span class="sxs-lookup"><span data-stu-id="c6437-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="c6437-300">*Null_literal* örtük olarak bir başvuru türüne veya null yapılabilir türe dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="c6437-301">İşleçler ve noktaleyiciler</span><span class="sxs-lookup"><span data-stu-id="c6437-301">Operators and punctuators</span></span>

<span data-ttu-id="c6437-302">Birkaç tür işleç ve noktalama vardır.</span><span class="sxs-lookup"><span data-stu-id="c6437-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="c6437-303">İşleçler, bir veya daha fazla işlenen ile ilgili işlemleri betimleyen ifadelerde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="c6437-304">Örneğin, ifadesi `a + b` iki `a` işleneni ve ' `+` `b`i eklemek için işlecini kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="c6437-305">Noktaleyiciler gruplandırma ve ayırma içindir.</span><span class="sxs-lookup"><span data-stu-id="c6437-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="c6437-306">*Right_shift* ve *right_shift_assignment* üretimlerinin dikey çubuğu, sözdizimsel dilbilgisinde diğer üretimlerin aksine, belirteçler arasında herhangi bir türde (çift boşluk değil) bir karakter yapılmasına izin verilmediğini belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="c6437-307">Bu üretimler, *type_parameter_list*s 'nin doğru işlemesini etkinleştirmek için özel olarak değerlendirilir ([tür parametreleri](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="c6437-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="c6437-308">Ön işleme yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-308">Pre-processing directives</span></span>

<span data-ttu-id="c6437-309">Ön işleme yönergeleri, kaynak dosyalarının bölümlerini koşullu olarak atlama, hata ve uyarı koşullarını raporlama ve kaynak kodunun farklı bölgelerini ayırıcı yapma yeteneği sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="c6437-310">"Ön işleme yönergeleri" terimi yalnızca C ve C++ programlama dilleri ile tutarlılık için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="c6437-311">' C#De, ayrı bir ön işleme adımı yoktur; ön işleme yönergeleri, sözcük temelli analiz aşamasının bir parçası olarak işlenir.</span><span class="sxs-lookup"><span data-stu-id="c6437-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="c6437-312">Aşağıdaki ön işleme yönergeleri mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="c6437-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="c6437-313">`#define`ve `#undef`sırasıyla, koşullu derleme sembollerini ([bildirim yönergeleri](lexical-structure.md#declaration-directives)) tanımlamak ve bunları kaldırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="c6437-314">`#if`, `#elif`, ve`#endif`, kaynak kodu bölümlerini koşullu olarak atlamak için kullanılır ([koşullu derleme yönergeleri](lexical-structure.md#conditional-compilation-directives)). `#else`</span><span class="sxs-lookup"><span data-stu-id="c6437-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="c6437-315">`#line`, hatalar ve uyarılar için oluşturulan satır numaralarını denetlemek için kullanılan ([satır yönergeleri](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="c6437-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="c6437-316">`#error`ve `#warning`sırasıyla hata ve uyarı vermek için kullanılır ([tanı yönergeleri](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="c6437-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="c6437-317">`#region`Kaynak kodun ([bölge yönergeleri](lexical-structure.md#region-directives)) bölümlerini açıkça işaretlemek için de kullanılır. `#endregion`</span><span class="sxs-lookup"><span data-stu-id="c6437-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="c6437-318">`#pragma`derleyici için isteğe bağlı bağlamsal bilgileri belirtmek için kullanılan ([pragma yönergeleri](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="c6437-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="c6437-319">Bir ön işleme yönergesi her zaman ayrı bir kaynak kod satırı kaplar ve her zaman bir karakter ve `#` bir ön işleme yönergesi adıyla başlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="c6437-320">`#` Karakterden önce boşluk, karakter ve yönerge adı `#` arasında boşluk oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="c6437-321">`#define` `#else` `#elif` `#undef` ,,`#endif`,,,, ,Veya`#endregion` yönergesini içeren bir kaynak satırı, teksatırlıkbiraçıklamailesonabaşlayabilir.`#if` `#line`</span><span class="sxs-lookup"><span data-stu-id="c6437-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="c6437-322">Önceden işleme yönergelerini içeren `/* */` kaynak satırlarda sınırlandırılmış açıklamalara (yorumların stili) izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="c6437-323">Ön işleme yönergeleri belirteç değildir ve sözdizimi dilbilgisinde bir C#parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="c6437-324">Ancak, önceden işleme yönergeleri, belirteç dizilerini dahil etmek veya hariç tutmak için kullanılabilir ve bu şekilde bir C# programın anlamını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="c6437-325">Örneğin, derlendiğinde program:</span><span class="sxs-lookup"><span data-stu-id="c6437-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="c6437-326">program ile aynı belirteçlerin tam dizisiyle sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="c6437-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="c6437-327">Bu nedenle, sözcüksel olarak iki program çok farklı, sözdizimsel olarak aynıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="c6437-328">Koşullu derleme sembolleri</span><span class="sxs-lookup"><span data-stu-id="c6437-328">Conditional compilation symbols</span></span>

<span data-ttu-id="c6437-329">`#if`,, Ve `#else` `#elif` yönergeleri`#endif` tarafından sunulan koşullu derleme işlevselliği, ön işleme ifadeleri ([ön işleme ifadeleri](lexical-structure.md#pre-processing-expressions)) ve koşullu olarak denetlenir derleme sembolleri.</span><span class="sxs-lookup"><span data-stu-id="c6437-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="c6437-330">Koşullu derleme sembolünün iki olası durumu vardır: ***tanımlı*** veya ***tanımsız***.</span><span class="sxs-lookup"><span data-stu-id="c6437-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="c6437-331">Bir kaynak dosyanın sözcük işleme başlangıcında, bir dış mekanizma (bir komut satırı derleyici seçeneği gibi) tarafından açıkça tanımlanmadığı sürece koşullu derleme simgesi tanımsızdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="c6437-332">Bir `#define` yönerge işlendiğinde, Bu yönergede adlı koşullu derleme sembolü bu kaynak dosyada tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="c6437-333">Sembol, aynı simgenin bir `#undef` yönergesi işlenene veya kaynak dosyanın sonuna ulaşılana kadar tanımlanmış olarak kalır.</span><span class="sxs-lookup"><span data-stu-id="c6437-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="c6437-334">`#define` Bunun`#undef` bir etkisi, bir kaynak dosyasındaki yönergelerin aynı programdaki diğer kaynak dosyaları üzerinde hiçbir etkisi olmaması.</span><span class="sxs-lookup"><span data-stu-id="c6437-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="c6437-335">Bir ön işleme ifadesinde başvurulmadığında, tanımlı bir koşullu derleme sembolünün Boole değeri `true`vardır ve tanımsız bir koşullu derleme sembolü Boolean değerine `false`sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c6437-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="c6437-336">Koşullu derleme simgelerinin, ön işleme ifadelerinde başvurulmadan önce açıkça bildirilmesini gerektiren bir gereksinim yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6437-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="c6437-337">Bunun yerine, bildirilmemiş semboller yalnızca tanımsız olur ve bu nedenle değeri `false`vardır.</span><span class="sxs-lookup"><span data-stu-id="c6437-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="c6437-338">Koşullu derleme sembolleri için ad alanı farklıdır ve bir C# programdaki diğer tüm adlandırılmış varlıklardan ayrıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="c6437-339">Koşullu derleme sembollerine yalnızca `#define` ve `#undef` yönergeleriyle ve önceden işleme ifadelerinde başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="c6437-340">Ön işleme ifadeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-340">Pre-processing expressions</span></span>

<span data-ttu-id="c6437-341">Ön işleme ifadeleri, ve `#if` `#elif` yönergeleri içinde oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="c6437-342">,, `!` `!=` `==` Ve`||` işleçlerine ön işleme ifadelerinde izin verilir ve gruplama için parantezler kullanılabilir. `&&`</span><span class="sxs-lookup"><span data-stu-id="c6437-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="c6437-343">Bir ön işleme ifadesinde başvurulmadığında, tanımlı bir koşullu derleme sembolünün Boole değeri `true`vardır ve tanımsız bir koşullu derleme sembolü Boolean değerine `false`sahiptir.</span><span class="sxs-lookup"><span data-stu-id="c6437-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="c6437-344">Bir ön işleme ifadesinin değerlendirmesi her zaman bir Boole değeri verir.</span><span class="sxs-lookup"><span data-stu-id="c6437-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="c6437-345">Bir ön işleme ifadesi için değerlendirme kuralları bir sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) ile aynıdır, ancak başvurılabilen Kullanıcı tanımlı varlıkların koşullu derleme sembolleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c6437-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="c6437-346">Bildirim yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-346">Declaration directives</span></span>

<span data-ttu-id="c6437-347">Bildirim yönergeleri, koşullu derleme sembollerini tanımlamak veya tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="c6437-348">Bir `#define` yönergeyi işleme, belirtilen koşullu derleme sembolünün, yönergeyi izleyen kaynak satırla başlayarak tanımlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="c6437-349">Benzer şekilde, bir `#undef` yönergenin işlenmesi verilen koşullu derleme sembolünün, yönergeyi izleyen kaynak satırla başlayarak tanımsız hale gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="c6437-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="c6437-350">Kaynak `#define` dosyadaki `#undef` tüm ve yönergelerinin kaynak dosyadaki ilk *belirteçten* ([belirteçler](lexical-structure.md#tokens)) önce gerçekleşmesi gerekir; Aksi takdirde derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="c6437-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="c6437-351">Sezgisel koşullarda `#define` ve `#undef` yönergeler kaynak dosyadaki tüm "gerçek koddan" gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="c6437-352">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c6437-352">The example:</span></span>
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
<span data-ttu-id="c6437-353">, kaynak dosyadaki ilk `#define` belirteçten `namespace` (anahtar sözcüğü) önce gelen yönergeler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="c6437-354">Aşağıdaki örnek, bir derleme zamanı hatasına neden olur çünkü bu, `#define` gerçek bir kod izler:</span><span class="sxs-lookup"><span data-stu-id="c6437-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="c6437-355">, `#define` Zaten tanımlanmış olan bir koşullu derleme sembolünü tanımlayabilir, bu sembol için herhangi bir araya `#undef` girmeden emin olamaz.</span><span class="sxs-lookup"><span data-stu-id="c6437-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="c6437-356">Aşağıdaki örnek bir koşullu derleme sembolünü `A` tanımlar ve sonra yeniden tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="c6437-357">`#undef` Tanımlı olmayan bir koşullu derleme sembolünü "tanımlayabilir".</span><span class="sxs-lookup"><span data-stu-id="c6437-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="c6437-358">Aşağıdaki örnek bir koşullu derleme sembolünü `A` tanımlar ve iki kez tanımlamaz; ikincisi `#undef` hiçbir etkiye sahip olmasa da, hala geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="c6437-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="c6437-359">Koşullu derleme yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-359">Conditional compilation directives</span></span>

<span data-ttu-id="c6437-360">Koşullu derleme yönergeleri, kaynak dosyanın bölümlerini koşullu olarak dahil etmek veya hariç tutmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="c6437-361">Sözdizimi tarafından gösterildiği gibi, koşullu derleme yönergeleri `#if` , sırasıyla, bir yönerge, sıfır veya daha fazla `#elif` yönerge, sıfır veya bir `#else` yönerge ve bir `#endif` yönerge olarak yazılmış şekilde yazılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6437-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="c6437-362">Yönergeler arasında kaynak kodunun koşullu bölümleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="c6437-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="c6437-363">Her bölüm hemen önceki yönerge tarafından denetlenir.</span><span class="sxs-lookup"><span data-stu-id="c6437-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="c6437-364">Koşullu bir bölümün kendisi bu yönergeler için tüm kümeler için iç içe geçmiş koşullu derleme yönergeleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="c6437-365">Bir *pp_conditional* , normal sözcük işleme için kapsanan *conditional_section*s 'nin en çok birini seçer:</span><span class="sxs-lookup"><span data-stu-id="c6437-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="c6437-366">`#if` `true` Ve`#elif` yönergelerinin pp_expression s, bir alınana kadar sırayla değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="c6437-367">Bir ifade `true`varsa, karşılık gelen yönergesinin *conditional_section* seçilidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="c6437-368">Tüm *pp_expression*s `false`yield varsa ve bir `#else` yönerge varsa, `#else` yönergesinin *conditional_section* seçilidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="c6437-369">Aksi takdirde, hiçbir *conditional_section* seçili değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="c6437-370">Seçili *conditional_section*, varsa normal *input_section*olarak işlenir: bölümünde yer alan kaynak kodu, sözlü dilbilgisine uymalıdır; belirteçler, bölümündeki kaynak koddan oluşturulur; ve bölümündeki ön işleme yönergelerinin tanımlanmış etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="c6437-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="c6437-371">Kalan *conditional_section*s, varsa, *skipped_section*s olarak işlenir: ön işleme yönergeleri haricinde, bölümdeki kaynak kodu sözlü dilbilgisine bağlı kalmamalıdır; bölümünde kaynak koddan hiçbir belirteç oluşturulmaz; ve bölümündeki ön işleme yönergeleri, sözcüksel doğru olmalıdır ancak başka türlü işlenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="c6437-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="c6437-372">Bir *skipped_section*olarak işlenmekte olan bir *conditional_section* içinde, iç içe geçmiş *conditional_section*s (iç içe `#if`geçmiş... içinde bulunur. `#endif` ve`#region`... yapılar) de skipped_section s olarak işlenir. `#endregion`</span><span class="sxs-lookup"><span data-stu-id="c6437-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="c6437-373">Aşağıdaki örnek, koşullu derleme yönergelerinin nasıl iç içe geçirebileceği gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="c6437-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="c6437-374">Ön işleme yönergeleri hariç, atlanan kaynak kodu sözcük temelli analize tabi değildir.</span><span class="sxs-lookup"><span data-stu-id="c6437-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="c6437-375">Örneğin, aşağıdaki, `#else` bölümünde Sonlandırılmamış açıklamaya rağmen geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="c6437-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="c6437-376">Ancak, bu ön işleme yönergelerinin kaynak kodun atlanan bölümlerinde bile, sözcüksel olarak doğru olması gerektiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6437-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="c6437-377">Önceden işleme yönergeleri, çok satırlı giriş öğeleri içinde göründükleri zaman işlenmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="c6437-378">Örneğin, program:</span><span class="sxs-lookup"><span data-stu-id="c6437-378">For example, the program:</span></span>
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
<span data-ttu-id="c6437-379">çıkışın sonucu:</span><span class="sxs-lookup"><span data-stu-id="c6437-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="c6437-380">Bu durumlarda, işlenen önceden işleme yönergelerinin kümesi, *pp_expression*değerlendirmesine bağlı olarak değişebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="c6437-381">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c6437-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="c6437-382">`class` , tanımlanmadığına `{` `}` `Q` bakılmaksızınherzamanaynıbelirteçakışını`X` () üretir.</span><span class="sxs-lookup"><span data-stu-id="c6437-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="c6437-383">Tanımlı ise, çok satırlı yorum nedeniyle işlenen tek yönergeler `#endif`ve olur `#if`. `X`</span><span class="sxs-lookup"><span data-stu-id="c6437-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="c6437-384">Tanımlanmamışsa, üç yönerge (`#if`, `#else`, `#endif`) yönerge kümesinin bir parçasıdır. `X`</span><span class="sxs-lookup"><span data-stu-id="c6437-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="c6437-385">Tanılama yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-385">Diagnostic directives</span></span>

<span data-ttu-id="c6437-386">Tanılama yönergeleri, diğer derleme zamanı hatalarıyla ve uyarılarla aynı şekilde bildirilen hata ve uyarı iletilerini açık bir şekilde oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="c6437-387">Örnek:</span><span class="sxs-lookup"><span data-stu-id="c6437-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="c6437-388">her zaman bir uyarı üretir ("iade etmeden önce kod incelemesi gereklidir") ve koşullu semboller `Debug` ve `Retail` ikisi de tanımlanmışsa bir derleme zamanı hatası ("derleme hem hata ayıklama hem de perakende olamaz") oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6437-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="c6437-389">Bir *pp_message* , rastgele metin içerebileceğini unutmayın; Özellikle, sözcükteki `can't`tek tırnak içinde gösterildiği gibi iyi biçimlendirilmiş belirteçler içermesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="c6437-390">Bölge yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-390">Region directives</span></span>

<span data-ttu-id="c6437-391">Bölge yönergeleri, kaynak kodu bölgelerini açıkça işaretlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="c6437-392">Bir bölgeye hiçbir anlam anlamı eklenmez; bölgeler, programcı tarafından veya kaynak kodun bir bölümünü işaretlemek için otomatikleştirilmiş araçlar tarafından kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6437-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="c6437-393">Aynı şekilde `#region` veya `#endregion` yönergesinde belirtilen iletide anlam anlamı yoktur; yalnızca bölgeyi tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="c6437-394">Eşleştirme `#region` ve`#endregion` yönergelerin farklı *pp_message*s 'leri olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="c6437-395">Bir bölgenin sözcük işleme işlemi:</span><span class="sxs-lookup"><span data-stu-id="c6437-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="c6437-396">formun koşullu bir derleme yönergesinin yalnızca sözcük işleme öğesine karşılık gelir:</span><span class="sxs-lookup"><span data-stu-id="c6437-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="c6437-397">Çizgi yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-397">Line directives</span></span>

<span data-ttu-id="c6437-398">Satır yönergeleri, uyarı ve hatalar gibi, derleyici tarafından raporlanan ve arayan bilgi öznitelikleri ([çağıran bilgi öznitelikleri](attributes.md#caller-info-attributes)) tarafından kullanılan satır numaralarını ve kaynak dosya adlarını değiştirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="c6437-399">Satır yönergeleri en yaygın olarak, diğer bir metin girişinden C# kaynak kodu oluşturan meta programlama araçlarında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="c6437-400">Hiçbir `#line` yönergesi yoksa, derleyici çıktıda gerçek satır numaralarını ve kaynak dosya adlarını raporlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="c6437-401">Olmayan `#line` bir`default` *line_indicator* içeren bir yönergeyi işlerken, derleyici, belirtilen satır numarası (ve belirtilmişse dosya adı) gibi yönergeden sonra satırı değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="c6437-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="c6437-402">Bir `#line default` yönerge, önceki tüm #line yönergelerinin etkisini tersine çevirir.</span><span class="sxs-lookup"><span data-stu-id="c6437-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="c6437-403">Derleyici, kesin olmayan `#line` bir yönergeler gibi, izleyen satırlar için gerçek satır bilgilerini raporlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="c6437-404">Bir `#line hidden` yönergenin hata iletilerinde bildirilen dosya ve satır numaraları üzerinde hiçbir etkisi yoktur, ancak kaynak düzeyinde hata ayıklamayı etkiler.</span><span class="sxs-lookup"><span data-stu-id="c6437-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="c6437-405">Hata ayıklarken, bir `#line hidden` yönergeyle sonraki `#line` `#line hidden`yönerge (olmayan) arasındaki tüm satırlarda satır numarası bilgisi yoktur.</span><span class="sxs-lookup"><span data-stu-id="c6437-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="c6437-406">Hata ayıklayıcıda kod üzerinden adımla, bu satırlar tamamen atlanır.</span><span class="sxs-lookup"><span data-stu-id="c6437-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="c6437-407">Bir *dosya_adı* , kaçış karakterlerinin işlenmediği normal bir dize sabit değerinden farklı olduğunu unutmayın; "`\`" karakteri, bir *dosya_adı*içindeki normal ters eğik çizgi karakterini belirtir.</span><span class="sxs-lookup"><span data-stu-id="c6437-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="c6437-408">Pragma yönergeleri</span><span class="sxs-lookup"><span data-stu-id="c6437-408">Pragma directives</span></span>

<span data-ttu-id="c6437-409">`#pragma` Ön işleme yönergesi, isteğe bağlı bağlamsal bilgileri derleyiciye belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="c6437-410">Bir `#pragma` yönergede sağlanan bilgiler hiçbir şekilde program semantiğini değiştirmez.</span><span class="sxs-lookup"><span data-stu-id="c6437-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="c6437-411">C#derleyici `#pragma` uyarılarını denetlemek için yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="c6437-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="c6437-412">Dilin gelecekteki sürümleri ek `#pragma` yönergeler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="c6437-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="c6437-413">Diğer C# derleyicilerle birlikte çalışabilirliği sağlamak için, Microsoft C# derleyicisi bilinmeyen `#pragma` yönergeler için derleme hataları vermez; bu yönergeler, ancak uyarılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6437-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="c6437-414">pragma uyarısı</span><span class="sxs-lookup"><span data-stu-id="c6437-414">Pragma warning</span></span>

<span data-ttu-id="c6437-415">`#pragma warning` Yönerge, sonraki program metninin derlenmesi sırasında tüm veya belirli bir uyarı iletisi kümesini devre dışı bırakmak veya geri yüklemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6437-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="c6437-416">Uyarı `#pragma warning` listesini atlayacak bir yönerge tüm uyarıları etkiler.</span><span class="sxs-lookup"><span data-stu-id="c6437-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="c6437-417">Uyarı `#pragma warning` listesi içeren bir yönerge yalnızca listede belirtilen uyarıları etkiler.</span><span class="sxs-lookup"><span data-stu-id="c6437-417">A `#pragma warning` directive that includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="c6437-418">Bir `#pragma warning disable` yönerge, tüm veya verilen uyarı kümesini devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="c6437-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="c6437-419">Bir `#pragma warning restore` yönerge, derleme biriminin başlangıcında geçerli olan durum kümesini veya verilen uyarıları geri yükler.</span><span class="sxs-lookup"><span data-stu-id="c6437-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="c6437-420">Belirli bir uyarı dışarıdan devre dışı bırakılmışsa, bir (tümü `#pragma warning restore` veya belirli bir uyarı için) bu uyarıyı yeniden etkinleştiremeyeceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="c6437-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="c6437-421">Aşağıdaki örnek, kullanım dışı bırakılmış `#pragma warning` üyelere, Microsoft C# derleyicisinden gelen uyarı numarasını kullanarak başvurulduğunu bildiren uyarıyı geçici olarak devre dışı bırakmak için öğesinin kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6437-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
