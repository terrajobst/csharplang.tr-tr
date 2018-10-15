# <a name="documentation-comments"></a>Belge açıklamaları

C# programcıları kodlarını XML metni içeren bir özel açıklama sözdizimi kullanılarak belge için bir mekanizma sağlar. Kaynak kodu dosyalarında açıklamaları belirli bir biçime sahip bir aracı bu yorumlar ve bunlar önce kaynak kodu öğesi XML üretmek için yönlendirmek için kullanılabilir. Bu söz dizimi çağrılır yorumları kullanarak ***belge açıklamaları***. Hemen bir kullanıcı tanımlı türü (örneğin, bir sınıf, temsilci veya arabirimi) ya da bir üyesi (örneğin, bir alan, olay, özelliği veya yöntemi) gelmelidir. XML oluşturma aracı olarak adlandırılır ***belgeleri Oluşturucu***. (Bu oluşturucunun olabilir, ancak olması gerekmez, C# derleyicisi kendisini.) Belgeleri Oluşturucu tarafından üretilen çıkış adlı ***soubor dokumentace***. Soubor dokumentace girdi olarak kullanılan bir ***belgeleri Görüntüleyicisi***; bir tür bilgilerini ve ilişkili belgelerini görünümünü çeşit üretmek için hedeflenen bir aracı.

Belge açıklamaları içinde kullanılacak etiketleri kümesi bu belirtim önerir ancak bu etiketleri kullanımı gerekli değildir ve uzun doğru biçimlendirilmiş XML kuralları ardından diğer etiketler isterseniz, kullanılabilir.

## <a name="introduction"></a>Giriş

