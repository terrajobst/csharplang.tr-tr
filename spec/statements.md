---
ms.openlocfilehash: 8f9551b9e7f70379836c23a60f0d37dc02f8e18e
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47230003"
---
# <a name="statements"></a>Deyimler

C# ifadeleri çeşitli sağlar. Bu deyimler çoğu C ve C++ içinde programlanmış geliştiriciler için tanıdık gelecektir.

```antlr
statement
    : labeled_statement
    | declaration_statement
    | embedded_statement
    ;

embedded_statement
    : block
    | empty_statement
    | expression_statement
    | selection_statement
    | iteration_statement
    | jump_statement
    | try_statement
    | checked_statement
    | unchecked_statement
    | lock_statement
    | using_statement
    | yield_statement
    | embedded_statement_unsafe
    ;
```

*Embedded_statement* bildirimlere diğer deyimleri içinde görünen deyimleri için kullanılır. Kullanımını *embedded_statement* yerine *deyimi* bildirim deyimleri ve etiketli deyimler şu bağlamlarda dışlar. Örnek
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
sonuçları bir derleme zamanı hatası nedeniyle bir `if` çalıştın ve bir *embedded_statement* yerine *deyimi* kendi IF için dal. Bu kod izin veriyorsa, ardından değişken `i` bildirilen, ancak hiçbir zaman kullanılabilir. Ancak, unutmayın, yerleştirerek `i`ait bildirimi bir blok içinde örnek geçerlidir.

## <a name="end-points-and-reachability"></a>Uç noktaları ve erişilebilirlik

Her deyim sahip bir ***uç noktası***. Sezgisel bağlamında, bir deyim bitiş noktasını deyim hemen izleyen konumdur. Bileşik deyimler (katıştırılmış deyimleri içeren ifadeler) yürütme kuralları denetimi bir katıştırılmış deyim bitiş noktasını ulaştığında alınmış eylemi belirtin. Örneğin, denetim bir blok içinde bir ifade bitiş noktasını ulaştığında, denetim bloğunda sonraki deyime aktarılır.

Bir deyimi yürütme büyük olasılıkla ulaşılabilirse, deyim olduğu söylenir ***erişilebilir***. Bir deyimi yürütülür olasılığı varsa, buna karşılık, deyim olduğu söylenir ***ulaşılamaz***.

Örnekte
```csharp
void F() {
    Console.WriteLine("reachable");
    goto Label;
    Console.WriteLine("unreachable");
    Label:
    Console.WriteLine("reachable");
}
```
İkinci çağırmayı `Console.WriteLine` deyimi yürütülür olasılığı olduğundan erişilemiyor.

Derleyici bir deyim ulaşılamaz olduğunu belirlerse, bir uyarı bildirilir. Özellikle değil ulaşılamaz olarak bir deyim için hatadır.

