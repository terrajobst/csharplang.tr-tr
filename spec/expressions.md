---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704082"
---
# <a name="expressions"></a>İfadeler

İfade, işleç ve işlenen dizisidir. Bu bölümde sözdizimi, işlenenler ve işleçler değerlendirmesi sırası ve ifadelerin anlamı tanımlanmaktadır.

## <a name="expression-classifications"></a>İfade sınıflandırmaları

Bir ifade, aşağıdakilerden biri olarak sınıflandırıldı:

*  Bir değer. Her değerin ilişkili bir türü vardır.
*  Bir değişken. Her değişkenin bir ilişkili türü vardır, yani bu değişkenin belirtilen türü.
*  Ad alanı. Bu sınıflandırmayla bir ifade, yalnızca bir *member_access* ([üye erişimi](expressions.md#member-access)) sol tarafında görünür. Diğer bir bağlamda, ad alanı olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.
*  Bir tür. Bu sınıflandırmayla bir ifade, yalnızca bir *member_access* ([üye erişimi](expressions.md#member-access)) sol tarafı veya `as` işleci ([as işleci](expressions.md#the-as-operator)), `is` işleci ([IS işleci](expressions.md#the-is-operator)) veya @no için bir işlenen olarak yer alabilir __T-6 işleci ([typeof işleci](expressions.md#the-typeof-operator)). Başka bir bağlamda, tür olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.
*  Bir üye aramasının ([üye arama](expressions.md#member-lookup)) sonucu olan aşırı yüklenmiş yöntemler kümesi olan bir yöntem grubu. Bir yöntem grubunun ilişkili bir örnek ifadesi ve ilişkili bir tür bağımsız değişkeni listesi olabilir. Bir örnek yöntemi çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) ile temsil edilen örnek haline gelir. Bir *invocation_expression* ([çağırma ifadelerinde](expressions.md#invocation-expressions)), bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) ve bir bir işleç işlecinin sol tarafı olarak bir yöntem grubuna izin verilir ve örtülü olarak olabilir uyumlu bir temsilci türüne dönüştürüldü ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)). Diğer bir bağlamda, Yöntem grubu olarak sınıflandırılan bir ifade derleme zamanı hatasına neden olur.
*  Null bir sabit değer. Bu sınıflandırmayla bir ifade, örtük olarak bir başvuru türüne veya null yapılabilir türe dönüştürülebilir.
*  Anonim bir işlev. Bu sınıflandırmayla bir ifade, örtülü olarak uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürülebilir.
*  Özellik erişimi. Her özellik erişiminin, özelliğin türü olarak ilişkili bir türü vardır. Ayrıca, özellik erişiminin ilişkili bir örnek ifadesi olabilir. Örnek özellik erişiminin bir erişimcisi (`get` veya `set` bloğu) çağrıldığında, örnek ifadesinin değerlendirilme sonucu, `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek haline gelir.
*  Bir olay erişimi. Her olay erişiminde ilişkili bir tür bulunur, bu olay türü olayın türüdür. Ayrıca, bir olay erişiminin ilişkili bir örnek ifadesi olabilir. @No__t-0 ve `-=` işleçlerinin ([olay atama](expressions.md#event-assignment)) sol tarafında bir olay erişimi görüntülenebilir. Diğer bir bağlamda, olay erişimi olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.
*  Dizin Oluşturucu erişimi. Her Dizin Oluşturucu erişiminin ilişkili bir türü vardır, yani dizin oluşturucunun öğe türü. Ayrıca, Dizin Oluşturucu erişiminin ilişkili bir örnek ifadesi ve ilişkili bağımsız değişken listesi vardır. Bir Dizin Oluşturucu erişiminin bir erişimcisi (`get` veya `set` bloğu) çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek olur ve bağımsız değişken listesinin değerlendirilme sonucu, çağrının parametre listesi.
*  Yapma. Bu, ifade `void` dönüş türüne sahip bir yöntemin çağrılışında oluşur. Nothing olarak sınıflandırılan bir ifade yalnızca bir *statement_expression* ([Expression deyimleri](statements.md#expression-statements)) bağlamında geçerlidir.

Bir ifadenin nihai sonucu hiçbir şekilde bir ad alanı, tür, Yöntem grubu veya olay erişimdir. Bunun yerine, yukarıda belirtildiği gibi, bu ifade kategorileri yalnızca belirli bağlamlarda izin verilen ara yapılardır.

Bir özellik erişimi veya Dizin Oluşturucu erişimi, *Get erişimcisinin* veya *set erişimcisinin*çağrılması gerçekleştirerek her zaman bir değer olarak yeniden sınıflanır. Belirli erişimci, özelliğin veya dizin oluşturucunun erişiminin bağlamına göre belirlenir: Erişim bir atamanın hedefi ise, *küme erişimcisi* yeni bir değer ([basit atama](expressions.md#simple-assignment)) atamak için çağrılır. Aksi takdirde, *get erişimcisi* geçerli değeri ([ifadelerin değerleri](expressions.md#values-of-expressions)) almak için çağrılır.

### <a name="values-of-expressions"></a>İfadelerin değerleri

Bir ifadeyi içeren yapıların çoğu sonunda ifadenin bir ***değeri***belirtmek için gereklidir. Bu gibi durumlarda, gerçek ifade bir ad alanı, bir tür, bir yöntem grubu veya hiçbir şey ifade ediyorsa, derleme zamanı hatası oluşur. Ancak, ifade bir özellik erişimi, Dizin Oluşturucu erişimi veya bir değişken ise, özelliğin, dizin oluşturucunun veya değişkenin değeri örtük olarak yerine kullanılır:

*  Bir değişkenin değeri, değişken tarafından tanımlanan depolama konumunda şu anda depolanan değer olacaktır. Değerin alınabilmesi için bir değişken kesin olarak atanmış ([kesin atama](variables.md#definite-assignment)) olarak kabul edilmelidir, aksi takdirde bir derleme zamanı hatası oluşur.
*  Özellik erişim ifadesinin değeri, özelliğin *get erişimcisi* çağrılarak elde edilir. Özelliğin *get erişimcisi*yoksa, bir derleme zamanı hatası oluşur. Aksi takdirde, bir işlev üye çağrısı ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir ve çağrının sonucu özellik erişim ifadesinin değeri olur.
*  Dizin Oluşturucu erişim ifadesinin değeri, dizin oluşturucunun *get erişimcisi* çağrılarak elde edilir. Dizin oluşturucunun *get erişimcisi*yoksa, bir derleme zamanı hatası oluşur. Aksi takdirde, Dizin Oluşturucu erişim ifadesiyle ilişkili bağımsız değişken listesiyle bir işlev üye çağrısı ([dinamik aşırı yükleme çözümlemesi çözümleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir ve çağrının sonucu dizin oluşturucunun erişim değeri olur ifadesini.

## <a name="static-and-dynamic-binding"></a>Statik ve dinamik bağlama

Bileşen ifadelerinin türü veya değerine (bağımsız değişkenler, işlenenler, alıcılar) göre bir işlemin anlamını belirleme işlemi genellikle ***bağlama***olarak adlandırılır. Örneğin, bir yöntem çağrısının anlamı alıcı ve bağımsız değişkenlerin türüne göre belirlenir. Bir işlecin anlamı, işlenenlerinin türüne göre belirlenir.

C# Bir işlemin anlamı genellikle derleme zamanında, bileşen ifadelerinin derleme zamanı türüne göre belirlenir. Benzer şekilde, bir ifade hata içeriyorsa, hata algılanır ve derleyici tarafından raporlanır. Bu yaklaşım ***statik bağlama***olarak bilinir.

Ancak, bir ifade dinamik bir ifadesiyse (yani, türü `dynamic`) Bu, içinde yer aldığı herhangi bir bağlamanın kendi çalışma zamanı türünü (yani çalışma zamanında gösterdiği nesnenin gerçek türü) temel aldığı tür değil, derleme zamanı. Bu nedenle, bu tür bir işlemin bağlanması, programın çalışması sırasında işlemin gerçekleştirileceği zamana kadar ertelenir. Bu, ***dinamik bağlama***olarak adlandırılır.

Bir işlem dinamik olarak bağlandığında, derleyici tarafından çok az veya hiçbir denetim yapılmaz. Çalışma zamanı bağlama başarısız olursa, hatalar çalışma zamanında özel durum olarak bildirilir.

' De C# aşağıdaki işlemler bağlamaya tabidir:

*  Üye erişimi: `e.M`
*  Yöntem çağırma: `e.M(e1, ..., eN)`
*  Temsilci çağrısı: `e(e1, ..., eN)`
*  Öğe erişimi: `e[e1, ..., eN]`
*  Nesne oluşturma: `new C(e1, ..., eN)`
*  Aşırı yüklenmiş Birli İşleçler: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Aşırı yüklenmiş ikili işleçler: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, 0, 1, 2, 3, 4, 5, 6, 7
*  Atama işleçleri: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, 0
*  Örtük ve açık dönüştürmeler

Hiçbir dinamik ifade dahil edildiğinde, C# varsayılan statik bağlama olur, bu, bileşen ifadelerinin derleme zamanı türlerinin seçim sürecinde kullanıldığı anlamına gelir. Ancak, yukarıda listelenen işlemler içindeki yapısal ifadelerden biri dinamik bir ifade olduğunda, işlem bunun yerine dinamik olarak bağlanır.

### <a name="binding-time"></a>Bağlama Zamanı

Statik bağlama, derleme zamanında gerçekleşir, ancak dinamik bağlama çalışma zamanında gerçekleşir. Aşağıdaki bölümlerde, bağlama ***zamanı*** terimi, bağlamanın ne zaman gerçekleşdiğine bağlı olarak, derleme zamanı veya çalışma zamanı anlamına gelir.

Aşağıdaki örnek, statik ve dinamik bağlamanın ve bağlama zamanının önemli sayısını göstermektedir:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

İlk iki çağrı statik olarak bağlanmıştır: `Console.WriteLine` ' nın aşırı yüklemesi, bağımsız değişkeninin derleme zamanı türüne göre çekilir. Bu nedenle, bağlama zamanı derleme zamanı olur.

Üçüncü çağrı dinamik olarak bağlı: `Console.WriteLine` ' nın aşırı yüklemesi, bağımsız değişkeninin çalışma zamanı türüne göre çekilir. Bağımsız değişken dinamik bir ifade olduğu için bunun nedeni, derleme zamanı türü `dynamic` ' dır. Bu nedenle, üçüncü çağrının bağlama zamanı çalışma zamanı olur.

### <a name="dynamic-binding"></a>Dinamik bağlama

Dinamik bağlamanın amacı, C# programların ***dinamik nesnelerle***etkileşime geçmesini sağlamaktır, yani C# tür sisteminin normal kurallarını takip etmez. Dinamik nesneler, farklı türlerde sistemlerle diğer programlama dillerinden nesneler olabilir veya farklı işlemler için kendi bağlama semantiğini uygulamak üzere programlı bir şekilde kurulum olan nesneler olabilir.

Dinamik bir nesnenin kendi semantiğini uyguladığı mekanizma uygulama tanımlı ' dır. Verilen bir arabirim (yeniden tanımlanmış bir uygulama), dinamik nesneler tarafından özel semantiklere sahip oldukları C# çalışma zamanına işaret etmek için uygulanır. Bu nedenle, dinamik bir nesne üzerindeki her bir işlem dinamik olarak bağlandığında, bu belgede belirtiler C# yerine kendi bağlama semantiğinin yerine geçer.

Dinamik bağlamanın amacı dinamik nesnelerle birlikte çalıştırılmasına izin verirken, C# dinamik olmasına bakılmaksızın tüm nesnelerde dinamik bağlamaya izin verir. Bu, dinamik nesnelerin daha yumuşak bir şekilde tümleştirilmesini sağlar, çünkü bunlar üzerindeki işlemlerin sonuçları dinamik nesneler olmayabilir, ancak hala derleme zamanında programcıya bilinmeyen bir tür olabilir. Ayrıca dinamik bağlama, hiçbir nesne dahil, dinamik nesneler olmasa bile hataya açık olan yansıma tabanlı kodu ortadan kaldırmaya yardımcı olabilir.

Aşağıdaki bölümlerde, dinamik bağlama uygulandığında, her bir yapı için, dinamik bağlama uygulandığında, derleme zaman denetimi (varsa), varsa derleme zamanı ve ifade sınıflandırmasının ne olduğu ile ilgili olarak açıklanır.

### <a name="types-of-constituent-expressions"></a>Anayent ifadelerin türleri

Bir işlem statik olarak bağlandığında, bir bileşen ifadesinin türü (örn. bir alıcı, bir bağımsız değişken, bir dizin veya işlenen), her zaman bu ifadenin derleme zamanı türü olarak değerlendirilir.

Bir işlem dinamik olarak bağlandığında, bileşen ifadesinin türü, bileşen ifadesinin derleme zamanı türüne bağlı olarak farklı yollarla belirlenir:

*  @No__t-0 derleme zamanı türünün yapısal ifadesi, ifadenin çalışma zamanında değerlendirilen gerçek değer türüne sahip olarak kabul edilir
*  Derleme zamanı türü bir tür parametresi olan bir anayent ifadesi tür parametresinin çalışma zamanında bağlı olduğu türe sahip olarak kabul edilir
*  Aksi halde, oluşturan ifade derleme zamanı türüne sahip olarak kabul edilir.

## <a name="operators"></a>İşleçler

İfadeler, ***işlenenler*** ve ***işleçlerden***oluşturulur. Bir ifadenin işleçleri, işlenenlerin hangi işlemleri uygulanacağını gösterir. `+`İşleç örnekleri `-` ,`/`,, ve`new`içerir. `*` İşlenenlerin örnekleri, sabit değerleri, alanları, yerel değişkenleri ve ifadeleri içerir.

Üç tür işleç vardır:

*  Birli İşleçler. Birli İşleçler bir işlenen alır ve önek gösterimini (`--x`) ya da sonek gösterimini (örneğin, `x++`) kullanır.
*  İkili işleçler. İkili işleçler iki işlenen alır ve tüm kullanım dışı gösterimi (`x + y`) kullanır.
*  Üçlü işleç. Yalnızca bir üçlü operatör, `?:`, var; üç işlenen alır ve geçersiz kılma gösterimi kullanır (`c ? x : y`).

Bir ifadedeki işleçlerin değerlendirilme sırası, işleçlerin ***Öncelik*** ve ***ilişkilendirilebilirliği*** ([işleç önceliği ve ilişkilendirilebilirlik](expressions.md#operator-precedence-and-associativity)) tarafından belirlenir.

Bir ifadedeki işlenenler soldan sağa değerlendirilir. Örneğin `F(i) + G(i++) * H(i)` ' da `F` yöntemi `i` ' nin eski değeri kullanılarak çağrılır, `G` yöntemi eski `i` değeri ile çağrılır ve son olarak `H` yöntemi `i` değeri ile çağırılır. Bu, ' den farklıdır ve işleç önceliğine ilgisiz değildir.

Bazı işleçler ***aşırı***yüklenebilir. İşleç aşırı yüklemesi, Kullanıcı tanımlı operatör uygulamalarının bir veya her ikisinin de Kullanıcı tanımlı sınıf veya yapı türünde ([operatör aşırı yüklemesi](expressions.md#operator-overloading)) olduğu işlemler için belirtilmesine izin verir.

### <a name="operator-precedence-and-associativity"></a>İşleç önceliği ve ilişkilendirilebilirlik

Bir ifade birden çok işleç içerdiğinde, işleçlerin ***önceliği*** ayrı işleçlerin değerlendirilme sırasını denetler. Örneğin, `*` işlecinin ikili `+` işlecinden daha yüksek önceliği olduğundan `x + y * z` ifadesi `x + (y * z)` olarak değerlendirilir. Bir işlecin önceliği, ilişkili dilbilgisi üretiminin tanımıyla belirlenir. Örneğin, bir *additive_expression* , `+` veya `-` işleçleriyle ayrılmış bir *multiplicative_expression*s dizisinden oluşur. bu nedenle, `*`, `/` ve @no__ kıyasla `+` ve `-` işleçleri daha düşük önceliğe sahiptir t-8 işleçleri.

Aşağıdaki tablo, en yüksekten en düşüğe öncelik sırasına göre tüm işleçleri özetler:

| __Kısmı__                                                                                   | __Kategori__                | __İşleçler__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Birincil ifadeler](expressions.md#primary-expressions)                                     | Birincil                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Birli İşleçler](expressions.md#unary-operators)                                             | Li                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Aritmetik işleçler](expressions.md#arithmetic-operators)                                   | Çarpma              | `*`  `/`  `%` | 
| [Aritmetik işleçler](expressions.md#arithmetic-operators)                                   | Msal                    | `+`  `-`      | 
| [Kaydırma işleçleri](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [İlişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators) | İlişkisel ve tür testi | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [İlişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators) | Eşitlik                    | `==`  `!=`    | 
| [Mantıksal işleçler](expressions.md#logical-operators)                                         | Mantıksal VE                 | `&`           | 
| [Mantıksal işleçler](expressions.md#logical-operators)                                         | Mantıksal XOR                 | `^`           | 
| [Mantıksal işleçler](expressions.md#logical-operators)                                         | Mantıksal VEYA                  | <code>&#124;</code>           |
| [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)                 | Koşullu VE             | `&&`          | 
| [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)                 | Koşullu VEYA              | <code>&#124;&#124;</code>          | 
| [Null birleştirme işleci](expressions.md#the-null-coalescing-operator)                   | Null birleşim             | `??`          | 
| [Koşullu işleç](expressions.md#conditional-operator)                                   | Koşullu                 | `?:`          | 
| [Atama işleçleri](expressions.md#assignment-operators), [anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)  | Atama ve lambda ifadesi | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Aynı önceliğe sahip iki işleç arasında bir işlenen gerçekleştiğinde, işleçlerin ilişkilendirilebilirliği, işlemlerin gerçekleştirileceği sırayı denetler:

*  Atama işleçleri ve null birleşim işleci dışında, tüm ikili işleçler ***sola ilişkilendirilebilir***, yani işlemler soldan sağa yapılır. Örneğin, `x + y + z` olarak `(x + y) + z`değerlendirilir.
*  Atama işleçleri, null birleşim işleci ve koşullu işleç (`?:`) ***doğru ilişkilendirilebilir***, yani işlemler sağdan sola yapılır. Örneğin, `x = y = z` olarak `x = (y = z)`değerlendirilir.

Öncelik ve ilişkilendirilebilirlik, parantezler kullanılarak denetlenebilir. Örneğin, `x + y * z` ' ı ilk önce `z` ile @no__t çarpar ve sonra sonucu `x` ' e ekler, ancak `(x + y) * z` ' ü önce `x` ve `y` ' ı ekler ve ardından sonucu @no__t 7 ile çarpar.

### <a name="operator-overloading"></a>İşleç aşırı yüklemesi

Tüm birli ve ikili işleçlerin, herhangi bir ifadede otomatik olarak kullanılabilen önceden tanımlanmış uygulamaları vardır. Önceden tanımlanmış uygulamalara ek olarak, Kullanıcı tanımlı uygulamalar, sınıflar ve yapılar ([işleçler](classes.md#operators)) `operator` bildirimleri eklenerek tanıtılamaz. Kullanıcı tanımlı operatör uygulamaları, her zaman önceden tanımlanmış operatör uygulamalarından önceliklidir: Yalnızca geçerli bir Kullanıcı tanımlı operatör uygulaması mevcut olmadığında, [birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)bölümünde açıklandığı gibi önceden tanımlanmış operatör uygulamaları göz önünde bulundurulacaktır.

***Fazla yüklenebilir Birli İşleçler*** şunlardır:
```csharp
+   -   !   ~   ++   --   true   false
```

@No__t-0 ve `false` açıkça ifadelerde kullanılmamalıdır (ve bu nedenle [işleç önceliği ve ilişkilendirilebilirliği](expressions.md#operator-precedence-and-associativity)içindeki öncelik tablosuna dahil edilmez), bunlar birkaç ifadede çağrıldıklarından işleçler olarak kabul edilir bağlamlar: Boolean ifadeleri ([Boolean](expressions.md#boolean-expressions)ifadeleri) ve koşullu ([koşullu işleç](expressions.md#conditional-operator)) ve koşullu mantıksal işleçler (koşullu[mantıksal](expressions.md#conditional-logical-operators)işleçler) içeren ifadeler.

***Fazla yüklenebilir ikili işleçler*** şunlardır:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Yalnızca yukarıda listelenen operatörler aşırı yüklenebilir. Özellikle, üye erişimi, yöntem çağırma veya `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, 0, 1 ve 2 işleçlerini aşırı yüklemek mümkün değildir.

İkili işleç aşırı yüklendiğinde, karşılık gelen atama işleci de dolaylı olarak aşırı yüklenmiştir. Örneğin, `*` işlecinin aşırı yüklemesi aynı zamanda `*=` işlecinin aşırı yüküne sahiptir. Bu, [bileşik atamada](expressions.md#compound-assignment)daha ayrıntılı olarak açıklanmıştır. Atama işlecinin kendisinin (`=`) aşırı yüklü olamayacağını unutmayın. Atama her zaman bir değeri bir değişkene basit bit temelinde bir kopyasını gerçekleştirir.

@No__t-0 gibi atama işlemleri, Kullanıcı tanımlı dönüştürmeler ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) sağlanarak aşırı yüklenmiştir.

@No__t-0 gibi öğe erişimi, aşırı yüklenebilir bir operatör olarak kabul edilmez. Bunun yerine, Kullanıcı tanımlı dizin oluşturucular ([Dizin oluşturucular](classes.md#indexers)) aracılığıyla desteklenir.

İfadelerde, işleçlere işleç gösterimi kullanılarak başvurulur ve bildirimlerinde, işlev gösterimi kullanılarak işleçlere başvurulur. Aşağıdaki tabloda, birli ve ikili işleçler için işleç ve işlevsel gösterimler arasındaki ilişki gösterilmektedir. İlk girişte, *op* fazla yüklenebilir birli ön ek işlecini gösterir. İkinci girişte, *op* birli sonek `++` ve `--` işleçlerini gösterir. Üçüncü girişte, *op* , aşırı yüklenebilir ikili işleç olduğunu gösterir.


| __İşleç gösterimi__ | __İşlevsel Gösterim__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Kullanıcı tanımlı işleç bildirimleri her zaman, işleç bildirimini içeren sınıf veya yapı türünde parametrelerden en az birini gerektirir. Bu nedenle, Kullanıcı tanımlı bir işlecin önceden tanımlanmış bir işleçle aynı imzaya sahip olması mümkün değildir.

Kullanıcı tanımlı işleç bildirimleri bir işlecin sözdizimini, önceliğini veya ilişkilendirilebilirliğini değiştiremez. Örneğin, `/` işleci her zaman bir ikili işleçtir, her zaman [işleç önceliği ve ilişkilendirilebilirliği](expressions.md#operator-precedence-and-associativity)içinde belirtilen öncelik düzeyine sahiptir ve her zaman sola ilişkilendirilebilir.

Kullanıcı tanımlı bir işlecin, kiralamaları yaptığı herhangi bir hesaplamayı gerçekleştirmesi mümkün olsa da, büyük bir süre içinde olması beklenen, farklı sonuçlar üreten uygulamalar kesinlikle önerilmez. Örneğin, `operator ==` ' ın uygulanması iki işleneni eşitlik için karşılaştırmalı ve uygun bir `bool` sonucu döndürmelidir.

[Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators) aracılığıyla [birincil ifadelerde](expressions.md#primary-expressions) bağımsız işleçlerin açıklamaları, işleçlerin önceden tanımlanmış uygulamalarını ve her bir operatör için uygulanan ek kuralları belirtir. Açıklamalar ***birli operatör aşırı yükleme çözünürlüğü***, ***ikili işleç aşırı yükleme çözümlemesi***ve ***sayısal yükseltme***, tanımları aşağıdaki bölümlerde bulunan tanımlardan birini kullanır.

### <a name="unary-operator-overload-resolution"></a>Birli operatör aşırı yükleme çözünürlüğü

@No__t-0 veya `x op` biçiminde bir işlem, `op` ' nin aşırı yüklenebilir birli işleç olduğu ve `x` ' i `X` türü bir ifade olduğu için şu şekilde işlenir:

*  @No__t-1 işlemi için `X` tarafından sunulan aday Kullanıcı tanımlı işleçler kümesi, [aday Kullanıcı tanımlı işleçlerin](expressions.md#candidate-user-defined-operators)kuralları kullanılarak belirlenir.
*  Aday Kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçler kümesi olur. Aksi halde, yükseltilmemiş formları dahil olmak üzere önceden tanımlanmış birli `operator op` uygulamaları, işlem için aday işleçler kümesi olur. Belirli bir işlecin önceden tanımlanmış uygulamaları, işlecinin açıklamasında belirtilir ([birincil ifadeler](expressions.md#primary-expressions) ve [Birli İşleçler](expressions.md#unary-operators)).
*  Aşırı [yükleme çözümlemesi](expressions.md#overload-resolution) çözüm kuralları, `(x)` bağımsız değişken listesine göre en iyi işleci seçmek üzere aday işleçler kümesine uygulanır ve bu işleç aşırı yükleme çözümleme işleminin sonucu olur. Aşırı yükleme çözümlemesi tek bir en iyi operatör seçmezse bağlama zamanı hatası oluşur.

### <a name="binary-operator-overload-resolution"></a>İkili işleç aşırı yükleme çözümü

@No__t-0, `op` ' in aşırı yüklenebilir bir ikili işleç olduğu, `x` `X` türünde bir ifadedir ve `y` ' ün `Y` türünde bir ifade olduğu bir işlem aşağıdaki şekilde işlenir:

*  @No__t-0 ve `Y` tarafından sunulan aday Kullanıcı tanımlı işleçler kümesi `operator op(x,y)` ' dir. Küme, `X` tarafından sunulan aday işleçleri birleşiminden ve `Y` tarafından sunulan aday işleçlerden (her biri [aday Kullanıcı tanımlı işleçlerin](expressions.md#candidate-user-defined-operators)kuralları kullanılarak belirlenir) oluşur. @No__t-0 ve `Y` aynı türde veya `X` ve `Y` ortak bir temel türden türetildiyse, paylaşılan aday işleçler yalnızca Birleşik küme içinde bir kez gerçekleşir.
*  Aday Kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçler kümesi olur. Aksi halde, yükseltilmemiş formları dahil önceden tanımlanmış ikili `operator op` uygulamaları, işlem için aday işleçler kümesi olur. Belirli bir işlecin önceden tanımlanmış uygulamaları, işlecinin açıklamasında belirtilir ( [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)aracılığıyla[Aritmetik işleçler](expressions.md#arithmetic-operators) ). Önceden tanımlanmış Enum ve temsilci işleçleri için kabul edilen tek işleçler, işlenenlerden birinin bağlama zamanı türü olan bir Enum veya temsilci türü tarafından tanımlananlar.
*  Aşırı [yükleme çözümlemesi](expressions.md#overload-resolution) çözüm kuralları, `(x,y)` bağımsız değişken listesine göre en iyi işleci seçmek üzere aday işleçler kümesine uygulanır ve bu işleç aşırı yükleme çözümleme işleminin sonucu olur. Aşırı yükleme çözümlemesi tek bir en iyi operatör seçmezse bağlama zamanı hatası oluşur.

### <a name="candidate-user-defined-operators"></a>Aday Kullanıcı tanımlı işleçler

@No__t-0 ve bir işlem `operator op(A)`, `op` aşırı yüklenebilir bir operatör ve `A` de bir bağımsız değişken listesidir, `operator op(A)` için `T` tarafından sağlanan aday Kullanıcı tanımlı işleçler kümesi aşağıdaki şekilde belirlenir:

*  @No__t-0 türünü saptayın. @No__t-0 null yapılabilir bir tür ise, `T0` temel türüdür, aksi takdirde `T0` `T` ' e eşittir.
*  @No__t-1 ' deki tüm `operator op` bildirimleri ve söz konusu operatörlerin tüm yükseltilmemiş formlarında, en az bir operatör uygulanabilir ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)), bağımsız değişken listesi `A` ' e göre geçerliyse, aday işleçler kümesi `T0` ' te uygun işleçler.
*  Aksi takdirde, `T0` `object` ise aday işleçler kümesi boştur.
*  Aksi takdirde, `T0` tarafından sunulan aday işleçleri kümesi, `T0` ' in doğrudan taban sınıfı tarafından sunulan aday işleçler kümesidir veya `T0` bir tür parametresi ise, `T0` ' nin etkin taban sınıfı olur.

### <a name="numeric-promotions"></a>Sayısal yükseltmeler

Sayısal yükseltme, önceden tanımlanmış birli ve ikili sayısal işleçlerin işlenenlerinin belirli örtük dönüştürmelerini otomatik olarak gerçekleştirmekten oluşur. Sayısal yükseltme, farklı bir mekanizma değildir, ancak önceden tanımlanmış işleçlere aşırı yükleme çözümlemesi uygulamanın bir etkisi vardır. Sayısal yükseltme, Kullanıcı tanımlı işleçlerin benzer etkileri sergilemek üzere uygulanabilmesine rağmen özellikle Kullanıcı tanımlı işleçlerin değerlendirilmesini etkilemez.

Sayısal yükseltmede örnek olarak, ikili `*` işlecinin önceden tanımlanmış uygulamalarını göz önünde bulundurun:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Bu işleç kümesine aşırı yükleme çözümleme kuralları ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) uygulandığında, efekt, örtük dönüştürmelerin işlenen türlerinden ilk birini seçmek için kullanılır. Örneğin, `b` ' in bir `byte` olduğu ve `s` ' ü bir `short` @no__t, aşırı yükleme çözümlemesi en iyi operatör olarak `operator *(int,int)` ' i seçer. Bu nedenle, `b` ve `s` `int` ' ye dönüştürülecektir ve sonucun türü `int` ' tür. Benzer şekilde, `i` ' in bir `int` olduğu ve `d` ' ü bir `double` @no__t, aşırı yükleme çözümü en iyi operatör olarak `operator *(double,double)` ' i seçer.

#### <a name="unary-numeric-promotions"></a>Tekli sayı yükseltmeleri

Ön tanımlı `+`, `-` ve `~` birli işleçlerin işlenenleri için tek başına sayısal yükseltme oluşur. Birli sayısal yükseltme yalnızca `sbyte`, `byte`, `short`, `ushort` veya `char` türündeki işlenenleri `int` türüne dönüştürmekten oluşur. Ayrıca, birli `-` işleci için tek başına sayısal promosyon, `uint` türündeki işlenenleri `long` türüne dönüştürür.

#### <a name="binary-numeric-promotions"></a>İkili sayısal yükseltmeler

Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, 0, 1, 2 ve 3 ikili işleçlerinin işlenenleri için ikili sayısal yükseltme oluşur. İkili sayısal yükseltme, ilişkisel olmayan işleçler söz konusu olduğunda her iki işleneni dolaylı olarak ortak bir türe dönüştürür. bu da işlemin sonuç türü olur. İkili sayısal yükseltme aşağıdaki kuralları, burada göründükleri düzende uygulamaktan oluşur:

*  Her iki işlenen `decimal` türünde ise, diğer işlenen `decimal` türüne dönüştürülür veya diğer işlenen `float` veya `double` türünde olduğunda bir bağlama zamanı hatası oluşur.
*  Aksi halde, her iki işlenen `double` türünde ise, diğer işlenen `double` türüne dönüştürülür.
*  Aksi halde, her iki işlenen `float` türünde ise, diğer işlenen `float` türüne dönüştürülür.
*  Aksi halde, her iki işlenen `ulong` türünde ise, diğer işlenen `ulong` türüne dönüştürülür veya diğer işlenen `sbyte`, `short`, `int` veya `long` türünde olduğunda bir bağlama zamanı hatası oluşur.
*  Aksi halde, her iki işlenen `long` türünde ise, diğer işlenen `long` türüne dönüştürülür.
*  Aksi halde, her iki işlenen `uint` türündedir ve diğer işlenen `sbyte`, `short` veya `int` türündedir, her iki işlenen de `long` türüne dönüştürülür.
*  Aksi halde, her iki işlenen `uint` türünde ise, diğer işlenen `uint` türüne dönüştürülür.
*  Aksi halde, her iki işlenen `int` türüne dönüştürülür.

İlk kuralın `decimal` türünü, `double` ve `float` türleriyle karıştırabelirten işlemlere izin vermez. Bu kural, `decimal` türü ile `double` ve `float` türleri arasında örtük dönüştürme olmadığı gerçeden sonraki bir şekildedir.

Ayrıca, diğer işlenen işaretli bir integral türünde olduğunda bir işlenenin `ulong` türünde olması mümkün değildir. Bunun nedeni, `ulong` ' ın tam aralığını ve imzalı integral türlerini temsil eden hiçbir integral türü yok.

Yukarıdaki her iki durumda da, bir işleneni açıkça diğer işlenenle uyumlu bir türe dönüştürmek için bir atama ifadesi kullanılabilir.

Örnekte
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
bir `decimal` `double` ile çarpılamıyor bir bağlama zamanı hatası oluşur. Hata, ikinci işlenen aşağıdaki gibi `decimal` ' a açıkça dönüştürülürken çözümlenir:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Yükseltilmemiş işleçleri

***Yükseltilmemiş işleçleri*** , null olamayan değer türlerinde çalışan, önceden tanımlanmış ve Kullanıcı tanımlı işleçlere izin vererek bu türlerin null yapılabilir formlarla de kullanılmasını sağlar. Yükseltilmemiş işleçleri, aşağıda açıklandığı gibi, belirli gereksinimleri karşılayan önceden tanımlı ve Kullanıcı tanımlı işleçlerden oluşturulur:

*   Birli İşleçler için

    ```csharp
    +  ++  -  --  !  ~
    ```

    bir işlecin yükseltilmemiş formu, işlenen ve sonuç türleri null yapılamayan değer türleri ise vardır. Yükseltilmemiş formu, işlenene ve sonuç türlerine tek bir `?` değiştiricisi eklenerek oluşturulur. Yükseltilmemiş işleci, işlenen null ise null değeri üretir. Aksi takdirde, yükseltilmemiş işleci işleneni geri sarar, temeldeki işleci uygular ve sonucu sarmalar.

*   İkili işleçler için

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    bir işlecin yükseltilmemiş formu, işlenen ve sonuç türleri null yapılamayan tüm değer türleri ise vardır. Yükseltilmemiş formu, her işlenenin ve sonuç türünün tek bir `?` değiştiricisi eklenerek oluşturulur. Yükseltilmemiş işleci, bir veya her iki işlenen de null ise null değeri üretir @no__t ( [Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)bölümünde açıklandığı gibi `bool?` türünün `|` işleçleri). Aksi halde, yükseltilmemiş işleci işlenenleri sarmalanmış, temeldeki işleci uygular ve sonucu sarmalar.

*   Eşitlik işleçleri için

    ```csharp
    ==  !=
    ```

    bir işlecin yükseltilmemiş formu, işlenen türleri hem null yapılamayan değer türleri hem de sonuç türü `bool` ise vardır. Yükseltilmemiş formu, her bir işlenen türüne tek bir `?` değiştiricisi eklenerek oluşturulur. Yükseltilmemiş işleci iki null değeri eşit kabul eder ve null olmayan bir değer null değeri boş değer olarak eşit değildir. Her iki işlenen de null değilse, yükseltilmemiş işleci işlenenleri geri sarar ve `bool` sonucunu üretmek için temeldeki işleci uygular.

*   İlişkisel işleçler için

    ```csharp
    <  >  <=  >=
    ```

    bir işlecin yükseltilmemiş formu, işlenen türleri hem null yapılamayan değer türleri hem de sonuç türü `bool` ise vardır. Yükseltilmemiş formu, her bir işlenen türüne tek bir `?` değiştiricisi eklenerek oluşturulur. Bir veya her iki işlenen de null ise, yükseltilmemiş işleci `false` değerini üretir. Aksi halde, yükseltilmemiş işleci işlenenleri kaldırır ve `bool` sonucunu üretmek için temeldeki işleci uygular.

## <a name="member-lookup"></a>Üye arama

Üye arama, bir tür bağlamındaki bir adın anlamını tespit eden işlemdir. Üye arama, bir ifadede bir *simple_name* ([basit adlar](expressions.md#simple-names)) veya *member_access* ([üye erişimi](expressions.md#member-access)) değerlendirme işleminin parçası olarak gerçekleşebilir. *Simple_name* veya *member_access* , bir *invocation_expression* ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) *primary_expression* olarak gerçekleşirse, üyenin çağrılması söylenir.

Bir üye bir yöntem veya olaydır ya da bir temsilci türü ([Temsilciler](delegates.md)) veya `dynamic` ([dinamik tür](types.md#the-dynamic-type)) türünde bir sabit, alan veya özellik ise, üyenin *ıncable*olarak kabul edilir.

Üye arama yalnızca bir üyenin adını değil, üyenin sahip olduğu tür parametrelerinin sayısını ve üyenin erişilebilir olup olmadığını dikkate alır. Üye arama amaçları için, genel metotlar ve iç içe geçmiş genel türler ilgili bildirimlerinde belirtilen tür parametrelerinin sayısına ve diğer tüm üyelerin sıfır tür parametrelerine sahip olması gerekir.

@ No__t-3 türünde `K` @ no__t-2tür parametreleri ile @ no__t-0 adlı bir üye araması şu şekilde işlenir:

*  İlk olarak, @ no__t-0 adlı bir erişilebilir üye kümesi belirlenir:
    * @No__t-0 bir tür parametresi ise, küme, birincil kısıtlama olarak belirtilen türlerin her birinde @ no__t-1 adlı erişilebilir üye kümelerinin birleşimidir ve bu küme, @ no__t-3 için ikincil kısıtlama ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ile birlikte `object` ' te @ no__t-4 adlı erişilebilir Üyeler.
    * Aksi halde, küme devralınan Üyeler ve `object` ' te @ no__t-3 adlı erişilebilir Üyeler dahil olmak üzere @ no__t-2 içinde @ no__t-1 adlı tüm erişilebilir ([üye erişim](basic-concepts.md#member-access)) üyelerinden oluşur. @No__t-0 oluşturulmuş bir türse, Üyeler kümesi, [oluşturulan türlerin üyelerinde](classes.md#members-of-constructed-types)açıklanan tür bağımsız değişkenlerinin yerine alınır. @No__t-0 değiştiricisi içeren Üyeler kümeden çıkarılır.
*  Sonra, `K` sıfırsa, bildirimleri tür parametreleri içeren tüm iç içe türler kaldırılır. @No__t-0 sıfır değilse, farklı sayıda tür parametrelerine sahip tüm Üyeler kaldırılır. Tür çıkarımı işlemi ([tür çıkarımı](expressions.md#type-inference)) tür bağımsız değişkenlerini çıkarsanbileceği için `K` olduğunda tür parametrelerine sahip Yöntemler kaldırılmaz.
*  Sonra, üye *çağrılırsa*,*çağrılamayan* tüm Üyeler kümeden kaldırılır.
*  Daha sonra, diğer Üyeler tarafından gizlenen Üyeler kümeden kaldırılır. Küme içindeki her üye için @no__t: `S`, @ no__t-2 üyesinin bildirildiği türdür, aşağıdaki kurallar uygulanır:
    * @No__t-0 bir sabit, alan, özellik, olay veya numaralandırma üyesiyse, temel türünde (`S`) belirtilen tüm Üyeler kümeden kaldırılır.
    * @No__t-0 bir tür bildirimidir, `S` ' in temel türünde bildirilmeyen tüm türler kümeden kaldırılır ve `S` temel türünde tanımlanan `M` olarak aynı sayıda parametre içeren tüm tür bildirimleri kümesinden kaldırılır.
    * @No__t-0 bir yöntem ise, `S` temel türünde belirtilen tüm yöntem olmayan Üyeler kümeden kaldırılır.
*  Daha sonra, sınıf üyeleri tarafından gizlenen arabirim üyeleri kümeden kaldırılır. Bu adım yalnızca `T` bir tür parametresidir ve `T` ' in hem `object` ' den hem de boş olmayan bir etkin arabirim kümesi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) dışında etkin bir temel sınıfı varsa, bu adımın bir etkisi vardır. Küme içindeki her üye için @no__t: `S`, üyenin `M` ' nin bildirildiği türdür, `S` `object` dışında bir sınıf bildirimidir, aşağıdaki kurallar uygulanır:
    * @No__t-0 bir sabit, alan, özellik, olay, numaralandırma üyesi veya tür bildirimidir, bir arabirim bildiriminde belirtilen tüm Üyeler kümeden kaldırılır.
    * @No__t-0 bir yöntem ise, bir arabirim bildiriminde belirtilen tüm yöntem olmayan Üyeler kümeden kaldırılır ve arabirim bildiriminde belirtilen `M` ile aynı imzaya sahip tüm yöntemler kümeden kaldırılır.
*  Son olarak, gizli üyeleri kaldırdıkça, aramanın sonucu belirlenir:
    * Küme, yöntem olmayan tek bir üyenin içeriyorsa, bu üye aramanın sonucudur.
    * Aksi takdirde, küme yalnızca yöntemleri içeriyorsa, bu yöntem grubu aramanın sonucudur.
    * Aksi takdirde, arama belirsizdir ve bir bağlama zamanı hatası oluşur.

Tür parametreleri ve arabirimleri dışındaki türlerde üye aramaları ve kesinlikle tek devralma olan arabirimlerde üye aramaları için (devralma zincirindeki her arabirim tam olarak sıfır veya bir doğrudan temel arabirimine sahiptir), arama kurallarının etkisi yalnızca bu türetilmiş Üyeler aynı ad veya imzaya sahip temel üyeleri gizler. Bu tür tek devralma aramaları hiçbir şekilde belirsiz değildir. Birden çok devralma arabirimindeki üye aramalarından muhtemelen olabilecek belirsizlikleri, [arabirim üye erişimi](interfaces.md#interface-member-access)bölümünde açıklanmıştır.

### <a name="base-types"></a>Temel türler

Üye arama amaçları doğrultusunda `T` türü aşağıdaki temel türlere sahip olacak şekilde değerlendirilir:

*  @No__t-0 `object` ise, `T` temel türe sahip değildir.
*  @No__t-0 bir *enum_type*ise, `T` taban türleri `System.Enum`, `System.ValueType` ve `object` sınıflardır.
*  @No__t-0 *ise, @no__t*-2 taban türleri `System.ValueType` ve `object` sınıf türleridir.
*  @No__t-0 ise, `T` Taban *türleri, @no__t*-4 sınıf türü de dahil olmak üzere `T` temel sınıflarıdır.
*  @No__t-0 bir *interface_type*ise, `T` taban türleri `T` ' ün temel arabirimlerdir ve `object` sınıf türüdür.
*  @No__t-0 bir *array_type*ise, `T` taban türleri `System.Array` ve `object` sınıflardır.
*  @No__t-0 *ise, @no__t*-2 taban türleri `System.Delegate` ve `object` sınıf türleridir.

## <a name="function-members"></a>İşlev üyeleri

İşlev üyeleri, çalıştırılabilir deyimler içeren üyeleridir. İşlev üyeleri her zaman türlerin üyeleridir ve ad alanlarının üyeleri olamaz. C#Aşağıdaki işlev üyesi kategorilerini tanımlar:

*  Yöntemler
*  properties
*  Events
*  Dizin Oluşturucular
*  Kullanıcı tanımlı işleçler
*  Örnek oluşturucular
*  Statik oluşturucular
*  Yıkıcılar

Yok ediciler ve statik oluşturucular dışında (açıkça çağrılamaz), işlev üyelerinde bulunan deyimler işlev üyesi etkinleştirmeleri aracılığıyla yürütülür. İşlev üye çağrısı yazmak için gerçek sözdizimi, belirli bir işlev üyesi kategorisine bağlıdır.

İşlev üyesi çağırma işlevinin bağımsız değişken listesi ([bağımsız değişken listeleri](expressions.md#argument-lists)), işlev üyesinin parametreleri için gerçek değerler veya değişken başvuruları sağlar.

Genel yöntemlerin etkinleştirmeleri, yönteme geçirilecek tür bağımsız değişkenleri kümesini belirlemede tür çıkarımı kullanabilir. Bu işlem [tür çıkarımı](expressions.md#type-inference)bölümünde açıklanmaktadır.

Yöntemler, Dizin oluşturucular, işleçler ve örnek oluşturucuların çağırmaları, hangi bir tür üye kümesinin çalıştırılacağını belirleyen aşırı yükleme çözümlemesi sağlar. Bu işlem [aşırı yükleme çözümlemesi](expressions.md#overload-resolution)bölümünde açıklanmaktadır.

Bağlama zamanında belirli bir işlev üyesi tanımlandıktan sonra, muhtemelen aşırı yükleme çözünürlüğü aracılığıyla işlev üyesini çağırma işleminin gerçek çalışma zamanı [denetimi, dinamik aşırı yükleme çözümünün derleme zamanı denetiminde](expressions.md#compile-time-checking-of-dynamic-overload-resolution)açıklanmaktadır.

Aşağıdaki tabloda, açıkça çağrılabilen işlev üyelerinin altı kategorisini içeren yapılar içinde gerçekleşen işlem özetlenmektedir. Tabloda, `e`, `x`, `y` ve `value`, değişkenler veya değerler olarak sınıflandırılan ifadeleri gösterir, `T` tür olarak sınıflandırılan bir ifadeyi belirtir, `F` bir yöntemin basit adıdır ve `P` bir özelliğin basit adıdır.


| __Oluşturma__     | __Örnek__    | __Açıklama__ |
|-------------------|----------------|-----------------|
| Yöntem çağırma | `F(x,y)`       | Aşırı yükleme çözümlemesi, kapsayan sınıf veya yapıda en iyi yöntem `F` ' ı seçmek üzere uygulanır. Yöntemi `(x,y)` bağımsız değişken listesiyle çağrılır. Yöntem `static` değilse, örnek ifadesi `this` ' dir. | 
|                   | `T.F(x,y)`     | Yeniden yükleme çözümlemesi, sınıf veya yapı `T` ' de en iyi yöntem `F` ' ı seçmek üzere uygulanır. Yöntem `static` değilse bir bağlama zamanı hatası oluşur. Yöntemi `(x,y)` bağımsız değişken listesiyle çağrılır. | 
|                   | `e.F(x,y)`     | @No__t-0 türü tarafından verilen sınıf, yapı veya arabirimdeki en iyi yöntemi seçmek için aşırı yükleme çözümlemesi uygulanır. Yöntem `static` olduğunda bağlama zamanı hatası oluşur. Yöntemi, `e` örnek ifadesiyle ve-1 @no__t bağımsız değişken listesinde çağrılır. | 
| Özellik erişimi   | `P`            | İçeren sınıf veya yapı içindeki `P` özelliğinin `get` erişimcisi çağrıldı. @No__t-0 salt yazılır ise, derleme zamanı hatası oluşur. @No__t-0 `static` değilse, örnek ifadesi `this` ' dir. | 
|                   | `P = value`    | İçeren sınıf veya yapı içindeki `P` özelliğinin `set` erişimcisi `(value)` bağımsız değişken listesiyle çağrılır. @No__t-0 salt okunurdur derleme zamanı hatası oluşur. @No__t-0 `static` değilse, örnek ifadesi `this` ' dir. | 
|                   | `T.P`          | Sınıf veya yapı `T` ' deki `P` özelliğinin `get` erişimcisi çağrılır. @No__t-0 `static` değilse veya `P` salt yazılır ise, derleme zamanı hatası oluşur. | 
|                   | `T.P = value`  | Sınıf veya yapı `T` ' deki `P` özelliğinin `set` erişimcisi `(value)` bağımsız değişken listesiyle çağrılır. @No__t-0 `static` değilse veya `P` salt okunurdur, derleme zamanı hatası oluşur. | 
|                   | `e.P`          | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `P` özelliğinin `get` erişimcisi, `e` örnek ifadesiyle çağrılır. @No__t-0 `static` ise bağlama zamanı hatası oluşur veya `P` salt yazılır ise. | 
|                   | `e.P = value`  | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `P` özelliğinin `set` erişimcisi, `e` örnek ifadesiyle ve `(value)` bağımsız değişken listesiyle çağrılır. @No__t-0 `static` ise bağlama zamanı hatası oluşur veya `P` salt okunurdur. | 
| Olay erişimi      | `E += value`   | Kapsayan sınıf veya yapı içindeki `E` olayının `add` erişimcisi çağrıldı. @No__t-0 statik değilse, örnek ifadesi `this` ' dir. | 
|                   | `E -= value`   | Kapsayan sınıf veya yapı içindeki `E` olayının `remove` erişimcisi çağrıldı. @No__t-0 statik değilse, örnek ifadesi `this` ' dir. | 
|                   | `T.E += value` | Sınıf veya yapı `T` ' deki `E` olayının `add` erişimcisi çağrılır. @No__t-0 statik değilse bir bağlama zamanı hatası oluşur. | 
|                   | `T.E -= value` | Sınıf veya yapı `T` ' deki `E` olayının `remove` erişimcisi çağrılır. @No__t-0 statik değilse bir bağlama zamanı hatası oluşur. | 
|                   | `e.E += value` | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `E` olayının `add` erişimcisi, `e` örnek ifadesiyle çağrılır. @No__t-0 statikse bir bağlama zamanı hatası oluşur. | 
|                   | `e.E -= value` | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `E` olayının `remove` erişimcisi, `e` örnek ifadesiyle çağrılır. @No__t-0 statikse bir bağlama zamanı hatası oluşur. | 
| Dizin Oluşturucu erişimi    | `e[x,y]`       | Aşırı yükleme çözümlemesi, sınıf, yapı veya e-postayla verilen arabirimdeki en iyi Dizin oluşturucuyu seçmek için uygulanır. Dizin oluşturucunun `get` erişimcisi, `e` örnek ifadesiyle ve `(x,y)` bağımsız değişken listesinde çağrılır. Dizin Oluşturucu salt yazılır ise bağlama zamanı hatası oluşur. | 
|                   | `e[x,y] = value` | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki en iyi Dizin oluşturucuyu seçmek için aşırı yükleme çözümlemesi uygulanır. Dizin oluşturucunun `set` erişimcisi, `e` örnek ifadesiyle ve `(x,y,value)` bağımsız değişken listesinde çağrılır. Dizin Oluşturucu salt okunurdur bağlama zamanı hatası oluşur. | 
| İşleç çağırma | `-x`         | @No__t-0 türü tarafından verilen sınıfta veya yapıda en iyi birli işleci seçmek için aşırı yükleme çözümlemesi uygulanır. Seçilen işleç `(x)` bağımsız değişken listesiyle çağrılır. | 
|                     | `x + y`      | @No__t-0 ve `y` türleri tarafından verilen sınıflarda veya yapılarda en iyi ikili işleci seçmek için aşırı yükleme çözümlemesi uygulanır. Seçilen işleç `(x,y)` bağımsız değişken listesiyle çağrılır. | 
| Örnek Oluşturucu çağrısı | `new T(x,y)` | Sınıf veya yapı `T` ' da en iyi örnek oluşturucusunu seçmek için aşırı yükleme çözümlemesi uygulanır. Örnek Oluşturucu `(x,y)` bağımsız değişken listesiyle çağrılır. | 

### <a name="argument-lists"></a>Bağımsız değişken listeleri

Her işlev üyesi ve temsilci çağrısı, işlev üyesinin parametreleri için gerçek değerler veya değişken başvuruları sağlayan bir bağımsız değişken listesi içerir. İşlev üyesi çağrısının bağımsız değişken listesini belirtmek için sözdizimi, işlev üyesi kategorisine bağlıdır:

*  Örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler için bağımsız değişkenler, aşağıda açıklandığı gibi bir *argument_list*olarak belirtilir. Dizin oluşturucular için, `set` erişimcisini çağırırken bağımsız değişken listesi, atama işlecinin sağ işleneni olarak belirtilen ifadeyi de içerir.
*  Özellikler için, `get` erişimcisi çağrılırken bağımsız değişken listesi boştur ve `set` erişimcisi çağrılırken atama işlecinin sağ işleneni olarak belirtilen ifadeden oluşur.
*  Olaylar için bağımsız değişken listesi, `+=` veya `-=` işlecinin sağ işleneni olarak belirtilen ifadeden oluşur.
*  Kullanıcı tanımlı işleçler için bağımsız değişken listesi birli işlecin tek işleneninden veya ikili işlecinin iki işleneninden oluşur.

Özelliklerinin bağımsız değişkenleri ([Özellikler](classes.md#properties)), Olaylar ([Olaylar](classes.md#events)) ve Kullanıcı tanımlı işleçler ([işleçler](classes.md#operators)) her zaman değer parametreleri olarak geçirilir ([değer parametreleri](classes.md#value-parameters)). Dizin oluşturucularının bağımsız değişkenleri ([Dizin oluşturucular](classes.md#indexers)) her zaman değer parametreleri ([değer parametreleri](classes.md#value-parameters)) veya parametre dizileri ([parametre dizileri](classes.md#parameter-arrays)) olarak geçirilir. Başvuru ve çıkış parametreleri bu işlev üyesi kategorileri için desteklenmez.

Örnek oluşturucusunun, yöntemin, dizin oluşturucunun veya temsilci çağrısının bağımsız değişkenleri bir *argument_list*olarak belirtilir:

```antlr
argument_list
    : argument (',' argument)*
    ;

argument
    : argument_name? argument_value
    ;

argument_name
    : identifier ':'
    ;

argument_value
    : expression
    | 'ref' variable_reference
    | 'out' variable_reference
    ;
```

Bir *argument_list* , virgülle ayrılmış bir veya daha fazla *bağımsız değişkenden*oluşur. Her bağımsız değişken, bir *argument_value*tarafından izlenen isteğe bağlı bir *argument_name* oluşur. *Argument_name* içeren bir *bağımsız* değişken ***adlandırılmış bağımsız değişken***olarak adlandırılır, ancak *argument_name* olmayan bir *bağımsız* değişken ***konumsal bir bağımsız***değişkendir. Bir *argument_list*içindeki adlandırılmış bir bağımsız değişkenden sonra yer aldığı konum bağımsız değişkeninin görünmesi hatadır.

*Argument_value* aşağıdaki formlardan birini gerçekleştirebilir:

*  Bağımsız değişkenin değer parametresi olarak geçtiğini belirten bir *ifade*([değer parametreleri](classes.md#value-parameters)).
*  Bağımsız değişkenin bir başvuru parametresi ([başvuru parametreleri](classes.md#reference-parameters)) olarak geçtiğini belirten bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)) tarafından izlenen `ref` anahtar sözcüğü. Bir değişken başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı ([kesin atama](variables.md#definite-assignment)). Bağımsız değişkenin bir çıkış parametresi ([çıkış parametreleri](classes.md#output-parameters)) olarak geçtiğini belirten bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)) tarafından izlenen `out` anahtar sözcüğü. Değişken, değişkenin çıkış parametresi olarak geçirildiği bir işlev üyesi çağrısı sonrasında kesin olarak atanmış ([kesin atama](variables.md#definite-assignment)) olarak değerlendirilir.

#### <a name="corresponding-parameters"></a>Karşılık gelen parametreler

Bir bağımsız değişken listesindeki her bağımsız değişken için, çağrılan işlev üyesinde veya temsilde karşılık gelen bir parametre olması gerekmez.

Aşağıda kullanılan parametre listesi aşağıdaki şekilde belirlenir:

*  Sınıflarda tanımlı sanal yöntemler ve Dizin oluşturucular için, parametre listesi, alıcı türü ve temel sınıfları arasında arama yaparak işlev üyesinin en belirgin bildiriminden veya geçersiz kılınmadan çekilir.
*  Arabirim yöntemleri ve Dizin oluşturucular için, parametre listesi, arabirim türü ile başlayıp temel arabirimler üzerinden arama yaparak üyenin en belirli tanımını oluşturacak şekilde çekilir. Benzersiz bir parametre listesi bulunamazsa, erişilemeyen adlara sahip bir parametre listesi ve isteğe bağlı parametre oluşturulur; bu nedenle, etkinleştirmeleri adlandırılmış parametreleri kullanamaz veya isteğe bağlı bağımsız değişkenleri alamaz.
*  Kısmi yöntemler için, tanımlayan kısmi Yöntem bildiriminin parametre listesi kullanılır.
*  Diğer tüm işlev üyeleri ve temsilciler için yalnızca tek bir parametre listesi bulunur ve bu, kullanılır.

Bir bağımsız değişkenin veya parametrenin konumu, bağımsız değişken listesi veya parametre listesinde kendisinden önceki bağımsız değişken veya parametre sayısı olarak tanımlanır.

İşlev üyesi bağımsız değişkenleri için karşılık gelen parametreler şu şekilde oluşturulur:

*  Örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilcilerin *argument_list* bağımsız değişkenleri:
    * Parametre listesindeki aynı konumda sabit bir parametrenin gerçekleştiği konumsal bir bağımsız değişken bu parametreye karşılık gelir.
    * Normal biçiminde çağrılan bir parametre dizisi olan bir işlev üyesinin Konumsal bağımsız değişkeni, parametre listesinde aynı konumda olması gereken parametre dizisine karşılık gelir.
    * Bir parametre dizisi olan bir işlev üyesinin, parametre listesinde aynı konumda hiçbir sabit parametre gerçekleşmediği, parametre dizisindeki bir öğeye karşılık gelen bir konum bağımsız değişkeni.
    * Adlandırılmış bir bağımsız değişken, parametre listesinde aynı ada sahip parametreye karşılık gelir.
    * Dizin oluşturucular için, `set` erişimcisi çağrılırken, atama işlecinin sağ işleneni olarak belirtilen ifade, `set` erişimci bildiriminin örtük `value` parametresine karşılık gelir.
*  @No__t-0 erişimcisi çağrılırken özellikler için bağımsız değişken yoktur. @No__t-0 erişimcisi çağrılırken, atama işlecinin sağ işleneni olarak belirtilen ifade, `set` erişimci bildiriminin örtük `value` parametresine karşılık gelir.
*  Kullanıcı tanımlı Birli İşleçler (dönüşümler dahil) için, tek işlenen işleç bildiriminin tek parametresine karşılık gelir.
*  Kullanıcı tanımlı ikili işleçler için, sol işlenen ilk parametreye karşılık gelir ve sağ işlenen işleç bildiriminin ikinci parametresine karşılık gelir.

#### <a name="run-time-evaluation-of-argument-lists"></a>Bağımsız değişken listelerinin çalışma süresi değerlendirmesi

Bir işlev üye çağrısının çalışma zamanı işlemesi sırasında ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), bir bağımsız değişken listesinin ifadeleri ya da değişken başvuruları, soldan sağa aşağıdaki gibi sırayla değerlendirilir:

*  Bir değer parametresi için bağımsız değişken ifadesi değerlendirilir ve ilgili parametre türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir. Sonuç değeri, işlev üye çağrısında değer parametresinin başlangıç değeri olur.
*  Bir başvuru veya çıkış parametresi için, değişken başvurusu değerlendirilir ve sonuçta elde edilen depolama konumu, işlev üyesi çağrısında parametresi tarafından temsil edilen depolama konumu olur. Başvuru veya çıkış parametresi olarak verilen değişken başvurusu bir *reference_type*dizi öğesi ise, dizinin öğe türünün parametrenin türüyle aynı olduğundan emin olmak için bir çalışma zamanı denetimi yapılır. Bu denetim başarısız olursa, bir `System.ArrayTypeMismatchException` oluşturulur.

Yöntemler, Dizin oluşturucular ve örnek oluşturucular, en sağdaki parametreleri parametre dizisi ([parametre dizileri](classes.md#parameter-arrays)) olarak bildirebilir. Bu tür işlev üyeleri, uygulanabilir olduğu ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) bağlı olarak kendi normal biçiminde veya genişletilmiş biçiminde çağırılır:

*  Parametre dizisi olan bir işlev üyesi normal biçimde çağrıldığında, parametre dizisi için verilen bağımsız değişken parametre dizi türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tek bir ifade olmalıdır. Bu durumda, parametre dizisi tam olarak bir değer parametresi gibi davranır.
*  Bir parametre dizisi olan bir işlev üyesi genişletilmiş biçimde çağrıldığında, çağırma parametre dizisi için sıfır veya daha fazla Konumsal bağımsız değişkeni belirtmelidir; burada her bağımsız değişken örtük olarak dönüştürülebilir bir ifadedir ([örtük dönüştürmeler ](conversions.md#implicit-conversions)) parametre dizisinin öğe türü. Bu durumda, çağırma parametre dizisi türünde bağımsız değişken sayısına karşılık gelen bir bir örnek oluşturur, dizi örneğinin öğelerini verilen bağımsız değişken değerleriyle başlatır ve gerçek olarak yeni oluşturulan dizi örneğini kullanır değişkendir.

Bir bağımsız değişken listesinin ifadeleri her zaman yazıldığı sırada değerlendirilir. Bu nedenle örnek
```csharp
class Test
{
    static void F(int x, int y = -1, int z = -2) {
        System.Console.WriteLine("x = {0}, y = {1}, z = {2}", x, y, z);
    }

    static void Main() {
        int i = 0;
        F(i++, i++, i++);
        F(z: i++, x: i++);
    }
}
```
çıktıyı üretir
```console
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Dizi birlikte değişim kuralları ([dizi Kovaryans](arrays.md#array-covariance)), `A[]` dizi türünde bir değere izin verir. bu, `B` ' ten `A` ' e kadar örtük bir başvuru dönüştürmesi sağlanmış `B[]` dizi türünün bir örneğine başvuru sağlar. Bu kurallar nedeniyle, bir *reference_type* dizi öğesi başvuru veya çıkış parametresi olarak geçirildiğinde, dizinin gerçek öğe türünün parametreyle aynı olduğundan emin olmak için bir çalışma zamanı denetimi gereklidir. Örnekte
```csharp
class Test
{
    static void F(ref object x) {...}

    static void Main() {
        object[] a = new object[10];
        object[] b = new string[10];
        F(ref a[0]);        // Ok
        F(ref b[1]);        // ArrayTypeMismatchException
    }
}
```
`F` ' ın ikinci çağrılması, `b` ' nin gerçek öğe türü `string` ve `object` olmadığından `System.ArrayTypeMismatchException` oluşturulmasına neden olur.

Bir parametre dizisi olan bir işlev üyesi genişletilmiş biçimde çağrıldığında, genişletilmiş parametrelere bir dizi Başlatıcısı ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)) içeren bir dizi oluşturma ifadesi eklenirse, çağırma tam olarak işlenir. Örneğin, bildirim verildiğinde
```csharp
void F(int x, int y, params object[] args);
```
yöntemin genişletilmiş biçimini aşağıdaki şekilde çağırma
```csharp
F(10, 20);
F(10, 20, 30, 40);
F(10, 20, 1, "hello", 3.0);
```
tam olarak karşılık gelir
```csharp
F(10, 20, new object[] {});
F(10, 20, new object[] {30, 40});
F(10, 20, new object[] {1, "hello", 3.0});
```

Özellikle, parametre dizisi için belirtilen 0 bağımsız değişken olduğunda boş bir dizi oluşturulduğunu unutmayın.

Bağımsız değişkenler, karşılık gelen isteğe bağlı parametrelere sahip bir işlev üyesinden atlandığında, işlev üyesi bildiriminin varsayılan bağımsız değişkenleri dolaylı olarak geçirilir. Bunlar her zaman sabit olduğundan, değerlendirmesi kalan bağımsız değişkenlerin değerlendirme sırasını etkilemez.

### <a name="type-inference"></a>Tür çıkarımı

Genel bir yöntem tür bağımsız değişkenleri belirtilmeden çağrıldığında, bir ***tür çıkarım*** işlemi çağrının tür bağımsız değişkenlerini çıkarmayı dener. Tür çıkarımı varlığı, genel bir yöntemi çağırmak için daha uygun bir sözdiziminin kullanılmasına izin verir ve programcı 'nın gereksiz tür bilgilerini belirtmelerini önler. Örneğin, yöntem bildirimi verildiğinde:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
açıkça bir tür bağımsız değişkeni belirtmeden `Choose` yöntemini çağırmak mümkündür:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Tür çıkarımı aracılığıyla `int` ve `string` tür bağımsız değişkenleri, bağımsız değişkenlerden yöntemine göre belirlenir.

Tür çıkarımı, bir yöntem çağırma işleminin ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) bağlama zamanı işlemenin bir parçası olarak oluşur ve çağrının aşırı yükleme çözümleme adımından önce gerçekleşir. Bir yöntem çağrısında belirli bir yöntem grubu belirtildiğinde ve yöntem çağrısının bir parçası olarak hiçbir tür bağımsız değişkeni belirtilmediğinde, yöntem grubundaki her genel yönteme tür çıkarımı uygulanır. Tür çıkarımı başarılı olursa, sonraki aşırı yükleme çözümlemesi için bağımsız değişken türlerini belirlemede çıkartılan tür bağımsız değişkenleri kullanılır. Aşırı yükleme çözümlemesi, çağırabileceğiniz bir genel yöntem seçerse, çağırma için gerçek tür bağımsız değişkenleri olarak, çıkartılan tür bağımsız değişkenleri kullanılır. Belirli bir yöntemin tür çıkarımı başarısız olursa, bu yöntem aşırı yükleme çözümüne katılmaz. , Ve içindeki çıkarım hatası, bağlama zamanı hatasına neden olmaz. Ancak, aşırı yükleme çözümlemesi sırasında bir bağlama zamanı hatasına neden olur ve ilgili herhangi bir yöntemi bulamaz.

Sağlanan bağımsız değişken sayısı yöntemdeki parametre sayısından farklıysa, çıkarımı hemen başarısız olur. Aksi takdirde, genel yöntemin aşağıdaki imzaya sahip olduğunu varsayalım:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Formun yöntem çağrısıyla `M(E1...Em)` tür çıkarımı görevi, `M<S1...Sn>(E1...Em)` ' ün geçerli hale gelmesi için `X1...Xn` tür parametrelerinin her biri için-1 @no__t benzersiz tür bağımsız değişkenlerini bulmalıdır.

Çıkarımı işlemi sırasında her tür parametresi `Xi`, belirli bir tür @no__t- *2 veya ilişkili* bir *sınır*kümesiyle *sabitlenmiştir* . Her bir sınır `T` ' dır. Başlangıçta her tür değişkeni `Xi` boş bir sınır kümesiyle sabitlenemez.

Tür çıkarımı aşamalar halinde gerçekleşir. Her aşama, önceki aşamanın bulgularını temel alarak daha fazla tür değişkeni için tür bağımsız değişkenlerini çıkarması denenecek. İlk aşama bazı ilk dereceden sınırları yapar, ancak ikinci aşama belirli türlere tür değişkenlerini ve daha fazla sınırları düzeltir. İkinci aşamanın birkaç kez tekrarlanmanız gerekebilir.

*Not:* Tür çıkarımı yalnızca genel bir yöntem çağrıldığında değil. Yöntem gruplarının dönüştürülmesine ilişkin tür çıkarımı, [Yöntem gruplarının dönüştürülmesi için tür çıkarımı](expressions.md#type-inference-for-conversion-of-method-groups) ve bir ifade kümesinin en iyi ortak [türünü bulma bölümünde açıklanmıştır.](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)

#### <a name="the-first-phase"></a>İlk aşama

Yöntem bağımsız değişkenlerinin her biri için `Ei`:

*   @No__t-0 Anonim bir işlevdir, bir *Açık parametre türü çıkarımı* ([Açık parametre türü](expressions.md#explicit-parameter-type-inferences)), `Ei` ' ten `Ti` ' e yapılır
*   Aksi takdirde, `Ei` ' a `U` türü varsa `xi` değeri bir değer parametresidir ve `U` ' *ten* `Ti` ' *ye* *daha düşük bir çıkarım* yapılır.
*   Aksi takdirde, `Ei` ' a `U` türü varsa `xi` `ref` veya `out` parametresi ise `U` ' *den* @no__t- *9 ' a* doğru bir *çıkarım* yapılır.
*   Aksi takdirde, bu bağımsız değişken için çıkarımı yapılmaz.


#### <a name="the-second-phase"></a>İkinci aşama

İkinci aşama aşağıdaki gibi ilerler:

*   @No__t-1 ' i ([bağımlıce](expressions.md#dependence)) *bağımlı* olmayan tüm *sabit* olmayan tür değişkenleri `Xj` sabittir ([düzeltiliyor](expressions.md#fixing)).
*   Böyle bir tür değişkeni yoksa, `Xi` olmayan tüm *sabit* tür değişkenleri aşağıdaki her bir bekletme için *sabittir* :
    *   En az bir tür değişkeni vardır `Xj` ' a bağlı `Xi`
    *   `Xi` ' da boş olmayan bir sınır kümesi vardır
*   Böyle bir tür değişkeni yoksa ve hala *sabit* olmayan tür değişkenleri varsa, tür çıkarımı başarısız olur.
*   Aksi halde, daha fazla *sabit olmayan* tür değişkeni yoksa, tür çıkarımı başarılı olur.
*   Aksi takdirde, karşılık gelen parametre türü `Ei` olan `Ti` olan tüm bağımsız değişkenler için, *Çıkış türlerinin* ([çıktı türleri](expressions.md#output-types)) *sabit* olmayan tür değişkenleri içerdiği `Xj` ' i, ancak *giriş türleri* ([giriş türleri](expressions.md#input-types)) değil,  *çıkış türü çıkarımı* ([Çıkış türü ını](expressions.md#output-type-inferences)) 1 ' *den* 3 ' *e* yapılır. İkinci aşama yinelenir.

#### <a name="input-types"></a>Giriş türleri

@No__t-0 bir yöntem grubu veya örtük olarak yazılmış anonim işlevse ve `T` bir temsilci türü ya da ifade ağacı türü ise, `T` türündeki tüm parametre türleri `T` *türünde* `E` ' ün *giriş türleridir* .

####  <a name="output-types"></a>Çıkış türleri

@No__t-0 bir yöntem grubu veya anonim bir işlevdir ve `T` bir temsilci türü ya da ifade ağacı türü ise, `T` ' nin dönüş türü `T` *türündeki* `E` *Çıkış türüdür* .

#### <a name="dependence"></a>Düzeyde bağımlı

@No__t-1 *sabit* olmayan bir tür değişkeni *doğrudan* sabit olmayan bir tür değişkenine bağlıdır `Xj` `Tk` türünde `Ek` `Xj` *türünde bir @no__t* -6 ve *`Tk` türü* `Xi`3 türündeki 2 çıkış türü.

`Xj` *'* a bağlı `Xj` doğrudan `Xi` *'* i kullanıyorsa ya da `Xi` `Xk` ' i @no__t ve `Xk` ' *a bağlıdır @no__t* -11 ' *ye bağlıdır.* Bu nedenle "bağlı olan", geçişli ancak yansımalı olmayan "açık" kapanışı değildir.

#### <a name="output-type-inferences"></a>Çıkış türü ında

Bir *Çıkış türü çıkarımı* @no__t *-2 '* *den* aşağıdaki şekilde `T` türüne yapılır:

*  @No__t-0, `U` ([çıkartılan dönüş türü](expressions.md#inferred-return-type)) dönüş türü olan anonim bir işlevdir ve `T`, dönüş türü `Tb` olan bir temsilci türü ya da ifade ağacı türü, daha sonra *alt sınır çıkarımı* ([alt sınır](expressions.md#lower-bound-inferences)) @no__t *-8 '* *den* 0 ' a yapılır.
*  Aksi takdirde, `E` bir yöntem grubudur ve `T`, parametre türleri `T1...Tk` ve dönüş türü `Tb` olan bir temsilci türü ya da ifade ağacı türü ve `E` ' ü `T1...Tk` ' i içeren tek bir yöntem döndürür `U` daha sonra, `U` ' *dan* 1 ' *e* *daha düşük bir çıkarım* yapılır.
*  Aksi takdirde, `E` türü `U` olan bir ifade ise, `U` ' *ten* *@no__t-* 6 ' a *daha düşük bir çıkarım* yapılır.
*  Aksi takdirde, hiçbir Inse yapılmaz.

#### <a name="explicit-parameter-type-inferences"></a>Açık parametre türü

@No__t *-2 '* *den* aşağıdaki şekilde `T` türüne *açık bir parametre türü çıkarımı* yapılır:

*  @No__t-0, parametre türleri `U1...Uk` ve `T` olan bir temsilci türü ya da ifade ağacı @no__t türü, her @no__t için her-4 ' *ü (* [tam](expressions.md#exact-inferences)değer)@no__t *-8 '* den karşılık gelen 0 ' a.

#### <a name="exact-inferences"></a>Tam ında

@No__t-2 `V` türüne *doğru bir çıkarım* *aşağıdaki şekilde yapılır* :

*  @No__t-0 ' ı *fixed* `Xi` ' den biri ise, `Xi` ' e ait tam sınırların kümesine `U` eklenir.

*  Aksi takdirde, `V1...Vk` ve `U1...Uk` ayarları aşağıdaki durumlardan herhangi birinin uygulanabilir olup olmadığını denetleyerek belirlenir:

   *  `V`,-1 @no__t bir dizi türüdür ve `U1[...]` aynı sırada  ' tür.
   *  `V` `V1?` türü ve `U` tür `U1?`
   *  `V` oluşturulmuş bir tür `C<V1...Vk>`ve `U` oluşturulmuş bir tür `C<U1...Uk>`

   Bu durumlardan herhangi biri uygunsa, her `Ui` ' *den* karşılık gelen `Vi` ' *e* *tam bir çıkarım* yapılır.

*  Aksi takdirde hiçbir Inse yapılmaz.

#### <a name="lower-bound-inferences"></a>Alt sınır

@No__t *-2 @no__t* -4 türüne *alt sınır çıkarımı* aşağıdaki gibi yapılır:

*  @No__t-0 *sabit* `Xi` ' den biri ise, `Xi` ' e ait alt sınırlar kümesine `U` eklenir.
*  Aksi takdirde, `V` `V1?`türüdür ve `U` tür `U1?` ise `U1` ' ten `V1` ' e daha düşük bir sınır çıkarımı yapılır.
*  Aksi takdirde, `U1...Uk` ve `V1...Vk` ayarları aşağıdaki durumlardan herhangi birinin uygulanabilir olup olmadığını denetleyerek belirlenir:
   *  `V` ' a bir dizi türü `V1[...]` ve `U` bir dizi @no__t türüdür (veya geçerli temel türü `U1[...]` olan bir tür parametresi) aynı derecenin
   *  `IEnumerable<V1>`  `ICollection<V1>` veya `IList<V1>` ve `U` bir tek boyutlu dizi türü `U1[]` ' dir (veya geçerli temel türü `U1[]` olan bir tür parametresi)
   *  `V`, oluşturulmuş bir sınıf, yapı, arabirim veya temsilci türü `C<V1...Vk>` ' dir @no__t ve `U` (veya `U` bir tür parametresi ise, etkin taban sınıfı veya etkin arabirim kümesinin herhangi bir üyesi) ile aynıdır. , öğesinden devralır (doğrudan veya dolaylı olarak) veya (doğrudan veya dolaylı olarak) `C<U1...Uk>` uygular.

      ("Benzersizlik" kısıtlaması, durum arabirimindeki `C<T> {} class U: C<X>, C<Y> {}` olduğunda, `U1` `X` veya `Y` olabileceği için, `U` ' den `C<T>` ' ye kadar bir çıkarımı yapılmadığını gösterir.)

   Bu durumlardan herhangi biri uygunsa, her `Ui` ' *den* karşılık gelen `Vi` ' *e* aşağıdaki şekilde bir çıkarımı yapılır:

   *  @No__t-0 ' ı bir başvuru türü olarak bilinmiyorsa, *kesin bir çıkarım* yapılır
   *  Aksi takdirde, `U` bir dizi türüdür, daha sonra *alt sınır çıkarımı* yapılır
   *  Aksi takdirde, `V` `C<V1...Vk>` ise, çıkarım, `C` ' nin ı-TH türü parametresine bağlıdır:
      *  Daha fazla değişken ise, *alt sınır çıkarımı* yapılır.
      *  Değişken karşıtı ise, *üst sınır çıkarımı* yapılır.
      *  Sabit ise, *kesin bir çıkarım* yapılır.
*  Aksi takdirde, hiçbir Inse yapılmaz.

#### <a name="upper-bound-inferences"></a>Üst sınır

@No__t-2 `V` *türünden bir* *üst sınır çıkarımı* *Şu şekilde yapılır* :

*  @No__t-0 ' ı *fixed* `Xi` ' den biri ise, `U` @no__t üst sınırlar kümesine eklenir.
*  Aksi takdirde, `V1...Vk` ve `U1...Uk` ayarları aşağıdaki durumlardan herhangi birinin uygulanabilir olup olmadığını denetleyerek belirlenir:
   *  `U`,-1 @no__t bir dizi türüdür ve `V1[...]` aynı sırada  ' tür.
   *  `U` `IEnumerable<Ue>` `ICollection<Ue>` veya `IList<Ue>` ve `V` tek boyutlu dizi türüdür `Ve[]`
   *  `U` `U1?` türü ve `V` tür `V1?`
   *  `U` oluşturulan sınıf, yapı, arabirim veya temsilci türü `C<U1...Uk>` ve `V` bir sınıf, yapı, arabirim veya temsilci türü, öğesinden devralır (doğrudan veya dolaylı) ya da (doğrudan veya dolaylı olarak) benzersiz bir tür uygular `C<V1...Vk>`

      ("Benzersizlik" kısıtlaması, `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}` olduğunda, `C<U1>` ' den `V<Q>` ' ye kadar bir çıkarımı yapılmadığını gösterir. Inıd `U1` ' dan `X<Q>` veya `Y<Q>` ' ye kadar değil.)

   Bu durumlardan herhangi biri uygunsa, her `Ui` ' *den* karşılık gelen `Vi` ' *e* aşağıdaki şekilde bir çıkarımı yapılır:
   *  @No__t-0 ' ı bir başvuru türü olarak bilinmiyorsa, *kesin bir çıkarım* yapılır
   *  Aksi takdirde, `V` bir dizi türü ise, *üst sınır çıkarımı* yapılır
   *  Aksi takdirde, `U` `C<U1...Uk>` ise, çıkarım, `C` ' nin ı-TH türü parametresine bağlıdır:
      *  Covaryant ise, *üst sınır çıkarımı* yapılır.
      *  Değişken karşıtı ise, *alt sınır çıkarımı* yapılır.
      *  Sabit ise, *kesin bir çıkarım* yapılır.
*  Aksi takdirde, hiçbir Inse yapılmaz.   

#### <a name="fixing"></a>Düzelttikten

Bir dizi sınır içeren @no__t *sabit* olmayan bir tür değişkeni aşağıdaki gibi *düzeltilir* :

*  @No__t-1 *aday türleri* kümesi, `Xi` için sınırlar kümesindeki tüm türlerin kümesi olarak başlatılır.
*  Sonra her bir `Xi` ' ı sırayla inceliyoruz: Her bir tam @no__t için-1 ' in 0 ' a `Xi` ' i `U` ' @no__t e ait olmayan tüm türler aday kümesinden kaldırılır. Her bir alt sınır için `U` ' @no__t dan `Xi` ' den örtük bir dönüştürme *olmadığı* `Uj` ' den tüm türler aday kümesinden kaldırılır. Her üst sınır için `U` ' dan `Xi` ' i n `U` ' e örtük dönüştürme *olmayan* tüm @no__t türler aday kümesinden kaldırılır.
*  Kalan aday türleri arasında `Uj`, diğer aday türlerine örtük bir dönüştürme olan `V` benzersiz bir tür vardır ve bu durumda `Xi` `V` ' e sabitlenmiştir.
*  Aksi takdirde tür çıkarımı başarısız olur.

#### <a name="inferred-return-type"></a>Çıkarılan dönüş türü

@No__t-0 Anonim işlevinin çıkarılan dönüş türü, tür çıkarımı ve aşırı yükleme çözümlemesi sırasında kullanılır. Çıkarılan dönüş türü yalnızca, açıkça verildiği veya bir kapsayan genel üzerinde tür çıkarımı sırasında çıkarsandığı için, tüm parametre türlerinin bilindikleri anonim bir işlev için belirlenebilir Yöntem çağırma.

***Çıkarılan sonuç türü*** aşağıdaki şekilde belirlenir:

*  @No__t-0 gövdesi türü olan bir *ifadesiyse* , `F` ' nin çıkartılan sonuç türü bu ifadenin türüdür.
*  @No__t-0 ' ın gövdesi bir *blok* ise ve bloğun `return` deyimlerindeki ifadelerin kümesi en iyi ortak tür olan `T` ' e sahiptir ([bir ifade kümesinin En Iyi ortak türünü bulmayla](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), `F` ' in ortaya çıkarılan sonuç türü `T` ' dır.
*  Aksi takdirde, `F` için sonuç türü çıkarsanamıyor.

***Çıkarılan dönüş türü*** aşağıdaki şekilde belirlenir:

*  @No__t-0 zaman uyumsuz ise ve `F` gövdesi Nothing ([ifade sınıflandırmaları](expressions.md#expression-classifications)) olarak sınıflandırılmış bir ifade veya hiçbir dönüş deyiminin ifadesi olmadığı bir deyim bloğu ise, çıkarılan dönüş türü `System.Threading.Tasks.Task` olur
*  @No__t-0 zaman uyumsuz ise ve ortaya çıkarılan sonuç türü `T` ise, çıkarılan dönüş türü `System.Threading.Tasks.Task<T>` ' dir.
*  @No__t-0, zaman uyumsuz ise ve ortaya çıkarılan bir sonuç türü `T` ise, çıkarılan dönüş türü `T` ' dir.
*  Aksi takdirde `F` için bir dönüş türü çıkarsanamıyor.

Anonim işlevler içeren tür çıkarımı örneği olarak, `System.Linq.Enumerable` sınıfında belirtilen `Select` genişletme yöntemini göz önünde bulundurun:
```csharp
namespace System.Linq
{
    public static class Enumerable
    {
        public static IEnumerable<TResult> Select<TSource,TResult>(
            this IEnumerable<TSource> source,
            Func<TSource,TResult> selector)
        {
            foreach (TSource element in source) yield return selector(element);
        }
    }
}
```

@No__t-0 ad alanının bir `using` yan tümcesiyle içeri aktarıldığını ve `string` türünde `Name` özelliğine sahip `Customer` sınıfı verildiğini varsayarsak, `Select` yöntemi bir müşteri listesinin adlarını seçmek için kullanılabilir:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

@No__t-1 ' in uzantı yöntemi çağırma ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)), statik bir yöntem çağrısına yapılan çağrıyı yeniden yazarak işlenir:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Tür bağımsız değişkenleri açıkça belirtilmediği için tür çıkarımı tür bağımsız değişkenlerini çıkarsanacak şekilde kullanılır. İlk olarak, `customers` bağımsız değişkeni `source` parametresiyle ilgilidir, bu `T` `Customer` olur. Daha sonra, yukarıda açıklanan anonim işlev türü çıkarımı işlemini kullanarak, `c` `Customer` türü ve `c.Name` ifadesi `selector` parametresinin dönüş türüyle ilişkilidir, `S` ' ün `string` olmasını sağlar. Bu nedenle, çağırma şu şekilde eşdeğerdir
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
Sonuç `IEnumerable<string>` türündedir.

Aşağıdaki örnek, anonim işlev türü çıkarımını genel yöntem çağrısında bağımsız değişkenler arasında "Flow" öğesine nasıl izin verdiğini gösterir. Yöntem verildi:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Çağırma için tür çıkarımı:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
Şu şekilde devam eder: İlk olarak, `"1:15:30"` bağımsız değişkeni `value` parametresiyle ilgilidir, bu `X` `string` olur. Ardından, `s` olan ilk adsız işlevin parametresine `string` Çıkarsanan tür `TimeSpan.Parse(s)` ifadesi `f1` ' ün dönüş türüyle ilişkilidir, `Y` ' ü `System.TimeSpan` olarak yeniden dener. Son olarak, `t` olan ikinci adsız işlevin parametresine `System.TimeSpan` Çıkarsanan tür `t.TotalSeconds` ifadesi `f2` ' ün dönüş türü ile ilgilidir ve `Z` ' ün `double` olmasını sağlar. Bu nedenle, çağrının sonucu `double` türündedir.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Yöntem gruplarının dönüştürülmesi için tür çıkarımı

Genel yöntemlerin çağrılarına benzer şekilde, genel bir yöntem içeren `M` Yöntem grubu, `D` ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) verilen temsilci türüne dönüştürüldüğünde tür çıkarımı da uygulanmalıdır. Verilen Yöntem
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
`M` Yöntem grubu, temsilci türüne atandı `D` tür çıkarımı, ifadenin tür bağımsız değişkenlerini bulmakta `S1...Sn` ifadesi.
```csharp
M<S1...Sn>
```
`D` ile uyumlu ([temsilci bildirimleri](delegates.md#delegate-declarations)) olur.

Genel yöntem çağrılarının tür çıkarımı algoritmasından farklı olarak, bu durumda yalnızca bağımsız değişken *türleri*vardır, hiçbir bağımsız değişken *ifadesi*yoktur. Özellikle, anonim işlev yoktur ve bu nedenle birden çok çıkarım aşaması gerekmez.

Bunun yerine, tüm `Xi` ' ı *sabitlenmemiş*olarak kabul edilir ve `Uj` ' *ten* `D` ' e ait her bağımsız değişken türünden `M` `Tj` ' *ye* karşılık gelen parametre türüne *alt sınır çıkarımı* yapılır. @No__t varsa-0 sınır bulunmazsa, tür çıkarımı başarısız olur. Aksi halde, her `Xi`, karşılık gelen `Si` ' ye *sabitlenir* ve bu tür çıkarımı oluşur.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Bir ifade kümesinin en iyi ortak türünü bulma

Bazı durumlarda, bir dizi ifade için ortak bir türün çıkarsanması gerekir. Özellikle, örtük olarak yazılan dizilerin öğe türleri ve *blok* gövdeleri olan anonim işlevlerin dönüş türleri bu şekilde bulunur.

Intuicanlı, bir dizi ifade verildiğinde `E1...Em` bu çıkarımı, bir yöntemi çağırmak için eşdeğer olmalıdır
```csharp
Tr M<X>(X x1 ... X xm)
```
`Ei` ile bağımsız değişken olarak.

Daha kesin olarak, çıkarım `X` *sabit* olmayan bir tür değişkeniyle başlar. Daha sonra her `Ei` ' *den* `X` ' *e* kadar *Çıkış türü* . Son olarak, `X` *sabittir* ve başarılıysa, sonuçta elde edilen tür `S`, ifadeler için en iyi ortak türdür. Böyle bir `S` yoksa, ifadelerde en iyi ortak tür yoktur.

### <a name="overload-resolution"></a>Aşırı yükleme çözümü

Aşırı yükleme çözümlemesi, verilen bağımsız değişken listesini ve aday işlev üyelerini çağırmak için en iyi işlev üyesini seçmeye yönelik bir bağlama zamanı mekanizmasıdır. Aşırı yükleme çözümlemesi, içindeki C#aşağıdaki farklı bağlamlarda çağrılacak işlev üyesini seçer:

*  Bir *invocation_expression* ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) içinde adlı bir yöntemin çağrılması.
*  Bir *object_creation_expression* ([nesne oluşturma ifadelerinde](expressions.md#object-creation-expressions)) adlı örnek oluşturucunun çağrılması.
*  Bir *element_access* ([öğe erişimi](expressions.md#element-access)) aracılığıyla bir Dizin Oluşturucu erişimcisinin çağrılması.
*  İfadede önceden tanımlanmış veya Kullanıcı tanımlı bir işlecin çağrılması ([birli operatör aşırı yükleme çözümü](expressions.md#unary-operator-overload-resolution) ve [ikili işleç aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)).

Bu bağlamların her biri, yukarıda listelenen bölümlerde ayrıntılı olarak açıklandığı gibi aday işlev üyeleri kümesini ve bağımsız değişkenlerin listesini kendi benzersiz şeklinde tanımlar. Örneğin, bir yöntem çağrısı için aday kümesi, `override` ([üye arama](expressions.md#member-lookup)) olarak işaretlenmiş yöntemleri içermez ve türetilmiş bir sınıftaki herhangi bir yöntem geçerliyse ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) temel sınıftaki Yöntemler aday değildir.

Aday işlev üyeleri ve bağımsız değişken listesi tanımlandıktan sonra, en iyi işlev üyesinin seçimi her durumda aynıdır:

*  Uygulanabilir aday işlev üyeleri kümesi verildiğinde, bu küme içindeki en iyi işlev üyesi bulunur. Küme yalnızca bir işlev üyesi içeriyorsa, bu işlev üyesi en iyi işlev üyesidir. Aksi halde, en iyi işlev üyesi, her bir işlev üyesinin [daha iyi işlevdeki kuralları kullanarak diğer tüm işlev üyeleriyle karşılaştırıldığı belirtilen bağımsız değişken listesine göre diğer tüm işlev üyelerinden daha iyi olan bir işlev üyesidir. üye](expressions.md#better-function-member). Tüm diğer işlev üyelerinden daha iyi olan tam olarak bir işlev üyesi yoksa, işlev üye çağrısı belirsizdir ve bir bağlama zamanı hatası oluşur.

Aşağıdaki bölümlerde, koşullara ***uygun işlev üyesinin*** ve ***daha iyi işlev üyesinin***tam anlamları tanımlanacaktır.

#### <a name="applicable-function-member"></a>Geçerli işlev üyesi

Bir işlev üyesi, aşağıdakilerin tümü doğru olduğunda bir bağımsız değişken listesi `A` olan ***geçerli bir işlev üyesi*** olarak kabul edilir:

*  @No__t-0 ' daki her bağımsız değişken, [karşılık gelen parametrelerde](expressions.md#corresponding-parameters)açıklandığı gibi işlev üye bildirimindeki parametreye karşılık gelir ve hiçbir bağımsız değişkenin karşılık geldiği parametre isteğe bağlı bir parametredir.
*  @No__t-0 ' daki her bir bağımsız değişken için, bağımsız değişkenin (örn. değer, `ref` veya `out`) parametre geçirme modu, karşılık gelen parametrenin parametre geçirme moduyla aynıdır ve
   *  bir değer parametresi veya bir parametre dizisi için, bağımsız değişkenden karşılık gelen parametrenin türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) var veya
   *  `ref` veya `out` parametresi için bağımsız değişkenin türü karşılık gelen parametrenin türüyle aynıdır. Tümü, `ref` veya `out` parametresi geçilen bağımsız değişken için bir diğer addır.

Bir parametre dizisi içeren bir işlev üyesi için, işlev üyesi Yukarıdaki kurallar tarafından geçerliyse, bunun ***normal biçiminde***geçerli olduğu söylenir. Bir parametre dizisi içeren bir işlev üyesi normal biçiminde geçerli değilse, işlev üyesi ***genişletilmiş biçimde***uygulanabilir olabilir:

*  Genişletilmiş form, işlev üyesi bildirimindeki parametre dizisi parametre dizisinin öğe türünün sıfır veya daha fazla değer parametresiyle değiştirilerek oluşturulur, çünkü bağımsız değişken listesindeki bağımsız değişkenlerin sayısı `A`, toplam sayısıyla eşleşir parametreleri. @No__t-0 ' ın işlev üyesi bildirimindeki sabit parametrelerin sayısından daha az bağımsız değişkeni varsa, işlev üyesinin genişletilmiş biçimi oluşturulamıyor ve bu nedenle geçerli değildir.
*  Aksi halde, `A` ' daki her bağımsız değişken için genişletilmiş form geçerli olur-0 parametre geçirme modu, karşılık gelen parametrenin parametre geçirme moduyla aynı ve
   *  sabit bir değer parametresi veya genişletme tarafından oluşturulan bir değer parametresi için, bağımsız değişkenin türünden, karşılık gelen parametrenin türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) vardır veya
   *  `ref` veya `out` parametresi için bağımsız değişkenin türü karşılık gelen parametrenin türüyle aynıdır.

#### <a name="better-function-member"></a>Daha iyi işlev üyesi

Daha iyi işlev üyesini belirleme amaçları doğrultusunda, yalnızca bağımsız değişken ifadelerinin orijinal bağımsız değişken listesinde göründükleri sırada kendilerini içeren bir aşağı açılan bağımsız değişken listesi oluşturulur.

Aday işlev üyelerinin her biri için parametre listeleri aşağıdaki şekilde oluşturulur:

*  İşlev üyesi yalnızca genişletilmiş biçimde uygulanabiliyorsa genişletilmiş form kullanılır.
*  Karşılık gelen bağımsız değişken içermeyen isteğe bağlı parametreler parametre listesinden kaldırılır
*  Parametreler, bağımsız değişken listesindeki ilgili bağımsız değişkenle aynı konumda gerçekleşmeleri için yeniden sıralanmaz.

Bir bağımsız değişken listesi `A`, bir bağımsız değişken ifadeleri kümesi `{E1, E2, ..., En}` ve `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}` parametre türleriyle `Mp` ve `Mq`, `Mp` ' dan ***daha iyi bir işlev üyesi*** olarak tanımlanmıştır @no__t

*  Her bağımsız değişken için, `Ex` ' dan `Qx` ' e örtük dönüştürme `Ex` ' den `Px` ' e kadar örtülü dönüşümden daha iyi değildir ve
*  en az bir bağımsız değişken için, `Ex` ' dan `Px` ' e dönüştürme `Ex` ' den `Qx` ' e kadar olan dönüşümden daha iyidir.

Bu değerlendirmeyi gerçekleştirirken, `Mp` veya `Mq` Genişletilmiş biçimde geçerliyse, `Px` veya `Qx` parametre listesinin genişletilmiş biçimindeki bir parametreye başvurur.

@ No__t-0 ve `{Q1, Q2, ..., Qn}` parametre tür dizileri eşdeğerdir (yani, her @no__t 2 ' ye karşılık gelen `Qi` ' e bir kimlik dönüştürmesi varsa), daha iyi işlev üyesini belirleyebilmek için aşağıdaki bağlama kuralları uygulanır.

*  @No__t-0 genel olmayan bir yöntem ise ve `Mq` genel bir yöntem ise, `Mp` `Mq` ' ten daha iyidir.
*  Aksi takdirde, `Mp` normal biçimde uygulanabiliyorsa ve `Mq` `params` dizisine sahipse ve yalnızca genişletilmiş biçimde uygulanabilir ise, `Mp` `Mq` ' ten daha iyidir.
*  Aksi takdirde, `Mp` ' ı `Mq` ' den daha fazla tanımlanmış parametre içeriyorsa, `Mp` @no__t 3 ' ten daha iyidir. Her iki yöntem de `params` dizileri içeriyorsa ve yalnızca genişletilmiş formlarında uygulanabildiğinde bu durum oluşabilir.
*  Aksi halde, `Mp` ' ın tüm parametrelerinin karşılık gelen bir bağımsız değişkeni varsa, varsayılan bağımsız değişkenlerin `Mq` ' de en az bir isteğe bağlı parametre yerine kullanılması gerekiyorsa `Mp` `Mq` ' ten daha iyidir.
*  Aksi takdirde, `Mp` ' ı `Mq` ' den daha belirli parametre türlerine sahipse, `Mp` `Mq` ' ten daha iyidir. @No__t-0 ve `{S1, S2, ..., Sn}`, `Mp` ve `Mq` ' ün örneklenmiş ve genişletilmemiş parametre türlerini temsil eder. `Mp` ' ın parametre türleri `Mq` ' den daha özeldir, her bir parametre için `Rx` `Sx` ' ten daha az değildir ve en az bir parametre için `Rx` `Sx` ' ten daha özgüdür:
   *  Tür parametresi, tür olmayan bir parametreden daha az özgüdür.
   *  Yinelemeli olarak, oluşturulmuş bir tür, en az bir tür bağımsız değişkeni daha büyükse ve hiçbir tür bağımsız değişkeni diğer bir tür bağımsız değişkeniyle daha az özel değilse, oluşturulmuş bir tür farklı oluşturulmuş türden (aynı sayıda tür bağımsız değişkenle) daha özgüdür.
   *  Bir dizi türü, birincinin öğe türü ikincisinin öğe türünden daha belirgin ise, başka bir dizi türünden (aynı sayıda boyut ile) daha özgüdür.
*  Aksi takdirde, bir üye yükseltilmemiş olmayan bir işleçtir ve diğeri bir yükseltilmemiş işleçse, yükseltilmemiş olmayan bir tane daha iyidir.
*  Aksi halde, hiçbir işlev üyesi daha iyidir.

#### <a name="better-conversion-from-expression"></a>Deyimden daha iyi dönüştürme

Bir örtük dönüştürme `E`  ifadesinden `T1` türüne dönüştüren ve `E` ifadesinden `T2` türüne dönüştüren örtük bir dönüştürme `C2` @no__t @no__t, @no__-6 ' dan ***daha iyi bir dönüştürme*** t-9 0 ' u ve en az birini aşağıdaki tutmalarla tam olarak eşleşmez:

* `E` @no__t tam olarak eşleşir-1 ([tam olarak eşleşen ifade](expressions.md#exactly-matching-expression))
* `T1`, `T2` ' den daha iyi bir dönüştürme hedefi ([daha iyi dönüştürme hedefi](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Tam eşleştirme Ifadesi

@No__t-0 ve `T` türünde bir ifade verildiğinde, aşağıdakilerden biri varsa `T` ' ü tam olarak  ile eşleşir:

*  `E` `S` türüne sahip ve `S` ' den `T` ' e bir kimlik dönüştürmesi var
*  `E` Anonim bir işlevdir, `T` bir temsilci türü `D` veya bir ifade ağacı türü `Expression<D>` ve aşağıdakilerden biri olabilir:
   *  @No__t-2 ' nin parametre listesi bağlamında `E` için @no__t çıkarılan bir dönüş türü vardır ([çıkarılan dönüş türü](expressions.md#inferred-return-type)) ve `X` ' ten dönüş türü `D` ' e bir kimlik dönüştürme var
   *  @No__t-0, zaman uyumsuz değil ve `D` bir dönüş türüne sahip `Y` ' dir veya `E` zaman uyumsuz ve `D` ' ü bir dönüş türü `Task<Y>` ' i ve aşağıdakilerden birini içerir:
      * @No__t-0 gövdesi tam olarak eşleşen bir ifadedir-1 @no__t
      * @No__t-0 gövdesi, her dönüş deyiminin tamamen @no__t eşleşen bir ifade döndürdüğü bir deyim bloğudur.

#### <a name="better-conversion-target"></a>Daha iyi dönüştürme hedefi

@No__t-0 ve `T2` olan iki farklı tür verildiğinde `T1` `T2` ' ten `T1` ' e örtük dönüştürme yoksa ve aşağıdakilerden en az biri varsa, `T2` ' ten daha iyi bir dönüştürme hedefidir:

*  @No__t-0 ' dan `T2` ' e örtük dönüştürme var
*  `T1`, bir temsilci türü `D1` veya bir ifade ağacı türü `Expression<D1>`, `T2` bir temsilci türü `D2` veya bir ifade ağacı türü `Expression<D2>`, `D1` bir dönüş türü `S1` ve aşağıdakilerden biri olabilir :
   * `D2` void döndürüyor
   * `D2` ' a bir dönüş türü `S2` ve `S1` `S2` ' ten daha iyi bir dönüştürme hedefidir
*  `T1` `Task<S1>`, `T2` `Task<S2>` ve `S1` `S2` ' ten daha iyi bir dönüştürme hedefi
*  `S1` veya `S1?` ' dir; burada `S1` ' ün işaretli bir integral türü olduğu ve `T2` ' ün bir işaretsiz integral türü olduğu `S2` veya `S2?` @no__t. Engelle
   * `S1` `sbyte` ve `S2` `byte`, `ushort`, `uint` veya `ulong`
   * `S1` `short` ve `S2` `ushort`, `uint` veya `ulong`
   * `S1` `int` ve `S2` `uint` veya `ulong`
   * `S1` `long` ve `S2` `ulong`

#### <a name="overloading-in-generic-classes"></a>Genel sınıflarda aşırı yükleme

Belirtilen imzaların benzersiz olması gerekir, ancak tür bağımsız değişkenlerinin değişimi de özdeş imzalara neden olabilir. Bu gibi durumlarda, yukarıdaki aşırı yükleme çözümünün bağlama kuralları en belirli üyeyi seçer.

Aşağıdaki örneklerde, bu kurala göre geçerli ve geçersiz olan aşırı yüklemeler gösterilmektedir:

```csharp
interface I1<T> {...}

interface I2<T> {...}

class G1<U>
{
    int F1(U u);                  // Overload resolution for G<int>.F1
    int F1(int i);                // will pick non-generic

    void F2(I1<U> a);             // Valid overload
    void F2(I2<U> a);
}

class G2<U,V>
{
    void F3(U u, V v);            // Valid, but overload resolution for
    void F3(V v, U u);            // G2<int,int>.F3 will fail

    void F4(U u, I1<V> v);        // Valid, but overload resolution for    
    void F4(I1<V> v, U u);        // G2<I1<int>,int>.F4 will fail

    void F5(U u1, I1<V> v2);      // Valid overload
    void F5(V v1, U u2);

    void F6(ref U u);             // valid overload
    void F6(out V v);
}
```

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi

Dinamik olarak bağlı işlemler için, derleme zamanında çözümleme için olası aday adayları bilinmiyor. Ancak, bazı durumlarda aday kümesi derleme zamanında bilinir:

*  Dinamik bağımsız değişkenlerle statik yöntem çağrıları
*  Örnek yöntemi, alıcının dinamik bir ifade olmadığı yerleri çağırır
*  Alıcının dinamik bir ifade olmadığı Dizin Oluşturucu çağrıları
*  Dinamik bağımsız değişkenlerle Oluşturucu çağrıları

Bu durumlarda, her bir aday için sınırlı bir derleme zamanı denetimi gerçekleştirilerek, bunlardan herhangi birinin çalışma zamanında uygulayıp uygulamamadığını görebilirler. Bu denetim aşağıdaki adımlardan oluşur:

*  Kısmi tür çıkarımı: @No__t-0 türünde bir bağımsız değişkene doğrudan veya dolaylı olarak bağımlı olmayan herhangi bir tür bağımsız değişkeni, [tür çıkarımı](expressions.md#type-inference)kuralları kullanılarak çıkarsanacaktır. Kalan tür bağımsız değişkenleri bilinmiyor.
*  Kısmi uygulanabilirlik denetimi: Uygulanabilirlik, [uygulanabilir işlev üyesine](expressions.md#applicable-function-member)göre denetlenir, ancak türleri bilinmeyen parametreler yok sayılıyor.
*  Bu testi hiçbir aday geçirmediğinde, derleme zamanı hatası oluşur.

### <a name="function-member-invocation"></a>İşlev üyesi çağırma

Bu bölümde, belirli bir işlev üyesini çağırmak için çalışma zamanında gerçekleşen işlem açıklanmaktadır. Bir bağlama zamanı işleminin, büyük olasılıkla bir aday işlev üyesi kümesine aşırı yükleme çözümlemesi uygulayarak, çağırmak için belirli bir üyeyi daha önce belirlediğini kabul edilir.

Çağırma işlemini açıklama amaçlarıyla, işlev üyeleri iki kategoriye ayrılmıştır:

*  Statik işlev üyeleri. Bunlar örnek oluşturucular, statik yöntemler, statik özellik erişimcileri ve Kullanıcı tanımlı işleçlerdir. Statik işlev üyeleri her zaman sanal değildir.
*  Örnek işlevi üyeleri. Bunlar örnek yöntemleridir, örnek özellik erişimcileri ve Dizin Oluşturucu erişimcileri. Örnek işlevi üyeleri sanal olmayan veya sanal değil, her zaman belirli bir örnek üzerinde çağrılır. Örnek bir örnek ifadesi tarafından hesaplanır ve işlev üyesi içinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir hale gelir.

Bir işlev üye çağrısının çalışma zamanı işleme aşağıdaki adımlardan oluşur; burada `M` İşlev üyesidir ve `M` bir örnek üyesiyse, `E` örnek ifadedir:

*  @No__t-0 bir statik işlev üyesiyse:
   * Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.
   * `M` çağrılır.

*  @No__t-0 bir *value_type*içinde belirtilen bir örnek işlev üyesiyse:
   * `E` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
   * @No__t-0 bir değişken olarak sınıflandırılmadıysa, `E` ' in türünün geçici bir yerel değişkeni oluşturulur ve bu değişkene `E` değeri atanır. `E` daha sonra bu geçici yerel değişkene bir başvuru olarak yeniden sınıflanır. Geçici değişken `M` içinde `this` olarak erişilebilir, ancak başka hiçbir şekilde kullanılamaz. Bu nedenle, yalnızca `E` gerçek bir değişken olduğunda, çağıranın `M` `this` ' ye yaptığı değişiklikleri gözlemlemek mümkün olur.
   * Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.
   * `M` çağrılır. @No__t-0 tarafından başvurulan değişken `this` tarafından başvurulan değişken haline gelir.

*  @No__t-0 bir *reference_type*içinde belirtilen bir örnek işlev üyesiyse:
   * `E` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
   * Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.
   * @No__t-0 türü bir *value_type*ise, `E` ' ü `object` türüne dönüştürmek için bir paketleme dönüştürmesi ([kutulama dönüştürmeleri](types.md#boxing-conversions)) gerçekleştirilir ve aşağıdaki adımlarda `E` `object` türünde olduğu kabul edilir. Bu durumda, `M` yalnızca `System.Object` ' in üyesi olabilir.
   * @No__t-0 değeri geçerli olacak şekilde işaretlendi. @No__t-0 değeri `null` ise, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
   * Çağrılacak işlev üyesi uygulama belirlenir:
     * @No__t-0 ' ın bağlama zamanı türü bir arabirim ise, çağrılacak işlev üyesi, `E` tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasıdır. Bu işlev üyesi, `E` tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasını belirlemekte kullanılacak arabirim eşleme kuralları ([arabirim eşleme](interfaces.md#interface-mapping)) uygulanarak belirlenir.
     * Aksi takdirde, `M` bir sanal işlev üyesiyse çağrılacak işlev üyesi, `E` tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasıdır. Bu işlev üyesi, `E` tarafından başvurulan örneğin çalışma zamanı türüne göre `M` ' in en fazla türetilmiş uygulamasını ([sanal yöntemler](classes.md#virtual-methods)) belirlemek için kurallar uygulanarak belirlenir.
     * Aksi takdirde, `M`, sanal olmayan bir işlev üyesidir ve çağrılacak işlev üyesi `M` ' dir.
   * Yukarıdaki adımda belirlenen işlev üyesi uygulama çağrılır. @No__t-0 tarafından başvurulan nesne `this` tarafından başvurulan nesne haline gelir.

#### <a name="invocations-on-boxed-instances"></a>Paketlenmiş örneklerde etkinleştirmeleri

Bir *value_type* içinde uygulanan bir işlev üyesi, aşağıdaki durumlarda bu *value_type* 'ın paketlenmiş bir örneği aracılığıyla çağrılabilir:

*  İşlev üyesi, `object` türünden devralınan bir yöntemin `override` olduğunda ve `object` türünde bir örnek ifadesi aracılığıyla çağrıldığında.
*  İşlev üyesi bir arabirim işlevi üyesinin bir uygulamasıdır ve bir *interface_type*örnek ifadesi aracılığıyla çağrılır.
*  İşlev üyesi bir temsilci aracılığıyla çağrıldığında.

Bu durumlarda kutulanmış örnek, *value_type*değişkenini içeren olarak değerlendirilir ve bu değişken, işlev üye çağrısı içinde `this` tarafından başvurulan değişken olur. Özellikle bu, bir işlev üyesinin paketlenmiş bir örnek üzerinde çağrıldığı anlamına gelir, işlev üyesinin kutulanmış örnekte bulunan değeri değiştirmesi mümkündür.

## <a name="primary-expressions"></a>Birincil ifadeler

Birincil ifadeler, en basit ifade biçimlerini içerir.

```antlr
primary_expression
    : primary_no_array_creation_expression
    | array_creation_expression
    ;

primary_no_array_creation_expression
    : literal
    | interpolated_string_expression
    | simple_name
    | parenthesized_expression
    | member_access
    | invocation_expression
    | element_access
    | this_access
    | base_access
    | post_increment_expression
    | post_decrement_expression
    | object_creation_expression
    | delegate_creation_expression
    | anonymous_object_creation_expression
    | typeof_expression
    | checked_expression
    | unchecked_expression
    | default_value_expression
    | nameof_expression
    | anonymous_method_expression
    | primary_no_array_creation_expression_unsafe
    ;
```

Birincil ifadeler *array_creation_expression*s ve *primary_no_array_creation_expression*s arasında bölünür. Dizi oluşturma-ifadesini diğer basit ifade formlarıyla birlikte listelemek yerine bu şekilde düşünerek, dilbilgisinde,
```csharp
object o = new int[3][1];
```
Aksi takdirde
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Sabit değerler

*Değişmez* değer ([sabit değerler](lexical-structure.md#literals)) içeren bir *primary_expression* , bir değer olarak sınıflandırıldı.


### <a name="interpolated-strings"></a>Ara değerli dizeler

Bir *interpolated_string_expression* , `$` işaretinden sonra, `{` ve `}`, tırnak işareti ve biçimlendirme belirtimleriyle sınırlandırılmış, boşluklar ve tam dize değişmez değeri oluşur. Enterpolasyonlu dize ifadesi, [enterpolasyonlu dize sabit değerleri](lexical-structure.md#interpolated-string-literals)içinde açıklandığı gibi ayrı belirteçlere ayrılmış bir *interpolated_string_literal* sonucudur.

```antlr
interpolated_string_expression
    : '$' interpolated_regular_string
    | '$' interpolated_verbatim_string
    ;

interpolated_regular_string
    : interpolated_regular_string_whole
    | interpolated_regular_string_start interpolated_regular_string_body interpolated_regular_string_end
    ;

interpolated_regular_string_body
    : interpolation (interpolated_regular_string_mid interpolation)*
    ;

interpolation
    : expression
    | expression ',' constant_expression
    ;

interpolated_verbatim_string
    : interpolated_verbatim_string_whole
    | interpolated_verbatim_string_start interpolated_verbatim_string_body interpolated_verbatim_string_end
    ;

interpolated_verbatim_string_body
    : interpolation (interpolated_verbatim_string_mid interpolation)+
    ;
```

Bir ilişkilendirmedeki *constant_expression* `int` ' e örtülü dönüştürmeye sahip olmalıdır.

Bir *interpolated_string_expression* değer olarak sınıflandırılır. Örtülü olarak enterpolasyonlu bir dize dönüştürmesi (örtük olarak bulunan dize[dönüştürmeleri](conversions.md#implicit-interpolated-string-conversions)) ile `System.IFormattable` veya `System.FormattableString` ' e dönüştürülürse, enterpolasyonlu dize ifadesinde bu tür vardır. Aksi takdirde, `string` türü vardır.

Enterpolasyonlu bir dizenin türü `System.IFormattable` veya `System.FormattableString` ise, anlamı `System.Runtime.CompilerServices.FormattableStringFactory.Create` ' ye bir çağrıdır. Tür `string` ise, ifadenin anlamı `string.Format` ' e bir çağrıdır. Her iki durumda da, çağrının bağımsız değişken listesi, her ilişkilendirme için yer tutucular içeren bir biçim dizesi değişmez değerinden ve yer sahiplerine karşılık gelen her bir ifade için bir bağımsız değişkenle oluşur.

Biçim dizesi değişmez değeri aşağıdaki gibi oluşturulur; burada `N`, *interpolated_string_expression*içindeki enterpolasyonun sayısıdır:

*  Bir *interpolated_regular_string_whole* veya *interpolated_verbatim_string_whole* `$` işaretini izliyorsa, biçim dize sabit değeri bu belirteç olur.
*  Aksi takdirde, biçim dizesi değişmez değeri şunlardan oluşur: 
   *  İlk *interpolated_regular_string_start* veya *interpolated_verbatim_string_start*
   *  Sonra her numara için, `0` ' den `N-1` ' ye @no__t: 
      * @No__t ondalık temsili-0
      * Ardından, karşılık gelen *ilişkilendirmeden* bir *constant_expression*varsa, `,` (virgül) ve ardından *constant_expression* değerinin ondalık temsili gelir.
      * Ardından, ilgili ilişkilendirmeden hemen sonra *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* veya *interpolated_verbatim_string_end* .

Sonraki bağımsız değişkenler, sırasıyla *enterpolasyonların* (varsa) *ifadeleridir* .

TODO: örnekler.


### <a name="simple-names"></a>Basit adlar

Bir *simple_name* , isteğe bağlı olarak bir tür bağımsız değişken listesi tarafından izlenen bir tanımlayıcıdan oluşur:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

*Simple_name* , `I` veya `I<A1,...,Ak>` biçiminde `I` tek bir tanımlayıcıdır ve `<A1,...,Ak>` ise isteğe bağlı bir *type_argument_list*. *Type_argument_list* belirtilmediğinde `K` ' i sıfır olacak şekilde düşünün. *Simple_name* değerlendirilir ve aşağıdaki şekilde sınıflandırılacaktır:

*  @No__t-0 sıfırsa ve *simple_name* bir *blok* içinde görünürse ve *blok*(veya kapsayan *bloğun*) yerel değişken bildirim alanı ([Bildirimler](basic-concepts.md#declarations)), ile bir yerel değişken, parametre veya sabit içeriyorsa ad @ no__t-6, sonra *simple_name* bu yerel değişkene, parametreye veya sabitine başvurur ve değişken veya değer olarak sınıflandırılmaktadır.
*  @No__t-0 sıfırsa ve *simple_name* bir genel yöntem bildiriminin gövdesinde görüntüleniyorsa ve bu bildirim @ no__t-2 adında bir tür parametresi içeriyorsa, *simple_name* bu tür parametresine başvurur.
*  Aksi halde, her bir örnek türü için @ no__t-0 ([örnek türü](classes.md#the-instance-type)), hemen kapsayan tür bildiriminin örnek türüyle başlar ve her bir kapsayan sınıfın ya da yapı bildiriminin örnek türüyle (varsa) devam eder:
   *  @No__t-0 ise ve `T` bildirimi, @ no__t-2 adlı bir tür parametresi içeriyorsa, *simple_name* bu tür parametresine başvurur.
   *  Aksi halde, `K` @ no__t-4tür bağımsız değişkenleriyle `T` ' de `I` ' i üye araması ([üye arama](expressions.md#member-lookup)) bir eşleşme üretir:
      * @No__t-0, hemen kapsayan sınıfın veya yapı türünün örnek türü ise ve arama bir veya daha fazla yöntemi tanımlarsa, sonuç, ilişkili bir örnek ifadesi `this` olan bir yöntem grubudur. Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.
      * Aksi takdirde, arama bir örnek üyesi tanımlarsa ve başvuru bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcisinin gövdesinde gerçekleşirse, `T`, hemen kapsayan sınıfın veya yapı türünün örnek türüdür. , `this.I` biçimindeki üye erişimi ([üye erişimi](expressions.md#member-access)) ile aynıdır. Bu yalnızca `K` ' ı sıfır olduğunda meydana gelebilir.
      * Aksi halde, sonuç, `T.I` veya `T.I<A1,...,Ak>` biçimindeki üye erişimi ([üye erişimi](expressions.md#member-access)) ile aynıdır. Bu durumda, *simple_name* 'in bir örnek üyesine başvurması için bağlama zamanı hatası vardır.

*  Aksi halde, *simple_name* gerçekleştiği ad alanından başlayarak, her bir kapsayan ad alanıyla (varsa) devam ederek ve genel ad alanıyla sona ermek üzere @ no__t-0 ad alanı için, aşağıdaki adımlar bir varlık bulunana kadar değerlendirilir:
   *  @No__t-0 sıfırsa ve `I`, @ no__t-2 içindeki bir ad alanının adı ise:
      * *Simple_name* 'in gerçekleştiği konum `N` için bir ad alanı bildirimiyle çevrelenmiş ve ad alanı bildirimi, @ no__t-4 adını bir ile ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa ad alanı veya tür, sonra *simple_name* belirsizdir ve bir derleme zamanı hatası oluşur.
      * Aksi takdirde *simple_name* , `I` adlı ad alanını `N` ' de belirtir.
   *  Aksi takdirde, `N` adı @ no__t-1 ve `K` @ no__t-3type parametrelerine sahip erişilebilir bir tür içeriyorsa:
      * @No__t-0 sıfırsa ve *simple_name* 'in gerçekleştiği konum `N` için bir ad alanı bildirimiyle çevrelenmiş ve ad alanı bildirimi, bir *extern_alias_directive* veya *using_alias_directive* içerir ad alanı veya tür ile @ no__t-5 adını, *simple_name* belirsizdir ve derleme zamanı hatası oluşur.
      * Aksi takdirde, *namespace_or_type_name* verilen tür bağımsız değişkenleriyle oluşturulan türe başvurur.
   *  Aksi takdirde, *simple_name* 'in gerçekleştiği konum @ no__t-1 için bir ad alanı bildirimi ile çevrelenmiş ise:
      * @No__t-0 sıfırsa ve ad alanı bildirimi, @ no__t-3 adını içeri aktarılan bir ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *simple_name* bu ad alanına başvurur veya türüyle.
      * Aksi halde, ad alanı bildiriminin *using_namespace_directive*s ve *using_static_directive*s tarafından içeri aktarılan ad alanları ve tür bildirimleri, tam olarak bir erişilebilir tür veya uzantı olmayan statik üye içeriyorsa @ no__ t-2 ve `K` @ no__t-4tür parametreleri, ardından *simple_name* , belirtilen tür bağımsız değişkenleriyle oluşturulan bu türe veya üyeye başvurur.
      * Aksi halde, ad alanı bildiriminin *using_namespace_directive*s tarafından içeri aktarılan ad alanları ve türler, @ no__t-1 ve `K` @ no__t-3type Parameters adına sahip birden fazla erişilebilir tür veya uzantı dışı statik üye içeriyorsa *simple_name* belirsizdir ve bir hata oluşur.

   Bu adımın tamamı, bir *namespace_or_type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) işlenirken ilgili adıma tam olarak paralel olduğunu unutmayın.

*  Aksi takdirde, *simple_name* tanımsızdır ve bir derleme zamanı hatası oluşur.


### <a name="parenthesized-expressions"></a>Parantez içinde ifadeler

Bir *parenthesized_expression* , parantez içine alınmış bir *ifadeden* oluşur.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Parantez içindeki *ifade* hesaplanarak bir *parenthesized_expression* değerlendirilir. Parantez içindeki *ifade* bir ad alanı veya tür alıyorsa, bir derleme zamanı hatası oluşur. Aksi takdirde, *parenthesized_expression* sonucu içerilen *ifadenin*değerlendirmesinin sonucudur.

### <a name="member-access"></a>Üye erişimi

Bir *member_access* , bir *primary_expression*, *predefined_type*veya *qualified_alias_member*, ardından bir "`.`" belirteci ve ardından bir *tanımlayıcı*tarafından ve isteğe bağlı olarak bir type_argument_list tarafından oluşur.

```antlr
member_access
    : primary_expression '.' identifier type_argument_list?
    | predefined_type '.' identifier type_argument_list?
    | qualified_alias_member '.' identifier
    ;

predefined_type
    : 'bool'   | 'byte'  | 'char'  | 'decimal' | 'double' | 'float' | 'int' | 'long'
    | 'object' | 'sbyte' | 'short' | 'string'  | 'uint'   | 'ulong' | 'ushort'
    ;
```

*Qualified_alias_member* üretimi, [ad alanı diğer adı niteleyicilerine](namespaces.md#namespace-alias-qualifiers)göre tanımlanır.

*Member_access* , `E.I` veya `E.I<A1, ..., Ak>` biçiminde `E` ' ün birincil ifade olduğu `I` ' ün tek bir tanımlayıcıdır ve `<A1, ..., Ak>` ' i isteğe bağlı bir *type_argument_list*. *Type_argument_list* belirtilmediğinde `K` ' i sıfır olacak şekilde düşünün.

@No__t-2 türünde *primary_expression* içeren bir *member_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, derleyici üye erişimini `dynamic` türünde bir özellik erişimi olarak sınıflandırır. *Member_access* 'in anlamını belirlemede aşağıdaki kurallar, *primary_expression*derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. Bu çalışma zamanı sınıflandırması bir yöntem grubuna yol açar, üye erişimi bir *invocation_expression* *primary_expression* olmalıdır.

*Member_access* değerlendirilir ve aşağıdaki şekilde sınıflandırılacaktır:

*  @No__t-0 ise ve `E` bir ad alanı ve `E` ' nin adı @ no__t-3 olan iç içe bir ad alanı içeriyorsa, sonuç bu ad alanıdır.
*  Aksi takdirde, `E` bir ad alanıdır ve `E`, @ no__t-2 ve `K` @ no__t-4type parametrelerine sahip erişilebilir bir tür içeriyorsa, sonuç verilen tür bağımsız değişkenleriyle oluşturulan türdür.
*  @No__t-0 bir *predefined_type* veya bir tür olarak sınıflandırılan bir *primary_expression* ise, `E` bir tür parametresi değilse ve `K` @ no__t-8tür parametrelerine sahip `E` içinde `I` ' ın üye araması ([üye araması](expressions.md#member-lookup)) bir eşleşme oluşturmazsa, sonra `E.I` değerlendirilir ve aşağıdaki şekilde sınıflandırılacaktır:
   *  @No__t-0 bir türü tanımlarsa, sonuç, belirtilen tür bağımsız değişkenleriyle oluşturulan türüdür.
   *  @No__t-0 bir veya daha fazla yöntemi tanımlarsa, sonuç ilişkili örnek ifadesi olmayan bir yöntem grubudur. Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.
   *  @No__t-0 bir `static` özelliği tanımlarsa, sonuç, ilişkili örnek ifadesi olmayan bir özellik erişimiydi.
   *  @No__t-0 `static` alanını tanımlar:
      * Alan `readonly` ise ve başvuru, alanın bildirildiği sınıf veya yapının statik oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani @ no__t-2 içinde @ no__t-1 statik alanının değeri olur.
      * Aksi halde, sonuç bir değişkendir, yani @ no__t-1 içinde @ no__t-0 statik alanı.
   *  @No__t-0 `static` olayını tanımlar:
      * Başvuru, olayın bildirildiği sınıf veya yapı içinde oluşursa ve olay *event_accessor_declarations* ([Olaylar](classes.md#events)) olmadan bildirilirse, `E.I` tam olarak, `I` statik bir alan gibi işlenir.
      * Aksi halde, sonuç, ilişkili örnek ifadesi olmayan bir olay erişimiydi.
   *  @No__t-0 bir sabiti tanımlarsa, sonuç bir değerdir, yani bu sabit değeridir.
    * @No__t-0 bir numaralandırma üyesini tanımlarsa, bu, söz konusu numaralandırma üyesinin değeri olan değer olur.
    * Aksi takdirde, `E.I` geçersiz bir üye başvurusudur ve derleme zamanı hatası oluşur.
*  @No__t-0 ' a bir özellik erişimi, Dizin Oluşturucu erişimi, değişken veya değer, türü @ no__t-1 ve `K` @ no__t-6type bağımsız değişkenleri ile `T` ' te `I` ' ün üye araması ([üye arama](expressions.md#member-lookup)) bir eşleşme üretir, `E.I` değerlendirilir ve şöyle sınıflandırıldı:
   *  İlk olarak, `E` bir özellik veya Dizin Oluşturucu erişimdir, özellik veya Dizin Oluşturucu erişimi değeri elde edilir ([Ifadelerin değerleri](expressions.md#values-of-expressions)) ve `E` bir değer olarak yeniden sınıflandıralınır.
   *  @No__t-0 bir veya daha fazla yöntemi tanımlarsa sonuç, ilişkili bir örnek ifadesi `E` olan bir yöntem grubudur. Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.
   *  @No__t-0 bir örnek özelliği tanımlarsa,
      * @No__t-0 `this` ise, `I` otomatik olarak uygulanan bir özelliği ([Otomatik uygulanan özellikler](classes.md#automatically-implemented-properties)) bir ayarlayıcı olmadan tanımlar ve başvuru bir sınıf veya yapı türü `T` için bir örnek oluşturucu içinde oluşur ve sonuç , `this` tarafından verilen `T` örneğindeki `I` tarafından verilen otomatik özellik için gizli destek alanı olan bir değişkendir.
      * Aksi takdirde, sonuç @ no__t-0 ' ın ilişkili örnek ifadesiyle bir özellik erişimiydi.
   *  @No__t-0 bir *class_type* ve `I` Bu *class_type*örnek alanını tanımlar:
      * @No__t-0 değeri `null` ise, `System.NullReferenceException` oluşturulur.
      * Aksi halde, alan `readonly` ise ve başvuru, alanın bildirildiği sınıfın örnek oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani @ no__t-2 tarafından başvurulan nesnedeki @ no__t-1 alanının değeri.
      * Aksi takdirde, sonuç bir değişkendir, yani @ no__t-1 tarafından başvurulan nesnedeki @ no__t-0 alanı.
   *  @No__t-0 bir *struct_type* ve `I` Bu *struct_type*örnek alanını tanımlar:
      * @No__t-0 bir değer ise veya alan `readonly` ise ve başvuru, alanın bildirildiği yapının örnek oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani @ no__t-3 tarafından verilen yapı örneğindeki @ no__t-2 alanının değeri.
      * Aksi takdirde, sonuç bir değişkendir, yani @ no__t-1 tarafından verilen yapı örneğindeki @ no__t-0 alanı.
   *  @No__t-0 bir örnek olayı tanımlarsa:
      * Başvuru, olayın bildirildiği sınıf veya yapı içinde oluşursa ve olay *event_accessor_declarations* ([Olaylar](classes.md#events)) olmadan bildirilirse ve başvuru `+=` veya `-=` işlecinin sol tarafında gerçekleşmezse `E.I`, tam olarak `I` bir örnek alanı olduğu gibi işlenir.
      * Aksi takdirde, sonuç @ no__t-0 ' ın ilişkili örnek ifadesiyle bir olay erişimiydi.
*  Aksi takdirde, `E.I` ' ı uzantı yöntemi çağırma ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak işlemek için bir girişimde bulunuldu. Bu başarısız olursa, `E.I` geçersiz bir üye başvurusudur ve bir bağlama zamanı hatası oluşur.

#### <a name="identical-simple-names-and-type-names"></a>Aynı basit adlar ve tür adları

@No__t-0 biçiminde bir üye erişiminde, `E` tek bir tanımlayıcıdır ve bir *simple_name* ([basit adlar](expressions.md#simple-names)) olarak @no__t 2 ' nin anlamı, `E` ' in anlamı ile aynı türe sahip bir sabit, alan, özellik, yerel değişken veya parametre ise *type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)), her ikisine de `E` ' in olası anlamları izin verilir. @No__t-0 ' ın iki olası anlamı hiçbir zaman belirsiz değildir, çünkü `I` her iki durumda da `E` türünün bir üyesi olmalıdır. Diğer bir deyişle, kural yalnızca statik üyelere ve derleme zamanı hatası oluşması durumunda `E` ' ın iç içe türlerine erişime izin verir. Örneğin:
```csharp
struct Color
{
    public static readonly Color White = new Color(...);
    public static readonly Color Black = new Color(...);

    public Color Complement() {...}
}

class A
{
    public Color Color;                // Field Color of type Color

    void F() {
        Color = Color.Black;           // References Color.Black static member
        Color = Color.Complement();    // Invokes Complement() on Color field
    }

    static void G() {
        Color c = Color.White;         // References Color.White static member
    }
}
```

#### <a name="grammar-ambiguities"></a>Dilbilgisi belirsizlikleri

*Simple_name* ([basit adlar](expressions.md#simple-names)) ve *member_access* ([üye erişimi](expressions.md#member-access)) üretimleri, ifadelerde dilbilgisi için belirsizlikleri verebilir. Örneğin, ifade:
```csharp
F(G<A,B>(7));
```
iki bağımsız değişkenle `F` ' a çağrı olarak yorumlanamıyor, `G < A` ve `B > (7)`. Alternatif olarak, iki tür bağımsız değişkeni ve bir normal bağımsız değişkeni olan @ no__t-1 Genel yöntemine yapılan bir çağrı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanabilecek.

Bir belirteç dizisi bir *simple_name* ([basit adlar](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)) veya *pointer_member_access* ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)) ile biten bir type_argument_ ile birlikte ayrıştırılamıyorsa  *liste* ([tür bağımsız değişkenleri](types.md#type-arguments)), kapatma `>` belirtecini izleyen belirteç incelenir. Bunlardan biri ise
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
daha sonra *type_argument_list* , *simple_name*, *member_access* veya *pointer_member_access* bir parçası olarak tutulur ve belirteçleri dizisinin diğer olası ayrıştırması atılır. Aksi takdirde, belirteç dizisinin başka bir olası ayrıştırması olmasa bile type_argument_list *simple_name*, *member_access* veya *pointer_member_access*'in bir parçası olarak kabul edilmez. Bu kuralların bir *namespace_or_type_name* içinde ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) *type_argument_list* ayrıştırılırken uygulanmadığını unutmayın. İfade
```csharp
F(G<A,B>(7));
```
Bu kurala göre, iki tür bağımsız değişkeni ve bir normal bağımsız değişken içeren bir genel @no__t yönteme çağrı olan bir bağımsız değişkenle `F` ' a çağrı olarak yorumlanacaktır. Deyimler
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
her biri, iki bağımsız değişkenle `F` ' a çağrı olarak yorumlanır. İfade
```csharp
x = F < A > +y;
```
, bir simple_name yerine bir *type_argument_list* ve ardından bir ikili artı işleci gelen bir olarak değil, bir küçüktür işleci, büyüktür operatörü ve birli artı @no__t işleci olarak yorumlanacaktır. İfadesinde
```csharp
x = y is C<T> + z;
```
`C<T>` belirteçleri, bir *type_argument_list*ile *namespace_or_type_name* olarak yorumlanır.

### <a name="invocation-expressions"></a>Çağırma ifadeleri

Bir yöntemi çağırmak için bir *invocation_expression* kullanılır.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Aşağıdakilerden en az biri tutuyorsa, bir *invocation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)):

* *Primary_expression* , derleme zamanı türü `dynamic` ' dir.
* İsteğe bağlı *argument_list* 'ın en az bir bağımsız değişkeninin derleme zamanı türü `dynamic` ve *primary_expression* bir temsilci türüne sahip değil.

Bu durumda, derleyici *invocation_expression* `dynamic` türünde bir değer olarak sınıflandırır. *İnvocation_expression* anlamını belirlemede aşağıdaki kurallar, *primary_expression* ve derleme zamanı türü olan bağımsız değişkenlerden derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. @no_ _T-2. *Primary_expression* , `dynamic` derleme zamanı türüne sahip değilse, yöntem çağrısı, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.

Bir *invocation_expression* 'ın *primary_expression* bir yöntem grubu veya bir *delegate_type*değeri olmalıdır. *Primary_expression* bir yöntem grubu ise, *invocation_expression* bir yöntem çağrıdır ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)). *Primary_expression* bir *delegate_type*değeri ise, *invocation_expression* bir temsilci çağrıdır ([temsilci etkinleştirmeleri](expressions.md#delegate-invocations)). *Primary_expression* bir yöntem grubu veya bir *delegate_type*değeri değilse, bir bağlama zamanı hatası oluşur.

İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)), metodun parametreleri için değerler veya değişken başvuruları sağlar.

Bir *invocation_expression* değerlendirmesi sonucu aşağıdaki şekilde sınıflandırıldı:

*  *İnvocation_expression* , `void` döndüren bir yöntemi veya temsilciyi çağıralıyorsa, sonuç Nothing olur. Nothing olarak sınıflandırılan bir ifadeye, yalnızca bir *statement_expression* ([Expression deyimleri](statements.md#expression-statements)) bağlamında veya bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) gövdesi olarak izin verilir. Aksi halde bağlama zamanı hatası oluşur.
*  Aksi takdirde, sonuç Yöntem veya temsilci tarafından döndürülen türün bir değeridir.

#### <a name="method-invocations"></a>Yöntem etkinleştirmeleri

Bir yöntem çağrısı için, *invocation_expression* öğesinin *primary_expression* bir yöntem grubu olması gerekir. Yöntem grubu, çağrılacak bir yöntemi veya çağırmak için belirli bir yöntemi seçebileceğiniz aşırı yüklenmiş yöntemleri belirler. İkinci durumda, çağrılacak belirli yöntemin belirlenmesi, *argument_list*içindeki bağımsız değişkenlerin türleri tarafından belirtilen bağlamı temel alır.

@No__t-0, `M` bir yöntem grubu (muhtemelen bir *type_argument_list*dahil) ve `A` isteğe bağlı bir *argument_list*olan bir yöntem çağrısı olan bağlama zamanı işleme, aşağıdaki adımlardan oluşur:

*  Yöntem çağrısı için aday yöntemler kümesi oluşturulur. Her yöntem için `M`  Yöntem grubuyla ilişkilendirilir:
   *  @No__t-0 genel değilse, şu durumlarda `F` bir adaydır:
      * `M` tür bağımsız değişken listesine sahip değil ve
      * `F` `A` ' e ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre uygulanabilir.
   *  @No__t-0 genel ise ve `M` tür bağımsız değişken listesine sahip değilse, şu durumlarda `F` bir adaydır:
      * Tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)) başarılı, çağrının tür bağımsız değişkenlerinin bir listesini, ve
      * Çıkarsanan tür bağımsız değişkenleri karşılık gelen yöntem türü parametreleri yerine getirildikten sonra, F parametre listesindeki tüm oluşturulmuş türler, kısıtlamalarını karşılar ([kısıtlamalar karşılamaları](types.md#satisfying-constraints)) ve `F` ' in parametre listesi ile geçerlidir `A` ' ye ([uygulanabilir işlev üyesine](expressions.md#applicable-function-member)) göre.
   *  @No__t-0 geneldir ve `M` bir tür bağımsız değişken listesi içeriyorsa, şu durumlarda `F` bir adaydır:
      * `F`, tür bağımsız değişkeni listesinde sağlanan sayıda yöntem türü parametreye sahiptir ve
      * Tür bağımsız değişkenleri, karşılık gelen yöntem türü parametreleri yerine eklendikten sonra, F parametre listesindeki tüm oluşturulmuş türler kısıtlamalarını karşılar ([kısıtlamalar karşılamaları](types.md#satisfying-constraints)) ve `F` ' in parametre listesi şu şekilde geçerlidir `A` ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)).
*  Aday yöntemler kümesi yalnızca en türetilmiş türlerden Yöntemler içerecek şekilde azaltılır: Küme içinde `C.F` olan her yöntem için, `C` `F` yönteminin bildirildiği türdür, `C` temel türünde belirtilen tüm yöntemler kümeden kaldırılır. Ayrıca, `C` `object` dışında bir sınıf türü ise, bir arabirim türünde belirtilen tüm yöntemler kümeden kaldırılır. (Bu ikinci kural yalnızca Yöntem grubu, nesne dışındaki etkin bir temel sınıfa sahip olan bir tür parametresindeki üye aramasının ve boş olmayan etkin bir arabirim kümesinin sonucu olduğunda etkiler.)
*  Elde edilen aday yöntemler kümesi boşsa, aşağıdaki adımlar üzerinde daha fazla işleme terk edilir ve bunun yerine, çağırma yöntemi çağırma ([uzantı yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak çağrıyı işlemek için bir girişimde bulunuldu. Bu başarısız olursa, geçerli hiçbir yöntem yoktur ve bir bağlama zamanı hatası oluşur.
*  Aday Yöntemler kümesinin en iyi yöntemi, [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak tanımlanır. Tek bir en iyi yöntem tanımlanamıyorsa, yöntem çağırma belirsizdir ve bir bağlama zamanı hatası oluşur. Aşırı yükleme çözümlemesi gerçekleştirirken, genel yöntemin parametreleri karşılık gelen yöntem türü parametreleri için tür bağımsız değişkenleri (sağlanan veya çıkartılan) değiştirildikten sonra değerlendirilir.
*  Seçilen en iyi yöntemin son doğrulaması gerçekleştirilir:
   * Yöntemi, Yöntem grubu bağlamında onaylanır: En iyi yöntem bir statik yöntem ise, Yöntem grubu bir *simple_name* veya *member_access* aracılığıyla bir tür aracılığıyla sonuçlanmış olmalıdır. En iyi yöntem bir örnek yöntemi ise, Yöntem grubu bir *simple_name*, bir değişken veya değer aracılığıyla *member_access* veya bir *base_access*ile sonuçlanmış olmalıdır. Bu gereksinimlerin hiçbiri doğru değilse, bir bağlama zamanı hatası oluşur.
   * En iyi yöntem genel bir yöntem ise, tür bağımsız değişkenleri (sağlanan veya çıkartılan) genel yöntemde belirtilen kısıtlamalara (karşılayan[kısıtlamalar](types.md#satisfying-constraints)) göre denetlenir. Herhangi bir tür bağımsız değişkeni tür parametresindeki karşılık gelen kısıtlamayı karşılamaz bir bağlama zamanı hatası oluşur.

Yukarıdaki adımlarla bağlama zamanında bir Yöntem seçildikten ve doğrulandıktan sonra, gerçek çalışma zamanı çağrısı, [dinamik aşırı yükleme çözümünün derleme zamanı denetiminde](expressions.md#compile-time-checking-of-dynamic-overload-resolution)açıklanan işlev üye çağırma kurallarına göre işlenir.

Yukarıda açıklanan çözümleme kurallarının sezgisel etkisi aşağıdaki gibidir: Bir yöntem çağrısı tarafından çağrılan belirli bir yöntemi bulmak için, yöntem çağırma tarafından belirtilen türle başlayın ve en az bir geçerli, erişilebilir, geçersiz kılma olmayan yöntem bildirimi bulunana kadar devralma zincirini devam edin. Daha sonra tür çıkarımı ve aşırı yükleme çözümlemesini, bu tür içinde belirtilen geçerli, erişilebilir, geçersiz kılma olmayan yöntemler kümesi ve bu nedenle seçilen yöntemi çağırmak için gerçekleştirin. Hiçbir yöntem bulunmazsa, çağırma yöntemini bir genişletme yöntemi çağrısı olarak işlemeyi deneyin.

#### <a name="extension-method-invocations"></a>Genişletme yöntemi etkinleştirmeleri

Bir yöntem çağrısında ([paketlenmiş örneklerde çağırma](expressions.md#invocations-on-boxed-instances)), formlardan birinin
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
çağrının normal işlemesi uygulanabilir bir yöntem bulamıyorsa, yapıyı uzantı metodu çağrısı olarak işlemek için bir girişimde bulunuldu. Eğer *Expr* veya *bağımsız* değişkenlerin herhangi birinde derleme zamanı türü `dynamic` varsa, genişletme yöntemleri uygulanmaz.

Amaç, karşılık gelen statik yöntem çağrısının gerçekleşmesi için en iyi *type_name* `C` ' i bulledir:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

@No__t-0 genişletme yöntemi şu durumlarda ***uygundur*** :

*  `Ci`, genel olmayan, iç içe olmayan bir sınıftır
*  @No__t-0 ' ın adı *tanımlayıcı*
*  `Mj`, yukarıda gösterildiği gibi bağımsız değişkenlere statik bir yöntem olarak uygulandığında erişilebilir ve uygulanabilir olur
*  Bir örtük kimlik, başvuru veya paketleme dönüştürmesi, `Mj` ' in ilk parametresinin türüne göre *ifade* ' ten oluşur.

@No__t-0 araması aşağıdaki gibi devam eder:

*  En yakın kapsayan ad alanı bildirimiyle başlayarak, kapsayan her bir ad alanı bildirimine devam edin ve kapsayan derleme birimi ile sona ermek üzere, bir dizi genişletme yöntemini bulmak için art arda denemeler yapılır:
   * Verilen ad alanı veya derleme birimi, uygun uzantı yöntemleriyle `Mj` @no__t doğrudan genel olmayan tür bildirimleri içeriyorsa, bu uzantı yöntemlerinin kümesi aday kümesidir.
   * *Using_static_declarations* tarafından içeri aktarılan ve doğrudan verilen ad alanındaki veya derleme birimindeki *using_namespace_directive*s tarafından içeri aktarılan ad alanlarında doğrudan bildirildiği @no__t takdirde, daha sonra uygun uzantı @no__t yöntemlerini içerir. Bu uzantı yöntemlerinin kümesi aday kümesidir.
*  Herhangi bir kapsayan ad alanı bildiriminde veya derleme biriminde hiçbir aday kümesi bulunamazsa, derleme zamanı hatası oluşur.
*  Aksi halde, aşırı yükleme çözümlemesi aday kümesine ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) açıklandığı şekilde uygulanır. Tek bir en iyi yöntem bulunamazsa, derleme zamanı hatası oluşur.
*  `C`, en iyi yöntemin bir genişletme yöntemi olarak bildirildiği türdür.

@No__t-0 ' ı hedef olarak kullanarak, yöntem çağrısı daha sonra statik bir yöntem çağırma ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) olarak işlenir.

Önceki kurallar, örnek yöntemlerinin genişletme yöntemlerine göre öncelikli olduğunu, iç ad alanı bildirimlerinde kullanılabilen genişletme yöntemlerinin, dış ad alanı bildirimlerinde bulunan genişletme yöntemlerine ve bu uzantıya göre öncelikli olduğunu ifade ediyor. bir ad alanında doğrudan tanımlanan Yöntemler, using ad alanı yönergesi ile aynı ad alanına aktarılan genişletme yöntemlerine göre önceliklidir. Örneğin:
```csharp
public static class E
{
    public static void F(this object obj, int i) { }

    public static void F(this object obj, string s) { }
}

class A { }

class B
{
    public void F(int i) { }
}

class C
{
    public void F(object obj) { }
}

class X
{
    static void Test(A a, B b, C c) {
        a.F(1);              // E.F(object, int)
        a.F("hello");        // E.F(object, string)

        b.F(1);              // B.F(int)
        b.F("hello");        // E.F(object, string)

        c.F(1);              // C.F(object)
        c.F("hello");        // C.F(object)
    }
}
```

Örnekte, `B` ' ın yöntemi ilk uzantı yöntemine göre önceliklidir ve `C` ' in yöntemi her iki uzantı yönteminden önceliklidir.

```csharp
public static class C
{
    public static void F(this int i) { Console.WriteLine("C.F({0})", i); }
    public static void G(this int i) { Console.WriteLine("C.G({0})", i); }
    public static void H(this int i) { Console.WriteLine("C.H({0})", i); }
}

namespace N1
{
    public static class D
    {
        public static void F(this int i) { Console.WriteLine("D.F({0})", i); }
        public static void G(this int i) { Console.WriteLine("D.G({0})", i); }
    }
}

namespace N2
{
    using N1;

    public static class E
    {
        public static void F(this int i) { Console.WriteLine("E.F({0})", i); }
    }

    class Test
    {
        static void Main(string[] args)
        {
            1.F();
            2.G();
            3.H();
        }
    }
}
```

Bu örneğin çıktısı şu şekilde olur:
```console
E.F(1)
D.G(2)
C.H(3)
```
`D.G` `C.G` ' den önceliklidir `E.F` `D.F` ve `C.F` arasında önceliklidir.

#### <a name="delegate-invocations"></a>Temsilci etkinleştirmeleri

Bir temsilci çağrısı için, *invocation_expression* öğesinin *primary_expression* bir *delegate_type*değeri olması gerekir. Ayrıca, *delegate_type* ile aynı parametre listesine sahip bir işlev üyesi olmak *için,* *delegate_type* , argument_ açısından uygulanabilir ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) olmalıdır *invocation_expression*listesi.

@No__t-0 biçiminde bir temsilci çağırma işleminin çalışma zamanı işleme `D` ' in bir *delegate_type* *primary_expression* ve `A` isteğe bağlı bir *argument_list*, aşağıdaki adımlardan oluşur:

*  `D` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
*  @No__t-0 değeri geçerli olacak şekilde işaretlendi. @No__t-0 değeri `null` ise, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
*  Aksi takdirde, `D` bir temsilci örneğine başvurudur. İşlev üyesi etkinleştirmeleri ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), temsilcinin çağırma listesindeki çağrılabilir varlıkların her birinde gerçekleştirilir. Örnek ve örnek yönteminden oluşan çağrılabilir varlıklar için, çağırma örneği çağrılabilir varlıkta bulunan örneğidir.

### <a name="element-access"></a>Öğe erişimi

Bir *element_access* *primary_no_array_creation_expression*, ardından bir "`[`" belirteci ve ardından bir *argument_list*ve ardından "`]`" belirteci ile oluşur. *Argument_list* , virgülle ayrılmış bir veya daha fazla *bağımsız değişkenden*oluşur.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

Bir *element_access* öğesinin *argument_list* `ref` veya `out` bağımsız değişken içermesine izin verilmez.

Aşağıdakilerden en az biri tutuyorsa, bir *element_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)):

* *Primary_no_array_creation_expression* , derleme zamanı türü `dynamic` ' dir.
* *Argument_list* öğesinin en az bir ifadesinin derleme zamanı türü `dynamic` ve *primary_no_array_creation_expression* bir dizi türüne sahip değil.

Bu durumda, derleyici *element_access* `dynamic` türünde bir değer olarak sınıflandırır. *Element_access* anlamını belirlemede aşağıdaki kurallar, *primary_no_array_creation_expression* ve *argument_list* 'in derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır derleme zamanı türü `dynamic` olan ifadeler. *Primary_no_array_creation_expression* , `dynamic` derleme zamanı türüne sahip değilse, öğe erişimi, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.

Bir *element_access* öğesinin *primary_no_array_creation_expression* değeri *array_type*ise, *element_access* bir dizi erişimdir ([dizi erişimi](expressions.md#array-access)). Aksi takdirde, *primary_no_array_creation_expression* bir veya daha fazla Indexer üyesine sahip bir sınıf, yapı veya arabirim türünün bir değişkeni ya da değeri olmalıdır ve bu durumda *element_access* bir Dizin Oluşturucu erişimi ([Dizin Oluşturucu erişimi](expressions.md#indexer-access)) olur.

#### <a name="array-access"></a>Dizi erişimi

Dizi erişimi için, *element_access* öğesinin *primary_no_array_creation_expression* bir *array_type*değeri olması gerekir. Ayrıca, bir dizi erişiminin *argument_list* adlandırılmış bağımsız değişkenler içermesine izin verilmez. *Argument_list* içindeki ifade sayısı *array_type*derecesi ile aynı olmalıdır ve her ifadenin `int`, `uint`, `long`, `ulong` türünde olması veya bu türlerden bir veya daha fazlasına örtük olarak dönüştürülebilir olması gerekir.

Dizi erişiminin hesaplanmasının sonucu, dizinin öğe türünün bir değişkenidir. Yani, *argument_list*içindeki ifade (ler) in değeri tarafından seçilen dizi öğesidir.

@No__t-0 biçiminde dizi erişiminin çalışma zamanı işleme `P` ' in bir *array_type* *primary_no_array_creation_expression* ve `A` bir *argument_list*, aşağıdaki adımlardan oluşur:

*  `P` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
*  *Argument_list* 'in Dizin ifadeleri, soldan sağa doğru sırayla değerlendirilir. Her bir dizin ifadesinin değerlendirmesi aşağıdaki türlerden birine örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir: `int`, `uint`, `long`, `ulong`. Bu listede örtük bir dönüştürmenin bulunduğu ilk tür seçilir. Örneğin, Dizin ifadesi `short` ise, `short` ' den `int` ' e ve `short` ' e kadar `long` arasında örtük dönüştürmeler yapıladıklarından `int` ' e örtük dönüştürme gerçekleştirilir. Bir dizin ifadesi veya sonraki örtük dönüştürme değerlendirmesi bir özel duruma neden oluyorsa, daha fazla dizin ifadesi değerlendirilmez ve başka bir adım yürütülmez.
*  @No__t-0 değeri geçerli olacak şekilde işaretlendi. @No__t-0 değeri `null` ise, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
*  *Argument_list* içindeki her ifadenin değeri, `P` tarafından başvurulan dizi örneği boyutunun gerçek sınırlarına göre denetlenir. Bir veya daha fazla değer Aralık dışında ise, `System.IndexOutOfRangeException` oluşturulur ve başka bir adım yürütülmez.
*  Dizin ifadeleri tarafından verilen dizi öğesinin konumu hesaplanır ve bu konum dizi erişiminin sonucu olur.

#### <a name="indexer-access"></a>Dizin Oluşturucu erişimi

Bir Dizin Oluşturucu erişimi için *element_access* öğesinin *primary_no_array_creation_expression* bir sınıf, yapı veya arabirim türünde bir değişken ya da değer olması gerekir ve bu tür, şu değere göre geçerli olan bir veya daha fazla dizinleyici uygulamalıdır *element_access* *argument_list* .

@No__t-0 biçiminde bir Dizin Oluşturucu erişiminin bağlama zamanı işleme `P` ' in bir sınıf, yapı veya arabirim türünün bir *primary_no_array_creation_expression* `T` ve `A` ' ün bir *argument_list*olması, aşağıdakilerden oluşur olanları

*  @No__t-0 tarafından belirtilen dizin oluşturucular kümesi oluşturulur. Küme, `T` ' da belirtilen tüm dizin oluşturuculardan veya `override` bildirimleri olmayan `T` temel türünde veya geçerli bağlamda ([üye erişimi](basic-concepts.md#member-access)) erişilebilir olan tüm dizin oluşturuculardan oluşur.
*  Küme, uygulanabilir ve diğer Dizin oluşturucular tarafından gizlenmediği dizin oluşturucularının düşürüldü. Aşağıdaki kurallar, küme içindeki her bir dizin oluşturucuya @no__t (`S` ' in Indexer `I` ' nin bildirildiği türdür) geçerlidir:
   * @No__t-0 `A` ' e ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre geçerli değilse, kümeden `I` kaldırılır.
   * @No__t-0 `A` ' e ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre geçerliyse, `S` temel türünde belirtilen tüm dizin oluşturucular kümeden kaldırılır.
   * @No__t-0 `A` ' e ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre uygulanabilir ve `S` `object` dışında bir sınıf türüdür, bir arabirimde bildirildiği tüm dizin oluşturucular kümeden kaldırılır.
*  Elde edilen aday Dizin oluşturucular kümesi boşsa, uygulanabilir Dizin oluşturucular yok ve bir bağlama zamanı hatası oluşur.
*  Aday Dizin oluşturucular kümesinin en iyi Dizin Oluşturucusu, [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak tanımlanır. Tek bir en iyi Dizin Oluşturucu tanımlanamıyorsa, Dizin Oluşturucu erişimi belirsizdir ve bir bağlama zamanı hatası oluşur.
*  *Argument_list* 'in Dizin ifadeleri, soldan sağa doğru sırayla değerlendirilir. Dizin Oluşturucu erişimini işlemenin sonucu, Dizin Oluşturucu erişimi olarak sınıflandırılan bir ifadedir. Dizin Oluşturucu erişim ifadesi, yukarıdaki adımda belirlenen dizin oluşturucuya başvurur ve `P` ' ın ilişkili bir örnek ifadesine ve `A` ' in ilişkili bağımsız değişken listesine sahiptir.

Bir Dizin Oluşturucu erişimi, kullanıldığı içeriğe bağlı olarak, dizin oluşturucunun *Get erişimcisinin* veya *set erişimcisinin* çağrısına neden olur. Dizin Oluşturucu erişimi bir atamanın hedefi ise, *küme erişimcisi* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)). Diğer tüm durumlarda, *get erişimcisi* geçerli değeri ([ifadelerin değerleri](expressions.md#values-of-expressions)) almak için çağrılır.

### <a name="this-access"></a>Bu erişim

Bir *This_Access* , `this` ayrılmış sözcükten oluşur.

```antlr
this_access
    : 'this'
    ;
```

Bir *This_Access* yalnızca bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcinin *bloğunda* izin verilir. Aşağıdaki anlamlara sahiptir:

*  @No__t-0 bir sınıfın örnek Oluşturucusu içindeki bir *primary_expression* kullanıldığında, değer olarak sınıflandırılmaktadır. Değerin türü, kullanımın gerçekleştiği sınıfın örnek türü ([örnek türü](classes.md#the-instance-type)) ve değer, oluşturulmakta olan nesneye bir başvurudur.
*  @No__t-0 bir sınıfın örnek metodu veya örnek erişimcisi içindeki bir *primary_expression* kullanıldığında, değer olarak sınıflandırılmaktadır. Değerin türü, kullanımının gerçekleştiği sınıfın örnek türü ([örnek türü](classes.md#the-instance-type)) ve değer, yöntemin veya erişimcinin çağrıldığı nesneye bir başvurudur.
*  @No__t-0 bir yapının örnek Oluşturucusu içindeki bir *primary_expression* kullanıldığında, değişken olarak sınıflandırılır. Değişkenin türü, kullanımın gerçekleştiği yapının örnek türüdür ([örnek türü](classes.md#the-instance-type)) ve değişken oluşturulan yapıyı temsil eder. Bir yapının örnek oluşturucusunun `this` değişkeni, yapı türünün bir `out` parametresiyle tamamen aynı şekilde davranır — özellikle, bu, değişkenin örnek oluşturucusunun her yürütme yolunda kesinlikle atanması gerektiği anlamına gelir.
*  @No__t-0 bir yapının örnek metodu veya örnek erişimcisi içindeki bir *primary_expression* kullanıldığında, bir değişken olarak sınıflandırılır. Değişkenin türü, kullanımının gerçekleştiği yapının örnek türüdür ([örnek türü](classes.md#the-instance-type)).
   * Yöntem veya erişimci bir yineleyici ([yineleyiciler](classes.md#iterators)) değilse, `this` değişkeni Yöntem veya erişimcinin çağrıldığı yapıyı temsil eder ve yapı türünün `ref` parametresiyle tam olarak aynı şekilde davranır.
   * Yöntem veya erişimci bir yineleyici ise, `this` değişkeni, yöntemin veya erişimcinin çağrıldığı yapının bir kopyasını temsil eder ve yapı türünün bir değer parametresiyle tam olarak aynı şekilde davranır.

Yukarıda listelenenlerin dışında bir bağlamda bir *primary_expression* içinde `this` kullanımı, derleme zamanı hatasıdır. Özellikle, bir statik yöntemde, statik bir özellik erişimcisinde veya bir alan bildirimi *variable_initializer* içinde `this` ' a başvurmak mümkün değildir.

### <a name="base-access"></a>Temel erişim

Bir *base_access* , bir "`.`" belirteci ve tanımlayıcı ya da köşeli ayraç içine alınmış bir *argument_list* tarafından izlenen `base` ayrılmış sözcükten oluşur:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Geçerli sınıf veya yapıda benzer adlandırılmış Üyeler tarafından gizlenen temel sınıf üyelerine erişmek için bir *base_access* kullanılır. Bir *base_access* yalnızca bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcinin *bloğunda* izin verilir. Bir sınıf veya yapıda `base.I` olduğunda, `I`, bu sınıfın veya yapının temel sınıfının bir üyesini belirtmelidir. Benzer şekilde, bir sınıfta `base[E]` olduğunda, temel sınıfta ilgili bir Dizin Oluşturucu bulunmalıdır.

Bağlama sırasında, `base.I` ve `base[E]` ' nin *base_access* ifadeleri tam olarak `((B)this).I` ve `((B)this)[E]` yazılmış gibi değerlendirilir; burada `B`, yapının gerçekleştiği sınıfın veya yapının temel sınıfıdır. Bu nedenle, `base.I` ve `base[E]` `this.I` ve `this[E]` ' e karşılık gelir, `this` hariç, temel sınıfın bir örneği olarak görüntülenir.

Bir *base_access* , bir sanal işlev üyesine (bir yöntem, özellik veya Dizin Oluşturucu) başvurduğunda, çalışma zamanında hangi işlev üyesinin çalıştırılacağını belirleme ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) değiştirilir. Çağrılan işlev üyesi, işlev üyesinin, `B` '[e (@no__t](classes.md#virtual-methods)-2 ' nin çalışma zamanı türüne göre değil, temel olmayan bir erişime göre her zamanki gibi) eklenerek belirlenir. . Bu nedenle, bir `virtual` işlev üyesinin `override` ' ın içinde, işlev üyesinin devralınmış uygulamasını çağırmak için bir *base_access* kullanılabilir. Bir *base_access* tarafından başvurulan işlev üyesi Özet ise, bir bağlama zamanı hatası oluşur.

### <a name="postfix-increment-and-decrement-operators"></a>Sonek artırma ve azaltma işleçleri

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

Bir sonek artışı veya azaltma işleminin işleneni, değişken olarak sınıflandırılmış bir ifade, özellik erişimi veya Dizin Oluşturucu erişimi olmalıdır. İşlemin sonucu, işlenenden aynı türde bir değerdir.

*Primary_expression* , derleme zamanı türü `dynamic` ise, işleç dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)), *post_increment_expression* veya *post_decrement_expression* derleme zamanı türüne sahiptir `dynamic` Aşağıdaki kurallar, *primary_expression*çalışma zamanı türü kullanılarak çalışma zamanında uygulanır.

Bir sonek artışı veya azaltma işleminin işleneni bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun hem `get` hem de bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Tekil operatör aşırı yükleme çözümü ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. Önceden tanımlı `++` ve `--` işleçleri şu türler için mevcuttur: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 ve herhangi bir numaralandırma türü. Önceden tanımlı `++` işleçleri, işlenene 1 eklenerek üretilen değeri döndürür ve önceden tanımlanmış `--` işleçleri işleneni 1 çıkararak üretilen değeri döndürür. @No__t-0 bağlamında, bu ekleme veya çıkarma sonucu sonuç türü aralığının dışındaysa ve sonuç türü bir tam sayı türü veya Enum türü ise, bir `System.OverflowException` atılır.

@No__t-0 veya `x--` biçiminde bir sonek artışı veya azaltma işleminin çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*   @No__t-0 bir değişken olarak sınıflandırıldıysanız:
    * `x` değişkeni üretmek için değerlendirilir.
    * @No__t-0 değeri kaydedilir.
    * Seçili operatör, bağımsız değişkeni olarak `x` olan kaydedilmiş değeriyle çağrılır.
    * İşleci tarafından döndürülen değer, `x` değerlendirmesi tarafından verilen konumda depolanır.
    * @No__t-0 ' ın kaydedilmiş değeri işlemin sonucu olur.
*   @No__t-0 bir özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldığında:
    * Örnek ifadesi (`x` `static` değilse) ve bağımsız değişken listesi (`x` ' in bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `get` ve `set` erişimci etkinleştirmeleri içinde kullanılır.
    * @No__t-1 ' in `get` erişimcisi çağrılır ve döndürülen değer kaydedilir.
    * Seçili operatör, bağımsız değişkeni olarak `x` olan kaydedilmiş değeriyle çağrılır.
    * @No__t-1 ' in `set` erişimcisi, `value` bağımsız değişkeni olarak işleç tarafından döndürülen değerle çağrılır.
    * @No__t-0 ' ın kaydedilmiş değeri işlemin sonucu olur.

@No__t-0 ve `--` işleçleri önek gösterimini de destekler ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)). Genellikle, `x++` veya `x--` sonucu işlemden önce `x` değeridir, ancak `++x` veya `--x` sonucu işlemden sonra `x` değeri olur. Her iki durumda da, `x` ' ın kendisi işlemden sonra aynı değere sahiptir.

@No__t-0 veya `operator --` bir uygulama, sonek veya ön ek gösterimi kullanılarak çağrılabilir. İki gösterimler için ayrı işleç uygulamalarına sahip olmak mümkün değildir.

### <a name="the-new-operator"></a>New işleci

@No__t-0 işleci, yeni tür örnekleri oluşturmak için kullanılır.

@No__t-0 ifadesinin üç biçimi vardır:

*  Nesne oluşturma ifadeleri, sınıf türlerinin ve değer türlerinin yeni örneklerini oluşturmak için kullanılır.
*  Dizi oluşturma ifadeleri dizi türlerinin yeni örneklerini oluşturmak için kullanılır.
*  Temsilci oluşturma ifadeleri temsilci türlerinin yeni örneklerini oluşturmak için kullanılır.

@No__t-0 işleci bir tür örneğinin oluşturulmasını, ancak dinamik bellek ayırmayı göstermez. Özellikle, değer türlerinin örnekleri, bulundukları değişkenlerin ötesinde ek bellek gerektirmez ve değer türlerinin örneklerini oluşturmak için `new` kullanıldığında dinamik ayırmalar gerçekleşmez.

#### <a name="object-creation-expressions"></a>Nesne oluşturma ifadeleri

Bir *object_creation_expression* , *class_type* veya *value_type*yeni bir örneğini oluşturmak için kullanılır.

```antlr
object_creation_expression
    : 'new' type '(' argument_list? ')' object_or_collection_initializer?
    | 'new' type object_or_collection_initializer
    ;

object_or_collection_initializer
    : object_initializer
    | collection_initializer
    ;
```

Bir *object_creation_expression* *türü* *class_type*, *value_type* veya *type_parameter*olmalıdır. *Tür* `abstract` *class_type*olamaz.

İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yalnızca *tür* bir *class_type* veya *struct_type*ise izin verilir.

Nesne oluşturma ifadesi, Oluşturucu bağımsız değişken listesini atlayabilir ve kapsayan parantez, bir nesne Başlatıcısı veya koleksiyon başlatıcısı içerir. Oluşturucu bağımsız değişken listesini atlama ve kapsayan parantezler boş bir bağımsız değişken listesi belirtmeye eşdeğerdir.

Bir nesne Başlatıcısı veya koleksiyon başlatıcısı içeren bir nesne oluşturma ifadesinin işlenmesi, ilk örnek oluşturucuyu işlemeyi ve sonra nesne Başlatıcısı tarafından belirtilen üye veya öğe başlatmaları işlemeyi içerir ([ Nesne başlatıcıları](expressions.md#object-initializers)) veya koleksiyon başlatıcısı ([koleksiyon başlatıcıları](expressions.md#collection-initializers)).

İsteğe bağlı *argument_list* ' deki bağımsız değişkenlerden herhangi biri `dynamic` ' in derleme zamanı türüne sahipse, *object_creation_expression* dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)) ve çalışma zamanında aşağıdaki kurallar çalışma zamanında uygulanır derleme zamanı türü `dynamic` olan *argument_list* bağımsız değişkenlerinin türü. Ancak, nesne oluşturma, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.

@No__t-1, `T` ' nin bir *class_type* ya da *value_type* olduğu ve `A` ' in isteğe bağlı bir *argument_list*olduğu, bu biçimdeki bir *object_creation_expression* bağlama zamanı işleme, aşağıdaki adımlardan oluşur:

*   @No__t-0 bir *value_type* ve `A` yoksa:
    * *Object_creation_expression* , varsayılan bir Oluşturucu çağrıdır. *Object_creation_expression* sonucu, `T` türünde bir değerdir, yani [System. ValueType türünde](types.md#the-systemvaluetype-type)tanımlanan `T` için varsayılan değerdir.
*   Aksi takdirde, `T` bir *type_parameter* ve `A` yoksa:
    * @No__t-1 için hiçbir değer türü kısıtlaması veya Oluşturucu kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) belirtilmemişse, bir bağlama zamanı hatası oluşur.
    * *Object_creation_expression* sonucu, tür parametresinin bağlandığı çalışma zamanı türünün bir değeridir, yani bu türün varsayılan yapıcısını çağırma sonucu. Çalışma zamanı türü bir başvuru türü veya değer türü olabilir.
*   Aksi takdirde, `T` bir *class_type* veya *struct_type*ise:
    * @No__t-0 bir `abstract` *class_type*ise, derleme zamanı hatası oluşur.
    * Çağrılacak örnek Oluşturucu [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak belirlenir. Aday örnek oluşturucular kümesi, `T` ' da belirtilen tüm erişilebilir örnek oluşturuculardan oluşur. Bu, `A` ' e ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre geçerlidir. Aday örnek oluşturucular kümesi boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir bağlama zamanı hatası oluşur.
    * *Object_creation_expression* sonucu, yukarıdaki adımda belirlenen örnek oluşturucuyu çağırarak üretilen değer olan `T` türünde bir değerdir.
*  Aksi takdirde, *object_creation_expression* geçersizdir ve bir bağlama zamanı hatası oluşur.

*Object_creation_expression* dinamik olarak bağlı olsa da, derleme zamanı türü hala `T` ' dir.

@No__t-1 biçiminde bir *object_creation_expression* çalışma zamanı işleme, `T` *class_type* veya bir *struct_type* ve `A` isteğe bağlı bir *argument_list*, aşağıdaki adımlardan oluşur:

*   @No__t-0 ise *class_type*:
    * @No__t-0 sınıfının yeni bir örneği ayrıldı. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa, `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
    * Yeni örneğin tüm alanları varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.
    * Örnek Oluşturucu, işlev üye çağırma kurallarına göre çağrılır ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Yeni ayrılmış örneğe bir başvuru, örnek oluşturucusuna otomatik olarak geçirilir ve örneğe `this` olarak bu Oluşturucu içinden erişilebilir.
*   @No__t-0 ise *struct_type*:
    * @No__t-0 türünde bir örnek, geçici bir yerel değişken ayırarak oluşturulur. Bir *struct_type* öğesinin örnek Oluşturucusu, oluşturulan örneğin her bir alanına kesinlikle değer atamak gerektiğinden, geçici değişkenin başlatılması gerekmez.
    * Örnek Oluşturucu, işlev üye çağırma kurallarına göre çağrılır ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Yeni ayrılmış örneğe bir başvuru, örnek oluşturucusuna otomatik olarak geçirilir ve örneğe `this` olarak bu Oluşturucu içinden erişilebilir.

#### <a name="object-initializers"></a>Nesne başlatıcıları

Bir ***nesne Başlatıcısı*** , bir nesnenin sıfır veya daha fazla alanı, özelliği veya dizinli öğeleri için değerler belirtir.

```antlr
object_initializer
    : '{' member_initializer_list? '}'
    | '{' member_initializer_list ',' '}'
    ;

member_initializer_list
    : member_initializer (',' member_initializer)*
    ;

member_initializer
    : initializer_target '=' initializer_value
    ;

initializer_target
    : identifier
    | '[' argument_list ']'
    ;

initializer_value
    : expression
    | object_or_collection_initializer
    ;
```

Bir nesne Başlatıcısı, `{` ve `}` belirteçlerinin içine alınmış ve virgüllerle ayrılmış bir dizi üye başlatıcıdan oluşur. Her *member_initializer* , başlatma için bir hedef belirler. Bir *tanımlayıcı* , başlatılan nesnenin erişilebilir bir alanı veya özelliğini sağlamalıdır, ancak köşeli ayraç içine alınmış bir *argument_list* , başlatılmış nesne üzerindeki erişilebilir bir Dizin Oluşturucu için bağımsız değişkenler belirtmelidir. Bir nesne başlatıcısının aynı alan veya özellik için birden fazla üye başlatıcısı içermesi hatadır.

Her *initializer_target* ardından bir eşittir işareti ve bir ifade, bir nesne Başlatıcısı veya bir koleksiyon başlatıcısı gelir. Nesne Başlatıcısı içindeki ifadeler, başlatılan yeni oluşturulan nesneye başvuracaktır.

Eşittir işaretinden sonra bir ifade belirten bir üye başlatıcısı, hedefle ([basit atama](expressions.md#simple-assignment)) aynı şekilde işlenir.

Eşittir işaretinden sonra bir nesne Başlatıcısı belirten bir üye başlatıcısı, ***iç içe geçmiş nesne Başlatıcısı***, yani gömülü bir nesnenin bir başlatması. Alan veya özelliğe yeni bir değer atamak yerine, iç içe nesne başlatıcısındaki atamalar, alan veya özellik üyelerine atamalar olarak değerlendirilir. İç içe nesne başlatıcıları bir değer türü olan özelliklere veya bir değer türüne sahip salt okuma alanlarına uygulanamaz.

Eşittir işaretinden sonra bir koleksiyon başlatıcısı belirten üye başlatıcısı, gömülü bir koleksiyonun başlatılmasından. Hedef alana, özelliğe veya dizin oluşturucuya yeni bir koleksiyon atamak yerine, başlatıcıda verilen öğeler hedef tarafından başvurulan koleksiyona eklenir. Hedef, [koleksiyon başlatıcıları](expressions.md#collection-initializers)'nda belirtilen gereksinimleri karşılayan bir koleksiyon türünde olmalıdır.

Bir dizin başlatıcısının bağımsız değişkenleri her zaman tam olarak bir kez değerlendirilir. Bu nedenle, bağımsız değişkenler (örneğin, boş bir iç içe bir başlatıcı nedeniyle) hiç kullanılmasa bile, bunların yan etkileri için değerlendirilirler.

Aşağıdaki sınıf iki koordinat içeren bir noktayı temsil eder:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

@No__t-0 ' ın bir örneği aşağıdaki şekilde oluşturulup başlatılabilir:
```csharp
Point a = new Point { X = 0, Y = 1 };
```
aynı etkiye sahip
```csharp
Point __a = new Point();
__a.X = 0;
__a.Y = 1; 
Point a = __a;
```
Burada `__a`, aksi durumda görünmeyen ve erişilemeyen geçici bir değişkendir. Aşağıdaki sınıf, iki noktadan oluşturulan bir dikdörtgeni temsil eder:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

@No__t-0 ' ın bir örneği aşağıdaki şekilde oluşturulup başlatılabilir:
```csharp
Rectangle r = new Rectangle {
    P1 = new Point { X = 0, Y = 1 },
    P2 = new Point { X = 2, Y = 3 }
};
```
aynı etkiye sahip
```csharp
Rectangle __r = new Rectangle();
Point __p1 = new Point();
__p1.X = 0;
__p1.Y = 1;
__r.P1 = __p1;
Point __p2 = new Point();
__p2.X = 2;
__p2.Y = 3;
__r.P2 = __p2; 
Rectangle r = __r;
```
`__r`, `__p1` ve `__p2`, aksi durumda görünmeyen ve erişilemeyen geçici değişkenlerdir.

@No__t-0 ' ın Oluşturucusu iki Embedded `Point` örneğini ayırırsa
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Aşağıdaki yapı, yeni örnekler atamak yerine gömülü `Point` örneklerini başlatmak için kullanılabilir:
```csharp
Rectangle r = new Rectangle {
    P1 = { X = 0, Y = 1 },
    P2 = { X = 2, Y = 3 }
};
```
aynı etkiye sahip
```csharp
Rectangle __r = new Rectangle();
__r.P1.X = 0;
__r.P1.Y = 1;
__r.P2.X = 2;
__r.P2.Y = 3;
Rectangle r = __r;
```

C 'nin uygun bir tanımı verildiğinde aşağıdaki örnek:
```csharp
var c = new C {
    x = true,
    y = { a = "Hello" },
    z = { 1, 2, 3 },
    ["x"] = 5,
    [0,0] = { "a", "b" },
    [1,2] = {}
};
```
Bu atama dizisine eşdeğerdir:
```csharp
C __c = new C();
__c.x = true;
__c.y.a = "Hello";
__c.z.Add(1); 
__c.z.Add(2);
__c.z.Add(3);
string __i1 = "x";
__c[__i1] = 5;
int __i2 = 0, __i3 = 0;
__c[__i2,__i3].Add("a");
__c[__i2,__i3].Add("b");
int __i4 = 1, __i5 = 2;
var c = __c;
```
Burada `__c`, vb., görünmeyen ve kaynak kodla erişilemeyen değişkenler tarafından oluşturulmuştur. @No__t-0 için bağımsız değişkenlerin yalnızca bir kez değerlendirileceğini ve `[1,2]` için bağımsız değişkenlerin, hiç kullanılmasa bile bir kez değerlendirildiğini unutmayın.

#### <a name="collection-initializers"></a>Koleksiyon başlatıcıları

Koleksiyon Başlatıcısı, bir koleksiyonun öğelerini belirtir.

```antlr
collection_initializer
    : '{' element_initializer_list '}'
    | '{' element_initializer_list ',' '}'
    ;

element_initializer_list
    : element_initializer (',' element_initializer)*
    ;

element_initializer
    : non_assignment_expression
    | '{' expression_list '}'
    ;

expression_list
    : expression (',' expression)*
    ;
```

Bir koleksiyon başlatıcısı, `{` ve `}` belirteçleri içine alınmış ve virgüllerle ayrılmış bir dizi öğe başlatıcısından oluşur. Her öğe başlatıcısı, başlatılmakta olan koleksiyon nesnesine eklenecek bir öğe belirtir ve `{` ve `}` belirteçleri içeren ve virgüllerle ayrılmış bir ifade listesinden oluşur.  Tek ifadeyle bir öğe başlatıcısı küme ayracı olmadan yazılabilir, ancak üye başlatıcılarla belirsizliğe engel olmak için atama ifadesi olamaz. *Non_assignment_expression* üretimi [ifadede](expressions.md#expression)tanımlanmıştır.

Aşağıda, bir koleksiyon başlatıcısı içeren bir nesne oluşturma ifadesine örnek verilmiştir:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Koleksiyon başlatıcısının uygulandığı koleksiyon nesnesi `System.Collections.IEnumerable` uygulayan bir türde olmalıdır veya bir derleme zamanı hatası oluşur. Koleksiyon Başlatıcısı sırasıyla belirtilen her öğe için, hedef nesnede bir `Add` yöntemini, bağımsız değişken listesi olarak öğe başlatıcısının ifade listesini çağırır, her çağrı için normal üye arama ve aşırı yükleme çözümlemesi uygulama. Bu nedenle, koleksiyon nesnesinin her öğe başlatıcısı için `Add` adına sahip uygulanabilir bir örnek veya genişletme yöntemi olmalıdır.

Aşağıdaki sınıf, bir kişinin adını ve telefon numaralarının listesini temsil eder:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

@No__t-0, aşağıdaki gibi oluşturulabilir ve başlatılabilir:
```csharp
var contacts = new List<Contact> {
    new Contact {
        Name = "Chris Smith",
        PhoneNumbers = { "206-555-0101", "425-882-8080" }
    },
    new Contact {
        Name = "Bob Harris",
        PhoneNumbers = { "650-555-0199" }
    }
};
```
aynı etkiye sahip
```csharp
var __clist = new List<Contact>();
Contact __c1 = new Contact();
__c1.Name = "Chris Smith";
__c1.PhoneNumbers.Add("206-555-0101");
__c1.PhoneNumbers.Add("425-882-8080");
__clist.Add(__c1);
Contact __c2 = new Contact();
__c2.Name = "Bob Harris";
__c2.PhoneNumbers.Add("650-555-0199");
__clist.Add(__c2);
var contacts = __clist;
```
`__clist`, `__c1` ve `__c2`, aksi durumda görünmeyen ve erişilemeyen geçici değişkenlerdir.

#### <a name="array-creation-expressions"></a>Dizi oluşturma ifadeleri

Bir *array_type*yeni bir örneğini oluşturmak için bir *array_creation_expression* kullanılır.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

İlk formun dizi oluşturma ifadesi, bağımsız ifadelerin her birini ifade listesinden silmenin sonucu olan türün bir dizi örneğini ayırır. Örneğin, `new int[10,20]` dizi oluşturma ifadesi `int[,]` türünde bir dizi örneği oluşturur ve `new int[10][,]` dizi oluşturma ifadesi `int[][,]` türünde bir dizi oluşturur. İfade listesindeki her bir ifadenin `int`, `uint`, `long` veya `ulong` türünde olması veya örtülü olarak bu türlerden bir veya daha fazlasına dönüştürülebilir olması gerekir. Her ifadenin değeri, yeni ayrılan dizi örneğindeki karşılık gelen boyutun uzunluğunu belirler. Bir dizi boyutunun uzunluğunun negatif olması gerektiğinden, ifade listesinde negatif bir değer olan bir *constant_expression* sahip olmak için bir derleme zamanı hatası olur.

Güvenli olmayan bir bağlam ([güvenli olmayan bağlamların](unsafe-code.md#unsafe-contexts)) dışında, dizilerin düzeni belirtilmemiş olur.

İlk formun dizi oluşturma ifadesi bir dizi başlatıcısı içeriyorsa, ifade listesindeki her bir ifade bir sabit olmalıdır ve ifade listesi tarafından belirtilen sıralama ve boyut uzunlukları dizi başlatıcısının ile aynı olmalıdır.

İkinci veya üçüncü formun dizi oluşturma ifadesinde, belirtilen dizi türü veya sıralama belirticisinin sıralaması dizi başlatıcısının ile aynı olmalıdır. Tek tek boyut uzunlukları, dizi başlatıcısının karşılık gelen iç içe geçme düzeylerindeki öğelerin sayısından algılanır. Bu nedenle, ifadesi
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
tam olarak karşılık gelen
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Üçüncü formun dizi oluşturma ifadesi, ***örtük olarak yazılmış dizi oluşturma ifadesi***olarak adlandırılır. Dizinin öğe türü açıkça verilmemiştir, ancak dizi başlatıcısındaki ifadelerin kümesinin en iyi ortak türü ([bir ifade kümesinin en iyi ortak türünü bulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) olarak belirlenir, ikinci forma benzerdir. Çok boyutlu bir dizi için, örneğin, *rank_specifier* en az bir virgül içerdiğinde, bu küme iç içe yerleştirilmiş *array_initializer*s 'de bulunan tüm *ifade*öğeleri içerir.

Dizi başlatıcıları, [dizi başlatıcılarda](arrays.md#array-initializers)daha ayrıntılı olarak açıklanmıştır.

Dizi oluşturma ifadesinin değerlendirilme sonucu bir değer olarak sınıflandırıldığından, yani yeni ayrılmış dizi örneğine bir başvurudur. Bir dizi oluşturma ifadesinin çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*  *Expression_list* boyut uzunluğu ifadeleri, soldan sağa doğru sırayla değerlendirilir. Her bir ifadenin aşağıdaki türlerden birine yönelik olarak örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilmesi, `int`, `uint`, `long`, `ulong` ' tür. Bu listede örtük bir dönüştürmenin bulunduğu ilk tür seçilir. Bir ifade veya sonraki örtük dönüştürme değerlendirmesi bir özel duruma neden oluyorsa, başka ifadeler değerlendirilmez ve başka bir adım yürütülmez.
*  Boyut uzunlukları için hesaplanan değerler aşağıdaki şekilde onaylanır. Bir veya daha fazla değer sıfırdan küçükse, `System.OverflowException` oluşturulur ve başka adımlar yürütülmez.
*  Verilen boyut uzunluklarına sahip bir dizi örneği ayrıldı. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa, `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
*  Yeni dizi örneğinin tüm öğeleri varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.
*  Dizi oluşturma ifadesi bir dizi başlatıcısı içeriyorsa, dizi başlatıcısındaki her bir ifade değerlendirilir ve karşılık gelen dizi öğesine atanır. Değerlendirmeler ve Atamalar, ifadelerin dizi başlatıcıda yazıldığı sırada gerçekleştirilir — diğer bir deyişle, öğeler, en sağdaki boyut ilk artarak Dizin sırasında başlatılır. Belirli bir ifadenin değerlendirilmesi veya karşılık gelen dizi öğesine sonraki atama bir özel duruma neden oluyorsa, daha fazla öğe başlatılmaz (ve kalan öğelerin varsayılan değerleri olur).

Dizi oluşturma ifadesi bir dizi türünün öğeleriyle bir dizinin örneklemesine izin verir, ancak bu tür bir dizinin öğeleri el ile başlatılmış olmalıdır. Örneğin, ekstresi
```csharp
int[][] a = new int[100][];
```
`int[]` türünde 100 öğe içeren tek boyutlu bir dizi oluşturur. Her öğenin başlangıç değeri `null` ' dır. Aynı dizi oluşturma ifadesinin alt dizileri ve deyimi de örneği oluşturması mümkün değildir
```csharp
int[][] a = new int[100][5];        // Error
```
derleme zamanı hatasına neden olur. Alt dizilerin örneklenmesi bunun yerine el ile gerçekleştirilmelidir, örneğin
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Dizi dizisinde "dikdörtgen" bir şekil varsa, bu, alt diziler aynı uzunluktadır ve çok boyutlu bir dizi kullanmak daha verimlidir. Yukarıdaki örnekte, dizi dizisinin örneklenmesi 101 nesne oluşturuyor — bir dış dizi ve 100 alt dizileri. Buna karşılık,
```csharp
int[,] = new int[100, 5];
```
yalnızca tek bir nesne, iki boyutlu bir dizi oluşturur ve ayırmayı tek bir ifadede gerçekleştirir.

Aşağıda örtük olarak yazılmış dizi oluşturma ifadelerinin örnekleri verilmiştir:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Son ifade, `int` veya `string` örtülü olarak birbirlerine dönüştürülebilir olduğundan ve en iyi ortak tür olmadığından, derleme zamanı hatasına neden olur. Açıkça yazılmış bir dizi oluşturma ifadesinin bu durumda kullanılması gerekir, örneğin `object[]` olacak tür belirtin. Alternatif olarak, öğelerinden biri ortak bir temel türe alınabilir, bu da çıkarılan öğe türü olur.

Anonim olarak belirlenmiş veri yapıları oluşturmak için örtük olarak yazılmış dizi oluşturma ifadeleri anonim nesne başlatıcıları ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) ile birleştirilebilir. Örneğin:
```csharp
var contacts = new[] {
    new {
        Name = "Chris Smith",
        PhoneNumbers = new[] { "206-555-0101", "425-882-8080" }
    },
    new {
        Name = "Bob Harris",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

#### <a name="delegate-creation-expressions"></a>Temsilci oluşturma ifadeleri

Bir *delegate_type*yeni bir örneğini oluşturmak için bir *delegate_creation_expression* kullanılır.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Bir temsilci oluşturma ifadesinin bağımsız değişkeni bir yöntem grubu, anonim bir işlev veya `dynamic` ya da bir *delegate_type*derleme zamanı türünde bir değer olmalıdır. Bağımsız değişken bir yöntem grubu ise, yöntemini ve bir örnek yöntemi için, bir temsilci oluşturulacak nesneyi tanımlar. Bağımsız değişken anonim bir işlevse, temsilci hedefinin parametrelerini ve Yöntem gövdesini doğrudan tanımlar. Bağımsız değişken bir değer ise, bir kopyasını oluşturmak için bir temsilci örneğini tanımlar.

*İfadede* derleme zamanı türü `dynamic` varsa, *delegate_creation_expression* dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kurallar, *ifadenin*çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. Aksi takdirde kurallar derleme zamanında uygulanır.

@No__t-1 biçiminde bir *delegate_creation_expression* bağlama zamanı işleme, `D` bir *delegate_type* ve `E` bir *ifadedir*, aşağıdaki adımlardan oluşur:

*  @No__t-0 bir yöntem grubu ise, temsilci oluşturma ifadesi, bir yöntem grubu dönüştürmesi ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) ile `E` ' den `D` ' e kadar aynı şekilde işlenir.
*  @No__t-0 Anonim bir işlevse, temsilci oluşturma ifadesi, anonim işlev dönüştürmesinde ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)), `E` ' ye `D` ' e kadar aynı şekilde işlenir.
*  @No__t-0 bir değer ise, `E` ' i `D` ile uyumlu olmalıdır ([temsilci bildirimleri](delegates.md#delegate-declarations)) ve sonuç, `E` ile aynı çağırma listesine başvuran `D` türünde yeni oluşturulmuş bir temsilciye başvurudur. @No__t-0 `D` ile uyumlu değilse, bir derleme zamanı hatası oluşur.

@No__t-1 biçiminde bir *delegate_creation_expression* çalışma zamanı işleme, `D` bir *delegate_type* ve `E` bir *ifadedir*, aşağıdaki adımlardan oluşur:

*   @No__t-0 bir yöntem grubu ise, temsilci oluşturma ifadesi, `E` ' den `D` ' e bir yöntem grubu dönüştürmesi ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) olarak değerlendirilir.
*   @No__t-0 Anonim bir işlevse, temsilci oluşturma, `E` ' den `D` ' ye ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)) anonim işlev dönüştürmesi olarak değerlendirilir.
*   @No__t-0 bir *delegate_type*değeridir:
    * `E` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
    * @No__t-0 değeri `null` ise, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
    * @No__t-0 temsilci türünün yeni bir örneği ayrıldı. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa, `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
    * Yeni temsilci örneği, `E` tarafından verilen temsilci örneğiyle aynı çağırma listesiyle başlatılır.

Temsilcinin çağırma listesi, temsilcinin örneklendiği zaman belirlenir ve sonra temsilcinin tüm ömrü boyunca sabit kalır. Diğer bir deyişle, oluşturulduktan sonra bir temsilcinin hedef çağrılabilir varlıklarını değiştirmek mümkün değildir. İki temsilci birleştirildiğinde veya bir diğeri ([temsilci bildirimlerinden](delegates.md#delegate-declarations)) kaldırıldığında, yeni bir temsilci oluşur; Mevcut temsilcinin içeriği değiştirilmedi.

Bir özellik, Dizin Oluşturucu, Kullanıcı tanımlı işleç, örnek Oluşturucu, yıkıcı veya statik oluşturucuya başvuran bir temsilci oluşturmak mümkün değildir.

Yukarıda açıklandığı gibi, bir yöntem grubundan bir temsilci oluşturulduğunda, temsilci 'nin biçimsel parametre listesi ve dönüş türü, ne tür aşırı yüklenmiş yöntemlerin ekleneceğini belirleyin. Örnekte
```csharp
delegate double DoubleFunc(double x);

class A
{
    DoubleFunc f = new DoubleFunc(Square);

    static float Square(float x) {
        return x * x;
    }

    static double Square(double x) {
        return x * x;
    }
}
```
`A.f` alanı, ikinci `Square` yöntemine başvuran bir temsilciyle başlatılır çünkü bu yöntem, biçimsel parametre listesi ve dönüş türü `DoubleFunc` ' dir. İkinci `Square` metodu yoktu, derleme zamanı hatası oluşmuş olabilir.

#### <a name="anonymous-object-creation-expressions"></a>Anonim nesne oluşturma ifadeleri

Anonim türdeki bir nesne oluşturmak için bir *anonymous_object_creation_expression* kullanılır.

```antlr
anonymous_object_creation_expression
    : 'new' anonymous_object_initializer
    ;

anonymous_object_initializer
    : '{' member_declarator_list? '}'
    | '{' member_declarator_list ',' '}'
    ;

member_declarator_list
    : member_declarator (',' member_declarator)*
    ;

member_declarator
    : simple_name
    | member_access
    | base_access
    | null_conditional_member_access
    | identifier '=' expression
    ;
```

Anonim bir nesne Başlatıcısı, anonim bir tür bildirir ve bu türün bir örneğini döndürür. Anonim tür, doğrudan `object` ' dan devralan isimsiz bir sınıf türüdür. Anonim bir türün üyeleri, türünün bir örneğini oluşturmak için kullanılan anonim nesne başlatıcıdan çıkarılan salt okunurdur salt yazılır özelliklerden oluşan bir dizidir. Özellikle, formun anonim nesne Başlatıcısı
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
formun anonim türünü bildirir
```csharp
class __Anonymous1
{
    private readonly T1 f1;
    private readonly T2 f2;
    ...
    private readonly Tn fn;

    public __Anonymous1(T1 a1, T2 a2, ..., Tn an) {
        f1 = a1;
        f2 = a2;
        ...
        fn = an;
    }

    public T1 p1 { get { return f1; } }
    public T2 p2 { get { return f2; } }
    ...
    public Tn pn { get { return fn; } }

    public override bool Equals(object __o) { ... }
    public override int GetHashCode() { ... }
}
```
Her `Tx`, karşılık gelen `ex` ifadesinin türüdür. Bir *member_declarator* içinde kullanılan ifadenin türü olmalıdır. Bu nedenle, *member_declarator* içindeki bir ifadenin null veya anonim bir işlev olması için derleme zamanı hatası vardır. Ayrıca, ifadenin güvenli olmayan bir türü olması için derleme zamanı hatası da vardır.

Anonim bir türün ve parametresinin `Equals` yöntemine ait adları otomatik olarak derleyici tarafından oluşturulur ve program metninde başvurulamaz.

Aynı programda aynı ada ve derleme zamanı türlerine sahip bir özellikler dizisini belirten iki anonim nesne başlatıcıları aynı şekilde aynı anonim türde örnekler oluşturacaktır.

Örnekte
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
`p1` ve `p2` aynı anonim türde olduğundan, son satırdaki atamaya izin verilir.

Anonim türlerdeki `Equals` ve `GetHashcode` yöntemleri `object` ' den devralınan yöntemleri geçersiz kılar ve özelliklerin `Equals` ve `GetHashcode` ' de tanımlanır. böylece, aynı anonim türdeki iki örnek yalnızca tüm özellikleri sıfıra.

Bir üye bildirimci basit bir ada ([tür çıkarımı](expressions.md#type-inference)), bir üye erişimini ([dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), temel erişim ([temel erişim](expressions.md#base-access)) veya null koşullu üye erişimini kısaltılabilir ([ Yansıtma başlatıcıları olarak null Koşullu ifadeler](expressions.md#null-conditional-expressions-as-projection-initializers)). Bu, ***projeksiyon başlatıcısı*** olarak adlandırılır ve aynı ada sahip bir özelliğe bir bildirim ve atama için toplu bir özelliktir. Özellikle, formların üye bildirimcileri
```csharp
identifier
expr.identifier
```
sırasıyla aşağıdaki gibi tam olarak eşdeğerdir:
```csharp
identifier = identifier
identifier = expr.identifier
```

Bu nedenle, bir projeksiyon başlatıcısında *tanımlayıcı* hem değeri hem de değerin atandığı alanı veya özelliği seçer. Bir projeksiyon başlatıcısı projesi yalnızca bir değer değil, aynı zamanda değerin adı değildir.

### <a name="the-typeof-operator"></a>Typeof işleci

@No__t-0 işleci, bir tür için `System.Type` nesnesini elde etmek için kullanılır.

```antlr
typeof_expression
    : 'typeof' '(' type ')'
    | 'typeof' '(' unbound_type_name ')'
    | 'typeof' '(' 'void' ')'
    ;

unbound_type_name
    : identifier generic_dimension_specifier?
    | identifier '::' identifier generic_dimension_specifier?
    | unbound_type_name '.' identifier generic_dimension_specifier?
    ;

generic_dimension_specifier
    : '<' comma* '>'
    ;

comma
    : ','
    ;
```

İlk *typeof_expression* biçimi `typeof` anahtar sözcükten ve sonra parantez içine alınmış bir *tür*ile oluşur. Bu formun bir ifadesinin sonucu, belirtilen tür için `System.Type` nesnesidir. Verilen herhangi bir tür için yalnızca bir `System.Type` nesnesi vardır. Bu, bir @ no__t-0, `typeof(T) == typeof(T)` türü için her zaman doğru olduğu anlamına gelir. *Tür* `dynamic` olamaz.

*Typeof_expression* ikinci biçimi `typeof` anahtar sözcükten ve sonra parantez içine alınmış bir *unbound_type_name*oluşur. Bir *unbound_type_name* *type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)), bir *unbound_type_name* , *type_name içeren* *generic_dimension_specifier* *s içerir. argument_list*s. Bir *typeof_expression* işleneni, hem *unbound_type_name* hem de *type_name*, her ikisi de ne bir *generic_dimension_specifier* ne de bir type_argument içerdiğinde bir belirteç dizisi olduğunda  *_liste*, belirteçlerin sırası bir *type_name*olarak kabul edilir. Bir *unbound_type_name* anlamı aşağıdaki şekilde belirlenir:

*  Her bir *generic_dimension_specifier* ile aynı sayıda virgül içeren bir *type_argument_list* ve her bir *type_argument*için `object` anahtar sözcüğünü değiştirerek, belirteçlerin dizisini bir *type_name* dönüştürün.
*  Tüm tür parametresi kısıtlamalarını yoksayarak elde edilen *type_name*değerlendirin.
*  *Unbound_type_name* , ortaya çıkan oluşturulan türle ilişkili ilişkisiz genel tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) ile çözümlenir.

*Typeof_expression* sonucu, sonuçta elde edilen ilişkisiz genel tür için `System.Type` nesnesidir.

Üçüncü *typeof_expression* biçimi `typeof` anahtar sözcüğünden ve sonra parantez içinde `void` anahtar sözcüğüyle oluşur. Bu formun bir ifadesinin sonucu, bir türün yokluğunu temsil eden `System.Type` nesnesidir. @No__t-0 tarafından döndürülen tür nesnesi herhangi bir tür için döndürülen tür nesnesinden farklıdır. Bu özel tür nesnesi, dildeki yöntemlere izin veren sınıf kitaplıklarında yararlıdır; burada, bu yöntemlerin, `System.Type` örneğiyle void yöntemler de dahil olmak üzere herhangi bir yöntemin dönüş türünü temsil etmek için bir yol olmasını ister.

@No__t-0 işleci bir tür parametresinde kullanılabilir. Sonuç, tür parametresine bağlanan çalışma zamanı türü için `System.Type` nesnesidir. @No__t-0 işleci, oluşturulmuş bir tür veya ilişkisiz genel tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) üzerinde de kullanılabilir. İlişkisiz genel tür için `System.Type` nesnesi, örnek türünün `System.Type` nesnesiyle aynı değildir. Örnek türü her zaman çalışma zamanında bir kapalı oluşturulmuş türdür, `System.Type` nesnesi kullanımda olan çalışma zamanı türü bağımsız değişkenlerine bağlıdır, ancak ilişkisiz genel türün tür bağımsız değişkenleri yoktur.

Örnek
```csharp
using System;

class X<T>
{
    public static void PrintTypes() {
        Type[] t = {
            typeof(int),
            typeof(System.Int32),
            typeof(string),
            typeof(double[]),
            typeof(void),
            typeof(T),
            typeof(X<T>),
            typeof(X<X<T>>),
            typeof(X<>)
        };
        for (int i = 0; i < t.Length; i++) {
            Console.WriteLine(t[i]);
        }
    }
}

class Test
{
    static void Main() {
        X<int>.PrintTypes();
    }
}
```
Aşağıdaki çıktıyı üretir:
```console
System.Int32
System.Int32
System.String
System.Double[]
System.Void
System.Int32
X`1[System.Int32]
X`1[X`1[System.Int32]]
X`1[T]
```

@No__t-0 ve `System.Int32` ' in aynı türde olduğunu unutmayın.

Ayrıca, `typeof(X<>)` ' nın sonucunun tür bağımsız değişkenine bağımlı olmadığına, ancak `typeof(X<T>)` ' in sonucunu unutmayın.

### <a name="the-checked-and-unchecked-operators"></a>Checked ve unchecked işleçleri

@No__t-0 ve `unchecked` işleçleri, tam sayı türü aritmetik işlemler ve dönüştürmeler için ***taşma denetimi bağlamını*** denetlemek üzere kullanılır.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

@No__t-0 işleci içerilen ifadeyi işaretlenmiş bir bağlamda değerlendirir ve `unchecked` işleci, içerilen ifadeyi işaretlenmemiş bir bağlamda değerlendirir. Bir *checked_expression* veya *unchecked_expression* , içerilen ifadenin verilen taşma Denetim bağlamı içinde değerlendirilmesinden bağımsız olarak, bir *parenthesized_expression* ([parantez içine alınmış ifadelerde](expressions.md#parenthesized-expressions)) öğesine karşılık gelir .

Taşma Denetim bağlamı `checked` ve `unchecked` deyimleri ([Checked ve unchecked deyimleri](statements.md#the-checked-and-unchecked-statements)) aracılığıyla da denetlenebilir.

Aşağıdaki işlemler `checked` ve `unchecked` işleçleri ve deyimleri tarafından oluşturulan taşma denetimi bağlamından etkilenir:

*  Ön tanımlı `++` ve `--` Birli İşleçler ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), işlenen bir integral türüdür.
*  Ön tanımlı `-` birli işleç ([birli eksi işleci](expressions.md#unary-minus-operator)), işlenen bir integral türüdür.
*  Ön tanımlı `+`, `-`, `*` ve `/` ikili işleçler ([Aritmetik işleçler](expressions.md#arithmetic-operators)), her iki işlenen de İntegral türleridir.
*  Tek bir integral türünden başka bir integral türüne veya `float` veya `double` ' den integral türüne açık sayısal dönüştürmeler ([Açık sayısal dönüştürmeler](conversions.md#explicit-numeric-conversions)).

Yukarıdaki işlemlerden biri hedef türünde temsil etmek için çok büyük bir sonuç üretdüğünde, işlemin gerçekleştirildiği bağlam, sonuçta elde edilen davranışı denetler:

*  @No__t-0 bağlamında, işlem sabit bir ifadesiyse ([sabit ifadeler](expressions.md#constant-expressions)), bir derleme zamanı hatası oluşur. Aksi takdirde, işlem çalışma zamanında gerçekleştirildiğinde, bir `System.OverflowException` oluşturulur.
*  @No__t-0 bağlamında, sonuç, hedef türüne sığmayan yüksek sıralı bitleri atarak kesilir.

Herhangi bir `checked` veya `unchecked` işleci veya deyimi tarafından kullanılmayan sabit olmayan ifadeler (çalışma zamanında değerlendirilen ifadeler) için @no__t, dış faktörler (örneğin, derleyici anahtarları ve yürütme ortamı yapılandırması) `checked` değerlendirmesi çağrısı.

Sabit ifadeler (derleme zamanında tam olarak değerlendirilebilecek ifadeler) için, varsayılan taşma denetimi bağlamı her zaman `checked` ' dır. Sabit bir ifade `unchecked` bağlamına açık bir şekilde yerleştirilmediği için, ifadenin derleme zamanı değerlendirmesi sırasında oluşan taşmalar her zaman derleme zamanı hatalarına neden olur.

Anonim işlevin gövdesi, anonim işlevin gerçekleştiği `checked` veya `unchecked` bağlamlarından etkilenmez.

Örnekte
```csharp
class Test
{
    static readonly int x = 1000000;
    static readonly int y = 1000000;

    static int F() {
        return checked(x * y);      // Throws OverflowException
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Depends on default
    }
}
```
hiçbir deyimin hiçbiri derleme zamanında değerlendirilemediğinden, derleme zamanı hataları bildirilmemiştir. Çalışma zamanında, `F` yöntemi bir `System.OverflowException` oluşturur ve `G` yöntemi-727379968 döndürür (Aralık dışı sonucun alt 32 bitleri). @No__t-0 yönteminin davranışı, derleme için varsayılan taşma denetimi bağlamına bağlıdır, ancak `F` ile aynı ya da `G` ile aynı olur.

Örnekte
```csharp
class Test
{
    const int x = 1000000;
    const int y = 1000000;

    static int F() {
        return checked(x * y);      // Compile error, overflow
    }

    static int G() {
        return unchecked(x * y);    // Returns -727379968
    }

    static int H() {
        return x * y;               // Compile error, overflow
    }
}
```
`F` ' da sabit ifadeler değerlendirilirken oluşan taşmalar ve `H`, ifadeler bir `checked` bağlamında değerlendirildiğinden derleme zamanı hatalarının raporlanmasına neden olur. @No__t-0 ' da sabit ifade değerlendirilirken bir taşma meydana gelir, ancak değerlendirme bir `unchecked` bağlamında gerçekleşdiğinden taşma bildirilmedi.

@No__t-0 ve `unchecked` işleçleri yalnızca "`(`" ve "`)`" belirteçlerinde bulunan metin içeriğini eklemek bu işlemler için taşma denetimi bağlamını etkiler. İşleçler, içerilen ifadenin hesaplanmasının sonucu olarak çağrılan işlev üyelerini etkilemez. Örnekte
```csharp
class Test
{
    static int Multiply(int x, int y) {
        return x * y;
    }

    static int F() {
        return checked(Multiply(1000000, 1000000));
    }
}
```
`F` ' de `checked` kullanımı, `Multiply` ' teki `x * y` ' nin değerlendirilmesini etkilemez, bu nedenle `x * y` varsayılan taşma denetimi bağlamında değerlendirilir.

@No__t-0 işleci, işaretli integral türlerinin sabitlerini onaltılık gösterimde yazarken kullanışlıdır. Örneğin:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Yukarıdaki onaltılık sabitlerin her ikisi de `uint` türündedir. Sabitler `int` aralığının dışında olduğundan, `unchecked` işleci olmadan `int` ' ye yapılan yayınlar derleme zamanı hataları oluşturur.

@No__t-0 ve `unchecked` işleçleri ve deyimleri, programcıların bazı sayısal hesaplamaların belirli yönlerini denetlemesine olanak tanır. Ancak, bazı sayısal işleçlerin davranışı işlenenlerinin veri türlerine bağlıdır. Örneğin, iki ondalıkın çarpılması her zaman açık bir `unchecked` yapısı içinde bile taşma üzerinde bir özel durumla sonuçlanır. Benzer şekilde, iki kayan noktalı çarpma, açıkça `checked` yapısı içinde bile taşma üzerinde hiçbir şekilde neden olmaz. Ayrıca, diğer işleçler, varsayılan veya açık olsun denetim modundan hiçbir şekilde etkilenmez.

### <a name="default-value-expressions"></a>Varsayılan değer ifadeleri

Varsayılan değer ifadesi bir türün varsayılan değerini ([varsayılan değerler](variables.md#default-values)) almak için kullanılır. Tür parametresi bir değer türü veya bir başvuru türü ise, genellikle varsayılan değer ifadesi tür parametreleri için kullanılır. (Tür parametresi bir başvuru türü olarak bilinmediği takdirde, `null` değişmez değerinde bir tür parametresine dönüştürme yok.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Bir *default_value_expression* içindeki *tür* çalışma zamanında bir başvuru türüne değerlendirilirse, sonuç `null` bu türe dönüştürülür. Bir *default_value_expression* içindeki *tür* çalışma zamanında bir değer türüne değerlendirilirse sonuç, *value_type*'in varsayılan değeridir ([Varsayılan oluşturucular](types.md#default-constructors)).

Tür bir başvuru türü veya başvuru türü olarak bilinen bir tür parametresi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ise, bir *default_value_expression* sabit ifadedir ([sabit ifadeler](expressions.md#constant-expressions)). Ayrıca, tür şu değer türlerinden biri ise, *default_value_expression* sabit bir ifadedir: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3 ya da herhangi bir numaralandırma türü.


### <a name="nameof-expressions"></a>NameOf ifadeleri

Bir *nameof_expression* , bir program varlığının adını sabit bir dize olarak almak için kullanılır.

```antlr
nameof_expression
    : 'nameof' '(' named_entity ')'
    ;

named_entity
    : simple_name
    | named_entity_target '.' identifier type_argument_list?
    ;

named_entity_target
    : 'this'
    | 'base'
    | named_entity 
    | predefined_type 
    | qualified_alias_member
    ;
```

Dilbilgisi, *named_entity* işleneni her zaman bir ifadedir. @No__t-0, ayrılmış bir anahtar sözcük olmadığından, bir NameOf ifadesi her zaman sözdizimsel olarak basit ad olan `nameof` ' i çağırmayla belirsizdir. Uyumluluk nedenleriyle `nameof` adının bir ad araması ([basit adlar](expressions.md#simple-names)) başarılı olursa, çağrının yasal olup olmamasına bakılmaksızın ifade *invocation_expression* olarak değerlendirilir. Aksi halde, bir *nameof_expression*.

Bir *nameof_expression* 'ın *named_entity* anlamı, ifadenin anlamı olarak ifade edilir; Yani, bir *simple_name*, *base_access* ya da *member_access*olarak. Ancak, [basit adlarda](expressions.md#simple-names) ve [üye erişimlerinde](expressions.md#member-access) açıklanan aramanın bir hata ile sonuçlandığı, bir örnek üyesi statik bir bağlamda bulunduğu için, bir *nameof_expression* böyle bir hata üretir.

Bir yöntem grubunu bir *type_argument_list*sahip olacak şekilde belirlemek bir *named_entity* için derleme zamanı hatasıdır. Bir *named_entity_target* `dynamic` türünde olması için derleme zamanı hatasıdır.

*Nameof_expression* , `string` türünde sabit bir ifadedir ve çalışma zamanında hiçbir etkiye sahip değildir. Özellikle, *named_entity* değerlendirilmez ve kesin atama analizinin ([basit ifadeler için genel kurallar](variables.md#general-rules-for-simple-expressions)) amaçları doğrultusunda yok sayılır. Değeri, isteğe bağlı son *type_argument_list*öncesindeki *named_entity* 'in son tanımlayıcısıdır ve aşağıdaki şekilde dönüştürülür:

* "`@`" Kullanılırsa, ' "öneki kaldırılır.
* Her *unicode_escape_sequence* karşılık gelen Unicode karakteriyle dönüştürülür.
* Herhangi bir *formatting_characters* kaldırılır.

Bunlar, tanımlayıcılar arasında eşitlik test edilirken [tanımlayıcılarla](lexical-structure.md#identifiers) uygulanan dönüşümlerdir.

TODO: örnekler

### <a name="anonymous-method-expressions"></a>Anonim yöntem ifadeleri

Bir *anonymous_method_expression* , anonim bir işlevi tanımlamanın iki yöntemlerinden biridir. Bunlar, [anonim işlev ifadelerinde](expressions.md#anonymous-function-expressions)daha ayrıntılı olarak açıklanmıştır.

## <a name="unary-operators"></a>Birli İşleçler

@No__t-0, `+`, `-`, `!`, `~`, `++`, `--`, cast ve `await` işleçleri Birli İşleçler olarak adlandırılır.

```antlr
unary_expression
    : primary_expression
    | null_conditional_expression
    | '+' unary_expression
    | '-' unary_expression
    | '!' unary_expression
    | '~' unary_expression
    | pre_increment_expression
    | pre_decrement_expression
    | cast_expression
    | await_expression
    | unary_expression_unsafe
    ;
```

Bir *unary_expression* işleneni `dynamic` ' in derleme zamanı türüne sahipse, dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, *unary_expression* 'in derleme zamanı türü `dynamic` ve aşağıda açıklanan çözüm, işlenenin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

### <a name="null-conditional-operator"></a>Null-koşullu işleç

Null koşullu işleç, yalnızca bu işlenen null değilse, işlenene bir işlem listesi uygular. Aksi takdirde, işleci uygulamanın sonucu `null` ' dır.

```antlr
null_conditional_expression
    : primary_expression null_conditional_operations
    ;

null_conditional_operations
    : null_conditional_operations? '?' '.' identifier type_argument_list?
    | null_conditional_operations? '?' '[' argument_list ']'
    | null_conditional_operations '.' identifier type_argument_list?
    | null_conditional_operations '[' argument_list ']'
    | null_conditional_operations '(' argument_list? ')'
    ;
```

İşlem listesi, üye erişimi ve öğe erişim işlemleri (kendileri de null koşullu olabilir) ve çağırma içerebilir.

Örneğin `a.b?[0]?.c()` ifadesi, *primary_expression* `a.b` ve *null_conditional_operations* `?[0]` (null-koşullu öğe erişimi), `?.c` (null-koşullu üye) ile bir *null_conditional_expression* . erişim) ve `()` (çağırma).

Bir *primary_expression* `P` içeren bir *null_conditional_expression* `E` için, `E` *null_conditional_operations* @no__t her birinden önde gelen `?` ' i kaldırmak metin içeriğini eklemek tarafından elde edilen ifade bir tane vardır. Kavramsal olarak, `E0` `?`s tarafından temsil edilen null denetimlerin hiçbiri `null` bulamazsa değerlendirilecek ifadedir.

Ayrıca, metin içeriğini eklemek tarafından alınan ifadenin, `E` ' teki *null_conditional_operations* 'un yalnızca birinciden önde gelen `?` kaldırılarak elde @no__t. Bu bir *birincil ifadeye* (yalnızca bir `?` varsa) veya başka bir *null_conditional_expression*yol açabilir.

Örneğin, `E` ifadesi-1 @no__t, `E0` ifadesi `a.b[0].c()` ve `E1` ifadesi `a.b[0]?.c()` ' tir.

@No__t-0 Nothing olarak sınıflandırıldıysanız, `E` Nothing olarak sınıflandırılır. Aksi halde E bir değer olarak sınıflandırılır.

`E0` ve `E1` `E` ' nin anlamını belirlemede kullanılır:

*  @No__t-0 *statement_expression* olarak gerçekleşirse, `E` anlamı deyimde aynı olur

   ```csharp
   if ((object)P != null) E1;
   ```

   Bu P yalnızca bir kez değerlendirilir.

*  Aksi takdirde, `E0` hiçbir şey olarak sınıflandırıldığında, derleme zamanı hatası oluşur.

*  Aksi takdirde, `T0` `E0` türü olamaz.

   *  @No__t-0, başvuru türü veya null yapılamayan bir değer türü olarak bilinen bir tür parametresidir, derleme zamanı hatası oluşur.

   *  @No__t-0 null yapılamayan bir değer türü ise, `E` türü `T0?` ' dir ve @no__t 3 ' ün anlamı şu şekilde aynıdır

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      `P` yalnızca bir kez değerlendirilir.

   *  Aksi takdirde, E türü T0 ve E 'nin anlamı

      ```csharp
      ((object)P == null) ? null : E1
      ```

      `P` yalnızca bir kez değerlendirilir.

@No__t-0 ' ı bir *null_conditional_expression*ise, bu kurallar tekrar uygulanır @no__t, daha fazla `?` ' e kadar ve ifade, birincil ifadeye `E0` ' e kadar azaltılır.

Örneğin, `a.b?[0]?.c()` ifadesi deyimde olduğu gibi bir deyim ifadesi olarak gerçekleşirse:
```csharp
a.b?[0]?.c();
```
anlamı şu şekilde eşdeğerdir:
```csharp
if (a.b != null) a.b[0]?.c();
```
yeniden eşittir:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
@No__t-0 ve `a.b[0]` yalnızca bir kez değerlendirilir.

Değeri kullanıldığı bir bağlamda gerçekleşirse, şöyle olduğu gibi:
```csharp
var x = a.b?[0]?.c();
```
son çağrının türünün null yapılamayan bir değer türü olmadığı varsayılarak, anlamı şu değere eşittir:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
`a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Projeksiyon başlatıcıları olarak null Koşullu ifadeler

Null koşullu ifadeye yalnızca bir *anonymous_object_creation_expression* ([anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions)) içinde, bir (isteğe bağlı olarak null koşullu) üye erişimiyle biterse *member_declarator* olarak izin verilir. Dilbilgisi, bu gereksinim şöyle ifade edilebilir:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Bu, yukarıdaki *null_conditional_expression* için dilbilgisinde özel bir durumdur. [Anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions) *member_declarator* için üretim, daha sonra yalnızca *null_conditional_member_access*içerir.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Deyim ifadeleri olarak null Koşullu ifadeler

Null koşullu ifadeye yalnızca bir çağırma ile bitiyorsa, *statement_expression* ([Expression deyimleri](statements.md#expression-statements)) olarak izin verilir. Dilbilgisi, bu gereksinim şöyle ifade edilebilir:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Bu, yukarıdaki *null_conditional_expression* için dilbilgisinde özel bir durumdur. *Statement_expression* for [Expression deyimlerine](statements.md#expression-statements) yönelik üretim, daha sonra yalnızca *null_conditional_invocation_expression*içerir.


### <a name="unary-plus-operator"></a>Tekli artı işleci

@No__t-0 biçiminde bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Önceden tanımlanmış birli artı işleçleri şunlardır:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Bu işleçlerin her biri için sonuç yalnızca işlenenin değeridir.

### <a name="unary-minus-operator"></a>Birli eksi işleci

@No__t-0 biçiminde bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Önceden tanımlanmış olumsuzlama işleçleri şunlardır:

*  Tamsayı değilleme:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Sonuç, sıfırdan `x` çıkarılarak hesaplanır. @No__t-0 değeri, işlenen türünün en küçük gösterilemeyen değeri ise (-2 ^ 31 for `int` veya-2 ^ 63 for `long`), `x` ' ün matematik değilleme, işlenen türü içinde gösterilemeyen bir tablo değildir. Bu bir `checked` bağlamı içinde oluşursa, bir `System.OverflowException` oluşturulur; `unchecked` bağlamında gerçekleşirse, sonuç işlenenin değeridir ve taşma raporlanır.

   Olumsuzlama işlecinin işleneni `uint` türünde ise, `long` türüne dönüştürülür ve sonucun türü `long` olur. Bir özel durum, `int` değeri-2147483648 (-2 ^ 31) değerinin ondalık tamsayı sabit değeri ([tamsayı değişmez](lexical-structure.md#integer-literals)değer) olarak yazılmasına izin veren kuraldır.

   Olumsuzlama işlecinin işleneni `ulong` türünde ise, bir derleme zamanı hatası oluşur. Özel durum, `long` değeri-9223372036854775808 (-2 ^ 63) ' nin ondalık tamsayı sabit değeri ([tamsayı değişmez değerler](lexical-structure.md#integer-literals)) olarak yazılmasına izin veren kuraldır.

*  Kayan nokta olumsuzlama:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Sonuç, ters çevrilme ile `x` değeridir. @No__t-0 NaN ise, sonuç de NaN olur.

*  Ondalık olumsuzlama:

   ```csharp
   decimal operator -(decimal x);
   ```

   Sonuç, sıfırdan `x` çıkarılarak hesaplanır. Decimal olumsuzlama, `System.Decimal` türündeki birli eksi işlecinin kullanılmasıyla eşdeğerdir.

### <a name="logical-negation-operator"></a>Mantıksal Değilleme İşleci

@No__t-0 biçiminde bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Yalnızca bir tane önceden tanımlanmış mantıksal olumsuzlama işleci var:
```csharp
bool operator !(bool x);
```

Bu işleç, işlenenin mantıksal olumsuzunu hesaplar: İşlenen `true` ise, sonuç `false` ' dir. İşlenen `false` ise, sonuç `true` ' dir.

### <a name="bitwise-complement-operator"></a>Bit düzeyinde tamamlama işleci

@No__t-0 biçiminde bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Önceden tanımlanmış bit düzeyinde tamamlama işleçleri şunlardır:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Bu işleçlerin her biri için, işlemin sonucu `x` ' ı bit düzeyinde tamamdır.

@No__t-0 her numaralandırma türü, aşağıdaki bit düzeyinde tamamlayıcı işleci dolaylı olarak sağlar:

```csharp
E operator ~(E x);
```

@No__t-0 ' ın değerlendirilmesinin sonucu, `x` ' in temel alınan bir tür `U` olan `E` ' n i n @no__t değerlendirmesiyle tamamen aynıdır, ancak `E` ' e dönüştürme her zaman `unchecked` bağlamında gibi gerçekleştirilir ( [Checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Önek arttırma ve azaltma işleçleri

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

Önek artırma veya azaltma işleminin işleneni, değişken olarak sınıflandırılmış bir ifade, özellik erişimi veya Dizin Oluşturucu erişimi olmalıdır. İşlemin sonucu, işlenenden aynı türde bir değerdir.

Bir önek artış veya azaltma işleminin işleneni bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun hem `get` hem de bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Tekil operatör aşırı yükleme çözümü ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. Önceden tanımlı `++` ve `--` işleçleri şu türler için mevcuttur: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, 0, 1, 2, 3 ve herhangi bir numaralandırma türü. Önceden tanımlı `++` işleçleri, işlenene 1 eklenerek üretilen değeri döndürür ve önceden tanımlanmış `--` işleçleri işleneni 1 çıkararak üretilen değeri döndürür. @No__t-0 bağlamında, bu ekleme veya çıkarma sonucu sonuç türü aralığının dışındaysa ve sonuç türü bir tam sayı türü veya Enum türü ise, bir `System.OverflowException` atılır.

@No__t-0 veya `--x` biçiminde bir önek artış veya azaltma işleminin çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*   @No__t-0 bir değişken olarak sınıflandırıldıysanız:
    * `x` değişkeni üretmek için değerlendirilir.
    * Seçili operatör, bağımsız değişkeni olarak `x` değeriyle çağrılır.
    * İşleci tarafından döndürülen değer, `x` değerlendirmesi tarafından verilen konumda depolanır.
    * İşleci tarafından döndürülen değer işlemin sonucu olur.
*   @No__t-0 bir özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldığında:
    * Örnek ifadesi (`x` `static` değilse) ve bağımsız değişken listesi (`x` ' in bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `get` ve `set` erişimci etkinleştirmeleri içinde kullanılır.
    * @No__t-1 ' in `get` erişimcisi çağrıldı.
    * Seçilen işleç, bağımsız değişkeni olarak `get` erişimcisinin döndürdüğü değerle çağrılır.
    * @No__t-1 ' in `set` erişimcisi, `value` bağımsız değişkeni olarak işleç tarafından döndürülen değerle çağrılır.
    * İşleci tarafından döndürülen değer işlemin sonucu olur.

@No__t-0 ve `--` işleçleri de sonek gösterimini ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) destekler. Genellikle, `x++` veya `x--` sonucu işlemden önce `x` değeridir, ancak `++x` veya `--x` sonucu işlemden sonra `x` değeri olur. Her iki durumda da, `x` ' ın kendisi işlemden sonra aynı değere sahiptir.

@No__t-0 veya `operator--` bir uygulama, sonek veya ön ek gösterimi kullanılarak çağrılabilir. İki gösterimler için ayrı işleç uygulamalarına sahip olmak mümkün değildir.

### <a name="cast-expressions"></a>Atama ifadeleri

Bir ifadeyi açıkça belirli bir türe dönüştürmek için bir *cast_expression* kullanılır.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

@No__t-1, `T` *' nin bir* *tür* olduğu ve `E` ' ün bir *unary_expression*olduğu `E` değerini `T` türünde bir açık dönüştürme ([Açık dönüştürmeler](conversions.md#explicit-conversions)) gerçekleştirir. @No__t-0 ' dan `T` ' e hiçbir açık dönüştürme yoksa, bağlama zamanı hatası oluşur. Aksi takdirde, sonuç açık dönüştürme tarafından üretilen değerdir. Sonuç her zaman bir değer olarak sınıflandırılır, `E` bir değişkeni belirtir.

Bir *cast_expression* için dilbilgisi, belirli bir sözdizimsel belirsizlikleri 'e yol açar. Örneğin, `(x)-y` ifadesi bir *cast_expression* (`x` türüne `-y` dönüştürmesi) ya da *parenthesized_expression* ile birleştirilmiş bir *additive_expression* (`x - y)` değerini hesaplayan) olarak yorumlanır.

*Cast_expression* belirsizlikleri 'i çözümlemek için aşağıdaki kural bulunur: Parantez içine alınmış bir veya daha fazla *belirteç*([boşluk](lexical-structure.md#white-space)) sırası, *cast_expression* 'in başlangıcını yalnızca en az bir tane doğru olduğunda kabul edilir:

*  Belirteçlerin sırası, bir *tür*için doğru dilbilgisinde bulunur, ancak bir *ifade*için kullanılamaz.
*  Belirteçlerin sırası, bir *tür*için doğru dilbilgisi ve kapatma parantezinden hemen sonraki belirteç "`~`", belirteç "`!`", belirteç "`(`", bir *tanımlayıcı* ([Unicode karakter kaçış dizileri ](lexical-structure.md#unicode-character-escape-sequences)), bir *değişmez değer* ([değişmez](lexical-structure.md#literals)değer) veya 0 ve 1 dışında herhangi bir *anahtar sözcük* ([anahtar](lexical-structure.md#keywords)sözcük).

Yukarıdaki "doğru dilbilgisi" terimi, yalnızca belirteçlerin sırasının belirli dilbilgisi üretimine uyması gerektiği anlamına gelir. Bu, özellikle herhangi bir anayayrılan tanımlayıcıların gerçek anlamını düşünmez. Örneğin, `x` ve `y` tanımlayıcısa, `x.y` gerçekten bir tür belirtmese bile, `x.y` bir tür için doğru dilbilgisidir.

Kesinleştirme kuralından sonra, `x` ve `y` tanımlayıcıları varsa, `(x)y`, `(x)(y)` ve `(x)(-y)` *cast_expression*s olsa da, `x` bir tür tanımladığı halde `(x)-y` değildir. Ancak, `x`, önceden tanımlanmış bir türü (`int` gibi) tanımlayan bir anahtar sözcüktür, tüm dört form *cast_expression*s 'dir (böyle bir anahtar sözcük büyük olasılıkla kendisi bir ifade olamaz).

### <a name="await-expressions"></a>Await ifadeleri

Await işleci, işlenen tarafından temsil edilen zaman uyumsuz işlem tamamlanana kadar kapsayan zaman uyumsuz işlevin değerlendirmesini askıya almak için kullanılır.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

*Await_expression* yalnızca zaman uyumsuz bir işlevin gövdesinde ([yineleyiciler](classes.md#iterators)) izin verilir. En yakın kapsayan zaman uyumsuz işlevi içinde, *await_expression* bu yerlerde gerçekleşmeyebilir:

*  İç içe geçmiş (zaman uyumsuz) anonim bir işlev içinde
*  Bir *lock_statement* bloğunun içinde
*  Güvenli olmayan bir bağlamda

Bir *await_expression* , bir *query_expression*içinde çoğu yerde gerçekleşmediğini unutmayın, çünkü bunlar sözdizimsel olmayan lambda ifadeleri kullanmak üzere CLS olarak dönüştürülür.

Zaman uyumsuz bir işlevin içinde, `await` tanımlayıcı olarak kullanılamaz. Bu nedenle, await ifadeleri ve tanımlayıcılar içeren çeşitli ifadeler arasında sözdizimsel belirsizlik yoktur. Zaman uyumsuz işlevlerin dışında, `await` normal tanımlayıcı işlevi görür.

Bir *await_expression* işleneni ***görev***olarak adlandırılır. *Await_expression* değerlendirilen zaman uyumsuz bir işlemi temsil eder veya tamamlanmayabilir. Await işlecinin amacı, beklenen zaman uyumsuz işlevin yürütülmesini, bekleme görevi tamamlanana kadar askıya almak ve sonra sonucunu elde etmek için kullanılır.

#### <a name="awaitable-expressions"></a>Awasever ifadeleri

Await ifadesinin görevi için ***beklenen***bir ifade olması gerekir. Aşağıdakilerden biri varsa-0 @no__t bir ifade beklenebilir:

*  `t` derleme zamanı türü `dynamic`
*  `t` ' da `GetAwaiter` adlı, hiçbir parametre olmadan ve hiçbir tür parametresi olmadan ve bir dönüş türü olan `A` ' yi içeren erişilebilir bir örnek veya genişletme yöntemi vardır:
   * `A` `System.Runtime.CompilerServices.INotifyCompletion` arabirimini uygular (bundan sonra breçekimi için `INotifyCompletion` olarak bilinirdi)
   * `A` ' @no__t erişilebilir, okunabilir bir örnek özelliğine sahiptir `bool` türünde-1
   * `A` ' @no__t erişilebilir bir örnek yöntemi vardır ve parametre içermeyen-1 ve tür parametresi yok

@No__t-0 yönteminin amacı, görev için bir ***awaiter*** elde sağlamaktır. @No__t-0 türü await ifadesi için ***awaiter türü*** olarak adlandırılır.

@No__t-0 özelliğinin amacı görevin zaten tamamlanıp tamamlanmadığını belirlemektir. Bu durumda, değerlendirmeyi askıya almanız gerekmez.

@No__t-0 yönteminin amacı, göreve bir "devamlılık" kaydı sağlamaktır; Yani, görev tamamlandıktan sonra çağrılacak bir temsilci (`System.Action` türü).

@No__t-0 yönteminin amacı, tamamlandıktan sonra görevin sonucunu elde etmek için kullanılır. Bu sonuç, büyük olasılıkla bir sonuç değeriyle birlikte başarılı bir şekilde tamamlanmayabilir veya `GetResult` yöntemi tarafından oluşturulan bir özel durum olabilir.

#### <a name="classification-of-await-expressions"></a>Await ifadelerinin sınıflandırması

@No__t-0 ifadesi, `(t).GetAwaiter().GetResult()` ifadesiyle aynı şekilde sınıflandırıldı. Bu nedenle, `GetResult` ' ın dönüş türü `void` ise, *await_expression* hiçbir şey olarak sınıflandırılacaktır. Void olmayan bir dönüş türü `T` ise, *await_expression* -2 @no__t türünde bir değer olarak sınıflandırılacaktır.

#### <a name="runtime-evaluation-of-await-expressions"></a>Await ifadelerinin çalışma zamanı değerlendirmesi

Çalışma zamanında, `await t` ifadesi aşağıdaki gibi değerlendirilir:

*  Awaiter `a` `(t).GetAwaiter()` ifadesi hesaplanarak elde edilir.
*  @No__t-0 `b` `(a).IsCompleted` ifadesi hesaplanarak elde edilir.
*  @No__t-0 ' ı `false` ise değerlendirme, `a` @no__t arabirimini (breçekimi için `ICriticalNotifyCompletion` olarak bilinirdi) uygulayıp uygulamadığına bağlıdır. Bu denetim bağlama zamanında yapılır; Yani, `a` ' ın derleme zamanı türü `dynamic` ise ve derleme sırasında çalışma zamanında. Let `r` sürdürme temsilciyi ([yineleyiciler](classes.md#iterators)) gösterir:
    * @No__t-0 ' ı @no__t uygulamadığından, `(a as (INotifyCompletion)).OnCompleted(r)` ifadesi değerlendirilir.
    * @No__t-0 `ICriticalNotifyCompletion` ' i uygulıyorsa, `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ifadesi değerlendirilir.
    * Değerlendirme daha sonra askıya alınır ve denetim zaman uyumsuz işlevin geçerli çağıranına döndürülür.
*  Hemen sonra (`b` `true` ise) veya daha sonra sürdürme temsilcisinin çağrılması durumunda (`b` `false` ise), `(a).GetResult()` ifadesi değerlendirilir. Değer döndürürse, bu değer *await_expression*sonucudur. Aksi takdirde sonuç Nothing olur.

Awaiter 'ın `INotifyCompletion.OnCompleted` ve `ICriticalNotifyCompletion.UnsafeOnCompleted` Arabirim yöntemlerinin uygulanması, `r` temsilcisinin en çok bir kez çağrılmasına neden olmalıdır. Aksi takdirde, kapsayan zaman uyumsuz işlevinin davranışı tanımsız olur.

## <a name="arithmetic-operators"></a>Aritmetik İşleçler

@No__t-0, `/`, `%`, `+` ve `-` işleçleri aritmetik işleçler olarak adlandırılır.

```antlr
multiplicative_expression
    : unary_expression
    | multiplicative_expression '*' unary_expression
    | multiplicative_expression '/' unary_expression
    | multiplicative_expression '%' unary_expression
    ;

additive_expression
    : multiplicative_expression
    | additive_expression '+' multiplicative_expression
    | additive_expression '-' multiplicative_expression
    ;
```

Aritmetik işlecin bir işleneni derleme zamanı türü `dynamic` ise, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic` olan bu işlenenlerinin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

### <a name="multiplication-operator"></a>Çarpma işleci

@No__t-0 biçiminde bir işlem için, ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış çarpma işleçleri aşağıda listelenmiştir. İşleçler `x` ve `y` çarpımını hesaplar.

*  Tamsayı çarpma:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   @No__t-0 bağlamında, ürün sonuç türü aralığının dışındaysa, bir `System.OverflowException` atılır. @No__t-0 bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.


*  Kayan nokta çarpma:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Ürün, IEEE 754 aritmetik kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` pozitif sonlu değerlerdir. `z` ' ın @no__t sonucudur. Sonuç hedef türü için çok büyükse, `z` sonsuzluk olur. Sonuç hedef türü için çok küçükse, `z` sıfırdır.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | \+ y   | -y   | +0  | -0  | \+ INF | -INF | NaN | 
   | \+ x   | \+ z   | -z   | +0  | -0  | \+ INF | -INF | NaN | 
   | -x   | -z   | \+ z   | -0  | +0  | -INF | \+ INF | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | \+ INF | \+ INF | -INF | NaN | NaN | \+ INF | -INF | NaN | 
   | -INF | -INF | \+ INF | NaN | NaN | -INF | \+ INF | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Ondalık çarpma:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Elde edilen değer `decimal` biçiminde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonuç değeri `decimal` biçiminde temsil etmek için çok küçük ise, sonuç sıfırdır. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklerinin toplamıdır.

   Ondalık çarpma `System.Decimal` türünde çarpma işlecinin kullanılmasıyla eşdeğerdir.


### <a name="division-operator"></a>Bölme işleci

@No__t-0 biçiminde bir işlem için, ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış bölüm işleçleri aşağıda listelenmiştir. İşleçler `x` ve `y` bölümünü hesaplar.

*  Tamsayı bölme:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Sağ işlenenin değeri sıfırsa, `System.DivideByZeroException` oluşturulur.

   Bölme sonucu sıfıra doğru yuvarlar. Bu nedenle, sonucun mutlak değeri, iki işlenenin alanının mutlak değerinden küçük veya ona eşit olan en büyük olası tamsayıdır. İki işlenen iki işlenen de ters işaret sahibi olduğunda, sonuç sıfır veya pozitif, sıfır veya negatif olur.

   Sol işlenen en küçük gösterilemeyen `int` veya `long` değeri ise ve sağ işlenen `-1` ise, taşma oluşur. @No__t-0 bağlamında, bu, bir `System.ArithmeticException` ' ın (veya alt sınıfının) oluşturulmasına neden olur. @No__t-0 bağlamında, uygulama tanımlı bir `System.ArithmeticException` (veya alt sınıfın) mi yoksa taşma değeri ise sol işlenenin elde edilen değerle bildirilmemiştir.

*  Kayan nokta bölmesi:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Bu bölüm, IEEE 754 aritmetiğinin kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` pozitif sonlu değerlerdir. `z` ' ın @no__t sonucudur. Sonuç hedef türü için çok büyükse, `z` sonsuzluk olur. Sonuç hedef türü için çok küçükse, `z` sıfırdır.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ INF | -INF | NaN  | 
   | \+ x   | \+ z   | -z   | \+ INF | -INF | +0   | -0   | NaN  | 
   | -x   | -z   | \+ z   | -INF | \+ INF | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | \+ INF | \+ INF | -INF | \+ INF | -INF | NaN  | NaN  | NaN  | 
   | -INF | -INF | \+ INF | -INF | \+ INF | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Ondalık bölme:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Sağ işlenenin değeri sıfırsa, `System.DivideByZeroException` oluşturulur. Elde edilen değer `decimal` biçiminde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonuç değeri `decimal` biçiminde temsil etmek için çok küçük ise, sonuç sıfırdır. Sonucun ölçeği, en yakın gösterilebilir tablo ondalık değerine eşit olan bir sonucu doğru matematik sonucuyla koruyacak en küçük ölçeğe neden olur.

   Ondalık bölme `System.Decimal` türünde Bölme işlecinin kullanılmasıyla eşdeğerdir.


### <a name="remainder-operator"></a>Kalan işleç

@No__t-0 biçiminde bir işlem için, ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış geri kalan işleçler aşağıda listelenmiştir. İşleçler, `x` ve `y` arasındaki bölmenin geri kalanını hesaplar.

*  Tamsayı geri kalanı:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   @No__t-0 sonucu, `x - (x / y) * y` tarafından üretilen değerdir. @No__t-0 sıfırsa, bir `System.DivideByZeroException` atılır.

   Sol işlenen en küçük `int` veya `long` değeri ise ve sağ işlenen `-1` ise, bir @no__t 3 oluşturulur. Hiçbir durumda `x % y`, `x / y` ' in bir özel durum oluşturmadığından bir özel durum oluşturur.

*  Kayan nokta kalanı:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` pozitif sonlu değerlerdir. `z` `x % y` ' in sonucudur ve `x - n * y` olarak hesaplanır; burada `n` `x / y` ' e eşit veya daha küçük olan en büyük tamsayıdır. Bu geri kalanı hesaplama yöntemi, tamsayı işlenenleri için kullanılan ile benzerdir, ancak IEEE 754 tanımından (`n` `x / y` ' e en yakın tamsayıdır) farklıdır.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ INF | -INF | NaN  | 
   | \+ x   | \+ z   | \+ z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | \+ INF | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -INF | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Ondalık kalan:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Sağ işlenenin değeri sıfırsa, `System.DivideByZeroException` oluşturulur. Sonucun ölçeği, herhangi bir yuvarlamadan önce iki işlenenin ölçeklendirilmesi ve sıfır olmayan bir değer ise `x` ' la aynı olur.

   Ondalık geri kalanı `System.Decimal` türündeki geri kalan işlecin kullanılmasıyla eşdeğerdir.


### <a name="addition-operator"></a>Toplama işleci

@No__t-0 biçiminde bir işlem için, ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış ekleme işleçleri aşağıda listelenmiştir. Sayısal ve sabit listesi türlerinde, önceden tanımlanmış ekleme işleçleri iki işlenenin toplamını hesaplar. Bir veya her iki işlenen de dize türünde olduğunda, önceden tanımlanmış toplama işleçleri işlenen dize gösterimini birleştirir.

*  Tamsayı ekleme:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   @No__t-0 bağlamında, Sum sonuç türü aralığının dışındaysa, bir `System.OverflowException` atılır. @No__t-0 bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.

*  Kayan nokta ekleme:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Toplam, IEEE 754 aritmetiğinin kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y`, sıfır dışında sonlu değerlerdir ve @no__t @no__t 2 ' nin sonucudur. @No__t-0 ve `y` aynı büyüklük, ancak ters işaretlere sahipse, `z` pozitif sıfırdır. @No__t-0, hedef türünde temsil etmek için çok büyükse, `z`, `x + y` ile aynı işarete sahip bir sonsuzluk olur.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | Y    | +0   | -0   | \+ INF | -INF | NaN  | 
   | x    | z    | x    | x    | \+ INF | -INF | NaN  | 
   | +0   | Y    | +0   | +0   | \+ INF | -INF | NaN  | 
   | -0   | Y    | +0   | -0   | \+ INF | -INF | NaN  | 
   | \+ INF | \+ INF | \+ INF | \+ INF | \+ INF | NaN  | NaN  | 
   | -INF | -INF | -INF | -INF | NaN  | -INF | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Ondalık ekleme:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Elde edilen değer `decimal` biçiminde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklendirilmesine göre daha büyüktür.

   Ondalık toplama, `System.Decimal` türünde toplama işlecinin kullanılmasıyla eşdeğerdir.

*  Numaralandırma ekleme. Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı işleçleri sağlar; burada `E` sabit listesi türüdür ve `U` `E` ' nin temel türüdür:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   Çalışma zamanında bu işleçler tam olarak `(E)((U)x + (U)y)` olarak değerlendirilir.

*  Dize birleştirme:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   İkili `+` işlecinin bu aşırı yüklemeleri, dizeyi birleştirme işlemini gerçekleştirir. Dize birleştirme işleneni `null` ise boş bir dize değiştirilir. Aksi takdirde, dize olmayan herhangi bir bağımsız değişken `object` türünden devralınan sanal `ToString` yöntemini çağırarak dize gösterimine dönüştürülür. @No__t-0 `null` döndürürse, boş bir dize değiştirilir.

   ```csharp
   using System;
   
   class Test
   {
       static void Main() {
           string s = null;
           Console.WriteLine("s = >" + s + "<");        // displays s = ><
           int i = 1;
           Console.WriteLine("i = " + i);               // displays i = 1
           float f = 1.2300E+15F;
           Console.WriteLine("f = " + f);               // displays f = 1.23E+15
           decimal d = 2.900m;
           Console.WriteLine("d = " + d);               // displays d = 2.900
       }
   }
   ```

   Dize birleştirme işlecinin sonucu, sol işlenenin karakterlerinden ve ardından sağ işlenenin karakterlerinden oluşan bir dizedir. Dize birleştirme işleci hiçbir şekilde `null` değeri döndürmez. Elde edilen dizeyi ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulabilir.

*  Temsilci birleşimi. Her temsilci türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `D` temsilci türüdür:

   ```csharp
   D operator +(D x, D y);
   ```

   İkili `+` işleci, her iki işlenen de `D` ' de bir temsilci türü olduğunda temsilci birleşimi gerçekleştirir. (İşlenenler farklı temsilci türlerine sahip ise, bir bağlama zamanı hatası oluşur.) İlk işlenen `null` ise, işlemin sonucu ikinci işlenenin değeridir (Bu da `null` olsa bile). Aksi takdirde, ikinci işlenen `null` ise, işlemin sonucu ilk işlenenin değeridir. Aksi takdirde, işlem sonucu çağrıldığında, ilk işleneni çağırır ve sonra ikinci işleneni çağıran yeni bir temsilci örneğidir. Temsilci birleşimi örnekleri için bkz. [çıkarma işleci](expressions.md#subtraction-operator) ve [temsilci çağrısı](delegates.md#delegate-invocation). @No__t-0 bir temsilci türü olmadığından, `operator` @ no__t-2 tanımlı değildir.

### <a name="subtraction-operator"></a>Çıkarma işleci

@No__t-0 biçiminde bir işlem için, ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış çıkarma işleçleri aşağıda listelenmiştir. İşleçler `y` ' dan `x` ' den çıkar.

*  Tamsayı çıkarma:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   @No__t-0 bağlamında, fark sonuç türü aralığının dışındaysa, bir `System.OverflowException` atılır. @No__t-0 bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.

*  Kayan nokta çıkarma:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Aradaki fark, IEEE 754 aritmetiğinin kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaNs 'ın tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y`, sıfır dışında sonlu değerlerdir ve @no__t @no__t 2 ' nin sonucudur. @No__t-0 ve `y` eşitse, `z` pozitif sıfırdır. @No__t-0, hedef türünde temsil etmek için çok büyükse, `z`, `x - y` ile aynı işarete sahip bir sonsuzluk olur.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | Y    | +0   | -0   | \+ INF | -INF | NaN | 
   | x    | z    | x    | x    | -INF | \+ INF | NaN | 
   | +0   | -y   | +0   | +0   | -INF | \+ INF | NaN | 
   | -0   | -y   | -0   | +0   | -INF | \+ INF | NaN | 
   | \+ INF | \+ INF | \+ INF | \+ INF | NaN  | \+ INF | NaN | 
   | -INF | -INF | -INF | -INF | -INF | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Ondalık çıkarma:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Elde edilen değer `decimal` biçiminde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklendirilmesine göre daha büyüktür.

   Decimal çıkarma `System.Decimal` türünde çıkarma işlecinin kullanılmasıyla eşdeğerdir.

*  Sabit Listesi çıkarma. Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `E` sabit listesi türüdür ve `U` `E` ' nin temel türüdür:

   ```csharp
   U operator -(E x, E y);
   ```

   Bu işleç tam olarak `(U)((U)x - (U)y)` olarak değerlendirilir. Diğer bir deyişle, işleci `x` ve `y` ' in Ordinal değerleri arasındaki farkı hesaplar ve sonucun türü, numaralandırmanın temel alınan türüdür.

   ```csharp
   E operator -(E x, U y);
   ```

   Bu işleç tam olarak `(E)((U)x - y)` olarak değerlendirilir. Diğer bir deyişle işleci, numaralandırmanın temel alınan türünden bir değeri çıkartır ve sabit listesinin bir değerini çıkarır.

*  Temsilci kaldırma. Her temsilci türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `D` temsilci türüdür:

   ```csharp
   D operator -(D x, D y);
   ```

   İkili `-` işleci her iki işlenen de `D` temsilcisinlerinde temsilci kaldırma gerçekleştirir. İşlenenler farklı temsilci türlerine sahip ise, bir bağlama zamanı hatası oluşur. İlk işlenen `null` ise, işlemin sonucu `null` ' dir. Aksi takdirde, ikinci işlenen `null` ise, işlemin sonucu ilk işlenenin değeridir. Aksi halde, her iki işlenen de bir veya daha fazla girişe sahip çağrı listelerini ([temsilci bildirimleri](delegates.md#delegate-declarations)) temsil eder ve sonuç, ikinci işlenenin işlenenin listesi, ilki için uygun bir bitişik alt liste.     (Alt liste eşitliğini belirleyebilmek için, karşılık gelen girişler, temsilci eşitlik işleci ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)) ile karşılaştırılır.) Aksi halde, sonuç sol işlenenin değeridir. İşlemde işlenen listelerden hiçbiri değiştirilmez. İkinci işlenenin listesi, ilk işlenen listesindeki bitişik girdilerin birden çok alt listesiyle eşleşiyorsa, bitişik girdilerin en sağ eşleşen alt listesi kaldırılır. Kaldırma işlemi boş bir liste ile sonuçlanırsa, sonuç `null` ' dır. Örneğin:

   ```csharp
   delegate void D(int x);
   
   class C
   {
       public static void M1(int i) { /* ... */ }
       public static void M2(int i) { /* ... */ }
   }

   class Test
   {
       static void Main() { 
           D cd1 = new D(C.M1);
           D cd2 = new D(C.M2);
           D cd3 = cd1 + cd2 + cd2 + cd1;   // M1 + M2 + M2 + M1
           cd3 -= cd1;                      // => M1 + M2 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd2;                // => M2 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd2;                // => M1 + M1
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd2 + cd1;                // => M1 + M2
   
           cd3 = cd1 + cd2 + cd2 + cd1;     // M1 + M2 + M2 + M1
           cd3 -= cd1 + cd1;                // => M1 + M2 + M2 + M1
       }
   }
   ```

## <a name="shift-operators"></a>Kaydırma işleçleri

@No__t-0 ve `>>` işleçleri bit kaydırma işlemlerini gerçekleştirmek için kullanılır.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Bir *shift_expression* işleneni derleme zamanı türü `dynamic` ise, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic` olan bu işlenenlerinin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

@No__t-0 veya `x >> count` biçiminde bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Aşırı yüklenmiş bir kaydırma işleci bildirirken, İlk işlenenin türü her zaman operatör bildirimini içeren sınıf veya yapı olmalıdır ve ikinci işlenenin türü her zaman `int` olmalıdır.

Önceden tanımlanmış kaydırma işleçleri aşağıda listelenmiştir.

*  Sola kaydır:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   @No__t-0 işleci, `x` ' i aşağıda açıklandığı şekilde hesaplanan sayıda bit ile sola kaydırır.

   @No__t-0 ' ın sonuç türü aralığının dışında yüksek sıralı bitler atılır, kalan bitler sola kaydıralınır ve düşük sıralı boş bit konumları sıfır olarak ayarlanır.

*  Sağa kaydır:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   @No__t-0 işleci, aşağıda açıklandığı gibi hesaplanmış bir bit sayısına `x` ' i kaydırır.

   @No__t-0 `int` veya `long` türünde olduğunda, `x` ' ün düşük sıralı bitleri atılır, kalan bitler sağa kaydıralınır ve yüksek sıralı boş bit konumları sıfır olarak ayarlanır ve `x` negatifse bir tane olarak ayarlanır.

   @No__t-0 `uint` veya `ulong` türünde olduğunda, `x` ' ün düşük sıralı bitleri atılır, kalan bitler sağ kaydıralınır ve yüksek sıralı boş bit konumları sıfır olarak ayarlanır.

Önceden tanımlanmış işleçler için, kaydırma yapılacak bit sayısı şu şekilde hesaplanır:

*  @No__t-0 `int` veya `uint` olduğunda, kaydırma sayısı düşük sıralı beş bit `count` ' i tarafından verilir. Diğer bir deyişle, kaydırma sayısı `count & 0x1F` ' dan hesaplanır.
*  @No__t-0 `long` veya `ulong` olduğunda, kaydırma sayısı, `count` ' ün alt-sırası ile verilir. Diğer bir deyişle, kaydırma sayısı `count & 0x3F` ' dan hesaplanır.

Elde edilen kaydırma sayısı sıfırsa, kaydırma işleçleri yalnızca `x` değerini döndürür.

SHIFT işlemleri hiçbir şekilde taşmaya neden olmaz ve `checked` ve `unchecked` bağlamlarındaki aynı sonuçları üretir.

@No__t-0 işlecinin sol işleneni işaretli bir integral türünde olduğunda, işleç, işlenenin en önemli bit (işaret biti) değerinde bir aritmetik kaydırma gerçekleştirir ve yüksek sıralı boş bit konumlarına yayılır. @No__t-0 işlecinin sol işleneni işaretsiz bir integral türünde olduğunda, işleç yüksek sıralı boş bit konumlarda her zaman sıfır olarak ayarlanmış bir mantıksal kaydırma gerçekleştirir. İşlenen türünden çıkarılan ' nin ters işlemini gerçekleştirmek için, açık atamalar kullanılabilir. Örneğin, `x` `int` türünde bir değişkense `unchecked((int)((uint)x >> y))` işlemi `x` ' ün bir mantıksal kaydırma uygular.

## <a name="relational-and-type-testing-operators"></a>İlişkisel ve tür testi işleçleri

@No__t-0, `!=`, `<`, `>`, `<=`, `>=`, `is` ve `as` işleçleri ilişkisel ve tür testi işleçleri olarak adlandırılır.

```antlr
relational_expression
    : shift_expression
    | relational_expression '<' shift_expression
    | relational_expression '>' shift_expression
    | relational_expression '<=' shift_expression
    | relational_expression '>=' shift_expression
    | relational_expression 'is' type
    | relational_expression 'as' type
    ;

equality_expression
    : relational_expression
    | equality_expression '==' relational_expression
    | equality_expression '!=' relational_expression
    ;
```

@No__t-0 işleci [,,](expressions.md#the-is-operator) ve `as` işlecinin [as işleci](expressions.md#the-as-operator)içinde açıklanmaktadır.

@No__t-0, `!=`, `<`, `>`, `<=` ve `>=` işleçleri ***karşılaştırma işleçleridir***.

Karşılaştırma işlecinin bir işleneni derleme zamanı türü `dynamic` ise, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic` olan bu işlenenlerinin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

@No__t-0 *op* `y` biçiminde bir işlem için, *op* bir karşılaştırma operatörü olduğunda, belirli bir operatör uygulamasını seçmek için aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış karşılaştırma işleçleri aşağıdaki bölümlerde açıklanmıştır. Önceden tanımlanmış tüm karşılaştırma işleçleri, aşağıdaki tabloda açıklandığı gibi `bool` türünde bir sonuç döndürür.


| __İşlem__ | __Sonuç__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `x`  `y` ' ye eşitse `false` Aksi takdirde                 | 
| `x != y`      | `x`  `y` ' ye eşit değilse `false` Aksi takdirde             | 
| `x < y`       | `x`  `y` ' den küçükse `false` Aksi takdirde                | 
| `x > y`       | `x`  `y` ' den büyükse `false` Aksi takdirde             | 
| `x <= y`      | `x`  `y` ' ye eşit veya daha küçükse `false` Aksi takdirde    | 
| `x >= y`      | `x`  `y` ' ye eşit veya daha büyükse `false` Aksi takdirde | 

### <a name="integer-comparison-operators"></a>Tamsayı karşılaştırma işleçleri

Önceden tanımlanmış tamsayı karşılaştırma işleçleri şunlardır:
```csharp
bool operator ==(int x, int y);
bool operator ==(uint x, uint y);
bool operator ==(long x, long y);
bool operator ==(ulong x, ulong y);

bool operator !=(int x, int y);
bool operator !=(uint x, uint y);
bool operator !=(long x, long y);
bool operator !=(ulong x, ulong y);

bool operator <(int x, int y);
bool operator <(uint x, uint y);
bool operator <(long x, long y);
bool operator <(ulong x, ulong y);

bool operator >(int x, int y);
bool operator >(uint x, uint y);
bool operator >(long x, long y);
bool operator >(ulong x, ulong y);

bool operator <=(int x, int y);
bool operator <=(uint x, uint y);
bool operator <=(long x, long y);
bool operator <=(ulong x, ulong y);

bool operator >=(int x, int y);
bool operator >=(uint x, uint y);
bool operator >=(long x, long y);
bool operator >=(ulong x, ulong y);
```

Bu işleçlerin her biri, iki tamsayı işleneninin sayısal değerlerini karşılaştırır ve belirli bir ilişkinin `true` veya `false` olduğunu belirten `bool` değerini döndürür.

### <a name="floating-point-comparison-operators"></a>Kayan nokta karşılaştırma işleçleri

Önceden tanımlanmış kayan nokta karşılaştırma işleçleri şunlardır:
```csharp
bool operator ==(float x, float y);
bool operator ==(double x, double y);

bool operator !=(float x, float y);
bool operator !=(double x, double y);

bool operator <(float x, float y);
bool operator <(double x, double y);

bool operator >(float x, float y);
bool operator >(double x, double y);

bool operator <=(float x, float y);
bool operator <=(double x, double y);

bool operator >=(float x, float y);
bool operator >=(double x, double y);
```

İşleçler, işlenenleri IEEE 754 standardının kurallarına göre karşılaştırır:

*  Her iki işlenen de NaN ise, sonuç `true` olan `!=` hariç tüm işleçler için `false` olur. Her iki işlenen için `x != y` her zaman `!(x == y)` ile aynı sonucu üretir. Ancak, bir veya her iki işlenen de NaN olduğunda, `<`, `>`, `<=` ve `>=` işleçleri, ters işlecin mantıksal Olumsuzlaştırma ile aynı sonuçları oluşturmaz. Örneğin, `x` ve `y` NaN ise, `x < y` `false`, ancak `!(x >= y)` `true` ' tir.
*  Hiçbir işlenen NaN olmadığında, işleçler iki kayan nokta işleneninin değerlerini sıralamaya göre karşılaştırır

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   Burada `min` ve `max`, verilen kayan nokta biçiminde temsil edilebilecek en küçük ve en büyük pozitif değerlerdir. Bu sıralamanın önemli etkileri şunlardır:
   * Negatif ve pozitif sıfırlar eşit kabul edilir.
   * Negatif sonsuzluk, diğer tüm değerlerden daha az kabul edilir, ancak başka bir negatif sonsuzya eşittir.
   * Pozitif sonsuzluk, diğer tüm değerlerden daha büyük sayılır, ancak başka bir pozitif sonsuzya eşittir.

### <a name="decimal-comparison-operators"></a>Ondalık karşılaştırma işleçleri

Önceden tanımlanmış ondalık karşılaştırma işleçleri şunlardır:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Bu işleçlerin her biri, iki ondalık işlenenin sayısal değerlerini karşılaştırır ve belirli bir ilişkinin `true` veya `false` olduğunu belirten `bool` değeri döndürür. Her ondalık karşılaştırma, `System.Decimal` türünde karşılık gelen ilişkisel veya eşitlik işlecinin kullanılmasıyla eşdeğerdir.

### <a name="boolean-equality-operators"></a>Boole eşitlik işleçleri

Önceden tanımlanmış Boole eşitlik işleçleri şunlardır:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

@No__t-0 ' ın sonucu, hem `x` hem de `y` `true` ise ve `x` ve `y` `false` ise `true` ' dir. Aksi takdirde, sonuç `false` ' dır.

@No__t-0 ' ın sonucu, hem `x` hem de `y` `true` ise ve `x` ve `y` `false` ise `false` ' dir. Aksi takdirde, sonuç `true` ' dır. İşlenenler `bool` türünde olduğunda `!=` işleci `^` işleci ile aynı sonucu üretir.

### <a name="enumeration-comparison-operators"></a>Sabit Listesi karşılaştırma işleçleri

Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı karşılaştırma işleçlerini sağlar:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

@No__t-1 ve @no__t-@no__t 2 ' nin temel alınan bir tür `U` ve `op` ' i karşılaştırma işleçlerinden biri, `((U)x) op ((U)y)` değerlendirilirken tam olarak aynı olduğu `x op y` ' ı değerlendirme sonucu. Diğer bir deyişle, numaralandırma türü karşılaştırma işleçleri yalnızca iki işlenenin temel alınan integral değerlerini karşılaştırmaktır.

### <a name="reference-type-equality-operators"></a>Başvuru türü eşitlik işleçleri

Önceden tanımlanmış başvuru türü eşitlik işleçleri şunlardır:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

İşleçler, eşitlik veya eşitlik için iki başvuruyu karşılaştırmanın sonucunu döndürür.

Önceden tanımlanmış başvuru türü eşitlik işleçleri `object` türündeki işlenenleri kabul ettiğinde, uygulanabilir `operator ==` ve `operator !=` üyelerini bildirmiyor tüm türler için geçerlidir. Buna karşılık, geçerli kullanıcı tanımlı eşitlik işleçleri, önceden tanımlanmış başvuru türü eşitlik işleçlerini etkin bir şekilde gizler.

Önceden tanımlanmış başvuru türü eşitlik işleçleri aşağıdakilerden birini gerektirir:

*  Her iki işlenen de, *reference_type* veya `null` sabit değeri olarak bilinen bir türün değeridir. Ayrıca, bir açık başvuru dönüştürmesi ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), herhangi bir işlenenin türünden diğer işlenenin türüne de sahiptir.
*  Bir işlenen `T` türünde, `T` ' in bir *type_parameter* ve diğer işlenen ise `null` ' tür. Ayrıca `T` ' ın değer türü kısıtlaması yoktur.

Bu koşullardan biri doğru değilse, bir bağlama zamanı hatası oluşur. Bu kuralların önemli etkileri şunlardır:

*  Bu, bağlama zamanında farklı oldukları bilinen iki başvuruyu karşılaştırmak için önceden tanımlanmış başvuru türü eşitlik işleçlerini kullanmanın bir bağlama zamanı hatasıdır. Örneğin, işlenenlerin bağlama zamanı türleri `A` ve `B` olarak iki sınıf türütürlerdi ve hiçbiri ne `A` ne de `B` diğeri ondan türetilmediği takdirde, iki işlenen de aynı nesneye başvuruda bulunmak olanaksızdır. Bu nedenle, işlem bağlama zamanı hatası olarak değerlendirilir.
*  Önceden tanımlanmış başvuru türü eşitlik işleçleri değer türü işlenenlerinin karşılaştırılabilmesi için izin vermez. Bu nedenle, bir struct türü kendi eşitlik işleçlerini bildirmediği için, bu yapı türünün değerlerini karşılaştırmak mümkün değildir.
*  Önceden tanımlanmış başvuru türü eşitlik işleçleri, işlenenleri için paketleme işlemlerine neden olmaz. Yeni ayrılmış paketlenmiş örneklere yapılan başvuruların diğer tüm başvurulardan farklı olması gerektiğinden, bu tür kutulanabilir işlemleri gerçekleştirmek anlamlı değildir.
*  @No__t-0 tür parametre türünün işleneni `null` ile karşılaştırılır ve `T` ' nin çalışma zamanı türü bir değer türü ise, karşılaştırma sonucu `false` ' tür.

Aşağıdaki örnek, kısıtlanmamış bir tür parametre türünün bağımsız değişkeninin `null` olup olmadığını denetler.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

@No__t-0 yapısına izin verilir, ancak `T` bir değer türünü temsil ediyor olsa da, `T` bir değer türü olduğunda sonuç yalnızca `false` olacak şekilde tanımlanır.

@No__t-0 veya `x != y` biçiminde bir işlem varsa, herhangi bir `operator ==` veya `operator !=` varsa, işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) kuralları önceden tanımlanmış başvuru türü eşitlik yerine bu işleci seçer işlecinde. Ancak, bir veya her ikisini de `object` türüne açıkça atayarak önceden tanımlanmış başvuru türü eşitlik işlecini seçmek her zaman mümkündür. Örnek
```csharp
using System;

class Test
{
    static void Main() {
        string s = "Test";
        string t = string.Copy(s);
        Console.WriteLine(s == t);
        Console.WriteLine((object)s == t);
        Console.WriteLine(s == (object)t);
        Console.WriteLine((object)s == (object)t);
    }
}
```
çıktıyı üretir
```console
True
False
False
False
```

@No__t-0 ve `t` değişkenleri aynı karakterleri içeren iki ayrı @no__t 2 örneğe başvurur. Her iki işlenen de `string` türünde olduğunda, önceden tanımlanmış dize eşitlik işleci ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) seçildiğinden ilk karşılaştırma çıktılarının `True` ' dır. Bir veya her ikisi de `object` türünde olduğunda, önceden tanımlanmış başvuru türü eşitlik işleci seçildiğinden, kalan tüm çıktılar `False` ' dır.

Yukarıdaki tekniğinin değer türleri için anlamlı olmadığına unutmayın. Örnek
```csharp
class Test
{
    static void Main() {
        int i = 123;
        int j = 123;
        System.Console.WriteLine((object)i == (object)j);
    }
}
```
yayınlar `False`, çünkü yayınlar kutulanmış `int` değerlerinin iki ayrı örneğine başvuru oluşturur.

### <a name="string-equality-operators"></a>Dize eşitlik işleçleri

Önceden tanımlanmış dize eşitlik işleçleri şunlardır:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Aşağıdakilerden biri doğru olduğunda iki `string` değeri eşit kabul edilir:

*  Her iki değer de `null` ' dır.
*  Her iki değer de aynı uzunlukta ve her bir karakter konumunda özdeş karakterlere sahip olan dize örneklerine yönelik null olmayan başvurulardır.

Dize eşitlik işleçleri dize başvuruları yerine dize değerlerini karşılaştırır. İki ayrı dize örneği tam olarak aynı karakter dizisini içerdiğinde, dizelerin değerleri eşittir, ancak başvurular farklıdır. [Başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)bölümünde açıklandığı gibi, dize değerleri yerine dize başvurularını karşılaştırmak için başvuru türü eşitlik işleçleri kullanılabilir.

### <a name="delegate-equality-operators"></a>Eşitlik işleçlerini devretmek

Her temsilci türü örtük olarak aşağıdaki önceden tanımlı karşılaştırma işleçlerini sağlar:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

İki temsilci örneği aşağıdaki gibi kabul edilir:

*  Temsilci örneklerinden biri `null` ise, yalnızca ikisi de `null` ise eşittir.
*  Temsilcilerin farklı çalışma zamanı türleri varsa, bunlar hiçbir zaman eşit değildir.
*  Her iki temsilci örneği de bir çağırma listesine ([temsilci bildirimleri](delegates.md#delegate-declarations)) sahip ise, bu örnekler eşit olur ve yalnızca çağırma listeleri aynı uzunluktadır ve tek bir çağrı listesindeki her girdinin karşılık gelen girişin, diğer öğesinin çağrı listesinde olması.

Aşağıdaki kurallar, çağırma listesi girişlerinin eşitlik düzeyini yönetir:

*  İki çağırma listesi girişi her ikisi de aynı statik yönteme başvurursanız, girdiler eşittir.
*  İki çağırma listesi girişi her ikisi de aynı hedef nesnede statik olmayan bir yönteme (başvuru eşitlik işleçleri tarafından tanımlandığı gibi) başvurmazsa, girdiler eşittir.
*  Yakalanan dış değişken örnekleri kümesiyle aynı (büyük olasılıkla boş) olan anlamsal özdeş *anonymous_method_expression*s veya *lambda_expression*s değerlendirmesinden üretilen çağırma listesi girişleri için izin verilir (ancak gerekli değildir) sıfıra.

### <a name="equality-operators-and-null"></a>Eşitlik işleçleri ve null

@No__t-0 ve `!=` işleçleri, işlem için önceden tanımlanmış veya Kullanıcı tanımlı bir operatör (unlifted veya yükseltilmemiş biçiminde) mevcut olmasa bile, tek bir işlenenin null yapılabilir türde ve diğeri de `null` sabit değeri olarak izin verir.

Formlardan birine bir işlem için
```csharp
x == null
null == x
x != null
null != x
```
`x` ' ın null yapılabilir bir tür ifadesi olduğu durumlarda, işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) ilgili bir işleci bulamazsa, bunun yerine `x` ' ün `HasValue` özelliğinden hesaplanır. Özellikle, ilk iki form `!x.HasValue` ' a çevrilir ve son iki form `x.HasValue` ' e çevrilir.

### <a name="the-is-operator"></a>SIS işleci

@No__t-0 işleci, bir nesnenin çalışma zamanı türünün belirli bir tür ile uyumlu olup olmadığını dinamik olarak denetlemek için kullanılır. @No__t-1 ' in bir ifade olduğu ve `T` bir tür olduğu `E is T` işleminin sonucu, `E` ' ün bir başvuru dönüştürmesi, paketleme dönüştürmesi veya bir kutudan çıkarma dönüştürmesi ile `T` türüne başarıyla dönüştürülüp dönüştürülmeyeceğini belirten bir Boole değeridir. Tür bağımsız değişkenleri tüm tür parametreleri yerine eklendikten sonra işlem aşağıdaki gibi değerlendirilir:

*  @No__t-0 Anonim bir işlevdir, derleme zamanı hatası oluşur
*  @No__t-0 bir yöntem grubu veya `null` değişmez değeri ise, `E` türü bir başvuru türü veya null yapılabilir bir tür ise ve `E` değeri null ise sonuç false olur.
*  Aksi takdirde, `D` `E` dinamik türünü aşağıdaki şekilde temsil eder:
   * @No__t-0 türü bir başvuru türü ise, `D`, örnek başvurusunun çalışma zamanı türüdür `E`.
   * @No__t-0 türü null yapılabilir bir tür ise, `D` null yapılabilir türün temel türüdür.
   * @No__t-0 türü null yapılamayan bir değer türü ise, `D` `E` türüdür.
*  İşlemin sonucu `D` ve `T` ' i şu şekilde değişir:
   * @No__t-0 bir başvuru türü ise, `D` ve `T` aynı türde ise sonuç true olur. `D` bir başvuru türüdür ve `D` ' ten `T` ' e bir örtük başvuru dönüştürmesi varsa veya `D` bir değer türüdür ve `D` ' den kutulamayı dönüştürmedir `T` için var.
   * @No__t-0 null yapılabilir bir tür ise, `D` `T` ' nin temel türü ise sonuç true olur.
   * @No__t-0 null yapılamayan bir değer türü ise, `D` ve `T` aynı türde ise sonuç true olur.
   * Aksi takdirde, sonuç false 'tur.

Kullanıcı tanımlı dönüştürmelerin `is` işleci tarafından değerlendirilmediğini unutmayın.

### <a name="the-as-operator"></a>As işleci

@No__t-0 işleci, açıkça bir değeri belirli bir başvuru türüne veya null yapılabilir türe dönüştürmek için kullanılır. Atama ifadesinin ([cast ifadeleri](expressions.md#cast-expressions)) aksine, `as` işleci hiçbir şekilde özel durum oluşturmaz. Bunun yerine, belirtilen dönüştürme mümkün değilse, elde edilen değer `null` ' dır.

@No__t-0 biçiminde bir işlemde, `E` bir ifade olmalıdır ve `T` bir başvuru türü, bir başvuru türü olarak bilinen bir tür parametresi veya null yapılabilir bir tür olmalıdır. Ayrıca, aşağıdakilerden en az birinin doğru olması gerekir, aksi takdirde bir derleme zamanı hatası oluşur:

*  Bir kimlik ([kimlik dönüştürme](conversions.md#identity-conversion)), örtük null yapılabilir ([örtük Nullable dönüşümler](conversions.md#implicit-nullable-conversions)), örtük başvuru ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)), paketleme ([kutulama dönüştürmeleri](conversions.md#boxing-conversions)), açık boş değer atanabilir ([Açık Nullable Dönüşümler](conversions.md#explicit-nullable-conversions)), açık başvuru ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) veya kutudan çıkarma ([kutudan çıkarma dönüşümleri](conversions.md#unboxing-conversions)) dönüştürme `E` ' den `T` ' e kadar.
*  @No__t-0 veya `T` türü açık bir tür.
*  `E`, `null` sabit değerdir.

@No__t-0 ' ın derleme zamanı türü `dynamic` değilse, `E as T` işlemi ile aynı sonucu üretir
```csharp
E is T ? (T)(E) : (T)null
```
`E` hariç yalnızca bir kez değerlendirilir. Derleyicinin, yukarıdaki genişleme tarafından kapsanan iki dinamik tür denetimi yerine en çok bir dinamik tür denetimi gerçekleştirmesi için `E as T` ' i iyileştirmek için, derleyici bekleniyor.

@No__t-0 ' ın derleme zamanı türü `dynamic` ise, atama işlecinin aksine `as` işleci dinamik olarak bağlı değildir ([dinamik bağlama](expressions.md#dynamic-binding)). Bu nedenle, bu durumda genişletme şu şekilde olur:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Kullanıcı tanımlı dönüştürmeler gibi bazı dönüştürmelerin `as` işleci ile mümkün olmadığı ve bunun yerine atama ifadeleri kullanılarak gerçekleştirilmesi gerektiğini unutmayın.

Örnekte
```csharp
class X
{

    public string F(object o) {
        return o as string;        // OK, string is a reference type
    }

    public T G<T>(object o) where T: Attribute {
        return o as T;             // Ok, T has a class constraint
    }

    public U H<U>(object o) {
        return o as U;             // Error, U is unconstrained 
    }
}
```
sınıf kısıtlamasına sahip olduğundan, `G` ' in @no__t parametresi bir başvuru türü olarak bilinir. @No__t-1 ' in @no__t parametresi, ancak değil; Bu nedenle `H` ' te `as` işlecinin kullanılmasına izin verilmez.

## <a name="logical-operators"></a>Mantıksal işleçler

@No__t-0, `^` ve `|` işleçleri mantıksal işleçler olarak adlandırılır.

```antlr
and_expression
    : equality_expression
    | and_expression '&' equality_expression
    ;

exclusive_or_expression
    : and_expression
    | exclusive_or_expression '^' and_expression
    ;

inclusive_or_expression
    : exclusive_or_expression
    | inclusive_or_expression '|' exclusive_or_expression
    ;
```

Bir mantıksal işlecin işleneni, derleme zamanı türü `dynamic` ise, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic` olan bu işlenenlerinin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

@No__t-0 biçiminde bir işlem için, `op` mantıksal işleçlerden biri olduğunda, belirli bir operatör uygulamasını seçmek için aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış mantıksal işleçler aşağıdaki bölümlerde açıklanmıştır.

### <a name="integer-logical-operators"></a>Tamsayı mantıksal işleçleri

Önceden tanımlı tamsayı mantıksal işleçleri şunlardır:
```csharp
int operator &(int x, int y);
uint operator &(uint x, uint y);
long operator &(long x, long y);
ulong operator &(ulong x, ulong y);

int operator |(int x, int y);
uint operator |(uint x, uint y);
long operator |(long x, long y);
ulong operator |(ulong x, ulong y);

int operator ^(int x, int y);
uint operator ^(uint x, uint y);
long operator ^(long x, long y);
ulong operator ^(ulong x, ulong y);
```

@No__t-0 işleci iki işlenenin bit düzeyinde mantıksal `AND` ' i hesaplar, `|` işleci iki işlenenin bit düzeyinde mantıksal `OR` ' ü hesaplar ve `^` işleci iki işlenenin bit düzeyinde mantıksal dışlamalı `OR` ' i hesaplar. Bu işlemlerden taşmalar mümkün değildir.

### <a name="enumeration-logical-operators"></a>Sabit listesi mantıksal işleçleri

@No__t-0 her numaralandırma türü, aşağıdaki önceden tanımlanmış mantıksal işleçleri dolaylı olarak sağlar:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

@No__t-1 ve @no__t-@no__t 2 ' nin temel alınan bir tür `U` ve `op` ' in mantıksal işleçlerden biri olduğu `x op y` ' ı değerlendirme sonucu, `(E)((U)x op (U)y)` değerlendirilirken tam olarak aynıdır. Diğer bir deyişle, mantıksal işleçler numaralandırma türü mantıksal işlemleri yalnızca iki işlenenin temel alınan türünde gerçekleştirir.

### <a name="boolean-logical-operators"></a>Boole mantıksal işleçleri

Önceden tanımlanmış Boole mantıksal işleçleri şunlardır:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

@No__t-0 ' ın sonucu, hem `x` hem de `y` `true` ise, `true` ' dir. Aksi takdirde, sonuç `false` ' dır.

@No__t-0 ' ın sonucu, `x` veya `y` ' ü `true` ise, `true` ' dir. Aksi takdirde, sonuç `false` ' dır.

@No__t-0 `true` ' dir. `x` `true` ise `y` `false`, `x` `false` ve `y` `true` ' dur. Aksi takdirde, sonuç `false` ' dır. İşlenenler `bool` türünde olduğunda `^` işleci `!=` işleci ile aynı sonucu hesaplar.

### <a name="nullable-boolean-logical-operators"></a>Null yapılabilir Boolean mantıksal işleçler

@No__t Nullable Boolean türü, `true`, `false` ve `null` olmak üzere üç değeri temsil edebilir ve kavramsal olarak SQL 'de Boolean ifadeler için kullanılan üç değerli tür ile benzerdir. @No__t-2 işlenenleri için `&` ve `|` işleçleri tarafından üretilen sonuçların SQL 'in üç değerli mantığıyla tutarlı olduğundan emin olmak için, aşağıdaki önceden tanımlanmış işleçler verilmiştir:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

Aşağıdaki tabloda, `true`, `false` ve `null` değerlerinin tüm birleşimleri için bu işleçler tarafından oluşturulan sonuçlar listelenmektedir.

| `x`     | `y`     | `x & y` | <code>x &#124; y</code> |
|:-------:|:-------:|:-------:|:-------:|
| `true`  | `true`  | `true`  | `true`  | 
| `true`  | `false` | `false` | `true`  | 
| `true`  | `null`  | `null`  | `true`  | 
| `false` | `true`  | `false` | `true`  | 
| `false` | `false` | `false` | `false` | 
| `false` | `null`  | `false` | `null`  | 
| `null`  | `true`  | `null`  | `true`  | 
| `null`  | `false` | `false` | `null`  | 
| `null`  | `null`  | `null`  | `null`  | 

## <a name="conditional-logical-operators"></a>Koşullu mantıksal işleçler

@No__t-0 ve `||` işleçleri Koşullu mantıksal işleçler olarak adlandırılır. Bunlar ayrıca "kısa devre dışı" mantıksal işleçler olarak da adlandırılır.

```antlr
conditional_and_expression
    : inclusive_or_expression
    | conditional_and_expression '&&' inclusive_or_expression
    ;

conditional_or_expression
    : conditional_and_expression
    | conditional_or_expression '||' conditional_and_expression
    ;
```

@No__t-0 ve `||` işleçleri, `&` ve `|` işleçlerinin koşullu sürümleridir:

*  @No__t-0 işlemi, yalnızca `x` `false` değilse `y` ' nin değerlendirilmemesi dışında, `x & y` işlemine karşılık gelir.
*  @No__t-0 işlemi, yalnızca `x` `true` değilse `y` ' nin değerlendirilmemesi dışında, `x | y` işlemine karşılık gelir.

Koşullu bir mantıksal işlecin işleneni, derleme zamanı türü `dynamic` ise, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic` olan bu işlenenlerinin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

@No__t-0 veya `x || y` biçiminde bir işlem, işlem `x & y` veya `x | y` yazılmış gibi aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümü](expressions.md#binary-operator-overload-resolution)) uygulanarak işlenir. Ni

*  Aşırı yükleme çözümlemesi tek bir en iyi operatör bulamazsa veya aşırı yükleme çözümlemesi önceden tanımlı tamsayı mantıksal işleçlerinden birini seçerse bağlama zamanı hatası oluşur.
*  Aksi takdirde, seçili operatör önceden tanımlanmış Boole mantıksal işleçlerinden ([Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)) veya null yapılabilir Boolean mantıksal Işleçlerden ([null yapılabilir Boolean mantıksal işleçler](expressions.md#nullable-boolean-logical-operators)) biri ise, işlem şu konuda açıklandığı [gibi işlenir. Boole Koşullu mantıksal işleçler](expressions.md#boolean-conditional-logical-operators).
*  Aksi takdirde, seçilen işleç Kullanıcı tanımlı bir işleçtir ve işlem [Kullanıcı tanımlı Koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators)bölümünde açıklandığı şekilde işlenir.

Koşullu mantıksal işleçleri doğrudan aşırı yüklemek mümkün değildir. Ancak, Koşullu mantıksal işleçler normal mantıksal işleçler açısından değerlendirildiğinden, normal mantıksal operatörlerin aşırı yüklemeleri, Koşullu mantıksal işleçlerin aşırı yüklerini de kabul edilen belirli kısıtlamalarla birlikte bulunur. Bu, [Kullanıcı tanımlı Koşullu mantıksal işleçlerde](expressions.md#user-defined-conditional-logical-operators)daha ayrıntılı olarak açıklanmıştır.

### <a name="boolean-conditional-logical-operators"></a>Boole Koşullu mantıksal işleçler

@No__t-0 veya `||` ' in işlenenleri `bool` türünde olduğunda veya işlenenler geçerli bir `operator &` veya `operator |` tanımlamaz, ancak `bool` ' e örtük dönüştürmeler tanımlamıyorsa, işlem aşağıdaki gibi işlenir :

*  @No__t-0 işlemi `x ? y : false` olarak değerlendirilir. Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `bool` türüne dönüştürülür. Ardından, `x` `true` ise, `y` değerlendirilir ve `bool` türüne dönüştürülür ve bu işlemin sonucu olur. Aksi takdirde, işlemin sonucu `false` ' dır.
*  @No__t-0 işlemi `x ? true : y` olarak değerlendirilir. Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `bool` türüne dönüştürülür. @No__t-0 `true` ise, işlemin sonucu `true` olur. Aksi takdirde, `y` değerlendirilir ve `bool` türüne dönüştürülür ve bu işlemin sonucu olur.

### <a name="user-defined-conditional-logical-operators"></a>Kullanıcı tanımlı Koşullu mantıksal işleçler

@No__t-0 veya `||` ' in işlenenleri, geçerli bir Kullanıcı tanımlı `operator &` veya `operator |` ' ü bildiren türlerse, aşağıdakilerin her ikisi de doğru olmalıdır; burada `T`, seçili işlecin bildirildiği türdür:

*  Seçili işlecin her bir parametresinin dönüş türü ve türü `T` olmalıdır. Diğer bir deyişle, işleç mantıksal `AND` veya `T` türünde iki işlenenin mantıksal `OR` ' i hesaplamalıdır ve `T` türünde bir sonuç döndürmelidir.
*  `T` `operator true` ve `operator false` bildirimlerini içermelidir.

Bu gereksinimlerin herhangi biri karşılanmıyorsa bağlama zamanı hatası oluşur. Aksi takdirde, `&&` veya `||` işlemi, Kullanıcı tanımlı `operator true` veya `operator false` ' ü seçili kullanıcı tanımlı işleçle birleştirerek değerlendirilir:

*  @No__t-0 işlemi `T.false(x) ? x : T.&(x, y)` olarak değerlendirilir; burada `T.false(x)` `T` ' te belirtilen `operator false` ' ün bir çağrıdır ve `T.&(x, y)` ' i seçilen `operator &` ' ın bir çağrıdır. Diğer bir deyişle, `x` ilk değerlendirilir ve sonuç üzerinde `operator false` çağrıldığında `x` ' nin kesinlikle yanlış olduğunu tespit edilir. Sonra, `x` kesinlikle yanlış ise, işlemin sonucu daha önce `x` için hesaplanan değerdir. Aksi takdirde, `y` değerlendirilir ve seçilen `operator &`, daha önce `x` için hesaplanan değerde ve işlem sonucunu üretmek için `y` için hesaplanan değer üzerinde çağrılır.
*  @No__t-0 işlemi `T.true(x) ? x : T.|(x, y)` olarak değerlendirilir; burada `T.true(x)` `T` ' te belirtilen `operator true` ' ün bir çağrıdır ve `T.|(x,y)` ' i seçilen `operator|` ' ın bir çağrıdır. Diğer bir deyişle, `x` ilk değerlendirilir ve `x` ' nin kesinlikle doğru doğru olup olmadığını anlamak için sonuç üzerinde `operator true` çağrılır. Sonra, `x` kesinlikle doğru ise, işlemin sonucu daha önce `x` için hesaplanan değerdir. Aksi takdirde, `y` değerlendirilir ve seçilen `operator |`, daha önce `x` için hesaplanan değerde ve işlem sonucunu üretmek için `y` için hesaplanan değer üzerinde çağrılır.

Bu işlemlerden birinde, `x` tarafından verilen ifade yalnızca bir kez değerlendirilir ve `y` tarafından verilen ifade değerlendirilmez ya da tam olarak bir kez değerlendirilmez.

@No__t-0 ve `operator false` uygulayan bir tür örneği için bkz. [veritabanı Boole türü](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Null birleştirme işleci

@No__t-0 işlecine null birleşim işleci denir.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

@No__t-0 biçiminde boş bir birleştirme ifadesi @no__t, null yapılabilir bir tür veya başvuru türü olmasını gerektirir. @No__t-0 null değilse, `a ?? b` sonucu `a` ' dir; Aksi takdirde, sonuç `b` ' dir. İşlem yalnızca `a` null ise `b` olarak değerlendirilir.

Null birleştirme işleci, işlemlerin sağdan sola gruplanarak doğru ilişkilendirilebilir. Örneğin, `a ?? b ?? c` biçiminde bir ifade `a ?? (b ?? c)` olarak değerlendirilir. Genel koşullarda, `E1 ?? E2 ?? ... ?? En` biçiminde bir ifade, null olmayan veya tüm işlenenler null ise null olan işlenenleri ilk döndürür.

@No__t-0 ifadesinin türü, işlenenler üzerinde hangi örtük dönüştürmelerin kullanılabilir olduğuna bağlıdır. Tercih sırasına göre `a ?? b` ' ın türü `A0`, `A` veya `B` ' dir; burada `A` ' ün türü `a` ' tir (`a` ' ın bir türü vardır) `B` türü `b` ' dir (`b` ' a bir tür vardır) ve 0, 2 null yapılabilir bir tür ya da 3 değilse 1 ' in temel türüdür. Özellikle, `a ?? b` şu şekilde işlenir:

*  @No__t-0 varsa ve null yapılabilir bir tür ya da bir başvuru türü değilse, derleme zamanı hatası oluşur.
*  @No__t-0 dinamik bir ifadesiyse, sonuç türü `dynamic` ' dir. Çalışma zamanında, `a` ilk olarak değerlendirilir. @No__t-0 null değilse, `a` dinamik olarak dönüştürülür ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve bu sonuç olur.
*  Aksi takdirde, `A` varsa ve null yapılabilir bir tür ise ve `b` ' den `A0` ' ye örtük bir dönüştürme varsa, sonuç türü `A0` ' tür. Çalışma zamanında, `a` ilk olarak değerlendirilir. @No__t-0 null değilse, `a` `A0` türüne sarmalanmamış ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve `A0` türüne dönüştürülür ve bu sonuç olur.
*  Aksi takdirde, `A` varsa ve `b` ' den `A` ' ye örtük bir dönüştürme varsa, sonuç türü `A` ' tür. Çalışma zamanında, `a` ilk olarak değerlendirilir. @No__t-0 null değilse, `a` sonuç haline gelir. Aksi takdirde, `b` değerlendirilir ve `A` türüne dönüştürülür ve bu sonuç olur.
*  Aksi takdirde, `b` ' a `B` türü varsa ve `a` ' den `B` ' e örtük bir dönüştürme varsa, sonuç türü `B` olur. Çalışma zamanında, `a` ilk olarak değerlendirilir. @No__t-0 null değilse, `a` `A0` türüne sarmalanır (`A` varsa ve null yapılabilir) ve `B` türüne dönüştürülürse, bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve sonuç olur.
*  Aksi takdirde, `a` ve `b` uyumsuzdur ve derleme zamanı hatası oluşur.

## <a name="conditional-operator"></a>Koşullu işleç

@No__t-0 işleci koşullu işleç olarak adlandırılır. Üçlü işleç olarak da adlandırılır.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

@No__t-0 ' ın koşullu bir ifadesi ilk olarak `b` koşulunu değerlendirir. Ardından, `b` `true` ise, `x` değerlendirilir ve işlemin sonucu olur. Aksi takdirde, `y` değerlendirilir ve işlemin sonucu olur. Koşullu bir ifade `x` ve `y` ' i hiçbir şekilde değerlendirmez.

Koşullu operatör doğru ilişkilendirilebilir, yani işlemler sağdan sola gruplandırılır. Örneğin, `a ? b : c ? d : e` biçiminde bir ifade `a ? b : (c ? d : e)` olarak değerlendirilir.

@No__t-0 işlecinin ilk işleneni örtük olarak `bool` ' e dönüştürülebilen bir ifade veya `operator true` uygulayan bir tür ifadesi olmalıdır. Bu gereksinimlerin hiçbiri karşılanmazsa, bir derleme zamanı hatası oluşur.

İkinci ve üçüncü işlenen `?:` işlecinin `x` ve `y`, koşullu ifadenin türünü denetler.

*  @No__t-0 ' a `X` türü varsa ve `y` türü `Y` ' e sahipse
   * @No__t-1 ' den `Y` ' ye örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) varsa ancak `Y` ' e `X` ' e kadar değilse, `Y` koşullu ifadenin türüdür.
   * @No__t-1 ' den `X` ' ye örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) varsa ancak `X` ' e `Y` ' e kadar değilse, `X` koşullu ifadenin türüdür.
   * Aksi takdirde, hiçbir ifade türü belirlenemez ve derleme zamanı hatası oluşur.
*  Yalnızca `x` ve `y` ' in bir türü varsa ve hem `x` hem de `y`, bu türe örtük olarak dönüştürülebilir ve bu, koşullu ifadenin türüdür.
*  Aksi takdirde, hiçbir ifade türü belirlenemez ve derleme zamanı hatası oluşur.

@No__t-0 formundaki koşullu bir ifadenin çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*  İlk olarak, `b` değerlendirilir ve `b` `bool` değeri belirlenir:
   * @No__t-0 ile `bool` arasında örtük bir dönüştürme varsa, bu örtük dönüştürme `bool` değeri üretmek için gerçekleştirilir.
   * Aksi takdirde, `b` türü tarafından tanımlanan `operator true` `bool` değeri üretmek için çağrılır.
*  Yukarıdaki adım tarafından üretilen `bool` değeri `true` ise, `x` değerlendirilir ve koşullu ifadenin türüne dönüştürülür ve bu, koşullu ifadenin sonucu olur.
*  Aksi takdirde, `y` değerlendirilir ve koşullu ifadenin türüne dönüştürülür ve bu, koşullu ifadenin sonucu olur.

## <a name="anonymous-function-expressions"></a>Anonim işlev ifadeleri

***Anonim işlev*** , "satır içi" yöntem tanımını temsil eden bir ifadedir. Anonim bir işlev, ve için bir değer veya tür içermez, ancak uyumlu bir temsilciye veya ifade ağacı türüne dönüştürülebilir. Anonim işlev dönüştürmenin değerlendirmesi, dönüştürmenin hedef türüne bağlıdır: Bir temsilci türü ise, dönüştürme, anonim işlevin tanımladığı yönteme başvuran bir temsilci değeri olarak değerlendirilir. Bir ifade ağacı türü ise dönüştürme, yöntemin yapısını bir nesne yapısı olarak temsil eden bir ifade ağacı olarak değerlendirilir.

Geçmiş nedenlerle anonim işlevlerin iki sözdizimsel özelliği vardır, örneğin, *lambda_expression*s ve *anonymous_method_expression*s. Neredeyse tüm amaçlar için, *lambda_expression*s, geriye doğru uyumluluk için dilde kalacak şekilde, *anonymous_method_expression*s 'den daha kısa ve açıklayıcı olur.

```antlr
lambda_expression
    : anonymous_function_signature '=>' anonymous_function_body
    ;

anonymous_method_expression
    : 'delegate' explicit_anonymous_function_signature? block
    ;

anonymous_function_signature
    : explicit_anonymous_function_signature
    | implicit_anonymous_function_signature
    ;

explicit_anonymous_function_signature
    : '(' explicit_anonymous_function_parameter_list? ')'
    ;

explicit_anonymous_function_parameter_list
    : explicit_anonymous_function_parameter (',' explicit_anonymous_function_parameter)*
    ;

explicit_anonymous_function_parameter
    : anonymous_function_parameter_modifier? type identifier
    ;

anonymous_function_parameter_modifier
    : 'ref'
    | 'out'
    ;

implicit_anonymous_function_signature
    : '(' implicit_anonymous_function_parameter_list? ')'
    | implicit_anonymous_function_parameter
    ;

implicit_anonymous_function_parameter_list
    : implicit_anonymous_function_parameter (',' implicit_anonymous_function_parameter)*
    ;

implicit_anonymous_function_parameter
    : identifier
    ;

anonymous_function_body
    : expression
    | block
    ;
```

@No__t-0 işleci atama ile aynı önceliğe sahiptir (`=`) ve sağ ilişkilendirilebilir.

@No__t-0 değiştiricisi olan anonim bir işlev zaman uyumsuz bir işlevdir ve [yineleyiciler](classes.md#iterators)bölümünde açıklanan kuralları izler.

Bir *lambda_expression* biçimindeki anonim bir işlevin parametreleri açık veya örtük olarak yazılabilir olabilir. Açıkça yazılmış bir parametre listesinde, her parametrenin türü açıkça belirtilir. Örtük olarak yazılmış bir parametre listesinde, parametrelerin türleri, anonim işlevin gerçekleştiği bağlamdan çıkarslanmıştır — özellikle anonim işlev, uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürüldüğünde, bu tür şunu sağlar parametre türleri ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)).

Tek ve örtük olarak yazılmış bir parametreye sahip anonim bir işlevde, parantez parametre listesinden atlanabilir. Diğer bir deyişle, formun anonim bir işlevi
```csharp
( param ) => expr
```
kısaltılabilir
```csharp
param => expr
```

Bir *anonymous_method_expression* biçimindeki adsız işlevin parametre listesi isteğe bağlıdır. Belirtilmişse, parametrelerin açıkça yazılması gerekir. Aksi takdirde, anonim işlev `out` parametrelerini içermeyen herhangi bir parametre listesi olan bir temsilciye dönüştürülebilir.

Adsız bir işlevin *blok* gövdesine erişilebilir ([bitiş noktaları ve erişilebilirlik](statements.md#end-points-and-reachability)). Bu, anonim işlev ulaşılamaz bir bildirimde gerçekleşmezse.

Anonim işlevlere bazı örnekler aşağıda verilmiştir:

```csharp
x => x + 1                              // Implicitly typed, expression body
x => { return x + 1; }                  // Implicitly typed, statement body
(int x) => x + 1                        // Explicitly typed, expression body
(int x) => { return x + 1; }            // Explicitly typed, statement body
(x, y) => x * y                         // Multiple parameters
() => Console.WriteLine()               // No parameters
async (t1,t2) => await t1 + await t2    // Async
delegate (int x) { return x + 1; }      // Anonymous method expression
delegate { return 1 + 1; }              // Parameter list omitted
```

*Lambda_expression*s ve *anonymous_method_expression*s davranışı aşağıdaki noktaları hariç aynıdır:

*  *anonymous_method_expression*s, parametre listesinin tamamen atlanmasına izin verir ve herhangi bir değer parametreleri listesinin temsilci türlerine göz ardı edilir.
*  *lambda_expression*s parametre türlerinin atlanmasına ve çıkarlanmasına izin verir, ancak *anonymous_method_expression*s parametre türlerinin açıkça belirtilmesini gerektirir.
*  Bir *lambda_expression* gövdesi bir ifade veya deyim bloğu olabilir, ancak bir *anonymous_method_expression* gövdesi bir deyim bloğu olmalıdır.
*  Yalnızca *lambda_expression*s, uyumlu ifade ağacı türlerine Dönüştürmelere sahiptir ([ifade ağacı türleri](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Anonim işlev imzaları

Anonim işlev için isteğe bağlı *anonymous_function_signature* , anonim işlev için adları ve isteğe bağlı olarak biçimsel parametrelerin türlerini tanımlar. Anonim işlevin parametrelerinin kapsamı *anonymous_function_body*' dir. ([Kapsamlar](basic-concepts.md#scopes)) Parametre listesi ile birlikte (verildiyse) anonim-yöntem-gövde bir bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) oluşturur. Bu nedenle, anonim işlev parametresinin adı için bir yerel değişkenin adı, yerel sabit veya kapsamı *anonymous_method_expression* veya *lambda_expression*içeren bir parametre için derleme zamanı hatası oluşur.

Anonim bir işlevde bir *explicit_anonymous_function_signature*varsa, uyumlu temsilci türleri ve ifade ağacı türleri kümesi aynı sırayla aynı parametre türleri ve değiştiricilerle kısıtlıdır. Yöntem grubu dönüştürmelerinde ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)), anonim işlev parametre türlerinin Contra-varyansı desteklenmez. Anonim bir işlevde bir *anonymous_function_signature*yoksa, uyumlu temsilci türleri ve ifade ağacı türleri kümesi `out` parametrelerine sahip olanlarla kısıtlıdır.

Bir *anonymous_function_signature* öznitelikleri veya parametre dizisi içeremediğini unutmayın. Bununla birlikte, bir *anonymous_function_signature* , parametre listesi parametre dizisi içeren bir temsilci türüyle uyumlu olabilir.

Ayrıca, aynı zamanda derleme zamanında ([ifade ağacı türleri](types.md#expression-tree-types)) hala başarısız olabilecek bir ifade ağacı türüne dönüştürme

### <a name="anonymous-function-bodies"></a>Anonim işlev gövdeleri

Anonim bir işlevin gövdesi (*ifadesi* veya *bloğu*) aşağıdaki kurallara tabidir:

*  Anonim işlev bir imza içeriyorsa, İmzada belirtilen parametreler gövdede kullanılabilir. Anonim işlevde imza yoksa, parametrelere sahip bir temsilci türüne veya ifade türüne dönüştürülebilir ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)), ancak parametrelere gövdede erişilemez.
*  En yakın kapsayan anonim işlevin imzasında (varsa) belirtilen `ref` veya `out` parametreleri dışında, gövdenin bir `ref` veya `out` parametresine erişmesi için derleme zamanı hatası olur.
*  @No__t-0 türü bir struct türü olduğunda, gövdenin `this` ' e erişmesi için derleme zamanı hatası olur. Bu, erişimin açık (`this.x`) veya örtük (`x` ' in yapının örnek üyesi olduğu `x` ' de olduğu gibi) için geçerlidir. Bu kural yalnızca bu erişimi yasaklar ve üye aramanın yapının bir üyesi olup olmadığını etkilemez.
*  Gövde, anonim işlevin dış değişkenlerine ([dış değişkenler](expressions.md#outer-variables)) erişebilir. Bir dış değişkene erişim, *lambda_expression* veya *anonymous_method_expression* değerlendirildiği sırada etkin olan değişkenin örneğine başvuracaktır ([anonim işlev ifadelerinin değerlendirmesi](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Gövdede bir `goto` ifadesini, `break` ifadesini veya `continue` ifadesini içeren, hedefi gövdenin dışında veya kapsanan bir anonim işlevin gövdesinde olan bir derleme zamanı hatasıdır.
*  Gövdedeki bir `return` ifadesinde, kapsayan işlev üyesinden değil, en yakın kapsayan anonim işlevin çağrısından denetim döndürülür. @No__t-0 ifadesinde belirtilen bir ifade, en yakın kapsayan *lambda_expression* veya *anonymous_method_expression* 'in dönüştürüldüğü temsilci türünün veya ifade ağacı türünün dönüş türüne örtük olarak dönüştürülebilir olmalıdır ( [Anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)).

*Lambda_expression* veya *anonymous_method_expression*' nin değerlendirmesi ve çağrılması dışında bir anonim işlevin bloğunu yürütme yolu olup olmadığı açıkça belirtilmemiş olur. Özellikle, derleyici bir veya daha fazla adlandırılmış yöntemi ya da türü birleştirerek anonim bir işlev uygulamayı tercih edebilir. Bu tür birleştirilmiş öğelerin adları, derleyici kullanımı için ayrılmış bir biçimde olmalıdır.

### <a name="overload-resolution-and-anonymous-functions"></a>Aşırı yükleme çözümlemesi ve anonim işlevler

Bir bağımsız değişken listesindeki anonim işlevler tür çıkarımı ve aşırı yükleme çözümüne katılır. Tam kurallar için lütfen [tür çıkarımı](expressions.md#type-inference) ve [aşırı yükleme çözümlemesi](expressions.md#overload-resolution) ' ne bakın.

Aşağıdaki örnekte, anonim işlevlerin aşırı yükleme çözünürlüğünde etkileri gösterilmektedir.

```csharp
class ItemList<T>: List<T>
{
    public int Sum(Func<T,int> selector) {
        int sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }

    public double Sum(Func<T,double> selector) {
        double sum = 0;
        foreach (T item in this) sum += selector(item);
        return sum;
    }
}
```

@No__t-0 sınıfında iki `Sum` yöntemi vardır. Her biri, bir liste öğesinden toplanacak değeri çıkaran bir `selector` bağımsız değişkeni alır. Ayıklanan değer bir `int` ya da bir `double` olabilir ve elde edilen toplam değer aynı şekilde `int` veya `double` olur.

@No__t-0 yöntemleri, bir sırayla ayrıntı satırları listesinden toplamları hesaplamak için kullanılabilir.

```csharp
class Detail
{
    public int UnitCount;
    public double UnitPrice;
    ...
}

void ComputeSums() {
    ItemList<Detail> orderDetails = GetOrderDetails(...);
    int totalUnits = orderDetails.Sum(d => d.UnitCount);
    double orderTotal = orderDetails.Sum(d => d.UnitPrice * d.UnitCount);
    ...
}
```

@No__t-0 ' ı ilk çağrılışında, anonim işlev `d => d. UnitCount` ' nin hem `Func<Detail,int>` hem de `Func<Detail,double>` ile uyumlu olduğu için `Sum` yöntemlerinin her ikisi de geçerlidir. Ancak, `Func<Detail,int>` ' e dönüştürme işlemi `Func<Detail,double>` ' ye dönüşümünden daha iyi olduğundan aşırı yükleme çözümlemesi ilk `Sum` yöntemini seçer.

@No__t-0 ' ın ikinci çağrısında yalnızca ikinci `Sum` yöntemi geçerlidir çünkü `d => d.UnitPrice * d.UnitCount` anonim işlevi `double` türünde bir değer oluşturur. Bu nedenle, aşırı yükleme çözümlemesi bu çağrı için ikinci `Sum` yöntemini seçer.

### <a name="anonymous-functions-and-dynamic-binding"></a>Anonim işlevler ve dinamik bağlama

Anonim bir işlev, dinamik olarak bağlı bir işlemin alıcısı, bağımsız değişkeni veya işleneni olamaz.

### <a name="outer-variables"></a>Dış değişkenler

Kapsamı *lambda_expression* veya *anonymous_method_expression* içeren herhangi bir yerel değişken, değer parametresi veya parametre dizisine anonim işlevin ***dış değişkeni*** denir. Bir sınıfın örnek işlev üyesinde, `this` değeri bir değer parametresi olarak değerlendirilir ve işlev üyesi içinde bulunan herhangi bir anonim işlevin dış değişkenidir.

#### <a name="captured-outer-variables"></a>Yakalanan dış değişkenler

Bir dış değişkene adsız bir işlev tarafından başvuruluyorsa, dış değişken anonim işlev tarafından ***yakalanarak*** bildirilir. Genellikle, yerel bir değişkenin ömrü, ilişkili olduğu blok veya deyimin yürütmesi ile sınırlıdır ([yerel değişkenler](variables.md#local-variables)). Ancak, yakalanan bir dış değişkenin ömrü, anonim işlevden oluşturulan temsilci veya ifade ağacı çöp toplama için uygun hale gelene kadar en az genişletilir.

Örnekte
```csharp
using System;

delegate int D();

class Test
{
    static D F() {
        int x = 0;
        D result = () => ++x;
        return result;
    }

    static void Main() {
        D d = F();
        Console.WriteLine(d());
        Console.WriteLine(d());
        Console.WriteLine(d());
    }
}
```
`x` yerel değişkeni anonim işlev tarafından yakalanır ve `F` ' den döndürülen temsilci çöp toplama için uygun hale gelene kadar (programın çok sonuna kadar gerçekleşmeyen) en az 1 ' i @no__t. Anonymous işlevinin her çağrılması `x` ' ın aynı örneğinde çalışır, örneğin çıktısı şu şekilde olur:
```console
1
2
3
```

Yerel bir değişken veya bir değer parametresi anonim bir işlev tarafından yakalandıktan sonra, yerel değişken veya parametre artık sabit bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) olarak kabul edilmez, ancak bunun yerine taşınabilir bir değişken olarak kabul edilir. Bu nedenle, yakalanan bir dış değişkenin adresini alan `unsafe` kodu, önce değişkeni çözmek için `fixed` ifadesini kullanmalıdır.

Yakalanamayan bir değişkenin aksine, yakalanan bir yerel değişkenin aynı anda birden fazla yürütme iş parçacığına sunulabileceğini unutmayın.

#### <a name="instantiation-of-local-variables"></a>Yerel değişkenlerin örneklenmesi

Bir yerel değişken, yürütme değişkenin kapsamına girdiğinde ***örnek*** olarak değerlendirilir. Örneğin, aşağıdaki yöntem çağrıldığında, `x` yerel değişkeni, döngünün her yinelemesi için bir kez oluşturulur ve üç kez başlatılır.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Ancak, `x` ' ın bildirimini döngü dışına taşımak, `x` ' in tek bir örneklemesinde oluşur:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Yakalanmadığı zaman, bir yerel değişkenin hangi sıklıkta örneklendirildiğini tam olarak gözlemleyebilmenin bir yolu yoktur. örneklemelerin yaşam süreleri kopuk olduğundan, her örnekleme için aynı depolama konumunu kullanmak mümkündür. Ancak, anonim bir işlev yerel bir değişken yakaladığında, örnekleme etkileri görünür hale gelir.

Örnek
```csharp
using System;

delegate void D();

class Test
{
    static D[] F() {
        D[] result = new D[3];
        for (int i = 0; i < 3; i++) {
            int x = i * 2 + 1;
            result[i] = () => { Console.WriteLine(x); };
        }
        return result;
    }

    static void Main() {
        foreach (D d in F()) d();
    }
}
```
çıktıyı üretir:
```console
1
3
5
```

Ancak, `x` bildirimi döngü dışına taşındığında:
```csharp
static D[] F() {
    D[] result = new D[3];
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        result[i] = () => { Console.WriteLine(x); };
    }
    return result;
}
```
Çıktı:
```console
5
5
5
```

Bir for döngüsü bir yineleme değişkeni bildirirse, bu değişkenin kendisi döngünün dışında bildirildikleri kabul edilir. Bu nedenle, örnek yineleme değişkeninin kendisini yakalamak üzere değiştirilirse:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
yalnızca bir yineleme değişkeni örneği yakalanır, bu da çıktıyı üretir:
```console
3
3
3
```

Bazı yakalanan değişkenlerin paylaşılması için anonim işlev temsilcilerinin, hala diğerlerinin farklı örneklerine sahip olması mümkündür. Örneğin, `F` olarak değiştirilirse
```csharp
static D[] F() {
    D[] result = new D[3];
    int x = 0;
    for (int i = 0; i < 3; i++) {
        int y = 0;
        result[i] = () => { Console.WriteLine("{0} {1}", ++x, ++y); };
    }
    return result;
}
```
üç temsilci `x` ' ın aynı örneğini yakalar, ancak `y` ' in farklı örneklerini yakalar ve çıkış şu şekilde olur:
```console
1 1
2 1
3 1
```

Ayrı anonim işlevler, bir dış değişkenin aynı örneğini yakalayabilir. Örnekte:
```csharp
using System;

delegate void Setter(int value);

delegate int Getter();

class Test
{
    static void Main() {
        int x = 0;
        Setter s = (int value) => { x = value; };
        Getter g = () => { return x; };
        s(5);
        Console.WriteLine(g());
        s(10);
        Console.WriteLine(g());
    }
}
```
iki anonim işlev, `x` yerel değişkeninin aynı örneğini yakalar ve bu nedenle bu değişken ile "iletişim kurabilir". Örneğin çıktısı şu şekilde olur:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Anonim işlev ifadelerinin değerlendirmesi

@No__t-0 Anonim işlevinin her zaman doğrudan veya bir temsilci oluşturma @no__t ifadesinin yürütülmesi yoluyla, bir `D` veya `E` ifade ağacı türüne dönüştürülmesi gerekir. Bu dönüştürme, anonim işlev [dönüştürmeleri](conversions.md#anonymous-function-conversions)bölümünde açıklandığı gibi anonim işlevin sonucunu belirler.

## <a name="query-expressions"></a>Sorgu ifadeleri

***Sorgu ifadeleri*** , SQL ve XQuery gibi ilişkisel ve hiyerarşik sorgu dillerine benzer sorgular için dil ile tümleşik bir sözdizimi sağlar.

```antlr
query_expression
    : from_clause query_body
    ;

from_clause
    : 'from' type? identifier 'in' expression
    ;

query_body
    : query_body_clauses? select_or_group_clause query_continuation?
    ;

query_body_clauses
    : query_body_clause
    | query_body_clauses query_body_clause
    ;

query_body_clause
    : from_clause
    | let_clause
    | where_clause
    | join_clause
    | join_into_clause
    | orderby_clause
    ;

let_clause
    : 'let' identifier '=' expression
    ;

where_clause
    : 'where' boolean_expression
    ;

join_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression
    ;

join_into_clause
    : 'join' type? identifier 'in' expression 'on' expression 'equals' expression 'into' identifier
    ;

orderby_clause
    : 'orderby' orderings
    ;

orderings
    : ordering (',' ordering)*
    ;

ordering
    : expression ordering_direction?
    ;

ordering_direction
    : 'ascending'
    | 'descending'
    ;

select_or_group_clause
    : select_clause
    | group_clause
    ;

select_clause
    : 'select' expression
    ;

group_clause
    : 'group' expression 'by' expression
    ;

query_continuation
    : 'into' identifier query_body
    ;
```

Sorgu ifadesi `from` yan tümcesiyle başlar ve `select` veya `group` yan tümcesiyle biter. İlk `from` yan tümcesinin ardından sıfır veya daha fazla `from`, `let`, `where`, `join` veya `orderby` yan tümcesi gelebilir. Her `from` yan tümcesi, bir ***dizinin***öğelerinin üzerinde değişen bir ***Aralık değişkeni*** sunan bir üreticidir. Her `let` yan tümcesi, önceki Aralık değişkenlerine göre hesaplanan bir değeri temsil eden bir Aralık değişkeni sunar. Her `where` yan tümcesi, nesneleri sonuçtan dışlayan bir filtredir. Her `join` yan tümcesi, eşleşen çiftleri oluşturan, kaynak dizinin belirtilen anahtarlarını başka bir dizinin anahtarlarıyla karşılaştırır. Her `orderby` yan tümcesi öğeleri belirtilen ölçütlere göre yeniden sıralar. Son `select` veya `group` yan tümcesi, Aralık değişkenleri bakımından sonucun şeklini belirtir. Son olarak, bir `into` yan tümcesi, bir sorgunun sonuçlarını sonraki bir sorguda Oluşturucu olarak düşünerek "splice" için kullanılabilir.

### <a name="ambiguities-in-query-expressions"></a>Sorgu ifadelerinde belirsizlikleri

Sorgu ifadeleri, belirli bir bağlamda özel anlamı olan tanımlayıcılar içeren bir dizi "bağlamsal anahtar sözcük", yani. Bunlar özellikle `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, 0, 1 ve 2 ' dir. Bu tanımlayıcıların, anahtar sözcük veya basit adlar olarak karışık olarak kullanılması nedeniyle sorgu ifadelerinde belirsizlikleri kaçınmak için, bu tanımlayıcılar bir sorgu ifadesinin içinde herhangi bir yerde olduğunda anahtar sözcük olarak kabul edilir.

Bu amaçla, bir sorgu ifadesi "`from identifier`" ile başlayan ve "`;`", "`=`" veya "`,`" dışında herhangi bir belirteç tarafından başlayan ifadedir.

Bu sözcüklerin bir sorgu ifadesinde tanımlayıcı olarak kullanılabilmesi için, "`@`" ([tanımlayıcılar](lexical-structure.md#identifiers)) ön eki uygulanabilir.

### <a name="query-expression-translation"></a>Sorgu ifadesi çevirisi

Dil C# , sorgu ifadelerinin yürütme semantiğini belirtmez. Bunun yerine sorgu ifadeleri *sorgu ifade düzenine* ([sorgu ifade deseninin](expressions.md#the-query-expression-pattern)) bağlı olan yöntemlerin etkinleştirilmesinde çevrilir. Özellikle, sorgu ifadeleri `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy` ve 0 adlı yöntemlerin etkinleştirilmesine çevrilir. [Sorgu ifadesi](expressions.md#the-query-expression-pattern)düzeninde açıklandığı gibi, bu yöntemlerin belirli imzaları ve sonuç türlerini olması beklenir. Bu yöntemler, sorgulanan nesnenin örnek yöntemleri veya nesne dışındaki genişletme yöntemleri olabilir ve sorgunun gerçek yürütmesini uygular.

Sorgu ifadelerinden Yöntem etkinleştirmeleri 'e yapılan çeviri, herhangi bir tür bağlama veya aşırı yükleme çözümlemesi yapılmadan önce oluşan bir sözdizimsel eşleme olur. Çeviri, sözdizimsel olarak doğru bir şekilde garanti edilir, ancak anlamsal olarak doğru C# kodun üretilmesi garanti edilmez. Sorgu ifadelerinin çevirisinde, sonuçta elde edilen yöntem etkinleştirmeleri normal Yöntem etkinleştirmeleri olarak işlenir ve bu, örneğin Yöntemler yoksa, bağımsız değişkenlerin yanlış türleri varsa veya yöntemler geneldir ve Tür çıkarımı başarısız oluyor.

Bir sorgu ifadesi, daha fazla azaltmayı mümkün olana kadar, aşağıdaki Çeviriler tekrar tekrar uygulanarak işlenir. Çeviriler uygulama sırasıyla listelenir: her bölümde, yukarıdaki bölümlerdeki çevirilerin tüketilildiği ve tükendiğinde, bir bölümün aynı sorgu ifadesinin işlenmesinde daha sonra yeniden ziyaret edilmesinin varsayıldığı varsayılır.

Sorgu ifadelerinde Aralık değişkenlerine atamaya izin verilmez. Ancak bir C# uygulamaya bu kısıtlamayı her zaman zorlamamasına izin verilir, çünkü bu durum bazen burada sunulan sözdizimsel çeviri düzeninde mümkün olmayabilir.

Belirli Çeviriler, `*` tarafından belirtilen saydam tanımlayıcılarla Aralık değişkenleri ekler. Saydam tanımlayıcıların özel özellikleri, [saydam tanımlayıcılarla](expressions.md#transparent-identifiers)daha ayrıntılı bir şekilde ele alınmıştır.

#### <a name="select-and-groupby-clauses-with-continuations"></a>Devamlılıklarla seçim ve GroupBy yan tümceleri

Devam eden bir sorgu ifadesi
```csharp
from ... into x ...
```
çevrilir
```csharp
from x in ( from ... ) ...
```

Aşağıdaki bölümlerde yer alan Çeviriler, sorguların `into` devamlılıkları olmadığını varsayar.

Örnek
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
çevrilir
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
son çeviri
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Açık Aralık değişken türleri

Açıkça bir Aralık değişkeni türünü belirten `from` yan tümcesi
```csharp
from T x in e
```
çevrilir
```csharp
from x in ( e ) . Cast < T > ( )
```

Açıkça bir Aralık değişkeni türünü belirten `join` yan tümcesi
```csharp
join T x in e on k1 equals k2
```
çevrilir
```csharp
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Aşağıdaki bölümlerde yer alan Çeviriler, sorguların hiçbir açık Aralık değişken türüne sahip olmadığını varsayar.

Örnek
```csharp
from Customer c in customers
where c.City == "London"
select c
```
çevrilir
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
son çeviri
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Açık Aralık değişkeni türleri genel olmayan `IEnumerable` arabirimini uygulayan, ancak genel `IEnumerable<T>` arabirimi olmayan koleksiyonları sorgulamak için yararlıdır. Yukarıdaki örnekte, `customers` `ArrayList` türünde ise bu durum olacaktır.

#### <a name="degenerate-query-expressions"></a>Sorgu ifadeleri oluşturmayı kaldırma

Formun sorgu ifadesi
```csharp
from x in e select x
```
çevrilir
```csharp
( e ) . Select ( x => x )
```

Örnek
```csharp
from c in customers
select c
```
çevrilir
```csharp
customers.Select(c => c)
```

Bir bozuk sorgu ifadesi, kaynağın öğelerini önemli bir şekilde seçen bir ifadedir. Çevirinin daha sonraki bir aşaması, diğer çeviri adımları tarafından tanıtılan ve kaynakları kendi kaynağıyla değiştirerek çıkartılmış olan sorgu kaldırma işlemleri kaldırır. Sorgu ifadesinin sonucunun, sorgunun istemcisinin türünü ve kimliğini açığa çıkardığından, hiçbir şekilde kaynak nesnenin kendisi olduğundan emin olmak önemlidir. Bu nedenle bu adım, kaynakta `Select` ' ı açıkça çağırarak doğrudan kaynak kodunda yazılan sorguları kaldırır. Daha sonra, bu yöntemlerin kaynak nesnenin kendisini döndürmemesini sağlamak için `Select` ve diğer sorgu işleçleri uygulayıcılarına sahiptir.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Kimden, Let, WHERE, JOIN ve OrderBy yan tümceleri

İkinci bir `from` yan tümcesine ve ardından `select` yan tümcesine sahip bir sorgu ifadesi
```csharp
from x1 in e1
from x2 in e2
select v
```
çevrilir
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

İkinci bir `from` yan tümcesine sahip bir sorgu ifadesi ve ardından `select` yan tümcesi dışında bir öğe.

```csharp
from x1 in e1
from x2 in e2
...
```
çevrilir
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

@No__t-0 yan tümcesiyle bir sorgu ifadesi
```csharp
from x in e
let y = f
...
```
çevrilir
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

@No__t-0 yan tümcesiyle bir sorgu ifadesi
```csharp
from x in e
where f
...
```
çevrilir
```csharp
from x in ( e ) . Where ( x => f )
...
```

Bir `into` ve arkasından bir `select` yan tümcesi olmadan `join` yan tümcesiyle bir sorgu ifadesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
çevrilir
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

@No__t-1 olmayan `join` yan tümcesiyle bir `select` yan tümcesi dışında bir sorgu ifadesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
çevrilir
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

@No__t-1 ve ardından bir `select` yan tümcesi içeren `join` yan tümcesiyle bir sorgu ifadesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
çevrilir
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Bir `into` ve ardından `select` yan tümcesi dışında bir `join` yan tümcesiyle bir sorgu ifadesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
çevrilir
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

@No__t-0 yan tümcesiyle bir sorgu ifadesi
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
çevrilir
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Bir sıralama yan tümcesi `descending` yönü göstergesi belirtiyorsa, bunun yerine `OrderByDescending` veya `ThenByDescending` ' nin bir çağrılması üretilir.

Aşağıdaki Çeviriler `let`, `where`, `join` veya `orderby` yan tümceleri olmadığını ve her sorgu ifadesinde bir ilk `from` yan tümcesinin daha fazlasını olmadığını varsayar.

Örnek
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
çevrilir
```csharp
customers.
SelectMany(c => c.Orders,
     (c,o) => new { c.Name, o.OrderID, o.Total }
)
```

Örnek
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
çevrilir
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
son çeviri
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
Burada `x`, aksi durumda görünmeyen ve erişilemeyen derleyici tarafından oluşturulan bir tanıtıcıdır.

Örnek
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
çevrilir
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
son çeviri
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
Burada `x`, aksi durumda görünmeyen ve erişilemeyen derleyici tarafından oluşturulan bir tanıtıcıdır.

Örnek
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
çevrilir
```csharp
customers.Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c.Name, o.OrderDate, o.Total })
```

Örnek
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID into co
let n = co.Count()
where n >= 10
select new { c.Name, OrderCount = n }
```
çevrilir
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
son çeviri
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
`x` ve `y`, aksi durumda görünmeyen ve erişilemeyen derleyici tarafından oluşturulan tanımlayıcılardır.

Örnek
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
Son çeviriyi içerir
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>Yan tümceleri Seç

Formun sorgu ifadesi
```csharp
from x in e select v
```
çevrilir
```csharp
( e ) . Select ( x => v )
```
v 'nin x tanımlayıcısı olduğu durumlar dışında, çeviri yalnızca
```csharp
( e )
```

Örneğin:
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
yalnızca şuna çevrilir
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>GroupBy yan tümceleri

Formun sorgu ifadesi
```csharp
from x in e group v by k
```
çevrilir
```csharp
( e ) . GroupBy ( x => k , x => v )
```
v 'nin x tanımlayıcısı olduğu durumlar dışında, çeviri
```csharp
( e ) . GroupBy ( x => k )
```

Örnek
```csharp
from c in customers
group c.Name by c.Country
```
çevrilir
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Saydam tanımlayıcılar

Belirli Çeviriler `*` tarafından belirtilen ***saydam tanımlayıcılarla*** Aralık değişkenleri ekler. Saydam tanımlayıcılar uygun bir dil özelliği değildir; Bunlar yalnızca sorgu ifadesi çevirisi işleminde bir ara adım olarak mevcuttur.

Bir sorgu çevirisi bir saydam tanımlayıcıyı geçersiz kılar, ek çeviri adımları saydam tanımlayıcıyı anonim işlevlere ve anonim nesne başlatıcılarına yayar. Bu bağlamlarda saydam tanımlayıcılar aşağıdaki davranışa sahiptir:

*  Bir saydam tanımlayıcı, anonim bir işlevde parametre olarak gerçekleştiğinde, ilişkili anonim türün üyeleri anonim işlevin gövdesinde otomatik olarak kapsam içinde olur.
*  Saydam tanımlayıcısı olan bir üye kapsam içinde olduğunda, o üyenin üyeleri de kapsam içinde olur.
*  Bir saydam tanımlayıcı, anonim nesne başlatıcısında bir üye bildirimci olarak oluştuğunda, saydam tanımlayıcı içeren bir üye tanıtır.
*  Yukarıda açıklanan çeviri adımlarında, saydam tanımlayıcılar her zaman anonim türlerle birlikte tanıtılmıştır ve birden çok Aralık değişkenini tek bir nesnenin üyeleri olarak yakalama amacını taşıyan bir şekilde. C# Uygulamasının birden çok Aralık değişkenini gruplamak için anonim türlerden farklı bir mekanizma kullanmasına izin verilir. Aşağıdaki çeviri örnekleri, anonim türlerin kullanıldığını varsayar ve saydam tanımlayıcıların nasıl çevrilebilmesi gerektiğini gösterir.

Örnek
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
çevrilir
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

daha fazla çevrilmiş
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
saydam tanımlayıcılar silindiklerinde, eşittir
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
Burada `x`, aksi durumda görünmeyen ve erişilemeyen derleyici tarafından oluşturulan bir tanıtıcıdır.

Örnek
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
çevrilir
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
daha az
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
son çeviri
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID,
    (c, o) => new { c, o }).
Join(details, x => x.o.OrderID, d => d.OrderID,
    (x, d) => new { x, d }).
Join(products, y => y.d.ProductID, p => p.ProductID,
    (y, p) => new { y, p }).
Select(z => new { z.y.x.c.Name, z.y.x.o.OrderDate, z.p.ProductName })
```
`x`, `y` ve `z`, aksi durumda görünmeyen ve erişilemeyen derleyici tarafından oluşturulan tanımlayıcılardır.

### <a name="the-query-expression-pattern"></a>Sorgu ifadesi deseninin

***Sorgu ifadesi stili*** , sorgu ifadelerini desteklemek için uygulayabileceğiniz yöntemlerin bir modelini oluşturur. Sorgu ifadeleri, sözdizimsel eşleme yoluyla Yöntem etkinleştirmeleri 'e çevrildiği için, türlerin sorgu ifadesi deseninin nasıl uygulandığı konusunda önemli ölçüde esneklik vardır. Örneğin, iki aynı çağırma söz dizimini içerdiğinden ve Yöntemler, anonim işlevler her ikisine de dönüştürülebildiğinden temsilci veya ifade ağaçları isteyebildiğinden, düzenin yöntemleri örnek yöntemleri veya genişletme yöntemleri olarak uygulanabilir.

Sorgu ifadesi modelini destekleyen `C<T>` genel türünün önerilen şekli aşağıda gösterilmiştir. Parametre ve sonuç türleri arasındaki uygun ilişkileri göstermek için genel bir tür kullanılır, ancak genel olmayan türler için de düzeni uygulamak mümkündür.

```csharp
delegate R Func<T1,R>(T1 arg1);

delegate R Func<T1,T2,R>(T1 arg1, T2 arg2);

class C
{
    public C<T> Cast<T>();
}

class C<T> : C
{
    public C<T> Where(Func<T,bool> predicate);

    public C<U> Select<U>(Func<T,U> selector);

    public C<V> SelectMany<U,V>(Func<T,C<U>> selector,
        Func<T,U,V> resultSelector);

    public C<V> Join<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,U,V> resultSelector);

    public C<V> GroupJoin<U,K,V>(C<U> inner, Func<T,K> outerKeySelector,
        Func<U,K> innerKeySelector, Func<T,C<U>,V> resultSelector);

    public O<T> OrderBy<K>(Func<T,K> keySelector);

    public O<T> OrderByDescending<K>(Func<T,K> keySelector);

    public C<G<K,T>> GroupBy<K>(Func<T,K> keySelector);

    public C<G<K,E>> GroupBy<K,E>(Func<T,K> keySelector,
        Func<T,E> elementSelector);
}

class O<T> : C<T>
{
    public O<T> ThenBy<K>(Func<T,K> keySelector);

    public O<T> ThenByDescending<K>(Func<T,K> keySelector);
}

class G<K,T> : C<T>
{
    public K Key { get; }
}
```

Yukarıdaki yöntemler `Func<T1,R>` ve `Func<T1,T2,R>` genel temsilci türlerini kullanır, ancak parametre ve sonuç türlerinde aynı ilişkilerle diğer temsilci veya ifade ağacı türleri de aynı şekilde kullanılabilir.

@No__t-0 ve `O<T>` arasında önerilen ilişkiye dikkat edin. Bu, `ThenBy` ve `ThenByDescending` yöntemlerinin yalnızca bir `OrderBy` veya `OrderByDescending` sonucu üzerinde kullanılabilir olmasını sağlar. Ayrıca, her iç sıranın ek bir `Key` özelliği olduğu `GroupBy` ' ın (dizi) sonucu için önerilen şekle de dikkat edin.

@No__t-0 ad alanı, `System.Collections.Generic.IEnumerable<T>` arabirimini uygulayan herhangi bir tür için sorgu işleci deseninin bir uygulamasını sağlar.

## <a name="assignment-operators"></a>Atama işleçleri

Atama işleçleri bir değişkene, özelliğe, olaya veya Dizin Oluşturucu öğesine yeni bir değer atar.

```antlr
assignment
    : unary_expression assignment_operator expression
    ;

assignment_operator
    : '='
    | '+='
    | '-='
    | '*='
    | '/='
    | '%='
    | '&='
    | '|='
    | '^='
    | '<<='
    | right_shift_assignment
    ;
```

Atamanın sol işleneni, değişken olarak sınıflandırılmış bir ifade, özellik erişimi, Dizin Oluşturucu erişimi veya olay erişimi olmalıdır.

@No__t-0 işlecine ***basit atama işleci***denir. Sağ işlenenin değerini, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine atar. Basit atama işlecinin sol işleneni bir olay erişimi olmayabilir ( [alan benzeri olaylar](classes.md#field-like-events)bölümünde açıklananlar dışında). Basit atama işleci [basit atamada](expressions.md#simple-assignment)açıklanmıştır.

@No__t-0 işlecinden farklı atama işleçleri ***bileşik atama işleçleri***olarak adlandırılır. Bu işleçler iki işlenen üzerinde belirtilen işlemi gerçekleştirir ve ardından sonuçtaki değeri, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine atar. Bileşik atama işleçleri [Birleşik atama](expressions.md#compound-assignment)bölümünde açıklanmıştır.

Sol işlenen olarak bir olay erişim ifadesiyle `+=` ve `-=` işleçleri *olay atama işleçleri*olarak adlandırılır. Sol işlenen olarak bir olay erişimiyle hiçbir başka atama işleci geçerli değildir. Olay atama işleçleri [olay atamasında](expressions.md#event-assignment)açıklanmaktadır.

Atama işleçleri doğru ilişkilendirilebilir, yani işlemler sağdan sola gruplandırılır. Örneğin, `a = b = c` biçiminde bir ifade `a = (b = c)` olarak değerlendirilir.

### <a name="simple-assignment"></a>Basit atama

@No__t-0 işlecine basit atama işleci denir.

Basit bir atamanın sol işleneni `E.P` veya `E[Ei]` ' in `E` ' nin derleme zamanı türü `dynamic` ' ü içeriyorsa, atama dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, atama ifadesinin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, `E` ' in çalışma zamanı türüne göre çalışma zamanında gerçekleşmeyecektir.

Basit bir atamada, sağ işlenen, sol işlenenin türüne örtük olarak dönüştürülebilir bir ifade olmalıdır. İşlem, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine sağ işlenenin değerini atar.

Basit atama ifadesinin sonucu, sol işlenene atanan değerdir. Sonuç, sol işleneniyle aynı türe sahiptir ve her zaman bir değer olarak sınıflandırılır.

Sol işlenen bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

@No__t-0 formunun basit atamasının çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*  @No__t-0 bir değişken olarak sınıflandırıldıysanız:
   * `x` değişkeni üretmek için değerlendirilir.
   * `y` değerlendirilir ve gerekirse, örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla `x` türüne dönüştürülür.
   * @No__t-0 tarafından verilen değişken bir *reference_type*dizi öğesi ise, `y` için hesaplanan değerin, `x` ' ün bir öğesi olduğu dizi örneğiyle uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi yapılır. @No__t-0 `null` olduğunda denetim başarılı olur veya `y` tarafından başvurulan örneğin gerçek türünden, `x` içeren dizi örneğinin gerçek öğe türüne bir örtük başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) varsa. Aksi takdirde, bir `System.ArrayTypeMismatchException` oluşturulur.
   * @No__t-0 ' ın değerlendirmesinden ve dönüşümden elde edilen değer, `x` değerlendirmesi tarafından verilen konuma depolanır.
*  @No__t-0 bir özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldığında:
   * Örnek ifadesi (`x` `static` değilse) ve bağımsız değişken listesi (`x` ' in bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `set` erişimci çağrısında kullanılır.
   * `y` değerlendirilir ve gerekirse, örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla `x` türüne dönüştürülür.
   * @No__t-1 ' in `set` erişimcisi `value` bağımsız değişkeni olarak `y` için hesaplanan değerle çağrılır.

Dizi birlikte değişim kuralları ([dizi Kovaryans](arrays.md#array-covariance)), `A[]` dizi türünde bir değere izin verir. bu, `B` ' ten `A` ' e kadar örtük bir başvuru dönüştürmesi sağlanmış `B[]` dizi türünün bir örneğine başvuru sağlar. Bu kurallar nedeniyle, bir *reference_type* dizi öğesine atama, atanmakta olan değerin dizi örneğiyle uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi gerektirir. Örnekte
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
Son atama bir `System.ArrayTypeMismatchException` oluşturulmasına neden olur çünkü bir `ArrayList` örneği bir `string[]` öğesinde depolanamaz.

Bir *struct_type* içinde belirtilen bir özellik veya Dizin Oluşturucu bir atamanın hedefi olduğunda, özellik veya Dizin Oluşturucu erişimi ile ilişkili örnek ifadesi bir değişken olarak sınıflandırılmalıdır. Örnek ifadesi bir değer olarak sınıflandırılasiyse, bağlama zamanı hatası oluşur. [Üye erişimi](expressions.md#member-access)nedeniyle aynı kural alanlar için de geçerli olur.

Bildirimler verildi:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int X {
        get { return x; }
        set { x = value; }
    }

    public int Y {
        get { return y; }
        set { y = value; }
    }
}

struct Rectangle
{
    Point a, b;

    public Rectangle(Point a, Point b) {
        this.a = a;
        this.b = b;
    }

    public Point A {
        get { return a; }
        set { a = value; }
    }

    public Point B {
        get { return b; }
        set { b = value; }
    }
}
```
Örnekte
```csharp
Point p = new Point();
p.X = 100;
p.Y = 100;
Rectangle r = new Rectangle();
r.A = new Point(10, 10);
r.B = p;
```
`p` ve `r` değişkenleri olduğundan `p.X`, `p.Y`, `r.A` ve `r.B` ' e yönelik atamalara izin verilir. Ancak, örnekte
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
`r.A` ve `r.B` değişkenleri olmadığından atamalar geçersiz.

### <a name="compound-assignment"></a>Bileşik atama

Bileşik atamanın sol işleneni `E.P` veya `E[Ei]` ' in `E` ' nin derleme zamanı türü `dynamic` ' ü içeriyorsa, atama dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, atama ifadesinin derleme zamanı türü `dynamic` ' dır ve aşağıda açıklanan çözüm, `E` ' in çalışma zamanı türüne göre çalışma zamanında gerçekleşmeyecektir.

@No__t-0 biçiminde bir işlem, işlem @no__t yazılmış gibi ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanarak işlenir. Ni

*  Seçili işlecin dönüş türü örtük olarak `x` türüne dönüştürüleiyorsa, işlem `x = x op y` olarak değerlendirilir, ancak `x` yalnızca bir kez değerlendirilir.
*  Aksi takdirde, seçilen işleç önceden tanımlanmış bir işleçse, seçili işlecin dönüş türü açık olarak `x` türüne dönüştürülebilir ve `y` örtük olarak `x` türüne dönüştürülebilir ve işleç bir kaydırma işleçtir sonra işlem `x = (T)(x op y)` olarak değerlendirilir; burada `T` `x` türüdür, ancak `x` yalnızca bir kez değerlendirilir.
*  Aksi takdirde, bileşik atama geçersizdir ve bir bağlama zamanı hatası oluşur.

"Yalnızca bir kez değerlendirilen" terimi, `x op y` ' nın değerlendirmesinde, `x` ' in yapısal ifadelerinin sonuçlarının geçici olarak kaydedildiği ve `x` ' ye atamasını gerçekleştirirken yeniden kullanıldığı anlamına gelir. Örneğin, `A` ' in `int[]` ' yi döndüren bir yöntem olduğu ve `B` ve `C` ' ün `int` döndüren yöntemleri @no__t, yöntemler yalnızca bir kez çağrılır; `A`, `B`, `C`.

Bileşik atamanın sol işleneni bir özellik erişimi veya Dizin Oluşturucu erişimi olduğunda, özelliğin veya dizin oluşturucunun hem `get` erişimcisi hem de bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Yukarıdaki ikinci kural, bazı bağlamlarda `x op= y` @no__t olarak değerlendirilmesine izin verir. Kural, sol işlenen `sbyte`, `byte`, `short`, `ushort` veya `char` türünde olduğunda, önceden tanımlanmış işleçlerin Bileşik işleçler olarak kullanılabilmesi için vardır. Her iki bağımsız değişken de bu türlerden birinde olsa da, önceden tanımlanmış işleçler, [ikili sayısal promosyonlar](expressions.md#binary-numeric-promotions)içinde açıklandığı gibi `int` türünde bir sonuç üretir. Bu nedenle, bir dönüştürme olmadan sonucu sol işlenene atamak mümkün olmaz.

Önceden tanımlanmış işleçler için kuralın sezgisel etkisi, `x op y` ve `x = y` ' nin her ikisi de izin verildiğinde `x op= y` ' dır. Örnekte
```csharp
byte b = 0;
char ch = '\0';
int i = 0;

b += 1;             // Ok
b += 1000;          // Error, b = 1000 not permitted
b += i;             // Error, b = i not permitted
b += (byte)i;       // Ok

ch += 1;            // Error, ch = 1 not permitted
ch += (char)1;      // Ok
```
her hatanın sezgisel olmasının nedeni, karşılık gelen basit atamanın de hata olması olabilir.

Bu ayrıca bileşik atama işlemlerinin yükseltilmemiş işlemlerini desteklediği anlamına gelir. Örnekte
```csharp
int? i = 0;
i += 1;             // Ok
```
yükseltilmemiş işleci `+(int?,int?)` kullanılır.

### <a name="event-assignment"></a>Olay ataması

@No__t-0 veya `-=` işlecinin sol işleneni bir olay erişimi olarak sınıflandırıldığında, ifade aşağıdaki gibi değerlendirilir:

*  Olay erişiminin, varsa örnek ifadesi değerlendirilir.
*  @No__t-0 veya `-=` işlecinin sağ işleneni değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla sol işlenenin türüne dönüştürülür.
*  Etkinliğin olay erişimcisi, değerlendirmeden sonra ve gerekirse dönüştürme işlemi tamamlandıktan sonra bağımsız değişken listesiyle çağrılır. İşleç `+=` ise, `add` erişimcisi çağrılır; işleç `-=` ise, `remove` erişimcisi çağrılır.

Olay atama ifadesi bir değer vermez. Bu nedenle, bir olay atama ifadesi yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında geçerlidir.

## <a name="expression"></a>İfade

Bir *ifade* , bir *non_assignment_expression* ya da *atamadır*.

```antlr
expression
    : non_assignment_expression
    | assignment
    ;

non_assignment_expression
    : conditional_expression
    | lambda_expression
    | query_expression
    ;
```

## <a name="constant-expressions"></a>Sabit ifadeler

*Constant_expression* , derleme zamanında tam olarak değerlendirilebilen bir ifadedir.

```antlr
constant_expression
    : expression
    ;
```

Sabit bir ifade `null` sabit değeri ya da aşağıdaki türlerden birine sahip bir değer olmalıdır: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, 0, 1, 2, 3, 4 , 5 veya herhangi bir numaralandırma türü. Sabit ifadelerde yalnızca aşağıdaki yapılara izin verilir:

*  Değişmez değerler (`null` sabit değeri dahil).
*  Sınıf ve yapı türlerinin `const` üyelerine başvurular.
*  Numaralandırma türlerinin üyelerine başvurular.
*  @No__t-0 parametrelerine veya yerel değişkenlere başvurular
*  Kendi sabit ifadeleri olan parantezli alt ifadeler.
*  Hedef türü yukarıda listelenen türlerden biri olarak verilen atama ifadeleri.
*  `checked` ve `unchecked` ifadeleri
*  Varsayılan değer ifadeleri
*  NameOf ifadeleri
*  Önceden tanımlı `+`, `-`, `!` ve `~` Birli İşleçler.
*  Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, 0, 1, 2, 3, 4, 5 ve 6 Yukarıda listelenen tür.
*  @No__t-0 koşullu işleci.

Sabit ifadelerde aşağıdaki Dönüştürmelere izin verilir:

*  Kimlik dönüştürmeleri
*  Sayısal Dönüşümler
*  Sabit Listesi dönüştürmeleri
*  Sabit ifade dönüştürmeleri
*  Örtük ve açık başvuru dönüştürmeleri, dönüştürmelerin kaynağı null değeri değerlendiren sabit bir ifadedir.

Null olmayan değerlerin kutulamayı, kutudan çıkarma ve örtük başvuru dönüştürmelerine dahil diğer dönüştürmeler sabit ifadelerde kullanılamaz. Örneğin:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
bir paketleme dönüştürmesi gerektiğinden, ı başlatması bir hatadır. Null olmayan bir değerden örtük bir başvuru dönüştürmesi gerektiğinden Str başlatma bir hatadır.

Bir ifade yukarıda listelenen gereksinimleri karşılaışında, ifade derleme zamanında değerlendirilir. Bu, ifade sabit olmayan yapılar içeren daha büyük bir ifadenin alt ifadesi olsa da geçerlidir.

Sabit ifadelerin derleme zamanı değerlendirmesi, sabit olmayan ifadelerin çalışma zamanı değerlendirmesiyle aynı kuralları kullanır, ancak çalışma zamanı değerlendirmesinin bir özel durum oluşturması, derleme zamanı değerlendirmesinin de bir derleme zamanı hatası oluşmasına neden olması gerekir.

Sabit bir ifade `unchecked` bağlamına açık bir şekilde yerleştirilmediği için, ifadenin derleme zamanı değerlendirmesi sırasında integral türü aritmetik işlemlerde ve dönüştürmelerde oluşan taşmalar her zaman derleme zamanı hatalarına neden olur ([sabit ifadeler](expressions.md#constant-expressions)).

Sabit ifadeler aşağıda listelenen bağlamlarda oluşur. Bu bağlamlarda, bir ifade derleme zamanında tam olarak değerlendirilemez bir derleme zamanı hatası oluşur.

*  Sabit bildirimler ([sabitler](classes.md#constants)).
*  Numaralandırma üyesi bildirimleri ([enum üyeleri](enums.md#enum-members)).
*  Biçimsel parametre listelerinin varsayılan bağımsız değişkenleri ([Yöntem parametreleri](classes.md#method-parameters))
*  `switch` ifadesinin `case` etiketleri ([Switch bildirisi](statements.md#the-switch-statement)).
*  `goto case` deyimleri ([goto deyimi](statements.md#the-goto-statement)).
*  Bir Başlatıcı içeren bir dizi oluşturma ifadesinde ([dizi oluşturma ifadelerinde](expressions.md#array-creation-expressions)) boyut uzunlukları.
*  Öznitelikler ([öznitelikler](attributes.md)).

Örtük bir sabit ifade dönüştürmesi ([örtük sabit ifade dönüştürmeleri](conversions.md#implicit-constant-expression-conversions)), `int` türündeki sabit bir ifadenin `sbyte`, `byte`, `short`, `ushort`, `uint` ya da `ulong` ' ye dönüştürülmesini sağlar ve bu değer Sabit ifade, hedef türünün aralığı içinde.

## <a name="boolean-expressions"></a>Boole ifadeleri

*Boolean_expression* , `bool`; türünde bir sonuç veren bir ifadedir. Aşağıda belirtildiği gibi, belirli bağlamlarda doğrudan veya `operator true` uygulaması aracılığıyla.

```antlr
boolean_expression
    : expression
    ;
```

Bir *if_statement* ([IF deyimi](statements.md#the-if-statement)), *while_statement* ([while deyimi](statements.md#the-while-statement)), *do_statement* ([Do deyimi](statements.md#the-do-statement)) veya *for_statement* 'nin koşullu ifadesini denetleme ([için Bildiri](statements.md#the-for-statement)) bir *Boolean_expression*. @No__t-0 işlecinin ([koşullu işleç](expressions.md#conditional-operator)) denetim koşullu ifadesi, *Boolean_expression*ile aynı kurallara uyar, ancak işleç önceliğin nedenleri bir *conditional_or_expression*olarak sınıflandırıldı.

*Boolean_expression* `E`, aşağıdaki gibi `bool` türünde bir değer üretebilmek için gereklidir:

*  @No__t-0 ' ı örtük olarak `bool` ' e dönüştürülebiliyorsanız, örtük dönüştürme uygulanmış çalışma zamanında.
*  Aksi halde, birli operatör aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)), `true` @no__t işlecinin en iyi bir en iyi uygulamasını bulmak için kullanılır ve bu uygulama çalışma zamanında uygulanır.
*  Böyle bir işleç bulunmazsa, bir bağlama zamanı hatası oluşur.

[Veritabanı Boole türünde](structs.md#database-boolean-type) `DBBool` yapı türü, `operator true` ve `operator false` uygulayan bir türe örnek sağlar.
