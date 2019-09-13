---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912432"
---
# <a name="documentation-comments"></a><span data-ttu-id="fc36d-101">Belge açıklamaları</span><span class="sxs-lookup"><span data-stu-id="fc36d-101">Documentation comments</span></span>

<span data-ttu-id="fc36d-102">C#programcıların, XML metni içeren özel bir açıklama söz dizimini kullanarak kodlarını belgetabilmesi için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="fc36d-103">Kaynak kodu dosyalarında, belirli bir biçime sahip olan Yorumlar, bir aracı bu açıklamalardan ve kaynak kodu öğelerinden önce XML oluşturacak şekilde yönlendirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="fc36d-104">Bu söz dizimini kullanan açıklamalara ***belge açıklamaları***denir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="fc36d-105">Kullanıcı tanımlı bir türün (bir sınıf, temsilci veya arabirim gibi) ya da bir üyenin (alan, olay, özellik veya yöntem gibi) hemen önce gelmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="fc36d-106">XML oluşturma aracına ***belge Oluşturucu***denir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="fc36d-107">(Bu Oluşturucu, C# derleyicinin kendisi olabilir, ancak olması gerekmez.) Belge Oluşturucu tarafından üretilen çıktıya ***belge dosyası***denir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="fc36d-108">Belge dosyası bir ***belge görüntüleyicisine***giriş olarak kullanılır; tür bilgilerinin ve ilişkili belgelerinin bir dizi görsel görüntüsünü oluşturmak için tasarlanan bir araç.</span><span class="sxs-lookup"><span data-stu-id="fc36d-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="fc36d-109">Bu belirtim belge açıklamalarında kullanılacak bir etiket kümesi önerir, ancak bu etiketlerin kullanımı gerekli değildir, ancak doğru biçimlendirilmiş XML kuralları izlenirken, istenirse diğer Etiketler de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="fc36d-110">Giriş</span><span class="sxs-lookup"><span data-stu-id="fc36d-110">Introduction</span></span>

