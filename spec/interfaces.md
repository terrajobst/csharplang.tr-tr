---
ms.openlocfilehash: 0a09585f4f885647230354c66a2449abb7ef1f44
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488773"
---
# <a name="interfaces"></a>Arabirimler

Bir arabirim bir sözleşmeyi tanımlar. Bir sınıf ya da bir arabirimi uygulayan yapı bu sözleşmeye uymalıdır. Birden fazla temel Ara birimden arabirim devralabilir ve bir sınıf veya yapı birden fazla arabirim uygulayabilir.

Arabirim yöntemleri, özellikleri, olayları ve dizin oluşturucular içerebilir. Arabirim, kendisini tanımlayan üyeleri için uygulamaları sağlamaz. Arabirimi yalnızca sınıf ya da arabirimi uygulayan yapının tarafından sağlanması gereken üyeleri belirtir.

## <a name="interface-declarations"></a>Arabirim bildirimi

Bir *interface_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir arabirim türü bildirir.

```antlr
interface_declaration
    : attributes? interface_modifier* 'partial'? 'interface'
      identifier variant_type_parameter_list? interface_base?
      type_parameter_constraints_clause* interface_body ';'?
    ;
```

Bir *interface_declaration* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)) ve ardından isteğe bağlı bir dizi *interface_modifier*s ([arabirim değiştiriciler](interfaces.md#interface-modifiers)) ve ardından isteğe bağlı `partial` değiştiricisi anahtar sözcüğü ve ardından, `interface` ve *tanımlayıcı* arabirimi adları İsteğe bağlı izlenen *variant_type_parameter_list* belirtimi ([değişken türü parametre listeleri](interfaces.md#variant-type-parameter-lists)) ve ardından isteğe bağlı *interface_base* belirtimi ([temel arabirimleri](interfaces.md#base-interfaces)) ve ardından isteğe bağlı *type_parameter_constraints_clause*s belirtimi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) , ardından bir *interface_body* ([arabirim gövdesi](interfaces.md#interface-body)), noktalı virgül tarafından izlenen isteğe bağlı olarak.

### <a name="interface-modifiers"></a>Değiştiriciler arabirimi

Bir *interface_declaration* isteğe bağlı olarak bir dizi arabirimi değiştiriciler içerebilir:

```antlr
interface_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | interface_modifier_unsafe
    ;
```

Aynı değiştiricisi arabirim bildirimiyle birden çok kez görünmesi için bir derleme zamanı hatasıdır.

`new` Değiştiricisi bir sınıf içinde tanımlanmış arabirimlere yalnızca verilir. Bölümünde anlatıldığı gibi arabirimi aynı ada göre devralınmış bir üyeyi gizler belirtir [yeni değiştiricisini](classes.md#the-new-modifier).

`public`, `protected`, `internal`, Ve `private` değiştiriciler arabirimi erişilebilirliğini denetim. Arabirim bildirimi oluştuğu bağlama bağlı olarak bu değiştiriciler yalnızca bazıları izin verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).

### <a name="partial-modifier"></a>Partial değiştiricisi

`partial` Değiştiricisi gösteren bu *interface_declaration* kısmi türü bildirimi. Kapsayan ad alanı veya tür bildirimi içinde aynı ada sahip birden fazla kısmi bir arabirim bildirimi birleştirmek için forma bir arabirim bildirimi, aşağıdaki kurallar belirtilen [kısmi türlerinden](classes.md#partial-types).

### <a name="variant-type-parameter-lists"></a>Değişken türü parametre listeleri

Değişken türü parametre listeleri yalnızca arabirim ve temsilci türlerinin üzerinde oluşabilir. Sıradan fark *type_parameter_list*s'dir isteğe bağlı *variance_annotation* üzerinde her tür parametresi.

```antlr
variant_type_parameter_list
    : '<' variant_type_parameters '>'
    ;

variant_type_parameters
    : attributes? variance_annotation? type_parameter
    | variant_type_parameters ',' attributes? variance_annotation? type_parameter
    ;

variance_annotation
    : 'in'
    | 'out'
    ;
```

Varyans ek açıklama olup olmadığını `out`, tür parametresi olarak kabul edilir ***birlikte değişken***. Varyans ek açıklama olup olmadığını `in`, tür parametresi olarak kabul edilir ***değişken karşıtı***. Varyans ek açıklaması yoksa, tür parametresi olarak kabul edilir ***sabit***.

Örnekte
```csharp
interface C<out X, in Y, Z> 
{
  X M(Y y);
  Z P { get; set; }
}
```
`X` birlikte değişen olduğundan `Y` değişken karşıtı olduğu ve `Z` değişmezdir.

#### <a name="variance-safety"></a>Varyans güvenliği

Bir tür değişken açıklamalarını türü parametre listesinde oluşumunu yerleri burada türleri içinde türü bildirimi oluşabilir kısıtlar.

Bir tür `T` olduğu ***güvenli olmayan çıkış*** tutuyorsa aşağıdakilerden biri:

*  `T` değişken karşıtı parametre türüdür
*  `T` Çıkış güvenli olmayan öğe türüne sahip bir dizi türü
*  `T` bir arabirim veya temsilci türü `S<A1,...,Ak>` genel türünden oluşturulduğu `S<X1,...,Xk>` için en az bir where `Ai` aşağıdakilerden birini içerir:
   * `Xi` birlikte değişken veya sabit ve `Ai` çıkış güvenli değildir.
   * `Xi` değişken karşıtı veya sabit ve `Ai` giriş güvenlidir.
   
Bir tür `T` olduğu ***güvenli olmayan giriş*** tutuyorsa aşağıdakilerden biri:

*  `T` birlikte değişen türde parametresi
*  `T` Giriş güvenli olmayan öğe türüne sahip bir dizi türü
*  `T` bir arabirim veya temsilci türü `S<A1,...,Ak>` genel türünden oluşturulduğu `S<X1,...,Xk>` için en az bir where `Ai` aşağıdakilerden birini içerir:
   * `Xi` birlikte değişken veya sabit ve `Ai` giriş güvenli değildir.
   * `Xi` değişken karşıtı veya sabit ve `Ai` çıkış güvenli değildir.

Sezgisel, çıkış güvenli olmayan bir tür bir çıkış konumunda yasaktır ve giriş güvenli olmayan bir tür Giriş bir konumda kullanılamaz.

Bir tür ***çıkış güvenli*** çıkış güvenli, değilse ve ***giriş güvenli*** giriş güvenli değilse.

#### <a name="variance-conversion"></a>Varyans dönüştürme

(Ancak yine de tür bakımından güvenli) daha esnek yapılan dönüştürmeler için arabirim ve temsilci türlerinin değişken açıklamalarını amacı sağlamaktır. Bunun için örtük tanımlarını bitiş ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) ve açık dönüştürmeler ([açık dönüştürmeler](conversions.md#explicit-conversions)) olun şu şekilde tanımlanır varyansı-convertibility kavramı kullanın:

Bir tür `T<A1,...,An>` varyansı dönüştürülebilir olduğundan türüne `T<B1,...,Bn>` varsa `T` arabirim ya da bir temsilci türü değişken türünde parametreleri bildirilmiş `T<X1,...,Xn>`ve her değişken türünde bir parametre için `Xi` aşağıdakilerden birini Tutar:

*  `Xi` birlikte değişkendir ve gelen örtük bir başvuru veya kimlik dönüştürme var `Ai` için `Bi`
*  `Xi` değişken karşıtı ve örtük bir başvuru veya kimlik dönüştürme var gelen `Bi` için `Ai`
*  `Xi` değişmez değer ve bir kimlik dönüştürme var gelen `Ai` için `Bi`

### <a name="base-interfaces"></a>Temel arabirimleri

Bir arabirim olarak adlandırılan sıfır veya daha fazla arabirim türünden devralınabilir ***açık temel arabirimleri*** arabirimi. Bir arabirim bir veya daha fazla temel arabirimde açık olduğunda, ardından o arabirimin bildiriminde arabirim tanımlayıcısı bir iki nokta üst üste ve virgül gelir ayrılmış temel arabirim türleri listesi.

```antlr
interface_base
    : ':' interface_type_list
    ;
```

Genel tür bildiriminde taban açık arabirim bildirimi alma ve değiştirme, her biri için oluşturulmuş bir oluşturulmuş arabirim türü için açık temel arabirimleri *type_parameter* , temel arabirim bildirimi, karşılık gelen *type_argument* oluşturulan türü.

Bir arabirimin açık temel arabirimleri en az arabirimi olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)). Örneğin, belirtmek için bir derleme zamanı hatası olduğu bir `private` veya `internal` arabiriminde *interface_base* , bir `public` arabirimi.

Doğrudan veya dolaylı olarak kendisinden devralmak bir arabirim için bir derleme zamanı hatasıdır.

***Temel arabirimleri*** açık temel arabirimleri ve temel arabirimlerini bir arabirimi olan. Diğer bir deyişle, taban arabirimlerin açık temel arabirimleri, açık temel arabirimleri ve benzeri tam geçişli kapatılmasını kümesidir. Bir arabirim temel arabirimlerinden tüm üyelerini devralır. Örnekte
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
temel arabirimleri `IComboBox` olan `IControl`, `ITextBox`, ve `IListBox`.

Diğer bir deyişle, `IComboBox` yukarıdaki arabirim üyeleri devralan `SetText` ve `SetItems` yanı `Paint`.

Bir arabirimin temel her arabirim çıkış güvenli olmalıdır ([varyansı güvenliği](interfaces.md#variance-safety)). Bir sınıf veya yapı ayrıca açık bir arabirimi uygulayan tüm arabiriminin temel arabirim uygular.

### <a name="interface-body"></a>Gövde arabirimi

*İnterface_body* arabirimin üyelerini bir arabirimi tanımlar.

```antlr
interface_body
    : '{' interface_member_declaration* '}'
    ;
```

## <a name="interface-members"></a>Arabirim üyeleri

Bir arabirimin üyelerini temel Ara birimden devralınan üyeleri ve üye arabirim tarafından bildirilen.

```antlr
interface_member_declaration
    : interface_method_declaration
    | interface_property_declaration
    | interface_event_declaration
    | interface_indexer_declaration
    ;
```

Bir arabirim bildirimi, sıfır veya daha fazla üye bildirebilir. Yöntemleri, özellikleri, olayları ve dizin oluşturucular bir arabirim üyesi olmalıdır. Bir arabirim, sabitleri, alanları, işleçleri, örnek Oluşturucular, Yıkıcılar veya türleri içeremez veya bir arabirim herhangi bir türde statik üyeler içerebilir.

Tüm arabirim üyeleri, örtük olarak genel erişime sahiptir. Arabirim üye bildirimleri tüm değiştiricilere dahil etmek için bir derleme zamanı hatasıdır. Özellikle, arabirimler üye değiştiricilerini bildirilemez `abstract`, `public`, `protected`, `internal`, `private`, `virtual`, `override`, veya `static`.

Örnek
```csharp
public delegate void StringListEvent(IStringList sender);

public interface IStringList
{
    void Add(string s);
    int Count { get; }
    event StringListEvent Changed;
    string this[int index] { get; set; }
}
```
Her bir üye olası türleri içeren bir arabirim bildirir: Bir yöntem, özellik, bir olay ve dizin oluşturucu.

Bir *interface_declaration* yeni bir bildirim alanı oluşturur ([bildirimleri](basic-concepts.md#declarations)) ve *interface_member_declaration*hemen tarafındanbulunans*interface_declaration* bu bildirim alanına yeni üye tanıtır. Aşağıdaki kurallar geçerli *interface_member_declaration*: %s

*  Bir yöntemin adını, tüm özellikleri ve olayları aynı arabirimde bildirilen adlarından farklı olmalıdır. Ayrıca, imza ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir yöntem imzaları aynı arabirimde bildirilen diğer tüm yöntemlerden farklı gerekir ve aynı arabirimde bildirilen iki yöntem imzaları sahip olmayabilir, sadece onun tarafından farklı `ref` ve `out`.
*  Bir özellik veya olay adı, aynı arabirimde bildirilen tüm diğer üyelerinin adları farklı olmalıdır.
*  Bir dizin oluşturucu imzasının aynı arabirimde bildirilen tüm diğer dizin imzalarını farklı olmalıdır.

Devralınan üyeleri bir arabirimin özellikle değil arabirim bildirimi alanının parçasıdır. Bu nedenle, bir arabirim, aynı ad veya imzaya sahip bir üyesi devralınmış bir üyeyi olarak bildirin izin verilmez. Bu durumda türetilmiş bir arabirim üyesi temel arabirim bir üyeyi gizlemek için kabul edilir. Devralınmış bir üyeyi gizlemek hata olarak kabul edilmez, ancak derleyici bir uyarı neden olmaz. Bu uyarının gösterilmemesi için türetilmiş bir arabirim üyesi bildirimi içermelidir bir `new` türetilen üye temel üyeyi Gizle amaçlandığını belirtmek için değiştiricisi. Bu konunun ele alındığı daha ayrıntılı olarak [devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance).

Varsa bir `new` değiştiricisi devralınan bir üyeyi gizlemek olmayan bir bildiriminde dahil, bir uyarı belirlememişse bu bağlamda verilir. Kaldırarak bu uyarı bastırılır `new` değiştiricisi.

Unutmayın sınıfı üyeleri `object` kesinlikle anlamda, herhangi bir arabirim üyesi değilseniz, ([arabirim üyeleri](interfaces.md#interface-members)). Ancak, sınıf üyeleri `object` herhangi bir arabirim türüne içinde'üye araması aracılığıyla kullanılabilir ([üye araması](expressions.md#member-lookup)).

### <a name="interface-methods"></a>Arabirim yöntemleri

Arabirim yöntemleri kullanarak bildirilir *interface_method_declaration*: %s

```antlr
interface_method_declaration
    : attributes? 'new'? return_type identifier type_parameter_list
      '(' formal_parameter_list? ')' type_parameter_constraints_clause* ';'
    ;
```

*Öznitelikleri*, *Döndür_tür*, *tanımlayıcı*, ve *formal_parameter_list* bir arabirim yöntemi bildirimi aynı sahip bir yöntem bildiriminde bir sınıf olarak anlamına gelir ([yöntemleri](classes.md#methods)). Bir yöntem gövdesi belirtmek için bir arabirim yöntemi bildiriminde izin verilmez ve bildirimi bu nedenle her zaman noktalı virgülle biter.

Her biçimsel parametre türü bir arabirim yönteminin giriş güvenli olmalıdır ([varyansı güvenliği](interfaces.md#variance-safety)), ve dönüş türü olmalıdır `void` veya çıkış için güvenli. Ayrıca, her sınıf türü kısıtlaması, arabirim tür kısıtlaması ve yöntemin herhangi bir tür parametresi tür parametresi kısıtlaması giriş uyumlu olması gerekir.

Bu kurallar herhangi bir değişken sağlamak veya değişken karşıtı kullanım arabirimin tür kullanımı uyumlu kalır. Örneğin,
```csharp
interface I<out T> { void M<U>() where U : T; }
```
geçersiz olduğundan kullanımını `T` üzerinde tür parametresi kısıtlaması olarak `U` giriş açısından güvenli değildir.

Bu kısıtlama yerinde değil olan aşağıdaki şekilde tür güvenliği ihlal etmek mümkün olacaktır:
```csharp
class B {}
class D : B{}
class E : B {}
class C : I<D> { public void M<U>() {...} }
...
I<B> b = new C();
b.M<E>();
```
Bu, aslında bir çağrı `C.M<E>`. Çağrı, gerektirir. ancak bu `E` öğesinden türetilen `D`, tür güvenliği ihlal burada.

### <a name="interface-properties"></a>Arabirim özellikleri

Arabirim özellikleri kullanarak bildirilir *interface_property_declaration*: %s

```antlr
interface_property_declaration
    : attributes? 'new'? type identifier '{' interface_accessors '}'
    ;

interface_accessors
    : attributes? 'get' ';'
    | attributes? 'set' ';'
    | attributes? 'get' ';' attributes? 'set' ';'
    | attributes? 'set' ';' attributes? 'get' ';'
    ;
```

*Öznitelikleri*, *türü*, ve *tanımlayıcı* bir arabirimi özelliği bildirimi, bir sınıfta bir özellik bildirimi içeriğiyle aynı anlama sahip ([ Özellikleri](classes.md#properties)).

Bir sınıf özelliği bildirimindeki erişimcisi için bir arabirim özelliği bildiriminde erişicilerini karşılık gelir ([erişimcileri](classes.md#accessors)) erişimcisinin gövdesi her zaman noktalı olmalı, dışında. Bu nedenle, erişimciler yalnızca salt okunur, salt okunur veya salt yazılır özelliği olup olmadığını gösterir.

Arabirim özelliği bir alma erişimcisi varsa çıktı-güvenli türünde olmalıdır ve giriş ayarlama erişimcisine ise güvenli olmalıdır.

### <a name="interface-events"></a>Arabirim olayları

Arabirim olaylarını kullanarak bildirilir *interface_event_declaration*: %s

```antlr
interface_event_declaration
    : attributes? 'new'? 'event' type identifier ';'
    ;
```

*Öznitelikleri*, *türü*, ve *tanımlayıcı* bir arabirimi olay bildirimi, bir sınıf içinde olay bildiriminde sahip aynı anlamı sahip ([olayları ](classes.md#events)).

Bir arabirim olayı türü giriş uyumlu olması gerekir.

### <a name="interface-indexers"></a>Arabirim dizin oluşturucuları

Arabirim dizin oluşturucuları kullanarak bildirilir *interface_indexer_declaration*: %s

```antlr
interface_indexer_declaration
    : attributes? 'new'? type 'this' '[' formal_parameter_list ']' '{' interface_accessors '}'
    ;
```

*Öznitelikleri*, *türü*, ve *formal_parameter_list* bir arabirim dizin oluşturucu bildirimi bir dizin oluşturucu bildirimi sahip aynı anlamı bir sınıfta (sahip.[ Dizin oluşturucular](classes.md#indexers)).

Bir arabirim dizin oluşturucu bildirimi erişicilerini erişicilerini geçersiz bir sınıf dizin oluşturucu bildirimi için karşılık gelen ([dizin oluşturucular](classes.md#indexers)) dışında erişimcisinin gövdesi her zaman noktalı olmalıdır. Bu nedenle, erişimciler yalnızca dizin oluşturucunun salt okunur, salt okunur veya sadece yazılabilir mi olduğunu belirtin.

Tüm biçimsel parametre türleri arabirimi dizin oluşturucu giriş uyumlu olması gerekir. Ayrıca, tüm `out` veya `ref` biçimsel parametre türleri de çıkış uyumlu olmalıdır. Not, hatta `out` giriş güvenli bir temel yürütme platform sınırlaması nedeniyle olmasını gerekli parametreler.

Arabirim dizin oluşturucu çıkış uyumlu bir alma erişimcisi varsa türünde olmalıdır ve giriş ayarlama erişimcisine ise güvenli olmalıdır.

### <a name="interface-member-access"></a>Arabirim üye erişimi

Arabirim üyeleri, üye erişimi erişilir ([üye erişimi](expressions.md#member-access)) ve dizinleyici erişimi ([dizinleyici erişimi](expressions.md#indexer-access)) ifadeleri biçiminde `I.M` ve `I[A]`burada `I` bir arabirim türü `M` yöntemi, özelliği veya arabirim türü, olay ve `A` bir dizin oluşturucu bağımsız değişken listesi.

Kesinlikle olan arabirimler için tek devralma (devralma zincirini her arabirim, tam olarak sıfır veya bir doğrudan temel arabirimi vardır), üye araması etkilerini ([üye araması](expressions.md#member-lookup)), yöntem çağırma ([ Yöntem çağrıları](expressions.md#method-invocations)) ve dizin oluşturucu erişim ([dizinleyici erişimi](expressions.md#indexer-access)) kuralları, tam olarak aynı sınıflar ve yapılar için: Daha fazla üyeleri Gizle daha az türetilmiş üyeleri aynı ad veya imzaya sahip türetilmiş. Ancak, birden çok devralma arabirimleri için belirsizlikleri iki oluşabilir veya daha ilgisiz temel arabirimleri aynı ad veya imzaya üyeleri tanımlanmış olarak bildirin. Bu bölüm, bu tür durumlarda çeşitli örneklerini gösterir. Tüm durumlarda açık yayınları belirsizlikleri gidermek için kullanılabilir.

Örnekte
```csharp
interface IList
{
    int Count { get; set; }
}

interface ICounter
{
    void Count(int i);
}

interface IListCounter: IList, ICounter {}

class C
{
    void Test(IListCounter x) {
        x.Count(1);                  // Error
        x.Count = 1;                 // Error
        ((IList)x).Count = 1;        // Ok, invokes IList.Count.set
        ((ICounter)x).Count(1);      // Ok, invokes ICounter.Count
    }
}
```
ilk iki deyim olduğundan derleme zamanı hatalarına neden üye araması ([üye araması](expressions.md#member-lookup)), `Count` içinde `IListCounter` belirsiz. Örnekte gösterildiği gibi belirsizliği atama tarafından çözümlenen `x` uygun temel arabirim türü. Tür atamaları, hiçbir çalışma zamanı maliyetlerini sahip; bunlar yalnızca derleme zamanında daha az türetilmiş bir tür örneği görüntüleme oluşur.

Örnekte
```csharp
interface IInteger
{
    void Add(int i);
}

interface IDouble
{
    void Add(double d);
}

interface INumber: IInteger, IDouble {}

class C
{
    void Test(INumber n) {
        n.Add(1);                // Invokes IInteger.Add
        n.Add(1.0);              // Only IDouble.Add is applicable
        ((IInteger)n).Add(1);    // Only IInteger.Add is a candidate
        ((IDouble)n).Add(1);     // Only IDouble.Add is a candidate
    }
}
```
Çağırma `n.Add(1)` seçer `IInteger.Add` aşırı yükleme çözünürlüğü kuralları uygulayarak [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution). Benzer şekilde çağırma `n.Add(1.0)` seçer `IDouble.Add`. Açık yayınları eklenen olduğunda yalnızca bir aday yöntemi ve böylece belirsizlik olmaz.

Örnekte
```csharp
interface IBase
{
    void F(int i);
}

interface ILeft: IBase
{
    new void F(int i);
}

interface IRight: IBase
{
    void G();
}

interface IDerived: ILeft, IRight {}

class A
{
    void Test(IDerived d) {
        d.F(1);                 // Invokes ILeft.F
        ((IBase)d).F(1);        // Invokes IBase.F
        ((ILeft)d).F(1);        // Invokes ILeft.F
        ((IRight)d).F(1);       // Invokes IBase.F
    }
}
```
`IBase.F` üyesi tarafından gizlenir `ILeft.F` üyesi. Çağırma `d.F(1)` böylece seçer `ILeft.F`rağmen `IBase.F` aracılığıyla müşteri adayları erişim yolda gizlenmemiş görünmesine `IRight`.

Birden çok devralma arabirimlerde gizleme için sezgisel Kuralın yalnızca bu. Üye herhangi bir erişim yolunda gizli ise, tüm erişim yollarında gizlenir. Çünkü erişim yolundan `IDerived` için `ILeft` için `IBase` gizler `IBase.F`, üye erişimi yolundan gizlidir ayrıca `IDerived` için `IRight` için `IBase`.

## <a name="fully-qualified-interface-member-names"></a>Tam bir arabirim üye adları

Bir arabirim üyesi, bazen olarak adlandırılır, ***tam nitelikli ad***. Arabirim üyesini tam adı, üyesi bildirilen, ardından bir nokta, üye adından önce gelen arabirimin adını oluşur. Tam bir üyenin adını üye bildirildiği arabirimde başvuruyor. Örneğin, bildirimleri verildiğinde
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}
```
tam olarak nitelenmiş adını `Paint` olduğu `IControl.Paint` ve tam olarak nitelenmiş adını `SetText` olduğu `ITextBox.SetText`.

Yukarıdaki örnekte, başvurmak mümkün değildir `Paint` olarak `ITextBox.Paint`.

Bir arabirim bir ad alanının parçası olduğunda, bir arabirim üyesi tam adı ad alanı adı içerir. Örneğin:
```csharp
namespace System
{
    public interface ICloneable
    {
        object Clone();
    }
}
```

Burada, tam olarak nitelenmiş adını `Clone` yöntemi `System.ICloneable.Clone`.

## <a name="interface-implementations"></a>Arabirim uygulamaları

Arabirimler, sınıflar ve yapılar tarafından uygulanabilir. Bir sınıf veya yapı doğrudan bir arabirim uyguladığını belirtmek için arabirim tanımlayıcısı, sınıfın veya yapının temel sınıf listesinde dahildir. Örneğin:
```csharp
interface ICloneable
{
    object Clone();
}

interface IComparable
{
    int CompareTo(object other);
}

class ListEntry: ICloneable, IComparable
{
    public object Clone() {...}
    public int CompareTo(object other) {...}
}
```

Bir sınıf ya da doğrudan bir arabirimi doğrudan uygulayan yapı bu arabirimin temel arabirimleri tüm örtük olarak uygular. Sınıfın veya yapının tüm temel arabirimleri taban sınıfı listesinde açıkça listelenmiyor olsa bile bu geçerlidir. Örneğin:
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    public void Paint() {...}
    public void SetText(string text) {...}
}
```

Burada, sınıf `TextBox` hem `IControl` ve `ITextBox`.

Bir sınıf, `C` doğrudan uygulayan bir arabirim, C'den türetilmiş tüm sınıfları ayrıca arayüzü dolaylı olarak gerçekleştir. Bir sınıf bildiriminde belirtilenden temel arabirimleri oluşturulmuş arabirim türlerinde olabilir ([oluşturulan türler](types.md#constructed-types)). Kapsam türü parametreler içerebilir ancak bir taban arabirimi kendi kendine, bir tür parametresi olamaz. Aşağıdaki kod nasıl bir sınıf uygulamak ve oluşturulan türler genişletmek gösterilmektedir:
```csharp
class C<U,V> {}

interface I1<V> {}

class D: C<string,int>, I1<string> {}

class E<T>: C<int,T>, I1<T> {}
```

Genel sınıf bildiriminin temel arabirimleri açıklanan benzersizlik kuralı karşılamalıdır [uygulanan arabirimleri benzersizliğini](interfaces.md#uniqueness-of-implemented-interfaces).

### <a name="explicit-interface-member-implementations"></a>Açık arabirim üyesi uygulamaları

Arabirimleri uygulama amacıyla, bir sınıf veya yapı bildirebilir ***açık arabirim üyesi uygulamalarını***. Açık arabirim üyesi tam arabirim üye adına başvuran bir yöntem, özellik, olay veya dizin oluşturucu bildirimi uygulamasıdır. Örneğin:
```csharp
interface IList<T>
{
    T[] GetElements();
}

interface IDictionary<K,V>
{
    V this[K key];
    void Add(K key, V value);
}

class List<T>: IList<T>, IDictionary<int,T>
{
    T[] IList<T>.GetElements() {...}
    T IDictionary<int,T>.this[int index] {...}
    void IDictionary<int,T>.Add(int index, T value) {...}
}
```

Burada `IDictionary<int,T>.this` ve `IDictionary<int,T>.Add` açık arabirim üyesi uygulamalarıdır.

Bazı durumlarda, bir arabirim üyesinin adı çalışması arabirim üyesi, uygulanabilir açık arabirim üyesi uygulaması kullanarak uygulama sınıfı için uygun olmayabilir. Örneğin, bir dosya özeti uygulayan bir sınıf uygulamak büyük olasılıkla bir `Close` dosya kaynağı serbest etkisi ve uygulama üye işlevi `Dispose` yöntemi `IDisposable` arabirimi açık arabirim kullanma üye uygulaması:
```csharp
interface IDisposable
{
    void Dispose();
}

class MyFile: IDisposable
{
    void IDisposable.Dispose() {
        Close();
    }

    public void Close() {
        // Do what's necessary to close the file
        System.GC.SuppressFinalize(this);
    }
}
```

Tam adı bir yöntem çağrısı, özellik erişimi veya dizin oluşturucu erişim aracılığıyla üye açık arabirim uygulaması erişmek mümkün değildir. Açık arabirim üyesi uygulaması yalnızca bir arabirim örneği erişilebilir ve bu durumda yalnızca üye adıyla başvurulur.

Erişim değiştiricileri içerecek şekilde açık arabirim üyesi uygulaması için bir derleme zamanı hata ve değiştiriciler dahil etmek için bir derleme zamanı hata `abstract`, `virtual`, `override`, veya `static`.

Açık arabirim üyesi uygulamaları, diğer üyelerinden farklı erişilebilirlik özelliklerine sahiptir. Açık arabirim üyesi uygulamalarını tam adında bir yöntem çağrısı veya özellik erişimi üzerinden erişilebilir olduğundan, hiçbir zaman özel bir anlamda değildirler. Ancak, bir arabirim örneği erişilebilir olduğundan, ayrıca genel anlamda uygulanır.

Açık arabirim üyesi uygulamalarını iki birincil amaca hizmet eder:

*  Açık arabirim üyesi uygulamalarını sınıfın veya yapının örneği erişilebilir olmadığından, bir sınıfın veya yapının ortak arabirimden hariç tutulacak arabirim uygulamalarına izin verin. Bu özellikle yararlı bir sınıf veya yapı, sınıfın veya yapının bir tüketici hiçbir ilgi iç bir arabirim uygular.
*  Açık arabirim üyesi uygulamalarını arabirim üyelerinin aynı imzayla Kesinleştirme izin verir. Açık arabirim üyesi uygulamalarını bu arabirim üyeleri aynı imzaya sahip ve, olarak, herhangi bir uygulama için bir sınıf veya yapı için imkansız olan dönüş türü farklı uygulamalarını sağlamak için bir sınıf veya yapı için mümkün olacaktır Tümünü arabirim üyelerinin ve aynı imzaya ancak farklı dönüş türlerine sahip.

Geçerli olması açık arabirim üyesi için bir uygulama, sınıf veya yapı, tam adı, türü ve parametre türleri tam olarak açık arabirim üyesi eşleşen bir üyeyi içeren kendi taban sınıfı listesinde bir arabirim adı olmalıdır uygulama. Bu nedenle, aşağıdaki sınıfı
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
    int IComparable.CompareTo(object other) {...}    // invalid
}
```
bildirimi `IComparable.CompareTo` sonuçları bir derleme zamanı hatası nedeniyle `IComparable` temel sınıf listesinde listelenmeyen `Shape` ve bir taban arabirimi değil `ICloneable`. Benzer şekilde, bildirimler
```csharp
class Shape: ICloneable
{
    object ICloneable.Clone() {...}
}

class Ellipse: Shape
{
    object ICloneable.Clone() {...}    // invalid
}
```
bildirimi `ICloneable.Clone` içinde `Ellipse` sonuçları bir derleme zamanı hatası nedeniyle `ICloneable` açıkça temel sınıf listesinde listelenmeyen `Ellipse`.

Arabirim üyesini tam adı, üyenin bildirilen arabirimi başvurmalıdır. Bu nedenle, bildirimler
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

class TextBox: ITextBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
}
```
Açık arabirim üye uygulamasıdır `Paint` olarak yazılmalıdır `IControl.Paint`.

### <a name="uniqueness-of-implemented-interfaces"></a>Uygulanan arabirimlerin benzersizlik

Bir genel tür bildirimi tarafından uygulanan arabirimler olası tüm oluşturulan türler için benzersiz olmalıdır. Bu kural oluşturulan belirli türler için çağrılacak doğru yöntem belirlemek mümkün olacaktır. Örneğin, bir genel sınıf bildirimi izin verilen gibi yazılacak varsayalım:
```csharp
interface I<T>
{
    void F();
}

class X<U,V>: I<U>, I<V>                    // Error: I<U> and I<V> conflict
{
    void I<U>.F() {...}
    void I<V>.F() {...}
}
```

Bu izin olan, aşağıdaki örnekte yürütmek için hangi kodun belirlemek mümkün olacaktır:
```csharp
I<int> x = new X<int,int>();
x.F();
```

Bir genel tür bildirimi arabirimi listesi geçerli olup olmadığını belirlemek için aşağıdaki adımları gerçekleştirilir:

*  İzin `L` doğrudan bir genel sınıf, yapı veya arabirim bildiriminde belirtilen arabirimlerin listesi `C`.
*  Ekleme `L` herhangi temel arabirimleri içinde arabirimlerin `L`.
*  Tüm yinelenenleri kaldırma `L`.
*  Türü oluşturulan tüm olası oluşturulmuş, `C` olduğu tür bağımsız değişkenleri ile başvurulduğunda sonra `L`, iki arabirimi neden `L` aynı olmaları için bildirimi ardından `C` geçersiz. Kısıtlama bildirimleri olası tüm oluşturulan türler belirlerken dikkate alınmaz.

Sınıf bildiriminde `X` arabirimi listesinin üst `L` oluşan `I<U>` ve `I<V>`. Herhangi bir türü ile oluşturulmuş bildirimi geçersiz `U` ve `V` aynı türde olan aynı olması bu iki arabirim neden.

Birleştirmek için farklı bir devralma düzeylerinde belirtilen arabirimleri mümkündür:
```csharp
interface I<T>
{
    void F();
}

class Base<U>: I<U>
{
    void I<U>.F() {...}
}

class Derived<U,V>: Base<U>, I<V>    // Ok
{
    void I<V>.F() {...}
}
```

Bu kod olsa bile geçerlidir `Derived<U,V>` hem `I<U>` ve `I<V>`. Kodu
```csharp
I<int> x = new Derived<int,int>();
x.F();
```
yöntemi çağırır `Derived`, bu yana `Derived<int,int>` etkili bir şekilde yeniden uygular `I<int>` ([arabirimini yeniden uygulama](interfaces.md#interface-re-implementation)).

### <a name="implementation-of-generic-methods"></a>Genel yöntemler uygulaması

Ne zaman bir genel yöntemini örtük olarak uygulayan bir arabirim yöntemi her yöntem türü parametresi (parametreleri uygun tür bağımsız değişkenleri ile değiştirilir herhangi bir arabirim türüne sonra), burada iki bildirimlerinde eşdeğer olmalıdır için verilen kısıtlamaları yöntemi Tür parametreleri, soldan sağa sıralı konumları tarafından tanımlanır.

Ancak, genel bir yöntem bir arabirim yöntemini açıkça kullanan kullandığınızda, hiçbir kısıtlama uygulama yöntemi izin verilir. Bunun yerine, kısıtlamaları arabirimi yöntemden devralınır

```csharp
interface I<A,B,C>
{
    void F<T>(T t) where T: A;
    void G<T>(T t) where T: B;
    void H<T>(T t) where T: C;
}

class C: I<object,C,string>
{
    public void F<T>(T t) {...}                    // Ok
    public void G<T>(T t) where T: C {...}         // Ok
    public void H<T>(T t) where T: string {...}    // Error
}
```

Yöntem `C.F<T>` örtük olarak uygulayan `I<object,C,string>.F<T>`. Bu durumda, `C.F<T>` gerekli (izin veya değil) kısıtlama belirtmek için `T:object` beri `object` tüm tür parametrelerindeki örtük bir sınırlamadır. Yöntem `C.G<T>` örtük olarak uygulayan `I<object,C,string>.G<T>` arabirimi tür parametreleri karşılık gelen tür bağımsız değişkenleri ile değiştirildikten sonra sınırlamalar arabirim içindeki alanlarla eşleşmesi için. Yöntem kısıtlaması `C.H<T>` hata türleri korumalı olduğundan (`string` bu durumda) kısıtlama olarak kullanılamaz. Örtük arabirimi yöntem uygulamaları kısıtlamaları eşleştirilmesi için bu yana kısıtlaması atlama hata da olacaktır. Bu nedenle, örtük olarak uygulamak mümkün değildir `I<object,C,string>.H<T>`. Bu arabirim yöntemi yalnızca açık arabirim üyesi uygulaması kullanılarak uygulanabilir:
```csharp
class C: I<object,C,string>
{
    ...

    public void H<U>(U u) where U: class {...}

    void I<object,C,string>.H<T>(T t) {
        string s = t;    // Ok
        H<T>(t);
    }
}
```

Bu örnekte, açık arabirim üyesi uygulaması kesin olarak daha zayıf kısıtlamalarına sahip ortak bir yöntemi çağırır. Unutmayın atamadan `t` için `s` itibaren geçerlidir `T` kısıtlaması, devralınan `T:string`rağmen bu kısıtlama, kaynak kodunda ifade edilemez.

### <a name="interface-mapping"></a>Arabirim eşleme

Bir sınıfın veya yapının sınıfın veya yapının temel sınıf listesinde listelenen arabirimler tüm üyelerinin uygulamalarını sağlamanız gerekir. Uygulayan bir sınıf veya yapı içinde arabirim üyelerinin uygulamalarını bulma işlemi olarak bilinir ***arabirim eşleme***.

Bir sınıf veya yapı için arabirim eşlemesi `C` her üye temel sınıf listesinde belirtilen her arabirim için bir uygulama bulur `C`. Belirli bir arabirim üye uygulamasıdır `I.M`burada `I` , arabirim üyesi `M` bildirilen, her bir sınıf veya yapı incelenerek belirlenir `S`ile başlayan `C` ve Her art arda gelen taban sınıfı için yinelenen `C`kadar bir eşleşme bulunur:

*  Varsa `S` eşleşen açık arabirim üyesi uygulaması bildirimi içeren `I` ve `M`, sonra da bu üye uygulamasıdır `I.M`.
*  Aksi takdirde `S` eşleşen bir ortak statik olmayan üye bildirimi içeren `M`, sonra da bu üye uygulamasıdır `I.M`. En fazla bir üye eşleşme unspecified ise hangi üye uygulamasıdır `I.M`. Bu durum yalnızca oluşabilir `S` bir oluşturulmuş tür burada genel türde bildirilen iki üyesi farklı imzalara sahip olduğunu, ancak bunların imzalarını tür bağımsız değişkenleri aynı olun.

Tüm arabirimleri temel sınıf listesinde belirtilen tüm üyeleri için uygulamaları bulunamazsa, bir derleme zamanı hatası oluşur `C`. Bir arabirimin üyelerini temel Ara birimden devralınır bu üyeler unutmayın.

Arabirim eşleme, bir sınıf üyesinin amacıyla `A` arabirim üyesiyle eşleşiyor `B` olduğunda:

*  `A` ve `B` yöntemleri ve adını, türünü ve biçimsel parametre listeleri `A` ve `B` aynıdır.
*  `A` ve `B` özellikleri ad ve tür `A` ve `B` aynıdır ve `A` aynı erişimci olarak sahip `B` (`A` açık bir arabirim değil ek erişimcisi varsa izin verilir üye uygulaması).
*  `A` ve `B` olaylar, ad ve tür `A` ve `B` aynıdır.
*  `A` ve `B` dizin oluşturucular türünü ve biçimsel parametre listesi `A` ve `B` aynıdır ve `A` aynı erişimci olarak sahip `B` (`A` değilse ek erişimcilerine sahip. izin verilen bir Açık arabirim üyesi uygulaması).

Arabirimi eşleştirme algoritmasını önemli etkileri verilmiştir:

*  Açık arabirim üyesi uygulamalarını diğer üyeleri aynı sınıfın veya yapının bir arabirim üyesi uygulayan sınıf veya yapı üyesi belirlerken önceliklidir.
*  Genel olmayan ne statik üyeleri arabirimi eşlemede katılın.

Örnekte
```csharp
interface ICloneable
{
    object Clone();
}

class C: ICloneable
{
    object ICloneable.Clone() {...}
    public object Clone() {...}
}
```
`ICloneable.Clone` üyesi `C` uygulaması haline gelir `Clone` içinde `ICloneable` çünkü açık arabirim üyesi uygulamalarını diğer üyelere göre önceliklidir.

Bir sınıf veya yapı iki uyguluyorsa veya daha fazla arabirim bir üyeyi aynı adı, türü ve parametre türleri ile içeren, her biri tek bir sınıf veya yapı üyesi üzerine bu arabirim üyeleri eşlemek mümkündür. Örneğin:
```csharp
interface IControl
{
    void Paint();
}

interface IForm
{
    void Paint();
}

class Page: IControl, IForm
{
    public void Paint() {...}
}
```

Burada, `Paint` hem de yöntemleri `IControl` ve `IForm` üzerine eşlenen `Paint` yönteminde `Page`. Elbette da iki yöntem için ayrı açık arabirim üyesi uygulamalarını olması mümkündür.

Gizli üyeleri içeren bir arabirim uygularsa, bir sınıfın veya yapının bazı üyeleri mutlaka açık arabirim üyesi uygulamaları uygulanmalıdır. Örneğin:
```csharp
interface IBase
{
    int P { get; }
}

interface IDerived: IBase
{
    new int P();
}
```

Bu arabirim uygulaması en az bir açık arabirim üyesi uygulaması gerekir ve aşağıdaki biçimlerden birini alır
```csharp
class C: IDerived
{
    int IBase.P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    public int P { get {...} }
    int IDerived.P() {...}
}

class C: IDerived
{
    int IBase.P { get {...} }
    public int P() {...}
}
```

Aynı temel arabirimi sahip birden çok arabirim arabirimini uygulayan bir sınıf, temel arabirim yalnızca bir adet olabilir. Örnekte
```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

class ComboBox: IControl, ITextBox, IListBox
{
    void IControl.Paint() {...}
    void ITextBox.SetText(string text) {...}
    void IListBox.SetItems(string[] items) {...}
}
```
için farklı uygulamalara sahip mümkün değildir `IControl` temel sınıf listesinde adlı `IControl` tarafından devralınan `ITextBox`ve `IControl` tarafından devralınan `IListBox`. Aslında bu arabirimler için ayrı kimlik kavramı yoktur. Bunun yerine, uygulamalarına `ITextBox` ve `IListBox` aynı uygulamasını paylaşmak `IControl`, ve `ComboBox` üç arabirimleri de uygulamak için basitçe kabul `IControl`, `ITextBox`, ve `IListBox`.

Bir temel sınıf üyelerinin arabirimi eşlemede katılın. Örnekte
```csharp
interface Interface1
{
    void F();
}

class Class1
{
    public void F() {}
    public void G() {}
}

class Class2: Class1, Interface1
{
    new public void G() {}
}
```
yöntem `F` içinde `Class1` kullanılır `Class2`'s uygulaması `Interface1`.

### <a name="interface-implementation-inheritance"></a>Arabirimi uygulama devralma

Bir sınıf kendi temel sınıflar tarafından sağlanan tüm arabirim uygulamalarına devralır.

Olmadan açıkça ***yeniden uygulama*** arabirim, türetilmiş bir sınıf kendi temel sınıftan devraldığı arabirim eşlemeleri herhangi bir şekilde değiştirilemiyor. Örneğin, bildirimler
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public void Paint() {...}
}

class TextBox: Control
{
    new public void Paint() {...}
}
```
`Paint` yöntemi `TextBox` gizler `Paint` yöntemi `Control`, ancak eşleme değiştirmez `Control.Paint` üzerine `IControl.Paint`ve çağrılar `Paint` sınıfı aracılığıyla örnekleri ve arabirimi örnekleri olur. Aşağıdaki etkileri
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes Control.Paint();
```

Bir arabirim yöntemi sanal bir yöntem bir sınıftaki eşlendiğinde ancak arabirim uygulamasını sanal yöntemi geçersiz kılın ve türetilen sınıflar için mümkündür. Örneğin, yukarıda bildirimi yeniden yazma
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    public virtual void Paint() {...}
}

class TextBox: Control
{
    public override void Paint() {...}
}
```
Aşağıdaki etkileri artık gözlemlenen
```csharp
Control c = new Control();
TextBox t = new TextBox();
IControl ic = c;
IControl it = t;
c.Paint();            // invokes Control.Paint();
t.Paint();            // invokes TextBox.Paint();
ic.Paint();           // invokes Control.Paint();
it.Paint();           // invokes TextBox.Paint();
```

Açık arabirim üyesi uygulaması geçersiz kılmak mümkün değildir, bu yana açık arabirim üyesi uygulamalarını sanal olarak bildirilemez. Ancak başka bir yöntem çağırmak açık arabirim üyesi uygulaması için mükemmel bir şekilde geçerli olduğundan ve başka bir yöntem izin vermek için sanal bildirilen, türetilmiş sınıfları geçersiz kılmak için. Örneğin:
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() { PaintControl(); }
    protected virtual void PaintControl() {...}
}

class TextBox: Control
{
    protected override void PaintControl() {...}
}
```

Burada, türetilmiş sınıflar `Control` yürütmesinin specialize `IControl.Paint` kılarak `PaintControl` yöntemi.

### <a name="interface-re-implementation"></a>Arabiriminin yeniden uygulanmasına

Bir arabirim uygulaması devralınan bir sınıf için izin verilen ***yeniden uygulama*** temel sınıf listesinde ekleyerek arabirime.

Bir arabirimin bir yeniden uygulanmasına tam olarak aynı arabirimi eşleme kurallarını bir arabirim uygulaması ilk olarak izler. Bu nedenle, devralınan arabirimi eşleme, vermemektedir arabirimi eşlemeye arabiriminin yeniden uygulanmasına için oluşturulan hiçbir etkisi olmaz. Örneğin, bildirimler
```csharp
interface IControl
{
    void Paint();
}

class Control: IControl
{
    void IControl.Paint() {...}
}

class MyControl: Control, IControl
{
    public void Paint() {}
}
```
Olgu, `Control` eşler `IControl.Paint` üzerine `Control.IControl.Paint` yeniden uygulamasında etkilemez `MyControl`, hangi haritalar `IControl.Paint` üzerine `MyControl.Paint`.

Genel üye bildirimleri ve devralınan açık arabirim üyesi bildirimi yeniden uygulanan arabirimleri için arabirim eşleme işlemi katılmak devralındı. Örneğin:
```csharp
interface IMethods
{
    void F();
    void G();
    void H();
    void I();
}

class Base: IMethods
{
    void IMethods.F() {}
    void IMethods.G() {}
    public void H() {}
    public void I() {}
}

class Derived: Base, IMethods
{
    public void F() {}
    void IMethods.H() {}
}
```

Burada, uygulanması `IMethods` içinde `Derived` eşleyen arabirim yöntemleri üzerine `Derived.F`, `Base.IMethods.G`, `Derived.IMethods.H`, ve `Base.I`.

Arabirimini uygulayan bir sınıf, arayüz, örtük olarak bu arabirimin temel arabirimleri de uygular. Benzer şekilde, bir yeniden bir arabirimin de örtülü olarak tüm arabiriminin temel arabirimleri bir yeniden uygulanmasına uygulamasıdır. Örneğin:
```csharp
interface IBase
{
    void F();
}

interface IDerived: IBase
{
    void G();
}

class C: IDerived
{
    void IBase.F() {...}
    void IDerived.G() {...}
}

class D: C, IDerived
{
    public void F() {...}
    public void G() {...}
}
```

Burada, yeniden uygulanması `IDerived` de yeniden uygular `IBase`eşleme `IBase.F` üzerine `D.F`.

### <a name="abstract-classes-and-interfaces"></a>Soyut sınıflar ve arabirimler

Soyut olmayan sınıfı gibi tüm üyeleri sınıfın temel sınıf listesinde listelenen arabirimler uygulamaları soyut bir sınıf sağlamanız gerekir. Ancak, soyut bir sınıf, arabirim yöntemleri soyut yöntemler üzerine eşlemek için izin verilir. Örneğin:
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    public abstract void F();
    public abstract void G();
}
```

Burada, uygulanması `IMethods` eşler `F` ve `G` soyut yöntemler hangi kılınmalıdır öğesinden türetilen soyut olmayan sınıflarda `C`.

Açık arabirim üyesi uygulamalarını soyut olamaz, ancak açık arabirim üyesi uygulamalarını Elbette soyut yöntemlerini çağırmak için izin verilen unutmayın. Örneğin:
```csharp
interface IMethods
{
    void F();
    void G();
}

abstract class C: IMethods
{
    void IMethods.F() { FF(); }
    void IMethods.G() { GG(); }
    protected abstract void FF();
    protected abstract void GG();
}
```

Burada, türetilen soyut olmayan sınıflar `C` geçersiz kılmak için gerekli olacak `FF` ve `GG`, bu nedenle gerçek uygulamasını sağlayan `IMethods`.
