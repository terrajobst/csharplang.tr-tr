---
ms.openlocfilehash: fad1425e2871f395a2eb1f39faccbc773d88d6a3
ms.sourcegitcommit: da1180f7eacdd5067b32d291a76e6764159e00fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "79485065"
---
# <a name="recursive-pattern-matching"></a>Özyinelemeli desenli eşleştirme

## <a name="summary"></a>Özet
[summary]: #summary

İçin C# model eşleştirme uzantıları, işlev dillerinden çok sayıda avantajı ve işlevsel dillerin örüntüsünün eşleşmesini sağlayan, ancak temel alınan dilin hisleriyle sorunsuz bir şekilde tümleşen birçok avantaj sağlayan bir şekilde. Bu yaklaşımın öğeleri, programlama dillerinde [F#](http://www.msr-waypoint.net/pubs/79947/p29-syme.pdf "Hafif bir dil aracılığıyla Genişletilebilir kalıp eşleme") ve [Scala](https://link.springer.com/content/pdf/10.1007%2F978-3-540-73589-2.pdf "Desenler, sayfa 273 Ile eşleşen nesneler")'daki ilgili özelliklerle ilham.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

### <a name="is-expression"></a>Ifade Ifadesi

`is` işleci, bir ifadeyi bir *düzene*karşı test etmek için genişletilir.

```antlr
relational_expression
    : is_pattern_expression
    ;
is_pattern_expression
    : relational_expression 'is' pattern
    ;
```

Bu *relational_expression* biçimi, C# belirtideki mevcut formlara ek niteliğindedir. `is` belirtecinin solundaki *relational_expression* bir değer belirlememez veya bir tür yoksa bir derleme zamanı hatasıdır.

Düzenin her *tanımlayıcısı* , `is` işleci `true` sonra *kesinlikle atanan* yeni bir yerel değişken sunar (yani, *doğru olduğunda, kesin olarak atanır*).

> Note: `is-expression` ve *constant_pattern* *türü* arasında teknik bir belirsizlik vardır; bunlardan biri, tam bir tanımlayıcının geçerli bir ayrıştırmasından kaynaklanabilir. Bunu, dilin önceki sürümleriyle uyumluluk için bir tür olarak bağlamaya çalışırız; yalnızca bu başarısız olursa, diğer bağlamlarda bir ifade yaptığımız (bir sabit veya bir tür olması gerekir) bunu çözeceğiz. Bu belirsizlik yalnızca `is` ifadesinin sağ tarafında bulunur.

### <a name="patterns"></a>Desenler

Desenler *is_pattern* işlecinde, bir *switch_statement*ve bir *switch_expression* içinde, gelen verilerin (giriş değerini çağırdığımız) veri şeklinin karşılaştırılacağı bir şekilde ifade etmek için kullanılır. Desenler, verilerin bölümlerinin alt desenlerle eşleştirileceği şekilde özyinelemeli olabilir.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    | positional_pattern
    | property_pattern
    | discard_pattern
    ;
declaration_pattern
    : type simple_designation
    ;
constant_pattern
    : expression
    ;
var_pattern
    : 'var' designation
    ;
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
property_subpattern
    : '{' subpatterns? '}'
    ;
property_pattern
    : type? property_subpattern simple_designation?
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
discard_pattern
    : '_'
    ;
```

#### <a name="declaration-pattern"></a>Bildirim kalıbı

```antlr
declaration_pattern
    : type simple_designation
    ;