Belirli bir deyimi veya uç noktası erişilebilir olup olmadığını belirlemek için akış analizi for each deyimi tanımlanan ulaşılabilirlik kurallara göre derleyici gerçekleştirir. Akış analizi, sabit ifadeler değerlerini dikkate alır ([sabit ifadeler](expressions.md#constant-expressions)) deyimleri davranışını kontrol eden, ancak sabit olmayan ifade olası değerlerin değil olarak kabul edilir. Diğer bir deyişle, denetim akışı analizi amacıyla herhangi bir olası değer türü olması, belirli bir türde bir sabit olmayan ifade değerlendirilir.

Örnekte
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
Boole ifadesi `if` deyimi olduğundan sabit bir ifade iki işleneni de `==` işleci değerlerdir. Derleme zamanında sabit ifade değerlendirmesinde gibi değer üreten `false`, `Console.WriteLine` çağırma ulaşılamaz olarak değerlendirilir. Ancak, varsa `i` yerel bir değişken olarak değiştirilir
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` gerçekte hiçbir zaman yürütülür olsa da, çağırma erişilebilir, değerlendirilir.

*Blok* işlevi üye her zaman erişilebilir olarak kabul edilir. Her deyim bloğunda ulaşılabilirlik kurallarına sırayla değerlendirerek, herhangi bir deyimle erişilebilirliğini belirlenebilir.

Örnekte
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
İkinci erişilebilirliğini `Console.WriteLine` şu şekilde belirlenir:

*  İlk `Console.WriteLine` ifade deyimi erişilebilir olduğundan bloğunu `F` yöntemi erişilebilir.
*  İlk uç noktası `Console.WriteLine` ifade deyimi, o ifadeyi erişilebilir olduğu için erişilebilir.
*  `if` Deyimi, ilk uç noktası için erişilebilir `Console.WriteLine` ifade deyimi erişilebilir.
*  İkinci `Console.WriteLine` ifade deyimi erişilebilir olduğundan boolean ifadesi `if` ifadesi sabit değer yok `false`.

Bir derleme zamanı hatası erişilebilir olması için tablo uç noktası için olan iki durum vardır:

*  Çünkü `switch` deyimi "geçiş için" anahtar bölüm sonraki switch bölümüne vermez, erişilebilir olması için bir anahtar bölümü deyim listesinin uç noktası için bir derleme zamanı hata. Bu hata meydana gelirse, genellikle bir göstergesi olduğu, bir `break` deyimi eksik.
*  Bloğun erişilebilir olması için bir değer hesaplayan bir işlevi üyesinin uç noktası için bir derleme zamanı hatasıdır. Bu hata meydana gelirse, genellikle bir göstergesi olduğu, bir `return` deyimi eksik.

## <a name="blocks"></a>Bloklar

A *blok* bağlamlarda yazılacak birden çok deyime burada tek bir deyimde izin verir.

```antlr
block
    : '{' statement_list? '}'
    ;
```

A *blok* isteğe oluşur *statement_list* ([deyimi listeler](statements.md#statement-lists)) küme ayraçları içine alınan. İfade listesinin atlanırsa, blok boş olarak kabul edilir.

Bildirim deyimleri bir blok içerebilir ([bildirim deyimleri](statements.md#declaration-statements)). Bir yerel değişken veya sabit bir blokta bildirilen blok kapsamıdır.

Bir blok gibi yürütülür:

*  Blok boşsa, denetimi bloğun uç noktasına aktarılır.
*  Blok boş değilse, Denetim deyimi listeye aktarılır. Zaman ve denetim deyim listesinin bitiş noktasını ulaşırsa, denetimi bloğun uç noktasına aktarılır.

Deyimi bir blok blok ulaşılıp ulaşılamadığını erişilebilir listesidir.

Uç noktası bir bloğun blok boş ise veya deyim listesinin bitiş noktasını ulaşılıp ulaşılamadığını erişilebiliyor.

A *blok* bir veya daha fazla bilgi içeren `yield` deyimleri ([yield deyimi](statements.md#the-yield-statement)) yineleyici bloğu çağrılır. Yineleyici bloğu yineleyiciler olarak işlev üyeleri uygulamak için kullanılır ([yineleyiciler](classes.md#iterators)). Yineleyici bloğu için bazı ek sınırlamalar:

*  Bir derleme zamanı hata için bir `return` yineleyici bloğu içinde görüntülenecek deyimi (ancak `yield return` deyimleri izin verilir).
*  Güvenli olmayan bir bağlam içerecek şekilde yineleyici bloğu için bir derleme zamanı hata ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)). Bile bildiriminden güvenli olmayan bir bağlamda yer alan bir yineleyici bloğu her zaman güvenli bir bağlamı tanımlar.

### <a name="statement-lists"></a>Deyimi listeler

A ***deyim listesinin*** sırada yazılmasını bir veya daha fazla deyim içerir. Deyimi listeleri gerçekleştirilir *blok*s ([blokları](statements.md#blocks)) ve *switch_block*s ([switch deyimi](statements.md#the-switch-statement)).

```antlr
statement_list
    : statement+
    ;
```

İfade listesinin denetim ilk deyime aktarılması yoluyla yürütülür. Zaman ve Denetim deyiminin uç noktası ulaşırsa, Denetim sonraki deyime aktarılır. Zaman ve denetim son deyim bitiş noktasını ulaşırsa, Denetim deyim listesinin son noktasına aktarılır.

Deyim listesinin deyiminde aşağıdakilerden en az biri doğruysa erişilebilir:

*  Deyim ilk deyim ve deyim listesinin erişilebilir.
*  Önceki deyim bitiş noktasının erişilebilir.
*  Etiketli deyim ifadesi ve etiket tarafından erişilebilir başvurulan `goto` deyimi.

Listedeki son deyim bitiş noktasını ulaşılıp ulaşılamadığını deyim listesinin bitiş noktasını erişilebilir.

## <a name="the-empty-statement"></a>Boş deyim

Bir *empty_statement* hiçbir şey yapmaz.

```antlr
empty_statement
    : ';'
    ;
```

Olduğunda bir bağlamda gerçekleştirmek için bir işlem bir deyim gerekli olduğu boş bir deyimi kullanılır.

Boş bir deyimi yürütme denetimi deyiminin uç noktasına yalnızca aktarır. Bu nedenle, boş bir deyimi bitiş noktası boş deyim ulaşılıp ulaşılamadığını erişilebilir.

Boş bir deyimi yazılırken kullanılan bir `while` boş gövdesi olan deyimi:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Ayrıca, boş bir deyimi yalnızca kapatmadan önce bir etiket bildirmek için kullanılabilir "`}`" bir blok:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Etiketli deyimler

A *labeled_statement* bir ifade tarafından bir etiket öneki verir. Etiketli deyimler bloklarında izin verilir, ancak katıştırılmış deyimler izin verilmez.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Etiketli deyim tarafından verilen ada sahip bir etiket bildirir *tanımlayıcı*. Tüm bloğu kapsamdır bir etiketin etiket bildirildiği içinde dahil iç içe bloklarınız. Çakışan bir kapsam için aynı ada sahip iki etiket için bir derleme zamanı hatasıdır.

Bir etiket başvurulabilir `goto` deyimleri ([goto deyimi](statements.md#the-goto-statement)) etiketin kapsamı içinde. Diğer bir deyişle `goto` deyimleri, denetim blokları içinde ve dışında blokları, ancak hiçbir zaman bloklara aktarabilir.

Etiketler, kendi bildirim alanına sahip ve diğer tanımlayıcıları ile müdahale etmez. Örnek
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
geçerli olduğundan ve bu adı kullanan `x` hem bir parametre hem de bir etiket olarak.

Etiketli deyim yürütülmesini etiketi takiben deyime sahip yürütme için tam olarak karşılık gelir.

Normal denetim akışını tarafından sağlanan erişilebilirlik ek olarak etiketli bir deyim etiketi tarafından erişilebilir başvuruyorsa ulaşılabildiğinden `goto` deyimi. (Özel durum: Varsa bir `goto` deyimdir içinde bir `try` içeren bir `finally` blok ve etiketli deyim olduğu dışında `try`ve bitiş noktasını `finally` blok erişilemez ve etiketli deyim öğesinden erişilebilir değil `goto` deyimi.)

## <a name="declaration-statements"></a>Bildirim deyimleri

A *declaration_statement* bir yerel değişken veya sabiti bildirir. Bildirim deyimleri bloklarında izin verilir, ancak katıştırılmış deyimler izin verilmez.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Yerel değişken bildirimleri

A *local_variable_declaration* veya daha fazla yerel değişken bildirir.

```antlr
local_variable_declaration
    : local_variable_type local_variable_declarators
    ;

local_variable_type
    : type
    | 'var'
    ;

local_variable_declarators
    : local_variable_declarator
    | local_variable_declarators ',' local_variable_declarator
    ;

local_variable_declarator
    : identifier
    | identifier '=' local_variable_initializer
    ;

local_variable_initializer
    : expression
    | array_initializer
    | local_variable_initializer_unsafe
    ;
```

*Local_variable_type* , bir *local_variable_declaration* doğrudan bildirim tarafından tanıtılan değişkenler türünü belirtir veya tanımlayıcısıyla gösterir `var` , bir başlatıcı üzerinde temel türü çıkarılmamalıdır. Tür listesi tarafından izlenir *local_variable_declarator*s, her biri yeni bir değişken tanıtır. A *local_variable_declarator* oluşan bir *tanımlayıcı* ardından isteğe bağlı olarak, değişken adları bir "`=`" belirteci ve *local_variable_initializer* değişkenin başlangıç değerini vermektedir.

Bir yerel değişken bildiriminde bağlamında tanımlayıcı var bağlamsal anahtar sözcük işlevi görür. ([anahtar sözcükleri](lexical-structure.md#keywords)). Zaman *local_variable_type* olarak belirtilen `var` ve adlandırılmış tür `var` olduğu bildirimi kapsam içinde olan bir ***yerel değişken bildiriminde örtük***, türü olan ilişkili Başlatıcı ifadesinin türünden çıkarsanan. Türü örtük olarak belirlenmiş yerel değişken bildirimlerini aşağıdaki kısıtlamalar şunlardır:

*  *Local_variable_declaration* birden çok içeremez *local_variable_declarator*s.
*  *Local_variable_declarator* içermelidir bir *local_variable_initializer*.
*  *Local_variable_initializer* olmalıdır bir *ifade*.
*  Başlatıcı *ifade* bir derleme zamanı türü olmalıdır.
*  Başlatıcı *ifade* bildirilmiş bir değişken için kendisine başvuramaz

Yanlış türü örtük olarak belirlenmiş yerel değişken tanımlama örnekleri şunlardır:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Yerel bir değişken değerini bir ifade kullanılarak elde edilir bir *simple_name* ([basit adları](expressions.md#simple-names)), ve kullanarak yerel bir değişken değerini değiştiren bir *atama* () [Atama işleçleri](expressions.md#assignment-operators)). Yerel bir değişken kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) her bir konumdaki değeri elde burada.

Bildirilen yerel değişken kapsamını bir *local_variable_declaration* blok bildirimi oluştuğu içinde. Önündeki değerinin metinsel bir konumda yerel bir değişkene başvurmak için bir hata olduğunu *local_variable_declarator* yerel değişkenin. Yerel bir değişken kapsamında buna başka bir yerel değişken ya da aynı ada sahip sabit bildirmek için bir derleme zamanı hatası var.

Birden fazla değişken bildiren bir yerel değişken bildiriminde tek değişkenlerin birden çok bildirimleri aynı türe ile eşdeğerdir. Ayrıca, yerel bir değişken bildirimi bir değişken başlatıcısında bildiriminden hemen sonra eklenen atama deyiminin tam olarak karşılık gelir.

Örnek
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
tam olarak karşılık geldiğinden
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

Bir türü örtük olarak belirlenmiş yerel değişken bildiriminde bildirilen yerel değişken türünü değişkeni başlatmak için kullanılan ifade türü ile aynı olacak şekilde alınır. Örneğin:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Türü örtük olarak belirlenmiş yerel değişken bildirimlerini yukarıdaki aşağıdaki açıkça belirlenmiş bildirimleri için tam olarak eşdeğerdir:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Yerel sabit bildirimleri

A *local_constant_declaration* bir veya daha fazla yerel sabit bildirir.

```antlr
local_constant_declaration
    : 'const' type constant_declarators
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

*Türü* , bir *local_constant_declaration* bildirim tarafından tanıtılan sabitleri türünü belirtir. Tür listesi tarafından izlenir *constant_declarator*s, her biri yeni bir sabit tanıtır. A *constant_declarator* oluşan bir *tanımlayıcı* sabiti adlarının arkasından bir "`=`" belirteci ve ardından bir *constant_expression* ([ Sabit ifadeler](expressions.md#constant-expressions)), sabit değerini verir.

*Türü* ve *constant_expression* yerel sabit bir bildirimde sabit üye bildirimi aynı kuralları izlemelidir ([sabitleri](classes.md#constants)).

Bir yerel sabit değeri bir ifade kullanarak elde edilen bir *simple_name* ([basit adları](expressions.md#simple-names)).

Bir yerel sabit kapsamını bloğudur bildirimi oluştuğu içinde. Önündeki değerinin metinsel bir konumda yerel bir sabit belirtmek için bir hata olduğunu, *constant_declarator*. Yerel sabit kapsamında, başka bir yerel değişken veya aynı ada sahip bir sabit bildirme bir derleme zamanı hatasıdır.

Birden fazla sabit bildiren bir yerel sabit bildiriminde tek sabitlerin aynı türde birden fazla bildirimi eşdeğerdir.

## <a name="expression-statements"></a>İfade deyimleri

Bir *expression_statement* belirtilen ifadeyi hesaplar. İfade tarafından hesaplanan değer, varsa, göz ardı edilir.

```antlr
expression_statement
    : statement_expression ';'
    ;

statement_expression
    : invocation_expression
    | null_conditional_invocation_expression
    | object_creation_expression
    | assignment
    | post_increment_expression
    | post_decrement_expression
    | pre_increment_expression
    | pre_decrement_expression
    | await_expression
    ;
```

Tüm ifadeler deyimleri izin verilir. Gibi belirli, ifadelerde `x + y` ve `x == 1` , yalnızca işlem (atılır) bir değer, ifadeleri izin verilmez.

Yürütülmesini bir *expression_statement* kapsanan ifadeyi değerlendirir ve bitiş noktası için Denetim aktarır *expression_statement*. Bitiş noktasını bir *expression_statement* erişilebilir olmadığını *expression_statement* erişilebilir.

## <a name="selection-statements"></a>Seçim deyimleri

Seçim deyimleri birkaç olası deyimler yürütme bazı ifadesinin değerine göre birini seçin.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>İf deyimi

`if` Deyimi, bir boolean ifadesinin değerine göre yürütülecek bir deyim seçer.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Bir `else` parçasıdır sözcüksel olarak en yakın önceki ile ilişkili `if` sözdizimi tarafından verilir. Bu nedenle, bir `if` formun deyimi
```csharp
if (x) if (y) F(); else G();
```
eşdeğerdir
```csharp
if (x) {
    if (y) {
        F();
    }
    else {
        G();
    }
}
```

Bir `if` deyim şu şekilde gerçekleştirilir:

*  *Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.
*  Boole ifadesine döndürürse `true`, denetimi, ilk katıştırılmış deyime aktarılır. Zaman ve denetim bu bildirimi bitiş noktasını ulaşırsa, denetim için bitiş noktasını aktarılır `if` deyimi.
*  Boole ifadesine döndürürse `false` ve bir `else` bölümü varsa, Denetim, ikinci katıştırılmış deyime aktarılır. Zaman ve denetim bu bildirimi bitiş noktasını ulaşırsa, denetim için bitiş noktasını aktarılır `if` deyimi.
*  Boole ifadesine döndürürse `false` ve bir `else` bölümü yoksa, denetim için bitiş noktasını aktarılan `if` deyimi.

İlk katıştırılmış deyimi bir `if` deyimi erişilebilir değilse `if` ifadesi erişilebilir ve Boole ifadesi sabit değer olmayan `false`.

İkinci katıştırılmış deyimi bir `if` deyimi, varsa, erişilebilir değilse `if` ifadesi erişilebilir ve Boole ifadesi sabit değer olmayan `true`.

Bitiş noktasını bir `if` deyimi, bir katıştırılmış deyim en az bir uç noktası ulaşılıp ulaşılamadığını erişilebilir. Ayrıca, uç noktasını bir `if` deyimi olmadan `else` parçasıdır erişilebilir değilse `if` ifadesi erişilebilir ve Boole ifadesi sabit değer yok `true`.

### <a name="the-switch-statement"></a>Switch deyimi

Switch deyimi için yürütme switch ifadesinin değerine karşılık gelen bir ilişkili anahtar etiketine sahip bir deyim listesinin seçer.

```antlr
switch_statement
    : 'switch' '(' expression ')' switch_block
    ;

switch_block
    : '{' switch_section* '}'
    ;

switch_section
    : switch_label+ statement_list
    ;

switch_label
    : 'case' constant_expression ':'
    | 'default' ':'
    ;
```

A *switch_statement* anahtar sözcüğünü oluşur `switch`, parantezli ifade (switch ifadesi olarak adlandırılır) tarafından izlenen, arkasından bir *switch_block*. *Switch_block* sıfır veya daha fazla oluşur *switch_section*s, küme ayraçları içine alınmış. Her *switch_section* bir veya daha fazla oluşur *switch_label*s arkasından bir *statement_list* ([deyimi listeler](statements.md#statement-lists)).

***Türü yöneten*** , bir `switch` ifadesi switch ifadesi kurulur.

*  Switch ifadesinin türü ise `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `bool`, `char`, `string`, veya bir *enum_type*, veya bu türlerden birine karşılık gelen null olabilen türü olan ardından yöneten olan türü `switch` deyimi.
*  Aksi halde, tam olarak bir kullanıcı tanımlı örtük dönüştürme ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)) switch ifadesinin türünden türleri yöneten aşağıdaki olası biri mevcut olmalıdır: `sbyte`, `byte`, `short` , `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `string`, veya bu türlerden birine karşılık gelen null yapılabilir bir tür.
*  Aksi takdirde, böyle bir örtük dönüştürme var ya da daha fazlası gibi örtük bir dönüştürme var, bir derleme zamanı hatası oluşur.

Her sabit ifade `case` etiket örtük olarak dönüştürülebilir bir değer belirtmek gerekir ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) değerlendirip türüne `switch` deyimi. İki veya daha fazla, bir derleme zamanı hatası meydana gelir `case` etiketleri aynı `switch` deyimi aynı sabit değeri belirtin.

En fazla bir olabilir `default` switch deyiminde etiketi.

A `switch` deyim şu şekilde gerçekleştirilir:

*  Switch ifadesi değerlendirilir ve değerlendirip türüne dönüştürülür.
*  Sabitlerinden birini içinde belirtilen bir `case` aynı etiketi `switch` ifadesi switch ifadesinin değerine eşittir, Denetim, eşleşen aşağıdaki deyim listesinin aktarılır `case` etiketi.
*  Belirtilen sabitlerden hiçbiri `case` etiketleri aynı `switch` ifadesi switch ifadesinin değerine eşittir ve bir `default` etiket varsa, Denetim listesi deyime aktarılır `default` Etiket.
*  Belirtilen sabitlerden hiçbiri `case` etiketleri aynı `switch` ifadesi switch ifadesinin değerine eşittir ve hiçbir `default` etiket varsa, denetim için bitiş noktasını aktarılan `switch` deyimi.

İfade listesinin bir anahtar bölümünün bitiş noktasını erişilebiliyorsa, bir derleme zamanı hatası oluşur. Bu, "fall aracılığıyla" kuralı olarak bilinir. Örnek
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
default:
    CaseOthers();
    break;
}
```
hiçbir anahtar bölümü erişilebilir bir uç noktaya sahip olduğundan geçerli değil. C ve C++'ın aksine, bir anahtar bölümün yürütülmesi "geçiş" sonraki switch bölümüne ve örnek iznine sahip değil
```csharp
switch (i) {
case 0:
    CaseZero();
case 1:
    CaseZeroOrOne();
default:
    CaseAny();
}
```
bir derleme zamanı hatası sonuçlanır. Bir switch bölümüne yürütülmesini olduğunda başka bir anahtar bölümü, açık bir yürütülmesini tarafından izlenen `goto case` veya `goto default` deyimi kullanılmalıdır:
```csharp
switch (i) {
case 0:
    CaseZero();
    goto case 1;
case 1:
    CaseZeroOrOne();
    goto default;
default:
    CaseAny();
    break;
}
```

Birden çok etiketle izin verilen bir *switch_section*. Örnek
```csharp
switch (i) {
case 0:
    CaseZero();
    break;
case 1:
    CaseOne();
    break;
case 2:
default:
    CaseTwo();
    break;
}
```
geçerli değil. Örneğin "fall aracılığıyla" kuralı nedeniyle ihlal etmemesini etiketleri `case 2:` ve `default:` aynı parçası olan *switch_section*.

Genel bir sınıf C ve C++ içinde oluşan hataların "fall aracılığıyla" kuralı engelleyen zaman `break` deyimleri yanlışlıkla atlanmış. Ayrıca, bu kural, anahtar bölümlerini nedeniyle bir `switch` deyimi rasgele düzenlenmeyecek deyim davranışını etkilemeden. Örneğin, bölümlerini `switch` yukarıdaki ifade deyimi davranışını etkilemeden alınması:
```csharp
switch (i) {
default:
    CaseAny();
    break;
case 1:
    CaseZeroOrOne();
    goto default;
case 0:
    CaseZero();
    goto case 1;
}
```

Bir switch bölümüne ifade listesi genellikle biten bir `break`, `goto case`, veya `goto default` deyimi, ancak ulaşılamaz ifade listesinin bitiş noktasını işleyen herhangi bir yapı izin verilir. Örneğin, bir `while` Boole ifadesine göre denetlenen deyim `true` uç noktasına hiçbir zaman için erişim verilir. Benzer şekilde, bir `throw` veya `return` ifade her zaman, başka bir yerde denetim aktarır ve hiçbir zaman uç noktasına ulaşır. Bu nedenle, aşağıdaki örnekte geçerlidir:
```csharp
switch (i) {
case 0:
    while (true) F();
case 1:
    throw new ArgumentException();
case 2:
    return;
}
```

Değerlendirip türünü bir `switch` deyim türü olabilir `string`. Örneğin:
```csharp
void DoCommand(string command) {
    switch (command.ToLower()) {
    case "run":
        DoRun();
        break;
    case "save":
        DoSave();
        break;
    case "quit":
        DoQuit();
        break;
    default:
        InvalidCommand(command);
        break;
    }
}
```

Gibi dize eşitlik işleçleri ([dize eşitlik işleçleri](expressions.md#string-equality-operators)), `switch` deyimi büyük/küçük harfe duyarlıdır ve yalnızca switch ifadesi dize tam olarak eşleşmesi durumunda belirli bir anahtar bölüm yürütecek bir `case` etiketi sabit değer.

Düzenleyen zaman türü bir `switch` deyimi `string`, değer `null` durum etiketi sabit olarak izin verilir.

*Statement_list*sn bir *switch_block* bildirim deyimleri içerebilir ([bildirim deyimleri](statements.md#declaration-statements)). Bir yerel değişken veya sabit anahtar blokta bildirilen switch bloğu kapsamıdır.

Belirli bir anahtar bölümündeki ifade listesinin erişilebilir olduğundan, `switch` ifadesi erişilebilir ve aşağıdakilerden en az birini doğrudur:

*  Switch ifadesi bir sabit değerdir.
*  Switch ifadesi ile eşleşen bir sabit değer olan bir `case` switch bölümüne etiketi.
*  Switch ifadesi herhangi eşleşmeyen bir sabit değerdir `case` etiket ve anahtar bölümü içeren `default` etiketi.
*  Switch bölümüne bir anahtar etiketi tarafından erişilebilir başvurulan `goto case` veya `goto default` deyimi.

Bitiş noktasını bir `switch` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:

*  `switch` Deyimi içeren bir erişilebilir `break` çıkar deyimi `switch` deyimi.
*  `switch` Deyimi erişilebilir, switch ifadesi bir sabit olmayan değer ve Hayır `default` etiketi görüntülenir.
*  `switch` Deyimi erişilebilir, switch ifadesi herhangi eşleşmeyen bir sabit değerdir `case` etiket ve Hayır `default` etiketi görüntülenir.

## <a name="iteration-statements"></a>Yineleme deyimleri

Yineleme deyimleri, bir katıştırılmış deyim tekrar tekrar yürütün.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While ifadesi

`while` Deyimi, bir katıştırılmış deyim sıfır veya daha fazla kez koşullu yürütür.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

A `while` deyim şu şekilde gerçekleştirilir:

*  *Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.
*  Boole ifadesine döndürürse `true`, Denetim, katıştırılmış deyime aktarılır. Zaman ve denetim gömülü deyim bitiş noktasını ulaşırsa (büyük olasılıkla yürütmesine bir `continue` deyimi), Denetim başlangıcına aktarılan `while` deyimi.
*  Boole ifadesine döndürürse `false`, denetim için bitiş noktasını aktarılan `while` deyimi.

Katıştırılmış deyimi içinde bir `while` deyimi, bir `break` deyimi ([break deyimi](statements.md#the-break-statement)) denetimi için bitiş noktasını aktarmak için kullanılabilir `while` (Bu nedenle Embedded yineleme bitiş deyimi deyimi) ve bir `continue` deyimi ([CONTINUE deyimi](statements.md#the-continue-statement)) uç noktasına katıştırılmış deyiminin denetim aktarmak için kullanılabilir (Bu nedenle, başka bir yineleme gerçekleştirme `while` deyimi).

Katıştırılmış deyimi bir `while` deyimi erişilebilir değilse `while` ifadesi erişilebilir ve Boole ifadesi sabit değer olmayan `false`.

Bitiş noktasını bir `while` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:

*  `while` Deyimi içeren bir erişilebilir `break` çıkar deyimi `while` deyimi.
*  `while` İfadesi erişilebilir ve Boole ifadesi sabit değer olmayan `true`.

### <a name="the-do-statement"></a>Do deyimi

`do` Deyimi, bir katıştırılmış deyimi bir veya daha fazla kez koşullu yürütür.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

A `do` deyim şu şekilde gerçekleştirilir:

*  Denetim, katıştırılmış deyime aktarılır.
*  Zaman ve denetim gömülü deyim bitiş noktasını ulaşırsa (büyük olasılıkla yürütülmesini gelen bir `continue` deyimi), *boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir. Boole ifadesine döndürürse `true`, Denetim başlangıcına aktarılan `do` deyimi. Aksi takdirde, denetim için bitiş noktasını aktarılır `do` deyimi.

Katıştırılmış deyimi içinde bir `do` deyimi, bir `break` deyimi ([break deyimi](statements.md#the-break-statement)) denetimi için bitiş noktasını aktarmak için kullanılabilir `do` (Bu nedenle Embedded yineleme bitiş deyimi deyimi) ve bir `continue` deyimi ([CONTINUE deyimi](statements.md#the-continue-statement)) uç noktasına katıştırılmış deyiminin denetim aktarmak için kullanılabilir.

Katıştırılmış deyimi bir `do` deyimi erişilebilir değilse `do` deyimi erişilebilir.

Bitiş noktasını bir `do` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:

*  `do` Deyimi içeren bir erişilebilir `break` çıkar deyimi `do` deyimi.
*  Gömülü deyim bitiş noktasının erişilebilir olduğundan ve Boole ifadesi sabit değer olmayan `true`.

### <a name="the-for-statement"></a>For deyimi

`for` Deyimi bir dizi başlatma ifadeleri değerlendirir ve daha sonra bir koşul true olduğu sürece art arda katıştırılmış bir deyimi yürütür ve bir dizi yineleme ifadeleri değerlendirir.

```antlr
for_statement
    : 'for' '(' for_initializer? ';' for_condition? ';' for_iterator? ')' embedded_statement
    ;

for_initializer
    : local_variable_declaration
    | statement_expression_list
    ;

for_condition
    : boolean_expression
    ;

for_iterator
    : statement_expression_list
    ;

statement_expression_list
    : statement_expression (',' statement_expression)*
    ;
```

*For_initializer*, varsa, birini oluşan bir *local_variable_declaration* ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) veya bir listesini *statement_ ifade*s ([ifade deyimleri](statements.md#expression-statements)) virgülle ayrılmış. Tarafından bildirilen yerel değişken kapsamını bir *for_initializer* başlar *local_variable_declarator* değişken için ve katıştırılmış deyim sonuna kadar genişletir. Kapsamının *for_condition* ve *for_iterator*.

*For_condition*, varsa, olmalıdır bir *boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)).

*For_iterator*, varsa, bir listesinden oluşur *statement_expression*s ([ifade deyimleri](statements.md#expression-statements)) virgülle ayrılmış.

Deyimi için şu şekilde gerçekleştirilir:

*  Varsa bir *for_initializer* değişken başlatıcıları varsa veya deyim ifadeleri, bunlar yazılır sırayla yürütülür. Bu adım yalnızca bir kez gerçekleştirilir.
*  Varsa bir *for_condition* var, bunu değerlendirilir.
*  Varsa *for_condition* yoksa veya değerlendirme verir durumunda değil `true`, Denetim, katıştırılmış deyime aktarılır. Zaman ve denetim gömülü deyim bitiş noktasını ulaşırsa (büyük olasılıkla yürütülmesini gelen bir `continue` deyimi), ifadelerin *for_iterator*, varsa, sırayla değerlendirilir ve ardından başka bir yineleme değerlendirmesi ile başlayarak, gerçekleştirilen *for_condition* Yukarıdaki adımda.
*  Varsa *for_condition* var ve değerlendirme verir `false`, denetim için bitiş noktasını aktarılan `for` deyimi.

Katıştırılmış deyimi içinde bir `for` deyimi, bir `break` deyimi ([break deyimi](statements.md#the-break-statement)) denetimi için bitiş noktasını aktarmak için kullanılabilir `for` (Bu nedenle Embedded yineleme bitiş deyimi deyimi) ve bir `continue` deyimi ([CONTINUE deyimi](statements.md#the-continue-statement)) denetim gömülü deyim uç noktasına aktarmak için kullanılabilir (Bu nedenle yürütme *for_iterator* ve başka bir yinelemesini gerçekleştirme `for` deyimi ile başlayarak, *for_condition*).

Katıştırılmış deyimi bir `for` deyimdir aşağıdakilerden biri doğruysa erişilebilir:

*  `for` Deyimi erişilebilir ve hiçbir *for_condition* mevcuttur.
*  `for` Deyimi erişilebilir ve *for_condition* varsa ve sabit değer olmayan `false`.

Bitiş noktasını bir `for` deyimdir aşağıdakilerden en az biri doğruysa erişilebilir:

*  `for` Deyimi içeren bir erişilebilir `break` çıkar deyimi `for` deyimi.
*  `for` Deyimi erişilebilir ve *for_condition* varsa ve sabit değer olmayan `true`.

### <a name="the-foreach-statement"></a>Foreach deyimi

`foreach` Deyimi koleksiyonundaki her öğe için bir katıştırılmış deyim yürütülürken, bir koleksiyonun öğeleri sıralar.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

*Türü* ve *tanımlayıcı* , bir `foreach` declare deyimi ***yineleme değişkeni*** deyiminin. Varsa `var` tanımlayıcısı olarak verilir *local_variable_type*ve adlandırılmış tür `var` olan kapsamda yineleme değişkeni olarak kabul edilir bir ***örtük olarak yazılan yineleme değişkeni***, ve onun türü öğe türü olarak alınır `foreach` aşağıda belirtildiği gibi bir ifade. Yineleme değişkeni salt okunur bir yerel değişken gömülü deyim genişleten bir kapsamla karşılık gelir. Yürütülmesi sırasında bir `foreach` deyimi, yineleme değişkeni, bir yineleme şu anda gerçekleştirildiği için koleksiyon öğeyi temsil eder. Gömülü deyim yineleme değişkeni değiştirme girişiminde bulunursa, bir derleme zamanı hatası oluşur (atama aracılığıyla veya `++` ve `--` işleçleri) veya yineleme değişkeni olarak bir `ref` veya `out` parametresi.

Kısaltma, aşağıdaki `IEnumerable`, `IEnumerator`, `IEnumerable<T>` ve `IEnumerator<T>` ad alanlarındaki karşılık gelen türlerine başvuran `System.Collections` ve `System.Collections.Generic`.

Foreach deyimi derleme zamanı işlenmesini önce belirler ***koleksiyon türü***, ***Numaralandırıcı türü*** ve ***öğe türü*** ifadesi. Bu belirleme gibi çalışır:

*  Varsa türü `X` , *ifade* bir örtük bir başvuru dönüştürme daha sonra bir dizi türü olduğu `X` için `IEnumerable` arabirimi (bu yana `System.Array` bu arabirimi uygulayan). ***Koleksiyon türü*** olduğu `IEnumerable` arabirimi ***Numaralandırıcı türü*** olduğu `IEnumerator` arabirimi ve ***öğe türü*** öğe türü dizi türü `X`.
*  Varsa türü `X` , *ifade* olduğu `dynamic` örtük bir dönüştürme daha sonra *ifade* için `IEnumerable` arabirimi ([örtük dinamik dönüştürmeleri](conversions.md#implicit-dynamic-conversions)). ***Koleksiyon türü*** olduğu `IEnumerable` arabirimi ve ***Numaralandırıcı türü*** olduğu `IEnumerator` arabirimi. Varsa `var` tanımlayıcısı olarak verilir *local_variable_type* sonra ***öğe türü*** olduğu `dynamic`, aksi halde kalır `object`.
*  Aksi takdirde, belirlemek olmadığını türü `X` uygun bir sahip `GetEnumerator` yöntemi:
   * Türde üye araması gerçekleştirmek `X` tanımlayıcıyla `GetEnumerator` ve hiçbir tür bağımsız değişkenleri. Üye araması bir eşleşme üretmez veya bir belirsizlik oluşturur veya bir yöntem grubu olmayan bir eşleşme üretir, numaralandırılabilir bir arabirim için aşağıda açıklandığı gibi kontrol edin. Üye araması bir yöntem grubu veya eşleşme dışında her şeyi oluşturursa uyarı verilmesi önerilir.
   * Elde edilen yöntem grubu ve boş bağımsız değişken listesi kullanılarak aşırı yükleme çözümlemesi gerçekleştirir. Aşırı yükleme çözünürlüğü geçerli hiçbir yöntemleri sonuçlanırsa, bir belirsizlik sonuçları veya yöntemi statik veya ortak değil, onay sıralanabilir arabirim için aşağıda açıklanan şekilde ancak bu, tek bir en iyi yöntem sonuçlanır. Aşırı yükleme çözümlemesi belirsiz ortak örnek yöntemi veya hiçbir uygun yöntemleri dışında her şeyi oluşturursa uyarı verilmesi önerilir.
   * Dönüş türü `E` , `GetEnumerator` yöntemi bir sınıf değil, yapı veya arabirim türü, bir hata oluşturulur ve başka bir adım alınır.
   * Üye araması gerçekleştirilir `E` tanımlayıcıyla `Current` ve hiçbir tür bağımsız değişkenleri. Üye araması eşleşme üretir, sonuç hatadır veya okuma izin veren bir genel örnek özelliğini dışındaki sonucudur, bir hata oluşturulur ve başka bir adım alınır.
   * Üye araması gerçekleştirilir `E` tanımlayıcıyla `MoveNext` ve hiçbir tür bağımsız değişkenleri. Üye araması eşleşme üretir, sonuç hatadır ve sonucu bir yöntem grubu dışında bir şey var mı, bir hata oluşturulur ve başka bir adım alınır.
   * Aşırı yükleme çözünürlüğü yöntem grubu boş bağımsız değişken listesi ile gerçekleştirilir. Aşırı yükleme çözünürlüğü sonuçlarında hiçbir uygun yöntemleri, sonuçları bir belirsizlik veya tek bir en iyi yöntem, ancak bu yöntem sonuçları, statik veya ortak değil ya da dönüş türü değil `bool`, bir hata oluşturulur ve başka bir adım alınır.
   * ***Koleksiyon türü*** olduğu `X`, ***Numaralandırıcı türü*** olduğu `E`ve ***öğe türü*** türü `Current` özelliği.

*  Aksi takdirde, numaralandırılabilir bir arabirim için denetleyin:
   * Eğer tüm türleri arasında `Ti` var olan açık bir dönüştürme `X` için `IEnumerable<Ti>`, benzersiz bir tür `T` şekilde `T` değil `dynamic` ve diğer tüm `Ti` yoktur bir örtük dönüştürme `IEnumerable<T>` için `IEnumerable<Ti>`, ardından ***koleksiyon türü*** arabirim `IEnumerable<T>`, ***Numaralandırıcı türü*** arabirimi`IEnumerator<T>`ve ***öğe türü*** olduğu `T`.
   * Aksi takdirde birden fazla tür ise `T`, ardından bir hata oluşturulur ve başka bir adım alınır.
   * Aksi takdirde örtük bir dönüştürme ise `X` için `System.Collections.IEnumerable` arabirimi, ardından ***koleksiyon türü*** bu arabirim, ***Numaralandırıcı türü*** arabirimi`System.Collections.IEnumerator`ve ***öğe türü*** olduğu `object`.
   * Aksi takdirde bir hata oluşturulur ve başka bir adım alınır.

Yukarıdaki adımları başarılı olursa, kesin bir şekilde bir koleksiyon türü üretmek `C`, numaralandırıcı türü `E` ve öğe türü `T`. Formun foreach deyimi
```csharp
foreach (V v in x) embedded_statement
```
ardından için genişletilir:
```csharp
{
    E e = ((C)(x)).GetEnumerator();
    try {
        while (e.MoveNext()) {
            V v = (V)(T)e.Current;
            embedded_statement
        }
    }
    finally {
        ... // Dispose e
    }
}
```

Değişken `e` görülebilir ya da ifade erişilebilir değil `x` veya katıştırılmış deyim ya da programın başka bir kaynak kodu. Değişken `v` katıştırılmış deyiminde salt okunur. Açık bir dönüştürme ise ([açık dönüştürmeler](conversions.md#explicit-conversions)) öğesinden `T` (öğe türü) için `V` ( *local_variable_type* foreach deyiminde), bir hata oluşturulur ve başka bir adım alınır. Varsa `x` değerine sahip `null`, `System.NullReferenceException` çalışma zamanında oluşturulur.

Yukarıdaki genişletmeyle tutarlı bir davranıştır sürece bir uygulama belirli bir foreach deyimi, örneğin performans nedenleriyle desteklememesinden izin verilir.

Yerleşimini `v` içinde while döngüsü nasıl gerçekleşen herhangi bir anonim işleve göre yakalandıktan için önemli *embedded_statement*.

Örneğin:
```csharp
int[] values = { 7, 9, 13 };
Action f = null;

foreach (var value in values)
{
    if (f == null) f = () => Console.WriteLine("First value: " + value);
}

f();
```
Varsa `v` while dışında bildirildi döngü, tüm yinelemeleri ve sonra değeri arasında paylaşılabilir döngü son değer olacaktır için `13`, hangi çağırma olduğu `f` yazdırır. Bunun yerine, her yineleme kendi değişken olduğundan `v`, bir yakalanan `f` ilk değerini tutmak yineleme devam edecek `7`, olduğu ne yazdırılır. (Not: C# ' ın önceki sürümlerinde bildirilen `v` dışına while döngüsü.)

Gövdesi finally bloğu göre aşağıdaki adımları oluşturulur:

*  Örtük bir dönüştürme ise `E` için `System.IDisposable` ardından arabirimi
   *  Varsa `E` NULL olmayan bir değer türü olan sonra finally yan tümcesi anlam denk genişletilir:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  Aksi takdirde finally yan tümcesi anlam denk genişletilir:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   dışındaki olması durumunda `E` bir değer türü veya tür parametresi bir değer yazın ve ardından atanacağını örneği `e` için `System.IDisposable` kutulama oluşmasına neden olmaz.

*  Aksi takdirde `E` mühürlenmiş bir tür finally yan tümcesi için boş bir blok genişletilir:

   ```csharp
   finally {
   }
   ```

*  Aksi takdirde, finally yan tümcesi için genişletilir:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Yerel değişken `d` görülebilir ya da herhangi bir kullanıcı kodu tarafından erişilebilir değil. Özellikle, kapsamı içeren diğer değişkeni ile çakışmaz finally bloğunda.

Bir sırayı `foreach` bir dizinin öğeleri erişir, aşağıdaki gibidir: Tek boyutlu dizi öğeleri, artan dizin sırasıyla çapraz geçiş için ile dizininden başlayarak `0` ve dizin ile biten `Length - 1`. En sağdaki boyutu dizinleri artan ilk sonra sonraki sol boyut vb. için sol olduğuna gibi çok boyutlu diziler için öğeleri geçirilir.

Aşağıdaki örnek, her değer sırayla öğesi iki boyutlu bir dizi kullanıma yazdırır:
```csharp
using System;

class Test
{
    static void Main() {
        double[,] values = {
            {1.2, 2.3, 3.4, 4.5},
            {5.6, 6.7, 7.8, 8.9}
        };

        foreach (double elementValue in values)
            Console.Write("{0} ", elementValue);

        Console.WriteLine();
    }
}
```
Oluşturulan çıktı şu şekildedir:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

Örnekte
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
türünü `n` olmasını çıkarılan `int`, öğe türü `numbers`.

## <a name="jump-statements"></a>Atlama deyimleri

Atlama deyimleri koşulsuz olarak denetim aktarın.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Atlama deyimi aktarır denetimi konumu adlı ***hedef*** atlama deyimi.

Bir blok içinde bir atlama deyimi gerçekleşir ve bu blok dışındaysa, atlama bildiriminin hedefi olan, atlama deyimi edilir ***çıkmak*** blok. Atlama deyimi denetimi dışında bir blok aktarabilir olsa da, Denetim bloğunun içine hiçbir zaman aktarabilirsiniz.

Atlama deyimleri yürütülmesini müdahale cihazlarınız varlık tarafından karmaşık `try` deyimleri. Örneğin mevcut olmadığında `try` deyimleri, atlama deyimi koşulsuz olarak aktarır denetimi atlama deyimden hedefte. Bu tür müdahale cihazlarınız varsa `try` deyimlerini yürütme daha karmaşık. Atlama deyimi bir veya daha fazla çıkılıyorsa `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi. Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi. Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.

Örnekte
```csharp
using System;

class Test
{
    static void Main() {
        while (true) {
            try {
                try {
                    Console.WriteLine("Before break");
                    break;
                }
                finally {
                    Console.WriteLine("Innermost finally block");
                }
            }
            finally {
                Console.WriteLine("Outermost finally block");
            }
        }
        Console.WriteLine("After break");
    }
}
```
`finally` iki ile ilişkili blokları `try` deyimleri denetimi atlama deyimi hedefe aktarılan önce yürütülür.

Oluşturulan çıktı şu şekildedir:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break deyimi

`break` Deyimi en yakın kapsayan çıkar `switch`, `while`, `do`, `for`, veya `foreach` deyimi.

```antlr
break_statement
    : 'break' ';'
    ;
```

Hedefi bir `break` deyimi en yakın kapsayan uç noktasıdır `switch`, `while`, `do`, `for`, veya `foreach` deyimi. Varsa bir `break` deyimi tarafından içine alınmayan bir `switch`, `while`, `do`, `for`, veya `foreach` deyimi, bir derleme zamanı hatası oluşur.

Zaman birden çok `switch`, `while`, `do`, `for`, veya `foreach` ifadeleri iç içe birbirine içinde bir `break` bildirimi yalnızca en içteki deyimi için geçerlidir. Denetim birden çok iç içe geçme düzeyi arasında aktarmak için bir `goto` deyimi ([goto deyimi](statements.md#the-goto-statement)) kullanılmalıdır.

A `break` olamaz exit deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)). Olduğunda bir `break` ifade oluştuğu yer bir `finally` bloğu hedefinin `break` deyimi içinde aynı olmalıdır `finally` engelle; Aksi takdirde, bir derleme zamanı hatası oluşur.

A `break` deyim şu şekilde gerçekleştirilir:

*  Varsa `break` deyimi, bir veya daha fazla çıkar `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi. Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi. Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.
*  Denetim hedefi için aktarılan `break` deyimi.

Çünkü bir `break` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `break` deyimi erişilebilir hiçbir zaman.

### <a name="the-continue-statement"></a>Continue deyimi

`continue` Deyimi en yakın kapsayan, yeni bir yineleme başlatır `while`, `do`, `for`, veya `foreach` deyimi.

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Hedefi bir `continue` deyimdir katıştırılmış deyimi en yakın kapsayan bitiş noktasını `while`, `do`, `for`, veya `foreach` deyimi. Varsa bir `continue` deyimi tarafından içine alınmayan bir `while`, `do`, `for`, veya `foreach` deyimi, bir derleme zamanı hatası oluşur.

Zaman birden çok `while`, `do`, `for`, veya `foreach` ifadeleri iç içe birbirine içinde bir `continue` bildirimi yalnızca en içteki deyimi için geçerlidir. Denetim birden çok iç içe geçme düzeyi arasında aktarmak için bir `goto` deyimi ([goto deyimi](statements.md#the-goto-statement)) kullanılmalıdır.

A `continue` olamaz exit deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)). Olduğunda bir `continue` ifade oluştuğu yer bir `finally` bloğu hedefinin `continue` deyimi içinde aynı olmalıdır `finally` engelle; Aksi takdirde bir derleme zamanı hatası oluşur.

A `continue` deyim şu şekilde gerçekleştirilir:

*  Varsa `continue` deyimi, bir veya daha fazla çıkar `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi. Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi. Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.
*  Denetim hedefi için aktarılan `continue` deyimi.

Çünkü bir `continue` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `continue` deyimi erişilebilir hiçbir zaman.

### <a name="the-goto-statement"></a>Goto deyimi

`goto` Deyimi denetimi etiket işaretlenmiş bir deyim aktarır.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Hedefi bir `goto` *tanımlayıcı* etiketli deyim belirli bir etikete sahip bir ifadedir. Belirtilen ada sahip etiket geçerli işlev üye mevcut değilse veya `goto` deyimi değil etiketin kapsamında bir derleme zamanı hatası oluşur. Bu kural kullanımına izin verir. bir `goto` denetimi bir iç içe kapsam dışında ancak iç içe bir kapsam aktarmak için deyimi. Örnekte
```csharp
using System;

class Test
{
    static void Main(string[] args) {
        string[,] table = {
            {"Red", "Blue", "Green"},
            {"Monday", "Wednesday", "Friday"}
        };

        foreach (string str in args) {
            int row, colm;
            for (row = 0; row <= 1; ++row)
                for (colm = 0; colm <= 2; ++colm)
                    if (str == table[row,colm])
                         goto done;

            Console.WriteLine("{0} not found", str);
            continue;
    done:
            Console.WriteLine("Found {0} at [{1}][{2}]", str, row, colm);
        }
    }
}
```
bir `goto` deyimi, denetimi bir iç içe kapsam dışına aktarmak için kullanılır.

Hedefi bir `goto case` deyimi listesinde hemen kapsayan bir ifadedir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)), içeren bir `case` verilen sabit değerine sahip etiket. Varsa `goto case` deyimi tarafından içine alınmayan bir `switch` ifadesi varsa *constant_expression* örtük olarak dönüştürülebilir değil ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) değerlendirip türüne en yakın kapsayan `switch` deyimi, veya en yakın kapsayan `switch` deyim içermiyor bir `case` etiket ile verilen sabit değer, bir derleme zamanı hatası oluşur.

Hedefi bir `goto default` deyimi listesinde hemen kapsayan bir ifadedir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)), içeren bir `default` etiketi. Varsa `goto default` deyimi tarafından içine alınmayan bir `switch` deyimi, veya en yakın kapsayan `switch` deyim içermiyor bir `default` etiketi, bir derleme zamanı hatası oluşur.

A `goto` olamaz exit deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)). Olduğunda bir `goto` ifade oluştuğu yer bir `finally` bloğu hedefinin `goto` deyimi içinde aynı olmalıdır `finally` bloğu veya aksi halde bir derleme zamanı hatası oluşur.

