---
ms.openlocfilehash: 94346034a667ad4af26796c0c4bbc96d6ed79aba
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876843"
---
# <a name="statements"></a>Deyimler

C#çeşitli deyimler sağlar. Bu deyimlerin çoğu, C ve C++' de programlanan geliştiricilere tanıdık gelecektir.

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

*Embedded_statement* & gt, diğer deyimler içinde görünen deyimler için kullanılır. Yerine *embedded_statement* kullanımı *deyimi* , bu bağlamlarda bildirim deyimlerinin ve etiketli deyimlerin kullanımını dışlar. Örnek
```csharp
void F(bool b) {
    if (b)
        int i = 44;
}
```
bir `if` deyimin If dalı için bir *bildirim* yerine bir *embedded_statement* gerektirdiğinden derleme zamanı hatası oluşur. Bu koda izin verildiğinde, değişken `i` bildirilebilecek, ancak hiçbir şekilde kullanılamaz. Ancak, bir blokta bildirimi yerleştirilerek `i`örneğin geçerli olduğunu unutmayın.

## <a name="end-points-and-reachability"></a>Bitiş noktaları ve ulaşılabilirlik

Her deyimin bir ***uç noktası***vardır. Sezgisel koşullarda, bir deyimin bitiş noktası, deyimden hemen sonraki konumdur. Bileşik deyimler için yürütme kuralları (katıştırılmış deyimler içeren deyimler), denetimin gömülü bir deyimin bitiş noktasına ulaştığında gerçekleştirilecek eylemi belirtir. Örneğin, denetim bir blok içindeki bir deyimin bitiş noktasına ulaştığında denetim bloğunda bir sonraki ifadeye aktarılır.

Bir ifadeye, yürütme tarafından büyük olasılıkla ulaşılırsa, ifadeye ***ulaşılabilir***denir. Buna karşılık, bir deyimin yürütülemeyecek bir olasılık yoksa, ifadeye ***ulaşılamıyor***denir.

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
ifadesinin yürütülebilmesi olası bir `Console.WriteLine` olasılık olmadığından, öğesinin ikinci çağrısı ulaşılamaz.

Derleyici bir deyimin ulaşılamaz olduğunu belirlerse bir uyarı bildirilir. Bir deyimin ulaşılamaz olması için özellikle bir hata değildir.

