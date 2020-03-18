---
ms.openlocfilehash: 49720d62c496ff904eacacb31ae29ef97a5aefaa
ms.sourcegitcommit: 8152182f0a477cb3082e625b607262cc459a17f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "79484946"
---
# <a name="records"></a>kayıtlar

* [x] önerilir
* [] Prototip: [Tamam](https://github.com/PROTOTYPE_OWNER/roslyn/BRANCH_NAME)
* [] Uygulama: [sürüyor](https://github.com/dotnet/roslyn/BRANCH_NAME)
* [] Belirtimi: eklenen taslak belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

Kayıtlar, bir dizi daha basit özelliğin avantajlarını birleştiren C# sınıf ve yapı türleri için yeni ve basitleştirilmiş bir bildirim formudur. Yeni özellikleri (çağıran alıcı parametreleri ve *ile ifadesi*) anladık, kayıt bildirimleri için söz dizimi ve semantiğe ve sonra bazı örnekler sunarız.


## <a name="motivation"></a>Amacı
[motivation]: #motivation

İçinde C# önemli sayıda tür bildirimi, yazılı verilerin toplam koleksiyonlarından çok daha fazla. Ne yazık ki, böyle bir tür bildirirken ortak kod için harika bir işlem yapılması gerekir. *Kayıtlar* , toplama üyelerini, varsa her zamanki ortak kod veya sapmaları açıklayarak tanımlamaya yönelik bir mekanizma sağlar.

Aşağıdaki [örneklere](#examples)bakın.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

### <a name="caller-receiver-parameters"></a>arayan alıcı parametreleri

Şu anda bir yöntem parametresinin *Varsayılan bağımsız değişkeni* olmalıdır 
- *sabit ifade*; veya
- formun bir ifadesi, `S` bir değer türüdür `new S()`; veya
- formun bir ifadesi `default(S)` `S` bir değer türüdür

Şunu eklemek için bunu genişlettik
- formun bir ifadesi `this.Identifier`

Bu yeni form, *çağıran alıcı varsayılan bağımsız değişkeni*olarak adlandırılır ve yalnızca aşağıdakilerin tümü karşılandığında izin verilir
- İçinde göründüğü Yöntem bir örnek yöntemidir; '
- İfade `this.Identifier`, bir alan veya özellik olması gereken kapsayan türün bir örnek üyesine bağlanır; '
- Bağlandığı üye (ve bir özellik ise `get` erişimcisi) en az Yöntem olarak erişilebilir olur; '
- `this.Identifier` türü, örtük olarak bir kimliğe dönüştürülebilir veya parametrenin türüne null değer atanabilir dönüştürmeye (Bu, *Varsayılan bağımsız değişkende*var olan bir kısıtlamadır).

Bir bağımsız değişken, *çağıran-alıcı varsayılan bağımsız değişkenli*karşılık gelen bir isteğe bağlı parametre için bir işlev üyesinin çağrısından atlandığında, alıcı üyesinin değeri örtük olarak geçirilir. 

> **Tasarım Notları**: arayan alıcı parametresinin ana nedeni *WITH ifadesi*' i desteklemelidir. Buradaki gibi bir yöntemi bildirebilmeniz önerilir
> ```cs
> class Point
> {
>     public readonly int X;
>     public readonly int Y;
>     public Point With(int x = this.X, int y = this.Y) => new Point(x, y);
>     // etc
> }
> ```
> daha sonra bunu bu şekilde kullanın
> ```cs
>     Point p = new Point(3, 4);
>     p = p.With(x: 1);
> ```
> Var olan bir `Point` gibi yeni bir `Point` oluşturmak için `X` değeri değişti.
> 
> Bu bir açık sorudır ve *WITH ifadesi* için, çağıran alıcı parametrelerine yönelik destek edindikten sonra *bunun yerine* bu şekilde ekleme yapıp yapmadığımızda *,* *WITH ifadesi*yerine bunu yapmanız mümkün değildir.

- [] **Açık sorun**: bir *arayan alıcı varsayılan bağımsız değişkeninin* diğer bağımsız değişkenlere göre değerlendirildiği sıra nedir? Belirtilmemiş olduğunu söyliyoruz mi?

### <a name="with-expressions"></a>with ifadeleri

Yeni bir ifade formu önerilir:

```antlr
primary_expression
    : with_expression
    ;

with_expression
    : primary_expression 'with' '{' with_initializer_list '}'
    ;

with_initializer_list
    : with_initializer
    | with_initializer ',' with_initializer_list
    ;

with_initializer
    : identifier '=' expression
    ;
```

Belirteç `with`, bağlama duyarlı yeni bir anahtar sözcüktür.

*With_initializer* solundaki her *tanımlayıcı* , *with_expression* *primary_expression* türünün erişilebilir bir örnek alanına veya özelliğine bağlanmalıdır. Belirli bir *with_expression*bu tanımlayıcılar arasında yinelenen ad olmayabilir.

Formun *with_expression*

> *e1* `with` `{` *tanımlayıcı* = *e2*,... `}`

Form çağrısı olarak değerlendirilir

> *e1*`.With(`*identifier2*`:` *e2*,...`)`

Burada, `With` adlı her bir yöntem için, bu yöntemin erişilebilir bir örnek üyesi olan her *yöntemin, bu*yöntemdeki örnek alan veya *özellik ile aynı*üye olan bir arayan alıcı parametresine sahip olan ilk parametrenin adı olarak *identifier2* ' i seçeceğiz. Böyle bir parametre tanımlanamıyorsa yöntemin göz önünde bulundurulmaz. Çağrılacak yöntem, aşırı yükleme çözünürlüğünden kalan adaylar arasından seçilir.

> **Tasarım Notları**: çağıran alıcı parametreleri, bu özel söz dizimi formu olmadan *WITH ifadesinin* avantajlarından birçoğu kullanılabilir. Bu nedenle, gerekli olup olmadığını düşünüyoruz. Temel avantajı, parametrelerin adları bakımından değil, bir programa, alanların ve özelliklerin adları açısından izin verir. Bu şekilde, her ikisi de okunabilirlik ve araç kalitesini geliştirdik (örn. bir *with_expression* tanımlayıcısına göre tanım, yöntem parametresi yerine özelliğe gidebilir).

- [] **Açık sorun**: Bu açıklama uzantı yöntemlerini destekleyecek şekilde değiştirilmelidir.

### <a name="pattern-matching"></a>desen eşleştirme

`Deconstruct` belirtimi ve onun düzeniyle eşleşen ilişkisi için bkz. [model eşleştirme belirtimi](csharp-8.0/patterns.md#positional-pattern) .

> **Tasarım Notları**: derleyici tarafından oluşturulan `Deconstruct` burada belirtilen şekilde sanallaştırarak ve model eşleme, bir kayıt bildirimi belirtimi
> ```cs
> public class Point(int X, int Y);
> ```
> Konumsal model eşleştirmeyi aşağıdaki gibi destekleyecektir
> ```cs
> Point p = new Point(3, 4);
> if (p is Point(3, var y)) { // if X is 3
>     Console.WriteLine(y);   // print Y
> }
> ```

### <a name="record-type-declarations"></a>Kayıt türü bildirimleri

`class` veya `struct` bildirimi için sözdizimi, değer parametrelerini destekleyecek şekilde genişletilir; Parametreler, türün özellikleri olur:

```antlr
class_declaration
    : attributes? class_modifiers? 'partial'? 'class' identifier type_parameter_list?
      record_parameters? record_class_base? type_parameter_constraints_clauses? class_body
    ;

struct_declaration
    : attributes? struct_modifiers? 'partial'? 'struct' identifier type_parameter_list?
      record_parameters? struct_interfaces? type_parameter_constraints_clauses? struct_body
    ;

record_class_base
    : class_type record_base_arguments?
    | interface_type_list
    | class_type record_base_arguments? ',' interface_type_list
    ;

record_base_arguments
    : '(' argument_list? ')'
    ;

record_parameters
    : '(' record_parameter_list? ')'
    ;

record_parameter_list
    : record_parameter
    | record_parameter ',' record_parameter_list
    ;

record_parameter
    : attributes? type identifier record_property_name? default_argument?
    ;

record_property_name
    : ':' identifier
    ;

class_body
    : '{' class_member_declarations? '}'
    | ';'
    ;

struct_body
    : '{' struct_members_declarations? '}'
    | ';'
    ;
```

> **Tasarım Notları**: kayıt türleri genellikle sınıf gövdesinde açıkça belirtilen herhangi bir üyeye gerek olmadan yararlı olduğundan, bir gövdenin sözdizimini yalnızca noktalı virgül olacak şekilde değiştirirsiniz.

Kayıt *parametreleriyle* belirtilen bir sınıf (struct), biri *kayıt türü*olan bir *kayıt sınıfı* (*kayıt yapısı*) olarak adlandırılır.

- [] **Açık sorun**: bir kayıt türü bildiriminin içinde görünebilmesi için *primary_constructor_body* dilbilgisinde yer almalıdır.
- [] **Açma sorunu**: parametre adları için ad çakışması kuralları nelerdir? Bir tür parametresiyle veya başka bir *kayıt parametresiyle*çakışmayla izin verilmez.
- [] **Açma sorunu**: kayıt parametrelerinin kapsamını belirtmemiz gerekir. Nerede kullanılabilir? Örnek alanı başlatıcıları ve en azından *primary_constructor_body* içinde görünür.
- [] **Açık sorun**: bir kayıt türü bildirimi kısmi olabilir mi? Varsa, parametrelerin her bölümde yinelenmeleri gerekir mi?

#### <a name="members-of-a-record-type"></a>Bir kayıt türünün üyeleri

*Sınıf gövdesinde*belirtilen üyelere ek olarak, bir kayıt türü aşağıdaki ek üyelere sahiptir:

##### <a name="primary-constructor"></a>Birincil Oluşturucu

Bir kayıt türünün, imzası tür bildiriminin değer parametrelerine karşılık gelen `public` Oluşturucusu vardır. Bu, türü için *birincil Oluşturucu* olarak adlandırılır ve örtük olarak belirtilen *varsayılan oluşturucunun* gizlenmesine neden olur.

Çalışma zamanında birincil Oluşturucu

* değer parametrelerine karşılık gelen özellikler için derleyici tarafından oluşturulan yedekleme alanlarını başlatır (Bu özellikler derleyici tarafından sağlanmışsa). [bkz. 1.1.2](#1.1.2)); ni
* *sınıf gövdesinde*görünen örnek alanı başlatıcıları 'nı yürütür; ve ardından
* bir temel sınıf oluşturucusunu çağırır:
    * *Record_base_arguments*bağımsız değişkenler varsa, bu bağımsız değişkenlerle aşırı yükleme çözümlemesi tarafından seçilen bir temel oluşturucu çağrılır;
    * Aksi halde, bir temel Oluşturucu bağımsız değişken olmadan çağrılır.
* , varsa, kaynak sırada her bir *primary_constructor_body*gövdesini yürütür.

- [] **Açık sorun**: Bu sırayı, özellikle de parmalar için derleme birimleri arasında belirtmemiz gerekir.
- [] **Açma sorunu**: açıkça belirtilen her oluşturucunun birincil oluşturucuya zincirlemesi gerektiğini belirtmemiz gerekir.
- [] **Açık sorun**: birincil oluşturucuda erişim değiştiricisinin değiştirilmesine izin verilmesi gerekir mi?
- [] **Açma sorunu**: bir kayıt yapısı içinde, hiçbir kayıt parametresi olmaması hatadır mi?

##### <a name="primary-constructor-body"></a>Birincil Oluşturucu gövdesi

```antlr
primary_constructor_body
    : attributes? constructor_modifiers? identifier block
    ;
```

*Primary_constructor_body* , yalnızca bir kayıt türü bildiriminde kullanılabilir. Bir *primary_constructor_body* *tanımlayıcısı* , bildirildiği kayıt türünü olarak adı olacaktır.

*Primary_constructor_body* kendi kendine üye bildirmiyor, ancak Programcı 'nin öznitelikleri sağlaması ve bir kayıt türünün birincil oluşturucusunun erişimini belirtmesi için bir yoldur. Ayrıca, programcının bir kayıt türü örneği oluşturulduğunda yürütülecek ek kod sağlamasına de olanak sağlar.

- [] **Açık sorun**: bir struct varsayılan oluşturucusunun bunu atladığını unutmamalısınız.
- [] **Açma sorunu**: başlatmanın yürütme sırasını belirttik.
- [] **Açma sorunu**: bir kayıt olmayan tür bildiriminde *primary_constructor_body* (öznitelikler ve değiştiriciler olmadan) gibi bir şeye izin vermemiz ve bir örnek alanı başlatıcısının kodu yaptığımız gibi davranmız gerekir mi?

##### <a name="properties"></a>Özellikler

Bir kayıt türü bildiriminin her bir kayıt parametresi için, Name ve Type değeri Parameter bildiriminden alınan karşılık gelen bir `public` Property üyesi vardır. Bunun adı, varsa *identifier* *record_property_name*, varsa *record_parameter* *tanımlayıcısıdır* . `get` erişimcisine sahip ve bu ad ve türe sahip bir somut (yani soyut olmayan) ortak özelliği yoksa, derleyici tarafından aşağıdaki gibi üretilir:

* Bir *kayıt yapısı* veya `sealed` *kayıt sınıfı*için:
 * Bir `private` `readonly` alanı bir `readonly` özelliği için bir yedekleme alanı olarak üretilir. Değeri, oluşturma sırasında karşılık gelen birincil Oluşturucu parametresinin değeri ile başlatılır.
 * Özelliğin `get` erişimcisi, yedekleme alanının değerini döndürecek şekilde uygulanır.
 * Devralınan her "eşleşen" sanal özelliğin `get` erişimcisi geçersiz kılındı.

> **Tasarım Notları**: diğer bir deyişle, bir temel sınıfı genişletebilir veya aynı ada sahip ortak soyut özelliği bildiren bir arabirim ve bir kayıt parametresi olarak yazın, bu özellik geçersiz kılınır veya uygulanır.

- [] **Açık sorun**: açıkça bildirildiği zaman bir özelliğin erişim değiştiricisini değiştirmek mümkün olmalıdır mi?
- [] **Açık sorun**: bir özellik için bir alanı değiştirmek mümkün olmalıdır mi?

##### <a name="object-methods"></a>Nesne yöntemleri

Bir *kayıt yapısı* veya `sealed` *kayıt sınıfı*için, `object.GetHashCode()` ve `object.Equals(object)` yöntemlerinin uygulamaları, Kullanıcı tarafından sağlanmadığı takdirde derleyici tarafından üretilir.

- [] **Açık sorun**: uygulamasını tam olarak belirtmemiz gerekir.
- [] **Açık sorun**: Ayrıca, kayıt türü için arabirim `IEquatable<T>` eklemeli ve uygulamaların sağlandığını belirtmelisiniz.
- [] **Açık sorun**: her `IEquatable<T>.Equals`uyguladığımızda de belirteceğiz.
- [] **Açık sorun**: tam olarak, kayıt devralma aşamasında eşittir sorununu çözeceğiz: özel olarak simetrik, geçişli, yansımalı, vb. gibi eşitlik yöntemleri üretme.
- [] **Açık sorun**: kayıt türleri için `operator ==` ve `operator !=` uyguladığımızda teklif edildi.
- [] **Açık sorun**: `object.ToString`bir uygulamasını otomatik olarak oluşturmamız gerekir mi?

##### `Deconstruct`

Kullanıcı tarafından herhangi bir imzaya sahip olmadığı müddetçe bir kayıt türü derleyicinin ürettiği `public` yöntemine sahiptir `void Deconstruct`. Her parametre aynı ada sahip bir `out` parametresidir ve kayıt türünün karşılık gelen parametresiyle aynı türde. Bu yöntemin derleyici tarafından sağlanmış uygulanması, her bir `out` parametresini karşılık gelen özelliğin değeriyle atayacaktır.

`Deconstruct`semantiği için bkz. [model eşleme belirtimi](csharp-8.0/patterns.md#positional-pattern) .

##### <a name="with-method"></a>`With` yöntemi

`With` bildirildiği adlı Kullanıcı tarafından belirtilen bir üye olmadıkça, kayıt türü, dönüş türü kayıt türü olan `With` adlı derleyici tarafından sağlanmış bir yönteme sahiptir ve bu parametrelerin kayıt türü bildiriminde göründüğü sırada her bir *Record parametresi* ile eşleşen bir değer parametresi içerir. Her parametrenin, karşılık gelen özelliğe ait bir *arayan alıcı varsayılan bağımsız değişkeni* olacaktır.

`abstract` bir kayıt sınıfında, derleyici tarafından sağlanmış `With` yöntemi soyuttur. Bir kayıt yapısına veya korumalı bir kayıt sınıfında, derleyici tarafından sağlanmış `With` yöntemi `sealed`. Aksi takdirde, derleyici tarafından sağlanmış `With` yöntemi ' sanal ' dir ve bunun uygulanması, parametrelerden yeni bir örnek oluşturmak için bağımsız değişken olarak parametrelere sahip birincil oluşturucuyu çağırarak oluşturulan yeni bir örneği döndürür ve bu yeni örneği döndürür.

- [] **Açık sorun**: aynı zamanda devralınan sanal `With` yöntemleri veya uygulanan arabirimlerden `With` yöntemleri hangi koşullarda belirttiğimiz veya uygulamamız gerekir.
- [] **Açık sorun**: sanal olmayan bir `With` yöntemi devraldığı zaman ne olacağını söylemelidir.

> **Tasarım Notları**: kayıt türleri varsayılan olarak sabit olduğu için `With` yöntemi, var olan bir örnekle aynı ancak seçili özelliklerle yeni değerler verilen yeni bir örnek oluşturma yolunu sağlar. Örneğin, verilen
> ```cs
> public class Point(int X, int Y);
> ```
> derleyici tarafından belirtilen üye var
> ```cs
>     public virtual Point With(int X = this.X, int Y = this.Y) => new Point(X, Y);
> ```
> Kayıt türü değişkenini sağlayan
> ```cs
> var p = new Point(3, 4);
> ```
> bir veya daha fazla özelliği farklı olan bir örnekle değiştirilmesini sağlamak için
> ```cs
>     p = p.With(X: 5);
> ```
> Bu da *with_expression*kullanılarak ifade edilebilir:
> ```cs
>     p = p with { X = 5 };
> ```

### <a name="examples"></a>Örnekler

#### <a name="compatibility-of-record-types"></a>Kayıt türlerinin uyumluluğu

Programcı bir kayıt türü bildirimine üye ekleyebildiğinden, varolan istemcileri etkilemeden kayıt öğelerinin kümesini değiştirmek genellikle mümkündür. Örneğin, bir kayıt türünün ilk sürümü verildiğinde

```cs
// v1
public class Person(string Name, DateTime DateOfBirth);
```

Kayıt türünün yeni bir öğesi, ikili veya kaynak uyumluluğunu etkilemeden türün bir sonraki düzeltmesine göre uyumluluk sağlayabilir:

```cs
// v2
public class Person(string Name, DateTime DateOfBirth, string HomeTown)
{
    // Note: below operations added to retain binary compatibility with v1
    public Person(string Name, DateTime DateOfBirth) : this(Name, DateOfBirth, string.Empty) {}
    public static void operator is(Person self, out string Name, out DateTime DateOfBirth)
        { Name = self.Name; DateOfBirth = self.DateOfBirth; }
    public Person With(string Name, DateTime DateOfBirth) => new Person(Name, DateOfBirth);
}
```

#### <a name="record-struct-example"></a>kayıt yapısı örneği

Bu kayıt yapısı

```cs
public struct Pair(object First, object Second);
```

Bu koda çevrilir

```cs
public struct Pair : IEquatable<Pair>
{
    public object First { get; }
    public object Second { get; }
    public Pair(object First, object Second)
    {
        this.First = First;
        this.Second = Second;
    }
    public bool Equals(Pair other) // for IEquatable<Pair>
    {
        return Equals(First, other.First) && Equals(Second, other.Second);
    }
    public override bool Equals(object other)
    {
        return (other as Pair)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (First?.GetHashCode()*17 + Second?.GetHashCode()).GetValueOrDefault();
    }
    public Pair With(object First = this.First, object Second = this.Second) => new Pair(First, Second);
    public void Deconstruct(out object First, out object Second)
    {
        First = self.First;
        Second = self.Second;
    }
}
```

- [] **Açık sorun**: eşittir (diğer çifti) uygulanması, Çift çiftinin ortak üyesi olmalıdır mi?
- [] **Açma sorunu**: Bu `Equals` uygulanması devralma aşamasında simetrik değildir.
- [] **Açık sorun**: bir kayıt `operator ==` ve `operator !=`bildirmelidir mi?

> **Tasarım Notları**: bir kayıt türü diğerinden devraldığı ve bu `Equals` uygulanması bu durumda simetrik olmadığından, doğru değildir. Bu şekilde eşitlik uygulamayı öneriyoruz:
> ```cs
>     public bool Equals(Pair other) // for IEquatable<Pair>
>     {
>         return other != null && EqualityContract == other.EqualityContract &&
>             Equals(First, other.First) && Equals(Second, other.Second);
>     }
>     protected virtual Type EqualityContract => typeof(Pair);
> ```
> Türetilmiş kayıtlar `override EqualityContract`. Daha az çekici bir alternatif, devralmayı kısıtlamadır.

#### <a name="sealed-record-example"></a>korumalı kayıt örneği

Bu korumalı kayıt sınıfı

```cs
public sealed class Student(string Name, decimal Gpa);
```

Bu koda çevrilir

```cs
public sealed class Student : IEquatable<Student>
{
    public string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public bool Equals(Student other) // for IEquatable<Student>
    {
        return other != null && Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public override bool Equals(object other)
    {
        return this.Equals(other as Student);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa?.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public void Deconstruct(out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

#### <a name="abstract-record-class-example"></a>Soyut kayıt sınıfı örneği

Bu soyut kayıt sınıfı

```cs
public abstract class Person(string Name);
```

Bu koda çevrilir

```cs
public abstract class Person : IEquatable<Person>
{
    public string Name { get; }
    public Person(string Name)
    {
        this.Name = Name;
    }
    public bool Equals(Person other)
    {
        return other != null && Equals(Name, other.Name);
    }
    public override bool Equals(object other)
    {
        return Equals(other as Person);
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()).GetValueOrDefault();
    }
    public abstract Person With(string Name = this.Name);
    public void Deconstruct(Person self, out string Name)
    {
        Name = self.Name;
    }
}
```

#### <a name="combining-abstract-and-sealed-records"></a>soyut ve korumalı kayıtları birleştirme

Yukarıdaki `Person` soyut kayıt sınıfı verildiğinde, bu korumalı kayıt sınıfı

```cs
public sealed class Student(string Name, decimal Gpa) : Person(Name);
```

Bu koda çevrilir

```cs
public sealed class Student : Person, IEquatable<Student>
{
    public override string Name { get; }
    public decimal Gpa { get; }
    public Student(string Name, decimal Gpa) : base(Name)
    {
        this.Name = Name;
        this.Gpa = Gpa;
    }
    public override bool Equals(Student other) // for IEquatable<Student>
    {
        return Equals(Name, other.Name) && Equals(Gpa, other.Gpa);
    }
    public bool Equals(Person other) // for IEquatable<Person>
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override bool Equals(object other)
    {
        return (other as Student)?.Equals(this) == true;
    }
    public override int GetHashCode()
    {
        return (Name?.GetHashCode()*17 + Gpa.GetHashCode()).GetValueOrDefault();
    }
    public Student With(string Name = this.Name, decimal Gpa = this.Gpa) => new Student(Name, Gpa);
    public override Person With(string Name = this.Name) => new Student(Name, Gpa);
    public void Deconstruct(Student self, out string Name, out decimal Gpa)
    {
        Name = self.Name;
        Gpa = self.Gpa;
    }
}
```

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

6 ' C# da *birincil oluşturucular* ekleme kabul ettik. Bu teklifle aynı sözdizimsel yüzeyi içerse de, kayıtlar tarafından sunulan avantajların ne kadar kısa olduğunu bulduk.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

Açık sorular teklifin gövdesinde görüntülenir.

## <a name="design-meetings"></a>Tasarım toplantıları

TBD