A `goto` deyim şu şekilde gerçekleştirilir:

*  Varsa `goto` deyimi, bir veya daha fazla çıkar `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi. Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi. Kadar bu işlem yinelenir `finally` tüm blokları müdahalede bulunan `try` deyimler yürütülür.
*  Denetim hedefi için aktarılan `goto` deyimi.

Çünkü bir `goto` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `goto` deyimi erişilebilir hiçbir zaman.

### <a name="the-return-statement"></a>Return deyimi

`return` Deyimi geçerli çağıran işlevin hangi denetim döndürür `return` ifadesi görünür.

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

A `return` herhangi bir ifade deyimi yalnızca bir değer, diğer bir deyişle, bir yöntem sonuç türü ile bir işlem değil bir işlev üyesi kullanılabilir ([yöntem gövdesini](classes.md#method-body)) `void`, `set` erişimci özelliğin veya Dizin Oluşturucu, `add` ve `remove` erişicilerini geçersiz bir olay, bir örnek oluşturucusu, bir statik oluşturucu veya yıkıcı.

A `return` bir ifade deyimi yalnızca kullanılabilir diğer bir deyişle, bir void olmayan sonuç türü olan bir yöntem bir değer hesaplayan bir işlevi üyesinde `get` erişimci bir özellik veya dizin oluşturucu veya bir kullanıcı tanımlı işleç. Örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) ifadesinin türünden içeren işlev üyenin dönüş türü için mevcut olması gerekir.

Dönüş deyimleri anonim işlev ifadeleri gövdesinde da kullanılabilir ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) ve hangi dönüştürmeler için bu işlevleri mevcut saptanırken katılın.

Bir derleme zamanı hata için bir `return` görünmesini deyimi bir `finally` blok ([try deyimi](statements.md#the-try-statement)).

A `return` deyim şu şekilde gerçekleştirilir:

*  Varsa `return` belirten bir ifade deyimi ifade değerlendirilir ve sonuç değerini içeren işlevin dönüş türüne örtük bir dönüştürme dönüştürülür. Dönüştürmenin sonucu işlev tarafından üretilen sonuç değeri olur.
*  Varsa `return` deyimi bir veya daha fazla içine `try` veya `catch` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi. Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi. Kadar bu işlem yinelenir `finally` tüm kapsayan, bloklarını `try` deyimler yürütülür.
*  İçeren işlev bir zaman uyumsuz işlev değilse, Denetim çağırana içeren işlevin sonucu değerin yanı sıra varsa döndürülür.
*  İçeren işlev bir zaman uyumsuz işlev ise, Denetim, geçerli çağırana döndürülür ve sonuç değeri varsa, dönüş görevde açıklandığı kaydedilir ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)).

Çünkü bir `return` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `return` deyimi erişilebilir hiçbir zaman.

### <a name="the-throw-statement"></a>Throw deyimi

`throw` Deyimi bir özel durum oluşturur.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

A `throw` değeri ifade tarafından değerlendirilen bir ifade deyimi oluşturur. İfade sınıf türünde bir değer belirtmek gerekir `System.Exception`, türetilen bir sınıf türünün `System.Exception` veya olan bir tür parametresi türü `System.Exception` (veya bir alt yapanın) etkili temel sınıfı olarak. İfadenin değerlendirilmesi oluşturursa `null`, `System.NullReferenceException` yerine oluşturulur.

A `throw` herhangi bir ifade deyimi yalnızca kullanılabilir bir `catch` bloğunda bu durumda bu deyimi tarafından işlenmekte olan özel durumu yeniden oluşturur `catch` blok.

Çünkü bir `throw` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `throw` deyimi erişilebilir hiçbir zaman.

Bir özel durum oluştuğunda denetim ilk aktarılır `catch` kapsayan içinde yan tümcesi `try` özel durumu işleyebilecek deyimi. Denetimi uygun özel durum işleyicisine aktarma noktasına oluşturulan özel durumun noktasından gerçekleşmeden işlemi olarak bilinir ***özel durum yayma***. Bir özel durum yayma oluşan aşağıdaki adımları kadar sürekli olarak değerlendirme bir `catch` eşleşen özel durum yan tümcesi bulundu. Bu açıklama ***noktası throw*** başlangıçta özel durum oluşturulur konumdur.

*  Geçerli işlev üyesinde her `try` throw noktası kapsayan bir deyim incelenir. For each deyimi `S`en içteki ile başlayan `try` deyimi ve en dıştaki görmektesiniz `try` deyimi, aşağıdaki adımları değerlendirilir:

   * Varsa `try` bloğu `S` throw noktası barındırır ve S bir veya daha fazla `catch` yan tümceleri, `catch` yan tümceleri içinde belirtilen kurallara göre özel durum için uygun bir işleyici bulmaya sırada görünümünü incelenir Bölüm [try deyimi](statements.md#the-try-statement). Eşleşen bir `catch` yan tümcesi bulunduğu, özel durum yayma denetimi, bloğunu aktararak tamamlanmış `catch` yan tümcesi.

   * Aksi takdirde `try` blok veya `catch` bloğu `S` throw noktası barındırır ve `S` sahip bir `finally` denetim bloğu aktarılır `finally` blok. Varsa `finally` blok başka bir özel durum oluşturursa, geçerli özel durumun işlenmesini sonlandırılır. Aksi takdirde denetim bitiş noktasını ulaştığında `finally` blok, geçerli özel durum işleme devam edilir.

*  Geçerli işlev çağrısını bir özel durum işleyicisi bulunan değil, işlev çağrısını sonlandırılır ve aşağıdakilerden biri gerçekleşir:

   * Geçerli işlev olmayan zaman uyumsuz ise, yukarıdaki adımları için işlev çağıran işlevi üyenin çağrıldığı ifadesine karşılık gelen bir throw noktasıyla yinelenir.

   * Geçerli işlev zaman uyumsuz ve görev döndüren ise özel durum açıklandığı bir hatalı veya iptal edilmiş duruma getirilir dönüş görev kaydedilir [Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces).

   * Geçerli işlev zaman uyumsuz ve void döndüren ise, geçerli iş parçacığının senkronizasyon bağlamı açıklandığı gibi bildirilir [numaralandırılabilir arabirimler](classes.md#enumerable-interfaces).

*  Tüm işlev üyesi çağrılarını geçerli iş parçacığında özel durum işleme sona ererse, iş parçacığı için özel durum işleyici yok olduğunu belirten ardından kendisini sonlandırıldı iş parçacığıdır. Bu tür sonlandırma uygulama tanımlı etkisidir.

## <a name="the-try-statement"></a>Try deyimi

`try` Deyim bloğu yürütülmesi sırasında oluşan özel durumları yakalama için bir mekanizma sağlar. Ayrıca, `try` deyimi, denetimi terk ettiğinde, her zaman yürütülür bir kod bloğunu belirtme olanağı sağlar `try` deyimi.

```antlr
try_statement
    : 'try' block catch_clause+
    | 'try' block finally_clause
    | 'try' block catch_clause+ finally_clause
    ;

