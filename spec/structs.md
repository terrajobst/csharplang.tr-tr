---
ms.openlocfilehash: 6dd1dde67597b2125de9a1aa2fab9144128d533f
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71704037"
---
# <a name="structs"></a>Yapılar

Yapıları, veri üyeleri ve işlev üyeleri içerebilen veri yapılarını temsil ettikleri sınıflara benzerdir. Ancak, sınıfların aksine yapılar değer türlerdir ve yığın ayırmayı gerektirmez. Yapı türünün bir değişkeni doğrudan yapı verilerini içerir, ancak bir sınıf türünün değişkeni, ikinci olarak bir nesne olarak bilinen veri başvurusunu içerir.

Yapılar, özellikle değer semantiği olan küçük veri yapıları için yararlıdır. Karmaşık sayılar, bir koordinat sistemindeki işaret veya bir sözlükte anahtar-değer çiftleri, yapı birimlerinin iyi örnekleridir. Bu veri yapılarına yönelik anahtar, çok sayıda veri üyesine sahip olduğundan, devralma veya başvuru kimliği kullanımını gerektirmediğinden ve atamanın başvuru yerine değeri kopyalayan değer semantiğinin kullanılmasıyla kolayca uygulanabilirler.

[Basit türlerde](types.md#simple-types)açıklandığı gibi, tarafından C#sunulan `int`, `double`ve `bool`gibi basit türler aslında tüm yapı türleridir. Bu öntanımlı türlerin yapıları olduğu gibi, C# dilde yeni "temel" türler uygulamak için yapılar ve işleç aşırı yüklemesi de kullanabilirsiniz. Bu bölümün sonunda bu türden iki örnek verilmiştir ([struct örnekleri](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Struct bildirimleri

*Struct_declaration* , yeni bir yapı bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)):

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