```

*Declaration_pattern* her ikisi de bir ifadenin verilen türden olduğunu sınar ve test başarılı olursa bu türe yayınlar. Bu, atama bir *single_variable_designation*ise verilen tanımlayıcı tarafından verilen türün yerel bir değişkenini ortaya çıkarabilir. Bu yerel değişken, bir kalıp eşleme işleminin sonucu `true`, *kesinlikle atanır* .

Bu ifadenin çalışma zamanı anlam düzeyi, sol taraftaki *relational_expression* işlenenin çalışma zamanı türünü, düzendeki *türe* göre sınar.  Çalışma zamanı türü (veya bazı alt tür) ise, `null``is operator` sonucu `true`.

Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır. `E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` 'den `T`'ye bir paket oluşturma dönüştürmesi ya da bu türlerden biri bir açık tür ise, bir tür `T` bir tür ile *uyumlu* olarak kabul edilir. `E` türünde bir giriş ile eşleştirildiği tür deseninin *türüyle* *uyumlu* değilse, derleme zamanı hatası olur.

Tür stili, başvuru türlerinin çalışma zamanı türü testlerini gerçekleştirmek için yararlıdır ve deyim yerini alır

```csharp
var v = expr as Type;
if (v != null) { // code using v
```

Biraz daha kısa

```csharp
if (expr is Type v) { // code using v
```

*Tür* null yapılabilir bir değer türünde ise bu bir hatadır.

Tür stili, null yapılabilir türlerin değerlerini test etmek için kullanılabilir: `Nullable<T>` (veya kutulanmış `T`) bir değer, bir tür düzeniyle eşleşir `T2 id` değer null değilse ve `T2` türü `T`veya bazı temel tür veya `T`arabirimi. Örneğin, kod parçasında

```csharp
int? x = 3;
if (x is int v) { // code using v
```

`if` deyimin koşulu çalışma zamanında `true` ve `v` değişkeni, blok içindeki `int` türündeki `3` değeri tutar. Blok `v`, değişken kapsam içinde ve kesin olarak atanmamıştır.

#### <a name="constant-pattern"></a>Sabit model

```antlr
constant_pattern
    : constant_expression
    ;
```

Sabit bir model, bir ifadenin değerini sabit bir değere göre sınar. Sabit, değişmez değer, belirtilen bir `const` değişkeninin adı veya bir numaralandırma sabiti gibi herhangi bir sabit ifade olabilir. Giriş değeri açık bir tür olmadığında, sabit ifade örtük olarak eşleşen ifadenin türüne dönüştürülür; giriş değerinin türü, sabit ifadenin türüyle *uyumlu* değilse, kalıp eşleme işlemi bir hatadır.

`object.Equals(c, e)`, *`true`* döndürecaksa, dönüştürülmüş giriş değeri *e* ile eşleşen olarak değerlendirilir.

Kullanıcı tanımlı bir `operator==`çağıracağından, yeni yazılmış koddaki `null` test etmenin en yaygın yolu olarak `e is null` görmeniz beklenir.

#### <a name="var-pattern"></a>Var deseninin

```antlr
var_pattern
    : 'var' designation
    ;
designation
    : simple_designation
    | tuple_designation
    ;
simple_designation
    : single_variable_designation
    | discard_designation
    ;
single_variable_designation
    : identifier
    ;
discard_designation
    : _
    ;
tuple_designation
    : '(' designations? ')'
    ;
designations
    : designation
    | designations ',' designation
    ;
```

Eğer *atama* bir *simple_designation*ise, *e* ifadesi bir ifade ile eşleşir. Diğer bir deyişle, bir *var düzeniyle* eşleşme *simple_designation*her zaman başarılı olur. *Simple_designation* bir *single_variable_designation*ise, *e* değeri yeni tanıtılan yerel değişkene göre sınırlar. Yerel değişkenin türü, *e*'nin statik türüdür.

Eğer *atama* bir *tuple_designation*ise, bu, *positional_pattern* , `(var` *atama*,... `)`, *tuple_designation*içinde bulunan *belirtimlerin*bulunduğu eşdeğerdir.  Örneğin, `var (x, (y, z))` `(var x, (var y, var z))`eşdeğerdir.

Ad `var` bir türe bağlandığında bu bir hatadır.

#### <a name="discard-pattern"></a>Stili at

```antlr
discard_pattern
    : '_'
    ;
```

Her zaman `_` bir ifade *e-* posta ile eşleşir. Diğer bir deyişle, her ifade atma düzeniyle eşleşir.

Bir atma deseninin bir *is_pattern_expression*kalıbı olarak kullanılması kullanılamayabilir.

#### <a name="positional-pattern"></a>Konumsal model

Konumsal bir model, giriş değerinin `null`olmadığını denetler, uygun bir `Deconstruct` yöntemini çağırır ve elde edilen değerlerde daha fazla model eşleştirmeyi gerçekleştirir.  Ayrıca, giriş değerinin türü `Deconstruct`içeren türle aynı olduğunda ya da giriş değerinin türü bir tanımlama grubu türü ise ya da deyimin çalışma zamanı türü `ITuple`uygularsa `ITuple` `object`, demet benzeri bir model söz dizimini (Bu tür olmadan) destekler.

```antlr
positional_pattern
    : type? '(' subpatterns? ')' property_subpattern? simple_designation?
    ;
subpatterns
    : subpattern
    | subpattern ',' subpatterns
    ;
subpattern
    : pattern
    | identifier ':' pattern
    ;
```

*Tür* atlanırsa, bu değeri giriş değerinin statik türü olacak şekilde sunuyoruz.

`(` *subpattern_list* `)`bir giriş değeriyle eşleşme verildiğinde, `Deconstruct` *type* erişilebilir bildirimler için *tür* arayarak ve ayrıştırma bildirimiyle aynı kurallara göre seçim yaparak bir yöntem seçilir.

*Positional_pattern* türü atladığında bir hatadır, *tanımlayıcı*olmadan tek bir *alt model* varsa, *property_subpattern* yoktur ve *simple_designation*içermez. Bu, parantez içine alınmış bir *constant_pattern* ve bir *positional_pattern*arasında ayırt edilir.

Listedeki desenlerle eşleşecek değerleri ayıklamak için
- *Tür* atlanmışsa ve giriş değerinin türü bir demet türü ise, alt desenlerin sayısının, kayıt düzeninin kardinalitesiyle aynı olması gerekir. Her demet öğesi karşılık gelen *alt düzen*ile eşleştirilir ve bu başarılı olursa eşleşme başarılı olur. Herhangi bir *alt düzenin* bir *tanımlayıcısı*varsa, bu, demet türünde ilgili konumda bir demet öğesi adı vermelidir.
- Aksi takdirde, uygun bir `Deconstruct` *türü*bir üye olarak mevcutsa, giriş *değerinin türü tür ile* *uyumlu* değilse, derleme zamanı hatası olur. Çalışma zamanında, giriş değeri *türe*göre test edilir. Bu başarısız olursa konumsal model eşleşmesi başarısız olur. Başarılı olursa, giriş değeri bu türe dönüştürülür ve `Deconstruct` `out` parametrelerini almak için derleyici tarafından oluşturulan yeni değişkenlerle çağrılır. Alınan her bir değer, karşılık gelen *alt düzen*ile eşleştirilir ve bu başarılı olursa eşleşme başarılı olur. Herhangi bir *alt düzenin* bir *tanımlayıcısı*varsa, bu, `Deconstruct`karşılık gelen konumunda bir parametre adı vermelidir.
- Aksi takdirde, *tür* atlanmışsa ve giriş değeri `object` veya `ITuple` ya da örtük bir başvuru dönüştürmesi tarafından `ITuple` dönüştürülebildiğiniz bir tür veya alt desenler arasında hiçbir *tanımlayıcı* olmazsa, `ITuple`kullanarak eşleşeceğiz.
- Aksi halde, model derleme zamanı hatasıdır.

Çalışma zamanında alt desenleri eşleştirildiği sıra belirtilmemiş ve başarısız eşleşme tüm alt desenlerle eşleştirmeye çalışmayabilir.

##### <a name="example"></a>Örnek

Bu örnek, bu belirtimde açıklanan birçok özelliği kullanır

``` c#
    var newState = (GetState(), action, hasKey) switch {
        (DoorState.Closed, Action.Open, _) => DoorState.Opened,
        (DoorState.Opened, Action.Close, _) => DoorState.Closed,
        (DoorState.Closed, Action.Lock, true) => DoorState.Locked,
        (DoorState.Locked, Action.Unlock, true) => DoorState.Closed,
        (var state, _, _) => state };
```

#### <a name="property-pattern"></a>Özellik kalıbı

Bir özellik deseninin giriş değerinin `null` olmadığını ve yinelemeli olarak erişilebilir özellikler veya alanlar kullanılarak ayıklanan değerlerle eşleştiğini kontrol eder.

```antlr
property_pattern
    : type? property_subpattern simple_designation?
    ;
property_subpattern
    : '{' '}'
    | '{' subpatterns ','? '}'
    ;
```

Bir _property_pattern_ _alt deseninin_ bir _tanımlayıcı_ içermemesi (bir _tanımlayıcıya_sahip ikinci biçimde olması gerekir) hatadır.  Son alt örüntüden sonra sondaki virgül isteğe bağlıdır.

Null denetim deseninin bir önemsiz Özellik deseninin dışına düştüğünü unutmayın. `s` dizenin null olmadığını denetlemek için aşağıdaki formlardan birini yazabilirsiniz

```csharp
if (s is object o) ... // o is of type object
if (s is string x) ... // x is of type string
if (s is {} x) ... // x is of type string
if (s is {}) ...
```

`{` *property_pattern_list* `}`düzeniyle *bir ifade ile* eşleşme verildiğinde, *type* *e* ifadesi *tür*tarafından belirlenen *t* türü ile *kalıp uyumlu* değilse, derleme zamanı hatası olur. Tür yoksa, bu, *e*'nin statik türü olarak ele alır. *Tanımlayıcı* varsa *, tür türünde*bir model değişkeni bildirir. *Property_pattern_list* sol tarafında görünen tanımlayıcıların her biri, erişilebilir okunabilir bir özellik veya *alan olarak belirtmelidir.* *Property_pattern* *simple_designation* varsa *T*türünde bir model değişkeni tanımlar.

Çalışma zamanında, ifade *T*'ye göre test edilir. Bu başarısız olursa, özellik deseninin eşleşmesi başarısız olur ve sonuç `false`. Başarılı olursa, her bir *property_subpattern* alanı veya özelliği okunurdur ve değeri karşılık gelen düzeniyle eşleştirilir. Tam eşleşmenin sonucu yalnızca, bunlardan herhangi birinin sonucu `false``false`. Alt desenlerin eşleştirildiği sıra belirtilmemiş ve başarısız eşleşme çalışma zamanında tüm alt desenlerle eşleşmeyebilir. Eşleşme başarılı olursa ve *property_pattern* *simple_designation* bir *single_variable_designation*ise, eşleşen değere atanmış *T* türünde bir değişkeni tanımlar.

> Note: Özellik deseninin, anonim türlerle eşleşmesi için kullanılabilir.

##### <a name="example"></a>Örnek

```csharp
if (o is string { Length: 5 } s)
```

### <a name="switch-expression"></a>Anahtar Ifadesi

Bir ifade bağlamı için `switch`benzeri semantiğini desteklemek üzere bir *switch_expression* eklenir.

C# Dil sözdizimi, aşağıdaki sözdizimsel üretimlerle birlikte artırmadır:

```antlr
multiplicative_expression
    : switch_expression
    | multiplicative_expression '*' switch_expression
    | multiplicative_expression '/' switch_expression
    | multiplicative_expression '%' switch_expression
    ;
switch_expression
    : range_expression 'switch' '{' '}'
    | range_expression 'switch' '{' switch_expression_arms ','? '}'
    ;
switch_expression_arms
    : switch_expression_arm
    | switch_expression_arms ',' switch_expression_arm
    ;
switch_expression_arm
    : pattern case_guard? '=>' expression
    ;
case_guard
    : 'when' null_coalescing_expression
    ;
```

*Switch_expression* *expression_statement*olarak izin verilmez.

> Bu, gelecekteki bir düzeltmeye yönelik olarak size bakıyor.

*Switch_expression* türü, böyle bir tür varsa ve anahtar ifadesinin her ARM içindeki ifade örtülü olarak bu türe dönüştürülebileceğinden, *switch_expression_arm*s `=>` belirteçlerinin sağında görünen ifadelerin [*en iyi ortak türüdür*](https://github.com/dotnet/csharplang/blob/master/spec/expressions.md#finding-the-best-common-type-of-a-set-of-expressions) .  Ayrıca, her bir ARM ifadesinin `T`için örtük bir dönüştürme var `T` her türe bir anahtar ifadesinden önceden tanımlanmış bir örtük dönüştürme olan yeni bir *anahtar ifadesi dönüştürmesi*ekledik.

Bazı önceki desenler ve korumada her zaman eşleşeceğinden, bazı *switch_expression_arm*deseninin sonucu etkilemediği bir hatadır.

Anahtar ifadesinin bir kolu, girişinin her değerini *işlediğinde, anahtar* ifadesi tam olarak kabul edilir.  Anahtar ifadesi *ayrıntılı*değilse, derleyici bir uyarı üretir.

Çalışma zamanında, *switch_expression* sonucu, *switch_expression* sol tarafındaki ifadenin *switch_expression_arm*düzeniyle eşleştiği ve *case_guard, varsa* switch_expression_arm sonucunu veren ilk *switch_expression_arm* *ifadesinin* değeridir. Bu, `true`olarak değerlendiriliyorsa *.* Böyle bir *switch_expression_arm*yoksa, *switch_expression* özel durum `System.Runtime.CompilerServices.SwitchExpressionException`örneği oluşturur.

### <a name="optional-parens-when-switching-on-a-tuple-literal"></a>Bir demet sabit değerinde geçiş yaparken isteğe bağlı paranlar

*Switch_statement*kullanarak bir demet hazır bilgisine geçiş yapmak için, gereksiz parmalar gibi görünen öğeleri yazmanız gerekir

```csharp
switch ((a, b))
{
```

İzin vermek için

```csharp
switch (a, b)
{
```

switch ifadesinin parantezleri, geçiş yapılan ifade bir demet sabit değeri olduğunda isteğe bağlıdır.

### <a name="order-of-evaluation-in-pattern-matching"></a>Düzen eşleme içinde değerlendirme sırası

Desenler eşleme sırasında yürütülen işlemlerin yeniden sıralanması için derleme esnekliği sağlamak, desenli eşleme verimliliğini artırmak için kullanılabilecek esnekliğe izin verebilir. (Zorlanmaz) gereksinimi, bir düzende erişilen özelliklerin ve Deyapý yöntemlerinin "saf" olması gerekir (yan etki, ıdempotent, vb.). Bu, yalnızca yeniden düzenleme işlemlerinde derleyici esnekliğine izin verdiğimiz bir dil kavramı olarak önemli bir değer ekleyeceğiz.

**Çözüm 2018-04-04 LDM**: onaylandı: derleyicinin, `ITuple``Deconstruct`, özellik erişimlerine ve yöntemlerin çağrılarına yeniden düzenleme yapmasına izin verilir ve döndürülen değerlerin birden çok çağrının aynı olduğunu varsayabilir. Derleyici, sonucu etkileyebilecek işlevleri çağırmamalıdır ve gelecekte derleyicinin ürettiği değerlendirme sırasında değişiklik yapmadan önce çok dikkatli olacak.

### <a name="some-possible-optimizations"></a>Bazı olası Iyileştirmeler

Desen eşleştirme derlemesi, desenlerin yaygın parçalarından faydalanabilir. Örneğin, bir *switch_statement* art arda iki desenin en üst düzey tür testi aynı türde ise, oluşturulan kod ikinci desen için tür testini atlayabilir.

Desenlerin bazıları tamsayı veya dizeler olduğunda, derleyici, dilin önceki sürümlerindeki bir switch ifadesinin oluşturduğu aynı kod türünü oluşturabilir.

Bu tür iyileştirmeler hakkında daha fazla bilgi için bkz. [[Scott ve Ramsey (2000)]](https://www.cs.tufts.edu/~nr/cs257/archive/norman-ramsey/match.pdf "Ne zaman eşle, derleme buluşsal yöntemlerini ne zaman").
