---
ms.openlocfilehash: f61039abd6bd557ac0ea625e6aac1c8bafa57b02
ms.sourcegitcommit: e134bb7058e9848120b93b345f96d6ac0cb8c815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "71704082"
---
# <a name="expressions"></a>{1&gt;İfadeler&lt;1}

İfade, işleç ve işlenen dizisidir. Bu bölümde sözdizimi, işlenenler ve işleçler değerlendirmesi sırası ve ifadelerin anlamı tanımlanmaktadır.

## <a name="expression-classifications"></a>İfade sınıflandırmaları

Bir ifade, aşağıdakilerden biri olarak sınıflandırıldı:

*  Bir değer. Her değerin ilişkili bir türü vardır.
*  Bir değişken. Her değişkenin bir ilişkili türü vardır, yani bu değişkenin belirtilen türü.
*  Ad alanı. Bu sınıflandırmayla bir ifade, yalnızca bir *member_access* ([üye erişimi](expressions.md#member-access)) sol tarafında görünür. Diğer bir bağlamda, ad alanı olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.
*  Bir tür. Bu sınıflandırmayla bir ifade, yalnızca bir *member_access* ([üye erişimi](expressions.md#member-access)) veya `as` işleci ([as işleci](expressions.md#the-as-operator)), `is` işleci ([IS işleci](expressions.md#the-is-operator)) veya `typeof` işleci ([typeof işleci](expressions.md#the-typeof-operator)) için bir işlenen olarak yer alabilir. Başka bir bağlamda, tür olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.
*  Bir üye aramasının ([üye arama](expressions.md#member-lookup)) sonucu olan aşırı yüklenmiş yöntemler kümesi olan bir yöntem grubu. Bir yöntem grubunun ilişkili bir örnek ifadesi ve ilişkili bir tür bağımsız değişkeni listesi olabilir. Bir örnek yöntemi çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek haline gelir. Bir yöntem grubuna bir *invocation_expression* ([çağırma ifadelerinde](expressions.md#invocation-expressions)), bir *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) ve bir bir bir işleç işlecinin sol tarafı olarak izin verilir ve örtülü olarak uyumlu bir temsilci türüne dönüştürülebilir ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)). Diğer bir bağlamda, Yöntem grubu olarak sınıflandırılan bir ifade derleme zamanı hatasına neden olur.
*  Null bir sabit değer. Bu sınıflandırmayla bir ifade, örtük olarak bir başvuru türüne veya null yapılabilir türe dönüştürülebilir.
*  Anonim bir işlev. Bu sınıflandırmayla bir ifade, örtülü olarak uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürülebilir.
*  Özellik erişimi. Her özellik erişiminin, özelliğin türü olarak ilişkili bir türü vardır. Ayrıca, özellik erişiminin ilişkili bir örnek ifadesi olabilir. Örnek özellik erişiminin bir erişimcisi (`get` veya `set` bloğu) çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek haline gelir.
*  Bir olay erişimi. Her olay erişiminde ilişkili bir tür bulunur, bu olay türü olayın türüdür. Ayrıca, bir olay erişiminin ilişkili bir örnek ifadesi olabilir. `+=` ve `-=` işleçlerinin ([olay atama](expressions.md#event-assignment)) sol tarafında bir olay erişimi görünebilir. Diğer bir bağlamda, olay erişimi olarak sınıflandırılan bir ifade, derleme zamanı hatasına neden olur.
*  Dizin Oluşturucu erişimi. Her Dizin Oluşturucu erişiminin ilişkili bir türü vardır, yani dizin oluşturucunun öğe türü. Ayrıca, Dizin Oluşturucu erişiminin ilişkili bir örnek ifadesi ve ilişkili bağımsız değişken listesi vardır. Bir Dizin Oluşturucu erişiminin bir erişimcisi (`get` veya `set` bloğu) çağrıldığında, örnek ifadesinin değerlendirilme sonucu `this` ([Bu erişim](expressions.md#this-access)) tarafından temsil edilen örnek olur ve bağımsız değişken listesinin değerlendirilme sonucu, çağrının parametre listesi olur.
*  Hiçbir şey. Bu, ifade `void`dönüş türü olan bir yöntemin çağrılışında oluşur. Nothing olarak sınıflandırılan bir ifade yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında geçerlidir.

Bir ifadenin nihai sonucu hiçbir şekilde bir ad alanı, tür, Yöntem grubu veya olay erişimdir. Bunun yerine, yukarıda belirtildiği gibi, bu ifade kategorileri yalnızca belirli bağlamlarda izin verilen ara yapılardır.

Bir özellik erişimi veya Dizin Oluşturucu erişimi, *Get erişimcisinin* veya *set erişimcisinin*çağrılması gerçekleştirerek her zaman bir değer olarak yeniden sınıflanır. Belirli erişimci, özelliğin veya dizin oluşturucunun erişim bağlamı tarafından belirlenir: erişim bir atamanın hedefi ise, *küme erişimcisi* yeni bir değer ([basit atama](expressions.md#simple-assignment)) atamak için çağrılır. Aksi takdirde, *get erişimcisi* geçerli değeri ([ifadelerin değerleri](expressions.md#values-of-expressions)) almak için çağrılır.

### <a name="values-of-expressions"></a>İfadelerin değerleri

Bir ifadeyi içeren yapıların çoğu sonunda ifadenin bir ***değeri***belirtmek için gereklidir. Bu gibi durumlarda, gerçek ifade bir ad alanı, bir tür, bir yöntem grubu veya hiçbir şey ifade ediyorsa, derleme zamanı hatası oluşur. Ancak, ifade bir özellik erişimi, Dizin Oluşturucu erişimi veya bir değişken ise, özelliğin, dizin oluşturucunun veya değişkenin değeri örtük olarak yerine kullanılır:

*  Bir değişkenin değeri, değişken tarafından tanımlanan depolama konumunda şu anda depolanan değer olacaktır. Değerin alınabilmesi için bir değişken kesin olarak atanmış ([kesin atama](variables.md#definite-assignment)) olarak kabul edilmelidir, aksi takdirde bir derleme zamanı hatası oluşur.
*  Özellik erişim ifadesinin değeri, özelliğin *get erişimcisi* çağrılarak elde edilir. Özelliğin *get erişimcisi*yoksa, bir derleme zamanı hatası oluşur. Aksi takdirde, bir işlev üye çağrısı ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir ve çağrının sonucu özellik erişim ifadesinin değeri olur.
*  Dizin Oluşturucu erişim ifadesinin değeri, dizin oluşturucunun *get erişimcisi* çağrılarak elde edilir. Dizin oluşturucunun *get erişimcisi*yoksa, bir derleme zamanı hatası oluşur. Aksi takdirde, Dizin Oluşturucu erişim ifadesiyle ilişkili bağımsız değişken listesiyle bir işlev üye çağrısı ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir ve çağrının sonucu Dizin Oluşturucu erişim ifadesinin değeri olur.

## <a name="static-and-dynamic-binding"></a>Statik ve dinamik bağlama

Bileşen ifadelerinin türü veya değerine (bağımsız değişkenler, işlenenler, alıcılar) göre bir işlemin anlamını belirleme işlemi genellikle ***bağlama***olarak adlandırılır. Örneğin, bir yöntem çağrısının anlamı alıcı ve bağımsız değişkenlerin türüne göre belirlenir. Bir işlecin anlamı, işlenenlerinin türüne göre belirlenir.

C# Bir işlemin anlamı genellikle derleme zamanında, bileşen ifadelerinin derleme zamanı türüne göre belirlenir. Benzer şekilde, bir ifade hata içeriyorsa, hata algılanır ve derleyici tarafından raporlanır. Bu yaklaşım ***statik bağlama***olarak bilinir.

Ancak, bir ifade dinamik bir ifadesiyse (yani türü `dynamic`) Bu, içinde yer aldığı herhangi bir bağlamanın, derleme zamanında sahip olduğu tür yerine çalışma zamanı türüne (çalışma zamanında gösterdiği nesnenin gerçek türü) bağlı olması gerektiğini belirtir. Bu nedenle, bu tür bir işlemin bağlanması, programın çalışması sırasında işlemin gerçekleştirileceği zamana kadar ertelenir. Bu, ***dinamik bağlama***olarak adlandırılır.

Bir işlem dinamik olarak bağlandığında, derleyici tarafından çok az veya hiçbir denetim yapılmaz. Çalışma zamanı bağlama başarısız olursa, hatalar çalışma zamanında özel durum olarak bildirilir.

' De C# aşağıdaki işlemler bağlamaya tabidir:

*  Üye erişimi: `e.M`
*  Yöntem çağırma: `e.M(e1, ..., eN)`
*  Temsilci çağrısı:`e(e1, ..., eN)`
*  Öğe erişimi: `e[e1, ..., eN]`
*  Nesne oluşturma: `new C(e1, ..., eN)`
*  Aşırı yüklenmiş Birli İşleçler: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  Aşırı yüklenmiş ikili işleçler: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<`, `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Atama işleçleri: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
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

İlk iki çağrı statik olarak bağlanmıştır: `Console.WriteLine` aşırı yüklemesi, bağımsız değişkeninin derleme zamanı türüne göre çekilir. Bu nedenle, bağlama zamanı derleme zamanı olur.

Üçüncü çağrı dinamik olarak bağlı: `Console.WriteLine` aşırı yüklemesi, bağımsız değişkeninin çalışma zamanı türüne göre çekilir. Bağımsız değişken dinamik bir ifade olduğu için bu durum, derleme zamanı türü `dynamic`. Bu nedenle, üçüncü çağrının bağlama zamanı çalışma zamanı olur.

### <a name="dynamic-binding"></a>Dinamik bağlama

Dinamik bağlamanın amacı, C# programların ***dinamik nesnelerle***etkileşime geçmesini sağlamaktır, yani C# tür sisteminin normal kurallarını takip etmez. Dinamik nesneler, farklı türlerde sistemlerle diğer programlama dillerinden nesneler olabilir veya farklı işlemler için kendi bağlama semantiğini uygulamak üzere programlı bir şekilde kurulum olan nesneler olabilir.

Dinamik bir nesnenin kendi semantiğini uyguladığı mekanizma uygulama tanımlı ' dır. Verilen bir arabirim (yeniden tanımlanmış bir uygulama), dinamik nesneler tarafından özel semantiklere sahip oldukları C# çalışma zamanına işaret etmek için uygulanır. Bu nedenle, dinamik bir nesne üzerindeki her bir işlem dinamik olarak bağlandığında, bu belgede belirtiler C# yerine kendi bağlama semantiğinin yerine geçer.

Dinamik bağlamanın amacı dinamik nesnelerle birlikte çalıştırılmasına izin verirken, C# dinamik olmasına bakılmaksızın tüm nesnelerde dinamik bağlamaya izin verir. Bu, dinamik nesnelerin daha yumuşak bir şekilde tümleştirilmesini sağlar, çünkü bunlar üzerindeki işlemlerin sonuçları dinamik nesneler olmayabilir, ancak hala derleme zamanında programcıya bilinmeyen bir tür olabilir. Ayrıca dinamik bağlama, hiçbir nesne dahil, dinamik nesneler olmasa bile hataya açık olan yansıma tabanlı kodu ortadan kaldırmaya yardımcı olabilir.

Aşağıdaki bölümlerde, dinamik bağlama uygulandığında, her bir yapı için, dinamik bağlama uygulandığında, derleme zaman denetimi (varsa), varsa derleme zamanı ve ifade sınıflandırmasının ne olduğu ile ilgili olarak açıklanır.

### <a name="types-of-constituent-expressions"></a>Anayent ifadelerin türleri

Bir işlem statik olarak bağlandığında, bir bileşen ifadesinin türü (örn. bir alıcı, bir bağımsız değişken, bir dizin veya işlenen), her zaman bu ifadenin derleme zamanı türü olarak değerlendirilir.

Bir işlem dinamik olarak bağlandığında, bileşen ifadesinin türü, bileşen ifadesinin derleme zamanı türüne bağlı olarak farklı yollarla belirlenir:

*  Derleme zamanı türü `dynamic` yapısal ifadesi, ifadenin çalışma zamanında değerlendirilen gerçek değerin türüne sahip olarak kabul edilir
*  Derleme zamanı türü bir tür parametresi olan bir anayent ifadesi tür parametresinin çalışma zamanında bağlı olduğu türe sahip olarak kabul edilir
*  Aksi halde, oluşturan ifade derleme zamanı türüne sahip olarak kabul edilir.

## <a name="operators"></a>İşleçler

İfadeler, ***işlenenler*** ve ***işleçlerden***oluşturulur. Bir ifadenin işleçleri, işlenenlerin hangi işlemleri uygulanacağını gösterir. Operatör örnekleri arasında `+`, `-`, `*`, `/`ve `new`sayılabilir. İşlenenlerin örnekleri, sabit değerleri, alanları, yerel değişkenleri ve ifadeleri içerir.

Üç tür işleç vardır:

*  Birli İşleçler. Birli İşleçler bir işlenen alır ve ön ek gösterimini (örneğin, `--x`) veya sonek gösterimini (`x++`gibi) kullanır.
*  İkili işleçler. İkili işleçler iki işlenen alır ve tüm yanlışın kullanımı (örneğin, `x + y`).
*  Üçlü işleç. Yalnızca bir üçlü operatör, `?:`var; üç işlenen alır ve geçersiz kılma gösterimi (`c ? x : y`) kullanır.

Bir ifadedeki işleçlerin değerlendirilme sırası, işleçlerin ***Öncelik*** ve ***ilişkilendirilebilirliği*** ([işleç önceliği ve ilişkilendirilebilirlik](expressions.md#operator-precedence-and-associativity)) tarafından belirlenir.

Bir ifadedeki işlenenler soldan sağa değerlendirilir. Örneğin, `F(i) + G(i++) * H(i)``F` yöntemi, eski `i`değeri kullanılarak çağrılır, yöntem `G` eski `i`değeriyle çağrılır ve son olarak, yöntem `H` yeni `i`değeri ile çağırılır. Bu, ' den farklıdır ve işleç önceliğine ilgisiz değildir.

Bazı işleçler ***aşırı***yüklenebilir. İşleç aşırı yüklemesi, Kullanıcı tanımlı operatör uygulamalarının bir veya her ikisinin de Kullanıcı tanımlı sınıf veya yapı türünde ([operatör aşırı yüklemesi](expressions.md#operator-overloading)) olduğu işlemler için belirtilmesine izin verir.

### <a name="operator-precedence-and-associativity"></a>İşleç önceliği ve ilişkilendirilebilirlik

Bir ifade birden çok işleç içerdiğinde, işleçlerin ***önceliği*** ayrı işleçlerin değerlendirilme sırasını denetler. Örneğin, `*` işleci ikili `+` işlecinden daha yüksek önceliğe sahip olduğundan ifade `x + y * z` `x + (y * z)` olarak değerlendirilir. Bir işlecin önceliği, ilişkili dilbilgisi üretiminin tanımıyla belirlenir. Örneğin, bir *additive_expression* , `+` veya `-` işleçleriyle ayrılan *multiplicative_expression*s dizisinden oluşur. böylece `+` ve `-` işleçleri `*`, `/`ve `%` işleçlerinden daha düşük önceliğe sahiptir.

Aşağıdaki tablo, en yüksekten en düşüğe öncelik sırasına göre tüm işleçleri özetler:

| __Bölüm__                                                                                   | __Kategori__                | __İşleçler__ | 
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

*  Atama işleçleri ve null birleşim işleci dışında, tüm ikili işleçler ***sola ilişkilendirilebilir***, yani işlemler soldan sağa yapılır. Örneğin, `x + y + z` `(x + y) + z`olarak değerlendirilir.
*  Atama işleçleri, null birleşim işleci ve koşullu işleç (`?:`), ***doğru ilişkilendirilebilir***, yani işlemler sağdan sola yapılır. Örneğin, `x = y = z` `x = (y = z)`olarak değerlendirilir.

Öncelik ve ilişkilendirilebilirlik, parantezler kullanılarak denetlenebilir. Örneğin, `x + y * z` önce `y` `z` ile çarpar ve sonucu `x`ekler, ancak `(x + y) * z` önce `x` ve `y` ekler ve sonra sonucu `z`ile çarpar.

### <a name="operator-overloading"></a>İşleç aşırı yüklemesi

Tüm birli ve ikili işleçlerin, herhangi bir ifadede otomatik olarak kullanılabilen önceden tanımlanmış uygulamaları vardır. Önceden tanımlanmış uygulamalara ek olarak, Kullanıcı tanımlı uygulamalar sınıflar ve yapılar ([işleçler](classes.md#operators)) `operator` bildirimleri dahil ederek tanıtılamaz. Kullanıcı tanımlı operatör uygulamaları, her zaman önceden tanımlanmış operatör uygulamalarından önceliklidir: yalnızca geçerli bir Kullanıcı tanımlı operatör uygulaması yoksa, [birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)bölümünde açıklandığı gibi önceden tanımlanmış operatör uygulamaları göz önünde bulundurulacaktır.

***Fazla yüklenebilir Birli İşleçler*** şunlardır:
```csharp
+   -   !   ~   ++   --   true   false
```

`true` ve `false` açıkça ifadelerde kullanılmasa da (ve bu nedenle, [işleç önceliği ve ilişkilendirilebilirliği](expressions.md#operator-precedence-and-associativity)içindeki öncelik tablosuna dahil edilmez), birkaç ifade bağlamlarında çağrıldıklarından işleçler kabul edilir: Boolean Ifadeleri ([Boolean ifadeleri](expressions.md#boolean-expressions)) ve koşullu ([koşullu işleç](expressions.md#conditional-operator)) ve koşullu mantıksal işleçler (koşullu[mantıksal işleçler](expressions.md#conditional-logical-operators)) içeren ifadeler.

***Fazla yüklenebilir ikili işleçler*** şunlardır:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Yalnızca yukarıda listelenen operatörler aşırı yüklenebilir. Özellikle, üye erişimi, yöntem çağırma veya `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`ve `is` işleçlerini aşırı yüklemek mümkün değildir.

İkili işleç aşırı yüklendiğinde, karşılık gelen atama işleci de dolaylı olarak aşırı yüklenmiştir. Örneğin, işleç `*` aşırı yüklemesi aynı zamanda `*=`işlecinin aşırı yüküne sahiptir. Bu, [bileşik atamada](expressions.md#compound-assignment)daha ayrıntılı olarak açıklanmıştır. Atama işlecinin kendisinin (`=`) aşırı yüklenemez olduğunu unutmayın. Atama her zaman bir değeri bir değişkene basit bit temelinde bir kopyasını gerçekleştirir.

`(T)x`gibi dönüştürme işlemleri, Kullanıcı tanımlı dönüştürmeler ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) sağlanarak aşırı yüklenmiştir.

`a[x]`gibi öğe erişimi, aşırı yüklenebilir bir operatör olarak kabul edilmez. Bunun yerine, Kullanıcı tanımlı dizin oluşturucular ([Dizin oluşturucular](classes.md#indexers)) aracılığıyla desteklenir.

İfadelerde, işleçlere işleç gösterimi kullanılarak başvurulur ve bildirimlerinde, işlev gösterimi kullanılarak işleçlere başvurulur. Aşağıdaki tabloda, birli ve ikili işleçler için işleç ve işlevsel gösterimler arasındaki ilişki gösterilmektedir. İlk girişte, *op* fazla yüklenebilir birli ön ek işlecini gösterir. İkinci girişte, *op* birli sonek `++` ve `--` işleçlerini gösterir. Üçüncü girişte, *op* , aşırı yüklenebilir ikili işleç olduğunu gösterir.


| __İşleç gösterimi__ | __İşlevsel Gösterim__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Kullanıcı tanımlı işleç bildirimleri her zaman, işleç bildirimini içeren sınıf veya yapı türünde parametrelerden en az birini gerektirir. Bu nedenle, Kullanıcı tanımlı bir işlecin önceden tanımlanmış bir işleçle aynı imzaya sahip olması mümkün değildir.

Kullanıcı tanımlı işleç bildirimleri bir işlecin sözdizimini, önceliğini veya ilişkilendirilebilirliğini değiştiremez. Örneğin, `/` işleci her zaman bir ikili işleçtir, her zaman [işleç önceliği ve ilişkilendirilebilirliği](expressions.md#operator-precedence-and-associativity)içinde belirtilen öncelik düzeyine sahiptir ve her zaman sola ilişkilendirilebilir.

Kullanıcı tanımlı bir işlecin, kiralamaları yaptığı herhangi bir hesaplamayı gerçekleştirmesi mümkün olsa da, büyük bir süre içinde olması beklenen, farklı sonuçlar üreten uygulamalar kesinlikle önerilmez. Örneğin, `operator ==` bir uygulama, eşitlik için iki işleneni karşılaştırmalı ve uygun bir `bool` sonucu döndürmelidir.

[Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators) aracılığıyla [birincil ifadelerde](expressions.md#primary-expressions) bağımsız işleçlerin açıklamaları, işleçlerin önceden tanımlanmış uygulamalarını ve her bir operatör için uygulanan ek kuralları belirtir. Açıklamalar ***birli operatör aşırı yükleme çözünürlüğü***, ***ikili işleç aşırı yükleme çözümlemesi***ve ***sayısal yükseltme***, tanımları aşağıdaki bölümlerde bulunan tanımlardan birini kullanır.

### <a name="unary-operator-overload-resolution"></a>Birli operatör aşırı yükleme çözünürlüğü

`op x` veya `x op`bir işlem, `op` aşırı yüklenebilir birli bir işleçtir ve `x` `X`türünde bir ifadedir, aşağıdaki şekilde işlenir:

*  İşlem `operator op(x)` için `X` tarafından sunulan aday Kullanıcı tanımlı operatörler kümesi, [aday Kullanıcı tanımlı işleçlerin](expressions.md#candidate-user-defined-operators)kuralları kullanılarak belirlenir.
*  Aday Kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçler kümesi olur. Aksi halde, yükseltilmemiş formları dahil olmak üzere önceden tanımlanmış birli `operator op` uygulamalar, işlem için aday işleçler kümesi olur. Belirli bir işlecin önceden tanımlanmış uygulamaları, işlecinin açıklamasında belirtilir ([birincil ifadeler](expressions.md#primary-expressions) ve [Birli İşleçler](expressions.md#unary-operators)).
*  Aşırı [yükleme çözümlemesi](expressions.md#overload-resolution) çözüm kuralları, `(x)`bağımsız değişken listesine göre en iyi işleci seçmek üzere aday işleçler kümesine uygulanır ve bu işleç aşırı yükleme çözümleme işleminin sonucu olur. Aşırı yükleme çözümlemesi tek bir en iyi operatör seçmezse bağlama zamanı hatası oluşur.

### <a name="binary-operator-overload-resolution"></a>İkili işleç aşırı yükleme çözümü

`x op y`bir işlem, `op` aşırı yüklenebilir bir ikili işleçtir, `x` `X`türünde bir ifadedir ve `y` `Y`türünde bir ifadedir, aşağıdaki şekilde işlenir:

*  İşlem `operator op(x,y)` için `X` ve `Y` tarafından sunulan aday Kullanıcı tanımlı operatörler kümesi belirlenir. Küme, `X` tarafından sunulan aday işleçleri birleşiminden ve `Y`tarafından sunulan aday işleçlerden (her biri [aday Kullanıcı tanımlı işleçlerin](expressions.md#candidate-user-defined-operators)kuralları kullanılarak belirlenir) oluşur. `X` ve `Y` aynı türde veya `X` ve `Y` ortak bir temel türden türetildiyse, paylaşılan aday işleçler yalnızca Birleşik küme içinde bir kez gerçekleşir.
*  Aday Kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçler kümesi olur. Aksi halde, yükseltilmemiş formları dahil olmak üzere önceden tanımlanmış ikili `operator op` uygulamalar, işlem için aday işleçler kümesi olur. Belirli bir işlecin önceden tanımlanmış uygulamaları, işlecinin açıklamasında belirtilir ( [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)aracılığıyla[Aritmetik işleçler](expressions.md#arithmetic-operators) ). Önceden tanımlanmış Enum ve temsilci işleçleri için kabul edilen tek işleçler, işlenenlerden birinin bağlama zamanı türü olan bir Enum veya temsilci türü tarafından tanımlananlar.
*  Aşırı [yükleme çözümlemesi](expressions.md#overload-resolution) çözüm kuralları, `(x,y)`bağımsız değişken listesine göre en iyi işleci seçmek üzere aday işleçler kümesine uygulanır ve bu işleç aşırı yükleme çözümleme işleminin sonucu olur. Aşırı yükleme çözümlemesi tek bir en iyi operatör seçmezse bağlama zamanı hatası oluşur.

### <a name="candidate-user-defined-operators"></a>Aday Kullanıcı tanımlı işleçler

`op` fazla yüklenebilir bir operatör olduğu ve `A` bir bağımsız değişken listesi olduğu bir tür `T` ve bir işlem `operator op(A)`veriliyorsa, `T` için `operator op(A)` tarafından sağlanan aday Kullanıcı tanımlı işleçler kümesi aşağıdaki şekilde belirlenir:

*  `T0`türünü saptayın. `T` null yapılabilir bir tür ise, `T0` temel türüdür, aksi takdirde `T0` `T`eşittir.
*  `T0` ve bu tür operatörlerin tüm yükseltilmemiş formlarında, en az bir operatör bağımsız değişken listesi `A`göre uygulanabilir ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)), aday işleçler kümesi, `T0`söz konusu tüm ilgili işleçlerden oluşur. `operator op`
*  Aksi takdirde, `T0` `object`, aday işleçleri kümesi boştur.
*  Aksi takdirde, `T0` tarafından sunulan aday işleçleri kümesi, `T0`doğrudan temel sınıfı tarafından sunulan aday işleçler kümesidir veya `T0` bir tür parametresi ise `T0` etkin taban sınıfı olur.

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

Bu işleç kümesine aşırı yükleme çözümleme kuralları ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) uygulandığında, efekt, örtük dönüştürmelerin işlenen türlerinden ilk birini seçmek için kullanılır. Örneğin, `b` bir `byte` olduğu ve `s` `short`olduğu `b * s`işlem için, aşırı yükleme çözümü en iyi operatör olarak `operator *(int,int)` seçer. Bu nedenle, `b` ve `s` `int`'e dönüştürüleceğinden ve sonucun türü `int`. Benzer şekilde, `i` bir `int` olduğu ve `d` `double`olduğu `i * d`işlem için, aşırı yükleme çözümü en iyi operatör olarak `operator *(double,double)` seçer.

#### <a name="unary-numeric-promotions"></a>Tekli sayı yükseltmeleri

Önceden tanımlanmış `+`, `-`ve `~` birli operatörlerin işlenenleri için birli sayısal yükseltme oluşur. Birli sayısal yükseltme, `char` türüne `sbyte`, `byte`, `short`, `ushort`veya `int`türündeki işlenenleri dönüştürmeyi içerir. Ayrıca, birli `-` işleci için, birli sayısal promosyon `uint` türündeki işlenenleri `long`türüne dönüştürür.

#### <a name="binary-numeric-promotions"></a>İkili sayısal yükseltmeler

Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`ve `<=` ikili işleçlerinin işlenenleri için ikili sayısal yükseltme oluşur. İkili sayısal yükseltme, ilişkisel olmayan işleçler söz konusu olduğunda her iki işleneni dolaylı olarak ortak bir türe dönüştürür. bu da işlemin sonuç türü olur. İkili sayısal yükseltme aşağıdaki kuralları, burada göründükleri düzende uygulamaktan oluşur:

*  Her iki işlenen `decimal`türünde ise, diğer işlenen `decimal`türüne dönüştürülür veya diğer işlenen `float` veya `double`türünde olduğunda bir bağlama zamanı hatası oluşur.
*  Aksi halde, her iki işlenen `double`türünde ise, diğer işlenen `double`türüne dönüştürülür.
*  Aksi halde, her iki işlenen `float`türünde ise, diğer işlenen `float`türüne dönüştürülür.
*  Aksi halde, her iki işlenen `ulong`türünde ise, diğer işlenen `ulong`türüne dönüştürülür veya diğer işlenen `sbyte`, `short`, `int`veya `long`türünde olduğunda bir bağlama zamanı hatası oluşur.
*  Aksi halde, her iki işlenen `long`türünde ise, diğer işlenen `long`türüne dönüştürülür.
*  Aksi halde, her iki işlenen de `uint` ve diğer işlenen `sbyte`, `short`veya `int`türündedir, her iki işlenen de `long`türüne dönüştürülür.
*  Aksi halde, her iki işlenen `uint`türünde ise, diğer işlenen `uint`türüne dönüştürülür.
*  Aksi halde, her iki işlenen `int`türüne dönüştürülür.

İlk kuralın `decimal` türünü karıştıraş tüm işlemlere `double` ve `float` türleriyle izin vermez. Kural, `decimal` türü ile `double` ve `float` türleri arasında örtük dönüştürme olmaması durumunda yer alan bir olay izler.

Ayrıca, diğer işlenen işaretli bir integral türü olduğunda bir işlenenin `ulong` türünden olması mümkün değildir. Bunun nedeni, `ulong` tam aralığını ve imzalı integral türlerini temsil eden hiçbir integral türü yok.

Yukarıdaki her iki durumda da, bir işleneni açıkça diğer işlenenle uyumlu bir türe dönüştürmek için bir atama ifadesi kullanılabilir.

Örnekte
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
bir `decimal` `double`çarpamadığı için bağlama zamanı hatası oluşur. Hata, ikinci işleneni aşağıdaki gibi `decimal`olarak açıkça dönüştürerek çözümlenir:

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

    bir işlecin yükseltilmemiş formu, işlenen ve sonuç türleri null yapılamayan tüm değer türleri ise vardır. Yükseltilmemiş formu, her işlenenin ve sonuç türünün tek bir `?` değiştiricisi eklenerek oluşturulur. Yükseltilmemiş işleci, bir veya her iki işlenen de null ise null değeri üretir ( [Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)bölümünde açıklandığı gibi `bool?` türünün `&` ve `|` işleçleri). Aksi halde, yükseltilmemiş işleci işlenenleri sarmalanmış, temeldeki işleci uygular ve sonucu sarmalar.

*   Eşitlik işleçleri için

    ```csharp
    ==  !=
    ```

    bir işlecin yükseltilmemiş formu, işlenen türleri hem null yapılamayan değer türleri hem de sonuç türü `bool`ise vardır. Yükseltilmemiş formu, her bir işlenen türüne tek bir `?` değiştiricisi eklenerek oluşturulur. Yükseltilmemiş işleci iki null değeri eşit kabul eder ve null olmayan bir değer null değeri boş değer olarak eşit değildir. Her iki işlenen de null değilse, yükseltilmemiş işleci işlenenleri geri sarar ve `bool` sonucunu üretmek için temeldeki işleci uygular.

*   İlişkisel işleçler için

    ```csharp
    <  >  <=  >=
    ```

    bir işlecin yükseltilmemiş formu, işlenen türleri hem null yapılamayan değer türleri hem de sonuç türü `bool`ise vardır. Yükseltilmemiş formu, her bir işlenen türüne tek bir `?` değiştiricisi eklenerek oluşturulur. Bir veya her iki işlenen de null ise, yükseltilmemiş işleci `false` değeri üretir. Aksi halde, yükseltilmemiş işleci işlenenleri geri sarar ve `bool` sonucunu üretmek için temeldeki işleci uygular.

## <a name="member-lookup"></a>Üye arama

Üye arama, bir tür bağlamındaki bir adın anlamını tespit eden işlemdir. Bir ifadede *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) değerlendirme işleminin parçası olarak bir üye arama gerçekleşebilir. *Simple_name* veya *member_access* bir *invocation_expression* ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) *primary_expression* olarak gerçekleşirse, üyenin çağrılması söylenir.

Bir üye bir yöntem veya olaydır veya bir temsilci türünün ([Temsilciler](delegates.md)) ya da tür `dynamic` ([dinamik tür](types.md#the-dynamic-type)) bir sabit, alan veya özellik ise, üyenin *ıncable*olarak kabul edilir.

Üye arama yalnızca bir üyenin adını değil, üyenin sahip olduğu tür parametrelerinin sayısını ve üyenin erişilebilir olup olmadığını dikkate alır. Üye arama amaçları için, genel metotlar ve iç içe geçmiş genel türler ilgili bildirimlerinde belirtilen tür parametrelerinin sayısına ve diğer tüm üyelerin sıfır tür parametrelerine sahip olması gerekir.

Bir ada `N` bir ad `K` tür `T` parametreleri bir üye araması, aşağıdaki gibi işlenir:

*  İlk olarak, `N` adlı bir erişilebilir üye kümesi belirlenir:
    * `T` bir tür parametresidir, küme, `T`için birincil kısıtlama veya ikincil kısıtlama ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) olarak belirtilen her türde `N` adlı erişilebilir üye kümelerinin birleşimidir. bu, `object` `N` adlı erişilebilir Üyeler kümesiyle birlikte.
    * Aksi takdirde, küme, devralınan Üyeler ve `object` `N` adlı erişilebilir Üyeler dahil olmak üzere `T` `N` adlı tüm erişilebilir ([üye erişim](basic-concepts.md#member-access)) üyelerinden oluşur. `T` oluşturulmuş bir türse, Üyeler kümesi, [oluşturulan türlerin üyelerinde](classes.md#members-of-constructed-types)açıklanan tür bağımsız değişkenlerini değiştirerek alınır. `override` değiştiricisi içeren Üyeler kümeden çıkarılır.
*  Sonra, `K` sıfır ise, bildirimleri tür parametreleri içeren tüm iç içe türler kaldırılır. `K` sıfır değilse, farklı sayıda tür parametrelerine sahip tüm Üyeler kaldırılır. `K` sıfırdan olduğunda, tür çıkarımı işlemi ([tür çıkarımı](expressions.md#type-inference)) tür bağımsız değişkenlerini çıkarsanbileceği için tür parametrelerine sahip yöntemlerin kaldırılmadığını unutmayın.
*  Sonra, üye *çağrılırsa*,*çağrılamayan* tüm Üyeler kümeden kaldırılır.
*  Daha sonra, diğer Üyeler tarafından gizlenen Üyeler kümeden kaldırılır. Küme içindeki her üye `S.M` için, `S` üye `M` bildirildiği türdür, aşağıdaki kurallar uygulanır:
    * `M` bir sabit, alan, özellik, olay veya numaralandırma üyesiyse, `S` temel türünde belirtilen tüm Üyeler kümeden kaldırılır.
    * `M` bir tür bildirimidir, temel `S` türünde bildirilmemiş tüm türler kümeden kaldırılır ve `S` temel türünde bildirildiği `M` aynı sayıda tür parametrelerine sahip tüm tür bildirimleri kümesinden kaldırılır.
    * `M` bir yöntem ise, temel türünde `S` belirtilen tüm yöntem olmayan Üyeler kümeden kaldırılır.
*  Daha sonra, sınıf üyeleri tarafından gizlenen arabirim üyeleri kümeden kaldırılır. Bu adım yalnızca `T` bir tür parametresidir ve `T` hem `object` dışındaki etkin bir temel sınıfa hem de boş olmayan bir etkin arabirim kümesine sahip ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) sahip olur. Küme içindeki her üye `S.M` için `S` üye `M` bildirildiği türdür, `S` `object`dışında bir sınıf bildirimidir, aşağıdaki kurallar uygulanır:
    * `M` bir sabit, alan, özellik, olay, numaralandırma üyesi veya tür bildirimi ise, bir arabirim bildiriminde belirtilen tüm Üyeler kümeden kaldırılır.
    * `M` bir yöntem ise, bir arabirim bildiriminde belirtilen tüm yöntem olmayan Üyeler kümeden kaldırılır ve bir arabirim bildiriminde bildirildiği `M` aynı imzaya sahip tüm yöntemler kümeden kaldırılır.
*  Son olarak, gizli üyeleri kaldırdıkça, aramanın sonucu belirlenir:
    * Küme, yöntem olmayan tek bir üyenin içeriyorsa, bu üye aramanın sonucudur.
    * Aksi takdirde, küme yalnızca yöntemleri içeriyorsa, bu yöntem grubu aramanın sonucudur.
    * Aksi takdirde, arama belirsizdir ve bir bağlama zamanı hatası oluşur.

Tür parametreleri ve arabirimleri dışındaki türlerde üye aramaları ve kesinlikle tek devralma olan arabirimlerde üye aramaları için (devralma zincirindeki her arabirim tam olarak sıfır veya bir doğrudan temel arabirimine sahiptir), arama kurallarının etkisi yalnızca bu türetilmiş Üyeler aynı ad veya imzaya sahip temel üyeleri gizler. Bu tür tek devralma aramaları hiçbir şekilde belirsiz değildir. Birden çok devralma arabirimindeki üye aramalarından muhtemelen olabilecek belirsizlikleri, [arabirim üye erişimi](interfaces.md#interface-member-access)bölümünde açıklanmıştır.

### <a name="base-types"></a>Temel türler

Üye arama amacıyla bir tür `T` aşağıdaki temel türlere sahip olacak şekilde değerlendirilir:

*  `T` `object`, `T` temel tür içermez.
*  `T` bir *enum_type*, `T` taban türleri `System.Enum`, `System.ValueType`ve `object`sınıf türleridir.
*  `T` bir *struct_type*, `T` taban türleri `System.ValueType` ve `object`sınıflardır.
*  `T` *class_type*`T` temel türleri, sınıf türü `object`dahil `T`temel sınıflarıdır.
*  `T` bir *interface_type*, `T` taban türleri `T` temel arayüzlerdir ve `object`sınıf türüdür.
*  `T` bir *array_type*, `T` taban türleri `System.Array` ve `object`sınıflardır.
*  `T` bir *delegate_type*, `T` taban türleri `System.Delegate` ve `object`sınıflardır.

## <a name="function-members"></a>İşlev üyeleri

İşlev üyeleri, çalıştırılabilir deyimler içeren üyeleridir. İşlev üyeleri her zaman türlerin üyeleridir ve ad alanlarının üyeleri olamaz. C#Aşağıdaki işlev üyesi kategorilerini tanımlar:

*  Yöntemler
*  Özellikler
*  Olaylar
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

Aşağıdaki tabloda, açıkça çağrılabilen işlev üyelerinin altı kategorisini içeren yapılar içinde gerçekleşen işlem özetlenmektedir. `e`, `x`, `y`ve `value` değişkenler veya değerler olarak sınıflandırılan ifadeleri gösterir, `T` tür olarak sınıflandırılan bir ifadeyi belirtir, `F` yöntemin basit adıdır ve `P` bir özelliğin basit adıdır.


| __Oluşturma__     | __Örnek__    | __Açıklama__ |
|-------------------|----------------|-----------------|
| Yöntem çağırma | `F(x,y)`       | Aşırı yükleme çözümlemesi, kapsayan sınıf veya yapıda en iyi yöntem `F` seçmek için uygulanır. Yöntemi, `(x,y)`bağımsız değişken listesiyle çağrılır. Yöntem `static`değilse, örnek ifadesi `this`. | 
|                   | `T.F(x,y)`     | Sınıf veya yapı `T`en iyi yöntem `F` seçmek için aşırı yükleme çözümlemesi uygulanır. Yöntem `static`değilse bir bağlama zamanı hatası oluşur. Yöntemi, `(x,y)`bağımsız değişken listesiyle çağrılır. | 
|                   | `e.F(x,y)`     | Sınıf, yapı veya `e`türü tarafından verilen arabirimdeki en iyi yöntem F 'yi seçmek için aşırı yükleme çözümlemesi uygulanır. Yöntem `static`bir bağlama zamanı hatası oluşur. Yöntemi, `e` örnek ifadesiyle çağrılır ve bağımsız değişken listesi `(x,y)`. | 
| Özellik erişimi   | `P`            | Özelliğin `get` erişimcisi, kapsayan sınıf veya yapıda `P` çağrıldı. `P` salt yazılır olduğunda bir derleme zamanı hatası oluşur. `P` `static`değilse, örnek ifadesi `this`. | 
|                   | `P = value`    | `set` erişimcisi, kapsayan sınıf veya yapıda `P`, `(value)`bağımsız değişken listesi ile çağırılır. `P` salt okunurdur derleme zamanı hatası oluşur. `P` `static`değilse, örnek ifadesi `this`. | 
|                   | `T.P`          | Sınıf veya yapı `T` `P` özelliğin `get` erişimcisi çağrılır. `P` `static` değilse veya `P` salt yazılır ise, derleme zamanı hatası oluşur. | 
|                   | `T.P = value`  | Sınıf veya yapı `T` `P` özelliğin `set` erişimcisi, `(value)`bağımsız değişken listesi ile çağrılır. `P` `static` değilse veya `P` salt okunurdur bir derleme zamanı hatası oluşur. | 
|                   | `e.P`          | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `P` özelliğin `get` erişimcisi, `e`örneği ifadesiyle çağrılır. `P` `static` veya `P` salt yazılır ise bağlama zamanı hatası oluşur. | 
|                   | `e.P = value`  | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki `P` özelliğin `set` erişimcisi, örnek ifadesi `e` ve bağımsız değişken listesi `(value)`ile çağrılır. `P` `static` veya `P` salt okunurdur bir bağlama zamanı hatası oluşur. | 
| Olay erişimi      | `E += value`   | Kapsayan sınıf veya yapı içindeki olay `E` `add` erişimcisi çağrıldı. `E` statik değilse, örnek ifadesi `this`. | 
|                   | `E -= value`   | Kapsayan sınıf veya yapı içindeki olay `E` `remove` erişimcisi çağrıldı. `E` statik değilse, örnek ifadesi `this`. | 
|                   | `T.E += value` | Sınıf veya yapı `T` içindeki olay `E` `add` erişimcisi çağrılır. `E` statik değilse bir bağlama zamanı hatası oluşur. | 
|                   | `T.E -= value` | Sınıf veya yapı `T` içindeki olay `E` `remove` erişimcisi çağrılır. `E` statik değilse bir bağlama zamanı hatası oluşur. | 
|                   | `e.E += value` | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki olay `E` `add` erişimcisi, `e`örnek ifadesiyle çağrılır. `E` statik olduğunda bağlama zamanı hatası oluşur. | 
|                   | `e.E -= value` | Sınıf, yapı veya `e` türü tarafından verilen arabirimdeki olay `E` `remove` erişimcisi, `e`örnek ifadesiyle çağrılır. `E` statik olduğunda bağlama zamanı hatası oluşur. | 
| Dizin Oluşturucu erişimi    | `e[x,y]`       | Aşırı yükleme çözümlemesi, sınıf, yapı veya e-postayla verilen arabirimdeki en iyi Dizin oluşturucuyu seçmek için uygulanır. Dizin oluşturucunun `get` erişimcisi, `e` örnek ifadesiyle ve bağımsız değişken listesi `(x,y)`çağrılır. Dizin Oluşturucu salt yazılır ise bağlama zamanı hatası oluşur. | 
|                   | `e[x,y] = value` | Sınıf, yapı veya `e`türü tarafından verilen arabirimdeki en iyi Dizin oluşturucuyu seçmek için aşırı yükleme çözümlemesi uygulanır. Dizin oluşturucunun `set` erişimcisi, `e` örnek ifadesiyle ve bağımsız değişken listesi `(x,y,value)`çağrılır. Dizin Oluşturucu salt okunurdur bağlama zamanı hatası oluşur. | 
| İşleç çağırma | `-x`         | `x`türü tarafından verilen sınıfta veya yapıda en iyi birli işleci seçmek için aşırı yükleme çözümlemesi uygulanır. Seçili operatör `(x)`bağımsız değişken listesiyle çağrılır. | 
|                     | `x + y`      | `x` ve `y`türleri tarafından verilen sınıflarda veya yapılarda en iyi ikili işleci seçmek için aşırı yükleme çözümlemesi uygulanır. Seçili operatör `(x,y)`bağımsız değişken listesiyle çağrılır. | 
| Örnek Oluşturucu çağrısı | `new T(x,y)` | Sınıf veya yapı `T`en iyi örnek oluşturucusunu seçmek için aşırı yükleme çözümlemesi uygulanır. Örnek Oluşturucu `(x,y)`bağımsız değişken listesiyle çağrılır. | 

### <a name="argument-lists"></a>Bağımsız değişken listeleri

Her işlev üyesi ve temsilci çağrısı, işlev üyesinin parametreleri için gerçek değerler veya değişken başvuruları sağlayan bir bağımsız değişken listesi içerir. İşlev üyesi çağrısının bağımsız değişken listesini belirtmek için sözdizimi, işlev üyesi kategorisine bağlıdır:

*  Örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler için bağımsız değişkenler, aşağıda açıklandığı gibi *argument_list*olarak belirtilir. Dizin oluşturucular için, `set` erişimcisini çağırırken bağımsız değişken listesi, atama işlecinin sağ işleneni olarak belirtilen ifadeyi de içerir.
*  Özellikler için, `get` erişimcisi çağrılırken bağımsız değişken listesi boştur ve `set` erişimcisi çağrılırken atama işlecinin sağ işleneni olarak belirtilen ifadeden oluşur.
*  Olaylar için bağımsız değişken listesi, `+=` veya `-=` işlecinin sağ işleneni olarak belirtilen ifadeden oluşur.
*  Kullanıcı tanımlı işleçler için bağımsız değişken listesi birli işlecin tek işleneninden veya ikili işlecinin iki işleneninden oluşur.

Özelliklerinin bağımsız değişkenleri ([Özellikler](classes.md#properties)), Olaylar ([Olaylar](classes.md#events)) ve Kullanıcı tanımlı işleçler ([işleçler](classes.md#operators)) her zaman değer parametreleri olarak geçirilir ([değer parametreleri](classes.md#value-parameters)). Dizin oluşturucularının bağımsız değişkenleri ([Dizin oluşturucular](classes.md#indexers)) her zaman değer parametreleri ([değer parametreleri](classes.md#value-parameters)) veya parametre dizileri ([parametre dizileri](classes.md#parameter-arrays)) olarak geçirilir. Başvuru ve çıkış parametreleri bu işlev üyesi kategorileri için desteklenmez.

Örnek oluşturucusunun, yöntemin, dizin oluşturucunun veya temsilci çağrısının bağımsız değişkenleri *argument_list*olarak belirtilir:

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

*Argument_list* , virgülle ayrılmış bir veya daha fazla *bağımsız değişkenden*oluşur. Her bağımsız değişken, bir *argument_value*ve ardından bir isteğe bağlı *argument_name* oluşur. *Argument_name* bir *bağımsız* değişken ***adlandırılmış bağımsız değişken***olarak adlandırılır, ancak *argument_name* olmayan bir *bağımsız* değişken ***konumsal bir bağımsız***değişkendir. Konumsal bir bağımsız değişkenin bir *argument_list*adlandırılmış bir bağımsız değişkenden sonra görünmesi hatadır.

*Argument_value* aşağıdaki formlardan birini alabilir:

*  Bağımsız değişkenin değer parametresi olarak geçtiğini belirten bir *ifade*([değer parametreleri](classes.md#value-parameters)).
*  Anahtar sözcüğü, bağımsız değişkenin bir başvuru parametresi ([başvuru parametreleri](classes.md#reference-parameters)) olarak geçtiğini belirten bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)) `ref`. Bir değişken başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı ([kesin atama](variables.md#definite-assignment)). Anahtar sözcüğü, bağımsız değişkenin çıkış parametresi olarak geçtiğini belirten bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)[) `out`](classes.md#output-parameters). Değişken, değişkenin çıkış parametresi olarak geçirildiği bir işlev üyesi çağrısı sonrasında kesin olarak atanmış ([kesin atama](variables.md#definite-assignment)) olarak değerlendirilir.

#### <a name="corresponding-parameters"></a>Karşılık gelen parametreler

Bir bağımsız değişken listesindeki her bağımsız değişken için, çağrılan işlev üyesinde veya temsilde karşılık gelen bir parametre olması gerekmez.

Aşağıda kullanılan parametre listesi aşağıdaki şekilde belirlenir:

*  Sınıflarda tanımlı sanal yöntemler ve Dizin oluşturucular için, parametre listesi, alıcı türü ve temel sınıfları arasında arama yaparak işlev üyesinin en belirgin bildiriminden veya geçersiz kılınmadan çekilir.
*  Arabirim yöntemleri ve Dizin oluşturucular için, parametre listesi, arabirim türü ile başlayıp temel arabirimler üzerinden arama yaparak üyenin en belirli tanımını oluşturacak şekilde çekilir. Benzersiz bir parametre listesi bulunamazsa, erişilemeyen adlara sahip bir parametre listesi ve isteğe bağlı parametre oluşturulur; bu nedenle, etkinleştirmeleri adlandırılmış parametreleri kullanamaz veya isteğe bağlı bağımsız değişkenleri alamaz.
*  Kısmi yöntemler için, tanımlayan kısmi Yöntem bildiriminin parametre listesi kullanılır.
*  Diğer tüm işlev üyeleri ve temsilciler için yalnızca tek bir parametre listesi bulunur ve bu, kullanılır.

Bir bağımsız değişkenin veya parametrenin konumu, bağımsız değişken listesi veya parametre listesinde kendisinden önceki bağımsız değişken veya parametre sayısı olarak tanımlanır.

İşlev üyesi bağımsız değişkenleri için karşılık gelen parametreler şu şekilde oluşturulur:

*  Örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler *argument_list* bağımsız değişkenler:
    * Parametre listesindeki aynı konumda sabit bir parametrenin gerçekleştiği konumsal bir bağımsız değişken bu parametreye karşılık gelir.
    * Normal biçiminde çağrılan bir parametre dizisi olan bir işlev üyesinin Konumsal bağımsız değişkeni, parametre listesinde aynı konumda olması gereken parametre dizisine karşılık gelir.
    * Bir parametre dizisi olan bir işlev üyesinin, parametre listesinde aynı konumda hiçbir sabit parametre gerçekleşmediği, parametre dizisindeki bir öğeye karşılık gelen bir konum bağımsız değişkeni.
    * Adlandırılmış bir bağımsız değişken, parametre listesinde aynı ada sahip parametreye karşılık gelir.
    * Dizin oluşturucular için, `set` erişimcisi çağrılırken, atama işlecinin sağ işleneni olarak belirtilen ifade, `set` erişimci bildiriminin örtük `value` parametresine karşılık gelir.
*  Özellikler için `get` erişimcisi çağrılırken bağımsız değişken yoktur. `set` erişimcisi çağrılırken, atama işlecinin sağ işleneni olarak belirtilen ifade, `set` erişimci bildiriminin örtük `value` parametresine karşılık gelir.
*  Kullanıcı tanımlı Birli İşleçler (dönüşümler dahil) için, tek işlenen işleç bildiriminin tek parametresine karşılık gelir.
*  Kullanıcı tanımlı ikili işleçler için, sol işlenen ilk parametreye karşılık gelir ve sağ işlenen işleç bildiriminin ikinci parametresine karşılık gelir.

#### <a name="run-time-evaluation-of-argument-lists"></a>Bağımsız değişken listelerinin çalışma süresi değerlendirmesi

Bir işlev üye çağrısının çalışma zamanı işlemesi sırasında ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), bir bağımsız değişken listesinin ifadeleri ya da değişken başvuruları, soldan sağa aşağıdaki gibi sırayla değerlendirilir:

*  Bir değer parametresi için bağımsız değişken ifadesi değerlendirilir ve ilgili parametre türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir. Sonuç değeri, işlev üye çağrısında değer parametresinin başlangıç değeri olur.
*  Bir başvuru veya çıkış parametresi için, değişken başvurusu değerlendirilir ve sonuçta elde edilen depolama konumu, işlev üyesi çağrısında parametresi tarafından temsil edilen depolama konumu olur. Başvuru veya çıkış parametresi olarak verilen değişken başvurusu bir *reference_type*dizi öğesi ise, dizinin öğe türünün parametrenin türüyle aynı olduğundan emin olmak için bir çalışma zamanı denetimi yapılır. Bu denetim başarısız olursa, bir `System.ArrayTypeMismatchException` oluşturulur.

Yöntemler, Dizin oluşturucular ve örnek oluşturucular, en sağdaki parametreleri parametre dizisi ([parametre dizileri](classes.md#parameter-arrays)) olarak bildirebilir. Bu tür işlev üyeleri, uygulanabilir olduğu ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) bağlı olarak kendi normal biçiminde veya genişletilmiş biçiminde çağırılır:

*  Parametre dizisi olan bir işlev üyesi normal biçimde çağrıldığında, parametre dizisi için verilen bağımsız değişken parametre dizi türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tek bir ifade olmalıdır. Bu durumda, parametre dizisi tam olarak bir değer parametresi gibi davranır.
*  Bir parametre dizisi olan bir işlev üyesi genişletilmiş biçimde çağrıldığında, çağırma parametre dizisi için sıfır veya daha fazla Konumsal bağımsız değişkeni, her bağımsız değişkenin parametre dizisinin öğe türüne örtük olarak dönüştürülebilir bir ifade ([örtük dönüştürmeler](conversions.md#implicit-conversions)) belirtmesi gerekir. Bu durumda, çağırma parametre dizisi türünde bağımsız değişken sayısına karşılık gelen bir bir örnek oluşturur, dizi örneğinin öğelerini verilen bağımsız değişken değerleriyle başlatır ve gerçek olarak yeni oluşturulan dizi örneğini kullanır değişkendir.

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

Dizi birlikte değişim kuralları ([dizi Kovaryans](arrays.md#array-covariance)), dizi türü bir değere `A[]` `B[]`bir dizi türü örneğine izin verir, `B` ' den `A`örtük bir başvuru dönüştürmesi sağlanmış olur. Bu kurallar nedeniyle, bir *reference_type* dizi öğesi başvuru veya çıkış parametresi olarak geçirildiğinde, dizinin gerçek öğe türünün parametreyle aynı olduğundan emin olmak için bir çalışma zamanı denetimi gereklidir. Örnekte
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
`F` ikinci çağrılması `System.ArrayTypeMismatchException` oluşturulmasına neden olur çünkü `b` gerçek öğe türü `string` ve `object`değildir.

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
açıkça bir tür bağımsız değişkeni belirtmeden `Choose` yöntemi çağırmak mümkündür:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Tür çıkarımı aracılığıyla `int` ve `string` tür bağımsız değişkenleri, bağımsız değişkenlerden yöntemine belirlenir.

Tür çıkarımı, bir yöntem çağırma işleminin ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) bağlama zamanı işlemenin bir parçası olarak oluşur ve çağrının aşırı yükleme çözümleme adımından önce gerçekleşir. Bir yöntem çağrısında belirli bir yöntem grubu belirtildiğinde ve yöntem çağrısının bir parçası olarak hiçbir tür bağımsız değişkeni belirtilmediğinde, yöntem grubundaki her genel yönteme tür çıkarımı uygulanır. Tür çıkarımı başarılı olursa, sonraki aşırı yükleme çözümlemesi için bağımsız değişken türlerini belirlemede çıkartılan tür bağımsız değişkenleri kullanılır. Aşırı yükleme çözümlemesi, çağırabileceğiniz bir genel yöntem seçerse, çağırma için gerçek tür bağımsız değişkenleri olarak, çıkartılan tür bağımsız değişkenleri kullanılır. Belirli bir yöntemin tür çıkarımı başarısız olursa, bu yöntem aşırı yükleme çözümüne katılmaz. , Ve içindeki çıkarım hatası, bağlama zamanı hatasına neden olmaz. Ancak, aşırı yükleme çözümlemesi sırasında bir bağlama zamanı hatasına neden olur ve ilgili herhangi bir yöntemi bulamaz.

Sağlanan bağımsız değişken sayısı yöntemdeki parametre sayısından farklıysa, çıkarımı hemen başarısız olur. Aksi takdirde, genel yöntemin aşağıdaki imzaya sahip olduğunu varsayalım:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

`M(E1...Em)` bir yöntem çağrısı ile, tür çıkarımı görevi `X1...Xn` tür parametrelerinin her biri için `S1...Sn` benzersiz tür bağımsız değişkenlerini bulmalıdır, böylece çağrının `M<S1...Sn>(E1...Em)` geçerli hale gelir.

Çıkarımı işlemi sırasında her tür parametre `Xi`, belirli bir tür `Si` *sabitlenir* veya ilişkili bir *sınır*kümesiyle *sabitlenemez* . Her bir sınır `T`türüdür. Başlangıçta her tür değişkeni `Xi` boş bir sınır kümesiyle düzeltilmedi.

Tür çıkarımı aşamalar halinde gerçekleşir. Her aşama, önceki aşamanın bulgularını temel alarak daha fazla tür değişkeni için tür bağımsız değişkenlerini çıkarması denenecek. İlk aşama bazı ilk dereceden sınırları yapar, ancak ikinci aşama belirli türlere tür değişkenlerini ve daha fazla sınırları düzeltir. İkinci aşamanın birkaç kez tekrarlanmanız gerekebilir.

*Note:* Tür çıkarımı yalnızca genel bir yöntem çağrıldığında değil. Yöntem gruplarının dönüştürülmesine ilişkin tür çıkarımı, [Yöntem gruplarının dönüştürülmesi için tür çıkarımı](expressions.md#type-inference-for-conversion-of-method-groups) ve bir ifade kümesinin en iyi ortak [türünü bulma bölümünde açıklanmıştır.](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)

#### <a name="the-first-phase"></a>İlk aşama

Yöntem bağımsız değişkenlerinin her biri için `Ei`:

*   `Ei` anonim bir işlevse, açık bir *parametre türü çıkarımı* ([Açık parametre türü](expressions.md#explicit-parameter-type-inferences)) `Ei` ' dan `Ti` yapılır
*   Aksi takdirde, `Ei` `U` bir tür `xi` ve bir değer parametresi ise, `U` *'den* `Ti` *'e* *daha düşük bir çıkarım* yapılır.
*   Aksi takdirde, `Ei` `U` bir tür varsa ve `xi` bir `ref` veya `out` parametresi ise `U` ' *den* `Ti`' *e* *doğru bir çıkarım* yapılır.
*   Aksi takdirde, bu bağımsız değişken için çıkarımı yapılmaz.


#### <a name="the-second-phase"></a>İkinci aşama

İkinci aşama aşağıdaki gibi ilerler:

*   Hiçbir `Xj` ([bağımlılıklara CE](expressions.md#dependence)) *bağımlı* olmayan `Xi` tüm *sabit* olmayan tür değişkenleri düzeltildi ([düzeltiliyor](expressions.md#fixing)).
*   Böyle bir tür değişkeni yoksa, tüm *sabit* olmayan tür değişkenleri aşağıdaki ayrı tutmadaki `Xi` *sabittir* :
    *   `Xi` bağlı olan en az bir tür değişken `Xj` vardır
    *   `Xi` boş olmayan bir sınır kümesine sahip
*   Böyle bir tür değişkeni yoksa ve hala *sabit* olmayan tür değişkenleri varsa, tür çıkarımı başarısız olur.
*   Aksi halde, daha fazla *sabit olmayan* tür değişkeni yoksa, tür çıkarımı başarılı olur.
*   Aksi takdirde, karşılık gelen parametre türü `Ei`, *Çıkış türlerinin* ([çıktı türleri](expressions.md#output-types)) *sabit* olmayan tür `Xj` değişkenleri içeren `Ti`, ancak *giriş türleri* ([giriş türleri](expressions.md#input-types)) olmayan tüm *bağımsız değişkenler için `Ei` '* *dan* `Ti`' a[bir](expressions.md#output-type-inferences) *Çıkış türü çıkarımı* yapılır. İkinci aşama yinelenir.

#### <a name="input-types"></a>Giriş türleri

`E` bir yöntem grubu veya örtük olarak yazılmış anonim işlevse ve `T` bir temsilci türü ya da ifade ağacı türsiyse, tüm `T` parametre türleri, *türü `T`olan* `E` *giriş türleridir* .

####  <a name="output-types"></a>Çıkış türleri

`E` bir yöntem grubu veya anonim işlevse ve `T` bir temsilci türü ya da ifade ağacı türü ise, `T` dönüş türü `T`*türünde* bir `E` *Çıkış türüdür* .

#### <a name="dependence"></a>Düzeyde bağımlı

*Sabit olmayan* *bir tür değişkeni `Xi`,* `Xj` türünde bir `Ek` *giriş türünde* meydana gelir ve `Tk` türünde `Xi` bir *çıkış türünde* meydana gelen bir `Ek` `Xj` `Tk`

`Xj`, `Xj` *doğrudan* `Xi` bağımlıysa veya `Xi` *doğrudan `Xk` 'ye* bağımlıysa ve `Xk` *`Xj`bağımlı ise* `Xi` *bağlıdır* . Bu nedenle "bağlı olan", geçişli ancak yansımalı olmayan "açık" kapanışı değildir.

#### <a name="output-type-inferences"></a>Çıkış türü ında

Aşağıdaki şekilde *`E` bir ifadeden* *bir tür* `T` bir *Çıkış türü çıkarımı* yapılır:

*  `E`, `U` çıkartılan dönüş türü olan bir anonim işlevse[(çıkartılan dönüş türü](expressions.md#inferred-return-type)) ve `T`, dönüş türü `Tb`olan bir temsilci türü veya ifade ağacı türü ise *, `U` ile `Tb`* *arasında* daha *düşük bağlantılı bir çıkarım* ([alt sınır](expressions.md#lower-bound-inferences)) yapılır.
*  Aksi takdirde, `E` bir yöntem grubu ise ve `T`, parametre türleri `T1...Tk` ve dönüş türü `Tb`olan bir temsilci türü ya da ifade ağacı türü `E` ve `T1...Tk` bir `U`dönüş türü ile tek bir yöntem ortaya çıkardı `U` *, `Tb`ile* arasında *daha düşük* bir çıkarma yapılır.
*  Aksi takdirde, `E` `U`türünde bir ifade ise, `U` ' *den* `T` *'ye* *daha düşük bir çıkarım* yapılır.
*  Aksi takdirde, hiçbir Inse yapılmaz.

#### <a name="explicit-parameter-type-inferences"></a>Açık parametre türü

Bir ifadeden `E` bir *Açık parametre türü çıkarımı* *aşağıdaki şekilde `T`* bir *türe yapılır:*

*  `E` parametre türleri `U1...Uk` açık *şekilde* belirlenmiş bir anonim işlevse ve `T` parametre türlerine sahip bir temsilci türü veya ifade ağacı türü `V1...Vk`, her *`Ui` `Ui` ilgili `Vi`* bir *çıkarım* ([tam](expressions.md#exact-inferences)).

#### <a name="exact-inferences"></a>Tam ında

*Bir tür `V` bir `U`* *tam çıkarımı* aşağıdaki gibi yapılır:

*  `V` *düzeltilmeyen* `Xi` biri ise, `U` `Xi`için tam sınırlara eklenir.

*  Aksi takdirde, `V1...Vk` ayarlar ve `U1...Uk` aşağıdaki durumlardan herhangi birinin uygulayıp uygulamadığı denetlenerek belirlenir:

   *  `V`, `V1[...]` bir dizi türüdür ve `U` aynı derecenin `U1[...]` bir dizi türüdür
   *  `V` tür `V1?` ve `U` türdür `U1?`
   *  `V`, oluşturulmuş bir tür `C<V1...Vk>`ve `U` oluşturulmuş bir tür `C<U1...Uk>`

   Bu durumlardan herhangi biri uygunsa *, her `Ui` karşılık gelen `Vi`* *tam çıkarımı* yapılır .

*  Aksi takdirde hiçbir Inse yapılmaz.

#### <a name="lower-bound-inferences"></a>Alt sınır

*Bir tür* `V` bir `U` *alt sınır çıkarımı* *Şu şekilde yapılır* :

*  `V` *düzeltilmeyen* `Xi` biri ise, `U` `Xi`alt sınırlar kümesine eklenir.
*  Aksi takdirde, `V` tür `V1?`ve `U` `U1?` tür ise, `U1` `V1`'e daha düşük bir sınır çıkarımı yapılır.
*  Aksi takdirde, `U1...Uk` ayarlar ve `V1...Vk` aşağıdaki durumlardan herhangi birinin uygulayıp uygulamadığı denetlenerek belirlenir:
   *  `V`, `V1[...]` bir dizi türüdür `U` ve aynı dereceye sahip bir dizi türü `U1[...]` (veya geçerli temel türü `U1[...]`olan bir tür parametresidir)
   *  `V` `IEnumerable<V1>`, `ICollection<V1>` veya `IList<V1>` ve `U` tek boyutlu dizi türü `U1[]`(veya geçerli temel türü `U1[]`olan bir tür parametresi)
   *  `V`, oluşturulmuş bir sınıf, yapı, arabirim veya temsilci türü `C<V1...Vk>` `C<U1...Uk>` ve `U` (ya da `U` bir tür parametresi ise, etkin taban sınıfı veya etkin arabirim kümesinin herhangi bir üyesi) ile aynıdır, öğesinden devralır (doğrudan veya dolaylı) ya da uygular (doğrudan veya dolaylı) `C<U1...Uk>`.

      ("Benzersizlik" kısıtlaması, büyük/küçük harf `U`, `X` veya `Y`olabileceği `U1` `C<T>` için, bu durum arabirimindeki `C<T> {} class U: C<X>, C<Y> {}`bir çıkarım yapılmayacağı anlamına gelir.)

   Bu durumlardan herhangi biri uygunsa her bir *`Ui` aşağıdaki şekilde* karşılık *gelen `Vi`* bir çıkarımı yapılır:

   *  `Ui` bir başvuru türü olarak bilinmiyorsa, *kesin bir çıkarım* yapılır
   *  Aksi takdirde, `U` bir dizi türü ise, *alt sınır çıkarımı* yapılır
   *  Aksi takdirde, `V` `C<V1...Vk>`, çıkarımı, `C`i-TH türü parametresine bağlıdır:
      *  Daha fazla değişken ise, *alt sınır çıkarımı* yapılır.
      *  Değişken karşıtı ise, *üst sınır çıkarımı* yapılır.
      *  Sabit ise, *kesin bir çıkarım* yapılır.
*  Aksi takdirde, hiçbir Inse yapılmaz.

#### <a name="upper-bound-inferences"></a>Üst sınır

`U` *bir tür* *`V` bir* *üst sınır çıkarımı* aşağıdaki gibi yapılır:

*  `V` *düzeltilmeyen* `Xi` biri ise, `Xi`için üst sınırlar kümesine `U` eklenir.
*  Aksi takdirde, `V1...Vk` ayarlar ve `U1...Uk` aşağıdaki durumlardan herhangi birinin uygulayıp uygulamadığı denetlenerek belirlenir:
   *  `U`, `U1[...]` bir dizi türüdür ve `V` aynı derecenin `V1[...]` bir dizi türüdür
   *  `U` bir `IEnumerable<Ue>`, `ICollection<Ue>` veya `IList<Ue>` ve `V` tek boyutlu dizi türüdür `Ve[]`
   *  `U` tür `U1?` ve `V` türdür `V1?`
   *  `U` oluşturulan sınıf, yapı, arabirim veya temsilci türü `C<U1...Uk>` ve `V`, ile özdeş olan (doğrudan veya dolaylı) ya da (doğrudan veya dolaylı olarak) benzersiz bir tür uygulayan bir sınıf, yapı, arabirim veya temsilci türüdür `C<V1...Vk>`

      ("Benzersizlik" kısıtlaması, `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`varsa `C<U1>` `V<Q>`'e kadar bir çıkarımı yapılmadığını gösterir. Inor, `U1` `X<Q>` veya `Y<Q>`olarak yapılmaz.)

   Bu durumlardan herhangi biri uygunsa her bir *`Ui` aşağıdaki şekilde* karşılık *gelen `Vi`* bir çıkarımı yapılır:
   *  `Ui` bir başvuru türü olarak bilinmiyorsa, *kesin bir çıkarım* yapılır
   *  Aksi takdirde, `V` bir dizi türü ise, *üst sınır çıkarımı* yapılır
   *  Aksi takdirde, `U` `C<U1...Uk>`, çıkarımı, `C`i-TH türü parametresine bağlıdır:
      *  Covaryant ise, *üst sınır çıkarımı* yapılır.
      *  Değişken karşıtı ise, *alt sınır çıkarımı* yapılır.
      *  Sabit ise, *kesin bir çıkarım* yapılır.
*  Aksi takdirde, hiçbir Inse yapılmaz.   

#### <a name="fixing"></a>Düzelttikten

Bir dizi sınırı olan `Xi` *sabit olmayan* bir tür değişkeni aşağıdaki gibi *düzeltilir* :

*  `Uj` *aday türleri* kümesi, `Xi`sınırları kümesindeki tüm türlerin kümesi olarak başlatılır.
*  Sonra her bir `Xi` için her birini sırayla inceliyoruz `U`: `U` aynı olmayan tüm türler `Uj` `Xi` aday kümesinden kaldırılır. Tüm `Xi` `U` her bir alt sınır için `U` örtük bir dönüştürme *olmadığı* `Uj` tüm türler için aday kümesinden kaldırılır. `U` örtük bir dönüştürme *olmadığı* `Uj` tüm türlerin `Xi` `U` her üst sınır için aday kümesinden kaldırılır.
*  Kalan aday türleri arasında `Uj`, tüm diğer aday türlerine örtük bir dönüştürme olan `V` benzersiz bir tür vardır ve `Xi` `V`için düzeltilir.
*  Aksi takdirde tür çıkarımı başarısız olur.

#### <a name="inferred-return-type"></a>Çıkarılan dönüş türü

Anonim bir işlevin `F` çıkarılan dönüş türü, tür çıkarımı ve aşırı yükleme çözümlemesi sırasında kullanılır. Çıkarılan dönüş türü yalnızca, açıkça verildiği veya bir kapsayan genel üzerinde tür çıkarımı sırasında çıkarsandığı için, tüm parametre türlerinin bilindikleri anonim bir işlev için belirlenebilir Yöntem çağırma.

***Çıkarılan sonuç türü*** aşağıdaki şekilde belirlenir:

*  `F` gövdesi türü olan bir *ifadesiyse* , `F` çıkarılan sonuç türü bu ifadenin türüdür.
*  `F` gövdesi bir *blok* ise ve bloğun `return` deyimlerindeki ifadelerin kümesi en iyi ortak tür `T` ([bir ifade kümesinin En Iyi ortak türünü bulmayla](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), bu durumda `F` çıkarılan sonuç türü `T`olur.
*  Aksi takdirde, `F`için bir sonuç türü çıkarsanamıyor.

***Çıkarılan dönüş türü*** aşağıdaki şekilde belirlenir:

*  `F` zaman uyumsuz ise ve `F` gövdesi Nothing ([ifade sınıflandırmaları](expressions.md#expression-classifications)) olarak sınıflandırılmış bir ifade veya hiçbir dönüş deyiminin ifadesi olmadığı bir deyim bloğu ise, çıkarılan dönüş türü `System.Threading.Tasks.Task`
*  `F` zaman uyumsuz ise ve ortaya çıkarılan sonuç türü `T`içeriyorsa, çıkarılan dönüş türü `System.Threading.Tasks.Task<T>`.
*  `F` zaman uyumsuz ise ve ortaya çıkarılan sonuç türü `T`içeriyorsa, çıkarılan dönüş türü `T`.
*  Aksi takdirde `F`için bir dönüş türü çıkarsanamıyor.

Anonim işlevler içeren tür çıkarımı örneği olarak, `System.Linq.Enumerable` sınıfında belirtilen `Select` uzantı yöntemini göz önünde bulundurun:
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

`System.Linq` ad alanının bir `using` yan tümcesiyle içeri aktarıldığını ve `string`türünde bir `Name` özelliği olan bir sınıf `Customer` verildiğini varsayarsak, `Select` yöntemi bir müşterilerin listesinin adlarını seçmek için kullanılabilir:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

`Select` uzantı yöntemi çağırma ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)), statik bir yöntem çağrısına yapılan çağrıyı yeniden yazarak işlenir:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Tür bağımsız değişkenleri açıkça belirtilmediği için tür çıkarımı tür bağımsız değişkenlerini çıkarsanacak şekilde kullanılır. İlk olarak, `customers` bağımsız değişkeni `source` parametresiyle ilgilidir ve `Customer``T`. Daha sonra, yukarıda açıklanan anonim işlev türü çıkarımı işlemini kullanarak, `c` `Customer`türü verilir ve `c.Name` ifadesi `selector` parametresinin dönüş türüyle ilgilidir ve `S` `string`. Bu nedenle, çağırma şu şekilde eşdeğerdir
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
ve sonuç `IEnumerable<string>`türündedir.

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
Şu şekilde devam eder: Ilk olarak, `"1:15:30"` bağımsız değişken `value` parametresiyle ilgilidir ve `string``X`. Ardından, `s`ilk adsız işlevin parametresi, `string`Çıkarsanan tür verilir ve `TimeSpan.Parse(s)` ifadesi `f1`dönüş türüyle ilgilidir ve `Y` `System.TimeSpan`. Son olarak, `t`ikinci adsız işlevin parametresine `System.TimeSpan`Çıkarsanan tür verilir ve `t.TotalSeconds` ifadesi `f2`dönüş türüyle ilgilidir ve `Z` `double`. Bu nedenle, çağrının sonucu `double`türüdür.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Yöntem gruplarının dönüştürülmesi için tür çıkarımı

Genel yöntemlerin çağrılarına benzer şekilde, genel bir yöntem içeren `M` bir yöntem grubu, belirli bir temsilci türüne `D` ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) dönüştürüldüğünde tür çıkarımı da uygulanmalıdır. Verilen Yöntem
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
`M` temsilci türüne atanmakta olan Yöntem grubu, `D` tür çıkarımı `S1...Sn`, ifadenin tür bağımsız değişkenlerini bulmaktadır.
```csharp
M<S1...Sn>
```
`D`ile uyumlu ([temsilci bildirimleri](delegates.md#delegate-declarations)) olur.

Genel yöntem çağrılarının tür çıkarımı algoritmasından farklı olarak, bu durumda yalnızca bağımsız değişken *türleri*vardır, hiçbir bağımsız değişken *ifadesi*yoktur. Özellikle, anonim işlev yoktur ve bu nedenle birden çok çıkarım aşaması gerekmez.

Bunun yerine, tüm `Xi` *düzeltilmez*ve `D` *`Uj` her bağımsız değişken türünden `M`* karşılık gelen parametre türü `Tj` *için* *alt sınır çıkarımı* yapılır. `Xi` herhangi bir sınır bulunmazsa, tür çıkarımı başarısız olur. Aksi takdirde, tüm `Xi`, tür çıkarımı sonucu olan karşılık gelen `Si`*sabitlenmiştir* .

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Bir ifade kümesinin en iyi ortak türünü bulma

Bazı durumlarda, bir dizi ifade için ortak bir türün çıkarsanması gerekir. Özellikle, örtük olarak yazılan dizilerin öğe türleri ve *blok* gövdeleri olan anonim işlevlerin dönüş türleri bu şekilde bulunur.

Bu çıkarımı `E1...Em` bir ifade kümesi verildiğinde ıntuicanlı, bir yöntemi çağırmak için eşdeğer olmalıdır
```csharp
Tr M<X>(X x1 ... X xm)
```
bağımsız değişken olarak `Ei`.

Daha kesin olarak, çıkarım, *sabit olmayan* bir tür değişkeniyle başlar `X`. Daha sonra *Çıkış türü* *, her `Ei`* `X`*olarak* yapılır. Son olarak, `X` *sabittir* ve başarılı olursa, sonuçta elde edilen tür `S` ifadeler için en iyi ortak türdür. Böyle `S` yoksa, ifadelerde en iyi ortak tür yoktur.

### <a name="overload-resolution"></a>Aşırı yükleme çözümü

Aşırı yükleme çözümlemesi, verilen bağımsız değişken listesini ve aday işlev üyelerini çağırmak için en iyi işlev üyesini seçmeye yönelik bir bağlama zamanı mekanizmasıdır. Aşırı yükleme çözümlemesi, içindeki C#aşağıdaki farklı bağlamlarda çağrılacak işlev üyesini seçer:

*  *İnvocation_expression* adında bir yöntemin çağrılması ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)).
*  *Object_creation_expression* ([nesne oluşturma ifadelerinde](expressions.md#object-creation-expressions)) adlı örnek oluşturucunun çağrılması.
*  Bir *element_access* Dizin Oluşturucu erişimcisinin çağrılması ([öğe erişimi](expressions.md#element-access)).
*  İfadede önceden tanımlanmış veya Kullanıcı tanımlı bir işlecin çağrılması ([birli operatör aşırı yükleme çözümü](expressions.md#unary-operator-overload-resolution) ve [ikili işleç aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)).

Bu bağlamların her biri, yukarıda listelenen bölümlerde ayrıntılı olarak açıklandığı gibi aday işlev üyeleri kümesini ve bağımsız değişkenlerin listesini kendi benzersiz şeklinde tanımlar. Örneğin, bir yöntem çağrısı için aday kümesi `override` ([üye arama](expressions.md#member-lookup)) olarak işaretlenmiş yöntemleri içermez ve türetilmiş bir sınıftaki herhangi bir yöntem geçerliyse ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) temel sınıftaki Yöntemler aday değildir.

Aday işlev üyeleri ve bağımsız değişken listesi tanımlandıktan sonra, en iyi işlev üyesinin seçimi her durumda aynıdır:

*  Uygulanabilir aday işlev üyeleri kümesi verildiğinde, bu küme içindeki en iyi işlev üyesi bulunur. Küme yalnızca bir işlev üyesi içeriyorsa, bu işlev üyesi en iyi işlev üyesidir. Aksi halde, en iyi işlev üyesi, her bir işlev üyesinin [daha iyi işlev üyesinde](expressions.md#better-function-member)olan kuralları kullanarak diğer tüm işlev üyeleriyle karşılaştırıldığı belirtilen bağımsız değişken listesine göre daha iyi olan bir işlev üyesidir. Tüm diğer işlev üyelerinden daha iyi olan tam olarak bir işlev üyesi yoksa, işlev üye çağrısı belirsizdir ve bir bağlama zamanı hatası oluşur.

Aşağıdaki bölümlerde, koşullara ***uygun işlev üyesinin*** ve ***daha iyi işlev üyesinin***tam anlamları tanımlanacaktır.

#### <a name="applicable-function-member"></a>Geçerli işlev üyesi

Bir işlev üyesi, aşağıdakilerin tümü doğru olduğunda `A` bağımsız değişken listesine göre ***geçerli bir işlev üyesi*** olarak kabul edilir:

*  `A` her bir bağımsız değişken, [karşılık gelen parametrelerde](expressions.md#corresponding-parameters)açıklandığı gibi işlev üye bildirimindeki bir parametreye karşılık gelir ve hiçbir bağımsız değişkenin karşılık geldiği parametre isteğe bağlı bir parametredir.
*  `A`her bağımsız değişken için, bağımsız değişkenin (örn. değer, `ref`veya `out`) parametre geçirme modu, karşılık gelen parametrenin parametre geçirme moduyla aynıdır ve
   *  bir değer parametresi veya bir parametre dizisi için, bağımsız değişkenden karşılık gelen parametrenin türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) var veya
   *  bir `ref` veya `out` parametresi için bağımsız değişkenin türü karşılık gelen parametrenin türüyle aynıdır. All, bir `ref` veya `out` parametresi geçilen bağımsız değişken için bir diğer addır.

Bir parametre dizisi içeren bir işlev üyesi için, işlev üyesi Yukarıdaki kurallar tarafından geçerliyse, bunun ***normal biçiminde***geçerli olduğu söylenir. Bir parametre dizisi içeren bir işlev üyesi normal biçiminde geçerli değilse, işlev üyesi ***genişletilmiş biçimde***uygulanabilir olabilir:

*  Genişletilmiş form, işlev üyesi bildirimindeki parametre dizisinin parametre dizisinin öğe türünün sıfır veya daha fazla değer parametresiyle değiştirilerek oluşturulur ve bağımsız değişken listesindeki bağımsız değişkenlerin sayısı `A`, toplam parametre sayısıyla eşleşir. `A`, işlev üyesi bildirimindeki sabit parametrelerin sayısından daha az bağımsız değişken içeriyorsa, işlev üyesinin genişletilmiş biçimi oluşturulamaz ve bu nedenle geçerli değildir.
*  Aksi takdirde genişletilmiş form, bağımsız değişkeninin parametre geçirme modu, karşılık gelen parametrenin parametre geçirme moduyla aynı olduğunda, `A` her bir bağımsız değişken için geçerlidir ve
   *  sabit bir değer parametresi veya genişletme tarafından oluşturulan bir değer parametresi için, bağımsız değişkenin türünden, karşılık gelen parametrenin türüne örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) vardır veya
   *  bir `ref` veya `out` parametresi için bağımsız değişkenin türü karşılık gelen parametrenin türüyle aynıdır.

#### <a name="better-function-member"></a>Daha iyi işlev üyesi

Daha iyi işlev üyesini belirleme amaçları doğrultusunda, yalnızca bağımsız değişken ifadelerinin orijinal bağımsız değişken listesinde göründükleri sırada kendilerini içeren bir aşağı açılan bağımsız değişken listesi oluşturulur.

Aday işlev üyelerinin her biri için parametre listeleri aşağıdaki şekilde oluşturulur:

*  İşlev üyesi yalnızca genişletilmiş biçimde uygulanabiliyorsa genişletilmiş form kullanılır.
*  Karşılık gelen bağımsız değişken içermeyen isteğe bağlı parametreler parametre listesinden kaldırılır
*  Parametreler, bağımsız değişken listesindeki ilgili bağımsız değişkenle aynı konumda gerçekleşmeleri için yeniden sıralanmaz.

Bağımsız değişken listesi `A` verildiğinde `{E1, E2, ..., En}` ve iki uygulanabilir ***işlev üyesi `Mp`*** ve `Mq` `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}`parametre türleri ile `Mp`

*  Her bağımsız değişken için, `Ex` 'den `Qx` örtük dönüştürme `Ex` ' den `Px`' a örtük dönüşümden daha iyi değildir ve
*  en az bir bağımsız değişken için `Ex` 'den `Px` dönüştürme `Ex` 'dan `Qx`dönüştürenden daha iyidir.

Bu değerlendirmeyi gerçekleştirirken, `Mp` veya `Mq` genişletilmiş biçimde geçerliyse, `Px` veya `Qx` parametre listesinin genişletilmiş biçimindeki bir parametreye başvurur.

Parametre türü dizileri `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}` eşdeğerdir (yani, her bir `Pi` karşılık gelen `Qi`bir kimlik dönüştürmesi vardır), daha iyi işlev üyesini belirleyebilmek için aşağıdaki bağlama kuralları sırasıyla uygulanır.

*  `Mp` genel olmayan bir yöntem ise ve `Mq` genel bir yöntem ise, `Mp` `Mq`daha iyidir.
*  Aksi takdirde, `Mp` normal biçiminde uygulanabiliyorsa ve `Mq` bir `params` dizisine sahipse ve yalnızca genişletilmiş biçimde geçerliyse, `Mp` `Mq`daha iyidir.
*  Aksi takdirde, `Mp` `Mq`daha fazla sayıda parametre içeriyorsa `Mp` `Mq`daha iyidir. Her iki yöntem de `params` dizileri içeriyorsa ve yalnızca genişletilmiş formlarında uygulanabildiğinde bu durum oluşabilir.
*  Aksi takdirde, `Mp` tüm parametrelerinin karşılık gelen bir bağımsız değişkeni varsa, varsayılan bağımsız değişkenlerin `Mq` ' de en az bir isteğe bağlı parametre yerine kullanılması gerekiyorsa `Mp` `Mq`daha iyidir.
*  Aksi takdirde, `Mp` `Mq`daha fazla parametre türü varsa `Mp` `Mq`daha iyidir. `{R1, R2, ..., Rn}` ve `{S1, S2, ..., Sn}` ' nin örneklenmiş ve genişletilmemiş parametre türlerini `Mp` ve `Mq`temsil etmesini sağlar. `Mp`parametre türleri, her bir parametre için `Mq`' den daha belirgin `Sx``Rx` ve en az bir parametre için `Rx` `Sx`daha büyüktür:
   *  Tür parametresi, tür olmayan bir parametreden daha az özgüdür.
   *  Yinelemeli olarak, oluşturulmuş bir tür, en az bir tür bağımsız değişkeni daha büyükse ve hiçbir tür bağımsız değişkeni diğer bir tür bağımsız değişkeniyle daha az özel değilse, oluşturulmuş bir tür farklı oluşturulmuş türden (aynı sayıda tür bağımsız değişkenle) daha özgüdür.
   *  Bir dizi türü, birincinin öğe türü ikincisinin öğe türünden daha belirgin ise, başka bir dizi türünden (aynı sayıda boyut ile) daha özgüdür.
*  Aksi takdirde, bir üye yükseltilmemiş olmayan bir işleçtir ve diğeri bir yükseltilmemiş işleçse, yükseltilmemiş olmayan bir tane daha iyidir.
*  Aksi halde, hiçbir işlev üyesi daha iyidir.

#### <a name="better-conversion-from-expression"></a>Deyimden daha iyi dönüştürme

Bir ifadeden `E` bir tür `T1`ve bir ifadeden `E` bir tür `T2`dönüştüren `C2` örtük bir dönüştürme olan örtük bir dönüştürme `C1` verildiğinde, `C1` `C2` tam olarak eşleşmiyorsa ve aşağıdakilerden en az biri olan `E` `T2` ***daha iyi bir dönüştürme*** olur:

* `E` `T1` tam olarak eşleşiyor ([tam eşleştirme ifadesi](expressions.md#exactly-matching-expression))
* `T1`, `T2` daha iyi bir dönüştürme hedefi ([daha iyi dönüştürme hedefi](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Tam eşleştirme Ifadesi

Bir ifade `E` ve bir tür `T`verildiğinde, aşağıdakilerden biri varsa `T` `E` tam olarak eşleşir:

*  `E` `S`bir tür ve `S` bir kimlik dönüştürmesi vardır `T`
*  `E` anonim bir işlevdir, `T` bir temsilci türü `D` ya da bir ifade ağacı türü `Expression<D>` ve aşağıdakilerden birini içerir:
   *  Çıkarılan bir dönüş türü, `D` ([çıkartılan dönüş türü](expressions.md#inferred-return-type)) parametre listesi bağlamında `E` için `X` vardır ve `X` bir kimlik dönüştürmesi `D` dönüş türüne mevcuttur
   *  `E` zaman uyumsuz değil ve `D` bir dönüş türüne sahip `Y` ya da `E` zaman uyumsuz ve `D` bir dönüş türü `Task<Y>`ve aşağıdakilerden biri şunları içerir:
      * `E` gövdesi tam olarak eşleşen bir ifadedir `Y`
      * `E` gövdesi, her dönüş deyiminin tam olarak eşleşen bir ifade döndürdüğü bir deyim bloğudur `Y`

#### <a name="better-conversion-target"></a>Daha iyi dönüştürme hedefi

`T1` ve `T2`iki farklı tür verildiğinde `T1`, `T2` ve `T1` arasında örtük dönüştürme yoksa aşağıdakilerden en az biri varsa, `T2` daha iyi bir dönüştürme hedefidir:

*  `T1` 'den `T2` örtük bir dönüştürme var
*  `T1`, bir temsilci türü `D1` ya da bir ifade ağacı türü `Expression<D1>``T2` bir temsilci türü `D2` veya bir ifade ağacı türü `Expression<D2>`, `D1` bir dönüş türü `S1` ve aşağıdakilerden biri tutalardan biridir:
   * `D2` void döndürüyor
   * `D2`, `S2`bir dönüş türüne sahiptir ve `S1` daha iyi bir dönüştürme hedefidir `S2`
*  `T1` `Task<S1>`, `T2` `Task<S2>`ve `S1` daha iyi bir dönüştürme hedefi `S2`
*  `T1` `S1` veya `S1?` `S1` işaretli bir integral türüdür ve `T2`, `S2` işaretsiz bir integral türü olduğu `S2?` veya `S2`. Özellikle:
   * `S1` `sbyte` ve `S2` `byte`, `ushort`, `uint`veya `ulong`
   * `S1` `short` ve `S2` `ushort`, `uint`veya `ulong`
   * `S1` `int` ve `S2` `uint`veya `ulong`
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

*  Kısmi tür çıkarımı: `dynamic` türünde bir bağımsız değişkene doğrudan veya dolaylı olarak bağımlı olmayan herhangi bir tür bağımsız değişkeni, [tür çıkarımı](expressions.md#type-inference)kuralları kullanılarak çıkarsanacaktır. Kalan tür bağımsız değişkenleri bilinmiyor.
*  Kısmi uygulanabilirlik denetimi: uygulanabilirliği, [uygulanabilir işlev üyesine](expressions.md#applicable-function-member)göre denetlenir, ancak türleri bilinmiyor olan parametreleri yoksayar.
*  Bu testi hiçbir aday geçirmediğinde, derleme zamanı hatası oluşur.

### <a name="function-member-invocation"></a>İşlev üyesi çağırma

Bu bölümde, belirli bir işlev üyesini çağırmak için çalışma zamanında gerçekleşen işlem açıklanmaktadır. Bir bağlama zamanı işleminin, büyük olasılıkla bir aday işlev üyesi kümesine aşırı yükleme çözümlemesi uygulayarak, çağırmak için belirli bir üyeyi daha önce belirlediğini kabul edilir.

Çağırma işlemini açıklama amaçlarıyla, işlev üyeleri iki kategoriye ayrılmıştır:

*  Statik işlev üyeleri. Bunlar örnek oluşturucular, statik yöntemler, statik özellik erişimcileri ve Kullanıcı tanımlı işleçlerdir. Statik işlev üyeleri her zaman sanal değildir.
*  Örnek işlevi üyeleri. Bunlar örnek yöntemleridir, örnek özellik erişimcileri ve Dizin Oluşturucu erişimcileri. Örnek işlevi üyeleri sanal olmayan veya sanal değil, her zaman belirli bir örnek üzerinde çağrılır. Örnek bir örnek ifadesi tarafından hesaplanır ve işlev üyesi içinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir hale gelir.

Bir işlev üyesi çağrısının çalışma zamanı işleme aşağıdaki adımlardan oluşur; burada `M` İşlev üyesidir ve `M` bir örnek üyesiyse `E` örnek ifadedir:

*  `M` statik bir işlev üyesiyse:
   * Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.
   * `M` çağrılır.

*  `M` bir *value_type*belirtilen örnek işlev üyesiyse:
   * `E` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
   * `E` bir değişken olarak sınıflandırılmadıysa, `E`türünün geçici bir yerel değişkeni oluşturulur ve bu değişkene `E` değeri atanır. `E` daha sonra bu geçici yerel değişkene bir başvuru olarak yeniden sınıflanır. Geçici değişken `M`içinde `this` olarak erişilebilir, ancak başka hiçbir şekilde erişilemez. Bu nedenle, yalnızca `E` gerçek bir değişken olduğunda, çağıranın `M` `this`yaptığı değişiklikleri gözlemlemek mümkün olur.
   * Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.
   * `M` çağrılır. `E` başvurduğu değişken `this`tarafından başvurulan değişken olur.

*  `M` bir *reference_type*belirtilen örnek işlev üyesiyse:
   * `E` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
   * Bağımsız değişken listesi, [bağımsız değişken listelerinde](expressions.md#argument-lists)açıklandığı şekilde değerlendirilir.
   * `E` türü bir *value_type*ise, `E` `object`türüne dönüştürmek için bir paketleme dönüştürmesi ([kutulama dönüştürmeleri](types.md#boxing-conversions)) gerçekleştirilir ve `E` aşağıdaki adımlarda `object` türünde olarak değerlendirilir. Bu durumda, `M` yalnızca bir `System.Object`üyesi olabilir.
   * `E` değeri geçerli olacak şekilde işaretlendi. `E` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
   * Çağrılacak işlev üyesi uygulama belirlenir:
     * `E` bağlama zamanı türü bir arabirimdir, çağrılacak işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasıdır. Bu işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasını belirlemekte kullanılacak arabirim eşleme kuralları ([arabirim eşleme](interfaces.md#interface-mapping)) uygulanarak belirlenir.
     * Aksi takdirde, `M` bir sanal işlev üyesiyse, çağrılacak işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türü tarafından verilen `M` uygulamasıdır. Bu işlev üyesi, `E`tarafından başvurulan örneğin çalışma zamanı türüne göre `M` en fazla türetilmiş uygulamayı ([sanal yöntemleri](classes.md#virtual-methods)) belirlemek için kurallar uygulanarak belirlenir.
     * Aksi takdirde, `M` sanal olmayan bir işlev üyesi ve çağrılacak işlev üyesi `M` kendisidir.
   * Yukarıdaki adımda belirlenen işlev üyesi uygulama çağrılır. `E` başvurduğu nesne `this`tarafından başvurulan nesne haline gelir.

#### <a name="invocations-on-boxed-instances"></a>Paketlenmiş örneklerde etkinleştirmeleri

Bir *value_type* uygulanan bir işlev üyesi, aşağıdaki durumlarda bu *value_type* paketlenmiş bir örneği aracılığıyla çağrılabilir:

*  İşlev üyesi, türden devralınan bir yöntemin `override` `object` ve `object`türünde bir örnek ifadesi aracılığıyla çağrılır.
*  İşlev üyesi bir arabirim işlevi üyesinin bir uygulamasıdır ve bir *interface_type*örnek ifadesi aracılığıyla çağrılır.
*  İşlev üyesi bir temsilci aracılığıyla çağrıldığında.

Bu durumlarda kutulanmış örnek, *value_type*bir değişken içeren olarak değerlendirilir ve bu değişken, işlev üyesi çağrısı içinde `this` tarafından başvurulan değişken olur. Özellikle bu, bir işlev üyesinin paketlenmiş bir örnek üzerinde çağrıldığı anlamına gelir, işlev üyesinin kutulanmış örnekte bulunan değeri değiştirmesi mümkündür.

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

*Değişmez* değer ([sabit değerler](lexical-structure.md#literals)) içeren bir *primary_expression* değer olarak sınıflandırıldı.


### <a name="interpolated-strings"></a>Ara değerli dizeler

*İnterpolated_string_expression* , bir `$` işaretinden sonra, `{` ve `}`ile ayrılmış, ifadeler ve biçimlendirme belirtimlerini içeren boşluklar ve düz bir dize değişmez değeri oluşur. Enterpolasyonlu dize ifadesi, [enterpolasyonlu dize sabit değerleri](lexical-structure.md#interpolated-string-literals)içinde açıklandığı gibi ayrı belirteçlere bölünmüş bir *interpolated_string_literal* sonucudur.

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

Bir ilişkilendirmedeki *constant_expression* `int`örtülü dönüştürmeye sahip olmalıdır.

Bir *interpolated_string_expression* değer olarak sınıflandırılır. Örtülü olarak enterpolasyonlu bir dize dönüştürmesi ([örtük olarak enterpolasyonlu dize dönüştürmeleri](conversions.md#implicit-interpolated-string-conversions)) ile `System.IFormattable` veya `System.FormattableString` olarak dönüştürülürse, enterpolasyonlu dize ifadesinde bu tür Aksi takdirde, türü `string`vardır.

Enterpolasyonlu bir dizenin türü `System.IFormattable` veya `System.FormattableString`, anlamı bir `System.Runtime.CompilerServices.FormattableStringFactory.Create`çağrıdır. Tür `string`ise, ifadenin anlamı `string.Format`bir çağrıdır. Her iki durumda da, çağrının bağımsız değişken listesi, her ilişkilendirme için yer tutucular içeren bir biçim dizesi değişmez değerinden ve yer sahiplerine karşılık gelen her bir ifade için bir bağımsız değişkenle oluşur.

Biçim dizesi değişmez değeri aşağıdaki gibi oluşturulur; burada `N`, *interpolated_string_expression*içindeki enterpolasyonun sayısıdır:

*  Bir *interpolated_regular_string_whole* veya *interpolated_verbatim_string_whole* `$` işaretini izliyorsa, biçim dizesi değişmez değeri bu belirteç olur.
*  Aksi takdirde, biçim dizesi değişmez değeri şunlardan oluşur: 
   *  İlk *interpolated_regular_string_start* veya *interpolated_verbatim_string_start*
   *  Sonra `0` `I` her numara için `N-1`: 
      * `I` ondalık temsili
      * Ardından, karşılık gelen *enterpolasyon* bir *constant_expression*içeriyorsa, bir `,` (virgül) ve ardından *constant_expression* değerinin ondalık temsili gelir.
      * *İnterpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* veya *interpolated_verbatim_string_end* karşılık gelen ilişkilendirmeyi hemen takip edin.

Sonraki bağımsız değişkenler, sırasıyla *enterpolasyonların* (varsa) *ifadeleridir* .

TODO: örnekler.


### <a name="simple-names"></a>Basit adlar

*Simple_name* bir tanımlayıcıdan, isteğe bağlı olarak bir tür bağımsız değişken listesi tarafından oluşur:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

*Simple_name* , `I` tek bir tanımlayıcı olduğu ve `<A1,...,Ak>` isteğe bağlı bir *type_argument_list*olduğu `I<A1,...,Ak>`form `I` veya form olabilir. *Type_argument_list* belirtilmediğinde, sıfır `K` göz önünde bulundurun. *Simple_name* değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:

*  `K` sıfırsa ve *simple_name* bir *blok* içinde görünürse ve *blok*(veya kapsayan *bloğun*) yerel değişken bildirim alanı ([Bildirimler](basic-concepts.md#declarations)), ad `I`olan bir yerel değişken, parametre veya sabit içeriyorsa, *simple_name* bu yerel değişkene, parametreye veya sabitine başvurur ve bir değişken veya değer olarak sınıflandırılmaktadır.
*  `K` sıfırsa ve *simple_name* bir genel yöntem bildiriminin gövdesinde yer alıyorsa ve bu bildirimde `I`adında bir tür parametresi varsa, *simple_name* bu tür parametresine başvurur.
*  Aksi halde, her bir örnek türü için `T` ([örnek türü](classes.md#the-instance-type)), hemen kapsayan tür bildiriminin örnek türüyle başlar ve her bir kapsayan sınıfın ya da yapı bildiriminin örnek türüne (varsa) devam edin:
   *  `K` sıfırsa ve `T` bildirimi, ad `I`olan bir tür parametresi içeriyorsa, *simple_name* bu tür parametresine başvurur.
   *  Aksi halde, `K` tür bağımsız değişkenleriyle `T` `I` üye araması ([üye araması](expressions.md#member-lookup)) bir eşleşme üretir:
      * `T`, hemen kapsayan sınıfın veya yapı türünün örnek türü ise ve arama bir veya daha fazla yöntemi tanımlarsa, sonuç, ilişkili bir örnek ifadesi `this`olan bir yöntem grubudur. Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.
      * Aksi takdirde `T`, arama bir örnek üye tanımlarsa ve başvuru bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcisinin gövdesinde gerçekleşirse, sonuç, form `this.I`bir üye erişimi ([üye erişimi](expressions.md#member-access)) ile aynıdır ve bu, hemen kapsayan sınıf veya yapı türünün örnek türüdür. Bu yalnızca `K` sıfır olduğunda meydana gelebilir.
      * Aksi halde, sonuç, `T.I` veya `T.I<A1,...,Ak>`bir üye erişimi ([üye erişimi](expressions.md#member-access)) ile aynıdır. Bu durumda, *simple_name* bir örnek üyesine başvurması için bağlama zamanı hatası olur.

*  Aksi takdirde `N`her bir ad alanı için *simple_name* gerçekleştiği ad alanından başlayarak, kapsayan her ad alanıyla (varsa) devam eder ve genel ad alanıyla sona eriyor, aşağıdaki adımlar bir varlık bulunana kadar değerlendirilir:
   *  `K` sıfırsa ve `I` `N`bir ad alanının adı ise:
      * *Simple_name* gerçekleştiği konum `N` için bir ad alanı bildirimiyle Çevrese ve ad alanı bildirimi, adı `I` ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *simple_name* belirsizdir ve derleme zamanı hatası oluşur.
      * Aksi takdirde *simple_name* , `N``I` adlı ad alanı anlamına gelir.
   *  Aksi takdirde, `N` ad `I` ve `K` tür parametrelerine sahip erişilebilir bir tür içeriyorsa:
      * `K` sıfırsa ve *simple_name* gerçekleştiği konum `N` için bir ad alanı bildirimiyle Çevrese ve ad alanı bildirimi ad using_alias_directive adını bir ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya * `I`* içeriyorsa, *simple_name* belirsizdir ve bir derleme zamanı hatası oluşur.
      * Aksi takdirde *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan türe başvurur.
   *  Aksi takdirde, *simple_name* gerçekleştiği konum `N`için bir ad alanı bildirimiyle çevrelenmiş ise:
      * `K` sıfırsa ve ad alanı bildirimi, adı `I` içeri aktarılan bir ad alanı veya türle ilişkilendirir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *simple_name* bu ad alanına veya türe başvurur.
      * Aksi takdirde, *using_namespace_directive*s ve ad alanı bildiriminin *using_static_directive*tarafından içeri aktarılan ad alanları ve tür bildirimleri, ad `I` ve `K` tür parametrelerine sahip tek bir erişilebilir tür veya uzantı olmayan statik üye içeriyorsa, *simple_name* bu türü veya verilen tür bağımsız değişkenleriyle oluşturulan üyeyi ifade eder.
      * Aksi takdirde, ad alanı bildiriminin *using_namespace_directive*tarafından içeri aktarılan ad alanları ve türler, ad `I` ve `K` tür parametrelerine sahip uzantı dışı bir statik üye içeriyorsa, *simple_name* belirsizdir ve bir hata oluşur.

   Bu adımın tamamı bir *namespace_or_type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) işlenirken ilgili adıma tam olarak paralel olduğunu unutmayın.

*  Aksi takdirde, *simple_name* tanımsız olur ve derleme zamanı hatası oluşur.


### <a name="parenthesized-expressions"></a>Parantez içinde ifadeler

*Parenthesized_expression* parantez içine alınmış bir *ifadeden* oluşur.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

Parantez içindeki *ifade* hesaplanarak bir *parenthesized_expression* değerlendirilir. Parantez içindeki *ifade* bir ad alanı veya tür alıyorsa, bir derleme zamanı hatası oluşur. Aksi takdirde, *parenthesized_expression* sonucu içerilen *ifadenin*değerlendirmesinin sonucudur.

### <a name="member-access"></a>Üye erişimi

Bir *member_access* *primary_expression*, *predefined_type*ya da *qualified_alias_member*, ardından bir "`.`" belirteci ve ardından bir *tanımlayıcı*tarafından ve isteğe bağlı olarak, bir *type_argument_list*tarafından oluşur.

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

*Qualified_alias_member* üretim, [ad alanı diğer adı niteleyicilerine](namespaces.md#namespace-alias-qualifiers)göre tanımlanır.

*Member_access* , `E` bir birincil ifade olduğu, `I` tek bir tanımlayıcı ve `<A1, ..., Ak>` isteğe bağlı bir *type_argument_list*olduğu `E.I<A1, ..., Ak>`form `E.I` veya form olabilir. *Type_argument_list* belirtilmediğinde, sıfır `K` göz önünde bulundurun.

`dynamic` türünde bir *primary_expression* olan *member_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, derleyici üye erişimini `dynamic`türünde bir özellik erişimi olarak sınıflandırır. *Member_access* anlamını belirlemede aşağıdaki kurallar, *primary_expression*derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. Bu çalışma zamanı sınıflandırması bir yöntem grubuna yol açar, üye erişimi bir *invocation_expression* *primary_expression* olmalıdır.

*Member_access* değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:

*  `K` sıfırsa ve `E` bir ad alanı ve `E` `I`ada sahip bir iç içe ad alanı içeriyorsa, sonuç bu ad alanıdır.
*  Aksi takdirde, `E` bir ad alanı ise ve `E` ad `I` ve `K` tür parametrelerine sahip erişilebilir bir tür içeriyorsa, sonuç verilen tür bağımsız değişkenleriyle oluşturulur.
*  `E` bir *predefined_type* ya da tür olarak sınıflandırılan bir *primary_expression* ise, `E` bir tür parametresi değilse ve `I` `E` tür parametrelerine sahip `K`bir üye araması ([üye arama](expressions.md#member-lookup)) bir eşleşme üretirse,  değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:
   *  `I`, bir türü tanımlarsa, bu tür belirtilen tür bağımsız değişkenleriyle oluşturulur.
   *  `I` bir veya daha fazla yöntemi tanımlarsa, sonuç ilişkili örnek ifadesi olmayan bir yöntem grubudur. Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.
   *  `I` bir `static` özelliğini tanımlarsa, sonuç, ilişkili örnek ifadesi olmayan bir özellik erişimiydi.
   *  `I` bir `static` alanı tanımlarsa:
      * Alan `readonly` ve başvuru, alanın bildirildiği sınıf veya yapının statik oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani statik alanın değeri `E` `I`.
      * Aksi takdirde, sonuç bir değişkendir, yani statik alan `E` `I`.
   *  `I` bir `static` olayı tanımlarsa:
      * Başvuru, olayın bildirildiği sınıf veya yapı içinde oluşursa ve olay *event_accessor_declarations* ([Olaylar](classes.md#events)) olmadan bildirilirse, `E.I` tam olarak `I` statik bir alan gibi işlenir.
      * Aksi halde, sonuç, ilişkili örnek ifadesi olmayan bir olay erişimiydi.
   *  `I` bir sabiti tanımlarsa, sonuç bir değerdir, yani bu sabit değeridir.
    * `I` bir numaralandırma üyesini tanımlarsa, sonuç bir değerdir, yani bu numaralandırma üyesinin değeri.
    * Aksi takdirde, `E.I` geçersiz bir üye başvurusu ve derleme zamanı hatası oluşur.
*  `E` bir özellik erişimi, Dizin Oluşturucu erişimi, değişken veya değer, `T`türü ve `K` türü bağımsız değişkenlerle `T` `I` üye araması ([üye arama](expressions.md#member-lookup)) bir eşleşme üretir, `E.I` değerlendirilir ve aşağıdaki gibi sınıflandırılacaktır:
   *  İlk olarak, `E` bir özellik veya Dizin Oluşturucu erişimi ise, özellik veya Dizin Oluşturucu erişimi değeri elde edilir ([Ifadelerin değerleri](expressions.md#values-of-expressions)) ve `E` bir değer olarak yeniden sınıflanır.
   *  `I` bir veya daha fazla yöntemi tanımlarsa, sonuç `E`ilişkili bir örnek ifadesi olan bir yöntem grubudur. Bir tür bağımsız değişken listesi belirtilmişse, genel bir yöntemi ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)) çağırmak için kullanılır.
   *  `I` bir örnek özelliğini tanımlarsa,
      * `E` `this`, `I` otomatik olarak uygulanan bir özelliği ([otomatik olarak uygulanan özellikler](classes.md#automatically-implemented-properties)) bir ayarlayıcı olmadan tanımlar ve başvuru, bir sınıf veya yapı `T`türü için örnek oluşturucu içinde meydana gelir. Bu durumda, sonuç bir değişkendir, yani `I` tarafından verilen `T` örneğindeki `this`tarafından verilen otomatik özellik için gizli yedekleme alanı.
      * Aksi takdirde sonuç, `E`ilişkili bir örnek ifadesiyle bir özellik erişimiydi.
   *  `T` bir *class_type* ve bu *class_type*bir örnek alanı `I` tanımlar:
      * `E` değeri `null`, bir `System.NullReferenceException` oluşturulur.
      * Aksi halde, alan `readonly` ve başvuru, alanın bildirildiği sınıfın örnek oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani alanın değeri `E`tarafından başvurulan nesnede `I`.
      * Aksi takdirde, sonuç bir değişkendir, ancak alan `E`tarafından başvurulan nesnedeki `I`.
   *  `T` bir *struct_type* ve bu *struct_type*bir örnek alanı `I` tanımlar:
      * `E` bir değer ise veya alan `readonly` ve başvuru, alanın bildirildiği yapının örnek oluşturucusunun dışında oluşursa, sonuç bir değerdir, yani alanın değeri, `E`tarafından verilen yapı örneğinde `I`.
      * Aksi takdirde, sonuç bir değişkendir, yani alan `E`tarafından verilen yapı örneğinde `I`.
   *  `I` bir örnek olayı tanımlarsa:
      * Başvuru, olayın bildirildiği sınıf veya yapı içinde oluşursa ve olay *event_accessor_declarations* ([Olaylar](classes.md#events)) olmadan bildirildiği ve başvuru bir `+=` veya `-=` işlecinin sol tarafında gerçekleşmemişse, `E.I` tam `I` olarak bir örnek alanı olduğu gibi işlenir.
      * Aksi takdirde, sonuç `E`ilişkili bir örnek ifadesiyle bir olay erişimiydi.
*  Aksi takdirde, bir uzantı yöntemi çağırma ([uzantı yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak `E.I` işlemek için bir girişimde bulunuldu. Bu başarısız olursa, `E.I` geçersiz bir üye başvurusu ve bir bağlama zamanı hatası oluşur.

#### <a name="identical-simple-names-and-type-names"></a>Aynı basit adlar ve tür adları

Formun üye erişiminin `E.I`, `E` tek bir tanımlayıcıdır ve *simple_name* ([basit adlar](expressions.md#simple-names)) olarak `E` anlamı bir sabit, alan, özellik, yerel değişken veya bir *`E`* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) olarak type_name anlamı ile aynı türde bir parametre ise, her ikisi de `E` olası anlamlarıyla izin verilir. `I` her iki durumda da `E` türünde bir üye olması gerektiğinden `E.I` iki olası anlamı hiçbir zaman belirsiz değildir. Diğer bir deyişle, kural yalnızca statik üyelere ve derleme zamanı hatası oluşması durumunda `E` iç içe geçmiş türlerine erişime izin verir. Örneğin:
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
, `G < A` ve `B > (7)`iki bağımsız değişkenle `F` çağrısı olarak yorumlanamaz. Alternatif olarak, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanabilecek.

Bir belirteç dizisi bir *simple_name* ([basit adlar](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)) veya *pointer_member_access* ([işaretçi üyesi erişimi](unsafe-code.md#pointer-member-access)) ile biten bir *type_argument_list* ([tür bağımsız değişkenleri](types.md#type-arguments)) olarak ayrıştırılamıyorsa, kapatma `>` belirtecini izleyen belirteç incelenir. Bunlardan biri ise
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
ardından *type_argument_list* *simple_name*bir parçası olarak tutulur, *member_access* veya *pointer_member_access* ve belirteçleri dizisinin diğer olası ayrıştırması atılır. Aksi takdirde, belirteç dizisinin başka bir olası ayrıştırması olmasa bile type_argument_list *simple_name*, *member_access* veya *pointer_member_access*bir parçası olarak kabul edilmez. Bu kuralların bir *namespace_or_type_name* *type_argument_list* ayrıştırılırken ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) uygulanmadığını unutmayın. İfade
```csharp
F(G<A,B>(7));
```
Bu kurala göre, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanacaktır. Deyimler
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
her biri, iki bağımsız değişkenle `F` çağrısı olarak yorumlanır. İfade
```csharp
x = F < A > +y;
```
, bir *type_argument_list* ve ardından ikili artı işleci gelen bir *simple_name* yerine, ' den daha az işleç, büyüktür operatörü ve birli artı `x = (F < A) > (+y)`işleci olarak yorumlanır. İfadesinde
```csharp
x = y is C<T> + z;
```
belirteçler `C<T>`, *type_argument_list*bir *namespace_or_type_name* olarak yorumlanır.

### <a name="invocation-expressions"></a>Çağırma ifadeleri

Bir yöntemi çağırmak için bir *invocation_expression* kullanılır.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Aşağıdakilerden en az biri tutuyorsa, bir *invocation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)):

* *Primary_expression* `dynamic`derleme zamanı türü vardır.
* İsteğe bağlı *argument_list* en az bir bağımsız değişkeni derleme zamanı türü `dynamic` ve *primary_expression* bir temsilci türüne sahip değil.

Bu durumda, derleyici *invocation_expression* `dynamic`türünde bir değer olarak sınıflandırır. *İnvocation_expression* anlamını öğrenmek için aşağıdaki kurallar, *primary_expression* ve derleme zamanı türü `dynamic`olan bağımsız değişkenlerin derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. *Primary_expression* , derleme zamanı türü `dynamic`yoksa, bu durumda çağrı, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.

Bir *invocation_expression* *primary_expression* bir yöntem grubu ya da bir *delegate_type*değeri olmalıdır. *Primary_expression* bir yöntem grubu ise, *invocation_expression* bir yöntem çağrıdır ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)). *Primary_expression* bir *delegate_type*değeri ise, *invocation_expression* bir temsilci çağrıdır ([temsilci etkinleştirmeleri](expressions.md#delegate-invocations)). *Primary_expression* bir yöntem grubu veya bir *delegate_type*değeri değilse, bir bağlama zamanı hatası oluşur.

İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)), yöntemin parametreleri için değerler veya değişken başvuruları sağlar.

Bir *invocation_expression* değerlendirilme sonucu aşağıdaki gibi sınıflandırıldı:

*  *İnvocation_expression* , `void`döndüren bir yöntemi veya temsilciyi çağıralıyorsa, sonuç Nothing olur. Nothing olarak sınıflandırılan bir ifadeye, yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında veya bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) gövdesinde izin verilir. Aksi halde bağlama zamanı hatası oluşur.
*  Aksi takdirde, sonuç Yöntem veya temsilci tarafından döndürülen türün bir değeridir.

#### <a name="method-invocations"></a>Yöntem etkinleştirmeleri

Bir yöntem çağrısı için, invocation_expression *primary_expression* bir yöntem grubu olmalıdır. Yöntem grubu, çağrılacak bir yöntemi veya çağırmak için belirli bir yöntemi seçebileceğiniz aşırı yüklenmiş yöntemleri belirler. İkinci durumda, çağrılacak belirli yöntemin belirlenmesi, *argument_list*bağımsız değişkenlerin türleri tarafından belirtilen bağlamı temel alır.

`M` `M(A)`bir yöntem grubu (muhtemelen bir *type_argument_list*dahil) ve `A` isteğe bağlı bir *argument_list*olduğunda, bu form için bir yöntem çağrısı olan bağlama zamanı işleme, aşağıdaki adımlardan oluşur:

*  Yöntem çağrısı için aday yöntemler kümesi oluşturulur. Her yöntem için `F` Yöntem grubuyla ilişkili `M`:
   *  `F` genel değilse, şu durumlarda `F` bir adaydır:
      * `M`, tür bağımsız değişken listesine sahip değil ve
      * `F`, `A` ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) göre uygulanabilir.
   *  `F` geneldir ve `M` tür bağımsız değişken listesine sahip değilse, şu durumlarda `F` bir adaydır:
      * Tür çıkarımı ([tür çıkarımı](expressions.md#type-inference)) başarılı, çağrının tür bağımsız değişkenlerinin bir listesini, ve
      * Çıkarsanan tür bağımsız değişkenleri karşılık gelen yöntem türü parametreleri yerine eklendikten sonra, F parametre listesindeki tüm oluşturulmuş türler kısıtlamalarını karşılar ([kısıtlamalar karşılamaları](types.md#satisfying-constraints)) ve `F` parametre listesi `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerlidir.
   *  `F` geneldir ve `M` bir tür bağımsız değişken listesi içeriyorsa, şu durumlarda bir aday `F`:
      * `F`, tür bağımsız değişkeni listesinde sağlanan sayıda yöntem türü parametreye sahiptir ve
      * Tür bağımsız değişkenleri karşılık gelen yöntem türü parametreleri yerine eklendikten sonra, F parametre listesindeki tüm oluşturulmuş türler kısıtlamalarını karşılar ([kısıtlamalar karşılamaları](types.md#satisfying-constraints)) ve `F` parametre listesi `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerlidir.
*  Aday yöntemler kümesi yalnızca en çok türetilen türlerden Yöntemler içerecek şekilde azaltılır: `C.F` her bir yöntem Için, `C` `F` bildirildiği tür, `C` temel türünde bildirildiği tüm yöntemler kümeden kaldırılır. Ayrıca, `C` `object`dışında bir sınıf türü ise, bir arabirim türünde belirtilen tüm yöntemler kümeden kaldırılır. (Bu ikinci kural yalnızca Yöntem grubu, nesne dışındaki etkin bir temel sınıfa sahip olan bir tür parametresindeki üye aramasının ve boş olmayan etkin bir arabirim kümesinin sonucu olduğunda etkiler.)
*  Elde edilen aday yöntemler kümesi boşsa, aşağıdaki adımlar üzerinde daha fazla işleme terk edilir ve bunun yerine, çağırma yöntemi çağırma ([uzantı yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) olarak çağrıyı işlemek için bir girişimde bulunuldu. Bu başarısız olursa, geçerli hiçbir yöntem yoktur ve bir bağlama zamanı hatası oluşur.
*  Aday Yöntemler kümesinin en iyi yöntemi, [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak tanımlanır. Tek bir en iyi yöntem tanımlanamıyorsa, yöntem çağırma belirsizdir ve bir bağlama zamanı hatası oluşur. Aşırı yükleme çözümlemesi gerçekleştirirken, genel yöntemin parametreleri karşılık gelen yöntem türü parametreleri için tür bağımsız değişkenleri (sağlanan veya çıkartılan) değiştirildikten sonra değerlendirilir.
*  Seçilen en iyi yöntemin son doğrulaması gerçekleştirilir:
   * Yöntem, Yöntem grubu bağlamında onaylanır: en iyi yöntem bir statik yöntem ise, Yöntem grubu bir *simple_name* veya bir *member_access* bir tür aracılığıyla sonuçlanmış olmalıdır. En iyi yöntem bir örnek yöntemi ise, Yöntem grubu bir *simple_name*, bir değişken veya değer aracılığıyla bir *member_access* veya bir *base_access*sonuçlanmış olmalıdır. Bu gereksinimlerin hiçbiri doğru değilse, bir bağlama zamanı hatası oluşur.
   * En iyi yöntem genel bir yöntem ise, tür bağımsız değişkenleri (sağlanan veya çıkartılan) genel yöntemde belirtilen kısıtlamalara (karşılayan[kısıtlamalar](types.md#satisfying-constraints)) göre denetlenir. Herhangi bir tür bağımsız değişkeni tür parametresindeki karşılık gelen kısıtlamayı karşılamaz bir bağlama zamanı hatası oluşur.

Yukarıdaki adımlarla bağlama zamanında bir Yöntem seçildikten ve doğrulandıktan sonra, gerçek çalışma zamanı çağrısı, [dinamik aşırı yükleme çözümünün derleme zamanı denetiminde](expressions.md#compile-time-checking-of-dynamic-overload-resolution)açıklanan işlev üye çağırma kurallarına göre işlenir.

Yukarıda açıklanan çözüm kurallarının sezgisel etkisi aşağıdaki gibidir: bir yöntem çağrısı tarafından çağrılan belirli bir yöntemi bulmak Için, yöntem çağırma tarafından belirtilen türle başlayın ve en az bir tane geçerli olana kadar devralma zincirini devam edin. erişilebilir, geçersiz kılma olmayan yöntem bildirimi bulundu. Daha sonra tür çıkarımı ve aşırı yükleme çözümlemesini, bu tür içinde belirtilen geçerli, erişilebilir, geçersiz kılma olmayan yöntemler kümesi ve bu nedenle seçilen yöntemi çağırmak için gerçekleştirin. Hiçbir yöntem bulunmazsa, çağırma yöntemini bir genişletme yöntemi çağrısı olarak işlemeyi deneyin.

#### <a name="extension-method-invocations"></a>Genişletme yöntemi etkinleştirmeleri

Bir yöntem çağrısında ([paketlenmiş örneklerde çağırma](expressions.md#invocations-on-boxed-instances)), formlardan birinin
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
çağrının normal işlemesi uygulanabilir bir yöntem bulamıyorsa, yapıyı uzantı metodu çağrısı olarak işlemek için bir girişimde bulunuldu. Eğer *Expr* veya *bağımsız* değişkenlerin herhangi birinde derleme zamanı türü `dynamic`varsa, genişletme yöntemleri uygulanmaz.

Amaç, karşılık gelen statik yöntem çağrısının gerçekleşmesi için en iyi *type_name* `C`bulma yöntemidir:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Bir genişletme yöntemi, şu durumlarda ***uygun*** `Ci.Mj`:

*  `Ci` genel olmayan, iç içe olmayan bir sınıftır
*  `Mj` adı *tanımlayıcı*
*  `Mj`, yukarıda gösterildiği gibi bağımsız değişkenlere statik bir yöntem olarak uygulandığında erişilebilir ve uygulanabilir
*  Bir örtük kimlik, başvuru veya paketleme dönüştürmesi, *ifadenin* `Mj`ilk parametresinin türüne.

`C` araması şu şekilde devam eder:

*  En yakın kapsayan ad alanı bildirimiyle başlayarak, kapsayan her bir ad alanı bildirimine devam edin ve kapsayan derleme birimi ile sona ermek üzere, bir dizi genişletme yöntemini bulmak için art arda denemeler yapılır:
   * Verilen ad alanı veya derleme birimi, uygun genişletme yöntemleriyle `Mj``Ci` genel olmayan tür bildirimlerini doğrudan içeriyorsa, bu uzantı yöntemlerinin kümesi aday kümesidir.
   * *Using_static_declarations* tarafından içeri aktarılan ve doğrudan verilen ad alanındaki veya derleme `Mj`birimindeki *using_namespace_directive*s tarafından içeri aktarılan ad alanları için `Ci`, bu genişletme yöntemlerinin kümesi aday kümesidir.
*  Herhangi bir kapsayan ad alanı bildiriminde veya derleme biriminde hiçbir aday kümesi bulunamazsa, derleme zamanı hatası oluşur.
*  Aksi halde, aşırı yükleme çözümlemesi aday kümesine ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) açıklandığı şekilde uygulanır. Tek bir en iyi yöntem bulunamazsa, derleme zamanı hatası oluşur.
*  `C`, en iyi yöntemin bir genişletme yöntemi olarak bildirildiği türdür.

`C` bir hedef olarak kullanıldığında, yöntem çağrısı daha sonra statik bir yöntem çağırma ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) olarak işlenir.

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

Örnekte, `B`yöntemi ilk uzantı yöntemine göre önceliklidir ve `C`yöntemi her iki uzantı yönteminden önceliklidir.

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
`D.G` `C.G`üzerinden önceliklidir ve `E.F` `D.F` ve `C.F`göre önceliklidir.

#### <a name="delegate-invocations"></a>Temsilci etkinleştirmeleri

Bir temsilci çağrısı için, *invocation_expression* *primary_expression* bir *delegate_type*değeri olmalıdır. Ayrıca, *delegate_type* *delegate_type*aynı parametre listesi olan bir işlev üyesi olması halinde *delegate_type* , *invocation_expression* *argument_list* göre uygulanabilir ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) olmalıdır.

`D(A)`, bir *delegate_type* *primary_expression* `D` ve `A` isteğe bağlı bir *argument_list*olduğunda, bu form için bir temsilci çağrısı çalıştırma zamanı işleme, aşağıdaki adımlardan oluşur:

*  `D` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
*  `D` değeri geçerli olacak şekilde işaretlendi. `D` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
*  Aksi takdirde, `D` bir temsilci örneğine başvurudur. İşlev üyesi etkinleştirmeleri ([dinamik aşırı yükleme çözümlemesi Için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), temsilcinin çağırma listesindeki çağrılabilir varlıkların her birinde gerçekleştirilir. Örnek ve örnek yönteminden oluşan çağrılabilir varlıklar için, çağırma örneği çağrılabilir varlıkta bulunan örneğidir.

### <a name="element-access"></a>Öğe erişimi

Bir *element_access* *primary_no_array_creation_expression*, ardından bir "`[`" belirteci ve ardından bir *argument_list*ve ardından "`]`" belirteci ile oluşur. *Argument_list* , virgülle ayrılmış bir veya daha fazla *bağımsız değişkenden*oluşur.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

Bir *element_access* *argument_list* `ref` veya `out` bağımsız değişken içermesine izin verilmez.

Aşağıdakilerden en az biri tutuyorsa, bir *element_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)):

* *Primary_no_array_creation_expression* `dynamic`derleme zamanı türü vardır.
* *Argument_list* en az bir ifadesinin derleme zamanı türü `dynamic` ve *primary_no_array_creation_expression* bir dizi türüne sahip değil.

Bu durumda, derleyici *element_access* `dynamic`türünde bir değer olarak sınıflandırır. *Element_access* anlamını öğrenmek için aşağıdaki kurallar, *primary_no_array_creation_expression* ve derleme zamanı türü `dynamic`olan *argument_list* ifadelerin derleme zamanı türü yerine çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. *Primary_no_array_creation_expression* derleme zamanı türünde `dynamic`yoksa, öğe erişimi, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.

Bir *element_access* *primary_no_array_creation_expression* bir *array_type*değeri ise, *element_access* bir dizi erişimdir ([dizi erişimi](expressions.md#array-access)). Aksi takdirde *primary_no_array_creation_expression* , bir veya daha fazla Indexer üyesine sahip bir sınıf, yapı veya arabirim türünün bir değişkeni ya da değeri olmalıdır ve bu durumda *element_access* bir Dizin Oluşturucu erişimi ([Dizin Oluşturucu erişimi](expressions.md#indexer-access)) olur.

#### <a name="array-access"></a>Dizi erişimi

Dizi erişimi için *element_access* *primary_no_array_creation_expression* bir *array_type*değeri olmalıdır. Ayrıca, dizi erişiminin *argument_list* adlandırılmış bağımsız değişkenler içermesine izin verilmez. *Argument_list* ifadelerin sayısı, *array_type*derecesine göre aynı olmalıdır ve her ifadenin `int`, `uint`, `long`, `ulong`veya bu türlerden bir veya daha fazla örtülü olarak dönüştürülebilir olması gerekir.

Dizi erişiminin hesaplanmasının sonucu, dizinin öğe türünün bir değişkenidir; Yani, *argument_list*ifade (ler) in değeri tarafından seçilen dizi öğesi.

Form `P[A]`, `P` bir *array_type* *primary_no_array_creation_expression* olduğu ve `A` bir *argument_list*olan bir dizi erişiminin çalışma zamanı işleme, aşağıdaki adımlardan oluşur:

*  `P` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
*  *Argument_list* Dizin ifadeleri soldan sağa sırayla değerlendirilir. Her bir dizin ifadesinin değerlendirmesi aşağıdaki türlerden birine örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir: `int`, `uint`, `long`, `ulong`. Bu listede örtük bir dönüştürmenin bulunduğu ilk tür seçilir. Örneğin, Dizin ifadesi türü `short`, `short` 'den `int` ve `short` 'den `long` 'e örtük dönüştürmeler yapıladıklarından `int` örtük bir dönüştürme işlemi gerçekleştirilir. Bir dizin ifadesi veya sonraki örtük dönüştürme değerlendirmesi bir özel duruma neden oluyorsa, daha fazla dizin ifadesi değerlendirilmez ve başka bir adım yürütülmez.
*  `P` değeri geçerli olacak şekilde işaretlendi. `P` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
*  *Argument_list* her bir ifadenin değeri, `P`tarafından başvurulan dizi örneğinin her boyutunun gerçek sınırlarına göre denetlenir. Bir veya daha fazla değer Aralık dışında ise, bir `System.IndexOutOfRangeException` oluşturulur ve başka bir adım yürütülmez.
*  Dizin ifadeleri tarafından verilen dizi öğesinin konumu hesaplanır ve bu konum dizi erişiminin sonucu olur.

#### <a name="indexer-access"></a>Dizin Oluşturucu erişimi

Dizin Oluşturucu erişimi için, element_access *primary_no_array_creation_expression* bir sınıf , yapı veya arabirim türünde bir değişken ya da değer olmalıdır ve bu tür *element_access* *argument_list* açısından geçerli olan bir veya daha fazla Dizin Oluşturucu uygulamalıdır.

Form `P[A]`Dizin Oluşturucu erişiminin bağlama zamanı işleme, `P` bir sınıf, yapı veya arabirim türü `T`*primary_no_array_creation_expression* ve `A` bir *argument_list*ise aşağıdaki adımlardan oluşur:

*  `T` tarafından belirtilen dizin oluşturucular kümesi oluşturulur. Küme, `T` belirtilen tüm dizin oluşturuculardan veya `override` bildirimleri olmayan ve geçerli bağlamda erişilebilen bir `T` taban türünde ([üye erişim](basic-concepts.md#member-access)) oluşur.
*  Küme, uygulanabilir ve diğer Dizin oluşturucular tarafından gizlenmediği dizin oluşturucularının düşürüldü. Aşağıdaki kurallar, kümesindeki `S.I` her bir dizin oluşturucuya uygulanır; burada `S`, dizin oluşturucunun `I` bildirildiği türdür:
   * `I`, `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerli değilse `I` kümeden kaldırılır.
   * `I`, `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) açısından geçerliyse, `S` temel türünde belirtilen tüm dizin oluşturucular kümeden kaldırılır.
   * `I`, `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) ve `S` `object`dışında bir sınıf türü ise, bir arabirimde bildirildiği tüm dizin oluşturucular kümeden kaldırılır.
*  Elde edilen aday Dizin oluşturucular kümesi boşsa, uygulanabilir Dizin oluşturucular yok ve bir bağlama zamanı hatası oluşur.
*  Aday Dizin oluşturucular kümesinin en iyi Dizin Oluşturucusu, [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak tanımlanır. Tek bir en iyi Dizin Oluşturucu tanımlanamıyorsa, Dizin Oluşturucu erişimi belirsizdir ve bir bağlama zamanı hatası oluşur.
*  *Argument_list* Dizin ifadeleri soldan sağa sırayla değerlendirilir. Dizin Oluşturucu erişimini işlemenin sonucu, Dizin Oluşturucu erişimi olarak sınıflandırılan bir ifadedir. Dizin Oluşturucu erişim ifadesi, yukarıdaki adımda belirlenen dizin oluşturucuya başvurur ve `P` ilişkili bir örnek ifadesi ve `A`ilişkili bağımsız değişken listesi içerir.

Bir Dizin Oluşturucu erişimi, kullanıldığı içeriğe bağlı olarak, dizin oluşturucunun *Get erişimcisinin* veya *set erişimcisinin* çağrısına neden olur. Dizin Oluşturucu erişimi bir atamanın hedefi ise, *küme erişimcisi* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)). Diğer tüm durumlarda, *get erişimcisi* geçerli değeri ([ifadelerin değerleri](expressions.md#values-of-expressions)) almak için çağrılır.

### <a name="this-access"></a>Bu erişim

*This_Access* , ayrılmış sözcükten `this`oluşur.

```antlr
this_access
    : 'this'
    ;
```

Bir *This_Access* yalnızca bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcinin *bloğunda* izin verilir. Aşağıdaki anlamlara sahiptir:

*  Bir sınıfın örnek Oluşturucusu içindeki bir *primary_expression* `this` kullanıldığında, değer olarak sınıflandırıldı. Değerin türü, kullanımın gerçekleştiği sınıfın örnek türü ([örnek türü](classes.md#the-instance-type)) ve değer, oluşturulmakta olan nesneye bir başvurudur.
*  `this`, bir sınıfın örnek metodu veya örnek erişimcisi içindeki bir *primary_expression* kullanıldığında, değer olarak sınıflandırılacaktır. Değerin türü, kullanımının gerçekleştiği sınıfın örnek türü ([örnek türü](classes.md#the-instance-type)) ve değer, yöntemin veya erişimcinin çağrıldığı nesneye bir başvurudur.
*  Bir yapının örnek Oluşturucusu içindeki bir *primary_expression* `this` kullanıldığında, değişken olarak sınıflandırılmaktadır. Değişkenin türü, kullanımın gerçekleştiği yapının örnek türüdür ([örnek türü](classes.md#the-instance-type)) ve değişken oluşturulan yapıyı temsil eder. Bir yapının örnek oluşturucusunun `this` değişkeni, yapı türünün bir `out` parametresiyle tamamen aynı şekilde davranır — özellikle bu, değişkenin örnek oluşturucusunun her yürütme yolunda kesinlikle atanması gerektiği anlamına gelir.
*  `this`, bir yapının örnek yöntemi veya örnek erişimcisi içindeki bir *primary_expression* kullanıldığında, bir değişken olarak sınıflandırılacaktır. Değişkenin türü, kullanımının gerçekleştiği yapının örnek türüdür ([örnek türü](classes.md#the-instance-type)).
   * Yöntem veya erişimci bir yineleyici ([yineleyiciler](classes.md#iterators)) değilse, `this` değişkeni Yöntem veya erişimcinin çağrıldığı yapıyı temsil eder ve yapı türünün bir `ref` parametresiyle tam olarak aynı şekilde davranır.
   * Yöntem veya erişimci bir yineleyici ise, `this` değişkeni yöntemi veya erişimcinin çağrıldığı yapının bir kopyasını temsil eder ve yapı türünün bir değer parametresiyle tam olarak aynı şekilde davranır.

Yukarıda listelenenlerin dışında bir bağlamda *primary_expression* `this` kullanımı, derleme zamanı hatasıdır. Özellikle, bir statik yöntemde `this`, statik bir özellik erişimcisinde veya bir alan bildiriminin bir *variable_initializer* başvurmak mümkün değildir.

### <a name="base-access"></a>Temel erişim

Bir *base_access* , "`.`" belirteci ve bir tanımlayıcı ya da köşeli ayraç içine alınmış bir *argument_list* gelen ayrılmış sözcükten `base` oluşur:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

Geçerli sınıf veya yapıda benzer adlandırılmış Üyeler tarafından gizlenen temel sınıf üyelerine erişmek için bir *base_access* kullanılır. Bir *base_access* yalnızca bir örnek oluşturucusunun, örnek yönteminin veya örnek erişimcinin *bloğunda* izin verilir. `base.I` bir sınıfta veya yapıda olduğunda, `I` bu sınıfın veya yapının temel sınıfının bir üyesini belirtmelidir. Benzer şekilde, bir sınıfta `base[E]` oluştuğunda, temel sınıfta ilgili bir Dizin Oluşturucu bulunmalıdır.

Bağlama zamanında, `base.I` ve `base[E]` `((B)this).I` *base_access* ifadeleri tam olarak yazılmış gibi değerlendirilir; burada `((B)this)[E]`, yapının gerçekleştiği sınıfın veya yapının temel sınıfıdır. Bu nedenle, `base.I` ve `base[E]` `this.I` ve `this[E]`karşılık gelir, bunun dışında `this` temel sınıfın bir örneği olarak görüntülenir.

Bir *base_access* bir sanal işlev üyesine (bir yöntem, özellik veya Dizin Oluşturucu) başvurduğunda, çalışma zamanında hangi işlev üyesinin çalıştırılacağını belirleme ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) değiştirilir. Çağrılan işlev üyesi, işlev üyesinin, `B` (`this`çalışma zamanı türüne göre değil, temel olmayan bir erişimde olduğu gibi) ilgili en çok türetilmiş uygulama ([sanal yöntemler](classes.md#virtual-methods)) ile belirlenir. Bu nedenle, bir `virtual` işlevi üyesinin `override` içinde, işlev üyesinin devralınmış uygulamasını çağırmak için bir *base_access* kullanılabilir. Bir *base_access* tarafından başvurulan işlev üyesi Özet ise, bir bağlama zamanı hatası oluşur.

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

*Primary_expression* , derleme zamanı türüne `dynamic`, işleç dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)), *post_increment_expression* veya *post_decrement_expression* derleme zamanı türü `dynamic` vardır ve aşağıdaki kurallar çalışma zamanında *primary_expression*çalışma zamanı türü kullanılarak uygulanır.

Bir sonek artışı veya azaltma işleminin işleneni bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun hem `get` hem de bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Tekil operatör aşırı yükleme çözümü ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. Önceden tanımlı `++` ve `--` işleçleri şu türler için mevcuttur: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`ve herhangi bir numaralandırma türü. Önceden tanımlanmış `++` işleçleri, işlenene 1 eklenerek üretilen değeri döndürür ve önceden tanımlanmış `--` işleçleri işleneni 1 çıkararak üretilen değeri döndürür. `checked` bağlamında, bu ekleme veya çıkarma sonucu sonuç türü aralığının dışındaysa ve sonuç türü bir tam sayı türü veya Enum türü ise, bir `System.OverflowException` oluşturulur.

`x++` veya `x--` form Sonek artışı veya azaltma işleminin çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*   `x` bir değişken olarak sınıflandırıldıysanız:
    * `x`, değişkeni üretmek için değerlendirilir.
    * `x` değeri kaydedilir.
    * Seçilen işleç, bağımsız değişkeni olarak kaydedilen `x` değeriyle çağrılır.
    * İşleci tarafından döndürülen değer, `x`değerlendirmesi tarafından verilen konumda depolanır.
    * `x` kaydedilen değeri işlemin sonucu olur.
*   `x`, özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldıysanız:
    * Örnek ifadesi (`x` `static`değilse) ve bağımsız değişken listesi (`x` bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `get` ve `set` erişimci etkinleştirmeleri içinde kullanılır.
    * `x` `get` erişimcisi çağrılır ve döndürülen değer kaydedilir.
    * Seçilen işleç, bağımsız değişkeni olarak kaydedilen `x` değeriyle çağrılır.
    * `x` `set` erişimcisi, işleç tarafından `value` bağımsız değişkeni olarak döndürülen değerle çağrılır.
    * `x` kaydedilen değeri işlemin sonucu olur.

`++` ve `--` işleçleri önek gösterimini de destekler ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)). Genellikle, `x++` veya `x--` sonucu işlemden önceki `x` değeridir, ancak `++x` veya `--x` sonucu işlemden sonraki `x` değeridir. Her iki durumda da `x`, işlemden sonra aynı değere sahiptir.

Bir `operator ++` veya `operator --` uygulama, sonek veya ön ek gösterimi kullanılarak çağrılabilir. İki gösterimler için ayrı işleç uygulamalarına sahip olmak mümkün değildir.

### <a name="the-new-operator"></a>New işleci

`new` işleci, türlerin yeni örneklerini oluşturmak için kullanılır.

Üç `new` ifade biçimi vardır:

*  Nesne oluşturma ifadeleri, sınıf türlerinin ve değer türlerinin yeni örneklerini oluşturmak için kullanılır.
*  Dizi oluşturma ifadeleri dizi türlerinin yeni örneklerini oluşturmak için kullanılır.
*  Temsilci oluşturma ifadeleri temsilci türlerinin yeni örneklerini oluşturmak için kullanılır.

`new` işleci bir tür örneğinin oluşturulmasını, ancak dinamik bellek ayırmayı göstermez. Özellikle, değer türlerinin örnekleri, bulundukları değişkenlerin ötesinde ek bellek gerektirmez ve değer türlerinin örneklerini oluşturmak için `new` kullanıldığında dinamik ayırmalar gerçekleşmez.

#### <a name="object-creation-expressions"></a>Nesne oluşturma ifadeleri

Bir *object_creation_expression* *class_type* veya *value_type*yeni bir örneğini oluşturmak için kullanılır.

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

Object_creation_expression *türü* *class_type*, *value_type* veya *type_parameter*olmalıdır. *Tür* bir *class_type*`abstract` olamaz.

İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yalnızca *tür* bir *class_type* veya *struct_type*olduğunda izin verilir.

Nesne oluşturma ifadesi, Oluşturucu bağımsız değişken listesini atlayabilir ve kapsayan parantez, bir nesne Başlatıcısı veya koleksiyon başlatıcısı içerir. Oluşturucu bağımsız değişken listesini atlama ve kapsayan parantezler boş bir bağımsız değişken listesi belirtmeye eşdeğerdir.

Bir nesne Başlatıcısı veya koleksiyon başlatıcısı içeren bir nesne oluşturma ifadesinin işlenmesi, ilk örnek oluşturucuyu işlemeyi ve ardından nesne Başlatıcısı ([nesne başlatıcıları](expressions.md#object-initializers)) veya koleksiyon başlatıcısı ([koleksiyon başlatıcıları](expressions.md#collection-initializers)) tarafından belirtilen üye veya öğe başlatmaları işlemeyi içerir.

İsteğe bağlı *argument_list* bağımsız değişkenlerden herhangi birinin derleme zamanı türü `dynamic` varsa *object_creation_expression* dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kurallar, derleme zamanı türü `dynamic`olan *argument_list* bağımsız değişkenlerinin çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. Ancak, nesne oluşturma, [dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)konusunda açıklandığı gibi sınırlı bir derleme süresi denetimi yapar.

Form `new T(A)`*object_creation_expression* bağlama zamanı işleme; burada `T` *class_type* veya *value_type* , `A` ise isteğe bağlı bir *argument_list*, aşağıdaki adımlardan oluşur:

*   `T` bir *value_type* ve `A` yoksa:
    * *Object_creation_expression* varsayılan Oluşturucu çağrıdır. *Object_creation_expression* sonucu, `T`türünde bir değerdir, yani [System. ValueType türünde](types.md#the-systemvaluetype-type)tanımlanan `T` için varsayılan değerdir.
*   Aksi takdirde, `T` bir *type_parameter* ve `A` yoksa:
    * `T`için hiçbir değer türü kısıtlaması veya Oluşturucu kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) belirtilmemişse, bir bağlama zamanı hatası oluşur.
    * *Object_creation_expression* sonucu, tür parametresinin bağlandığı çalışma zamanı türünün bir değeridir, yani bu türün varsayılan yapıcısını çağırma sonucu. Çalışma zamanı türü bir başvuru türü veya değer türü olabilir.
*   Aksi takdirde, `T` bir *class_type* veya *struct_type*:
    * `T` bir `abstract` *class_type*ise, derleme zamanı hatası oluşur.
    * Çağrılacak örnek Oluşturucu [aşırı yükleme çözümünün](expressions.md#overload-resolution)aşırı yükleme çözümleme kuralları kullanılarak belirlenir. Aday örnek oluşturucular kümesi, `A` ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) açısından geçerli olan `T` tarafından belirtilen tüm erişilebilir örnek oluşturuculardan oluşur. Aday örnek oluşturucular kümesi boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir bağlama zamanı hatası oluşur.
    * *Object_creation_expression* sonucu, yukarıdaki adımda belirlenen örnek oluşturucuyu çağırarak üretilen değer olan `T`türünde bir değerdir.
*  Aksi takdirde, *object_creation_expression* geçersizdir ve bir bağlama zamanı hatası oluşur.

*Object_creation_expression* dinamik olarak bağlı olsa da, derleme zamanı türü hala `T`.

Form `new T(A)`*object_creation_expression* çalışma zamanı işleme; burada `T` *class_type* veya *struct_type* ve `A` isteğe bağlı bir *argument_list*, aşağıdaki adımlardan oluşur:

*   `T` bir *class_type*:
    * `T` sınıfının yeni bir örneği ayrıldı. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
    * Yeni örneğin tüm alanları varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.
    * Örnek Oluşturucu, işlev üye çağırma kurallarına göre çağrılır ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Yeni ayrılmış örneğe bir başvuru, örnek oluşturucusuna otomatik olarak geçirilir ve örneğe `this`olarak bu oluşturucunun içinden erişilebilir.
*   `T` bir *struct_type*:
    * `T` türünde bir örnek, geçici bir yerel değişken ayırarak oluşturulur. Bir *struct_type* örnek Oluşturucusu oluşturulan örneğin her bir alanına kesinlikle değer atamak gerektiğinden, geçici değişkenin başlatılması gerekmez.
    * Örnek Oluşturucu, işlev üye çağırma kurallarına göre çağrılır ([dinamik aşırı yükleme çözümünün derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Yeni ayrılmış örneğe bir başvuru, örnek oluşturucusuna otomatik olarak geçirilir ve örneğe `this`olarak bu oluşturucunun içinden erişilebilir.

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

Bir nesne Başlatıcısı, `{` ve `}` belirteçleri içine alınmış ve virgülle ayrılmış bir dizi üye başlatıcıdan oluşur. Her bir *member_initializer* başlatma için bir hedef belirler. Bir *tanımlayıcı* , başlatılan nesnenin erişilebilir bir alanı veya özelliğini sağlamalıdır, ancak köşeli ayraç içine alınmış bir *argument_list* , başlatılan nesnede erişilebilir bir Dizin Oluşturucu için bağımsız değişkenler belirtmelidir. Bir nesne başlatıcısının aynı alan veya özellik için birden fazla üye başlatıcısı içermesi hatadır.

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

`Point` bir örneği aşağıdaki şekilde oluşturulup başlatılabilir:
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
`__a`, aksi durumda görünmeyen ve erişilemeyen geçici değişken. Aşağıdaki sınıf, iki noktadan oluşturulan bir dikdörtgeni temsil eder:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

`Rectangle` bir örneği aşağıdaki şekilde oluşturulup başlatılabilir:
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
`__r`, `__p1` ve `__p2`, aksi durumda görünmez ve erişilemeyen geçici değişkenlerdir.

`Rectangle`Oluşturucusu iki katıştırılmış `Point` örneği ayırırsa
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Aşağıdaki yapı, yeni örnekleri atamak yerine katıştırılmış `Point` örneklerini başlatmak için kullanılabilir:
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
Burada `__c`, vb., kaynak kodla görünmeyen ve erişilemeyen değişkenler oluşturulur. `[0,0]` bağımsız değişkenlerinin yalnızca bir kez değerlendirileceğini ve `[1,2]` bağımsız değişkenlerinin hiç kullanılmasa bile bir kez değerlendirildiğini unutmayın.

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

Bir koleksiyon başlatıcısı, `{` ve `}` belirteçleri içine alınmış ve virgülle ayrılmış bir dizi öğe başlatıcısından oluşur. Her öğe başlatıcısı, başlatılmakta olan koleksiyon nesnesine eklenecek bir öğe belirtir ve `{` ve `}` belirteçleri ve virgülle ayrılmış bir ifade listesinden oluşur.  Tek ifadeyle bir öğe başlatıcısı küme ayracı olmadan yazılabilir, ancak üye başlatıcılarla belirsizliğe engel olmak için atama ifadesi olamaz. *Non_assignment_expression* üretimi [ifadede](expressions.md#expression)tanımlanmıştır.

Aşağıda, bir koleksiyon başlatıcısı içeren bir nesne oluşturma ifadesine örnek verilmiştir:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Koleksiyon başlatıcısının uygulandığı koleksiyon nesnesi `System.Collections.IEnumerable` uygulayan bir türde veya bir derleme zamanı hatası meydana gelmelidir. Koleksiyon Başlatıcısı, belirtilen her öğe için, her bir çağrı için normal üye arama ve aşırı yükleme çözümlemesi uygulayarak, hedef nesnede, öğe başlatıcısı için bağımsız değişken listesi olarak bir `Add` yöntemi çağırır. Bu nedenle, koleksiyon nesnesinin her öğe başlatıcısı için `Add` ad ile ilgili bir örnek veya genişletme yöntemi olmalıdır.

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

`List<Contact>` aşağıdaki şekilde oluşturulup başlatılabilir:
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
`__clist`, `__c1` ve `__c2`, aksi durumda görünmez ve erişilemeyen geçici değişkenlerdir.

#### <a name="array-creation-expressions"></a>Dizi oluşturma ifadeleri

Bir *array_creation_expression* *array_type*yeni bir örneğini oluşturmak için kullanılır.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

İlk formun dizi oluşturma ifadesi, bağımsız ifadelerin her birini ifade listesinden silmenin sonucu olan türün bir dizi örneğini ayırır. Örneğin, dizi oluşturma ifadesi `new int[10,20]` `int[,]`türünde bir dizi örneği üretir ve dizi oluşturma ifadesi `new int[10][,]` `int[][,]`türünde bir dizi üretir. İfade listesindeki her bir ifade `int`, `uint`, `long`veya `ulong`türünde ya da örtülü olarak bu türlerden bir veya daha fazlasına dönüştürülebilir olmalıdır. Her ifadenin değeri, yeni ayrılan dizi örneğindeki karşılık gelen boyutun uzunluğunu belirler. Bir dizi boyutunun uzunluğunun negatif olması gerektiğinden, ifade listesinde negatif bir değer olan *constant_expression* bir derleme zamanı hatası olur.

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

Üçüncü formun dizi oluşturma ifadesi, ***örtük olarak yazılmış dizi oluşturma ifadesi***olarak adlandırılır. Dizinin öğe türü açıkça verilmemiştir, ancak dizi başlatıcısındaki ifadelerin kümesinin en iyi ortak türü ([bir ifade kümesinin en iyi ortak türünü bulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) olarak belirlenir, ikinci forma benzerdir. Çok boyutlu bir dizi için, yani *rank_specifier* en az bir virgül içerdiğinde, bu küme iç içe *array_initializer*s ' de bulunan tüm *ifade*öğeleri içerir.

Dizi başlatıcıları, [dizi başlatıcılarda](arrays.md#array-initializers)daha ayrıntılı olarak açıklanmıştır.

Dizi oluşturma ifadesinin değerlendirilme sonucu bir değer olarak sınıflandırıldığından, yani yeni ayrılmış dizi örneğine bir başvurudur. Bir dizi oluşturma ifadesinin çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*  *Expression_list* boyut uzunluğu ifadeleri, soldan sağa doğru sırayla değerlendirilir. Her bir ifadenin aşağıdaki türlerden birine yönelik olarak örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) gerçekleştirilir: `int`, `uint`, `long`, `ulong`. Bu listede örtük bir dönüştürmenin bulunduğu ilk tür seçilir. Bir ifade veya sonraki örtük dönüştürme değerlendirmesi bir özel duruma neden oluyorsa, başka ifadeler değerlendirilmez ve başka bir adım yürütülmez.
*  Boyut uzunlukları için hesaplanan değerler aşağıdaki şekilde onaylanır. Bir veya daha fazla değer sıfırdan küçükse bir `System.OverflowException` oluşturulur ve başka adımlar yürütülmez.
*  Verilen boyut uzunluklarına sahip bir dizi örneği ayrıldı. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
*  Yeni dizi örneğinin tüm öğeleri varsayılan değerlerine ([varsayılan değerler](variables.md#default-values)) başlatılır.
*  Dizi oluşturma ifadesi bir dizi başlatıcısı içeriyorsa, dizi başlatıcısındaki her bir ifade değerlendirilir ve karşılık gelen dizi öğesine atanır. Değerlendirmeler ve Atamalar, ifadelerin dizi başlatıcıda yazıldığı sırada gerçekleştirilir — diğer bir deyişle, öğeler, en sağdaki boyut ilk artarak Dizin sırasında başlatılır. Belirli bir ifadenin değerlendirilmesi veya karşılık gelen dizi öğesine sonraki atama bir özel duruma neden oluyorsa, daha fazla öğe başlatılmaz (ve kalan öğelerin varsayılan değerleri olur).

Dizi oluşturma ifadesi bir dizi türünün öğeleriyle bir dizinin örneklemesine izin verir, ancak bu tür bir dizinin öğeleri el ile başlatılmış olmalıdır. Örneğin, ekstresi
```csharp
int[][] a = new int[100][];
```
`int[]`türünde 100 öğe içeren tek boyutlu bir dizi oluşturur. Her öğenin başlangıç değeri `null`. Aynı dizi oluşturma ifadesinin alt dizileri ve deyimi de örneği oluşturması mümkün değildir
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

Son ifade, hiçbir `int` ne de `string` örtülü olarak dönüştürülebilir olduğundan ve en iyi ortak tür olmadığından, derleme zamanı hatasına neden olur. Açıkça yazılmış bir dizi oluşturma ifadesi bu durumda kullanılmalıdır, örneğin `object[]`tür olarak belirtiliyor. Alternatif olarak, öğelerinden biri ortak bir temel türe alınabilir, bu da çıkarılan öğe türü olur.

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

Bir *delegate_creation_expression* *delegate_type*yeni bir örneğini oluşturmak için kullanılır.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Bir temsilci oluşturma ifadesinin bağımsız değişkeni bir yöntem grubu, anonim bir işlev veya `dynamic` ya da *delegate_type*derleme zamanı türünde bir değer olmalıdır. Bağımsız değişken bir yöntem grubu ise, yöntemini ve bir örnek yöntemi için, bir temsilci oluşturulacak nesneyi tanımlar. Bağımsız değişken anonim bir işlevse, temsilci hedefinin parametrelerini ve Yöntem gövdesini doğrudan tanımlar. Bağımsız değişken bir değer ise, bir kopyasını oluşturmak için bir temsilci örneğini tanımlar.

*İfadede* derleme zamanı türü `dynamic`varsa, *delegate_creation_expression* dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kurallar, *ifadenin*çalışma zamanı türü kullanılarak çalışma zamanında uygulanır. Aksi takdirde kurallar derleme zamanında uygulanır.

Form `new D(E)`*delegate_creation_expression* , `D` bir *delegate_type* ve `E` bir *ifade*olan bağlama zamanı işleme, aşağıdaki adımlardan oluşur:

*  `E` bir yöntem grubu ise, temsilci oluşturma ifadesi, bir yöntem grubu dönüştürmesi ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) ile `E` `D`aynı şekilde işlenir.
*  `E` anonim bir işlevse, temsilci oluşturma ifadesi, anonim işlev dönüştürme ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)) ile `E` `D`arasında aynı şekilde işlenir.
*  `E` bir değer ise, `E` uyumlu olmalıdır ([temsilci bildirimleri](delegates.md#delegate-declarations)) `D`ve sonuç, `E`olarak aynı çağırma listesine başvuran `D` yeni oluşturulan bir temsilciye başvurudur. `E` `D`uyumlu değilse, bir derleme zamanı hatası oluşur.

Form `new D(E)`*delegate_creation_expression* çalışma zamanı işleme; burada `D` *delegate_type* ve `E` bir *ifadedir*, aşağıdaki adımlardan oluşur:

*   `E` bir yöntem grubu ise, temsilci oluşturma ifadesi bir yöntem grubu dönüştürmesi ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)) olarak `E` `D`olarak değerlendirilir.
*   `E` anonim bir işlevse, temsilci oluşturma, `E` bir anonim işlev dönüştürmesi olarak değerlendirilir `D` ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)).
*   `E` bir *delegate_type*değeridir:
    * `E` değerlendirilir. Bu değerlendirme bir özel duruma neden olursa başka bir adım yürütülmez.
    * `E` değeri `null`, bir `System.NullReferenceException` oluşturulur ve başka bir adım yürütülmez.
    * `D` temsilci türünün yeni bir örneği ayrılır. Yeni örneği ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülmez.
    * Yeni temsilci örneği, `E`tarafından verilen temsilci örneğiyle aynı çağırma listesiyle başlatılır.

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
Bu yöntem, biçimsel parametre listesi ve `DoubleFunc`dönüş türü ile tam olarak eşleştiğinden, `A.f` alanı ikinci `Square` yöntemine başvuran bir temsilciyle başlatılır. İkinci `Square` yöntemi yoktu, derleme zamanı hatası oluşmuş olabilir.

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

Anonim bir nesne Başlatıcısı, anonim bir tür bildirir ve bu türün bir örneğini döndürür. Anonim tür, doğrudan `object`devralan isimsiz bir sınıf türüdür. Anonim bir türün üyeleri, türünün bir örneğini oluşturmak için kullanılan anonim nesne başlatıcıdan çıkarılan salt okunurdur salt yazılır özelliklerden oluşan bir dizidir. Özellikle, formun anonim nesne Başlatıcısı
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
Her `Tx` karşılık gelen ifade `ex`türüdür. Bir *member_declarator* kullanılan ifadenin türü olmalıdır. Bu nedenle, *member_declarator* bir ifadenin null veya anonim bir işlev olması için derleme zamanı hatası vardır. Ayrıca, ifadenin güvenli olmayan bir türü olması için derleme zamanı hatası da vardır.

Anonim bir türün adı ve `Equals` yöntemi parametresi, derleyici tarafından otomatik olarak oluşturulur ve program metninde başvurulamaz.

Aynı programda aynı ada ve derleme zamanı türlerine sahip bir özellikler dizisini belirten iki anonim nesne başlatıcıları aynı şekilde aynı anonim türde örnekler oluşturacaktır.

Örnekte
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
`p1` ve `p2` aynı anonim türde olduğundan, son satırdaki atamaya izin verilir.

Anonim türlerdeki `Equals` ve `GetHashcode` yöntemleri `object`devralınan yöntemleri geçersiz kılar ve özelliklerin `Equals` ve `GetHashcode` bakımından tanımlanmıştır. böylece, aynı anonim türdeki iki örnek yalnızca tüm özellikleri eşitse eşit olmalıdır.

Bir üye bildirimci basit bir ad ([tür çıkarımı](expressions.md#type-inference)), bir üye erişimi ([dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), temel erişim ([temel erişim](expressions.md#base-access)) veya null koşullu üye erişimi ([projeksiyon başlatıcıları olarak null Koşullu ifadeler](expressions.md#null-conditional-expressions-as-projection-initializers)) için kısaltılabilir. Bu, ***projeksiyon başlatıcısı*** olarak adlandırılır ve aynı ada sahip bir özelliğe bir bildirim ve atama için toplu bir özelliktir. Özellikle, formların üye bildirimcileri
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

`typeof` işleci, bir türün `System.Type` nesnesini almak için kullanılır.

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

*Typeof_expression* ilk formu, bir `typeof` anahtar sözcüğünden ve sonra parantez içine alınmış bir *tür*ile oluşur. Bu formun bir ifadesinin sonucu, belirtilen tür için `System.Type` nesnesidir. Verilen herhangi bir tür için yalnızca bir `System.Type` nesnesi vardır. Bu, bir tür `T`için `typeof(T) == typeof(T)` her zaman doğru olduğu anlamına gelir. *Tür* `dynamic`olamaz.

*Typeof_expression* ikinci biçimi `typeof` bir anahtar sözcükten ve sonra parantez içine alınmış *unbound_type_name*oluşur. Bir *unbound_type_name* bir *type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)) çok benzerdir, çünkü bir *unbound_type_name* *type_name bir* *type_argument_list içeren* *generic_dimension_specifier*s 'yi içerir. Bir *typeof_expression* işleneni hem *unbound_type_name* hem de *type_name*her ikisi de bir *generic_dimension_specifier* veya *type_argument_list*içerdiğinde, belirteçlerin sırası bir *type_name*olarak kabul edilir. Bir *unbound_type_name* anlamı aşağıdaki şekilde belirlenir:

*  Her bir *generic_dimension_specifier* aynı sayı sayısına sahip bir *type_argument_list* değiştirip her bir *type_argument*gibi anahtar sözcüğü `object`, belirteç sırasını *type_name* dönüştürün.
*  Tüm tür parametresi kısıtlamalarını yoksayarak elde edilen *type_name*değerlendirin.
*  *Unbound_type_name* , ortaya çıkan oluşturulan türle ilişkili ilişkisiz genel tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) ile çözümlenir.

*Typeof_expression* sonucu, sonuçta elde edilen ilişkisiz genel tür için `System.Type` nesnesidir.

Üçüncü *typeof_expression* biçimi, bir `typeof` anahtar sözcüğünden ve sonra parantez içine alınmış `void` anahtar kelimesiyle oluşur. Bu formun bir ifadesinin sonucu, bir türün yokluğunu temsil eden `System.Type` nesnesidir. `typeof(void)` tarafından döndürülen tür nesnesi herhangi bir tür için döndürülen tür nesnesinden farklıdır. Bu özel tür nesnesi, dildeki yöntemlere izin veren sınıf kitaplıklarında yararlıdır; burada, bu yöntemlerin bir `System.Type`örneğiyle void yöntemler de dahil olmak üzere herhangi bir yöntemin dönüş türünü temsil etmek için bir yol olmasını ister.

`typeof` işleci bir tür parametresinde kullanılabilir. Sonuç, tür parametresine bağlanan çalışma zamanı türünün `System.Type` nesnesidir. `typeof` işleci, oluşturulmuş bir tür veya ilişkisiz genel tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) üzerinde de kullanılabilir. İlişkisiz genel tür için `System.Type` nesnesi, örnek türünün `System.Type` nesnesiyle aynı değildir. Örnek türü her zaman çalışma zamanında bir kapalı oluşturulmuş türdür, `System.Type` nesnesi kullanımda olan çalışma zamanı türü bağımsız değişkenlerine bağlıdır, ancak ilişkisiz genel türün tür bağımsız değişkenleri yoktur.

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

`int` ve `System.Int32` aynı türde olduğunu unutmayın.

Ayrıca `typeof(X<>)` sonucunun tür bağımsız değişkenine dayanmadığını, ancak `typeof(X<T>)` sonucunu unutmayın.

### <a name="the-checked-and-unchecked-operators"></a>Checked ve unchecked işleçleri

`checked` ve `unchecked` işleçleri, tam sayı türü aritmetik işlemler ve dönüştürmeler için ***taşma denetimi bağlamını*** denetlemek üzere kullanılır.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

`checked` işleci içerilen ifadeyi işaretlenmiş bir bağlamda değerlendirir ve `unchecked` işleci içerilen ifadeyi işaretlenmemiş bir bağlamda değerlendirir. Bir *checked_expression* veya *unchecked_expression* tam olarak bir *parenthesized_expression* ([parantez içine alınmış ifadelerde](expressions.md#parenthesized-expressions)) karşılık gelir, ancak içerilen ifade verilen taşma Denetim bağlamında değerlendirilir.

Taşma Denetim bağlamı, `checked` ve `unchecked` deyimleri ([Checked ve unchecked deyimleri](statements.md#the-checked-and-unchecked-statements)) aracılığıyla da denetlenebilir.

Aşağıdaki işlemler, `checked` ve `unchecked` işleçleri ve deyimleri tarafından oluşturulan taşma denetimi bağlamından etkilenir:

*  Ön tanımlı `++` ve `--` Birli İşleçler ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), işlenen bir integral türüdür.
*  Ön tanımlı `-` birli işleci ([birli eksi işleci](expressions.md#unary-minus-operator)), işlenen bir integral türüdür.
*  Ön tanımlı `+`, `-`, `*`ve `/` ikili işleçler ([Aritmetik işleçler](expressions.md#arithmetic-operators)), her iki işlenen de İntegral türlerdir.
*  Tek bir integral türünden başka bir integral türüne veya `float` ya da `double` bir integral türüne açık sayısal dönüştürmeler ([Açık sayısal dönüştürmeler](conversions.md#explicit-numeric-conversions)).

Yukarıdaki işlemlerden biri hedef türünde temsil etmek için çok büyük bir sonuç üretdüğünde, işlemin gerçekleştirildiği bağlam, sonuçta elde edilen davranışı denetler:

*  `checked` bağlamında, işlem sabit bir ifadesiyse ([sabit ifadeler](expressions.md#constant-expressions)), bir derleme zamanı hatası oluşur. Aksi takdirde, işlem çalışma zamanında gerçekleştirildiğinde bir `System.OverflowException` oluşturulur.
*  `unchecked` bağlamında, sonuç, hedef türüne uymayan yüksek sıralı bitleri atarak atılır.

Herhangi bir `checked` veya `unchecked` işleci ya da deyimler tarafından alınmamış sabit olmayan ifadeler (çalışma zamanında değerlendirilen ifadeler) için, `checked` değerlendirmesi için dış faktörler (derleyici anahtarları ve yürütme ortamı yapılandırması gibi) çağrısı olmadığı sürece varsayılan taşma denetimi bağlamı `unchecked`.

Sabit ifadeler (derleme zamanında tam olarak değerlendirilebilecek ifadeler) için, varsayılan taşma denetimi bağlamı her zaman `checked`. Sabit bir ifade bir `unchecked` bağlamına açıkça yerleştirilmediği için, ifadenin derleme zamanı değerlendirmesi sırasında oluşan taşmalar her zaman derleme zamanı hatalarına neden olur.

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
hiçbir deyimin hiçbiri derleme zamanında değerlendirilemediğinden, derleme zamanı hataları bildirilmemiştir. Çalışma zamanında `F` yöntemi bir `System.OverflowException`oluşturur ve `G` yöntemi-727379968 döndürür (Aralık dışı sonucun alt 32 bitleridir). `H` yönteminin davranışı, derleme için varsayılan taşma denetimi bağlamına bağlıdır, ancak `F` veya `G`ile aynı olur.

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
`F` sabit ifadeler değerlendirilirken meydana gelen taşmalar ve `H` ifadeler bir `checked` bağlamında değerlendirildiğinden derleme zamanı hatalarının raporlanmasına neden olur. `G`sabit ifade değerlendirilirken bir taşma da oluşur, ancak değerlendirme bir `unchecked` bağlamında gerçekleşirken taşma bildirilmedi.

`checked` ve `unchecked` işleçleri yalnızca "`(`" ve "`)`" belirteçlerinde bulunan metin içeriğini eklemek bu işlemler için taşma denetimi bağlamını etkiler. İşleçler, içerilen ifadenin hesaplanmasının sonucu olarak çağrılan işlev üyelerini etkilemez. Örnekte
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
`F` `checked` kullanımı `Multiply``x * y` değerlendirmesini etkilemez, bu nedenle `x * y` varsayılan taşma denetimi bağlamında değerlendirilir.

`unchecked` işleci, işaretli integral türlerinin sabitlerini onaltılık gösterimde yazarken kullanışlıdır. Örneğin:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Yukarıdaki onaltılık sabitlerin her ikisi de `uint`türündedir. Sabitler `int` aralığın dışında olduğundan, `unchecked` işleci olmadan `int` yapılan yayınlar derleme zamanı hataları oluşturur.

`checked` ve `unchecked` işleçleri ve deyimleri, programcıların bazı sayısal hesaplamaların belirli yönlerini denetlemesine olanak tanır. Ancak, bazı sayısal işleçlerin davranışı işlenenlerinin veri türlerine bağlıdır. Örneğin, iki ondalıkın çarpılması her zaman açık bir `unchecked` yapısı içinde bile taşma üzerinde bir özel durumla sonuçlanır. Benzer şekilde, iki kayan noktalar, açıkça bir `checked` yapısı içinde bile taşma durumunda hiçbir şekilde neden olmaz. Ayrıca, diğer işleçler, varsayılan veya açık olsun denetim modundan hiçbir şekilde etkilenmez.

### <a name="default-value-expressions"></a>Varsayılan değer ifadeleri

Varsayılan değer ifadesi bir türün varsayılan değerini ([varsayılan değerler](variables.md#default-values)) almak için kullanılır. Tür parametresi bir değer türü veya bir başvuru türü ise, genellikle varsayılan değer ifadesi tür parametreleri için kullanılır. (Tür parametresi bir başvuru türü olarak bilinmediği takdirde, `null` sabit değerinden bir tür parametresine dönüştürme yok.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Bir default_value_expression *tür* , çalışma zamanında bir başvuru türü olarak değerlendirilirse, sonuç o türe dönüştürülür `null`. Bir default_value_expression *türü* çalışma zamanında bir değer türüne değerlendirilirse, sonuç, *value_type*varsayılan değeridir ([Varsayılan oluşturucular](types.md#default-constructors)).

*Default_value_expression* bir başvuru türü veya bir başvuru türü olarak bilinen bir tür parametresi ise, bir sabit[ifadedir (](classes.md#type-parameter-constraints)[sabit ifadeler](expressions.md#constant-expressions)). Ayrıca, tür şu değer türlerinden biri ise *default_value_expression* sabit bir ifadedir: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`veya herhangi bir numaralandırma türü.


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

Dilbilgisi, *named_entity* işleneni her zaman bir ifadedir. `nameof` ayrılmış bir anahtar sözcük olmadığından, bir NameOf ifadesi, basit ad `nameof`çağrısı ile her zaman sözdizimsel olarak belirsizdir. Uyumluluk nedenleriyle, adın bir ad araması ([basit adlar](expressions.md#simple-names)) `nameof` başarılı olursa, çağrının yasal olup olmamasına bakılmaksızın ifade *invocation_expression* olarak değerlendirilir. Aksi takdirde, bir *nameof_expression*.

Bir *nameof_expression* *named_entity* anlamı, ifadenin anlamı olarak ifade edilir; Yani *simple_name*, bir *base_access* veya *member_access*olarak. Ancak, [basit adlarda](expressions.md#simple-names) ve [üye erişimlerinde](expressions.md#member-access) açıklanan aramanın bir hata ile sonuçlandığı durumlarda, bir örnek üyesi statik bir bağlamda bulunduğu için, *nameof_expression* böyle bir hata üretir.

Bir yöntem grubunu bir *type_argument_list*sahip olacak şekilde belirlemek *named_entity* bir derleme zamanı hatasıdır. Bir *named_entity_target* `dynamic`türüne sahip olması için derleme zamanı hatasıdır.

*Nameof_expression* , `string`türünde sabit bir ifadedir ve çalışma zamanında hiçbir etkiye sahip değildir. Özellikle, *named_entity* değerlendirilmez ve kesin atama analizinin amaçları doğrultusunda yok sayılır ([basit ifadeler için genel kurallar](variables.md#general-rules-for-simple-expressions)). Değeri, isteğe bağlı son *type_argument_list*önce *named_entity* , aşağıdaki şekilde dönüştürülecek olan en son tanıtıcıdır:

* Kullanıldıysa, "`@`" ön eki kaldırılır.
* Her *unicode_escape_sequence* karşılık gelen Unicode karakteriyle dönüştürülür.
* *Formatting_characters* kaldırılır.

Bunlar, tanımlayıcılar arasında eşitlik test edilirken [tanımlayıcılarla](lexical-structure.md#identifiers) uygulanan dönüşümlerdir.

TODO: örnekler

### <a name="anonymous-method-expressions"></a>Anonim yöntem ifadeleri

Bir *anonymous_method_expression* anonim bir işlevi tanımlamanın iki yöntemlerinden biridir. Bunlar, [anonim işlev ifadelerinde](expressions.md#anonymous-function-expressions)daha ayrıntılı olarak açıklanmıştır.

## <a name="unary-operators"></a>Birli İşleçler

`?`, `+`, `-`, `!`, `~`, `++`, `--`, cast ve `await` işleçleri Birli İşleçler olarak adlandırılır.

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

Bir *unary_expression* işleneni derleme zamanı türü `dynamic`sahipse, dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda *unary_expression* derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, işlenenin çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

### <a name="null-conditional-operator"></a>Null-koşullu işleç

Null koşullu işleç, yalnızca bu işlenen null değilse, işlenene bir işlem listesi uygular. Aksi takdirde, işleci uygulamanın sonucu `null`.

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

Örneğin, `a.b?[0]?.c()` ifadesi bir *primary_expression* `a.b` ve *null_conditional_operations* `?[0]` (null-koşullu öğe erişimi), `?.c` (null-koşullu üye erişimi) ve `()` (çağırma) içeren bir *null_conditional_expression* .

*Primary_expression* `P`sahip bir *null_conditional_expression* `E` için, metin içeriğini eklemek tarafından alınan ve bir *`E0` `?` her* birinden önde gelen null_conditional_operations kaldıran ifade olmasına izin ver. Kavramsal olarak, `E0` `?`s tarafından temsil edilen null denetimlerin hiçbiri bir `null`bulmadığı takdirde değerlendirilecek ifadedir.

Ayrıca, metin içeriğini eklemek tarafından elde edilen `E1`, önde gelen `?` `E`' deki *null_conditional_operations* yalnızca ilkini kaldırarak elde edin. Bu, *birincil ifadeye* (yalnızca bir `?`) veya başka bir *null_conditional_expression*yol açabilir.

Örneğin, `E` ifade `a.b?[0]?.c()`, `E0` ifade `a.b[0].c()` ve `E1` ifade `a.b[0]?.c()`.

`E0` hiçbir şey olarak sınıflandırıldıysanız `E` hiçbir şey olarak sınıflandırılacaktır. Aksi halde E bir değer olarak sınıflandırılır.

`E0` ve `E1`, `E`anlamını belirlemede kullanılır:

*  `E` *statement_expression* olarak gerçekleşirse `E` anlamı deyimde aynı olur

   ```csharp
   if ((object)P != null) E1;
   ```

   Bu P yalnızca bir kez değerlendirilir.

*  Aksi takdirde, `E0` hiçbir şey Nothing olarak sınıflandırıldığında, derleme zamanı hatası oluşur.

*  Aksi takdirde, `E0`türü `T0`.

   *  `T0`, başvuru türü veya null yapılamayan bir değer türü olarak bilinen bir tür parametresi ise, derleme zamanı hatası oluşur.

   *  `T0` null yapılamayan bir değer türü ise, `E` türü `T0?`ve `E` anlamı

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      `P` hariç, yalnızca bir kez değerlendirilir.

   *  Aksi takdirde, E türü T0 ve E 'nin anlamı

      ```csharp
      ((object)P == null) ? null : E1
      ```

      `P` hariç, yalnızca bir kez değerlendirilir.

`E1` bir *null_conditional_expression*kendi kendine `null`, daha fazla `?`olmadığı sürece bu kurallar yeniden uygulanır ve ifade, birincil ifade `E0`kadar tüm şekilde azaltılmıştır.

Örneğin, ifade `a.b?[0]?.c()` deyiminde olduğu gibi deyim ifadesi olarak gerçekleşirse:
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
`a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.

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

Null koşullu bir ifadeye, yalnızca bir (isteğe bağlı null koşullu) üye erişimiyle biterse bir *anonymous_object_creation_expression* ([anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions)) *member_declarator* olarak izin verilir. Dilbilgisi, bu gereksinim şöyle ifade edilebilir:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Bu, yukarıdaki *null_conditional_expression* dilbilgisinde özel bir durumdur. [Anonim nesne oluşturma ifadelerinde](expressions.md#anonymous-object-creation-expressions) *member_declarator* için üretim, yalnızca *null_conditional_member_access*içerir.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Deyim ifadeleri olarak null Koşullu ifadeler

Null koşullu ifadeye yalnızca bir çağrı ile bitiyorsa *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) olarak izin verilir. Dilbilgisi, bu gereksinim şöyle ifade edilebilir:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Bu, yukarıdaki *null_conditional_expression* dilbilgisinde özel bir durumdur. [Expression deyimlerdeki](statements.md#expression-statements) *statement_expression* için üretim, yalnızca *null_conditional_invocation_expression*içerir.


### <a name="unary-plus-operator"></a>Tekli artı işleci

`+x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Önceden tanımlanmış birli artı işleçleri şunlardır:

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

`-x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Önceden tanımlanmış olumsuzlama işleçleri şunlardır:

*  Tamsayı değilleme:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Sonuç, sıfırdan `x` çıkararak hesaplanır. `x` değeri, işlenen türünün en küçük gösterilebilir tablo değeridir (`int` için-2 ^ 31 veya `long`için-2 ^ 63), `x` matematik Olumsuzlaştırma, işlenen türü içinde gösterilemeyen bir tablo değildir. Bu bir `checked` bağlamı içinde oluşursa, bir `System.OverflowException` oluşturulur; `unchecked` bağlamı içinde oluşursa, sonuç işlenenin değeridir ve taşma raporlanır.

   Olumsuzlama işlecinin işleneni `uint`türünde ise, `long`türüne dönüştürülür ve sonucun türü `long`olur. Bir özel durum, 2147483648 (-2 ^ 31) değerinin ondalık tamsayı sabit değeri ([tamsayı değişmez](lexical-structure.md#integer-literals)değer) olarak yazılmasına `int` izin veren bir kuraldır.

   Olumsuzlama işlecinin işleneni `ulong`türünde ise, derleme zamanı hatası oluşur. Özel durum,-9223372036854775808 (-2 ^ 63) `long` değerinin ondalık tamsayı sabit değeri ([tamsayı değişmez](lexical-structure.md#integer-literals)değer) olarak yazılmasına izin veren kuraldır.

*  Kayan nokta olumsuzlama:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Sonuç, ters çevrilme ile `x` değeridir. `x` NaN ise, sonuç de NaN olur.

*  Ondalık olumsuzlama:

   ```csharp
   decimal operator -(decimal x);
   ```

   Sonuç, sıfırdan `x` çıkararak hesaplanır. Decimal olumsuzlama, `System.Decimal`türünde birli eksi işlecinin kullanılmasıyla eşdeğerdir.

### <a name="logical-negation-operator"></a>Mantıksal Değilleme İşleci

`!x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Yalnızca bir tane önceden tanımlanmış mantıksal olumsuzlama işleci var:
```csharp
bool operator !(bool x);
```

Bu işleç, işlenenin mantıksal olumsuzunu hesaplar: işlenen `true`, sonuç `false`. İşlenen `false`, sonuç `true`olur.

### <a name="bitwise-complement-operator"></a>Bit düzeyinde tamamlama işleci

`~x`bir işlem için, tekil işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. İşlenen, seçili işlecin parametre türüne dönüştürülür ve sonucun türü işlecin dönüş türüdür. Önceden tanımlanmış bit düzeyinde tamamlama işleçleri şunlardır:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Bu işleçlerin her biri için, işlemin sonucu `x`bit düzeyinde tamamlayıcı bir işlemdir.

Her numaralandırma türü `E` örtük olarak aşağıdaki bit düzeyinde tamamlayıcı işleci sağlar:

```csharp
E operator ~(E x);
```

`~x`değerlendirilmesinin sonucu, `x` bir numaralandırma türünün temel alınan `U`türü `E` bir ifadesiyse, `(E)(~(U)x)`' a dönüştürme her zaman `E` bağlamında ([denetlenen ve işaretlenmemiş operatörler](expressions.md#the-checked-and-unchecked-operators)) gibi gerçekleştirilir.

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

Bir önek artışı veya azaltma işleminin işleneni bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun hem `get` hem de bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Tekil operatör aşırı yükleme çözümü ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli bir operatör uygulamasını seçmek için uygulanır. Önceden tanımlı `++` ve `--` işleçleri şu türler için mevcuttur: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`ve herhangi bir numaralandırma türü. Önceden tanımlanmış `++` işleçleri, işlenene 1 eklenerek üretilen değeri döndürür ve önceden tanımlanmış `--` işleçleri işleneni 1 çıkararak üretilen değeri döndürür. `checked` bağlamında, bu ekleme veya çıkarma sonucu sonuç türü aralığının dışındaysa ve sonuç türü bir tam sayı türü veya Enum türü ise, bir `System.OverflowException` oluşturulur.

`++x` veya `--x` bir ön ekin çalışma zamanı işleme işlemi, aşağıdaki adımlardan oluşur:

*   `x` bir değişken olarak sınıflandırıldıysanız:
    * `x`, değişkeni üretmek için değerlendirilir.
    * Seçili operatör, bağımsız değişkeni olarak `x` değeriyle çağrılır.
    * İşleci tarafından döndürülen değer, `x`değerlendirmesi tarafından verilen konumda depolanır.
    * İşleci tarafından döndürülen değer işlemin sonucu olur.
*   `x`, özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldıysanız:
    * Örnek ifadesi (`x` `static`değilse) ve bağımsız değişken listesi (`x` bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `get` ve `set` erişimci etkinleştirmeleri içinde kullanılır.
    * `x` `get` erişimcisi çağrılır.
    * Seçili operatör, bağımsız değişkeni olarak `get` erişimcisinin döndürdüğü değerle çağrılır.
    * `x` `set` erişimcisi, işleç tarafından `value` bağımsız değişkeni olarak döndürülen değerle çağrılır.
    * İşleci tarafından döndürülen değer işlemin sonucu olur.

`++` ve `--` işleçleri de sonek gösterimini ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) destekler. Genellikle, `x++` veya `x--` sonucu işlemden önceki `x` değeridir, ancak `++x` veya `--x` sonucu işlemden sonraki `x` değeridir. Her iki durumda da `x`, işlemden sonra aynı değere sahiptir.

Bir `operator++` veya `operator--` uygulama, sonek veya ön ek gösterimi kullanılarak çağrılabilir. İki gösterimler için ayrı işleç uygulamalarına sahip olmak mümkün değildir.

### <a name="cast-expressions"></a>Atama ifadeleri

Bir ifadeyi açıkça verilen bir türe dönüştürmek için *cast_expression* kullanılır.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

*Cast_expression* , `T` bir *tür* olduğu ve `E` *unary_expression*, `E` türüne `T`değerine sahip bir açık dönüştürme ([Açık dönüştürmeler](conversions.md#explicit-conversions)) gerçekleştirir `(T)E`form. `E` `T`' den hiçbir açık dönüştürme yoksa, bağlama zamanı hatası oluşur. Aksi takdirde, sonuç açık dönüştürme tarafından üretilen değerdir. Sonuç, `E` bir değişken olduğunu belirtse bile her zaman bir değer olarak sınıflandırılır.

Bir *cast_expression* için dilbilgisi, belirli sözdizimsel belirsizlikleri sağlar. Örneğin, `(x)-y` ifade, bir *cast_expression* (`x`türüne `-y` dönüştürme) veya additive_expression *ile birleştirilmiş bir parenthesized_expression olarak* *(`x - y)`* değerini hesaplayan) olarak yorumlanacaktır.

*Cast_expression* belirsizlikleri çözümlemek için aşağıdaki kural bulunur: parantez içine alınmış bir veya daha fazla *belirteç*[dizisi (boşluk](lexical-structure.md#white-space)), yalnızca aşağıdakilerden en az biri doğru ise, bir *cast_expression* başlangıcını kabul ediyor:

*  Belirteçlerin sırası, bir *tür*için doğru dilbilgisinde bulunur, ancak bir *ifade*için kullanılamaz.
*  Belirteçlerin sırası, bir *tür*için doğru dilbilgisi ve kapatma parantezinden hemen sonraki belirteç, "`~`", belirteç "`!`", belirteç "`(`", *tanımlayıcı* ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)), *değişmez değer* ([sabit değerler](lexical-structure.md#literals)) veya `as` ve `is`dışında herhangi bir *anahtar sözcüğü* ([anahtar](lexical-structure.md#keywords)sözcük).

Yukarıdaki "doğru dilbilgisi" terimi, yalnızca belirteçlerin sırasının belirli dilbilgisi üretimine uyması gerektiği anlamına gelir. Bu, özellikle herhangi bir anayayrılan tanımlayıcıların gerçek anlamını düşünmez. Örneğin, `x` ve `y` tanımlayıcılar ise, `x.y` gerçekten bir tür belirtmese bile `x.y`, bir tür için doğru dilbilgisi olur.

Kesinleştirme kuralından, `x` ve `y` tanımlayıcılar ise, `(x)y`, `(x)(y)`ve `(x)(-y)` *, cast_expression bir*tür tanımlasa bile, `(x)-y` değildir. Ancak, `x` önceden tanımlanmış bir türü tanımlayan bir anahtar sözcüktür (örneğin, `int`), dört *cast_expression*formun tamamı, bu tür bir anahtar sözcük muhtemelen bir ifade olamaz.

### <a name="await-expressions"></a>Await ifadeleri

Await işleci, işlenen tarafından temsil edilen zaman uyumsuz işlem tamamlanana kadar kapsayan zaman uyumsuz işlevin değerlendirmesini askıya almak için kullanılır.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Bir *await_expression* yalnızca zaman uyumsuz bir işlevin gövdesinde ([yineleyiciler](classes.md#iterators)) izin verilir. En yakın kapsayan zaman uyumsuz işlevi içinde, bu yerlerde bir *await_expression* gerçekleşmeyebilir:

*  İç içe geçmiş (zaman uyumsuz) anonim bir işlev içinde
*  *Lock_statement* bloğunun içinde
*  Güvenli olmayan bir bağlamda

Bir *await_expression* , bir *query_expression*içinde çoğu yerde gerçekleşmediğini unutmayın, çünkü bunlar sözdizimsel olarak zaman uyumsuz lambda ifadeleri kullanmak üzere dönüştürüldü.

Zaman uyumsuz bir işlevin içinde `await` tanımlayıcı olarak kullanılamaz. Bu nedenle, await ifadeleri ve tanımlayıcılar içeren çeşitli ifadeler arasında sözdizimsel belirsizlik yoktur. Zaman uyumsuz işlevlerin dışında `await` normal tanımlayıcı işlevi görür.

Bir *await_expression* işleneni ***görev***olarak adlandırılır. *Await_expression* değerlendirilen zaman uyumsuz bir işlemi temsil eder veya tamamlanmayabilir. Await işlecinin amacı, beklenen zaman uyumsuz işlevin yürütülmesini, bekleme görevi tamamlanana kadar askıya almak ve sonra sonucunu elde etmek için kullanılır.

#### <a name="awaitable-expressions"></a>Awasever ifadeleri

Await ifadesinin görevi için ***beklenen***bir ifade olması gerekir. Aşağıdakilerden biri varsa, bir ifade `t` beklenebilir:

*  `t` derleme zamanı türüdür `dynamic`
*  `t`, hiçbir parametre olmadan ve hiçbir tür parametresi olmadan `GetAwaiter` adlı erişilebilir bir örnek veya genişletme yöntemine sahiptir ve bir dönüş türü, aşağıdaki her bir bekletme için `A`.
   * `A` arabirim `System.Runtime.CompilerServices.INotifyCompletion` uygular (bundan sonra breçekimi için `INotifyCompletion` olarak bilinirdi)
   * `A`, `IsCompleted` erişilebilir, okunabilir bir örnek özelliğine sahiptir `bool`
   * `A`, parametresiz ve hiçbir tür parametresi olmayan `GetResult` erişilebilir bir örnek metoduna sahiptir

`GetAwaiter` yönteminin amacı, görev için bir ***awaiter*** elde sağlamaktır. `A` türü await ifadesi için ***awaiter türü*** olarak adlandırılır.

`IsCompleted` özelliğinin amacı görevin zaten tamamlanıp tamamlanmadığını belirlemektir. Bu durumda, değerlendirmeyi askıya almanız gerekmez.

`INotifyCompletion.OnCompleted` yönteminin amacı, göreve bir "devamlılık" kaydı sağlamaktır; Yani, görev tamamlandıktan sonra çağrılacak bir temsilci (`System.Action`türü).

`GetResult` yönteminin amacı, tamamlandıktan sonra görevin sonucunu elde etmek için kullanılır. Bu sonuç, muhtemelen bir sonuç değeriyle birlikte başarılı bir tamamlama veya `GetResult` yöntemi tarafından oluşturulan bir özel durum olabilir.

#### <a name="classification-of-await-expressions"></a>Await ifadelerinin sınıflandırması

`await t` ifade, ifade `(t).GetAwaiter().GetResult()`aynı şekilde sınıflandırıldı. Bu nedenle, `GetResult` dönüş türü `void`, *await_expression* hiçbir şey olarak sınıflandırılacaktır. `T`void olmayan bir dönüş türüne sahipse *await_expression* , `T`türünde bir değer olarak sınıflandırılacaktır.

#### <a name="runtime-evaluation-of-await-expressions"></a>Await ifadelerinin çalışma zamanı değerlendirmesi

Çalışma zamanında ifade `await t` aşağıdaki gibi değerlendirilir:

*  Bir awaiter `a`, ifade `(t).GetAwaiter()`değerlendirilirken elde edilir.
*  Bir `bool` `b`, ifade `(a).IsCompleted`değerlendirilirken elde edilir.
*  `b` `false`, değerlendirme, `a` arabirimi `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (bundan sonra breçekimi için `ICriticalNotifyCompletion` olarak bilinirdi) uygulayıp uygulamadığına bağlıdır. Bu denetim bağlama zamanında yapılır; Yani, `a` derleme zamanı türü `dynamic`varsa ve derleme sırasında çalışma zamanında. `r` sürdürme temsilcisini ([yineleyiciler](classes.md#iterators)) göstermek için:
    * `a` `ICriticalNotifyCompletion`uygulamaz `(a as (INotifyCompletion)).OnCompleted(r)` ifade değerlendirilir.
    * `a` `ICriticalNotifyCompletion`uygulıyorsa `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` ifade değerlendirilir.
    * Değerlendirme daha sonra askıya alınır ve denetim zaman uyumsuz işlevin geçerli çağıranına döndürülür.
*  Hemen sonra (`b` `true`) veya daha sonra sürdürme temsilcisinin çağrılması durumunda (`b` `false`ise), ifade `(a).GetResult()` değerlendirilir. Değer döndürürse, bu değer *await_expression*sonucudur. Aksi takdirde sonuç Nothing olur.

Bir awaiter 'ın `INotifyCompletion.OnCompleted` ve `ICriticalNotifyCompletion.UnsafeOnCompleted` Arabirim yöntemlerinin uygulanması, temsilci `r` en çok bir kez çağrılabilir. Aksi takdirde, kapsayan zaman uyumsuz işlevinin davranışı tanımsız olur.

## <a name="arithmetic-operators"></a>Aritmetik İşleçler

`*`, `/`, `%`, `+`ve `-` işleçleri aritmetik işleçler olarak adlandırılır.

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

Aritmetik işlecin bir işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

### <a name="multiplication-operator"></a>Çarpma işleci

`x * y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış çarpma işleçleri aşağıda listelenmiştir. İşleçler `x` ve `y`çarpımını hesaplar.

*  Tamsayı çarpma:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   `checked` bağlamında, ürün sonuç türü aralığının dışındaysa, bir `System.OverflowException` oluşturulur. `unchecked` bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.


*  Kayan nokta çarpma:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Ürün, IEEE 754 aritmetik kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` pozitif sonlu değerlerdir. `z`, `x * y`sonucudur. Sonuç hedef türü için çok büyükse, `z` sonsuzluk olur. Sonuç, hedef türü için çok küçükse, `z` sıfırdır.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | \+ y   | -y   | +0  | -0  | \+ INF | -INF | {1&gt;NaN&lt;1} | 
   | {1&gt;+&lt;1}x   | \+ z   | -z   | +0  | -0  | \+ INF | -INF | {1&gt;NaN&lt;1} | 
   | {1&gt;-&lt;1}x   | -z   | \+ z   | -0  | +0  | -INF | \+ INF | {1&gt;NaN&lt;1} | 
   | +0   | +0   | -0   | +0  | -0  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | -0   | -0   | +0   | -0  | +0  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | \+ INF | \+ INF | -INF | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | \+ INF | -INF | {1&gt;NaN&lt;1} | 
   | -INF | -INF | \+ INF | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | -INF | \+ INF | {1&gt;NaN&lt;1} | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1} | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 

*  Ondalık çarpma:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonuç değeri `decimal` biçimde temsil etmek için çok küçük ise, sonuç sıfırdır. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklerinin toplamıdır.

   Ondalık çarpma, `System.Decimal`türünde çarpma işlecinin kullanılmasıyla eşdeğerdir.


### <a name="division-operator"></a>Bölme işleci

`x / y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış bölüm işleçleri aşağıda listelenmiştir. İşleçler `x` ve `y`bölümünü hesaplar.

*  Tamsayı bölme:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Sağ işlenenin değeri sıfırsa, bir `System.DivideByZeroException` oluşturulur.

   Bölme sonucu sıfıra doğru yuvarlar. Bu nedenle, sonucun mutlak değeri, iki işlenenin alanının mutlak değerinden küçük veya ona eşit olan en büyük olası tamsayıdır. İki işlenen iki işlenen de ters işaret sahibi olduğunda, sonuç sıfır veya pozitif, sıfır veya negatif olur.

   Sol işlenen en küçük gösterilemeyen `int` veya `long` değeri ve sağ işlenen `-1`ise, bir taşma oluşur. `checked` bağlamında, bu, bir `System.ArithmeticException` (veya alt sınıfın) oluşturulmasına neden olur. `unchecked` bağlamında, uygulama tanımlı bir `System.ArithmeticException` (veya alt sınıfın) oluşturulup oluşturulmayacağını ya da taşma değeri sol işlenenin elde edilen değerle bildirilmeyen bir şekilde geçer.

*  Kayan nokta bölmesi:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Bu bölüm, IEEE 754 aritmetiğinin kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` pozitif sonlu değerlerdir. `z`, `x / y`sonucudur. Sonuç hedef türü için çok büyükse, `z` sonsuzluk olur. Sonuç, hedef türü için çok küçükse, `z` sıfırdır.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ INF | -INF | {1&gt;NaN&lt;1}  | 
   | {1&gt;+&lt;1}x   | \+ z   | -z   | \+ INF | -INF | +0   | -0   | {1&gt;NaN&lt;1}  | 
   | {1&gt;-&lt;1}x   | -z   | \+ z   | -INF | \+ INF | -0   | +0   | {1&gt;NaN&lt;1}  | 
   | +0   | +0   | -0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | +0   | -0   | {1&gt;NaN&lt;1}  | 
   | -0   | -0   | +0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -0   | +0   | {1&gt;NaN&lt;1}  | 
   | \+ INF | \+ INF | -INF | \+ INF | -INF | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -INF | -INF | \+ INF | -INF | \+ INF | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Ondalık bölme:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Sağ işlenenin değeri sıfırsa, bir `System.DivideByZeroException` oluşturulur. Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonuç değeri `decimal` biçimde temsil etmek için çok küçük ise, sonuç sıfırdır. Sonucun ölçeği, en yakın gösterilebilir tablo ondalık değerine eşit olan bir sonucu doğru matematik sonucuyla koruyacak en küçük ölçeğe neden olur.

   Ondalık bölme `System.Decimal`türünde Bölme işlecinin kullanılmasıyla eşdeğerdir.


### <a name="remainder-operator"></a>Kalan işleç

`x % y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış geri kalan işleçler aşağıda listelenmiştir. İşleçler, `x` ve `y`arasında bölmenin geri kalanını hesaplar.

*  Tamsayı geri kalanı:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   `x % y` sonucu, `x - (x / y) * y`tarafından üretilen değerdir. `y` sıfırsa, bir `System.DivideByZeroException` oluşturulur.

   Sol işlenen en küçük `int` veya `long` değeri ise ve sağ işlenen `-1`, bir `System.OverflowException` atılır. Hiçbir durumda `x % y` bir özel durum oluşturmayan bir özel `x / y` durum oluşturur.

*  Kayan nokta kalanı:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` pozitif sonlu değerlerdir. `z`, `x % y` sonucudur ve `x - n * y`olarak hesaplanır; burada `n`, `x / y`küçük veya eşit olan en büyük tamsayıdır. Bu geri kalanı hesaplama yöntemi, tamsayı işlenenleri için kullanılan ile benzerdir, ancak IEEE 754 tanımından (`n` `x / y`en yakın tamsayıdır) farklıdır.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | \+ y   | -y   | +0   | -0   | \+ INF | -INF | {1&gt;NaN&lt;1}  | 
   | {1&gt;+&lt;1}x   | \+ z   | \+ z   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | x    | x    | {1&gt;NaN&lt;1}  | 
   | {1&gt;-&lt;1}x   | -z   | -z   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;-&lt;1}x   | {1&gt;-&lt;1}x   | {1&gt;NaN&lt;1}  | 
   | +0   | +0   | +0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | +0   | +0   | {1&gt;NaN&lt;1}  | 
   | -0   | -0   | -0   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | -0   | -0   | {1&gt;NaN&lt;1}  | 
   | \+ INF | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -INF | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Ondalık kalan:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Sağ işlenenin değeri sıfırsa, bir `System.DivideByZeroException` oluşturulur. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklerinin daha büyük olması ve sıfır olmayan bir değer varsa, `x`işaretiyle aynıdır.

   Ondalık geri kalanı `System.Decimal`türündeki kalan işlecin kullanılmasıyla eşdeğerdir.


### <a name="addition-operator"></a>Toplama işleci

`x + y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış ekleme işleçleri aşağıda listelenmiştir. Sayısal ve sabit listesi türlerinde, önceden tanımlanmış ekleme işleçleri iki işlenenin toplamını hesaplar. Bir veya her iki işlenen de dize türünde olduğunda, önceden tanımlanmış toplama işleçleri işlenen dize gösterimini birleştirir.

*  Tamsayı ekleme:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   `checked` bağlamında, Toplam Sonuç türü aralığının dışındaysa, bir `System.OverflowException` oluşturulur. `unchecked` bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.

*  Kayan nokta ekleme:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Toplam, IEEE 754 aritmetiğinin kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaN 'nin tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` sıfır dışında sonlu değerlerdir ve `z` `x + y`sonucudur. `x` ve `y` aynı büyüklük, ancak ters işaretlere sahipse, `z` pozitif sıfırdır. `x + y` hedef türünde temsil etmek için çok büyükse, `z`, `x + y`aynı işareti içeren bir sonsuzluk olur.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | {1&gt;y&lt;1}    | +0   | -0   | \+ INF | -INF | {1&gt;NaN&lt;1}  | 
   | x    | z    | x    | x    | \+ INF | -INF | {1&gt;NaN&lt;1}  | 
   | +0   | {1&gt;y&lt;1}    | +0   | +0   | \+ INF | -INF | {1&gt;NaN&lt;1}  | 
   | -0   | {1&gt;y&lt;1}    | +0   | -0   | \+ INF | -INF | {1&gt;NaN&lt;1}  | 
   | \+ INF | \+ INF | \+ INF | \+ INF | \+ INF | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 
   | -INF | -INF | -INF | -INF | {1&gt;NaN&lt;1}  | -INF | {1&gt;NaN&lt;1}  | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | 

*  Ondalık ekleme:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklendirilmesine göre daha büyüktür.

   Ondalık ekleme, `System.Decimal`türünde toplama işlecinin kullanılmasıyla eşdeğerdir.

*  Numaralandırma ekleme. Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı işleçleri sağlar; burada `E` sabit listesi türüdür ve `U` `E`temel türüdür:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   Çalışma zamanında bu işleçler tam olarak `(E)((U)x + (U)y)`olarak değerlendirilir.

*  Dize birleştirme:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   İkili `+` işlecinin bu aşırı yüklemeleri dize birleştirmesi gerçekleştirir. Dize birleştirme işleneni `null`ise boş bir dize değiştirilir. Aksi takdirde, dize olmayan herhangi bir bağımsız değişken `object`türünden devralınan sanal `ToString` yöntemi çağrılarak dize gösterimine dönüştürülür. `ToString` `null`döndürürse boş bir dize değiştirilir.

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

   Dize birleştirme işlecinin sonucu, sol işlenenin karakterlerinden ve ardından sağ işlenenin karakterlerinden oluşan bir dizedir. Dize birleştirme işleci hiçbir şekilde `null` değer döndürmez. Elde edilen dizeyi ayırmak için yeterli kullanılabilir bellek yoksa bir `System.OutOfMemoryException` oluşturulabilir.

*  Temsilci birleşimi. Her temsilci türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `D` temsilci türüdür:

   ```csharp
   D operator +(D x, D y);
   ```

   İkili `+` işleci, her iki işlenen de `D`bazı temsilci türleri olduğunda temsilci bileşimini gerçekleştirir. (İşlenenler farklı temsilci türlerine sahip ise, bir bağlama zamanı hatası oluşur.) İlk işlenen `null`, işlemin sonucu ikinci işlenenin değeridir (aynı zamanda `null`olsa bile). Aksi takdirde, ikinci işlenen `null`, işlemin sonucu ilk işlenenin değeridir. Aksi takdirde, işlem sonucu çağrıldığında, ilk işleneni çağırır ve sonra ikinci işleneni çağıran yeni bir temsilci örneğidir. Temsilci birleşimi örnekleri için bkz. [çıkarma işleci](expressions.md#subtraction-operator) ve [temsilci çağrısı](delegates.md#delegate-invocation). `System.Delegate` bir temsilci türü olmadığından, `operator` `+` tanımlı değil.

### <a name="subtraction-operator"></a>Çıkarma işleci

`x - y`bir işlem için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış çıkarma işleçleri aşağıda listelenmiştir. İşleçler `x``y` tüm çıkar.

*  Tamsayı çıkarma:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   `checked` bağlamında, fark sonuç türü aralığının dışındaysa, bir `System.OverflowException` oluşturulur. `unchecked` bağlamında, taşmalar raporlanmaz ve sonuç türü aralığı dışındaki önemli yüksek sıralı bitler atılır.

*  Kayan nokta çıkarma:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Aradaki fark, IEEE 754 aritmetiğinin kurallarına göre hesaplanır. Aşağıdaki tabloda, sıfır dışında sınırlı değer, sıfır, sonsuz ve NaNs 'ın tüm olası birleşimlerinin sonuçları listelenmiştir. Tabloda, `x` ve `y` sıfır dışında sonlu değerlerdir ve `z` `x - y`sonucudur. `x` ve `y` eşitse, `z` pozitif sıfırdır. `x - y` hedef türünde temsil etmek için çok büyükse, `z`, `x - y`aynı işareti içeren bir sonsuzluk olur.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | {1&gt;y&lt;1}    | +0   | -0   | \+ INF | -INF | {1&gt;NaN&lt;1} | 
   | x    | z    | x    | x    | -INF | \+ INF | {1&gt;NaN&lt;1} | 
   | +0   | -y   | +0   | +0   | -INF | \+ INF | {1&gt;NaN&lt;1} | 
   | -0   | -y   | -0   | +0   | -INF | \+ INF | {1&gt;NaN&lt;1} | 
   | \+ INF | \+ INF | \+ INF | \+ INF | {1&gt;NaN&lt;1}  | \+ INF | {1&gt;NaN&lt;1} | 
   | -INF | -INF | -INF | -INF | -INF | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 
   | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1}  | {1&gt;NaN&lt;1} | 

*  Ondalık çıkarma:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Elde edilen değer `decimal` biçimde temsil etmek için çok büyükse, bir `System.OverflowException` oluşturulur. Sonucun ölçeği, herhangi bir yuvarlamadan önce, iki işlenenin ölçeklendirilmesine göre daha büyüktür.

   Decimal çıkarma, `System.Decimal`türünde çıkarma işlecinin kullanılmasıyla eşdeğerdir.

*  Sabit Listesi çıkarma. Her numaralandırma türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `E` sabit listesi türüdür ve `U` `E`temel türüdür:

   ```csharp
   U operator -(E x, E y);
   ```

   Bu işleç tam olarak `(U)((U)x - (U)y)`olarak değerlendirilir. Diğer bir deyişle işleci, `x` ve `y`Ordinal değerleri arasındaki farkı hesaplar ve sonucun türü, numaralandırmanın temel alınan türüdür.

   ```csharp
   E operator -(E x, U y);
   ```

   Bu işleç tam olarak `(E)((U)x - y)`olarak değerlendirilir. Diğer bir deyişle işleci, numaralandırmanın temel alınan türünden bir değeri çıkartır ve sabit listesinin bir değerini çıkarır.

*  Temsilci kaldırma. Her temsilci türü örtük olarak aşağıdaki önceden tanımlı işleci sağlar; burada `D` temsilci türüdür:

   ```csharp
   D operator -(D x, D y);
   ```

   İkili `-` işleci her iki işlenen de `D`bazı temsilci türleri olduğunda temsilci kaldırma işlemini gerçekleştirir. İşlenenler farklı temsilci türlerine sahip ise, bir bağlama zamanı hatası oluşur. İlk işlenen `null`, işlemin sonucu `null`. Aksi takdirde, ikinci işlenen `null`, işlemin sonucu ilk işlenenin değeridir. Aksi halde, her iki işlenen de bir veya daha fazla girişe sahip çağrı listelerini ([temsilci bildirimleri](delegates.md#delegate-declarations)) temsil eder ve sonuç, ikinci işlenenin listesinin ilk olarak doğru bir bitişik alt listesi olması şartıyla, ikinci işlenenin listesini içeren ilk işlenenin listesinden oluşan yeni bir çağırma listesidir.     (Alt liste eşitliğini belirleyebilmek için, karşılık gelen girişler, temsilci eşitlik işleci ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)) ile karşılaştırılır.) Aksi halde, sonuç sol işlenenin değeridir. İşlemde işlenen listelerden hiçbiri değiştirilmez. İkinci işlenenin listesi, ilk işlenen listesindeki bitişik girdilerin birden çok alt listesiyle eşleşiyorsa, bitişik girdilerin en sağ eşleşen alt listesi kaldırılır. Kaldırma işlemi boş bir liste ile sonuçlanırsa sonuç `null`. Örneğin:

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

`<<` ve `>>` işleçleri bit kaydırma işlemlerini gerçekleştirmek için kullanılır.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

Bir *shift_expression* işleneni derleme zamanı türü `dynamic`içeriyorsa, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

`x << count` veya `x >> count`form işlemi için, belirli bir operatör uygulamasını seçmek üzere ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Aşırı yüklenmiş bir kaydırma işleci bildirirken, İlk işlenenin türü her zaman operatör bildirimini içeren sınıf veya yapı olmalıdır ve ikinci işlenenin türü her zaman `int`olmalıdır.

Önceden tanımlanmış kaydırma işleçleri aşağıda listelenmiştir.

*  Sola kaydır:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   `<<` işleci, aşağıda açıklandığı gibi hesaplanan sayıda bit `x` sola kayar.

   `x` sonuç türü aralığının dışındaki yüksek sıralı bitler atılır, kalan bitler sola kaydıralınır ve düşük sıralı boş bit konumları sıfır olarak ayarlanır.

*  Sağa kaydır:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   `>>` işleci, aşağıda açıklandığı gibi hesaplanmış bir bit sayısına göre `x` kayar.

   `x` `int` veya `long`türünde olduğunda, `x` düşük sıralı bitleri atılır, kalan bitler sağa kaydıralınır ve `x` negatifse yüksek sıralı boş bit konumları sıfır olarak ayarlanır ve `x` negatifse, bir olarak ayarlanır.

   `x` `uint` veya `ulong`türünde olduğunda, `x` düşük sıralı bitleri atılır, kalan bitler sağa kaydıralınır ve yüksek sıralı boş bit konumları sıfır olarak ayarlanır.

Önceden tanımlanmış işleçler için, kaydırma yapılacak bit sayısı şu şekilde hesaplanır:

*  `x` türü `int` veya `uint`olduğunda, kaydırma sayısı düşük sıralı beş bit `count`tarafından verilir. Diğer bir deyişle, kaydırma sayısı `count & 0x1F`hesaplanır.
*  `x` türü `long` veya `ulong`olduğunda, kaydırma sayısı en düşük altı bit `count`tarafından verilir. Diğer bir deyişle, kaydırma sayısı `count & 0x3F`hesaplanır.

Sonuç kaydırma sayısı sıfırsa, kaydırma işleçleri yalnızca `x`değerini döndürür.

SHIFT işlemleri hiçbir şekilde taşmaya neden olmaz ve `checked` ve `unchecked` bağlamlarında aynı sonuçları üretir.

`>>` işlecinin sol işleneni işaretli bir integral türünde olduğunda, işleç, işlenenin en önemli bit (işaret biti) değerinde bir aritmetik kaydırma gerçekleştirir ve yüksek sıralı boş bit konumlarına yayılır. `>>` işlecinin sol işleneni işaretsiz bir integral türünde olduğunda, işleç yüksek sıralı boş bit konumlarda her zaman sıfır olarak ayarlanmış bir mantıksal kaydırma gerçekleştirir. İşlenen türünden çıkarılan ' nin ters işlemini gerçekleştirmek için, açık atamalar kullanılabilir. Örneğin, `x` `int`türünde bir değişkense, işlem `unchecked((int)((uint)x >> y))` `x`sağ bir mantıksal kaydırma gerçekleştirir.

## <a name="relational-and-type-testing-operators"></a>İlişkisel ve tür testi işleçleri

`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` ve `as` işleçleri ilişkisel ve tür testi işleçleri olarak adlandırılır.

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

`is` işleci [,,,](expressions.md#the-is-operator) ve `as` işleci [as işleci](expressions.md#the-as-operator)içinde açıklanmıştır.

`==`, `!=`, `<`, `>`, `<=` ve `>=` işleçleri ***karşılaştırma işleçleridir***.

Karşılaştırma işlecinin bir işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

`x` *op* `y`biçiminde bir işlem için, *op* bir karşılaştırma operatörü olduğunda, belirli bir operatör uygulamasını seçmek için aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

Önceden tanımlanmış karşılaştırma işleçleri aşağıdaki bölümlerde açıklanmıştır. Önceden tanımlanmış tüm karşılaştırma işleçleri, aşağıdaki tabloda açıklandığı gibi `bool`türünde bir sonuç döndürür.


| __İşlem__ | __Sonuç__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `x` `y`eşitse `true` `false` Aksi takdirde                 | 
| `x != y`      | `x` `y`eşitse `true` `false` Aksi takdirde             | 
| `x < y`       | `x` `y`'den küçükse `true` `false` Aksi takdirde                | 
| `x > y`       | `x` `y`daha büyükse `true` `false` Aksi takdirde             | 
| `x <= y`      | `x` `y`daha az veya eşit ise `true` `false` Aksi halde    | 
| `x >= y`      | `x` `y`büyük veya buna eşitse `true` `false` Aksi takdirde | 

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

Bu işleçlerin her biri, iki tamsayı işleneninin sayısal değerlerini karşılaştırır ve belirli bir ilişkinin `true` veya `false`olduğunu belirten `bool` bir değer döndürür.

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

*  Her iki işlenen de NaN ise sonuç, `!=`hariç tüm işleçler için `false`, sonuç olarak `true`. Her iki işlenen için `x != y` her zaman `!(x == y)`ile aynı sonucu üretir. Ancak, bir veya her iki işlenen de NaN olduğunda, `<`, `>`, `<=`ve `>=` işleçleri ters işlecin mantıksal değille aynı sonuçları oluşturmaz. Örneğin, `x` ve `y` ikisi NaN ise, `x < y` `false`, ancak `!(x >= y)` `true`.
*  Hiçbir işlenen NaN olmadığında, işleçler iki kayan nokta işleneninin değerlerini sıralamaya göre karşılaştırır

   ```csharp
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   Burada `min` ve `max`, verilen kayan nokta biçiminde temsil edilebilecek en küçük ve en büyük pozitif sınırlı değerlerdir. Bu sıralamanın önemli etkileri şunlardır:
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

Bu işleçlerin her biri, iki ondalık işlenenin sayısal değerlerini karşılaştırır ve belirli bir ilişkinin `true` veya `false`olduğunu belirten `bool` bir değer döndürür. Her ondalık karşılaştırma, `System.Decimal`türünde karşılık gelen ilişkisel veya eşitlik işlecinin kullanılmasıyla eşdeğerdir.

### <a name="boolean-equality-operators"></a>Boole eşitlik işleçleri

Önceden tanımlanmış Boole eşitlik işleçleri şunlardır:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

`==` sonucu, hem `x` hem de `y` `true` ve `x` ve `y` `false`ise `true`. Aksi takdirde, sonuç `false`.

`!=` sonucu, hem `x` hem de `y` `true` ve `x` ve `y` `false`ise `false`. Aksi takdirde, sonuç `true`. İşlenenler `bool`türünde olduğunda `!=` işleci `^` işleci ile aynı sonucu üretir.

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

`x` ve `y`, temel alınan bir tür `U`olan `E` bir numaralandırma türünün ifadeleridir ve `op` karşılaştırma işleçlerinden biri olan `x op y`değerlendirilme sonucu, `((U)x) op ((U)y)`değerlendirmesiyle tamamen aynıdır. Diğer bir deyişle, numaralandırma türü karşılaştırma işleçleri yalnızca iki işlenenin temel alınan integral değerlerini karşılaştırmaktır.

### <a name="reference-type-equality-operators"></a>Başvuru türü eşitlik işleçleri

Önceden tanımlanmış başvuru türü eşitlik işleçleri şunlardır:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

İşleçler, eşitlik veya eşitlik için iki başvuruyu karşılaştırmanın sonucunu döndürür.

Önceden tanımlanmış başvuru türü eşitlik işleçleri `object`türündeki işlenenleri kabul ettiğinde, uygulanabilir `operator ==` ve `operator !=` üyelerini bildirmez tüm türlere uygulanır. Buna karşılık, geçerli kullanıcı tanımlı eşitlik işleçleri, önceden tanımlanmış başvuru türü eşitlik işleçlerini etkin bir şekilde gizler.

Önceden tanımlanmış başvuru türü eşitlik işleçleri aşağıdakilerden birini gerektirir:

*  Her iki işlenen de *reference_type* veya değişmez değer `null`bilinen bir türün değeridir. Ayrıca, bir açık başvuru dönüştürmesi ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)), herhangi bir işlenenin türünden diğer işlenenin türüne de sahiptir.
*  Bir işlenen, `T` bir *type_parameter* olan ve diğer işlenen ise `null`olan `T` türünde bir değerdir. Ayrıca `T` değer türü kısıtlamasına sahip değildir.

Bu koşullardan biri doğru değilse, bir bağlama zamanı hatası oluşur. Bu kuralların önemli etkileri şunlardır:

*  Bu, bağlama zamanında farklı oldukları bilinen iki başvuruyu karşılaştırmak için önceden tanımlanmış başvuru türü eşitlik işleçlerini kullanmanın bir bağlama zamanı hatasıdır. Örneğin, işlenenlerin bağlama zamanı türleri `A` ve `B`iki sınıf türütürlerdi ve hiçbiri ne `A` ne de `B` türetilmediği takdirde, iki işlenen de aynı nesneye başvuruda bulunmak olanaksızdır. Bu nedenle, işlem bağlama zamanı hatası olarak değerlendirilir.
*  Önceden tanımlanmış başvuru türü eşitlik işleçleri değer türü işlenenlerinin karşılaştırılabilmesi için izin vermez. Bu nedenle, bir struct türü kendi eşitlik işleçlerini bildirmediği için, bu yapı türünün değerlerini karşılaştırmak mümkün değildir.
*  Önceden tanımlanmış başvuru türü eşitlik işleçleri, işlenenleri için paketleme işlemlerine neden olmaz. Yeni ayrılmış paketlenmiş örneklere yapılan başvuruların diğer tüm başvurulardan farklı olması gerektiğinden, bu tür kutulanabilir işlemleri gerçekleştirmek anlamlı değildir.
*  Tür parametre `T` türü işleneni `null`ile karşılaştırıldıysanız ve `T` çalışma zamanı türü bir değer türündeyse, karşılaştırma sonucu `false`olur.

Aşağıdaki örnek, kısıtlanmamış bir tür parametre türünün bağımsız değişkeninin `null`olup olmadığını denetler.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

`T` bir değer türünü temsil etse bile `x == null` yapısına izin verilir ve sonuç yalnızca `T` bir değer türü olduğunda `false` olarak tanımlanmıştır.

`x == y` veya `x != y`form işlemi için, uygulanabilir `operator ==` veya `operator !=` varsa, işleç aşırı yükleme çözümü ([ikili işleç aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) kuralları önceden tanımlanmış başvuru türü eşitlik işleci yerine bu işleci seçer. Ancak, bir veya her ikisini de `object`türüne açıkça atayarak önceden tanımlanmış başvuru türü eşitlik işlecini seçmek her zaman mümkündür. Örnek
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

`s` ve `t` değişkenleri aynı karakterleri içeren iki ayrı `string` örneğine başvurur. Her iki işlenen de `string`tür olduğunda önceden tanımlanmış dize eşitlik işleci ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) seçildiğinden ilk karşılaştırma çıkışı `True`. Kalan karşılaştırmalar, bir veya her ikisi de `object`tür olduğunda önceden tanımlanmış başvuru türü eşitlik işleci seçildiğinden, tüm çıkış `False`.

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
yayınlar, kutulanmış `int` değerlerinin iki ayrı örneğine başvuru oluşturduğundan `False` çıktılar.

### <a name="string-equality-operators"></a>Dize eşitlik işleçleri

Önceden tanımlanmış dize eşitlik işleçleri şunlardır:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

Aşağıdakilerden biri doğru olduğunda iki `string` değeri eşit kabul edilir:

*  Her iki değer de `null`.
*  Her iki değer de aynı uzunlukta ve her bir karakter konumunda özdeş karakterlere sahip olan dize örneklerine yönelik null olmayan başvurulardır.

Dize eşitlik işleçleri dize başvuruları yerine dize değerlerini karşılaştırır. İki ayrı dize örneği tam olarak aynı karakter dizisini içerdiğinde, dizelerin değerleri eşittir, ancak başvurular farklıdır. [Başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators)bölümünde açıklandığı gibi, dize değerleri yerine dize başvurularını karşılaştırmak için başvuru türü eşitlik işleçleri kullanılabilir.

### <a name="delegate-equality-operators"></a>Eşitlik işleçlerini devretmek

Her temsilci türü örtük olarak aşağıdaki önceden tanımlı karşılaştırma işleçlerini sağlar:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

İki temsilci örneği aşağıdaki gibi kabul edilir:

*  Temsilci örneklerinden biri `null`ise, yalnızca ikisi de `null`varsa eşittir.
*  Temsilcilerin farklı çalışma zamanı türleri varsa, bunlar hiçbir zaman eşit değildir.
*  Her iki temsilci örneği de bir çağırma listesine ([temsilci bildirimleri](delegates.md#delegate-declarations)) sahip ise, bu örnekler eşit olur ve yalnızca çağırma listeleri aynı uzunluktadır ve tek bir çağrı listesindeki her giriş, karşılık gelen giriş listesindeki diğer çağrı listesinde sırasıyla eşittir (aşağıda tanımlandığı gibi).

Aşağıdaki kurallar, çağırma listesi girişlerinin eşitlik düzeyini yönetir:

*  İki çağırma listesi girişi her ikisi de aynı statik yönteme başvurursanız, girdiler eşittir.
*  İki çağırma listesi girişi her ikisi de aynı hedef nesnede statik olmayan bir yönteme (başvuru eşitlik işleçleri tarafından tanımlandığı gibi) başvurmazsa, girdiler eşittir.
*  Aynı (muhtemelen boş) yakalanan dış değişken örnekleri kümesiyle aynı (büyük olasılıkla boş) bir değere sahip anlamsal özdeş *anonymous_method_expression*s veya *lambda_expression*s değerlendirmesinden üretilen çağırma listesi girişleri, eşit olması için izin verilir (ancak gerekli değildir).

### <a name="equality-operators-and-null"></a>Eşitlik işleçleri ve null

`==` ve `!=` işleçleri, işlem için önceden tanımlanmış veya Kullanıcı tanımlı bir operatör (unlifted veya yükseltilmemiş biçiminde) mevcut olmasa bile, tek bir işlenenin null yapılabilir türde ve diğeri de `null` değişmez değeri olarak izin verir.

Formlardan birine bir işlem için
```csharp
x == null
null == x
x != null
null != x
```
`x`, null yapılabilir bir türdeki bir ifadedir, işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) ilgili bir işleci bulamazsa, bunun yerine `x``HasValue` özelliğinden hesaplanır. Özellikle, ilk iki form `!x.HasValue`çevrilir ve son iki form `x.HasValue`dönüştürülür.

### <a name="the-is-operator"></a>SIS işleci

`is` işleci, bir nesnenin çalışma zamanı türünün belirli bir tür ile uyumlu olup olmadığını dinamik olarak denetlemek için kullanılır. İşlemin sonucu `E is T`, `E` bir ifadedir ve `T` bir tür ise, `E` bir başvuru dönüştürmesi, paketleme dönüştürmesi veya bir kutudan çıkarma dönüştürmesi tarafından `T` türüne başarıyla dönüştürülüp dönüştürülmeyeceğini belirten bir Boole değeridir. Tür bağımsız değişkenleri tüm tür parametreleri yerine eklendikten sonra işlem aşağıdaki gibi değerlendirilir:

*  `E` anonim bir işlevse, derleme zamanı hatası oluşur
*  `E` bir yöntem grubu veya `null` değişmez değeri ise, `E` türü bir başvuru türü veya null yapılabilir bir tür ise ve `E` değeri null ise sonuç false olur.
*  Aksi takdirde, `D` `E` dinamik türünü aşağıdaki gibi temsil edelim:
   * `E` türü bir başvuru türü ise, `D`, `E`tarafından örnek başvurusunun çalışma zamanı türüdür.
   * `E` türü null yapılabilir bir tür ise, `D` bu null yapılabilir türün temel türüdür.
   * `E` türü null yapılamayan bir değer türü ise, `D` `E`türüdür.
*  İşlemin sonucu `D` ve `T` aşağıdaki gibi değişir:
   * `T` bir başvuru türü ise, `D` ve `T` aynı türde ise sonuç true olur. `D` bir başvuru türüdür ve `D` bir değer türü ise ve `T` ' dan `D` ' a bir paketleme dönüştürmesi varsa.
   * `T` null yapılabilir bir tür ise, `D` temeldeki `T`türü ise sonuç true olur.
   * `T` null yapılamayan bir değer türü ise, `D` ve `T` aynı türde ise sonuç true olur.
   * Aksi takdirde, sonuç false 'tur.

Kullanıcı tanımlı dönüştürmelerin `is` işleci tarafından değerlendirilmediğini unutmayın.

### <a name="the-as-operator"></a>As işleci

`as` işleci, açıkça bir değeri belirli bir başvuru türüne veya null yapılabilir türe dönüştürmek için kullanılır. Atama ifadesinin ([cast ifadeleri](expressions.md#cast-expressions)) aksine, `as` işleci hiçbir şekilde özel durum oluşturmaz. Bunun yerine, belirtilen dönüştürme mümkün değilse, sonuçta elde edilen değer `null`.

Form `E as T`işleminde, `E` bir ifade olmalıdır ve `T` bir başvuru türü, bir başvuru türü olarak bilinen bir tür parametresi veya null yapılabilir bir tür olması gerekir. Ayrıca, aşağıdakilerden en az birinin doğru olması gerekir, aksi takdirde bir derleme zamanı hatası oluşur:

*  Bir kimlik ([kimlik dönüştürme](conversions.md#identity-conversion)), örtük null yapılabilir[(örtük Nullable dönüşümler](conversions.md#implicit-nullable-conversions)), örtük başvuru ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)), kutulama ([kutulama dönüştürmeleri](conversions.md#boxing-conversions)), açık boş değer atanabilir ([Açık Nullable dönüşümler](conversions.md#explicit-nullable-conversions)), açık başvuru ([açık başvuru dönüştürmeleri](conversions.md#explicit-reference-conversions)) veya kutudan çıkarma (kutudan çıkarma[dönüştürmeleri](conversions.md#unboxing-conversions)) dönüştürme `E` `T`.
*  `E` veya `T` türü açık bir tür.
*  `E`, `null` değişmez değerdir.

`E` derleme zamanı türü `dynamic`değilse, işlem `E as T` aynı sonucu üretir
```csharp
E is T ? (T)(E) : (T)null
```
`E` hariç, yalnızca bir kez değerlendirilir. Derleyicinin, yukarıdaki genişleme tarafından kapsanan iki dinamik tür denetimi yerine en çok bir dinamik tür denetimi gerçekleştirmesi için `E as T` iyileştirmek beklenir.

Derleme zamanı türü `E` `dynamic`ise, atama işlecinin aksine `as` işleci dinamik olarak bağlı değildir ([dinamik bağlama](expressions.md#dynamic-binding)). Bu nedenle, bu durumda genişletme şu şekilde olur:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Kullanıcı tanımlı dönüştürmeler gibi bazı dönüştürmelerin `as` işleçleriyle mümkün olmadığına ve bunun yerine atama ifadeleri kullanılarak gerçekleştirilmesi gerektiğini unutmayın.

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
sınıf kısıtlamasına sahip olduğundan, `G` tür parametresi `T` bir başvuru türü olarak bilinir. `H` tür parametresi `U` değil, ancak Bu nedenle `H` `as` işlecinin kullanımına izin verilmez.

## <a name="logical-operators"></a>Mantıksal işleçler

`&`, `^`ve `|` işleçleri mantıksal işleçler olarak adlandırılır.

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

Bir mantıksal işlecin işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

`op` mantıksal işleçlerden biri olduğu `x op y`form işlemi için, belirli bir operatör uygulamasını seçmek üzere aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanır. İşlenenler, seçili işlecin parametre türlerine dönüştürülür ve sonucun türü işlecin dönüş türüdür.

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

`&` işleci iki işlenenin bit düzeyinde mantıksal `AND` hesaplar, `|` işleci iki işlenenin bit düzeyinde mantıksal `OR` hesaplar ve `^` işleci iki işlenenin bit düzeyinde mantıksal dışlamalı `OR` hesaplar. Bu işlemlerden taşmalar mümkün değildir.

### <a name="enumeration-logical-operators"></a>Sabit listesi mantıksal işleçleri

Her numaralandırma türü `E` örtük olarak aşağıdaki önceden tanımlanmış mantıksal işleçleri sağlar:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

`x` ve `y`, temel alınan bir tür `U``E` bir numaralandırma türünün ifadeleridir ve `op`, mantıksal işleçlerden biridir, `(E)((U)x op (U)y)`değerlendirmesiyle tamamen aynıdır. `x op y` Diğer bir deyişle, mantıksal işleçler numaralandırma türü mantıksal işlemleri yalnızca iki işlenenin temel alınan türünde gerçekleştirir.

### <a name="boolean-logical-operators"></a>Boole mantıksal işleçleri

Önceden tanımlanmış Boole mantıksal işleçleri şunlardır:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

`x & y` sonucu, hem `x` hem de `y` `true`ise `true`. Aksi takdirde, sonuç `false`.

`x | y` sonucu, `x` ya da `y` `true`ise `true`. Aksi takdirde, sonuç `false`.

`x ^ y` sonucu `x` `true` ve `y` `false`ve `x` `false` ve `y` `true`ise `true`. Aksi takdirde, sonuç `false`. İşlenenler `bool`türünde olduğunda `^` işleci `!=` işleci ile aynı sonucu hesaplar.

### <a name="nullable-boolean-logical-operators"></a>Null yapılabilir Boolean mantıksal işleçler

Null yapılabilir Boolean türü `bool?` üç değeri temsil edebilir, `true`, `false`ve `null`ve kavramsal olarak SQL 'de Boolean ifadeler için kullanılan üç değerli tür ile benzerdir. `bool?` işlenenleri için `&` ve `|` işleçleri tarafından üretilen sonuçların SQL 'in üç değerli mantığıyla tutarlı olduğundan emin olmak için, aşağıdaki önceden tanımlanmış işleçler verilmiştir:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

Aşağıdaki tabloda, `true`, `false`ve `null`değerlerinin tüm birleşimleri için bu işleçler tarafından oluşturulan sonuçlar listelenmektedir.

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

`&&` ve `||` işleçleri Koşullu mantıksal işleçler olarak adlandırılır. Bunlar ayrıca "kısa devre dışı" mantıksal işleçler olarak da adlandırılır.

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

`&&` ve `||` işleçleri, `&` ve `|` işleçlerinin koşullu sürümleridir:

*  İşlem `x && y` işlem `x & y`karşılık gelir, ancak `y` yalnızca `x` `false`olmadığında değerlendirilir.
*  İşlem `x || y` işlem `x | y`karşılık gelir, ancak `y` yalnızca `x` `true`olmadığında değerlendirilir.

Koşullu bir mantıksal işlecin işleneni derleme zamanı türü `dynamic`, ifade dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, ifadenin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, derleme zamanı türü `dynamic`sahip olan işlenenleri çalışma zamanı türü kullanılarak çalışma zamanında gerçekleşmeyecektir.

`x && y` veya `x || y` form işlemi, işlem `x & y` veya `x | y`yazılmış gibi aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanarak işlenir. Ni

*  Aşırı yükleme çözümlemesi tek bir en iyi operatör bulamazsa veya aşırı yükleme çözümlemesi önceden tanımlı tamsayı mantıksal işleçlerinden birini seçerse bağlama zamanı hatası oluşur.
*  Aksi takdirde, seçili operatör önceden tanımlanmış Boole mantıksal işleçlerinden ([Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)) veya null yapılabilir Boolean mantıksal Işleçlerden ([null yapılabilir Boolean mantıksal](expressions.md#nullable-boolean-logical-operators)işleçler) biri ise, işlem [Boole Koşullu mantıksal işleçler](expressions.md#boolean-conditional-logical-operators)bölümünde açıklandığı şekilde işlenir.
*  Aksi takdirde, seçilen işleç Kullanıcı tanımlı bir işleçtir ve işlem [Kullanıcı tanımlı Koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators)bölümünde açıklandığı şekilde işlenir.

Koşullu mantıksal işleçleri doğrudan aşırı yüklemek mümkün değildir. Ancak, Koşullu mantıksal işleçler normal mantıksal işleçler açısından değerlendirildiğinden, normal mantıksal operatörlerin aşırı yüklemeleri, Koşullu mantıksal işleçlerin aşırı yüklerini de kabul edilen belirli kısıtlamalarla birlikte bulunur. Bu, [Kullanıcı tanımlı Koşullu mantıksal işleçlerde](expressions.md#user-defined-conditional-logical-operators)daha ayrıntılı olarak açıklanmıştır.

### <a name="boolean-conditional-logical-operators"></a>Boole Koşullu mantıksal işleçler

`&&` veya `||` işlenenleri `bool`, ya da işlenenleri geçerli bir `operator &` veya `operator |`tanımlamayan, ancak `bool`örtük dönüştürmeler tanımlamadığında, işlem şu şekilde işlenir:

*  İşlem `x && y` `x ? y : false`olarak değerlendirilir. Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `bool`türüne dönüştürülür. `x` `true`, `y` değerlendirilir ve `bool`türüne dönüştürülür ve bu işlemin sonucu olur. Aksi takdirde, işlemin sonucu `false`.
*  İşlem `x || y` `x ? true : y`olarak değerlendirilir. Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `bool`türüne dönüştürülür. `x` `true`, işlemin sonucu `true`olur. Aksi takdirde, `y` değerlendirilir ve `bool`türüne dönüştürülür ve bu işlemin sonucu olur.

### <a name="user-defined-conditional-logical-operators"></a>Kullanıcı tanımlı Koşullu mantıksal işleçler

`&&` veya `||` işlenenleri, Kullanıcı tanımlı geçerli bir `operator &` veya `operator |`bildiren türlerse, aşağıdakilerin her ikisi de doğru olmalıdır; burada `T`, seçili işlecin bildirildiği türdür:

*  Seçili işlecin her bir parametresinin dönüş türü ve türü `T`olmalıdır. Diğer bir deyişle, işleç, mantıksal `AND` veya `T`türünde iki işlenenin mantıksal `OR` hesaplamalıdır ve `T`türünde bir sonuç döndürmelidir.
*  `T`, `operator true` ve `operator false`bildirimleri içermelidir.

Bu gereksinimlerin herhangi biri karşılanmıyorsa bağlama zamanı hatası oluşur. Aksi takdirde, `&&` veya `||` işlemi, Kullanıcı tanımlı `operator true` veya `operator false` seçili kullanıcı tanımlı işleçle birleştirilerek değerlendirilir:

*  İşlem `x && y` `T.false(x) ? x : T.&(x, y)`olarak değerlendirilir; burada `T.false(x)` `T`olarak belirtilen `operator false` bir çağrıdır ve `T.&(x, y)` seçilen `operator &`çağrıdır. Diğer bir deyişle `x` ilk değerlendirilir ve `x` kesinlikle yanlış olduğunu anlamak için `operator false` sonuç üzerinde çağrılır. `x` kesinlikle false ise, işlemin sonucu daha önce `x`için hesaplanan değerdir. Aksi takdirde, `y` değerlendirilir ve seçilen `operator &`, daha önce `x` için hesaplanan değerde ve işlemin sonucunu üretmek için `y` için hesaplanan değer üzerinde çağrılır.
*  İşlem `x || y` `T.true(x) ? x : T.|(x, y)`olarak değerlendirilir; burada `T.true(x)` `T`olarak belirtilen `operator true` bir çağrıdır ve `T.|(x,y)` seçilen `operator|`çağrıdır. Diğer bir deyişle, `x` ilk değerlendirilir ve `x` kesinlikle doğru doğru olup olmadığını anlamak için `operator true` sonuç üzerinde çağrılır. Daha sonra, `x` kesinlikle doğru ise, işlemin sonucu daha önce `x`için hesaplanan değerdir. Aksi takdirde, `y` değerlendirilir ve seçilen `operator |`, daha önce `x` için hesaplanan değerde ve işlemin sonucunu üretmek için `y` için hesaplanan değer üzerinde çağrılır.

Bu işlemlerden birinde, `x` tarafından verilen ifade yalnızca bir kez değerlendirilir ve `y` tarafından verilen ifade değerlendirilmez ya da tam olarak bir kez değerlendirilmez.

`operator true` ve `operator false`uygulayan bir tür örneği için bkz. [veritabanı Boole türü](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Null birleştirme işleci

`??` işlecine null birleşim işleci denir.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Form `a ?? b` bir boş birleştirme ifadesi, `a` null yapılabilir bir tür veya başvuru türü olmasını gerektirir. Null olmayan `a`, `a ?? b` sonucu `a`; Aksi takdirde, sonuç `b`. İşlem yalnızca `a` null ise `b` değerlendirir.

Null birleştirme işleci, işlemlerin sağdan sola gruplanarak doğru ilişkilendirilebilir. Örneğin, `a ?? b ?? c` form ifadesi `a ?? (b ?? c)`olarak değerlendirilir. Genel koşullarda, form `E1 ?? E2 ?? ... ?? En` bir ifade, null olmayan veya tüm işlenenler null ise null olan işlenenlerin Birincini döndürür.

`a ?? b` ifadenin türü, işlenenlerde hangi örtük dönüştürmelerin kullanılabilir olduğuna bağlıdır. Tercih sırasına göre `a ?? b` türü `A0`, `A`veya `B`, burada `A` tür olan `a` (`a` bir tür olduğunda), `B` türü `b` olur (`b` bir tür olduğunda) ve `A0`, null yapılabilir bir tür ise `A` temel türüdür. Özellikle, `a ?? b` aşağıdaki şekilde işlenir:

*  `A` varsa ve null yapılabilir bir tür ya da bir başvuru türü değilse, derleme zamanı hatası oluşur.
*  `b` dinamik bir ifadesiyse, sonuç türü `dynamic`. Çalışma zamanında `a` ilk olarak değerlendirilir. `a` null değilse, `a` dinamik olarak dönüştürülür ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve bu sonuç olur.
*  Aksi takdirde, `A` varsa ve null yapılabilir bir tür ise ve `b` `A0`'e örtük bir dönüştürme varsa, sonuç türü `A0`olur. Çalışma zamanında `a` ilk olarak değerlendirilir. `a` null değilse, `a` `A0`tipine geri sarılır ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve `A0`türüne dönüştürülür ve bu sonuç olur.
*  Aksi takdirde, `A` varsa ve `b` `A`'e örtük bir dönüştürme varsa, sonuç türü `A`olur. Çalışma zamanında `a` ilk olarak değerlendirilir. `a` null değilse, `a` sonuç olur. Aksi takdirde, `b` değerlendirilir ve `A`türüne dönüştürülür ve bu sonuç olur.
*  Aksi takdirde, `b` `B` bir tür varsa ve `B``a` bir örtük dönüştürme varsa, sonuç türü `B`olur. Çalışma zamanında `a` ilk olarak değerlendirilir. `a` null değilse, `A0` `a` (`A` varsa ve null yapılabilir) ve `B`türüne dönüştürülürse, bu, bu sonuç haline gelir. Aksi takdirde, `b` değerlendirilir ve sonuç olur.
*  Aksi takdirde, `a` ve `b` uyumsuzdur ve derleme zamanı hatası oluşur.

## <a name="conditional-operator"></a>Koşullu işleç

`?:` işlecine koşullu işleç denir. Üçlü işleç olarak da adlandırılır.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Formun koşullu bir ifadesi öncelikle koşul `b`değerlendirir `b ? x : y`. `b` `true`, `x` değerlendirilir ve işlemin sonucu olur. Aksi takdirde, `y` değerlendirilir ve işlemin sonucu olur. Koşullu bir ifade `x` ve `y`hiçbir şekilde değerlendirmez.

Koşullu operatör doğru ilişkilendirilebilir, yani işlemler sağdan sola gruplandırılır. Örneğin, `a ? b : c ? d : e` form ifadesi `a ? b : (c ? d : e)`olarak değerlendirilir.

`?:` işlecinin ilk işleneni, örtük olarak `bool`veya `operator true`uygulayan bir türün bir ifadesi olarak dönüştürülebileceği bir ifade olmalıdır. Bu gereksinimlerin hiçbiri karşılanmazsa, bir derleme zamanı hatası oluşur.

`?:` işlecinin ikinci ve üçüncü işlenenleri, `x` ve `y`, koşullu ifadenin türünü denetler.

*  `x` türü `X` varsa ve `y` tür `Y` sahipse
   * Örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), `X` `Y`, ancak `Y` ' den `X`' a varsa, `Y` koşullu ifadenin türüdür.
   * Örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), `Y` `X`, ancak `X` ' den `Y`' a varsa, `X` koşullu ifadenin türüdür.
   * Aksi takdirde, hiçbir ifade türü belirlenemez ve derleme zamanı hatası oluşur.
*  Yalnızca bir `x` ve `y` bir tür varsa ve hem `x` hem de `y`, bu türe örtülü olarak dönüştürülebilir ve sonra koşullu ifadenin türüdür.
*  Aksi takdirde, hiçbir ifade türü belirlenemez ve derleme zamanı hatası oluşur.

Formun koşullu ifadesinin çalışma zamanı işleme `b ? x : y` aşağıdaki adımlardan oluşur:

*  İlk olarak, `b` değerlendirilir ve `b` `bool` değeri belirlenir:
   * `b` türünden `bool` örtük bir dönüştürme varsa, bu örtük dönüştürme bir `bool` değeri üretmek için gerçekleştirilir.
   * Aksi takdirde, bir `bool` değeri üretmek için `b` türü tarafından tanımlanan `operator true` çağrılır.
*  Yukarıdaki adım tarafından üretilen `bool` değeri `true`, `x` değerlendirilir ve koşullu ifadenin türüne dönüştürülür ve bu, koşullu ifadenin sonucu olur.
*  Aksi takdirde, `y` değerlendirilir ve koşullu ifadenin türüne dönüştürülür ve bu, koşullu ifadenin sonucu olur.

## <a name="anonymous-function-expressions"></a>Anonim işlev ifadeleri

***Anonim işlev*** , "satır içi" yöntem tanımını temsil eden bir ifadedir. Anonim bir işlev, ve için bir değer veya tür içermez, ancak uyumlu bir temsilciye veya ifade ağacı türüne dönüştürülebilir. Anonim işlev dönüştürmenin değerlendirmesi, dönüştürmenin hedef türüne bağlıdır: bir temsilci türü ise, dönüştürme, anonim işlevin tanımladığı yönteme başvuran bir temsilci değeri olarak değerlendirilir. Bir ifade ağacı türü ise dönüştürme, yöntemin yapısını bir nesne yapısı olarak temsil eden bir ifade ağacı olarak değerlendirilir.

Geçmiş nedenlerle anonim işlevlerin iki sözdizimsel özelliği vardır, örneğin *lambda_expression*s ve *anonymous_method_expression*s. Neredeyse tüm amaçlar için *lambda_expression*s, geriye doğru uyumluluk için dilde kalan *anonymous_method_expression*s ' den daha kısa ve açıklayıcı bir şekilde bulunur.

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

`=>` işleci atama (`=`) ile aynı önceliğe sahiptir ve doğru ilişkilendirilebilir.

`async` değiştiricisine sahip anonim bir işlev zaman uyumsuz bir işlevdir ve [yineleyiciler](classes.md#iterators)bölümünde açıklanan kuralları izler.

Bir *lambda_expression* biçimindeki anonim işlevin parametreleri açık veya örtük olarak yazılabilir olabilir. Açıkça yazılmış bir parametre listesinde, her parametrenin türü açıkça belirtilir. Örtük olarak yazılmış bir parametre listesinde, parametrelerin türleri, anonim işlevin gerçekleştiği bağlamdan çıkarslanmıştır — özellikle, anonim işlev uyumlu bir temsilci türüne veya ifade ağacı türüne dönüştürüldüğünde, bu tür parametre türlerini ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)) sağlar.

Tek ve örtük olarak yazılmış bir parametreye sahip anonim bir işlevde, parantez parametre listesinden atlanabilir. Diğer bir deyişle, formun anonim bir işlevi
```csharp
( param ) => expr
```
kısaltılabilir
```csharp
param => expr
```

Bir *anonymous_method_expression* biçimindeki adsız işlevin parametre listesi isteğe bağlıdır. Belirtilmişse, parametrelerin açıkça yazılması gerekir. Aksi takdirde, anonim işlev `out` parametreleri içermeyen herhangi bir parametre listesi olan bir temsilciye dönüştürülebilir.

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
*  *lambda_expression*s parametre türlerinin atlanmasına ve çıkarlanmasına izin verirken, *anonymous_method_expression*s parametre türlerinin açıkça belirtilmesini gerektirir.
*  Bir *lambda_expression* gövdesi bir ifade veya deyim bloğu olabilir, ancak bir *anonymous_method_expression* gövdesi bir deyim bloğu olmalıdır.
*  Yalnızca *lambda_expression*s, uyumlu ifade ağacı türlerine Dönüştürmelere sahiptir ([ifade ağacı türleri](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Anonim işlev imzaları

Anonim işlev için isteğe bağlı *anonymous_function_signature* adları ve isteğe bağlı olarak anonim işlev için biçimsel parametrelerin türlerini tanımlar. Anonim işlevin parametrelerinin kapsamı *anonymous_function_body*. ([Kapsamlar](basic-concepts.md#scopes)) Parametre listesi ile birlikte (verildiyse) anonim-yöntem-gövde bir bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) oluşturur. Bu nedenle, anonim işlev parametresinin adı için bir yerel değişkenin adı, yerel sabit veya kapsamı *anonymous_method_expression* ya da *lambda_expression*içeren bir parametre için derleme zamanı hatası oluşur.

Anonim bir işlevde *explicit_anonymous_function_signature*varsa, uyumlu temsilci türleri ve ifade ağacı türleri kümesi aynı sırayla aynı parametre türleri ve değiştiricilerle kısıtlıdır. Yöntem grubu dönüştürmelerinde ([Yöntem grubu dönüştürmeleri](conversions.md#method-group-conversions)), anonim işlev parametre türlerinin Contra-varyansı desteklenmez. Anonim bir işlevde *anonymous_function_signature*yoksa, uyumlu temsilci türleri ve ifade ağacı türleri kümesi, hiçbir `out` parametresi olmayan ile kısıtlıdır.

*Anonymous_function_signature* öznitelikler veya parametre dizisi içeremediğini unutmayın. Bununla birlikte, bir *anonymous_function_signature* , parametre listesi parametre dizisi içeren bir temsilci türüyle uyumlu olabilir.

Ayrıca, aynı zamanda derleme zamanında ([ifade ağacı türleri](types.md#expression-tree-types)) hala başarısız olabilecek bir ifade ağacı türüne dönüştürme

### <a name="anonymous-function-bodies"></a>Anonim işlev gövdeleri

Anonim bir işlevin gövdesi (*ifadesi* veya *bloğu*) aşağıdaki kurallara tabidir:

*  Anonim işlev bir imza içeriyorsa, İmzada belirtilen parametreler gövdede kullanılabilir. Anonim işlevde imza yoksa, parametrelere sahip bir temsilci türüne veya ifade türüne dönüştürülebilir ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)), ancak parametrelere gövdede erişilemez.
*  En yakın kapsayan anonim işlevin imzasında (varsa) belirtilen `ref` veya `out` parametreleri dışında, gövdenin bir `ref` veya `out` parametresine erişmesi için derleme zamanı hatası olur.
*  `this` türü bir yapı türünde olduğunda, gövdenin `this`erişmesi için derleme zamanı hatası olur. Bu, erişimin açık (`this.x`gibi) veya örtük (`x`, yapının örnek üyesi olduğu `x` gibi) için geçerlidir. Bu kural yalnızca bu erişimi yasaklar ve üye aramanın yapının bir üyesi olup olmadığını etkilemez.
*  Gövde, anonim işlevin dış değişkenlerine ([dış değişkenler](expressions.md#outer-variables)) erişebilir. Bir dış değişkene erişim, *lambda_expression* veya *anonymous_method_expression* değerlendirildiği sırada etkin olan değişkenin örneğine başvuracaktır ([anonim işlev ifadelerinin değerlendirmesi](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Gövdesi için bir derleme zamanı hatası, örneğin, hedefi gövdenin dışında veya kapsanan bir anonim işlevin gövdesinde olan bir `goto` bir ifade, `break` ifade veya `continue` ifadesini içermelidir.
*  Gövdedeki bir `return` ifade, kapsayan işlev üyesinden değil, en yakın kapsayan anonim işlev çağrısından denetim döndürüyor. `return` deyiminde belirtilen bir ifade, en yakın kapsayan *lambda_expression* veya *Anonymous_method_expression* dönüştürüldüğü ([anonim işlev dönüştürmeleri](conversions.md#anonymous-function-conversions)), temsilci türünün veya ifade ağacı türünün dönüş türüne örtülü olarak dönüştürülebilir olmalıdır.

*Lambda_expression* veya *anonymous_method_expression*için değerlendirme ve çağırma aracılığıyla değil, anonim bir işlevin bloğunu yürütmek için bir yol olup olmadığı açıkça belirtilmemiş. Özellikle, derleyici bir veya daha fazla adlandırılmış yöntemi ya da türü birleştirerek anonim bir işlev uygulamayı tercih edebilir. Bu tür birleştirilmiş öğelerin adları, derleyici kullanımı için ayrılmış bir biçimde olmalıdır.

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

`ItemList<T>` sınıfında iki `Sum` yöntemi vardır. Her biri, bir liste öğesinden toplanacak değeri çıkaran bir `selector` bağımsız değişkeni alır. Ayıklanan değer bir `int` veya `double` olabilir ve sonuçta elde edilen Sum aynı şekilde bir `int` veya `double`.

`Sum` yöntemleri, bir siparişte ayrıntı satırları listesinden toplamları hesaplamak için kullanılabilir.

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

`orderDetails.Sum`ilk çağrısında, anonim işlev `d => d. UnitCount` hem `Func<Detail,int>` hem de `Func<Detail,double>`uyumlu olduğundan, her iki `Sum` yöntemi de geçerlidir. Ancak, `Func<Detail,int>` dönüştürme işlemi `Func<Detail,double>`dönüşümünden daha iyi olduğundan, aşırı yükleme çözümlemesi ilk `Sum` yöntemini seçer.

`orderDetails.Sum`ikinci çağrılışında, anonim işlev `d => d.UnitPrice * d.UnitCount` `double`türünde bir değer ürettiğinden yalnızca ikinci `Sum` yöntemi geçerlidir. Bu nedenle, aşırı yükleme çözümlemesi bu çağrı için ikinci `Sum` yöntemini seçer.

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
`x` yerel değişkeni anonim işlev tarafından yakalanır ve `x` yaşam süresi en az, `F` ' den döndürülen temsilci atık toplama için uygun hale gelene kadar (programın çok sona erene kadar gerçekleşmez) genişletilir. Anonymous işlevinin her çağrısı aynı `x`örneği üzerinde çalışır, örneğin çıktısı şu şekilde olur:
```console
1
2
3
```

Yerel bir değişken veya bir değer parametresi anonim bir işlev tarafından yakalandıktan sonra, yerel değişken veya parametre artık sabit bir değişken ([sabit ve taşınabilir değişkenler](unsafe-code.md#fixed-and-moveable-variables)) olarak kabul edilmez, ancak bunun yerine taşınabilir bir değişken olarak kabul edilir. Bu nedenle, yakalanan bir dış değişkenin adresini alan `unsafe` kodun, önce değişkeni onarmak için `fixed` ifadesini kullanması gerekir.

Yakalanamayan bir değişkenin aksine, yakalanan bir yerel değişkenin aynı anda birden fazla yürütme iş parçacığına sunulabileceğini unutmayın.

#### <a name="instantiation-of-local-variables"></a>Yerel değişkenlerin örneklenmesi

Bir yerel değişken, yürütme değişkenin kapsamına girdiğinde ***örnek*** olarak değerlendirilir. Örneğin, aşağıdaki yöntem çağrıldığında yerel değişken `x`, döngünün her yinelemesi için bir kez oluşturulur ve üç kez başlatılır.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Ancak, `x` bildirimini döngü dışına taşımak `x`tek bir örneklemede oluşur:
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
çıktı:
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
üç temsilci aynı `x` örneğini yakalar, ancak `y`örneklerini ayırır ve çıkış şu şekilde olur:
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
iki anonim işlev, `x`yerel değişkenin aynı örneğini yakalar ve bu nedenle bu değişken aracılığıyla "iletişim kurabilir". Örneğin çıktısı şu şekilde olur:
```console
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Anonim işlev ifadelerinin değerlendirmesi

Anonim bir işlev `F` her zaman doğrudan veya bir temsilci oluşturma `new D(F)`ifadesinin yürütülmesi aracılığıyla bir temsilci türüne `D` ya da bir ifade ağacı türüne dönüştürülmelidir `E`olmalıdır. Bu dönüştürme, anonim işlev [dönüştürmeleri](conversions.md#anonymous-function-conversions)bölümünde açıklandığı gibi anonim işlevin sonucunu belirler.

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

Sorgu ifadesi bir `from` yan tümcesiyle başlar ve `select` ya da `group` yan tümcesiyle biter. İlk `from` yan tümcesinin arkasından sıfır veya daha fazla `from`, `let`, `where`, `join` veya `orderby` yan tümceleri gelebilir. Her `from` yan tümcesi, bir ***dizi***öğeleri üzerinde değişen bir ***Aralık değişkeni*** sunan bir üreticidir. Her `let` yan tümcesi, önceki Aralık değişkenlerine göre hesaplanan bir değeri temsil eden bir Aralık değişkeni tanıtır. Her `where` yan tümcesi, nesneleri sonuçtan dışlayan bir filtredir. Her `join` yan tümcesi, eşleşen çiftleri oluşturan kaynak dizinin belirtilen anahtarlarını başka bir dizinin anahtarlarıyla karşılaştırır. Her `orderby` yan tümcesi öğeleri belirtilen ölçütlere göre yeniden sıralar. Son `select` veya `group` yan tümcesi, Aralık değişkenleri açısından sonucun şeklini belirtir. Son olarak, bir `into` yan tümcesi, bir sorgunun sonuçlarını sonraki bir sorguda Oluşturucu olarak düşünerek, "splice" sorguları için kullanılabilir.

### <a name="ambiguities-in-query-expressions"></a>Sorgu ifadelerinde belirsizlikleri

Sorgu ifadeleri, belirli bir bağlamda özel anlamı olan tanımlayıcılar içeren bir dizi "bağlamsal anahtar sözcük", yani. Bunlar özellikle `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` ve `by`. Bu tanımlayıcıların, anahtar sözcük veya basit adlar olarak karışık olarak kullanılması nedeniyle sorgu ifadelerinde belirsizlikleri kaçınmak için, bu tanımlayıcılar bir sorgu ifadesinin içinde herhangi bir yerde olduğunda anahtar sözcük olarak kabul edilir.

Bu amaçla, bir sorgu ifadesi "`from identifier`" ile başlayan ve "`;`", "`=`" veya "`,`" dışında herhangi bir belirteç tarafından başlayan ifadedir.

Bu sözcüklerin bir sorgu ifadesinde tanımlayıcı olarak kullanılabilmesi için, "`@`" ([tanımlayıcılar](lexical-structure.md#identifiers)) ön eki uygulanabilir.

### <a name="query-expression-translation"></a>Sorgu ifadesi çevirisi

Dil C# , sorgu ifadelerinin yürütme semantiğini belirtmez. Bunun yerine sorgu ifadeleri *sorgu ifade düzenine* ([sorgu ifade deseninin](expressions.md#the-query-expression-pattern)) bağlı olan yöntemlerin etkinleştirilmesinde çevrilir. Özellikle, sorgu ifadeleri `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`ve `Cast`adlı yöntemlerin etkinleştirilmesine çevrilir. [Sorgu ifadesi](expressions.md#the-query-expression-pattern)düzeninde açıklandığı gibi, bu yöntemlerin belirli imzaları ve sonuç türlerini olması beklenir. Bu yöntemler, sorgulanan nesnenin örnek yöntemleri veya nesne dışındaki genişletme yöntemleri olabilir ve sorgunun gerçek yürütmesini uygular.

Sorgu ifadelerinden Yöntem etkinleştirmeleri 'e yapılan çeviri, herhangi bir tür bağlama veya aşırı yükleme çözümlemesi yapılmadan önce oluşan bir sözdizimsel eşleme olur. Çeviri, sözdizimsel olarak doğru bir şekilde garanti edilir, ancak anlamsal olarak doğru C# kodun üretilmesi garanti edilmez. Sorgu ifadelerinin çevirisinde, sonuçta elde edilen yöntem etkinleştirmeleri normal Yöntem etkinleştirmeleri olarak işlenir ve bu, örneğin Yöntemler yoksa, bağımsız değişkenlerin yanlış türleri varsa veya yöntemler geneldir ve Tür çıkarımı başarısız oluyor.

Bir sorgu ifadesi, daha fazla azaltmayı mümkün olana kadar, aşağıdaki Çeviriler tekrar tekrar uygulanarak işlenir. Çeviriler uygulama sırasıyla listelenir: her bölümde, yukarıdaki bölümlerdeki çevirilerin tüketilildiği ve tükendiğinde, bir bölümün aynı sorgu ifadesinin işlenmesinde daha sonra yeniden ziyaret edilmesinin varsayıldığı varsayılır.

Sorgu ifadelerinde Aralık değişkenlerine atamaya izin verilmez. Ancak bir C# uygulamaya bu kısıtlamayı her zaman zorlamamasına izin verilir, çünkü bu durum bazen burada sunulan sözdizimsel çeviri düzeninde mümkün olmayabilir.

Belirli Çeviriler, `*`tarafından belirtilen saydam tanımlayıcılarla Aralık değişkenleri ekler. Saydam tanımlayıcıların özel özellikleri, [saydam tanımlayıcılarla](expressions.md#transparent-identifiers)daha ayrıntılı bir şekilde ele alınmıştır.

#### <a name="select-and-groupby-clauses-with-continuations"></a>Devamlılıklarla seçim ve GroupBy yan tümceleri

Devam eden bir sorgu ifadesi
```csharp
from ... into x ...
```
çevrilir
```csharp
from x in ( from ... ) ...
```

Aşağıdaki bölümlerde yer alan Çeviriler, sorguların `into` devamlılıklar olmadığını varsayar.

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

Açık Aralık değişkeni türleri genel olmayan `IEnumerable` arabirimini uygulayan, ancak genel `IEnumerable<T>` arabirimini uygulayan koleksiyonları sorgulamak için yararlıdır. Yukarıdaki örnekte, `customers` `ArrayList`türünde olması durumunda bu durum olacaktır.

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

Bir bozuk sorgu ifadesi, kaynağın öğelerini önemli bir şekilde seçen bir ifadedir. Çevirinin daha sonraki bir aşaması, diğer çeviri adımları tarafından tanıtılan ve kaynakları kendi kaynağıyla değiştirerek çıkartılmış olan sorgu kaldırma işlemleri kaldırır. Sorgu ifadesinin sonucunun, sorgunun istemcisinin türünü ve kimliğini açığa çıkardığından, hiçbir şekilde kaynak nesnenin kendisi olduğundan emin olmak önemlidir. Bu nedenle bu adım, kaynak kodunda doğrudan yazılan sorguları, kaynak üzerinde açıkça `Select` çağırarak kaldırır. Daha sonra bu yöntemlerin kaynak nesnenin kendisini döndürmemesini sağlamak için `Select` ve diğer sorgu işleçleri uygulayıcılarına kadar.

#### <a name="from-let-where-join-and-orderby-clauses"></a>Kimden, Let, WHERE, JOIN ve OrderBy yan tümceleri

İkinci bir `from` yan tümcesi ve sonra `select` yan tümcesi içeren bir sorgu ifadesi
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

`let` yan tümcesiyle bir sorgu ifadesi
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

`where` yan tümcesiyle bir sorgu ifadesi
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

Bir `into` ve ardından bir `select` yan tümcesi olmadan `join` yan tümcesiyle bir sorgu ifadesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
çevrilir
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Bir `into` ve ardından bir `select` yan tümcesi dışında bir `join` yan tümcesi içeren bir sorgu ifadesi
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

Bir `into` ve ardından bir `select` yan tümcesi içeren `join` yan tümcesiyle bir sorgu ifadesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
çevrilir
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Bir `into` ve ardından bir `select` yan tümcesi dışında bir `join` yan tümcesi içeren bir sorgu ifadesi
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

`orderby` yan tümcesiyle bir sorgu ifadesi
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

Bir sıralama yan tümcesi `descending` yön göstergesi belirtiyorsa, bunun yerine `OrderByDescending` veya `ThenByDescending` bir çağırma oluşturulur.

Aşağıdaki Çeviriler `let`, `where`, `join` veya `orderby` yan tümcelerinin olmadığını ve her sorgu ifadesinde bir ilk `from` yan tümcesinin daha fazlasını belirtdüğünü varsayar.

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
Burada `x`, aksi durumda görünmeyen ve erişilemeyen bir derleyicinin ürettiği tanıtıcıdır.

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
Burada `x`, aksi durumda görünmeyen ve erişilemeyen bir derleyicinin ürettiği tanıtıcıdır.

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
`x` ve `y` derleyici tarafından oluşturulan ve aksi durumda görünmeyen ve erişilemeyen tanımlayıcılardır.

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

Belirli Çeviriler, `*`tarafından belirtilen ***saydam tanımlayıcılarla*** Aralık değişkenleri ekler. Saydam tanımlayıcılar uygun bir dil özelliği değildir; Bunlar yalnızca sorgu ifadesi çevirisi işleminde bir ara adım olarak mevcuttur.

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
Burada `x`, aksi durumda görünmeyen ve erişilemeyen bir derleyicinin ürettiği tanıtıcıdır.

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
`x`, `y`ve `z` derleyici tarafından oluşturulan ve aksi durumda görünmeyen ve erişilemeyen tanımlayıcılardır.

### <a name="the-query-expression-pattern"></a>Sorgu ifadesi deseninin

***Sorgu ifadesi stili*** , sorgu ifadelerini desteklemek için uygulayabileceğiniz yöntemlerin bir modelini oluşturur. Sorgu ifadeleri, sözdizimsel eşleme yoluyla Yöntem etkinleştirmeleri 'e çevrildiği için, türlerin sorgu ifadesi deseninin nasıl uygulandığı konusunda önemli ölçüde esneklik vardır. Örneğin, iki aynı çağırma söz dizimini içerdiğinden ve Yöntemler, anonim işlevler her ikisine de dönüştürülebildiğinden temsilci veya ifade ağaçları isteyebildiğinden, düzenin yöntemleri örnek yöntemleri veya genişletme yöntemleri olarak uygulanabilir.

Sorgu ifadesi modelini destekleyen `C<T>` genel bir tür için önerilen şekil aşağıda gösterilmiştir. Parametre ve sonuç türleri arasındaki uygun ilişkileri göstermek için genel bir tür kullanılır, ancak genel olmayan türler için de düzeni uygulamak mümkündür.

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

Yukarıdaki yöntemler `Func<T1,R>` ve `Func<T1,T2,R>`genel temsilci türlerini kullanır, ancak parametre ve sonuç türlerinde aynı ilişkilerle diğer temsilci veya ifade ağacı türleri de aynı şekilde kullanılabilir.

`ThenBy` ve `ThenByDescending` yöntemlerinin yalnızca bir `OrderBy` veya `OrderByDescending`sonucu üzerinde kullanılabilir olmasını sağlayan `C<T>` ve `O<T>` arasında önerilen ilişkiye dikkat edin. Ayrıca, her bir iç sıranın ek bir `Key` özelliğine sahip olduğu `GroupBy`, bir dizi sıra için önerilen şekle de dikkat edin.

`System.Linq` ad alanı, `System.Collections.Generic.IEnumerable<T>` arabirimini uygulayan herhangi bir tür için sorgu işleci deseninin bir uygulamasını sağlar.

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

`=` işlecine ***basit atama işleci***denir. Sağ işlenenin değerini, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine atar. Basit atama işlecinin sol işleneni bir olay erişimi olmayabilir ( [alan benzeri olaylar](classes.md#field-like-events)bölümünde açıklananlar dışında). Basit atama işleci [basit atamada](expressions.md#simple-assignment)açıklanmıştır.

`=` işlecinden farklı atama işleçleri ***bileşik atama işleçleri***olarak adlandırılır. Bu işleçler iki işlenen üzerinde belirtilen işlemi gerçekleştirir ve ardından sonuçtaki değeri, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine atar. Bileşik atama işleçleri [Birleşik atama](expressions.md#compound-assignment)bölümünde açıklanmıştır.

Sol işlenen olarak bir olay erişim ifadesiyle `+=` ve `-=` işleçlerine *olay atama işleçleri*denir. Sol işlenen olarak bir olay erişimiyle hiçbir başka atama işleci geçerli değildir. Olay atama işleçleri [olay atamasında](expressions.md#event-assignment)açıklanmaktadır.

Atama işleçleri doğru ilişkilendirilebilir, yani işlemler sağdan sola gruplandırılır. Örneğin, `a = b = c` form ifadesi `a = (b = c)`olarak değerlendirilir.

### <a name="simple-assignment"></a>Basit atama

`=` işlecine basit atama işleci denir.

Basit bir atamanın sol işleneni form `E.P` veya `E` derleme zamanı türü `dynamic`sahip olduğu `E[Ei]`, atama dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, atama ifadesinin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, `E`çalışma zamanı türüne göre çalışma zamanında gerçekleşmeyecektir.

Basit bir atamada, sağ işlenen, sol işlenenin türüne örtük olarak dönüştürülebilir bir ifade olmalıdır. İşlem, sol işlenen tarafından verilen değişken, özellik veya Dizin Oluşturucu öğesine sağ işlenenin değerini atar.

Basit atama ifadesinin sonucu, sol işlenene atanan değerdir. Sonuç, sol işleneniyle aynı türe sahiptir ve her zaman bir değer olarak sınıflandırılır.

Sol işlenen bir özellik veya Dizin Oluşturucu erişimi ise, özelliğin veya dizin oluşturucunun bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Form `x = y` basit atamanın çalışma zamanı işleme aşağıdaki adımlardan oluşur:

*  `x` bir değişken olarak sınıflandırıldıysanız:
   * `x`, değişkeni üretmek için değerlendirilir.
   * `y` değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla `x` türüne dönüştürülür.
   * `x` tarafından verilen değişken bir *reference_type*dizi öğesidir `y` için hesaplanan değerin, `x` bir öğe olan dizi örneğiyle uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi yapılır. `y` `null`veya `y` tarafından başvurulan örneğin gerçek türünden, `x`içeren dizi örneğinin gerçek öğe türüne dolaylı bir başvuru dönüştürmesi ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) varsa denetim başarılı olur. Aksi takdirde, bir `System.ArrayTypeMismatchException` oluşturulur.
   * `y` değerlendirmesinden ve dönüşümden elde edilen değer, `x`değerlendirmesi tarafından verilen konuma depolanır.
*  `x`, özellik veya Dizin Oluşturucu erişimi olarak sınıflandırıldıysanız:
   * Örnek ifadesi (`x` `static`değilse) ve bağımsız değişken listesi (`x` bir Dizin Oluşturucu erişimsiyse) `x` ile ilişkili olarak değerlendirilir ve sonuçlar sonraki `set` erişimci çağrısında kullanılır.
   * `y` değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla `x` türüne dönüştürülür.
   * `x` `set` erişimcisi `value` bağımsız değişkeni olarak `y` için hesaplanan değerle çağrılır.

Dizi birlikte değişim kuralları ([dizi Kovaryans](arrays.md#array-covariance)), dizi türü bir değere `A[]` `B[]`bir dizi türü örneğine izin verir, `B` ' den `A`örtük bir başvuru dönüştürmesi sağlanmış olur. Bu kurallar nedeniyle, bir *reference_type* dizi öğesine atama, atanmakta olan değerin dizi örneğiyle uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi gerektirir. Örnekte
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
Son atama, bir `ArrayList` örneği bir `string[]`öğesinde depolanamadığı için `System.ArrayTypeMismatchException` oluşturulmasına neden olur.

Bir *struct_type* içinde bildirildiği bir özellik veya Dizin Oluşturucu bir atamanın hedefi olduğunda, özellik veya Dizin Oluşturucu erişimi ile ilişkili örnek ifadesi bir değişken olarak sınıflandırılmalıdır. Örnek ifadesi bir değer olarak sınıflandırılasiyse, bağlama zamanı hatası oluşur. [Üye erişimi](expressions.md#member-access)nedeniyle aynı kural alanlar için de geçerli olur.

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
`p` ve `r` değişkenler olduğundan `p.X`, `p.Y`, `r.A`ve `r.B` atamalara izin verilir. Ancak, örnekte
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
`r.A` ve `r.B` değişken olmadığından atamalar geçersizdir.

### <a name="compound-assignment"></a>Bileşik atama

Bileşik atamanın sol işleneni `E.P` veya `E` derleme zamanı türü `dynamic`sahip olduğu `E[Ei]`, atama dinamik olarak bağlanır ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda, atama ifadesinin derleme zamanı türü `dynamic`ve aşağıda açıklanan çözüm, `E`çalışma zamanı türüne göre çalışma zamanında gerçekleşmeyecektir.

`x op= y` bir işlem, işlem `x op y`yazılmış gibi ikili işleç aşırı yükleme çözümlemesi ([ikili işleç aşırı yükleme çözümlemesi](expressions.md#binary-operator-overload-resolution)) uygulanarak işlenir. Ni

*  Seçili işlecin dönüş türü örtük olarak `x`türüne dönüştürülebilir ise, işlem `x = x op y`olarak değerlendirilir, ancak `x` yalnızca bir kez değerlendirilir.
*  Aksi takdirde, seçilen işleç önceden tanımlanmış bir işleçse, seçili işlecin dönüş türü `x`türüne açıkça dönüştürülebilir ve `y` `x` türüne örtük olarak dönüştürülebilir veya işleç bir kaydırma işleçse, işlem `x = (T)(x op y)`olarak değerlendirilir; burada `T`, `x`yalnızca bir kez değerlendirilir.
*  Aksi takdirde, bileşik atama geçersizdir ve bir bağlama zamanı hatası oluşur.

"Yalnızca bir kez değerlendirilir" terimi, `x op y`değerlendirmesinde, `x` yapısal ifadelerin sonuçlarının geçici olarak kaydedildiği ve `x`atamasını gerçekleştirirken yeniden kullanıldığı anlamına gelir. Örneğin, atama `A()[B()] += C()`, `A` `int[]`döndüren bir yöntem olduğu ve `B` ve `C` `int`döndüren metotlardır, yöntemler yalnızca bir kez çağrılır; sırasıyla `A`, `B``C`.

Bileşik atamanın sol işleneni bir özellik erişimi veya Dizin Oluşturucu erişimi olduğunda, özelliğin veya dizin oluşturucunun hem `get` erişimcisi hem de bir `set` erişimcisi olmalıdır. Bu durumda, bir bağlama zamanı hatası oluşur.

Yukarıdaki ikinci kural, `x op= y` belirli bağlamlarda `x = (T)(x op y)` olarak değerlendirilmesine izin verir. Kural, sol işlenen `sbyte`, `byte`, `short`, `ushort`veya `char`türünde olduğunda, önceden tanımlanmış işleçlerin Bileşik işleçler olarak kullanılabilmesi için vardır. Her iki bağımsız değişken de bu türlerden birinde olsa da, önceden tanımlanmış işleçler, [ikili sayısal yükseltmeler](expressions.md#binary-numeric-promotions)bölümünde açıklandığı gibi `int`türünde bir sonuç üretir. Bu nedenle, bir dönüştürme olmadan sonucu sol işlenene atamak mümkün olmaz.

Önceden tanımlanmış işleçler için kuralın sezgisel etkisi, hem `x op y` hem de `x = y` izin verildiğinde `x op= y` izin verilir. Örnekte
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

Bir `+=` veya `-=` işlecinin sol işleneni bir olay erişimi olarak sınıflandırıldığında, ifade aşağıdaki gibi değerlendirilir:

*  Olay erişiminin, varsa örnek ifadesi değerlendirilir.
*  `+=` veya `-=` işlecinin sağ işleneni değerlendirilir ve gerekirse örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) aracılığıyla sol işlenenin türüne dönüştürülür.
*  Etkinliğin olay erişimcisi, değerlendirmeden sonra ve gerekirse dönüştürme işlemi tamamlandıktan sonra bağımsız değişken listesiyle çağrılır. İşleç `+=`, `add` erişimci çağrılır; işleç `-=`, `remove` erişimcisi çağrılır.

Olay atama ifadesi bir değer vermez. Bu nedenle, bir olay atama ifadesi yalnızca bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) bağlamında geçerlidir.

## <a name="expression"></a>İfade

Bir *ifade* *non_assignment_expression* veya *atamadır*.

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

Sabit bir ifade, `null` sabit değeri veya şu türlerden birine sahip bir değer olmalıdır: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `object`, `string`veya herhangi bir numaralandırma türü. Sabit ifadelerde yalnızca aşağıdaki yapılara izin verilir:

*  Değişmez değerler (`null` değişmez değeri dahil).
*  Sınıf ve yapı türlerinin `const` üyelerine başvurular.
*  Numaralandırma türlerinin üyelerine başvurular.
*  `const` parametrelere veya yerel değişkenlere başvurular
*  Kendi sabit ifadeleri olan parantezli alt ifadeler.
*  Hedef türü yukarıda listelenen türlerden biri olarak verilen atama ifadeleri.
*  `checked` ve `unchecked` ifadeleri
*  Varsayılan değer ifadeleri
*  NameOf ifadeleri
*  Önceden tanımlanmış `+`, `-`, `!`ve `~` Birli İşleçler.
*  Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`ve `>=` ikili işleçleri, her işlenen yukarıda listelenen bir tür.
*  `?:` koşullu işleç.

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

Sabit bir ifade bir `unchecked` bağlamına açıkça yerleştirilmediği için, ifadenin derleme zamanı değerlendirmesi sırasında integral türü aritmetik işlemlerde ve dönüştürmelerde oluşan taşmalar her zaman derleme zamanı hatalarına neden olur ([sabit ifadeler](expressions.md#constant-expressions)).

Sabit ifadeler aşağıda listelenen bağlamlarda oluşur. Bu bağlamlarda, bir ifade derleme zamanında tam olarak değerlendirilemez bir derleme zamanı hatası oluşur.

*  Sabit bildirimler ([sabitler](classes.md#constants)).
*  Numaralandırma üyesi bildirimleri ([enum üyeleri](enums.md#enum-members)).
*  Biçimsel parametre listelerinin varsayılan bağımsız değişkenleri ([Yöntem parametreleri](classes.md#method-parameters))
*  `switch` deyimin etiketleri `case` ([Switch bildirisi](statements.md#the-switch-statement)).
*  `goto case` deyimleri ([goto deyimi](statements.md#the-goto-statement)).
*  Bir Başlatıcı içeren bir dizi oluşturma ifadesinde ([dizi oluşturma ifadelerinde](expressions.md#array-creation-expressions)) boyut uzunlukları.
*  Öznitelikler ([öznitelikler](attributes.md)).

Örtük bir sabit ifade dönüştürmesi ([örtük sabit ifade dönüştürmeleri](conversions.md#implicit-constant-expression-conversions)), `int` türünde sabit bir ifadenin `sbyte`, `byte`, `short`, `ushort`, `uint`veya `ulong`olarak dönüştürülmesini sağlar, ancak sabit ifadenin değeri hedef türü aralığı içinde olur.

## <a name="boolean-expressions"></a>Boole ifadeleri

*Boolean_expression* , `bool`türünde bir sonuç veren bir ifadedir; Aşağıda belirtildiği gibi, belirli bağlamlarda doğrudan veya `operator true` uygulaması aracılığıyla.

```antlr
boolean_expression
    : expression
    ;
```

Bir *if_statement* ([IF deyimi](statements.md#the-if-statement)), *while_statement* ([while](statements.md#the-while-statement)deyimi), *do_statement* ([Do deyimi](statements.md#the-do-statement)) veya *for_statement* ([for deyimi](statements.md#the-for-statement)) denetim koşullu ifadesi bir *Boolean_expression*. `?:` işlecinin ([koşullu operatör](expressions.md#conditional-operator)) denetim koşullu ifadesi, bir *Boolean_expression*aynı kurallara uyar, ancak işleç önceliğin nedenleri bir *conditional_or_expression*olarak sınıflandırıldı.

Bir *boolean_expression* `E`, aşağıdaki gibi `bool`türünde bir değer üretebilmek için gereklidir:

*  `E` örtük olarak dönüştürülebilir bir `bool`, örtük dönüştürme uygulanmış çalışma zamanında.
*  Aksi halde, tek işleç aşırı yükleme çözümlemesi ([birli operatör aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)), `E``true` işlecinin benzersiz bir en iyi uygulamasını bulmak için kullanılır ve bu uygulama çalışma zamanında uygulanır.
*  Böyle bir işleç bulunmazsa, bir bağlama zamanı hatası oluşur.

[Veritabanı Boole türünde](structs.md#database-boolean-type) `DBBool` struct türü, `operator true` ve `operator false`uygulayan bir türe örnek sağlar.