<span data-ttu-id="fc36d-111">Özel bir forma sahip olan Yorumlar, bir aracı bu açıklamalardan ve kaynak kod öğelerinden önce XML oluşturacak şekilde yönlendirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="fc36d-112">Bu yorumlar, üç eğik çizgi (`///`) ile başlayan tek satırlık açıklamalardır ve eğik çizgiyle ve iki yıldızlı (`/**`) ile başlayan sınırlandırılmış açıklamalardır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="fc36d-113">Bunlar, bir Kullanıcı tanımlı türün (bir sınıf, temsilci veya arabirim gibi) ya da ek açıklama eklenen bir üyenin (bir alan, olay, özellik veya yöntem gibi) hemen önce gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="fc36d-114">Öznitelik bölümleri ([öznitelik belirtimi](attributes.md#attribute-specification)) bildirimlerin bir parçası olarak değerlendirilir, bu nedenle belge açıklamalarının bir tür veya üyeye uygulanan özniteliklerin önünde olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="fc36d-115">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="fc36d-116">Bir *single_line_doc_comment*içinde, her bir *single_line_doc_comment*için geçerli *single_line_doc_comment*bitişik olan `///` karakterleri izleyen bir *boşluk* karakteri varsa, buXML çıktısına boşluk karakteri dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="fc36d-117">Ayrılmış bir belge açıklamasında, ikinci satırdaki ilk boşluk olmayan karakter bir yıldız işareti ise ve isteğe bağlı boşluk karakterlerinden aynı desenli ve bir yıldız karakteri ayrılmış-belge-açıklama içindeki her satırın başlangıcında tekrarlanırsa, sonra yinelenen modelin karakterleri XML çıktısına dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="fc36d-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="fc36d-118">Bu düzende, sonra yıldız karakteri ve daha önce boşluk karakterleri bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="fc36d-119">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-119">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>
///
public class Point 
{
    /// <summary>method <c>draw</c> renders the point.</summary>
    void draw() {...}
}
```

<span data-ttu-id="fc36d-120">Belge açıklamalarındaki metin, XML kurallarına göre düzgün şekilde oluşturulmalıdır (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="fc36d-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="fc36d-121">XML hatalı biçimlendirilmişse bir uyarı oluşturulur ve belge dosyası bir hata ile karşılaşıldığını söyleyen bir açıklama içerir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="fc36d-122">Geliştiriciler kendi etiket kümesini oluşturmak için ücretsiz olsa da önerilen [etiketlerde](documentation-comments.md#recommended-tags)önerilen bir küme tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="fc36d-123">Önerilen etiketlerden bazılarının özel anlamları vardır:</span><span class="sxs-lookup"><span data-stu-id="fc36d-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="fc36d-124">Etiketi `<param>` , parametreleri tanımlamakta kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="fc36d-125">Bu tür bir etiket kullanılırsa, belge Oluşturucu belirtilen parametrenin var olduğunu ve tüm parametrelerin belge açıklamalarında açıklananlarının doğrulanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="fc36d-126">Bu doğrulama başarısız olursa, belge Oluşturucu bir uyarı verir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="fc36d-127">`cref` Özniteliği bir kod öğesine başvuru sağlamak için herhangi bir etikete iliştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="fc36d-128">Belge oluşturucunun Bu kod öğesinin var olduğunu doğrulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="fc36d-129">Doğrulama başarısız olursa, belge Oluşturucu bir uyarı verir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="fc36d-130">Bir `cref` öznitelikte açıklanan adı ararken belge oluşturucunun, kaynak kodu içinde görüntülenen `using` deyimlere göre ad alanı görünürlüğüne göre olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="fc36d-131">Genel olan kod öğeleri için, normal genel sözdizimi (yani, "`List<T>`") geçersiz XML oluşturduğundan kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="fc36d-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="fc36d-132">Ayraçlar (`List{T}`Yani, "") veya XML kaçış söz dizimi (yani, "`List&lt;T&gt;`") yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="fc36d-133">Etiket `<summary>` , bir tür veya üyeyle ilgili ek bilgileri göstermek için bir belge Görüntüleyicisi tarafından kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="fc36d-134">Etiketi `<include>` , bir dış XML dosyasındaki bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="fc36d-135">Belge dosyasının tür ve Üyeler hakkında tam bilgi sağlamadığına ve (örneğin, herhangi bir tür bilgisi içermediğinden) emin olun.</span><span class="sxs-lookup"><span data-stu-id="fc36d-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="fc36d-136">Bir tür veya üye hakkında daha fazla bilgi almak için, belge dosyası gerçek tür veya üyede yansıma ile birlikte kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="fc36d-137">Önerilen Etiketler</span><span class="sxs-lookup"><span data-stu-id="fc36d-137">Recommended tags</span></span>

<span data-ttu-id="fc36d-138">Belge oluşturucunun, XML kurallarına göre geçerli olan herhangi bir etiketi kabul etmesi ve işlemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="fc36d-139">Aşağıdaki Etiketler kullanıcı belgelerinde yaygın olarak kullanılan işlevleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="fc36d-140">(Kuşkusuz, diğer Etiketler mümkündür.)</span><span class="sxs-lookup"><span data-stu-id="fc36d-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="fc36d-141">__Etiket__</span><span class="sxs-lookup"><span data-stu-id="fc36d-141">__Tag__</span></span>          | <span data-ttu-id="fc36d-142">__Kısmı__</span><span class="sxs-lookup"><span data-stu-id="fc36d-142">__Section__</span></span>                                            | <span data-ttu-id="fc36d-143">__Amaç__</span><span class="sxs-lookup"><span data-stu-id="fc36d-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="fc36d-144">Kod benzeri yazı tipinde metin ayarlama</span><span class="sxs-lookup"><span data-stu-id="fc36d-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="fc36d-145">Kaynak kodu veya program çıkışının bir veya daha fazla satırını ayarlama</span><span class="sxs-lookup"><span data-stu-id="fc36d-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="fc36d-146">Bir örnek belirtin</span><span class="sxs-lookup"><span data-stu-id="fc36d-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="fc36d-147">Bir yöntemin oluşturabilecek özel durumları tanımlar</span><span class="sxs-lookup"><span data-stu-id="fc36d-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="fc36d-148">Dış dosyadan XML içerir</span><span class="sxs-lookup"><span data-stu-id="fc36d-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="fc36d-149">Liste veya tablo oluşturma</span><span class="sxs-lookup"><span data-stu-id="fc36d-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="fc36d-150">Yapıya metin eklenmesine izin ver</span><span class="sxs-lookup"><span data-stu-id="fc36d-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="fc36d-151">Yöntem veya Oluşturucu için bir parametre tanımlama</span><span class="sxs-lookup"><span data-stu-id="fc36d-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="fc36d-152">Bir sözcüğün parametre adı olduğunu belirler</span><span class="sxs-lookup"><span data-stu-id="fc36d-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="fc36d-153">Üyenin güvenlik erişilebilirliğini belgeleme</span><span class="sxs-lookup"><span data-stu-id="fc36d-153">Document the security accessibility of a member</span></span>        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | <span data-ttu-id="fc36d-154">Bir tür hakkındaki ek bilgileri açıkla</span><span class="sxs-lookup"><span data-stu-id="fc36d-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="fc36d-155">Bir yöntemin dönüş değerini açıkla</span><span class="sxs-lookup"><span data-stu-id="fc36d-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="fc36d-156">Bir bağlantı belirtin</span><span class="sxs-lookup"><span data-stu-id="fc36d-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="fc36d-157">Ayrıca bkz. bir girdi oluştur</span><span class="sxs-lookup"><span data-stu-id="fc36d-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="fc36d-158">Türü veya bir türün üyesini açıklama</span><span class="sxs-lookup"><span data-stu-id="fc36d-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="fc36d-159">Bir özelliği açıkla</span><span class="sxs-lookup"><span data-stu-id="fc36d-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="fc36d-160">Genel tür parametresini açıkla</span><span class="sxs-lookup"><span data-stu-id="fc36d-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="fc36d-161">Bir sözcüğün tür parametre adı olduğunu belirler</span><span class="sxs-lookup"><span data-stu-id="fc36d-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="fc36d-162">Bu etiket, bir açıklama içindeki bir metin parçasının bir kod bloğu için kullanılan gibi özel bir yazı tipinde ayarlanması gerektiğini belirten bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="fc36d-163">Gerçek kod satırları için ( `<code>` [`<code>`](documentation-comments.md#code)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc36d-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="fc36d-164">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="fc36d-165">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="fc36d-166">Bu etiket, bazı özel yazı tiplerinde kaynak kodu veya program çıkışının bir veya daha fazla satırını ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="fc36d-167">Anlatıcı olarak küçük kod parçaları için ( `<c>` [`<c>`](documentation-comments.md#c)) kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc36d-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="fc36d-168">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="fc36d-169">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-169">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// <example>For example:
/// <code>
///    Point p = new Point(3,5);
///    p.Translate(-1,3);
/// </code>
/// results in <c>p</c>'s having the value (2,8).
/// </example>
/// </summary>

public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}   
```

### `<example>`

<span data-ttu-id="fc36d-170">Bu etiket, bir metodun veya diğer kitaplık üyesinin nasıl kullanılabileceğini belirtmek için bir açıklama içindeki örnek koda izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="fc36d-171">Normalde, bu da etiketin `<code>` ([`<code>`](documentation-comments.md#code)) kullanımını da kapsar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="fc36d-172">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="fc36d-173">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-173">__Example:__</span></span>

<span data-ttu-id="fc36d-174">Bir `<code>` örnek[`<code>`](documentation-comments.md#code)için bkz. ().</span><span class="sxs-lookup"><span data-stu-id="fc36d-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="fc36d-175">Bu etiket, bir yöntemin oluşturmakta olduğu özel durumları belgelemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="fc36d-176">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="fc36d-177">Burada</span><span class="sxs-lookup"><span data-stu-id="fc36d-177">where</span></span>

* <span data-ttu-id="fc36d-178">`member`üyenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-178">`member` is the name of a member.</span></span> <span data-ttu-id="fc36d-179">Belge Oluşturucu, belirtilen üyenin var olduğunu denetler ve belge dosyasında `member` kurallı öğe adına çevirir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="fc36d-180">`description`, özel durumun oluşturulduğu durumların açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="fc36d-181">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-181">__Example:__</span></span>

```csharp
public class DataBaseOperations
{
    /// <exception cref="MasterFileFormatCorruptException"></exception>
    /// <exception cref="MasterFileLockedOpenException"></exception>
    public static void ReadRecord(int flag) {
        if (flag == 1)
            throw new MasterFileFormatCorruptException();
        else if (flag == 2)
            throw new MasterFileLockedOpenException();
        // ...
    } 
}
```

### `<include>`

<span data-ttu-id="fc36d-182">Bu etiket, kaynak kodu dosyasının dışında bir XML belgesinden bilgi dahil etmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="fc36d-183">Dış dosya iyi biçimlendirilmiş bir XML belgesi olmalıdır ve bu belgeye hangi XML ekleneceğini belirtmek için bu belgeye bir XPath ifadesi uygulanmış olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="fc36d-184">Daha `<include>` sonra etiket, dış belgedeki seçili XML ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="fc36d-185">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="fc36d-186">Burada</span><span class="sxs-lookup"><span data-stu-id="fc36d-186">where</span></span>

* <span data-ttu-id="fc36d-187">`filename`, harici bir XML dosyasının dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="fc36d-188">Dosya adı, içerme etiketini içeren dosyaya göre yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="fc36d-189">`xpath`Dış XML dosyasındaki XML 'den bazılarını seçen bir XPath ifadesidir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="fc36d-190">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-190">__Example:__</span></span>

<span data-ttu-id="fc36d-191">Kaynak kodu şöyle bir bildirim içeriyorsa:</span><span class="sxs-lookup"><span data-stu-id="fc36d-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="fc36d-192">"docs. xml" dış dosyası aşağıdaki içeriğe sahiptir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-192">and the external file "docs.xml" had the following contents:</span></span>

```xml
<?xml version="1.0"?>
<extradoc>
  <class name="IntList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
  <class name="StringList">
     <summary>
        Contains a list of integers.
     </summary>
  </class>
</extradoc>
```

<span data-ttu-id="fc36d-193">daha sonra aynı belgeler, kaynak kodu içeren çıktıdır:</span><span class="sxs-lookup"><span data-stu-id="fc36d-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="fc36d-194">Bu etiket bir liste veya öğe tablosu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="fc36d-195">Bir tablo ya da `<listheader>` tanım listesinin başlık satırını tanımlamak için bir blok içerebilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="fc36d-196">(Bir tablo tanımlarken, yalnızca başlık için `term` bir girdinin sağlanması gerekir.)</span><span class="sxs-lookup"><span data-stu-id="fc36d-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="fc36d-197">Listedeki her öğe bir `<item>` blokla belirtilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="fc36d-198">Bir tanım listesi oluştururken, her ikisi `term` de `description` belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="fc36d-199">Ancak, bir tablo, madde işaretli liste veya numaralandırılmış liste için yalnızca `description` gerekli olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="fc36d-200">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-200">__Syntax:__</span></span>

```xml
<list type="bullet" | "number" | "table">
   <listheader>
      <term>term</term>
      <description>*description*</description>
   </listheader>
   <item>
      <term>term</term>
      <description>*description*</description>
   </item>
    ...
   <item>
      <term>term</term>
      <description>description</description>
   </item>
</list>
```

<span data-ttu-id="fc36d-201">Burada</span><span class="sxs-lookup"><span data-stu-id="fc36d-201">where</span></span>

* <span data-ttu-id="fc36d-202">`term`tanımı içinde `description`olan tanımlama terimi.</span><span class="sxs-lookup"><span data-stu-id="fc36d-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="fc36d-203">`description`, bir madde işareti veya numaralandırılmış listedeki bir öğe ya da bir `term`tanımı.</span><span class="sxs-lookup"><span data-stu-id="fc36d-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="fc36d-204">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-204">__Example:__</span></span>

```csharp
public class MyClass
{
    /// <summary>Here is an example of a bulleted list:
    /// <list type="bullet">
    /// <item>
    /// <description>Item 1.</description>
    /// </item>
    /// <item>
    /// <description>Item 2.</description>
    /// </item>
    /// </list>
    /// </summary>
    public static void Main () {
        // ...
    }
}
```

### `<para>`

<span data-ttu-id="fc36d-205">Bu etiket, `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) veya `<returns>` ([`<returns>`](documentation-comments.md#returns)) gibi diğer etiketlerin içinde kullanım içindir ve yapının metne eklenmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-205">This tag is for use inside other tags, such as `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="fc36d-206">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="fc36d-207">paragrafın `content` metni nerede.</span><span class="sxs-lookup"><span data-stu-id="fc36d-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="fc36d-208">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-208">__Example:__</span></span>

```csharp
/// <summary>This is the entry point of the Point class testing program.
/// <para>This program tests each method and operator, and
/// is intended to be run after any non-trivial maintenance has
/// been performed on the Point class.</para></summary>
public static void Main() {
    // ...
}
```

### `<param>`

<span data-ttu-id="fc36d-209">Bu etiket bir yöntem, Oluşturucu veya Dizin Oluşturucu için bir parametre belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="fc36d-210">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="fc36d-211">Burada</span><span class="sxs-lookup"><span data-stu-id="fc36d-211">where</span></span>

* <span data-ttu-id="fc36d-212">`name`parametrenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="fc36d-213">`description`, parametresinin bir açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="fc36d-214">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-214">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <param name="xor">the new x-coordinate.</param>
/// <param name="yor">the new y-coordinate.</param>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<paramref>`

<span data-ttu-id="fc36d-215">Bu etiket bir sözcüğün bir parametre olduğunu göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="fc36d-216">Belge dosyası bu parametreyi farklı bir şekilde biçimlendirmek için işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="fc36d-217">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="fc36d-218">`name` parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="fc36d-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="fc36d-219">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-219">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to
///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
/// <param name="xor">the new Point's x-coordinate.</param>
/// <param name="yor">the new Point's y-coordinate.</param>

public Point(int xor, int yor) {
    X = xor;
    Y = yor;
}
```

### `<permission>`

<span data-ttu-id="fc36d-220">Bu etiket bir üyenin güvenlik erişilebilirliğinin belgelenme olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="fc36d-221">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="fc36d-222">Burada</span><span class="sxs-lookup"><span data-stu-id="fc36d-222">where</span></span>

* <span data-ttu-id="fc36d-223">`member`üyenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-223">`member` is the name of a member.</span></span> <span data-ttu-id="fc36d-224">Belge Oluşturucu verilen kod öğesinin var olduğunu denetler ve belge *dosyasındaki öğeyi kurallı öğe adına çevirir.*</span><span class="sxs-lookup"><span data-stu-id="fc36d-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="fc36d-225">`description`, üyeye erişimin bir açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="fc36d-226">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

<span data-ttu-id="fc36d-227">Bu etiket, bir tür hakkındaki ek bilgileri belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="fc36d-228">(Türünün `<summary>` kendisini[`<summary>`](documentation-comments.md#summary)ve bir türün üyelerini anlatmak için () kullanın.)</span><span class="sxs-lookup"><span data-stu-id="fc36d-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="fc36d-229">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-229">__Syntax:__</span></span>

```xml
<remarks>description</remarks>
```

<span data-ttu-id="fc36d-230">Burada `description` , açıklamanın metni bulunur.</span><span class="sxs-lookup"><span data-stu-id="fc36d-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="fc36d-231">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remarks>Uses polar coordinates</remarks>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="fc36d-232">Bu etiket bir yöntemin dönüş değerini tanımlamakta kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="fc36d-233">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="fc36d-234">`description` , dönüş değerinin bir açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="fc36d-235">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="fc36d-236">Bu etiket, bir bağlantının metin içinde belirtilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="fc36d-237">Ayrıca `<seealso>` bkz[`<seealso>`](documentation-comments.md#seealso). bölümünde görünecek metni göstermek için () kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc36d-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="fc36d-238">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="fc36d-239">`member` üyenin adı.</span><span class="sxs-lookup"><span data-stu-id="fc36d-239">where `member` is the name of a member.</span></span> <span data-ttu-id="fc36d-240">Belge Oluşturucu verilen kod öğesinin var olduğunu denetler ve oluşturulan belgeler dosyasındaki öğe adına *üye* olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="fc36d-241">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-241">__Example:__</span></span>

```csharp
/// <summary>This method changes the point's location to
///    the given coordinates.</summary>
/// <see cref="Translate"/>
public void Move(int xor, int yor) {
    X = xor;
    Y = yor;
}

/// <summary>This method changes the point's location by
///    the given x- and y-offsets.
/// </summary>
/// <see cref="Move"/>
public void Translate(int xor, int yor) {
    X += xor;
    Y += yor;
}
```

### `<seealso>`

<span data-ttu-id="fc36d-242">Bu etiket, Ayrıca bkz. bölümü için bir girişin oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="fc36d-243">Metin `<see>` içinden[`<see>`](documentation-comments.md#see)bir bağlantı belirtmek için () kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc36d-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="fc36d-244">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="fc36d-245">`member` üyenin adı.</span><span class="sxs-lookup"><span data-stu-id="fc36d-245">where `member` is the name of a member.</span></span> <span data-ttu-id="fc36d-246">Belge Oluşturucu verilen kod öğesinin var olduğunu denetler ve oluşturulan belgeler dosyasındaki öğe adına *üye* olarak değişir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="fc36d-247">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-247">__Example:__</span></span>

```csharp
/// <summary>This method determines whether two Points have the same
///    location.</summary>
/// <seealso cref="operator=="/>
/// <seealso cref="operator!="/>
public override bool Equals(object o) {
    // ...
}
```

### `<summary>`

Bu etiket, bir türü veya bir türün bir üyesini anlatmak için kullanılabilir. <span data-ttu-id="fc36d-249">Türün `<remarks>` kendisini[`<remarks>`](documentation-comments.md#remarks)anlatmak için () kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc36d-249">Use `<remarks>` ([`<remarks>`](documentation-comments.md#remarks)) to describe the type itself.</span></span>

<span data-ttu-id="fc36d-250">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="fc36d-251">`description` , türün veya üyenin bir özetidir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="fc36d-252">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="fc36d-253">Bu etiket, bir özelliğin açıklanalmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="fc36d-254">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="fc36d-255">`property description` , özelliği için bir açıklamadır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="fc36d-256">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="fc36d-257">Bu etiket, bir sınıf, yapı, arabirim, temsilci veya yöntem için genel bir tür parametresi tanımlamakta kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="fc36d-258">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="fc36d-259">Burada `name` tür parametresinin adıdır ve `description` açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="fc36d-260">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="fc36d-261">Bu etiket, bir sözcüğün bir tür parametresi olduğunu göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="fc36d-262">Belge dosyası bu tür parametresini farklı bir şekilde biçimlendirmek için işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="fc36d-263">__Sözdizimi__</span><span class="sxs-lookup"><span data-stu-id="fc36d-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="fc36d-264">Burada `name` tür parametresinin adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="fc36d-265">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="fc36d-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="fc36d-266">Belge dosyası işleniyor</span><span class="sxs-lookup"><span data-stu-id="fc36d-266">Processing the documentation file</span></span>

<span data-ttu-id="fc36d-267">Belge Oluşturucu bir belge açıklamasıyla etiketlenmiş kaynak kodundaki her öğe için bir KIMLIK dizesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fc36d-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="fc36d-268">Bu KIMLIK dizesi bir kaynak öğeyi benzersiz şekilde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="fc36d-269">Belge Görüntüleyicisi, belgelerin uygulandığı karşılık gelen meta veri/yansıma öğesini tanımlamak için bir KIMLIK dizesi kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="fc36d-270">Belge dosyası kaynak kodun hiyerarşik bir temsili değildir; Bunun yerine, her öğe için oluşturulmuş bir KIMLIK dizesi olan düz bir liste olur.</span><span class="sxs-lookup"><span data-stu-id="fc36d-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="fc36d-271">KIMLIK dize biçimi</span><span class="sxs-lookup"><span data-stu-id="fc36d-271">ID string format</span></span>

<span data-ttu-id="fc36d-272">Belge Oluşturucu, KIMLIK dizelerini oluşturduğunda aşağıdaki kuralları sunar:</span><span class="sxs-lookup"><span data-stu-id="fc36d-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="fc36d-273">Dizeye boşluk yerleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="fc36d-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="fc36d-274">Dizenin ilk bölümü, belgelendirilmiş üye türünü, tek bir karakter ve ardından iki nokta üst üste ile tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="fc36d-275">Aşağıdaki üye türleri tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="fc36d-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="fc36d-276">__İnde__</span><span class="sxs-lookup"><span data-stu-id="fc36d-276">__Character__</span></span> | <span data-ttu-id="fc36d-277">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="fc36d-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="fc36d-278">E</span><span class="sxs-lookup"><span data-stu-id="fc36d-278">E</span></span>             | <span data-ttu-id="fc36d-279">Olay</span><span class="sxs-lookup"><span data-stu-id="fc36d-279">Event</span></span>                                                       |
   | <span data-ttu-id="fc36d-280">F</span><span class="sxs-lookup"><span data-stu-id="fc36d-280">F</span></span>             | <span data-ttu-id="fc36d-281">Alan</span><span class="sxs-lookup"><span data-stu-id="fc36d-281">Field</span></span>                                                       |
   | <span data-ttu-id="fc36d-282">M</span><span class="sxs-lookup"><span data-stu-id="fc36d-282">M</span></span>             | <span data-ttu-id="fc36d-283">Yöntem (oluşturucular, Yıkıcılar ve işleçler dahil)</span><span class="sxs-lookup"><span data-stu-id="fc36d-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="fc36d-284">N</span><span class="sxs-lookup"><span data-stu-id="fc36d-284">N</span></span>             | <span data-ttu-id="fc36d-285">Ad Alanı</span><span class="sxs-lookup"><span data-stu-id="fc36d-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="fc36d-286">P</span><span class="sxs-lookup"><span data-stu-id="fc36d-286">P</span></span>             | <span data-ttu-id="fc36d-287">Özellik (Dizin oluşturucular dahil)</span><span class="sxs-lookup"><span data-stu-id="fc36d-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="fc36d-288">T</span><span class="sxs-lookup"><span data-stu-id="fc36d-288">T</span></span>             | <span data-ttu-id="fc36d-289">Tür (class, Delegate, Enum, Interface ve struct gibi)</span><span class="sxs-lookup"><span data-stu-id="fc36d-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="fc36d-290">!</span><span class="sxs-lookup"><span data-stu-id="fc36d-290">!</span></span>             | <span data-ttu-id="fc36d-291">Hata dizesi; dizenin geri kalanı hata hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="fc36d-292">Örneğin, belge Oluşturucu çözülemeyen bağlantılar için hata bilgileri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fc36d-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="fc36d-293">Dizenin ikinci bölümü, ad alanının köküden başlayarak öğesinin tam nitelikli adıdır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="fc36d-294">Öğenin adı, kapsayan tür (ler) ve ad alanı noktalarla ayrılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="fc36d-295">Öğenin adında nokta varsa, bunlar karakter olarak `#(U+0023)` değiştirilirler.</span><span class="sxs-lookup"><span data-stu-id="fc36d-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="fc36d-296">(Bir öğe adında bu karakterin olmadığı varsayılır.)</span><span class="sxs-lookup"><span data-stu-id="fc36d-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="fc36d-297">Bağımsız değişkenlerle Yöntemler ve özellikler için bağımsız değişken listesi, parantez içine alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="fc36d-298">Bağımsız değişkenler olmadan, parantezler atlanır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="fc36d-299">Bağımsız değişkenler virgülle ayrılır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-299">The arguments are separated by commas.</span></span> <span data-ttu-id="fc36d-300">Her bağımsız değişkenin kodlaması, aşağıdaki gibi bir CLı imzasıyla aynıdır:</span><span class="sxs-lookup"><span data-stu-id="fc36d-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="fc36d-301">Bağımsız değişkenler, tam adına göre, aşağıdaki şekilde değiştirilen belge adlarıyla temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="fc36d-302">Genel türleri temsil eden bağımsız değişkenler bir eklenmiş `` ` `` (backtick) karaktere ve ardından tür parametrelerinin sayısına sahiptir</span><span class="sxs-lookup"><span data-stu-id="fc36d-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="fc36d-303">`out` `@` Veya değiştiricisinesahipbağımsızdeğişkenler,türadlarınıtakipedenbir.`ref`</span><span class="sxs-lookup"><span data-stu-id="fc36d-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="fc36d-304">Değer veya aracılığıyla `params` geçirilen bağımsız değişkenlerin özel bir gösterimi yoktur.</span><span class="sxs-lookup"><span data-stu-id="fc36d-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="fc36d-305">Diziler olan bağımsız değişkenler, virgül sayısının `[lowerbound:size, ... , lowerbound:size]` derece daha az bir olduğu ve bilinen her boyutun alt sınırları ve boyutunun ondalık olarak temsil edildiği şekilde temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="fc36d-306">Daha düşük bir sınır veya boyut belirtilmemişse, atlanır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="fc36d-307">Belirli bir boyutun alt sınırı ve boyutu atlanmışsa `:` , de atlanır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="fc36d-308">Pürüzlü Diziler, düzey başına bir `[]` ile temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="fc36d-309">Void dışındaki işaretçi türlerine sahip bağımsız değişkenler, `*` aşağıdaki tür adı kullanılarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="fc36d-310">Void işaretçisi, bir tür adı `System.Void`kullanılarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="fc36d-311">Türler üzerinde tanımlanan genel tür parametrelerine başvuran bağımsız değişkenler, `` ` `` (backtick) karakteri ve ardından tür parametresinin sıfır tabanlı dizini kullanılarak kodlanır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="fc36d-312">Metotlarda tanımlanan genel tür parametrelerini kullanan bağımsız değişkenler türler için ``` `` ``` `` ` `` kullanılan bir çift yönlü kullanır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="fc36d-313">Oluşturulan Genel türlere başvuran bağımsız değişkenler genel türü `{`kullanılarak kodlanır, ardından, ve ardından bir tür bağımsız değişken `}`türü ve sonra gelen virgülle ayrılmış bir liste gelir.</span><span class="sxs-lookup"><span data-stu-id="fc36d-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="fc36d-314">KIMLIK dizesi örnekleri</span><span class="sxs-lookup"><span data-stu-id="fc36d-314">ID string examples</span></span>

<span data-ttu-id="fc36d-315">Aşağıdaki örneklerde her biri bir C# kod parçasını ve bir belge yorumu olan her bir kaynak ÖĞEDEN üretilen kimlik dizesiyle birlikte gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="fc36d-316">Türler, tam nitelikli adı kullanılarak temsil edilir ve genel bilgilerle Genişletilebilir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

   ```csharp
   enum Color { Red, Blue, Green }

   namespace Acme
   {
       interface IProcess {...}

       struct ValueType {...}

       class Widget: IProcess
       {
           public class NestedClass {...}
           public interface IMenuItem {...}
           public delegate void Del(int i);
           public enum Direction { North, South, East, West }
       }

       class MyList<T>
       {
           class Helper<U,V> {...}
       }
   }

   "T:Color"
   "T:Acme.IProcess"
   "T:Acme.ValueType"
   "T:Acme.Widget"
   "T:Acme.Widget.NestedClass"
   "T:Acme.Widget.IMenuItem"
   "T:Acme.Widget.Del"
   "T:Acme.Widget.Direction"
   "T:Acme.MyList`1"
   "T:Acme.MyList`1.Helper`2"
   ```

*  <span data-ttu-id="fc36d-317">Alanlar, tam adlarıyla temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-317">Fields are represented by their fully qualified name:</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           private int total;
       }
   
       class Widget: IProcess
       {
           public class NestedClass
           {
               private int value;
           }
   
           private string message;
           private static Color defaultColor;
           private const double PI = 3.14159;
           protected readonly double monthlyAverage;
           private long[] array1;
           private Widget[,] array2;
           private unsafe int *pCount;
           private unsafe float **ppValues;
       }
   }

   "F:Acme.ValueType.total"
   "F:Acme.Widget.NestedClass.value"
   "F:Acme.Widget.message"
   "F:Acme.Widget.defaultColor"
   "F:Acme.Widget.PI"
   "F:Acme.Widget.monthlyAverage"
   "F:Acme.Widget.array1"
   "F:Acme.Widget.array2"
   "F:Acme.Widget.pCount"
   "F:Acme.Widget.ppValues"
   ```

*  <span data-ttu-id="fc36d-318">Kurucu.</span><span class="sxs-lookup"><span data-stu-id="fc36d-318">Constructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           static Widget() {...}
           public Widget() {...}
           public Widget(string s) {...}
       }
   }

   "M:Acme.Widget.#cctor"
   "M:Acme.Widget.#ctor"
   "M:Acme.Widget.#ctor(System.String)"
   ```

*  <span data-ttu-id="fc36d-319">Yıkıcılar.</span><span class="sxs-lookup"><span data-stu-id="fc36d-319">Destructors.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           ~Widget() {...}
       }
   }
   
   "M:Acme.Widget.Finalize"
   ```

*  <span data-ttu-id="fc36d-320">Yöntem.</span><span class="sxs-lookup"><span data-stu-id="fc36d-320">Methods.</span></span>

   ```csharp
   namespace Acme
   {
       struct ValueType
       {
           public void M(int i) {...}
       }

       class Widget: IProcess
       {
           public class NestedClass
           {
               public void M(int i) {...}
           }

           public static void M0() {...}
           public void M1(char c, out float f, ref ValueType v) {...}
           public void M2(short[] x1, int[,] x2, long[][] x3) {...}
           public void M3(long[][] x3, Widget[][,,] x4) {...}
           public unsafe void M4(char *pc, Color **pf) {...}
           public unsafe void M5(void *pv, double *[][,] pd) {...}
           public void M6(int i, params object[] args) {...}
       }

       class MyList<T>
       {
           public void Test(T t) { }
       }

       class UseList
       {
           public void Process(MyList<int> list) { }
           public MyList<T> GetValues<T>(T inputValue) { return null; }
       }
   }

   "M:Acme.ValueType.M(System.Int32)"
   "M:Acme.Widget.NestedClass.M(System.Int32)"
   "M:Acme.Widget.M0"
   "M:Acme.Widget.M1(System.Char,System.Single@,Acme.ValueType@)"
   "M:Acme.Widget.M2(System.Int16[],System.Int32[0:,0:],System.Int64[][])"
   "M:Acme.Widget.M3(System.Int64[][],Acme.Widget[0:,0:,0:][])"
   "M:Acme.Widget.M4(System.Char*,Color**)"
   "M:Acme.Widget.M5(System.Void*,System.Double*[0:,0:][])"
   "M:Acme.Widget.M6(System.Int32,System.Object[])"
   "M:Acme.MyList`1.Test(`0)"
   "M:Acme.UseList.Process(Acme.MyList{System.Int32})"
   "M:Acme.UseList.GetValues``(``0)"
   ```

*  <span data-ttu-id="fc36d-321">Özellikler ve Dizin oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="fc36d-321">Properties and indexers.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public int Width { get {...} set {...} }
           public int this[int i] { get {...} set {...} }
           public int this[string s, int i] { get {...} set {...} }
       }
   }

   "P:Acme.Widget.Width"
   "P:Acme.Widget.Item(System.Int32)"
   "P:Acme.Widget.Item(System.String,System.Int32)"
   ```

*  <span data-ttu-id="fc36d-322">Olayları.</span><span class="sxs-lookup"><span data-stu-id="fc36d-322">Events.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public event Del AnEvent;
       }
   }

   "E:Acme.Widget.AnEvent"
   ```

*  <span data-ttu-id="fc36d-323">Birli İşleçler.</span><span class="sxs-lookup"><span data-stu-id="fc36d-323">Unary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_UnaryPlus(Acme.Widget)"
   ```

   <span data-ttu-id="fc36d-324">Kullanılan birli işleç işlev adlarının tamamı aşağıdaki gibidir: `op_UnaryPlus`, `op_UnaryNegation`, `op_Increment` `op_OnesComplement` `op_LogicalNot`,,, `op_Decrement`, `op_True`ve `op_False`.</span><span class="sxs-lookup"><span data-stu-id="fc36d-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="fc36d-325">İkili işleçler.</span><span class="sxs-lookup"><span data-stu-id="fc36d-325">Binary operators.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static Widget operator+(Widget x1, Widget x2) {...}
       }
   }

   "M:Acme.Widget.op_Addition(Acme.Widget,Acme.Widget)"
   ```

   <span data-ttu-id="fc36d-326">`op_Addition`Kullanılan ikili işleç işlev adlarının tamamen kümesi aşağıdaki gibidir:, `op_BitwiseOr` `op_ExclusiveOr` `op_BitwiseAnd` `op_Subtraction`, `op_Multiply` `op_Modulus` `op_Division`,,,,,, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, ,,`op_LessThan`ve .`op_GreaterThanOrEqual` `op_LessThanOrEqual` `op_GreaterThan`</span><span class="sxs-lookup"><span data-stu-id="fc36d-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="fc36d-327">Dönüştürme işleçleri sonunda bir "`~`" ve ardından dönüş türü vardır.</span><span class="sxs-lookup"><span data-stu-id="fc36d-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

   ```csharp
   namespace Acme
   {
       class Widget: IProcess
       {
           public static explicit operator int(Widget x) {...}
           public static implicit operator long(Widget x) {...}
       }
   }

   "M:Acme.Widget.op_Explicit(Acme.Widget)~System.Int32"
   "M:Acme.Widget.op_Implicit(Acme.Widget)~System.Int64"
   ```

## <a name="an-example"></a><span data-ttu-id="fc36d-328">Örnek</span><span class="sxs-lookup"><span data-stu-id="fc36d-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="fc36d-329">C#kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="fc36d-329">C# source code</span></span>

<span data-ttu-id="fc36d-330">Aşağıdaki örnekte bir `Point` sınıfın kaynak kodu gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-330">The following example shows the source code of a `Point` class:</span></span>

```csharp
namespace Graphics
{

/// <summary>Class <c>Point</c> models a point in a two-dimensional plane.
/// </summary>
public class Point 
{

    /// <summary>Instance variable <c>x</c> represents the point's
    ///    x-coordinate.</summary>
    private int x;

    /// <summary>Instance variable <c>y</c> represents the point's
    ///    y-coordinate.</summary>
    private int y;

    /// <value>Property <c>X</c> represents the point's x-coordinate.</value>
    public int X
    {
        get { return x; }
        set { x = value; }
    }

    /// <value>Property <c>Y</c> represents the point's y-coordinate.</value>
    public int Y
    {
        get { return y; }
        set { y = value; }
    }

    /// <summary>This constructor initializes the new Point to
    ///    (0,0).</summary>
    public Point() : this(0,0) {}

    /// <summary>This constructor initializes the new Point to
    ///    (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
    /// <param><c>xor</c> is the new Point's x-coordinate.</param>
    /// <param><c>yor</c> is the new Point's y-coordinate.</param>
    public Point(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location to
    ///    the given coordinates.</summary>
    /// <param><c>xor</c> is the new x-coordinate.</param>
    /// <param><c>yor</c> is the new y-coordinate.</param>
    /// <see cref="Translate"/>
    public void Move(int xor, int yor) {
        X = xor;
        Y = yor;
    }

    /// <summary>This method changes the point's location by
    ///    the given x- and y-offsets.
    /// <example>For example:
    /// <code>
    ///    Point p = new Point(3,5);
    ///    p.Translate(-1,3);
    /// </code>
    /// results in <c>p</c>'s having the value (2,8).
    /// </example>
    /// </summary>
    /// <param><c>xor</c> is the relative x-offset.</param>
    /// <param><c>yor</c> is the relative y-offset.</param>
    /// <see cref="Move"/>
    public void Translate(int xor, int yor) {
        X += xor;
        Y += yor;
    }

    /// <summary>This method determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>o</c> is the object to be compared to the current object.
    /// </param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="operator=="/>
    /// <seealso cref="operator!="/>
    public override bool Equals(object o) {
        if (o == null) {
            return false;
        }

        if (this == o) {
            return true;
        }

        if (GetType() == o.GetType()) {
            Point p = (Point)o;
            return (X == p.X) && (Y == p.Y);
        }
        return false;
    }

    /// <summary>Report a point's location as a string.</summary>
    /// <returns>A string representing a point's location, in the form (x,y),
    ///    without any leading, training, or embedded whitespace.</returns>
    public override string ToString() {
        return "(" + X + "," + Y + ")";
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points have the same location and they have
    ///    the exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator!="/>
    public static bool operator==(Point p1, Point p2) {
        if ((object)p1 == null || (object)p2 == null) {
            return false;
        }

        if (p1.GetType() == p2.GetType()) {
            return (p1.X == p2.X) && (p1.Y == p2.Y);
        }

        return false;
    }

    /// <summary>This operator determines whether two Points have the same
    ///    location.</summary>
    /// <param><c>p1</c> is the first Point to be compared.</param>
    /// <param><c>p2</c> is the second Point to be compared.</param>
    /// <returns>True if the Points do not have the same location and the
    ///    exact same type; otherwise, false.</returns>
    /// <seealso cref="Equals"/>
    /// <seealso cref="operator=="/>
    public static bool operator!=(Point p1, Point p2) {
        return !(p1 == p2);
    }

    /// <summary>This is the entry point of the Point class testing
    /// program.
    /// <para>This program tests each method and operator, and
    /// is intended to be run after any non-trivial maintenance has
    /// been performed on the Point class.</para></summary>
    public static void Main() {
        // class test code goes here
    }
}
}
```

### <a name="resulting-xml"></a><span data-ttu-id="fc36d-331">Elde edilen XML</span><span class="sxs-lookup"><span data-stu-id="fc36d-331">Resulting XML</span></span>

<span data-ttu-id="fc36d-332">Aşağıda gösterildiği gibi, sınıf `Point`için kaynak kodu verildiğinde bir belge Oluşturucu tarafından oluşturulan çıktı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="fc36d-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>Point</name>
    </assembly>
    <members>
        <member name="T:Graphics.Point">
            <summary>Class <c>Point</c> models a point in a two-dimensional
            plane.
            </summary>
        </member>

        <member name="F:Graphics.Point.x">
            <summary>Instance variable <c>x</c> represents the point's
            x-coordinate.</summary>
        </member>

        <member name="F:Graphics.Point.y">
            <summary>Instance variable <c>y</c> represents the point's
            y-coordinate.</summary>
        </member>

        <member name="M:Graphics.Point.#ctor">
            <summary>This constructor initializes the new Point to
        (0,0).</summary>
        </member>

        <member name="M:Graphics.Point.#ctor(System.Int32,System.Int32)">
            <summary>This constructor initializes the new Point to
            (<paramref name="xor"/>,<paramref name="yor"/>).</summary>
            <param><c>xor</c> is the new Point's x-coordinate.</param>
            <param><c>yor</c> is the new Point's y-coordinate.</param>
        </member>

        <member name="M:Graphics.Point.Move(System.Int32,System.Int32)">
            <summary>This method changes the point's location to
            the given coordinates.</summary>
            <param><c>xor</c> is the new x-coordinate.</param>
            <param><c>yor</c> is the new y-coordinate.</param>
            <see cref="M:Graphics.Point.Translate(System.Int32,System.Int32)"/>
        </member>

        <member
            name="M:Graphics.Point.Translate(System.Int32,System.Int32)">
            <summary>This method changes the point's location by
            the given x- and y-offsets.
            <example>For example:
            <code>
            Point p = new Point(3,5);
            p.Translate(-1,3);
            </code>
            results in <c>p</c>'s having the value (2,8).
            </example>
            </summary>
            <param><c>xor</c> is the relative x-offset.</param>
            <param><c>yor</c> is the relative y-offset.</param>
            <see cref="M:Graphics.Point.Move(System.Int32,System.Int32)"/>
        </member>

        <member name="M:Graphics.Point.Equals(System.Object)">
            <summary>This method determines whether two Points have the same
            location.</summary>
            <param><c>o</c> is the object to be compared to the current
            object.
            </param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
            <seealso
      cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.ToString">
            <summary>Report a point's location as a string.</summary>
            <returns>A string representing a point's location, in the form
            (x,y),
            without any leading, training, or embedded whitespace.</returns>
        </member>

        <member
       name="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points have the same location and they have
            the exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
     cref="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member
      name="M:Graphics.Point.op_Inequality(Graphics.Point,Graphics.Point)">
            <summary>This operator determines whether two Points have the
            same
            location.</summary>
            <param><c>p1</c> is the first Point to be compared.</param>
            <param><c>p2</c> is the second Point to be compared.</param>
            <returns>True if the Points do not have the same location and
            the
            exact same type; otherwise, false.</returns>
            <seealso cref="M:Graphics.Point.Equals(System.Object)"/>
            <seealso
      cref="M:Graphics.Point.op_Equality(Graphics.Point,Graphics.Point)"/>
        </member>

        <member name="M:Graphics.Point.Main">
            <summary>This is the entry point of the Point class testing
            program.
            <para>This program tests each method and operator, and
            is intended to be run after any non-trivial maintenance has
            been performed on the Point class.</para></summary>
        </member>

        <member name="P:Graphics.Point.X">
            <value>Property <c>X</c> represents the point's
            x-coordinate.</value>
        </member>

        <member name="P:Graphics.Point.Y">
            <value>Property <c>Y</c> represents the point's
            y-coordinate.</value>
        </member>
    </members>
</doc>
```
