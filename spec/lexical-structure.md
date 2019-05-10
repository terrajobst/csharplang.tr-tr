---
ms.openlocfilehash: e103f6629a363c6cd76607699ff74d69aa73ed57
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488964"
---
# <a name="lexical-structure"></a><span data-ttu-id="0c69e-101">Sözcük yapısı</span><span class="sxs-lookup"><span data-stu-id="0c69e-101">Lexical structure</span></span>

## <a name="programs"></a><span data-ttu-id="0c69e-102">Programlar</span><span class="sxs-lookup"><span data-stu-id="0c69e-102">Programs</span></span>

<span data-ttu-id="0c69e-103">C# ***program*** bir veya daha fazla oluşur ***kaynak dosyaları***, resmi olarak bilinen ***derleme biriminden*** ([derleme biriminden](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-103">A C# ***program*** consists of one or more ***source files***, known formally as ***compilation units*** ([Compilation units](namespaces.md#compilation-units)).</span></span> <span data-ttu-id="0c69e-104">Sıralı bir Unicode karakter kaynak dosyasıdır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-104">A source file is an ordered sequence of Unicode characters.</span></span> <span data-ttu-id="0c69e-105">Kaynak dosyaları, dosyaları ile bire bir ilişkisi genellikle bir dosya sisteminde vardır. ancak bu yazışma gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-105">Source files typically have a one-to-one correspondence with files in a file system, but this correspondence is not required.</span></span> <span data-ttu-id="0c69e-106">Düzeyde taşınabilirlik için dosya sistemindeki dosyaları UTF-8 ile kodlanmış önerilir kodlama.</span><span class="sxs-lookup"><span data-stu-id="0c69e-106">For maximal portability, it is recommended that files in a file system be encoded with the UTF-8 encoding.</span></span>

<span data-ttu-id="0c69e-107">Kavramsal olarak açıklamak gerekirse, bir program üç adımda derlenir:</span><span class="sxs-lookup"><span data-stu-id="0c69e-107">Conceptually speaking, a program is compiled using three steps:</span></span>

1. <span data-ttu-id="0c69e-108">Dönüştürme, bir dosyayı bir özel karakter topluluğunun ve kodlama düzeni, Unicode karakter dizisine dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="0c69e-108">Transformation, which converts a file from a particular character repertoire and encoding scheme into a sequence of Unicode characters.</span></span>
2. <span data-ttu-id="0c69e-109">Sözcük analizi giriş karakter Unicode akışı belirteçleri bir akışa dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="0c69e-109">Lexical analysis, which translates a stream of Unicode input characters into a stream of tokens.</span></span>
3. <span data-ttu-id="0c69e-110">Söz dizimi analizi, belirteçlerin akışı yürütülebilir koda çevirir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-110">Syntactic analysis, which translates the stream of tokens into executable code.</span></span>

## <a name="grammars"></a><span data-ttu-id="0c69e-111">Dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="0c69e-111">Grammars</span></span>

<span data-ttu-id="0c69e-112">Bu belirtimi iki dilbilgisi kullanarak C# programlama dilinin söz dizimini gösterir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-112">This specification presents the syntax of the C# programming language using two grammars.</span></span> <span data-ttu-id="0c69e-113">***Sözcük Dilbilgisi*** ([sözcük Dilbilgisi](lexical-structure.md#lexical-grammar)) formunu satır sonlandırıcılar, boşluk, yorumlar, belirteçleri ve ön işleme yönergeleri için Unicode karakterler nasıl birleştirileceğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-113">The ***lexical grammar*** ([Lexical grammar](lexical-structure.md#lexical-grammar)) defines how Unicode characters are combined to form line terminators, white space, comments, tokens, and pre-processing directives.</span></span> <span data-ttu-id="0c69e-114">***Söz dizimsel Dilbilgisi*** ([söz Dizimsel Dilbilgisi](lexical-structure.md#syntactic-grammar)) sözcük dilbilgisi kaynaklanan belirteçleri form C# programları nasıl birleştirileceğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-114">The ***syntactic grammar*** ([Syntactic grammar](lexical-structure.md#syntactic-grammar)) defines how the tokens resulting from the lexical grammar are combined to form C# programs.</span></span>

### <a name="grammar-notation"></a><span data-ttu-id="0c69e-115">Dilbilgisi gösterimi</span><span class="sxs-lookup"><span data-stu-id="0c69e-115">Grammar notation</span></span>

<span data-ttu-id="0c69e-116">Sözcük temelli ve söz dizimsel dilbilgisi ANTLR dilbilgisi aracının gösterimini kullanarak Backus-Naur formu içinde sunulur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-116">The lexical and syntactic grammars are presented in Backus-Naur form using the notation of the ANTLR grammar tool.</span></span>

### <a name="lexical-grammar"></a><span data-ttu-id="0c69e-117">Sözcük dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="0c69e-117">Lexical grammar</span></span>

<span data-ttu-id="0c69e-118">Sözcük dilbilgisi C# içinde sunulan [sözcük analizi](lexical-structure.md#lexical-analysis), [belirteçleri](lexical-structure.md#tokens), ve [yönergeleri ön işleme](lexical-structure.md#pre-processing-directives).</span><span class="sxs-lookup"><span data-stu-id="0c69e-118">The lexical grammar of C# is presented in [Lexical analysis](lexical-structure.md#lexical-analysis), [Tokens](lexical-structure.md#tokens), and [Pre-processing directives](lexical-structure.md#pre-processing-directives).</span></span> <span data-ttu-id="0c69e-119">Sözcük dilbilgisi terminal simgelerini Unicode karakter kümesinin karakterlerdir ve karakter form belirteçleri nasıl birleştirildiğini sözcük dilbilgisi belirtir ([belirteçleri](lexical-structure.md#tokens)), boşluk ([boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamaları](lexical-structure.md#comments)) ve ön işleme yönergeleri ([yönergeleri ön işleme](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-119">The terminal symbols of the lexical grammar are the characters of the Unicode character set, and the lexical grammar specifies how characters are combined to form tokens ([Tokens](lexical-structure.md#tokens)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span>

<span data-ttu-id="0c69e-120">Her kaynak dosyası bir C# programında uymalıdır *giriş* üretimi sözcük dilbilgisi ([sözcük analizi](lexical-structure.md#lexical-analysis)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-120">Every source file in a C# program must conform to the *input* production of the lexical grammar ([Lexical analysis](lexical-structure.md#lexical-analysis)).</span></span>

### <a name="syntactic-grammar"></a><span data-ttu-id="0c69e-121">Söz dizimi dilbilgisi</span><span class="sxs-lookup"><span data-stu-id="0c69e-121">Syntactic grammar</span></span>

<span data-ttu-id="0c69e-122">C# sözdizimi dilbilgisi bölümleri ve bu bölümün aşağıdaki ek sunulur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-122">The syntactic grammar of C# is presented in the chapters and appendices that follow this chapter.</span></span> <span data-ttu-id="0c69e-123">Söz dizimi dilbilgisi terminal simgelerini sözcük dilbilgisi tarafından tanımlanan belirteçleridir ve belirteçleri form C# programları nasıl birleştirildiğini söz dizimsel dilbilgisi belirtir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-123">The terminal symbols of the syntactic grammar are the tokens defined by the lexical grammar, and the syntactic grammar specifies how tokens are combined to form C# programs.</span></span>

<span data-ttu-id="0c69e-124">Her kaynak dosyada bir C# programı için uygun olmalıdır *compilation_unit* söz dizimsel dilbilgisi üretimini ([derleme biriminden](namespaces.md#compilation-units)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-124">Every source file in a C# program must conform to the *compilation_unit* production of the syntactic grammar ([Compilation units](namespaces.md#compilation-units)).</span></span>

## <a name="lexical-analysis"></a><span data-ttu-id="0c69e-125">Sözcük analizi</span><span class="sxs-lookup"><span data-stu-id="0c69e-125">Lexical analysis</span></span>

<span data-ttu-id="0c69e-126">*Giriş* üretim bir C# kaynak dosyası sözcük yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-126">The *input* production defines the lexical structure of a C# source file.</span></span> <span data-ttu-id="0c69e-127">Her kaynak dosyasının bir C# programında, bu sözcük dilbilgisi üretim için uygun olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-127">Each source file in a C# program must conform to this lexical grammar production.</span></span>

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

<span data-ttu-id="0c69e-128">Beş temel öğeleri olun sözcük yapısı, bir C# kaynak dosyası: Satır sonlandırıcılar ([satır sonlandırıcılar](lexical-structure.md#line-terminators)), boşluk ([boşluk](lexical-structure.md#white-space)), açıklamalar ([açıklamaları](lexical-structure.md#comments)), belirteçleri ([belirteçleri](lexical-structure.md#tokens)), ve yönergeleri ön işleme ([yönergeleri ön işleme](lexical-structure.md#pre-processing-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-128">Five basic elements make up the lexical structure of a C# source file: Line terminators ([Line terminators](lexical-structure.md#line-terminators)), white space ([White space](lexical-structure.md#white-space)), comments ([Comments](lexical-structure.md#comments)), tokens ([Tokens](lexical-structure.md#tokens)), and pre-processing directives ([Pre-processing directives](lexical-structure.md#pre-processing-directives)).</span></span> <span data-ttu-id="0c69e-129">Bu temel öğeleri, yalnızca belirteçleri söz dizimi bir C# programı dilbilgisi içinde önemli ([söz Dizimsel Dilbilgisi](lexical-structure.md#syntactic-grammar)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-129">Of these basic elements, only tokens are significant in the syntactic grammar of a C# program ([Syntactic grammar](lexical-structure.md#syntactic-grammar)).</span></span>

<span data-ttu-id="0c69e-130">Dosya, söz dizimi analizi giriş dönüşen belirteçleri dizisini azaltma C# kaynak dosyası sözcük işlenmesini oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-130">The lexical processing of a C# source file consists of reducing the file into a sequence of tokens which becomes the input to the syntactic analysis.</span></span> <span data-ttu-id="0c69e-131">Satır sonlandırıcılar, boşluk ve yorum belirteçleri, ayırmak için görebilir ve ön işleme yönergeleri atlanacak kaynak dosyasının bölümlerini neden olabilir, ancak Aksi halde bu sözcük temelli öğeler söz dizimi bir C# programı yapısını üzerinde hiçbir etkiye sahip.</span><span class="sxs-lookup"><span data-stu-id="0c69e-131">Line terminators, white space, and comments can serve to separate tokens, and pre-processing directives can cause sections of the source file to be skipped, but otherwise these lexical elements have no impact on the syntactic structure of a C# program.</span></span>

