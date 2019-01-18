---
ms.openlocfilehash: c9f8417dc68153f02ceb72bb1d51f3615f3c4961
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54272052"
---
# <a name="documentation-comments"></a><span data-ttu-id="e485c-101">Belge açıklamaları</span><span class="sxs-lookup"><span data-stu-id="e485c-101">Documentation comments</span></span>

<span data-ttu-id="e485c-102">C# programcıları kodlarını XML metni içeren bir özel açıklama sözdizimi kullanılarak belge için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-102">C# provides a mechanism for programmers to document their code using a special comment syntax that contains XML text.</span></span> <span data-ttu-id="e485c-103">Kaynak kodu dosyalarında açıklamaları belirli bir biçime sahip bir aracı bu yorumlar ve bunlar önce kaynak kodu öğesi XML üretmek için yönlendirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-103">In source code files, comments having a certain form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="e485c-104">Bu söz dizimi çağrılır yorumları kullanarak ***belge açıklamaları***.</span><span class="sxs-lookup"><span data-stu-id="e485c-104">Comments using such syntax are called ***documentation comments***.</span></span> <span data-ttu-id="e485c-105">Hemen bir kullanıcı tanımlı türü (örneğin, bir sınıf, temsilci veya arabirimi) ya da bir üyesi (örneğin, bir alan, olay, özelliği veya yöntemi) gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="e485c-105">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method).</span></span> <span data-ttu-id="e485c-106">XML oluşturma aracı olarak adlandırılır ***belgeleri Oluşturucu***.</span><span class="sxs-lookup"><span data-stu-id="e485c-106">The XML generation tool is called the ***documentation generator***.</span></span> <span data-ttu-id="e485c-107">(Bu oluşturucunun olabilir, ancak olması gerekmez, C# derleyicisi kendisini.) Belgeleri Oluşturucu tarafından üretilen çıkış adlı ***soubor dokumentace***.</span><span class="sxs-lookup"><span data-stu-id="e485c-107">(This generator could be, but need not be, the C# compiler itself.) The output produced by the documentation generator is called the ***documentation file***.</span></span> <span data-ttu-id="e485c-108">Soubor dokumentace girdi olarak kullanılan bir ***belgeleri Görüntüleyicisi***; bir tür bilgilerini ve ilişkili belgelerini görünümünü çeşit üretmek için hedeflenen bir aracı.</span><span class="sxs-lookup"><span data-stu-id="e485c-108">A documentation file is used as input to a ***documentation viewer***; a tool intended to produce some sort of visual display of type information and its associated documentation.</span></span>

<span data-ttu-id="e485c-109">Belge açıklamaları içinde kullanılacak etiketleri kümesi bu belirtim önerir ancak bu etiketleri kullanımı gerekli değildir ve uzun doğru biçimlendirilmiş XML kuralları ardından diğer etiketler isterseniz, kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-109">This specification suggests a set of tags to be used in documentation comments, but use of these tags is not required, and other tags may be used if desired, as long the rules of well-formed XML are followed.</span></span>

## <a name="introduction"></a><span data-ttu-id="e485c-110">Giriş</span><span class="sxs-lookup"><span data-stu-id="e485c-110">Introduction</span></span>