catch_clause
    : 'catch' exception_specifier? exception_filter?  block
    ;

exception_specifier
    : '(' type identifier? ')'
    ;

exception_filter
    : 'when' '(' expression ')'
    ;

finally_clause
    : 'finally' block
    ;
```

Üç olası biçimi vardır `try` ifadeleri:

*  A `try` blok izlenen bir veya daha fazla tarafından `catch` engeller.
*  A `try` blok arkasından bir `finally` blok.
*  A `try` blok izlenen bir veya daha fazla tarafından `catch` blokları arkasından bir `finally` blok.

Olduğunda bir `catch` yan tümcesi belirtir bir *exception_specifier*, türü olmalıdır `System.Exception`, türetilen bir tür `System.Exception` ya da olan bir tür parametresi türü `System.Exception` (veya bir alt yapanın), etkin olarak temel sınıf.

Olduğunda bir `catch` yan tümcesi hem belirtir bir *exception_specifier* ile bir *tanımlayıcı*, bir ***özel değişken*** verilen ad ve tür bildirilir. Özel durum değişkeni üzerinden genişleten bir kapsama sahip bir yerel değişken için karşılık gelen `catch` yan tümcesi. Yürütülmesi sırasında *exception_filter* ve *blok*, özel durum değişkeni işlenmekte olan özel durumu temsil eder. Belirli atama onayına denetleme amaçları kesinlikle tüm kapsamında atanan özel değişken olarak kabul edilir.

Sürece bir `catch` yan tümcesi içeren bir özel durum değişken adı, özel durum nesnesi filtredeki erişmek mümkün değildir ve `catch` blok.

A `catch` belirttiğinde yan tümcesi bir *exception_specifier* genel olarak adlandırılan `catch` yan tümcesi.

Bazı programlama dilleri, türetilen bir nesne olarak gösterilebilir değil özel durumları destekleyebilir `System.Exception`, ancak bu tür özel durumların C# kodu tarafından asla oluşturulabilir. Genel `catch` yan tümcesi, bu tür özel durumları yakalamak için kullanılabilir. Bu nedenle, genel `catch` yan tümcesi ise türünü belirten bir anlamsal olarak farklı `System.Exception`bu eski ayrıca diğer dillerdeki özel durumlara catch.

Bir özel durum işleyicisi bulmak için `catch` yan tümceleri sözcük sırayla incelenir. Varsa bir `catch` yan tümcesi, bir tür, ancak hiçbir özel durum filtresi belirtir, bir derleme zamanı hatası için bir sonraki `catch` yan tümcesinde aynı `try` ifade türü bir aynı veya ondan türetilmiş bir tür belirtmek için. Varsa bir `catch` hiçbir türü ve filtre yan tümcesi belirtir, son olmalıdır `catch` söz konusu yan tümcesi `try` deyimi.

İçinde bir `catch` bloğu bir `throw` deyimi ([fırlatma](statements.md#the-throw-statement)) tarafından yakalanan özel durumu yeniden harekete geçirileceğini herhangi bir ifade ile kullanılan `catch` blok. Bir özel durum değişken atamaları yeniden oluşturulan özel durum değiştirmeyin.

Örnekte
```csharp
using System;

