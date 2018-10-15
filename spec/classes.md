# <a name="classes"></a>Sınıflar

Bir sınıf veri üyeleri (sabitleri ve alanları), işlev üyeleri (yöntemler, özellikler, olaylar, dizin oluşturucular, işleçler, örnek oluşturucuları, yok ediciler ve statik oluşturucular) ve iç içe türler içerebilecek bir veri yapısıdır. Sınıf türleri, devralma, yapabildiği türetilmiş bir sınıf genişletin ve bir temel sınıf uzmanlaşmış bir mekanizma destekler.

## <a name="class-declarations"></a>Sınıf bildirimleri

A *class_declaration* olduğu bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)), yeni bir sınıfı bildirir.

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

A *class_declaration* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)) ve ardından isteğe bağlı bir dizi *class_modifier*s ([sınıfı değiştiricileri](classes.md#class-modifiers)) ve ardından isteğe bağlı `partial` değiştiricisi anahtar sözcüğü ve ardından, `class` ve *tanımlayıcı* ardından sınıf adları bir İsteğe bağlı *type_parameter_list* ([tür parametrelerindeki](classes.md#type-parameters)) ve ardından isteğe bağlı *class_base* belirtimi ([temel sınıfı belirtimi](classes.md#class-base-specification)) ve ardından isteğe bağlı bir dizi *type_parameter_constraints_clause*s ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *class_body*  ([Sınıfı gövdesi](classes.md#class-body)), noktalı virgül tarafından izlenen isteğe bağlı olarak.

Bir sınıf bildiriminde sağlayamazsınız *type_parameter_constraints_clause*s ayrıca sağlanmadıkça bir *type_parameter_list*.

Sağlayan bir sınıf bildirimi bir *type_parameter_list* olduğu bir ***genel sınıf bildirimi***. Ayrıca bu yana oluşturulan bir tür oluşturmak için kapsayan tür için tür parametreleri sağlanmalıdır bir genel sınıf bildirimi veya genel yapı bildirimi içinde herhangi bir sınıfın kendisi bir genel sınıf bildirimi sunulmuştur.

### <a name="class-modifiers"></a>Sınıfı değiştiricilere

A *class_declaration* isteğe bağlı olarak bir dizi sınıfı değiştiricilere içerebilir:

```antlr
class_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'abstract'
    | 'sealed'
    | 'static'
    | class_modifier_unsafe
    ;
```

Aynı değiştiricisi bir sınıf bildirimi içinde birden çok kez görünmesi için bir derleme zamanı hatasıdır.

`new` Değiştiricisi iç içe sınıflarda izin verilir. Bölümünde anlatıldığı gibi sınıfı aynı ada göre devralınmış bir üyeyi gizler belirtir [yeni değiştiricisini](classes.md#the-new-modifier). Bir derleme zamanı hata için `new` iç içe geçmiş sınıf bildirimi olmayan bir sınıf bildirimi üzerinde görünecek değiştiricisi.

`public`, `protected`, `internal`, Ve `private` sınıf erişilebilirlik değiştiricileri denetim. Sınıf bildiriminin oluştuğu bağlama bağlı olarak bu değiştiriciler bazıları değil izin verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).

`abstract`, `sealed` Ve `static` değiştiriciler, aşağıdaki bölümlerde ele alınmıştır.

#### <a name="abstract-classes"></a>Soyut sınıflar

`abstract` Değiştiricisi bir sınıf eksik olduğunu ve yalnızca temel sınıf olarak kullanılmak üzere tasarlanmış olduğunu belirtmek için kullanılır. Bir Özet sınıf, Özet olmayan bir sınıftan aşağıdaki yollarla farklıdır:

*  Soyut bir sınıf doğrudan oluşturulamaz ve kullanmak için bir derleme zamanı hata `new` işlecinin bir Özet sınıf. Değişkenler ve değerleri, derleme zamanı türü soyut mümkün olsa da, bu tür değişkenlere ve değerlere mutlaka olacaktır `null` veya soyut türlerden türetilmiş soyut olmayan sınıflar örneklerini başvurular içeriyor.
*  Bir soyut sınıfı izin verilir (ancak gerekli değildir) soyut üye içermesi.
*  Bir soyut sınıfı korumalı olamaz.

Bir soyut sınıftan türetilmiş soyut olmayan sınıf, soyut olmayan sınıf gerçek uygulamaları böylece bu soyut üyelerini geçersiz kılarak tüm devralınan soyut üye içermesi gerekir. Örnekte
```csharp
abstract class A
{
    public abstract void F();
}

abstract class B: A
{
    public void G() {}
}

class C: B
{
    public override void F() {
        // actual implementation of F
    }
}
```
soyut sınıf `A` soyut bir yöntem sunar `F`. Sınıf `B` ek bir yöntem sunar `G`, ancak bu yana uygulaması sağlamaz `F`, `B` da soyut bildirilmelidir. Sınıf `C` geçersiz kılmalar `F` ve gerçek bir uygulamasını sağlar. Soyut üye yok olduğundan `C`, `C` izin verilir (ancak gerekli değildir) Özet olmayan olacak.

#### <a name="sealed-classes"></a>Sealed sınıfları

`sealed` Değiştiricisi, bir sınıftan türetme önlemek için kullanılır. Korumalı sınıf başka bir sınıfın temel sınıf olarak belirtilirse bir derleme zamanı hatası oluşur.

Korumalı sınıf bir soyut sınıfı da olamaz.

`sealed` Değiştiricisi istenmeyen türetme önlemek için öncelikli olarak kullanılır, ancak ayrıca bazı çalışma zamanı iyileştirmeleri sağlar. Özellikle, korumalı bir sınıfın tüm türetilmiş sınıfları için hiçbir zaman bilindiğinden, sanal olmayan çağrılarını korumalı sınıf örneklerinde sanal işlev üyesi çağrılarını dönüştürme mümkündür.

#### <a name="static-classes"></a>Statik sınıflar

`static` Değiştirici olarak bildirilen bir sınıfı işaretlemek için kullanılan bir ***statik sınıf***. Statik sınıf oluşturulamaz, bir tür olarak kullanılamaz ve yalnızca statik üyeleri içerebilir. Yalnızca bir statik sınıf genişletme yöntemleri, bildirimler içerebilir ([genişletme yöntemleri](classes.md#extension-methods)).

Bir statik sınıf bildirimi aşağıdaki sınırlamalara tabi şu şekildedir:

*  Statik sınıf içermiyor bir `sealed` veya `abstract` değiştiricisi. Ancak, bir statik sınıf örneği veya türetilmiş olduğundan, mühürlendi ve soyut gibi davranır olduğunu unutmayın.
*  Statik sınıf içermiyor bir *class_base* belirtimi ([sınıf temel belirtimine](classes.md#class-base-specification)) ve bir temel sınıf veya uygulanan arabirimleri listesini açıkça belirtemezsiniz. Statik sınıf örtük tür tarafından devralındığında `object`.
*  Statik sınıf yalnızca statik üyeleri içerebilir ([statik ve örnek üyeleri](classes.md#static-and-instance-members)). Sabitler ve iç içe geçmiş türler statik üyeleri olarak sınıflandırıldığını unutmayın.
*  Statik sınıf üyeleri ile olamaz `protected` veya `protected internal` erişilebilirlik bildirilir.

Tüm bu kısıtlamalar ihlal eden bir derleme zamanı hatasıdır.

Statik sınıf örneği oluşturucusu yok. Bir statik sınıftaki örnek oluşturucusu ve yok varsayılan örnek oluşturucusu bildirmek mümkün değildir ([varsayılan oluşturucular](classes.md#default-constructors)) statik bir sınıf için sağlanır.

Statik sınıf üyeleri otomatik olarak statik olmayan ve üye bildirimleri açıkça içermelidir bir `static` değiştirici (hariç, sabitleri ve iç içe geçmiş türler). Bir sınıfın statik bir dış sınıf içinde iç içe geçmiş, açıkça içermedikçe iç içe geçmiş sınıf bir statik sınıf değil bir `static` değiştiricisi.

__Statik sınıf türlerine başvurma__

A *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)), bir statik sınıf başvurmak için izin verilir

*  *Namespace_or_type_name* olduğu `T` içinde bir *namespace_or_type_name* formun `T.I`, veya
*  *Namespace_or_type_name* olduğu `T` içinde bir *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) biçiminde `typeof(T)`.

A *primary_expression* ([işlev üyeleri](expressions.md#function-members)), bir statik sınıf başvurmak için izin verilir

*  *Primary_expression* olduğu `E` içinde bir *member_access* ([derleme zamanı dinamik aşırı yükleme çözünürlüğü denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) biçiminde `E.I`.

Başka bir bağlamda, statik sınıf başvurmak için bir derleme zamanı hatasıdır. Örneğin, bir temel sınıf bağlı bir türü olarak kullanılacak bir statik sınıf için bir hata olduğu ([türlerin](classes.md#nested-types)) üyesi, bir genel tür bağımsız değişkeni ya da bir tür parametresi kısıtlaması. Benzer şekilde, bir statik sınıf bir dizi türü, işaretçi türünde kullanılamaz bir `new` ifade, bir atama ifadesi bir `is` ifadesi, bir `as` ifadesi, bir `sizeof` ifadesi veya varsayılan değer ifadesi.

### <a name="partial-modifier"></a>Partial değiştiricisi

`partial` Değiştiricisi belirtmek için kullanılır, bu *class_declaration* kısmi türü bildirimi. Form bir türü bildirimi için birleştirme kapsayan bir ad alanı veya tür bildirimi içinde aynı ada sahip birden fazla kısmi tür bildirimleri, kuralları aşağıdaki belirtilen [kısmi türlerinden](classes.md#partial-types).

Bu kesimler üretilen veya farklı bağlamlardaki tutulan ayrı program metni parçalarını dağıtılan bir sınıf bildirimi sahip yararlı olabilir. Örneğin, diğer el ile yazılan ise sınıf bildiriminin bir parçası oluşturulan makine olabilir. İki metin ayrımı güncelleştirmeleri tarafından diğer güncelleştirmelerle çakışan birinden tarafından engeller.

### <a name="type-parameters"></a>Tür parametreleri

Bir tür parametresi oluşturulan bir tür oluşturmak için sağlanan bir tür bağımsız değişkeni için bir yer tutucu gösteren basit bir tanımlayıcıdır. Bir tür parametresi daha sonra sunulacaktır bir tür için resmi bir yer tutucudur. Bunun aksine, bir tür bağımsız değişkeni olarak ([tür bağımsız değişkenleri](types.md#type-arguments)) oluşturulmuş bir türü oluşturulduğunda, tür parametresi yerine koyulan gerçek türü.

```antlr
type_parameter_list
    : '<' type_parameters '>'
    ;

type_parameters
    : attributes? type_parameter
    | type_parameters ',' attributes? type_parameter
    ;

type_parameter
    : identifier
    ;
```

Her bir sınıf bildiriminde tür parametresi bildirim alanında ad tanımlar ([bildirimleri](basic-concepts.md#declarations)) söz konusu sınıfın. Bu nedenle, başka bir tür parametresiyle aynı ada sahip olamaz veya bu sınıfta bir üye bildirildi. Bir tür parametresi türü ile aynı ada sahip olamaz.

### <a name="class-base-specification"></a>Temel sınıfı belirtimi

Bir sınıf bildirimi içerebilir bir *class_base* tanımlayan doğrudan temel sınıf sınıf ve arabirim belirtimi ([arabirimleri](interfaces.md)) doğrudan sınıf tarafından uygulanan.

```antlr
class_base
    : ':' class_type
    | ':' interface_type_list
    | ':' class_type ',' interface_type_list
    ;

interface_type_list
    : interface_type (',' interface_type)*
    ;
```

Oluşturulan sınıf türü bir sınıf bildiriminde belirtilen taban sınıfı olabilir ([oluşturulan türler](types.md#constructed-types)). Kapsam türü parametreler içerebilir ancak bir taban sınıfı kendi kendine, bir tür parametresi olamaz.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Temel sınıflar

Olduğunda bir *class_type* dahil *class_base*, bildirilen sınıf doğrudan temel sınıfını belirtir. Bir sınıf bildirimi Hayır varsa *class_base*, veya *class_base* listeleri yalnızca arabirim türleri, doğrudan temel sınıf olarak kabul edilir `object`. Bölümünde anlatıldığı gibi bir sınıf üyelerini doğrudan kendi temel sınıfından devralır [devralma](classes.md#inheritance).

Örnekte
```csharp
class A {}

class B: A {}
```
sınıf `A` doğrudan temel sınıfını olduğu söylenir `B`, ve `B` nesnesinden türetilmesi söylenir `A`. Bu yana `A` mu doğrudan temel sınıfının açıkça belirtin, doğrudan temel sınıfı örtülü olarak `object`.

Genel sınıf bildiriminde bir temel sınıf belirtilmişse oluşturulan sınıf türü için oluşturulan tür taban sınıfı, her biri için değiştirerek elde edilen *type_parameter* karşılık gelen taban sınıf bildiriminde *type_argument* oluşturulan türü. Genel sınıf bildirimleri verildiğinde
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
oluşturulan tür temel sınıfını `G<int>` olacaktır `B<string,int[]>`.

Bir sınıf türünün doğrudan temel sınıf en az sınıf türü kendisini olarak erişilebilir olmalıdır ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)). Örneğin, bir derleme zamanı hatası için olduğu bir `public` öğesinden türetilen sınıfın bir `private` veya `internal` sınıfı.

Bir sınıf türünün doğrudan temel sınıf aşağıdaki türlerinden herhangi birini olmamalıdır: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`, veya `System.ValueType`. Ayrıca, bir genel sınıf bildiriminde kullanılamaz `System.Attribute` doğrudan veya dolaylı bir temel sınıf olarak.

Doğrudan temel sınıf belirtimi anlamını belirlemek çalışırken `A` bir sınıfın `B`, doğrudan temel sınıfını `B` geçici olarak kabul edilir `object`. Bunun anlamı bir temel sınıf belirtimi, özyinelemeli olarak olamaz sezgisel sağlar kendisine bağımlı. Örnek:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
temel sınıf belirtiminde hata beri bulunduğu `A<C.B>` doğrudan temel sınıfını `C` olduğu kabul edilir `object`ve bu nedenle (kurallarına göre [Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) `C` olmadığı kabul edilir bir üyeye sahip `B`.

Temel bir sınıf türünün doğrudan temel sınıfı ve temel sınıfları sınıflardır. Diğer bir deyişle, temel sınıfların doğrudan temel sınıf ilişkisinin geçişli kapatma kümesidir. Yukarıdaki örnekte, temel sınıflarını başvuran `B` olan `A` ve `object`. Örnekte
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
Taban sınıflarını `D<int>` olan `C<int[]>`, `B<IComparable<int[]>>`, `A`, ve `object`.

Sınıf dışında `object`, her sınıf türünde tam olarak bir doğrudan temel sınıf. `object` Sınıf doğrudan temel sınıfa sahip ve diğer sınıfların ultimate temel sınıftır.

Bir sınıf, `B` bir sınıftan türetilen `A`, bir derleme zamanı hata için `A` bağlıdır `B`. Bir sınıf ***doğrudan bağlı*** doğrudan temel sınıfı (varsa) ve ***doğrudan bağlı*** içinde bunu hemen iç içe geçmiş (varsa) sınıfı. Bu tanımı verildiğinde, tam bir sınıf olduğu sınıf yansıma ve geçişli kapatılmasını kümesidir ***doğrudan bağlı*** ilişki.

Örnek
```csharp
class A: A {}
```
sınıf kendisine güveniyor hatalı olmasıdır. Benzer şekilde, örneğin
```csharp
class A: B {}
class B: C {}
class C: A {}
```
hata, sınıfları döngüsel olarak kendilerini üzerinde bağımlı olmasıdır. Son olarak, örnek
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
sonuçları bir derleme zamanı hatası nedeniyle `A` bağlıdır `B.C` (doğrudan temel sınıfı), bağımlı olan `B` (kapsayan sınıfı) döngüsel olarak bağımlı olan `A`.

Bir sınıf içinde iç içe sınıflarda benzemez unutmayın. Örnekte
```csharp
class A
{
    class B: A {}
}
```
`B` bağımlı `A` (çünkü `A` hem doğrudan temel sınıfı, hem de kapsayan sınıfı), ancak `A` bağlı değildir `B` (bu yana `B` bir temel sınıf ya da kapsayan bir sınıfı değil `A` ). Bu nedenle, örneğin geçerli değil.

Öğesinden türetilen mümkün değil bir `sealed` sınıfı. Örnekte
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
sınıf `B` hata türetmek çalışır çünkü `sealed` sınıfı `A`.

#### <a name="interface-implementations"></a>Arabirim uygulamaları

A *class_base* belirtimi içinde çalışması sınıfı söyledi doğrudan belirli bir arabirim türleri uygulamak için arabirim türlerinin bir listesini içerebilir. Arabirim uygulamalarına açıklanmıştır daha ayrıntılı olarak [arabirimi uygulamaları](interfaces.md#interface-implementations).

### <a name="type-parameter-constraints"></a>Tür parametresi kısıtlamaları

Genel tür ve yöntem bildirimlerinde isteğe bağlı olarak belirtebilirsiniz tür parametresi kısıtlamaları dahil ederek *type_parameter_constraints_clause*s.

```antlr
type_parameter_constraints_clause
    : 'where' type_parameter ':' type_parameter_constraints
    ;

type_parameter_constraints
    : primary_constraint
    | secondary_constraints
    | constructor_constraint
    | primary_constraint ',' secondary_constraints
    | primary_constraint ',' constructor_constraint
    | secondary_constraints ',' constructor_constraint
    | primary_constraint ',' secondary_constraints ',' constructor_constraint
    ;

primary_constraint
    : class_type
    | 'class'
    | 'struct'
    ;

secondary_constraints
    : interface_type
    | type_parameter
    | secondary_constraints ',' interface_type
    | secondary_constraints ',' type_parameter
    ;

constructor_constraint
    : 'new' '(' ')'
    ;
```

Her *type_parameter_constraints_clause* belirtecini oluşur `where`, bir tür parametre adından önce gelen, ardından bir iki nokta üst üste ve bu tür parametresi kısıtlamaları listesi. En fazla bir olabilir `where` her tür parametresi için yan tümcesi ve `where` yan tümceleri herhangi bir sırada listelenen. Gibi `get` ve `set` bir özellik erişimcisi belirteçlerinde `where` belirteci bir anahtar değil.

Verilen kısıtlamalar listesine bir `where` yan tümcesi, bu sırada aşağıdaki bileşenlerden birini içerebilir: tek bir birincil kısıtlaması, bir veya daha fazla ikincil kısıtlamalar ve oluşturucu kısıtlaması `new()`.

Birincil kısıtlaması bir sınıf türü olabilir veya ***başvuru türü kısıtlaması*** `class` veya ***değer türü kısıtlaması*** `struct`. İkincil bir kısıtlama olabilir bir *type_parameter* veya *INTERFACE_TYPE*.

Başvuru türü kısıtlaması tür parametresi için kullanılan bir tür bağımsız değişkeni bir başvuru türü olması gerektiğini belirtir. Tüm sınıf türleri, arabirim türlerinde, temsilci türleri, dizi türleri ve tür parametreleri bir başvuru türü (yukarıda tanımlandığı şekilde) olarak bilinen bu kısıtlamasını.

Değer türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeni null olmayan bir değer türü olması gerektiğini belirtir. Tüm NULL olmayan bir yapı türleri, sabit listesi türleri ve değer türü kısıtlamasına sahip tür parametreleri bu kısıtlamasını. Bir değer türü, null yapılabilir bir tür sınıflandırılan rağmen unutmayın ([boş değer atanabilir türler](types.md#nullable-types)) değer türü kısıtlamasını karşılamaz. Değer türü kısıtlamasına sahip bir tür parametresi de olamaz *constructor_constraint*.

İşaretçi türleri, tür bağımsız değişkenleri olarak hiçbir zaman izin verilir ve her iki başvuru türü veya değer türü kısıtlamaları karşılamak için dikkate alınmaz.

Bir kısıtlaması bir sınıf türü, bir arabirim türü veya tür parametresi olduğundan, bu tür ", bu tür parametresi için kullanılan her tür bağımsız değişkeni desteklemesi gereken bir minimum temel tür" belirtir. Oluşturulan tür ya da genel yöntemin kullanıldığı her tür bağımsız değişkeni derleme zamanında tür parametresi kısıtlamaları karşılaştırılır. Sağlanan tür bağımsız değişkeni açıklanan koşulları karşılaması gerekir [kısıtlamaları karşılamadığınızı](types.md#satisfying-constraints).

A *class_type* kısıtlamasını aşağıdaki kurallar gerekir:

*  Türü, sınıf türünde olmalıdır.
*  Türü olmamalıdır `sealed`.
*  Türü aşağıdaki türlerden biri olmamalıdır: `System.Array`, `System.Delegate`, `System.Enum`, veya `System.ValueType`.
*  Türü olmamalıdır `object`. Türetilen tüm türler için `object`, bu izin veriyorsa, bu tür bir kısıtlama hiçbir etkisi yoktur.
*  En fazla bir kısıtlaması belirtilen tür parametresi için bir sınıf türü olabilir.

Bir tür olarak belirtilen bir *INTERFACE_TYPE* kısıtlamasını aşağıdaki kurallar gerekir:

*  Türü bir arabirim türü olmalıdır.
*  Bir türü birden fazla kez belirtilmesi gerekir değil bir verilen `where` yan tümcesi.

Her iki durumda da kısıtlaması herhangi bir türü parametre türüyle veya kapsamında oluşturulan tür yöntem bildiriminde içerebilir ve bildirilen türü içerebilir.

Bir tür parametresi kısıtlaması en az olarak erişilebilir olarak belirtilen herhangi bir sınıf veya arabirim türü ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) genel tür veya yöntem bildirilen.

Bir tür olarak belirtilen bir *type_parameter* kısıtlamasını aşağıdaki kurallar gerekir:

*  Türü bir tür parametresi olması gerekir.
*  Bir türü birden fazla kez belirtilmesi gerekir değil bir verilen `where` yan tümcesi.

Buna ek olarak olmalıdır hiçbir döngü bağımlılık tarafından tanımlanan geçişli bir ilişki olduğu tür parametrelerinin bağımlılık grafiğinde:

*  Bir tür parametresi, `T` tür parametresi için bir kısıtlama olarak kullanılan `S` ardından `S` ***bağlıdır*** `T`.
*  Bir tür parametresi, `S` bir tür parametresine bağlı `T` ve `T` bir tür parametresine bağlı `U` ardından `S` ***bağlıdır*** `U`.

Bu ilişki göz önünde bulundurulduğunda, kendisine bağımlı (doğrudan veya dolaylı olarak) için bir tür parametresi için bir derleme zamanı hatası olduğundan.

Kısıtlamalardan bağımlı tür parametreleri arasında tutarlı olması gerekir. Tür parametresi `S` tür parametresine bağlı `T` sonra:

*  `T` değer türü kısıtlaması olamaz. Aksi takdirde, `T` etkili bir şekilde bunu korumalı `S` aynı türde olacak şekilde zorlanması `T`, iki tür parametreleri gereği ortadan kaldırılır.
*  Varsa `S` sonra değer türü kısıtlamasına sahip `T` değil olmalıdır bir *class_type* kısıtlaması.
*  Varsa `S` sahip bir *class_type* kısıtlaması `A` ve `T` sahip bir *class_type* kısıtlaması `B` olmalıdır sonra bir kimlik dönüştürme ya da örtük başvuru dönüştürmesi `A` için `B` veya bir örtük bir başvuru dönüştürme `B` için `A`.
*  Varsa `S` ayrıca tür parametresine bağlı `U` ve `U` sahip bir *class_type* kısıtlaması `A` ve `T` sahip bir *class_type* kısıtlaması`B` olmalıdır sonra bir kimlik dönüştürme ya da örtük bir başvuru dönüştürmesi `A` için `B` veya bir örtük bir başvuru dönüştürme `B` için `A`.

İçin geçerli olduğundan `S` değer türü kısıtlaması olmasını ve `T` başvuru türü kısıtlamasına sahip. Etkili bir şekilde bu sınırlar `T` türlerine `System.Object`, `System.ValueType`, `System.Enum`ve herhangi bir arabirim türü.

Varsa `where` yan tümcesi bir tür parametresi için bir oluşturucu kısıtlaması içerir (formu olan `new()`), kullanmak mümkün mü `new` türünün örneğini oluşturmak için işleç ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)). Kullanılan bağımsız değişkeni girin Oluşturucu kısıtlaması tür parametreyle (Bu oluşturucu örtük olarak mevcut herhangi bir değer türü için) ortak bir parametresiz oluşturucusu olmalıdır veya değer türü kısıtlaması veya Oluşturucu kısıtlaması (bkz: sahip bir tür parametresi olmalıdır [Tür parametresi kısıtlamaları](classes.md#type-parameter-constraints) Ayrıntılar için).

Kısıtlamaları örnekleri şunlardır:
```csharp
interface IPrintable
{
    void Print();
}

interface IComparable<T>
{
    int CompareTo(T value);
}

interface IKeyProvider<T>
{
    T GetKey();
}

class Printer<T> where T: IPrintable {...}

class SortedList<T> where T: IComparable<T> {...}

class Dictionary<K,V>
    where K: IComparable<K>
    where V: IPrintable, IKeyProvider<K>, new()
{
    ...
}
```

Aşağıdaki örnekte hata çünkü tür parametrelerinin bağımlılık grafiğinde bir döngü neden olur:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Aşağıdaki örnekler, ek geçersiz durumlar gösterir:
```csharp
class Sealed<S,T>
    where S: T
    where T: struct        // Error, T is sealed
{
    ...
}

class A {...}

class B {...}

class Incompat<S,T>
    where S: A, T
    where T: B                // Error, incompatible class-type constraints
{
    ...
}

class StructWithClass<S,T,U>
    where S: struct, T
    where T: U
    where U: A                // Error, A incompatible with struct
{
    ...
}
```

***Etkin bir temel sınıf*** bir tür parametresini `T` şu şekilde tanımlanır:

*  Varsa `T` birincil hiçbir kısıtlama veya tür parametresi kısıtlamaları, etkili temel sınıfı `object`.
*  Varsa `T` değer türü kısıtlaması vardır, geçerli bir temel sınıf `System.ValueType`.
*  Varsa `T` sahip bir *class_type* kısıtlaması `C` ancak hiçbir *type_parameter* kısıtlamaları, etkili temel sınıfı olan `C`.
*  Varsa `T` hiçbir *class_type* kısıtlama ancak sahip bir veya daha fazla *type_parameter* kısıtlamaları, etkili temel sınıfındaki en encompassed türüdür ([yükseltilmiş dönüştürme işleçler](conversions.md#lifted-conversion-operators)) etkili temel sınıfları kümesi kendi *type_parameter* kısıtlamaları. Tutarlık kuralları en encompassed bir tür olduğundan emin olun.
*  Varsa `T` hem de bir *class_type* kısıtlaması ve bir veya daha fazla *type_parameter* kısıtlamaları, etkili temel sınıfındaki en encompassed türüdür ([yükseltilmiş dönüştürme işleçler](conversions.md#lifted-conversion-operators)) oluşan kümesindeki *class_type* kısıtlamasını `T` ve etkili temel sınıfları, *type_parameter* kısıtlamaları. Tutarlık kuralları en encompassed bir tür olduğundan emin olun.
*  Varsa `T` başvuru türü kısıtlaması ancak Hayır *class_type* kısıtlamaları, etkili temel sınıfı olan `object`.

T bir kısıtlama varsa bu kurallar amacıyla `V` diğer bir deyişle bir *value_type*, bunun yerine en belirgin taban türünü kullanın `V` diğer bir deyişle bir *class_type*. Hiçbir zaman içinde açıkça belirtilen bir kısıtlama olabilir, ancak kısıtlamaları dahilinde bir genel yöntem, örtük olarak geçersiz kılan bir yöntem bildiriminde veya bir arabirim yönteminin açık bir uygulama tarafından devralınır oluşabilir.

Bu kurallar etkin bir temel sınıf her zaman olduğundan emin olun. bir *class_type*.

***Etkili arabirimi kümesi*** bir tür parametresini `T` şu şekilde tanımlanır:

*  Varsa `T` hiçbir *secondary_constraints*, etkili arabirimi kendine boştur.
*  Varsa `T` sahip *INTERFACE_TYPE* kısıtlamaları ancak Hayır *type_parameter* kısıtlamaları, etkili arabirimi kendine olan bir dizi *INTERFACE_TYPE* kısıtlamaları.
*  Varsa `T` hiçbir *INTERFACE_TYPE* ancak kısıtlamalara *type_parameter* kısıtlamaları, etkili arabirimi kendine olan etkili arabirimi kümesi birleşimi, *type_ parametre* kısıtlamaları.
*  Varsa `T` hem de *INTERFACE_TYPE* kısıtlamaları ve *type_parameter* kısıtlamaları, etkili arabirimi kendine olan bir dizi birleşimi *INTERFACE_TYPE* kısıtlamalar ve geçerli arabirimi kümeleri kendi *type_parameter* kısıtlamaları.

Bir tür parametresi ***bir başvuru türü olduğu bilinen*** başvuru türü kısıtlaması olan veya bunun geçerli bir taban sınıf değil `object` veya `System.ValueType`.

Kısıtlanmış bir tür parametre türü değerlerinin kısıtlamalar tarafından kapsanan örneği üyelerine erişmek için kullanılabilir. Örnekte
```csharp
interface IPrintable
{
    void Print();
}

class Printer<T> where T: IPrintable
{
    void PrintOne(T x) {
        x.Print();
    }
}
```
yöntemlerinin `IPrintable` doğrudan çağrılabilir `x` çünkü `T` her zaman uygulamak için kısıtlı `IPrintable`.

### <a name="class-body"></a>Sınıfın gövdesi

*Class_body* söz konusu sınıfın üyeleri bir sınıfı tanımlar.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Kısmi türler

Birden çok tür bildirimi bölünebilir ***kısmi tür bildirimleri***. Program derleme zamanı ve çalışma zamanı işlenmesi sırasında kalan tek bir bildirimi olarak kabul edilir kullanılması türü bildirimi bu bölümde, kurallara, parça oluşturulur.

A *class_declaration*, *struct_declaration* veya *interface_declaration* içeriyorsa, kısmi türü bildirimi temsil eden bir `partial` değiştiricisi. `partial` bir anahtar sözcük değildir ve anahtar sözcükleri hemen birini önce görünürse yalnızca bir değiştirici olarak davranan `class`, `struct` veya `interface` tür bildiriminde veya türünden önce `void` yöntem bildiriminde. Diğer bağlamlarda Bu, normal bir tanımlayıcı olarak kullanılabilir.

Her bir parçasının kısmi türü bildirimi içermelidir bir `partial` değiştiricisi. Aynı ada sahip olmalıdır ve aynı ad alanı veya tür bildirimi diğer bölümleri olarak bildirilmelidir. `partial` Değiştiricisi belirten ek bölümlerini türü bildirimi başka bir yerde bulunabilir, ancak bu tür bölümler varlığını zorunlu değildir; eklemek bir tür için tek bir bildirimde ile geçerli `partial` değiştiricisi.

Parçaları bir tek tür bildirimi derleme zamanında birleştirilebilir, kısmi bir türde tüm parçalarını birlikte derlenmelidir. Kısmi türler özellikle genişletilmesi önceden derlenmiş türler izin vermez.

İç içe geçmiş türler belirtilebilir birden çok bölümlerinde kullanarak `partial` değiştiricisi. Genellikle, kapsayan tür kullanarak bildirildiği `partial` iyi ve iç içe türün her bir parçası olarak bildirilen gibi farklı bir kapsayan tür parçası.

`partial` Değiştiricisi temsilci veya sabit listesi bildirimlerinde izin verilmez.

### <a name="attributes"></a>Öznitelikler

Kısmi bir türün öznitelikleri, belirsiz bir sırayla her biriyle özniteliklerini bir araya getirerek belirlenir. Özniteliğin birden çok parça için de yerleştirilirse, öznitelik türünde birden çok kez belirtmeye eşdeğerdir. Örneğin, iki bölümü:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
gibi bir bildirime eşdeğerdir:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Tür parametrelerindeki öznitelikleri benzer bir şekilde birleştirin.

### <a name="modifiers"></a>Değiştiriciler

Ne zaman bir kısmi tür bildirimi içeren bir erişilebilirlik belirtimi ( `public`, `protected`, `internal`, ve `private` değiştiriciler) erişilebilirlik belirtimi içeren diğer tüm bölümleri ile kabul etmesi gerekir. Kısmi bir türde bir parçası erişilebilirlik belirtimi içeriyorsa, türü uygun varsayılan erişilebilirlik verilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)).

İç içe türün bir veya daha fazla kısmi bildirimleri içeriyorsa bir `new` değiştiricisi, iç içe türün devralınan bir üyeyi gizler, hiçbir uyarı bildirilen ([devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)).

Bir sınıfın bir veya daha fazla kısmi bildirimleri içeriyorsa bir `abstract` değiştiricisi, bir sınıf olarak kabul edilir soyut ([soyut sınıflar](classes.md#abstract-classes)). Aksi takdirde, sınıf, soyut olmayan olarak kabul edilir.

Bir sınıfın bir veya daha fazla kısmi bildirimleri içeriyorsa bir `sealed` değiştiricisi, sınıfın korumalı kabul edilir ([sınıflar korumalı](classes.md#sealed-classes)). Aksi takdirde, sınıf olarak kabul edilir korumasız.

Bir sınıf hem soyut hem korumalı olamaz unutmayın.

Zaman `unsafe` değiştiricisi, bir kısmi tür bildiriminde kullanıldığında, yalnızca güvenli olmayan bir bağlam, belirli bir parçası olarak kabul edilir ([güvensiz bir bağlamı](unsafe-code.md#unsafe-contexts)).

### <a name="type-parameters-and-constraints"></a>Tür parametreleri ve kısıtlamalar

Bir genel türü birden çok bölümlerinde bildirirse, her parça türü parametreleri belirtmelidir. Her parça sırada tür parametreleri aynı sayıda ve her tür parametresi için aynı ada sahip olmalıdır.

Ne zaman bir kısmi genel tür bildirimi içeren kısıtlamaları (`where` yan tümceleri), kısıtlamaları kısıtlamaları içeren diğer tüm bölümleri ile kabul etmesi gerekir. Özellikle, aynı tür parametrelerinin kısıtlamaları kısıtlamaları içeren her bir parçası olması gerekir ve her tür parametresi için kümeleri birincil, ikincil ve Oluşturucusu kısıtlamaları eşdeğer olması gerekir. Bunlar aynı üyeleri içeriyorsa, iki kısıtlamaları kümesini eşdeğerdir. Tür parametreleri olarak kabul edilir, hiçbir bölümü kısmi ve genel bir türün tür parametresi kısıtlamaları belirtiyorsa, sınırlandırılmamış.

Örnek
```csharp
partial class Dictionary<K,V>
    where K: IComparable<K>
    where V: IKeyProvider<K>, IPersistable
{
    ...
}

partial class Dictionary<K,V>
    where V: IPersistable, IKeyProvider<K>
    where K: IComparable<K>
{
    ...
}

partial class Dictionary<K,V>
{
    ...
}
```
etkili bir şekilde kısıtlamaları (ilk iki) içeren bu bölümleri aynı birincil, ikincil ayarlayın ve aynı türde parametreler kümesi için oluşturucu kısıtlamaları belirttiğinden sırasıyla doğrudur.

### <a name="base-class"></a>Temel sınıf

Kısmi sınıf bildiriminin bir temel sınıf belirtimi içerdiğinde bir temel sınıf belirtimi içeren diğer tüm bölümleri ile kabul etmesi gerekir. Hiçbir bölümü, kısmi bir sınıfın temel sınıf belirtimi içeriyorsa, temel sınıf olur `System.Object` ([temel sınıflar](classes.md#base-classes)).

### <a name="base-interfaces"></a>Temel arabirimleri

Her bölüm üzerinde belirtilen temel arabirimleri birleşimi birden fazla bölümü içinde bildirilen bir tür için temel arabirimleri kümesidir. Belirli bir taban arabirimi her bölümü yalnızca bir kez adlandırılabilir, ancak aynı temel arabirimlerini adı birden fazla bölümü için izin verilir. Yalnızca bir adet herhangi belirli temel arabirim üyeleri olmalıdır.

Örnekte
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
sınıf için temel arabirimleri kümesi `C` olduğu `IA`, `IB`, ve `IC`.

Genellikle, her bölümü bu bölümünde bildirilen arabirimlere uygulaması sağlar. Ancak, bu bir gereksinim değildir. Bir bölümü üzerinde farklı bir parçası olarak bildirilen bir arabirim uygulamasını sağlayabilir:
```csharp
partial class X
{
    int IComparable.CompareTo(object o) {...}
}

partial class X: IComparable
{
    ...
}
```

### <a name="members"></a>Üyeler

Kısmi yöntemler dışında ([kısmi yöntemler](classes.md#partial-methods)), birden fazla bölümü içinde bildirilen bir türün üyeleri yalnızca her bir parçası olarak bildirilen üyeler kümesi birleşimi kümesidir. Tüm bölümlerinde tür bildirimi gövdesi aynı bildirim alanına paylaşma ([bildirimleri](basic-concepts.md#declarations)) ve her üye kapsamını ([kapsamları](basic-concepts.md#scopes)) tüm bölümlerin gövdeleri için genişletir. Erişilebilirlik etki alanı herhangi bir üyenin, her zaman kapsayan türdeki tüm parçaları içerir; bir `private` bir bölümünde bildirilen üye, başka bir kısmından ücretsiz olarak erişilebilir. Bu üyenin türü ile olmadığı sürece aynı türde birden fazla bölümü üye bildirmek için bir derleme zamanı hatası olduğu `partial` değiştiricisi.

```csharp
partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int y;
    }
}

partial class A
{
    int x;                     // Error, cannot declare x more than once

    partial class Inner        // Ok, Inner is a partial type
    {
        int z;
    }
}
```

Bir tür içindeki üyelerden sıralama nadiren C# kodu için önemlidir, ancak diğer diller ve ortamları ile arabirim olduğunda önemli olabilir. Bu gibi durumlarda birden fazla bölümü içinde bildirilen bir tür içindeki üyelerden sırası tanımlanmamıştır.

### <a name="partial-methods"></a>Kısmi yöntemler

Kısmi yöntemler tür bildirimi bir parçası tanımlanabilir ve başka bir programda uygulanır. Uygulama isteğe bağlıdır; hiçbir bölümü kısmi yöntem uygularsa, kısmi yöntem bildiriminde ve tüm çağrıları bölümleri birleşiminden elde edilen tür bildirimi çıkarılır.

Kısmi yöntemler erişim değiştiricileri tanımlayamazsınız ancak örtük olarak olan `private`. Kendi dönüş türü olmalıdır `void`, ve bunların parametreleri olamaz `out` değiştiricisi. Tanımlayıcı `partial` yalnızca hemen önce görünürse, özel bir anahtar sözcüğü bir yöntem bildiriminde olarak değerlendirilmiştir `void` yazın; Aksi halde normal bir tanımlayıcı olarak kullanılabilir. Kısmi bir yöntemin, arabirim yöntemleri olarak açıkça uygulayamaz.

Kısmi yöntem bildiriminin iki tür vardır: yöntem bildiriminde gövdesi bir noktalı virgül, bildirimi olarak kabul edilir bir ***kısmi yöntem bildiriminde tanımlama***. Gövde olarak belirtilen bir *blok*, bildirimi olarak kabul edilir bir ***kısmi yöntem bildiriminin yürüten***. Bir tür bildirimi bölümleri arasında yalnızca bir olabilir kısmi yöntem bildiriminde verilen imzayla tanımlama ve yalnızca bir kısmi yöntem bildiriminde verilen imzayla uygulama olabilir. Uygulama kısmi yöntem bildirimi verilir, kısmi yöntem bildiriminde tanımlama karşılık gelen bulunmalı ve bildirimleri olarak eşleşmelidir aşağıdaki konumlarda arar:

* (Mutlaka aynı sırada rağmen) bildirimleri aynı değiştiricilere sahip olmalıdır, yöntem adı, türü parametre sayısı ve parametre sayısı.
* Parametrelerini ilgili bildirimleri aynı değiştiricilere (mutlaka aynı sırada rağmen) olmalıdır ve aynı türleri (modül tür parametresi adlarına farklılıkları).
* Karşılık gelen tür parametreleri bildirimlerinde aynı kısıtlamalara (modül tür parametresi adlarına farklılıkları) olması gerekir.

Uygulama kısmi yöntem bildirimi aynı bölümünde karşılık gelen tanımlama kısmi yöntem bildirimi olarak görünebilir.

Yalnızca tanımlayan kısmi bir yöntemin aşırı yükleme çözümlemesinde rol oynar. Bu nedenle, uygulama bildirimi verilen olsun veya olmasın, çağrı ifadeleri kısmi yöntem çağrılarına çözülebilir. Kısmi bir yöntem her zaman döndürüldüğünden `void`, tür çağrı ifadeleri ifade deyimleri her zaman olur. Ayrıca, kısmi bir yöntemin örtük olarak olduğundan `private`, bu tür deyimler her zaman içinde kısmi yöntem bildirimi türü bildirimin parçalardan biri içinde gerçekleşir.

Kısmi türü bildirimi hiçbir bölümü, belirli bir kısmi yönteminin uygulama bildirimi içeriyorsa, onu çağırır herhangi bir ifade deyimi yalnızca Birleşik türü bildirimden kaldırıldı. Bu nedenle çağrısı ifadesi, bağlı tüm ifadeleri de dahil olmak üzere çalışma zamanında bir etkisi yoktur. Kısmi yöntem da kaldırılır ve birleşik tür bildirimi üyesi olur.

Uygulama bildirimi için belirtilen bir kısmi yöntem yoksa, kısmi yöntem çağrılarını korunur. Kısmi yöntem artış olduğunda uygulama kısmi yöntem bildirimi aşağıdaki istisnalar dışında benzer bir yöntem bildiriminde sağlar:

* `partial` Değiştiricisi dahil değildir
* Sonuçta elde edilen yöntem bildiriminde öznitelikleri birleştirilmiş tanımlama ve uygulama kısmi yöntem bildiriminde belirtilmemiş sırayla öznitelikleridir. Yinelenenler kaldırılamaz.
* Sonuçta elde edilen yöntem bildiriminde parametrelerinin özniteliklerinde birleştirilmiş tanımlama ve uygulama kısmi yöntem bildiriminde ilgili parametreleri belirtilmemiş sırayla öznitelikleridir. Yinelenenler kaldırılamaz.

Bir tanımlama bildirimi ancak bir uygulama bildirimi verilmiştir, kısmi bir yöntem için M aşağıdaki kısıtlamalar uygulanır:

* Yöntemi temsilci oluşturmak için bir derleme zamanı hata ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)).
* Başvurmak için bir derleme zamanı hata `M` bir ifade ağacı türüne dönüştürülmüş anonim bir işlevin içindeki ([ifade ağacı türlerine dönüştürme işlemi anonim İşlev değerlendirmesi](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* Çağrısından bir parçası olarak gerçekleşen ifadeleri `M` belirli atama onayına durumunu etkilemez ([belirli atama onayına](variables.md#definite-assignment)), bu potansiyel olarak yol açabilir. derleme zamanı hatalarına.
* `M` bir uygulama için giriş noktası olamaz ([uygulama başlatma](basic-concepts.md#application-startup)).

Kısmi yöntemler sağlayan bir araç tarafından oluşturulan bir örneğin, başka bir bölümü davranışını özelleştirmek için bir tür bildiriminde kullanıldığında bir parçası için kullanışlıdır. Aşağıdaki kısmi sınıf bildirimi göz önünde bulundurun:
```csharp
partial class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    partial void OnNameChanging(string newName);

    partial void OnNameChanged();
}
```

Bu sınıf diğer tüm bölümler derlenir tanımlayan kısmi yöntem bildiriminin ve bunların çağrılarını kaldırılacak ve sonuçta elde edilen birleştirilmiş sınıf bildiriminin aşağıdaki eşdeğer olacaktır:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set { name = value; }
    }
}
```

Başka bir bölümü, ancak uygulama bildirimi olan kısmi yöntemler sağlayan verilen varsayın:
```csharp
partial class Customer
{
    partial void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    partial void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

Sonuçta elde edilen birleştirilmiş sınıf bildiriminin ardından aşağıdaki denk olacaktır:
```csharp
class Customer
{
    string name;

    public string Name {
        get { return name; }
        set {
            OnNameChanging(value);
            name = value;
            OnNameChanged();
        }

    }

    void OnNameChanging(string newName)
    {
        Console.WriteLine("Changing " + name + " to " + newName);
    }

    void OnNameChanged()
    {
        Console.WriteLine("Changed to " + name);
    }
}
```

### <a name="name-binding"></a>Bağlama adı

Her bir parçasının genişletilebilir bir tür aynı ad alanı içinde bildirilmelidir olsa da, parça genellikle farklı bir ad alanı bildirimi içinde yazılır. Bu nedenle, farklı `using` yönergeleri ([yönergeleri kullanarak](namespaces.md#using-directives)) için her bir bölümü bulunabilir. Basit adları yorumlarken ([anlam çıkarma](expressions.md#type-inference)) bir bölümü, yalnızca içinde `using` yönergeleri bu bölümü kapsayan ad alanı bildirimlerinden biri olarak kabul edilir. Bu, farklı bölümleri farklı anlamlara sahip aynı tanımlayıcıda neden olabilir:
```csharp
namespace N
{
    using List = System.Collections.ArrayList;

    partial class A
    {
        List x;                // x has type System.Collections.ArrayList
    }
}

namespace N
{
    using List = Widgets.LinkedList;

    partial class A
    {
        List y;                // y has type Widgets.LinkedList
    }
}
```

## <a name="class-members"></a>Sınıf üyeleri

Bir sınıfın üyeleri tarafından tanıtılan üyeleri oluşur, *class_member_declaration*s ve üyelerini doğrudan temel sınıftan devralınan.

```antlr
class_member_declaration
    : constant_declaration
    | field_declaration
    | method_declaration
    | property_declaration
    | event_declaration
    | indexer_declaration
    | operator_declaration
    | constructor_declaration
    | destructor_declaration
    | static_constructor_declaration
    | type_declaration
    ;
```

Bir sınıf türünün üyeleri aşağıdaki kategorilere ayrılır:

*  Sınıf ile ilişkili sabit değerleri temsil eden sabitleri ([sabitleri](classes.md#constants)).
*  Sınıf değişkenler alanları ([alanları](classes.md#fields)).
*  Sınıfı tarafından gerçekleştirilen eylemler ve hesaplamalar uygulayan yöntemler ([yöntemleri](classes.md#methods)).
*  Tanımlama özellikleri adlı özellik ve okuma ve bu özellikleri yazma ile ilişkili eylem ([özellikleri](classes.md#properties)).
*  Sınıfı tarafından oluşturulan bildirimleri tanımlayan olayları ([olayları](classes.md#events)).
*  Aynı şekilde sıralanması sınıfının örneklerini izin dizin oluşturucular (sözdizimi kurallarına göre) olarak diziler ([dizin oluşturucular](classes.md#indexers)).
*  İşleçler, sınıf örnekleri için uygulanabilir ifade operatörlerini tanımlayın ([işleçleri](classes.md#operators)).
*  Örnek sınıf örnekleri başlatmak için gerekli eylemleri uygulayan oluşturucular ([örnek oluşturucular](classes.md#instance-constructors))
*  Sınıf örneğini kalıcı olarak atılmadan önce gerçekleştirilecek eylemleri uygulayan Yıkıcılar ([yok ediciler](classes.md#destructors)).
*  Sınıfını başlatmak için gerekli eylemleri uygulayan statik oluşturucular ([statik oluşturucular](classes.md#static-constructors)).
*  Sınıf için yerel türleri temsil eden türleri ([türlerin](classes.md#nested-types)).

Yürütülebilir kod içerebilecek üyeleri olarak bilinir topluca *işlev üyeleri* sınıf türü. Bir sınıf türünün işlev üyeleri, yöntemleri, özellikleri, olayları, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler ve bu sınıf türünün statik oluşturucular ' dir.

A *class_declaration* yeni bir bildirim alanı oluşturur ([bildirimleri](basic-concepts.md#declarations)) ve *class_member_declaration*hemen içerdiği s *sınıfı _declaration* bu bildirim alanına yeni üye tanıtır. Aşağıdaki kurallar geçerli *class_member_declaration*: %s

*  Örnek oluşturucuları ve yıkıcıları statik oluşturucular kapsayan sınıf adıyla aynı olmalıdır. Diğer tüm üyeleri hemen kapsayan sınıfın adından farklı adlara sahip olması gerekir.
*  Bir sabit, alan, özelliği, olay veya türü adı aynı sınıfta bildirilen tüm diğer üyelerinin adları farklı olmalıdır.
*  Bir yöntemin adını, diğer tüm olmayan-aynı sınıfta bildirilen yöntemlerin adlarından farklı olmalıdır. Ayrıca, imza ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir yöntem imzaları aynı sınıfta bildirilen tüm diğer yöntemlerden farklı olmalıdır ve yalnızca farklı imzaları aynı sınıfta bildirilen iki yöntem olamaz tarafından `ref` ve `out`.
*  Bir örnek oluşturucusunda imzası aynı sınıfta bildirilen tüm diğer örnek oluşturucuları imzalarını farklı olmalıdır ve sadece onun tarafından farklı imzaları aynı sınıfta bildirilen iki Oluşturucu olmayabilir `ref` ve `out`.
*  Bir dizin oluşturucu imzasının aynı sınıfta bildirilen tüm diğer dizin imzalarını farklı olmalıdır.
*  Bir işleç imzası, imzaları aynı sınıfta bildirilen tüm diğer işleçlerin kadar farklı olmalıdır.

Devralınan bir sınıf türü üyeleri ([devralma](classes.md#inheritance)) bir sınıf bildirimi alanının parçası değildir. Bu nedenle, türetilmiş bir sınıf olarak (Bu aslında devralınan üyeyi gizler) devralınmış bir üyeyi aynı ad veya imzaya sahip bir üye bildirmek için kullanılabilir.

### <a name="the-instance-type"></a>Örnek türü

Her sınıf bildirimi ilişkili ilişkili bir türü vardır ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)), ***örnek türünü***. Genel sınıf bildirimi, örnek türüne oluşturulan bir tür oluşturarak biçimlendirilmiş ([oluşturulan türler](types.md#constructed-types)) türü bildirimden sağlanan tür her biriyle ilgili olan bağımsız değişkenleri tür parametresi. Örnek türü tür parametreleri kullandığından, yalnızca tür parametreleri kapsamda nerede kullanılabilir; diğer bir deyişle, sınıf bildirimi içinde. Örnek türü türüdür `this` sınıf bildirimi içinde yazılan kod için. Genel olmayan sınıflar için örneği yalnızca bildirilen sınıf türüdür. Aşağıdaki örnek türleri ile birlikte birkaç sınıf bildirimi gösterilmektedir: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Oluşturulan türler üyeleri

Oluşturulan tür olmayan devralınan üyeleri, her biri için değiştirerek elde edilen *type_parameter* karşılık gelen üye bildiriminde *type_argument* oluşturulan türü. Değiştirme işlemi anlam tür bildirimleri üzerinde temel alır ve yalnızca metin değiştirme değil.

Örneğin, genel bir sınıf bildiriminde belirtilen
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
oluşturulan tür `Gen<int[],IComparable<string>>` aşağıdaki üyeleri içerir:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Üyenin türü `a` genel sınıf bildiriminde `Gen` olan "iki boyutlu dizi `T`", bu nedenle üyenin türü `a` oluşturulmuş yukarıdaki "iki boyutlu dizi tek boyutlu dizi türüdür`int`", veya `int[,][]`.

Örnek işlev üyeleri, tür içinde `this` örneği türü ([örnek türü](classes.md#the-instance-type)) içeren bildirimi.

Bir genel sınıfın tüm üyeleri, doğrudan veya oluşturulan tür bir parçası olarak herhangi bir kapsayan sınıf türü parametreleri kullanabilirsiniz. Belirli bir kapatıldığı türü oluşturulan ([açık ve kapalı türleri](types.md#open-and-closed-types)) kullanılan çalışma zamanında, her bir tür parametresi kullanımının oluşturulan tür için sağlanan gerçek tür bağımsız değişken ile değiştirilir. Örneğin:
```csharp
class C<V>
{
    public V f1;
    public C<V> f2 = null;

    public C(V x) {
        this.f1 = x;
        this.f2 = this;
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>(1);
        Console.WriteLine(x1.f1);        // Prints 1

        C<double> x2 = new C<double>(3.1415);
        Console.WriteLine(x2.f1);        // Prints 3.1415
    }
}
```

### <a name="inheritance"></a>Devralma

Bir sınıf ***devralan*** doğrudan temel sınıf türü üyeleri. Devralma, bir sınıf örtük olarak örnek oluşturucuları ve yıkıcıları temel sınıfın statik oluşturucular dışında doğrudan temel sınıf türünün tüm üyelerini içeren anlamına gelir. Devralma önemli bazı yönleri şunlardır:

*  Devralma geçişlidir. Varsa `C` türetilir `B`, ve `B` türetilir `A`, ardından `C` bildirilen üyelerini devralır `B` bildirilen üyeler yanı sıra `A`.
*  Türetilmiş bir sınıf doğrudan temel sınıfı genişletir. Türetilmiş bir sınıf devralır bu yeni üyeler ekleyebilirsiniz, ancak devralınan bir üyeyi tanımını kaldırılamaz.
*  Örnek oluşturucuları ve yıkıcıları statik oluşturucular devralınmaz var, ancak diğer tüm üyeleri, bağımsız olarak kendi bildirilen erişilebilirliği ([üye erişimi](basic-concepts.md#member-access)). Ancak, kendi bildirilen erişilebilirliği bağlı olarak, devralınan üyeler türetilen bir sınıfta erişilebilir olmayabilir.
*  Türetilmiş sınıf ***Gizle*** ([devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) aynı ad veya imzaya sahip yeni üyeler bildirerek devralınan üyeleri. Devralınmış bir üyeyi gizlemek, ancak bu üyede kaldırmaz unutmayın; yalnızca o üyesi türetilmiş sınıf üzerinden doğrudan erişilemez olmasını sağlar.
*  Tüm örnek alanları sınıfı ve temel sınıflarının ve örtük olarak bildirilen bir dizi bir sınıf örneğini içerir ([örtük bir başvuru dönüşümleri](conversions.md#implicit-reference-conversions)) bir türetilmiş sınıf türünden temel sınıf türlerinden birinin bulunmaktadır. Bu nedenle, bazı türetilmiş sınıfın bir örneğine başvuru temel sınıflarını herhangi bir örneğe bir başvuru olarak işlenebilir.
*  Bir sınıf sanal yöntemler, özellikler ve dizin oluşturucular bildirebilir ve türetilen sınıflar bu işlev üyeler uygulamasını geçersiz kılabilir. Bu müşteriyle üzerinden işlevi üyenin çağrıldığı örnek çalışma zamanı türüne bağlı olarak değişir işlevi üye çağrısı tarafından gerçekleştirilen eylemlerin polimorfik davranışlar sergilemesine izin sınıflar sağlar.

Oluşturulan sınıf türü devralınan üye olan hemen temel sınıf türünün üyeleri ([temel sınıflar](classes.md#base-classes)), hangi oluşturulan tür tür bağımsız değişkenleri karşılık gelen türü her geçtiği için değiştirerek bulunur parametrelerinde *class_base* belirtimi. Bu üyeleri, buna karşılık, her biri için getirilmesiyle dönüştürülür *type_parameter* karşılık gelen üye bildiriminde *type_argument* , *class_base* belirtimi.

```csharp
class B<U>
{
    public U F(long index) {...}
}

class D<T>: B<T[]>
{
    public T G(string s) {...}
}
```

Yukarıdaki örnekte, yapılandırılmış türdedir `D<int>` olmayan devralınan bir üyenin `public int G(string s)` tür bağımsız değişkeni değiştirerek elde `int` tür parametresi için `T`. `D<int>` devralınan bir sınıf bildiriminde üye de sahip `B`. Bu devralınan üye, temel sınıf türünü belirleyerek belirlenir `B<int[]>` , `D<int>` değiştirerek tarafından `int` için `T` temel sınıf belirtiminde `B<T[]>`. Ardından, bir tür bağımsız değişkeni olarak `B`, `int[]` için yerine `U` içinde `public U F(long index)`, devralınan üye sonuçlanmıyor `public int[] F(long index)`.

### <a name="the-new-modifier"></a>New değiştiricisi

A *class_member_declaration* devralınmış bir üyeyi olarak aynı ad veya imzaya sahip bir üye bildirmek için izin verilir. Bu durumda, türetilmiş sınıf üyesi edilir ***Gizle*** temel sınıf üyesi. Devralınmış bir üyeyi gizlemek hata olarak kabul edilmez, ancak derleyici bir uyarı neden olmaz. Bu uyarının gösterilmemesi için türetilmiş sınıf üyesinin bildirimi içerebilir bir `new` türetilen üye temel üyeyi Gizle amaçlandığını belirtmek için değiştiricisi. Bu konunun ele alındığı daha ayrıntılı olarak [devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance).

Varsa bir `new` değiştiricisi devralınan bir üyeyi gizlemek olmayan bir bildiriminde dahil, bir uyarı belirlememişse bu bağlamda verilir. Kaldırarak bu uyarı bastırılır `new` değiştiricisi.

### <a name="access-modifiers"></a>Erişim değiştiricileri

A *class_member_declaration* bildirilen erişilebilirliği beş olası tür herhangi biri olabilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)): `public`, `protected internal`, `protected`, `internal` , veya `private`. Dışında `protected internal` birleşimi, birden fazla erişim değiştiricisi belirtmek için bir derleme zamanı hatası olduğundan. Olduğunda bir *class_member_declaration* herhangi bir erişim değiştiricileri içermez `private` varsayılır.

### <a name="constituent-types"></a>Bağlı türleri

Bu üye bağlı tür üyesi bildiriminde kullanılan türler çağrılır. Olası bağlı bir sabit, alan, özelliği, olay veya dizin oluşturucu türü, bir yöntem veya işlecin dönüş türü ve parametre türleri, yöntem, dizin oluşturucu, işleci veya örnek oluşturucusu türleridir. Bağlı üye türleri en az bu üye olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

### <a name="static-and-instance-members"></a>Statik ve örnek üyeleri

Bir sınıf üyesi olan ya da ***statik üyeleri*** veya ***örnek üyeleri***. Genel olarak bakıldığında, statik üyeler sınıf türleri ve (sınıf türlerinin örneklerini) nesnelere ait örnek üyeleri ait olarak düşünmek kullanışlıdır.

Zaman alan, yöntem, özellik, olay, işleci veya Oluşturucu bildirimi içeren bir `static` değiştiricisi, statik bir üye bildirir. Ayrıca, bir sabit değer veya tür bildirimi statik üyesi örtük olarak bildiriyor. Statik üyeleri aşağıdaki özelliklere sahiptir:

*  Statik üye `M` başvurulduğundan bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `E` içeren türü belirtmek `M`. Bir derleme zamanı hata için `E` örneğini belirtmek için.
*  Statik alan belirli bir kapalı sınıf türünün tüm örnekleri tarafından paylaşılan tek bir depolama konumu belirtir. Belirli bir kapalı sınıf türü kaç örneklerin oluşturulduğu ne olursa olsun, yalnızca bir kopyasını statik bir alan yoktur.
*  Statik işlev üyesi (yöntem, özellik, olay, işleci veya Oluşturucu) belirli bir örneği üzerinde çalışmaz ve başvurmak için bir derleme zamanı hata `this` işlevi gibi bir üyesi olarak.

Zaman alan, yöntemi, özelliği, olay, dizin oluşturucu, oluşturucu veya yıkıcı bildirimi içermez bir `static` değiştiricisi, bir örnek üye bildirir. (Bir örnek üye statik olmayan üye adlandırılır.) Örnek üyeleri şu özelliklere sahiptir:

*  Bir örnek üyesi olduğunda `M` başvurulduğundan bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `E` içerenbirtürünbirörneğinibelirtmekgerekir`M`. Bir bağlama-time hata için `E` bir tür belirtmek için.
*  Bir sınıfın her örneği ayrı bir sınıfın tüm örnek alanları kümesi içerir.
*  Örnek işlevi üyesine (yöntemi, özelliği, dizin oluşturucu, örnek oluşturucusu veya yıkıcısı) belirli bir sınıf örneği üzerinde çalışır ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)).

Aşağıdaki örnek, statik erişmek için kuralları ve örnek üyeleri gösterilmektedir:
```csharp
class Test
{
    int x;
    static int y;

    void F() {
        x = 1;            // Ok, same as this.x = 1
        y = 1;            // Ok, same as Test.y = 1
    }

    static void G() {
        x = 1;            // Error, cannot access this.x
        y = 1;            // Ok, same as Test.y = 1
    }

    static void Main() {
        Test t = new Test();
        t.x = 1;          // Ok
        t.y = 1;          // Error, cannot access static member through instance
        Test.x = 1;       // Error, cannot access instance member through type
        Test.y = 1;       // Ok
    }
}
```

`F` Yöntemi gösterir, bir örnek işlevi üyesi içinde bir *simple_name* ([basit adları](expressions.md#simple-names)) örnek üyeleri hem statik üyeleri erişmek için kullanılabilir. `G` Yöntemi gösterir statik işlev üyesi onu aracılığıyla örnek üyesine erişmek için bir derleme zamanı hatası olduğu bir *simple_name*. `Main` Yöntemi gösterir, bir *member_access* ([üye erişimi](expressions.md#member-access)) örnekleri örnek üyeleri erişilen ve türleri statik üye erişilebilir.

### <a name="nested-types"></a>İç içe geçmiş türler

Bir sınıf veya yapı bildirimi içinde bildirilen bir tür olarak adlandırılan bir ***iç içe türü***. Bir derleme biriminde veya ad alanı içinde bildirilen bir tür olarak adlandırılan bir ***iç içe türü***.

Örnekte
```csharp
using System;

class A
{
    class B
    {
        static void F() {
            Console.WriteLine("A.B.F");
        }
    }
}
```
sınıf `B` sınıfı içinde bildirildiği için iç içe geçmiş bir türdür `A`ve sınıfı `A` bir derleme birimi içinde bildirildiği için bir iç içe olmayan türüdür.

#### <a name="fully-qualified-name"></a>Tam adı

Tam adı ([adları tam](basic-concepts.md#fully-qualified-names)) iç içe geçmiş bir tür için `S.N` burada `S` hangi türdeki türünün tam adı `N` bildirilir.

#### <a name="declared-accessibility"></a>Bildirilen erişilebilirliği

İç içe olmayan türleri olabilir `public` veya `internal` bildirilen erişilebilirliği ve sahip `internal` erişilebilirlik varsayılan olarak bildirilmiş. İç içe geçmiş türler biçimlerinden birini bildirilen erişilebilirliği çok, artı içeren türün bir sınıf veya yapı olup olmadığına bağlı olarak bildirilen erişilebilirliği, bir veya daha fazla başka biçimlerde olabilir:

*  Bir sınıfta bildirilen iç içe türü bildirilen erişilebilirliği, beş biçimlerden birini olabilir (`public`, `protected internal`, `protected`, `internal`, veya `private`) ve diğer sınıf üyeleri gibi varsayılan olarak `private` bildirildi Erişilebilirlik.
*  Bir yapı içinde bildirilen iç içe türü bildirilen erişilebilirliği, üç biçimlerden birini olabilir (`public`, `internal`, veya `private`) ve diğer yapı üyeleri gibi varsayılan olarak `private` erişilebilirlik bildirilir.

Örnek
```csharp
public class List
{
    // Private data structure
    private class Node
    { 
        public object Data;
        public Node Next;

        public Node(object data, Node next) {
            this.Data = data;
            this.Next = next;
        }
    }

    private Node first = null;
    private Node last = null;

    // Public interface
    public void AddToFront(object o) {...}
    public void AddToBack(object o) {...}
    public object RemoveFromFront() {...}
    public object RemoveFromBack() {...}
    public int Count { get {...} }
}
```
özel iç içe geçmiş bir sınıfı bildirir `Node`.

#### <a name="hiding"></a>Gizleme

İç içe geçmiş bir tür gizle ([ad gizleme](basic-concepts.md#name-hiding)) temel üye. `new` Değiştiricisi, iç içe geçmiş tür bildirimlerinde verilir, böylece gizleme açıkça ifade edilebilir. Örnek
```csharp
using System;

class Base
{
    public static void M() {
        Console.WriteLine("Base.M");
    }
}

class Derived: Base 
{
    new public class M 
    {
        public static void F() {
            Console.WriteLine("Derived.M.F");
        }
    }
}

class Test 
{
    static void Main() {
        Derived.M.F();
    }
}
```
iç içe geçmiş bir sınıfı göstermektedir `M` , yöntemini gizliyor `M` tanımlanan `Base`.

#### <a name="this-access"></a>Bu erişim

İç içe geçmiş bir tür ve kapsayan türü ile özel bir ilişkisi olmayan *this_access* ([bu erişim](expressions.md#this-access)). Özellikle, `this` kapsayan türdeki örneği üyelerine başvuru için bir iç içe geçmiş bir tür içinde kullanılamaz. İç içe türü, kapsayan türün örnek üyeleri için erişim gereken yere durumlarda sağlayarak erişim sağlanabilir `this` olarak iç içe geçmiş tür için oluşturucu bağımsız değişkeni bir kapsayan tür örneği için. Aşağıdaki örnek
```csharp
using System;

class C
{
    int i = 123;

    public void F() {
        Nested n = new Nested(this);
        n.G();
    }

    public class Nested
    {
        C this_c;

        public Nested(C c) {
            this_c = c;
        }

        public void G() {
            Console.WriteLine(this_c.i);
        }
    }
}

class Test
{
    static void Main() {
        C c = new C();
        c.F();
    }
}
```
Bu teknik gösterir. Örneği `C` örneği oluşturur `Nested` ve kendi geçirir `this` için `Nested`'s sonraki erişim sağlamak için oluşturucu `C`ait örnek üyeleri.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Kapsayan türdeki özel ve korumalı üyelerine erişimi

İç içe türü kapsayan tür sahip üyeleri dahil olmak üzere, kapsadığı tür için erişilebilir olan üyelerin tümüne erişebilir `private` ve `protected` erişilebilirlik bildirilir. Örnek
```csharp
using System;

class C 
{
    private static void F() {
        Console.WriteLine("C.F");
    }

    public class Nested 
    {
        public static void G() {
            F();
        }
    }
}

class Test 
{
    static void Main() {
        C.Nested.G();
    }
}
```
bir sınıfı göstermektedir `C` iç içe geçmiş bir sınıf içeren `Nested`. İçinde `Nested`, yöntem `G` statik metodu çağırır `F` tanımlanan `C`, ve `F` özel erişilebilirlik bildirdiği.

İç içe türü, kapsadığı tür temel bir türde tanımlanan korumalı üyeler da erişebilirsiniz. Örnekte
```csharp
using System;

class Base 
{
    protected void F() {
        Console.WriteLine("Base.F");
    }
}

class Derived: Base 
{
    public class Nested 
    {
        public void G() {
            Derived d = new Derived();
            d.F();        // ok
        }
    }
}

class Test 
{
    static void Main() {
        Derived.Nested n = new Derived.Nested();
        n.G();
    }
}
```
iç içe geçmiş sınıf `Derived.Nested` korumalı yöntem erişen `F` tanımlanan `Derived`ait temel sınıfı, `Base`kullanarak, bir örneği üzerinden çağırma `Derived`.

#### <a name="nested-types-in-generic-classes"></a>Genel sınıflar içinde iç içe geçmiş türler

Genel sınıf bildirimi, iç içe geçmiş tür bildirimleri içerebilir. Tür parametreleri kapsayan sınıfın içinde iç içe geçmiş türleri kullanılabilir. Bir iç içe tür bildiriminde kullanıldığında, yalnızca iç içe türü için geçerli olan ek tür parametreleri içerebilir.

Genel sınıf bildirimi içinde yer alan her tür bildirim örtük olarak bir genel tür bildirimidir. Genel tür içinde iç içe bir türe başvuru yazma, tür bağımsız değişkenlerinden dahil olmak üzere kapsayan bir oluşturulmuş tür adlandırılmalıdır. Ancak, gelen dış sınıf içinde iç içe türün nitelenmeden kullanılabilir; Dış sınıfın örnek türüne örtük olarak iç içe türün oluşturulurken kullanılabilir. Aşağıdaki örnek, oluşturulan oluşturulan bir tür belirtmek için üç farklı doğru şekilde gösterir. `Inner`; ilk iki eşdeğerdir:
```csharp
class Outer<T>
{
    class Inner<U>
    {
        public static void F(T t, U u) {...}
    }

    static void F(T t) {
        Outer<T>.Inner<string>.F(t, "abc");      // These two statements have
        Inner<string>.F(t, "abc");               // the same effect

        Outer<int>.Inner<string>.F(3, "abc");    // This type is different

        Outer.Inner<string>.F(t, "abc");         // Error, Outer needs type arg
    }
}
```

Hatalı olmasına rağmen stili programlama bir tür parametresi iç içe bir türde üye gizleyebilir veya dış türde bildirilen parametre türü:
```csharp
class Outer<T>
{
    class Inner<T>        // Valid, hides Outer's T
    {
        public T t;       // Refers to Inner's T
    }
}
```

### <a name="reserved-member-names"></a>Ayrılmış üye adları

Uygulama, temel C# çalışma zamanı uygulaması için bir özellik, olay veya dizin oluşturucu, her kaynak üye bildirimi kolaylaştırmak için iki yöntem imzaları üye bildirimi, adını ve türünü türüne göre ayırmanız gerekir. Bir derleme zamanı hata imzası olan, bunlardan birini ile eşleşen bir üye imzaları, ayrılmış temel alınan çalışma zamanı uygulama kullanmayan bile bildirmek bir program için bu ayırmalar kullanın.

Ayrılmış adlar bildirimleri İstemediğimiz, bu nedenle bunlar içinde üye araması katılmaz. Ancak, bir bildirim ayrılmış yöntem imzaları Devralmada katılmak ilişkili ([devralma](classes.md#inheritance)) ve ile gizli `new` değiştirici ([yeni değiştiricisini](classes.md#the-new-modifier)).

Bu adlar ayırma üç amaca hizmet eder:

*  Get için bir yöntem adı olarak normal bir tanımlayıcı kullanın veya erişim için C# dil özelliği ayarlamak temel uygulama izin verme.
*  Diğer diller, bir yöntem adı get için normal bir tanımlayıcı ile birlikte çalışmak veya erişim için C# dil özelliği ayarlamak izin verme.
*  Kaynak bir uyumlu derleyici tarafından kabul sağlamaya yardımcı olmak için başkası tarafından ayrılmış üyeyle ayrıntılarını adları tutarlı tüm C# uygulamaları arasında yaparak kabul edilir.

Bir yok edici bildirimini ([yok ediciler](classes.md#destructors)) de ayrılacak imza neden olur ([üye adları yok ediciler için ayrılmış](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Üye adları ayrılmış özellikleri

Bir özellik için `P` ([özellikleri](classes.md#properties)) türü `T`, aşağıdaki imzalar ayrılmıştır:

```csharp
T get_P();
void set_P(T value);
```

Salt okunur veya salt yazılır özelliği olsa bile, her iki imzaları ayrılmıştır.

Örnekte
```csharp
using System;

class A
{
    public int P {
        get { return 123; }
    }
}

class B: A
{
    new public int get_P() {
        return 456;
    }

    new public void set_P(int value) {
    }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        Console.WriteLine(a.P);
        Console.WriteLine(b.P);
        Console.WriteLine(b.get_P());
    }
}
```
bir sınıf `A` salt okunur özelliği tanımlar `P`, bu nedenle imzaları ayırma `get_P` ve `set_P` yöntemleri. Bir sınıf `B` türetildiği `A` ve bu ayrılmış imzalar her ikisi de gizler. Örnek çıktıyı üretir:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Üye adları ayrılmış olayları

Bir olayın `E` ([olayları](classes.md#events)) temsilci türünün `T`, aşağıdaki imzalar ayrılmıştır:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Dizin oluşturucular için ayrılmış üye adları

Bir dizin oluşturucu için ([dizin oluşturucular](classes.md#indexers)) türü `T` parametre listesiyle `L`, aşağıdaki imzalar ayrılmıştır:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Dizin Oluşturucu salt okunur veya salt yazılır olsa bile, her iki imzaları ayrılmıştır.

Ayrıca üye adı `Item` ayrılmıştır.

#### <a name="member-names-reserved-for-destructors"></a>Üye adları ayrılmış için Yıkıcılar

Yıkıcı içeren bir sınıf için ([yok ediciler](classes.md#destructors)), aşağıdaki imzası ayrılmıştır:
```csharp
void Finalize();
```

## <a name="constants"></a>Sabitler

A ***sabit*** sabit bir değeri temsil eden bir sınıf üyesi: derleme zamanında hesaplanabilir bir değer. A *constant_declaration* belirli bir türün bir veya daha fazla sabitleri tanıtır.

```antlr
constant_declaration
    : attributes? constant_modifier* 'const' type constant_declarators ';'
    ;

constant_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    ;

constant_declarators
    : constant_declarator (',' constant_declarator)*
    ;

constant_declarator
    : identifier '=' constant_expression
    ;
```

A *constant_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)), `new` değiştirici ([yeni değiştiricisini](classes.md#the-new-modifier)), Geçerli birlikte dört erişim değiştiricileri ([erişim değiştiricilerine](classes.md#access-modifiers)). Üyeleri tarafından bildirilen tüm öznitelikler ve değiştiriciler uygulamak *constant_declaration*. Sabit statik üyeleri olarak kabul edilir olsa bile bir *constant_declaration* gerektirir veya sağlar bir `static` değiştiricisi. Aynı değiştiricisi sabit bir bildirimde birden çok kez görünmesi için bir hatadır.

*Türü* , bir *constant_declaration* bildirim tarafından tanıtılan üyelerin türünü belirtir. Tür listesi tarafından izlenir *constant_declarator*s, her biri yeni bir üye tanıtır. A *constant_declarator* oluşan bir *tanımlayıcı* ardından üye adları bir "`=`" belirteci ve ardından bir *constant_expression* ([ Sabit ifadeler](expressions.md#constant-expressions)), üyenin değeri verir.

*Türü* Sabitte belirtilen bildirimi olmalıdır `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`e *enum_type*, veya bir *reference_type*. Her *constant_expression* hedef türünün veya hedef türe örtük bir dönüştürme tarafından dönüştürülebilir bir türün bir değeri yield gerekir ([örtük dönüştürmelerin](conversions.md#implicit-conversions)).

*Türü* bir sabit'ın en az sabit olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

Bir sabit değeri bir ifade kullanarak elde edilen bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)).

Bir sabit kendisini katılabilir bir *constant_expression*. Bu nedenle, bir sabit gerektiren herhangi bir yapı içinde kullanılabilir bir *constant_expression*. Bu tür yapıları örneklerindendir `case` etiketleri, `goto case` deyimleri `enum` üye bildirimleri, öznitelikler ve diğer sabit bildirimleri.

Bölümünde anlatıldığı gibi [sabit ifadeler](expressions.md#constant-expressions), *constant_expression* , derleme zamanında tam olarak değerlendirilebilecek bir ifadedir. Tek yolu, bir null olmayan değer oluşturmak için bu yana bir *reference_type* dışında `string` uygulamaktır `new` işleci ve bu yana `new` işleci izin verilmez bir *constant_ ifade*, sabitleri için tek olası değer *reference_type*s dışında `string` olduğu `null`.

Sabit değer için bir simgesel ad ne zaman isterseniz, ancak, bu değerin türü Sabit bildiriminde izin verilmez veya ne zaman değeri derleme zamanında deltanın hesaplanamaması bir *constant_expression*, `readonly` alan () [Salt okunur alanların](classes.md#readonly-fields)) bunun yerine kullanılabilir.

Birden fazla sabit bildirir sabit bir bildirimde öznitelikler, değiştiriciler ve türe sahip tek sabitlerin birden fazla bildirimi eşdeğerdir. Örneğin
```csharp
class A
{
    public const double X = 1.0, Y = 2.0, Z = 3.0;
}
```
eşdeğerdir
```csharp
class A
{
    public const double X = 1.0;
    public const double Y = 2.0;
    public const double Z = 3.0;
}
```

Sabitler, diğer sabitleri aynı programda bağımlılıkları döngüsel doğasını olmayan sürece bağımlı izin verilmez. Derleyici, sabit bildirimleri doğru sırada değerlendirilecek otomatik olarak düzenler. Örnekte
```csharp
class A
{
    public const int X = B.Z + 1;
    public const int Y = 10;
}

class B
{
    public const int Z = A.Y + 1;
}
```
Derleyici ilk değerlendirir `A.Y`, sonra değerlendirir `B.Z`ve son olarak değerlendirilirse `A.X`, değerleri üreten `10`, `11`, ve `12`. Sabit bildirimi programlardan sabitleri bağlı olabilir, ancak bu tür bağımlılıkları sadece tek yöndedir mümkündür. Yukarıdaki örnekte, başvuran `A` ve `B` bildirilen ayrı programlarında mümkün olacaktır `A.X` bağlıdır `B.Z`, ancak `B.Z` sonra değil, aynı anda bağımlı `A.Y`.

## <a name="fields"></a>Alanlar

A ***alan*** bir nesneyle veya sınıfla ilişkili bir değişkeni temsil eden bir üyesidir. A *field_declaration* belirli bir türün bir veya daha fazla alan sunar.

```antlr
field_declaration
    : attributes? field_modifier* type variable_declarators ';'
    ;

field_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'readonly'
    | 'volatile'
    | field_modifier_unsafe
    ;

variable_declarators
    : variable_declarator (',' variable_declarator)*
    ;

variable_declarator
    : identifier ('=' variable_initializer)?
    ;

variable_initializer
    : expression
    | array_initializer
    ;
```

A *field_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)), `new` değiştirici ([yeni değiştiricisini](classes.md#the-new-modifier)), Geçerli birleşimi dört erişim değiştiricileri ([erişim değiştiricilerine](classes.md#access-modifiers)) ve `static` değiştirici ([statik ve örnek alanları](classes.md#static-and-instance-fields)). Ayrıca, bir *field_declaration* içerebilir bir `readonly` değiştiricisi ([salt okunur alanların](classes.md#readonly-fields)) veya bir `volatile` değiştiricisi ([geçici alanları](classes.md#volatile-fields)) ikisini birden belirtmeyin. Üyeleri tarafından bildirilen tüm öznitelikler ve değiştiriciler uygulamak *field_declaration*. V deklaraci pole birden çok kez görünmesini aynı değiştiricisi için bir hatadır.

*Türü* , bir *field_declaration* bildirim tarafından tanıtılan üyelerin türünü belirtir. Tür listesi tarafından izlenir *variable_declarator*s, her biri yeni bir üye tanıtır. A *variable_declarator* oluşan bir *tanımlayıcı* ardından isteğe bağlı olarak, bu üye adları bir "`=`" belirteci ve *variable_initializer* ([ Değişken başlatıcılar](classes.md#variable-initializers)), bu üyenin ilk değeri verir.

*Türü* alanı en az alan olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

Bir alanın değeri bir ifade kullanarak alınan bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)). Kullanarak salt okunur olmayan bir alan değerini değiştiren bir *atama* ([atama işleçleri](expressions.md#assignment-operators)). Salt okunur olmayan bir alan değeri, değiştirilmiş ve elde edilen hem de sonek artırma ve azaltma işleçleri olabilir ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve önek arttırma ve azaltma işleçleri ([ön eki artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)).

Birden çok alan bildiren bir alan bildirimi tek alanların özniteliklerini, değiştiriciler ve türe sahip birden fazla bildirimi eşdeğerdir. Örneğin
```csharp
class A
{
    public static int X = 1, Y, Z = 100;
}
```
eşdeğerdir
```csharp
class A
{
    public static int X = 1;
    public static int Y;
    public static int Z = 100;
}
```

### <a name="static-and-instance-fields"></a>Statik ve örnek alanları

Ne zaman bir alan bildirimi içeren bir `static` değiştiricisi bildirim tarafından tanıtılan alanlar ***statik alanlar***. Hiçbir `static` değiştirici mevcut, alanlar bildirim tarafından tanıtılan ***örnek alanları***. Statik ve örnek alanları olan iki değişkenleri çeşitli türlerde ([değişkenleri](variables.md)) C# tarafından desteklenen ve bazen kullanıcılar olarak adlandırılır ***statik değişkenler*** ve ***örnek değişkenleri*** sırasıyla.

Statik bir alan belirli bir örneğine bir parçası değil; Bunun yerine, tüm örneklerini kapalı bir tür arasında paylaşılan ([açık ve kapalı türleri](types.md#open-and-closed-types)). Bir kapalı sınıf türünün kaç örneklerin oluşturulduğu ne olursa olsun, yalnızca bir kopyasını ilişkili uygulama etki alanı için statik bir alan yoktur.

Örneğin:
```csharp
class C<V>
{
    static int count = 0;

    public C() {
        count++;
    }

    public static int Count {
        get { return count; }
    }
}

class Application
{
    static void Main() {
        C<int> x1 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<double> x2 = new C<double>();
        Console.WriteLine(C<int>.Count);        // Prints 1

        C<int> x3 = new C<int>();
        Console.WriteLine(C<int>.Count);        // Prints 2
    }
}
```

Bir örnek alanıyla örneğine ait. Özellikle, bir sınıfın her örneği, bu sınıfın tüm örnek alanları ayrı bir kümesini içerir.

Ne zaman bir alan başvuru olarak bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` bir statik alanı `E` içeren türü belirtmek `M` ve eğer `M` bir örnek alanı E türünü içeren bir örneğini belirtmek gerekir `M`.

Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).

### <a name="readonly-fields"></a>Salt okunur alanları

Olduğunda bir *field_declaration* içeren bir `readonly` değiştiricisi bildirim tarafından tanıtılan alanlar ***salt okunur alanların***. Salt okunur alanlar doğrudan atamaları, yalnızca bu bildirimin ya da bir örnek oluşturucusu veya aynı sınıftaki statik Oluşturucusu bir parçası olarak gerçekleşebilir. (Salt okunur bir alan için birden çok kez bu bağlamda atanabilir.) Özellikle, doğrudan atamaları bir `readonly` alanı yalnızca şu bağlamlarda verilir:

*  İçinde *variable_declarator* alanı sunan (ekleyerek bir *variable_initializer* bildiriminde).
*  Bir örnek alan için sınıf örneği oluşturucularda alan bildirimi içerir; sınıfın statik oluşturucuda statik bir alan için alan bildirimini içerir. Olduğu geçirmek için geçerli tek bağlamı da bunlar bir `readonly` olarak alan bir `out` veya `ref` parametresi.

İçin atanmaya çalışılırken bir `readonly` olarak geçirmek veya alanı bir `out` veya `ref` başka bir bağlam parametresi olan bir derleme zamanı hatası.

#### <a name="using-static-readonly-fields-for-constants"></a>Sabitler için statik salt okunur alanları kullanma

A `static readonly` ne zaman bir sabit değer için bir simgesel ad isteniyorsa, ancak ne zaman değer türü olarak izin verilmez alan yararlı bir `const` bildirimi veya ne zaman kullanılamaz hesaplanan değer derleme zamanında. Örnekte
```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);

    private byte red, green, blue;

    public Color(byte r, byte g, byte b) {
        red = r;
        green = g;
        blue = b;
    }
}
```
`Black`, `White`, `Red`, `Green`, ve `Blue` üyeleri olarak bildirilemez `const` üyelerinin değerlerini derleme zamanında hesaplanamıyor çünkü. Ancak, bunları bildirme `static readonly` yerine çok aynı etkiye sahiptir.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Sabitler ve statik salt okunur alanların sürüm oluşturma

Sabitler ve salt okunur alanları farklı ikili sürüm semantiklere sahip. Bir sabit bir ifade başvuruda bulunduğunda, derleme zamanında sabit değeri elde edilir, ancak salt okunur bir alan bir ifade başvuruda bulunduğunda, alanın değerini kadar çalışma zamanı elde değil. İki ayrı programlardan oluşan bir uygulamayı düşünün:
```csharp
using System;

namespace Program1
{
    public class Utils
    {
        public static readonly int X = 1;
    }
}

namespace Program2
{
    class Test
    {
        static void Main() {
            Console.WriteLine(Program1.Utils.X);
        }
    }
}
```

`Program1` Ve `Program2` ad alanları belirtmek ayrı olarak derlenen iki program. Çünkü `Program1.Utils.X` değeri çıktı tarafından statik salt okunur bir alan olarak bildirilen `Console.WriteLine` deyimi, derleme zamanında bilinmiyor ancak bunun yerine, çalışma zamanında elde edilir. Bu nedenle, varsa değerini `X` değiştirilir ve `Program1` derlenir, `Console.WriteLine` deyimi, yeni değer olsa bile çıktı `Program2` yeniden derlenen değil. Ancak sahip `X` bir sabit değeri olan `X` zaman almış olmanız `Program2` derlenen ve değişikliklerinden etkilenmeyen kalır `Program1` kadar `Program2` derlenir.

### <a name="volatile-fields"></a>Volatile alanları

Olduğunda bir *field_declaration* içeren bir `volatile` değiştiricisi, bu bildirim tarafından tanıtılan alanlar ***geçici alanları***.

Geçici olmayan alanlar için yönergeler yeniden sıralama iyileştirme teknikleri tarafından sağlanan gibi eşitleme olmadan alanlarına erişmek çok iş parçacıklı programlarda beklenmeyen ve beklenmeyen sonuçlara neden olabilecek *lock_statement*  ([Lock deyiminin](statements.md#the-lock-statement)). Bu iyileştirmeler, derleyici tarafından çalışma zamanı sistemi tarafından veya donanım tarafından gerçekleştirilebilir. Volatile alanlar için sipariş gibi iyileştirmeler sınırlıdır:

*  Geçici bir alan okuma adlı bir ***geçici okuma***. Geçici okuma "semantiği Al"; var diğer bir deyişle, bundan sonra yönerge sırada gerçekleşen tüm başvurularını bellek önce gerçekleşmesi için sağlanır.
*  Geçici bir alan yazma adlı bir ***geçici yazma***. Geçici bir yazma "semantik sürüm"; var diğer bir deyişle, tüm bellek başvuruları yazma yönerge yönerge dizisindeki önce sonra gerçekleşecek garanti edilir.

Bu kısıtlamalar, tüm iş parçacıkları tarafından gerçekleştirilen sırada başka bir iş parçacığı tarafından gerçekleştirilen geçici yazma dikkate aldığınızdan emin olun. Uyumlu bir uygulama, bir tek toplam yürütme tüm iş parçacıklarından görülen olarak geçici yazma sıralamasını sağlamak için gerekli değildir. Geçici bir alan türü aşağıdakilerden biri olmalıdır:

*  A *reference_type*.
*  Türü `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`, veya` System.UIntPtr`.
*  Bir *enum_type* temel bir enum türü olan `byte`, `sbyte`, `short`, `ushort`, `int`, veya `uint`.

Örnek
```csharp
using System;
using System.Threading;

class Test
{
    public static int result;   
    public static volatile bool finished;

    static void Thread2() {
        result = 143;    
        finished = true; 
    }

    static void Main() {
        finished = false;

        // Run Thread2() in a new thread
        new Thread(new ThreadStart(Thread2)).Start();

        // Wait for Thread2 to signal that it has a result by setting
        // finished to true.
        for (;;) {
            if (finished) {
                Console.WriteLine("result = {0}", result);
                return;
            }
        }
    }
}
```
çıktıyı üretir:
```
result = 143
```

Bu örnekte, yöntem `Main` yöntemi çalıştıran yeni bir iş parçacığı başlattığını `Thread2`. Bu yöntem bir değer olarak adlandırılan bir geçici olmayan alana depolar `result`, ardından depolar `true` geçici bir alan olarak `finished`. Alan için ana iş parçacığı bekler `finished` ayarlanması `true`, alan olarak görülür `result`. Bu yana `finished` bildirilmiş `volatile`, ana iş parçacığı değeri okumalıdır `143` alanından `result`. Alanın `finished` değil bildirilmiş `volatile`, depoya için izin verilen sonra `result` deposuna sonra ana iş parçacığı için görünür olmasını `finished`ve bu nedenle değerini okumak ana iş parçacığı için `0` gelen alan `result`. Bildirme `finished` olarak bir `volatile` alan herhangi bir tutarsızlık engeller.

### <a name="field-initialization"></a>Alan başlatma

Bir alanın başlangıç değerini bir statik alanı veya bir örnek alanıyla oluşmasından varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) alanın türü. Bu varsayılan başlatma oluştu ve bir alan bu nedenle hiçbir zaman "başlatılmamış" önce bir alanın değerini gözlemlemek mümkün değildir. Örnek
```csharp
using System;

class Test
{
    static bool b;
    int i;

    static void Main() {
        Test t = new Test();
        Console.WriteLine("b = {0}, i = {1}", b, t.i);
    }
}
```
çıktıyı üretir
```
b = False, i = 0
```
çünkü `b` ve `i` hem de varsayılan değerleri otomatik olarak başlatılır.

### <a name="variable-initializers"></a>Değişken başlatıcıları

Alan bildirimler içerebilir *variable_initializer*s. Statik alanlar için değişken başlatıcılar sınıf başlatma sırasında yürütülen atama deyimleri karşılık gelir. Örneğin alanları, değişken başlatıcılar sınıfının bir örneği oluşturulduğunda, yürütülen atama deyimleri karşılık gelir.

Örnek
```csharp
using System;

class Test
{
    static double x = Math.Sqrt(2.0);
    int i = 100;
    string s = "Hello";

    static void Main() {
        Test a = new Test();
        Console.WriteLine("x = {0}, i = {1}, s = {2}", x, a.i, a.s);
    }
}
```
çıktıyı üretir
```
x = 1.4142135623731, i = 100, s = Hello
```
çünkü atama `x` gerçekleşir zaman statik alan başlatıcıları çalıştırmak ve atamalarını `i` ve `s` örnek alan başlatıcıları çalıştırmak oluşur.

Açıklanan varsayılan değer başlatma [alan başlatma](classes.md#field-initialization) değişken başlatıcıları olan alanların dahil olmak üzere tüm alanlar için gerçekleşir. Bu nedenle, bir sınıf başlatıldığında, bu sınıfın tüm statik alanları varsayılan değerlerine önce başlatılır ve statik alan başlatıcıları metinsel sırayla sonra yürütülür. Benzer şekilde, bir sınıfın bir örneği oluşturulduğunda, bu örnekteki tüm örnek alanları varsayılan değerlerine önce başlatılır ve ardından örnek alan başlatıcıları metinsel sırayla yürütülür.

Varsayılan değer durumlarını gözlenecek değişken başlatıcıları olan statik alanlar mümkündür. Ancak, bu stil sağlasa da kesinlikle önerilmez. Örnek
```csharp
using System;

class Test
{
    static int a = b + 1;
    static int b = a + 1;

    static void Main() {
        Console.WriteLine("a = {0}, b = {1}", a, b);
    }
}
```
Bu davranışı sergiler. Döngüsel tanımları rağmen bir ve b, program geçerlidir. Çıktı olur
```
a = 1, b = 2
```
çünkü statik alanlar `a` ve `b` başlatılır `0` (varsayılan değeri `int`) kendi başlatıcılar yürütülmeden önce. Zaman için Başlatıcı `a` çalıştığında, değerini `b` sıfır ve bu nedenle `a` değerine ayarlanır `1`. Zaman için Başlatıcı `b` çalıştığında, değerini `a` zaten `1`ve bu nedenle `b` değerine ayarlanır `2`.

#### <a name="static-field-initialization"></a>Statik alan başlatma

Bir sınıfın statik alan değişken başlatıcıları atamaları sınıfı bildiriminde göründükleri metinsel sırayla yürütülen bir dizi karşılık gelir. Bir statik Oluşturucu ([statik oluşturucular](classes.md#static-constructors)) var. sınıfında statik alan başlatıcıları yürütülmesi, statik Oluşturucusu yürütülmeden hemen önce gerçekleşir. Aksi takdirde, statik alan başlatıcıları, söz konusu sınıfın statik alanının ilk kez kullanıyorsanız önce bir uygulama bağımlıdır zamanında yürütülür. Örnek
```csharp
using System;

class Test 
{ 
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    public static int X = Test.F("Init A");
}

class B
{
    public static int Y = Test.F("Init B");
}
```
Çıkış ortaya çıkmasına neden olabilir:
```
Init A
Init B
1 1
```
veya çıktı:
```
Init B
Init A
1 1
```
çünkü yürütülmesini `X`'s başlatıcı ve `Y`'s Başlatıcı, herhangi bir sırada olabilir; bunlar yalnızca bu alanlara başvurular önce gerçekleşmesi için kısıtlanmıştır. Ancak, örnekte:
```csharp
using System;

class Test
{
    static void Main() {
        Console.WriteLine("{0} {1}", B.Y, A.X);
    }

    public static int F(string s) {
        Console.WriteLine(s);
        return 1;
    }
}

class A
{
    static A() {}

    public static int X = Test.F("Init A");
}

class B
{
    static B() {}

    public static int Y = Test.F("Init B");
}
```
Çıkış olmalıdır:
```
Init B
Init A
1 1
```
çünkü zaman statik oluşturucular yürütmek için kuralları (sınıfında tanımlandığı gibi [statik oluşturucular](classes.md#static-constructors)) sağlayan `B`'s statik Oluşturucusu (ve bu nedenle `B`'s statik alan başlatıcıları) önce çalıştırmanızgerekir`A`'s statik oluşturucusu ve alan başlatıcıları.

#### <a name="instance-field-initialization"></a>Örnek alanı başlatma

Bir sınıfın örnek alan değişken başlatıcıları bir dizi hemen sonra Anonime örnek oluşturucuları herhangi birine çalıştırılan atamaları karşılık gelir ([Oluşturucu başlatıcıları](classes.md#constructor-initializers)) söz konusu sınıfın. Değişken başlatıcılar, sınıf bildirimi içinde göründükleri metinsel sırayla yürütülür. Sınıf örneği oluşturma ve başlatma işlemi daha ayrıntılı açıklanır [örnek oluşturucular](classes.md#instance-constructors).

Değişken başlatıcı bir örnek alanıyla için oluşturulan örnek başvuramaz. Bu nedenle, başvuru için bir derleme zamanı hatası olduğu `this` bir değişken başlatıcısında olarak olan bir derleme zamanı hatası için herhangi bir örnek üyesi ile başvurmak değişken başlatıcı bir *simple_name*. Örnekte
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
değişken için Başlatıcı `y` oluşturulan örnek üyesi başvuruda bulunduğundan bir derleme zamanı hatası oluşur.

## <a name="methods"></a>Yöntemler

A ***yöntemi*** hesaplama veya bir nesne veya sınıf tarafından gerçekleştirilen eylem uygulayan bir üyesidir. Yöntemleri kullanarak bildirilir *method_declaration*: %s

```antlr
method_declaration
    : method_header method_body
    ;

method_header
    : attributes? method_modifier* 'partial'? return_type member_name type_parameter_list?
      '(' formal_parameter_list? ')' type_parameter_constraints_clause*
    ;

method_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | 'async'
    | method_modifier_unsafe
    ;

return_type
    : type
    | 'void'
    ;

member_name
    : identifier
    | interface_type '.' identifier
    ;

method_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

A *method_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve dört erişim değiştiricilere ait geçerli bir bileşim ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemler](classes.md#static-and-instance-methods)), `virtual` ([sanalyöntemleri](classes.md#virtual-methods)), `override` ([Geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([Dış yöntemleri](classes.md#external-methods)) değiştiriciler.

Aşağıdakilerin tümü doğru olduğunda bir bildirimi, değiştiricilere ait geçerli bir bileşim sahiptir:

*  Erişim değiştiricileri geçerli bir bileşimin bildirimi içerir ([erişim değiştiricilerine](classes.md#access-modifiers)).
*  Bildirimi, birden çok kez aynı değiştiricisi içermez.
*  Aşağıdaki değiştiriciler en fazla bir bildirim içerir: `static`, `virtual`, ve `override`.
*  Aşağıdaki değiştiriciler en fazla bir bildirim içerir: `new` ve `override`.
*  Bildirimi içeriyorsa `abstract` değiştiricisi, sonra bildirimi içermez herhangi birini aşağıdaki değiştiriciler: `static`, `virtual`, `sealed` veya `extern`.
*  Bildirimi içeriyorsa `private` değiştiricisi, sonra bildirimi içermez herhangi birini aşağıdaki değiştiriciler: `virtual`, `override`, veya `abstract`.
*  Bildirimi içeriyorsa `sealed` değiştiricisi, sonra bildirimi de içerir `override` değiştiricisi.
*  Bildirimi içeriyorsa `partial` değiştiricisi, ardından içermez herhangi birini aşağıdaki değiştiriciler: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override` , `abstract`, veya `extern`.

Bir yöntemin `async` değiştiricisi bir zaman uyumsuz işlev ve açıklanan kurallara [zaman uyumsuz işlevleri](classes.md#async-functions).

*Döndür_tür* bir metodun bildirimi hesaplanır ve yöntem tarafından döndürülen değerin türü belirtir. *Döndür_tür* olduğu `void` yöntemi bir değer döndürmezse. Bildirimi içeriyorsa `partial` değiştiricisi, sonra da dönüş türü olmalıdır `void`.

*Member_name* yöntem adını belirtir. Açık arabirim üyesi uygulaması yöntemi olmadığı sürece ([açık arabirim üyesi uygulamalarını](interfaces.md#explicit-interface-member-implementations)), *member_name* olan yalnızca bir *tanımlayıcı*. Bir açık arabirim üyesi uygulaması *member_name* oluşan bir *INTERFACE_TYPE* arkasından bir "`.`" ve *tanımlayıcı*.

İsteğe bağlı *type_parameter_list* yönteminin tür parametreleri belirtir ([tür parametrelerindeki](classes.md#type-parameters)). Varsa bir *type_parameter_list* yöntemi belirtilen bir ***genel yöntem***. Yöntemi varsa bir `extern` değiştiricisi, bir *type_parameter_list* belirtilemez.

İsteğe bağlı *formal_parameter_list* yönteminin parametreleri belirtir ([yöntem parametreleri](classes.md#method-parameters)).

İsteğe bağlı *type_parameter_constraints_clause*s belirtin tek tek tür parametrelerindeki kısıtlamalar ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve varsa belirtilebilir bir *type_parameter_ Liste* da sağlanır ve yöntemi olmayan bir `override` değiştiricisi.

*Döndür_tür* ve her başvurulan tür *formal_parameter_list* bir yöntemi en az yöntemi olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

*Method_body* ya da bir noktalı virgüldür bir ***deyim gövdesi*** veya ***ifade gövdesi***. Deyim gövdesi oluşan bir *blok*, yöntem çağrıldığında çalıştırılacak deyimleri belirtir. İfade gövdesi oluşan `=>` arkasından bir *ifade* ve noktalı virgül ve tek bir ifade yöntemi çağrıldığında gerçekleştirilecek gösterir. 

İçin `abstract` ve `extern` yöntemleri *method_body* yalnızca noktalı virgül içerir. İçin `partial` yöntemleri *method_body* ya da noktalı virgül, bir blok gövdesi veya bir ifade gövdesi oluşabilir. Diğer tüm yöntemler için *method_body* blok gövdesi ya da bir deyim gövdesi.

Varsa *method_body* bildirimi içermiyor sonra bir virgül, içerir `async` değiştiricisi.

Ad, tür parametresi listesi ve biçimsel parametre listesi bir yöntemin imzası tanımlayın ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) yöntem. Özellikle, yöntemin imzası, adı, tür parametreleri ve sayısı, değiştiriciler ve biçimsel parametrelerinin türleri sayısı oluşur. Bu amaçlar için oluşan bir biçimsel parametre türü yöntemin herhangi bir tür parametre adına göre değil, ancak türü bağımsız değişken listesindeki sıralı konumuna yöntem tarafından tanımlanır. Dönüş türü, yönetim imzasının bir parçası değil veya tür parametreleri veya biçimsel parametre adları.

Bir yöntemin adını, diğer tüm olmayan-aynı sınıfta bildirilen yöntemlerin adlarından farklı olmalıdır. Ayrıca, yöntemin imzası aynı sınıfta bildirilen tüm diğer yöntemler imzalarını farklı gerekir ve aynı sınıfta bildirilen iki yöntem sadece onun tarafından farklı imzaları olmayabilir `ref` ve `out`.

Yöntemin *type_parameter*lar kapsamdadır *method_declaration*ve bu kapsamda boyunca form türleri için kullanılabilir *Döndür_tür*, *method_body*, ve *type_parameter_constraints_clause*s fakat *öznitelikleri*.

Tüm biçimsel parametreler ve tür parametreleri farklı adlara sahip olmalıdır.

### <a name="method-parameters"></a>Yöntem parametreleri

Bir yöntemin parametre varsa, yöntemin tarafından bildirilir *formal_parameter_list*.

```antlr
formal_parameter_list
    : fixed_parameters
    | fixed_parameters ',' parameter_array
    | parameter_array
    ;

fixed_parameters
    : fixed_parameter (',' fixed_parameter)*
    ;

fixed_parameter
    : attributes? parameter_modifier? type identifier default_argument?
    ;

default_argument
    : '=' expression
    ;

parameter_modifier
    : 'ref'
    | 'out'
    | 'this'
    ;

parameter_array
    : attributes? 'params' array_type identifier
    ;
```

Biçimsel parametre listesi hangi yalnızca son olabilir bir veya daha fazla virgülle ayrılmış parametrelerinin oluşan bir *parameter_array*.

A *fixed_parameter* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)), isteğe bağlı `ref`, `out` veya `this` değiştiricisi, bir *türü*e *tanımlayıcı* ve isteğe bağlı *default_argument*. Her *fixed_parameter* verilen ada sahip belirtilen türde bir parametre bildirir. `this` Değiştirici yöntem bir genişletme yöntemi belirler ve yalnızca bir statik yöntem ilk parametrede yer izin verilir. Genişletme yöntemleri bölümünde daha ayrıntılı açıklanmıştır [genişletme yöntemleri](classes.md#extension-methods).

A *fixed_parameter* ile bir *default_argument* olarak bilinen bir ***isteğe bağlı parametre***bilgileriyse bir *fixed_parameter* bir olmadan*default_argument* olduğu bir ***gerekli parametre***. Gerekli bir parametre, isteğe bağlı bir parametre sonra görünmeyebilir bir *formal_parameter_list*.

A `ref` veya `out` parametresi olamaz bir *default_argument*. *İfade* içinde bir *default_argument* aşağıdakilerden biri olmalıdır:

*  bir *constant_expression*
*  bir ifade formun `new S()` burada `S` bir değer türü
*  bir ifade formun `default(S)` burada `S` bir değer türü

*İfade* bir kimlik veya parametresinin türü için boş değer atanabilir dönüştürme tarafından örtük olarak dönüştürülebilir olmalıdır.

İsteğe bağlı parametreler uygulayan bir kısmi yöntem bildiriminde gerçekleşmesi durumunda ([kısmi yöntemler](classes.md#partial-methods)), üye açık arabirim uygulaması ([açık arabirim üyesi uygulamalarını](interfaces.md#explicit-interface-member-implementations)) veya bir tek parametre dizin oluşturucu bildirimi ([dizin oluşturucular](classes.md#indexers)) bu üyeleri hiçbir zaman atlanacak bağımsız değişkenler veren bir şekilde çağrılabilir olduğundan derleyici bir uyarı vermesi gerekir.

A *parameter_array* isteğe bağlı bir kümesinden oluşur *öznitelikleri* ([öznitelikleri](attributes.md)), `params` değiştiricisi, bir *array_type*, ve bir *tanımlayıcı*. Bir parametre dizisi, belirtilen ada sahip belirtilen dizi türünde tek bir parametre bildirir. *Array_type* parametre dizisi tek boyutlu dizi türü olmalıdır ([dizisi, türlerini](arrays.md#array-types)). Bir yöntem çağırmayla ya da tek bir bağımsız değişken belirtilmesi için belirtilen dizi türünde bir parametre dizisi izin verir veya dizi öğesi türünün belirtilmesi için sıfır veya daha fazla bağımsız değişken verir. Parametre dizileri daha ayrıntılı açıklanır [parametre dizileri](classes.md#parameter-arrays).

A *parameter_array* sonra isteğe bağlı bir parametre oluşabilir, ancak--atlama bağımsız değişken için bir varsayılan değere sahip olamaz bir *parameter_array* boş bir dizi oluşturma bunun yerine neden olur.

Aşağıdaki örnekte, farklı türde parametreler gösterilmektedir:
```csharp
public void M(
    ref int      i,
    decimal      d,
    bool         b = false,
    bool?        n = false,
    string       s = "Hello",
    object       o = null,
    T            t = default(T),
    params int[] a
) { }
```

İçinde *formal_parameter_list* için `M`, `i` gerekli ref parametresi `d` gerekli değer parametresi `b`, `s`, `o` ve `t` İsteğe bağlı değeri parametreleri ve `a` bir parametre dizisi.

Yöntem bildiriminde parametre, tür parametreleri ve yerel değişkenler için ayrı bildirimi boşluk oluşturur. Tür parametresi listesi ve yöntem biçimsel parametre listesi ve yerel değişken bildirimlerinde adları bu bildirim alanına tanıtılır *blok* yöntemi. Aynı ada sahip iki üyesi bir yöntem bildirim alanı için bir hatadır. Yöntemi bildirimini alan ve yerel değişken bildiriminde alan aynı ada sahip öğeleri içeren bir iç içe geçmiş bir bildirim alanı için bir hatadır.

Yöntem çağırma ([yöntem çağrıları](expressions.md#method-invocations)) bu çağrıya belirli bir kopyasını oluşturur, biçimsel parametreler ve yöntemin yerel değişkenleri ve çağırma bağımsız değişken listesi değerlerini veya değişken başvuruları atar. biçimsel parametre yeni oluşturulmuş. İçinde *blok* yöntemi, biçimsel parametre kendi tanımlayıcıları tarafından başvurulabilir *simple_name* ifadeleri ([basit adları](expressions.md#simple-names)).

Biçimsel parametre dört çeşit vardır:

*  Tüm değiştiricilere bildirilen değer parametreleri.
*  Başvuru ile bildirilen parametreleri `ref` değiştiricisi.
*  İle bildirilen çıkış parametreleri `out` değiştiricisi.
*  İle bildirilen parametre dizileri `params` değiştiricisi.

Bölümünde anlatıldığı gibi [imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading), `ref` ve `out` değiştiriciler yöntemin imzası bir parçasıdır ancak `params` değiştiricisi değil.

#### <a name="value-parameters"></a>Değer parametreleri

Hiçbir değiştiricileri ile bildirilen bir parametre değeri bir parametredir. Bir değer parametresini karşılık gelen bir yerel değişkene yöntemi çağrısını sağlanan karşılık gelen bağımsız değişken bunun başlangıçtaki değerini alır.

Biçimsel parametre bir değer parametresini olduğunda, karşılık gelen bağımsız değişken bir yöntem çağrısı örtük olarak dönüştürülebilir bir ifade olmalıdır ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) biçimsel parametre türü.

Bir yöntem için bir değer parametresini yeni değerler atamanıza izin verilir. Bu tür atamalar yalnızca değer parametresi tarafından temsil edilen yerel depolama konumunu etkileyen — bunlar yöntem çağırma verilen gerçek bağımsız değişken üzerinde hiçbir etkisi yoktur.

#### <a name="reference-parameters"></a>Başvuru parametreleri

Bir parametre ile bildirilen bir `ref` bir başvuru parametresi bir değiştiricidir. Bir değer parametresini, yeni bir depolama konumuna bir başvuru parametresi oluşturmaz. Bunun yerine, bir başvuru parametresi aynı depolama konumu olarak yöntem çağırma bağımsız değişken olarak verilen değişkeni temsil eder.

Biçimsel parametre bir başvuru parametresi olduğunda, karşılık gelen bağımsız değişken bir yöntem çağrısı anahtar sözcüğünü oluşmalıdır `ref` arkasından bir *variable_reference* ([kesin kurallar belirlemek için belirli atama onayına](variables.md#precise-rules-for-determining-definite-assignment)) biçimsel parametresi ile aynı türde. Bir başvuru parametresi geçirilebilir önce bir değişkeni kesinlikle atanmalıdır.

Bir yöntem içinde bir başvuru parametresi her zaman kesin atanmış olarak kabul edilir.

Bir yöntem bir yineleyici bildirilen ([yineleyiciler](classes.md#iterators)) başvuru parametrelere sahip olamaz.

Örnek
```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("i = {0}, j = {1}", i, j);
    }
}
```
çıktıyı üretir
```
i = 2, j = 1
```

Çalıştırılışı için `Swap` içinde `Main`, `x` temsil `i` ve `y` temsil `j`. Bu nedenle, çağırma değerlerini değiştirme etkisi vardır `i` ve `j`.

Bir yöntemde, aynı depolama konumunu göstermek birden çok ad mümkündür başvuru parametreleri alır. Örnekte
```csharp
class A
{
    string s;

    void F(ref string a, ref string b) {
        s = "One";
        a = "Two";
        b = "Three";
    }

    void G() {
        F(ref s, ref s);
    }
}
```
davranışınızın `F` içinde `G` bir başvuru geçirir `s` hem `a` ve `b`. Bu nedenle, bu çağrı, adları için `s`, `a`, ve `b` tümü aynı depolama konumuna bakın ve örnek alanı tüm üç atamaları değiştirmek `s`.

#### <a name="output-parameters"></a>Çıktı parametreleri

Bir parametre ile bildirilen bir `out` değiştiricidir çıkış parametresi. Bir başvuru parametresi için benzer bir çıktı parametresi yeni bir depolama konumuna oluşturmaz. Bunun yerine, bir çıkış parametresi aynı depolama konumu olarak yöntem çağırma bağımsız değişken olarak verilen değişkeni temsil eder.

Bir biçimsel parametresi bir output parametresi olduğunda, karşılık gelen bağımsız değişken bir yöntem çağrısı anahtar sözcüğünü oluşmalıdır `out` arkasından bir *variable_reference* ([kesin kurallar belirlemek için belirli atama onayına](variables.md#precise-rules-for-determining-definite-assignment)) biçimsel parametresi ile aynı türde. Bir değişken kesinlikle bir output parametresi olarak geçirildi, ancak burada bir değişken bir output parametresi olarak geçirildi bir çağrısını değişkeni kesinlikle atanan kabul önce atanmamış.

Bir yöntem içinde olduğu gibi yerel bir değişken, bir çıkış parametresi ilk olarak kabul edilir atanmamış ve değeri kullanılmadan önce kesinlikle atanmalıdır.

Yöntem döndürmeden önce her bir yöntemin çıkış parametresi kesinlikle atanmalıdır.

Bir yöntem kısmi bir yöntem bildirilen ([kısmi yöntemler](classes.md#partial-methods)) ya da bir yineleyici ([yineleyiciler](classes.md#iterators)) çıktı parametreleri olamaz.

Çıkış parametreleri, genellikle birden çok değer oluşturan yöntemleri kullanılır. Örneğin:
```csharp
using System;

class Test
{
    static void SplitPath(string path, out string dir, out string name) {
        int i = path.Length;
        while (i > 0) {
            char ch = path[i - 1];
            if (ch == '\\' || ch == '/' || ch == ':') break;
            i--;
        }
        dir = path.Substring(0, i);
        name = path.Substring(i);
    }

    static void Main() {
        string dir, name;
        SplitPath("c:\\Windows\\System\\hello.txt", out dir, out name);
        Console.WriteLine(dir);
        Console.WriteLine(name);
    }
}
```

Örnek çıktıyı üretir:
```
c:\Windows\System\
hello.txt
```

Unutmayın `dir` ve `name` için geçirilmeden önce değişkenleri atanmamış olabilir `SplitPath`, ve bunlar çağrıyı izleyen kesinlikle atanmış olarak kabul edilir.

#### <a name="parameter-arrays"></a>Parametre dizileri

Bir parametre ile bildirilen bir `params` değiştiricidir bir parametre dizisi. Bir parametre dizisi biçimsel parametre listesi içeriyorsa, listesindeki son parametre olmalıdır ve bir tek boyutlu dizi türünde olmalıdır. Örneğin, türleri `string[]` ve `string[][]` parametre dizi türü, ancak türü kullanılabilir `string[,]` gerçekleştiremezsiniz. Birleştirmek mümkün değildir `params` değiştiriciler değiştiriciyle `ref` ve `out`.

Bir parametre dizisi bağımsız değişken bir yöntem çağrısı iki yoldan biriyle belirtilmesi izin verir:

*  Bir parametre dizisi örtük olarak dönüştürülebilir tek bir ifade olabilir için verilen bağımsız değişken ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) parametre dizi türü. Bu durumda, parametre dizisi, tam olarak bir değer parametresini gibi davranır.
*  Alternatif olarak, çağırma örtük olarak dönüştürülebilir bir ifade bulunduğu her bağımsız değişken, bir parametre dizisi için sıfır veya daha fazla bağımsız değişken belirtebilirsiniz ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) parametre dizinin öğe türü. Bu durumda, çağırma bağımsız değişken sayısı için karşılık gelen uzunluğu parametre dizi türü örneği oluşturur, belirtilen bağımsız değişken değerleri dizi örneği başlatır ve yeni oluşturulan bir dizi örneği fiili kullanır bağımsız değişken.

Değişken sayıda bağımsız değişken bir çağrıda izin vererek dışında bir parametre dizisi için bir değer parametresini tam olarak eşdeğerdir ([değer parametreleri](classes.md#value-parameters)) aynı türde.

Örnek
```csharp
using System;

class Test
{
    static void F(params int[] args) {
        Console.Write("Array contains {0} elements:", args.Length);
        foreach (int i in args) 
            Console.Write(" {0}", i);
        Console.WriteLine();
    }

    static void Main() {
        int[] arr = {1, 2, 3};
        F(arr);
        F(10, 20, 30, 40);
        F();
    }
}
```
çıktıyı üretir
```
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

İlk çağrısıysa `F` yalnızca dizi geçirir `a` değer parametre olarak. İkinci çağırmayı `F` dört öğeli otomatik olarak oluşturur `int[]` verilen öğe değerlerini ve dizi örneği bir değer parametresi olarak geçirir. Benzer şekilde, üçüncü çağırmayı `F` sıfır öğeli oluşturur `int[]` ve bu örneğe bir değer parametre olarak geçirir. İkinci ve üçüncü çağrıları, yazma tam olarak eşdeğerdir:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Aşırı yükleme çözümlemesi yaparken, bir parametre dizisi olan bir yöntem normal biçimde veya genişletilmiş biçimde uygun olabilir ([geçerli işlev üyesi](expressions.md#applicable-function-member)). Bir yöntemin genişletilmiş form, yalnızca normal yöntem biçiminin geçerli değilse ve yalnızca genişletilmiş formun aynı imzaya sahip geçerli bir yöntem zaten aynı türden bildirilmedi varsa kullanılabilir.

Örnek
```csharp
using System;

class Test
{
    static void F(params object[] a) {
        Console.WriteLine("F(object[])");
    }

    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object a0, object a1) {
        Console.WriteLine("F(object,object)");
    }

    static void Main() {
        F();
        F(1);
        F(1, 2);
        F(1, 2, 3);
        F(1, 2, 3, 4);
    }
}
```
çıktıyı üretir
```
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

Örnekte, iki parametre dizisi yöntemiyle olası genişletilmiş biçimlerinin zaten sınıfında normal yöntemleri olarak dahil edilir. Bu Genişletilmiş formlar bu nedenle aşırı yükleme çözümlemesi yaparken dikkate alınmaz ve birinci ve üçüncü yöntem çağrıları, böylece normal yöntemleri seçin. Bir yöntem parametre dizisi olan bir sınıfı bildirir, bazı normal yöntemler olarak genişletilmiş forms de sık karşılaşılan bir durum değil. Bir dizinin ayırma önlemek için mümkün olacak şekilde yaparak bir yöntemin parametre dizisi olan Genişletilmiş bir form oluşur örneği çağrılır.

Bir parametre dizi türü olduğunda `object[]`, tüketim form için tek bir yöntem normal biçiminin arasındaki olası bir belirsizliğe ortaya `object` parametresi. Belirsizlik nedeni bir `object[]` kendisi türüne örtük olarak dönüştürülebilir değildir `object`. Gerekirse bir yayın ekleyerek çözülebilir beri belirsizlik hiç sorun değil, ancak sunar.

Örnek
```csharp
using System;

class Test
{
    static void F(params object[] args) {
        foreach (object o in args) {
            Console.Write(o.GetType().FullName);
            Console.Write(" ");
        }
        Console.WriteLine();
    }

    static void Main() {
        object[] a = {1, "Hello", 123.456};
        object o = a;
        F(a);
        F((object)a);
        F(o);
        F((object[])o);
    }
}
```
çıktıyı üretir
```
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

İlk ve son çağrılarını içinde `F`, normal şeklinde `F` örtük bir dönüştürme için parametre türü bağımsız değişken türünden varolduğundan geçerlidir (her ikisi de türlerinin `object[]`). Bu nedenle, aşırı yükleme çözünürlüğü normal şeklinde seçer `F`, ve normal değer parametre olarak geçirilen bağımsız değişken. İkinci ve üçüncü çağrılarını normal şeklinde içinde `F` örtük dönüştürme için parametre türü bağımsız değişken türü olduğundan geçerli değildir (tür `object` türüne örtük olarak dönüştürülemez `object[]`). Ancak, genişletilmiş biçiminin `F` aşırı yükleme çözünürlüğü tarafından Seçili olmaması için geçerlidir. Sonuç olarak, bir öğe `object[]` çağrı tarafından oluşturulur ve belirtilen bağımsız değişken değeri ile başlatılmış dizinin tek öğesi (kendisi bir başvurudur bir `object[]`).

### <a name="static-and-instance-methods"></a>Statik ve örnek yöntemleri

Ne zaman bir yöntem bildiriminde içeren bir `static` değiştiricisi, yöntemi statik yöntem olarak değerlendirilir. Hiçbir `static` değiştirici mevcut olduğundan, yöntemi bir örnek yöntemi olarak kabul edilir.

Statik bir yöntemi de belirli bir örneği üzerinde çalışmaz ve başvurmak için bir derleme zamanı hata `this` statik yöntemde.

Bir örnek yöntemi, bir sınıfın belirli bir örneği üzerinde çalışır ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)).

Ne zaman bir yöntem başvuruluyor bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` statik bir yöntem `E` içerenbirtürbelirtmekgerekir`M`ve eğer `M` bir örnek yöntemi olduğundan `E` türünü içeren bir örneğini belirtmek gerekir `M`.

Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).

### <a name="virtual-methods"></a>Sanal yöntemler

Ne zaman bir örnek yöntemi bildirimi içeren bir `virtual` değiştiricisi, yöntemi sanal bir yöntem olarak değerlendirilir. Hiçbir `virtual` değiştirici mevcut olduğundan, yöntemi sanal olmayan bir yöntem olarak kabul edilir.

Sanal olmayan bir yöntemin uygulanmasını sabit: yöntemi sınıfının bir örneği üzerinde çağrıldıktan uygulaması aynı olup, bildirilen veya türetilmiş bir sınıfın bir örneği. Buna karşılık, bir sanal yöntemin uygulanmasını türetilmiş sınıflar tarafından yenisiyle değiştirilmiş. Devralınan bir sanal yöntemin uygulanmasını yerini alma işlemi olarak bilinir ***geçersiz kılma*** bu yöntem ([geçersiz kılma yöntemleri](classes.md#override-methods)).

Bir sanal yöntem çağrısı ***çalışma zamanı tür*** bu çağırma aldığı örneğini yerde çağrılacak gerçek yöntem uygulaması belirler. Bir sanal olmayan bir yöntem çağrısı ***derleme zamanı tür*** belirleyici faktör örneğidir. Kesin terimleri, zaman adlı yöntemi `N` bir bağımsız değişken listesi ile çağrılan `A` ile bir derleme zamanı tür örneği üzerinde `C` ve çalışma zamanı tür `R` (burada `R` ya da `C` veya türetilmiş bir sınıf gelen `C`), çağırma şu şekilde işlenir:

*  İlk olarak, aşırı yükleme çözünürlüğü uygulanan `C`, `N`, ve `A`, belirli bir yöntemi seçmek için `M` bildirilen ve tarafından devralınan yöntemleri kümesinden `C`. Bu açıklanan [yöntem çağrıları](expressions.md#method-invocations).
*  Ardından, eğer `M` sanal olmayan bir yöntem `M` çağrılır.
*  Aksi takdirde, `M` sanal bir yöntem ve en çok türetilen uygulamasını `M` ilişkilendirilebilmesi için `R` çağrılır.

Bildirilen veya devralınan bir sınıf tarafından her sanal yöntem için mevcut bir ***uygulama'en çok türetilene*** o sınıfa göre yöntemi. Sanal bir yöntemin en türetilen uygulamaya `M` bir sınıfa göre `R` şu şekilde belirlenir:

*  Varsa `R` Tanıtımı içeren `virtual` bildirimi `M`, sonra da bu en çok türetilen uygulamasıdır `M`.
*  Aksi takdirde `R` içeren bir `override` , `M`, sonra da bu en çok türetilen uygulamasıdır `M`.
*  Aksi takdirde, en iyi uygulaması türetilen `M` ilişkilendirilebilmesi için `R` en çok türetilen uygulamasını aynı `M` doğrudan temel sınıfını göre `R`.

Aşağıdaki örnekte, sanal ve sanal olmayan yöntemleri arasındaki farklar gösterilmektedir:
```csharp
using System;

class A
{
    public void F() { Console.WriteLine("A.F"); }

    public virtual void G() { Console.WriteLine("A.G"); }
}

class B: A
{
    new public void F() { Console.WriteLine("B.F"); }

    public override void G() { Console.WriteLine("B.G"); }
}

class Test
{
    static void Main() {
        B b = new B();
        A a = b;
        a.F();
        b.F();
        a.G();
        b.G();
    }
}
```

Örnekte, `A` sanal olmayan bir yöntem sunar `F` ve sanal bir yöntem `G`. Sınıf `B` sanal olmayan bir yöntem sunar `F`, bu nedenle Devralınanı gizleme `F`ve ayrıca devralınan yöntemi geçersiz kılar `G`. Örnek çıktıyı üretir:
```
A.F
B.F
B.G
B.G
```

Dikkat deyim `a.G()` çağırır `B.G`değil `A.G`. Çalışma zamanı örneğini yazın olmasıdır (olduğu `B`), derleme zamanı türü örneğinin değil (olduğu `A`), çağrılacak gerçek yöntem uygulaması belirler.

Devralınan yöntemleri gizlemek için yöntemleri izin verildiğinden, aynı imzaya sahip birden çok sanal yöntemler içeren sınıf için mümkündür. En çok türetilen metot dışındaki tüm gizlenmiş olduğundan bu bir belirsizlik sorunu sunmaz. Örnekte
```csharp
using System;

class A
{
    public virtual void F() { Console.WriteLine("A.F"); }
}

class B: A
{
    public override void F() { Console.WriteLine("B.F"); }
}

class C: B
{
    new public virtual void F() { Console.WriteLine("C.F"); }
}

class D: C
{
    public override void F() { Console.WriteLine("D.F"); }
}

class Test
{
    static void Main() {
        D d = new D();
        A a = d;
        B b = d;
        C c = d;
        a.F();
        b.F();
        c.F();
        d.F();
    }
}
```
`C` ve `D` sınıflar aynı imzaya sahip iki sanal yöntem bulunur: tarafından sunulan bir `A` tarafından sunulan bir `C`. Yöntemi tarafından tanıtılan `C` devralınan yöntemini gizliyor `A`. Bu nedenle, geçersiz kılma bildiriminde `D` tarafından tanıtılan yöntemini geçersiz kılan `C`, ve mümkün değil `D` tarafından tanıtılan yöntemi geçersiz kılmak için `A`. Örnek çıktıyı üretir:
```
B.F
B.F
D.F
D.F
```

Örneği erişerek gizli sanal yöntemini çağırmak mümkün olmadığını unutmayın `D` daha az türetilmiş tür, yöntem gizlenmediğinden.

### <a name="override-methods"></a>Geçersiz kılma yöntemleri

Ne zaman bir örnek yöntemi bildirimi içeren bir `override` değiştiricisi, yöntem olduğu söylenir bir ***yöntemi yok sayın***. Bir geçersiz kılma yöntemi, aynı imzaya sahip devralınan sanal yöntemi geçersiz kılar. Bir geçersiz kılma yöntemi bildirimini sanal yöntem bildiriminde bir yöntem sunar ancak bu yöntem yeni bir uygulamasını sağlayarak mevcut devralınan sanal yöntemi uzmanlaşmış.

Yöntem tarafından geçersiz kılınan bir `override` bildirimi olarak bilinen ***temel yöntemi geçersiz kılmışsa***. Bir geçersiz kılma yöntemi için `M` bir sınıfta bildirilen `C`, geçersiz kılınan taban yöntemi her temel sınıf türünü incelenerek belirlenir `C`doğrudan temel sınıf türü ile başlayan `C` ve her birinin art arda gelen bir verilen temel sınıf türünde en az bir erişilebilir yöntemi, bulunan kadar doğrudan temel sınıf türünde aynı imzaya `M` tür bağımsız değişkenlerini değiştirme sonra. Geçersiz kılınan taban yöntemini bulma amacıyla, bir yöntem ise erişilebilir kabul `public`, bu ise `protected`, bu ise `protected internal`, ya da ise `internal` ve aynı programın içinde bildirilen `C`.

Aşağıdakilerin tümü için bir geçersiz kılma bildirimi doğru olduğu sürece, bir derleme zamanı hatası oluşur:

*  Yukarıda açıklanan şekilde geçersiz kılınan bir taban yöntemi bulunabilir.
*  Tam olarak bir tür geçersiz kılınan taban yöntemi yoktur. Yalnızca temel sınıf türünü nerede tür bağımsız değişkenleri değiştirme iki yöntem imzası aynı yapar oluşturulan tür ise, bu kısıtlama etkiye sahiptir.
*  Geçersiz kılınan taban yöntemi sanal, Özet ya da yöntemi yok sayın. Diğer bir deyişle, geçersiz kılınan taban yöntemi sanal olmayan ya da statik olamaz.
*  Geçersiz kılınan taban yöntemi, korumalı bir yöntem değildir.
*  Aşırı yükleme yöntemi ve geçersiz kılınan taban yöntemi aynı dönüş türüne sahip.
*  Aynı bildirilen erişilebilirliği ve geçersiz kılma bildirimi geçersiz kılınan taban yöntemi vardır. Diğer bir deyişle, bir geçersiz kılma bildirimi sanal yöntemin erişilebilirliği değiştiremezsiniz. Ancak, geçersiz kılınan taban yöntemi dahili korunuyorsa ve daha sonra geçersiz kılma yöntemin aşırı yükleme yöntemini içeren derlemenin bildirilen olandan farklı bir derlemede bildirilmiş erişilebilirlik korunmalıdır.
*  Geçersiz kılma bildirimi tür parametresi kısıtlamaları tümceleri belirtmiyor. Bunun yerine kısıtlamaları geçersiz kılınan taban yöntemden devralınır. Devralınan kısıtlamasında tür bağımsız değişkeni tarafından geçersiz kılınan yönteminin tür parametreleri olan kısıtlamaları değiştirilebilir unutmayın. Bu açıkça, değer türleri veya sealed türlerde gibi belirtildiğinde yasal olmayan kısıtlamaları neden olabilir.

Aşağıdaki örnek, geçersiz kılma kuralları için Genel sınıflar nasıl çalıştığını gösterir:
```csharp
abstract class C<T>
{
    public virtual T F() {...}
    public virtual C<T> G() {...}
    public virtual void H(C<T> x) {...}
}

class D: C<string>
{
    public override string F() {...}            // Ok
    public override C<string> G() {...}         // Ok
    public override void H(C<T> x) {...}        // Error, should be C<string>
}

class E<T,U>: C<U>
{
    public override U F() {...}                 // Ok
    public override C<U> G() {...}              // Ok
    public override void H(C<T> x) {...}        // Error, should be C<U>
}
```

Geçersiz kılınan taban yöntemini kullanarak bir geçersiz kılma bildirimi erişip bir *base_access* ([temel erişim](expressions.md#base-access)). Örnekte
```csharp
class A
{
    int x;

    public virtual void PrintFields() {
        Console.WriteLine("x = {0}", x);
    }
}

class B: A
{
    int y;

    public override void PrintFields() {
        base.PrintFields();
        Console.WriteLine("y = {0}", y);
    }
}
```
`base.PrintFields()` çağrısını `B` çağırır `PrintFields` yöntemi içinde bildirilen `A`. A *base_access* sanal çağırma mekanizması devre dışı bırakır ve yalnızca temel yöntemi sanal olmayan bir yöntem olarak değerlendirir. Çağırma gerekiyordu `B` edilmiş yazılan `((A)this).PrintFields()`, yinelemeli olarak olduğu çağırma `PrintFields` yöntemi içinde bildirilen `B`, bildirilen bir `A`, bu yana `PrintFields` sanal çalışmazamanıtürü`((A)this)` olduğu `B`.

Ekleyerek yalnızca bir `override` değiştiricisi can bir yöntem başka bir yöntemi geçersiz kılın. Diğer durumlarda, bir devralınan yönteme aynı imzaya sahip bir yöntemi yalnızca devralınan yöntemi gizler. Örnekte
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    public virtual void F() {}        // Warning, hiding inherited F()
}
```
`F` yönteminde `B` içermemesi bir `override` değiştiricisi ve bu nedenle geçersiz `F` yönteminde `A`. Bunun yerine, `F` yönteminde `B` yöntemi gizler `A`, ve bir uyarı bildirimi içermez çünkü bildirilen bir `new` değiştiricisi.

Örnekte
```csharp
class A
{
    public virtual void F() {}
}

class B: A
{
    new private void F() {}        // Hides A.F within body of B
}

class C: B
{
    public override void F() {}    // Ok, overrides A.F
}
```
`F` yönteminde `B` sanal gizler `F` yöntemi öğesinden devralınan `A`. Bu yana yeni `F` içinde `B` özel erişimi olduğunu, yalnızca sınıf gövdesinin kapsamının `B` ve kapsamaz `C`. Bu nedenle, bildirimi `F` içinde `C` geçersiz kılmak için izin verilen `F` devralınan `A`.

### <a name="sealed-methods"></a>Sealed yöntemleri

Ne zaman bir örnek yöntemi bildirimi içeren bir `sealed` değiştiricisi, yöntemi olarak kabul edilir bir ***yöntem korumalı***. Bir örnek yöntem bildiriminde içeriyorsa `sealed` değiştiricisi, aynı zamanda içermelidir `override` değiştiricisi. Kullanım `sealed` değiştiricisi türetilmiş bir sınıf başka bir yöntemi geçersiz kılmasını önler.

Örnekte
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }

    public virtual void G() {
        Console.WriteLine("A.G");
    }
}

class B: A
{
    sealed override public void F() {
        Console.WriteLine("B.F");
    } 

    override public void G() {
        Console.WriteLine("B.G");
    } 
}

class C: B
{
    override public void G() {
        Console.WriteLine("C.G");
    } 
}
```
sınıf `B` yöntemleri iki geçersiz kılma sağlar: bir `F` yöntemin `sealed` değiştiricisi ve `G` yok yöntemi. `B`kullanıcının kullanın korumalı `modifier` engeller `C` daha fazla geçersiz kılmasını `F`.

### <a name="abstract-methods"></a>Soyut metotlar

Ne zaman bir örnek yöntemi bildirimi içeren bir `abstract` değiştiricisi, yöntemi olarak kabul edilir bir ***yöntemi soyut***. Soyut Metoda örtük olarak da sanal bir yöntem olmasına karşın, değiştiricisine sahip olamaz `virtual`.

Bir soyut yöntem bildiriminde sanal bir yöntem sunar ancak bu yöntemin bir uygulaması, sağlamaz. Bunun yerine, soyut olmayan türetilen sınıfların bu yöntemi geçersiz kılarak, kendi uygulama sağlamak için gereklidir. Bir soyut yönteminden hiçbir kullanımınız sağladığından *method_body* soyut bir yöntemi yalnızca noktalı virgül içerir.

Soyut yöntem bildirimleri yalnızca soyut sınıfları izin ([soyut sınıflar](classes.md#abstract-classes)).

Örnekte
```csharp
public abstract class Shape
{
    public abstract void Paint(Graphics g, Rectangle r);
}

public class Ellipse: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawEllipse(r);
    }
}

public class Box: Shape
{
    public override void Paint(Graphics g, Rectangle r) {
        g.DrawRect(r);
    }
}
```
`Shape` sınıfı soyut kavramı kendi boyama yapabileceğiniz bir geometrik şekil nesnesi tanımlar. `Paint` Yöntemdir soyut olduğu için anlamlı varsayılan uygulaması. `Ellipse` Ve `Box` sınıflar somut `Shape` uygulamaları. Bu sınıflar, soyut olmayan olduğundan, bunlar geçersiz kılmak için gerekli `Paint` yöntemi ve bir gerçek uygulanmasını sağlar.

Bir derleme zamanı hata için bir *base_access* ([temel erişim](expressions.md#base-access)) bir soyut yönteminden başvurmak için. Örnekte
```csharp
abstract class A
{
    public abstract void F();
}

class B: A
{
    public override void F() {
        base.F();                        // Error, base.F is abstract
    }
}
```
bir derleme zamanı hatası için bildirilen `base.F()` çağırma soyut Metoda başvurduğundan.

Bir soyut yöntem bildiriminde sanal bir yöntemi geçersiz kılmak için izin verilir. Bu yöntemin türetilmiş sınıflarda yeniden uygulanmasını zorlamak bir Özet sınıf sağlar ve yöntemin asıl uygulamasına kullanılamaz hale getirir. Örnekte
```csharp
using System;

class A
{
    public virtual void F() {
        Console.WriteLine("A.F");
    }
}

abstract class B: A
{
    public abstract override void F();
}

class C: B
{
    public override void F() {
        Console.WriteLine("C.F");
    }
}
```
sınıf `A` sanal yöntem, sınıf `B` bir soyut metot ve sınıf bu metodu geçersiz kılar `C` kendi uygulamasını sağlamak için soyut yöntemini geçersiz kılar.

### <a name="external-methods"></a>Dış yöntemleri

Ne zaman bir yöntem bildiriminde içeren bir `extern` değiştiricisi, yöntemi olarak kabul edilir bir ***dış yöntem***. Dış yöntem, genellikle C# dışında bir dil kullanarak harici olarak uygulanır. Bir dış yöntem bildiriminde yok kullanımınız sağladığından *method_body* dış bir Metoda yalnızca noktalı virgül içerir. Dış yöntem genel olamaz.

`extern` Değiştiricisi ile birlikte kullanılan genellikle bir `DllImport` özniteliği ([bileşenlerle, COM ve Win32 birlikte çalışması](attributes.md#interoperation-with-com-and-win32-components)), dış yöntemleri (dinamik bağlantı kitaplıklarını) DLL tarafından uygulanmak üzere izin verme. Yürütme Ortamı ile dış yöntem uygulamaları sağlanabilir başka mekanizmalar destekleyebilir.

Ne zaman dış bir Metoda içeren bir `DllImport` özniteliği, yöntem bildiriminde içermelidir ayrıca bir `static` değiştiricisi. Bu örnek kullanımını gösterir `extern` değiştiricisi ve `DllImport` özniteliği:
```csharp
using System.Text;
using System.Security.Permissions;
using System.Runtime.InteropServices;

class Path
{
    [DllImport("kernel32", SetLastError=true)]
    static extern bool CreateDirectory(string name, SecurityAttribute sa);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool RemoveDirectory(string name);

    [DllImport("kernel32", SetLastError=true)]
    static extern int GetCurrentDirectory(int bufSize, StringBuilder buf);

    [DllImport("kernel32", SetLastError=true)]
    static extern bool SetCurrentDirectory(string name);
}
```

### <a name="partial-methods-recap"></a>Kısmi yöntemler (özeti)

Ne zaman bir yöntem bildiriminde içeren bir `partial` değiştiricisi, yöntemi olarak kabul edilir bir ***kısmi yöntem***. Kısmi yöntemler yalnızca kısmi türlerinden bir üyesi olarak bildirilebilir ([kısmi türlerinden](classes.md#partial-types)) ve kısıtlama sayısı tabidir. Kısmi yöntemler bölümünde daha ayrıntılı açıklanmıştır [kısmi yöntemler](classes.md#partial-methods).

### <a name="extension-methods"></a>Genişletme yöntemleri

Yönteminin ilk parametresinin zaman içerir `this` değiştiricisi, yöntemi olarak kabul edilir bir ***genişletme yöntemi***. Genişletme yöntemleri, genel olmayan, iç içe olmayan statik sınıflarda yalnızca bildirilebilir. Genişletme yönteminin ilk parametresi dışında hiçbir değiştiriciler olabilir `this`, ve parametre türü bir işaretçi türü olamaz.

Aşağıdaki iki uzantı yöntemleri bildiren bir statik sınıfının bir örneğidir:
```csharp
public static class Extensions
{
    public static int ToInt32(this string s) {
        return Int32.Parse(s);
    }

    public static T[] Slice<T>(this T[] source, int index, int count) {
        if (index < 0 || count < 0 || source.Length - index < count)
            throw new ArgumentException();
        T[] result = new T[count];
        Array.Copy(source, index, result, 0, count);
        return result;
    }
}
```

Bir genişletme yöntemi bir normal statik yöntemidir. Kapsayan statik sınıfıyla kapsamında olduğunda, ayrıca, bir genişletme yöntemi örnek yöntem çağırma söz dizimi kullanılarak çağrılabilir ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)), ilk bağımsız değişken olarak alıcı ifade kullanarak.

Aşağıdaki program, yukarıda belirtilen genişletme yöntemleri kullanır:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in strings.Slice(1, 2)) {
            Console.WriteLine(s.ToInt32());
        }
    }
}
```

`Slice` Yöntemi edinilebilir `string[]`ve `ToInt32` yöntemi edinilebilir `string`, genişletme yöntemleri bildirilmedi. Program anlamını aşağıdaki, kullanarak normal statik yöntem çağrılarını aynıdır:
```csharp
static class Program
{
    static void Main() {
        string[] strings = { "1", "22", "333", "4444" };
        foreach (string s in Extensions.Slice(strings, 1, 2)) {
            Console.WriteLine(Extensions.ToInt32(s));
        }
    }
}
```

### <a name="method-body"></a>Yöntem gövdesi

*Method_body* yöntemi bildirimi ya da bir blok gövdesi, bir deyim gövdesinin veya noktalı virgül oluşur.

***Sonuç türü*** bir yöntemidir `void` dönüş türü ise `void`, veya zaman uyumsuz yöntem ise ve dönüş türü `System.Threading.Tasks.Task`. Aksi takdirde, sonuç türü olmayan zaman uyumsuz yöntemin dönüş türü ve zaman uyumsuz bir yöntemin dönüş türü ile sonuç türü olan `System.Threading.Tasks.Task<T>` olduğu `T`.

Bir yöntem olduğunda bir `void` sonuç türü ve bir blok gövdesi `return` deyimleri ([return deyiminin](statements.md#the-return-statement)) bloğunda bir ifadeyi belirtmek için izin verilmez. Void bir yöntem bloğu yürütülmesini normalde tamamlandıktan, (yani, Denetim Akışları yöntem gövdesinin sonuna kapalı), yöntemi yalnızca geçerli arayanına döner.
    
Bir yöntem olduğunda bir `void` sonuç ve ifade bir deyim gövdesinin `E` olmalıdır bir *statement_expression*, ve gövde tam olarak bir form için blok gövdesi eşdeğerdir `{ E; }`.
    
Bir yöntem bir sonucu void olmayan türe sahip olduğunda ve bir blok gövdesi, her `return` deyim bloğu içindeki sonuç türüne örtük olarak dönüştürülebilir bir ifade belirtmeniz gerekir. Bir blok gövdesi değerini döndüren yöntemi uç noktası erişilebilir olmalıdır. Diğer bir deyişle, bir değer döndüren yöntemi ile bir blok gövdesi, denetim devre dışı yöntem gövdesinin sonuna akış izin verilmiyor.
    
Bir yöntemi void olmayan sonuç türü ve bir deyim gövdesi olan, ifade için sonuç türü örtük olarak dönüştürülebilir olmalıdır ve gövde tam olarak bir form için blok gövdesi eşdeğerdir `{ return E; }`.
    
Örnekte
```csharp
class A
{
    public int F() {}            // Error, return value required

    public int G() {
        return 1;
    }

    public int H(bool b) {
        if (b) {
            return 1;
        }
        else {
            return 0;
        }
    }

    public int I(bool b) => b ? 1 : 0;
}
```
değer döndürme `F` yöntem, yöntem gövdesinin sonuna devre dışı akış denetimi için bir derleme zamanı hatası sonuçlanır. `G` Ve `H` dönüş değeri belirten bir return deyimi içinde tüm olası yürütme yolları sonlandırmak için yöntemleri doğru. `I` Yöntemi olduğundan doğru gövdesinde bir deyim bloğunu yalnızca tek bir dönüş deyimi içindeki ile eşdeğerdir.

### <a name="method-overloading"></a>Yöntemi aşırı yüklemesi

Yöntemi aşırı yükleme çözünürlüğü kuralları açıklanan [anlam çıkarma](expressions.md#type-inference).

## <a name="properties"></a>Özellikler

A ***özelliği*** bir nesnenin ya da bir sınıf bir karakteristik erişim sağlayan bir üyesidir. Örnekler Özellikler penceresinde, bir müşterinin adını Başlığı yazı tipi boyutu bir dizenin uzunluğunu içeren ve benzeri. Özellikleri alanlarının doğal bir uzantısı olan — hem de üyeleri ile ilişkilendirilmiş türler olarak adlandırılır ve alanlar ve Özellikler erişmek için sözdizimi aynıdır. Ancak, alanlar özellikleri depolama konumları belirtmek değil. Bunun yerine, özelliklere sahip ***erişimcileri*** değerlerine okunabilir veya yazılabilir bırakıldığında yürütülecek deyimler belirtin. Özellikler, bu nedenle Eylemler okuma ve yazma nesnenin öznitelikleri ile ilişkilendirmek için bir mekanizma sağlar; Ayrıca, hesaplanan değer gibi özniteliklere izin.

Özellikleri kullanarak bildirilir *property_declaration*: %s

```antlr
property_declaration
    : attributes? property_modifier* type member_name property_body
    ;

property_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | property_modifier_unsafe
    ;

property_body
    : '{' accessor_declarations '}' property_initializer?
    | '=>' expression ';'
    ;

property_initializer
    : '=' variable_initializer ';'
    ;
```

A *property_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve dört erişim değiştiricilere ait geçerli bir bileşim ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemler](classes.md#static-and-instance-methods)), `virtual` ([sanalyöntemleri](classes.md#virtual-methods)), `override` ([Geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([Dış yöntemleri](classes.md#external-methods)) değiştiriciler.

Özellik bildirimleri aynı kurallara göre yöntem bildirimleri tabidir ([yöntemleri](classes.md#methods)) değiştiriciler geçerli birleşimlerini onaylamaz.

*Türü* özelliği bildirimi bildirim tarafından tanıtılan özelliğin türünü belirtir ve *member_name* özelliğin adını belirtir. Açık arabirim üyesi uygulaması, özelliği olmadığı sürece *member_name* olan yalnızca bir *tanımlayıcı*. Açık arabirim üyesi uygulaması için ([açık arabirim üyesi uygulamalarını](interfaces.md#explicit-interface-member-implementations)), *member_name* oluşan bir *INTERFACE_TYPE* arkasından bir " `.`"ve *tanımlayıcı*.

*Türü* bir özelliği en az özelliği olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

A *property_body* herhangi birini içerebilir bir ***erişimcisinin gövdesi*** veya ***ifade gövdesi***. Bir erişimci gövdesindeki *accessor_declarations*, hangi alınmalıdır içinde "`{`"ve"`}`" belirteçleri, erişimci bildirir ([erişimcileri](classes.md#accessors)) özelliği. Erişimciler okuma ve yazma özelliği ilişkili yürütülebilir deyimleri belirtin.

Oluşan bir deyim gövdesinin `=>` arkasından bir *ifade* `E` ve noktalı virgül deyim gövdesi için tam olarak eşdeğerdir `{ get { return E; } }`ve bu nedenle yalnızca salt okuyucu belirtmek için kullanılabilir tek bir ifade tarafından sonucu alıcının burada verilen özellikleri.

A *property_initializer* otomatik olarak uygulanan bir özellik için yalnızca verilen ([Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties)) ve başlatma gibi temel alınan alanının neden olur özellikleri tarafından verilen değeri ile *ifade*.

Bir özelliğe erişmek için söz dizimi aynı alan için olsa da, bir değişken olarak bir özellik sınıflandırılmaz. Bu nedenle, bir özellik olarak geçirmek mümkün değil bir `ref` veya `out` bağımsız değişken.

Ne zaman bir özellik bildirimi içeren bir `extern` değiştiricisi, özellik olduğu söylenir bir ***dış özellik***. Hiçbir gerçek uygulama, her biri bir dış özellik bildiriminde sağladığından, *accessor_declarations* noktalı virgül içerir.

### <a name="static-and-instance-properties"></a>Statik ve örnek özellikleri

Ne zaman bir özellik bildirimi içeren bir `static` değiştiricisi, özellik olduğu söylenir bir ***statik özellik***. Hiçbir `static` değiştiricisi, özellik olduğu söylenir bir ***örnek özellik***.

Statik bir özellik, belirli bir örneği ile ilişkili değil ve başvurmak için bir derleme zamanı hata `this` Erişimcilerde statik özellik.

Bir örnek özelliği belirli bir sınıf örneği ile ilişkili olduğunu ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)), bu özelliğin erişimcileri.

Ne zaman bir özelliği başvuru olarak bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` statik bir özellik `E` içerenbirtürbelirtmekgerekir`M`ve eğer `M` bir örnek özelliği E türünü içeren bir örneğini belirtmek gerekir `M`.

Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).

### <a name="accessors"></a>Erişimciler

*Accessor_declarations* okuma ve bu özelliğe yazma ile ilişkili yürütülebilir deyimlerin bir özelliğini belirtin.

```antlr
accessor_declarations
    : get_accessor_declaration set_accessor_declaration?
    | set_accessor_declaration get_accessor_declaration?
    ;

get_accessor_declaration
    : attributes? accessor_modifier? 'get' accessor_body
    ;

set_accessor_declaration
    : attributes? accessor_modifier? 'set' accessor_body
    ;

accessor_modifier
    : 'protected'
    | 'internal'
    | 'private'
    | 'protected' 'internal'
    | 'internal' 'protected'
    ;

accessor_body
    : block
    | ';'
    ;
```

Erişimcisi bildirimlerine oluşan bir *get_accessor_declaration*, *set_accessor_declaration*, veya her ikisini de. Belirteci her erişimci bildirimi oluşur `get` veya `set` bir isteğe bağlı olarak izlenen *accessor_modifier* ve *accessor_body*.

Kullanımını *accessor_modifier*s tarafından aşağıdaki kısıtlamalara tabidir:

*  Bir *accessor_modifier* bir arabirim veya üye açık arabirim uygulaması kullanılamaz.
*  Bir özellik veya hiçbir dizin oluşturucu için `override` değiştiricisi, bir *accessor_modifier* yalnızca özellik veya dizin oluşturucusu hem de varsa izin verilir bir `get` ve `set` erişimci ve ardından yalnızca birinde bu izin verilir erişimcileri.
*  Bir özellik veya içeren dizin oluşturucu için bir `override` değiştiricisi, bir erişimci eşleşmelidir *accessor_modifier*varsa kılınmasını erişimcisi.
*  *Accessor_modifier* özellik veya dizin oluşturucu kendisini bildirilen erişilebilirliği kesinlikle daha kısıtlayıcı bir erişilebilirlik bildirmeniz gerekir. Kesin olarak için:
   * Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `public`, *accessor_modifier* olabilir `protected internal`, `internal`, `protected`, veya `private`.
   * Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `protected internal`, *accessor_modifier* olabilir `internal`, `protected`, veya `private`.
   * Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `internal` veya `protected`, *accessor_modifier* olmalıdır `private`.
   * Özellik veya dizin oluşturucu bir bildirilen erişilebilirliği varsa `private`, *accessor_modifier* kullanılabilir.

İçin `abstract` ve `extern` özellikleri *accessor_body* için belirtilen her bir erişimci noktalı yeterlidir. Her bir soyut olmayan, extern olmayan özelliğe sahip *accessor_body* noktalı olabilir, bu durumda olduğu bir ***özelliği'otomatik olarak uygulanan*** ([Özellikleri'otomatik olarak uygulanan ](classes.md#automatically-implemented-properties)). Otomatik olarak uygulanan bir özellik en az bir get erişimcisine sahip olmalıdır. Herhangi diğer soyut olmayan, extern olmayan özellik erişimcileri için *accessor_body* olduğu bir *blok* karşılık gelen erişimci çağrıldığında çalıştırılacak deyimleri belirtir.

A `get` bir parametresiz yöntemin dönüş değerini özellik türü ile erişimci karşılık gelir. Bir özellik bir ifadede başvurulduğunda, atama hedefi olarak dışında `get` özelliğin değerini hesaplamak için erişimci özelliğin çağırılır ([ifadelerin değerleri](expressions.md#values-of-expressions)). Gövdesi bir `get` erişimci değeri döndürmek için kurallara uygun olmalıdır açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body). Özellikle, tüm `return` deyimleri gövdesinde bir `get` erişimci özelliği türüne örtük olarak dönüştürülebilir bir ifade belirtmeniz gerekir. Ayrıca, uç noktası bir `get` erişimci erişilebilir olmalıdır.

A `set` erişimci karşılık gelen bir tek değer özellik türü parametresi olan bir yönteme ve `void` dönüş türü. Örtük parametresi bir `set` erişimci adlı her zaman `value`. Bir özellik atama hedefi başvurulan ne zaman ([atama işleçleri](expressions.md#assignment-operators)), veya işleneni olarak `++` veya `--` ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [ Önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), `set` erişimci bağımsız değişkenlerle çağrılır (değeri ataması sağ tarafında veya işleneni olan `++` veya `--` işleci), Yeni bir değer sağlar ([basit atama](expressions.md#simple-assignment)). Gövdesi bir `set` erişimcisi için kurallara uygun olmalıdır `void` açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body). Özellikle, `return` deyimlerinde `set` erişimcisinin gövdesi bir ifadeyi belirtmek izin verilmez. Bu yana bir `set` erişimci örtük olarak adlandırılan bir parametresi sahip `value`, bir derleme zamanı hata için bir yerel değişken veya Sabit bildiriminde bir `set` erişimci bu ada sahip.

Varlığı veya yokluğu tabanlı `get` ve `set` erişimcileri, bir özellik gibi Sınıflandırması:

*  Her ikisini de içeren bir özellik bir `get` erişimci ve `set` erişimci söyledi olacak şekilde bir ***okuma-yazma*** özelliği.
*  Yalnızca bir özellik bir `get` erişimci söyledi olacak şekilde bir ***salt okunur*** özelliği. Atama hedefi olmasını salt okunur bir özellik için bir derleme zamanı hatasıdır.
*  Yalnızca bir özellik bir `set` erişimci söyledi olacak şekilde bir ***salt yazılır*** özelliği. Dışında bir atama hedefi olarak, bir ifadede bir salt yazılır özellik başvurmak için bir derleme zamanı hatasıdır.

Örnekte
```csharp
public class Button: Control
{
    private string caption;

    public string Caption {
        get {
            return caption;
        }
        set {
            if (caption != value) {
                caption = value;
                Repaint();
            }
        }
    }

    public override void Paint(Graphics g, Rectangle r) {
        // Painting code goes here
    }
}
```
`Button` denetimini bildirir genel `Caption` özelliği. `get` Erişimcisine `Caption` özelliği, özel depolanan bir dize döndürür `caption` alan. `set` Erişimcisi varsa yeni değerin geçerli değerden farklı ve bu durumda, yeni bir değer depolar. denetler ve denetimi halinde yeniden boyar. Özellikleri, yukarıda gösterilen deseni genellikle izleyin: `get` erişimcisi yalnızca özel bir alanda depolanmış bir değer döndürür ve `set` erişimcisi bu özel alan değiştirir ve ardından tam durumu güncelleştirmek için gereken ek eylemleri gerçekleştirir nesne.

Verilen `Button` sınıfında above, aşağıdaki örneğidir kullanımını `Caption` özelliği:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Burada, `set` erişimci özelliği için bir değer atama tarafından çağrılır ve `get` erişimci özelliği bir ifadede başvurarak çağrılır.

`get` Ve `set` özellik erişimcileri ayrı üyeleri olmayan ve bir özelliğin erişimcileri ayrı olarak bildirmek mümkün değildir. Bu nedenle, farklı erişilebilirliğe sahip iki erişimci okuma-yazma özelliği için mümkün değildir. Örnek
```csharp
class A
{
    private string name;

    public string Name {                // Error, duplicate member name
        get { return name; }
    }

    public string Name {                // Error, duplicate member name
        set { name = value; }
    }
}
```
tek bir okuma-yazma özelliği bildirmiyor. Bunun yerine, bu iki özellik aynı ada sahip, bir salt okunur bildirir ve bir salt yazılır. Örnek aynı sınıfta bildirilen iki üye aynı ada sahip olamaz olduğundan, bir derleme zamanı hatası oluşmasına neden olur.

Türetilmiş bir sınıf, devralınmış bir özellik olarak aynı adı taşıyan bir özelliği bildirir, hem okumaya hem yazmaya göre devralınan özelliği türetilmiş özelliğini gizler. Örnekte
```csharp
class A
{
    public int P {
        set {...}
    }
}

class B: A
{
    new public int P {
        get {...}
    }
}
```
`P` özelliğinde `B` gizler `P` özelliğinde `A` hem okuma ve yazma ile ilgili. Bu nedenle, ifadeler
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
atamayı `b.P` salt okunur beri bildirilmesini bir derleme zamanı hatasına neden olur `P` özelliğinde `B` salt yazılır gizler `P` özelliğinde `A`. Ancak, bir yayın gizli erişmek için kullanılabileceğini unutmayın `P` özelliği.

Özellikler, ortak alanları, bir nesnenin iç durumu ve ortak arabirimi arasında bir ayrım sağlar. Örneği göz önünde bulundurun:
```csharp
class Label
{
    private int x, y;
    private string caption;

    public Label(int x, int y, string caption) {
        this.x = x;
        this.y = y;
        this.caption = caption;
    }

    public int X {
        get { return x; }
    }

    public int Y {
        get { return y; }
    }

    public Point Location {
        get { return new Point(x, y); }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Burada, `Label` sınıfını kullanan iki `int` alanları `x` ve `y`konumunu saklamak için. Herkese açık şekilde konumdur her ikisi de olarak kullanıma sunulan bir `X` ve `Y` özelliği ve bir `Location` türünün özelliği `Point`. Eğer, gelecek bir sürümünde `Label`, konumu olarak depolamak daha kullanışlı hale gelir bir `Point` sınıfının ortak arabirimi etkilemeden değişiklik dahili olarak, yapılabilir:
```csharp
class Label
{
    private Point location;
    private string caption;

    public Label(int x, int y, string caption) {
        this.location = new Point(x, y);
        this.caption = caption;
    }

    public int X {
        get { return location.x; }
    }

    public int Y {
        get { return location.y; }
    }

    public Point Location {
        get { return location; }
    }

    public string Caption {
        get { return caption; }
    }
}
```

Vardı `x` ve `y` bunun yerine silinmiş `public readonly` alanları, olabilirdi böyle bir değişiklik yapmak mümkün `Label` sınıfı.

Durum özellikleri aracılığıyla kullanıma sunmak, alanlar doğrudan sunmaktan mutlaka daha az verimli değil. Özellikle, bir özellik sanal olmayan ve yalnızca küçük miktarda kod içerdiğinde yürütme ortamı erişimcileri gerçek koduyla erişimcileri çağrıları değiştirebilir. Bu işlem olarak bilinir ***inlining'i***, özellik erişimi olarak alan erişimini verimli hale getirir henüz özelliklerinin daha fazla esneklik korur.

Çağrılırken bu yana bir `get` erişimci eşdeğerdir bir alanın değerini okumak için kavramsal olarak, hatalı programlama stili olarak kabul edilir `get` gözlemlenebilir bir yan etkileri olduğu erişimcileri. Örnekte
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
Değerini `Next` özelliği özelliği daha önce erişilmeden kez sayısına bağlıdır. Bu nedenle, özelliğine erişmek için gözlemlenebilir bir yan etkisi üretir ve özellik yöntemi olarak bunun yerine uygulanmalıdır.

İçin "hiçbir yan etkiler" kuralı `get` erişimcileri, ortalama değil `get` erişimcileri her zaman yazılması alanlarında depolanan değerleri döndürülecek. Aslında, `get` erişimcileri birden çok alan erişme veya yöntemlerini çağırmaktan bir özelliğin değerini genellikle işlem. Ancak, düzgün bir şekilde tasarlanmış `get` erişimci gözlemlenebilir değişiklikleri nesne durumunda neden herhangi bir eylem gerçekleştirir.

İlk başvurulan şu kadar olan bir kaynağın başlatma gecikme özellikleri kullanılabilir. Örneğin:
```csharp
using System.IO;

public class Console
{
    private static TextReader reader;
    private static TextWriter writer;
    private static TextWriter error;

    public static TextReader In {
        get {
            if (reader == null) {
                reader = new StreamReader(Console.OpenStandardInput());
            }
            return reader;
        }
    }

    public static TextWriter Out {
        get {
            if (writer == null) {
                writer = new StreamWriter(Console.OpenStandardOutput());
            }
            return writer;
        }
    }

    public static TextWriter Error {
        get {
            if (error == null) {
                error = new StreamWriter(Console.OpenStandardError());
            }
            return error;
        }
    }
}
```

`Console` Sınıfı üç özellik içerir `In`, `Out`, ve `Error`, temsil eden standart giriş, çıkış ve hata cihazları, sırasıyla. Bu üye özellikleri olarak gösterme tarafından `Console` gerçekten kullanılana kadar sınıf başlatma gecikme. Örneğin, ilk başvuran bağlı `Out` görüldüğü özelliği
```csharp
Console.Out.WriteLine("hello, world");
```
arka plandaki `TextWriter` çıktı cihazına oluşturulur. Ancak uygulama başvuru yaparsa `In` ve `Error` özellikleri ve herhangi bir nesnenin bu cihazlar için oluşturulur.

### <a name="automatically-implemented-properties"></a>Otomatik olarak uygulanan özellikler

Otomatik olarak uygulanan bir özellik (veya ***otomatik-özellik*** kısaca), yalnızca noktalı virgül erişimcisinin gövdesi ile soyut olmayan extern olmayan özelliğidir. Otomatik özelliklerde bir get erişimcisine sahip olmalıdır ve isteğe bağlı olarak ayarlama erişimcisine sahip olabilir.

Bir özelliği otomatik olarak uygulanan bir özellik olarak belirtildiğinde, gizli destek alanı özelliği için otomatik olarak kullanılabilir ve erişimcileri, okuma ve yazma ilgili yedekleme alanı için uygulanır. Otomatik-özellik set erişimcisine sahip değildir, yedekleme alanını kabul edildiği `readonly` ([salt okunur alanların](classes.md#readonly-fields)). Olduğu gibi bir `readonly` alan, bir yalnızca otomatik özellik de atanabilen kapsayan sınıfın bir oluşturucu gövdesinde. Bu tür bir atama doğrudan salt okunur yedekleme alanına özelliğinin atar.

Otomatik özellik isteğe bağlı olarak olabilir bir *property_initializer*, doğrudan yedekleme alanı olarak için uygulanan bir *variable_initializer* ([değişken başlatıcılar](classes.md#variable-initializers)) .

Aşağıdaki örnekte:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
Aşağıdaki bildirime eşdeğerdir:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

Aşağıdaki örnekte:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
Aşağıdaki bildirime eşdeğerdir:
```csharp
public class ReadOnlyPoint
{
    private readonly int __x;
    private readonly int __y;
    public int X { get { return __x; } }
    public int Y { get { return __y; } }
    public ReadOnlyPoint(int x, int y) { __x = x; __y = y; }
}
```

Oluşturucu içinde ortaya olduğundan salt okunur alan atamalarını yasal, olduğuna dikkat edin.


### <a name="accessibility"></a>Erişilebilirlik

Erişimci varsa bir *accessor_modifier*, Erişilebilirlik etki alanı ([erişilebilirlik etki alanı](basic-concepts.md#accessibility-domains)) erişimcisine öğesinin bildirilen erişilebilirliği belirlenir *accessor_modifier* . Erişimci yoksa bir *accessor_modifier*, özelliğin veya dizin oluşturucu bildirilen erişilebilirliği erişimcisinin erişilebilirlik etki alanı belirlenir.

Varlığı bir *accessor_modifier* hiç üye araması etkiler ([işleçleri](expressions.md#operators)) veya aşırı yükleme çözümlemesi ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)). Değiştiriciler özelliği veya dizin oluşturucu hangi özelliğin veya dizin oluşturucu bağımsız olarak erişim bağlamında bağlı olduğu her zaman belirleyin.

Belirli bir özellik veya dizin oluşturucu seçildikten sonra ilgili özel erişimciler erişilebilirlik etki alanları, kullanım geçerli olup olmadığını belirlemek için kullanılır:

*  Kullanım bir değer olarak ise ([ifadelerin değerleri](expressions.md#values-of-expressions)), `get` erişimci mevcut ve erişilebilir olması gerekir.
*  Kullanımı basit atama hedefi olarak ayarlanırsa ([basit atama](expressions.md#simple-assignment)), `set` erişimci mevcut ve erişilebilir olması gerekir.
*  Kullanım bileşik atama hedefi olarak olup olmadığını ([bileşik atama](expressions.md#compound-assignment)), veya hedefi olarak `++` veya `--` işleçleri ([işlev üyeleri](expressions.md#function-members).9, [ Çağrı ifadeleri](expressions.md#invocation-expressions)), her iki `get` erişimcileri ve `set` erişimci mevcut ve erişilebilir olması gerekir.

Aşağıdaki örnekte, özellik `A.Text` özelliği tarafından gizleniyor `B.Text`yalnızca olduğunda bile bağlamlarda `set` erişimci çağrılır. Buna karşılık, özellik `B.Count` sınıfına erişilemiyor `M`, bu nedenle erişilebilir özellik `A.Count` yerine kullanılır.

```csharp
class A
{
    public string Text {
        get { return "hello"; }
        set { }
    }

    public int Count {
        get { return 5; }
        set { }
    }
}

class B: A
{
    private string text = "goodbye"; 
    private int count = 0;

    new public string Text {
        get { return text; }
        protected set { text = value; }
    }

    new protected int Count { 
        get { return count; }
        set { count = value; }
    }
}

class M
{
    static void Main() {
        B b = new B();
        b.Count = 12;             // Calls A.Count set accessor
        int i = b.Count;          // Calls A.Count get accessor
        b.Text = "howdy";         // Error, B.Text set accessor not accessible
        string s = b.Text;        // Calls B.Text get accessor
    }
}
```

Bir arabirim uygulamak için kullanılan bir erişimci olmayabilir bir *accessor_modifier*. Yalnızca bir erişimci bir arabirim uygulamak için kullanılan diğer erişimcisi ile bildirilebilir bir *accessor_modifier*:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Korumalı sanal, geçersiz kılma ve Özet özellik erişimcileri

A `virtual` özellik bildiriminde özelliğin erişimcileri sanal olduğunu belirtir. `virtual` Değiştiricisi, bir okuma-yazma özellik erişenleri için uygular — sanal yalnızca bir erişimci okuma-yazma özelliği için mümkün değildir.

Bir `abstract` özellik bildiriminde özellik erişimcileri sanal ancak gerçek erişimcileri uygulanışı sağlamaz belirtir. Bunun yerine, türetilen soyut olmayan sınıflar özellik geçersiz kılarak erişimcileri için kendi uygulama sağlamak için gereklidir. Bir soyut özellik bildiriminde bir erişimci hiçbir kullanımınız sağladığından, *accessor_body* yalnızca noktalı virgül içerir.

Her ikisini de içeren bir özellik bildirimi `abstract` ve `override` değiştiriciler özelliği soyuttur ve bir temel özelliğini geçersiz kılar belirtir. Böyle bir özellik erişimcisi de soyuttur.

Soyut özellik bildirimleri yalnızca soyut sınıfları izin ([soyut sınıflar](classes.md#abstract-classes)). Devralınmış bir sanal özellik erişimcileri belirten bir özellik bildirimi dahil olmak üzere türetilen bir sınıfta kılınabilir bir `override` yönergesi. Bu olarak bilinen bir ***özellik bildiriminde geçersiz kılma***. Bir geçersiz kılma özelliği bildirimi, yeni bir özellik bildirmiyor. Bunun yerine, mevcut sanal özelliğin erişimcileri, uygulamaları yalnızca uzmanlaşmış.

Bir geçersiz kılma özellik bildiriminde tam aynı erişilebilirlik değiştiricileri, tür ve ad devralınan özelliği olarak belirtmeniz gerekir. Varsa (yani, devralınan özelliği salt okunur veya salt yazılır) devralınan özelliği yalnızca tek bir erişimci sahipse, geçersiz kılma özelliği yalnızca bir erişimci içermesi gerekir. Varsa (yani, devralınan özelliği salt okunur ise), her iki erişimcisi devralınan özelliği içerir, geçersiz kılma özelliği tek bir erişimci veya her iki erişimcisi içerebilir.

Bir geçersiz kılma özelliği bildirimi içerebilir `sealed` değiştiricisi. Bu değiştirici kullanımını türetilmiş bir sınıf, daha fazla özellik geçersiz kılmasını önler. Ayrıca korumalı bir korumalı özelliğin erişimcileri.

Bildirim ve çağırma farklılıkları dışında sözdizimi, sanal, sealed bir geçersiz kılma ve soyut erişimcileri tam olarak sanal, sealed bir geçersiz kılma ve Özet yöntemler gibi davranır. Kuralları açıklanan özellikle [sanal yöntemleri](classes.md#virtual-methods), [geçersiz kılma yöntemleri](classes.md#override-methods), [yöntemleri korumalı](classes.md#sealed-methods), ve [soyut yöntemler](classes.md#abstract-methods) uygulamak gibi erişimciler yöntemleri karşılık gelen bir formun şöyleydi:

*  A `get` bir parametresiz yöntemin dönüş değeri içeren özellik olarak aynı değiştiricilere ve özellik türü ile erişimci karşılık gelir.
*  A `set` erişimci karşılık gelen bir tek değer özellik türü parametresi olan bir yönteme bir `void` dönüş türü ve aynı değiştiricilere kapsayan özelliği olarak.

Örnekte
```csharp
abstract class A
{
    int y;

    public virtual int X {
        get { return 0; }
    }

    public virtual int Y {
        get { return y; }
        set { y = value; }
    }

    public abstract int Z { get; set; }
}
```
`X` bir sanal salt okunur özelliği `Y` sanal bir okuma-yazma özelliği ve `Z` soyut bir okuma-yazma özelliği. Çünkü `Z` soyuttur, kapsayan sınıfı `A` da soyut bildirilmelidir.

Türetilen bir sınıf `A` aşağıdaki gösterisi:
```csharp
class B: A
{
    int z;

    public override int X {
        get { return base.X + 1; }
    }

    public override int Y {
        set { base.Y = value < 0? 0: value; }
    }

    public override int Z {
        get { return z; }
        set { z = value; }
    }
}
```

Burada, bildirimlerini `X`, `Y`, ve `Z` özellik bildirimleri geçersiz. Her özellik bildiriminde erişilebilirlik değiştiricileri, türü ve karşılık gelen devralınan özelliğin adını tam olarak eşleşir. `get` Erişimcisine `X` ve `set` erişimcisine `Y` kullanın `base` devralınan erişimcileri erişmek için anahtar sözcüğü. Bildirimi `Z` soyut her iki erişimcisi geçersiz kılar; bu nedenle, hiçbir bekleyen soyut işlev üyeleri vardır `B`, ve `B` soyut olmayan bir sınıf olması için izin verilir.

Ne zaman bir özelliği olarak bildirilen bir `override`, herhangi bir geçersiz kılınan erişimcileri geçersiz kılma koda erişilebilir olması gerekir. Ayrıca, özellik veya dizin oluşturucu kendisini ve erişimcileri öğesinin bildirilen erişilebilirliği erişimcileri ve geçersiz kılınan üyesiyle eşleşmelidir. Örneğin:
```csharp
public class B
{
    public virtual int P {
        protected set {...}
        get {...}
    }
}

public class D: B
{
    public override int P {
        protected set {...}            // Must specify protected here
        get {...}                      // Must not have a modifier here
    }
}
```

## <a name="events"></a>Olaylar

Bir ***olay*** bir nesne veya sınıf bildirimleri sağlamak için sağlayan bir üyesidir. İstemciler, olaylar için yürütülebilir kodu sağlayarak iliştirebilirsiniz ***olay işleyicileri***.

Olayları kullanarak bildirilir *event_declaration*: %s

```antlr
event_declaration
    : attributes? event_modifier* 'event' type variable_declarators ';'
    | attributes? event_modifier* 'event' type member_name '{' event_accessor_declarations '}'
    ;

event_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'static'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | event_modifier_unsafe
    ;

event_accessor_declarations
    : add_accessor_declaration remove_accessor_declaration
    | remove_accessor_declaration add_accessor_declaration
    ;

add_accessor_declaration
    : attributes? 'add' block
    ;

remove_accessor_declaration
    : attributes? 'remove' block
    ;
```

Bir *event_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve geçerli bir bileşimin dört erişim değiştiricileri ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemler](classes.md#static-and-instance-methods)), `virtual` ([sanalyöntemleri](classes.md#virtual-methods)), `override` ([Geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([Dış yöntemleri](classes.md#external-methods)) değiştiriciler.

Yöntem bildirimleri gibi aynı kurallara olay bildirimlerine tabidir ([yöntemleri](classes.md#methods)) değiştiriciler geçerli birleşimlerini onaylamaz.

*Türü* olay bildirimi olmalıdır bir *delegate_type* ([başvuru türleri](types.md#reference-types)) ve *delegate_type* en az olarak olay olarak erişilebilir ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

Bir olay bildirimi içerebilir *event_accessor_declarations*. Olmayan-extern, soyut olmayan olaylar içermiyorsa, ancak derleyicinin bunları otomatik olarak sağlar ([alan benzeri olaylara](classes.md#field-like-events)); extern olaylar için erişimciler harici olarak sağlanır.

Olay bildiriminde atlar *event_accessor_declarations* bir veya daha fazla olayı tanımlar — için birer tane *variable_declarator*s. Tüm gibi tarafından bildirilen üyeler değiştiriciler ve öznitelikleri geçerli bir *event_declaration*.

Bir derleme zamanı hata için bir *event_declaration* hem de içerecek şekilde `abstract` değiştiricisi ve küme ayracı ayrılmış *event_accessor_declarations*.

Ne zaman bir olay bildirimi içeren bir `extern` değiştiricisi, bir olay olarak kabul edilir bir ***dış olay***. Bir dış olay bildirimi gerçek uygulaması sağladığından, her ikisi de içerecek şekilde için bir hata olduğu `extern` değiştiricisi ve *event_accessor_declarations*.

Bir derleme zamanı hata için bir *variable_declarator* ile bir olay bildiriminin bir `abstract` veya `external` içerecek şekilde değiştiricisi bir *variable_initializer*.

Bir olay sol işleneni kullanılabilir `+=` ve `-=` işleçleri ([olay ataması](expressions.md#event-assignment)). Bu işleçler, sırasıyla, olay işleyicileri eklemek veya bir olay, olay işleyicilerini kaldırmak için kullanılır ve bu işlemler izin bağlamları olayın erişim değiştiricileri denetleyebilirsiniz.

Bu yana `+=` ve `-=` bildirir olay, dış kod türü dışında bir olay için izin verilen işlemler ekleyin ve bir olay işleyicilerini kaldırmak ancak olamaz diğer herhangi bir yolla edinebilir veya temel alınan olay listesini değiştirmek olan işleyicileri.

Formun işleminde `x += y` veya `x -= y`, `x` bir olaydır ve bildirimini içeren türü dışında bir yerde başvuru alan `x`, işlemin sonucunu türüne sahip `void` (aksine sahip türünü `x`, değeriyle `x` Atama işleminden sonra). Bu kural, bir olayın temel temsilci dolaylı olarak İnceleme dış kod yasaklar.

Aşağıdaki örnek, olay işleyicileri örneklerine bağlı nasıl gösterir `Button` sınıfı:
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;
}

public class LoginDialog: Form
{
    Button OkButton;
    Button CancelButton;

    public LoginDialog() {
        OkButton = new Button(...);
        OkButton.Click += new EventHandler(OkButtonClick);
        CancelButton = new Button(...);
        CancelButton.Click += new EventHandler(CancelButtonClick);
    }

    void OkButtonClick(object sender, EventArgs e) {
        // Handle OkButton.Click event
    }

    void CancelButtonClick(object sender, EventArgs e) {
        // Handle CancelButton.Click event
    }
}
```

Burada, `LoginDialog` örnek oluşturucusu oluşturur iki `Button` örnekler ve ekler için olay işleyicileri `Click` olayları.

### <a name="field-like-events"></a>Alan benzeri olaylara

Sınıf veya bir olay bildirimini içeren yapı öğesinin program metni alanlar gibi belirli olaylar kullanılabilir. Bu şekilde kullanılması için bir olay değil olmalıdır `abstract` veya `extern`ve açıkça içermemelidir *event_accessor_declarations*. Böyle bir olaya izin veren bir alan her bağlamda kullanılabilir. Alanın bir temsilci içerir ([Temsilciler](delegates.md)) eklenmiş olan olaya olay işleyicileri listesine başvuruyor. Hiçbir olay işleyicileri eklenmişse içeren alan `null`.

Örnekte
```csharp
public delegate void EventHandler(object sender, EventArgs e);

public class Button: Control
{
    public event EventHandler Click;

    protected void OnClick(EventArgs e) {
        if (Click != null) Click(this, e);
    }

    public void Reset() {
        Click = null;
    }
}
```
`Click` bir alanı olarak kullanılan `Button` sınıfı. Örnekte de gösterildiği gibi alanın incelenir, değiştirilebilir ve temsilci çağırma ifadelerinde kullanılır. `OnClick` Yönteminde `Button` sınıf "harekete geçirirse" `Click` olay. Olay bildirmek, kavram olay tarafından temsil edilen temsilci çağırmak için tam olarak eşittir; Bu nedenle, olayları oluşturma için hiçbir özel dil yapıları vardır. Temsilci çağrısı null olmayan bir temsilci sağlar bir denetimi tarafından bulunduğuna dikkat edin.

Bildirimi dışındaki `Button` sınıfı `Click` üye yalnızca sol tarafında, kullanılabilir `+=` ve `-=` görüldüğü işleçleri
```csharp
b.Click += new EventHandler(...);
```
çağırma listesine bir temsilci ekler `Click` olay ve
```csharp
b.Click -= new EventHandler(...);
```
çağırma listesinden bir temsilciyi kaldırır `Click` olay.

Alan benzeri olay derlerken, derleyici otomatik olarak temsilci tutmak için depolama oluşturur ve temsilci alanı için olay işleyicileri ekleyip erişimcileri olayı oluşturur. Ekleme ve kaldırma işlemlerinin iş parçacığı güvenlidir ve olabilir (ancak gerekli değildir) olması biraz yanıtlar bulunduran kilit ([lock deyiminin](statements.md#the-lock-statement)) içeren bir nesne için bir örnek olay veya tür nesnesi ([anonim Nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) statik bir olay için.

Bu nedenle, bir örnek olay bildirimi formun:
```csharp
class X
{
    public event D Ev;
}
```
eşdeğer bir şey derlenecek:
```csharp
class X
{
    private D __Ev;  // field to hold the delegate

    public event D Ev {
        add {
            /* add the delegate in a thread safe way */
        }

        remove {
            /* remove the delegate in a thread safe way */
        }
    }
}
```
Sınıf içindeki `X`, başvurular `Ev` sol tarafındaki `+=` ve `-=` işleçler neden ekleme ve kaldırma erişimcileri çağrılacak. Tüm başvuruları `Ev` gizli alan başvurmak için derlenmiş `__Ev` bunun yerine ([üye erişimi](expressions.md#member-access)). Adı "`__Ev`" rastgeledir; gizli alanı hiç herhangi bir ad veya ad olabilir.

### <a name="event-accessors"></a>Olay erişimcileri

Olay bildirimleri genellikle atlamak *event_accessor_declarations*, olarak `Button` Yukarıdaki örneği. Bunu yapmak için bir durum kadar olay başına tek bir alanın depolama maliyeti kabul edilebilir değil çalışması içerir. Bu gibi durumlarda, bir sınıf içerebilir *event_accessor_declarations* ve olay işleyicileri listesi depolamakla özel bir mekanizma kullanabilirsiniz.

*Event_accessor_declarations* ekleme ve kaldırma olay işleyicileri ile ilişkili yürütülebilir deyimleri bir olay belirtin.

Erişimcisi bildirimlerine oluşan bir *add_accessor_declaration* ve *remove_accessor_declaration*. Belirteci her erişimci bildirimi oluşur `add` veya `remove` arkasından bir *blok*. *Blok* ile ilişkili bir *add_accessor_declaration* bir olay işleyicisi eklendiğinde çalıştırılacak deyimleri belirtir ve *blok* bir ileilişkili*remove_accessor_declaration* bir olay işleyicisi kaldırıldığında çalıştırılacak deyimleri belirtir.

Her *add_accessor_declaration* ve *remove_accessor_declaration* karşılık gelen bir olay türü tek değer parametresi olan bir yönteme ve `void` dönüş türü. Örtük bir olay erişimcisinin parametresi adlı `value`. Bir olay Olay atama kullanıldığında, ilgili olay erişimcisi kullanılır. Özellikle de atama işleci ise `+=` ekleme erişimcisi kullanılır ve atama işleci ise `-=` ait kaldırma erişimcisi kullanılır. Her iki durumda da atama işlecinin sağ işleneni, bağımsız değişkeni olarak bir olay erişimcisi kullanılır. Bloğunu bir *add_accessor_declaration* veya *remove_accessor_declaration* kurallarına uymalıdır `void` açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body). Özellikle, `return` ifadeleri gibi bir blok içinde bir ifade belirtin izin verilmez.

Bir olay erişimcisi örtük olarak adlı bir parametre olduğundan `value`, yerel bir değişken veya sabit bildirilen bu ada sahip bir olay erişimcisi için bir derleme zamanı hatası olduğu.

Örnekte
```csharp
class Control: Component
{
    // Unique keys for events
    static readonly object mouseDownEventKey = new object();
    static readonly object mouseUpEventKey = new object();

    // Return event handler associated with key
    protected Delegate GetEventHandler(object key) {...}

    // Add event handler associated with key
    protected void AddEventHandler(object key, Delegate handler) {...}

    // Remove event handler associated with key
    protected void RemoveEventHandler(object key, Delegate handler) {...}

    // MouseDown event
    public event MouseEventHandler MouseDown {
        add { AddEventHandler(mouseDownEventKey, value); }
        remove { RemoveEventHandler(mouseDownEventKey, value); }
    }

    // MouseUp event
    public event MouseEventHandler MouseUp {
        add { AddEventHandler(mouseUpEventKey, value); }
        remove { RemoveEventHandler(mouseUpEventKey, value); }
    }

    // Invoke the MouseUp event
    protected void OnMouseUp(MouseEventArgs args) {
        MouseEventHandler handler; 
        handler = (MouseEventHandler)GetEventHandler(mouseUpEventKey);
        if (handler != null)
            handler(this, args);
    }
}
```
`Control` sınıfı olaylar için bir iç depolama mekanizması uygular. `AddEventHandler` Yöntemi temsilci değer bir anahtar ile ilişkilendirir `GetEventHandler` yöntem şu anda bir anahtar ile ilişkilendirilmiş olan metot temsilcisinin döndürür ve `RemoveEventHandler` yöntemi, belirtilen olay için bir olay işleyicisi olarak bir temsilci kaldırır. Yok ilişkilendirmek için ücretsiz, temel depolama mekanizması muhtemelen, tasarlanmış bir `null` değeri bir anahtar ile temsilci olarak seçmeyi ve bu nedenle hiçbir depolama işlenmemiş olayları kullanma.

### <a name="static-and-instance-events"></a>Statik ve örnek olaylar

Ne zaman bir olay bildirimi içeren bir `static` değiştiricisi, bir olay olarak kabul edilir bir ***statik olay***. Hiçbir `static` değiştirici mevcut, olay olarak kabul edilir bir ***örnek olay***.

Statik olay belirli bir örneği ile ilişkili değil ve başvurmak için bir derleme zamanı hata `this` statik bir olayın Erişimcilerde.

Bir örnek olay bir sınıfın belirli bir örneği ile ilişkilendirilir ve bu örnek olarak erişilebilen `this` ([bu erişim](expressions.md#this-access)) Bu olayın Erişimcilerde.

Ne zaman bir olay başvuruluyor bir *member_access* ([üye erişimi](expressions.md#member-access)) biçiminde `E.M`, `M` statik bir olay `E` içerenbirtürbelirtmekgerekir`M`ve eğer `M` bir örnek olay E türünü içeren bir örneğini belirtmek gerekir `M`.

Statik arasındaki farklar ve örnek üyeleri ele alınmıştır daha ayrıntılı olarak [statik ve örnek üyeleri](classes.md#static-and-instance-members).

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Korumalı sanal, geçersiz kılma ve soyut olay erişimcileri

A `virtual` olay bildirimi olay erişimcileri sanal olduğunu belirtir. `virtual` Değiştiricisi, bir olay erişenleri için geçerlidir.

Bir `abstract` olay bildirimi, olay erişimcileri sanal ancak gerçek erişimcileri uygulanışı sağlamaz belirtir. Bunun yerine, türetilen soyut olmayan sınıflar, geçersiz kılarak olay erişimcileri için kendi uygulamasını sağlamak için gereklidir. Bir soyut olay bildirimi gerçek uygulaması sağladığından, küme ayracı ayrılmış sağlayamaz *event_accessor_declarations*.

Her ikisini de içeren bir olay bildirimi `abstract` ve `override` değiştiriciler olay soyuttur ve temel olayı geçersiz kılmaları belirtir. Böyle bir olay erişimci de soyut.

Soyut olay bildirimleri yalnızca soyut sınıfları izin ([soyut sınıflar](classes.md#abstract-classes)).

Devralınan sanal olay erişimcileri belirten bir olay bildirimi dahil olmak üzere türetilen bir sınıfta kılınabilir bir `override` değiştiricisi. Bu olarak bilinen bir ***olay bildirimi geçersiz kılma***. Yeni bir olay Olay bildiriminde geçersiz kılma bildirmiyor. Bunun yerine, mevcut bir sanal olay erişimcileri, uygulamaları yalnızca uzmanlaşmış.

Olay bildiriminde geçersiz kılma tam aynı erişilebilirlik değiştiricileri, türü ve adı geçersiz kılınan olay belirtmeniz gerekir.

Bir geçersiz kılma olay bildirimi içerebilir `sealed` değiştiricisi. Bu değiştirici kullanımını türetilmiş bir sınıf daha fazla olay geçersiz kılmasını önler. Ayrıca korumalı bir korumalı olay erişimcileri.

Bir derleme zamanı hata dahil etmek bir geçersiz kılma olay bildirimi için bir `new` değiştiricisi.

Bildirim ve çağırma farklılıkları dışında sözdizimi, sanal, sealed bir geçersiz kılma ve soyut erişimcileri tam olarak sanal, sealed bir geçersiz kılma ve Özet yöntemler gibi davranır. Kuralları açıklanan özellikle [sanal yöntemleri](classes.md#virtual-methods), [geçersiz kılma yöntemleri](classes.md#override-methods), [yöntemleri korumalı](classes.md#sealed-methods), ve [soyut yöntemler](classes.md#abstract-methods) uygulamak gibi erişimciler yöntemleri karşılık gelen bir formun yoktu. Olay türü, bir tek değer parametresi olan bir yönteme her erişimci karşılık gelen bir `void` dönüş türü ve içeren olayla aynı değiştiriciler.

## <a name="indexers"></a>Dizin Oluşturucular

Bir ***dizin oluşturucu*** bir nesne dizisi olarak aynı şekilde dizinlenmesini sağlar bir üyesidir. Dizin oluşturucular kullanarak bildirilir *indexer_declaration*: %s

```antlr
indexer_declaration
    : attributes? indexer_modifier* indexer_declarator indexer_body
    ;

indexer_modifier
    : 'new'
    | 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'virtual'
    | 'sealed'
    | 'override'
    | 'abstract'
    | 'extern'
    | indexer_modifier_unsafe
    ;

indexer_declarator
    : type 'this' '[' formal_parameter_list ']'
    | type interface_type '.' 'this' '[' formal_parameter_list ']'
    ;

indexer_body
    : '{' accessor_declarations '}' 
    | '=>' expression ';'
    ;
```

Bir *indexer_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve geçerli bir bileşimin dört erişim değiştiricileri ([erişim değiştiricileri ](classes.md#access-modifiers)), `new` ([yeni değiştiricisini](classes.md#the-new-modifier)), `virtual` ([sanal yöntemleri](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods) ), `sealed` ([Yöntemleri korumalı](classes.md#sealed-methods)), `abstract` ([soyut yöntemler](classes.md#abstract-methods)), ve `extern` ([dış yöntemleri](classes.md#external-methods)) değiştiriciler.

Dizin Oluşturucu bildirimlerdir yöntem bildirimleri gibi aynı kurallara tabi ([yöntemleri](classes.md#methods)) değiştiriciler geçerli birleşimlerini onaylamaz, statik değiştirici olan bir özel durumla bir dizin oluşturucu bildiriminde izin verilmez.

Değiştiriciler `virtual`, `override`, ve `abstract` bir durumda dışında kullanılamaz. `abstract` Ve `override` değiştiriciler kullanılabilir birlikte böylece sanal bir soyut bir dizin oluşturucu geçersiz kılabilirsiniz.

*Türü* bir dizin oluşturucu bildirim bildirim tarafından tanıtılan dizin oluşturucu öğenin türünü belirtir. Açık arabirim üyesi uygulaması, dizin oluşturucu olmadığı sürece *türü* anahtar sözcüğü tarafından izlenen `this`. Bir açık arabirim üyesi uygulaması *türü* tarafından izlenen bir *INTERFACE_TYPE*, "`.`" ve anahtar sözcüğü `this`. Diğer üyeleri farklı olarak, dizin oluşturucular kullanıcı tanımlı adları yok.

*Formal_parameter_list* dizin oluşturucunun parametreleri belirtir. Bir dizin oluşturucu biçimsel parametre listesi, yöntemin karşılık gelir ([yöntem parametreleri](classes.md#method-parameters)) en az bir parametresi belirtilmelidir, dışında ve `ref` ve `out` parametre değiştiricilere izin verilmez .

*Türü* bir dizin oluşturucu ve her başvurulan tür *formal_parameter_list* en az Indexer olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

Bir *indexer_body* herhangi birini içerebilir bir ***erişimcisinin gövdesi*** veya ***ifade gövdesi***. Bir erişimci gövdesindeki *accessor_declarations*, hangi alınmalıdır içinde "`{`"ve"`}`" belirteçleri, erişimci bildirir ([erişimcileri](classes.md#accessors)) özelliği. Erişimciler okuma ve yazma özelliği ilişkili yürütülebilir deyimleri belirtin.

Oluşan bir deyim gövdesinin "`=>`" bir ifade tarafından izlenen `E` ve noktalı virgül deyim gövdesi için tam olarak eşdeğerdir `{ get { return E; } }`ve bu nedenle yalnızca yalnızca dizin oluşturucular belirtmek için alıcının sonucu olduğu kullanılabilir tek bir ifade belirler.

Dizin Oluşturucu öğenin erişmek için söz dizimi aynı dizi öğesi için olsa da, bir değişken olarak bir dizin oluşturucu öğenin sınıflandırılmaz. Bu nedenle, dizin oluşturucu öğenin olarak geçirmek mümkün değil bir `ref` veya `out` bağımsız değişken.

İmza biçimsel parametre listesinde bir dizin oluşturucu tanımlar ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) dizin oluşturucu. Özellikle, bir dizin oluşturucu imzası, sayı ve biçimsel parametrelerinin türleri oluşur. Öğe türü ve biçimsel parametrelerinin adları bir oluşturucunun imzasının parçası değildir.

Bir dizin oluşturucu imzasının aynı sınıfta bildirilen tüm diğer dizin imzalarını farklı olmalıdır.

Dizin oluşturucular ve özellikler çok benzer, ancak şu bakımlardan ayrılır:

*  Bir dizin oluşturucu imzası tarafından tanımlanan ise özellik sunucu adına göre tanımlanır.
*  Bir özellik üzerinden erişilen bir *simple_name* ([basit adları](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)), ancak bir dizin oluşturucu öğesi aracılığıyla erişilen bir *element_access* ([dizinleyici erişimi](expressions.md#indexer-access)).
*  Bir özellik ayarlanabilir bir `static` üyesi, ancak dizin oluşturucunun her zaman bir örnek üyesi.
*  A `get` özellik erişimcisi karşılık gelen bir yönteme parametre olmadan, ancak bir `get` erişimci bir dizin oluşturucu aynı dizin oluşturucu biçimsel parametre listesine sahip bir yöntemi karşılık gelir.
*  A `set` özellik erişimcisi karşılık gelen bir yönteme adlı tek bir parametre `value`bilgileriyse bir `set` bir yöntemle aynı biçimsel parametre listesi dizin oluşturucu yanı sıra, ek bir parametre olarak bir dizin oluşturucu erişimcisine karşılık gelir adlı `value`.
*  Dizin Oluşturucu parametresi olarak aynı ada sahip yerel bir değişken bildirmek bir dizin oluşturucu erişimcisi için bir derleme zamanı hatasıdır.
*  Bir geçersiz kılma özelliği bildiriminde devralınan özelliği söz dizimi kullanılarak erişilen `base.P`burada `P` özellik adı. Geçersiz kılan bir dizin oluşturucu bildiriminde devralınan dizin oluşturucu sözdizimi kullanılarak erişilir `base[E]`burada `E` ifadelerin virgülle ayrılmış listesidir.
*  "Otomatik olarak uygulanan dizin oluşturucu" kavramı yoktur. Noktalı virgül erişimcileri ile soyut olmayan, dış olmayan bir dizinleyici olması hatadır.

Tüm kuralları aşağıdaki farklar dışında tanımlanan [erişimcileri](classes.md#accessors) ve [Özellikleri'otomatik olarak uygulanan](classes.md#automatically-implemented-properties) dizin oluşturucu erişimcileri de özellik erişimcileri için geçerlidir.

Ne zaman bir dizin oluşturucu bildirimi içeren bir `extern` değiştiricisi, dizin oluşturucu olduğu söylenir bir ***dış dizin oluşturucu***. Hiçbir gerçek uygulama, her biri bir dış dizin oluşturucu bildirimi sağladığından, *accessor_declarations* noktalı virgül içerir.

Aşağıdaki örnekte bildiren bir `BitArray` bit dizideki her biti erişmek için bir dizin oluşturucu uygulayan sınıf.
```csharp
using System;

class BitArray
{
    int[] bits;
    int length;

    public BitArray(int length) {
        if (length < 0) throw new ArgumentException();
        bits = new int[((length - 1) >> 5) + 1];
        this.length = length;
    }

    public int Length {
        get { return length; }
    }

    public bool this[int index] {
        get {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            return (bits[index >> 5] & 1 << index) != 0;
        }
        set {
            if (index < 0 || index >= length) {
                throw new IndexOutOfRangeException();
            }
            if (value) {
                bits[index >> 5] |= 1 << index;
            }
            else {
                bits[index >> 5] &= ~(1 << index);
            }
        }
    }
}
```

Örneği `BitArray` sınıfı, karşılık gelen daha önemli ölçüde daha az bellek tüketir `bool[]` (önceki her değeri yerine bir bit kapladığı olduğundan ikinci bir bayt kullanıcının), ancak aynı işlemlere izin bir `bool[]`.

Aşağıdaki `CountPrimes` sınıfını kullanan bir `BitArray` ve belirli en fazla 1 ila primes sayısını hesaplamak için Klasik olan "sieve" algoritması:
```csharp
class CountPrimes
{
    static int Count(int max) {
        BitArray flags = new BitArray(max + 1);
        int count = 1;
        for (int i = 2; i <= max; i++) {
            if (!flags[i]) {
                for (int j = i * 2; j <= max; j += i) flags[j] = true;
                count++;
            }
        }
        return count;
    }

    static void Main(string[] args) {
        int max = int.Parse(args[0]);
        int count = Count(max);
        Console.WriteLine("Found {0} primes between 1 and {1}", count, max);
    }
}
```

Unutmayın öğeleri erişmek için söz dizimi `BitArray` tam olarak aynı olan bir `bool[]`.

Aşağıdaki örnek, iki parametre ile bir dizin oluşturucu sahip bir 26 * 10 kılavuz sınıfı gösterir. İlk parametre bir üst - veya A-Z aralığındaki küçük harf olması gerekir ve ikinci 0-9 aralığında bir tamsayı olması gerekir.

```csharp
using System;

class Grid
{
    const int NumRows = 26;
    const int NumCols = 10;

    int[,] cells = new int[NumRows, NumCols];

    public int this[char c, int col] {
        get {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            return cells[c - 'A', col];
        }

        set {
            c = Char.ToUpper(c);
            if (c < 'A' || c > 'Z') {
                throw new ArgumentException();
            }
            if (col < 0 || col >= NumCols) {
                throw new IndexOutOfRangeException();
            }
            cells[c - 'A', col] = value;
        }
    }
}
```

### <a name="indexer-overloading"></a>Dizin oluşturucu aşırı yüklemesi

Dizin oluşturucu aşırı yükleme çözünürlüğü kuralları açıklanan [anlam çıkarma](expressions.md#type-inference).

## <a name="operators"></a>İşleçler

Bir ***işleci*** sınıf örnekleri için uygulanabilir bir ifade işleci anlamını tanımlayan bir üyesidir. İşleçleri kullanarak bildirilir *operator_declaration*: %s

```antlr
operator_declaration
    : attributes? operator_modifier+ operator_declarator operator_body
    ;

operator_modifier
    : 'public'
    | 'static'
    | 'extern'
    | operator_modifier_unsafe
    ;

operator_declarator
    : unary_operator_declarator
    | binary_operator_declarator
    | conversion_operator_declarator
    ;

unary_operator_declarator
    : type 'operator' overloadable_unary_operator '(' type identifier ')'
    ;

overloadable_unary_operator
    : '+' | '-' | '!' | '~' | '++' | '--' | 'true' | 'false'
    ;

binary_operator_declarator
    : type 'operator' overloadable_binary_operator '(' type identifier ',' type identifier ')'
    ;

overloadable_binary_operator
    : '+'   | '-'   | '*'   | '/'   | '%'   | '&'   | '|'   | '^'   | '<<'
    | 'right_shift' | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
    ;

conversion_operator_declarator
    : 'implicit' 'operator' type '(' type identifier ')'
    | 'explicit' 'operator' type '(' type identifier ')'
    ;

operator_body
    : block
    | '=>' expression ';'
    | ';'
    ;
```

Fazla yüklenebilir işleçler üç kategorisi vardır: tekli işleçler ([birli işleçler](classes.md#unary-operators)), ikili işleçler ([ikili işleçler](classes.md#binary-operators)) ve dönüştürme işleçleri ([dönüştürme işleçleri ](classes.md#conversion-operators)).

*Operator_body* ya da bir noktalı virgüldür bir ***deyim gövdesi*** veya ***ifade gövdesi***. Deyim gövdesi oluşan bir *blok*, işleci çağrıldığında çalıştırılacak deyimleri belirtir. *Blok* değeri döndürmek için kurallarına uymalıdır açıklanan yöntemlerden [yöntem gövdesini](classes.md#method-body). İfade gövdesi oluşan `=>` bir ifade ve noktalı virgül tarafından izlenen ve tek bir ifade işleci çağrıldığında gerçekleştirilecek gösterir.

İçin `extern` işleçleri *operator_body* yalnızca noktalı virgül içerir. Diğer işleçler için *operator_body* blok gövdesi ya da bir deyim gövdesi.

Tüm işleci bildirimi aşağıdaki kurallar geçerlidir:

*  İşleç bildirimi hem de içermelidir bir `public` ve `static` değiştiricisi.
*  Parametre bir işlecin değeri parametreler olmalıdır ([değer parametreleri](variables.md#value-parameters)). Bir derleme zamanı hata belirtmek bir işleç bildirimi için `ref` veya `out` parametreleri.
*  Bir işleç imzası ([birli işleçler](classes.md#unary-operators), [ikili işleçler](classes.md#binary-operators), [dönüştürme işleçleri](classes.md#conversion-operators)) bildirilen tüm diğer işleçlerin imzalarını farklı olmalıdır aynı sınıf.
*  Bir operatörü bildiriminde başvurulan tüm türleri en azından operatör olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).
*  Aynı değiştiricisi bir operatörü bildiriminde birden çok kez görünmesi için bir hatadır.

Her işleç kategorisi, aşağıdaki bölümlerde açıklandığı gibi ek kısıtlamalar getirir.

Diğer üyeler gibi bir temel sınıfta bildirilen işleçleri türetilmiş sınıflar tarafından devralınır. İşleç bildirimi sınıfın veya yapının işlecin işleci imzasında katılmak bildirildiği her zaman gerekli olduğundan, bir temel sınıfta bildirilen bir işleç gizlemek türetilen bir sınıfta bildirilen bir işleç için mümkün değildir. Bu nedenle, `new` değiştiricisi hiçbir zaman gerekli ve bu nedenle hiçbir zaman, bir işleç bildiriminde izin verilir.

Tekli veya ikili işleçler hakkında daha fazla bilgi bulunabilir [işleçleri](expressions.md#operators).

Dönüştürme işleçleri hakkında daha fazla bilgi bulunabilir [kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions).

### <a name="unary-operators"></a>Birli işleçler

Birli işleç bildirimi, aşağıdaki kurallar geçerli olduğu `T` sınıfın veya yapının işleç bildirimi içeren örnek türünü gösterir:

*  Bir birli `+`, `-`, `!`, veya `~` işleci türünde tek bir parametre gerçekleştirmeniz gereken `T` veya `T?` ve her türlü döndürebilir.
*  Bir birli `++` veya `--` işleci türünde tek bir parametre gerçekleştirmeniz gereken `T` veya `T?` ve aynı türü veya tür bundan türetilmiş döndürmesi gerekir.
*  Bir birli `true` veya `false` işleci türünde tek bir parametre gerçekleştirmeniz gereken `T` veya `T?` ve türü döndürmelidir `bool`.

İmza birli işleç işleç belirtecini oluşur (`+`, `-`, `!`, `~`, `++`, `--`, `true`, veya `false`) ve tek bir biçimsel parametrenin türü. Dönüş türü bir birli işlecin imzasının parçası değil veya biçimsel parametrenin adı değil.

`true` Ve `false` birli işleçler pair-wise bildirimi gerektirir. Bir sınıf diğerini de bildirmeden bu işleçlerden birini bildiriyorsa, bir derleme zamanı hatası oluşur. `true` Ve `false` işleçler şunlardır: daha ayrıntılı açıklanır [kullanıcı tanımlı koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators) ve [Boolean ifadeler](expressions.md#boolean-expressions).

Aşağıdaki örnek, bir uygulama ve sonraki kullanımını gösterir. `operator ++` bir tamsayı vektörü sınıfı için:
```csharp
public class IntVector
{
    public IntVector(int length) {...}

    public int Length {...}                 // read-only property

    public int this[int index] {...}        // read-write indexer

    public static IntVector operator ++(IntVector iv) {
        IntVector temp = new IntVector(iv.Length);
        for (int i = 0; i < iv.Length; i++)
            temp[i] = iv[i] + 1;
        return temp;
    }
}

class Test
{
    static void Main() {
        IntVector iv1 = new IntVector(4);    // vector of 4 x 0
        IntVector iv2;

        iv2 = iv1++;    // iv2 contains 4 x 0, iv1 contains 4 x 1
        iv2 = ++iv1;    // iv2 contains 4 x 2, iv1 contains 4 x 2
    }
}
```

Ne işleci yöntemi yalnızca sonek artırma gibi işlenenin 1 ekleyerek üretilen değerini döndürür unutmayın ve azaltma işleçleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve ön ek arttırma ve azaltma işleçler ([önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)). Farklı C++'da, bu yöntem, işleneninin değeri doğrudan değişiklik. Aslında, işlenenin değerinin değiştirme sonek artırma işlecini standart semantiği ihlal ediyor.

### <a name="binary-operators"></a>İkili işleçler

İkili işleç bildirimi, aşağıdaki kurallar geçerli olduğu `T` sınıfın veya yapının işleç bildirimi içeren örnek türünü gösterir:

*  Bir ikili olmayan kaydırma işleci, en az biri türünde olmalıdır, iki parametre almalıdır `T` veya `T?`ve her türlü döndürebilir.
*  Bir ikili `<<` veya `>>` işleci iki parametre ilki türünde olmalıdır, gerçekleştirmeniz gereken `T` veya `T?` ve ikinci türüne sahip olmalıdır `int` veya `int?`ve her türlü döndürebilir.

İkili işleç imzası işleç belirteci oluşur (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`, veya `<=`) ve iki biçimsel parametre türleri. Dönüş türünü ve biçimsel parametrelerinin adları bir ikili işlecin imzasının parçası değildir.

Bazı ikili işleçlerin pair-wise bildirimi gerektirir. Her bir çifti ya da işleç bildirimi için eşleşen bir bildirim çiftinin başka bir işleç olmalıdır. Aynı dönüş türü ve her parametre için aynı türe sahip olduğunda iki işleç bildirimi eşleştirin. Aşağıdaki işleçleri pair-wise bildirimi gerektirir:

*  `operator ==` ve `operator !=`
*  `operator >` ve `operator <`
*  `operator >=` ve `operator <=`

### <a name="conversion-operators"></a>Dönüştürme işleçleri

Bir dönüştürme işleci bildirimi sunar bir ***kullanıcı tanımlı dönüştürme*** ([kullanıcı tanımlı dönüşümler](conversions.md#user-defined-conversions)) önceden tanımlı örtük ve açık dönüştürmeler artırmaktadır.

İçeren bir dönüştürme işleci bildirimi `implicit` anahtar sözcüğü, bir kullanıcı tanımlı bir örtük dönüştürme tanıtır. Örtük dönüştürmeleri durumlarda, işlev üyesi çağrılarını cast ifadeleri ve atamaları da dahil olmak üzere çeşitli içinde ortaya çıkabilir. Daha ayrıntılı açıklanır budur [örtük dönüştürmelerin](conversions.md#implicit-conversions).

İçeren bir dönüştürme işleci bildirimi `explicit` anahtar sözcüğü, bir kullanıcı tanımlı açık dönüştürme tanıtır. Açık dönüştürmeler cast ifadeleri oluşabilir ve daha ayrıntılı açıklanır [açık dönüştürmeler](conversions.md#explicit-conversions).

Bir kaynak türünden dönüştürme işlecinin dönüş türü tarafından belirtilen bir hedef türüne dönüştürme işlecinin parametre türü tarafından belirtilen bir dönüştürme operatörünün dönüştürür.

Belirtilen kaynak türü için `S` ve hedef türü `T`, `S` veya `T` boş değer atanabilir türler izin `S0` ve `T0` Aksi takdirde, temel alınan türler için başvuru `S0` ve `T0` olan eşit `S` ve `T` sırasıyla. Bir kaynak türünden dönüştürme bildirmek için bir sınıf veya yapı izin `S` hedef türüyle `T` yalnızca aşağıdakilerin tümü doğru olduğunda:

*  `S0` ve `T0` farklı tür aşağıda verilmiştir.
*  Ya da `S0` veya `T0` işleç bildirimi yer alan sınıf veya yapı türü.
*  Ne `S0` ya da `T0` olduğu bir *INTERFACE_TYPE*.
*  Kullanıcı tanımlı dönüşümler, bir dönüştürme yok gelen `S` için `T` veya `T` için `S`.

Bu kurallar amaçları için ilişkili parametreler girin `S` veya `T` diğer tür devralma ilişkisi yok ve bu tür parametreleri yok sayılır tüm kısıtlamalar benzersiz tür olarak değerlendirilir.

Örnekte
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
ilk iki işleç bildirimleri için izin verilen amacıyla [dizin oluşturucular](classes.md#indexers).3, `T` ve `int` ve `string` sırasıyla ile hiçbir ilişkisi benzersiz tür olarak kabul edilir. Ancak, üçüncü bir hata nedeniyle işlecidir `C<T>` taban sınıfıdır `D<T>`.

İçin veya işlecin bildirildiği sınıf veya yapı türü, takip eden ikinci kuralından bir dönüştürme operatörünün dönüştürmeniz gerekir. Örneğin, bir sınıf veya yapı türü için olası `C` dönüştürme tanımlamak için `C` için `int` ve `int` için `C`, ancak `int` için `bool`.

Önceden tanımlı bir dönüştürme doğrudan tanımlanacak mümkün değildir. Bu nedenle, dönüştürme işleçleri veya dönüştürmeye izin verilmez `object` arasında örtük ve açık dönüştürmeler olduğundan `object` ve diğer tüm türleri. Benzer şekilde, kaynak ya da hedef tür bir dönüştürme dönüştürme ardından zaten var olduğundan, diğer bir taban türü olabilir.

Ancak, belirttiğiniz, belirli tür bağımsız değişkenleri, zaten dönüştürmeleri önceden tanımlı dönüştürmeler genel türlerde işleçleri bildirmek mümkündür. Örnekte
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
yazdığınızda `object` bir tür bağımsız değişkeni olarak belirtilen `T`, zaten bir dönüştürme ikinci işleç bildirir (örtük ve bu nedenle ayrıca açık, her tür için türünden dönüştürme var `object`).

Önceden tanımlı bir dönüştürme iki tür arasında mevcut olduğu durumlarda, tüm bu türler arasında kullanıcı tanımlı dönüştürme göz ardı edilir. Özellikle:

*  Önceden tanımlanmış bir örtük dönüştürme varsa ([örtük dönüştürmelerin](conversions.md#implicit-conversions)) türüne `S` türüne `T`, tüm kullanıcı tanımlı Dönüşümler (örtük veya açık) öğesinden `S` için `T` göz ardı edilir.
*  Önceden tanımlı açık bir dönüştürme ise ([açık dönüştürmeler](conversions.md#explicit-conversions)) türüne `S` türüne `T`, kullanıcı tanımlı hiçbir açık dönüşümlerse `S` için `T` göz ardı edilir. Ayrıca:

Varsa `T` bir arabirim türüne örtük dönüşümlere kullanıcı tanımlı olan `S` için `T` göz ardı edilir.

Aksi takdirde, kullanıcı tanımlı örtük dönüştürmelerine `S` için `T` olarak kabul edilir.

Tüm türleri ancak `object`, işleçler tarafından bildirilen `Convertible<T>` türü yukarıdaki önceden tanımlı dönüştürmeler çakışma yok. Örneğin:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Ancak, türünün `object`, önceden tanımlı dönüştürmeler tüm durumlarda ancak bir kullanıcı tanımlı dönüşümler Gizle:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Kullanıcı tanımlı Dönüştürmelere izin veya dönüştürülecek verilmez *INTERFACE_TYPE*s. Özellikle, bu kısıtlama hiçbir kullanıcı tanımlı dönüştürme dönüştürülürken gerçekleşmesini sağlar. bir *INTERFACE_TYPE*ve dönüştürme bir *INTERFACE_TYPE* yalnızca başarılı nesnesi Dönüştürülen gerçekten uygulayan belirtilen *INTERFACE_TYPE*.

Kaynak türü ve hedef türü bir dönüştürme operatörünün imzası oluşur. (Bu yalnızca formun kendisi için dönüş türü imzada katıldığı üyesi olduğunu unutmayın.) `implicit` Veya `explicit` sınıflandırma dönüştürme operatörü işlecin imzasının parçası değil. Bu nedenle, bir sınıf veya yapı hem bildirimini yapamazsınız bir `implicit` ve `explicit` dönüştürme işleci ile aynı kaynak ve hedef türleri.

Genel olarak, kullanıcı tanımlı örtük dönüştürmeler, hiçbir zaman özel durumlar ve hiçbir zaman bilgi kaybetmek tasarlanmalıdır. Bir kullanıcı tanımlı dönüştürme (örneğin, kaynak bağımsız değişkeni aralık dışında olduğundan) özel durumları ortaya çıkmasına neden verebilir veya kaybı bilgi (örneğin, yüksek sıra bitleri atılıyor) ve ardından bu dönüştürme açık bir dönüştürme tanımlanması gerekir

Örnekte
```csharp
using System;

public struct Digit
{
    byte value;

    public Digit(byte value) {
        if (value < 0 || value > 9) throw new ArgumentException();
        this.value = value;
    }

    public static implicit operator byte(Digit d) {
        return d.value;
    }

    public static explicit operator Digit(byte b) {
        return new Digit(b);
    }
}
```
Dönüştürme `Digit` için `byte` hiçbir zaman istisnalar fırlatıyorsa veya bilgiler, ancak bu dönüştürme kaybeder örtük olarak `byte` için `Digit` beri açıktır `Digit` yalnızca bir alt kümesini olası temsil edebilir değerleri bir `byte`.

## <a name="instance-constructors"></a>Örnek oluşturucuları

Bir ***örnek oluşturucusu*** uygulayan bir sınıf örneği başlatmak için gerekli eylemleri bir üyesidir. Örnek oluşturucuları kullanarak bildirilir *constructor_declaration*: %s

```antlr
constructor_declaration
    : attributes? constructor_modifier* constructor_declarator constructor_body
    ;

constructor_modifier
    : 'public'
    | 'protected'
    | 'internal'
    | 'private'
    | 'extern'
    | constructor_modifier_unsafe
    ;

constructor_declarator
    : identifier '(' formal_parameter_list? ')' constructor_initializer?
    ;

constructor_initializer
    : ':' 'base' '(' argument_list? ')'
    | ':' 'this' '(' argument_list? ')'
    ;

constructor_body
    : block
    | ';'
    ;
```

A *constructor_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)), geçerli bir bileşimin dört erişim değiştiricileri ([erişim değiştiricilerine ](classes.md#access-modifiers)) ve bir `extern` ([dış yöntemleri](classes.md#external-methods)) değiştiricisi. Birden çok kez aynı değiştiricisi içerecek şekilde bir oluşturucu bildirimi izin verilmez.

*Tanımlayıcı* , bir *constructor_declarator* örnek oluşturucusu bildirilmiş sınıfı adı olmalıdır. Herhangi bir ad belirtilmezse, bir derleme zamanı hatası oluşur.

İsteğe bağlı *formal_parameter_list* örneği aynı kurallara tabi oluşturucudur *formal_parameter_list* bir yöntemin ([yöntemleri](classes.md#methods)). İmza biçimsel parametre listesi tanımlar ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir örnek oluşturucusu ve işlem yapabildiği yöneten aşırı yükleme çözümlemesi ([anlam çıkarma](expressions.md#type-inference)) belirli bir seçer bir çağrı örnek oluşturucusu.

Başvuru türlerinin her biri *formal_parameter_list* örneğini Oluşturucusu en az Oluşturucu olarak olarak erişilebilir olmalıdır ([erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)).

İsteğe bağlı *constructor_initializer* verilen deyimleri yürütmeden önce çağrılacak başka bir örnek oluşturucusu belirtir *constructor_body* , bu örnek oluşturucusu. Daha ayrıntılı açıklanır budur [Oluşturucu başlatıcıları](classes.md#constructor-initializers).

Ne zaman bir oluşturucu bildirimi içeren bir `extern` değiştiricisi, kurucusu olduğu söylenir bir ***dış Oluşturucu***. Bir dış Oluşturucu bildirimi yok kullanımınız sağladığından, *constructor_body* noktalı virgül içerir. Diğer tüm oluşturucular için *constructor_body* oluşan bir *blok* sınıfının yeni bir örneğini başlatmak için belirtir. Bu tam olarak karşılık geldiğinden *blok* ile bir örnek yöntemi, bir `void` dönüş türü ([yöntem gövdesini](classes.md#method-body)).

Örnek oluşturucuları devralınmaz. Bu nedenle, bir sınıf gerçekten sınıfta bildirilen dışındaki örnek oluşturucusu yok. Bir sınıf örneği hiçbir oluşturucu bildirimleri içeriyorsa, varsayılan örnek oluşturucusu otomatik olarak sağlanır ([varsayılan oluşturucular](classes.md#default-constructors)).

Örnek Oluşturucuları tarafından çağrılır *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) ve ile *constructor_initializer*s.

### <a name="constructor-initializers"></a>Oluşturucu başlatıcıları

Tüm oluşturucular örnek (sınıfı için hariç `object`) örtük olarak başka bir örnek oluşturucusu çağrısından hemen önce dahil *constructor_body*. Örtük olarak çağrılacak oluşturucunun belirlenir *constructor_initializer*:

*  Form örneği oluşturucu başlatıcı `base(argument_list)` veya `base()` çağrılacak doğrudan temel sınıftan bir örnek oluşturucusunda neden olur. Bu oluşturucu kullanılarak seçilen *argument_list* , mevcut ve aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution). Tüm erişilebilir örnek oluşturucular doğrudan temel sınıfta yer alan veya varsayılan oluşturucu aday örnek oluşturucuları kümesini oluşur ([varsayılan oluşturucular](classes.md#default-constructors)), hiçbir örnek oluşturucuları bildirilmiş olan doğrudan temel sınıf. Bu kümenin boş olup olmadığını veya tek bir en iyi örnek oluşturucusu tanımladıysanız, bir derleme zamanı hatası oluşur.
*  Form örneği oluşturucu başlatıcı `this(argument-list)` veya `this()` sınıfın kendisi çağrılacak bir örnek oluşturucusunda neden olur. Oluşturucu kullanılarak seçilen *argument_list* , mevcut ve aşırı yükleme çözünürlüğü kurallarına [aşırı yükleme çözünürlüğü](expressions.md#overload-resolution). Örnek oluşturucuları aday kümesini sınıfta bildirilen tüm erişilebilir örnek oluşturucular oluşur. Bu kümenin boş olup olmadığını veya tek bir en iyi örnek oluşturucusu tanımladıysanız, bir derleme zamanı hatası oluşur. Bir kopya Oluşturucusu bildirimi Oluşturucusu kendisini çağıran bir oluşturucu başlatıcı içeriyorsa, bir derleme zamanı hatası oluşur.

Bir örnek oluşturucusunda bir oluşturucu başlatıcı formun hiçbir oluşturucu başlatıcı varsa `base()` örtük olarak sağlanır. Bu nedenle, bir kopya Oluşturucusu bildirimi form
```csharp
C(...) {...}
```
tam olarak eşdeğerdir
```csharp
C(...): base() {...}
```

Tarafından belirtilen parametrelerle kapsamını *formal_parameter_list* örnek oluşturucusu bu bildirimin oluşturucu başlatıcı bildirimi içerir. Bu nedenle, bir oluşturucu başlatıcı oluşturucunun parametreler erişim izin verilir. Örneğin:
```csharp
class A
{
    public A(int x, int y) {}
}

class B: A
{
    public B(int x, int y): base(x + y, x - y) {}
}
```

Bir örneği oluşturucu başlatıcı oluşturulan örnek erişemez. Bu nedenle, başvuru bir derleme zamanı hatasıdır `this` oluşturucu başlatıcı bir bağımsız değişken ifadede gibidir, bir derleme zamanı hatası aracılığıyla herhangi bir örnek üyesi başvurmak bir bağımsız değişken ifadesi için bir *simple_name*.

### <a name="instance-variable-initializers"></a>Örnek değişken başlatıcıları

Ne zaman bir örnek oluşturucusunda oluşturucu başlatıcı içermiyor veya form Oluşturucu başlatıcısına sahip `base(...)`, o Oluşturucu örtük olarak belirtilen başlatmalar gerçekleştirir *variable_initializer*, s örnek alanları, sınıfta bildirilen. Bu bir dizi hemen sonra Anonime oluşturucusuna ve doğrudan temel sınıf oluşturucu örtük çağırmadan önce yürütülür atamaları karşılık gelir. Değişken başlatıcılar, sınıf bildirimi içinde göründükleri metinsel sırayla yürütülür.

### <a name="constructor-execution"></a>Oluşturucusu yürütme

Değişken başlatıcılar atama deyimleri dönüştürülür ve bu atama deyimleri, temel sınıf örneği oluşturucunun çağrılacağını önce yürütülür. Bu sıralama, söz konusu örneğine erişimi olan herhangi bir deyim yürütülmeden önce tüm örnek alanları, değişken başlatıcılar tarafından başlatılır sağlar.

Verilen örnek
```csharp
using System;

class A
{
    public A() {
        PrintFields();
    }

    public virtual void PrintFields() {}
}

class B: A
{
    int x = 1;
    int y;

    public B() {
        y = -1;
    }

    public override void PrintFields() {
        Console.WriteLine("x = {0}, y = {1}", x, y);
    }
}
```
Zaman `new B()` bir örneğini oluşturmak için kullanılan `B`, aşağıdaki çıkış üretilir:
```
x = 1, y = 0
```

Değerini `x` temel sınıfının örnek oluşturucusu çağırılmadan önce değişken başlatıcı çalıştırıldığı için 1'dir. Ancak, değerini `y` 0'dır (varsayılan değer olan bir `int`) çünkü atamaya `y` temel sınıf oluşturucusunu döndürdükten sonra kadar yürütülmez.

Örnek değişken başlatıcılar ve oluşturucu başlatıcıları önce otomatik olarak eklenen deyimleri düşünmek faydalıdır *constructor_body*. Örnek
```csharp
using System;
using System.Collections;

class A
{
    int x = 1, y = -1, count;

    public A() {
        count = 0;
    }

    public A(int n) {
        count = n;
    }
}

class B: A
{
    double sqrt2 = Math.Sqrt(2.0);
    ArrayList items = new ArrayList(100);
    int max;

    public B(): this(100) {
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        max = n;
    }
}
```
birden fazla değişken başlatıcılar içerir. Ayrıca, her iki Oluşturucu başlatıcıları içerir (`base` ve `this`). Örnek aşağıda gösterilen koda karşılık gelen burada açıklamaları (otomatik olarak eklenen Oluşturucu çağrıları için kullanılan sözdizimi geçerli değil, ancak yalnızca mekanizması göstermek için kullanılır) otomatik olarak eklenen bir ifade belirtir.

```csharp
using System.Collections;

class A
{
    int x, y, count;

    public A() {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = 0;
    }

    public A(int n) {
        x = 1;                       // Variable initializer
        y = -1;                      // Variable initializer
        object();                    // Invoke object() constructor
        count = n;
    }
}

class B: A
{
    double sqrt2;
    ArrayList items;
    int max;

    public B(): this(100) {
        B(100);                      // Invoke B(int) constructor
        items.Add("default");
    }

    public B(int n): base(n - 1) {
        sqrt2 = Math.Sqrt(2.0);      // Variable initializer
        items = new ArrayList(100);  // Variable initializer
        A(n - 1);                    // Invoke A(int) constructor
        max = n;
    }
}
```

### <a name="default-constructors"></a>Varsayılan Oluşturucu

Bir sınıf örneği hiçbir oluşturucu bildirimleri içeriyorsa, varsayılan örnek oluşturucusu otomatik olarak sağlanır. Bu varsayılan oluşturucu yalnızca doğrudan temel sınıf parametresiz oluşturucu çağırır. Ardından sınıf soyuttur varsayılan oluşturucusu için bildirilen erişilebilirliği korunmaz. Aksi takdirde, varsayılan oluşturucusu için bildirilen erişilebilirliği herkes tarafından kullanılabilir. Bu nedenle, varsayılan oluşturucu her zaman biçimindedir

```csharp
protected C(): base() {}
```
veya
```csharp
public C(): base() {}
```
Burada `C` sınıf adıdır. Ardından aşırı yükleme çözünürlüğü için temel sınıf oluşturucu başlatıcı benzersiz bir en iyi aday belirlenemiyor ise bir derleme zamanı hatası oluşur.

Örnekte
```csharp
class Message
{
    object sender;
    string text;
}
```
hiçbir örnek oluşturucusu bildirimleri sınıfı içerdiği için varsayılan bir oluşturucu sağlanır. Bu nedenle, örneğin tam olarak eşdeğerdir
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Özel oluşturucular

Bir sınıf, `T` öğesinin program metni dışında sınıflar için olası değil, yalnızca özel örnek oluşturucuları bildirir `T` türetmek için `T` veya doğrudan örneklerini oluşturmak için `T`. Bir sınıf yalnızca statik üyeleri içerir ve örneği için tasarlanmamıştır, bu nedenle, boş özel örnek oluşturucusu ekleme örneğini oluşturmada engelleyecek. Örneğin:
```csharp
public class Trig
{
    private Trig() {}        // Prevent instantiation

    public const double PI = 3.14159265358979323846;

    public static double Sin(double x) {...}
    public static double Cos(double x) {...}
    public static double Tan(double x) {...}
}
```

`Trig` Sınıfı için ilgili yöntemler ve sabitler gruplarını, ancak bu örneği için tasarlanmamıştır. Bu nedenle tek boş özel örnek oluşturucusu bildirir. Varsayılan Oluşturucu otomatik olarak oluşturulmasını engellemek için en az bir örnek oluşturucusu bildirilmesi gerekir.

### <a name="optional-instance-constructor-parameters"></a>İsteğe bağlı örnek oluşturucusu parametreleri

`this(...)` Oluşturucu başlatıcı biçimi yaygın olarak aşırı yüklemesi ile birlikte isteğe bağlı örnek oluşturucusu parametreleri uygulamak için kullanılır. Örnekte
```csharp
class Text
{
    public Text(): this(0, 0, null) {}

    public Text(int x, int y): this(x, y, null) {}

    public Text(int x, int y, string s) {
        // Actual constructor implementation
    }
}
```
ilk iki örnek oluşturucuları, yalnızca varsayılan değerleri eksik bağımsız değişkenleri sağlayın. İkisi de bir `this(...)` gerçekten çalışır yeni örneği başlatılıyor, üçüncü örnek oluşturucusu çağırmak için oluşturucu başlatıcı. Etkisi isteğe bağlı Oluşturucu parametrelerinin olan:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Statik oluşturucular

A ***statik Oluşturucu*** kapalı sınıf türünü başlatmak için gerekli eylemleri uygulayan bir üyesidir. Statik oluşturucular kullanarak bildirilir *static_constructor_declaration*: %s

```antlr
static_constructor_declaration
    : attributes? static_constructor_modifiers identifier '(' ')' static_constructor_body
    ;

static_constructor_modifiers
    : 'extern'? 'static'
    | 'static' 'extern'?
    | static_constructor_modifiers_unsafe
    ;

static_constructor_body
    : block
    | ';'
    ;
```

A *static_constructor_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)) ve bir `extern` değiştirici ([dışyöntemleri](classes.md#external-methods)).

*Tanımlayıcı* , bir *static_constructor_declaration* statik Oluşturucu bildirilmiş sınıfı adı olmalıdır. Herhangi bir ad belirtilmezse, bir derleme zamanı hatası oluşur.

Ne zaman bir statik Oluşturucu bildirimi içeren bir `extern` değiştiricisi, statik oluşturucunun olduğu söylenir bir ***dış statik Oluşturucu***. Bir dış statik Oluşturucu bildirimi yok kullanımınız sağladığından, *static_constructor_body* noktalı virgül içerir. Diğer statik Oluşturucu bildirimleri *static_constructor_body* oluşan bir *blok* sınıf başlatma çalıştırılacak deyimleri belirtir. Bu tam olarak karşılık geldiğinden *method_body* ile statik bir yöntemin bir `void` dönüş türü ([yöntem gövdesini](classes.md#method-body)).

Statik oluşturucular değil devralınır ve doğrudan çağrılamaz.

Statik Oluşturucu bir kapalı sınıf türü için en fazla bir kez bir verilen uygulama etki alanında yürütür. Statik Oluşturucu yürütülmesi, bir uygulama etki alanı içinde gerçekleşmesi için ilk aşağıdaki olaylardan biri tarafından tetiklenir:

*  Sınıf türünün bir örneği oluşturulur.
*  Statik üyeler sınıf türünden birini başvurulur.

Bir sınıf içeriyorsa `Main` yöntemi ([uygulama başlatma](basic-concepts.md#application-startup)) hangi yürütmeyi başlatır, statik Oluşturucu için önce o sınıfın yürütür, `Main` yöntemi çağrılır.

Yeni bir kapalı sınıf türü statik alanları yeni bir dizi ilk kez başlatmak için ([statik ve örnek alanları](classes.md#static-and-instance-fields)), belirli kapalı bir tür oluşturulur. Statik alanların her biri varsayılan değer olarak başlatılır ([varsayılan değerler](variables.md#default-values)). Ardından, statik alan başlatıcıları ([statik alan başlatma](classes.md#static-field-initialization)) statik alanlara yürütülür. Son olarak, statik oluşturucunun yürütülür.

Örnek
```csharp
using System;

class Test
{
    static void Main() {
        A.F();
        B.F();
    }
}

class A
{
    static A() {
        Console.WriteLine("Init A");
    }
    public static void F() {
        Console.WriteLine("A.F");
    }
}

class B
{
    static B() {
        Console.WriteLine("Init B");
    }
    public static void F() {
        Console.WriteLine("B.F");
    }
}
```
Çıkış üretmesi gerekir:
```
Init A
A.F
Init B
B.F
```
çünkü yürütülmesini `A`'s statik oluşturucu çağrısı tarafından tetiklendiğini `A.F`ve yürütülmesini `B`'s statik oluşturucu çağrısı tarafından tetiklenir `B.F`.

Varsayılan değer durumlarını gözlenecek değişken başlatıcıları olan statik alanlar izin döngüsel bağımlılıklar oluşturmak mümkündür.

Örnek
```csharp
using System;

class A
{
    public static int X;

    static A() {
        X = B.Y + 1;
    }
}

class B
{
    public static int Y = A.X + 1;

    static B() {}

    static void Main() {
        Console.WriteLine("X = {0}, Y = {1}", A.X, B.Y);
    }
}
```
çıktıyı üretir
```
X = 1, Y = 2
```

Yürütülecek `Main` yöntemi, sistemin ilk için Başlatıcı çalıştırır `B.Y`, sınıfa önceki `B`'s statik Oluşturucu. `Y`ın Başlatıcı neden olur `A`ait olduğundan çalıştırılacak statik Oluşturucu değerini `A.X` başvuruluyor. Statik oluşturucusunun `A` değerini hesaplamak için sırayla geçer `X`ve bunu öğesinden varsayılan değerini `Y`, sıfır olan. `A.X` Bu nedenle, 1 olarak başlatılır. Çalışan işlemi `A`ilk değerini hesaplaması için döndüren tamamlandıktan sonra statik alan başlatıcıları ve statik Oluşturucu `Y`, 2 sonucunu olur.

Statik Oluşturucu her kapalı oluşturulan sınıf türü için tam bir kez yürütülür çünkü bu iade edilemez kısıtlamaları aracılığıyla derleme zamanında tür parametresi üzerinde çalışma zamanı denetimleri zorunlu kılmak için kullanışlı bir yerdir ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)). Örneğin, şu tür, tür bağımsız değişkeni bir sabit listesi olduğunu zorlamak için bir statik Oluşturucu kullanır:
```csharp
class Gen<T> where T: struct
{
    static Gen() {
        if (!typeof(T).IsEnum) {
            throw new ArgumentException("T must be an enum");
        }
    }
}
```

## <a name="destructors"></a>Yıkıcılar

A ***yok Edicisi*** uygulayan bir sınıf örneği yok etmek için gereken eylemler bir üyesidir. Bir yok edici kullanılarak bildirilen bir *destructor_declaration*:

```antlr
destructor_declaration
    : attributes? 'extern'? '~' identifier '(' ')' destructor_body
    | destructor_declaration_unsafe
    ;

destructor_body
    : block
    | ';'
    ;
```

A *destructor_declaration* bir dizi içerebilir *öznitelikleri* ([öznitelikleri](attributes.md)).

*Tanımlayıcı* , bir *destructor_declaration* yok edici bildirilmiş sınıfı adı olmalıdır. Herhangi bir ad belirtilmezse, bir derleme zamanı hatası oluşur.

Ne zaman bir yok edici bildirimi içeren bir `extern` değiştiricisi, yok edici olduğu söylenir bir ***dış yok Edicisi***. Bir dış yok Edicisi bildirimi yok kullanımınız sağladığından, *destructor_body* noktalı virgül içerir. Diğer yıkıcı için *destructor_body* oluşan bir *blok* sınıfının bir örneğini yok etmek çalıştırılacak deyimleri belirtir. A *destructor_body* tam olarak karşılık geldiğinden *method_body* ile bir örnek yöntemi, bir `void` dönüş türü ([yöntem gövdesini](classes.md#method-body)).

Yok ediciler devralınmaz. Bu nedenle, bir sınıfın yok edicileri bu sınıfta bildirilen farklı vardır.

Hiçbir parametrelere sahip olacak şekilde bir yıkıcı gerekli olduğundan, bir sınıf, en fazla bir yıkıcı çalışabilmeniz için aşırı yüklenmiş, olamaz.

Yıkıcılar, otomatik olarak çağrılır ve açıkça çağrılamaz. Artık bu örneği kullanmak herhangi bir kod için mümkün olduğunda örneği yok etme için uygun hale gelir. Örnek yok Edicisi yürütülmesini örneği yok etme için uygun hale geldikten sonra herhangi bir zamanda meydana gelebilir. Bir örneği imha edildiğinde yok ediciler o örneğin devralma zincirinde, sırayla çağrılır için türetilmiş az türetilmiş en iyi. Bir yok edici herhangi bir iş parçacığı üzerinde çalıştırılabilir. Daha ayrıntılı bir açıklaması ne zaman ve yok edici nasıl yürütüleceğini yöneten kurallar için [otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management).

Örnek çıktısı
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("A's destructor");
    }
}

class B: A
{
    ~B() {
        Console.WriteLine("B's destructor");
    }
}

class Test
{
   static void Main() {
        B b = new B();
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
   }
}
```
is
```
B's destructor
A's destructor
```
yok ediciler bir devralma zincirindeki sırayla çağrılır beri için türetilmiş az türetilmiş en iyi.

Yıkıcılar, sanal yöntemini geçersiz kılarak uygulanır `Finalize` üzerinde `System.Object`. C# programları (ya da geçersiz kılmalarına) çağırın ya da bu yöntemi yok sayın izin verilmez doğrudan. Örneğin, program
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
iki hatalar içeriyor.

Derleyici bu yöntem ve geçersiz kılmalar, tüm yok gibi davranır. Bu nedenle, bu program:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
geçerli olduğunu ve yöntemini gizliyor gösterilen `System.Object`'s `Finalize` yöntemi.

Bir yok ediciden bir özel durum oluştuğunda, davranış hakkında ayrıntılı bilgi için bkz. [özel durumların nasıl işleneceğini](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Yineleyiciler

İşlev üyesi ([işlev üyeleri](expressions.md#function-members)) yineleyici bloğu kullanılarak uygulanan ([blokları](statements.md#blocks)) olarak adlandırılır bir ***yineleyici***.

Dönüş türü karşılık gelen işlevi üyesinin Numaralandırıcı arabirimleri biri olduğu sürece, bir yineleyici bloğu işlevi üyesinin gövdesi olarak kullanılabilir ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) veya numaralandırılabilir arabirimler ([Numaralandırılabilir arabirimler](classes.md#enumerable-interfaces)). Olarak gerçekleşebilir bir *method_body*, *operator_body* veya *accessor_body*bilgileriyse olayları, örnek oluşturucuları, statik oluşturucular ve Yıkıcılar olamaz yineleyiciler uygulanır.

Yineleyici bloğu kullanarak bir işlev üyesi uygulandığında belirtmek için işlev üyesi biçimsel parametre listesi için bir derleme zamanı hatası olduğu `ref` veya `out` parametreleri.

### <a name="enumerator-interfaces"></a>Numaralandırıcı arabirimleri

***Numaralandırıcı arabirimleri*** genel olmayan arabirimi `System.Collections.IEnumerator` ve tüm genel arabirim örneklemeleri `System.Collections.Generic.IEnumerator<T>`. Konuyu uzatmamak amacıyla, bu bölümde bu arabirimleri olarak başvurulan `IEnumerator` ve `IEnumerator<T>`sırasıyla.

### <a name="enumerable-interfaces"></a>Numaralandırılabilir arabirimler

***Numaralandırılabilir arabirimler*** genel olmayan arabirimi `System.Collections.IEnumerable` ve tüm genel arabirim örneklemeleri `System.Collections.Generic.IEnumerable<T>`. Konuyu uzatmamak amacıyla, bu bölümde bu arabirimleri olarak başvurulan `IEnumerable` ve `IEnumerable<T>`sırasıyla.

### <a name="yield-type"></a>Yield türü

Tümü aynı türdeki değerler bir yineleyici oluşturur. Bu tür adında ***yield türü*** yineleyici.

*  Yield türü döndüren bir yineleyicinin `IEnumerator` veya `IEnumerable` olduğu `object`.
*  Yield türü döndüren bir yineleyicinin `IEnumerator<T>` veya `IEnumerable<T>` olduğu `T`.

### <a name="enumerator-objects"></a>Numaralandırıcı nesne

Yineleyici bloğu kullanarak bir numaralandırıcı arabirim türü döndüren bir işlev üyesi uygulandığında işlevi üye çağırma hemen kod yineleyici bloğunda yürütmez. Bunun yerine, bir ***Numaralandırıcı nesnesi*** oluşturulan ve döndürdü. Bu nesne yineleyici bloğunda belirtilen kodu kapsüller ve yineleyici bloğu içindeki kod yürütülmesi gerçekleşir, numaralandırıcı nesnenin `MoveNext` yöntemi çağrılır. Numaralandırıcı nesnesi, aşağıdaki özelliklere sahiptir:

*  Bunu uygulayan `IEnumerator` ve `IEnumerator<T>`burada `T` yield yineleyici türüdür.
*  Bunu uygulayan `System.IDisposable`.
*  Bağımsız değişken değerlerini bir kopyasını (varsa) başlatılır ve işlev üyesine geçirilen örnek değer.
*  Olası durumlar dört sahip ***önce***, ***çalıştıran***, ***askıya***, ve ***sonra***ve başlangıçta ***önce***  durumu.

Numaralandırıcı nesnesi genellikle yineleyici bloğu içindeki kod kapsüller ve Numaralandırıcı arabirimleri uygulayan bir numaralandırıcı derleyici tarafından oluşturulan sınıfın bir örneği olduğu halde uygulamasının diğer yöntemler kullanılabilir. Derleyici tarafından oluşturulan bir numaralandırıcı sınıfı, bu sınıfı iç içe olamaz, doğrudan veya dolaylı olarak işlevi üye içeren sınıfın özel erişilebilirlik sahip olur ve derleyici kullanılmak üzere ayrılmış bir adı olacaktır ([tanımlayıcıları ](lexical-structure.md#identifiers)).

Numaralandırıcı nesnesi yukarıda belirtilen olandan daha fazla arabirim uygulayabilir.

Aşağıdaki bölümlerde tam davranışını `MoveNext`, `Current`, ve `Dispose` üyeleri `IEnumerable` ve `IEnumerable<T>` arabirimi bir sabit listesi nesnesi tarafından sağlanan uygulamaları.

Numaralandırıcı nesne desteklemeyen unutmayın `IEnumerator.Reset` yöntemi. Bu yöntem çağırma neden olan bir `System.NotSupportedException` oluşturulması için.

#### <a name="the-movenext-method"></a>MoveNext yöntemi

`MoveNext` Yöntemi bir sabit listesi nesnesi, bir yineleyici bloğu kod kapsüller. Çağırma `MoveNext` yöntemi kümeleri ve yineleyici bloğu içinde kod yürütülmeden `Current` uygun şekilde Numaralandırıcı nesnesi bir özelliğidir. Tarafından gerçekleştirilen kesin eylemi `MoveNext` Numaralandırıcı nesnesi durumuna bağlıdır, `MoveNext` çağrılır:

*  Numaralandırıcı nesne durumunu ise ***önce***, çağrılıyor `MoveNext`:
   * Durumu ***çalıştıran***.
   * Parametreleri başlatır (dahil olmak üzere `this`) yineleyici bloğun bağımsız değişken değerlerini ve örnek değeri Numaralandırıcı nesnesi başlatıldığında kaydedildi.
   * Yürütme (aşağıda açıklandığı gibi) kesilene kadar yineleyici bloğu baştan yürütür.
*  Numaralandırıcı nesne durumunu ise ***çalıştıran***, çağırma sonucu `MoveNext` belirtilmemiş.
*  Numaralandırıcı nesne durumunu ise ***askıya***, çağrılıyor `MoveNext`:
   * Durumu ***çalıştıran***.
   * Tüm yerel değişkenleri ve parametreleri (Bu dahil) değerlerini yineleyici bloğu yürütülmesini son askıya alındığı zaman kaydedilmiş değerleri geri yükler. Bu değişkenler tarafından başvurulan tüm nesneleri içeriğini önceki MoveNext çağrısı beri değişmiş olabilir unutmayın.
   * Hemen yineleyici bloğu yürütülmesini sürdürür `yield return` (aşağıda açıklandığı gibi) yürütülmesi yarıda kadar devam eder ve yürütmeyi askıya alınması neden deyimi.
*  Numaralandırıcı nesne durumunu ise ***sonra***, çağrılıyor `MoveNext` döndürür `false`.


Zaman `MoveNext` yineleyici bloğun yürütme dört yollarla kesintiye: tarafından bir `yield return` deyimi tarafından bir `yield break` deyimi, bir özel durum ve yineleyici bloğunun sonu karşılaşıldığında tarafından oluşturulur ve tanesi yayılan yineleyici bloğu.

*  Olduğunda bir `yield return` deyimi karşılaşıldığında ([yield deyimi](statements.md#the-yield-statement)):
   * Deyiminde belirtilen ifade değerlendirilir, örtük olarak yield türüne dönüştürülerek ve atanan `Current` Numaralandırıcı nesnesi bir özelliğidir.
   * Yineleyici gövdenin yürütülmesi askıya alınır. Tüm yerel değişkenlerin ve parametrelerin değerlerini (dahil olmak üzere `this`) kaydedilir, bu konumu olarak `yield return` deyimi. Varsa `yield return` deyimi, bir veya daha fazla içinde `try` engeller, ilişkili `finally` blokları şu anda yürütülmez.
   * Numaralandırıcı nesne durumunu değiştirilecek ***askıya***.
   * `MoveNext` Yöntemi döndürür `true` yineleme sonraki değere başarıyla Gelişmiş belirten arayanına için.
*  Olduğunda bir `yield break` deyimi karşılaşıldığında ([yield deyimi](statements.md#the-yield-statement)):
   * Varsa `yield break` deyimi, bir veya daha fazla içinde `try` engeller, ilişkili `finally` bloğu yürütülür.
   * Numaralandırıcı nesne durumunu değiştirilecek ***sonra***.
   * `MoveNext` Yöntemi döndürür `false` yineleme tamamlandığını gösteren arayanına için.
*  Yineleyici gövdenin sonuna ne zaman karşılaştı:
   * Numaralandırıcı nesne durumunu değiştirilecek ***sonra***.
   * `MoveNext` Yöntemi döndürür `false` yineleme tamamlandığını gösteren arayanına için.
*  Ne zaman bir özel durum ve yineleyici bloğu dışında yayılır:
   * Uygun `finally` yineleyici gövdenin bloklarında özel durum yayma tarafından yürütülen.
   * Numaralandırıcı nesne durumunu değiştirilecek ***sonra***.
   * Özel durum yayma çağırana devam `MoveNext` yöntemi.

#### <a name="the-current-property"></a>Geçerli özelliği

Numaralandırıcı nesne `Current` özelliği tarafından etkilenir `yield return` yineleyici bloğu içindeki deyimler.

Numaralandırıcı nesnesi olduğunda ***askıya*** durumunda değerini `Current` önceki çağrı tarafından ayarlanan değer `MoveNext`. Numaralandırıcı nesnesi olduğunda ***önce***, ***çalıştıran***, veya ***sonra*** durumlarını erişme sonucunu `Current` belirtilmemiş.

Bir yield ile bir yineleyici için yazın dışında `object`, erişme sonucunu `Current` Numaralandırıcı nesnenin aracılığıyla `IEnumerable` uygulama karşılık gelen erişimi `Current` Numaralandırıcı nesnenin aracılığıyla `IEnumerator<T>` Uygulama ve sonucu atama `object`.

#### <a name="the-dispose-method"></a>Dispose yöntemi

`Dispose` Yöntemi tarafından Numaralandırıcı nesnesi getirmek yineleme temizlemek için kullanılan ***sonra*** durumu.

*  Numaralandırıcı nesne durumunu ise ***önce***, çağrılıyor `Dispose` duruma geçmesi ***sonra***.
*  Numaralandırıcı nesne durumunu ise ***çalıştıran***, çağırma sonucu `Dispose` belirtilmemiş.
*  Numaralandırıcı nesne durumunu ise ***askıya***, çağrılıyor `Dispose`:
   * Durumu ***çalıştıran***.
   * Son olarak çalıştırıldığında herhangi finally blokları yürütür `yield return` ifade edilen bir `yield break` deyimi. Bu durum ve yineleyici gövdenin dışında yayılan bir özel durum neden olursa, numaralandırıcı nesne durumunu kümesine ***sonra*** ve özel durum çağırana yayılır `Dispose` yöntemi.
   * Durumu ***sonra***.
*  Numaralandırıcı nesne durumunu ise ***sonra***, çağrılıyor `Dispose` herhangi bir etkisi yoktur.

### <a name="enumerable-objects"></a>Numaralandırılabilir nesneleri

Sıralanabilir arabirim türü döndüren bir işlev üyesi yineleyici bloğu kullanarak uygulandığında işlevi üye çağırma hemen kod yineleyici bloğunda yürütmez. Bunun yerine, bir ***numaralandırma nesnesi*** oluşturulan ve döndürdü. Numaralandırılabilir nesnenin `GetEnumerator` yöntemi döndürür yineleyici bloğunda kodu kapsülleyen bir sabit listesi nesnesi belirtilen ve yineleyici bloğu içindeki kod yürütülmesi gerçekleşir, numaralandırıcı nesnenin `MoveNext` yöntemi çağrılır. Bir numaralandırma nesnesi, aşağıdaki özelliklere sahiptir:

*  Bunu uygulayan `IEnumerable` ve `IEnumerable<T>`burada `T` yield yineleyici türüdür.
*  Bağımsız değişken değerlerini bir kopyasını (varsa) başlatılır ve işlev üyesine geçirilen örnek değer.

Bir numaralandırma nesnesi genellikle bir yineleyici bloğu içindeki kod kapsüller ve numaralandırılabilir arabirimler uygulayan bir sınıfın derleyicinin ürettiği numaralandırılabilir örneğidir, ancak uygulama, diğer yöntemler kullanılabilir. Numaralandırılabilir bir sınıf, derleyici tarafından oluşturulan, o sınıfı iç içe olamaz, doğrudan veya dolaylı olarak işlevi üye içeren sınıfın özel erişilebilirlik sahip olur ve derleyici kullanılmak üzere ayrılmış bir adı olacaktır ([tanımlayıcıları ](lexical-structure.md#identifiers)).

Bir numaralandırma nesnesi yukarıda belirtilen olandan daha fazla arabirim uygulayabilir. Özellikle, bir numaralandırma nesnesi de uygulayabilir `IEnumerator` ve `IEnumerator<T>`, bir numaralandırılabilir hem bir numaralandırıcı olarak hizmet edecek şekilde etkinleştirme. Bu tür uygulama, ilk kez bir nesnenin numaralandırılabilir `GetEnumerator` yöntemi çağrılır, numaralandırılabilir nesne döndürülür. Sonraki çağrılar nesnenin numaralandırılabilir `GetEnumerator`herhangi biri, döndürmek, numaralandırılabilir nesnesinin bir kopyasını. Bu nedenle, her Numaralayıcı kendi durumunu sahiptir ve başka bir numaralandırıcı değişiklikleri etkilemez döndürdü.

#### <a name="the-getenumerator-method"></a>GetEnumerator yöntemi

Bir numaralandırma nesnesi bir uygulamasını sağlar `GetEnumerator` yöntemlerinin `IEnumerable` ve `IEnumerable<T>` arabirimleri. İki `GetEnumerator` yöntemleri alır ve bir kullanılabilir sabit listesi nesnesi döndüren ortak bir uygulama paylaşın. Numaralandırıcı nesnesi bağımsız değişken değerleri başlatılır ve başlatıldı ancak aksi numaralandırma nesnesi, kaydedilen değeri açıklandığı Numaralandırıcı nesnesi işlevleri örnek [Numaralandırıcı nesneleri](classes.md#enumerator-objects).

### <a name="implementation-example"></a>Uygulama örneği

Bu bölümde, standart C# yapılarını açısından Yineleyicilerin olası bir uygulaması açıklanmaktadır. Burada açıklanan uygulama, Microsoft C# derleyicisi tarafından kullanılan aynı ilkelere dayalı ancak mandated uygulaması veya yalnızca bir mümkün olmadığı göre Hayır değil.

Aşağıdaki `Stack<T>` sınıfının uygular, `GetEnumerator` yöntemi kullanarak bir yineleyici. Yineleyici yığınının alt sipariş üstteki öğeleri sıralar.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

class Stack<T>: IEnumerable<T>
{
    T[] items;
    int count;

    public void Push(T item) {
        if (items == null) {
            items = new T[4];
        }
        else if (items.Length == count) {
            T[] newItems = new T[count * 2];
            Array.Copy(items, 0, newItems, 0, count);
            items = newItems;
        }
        items[count++] = item;
    }

    public T Pop() {
        T result = items[--count];
        items[count] = default(T);
        return result;
    }

    public IEnumerator<T> GetEnumerator() {
        for (int i = count - 1; i >= 0; --i) yield return items[i];
    }
}
```

`GetEnumerator` Yöntemi dönüştürülür örneklemesi yineleyici bloğu içindeki kod kapsülleyen bir numaralandırıcı derleyici tarafından oluşturulan sınıfın içine aşağıdaki gösterildiği gibi.

```csharp
class Stack<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1: IEnumerator<T>, IEnumerator
    {
        int __state;
        T __current;
        Stack<T> __this;
        int i;

        public __Enumerator1(Stack<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
                case 1: goto __state1;
                case 2: goto __state2;
            }
            i = __this.count - 1;
        __loop:
            if (i < 0) goto __state2;
            __current = __this.items[i];
            __state = 1;
            return true;
        __state1:
            --i;
            goto __loop;
        __state2:
            __state = 2;
            return false;
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Önceki çeviri yineleyici bloğu içindeki kod bir Durum makinesi açık ve bulundukları `MoveNext` Numaralandırıcı sınıfının yöntemi. Ayrıca, yerel değişken `i` Numaralandırıcı nesnesi bir alana çağrıları arasında mevcut devam edebilmesi açık `MoveNext`.

Aşağıdaki örnek, basit bir çarpan tablosunu 1-10 arasındaki tamsayılar yazdırır. `FromTo` Örnek yönteminde sıralanabilir bir nesne döndürür ve bir yineleyici kullanılarak uygulanır.

```csharp
using System;
using System.Collections.Generic;

class Test
{
    static IEnumerable<int> FromTo(int from, int to) {
        while (from <= to) yield return from++;
    }

    static void Main() {
        IEnumerable<int> e = FromTo(1, 10);
        foreach (int x in e) {
            foreach (int y in e) {
                Console.Write("{0,3} ", x * y);
            }
            Console.WriteLine();
        }
    }
}
```

`FromTo` Yöntemi dönüştürülür örneklemesi derleyici tarafından oluşturulan ve yineleyici bloğu içindeki kod kapsülleyen bir numaralandırılabilir sınıfı içine aşağıdaki gösterildiği gibi.

```csharp
using System;
using System.Threading;
using System.Collections;
using System.Collections.Generic;

class Test
{
    ...

    static IEnumerable<int> FromTo(int from, int to) {
        return new __Enumerable1(from, to);
    }

    class __Enumerable1:
        IEnumerable<int>, IEnumerable,
        IEnumerator<int>, IEnumerator
    {
        int __state;
        int __current;
        int __from;
        int from;
        int to;
        int i;

        public __Enumerable1(int __from, int to) {
            this.__from = __from;
            this.to = to;
        }

        public IEnumerator<int> GetEnumerator() {
            __Enumerable1 result = this;
            if (Interlocked.CompareExchange(ref __state, 1, 0) != 0) {
                result = new __Enumerable1(__from, to);
                result.__state = 1;
            }
            result.from = result.__from;
            return result;
        }

        IEnumerator IEnumerable.GetEnumerator() {
            return (IEnumerator)GetEnumerator();
        }

        public int Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            switch (__state) {
            case 1:
                if (from > to) goto case 2;
                __current = from++;
                __state = 1;
                return true;
            case 2:
                __state = 2;
                return false;
            default:
                throw new InvalidOperationException();
            }
        }

        public void Dispose() {
            __state = 2;
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Numaralandırılabilir arabirimler hem bir numaralandırılabilir hem bir numaralandırıcı olarak hizmet edecek şekilde etkinleştirme Numaralandırıcı arabirimleri numaralandırılabilir sınıfı uygular. İlk kez `GetEnumerator` yöntemi çağrılır, numaralandırılabilir nesne döndürülür. Sonraki çağrılar nesnenin numaralandırılabilir `GetEnumerator`herhangi biri, döndürmek, numaralandırılabilir nesnesinin bir kopyasını. Bu nedenle, her Numaralayıcı kendi durumunu sahiptir ve başka bir numaralandırıcı değişiklikleri etkilemez döndürdü. `Interlocked.CompareExchange` Yöntemi iş parçacığı açısından güvenli çalışmasını sağlamak için kullanılır.

`from` Ve `to` parametreleri alanlara numaralandırılabilir sınıfında etkinleştirilir. Çünkü `from` yineleyici bloğunda, ek bir değişiklik `__from` alan için verilen ilk değer tutacak sunulmuştur `from` içinde her Numaralandırıcı.

`MoveNext` Yöntem bir `InvalidOperationException` ne zaman çağrılırsa `__state` olduğu `0`. Bu ilk çağırmadan bir sabit listesi nesnesi olarak numaralandırma nesnesi kullanımını karşı korur `GetEnumerator`.

Aşağıdaki örnek, bir basit ağaç sınıfı gösterir. `Tree<T>` Sınıfının uygular, `GetEnumerator` yöntemi kullanarak bir yineleyici. Yineleyici içtakı sırayla ağacı öğeleri sıralar.

```csharp
using System;
using System.Collections.Generic;

class Tree<T>: IEnumerable<T>
{
    T value;
    Tree<T> left;
    Tree<T> right;

    public Tree(T value, Tree<T> left, Tree<T> right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }

    public IEnumerator<T> GetEnumerator() {
        if (left != null) foreach (T x in left) yield x;
        yield value;
        if (right != null) foreach (T x in right) yield x;
    }
}

class Program
{
    static Tree<T> MakeTree<T>(T[] items, int left, int right) {
        if (left > right) return null;
        int i = (left + right) / 2;
        return new Tree<T>(items[i], 
            MakeTree(items, left, i - 1),
            MakeTree(items, i + 1, right));
    }

    static Tree<T> MakeTree<T>(params T[] items) {
        return MakeTree(items, 0, items.Length - 1);
    }

    // The output of the program is:
    // 1 2 3 4 5 6 7 8 9
    // Mon Tue Wed Thu Fri Sat Sun

    static void Main() {
        Tree<int> ints = MakeTree(1, 2, 3, 4, 5, 6, 7, 8, 9);
        foreach (int i in ints) Console.Write("{0} ", i);
        Console.WriteLine();

        Tree<string> strings = MakeTree(
            "Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun");
        foreach (string s in strings) Console.Write("{0} ", s);
        Console.WriteLine();
    }
}
```

`GetEnumerator` Yöntemi dönüştürülür örneklemesi yineleyici bloğu içindeki kod kapsülleyen bir numaralandırıcı derleyici tarafından oluşturulan sınıfın içine aşağıdaki gösterildiği gibi.

```csharp
class Tree<T>: IEnumerable<T>
{
    ...

    public IEnumerator<T> GetEnumerator() {
        return new __Enumerator1(this);
    }

    class __Enumerator1 : IEnumerator<T>, IEnumerator
    {
        Node<T> __this;
        IEnumerator<T> __left, __right;
        int __state;
        T __current;

        public __Enumerator1(Node<T> __this) {
            this.__this = __this;
        }

        public T Current {
            get { return __current; }
        }

        object IEnumerator.Current {
            get { return __current; }
        }

        public bool MoveNext() {
            try {
                switch (__state) {

                case 0:
                    __state = -1;
                    if (__this.left == null) goto __yield_value;
                    __left = __this.left.GetEnumerator();
                    goto case 1;

                case 1:
                    __state = -2;
                    if (!__left.MoveNext()) goto __left_dispose;
                    __current = __left.Current;
                    __state = 1;
                    return true;

                __left_dispose:
                    __state = -1;
                    __left.Dispose();

                __yield_value:
                    __current = __this.value;
                    __state = 2;
                    return true;

                case 2:
                    __state = -1;
                    if (__this.right == null) goto __end;
                    __right = __this.right.GetEnumerator();
                    goto case 3;

                case 3:
                    __state = -3;
                    if (!__right.MoveNext()) goto __right_dispose;
                    __current = __right.Current;
                    __state = 3;
                    return true;

                __right_dispose:
                    __state = -1;
                    __right.Dispose();

                __end:
                    __state = 4;
                    break;

                }
            }
            finally {
                if (__state < 0) Dispose();
            }
            return false;
        }

        public void Dispose() {
            try {
                switch (__state) {

                case 1:
                case -2:
                    __left.Dispose();
                    break;

                case 3:
                case -3:
                    __right.Dispose();
                    break;

                }
            }
            finally {
                __state = 4;
            }
        }

        void IEnumerator.Reset() {
            throw new NotSupportedException();
        }
    }
}
```

Kullanılan derleyicinin ürettiği değerlendirmesidir `foreach` deyimleri yükseltilmiş içine `__left` ve `__right` Numaralandırıcı nesnesi alanları. `__state` Numaralandırıcı nesnesi alanının dikkatli bir şekilde güncelleştirilmiş böylece doğru `Dispose()` yöntemin çağrılacağı doğru değilse bir özel durum oluşturulur. Not ile basit kod yazmayı mümkün değildir, `foreach` deyimleri.

## <a name="async-functions"></a>Zaman uyumsuz işlevleri

Bir yöntem ([yöntemleri](classes.md#methods)) veya anonim işlevi ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) ile `async` değiştiricisi çağrıldığında bir ***zaman uyumsuz işlev***. Genel olarak, terim ***zaman uyumsuz*** herhangi bir tür olan işlev açıklamak için kullanılan `async` değiştiricisi.

Bir derleme zamanı hata belirtmek için bir zaman uyumsuz işlev biçimsel parametre listesinde `ref` veya `out` parametreleri.

*Döndür_tür* zaman uyumsuz bir yöntem ya da olmalıdır `void` veya ***görev türü***. Görev türleri `System.Threading.Tasks.Task` ve türleri oluşturulan gelen `System.Threading.Tasks.Task<T>`. Konuyu uzatmamak amacıyla, bu bölümde bu tür olarak başvurulan `Task` ve `Task<T>`sırasıyla. Görev türü döndüren bir zaman uyumsuz yöntem, görev döndüren olarak kabul edilir.

Görev türleri tam tanımı tanımlanan uygulama, ancak dil açısından bakıldığında görev türü durumlardan birinde eksik başarılı veya hatalı. İlgili bir özel durum hatalı bir görev kaydeder. Başarılı bir `Task<T>` türünün bir sonucunun kayıtları `T`. Görev türü beklenebilir ve bu nedenle, işlenenler olabilir await ifadeleri ([Await ifadeleri](expressions.md#await-expressions)).

Bir zaman uyumsuz işlev çağrısını değerlendirme askıya alma olanağı vardır await ifadeleri yoluyla ([Await ifadeleri](expressions.md#await-expressions)) gövdesinde. Değerlendirme daha sonra sürdürüldü askıya alma noktasında await ifadesi yoluyla bir ***sürdürme temsilci***. Sürdürme temsilci türünde `System.Action`, ve çağrıldığında, zaman uyumsuz işlev çağrısının değerlendirme seçeneğindeki await ifadesine kaldığı yerden devam eder. ***Geçerli arayan*** bir zaman uyumsuz çağırma özgün çağıran işlev çağrısını hiçbir zaman askıya alınırsa veya sürdürme temsilci en son çağıran aksi işlevidir.

### <a name="evaluation-of-a-task-returning-async-function"></a>Bir görev döndüren zaman uyumsuz İşlev değerlendirmesi

Bir görev döndüren zaman uyumsuz işlev çağırmayı oluşturulacak döndürülen görevin türü örneği neden olur. Bu adlandırılır ***dönüş görev*** zaman uyumsuz işlev. Görev başlangıçta bir eksik durumda.

(Bir await deyimindeki ulaşma) ya da askıya alındı veya sonlandırılırsa, kadar zaman uyumsuz işlev gövdesi sonra hangi noktası denetim görevi Döndür birlikte çağırana döndürülen değerlendirilir.

Zaman uyumsuz işlev gövdesini sonlandırıldığında dönüş görev tamamlanmamış durumundan taşınır:

*  İşlev gövdesi bir return deyimi veya gövdesinin sonuna ulaşmadan sonucu olarak sona ererse, herhangi bir sonuç değeri başarılı bir duruma getirilir dönüş görev kaydedilir.
*  İşlev gövdesi yakalanmayan bir özel durum sonucu olarak kesilip kesilmediğini ([fırlatma](statements.md#the-throw-statement)) özel durum, hatalı bir duruma getirilir dönüş görev kaydedilir.

### <a name="evaluation-of-a-void-returning-async-function"></a>Void döndüren zaman uyumsuz işlev değerlendirme

Zaman uyumsuz işlev dönüş türü ise `void`, değerlendirme farklı Yukarıdakilerden şu şekilde: hiçbir görev döndürdüğünden işlevi yerine tamamlama ve geçerli iş parçacığının özel durumlar iletişim kuran ***eşitleme bağlam***. Eşitleme bağlamı tam tanımı uygulamaya bağlıdır, ancak bir gösterimiyse, geçerli iş parçacığı "nerede" çalışıyor. Void döndüren zaman uyumsuz işlev değerlendirme yenilenirse, başarıyla tamamlandığında veya yakalanmayan bir özel durum oluşturulmasına neden olur, eşitleme bağlamı bildirilir.

Bu, kaç void döndüren zaman uyumsuz işlevleri altında çalışan izler ve bunların dışında özel durum yayma nasıl karar vermek için bağlam sağlar.