<span data-ttu-id="0c69e-132">İlişkilendirilmiş dize değişmez değerleri söz konusu olduğunda ([ilişkilendirilmiş dize değişmez değerleri](lexical-structure.md#interpolated-string-literals)) tek bir belirteç başlangıçta sözcük analizi tarafından oluşturulur ancak sürekli olarak sözcük temelli analize bağlı birkaç giriş öğelerine bozuk tüm ilişkilendirilmiş dize değişmez değerleri Çözüldü kadar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-132">In the case of interpolated string literals ([Interpolated string literals](lexical-structure.md#interpolated-string-literals)) a single token is initially produced by lexical analysis, but is broken up into several input elements which are repeatedly subjected to lexical analysis until all interpolated string literals have been resolved.</span></span> <span data-ttu-id="0c69e-133">Sonuçta elde edilen belirteçleri sonra söz dizimi analizi giriş olarak sunar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-133">The resulting tokens then serve as input to the syntactic analysis.</span></span>

<span data-ttu-id="0c69e-134">Kaynak dosyada bir karakter dizisi birkaç sözcük dilbilgisi üretim eşleşen sözcük işleme her zaman olası en uzun sözcük öğesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-134">When several lexical grammar productions match a sequence of characters in a source file, the lexical processing always forms the longest possible lexical element.</span></span> <span data-ttu-id="0c69e-135">Örneğin, karakter dizisi `//` sözcük o öğenin tek bir uzun olduğu için tek satır açıklama başlangıcı olarak işlenir `/` belirteci.</span><span class="sxs-lookup"><span data-stu-id="0c69e-135">For example, the character sequence `//` is processed as the beginning of a single-line comment because that lexical element is longer than a single `/` token.</span></span>

### <a name="line-terminators"></a><span data-ttu-id="0c69e-136">Satır sonlandırıcılar</span><span class="sxs-lookup"><span data-stu-id="0c69e-136">Line terminators</span></span>

<span data-ttu-id="0c69e-137">Satır sonlandırıcılar bir C# kaynak dosyası karakterlerini çizgiye bölün.</span><span class="sxs-lookup"><span data-stu-id="0c69e-137">Line terminators divide the characters of a C# source file into lines.</span></span>

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

<span data-ttu-id="0c69e-138">Kaynak ile uyumluluk kod dosya sonu işaretleyicileri ekleyin düzenleme araçlarını ve bir kaynağı etkinleştirmek için bir dizi düzgün görüntülenmesi için dosya satırları sona için her kaynak dosyasının bir C# programı içinde sırayla aşağıdaki dönüştürmeleri uygulanır:</span><span class="sxs-lookup"><span data-stu-id="0c69e-138">For compatibility with source code editing tools that add end-of-file markers, and to enable a source file to be viewed as a sequence of properly terminated lines, the following transformations are applied, in order, to every source file in a C# program:</span></span>

*  <span data-ttu-id="0c69e-139">Kaynak dosyasının son karakterin bir denetim Z karakterini olup olmadığını (`U+001A`), bu karakter silinir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-139">If the last character of the source file is a Control-Z character (`U+001A`), this character is deleted.</span></span>
*  <span data-ttu-id="0c69e-140">Bir satır başı karakteri (`U+000D`), kaynak dosyası boş ise ve kaynak dosyasının son karakter bir satır başı değilse kaynak dosyasının sonuna eklenir (`U+000D`), satır besleme (`U+000A`), satır ayırıcı (`U+2028`), veya bir paragraf ayracı (`U+2029`).</span><span class="sxs-lookup"><span data-stu-id="0c69e-140">A carriage-return character (`U+000D`) is added to the end of the source file if that source file is non-empty and if the last character of the source file is not a carriage return (`U+000D`), a line feed (`U+000A`), a line separator (`U+2028`), or a paragraph separator (`U+2029`).</span></span>

### <a name="comments"></a><span data-ttu-id="0c69e-141">Açıklamalar</span><span class="sxs-lookup"><span data-stu-id="0c69e-141">Comments</span></span>

<span data-ttu-id="0c69e-142">İki tür açıklamaları desteklenir: tek satırlı yorumlar ve sınırlandırılmış yorumlar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-142">Two forms of comments are supported: single-line comments and delimited comments.</span></span> <span data-ttu-id="0c69e-143">***Tek satırlı yorumlar*** karakterleri ile Başlat `//` ve kaynak satır sonuna kadar genişletin.</span><span class="sxs-lookup"><span data-stu-id="0c69e-143">***Single-line comments*** start with the characters `//` and extend to the end of the source line.</span></span> <span data-ttu-id="0c69e-144">***Ayrılmış açıklama*** karakterleri ile Başlat `/*` ve son karakterler `*/`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-144">***Delimited comments*** start with the characters `/*` and end with the characters `*/`.</span></span> <span data-ttu-id="0c69e-145">Ayrılmış açıklama birden fazla satırı kapsayabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-145">Delimited comments may span multiple lines.</span></span>

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

<span data-ttu-id="0c69e-146">Açıklamalar iç içe kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="0c69e-146">Comments do not nest.</span></span> <span data-ttu-id="0c69e-147">Karakter sıraları `/*` ve `*/` içinde özel bir anlamı yoktur bir `//` yorum ve karakter sıraları `//` ve `/*` ayrılmış bir yorum içinde özel bir anlamı yoktur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-147">The character sequences `/*` and `*/` have no special meaning within a `//` comment, and the character sequences `//` and `/*` have no special meaning within a delimited comment.</span></span>

<span data-ttu-id="0c69e-148">Yorumlar, karakter ve dize değişmez değerleri içinde işlenir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-148">Comments are not processed within character and string literals.</span></span>

<span data-ttu-id="0c69e-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="0c69e-149">The example</span></span>
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
<span data-ttu-id="0c69e-150">sınırlandırılmış bir açıklama içerir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-150">includes a delimited comment.</span></span>

<span data-ttu-id="0c69e-151">Örnek</span><span class="sxs-lookup"><span data-stu-id="0c69e-151">The example</span></span>
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
<span data-ttu-id="0c69e-152">birkaç tek satırlık açıklamaları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-152">shows several single-line comments.</span></span>

### <a name="white-space"></a><span data-ttu-id="0c69e-153">Boşluk</span><span class="sxs-lookup"><span data-stu-id="0c69e-153">White space</span></span>

<span data-ttu-id="0c69e-154">Boşluk karakterleri (boşluk karakteri içeren) Zs Unicode sınıfıyla yanı sıra yatay sekme karakterinin, dikey sekme karakteri ve form besleme karakteri gibi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-154">White space is defined as any character with Unicode class Zs (which includes the space character) as well as the horizontal tab character, the vertical tab character, and the form feed character.</span></span>

```antlr
whitespace
    : '<Any character with Unicode class Zs>'
    | '<Horizontal tab character (U+0009)>'
    | '<Vertical tab character (U+000B)>'
    | '<Form feed character (U+000C)>'
    ;
```

## <a name="tokens"></a><span data-ttu-id="0c69e-155">Belirteçler</span><span class="sxs-lookup"><span data-stu-id="0c69e-155">Tokens</span></span>

<span data-ttu-id="0c69e-156">Belirteçlerin birkaç türü vardır: tanımlayıcılar, anahtar sözcükler, değişmez değerler, işleçler ve noktalama işaretçileri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-156">There are several kinds of tokens: identifiers, keywords, literals, operators, and punctuators.</span></span> <span data-ttu-id="0c69e-157">Ayırıcılar belirteçleri görür ancak boşluk ve yorum belirteçleri, değildir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-157">White space and comments are not tokens, though they act as separators for tokens.</span></span>

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

### <a name="unicode-character-escape-sequences"></a><span data-ttu-id="0c69e-158">Unicode karakter kaçış dizileri</span><span class="sxs-lookup"><span data-stu-id="0c69e-158">Unicode character escape sequences</span></span>

<span data-ttu-id="0c69e-159">Bir Unicode karakter kaçış dizisi, bir Unicode karakteri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0c69e-159">A Unicode character escape sequence represents a Unicode character.</span></span> <span data-ttu-id="0c69e-160">Unicode karakter kaçış dizileri tanımlayıcıları olarak işlenir ([tanımlayıcıları](lexical-structure.md#identifiers)), karakter değişmez değerleri ([karakter değişmez değerleri](lexical-structure.md#character-literals)) ve normal dize değişmez değerleri ([dize değişmez değerleri](lexical-structure.md#string-literals)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-160">Unicode character escape sequences are processed in identifiers ([Identifiers](lexical-structure.md#identifiers)), character literals ([Character literals](lexical-structure.md#character-literals)), and regular string literals ([String literals](lexical-structure.md#string-literals)).</span></span> <span data-ttu-id="0c69e-161">Bir Unicode karakter kaçış başka bir konuma (örneğin, bir işleç, noktalama işaretçisi veya anahtar oluşturmak için) işlenmiyor.</span><span class="sxs-lookup"><span data-stu-id="0c69e-161">A Unicode character escape is not processed in any other location (for example, to form an operator, punctuator, or keyword).</span></span>

```antlr
unicode_escape_sequence
    : '\\u' hex_digit hex_digit hex_digit hex_digit
    | '\\U' hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit
    ;
```

<span data-ttu-id="0c69e-162">Unicode çıkış dizisi onaltılık sayı aşağıdaki tarafından oluşturulmuş tek Unicode karakteri temsil eder "`\u`"veya"`\U`" karakter.</span><span class="sxs-lookup"><span data-stu-id="0c69e-162">A Unicode escape sequence represents the single Unicode character formed by the hexadecimal number following the "`\u`" or "`\U`" characters.</span></span> <span data-ttu-id="0c69e-163">C# bir 16 bitlik Unicode kod noktaları karakter ve dize değerlerini kodlamasını kullandığından bir Unicode karakter ile U + 10FFFF U + 10000 aralık içinde bir karakter sabiti içinde izin verilmez ve bir dize sabit değeri bir Unicode vekil çifti kullanarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-163">Since C# uses a 16-bit encoding of Unicode code points in characters and string values, a Unicode character in the range U+10000 to U+10FFFF is not permitted in a character literal and is represented using a Unicode surrogate pair in a string literal.</span></span> <span data-ttu-id="0c69e-164">Unicode karakterler 0x10FFFF yukarıda kod noktası ile desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-164">Unicode characters with code points above 0x10FFFF are not supported.</span></span>

<span data-ttu-id="0c69e-165">Birden çok çevirileri gerçekleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-165">Multiple translations are not performed.</span></span> <span data-ttu-id="0c69e-166">Örneğin, dize sabit değeri "`\u005Cu005C`"değerine eşdeğer olan"`\u005C`"yerine"`\`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-166">For instance, the string literal "`\u005Cu005C`" is equivalent to "`\u005C`" rather than "`\`".</span></span> <span data-ttu-id="0c69e-167">Unicode değerini `\u005C` karakter "`\`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-167">The Unicode value `\u005C` is the character "`\`".</span></span>

<span data-ttu-id="0c69e-168">Örnek</span><span class="sxs-lookup"><span data-stu-id="0c69e-168">The example</span></span>
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
<span data-ttu-id="0c69e-169">bazı kullanımlarını gösterir `\u0066`, harfi için kaçış sırası olduğu "`f`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-169">shows several uses of `\u0066`, which is the escape sequence for the letter "`f`".</span></span> <span data-ttu-id="0c69e-170">Program eşdeğerdir</span><span class="sxs-lookup"><span data-stu-id="0c69e-170">The program is equivalent to</span></span>
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

### <a name="identifiers"></a><span data-ttu-id="0c69e-171">Tanımlayıcılar</span><span class="sxs-lookup"><span data-stu-id="0c69e-171">Identifiers</span></span>

<span data-ttu-id="0c69e-172">Alt çizgi (C programlama dilinde geleneksel olduğu gibi) ilk karakteri olarak Unicode kaçış dizileri izin dışında bu bölümde verilen tanımlayıcıları için kuralları tam olarak Unicode standart eki 31 tarafından önerilen karşılık gelir tanımlayıcılar, izin verilen ve "`@`" karakteri, tanımlayıcı olarak kullanılacak anahtar sözcükleri etkinleştirmek için önek olarak verilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-172">The rules for identifiers given in this section correspond exactly to those recommended by the Unicode Standard Annex 31, except that underscore is allowed as an initial character (as is traditional in the C programming language), Unicode escape sequences are permitted in identifiers, and the "`@`" character is allowed as a prefix to enable keywords to be used as identifiers.</span></span>

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

<span data-ttu-id="0c69e-173">Yukarıda belirtilen Unicode karakter sınıfları hakkında daha fazla bilgi için Unicode standardı, sürüm 3.0, 4.5 bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="0c69e-173">For information on the Unicode character classes mentioned above, see The Unicode Standard, Version 3.0, section 4.5.</span></span>

<span data-ttu-id="0c69e-174">Geçerli tanımlayıcıları örnekler "`identifier1`","`_identifier2`", ve "`@if`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-174">Examples of valid identifiers include "`identifier1`", "`_identifier2`", and "`@if`".</span></span>

<span data-ttu-id="0c69e-175">Uyumlu bir program bir tanımlayıcıda Unicode standart eki 15 tarafından tanımlandığı şekilde, Unicode normalleştirme Form C tarafından tanımlanan kurallı biçimde olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-175">An identifier in a conforming program must be in the canonical format defined by Unicode Normalization Form C, as defined by Unicode Standard Annex 15.</span></span> <span data-ttu-id="0c69e-176">Uygulama tanımlı davranış normalleştirme formu C'de değil bir tanımlayıcı ile karşılaşıldığında; Ancak, bir tanılama gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-176">The behavior when encountering an identifier not in Normalization Form C is implementation-defined; however, a diagnostic is not required.</span></span>

<span data-ttu-id="0c69e-177">Önek "`@`" diğer programlama dilleriyle arabirim zaman yararlı tanımlayıcı olarak anahtar sözcükler kullanılmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-177">The prefix "`@`" enables the use of keywords as identifiers, which is useful when interfacing with other programming languages.</span></span> <span data-ttu-id="0c69e-178">Karakter `@` tanımlayıcısı, diğer dillerde öneki olmadan normal bir tanımlayıcı olarak görülebilir şekilde tanımlayıcısı aslında bir parçası değil.</span><span class="sxs-lookup"><span data-stu-id="0c69e-178">The character `@` is not actually part of the identifier, so the identifier might be seen in other languages as a normal identifier, without the prefix.</span></span> <span data-ttu-id="0c69e-179">Bir tanımlayıcıyla bir `@` önek çağrıldığında bir ***tam tanımlayıcı***.</span><span class="sxs-lookup"><span data-stu-id="0c69e-179">An identifier with an `@` prefix is called a ***verbatim identifier***.</span></span> <span data-ttu-id="0c69e-180">Kullanım `@` olmayan anahtar sözcükler tanımlayıcılar için önek izin, ancak stil sağlasa da kesinlikle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-180">Use of the `@` prefix for identifiers that are not keywords is permitted, but strongly discouraged as a matter of style.</span></span>

<span data-ttu-id="0c69e-181">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0c69e-181">The example:</span></span>
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
<span data-ttu-id="0c69e-182">adlı bir sınıf tanımlar "`class`"adlı statik bir yöntem ile"`static`"adlı bir parametre alan"`bool`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-182">defines a class named "`class`" with a static method named "`static`" that takes a parameter named "`bool`".</span></span> <span data-ttu-id="0c69e-183">Unicode çıkışları olduğundan, anahtar, belirteç izin verilmez Not "`cl\u0061ss`"bir tanımlayıcıdır ve aynı tanımlayıcı olarak"`@class`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-183">Note that since Unicode escapes are not permitted in keywords, the token "`cl\u0061ss`" is an identifier, and is the same identifier as "`@class`".</span></span>

<span data-ttu-id="0c69e-184">Bunlar aşağıdaki dönüştürmeleri, sırayla uygulanır sonra aynıysa iki tanımlayıcı aynı kabul edilir:</span><span class="sxs-lookup"><span data-stu-id="0c69e-184">Two identifiers are considered the same if they are identical after the following transformations are applied, in order:</span></span>

*  <span data-ttu-id="0c69e-185">Önek "`@`", kullandıysanız kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-185">The prefix "`@`", if used, is removed.</span></span>
*  <span data-ttu-id="0c69e-186">Her *unicode_escape_sequence* , karşılık gelen bir Unicode karakter dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="0c69e-186">Each *unicode_escape_sequence* is transformed into its corresponding Unicode character.</span></span>
*  <span data-ttu-id="0c69e-187">Tüm *formatting_character*s kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-187">Any *formatting_character*s are removed.</span></span>

<span data-ttu-id="0c69e-188">İki ardışık içeren tanımlayıcılar alt çizgi karakteri (`U+005F`) uygulama tarafından kullanım için ayrılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-188">Identifiers containing two consecutive underscore characters (`U+005F`) are reserved for use by the implementation.</span></span> <span data-ttu-id="0c69e-189">Örneğin, bir uygulama iki alt çizgi ile başlayan genişletilmiş anahtar sözcükleri sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-189">For example, an implementation might provide extended keywords that begin with two underscores.</span></span>

### <a name="keywords"></a><span data-ttu-id="0c69e-190">anahtar sözcükler</span><span class="sxs-lookup"><span data-stu-id="0c69e-190">Keywords</span></span>

<span data-ttu-id="0c69e-191">A ***anahtar sözcüğü*** ayrılmıştır ve tarafından zaman başında dışında bir tanımlayıcı olarak kullanılamaz, tanımlayıcı benzeri karakter dizisidir `@` karakter.</span><span class="sxs-lookup"><span data-stu-id="0c69e-191">A ***keyword*** is an identifier-like sequence of characters that is reserved, and cannot be used as an identifier except when prefaced by the `@` character.</span></span>

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

<span data-ttu-id="0c69e-192">Belirli tanımlayıcıları, bazı yerlerde dilbilgisi içinde özel bir anlamı yoktur ancak anahtar sözcükler değildir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-192">In some places in the grammar, specific identifiers have special meaning, but are not keywords.</span></span> <span data-ttu-id="0c69e-193">Tür tanımlayıcıları, bazen "bağlamsal anahtar sözcükler" da denir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-193">Such identifiers are sometimes referred to as "contextual keywords".</span></span> <span data-ttu-id="0c69e-194">Örneğin, bir özellik bildirimi içinde "`get`"ve"`set`" tanımlayıcıları specified ([erişimcileri](classes.md#accessors)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-194">For example, within a property declaration, the "`get`" and "`set`" identifiers have special meaning ([Accessors](classes.md#accessors)).</span></span> <span data-ttu-id="0c69e-195">Dışında bir tanımlayıcı `get` veya `set` bu kullanım bu sözcükler tanımlayıcı olarak kullanımını ile çakışmaması bu konumda hiçbir zaman izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-195">An identifier other than `get` or `set` is never permitted in these locations, so this use does not conflict with a use of these words as identifiers.</span></span> <span data-ttu-id="0c69e-196">Diğer durumlarda, örneğin bir tanımlayıcıyla gibi "`var`" türü örtük olarak belirlenmiş yerel değişken bildirimlerinde ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)), bağlamsal anahtar sözcüğü ile bildirilen adlar çakışabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-196">In other cases, such as with the identifier "`var`" in implicitly typed local variable declarations ([Local variable declarations](statements.md#local-variable-declarations)), a contextual keyword can conflict with declared names.</span></span> <span data-ttu-id="0c69e-197">Bu gibi durumlarda, bildirilen ad tanımlayıcısı olarak bağlamsal anahtar sözcük kullanmak önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-197">In such cases, the declared name takes precedence over the use of the identifier as a contextual keyword.</span></span>

### <a name="literals"></a><span data-ttu-id="0c69e-198">Sabit değerler</span><span class="sxs-lookup"><span data-stu-id="0c69e-198">Literals</span></span>

<span data-ttu-id="0c69e-199">A ***değişmez değer*** bir kaynak kodu bir değer gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-199">A ***literal*** is a source code representation of a value.</span></span>

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

#### <a name="boolean-literals"></a><span data-ttu-id="0c69e-200">Boole sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-200">Boolean literals</span></span>

<span data-ttu-id="0c69e-201">İki boolean değişmez değer vardır: `true` ve `false`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-201">There are two boolean literal values: `true` and `false`.</span></span>

```antlr
boolean_literal
    : 'true'
    | 'false'
    ;
```

<span data-ttu-id="0c69e-202">Türü bir *boolean_literal* olduğu `bool`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-202">The type of a *boolean_literal* is `bool`.</span></span>

#### <a name="integer-literals"></a><span data-ttu-id="0c69e-203">Tamsayı sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-203">Integer literals</span></span>

<span data-ttu-id="0c69e-204">Tamsayı sabit değerlerinde türlerindeki değerleri yazmak için kullanılan `int`, `uint`, `long`, ve `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-204">Integer literals are used to write values of types `int`, `uint`, `long`, and `ulong`.</span></span> <span data-ttu-id="0c69e-205">Tamsayı sabit değerlerinde sahip iki olası form: ondalık ve onaltılık.</span><span class="sxs-lookup"><span data-stu-id="0c69e-205">Integer literals have two possible forms: decimal and hexadecimal.</span></span>

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

<span data-ttu-id="0c69e-206">Değişmez değer bir tamsayı türü şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="0c69e-206">The type of an integer literal is determined as follows:</span></span>

*  <span data-ttu-id="0c69e-207">Değişmez değer sonek varsa, bu ilk hangi değeri gösterilebileceği bu tür vardır: `int`, `uint`, `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-207">If the literal has no suffix, it has the first of these types in which its value can be represented: `int`, `uint`, `long`, `ulong`.</span></span>
*  <span data-ttu-id="0c69e-208">Tarafından değişmez değer olup olmadığını `U` veya `u`, ilk hangi değeri gösterilebileceği bu tür vardır: `uint`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-208">If the literal is suffixed by `U` or `u`, it has the first of these types in which its value can be represented: `uint`, `ulong`.</span></span>
*  <span data-ttu-id="0c69e-209">Tarafından değişmez değer olup olmadığını `L` veya `l`, ilk hangi değeri gösterilebileceği bu tür vardır: `long`, `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-209">If the literal is suffixed by `L` or `l`, it has the first of these types in which its value can be represented: `long`, `ulong`.</span></span>
*  <span data-ttu-id="0c69e-210">Tarafından değişmez değer olup olmadığını `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, veya `lu`, türü `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-210">If the literal is suffixed by `UL`, `Ul`, `uL`, `ul`, `LU`, `Lu`, `lU`, or `lu`, it is of type `ulong`.</span></span>

<span data-ttu-id="0c69e-211">Bir tamsayı sabit değeri tarafından temsil edilen değeri aralığının dışında ise `ulong` yazın, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-211">If the value represented by an integer literal is outside the range of the `ulong` type, a compile-time error occurs.</span></span>

<span data-ttu-id="0c69e-212">Birkaç stil önerilir, "`L`"kullanılabilir yerine"`l`" sabit değerleri türü yazarken `long`, harf karıştırılabilir kolay olduğundan "`l`"ile "basamak`1`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-212">As a matter of style, it is suggested that "`L`" be used instead of "`l`" when writing literals of type `long`, since it is easy to confuse the letter "`l`" with the digit "`1`".</span></span>

<span data-ttu-id="0c69e-213">En küçük olası izin vermek için `int` ve `long` yazılabilir ondalık tamsayı değişmez değer olarak şu iki kuralı mevcut değerleri:</span><span class="sxs-lookup"><span data-stu-id="0c69e-213">To permit the smallest possible `int` and `long` values to be written as decimal integer literals, the following two rules exist:</span></span>

* <span data-ttu-id="0c69e-214">Olduğunda bir *decimal_integer_literal* 2147483648 değerle (2 ^ 31) ve hiçbir *integer_type_suffix* bir tekli işleç belirteci eksi hemen ardından belirteci olarak görünür ([birli eksi İşleç](expressions.md#unary-minus-operator)), sonuç türü bir sabittir `int` değeri -2147483648 ile (-2 ^ 31).</span><span class="sxs-lookup"><span data-stu-id="0c69e-214">When a *decimal_integer_literal* with the value 2147483648 (2^31) and no *integer_type_suffix* appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `int` with the value -2147483648 (-2^31).</span></span> <span data-ttu-id="0c69e-215">Diğer durumlarda bu tür bir *decimal_integer_literal* türünde `uint`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-215">In all other situations, such a *decimal_integer_literal* is of type `uint`.</span></span>
* <span data-ttu-id="0c69e-216">Olduğunda bir *decimal_integer_literal* 9223372036854775808 değerle (2 ^ 63) ve hiçbir *integer_type_suffix* veya *integer_type_suffix* `L` veya `l` bir tekli işleç belirteci eksi hemen ardından belirteci olarak görünür ([birli eksi işleci](expressions.md#unary-minus-operator)), sonuç türü bir sabittir `long` -9223372036854775808 değerle (-2 ^ 63).</span><span class="sxs-lookup"><span data-stu-id="0c69e-216">When a *decimal_integer_literal* with the value 9223372036854775808 (2^63) and no *integer_type_suffix* or the *integer_type_suffix* `L` or `l` appears as the token immediately following a unary minus operator token ([Unary minus operator](expressions.md#unary-minus-operator)), the result is a constant of type `long` with the value -9223372036854775808 (-2^63).</span></span> <span data-ttu-id="0c69e-217">Diğer durumlarda bu tür bir *decimal_integer_literal* türünde `ulong`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-217">In all other situations, such a *decimal_integer_literal* is of type `ulong`.</span></span>

#### <a name="real-literals"></a><span data-ttu-id="0c69e-218">Gerçek değişmez değerleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-218">Real literals</span></span>

<span data-ttu-id="0c69e-219">Türlerindeki değerleri yazmak için kullanılan gerçek değişmez değerleri `float`, `double`, ve `decimal`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-219">Real literals are used to write values of types `float`, `double`, and `decimal`.</span></span>

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

<span data-ttu-id="0c69e-220">Hayır ise *real_type_suffix* belirtilirse, gerçek değişmez değer türü `double`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-220">If no *real_type_suffix* is specified, the type of the real literal is `double`.</span></span> <span data-ttu-id="0c69e-221">Aksi takdirde, gerçek tür soneki gerçek değişmez değer türü şu şekilde belirler:</span><span class="sxs-lookup"><span data-stu-id="0c69e-221">Otherwise, the real type suffix determines the type of the real literal, as follows:</span></span>

*  <span data-ttu-id="0c69e-222">Tarafından ve sonra gerçek değişmez değerin `F` veya `f` türünde `float`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-222">A real literal suffixed by `F` or `f` is of type `float`.</span></span> <span data-ttu-id="0c69e-223">Örneğin, değişmez değerler `1f`, `1.5f`, `1e10f`, ve `123.456F` türü tümü `float`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-223">For example, the literals `1f`, `1.5f`, `1e10f`, and `123.456F` are all of type `float`.</span></span>
*  <span data-ttu-id="0c69e-224">Tarafından ve sonra gerçek değişmez değerin `D` veya `d` türünde `double`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-224">A real literal suffixed by `D` or `d` is of type `double`.</span></span> <span data-ttu-id="0c69e-225">Örneğin, değişmez değerler `1d`, `1.5d`, `1e10d`, ve `123.456D` türü tümü `double`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-225">For example, the literals `1d`, `1.5d`, `1e10d`, and `123.456D` are all of type `double`.</span></span>
*  <span data-ttu-id="0c69e-226">Tarafından ve sonra gerçek değişmez değerin `M` veya `m` türünde `decimal`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-226">A real literal suffixed by `M` or `m` is of type `decimal`.</span></span> <span data-ttu-id="0c69e-227">Örneğin, değişmez değerler `1m`, `1.5m`, `1e10m`, ve `123.456M` türü tümü `decimal`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-227">For example, the literals `1m`, `1.5m`, `1e10m`, and `123.456M` are all of type `decimal`.</span></span> <span data-ttu-id="0c69e-228">Bu sabit dönüştürülür bir `decimal` tam değer almak ve gerekirse, en yakın kullanarak gösterilebilir değere yuvarlama değer banker yuvarlama ([decimal türü](types.md#the-decimal-type)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-228">This literal is converted to a `decimal` value by taking the exact value, and, if necessary, rounding to the nearest representable value using banker's rounding ([The decimal type](types.md#the-decimal-type)).</span></span> <span data-ttu-id="0c69e-229">Değişmez değer görünen herhangi bir ölçekte, değerin yuvarlanmış ya da değer sıfırdır (hangi ikinci durumda oturum ve ölçek 0 olacaktır) korunur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-229">Any scale apparent in the literal is preserved unless the value is rounded or the value is zero (in which latter case the sign and scale will be 0).</span></span> <span data-ttu-id="0c69e-230">Bu nedenle, değişmez değer `2.900m` ondalık oturum oluşturmak için ayrıştırılacak `0`, katsayısı `2900`ve ölçek `3`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-230">Hence, the literal `2.900m` will be parsed to form the decimal with sign `0`, coefficient `2900`, and scale `3`.</span></span>

<span data-ttu-id="0c69e-231">Belirtilen hazır değerin belirtilen türdeki temsil edilemiyorsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-231">If the specified literal cannot be represented in the indicated type, a compile-time error occurs.</span></span>

<span data-ttu-id="0c69e-232">Gerçek bir sabit değer türündeki değeri `float` veya `double` IEEE kullanılarak belirlenir "yuvarlak yakın için" modu.</span><span class="sxs-lookup"><span data-stu-id="0c69e-232">The value of a real literal of type `float` or `double` is determined by using the IEEE "round to nearest" mode.</span></span>

<span data-ttu-id="0c69e-233">Bir gerçek değişmez değerin ondalık basamak her zaman ondalık ayırıcıdan sonra gerekli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0c69e-233">Note that in a real literal, decimal digits are always required after the decimal point.</span></span> <span data-ttu-id="0c69e-234">Örneğin, `1.3F` gerçek sabit değer ancak olan `1.F` değil.</span><span class="sxs-lookup"><span data-stu-id="0c69e-234">For example, `1.3F` is a real literal but `1.F` is not.</span></span>

#### <a name="character-literals"></a><span data-ttu-id="0c69e-235">Karakter değişmez değerleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-235">Character literals</span></span>

<span data-ttu-id="0c69e-236">Karakterin değişmez değeri tek bir karakteri temsil eder ve genellikle bir tırnak karakteri olarak oluşan `'a'`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-236">A character literal represents a single character, and usually consists of a character in quotes, as in `'a'`.</span></span>

<span data-ttu-id="0c69e-237">Not: ANTLR dilbilgisi gösterimi aşağıdaki karmaşık hale getirir!</span><span class="sxs-lookup"><span data-stu-id="0c69e-237">Note: The ANTLR grammar notation makes the following confusing!</span></span> <span data-ttu-id="0c69e-238">Siz yazarken ANTLR içinde `\'` için tek bir teklif anlaşıldığı `'`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-238">In ANTLR, when you write `\'` it stands for a single quote `'`.</span></span> <span data-ttu-id="0c69e-239">Ve yazarken `\\` için tek bir ters eğik çizgi anlaşıldığı `\`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-239">And when you write `\\` it stands for a single backslash `\`.</span></span> <span data-ttu-id="0c69e-240">Bu nedenle tek tırnak işareti sonra bir karakter, sonra da tek bir tırnak işareti ile başlayan bir karakter sabiti için ilk kural anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-240">Therefore the first rule for a character literal means it starts with a single quote, then a character, then a single quote.</span></span> <span data-ttu-id="0c69e-241">Ve on olası basit çıkış sıraları `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-241">And the eleven possible simple escape sequences are `\'`, `\"`, `\\`, `\0`, `\a`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`.</span></span>

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

<span data-ttu-id="0c69e-242">Ters eğik çizgi karakterini izleyen bir karakter (`\`) içinde bir *karakter* şu karakterlerden biri olmalıdır: `'`, `"`, `\`, `0`, `a`, `b` , `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-242">A character that follows a backslash character (`\`) in a *character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="0c69e-243">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-243">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0c69e-244">Onaltılık kaçış sırası onaltılık sayı aşağıdaki tarafından oluşturulmuş bir değerle tek bir Unicode karakterini temsil eder "`\x`".</span><span class="sxs-lookup"><span data-stu-id="0c69e-244">A hexadecimal escape sequence represents a single Unicode character, with the value formed by the hexadecimal number following "`\x`".</span></span>

<span data-ttu-id="0c69e-245">Bir karakter sabiti temsil edilen değeri büyük olup olmadığını `U+FFFF`, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-245">If the value represented by a character literal is greater than `U+FFFF`, a compile-time error occurs.</span></span>

<span data-ttu-id="0c69e-246">Bir Unicode karakter kaçış dizisi ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)) aralığında bir karakter sabiti olmalıdır `U+0000` için `U+FFFF`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-246">A Unicode character escape sequence ([Unicode character escape sequences](lexical-structure.md#unicode-character-escape-sequences)) in a character literal must be in the range `U+0000` to `U+FFFF`.</span></span>

<span data-ttu-id="0c69e-247">Basit çıkış dizisi, bir Unicode karakter kodlamasını, aşağıdaki tabloda açıklandığı gibi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="0c69e-247">A simple escape sequence represents a Unicode character encoding, as described in the table below.</span></span>


| <span data-ttu-id="0c69e-248">__Kaçış sırası__</span><span class="sxs-lookup"><span data-stu-id="0c69e-248">__Escape sequence__</span></span> | <span data-ttu-id="0c69e-249">__Karakter adı__</span><span class="sxs-lookup"><span data-stu-id="0c69e-249">__Character name__</span></span> | <span data-ttu-id="0c69e-250">__Unicode kodlama__</span><span class="sxs-lookup"><span data-stu-id="0c69e-250">__Unicode encoding__</span></span> |
|---------------------|--------------------|----------------------|
| `\'`                | <span data-ttu-id="0c69e-251">tek tırnak</span><span class="sxs-lookup"><span data-stu-id="0c69e-251">Single quote</span></span>       | `0x0027`             | 
| `\"`                | <span data-ttu-id="0c69e-252">çift tırnak işareti</span><span class="sxs-lookup"><span data-stu-id="0c69e-252">Double quote</span></span>       | `0x0022`             | 
| `\\`                | <span data-ttu-id="0c69e-253">Ters eğik çizgi</span><span class="sxs-lookup"><span data-stu-id="0c69e-253">Backslash</span></span>          | `0x005C`             | 
| `\0`                | <span data-ttu-id="0c69e-254">Null</span><span class="sxs-lookup"><span data-stu-id="0c69e-254">Null</span></span>               | `0x0000`             | 
| `\a`                | <span data-ttu-id="0c69e-255">Uyarı</span><span class="sxs-lookup"><span data-stu-id="0c69e-255">Alert</span></span>              | `0x0007`             | 
| `\b`                | <span data-ttu-id="0c69e-256">Geri Al tuşu</span><span class="sxs-lookup"><span data-stu-id="0c69e-256">Backspace</span></span>          | `0x0008`             | 
| `\f`                | <span data-ttu-id="0c69e-257">form besleme</span><span class="sxs-lookup"><span data-stu-id="0c69e-257">Form feed</span></span>          | `0x000C`             | 
| `\n`                | <span data-ttu-id="0c69e-258">Yeni satır</span><span class="sxs-lookup"><span data-stu-id="0c69e-258">New line</span></span>           | `0x000A`             | 
| `\r`                | <span data-ttu-id="0c69e-259">satır başı</span><span class="sxs-lookup"><span data-stu-id="0c69e-259">Carriage return</span></span>    | `0x000D`             | 
| `\t`                | <span data-ttu-id="0c69e-260">Yatay sekme</span><span class="sxs-lookup"><span data-stu-id="0c69e-260">Horizontal tab</span></span>     | `0x0009`             | 
| `\v`                | <span data-ttu-id="0c69e-261">dikey sekme</span><span class="sxs-lookup"><span data-stu-id="0c69e-261">Vertical tab</span></span>       | `0x000B`             | 

<span data-ttu-id="0c69e-262">Türü bir *character_literal* olduğu `char`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-262">The type of a *character_literal* is `char`.</span></span>

#### <a name="string-literals"></a><span data-ttu-id="0c69e-263">Dize sabit değerleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-263">String literals</span></span>

<span data-ttu-id="0c69e-264">C# dize değişmez değerlerinin iki biçimini destekler: ***normal dize değişmez değerleri*** ve ***verbatim dize değişmez değerleri***.</span><span class="sxs-lookup"><span data-stu-id="0c69e-264">C# supports two forms of string literals: ***regular string literals*** and ***verbatim string literals***.</span></span>

<span data-ttu-id="0c69e-265">Normal bir dize sabit değeri olarak çift tırnak içine alınmış sıfır veya daha fazla karakterden oluşur `"hello"`ve her iki basit çıkış sıraları içerebilir (gibi `\t` karakter için sekmesinde) ve Unicode kaçış dizileri ve onaltılık.</span><span class="sxs-lookup"><span data-stu-id="0c69e-265">A regular string literal consists of zero or more characters enclosed in double quotes, as in `"hello"`, and may include both simple escape sequences (such as `\t` for the tab character), and hexadecimal and Unicode escape sequences.</span></span>

<span data-ttu-id="0c69e-266">Verbatim dize sabit değeri oluşan bir `@` karakter çift tırnak karakteri, sıfır veya daha fazla karakter ve bir kapanış çift tırnak karakteri tarafından izlenen.</span><span class="sxs-lookup"><span data-stu-id="0c69e-266">A verbatim string literal consists of an `@` character followed by a double-quote character, zero or more characters, and a closing double-quote character.</span></span> <span data-ttu-id="0c69e-267">Basit bir örnektir `@"hello"`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-267">A simple example is `@"hello"`.</span></span> <span data-ttu-id="0c69e-268">Bir harfi harfine dizede sabit değer, aynen, sınırlayıcılar arasındaki karakterleri yorumlanan yalnızca özel durum olan bir *quote_escape_sequence*.</span><span class="sxs-lookup"><span data-stu-id="0c69e-268">In a verbatim string literal, the characters between the delimiters are interpreted verbatim, the only exception being a *quote_escape_sequence*.</span></span> <span data-ttu-id="0c69e-269">Özellikle, basit kaçış dizileri ve onaltılık ve Unicode kaçış dizileri verbatim dize değişmez değerlerine işlenmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-269">In particular, simple escape sequences, and hexadecimal and Unicode escape sequences are not processed in verbatim string literals.</span></span> <span data-ttu-id="0c69e-270">Verbatim dize değişmez değeri birden fazla satırı kapsayabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-270">A verbatim string literal may span multiple lines.</span></span>

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

<span data-ttu-id="0c69e-271">Ters eğik çizgi karakterini izleyen bir karakter (`\`) içinde bir *regular_string_literal_character* şu karakterlerden biri olmalıdır: `'`, `"`, `\`, `0`, `a` , `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-271">A character that follows a backslash character (`\`) in a *regular_string_literal_character* must be one of the following characters: `'`, `"`, `\`, `0`, `a`, `b`, `f`, `n`, `r`, `t`, `u`, `U`, `x`, `v`.</span></span> <span data-ttu-id="0c69e-272">Aksi takdirde, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-272">Otherwise, a compile-time error occurs.</span></span>

<span data-ttu-id="0c69e-273">Örnek</span><span class="sxs-lookup"><span data-stu-id="0c69e-273">The example</span></span>
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
<span data-ttu-id="0c69e-274">dize değişmez değerleri çeşitli gösterir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-274">shows a variety of string literals.</span></span> <span data-ttu-id="0c69e-275">Son dize sabit değeri, `j`, birden fazla satır kapsayan bir verbatim dizesi sabit değeri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-275">The last string literal, `j`, is a verbatim string literal that spans multiple lines.</span></span> <span data-ttu-id="0c69e-276">Boşluk gibi yeni satır karakterleri dahil olmak üzere tırnak işaretleri arasındaki karakterleri verbatim korunur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-276">The characters between the quotation marks, including white space such as new line characters, are preserved verbatim.</span></span>

<span data-ttu-id="0c69e-277">Değişken sayıda onaltılık basamak, dize sabit değeri bir onaltılık kaçış sırası olabileceğinden `"\x123"` 123 onaltılık değer ile tek bir karakter içeriyor.</span><span class="sxs-lookup"><span data-stu-id="0c69e-277">Since a hexadecimal escape sequence can have a variable number of hex digits, the string literal `"\x123"` contains a single character with hex value 123.</span></span> <span data-ttu-id="0c69e-278">Onaltılık değeri 3 ardından 12 karakter içeren bir dize oluşturmak için bir yazabilirsiniz `"\x00123"` veya `"\x12" + "3"` yerine.</span><span class="sxs-lookup"><span data-stu-id="0c69e-278">To create a string containing the character with hex value 12 followed by the character 3, one could write `"\x00123"` or `"\x12" + "3"` instead.</span></span>

<span data-ttu-id="0c69e-279">Türü bir *string_literal* olduğu `string`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-279">The type of a *string_literal* is `string`.</span></span>

<span data-ttu-id="0c69e-280">Her dize sabit değeri yeni bir dize örneğinde sonuçlanmaz.</span><span class="sxs-lookup"><span data-stu-id="0c69e-280">Each string literal does not necessarily result in a new string instance.</span></span> <span data-ttu-id="0c69e-281">İki veya daha fazla dize değişmez değerleri, olduğunda göre dize eşitlik işlecini eşdeğer ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) görünmesini aynı programda, bu dize değişmez değerleri, aynı dize örneğine bakın.</span><span class="sxs-lookup"><span data-stu-id="0c69e-281">When two or more string literals that are equivalent according to the string equality operator ([String equality operators](expressions.md#string-equality-operators)) appear in the same program, these string literals refer to the same string instance.</span></span> <span data-ttu-id="0c69e-282">Örneğin, tarafından üretilen çıkış</span><span class="sxs-lookup"><span data-stu-id="0c69e-282">For instance, the output produced by</span></span>
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
<span data-ttu-id="0c69e-283">olan `True` iki değişmez değer için aynı dize örneğinde başvurduğundan.</span><span class="sxs-lookup"><span data-stu-id="0c69e-283">is `True` because the two literals refer to the same string instance.</span></span>

#### <a name="interpolated-string-literals"></a><span data-ttu-id="0c69e-284">İlişkilendirilmiş dize değişmez değerleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-284">Interpolated string literals</span></span>

<span data-ttu-id="0c69e-285">İlişkilendirilmiş dize değişmez değerleri, dize sabit değerleri için benzerdir, ancak tarafından ayrılmış açıkları içeren `{` ve `}`, burada görüntülerle deyimleri ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-285">Interpolated string literals are similar to string literals, but contain holes delimited by `{` and `}`, wherein expressions can occur.</span></span> <span data-ttu-id="0c69e-286">Çalışma zamanında, metinsel formlarına delik gerçekleştiği yerde dizesine yerine sahip kullanılabilir olmasını sağlamak amacıyla ifadeler değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-286">At runtime, the expressions are evaluated with the purpose of having their textual forms substituted into the string at the place where the hole occurs.</span></span> <span data-ttu-id="0c69e-287">Sözdizimi ve semantiği dize ilişkilendirme bölümünde açıklanan ([Ara değerli dizeler](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-287">The syntax and semantics of string interpolation are described in section ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="0c69e-288">Dize değişmez değerleri gibi normal veya verbatim ilişkilendirilmiş dize değişmez değerleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-288">Like string literals, interpolated string literals can be either regular or verbatim.</span></span> <span data-ttu-id="0c69e-289">Normal ilişkilendirilmiş dize değişmez değerleri ayrılmıştır `$"` ve `"`, ve verbatim ilişkilendirilmiş dize değişmez değerleri ayrılmıştır `$@"` ve `"`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-289">Interpolated regular string literals are delimited by `$"` and `"`, and interpolated verbatim string literals are delimited by `$@"` and `"`.</span></span>

<span data-ttu-id="0c69e-290">Diğer sabitler gibi sözcük temelli analiz ilişkilendirilmiş bir dize sabitinin aşağıdaki dilbilgisinde göre tek bir belirteç başlangıçta sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-290">Like other literals, lexical analysis of an interpolated string literal initially results in a single token, as per the grammar below.</span></span> <span data-ttu-id="0c69e-291">Ancak, söz dizimi analizi önce birkaç belirteç boşluklarını kapsayan dizesi bölümleri için ilişkilendirilmiş bir dize sabitinin tek belirteç bölünür ve açıklarına oluşan giriş öğelerini sözcüksel olarak yeniden analysed.</span><span class="sxs-lookup"><span data-stu-id="0c69e-291">However, before syntactic analysis, the single token of an interpolated string literal is broken into several tokens for the parts of the string enclosing the holes, and the input elements occurring in the holes are lexically analysed again.</span></span> <span data-ttu-id="0c69e-292">Bu sırayla işlenmesi, ancak varsa sözcüksel olarak düzeltin, sonunda bir dizi belirteçleri işlemek söz dizimi analizi için neden, daha ilişkilendirilmiş dize değişmez değerlerine oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-292">This may in turn produce more interpolated string literals to be processed, but, if lexically correct, will eventually lead to a sequence of tokens for syntactic analysis to process.</span></span>

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

<span data-ttu-id="0c69e-293">Bir *interpolated_string_literal* belirteci birden çok belirteçleri ve diğer öğeleri aşağıdaki gibi oluşumunu sırasına göre giriş olarak düşürülen *interpolated_string_literal*:</span><span class="sxs-lookup"><span data-stu-id="0c69e-293">An *interpolated_string_literal* token is reinterpreted as multiple tokens and other input elements as follows, in order of occurrence in the *interpolated_string_literal*:</span></span>

* <span data-ttu-id="0c69e-294">Aşağıdaki örneği ayrı ayrı belirteçlerin düşürülen: başında `$` işareti *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*,  *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* ve *interpolated_verbatim_string_end*.</span><span class="sxs-lookup"><span data-stu-id="0c69e-294">Occurrences of the following are reinterpreted as separate individual tokens: the leading `$` sign, *interpolated_regular_string_whole*, *interpolated_regular_string_start*, *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_whole*, *interpolated_verbatim_string_start*, *interpolated_verbatim_string_mid* and *interpolated_verbatim_string_end*.</span></span>
* <span data-ttu-id="0c69e-295">Oluşumlarını *regular_balanced_text* ve *verbatim_balanced_text* olarak bunlar arasında işlenmiş bir *input_section* ([sözcük analizi ](lexical-structure.md#lexical-analysis)) ve sonuçta elde edilen giriş öğeleri dizisi olarak düşürülen.</span><span class="sxs-lookup"><span data-stu-id="0c69e-295">Occurrences of *regular_balanced_text* and *verbatim_balanced_text* between these are reprocessed as an *input_section* ([Lexical analysis](lexical-structure.md#lexical-analysis)) and are reinterpreted as the resulting sequence of input elements.</span></span> <span data-ttu-id="0c69e-296">Bu sırayla ilişkilendirilmiş dize sabit değeri belirteçleri düşürülen için de içerebilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-296">These may in turn include interpolated string literal tokens to be reinterpreted.</span></span>

<span data-ttu-id="0c69e-297">Söz dizimi analizi yeniden birleştirmek belirteçlere bir *interpolated_string_expression* ([Ara değerli dizeler](expressions.md#interpolated-strings)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-297">Syntactic analysis will recombine the tokens into an *interpolated_string_expression* ([Interpolated strings](expressions.md#interpolated-strings)).</span></span>

<span data-ttu-id="0c69e-298">TODO örnekleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-298">Examples TODO</span></span>


#### <a name="the-null-literal"></a><span data-ttu-id="0c69e-299">Null sabiti</span><span class="sxs-lookup"><span data-stu-id="0c69e-299">The null literal</span></span>

```antlr
null_literal
    : 'null'
    ;
```

<span data-ttu-id="0c69e-300">*Null_literal* örtük bir başvuru türüyle veya Null olabilen bir türle dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-300">The  *null_literal* can be implicitly converted to a reference type or nullable type.</span></span>

### <a name="operators-and-punctuators"></a><span data-ttu-id="0c69e-301">İşleçler ve noktalama işaretçileri</span><span class="sxs-lookup"><span data-stu-id="0c69e-301">Operators and punctuators</span></span>

<span data-ttu-id="0c69e-302">İşleçler ve noktalama birkaç türü vardır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-302">There are several kinds of operators and punctuators.</span></span> <span data-ttu-id="0c69e-303">İşleçler, ifadelerde, bir veya daha fazla işlenen ilgili işlemler tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-303">Operators are used in expressions to describe operations involving one or more operands.</span></span> <span data-ttu-id="0c69e-304">Örneğin, ifade `a + b` kullanan `+` işleci iki işlenenleri eklemek için `a` ve `b`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-304">For example, the expression `a + b` uses the `+` operator to add the two operands `a` and `b`.</span></span> <span data-ttu-id="0c69e-305">Noktalama işaretçileri gruplama ve ayırma içindir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-305">Punctuators are for grouping and separating.</span></span>

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

<span data-ttu-id="0c69e-306">İçinde dikey çubuk *right_shift* ve *right_shift_assignment* üretim, diğer üretim söz dizimsel dilbilgisi, herhangi bir türde hiçbir karakter içinde aksine (bile belirtmek için kullanılır boşluk) simgeleri arasında izin verilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-306">The vertical bar in the *right_shift* and *right_shift_assignment* productions are used to indicate that, unlike other productions in the syntactic grammar, no characters of any kind (not even whitespace) are allowed between the tokens.</span></span> <span data-ttu-id="0c69e-307">Bu üretim özel için doğru işlemeyi etkinleştirmek için kabul edilir *type_parameter_list*s ([tür parametrelerindeki](classes.md#type-parameters)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-307">These productions are treated specially in order to enable the correct  handling of *type_parameter_list*s ([Type parameters](classes.md#type-parameters)).</span></span>

## <a name="pre-processing-directives"></a><span data-ttu-id="0c69e-308">Ön işleme yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-308">Pre-processing directives</span></span>

<span data-ttu-id="0c69e-309">Ön işleme yönergeleri koşullu olarak bölümlerde rapor hata ve uyarı koşulları için kaynak dosyalarının atlamak için ve kaynak kodunun farklı bölgeleri ayırmak için olanağı sunar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-309">The pre-processing directives provide the ability to conditionally skip sections of source files, to report error and warning conditions, and to delineate distinct regions of source code.</span></span> <span data-ttu-id="0c69e-310">"Ön işleme yönergeleri" terimi, yalnızca C ve C++ programlama dilleriyle tutarlılık için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-310">The term "pre-processing directives" is used only for consistency with the C and C++ programming languages.</span></span> <span data-ttu-id="0c69e-311">C# ' ta ayrı hiçbir ön işleme adım yoktur; ön işleme yönergeleri sözcük analizi aşamasının bir parçası işlenir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-311">In C#, there is no separate pre-processing step; pre-processing directives are processed as part of the lexical analysis phase.</span></span>

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

<span data-ttu-id="0c69e-312">Aşağıdaki ön işleme yönergeleri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0c69e-312">The following pre-processing directives are available:</span></span>

*  <span data-ttu-id="0c69e-313">`#define` ve `#undef`, Kaldır, sırasıyla, koşullu derleme simgeleri ve tanımlamak için kullanılır ([bildirim yönergeleri](lexical-structure.md#declaration-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-313">`#define` and `#undef`, which are used to define and undefine, respectively, conditional compilation symbols ([Declaration directives](lexical-structure.md#declaration-directives)).</span></span>
*  <span data-ttu-id="0c69e-314">`#if`, `#elif`, `#else`, ve `#endif`, kaynak kodun bölümlerini koşullu olarak atlamak için kullanılır ([koşullu derleme yönergeleri](lexical-structure.md#conditional-compilation-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-314">`#if`, `#elif`, `#else`, and `#endif`, which are used to conditionally skip sections of source code ([Conditional compilation directives](lexical-structure.md#conditional-compilation-directives)).</span></span>
*  <span data-ttu-id="0c69e-315">`#line`, hatalar ve Uyarılar için yayılan satır numaralarını denetlemek için kullanılan ([satır yönergeleri](lexical-structure.md#line-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-315">`#line`, which is used to control line numbers emitted for errors and warnings ([Line directives](lexical-structure.md#line-directives)).</span></span>
*  <span data-ttu-id="0c69e-316">`#error` ve `#warning`, hatalar ve uyarılar sırasıyla vermek için kullanılır ([tanılama yönergeleri](lexical-structure.md#diagnostic-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-316">`#error` and `#warning`, which are used to issue errors and warnings, respectively ([Diagnostic directives](lexical-structure.md#diagnostic-directives)).</span></span>
*  <span data-ttu-id="0c69e-317">`#region` ve `#endregion`, açık kaynak kod bölümlerini işaretlemek için kullanılır ([bölge yönergeleri](lexical-structure.md#region-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-317">`#region` and `#endregion`, which are used to explicitly mark sections of source code ([Region directives](lexical-structure.md#region-directives)).</span></span>
*  <span data-ttu-id="0c69e-318">`#pragma`, isteğe bağlı bağlamsal bilgi derleyiciye belirtmek için kullanılır ([Pragma yönergeleri](lexical-structure.md#pragma-directives)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-318">`#pragma`, which is used to specify optional contextual information to the compiler ([Pragma directives](lexical-structure.md#pragma-directives)).</span></span>

<span data-ttu-id="0c69e-319">Ön işleme yönergesi her zaman ayrı bir kaynak kod satırı kaplar ve her zaman ile başlayan bir `#` karakter ve bir ön işleme yönergesi adı.</span><span class="sxs-lookup"><span data-stu-id="0c69e-319">A pre-processing directive always occupies a separate line of source code and always begins with a `#` character and a pre-processing directive name.</span></span> <span data-ttu-id="0c69e-320">Boşluk önce oluşabilir `#` karakter ve arasında `#` karakter ve yönerge adı.</span><span class="sxs-lookup"><span data-stu-id="0c69e-320">White space may occur before the `#` character and between the `#` character and the directive name.</span></span>

<span data-ttu-id="0c69e-321">Kaynak satır içeren bir `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, veya `#endregion` yönergesi ile tek satır açıklama sonu olabilecek.</span><span class="sxs-lookup"><span data-stu-id="0c69e-321">A source line containing a `#define`, `#undef`, `#if`, `#elif`, `#else`, `#endif`, `#line`, or `#endregion` directive may end with a single-line comment.</span></span> <span data-ttu-id="0c69e-322">Ayrılmış Açıklama ( `/* */` açıklamaları stilini) ön işleme yönergeleri içeren kaynak satırları izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-322">Delimited comments (the `/* */` style of comments) are not permitted on source lines containing pre-processing directives.</span></span>

<span data-ttu-id="0c69e-323">Ön işleme yönergeleri belirteçlerinin ve C# sözdizimi dilbilgisi parçası değildir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-323">Pre-processing directives are not tokens and are not part of the syntactic grammar of C#.</span></span> <span data-ttu-id="0c69e-324">Ancak, ön işleme yönergeleri dahil etme veya belirteçleri dönüştürülmelerini dışlama için kullanılabilir ve bu şekilde bir C# programı anlamını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-324">However, pre-processing directives can be used to include or exclude sequences of tokens and can in that way affect the meaning of a C# program.</span></span> <span data-ttu-id="0c69e-325">Örneğin, derlendiğinde, program:</span><span class="sxs-lookup"><span data-stu-id="0c69e-325">For example, when compiled, the program:</span></span>
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
<span data-ttu-id="0c69e-326">Program belirteçleri tam aynı sırada sonuçları:</span><span class="sxs-lookup"><span data-stu-id="0c69e-326">results in the exact same sequence of tokens as the program:</span></span>
```csharp
class C
{
    void F() {}
    void I() {}
}
```

<span data-ttu-id="0c69e-327">Bu nedenle, sözcük temelinde iki program sözdizimi kurallarına göre oldukça farklı ise, bunlar aynıdır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-327">Thus, whereas lexically, the two programs are quite different, syntactically, they are identical.</span></span>

### <a name="conditional-compilation-symbols"></a><span data-ttu-id="0c69e-328">Koşullu derleme simgeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-328">Conditional compilation symbols</span></span>

<span data-ttu-id="0c69e-329">Koşullu derleme tarafından sağlanan işlevselliği `#if`, `#elif`, `#else`, ve `#endif` yönergeleri ifadeleri ön işleme aracılığıyla kontrol edilir ([ifadeleri ön işleme](lexical-structure.md#pre-processing-expressions)) ve koşullu derleme simgeleri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-329">The conditional compilation functionality provided by the `#if`, `#elif`, `#else`, and `#endif` directives is controlled through pre-processing expressions ([Pre-processing expressions](lexical-structure.md#pre-processing-expressions)) and conditional compilation symbols.</span></span>

```antlr
conditional_symbol
    : '<Any identifier_or_keyword except true or false>'
    ;
```

<span data-ttu-id="0c69e-330">Koşullu derleme sembol iki durumda olabilir: ***tanımlanan*** veya ***tanımlanmamış***.</span><span class="sxs-lookup"><span data-stu-id="0c69e-330">A conditional compilation symbol has two possible states: ***defined*** or ***undefined***.</span></span> <span data-ttu-id="0c69e-331">Dış mekanizmayı (örneğin, bir komut satırı derleyicisi seçeneği) tarafından açıkça tanımlanmadığı sürece sözcük işleme bir kaynak dosyasının başında, koşullu derleme sembol tanımlı değil.</span><span class="sxs-lookup"><span data-stu-id="0c69e-331">At the beginning of the lexical processing of a source file, a conditional compilation symbol is undefined unless it has been explicitly defined by an external mechanism (such as a command-line compiler option).</span></span> <span data-ttu-id="0c69e-332">Olduğunda bir `#define` yönergesi işlenir, bu yönerge adlı koşullu derleme sembol, kaynak dosyasında tanımlanan olur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-332">When a `#define` directive is processed, the conditional compilation symbol named in that directive becomes defined in that source file.</span></span> <span data-ttu-id="0c69e-333">Kadar sembol kalacağı bir `#undef` aynı sembolün işlenir veya kaynak dosyasının sonuna ulaşılana kadar yönergesi.</span><span class="sxs-lookup"><span data-stu-id="0c69e-333">The symbol remains defined until an `#undef` directive for that same symbol is processed, or until the end of the source file is reached.</span></span> <span data-ttu-id="0c69e-334">Bu bir olduğu çıkarımında olan `#define` ve `#undef` yönergeleri bir kaynak dosyasında diğer kaynak dosyaları aynı programda üzerinde hiçbir etkisi vardır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-334">An implication of this is that `#define` and `#undef` directives in one source file have no effect on other source files in the same program.</span></span>

<span data-ttu-id="0c69e-335">Bir ön işleme ifadesinde başvurulduğunda tanımlanmış koşullu derleme sembol Boole değerine sahip `true`, ve bir tanımsız koşullu derleme sembol Boole değerine sahip `false`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-335">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span> <span data-ttu-id="0c69e-336">Gereksinimi yoktur ifadeleri önceden işleme'de başvurulan önce bildirilen koşullu derleme simgeleri açıkça olabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-336">There is no requirement that conditional compilation symbols be explicitly declared before they are referenced in pre-processing expressions.</span></span> <span data-ttu-id="0c69e-337">Bunun yerine, bildirilmemiş simgeleri yalnızca tanımsız olur ve böylece değerine sahip `false`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-337">Instead, undeclared symbols are simply undefined and thus have the value `false`.</span></span>

<span data-ttu-id="0c69e-338">Koşullu derleme simgeleri için ad alanı diğer adlandırılmış varlıklar bir C# programında ayrıdır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-338">The name space for conditional compilation symbols is distinct and separate from all other named entities in a C# program.</span></span> <span data-ttu-id="0c69e-339">Koşullu derleme simgeleri yalnızca başvurulabilir içinde `#define` ve `#undef` yönergeleri ve ifadeleri önceden işliyor.</span><span class="sxs-lookup"><span data-stu-id="0c69e-339">Conditional compilation symbols can only be referenced in `#define` and `#undef` directives and in pre-processing expressions.</span></span>

### <a name="pre-processing-expressions"></a><span data-ttu-id="0c69e-340">Ön işleme ifadeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-340">Pre-processing expressions</span></span>

<span data-ttu-id="0c69e-341">İfadeleri ön işleme içinde gerçekleşebilir `#if` ve `#elif` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-341">Pre-processing expressions can occur in `#if` and `#elif` directives.</span></span> <span data-ttu-id="0c69e-342">İşleçler `!`, `==`, `!=`, `&&` ve `||` ifadeleri, ön işleme izin verilir ve Gruplama için parantez kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-342">The operators `!`, `==`, `!=`, `&&` and `||` are permitted in pre-processing expressions, and parentheses may be used for grouping.</span></span>

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

<span data-ttu-id="0c69e-343">Bir ön işleme ifadesinde başvurulduğunda tanımlanmış koşullu derleme sembol Boole değerine sahip `true`, ve bir tanımsız koşullu derleme sembol Boole değerine sahip `false`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-343">When referenced in a pre-processing expression, a defined conditional compilation symbol has the boolean value `true`, and an undefined conditional compilation symbol has the boolean value `false`.</span></span>

<span data-ttu-id="0c69e-344">Her zaman önceden işleme ifadesi değerlendirmesi bir Boole değeri verir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-344">Evaluation of a pre-processing expression always yields a boolean value.</span></span> <span data-ttu-id="0c69e-345">Ön işleme bir ifade değerlendirme kuralları için sabit bir ifade aynıdır ([sabit ifadeler](expressions.md#constant-expressions)), şeylerdir başvurulabilir yalnızca kullanıcı tarafından tanımlanan varlıkları koşullu derleme simgeleri .</span><span class="sxs-lookup"><span data-stu-id="0c69e-345">The rules of evaluation for a pre-processing expression are the same as those for a constant expression ([Constant expressions](expressions.md#constant-expressions)), except that the only user-defined entities that can be referenced are conditional compilation symbols.</span></span>

### <a name="declaration-directives"></a><span data-ttu-id="0c69e-346">Bildirim yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-346">Declaration directives</span></span>

<span data-ttu-id="0c69e-347">Bildirim yönergeleri, koşullu derleme simge tanımlarını Kaldır veya tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-347">The declaration directives are used to define or undefine conditional compilation symbols.</span></span>

```antlr
pp_declaration
    : whitespace? '#' whitespace? 'define' whitespace conditional_symbol pp_new_line
    | whitespace? '#' whitespace? 'undef' whitespace conditional_symbol pp_new_line
    ;

pp_new_line
    : whitespace? single_line_comment? new_line
    ;
```

<span data-ttu-id="0c69e-348">İşlenmesini bir `#define` yönergesi yönergesini izleyen kaynak satırıyla başlangıç tanımlı duruma, belirtilen koşullu derleme simge neden olur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-348">The processing of a `#define` directive causes the given conditional compilation symbol to become defined, starting with the source line that follows the directive.</span></span> <span data-ttu-id="0c69e-349">Benzer şekilde, işlenmesini bir `#undef` yönergesi, yönergesini izleyen kaynak satırı ile başlayarak, tanımsız hale verilen koşullu derleme simge neden olur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-349">Likewise, the processing of an `#undef` directive causes the given conditional compilation symbol to become undefined, starting with the source line that follows the directive.</span></span>

<span data-ttu-id="0c69e-350">Tüm `#define` ve `#undef` yönergeleri kaynak dosyasında ilk önce gerçekleşmelidir *belirteci* ([belirteçleri](lexical-structure.md#tokens)) kaynak dosyada; Aksi takdirde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="0c69e-350">Any `#define` and `#undef` directives in a source file must occur before the first *token* ([Tokens](lexical-structure.md#tokens)) in the source file; otherwise a compile-time error occurs.</span></span> <span data-ttu-id="0c69e-351">Sezgisel koşullarında `#define` ve `#undef` yönergeleri, kaynak dosyadaki tüm "gerçek kod" gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-351">In intuitive terms, `#define` and `#undef` directives must precede any "real code" in the source file.</span></span>

<span data-ttu-id="0c69e-352">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0c69e-352">The example:</span></span>
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
<span data-ttu-id="0c69e-353">geçerli olduğundan `#define` yönergeleri önünde ilk belirteç ( `namespace` anahtar sözcüğü) kaynak dosyadaki.</span><span class="sxs-lookup"><span data-stu-id="0c69e-353">is valid because the `#define` directives precede the first token (the `namespace` keyword) in the source file.</span></span>

<span data-ttu-id="0c69e-354">Aşağıdaki örnek bir derleme zamanı hatası nedeniyle sonuçlanır bir `#define` gerçek kod izler:</span><span class="sxs-lookup"><span data-stu-id="0c69e-354">The following example results in a compile-time error because a `#define` follows real code:</span></span>
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

<span data-ttu-id="0c69e-355">A `#define` zaten, burada bir araya giren olan tanımlı bir koşullu derleme simge tanımlayabilir `#undef` , sembol.</span><span class="sxs-lookup"><span data-stu-id="0c69e-355">A `#define` may define a conditional compilation symbol that is already defined, without there being any intervening `#undef` for that symbol.</span></span> <span data-ttu-id="0c69e-356">Aşağıdaki örnek bir koşullu derleme sembolünü tanımlar `A` ve yeniden tanımlar.</span><span class="sxs-lookup"><span data-stu-id="0c69e-356">The example below defines a conditional compilation symbol `A` and then defines it again.</span></span>
```csharp
#define A
#define A
```

<span data-ttu-id="0c69e-357">A `#undef` "tanımlanmamış bir koşullu derleme sembolün tanımını".</span><span class="sxs-lookup"><span data-stu-id="0c69e-357">A `#undef` may "undefine" a conditional compilation symbol that is not defined.</span></span> <span data-ttu-id="0c69e-358">Aşağıdaki örnekte bir koşullu derleme sembolünü tanımlar `A` ve sonra bu iki kez; ancak tanımsızı ikinci `#undef` hiçbir etkisi yoktur, hala geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-358">The example below defines a conditional compilation symbol `A` and then undefines it twice; although the second `#undef` has no effect, it is still valid.</span></span>
```csharp
#define A
#undef A
#undef A
```

### <a name="conditional-compilation-directives"></a><span data-ttu-id="0c69e-359">Koşullu derleme yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-359">Conditional compilation directives</span></span>

<span data-ttu-id="0c69e-360">Koşullu derleme yönergeleri koşullu olarak dahil etmek veya hariç bir kaynak dosyası bölümleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-360">The conditional compilation directives are used to conditionally include or exclude portions of a source file.</span></span>

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

<span data-ttu-id="0c69e-361">Sözdizimi tarafından belirtildiği gibi koşullu derleme yönergeleri oluşan, sırasıyla kümeleri olarak yazılmış olmalıdır bir `#if` yönergesi, sıfır veya daha fazla `#elif` yönergeleri, sıfır veya bir `#else` yönergesi ve bir `#endif` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="0c69e-361">As indicated by the syntax, conditional compilation directives must be written as sets consisting of, in order, an `#if` directive, zero or more `#elif` directives, zero or one `#else` directive, and an `#endif` directive.</span></span> <span data-ttu-id="0c69e-362">Kaynak kodu koşullu bölümler arasında yönergeleridir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-362">Between the directives are conditional sections of source code.</span></span> <span data-ttu-id="0c69e-363">Her bölümde hemen önceki yönergesi tarafından denetlenir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-363">Each section is controlled by the immediately preceding directive.</span></span> <span data-ttu-id="0c69e-364">Bu yönergeler tam kümeleri form sağlanan koşullu bölüme kendi iç içe geçmiş koşullu derleme yönergeleri içeriyor olabilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-364">A conditional section may itself contain nested conditional compilation directives provided these directives form complete sets.</span></span>

<span data-ttu-id="0c69e-365">A *pp_conditional* kapsanan en fazla birini seçer *conditional_section*s sözcük normal işlenmesi için:</span><span class="sxs-lookup"><span data-stu-id="0c69e-365">A *pp_conditional* selects at most one of the contained *conditional_section*s for normal lexical processing:</span></span>

*  <span data-ttu-id="0c69e-366">*Pp_expression*sn `#if` ve `#elif` yönergeleri bir verir kadar sırayla değerlendirilir `true`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-366">The *pp_expression*s of the `#if` and `#elif` directives are evaluated in order until one yields `true`.</span></span> <span data-ttu-id="0c69e-367">Bir ifade döndürürse `true`, *conditional_section* karşılık gelen yönergesi seçilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-367">If an expression yields `true`, the *conditional_section* of the corresponding directive is selected.</span></span>
*  <span data-ttu-id="0c69e-368">Tüm *pp_expression*s yield `false`ve eğer bir `#else` yönergesi mevcutsa, *conditional_section* , `#else` yönergesi seçilidir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-368">If all *pp_expression*s yield `false`, and if an `#else` directive is present, the *conditional_section* of the `#else` directive is selected.</span></span>
*  <span data-ttu-id="0c69e-369">Aksi takdirde Hayır *conditional_section* seçilir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-369">Otherwise, no *conditional_section* is selected.</span></span>

<span data-ttu-id="0c69e-370">Seçili *conditional_section*herhangi biri, işlenir, normal *input_section*: bölümünde yer alan kaynak kodu için sözcük dilbilgisi uyması gerekir; belirteçleri, kaynaktan oluşturulur kod bölümünde; ve ön işleme yönergeleri bölümünde önceden belirlenmiş etkileri vardır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-370">The selected *conditional_section*, if any, is processed as a normal *input_section*: the source code contained in the section must adhere to the lexical grammar; tokens are generated from the source code in the section; and pre-processing directives in the section have the prescribed effects.</span></span>

<span data-ttu-id="0c69e-371">Kalan *conditional_section*varsa s olarak işlenir *skipped_section*s: ön işleme yönergeleri dışında kaynak bölümünde olmayan esaslarına sözcük dilbilgisi; yok belirteçleri bölümünde kaynak kodundan oluşturulan; ve bölümünde ön işleme yönergeleri sözcüksel olarak doğru olması gerekir, ancak Aksi halde işlenmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-371">The remaining *conditional_section*s, if any, are processed as *skipped_section*s: except for pre-processing directives, the source code in the section need not adhere to the lexical grammar; no tokens are generated from the source code in the section; and pre-processing directives in the section must be lexically correct but are not otherwise processed.</span></span> <span data-ttu-id="0c69e-372">İçinde bir *conditional_section* olarak işleniyor bir *skipped_section*, tüm iç içe geçmiş *conditional_section*s (bulunan iç içe geçmiş `#if`... `#endif` ve `#region`... `#endregion` oluşturur) olarak işlenir *skipped_section*s.</span><span class="sxs-lookup"><span data-stu-id="0c69e-372">Within a *conditional_section* that is being processed as a *skipped_section*, any nested *conditional_section*s (contained in nested `#if`...`#endif` and `#region`...`#endregion` constructs) are also processed as *skipped_section*s.</span></span>

<span data-ttu-id="0c69e-373">Aşağıdaki örnekte, koşullu derleme yönergelerinin iç içe yerleştirebilirsiniz gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="0c69e-373">The following example illustrates how conditional compilation directives can nest:</span></span>
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

<span data-ttu-id="0c69e-374">Ön işleme yönergeleri dışında Atlanan kaynak kodu tabi sözcük analizi değil.</span><span class="sxs-lookup"><span data-stu-id="0c69e-374">Except for pre-processing directives, skipped source code is not subject to lexical analysis.</span></span> <span data-ttu-id="0c69e-375">Örneğin, aşağıdaki Sonlandırılmamış açıklama rağmen geçerli `#else` bölümü:</span><span class="sxs-lookup"><span data-stu-id="0c69e-375">For example, the following is valid despite the unterminated comment in the `#else` section:</span></span>
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

<span data-ttu-id="0c69e-376">Ancak, ön işleme yönergeleri bile Atlanan bölümlerinde kaynak kodu sözcüksel olarak doğru olması için gerekli olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="0c69e-376">Note, however, that pre-processing directives are required to be lexically correct even in skipped sections of source code.</span></span>

<span data-ttu-id="0c69e-377">Çok satırlı giriş öğelerini içinde görüntülendiğinde ön işleme yönergeleri işlenmez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-377">Pre-processing directives are not processed when they appear inside multi-line input elements.</span></span> <span data-ttu-id="0c69e-378">Örneğin, program:</span><span class="sxs-lookup"><span data-stu-id="0c69e-378">For example, the program:</span></span>
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
<span data-ttu-id="0c69e-379">Çıktı sonuçları:</span><span class="sxs-lookup"><span data-stu-id="0c69e-379">results in the output:</span></span>
```
hello,
#if Debug
        world
#else
        Nebraska
#endif
```

<span data-ttu-id="0c69e-380">Özgü durumlarda işlenen ön işleme yönergeleri kümesi üzerinde değerlendirmesi bağlı olabilir *pp_expression*.</span><span class="sxs-lookup"><span data-stu-id="0c69e-380">In peculiar cases, the set of pre-processing directives that is processed might depend on the evaluation of the *pp_expression*.</span></span> <span data-ttu-id="0c69e-381">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0c69e-381">The example:</span></span>
```csharp
#if X
    /*
#else
    /* */ class Q { }
#endif
```
<span data-ttu-id="0c69e-382">her zaman aynı belirteç akışı üretir (`class` `Q` `{` `}`) isteyip istemediğinizi bakılmaksızın `X` tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-382">always produces the same token stream (`class` `Q` `{` `}`), regardless of whether or not `X` is defined.</span></span> <span data-ttu-id="0c69e-383">Varsa `X` olan tanımlanan, yalnızca işlenen yönergeleridir `#if` ve `#endif`nedeniyle çok satırlı açıklama.</span><span class="sxs-lookup"><span data-stu-id="0c69e-383">If `X` is defined, the only processed directives are `#if` and `#endif`, due to the multi-line comment.</span></span> <span data-ttu-id="0c69e-384">Varsa `X` olan tanımlanmamışsa, sonra üç yönergeleri (`#if`, `#else`, `#endif`) yönerge kümesinin bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-384">If `X` is undefined, then three directives (`#if`, `#else`, `#endif`) are part of the directive set.</span></span>

### <a name="diagnostic-directives"></a><span data-ttu-id="0c69e-385">Tanılama yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-385">Diagnostic directives</span></span>

<span data-ttu-id="0c69e-386">Tanılama yönergeleri açıkça hata ve diğer derleme zamanı hataları ve Uyarıları aynı şekilde bildirilir uyarı iletilerini oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-386">The diagnostic directives are used to explicitly generate error and warning messages that are reported in the same way as other compile-time errors and warnings.</span></span>

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

<span data-ttu-id="0c69e-387">Örnek:</span><span class="sxs-lookup"><span data-stu-id="0c69e-387">The example:</span></span>
```csharp
#warning Code review needed before check-in

#if Debug && Retail
    #error A build can't be both debug and retail
#endif

class Test {...}
```
<span data-ttu-id="0c69e-388">her zaman bir uyarı ("iadeden önce gereken kod İnceleme") oluşturur ve bir derleme zamanı hatası oluşturur ("bir derleme hem hata ayıklama hem de perakende bırakılamaz"), koşullu simgeleri `Debug` ve `Retail` tanımlanmış olan iki.</span><span class="sxs-lookup"><span data-stu-id="0c69e-388">always produces a warning ("Code review needed before check-in"), and produces a compile-time error ("A build can't be both debug and retail") if the conditional symbols `Debug` and `Retail` are both defined.</span></span> <span data-ttu-id="0c69e-389">Unutmayın bir *pp_message* rastgele metin içerebilir; özellikle, doğru biçimlendirilmiş belirteçleri, tek tırnak işareti Word gösterildiği içermesi gerekmez `can't`.</span><span class="sxs-lookup"><span data-stu-id="0c69e-389">Note that a *pp_message* can contain arbitrary text; specifically, it need not contain well-formed tokens, as shown by the single quote in the word `can't`.</span></span>

### <a name="region-directives"></a><span data-ttu-id="0c69e-390">Bölge yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-390">Region directives</span></span>

<span data-ttu-id="0c69e-391">Bölge yönergeleri bölgeleri kaynak kodu açıkça işaretlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-391">The region directives are used to explicitly mark regions of source code.</span></span>

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

<span data-ttu-id="0c69e-392">Hiçbir anlam bir bölgeye eklenir; bölgeleri kullanmak için programcı veya otomatikleştirilmiş araçları tarafından kaynak kodun bir bölümünü işaretlemek için içindir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-392">No semantic meaning is attached to a region; regions are intended for use by the programmer or by automated tools to mark a section of source code.</span></span> <span data-ttu-id="0c69e-393">Belirtilen iletiyi bir `#region` veya `#endregion` yönergesi benzer şekilde, hiçbir anlam içeriyor; yalnızca bölge tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-393">The message specified in a `#region` or `#endregion` directive likewise has no semantic meaning; it merely serves to identify the region.</span></span> <span data-ttu-id="0c69e-394">Eşleşen `#region` ve `#endregion` yönergeleri farklı sahip *pp_message*s.</span><span class="sxs-lookup"><span data-stu-id="0c69e-394">Matching `#region` and `#endregion` directives may have different *pp_message*s.</span></span>

<span data-ttu-id="0c69e-395">Bir bölgenin sözcük işleme:</span><span class="sxs-lookup"><span data-stu-id="0c69e-395">The lexical processing of a region:</span></span>
```csharp
#region
...
#endregion
```
<span data-ttu-id="0c69e-396">Koşullu derleme yönergesi formun sözcük işleme için tam olarak karşılık gelmektedir:</span><span class="sxs-lookup"><span data-stu-id="0c69e-396">corresponds exactly to the lexical processing of a conditional compilation directive of the form:</span></span>
```csharp
#if true
...
#endif
```

### <a name="line-directives"></a><span data-ttu-id="0c69e-397">Satır yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-397">Line directives</span></span>

<span data-ttu-id="0c69e-398">Satır yönergeleri satır numaraları ve çıkışında uyarılar ve hatalar gibi derleyici tarafından bildirilen ve arayan bilgileri öznitelikleri tarafından kullanılan kaynak dosya adlarını değiştirmek için kullanılabilir ([arayan bilgileri öznitelikleri](attributes.md#caller-info-attributes)).</span><span class="sxs-lookup"><span data-stu-id="0c69e-398">Line directives may be used to alter the line numbers and source file names that are reported by the compiler in output such as warnings and errors, and that are used by caller info attributes ([Caller info attributes](attributes.md#caller-info-attributes)).</span></span>

<span data-ttu-id="0c69e-399">Satır yönergeleri, bazı diğer metin girişi C# kaynak kodu oluşturun meta programlama araçlarında en yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-399">Line directives are most commonly used in meta-programming tools that generate C# source code from some other text input.</span></span>

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

<span data-ttu-id="0c69e-400">Hiçbir `#line` yönergeleri, derleyici doğru satır numaraları ve kaynak dosya adlarında çıktısını bildirir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-400">When no `#line` directives are present, the compiler reports true line numbers and source file names in its output.</span></span> <span data-ttu-id="0c69e-401">İşleme sırasında bir `#line` içeren yönergesi bir *line_indicator* olmayan `default`, derleyici belirtilen satır numarası (ve dosya adı belirtilmişse) sahip olarak yönergesi sonra satır değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-401">When processing a `#line` directive that includes a *line_indicator* that is not `default`, the compiler treats the line after the directive as having the given line number (and file name, if specified).</span></span>

<span data-ttu-id="0c69e-402">A `#line default` yönergesi, tüm önceki #line direktifleri etkisini tersine çevirir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-402">A `#line default` directive reverses the effect of all preceding #line directives.</span></span> <span data-ttu-id="0c69e-403">Tam olarak Hayır olarak ise sonraki satırları, doğru satır bilgilerini derleyici raporları `#line` yönergeleri işlenmemiş.</span><span class="sxs-lookup"><span data-stu-id="0c69e-403">The compiler reports true line information for subsequent lines, precisely as if no `#line` directives had been processed.</span></span>

<span data-ttu-id="0c69e-404">A `#line hidden` yönergesi dosya üzerinde hiçbir etkisi ve satır numaraları hatası bildirdi iletileri, ancak kaynağı seviyesinde hata ayıklamayı etkilemez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-404">A `#line hidden` directive has no effect on the file and line numbers reported in error messages, but does affect source level debugging.</span></span> <span data-ttu-id="0c69e-405">Hata ayıklama sırasında arasındaki tüm satırları bir `#line hidden` yönergesi ve sonraki `#line` yönergesi (olmayan `#line hidden`) sahip satır numarası bilgisi.</span><span class="sxs-lookup"><span data-stu-id="0c69e-405">When debugging, all lines between a `#line hidden` directive and the subsequent `#line` directive (that is not `#line hidden`) have no line number information.</span></span> <span data-ttu-id="0c69e-406">Kod hata ayıklayıcı ile Adımlama, bu satırları tamamen atlanacak.</span><span class="sxs-lookup"><span data-stu-id="0c69e-406">When stepping through code in the debugger, these lines will be skipped entirely.</span></span>

<span data-ttu-id="0c69e-407">Unutmayın bir *file_name* kaçış karakterleri işlenmez; normal dize sabit değeri farklıdır "`\`" karakter yalnızca içinde bir sıradan bir ters eğik çizgi karakteri atayan bir *file_name*.</span><span class="sxs-lookup"><span data-stu-id="0c69e-407">Note that a *file_name* differs from a regular string literal in that escape characters are not processed; the "`\`" character simply designates an ordinary backslash character within a *file_name*.</span></span>

### <a name="pragma-directives"></a><span data-ttu-id="0c69e-408">Pragma yönergeleri</span><span class="sxs-lookup"><span data-stu-id="0c69e-408">Pragma directives</span></span>

<span data-ttu-id="0c69e-409">`#pragma` Ön işleme yönergesi, isteğe bağlı bağlamsal bilgi derleyiciye belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0c69e-409">The `#pragma` preprocessing directive is used to specify optional contextual information to the compiler.</span></span> <span data-ttu-id="0c69e-410">Bilgi sağlanan bir `#pragma` yönergesi hiçbir zaman programı semantiğini değiştirir.</span><span class="sxs-lookup"><span data-stu-id="0c69e-410">The information supplied in a `#pragma` directive will never change program semantics.</span></span>

```antlr
pp_pragma
    : whitespace? '#' whitespace? 'pragma' whitespace pragma_body pp_new_line
    ;

pragma_body
    : pragma_warning_body
    ;
```

<span data-ttu-id="0c69e-411">C# sağlar `#pragma` Derleyici uyarılarını kontrol etmek için yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-411">C# provides `#pragma` directives to control compiler warnings.</span></span> <span data-ttu-id="0c69e-412">Dil gelecek sürümlerinde ek dahil `#pragma` yönergeleri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-412">Future versions of the language may include additional `#pragma` directives.</span></span> <span data-ttu-id="0c69e-413">Diğer C# Derleyicileri birlikte çalışabilirliği sağlamak için Microsoft C# derleyicisi için bilinmeyen derleme hatası kesmez `#pragma` yönergeleri; ancak uyarı oluşturma gibi yönergeleri yapın.</span><span class="sxs-lookup"><span data-stu-id="0c69e-413">To ensure interoperability with other C# compilers, the Microsoft C# compiler does not issue compilation errors for unknown `#pragma` directives; such directives do however generate warnings.</span></span>

#### <a name="pragma-warning"></a><span data-ttu-id="0c69e-414">Pragma Uyarısı</span><span class="sxs-lookup"><span data-stu-id="0c69e-414">Pragma warning</span></span>

<span data-ttu-id="0c69e-415">`#pragma warning` Yönergesi devre dışı bırakın veya tümünü geri yüklemek için kullanılan veya belirli bir uyarı kümesini sonraki program metni bir derleme sırasında iletileri.</span><span class="sxs-lookup"><span data-stu-id="0c69e-415">The `#pragma warning` directive is used to disable or restore all or a particular set of warning messages during compilation of the subsequent program text.</span></span>

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

<span data-ttu-id="0c69e-416">A `#pragma warning` uyarı listesini atlayan yönergesi, tüm uyarıları etkiler.</span><span class="sxs-lookup"><span data-stu-id="0c69e-416">A `#pragma warning` directive that omits the warning list affects all warnings.</span></span> <span data-ttu-id="0c69e-417">A `#pragma warning` yönergesini içeren bir uyarı listesi listesinde belirtilen uyarıları etkiler.</span><span class="sxs-lookup"><span data-stu-id="0c69e-417">A `#pragma warning` directive the includes a warning list affects only those warnings that are specified in the list.</span></span>

<span data-ttu-id="0c69e-418">A `#pragma warning disable` yönerge devre dışı bırakır tüm veya belirli uyarıları kümesi.</span><span class="sxs-lookup"><span data-stu-id="0c69e-418">A `#pragma warning disable` directive disables all or the given set of warnings.</span></span>

<span data-ttu-id="0c69e-419">A `#pragma warning restore` tüm yönerge geri yükler veya derleme biriminde başında yürürlükte olan durumu uyarıları verilen kümesi.</span><span class="sxs-lookup"><span data-stu-id="0c69e-419">A `#pragma warning restore` directive restores all or the given set of warnings to the state that was in effect at the beginning of the compilation unit.</span></span> <span data-ttu-id="0c69e-420">Belirli bir uyarı harici olarak devre dışı bırakılmışsa unutmayın, `#pragma warning restore` (olup tüm veya belirli uyarı), uyarı yeniden izin vermez.</span><span class="sxs-lookup"><span data-stu-id="0c69e-420">Note that if a particular warning was disabled externally, a `#pragma warning restore` (whether for all or the specific warning) will not re-enable that warning.</span></span>

<span data-ttu-id="0c69e-421">Aşağıdaki örnek, kullanımını gösterir. `#pragma warning` geçici olarak uyarı devre dışı bırakmak için geçersiz zaman üyeleri başvuru, Microsoft C# derleyicisi uyarı numarasını kullanarak bildirilen.</span><span class="sxs-lookup"><span data-stu-id="0c69e-421">The following example shows use of `#pragma warning` to temporarily disable the warning reported when obsoleted members are referenced, using the warning number from the Microsoft C# compiler.</span></span>
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