class Test
{
    static void F() {
        try {
            G();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in F: " + e.Message);
            e = new Exception("F");
            throw;                // re-throw
        }
    }

    static void G() {
        throw new Exception("G");
    }

    static void Main() {
        try {
            F();
        }
        catch (Exception e) {
            Console.WriteLine("Exception in Main: " + e.Message);
        }
    }
}
```
yöntem `F` bir özel durumu yakalar, bazı tanı bilgilerini konsola yazar, özel durum değişkeni değiştirir ve özel durum yeniden oluşturur. Yeniden oluşturulan özel durum özgün özel durum olduğundan, oluşturulan çıktı şu şekildedir:
```
Exception in F: G
Exception in Main: G
```

Eğer ilk catch bloğu harekete `e` geçerli özel durumun yeniden atma yerine oluşturulan çıktı şu şekilde olacaktır:
```csharp
Exception in F: G
Exception in Main: F
```

Bir derleme zamanı hata için bir `break`, `continue`, veya `goto` kontrolünü dışarı aktarmak için deyimi bir `finally` blok. Olduğunda bir `break`, `continue`, veya `goto` deyimi oluşuyor bir `finally` deyiminin hedefi bloğu içinde aynı olmalıdır `finally` bloğu veya aksi halde bir derleme zamanı hatası oluşur.

Bir derleme zamanı hata için bir `return` deyimini oluşan bir `finally` blok.

A `try` deyim şu şekilde gerçekleştirilir:

*  Denetim aktarılır `try` blok.
*  Zaman ve denetim bitiş noktasını ulaşırsa `try` engelle:
   *  Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.
   *  Denetim için bitiş noktasını aktarılan `try` deyimi.

*  Bir özel durum yayılır, `try` deyimi yürütülürken `try` engelle:
   *  `catch` Yan tümcesi varsa, sırayla incelenir uygun bir özel durum işleyicisi bulunacak görünümünü. Varsa bir `catch` yan tümcesi bir tür belirtmiyor veya özel durum türü veya temel türde bir özel durum türünü belirtir:
      *  Varsa `catch` yan tümcesi bir özel durum değişkenini bildirir, özel durum nesnesi özel durum değişkenine atanır.
      *  Varsa `catch` yan tümcesi bir özel durum filtresi bildirir, filtre değerlendirilir. Değerlendirilirse `false`catch yan tümcesi bir eşleşme değil ve arama herhangi biri ile devam eder. sonraki `catch` yan tümceleri uygun bir işleyici.
      *  Aksi takdirde, `catch` yan tümcesi bir eşleşme olarak kabul edilir ve denetim, eşleşen aktarılır `catch` blok.
      *  Zaman ve denetim bitiş noktasını ulaşırsa `catch` engelle:
         * Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.
         * Denetim için bitiş noktasını aktarılan `try` deyimi.
      *  Bir özel durum yayılır, `try` deyimi yürütülürken `catch` engelle:
         *  Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.
         *  Özel durum sonraki kapsayan yayılır `try` deyimi.
   *  Varsa `try` deyimi yok `catch` yan tümceleri veya hiçbir `catch` eşleşiyorsa bir özel durum:
      *  Varsa `try` deyimi bir `finally` bloğu `finally` bloğu yürütülür.
      *  Özel durum sonraki kapsayan yayılır `try` deyimi.

Deyimleri bir `finally` denetimden çıktığında, blok yürütülen her zaman bir `try` deyimi. Olup yürütülmesi sonucunda normal yürütmenin, sonucunda denetim aktarımı gerçekleşir, böyle bir `break`, `continue`, `goto`, veya `return` deyimi veya bir özel durumun yayılmasını sonucunda `try` deyimi.

Yürütülmesi sırasında bir özel durum oluşturulursa bir `finally` blok ve yakalanan değil aynı içinde finally bloğu, özel durum sonraki kapsayan yayılır `try` deyimi. Başka bir özel durum yayılmaz sürecinde olduysa, o özel durumu kaybolur. Bir özel durum yayma işleminin ele alınan ayrıntılı açıklamasını olarak `throw` deyimi ([fırlatma](statements.md#the-throw-statement)).

`try` Bloğu bir `try` deyimi erişilebilir değilse `try` deyimi erişilebilir.

A `catch` bloğu bir `try` deyimi erişilebilir değilse `try` deyimi erişilebilir.

`finally` Bloğu bir `try` deyimi erişilebilir değilse `try` deyimi erişilebilir.

Bitiş noktasını bir `try` ifadesi aşağıdakilerin her ikisi de doğruysa erişilebilir:

*  Bitiş noktasını `try` blok erişilebilir olduğundan veya en az biri son noktası `catch` blok erişilebilir.
*  Varsa bir `finally` blok mevcutsa, bitiş noktasını `finally` blok erişilebilir.

## <a name="the-checked-and-unchecked-statements"></a>Checked ve unchecked deyimleri

`checked` Ve `unchecked` deyimleri denetlemek için kullanılan ***taşma denetimi bağlamını*** Tamsayı türünde aritmetik işlemler ve dönüştürmeler için.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

`checked` Deyimi neden olan tüm ifadeler *blok* denetlenen bir bağlamda değerlendirilecek ve `unchecked` deyimi neden olan tüm ifadeler *blok* uyumluluğunun değerlendirilebilmesi için bir denetlenmeyen bağlamı.

`checked` Ve `unchecked` ifadeleri tam olarak eşdeğer `checked` ve `unchecked` işleçleri ([checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)) dışında bunlar bloklarındaki ifadeler yerine çalışır .

## <a name="the-lock-statement"></a>Lock deyimi

`lock` Deyimi belirli bir nesne için karşılıklı dışlama kilidini alır, bir deyimi yürütür ve ardından kilidi serbest bırakır.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

İfade bir `lock` deyimi olarak bilinen bir türde bir değer belirtmek gerekir bir *reference_type*. Örtük kutulama dönüştürme ([kutulama dönüştürmeler](conversions.md#boxing-conversions)) ifadesi için hiç olmadığı kadar gerçekleştirilen bir `lock` deyimi ve bu nedenle bu, ifade değeri belirtmek için bir derleme zamanı hatası bir *value_type*.

A `lock` formun deyimi
```csharp
lock (x) ...
```
Burada `x` bir ifade olan bir *reference_type*, tam olarak eşdeğerdir
```csharp
bool __lockWasTaken = false;
try {
    System.Threading.Monitor.Enter(x, ref __lockWasTaken);
    ...
}
finally {
    if (__lockWasTaken) System.Threading.Monitor.Exit(x);
}
```
dışında `x` yalnızca bir kez değerlendirilir.

Karşılıklı dışlama kilidi açık tutulduğu sürece, kod içinde aynı yürütme iş parçacığını yürüten de alabilir ve kilidi serbest bırakır. Ancak, diğer iş parçacıkları kodu yürüten kilidi serbest bırakılıncaya kadar kilitlemeye karşı engellenir.

Kilitleme `System.Type` nesneleri statik veri erişimi eşitlemek için önerilmez. Diğer kod kilitlenmeyle sonuçlanabilir aynı tür kilitleyebilir. Özel statik nesne kilitleyerek statik veri erişimi eşitleme daha iyi bir yaklaşımdır. Örneğin:
```csharp
class Cache
{
    private static readonly object synchronizationObject = new object();