Belirli bir deyimin veya bitiş noktasının erişilebilir olup olmadığını anlamak için, derleyici her bir bildirimde tanımlanan erişilebilir kurallara göre Akış Analizi gerçekleştirir. Akış Analizi, deyimlerin davranışını denetleyen sabit ifadelerin ([sabit ifadeler](expressions.md#constant-expressions)) değerlerini hesaba alır, ancak sabit olmayan ifadelerin olası değerleri dikkate alınmaz. Diğer bir deyişle, denetim akışı analizinin amaçları doğrultusunda, belirli bir türün sabit olmayan bir ifadesi söz konusu türde herhangi bir olası değere sahip olduğu kabul edilir.

Örnekte
```csharp
void F() {
    const int i = 1;
    if (i == 2) Console.WriteLine("unreachable");
}
```
`==` işlecin Boolean ifadesi `if` sabit bir ifadedir çünkü her iki işleç işlenenleri de sabitler. Sabit ifade derleme zamanında değerlendirildiğinden, değeri `false` `Console.WriteLine` üreten çağrının ulaşılamaz olduğu kabul edilir. Ancak, `i` yerel bir değişken olarak değiştirilirse
```csharp
void F() {
    int i = 1;
    if (i == 2) Console.WriteLine("reachable");
}
```
`Console.WriteLine` çağrının erişilebilir olduğu kabul edilir, ancak gerçekte hiçbir şekilde yürütülmeyecektir.

Bir işlev üyesinin *bloğu* her zaman ulaşılabilir olarak değerlendirilir. Bir blok içindeki her bir deyimin erişilebilirlik kurallarını kapsamlı bir şekilde değerlendirerek, belirli bir deyimin erişilebilirliği belirlenebilir.

Örnekte
```csharp
void F(int x) {
    Console.WriteLine("start");
    if (x < 0) Console.WriteLine("negative");
}
```
İkincinin `Console.WriteLine` ulaşılabilirlik aşağıdaki gibi belirlenir:

*  Metodun`F` bloğu `Console.WriteLine` erişilebilir olduğundan ilk ifade deyimine erişilebilir.
*  İlk `Console.WriteLine` ifade deyiminin bitiş noktasına ulaşılabilir çünkü bu deyim erişilebilir durumda.
*  `if` İlk`Console.WriteLine` ifade deyiminin bitiş noktası erişilebilir olduğundan deyim erişilebilir.
*  Deyimin Boole `Console.WriteLine` ifadesi `false`sabit değere sahip olmadığından ikinci ifade deyimine erişilebilir. `if`

Bir deyimin bitiş noktasında erişilebilir olması için derleme zamanı hatası olduğu iki durum vardır:

*  `switch` İfade, bir switch bölümünün bir sonraki Switch bölümüne "Fall" olarak geçmesine izin vermediğinden, bir switch bölümünün bildirim listesinin bitiş noktası için bir derleme zamanı hatası, erişilebilir olacaktır. Bu hata oluşursa, genellikle bir `break` deyimin eksik olduğunun bir göstergesidir.
*  Bu, erişilebilen bir değeri hesaplayan bir işlev üyesinin bloğunun bitiş noktası için derleme zamanı hatasıdır. Bu hata oluşursa, genellikle bir `return` deyimin eksik olduğunun göstergesidir.

## <a name="blocks"></a>Bloklar

Bir *blok* , tek bir ifadeye izin verilen bağlamlarda birden çok deyimin yazılmasına izin verir.

```antlr
block
    : '{' statement_list? '}'
    ;
```

Bir *blok* , küme ayraçları içine alınmış bir isteğe bağlı *statement_list* ([ifade listeleri](statements.md#statement-lists)) içerir. Ekstre listesi atlanırsa, blok boş olarak kabul edilir.

Bir blok, bildirim deyimleri içerebilir ([bildirim deyimleri](statements.md#declaration-statements)). Bir blok içinde belirtilen yerel bir değişken veya sabit kapsam, bloğudur.

Bir blok aşağıdaki gibi yürütülür:

*  Blok boşsa denetim, bloğun bitiş noktasına aktarılır.
*  Blok boş değilse denetim, ifade listesine aktarılır. Ve denetimi, ekstre listesinin bitiş noktasına ulaştığında denetim, bloğun bitiş noktasına aktarılır.

Bloğun kendisi erişilebilir olduğunda bir bloğun ifade listesi erişilebilir olur.

Blok boşsa veya ekstre listesinin bitiş noktası ulaşılabilir ise bir bloğun bitiş noktası erişilebilir olur.

Bir veya daha fazla `yield` deyim ([Yield deyimi](statements.md#the-yield-statement)) içeren bir blok, yineleyici bloğu olarak adlandırılır. Yineleyici blokları, işlev üyelerini yineleyiciler ([yineleyiciler](classes.md#iterators)) olarak uygulamak için kullanılır. Yineleyici blokları için bazı ek kısıtlamalar geçerlidir:

*  Bir `return` deyimin Yineleyici bloğunda görünmesi için derleme zamanı hatası (ancak `yield return` deyimlerine izin verilir).
*  Bir yineleyici bloğunun güvenli olmayan bir bağlam ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) içermesi için derleme zamanı hatası. Bir yineleyici bloğu, bildirimi güvenli olmayan bir bağlamda iç içe yerleştirilmiş olsa bile her zaman güvenli bir bağlam tanımlar.

### <a name="statement-lists"></a>Ekstre listeleri

Deyim listesi, sırayla yazılan bir veya daha fazla ***deyimden*** oluşur. İfade listeleri, *blok*s ([bloklar](statements.md#blocks)) ve *switch_block*s ([Switch ifadesinde](statements.md#the-switch-statement)) içinde oluşur.

```antlr
statement_list
    : statement+
    ;
```

Bir ifade listesi, denetim ilk ifadeye aktarılarak yürütülür. Ve denetimi bir deyimin bitiş noktasına ulaştığında, Denetim sonraki deyime aktarılır. Denetim, son deyimin bitiş noktasına ulaştığında denetim, ekstre listesinin bitiş noktasına aktarılır.

Aşağıdakilerden en az biri doğru ise, bir ifade listesindeki deyime erişilebilir:

*  İfade ilk ifadedir ve ekstre listesinin kendisi erişilebilir olur.
*  Önceki deyimin bitiş noktasına ulaşılabilir.
*  İfade etiketli bir ifadedir ve etikete erişilebilir `goto` bir ifade tarafından başvuruluyor.

Listedeki son deyimin bitiş noktası ulaşılabilir ise, bir ekstre listesinin bitiş noktası erişilebilir olur.

## <a name="the-empty-statement"></a>Boş ifade

Bir *empty_statement* hiçbir şey yapmaz.

```antlr
empty_statement
    : ';'
    ;
```

Boş bir ifade, bir deyimin gerekli olduğu bir bağlamda gerçekleştirilecek bir işlem olmadığında kullanılır.

Boş bir deyimin yürütülmesi, denetimi yalnızca deyimin bitiş noktasına aktarır. Bu nedenle, boş deyimin erişilebilir olması halinde boş bir deyimin bitiş noktasına ulaşılabilir.

Boş bir ifade, null gövdesi olan bir `while` ifade yazılırken kullanılabilir:
```csharp
bool ProcessMessage() {...}

void ProcessMessages() {
    while (ProcessMessage())
        ;
}
```

Ayrıca, bir bloğun "`}`" kapanışından hemen önce bir etiketi bildirmek için boş bir ifade kullanılabilir:
```csharp
void F() {
    ...
    if (done) goto exit;
    ...
    exit: ;
}
```

## <a name="labeled-statements"></a>Etiketli deyimler

Bir *labeled_statement* , bir deyimin ön eki olarak belirtilmesine izin verir. Etiketli deyimlerde bloklara izin verilir, ancak gömülü deyimler olarak izin verilmez.

```antlr
labeled_statement
    : identifier ':' statement
    ;
```

Etiketli bir ifade, *tanımlayıcı*tarafından verilen ada sahip bir etiket bildirir. Bir etiketin kapsamı, iç içe geçmiş bloklar dahil olmak üzere, etiketin bildirildiği tüm bloğudur. Çakışan kapsamlara sahip olmak için aynı ada sahip iki etiket için derleme zamanı hatasıdır.

Etiketin kapsamı içinde deyimlerden `goto` ([goto deyimi](statements.md#the-goto-statement)) bir etikete başvurulabilir. Bu, `goto` deyimlerin blokları bloklar içinde ve blok dışında, ancak hiçbir şekilde bloklara aktarabileceği anlamına gelir.

Etiketler kendi bildirim alanına sahiptir ve diğer tanımlayıcılarla karışmaz. Örnek
```csharp
int F(int x) {
    if (x >= 0) goto x;
    x = -x;
    x: return x;
}
```
geçerli olur ve adı `x` hem parametre hem de etiket olarak kullanır.

Etiketli bir deyimin yürütülmesi, etiketi izleyen deyimin yürütülmesi için tam olarak karşılık gelir.

Normal denetim akışı tarafından sağlanmış olan erişilebilirlik 'e ek olarak, etikete erişilebilir `goto` bir bildirimde başvuruluyorsa etiketli ifadeye erişilebilir. Duruma `finally` `try` `finally` Bir ifade bir blok içeren bir `try` içinde ise ve etiketli ifade öğesinin dışındaysa ve bloğun bitiş noktası ulaşılamaz durumdaysa, etiketli ifadeye öğesinden ulaşılamıyor `goto` Bu `goto` ifade.)

## <a name="declaration-statements"></a>Bildirim deyimleri

Bir *declaration_statement* yerel bir değişken veya sabit bildirir. Bildirim deyimlerinin bloklara izin verilir, ancak gömülü deyimler olarak izin verilmez.

```antlr
declaration_statement
    : local_variable_declaration ';'
    | local_constant_declaration ';'
    ;
```

### <a name="local-variable-declarations"></a>Yerel değişken bildirimleri

Bir *local_variable_declaration* , bir veya daha fazla yerel değişken bildirir.

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

Bir *local_variable_declaration* 'ın `var` *local_variable_type* , bildirim tarafından tanıtılan değişkenlerin türünü doğrudan belirtir veya bir tür ile bir izer. Türün ardından, her biri yeni bir değişken tanıtan bir *local_variable_declarator*s listesi gelir. Bir *local_variable_declarator* değişkeni, isteğe bağlı olarak bir "`=`" belirteci ve değişkenin başlangıç değerini veren bir *local_variable_initializer* tarafından kullanılan bir *tanımlayıcıdan* oluşur.

Yerel bir değişken bildirimi bağlamında, tanımlayıcı var bir bağlamsal anahtar sözcük ([anahtar sözcük](lexical-structure.md#keywords)) olarak davranır. *Local_variable_type* `var` olarak belirtildiğinde ve adlandırılmış `var` hiçbir tür kapsamda olduğunda, bildirim ***örtük olarak yazılmış bir yerel değişken bildirimidir***ve türü ilişkili Başlatıcı türünden çıkarsandır ifadesini. Örtük olarak yazılan yerel değişken bildirimleri aşağıdaki kısıtlamalara tabidir:

*  *Local_variable_declaration* birden çok *local_variable_declarator*s içeremez.
*  *Local_variable_declarator* bir *local_variable_initializer*içermelidir.
*  *Local_variable_initializer* bir *ifade*olmalıdır.
*  Başlatıcı *ifadesi* bir derleme zamanı türüne sahip olmalıdır.
*  Başlatıcı *ifadesi* , belirtilen değişkenin kendine başvuramaz

Aşağıda, türü kesin olarak belirlenmiş geçersiz yerel değişken bildirimlerinin örnekleri verilmiştir:

```csharp
var x;               // Error, no initializer to infer type from
var y = {1, 2, 3};   // Error, array initializer not permitted
var z = null;        // Error, null does not have a type
var u = x => x + 1;  // Error, anonymous functions do not have a type
var v = v++;         // Error, initializer cannot refer to variable itself
```

Yerel bir değişkenin değeri, bir *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılarak bir ifadede alınır ve yerel bir değişkenin değeri *atama* ([atama işleçleri](expressions.md#assignment-operators)) kullanılarak değiştirilir. Yerel değişken, değerinin alındığı her konumda kesinlikle atanmalıdır ([kesin atama](variables.md#definite-assignment)).

Bir *local_variable_declaration* içinde belirtilen yerel değişkenin kapsamı, bildirimin gerçekleştiği bloğudur. Yerel değişkenin *local_variable_declarator* önündeki bir metinsel konumdaki yerel değişkene başvurabileceğiniz bir hatadır. Yerel bir değişken kapsamında, aynı ada sahip başka bir yerel değişken veya sabit bildirmek için derleme zamanı hatası vardır.

Birden çok değişken bildiren bir yerel değişken bildirimi, aynı türe sahip tek değişkenlerin birden çok bildirimi ile eşdeğerdir. Ayrıca, yerel bir değişken bildirimindeki bir değişken başlatıcısı, bildirimden hemen sonra eklenen bir atama bildirimine karşılık gelir.

Örnek
```csharp
void F() {
    int x = 1, y, z = x * 2;
}
```
tam olarak öğesine karşılık gelir
```csharp
void F() {
    int x; x = 1;
    int y;
    int z; z = x * 2;
}
```

Örtük olarak yazılmış bir yerel değişken bildiriminde, bildirildiği yerel değişkenin türü, değişkeni başlatmak için kullanılan ifadenin türüyle aynı olacak şekilde alınır. Örneğin:
```csharp
var i = 5;
var s = "Hello";
var d = 1.0;
var numbers = new int[] {1, 2, 3};
var orders = new Dictionary<int,Order>();
```

Yukarıdaki örtük olarak yazılan yerel değişken bildirimleri aşağıdaki açıkça belirlenmiş bildirimlerle tam olarak eşdeğerdir:
```csharp
int i = 5;
string s = "Hello";
double d = 1.0;
int[] numbers = new int[] {1, 2, 3};
Dictionary<int,Order> orders = new Dictionary<int,Order>();
```

### <a name="local-constant-declarations"></a>Yerel sabit bildirimler

Bir *local_constant_declaration* bir veya daha fazla yerel sabit bildirir.

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

Bir *local_constant_declaration* *türü* , bildirim tarafından tanıtılan sabitlerin türünü belirtir. Türün ardından, her biri yeni bir sabit sunan bir *constant_declarator*s listesi bulunur. Bir *constant_declarator* , sabit değerli bir *tanımlayıcıdan* , ardından bir "`=`" belirteci ve ardından sabit değeri veren bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) ile oluşur.

Yerel bir sabit bildirimin *türü* ve *constant_expression* , sabit üye bildirimiyle ([sabitler](classes.md#constants)) aynı kurallara uymalıdır.

Yerel bir sabitin değeri, *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılarak bir ifadede elde edilir.

Yerel bir sabitin kapsamı, bildirimin gerçekleştiği bloğudur. *Constant_declarator*önünde bir metinsel konumda yerel bir Sabitte başvurmak hatadır. Yerel bir sabit kapsamında, aynı ada sahip başka bir yerel değişken veya sabit bildirmek için derleme zamanı hatası vardır.

Birden çok sabiti bildiren yerel bir sabit bildirim, aynı türe sahip tek sabitlerin birden çok bildirimine eşdeğerdir.

## <a name="expression-statements"></a>İfade deyimleri

Bir *expression_statement* verilen bir ifadeyi değerlendirir. Varsa, ifade tarafından hesaplanan değer atılır.

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

Tüm ifadelere deyim olarak izin verilmez. Özellikle, `x + y` ve `x == 1` gibi yalnızca bir değeri hesaplama (atılacak) gibi ifadeler, deyimler olarak izin verilmez.

Bir *expression_statement* öğesinin yürütülmesi içerilen ifadeyi değerlendirir ve sonra denetimi *expression_statement*bitiş noktasına aktarır. Bu *expression_statement* ulaşılabilir olduğunda bir *expression_statement* bitiş noktasına ulaşılabilir.

## <a name="selection-statements"></a>Seçim deyimleri

Seçim deyimleri, bazı deyimlerin değerine göre yürütme için bir dizi olası deyimden birini seçer.

```antlr
selection_statement
    : if_statement
    | switch_statement
    ;
```

### <a name="the-if-statement"></a>If ekstresi

Deyimi `if` , bir Boolean ifadesinin değerine göre yürütme için bir deyim seçer.

```antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Bir `else` bölüm, söz dizimi tarafından izin verilen, `if` en yakın olan sözcüme ile ilişkilidir. Bu nedenle, `if` formun bir ifadesidir
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

Bir `if` ifade aşağıdaki gibi yürütülür:

*  *Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.
*  Boolean ifadesi `true`uygunsa denetim ilk eklenmiş ifadeye aktarılır. Ve denetimi bu deyimin bitiş noktasına ulaştığında, Denetim `if` deyimin bitiş noktasına aktarılır.
*  Boolean ifadesi `false` varsa ve bir `else` bölüm varsa, denetim ikinci katıştırılmış deyime aktarılır. Ve denetimi bu deyimin bitiş noktasına ulaştığında, Denetim `if` deyimin bitiş noktasına aktarılır.
*  Boolean ifadesi `false` varsa ve bir `else` bölüm yoksa, Denetim `if` deyimin bitiş noktasına aktarılır.

Deyim erişilebilir olduğunda ve Boolean ifadesinde `if` sabit değer `false`yoksa, bir `if` deyimin ilk ekli deyimine erişilebilir.

Varsa, bir `if` deyimin ikinci gömülü deyimi, `if` deyimi erişilebilir olduğunda ve Boolean ifadesinde sabit değer `true`yoksa erişilebilir olur.

Bir `if` deyimin bitiş noktası, katıştırılmış deyimlerinden en az birinin bitiş noktası ulaşılabilir ise erişilebilir olur. Ayrıca, `if` deyim erişilebilir olduğunda ve Boole ifadesinde `if` sabit değer `true`yoksa `else` , Bölüm içermeyen bir deyimin bitiş noktasına ulaşılamaz.

### <a name="the-switch-statement"></a>Switch deyimleri

Switch deyimi, anahtar ifadesinin değerine karşılık gelen ilişkili bir anahtar etiketine sahip bir deyim listesini yürütmeye yönelik seçer.

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

Bir *switch_statement* , öğesinden sonra parantez `switch`içine alınmış bir ifade (switch ifadesi olarak adlandırılır) ve ardından bir *switch_block*olan anahtar sözcüğünden oluşur. *Switch_block* , küme ayraçları içine alınmış sıfır veya daha fazla *switch_section*s içerir. Her *switch_section* , bir veya daha fazla *switch_label*s öğesinden sonra bir *statement_list* ([ifade listesi](statements.md#statement-lists)) içerir.

Bir`switch` deyimin ***yöneten türü*** , switch ifadesi tarafından oluşturulur.

*  Anahtar `sbyte`ifadesinin türü ,`uint` ,,`char`,,, ,,`ulong`, ,`string`veya `byte` `short` `ushort` `long` `int` `bool`  *enum_type*ya da bu türlerden birine karşılık gelen null yapılabilir türde ise, bu `switch` bildirimin yöneten türüdür.
*  Aksi halde, tek bir Kullanıcı tanımlı örtük dönüştürme ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)), anahtar ifadesinin türünden aşağıdaki olası bir tür düzenleyen türlerden `sbyte`birine varolmalıdır:, `byte`,, `ushort` `short` `int` ,,`ulong`, ,,`char`, veya,butürlerdenbirinekarşılıkgelennullyapılabilirbirtür`string`. `uint` `long`
*  Aksi takdirde, böyle bir örtük dönüştürme yoksa veya birden fazla örtük dönüştürme varsa, bir derleme zamanı hatası oluşur.

Her `case` bir etiketin sabit ifadesi örtük olarak dönüştürülebilir bir değeri ([örtük dönüştürmeler](conversions.md#implicit-conversions)) `switch` deyimin yöneten türüne göre belirtmelidir. `case` Aynı`switch` deyimdeki iki veya daha fazla etiket aynı sabit değeri belirtmediğinde bir derleme zamanı hatası oluşur.

Switch ifadesinde en fazla bir `default` etiket olabilir.

Bir `switch` ifade aşağıdaki gibi yürütülür:

*  Anahtar ifadesi değerlendirilir ve yöneten türe dönüştürülür.
*  Aynı `case` `case` deyimdeki bir etikette belirtilen sabitlerden biri, switch ifadesinin değerine eşitse, denetim eşleşen etiketten sonraki deyim listesine aktarılır. `switch`
*  Aynı `case` `default` `default` deyimde Etiketler içinde belirtilen sabitlerden hiçbiri anahtar ifadesinin değerine eşit değilse ve bir etiket mevcutsa, Denetim aşağıdaki ifadeyi izleyen deyim listesine aktarılır. `switch` etiketin.
*  Aynı `case` `default` `switch` deyimdeki etiketlerde belirtilen sabitlerin hiçbiri anahtar ifadesinin değerine eşit değilse ve bir etiket yoksa, denetim deyimin bitiş noktasına aktarılır. `switch`

Bir anahtar bölümünün ekstre listesinin bitiş noktasına ulaşılabiliyorsa, bir derleme zamanı hatası oluşur. Bu, "hiçbir düşüş olmadan" kuralı olarak bilinir. Örnek
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
, Switch bölümünün erişilebilir bir uç noktasına sahip olmadığı için geçerlidir. C ve C++' nin aksine, bir switch bölümünün yürütülmesi bir sonraki anahtar bölümüne "dönemez" ve örnek
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
derleme zamanı hatasına neden olur. Bir switch bölümünün yürütülmesi başka bir anahtar bölümünün yürütülmesi tarafından izlenmesinin ardından açık `goto case` veya `goto default` bir deyimin kullanılması gerekir:
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

Bir *switch_section*içinde birden çok etikete izin verilir. Örnek
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
geçerli. Bu örnek, Etiketler `case 2:` ve `default:` aynı *switch_section*bir parçası olduğundan "hiçbir düşüş olmadan" kuralını ihlal etmez.

"Fall yok" kuralı, C 'de oluşan ve C++ `break` deyimler yanlışlıkla atlandığında ortak bir hata sınıfını engeller. Ayrıca, bu kural nedeniyle, bir `switch` deyimin anahtar bölümleri deyimin davranışını etkilemeden rastgele bir şekilde yeniden düzenlenebilir. Örneğin, yukarıdaki `switch` deyimin bölümleri, deyimin davranışı etkilenmeden ters alınabilir:
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

Bir switch bölümünün ifade listesi genellikle bir `break`, `goto case`veya `goto default` ifadesinde sona erer, ancak bildirim listesinin ulaşılamaz bitiş noktasını işleyen herhangi bir yapı için izin verilir. Örneğin, Boolean ifadesinin `while` `true` denetimindeki bir deyim, uç noktasına hiçbir şekilde ulaşamadığı bilinmektedir. Benzer şekilde, `throw` bir `return` veya deyimleri her zaman denetimi başka bir yere aktarır ve bitiş noktasına hiçbir zaman ulaşmaz. Bu nedenle, aşağıdaki örnek geçerlidir:
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

Bir `switch` deyimin yöneten türü tür `string`olabilir. Örneğin:
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

Dize eşitlik işleçleri ([dize eşitlik işleçleri](expressions.md#string-equality-operators)) gibi, `switch` deyim büyük/küçük harfe duyarlıdır ve yalnızca anahtar ifadesi dizesi bir `case` etiket sabiti ile tam olarak eşleşiyorsa verilen bir anahtar bölümünü yürütür.

Bir `switch` deyiminyöneten`null` türü olduğunda ,değerebirCaseetiketisabitiolarakizinverilir.`string`

Bir *switch_block* *statement_list*s, bildirim deyimlerini ([bildirim deyimleri](statements.md#declaration-statements)) içerebilir. Anahtar bloğunda belirtilen yerel bir değişken veya sabit kapsam, anahtar bloğudur.

Belirli bir switch bölümünün ifade listesi, `switch` ifadeye ulaşılabiliyorsa ve aşağıdakilerden en az biri geçerliyse erişilebilir olur:

*  Switch ifadesi sabit olmayan bir değer.
*  Switch ifadesi, Switch bölümündeki bir `case` etiketle eşleşen sabit bir değerdir.
*  Switch ifadesi herhangi `case` bir etiketle eşleşmeyen sabit bir değerdir ve Switch bölümü `default` etiketi içerir.
*  Switch bölümünün anahtar etiketine erişilebilir `goto case` veya `goto default` bildirim tarafından başvuruluyor.

Aşağıdakilerden en az biri doğru `switch` ise, bir deyimin bitiş noktasına ulaşılabilir:

*  `switch` İfade `break` , ifadesiyle`switch` çıkış yapan erişilebilir bir ifade içeriyor.
*  Deyim erişilebilir, anahtar ifadesi sabit olmayan bir değer `default` ve etiket yok. `switch`
*  Deyim erişilebilir, anahtar ifadesi hiçbir `case` etiketle eşleşmeyen bir sabit değerdir ve hiçbir `default` etiket yoktur. `switch`

## <a name="iteration-statements"></a>Yineleme deyimleri

Yineleme deyimleri, art arda gömülü bir deyimi yürütür.

```antlr
iteration_statement
    : while_statement
    | do_statement
    | for_statement
    | foreach_statement
    ;
```

### <a name="the-while-statement"></a>While ekstresi

İfade `while` , gömülü bir ifadeyi sıfır veya daha fazla kez koşullu olarak yürütür.

```antlr
while_statement
    : 'while' '(' boolean_expression ')' embedded_statement
    ;
```

Bir `while` ifade aşağıdaki gibi yürütülür:

*  *Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir.
*  Boolean ifadesi `true`uygunsa denetim katıştırılmış deyime aktarılır. Ve denetimi katıştırılmış deyimin bitiş noktasına ulaşırsa (muhtemelen bir `continue` deyimin yürütülmesinden), Denetim `while` deyimin başlangıcına aktarılır.
*  Boolean ifadesi `false`uygunsa denetim `while` deyimin bitiş noktasına aktarılır.

Bir `while` deyimin gömülü ifadesinde `break` , `while` denetimi deyimin bitiş noktasına (Bu nedenle katıştırılmış deyimin sonuna kadar)[](statements.md#the-break-statement) aktarmakiçinbirifade(breakdeyimleri)kullanılabilir.`continue` deyimin ([devam eden bildiri](statements.md#the-continue-statement)), denetimi katıştırılmış deyimin bitiş noktasına aktarmak için kullanılabilir (Bu nedenle `while` deyimin başka bir yinelemesi gerçekleştiriliyor).

Deyim erişilebilir olduğunda ve Boolean `while` ifadesinde sabit değer `false`yoksa, bir deyimin gömülü deyimine erişilebilir. `while`

Aşağıdakilerden en az biri doğru `while` ise, bir deyimin bitiş noktasına ulaşılabilir:

*  `while` İfade `break` , ifadesiyle`while` çıkış yapan erişilebilir bir ifade içeriyor.
*  Deyimine erişilebilir ve Boole ifadesi sabit değere `true`sahip değil. `while`

### <a name="the-do-statement"></a>Do ekstresi

İfade `do` , gömülü bir ifadeyi bir veya daha fazla kez koşullu olarak yürütür.

```antlr
do_statement
    : 'do' embedded_statement 'while' '(' boolean_expression ')' ';'
    ;
```

Bir `do` ifade aşağıdaki gibi yürütülür:

*  Denetim katıştırılmış deyime aktarılır.
*  Ve denetimi katıştırılmış deyimin bitiş noktasına ulaşırsa (muhtemelen bir `continue` deyimin yürütülmesinden), *Boolean_expression* ([Boolean ifadeler](expressions.md#boolean-expressions)) değerlendirilir. Boolean ifadesi `true`uygunsa denetim `do` deyimin başlangıcına aktarılır. Aksi takdirde denetim, `do` deyimin bitiş noktasına aktarılır.

Bir `do` deyimin gömülü ifadesinde `break` , `do` denetimi deyimin bitiş noktasına (Bu nedenle katıştırılmış deyimin sonuna kadar)[](statements.md#the-break-statement) aktarmakiçinbirifade(breakdeyimleri)kullanılabilir.`continue` bildirim ([Continue bildirisi](statements.md#the-continue-statement)), denetimi katıştırılmış deyimin bitiş noktasına aktarmak için kullanılabilir.

Deyimden ulaşılabilir olduğunda bir `do` deyimin gömülü ifadesine ulaşılabilir. `do`

Aşağıdakilerden en az biri doğru `do` ise, bir deyimin bitiş noktasına ulaşılabilir:

*  `do` İfade `break` , ifadesiyle`do` çıkış yapan erişilebilir bir ifade içeriyor.
*  Katıştırılmış deyimin bitiş noktasına ulaşılabilir ve Boole ifadesi sabit değere `true`sahip değil.

### <a name="the-for-statement"></a>For deyimleri

`for` Deyimi, bir dizi başlatma ifadesi değerlendirir ve sonra bir koşul true olduğunda, gömülü bir deyimi tekrar tekrar yürütür ve bir dizi yineleme ifadesini değerlendirir.

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

*For_initializer*, varsa, virgülle ayrılmış bir *local_variable_declaration* ([yerel değişken bildirimleri](statements.md#local-variable-declarations)) ya da bir *statement_expression*s ([ifade deyimleri](statements.md#expression-statements)) listesinden oluşur. *For_initializer* tarafından belirtilen bir yerel değişkenin kapsamı, değişken için *local_variable_declarator* başlar ve ekli deyimin sonuna genişletilir. Kapsam *for_condition* ve *for_iterator*içerir.

*For_condition*, varsa, bir *Boolean_expression* ([Boolean ifadeleri](expressions.md#boolean-expressions)) olmalıdır.

Varsa *for_iterator*, virgülle ayrılmış bir *Statement_expression*s ([Expression deyimleri](statements.md#expression-statements)) listesinden oluşur.

A for deyimleri aşağıdaki gibi yürütülür:

*  Bir *for_initializer* varsa, değişken başlatıcıları veya deyim ifadeleri yazıldığı sırada yürütülür. Bu adım yalnızca bir kez gerçekleştirilir.
*  Bir *for_condition* varsa, değerlendirilir.
*  *For_condition* yoksa veya değerlendirme `true`uygunsa denetim katıştırılmış deyime aktarılır. Ve denetimi katıştırılmış deyimin bitiş noktasına ulaştığında (büyük olasılıkla bir `continue` deyimin yürütülmesinden), varsa, *for_iterator*ifadeleri sırayla değerlendirilir ve ardından, ile başlayan başka bir yineleme yapılır. Yukarıdaki adımda *for_condition* değerlendirmesi.
*  *For_condition* varsa ve değerlendirme `false`uygunsa denetim, `for` deyimin bitiş noktasına aktarılır.

Bir `for` deyimin gömülü ifadesinde `break` , `for` denetimi deyimin bitiş noktasına (Bu nedenle katıştırılmış deyimin sonuna kadar)[](statements.md#the-break-statement) aktarmakiçinbirifade(breakdeyimleri)kullanılabilir.`continue` deyimin ([Continue deyimleri](statements.md#the-continue-statement)), denetimi katıştırılmış deyimin bitiş noktasına aktarmak için kullanılabilir (Bu nedenle, *for_iterator* öğesini yürüten ve `for` *for_condition*).

Aşağıdakilerden biri doğruysa, bir `for` deyimin gömülü ifadesine ulaşılabilir:

*  İfadeye ulaşılamıyor ve hiçbir for_condition yok. `for`
*  Deyime erişilebilir ve bir *for_condition* var ve sabit değere `false`sahip değil. `for`

Aşağıdakilerden en az biri doğru `for` ise, bir deyimin bitiş noktasına ulaşılabilir:

*  `for` İfade `break` , ifadesiyle`for` çıkış yapan erişilebilir bir ifade içeriyor.
*  Deyime erişilebilir ve bir *for_condition* var ve sabit değere `true`sahip değil. `for`

### <a name="the-foreach-statement"></a>Foreach ifadesi

`foreach` İfade, koleksiyonun öğelerinin her öğesi için gömülü bir ifade yürüten bir koleksiyonun öğelerini numaralandırır.

```antlr
foreach_statement
    : 'foreach' '(' local_variable_type identifier 'in' expression ')' embedded_statement
    ;
```

`foreach` Deyimin *türü* ve *tanımlayıcısı* deyimin ***yineleme değişkenini*** bildirir. Tanımlayıcı, *local_variable_type*olarak verilirse ve adında `var` bir tür varsa, yineleme değişkeni ***örtük olarak yazılmış bir yineleme değişkeni***olarak adlandırılır ve türü, öğe türü olan `var` `foreach` ifadesini aşağıda belirtildiği gibi. Yineleme değişkeni, katıştırılmış deyimin kapsamını genişleten bir salt okunurdur yerel değişkene karşılık gelir. Bir `foreach` deyimin yürütülmesi sırasında yineleme değişkeni, bir yinelemenin Şu anda gerçekleştirildiği koleksiyon öğesini temsil eder. Katıştırılmış deyimin yineleme değişkenini `++` (atama ya da ve `--` işleçler aracılığıyla) değiştirmeye veya yineleme değişkenini bir `ref` veya `out` parametresi olarak geçirmeye çalışırsa bir derleme zamanı hatası oluşur.

`IEnumerable`Aşağıda, kısaltma `IEnumerator` `IEnumerator<T>` `System.Collections` için,, vead`System.Collections.Generic`alanları ve içindeki ilgili türlere bakın. `IEnumerable<T>`

Bir foreach deyiminin derleme zamanı işleme öncelikle deyimin ***koleksiyon türünü***, ***Numaralandırıcı türünü*** ve ***öğe türünü*** belirler. Bu belirleme işlemi aşağıdaki gibi gerçekleştirilir:

*  `X` *İfade* türü bir dizi türü ise, `X` `IEnumerable` arabirime bir örtük başvuru dönüştürmesi vardır (Bu arabirimden bu yana `System.Array` ). ***Koleksiyon türü*** `IEnumerable` arabirimdir, ***Numaralandırıcı türü*** `IEnumerator` arayüztür ve ***öğe*** türü dizi türünün `X`öğe türüdür.
*  `X` `IEnumerable` *İfadenin* [](conversions.md#implicit-dynamic-conversions)türü daha sonra deyimden arabirime (örtük dinamik dönüştürmeler) örtük bir dönüşüm varsa. `dynamic` ***Koleksiyon türü*** `IEnumerable` arayüztür ve `IEnumerator` ***Numaralandırıcı türü arayüztür*** . `dynamic` `object` Tanımlayıcı local_variable_type olarak verilirse öğe türü olur, aksi durumda. `var`
*  Aksi takdirde, türün `X` uygun `GetEnumerator` bir yönteme sahip olup olmadığını saptayın:
   * `X` Tanımlayıcı`GetEnumerator` ve tür bağımsız değişkeni olmayan tür üzerinde üye araması gerçekleştirin. Üye arama bir eşleşme oluşturmuyorsa veya bir belirsizlik üretir ya da bir yöntem grubu olmayan bir eşleşme üretirse, aşağıda açıklandığı gibi sıralanabilir bir arabirim olup olmadığını kontrol edin. Üye arama yöntemi bir yöntem grubu veya eşleşme dışında bir şey üretirse bir uyarı verilmesi önerilir.
   * Elde edilen yöntem grubunu ve boş bir bağımsız değişken listesini kullanarak aşırı yükleme çözümlemesi gerçekleştirin. Aşırı yükleme çözümlemesi uygulanabilir bir yöntem olmadan sonuçlanırsa, belirsizliğe neden olur veya tek bir en iyi yöntemle sonuçlanır, ancak bu yöntem statik veya genel değil, aşağıda açıklandığı gibi sıralanabilir bir arabirim olup olmadığını kontrol edin. Aşırı yükleme çözümlemesi, belirsiz bir ortak örnek yöntemi veya uygulanabilir Yöntemler dışında bir şey üretirse bir uyarı verilmesi önerilir.
   * `GetEnumerator` Yöntemin dönüş türü `E` bir sınıf, yapı veya arabirim türü değilse, bir hata oluşturulur ve başka bir adım alınmaz.
   * Üye arama, tanımlayıcısı `E` `Current` ve tür bağımsız değişkenleri olmadan üzerinde gerçekleştirilir. Üye arama hiçbir eşleşme oluşturmazsa, sonuç bir hatadır veya sonuç, okumaya izin veren bir ortak örnek özelliği dışında bir hata oluşturulur ve başka hiçbir adım alınmaz.
   * Üye arama, tanımlayıcısı `E` `MoveNext` ve tür bağımsız değişkenleri olmadan üzerinde gerçekleştirilir. Üye arama hiçbir eşleşme oluşturmazsa, sonuç bir hatadır veya sonuç bir yöntem grubu dışında bir hata üretiyorsa, başka bir adım alınmaz.
   * Aşırı yükleme çözümlemesi, metot grubunda boş bir bağımsız değişken listesiyle gerçekleştirilir. Aşırı yükleme çözümlemesi uygulanabilir bir yöntem olmadan sonuçlanırsa, bir belirsizliğe neden olur ya da tek bir en iyi yöntem ile sonuçlanır, ancak bu yöntem statik veya genel değildir veya dönüş türü değildir `bool`, bir hata üretilir ve başka bir adım alınmaz.
   * ***Koleksiyon türü*** `X`, ***Numaralandırıcı türüdür*** `E` ***ve öğe türü*** özelliğintürüdür.`Current`

*  Aksi takdirde, sıralanabilir bir arabirim olup olmadığını kontrol edin:
   * `Ti` `dynamic` `T` `Ti`'Den örtükbirdönüştürmeolantümtürlerarasında,olmayanbirtürüvardırvebununiçin`T` `X` `IEnumerable<Ti>` `IEnumerable<T>` öğesinden öğesine `IEnumerator<T>` `IEnumerable<T>` örtük dönüştürme, sonra koleksiyon türü arabirimdir, Numaralandırıcı türü arayüztür ve öğe türü `IEnumerable<Ti>` `T`.
   * Aksi takdirde, böyle birden çok tür `T`varsa, bir hata oluşturulur ve başka bir adım alınmaz.
   * Aksi `X` halde, `System.Collections.IEnumerable` arabirimine örtük bir dönüştürme varsa, ***koleksiyon türü*** bu arabirimdir, ***Numaralandırıcı türü arayüztür*** `System.Collections.IEnumerator`ve ***öğe türü*** `object`.
   * Aksi halde, bir hata oluşturulur ve başka bir adım alınmaz.

Yukarıdaki adımlar başarılı olursa, kesin bir şekilde koleksiyon türü `C`, Numaralandırıcı türü `E` ve öğe türü `T`üretir. Formun foreach ifadesi
```csharp
foreach (V v in x) embedded_statement
```
daha sonra şu şekilde genişletilir:
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

Değişken `e` , ifade `x` veya katıştırılmış deyim ya da programın başka bir kaynak kodu için görünür veya erişilebilir değil. Değişken `v` , gömülü ifadede salt okunurdur. ( `T` *Öğe türü* )[](conversions.md#explicit-conversions) öğesinden(foreachdeyimindekilocal_variable_type)açıkbirdönüştürme(açıkdönüştürmeler)yoksa,birhataoluşturulurvebaşka`V` bir adım alınmaz. `System.NullReferenceException` Değeri `x` varsa`null`, çalışma zamanında bir oluşturulur.

Davranışın yukarıdaki genişlemeyle tutarlı olduğu sürece, bir uygulamanın belirli bir foreach-deyimin farklı şekilde uygulanmasına izin verilir; Örneğin performans nedenleriyle.

While döngüsünün `v` içine yerleştirilmesi, *embedded_statement*içinde oluşan herhangi bir anonim işlev tarafından nasıl yakalandığından önemlidir.

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
While döngüsünün dışında bildirilirse, bu, tüm yinelemeler arasında paylaşılır ve for döngüsünden sonraki değeri son `13`değer olacaktır, bu, ' nin `f` çağrılma değeridir. `v` Bunun yerine, her yineleme kendi değişkenine `v`sahip olduğundan, ilk yinelemede tarafından `f` yakalanan tek şey değeri `7`tutmaya devam eder, bu, yazdırılacak şeydir. (Note: while döngüsünün dışında C# bildirildiği `v` önceki sürümleri.)

Finally bloğunun gövdesi aşağıdaki adımlara göre oluşturulur:

*  `System.IDisposable` Arabiriminden örtülü bir dönüşüm `E` varsa,
   *  `E` Null yapılamayan bir değer türü ise finally yan tümcesi, ' nin anlamsal eşdeğerine genişletilir:

      ```csharp
      finally {
          ((System.IDisposable)e).Dispose();
      }
      ```

   *  Aksi halde finally yan tümcesi anlam eşdeğerine genişletilir:

      ```csharp
      finally {
          if (e != null) ((System.IDisposable)e).Dispose();
      }
      ```

   bir değer türü veya bir değer türüne örneklenmiş tür parametresi olması dışında, ' `System.IDisposable` ın `e` ' a dönüştürme işlemi, kutulamayı oluşmasına neden olmaz. `E`

*  Aksi halde, `E` korumalı bir tür ise finally yan tümcesi boş bir bloğa genişletilir:

   ```csharp
   finally {
   }
   ```

*  Aksi takdirde, finally yan tümcesi şu şekilde genişletilir:

   ```csharp
   finally {
       System.IDisposable d = e as System.IDisposable;
       if (d != null) d.Dispose();
   }
   ```    

   Yerel değişken `d` , herhangi bir kullanıcı koduna görünmez veya erişemez. Özellikle, kapsamı finally bloğunu içeren başka herhangi bir değişkenle çakışmaz.

Bir dizinin öğelerinin taşındığı `foreach` sıra aşağıdaki gibidir: Tek boyutlu diziler öğeleri için dizin ile `0` başlayıp dizinle biten `Length - 1`Dizin sıralaması artılarak çapraz işlenir. Çok boyutlu diziler için öğelere, en sağdaki boyutun dizinlerinin önce artması, sonra bir sonraki sol boyutun ve bu şekilde sol tarafta olması gerekir.

Aşağıdaki örnek, her bir değeri öğe sırasıyla iki boyutlu bir dizide yazdırır:
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
Oluşturulan çıkış aşağıdaki gibidir:
```csharp
1.2 2.3 3.4 4.5 5.6 6.7 7.8 8.9
```

Örnekte
```csharp
int[] numbers = { 1, 3, 5, 7, 9 };
foreach (var n in numbers) Console.WriteLine(n);
```
türü `n` olarak `int` algılanır`numbers`, öğesinin öğesi türü.

## <a name="jump-statements"></a>Atlama deyimleri

Geçiş deyimleri koşulsuz olmayan aktarma denetimi.

```antlr
jump_statement
    : break_statement
    | continue_statement
    | goto_statement
    | return_statement
    | throw_statement
    ;
```

Bir atma bildirimine aktarma denetimi, atlamanın ***hedefi*** olarak adlandırılır.

Bir blok içinde bir atdeyim oluştuğunda ve bu sıçrama ifadesinin hedefi o bloğun dışında olduğunda, atlamanın bloğundan ***Çıkış*** olarak söylenir. Bir atlamayla denetimi bir bloğun dışına aktarabileceği sürece denetim bir bloğa hiçbir şekilde aktarılmaz.

Sıçrama deyimlerinin yürütülmesi, aradaki `try` deyimlerin varlığı tarafından karmaşıktır. Bu tür `try` deyimler yokluğunda, bir sıçrama deyimi, denetimi bir atlamanın hedefine koşulsuz olarak aktarır. Bu tür bir araya giren `try` deyimler varsa, yürütme daha karmaşıktır. Atif deyim ilişkili `finally` bloklarla bir veya `try` daha fazla `finally` bloktan çıkıldığında denetim başlangıçta en içteki `try` deyimin bloğuna aktarılır. Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır. Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.

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
`finally` iki`try` deyimle ilişkili bloklar, denetim, sıçrama ifadesinin hedefine aktarılmadan önce yürütülür.

Oluşturulan çıkış aşağıdaki gibidir:
```
Before break
Innermost finally block
Outermost finally block
After break
```

### <a name="the-break-statement"></a>Break ekstresi

`switch` İfadeen`while`yakın kapsayan,, `do` ,,`foreach` veya ifadesiyle çıkar. `for` `break`

```antlr
break_statement
    : 'break' ';'
    ;
```

`break` Bir deyimin hedefi, en yakın kapsayan `do` `switch`, `while`, `for`,, veya `foreach` ifadesinin bitiş noktasıdır. Bir `break` ifade bir `switch`, `while` ,,`foreach` veya ifadesiyle çevrelenemez, derleme zamanı hatası oluşur. `do` `for`

Birden çok `switch`, `while`, `do` `break` ,, veya deyimleri`foreach` birbirine yuvalandığı zaman, bir deyim yalnızca en içteki deyim için geçerlidir. `for` Birden çok iç içe düzeylerdeki denetimi aktarmak için bir `goto` deyimin ([goto deyimidir](statements.md#the-goto-statement)) kullanılması gerekir.

Bir `break` ifade bir `finally` bloğundan ([TRY ifadesiyle](statements.md#the-try-statement)) çıkabilir. Bir `break` blok`finally` içinde bir ifade gerçekleştiğinde `break` , deyimin hedefi aynı `finally` blok içinde olmalıdır; Aksi takdirde, bir derleme zamanı hatası oluşur.

Bir `break` ifade aşağıdaki gibi yürütülür:

*  `try` `try` İfadeilişkili`finally`bloklarla bir veya daha fazla bloktan çıkıldığında denetim başlangıçta en içteki deyimin bloğuna aktarılır. `finally` `break` Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır. Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.
*  Denetim, `break` deyimin hedefine aktarılır.

Bir `break` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `break` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.

### <a name="the-continue-statement"></a>Continue ekstresi

`while` `do` `for`Bu ifade, en yakın kapsayan,,, veya `foreach` bildiriminin yeni bir yinelemesini başlatır. `continue`

```antlr
continue_statement
    : 'continue' ';'
    ;
```

Bir `continue` deyimin hedefi, en yakın kapsayan `while`, `do`, `for`, veya `foreach` bildiriminin gömülü ifadesinin bitiş noktasıdır. Bir `continue` ifade `while`, `do` ,,`foreach` veya ifadesiyle çevrelenemez, derleme zamanı hatası oluşur. `for`

Birden çok `while`, `do`, `for`, `continue` veya `foreach` deyimleri birbirine yuvalandığı zaman, bir deyim yalnızca en içteki deyim için geçerlidir. Birden çok iç içe düzeylerdeki denetimi aktarmak için bir `goto` deyimin ([goto deyimidir](statements.md#the-goto-statement)) kullanılması gerekir.

Bir `continue` ifade bir `finally` bloğundan ([TRY ifadesiyle](statements.md#the-try-statement)) çıkabilir. Bir `continue` blok`finally` içinde bir ifade gerçekleştiğinde `continue` , deyimin hedefi aynı `finally` blok içinde olmalıdır; Aksi takdirde derleme zamanı hatası oluşur.

Bir `continue` ifade aşağıdaki gibi yürütülür:

*  `try` `try` İfadeilişkili`finally`bloklarla bir veya daha fazla bloktan çıkıldığında denetim başlangıçta en içteki deyimin bloğuna aktarılır. `finally` `continue` Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır. Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.
*  Denetim, `continue` deyimin hedefine aktarılır.

Bir `continue` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `continue` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.

### <a name="the-goto-statement"></a>Goto ekstresi

İfade `goto` , denetimi bir etiketi tarafından işaretlenen bir ifadeye aktarır.

```antlr
goto_statement
    : 'goto' identifier ';'
    | 'goto' 'case' constant_expression ';'
    | 'goto' 'default' ';'
    ;
```

Bir `goto` *Identifier* ifadesinin hedefi, verilen etikete sahip etiketli deyimdir. Verilen ada sahip bir etiket geçerli işlev üyesinde yoksa veya `goto` ifade etiketin kapsamı içinde değilse, bir derleme zamanı hatası oluşur. Bu kural, iç içe geçmiş bir `goto` kapsamın denetimini dışarı aktarmak için bir deyimin kullanılmasına izin verir. Örnekte
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
bir `goto` ifade, iç içe bir kapsamın denetimini aktarmak için kullanılır.

Bir `goto case` deyimin hedefi, belirtilen sabit değere sahip bir `case` etiketi içeren hemen kapsayan `switch` deyimindeki ([Switch deyimindeki](statements.md#the-switch-statement)) ifade listesidir. [](conversions.md#implicit-conversions) `switch` İfade bir `switch` deyimden çevrede yoksa, constant_expression örtük olarak dönüştürülebilir (örtük dönüştürmeler), en yakın kapsayan deyimin yöneten türüne veya `goto case` En yakın kapsayan `switch` ifade verilen sabit değeri olan `case` bir etiket içermiyor, derleme zamanı hatası oluşur.

Bir `goto default` deyimin hedefi, `switch` bir`default` etiketi içeren hemen kapsayan deyimindeki ([Switch deyimindeki](statements.md#the-switch-statement)) ifade listesidir. İfade bir `switch` deyimle çevrede yoksa veya en yakın kapsayan `switch` ifade bir `default` etiket içermiyorsa, derleme zamanı hatası oluşur. `goto default`

Bir `goto` ifade bir `finally` bloğundan ([TRY ifadesiyle](statements.md#the-try-statement)) çıkabilir. Bir `goto` blok`finally` içinde bir ifade gerçekleştiğinde `goto` , deyimin hedefi aynı `finally` blok içinde olmalıdır, aksi takdirde bir derleme zamanı hatası oluşur.

Bir `goto` ifade aşağıdaki gibi yürütülür:

*  `try` `try` İfadeilişkili`finally`bloklarla bir veya daha fazla bloktan çıkıldığında denetim başlangıçta en içteki deyimin bloğuna aktarılır. `finally` `goto` Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır. Bu işlem, tüm araya `finally` `try` eklenen deyimlerin blokları yürütülene kadar yinelenir.
*  Denetim, `goto` deyimin hedefine aktarılır.

Bir `goto` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `goto` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.

### <a name="the-return-statement"></a>Return ekstresi

İfadesi, denetimi `return` deyimin göründüğü işlevin geçerli çağıranına döndürür. `return`

```antlr
return_statement
    : 'return' expression? ';'
    ;
```

İfadesi `return` olmayan bir deyim yalnızca bir değer hesaplamayan bir işlev üyesinde, diğer bir deyişle, sonuç türü olan bir Yöntem ([Yöntem gövdesi](classes.md#method-body)) `void`, `set` bir özelliğin veya dizin oluşturucunun erişimcisi, `add` ve `remove` bir olayın erişimcileri, örnek Oluşturucu, statik oluşturucu ya da yok edicisi.

İfadesi `return` içeren bir deyim yalnızca bir değeri hesaplayan bir işlev üyesinde, diğer bir deyişle, void olmayan sonuç türü olan bir yöntem `get` , bir özellik veya dizin oluşturucunun erişimcisi veya Kullanıcı tanımlı bir operatör ile kullanılabilir. Örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), ifadenin türünden kapsayan işlev üyesinin dönüş türüne sahip olmalıdır.

Return deyimleri, anonim işlev ifadelerinin gövdesinde de kullanılabilir ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) ve bu işlevler için hangi dönüştürmelerin var olduğunu belirlemeye katılabilirsiniz.

Bir `return` deyimin `finally` bloğunda ([TRY ifadesinde](statements.md#the-try-statement)) görünmesi için derleme zamanı hatası.

Bir `return` ifade aşağıdaki gibi yürütülür:

*  `return` Deyim bir ifade belirtiyorsa, ifade değerlendirilir ve elde edilen değer, örtük bir dönüştürme tarafından içerilen işlevin dönüş türüne dönüştürülür. Dönüştürmenin sonucu, işlev tarafından üretilen sonuç değeri haline gelir.
*  `try` `catch` `try` `finally` Deyimle ilişkilendirilmiş`finally` blokları olan bir veya daha fazla ya da blok varsa, denetim başlangıçta en içteki deyimin bloğuna aktarılır. `return` Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır. Bu işlem, kapsayan `finally` `try` tüm deyimlerin blokları yürütülene kadar yinelenir.
*  İçeren işlev bir zaman uyumsuz işlev değilse, denetim, içerilen işlevin çağıranına, varsa sonuç değeriyle birlikte döndürülür.
*  İçeren işlev bir zaman uyumsuz işlevtiyse, Denetim geçerli çağırana döndürülür ve sonuç değeri varsa, ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) bölümünde açıklandığı gibi Return görevinde kaydedilir.

Bir `return` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `return` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.

### <a name="the-throw-statement"></a>Throw deyimleri

`throw` İfade bir özel durum oluşturur.

```antlr
throw_statement
    : 'throw' expression? ';'
    ;
```

İfadesi `throw` içeren bir deyim, ifade hesaplanarak üretilen değeri oluşturur. İfade, etkin taban sınıfı olarak `System.Exception` `System.Exception` (veya alt sınıfı) olan bir tür parametre türünden veya ondan `System.Exception` türetilen bir sınıf türünün sınıf türünün bir değerini belirtmelidir. İfadenin `null`değerlendirmesi oluşursa bunun yerine bir `System.NullReferenceException` oluşturulur.

İfadesi `throw` olmayan bir deyim yalnızca bir `catch` blokta kullanılabilir, bu durumda deyim o `catch` blok tarafından işlenmekte olan özel durumu yeniden atar.

Bir `throw` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `throw` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.

Bir özel durum oluştuğunda denetim, özel durumu işleyebilen bir kapsayan `catch` `try` ifadede ilk yan tümcesine aktarılır. Oluşturulan özel durumun noktasından, uygun bir özel durum işleyicisine denetim aktarma noktasına gerçekleşen işlem, ***özel durum yayma***olarak bilinir. Bir özel durumun yayılması, özel durumla eşleşen bir `catch` yan tümce bulunana kadar aşağıdaki adımları tekrar tekrar değerlendirmeden oluşur. Bu açıklamada, ***throw noktası*** başlangıçta özel durumun oluşturulduğu konumdur.

*  Geçerli işlev üyesinde, throw noktasını kapsayan `try` her bir ifade incelenir. Her bir bildirimde `S`, en içteki `try` bildirimiyle başlayıp en dıştaki `try` ifadesiyle sona ermek üzere aşağıdaki adımlar değerlendirilir:

   * Bloğu bir `catch`veyadaha fazla yan tümce içeriyorsa ve bu blok birveyadahafazlayantümceiçeriyorsa,yantümceleri,'debelirtilenkurallaragöreözeldurumiçinuygunbirişleyicininyerinibulacakşekildeincelenir.`catch` `S` `try` [TRY ifadesinin](statements.md#the-try-statement)bölümü. Eşleşen `catch` bir yan tümce varsa, özel durum yayma, denetim bu `catch` yan tümce bloğuna aktarılarak tamamlanır.

   * Aksi takdirde `try` , blok veya bir `catch` bloğu `S` throw noktasını barındırır ve eğer bir `finally` blok içeriyorsa `S` denetim `finally` bloğa aktarılır. `finally` Blok başka bir özel durum oluşturursa, geçerli özel durumun işlenmesi sonlandırılır. Aksi takdirde, Denetim `finally` bloğun bitiş noktasına ulaştığında, geçerli özel durumun işlenmesi devam eder.

*  Geçerli işlev çağrısında bir özel durum işleyicisi bulunmuyorsa, işlev çağırma sonlandırılır ve aşağıdakilerden biri oluşur:

   * Geçerli işlev zaman uyumsuz ise, yukarıdaki adımlar işlev üyesinin çağrıldığı ifadeye karşılık gelen bir throw noktasıyla işlevin çağıranı için yinelenir.

   * Geçerli işlev zaman uyumsuz ve görev döndürüyor ise, özel durum, [Numaralandırıcı arabirimlerinde](classes.md#enumerator-interfaces)açıklandığı şekilde hatalı veya iptal edilmiş duruma yerleştirilen geri dönüş görevine kaydedilir.

   * Geçerli işlev zaman uyumsuz ve void döndürüyorsa, geçerli iş parçacığının eşitleme bağlamı, [sıralanabilir arabirimlerde](classes.md#enumerable-interfaces)açıklandığı gibi bilgilendirilir.

*  Özel durum işleme geçerli iş parçacığında tüm işlev üyesi çağırmaları sonlandırdığında, iş parçacığının özel durum için bir işleyici olmadığını belirten bir iş parçacığının kendisi sonlandırılır. Bu sonlandırmanın etkisi, uygulama tanımlı ' dır.

## <a name="the-try-statement"></a>TRY deyimleri

İfade `try` , bir bloğun yürütülmesi sırasında oluşan özel durumları yakalamak için bir mekanizma sağlar. Ayrıca,, `try` denetimi `try` deyimden çıktığında her zaman yürütülen bir kod bloğunu belirtme özelliği de sağlar.

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

Üç olası `try` deyim biçimi vardır:

*  Bir `try` blok ve ardından bir veya daha `catch` fazla blok gelir.
*  Bir `try` blok ve ardından bir `finally` blok gelir.
*  Bir blok ve ardından bir blok tarafından `catch` `finally` izlenen bir veya daha fazla blok. `try`

Bir `catch` yan tümce bir *exception_specifier*belirttiğinde, türü `System.Exception`, geçerli temel sınıfı olarak `System.Exception` (veya bir alt `System.Exception` sınıfı) içeren tür parametre türünden veya türetilen bir tür olmalıdır.

Bir `catch` yan tümce her ikisi de *tanımlayıcı*içeren bir *exception_specifier* belirttiğinde, verilen ad ve tür için bir ***özel durum değişkeni*** bildirilmiştir. Özel durum değişkeni, `catch` yan tümcesini genişleten bir kapsama sahip yerel bir değişkene karşılık gelir. *Exception_filter* ve *bloğunun*yürütülmesi sırasında, özel durum değişkeni işlenmekte olan özel durumu temsil eder. Kesin atama denetimi amacıyla, özel durum değişkeni tüm kapsamda kesin olarak atanır olarak değerlendirilir.

Bir `catch` yan tümce bir özel durum değişkeni adı içermiyorsa, filtre ve `catch` bloktaki özel durum nesnesine erişmek olanaksız olur.

Bir `catch` *exception_specifier* belirtmeyen bir yan tümce genel `catch` yan tümce olarak adlandırılır.

Bazı programlama dilleri, ' den `System.Exception`türetilmiş bir nesne olarak gösterilemeyen özel durumları destekleyebilir, ancak bu tür özel durumlar hiçbir şekilde kod tarafından C# üretilmeyebilir. Bu tür `catch` özel durumları yakalamak için genel bir yan tümce kullanılabilir. Bu nedenle, genel `catch` bir yan tümce türü `System.Exception`belirten bir değer olan anlam, diğer bir deyişle diğer dillerdeki özel durumları da yakalayabiliriz.

Bir özel durum için işleyicinin yerini bulmak için, `catch` yan tümceler sözlü sırada incelenir. Bir `catch` yan tümce bir tür belirtiyorsa, ancak özel durum filtresi yoksa, bu tür ile aynı veya ondan türetilmiş bir `catch` türü belirtmek için aynı `try` deyimdeki sonraki yan tümce için derleme zamanı hatası olur. Bir `catch` yan tümce hiçbir tür ve filtre yoksa, bu `try` deyimin son `catch` yan tümcesi olmalıdır.

Bir `catch` blok içinde, ifadesi `throw` olmayan bir deyim ([throw deyimi](statements.md#the-throw-statement)), `catch` blok tarafından yakalanan özel durumu yeniden oluşturmak için kullanılabilir. Bir özel durum değişkenine atamalar yeniden oluşturulan özel durumu değiştirmez.

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
yöntemi `F` bir özel durumu yakalar, konsola bazı tanılama bilgileri yazar, özel durum değişkenini değiştirir ve özel durumu yeniden oluşturur. Yeniden oluşturulan özel durum özgün özel durumdur, bu nedenle oluşturulan çıkış şu şekilde yapılır:
```
Exception in F: G
Exception in Main: G
```

İlk catch bloğu `e` geçerli özel durumu yeniden oluşturmak yerine oluşursa, üretilen çıkış aşağıdaki gibi olacaktır:
```
Exception in F: G
Exception in Main: F
```

Bir `break` blokdışına`finally` denetim aktarmak için, `continue`veya `goto` bildiriminde derleme zamanı hatasıdır. `break`Bir `continue` `finally` blok içinde bir, `goto` veya ifade oluştuğunda, deyimin hedefi aynı blok içinde olmalıdır, aksi takdirde bir derleme zamanı hatası oluşur. `finally`

Bir `return` deyimin bir `finally` blokta oluşması için derleme zamanı hatası.

Bir `try` ifade aşağıdaki gibi yürütülür:

*  Denetim, `try` bloğa aktarılır.
*  Denetim, `try` bloğunun bitiş noktasına ulaştığında:
   *  Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`
   *  Denetim, `try` deyimin bitiş noktasına aktarılır.

*  Bloğun`try` yürütülmesi sırasında `try` ifadeye bir özel durum yayıldığında:
   *  `catch` Yan tümceleri, varsa, özel durum için uygun bir işleyicinin yerini bulmak üzere görünüm sırasına göre incelenir. Bir `catch` yan tümce bir tür belirtmezse veya özel durum türünü ya da özel durum türünün temel türünü belirtir:
      *  `catch` Yan tümce bir özel durum değişkeni bildiriyorsa, özel durum nesnesi özel durum değişkenine atanır.
      *  `catch` Yan tümce bir özel durum filtresi bildirirse, filtre değerlendirilir. Olarak `false`değerlendirilirse, catch yan tümcesi bir eşleşme değildir ve arama uygun bir işleyici için sonraki `catch` yan tümcelerde devam eder.
      *  Aksi takdirde, `catch` yan tümce eşleşme olarak değerlendirilir ve denetim eşleşen `catch` bloğa aktarılır.
      *  Denetim, `catch` bloğunun bitiş noktasına ulaştığında:
         * Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`
         * Denetim, `try` deyimin bitiş noktasına aktarılır.
      *  Bloğun`catch` yürütülmesi sırasında `try` ifadeye bir özel durum yayıldığında:
         *  Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`
         *  Özel durum bir sonraki kapsayan `try` ifadeye yayılır.
   *  Deyimin hiçbir yan tümcesi yoksa `catch` veya hiçbir `catch` yan tümce özel durumla eşleşmez: `try`
      *  Deyimde bir `finally` blok varsa, `finally` blok yürütülür. `try`
      *  Özel durum bir sonraki kapsayan `try` ifadeye yayılır.

Bir `finally` bloğun deyimleri her zaman denetim bir `try` ifadeden ayrıldığında yürütülür. Bu, denetim aktarımının normal yürütmenin sonucu olarak,,, `break`veya `return` ifadesinin yürütülmesi `continue` `goto` `try` veya bir özel durumun bir özel durum yaymasından kaynaklanan bir sonuç olarak oluşup oluşmadığını belirtir. Ekstre.

Bir `finally` bloğun yürütülmesi sırasında bir özel durum oluşturulursa ve aynı finally bloğu içinde yakalanmadığında, özel durum sonraki kapsayan `try` ifadeye yayılır. Yayılmakta olan işlemde başka bir özel durum varsa, bu özel durum kaybedilir. Bir özel durumu yayma işlemi, `throw` deyimin açıklamasında ([throw deyimidir](statements.md#the-throw-statement)) daha ayrıntılı bir şekilde ele alınmıştır.

Deyimin erişilebilir olması durumunda `try` bir `try` deyimin `try` bloğuna erişilebilir.

Deyimde erişilebilir olduğunda `try` bir `catch` deyimin bloğu `try` erişilebilir olur.

Deyimin erişilebilir olması durumunda `try` bir `finally` deyimin `try` bloğuna erişilebilir.

Aşağıdakilerin her ikisi de doğruysa `try` , bir deyimin bitiş noktasına ulaşılabilir:

*  `try` Bloğun bitiş noktasına ulaşılabilir veya en az bir `catch` bloğun bitiş noktası erişilebilir.
*  Bir `finally` blok varsa, `finally` bloğun bitiş noktasına ulaşılabilir.

## <a name="the-checked-and-unchecked-statements"></a>Checked ve unchecked deyimleri

`checked` Ve`unchecked` deyimleri, tamsayı türü aritmetik işlemler ve dönüştürmeler için ***taşma denetimi bağlamını*** denetlemek için kullanılır.

```antlr
checked_statement
    : 'checked' block
    ;

unchecked_statement
    : 'unchecked' block
    ;
```

Deyimi, *bloktaki* tüm ifadelerin işaretlenmiş bir `unchecked` bağlamda değerlendirilmesini sağlar ve deyim, bloktaki tüm ifadelerin işaretlenmemiş bir bağlamda değerlendirilmesini sağlar. `checked`

`checked` `unchecked` Ve deyimleri, ve işleçleri ([Checked ve unchecked işleçleri](expressions.md#the-checked-and-unchecked-operators)), ifadeler yerine bloklar üzerinde çalıştıkları durumlar hariç, ve işleçlerine tam olarak eşdeğerdir. `unchecked` `checked`

## <a name="the-lock-statement"></a>Lock deyimleri

`lock` İfade, belirli bir nesne için karşılıklı dışlama kilidini edinir, bir ifade yürütür ve sonra kilidi serbest bırakır.

```antlr
lock_statement
    : 'lock' '(' expression ')' embedded_statement
    ;
```

Bir `lock` deyimin ifadesi, *reference_type*olarak bilinen bir türün değerini belirtmelidir. [](conversions.md#boxing-conversions) Bir`lock` deyimin ifadesi için hiçbir örtük paketleme dönüştürmesi (kutulama dönüştürmesi) yapılmaz ve bu nedenle, ifadenin bir *value_type*değerini belirtmek için derleme zamanı hatası vardır.

Formun `lock` bir açıklaması
```csharp
lock (x) ...
```
, reference_type 'in bir ifadesi olduğu için tam olarak eşdeğerdir `x`
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
`x` hariç yalnızca bir kez değerlendirilir.

Karşılıklı dışlama kilidi tutulurken, aynı yürütme iş parçacığında yürütülen kod da kilidi alabilir ve serbest bırakabilir. Ancak, diğer iş parçacıklarında yürütülen kodun kilit serbest bırakılana kadar kilidi almasını engellenir.

Statik `System.Type` verilere erişimin eşitlenmesi için nesneleri kilitleme önerilmez. Diğer kod aynı tür üzerinde kilitlenebilir, bu da kilitlenmeye neden olabilir. Özel bir statik nesneyi kilitleyerek statik verilere erişimi eşitlememeye daha iyi bir yaklaşım. Örneğin:
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

`using` İfade bir veya daha fazla kaynak edinir, bir ifade yürütür ve sonra kaynağı atar.

```antlr
using_statement
    : 'using' '(' resource_acquisition ')' embedded_statement
    ;

resource_acquisition
    : local_variable_declaration
    | expression
    ;
```

***Kaynak*** , adlı `System.IDisposable` `Dispose`tek bir parametresiz yöntemi içeren, uygulayan bir sınıf veya yapı olur. Kaynak kullanan kod, kaynağın artık gerekli olmadığını `Dispose` belirtmek için çağrı yapabilir. `Dispose` Çağrılırsa, çöp toplamanın bir sonucu olarak otomatik çıkarma işlemi gerçekleşir.

*Resource_acquisition* biçimi *local_variable_declaration* ise *local_variable_declaration* türü ya da `dynamic` örtük olarak dönüştürülebilir `System.IDisposable`bir tür olmalıdır. *Resource_acquisition* , *ifadesi* ise, bu ifade `System.IDisposable`örtülü olarak dönüştürülebilir olmalıdır.

Bir *resource_acquisition* içinde belirtilen yerel değişkenler salt okunurdur ve bir başlatıcı içermelidir. Katıştırılmış ifade bu yerel değişkenleri değiştirmeye çalışırsa (atama `++` veya ve `--` işleçler aracılığıyla) bir derleme zamanı hatası oluşur, bunların adresini alın veya ya da parametreleri veya `out` parametreleri olarak `ref` geçirin.

Bir `using` ifade üç parçaya çevrilir: alma, kullanım ve çıkarma. Kaynağın kullanımı dolaylı olarak bir `try` `finally` yan tümce içeren bir ifadeye alınmıştır. Bu `finally` yan tümce kaynağı ortadan kaldırır. Bir `null` kaynak elde alınırsa, hiçbir `Dispose` çağrı yapılmaz ve hiçbir özel durum oluşturulmaz. Kaynak türtür `dynamic` ise, dönüştürmenin kullanımdan önce başarılı olduğundan emin olmak için, alma `IDisposable` sırasında dinamik bir dinamik dönüştürme ([örtük dinamik dönüştürmeler](conversions.md#implicit-dynamic-conversions)) üzerinden dinamik olarak dönüştürülür. elden.

Formun `using` bir açıklaması
```csharp
using (ResourceType resource = expression) statement
```
olası üç Genişlemeden birine karşılık gelir. Null `ResourceType` yapılamayan bir değer türü olduğunda, genişletme
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

Aksi halde, `ResourceType` null yapılabilir bir değer türü veya dışında `dynamic`bir başvuru türü olduğunda, genişletme
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

Aksi halde, `ResourceType` ne `dynamic`zaman, genişletme
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

Her iki genişlede `resource` de değişken gömülü ifadede salt okunurdur `d` ve değişkenine, gömülü ifadeye ve görünmez.

Davranışın yukarıdaki genişlemeyle tutarlı olduğu sürece, bir uygulamanın belirli bir using ifadesini farklı bir şekilde uygulaması için, örneğin performans nedenleriyle, bir uygulamaya izin verilir.

Formun `using` bir açıklaması
```csharp
using (expression) statement
```
, mümkün olan üç genişleme sahiptir. Bu durumda `ResourceType` , varsa, `expression`öğesinin derleme zamanı türü örtülü olarak oluşturulur. Aksi takdirde, `IDisposable` `ResourceType`arabirimin kendisi olarak kullanılır. `resource` Değişkenine, gömülü ifadeye, ve görünmez olarak erişilemez.

Bir *resource_acquisition* , bir *local_variable_declaration*formunu aldığında, belirli bir türün birden çok kaynağını elde etmek mümkündür. Formun `using` bir açıklaması
```csharp
using (ResourceType r1 = e1, r2 = e2, ..., rN = eN) statement
```
, iç içe geçmiş `using` deyimlerin dizisine tam olarak eşdeğerdir:
```csharp
using (ResourceType r1 = e1)
    using (ResourceType r2 = e2)
        ...
            using (ResourceType rN = eN)
                statement
```

Aşağıdaki örnek adlı `log.txt` bir dosya oluşturur ve dosyaya iki satırlık metin yazar. Örnek daha sonra bu dosyayı okumak için aynı dosyayı açar ve içerilen metin satırlarını konsola kopyalar.
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

Ve sınıfları arabirimini kullandığından, örnek, temel alınan dosyanın yazma veya okuma işlemlerinden sonra düzgün şekilde kapatılmasını sağlamak için deyimlerini kullanabilir. `using` `TextWriter` `TextReader` `IDisposable`

## <a name="the-yield-statement"></a>Yield ekstresi

İfade, bir yineleyici bloğunda ([bloklar](statements.md#blocks)), bir yineleyicinin Numaralandırıcı nesnesine ([Numaralandırıcı nesneleri](classes.md#enumerator-objects)) veya sıralanabilir nesne ([numaralandırılabilir nesneler](classes.md#enumerable-objects)) bir değer vermek veya yinelemenin sonuna işaret etmek için kullanılır. `yield`

```antlr
yield_statement
    : 'yield' 'return' expression ';'
    | 'yield' 'break' ';'
    ;
```

`yield`ayrılmış bir sözcük değil; yalnızca bir `return` veya `break` anahtar sözcüğünden hemen önce kullanıldığında özel anlamı vardır. Diğer bağlamlarda `yield` tanımlayıcı olarak kullanılabilir.

Aşağıda açıklandığı gibi bir `yield` deyimin görünebileceği çeşitli kısıtlamalar vardır.

*  Bir `yield` deyimin (herhangi bir biçimde) bir *method_body*, *operator_body* veya *accessor_body* dışında görünmesi için derleme zamanı hatası
*  Bir deyimin (herhangi bir `yield` biçimde) anonim bir işlev içinde görünmesi için derleme zamanı hatasıdır.
*  Bir deyimin `yield` `finally` yan tümcesinde `try` görünmesi için (her bir biçimde) bir derleme zamanı hatasıdır.
*  Bir `yield return` deyimin, `try` herhangi`catch` bir yan tümce içeren bir ifadede görünmesini sağlayan derleme zamanı hatası.

Aşağıdaki örnekte, bazı geçerli ve geçersiz `yield` deyim kullanımları gösterilmektedir.

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

Bir örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)), `yield return` deyimdeki ifade türünün, yineleyicinin yield türüne ([yield türü](classes.md#yield-type)) sahip olması gerekir.

Bir `yield return` ifade aşağıdaki gibi yürütülür:

*  Deyimde verilen ifade değerlendirilir, örtülü olarak yield türüne dönüştürülür ve Numaralandırıcı nesnesinin `Current` özelliğine atanır.
*  Yineleyici bloğunun yürütülmesi askıya alındı. İfade bir veya daha fazla `try` blok içindeyse, ilişkili `finally` bloklar Şu anda yürütülmez. `yield return`
*  Numaralandırıcı nesnesinin `true` yöntemi, Numaralandırıcı nesnesinin bir sonraki öğeye başarıyla ilerlemediğini belirten, çağırana döner. `MoveNext`

Numaralandırıcı nesnesinin `MoveNext` yönteminin sonraki çağrısı, yineleyici bloğunun son askıya alındığı yerden yürütülmesini sürdürür.

Bir `yield break` ifade aşağıdaki gibi yürütülür:

*  `try` Deyimle`finally` `try` ilişkili`finally` blokları olan bir veya daha fazla blok varsa, denetim başlangıçta en içteki deyimin bloğuna aktarılır. `yield break` Ve denetimi bir `finally` bloğun bitiş noktasına ulaştığında, Denetim sonraki kapsayan `try` deyimin `finally` bloğuna aktarılır. Bu işlem, kapsayan `finally` `try` tüm deyimlerin blokları yürütülene kadar yinelenir.
*  Denetim, yineleyici bloğunun çağıranına döndürülür. Bu, Numaralandırıcı `Dispose` nesnesinin yöntemiyadayöntemidir.`MoveNext`

Bir `yield break` ifade denetimi başka bir yerde bir yere aktarmadığı için, bir `yield break` deyimin bitiş noktasına hiçbir şekilde ulaşılamıyor.