<span data-ttu-id="e485c-111">Özel bir biçime sahip açıklamaları, XML, bu yorumlar ve bunlar önce kaynak kod öğeleri oluşturmak için bir araç yönlendirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-111">Comments having a special form can be used to direct a tool to produce XML from those comments and the source code elements, which they precede.</span></span> <span data-ttu-id="e485c-112">Bu tür açıklamaları üç eğik çizgi ile başlayan tek satırlık açıklamaları bulunmaktadır (`///`), ya bir eğik çizgi ve iki yıldız ile başlayan açıklamaları ayrılmış (`/**`).</span><span class="sxs-lookup"><span data-stu-id="e485c-112">Such comments are single-line comments that start with three slashes (`///`), or delimited comments that start with a slash and two stars (`/**`).</span></span> <span data-ttu-id="e485c-113">Hemen bir kullanıcı tanımlı türü (örneğin, bir sınıf, temsilci veya arabirimi) ya da bunlar ek açıklama bir üye (örneğin, bir alan, olay, özelliği veya yöntemi) gelmelidir.</span><span class="sxs-lookup"><span data-stu-id="e485c-113">They must immediately precede a user-defined type (such as a class, delegate, or interface) or a member (such as a field, event, property, or method) that they annotate.</span></span> <span data-ttu-id="e485c-114">Öznitelik bölümleri ([öznitelik belirtimi](attributes.md#attribute-specification)) belge yorumlarını bir türe veya üyeye uygulanan öznitelikleri gelmelidir için bildirimler, bir parçası olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-114">Attribute sections ([Attribute specification](attributes.md#attribute-specification)) are considered part of declarations, so documentation comments must precede attributes applied to a type or member.</span></span>

<span data-ttu-id="e485c-115">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-115">__Syntax:__</span></span>

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

<span data-ttu-id="e485c-116">İçinde bir *single_line_doc_comment*, yoksa bir *boşluk* karakter aşağıdaki `///` her bir karakter *single_line_doc_comment*bitişik s Geçerli *single_line_doc_comment*, ardından, *boşluk* XML çıkış karakteri dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="e485c-116">In a *single_line_doc_comment*, if there is a *whitespace* character following the `///` characters on each of the *single_line_doc_comment*s adjacent to the current *single_line_doc_comment*, then that *whitespace* character is not included in the XML output.</span></span>

<span data-ttu-id="e485c-117">İkinci satırdaki ilk boşluk olmayan karakter bir yıldız işareti ve aynı deseni isteğe bağlı boşluk karakterleri ve bir yıldız işareti ise sınırlı-doc-açıklama, her birinin sınırlı-doc-açıklama içindeki satır başında yinelenir, ardından karakterlerinden yinelenen desen XML çıktısında dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="e485c-117">In a delimited-doc-comment, if the first non-whitespace character on the second line is an asterisk and the same pattern of optional whitespace characters and an asterisk character is repeated at the beginning of each of the line within the delimited-doc-comment, then the characters of the repeated pattern are not included in the XML output.</span></span> <span data-ttu-id="e485c-118">Desen, sonra yanı sıra, yıldız işareti önce boşluk karakterleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-118">The pattern may include whitespace characters after, as well as before, the asterisk character.</span></span>

<span data-ttu-id="e485c-119">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-119">__Example:__</span></span>

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

<span data-ttu-id="e485c-120">Kurallara göre XML belge açıklamaları içindeki metin iyi biçimlendirilmiş olmalıdır (https://www.w3.org/TR/REC-xml).</span><span class="sxs-lookup"><span data-stu-id="e485c-120">The text within documentation comments must be well formed according to the rules of XML (https://www.w3.org/TR/REC-xml).</span></span> <span data-ttu-id="e485c-121">Doğru biçimlendirilmiş XML olgu olması durumunda bir uyarı oluşturulur ve soubor dokumentace bir hatayla karşılaşıldı belirten bir açıklama içerir.</span><span class="sxs-lookup"><span data-stu-id="e485c-121">If the XML is ill formed, a warning is generated and the documentation file will contain a comment saying that an error was encountered.</span></span>

<span data-ttu-id="e485c-122">Geliştiriciler kendi kümesi etiketleri oluşturmak ücretsiz olsa da, önerilen kümesi tanımlanan [önerilen etiketler](documentation-comments.md#recommended-tags).</span><span class="sxs-lookup"><span data-stu-id="e485c-122">Although developers are free to create their own set of tags, a recommended set is defined in [Recommended tags](documentation-comments.md#recommended-tags).</span></span> <span data-ttu-id="e485c-123">Önerilen etiketler bazıları, özel anlamları vardır:</span><span class="sxs-lookup"><span data-stu-id="e485c-123">Some of the recommended tags have special meanings:</span></span>

*  <span data-ttu-id="e485c-124">`<param>` Etiketi parametreler tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-124">The `<param>` tag is used to describe parameters.</span></span> <span data-ttu-id="e485c-125">Böyle bir etiket kullandıysanız, belgeleri Oluşturucu belirtilen parametre var olduğundan ve tüm parametreleri belgeleri açıklamaları açıklanan doğrulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e485c-125">If such a tag is used, the documentation generator must verify that the specified parameter exists and that all parameters are described in documentation comments.</span></span> <span data-ttu-id="e485c-126">Bu tür doğrulama başarısız olursa, belgeleri Oluşturucu bir uyarı verir.</span><span class="sxs-lookup"><span data-stu-id="e485c-126">If such verification fails, the documentation generator issues a warning.</span></span>
*  <span data-ttu-id="e485c-127">`cref` Öznitelik, bir kod öğesi başvuru sağlamak için herhangi bir etiket eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-127">The `cref` attribute can be attached to any tag to provide a reference to a code element.</span></span> <span data-ttu-id="e485c-128">Belgeleri Oluşturucu, bu kod öğesi var olduğunu doğrulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e485c-128">The documentation generator must verify that this code element exists.</span></span> <span data-ttu-id="e485c-129">Belgeleri Oluşturucu, doğrulama başarısız olursa bir uyarı verir.</span><span class="sxs-lookup"><span data-stu-id="e485c-129">If the verification fails, the documentation generator issues a warning.</span></span> <span data-ttu-id="e485c-130">Bir ad açıklanan için aranırken bir `cref` özniteliği belgeleri Oluşturucu gerekir saygı göre ad alanı görünürlük `using` kaynak kodu içinde görünen deyimleri.</span><span class="sxs-lookup"><span data-stu-id="e485c-130">When looking for a name described in a `cref` attribute, the documentation generator must respect namespace visibility according to `using` statements appearing within the source code.</span></span> <span data-ttu-id="e485c-131">Genel, kod öğeleri normal genel sözdizimi için (diğer bir deyişle, "`List<T>`") kullanılamaz çünkü geçersiz XML üretir.</span><span class="sxs-lookup"><span data-stu-id="e485c-131">For code elements that are generic, the normal generic syntax (that is, "`List<T>`") cannot be used because it produces invalid XML.</span></span> <span data-ttu-id="e485c-132">Kaşlı ayraçlar yerine kullanılabilir (diğer bir deyişle, "`List{T}`"), veya XML kaçış sözdiziminin kullanılabilir (diğer bir deyişle, "`List&lt;T&gt;`").</span><span class="sxs-lookup"><span data-stu-id="e485c-132">Braces can be used instead of brackets (that is, "`List{T}`"), or the XML escape syntax can be used (that is, "`List&lt;T&gt;`").</span></span>
*  <span data-ttu-id="e485c-133">`<summary>` Etiketi bir tür veya üyeyle ilgili ek bilgileri görüntülemek için bir belge Görüntüleyici tarafından kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e485c-133">The `<summary>` tag is intended to be used by a documentation viewer to display additional information about a type or member.</span></span>
*  <span data-ttu-id="e485c-134">`<include>` Etiketi harici bir XML dosyasından bilgiler içerir.</span><span class="sxs-lookup"><span data-stu-id="e485c-134">The `<include>` tag includes information from an external XML file.</span></span>

<span data-ttu-id="e485c-135">Soubor dokumentace tür ve üyeler hakkında tam bilgi sağlamaz dikkatle unutmayın (örneğin, bunu herhangi bir tür bilgilerini içermiyor).</span><span class="sxs-lookup"><span data-stu-id="e485c-135">Note carefully that the documentation file does not provide full information about the type and members (for example, it does not contain any type information).</span></span> <span data-ttu-id="e485c-136">Böyle bir tür veya üye bilgilerini almak için soubor dokumentace gerçek türe veya üyeye yansıma ile birlikte kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-136">To get such information about a type or member, the documentation file must be used in conjunction with reflection on the actual type or member.</span></span>

## <a name="recommended-tags"></a><span data-ttu-id="e485c-137">Önerilen etiketler</span><span class="sxs-lookup"><span data-stu-id="e485c-137">Recommended tags</span></span>

<span data-ttu-id="e485c-138">Belgeleri Oluşturucu kabul et ve XML kurallarına göre geçerli herhangi bir etiket işleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="e485c-138">The documentation generator must accept and process any tag that is valid according to the rules of XML.</span></span> <span data-ttu-id="e485c-139">Aşağıdaki kullanıcı belgeleri, yaygın olarak kullanılan işlevler sunar.</span><span class="sxs-lookup"><span data-stu-id="e485c-139">The following tags provide commonly used functionality in user documentation.</span></span> <span data-ttu-id="e485c-140">(Kuşkusuz, diğer etiketlerin mümkündür.)</span><span class="sxs-lookup"><span data-stu-id="e485c-140">(Of course, other tags are possible.)</span></span>


| <span data-ttu-id="e485c-141">__Etiket__</span><span class="sxs-lookup"><span data-stu-id="e485c-141">__Tag__</span></span>          | <span data-ttu-id="e485c-142">__Bölüm__</span><span class="sxs-lookup"><span data-stu-id="e485c-142">__Section__</span></span>                                            | <span data-ttu-id="e485c-143">__Amaç__</span><span class="sxs-lookup"><span data-stu-id="e485c-143">__Purpose__</span></span>                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | <span data-ttu-id="e485c-144">Metin bir kod benzeri yazı tipini Ayarla</span><span class="sxs-lookup"><span data-stu-id="e485c-144">Set text in a code-like font</span></span>                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | <span data-ttu-id="e485c-145">Bir veya daha fazla satır, kaynak kodu veya program çıktısı</span><span class="sxs-lookup"><span data-stu-id="e485c-145">Set one or more lines of source code or program output</span></span> |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | <span data-ttu-id="e485c-146">Bir örnek belirtin</span><span class="sxs-lookup"><span data-stu-id="e485c-146">Indicate an example</span></span>                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | <span data-ttu-id="e485c-147">Bir yöntem oluşturabilecek özel durumları tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-147">Identifies the exceptions a method can throw</span></span>           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | <span data-ttu-id="e485c-148">Dış dosya XML'den içerir</span><span class="sxs-lookup"><span data-stu-id="e485c-148">Includes XML from an external file</span></span>                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | <span data-ttu-id="e485c-149">Bir liste veya tablo oluşturma</span><span class="sxs-lookup"><span data-stu-id="e485c-149">Create a list or table</span></span>                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | <span data-ttu-id="e485c-150">İzin vermek için metin eklenecek yapısı</span><span class="sxs-lookup"><span data-stu-id="e485c-150">Permit structure to be added to text</span></span>                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | <span data-ttu-id="e485c-151">Yöntem veya oluşturucu için bir parametre açıklayın</span><span class="sxs-lookup"><span data-stu-id="e485c-151">Describe a parameter for a method or constructor</span></span>       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | <span data-ttu-id="e485c-152">Bir Word'ün bir parametre adı olduğunu belirleyin</span><span class="sxs-lookup"><span data-stu-id="e485c-152">Identify that a word is a parameter name</span></span>               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | <span data-ttu-id="e485c-153">Belge bir üye güvenlik erişilebilirliği</span><span class="sxs-lookup"><span data-stu-id="e485c-153">Document the security accessibility of a member</span></span>        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | <span data-ttu-id="e485c-154">Bir türü hakkında ek bilgiler açıklanmaktadır</span><span class="sxs-lookup"><span data-stu-id="e485c-154">Describe additional information about a type</span></span>           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | <span data-ttu-id="e485c-155">Bir yöntemin dönüş değerini açıklayın</span><span class="sxs-lookup"><span data-stu-id="e485c-155">Describe the return value of a method</span></span>                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | <span data-ttu-id="e485c-156">Bir bağlantı belirtme</span><span class="sxs-lookup"><span data-stu-id="e485c-156">Specify a link</span></span>                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | <span data-ttu-id="e485c-157">Ayrıca bkz: girişi oluştur</span><span class="sxs-lookup"><span data-stu-id="e485c-157">Generate a See Also entry</span></span>                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | <span data-ttu-id="e485c-158">Bir tür veya üye türü açıklar.</span><span class="sxs-lookup"><span data-stu-id="e485c-158">Describe a type or a member of a type</span></span>                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | <span data-ttu-id="e485c-159">Bir özelliği açıklamaktadır</span><span class="sxs-lookup"><span data-stu-id="e485c-159">Describe a property</span></span>                                    |
| `<typeparam>`    |                                                        | <span data-ttu-id="e485c-160">Genel tür parametresi açıklayın</span><span class="sxs-lookup"><span data-stu-id="e485c-160">Describe a generic type parameter</span></span>                      |
| `<typeparamref>` |                                                        | <span data-ttu-id="e485c-161">Bir sözcük tür parametre adı olduğunu belirleyin</span><span class="sxs-lookup"><span data-stu-id="e485c-161">Identify that a word is a type parameter name</span></span>          |

### `<c>`

<span data-ttu-id="e485c-162">Bu etiket, açıklama içindeki metnin bir parçası gibi bir kod bloğu için kullanılan özel bir yazı tipi ayarlanmalıdır belirtmek için bir mekanizma sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-162">This tag provides a mechanism to indicate that a fragment of text within a description should be set in a special font such as that used for a block of code.</span></span> <span data-ttu-id="e485c-163">Gerçek kod satırı için kullanarak `<code>` ([`<code>`](documentation-comments.md#code)).</span><span class="sxs-lookup"><span data-stu-id="e485c-163">For lines of actual code, use `<code>` ([`<code>`](documentation-comments.md#code)).</span></span>

<span data-ttu-id="e485c-164">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-164">__Syntax:__</span></span>

```xml
<c>text</c>
```

<span data-ttu-id="e485c-165">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-165">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

<span data-ttu-id="e485c-166">Bu etiket, bir veya daha fazla kaynak kodu veya program çıkış satırlarını bazı özel bir yazı tipinde ayarlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-166">This tag is used to set one or more lines of source code or program output in some special font.</span></span> <span data-ttu-id="e485c-167">Anlatımlı küçük kod parçaları için kullanmak `<c>` ([`<c>`](documentation-comments.md#c)).</span><span class="sxs-lookup"><span data-stu-id="e485c-167">For small code fragments in narrative, use `<c>` ([`<c>`](documentation-comments.md#c)).</span></span>

<span data-ttu-id="e485c-168">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-168">__Syntax:__</span></span>

```xml
<code>source code or program output</code>
```

<span data-ttu-id="e485c-169">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-169">__Example:__</span></span>

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

<span data-ttu-id="e485c-170">Bu etiket, örnek kod içinde nasıl bir yöntem veya diğer kitaplık üyesini kullanılabilir belirtmek için bir açıklama sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-170">This tag allows example code within a comment, to specify how a method or other library member may be used.</span></span> <span data-ttu-id="e485c-171">Normalde, bu da etiketinin kapsayacaktır `<code>` ([`<code>`](documentation-comments.md#code)) de.</span><span class="sxs-lookup"><span data-stu-id="e485c-171">Ordinarily, this would also involve use of the tag `<code>` ([`<code>`](documentation-comments.md#code)) as well.</span></span>

<span data-ttu-id="e485c-172">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-172">__Syntax:__</span></span>

```xml
<example>description</example>
```

<span data-ttu-id="e485c-173">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-173">__Example:__</span></span>

<span data-ttu-id="e485c-174">Bkz: `<code>` ([`<code>`](documentation-comments.md#code)) örneği.</span><span class="sxs-lookup"><span data-stu-id="e485c-174">See `<code>` ([`<code>`](documentation-comments.md#code)) for an example.</span></span>

### `<exception>`

<span data-ttu-id="e485c-175">Bu etiket, bir yöntem oluşturabilecek özel durumları belge için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-175">This tag provides a way to document the exceptions a method can throw.</span></span>

<span data-ttu-id="e485c-176">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-176">__Syntax:__</span></span>

```xml
<exception cref="member">description</exception>
```

<span data-ttu-id="e485c-177">Burada</span><span class="sxs-lookup"><span data-stu-id="e485c-177">where</span></span>

* <span data-ttu-id="e485c-178">`member` bir üyenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-178">`member` is the name of a member.</span></span> <span data-ttu-id="e485c-179">Belirtilen üyeyi var ve çevirir belgeleri Oluşturucu denetler `member` soubor dokumentace kurallı öğesi adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-179">The documentation generator checks that the given member exists and translates `member` to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="e485c-180">`description` özel durumun koşullar açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-180">`description` is a description of the circumstances in which the exception is thrown.</span></span>

<span data-ttu-id="e485c-181">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-181">__Example:__</span></span>

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

<span data-ttu-id="e485c-182">Bu etiket için kaynak kod dosyası dışındaki bir XML belgesi bilgileri dahil olmak üzere sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-182">This tag allows including information from an XML document that is external to the source code file.</span></span> <span data-ttu-id="e485c-183">Dış dosya doğru biçimlendirilmiş bir XML belgesi olmalıdır ve bir XPath ifadesi eklemek için bu belgede hangi XML'den belirtmek için bu belgeye uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e485c-183">The external file must be a well-formed XML document, and an XPath expression is applied to that document to specify what XML from that document to include.</span></span> <span data-ttu-id="e485c-184">`<include>` Etiketi dış belgedeki seçilen XML ile sonra değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-184">The `<include>` tag is then replaced with the selected XML from the external document.</span></span>

<span data-ttu-id="e485c-185">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-185">__Syntax:__</span></span>

```
<include file="filename" path="xpath" />
```

<span data-ttu-id="e485c-186">Burada</span><span class="sxs-lookup"><span data-stu-id="e485c-186">where</span></span>

* <span data-ttu-id="e485c-187">`filename` Dış XML dosyasının dosya adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-187">`filename` is the file name of an external XML file.</span></span> <span data-ttu-id="e485c-188">Dosya adı include etiketiyle içeren dosyayı göre yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="e485c-188">The file name is interpreted relative to the file that contains the include tag.</span></span>
* <span data-ttu-id="e485c-189">`xpath` Dış XML dosyasında XML bazıları seçen bir XPath ifadesidir.</span><span class="sxs-lookup"><span data-stu-id="e485c-189">`xpath` is an XPath expression that selects some of the XML in the external XML file.</span></span>

<span data-ttu-id="e485c-190">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-190">__Example:__</span></span>

<span data-ttu-id="e485c-191">Kaynak kodu gibi bir bildirim içeriyorsa:</span><span class="sxs-lookup"><span data-stu-id="e485c-191">If the source code contained a declaration like:</span></span>

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

<span data-ttu-id="e485c-192">ve aşağıdaki içeriği dış dosya "docs.xml" sahipti:</span><span class="sxs-lookup"><span data-stu-id="e485c-192">and the external file "docs.xml" had the following contents:</span></span>

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

<span data-ttu-id="e485c-193">Kaynak kodu içeriyorsa gibi aynı belgeleri çıktı olacaktır:</span><span class="sxs-lookup"><span data-stu-id="e485c-193">then the same documentation is output as if the source code contained:</span></span>

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

<span data-ttu-id="e485c-194">Bu etiket, liste veya Tablo öğesi oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-194">This tag is used to create a list or table of items.</span></span> <span data-ttu-id="e485c-195">İçerebilir bir `<listheader>` satırında bir tablo veya tanım listesi tanımlamak için blok.</span><span class="sxs-lookup"><span data-stu-id="e485c-195">It may contain a `<listheader>` block to define the heading row of either a table or definition list.</span></span> <span data-ttu-id="e485c-196">(Bir tablo için yalnızca bir giriş tanımlarken `term` başlıkta sağlanması.)</span><span class="sxs-lookup"><span data-stu-id="e485c-196">(When defining a table, only an entry for `term` in the heading need be supplied.)</span></span>

<span data-ttu-id="e485c-197">Listedeki her bir öğe ile belirtilen bir `<item>` blok.</span><span class="sxs-lookup"><span data-stu-id="e485c-197">Each item in the list is specified with an `<item>` block.</span></span> <span data-ttu-id="e485c-198">Bir tanım listesi oluştururken hem de `term` ve `description` belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e485c-198">When creating a definition list, both `term` and `description` must be specified.</span></span> <span data-ttu-id="e485c-199">Ancak, bir tablo, madde işaretli liste veya numaralı liste, yalnızca için `description` belirtilmesi.</span><span class="sxs-lookup"><span data-stu-id="e485c-199">However, for a table, bulleted list, or numbered list, only `description` need be specified.</span></span>

<span data-ttu-id="e485c-200">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-200">__Syntax:__</span></span>

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

<span data-ttu-id="e485c-201">Burada</span><span class="sxs-lookup"><span data-stu-id="e485c-201">where</span></span>

* <span data-ttu-id="e485c-202">`term` tanımları konusu tanımlamak için terimi `description`.</span><span class="sxs-lookup"><span data-stu-id="e485c-202">`term` is the term to define, whose definition is in `description`.</span></span>
* <span data-ttu-id="e485c-203">`description` bir madde işareti veya numaralı liste bir öğeyi veya tanımı bir `term`.</span><span class="sxs-lookup"><span data-stu-id="e485c-203">`description` is either an item in a bullet or numbered list, or the definition of a `term`.</span></span>

<span data-ttu-id="e485c-204">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-204">__Example:__</span></span>

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

<span data-ttu-id="e485c-205">Bu etiket diğer etiketler iç kullanım için olduğu gibi `<summary>` ([`<remark>`](documentation-comments.md#remark)) veya `<returns>` ([`<returns>`](documentation-comments.md#returns)) ve metin eklenecek yapısı izin verir.</span><span class="sxs-lookup"><span data-stu-id="e485c-205">This tag is for use inside other tags, such as `<summary>` ([`<remark>`](documentation-comments.md#remark)) or `<returns>` ([`<returns>`](documentation-comments.md#returns)), and permits structure to be added to text.</span></span>

<span data-ttu-id="e485c-206">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-206">__Syntax:__</span></span>

```xml
<para>content</para>
```

<span data-ttu-id="e485c-207">Burada `content` metni paragraf görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e485c-207">where `content` is the text of the paragraph.</span></span>

<span data-ttu-id="e485c-208">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-208">__Example:__</span></span>

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

<span data-ttu-id="e485c-209">Bu etiket, yöntem, oluşturucu veya dizin oluşturucu için bir parametre tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-209">This tag is used to describe a parameter for a method, constructor, or indexer.</span></span>

<span data-ttu-id="e485c-210">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-210">__Syntax:__</span></span>

```xml
<param name="name">description</param>
```

<span data-ttu-id="e485c-211">Burada</span><span class="sxs-lookup"><span data-stu-id="e485c-211">where</span></span>

* <span data-ttu-id="e485c-212">`name` parametrenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-212">`name` is the name of the parameter.</span></span>
* <span data-ttu-id="e485c-213">`description` parametre açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-213">`description` is a description of the parameter.</span></span>

<span data-ttu-id="e485c-214">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-214">__Example:__</span></span>

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

<span data-ttu-id="e485c-215">Bu etiket, bir sözcüğün bir parametre olduğunu belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-215">This tag is used to indicate that a word is a parameter.</span></span> <span data-ttu-id="e485c-216">Soubor dokumentace bu parametreyi belirgin bir şekilde biçimlendirmek için işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-216">The documentation file can be processed to format this parameter in some distinct way.</span></span>

<span data-ttu-id="e485c-217">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-217">__Syntax:__</span></span>

```xml
<paramref name="name"/>
```

<span data-ttu-id="e485c-218">Burada `name` parametrenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-218">where `name` is the name of the parameter.</span></span>

<span data-ttu-id="e485c-219">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-219">__Example:__</span></span>

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

<span data-ttu-id="e485c-220">Bu etiket belgelenecektir üye güvenlik erişilebilirliğini sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-220">This tag allows the security accessibility of a member to be documented.</span></span>

<span data-ttu-id="e485c-221">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-221">__Syntax:__</span></span>

```xml
<permission cref="member">description</permission>
```

<span data-ttu-id="e485c-222">Burada</span><span class="sxs-lookup"><span data-stu-id="e485c-222">where</span></span>

* <span data-ttu-id="e485c-223">`member` bir üyenin adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-223">`member` is the name of a member.</span></span> <span data-ttu-id="e485c-224">Belirli bir kod öğesi var ve çevirir belgeleri Oluşturucu denetler *üye* soubor dokumentace kurallı öğesi adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-224">The documentation generator checks that the given code element exists and translates *member* to the canonical element name in the documentation file.</span></span>
* <span data-ttu-id="e485c-225">`description` üye erişimi açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-225">`description` is a description of the access to the member.</span></span>

<span data-ttu-id="e485c-226">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-226">__Example:__</span></span>

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

<span data-ttu-id="e485c-227">Bu etiket, bir türle ilgili ek bilgileri belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-227">This tag is used to specify extra information about a type.</span></span> <span data-ttu-id="e485c-228">(Kullanım `<summary>` ([`<summary>`](documentation-comments.md#summary)) türü ve bir türün üyelerini tanımlamak için.)</span><span class="sxs-lookup"><span data-stu-id="e485c-228">(Use `<summary>` ([`<summary>`](documentation-comments.md#summary)) to describe the type itself and the members of a type.)</span></span>

<span data-ttu-id="e485c-229">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-229">__Syntax:__</span></span>

```xml
<remark>description</remark>
```

<span data-ttu-id="e485c-230">Burada `description` açıklama metni.</span><span class="sxs-lookup"><span data-stu-id="e485c-230">where `description` is the text of the remark.</span></span>

<span data-ttu-id="e485c-231">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-231">__Example:__</span></span>

```csharp
/// <summary>Class <c>Point</c> models a point in a 
/// two-dimensional plane.</summary>
/// <remark>Uses polar coordinates</remark>
public class Point 
{
    // ...
}
```

### `<returns>`

<span data-ttu-id="e485c-232">Bu etiket, bir yöntemin dönüş değerini tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-232">This tag is used to describe the return value of a method.</span></span>

<span data-ttu-id="e485c-233">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-233">__Syntax:__</span></span>

```xml
<returns>description</returns>
```

<span data-ttu-id="e485c-234">Burada `description` dönüş değeri bir açıklaması verilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e485c-234">where `description` is a description of the return value.</span></span>

<span data-ttu-id="e485c-235">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-235">__Example:__</span></span>

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

<span data-ttu-id="e485c-236">Bu etiket metin içinde belirtilmesi için bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-236">This tag allows a link to be specified within text.</span></span> <span data-ttu-id="e485c-237">Kullanım `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) bir Ayrıca bkz. bölümünde görüntülenecek olan metin göstermek için.</span><span class="sxs-lookup"><span data-stu-id="e485c-237">Use `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) to indicate text that is to appear in a See Also section.</span></span>

<span data-ttu-id="e485c-238">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-238">__Syntax:__</span></span>

```xml
<see cref="member"/>
```

<span data-ttu-id="e485c-239">Burada `member` üyesi adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-239">where `member` is the name of a member.</span></span> <span data-ttu-id="e485c-240">Belirli bir kod öğesi var ve değişiklikleri belgeleri Oluşturucu denetler *üye* oluşturulan belgeleri dosyasındaki öğesi adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-240">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="e485c-241">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-241">__Example:__</span></span>

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

<span data-ttu-id="e485c-242">Bu etiket için Ayrıca bakınız bölümüne oluşturulması gereken bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-242">This tag allows an entry to be generated for the See Also section.</span></span> <span data-ttu-id="e485c-243">Kullanım `<see>` ([`<see>`](documentation-comments.md#see)) bağlantı metninde belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="e485c-243">Use `<see>` ([`<see>`](documentation-comments.md#see)) to specify a link from within text.</span></span>

<span data-ttu-id="e485c-244">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-244">__Syntax:__</span></span>

```xml
<seealso cref="member"/>
```

<span data-ttu-id="e485c-245">Burada `member` üyesi adıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-245">where `member` is the name of a member.</span></span> <span data-ttu-id="e485c-246">Belirli bir kod öğesi var ve değişiklikleri belgeleri Oluşturucu denetler *üye* oluşturulan belgeleri dosyasındaki öğesi adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-246">The documentation generator checks that the given code element exists and changes *member* to the element name in the generated documentation file.</span></span>

<span data-ttu-id="e485c-247">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-247">__Example:__</span></span>

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

Bu etiket, bir tür veya üye türü tanımlamak için kullanılabilir. <span data-ttu-id="e485c-249">Kullanım `<remark>` ([`<remark>`](documentation-comments.md#remark)) türünü belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="e485c-249">Use `<remark>` ([`<remark>`](documentation-comments.md#remark)) to describe the type itself.</span></span>

<span data-ttu-id="e485c-250">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-250">__Syntax:__</span></span>

```xml
<summary>description</summary>
```

<span data-ttu-id="e485c-251">Burada `description` bir türe veya üyeye özetidir.</span><span class="sxs-lookup"><span data-stu-id="e485c-251">where `description` is a summary of the type or member.</span></span>

<span data-ttu-id="e485c-252">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-252">__Example:__</span></span>

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

<span data-ttu-id="e485c-253">Bu etiket tanımlanmış bir özellik sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-253">This tag allows a property to be described.</span></span>

<span data-ttu-id="e485c-254">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-254">__Syntax:__</span></span>

```xml
<value>property description</value>
```

<span data-ttu-id="e485c-255">Burada `property description` özelliği için bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="e485c-255">where `property description` is a description for the property.</span></span>

<span data-ttu-id="e485c-256">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-256">__Example:__</span></span>

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

<span data-ttu-id="e485c-257">Bu etiket, genel tür parametresi için bir sınıf, yapı, arabirim, temsilci veya yöntemi tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-257">This tag is used to describe a generic type parameter for a class, struct, interface, delegate, or method.</span></span>

<span data-ttu-id="e485c-258">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-258">__Syntax:__</span></span>

```xml
<typeparam name="name">description</typeparam>
```

<span data-ttu-id="e485c-259">Burada `name` tür parametresi adıdır ve `description` kendi açıklamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-259">where `name` is the name of the type parameter, and `description` is its description.</span></span>

<span data-ttu-id="e485c-260">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-260">__Example:__</span></span>

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

<span data-ttu-id="e485c-261">Bu etiket, bir sözcüğün bir tür parametresi olduğunu belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-261">This tag is used to indicate that a word is a type parameter.</span></span> <span data-ttu-id="e485c-262">Soubor dokumentace, bu tür parametresi farklı şekilde biçimlendirmek için işlenebilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-262">The documentation file can be processed to format this type parameter in some distinct way.</span></span>

<span data-ttu-id="e485c-263">__Sözdizimi:__</span><span class="sxs-lookup"><span data-stu-id="e485c-263">__Syntax:__</span></span>

```xml
<typeparamref name="name"/>
```

<span data-ttu-id="e485c-264">Burada `name` tür parametresinin adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-264">where `name` is the name of the type parameter.</span></span>

<span data-ttu-id="e485c-265">__Örnek:__</span><span class="sxs-lookup"><span data-stu-id="e485c-265">__Example:__</span></span>

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a><span data-ttu-id="e485c-266">Soubor dokumentace işleme</span><span class="sxs-lookup"><span data-stu-id="e485c-266">Processing the documentation file</span></span>

<span data-ttu-id="e485c-267">Belgeleri Oluşturucu bir belge açıklaması ile etiketlenir kaynak kod içindeki her öğe için bir kimlik dizesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e485c-267">The documentation generator generates an ID string for each element in the source code that is tagged with a documentation comment.</span></span> <span data-ttu-id="e485c-268">Bu kimlik dizesi, bir kaynak öğesi benzersiz olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-268">This ID string uniquely identifies a source element.</span></span> <span data-ttu-id="e485c-269">Bir belge Görüntüleyici, bir kimlik dizesi belgeleri uygulandığı ilgili meta verileri/yansıma öğeyi tanımlamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e485c-269">A documentation viewer can use an ID string to identify the corresponding metadata/reflection item to which the documentation applies.</span></span>

<span data-ttu-id="e485c-270">Soubor dokumentace kaynak kodunu hiyerarşik temsili değildir; Bunun yerine, her öğe için oluşturulan bir kimlik dizesi ile düz bir liste olduğu.</span><span class="sxs-lookup"><span data-stu-id="e485c-270">The documentation file is not a hierarchical representation of the source code; rather, it is a flat list with a generated ID string for each element.</span></span>

### <a name="id-string-format"></a><span data-ttu-id="e485c-271">Kimlik dizesi biçimi</span><span class="sxs-lookup"><span data-stu-id="e485c-271">ID string format</span></span>

<span data-ttu-id="e485c-272">Kimliği dizesi oluştururken aşağıdaki kuralları belgeleri Oluşturucu gözlemler:</span><span class="sxs-lookup"><span data-stu-id="e485c-272">The documentation generator observes the following rules when it generates the ID strings:</span></span>

*  <span data-ttu-id="e485c-273">Hiçbir boşluk dizesinde yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-273">No white space is placed in the string.</span></span>

*  <span data-ttu-id="e485c-274">Dizenin ilk bölümü, tek bir karakter üste aracılığıyla belgelenmiş üye türünü tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-274">The first part of the string identifies the kind of member being documented, via a single character followed by a colon.</span></span> <span data-ttu-id="e485c-275">Aşağıdaki türde üye tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="e485c-275">The following kinds of members are defined:</span></span>

   | <span data-ttu-id="e485c-276">__Karakter__</span><span class="sxs-lookup"><span data-stu-id="e485c-276">__Character__</span></span> | <span data-ttu-id="e485c-277">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="e485c-277">__Description__</span></span>                                             |
   |---------------|-------------------------------------------------------------|
   | <span data-ttu-id="e485c-278">E</span><span class="sxs-lookup"><span data-stu-id="e485c-278">E</span></span>             | <span data-ttu-id="e485c-279">Olay</span><span class="sxs-lookup"><span data-stu-id="e485c-279">Event</span></span>                                                       |
   | <span data-ttu-id="e485c-280">F</span><span class="sxs-lookup"><span data-stu-id="e485c-280">F</span></span>             | <span data-ttu-id="e485c-281">Alan</span><span class="sxs-lookup"><span data-stu-id="e485c-281">Field</span></span>                                                       |
   | <span data-ttu-id="e485c-282">M</span><span class="sxs-lookup"><span data-stu-id="e485c-282">M</span></span>             | <span data-ttu-id="e485c-283">(Oluşturucular, Yıkıcılar ve işleçler dahil) yöntemi</span><span class="sxs-lookup"><span data-stu-id="e485c-283">Method (including constructors, destructors, and operators)</span></span> |
   | <span data-ttu-id="e485c-284">N</span><span class="sxs-lookup"><span data-stu-id="e485c-284">N</span></span>             | <span data-ttu-id="e485c-285">Ad Alanı</span><span class="sxs-lookup"><span data-stu-id="e485c-285">Namespace</span></span>                                                   |
   | <span data-ttu-id="e485c-286">P</span><span class="sxs-lookup"><span data-stu-id="e485c-286">P</span></span>             | <span data-ttu-id="e485c-287">Özellik (dizin oluşturucular dahil)</span><span class="sxs-lookup"><span data-stu-id="e485c-287">Property (including indexers)</span></span>                               |
   | <span data-ttu-id="e485c-288">T</span><span class="sxs-lookup"><span data-stu-id="e485c-288">T</span></span>             | <span data-ttu-id="e485c-289">(Sınıfı, temsilci, enum, arabirimi ve yapı gibi) yazın</span><span class="sxs-lookup"><span data-stu-id="e485c-289">Type (such as class, delegate, enum, interface, and struct)</span></span> |
   | <span data-ttu-id="e485c-290">!</span><span class="sxs-lookup"><span data-stu-id="e485c-290">!</span></span>             | <span data-ttu-id="e485c-291">Hata dizesi; dizenin geri kalanı, hata hakkında bilgi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e485c-291">Error string; the rest of the string provides information about the error.</span></span> <span data-ttu-id="e485c-292">Örneğin, belgeleri Oluşturucu çözümlenemeyen bağlantılar için hata bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e485c-292">For example, the documentation generator generates error information for links that cannot be resolved.</span></span> |

*  <span data-ttu-id="e485c-293">İkinci dize ad alanı kökünde başlangıç öğesi tam olarak nitelenmiş adını parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="e485c-293">The second part of the string is the fully qualified name of the element, starting at the root of the namespace.</span></span> <span data-ttu-id="e485c-294">Öğe, kapsayan türleri ve ad alanı adı noktalarla ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="e485c-294">The name of the element, its enclosing type(s), and namespace are separated by periods.</span></span> <span data-ttu-id="e485c-295">Öğenin adını nokta varsa, bunlar tarafından değiştirilir `#(U+0023)` karakter.</span><span class="sxs-lookup"><span data-stu-id="e485c-295">If the name of the item itself has periods, they are replaced by `#(U+0023)` characters.</span></span> <span data-ttu-id="e485c-296">(Hiçbir öğe adını bu karakteri olduğunu varsayılır.)</span><span class="sxs-lookup"><span data-stu-id="e485c-296">(It is assumed that no element has this character in its name.)</span></span>
*  <span data-ttu-id="e485c-297">Yöntemleri ve özellikleri parantez içindeki bağımsız değişken, bağımsız değişken listesi aşağıdadır.</span><span class="sxs-lookup"><span data-stu-id="e485c-297">For methods and properties with arguments, the argument list follows, enclosed in parentheses.</span></span> <span data-ttu-id="e485c-298">Bu bağımsız değişkenler olmadan parantezler göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-298">For those without arguments, the parentheses are omitted.</span></span> <span data-ttu-id="e485c-299">Bağımsız değişkenlerin virgülle ayrılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-299">The arguments are separated by commas.</span></span> <span data-ttu-id="e485c-300">Kodlama her bağımsız değişkeni bir CLI imza aynı şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="e485c-300">The encoding of each argument is the same as a CLI signature, as follows:</span></span>
   *  <span data-ttu-id="e485c-301">Bağımsız değişkenler şu şekilde değiştirilmiş kendi tam adına göre kendi belgesi adı tarafından temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="e485c-301">Arguments are represented by their documentation name, which is based on their fully qualified name, modified as follows:</span></span>
      * <span data-ttu-id="e485c-302">Genel türleri temsil eden bağımsız değişkenlere sahip bir eklenmiş `` ` `` (vurgulamasını belirtir) karakteri ve ardından tür parametrelerinin sayısı</span><span class="sxs-lookup"><span data-stu-id="e485c-302">Arguments that represent generic types have an appended `` ` `` (backtick) character followed by the number of type parameters</span></span>
      * <span data-ttu-id="e485c-303">Bağımsız değişkenleri olan `out` veya `ref` değiştiricisine sahip bir `@` izleyerek kendi tür adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-303">Arguments having the `out` or `ref` modifier have an `@` following their type name.</span></span> <span data-ttu-id="e485c-304">Geçirilen bağımsız değişkenleri değere veya aracılığıyla `params` hiçbir özel gösterimi vardır.</span><span class="sxs-lookup"><span data-stu-id="e485c-304">Arguments passed by value or via `params` have no special notation.</span></span>
      * <span data-ttu-id="e485c-305">Dizileri bağımsız değişkenleri olarak temsil edilir `[lowerbound:size, ... , lowerbound:size]` burada virgüllerle küçük bir boyut sayısıdır ve her boyut, boyutu ve alt sınırlarını biliniyorsa, ondalık olarak gösterilir.</span><span class="sxs-lookup"><span data-stu-id="e485c-305">Arguments that are arrays are represented as `[lowerbound:size, ... , lowerbound:size]` where the number of commas is the rank less one, and the lower bounds and size of each dimension, if known, are represented in decimal.</span></span> <span data-ttu-id="e485c-306">Alt sınır veya boyutu belirtilmezse atlanır.</span><span class="sxs-lookup"><span data-stu-id="e485c-306">If a lower bound or size is not specified, it is omitted.</span></span> <span data-ttu-id="e485c-307">Belirli bir boyut için boyut ve alt sınır atlanırsa, `:` de atlanır.</span><span class="sxs-lookup"><span data-stu-id="e485c-307">If the lower bound and size for a particular dimension are omitted, the `:` is omitted as well.</span></span> <span data-ttu-id="e485c-308">Basit diziler tarafından temsil edilir `[]` düzeyi başına.</span><span class="sxs-lookup"><span data-stu-id="e485c-308">Jagged arrays are represented by one `[]` per level.</span></span>
      * <span data-ttu-id="e485c-309">İşaretçi türleri void dışında olan bağımsız değişkenler kullanılarak temsil edilir bir `*` aşağıdaki tür adı.</span><span class="sxs-lookup"><span data-stu-id="e485c-309">Arguments that have pointer types other than void are represented using a `*` following the type name.</span></span> <span data-ttu-id="e485c-310">Void bir işaretçi türü adını kullanılarak temsil edilir `System.Void`.</span><span class="sxs-lookup"><span data-stu-id="e485c-310">A void pointer is represented using a type name of `System.Void`.</span></span>
      * <span data-ttu-id="e485c-311">Genel tür parametreleri türlerinde tanımlanan başvuran bağımsız değişkenler kullanılarak kodlanır `` ` `` (vurgulamasını belirtir) karakteri ve ardından tür parametresi sıfır tabanlı dizini.</span><span class="sxs-lookup"><span data-stu-id="e485c-311">Arguments that refer to generic type parameters defined on types are encoded using the `` ` `` (backtick) character followed by the zero-based index of the type parameter.</span></span>
      * <span data-ttu-id="e485c-312">Yöntemleri tanımlanan genel tür parametrelerini kullanan bağımsız değişkenleri kullanma double-vurgulamasını belirtir ``` `` ``` yerine `` ` `` türleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e485c-312">Arguments that use generic type parameters defined in methods use a double-backtick ``` `` ``` instead of the `` ` `` used for types.</span></span>
      * <span data-ttu-id="e485c-313">Oluşturulan genel türler için başvuru bağımsız değişkenleri, ardından genel türü kullanılarak kodlanır `{`, tür bağımsız değişkenleri virgülle ayrılmış listesi tarafından izlenen, ardından `}`.</span><span class="sxs-lookup"><span data-stu-id="e485c-313">Arguments that refer to constructed generic types are encoded using the generic type, followed by `{`, followed by a comma-separated list of type arguments, followed by `}`.</span></span>

### <a name="id-string-examples"></a><span data-ttu-id="e485c-314">Kimlik dizesi örnekleri</span><span class="sxs-lookup"><span data-stu-id="e485c-314">ID string examples</span></span>

<span data-ttu-id="e485c-315">Aşağıdaki örnekler her C# kod, her kaynak öğesi yeteneğine sahip bir belge açıklaması kilitlenmelerinden üretilen kimliği dizesi ile birlikte bir parçasını gösterir:</span><span class="sxs-lookup"><span data-stu-id="e485c-315">The following examples each show a fragment of C# code, along with the ID string produced from each source element capable of having a documentation comment:</span></span>

*  <span data-ttu-id="e485c-316">Türleri, genel bilgiler ile genişletilmiş kendi tam adı kullanılarak temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="e485c-316">Types are represented using their fully qualified name, augmented with generic information:</span></span>

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

*  <span data-ttu-id="e485c-317">Alanları tam adına göre temsil edilir:</span><span class="sxs-lookup"><span data-stu-id="e485c-317">Fields are represented by their fully qualified name:</span></span>

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

*  <span data-ttu-id="e485c-318">Oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="e485c-318">Constructors.</span></span>

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

*  <span data-ttu-id="e485c-319">Yok ediciler.</span><span class="sxs-lookup"><span data-stu-id="e485c-319">Destructors.</span></span>

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

*  <span data-ttu-id="e485c-320">Yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="e485c-320">Methods.</span></span>

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

*  <span data-ttu-id="e485c-321">Özellikler ve dizin oluşturucular.</span><span class="sxs-lookup"><span data-stu-id="e485c-321">Properties and indexers.</span></span>

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

*  <span data-ttu-id="e485c-322">olaylar.</span><span class="sxs-lookup"><span data-stu-id="e485c-322">Events.</span></span>

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

*  <span data-ttu-id="e485c-323">Birli işleçler.</span><span class="sxs-lookup"><span data-stu-id="e485c-323">Unary operators.</span></span>

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

   <span data-ttu-id="e485c-324">Birli işleç işlev adlarını kullanılan kümesinin tamamını aşağıdaki gibidir: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, ve `op_False`.</span><span class="sxs-lookup"><span data-stu-id="e485c-324">The complete set of unary operator function names used is as follows: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, and `op_False`.</span></span>

*  <span data-ttu-id="e485c-325">İkili işleçler.</span><span class="sxs-lookup"><span data-stu-id="e485c-325">Binary operators.</span></span>

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

   <span data-ttu-id="e485c-326">Kullanılan ikili işleç işlev adları tüm kümesini aşağıdaki gibidir: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, ve `op_GreaterThanOrEqual`.</span><span class="sxs-lookup"><span data-stu-id="e485c-326">The complete set of binary operator function names used is as follows: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, and `op_GreaterThanOrEqual`.</span></span>

*  <span data-ttu-id="e485c-327">Dönüştürme işleçleri sahip bir sondaki "`~`" dönüş türü tarafından izlenen.</span><span class="sxs-lookup"><span data-stu-id="e485c-327">Conversion operators have a trailing "`~`" followed by the return type.</span></span>

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

## <a name="an-example"></a><span data-ttu-id="e485c-328">Bir örnek</span><span class="sxs-lookup"><span data-stu-id="e485c-328">An example</span></span>

### <a name="c-source-code"></a><span data-ttu-id="e485c-329">C# kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="e485c-329">C# source code</span></span>

<span data-ttu-id="e485c-330">Aşağıdaki örnekte kaynak kodunu gösteren bir `Point` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e485c-330">The following example shows the source code of a `Point` class:</span></span>

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

### <a name="resulting-xml"></a><span data-ttu-id="e485c-331">Elde edilen XML</span><span class="sxs-lookup"><span data-stu-id="e485c-331">Resulting XML</span></span>

<span data-ttu-id="e485c-332">Kaynak kodu için sınıf verildiğinde bir belge Oluşturucu tarafından oluşturulan çıktı şu şekildedir `Point`yukarıda gösterilen:</span><span class="sxs-lookup"><span data-stu-id="e485c-332">Here is the output produced by one documentation generator when given the source code for class `Point`, shown above:</span></span>

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