    public static void Add(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }

    public static void Remove(object x) {
        lock (Cache.synchronizationObject) {
            ...
        }
    }
}
```

## <a name="the-using-statement"></a>Using deyimi

`using` Deyimi bir veya daha fazla kaynağı alır, bir deyimi yürütür ve ardından kaynağı siler.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

A ***kaynak*** sınıfı veya uygulayan yapı `System.IDisposable`, adlı tek bir parametresiz yöntemin içeren `Dispose`. Bir kaynağı kullanarak kodu çağırabilir `Dispose` kaynak artık gerekli olduğunu göstermek için. Varsa `Dispose` otomatik çıkarma söz konusu kümelerdeki çöp toplamanın sonunda oluşursa, çağrılmaz.

Varsa biçiminde *resource_acquisition* olan *local_variable_declaration* türü *local_variable_declaration* olmalıdır `dynamic` veya türü örtük olarak dönüştürülebilir `System.IDisposable`. Varsa biçiminde *resource_acquisition* olduğu *ifade* Bu ifade için örtük olarak dönüştürülebilir olmalıdır `System.IDisposable`.

Bildirilen yerel değişken bir *resource_acquisition* salt okunurdur ve Başlatıcı içermelidir. Gömülü deyim, bu yerel değişkenleri değiştirme girişiminde bulunursa, bir derleme zamanı hatası oluşur (atama aracılığıyla veya `++` ve `--` işleçleri) adresi alınamaz veya olarak geçirileceğini `ref` veya `out` parametreleri.

A `using` deyimi, üç bölüme çevrilir: alım, kullanım ve silinecek. Kaynak kullanımını içine örtük olarak bir `try` içeren deyimi bir `finally` yan tümcesi. Bu `finally` yan tümcesi, kaynağın siler. Varsa bir `null` kaynak alınan, ardından hiçbir çağrı `Dispose` yapılmış ve hiçbir özel durum. Kaynak türü ise `dynamic` örtük bir dinamik dönüştürme dinamik olarak dönüştürülür ([dinamik örtük dönüştürmelerin](conversions.md#implicit-dynamic-conversions)) için `IDisposable` dönüştürme olmasını sağlamak için alma işlemi sırasında Kullanım ve elden önce başarılı.

A `using` formun deyimi
```csharp
using (ResourceType resource = expression) statement
```
üç olası genişletmeleri birine karşılık gelir. Zaman `ResourceType` bir NULL olmayan değer türü olan genişletme
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        ((IDisposable)resource).Dispose();
    }
}
```