*Struct_declaration* , isteğe bağlı bir dizi *öznitelik* ([öznitelik](attributes.md) *struct_modifier*) ve ardından isteğe bağlı bir `partial` değiştiricisi ve ardından anahtar `struct`[](structs.md#struct-modifiers)sözcüğünün ardından isteğe bağlı bir *type_parameter_list* belirtimi (tür parametreleri) ve *ardından isteğe bağlı* bir Struct_interfaces belirtimi ([tür parametreleri](classes.md#type-parameters)) ve ardından isteğe bağlı bir belirtimi (kısmi değiştirici) içerir.[ ](structs.md#partial-modifier)) ve ardından isteğe bağlı bir *type_parameter_constraints_clause*s belirtimi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *struct_body* ([struct Body](structs.md#struct-body)), isteğe bağlı olarak noktalı virgül gelir.

### <a name="struct-modifiers"></a>Yapı değiştiricileri

*Struct_declaration* , isteğe bağlı olarak bir yapı değiştiricileri dizisi içerebilir:

```antlr
struct_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | struct_modifier_unsafe
    ;
```

Aynı değiştiricinin bir struct bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.

Bir struct bildiriminin değiştiricileri, bir sınıf bildirimiyle ([sınıf bildirimleri](classes.md#class-declarations)) aynı anlama sahiptir.

### <a name="partial-modifier"></a>Kısmi değiştirici

`partial` değiştirici, bu *struct_declaration* kısmi bir tür bildirimi olduğunu gösterir. Bir kapsayan ad alanı veya tür bildiriminde aynı ada sahip birden çok kısmi struct bildirimi, [kısmi türlerde](classes.md#partial-types)belirtilen kurallara göre tek bir struct bildirimi oluşturmak için birleştirilir.

### <a name="struct-interfaces"></a>Struct arabirimleri

Yapı bildirimi *struct_interfaces* bir belirtim içerebilir ve bu durumda yapının verilen arabirim türlerini doğrudan uygulaması söylenemez.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Arabirim Uygulamaları, [arabirim uygulamalarında](interfaces.md#interface-implementations)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="struct-body"></a>Yapı gövdesi

Bir yapının *struct_body* yapının üyelerini tanımlar.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Yapı üyeleri

Bir yapının üyeleri, *struct_member_declaration*s tarafından tanıtılan üyelerden ve `System.ValueType`türünden devralınan üyelere oluşur.

```antlr
struct_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | static_constructor_declaration
    | type_declaration
    | struct_member_declaration_unsafe
    ;
```

[Sınıf ve yapı farklılıklarında](structs.md#class-and-struct-differences)belirtilen farklar dışında, [sınıf üyeleri için](classes.md#class-members) [yineleyiciler](classes.md#iterators) aracılığıyla sunulan sınıf üyelerinin açıklamaları da yapı üyelerine uygulanır.

## <a name="class-and-struct-differences"></a>Sınıf ve yapı farkları

Yapılar çeşitli önemli yollarla sınıflardan farklıdır:

*  Yapılar değer türlerdir ([değer semantiği](structs.md#value-semantics)).
*  Tüm yapı türleri dolaylı olarak `System.ValueType` ([Devralma](structs.md#inheritance)) sınıfından devralınır.
*  Struct türündeki bir değişkene atama, atanmakta olan değerin ([atama](structs.md#assignment)) bir kopyasını oluşturur.
*  Bir yapının varsayılan değeri, tüm değer türü alanları varsayılan değerlerine ve tüm başvuru türü alanlarına `null` ([varsayılan değerler](structs.md#default-values)) olarak ayarlanarak oluşturulan değerdir.
*  Kutulama ve kutudan çıkarma işlemleri bir yapı türü ve `object` ([kutulama ve kutudan](structs.md#boxing-and-unboxing)çıkarma) arasında dönüştürmek için kullanılır.
*  `this` anlamı yapılar için farklıdır ([Bu erişim](expressions.md#this-access)).
*  Bir yapının örnek alanı bildirimlerine değişken başlatıcıları ([alan başlatıcıları](structs.md#field-initializers)) ekleme izni verilmez.
*  Bir yapının parametresiz örnek Oluşturucusu ([oluşturucular](structs.md#constructors)) bildirme izni yok.
*  Bir yapının yıkıcı ([Yıkıcılar](structs.md#destructors)) bildirme izni yok.

### <a name="value-semantics"></a>Değer semantiği

Yapılar değer türlerdir ([değer türleri](types.md#value-types)) ve değer semantiğini kabul edilir. Diğer yandan sınıflar, başvuru türleridir ([başvuru türleri](types.md#reference-types)) ve başvuru semantiklerine sahip olarak söylenir.

Yapı türünün bir değişkeni doğrudan yapı verilerini içerir, ancak bir sınıf türünün değişkeni, ikinci olarak bir nesne olarak bilinen veri başvurusunu içerir. Bir struct `B` `A` türünde bir örnek alanı içerdiğinde ve `A` bir yapı türü ise, `A` `B` veya `B`oluşturulan bir türe bağlı olarak bir derleme zamanı hatası olur. Bir struct `X`, `X` `Y`bir örnek alanı içeriyorsa, bir struct `Y` ***doğrudan bağlıdır*** . Bu tanım verildiğinde, bir yapının bağlı olduğu yapı kümesinin tamamı, ilişkiye bağlı olarak, ***doğrudan '*** nin geçişli kapanışı olur.  Örneğin:
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
`Node`, kendi türünün bir örnek alanı içerdiğinden bir hatadır.  Başka bir örnek
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
Her türden `A`, `B`ve `C` birbirlerine bağlı olduğundan bir hata oluştu.

Sınıflar ile, iki değişkenin aynı nesneye başvurması ve bu nedenle bir değişkende işlemler için diğer değişken tarafından başvurulan nesneyi etkilemesi mümkündür. Yapılar ile, her birinin verilerin kendi kopyası vardır (`ref` ve `out` parametre değişkenleri olması dışında) ve birindeki işlemler diğerini etkilemeyebilir. Ayrıca, yapılar başvuru türleri olmadığından, yapı türünün değerlerinin `null`olması mümkün değildir.

Bildirim verildi
```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
kod parçası
```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 100;
System.Console.WriteLine(b.x);
```
`10`değerini verir. `a` `b` atama, değerin bir kopyasını oluşturur ve bu nedenle `b` `a.x`atamasından etkilenmez. Bunun yerine bir sınıf olarak bildirildiği `Point`, `a` ve `b` aynı nesneye başvuracağından çıkış `100`.

### <a name="inheritance"></a>Devralma

Tüm yapı türleri dolaylı olarak `System.ValueType`sınıfından devralır, bu, sırasıyla sınıf `object`devralır. Bir struct bildirimi uygulanan arabirimlerin bir listesini belirtebilir, ancak bir struct bildiriminin bir temel sınıf belirtmesi mümkün değildir.

Yapı türleri hiçbir zaman soyut değildir ve her zaman örtük olarak mühürlenmez. Bu nedenle `abstract` ve `sealed` değiştiricilerine bir struct bildiriminde izin verilmez.

Yapılar için devralma desteklenmediğinden, bir struct üyesinin tanımlanmış erişilebilirliği `protected` veya `protected internal`olamaz.

Bir yapıdaki işlev üyeleri `abstract` veya `virtual`olamaz ve `override` değiştiricisine yalnızca `System.ValueType`devralınan yöntemleri geçersiz kılmak için izin verilir.

### <a name="assignment"></a>Atama

Struct türündeki bir değişkene atama, atanmakta olan değerin bir kopyasını oluşturur. Bu, atamadan bir sınıf türü değişkenine farklılık gösterir. Bu, başvuruyu kopyalayan ancak başvuru tarafından tanımlanan nesneye değil.

Bir atamaya benzer şekilde, bir struct bir değer parametresi olarak geçirildiğinde veya bir işlev üyesinin sonucu olarak döndürüldüğünde, yapının bir kopyası oluşturulur. Bir struct, bir `ref` veya `out` parametresi kullanılarak bir işlev üyesine başvuru ile geçirilebilir.

Bir yapının özelliği veya Dizin Oluşturucusu bir atamanın hedefi olduğunda, özellik veya Dizin Oluşturucu erişimi ile ilişkili örnek ifadesi bir değişken olarak sınıflandırılmalıdır. Örnek ifadesi bir değer olarak sınıflandırıldığında, bir derleme zamanı hatası oluşur. Bu, [basit atamada](expressions.md#simple-assignment)daha ayrıntılı olarak açıklanmıştır.

### <a name="default-values"></a>Varsayılan değerler

[Varsayılan değerler](variables.md#default-values)' de açıklandığı gibi çeşitli değişkenler, oluşturuldukları sırada varsayılan değerlerine otomatik olarak başlatılır. Sınıf türleri ve diğer başvuru türleri değişkenleri için, bu varsayılan değer `null`. Ancak, yapılar `null`değer türleri olduğundan, yapının varsayılan değeri, tüm değer türü alanları varsayılan değerlerine ve tüm başvuru türü `null`alanlarına ayarlanarak oluşturulan değerdir.

Yukarıda belirtilen `Point` yapısına başvurma, örnek
```csharp
Point[] a = new Point[100];
```
dizideki her `Point`, `x` ve `y` alanları sıfıra ayarlanarak oluşturulan değere başlatır.

Bir yapının varsayılan değeri, yapının varsayılan oluşturucusunun ([Varsayılan oluşturucular](types.md#default-constructors)) döndürdüğü değere karşılık gelir. Bir sınıfın aksine, bir yapının parametresiz örnek Oluşturucusu bildirmesine izin verilmez. Bunun yerine, her yapının örtük olarak, tüm değer türü alanlarını varsayılan değerlerine ve tüm başvuru türü alanlarına `null`için ayarlamalarından elde edilen değeri döndüren, parametresiz bir örnek Oluşturucusu vardır.

Yapılar, varsayılan başlatma durumunu geçerli bir durum olacak şekilde göz önünde bulundurmanız için tasarlanmalıdır. örnekte
```csharp
using System;

struct KeyValuePair
{
    string key;
    string value;

    public KeyValuePair(string key, string value) {
        if (key == null || value == null) throw new ArgumentException();
        this.key = key;
        this.value = value;
    }
}
```
Kullanıcı tanımlı örnek Oluşturucu yalnızca açıkça çağrıldığı yerde null değerlere karşı koruma sağlar. Bir `KeyValuePair` değişkeninin varsayılan değer başlatmasına tabi olduğu durumlarda, `key` ve `value` alanları null olur ve yapının bu durumu işlemek için hazırlanması gerekir.

### <a name="boxing-and-unboxing"></a>Kutulama ve kutudan çıkarma

Bir sınıf türünün değeri, başvuruya yalnızca derleme zamanında başka bir tür olarak davranarak sınıf tarafından uygulanan bir arabirim türü `object` veya türü türüne dönüştürülebilir. Benzer şekilde, `object` veya bir arabirim türünün değeri bir değeri, başvuru değiştirilmeden bir sınıf türüne geri dönüştürülebilir (ancak bu durumda bir çalışma zamanı tür denetimi gereklidir).

Yapılar başvuru türleri olmadığından, bu işlemler yapı türleri için farklı şekilde uygulanır. Yapı türünde bir değer `object` türüne veya yapı tarafından uygulanan bir arabirim türüne dönüştürüldüğünde, kutulama işlemi gerçekleşir. Benzer şekilde, `object` türü veya bir arabirim türünün değeri bir yapı türüne geri dönüştürüldüğünde, kutudan çıkarma işlemi gerçekleşir. Sınıf türlerinde aynı işlemlerden önemli bir farklılık, kutulama ve kutudan çıkarma, yapı değerini kutulanmış örneğin içine veya dışına kopyaladır. Bu nedenle, kutulama veya kutudan çıkarma işleminin ardından kutulanmış yapıda yapılan değişiklikler kutulanmış yapıda yansıtılmaz.

Bir struct türü `System.Object` devralınan sanal bir yöntemi geçersiz kıldığında (`Equals`, `GetHashCode`veya `ToString`gibi), yapı türünün bir örneği aracılığıyla sanal yöntemin çağrılması kutulama oluşmasına neden olmaz. Bu, struct bir tür parametresi olarak kullanıldığında ve çağrı tür parametre türünün bir örneği aracılığıyla gerçekleşse bile geçerlidir. Örneğin:
```csharp
using System;

struct Counter
{
    int value;

    public override string ToString() {
        value++;
        return value.ToString();
    }
}

class Program
{
    static void Test<T>() where T: new() {
        T x = new T();
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
        Console.WriteLine(x.ToString());
    }

    static void Main() {
        Test<Counter>();
    }
}
```

Programın çıkışı:
```console
1
2
3
```

`ToString` yan etkileri olması için hatalı stil olsa da, örnek üç `x.ToString()`çağırma için kutulanmanın olmadığını gösterir.

Benzer şekilde, kısıtlı bir tür parametresindeki bir üyeye erişirken paketleme hiçbir zaman açıkça gerçekleşmez. Örneğin, bir arabirimin, bir değeri değiştirmek için kullanılabilecek bir yöntem `Increment` `ICounter` olduğunu varsayalım. `ICounter` bir kısıtlama olarak kullanılıyorsa, `Increment` yönteminin uygulanması, `Increment` çağrılan değişkene bir başvuru ile çağrılır, hiçbir koşulda paketlenmiş bir kopya.

```csharp
using System;

interface ICounter
{
    void Increment();
}

struct Counter: ICounter
{
    int value;

    public override string ToString() {
        return value.ToString();
    }

    void ICounter.Increment() {
        value++;
    }
}

class Program
{
    static void Test<T>() where T: ICounter, new() {
        T x = new T();
        Console.WriteLine(x);
        x.Increment();                    // Modify x
        Console.WriteLine(x);
        ((ICounter)x).Increment();        // Modify boxed copy of x
        Console.WriteLine(x);
    }

    static void Main() {
        Test<Counter>();
    }
}
```

İlk `Increment` çağrısı, `x`değişken içindeki değeri değiştirir. Bu, `x`paketlenmiş bir kopyasında değeri değiştiren `Increment`ikinci çağrısıyla eşdeğer değildir. Bu nedenle, programın çıktısı şu şekilde olur:
```console
0
1
1
```

Kutulama ve kutudan çıkarma hakkında daha fazla bilgi için bkz. [kutulama ve kutudan](types.md#boxing-and-unboxing)çıkarma.

### <a name="meaning-of-this"></a>Bunun anlamı

Bir sınıfın örnek Oluşturucu veya örnek işlev üyesi içinde `this`, bir değer olarak sınıflandırıldı. Bu nedenle `this`, işlev üyesinin çağrıldığı örneğe başvurmak için kullanılabilir ancak, bir sınıfın işlev üyesinde `this` atamak mümkün değildir.

Bir yapının örnek Oluşturucusu içinde `this`, yapı türünün bir `out` parametresine karşılık gelir ve bir yapının örnek işlev üyesi `this`, yapı türünün `ref` parametresine karşılık gelir. Her iki durumda da, `this` bir değişken olarak sınıflandırıldı ve işlev üyesinin `this` atama ile veya `ref` ya da `out` parametresi olarak geçirerek çağrıldığı yapının tamamını değiştirmek mümkündür.

### <a name="field-initializers"></a>Alan başlatıcıları

[Varsayılan değerler](structs.md#default-values)bölümünde açıklandığı gibi, bir yapının varsayılan değeri, tüm değer türü alanlarını varsayılan değerlerine ve tüm başvuru türü alanlarına `null`. Bu nedenle, bir yapı, örnek alan bildirimlerine değişken başlatıcıları dahil etmek için izin vermez. Bu kısıtlama yalnızca örnek alanları için geçerlidir. Yapının statik alanlarının değişken başlatıcıları içermesi için izin verilir.

Örnek
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
örnek alan bildirimleri değişken başlatıcılar içerdiğinden hata oluştu.

### <a name="constructors"></a>Oluşturucular

Bir sınıfın aksine, bir yapının parametresiz örnek Oluşturucusu bildirmesine izin verilmez. Bunun yerine, her yapının örtük olarak, tüm değer türü alanlarını varsayılan değerlerine ve tüm başvuru türü alanlarına null ([Varsayılan oluşturucular](types.md#default-constructors)) olarak ayarlamalarından elde edilen değeri döndüren, parametresiz bir örnek Oluşturucusu vardır. Bir struct, parametrelere sahip örnek oluşturucularını bildirebilir. Örneğin:
```csharp
struct Point
{
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

Yukarıdaki bildirim ve deyimler verildiğinde
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
her ikisi de `x` ve `y` sıfırdan `Point` oluşturun.

Bir struct Instance oluşturucusunun `base(...)`form Oluşturucu başlatıcısı içerme izni yoktur.

Yapı örneği Oluşturucusu bir Oluşturucu başlatıcısı belirtmezse, `this` değişkeni yapı türünün bir `out` parametresine karşılık gelir ve bir `out` parametresine benzer şekilde, oluşturucunun döndürdüğü her yerde `this` kesinlikle atanmalıdır ([kesin atama](variables.md#definite-assignment)). Yapı örneği Oluşturucusu bir Oluşturucu başlatıcısı belirtiyorsa, `this` değişkeni yapı türünün bir `ref` parametresine karşılık gelir ve bir `ref` parametresine benzer `this`, Oluşturucu gövdesine kesin olarak atanmış olarak değerlendirilir. Örnek Oluşturucu uygulamasını aşağıdan göz önünde bulundurun:
```csharp
struct Point
{
    int x, y;

    public int X {
        set { x = value; }
    }

    public int Y {
        set { y = value; }
    }

    public Point(int x, int y) {
        X = x;        // error, this is not yet definitely assigned
        Y = y;        // error, this is not yet definitely assigned
    }
}
```

Oluşturulan yapının tüm alanları kesinlikle atanana kadar, örnek üye işlevi (`X` ve `Y`için küme erişimcileri dahil) çağrılabilir. Tek özel durum otomatik olarak uygulanan Özellikler ([Otomatik uygulanan özellikler](classes.md#automatically-implemented-properties)) içerir. Kesin atama kuralları ([basit atama ifadeleri](variables.md#simple-assignment-expressions)), bu yapı türünün örnek Oluşturucusu içindeki bir yapı türünün bir otomatik özelliğine atamayı özel olarak muaf bulundurmaktır: böyle bir atama, otomatik özelliğin gizli yedekleme alanının kesin bir atamasını kabul edilir. Bu nedenle aşağıdakilere izin verilir:

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y) {
        X = x;      // allowed, definitely assigns backing field
        Y = y;      // allowed, definitely assigns backing field
    }
```

### <a name="destructors"></a>Yıkıcılar

Bir yapının yıkıcı bildirme izni yok.

### <a name="static-constructors"></a>Statik oluşturucular

Yapılar için statik oluşturucular, sınıflarla aynı kuralların çoğunu izler. Bir yapı türü için statik oluşturucunun yürütülmesi, aşağıdaki olayların ilki tarafından bir uygulama etki alanında gerçekleşecek şekilde tetiklenir:

*  Struct türündeki statik bir üyeye başvurulur.
*  Struct türünün açıkça tanımlanmış bir Oluşturucusu çağırılır.

Yapı türlerinin varsayılan değerlerinin ([varsayılan değerler](structs.md#default-values)) oluşturulması statik oluşturucuyu tetiklemez. (Bir dizideki öğelerin ilk değeri bu bir örnektir.)

## <a name="struct-examples"></a>Struct örnekleri

Aşağıda, dilin önceden tanımlanmış türlerine benzer şekilde kullanılabilecek türler oluşturmak için `struct` türlerini kullanmanın iki önemli örneği gösterilmektedir, ancak değiştirme semantiği vardır.

### <a name="database-integer-type"></a>Veritabanı tamsayı türü

Aşağıdaki `DBInt` yapısı, `int` türünün tüm değer kümesini temsil eden bir tamsayı türü ve bilinmeyen bir değeri belirten ek bir durum uygular. Bu özelliklere sahip bir tür veritabanlarında yaygın olarak kullanılır.

```csharp
using System;

public struct DBInt
{
    // The Null member represents an unknown DBInt value.

    public static readonly DBInt Null = new DBInt();

    // When the defined field is true, this DBInt represents a known value
    // which is stored in the value field. When the defined field is false,
    // this DBInt represents an unknown value, and the value field is 0.

    int value;
    bool defined;

    // Private instance constructor. Creates a DBInt with a known value.

    DBInt(int value) {
        this.value = value;
        this.defined = true;
    }

    // The IsNull property is true if this DBInt represents an unknown value.

    public bool IsNull { get { return !defined; } }

    // The Value property is the known value of this DBInt, or 0 if this
    // DBInt represents an unknown value.

    public int Value { get { return value; } }

    // Implicit conversion from int to DBInt.

    public static implicit operator DBInt(int x) {
        return new DBInt(x);
    }

    // Explicit conversion from DBInt to int. Throws an exception if the
    // given DBInt represents an unknown value.

    public static explicit operator int(DBInt x) {
        if (!x.defined) throw new InvalidOperationException();
        return x.value;
    }

    public static DBInt operator +(DBInt x) {
        return x;
    }

    public static DBInt operator -(DBInt x) {
        return x.defined ? -x.value : Null;
    }

    public static DBInt operator +(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value + y.value: Null;
    }

    public static DBInt operator -(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value - y.value: Null;
    }

    public static DBInt operator *(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value * y.value: Null;
    }

    public static DBInt operator /(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value / y.value: Null;
    }

    public static DBInt operator %(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value % y.value: Null;
    }

    public static DBBool operator ==(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value == y.value: DBBool.Null;
    }

    public static DBBool operator !=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value != y.value: DBBool.Null;
    }

    public static DBBool operator >(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value > y.value: DBBool.Null;
    }

    public static DBBool operator <(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value < y.value: DBBool.Null;
    }

    public static DBBool operator >=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value >= y.value: DBBool.Null;
    }

    public static DBBool operator <=(DBInt x, DBInt y) {
        return x.defined && y.defined? x.value <= y.value: DBBool.Null;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBInt)) return false;
        DBInt x = (DBInt)obj;
        return value == x.value && defined == x.defined;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        return defined? value.ToString(): "DBInt.Null";
    }
}
```

### <a name="database-boolean-type"></a>Veritabanı Boole türü

Aşağıdaki `DBBool` yapısı üç değerli bir mantıksal tür uygular. Bu türün olası değerleri, `Null` üyesinin bilinmeyen bir değeri gösterdiği `DBBool.True`, `DBBool.False`ve `DBBool.Null`. Bu üç değerli mantıksal türler genellikle veritabanlarında kullanılır.

```csharp
using System;

public struct DBBool
{
    // The three possible DBBool values.

    public static readonly DBBool Null = new DBBool(0);
    public static readonly DBBool False = new DBBool(-1);
    public static readonly DBBool True = new DBBool(1);

    // Private field that stores -1, 0, 1 for False, Null, True.

    sbyte value;

    // Private instance constructor. The value parameter must be -1, 0, or 1.

    DBBool(int value) {
        this.value = (sbyte)value;
    }

    // Properties to examine the value of a DBBool. Return true if this
    // DBBool has the given value, false otherwise.

    public bool IsNull { get { return value == 0; } }

    public bool IsFalse { get { return value < 0; } }

    public bool IsTrue { get { return value > 0; } }

    // Implicit conversion from bool to DBBool. Maps true to DBBool.True and
    // false to DBBool.False.

    public static implicit operator DBBool(bool x) {
        return x? True: False;
    }

    // Explicit conversion from DBBool to bool. Throws an exception if the
    // given DBBool is Null, otherwise returns true or false.

    public static explicit operator bool(DBBool x) {
        if (x.value == 0) throw new InvalidOperationException();
        return x.value > 0;
    }

    // Equality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator ==(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value == y.value? True: False;
    }

    // Inequality operator. Returns Null if either operand is Null, otherwise
    // returns True or False.

    public static DBBool operator !=(DBBool x, DBBool y) {
        if (x.value == 0 || y.value == 0) return Null;
        return x.value != y.value? True: False;
    }

    // Logical negation operator. Returns True if the operand is False, Null
    // if the operand is Null, or False if the operand is True.

    public static DBBool operator !(DBBool x) {
        return new DBBool(-x.value);
    }

    // Logical AND operator. Returns False if either operand is False,
    // otherwise Null if either operand is Null, otherwise True.

    public static DBBool operator &(DBBool x, DBBool y) {
        return new DBBool(x.value < y.value? x.value: y.value);
    }

    // Logical OR operator. Returns True if either operand is True, otherwise
    // Null if either operand is Null, otherwise False.

    public static DBBool operator |(DBBool x, DBBool y) {
        return new DBBool(x.value > y.value? x.value: y.value);
    }

    // Definitely true operator. Returns true if the operand is True, false
    // otherwise.

    public static bool operator true(DBBool x) {
        return x.value > 0;
    }

    // Definitely false operator. Returns true if the operand is False, false
    // otherwise.

    public static bool operator false(DBBool x) {
        return x.value < 0;
    }

    public override bool Equals(object obj) {
        if (!(obj is DBBool)) return false;
        return value == ((DBBool)obj).value;
    }

    public override int GetHashCode() {
        return value;
    }

    public override string ToString() {
        if (value > 0) return "DBBool.True";
        if (value < 0) return "DBBool.False";
        return "DBBool.Null";
    }
}
```