Özel bir biçime sahip açıklamaları, XML, bu yorumlar ve bunlar önce kaynak kod öğeleri oluşturmak için bir araç yönlendirmek için kullanılabilir. Bu tür açıklamaları üç eğik çizgi ile başlayan tek satırlık açıklamaları bulunmaktadır (`///`), ya bir eğik çizgi ve iki yıldız ile başlayan açıklamaları ayrılmış (`/**`). Hemen bir kullanıcı tanımlı türü (örneğin, bir sınıf, temsilci veya arabirimi) ya da bunlar ek açıklama bir üye (örneğin, bir alan, olay, özelliği veya yöntemi) gelmelidir. Öznitelik bölümleri ([öznitelik belirtimi](attributes.md#attribute-specification)) belge yorumlarını bir türe veya üyeye uygulanan öznitelikleri gelmelidir için bildirimler, bir parçası olarak kabul edilir.

__Sözdizimi:__

```antlr
single_line_doc_comment
    : '///' input_character*
    ;

delimited_doc_comment
    : '/**' delimited_comment_section* asterisk+ '/'
    ;
```

İçinde bir *single_line_doc_comment*, yoksa bir *boşluk* karakter aşağıdaki `///` her bir karakter *single_line_doc_comment*bitişik s Geçerli *single_line_doc_comment*, ardından, *boşluk* XML çıkış karakteri dahil değildir.

İkinci satırdaki ilk boşluk olmayan karakter bir yıldız işareti ve aynı deseni isteğe bağlı boşluk karakterleri ve bir yıldız işareti ise sınırlı-doc-açıklama, her birinin sınırlı-doc-açıklama içindeki satır başında yinelenir, ardından karakterlerinden yinelenen desen XML çıktısında dahil edilmez. Desen, sonra yanı sıra, yıldız işareti önce boşluk karakterleri içerebilir.

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

Kurallara göre XML belge açıklamaları içindeki metin iyi biçimlendirilmiş olmalıdır (http://www.w3.org/TR/REC-xml). Doğru biçimlendirilmiş XML olgu olması durumunda bir uyarı oluşturulur ve soubor dokumentace bir hatayla karşılaşıldı belirten bir açıklama içerir.

Geliştiriciler kendi kümesi etiketleri oluşturmak ücretsiz olsa da, önerilen kümesi tanımlanan [önerilen etiketler](documentation-comments.md#recommended-tags). Önerilen etiketler bazıları, özel anlamları vardır:

*  `<param>` Etiketi parametreler tanımlamak için kullanılır. Böyle bir etiket kullandıysanız, belgeleri Oluşturucu belirtilen parametre var olduğundan ve tüm parametreleri belgeleri açıklamaları açıklanan doğrulamanız gerekir. Bu tür doğrulama başarısız olursa, belgeleri Oluşturucu bir uyarı verir.
*  `cref` Öznitelik, bir kod öğesi başvuru sağlamak için herhangi bir etiket eklenebilir. Belgeleri Oluşturucu, bu kod öğesi var olduğunu doğrulamanız gerekir. Belgeleri Oluşturucu, doğrulama başarısız olursa bir uyarı verir. Bir ad açıklanan için aranırken bir `cref` özniteliği belgeleri Oluşturucu gerekir saygı göre ad alanı görünürlük `using` kaynak kodu içinde görünen deyimleri. Genel, kod öğeleri normal genel sözdizimi için (IE "`List<T>`") kullanılamaz çünkü geçersiz XML üretir. Kaşlı ayraçlar yerine kullanılabilir (IE "`List{T}`"), veya XML kaçış sözdiziminin kullanılabilir (IE "`List&lt;T&gt;`").
*  `<summary>` Etiketi bir tür veya üyeyle ilgili ek bilgileri görüntülemek için bir belge Görüntüleyici tarafından kullanılmak üzere tasarlanmıştır.
*  `<include>` Etiketi harici bir XML dosyasından bilgiler içerir.

Soubor dokumentace tür ve üyeler hakkında tam bilgi sağlamaz dikkatle unutmayın (örneğin, bunu herhangi bir tür bilgilerini içermiyor). Böyle bir tür veya üye bilgilerini almak için soubor dokumentace gerçek türe veya üyeye yansıma ile birlikte kullanılmalıdır.

## <a name="recommended-tags"></a>Önerilen etiketler

Belgeleri Oluşturucu kabul et ve XML kurallarına göre geçerli herhangi bir etiket işleme gerekir. Aşağıdaki kullanıcı belgeleri, yaygın olarak kullanılan işlevler sunar. (Kuşkusuz, diğer etiketlerin mümkündür.)


| __Etiket__          | __Bölüm__                                            | __Amaç__                                            |
|------------------|--------------------------------------------------------|--------------------------------------------------------|
| `<c>`            | [`<c>`](documentation-comments.md#c)                   | Metin bir kod benzeri yazı tipini Ayarla                           | 
| `<code>`         | [`<code>`](documentation-comments.md#code)             | Bir veya daha fazla satır, kaynak kodu veya program çıktısı |
| `<example>`      | [`<example>`](documentation-comments.md#example)       | Bir örnek belirtin                                    |
| `<exception>`    | [`<exception>`](documentation-comments.md#exception)   | Bir yöntem oluşturabilecek özel durumları tanımlar.           |
| `<include>`      | [`<include>`](documentation-comments.md#include)       | Dış dosya XML'den içerir                     |
| `<list>`         | [`<list>`](documentation-comments.md#list)             | Bir liste veya tablo oluşturma                                 |
| `<para>`         | [`<para>`](documentation-comments.md#para)             | İzin vermek için metin eklenecek yapısı                   |
| `<param>`        | [`<param>`](documentation-comments.md#param)           | Yöntem veya oluşturucu için bir parametre açıklayın       |
| `<paramref>`     | [`<paramref>`](documentation-comments.md#paramref)     | Bir Word'ün bir parametre adı olduğunu belirleyin               |
| `<permission>`   | [`<permission>`](documentation-comments.md#permission) | Belge bir üye güvenlik erişilebilirliği        |
| `<remark>`       | [`<remark>`](documentation-comments.md#remark)         | Bir türü hakkında ek bilgiler açıklanmaktadır           |
| `<returns>`      | [`<returns>`](documentation-comments.md#returns)       | Bir yöntemin dönüş değerini açıklayın                  |
| `<see>`          | [`<see>`](documentation-comments.md#see)               | Bir bağlantı belirtme                                         |
| `<seealso>`      | [`<seealso>`](documentation-comments.md#seealso)       | Ayrıca bkz: girişi oluştur                              |
| `<summary>`      | [`<summary>`](documentation-comments.md#summary)       | Bir tür veya üye türü açıklar.                  |
| `<value>`        | [`<value>`](documentation-comments.md#value)           | Bir özelliği açıklamaktadır                                    |
| `<typeparam>`    |                                                        | Genel tür parametresi açıklayın                      |
| `<typeparamref>` |                                                        | Bir sözcük tür parametre adı olduğunu belirleyin          |

### `<c>`

Bu etiket, açıklama içindeki metnin bir parçası gibi bir kod bloğu için kullanılan özel bir yazı tipi ayarlanmalıdır belirtmek için bir mekanizma sağlar. Gerçek kod satırı için kullanarak `<code>` ([`<code>`](documentation-comments.md#code)).

__Sözdizimi:__

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

Bu etiket, bir veya daha fazla kaynak kodu veya program çıkış satırlarını bazı özel bir yazı tipinde ayarlamak için kullanılır. Anlatımlı küçük kod parçaları için kullanmak `<c>` ([`<c>`](documentation-comments.md#c)).

__Sözdizimi:__

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

Bu etiket, örnek kod içinde nasıl bir yöntem veya diğer kitaplık üyesini kullanılabilir belirtmek için bir açıklama sağlar. Normalde, bu da etiketinin kapsayacaktır `<code>` ([`<code>`](documentation-comments.md#code)) de.

__Sözdizimi:__

```xml
<example>description</example>
```

__Örnek:__

Bkz: `<code>` ([`<code>`](documentation-comments.md#code)) örneği.

### `<exception>`

Bu etiket, bir yöntem oluşturabilecek özel durumları belge için bir yol sağlar.

__Sözdizimi:__

```xml
<exception cref="member">description</exception>
```

Burada

* `member` bir üyenin adıdır. Belirtilen üyeyi var ve çevirir belgeleri Oluşturucu denetler `member` soubor dokumentace kurallı öğesi adı.
* `description` özel durumun koşullar açıklamasıdır.

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

Bu etiket için kaynak kod dosyası dışındaki bir XML belgesi bilgileri dahil olmak üzere sağlar. Dış dosya doğru biçimlendirilmiş bir XML belgesi olmalıdır ve bir XPath ifadesi eklemek için bu belgede hangi XML'den belirtmek için bu belgeye uygulanır. `<include>` Etiketi dış belgedeki seçilen XML ile sonra değiştirilir.

__Sözdizimi:__

```
<include file="filename" path="xpath" />
```

Burada

* `filename` Dış XML dosyasının dosya adıdır. Dosya adı include etiketiyle içeren dosyayı göre yorumlanır.
* `xpath` Dış XML dosyasında XML bazıları seçen bir XPath ifadesidir.

__Örnek:__

Kaynak kodu gibi bir bildirim içeriyorsa:

```csharp
/// <include file="docs.xml" path='extradoc/class[@name="IntList"]/*' />
public class IntList { ... }
```

ve aşağıdaki içeriği dış dosya "docs.xml" sahipti:

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

Kaynak kodu içeriyorsa gibi aynı belgeleri çıktı olacaktır:

```csharp
/// <summary>
///    Contains a list of integers.
/// </summary>
public class IntList { ... }
```

### `<list>`

Bu etiket, liste veya Tablo öğesi oluşturmak için kullanılır. İçerebilir bir `<listheader>` satırında bir tablo veya tanım listesi tanımlamak için blok. (Bir tablo için yalnızca bir giriş tanımlarken `term` başlıkta sağlanması.)

Listedeki her bir öğe ile belirtilen bir `<item>` blok. Bir tanım listesi oluştururken hem de `term` ve `description` belirtilmesi gerekir. Ancak, bir tablo, madde işaretli liste veya numaralı liste, yalnızca için `description` belirtilmesi.

__Sözdizimi:__

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

* `term` tanımları konusu tanımlamak için terimi `description`.
* `description` bir madde işareti veya numaralı liste bir öğeyi veya tanımı bir `term`.

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

Bu etiket diğer etiketler iç kullanım için olduğu gibi `<summary>` ([`<remark>`](documentation-comments.md#remark)) veya `<returns>` ([`<returns>`](documentation-comments.md#returns)) ve metin eklenecek yapısı izin verir.

__Sözdizimi:__

```xml
<para>content</para>
```

Burada `content` metni paragraf görüntülenir.

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

Bu etiket, yöntem, oluşturucu veya dizin oluşturucu için bir parametre tanımlamak için kullanılır.

__Sözdizimi:__

```xml
<param name="name">description</param>
```

Burada

* `name` parametrenin adıdır.
* `description` parametre açıklamasıdır.

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

Bu etiket, bir sözcüğün bir parametre olduğunu belirtmek için kullanılır. Soubor dokumentace bu parametreyi belirgin bir şekilde biçimlendirmek için işlenebilir.

__Sözdizimi:__

```xml
<paramref name="name"/>
```

Burada `name` parametrenin adıdır.

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

Bu etiket belgelenecektir üye güvenlik erişilebilirliğini sağlar.

__Sözdizimi:__

```xml
<permission cref="member">description</permission>
```

Burada

* `member` bir üyenin adıdır. Belirli bir kod öğesi var ve çevirir belgeleri Oluşturucu denetler *üye* soubor dokumentace kurallı öğesi adı.
* `description` üye erişimi açıklamasıdır.

__Örnek:__

```csharp
/// <permission cref="System.Security.PermissionSet">Everyone can
/// access this method.</permission>

public static void Test() {
    // ...
}
```

### `<remark>`

Bu etiket, bir türle ilgili ek bilgileri belirtmek için kullanılır. (Kullanım `<summary>` ([`<summary>`](documentation-comments.md#summary)) türü ve bir türün üyelerini tanımlamak için.)

__Sözdizimi:__

```xml
<remark>description</remark>
```

Burada `description` açıklama metni.

__Örnek:__

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

Bu etiket, bir yöntemin dönüş değerini tanımlamak için kullanılır.

__Sözdizimi:__

```xml
<returns>description</returns>
```

Burada `description` dönüş değeri bir açıklaması verilmektedir.

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

Bu etiket metin içinde belirtilmesi için bir bağlantı sağlar. Kullanım `<seealso>` ([`<seealso>`](documentation-comments.md#seealso)) bir Ayrıca bkz. bölümünde görüntülenecek olan metin göstermek için.

__Sözdizimi:__

```xml
<see cref="member"/>
```

Burada `member` üyesi adıdır. Belirli bir kod öğesi var ve değişiklikleri belgeleri Oluşturucu denetler *üye* oluşturulan belgeleri dosyasındaki öğesi adı.

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

Bu etiket için Ayrıca bakınız bölümüne oluşturulması gereken bir giriş sağlar. Kullanım `<see>` ([`<see>`](documentation-comments.md#see)) bağlantı metninde belirtmek için.

__Sözdizimi:__

```xml
<seealso cref="member"/>
```

Burada `member` üyesi adıdır. Belirli bir kod öğesi var ve değişiklikleri belgeleri Oluşturucu denetler *üye* oluşturulan belgeleri dosyasındaki öğesi adı.

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

Bu etiket, bir tür veya üye türü tanımlamak için kullanılabilir. Kullanım `<remark>` ([`<remark>`](documentation-comments.md#remark)) türünü belirtmek için.

__Sözdizimi:__

```xml
<summary>description</summary>
```

Burada `description` bir türe veya üyeye özetidir.

__Örnek:__

```csharp
/// <summary>This constructor initializes the new Point to (0,0).</summary>
public Point() : this(0,0) {
}
```

### `<value>`

Bu etiket tanımlanmış bir özellik sağlar.

__Sözdizimi:__

```xml
<value>property description</value>
```

Burada `property description` özelliği için bir açıklama.

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

Bu etiket, genel tür parametresi için bir sınıf, yapı, arabirim, temsilci veya yöntemi tanımlamak için kullanılır.

__Sözdizimi:__

```xml
<typeparam name="name">description</typeparam>
```

Burada `name` tür parametresi adıdır ve `description` kendi açıklamasıdır.

__Örnek:__

```csharp
/// <summary>A generic list class.</summary>
/// <typeparam name="T">The type stored by the list.</typeparam>
public class MyList<T> {
    ...
}
```

### `<typeparamref>`

Bu etiket, bir sözcüğün bir tür parametresi olduğunu belirtmek için kullanılır. Soubor dokumentace, bu tür parametresi farklı şekilde biçimlendirmek için işlenebilir.

__Sözdizimi:__

```xml
<typeparamref name="name"/>
```

Burada `name` tür parametresinin adı.

__Örnek:__

```csharp
/// <summary>This method fetches data and returns a list of <typeparamref name="T"/>.</summary>
/// <param name="query">query to execute</param>
public List<T> FetchData<T>(string query) {
    ...
}
```

## <a name="processing-the-documentation-file"></a>Soubor dokumentace işleme

Belgeleri Oluşturucu bir belge açıklaması ile etiketlenir kaynak kod içindeki her öğe için bir kimlik dizesi oluşturur. Bu kimlik dizesi, bir kaynak öğesi benzersiz olarak tanımlar. Bir belge Görüntüleyici, bir kimlik dizesi belgeleri uygulandığı ilgili meta verileri/yansıma öğeyi tanımlamak için kullanabilirsiniz.

Soubor dokumentace kaynak kodunu hiyerarşik temsili değildir; Bunun yerine, her öğe için oluşturulan bir kimlik dizesi ile düz bir liste olduğu.

### <a name="id-string-format"></a>Kimlik dizesi biçimi

Kimliği dizesi oluştururken aşağıdaki kuralları belgeleri Oluşturucu gözlemler:

*  Hiçbir boşluk dizesinde yerleştirilir.

*  Dizenin ilk bölümü, tek bir karakter üste aracılığıyla belgelenmiş üye türünü tanımlar. Aşağıdaki türde üye tanımlanır:

   | __Karakter__ | __Açıklama__                                             |
   |---------------|-------------------------------------------------------------|
   | E             | Olay                                                       |
   | F             | Alan                                                       |
   | M             | (Oluşturucular, Yıkıcılar ve işleçler dahil) yöntemi |
   | N             | Ad Alanı                                                   |
   | P             | Özellik (dizin oluşturucular dahil)                               |
   | T             | (Sınıfı, temsilci, enum, arabirimi ve yapı gibi) yazın |
   | !             | Hata dizesi; dizenin geri kalanı, hata hakkında bilgi sağlar. Örneğin, belgeleri Oluşturucu çözümlenemeyen bağlantılar için hata bilgisi oluşturur. |

*  İkinci dize ad alanı kökünde başlangıç öğesi tam olarak nitelenmiş adını parçasıdır. Öğe, kapsayan türleri ve ad alanı adı noktalarla ayrılmış. Öğenin adını nokta varsa, bunlar tarafından değiştirilir `#(U+0023)` karakter. (Hiçbir öğe adını bu karakteri olduğunu varsayılır.)
*  Yöntemleri ve özellikleri parantez içindeki bağımsız değişken, bağımsız değişken listesi aşağıdadır. Bu bağımsız değişkenler olmadan parantezler göz ardı edilir. Bağımsız değişkenlerin virgülle ayrılır. Kodlama her bağımsız değişkeni bir CLI imza aynı şu şekildedir:
   *  Bağımsız değişkenler şu şekilde değiştirilmiş kendi tam adına göre kendi belgesi adı tarafından temsil edilir:
      * Genel türleri temsil eden bağımsız değişkenlere sahip bir eklenmiş "'" karakteri ve ardından tür parametrelerinin sayısı
      * Bağımsız değişkenleri olan `out` veya `ref` değiştiricisine sahip bir `@` izleyerek kendi tür adı. Geçirilen bağımsız değişkenleri değere veya aracılığıyla `params` hiçbir özel gösterimi vardır.
      * Dizileri bağımsız değişkenleri olarak temsil edilir `[lowerbound:size, ... , lowerbound:size]` burada virgüllerle küçük bir boyut sayısıdır ve her boyut, boyutu ve alt sınırlarını biliniyorsa, ondalık olarak gösterilir. Alt sınır veya boyutu belirtilmezse atlanır. Belirli bir boyut için boyut ve alt sınır atlanırsa, "`:`" de atlanır. Basit diziler tarafından temsil edilir "`[]`" düzeyi başına.
      * İşaretçi türleri void dışında olan bağımsız değişkenler kullanılarak temsil edilir bir `*` aşağıdaki tür adı. Void bir işaretçi türü adını kullanılarak temsil edilir `System.Void`.
      * Genel tür parametreleri türlerinde tanımlanan başvuran bağımsız değişkenler kullanılarak kodlanır "'" karakteri ve ardından tür parametresi sıfır tabanlı dizini.
      * Yöntemleri tanımlanan genel tür parametrelerini kullanan bağımsız değişkenleri kullanma double-vurgulamasını belirtir "\`\`" yerine "\`" türleri için kullanılır.
      * Oluşturulan genel türler için başvuru bağımsız değişkenleri, ardından genel türü kullanılarak kodlanır "{", tür bağımsız değişkenleri virgülle ayrılmış listesi tarafından izlenen, ardından "}".

### <a name="id-string-examples"></a>Kimlik dizesi örnekleri

Aşağıdaki örnekler her C# kod, her kaynak öğesi yeteneğine sahip bir belge açıklaması kilitlenmelerinden üretilen kimliği dizesi ile birlikte bir parçasını gösterir:

*  Türleri, genel bilgiler ile genişletilmiş kendi tam adı kullanılarak temsil edilir:

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

*  Alanları tam adına göre temsil edilir:

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

*  Oluşturucular.

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

*  Yok ediciler.

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

*  Yöntemleri.

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

*  Özellikler ve dizin oluşturucular.

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

*  olaylar.

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

*  Birli işleçler.

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

   Birli işleç işlev adlarını kullanılan kümesinin tamamını aşağıdaki gibidir: `op_UnaryPlus`, `op_UnaryNegation`, `op_LogicalNot`, `op_OnesComplement`, `op_Increment`, `op_Decrement`, `op_True`, ve `op_False`.

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

   Kullanılan ikili işleç işlev adları tüm kümesini aşağıdaki gibidir: `op_Addition`, `op_Subtraction`, `op_Multiply`, `op_Division`, `op_Modulus`, `op_BitwiseAnd`, `op_BitwiseOr`, `op_ExclusiveOr`, `op_LeftShift`, `op_RightShift`, `op_Equality`, `op_Inequality`, `op_LessThan`, `op_LessThanOrEqual`, `op_GreaterThan`, ve `op_GreaterThanOrEqual`.

*  Dönüştürme işleçleri sahip bir sondaki "`~`" dönüş türü tarafından izlenen.

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

## <a name="an-example"></a>Bir örnek

### <a name="c-source-code"></a>C# kaynak kodu

Aşağıdaki örnekte kaynak kodunu gösteren bir `Point` sınıfı:

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

Kaynak kodu için sınıf verildiğinde bir belge Oluşturucu tarafından oluşturulan çıktı şu şekildedir `Point`yukarıda gösterilen:

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