Aksi takdirde olduğunda `ResourceType` null bir değer türü veya bir başvuru türü dışında olan `dynamic`, genişletilmiş halidir
```csharp
{
    ResourceType resource = expression;
    try {
        statement;
    }
    finally {
        if (resource != null) ((IDisposable)resource).Dispose();
    }
}
```

Aksi takdirde olduğunda `ResourceType` olduğu `dynamic`, genişletilmiş halidir
```csharp
{
    ResourceType resource = expression;
    IDisposable d = (IDisposable)resource;
    try {
        statement;
    }
    finally {
        if (d != null) d.Dispose();
    }
}
```

Ya da genişletme içinde `resource` değişkendir katıştırılmış deyiminde salt okunur ve `d` değişken için katıştırılmış deyimi içinde erişilemez ve görünmez.

Yukarıdaki genişletmeyle tutarlı bir davranıştır sürece bir uygulama bir verilen kullanan-deyimi, örneğin performans nedenleriyle desteklememesinden izin verilir.

A `using` formun deyimi
```csharp
using (expression) statement
```
aynı üç olası uzantılarına sahiptir. Bu durumda `ResourceType` derleme zamanı türü örtülü olarak başvuruluyor `expression`, varsa. Aksi takdirde arabirimi `IDisposable` olarak tek başına kullanılan `ResourceType`. `resource` Değişken için katıştırılmış deyimi içinde erişilemez ve görünmez.

