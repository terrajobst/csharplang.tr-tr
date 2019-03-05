---
ms.openlocfilehash: 67019511d49a786a5d6edf6fea442f745fc40f3f
ms.sourcegitcommit: 0a80f26b8e455c4f09843a10e11e29c24d2d922e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57347280"
---
# <a name="expressions"></a>İfadeler

Bir ifade, işleçler ve işlenenler dizisidir. Bu bölümde sözdizimi, değerlendirme işleçler ve işlenenler ve ifadeler anlamını sırasını tanımlar.

## <a name="expression-classifications"></a>İfade sınıflandırmaları

Bir ifade aşağıdakilerden biri sınıflandırıldığını:

*  Bir değer. Her değer, ilişkili bir türe sahip.
*  Bir değişken. Her değişken, ilişkili bir türü, yani değişkeninin bildirilen türü vardır.
*  Bir ad alanı. Bu sınıflandırmada bir ifade yalnızca sol tarafında görünebilir bir *member_access* ([üye erişimi](expressions.md#member-access)). Diğer herhangi bir bağlam içinde bir ad alanı olarak sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.
*  Bir tür. Bu sınıflandırmada bir ifade yalnızca sol tarafında görünebilir bir *member_access* ([üye erişimi](expressions.md#member-access)), veya bir işleneni olarak `as` işleci ([işleci ](expressions.md#the-as-operator)), `is` işleci ([işleci](expressions.md#the-is-operator)), veya `typeof` işleci ([typeof işleci](expressions.md#the-typeof-operator)). Diğer herhangi bir bağlam içinde bir tür olarak sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.
*  Bir üye aramasından kaynaklanan aşırı yüklenmiş yöntemler kümesi bir yöntem grubu ([üye araması](expressions.md#member-lookup)). Bir yöntem grubu, ilişkilendirilmiş ifadenin ve ilişkili tür bağımsız değişken listesi olabilir. Bir örnek yöntemi çağrıldığında, örnek ifade değerlendirme sonucu örneği tarafından temsil edilen olur `this` ([bu erişim](expressions.md#this-access)). Bir yöntem grubu içinde izin verilen bir *invocation_expression* ([çağrı ifadeleri](expressions.md#invocation-expressions)), *delegate_creation_expression* ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)) ve sol tarafında bir işleç ve bir uyumlu temsilci türüne örtük olarak dönüştürülebilir ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)). Diğer herhangi bir bağlam içinde bir yöntem grubu sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.
*  Bir null sabit değer. Bu sınıflandırmada bir ifade bir başvuru türüyle veya Null olabilen türü örtük olarak dönüştürülebilir.
*  Anonim bir işlevdir. Bu sınıflandırmada bir ifade bir uyumlu temsilci türü ya da ifade ağaç türüne örtük olarak dönüştürülebilir.
*  Özellik erişimi. Yani özellik türünün ilişkili bir türü her özellik erişebilir. Ayrıca, bir özellik erişimi ilişkili örnek bir ifade olabilir. Erişimci olduğunda ( `get` veya `set` blok) örneğinin özellik erişimi çağrılır, örnek ifade değerlendirme sonucu örneği tarafından temsil edilen olur `this` ([bu erişim](expressions.md#this-access)).
*  Bir olay erişim. İlişkili bir türü, yani olay türünü her olay erişebilir. Ayrıca, bir olay erişimini ilişkili örnek bir ifade olabilir. Olay erişimini'nın sol işlenen olarak görünebilir `+=` ve `-=` işleçleri ([olay ataması](expressions.md#event-assignment)). Diğer herhangi bir bağlam içinde bir olay erişimini sınıflandırılmış bir ifade, bir derleme zamanı hatasına neden olur.
*  Bir dizin oluşturucu erişim. İlişkili bir türü, yani öğe türü Indexer'ın her dizin oluşturucu erişebilir. Ayrıca, bir dizin oluşturucu erişim ilişkilendirilmiş ifade ve ilişkili bağımsız değişken listesi vardır. Erişimci olduğunda ( `get` veya `set` blok) bir dizin oluşturucu erişim çağrılır, örnek ifade değerlendirme sonucu örneği tarafından temsil edilen olur `this` ([bu erişim](expressions.md#this-access)), sonucu bağımsız değişken listesi değerlendirme çağırmanın parametre listesi olur.
*  Bir şey yok. İfade bir yöntem çağrısından dönüş türüne sahip olduğunda bu gerçekleşir `void`. Hiçbir şey yalnızca bağlamında geçerli olarak sınıflandırılmış bir ifade bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)).

Bir ifade sonucunu hiçbir zaman ad alanı, tür, yöntem grubuna veya olay erişimini ' dir. Bunun yerine, yukarıda belirtildiği gibi bu kategorileri ifadelerin yalnızca belirli bağlamlarda izin verilen ara yapılarıdır.

Bir özellik erişimi veya dizin oluşturucu erişim her zaman bir değer olarak, bir çağrı gerçekleştirerek yeniden sınıflandırılması *get erişimcisine* veya *set erişimcisine*. Belirli bir erişimci özelliğin veya dizin oluşturucu erişim bağlamında tarafından belirlenir: Erişim atama, hedef ise *set erişimcisine* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)). Aksi takdirde, *get erişimcisine* geçerli değerini almak için çağrılır ([ifadelerin değerleri](expressions.md#values-of-expressions)).

### <a name="values-of-expressions"></a>İfadelerin değerleri

Bir ifade içeren yapıları çoğunu nihai olarak belirtmek için ifadeyi gerektiren bir ***değer***. Gerçek ifade bir ad alanı, bir tür, bir yöntem grubu veya hiçbir şey gösteriyorsa bu gibi durumlarda, bir derleme zamanı hatası oluşur. Ancak, ifade, özellik erişimi, bir dizin oluşturucu erişim veya bir değişken gösteriyorsa, özelliği, dizin oluşturucu veya değişken değeri örtük olarak değiştirilir:

*  Yalnızca şu anda değişkeni tarafından tanımlanan depolama konumunda depolanan değeri bir değişkene değeridir. Bir değişken kesinlikle atanan dikkate alınmalıdır ([belirli atama onayına](variables.md#definite-assignment)) önce değeri alınabilir veya aksi halde bir derleme zamanı hatası oluşur.
*  Bir özellik erişim ifadesinin değerine çağırarak elde edilen *get erişimcisine* özelliği. Özellik Hayır varsa *get erişimcisine*, bir derleme zamanı hatası oluşur. Aksi takdirde, işlev üyesi çağırma ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) gerçekleştirilir, ve çağrısının sonucunu özellik erişim ifadesinin değeri olur.
*  Bir dizin oluşturucu erişim ifadesinin değerini çağırarak elde *get erişimcisine* dizin oluşturucu. Dizin Oluşturucu Hayır varsa *get erişimcisine*, bir derleme zamanı hatası oluşur. Aksi takdirde, bir işlev üye çağrısı ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) bağımsız değişkeniyle gerçekleştirilir listesi výraz indexeru erişim ile ilişkili ve bu değeri çağrısının sonucunu olur Dizin Oluşturucu erişim ifadesi.

## <a name="static-and-dynamic-binding"></a>Statik ve dinamik bağlama

Tür veya bağlı ifadeleri (bağımsız değişkenler, işlenen, Alıcılar) değerini temel alan bir işlem anlamını belirleme işlemi, genellikle olarak adlandırılır ***bağlama***. Örneğin bir yöntem çağrısının anlamı alıcı ve bağımsız değişkenler türüne göre belirlenir. Bir işleç anlamını işlenenleri türüne göre belirlenir.

C# anlamı bir işlem, genellikle belirlenir derleme zamanında, bağlı alt ifadeler, derleme zamanı türüne dayalı. Benzer şekilde, bir ifade bir hata varsa hata algılandı ve derleyici tarafından bildirilen. Bu yaklaşım olarak da bilinen ***statik bağlama***.

Ancak, bir ifade dinamik bir ifadeyi ise (yani türüne sahip `dynamic`) bu yer aldığı herhangi bir bağlamayı çalışma zamanı türünü (yani gerçek nesnenin türü çalışma zamanında gösterir) üzerinde bulunan, türü yerine dayalı olmaması gerektiğini gösterir. derleme zamanı. Bu tür bir işlem bağlaması, bu nedenle işlem programın çalıştırılması sırasında yürütülmek üzere olduğu zamana kadar ertelenir. Bu olarak adlandırılır ***dinamik bağlama***.

Bir işlemi dinamik olarak bağlı olduğunda, çok az kayıpla veya hiç denetimi derleyici tarafından gerçekleştirilir. Bunun yerine çalışma zamanı bağlama başarısız olursa, hataları çalışma zamanında özel durumlar olarak raporlanır.

Aşağıdaki C# işlemlerinde tabi bağlanır:

*  Üye erişimi: `e.M`
*  Yöntem çağırma: `e.M(e1, ..., eN)`
*  Temsilci çağrısı:`e(e1, ..., eN)`
*  Öğe erişimi: `e[e1, ..., eN]`
*  Nesne oluşturma: `new C(e1, ..., eN)`
*  Birli işleçler aşırı: `+`, `-`, `!`, `~`, `++`, `--`, `true`, `false`
*  İkili işleçler aşırı: `+`, `-`, `*`, `/`, `%`, `&`, `&&`, `|`, `||`, `??`, `^`, `<<` , `>>`, `==`,`!=`, `>`, `<`, `>=`, `<=`
*  Atama İşleçleri: `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`
*  Örtük ve açık dönüştürmeler

Hiçbir dinamik ifadeler söz konusu olduğunda, C# derleme zamanı türlerini bağlı ifadeleri seçim işleminde kullanıldığı anlamına gelir statik bağlama varsayılan kullanır. Yukarıda listelenen işlemlerinde bağlı ifadelerden biri dinamik bir ifadeyi olduğunda, ancak işlemi yerine dinamik olarak bağlı.

### <a name="binding-time"></a>Bağlama zamanı

Çalışma zamanında dinamik bağlama gerçekleşmeden ise statik bağlama derleme zamanında, gerçekleşir. Aşağıdaki bölümlerde terimi ***bağlama zamanı*** derleme zamanı veya çalışma bağlama ne zaman yer aldığı bağlı olarak süresi, ifade eder.

Aşağıdaki örnek, statik ve dinamik bağlama ve bağlama zamanı kavramları göstermektedir:
```csharp
object  o = 5;
dynamic d = 5;

Console.WriteLine(5);  // static  binding to Console.WriteLine(int)
Console.WriteLine(o);  // static  binding to Console.WriteLine(object)
Console.WriteLine(d);  // dynamic binding to Console.WriteLine(int)
```

İlk iki statik olarak bağlı: aşırı yüklemesini `Console.WriteLine` kendi bağımsız değişkenin derleme zamanı türüne göre seçilir. Bu nedenle, derleme zamanı bağlama zamanı.

Üçüncü çağrı dinamik olarak bağlı: aşırı yüklemesini `Console.WriteLine` bağımsız değişkeninin çalışma zamanı türüne göre seçilir. Derleme zamanı türü bildiğinizdir dinamik bir ifadeyi bağımsız değişken olduğu için böyle `dynamic`. Bu nedenle, üçüncü çağrı için bağlama süresi, çalışma zamanı.

### <a name="dynamic-binding"></a>Dinamik bağlama

Dinamik bağlama amacı ile etkileşim kurmak C# programları izin vermektir ***dinamik nesneler***, yani normal C# kurallarına izlemeyin nesneleri tür sistemi. Dinamik nesneler nesnelerden farklı sistemleri ile başka bir programlama dili olabilir veya program aracılığıyla farklı işlemler için kendi bağlama semantiği uygulamak için Kurulum olan nesneler olması olabilir.

Dinamik Nesne kendi semantiği uygulayan mekanizması tanımlanan uygulamasıdır. Belirli bir arabirim--yeniden tanımlanan uygulama--C# çalışma zamanı için özel semantiğine sahip olduğunu göstermek için dinamik nesneler tarafından uygulanır. Dinamik bir nesne üzerinde işlemler dinamik olarak bağlı olduğunda, bu nedenle, kendi bağlama semantiği yerine C# bu belgede belirtildiği gibi devralır.

Dinamik nesnelerle birlikte çalışabilirlik izin vermek için dinamik bağlama amacı olmakla birlikte, dinamik olsalar da olmadığını C# dinamik bağlamanın tüm nesneler üzerinde sağlar. Bu, üzerinde işlemler sonuçlarını değil kendilerini dinamik nesneler olabilir, ancak yine de programcıları derleme zamanında bilinmeyen bir tür ilgilidir dinamik Nesne için daha sorunsuz bir tümleştirme sağlar. Ayrıca Dinamik bağlama ilgili hiçbir nesne dinamik nesneler olsa bile hata yapmaya açık yansıma tabanlı kodun ortadan kaldırılmasına yardımcı olur.

Tam olarak tüm--uygulanır, dinamik bağlama, ne zaman denetleme--derleme uygulandığında ve hangi derleme zamanı sonucu ve ifade sınıflandırma olan dilde her yapı için aşağıdaki bölümlerde açıklanmaktadır.

### <a name="types-of-constituent-expressions"></a>Bağlı ifade türleri

Bir işlem statik olarak bağlı olduğunda (örneğin bir alıcı, bağımsız değişken, bir dizin veya bir işlenen) bağlı bir ifadenin türü her zaman bu ifade, derleme zamanı türünde kabul edilir.

Bir işlemi dinamik olarak bağlı olduğunda, bağlı bir ifadenin türü oluşturan ifadenin derleme zamanı türüne bağlı olarak farklı şekilde belirlenir:

*  Derleme zamanı türündeki bağlı bir ifade `dynamic` ifade için çalışma zamanında değerlendirilir. gerçek değer türü olduğu kabul edilir
*  Derleme zamanı türü olan bir tür parametresine bağlı bir ifade çalışma zamanında tür parametresi bağlı türe sahip olduğu kabul edildiği
*  Aksi takdirde bağlı ifadesi, derleme zamanı türü olduğu kabul edilir.

## <a name="operators"></a>İşleçler

İfadeleri oluşturulmuş ***işlenenler*** ve ***işleçleri***. İfade işleçleri işlenenlere uygulamak için hangi işlemleri gösterir. İşleçler örnekler `+`, `-`, `*`, `/`, ve `new`. Değişmez değerler, alanlar, yerel değişkenleri ve ifadeleri işlenenler örnekleridir.

Üç tür işleç vardır:

*  Birli işleçler. Birli işleçleri, tek bir işlenen alır ve iki önek gösterimini kullanın (gibi `--x`) veya sonek gösteriminde (gibi `x++`).
*  İkili işleçler. İkili işleçler iki işlenen alır ve tüm içtakı gösterimi kullanın (gibi `x + y`).
*  Üçlü işleci. Yalnızca bir Üçlü işleç `?:`, var; üç işlenen alır ve infix gösterim kullanır (`c ? x : y`).

Bir ifadede, işleçlerin değerlendirilme sırasını tarafından belirlenen ***öncelik*** ve ***ilişkilendirilebilirliği*** işleçlerinin ([İşleç önceliği ve ilişkiselliği](expressions.md#operator-precedence-and-associativity)) .

İfadelerde işlenenlerin soldan sağa doğru değerlendirilir. Örneğin, `F(i) + G(i++) * H(i)`, yöntemi `F` eski değeri kullanılarak çağrılacak `i`, ardından yöntemi `G` eski değeriyle adlı `i`ve son olarak, yöntemi `H` Yenideğerleçağrılır`i`. İşleç önceliği ayrıdır ve ilgisiz budur.

Belirli işleçleri olabilir ***aşırı***. İşleç aşırı yüklemesi bir yerde işlemleri için belirtilen kullanıcı tanımlı işleç uygulamalarına izin verir ya da her iki işlenen kullanıcı tanımlı sınıf veya yapı türü ([işleci aşırı yüklemesi](expressions.md#operator-overloading)).

### <a name="operator-precedence-and-associativity"></a>İşleç önceliği ve ilişkiselliği

Bir ifade birden çok işleç içeren ***öncelik*** işleçleri tek tek işleçler değerlendirilme sırası denetler. Örneğin, ifade `x + y * z` değerlendirmesinde `x + (y * z)` çünkü `*` işleci, ikili daha yüksek bir önceliğe sahip `+` işleci. Bir işleç önceliği, ilişkili dilbilgisi üretim tanımına göre belirlenir. Örneğin, bir *additive_expression* oluşan bir dizi *multiplicative_expression*s ayırarak `+` veya `-` işleçleri, bu nedenle vererek `+` ve `-` işleçleri daha düşük önceliğe `*`, `/`, ve `%` işleçleri.

Tüm işleçler sırayla önceliği en yüksekten en düşüğe aşağıdaki tabloda özetlenmiştir:

| __Bölüm__                                                                                   | __Kategori__                | __İşleçler__ | 
|-----------------------------------------------------------------------------------------------|-----------------------------|---------------|
| [Birincil ifadeler](expressions.md#primary-expressions)                                     | Birincil                     | `x.y`  `f(x)`  `a[x]`  `x++`  `x--`  `new`  `typeof`  `default`  `checked`  `unchecked`  `delegate` | 
| [Birli işleçler](expressions.md#unary-operators)                                             | Birli                       | `+`  `-`  `!`  `~`  `++x`  `--x`  `(T)x` | 
| [Aritmetik işleçler](expressions.md#arithmetic-operators)                                   | Çarpma              | `*`  `/`  `%` | 
| [Aritmetik işleçler](expressions.md#arithmetic-operators)                                   | Eklenebilir                    | `+`  `-`      | 
| [Kaydırma işleçleri](expressions.md#shift-operators)                                             | Shift                       | `<<`  `>>`    | 
| [İlişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators) | İlişkisel ve tür testi | `<`  `>`  `<=`  `>=`  `is`  `as` | 
| [İlişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators) | Eşitlik                    | `==`  `!=`    | 
| [Mantıksal işleçler](expressions.md#logical-operators)                                         | Mantıksal VE                 | `&`           | 
| [Mantıksal işleçler](expressions.md#logical-operators)                                         | Mantıksal XOR                 | `^`           | 
| [Mantıksal işleçler](expressions.md#logical-operators)                                         | Mantıksal VEYA                  | <code>&#124;</code>           |
| [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)                 | Koşullu VE             | `&&`          | 
| [Koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)                 | Koşullu VEYA              | <code>&#124;&#124;</code>          | 
| [Null birleşim işleci](expressions.md#the-null-coalescing-operator)                   | Null birleşim             | `??`          | 
| [Koşullu işleç](expressions.md#conditional-operator)                                   | Koşullu                 | `?:`          | 
| [Atama işleçleri](expressions.md#assignment-operators), [anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)  | Atama ve lambda ifadesi | `=`  `*=`  `/=`  `%=`  `+=`  `-=`  `<<=`  `>>=`  `&=`  `^=`  <code>&#124;=</code>  `=>` | 

Aynı önceliğe sahip iki işleç arasında bir işlenen ortaya çıktığında, işleçlerin ilişkiselliği işlemlerinin gerçekleştirildiği sırayı denetler:

*  Atama işleçleri ve null birleşim işleci hariç tüm ikili işleçler şunlardır: ***ilişkilendirilebilir***, yani işlemler soldan sağa doğru gerçekleştirilir. Örneğin, `x + y + z` değerlendirmesinde `(x + y) + z`.
*  Atama İşleçleri, null birleşim işleci ve koşullu işleç (`?:`) olan ***sağla ilişkilendirilebilir***, sağdan sola işlemler gerçekleştirilir anlamına gelir. Örneğin, `x = y = z` değerlendirmesinde `x = (y = z)`.

Öncelik ve ilişkisellik parantez kullanılarak denetlenebilir. Örneğin, `x + y * z` ilk çarpar `y` tarafından `z` ve ardından sonuca ekler `x`, ancak `(x + y) * z` ilk ekler `x` ve `y` ve sonucu çarpan `z`.

### <a name="operator-overloading"></a>İşleç aşırı yüklemesi

Tüm birli ve ikili işleçler, herhangi bir ifade içinde otomatik olarak kullanılabilir uygulamalar önceden tanımlanmış. Önceden tanımlanmış uygulamalarının yanı sıra, kullanıcı tanımlı uygulamaları dahil ederek tanıtılabilir `operator` bildirimlerinde sınıflar ve yapılar ([işleçleri](classes.md#operators)). Kullanıcı tanımlı işleç uygulamalarına her zaman önceden tanımlanmış işleç uygulamalarına öncelikli: Hiçbir geçerli kullanıcı tanımlı işleç uygulamalarına varken önceden tanımlanmış işleç uygulamalarına, açıklanan şekilde değerlendirilmesi yalnızca [birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili İşleç aşırı yüklemesi Çözüm](expressions.md#binary-operator-overload-resolution).

***Aşırı yüklenebilir birli işleçler*** şunlardır:
```csharp
+   -   !   ~   ++   --   true   false
```

Ancak `true` ve `false` ifadelerinde açıkça kullanılmaz (ve bu nedenle öncelik tablosunda bulunmayan [İşleç önceliği ve ilişkiselliği](expressions.md#operator-precedence-and-associativity)), çünkü bunlar işleçler değerlendirilir birden fazla ifade bağlamlarda çağrılır: boolean ifadeleri ([Boolean ifadeler](expressions.md#boolean-expressions)) ve koşul içeren ifadeler ([koşullu işleç](expressions.md#conditional-operator)) ve koşullu mantıksal işleçler ([koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)).

***Aşırı yüklenebilir ikili işleçler*** şunlardır:
```csharp
+   -   *   /   %   &   |   ^   <<   >>   ==   !=   >   <   >=   <=
```

Yalnızca yukarıda listelenen işleçleri aşırı yüklenebilir. Özellikle, üye erişimi, yöntem çağırma aşırı mümkün değildir veya `=`, `&&`, `||`, `??`, `?:`, `=>`, `checked`, `unchecked`, `new`, `typeof`, `default`, `as`, ve `is` işleçleri.

İkili İşleç aşırı karşılık gelen atama işleci, varsa, aynı zamanda örtük olarak aşırı yüklenmiş olur. Örneğin, bir işleç aşırı `*` aynı zamanda bir işleç aşırı yüklemesi olan `*=`. Daha ayrıntılı açıklanır budur [bileşik atama](expressions.md#compound-assignment). Unutmayın atama işleci (`=`) aşırı yüklenemez. Atama her zaman bir değişkene bir değer Bitsel basit kopyasını gerçekleştirir.

İşlemleri gibi cast `(T)x`, kullanıcı tanımlı dönüşümler sağlayarak aşırı ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)).

Öğe erişimi, gibi `a[x]`, bir aşırı yüklenebilir işleç olarak kabul edilmez. Kullanıcı tanımlı dizin dizin oluşturucular bunun yerine, desteklenir ([dizin oluşturucular](classes.md#indexers)).

İfadeler, işleçleri, işleç gösterimi kullanılarak başvurulur ve bildirimlerinde, işleçler işlevsel bir gösterim kullanılarak başvurulur. Aşağıdaki tabloda, işleci ve tekli veya ikili işleçler için işlevsel gösterimler arasındaki ilişkiyi gösterir. İlk giriş *op* hiçbir aşırı yüklenebilir birli önek işleci gösterir. İkinci giriş *op* birli sonek gösterir `++` ve `--` işleçleri. Üçüncü giriş *op* hiçbir aşırı yüklenebilir ikili işleç gösterir.


| __İşleç gösterimi__ | __İşlevsel bir gösterim__ |
|-----------------------|-------------------------|
| `op x`                | `operator op(x)`        | 
| `x op`                | `operator op(x)`        | 
| `x op y`              | `operator op(x,y)`      | 

Kullanıcı tanımlı işleç bildirimleri her zaman en az bir parametre işleç bildirimi içeren sınıf veya yapı türü olmasını gerektirir. Bu nedenle, önceden tanımlanmış işleç aynı imzaya sahip bir kullanıcı tanımlı işleç için mümkün değildir.

Kullanıcı tanımlı işleç bildirimi sözdizimi, öncelik veya bir işleç ilişkilendirilebilirliği değiştiremezsiniz. Örneğin, `/` ikili işleç, her zaman sahip öncelik düzeyi belirtilen işleci her zaman, [İşleç önceliği ve ilişkiselliği](expressions.md#operator-precedence-and-associativity)ve her zaman ilişkilendirilebilir.

Bunu pleases herhangi bir hesaplama gerçekleştirmek bir kullanıcı tanımlı işleç için mümkün olsa da, sonuçlar sezgisel beklenen dışındaki uygulamalar kesinlikle önerilmez. Örneğin, uygulaması `operator ==` ve eşitlik için iki işlenenden karşılaştırma uygun bir dönüş `bool` sonucu.

Tek tek işleçler açıklamalarını [birincil ifadeler](expressions.md#primary-expressions) aracılığıyla [koşullu mantıksal işleçler](expressions.md#conditional-logical-operators) önceden tanımlanmış uygulamalar işleçler ve geçerli tüm ek kuralları belirtin her işleç için. Açıklamaları olun kullanım koşulları ***birli işleç aşırı yükleme çözünürlüğü***, ***ikili işleci aşırı yükleme çözünürlüğü***, ve ***sayısal yükseltme***, bir işlem olan tanımları Aşağıdaki bölümlerde bulundu.

### <a name="unary-operator-overload-resolution"></a>Birli işleç aşırı yükleme çözünürlüğü

Bir işlem formun `op x` veya `x op`burada `op` bir aşırı yüklenebilir birli işleç ve `x` türündeki bir ifade `X`, şu şekilde işlenir:

*  Aday kullanıcı tanımlı işleçler tarafından sağlanan dizi `X` işlemi için `operator op(x)` kurallarını kullanarak belirlenir [aday kullanıcı tanımlı işleçler](expressions.md#candidate-user-defined-operators).
*  Aday kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçleri kümesini olur. Aksi takdirde, önceden tanımlanmış birli `operator op` lifted formlarına dahil olmak üzere uygulamalar, işlem için aday işleçleri kümesini haline gelir. Önceden tanımlanmış uygulamalar belirli bir işlecin işleci açıklamasında belirtilen ([birincil ifadeler](expressions.md#primary-expressions) ve [birli işleçler](expressions.md#unary-operators)).
*  Aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution) bağımsız değişken listesi ile ilgili en iyi operatör seçmek için aday işleçleri kümesini uygulanan `(x)`, ve bu işleci aşırı yükleme sonucu haline gelir. Çözümleme işlemi. Tek bir en iyi operatör seçmek aşırı yükleme çözümlemesi başarısız olursa bir bağlama zamanı hatası oluşur.

### <a name="binary-operator-overload-resolution"></a>İkili İşleç aşırı yükleme çözünürlüğü

Bir işlem formun `x op y`burada `op` bir aşırı yüklenebilir ikili işleç `x` türündeki bir ifade `X`, ve `y` türündeki bir ifade `Y`, şu şekilde işlenir:

*  Aday kullanıcı tanımlı işleçler tarafından sağlanan dizi `X` ve `Y` işlemi için `operator op(x,y)` belirlenir. Tarafından sağlanan aday işleçleri birleşimi kümesi oluşan `X` ve tarafından sağlanan aday işleçleri `Y`, her belirlenen kurallarını kullanarak [aday kullanıcı tanımlı işleçler](expressions.md#candidate-user-defined-operators). Varsa `X` ve `Y` aynı türde olan veya `X` ve `Y` paylaşılan aday işleçler yalnızca Birleşik kümesine defa geçebildiği sonra ortak bir taban türünden türetilir.
*  Aday kullanıcı tanımlı işleçler kümesi boş değilse, bu işlem için aday işleçleri kümesini olur. Aksi takdirde, önceden tanımlanmış ikili `operator op` lifted formlarına dahil olmak üzere uygulamalar, işlem için aday işleçleri kümesini haline gelir. Önceden tanımlanmış uygulamalar belirli bir işlecin işleci açıklamasında belirtilen ([aritmetik işleçler](expressions.md#arithmetic-operators) aracılığıyla [koşullu mantıksal işleçler](expressions.md#conditional-logical-operators)). Önceden tanımlanmış sabit listesi ve temsilci işleçleri için kabul yalnızca biri bağlama zamanı türü olan bir enum veya temsilci türüne göre tanımlanan işleçleridir.
*  Aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution) bağımsız değişken listesi ile ilgili en iyi operatör seçmek için aday işleçleri kümesini uygulanan `(x,y)`, ve bu işleci aşırı yükleme sonucu haline gelir. Çözümleme işlemi. Tek bir en iyi operatör seçmek aşırı yükleme çözümlemesi başarısız olursa bir bağlama zamanı hatası oluşur.

### <a name="candidate-user-defined-operators"></a>Aday kullanıcı tanımlı işleçler

Belirli bir türe `T` ve bir işlem `operator op(A)`burada `op` aşırı yüklenebilir bir işleç ve `A` aday kümesini bir bağımsız değişken listesi tarafından sağlanan kullanıcı tanımlı işleçler olan `T` için `operator op(A)` belirlenir şu şekilde:

*  Tür belirleme `T0`. Varsa `T` boş değer atanabilir bir tür `T0` Aksi takdirde, temelindeki türe olan `T0` eşittir `T`.
*  Tüm `operator op` bildirimlerinde `T0` ve tüm yükseltilmiş böyle işleçlerinin forms en az bir işleç uygulanabilir olduğunda ([geçerli işlev üyesi](expressions.md#applicable-function-member)) bağımsız değişken listesine göre `A`, ardından kümesi Aday işleçler gibi ilgili tüm işleçler oluşur `T0`.
*  Aksi takdirde `T0` olduğu `object`, aday işleçleri kümesini boştur.
*  Aksi takdirde, aday işleçleri kümesini sağlayan `T0` doğrudan taban sınıfı tarafından sağlanan aday işleçleri kümesi `T0`, veya etkili temel sınıfını `T0` varsa `T0` parametre türüdür.

### <a name="numeric-promotions"></a>Sayısal promosyonlar

Sayısal promosyon, belirli örtük dönüştürmeleri önceden tanımlı tekli veya ikili sayısal işleçler işlenenlerin gerçekleştirme otomatik olarak oluşur. Sayısal yükseltme ayrı bir mekanizma, ancak önceden tanımlı operatörler için aşırı yükleme çözünürlüğü uygulanması yerine bir etkin değil. Kullanıcı tanımlı işleçler benzer etkileri göstermesi için uygulanabilir ancak sayısal yükseltme değerlendirmesi kullanıcı tanımlı işleçler, özellikle etkilemez.

Sayısal yükseltme örnek olarak, önceden tanımlanmış uygulamalar ikili göz önünde bulundurun `*` işleci:

```csharp
int operator *(int x, int y);
uint operator *(uint x, uint y);
long operator *(long x, long y);
ulong operator *(ulong x, ulong y);
float operator *(float x, float y);
double operator *(double x, double y);
decimal operator *(decimal x, decimal y);
```

Ne zaman çözümleme kurallarını aşırı yükleme ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)) bu kümesine uygulanan operatörleri, işlenen türleri için örtük dönüştürmelerin mevcut işleçlerinin ilk seçmek için etkisidir. Örneğin, işlemi için `b * s`burada `b` olduğu bir `byte` ve `s` olduğu bir `short`, aşırı yükleme çözümlemesi seçer `operator *(int,int)` en iyi işleci olarak. Bu nedenle, etkin olan `b` ve `s` dönüştürülür `int`, ve sonuç türü `int`. Benzer şekilde, işlem için `i * d`burada `i` olduğu bir `int` ve `d` olduğu bir `double`, aşırı yükleme çözümlemesi seçer `operator *(double,double)` en iyi işleci olarak.

#### <a name="unary-numeric-promotions"></a>Birli sayısal promosyonlar

Önceden tanımlanmış işlenenler için sayısal yükseltmesi Unary gerçekleşir `+`, `-`, ve `~` birli işleçler. Yalnızca sayısal yükseltmesi Unary oluşur türündeki işlenenler dönüştürme `sbyte`, `byte`, `short`, `ushort`, veya `char` türüne `int`. Ayrıca, birli için `-` işleci, sayısal yükseltmesi unary dönüştürür türündeki işlenenler `uint` türüne `long`.

#### <a name="binary-numeric-promotions"></a>İkili sayısal promosyonlar

Önceden tanımlanmış işlenenler için ikili sayısal yükseltme gerçekleşir `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `==`, `!=`, `>`, `<`, `>=`, ve `<=` ikili işleçler. İkili sayısal yükseltme, her iki işlenen de ilişkisel olmayan işleçleri olması durumunda, ayrıca işlemin sonuç türü olur, ortak bir türe örtük olarak dönüştürür. Burada göründükleri sırayla aşağıdaki kuralları, uygulama ikili sayısal yükseltme oluşur:

*  İki işlenenden türü ise `decimal`, diğer işlenen türüne dönüştürülür `decimal`, veya bir bağlama hatası diğer işlenen türü ise oluşur `float` veya `double`.
*  Aksi takdirde iki işlenenden türü ise `double`, diğer işlenen türüne dönüştürülür `double`.
*  Aksi takdirde iki işlenenden türü ise `float`, diğer işlenen türüne dönüştürülür `float`.
*  Aksi takdirde iki işlenenden türü ise `ulong`, diğer işlenen türüne dönüştürülür `ulong`, veya bir bağlama hatası diğer işlenen türü ise oluşur `sbyte`, `short`, `int`, veya `long`.
*  Aksi takdirde iki işlenenden türü ise `long`, diğer işlenen türüne dönüştürülür `long`.
*  Aksi takdirde iki işlenenden türü ise `uint` ve diğer işlenen türünde `sbyte`, `short`, veya `int`, her iki işlenen de türüne dönüştürülür `long`.
*  Aksi takdirde iki işlenenden türü ise `uint`, diğer işlenen türüne dönüştürülür `uint`.
*  Aksi takdirde, her iki işlenen de türüne dönüştürülür `int`.

İlk kural karıştırmak herhangi bir işlem izin vermiyor Not `decimal` tür `double` ve `float` türleri. Arasında örtük dönüştürme işlemi yok olgu gelen kuralını izler `decimal` türü ve `double` ve `float` türleri.

Ayrıca bir işlenen türünde olması mümkün olmadığını göz önünde bulundurun `ulong` imzalı bir tamsayı türünde olduğunda diğer işlenen. Neden hiçbir integral türü bulunmaktadır: tam aralığını temsil edebilir `ulong` işaretli integral türlerindeki yanı sıra.

Yukarıdaki durumların her ikisinde atama ifadesi açıkça bir işlenen diğer işlenen ile uyumlu bir türe dönüştürmek için kullanılabilir.

Örnekte
```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (1.0 + percent / 100.0);
}
```
bağlama zamanı hatası oluşur bir `decimal` çarpılmasıyla edilemez bir `double`. İkinci işleneni açıkça dönüştürerek hata çözümlenene `decimal`gibi:

```csharp
decimal AddPercent(decimal x, double percent) {
    return x * (decimal)(1.0 + percent / 100.0);
}
```

### <a name="lifted-operators"></a>Yükseltilmiş işleçleri

***İşleçler yükseltilmiş*** da bu türlerin boş değer atanabilir forms ile kullanılmak üzere atanamayan değer türleri üzerinde çalışması önceden tanımlanmış ve kullanıcı tanımlı işleçler izin verir. Yükseltilmiş işleçleri, aşağıda açıklandığı gibi belirli gereksinimleri karşılaması önceden tanımlanmış ve kullanıcı tanımlı işleçler oluşturulur:

*   Birli işleçleri

    ```csharp
    +  ++  -  --  !  ~
    ```

    Her iki değer atanamayan değer türleri işlenen ve sonuç türleri olması durumunda lifted form bir işleç var. Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi işlenen ve sonuç türleri için. İşlenen null ise lifted işlecini bir null değer oluşturur. Aksi takdirde lifted işleci, işlenen kaydırmayacağını, temel alınan işlecini uygular ve sonucu sarmalar.

*   İkili işleçler için

    ```csharp
    +  -  *  /  %  &  |  ^  <<  >>
    ```

    Tüm NULL olmayan değer türleri işlenen ve sonuç türleri olması durumunda lifted form bir işleç var. Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi her işlenen ve sonuç türü. Varsa lifted işleci bir null değer üreten veya her iki işlenen de null (bir özel durum olan `&` ve `|` operatörleri `bool?` açıklandığı yazın [Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)). Aksi takdirde, lifted işleci işlenenler kaydırmayacağını, temel alınan işleci uygulanır ve sonuç sarmalar.

*   Eşitlik işleçleri

    ```csharp
    ==  !=
    ```

    İşlenen türleri atanamayan değer türleri ve sonuç türü olup olmadığını ise lifted form bir işleç var. `bool`. Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi her işlenen türü. Lifted işleci iki null değeri eşit ve null değeri eşit olmayan herhangi bir null olmayan değer için değerlendirir. Her iki işlenen de null olmayan, lifted işleci işlenenler sarmalanmış olmaktan çıkaran ve üretmek için temel alınan işleci geçerlidir `bool` sonucu.

*   İlişkisel işleçleri

    ```csharp
    <  >  <=  >=
    ```

    İşlenen türleri atanamayan değer türleri ve sonuç türü olup olmadığını ise lifted form bir işleç var. `bool`. Tek bir ekleyerek lifted formun oluşturulan `?` değiştiricisi her işlenen türü. Değer lifted işleci üretir `false` bir veya iki işlenenin null olduğunda. Aksi takdirde lifted işleci işlenenler sarmalanmış olmaktan çıkaran ve oluşturmak için temel alınan işleci geçerlidir `bool` sonucu.

## <a name="member-lookup"></a>Üye araması

Üye araması anlamı bir türün bağlamı içindeki bir adın yapabildiği belirlenir işlemidir. Değerlendirme bir parçası olarak bir üye araması oluşabilir bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) içinde bir ifade. Varsa *simple_name* veya *member_access* olarak gerçekleşir *primary_expression* , bir *invocation_expression* ([ Yöntem çağrıları](expressions.md#method-invocations)), üye çağrılmasına kabul edilir.

Üye bir yöntem veya olay ise ya da bir sabit, alan veya özellik bir temsilci türü ise ([Temsilciler](delegates.md)) veya türü `dynamic` ([dinamik tür](types.md#the-dynamic-type)), üye olduğusöylenirsonra*çağrılmayan*.

Üye araması üyesi aynı zamanda üyesinin tür parametreleri ve üye erişilebilir olup yalnızca adını göz önünde bulundurur. Üye araması amacıyla, genel yöntemleri ve iç içe geçmiş genel türler dizi ilgili bildirimleri belirtilen tür parametreleri ve diğer tüm üyeleri sıfır Tür parametrelerine sahip.

Adı bir üye araması `N` ile `K`  tür parametrelerindeki tür `T` şu şekilde işlenir:

*  İlk olarak bir dizi adlı erişilebilir üyeler `N` belirlenir:
    * Varsa `T` adlı erişilebilir üyeler kümesi birleşimi kümesi ise bir tür parametresi olduğundan `N` her bir birincil kısıtlaması veya ikincil bir kısıtlama olarak belirtilen türlerle ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) için  `T`, adlı erişilebilir üyeler kümesini birlikte `N` içinde `object`.
    * Aksi takdirde, küme, tüm erişilebilir oluşur ([üye erişimi](basic-concepts.md#member-access)) adlı üye `N` içinde `T`devralınan üyeleri ve adlı erişilebilir Üyeler dahil olmak üzere `N` içinde `object`. Varsa `T` bir oluşturulmuş tür olsa üyelerin kümesini tür bağımsız değişkenleri açıklanan şekilde değiştirerek elde edilen [oluşturulan türler üyeleri](classes.md#members-of-constructed-types). İçeren üye bir `override` değiştiricisi kümeden hariç tutulur.
*  Ardından, eğer `K` sıfırsa, türleri, bildirimleri içeren tür parametreleri kaldırıldığında tüm iç içe. Varsa `K` sıfır olmayan tür parametreleri kaldırılır, farklı bir değere sahip tüm üyeleri olan. Dikkat edin `K` sıfır, yöntemleri sahip olmak parametreleri kaldırılmaz, bu yana tür çıkarımı işlem türü ([anlam çıkarma](expressions.md#type-inference)) tür bağımsız değişkenlerini çıkarsanacak mümkün olabilir.
*  Üye ise sonraki *çağrılan*, tüm olmayan-*çağrılmayan* üyeleri kümesinden kaldırılır.
*  Ardından, diğer üyeleri tarafından gizlenen üyeleri kümesinden kaldırılır. Her üye için `S.M` kümesindeki burada `S` tür, üye `M` bildirildiğinde, aşağıdaki kurallar uygulanır:
    * Varsa `M` tüm üyelerini bir temel türde bildirilen sonra sabit, alan, özelliği, olay veya numaralandırma üyesi olduğu `S` kümesinden kaldırılır.
    * Varsa `M` bir tür bildiriminde kullanıldığında, ardından tüm olmayan bir temel türde bildirilen türleri olan `S` kümesinden kaldırılır ve tür parametreleri aynı sayıda bildirimlerle type all `M` bir temel türde bildirilen `S` kaldırılır kümesinden.
    * Varsa `M` tüm yöntemi olmayan üyeleri bir temel türde bildirilen sonra bir yöntem olan `S` kümesinden kaldırılır.
*  Ardından, sınıf üyeleri tarafından gizlenen bir arabirim üyeleri kümesinden kaldırılır. Bu adım yalnızca, bir etkisi olmaz `T` parametre türüdür ve `T` hem etkin temel bir sınıf dışında olan `object` ve boş olmayan etkin arabirimi ayarlayın ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)). Her üye için `S.M` kümesindeki burada `S` hangi tür üyesi `M` bildirildiğinde, aşağıdaki kurallar uygulanır, `S` sınıf bildirimi dışında olan `object`:
    * Varsa `M` bir sabit, alan, özelliği, olay, sabit listesi üyesi veya türü bildirimi bir arabirim bildirimi içinde bildirilen tüm üyelerle kümesinden kaldırılır sonra olduğu.
    * Varsa `M` bir yöntem bir arabirim bildirimi içinde bildirilen tüm yöntemi olmayan üyeleri kümesi ve aynı imzaya sahip tüm yöntemleri kaldırılır sonra olduğu `M` bildirilen bir arabirimde bildirimi kümesinden kaldırılır.
*  Son olarak, gizli üyeleri kaldırılmış arama sonucu belirlenir:
    * Belirlenen bir metot değil tek bir üyesi oluşuyorsa, ardından bu arama sonucunu üyesidir.
    * Küme yalnızca yöntemleri içeriyorsa, aksi takdirde, ardından bu yöntemleri arama sonucu grubudur.
    * Aksi takdirde, belirsiz arama ve bir bağlama zamanı hatası oluşur.

Tür parametreleri ve arabirimler dışında türlerindeki üye aramalar ve kesinlikle tek devralımlı arabirimler üye arama için (devralma zincirini her arabirim tam olarak sıfır veya bir doğrudan temel arabirim varsa), arama kurallarını etkisi yalnızca, aynı ad veya imzaya sahip üyeler Gizle temel üyeler türetilmiş. Bu tür tek devralımlı aramaları hiçbir zaman belirsiz olabilir. Büyük olasılıkla birden çok devralma, arabirimler üye arama gelen ortaya çıkabilecek belirsizlikleri açıklanan [arabirim üye erişimi](interfaces.md#interface-member-access).

### <a name="base-types"></a>Taban türleri

Bir tür üyesi arama amacıyla `T` aşağıdaki temel türleri olduğu kabul edilir:

*  Varsa `T` olduğu `object`, ardından `T` temel tür yok.
*  Varsa `T` olduğu bir *enum_type*, temel türde `T` sınıf türleri `System.Enum`, `System.ValueType`, ve `object`.
*  Varsa `T` olduğu bir *struct_type*, temel türde `T` sınıf türleri `System.ValueType` ve `object`.
*  Varsa `T` olduğu bir *class_type*, temel türde `T` temel sınıfları, `T`, sınıf türü de dahil olmak üzere `object`.
*  Varsa `T` olduğu bir *INTERFACE_TYPE*, temel türde `T` temel arabirimleri olan `T` ve sınıf türüne `object`.
*  Varsa `T` olduğu bir *array_type*, temel türde `T` sınıf türleri `System.Array` ve `object`.
*  Varsa `T` olduğu bir *delegate_type*, temel türde `T` sınıf türleri `System.Delegate` ve `object`.

## <a name="function-members"></a>İşlev üyeleri

İşlev üyeleri yürütülebilir deyimleri içermesine üyeleridir. İşlev üyeleri her zaman türlerin üyelerini ve ad alanları üyeleri olamaz. C# işlev üyeleri aşağıdaki kategorileri tanımlar:

*  Yöntemler
*  Özellikler
*  Olaylar
*  Dizin Oluşturucular
*  Kullanıcı tanımlı işleçler
*  Örnek oluşturucuları
*  Statik oluşturucular
*  Yıkıcılar

Yok ediciler ve (hangi açıkça çağrılamaz) statik oluşturucular dışında işlev üyeleri içinde yer alan ifadeleri işlev üyesi çağrılarını yürütülür. Bir işlev üye çağrısı yazma olarak gerçek sözdizimini üzerinde belirli işlev üye kategorisine bağlıdır.

Bağımsız değişken listesi ([bağımsız değişken listeleri](expressions.md#argument-lists)) işlevi üyesi çağırma gerçek değerleri veya değişken başvuruları işlevi üyesinin parametrelerini sağlar.

Genel yöntemler çağrıları tür bağımsız değişkenleri, yönteme kümesini belirlemek için tür çıkarımı uygulayabilir. Bu işlem açıklanan [anlam çıkarma](expressions.md#type-inference).

Yöntemleri, Dizinleyicileri, işleçleri ve örnek oluşturucuları çağrılarını aşırı yükleme çözünürlüğü, aday birtakım çağrılacak işlev üyeleri belirlemek için kullanın. Bu işlem açıklanan [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution).

Belirli işlev üyesi bağlama zamanında belirlendikten sonra büyük olasılıkla aşırı yükleme çözünürlüğü işlevi üye çağrılırken gerçek çalışma zamanı açıklanan işlemidir [derleme zamanı dinamik aşırı yükleme çözünürlüğüdenetleniyor](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Açıkça çağrılan işlev üyeleri altı kategorileri içeren yapıları yerinde alan işleme aşağıdaki tabloda özetlenmiştir. Tabloda `e`, `x`, `y`, ve `value` değişkenler veya değerler olarak sınıflandırılan ifadeleri belirtmek `T` bir tür olarak sınıflandırılmış bir ifadeyi gösterir `F` basit bir yöntem ve adıdır`P` basit bir özelliğin adıdır.


| __Yapısı__     | __Örnek__    | __Açıklama__ |
|-------------------|----------------|-----------------|
| Yöntem çağırma | `F(x,y)`       | Aşırı yükleme çözünürlüğü uygulanan en iyi yöntem seçmek için `F` içeren sınıf veya yapı içinde. Yöntem bağımsız değişken listesi ile çağrılan `(x,y)`. Yöntem değilse `static`, örnek ifade `this`. | 
|                   | `T.F(x,y)`     | Aşırı yükleme çözünürlüğü uygulanan en iyi yöntem seçmek için `F` sınıf veya yapı içinde `T`. Yöntem değilse bir bağlama zamanı hatası oluşur `static`. Yöntem bağımsız değişken listesi ile çağrılan `(x,y)`. | 
|                   | `e.F(x,y)`     | Aşırı yükleme çözünürlüğü uygulanan en iyi yöntem F sınıf, yapı veya arabirim türü tarafından verilen seçmek için `e`. Yöntem ise bir bağlama zamanı hatası oluşur `static`. Örnek ifade ile yöntemi çağrılır `e` ve bağımsız değişken listesi `(x,y)`. | 
| Özellik erişimi   | `P`            | `get` Erişimci özelliğin `P` kapsayan sınıfın veya yapının çağrılır. Bir derleme zamanı hatası oluşur `P` salt yazılır. Varsa `P` değil `static`, örnek ifade `this`. | 
|                   | `P = value`    | `set` Erişimci özelliğin `P` içeren sınıf veya yapı bağımsız değişken listesiyle çağrılır `(value)`. Bir derleme zamanı hatası oluşur `P` salt okunur. Varsa `P` değil `static`, örnek ifade `this`. | 
|                   | `T.P`          | `get` Erişimci özelliğin `P` sınıf veya yapı içinde `T` çağrılır. Bir derleme zamanı hatası oluşur `P` değil `static` veya `P` salt yazılır. | 
|                   | `T.P = value`  | `set` Erişimci özelliğin `P` sınıf veya yapı içinde `T` bağımsız değişken listesi ile çağrılan `(value)`. Bir derleme zamanı hatası oluşur `P` değil `static` veya `P` salt okunur. | 
|                   | `e.P`          | `get` Erişimci özelliğin `P` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e`. Bir bağlama zamanı hatası oluşur `P` olduğu `static` veya `P` salt yazılır. | 
|                   | `e.P = value`  | `set` Erişimci özelliğin `P` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e` ve bağımsız değişken listesi `(value)`. Bir bağlama zamanı hatası oluşur `P` olduğu `static` veya `P` salt okunur. | 
| Olay erişimini      | `E += value`   | `add` Olay erişimcisi `E` kapsayan sınıfın veya yapının çağrılır. Varsa `E` olan statik, örnek ifadesidir `this`. | 
|                   | `E -= value`   | `remove` Olay erişimcisi `E` kapsayan sınıfın veya yapının çağrılır. Varsa `E` olan statik, örnek ifadesidir `this`. | 
|                   | `T.E += value` | `add` Olay erişimcisi `E` sınıf veya yapı içinde `T` çağrılır. Bir bağlama zamanı hatası oluşur `E` statik değil. | 
|                   | `T.E -= value` | `remove` Olay erişimcisi `E` sınıf veya yapı içinde `T` çağrılır. Bir bağlama zamanı hatası oluşur `E` statik değil. | 
|                   | `e.E += value` | `add` Olay erişimcisi `E` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e`. Bir bağlama zamanı hatası oluşur `E` statiktir. | 
|                   | `e.E -= value` | `remove` Olay erişimcisi `E` içinde sınıf, yapı veya arabirim türü tarafından verilen `e` örnek ifade ile çağrılan `e`. Bir bağlama zamanı hatası oluşur `E` statiktir. | 
| Dizinleyici erişimi    | `e[x,y]`       | Aşırı yükleme çözünürlüğü, dizin oluşturucunun en iyi sınıf, yapı veya arabirim e türü tarafından verilen seçmek için uygulanır. `get` Erişimci Indexer'ın örnek ifade ile çağrılan `e` ve bağımsız değişken listesi `(x,y)`. Dizin Oluşturucu salt yazılır ise bir bağlama zamanı hatası oluşur. | 
|                   | `e[x,y] = value` | Aşırı yükleme çözünürlüğü uygulanan en iyi dizin oluşturucu sınıf, yapı veya arabirim türü tarafından verilen seçmek için `e`. `set` Erişimci Indexer'ın örnek ifade ile çağrılan `e` ve bağımsız değişken listesi `(x,y,value)`. Dizin Oluşturucu salt okunur ise bir bağlama zamanı hatası oluşur. | 
| İşleç çağırma | `-x`         | Aşırı yükleme çözünürlüğü uygulanan en iyi birli işleç sınıf veya yapı türü tarafından verilen seçmek için `x`. Seçili işleç bağımsız değişken listesi ile çağrılan `(x)`. | 
|                     | `x + y`      | Aşırı yükleme çözünürlüğü sınıflar veya yapılar türleri tarafından verilen iyi ikili operatör seçmek için uygulanan `x` ve `y`. Seçili işleç bağımsız değişken listesi ile çağrılan `(x,y)`. | 
| Örnek oluşturucusu çağırma | `new T(x,y)` | Sınıf veya yapı en iyi örnek oluşturucusu seçmek için aşırı yükleme çözünürlüğü uygulanan `T`. Örnek oluşturucusu bağımsız değişken listesi ile çağrılan `(x,y)`. | 

### <a name="argument-lists"></a>Bağımsız değişken listeleri

Her işlev üyesi ve temsilci çağırma, gerçek değerleri veya değişken başvuruları işlevi üyesinin parametrelerini sağlayan bir bağımsız değişken listesi içerir. Bir işlev üye çağrısının bağımsız değişken listesi belirtmek için sözdizimi işlevi üye kategorisine göre bağlıdır:

*  Örneğin Oluşturucular, yöntemler, dizin oluşturucular ve Temsilciler, bağımsız değişkenler olarak belirtilen bir *argument_list*, aşağıda açıklandığı gibi. Çağrılırken, dizin oluşturucular için `set` erişimci bağımsız değişken listesi ayrıca atama işlecinin sağ işleneni belirtilen ifade içerir.
*  Özellikler için bağımsız değişken listesi çağrılırken boştur `get` erişimci ve çağrılırken atama işlecinin sağ işleneni belirtilen ifade oluşur `set` erişimcisi.
*  Olaylar için sağ işleneni belirtilen ifade bağımsız değişken listesi oluşan `+=` veya `-=` işleci.
*  Kullanıcı tanımlı işleçler için bağımsız değişken listesi tek işlecinin işleneni, tekli veya ikili işleci iki işlenenleri oluşur.

Bağımsız değişken özelliklerinin ([özellikleri](classes.md#properties)), olaylar ([olayları](classes.md#events)) ve kullanıcı tanımlı işleçler ([işleçleri](classes.md#operators)) her zaman değer parametre olarak geçirilen ([ Parametre değeri](classes.md#value-parameters)). Dizin oluşturucular bağımsız değişkenleri ([dizin oluşturucular](classes.md#indexers)) her zaman değer parametre olarak geçirilen ([değer parametreleri](classes.md#value-parameters)) ya da parametre dizileri ([parametre dizileri](classes.md#parameter-arrays)). Başvuru ve çıkış parametreleri, bu işlev üyeleri kategoriler için desteklenmez.

Bir örnek oluşturucusu, yöntem, dizin oluşturucu veya temsilci çağırma bağımsız değişkenler olarak belirtilen bir *argument_list*:

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

Bir *argument_list* bir veya daha fazla oluşur *bağımsız değişken*virgülle ayırarak, s. Her bağımsız değişken isteğe oluşur *argument_name* arkasından bir *argument_value*. Bir *bağımsız değişken* ile bir *argument_name* şeklinde adlandırılan bir ***adlandırılmış bağımsız değişkeni***bilgileriyse bir *bağımsız değişken* olmadan bir  *argument_name* olduğu bir ***konumsal bağımsız değişkeni***. Adlandırılmış bir bağımsız değişken görüntülenmesini konumsal bağımsız değişken için bir hata olduğu bir *argument_list*.

*Argument_value* aşağıdaki biçimlerden birini alabilir:

*  Bir *ifade*, gösteren bir değer parametre olarak geçirilen bağımsız değişken ([değer parametreleri](classes.md#value-parameters)).
*  Anahtar sözcüğü `ref` arkasından bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)), bir başvuru parametresi geçirilen bağımsız değişken belirten ([başvuru parametreleri ](classes.md#reference-parameters)). Bir değişken kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) önce bir başvuru parametresi geçirilebilir. Anahtar sözcüğü `out` arkasından bir *variable_reference* ([değişken başvuruları](variables.md#variable-references)), bir output parametresi olarak geçirilen bağımsız değişken belirten ([çıkış parametresi](classes.md#output-parameters)). Bir değişken kesinlikle atanmış olarak kabul edilir ([belirli atama onayına](variables.md#definite-assignment)) izleyen bir işlev üye çağrısı değişkeni bir output parametresi olarak geçirilir.

#### <a name="corresponding-parameters"></a>Karşılık gelen parametreleri

Bir bağımsız değişken listesindeki her bağımsız değişkeni var. sahip işlev üyesi veya çağrılan temsilci karşılık gelen bir parametre olarak.

Aşağıda kullanılan parametre listesi aşağıdaki gibi belirlenir:

*  Sanal yöntemler ve sınıflar tanımlanmış dizin oluşturuculara, parametre listesi en belirgin bildirimden seçilir ya işlevi üyesinin alıcısı statik türü ile başlayan ve temel sınıflarının aramayı geçersiz kılın.
*  Arabirim yöntemleri ve dizin oluşturucular için parametre listesi çekilir üyesinin, arabirim türü ile başlayan ve temel arabirimleri aramayı en belirgin tanımı oluşturur. Benzersiz bir parametre listesi yok bulunduğunda, erişilemez adlarını ve isteğe bağlı parametre içeren bir parametre listesi oluşturulur, çağrıları adlandırılmış parametreler kullanan veya isteğe bağlı bağımsız değişkenlere atlayın.
*  Kısmi yöntemler için tanımlayan kısmi yöntem bildiriminde parametre listesi kullanılır.
*  Diğer tüm işlev üyeleri ve temsilciler için kullanılanla olan yalnızca bir tek bir parametre listesi yoktur.

Bir değişken veya parametre konumunu sayıda bağımsız değişken veya bağımsız değişken listesi veya parametre listesi önceki parametreleri olarak tanımlanır.

İşlev üye bağımsız değişkenleri için karşılık gelen parametreleri şu şekilde oluşturulur:

*  Bağımsız değişken *argument_list* örnek oluşturucuları, yöntemleri, Dizinleyicileri ve temsilciler:
    * Konumsal bağımsız değişken bir sabit parametre parametre listesindeki aynı konumda oluştuğu bu parametreye karşılık gelir.
    * Konumsal bağımsız değişken işlev üyesi çağrılır, normal bir biçimde bir parametre dizisi ile aynı konumda parametre listesinde gerçekleşmelidir parametre dizisine karşılık gelir.
    * Konumsal bağımsız değişken bir sabit parametre parametre listesindeki aynı konumda gerçekleştiği genişletilmiş hâli içinde çağrılan bir parametre dizisi ile işlevi üyesinin parametre dizisi içindeki bir öğeye karşılık gelir.
    * Adlandırılmış bir bağımsız değişken, parametre listesinde aynı ada sahip parametre karşılık gelir.
    * Çağrılırken, dizin oluşturucular için `set` erişimci, atama işlecinin sağ işleneninin karşılık gelen örtük olarak belirtilen ifade `value` parametresinin `set` erişimci bildirimi.
*  Çağrılırken özellikleri `get` erişimci var olan herhangi bir bağımsız değişken. Çağrılırken `set` erişimci, atama işlecinin sağ işleneninin karşılık gelen örtük olarak belirtilen ifade `value` parametresinin `set` erişimci bildirimi.
*  Kullanıcı tanımlı birli işleçler (dönüştürmeleri dahil) için tek bir işlenen işleç bildirimi, tek bir parametre karşılık gelir.
*  Kullanıcı tanımlı ikili işleçler için ilk parametre olarak sol işlenen karşılık gelir ve sağ işlenen işleç bildirimi ikinci parametresi için karşılık gelir.

#### <a name="run-time-evaluation-of-argument-lists"></a>Çalışma zamanı değerlendirmesini bağımsız değişken listeleri

Bir işlev üye çağrısı çalışma zamanı işlenmesi sırasında ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), ifadeler veya bağımsız değişken listesi değişken başvuruları sırayla soldan sağa doğru olarak değerlendirilir aşağıdaki gibidir:

*  Bir değer parametresi için bağımsız değişken ifadesi değerlendirilir ve örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) karşılık gelen parametre türü gerçekleştirilir. Sonuç değerini işlevi üye çağrısı değer parametresi ilk değeri olur.
*  Bir başvuru veya çıkış parametresi için değişken başvurusu değerlendirilir ve elde edilen depolama konumu işlev üyesi çağırma parametresi tarafından temsil edilen depolama konumu olur. Bir dizi öğesinin bir başvuru veya çıktı parametresi olarak verilen bir değişken başvurusu ise bir *reference_type*, dizinin öğe türü parametresinin türü için aynı olduğundan emin olmak için bir çalışma zamanı denetimi gerçekleştirilir. Bu denetim başarısız olursa bir `System.ArrayTypeMismatchException` oluşturulur.

Yöntemler, dizin oluşturucular ve örnek oluşturucuları bir parametre dizisi, en sağdaki parametre bildirmek ([parametre dizileri](classes.md#parameter-arrays)). Bu işlev üyeleri normal formlarına veya hangi uygun olduğuna bağlı olarak, genişletilmiş biçimde çağrılır ([geçerli işlev üyesi](expressions.md#applicable-function-member)):

*  Bir parametre dizisi olan bir işlevi üyesinin, normal bir biçimde çağrıldığında parametre dizisi için belirtilen bağımsız değişken örtük olarak dönüştürülebilir tek bir ifade olmalıdır ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) parametre dizi türü. Bu durumda, parametre dizisi, tam olarak bir değer parametresini gibi davranır.
*  Bir parametre dizisi olan bir işlevi üyesinin, genişletilmiş biçimde çağrıldığında, sıfır veya daha fazla konumsal bağımsız değişkenler örtük olarak dönüştürülebilir bir ifade bulunduğu her bağımsız değişken, bir parametre dizisi için çağırma belirtmeniz gerekir ([örtük dönüştürmeleri](conversions.md#implicit-conversions)) parametre dizinin öğe türü. Bu durumda, çağırma bağımsız değişken sayısı için karşılık gelen uzunluğu parametre dizi türü örneği oluşturur, belirtilen bağımsız değişken değerleri dizi örneği başlatır ve yeni oluşturulan bir dizi örneği fiili kullanır bağımsız değişken.

İfade bir bağımsız değişken listesinin, yazıldıkları sırayla her zaman değerlendirilir. Bu nedenle, örneğin
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
```
x = 0, y = 1, z = 2
x = 4, y = -1, z = 3
```

Dizi değişimini kuralları ([dizi kovaryansı](arrays.md#array-covariance)) bir dizi türünde bir değer izin `A[]` bir dizi türünde bir örneğe bir başvuru olarak `B[]`, gelen bir örtük bir başvuru dönüştürmesi var sağlanan `B` için `A`. Bu kurallar, bir dizi öğesi olduğunda nedeniyle bir *reference_type* geçirilen bir başvurusu veya çıktı parametresi olarak, dizinin gerçek öğe türü, parametre için aynı olduğundan emin olmak için bir çalışma zamanı denetimi gereklidir. Örnekte
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
İkinci çağırmayı `F` neden olan bir `System.ArrayTypeMismatchException` gerçek öğe türü için durum `b` olduğu `string` değil `object`.

Bir parametre dizisi olan bir işlevi üyesinin, genişletilmiş biçimde çağrıldığında, çağırma tam olarak bir dizi oluşturma ifadesi olarak bir dizi Başlatıcısı ile işlenir ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)) geçici olarak eklendi Genişletilmiş parametreleri. Örneğin, bildirimi verilen
```csharp
void F(int x, int y, params object[] args);
```
yöntem genişletilmiş biçiminin aşağıdaki çağrıları
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

Özellikle, boş bir dizi parametre dizisi için verilen sıfır bağımsız değişken olduğunda oluşturulduğunu unutmayın.

Karşılık gelen bir isteğe bağlı parametreler içeren bir işlevi üyesinin bağımsız değişken atlanırsa, varsayılan bağımsız değişkenleri işlev üyesi bildirimi örtük olarak geçirilir. Bunlar her zaman sabit olduğu için bunların değerlendirme kalan bağımsız değişkenleri Değerlendirme sırasını etkilemez.

### <a name="type-inference"></a>Tür çıkarımı

Genel yöntem çağrıldığında tür bağımsız değişkenleri, belirtmeden bir ***anlam çıkarma*** işlemi çağrısı için tür bağımsız değişkenleri Infer dener. Tür çıkarımı varlığını genel bir yöntem çağırmak için kullanılacak daha kullanışlı bir söz dizimi ve Programcı gereksiz tür bilgilerini belirtmekten kaçınmak olanak sağlar. Örneğin, yöntem bildiriminde verilen:
```csharp
class Chooser
{
    static Random rand = new Random();

    public static T Choose<T>(T first, T second) {
        return (rand.Next(2) == 0)? first: second;
    }
}
```
çağrılacak mümkündür `Choose` tür bağımsız değişkeni açıkça belirtmeden yöntemi:
```csharp
int i = Chooser.Choose(5, 213);                 // Calls Choose<int>

string s = Chooser.Choose("foo", "bar");        // Calls Choose<string>
```

Tür çıkarımı, tür bağımsız değişkeni aracılığıyla `int` ve `string` bağımsız değişkenlerden yönteme belirlenir.

Tür çıkarımı bağlama zamanı işleme yöntemi çağrısının bir parçası olarak gerçekleşir ([yöntem çağrıları](expressions.md#method-invocations)) ve çağırma aşırı yükleme çözünürlüğü adımdan önce gerçekleşir. Belirli yöntem grubu içinde bir yöntem çağırmayla belirtilir ve hiçbir tür bağımsız değişkenleri yöntemi çağrısının bir parçası olarak belirtilen tür çıkarımı her genel yöntem yöntem grubu uygulanır. Tür çıkarımı başarılı olursa çıkarsanan tür bağımsız değişkenlerini sonraki aşırı yükleme çözümlemesi için bağımsız değişken türlerini belirlemek için kullanılır. Aşırı yükleme çözünürlüğü çağırmak için bir genel yöntem seçerse, gösterilen türde bağımsız değişkenleri çağrı gerçek tür bağımsız değişkenleri olarak kullanılır. Belirli bir yöntem için tür çıkarımı başarısız olursa, bu yöntem aşırı yükleme çözünürlüğü içinde yer almaz. Tür çıkarımı, içinde ve kendisinin, başarısız bir bağlama zamanı hatası neden olmaz. Ancak, uygun yöntemleri bulmak ardından aşırı yükleme çözümlemesi başarısız olduğunda genellikle bir bağlama zamanı hata oluşur.

Sağlanan bağımsız değişken sayısı yöntemindeki parametre sayısından farklı ise, çıkarım hemen başarısız olur. Aksi takdirde, genel yöntem aşağıdaki imzası sahip olduğunu varsayın:
```csharp
Tr M<X1,...,Xn>(T1 x1, ..., Tm xm)
```

Formun yöntemi çağrısıyla `M(E1...Em)` tür çıkarımı görevin benzersiz tür bağımsız değişkenleri bulmaktır `S1...Sn` her tür parametrelerinin `X1...Xn` böylece çağrı `M<S1...Sn>(E1...Em)` geçerli olur.

Çıkarma işlemi sırasında her tür parametresi `Xi` ya da *sabit* belirli bir türe `Si` veya *sabitlenmemiş* kümesi ile *sınırları*. Her sınır bazı türüdür `T`. Başlangıçta her tür değişkeni `Xi` boş kümesi ile sınırlarının sabitlenmemiş olduğu.

Tür çıkarımı aşamada gerçekleşir. Her aşama önceki aşamanın bulguları üzerinde göre daha fazla türü değişkenler için tür bağımsız değişkenleri Infer dener. İkinci aşama türü değişkenler belirli türlere giderir ve daha fazla sınır algılar ancak ilk aşaması sınırlarının, ilk bazı çıkarımı yapar. İkinci aşama olması gerekebilir birkaç kez yinelenir.

*Not:* Yalnızca bir genel yöntem çağrıldığında tür çıkarımı gerçekleşir. Tür çıkarımı yöntemi gruplarına dönüştürülmesi için açıklanan [anlam çıkarma dönüştürme yöntemi gruplarının](expressions.md#type-inference-for-conversion-of-method-groups) ve en iyi ortak türü bir ifade kümesi bulma bölümünde açıklanmıştır [birtakım en iyi ortak türü bulma ifadelerin](expressions.md#finding-the-best-common-type-of-a-set-of-expressions).

#### <a name="the-first-phase"></a>Birinci aşama

Her yöntem bağımsız `Ei`:

*   Varsa `Ei` anonim bir işlev, bir *açık parametre tür çıkarımı* ([açık parametre türü çıkarımı](expressions.md#explicit-parameter-type-inferences)) öğesinden yapılan `Ei` için `Ti`
*   Aksi takdirde `Ei` bir türe sahip `U` ve `xi` değer parametresi bir *alt sınır çıkarımı* yapılan *gelen* `U` *için* `Ti`.
*   Aksi takdirde `Ei` bir türe sahip `U` ve `xi` olduğu bir `ref` veya `out` parametresi bir *tam çıkarımı* yapılan *gelen* `U` *için* `Ti`.
*   Aksi takdirde, hiçbir çıkarımı bu bağımsız değişken için yapılır.


#### <a name="the-second-phase"></a>İkinci aşaması

İkinci aşama aşağıdaki gibi çalışır:

*   Tüm *sabitlenmemiş* türü değişkenler `Xi` gerektirmeyen *bağlıdır* ([bağımlılığı](expressions.md#dependence)) tüm `Xj` sabit ([düzeltme](expressions.md#fixing)).
*   Böyle bir türü değişkenler varsa, tüm *sabitlenmemiş* türü değişkenler `Xi` olan *sabit* kendisi için aşağıdakilerin tümü tutun:
    *   En az bir tür değişkeni `Xj` , bağlıdır `Xi`
    *   `Xi` bir olmayan-boş dizi sınırları vardır
*   Böyle bir türü değişkenler mevcutsa ve hala *sabitlenmemiş* değişkenleri, anlam çıkarma başarısız yazın.
*   Aksi takdirde, başka *sabitlenmemiş* türü değişkenler var, tür çıkarımı başarılı olur.
*   Aksi takdirde tüm bağımsız değişkenler için `Ei` karşılık gelen parametre türüyle `Ti` burada *çıktı türleri* ([çıktı türleri](expressions.md#output-types)) içeren *sabitlenmemiş* türü değişkenler `Xj` ancak *giriş türleri* ([giriş türleri](expressions.md#input-types)) bulunmayan bir *çıkış tür çıkarımı* ([çıkış tür çıkarımı ](expressions.md#output-type-inferences)) yapılan *gelen* `Ei` *için* `Ti`. İkinci aşama daha sonra yinelenir.

#### <a name="input-types"></a>Giriş türleri

Varsa `E` bir yöntem grubu ya da anonim işlev örtük olarak belirlenmiş ve `T` bir temsilci türü veya ifade ağacı türü sonra tüm parametre türlerini mi `T` olan *giriş türleri* , `E` *türüyle* `T`.

####  <a name="output-types"></a>Çıktı türleri

Varsa `E` bir yöntem grubu veya anonim bir işlevdir ve `T` bir temsilci türü veya ifade ağacı türü sonra dönüş türü olan `T` olduğu bir *çıktı türünü* `E` *türüne sahip*  `T`.

#### <a name="dependence"></a>Bağımlılık

Bir *sabitlenmemiş* tür değişkeni `Xi` *doğrudan bağlı olduğu* sabitlenmemiş türü değişkeni `Xj` bazı bağımsız değişkeni ise `Ek` türüyle `Tk` `Xj` oluşan bir *giriş türü* , `Ek` türüyle `Tk` ve `Xi` oluşan bir *çıkış türü* , `Ek` türüyle `Tk`.

`Xj` *bağımlı* `Xi` varsa `Xj` *doğrudan bağlı olduğu* `Xi` veya `Xi` *doğrudan bağlı olduğu* `Xk` ve `Xk` *bağlıdır* `Xj`. Bu nedenle "bağlıdır" olan "doğrudan bağlı olduğu" geçişli ancak değil Yansımalı kapatma.

#### <a name="output-type-inferences"></a>Çıkış türü çıkarımı

Bir *çıkış tür çıkarımı* yapılan *gelen* bir ifade `E` *için* türü `T` şu şekilde:

*  Varsa `E` çıkarılan dönüş türü anonim bir işlevdir `U` ([Inferred dönüş türü](expressions.md#inferred-return-type)) ve `T` bir temsilci türü veya dönüş türüne sahip ifade ağacı türü `Tb`, ardından *alt sınır çıkarımı* ([alt sınır çıkarımı](expressions.md#lower-bound-inferences)) yapılan *gelen* `U` *için* `Tb`.
*  Aksi takdirde `E` bir yöntem grubu olduğundan ve `T` bir temsilci türü veya parametre türleri ile ifade ağacı türü `T1...Tk` ve dönüş türü `Tb`ve aşırı yükleme çözünürlüğü `E` türleriyle `T1...Tk` verir bir tek bir yöntemin dönüş türü içeren `U`, ardından bir *alt sınır çıkarımı* yapılan *gelen* `U` *için* `Tb`.
*  Aksi takdirde `E` türüne sahip ifade `U`, bir *alt sınır çıkarımı* yapılan *gelen* `U` *için* `T`.
*  Aksi takdirde, hiçbir çıkarımı yapılır.

#### <a name="explicit-parameter-type-inferences"></a>Açık parametre tür çıkarımı

Bir *açık parametre tür çıkarımı* yapılan *gelen* bir ifade `E` *için* türü `T` şu şekilde:

*  Varsa `E` parametre türleri ile açıkça yazılmış bir anonim işlev `U1...Uk` ve `T` bir temsilci türü veya parametre türleri ile ifade ağacı türü `V1...Vk` sonra her `Ui` bir *tam çıkarım* ([tam çıkarımı](expressions.md#exact-inferences)) yapılan *gelen* `Ui` *için* karşılık gelen `Vi`.

#### <a name="exact-inferences"></a>Tam çıkarımı

Bir *tam çıkarımı* *gelen* türü `U` *için* türü `V` şu şekilde yapılır:

*  Varsa `V` biri *sabitlenmemiş* `Xi` ardından `U` tam sınırlarının kümesine eklenen `Xi`.

*  Aksi takdirde ayarlar `V1...Vk` ve `U1...Uk` aşağıdaki durumlarda atanamadığı olmadığının kontrol edilmesiyle belirlenir:

   *  `V` bir dizi türü `V1[...]` ve `U` bir dizi türü `U1[...]` aynı boyut,
   *  `V` türü `V1?` ve `U` türü `U1?`
   *  `V` bir oluşturulmuş tür olsa `C<V1...Vk>`ve `U` bir oluşturulmuş tür olsa `C<U1...Uk>`

   Bu durumların herhangi birinde ardından uygularsanız bir *tam çıkarımı* yapılan *gelen* her `Ui` *için* karşılık gelen `Vi`.

*  Aksi takdirde, hiçbir çıkarımı yapılır.

#### <a name="lower-bound-inferences"></a>Alt sınır çıkarımı

A *alt sınır çıkarımı* *gelen* türü `U` *için* türü `V` şu şekilde yapılır:

*  Varsa `V` biri *sabitlenmemiş* `Xi` ardından `U` alt sınırı kümesine eklenen `Xi`.
*  Aksi takdirde `V` türü `V1?`ve `U` türü `U1?` alt sınır çıkarımı yapılan sonra `U1` için `V1`.
*  Aksi takdirde ayarlar `U1...Uk` ve `V1...Vk` aşağıdaki durumlarda atanamadığı olmadığının kontrol edilmesiyle belirlenir:
   *  `V` bir dizi türü `V1[...]` ve `U` bir dizi türü `U1[...]` (veya tür parametresi, geçerli bir temel türü olan `U1[...]`) aynı boyut,
   *  `V` biri `IEnumerable<V1>`, `ICollection<V1>` veya `IList<V1>` ve `U` tek boyutlu dizi türü `U1[]`(veya tür parametresi, geçerli bir temel türü olan `U1[]`)
   *  `V` oluşturulan sınıf, yapı, arabirim veya temsilci türü `C<V1...Vk>` ve benzersiz bir tür `C<U1...Uk>` şekilde `U` (veya `U` bir tür parametresi, etkili temel sınıfı veya etkin arabirim kendine herhangi bir üyesi) olan aynıdır, devralınan (doğrudan veya dolaylı olarak) veya (doğrudan veya dolaylı olarak) uygulayan `C<U1...Uk>`.

      (Büyük/küçük harf arabiriminde anlamına "benzersizlik" kısıtlama `C<T> {} class U: C<X>, C<Y> {}`, ardından gelen çıkarımını yapma, hiçbir çıkarımı yapılan `U` için `C<T>` çünkü `U1` olabilir `X` veya `Y`.)

   Ardından bir çıkarımı yapılan tüm durumlarda uygularsanız *gelen* her `Ui` *için* karşılık gelen `Vi` şu şekilde:

   *  Varsa `Ui` bir başvuru türü bilinmiyor bir *tam çıkarımı* yapılır
   *  Aksi takdirde `U` bir dizi türü bir *alt sınır çıkarımı* yapılır
   *  Aksi takdirde `V` olduğu `C<V1...Vk>` çıkarımı i. tür parametresi üzerinde bağlıdır sonra `C`:
      *  Birlikte değişken ise bir *alt sınır çıkarımı* yapılır.
      *  Değişken karşıtı ise bir *üst sınır çıkarımı* yapılır.
      *  Sabit olması durumunda bir *tam çıkarımı* yapılır.
*  Aksi takdirde, hiçbir çıkarımı yapılır.

#### <a name="upper-bound-inferences"></a>Üst sınır çıkarımı

Bir *üst sınır çıkarımı* *gelen* türü `U` *için* türü `V` şu şekilde yapılır:

*  Varsa `V` biri *sabitlenmemiş* `Xi` ardından `U` için üst sınır kümesine eklenen `Xi`.
*  Aksi takdirde ayarlar `V1...Vk` ve `U1...Uk` aşağıdaki durumlarda atanamadığı olmadığının kontrol edilmesiyle belirlenir:
   *  `U` bir dizi türü `U1[...]` ve `V` bir dizi türü `V1[...]` aynı boyut,
   *  `U` biri `IEnumerable<Ue>`, `ICollection<Ue>` veya `IList<Ue>` ve `V` tek boyutlu dizi türü `Ve[]`
   *  `U` türü `U1?` ve `V` türü `V1?`
   *  `U` oluşturulan sınıf, yapı, arabirim veya temsilci türü olduğundan `C<U1...Uk>` ve `V` bir sınıf, yapı, devralır, (doğrudan veya dolaylı olarak) eşdeğer olan arabirim veya temsilci türünün veya uygular (doğrudan veya dolaylı olarak) olan bir benzersiz tür `C<V1...Vk>`

      (Biz varsa anlamına "benzersizlik" kısıtlama `interface C<T>{} class V<Z>: C<X<Z>>, C<Y<Z>>{}`, ardından gelen çıkarımını yapma, hiçbir çıkarımı yapılan `C<U1>` için `V<Q>`. Çıkarımı değil yapılır `U1` ya da `X<Q>` veya `Y<Q>`.)

   Ardından bir çıkarımı yapılan tüm durumlarda uygularsanız *gelen* her `Ui` *için* karşılık gelen `Vi` şu şekilde:
   *  Varsa `Ui` bir başvuru türü bilinmiyor bir *tam çıkarımı* yapılır
   *  Aksi takdirde `V` bir dizi türü bir *üst sınır çıkarımı* yapılır
   *  Aksi takdirde `U` olduğu `C<U1...Uk>` çıkarımı i. tür parametresi üzerinde bağlıdır sonra `C`:
      *  Birlikte değişken ise bir *üst sınır çıkarımı* yapılır.
      *  Değişken karşıtı ise bir *alt sınır çıkarımı* yapılır.
      *  Sabit olması durumunda bir *tam çıkarımı* yapılır.
*  Aksi takdirde, hiçbir çıkarımı yapılır.   

#### <a name="fixing"></a>Düzeltme

Bir *sabitlenmemiş* tür değişkeni `Xi` sınırları bir dizi *sabit* şu şekilde:

*  Dizi *aday türleri* `Uj` sınırlarının kümesindeki tüm türleri olarak başlar `Xi`.
*  Ardından her sınırını inceleyeceğiz `Xi` sırasıyla: Her tam bağlama için `U` , `Xi` tüm türleri `Uj` olmayan aynı `U` aday kümesinden kaldırılır. Her alt sınırı için `U` , `Xi` tüm türleri `Uj` için hangi orada olduğu *değil* örtük bir dönüştürme `U` aday kümesinden kaldırılır. Her üst sınırı için `U` , `Xi` tüm türleri `Uj` hangi buradan olan *değil* örtük dönüştürmeleri `U` aday kümesinden kaldırılır.
*  Eğer kalan aday türleri arasında `Uj` benzersiz bir tür `V` olduğu örtük dönüştürmeleri diğer tüm aday türleri, ardından gelen `Xi` için sabit `V`.
*  Aksi takdirde, tür çıkarımı başarısız olur.

#### <a name="inferred-return-type"></a>Çıkarılan dönüş türü

Çıkarılan dönüş türü anonim bir işlevin `F` tür çıkarımı ve aşırı yükleme çözümlemesi sırasında kullanılır. Çıkarılan dönüş türü, burada verilen açık olduğundan, türleri, ya da bilinen tüm parametre bir anonim işlev dönüştürme sağlanan veya üzerinde bir kapsayan tür çıkarımı sırasında genel çıkarılan anonim bir işlev için yalnızca belirlenebilir yöntem çağırma.

***Sonuç türü çıkarımı yapılan*** şu şekilde belirlenir:

*  Varsa gövdesinin `F` olduğu bir *ifade* türü ve çıkarılan bir sonuç türü olan `F` ifade türü.
*  Varsa gövdesinin `F` olduğu bir *blok* ve bloğun ifadelerde dizi `return` deyimleri en iyi ortak bir türe sahip `T` ([birifadekümesienyaygıntürübulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)), ardından çıkarılan bir sonuç türü `F` olduğu `T`.
*  Aksi halde, sonuç türü için çıkarsanamıyor `F`.

***Dönüş türü çıkarımı yapılan*** şu şekilde belirlenir:

*  Varsa `F` zaman uyumsuz ve gövdesini `F` nothing sınıflandırılmış bir ya da bir ifade ([ifade sınıflandırmaları](expressions.md#expression-classifications)), ya da dönüş deyim ifadeleri, sahip olduğu bir deyim bloğunu çıkarılan dönüş türü `System.Threading.Tasks.Task`
*  Varsa `F` zaman uyumsuz olduğunu ve çıkarılan bir sonuç türü `T`, çıkarılan dönüş türü `System.Threading.Tasks.Task<T>`.
*  Varsa `F` olmayan zaman uyumsuz olduğunu ve çıkarılan bir sonuç türü `T`, çıkarılan dönüş türü `T`.
*  Aksi takdirde bir dönüş türü için çıkarsanamıyor `F`.

Tür çıkarımı anonim işlevler içeren bir örnek olarak, göz önünde bulundurun `Select` genişletme yöntemi içinde bildirilen `System.Linq.Enumerable` sınıfı:
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

Varsayılarak `System.Linq` ad alanı ile alınmış bir `using` yan tümcesi ve bir sınıf belirtilen `Customer` ile bir `Name` türünün özelliği `string`, `Select` yöntemi, müşterilerin listesini adlarını seçmek için kullanılabilir:
```csharp
List<Customer> customers = GetCustomerList();
IEnumerable<string> names = customers.Select(c => c.Name);
```

Uzantı metodu çağırma ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)), `Select` çağrıya bir statik yöntem çağırma yazarak işlenir:
```csharp
IEnumerable<string> names = Enumerable.Select(customers, c => c.Name);
```

Tür bağımsız değişkenleri açıkça belirtilmedi olduğundan, tür çıkarımı tür bağımsız değişkenlerini çıkarsamak için kullanılır. İlk olarak, `customers` bağımsız değişken ilgili `source` parametresi çıkarımını yapma `T` olmasını `Customer`. Anonim işlev kullanarak yazın, yukarıda açıklanan çıkarımı işleminin `c` türü verilen `Customer`ve ifade `c.Name` dönüş türü için ilgili `selector` parametresi çıkarımını yapma `S` olması`string`. Bu nedenle, çağırma eşdeğerdir
```csharp
Sequence.Select<Customer,string>(customers, (Customer c) => c.Name)
```
ve sonuç türünü `IEnumerable<string>`.

Aşağıdaki örnek nasıl anonim işlev türü gösterir çıkarımı "bağımsız değişken bir genel yöntem çağrısı arasında akış" tür bilgilerini sağlar. Yöntem verilen:
```csharp
static Z F<X,Y,Z>(X value, Func<X,Y> f1, Func<Y,Z> f2) {
    return f2(f1(value));
}
```

Tür çıkarımı çağrısı için:
```csharp
double seconds = F("1:15:30", s => TimeSpan.Parse(s), t => t.TotalSeconds);
```
şu şekilde çalışır: İlk olarak, bağımsız değişken `"1:15:30"` ilgili `value` parametresi çıkarımını yapma `X` olmasını `string`. Ardından, ilk anonim işlev parametresinin `s`, çıkarsanan tür verilen `string`ve ifade `TimeSpan.Parse(s)` dönüş türü için ilgili `f1`, çıkarımını yapma `Y` olmasını `System.TimeSpan`. Son olarak, ikinci anonim işlev parametresi `t`, çıkarsanan tür verilen `System.TimeSpan`ve ifade `t.TotalSeconds` dönüş türü için ilgili `f2`, çıkarımını yapma `Z` olmasını `double`. Bu nedenle, çağrısının sonucunu türünde `double`.

#### <a name="type-inference-for-conversion-of-method-groups"></a>Dönüştürme yöntemi grupları için tür çıkarımı

Benzer şekilde, genel yöntemlerin kullanımını çağrıları, tür çıkarımı ayrıca bir yöntem grubu olduğunda uygulanmalıdır `M` genel yöntem içeren bir temsilci türüne dönüştürülür `D` ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)). Verilen bir yöntemi
```csharp
Tr M<X1...Xn>(T1 x1 ... Tm xm)
```
ve yöntem grubu `M` temsilci türüne atanan `D` görevi tür çıkarımı, tür bağımsız değişkenleri bulmaktır `S1...Sn` böylece ifade:
```csharp
M<S1...Sn>
```
uyumlu hale gelir ([temsilci bildirimi](delegates.md#delegate-declarations)) ile `D`.

Genel yöntem çağrıları için tür çıkarımı algoritması bu durumda olup yalnızca bağımsız değişken *türleri*, hiçbir bağımsız değişken *ifadeleri*. Özellikle, hiçbir anonim işlev vardır ve bu nedenle birden çok aşamaları çıkarımı gerek yoktur.

Bunun yerine, tüm `Xi` değerlendirilir *sabitlenmemiş*ve *alt sınır çıkarımı* yapılan *gelen* her bağımsız değişken türü `Uj` , `D` *için* karşılık gelen parametre türü `Tj` , `M`. Eğer herhangi birinin `Xi` sınır yoktur bulunamadı, tür çıkarımı başarısız olur. Aksi takdirde, tüm `Xi` olan *sabit* karşılık gelen `Si`, tür çıkarımı sonucu olan.

#### <a name="finding-the-best-common-type-of-a-set-of-expressions"></a>Bir ifade kümesi en iyi ortak türü bulma

Bazı durumlarda, ortak bir türe ifade kümesi için çıkarım gerekir. Belirli, örtük olarak yazılan diziler öğesi türleri ve dönüş türleri anonim işlevlerle *blok* gövdeleri bu şekilde bulundu.

Sezgisel, bir ifade kümesi verilen `E1...Em` bu çıkarım bir yöntem çağrısına eşdeğerdir
```csharp
Tr M<X>(X x1 ... X xm)
```
ile `Ei` bağımsız değişkenler olarak.

Daha kesin çıkarımı başlar bir *sabitlenmemiş* tür değişkeni `X`. *Çıkış türü çıkarımı* ardından yapılan *gelen* her `Ei` *için* `X`. Son olarak, `X` olduğu *sabit* ve başarılı olursa, sonuç türü `S` elde edilen en yaygın tür ifadeler için. Eğer böyle `S` yoksa, en iyi ortak tür ifadeleri sahip.

### <a name="overload-resolution"></a>Aşırı yükleme çözümlemesi

Aşırı yükleme çözünürlüğü, bir bağımsız değişken listesi ve aday işlevi üyelerin kümesini çağırmak için en iyi işlevi üye seçme bir bağlama zamanı mekanizmadır. Aşırı yükleme çözünürlüğü, C# içinde aşağıdaki ayrı bağlamlarda çağrılacak işlev üyesi seçer:

*  Adlı bir yöntemin çağrı bir *invocation_expression* ([yöntem çağrıları](expressions.md#method-invocations)).
*  Adlı bir örnek oluşturucusunda çağrılmasını bir *object_creation_expression* ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).
*  Çağırma bir dizin oluşturucu erişimcisinin bir *element_access* ([öğe erişimi](expressions.md#element-access)).
*  Bir deyimde başvurulan önceden tanımlı veya kullanıcı tanımlı işleç çağırmayı ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution) ve [ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)).

Her biri bu içerikler aday işlevi üyelerin kümesini ve bağımsız değişkenler listesi kendi benzersiz bir yolla, yukarıda listelenen bölümlerde ayrıntılı olarak açıklandığı gibi tanımlar. Örneğin, bir yöntem çağırma için aday kümesini işaretlenmiş yöntemler içermez `override` ([üye araması](expressions.md#member-lookup)), ve bir taban sınıf yöntemlerini olmayan adaylar türetilmiş bir sınıf içinde herhangi bir yöntemi geçerli ise ([ Yöntem çağrıları](expressions.md#method-invocations)).

Aday işlev üyeleri ve bağımsız değişken listesi uyguladığınızı belirledikten sonra en iyi işlevi üye seçimi tüm durumlarda aynıdır:

*  Kümesi bulunur, uygun aday işlevi üyelerin kümesini göz önünde bulundurulduğunda, en iyi üye işlevi. Set işlevi yalnızca bir üye içeriyorsa, ardından işlevi üyenin en iyi işlevi üyesidir. Aksi takdirde, en iyi işlevi şartıyla her işlev üyesi kuralları kullanarak diğer işlev üyeleri karşılaştırılır, belirtilen bağımsız değişken listesi ile ilgili diğer tüm işlev üyeleri daha iyi bir işlevin üye üyesidir [ Daha iyi işlevi üye](expressions.md#better-function-member). Tam olarak diğer tüm işlev üyeleri iyi bir işlev üyesi ise, işlev üye çağrısı belirsiz olduğu ve bir bağlama zamanı hatası oluşur.

Aşağıdaki bölümlerde tam anlamları koşullarını tanımlamak ***geçerli işlev üyesi*** ve ***daha iyi işlevi üye***.

#### <a name="applicable-function-member"></a>Geçerli işlev üyesi

Bir işlevin üyesi olduğu söylenir bir ***geçerli işlev üyesi*** bağımsız değişken listesine göre `A` şunların hepsini olduğunda true:

*  Her bağımsız değişkende `A` bir parametreye işlev üyesi bildiriminde açıklandığı karşılık gelen [karşılık gelen parametreleri](expressions.md#corresponding-parameters), ve herhangi bir parametre hiçbir bağımsız değişken karşılık gelen bir isteğe bağlı bir parametredir.
*  Her bağımsız değişkeni için `A`, parametre modunu bağımsız değişken geçirme (yani, değer `ref`, veya `out`) karşılık gelen parametre, parametre geçirme moduna aynıdır ve
   *  bir değer parametresini veya bir parametre dizisi, örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) karşılık gelen parametre türüne bağımsız değişkende var veya
   *  için bir `ref` veya `out` parametresi, bağımsız değişken türünü karşılık gelen parametre türü için aynı. Sonuçta bir `ref` veya `out` parametredir geçirilen bağımsız değişken için bir diğer ad.

Bir parametre dizisi içeren işlev üyesi için yukarıdaki kurallar tarafından işlevi üyesi olup olmadığını, uygulanabilir olduğu söylenir kendi ***normal form***. Bir parametre dizisi içeren işlev üyesi, normal bir biçimde geçerli değilse, işlev üye bunun yerine uygulanabilir olabilir, ***genişletilmiş form***:

*  Genişletilmiş formun işlev üyesi bildiriminde parametre dizisi sıfır ile değiştirilerek oluşturulur veya daha fazla değer parametre parametresinin öğe türünün gibi dizi bu bağımsız değişken listesindeki bağımsız değişken sayısı `A` toplam eşleşir parametre sayısı. Varsa `A` işlevi üyesi bildiriminde sabit parametre sayısından daha az sayıda bağımsız değişken varsa, genişletilmiş formun işlevi üyesinin oluşturulamaz ve bu nedenle geçerli değildir.
*  Aksi takdirde, genişletilmiş form, her değişken için geçerli `A` bağımsız değişken geçirme mode parametresi için karşılık gelen parametre, parametre geçirme modu aynıdır ve
   *  bir sabit değer parametresi veya örtük bir dönüştürme genişletmesi tarafından oluşturulan bir değer parametresini ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) karşılık gelen parametresinin türü için bağımsız değişkenin türünden var veya
   *  için bir `ref` veya `out` parametresi, bağımsız değişken türünü karşılık gelen parametre türü için aynı.

#### <a name="better-function-member"></a>Daha iyi işlev üyesi

Daha iyi işlevi üye belirleme amacıyla, yalnızca bağımsız değişken ifadeleri kendilerini özgün bağımsız değişken listesinde göründükleri sırayla içeren stripped-down bağımsız değişken listesini bir oluşturulur.

Her üye şu şekilde oluşturulur aday işlevi için parametre listeler:

*  Üye işlevi yalnızca genişletilmiş biçiminde geçerli genişletilmiş formun kullanılır.
*  Karşılık gelen bağımsız değişken içermeyen isteğe bağlı parametreler parametre listesinden kaldırılır
*  Bunlar bağımsız değişken listesindeki aynı konuma karşılık gelen bağımsız değişken olarak gerçekleşmesi parametreleri yeniden sıralanır.

Bir bağımsız değişken listesi verilen `A` bağımsız değişken ifadeleri kümesiyle `{E1, E2, ..., En}` ve iki geçerli işlev üyeleri `Mp` ve `Mq` parametre türleri ile `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}`, `Mp` olarak tanımlanır bir ***daha iyi işlevi üye*** daha `Mq` varsa

*  Her bir bağımsız değişkeni, arasında örtük dönüşüm `Ex` için `Qx` arasında örtük dönüşüm daha iyi değil `Ex` için `Px`, ve
*  en az bir bağımsız değişkeni, dönüştürme `Ex` için `Px` dönüştürme işlemi daha iyidir `Ex` için `Qx`.

Bu değerlendirme, gerçekleştirirken `Mp` veya `Mq` genişletilmiş hâli içinde geçerli ise `Px` veya `Qx` parametre listesi genişletilmiş biçiminde bir parametreye başvuruyor.

Tür parametresi dizileri durumunda `{P1, P2, ..., Pn}` ve `{Q1, Q2, ..., Qn}` eşdeğerdir (yani her `Pi` karşılık gelen bir kimlik dönüştürme sahip `Qi`), aşağıdaki KRAVAT sonu kuralları uygulanır, daha iyi belirlemek için sırayla üye işlevi.

*  Varsa `Mp` genel olmayan bir yöntem ve `Mq` genel bir yöntem ise `Mp` göre daha iyidir `Mq`.
*  Aksi takdirde `Mp` , normal bir biçimde uygulanabilir ve `Mq` sahip bir `params` dizisi ve ardından genişletilmiş biçimde yalnızca kendi geçerlidir `Mp` göre daha iyidir `Mq`.
*  Aksi takdirde `Mp` daha fazla parametre gözükeceğini `Mq`, ardından `Mp` göre daha iyidir `Mq`. Her iki yöntem de sahip olmadığında ortaya çıkabilir `params` diziler ve bu yalnızca genişletilmiş formlarında uygulanabilir.
*  Aksi halde, tüm parametreleri `Mp` varsayılan bağımsız değişkenler için en az bir isteğe bağlı bir parametre atanması gerekiyor ancak karşılık gelen bir bağımsız değişken olan `Mq` ardından `Mp` göre daha iyidir `Mq`.
*  Aksi takdirde `Mp` daha fazla belirli parametre türleri vardır `Mq`, ardından `Mp` göre daha iyidir `Mq`. İzin `{R1, R2, ..., Rn}` ve `{S1, S2, ..., Sn}` örneklenmemiş ve genişletilmemiş parametre türleri temsil eden `Mp` ve `Mq`. `Mp`ın parametre türleri daha belirli `Mq`ait ise, her bir parametreye `Rx` less yazımına özgü değil `Sx`ve en az bir parametre `Rx` daha belirli olduğundan `Sx`:
   *  Bir tür olmayan parametresi daha az belirli bir tür parametresidir.
   *  Yinelemeli olarak oluşturulmuş bir türü en az bir bağımsız değişken türü (tür bağımsız değişkenleri aynı sayıda ile) oluşturulan başka bir tür daha özeldir ve hiçbir tür bağımsız değişkeni karşılık gelen tür bağımsız değişkeni diğer less yazımına özgü daha ayrıntılı olarak.
   *  İlk öğe türü ikinci öğesi türünden daha belirli ise bir dizi türü (aynı boyut sayısı ile) başka bir dizi türü daha belirgin olur.
*  Aksi takdirde yükseltilmiş bir işleci bir üyesidir ve diğer lifted işleci ise, yükseltilmiş bir iyidir.
*  Aksi takdirde, hiçbir işlev üyesi daha iyidir.

#### <a name="better-conversion-from-expression"></a>İfadeden daha iyi dönüştürme

Örtük bir dönüştürme verilen `C1` ifadeden dönüştüren `E` türüne `T1`ve örtük bir dönüştürme `C2` ifadeden dönüştüren `E` türüne `T2`, `C1` olan bir ***daha iyi bir dönüştürme*** daha `C2` varsa `E` tam olarak eşleşmiyor `T2` ve aşağıdakilerden birini barındırır:

* `E` tam olarak eşleşen `T1` ([ifadesi tam olarak eşleşen](expressions.md#exactly-matching-expression))
* `T1` daha iyi bir dönüştürme hedefi `T2` ([daha iyi dönüştürme hedef](expressions.md#better-conversion-target))

#### <a name="exactly-matching-expression"></a>Tam olarak eşleşen bir ifade

Bir ifade verilen `E` ve türü `T`, `E` tam olarak eşleşen `T` tutuyorsa aşağıdakilerden biri:

*  `E` bir türe sahip `S`, ve gelen bir kimlik dönüştürme var `S` için `T`
*  `E` Anonim bir işlev ise `T` bir temsilci türü `D` veya bir ifade ağacı türü `Expression<D>` ve aşağıdakilerden birini barındırır:
   *  Çıkarılan bir dönüş türü `X` yok `E` parametre listesini bağlamında `D` ([Inferred dönüş türü](expressions.md#inferred-return-type)), ve gelen bir kimlik dönüştürme var `X` için dönüş türü `D`
   *  Her iki `E` olmayan zaman uyumsuz olduğunu ve `D` dönüş türü olan `Y` veya `E` zaman uyumsuz olduğunu ve `D` dönüş türü olan `Task<Y>`, ve aşağıdakilerden birini barındırır:
      * Gövdesi `E` bir ifade, tam olarak eşleşme. `Y`
      * Gövdesi `E` her return deyimi bu tam olarak eşleşen bir ifade döndüğü bir deyim bloğunu olduğu `Y`

#### <a name="better-conversion-target"></a>Daha iyi dönüştürme hedef

Verilen iki `T1` ve `T2`, `T1` daha iyi bir dönüştürme hedefi `T2` örtülü dönüştürme olmaz, `T2` için `T1` var ve aşağıdakilerden birini barındırır:

*  Örtük bir dönüştürme `T1` için `T2` var.
*  `T1` bir temsilci türü `D1` veya bir ifade ağacı türü `Expression<D1>`, `T2` bir temsilci türü `D2` veya bir ifade ağacı türü `Expression<D2>`, `D1` dönüş türü olan `S1` ve biri Aşağıdaki tutar:
   * `D2` void döndüren
   * `D2` dönüş türü olan `S2`, ve `S1` daha iyi bir dönüştürme hedefi `S2`
*  `T1` olan `Task<S1>`, `T2` olduğu `Task<S2>`, ve `S1` daha iyi bir dönüştürme hedefi `S2`
*  `T1` olan `S1` veya `S1?` burada `S1` işaretli bir integral türü ve `T2` olduğu `S2` veya `S2?` burada `S2` işaretsiz bir tamsayı türüdür. Özellikle:
   * `S1` olan `sbyte` ve `S2` olduğu `byte`, `ushort`, `uint`, veya `ulong`
   * `S1` olan `short` ve `S2` olduğu `ushort`, `uint`, veya `ulong`
   * `S1` olan `int` ve `S2` olduğu `uint`, veya `ulong`
   * `S1` olan `long` ve `S2` olduğu `ulong`

#### <a name="overloading-in-generic-classes"></a>Genel sınıflar aşırı yükleme

Bildirilen imzaları benzersiz olmasını sağlarken tür bağımsız değişkenlerini değiştirme içinde aynı imzaya sonuçları mümkündür. Bu gibi durumlarda, yukarıdaki aşırı yükleme çözünürlüğü KRAVAT sonu kurallarını en belirgin üye seçer.

Aşağıdaki örnekler, geçerli ve bu kuralına göre geçersiz aşırı yüklemeleri gösterir:

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

### <a name="compile-time-checking-of-dynamic-overload-resolution"></a>Derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor

En dinamik olarak bağlı işlemler için olası adaylar çözümlemesi için derleme zamanında bilinmeyen kümesidir. Bazı durumlarda, ancak aday kümesi derleme zamanında bilinen:

*  Dinamik bağımsız değişkenlere sahip statik yöntem çağrıları
*  Örnek yöntemi çağrılarında nerede alıcı dinamik bir ifade değil
*  Dizin Oluşturucu çağrıları nerede alıcı dinamik bir ifade değil
*  Dinamik bağımsız değişkenlere sahip Oluşturucu çağrı

Bu gibi durumlarda, bunlardan herhangi birinin büyük olasılıkla çalışma zamanında uygulanabilir değilse görmek her adayı için sınırlı bir derleme zamanı denetimi gerçekleştirilir. Bu onay, aşağıdaki adımlardan oluşur:

*  Kısmi tür çıkarımı: Herhangi bir tür doğrudan veya dolaylı olarak türünde bir bağımsız değişken üzerinde bağlı olmayan bağımsız değişkeni `dynamic` kurallarını kullanarak algılanır [anlam çıkarma](expressions.md#type-inference). Kalan tür bağımsız değişkeni bilinmeyen.
*  Kısmi Uygulanabilirlik denetimi: Uygulanabilirlik göre denetlenir [geçerli işlev üyesi](expressions.md#applicable-function-member), ancak eşleşen türleri bilinmeyen parametreler yoksayılıyor.
*  Hiçbir aday bu testi başarılı olursa, bir derleme zamanı hatası oluşur.

### <a name="function-member-invocation"></a>İşlev üye çağrısı

Bu bölümde belirli işlev üye çağırmak için çalışma zamanında gerçekleşen işlemi açıklanmaktadır. Bağlama zamanı işlemini çağırmak için çözüm candidate işlev üyeleri bir dizi uygulayarak büyük olasılıkla aşırı belirli üye zaten belirledi varsayılır.

Çağırma işlemi açıklamak amacıyla işlev üyeleri iki kategoriye ayrılır:

*  Statik işlev üyeleri. Örnek oluşturucuları, statik yöntemler, statik özellik erişimcileri ve kullanıcı tanımlı işleçler şunlardır. Statik işlev her zaman sanal olmayan üyeleridir.
*  Örnek işlev üyeleri. Bu örnek yöntemleri, örnek özellik erişimcileri ve dizin oluşturucu erişimcileri ücretlerdir. Örnek işlev üyeleri, sanal olmayan ya da sanal ve her zaman belirli bir örneği üzerinde çağrılır. Örnek bir örnek ifade Hesaplandı ve işlev üyesi olarak içinde erişilebilir duruma gelir `this` ([bu erişim](expressions.md#this-access)).

Çalışma zamanı işleme işlevi üye çağırmanın, aşağıdaki adımlardan oluşur burada `M` işlevi üyesidir ve `M` bir örnek üyesi `E` örnek ifade:

*  Varsa `M` statik işlev üye:
   * Bağımsız değişken listesi açıklandığı gibi değerlendirilir [bağımsız değişken listeleri](expressions.md#argument-lists).
   * `M` çağrılır.

*  Varsa `M` örnek işlevi üyesine içinde bildirilen bir *value_type*:
   * `E` değerlendirilir. Bu değerlendirme bir özel durum neden olursa, başka bir adım daha sonra yürütülür.
   * Varsa `E` bir değişken, ardından geçici bir yerel değişken sınıflandırılmaz `E`ın türü oluşturulur ve değerini `E` bu değişkenine atanır. `E` Ardından, geçici yerel değişken başvuru olarak yeniden sınıflandırılması. Geçici değişken olarak erişilebilir `this` içinde `M`, ancak başka hiçbir şekilde. Bu nedenle, yalnızca zaman `E` true değişkeni çağırıcı değişiklikleri gözlemlemek için mümkün olduğu, `M` yapar `this`.
   * Bağımsız değişken listesi açıklandığı gibi değerlendirilir [bağımsız değişken listeleri](expressions.md#argument-lists).
   * `M` çağrılır. Tarafından başvurulan değişken `E` tarafından başvurulan değişken olur `this`.

*  Varsa `M` örnek işlevi üyesine içinde bildirilen bir *reference_type*:
   * `E` değerlendirilir. Bu değerlendirme bir özel durum neden olursa, başka bir adım daha sonra yürütülür.
   * Bağımsız değişken listesi açıklandığı gibi değerlendirilir [bağımsız değişken listeleri](expressions.md#argument-lists).
   * Varsa türünü `E` olduğu bir *value_type*, paketleme dönüştürmesi ([kutulama dönüştürmeler](types.md#boxing-conversions)) dönüştürmek için gerçekleştirilen `E` türüne `object`, ve `E` olarak kabul edilir türünde olmasını `object` aşağıdaki adımlarda. Bu durumda, `M` yalnızca üyesi olabilir `System.Object`.
   * Değerini `E` geçerli olması için denetlenir. Varsa değerini `E` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.
   * Çağrılacak işlev üyesi uygulama belirlenir:
     * Bağlama zamanı türü `E` bir arabirim, çağrılacak işlev üye uygulamasıdır `M` tarafından başvurulan örnek çalışma zamanı türü tarafından sağlanan `E`. Bu işlev üye arabirim eşleme kurallarını uygulayarak belirlenir ([arabirim eşleme](interfaces.md#interface-mapping)) uygulamasını belirlemek için `M` tarafından başvurulan örnek çalışma zamanı türü tarafından sağlanan `E`.
     * Aksi takdirde `M` sanal işlev üyesi olduğu çağrılacak işlev üye uygulamasıdır `M` tarafından başvurulan örnek çalışma zamanı türü tarafından sağlanan `E`. Bu işlev üye en türetilen uygulamaya belirlemek için kuralları uygulayarak belirlenir ([sanal yöntemleri](classes.md#virtual-methods)), `M` tarafından başvurulan örnek çalışma zamanı türüne göre `E`.
     * Aksi takdirde, `M` sanal olmayan işlev üyesi olduğu ve çağrılacak işlev üyesidir `M` kendisi.
   * Yukarıdaki adımda belirlenen işlev üyesi uygulama çağrılır. Tarafından başvurulan nesne `E` tarafından başvurulan nesne haline gelir `this`.

#### <a name="invocations-on-boxed-instances"></a>Paketlenmiş örneklerinde etkinleştirmeleri

İşlev üyesi uygulanan bir *value_type* , söz konusu kutulanmış örneği çağrılabilir *value_type* aşağıdaki durumlarda:

*  İşlev üyesi olduğunda bir `override` türden devralınan bir yöntemin `object` ve örnek türündeki bir ifade, çağrılan `object`.
*  Ne zaman işlevi üye işlevi arabirim üyesini uygulamasıdır ve bir örnek ifade çağrılan bir *INTERFACE_TYPE*.
*  Ne zaman işlev üyesi bir temsilci çağrılır.

Bu gibi durumlarda paketlenmiş örneği içeren bir değişken değerlendirilir *value_type*, ve bu değişken tarafından başvurulan değişken `this` işlevi üye çağrısı içinde. Özellikle, bu işlev üyesi paketlenmiş bir örneğinde çağrıldığında, kutulanmış örneğinde yer alan değeri değiştirmek işlev üyesi mümkündür, anlamına gelir.

## <a name="primary-expressions"></a>Birincil ifadeler

Birincil ifadeler basit formlar ifadeleri içerir.

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

Birincil ifadeler arasında bölünen *array_creation_expression*s ve *primary_no_array_creation_expression*s. Dizi oluşturma ifadesi bu şekilde kullanarak, diğer basit ifade forms birlikte listelemek yerine olası karmaşık kod gibi izin vermeyecek şekilde dilbilgisi sağlar
```csharp
object o = new int[3][1];
```
hangi olarak yoksa yorumlanacağını
```csharp
object o = (new int[3])[1];
```

### <a name="literals"></a>Sabit değerler

A *primary_expression* , oluşur bir *değişmez değer* ([değişmez değerleri](lexical-structure.md#literals)) bir değer olarak sınıflandırılır.


### <a name="interpolated-strings"></a>Ara değerli dizeler

Bir *interpolated_string_expression* oluşan bir `$` burada görüntülerle boşluklarını, virgülle ayrılmış bir normal veya verbatim değişmez dize değeri tarafından izlenen oturum `{` ve `}`ifadeleri alın ve biçimlendirme belirtimleri. İlişkilendirilmiş dize ifadesi sonucu olan bir *interpolated_string_literal* , parçalanmış bireysel belirteçlere açıklandığı [ilişkilendirilmiş dize değişmez değerleri](lexical-structure.md#interpolated-string-literals).

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

*Constant_expression* içinde bir ilişkilendirme için örtük bir dönüştürme olmalıdır `int`.

Bir *interpolated_string_expression* bir değer olarak sınıflandırılır. İçin hemen dönüştürülürse `System.IFormattable` veya `System.FormattableString` örtük ilişkilendirilmiş dize dönüştürme ([örtük ilişkilendirilmiş dize dönüştürme](conversions.md#implicit-interpolated-string-conversions)), ilişkilendirilmiş dize ifadesi türü vardır. Aksi takdirde, bu türün sahip `string`.

İlişkilendirilmiş dize türünde ise `System.IFormattable` veya `System.FormattableString`, bir çağrıdır anlamı `System.Runtime.CompilerServices.FormattableStringFactory.Create`. Tür ise `string`, bir çağrıdır ifadesinin anlamını `string.Format`. Her iki durumda da, her bir ilişkilendirme için yer tutucular ve bağımsız değişken için yer tutucu karşılık gelen her ifade için hazır bir biçim dizesi çağrısının bağımsız değişken listesi oluşur.

Biçim dize sabit değeri şu şekilde oluşturulur burada `N` içinde ilişkilendirme sayısı *interpolated_string_expression*:

*  Bir *interpolated_regular_string_whole* veya bir *interpolated_verbatim_string_whole* izleyen `$` Bu belirteci biçimi dize sabit değeri ise, oturum açın.
*  Aksi takdirde, biçim dize sabit değeri şunlardan oluşur: 
   *  İlk *interpolated_regular_string_start* veya *interpolated_verbatim_string_start*
   *  Sonra her numarası için `I` gelen `0` için `N-1`: 
      * Ondalık temsili `I`
      * Ardından, eğer karşılık gelen *ilişkilendirme* sahip bir *constant_expression*, `,` (ayrılmış) değerini ondalık gösterimini tarafından izlenen *constant_expression*
      * Ardından *interpolated_regular_string_mid*, *interpolated_regular_string_end*, *interpolated_verbatim_string_mid* veya *interpolated_ verbatim_string_end* karşılık gelen ilişkilendirme hemen ardından.

Yalnızca sonraki bağımsız değişkenler *ifadeleri* gelen *ilişkilendirme* (varsa), sırasıyla.

TODO: örnekler.


### <a name="simple-names"></a>Basit adları

A *simple_name* isteğe bağlı olarak bir tür bağımsız değişken listesi tarafından izlenen bir tanımlayıcı oluşur:

```antlr
simple_name
    : identifier type_argument_list?
    ;
```

A *simple_name* herhangi formunun `I` veya formun `I<A1,...,Ak>`burada `I` tek bir tanımlayıcıdır ve `<A1,...,Ak>` isteğe bağlıdır *type_argument_list*. Hiçbir *type_argument_list* olduğundan belirtilen göz önünde bulundurun `K` sıfır olmalıdır. *Simple_name* değerlendirilir ve şu şekilde sınıflandırılan:

*  Varsa `K` sıfırdır ve *simple_name* içinde görünür bir *blok* ve *blok*'s (veya bir kapsayan *blok*ait) yerel değişken bildirimi alanı ([bildirimleri](basic-concepts.md#declarations)) bir yerel değişken, parametre veya adla sabiti içeren `I`, ardından *simple_name* , yerel bir değişkene başvuruyor parametresi veya sabit ve değişken veya değer sınıflandırılır.
*  Varsa `K` sıfırdır ve *simple_name* bir genel yöntem bildirimi gövdesi içinde görüntülenir ve bu bildirimi bir tür parametresi adı ile içeriyorsa `I`, ardından *simple_name*bu tür parametresine başvuruyor.
*  Aksi durumda, her örnek türü için `T` ([örnek türü](classes.md#the-instance-type)), örnek türü kapsayan tür bildirimi ile başlayan ve her kapsayan sınıfın veya yapının örneği türüyle devam bildiriminin (varsa):
   *  Varsa `K` sıfır ve bildirimi `T` ada sahip bir tür parametresi içeren `I`, ardından *simple_name* bu tür parametresine başvuran.
   *  Aksi takdirde, bir üye araması ([üye araması](expressions.md#member-lookup)), `I` içinde `T` ile `K`  tür bağımsız değişkeni bir eşleşme üretir:
      * Varsa `T` kapsayan sınıf veya yapı türü örneği türüdür ve Ara bir tanımlayan veya hakkında daha fazla yöntem, sonucu bir ilişkili örnek ifade olan bir yöntem grubu olduğundan `this`. Tür bağımsız değişken listesi belirtilmişse genel yöntem arayan kullanılır ([yöntem çağrıları](expressions.md#method-invocations)).
      * Aksi takdirde `T` arama bir örnek üyesi tanımlıyorsa ve başvuru örnek oluşturucusu, bir örnek yöntemi veya örneği erişimci gövdesi içinde ortaya çıkarsa kapsayan sınıf veya yapı türü örneği türü değil Sonuç üye erişimi ile aynı olur ([üye erişimi](expressions.md#member-access)) biçiminde `this.I`. Bu durum yalnızca oluşabilir olduğunda `K` sıfırdır.
      * Aksi halde, sonuç bir üye erişimi ile aynı olur ([üye erişimi](expressions.md#member-access)) biçiminde `T.I` veya `T.I<A1,...,Ak>`. Bu durumda, bunun için bir bağlama zamanı hatasına neden olur *simple_name* bir örnek üyesine başvurma.

*  Aksi durumda, her ad alanı için `N`, ad alanı ile başlayan *simple_name* devam her ad alanı (varsa) kapsayan ve bitiş genel ad alanı ile birlikte aşağıdaki adımlardan oluşur bir varlık bulunana kadar değerlendirilir:
   *  Varsa `K` sıfırdır ve `I` bir ad alanındaki adı `N`, ardından:
      * Varsa konumu burada *simple_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive* veya  *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *simple_name* belirsiz ve bir derleme zamanı hatası oluşur.
      * Aksi takdirde, *simple_name* adlı isim uzayına başvuruyor `I` içinde `N`.
   *  Aksi takdirde `N` adına sahip bir erişilebilir türü içeren `I` ve `K`  tür parametreleri:
      * Varsa `K` sıfır ve konumun burada *simple_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive*veya *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *simple_name* belirsiz ve bir derleme zamanı hatası oluşur.
      * Aksi takdirde, *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.
   *  Aksi halde, konum burada *simple_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N`:
      * Varsa `K` sıfırdır ve ad alanı bildirimi içeren bir *extern_alias_directive* veya *using_alias_directive* , ad ilişkilendirir `I` bir içeri aktarılan ad alanıyla veya tür, ardından *simple_name* bu ad alanı veya tür ifade eder.
      * Aksi takdirde, ad alanları ve tür bildirimleri tarafından aldıysanız *using_namespace_directive*s ve *using_static_directive*ad alanı bildiriminin bir s içeren tam olarak bir erişilebilir türü veya Uzantı olmayan statik üye adına sahip `I` ve `K`  tür parametrelerindeki, ardından *simple_name* bu türe veya üyeye belirli tür bağımsız değişkenleri ile oluşturulmuş ifade eder.
      * Aksi takdirde ad alanları ve türler tarafından alınan *using_namespace_directive*ad alanı bildiriminin bir s içeren birden fazla erişilebilir türü veya genişletme yöntemi statik üye adına sahip `I` ve `K`  tür parametrelerindeki, ardından *simple_name* belirsiz ve hata oluşur.

   Tüm bu adımı tam olarak paralel işlenmesini karşılık gelen adımına olduğuna dikkat edin bir *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)).

*  Aksi takdirde, *simple_name* olup tanımsız ve bir derleme zamanı hatası oluşur.


### <a name="parenthesized-expressions"></a>Parantezli İfade

A *parenthesized_expression* oluşan bir *ifade* parantez içine alınmış.

```antlr
parenthesized_expression
    : '(' expression ')'
    ;
```

A *parenthesized_expression* değerlendirilerek hesaplanan *ifade* parantez içinde. Varsa *ifade* parantez içinde bir ad alanı veya tür, bir derleme zamanı hatası oluşur gösterir. Aksi takdirde, sonucunu *parenthesized_expression* içerdiği için değerlendirme sonucu *ifade*.

### <a name="member-access"></a>Üye erişimi

A *member_access* oluşan bir *primary_expression*, *predefined_type*, veya bir *qualified_alias_member*"çizgidir`.`"belirteci, izleyen bir *tanımlayıcı*ve ardından isteğe bağlı olarak bir *type_argument_list*.

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

*Qualified_alias_member* üretim tanımlanmış [Namespace diğer adı niteleyicileri](namespaces.md#namespace-alias-qualifiers).

A *member_access* herhangi formunun `E.I` veya formun `E.I<A1, ..., Ak>`burada `E` olan bir birincil ifade, `I` tek bir tanımlayıcıdır ve `<A1, ..., Ak>` isteğe bağlıdır  *type_argument_list*. Hiçbir *type_argument_list* olduğundan belirtilen göz önünde bulundurun `K` sıfır olmalıdır.

A *member_access* ile bir *primary_expression* türü `dynamic` dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda üye erişimi derleyici özellik erişimi türü sınıflandırır `dynamic`. Anlamını belirlemek için aşağıdaki kuralları *member_access* çalışma zamanında çalışma zamanı tür yerine derleme zamanı türü kullanarak, daha sonra uygulanır *primary_expression*. Bu çalışma zamanı sınıflandırma için bir yöntem grubu müşteri adayları sonra üye erişimi olmalıdır *primary_expression* , bir *invocation_expression*.

*Member_access* değerlendirilir ve şu şekilde sınıflandırılan:

*  Varsa `K` sıfırdır ve `E` bir ad alanı ve `E` ada sahip iç içe geçmiş bir ad alanı içerir `I`, sonra bu ad alanı oluşur.
*  Aksi takdirde `E` bir ad alanı ve `E` adına sahip bir erişilebilir türü içeren `I` ve `K`  sonucu belirli tür bağımsız değişkenleri ile oluşturulan türü ise parametreleri yazın.
*  Varsa `E` olduğu bir *predefined_type* veya *primary_expression* , bir tür olarak sınıflandırılan `E` bir tür parametresi değil ve üye araması ([üyearaması](expressions.md#member-lookup)) ' ın `I` içinde `E` ile `K`  tür parametreleri üreten bir eşleşme ardından `E.I` değerlendirilir ve şu şekilde sınıflandırılan:
   *  Varsa `I` sonucu belirli tür bağımsız değişkenleri ile oluşturulan türü ise bir tür tanımlar.
   *  Varsa `I` sonucu bir yöntem grubu ile ilişkili örnek ifade yok ise bir veya daha fazla yöntemleri tanımlar. Tür bağımsız değişken listesi belirtilmişse genel yöntem arayan kullanılır ([yöntem çağrıları](expressions.md#method-invocations)).
   *  Varsa `I` tanımlayan bir `static` özelliği ve ardından sonucu olan bir özellik erişimi ile ilişkilendirilmiş ifade yok.
   *  Varsa `I` tanımlayan bir `static` alan:
      * Alan ise `readonly` ve başvuru sınıfın veya yapının alan bildirilmiş statik Oluşturucu dışında gerçekleşir ve ardından bir değeri, yani statik alanının sonucudur `I` içinde `E`.
      * Aksi takdirde, bir değişken, yani statik alan sonucudur `I` içinde `E`.
   *  Varsa `I` tanımlayan bir `static` olay:
      * Başvuru sınıfı veya olay bildirilir ve olayı olmadan bildirildi yapı içinde oluşursa *event_accessor_declarations* ([olayları](classes.md#events)), ardından `E.I` tam olarak işlenir gibi `I` statik alan bırakıldı.
      * Aksi takdirde, hiçbir ilişkilendirilmiş ifade ile bir olay erişimini sonucudur.
   *  Varsa `I` bir sabiti sonuç değeri, yani bu sabitin değeri ise tanımlar.
    * Varsa `I` sonucu bir değeri, yani bu sabit listesi üyesi ise, bir numaralandırma üyesine tanımlar.
    * Aksi takdirde, `E.I` geçersiz üye başvuru ve bir derleme zamanı hatası oluşur.
*  Varsa `E` özellik erişimi, dizin oluşturucu erişim, değişken veya değer türü, `T`ve bir üye araması ([üye araması](expressions.md#member-lookup)), `I` içinde `T` ile `K`  tür bağımsız değişkenleri üreten bir eşleşme ardından `E.I` değerlendirilir ve şu şekilde sınıflandırılan:
   *  İlk olarak, eğer `E` bir özellik veya dizin oluşturucu erişim sonra özelliğin değerini ya da dizin oluşturucu erişim elde edilir ([ifadelerin değerleri](expressions.md#values-of-expressions)) ve `E` bir değer olarak yeniden sınıflandırılması.
   *  Varsa `I` sonucu bir ilişkili örnek ifade olan bir yöntem grubu ise bir veya daha fazla yöntemleri tanımlayan `E`. Tür bağımsız değişken listesi belirtilmişse genel yöntem arayan kullanılır ([yöntem çağrıları](expressions.md#method-invocations)).
   *  Varsa `I` bir Instance özelliği tanımlar.
      * Varsa `E` olduğu `this`, `I` otomatik olarak uygulanan bir özellik tanımlar ([Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties)) için bir örnek oluşturucusu içinde bir ayarlayıcı ve başvuru olmadan bir sınıf veya yapı türü `T`, sonucu bir değişken, yani otomatik-özellik tarafından verilen gizli destek alanı ise `I` örneğinde `T` tarafından verilen `this`.
      * Aksi halde, sonuç ilişkili örnek bir ifade olan bir özellik erişimi, `E`.
   *  Varsa `T` olduğu bir *class_type* ve `I` tanımlar, bir örnek alan *class_type*:
      * Varsa değerini `E` olduğu `null`, ardından bir `System.NullReferenceException` oluşturulur.
      * Aksi durumda, alan ise `readonly` ve başvuru alan bildirilmiş sınıfının örnek oluşturucusu dışında gerçekleşir ve ardından bir değer, yani bir alanın değerini sonucudur `I` başvurduğu nesnede `E`.
      * Aksi takdirde, bir değişken, yani alanın sonucudur `I` başvurduğu nesnede `E`.
   *  Varsa `T` olduğu bir *struct_type* ve `I` tanımlar, bir örnek alan *struct_type*:
      * Varsa `E` bir değer veya alan ise `readonly` ve başvuru alan bildirilmiş struct'ın örnek oluşturucusu dışında gerçekleşir ve ardından bir değer, yani bir alanın değerini sonucudur `I` tarafından verilen yapı örneğinde  `E`.
      * Aksi takdirde, bir değişken, yani alanın sonucudur `I` tarafından verilen yapı örneğinde `E`.
   *  Varsa `I` tanımlayan bir örnek olay:
      * Başvuru sınıfı veya olay bildirilir ve olayı olmadan bildirildi yapı içinde oluşursa *event_accessor_declarations* ([olayları](classes.md#events)), ve başvuru olarak gerçekleşmez sol tarafında bir `+=` veya `-=` işleci, ardından `E.I` tam olarak işlenen gibi `I` örnek alanı.
      * Aksi halde, sonuç ilişkili örnek bir ifade olan bir olay erişimini, `E`.
*  Aksi takdirde, bir işlem için girişimde `E.I` olarak bir uzantı metodu çağırma ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)). Bu başarısız olursa, `E.I` geçersiz üye başvuru ve bir bağlama zamanı hatası oluşur.

#### <a name="identical-simple-names-and-type-names"></a>Basit aynı adlar ve tür adları

Üye erişimi formun içinde `E.I`, `E` tek bir tanımlayıcı ve anlamını `E` olarak bir *simple_name* ([basit adları](expressions.md#simple-names)) olan bir sabit, alan, özelliği yerel değişken veya parametre olarak anlamı, aynı türde `E` olarak bir *type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)), ardından her iki olası anlamı `E` olan izin verilir. İki olası anlamı `E.I` hiçbir zaman bu yana belirsiz olan `I` mutlaka türünün bir üyesi olmalıdır `E` her iki durumda. Diğer bir deyişle, kural yalnızca statik üyeleri ve iç içe geçmiş türleri erişimine `E` burada bir derleme zamanı hatası Aksi halde oluşmuş. Örneğin:
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

Üretime *simple_name* ([basit adları](expressions.md#simple-names)) ve *member_access* ([üye erişimi](expressions.md#member-access)) ortaya çıkmasına neden belirsizliklerden verebilirsiniz Dilbilgisi ifadeler için. Örneğin, ifade:
```
F(G<A,B>(7));
```
bir çağrı olarak yorumlanabilecek `F` iki bağımsız değişkeni ile `G < A` ve `B > (7)`. Alternatif olarak, bunu bir çağrı olarak yorumlanabilecek `F` bir bağımsız değişken ile genel yöntem çağrısına olduğu `G` iki tür bağımsız değişkenleri ve normal bir bağımsız değişken.

Bir dizi belirteçleri (bağlamda) olarak ayrıştırılabilir varsa bir *simple_name* ([basit adları](expressions.md#simple-names)), *member_access* ([üye erişimi](expressions.md#member-access)), veya *pointer_member_access* ([işaretçi üye erişimi](unsafe-code.md#pointer-member-access)) ile biten bir *type_argument_list* ([tür bağımsız değişkenleri](types.md#type-arguments)), Kapanış hemen ardından belirteci `>` belirteci incelenir. Biri ise
```csharp
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```
ardından *type_argument_list* parçası olarak korunur *simple_name*, *member_access* veya *pointer_member_access* ve tüm diğer olası ayrıştırma belirteçleri dizisini göz ardı edilir. Aksi takdirde, *type_argument_list* parçası olarak kabul edilmemektedir *simple_name*, *member_access* veya *pointer_member_access*, hiçbir diğer olası ayrıştırma belirteçleri dizisini olsa bile. Bu kurallar Not uygulanan ayrıştırılırken bir *type_argument_list* içinde bir *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)). Deyimi
```csharp
F(G<A,B>(7));
```
Bu kural göre bir çağrı olarak yorumlanır `F` bir bağımsız değişken ile genel yöntem çağrısına olduğu `G` iki tür bağımsız değişkenleri ve normal bir bağımsız değişken. Deyimleri
```csharp
F(G < A, B > 7);
F(G < A, B >> 7);
```
Her bir çağrı olarak yorumlanacaktır `F` iki bağımsız değişken. Deyimi
```csharp
x = F < A > +y;
```
deyim yazılmış gibi işleç, işleç ve tek işlenenli artı işleci, büyük bir küçüktür olarak yorumlanacaktır `x = (F < A) > (+y)`, yerine olarak bir *simple_name* ile bir *type_argument_list* bir ikili artı işleci tarafından izlenen. Deyimi
```csharp
x = y is C<T> + z;
```
belirteçleri `C<T>` olarak yorumlanan bir *namespace_or_type_name* ile bir *type_argument_list*.

### <a name="invocation-expressions"></a>Çağrı ifadeleri

Bir *invocation_expression* bir yöntem çağırmak için kullanılır.

```antlr
invocation_expression
    : primary_expression '(' argument_list? ')'
    ;
```

Bir *invocation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)) aşağıdakilerden en az birini tutuyorsa:

* *Primary_expression* derleme zamanı türü `dynamic`.
* En az bir bağımsız değişken isteğe bağlı *argument_list* derleme zamanı türü `dynamic` ve *primary_expression* bir temsilci türü yok.

Bu durumda, derleyici sınıflandırır *invocation_expression* türünde bir değer olarak `dynamic`. Anlamını belirlemek için aşağıdaki kuralları *invocation_expression* çalışma zamanında derleme zamanı türü ihtiyaçlarını yerine çalışma zamanı türü kullanarak, daha sonra uygulanır *primary_expression* ve derleme zamanı türü olan bağımsız değişkenler `dynamic`. Varsa *primary_expression* derleme zamanı tür yok `dynamic`, yöntem çağırma açıklandığı gibi sınırlı bir derleme zamanı denetimi uğradığında sonra [derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

*Primary_expression* , bir *invocation_expression* bir yöntem grubu ya da bir değeri olmalıdır bir *delegate_type*. Varsa *primary_expression* bir yöntem grubu olduğundan *invocation_expression* yöntemi bir çağrıdır ([yöntem çağrıları](expressions.md#method-invocations)). Varsa *primary_expression* bir değeri bir *delegate_type*, *invocation_expression* temsilci bir çağrıdır ([temsilci çağrılarını](expressions.md#delegate-invocations)). Varsa *primary_expression* ne bir yöntem grubu ne de değeri bir *delegate_type*, bir bağlama zamanı hatası oluşur.

İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yönteminin parametreleri için değerleri veya değişken başvuruları sağlar.

Değerlendirme sonucu bir *invocation_expression* şu şekilde sınıflandırılır:

*  Varsa *invocation_expression* bir yöntem veya döndüren bir temsilci çağıran `void`, hiçbir şey sonucudur. Hiçbir şey yalnızca bağlamında izin verilir olarak sınıflandırılmış bir ifade bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) veya gövdesi bir *lambda_expression*([Anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)). Aksi halde bir bağlama zamanı hatası oluşur.
*  Aksi halde, sonuç bir yöntem veya temsilci tarafından döndürülen tür değeri.

#### <a name="method-invocations"></a>Yöntem çağrıları

Bir yöntem çağırma için *primary_expression* , *invocation_expression* bir yöntem grubu olması gerekir. Yöntem grubu çağrılacak bir yöntem veya belirli bir yöntemi çağırmak için seçilecek aşırı yüklenmiş yöntemler kümesini tanımlar. İkinci durumda, belirli bir yöntemi çağırmak için belirlenmesi bağımsız değişken türleri tarafından sağlanan bağlam bağlıdır *argument_list*.

Bağlama zamanı işleme biçiminde bir yöntem çağrısının `M(A)`burada `M` bir yöntem grubu olduğundan (belki de dahil olmak üzere bir *type_argument_list*), ve `A` isteğe bağlıdır *argument_ Liste*, aşağıdaki adımlardan oluşur:

*  Yöntem çağırma için aday yöntemleri kümesini oluşturulur. Her bir yöntemin `F` yöntem grubu ile ilişkili `M`:
   *  Varsa `F` genel olmayan, olan `F` bir adaydır olduğunda:
      * `M` hiçbir tür bağımsız değişken listesi vardır ve
      * `F` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).
   *  Varsa `F` geneldir ve `M` hiçbir tür bağımsız değişken listesi vardır `F` bir adaydır olduğunda:
      * Anlam çıkarma ([anlam çıkarma](expressions.md#type-inference)) başarılı, tür bağımsız değişkenleri için arama listesini çıkarımını yapma ve
      * İlgili yöntem tür parametreleri için çıkarsanan tür bağımsız değişkenlerini başvurulduğunda sonra parametre listesindeki tüm oluşturulan türler f kendi kısıtlamaları karşılamak ([kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints)) ve parametrelistesi`F` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).
   *  Varsa `F` geneldir ve `M` türü bağımsız değişken listesi içeren `F` bir adaydır olduğunda:
      * `F` tür bağımsız değişken listesinde sağlanan gibi aynı sayıda yöntem tür parametreleri vardır ve
      * Tür bağımsız değişkenleri karşılık gelen yöntemi tür parametreleri için başvurulduğunda sonra parametre listesindeki tüm oluşturulan türler f kendi kısıtlamaları karşılamak ([kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints)) ve parametre listesini `F` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)).
*  Aday yöntemleri kümesini yalnızca en çok türetilen türler yöntemlerinden içerecek şekilde azalır: Her bir yöntemin `C.F` kümesindeki burada `C` hangi tür yöntemi `F` bildirilen, tüm yöntemler temel bir türde bildirilen `C` kümesinden kaldırılır. Ayrıca, varsa `C` bir sınıf türü dışındaki: `object`, bir arabirim türü içinde bildirilen tüm yöntemler kümesinden kaldırılır. (Yöntem grubu dışındaki nesne etkin bir temel sınıf ve ayarlanmış bir boş olmayan etkin arabirimi sahip bir tür parametresi bir üye araması sonucu olduğunda bu ikinci kuralı yalnızca bir etkisi yoktur.)
*  Aday yöntemleri sonuç kümesi boşsa, aşağıdaki adımları başka işlem terk ve bunun yerine bir çağrı bir uzantı metodu çağırma olarak işlemek için girişimde ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)). Bu başarısız olursa, ardından hiçbir uygun yöntemleri kayıtlı ve bir bağlama zamanı hatası oluşur.
*  Aday yöntemleri kümesinin en iyi yöntem aşırı yükleme çözünürlüğü kurallarını kullanarak tanımlanır [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution). Tek bir en iyi yöntem tanımlanamaz, yöntem çağırma belirsiz ve bir bağlama zamanı hatası oluşur. Aşırı yükleme çözümlemesi yaparken bir genel yöntem parametreleri (sağlanan veya çıkarsanan) tür bağımsız değişkenlerini değiştirerek sonra için karşılık gelen yöntem türü parametreleri olarak kabul edilir.
*  Seçilen en iyi yöntem, nihai doğrulama gerçekleştirilir:
   * Yöntem, yöntem grubuna bağlamında doğrulanır: En iyi yöntem statik bir yöntemi ise, yöntem grubu kullanmasından gereken bir *simple_name* veya *member_access* bir tür. En iyi yöntem bir örnek yöntem ise, yöntem grubu kullanmasından gereken bir *simple_name*, *member_access* bir değişken veya değer aracılığıyla veya *base_access*. Bu gereksinimleri hiçbiri true ise, bir bağlama zamanı hatası oluşur.
   * En iyi yöntem genel bir yöntem ise (sağlanan veya çıkarsanan) tür bağımsız değişkenlerini yönelik kısıtlamalar denetlenir ([kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints)) genel metodunda bildirilen. Herhangi bir tür bağımsız değişkeni tür parametresi üzerinde karşılık gelen Kısıt karşılamaz, bir bağlama zamanı hatası meydana gelir.

Bir yöntem seçili ve Yukarıdaki adımlarla bağlama zamanında doğrulanmış sonra gerçek çalışma zamanı çağırma işlevi üye çağrısının açıklanan kurallara göre işlenir [derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor ](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Yukarıda açıklanan çözümleme kurallarını sezgisel etkisini aşağıdaki gibidir: Bir yöntem çağrısı tarafından çağrılan yöntemin belirli bulmak için yöntem çağırma tarafından gösterilen türü ile başlayın ve en az bir geçerli, erişilebilir, geçersiz kılma olmayan yöntem bildiriminde bulunana kadar devralma zincirini devam edin. Ardından tür çıkarımı gerçekleştirin ve aşırı yükleme çözümlemesi, türde bildirilen geçerli, erişilebilir, geçersiz kılma olmayan yöntemler kümesi üzerinde ve bu nedenle seçili yöntemi çağır. Hiçbir yöntemi bulunamadı, bunun yerine çağrı bir uzantı metodu çağırma olarak işlenecek deneyin.

#### <a name="extension-method-invocations"></a>Uzantı yöntem çağrıları

Bir yöntem çağrısı ([çağrılarını paketlenmiş örneklerinde](expressions.md#invocations-on-boxed-instances)) biçimlerden biri
```csharp
expr . identifier ( )

expr . identifier ( args )

expr . identifier < typeargs > ( )

expr . identifier < typeargs > ( args )
```
normal işleme çağrısının hiçbir uygun yöntemleri bulursa, bir yapı bir uzantı metodu çağırma olarak işlemek için girişimde. Varsa *expr* veya herhangi bir *args* derleme zamanı türü `dynamic`, genişletme yöntemleri geçerli değil.

Amaç en iyi bulmaktır *type_name* `C`, böylece karşılık gelen statik yöntem çağırma gerçekleşir:
```csharp
C . identifier ( expr )

C . identifier ( expr , args )

C . identifier < typeargs > ( expr )

C . identifier < typeargs > ( expr , args )
```

Bir genişletme yöntemi `Ci.Mj` olduğu ***uygun*** varsa:

*  `Ci` Genel olmayan, iç içe bir sınıftır
*  Adını `Mj` olduğu *tanımlayıcısı*
*  `Mj` erişilebilir ve bağımsız değişkenler için yukarıda gösterildiği gibi bir statik yöntem olarak uygulandığında uygulanabilir
*  Gelen bir örtük kimlik, başvuru veya paketleme dönüştürmesi var *expr* ilk parametresinin türü için `Mj`.

Arama `C` aşağıdaki gibi çalışır:

*  Her kapsayan ad alanı bildirimi ile devam etmesini ve içeren derleme birimi ile biten en yakın kapsayan ad alanı bildirimi, başlayarak art arda denemeler genişletme yöntemleri aday kümesini bulmak için oluşturulur:
   * Belirtilen ad alanı veya derleme birimi genel olmayan tür bildirimleri doğrudan içerip içermediğini `Ci` uygun genişletme yöntemleri ile `Mj`, sonra da bu genişletme yöntemleri kümesini aday kümesidir.
   * Varsa türleri `Ci` tarafından alınan *using_static_declarations* ve doğrudan tarafından içeri aktarılan ad alanlarını bildirilen *using_namespace_directive*belirtilen ad alanı veya derleme biriminde doğrudan s uygun bir uzantı yöntemlerini içeren `Mj`, sonra da bu genişletme yöntemleri kümesini aday kümesidir.
*  Aday ayarlanmadı kapsayan tüm ad alanı bildirimi veya derleme biriminde bulunursa, bir derleme zamanı hatası oluşur.
*  Aksi takdirde, aşırı yükleme çözünürlüğü açıklanan aday uygulanır ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)). Tek bir en iyi yöntem bulunursa, bir derleme zamanı hatası oluşur.
*  `C` bir genişletme yöntemi bildirilen en iyi yöntem içinde türüdür.

Kullanarak `C` bir hedef olarak yöntem çağrısından sonra bir statik yöntem çağırma işlenir ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)).

Önceki kuralların ortalama örnek yöntemleri genişletme yöntemleri üzerinde önceliklidir, iç ad alanı bildirimi uzantı yöntemleri genişletme yöntemleri dış ad alanı bildirimi ve bu uzantı üzerinde öncelik kazanır doğrudan bir ad alanında bildirilen yöntemler önceliklidir genişletme yöntemleri kullanarak, aynı ad alanı içeri aktarılan ad alanı yönergesi. Örneğin:
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

Örnekte, `B`'s yöntemi ilk genişletme yöntemi göre önceliklidir ve `C`'s yöntemi her iki uzantı yöntemleri göre önceliklidir.

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

Bu örneğin çıktısı şöyledir:
```
E.F(1)
D.G(2)
C.H(3)
```
`D.G` öncelikli `C.G`, ve `E.F` hem de önceliklidir `D.F` ve `C.F`.

#### <a name="delegate-invocations"></a>Temsilci çağrılarını

Bir temsilci çağrısı için *primary_expression* , *invocation_expression* değeri olmalıdır bir *delegate_type*. Ayrıca, dikkate *delegate_type* işlevi üye aynı parametre listesiyle *delegate_type*, *delegate_type* uygulanabilir (olması gerekir [Geçerli işlev üyesi](expressions.md#applicable-function-member)) ile ilişkilendirilebilmesi için *argument_list* , *invocation_expression*.

Bir temsilci çağrısı formun çalışma zamanı işlenmesini `D(A)`burada `D` olduğu bir *primary_expression* , bir *delegate_type* ve `A` isteğe bağlı bir olan*argument_list*, aşağıdaki adımlardan oluşur:

*  `D` değerlendirilir. Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.
*  Değerini `D` geçerli olması için denetlenir. Varsa değerini `D` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.
*  Aksi takdirde, `D` temsilci örneği bir başvurudur. İşlev çağrılarını üyesi ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) temsilcinin çağırma listesine çağrılabilir varlıkların her biri üzerinde gerçekleştirilir. Bir örnek ve örnek yöntemi oluşan çağrılabilir varlıklar için örnek çağrısına çağrılabilir varlıkta içerilen örneğidir.

### <a name="element-access"></a>Öğe erişimi

Bir *element_access* oluşan bir *primary_no_array_creation_expression*ve ardından bir "`[`" belirteci, izleyen bir *argument_list*ve ardından bir " `]`"belirteci. *Argument_list* bir veya daha fazla oluşur *bağımsız değişken*virgülle ayırarak, s.

```antlr
element_access
    : primary_no_array_creation_expression '[' expression_list ']'
    ;
```

*Argument_list* , bir *element_access* içermesine izin verilmiyor `ref` veya `out` bağımsız değişkenler.

Bir *element_access* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)) aşağıdakilerden en az birini tutuyorsa:

* *Primary_no_array_creation_expression* derleme zamanı türü `dynamic`.
* En az bir ifade *argument_list* derleme zamanı türü `dynamic` ve *primary_no_array_creation_expression* bir dizi türü yok.

Bu durumda, derleyici sınıflandırır *element_access* türünde bir değer olarak `dynamic`. Anlamını belirlemek için aşağıdaki kuralları *element_access* çalışma zamanında derleme zamanı türü ihtiyaçlarını yerine çalışma zamanı türü kullanarak, daha sonra uygulanır *primary_no_array_creation_expression*ve *argument_list* derleme zamanı türüne sahip ifade `dynamic`. Varsa *primary_no_array_creation_expression* derleme zamanı tür yok `dynamic`, öğe erişimi açıklandığı gibi sınırlı bir derleme zamanı denetimi uğradığında sonra [derleme zamanı dinamik denetleniyor aşırı yükleme çözümlemesi](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Varsa *primary_no_array_creation_expression* , bir *element_access* bir değeri bir *array_type*, *element_access* olduğu bir dizi erişim ([dizi erişim](expressions.md#array-access)). Aksi takdirde, *primary_no_array_creation_expression* bir değişken veya değeri bir sınıf, yapı veya bir veya daha fazla dizin oluşturucu üye, bu durumda olan bir arabirim türü olmalıdır *element_access* olduğu bir Dizin Oluşturucu erişim ([dizinleyici erişimi](expressions.md#indexer-access)).

#### <a name="array-access"></a>Dizi erişimi

Bir dizi erişim *primary_no_array_creation_expression* , *element_access* değeri olmalıdır bir *array_type*. Ayrıca, *argument_list* bir dizi erişim adlandırılmış bağımsız değişkenler içeren izni yok. İfadelerin sayısı *argument_list* boyut sayısı aynı olmalıdır *array_type*, ve her deyim türü olmalıdır `int`, `uint`, `long`, `ulong`, ya da bir veya daha fazla bu türleri için örtük olarak dönüştürülebilir olmalıdır.

Yani ifadelerinin içinde değerleri tarafından seçilen dizi öğesi dizinin öğe türü bir değişken bir dizi erişim değerlendirmesinin sonucu olan *argument_list*.

Dizi erişiminin formun çalışma zamanı işlenmesini `P[A]`burada `P` olduğu bir *primary_no_array_creation_expression* , bir *array_type* ve `A` bir olduğu*argument_list*, aşağıdaki adımlardan oluşur:

*  `P` değerlendirilir. Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.
*  Dizin ifadelerin *argument_list* sırayla soldan sağa doğru değerlendirilir. Her dizin ifadesi, örtük bir dönüştürme değerlendirmesinin ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) şu türlerden biri olarak gerçekleştirilir: `int`, `uint`, `long`, `ulong`. Örtük bir dönüştürme bulunduğu bu listedeki ilk türü seçilir. Örneğin, Dizin ifadesi türü ise `short` ardından örtük dönüştürmeleri `int` örtük dönüştürmelerine bu yana gerçekleştirilen `short` için `int` ve `short` için `long` mümkündür. Bir dizini ifade veya sonraki örtük dönüştürme değerinin bir özel durum neden olursa, daha fazla dizin ifadesi değerlendirilir ve başka adımlar yürütülür.
*  Değerini `P` geçerli olması için denetlenir. Varsa değerini `P` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.
*  Her bir ifadenin değerine *argument_list* gerçek sınırları her boyutunun başvurduğu dizi örneği karşılaştırılarak `P`. Bir veya daha fazla değer aralık dışında ise bir `System.IndexOutOfRangeException` oluşturulur ve başka bir adım yürütülür.
*  Dizin ifadelerini tarafından verilen bir dizi öğesinin konumunu hesaplanır ve bu konum dizi erişimi sonucunu olur.

#### <a name="indexer-access"></a>Dizinleyici erişimi

Bir dizin oluşturucu erişim *primary_no_array_creation_expression* , *element_access* bir değişkeni veya bir sınıf, yapı veya arabirim türü değeri olması gerekir ve bu tür bir veya daha fazla uygulamalıdır ilişkilendirilebilmesi için geçerli olan dizin oluşturucular *argument_list* , *element_access*.

Bir dizin oluşturucu erişim formun bağlama zamanı işlenmesini `P[A]`burada `P` olduğu bir *primary_no_array_creation_expression* bir sınıf, yapı veya arabirim türü `T`, ve `A` olduğu bir *argument_list*, aşağıdaki adımlardan oluşur:

*  Dizin oluşturucular tarafından sağlanan dizi `T` oluşturulur. Küme içinde bildirilen tüm dizin oluşturucuların oluşur `T` veya bir taban türü `T` olmayan `override` bildirimleri ve bu bağlamda erişilebilir ([üye erişimi](basic-concepts.md#member-access)).
*  Belirlenen, geçerli ve gizlenmesi bu dizin oluşturucular tarafından diğer dizin oluşturucular için azaltılır. Her dizin oluşturucu için aşağıdaki kurallar uygulanır `S.I` kümesindeki burada `S` türüdür, dizin oluşturucu `I` bildirilir:
   * Varsa `I` ilişkilendirilebilmesi için geçerli değildir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)), ardından `I` kümesinden kaldırılır.
   * Varsa `I` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)), tüm dizin oluşturucuların temel bir tür içinde bildirilirse `S` kümesinden kaldırılır.
   * Varsa `I` ilişkilendirilebilmesi için uygulanabilir `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)) ve `S` bir sınıf türü dışında: `object`, bir arabirimde bildirilen tüm dizin oluşturucuların kümesinden kaldırılır.
*  Aday dizin oluşturucular sonuç kümesi boşsa, ardından ilgili hiçbir dizin oluşturucular var ve bir bağlama zamanı hatası oluşur.
*  En iyi aday dizin oluşturucular kümesini Oluşturucusu aşırı yükleme çözünürlüğü kurallarını kullanarak tanımlanır [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution). Tek bir en iyi dizin oluşturucu tanımlanamıyor dizinleyici erişimi belirsiz ve bir bağlama zamanı hatası oluşur.
*  Dizin ifadelerin *argument_list* sırayla soldan sağa doğru değerlendirilir. Dizin Oluşturucu erişim işlemenin sonucu bir dizin oluşturucu erişim sınıflandırılmış bir ifadedir. Výraz indexeru erişim Yukarıdaki adımda belirlenen dizin oluşturucu başvuruyor ve ilişkili örneği deyimine sahip `P` ve bir ilişkili bağımsız değişken listesi `A`.

Bu kullanılır bağlama bağlı olarak bir dizin oluşturucu erişim çağrılması ya da neden olur. *get erişimcisine* veya *set erişimcisine* dizin oluşturucu. Dizin Oluşturucu erişim atama, hedef ise *set erişimcisine* yeni bir değer atamak için çağrılır ([basit atama](expressions.md#simple-assignment)). Diğer tüm durumlarda *get erişimcisine* geçerli değerini almak için çağrılır ([ifadelerin değerleri](expressions.md#values-of-expressions)).

### <a name="this-access"></a>Bu erişim

A *this_access* ayrılmış sözcükten `this`.

```antlr
this_access
    : 'this'
    ;
```

A *this_access* yalnızca izin verilen *blok* örnek oluşturucusu, bir örnek yöntemi veya bir örneği erişimcisi. Biri, aşağıdaki anlamlara sahiptir:

*  Zaman `this` kullanılan bir *primary_expression* sınıfının örnek oluşturucusu içinde bir değer olarak sınıflandırılır. Değer türü örneği türüdür ([örnek türü](classes.md#the-instance-type)) sınıfın içine girerek kullanımı gerçekleşir ve yapılandırılmakta olan nesneye bir başvuru değerdir.
*  Zaman `this` kullanılan bir *primary_expression* bir örnek yöntemi veya bir sınıfın örneği erişimci içinde bir değer olarak sınıflandırılır. Değer türü örneği türüdür ([örnek türü](classes.md#the-instance-type)), içinde kullanımı gerçekleşir ve yöntem veya erişimci çağrılan nesne başvuru değeri olduğu sınıf.
*  Zaman `this` kullanılan bir *primary_expression* bir yapı içinde bir örnek oluşturucusunda bir değişken olarak sınıflandırılır. Değişkeninin türü örnek türüdür ([örnek türü](classes.md#the-instance-type)) yapı içinde hangi kullanımı gerçekleşir ve yapılandırılmakta struct değişkeni temsil eder. `this` Değişken bir örnek oluşturucusunda bir yapının, aynı şekilde davranır bir `out` yapı türünde parametre — özellikle, bu değişken her örneğinin yürütme yolu kesinlikle atanmalıdır anlamına gelir Oluşturucu.
*  Zaman `this` kullanılan bir *primary_expression* bir örnek yöntemi veya bir yapının örneği erişimci içinde bir değişken olarak sınıflandırılır. Değişkeninin türü örnek türüdür ([örnek türü](classes.md#the-instance-type)) yapı içinde kullanımı gerçekleşir.
   * Yöntem veya erişimci bir yineleyici değilse ([yineleyiciler](classes.md#iterators)), `this` değişkenini temsil eder, yapı, yöntem veya erişimci çağrıldı ve aynı şekilde davranır bir `ref` yapı türünde parametre.
   * Bir yineleyici yöntem veya erişimci ise `this` değişken, yöntem veya erişimci çağrıldı ve tam olarak aynı yapı türünün bir değer parametresini davranışını struct'ın bir kopyasını temsil eder.

Kullanım `this` içinde bir *primary_expression* yukarıda listelenen olanlar dışındaki bir bağlamda bir derleme zamanı hatasıdır. Özellikle, başvurmak mümkün değildir `this` statik bir yöntemi, bir statik özellik erişimcisi ya da bir *variable_initializer* alanı bildirimi.

### <a name="base-access"></a>Temel erişim

A *base_access* ayrılmış sözcükten `base` tarafından izlenen bir "`.`" belirteci ve tanımlayıcısı veya bir *argument_list* köşeli ayraçlar içinde:

```antlr
base_access
    : 'base' '.' identifier
    | 'base' '[' expression_list ']'
    ;
```

A *base_access* benzer şekilde adlandırılmış üyeleri geçerli sınıf veya yapı tarafından gizlenen bir temel sınıf üyelerinin erişmek için kullanılır. A *base_access* yalnızca izin verilen *blok* örnek oluşturucusu, bir örnek yöntemi veya bir örneği erişimcisi. Zaman `base.I` bir sınıf ya da yapının gerçekleşir `I` o sınıfın veya yapının temel sınıfının üyesi belirtmek gerekir. Benzer şekilde, `base[E]` uygulanabilir bir dizin oluşturucu, temel sınıfta bulunmalıdır bir sınıf içinde gerçekleşir.

Bağlama zamanında *base_access* ifadeleri biçiminde `base.I` ve `base[E]` tam olarak yazılmış yokmuş gibi değerlendirilir `((B)this).I` ve `((B)this)[E]`burada `B` sınıfın temel sınıf ya da yapı oluştuğu yapı. Bu nedenle, `base.I` ve `base[E]` karşılık `this.I` ve `this[E]`, dışında `this` temel sınıfın bir örneği görüntülenir.

Olduğunda bir *base_access* belirlenmesi, çalışma zamanında çağrılacak üye işlevi bir sanal işlev üyesi (yöntemi, özelliği veya dizin oluşturucu) için başvurur ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetleniyor ](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) değiştirilir. Çağrılan işlev üyesi en türetilen uygulamaya bularak belirlenir ([sanal yöntemleri](classes.md#virtual-methods)) ilişkilendirilebilmesi için işlevi üyesinin `B` (yerine çalışma zamanı türü göre `this`, olarak temel olmayan Access'te normal olacaktır). Bu nedenle, içinde bir `override` , bir `virtual` üye işlevi, bir *base_access* işlevi üye'nın devralınmış uygulaması çağırmak için kullanılabilir. İşlev üyesi tarafından başvurulan bir *base_access* bir bağlama zamanı hatası oluşur, soyuttur.

### <a name="postfix-increment-and-decrement-operators"></a>Sonek artırma ve azaltma işleçleri

```antlr
post_increment_expression
    : primary_expression '++'
    ;

post_decrement_expression
    : primary_expression '--'
    ;
```

İşlenen bir sonek artırma veya azaltma işlemi bir değişken, bir özellik erişimi veya bir dizin oluşturucu erişim sınıflandırılmış bir ifade olmalıdır. İşleminin sonucu, işlenenin aynı türden bir değerdir.

Varsa *primary_expression* derleme zamanı türü `dynamic` işleci dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)), *post_increment_expression*veya *post_decrement_expression* derleme zamanı türü `dynamic` ve çalışma zamanı türünü kullanarak zamanında aşağıdaki kurallar uygulanır *primary_expression*.

Bir sonek işleneni artırmak, veya azaltma işlemi bir özellik veya dizin oluşturucu erişim, özelliği veya dizin oluşturucu gerekir sahip hem bir `get` ve `set` erişimcisi. Durum bu değilse, bir bağlama zamanı hatası oluşur.

Birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. Önceden tanımlanmış `++` ve `--` işleçleri aşağıdaki türleri için mevcut: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`ve herhangi bir sabit listesi türü. Önceden tanımlanmış `++` işleçleri dönüş işleneni ve önceden tanımlanmış 1 ekleyerek üretilen değeri `--` işleçleri, işlenende 1 çıkarılmasıyla oluşturulan değeri döndürür. İçinde bir `checked` bu ekleme veya çıkarma sonucu sonuç türü aralığının dışında ve sonuç türü bir integral türünü ya da sabit listesi türü ise bağlam bir `System.OverflowException` oluşturulur.

Çalışma zamanı işlenmesini bir sonek artırma veya azaltma işlemi formun `x++` veya `x--` aşağıdaki adımlardan oluşur:

*   Varsa `x` bir değişken olarak sınıflandırıldığını:
    * `x` değişkeni oluşturmak için değerlendirilir.
    * Değerini `x` kaydedilir.
    * Seçili işleç kaydedilmiş değerle çağrıldığını `x` bağımsız değişken olarak.
    * Operatör tarafından döndürülen değer değerlendirmesi tarafından belirtilen konumda depolanan `x`.
    * Kaydedilen değeri `x` işleminin sonucu olur.
*   Varsa `x` bir özellik veya dizin oluşturucu erişim sınıflandırıldığını:
    * Örnek ifade (varsa `x` değil `static`) ve bağımsız değişken listesi (varsa `x` bir dizin oluşturucu erişim) ile ilişkili `x` değerlendirilir, ve sonuçları sonraki kullanılan `get` ve `set` erişimci çağrıları.
    * `get` Erişimcisine `x` çağrılır ve döndürülen değer kaydedilir.
    * Seçili işleç kaydedilmiş değerle çağrıldığını `x` bağımsız değişken olarak.
    * `set` Erişimcisine `x` operatör olarak tarafından döndürülen değerle çağrılır, `value` bağımsız değişken.
    * Kaydedilen değeri `x` işleminin sonucu olur.

`++` Ve `--` işleçleri de destekler öneki biçimini ([önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)). Genellikle, sonucunu `x++` veya `x--` değeri `x` işleminden önce ise sonucu `++x` veya `--x` değeri `x` işleminden sonra. Her iki durumda da `x` kendisini işleminden sonra aynı değere sahiptir.

Bir `operator ++` veya `operator --` uygulama sonek veya önek gösterim kullanılarak çağrılacak. İki gösterimler için ayrı bir işleç uygulamalarına sahip mümkün değildir.

### <a name="the-new-operator"></a>New işleci

`new` İşleci yeni türlerin örneklerini oluşturmak için kullanılır.

Üç tür vardır `new` ifadeleri:

*  Nesne oluşturma ifadeler yeni örneklerini sınıf türleri ve değer türleri oluşturmak için kullanılır.
*  Dizi oluşturma ifadeleri, dizi türleri yeni örnekleri oluşturmak için kullanılır.
*  Temsilci oluşturma ifadeler yeni örnekleri temsilci türleri oluşturmak için kullanılır.

`new` İşleci, bir türün bir örneğinin oluşturulmasını gösterir, ancak dinamik bellek ayrılması gelmez. Özellikle, örnekleri değer türlerinin değişkenleri bulundukları ve dinamik edilmedi meydana ötesinde ek bellek gerektirir, `new` türlerin örneklerini oluşturmak için kullanılır.

#### <a name="object-creation-expressions"></a>Nesne oluşturma ifadeleri

Bir *object_creation_expression* yeni bir örneğini oluşturmak için kullanılan bir *class_type* veya *value_type*.

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

*Türü* , bir *object_creation_expression* olmalıdır bir *class_type*, *value_type* veya *type_parameter* . *Türü* olamaz bir `abstract` *class_type*.

İsteğe bağlı *argument_list* ([bağımsız değişken listeleri](expressions.md#argument-lists)) yalnızca izin verilen *türü* olduğu bir *class_type* veya *struct_ tür*.

Parantez içine bir nesne Başlatıcı veya koleksiyon Başlatıcısı içerir sağlanan ve bir nesne oluşturma ifadesi oluşturucu bağımsız değişken listesini atlayabilirsiniz. Oluşturucu bağımsız değişken listesi atlama ve parantez kapsayan bir boş bağımsız değişken listesi belirtmeye eşdeğerdir.

İlk örnek oluşturucusu ve ardından nesne Başlatıcı (BelirtilenüyeveyaöğeninbaşlatmalarişlemebirnesneBaşlatıcıveyakoleksiyonBaşlatıcısıiçerenbirnesneoluşturmaifadesiişlenmesinioluşur[ Nesne başlatıcılarda](expressions.md#object-initializers)) veya koleksiyon Başlatıcısı ([koleksiyon başlatıcıları](expressions.md#collection-initializers)).

İsteğe bağlı bağımsız değişken varsa *argument_list* derleme zamanı türü `dynamic` sonra *object_creation_expression* dinamik olarak bağlı ([dinamikbağlama](expressions.md#dynamic-binding)) ve bağımsız değişkenleri çalışma zamanı türünü kullanarak zamanında aşağıdaki kurallar uygulanır *argument_list* derleme zamanı türü `dynamic`. Ancak, nesne oluşturma açıklandığı gibi sınırlı bir derleme zamanı denetimi uğradığında [derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution).

Bağlama zamanı işlenmesini bir *object_creation_expression* formun `new T(A)`burada `T` olduğu bir *class_type* veya *value_type* ve `A` isteğe bağlıdır *argument_list*, aşağıdaki adımlardan oluşur:

*   Varsa `T` olduğu bir *value_type* ve `A` mevcut değil:
    * *Object_creation_expression* bir varsayılan oluşturucu çağrıdır. Sonucu *object_creation_expression* türü değeri `T`, yani için varsayılan değer `T` tanımlandığı gibi [System.ValueType türü](types.md#the-systemvaluetype-type).
*   Aksi takdirde `T` olduğu bir *type_parameter* ve `A` mevcut değil:
    * Hiçbir değer tür kısıtlaması veya Oluşturucu kısıtlaması ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) için belirtilen `T`, bir bağlama zamanı hatası oluşur.
    * Sonucu *object_creation_expression* tür parametresi için ilişkili çalışma zamanı tür değeri adlı varsayılan oluşturucu türü çağırma sonucu. Çalışma zamanı türü bir başvuru türü veya değer türü olabilir.
*   Aksi takdirde `T` olduğu bir *class_type* veya *struct_type*:
    * Varsa `T` olduğu bir `abstract` *class_type*, bir derleme zamanı hatası oluşur.
    * Çağrılacak örnek oluşturucusu aşırı yükleme çözünürlüğü kurallarını kullanarak belirlenir [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution). Bildirilen tüm erişilebilir örnek oluşturucular aday örnek oluşturucuları kümesini oluşan `T` ilişkilendirilebilmesi için geçerli olan `A` ([geçerli işlev üyesi](expressions.md#applicable-function-member)). Aday örnek oluşturucuları kümesi boşsa veya tek bir en iyi örnek oluşturucusu tanımladıysanız, bir bağlama zamanı hatası oluşur.
    * Sonucu *object_creation_expression* türü değeri `T`, yani değer üretilmediğini Yukarıdaki adımda belirlenen örnek oluşturucusunu çağırarak.
*  Aksi takdirde, *object_creation_expression* geçersiz ve bir bağlama zamanı hatası oluşur.

Bile *object_creation_expression* dinamik bağlı, derleme zamanı hala türdür `T`.

Çalışma zamanı işlenmesini bir *object_creation_expression* formun `new T(A)`burada `T` olduğu *class_type* veya *struct_type* ve `A` isteğe bağlıdır *argument_list*, aşağıdaki adımlardan oluşur:

*   Varsa `T` olduğu bir *class_type*:
    * Sınıfının yeni bir örneğini `T` ayrılır. Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.
    * Yeni örneğin tüm alanları varsayılan değerlerine başlatılır ([varsayılan değerler](variables.md#default-values)).
    * Örnek oluşturucusu işlevi üye çağırma kurallarına göre çağrılır ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Yeni ayrılmış örneğe bir başvuru için örnek oluşturucusu otomatik olarak geçirilir ve örnek bu oluşturucu içinde erişilebilir `this`.
*   Varsa `T` olduğu bir *struct_type*:
    * Türün bir örneğini `T` geçici yerel değişken ayırarak oluşturulur. Bir örnek oluşturucusunu beri bir *struct_type* kesinlikle oluşturulmasını, hiçbir başlatma geçici değişken gereklidir örneğin her alan için bir değer atamak için gereklidir.
    * Örnek oluşturucusu işlevi üye çağırma kurallarına göre çağrılır ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)). Yeni ayrılmış örneğe bir başvuru için örnek oluşturucusu otomatik olarak geçirilir ve örnek bu oluşturucu içinde erişilebilir `this`.

#### <a name="object-initializers"></a>Nesne başlatıcıları

Bir ***nesne Başlatıcı*** sıfır veya daha fazla alanlar, özellikler veya dizini oluşturulmuş bir nesne öğelerin değerleri belirtir.

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

Kapsadığı üyesi başlatıcıları, bir dizi nesne Başlatıcı oluşan `{` ve `}` belirteçler ve virgülle ayrılmış. Her *member_initializer* başlatma hedefi belirler. Bir *tanımlayıcı* ise bir erişilebilir alan veya özellik başlatılmakta, nesnenin adı olmalıdır bir *argument_list* içine üzerinde erişilebilir bir dizin oluşturucu için bağımsız değişkenler köşeli ayraç belirtmelisiniz başlatılan nesne. Nesne Başlatıcı aynı alan veya özellik için birden fazla üye Başlatıcı eklemek için bir hatadır.

Her *initializer_target* bir eşittir işareti ve bir ifade, bir nesne Başlatıcı veya koleksiyon başlatıcısı tarafından izlenir. İçinde başlatma yeni oluşturulan nesneye başvurmak için nesne Başlatıcı ifadeleri için mümkün değildir.

Eşittir işareti atama aynı şekilde işlendikten sonra bir ifade belirtir bir üye Başlatıcısı ([basit atama](expressions.md#simple-assignment)) hedef.

Nesne Başlatıcı sonra eşittir işareti belirten bir üye başlatıcısı bir ***iç içe geçmiş nesne Başlatıcı***, yani gömülü bir nesnenin bir başlatma. Alan veya özellik için yeni bir değer atamak yerine, iç içe geçmiş nesne Başlatıcı atamalarını atamaları üyelerine alanı veya özelliği olarak kabul edilir. İç içe geçmiş nesne başlatıcıları özelliklere sahip bir değer türü veya değer türü salt okunur alanlara uygulanamaz.

Eşittir işaretinden sonra koleksiyon Başlatıcısı belirten bir üye başlatıcısı, katıştırılmış bir koleksiyonun bir başlatmadır. Hedef alan, özelliği veya dizin oluşturucu için yeni bir koleksiyon atamak yerine başlatıcısında verilen öğeleri hedef tarafından başvurulan koleksiyona eklenir. Hedef belirtilen gereksinimleri karşılayan bir koleksiyon türünde olmalıdır [koleksiyon başlatıcıları](expressions.md#collection-initializers).

Bir dizin Başlatıcı bağımsız değişkenleri her zaman tam olarak bir kez değerlendirilir. Bu nedenle, bunlar bağımsız değişkenler (örneğin bir boş iç içe geçmiş Başlatıcı nedeniyle) hiç kullanılmadı düştüğünden olsa bile, yan etkileri için değerlendirilir.

Aşağıdaki sınıf iki koordinat noktasıyla temsil eder:
```csharp
public class Point
{
    int x, y;

    public int X { get { return x; } set { x = value; } }
    public int Y { get { return y; } set { y = value; } }
}
```

Örneği `Point` oluşturulabilir ve şu şekilde başlatıldı:
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
Burada `__a` yoksa görünmez ve erişilemez bir geçici değişken. Aşağıdaki sınıf iki noktalarından oluşturulan bir dikdörtgen temsil eder:
```csharp
public class Rectangle
{
    Point p1, p2;

    public Point P1 { get { return p1; } set { p1 = value; } }
    public Point P2 { get { return p2; } set { p2 = value; } }
}
```

Örneği `Rectangle` oluşturulabilir ve şu şekilde başlatıldı:
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
Burada `__r`, `__p1` ve `__p2` görünmez ve erişilemez Aksi takdirde geçici değişkenlerdir.

Varsa `Rectangle`ın katıştırılmış iki Oluşturucu ayırır `Point` örnekleri
```csharp
public class Rectangle
{
    Point p1 = new Point();
    Point p2 = new Point();

    public Point P1 { get { return p1; } }
    public Point P2 { get { return p2; } }
}
```
Şu yapı katıştırılmış başlatmak için kullanılan `Point` yeni örnekleri atamak yerine örnekleri:
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

C, aşağıdaki örnek, uygun bir tanımını ele alalım:
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
Bu bir dizi atamaları eşdeğerdir:
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
Burada `__c`vb. görünmez ve kaynak kodu için erişilemez oluşturulan değişkenlerdir. Unutmayın bağımsız değişkenleri için `[0,0]` değerlendirilen yalnızca bir kez ve bağımsız değişkenleri `[1,2]` hiçbir zaman kullanılmaz olsa bile bir kez değerlendirilir.

#### <a name="collection-initializers"></a>Koleksiyon başlatıcıları

Koleksiyon başlatıcısı, bir koleksiyonun öğeleri belirtir.

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

Koleksiyon başlatıcısı, bir dizi öğesi başlatıcıları, kapsadığı oluşan `{` ve `}` belirteçler ve virgülle ayrılmış. Her öğe Başlatıcısı başlatılmakta koleksiyon nesnesine eklenecek bir öğeyi belirtir ve ifadeler tarafından alınmış bir listesini oluşur `{` ve `}` belirteçler ve virgülle ayrılmış.  Tek ifadeli öğe Başlatıcısı küme ayraçları olmadan yazılabilir, ancak ardından belirsizlik üyesi başlatıcıları önlemek için bir atama ifadesi olamaz. *Non_assignment_expression* üretim tanımlanmış [ifade](expressions.md#expression).

Koleksiyon Başlatıcısı içeren bir nesne oluşturma ifadesi örneği verilmiştir:
```csharp
List<int> digits = new List<int> { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
```

Koleksiyon Başlatıcısı uygulandığı koleksiyon nesnesi uygulayan bir türü olmalıdır `System.Collections.IEnumerable` veya bir derleme zamanı hatası oluşur. Koleksiyon Başlatıcısı sırada belirtilen her öğe için çağıran bir `Add` yöntemi hedef uygulama normal üye araması bağımsız değişken listesi öğe Başlatıcısı ile ifade listesi nesnesi ve aşırı yükleme çözümlemesi her. Bu nedenle, koleksiyon nesnesi adı ile ilgili bir örneği veya genişletme metodu olmalıdır `Add` her öğe Başlatıcısı için.

Aşağıdaki sınıf bir kişiyle bir ad ve telefon numaralarının listesini temsil eder:
```csharp
public class Contact
{
    string name;
    List<string> phoneNumbers = new List<string>();

    public string Name { get { return name; } set { name = value; } }

    public List<string> PhoneNumbers { get { return phoneNumbers; } }
}
```

A `List<Contact>` oluşturulabilir ve şu şekilde başlatıldı:
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
Burada `__clist`, `__c1` ve `__c2` görünmez ve erişilemez Aksi takdirde geçici değişkenlerdir.

#### <a name="array-creation-expressions"></a>Dizi oluşturma ifadeleri

Bir *array_creation_expression* yeni bir örneğini oluşturmak için kullanılan bir *array_type*.

```antlr
array_creation_expression
    : 'new' non_array_type '[' expression_list ']' rank_specifier* array_initializer?
    | 'new' array_type array_initializer
    | 'new' rank_specifier array_initializer
    ;
```

Bir dizi oluşturma ifadesi ilk form, her biri ayrı ayrı ifadeleri ifade listeden silmesi sonuçları türünde bir dizi örnek ayırır. Örneğin, dizi oluşturma ifadesi `new int[10,20]` türünün bir dizi örneği üretir `int[,]`ve dizi oluşturma ifadesi `new int[10][,]` türünde bir dizi üretir `int[][,]`. Her deyim ifade listesi içinde türünde olmalıdır `int`, `uint`, `long`, veya `ulong`, ya da bir veya daha fazla bu türleri için örtük olarak dönüştürülebilir. Her bir ifadenin değerine karşılık gelen yeni ayrılan dizi örneği boyutun uzunluğunu belirler. Bir dizi boyutun uzunluğu negatif olmayan olması gerektiğinden, bir derleme zamanı hatası sahip olduğu bir *constant_expression* ifade listesi negatif bir değere sahip.

Güvenli olmayan bir bağlamda dışında ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)), dizileri düzenini belirtilmemiş.

İlk biçiminde bir dizi oluşturma ifadesi dizi Başlatıcı içeriyorsa, ifade listesi içinde her bir ifade bir sabit olmalıdır ve ifade listesi tarafından belirtilen boyut sayısı ve boyutu uzunlukları dizi Başlatıcısı içeriğiyle eşleşmesi gerekir.

İçinde bir dizi oluşturma ifadesi ikinci veya üçüncü formunun boyut sayısı belirtilen dizi türü veya boyut tanımlayıcısı dizi Başlatıcısı eşleşmelidir. Tek tek boyut uzunlukları, öğeleri dizi başlatıcısı, karşılık gelen iç içe geçme düzeylerinin her biri çok sayıda algılanır. Bu nedenle, ifade
```csharp
new int[,] {{0, 1}, {2, 3}, {4, 5}}
```
tam olarak karşılık gelir
```csharp
new int[3, 2] {{0, 1}, {2, 3}, {4, 5}}
```

Üçüncü biçiminde bir dizi oluşturma ifadesi olarak adlandırılır bir ***dizi oluşturma ifadesi örtük***. Dizinin öğe türü açıkça, ancak en yaygın türü olarak belirlenen verilmez, ikinci forma benzer ([en iyi ortak türü bir ifade kümesi bulma](expressions.md#finding-the-best-common-type-of-a-set-of-expressions)) dizi ifadeleri kümesinin Başlatıcı. Çok boyutlu bir dizi için başka bir deyişle, bir where *rank_specifier* en az bir virgül içeren tüm bu kümesi oluşur *ifade*içinde bulunan iç içe geçmiş s *array_initializer*s.

Dizi başlatıcıları daha ayrıntılı açıklanır [Dizi başlatıcıları](arrays.md#array-initializers).

Bir dizi oluşturma ifadesi değerlendirme sonucu bir değer, yani yeni ayrılan dizi örneğe bir başvuru olarak sınıflandırılır. Bir dizi oluşturma ifadesi çalışma zamanı işlenmesini aşağıdaki adımlardan oluşur:

*  Boyut uzunluğu ifadelerin *expression_list* sırayla soldan sağa doğru değerlendirilir. Örtük bir dönüştürme her ifade değerlendirmesinin ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) şu türlerden biri olarak gerçekleştirilir: `int`, `uint`, `long`, `ulong`. Örtük bir dönüştürme bulunduğu bu listedeki ilk türü seçilir. Bir ifade veya sonraki örtük dönüştürme değerinin bir özel durum neden olursa, hiçbir ek ifadeler değerlendirilir ve başka bir adım yürütülür.
*  Boyut uzunluğu hesaplanan değerleri şu şekilde doğrulanır. Bir veya daha fazla değer olan küçükse sıfır, bir `System.OverflowException` oluşturulur ve başka bir adım yürütülür.
*  Belirtilen boyut uzunlukta bir dizi örneğiyle ayrılır. Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.
*  Yeni dizi örneğinin tüm öğeleri varsayılan değerlerine başlatılır ([varsayılan değerler](variables.md#default-values)).
*  Dizi oluşturma ifadesi dizi Başlatıcı içeriyorsa, ardından her bir dizi Başlatıcısı ifadesinde değerlendirilir ve karşılık gelen dizi öğesine atanır. Atamalar ve değerlendirmeleri ifadeleri, dizi başlatıcısında yazılır sırayla gerçekleştirilir — diğer bir deyişle, öğeleri en sağdaki boyutu ilk artan dizin sırasıyla artan düzende başlatılır. Belirtilen bir ifade veya sonraki atama karşılık gelen bir dizi öğesinin değerinin bir özel durum neden olursa, daha fazla öğe başlatılır (ve kalan öğeleri, bu nedenle varsayılan değerlerine sahip olur).

Bir dizi oluşturma ifadesi dizi oluşturmada bir dizi türünde öğelerle izin verir, ancak böyle bir dizinin öğeleri el ile başlatılması gerekir. Örneğin, deyim
```csharp
int[][] a = new int[100][];
```
tek boyutlu bir dizi türü 100 öğelerle oluşturur `int[]`. Her öğenin ilk değer `null`. Ayrıca alt dizileri ve deyim örneği oluşturmak aynı dizi oluşturma ifadesi için mümkün değildir
```csharp
int[][] a = new int[100][5];        // Error
```
bir derleme zamanı hatası sonuçlanır. Alt dizileri örneğinin bunun yerine el ile giriş olarak gerçekleştirilmelidir
```csharp
int[][] a = new int[100][];
for (int i = 0; i < 100; i++) a[i] = new int[5];
```

Dizi alt dizileri tümü aynı uzunlukta olduğunda olan "dikdörtgen" bir şekil varsa çok boyutlu bir dizi kullanmak daha verimlidir. Yukarıdaki örnekte, dizi örneğinin 101 nesneleri oluşturur — bir dış dizi ve 100 alt dizi. Buna karşılık,
```csharp
int[,] = new int[100, 5];
```
yalnızca tek bir nesne, iki boyutlu bir dizi oluşturur ve tek bir deyimde ayırma gerçekleştirir.

Türü örtük olarak belirlenmiş dizi oluşturma örnekleri şunlardır:
```csharp
var a = new[] { 1, 10, 100, 1000 };                       // int[]

var b = new[] { 1, 1.5, 2, 2.5 };                         // double[]

var c = new[,] { { "hello", null }, { "world", "!" } };   // string[,]

var d = new[] { 1, "one", 2, "two" };                     // Error
```

Son deyim bir derleme zamanı hatası olduğundan neden ne `int` ya da `string` diğerine örtük olarak dönüştürülebilir ve bu nedenle var olmayan en sık yazın. Açıkça belirlenmiş dizi oluşturma ifadesi bu durumda, örneğin olmasını türü belirtme kullanılmalıdır `object[]`. Alternatif olarak, bir öğenin bunlar da sonra gösterilen öğe türü ortak bir taban türe, başvurusuna yayınlanabilir.

Türü örtük olarak belirlenmiş dizi oluşturma ifadeleri anonim nesne başlatıcıları birleştirilebilir ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) anonim olarak oluşturmak için veri yapılarını yazılı. Örneğin:
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

A *delegate_creation_expression* yeni bir örneğini oluşturmak için kullanılan bir *delegate_type*.

```antlr
delegate_creation_expression
    : 'new' delegate_type '(' expression ')'
    ;
```

Temsilci oluşturma ifadesine bağımsız değişkeni bir yöntem grubu, anonim bir işlev veya derleme zamanı tür değeri olmalıdır `dynamic` veya *delegate_type*. Bağımsız değişken bir yöntem grubu ise, bu yöntem tanımlar ve bir örnek yöntemi temsilci oluşturmak istediğiniz bir nesne. Bağımsız değişken anonim bir işlev ise doğrudan temsilci hedef yöntem gövdesini ve parametreleri tanımlar. Bağımsız değişken bir değer ise bir kopyasını oluşturmak için bir temsilci örneğini tanımlar.

Varsa *ifade* derleme zamanı türü `dynamic`, *delegate_creation_expression* dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)) ve aşağıdaki kuralları çalışma zamanı türünü kullanarak zamanında uygulanan *ifade*. Aksi takdirde kuralları, derleme zamanında uygulanır.

Bağlama zamanı işlenmesini bir *delegate_creation_expression* formun `new D(E)`burada `D` olduğu bir *delegate_type* ve `E` olduğu bir *ifadesi* , aşağıdaki adımlardan oluşur:

*  Varsa `E` bir yöntem grubu olduğundan temsilci oluşturma ifadesi, bir yöntem grubu dönüştürme aynı şekilde işlenir ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)) öğesinden `E` için `D`.
*  Varsa `E` anonim bir işlev, temsilci oluşturma ifadesi, tıpkı bir anonim işlev dönüştürme olarak işlenir ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)) gelen `E` için `D`.
*  Varsa `E` bir değer `E` uyumlu olması gerekir ([temsilci bildirimi](delegates.md#delegate-declarations)) ile `D`, ve sonuç yeni oluşturulan bir temsilci türü başvuru `D` aynı çağrıya başvuruyor liste olarak `E`. Varsa `E` ile uyumlu olmayan `D`, bir derleme zamanı hatası oluşur.

Çalışma zamanı işlenmesini bir *delegate_creation_expression* formun `new D(E)`burada `D` olduğu bir *delegate_type* ve `E` olduğu bir *ifadesi* , aşağıdaki adımlardan oluşur:

*   Varsa `E` bir yöntem grubu olduğundan temsilci oluşturma ifadesi bir yöntem grubu dönüştürme olarak değerlendirilir ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)) öğesinden `E` için `D`.
*   Varsa `E` anonim bir işlev, temsilci oluşturma bir anonim işlev dönüştürme değerlendirmesinde `E` için `D` ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)).
*   Varsa `E` bir değeri bir *delegate_type*:
    * `E` değerlendirilir. Bu değerlendirme bir özel durum neden olursa, başka bir adım yürütülür.
    * Varsa değerini `E` olduğu `null`, `System.NullReferenceException` oluşturulur ve başka bir adım yürütülür.
    * Temsilci türüne yeni bir örneğini `D` ayrılır. Yeni bir örneğini ayırmak yeterli bellek yoksa bir `System.OutOfMemoryException` oluşturulur ve başka bir adım yürütülür.
    * Yeni temsilci örneğini temsilci örneği tarafından verilen aynı çağırma listesiyle başlatılır `E`.

Temsilci örneği ve ardından tüm temsilci ömrü boyunca sabit mi kaldığını bir temsilcinin çağırma listesi belirlenir. Diğer bir deyişle, oluşturulduktan sonra bir temsilcinin hedef çağrılabilir varlıkları değiştirmek mümkün değildir. Ne zaman iki temsilci olduğunda birleştirilir veya bir kaldırıldı diğerinden ([temsilci bildirimi](delegates.md#delegate-declarations)), yeni bir temsilci sonuçlanır; varolan bir temsilci değiştirilen içeriğini sahip.

Bir özellik, dizin oluşturucu, kullanıcı tanımlı işleç, örnek oluşturucusu, yok Edicisi veya statik Oluşturucu başvuran bir temsilci oluşturmak mümkün değildir.

Bir temsilci yukarıda açıklandığı gibi bir yöntem grubu, biçimsel parametre listesi oluşturulduğunda ve temsilcinin dönüş türü, seçmek için aşırı yüklenmiş yöntemler belirleyin. Örnekte
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
`A.f` alan ikinci başvuran bir temsilci ile başlatılan `Square` yöntemi yöntemin dönüş türünü ve biçimsel parametre listesi tam olarak eşleştiğinden `DoubleFunc`. İkinci vardı `Square` yöntemi yüklenmemiş değil, bir derleme zamanı hatası oluşmuş.

#### <a name="anonymous-object-creation-expressions"></a>Anonim nesne oluşturma ifadeleri

Bir *anonymous_object_creation_expression* anonim bir türün bir nesne oluşturmak için kullanılır.

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

Anonim nesne Başlatıcı, anonim bir tür bildirir ve o türün örneğini döndürür. Anonim bir tür doğrudan devralan bir adsız sınıf türüdür `object`. Anonim bir tür, türün bir örneğini oluşturmak için kullanılan anonim nesne başlatıcısından algılanan salt okunur özellikler bir dizi üyeleridir. Özellikle, anonim nesne Başlatıcı form
```csharp
new { p1 = e1, p2 = e2, ..., pn = en }
```
Formun anonim bir tür bildirir
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
Burada her `Tx` karşılık gelen ifade türü `ex`. İçinde kullanılan ifade bir *member_declarator* bir türü olmalıdır. Bu nedenle, bir ifade için bir derleme zamanı hatası olduğu bir *member_declarator* null veya anonim bir işlevdir. Ayrıca bir derleme zamanı hatası güvenli olmayan bir tür için ifadeyi için bir hizmettir.

Anonim bir türün ve parametre adlarını kendi `Equals` yöntemi derleyici tarafından otomatik olarak oluşturulur ve program metni başvurulamaz.

Aynı programın içinde aynı sırada aynı adları ve derleme zamanı türü özellikleri dizisi belirtin iki anonim nesne başlatıcıları aynı anonim türün örneğini oluşturur.

Örnekte
```csharp
var p1 = new { Name = "Lawnmower", Price = 495.00 };
var p2 = new { Name = "Shovel", Price = 26.95 };
p1 = p2;
```
son satırı atamaya izin `p1` ve `p2` anonim türleri aynı.

`Equals` Ve `GetHashcode` anonim türlerde geçersiz kılma yöntemleri devralınan yöntemleri `object`, açısından tanımlanan `Equals` ve `GetHashcode` özelliklerinin iki örneği aynı anonim tür eşit olacak şekilde, ve yalnızca bunların tüm özelliklerini eşitse.

Basit bir ad üye bildirimcisi kısaltılmış ([anlam çıkarma](expressions.md#type-inference)), üye erişimi ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)), bir taban erişim ([temel erişim](expressions.md#base-access)) ya da bir üyesi null koşullu erişim ([projeksiyon başlatıcılar olarak Null koşullu ifadeler](expressions.md#null-conditional-expressions-as-projection-initializers)). Bu adlı bir ***projeksiyon Başlatıcı*** ve aynı ada sahip bir özellik ataması ve bildirimi için toplu özelliktir. Özellikle, üye bildirimcilerde formları
```csharp
identifier
expr.identifier
```
sırasıyla aşağıdaki tam olarak eşdeğerdir:
```csharp
identifier = identifier
identifier = expr.identifier
```

Bu nedenle, projeksiyon başlatıcısında *tanımlayıcı* hem değer ve alan veya özellik değeri atanır seçer. Sezgisel, yalnızca bir değer, aynı zamanda değerin adını bir projeksiyon Başlatıcısı yansıtıyor.

### <a name="the-typeof-operator"></a>Typeof işleci

`typeof` Elde etmek için kullanılan işleci `System.Type` nesne türü.

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

İlk formu *typeof_expression* oluşan bir `typeof` tarafından bir parantez içine alınmış anahtar sözcüğünü *türü*. Bu formu, bir ifadenin sonucu `System.Type` belirtilen tür için nesnesi. Yalnızca bir tane `System.Type` herhangi bir türü için nesne. Bir tür için buna `T`, `typeof(T) == typeof(T)` her zaman doğrudur. *Türü* olamaz `dynamic`.

İkinci form, *typeof_expression* oluşan bir `typeof` tarafından bir parantez içine alınmış anahtar sözcüğünü *unbound_type_name*. Bir *unbound_type_name* çok benzer bir *type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) dışında bir *unbound_type_name* içerir *generic_dimension_specifier*s burada bir *type_name* içeren *type_argument_list*s. Zaman işleneni bir *typeof_expression* hem de yapılarının dilbilgisi karşılayan belirteçler dizisidir *unbound_type_name* ve *type_name*, yani zaman içeriyor ne bir *generic_dimension_specifier* ya da bir *type_argument_list*, bir dizi belirteçleri olarak kabul edilir bir *type_name*. Anlamı bir *unbound_type_name* şu şekilde belirlenir:

*  Belirteçleri için bir dizi dönüştürme bir *type_name* her değiştirerek *generic_dimension_specifier* ile bir *type_argument_list* virgül aynı sayıda olması ve anahtar sözcüğü `object` her *type_argument*.
*  Ortaya çıkan değerlendirmek *type_name*, çalışırken tüm tür parametresi kısıtlamaları yoksayılıyor.
*  *Unbound_type_name* sonuç olarak oluşturulan tür ile ilişkili ilişkisiz genel tür çözümler ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)).

Sonucu *typeof_expression* olduğu `System.Type` genel tür ilişkisiz sonuç nesnesi.

Üçüncü biçiminde *typeof_expression* oluşan bir `typeof` tarafından bir parantez içine alınmış anahtar sözcüğünü `void` anahtar sözcüğü. Bu formu, bir ifadenin sonucu `System.Type` devamsızlık türü temsil eden nesne. Tarafından döndürülen tür nesnesi `typeof(void)` herhangi bir türü için döndürülen tür nesneden farklıdır. Burada bu yöntem istediğiniz örneğiyle birlikte void yöntemleri dahil olmak üzere herhangi bir yöntem dönüş türünü temsil eden bir yöntemden faydalanabilir yansıma yöntemleri üzerine olanak dilde sınıf kitaplıkları bu özel türü nesne yararlıdır `System.Type`.

`typeof` İşleci bir tür parametresinde kullanılabilir. Sonuç `System.Type` tür parametresine bağlanan çalışma zamanı türü için nesne. `typeof` İşleci, aynı zamanda oluşturulmuş bir türü veya ilişkisiz bir genel tür üzerinde kullanılabilir ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)). `System.Type` İlişkisiz bir genel tür ile aynı değil nesne `System.Type` örneği türündeki nesne. Bu nedenle, her zaman olduğu kapalı bir oluşturulmuş tür çalışma zamanında örnek türü kendi `System.Type` nesne bağlıdır kullanın, çalışma zamanı tür bağımsız değişkenlerde ilişkisiz genel tür hiçbir tür bağımsız değişkenleri sahipken.

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
aşağıdaki çıktıyı üretir:
```
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

Unutmayın `int` ve `System.Int32` aynı türdedir.

Ayrıca sonucunu `typeof(X<>)` tür bağımsız değişkeni ancak sonucu benzemez `typeof(X<T>)` yapar.

### <a name="the-checked-and-unchecked-operators"></a>Checked ve unchecked işleçleri

`checked` Ve `unchecked` işleçleri denetlemek için kullanılan ***taşma denetimi bağlamını*** Tamsayı türünde aritmetik işlemler ve dönüştürmeler için.

```antlr
checked_expression
    : 'checked' '(' expression ')'
    ;

unchecked_expression
    : 'unchecked' '(' expression ')'
    ;
```

`checked` İşleci denetlenen bir bağlamda kapsanan ifadeyi değerlendirir ve `unchecked` işleci işaretlenmemiş bir bağlamda kapsanan ifadeyi değerlendirir. A *checked_expression* veya *unchecked_expression* tam olarak karşılık gelen bir *parenthesized_expression* ([parantezliifade](expressions.md#parenthesized-expressions)) dışında verilen taşma denetimi bağlamını kapsanan ifade değerlendirilir.

İçerik denetleme taşma ayrıca aracılığıyla denetlenebilir `checked` ve `unchecked` deyimleri ([checked ve unchecked deyimleri](statements.md#the-checked-and-unchecked-statements)).

Aşağıdaki işlemleri tarafından kurulan bağlam taşma etkilenen `checked` ve `unchecked` işleçler ve deyimler:

*  Önceden tanımlanmış `++` ve `--` birli işleçleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators) ve [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), işlenen integral olduğunda yazın.
*  Önceden tanımlanmış `-` birli işleç ([birli eksi işleci](expressions.md#unary-minus-operator)), işlenen bir tamsayı türünde olduğunda.
*  Önceden tanımlanmış `+`, `-`, `*`, ve `/` ikili işleçler ([aritmetik işleçler](expressions.md#arithmetic-operators)), her iki işlenen de integral türleridir.
*  Açık sayısal Dönüşümler ([açık sayısal dönüşümler](conversions.md#explicit-numeric-conversions)) bir integral türünden başka bir integral türüne veya gelen `float` veya `double` bir integral türü.

Ne zaman yukarıdaki işlemlerden biri hedef türü, işlemi gerçekleştirilen denetimler olduğu bağlam sonuç davranışına temsil etmek için çok büyük bir sonuç üreten:

*  İçinde bir `checked` bağlamı, işlemi bir sabit ifade ise ([sabit ifadeler](expressions.md#constant-expressions)), bir derleme zamanı hatası oluşur. Aksi durumda, çalışma zamanında, işlemi gerçekleştirilirken bir `System.OverflowException` oluşturulur.
*  İçinde bir `unchecked` bağlamı, sonucu, hedef türüne uygun değil herhangi bir yüksek sıra bitleri atarak kısaltılır.

Sabit olmayan ifade (çalışma zamanında değerlendirilir ifadeleri) değil içine alınan göre `checked` veya `unchecked` işleçleri deyimleri, bağlam varsayılan taşma mi `unchecked` (derleyici gibi dış etkenler sürece anahtarlar ve yürütme ortamı yapılandırması) arama `checked` değerlendirme.

(Tam olarak derleme zamanında değerlendirilebilen ifadeleri) sabit ifadeler için bağlam varsayılan taşma her zaman olduğu `checked`. Sabit bir ifade açıkça yerleştirilir sürece bir `unchecked` içerik, ifade her zaman derleme zamanı değerlendirmesi sırasında gerçekleşen taşıyor, derleme zamanı hatalarına neden olabilir.

Anonim bir işlev gövdesini etkilenmez `checked` veya `unchecked` anonim işlev gerçekleştiği bağlamı.

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
Hiçbiri bir ifadenin derleme zamanında değerlendirilebilen bu yana herhangi bir derleme zamanı hata bildirilir. Çalışma zamanında, `F` yöntem bir `System.OverflowException`ve `G` yöntemi-727379968 (aralık dışı sonuç alt 32 bit) döndürür. Davranışını `H` yöntemi, derleme için içerik denetimi varsayılan taşma bağlıdır, ancak aynı olan `F` veya aynı `G`.

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
Sabit ifadelerde değerlendirirken oluşan taşıyor `F` ve `H` ifadeler değerlendirilir çünkü bildirilmesini derleme zamanı hatalarına neden bir `checked` bağlamı. Taşma Sabit ifadede değerlendirirken gerçekleşir `G`, ancak değerlendirme gerçekleştikten sonra bir `unchecked` bağlamı taşma bildirilmedi.

`checked` Ve `unchecked` işleçler yalnızca metin içeriğini eklemek bulunan bu işlemler için bağlam taşma etkiler "`(`"ve"`)`" belirteçleri. İşleçler, kapsanan ifade değerlendirme sonucu olarak çağrılan işlev üyeleri üzerinde etkisi yoktur. Örnekte
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
kullanımını `checked` içinde `F` değerlendirmesi etkilemez `x * y` içinde `Multiply`, bu nedenle `x * y` varsayılan taşma denetimi bağlamını değerlendirilir.

`unchecked` İşleci, işaretli integral türlerindeki sabitleri onaltılık gösterimde yazarken kullanışlıdır. Örneğin:
```csharp
class Test
{
    public const int AllBits = unchecked((int)0xFFFFFFFF);

    public const int HighBit = unchecked((int)0x80000000);
}
```

Yukarıdaki onaltılık sabitler her ikisi de türlerinin `uint`. Sabitler dışında olduğundan `int` olmadan aralığı `unchecked` işleç, yayınlar için `int` derleme zamanı hataya neden.

`checked` Ve `unchecked` işleçler ve ifadeler programcılar bazı sayısal hesaplamalar belirli yönlerini denetlemek izin verin. Ancak, bazı sayısal işleçlerin davranışını işlenenlerini veri türlerine bağlıdır. Örneğin, her zaman iki ondalık basamak çarparak sonuçlanıyor bir özel durum taşmada bile içinde bir açıkça `unchecked` oluşturun. Benzer şekilde, iki çarparak hiçbir zaman sonuçları bir özel durum taşmada bile içinde gezinen bir açıkça `checked` oluşturun. Ayrıca, diğer işleçlerin Denetleme modu tarafından hiçbir zaman etkilenir, yoksa varsayılan veya açık.

### <a name="default-value-expressions"></a>Varsayılan değer ifadeleri

Varsayılan değeri elde etmek için kullanılan bir varsayılan değer ifadesi ([varsayılan değerler](variables.md#default-values)) türü. Genellikle bir varsayılan değer ifadesi türü parametre bir değer türü veya bir başvuru türü ise, bilinmeyebilir beri tür parametreleri için kullanılır. (Öğesinden dönüştürme var `null` tür parametresi olarak bir başvuru türü bilinen sürece sabit bir tür parametresine.)

```antlr
default_value_expression
    : 'default' '(' type ')'
    ;
```

Varsa *türü* içinde bir *default_value_expression* değerlendirir çalışma zamanında bir başvuru türü için sonucudur `null` bu türe dönüştürülüp. Varsa *türü* içinde bir *default_value_expression* değerlendirir çalışma zamanında bir değer türüne, sonucudur *value_type*ait varsayılan değer ([varsayılan Oluşturucular](types.md#default-constructors)).

A *default_value_expression* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) türü bir başvuru türü veya bir başvuru türü olarak bilinen bir tür parametresi ise ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)). Ayrıca, bir *default_value_expression* sabit bir ifade türü aşağıdaki değer türlerinden biri ise: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, ya da herhangi bir numaralandırma türü.


### <a name="nameof-expressions"></a>Nameof ifadeleri

A *nameof_expression* bir program varlık olarak bir sabit dize adını almak için kullanılır.

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

Dilbilgisi bakımından Konuşmayı *named_entity* işlenen, her zaman bir ifade. Çünkü `nameof` ayrılmış bir anahtar sözcük olmayan bir nameof ifade her zaman basit bir ad ile bir çağrı sözdizimi kurallarına göre belirsiz `nameof`. Ad arama, uyumluluk nedenleriyle ([basit adları](expressions.md#simple-names)) adını `nameof` başarılı, ifade olarak işlenir bir *invocation_expression* --çağırma olmasına bakılmaksızın yasal. Aksi durumda olduğu bir *nameof_expression*.

Anlamını *named_entity* , bir *nameof_expression* Bunu anlamı; ifade olarak olarak diğer bir deyişle, ya da bir *simple_name*, *base_access*  veya *member_access*. Bununla birlikte, burada arama açıklanan [basit adları](expressions.md#simple-names) ve [üye erişimi](expressions.md#member-access) bir örnek üye statik bir bağlamda bulunamadığı için hatayla sonuçlanır bir *nameof_expression*böyle bir hata oluşturur.

Bir derleme zamanı hata için bir *named_entity* atamak için bir yöntem grubu bir *type_argument_list*. Bir derleme zamanı hata için bir *named_entity_target* türünün `dynamic`.

A *nameof_expression* sabit ifade türü `string`, ve çalışma zamanında hiçbir etkisi olmaz. Özellikle, kendi *named_entity* değerlendirilmez ve belirli atama onayına analiz amacıyla göz ardı edilir ([basit ifadeler için genel kurallar](variables.md#general-rules-for-simple-expressions)). Değerini son tanımlayıcısıdır *named_entity* isteğe bağlı son önce *type_argument_list*, aşağıdaki şekilde dönüştürülmüş:

* Önek "`@`", kullandıysanız kaldırılır.
* Her *unicode_escape_sequence* , karşılık gelen bir Unicode karakter dönüştürülür.
* Tüm *formatting_characters* kaldırılır.

Uygulanan aynı dönüştürmeleri bunlar [tanımlayıcıları](lexical-structure.md#identifiers) tanımlayıcıları arasındaki test ederken.

TODO: örnekler

### <a name="anonymous-method-expressions"></a>Anonim yöntem ifadeleri

Bir *anonymous_method_expression* anonim bir işlevdir tanımlamanın iki yollarından biridir. Bunlar daha ayrıntılı açıklanmıştır [anonim işlev ifadeleri](expressions.md#anonymous-function-expressions).

## <a name="unary-operators"></a>Birli işleçler

`?`, `+`, `-`, `!`, `~`, `++`, `--`, Noktaya yayın, ve `await` işleçleri birli işleçler çağrılır.

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

Varsa işleneni bir *unary_expression* derleme zamanı türü `dynamic`, dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı türü *unary_expression* olduğu `dynamic`, ve çalışma zamanında çalışma zamanı tür işleneninin kullanarak aşağıda açıklanan çözümleme gerçekleşir.

### <a name="null-conditional-operator"></a>Null koşullu işleci

Yalnızca işlenen null değilse null koşullu işleci, işleneni işlemlerin bir listesi uygular. Aksi takdirde sonucudur işlecini `null`.

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

Üye erişimi ve (hangi kendilerini null koşullu olabilir) öğe erişim işlemleri yanı sıra, çağırma işlemlerin listesini içerebilir.

Örneğin, ifade `a.b?[0]?.c()` olduğu bir *null_conditional_expression* ile bir *primary_expression* `a.b` ve *null_conditional_operations* `?[0]` (öğesi null koşullu erişimi) `?.c` (null koşullu üye erişimi) ve `()` (çağırma).

İçin bir *null_conditional_expression* `E` ile bir *primary_expression* `P`, let `E0` olması öndegelenmetiniçeriğinieklemekkaldırarakeldeedilenifade`?`her birinden *null_conditional_operations* , `E` bir sahip. Kavramsal olarak, `E0` null denetimleri hiçbiri tarafından temsil edilen, hesaplanacak olan ifade `?`s bulma bir `null`.

Ayrıca, `E1` olması önde gelen metin içeriğini eklemek kaldırarak elde edilen ifade `?` yalnızca gelen, ilk *null_conditional_operations* içinde `E`. Bu neden bir *birincil ifade* (varsa yalnızca `?`) veya başka bir *null_conditional_expression*.

Örneğin, varsa `E` ifade `a.b?[0]?.c()`, ardından `E0` ifade `a.b[0].c()` ve `E1` ifade `a.b[0]?.c()`.

Varsa `E0` nothing, ardından sınıflandırılır `E` nothing sınıflandırılır. Aksi takdirde E bir değer olarak sınıflandırılır.

`E0` ve `E1` anlamını belirlemek için kullanılan `E`:

*  Varsa `E` olarak gerçekleşir bir *statement_expression* anlamını `E` deyimi ile aynıdır

   ```csharp
   if ((object)P != null) E1;
   ```

   dışında P yalnızca bir kez değerlendirilir.

*  Aksi takdirde `E0` bir derleme zamanı hatası oluşur nothing sınıflandırılır.

*  Aksi takdirde, izin `T0` türünde `E0`.

   *  Varsa `T0` bir derleme zamanı hatası oluşur, bir başvuru türüyle veya NULL olmayan değer türü olması için bilinmeyen bir tür parametresi.

   *  Varsa `T0` NULL olmayan bir değer türü ve türü olan `E` olduğu `T0?`ve anlamını `E` aynıdır

      ```csharp
      ((object)P == null) ? (T0?)null : E1
      ```

      dışında `P` yalnızca bir kez değerlendirilir.

   *  Aksi takdirde E T0 türüdür ve E anlamını aynıdır

      ```csharp
      ((object)P == null) ? null : E1
      ```

      dışında `P` yalnızca bir kez değerlendirilir.

Varsa `E1` kendisi bir *null_conditional_expression*, daha sonra testler için iç içe yeniden, bu kurallar uygulanır `null` kalmayana kadar başka `?`'s, ve ifade tüm aşağı azaltıldı Birincil ifade `E0`.

Örneğin, ifade `a.b?[0]?.c()` deyim olduğu gibi bir deyim-ifadesi olarak gerçekleşir:
```csharp
a.b?[0]?.c();
```
anlamını eşdeğerdir:
```csharp
if (a.b != null) a.b[0]?.c();
```
hangi yeniden eşdeğerdir:
```csharp
if (a.b != null) if (a.b[0] != null) a.b[0].c();
```
Dışında `a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.

Değerini, olarak kullanıldığı bir bağlam içinde ortaya çıkarsa:
```csharp
var x = a.b?[0]?.c();
```
ve son çağırma türü, NULL olmayan bir değer türü değil varsayarak anlamını eşdeğerdir:
```csharp
var x = (a.b == null) ? null : (a.b[0] == null) ? null : a.b[0].c();
```
Dışında `a.b` ve `a.b[0]` yalnızca bir kez değerlendirilir.

#### <a name="null-conditional-expressions-as-projection-initializers"></a>Null koşullu ifadeler projeksiyon başlatıcılar olarak

Null koşullu ifade yalnızca olarak kullanılabilir bir *member_declarator* içinde bir *anonymous_object_creation_expression* ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)), Bu, bir (isteğe bağlı olarak null koşullu) üye erişimi ile sona erer. Dilbilgisi bakımından, bu gereklilik olarak ifade edilebilir:

```antlr
null_conditional_member_access
    : primary_expression null_conditional_operations? '?' '.' identifier type_argument_list?
    | primary_expression null_conditional_operations '.' identifier type_argument_list?
    ;
```

Bu bir dil bilgisi için özel durumdur *null_conditional_expression* yukarıda. Üretim için *member_declarator* içinde [anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions) yalnızca içeren *null_conditional_member_access*.

#### <a name="null-conditional-expressions-as-statement-expressions"></a>Null koşullu ifadeleri deyim ifadeleri olarak

Null koşullu ifade yalnızca olarak kullanılabilir bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)) ile bir çağrı sona ererse. Dilbilgisi bakımından, bu gereklilik olarak ifade edilebilir:

```antlr
null_conditional_invocation_expression
    : primary_expression null_conditional_operations '(' argument_list? ')'
    ;
```

Bu bir dil bilgisi için özel durumdur *null_conditional_expression* yukarıda. Üretim için *statement_expression* içinde [ifade deyimleri](statements.md#expression-statements) yalnızca içeren *null_conditional_invocation_expression*.


### <a name="unary-plus-operator"></a>Tekli artı işleci

Formun bir işlem için `+x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür. Önceden tanımlanmış birli artı işleçler şunlardır:

```csharp
int operator +(int x);
uint operator +(uint x);
long operator +(long x);
ulong operator +(ulong x);
float operator +(float x);
double operator +(double x);
decimal operator +(decimal x);
```

Bu işleçlerden her biri için sonuç basit işlenenin değeridir.

### <a name="unary-minus-operator"></a>Birli eksi işleci

Formun bir işlem için `-x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür. Önceden tanımlanmış değilleme işleçleri şunlardır:

*  Tamsayı olumsuzlama:

   ```csharp
   int operator -(int x);
   long operator -(long x);
   ```

   Sonuç çıkarılmasıyla hesaplanır `x` sıfırdan. Varsa değerini, `x` işlenen türünde gösterilebilir en küçük değer (-2 ^ 31 için `int` veya -2 ^ 63 için `long`), matematik negation ardından `x` işlenen türü içinde gösterilebilir değil. İçinde bu meydana gelirse bir `checked` bağlamı bir `System.OverflowException` içinde ortaya çıkarsa; oluşturulur bir `unchecked` bağlamını, işlenenin sonucudur ve taşma bildirilmedi.

   Değilleme işleci, işlenenin türü olup olmadığını `uint`, türüne dönüştürülür `long`, ve sonuç türü `long`. Bir özel durum verir kuralıdır `int` değeri -2147483648 (-2 ^ 31) ondalık tamsayı sabit değeri yazılacak ([tamsayı sabit değerlerinde](lexical-structure.md#integer-literals)).

   Değilleme işleci, işlenenin türü olup olmadığını `ulong`, bir derleme zamanı hatası oluşur. Bir özel durum verir kuralıdır `long` değer -9223372036854775808 (-2 ^ 63) ondalık tamsayı sabit değeri yazılacak ([tamsayı sabit değerlerinde](lexical-structure.md#integer-literals)).

*  Kayan nokta olumsuzlama:

   ```csharp
   float operator -(float x);
   double operator -(double x);
   ```

   Sonuç değeri `x` ters işareti ile. Varsa `x` , NaN olup da NaN sonucudur.

*  Ondalık olumsuzlama:

   ```csharp
   decimal operator -(decimal x);
   ```

   Sonuç çıkarılmasıyla hesaplanır `x` sıfırdan. Ondalık olumsuzlama birli eksi işleci türünde kullanmaya eşdeğerdir `System.Decimal`.

### <a name="logical-negation-operator"></a>Mantıksal değilleme işleci

Formun bir işlem için `!x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür. Yalnızca bir önceden tanımlanmış mantıksal değilleme işleci vardır:
```csharp
bool operator !(bool x);
```

Bu işleç, işlenenin mantıksal olumsuzlama hesaplar: İşlenen `true`, sonuç `false`. İşlenen `false`, sonuç `true`.

### <a name="bitwise-complement-operator"></a>Bit düzeyinde tamamlama işleci

Formun bir işlem için `~x`, birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenen seçili işleç parametre türüne dönüştürülür ve sonuç türü işlecinin dönüş türüdür. Önceden tanımlanmış bir bit düzeyinde tamamlayıcı işleçler şunlardır:
```csharp
int operator ~(int x);
uint operator ~(uint x);
long operator ~(long x);
ulong operator ~(ulong x);
```

Bu işleçlerden her biri için işlemin sonucunu, bit düzeyinde tamamlayıcı olan `x`.

Her bir numaralandırma türü `E` örtük olarak aşağıdaki bit düzeyinde tamamlama işleci sağlar:

```csharp
E operator ~(E x);
```

Değerlendirme sonucu `~x`burada `x` ifade bir sabit listesi türü `E` bir temel türü ile `U`, tam olarak değerlendiriliyor aynıdır `(E)(~(U)x)`dışında dönüştürme `E` olduğu her zaman olarak gerçekleştirilen sahipse bir `unchecked` bağlam ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)).

### <a name="prefix-increment-and-decrement-operators"></a>Önek artırma ve azaltma işleçleri

```antlr
pre_increment_expression
    : '++' unary_expression
    ;

pre_decrement_expression
    : '--' unary_expression
    ;
```

İşlenen bir önek artırma veya azaltma işlemi bir değişken, bir özellik erişimi veya bir dizin oluşturucu erişim sınıflandırılmış bir ifade olmalıdır. İşleminin sonucu, işlenenin aynı türden bir değerdir.

Bir önek işleneni artırmak, veya azaltma işlemi bir özellik veya dizin oluşturucu erişim, özelliği veya dizin oluşturucu gerekir sahip hem bir `get` ve `set` erişimcisi. Durum bu değilse, bir bağlama zamanı hatası oluşur.

Birli işleç aşırı yükleme çözümlemesi ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. Önceden tanımlanmış `++` ve `--` işleçleri aşağıdaki türleri için mevcut: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`ve herhangi bir sabit listesi türü. Önceden tanımlanmış `++` işleçleri dönüş işleneni ve önceden tanımlanmış 1 ekleyerek üretilen değeri `--` işleçleri, işlenende 1 çıkarılmasıyla oluşturulan değeri döndürür. İçinde bir `checked` bu ekleme veya çıkarma sonucu sonuç türü aralığının dışında ve sonuç türü bir integral türünü ya da sabit listesi türü ise bağlam bir `System.OverflowException` oluşturulur.

Çalışma zamanı işlenmesini önek artırma veya azaltma işlemi formun `++x` veya `--x` aşağıdaki adımlardan oluşur:

*   Varsa `x` bir değişken olarak sınıflandırıldığını:
    * `x` değişkeni oluşturmak için değerlendirilir.
    * Seçili işleç değeri ile çağrılan `x` bağımsız değişken olarak.
    * Operatör tarafından döndürülen değer değerlendirmesi tarafından belirtilen konumda depolanan `x`.
    * Operatör tarafından döndürülen değer, işlemin sonucunu haline gelir.
*   Varsa `x` bir özellik veya dizin oluşturucu erişim sınıflandırıldığını:
    * Örnek ifade (varsa `x` değil `static`) ve bağımsız değişken listesi (varsa `x` bir dizin oluşturucu erişim) ile ilişkili `x` değerlendirilir, ve sonuçları sonraki kullanılan `get` ve `set` erişimci çağrıları.
    * `get` Erişimcisine `x` çağrılır.
    * Seçili işleç tarafından döndürülen değerle çağrılır `get` bağımsız değişken olarak erişimcisi.
    * `set` Erişimcisine `x` operatör olarak tarafından döndürülen değerle çağrılır, `value` bağımsız değişken.
    * Operatör tarafından döndürülen değer, işlemin sonucunu haline gelir.

`++` Ve `--` işleçleri de destekler sonek gösteriminde ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)). Genellikle, sonucunu `x++` veya `x--` değeri `x` işleminden önce ise sonucu `++x` veya `--x` değeri `x` işleminden sonra. Her iki durumda da `x` kendisini işleminden sonra aynı değere sahiptir.

Bir `operator++` veya `operator--` uygulama sonek veya önek gösterim kullanılarak çağrılacak. İki gösterimler için ayrı bir işleç uygulamalarına sahip mümkün değildir.

### <a name="cast-expressions"></a>Cast ifadeleri

A *cast_expression* açıkça bir ifadenin belirtilen bir türe dönüştürmek için kullanılır.

```antlr
cast_expression
    : '(' type ')' unary_expression
    ;
```

A *cast_expression* formun `(T)E`burada `T` olduğu bir *türü* ve `E` olduğu bir *unary_expression*, açık bir gerçekleştirir dönüştürme ([açık dönüştürmeler](conversions.md#explicit-conversions)) değerinin `E` türüne `T`. Açık bir dönüştürme gelen varsa `E` için `T`, bir bağlama zamanı hatası oluşur. Aksi halde sonuç, açık bir dönüştürme tarafından üretilen değeridir. Sonucu her zaman bir değer olarak sınıflandırılır bile `E` bir değişkeni gösterir.

Dil bilgisi için bir *cast_expression* için belirli bir söz dizimi belirsizliğe neden olur. Örneğin, ifade `(x)-y` ya da olarak yorumlanabilecek bir *cast_expression* (atanacağını `-y` türüne `x`) veya farklı bir *additive_expression* birlikte bir *parenthesized_expression* (değeri hesaplar `x - y)`.

Çözümlenecek *cast_expression* belirsizlikler, aşağıdaki kural bulunmaktadır: Bir veya daha fazla bir dizi *belirteci*s ([boşluk](lexical-structure.md#white-space)) içine parantez içine başlangıcı olarak kabul edilir bir *cast_expression* yalnızca aşağıdakilerden en az biri doğruysa:

*  Bir dizi belirteçleri doğru dilbilgisi için olan bir *türü*, ancak bir *ifade*.
*  Dizi belirteçleri doğru dilbilgisi için olan bir *türü*, ve ayraç takip belirtecin belirteç "`~`", belirteç "`!`", belirteç "`(`", bir  *tanımlayıcı* ([Unicode karakter kaçış dizileri](lexical-structure.md#unicode-character-escape-sequences)), *değişmez değer* ([değişmez değerleri](lexical-structure.md#literals)), veya tüm *anahtar sözcüğü*([Anahtar sözcükleri](lexical-structure.md#keywords)) dışında `as` ve `is`.

Terim "doğru dilbilgisi" yukarıda yalnızca, bir dizi belirteçleri belirli dilbilgisi üretim uymalıdır anlamına gelir. Özellikle bağlı tüm tanımlayıcılar gerçek anlamını algılamaz. Örneğin, varsa `x` ve `y` , ardından tanımlayıcılardır `x.y` olan bir tür için doğru dilbilgisi bile `x.y` gerçekte bir tür belirtmek değil.

Takip eden, varsa Kesinleştirme kuraldan `x` ve `y` tanımlayıcılardır, `(x)y`, `(x)(y)`, ve `(x)(-y)` olan *cast_expression*s, ancak `(x)-y` değil, bile `x` bir türü tanımlar. Ancak, varsa `x` önceden tanımlanmış bir türü tanımlayan bir anahtar sözcüğü (gibi `int`), tüm dört formlar sonra *cast_expression*s (böyle bir anahtar sözcük büyük olasılıkla bir ifade kendisi tarafından çözümlenemediğinden).

### <a name="await-expressions"></a>Await ifadeleri

Await işleci işlenen tarafından temsil edilen zaman uyumsuz işlem tamamlanıncaya kadar kapsayan zaman uyumsuz İşlev değerlendirmesi askıya almak için kullanılır.

```antlr
await_expression
    : 'await' unary_expression
    ;
```

Bir *await_expression* yalnızca bir zaman uyumsuz işlev gövdesinde izin verilir ([yineleyiciler](classes.md#iterators)). İçinde en yakın kapsayan zaman uyumsuz işlev bir *await_expression* bu sayfalarda gerçekleşmeyebilir:

*  İç içe geçmiş (async olmayan) bir anonim işlev içinde
*  Bloğu içinde bir *lock_statement*
*  Güvenli olmayan bir bağlamda

Unutmayın bir *await_expression* içinde birçok yerinden olamaz bir *query_expression*olanlar olmayan zaman uyumsuz lambda ifadelerini kullanmak sözdizimsel olarak dönüştürülür çünkü.

Bir zaman uyumsuz işlev içinde `await` tanımlayıcı olarak kullanılamaz. Bu nedenle hiçbir belirsizliğine await ifadeleri ve tanımlayıcıları içeren çeşitli ifadeler arasında yoktur. Zaman uyumsuz işlevleri dışında `await` normal bir tanımlayıcı olarak görev yapar.

İşleneni bir *await_expression* çağrılır ***görev***. Zaman tam olmayabilir veya zaman uyumsuz bir işlem temsil ettiği *await_expression* değerlendirilir. Await işleci amacı, beklenen görev tamamlanana kadar kapsayan zaman uyumsuz işlev yürütülmesini askıya alma ve sonra sonucunu elde etmektir.

#### <a name="awaitable-expressions"></a>Beklenebilir ifadeleri

Görev bir await ifadesi olması gereken ***beklenebilir***. Bir ifade `t` aşağıdakilerden birini tutuyorsa beklenebilir olduğu:

*  `t` derleme zamanı türü `dynamic`
*  `t` sahip olarak adlandırılan bir erişilebilir örneği veya genişletme yöntemi `GetAwaiter` parametresiz ve hiçbir tür parametreleri ve dönüş türü ile `A` kendisi için aşağıdakilerin tümü tutun:
   * `A` arabirimi uygulayan `System.Runtime.CompilerServices.INotifyCompletion` (bundan böyle olarak bilinen `INotifyCompletion` kısaltma)
   * `A` erişilebilir, okunabilir örneği özelliği `IsCompleted` türü `bool`
   * `A` erişilebilir bir örnek yöntemi olan `GetResult` parametresiz ve hiçbir tür parametreleri

Amacı `GetAwaiter` yöntemi olduğundan edinmek için bir ***awaiter*** görev. Türü `A` çağrılır ***awaiter türü*** await ifadesi için.

Amacı `IsCompleted` görev zaten tamamlandıysa, belirlemek için özelliğidir. Bu durumda, değerlendirme askıya almak için gerek yoktur.

Amacı `INotifyCompletion.OnCompleted` yöntemdir; görev için bir "Devam" kaydolma için başka bir deyişle bir temsilci (tür `System.Action`), çağrılacak görev tamamlandıktan sonra.

Amacı `GetResult` yöntemi tamamlandığında, görevin sonucunu elde etmektir. Bu sonucu bir sonuç değeri ile büyük olasılıkla Başarılı tamamlama olabilir veya tarafından oluşturulan bir özel durum olabilir `GetResult` yöntemi.

#### <a name="classification-of-await-expressions"></a>Sınıflandırılması await ifadeleri

İfade `await t` ifade aynı şekilde sınıflandırılır `(t).GetAwaiter().GetResult()`. Bu nedenle, dönüş türü `GetResult` olduğu `void`, *await_expression* nothing sınıflandırılır. Void olmayan dönüş türü varsa `T`, *await_expression* türünde bir değer sınıflandırılır `T`.

#### <a name="runtime-evaluation-of-await-expressions"></a>Çalışma zamanı değerlendirmesini await ifadeleri

Çalışma zamanında, ifade `await t` gibi değerlendirilir:

*  Bir awaiter `a` ifadenin değerlendirilmesi yoluyla elde edilen `(t).GetAwaiter()`.
*  A `bool` `b` ifadenin değerlendirilmesi yoluyla elde edilen `(a).IsCompleted`.
*  Varsa `b` olduğu `false` değerlendirme bağlıdır sonra `a` arabirimi uygulayan `System.Runtime.CompilerServices.ICriticalNotifyCompletion` (bundan böyle olarak bilinen `ICriticalNotifyCompletion` kısaltma). Bu denetim, zaman bağlama sırasında gerçekleştirilir; yani, çalışma zamanında, `a` derleme zamanı tür sahip `dynamic`ve aksi takdirde derleme zamanında. İzin `r` sürdürme temsilci belirtmek ([yineleyiciler](classes.md#iterators)):
    * Varsa `a` uygulamıyor `ICriticalNotifyCompletion`, ifade `(a as (INotifyCompletion)).OnCompleted(r)` değerlendirilir.
    * Varsa `a` uygulamak `ICriticalNotifyCompletion`, ifade `(a as (ICriticalNotifyCompletion)).UnsafeOnCompleted(r)` değerlendirilir.
    * Değerlendirme sonra askıya alınır ve denetim, zaman uyumsuz işlev geçerli çağırana döndürülür.
*  Ya da hemen sonra (varsa `b` olduğu `true`), veya sürdürme temsilcinin sonraki çağrı sırasında (varsa `b` olduğu `false`), ifade `(a).GetResult()` değerlendirilir. Bir değer döndürürse, bu değeri sonucudur *await_expression*. Aksi halde sonuç hiçbir şey olur.

Arabirim yöntemleri bir awaiter'ın uygulaması `INotifyCompletion.OnCompleted` ve `ICriticalNotifyCompletion.UnsafeOnCompleted` temsilci edilmesini `r` en fazla bir kez çağrılacak. Aksi takdirde, kapsayan zaman uyumsuz işlev davranışı tanımsızdır.

## <a name="arithmetic-operators"></a>Aritmetik işleçler

`*`, `/`, `%`, `+`, Ve `-` işleçleri aritmetik işleçler çağrılır.

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

Bir aritmetik işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.

### <a name="multiplication-operator"></a>Çarpma işleci

Formun bir işlem için `x * y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Aşağıdaki önceden tanımlanmış çarpma işleçleri listelenir. Tüm işleçler çarpımını `x` ve `y`.

*  Tamsayı çarpma:

   ```csharp
   int operator *(int x, int y);
   uint operator *(uint x, uint y);
   long operator *(long x, long y);
   ulong operator *(ulong x, ulong y);
   ```

   İçinde bir `checked` ürün sonuç türü aralığının dışında ise bağlam bir `System.OverflowException` oluşturulur. İçinde bir `unchecked` bağlamı, taşan bildirilmez ve tüm önemli sonuç türü yüksek düzeyli bitleri aralığın dışında atılır.


*  Kayan nokta çarpma:

   ```csharp
   float operator *(float x, float y);
   double operator *(double x, double y);
   ```

   Ürün aritmetik IEEE 754 kurallarına göre hesaplanır. Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir. Tabloda `x` ve `y` sonlu pozitif değerler. `z` sonucu `x * y`. Sonucu hedef türü için çok büyük ise `z` sonsuz. Sonucu hedef türü için çok küçükse `z` sıfırdır.

   |      |      |      |     |     |      |      |     |
   |:----:|-----:|:----:|:---:|:---:|:----:|:----:|:----|
   |      | +y   | -y   | +0  | -0  | + INF | -INF | NaN | 
   | + x   | +z   | -z   | +0  | -0  | + INF | -INF | NaN | 
   | -x   | -z   | +z   | -0  | +0  | -INF | + INF | NaN | 
   | +0   | +0   | -0   | +0  | -0  | NaN  | NaN  | NaN | 
   | -0   | -0   | +0   | -0  | +0  | NaN  | NaN  | NaN | 
   | + INF | + INF | -INF | NaN | NaN | + INF | -INF | NaN | 
   | -INF | -INF | + INF | NaN | NaN | -INF | + INF | NaN | 
   | NaN  | NaN  | NaN  | NaN | NaN | NaN  | NaN  | NaN | 

*  Ondalık çarpma:

   ```csharp
   decimal operator *(decimal x, decimal y);
   ```

   Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur. Sonuç değeri içinde temsil etmek için çok küçük olup olmadığını `decimal` sonuç biçimi de sıfırdır. Bir yuvarlama önce bu sonucun ölçek iki işlenenden ölçekler toplamıdır.

   Ondalık çarpma çarpma işleci türünde kullanmaya eşdeğerdir `System.Decimal`.


### <a name="division-operator"></a>Bölme işleci

Formun bir işlem için `x / y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Önceden tanımlanmış bölme işleçlerini aşağıda listelenmiştir. Tüm işleçler kalanını işlem `x` ve `y`.

*  Tamsayı bölme:

   ```csharp
   int operator /(int x, int y);
   uint operator /(uint x, uint y);
   long operator /(long x, long y);
   ulong operator /(ulong x, ulong y);
   ```

   Sağ işlenen değeri sıfır olursa bir `System.DivideByZeroException` oluşturulur.

   Bölme işlemi sonucu sıfıra doğru yuvarlar. Bu nedenle sonuç mutlak değerini iki işlenenden sayının mutlak değerini eşit veya küçük en büyük olası tamsayı ' dir. İki işlenen de aynı işarete ve sıfır veya iki işlenenden işaretleri olduğunda negatif sıfır ya da pozitif sonuç olur.

   Sol işlenen en küçük gösterilebilir ise `int` veya `long` değeri ve sağ işlenen `-1`, taşma meydana gelir. İçinde bir `checked` bağlam, bu neden olur. bir `System.ArithmeticException` (veya bir alt yapanın) oluşturulması için. İçinde bir `unchecked` bağlamında mı kullanılacağına uygulama tarafından tanımlanan bir `System.ArithmeticException` (veya bir alt yapanın) oluşturulur veya taşma, sol işlenen olan sonuç değerle bildirilmeyen gider.

*  Kayan nokta bölme:

   ```csharp
   float operator /(float x, float y);
   double operator /(double x, double y);
   ```

   Sayının aritmetik IEEE 754 kurallarına göre hesaplanır. Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir. Tabloda `x` ve `y` sonlu pozitif değerler. `z` sonucu `x / y`. Sonucu hedef türü için çok büyük ise `z` sonsuz. Sonucu hedef türü için çok küçükse `z` sıfırdır.

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | + INF | -INF | NaN  | 
   | + x   | +z   | -z   | + INF | -INF | +0   | -0   | NaN  | 
   | -x   | -z   | +z   | -INF | + INF | -0   | +0   | NaN  | 
   | +0   | +0   | -0   | NaN  | NaN  | +0   | -0   | NaN  | 
   | -0   | -0   | +0   | NaN  | NaN  | -0   | +0   | NaN  | 
   | + INF | + INF | -INF | + INF | -INF | NaN  | NaN  | NaN  | 
   | -INF | -INF | + INF | -INF | + INF | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Ondalık bölme:

   ```csharp
   decimal operator /(decimal x, decimal y);
   ```

   Sağ işlenen değeri sıfır olursa bir `System.DivideByZeroException` oluşturulur. Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur. Sonuç değeri içinde temsil etmek için çok küçük olup olmadığını `decimal` sonuç biçimi de sıfırdır. Ölçek sonucu bir sonuç eşit korumak ölçeğin en küçük olan en yakın gerçek matematik sonucun ondalık gösterilebilir değere.

   Ondalık bölme bölme işleci türünde kullanmaya eşdeğerdir `System.Decimal`.


### <a name="remainder-operator"></a>Kalan işleci

Formun bir işlem için `x % y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Aşağıdaki önceden tanımlanmış kalan işleçleri listelenir. Tüm işleçleri arasında bölme kalanını işlem `x` ve `y`.

*  Tamsayı kalan:

   ```csharp
   int operator %(int x, int y);
   uint operator %(uint x, uint y);
   long operator %(long x, long y);
   ulong operator %(ulong x, ulong y);
   ```

   Sonucu `x % y` değeri tarafından üretilen `x - (x / y) * y`. Varsa `y` sıfır, bir `System.DivideByZeroException` oluşturulur.

   Sol işlenen en küçük ise `int` veya `long` değeri ve sağ işlenen `-1`, `System.OverflowException` oluşturulur. Hiçbir durumda mu `x % y` bir özel durum burada `x / y` bir özel durum oluşturması beklenmiyor.

*  Kayan nokta kalanını:

   ```csharp
   float operator %(float x, float y);
   double operator %(double x, double y);
   ```

   Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir. Tabloda `x` ve `y` sonlu pozitif değerler. `z` sonucu `x % y` ve olarak hesaplanan `x - n * y`burada `n` küçük veya eşit en büyük olası tamsayı `x / y`. Kalanı hesaplama, bu yöntem, tamsayı işlenenler için kullanılan benzer ancak IEEE 754 tanımından farklılık gösterir (burada `n` en yakın bir tamsayıdır `x / y`).

   |      |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | +y   | -y   | +0   | -0   | + INF | -INF | NaN  | 
   | + x   | +z   | +z   | NaN  | NaN  | x    | x    | NaN  | 
   | -x   | -z   | -z   | NaN  | NaN  | -x   | -x   | NaN  | 
   | +0   | +0   | +0   | NaN  | NaN  | +0   | +0   | NaN  | 
   | -0   | -0   | -0   | NaN  | NaN  | -0   | -0   | NaN  | 
   | + INF | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | -INF | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Ondalık kalan:

   ```csharp
   decimal operator %(decimal x, decimal y);
   ```

   Sağ işlenen değeri sıfır olursa bir `System.DivideByZeroException` oluşturulur. Bir yuvarlama önce bu sonucun ölçek büyük ölçekleri iki işlenenden biri ve sonucun oturum sıfır olmayan, aynı `x`.

   Ondalık kalan kalan işleci türünde kullanmaya eşdeğerdir `System.Decimal`.


### <a name="addition-operator"></a>Toplama işleci

Formun bir işlem için `x + y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Aşağıdaki önceden tanımlanmış toplama işleçleri listelenir. Sayısal ve sabit listesi türleri için önceden tanımlanmış toplama işleçleri iki işlenenden toplam hesaplaması. Bir veya iki işlenenin türü dize olmadığında, önceden tanımlı toplama işleçleri işlenenler dize gösterimini art arda ekler.

*  Tamsayı ayrıca:

   ```csharp
   int operator +(int x, int y);
   uint operator +(uint x, uint y);
   long operator +(long x, long y);
   ulong operator +(ulong x, ulong y);
   ```

   İçinde bir `checked` toplamı sonuç türü aralığının dışında ise bağlam bir `System.OverflowException` oluşturulur. İçinde bir `unchecked` bağlamı, taşan bildirilmez ve tüm önemli sonuç türü yüksek düzeyli bitleri aralığın dışında atılır.

*  Kayan nokta ekleme:

   ```csharp
   float operator +(float x, float y);
   double operator +(double x, double y);
   ```

   Toplam aritmetik IEEE 754 kurallarına göre hesaplanır. Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ın sonuçları aşağıdaki tabloda listelenmektedir. Tabloda `x` ve `y` sınırlı sıfır olmayan değerler ve `z` sonucu `x + y`. Varsa `x` ve `y` aynı büyüklük sahip ancak işaretlere ters `z` pozitif sıfır. Varsa `x + y` hedef türünde temsil etmek için çok büyük `z` aynı işarete sahip bir sonsuzluk olup `x + y`.

   |      |      |      |      |      |      |      |
   |:----:|:----:|:----:|:----:|:----:|:----:|:----:|
   |      | y    | +0   | -0   | + INF | -INF | NaN  | 
   | x    | z    | x    | x    | + INF | -INF | NaN  | 
   | +0   | y    | +0   | +0   | + INF | -INF | NaN  | 
   | -0   | y    | +0   | -0   | + INF | -INF | NaN  | 
   | + INF | + INF | + INF | + INF | + INF | NaN  | NaN  | 
   | -INF | -INF | -INF | -INF | NaN  | -INF | NaN  | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | 

*  Ondalık ayrıca:

   ```csharp
   decimal operator +(decimal x, decimal y);
   ```

   Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur. Bir yuvarlama önce bu sonucun ölçek büyük ölçekleri iki işlenenden biri ' dir.

   Ondalık toplama türü Toplama işleci kullanmaya eşdeğerdir `System.Decimal`.

*  Sabit listesi ekleme. Aşağıdaki önceden tanımlanmış işleçleri, her bir numaralandırma türü örtük olarak sağlar burada `E` sabit listesi türüdür ve `U` temel alınan türü `E`:

   ```csharp
   E operator +(E x, U y);
   E operator +(U x, E y);
   ```

   Çalışma zamanında tam olarak bu işleçler değerlendirilir `(E)((U)x + (U)y)`.

*  Dize birleştirme:

   ```csharp
   string operator +(string x, string y);
   string operator +(string x, object y);
   string operator +(object x, string y);
   ```

   Bu aşırı yüklemeler ikili `+` dize birleştirme işlecini gerçekleştirin. Dize birleştirme işleci ise `null`, boş bir dize yerine konur. Aksi takdirde, herhangi bir dize olmayan bağımsız değişken dize gösterimine sanal çağırarak dönüştürülür `ToString` yöntemi türden devralınan `object`. Varsa `ToString` döndürür `null`, boş bir dize yerine konur.

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

   Dize birleştirme işlecini sol işlenen sağ işlenenin karakterleridir karakterleri içeren bir dize sonucudur. Dize birleştirme işlecini hiç dönmüyor bir `null` değeri. A `System.OutOfMemoryException` sonuç dizesi ayırmak yeterli bellek yoksa oluşturulur.

*  Temsilci birleşimi. Her temsilci türü örtük olarak aşağıdaki önceden tanımlanmış işleç sağlar burada `D` temsilci türü:

   ```csharp
   D operator +(D x, D y);
   ```

   İkili `+` işleci, temsilci birleşimi her iki işlenen de bazı temsilci türünde olduğunda gerçekleştirir `D`. (İşlenenleri farklı temsilci türleri varsa, bir bağlama zamanı hatası oluşur.) Birinci işlenenin ise `null`, işlemin sonucunu ikinci işlenenin değeri (Ayrıca olsa bile `null`). Aksi durumda, ikinci işleneni ise `null`, işlemin sonucunu birinci işlenenin değeridir. Aksi takdirde, işlemin sonucunu yeni bir temsilci örneği, çağrılır, birinci işlenenin çağırır ve ardından ikinci işlenenin çağırır olur. Temsilci birleşimi örnekleri için bkz: [çıkarma işleci](expressions.md#subtraction-operator) ve [temsilci çağırma](delegates.md#delegate-invocation). Bu yana `System.Delegate` bir temsilci türü değil `operator`  `+` için tanımlı değil.

### <a name="subtraction-operator"></a>Çıkarma işleci

Formun bir işlem için `x - y`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Aşağıdaki önceden tanımlanmış çıkarma işleçleri listelenir. Tüm çıkarma işleçleri `y` gelen `x`.

*  Tamsayı çıkarma:

   ```csharp
   int operator -(int x, int y);
   uint operator -(uint x, uint y);
   long operator -(long x, long y);
   ulong operator -(ulong x, ulong y);
   ```

   İçinde bir `checked` fark sonuç türü aralığının dışında ise bağlam bir `System.OverflowException` oluşturulur. İçinde bir `unchecked` bağlamı, taşan bildirilmez ve tüm önemli sonuç türü yüksek düzeyli bitleri aralığın dışında atılır.

*  Kayan nokta çıkarma:

   ```csharp
   float operator -(float x, float y);
   double operator -(double x, double y);
   ```

   Fark aritmetik IEEE 754 kurallarına göre hesaplanır. Sıfır olmayan sınırlı değerler tüm olası eşleştirme birleşimlerini, sıfır sonsuz ve NaN'ler sonuçları aşağıdaki tabloda listelenmektedir. Tabloda `x` ve `y` sınırlı sıfır olmayan değerler ve `z` sonucu `x - y`. Varsa `x` ve `y` eşit olup olmadığını `z` pozitif sıfır. Varsa `x - y` hedef türünde temsil etmek için çok büyük `z` aynı işarete sahip bir sonsuzluk olup `x - y`.

   |      |      |      |      |      |      |     |
   |:----:|:----:|:----:|:----:|:----:|:----:|:---:|
   |      | y    | +0   | -0   | + INF | -INF | NaN | 
   | x    | z    | x    | x    | -INF | + INF | NaN | 
   | +0   | -y   | +0   | +0   | -INF | + INF | NaN | 
   | -0   | -y   | -0   | +0   | -INF | + INF | NaN | 
   | + INF | + INF | + INF | + INF | NaN  | + INF | NaN | 
   | -INF | -INF | -INF | -INF | -INF | NaN  | NaN | 
   | NaN  | NaN  | NaN  | NaN  | NaN  | NaN  | NaN | 

*  Ondalık çıkarma:

   ```csharp
   decimal operator -(decimal x, decimal y);
   ```

   Sonuçta elde edilen değeri içinde temsil etmek için çok büyük ise `decimal` biçiminde bir `System.OverflowException` oluşturulur. Bir yuvarlama önce bu sonucun ölçek büyük ölçekleri iki işlenenden biri ' dir.

   Ondalık çıkarma tür çıkarma işleci kullanmaya eşdeğerdir `System.Decimal`.

*  Sabit listesi çıkarma. Her bir numaralandırma türü örtük olarak aşağıdaki önceden tanımlanmış işleç sağlar burada `E` sabit listesi türüdür ve `U` temel alınan türü `E`:

   ```csharp
   U operator -(E x, E y);
   ```

   Bu işleç, tam olarak değerlendirilir `(U)((U)x - (U)y)`. Diğer bir deyişle, işlecin sıralı değerlerini arasındaki farkı hesaplar `x` ve `y`, ve sonuç türü temel numaralandırma türüdür.

   ```csharp
   E operator -(E x, U y);
   ```

   Bu işleç, tam olarak değerlendirilir `(E)((U)x - y)`. Diğer bir deyişle, işleci, bir numaralandırma değeri oluşturan bir sabit listesi türünü temel değeri çıkarır.

*  Temizleme temsilci. Her temsilci türü örtük olarak aşağıdaki önceden tanımlanmış işleç sağlar burada `D` temsilci türü:

   ```csharp
   D operator -(D x, D y);
   ```

   İkili `-` işleç, her iki işlenen de bazı temsilci türünde olduğunda temsilci kaldırma gerçekleştirir `D`. İşlenenleri farklı temsilci türleri varsa, bir bağlama zamanı hatası oluşur. Birinci işlenenin ise `null`, işlem sonucu `null`. Aksi durumda, ikinci işleneni ise `null`, işlemin sonucunu birinci işlenenin değeridir. Aksi takdirde, her iki işlenen de çağırma listeleri temsil eden ([temsilci bildirimi](delegates.md#delegate-declarations)) bir veya daha fazla giriş ve sonuç sahip olan ilk işlenen 's listesi kaldırıldığında ikinci işlenenin ait girişleri ile oluşan yeni bir çağırma listesi Bu ikinci işlenenin 's liste sağlanmıştır, uygun bir bitişik alt liste ilk ait olur.     (Alt liste eşitlik belirlemek için karşılık gelen girişler için temsilci eşitlik işlecini karşılaştırılır ([temsilci eşitlik işleçleri](expressions.md#delegate-equality-operators)).) Aksi halde sol işlenenin sonucudur. İşlenenleri listeleri hiçbiri işlemde değiştirilir. İkinci işlenenin 's listesiyle eşleşen birden çok alt listelerin ilk işlenen 's listesinde bitişik girişlerinin bitişik girişlerinin en sağdaki eşleşen alt liste kaldırılır. Kaldırma işlemi boş bir liste sonuçlanırsa sonucudur `null`. Örneğin:

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

`<<` Ve `>>` işleçler bit kaydırma işlemleri gerçekleştirmek için kullanılır.

```antlr
shift_expression
    : additive_expression
    | shift_expression '<<' additive_expression
    | shift_expression right_shift additive_expression
    ;
```

İşleci, bir *öğesinin* derleme zamanı türü `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.

Formun bir işlem için `x << count` veya `x >> count`, ikili İşleç aşırı yükleme çözümlemesi ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Aşırı yüklenen kaydırma işlecinin bildirirken, birinci işlenenin türü her zaman sınıfın veya yapının işleç bildirimi içeren olmalıdır ve ikinci işlenenin türü her zaman olmalıdır `int`.

Aşağıdaki önceden tanımlanmış kaydırma işleçleri listelenir.

*  Sola kaydırma:

   ```csharp
   int operator <<(int x, int count);
   uint operator <<(uint x, int count);
   long operator <<(long x, int count);
   ulong operator <<(ulong x, int count);
   ```

   `<<` İşleci kaydırmalar `x` aşağıda açıklandığı gibi sola bit sayısına göre hesaplanır.

   Sonuç türü aralık dışında yüksek düzeyli bitlerini `x` olan atılan, kalan bitleri sola kaydırılacak ve düşük düzey boş bit konumları sıfır olarak ayarlanır.

*  Sağa kaydırma:

   ```csharp
   int operator >>(int x, int count);
   uint operator >>(uint x, int count);
   long operator >>(long x, int count);
   ulong operator >>(ulong x, int count);
   ```

   `>>` İşleci kaydırmalar `x` aşağıda açıklandığı sağa bit sayısına göre hesaplanır.

   Zaman `x` türünde `int` veya `long`, alt sıra bitleri `x` olan atılan, kalan bitleri sağa kaydırılacak ve yüksek düzeyli boş bit konumları sıfır if ayarlanır `x` negatif olmayan ve bir halinde `x` negatiftir.

   Zaman `x` türünde `uint` veya `ulong`, alt sıra bitleri `x` olan atılan, kalan bitleri sağa kaydırılacak ve yüksek düzeyli boş bit konumları sıfır olarak ayarlanır.

Önceden tanımlı operatörler için kaydırılacak bit sayısını gibi hesaplanır:

*  Zaman türü `x` olduğu `int` veya `uint`, kaydırma sayısı düşük düzey beş bitleri tarafından verilen `count`. Kaydırma sayısı alanından başka bir deyişle, hesaplanan `count & 0x1F`.
*  Zaman türü `x` olduğu `long` veya `ulong`, kaydırma sayısı düşük düzey altı bitleri tarafından verilen `count`. Kaydırma sayısı alanından başka bir deyişle, hesaplanan `count & 0x3F`.

Elde edilen kaydırma sayısı sıfır ise, yalnızca dönüş değeri kaydırma işleçleri `x`.

Kaydırma işleçlerini hiçbir zaman taşıyor neden ve aynı sonuçlar `checked` ve `unchecked` bağlamı.

Zaman sol işleneni `>>` işleci imzalı bir tamsayı türünde, burada görüntülerle en önemli bite (imza biti) işlenenin değerini yayılan aritmetik kaydırma şu yüksek düzeyli boş bit konumları için işleç gerçekleştirir. Zaman sol işleneni `>>` işleci bir işaretsiz tamsayı türü, mantıksal kaydırma gibi yüksek düzeyli boş bit konumları her zaman ayarlanır sıfıra bir sağ işlecini uygular. İşlenen türü ortaya çıkan ters işlemi gerçekleştirmek için açık yayınları kullanılabilir. Örneğin, varsa `x` türünde bir değişken `int`, işlemi `unchecked((int)((uint)x >> y))` sağında bir mantıksal kaydırma uygular `x`.

## <a name="relational-and-type-testing-operators"></a>İlişkisel ve tür testi işleçleri

`==`, `!=`, `<`, `>`, `<=`, `>=`, `is` Ve `as` işleçleri, ilişkisel ve tür testi işleçleri denir.

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

`is` İşleci açıklanan [işleci](expressions.md#the-is-operator) ve `as` işleci açıklanan [işleci olarak](expressions.md#the-as-operator).

`==`, `!=`, `<`, `>`, `<=` Ve `>=` işleçleri ***Karşılaştırma işleçleri***.

Bir karşılaştırma işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.

Formun bir işlem için `x` *op* `y`burada *op* bir karşılaştırma işleci, aşırı yükleme çözünürlüğü ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Önceden tanımlanmış Karşılaştırma işleçleri aşağıdaki bölümlerde açıklanmıştır. Tüm önceden tanımlanmış Karşılaştırma işleçleri bir sonuç türü `bool`aşağıdaki tabloda açıklandığı gibi.


| __İşlem__ | __Sonuç__                                                       |
|---------------|------------------------------------------------------------------|
| `x == y`      | `true` varsa `x` eşittir `y`, `false` Aksi takdirde                 | 
| `x != y`      | `true` varsa `x` eşit değildir `y`, `false` Aksi takdirde             | 
| `x < y`       | `true` varsa `x` olduğu küçüktür `y`, `false` Aksi takdirde                | 
| `x > y`       | `true` varsa `x` büyüktür `y`, `false` Aksi takdirde             | 
| `x <= y`      | `true` varsa `x` küçüktür veya eşittir `y`, `false` Aksi takdirde    | 
| `x >= y`      | `true` varsa `x` büyüktür veya eşittir `y`, `false` Aksi takdirde | 

### <a name="integer-comparison-operators"></a>Tamsayı Karşılaştırma işleçleri

Önceden tanımlanmış tamsayı Karşılaştırma işleçleri şunlardır:
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

Bu işleçlerden her biri sayısal değerleri döndürür ve iki tamsayı işlenenler karşılaştıran bir `bool` belirli ilişki olup olmadığını belirten bir değer `true` veya `false`.

### <a name="floating-point-comparison-operators"></a>Kayan nokta Karşılaştırma işleçleri

Önceden tanımlanmış bir kayan nokta Karşılaştırma işleçleri şunlardır:
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

İşleçler işlenenlerin standart IEEE 754 kurallarına göre karşılaştırın:

*  İki işlenenden NaN ise, sonucudur `false` tüm işleçleri `!=`, sonucu olan `true`. Her iki işlenenleri için `x != y` her zaman aynı sonucu üretir `!(x == y)`. Ancak, bir veya iki işlenenin olduğunda, NaN `<`, `>`, `<=`, ve `>=` işleçleri mantıksal olumsuzlama ters işleci,'dekiyle aynı sonucu üretir değil. Örneğin, ya da, `x` ve `y` NaN ise `x < y` olduğu `false`, ancak `!(x >= y)` olduğu `true`.
*  İki işlenenin NaN olduğunda, işleçleri değerleri sıralama göre kayan nokta iki işlenenden karşılaştırın.

   ```
   -inf < -max < ... < -min < -0.0 == +0.0 < +min < ... < +max < +inf
   ```

   Burada `min` ve `max` belirli kayan nokta biçiminde temsil edilebilir en küçük ve büyük pozitif sınırlı değerlerdir. Bu sıralama önemli etkileri verilmiştir:
   * Negatif ve pozitif sıfır eşit olarak kabul edilir.
   * Negatif sonsuz küçük diğer tüm değerler daha ancak başka bir negatif sonsuz eşit olarak kabul edilir.
   * Diğer tüm değerler büyüktür, ancak başka bir pozitif sonsuz eşit bir pozitif sonsuz olarak kabul edilir.

### <a name="decimal-comparison-operators"></a>Ondalık Karşılaştırma işleçleri

Önceden tanımlanmış ondalık Karşılaştırma işleçleri şunlardır:
```csharp
bool operator ==(decimal x, decimal y);
bool operator !=(decimal x, decimal y);
bool operator <(decimal x, decimal y);
bool operator >(decimal x, decimal y);
bool operator <=(decimal x, decimal y);
bool operator >=(decimal x, decimal y);
```

Bu işleçlerden her biri iki ondalık işlenenler ve döndürür, sayısal değerler karşılaştıran bir `bool` belirli ilişki olup olmadığını belirten bir değer `true` veya `false`. Her bir ondalık karşılaştırma karşılık gelen ilişkisel veya eşitlik işleci türünde kullanmakla eşdeğerdir `System.Decimal`.

### <a name="boolean-equality-operators"></a>Boole eşitlik işleçleri

Önceden tanımlanmış Boole eşitlik işleçleri şunlardır:
```csharp
bool operator ==(bool x, bool y);
bool operator !=(bool x, bool y);
```

Sonucu `==` olduğu `true` hem `x` ve `y` olan `true` veya her iki `x` ve `y` olan `false`. Aksi halde sonuç, `false`.

Sonucu `!=` olduğu `false` hem `x` ve `y` olan `true` veya her iki `x` ve `y` olan `false`. Aksi halde sonuç, `true`. İşlenen türü olduğunda `bool`, `!=` işleci aynı sonucu üretir `^` işleci.

### <a name="enumeration-comparison-operators"></a>Sabit listesi Karşılaştırma işleçleri

Her bir numaralandırma türü örtük olarak aşağıdaki önceden tanımlanmış Karşılaştırma işleçleri sağlar:
```csharp
bool operator ==(E x, E y);
bool operator !=(E x, E y);
bool operator <(E x, E y);
bool operator >(E x, E y);
bool operator <=(E x, E y);
bool operator >=(E x, E y);
```

Değerlendirme sonucu `x op y`burada `x` ve `y` ifadeler bir numaralandırma türünün `E` bir temel türü ile `U`, ve `op` Karşılaştırma işleçleri biridir, tamamen aynı. Değerlendirme `((U)x) op ((U)y)`. Diğer bir deyişle, sabit listesi türü Karşılaştırma işleçleri, yalnızca arka plandaki tamsayı değerlerini iki işlenenden karşılaştırın.

### <a name="reference-type-equality-operators"></a>Başvuru türü eşitlik işleçleri

Önceden tanımlı başvuru türü eşitlik işleçleri şunlardır:
```csharp
bool operator ==(object x, object y);
bool operator !=(object x, object y);
```

İşleçler, karşılaştırma eşitlik ya da olmayan eşitlik için iki başvuru sonucunu döndürür.

Önceden tanımlı başvuru türü eşitlik işleçleri türünde işlenenleri kabul beri `object`, bunlar uygun bildirmeyin tüm türleri için geçerli `operator ==` ve `operator !=` üyeleri. Buna karşılık, herhangi bir geçerli kullanıcı tanımlı eşitlik işlecini etkili bir şekilde önceden tanımlı başvuru türü eşitlik işleçleri gizleyin.

Önceden tanımlı başvuru türü eşitlik işleçleri aşağıdakilerden birini gerektirir:

*  Her iki işlenen de olduğu bilinen bir türde bir değer olan bir *reference_type* veya sabit değer `null`. Ayrıca, bir açık bir başvuru dönüştürmesi ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)) diğer işlenen türü için iki işlenenden türünden bulunmaktadır.
*  Tek bir işlenen türünde bir değer olan `T` burada `T` olduğu bir *type_parameter* ve diğer işlenen değişmez değer `null`. Ayrıca `T` değer türü kısıtlaması yok.

Bu koşullardan biri olmadığı sürece true, bir bağlama zamanı hatası oluşur. Bu kurallar önemli etkileri verilmiştir:

*  Bu önceden tanımlı başvuru türü eşitlik işleçleri bağlama zamanında farklı olduğu bilinen iki başvuru karşılaştırmak için kullanılacak bir bağlama zamanı hatasıdır. Örneğin, iki sınıf türleri bağlama zamanı türleridir işlenenden `A` ve `B`ve ne `A` ya da `B` iki işlenenden aynı nesneye başvurmak imkansız sonra diğerinden, türetilir. Bu nedenle, işlemi bir bağlama zamanı hata olarak kabul edilir.
*  Önceden tanımlı başvuru türü eşitlik işleçleri değer türü işlenen Karşılaştırılacak izin vermez. Bir yapı türü kendi eşitlik işleçleri bildirir sürece, bu nedenle, bu yapı türünün değerlerini karşılaştırmak mümkün değildir.
*  Önceden tanımlı başvuru türü eşitlik işleçleri asla kutulama işlemleri, işlenenleri için oluşmasına neden. Bu yana yeni ayrılan paketlenmiş örneklerine başvuruları mutlaka diğer tüm başvurularından farklı gibi kutulama işlemleri gerçekleştirmek için etmezdi.
*  Bir tür parametresi türünde bir işlenen, `T` karşılaştırılır `null`ve çalışma zamanı türü `T` bir değer türüdür karşılaştırmanın sonucudur `false`.

Aşağıdaki örnek, sınırlandırılmamış türü parametre türünde bir bağımsız değişken olup olmadığını denetler `null`.
```csharp
class C<T>
{
    void F(T x) {
        if (x == null) throw new ArgumentNullException();
        ...
    }
}
```

`x == null` Yapısı olsa bile izin `T` bir değer türü temsil edebilir ve sonucu yalnızca olarak tanımlanır `false` olduğunda `T` bir değer türüdür.

Formun bir işlem için `x == y` veya `x != y`, her varsa `operator ==` veya `operator !=` yoksa, İşleç aşırı yükleme çözünürlüğü ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) kuralları, seçer önceden tanımlı başvuru türü eşitlik işlecini yerine işleci. Ancak, her zaman önceden tanımlı başvuru türü eşitlik işlecini birini veya her ikisini işlenenler türüne açıkça atayarak seçmek mümkündür `object`. Örnek
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
```
True
False
False
False
```

`s` Ve `t` değişkenleri başvuran iki farklı `string` aynı karakterleri içeren örnekleri. İlk karşılaştırma çıkarır `True` çünkü önceden tanımlı dize eşitlik işlecini ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) her iki işlenen türünde olduğunda seçili `string`. Tüm diğer karşılaştırmalar çıkış `False` önceden tanımlı başvuru türü eşitlik işlecini bir veya iki işlenenden türünde olduğunda seçilmediğinden `object`.

Yukarıdaki tekniği değer türleri için anlamlı olmadığını unutmayın. Örnek
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
çıkaran `False` yayınları iki ayrı örneklerini başvuruları oluşturduğundan Kutulu `int` değerleri.

### <a name="string-equality-operators"></a>Dize eşitlik işleçleri

Önceden tanımlı dize eşitlik işleçleri şunlardır:
```csharp
bool operator ==(string x, string y);
bool operator !=(string x, string y);
```

İki `string` aşağıdakilerden biri doğruysa, değerleri eşit değerlendirilir:

*  Her iki değerler `null`.
*  Her iki aynı uzunluk ve aynı karakterlerin her karakter konumunda sahip dize örnekleri null olmayan başvurular değerlerdir.

Dize başvurular yerine dize değerleri dize eşitlik işleçleri karşılaştırın. İki ayrı bir dize örneği tam olarak aynı karakter dizisi içerdiğinde, dizelerin değerlerin eşit olup, ancak başvuruları farklıdır. Bölümünde anlatıldığı gibi [başvuru türü eşitlik işleçleri](expressions.md#reference-type-equality-operators), başvuru türü eşitlik işleçleri dize başvurular yerine dize değerleri karşılaştırmak için kullanılabilir.

### <a name="delegate-equality-operators"></a>Temsilci eşitlik işleçleri

Her temsilci türü örtük olarak aşağıdaki önceden tanımlanmış Karşılaştırma işleçleri sağlar:

```csharp
bool operator ==(System.Delegate x, System.Delegate y);
bool operator !=(System.Delegate x, System.Delegate y);
```

İki temsilci örneklerini şu şekilde eşit olarak kabul edilir:

*  Temsilci örneklerini biri geçerli olduğunda `null`, her ikisi de ve yalnızca, eşit oldukları `null`.
*  Hiçbir zaman eşit temsilcileri farklı çalışma zamanı türü varsa.
*  Her iki temsilci örneklerini çağrı listesine sahip olup ([temsilci bildirimi](delegates.md#delegate-declarations)), çağırma listelerini aynı uzunlukta olan ve her birinin çağırma listesindeki girdinin (yukarıda tanımlandığı şekilde) eşittir ve yalnızca, bu örneğinin eşit olup karşılık gelen bir giriş için sırayla diğer tarafın çağırma listesi.

Aşağıdaki kurallar çağırma Liste girişlerini eşitliğine yöneten:

*  İki çağırma Liste girişlerini hem aynı statik başvuruyorsa yöntemi sonra girişleri eşit olur.
*  İki çağırma Liste girişlerini hem aynı hedef nesne üzerinde aynı statik olmayan yöntemi (referans eşitlik operatörleri tarafından tanımlandığı gibi) başvurursanız girişleri eşit olur.
*  Anlamsal olarak özdeş bir değerlendirmesinden gelen çağırma Liste girişlerini üretilen *anonymous_method_expression*s veya *lambda_expression*s yakalanan dış değişken aynı (büyük olasılıkla boş) kümesi örnekleri izin verilir (ancak gerekli değildir) eşit.

### <a name="equality-operators-and-null"></a>Eşitlik işleçleri ve null

`==` Ve `!=` işleçleri, tek bir işlenen boş değer atanabilir bir tür ve diğer olması için bir değer olmasını izin `null` işlemi için (içinde unlifted veya form yükseltilmiş) önceden tanımlı veya kullanıcı tanımlı hiçbir işleç bulunuyor olsa bile değişmez.

Bir form için bir işlem
```csharp
x == null
null == x
x != null
null != x
```
Burada `x` işleci aşırı yükleme çözümlemesi bir ifade, null yapılabilir bir tür ise ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) başarısız sonucu geçerli bir işleç bulmak için gelen hesaplanan yerine `HasValue` özelliği `x`. Özellikle, ilk iki forms çevrilir `!x.HasValue`, ve son iki forms çevrilmiş içine `x.HasValue`.

### <a name="the-is-operator"></a>Is işleci

`is` İşleci, dinamik olarak bir nesnenin çalışma zamanı türü belirli bir tür ile uyumlu olup olmadığını denetlemek için kullanılır. İşlemin sonucu `E is T`burada `E` ifade ve `T` bir tür, bir Boole değeri belirten değer olup olmadığını `E` başarıyla türüne dönüştürülebilir `T` bir başvuru dönüştürmesi, bir paketleme tarafından dönüştürme veya bir kutudan çıkarma dönüştürme. Tüm tip parametreleri için tür bağımsız değişkeni yapıştırıyorsanız sonra işlemi aşağıdaki gibi değerlendirilir:

*  Varsa `E` bir derleme zamanı hatası oluşur anonim bir işlev ise
*  Varsa `E` bir yöntem grubu olduğundan veya `null` biri değişmez değer türü `E` bir başvuru türü veya boş değer atanabilir bir tür ve değerini `E` olan null, sonucun false olduğu.
*  Aksi takdirde, izin `D` dinamik türünü temsil eden `E` gibi:
   * Varsa türünü `E` bir başvuru türüdür `D` örneği başvuruya göre çalışma zamanı türü `E`.
   * Varsa türünü `E` boş değer atanabilir bir tür `D` temel alınan türü, boş değer atanabilir bir tür.
   * Varsa türünü `E` bir NULL olmayan değer türü `D` türü `E`.
*  İşlemin sonucu bağımlı `D` ve `T` şu şekilde:
   * Varsa `T` bir başvuru türü ise sonucun true olduğu, `D` ve `T` aynı türde olan `D` bir başvuru türü ve bir örtük bir başvuru dönüştürme `D` için `T` var, veya `D` bir değer türü ve kutulama dönüştürme `D` için `T` bulunmaktadır.
   * Varsa `T` boş değer atanabilir bir tür olduğunda sonucun true olduğu, `D` temel alınan türü `T`.
   * Varsa `T` bir NULL olmayan değer türü sonucun true olduğu, `D` ve `T` aynı türdeyse.
   * Aksi takdirde sonuç yanlış olur.

Kullanıcı tanımlı dönüştürmeler dikkate alınmaz tarafından olduğunu unutmayın `is` işleci.

### <a name="the-as-operator"></a>AS işleci

`as` İşleci bir verilen başvuru türüyle veya null yapılabilir bir tür için bir değer açıkça dönüştürmek için kullanılır. Cast ifadesi aksine ([Cast ifadeleri](expressions.md#cast-expressions)), `as` işleci hiçbir zaman bir özel durum oluşturur. Bunun yerine, belirtilen dönüştürme mümkün değilse, sonuç değeri olan `null`.

Formun işleminde `E as T`, `E` bir ifade olmalı ve `T` bir başvuru türü bir başvuru türüyle veya null yapılabilir bir tür olarak bilinen bir tür parametresi olması gerekir. Ayrıca, aşağıdakilerden en az biri true ya da aksi takdirde bir derleme zamanı hatası oluşur:

*  Bir kimlik ([kimlik dönüştürme](conversions.md#identity-conversion)), örtük boş değer atanabilir ([boş değer atanabilir örtük dönüştürmelerin](conversions.md#implicit-nullable-conversions)), örtük bir başvuru ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)), (kutulama[ Kutulama dönüştürmeler](conversions.md#boxing-conversions)), açık boş değer atanabilir ([açık boş değer atanabilir dönüştürmeler](conversions.md#explicit-nullable-conversions)), açık bir başvuru ([açık başvuru dönüşümleri](conversions.md#explicit-reference-conversions)), veya kutudan çıkarma ([Kutudan çıkarma dönüştürme](conversions.md#unboxing-conversions)) dönüştürme var gelen `E` için `T`.
*  Türünü `E` veya `T` bir açık türdür.
*  `E` olan `null` değişmez.

Derleme zamanı türü `E` değil `dynamic`, işlemi `E as T` aynı sonucu üretir
```csharp
E is T ? (T)(E) : (T)null
```
dışında `E` yalnızca bir kez değerlendirilir. Derleyici, en iyi duruma getirme beklenebilir `E as T` yukarıdaki genişletme tarafından kapsanan iki dinamik tür denetimlerini aksine en fazla bir dinamik tür denetimi gerçekleştirmek için.

Derleme zamanı türü `E` olduğu `dynamic`, atama işleci aksine `as` işleci değil dinamik olarak bağlı ([dinamik bağlama](expressions.md#dynamic-binding)). Bu nedenle bu durumda genişletme şöyledir:
```csharp
E is T ? (T)(object)(E) : (T)null
```

Kullanıcı tanımlı dönüştürmeler gibi bazı dönüştürmeler mümkün olmadığını unutmayın `as` işleci ve bunun yerine cast ifadeleri kullanarak gerçekleştirilmelidir.

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
tür parametresi `T` , `G` bir başvuru türü olması için sınıf kısıtlaması içerdiğinden bilinir. Tür parametresi `U` , `H` kullanımını değil ancak; bu nedenle `as` işlecinde `H` izin verilmiyor.

## <a name="logical-operators"></a>Mantıksal işleçler

`&`, `^`, Ve `|` işleçleri mantıksal işleçler çağrılır.

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

Bir mantıksal işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.

Formun bir işlem için `x op y`burada `op` mantıksal işleçler, aşırı yükleme çözünürlüğü biridir ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) belirli işleci uygulama seçmek için uygulanır. İşlenenler seçilen operatöre parametre türleri dönüştürülür ve sonuç türü işlecinin dönüş türüdür.

Önceden tanımlanmış mantıksal işleçleri aşağıdaki bölümlerde açıklanmıştır.

### <a name="integer-logical-operators"></a>Tamsayı mantıksal işleçler

Önceden tanımlanmış tamsayı mantıksal işleçler şunlardır:
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

`&` İşleci hesaplar. bit düzeyinde mantıksal `AND` iki işlenenden `|` işleci hesaplar. bit düzeyinde mantıksal `OR` iki işlenenden ve `^` işleci mantıksal bit düzeyinde özel hesaplar `OR` iki işlenenden. Bu işlemlerini hiçbir taşıyor mümkündür.

### <a name="enumeration-logical-operators"></a>Mantıksal işleçler numaralandırması

Her bir numaralandırma türü `E` örtük olarak aşağıdaki önceden tanımlanmış mantıksal işleçleri sağlar:

```csharp
E operator &(E x, E y);
E operator |(E x, E y);
E operator ^(E x, E y);
```

Değerlendirme sonucu `x op y`burada `x` ve `y` ifadeler bir numaralandırma türünün `E` bir temel türü ile `U`, ve `op` mantıksal işleçler biridir, tamamen aynı. Değerlendirme `(E)((U)x op (U)y)`. Diğer bir deyişle, sabit listesi türü mantıksal işleçler yalnızca iki işlenenden temel alınan türü üzerinde mantıksal işlem gerçekleştirin.

### <a name="boolean-logical-operators"></a>Boolean mantıksal işleçler

Önceden tanımlanmış boolean mantıksal işleçler şunlardır:
```csharp
bool operator &(bool x, bool y);
bool operator |(bool x, bool y);
bool operator ^(bool x, bool y);
```

Sonucu `x & y` olduğu `true` hem `x` ve `y` olan `true`. Aksi halde sonuç, `false`.

Sonucu `x | y` olduğu `true` ya da `x` veya `y` olduğu `true`. Aksi halde sonuç, `false`.

Sonucu `x ^ y` olduğu `true` varsa `x` olduğu `true` ve `y` olduğu `false`, veya `x` olan `false` ve `y` olan `true`. Aksi halde sonuç, `false`. İşlenen türü olduğunda `bool`, `^` işleci aynı sonucu hesaplar `!=` işleci.

### <a name="nullable-boolean-logical-operators"></a>Boş değer atanabilir boolean mantıksal işleçler

Boş değer atanabilir bir boolean türü `bool?` üç değerleri temsil edebilen `true`, `false`, ve `null`ve SQL boolean ifadeler için kullanılan üç değerli türü kavramsal olarak benzer. Tarafından üretilen sonuçlar emin olmak için `&` ve `|` işleçleri `bool?` işlenenler SQL'in üç değerli mantığı ile tutarlı, aşağıdaki önceden tanımlı operatörler sağlanır:

```csharp
bool? operator &(bool? x, bool? y);
bool? operator |(bool? x, bool? y);
```

Bu işleçler için tüm değerleri birleşimlerini tarafından üretilen sonuçları aşağıdaki tabloda listelenmektedir `true`, `false`, ve `null`.

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

`&&` Ve `||` işleçleri koşullu mantıksal işleçler çağrılır. "Short-circuiting" mantıksal işleçler de çağrılır.

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

`&&` Ve `||` işleçler şunlardır: koşullu sürümleri `&` ve `|` işleçleri:

*  İşlemi `x && y` işlemine karşılık gelir `x & y`dışında `y` yalnızca değerlendirilir `x` değil `false`.
*  İşlemi `x || y` işlemine karşılık gelir `x | y`dışında `y` yalnızca değerlendirilir `x` değil `true`.

Koşullu bir mantıksal işlecinin bir işleneni derleme zamanı türü olup olmadığını `dynamic`, ifadeyi dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı ifadenin türüdür `dynamic`, ve çalışma zamanında derleme zamanı türü bu işlenenler çalışma zamanı türünü kullanarak aşağıda açıklanan çözümleme gerçekleşecek `dynamic`.

Form işlemini `x && y` veya `x || y` aşırı yükleme çözünürlüğü uygulama tarafından işlenen ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) işlemi olarak yazılmışsa `x & y` veya `x | y`. Then,

*  Tek bir en iyi işleci bulmak aşırı yükleme çözümlemesi başarısız olursa veya aşırı yükleme çözünürlüğü önceden tanımlanmış tamsayı mantıksal işleçler seçerse, bir bağlama zamanı hatası oluşur.
*  Aksi takdirde, seçilen operatöre önceden tanımlanmış boolean mantıksal işleçler ise ([Boolean mantıksal işleçler](expressions.md#boolean-logical-operators)) veya boş değer atanabilir boolean mantıksal işleçler ([null, boolean mantıksal işleçler](expressions.md#nullable-boolean-logical-operators)), işlem bölümünde anlatıldığı gibi işlenir [Boole koşullu mantıksal işleçleri](expressions.md#boolean-conditional-logical-operators).
*  Aksi takdirde, seçilen operatöre bir kullanıcı tanımlı işleç ve işlem bölümünde anlatıldığı gibi işlenir [kullanıcı tanımlı koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators).

Doğrudan koşullu mantıksal işleçler aşırı mümkün değildir. Koşullu mantıksal işleçler normal mantıksal işleçler bakımından değerlendirildiği için ancak normal mantıksal işleçler aşırı belirli kısıtlamalar ile de koşullu mantıksal işleçler aşırı yüklemeleri olarak kabul edilir. Daha ayrıntılı açıklanır budur [kullanıcı tanımlı koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators).

### <a name="boolean-conditional-logical-operators"></a>Boole koşullu mantıksal işleçleri

Zaman işleneni `&&` veya `||` türü `bool`, veya uygun bir tanımlamazsanız türde olduğunda işlenenler `operator &` veya `operator |`, ancak örtük dönüştürme `bool`, işlem şu şekilde işlenir:

*  İşlemi `x && y` değerlendirmesinde `x ? y : false`. Diğer bir deyişle, `x` önce değerlendirilir ve türüne dönüştürülemediğinden `bool`. Ardından, eğer `x` olduğu `true`, `y` değerlendirilir ve türüne dönüştürülemediğinden `bool`, ve bu işlemin sonucunu olur. Aksi takdirde işlemi sonucudur `false`.
*  İşlemi `x || y` değerlendirmesinde `x ? true : y`. Diğer bir deyişle, `x` önce değerlendirilir ve türüne dönüştürülemediğinden `bool`. Ardından, eğer `x` olduğu `true`, işlem sonucu `true`. Aksi takdirde, `y` değerlendirilir ve türüne dönüştürülemediğinden `bool`, ve bu işlemin sonucunu olur.

### <a name="user-defined-conditional-logical-operators"></a>Kullanıcı tanımlı koşullu mantıksal işleçler

Olduğunda, işlenenler `&&` veya `||` uygun bir bildirme türleri kullanıcı tanımlı `operator &` veya `operator |`, aşağıdaki her iki burada true olmalıdır `T` seçilen operatöre bildirilmiş türü:

*  Dönüş türü ile seçilen işlecinin her parametresinin türü olmalıdır `T`. Diğer bir deyişle, işlecin mantıksal işlem gerekir `AND` veya mantıksal `OR` türünde iki işlenenden `T`ve bir sonuç türü döndürmelidir `T`.
*  `T` bildirimleri içermelidir `operator true` ve `operator false`.

Bu iki gereksinimi koşullar karşılanırsa bir bağlama zamanı hatası oluşur. Aksi takdirde, `&&` veya `||` işlemi, kullanıcı tanımlı birleştirerek değerlendirilir `operator true` veya `operator false` seçili kullanıcı tanımlı işleç ile:

*  İşlemi `x && y` değerlendirmesinde `T.false(x) ? x : T.&(x, y)`burada `T.false(x)` bir çağrıdır, `operator false` bildirilen `T`, ve `T.&(x, y)` bir çağrıdır seçili `operator &`. Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `operator false` sonucunu belirlemek için çağrılır `x` kesinlikle false'tur. Ardından, eğer `x` işleminin sonucu için önceden hesaplanan değerdir kesinlikle yanlış `x`. Aksi takdirde, `y` değerlendirilir ve seçili `operator &` için önceden hesaplanan değer çağrılır `x` ve için hesaplanan değer `y` işlemin sonucu verecek.
*  İşlemi `x || y` değerlendirmesinde `T.true(x) ? x : T.|(x, y)`burada `T.true(x)` bir çağrıdır, `operator true` bildirilen `T`, ve `T.|(x,y)` bir çağrıdır seçili `operator|`. Diğer bir deyişle, `x` ilk olarak değerlendirilir ve `operator true` sonucunu belirlemek için çağrılır `x` kesinlikle geçerlidir. Ardından, eğer `x` işleminin sonucu için önceden hesaplanan değerdir kesinlikle true `x`. Aksi takdirde, `y` değerlendirilir ve seçili `operator |` için önceden hesaplanan değer çağrılır `x` ve için hesaplanan değer `y` işlemin sonucu verecek.

Bu işlemler tarafından sağlanan ifadenin birini `x` yalnızca bir kez değerlendirilir ve tarafından sağlanan ifadenin `y` olmayan hesaplanan veya tam bir kez değerlendirilir.

Uygulayan bir tür örneği `operator true` ve `operator false`, bkz: [veritabanı Boole türü](structs.md#database-boolean-type).

## <a name="the-null-coalescing-operator"></a>Null birleşim işleci

`??` İşleci, null birleşim işleci çağrılır.

```antlr
null_coalescing_expression
    : conditional_or_expression
    | conditional_or_expression '??' null_coalescing_expression
    ;
```

Bir null birleşim ifadesi formun `a ?? b` gerektirir `a` boş değer atanabilir bir tür ya da başvuru türünde olması. Varsa `a` kullanmaktan, sonucunu `a ?? b` olduğu `a`; Aksi takdirde sonuç `b`. İşlemi değerlendirir `b` yalnızca `a` null.

Null birleşim işleci sağla ilişkilendirilebilir, işlemler soldan sağa gruplandırılır anlamına gelir. Örneğin, bir ifade formun `a ?? b ?? c` değerlendirmesinde `a ?? (b ?? c)`. Genel koşulları, bir ifade formun `E1 ?? E2 ?? ... ?? En` ilk işlenenden null olmayan veya tüm işlenenler null ise null döndürür.

İfadenin türü `a ?? b` hangi örtük dönüştürmelerin işlenenler üzerinde kullanılabilir olduğuna bağlıdır. Türünü tercih sırasına göre `a ?? b` olduğu `A0`, `A`, veya `B`burada `A` türü `a` (koşuluyla `a` türüne sahip), `B` türü `b` ( koşuluyla `b` türüne sahip), ve `A0` temel alınan türü `A` varsa `A` boş değer atanabilir bir tür veya `A` Aksi takdirde. Özellikle, `a ?? b` şu şekilde işlenir:

*  Varsa `A` var ve boş değer atanabilir bir tür veya bir başvuru türünün bir derleme zamanı hatası oluşur.
*  Varsa `b` dinamik bir ifade sonucu türü `dynamic`. Çalışma zamanında, `a` ilk olarak değerlendirilir. Varsa `a` null değil `a` dinamik diske dönüştürülür ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve bu sonuç olur.
*  Aksi takdirde `A` var ve boş değer atanabilir bir tür ve öğesinden örtük bir dönüştürme yok `b` için `A0`, sonuç türü olan `A0`. Çalışma zamanında, `a` ilk olarak değerlendirilir. Varsa `a` null değil `a` yazmak için sarmalanmamış `A0`, ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve türüne dönüştürülemediğinden `A0`, ve bu sonuç olur.
*  Aksi takdirde `A` var ve gelen örtük bir dönüştürme var `b` için `A`, sonuç türü olan `A`. Çalışma zamanında, `a` ilk olarak değerlendirilir. Varsa `a` null değil `a` sonuç olur. Aksi takdirde, `b` değerlendirilir ve türüne dönüştürülemediğinden `A`, ve bu sonuç olur.
*  Aksi takdirde `b` bir türe sahip `B` ve gelen örtük bir dönüştürme var `a` için `B`, sonuç türü olan `B`. Çalışma zamanında, `a` ilk olarak değerlendirilir. Varsa `a` null değil `a` yazmak için sarmalanmamış olur `A0` (varsa `A` yok ve null olabilir) ve türüne dönüştürülmüş `B`, ve bu sonuç olur. Aksi takdirde, `b` değerlendirilir ve sonuç olur.
*  Aksi takdirde, `a` ve `b` uyumlu olmayan ve bir derleme zamanı hatası oluşur.

## <a name="conditional-operator"></a>Koşullu işleç

`?:` İşleç, koşullu işleç çağrılır. Bazen de Üçlü işleç adı verilir.

```antlr
conditional_expression
    : null_coalescing_expression
    | null_coalescing_expression '?' expression ':' expression
    ;
```

Bir koşullu ifade formun `b ? x : y` önce koşulu değerlendirir `b`. Ardından, eğer `b` olduğu `true`, `x` değerlendirilir ve işlemin sonucu olur. Aksi takdirde, `y` değerlendirilir ve işlemin sonucu olur. Bir koşullu ifade hiçbir zaman hem de değerlendirir `x` ve `y`.

Koşullu işleç sağla ilişkilendirilebilir, işlemler soldan sağa gruplandırılır anlamına gelir. Örneğin, bir ifade formun `a ? b : c ? d : e` değerlendirmesinde `a ? b : (c ? d : e)`.

İlk işleneni `?:` işleci, örtük olarak dönüştürülebilir bir ifade olmalıdır `bool`, veya bir ifade uygulayan bir tür `operator true`. Bu gereksinimleri hiçbiri sağlanıyorsa, bir derleme zamanı hatası oluşur.

İkinci ve üçüncü işlenenleri `x` ve `y`, biri `?:` işleci denetleyen koşullu ifadenin türü.

*  Varsa `x` türünde `X` ve `y` türünde `Y` sonra
   * Örtük bir dönüştürme ise ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) öğesinden var. `X` için `Y`, ancak `Y` için `X`, ardından `Y` koşullu ifadenin türü.
   * Örtük bir dönüştürme ise ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) öğesinden var. `Y` için `X`, ancak `X` için `Y`, ardından `X` koşullu ifadenin türü.
   * Aksi takdirde, hiçbir ifade türü belirlenebilir ve bir derleme zamanı hatası oluşur.
*  Yalnızca birini `x` ve `y` bir tür ve her ikisi de sahip `x` ve `y`, koşullu ifadenin türü ise, bu türe örtük olarak dönüştürülebilir olduğundan.
*  Aksi takdirde, hiçbir ifade türü belirlenebilir ve bir derleme zamanı hatası oluşur.

Bir koşullu ifade formun çalışma zamanı işlenmesini `b ? x : y` aşağıdaki adımlardan oluşur:

*  İlk olarak, `b` değerlendirilir ve `bool` değerini `b` belirlenir:
   * Örtük bir dönüştürme, türü `b` için `bool` üretmek için bu örtülü dönüştürme işlemi daha sonra mevcut bir `bool` değeri.
   * Aksi takdirde, `operator true` türüne göre tanımlanan `b` oluşturmak için çağrılan bir `bool` değeri.
*  Varsa `bool` Yukarıdaki adımı tarafından üretilen değer `true`, ardından `x` değerlendirilir ve koşullu ifade türüne dönüştürülür ve bu koşullu ifadenin sonuç olur.
*  Aksi takdirde, `y` değerlendirilir ve koşullu ifade türüne dönüştürülür ve bu koşullu ifadenin sonuç olur.

## <a name="anonymous-function-expressions"></a>Anonim işlev ifadeleri

Bir ***anonim işlev*** bir "satır içi" yöntem tanımını temsil eden bir ifadedir. Anonim bir işlev bir değer veya tür içinde ve kendisinin yok, ancak uyumlu temsilci veya ifade ağacı türüne dönüştürülemez. Bir anonim işlev dönüştürme değerlendirmesi dönüştürme hedef türüne bağlıdır: Bir temsilci türü ise, anonim işlevini tanımlayan yöntemin başvuran bir temsilci değerine dönüştürme değerlendirir. Bir ifade ağacı türü ise, dönüştürme yöntemi bir nesne yapısını olarak yapısını temsil eden bir ifade ağacı için değerlendirir.

Geçmiş nedenleri vardır iki söz dizimsel türü anonim İşlevler, yani *lambda_expression*s ve *anonymous_method_expression*s. Neredeyse tüm amacıyla *lambda_expression*s daha kısa ve daha ifadesel *anonymous_method_expression*s, geriye dönük uyumluluk için dilde kalır.

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

`=>` İşleci atamayla aynı önceliğe sahip (`=`) ve sağa ilişkilendirilmiştir.

Anonim bir işlevle `async` değiştiricisi bir zaman uyumsuz işlev ve açıklanan kurallara [yineleyiciler](classes.md#iterators).

Biçiminde anonim bir işlevin parametreleri bir *lambda_expression* açıkça veya dolaylı olarak yazılabilir. Bir açıkça belirtilmiş bir parametre listesinde her parametresinin türünü açıkça belirtilir. Bir türü örtük olarak belirlenmiş parametre listesinde parametre türleri anonim işlev oluştuğu bağlamdan algılanır — özellikle zaman anonim işlev bir uyumlu temsilci türü veya türü sağlayan ifade ağaç türüne dönüştürülür parametre türleri ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)).

Türü örtük olarak belirlenmiş, tek bir parametre ile anonim işlevde parametre listesinden parantezler atlanabilir. Formun başka bir deyişle, anonim bir işlev
```csharp
( param ) => expr
```
olarak kısaltılır
```csharp
param => expr
```

Biçiminde anonim bir işlevin parametre listesine bir *anonymous_method_expression* isteğe bağlıdır. Verilen parametreler açıkça yazılması gerekir. Anonim işlevi herhangi bir parametre ile bir temsilciye dönüştürülebilir değilse değil içeren liste `out` parametreleri.

A *blok* anonim bir işlev gövdesini erişilebilir ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)) erişilemeyen bir ifadeye anonim işlev gerçekleşmediği sürece.

Anonim işlevler bazı örnekleri aşağıda izleyin:

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

Davranışını *lambda_expression*s ve *anonymous_method_expression*s aşağıdaki noktaları dışında aynı olan:

*  *anonymous_method_expression*s, herhangi bir değer parametreleri listesi türleri temsilci convertibility sonuçlanmıyor tamamen atlanacak parametre listesine izin verir.
*  *lambda_expression*s izin atlanmış ve ancak çıkarımı yapılan parametre türleri *anonymous_method_expression*s açıkça belirtilmediği parametre türleri gerektirir.
*  Gövdesi bir *lambda_expression* ise bir ifade ya da bir deyim bloğunu olabilir gövdesi bir *anonymous_method_expression* bir deyim bloğunu olması gerekir.
*  Yalnızca *lambda_expression*s sahip uyumlu bir ifade ağacı türlerine dönüştürme işlemi ([ifade ağacı türleri](types.md#expression-tree-types)).

### <a name="anonymous-function-signatures"></a>Anonim işlev imzası

İsteğe bağlı *anonymous_function_signature* anonim bir işlev adlarını ve isteğe bağlı olarak anonim işlev biçimsel parametre türlerini tanımlar. Anonim işlev parametrelerinin kapsamı *anonymous_function_body*. ([Kapsamları](basic-concepts.md#scopes)) (belirtilmişse) parametre listesi ile birlikte anonim yöntem-gövdesi bir bildirim alanı oluşturur ([bildirimleri](basic-concepts.md#declarations)). Bu nedenle bir yerel değişken, yerel sabit veya kapsamı içeren parametre adını eşleştirmek için anonim işlev bir parametre adı için bir derleme zamanı hatası olduğundan *anonymous_method_expression* veya *lambda_ ifade*.

Anonim bir işlevdir varsa bir *explicit_anonymous_function_signature*, uyumlu temsilci türleri ve ifade ağacı türleri kümesini aynı sırada değiştiriciler ve aynı parametre türlerine sahip olanlar kısıtlanmış kaldırılır. Yöntem grubu dönüştürmeler aksine ([yöntem grubu dönüştürmeler](conversions.md#method-group-conversions)), anonim işlevi parametre türleri karşıt varyansını desteklenmiyor. Anonim bir işlevdir yoksa bir *anonymous_function_signature*, uyumlu temsilci türleri ve ifade ağacı türleri kümesini sahip olanlar kısıtlanmış ise `out` parametreleri.

Unutmayın bir *anonymous_function_signature* öznitelikleri veya bir parametre dizisi içeremez. Bununla birlikte, bir *anonymous_function_signature* , parametre listesi içeren bir parametre dizisi bir temsilci türüyle uyumlu olmayabilir.

Derleme zamanında bile uyumlu yine de başarısız olabilir, ayrıca, bir ifade ağaç türüne dönüştürme unutmayın ([ifade ağacı türleri](types.md#expression-tree-types)).

### <a name="anonymous-function-bodies"></a>Anonim işlev gövdeleri

Gövde (*ifade* veya *blok*) anonim bir işlev olan aşağıdaki kurallarına tabidir:

*  Anonim işlev bir imza içeriyorsa, imzasında belirtilen parametreler gövdesinde kullanılabilir. Anonim işlev imzasına varsa, bir temsilci türü ya da ifade türü parametreleri olan dönüştürülebilir ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)), ancak parametreler gövdesinde erişilemez.
*  Dışında `ref` veya `out` en yakın kapsayan imzasında (varsa) belirtilen parametreler anonim işlev gövdesi erişmek için bir derleme zamanı hatası ise bir `ref` veya `out` parametresi.
*  Zaman türü `this` bir yapı türü gövdesi erişmek için bir derleme zamanı hata `this`. Bu erişim açık olup geçerlidir (olarak `this.x`) ya da örtük (olarak `x` burada `x` struct'ın bir örnek üyesi). Bu kural yalnızca erişimi engeller ve üye araması sonuçları struct üyesi olup olmadığını etkilemez.
*  Gövde dış değişkenlere erişebilir ([dış değişkenler](expressions.md#outer-variables)), anonim bir işlevdir. Dış bir değişkenin erişim sırasında etkin olan değişken örneğini başvuracağı *lambda_expression* veya *anonymous_method_expression* değerlendirilir ([değerlendirmesi Anonim işlev ifadeleri](expressions.md#evaluation-of-anonymous-function-expressions)).
*  Bir derleme zamanı hata içerecek şekilde gövdesi için bir `goto` deyimi, `break` deyimi, veya `continue` hedefi, gövdenin dışında veya kapsanan anonim bir işlevin gövdesi içinde olan ifade.
*  A `return` gövdesinde deyimi en yakın kapsayan bir çağrısından denetim döndürür kapsayan işlev üyesi değil, anonim işlev. Belirtilen bir ifade bir `return` ifadesi örtük olarak temsilci türüne veya ifade ağacı türü olan dönüş türüne dönüştürülebilir olmalıdır en yakın kapsayan *lambda_expression* veya *anonymous_ method_expression* dönüştürülür ([anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions)).

Dışında bir anonim İşlev değerlendirmesi ve çağırmayı aracılığıyla bloğunu yürütülecek herhangi bir şekilde olup olmadığını açıkça belirtilmeyen *lambda_expression* veya *anonymous_method_expression*. Özellikle, derleyici bir synthesizing anonim bir işlevdir uygulamayı seçebilir veya daha fazla yöntemleri veya türleri adlandırılmış. Derleyici kullanılmak üzere ayrılmış bir formun tür Sentezlenen öğeleri adları olması gerekir.

### <a name="overload-resolution-and-anonymous-functions"></a>Aşırı yükleme çözünürlüğü ve anonim İşlevler

Anonim işlevler bir bağımsız değişken listesindeki içinde tür çıkarımı katılmak ve aşırı yükleme çözümlemesi. Lütfen [anlam çıkarma](expressions.md#type-inference) ve [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution) kesin kurallar için.

Aşağıdaki örnek, aşırı yükleme çözünürlüğü anonim işlevler etkisini gösterir.

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

`ItemList<T>` Sınıfına sahip iki `Sum` yöntemleri. Her alan bir `selector` değeri ayıklar bir liste öğesinden toplama üzerinden bağımsız değişken. Ayıklanan değerin şöyle bir `int` veya `double` ve altındaysa aynı şekilde ya da bir `int` veya `double`.

`Sum` Yöntemleri örneğin kullanılabilir ayrıntı satırları bir sırada bir listeden toplamları hesaplamak için.

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

İlk çağrısıysa, `orderDetails.Sum`hem `Sum` yöntemleri uygulanabilir olduğundan anonim işlev `d => d. UnitCount` ikisiyle uyumlu `Func<Detail,int>` ve `Func<Detail,double>`. Ancak, ilk aşırı yükleme çözünürlüğü seçer `Sum` yöntemi olduğundan dönüştürme `Func<Detail,int>` dönüştürme işlemini daha iyidir `Func<Detail,double>`.

İkinci çağrılmasını içinde `orderDetails.Sum`yalnızca ikinci `Sum` yöntemi uygulanabilir olduğundan anonim işlev `d => d.UnitPrice * d.UnitCount` türünde bir değer üreten `double`. Bu nedenle, aşırı yükleme çözümlemesi seçer ikinci `Sum` , çağırma yöntemi.

### <a name="anonymous-functions-and-dynamic-binding"></a>Anonim işlevler ve dinamik bağlama

Bir anonim işlev bir alıcı, bağımsız değişken veya dinamik olarak bağlı bir işlemin işlenen olamaz.

### <a name="outer-variables"></a>Dış değişkenleri

Herhangi bir yerel değişken, değer parametresi veya parametre dizisi kapsamı içerir *lambda_expression* veya *anonymous_method_expression* çağrılır bir ***dış değişken*** Anonim işlevi. Bir sınıfın bir örneği işlevi üyesinde `this` değeri bir değer parametresi olarak kabul edilir ve bir dış değişken işlev üyesi içinde yer alan anonim bir işlevdir.

#### <a name="captured-outer-variables"></a>Yakalanan dış değişkenler

Bir dış değişkenine bir anonim işlev tarafından başvurulduğunda dış değişken olması bildirilir ***yakalanan*** tarafından anonim bir işlevdir. Normalde, blok veya olduğu ilişkili deyimi yürütme için bir yerel değişken ömrü sınırlıdır ([yerel değişkenler](variables.md#local-variables)). Ancak, yakalanan bir dış değişken ömrü en az bir temsilci kadar genişletilmiş veya ifade ağacı anonim işlevden oluşturulan çöp toplama işlemi için uygun hale gelir.

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
yerel değişken `x` anonim işlev ve ömrünü tarafından yakalanan `x` döndürüldüğü temsilci kadar en az Genişletilmiş `F` (hangi sonuna kadar gerçekleşmez çöp toplama işlemi için uygun hale gelir program). Anonim işlev her çağrıldığında'nın aynı örneğinde çalışır olduğundan `x`, örnek çıktısı:
```
1
2
3
```

Yerel bir değişken veya değer parametresi bir anonim işlev tarafından yakalandığında, yerel değişken veya parametre artık sabit bir değişken olarak kabul edilir ([sabit ve taşınabilir değişkenleri](unsafe-code.md#fixed-and-moveable-variables)), ancak bunun yerine bir taşınabilir olarak kabul edilir değişken. Bu nedenle herhangi `unsafe` yakalanan bir dış değişkenin adresini alır kod ilk kullanmalıdır `fixed` değişkeni düzeltmek için deyimi.

Uncaptured bir değişkeni, yakalanan bir yerel değişken aynı anda birden çok iş parçacığı yürütme kullanıma sunulabilecek şekilde unutmayın.

#### <a name="instantiation-of-local-variables"></a>Yerel değişkenler örneklemesi

Yerel bir değişken olarak kabul edilir ***örneği*** yürütme, değişken kapsamını girdiğinde. Örneğin, aşağıdaki yöntem olduğunda çağrılır, yerel değişken `x` örneği ve üç kez başlatılan — döngünün her yinelemesinden için bir kez.

```csharp
static void F() {
    for (int i = 0; i < 3; i++) {
        int x = i * 2 + 1;
        ...
    }
}
```

Ancak, bildirimi taşıma `x` döngü sonuçları tek bir örneğinin dışında `x`:
```csharp
static void F() {
    int x;
    for (int i = 0; i < 3; i++) {
        x = i * 2 + 1;
        ...
    }
}
```

Yakalanan değil, yerel bir değişken tam olarak ne sıklıkta örneği gözlemek için bir yolu yoktur; örneklemeleri ömrü ayrık olduğundan, yalnızca aynı depolama konumu kullanmak üzere her örneklemesi için mümkündür. Ancak, bir anonim işlev bir yerel değişken yakaladığında örneklemesine etkisini belirgin hale gelir.

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
```
1
3
5
```

Ancak, bildirimi `x` döngü dışına taşındı:
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
Çıktı.
```
5
5
5
```

Bir for döngüsü bir yineleme değişkeni bildirirse, bu değişkeni döngü dışında bildirilmesi kabul edilir. Bu nedenle, yineleme değişkeni yakalamak için örnek değiştirilirse:

```csharp
static D[] F() {
    D[] result = new D[3];
    for (int i = 0; i < 3; i++) {
        result[i] = () => { Console.WriteLine(i); };
    }
    return result;
}
```
yineleme değişkeni yalnızca bir örneğini yakalanan, hangi çıktıyı üretir:
```
3
3
3
```

Bazı yakalanan değişkenlere paylaşmak henüz başkalarının ayrı Örneğiniz için anonim işlev temsilciler için mümkündür. Örneğin, varsa `F` değiştirildi
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
' ın aynı örneğine üç temsilcileri yakalama `x` ancak örnekleri'ni ayrı `y`, çıktı şu şekildedir:
```
1 1
2 1
3 1
```

Ayrı anonim işlevler bir harici değişken'ın aynı örneğine yakalayabilirsiniz. Örnekte:
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
yerel değişken'ın aynı örneğine iki anonim işlevler yakalama `x`, ve bu nedenle "değişken üzerinden kurabilmek". Örneğin çıktısı şöyledir:
```
5
10
```

### <a name="evaluation-of-anonymous-function-expressions"></a>Anonim işlev ifadeleri değerlendirme

Anonim bir işlevdir `F` her zaman bir temsilci türüne dönüştürülmesi gerekir `D` veya bir ifade ağacı türü `E`, doğrudan ya da temsilci oluşturma ifadesine yürütülmesini aracılığıyla `new D(F)`. Bu dönüştürme sonucu anonim işlev açıklandığı belirler [anonim işlev dönüştürmeler](conversions.md#anonymous-function-conversions).

## <a name="query-expressions"></a>Sorgu ifadeleri

***Sorgu ifadelerinde*** SQL ve XQuery gibi ilişkisel ve hiyerarşik sorgu dillerini benzer sorgular için tümleşik dil söz dizimi sağlar.

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

Sorgu ifadesi ile başlayan bir `from` yan tümcesi ve biter ya da bir `select` veya `group` yan tümcesi. İlk `from` yan tümcesi ve ardından sıfır veya daha fazla `from`, `let`, `where`, `join` veya `orderby` yan tümceleri. Her `from` yan tümcesi ise bir oluşturucuyu Giriş bir ***aralık değişkeni*** öğelerinin aralıkları bir ***dizisi***. Her `let` yan tümce ortaya çıkarır, önceki aralık değişkenleri yoluyla hesaplanan bir değeri temsil eden bir aralık değişkeni. Her `where` öğeleri sonuçtan dışlar bir filtre yan tümcesi ise. Her `join` yan tümcesi, belirtilen kaynak sırası anahtarlarla eşleşen çiftleri sağlayan başka bir dizinin anahtarları karşılaştırır. Her `orderby` yan tümcesi belirtilen ölçütlere göre öğeleri yeniden sıralar. En son `select` veya `group` yan tümcesinin aralık değişkenleri açısından sonucun şeklini belirtir. Son olarak, bir `into` "sorguları bir sonraki sorguda bir oluşturucu olarak bir sorgunun sonuçlarını düşünerek splice için" yan tümcesi kullanılabilir.

### <a name="ambiguities-in-query-expressions"></a>Sorgu ifadelerinde belirsizlikleri

Sorgu ifadeleri "bağlamsal anahtar sözcükler", yani, belirli bir bağlamda özel bir anlamı yoktur tanımlayıcıları sayısını içerir. Bunlar özellikle `from`, `where`, `join`, `on`, `equals`, `into`, `let`, `orderby`, `ascending`, `descending`, `select`, `group` ve `by`. Sorgu ifadeleri anahtar sözcükler veya basit adları bu tanımlayıcıları karışık kullanımına neden belirsizliklerden kaçınmak için bu tanımlayıcıları anahtar sözcükleri herhangi bir yerde bir sorgu ifadesi içinde gerçekleşen zaman olarak kabul edilir.

Bu amaç için bir sorgu ifadesi ile başlayan herhangi bir ifade olan "`from identifier`"dışında herhangi bir belirteci tarafından izlenen"`;`","`=`"veya"`,`".

Sorgu ifadesi içinde tanımlayıcı olarak bu sözcükleri kullanmak için bunlar ile belirtilebilir "`@`" ([tanımlayıcıları](lexical-structure.md#identifiers)).

### <a name="query-expression-translation"></a>Sorgu ifade çevirisi

C# dili sorgu ifadeleri yürütme semantikleri belirtmiyor. Bunun yerine, sorgu ifadeleri izliyor yöntemlerine çağrılarını çevrilir *sorgu ifade deseninin* ([sorgu ifade deseninin](expressions.md#the-query-expression-pattern)). Sorgu ifadeleri adlı yöntem çağrılarını özellikle çevrilir `Where`, `Select`, `SelectMany`, `Join`, `GroupJoin`, `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `GroupBy`, ve `Cast`. Bu yöntemler belirli imzalar ve sonuç türleri açıklandığı beklenir [sorgu ifade deseninin](expressions.md#the-query-expression-pattern). Bu yöntemler, sorgulanan nesne örnek yöntemlerine veya nesnenin dış genişletme yöntemleri olabilir ve sorgunun gerçek yürütmesi uyguladıkları.

Sorgu ifadeleri gelen çeviri yöntem çağrılarına herhangi bir türü bağlamayı önce gerçekleşen bir söz dizimi eşlemesi ya da aşırı yükleme çözünürlüğü gerçekleştirildi. Çeviri sözdizimsel olarak doğru olmasını garanti edilmez, ancak anlamsal olarak doğru C# kod üretmek için garanti edilmez. Çeviri sorgu ifadeleri, aşağıdaki oluşturulan yöntem çağrıları normal yöntem çağrıları işlenir ve bu sırayla hataları, örneğin yanlış tür bağımsız değişkenleri varsa yöntemi, mevcut değilse ya da genel yöntemleri açığa çıkarmak ve tür çıkarımı başarısız olur.

Sorgu ifadesi, başka hiçbir indirimleri mümkün olana kadar sürekli olarak aşağıdaki çevirileri uygulayarak işlenir. Çevirileri uygulama sırasına göre listelenir: her bölüm önceki bölümlerde yer çevirileri ayrıntısına gerçekleştirilmiş ve tükenmiş sonra bir bölüm daha sonra aynı sorgu ifadesinde işleme tekrar ziyaret değil varsayar.

Sorgu ifadelerinde aralık değişkenleri atamaya izin verilmiyor. Ancak bir C# uygulaması Bu bazen Burada sunulan söz dizimsel çeviri şemasıyla mümkün olmayabilir olduğundan bu kısıtlama, her zaman zorlamak için izin verilir.

Belirli çevirileri çağrılarınızla saydam tanımlayıcılarına sahip aralık değişkenleri ekleme `*`. Saydam tanımlayıcı özel özelliklerini ele alınmıştır daha ayrıntılı olarak [saydam tanımlayıcıları](expressions.md#transparent-identifiers).

#### <a name="select-and-groupby-clauses-with-continuations"></a>Devamlılıklarla seçin ve groupby yan tümceleri

Devamlılık sahip bir sorgu ifadesi
```csharp
from ... into x ...
```
erişimcisine
```csharp
from x in ( from ... ) ...
```

Aşağıdaki bölümlerde çevirileri sorguları Hayır sahip olduğunuzu varsaymaktadır `into` devamlılıkları.

Örnek
```csharp
from c in customers
group c by c.Country into g
select new { Country = g.Key, CustCount = g.Count() }
```
erişimcisine
```csharp
from g in
    from c in customers
    group c by c.Country
select new { Country = g.Key, CustCount = g.Count() }
```
hangi son çevrilmesidir
```csharp
customers.
GroupBy(c => c.Country).
Select(g => new { Country = g.Key, CustCount = g.Count() })
```

#### <a name="explicit-range-variable-types"></a>Açık aralığı değişken türleri

A `from` aralığı değişken türünü açıkça belirten yan tümcesi
```csharp
from T x in e
```
erişimcisine
```csharp
from x in ( e ) . Cast < T > ( )
```

A `join` aralığı değişken türünü açıkça belirten yan tümcesi
```
join T x in e on k1 equals k2
```
erişimcisine
```
join x in ( e ) . Cast < T > ( ) on k1 equals k2
```

Aşağıdaki bölümlerde çevirileri sorguları hiçbir açık aralığı değişken türleri olduğunu varsayın.

Örnek
```csharp
from Customer c in customers
where c.City == "London"
select c
```
erişimcisine
```csharp
from c in customers.Cast<Customer>()
where c.City == "London"
select c
```
hangi son çevrilmesidir
```csharp
customers.
Cast<Customer>().
Where(c => c.City == "London")
```

Açık aralığı değişken türleri: genel olmayan uygulama koleksiyonları sorgulamak için kullanışlı `IEnumerable` arabirimi, ancak genel `IEnumerable<T>` arabirimi. Yukarıdaki örnekte, bu durum geçerlidir olacaktır `customers` türü olan `ArrayList`.

#### <a name="degenerate-query-expressions"></a>Bozuk sorgu ifadeleri

Formun bir sorgu ifadesi
```csharp
from x in e select x
```
erişimcisine
```csharp
( e ) . Select ( x => x )
```

Örnek
```csharp
from c in customers
select c
```
erişimcisine
```csharp
customers.Select(c => c)
```

Bozuk bir sorgu ifadesinin artık önemsiz olarak kaynak öğelerini seçer biridir. Bir sonraki çeviri aşaması, kaynak ile değiştirerek diğer çeviri adımları tarafından sunulan bozuk sorguları kaldırır. Bir sorgunun sonucu emin olmak için ifade hiçbir zaman kaynak nesnenin kendisi, ancak, kaynak kimliğini ve türünü sorgu istemciye açığa gibi önemlidir. Bu nedenle bu adımı açıkça çağırarak doğrudan kaynak kodda yazılmış bozuk sorguları korur `Select` kaynak. Ardından uygulayıcılar kadar olan `Select` ve bu yöntemlerden hiç kaynak nesne döndürdüğünden emin olmak için diğer sorgu işleçleri.

#### <a name="from-let-where-join-and-orderby-clauses"></a>From, where, birleştirme ve orderby yan tümceleri sağlar

İkinci sorgu ifadesiyle `from` yan tümcesi tarafından izlenen bir `select` yan tümcesi
```csharp
from x1 in e1
from x2 in e2
select v
```
erişimcisine
```csharp
( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => v )
```

İkinci sorgu ifadesiyle `from` yan tümcesi izleyen bir şey tarafından dışındaki bir `select` yan tümcesi:

```csharp
from x1 in e1
from x2 in e2
...
```
erişimcisine
```csharp
from * in ( e1 ) . SelectMany( x1 => e2 , ( x1 , x2 ) => new { x1 , x2 } )
...
```

Bir sorgu ifadesini içeren bir `let` yan tümcesi
```csharp
from x in e
let y = f
...
```
erişimcisine
```csharp
from * in ( e ) . Select ( x => new { x , y = f } )
...
```

Bir sorgu ifadesini içeren bir `where` yan tümcesi
```csharp
from x in e
where f
...
```
erişimcisine
```csharp
from x in ( e ) . Where ( x => f )
...
```

Bir sorgu ifadesini içeren bir `join` yan tümcesi olmadan bir `into` arkasından bir `select` yan tümcesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
select v
```
erişimcisine
```csharp
( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => v )
```

Bir sorgu ifadesini içeren bir `join` yan tümcesi olmadan bir `into` dışında bir şey tarafından izlenen bir `select` yan tümcesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2
...
```
erişimcisine
```csharp
from * in ( e1 ) . Join( e2 , x1 => k1 , x2 => k2 , ( x1 , x2 ) => new { x1 , x2 })
...
```

Bir sorgu ifadesi ile bir `join` yan tümcesiyle birlikte bir `into` arkasından bir `select` yan tümcesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
select v
```
erişimcisine
```csharp
( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => v )
```

Bir sorgu ifadesini içeren bir `join` yan tümcesiyle birlikte bir `into` dışında bir şey tarafından izlenen bir `select` yan tümcesi
```csharp
from x1 in e1
join x2 in e2 on k1 equals k2 into g
...
```
erişimcisine
```csharp
from * in ( e1 ) . GroupJoin( e2 , x1 => k1 , x2 => k2 , ( x1 , g ) => new { x1 , g })
...
```

Bir sorgu ifadesini içeren bir `orderby` yan tümcesi
```csharp
from x in e
orderby k1 , k2 , ..., kn
...
```
erişimcisine
```csharp
from x in ( e ) . 
OrderBy ( x => k1 ) . 
ThenBy ( x => k2 ) .
... .
ThenBy ( x => kn )
...
```

Sıralama yan tümcesi belirtir bir `descending` yönü gösterge, çağrısından `OrderByDescending` veya `ThenByDescending` yerine üretilir.

Aşağıdaki çevirileri olduğunu varsayın hiçbir `let`, `where`, `join` veya `orderby` yan tümceleri ve en fazla bir ilk `from` yan tümcesinde her sorgu ifadesi.

Örnek
```csharp
from c in customers
from o in c.Orders
select new { c.Name, o.OrderID, o.Total }
```
erişimcisine
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
erişimcisine
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.OrderID, o.Total }
```
hangi son çevrilmesidir
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.OrderID, x.o.Total })
```
Burada `x` yoksa görünmez ve erişilemez bir derleyicinin ürettiği tanımlayıcıdır.

Örnek
```csharp
from o in orders
let t = o.Details.Sum(d => d.UnitPrice * d.Quantity)
where t >= 1000
select new { o.OrderID, Total = t }
```
erişimcisine
```csharp
from * in orders.
    Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) })
where t >= 1000 
select new { o.OrderID, Total = t }
```
hangi son çevrilmesidir
```csharp
orders.
Select(o => new { o, t = o.Details.Sum(d => d.UnitPrice * d.Quantity) }).
Where(x => x.t >= 1000).
Select(x => new { x.o.OrderID, Total = x.t })
```
Burada `x` yoksa görünmez ve erişilemez bir derleyicinin ürettiği tanımlayıcıdır.

Örnek
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
select new { c.Name, o.OrderDate, o.Total }
```
erişimcisine
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
erişimcisine
```csharp
from * in customers.
    GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
        (c, co) => new { c, co })
let n = co.Count()
where n >= 10 
select new { c.Name, OrderCount = n }
```
hangi son çevrilmesidir
```csharp
customers.
GroupJoin(orders, c => c.CustomerID, o => o.CustomerID,
    (c, co) => new { c, co }).
Select(x => new { x, n = x.co.Count() }).
Where(y => y.n >= 10).
Select(y => new { y.x.c.Name, OrderCount = y.n)
```
Burada `x` ve `y` aksi görünmez ve erişilemez derleyicinin ürettiği tanıtıcılardır.

Örnek
```csharp
from o in orders
orderby o.Customer.Name, o.Total descending
select o
```
Son çeviri içeriyor
```csharp
orders.
OrderBy(o => o.Customer.Name).
ThenByDescending(o => o.Total)
```

#### <a name="select-clauses"></a>SELECT yan tümceleri

Formun bir sorgu ifadesi
```csharp
from x in e select v
```
erişimcisine
```csharp
( e ) . Select ( x => v )
```
v tanımlayıcı olduğunda dışında x, çeviri, yalnızca
```csharp
( e )
```

Örneğin:
```csharp
from c in customers.Where(c => c.City == "London")
select c
```
yalnızca veri dönüştürülür
```csharp
customers.Where(c => c.City == "London")
```

#### <a name="groupby-clauses"></a>Groupby yan tümceleri

Formun bir sorgu ifadesi
```csharp
from x in e group v by k
```
erişimcisine
```csharp
( e ) . GroupBy ( x => k , x => v )
```
v tanımlayıcı olduğunda dışında x, çeviri,
```csharp
( e ) . GroupBy ( x => k )
```

Örnek
```csharp
from c in customers
group c.Name by c.Country
```
erişimcisine
```csharp
customers.
GroupBy(c => c.Country, c => c.Name)
```

#### <a name="transparent-identifiers"></a>Saydam tanımlayıcı

Aralık değişkenleri ile belirli çevirileri ekleme ***saydam tanımlayıcıları*** çağrılarınızla `*`. Saydam tanımlayıcı uygun dil özelliği değildir; yalnızca sorgu ifade çevirisi sürecinde ara adım olarak mevcut.

Daha fazla sorgu çevirisi saydam tanımlayıcı yerleştirir, çeviri adımları saydam tanımlayıcı anonim işlevler ve anonim nesne başlatıcıları yay. Bu bağlamda, aşağıdaki davranış saydam tanımlayıcıları vardır:

*  Saydam tanımlayıcı bir anonim işlev bir parametre olarak ortaya çıktığında, ilişkili anonim tür otomatik olarak anonim işlev gövdesinde kapsamda üyeleridir.
*  Saydam tanımlayıcı üyesi kapsamında olduğunda, bu üyenin de kapsamda üyeleridir.
*  Saydam tanımlayıcı olarak bir üye bildirimcisi anonim nesne başlatıcı içinde ortaya çıktığında, bir saydam tanımlayıcı üyesi tanıtır.
*  Yukarıda açıklanan çeviri adımlarda saydam tanımlayıcılar her zaman tek bir nesnenin bir üyesi olarak birden çok aralık değişkenleri yakalama amacıyla anonim türleri ile birlikte sunulmuştur. C# uygulaması birden çok aralık değişkenleri gruplamak için anonim türler değerinden farklı bir mekanizma kullanmak için izin verilir. Anonim türleri kullanılır ve saydam tanımlayıcıları Göster aşağıdaki çeviri örnekler varsayılır hemen çevrilebilir.

Örnek
```csharp
from c in customers
from o in c.Orders
orderby o.Total descending
select new { c.Name, o.Total }
```
erişimcisine
```csharp
from * in customers.
    SelectMany(c => c.Orders, (c,o) => new { c, o })
orderby o.Total descending
select new { c.Name, o.Total }
```

Daha fazla olduğu çevrilir
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(* => o.Total).
Select(* => new { c.Name, o.Total })
```
Saydam tanımlayıcı silinmesi, hangi eşdeğerdir
```csharp
customers.
SelectMany(c => c.Orders, (c,o) => new { c, o }).
OrderByDescending(x => x.o.Total).
Select(x => new { x.c.Name, x.o.Total })
```
Burada `x` yoksa görünmez ve erişilemez bir derleyicinin ürettiği tanımlayıcıdır.

Örnek
```csharp
from c in customers
join o in orders on c.CustomerID equals o.CustomerID
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
erişimcisine
```csharp
from * in customers.
    Join(orders, c => c.CustomerID, o => o.CustomerID, 
        (c, o) => new { c, o })
join d in details on o.OrderID equals d.OrderID
join p in products on d.ProductID equals p.ProductID
select new { c.Name, o.OrderDate, p.ProductName }
```
Daha fazla olduğu için azaltıldı
```csharp
customers.
Join(orders, c => c.CustomerID, o => o.CustomerID, (c, o) => new { c, o }).
Join(details, * => o.OrderID, d => d.OrderID, (*, d) => new { *, d }).
Join(products, * => d.ProductID, p => p.ProductID, (*, p) => new { *, p }).
Select(* => new { c.Name, o.OrderDate, p.ProductName })
```
hangi son çevrilmesidir
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
Burada `x`, `y`, ve `z` aksi görünmez ve erişilemez derleyicinin ürettiği tanıtıcılardır.

### <a name="the-query-expression-pattern"></a>Sorgu ifade deseni

***Sorgu ifade deseninin*** türleri sorgu ifadeleri desteklemek için uygulayabileceğiniz yöntemler desenini oluşturur. Sorgu ifadeleri yöntem çağrıları için söz dizimi bir eşleme yoluyla çevrilir için sorgu ifade deseninin nasıl uyguladıkları önemli ölçüde esneklik türleri sahiptir. Örneğin, düzeni yöntemlerini örnek yöntemlerine veya genişletme yöntemleri olarak çağırma sözdiziminde iki sahip, ve anonim işlevler hem de dönüştürülebilir olduğundan yöntemleri temsilci veya ifade ağaçları isteyebilir uygulanabilir.

Önerilen genel bir tür şeklini `C<T>` desteklediği sorgu ifade deseninin aşağıda gösterilmiştir. Genel tür parametre ve sonuç türleri arasındaki ilişkileri doğru şekilde göstermek için kullanılır, ancak genel olmayan türler deseni uygulamak mümkündür.

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

Yukarıdaki yöntemleri Genel temsilci türleriyle kullanın `Func<T1,R>` ve `Func<T1,T2,R>`, ancak bunlar eşit derecede iyi diğer temsilci veya ifade ağacı türü aynı parametre ve sonuç türleri ilişkileri kullanmış.

Önerilen ilişki arasındaki fark `C<T>` ve `O<T>` da sağlar `ThenBy` ve `ThenByDescending` yöntemleri sonucu üzerinde yalnızca bir `OrderBy` veya `OrderByDescending`. Ayrıca sonucunu önerilen şeklini fark `GroupBy` --sıralarının, her iç sıra sahip olduğu ek `Key` özelliği.

`System.Linq` Uygulayan herhangi bir türü için ad alanı sağlar sorgu işleci deseninin bir uygulaması `System.Collections.Generic.IEnumerable<T>` arabirimi.

## <a name="assignment-operators"></a>Atama işleçleri

Atama İşleçleri, yeni bir değer bir değişken, bir özellik, bir olay veya dizin oluşturucu öğenin atayın.

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

Atamanın sol işleneni, bir değişken, bir özellik erişimi, bir dizin oluşturucu erişim veya bir olay erişimini sınıflandırılmış bir ifade olmalıdır.

`=` İşleci çağrıldığında ***basit atama işleci***. Sağ işleneninin değerini sol işlenen tarafından belirtilen değişken, özellik veya dizin oluşturucu öğenin atar. Basit atama işlecinin sol işleneni, bir olay erişimini olmayabilir (açıklanan haller dışında [alan benzeri olaylara](classes.md#field-like-events)). Basit atama işleci açıklanan [basit atama](expressions.md#simple-assignment).

Atama işleçleri dışında `=` işleci çağrılır ***bileşik atama işleçleri***. Bu işleçler, iki işlenen üzerinde belirtilen işlemi gerçekleştirmek ve ardından sonuç değerini sol işlenen tarafından belirtilen değişken, özellik veya dizin oluşturucu öğenin atayın. Bileşik atama işleçleri açıklanan [bileşik atama](expressions.md#compound-assignment).

`+=` Ve `-=` işleçleri ile bir olay erişim ifadesi sol işlenen olarak adlandırılır *olay atama işleçleri*. Diğer bir atama işleci sol işlenen olarak bir olay erişimini ile geçerlidir. Olay atama işleçleri açıklanan [olay ataması](expressions.md#event-assignment).

Atama İşleçleri sağla ilişkilendirilebilir, işlemler soldan sağa gruplandırılır anlamına gelir. Örneğin, bir ifade formun `a = b = c` değerlendirmesinde `a = (b = c)`.

### <a name="simple-assignment"></a>Basit atama

`=` İşleci basit atama işleci çağrılır.

Sol işleneni, bir basit atama biçiminde olup olmadığını `E.P` veya `E[Ei]` burada `E` derleme zamanı türü `dynamic`, atama dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı Atama ifadesinin türüdür `dynamic`, ve çalışma zamanında çalışma zamanı türüne göre aşağıda açıklanan çözümleme gerçekleşecek `E`.

Bir basit atama sağ işlenen, sol işlenen türüne örtük olarak dönüştürülebilir bir ifade olması gerekir. İşlemi sağ işleneninin değerini sol işlenen tarafından belirtilen değişken, özellik veya dizin oluşturucu öğenin atar.

Basit atama ifadesi sol işlenen için atanan değer sonucudur. Sonuç sol işlenenle aynı türde ve her zaman bir değer olarak sınıflandırılır.

Sol işlenen bir özellik veya dizin oluşturucu erişimi varsa özelliği veya dizin oluşturucu olmalıdır bir `set` erişimcisi. Durum bu değilse, bir bağlama zamanı hatası oluşur.

Bir basit atama formun çalışma zamanı işlenmesini `x = y` aşağıdaki adımlardan oluşur:

*  Varsa `x` bir değişken olarak sınıflandırıldığını:
   * `x` değişkeni oluşturmak için değerlendirilir.
   * `y` değerlendirilir ve, gerekirse türüne dönüştürülmüş `x` aracılığıyla örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).
   * Varsa tarafından verilen değişkeni `x` bir dizi öğesinin bir *reference_type*, için hesaplanan değer sağlamak için bir çalışma zamanı denetimi gerçekleştirilir `y` biri dizi örneği ile uyumlu `x` olduğu bir öğe. Varsa denetimi başarılı `y` olan `null`, ya da örtük bir başvuru dönüştürmesi, ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) gerçek tür tarafından başvurulan örnek var. `y` gerçek öğe türü dizi örneği içeren `x`. Aksi takdirde, bir `System.ArrayTypeMismatchException` oluşturulur.
   * Dönüşümü ve değerlendirme elde edilen değer `y` değerlendirmesi tarafından verilen konuma depolanır `x`.
*  Varsa `x` bir özellik veya dizin oluşturucu erişim sınıflandırıldığını:
   * Örnek ifade (varsa `x` değil `static`) ve bağımsız değişken listesi (varsa `x` bir dizin oluşturucu erişim) ile ilişkili `x` değerlendirilir, ve sonuçları sonraki kullanılan `set` erişimci çağırma.
   * `y` değerlendirilir ve, gerekirse türüne dönüştürülmüş `x` aracılığıyla örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).
   * `set` Erişimcisine `x` için hesaplanan değer ile çağrılan `y` olarak kendi `value` bağımsız değişken.

Dizi değişimini kuralları ([dizi kovaryansı](arrays.md#array-covariance)) bir dizi türünde bir değer izin `A[]` bir dizi türünde bir örneğe bir başvuru olarak `B[]`, gelen bir örtük bir başvuru dönüştürmesi var sağlanan `B` için `A`. Bu kurallar, bir dizi öğesine atama nedeniyle bir *reference_type* atanan değerin dizi örneği ile uyumlu olduğundan emin olmak için bir çalışma zamanı denetimi gerektirir. Örnekte
```csharp
string[] sa = new string[10];
object[] oa = sa;

oa[0] = null;               // Ok
oa[1] = "Hello";            // Ok
oa[2] = new ArrayList();    // ArrayTypeMismatchException
```
Son ataması neden olan bir `System.ArrayTypeMismatchException` çünkü durum örneğini `ArrayList` bir öğesinde depolanan bir `string[]`.

Bir özellik veya dizin oluşturucu ne zaman bildirilen bir *struct_type* örnek ifade atama işleminin hedefi, özellikle ilişkili veya dizin oluşturucu erişim bir değişken olarak sınıflandırılan gerekir. Örnek ifade bir değer olarak sınıflandırılmış bir bağlama zamanı hatası oluşur. Nedeniyle [üye erişimi](expressions.md#member-access), aynı kural alanları için de geçerlidir.

Bildirimleri verildiğinde:
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
atamaları `p.X`, `p.Y`, `r.A`, ve `r.B` için izin verilen `p` ve `r` değişkenlerdir. Ancak, örnekte
```csharp
Rectangle r = new Rectangle();
r.A.X = 10;
r.A.Y = 10;
r.B.X = 100;
r.B.Y = 100;
```
atamaları itibaren tüm geçersiz `r.A` ve `r.B` değişkenleri değildir.

### <a name="compound-assignment"></a>Bileşik atama

Bileşik atama sol işleneni biçiminde olup olmadığını `E.P` veya `E[Ei]` burada `E` derleme zamanı türü `dynamic`, atama dinamik olarak bağlı sonra ([dinamik bağlama](expressions.md#dynamic-binding)). Bu durumda derleme zamanı Atama ifadesinin türüdür `dynamic`, ve çalışma zamanında çalışma zamanı türüne göre aşağıda açıklanan çözümleme gerçekleşecek `E`.

Formun bir işlem `x op= y` ikili işleci aşırı yükleme çözünürlüğü uygulama tarafından işlenen ([ikili işleci aşırı yükleme çözünürlüğü](expressions.md#binary-operator-overload-resolution)) işlemi olarak yazılmışsa `x op y`. Then,

*  Seçili işlecinin dönüş türü türüne örtük olarak dönüştürülebilir ise `x`, işlemi olarak değerlendirilir `x = x op y`dışında `x` yalnızca bir kez değerlendirilir.
*  Aksi takdirde seçilen işlecinin dönüş türü türüne açıkça dönüştürülebilir ise seçilen operatöre bir önceden tanımlanmış işleç ise `x`ve eğer `y` türüne açıkça dönüştürülemez `x` veya işlecin bir kaydırma işleci, sonra da işlemi olarak değerlendirilir `x = (T)(x op y)`burada `T` türü `x`dışında `x` yalnızca bir kez değerlendirilir.
*  Aksi takdirde, bileşik atama geçersiz ve bir bağlama zamanı hatası oluşur.

"Yalnızca bir kez değerlendirilir" terimi içinde değerlendirmesi anlamına `x op y`, bağlı tüm ifadelerin sonuçlarını `x` geçici olarak kaydedilir ve ardından atamaya gerçekleştirirken yeniden `x`. Örneğin, atama, `A()[B()] += C()`burada `A` döndüren bir yöntem `int[]`, ve `B` ve `C` döndüren yöntemi `int`, yöntemleri, yalnızca bir kez sırayla çağrılır `A`, `B`, `C`.

Bileşik atama sol işleneni bir özellik erişimi ya da dizin oluşturucu erişimi olduğunda özellik veya dizin oluşturucu her ikisi de olmalıdır bir `get` erişimci ve `set` erişimcisi. Durum bu değilse, bir bağlama zamanı hatası oluşur.

İkinci kuralı izinleri yukarıda `x op= y` olarak değerlendirilecek `x = (T)(x op y)` belirli bağlamlarda. Sol işlenen türü olduğunda, önceden tanımlı operatörler bileşik işleçleri kullanılabilir olacağı şekilde kuralı mevcut `sbyte`, `byte`, `short`, `ushort`, veya `char`. Ne zaman her iki değişken bu türlerin birini olsa bile, önceden tanımlı operatörler türünün bir sonucu üretmek `int`anlatılan şekilde [ikili sayısal promosyonlar](expressions.md#binary-numeric-promotions). Bu nedenle, bir yayın olmadan sol işleneni sonucu atamak mümkün olmayacaktır.

Kural önceden tanımlı operatörler için sezgisel etkisini yalnızca olan `x op= y` hem de izin verilir, `x op y` ve `x = y` izin verilir. Örnekte
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
Her hata için sezgisel nedeni, karşılık gelen bir basit atama bir hata de olabilirdi olmasıdır.

Bu, ayrıca işlemlerini destekleyen bileşik atama işlemlerinin yükseltilmiş anlamına gelir. Örnekte
```csharp
int? i = 0;
i += 1;             // Ok
```
lifted işleci `+(int?,int?)` kullanılır.

### <a name="event-assignment"></a>Olay atama

Sol işleneni, bir `+=` veya `-=` işleci bir olay erişimini sınıflandırılır ve ardından ifade gibi değerlendirilir:

*  Örnek ifade varsa olay erişimini değerlendirilir.
*  Sağ işleneninin `+=` veya `-=` işleci değerlendirilir ve, gerekirse, sol işlenen aracılığıyla örtük bir dönüştürme türü dönüştürülür ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).
*  Bir olayın olay erişimcisi sağ işleneni değerlendirmesinden sonra oluşan bağımsız değişken listesiyle çağrılır ve gerekirse, dönüştürme. İşleç olduysa `+=`, `add` erişimci çağrılır; işleci ise `-=`, `remove` erişimci çağrılır.

Bir olay atama ifadesi bir değer vermez. Bu nedenle, bir olay atama ifadesi yalnızca bağlamında geçerli bir *statement_expression* ([ifade deyimleri](statements.md#expression-statements)).

## <a name="expression"></a>İfade

Bir *ifade* ya da bir *non_assignment_expression* veya *atama*.

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

## <a name="constant-expressions"></a>Sabit ifadeleri

A *constant_expression* , derleme zamanında tam olarak değerlendirilebilecek bir ifadedir.

```antlr
constant_expression
    : expression
    ;
```

Sabit ifade olmalıdır `null` değişmez değeri ya da bir değer şu türlerden biriyle: `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char` , `float`, `double`, `decimal`, `bool`, `object`, `string`, ya da herhangi bir numaralandırma türü. Yalnızca aşağıdaki yapılar, sabit ifadelerde izin verilir:

*  Değişmez değerler (dahil olmak üzere `null` değişmez değer).
*  Başvurular `const` üyeleri sınıf ile yapı türü.
*  Numaralandırma türleri üyeleri için başvurular.
*  Başvurular `const` parametrelerin veya yerel değişkenler
*  Kendileri sabit ifadeler parantezli alt ifadeleri.
*  Cast ifadeleri, sağlanan hedef türü, yukarıda listelenen türler biridir.
*  `checked` ve `unchecked` ifadeleri
*  Varsayılan değer ifadeleri
*  Nameof ifadeleri
*  Önceden tanımlanmış `+`, `-`, `!`, ve `~` birli işleçler.
*  Önceden tanımlanmış `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `|`, `^`, `&&`, `||`, `==`, `!=`, `<`, `>`, `<=`, ve `>=` ikili işleçler, sağlanan her işlenen yukarıda listelenen bir tür.
*  `?:` Koşullu işleç.

Aşağıdaki dönüşümlerden sabit ifadelerde izin verilir:

*  Kimlik dönüştürme
*  Sayısal dönüşümler
*  Sabit listesi dönüştürmeler
*  Sabit ifade dönüştürmeler
*  Örtük ve açık başvuru dönüşümleri, dönüştürmeler kaynağı null değer veren bir sabit ifade sağlanır.

Kutulama dahil olmak üzere diğer dönüştürme, kutudan çıkarma ve dolaylı başvuru dönüşümleri null olmayan değerlerin sabit ifadelerde izin verilmez. Örneğin:
```csharp
class C {
    const object i = 5;         // error: boxing conversion not permitted
    const object str = "hello"; // error: implicit reference conversion
}
```
başlatma i olduğundan bir hata paketleme dönüştürmesi gereklidir. Bir null olmayan değer dosyasından bir örtük bir başvuru dönüştürmesi gerektiğinden str başlatma bir hatadır.

Bir ifade yukarıda listelenen gereksinimleri karşılayan her ifade derleme zamanında değerlendirilir. İfade sabit olmayan yapıları içeren daha büyük bir ifadenin bir alt ifade olsa bile bu geçerlidir.

Sabit ifadeler derleme zamanı değerlendirmesi bir derleme zamanı hatası oluşmasına neden olduğu çalışma zamanı değerlendirme atılmış bir özel durum, derleme zamanı değerlendirmesi dışında çalışma zamanı değerlendirme sabit olmayan ifade aynı kurallara kullanır.

Sabit bir ifade açıkça yerleştirilir sürece bir `unchecked` bağlam, Tamsayı türünde aritmetik işlemler ve dönüştürmeler ifade her zaman derleme zamanı değerlendirmesi sırasında gerçekleşen taşıyor derleme zamanı hatalarına neden olabilir ([Sabit ifadeler](expressions.md#constant-expressions)).

Sabit ifadeler, aşağıda listelenen bağlamlarda oluşur. Bir ifadenin derleme zamanında tam olarak değerlendirilemezse şu bağlamlarda Bu, bir derleme zamanı hatası oluşur.

*  Sabit bildirimi ([sabitleri](classes.md#constants)).
*  Sabit listesi üye bildirimleri ([numaralandırma üyeleri](enums.md#enum-members)).
*  Varsayılan bağımsız değişkenler biçimsel parametre listesi ([yöntem parametreleri](classes.md#method-parameters))
*  `case` Etiketler, bir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)).
*  `goto case` deyimler ([goto deyimi](statements.md#the-goto-statement)).
*  Bir dizi oluşturma ifadesi içinde uzunluklarını boyut ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)), bir başlatıcı içerir.
*  Öznitelikler ([öznitelikleri](attributes.md)).

Örtük sabit ifade dönüştürme ([örtük sabit ifade dönüştürmeler](conversions.md#implicit-constant-expression-conversions)) türünde sabit bir ifade verir `int` dönüştürülecek `sbyte`, `byte`, `short`, `ushort`, `uint`, veya `ulong`, sağlanan sabit ifadenin değeri hedef türün aralığı içinde.

## <a name="boolean-expressions"></a>Boole ifadeleri

A *boolean_expression* türünün bir sonucu veren ifade `bool`; ya da doğrudan veya uygulaması aracılığıyla `operator true` aşağıda belirtildiği gibi belirli bağlamlarda.

```antlr
boolean_expression
    : expression
    ;
```

Denetleyen koşullu ifadede bir *if_statement* ([if deyimi](statements.md#the-if-statement)), *while_statement* ([while ifadesi](statements.md#the-while-statement)), *do_statement* ([do deyimi](statements.md#the-do-statement)), veya *for_statement* ([deyimi için](statements.md#the-for-statement)) olan bir *boolean_ ifade*. Denetleyen koşullu ifadede `?:` işleci ([koşullu işleç](expressions.md#conditional-operator)) olarak aynı kurallara bir *boolean_expression*, ancak işleci nedenlerle öncelik sınıflandırılır olarak bir *conditional_or_expression*.

A *boolean_expression* `E` türünde bir değer üretmek için gerekli `bool`gibi:

*  Varsa `E` örtük olarak dönüştürülebilir `bool` çalışma zamanında bu örtülü bir dönüştürme uygulanır.
*  Aksi takdirde, birli işleç aşırı çözümleme ([birli işleç aşırı yükleme çözünürlüğü](expressions.md#unary-operator-overload-resolution)) işlecinin benzersiz bir en iyi uygulama bulmak için kullanılan `true` üzerinde `E`, ve bu uygulamayı çalışma zamanında uygulanır.
*  Böyle bir işleç bulunursa, bir bağlama zamanı hatası oluşur.

`DBBool` Yapı türünde [veritabanı Boole türü](structs.md#database-boolean-type) uygulayan bir tür bir örnek sağlar `operator true` ve `operator false`.
