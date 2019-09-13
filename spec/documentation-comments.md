---
ms.openlocfilehash: adf81842e3c763c7bbdd3f10bb884dc1207b9099
ms.sourcegitcommit: 0489cb64b7dfb328813d757f4d447a15b85a5851
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70912432"
---
# <a name="documentation-comments"></a>Belge açıklamaları

C#programcıların, XML metni içeren özel bir açıklama söz dizimini kullanarak kodlarını belgetabilmesi için bir mekanizma sağlar. Kaynak kodu dosyalarında, belirli bir biçime sahip olan Yorumlar, bir aracı bu açıklamalardan ve kaynak kodu öğelerinden önce XML oluşturacak şekilde yönlendirmek için kullanılabilir. Bu söz dizimini kullanan açıklamalara ***belge açıklamaları***denir. Kullanıcı tanımlı bir türün (bir sınıf, temsilci veya arabirim gibi) ya da bir üyenin (alan, olay, özellik veya yöntem gibi) hemen önce gelmesi gerekir. XML oluşturma aracına ***belge Oluşturucu***denir. (Bu Oluşturucu, C# derleyicinin kendisi olabilir, ancak olması gerekmez.) Belge Oluşturucu tarafından üretilen çıktıya ***belge dosyası***denir. Belge dosyası bir ***belge görüntüleyicisine***giriş olarak kullanılır; tür bilgilerinin ve ilişkili belgelerinin bir dizi görsel görüntüsünü oluşturmak için tasarlanan bir araç.

Bu belirtim belge açıklamalarında kullanılacak bir etiket kümesi önerir, ancak bu etiketlerin kullanımı gerekli değildir, ancak doğru biçimlendirilmiş XML kuralları izlenirken, istenirse diğer Etiketler de kullanılabilir.

## <a name="introduction"></a>Giriş