Olduğunda bir *resource_acquisition* biçimini alır bir *local_variable_declaration*, belirli bir türün birden çok kaynak elde mümkündür. A `using` formun deyimi
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
tam olarak bir dizi denk iç içe `using` ifadeleri:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Aşağıdaki örnekte adlı bir dosya oluşturur `log.txt` ve iki satırlık bir metin dosyasına yazar. Örnek, aynı dosya okuma için açar ve içerdiği satırlık metin konsola kopyalar.
```csharp
using System;
using System.IO;

class Test
{
    static void Main() {
        using (TextWriter w = File.CreateText("log.txt")) {
            w.WriteLine("This is line one");
            w.WriteLine("This is line two");
        }

        using (TextReader r = File.OpenText("log.txt")) {
            string s;
            while ((s = r.ReadLine()) != null) {
                Console.WriteLine(s);
            }

        }
    }
}
```

Bu yana `TextWriter` ve `TextReader` sınıfları uygulama `IDisposable` arabirimi, örnek kullanabilir `using` temel alınan dosya düzgün şekilde yazma aşağıdaki kapatıldığından emin olun veya okuma işlemleri için deyimleri.

## <a name="the-yield-statement"></a>Yield deyimi

`yield` Deyimi, bir yineleyici bloğunda kullanılır ([blokları](statements.md#blocks)) Numaralandırıcı nesnesi için bir değer elde etmek üzere ([Numaralandırıcı nesneleri](classes.md#enumerator-objects)) veya numaralandırma nesnesi ([numaralandırılabilir nesneleri](classes.md#enumerable-objects)) bir yineleyicinin veya yinelemenin sonunda, sinyal.

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield` ayrılmış bir sözcük değildir; yalnızca hemen önce kullanıldığında özel anlamı vardır bir `return` veya `break` anahtar sözcüğü. Diğer bağlamlarda `yield` tanımlayıcı olarak kullanılabilir.

Nereye birkaç kısıtlama vardır bir `yield` deyimi görüntülenebilir, aşağıda açıklandığı gibi.

*  Bir derleme zamanı hata için bir `yield` deyimi (ya da formun) dışında görüntülenecek bir *method_body*, *operator_body* veya *accessor_body*
*  Bir derleme zamanı hata için bir `yield` (ya da formun) deyimi anonim bir işlev içinde görüntülenecek.
*  Bir derleme zamanı hata için bir `yield` deyimi (ya da formun) görünmesini `finally` yan tümcesi bir `try` deyimi.
*  Bir derleme zamanı hata için bir `yield return` deyimini her yerde görünür bir `try` herhangi içeren ifade `catch` yan tümceleri.

Aşağıdaki örnek, geçerli ve geçersiz bazı kullanımlarını gösterir `yield` deyimleri.

```csharp
delegate IEnumerable<int> D();

IEnumerator<int> GetEnumerator() {
    try {
        yield return 1;        // Ok
        yield break;           // Ok
    }
    finally {
        yield return 2;        // Error, yield in finally
        yield break;           // Error, yield in finally
    }

    try {
        yield return 3;        // Error, yield return in try...catch
        yield break;           // Ok
    }
    catch {
        yield return 4;        // Error, yield return in try...catch
        yield break;           // Ok
    }

    D d = delegate { 
        yield return 5;        // Error, yield in an anonymous function
    }; 
}

int MyMethod() {
    yield return 1;            // Error, wrong return type for an iterator block
}
```

Örtük bir dönüştürme ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) ifadede türünden bulunmalıdır `yield return` deyimi yield türüne ([Yield türü](classes.md#yield-type)) yineleyici.

A `yield return` deyim şu şekilde gerçekleştirilir:

*  Deyiminde belirtilen ifade değerlendirilir, örtük olarak yield türüne dönüştürülerek ve atanan `Current` Numaralandırıcı nesnesi bir özelliğidir.
*  Yineleyici bloğu yürütülmesini askıya alınır. Varsa `yield return` deyimi, bir veya daha fazla içinde `try` engeller, ilişkili `finally` blokları şu anda yürütülmez.
*  `MoveNext` Yöntemi, numaralandırıcı nesnesi döndürür `true` Numaralandırıcı nesnesi bir sonraki öğeye başarıyla Gelişmiş belirten arayanına için.

Numaralandırıcı nesne yapılan sonraki çağrıda `MoveNext` yöntemi burada son askıya alındığı gelen yineleyici bloğu yürütülmesini sürdürür.

A `yield break` deyim şu şekilde gerçekleştirilir:

*  Varsa `yield break` deyimi bir veya daha fazla içine `try` bloklarla ilişkili `finally` denetim blokları için başlangıçta aktarılır `finally` en içteki bloğunu `try` deyimi. Zaman ve denetim bitiş noktasını ulaşırsa bir `finally` denetim bloğu aktarılır `finally` sonraki kapsayan, blok `try` deyimi. Kadar bu işlem yinelenir `finally` tüm kapsayan, bloklarını `try` deyimler yürütülür.
*  Yineleyici bloğu çağırana döndürülür. Bu `MoveNext` yöntemi veya `Dispose` Numaralandırıcı nesnesi yöntemi.

Çünkü bir `yield break` deyimi koşulsuz aktarımları denetimi başka bir yerde bitiş noktasını bir `yield break` deyimi erişilebilir hiçbir zaman.
