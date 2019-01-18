---
ms.openlocfilehash: 72d17175dfb8ef284dce6cf7e5837420fa06f16a
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47229960"
---
# <a name="structs"></a>Yapılar

Veri üyeleri ve işlev üyeleri içerebilir veri yapılarını temsil ettikleri yapılar için sınıflar benzer. Ancak, farklı sınıflar, yapılar değer türleri ve yığın ayırma gerektirmez. Bir sınıf türünün bir değişkeni verilerin ikinci bilinen bir nesne olarak başvuru içerirken bir yapı türünün değişkenini doğrudan, struct'ın verileri içerir.

Yapılar değer semantiklere sahip küçük veri yapıları için özellikle yararlıdır. Karmaşık sayılar, koordinat sisteminde noktaları veya anahtar-değer çiftlerinin bir sözlükteki tüm iyi yapılar örnekleridir. Bu veri yapıları için anahtar, birkaç veri üyeleri, devralma veya başvuru kimliği kullanımı gerektirmez ve bunların rahatça atama değerin başvurusu yerine nereye kopyalayacağını değeri semantiği kullanıyor uygulanabilir olduğunu sahip olduğunu belirtir.

Bölümünde anlatıldığı gibi [basit türler](types.md#simple-types), sağlanan C# kullanarak gibi basit türler `int`, `double`, ve `bool`, hatta tüm yapı türleri şunlardır. Yalnızca bu önceden tanımlanmış türler yapı birimleridir gibi ayrıca yapıları ve yeni "temel" türler C# dilinde uygulamak için işleç aşırı yüklemesi kullanmak da mümkündür. İki tür örnekleri, tür, bu bölümün sonunda verilmiştir ([yapı örnekler](structs.md#struct-examples)).

## <a name="struct-declarations"></a>Yapı bildirimleri

A *struct_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir yapı bildirir:

```antlr
struct_declaration
    : attributes? struct_modifier* 'partial'? 'struct' identifier type_parameter_list?
      struct_interfaces? type_parameter_constraints_clause* struct_body ';'?
    ;
```

A *struct_declaration* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)) ve ardından isteğe bağlı bir dizi *struct_modifier*s ([yapı değiştiriciler](structs.md#struct-modifiers)) ve ardından isteğe bağlı `partial` değiştiricisi anahtar sözcüğü ve ardından, `struct` ve *tanımlayıcı* ardından yapısı adları bir İsteğe bağlı *type_parameter_list* belirtimi ([tür parametrelerindeki](classes.md#type-parameters)) ve ardından isteğe bağlı *struct_interfaces* belirtimi ([Partial değiştiricisi](structs.md#partial-modifier))) ve ardından isteğe bağlı *type_parameter_constraints_clause*s belirtimi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *struct_body* ([yapı gövdesi](structs.md#struct-body)), noktalı virgül tarafından izlenen isteğe bağlı olarak.

### <a name="struct-modifiers"></a>Yapı değiştiriciler

A *struct_declaration* isteğe bağlı olarak bir dizi yapı değiştiriciler içerebilir:

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

Aynı değiştiricisi bir struct bildiriminde birden çok kez görünmesi için bir derleme zamanı hatasıdır.

Bu sınıf bildirimi aynı anlamı, bir yapı bildirimi, değiştiricilere sahip ([sınıf bildirimleri](classes.md#class-declarations)).

### <a name="partial-modifier"></a>Partial değiştiricisi

`partial` Değiştiricisi gösteren bu *struct_declaration* kısmi türü bildirimi. Kapsayan ad alanı veya tür bildirimi içinde aynı ada sahip birden fazla kısmi sınıfının veya yapı bildirimi birleştiren bir yapı bildirimi oluşturmak için aşağıdaki kuralları belirtilen [kısmi türlerinden](classes.md#partial-types).

### <a name="struct-interfaces"></a>Yapı arabirimleri

Yapı bildirimi içerebilir bir *struct_interfaces* içinde çalışması struct söyledi doğrudan belirli bir arabirim türleri uygulamak için belirtimi.

```antlr
struct_interfaces
    : ':' interface_type_list
    ;
```

Arabirim uygulamalarına açıklanmıştır daha ayrıntılı olarak [arabirimi uygulamaları](interfaces.md#interface-implementations).

### <a name="struct-body"></a>Yapı gövdesi

*Struct_body* struct'ın Yapı üyeleri tanımlar.

```antlr
struct_body
    : '{' struct_member_declaration* '}'
    ;
```

## <a name="struct-members"></a>Yapı üyeleri

Yapı üyeleri tarafından tanıtılan üyeleri oluşur, *struct_member_declaration*s ve üyeleri devralınan bir türden `System.ValueType`.

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

Belirtilen farklılıklar dışında [sınıf ile yapı farklar](structs.md#class-and-struct-differences), sağlanan sınıf üyeleri açıklamalarını [sınıf üyeleri](classes.md#class-members) aracılığıyla [yineleyiciler](classes.md#iterators) yapısı için geçerlidir Üyeler de.

## <a name="class-and-struct-differences"></a>Sınıf ile yapı farkları

Yapılar, birkaç önemli şekilde sınıflardan farklılık gösterir:

*  Değer türleri yapı birimleridir ([değer semantiği](structs.md#value-semantics)).
*  Tüm yapı türleri örtülü olarak sınıftan `System.ValueType` ([devralma](structs.md#inheritance)).
*  Bir yapı türü bir değişkene atanması, atanan değerin bir kopyasını oluşturur ([atama](structs.md#assignment)).
*  Bir yapı varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarlayarak üretilen değeridir `null` ([varsayılan değerler](structs.md#default-values)).
*  Kutulama ve kutudan çıkarma işlemleri, bir yapı türü arasında dönüştürmek için kullanılır ve `object` ([kutulama ve kutudan çıkarma](structs.md#boxing-and-unboxing)).
*  Anlamını `this` yapılar için farklı ([bu erişim](expressions.md#this-access)).
*  Bir yapı için örnek alanı bildirimleri, değişken başlatıcılar dahil izin verilmez ([alan başlatıcıları](structs.md#field-initializers)).
*  Bir yapının bir parametresiz örnek oluşturucusu bildirmek için izin verilmiyor ([oluşturucular](structs.md#constructors)).
*  Bir yapının bir yıkıcı bildirmek için izin verilmiyor ([yok ediciler](structs.md#destructors)).

### <a name="value-semantics"></a>Değer semantiği

Değer türleri yapı birimleridir ([değer türleri](types.md#value-types)) ve değer semantiği olduğu söylenir. Sınıflar, diğer taraftan, başvuru türleridir ([başvuru türleri](types.md#reference-types)) ve başvuru semantiği olduğu söylenir.

Bir sınıf türünün bir değişkeni verilerin ikinci bilinen bir nesne olarak başvuru içerirken bir yapı türünün değişkenini doğrudan, struct'ın verileri içerir. Bir struct olduğunda `B` bir örnek alan türü içeren `A` ve `A` bir yapı türü için bir derleme zamanı hata `A` bağlıdır `B` veya bir tür oluşturulan `B`. Bir yapı `X` ***doğrudan bağlı*** yapı `Y` varsa `X` bir örnek alan türü içeren `Y`. Bu tanımı verildiğinde, tam bir yapı olduğu yapıları geçişli kapatılmasını kümesidir ***doğrudan bağlı*** ilişki.  Örneğin:
```csharp
struct Node
{
    int data;
    Node next; // error, Node directly depends on itself
}
```
bir hata olduğundan `Node` kendi türünde bir örnek alanı içerir.  Başka bir örnek
```csharp
struct A { B b; }

struct B { C c; }

struct C { A a; }
```
bir hata olduğundan türlerinin her biri `A`, `B`, ve `C` birbirine bağımlı.

Sınıfları ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur. Yapılar ile her değişkenleri kendi veri kopyası vardır (dışındaki durumunda `ref` ve `out` parametresi değişkenleri), ve işlemlerin bir diğerini etkilemesi olanaklı değildir. Yapılar, başvuru türleri değil, ayrıca, mümkün olması için bir yapı türü değerleri için değildir, çünkü `null`.

Verilen bildirimi
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
değeri çıkarır `10`. Atamasını `a` için `b` değeri bir kopyasını oluşturur ve `b` böylece atamaya tarafından etkilenmez `a.x`. Vardı `Point` bunun yerine silinmiş bir sınıf olarak bildirilen, çıktı olacaktır `100` çünkü `a` ve `b` aynı nesneye başvuru.

### <a name="inheritance"></a>Devralma

Tüm yapı türleri örtülü olarak sınıftan `System.ValueType`, sırasıyla sınıfından devralan `object`. Yapı bildirimi uygulanan arabirimleri listesini belirtebilir, ancak bir taban sınıfı belirtmek bir yapı bildirimi için mümkün değildir.

Yapı türleri, hiçbir zaman soyuttur ve her zaman örtük olarak korumalı. `abstract` Ve `sealed` değiştiriciler bu nedenle izin verilmiyor bir struct bildiriminde.

Öğesinin bildirilen erişilebilirliği yapı üyesi devralma yapılar için desteklenmiyor. bu yana olamaz `protected` veya `protected internal`.

Bir yapı içinde işlev üyeleri olamaz `abstract` veya `virtual`ve `override` değiştiriciye izin yalnızca devralınan yöntemleri geçersiz kılmak için `System.ValueType`.

### <a name="assignment"></a>Atama

Bir yapı türü bir değişkene atanması, atanan değerin bir kopyasını oluşturur. Bu başvuru ancak başvuru tarafından tanımlanan nesnesi değil kopyalar bir sınıf türünde bir değişken için bir atama farklıdır.

Benzer şekilde bir struct olduğunda veya değer parametre olarak geçen işlevi üyesi bir sonuç döndürdü atama, struct'ın bir kopyası oluşturulur. Bir yapı kullanarak bir işlev üyeye başvuruya göre geçirilebilir bir `ref` veya `out` parametresi.

Bir özellik veya dizin oluşturucu bir yapının atama hedefi olduğunda özellik veya dizin oluşturucu erişimiyle ilişkili örnek ifade bir değişken olarak sınıflandırılan gerekir. Örnek ifade bir değer olarak sınıflandırılır, bir derleme zamanı hatası oluşur. Bu daha ayrıntılı anlatılan [basit atama](expressions.md#simple-assignment).

### <a name="default-values"></a>Varsayılan değerler

Bölümünde anlatıldığı gibi [varsayılan değerler](variables.md#default-values), oluşturulduğunda varsayılan değerlerine çeşitli türlerde değişkenleri otomatik olarak başlatılır. Sınıf türleri ve diğer başvuru türlerinin değişkenleri için bu varsayılan değerdir `null`. Ancak, yapılar olamaz değer türleri olduğundan `null`, bir yapı varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarlayarak üretilen değeridir `null`.

Başvuran `Point` yapısı, yukarıdaki örnekte bildirilen
```csharp
Point[] a = new Point[100];
```
Her başlatır `Point` değerine ayarlayarak üretilen dizideki `x` ve `y` sıfıra alanları.

Bir yapı varsayılan değerini struct'ın varsayılan oluşturucu tarafından döndürülen değer karşılık gelir ([varsayılan oluşturucular](types.md#default-constructors)). Bir sınıf, yapı, parametresiz örnek oluşturucusu bildirmek için izin verilmez. Bunun yerine, her yapı her zaman tüm değer tür alanları varsayılan değerlerine ve tüm başvuru türü alanlarını ayarlama özelliğinden sonucunda değeri döndüren bir parametresiz örnek oluşturucusu örtük olarak sahip `null`.

Yapılar varsayılan başlatma durumu geçerli bir durum düşünün şekilde tasarlanmalıdır. Örnekte
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
örnek kullanıcı tanımlı oluşturucusu yalnızca burada açıkça çağrıldığı null değerler karşı korur. Durumlarda burada bir `KeyValuePair` değişkeni ise varsayılan değer başlatma tabi `key` ve `value` alanları null olacaktır ve struct bu durumunu işlemek için hazırlanması gerekir.

### <a name="boxing-and-unboxing"></a>Kutulama ve kutudan çıkarma

Bir sınıf türünde bir değer türüne dönüştürülebilir `object` veya başka bir türü derleme zamanında yalnızca başvuru düşünerek sınıfı tarafından uygulanan bir arabirim türü. Benzer şekilde, bir değer türü `object` veya bir arabirim türü değeri başvurusu değiştirmeden bir sınıf türüne dönüştürülebilir (ancak Elbette bir çalışma zamanı tür denetimi bu durumda gereklidir).

Yapılar başvuru türleri olmadığından, bu işlemleri farklı yapı türleri için uygulanır. Ne zaman bir yapı türünün bir değer türüne dönüştürülür `object` veya struct tarafından uygulanan bir arabirim türü için bir paketleme işlemi gerçekleşir. Benzer şekilde, bir değer türü `object` veya bir yapı türünü bir arabirim türü değeri dönüştürülür, bir kutudan çıkarma işlemi gerçekleşir. Sınıf türleri aynı işlemleri önemli bir fark, kutulama ve kutudan çıkarma yapı değeri veya paketlenmiş örneği dışından kopyalar ' dir. Bu nedenle, kutulama veya kutudan çıkarma işlemi, kutulanmış struct kutulanmamış struct için yapılan değişiklikler yansıtılmaz.

Bir yapı türü devralınan sanal bir yöntem olduğunda geçersiz kılmalar `System.Object` (gibi `Equals`, `GetHashCode`, veya `ToString`), yapı türünün bir örneği üzerinden sanal yöntemi çağırmayı kutulama oluşmasına neden olmaz. Bu yapı türü parametre olarak kullanılır ve çağırma parametre türü bir örneği üzerinden gerçekleşir bile geçerlidir. Örneğin:
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

Program çıktısı şöyledir:
```
1
2
3
```

Hatalı stili olmasına rağmen `ToString` yan etkileri olduğu için örnek hiçbir kutulama için üç çağrılarını oluştuğunu gösterir `x.ToString()`.

Benzer şekilde, hiçbir zaman örtük olarak kutulama kısıtlanmış bir tür parametresi üzerindeki bir üyeye erişilmesi meydana gelir. Örneğin, bir arabirim varsayalım `ICounter` bir yöntem içerir `Increment` değeri değiştirmek için kullanılabilir. Varsa `ICounter` yürütmesinin kısıtlama olarak kullanılan `Increment` değişken başvuru ile yöntemi çağrıldığında, `Increment` paketlenmiş bir kopyasını, hiçbir zaman çağrıldı.

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

İlk çağrıda `Increment` değişken içindeki değeri değiştirir `x`. Bu ikinci çağrının eşdeğer değildir `Increment`, paketlenmiş bir kopyasını değeri değiştirir `x`. Bu nedenle, program çıktısı şöyledir:
```
0
1
1
```

Kutulama ve kutudan çıkarma hakkında daha fazla bilgi için bkz. [kutulama ve kutudan çıkarma](types.md#boxing-and-unboxing).

### <a name="meaning-of-this"></a>Bunun anlamı

Bir örnek oluşturucusu veya örnek işlevi bir sınıf üyesi `this` bir değer olarak sınıflandırılır. Bu nedenle, while `this` örneğine başvurmak için kullanılabilir olan işlevi üyenin çağrıldığı için atamak mümkün değildir `this` bir sınıf üyesi işlevi olarak.

Bir örnek oluşturucusunda bir yapı içinde `this` karşılık gelen bir `out` parametresi yapı türünün ve örnek işlev üyesi bir yapı içinde `this` karşılık gelen bir `ref` yapı türünde parametre. Her iki durumda da `this` bir değişken olarak sınıflandırılır ve kendisi için işlev üyenin çağrıldığı atayarak yapının tamamını değiştirmek mümkündür `this` veya olarak geçirerek bir `ref` veya `out` parametresi.

### <a name="field-initializers"></a>Alan başlatıcıları

Bölümünde anlatıldığı gibi [varsayılan değerler](structs.md#default-values), bir yapı varsayılan değer tür alanları için tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarından sonucunda değeri oluşur `null`. Bu nedenle, bir yapı değişken başlatıcılar eklemek için örnek alanı bildirimleri izin vermez. Bu kısıtlama yalnızca örnek alanları için geçerlidir. Bir yapının statik alanlar, değişken başlatıcılar içerecek şekilde izin verilir.

Örnek
```csharp
struct Point
{
    public int x = 1;  // Error, initializer not permitted
    public int y = 1;  // Error, initializer not permitted
}
```
örnek alanı bildirimleri değişken başlatıcılar içerdiğinden hatasıdır.

### <a name="constructors"></a>Oluşturucular

Bir sınıf, yapı, parametresiz örnek oluşturucusu bildirmek için izin verilmez. Bunun yerine, her yapı her zaman null tür alanları tüm değer tür alanları varsayılan değerlerine ve tüm başvuru ayarından sonucunda değeri döndüren bir parametresiz örnek oluşturucusu örtük olarak sahip ([varsayılan oluşturucular](types.md#default-constructors)). Örnek oluşturucuları parametreleri olan bir yapı bildirebilirsiniz. Örneğin:
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

Yukarıdaki bildirim, deyimleri verilen
```csharp
Point p1 = new Point();
Point p2 = new Point(0, 0);
```
her ikisini de oluşturma bir `Point` ile `x` ve `y` sıfır olarak başlatılır.

Bir yapı örnek oluşturucusu biçiminde bir oluşturucu başlatıcı eklemek için izin verilmiyor `base(...)`.

Yapı örnek oluşturucusu bir oluşturucu başlatıcı belirtmiyorsa `this` değişken karşılık gelen bir `out` parametresi yapı türünün ve benzer bir `out` parametresi `this` kesinlikle atanmalıdır ( [Belirli atama onayına](variables.md#definite-assignment)) Oluşturucu döndüğü her konumda. Yapı örnek oluşturucusu bir oluşturucu başlatıcı belirtiyorsa `this` değişken karşılık gelen bir `ref` parametresi yapı türünün ve benzer bir `ref` parametresi `this` atanan kesin olarak kabul edilir Oluşturucu body girişi. Aşağıdaki örnek oluşturucusu uygulaması göz önünde bulundurun:
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

Hiçbir örnek üyesi işlevini (set erişimcisine özellikleri dahil olmak üzere `X` ve `Y`) yapılandırılmakta yapının tüm alanlarını kesinlikle atanmış kadar çağrılamaz. Tek özel durum otomatik olarak uygulanan özellikler içerir ([Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties)). Belirli atama onayına kuralları ([basit atama ifadeleri](variables.md#simple-assignment-expressions)) özellikle atama otomatik özellik için bir yapı türünün bir örneği oluşturucu içinde yapı türü muaf: Böyle bir atama bir kesin olarak kabul edilir Otomatik-özellik gizli yedekleme alanının atama. Bu nedenle, aşağıdaki izin verilir:

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

Bir yapının bir yıkıcı bildirmek için izin verilmez.

### <a name="static-constructors"></a>Statik oluşturucular

Yapılar için statik oluşturucular sınıfları olduğu gibi aynı kurallara çoğunu izleyin. Bir yapı türü için bir statik Oluşturucu yürütülmesi, bir uygulama etki alanı içinde gerçekleşmesi için ilk aşağıdaki olaylardan biri tarafından tetiklenir:

*  Yapı türünün statik üyesi başvuruluyor.
*  Yapı türünün açıkça bildirilen bir oluşturucu çağrılır.

Varsayılan değerleri oluşturulmasını ([varsayılan değerler](structs.md#default-values)) struct'ın türleri statik Oluşturucu tetiklemez. (Buna örnek olarak bir dizideki öğelerin ilk değeri var.)

## <a name="struct-examples"></a>Yapı örnekleri

Aşağıdakileri kullanarak iki önemli örnek gösterir `struct` dilinin ancak değiştirilen semantiğine sahip önceden tanımlanmış türlerine benzer şekilde kullanılabilir türleri oluşturma türleri.

### <a name="database-integer-type"></a>Veritabanı tamsayı türü

`DBInt` Yapısı aşağıdaki değerleri tam kümesini temsil edebilen bir tamsayı türü uygulayan `int` türü yanı sıra, bilinmeyen bir değere gösteren bir ek durumu. Bu özelliklere sahip bir tür veritabanlarında yaygın olarak kullanılır.

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

`DBBool` Yapısı aşağıdaki üç değerli mantıksal türü uygular. Bu tür olası değerler şunlardır: `DBBool.True`, `DBBool.False`, ve `DBBool.Null`burada `Null` üye bilinmeyen bir değere gösterir. Bu üç değerli mantıksal türler veritabanlarında yaygın olarak kullanılır.

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