Özel bir forma sahip olan Yorumlar, bir aracı bu açıklamalardan ve kaynak kod öğelerinden önce XML oluşturacak şekilde yönlendirmek için kullanılabilir. Bu yorumlar, üç eğik çizgi (`///`) ile başlayan tek satırlık açıklamalardır ve eğik çizgiyle ve iki yıldızlı (`/**`) ile başlayan sınırlandırılmış açıklamalardır. Bunlar, bir Kullanıcı tanımlı türün (bir sınıf, temsilci veya arabirim gibi) ya da ek açıklama eklenen bir üyenin (bir alan, olay, özellik veya yöntem gibi) hemen önce gelmelidir. Öznitelik bölümleri ([öznitelik belirtimi](attributes.md#attribute-specification)) bildirimlerin bir parçası olarak değerlendirilir, bu nedenle belge açıklamalarının bir tür veya üyeye uygulanan özniteliklerin önünde olması gerekir.

__Sözdizimi__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

Bir *single_line_doc_comment*içinde, her bir *single_line_doc_comment*için geçerli *single_line_doc_comment*bitişik olan `///` karakterleri izleyen bir *boşluk* karakteri varsa, buXML çıktısına boşluk karakteri dahil değildir.

Ayrılmış bir belge açıklamasında, ikinci satırdaki ilk boşluk olmayan karakter bir yıldız işareti ise ve isteğe bağlı boşluk karakterlerinden aynı desenli ve bir yıldız karakteri ayrılmış-belge-açıklama içindeki her satırın başlangıcında tekrarlanırsa, sonra yinelenen modelin karakterleri XML çıktısına dahil edilmez. Bu düzende, sonra yıldız karakteri ve daha önce boşluk karakterleri bulunabilir.

__Örnek:__

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

Belge açıklamalarındaki metin, XML kurallarına göre düzgün şekilde oluşturulmalıdır (https://www.w3.org/TR/REC-xml). XML hatalı biçimlendirilmişse bir uyarı oluşturulur ve belge dosyası bir hata ile karşılaşıldığını söyleyen bir açıklama içerir.

Geliştiriciler kendi etiket kümesini oluşturmak için ücretsiz olsa da önerilen [etiketlerde](documentation-comments.md#recommended-tags)önerilen bir küme tanımlanmıştır. Önerilen etiketlerden bazılarının özel anlamları vardır:

*  Etiketi `<param>` , parametreleri tanımlamakta kullanılır. Bu tür bir etiket kullanılırsa, belge Oluşturucu belirtilen parametrenin var olduğunu ve tüm parametrelerin belge açıklamalarında açıklananlarının doğrulanması gerekir. Bu doğrulama başarısız olursa, belge Oluşturucu bir uyarı verir.
*  `cref` Özniteliği bir kod öğesine başvuru sağlamak için herhangi bir etikete iliştirilebilir. Belge oluşturucunun Bu kod öğesinin var olduğunu doğrulaması gerekir. Doğrulama başarısız olursa, belge Oluşturucu bir uyarı verir. Bir `cref` öznitelikte açıklanan adı ararken belge oluşturucunun, kaynak kodu içinde görüntülenen `using` deyimlere göre ad alanı görünürlüğüne göre olması gerekir. Genel olan kod öğeleri için, normal genel sözdizimi (yani, "`List<T>`") geçersiz XML oluşturduğundan kullanılamaz. Ayraçlar (`List{T}`Yani, "") veya XML kaçış söz dizimi (yani, "`List&lt;T&gt;`") yerine kullanılabilir.
*  Etiket `<summary>` , bir tür veya üyeyle ilgili ek bilgileri göstermek için bir belge Görüntüleyicisi tarafından kullanılmak üzere tasarlanmıştır.
*  Etiketi `<include>` , bir dış XML dosyasındaki bilgileri içerir.

Belge dosyasının tür ve Üyeler hakkında tam bilgi sağlamadığına ve (örneğin, herhangi bir tür bilgisi içermediğinden) emin olun. Bir tür veya üye hakkında daha fazla bilgi almak için, belge dosyası gerçek tür veya üyede yansıma ile birlikte kullanılmalıdır.

## <a name="recommended-tags"></a>Önerilen Etiketler

Belge oluşturucunun, XML kurallarına göre geçerli olan herhangi bir etiketi kabul etmesi ve işlemesi gerekir. Aşağıdaki Etiketler kullanıcı belgelerinde yaygın olarak kullanılan işlevleri sağlar. (Kuşkusuz, diğer Etiketler mümkündür.)


| __Etiket__          | __Kısmı__                                            | __Amaç__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Kod benzeri yazı tipinde metin ayarlama                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Kaynak kodu veya program çıkışının bir veya daha fazla satırını ayarlama |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Bir örnek belirtin                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Bir yöntemin oluşturabilecek özel durumları tanımlar           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Dış dosyadan XML içerir                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Liste veya tablo oluşturma                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | Yapıya metin eklenmesine izin ver                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Yöntem veya Oluşturucu için bir parametre tanımlama       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Bir sözcüğün parametre adı olduğunu belirler               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Üyenin güvenlik erişilebilirliğini belgeleme        |
| `<remarks>`      | [`<remarks>`](documentation-comments.md#remarks)       | Bir tür hakkındaki ek bilgileri açıkla           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Bir yöntemin dönüş değerini açıkla                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Bir bağlantı belirtin                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Ayrıca bkz. bir girdi oluştur                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Türü veya bir türün üyesini açıklama                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Bir özelliği açıkla                                    |
| `<typeparam>`    |                                                        | Genel tür parametresini açıkla                      |
| `<typeparamref>` |                                                        | Bir sözcüğün tür parametre adı olduğunu belirler          |

### `<c>`

Bu etiket, bir açıklama içindeki bir metin parçasının bir kod bloğu için kullanılan gibi özel bir yazı tipinde ayarlanması gerektiğini belirten bir mekanizma sağlar. Gerçek kod satırları için ( `<code>` [`<code>`](documentation-comments.md#code)) kullanın.

__Sözdizimi__

```xml
<c>text</c>
```

__Örnek:__

```csharp
/// <summary>Class <c>Point</c> models a point in a two-dimensional
/// plane.</summary>

public class Point 
{
    // ...
}
```

### `<code>`

Bu etiket, bazı özel yazı tiplerinde kaynak kodu veya program çıkışının bir veya daha fazla satırını ayarlamak için kullanılır. Anlatıcı olarak küçük kod parçaları için ( `<c>` [`<c>`](documentation-comments.md#c)) kullanın.

__Sözdizimi__

```xml
<code>source code or program output</code>
```

__Örnek:__

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

Bu etiket, bir metodun veya diğer kitaplık üyesinin nasıl kullanılabileceğini belirtmek için bir açıklama içindeki örnek koda izin verir. Normalde, bu da etiketin `<code>` ([`<code>`](documentation-comments.md#code)) kullanımını da kapsar.

__Sözdizimi__

```xml
<example>description</example>
```

__Örnek:__

Bir `<code>` örnek[`<code>`](documentation-comments.md#code)için bkz. ().

### `<exception>`

Bu etiket, bir yöntemin oluşturmakta olduğu özel durumları belgelemek için bir yol sağlar.

__Sözdizimi__

```xml
<exception cref="member">description</exception>
```

Burada

* `member`üyenin adıdır. Belge Oluşturucu, belirtilen üyenin var olduğunu denetler ve belge dosyasında `member` kurallı öğe adına çevirir.
* `description`, özel durumun oluşturulduğu durumların açıklamasıdır.

__Örnek:__

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

Bu etiket, kaynak kodu dosyasının dışında bir XML belgesinden bilgi dahil etmenizi sağlar. Dış dosya iyi biçimlendirilmiş bir XML belgesi olmalıdır ve bu belgeye hangi XML ekleneceğini belirtmek için bu belgeye bir XPath ifadesi uygulanmış olması gerekir. Daha `<include>` sonra etiket, dış belgedeki seçili XML ile değiştirilmiştir.

__Sözdizimi__

```
<include file="filename" path="xpath" />
```

Burada

* `filename`, harici bir XML dosyasının dosya adıdır. Dosya adı, içerme etiketini içeren dosyaya göre yorumlanır.
* `xpath`Dış XML dosyasındaki XML 'den bazılarını seçen bir XPath ifadesidir.

__Örnek:__

Kaynak kodu şöyle bir bildirim içeriyorsa:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

"docs. xml" dış dosyası aşağıdaki içeriğe sahiptir:

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

daha sonra aynı belgeler, kaynak kodu içeren çıktıdır:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Bu etiket bir liste veya öğe tablosu oluşturmak için kullanılır. Bir tablo ya da `<listheader>` tanım listesinin başlık satırını tanımlamak için bir blok içerebilir. (Bir tablo tanımlarken, yalnızca başlık için `term` bir girdinin sağlanması gerekir.)

Listedeki her öğe bir `<item>` blokla belirtilir. Bir tanım listesi oluştururken, her ikisi `term` de `description` belirtilmelidir. Ancak, bir tablo, madde işaretli liste veya numaralandırılmış liste için yalnızca `description` gerekli olmalıdır.

__Sözdizimi__

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

Burada

* `term`tanımı içinde `description`olan tanımlama terimi.
* `description`, bir madde işareti veya numaralandırılmış listedeki bir öğe ya da bir `term`tanımı.

__Örnek:__

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

Bu etiket, `<summary>` ([`<remarks>`](documentation-comments.md#remarks)) veya `<returns>` ([`<returns>`](documentation-comments.md#returns)) gibi diğer etiketlerin içinde kullanım içindir ve yapının metne eklenmesine izin verir.

__Sözdizimi__

```xml
<para>content</para>
```

paragrafın `content` metni nerede.

__Örnek:__

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

Bu etiket bir yöntem, Oluşturucu veya Dizin Oluşturucu için bir parametre belirtmek için kullanılır.

__Sözdizimi__

```xml
<param name="name">description</param>
```

Burada

* `name`parametrenin adıdır.
* `description`, parametresinin bir açıklamasıdır.

__Örnek:__

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

Bu etiket bir sözcüğün bir parametre olduğunu göstermek için kullanılır. Belge dosyası bu parametreyi farklı bir şekilde biçimlendirmek için işlenebilir.

__Sözdizimi__

```xml
<paramref name="name"/>
```

`name` parametresinin adı.

__Örnek:__

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

Bu etiket bir üyenin güvenlik erişilebilirliğinin belgelenme olanak sağlar.

__Sözdizimi__

```xml
<permission cref="member">description</permission>
```

Burada

* `member`üyenin adıdır. Belge Oluşturucu verilen kod öğesinin var olduğunu denetler ve belge *dosyasındaki öğeyi kurallı öğe adına çevirir.*
* `description`, üyeye erişimin bir açıklamasıdır.

__Örnek:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remarks>`

Bu etiket, bir tür hakkındaki ek bilgileri belirtmek için kullanılır. (Türünün `<summary>` kendisini[`<summary>`](documentation-comments.md#summary)ve bir türün üyelerini anlatmak için () kullanın.)

__Sözdizimi__

```xml
<remarks>description</remarks>
```

Burada `description` , açıklamanın metni bulunur.

__Örnek:__

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

Bu etiket bir yöntemin dönüş değerini tanımlamakta kullanılır.

__Sözdizimi__

```xml
<returns>description</returns>
```

`description` , dönüş değerinin bir açıklamasıdır.

__Örnek:__

```csharp
/// <summary>Report a point's location as a string.</summary>
/// <returns>A string representing a point's location, in the form (x,y),
///    without any leading, trailing, or embedded whitespace.</returns>
public override string ToString() {
    return "(" + X + "," + Y + ")";
}
```

### `<see>`

Bu etiket, bir bağlantının metin içinde belirtilmesini sağlar. Ayrıca `<seealso>` bkz[`<seealso>`](documentation-comments.md#seealso). bölümünde görünecek metni göstermek için () kullanın.

__Sözdizimi__

```xml
<see cref="member"/>
```

`member` üyenin adı. Belge Oluşturucu verilen kod öğesinin var olduğunu denetler ve oluşturulan belgeler dosyasındaki öğe adına *üye* olarak değişir.

__Örnek:__

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

Bu etiket, Ayrıca bkz. bölümü için bir girişin oluşturulmasına izin verir. Metin `<see>` içinden[`<see>`](documentation-comments.md#see)bir bağlantı belirtmek için () kullanın.

__Sözdizimi__

```xml
<seealso cref="member"/>
```

`member` üyenin adı. Belge Oluşturucu verilen kod öğesinin var olduğunu denetler ve oluşturulan belgeler dosyasındaki öğe adına *üye* olarak değişir.

__Örnek:__

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

Bu etiket, bir türü veya bir türün bir üyesini anlatmak için kullanılabilir. Türün `<remarks>` kendisini[`<remarks>`](documentation-comments.md#remarks)anlatmak için () kullanın.

__Sözdizimi__

```xml
<summary>description</summary>
```

`description` , türün veya üyenin bir özetidir.

__Örnek:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Bu etiket, bir özelliğin açıklanalmasına izin verir.

__Sözdizimi__

```xml
<value>property description</value>
```

`property description` , özelliği için bir açıklamadır.

__Örnek:__

```csharp
/// <value>Property <c>X</c> represents the point's x-coordinate.</value>
public int X
{
    get { return x; }
    set { x = value; }
}
```

### `<typeparam>`

Bu etiket, bir sınıf, yapı, arabirim, temsilci veya yöntem için genel bir tür parametresi tanımlamakta kullanılır.

__Sözdizimi__

```xml
<typeparam name="name">description</typeparam>
```

Burada `name` tür parametresinin adıdır ve `description` açıklamasıdır.

__Örnek:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Bu etiket, bir sözcüğün bir tür parametresi olduğunu göstermek için kullanılır. Belge dosyası bu tür parametresini farklı bir şekilde biçimlendirmek için işlenebilir.

__Sözdizimi__

```xml
<typeparamref name="name"/>
```

Burada `name` tür parametresinin adıdır.

__Örnek:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Belge dosyası işleniyor

Belge Oluşturucu bir belge açıklamasıyla etiketlenmiş kaynak kodundaki her öğe için bir KIMLIK dizesi oluşturur. Bu KIMLIK dizesi bir kaynak öğeyi benzersiz şekilde tanımlar. Belge Görüntüleyicisi, belgelerin uygulandığı karşılık gelen meta veri/yansıma öğesini tanımlamak için bir KIMLIK dizesi kullanabilir.

Belge dosyası kaynak kodun hiyerarşik bir temsili değildir; Bunun yerine, her öğe için oluşturulmuş bir KIMLIK dizesi olan düz bir liste olur.

### <a name="id-string-format"></a>KIMLIK dize biçimi

Belge Oluşturucu, KIMLIK dizelerini oluşturduğunda aşağıdaki kuralları sunar:

*  Dizeye boşluk yerleştirilmez.

*  Dizenin ilk bölümü, belgelendirilmiş üye türünü, tek bir karakter ve ardından iki nokta üst üste ile tanımlar. Aşağıdaki üye türleri tanımlanmıştır:

   | __İnde__ | __Açıklama__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Olay                                                       |
   | F             | Alan                                                       |
   | M             | Yöntem (oluşturucular, Yıkıcılar ve işleçler dahil) |
   | N             | Ad Alanı                                                   |
   | P             | Özellik (Dizin oluşturucular dahil)                               |
   | T             | Tür (class, Delegate, Enum, Interface ve struct gibi) |
   | !             | Hata dizesi; dizenin geri kalanı hata hakkında bilgi sağlar. Örneğin, belge Oluşturucu çözülemeyen bağlantılar için hata bilgileri oluşturur. |

*  Dizenin ikinci bölümü, ad alanının köküden başlayarak öğesinin tam nitelikli adıdır. Öğenin adı, kapsayan tür (ler) ve ad alanı noktalarla ayrılır. Öğenin adında nokta varsa, bunlar karakter olarak `#(U+0023)` değiştirilirler. (Bir öğe adında bu karakterin olmadığı varsayılır.)
*  Bağımsız değişkenlerle Yöntemler ve özellikler için bağımsız değişken listesi, parantez içine alınmıştır. Bağımsız değişkenler olmadan, parantezler atlanır. Bağımsız değişkenler virgülle ayrılır. Her bağımsız değişkenin kodlaması, aşağıdaki gibi bir CLı imzasıyla aynıdır:
   *  Bağımsız değişkenler, tam adına göre, aşağıdaki şekilde değiştirilen belge adlarıyla temsil edilir:
      * Genel türleri temsil eden bağımsız değişkenler bir eklenmiş `` ` `` (backtick) karaktere ve ardından tür parametrelerinin sayısına sahiptir
      * `out` `@` Veya değiştiricisinesahipbağımsızdeğişkenler,türadlarınıtakipedenbir.`ref` Değer veya aracılığıyla `params` geçirilen bağımsız değişkenlerin özel bir gösterimi yoktur.
      * Diziler olan bağımsız değişkenler, virgül sayısının `[lowerbound:size, ... , lowerbound:size]` derece daha az bir olduğu ve bilinen her boyutun alt sınırları ve boyutunun ondalık olarak temsil edildiği şekilde temsil edilir. Daha düşük bir sınır veya boyut belirtilmemişse, atlanır. Belirli bir boyutun alt sınırı ve boyutu atlanmışsa `:` , de atlanır. Pürüzlü Diziler, düzey başına bir `[]` ile temsil edilir.
      * Void dışındaki işaretçi türlerine sahip bağımsız değişkenler, `*` aşağıdaki tür adı kullanılarak temsil edilir. Void işaretçisi, bir tür adı `System.Void`kullanılarak temsil edilir.
      * Türler üzerinde tanımlanan genel tür parametrelerine başvuran bağımsız değişkenler, `` ` `` (backtick) karakteri ve ardından tür parametresinin sıfır tabanlı dizini kullanılarak kodlanır.
      * Metotlarda tanımlanan genel tür parametrelerini kullanan bağımsız değişkenler türler için ``` `` ``` `` ` `` kullanılan bir çift yönlü kullanır.
      * Oluşturulan Genel türlere başvuran bağımsız değişkenler genel türü `{`kullanılarak kodlanır, ardından, ve ardından bir tür bağımsız değişken `}`türü ve sonra gelen virgülle ayrılmış bir liste gelir.

### <a name="id-string-examples"></a>KIMLIK dizesi örnekleri

Aşağıdaki örneklerde her biri bir C# kod parçasını ve bir belge yorumu olan her bir kaynak ÖĞEDEN üretilen kimlik dizesiyle birlikte gösterilmektedir:

*  Türler, tam nitelikli adı kullanılarak temsil edilir ve genel bilgilerle Genişletilebilir:

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

*  Alanlar, tam adlarıyla temsil edilir:

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

*  Kurucu.

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

*  Yıkıcılar.

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

*  Yöntem.

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

*  Özellikler ve Dizin oluşturucular.

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

*  Olayları.

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

*  Birli İşleçler.

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

   Kullanılan birli işleç işlev adlarının tamamı aşağıdaki gibidir: `op_UnaryPlus`, `op_UnaryNegation`, `op_Increment` `op_OnesComplement` `op_LogicalNot`,,, `op_Decrement`, `op_True`ve `op_False`.

*  İkili işleçler.

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

   `op_Addition`Kullanılan ikili işleç işlev adlarının tamamen kümesi aşağıdaki gibidir:, `op_BitwiseOr` `op_ExclusiveOr` `op_BitwiseAnd` `op_Subtraction`, `op_Multiply` `op_Modulus` `op_Division`,,,,,, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, ,,`op_LessThan`ve .`op_GreaterThanOrEqual` `op_LessThanOrEqual` `op_GreaterThan`

*  Dönüştürme işleçleri sonunda bir "`~`" ve ardından dönüş türü vardır.

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

## <a name="an-example"></a>Örnek

### <a name="c-source-code"></a>C#kaynak kodu

Aşağıdaki örnekte bir `Point` sınıfın kaynak kodu gösterilmektedir:

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

### <a name="resulting-xml"></a>Elde edilen XML

Aşağıda gösterildiği gibi, sınıf `Point`için kaynak kodu verildiğinde bir belge Oluşturucu tarafından oluşturulan çıktı aşağıda verilmiştir:

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
