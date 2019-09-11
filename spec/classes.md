---
ms.openlocfilehash: 2c87cafb8591b9dff2aa517b65af80ab263c7faa
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876898"
---
# <a name="classes"></a>Sınıflar

Sınıf veri üyeleri (sabitler ve alanlar), işlev üyeleri (Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve iç içe türler içerebilen bir veri yapısıdır. Sınıf türleri, türetilmiş bir sınıfın bir temel sınıfı genişletebileceği ve özelleştirileceği bir mekanizma olan devralmayı destekler.

## <a name="class-declarations"></a>Sınıf bildirimleri

Bir *class_declaration* , yeni bir sınıf bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)).

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

Bir *class_declaration* , isteğe bağlı bir *öznitelik* kümesi ([öznitelikler](attributes.md)) ve ardından isteğe bağlı bir `partial` değiştirici kümesi ([sınıf değiştiricileri](classes.md#class-modifiers)) ve ardından anahtar `class` sözcüğü ve sonra, isteğe bağlı bir *type_parameter_list* ([tür parametreleri](classes.md#type-parameters)) ve ardından isteğe bağlı bir *class_base* belirtimi (sınıf taban belirtimi) içeren bir *tanımlayıcı* .[ ](classes.md#class-base-specification)) ardından, isteğe bağlı bir *type_parameter_constraints_clause*s kümesi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) ve ardından bir *class_body* ([sınıf gövdesi](classes.md#class-body)), isteğe bağlı olarak bir noktalı virgül gelir.

Bir sınıf bildirimi, bir *type_parameter_list*sağlamadıkça, *type_parameter_constraints_clause*s sağlayamazsınız.

Bir *type_parameter_list* sağlayan bir sınıf bildirimi, ***Genel sınıf bildirimidir***. Ayrıca, bir genel sınıf bildiriminde iç içe yerleştirilmiş olan herhangi bir sınıf ya da genel bir yapı bildirimi, oluşturulmuş bir tür oluşturmak için, kapsayan türün tür parametreleri sağlanmalıdır.

### <a name="class-modifiers"></a>Sınıf değiştiricileri

Bir *class_declaration* , isteğe bağlı olarak bir sınıf değiştiricileri dizisi içerebilir:

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

Aynı değiştiricinin bir sınıf bildiriminde birden çok kez görünmesi için derleme zamanı hatası vardır.

İç içe sınıflarda değiştiriciye izin verilir. `new` Bu, sınıfın, [Yeni değiştirici](classes.md#the-new-modifier)bölümünde açıklandığı gibi, devralınan bir üyeyi aynı adla gizlediğini belirtir. `new` Değiştiricinin iç içe geçmiş sınıf bildirimi olmayan bir sınıf bildiriminde görünmesi için derleme zamanı hatası.

,, Ve değiştiricileri`private` sınıfının erişilebilirliğini denetler. `protected` `public` `internal` Sınıf bildiriminin gerçekleştiği içeriğe bağlı olarak, bu değiştiricilerin bazılarına izin verilmeyebilir ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)).

`abstract`, Vedeğiştiricileri`static` aşağıdaki bölümlerde ele alınmıştır. `sealed`

#### <a name="abstract-classes"></a>Soyut sınıflar

`abstract` Değiştirici, bir sınıfın tamamlanmamış olduğunu ve yalnızca temel sınıf olarak kullanılması amaçlanan olduğunu göstermek için kullanılır. Soyut bir sınıf, soyut olmayan bir sınıftan aşağıdaki yollarla farklılık gösterir:

*  Soyut bir sınıf doğrudan başlatılamaz ve bir derleme zamanı hatası, `new` işleci bir soyut sınıfta kullanılır. Derleme zamanı türleri soyut olan değişkenler ve değerler olması mümkün olsa da, bu tür değişkenler ve değerler, soyut türlerden türetilmiş soyut olmayan sınıfların `null` örneklerine başvuru ya da içermeli.
*  Soyut bir sınıfa, soyut Üyeler içermesi için izin verilir (ancak gerekli değildir).
*  Soyut bir sınıf korumalı olamaz.

Soyut olmayan bir sınıf, soyut bir sınıftan türetilmişse, soyut olmayan sınıfın devralınan tüm soyut üyelerin gerçek uygulamalarını içermesi gerekir, böylece bu soyut üyeleri geçersiz kılar. Örnekte
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
Soyut sınıf `A` , soyut bir yöntem `F`sunar. Sınıfı `B` ek bir yöntem `G`tanıtır, ancak uygulamasının bir uygulamasını `F`sağlamadığından, `B` soyut olarak da bildirilmelidir. Sınıfı `C` geçersiz `F` kılar ve gerçek bir uygulama sağlar. İçinde `C`soyut üye olmadığından, `C` soyut olmayan olması için izin verilir (ancak zorunlu değildir).

#### <a name="sealed-classes"></a>Mühürlü sınıflar

Değiştirici `sealed` , bir sınıftan türetmeye engel olmak için kullanılır. Bir Sealed sınıfı, başka bir sınıfın temel sınıfı olarak belirtilmişse, derleme zamanı hatası oluşur.

Sealed bir sınıf de soyut bir sınıf olamaz.

`sealed` Değiştirici, genellikle istenmeden Türetmenin önlenmesi için kullanılır, ancak belirli çalışma zamanı iyileştirmeleri de sağlar. Özellikle, korumalı bir sınıfın hiç türetilmiş sınıfa sahip olmadığı bilindiğinden, korumalı sınıf örneklerine sanal işlev üye çağırmaları sanal olmayan çağırmaları dönüştürmek mümkündür.

#### <a name="static-classes"></a>Statik sınıflar

Değiştirici, bir ***statik sınıf***olarak bildirildiği sınıfı işaretlemek için kullanılır. `static` Statik bir sınıf örneği oluşturulamıyor, tür olarak kullanılamaz ve yalnızca statik üyeleri içerebilir. Yalnızca bir statik sınıf uzantı yöntemlerinin bildirimlerini içerebilir ([Uzantı yöntemleri](classes.md#extension-methods)).

Statik sınıf bildirimi aşağıdaki kısıtlamalara tabidir:

*  Statik bir sınıf, `sealed` veya `abstract` değiştiricisini içeremez. Ancak, statik bir sınıf tarafından örneklenemez veya türetilmediği için, hem korumalı hem de soyut gibi davranır.
*  Statik bir sınıf, bir *class_base* belirtimi ([sınıf taban belirtimi](classes.md#class-base-specification)) içeremez ve açıkça bir temel sınıf veya uygulanan arabirimlerin listesini belirtemez. Statik bir sınıf örtülü olarak türünden `object`devralınır.
*  Statik bir sınıf yalnızca statik Üyeler ([statik ve örnek üyeleri](classes.md#static-and-instance-members)) içerebilir. Sabitler ve iç içe geçmiş türlerin statik üye olarak sınıflandırıldığını unutmayın.
*  Statik bir sınıf, erişilebilirliği olan veya `protected` `protected internal` tarafından tanımlanan üyelere sahip olamaz.

Bu kısıtlamaların herhangi birini ihlal etmek için derleme zamanı hatası vardır.

Statik bir sınıfın örnek oluşturucuları yok. Statik bir sınıfta örnek Oluşturucu bildirmek mümkün değildir ve statik bir sınıf için varsayılan örnek Oluşturucu ([Varsayılan oluşturucular](classes.md#default-constructors)) sağlanmaz.

Statik bir sınıfın üyeleri otomatik olarak statik değildir ve üye bildirimlerinin açıkça bir `static` değiştirici içermesi gerekir (sabitler ve iç içe türler hariç). Bir sınıf statik bir dış sınıf içinde iç içe olduğunda, açıkça bir `static` değiştirici içermiyorsa, iç içe yerleştirilmiş sınıf statik bir sınıf değildir.

__Statik Sınıf türlerine başvurma__

Bir *namespace_or_type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)), şu durumlarda bir statik sınıfa başvurmasına izin verilir

*  *Namespace_or_type_name* `T` , formun namespace_or_type_name`T.I`veya
*  *Namespace_or_type_name* `T` , formun`typeof(T)`bir *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1).

Bir *primary_expression* ([işlev üyeleri](expressions.md#function-members)), şu durumlarda bir statik sınıfa başvurmasına izin verilir

*  *Primary_expression* , `E` formun[](expressions.md#compile-time-checking-of-dynamic-overload-resolution) (dinamikaşırıyüklemeçözümlemesiiçinderlemezamanıdenetimi`E.I`) bir member_access.

Diğer bir bağlamda, statik bir sınıfa başvurmak için derleme zamanı hatası olur. Örneğin, bir statik sınıfın temel sınıf olarak kullanılabilmesi, bir üyenin bir bileşen türü ([Iç içe türler](classes.md#nested-types)), bir genel tür bağımsız değişkeni veya tür parametresi kısıtlaması olması hatadır. Benzer şekilde, bir statik sınıf dizi türünde, bir `new` işaretçi türü, bir ifade, bir tür dönüştürme ifadesi `is` , ifade `sizeof` , `as` ifade, ifade veya varsayılan değer ifadesi içinde kullanılamaz.

### <a name="partial-modifier"></a>Kısmi değiştirici

Değiştirici, bu class_declaration kısmi bir tür bildirimi olduğunu göstermek için kullanılır. `partial` Bir kapsayan ad alanı veya tür bildiriminde aynı ada sahip birden çok kısmi tür bildirimi, [kısmi türlerde](classes.md#partial-types)belirtilen kurallara göre tek bir tür bildirimi oluşturmak için birleştirilir.

Program metninin ayrı kesimleri üzerinde dağıtılan bir sınıf bildiriminin olması, bu parçaların farklı bağlamlarda üretilmesi veya saklanması durumunda yararlı olabilir. Örneğin, bir sınıf bildiriminin bir kısmı makine tarafından oluşturulmuş olabilir, diğeri el ile yazılır. İki ' un metinsel ayrımı, diğer bir güncelleştirme ile çakışma arasından güncelleştirme yapılmasını engeller.

### <a name="type-parameters"></a>Tür parametreleri

Tür parametresi, oluşturulmuş bir tür oluşturmak için sağlanan bir tür bağımsız değişkeni için yer tutucuyu belirten basit bir tanıtıcıdır. Tür parametresi, daha sonra sağlanacak bir tür için resmi yer tutucudur. Buna karşılık, bir tür bağımsız değişkeni ([tür bağımsız değişkenleri](types.md#type-arguments)) oluşturulmuş bir tür oluşturulduğunda tür parametresi için değiştirilen gerçek türdür.

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

Bir sınıf bildirimindeki her tür parametresi, bu sınıfın bildirim alanında ([bildirimlerinde](basic-concepts.md#declarations)) bir ad tanımlar. Bu nedenle, başka bir tür parametresiyle aynı ada veya bu sınıfta belirtilen bir üyeye sahip olamaz. Tür parametresi, türün kendisiyle aynı ada sahip olamaz.

### <a name="class-base-specification"></a>Sınıf temel belirtimi

Sınıf bildirimi, sınıfının doğrudan temel sınıfını ve doğrudan sınıf tarafından uygulanan arabirimleri ([arabirimler](interfaces.md)) tanımlayan bir *class_base* belirtimi içerebilir.

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

Sınıf bildiriminde belirtilen temel sınıf, oluşturulmuş bir sınıf türü ([oluşturulmuş türler](types.md#constructed-types)) olabilir. Bir temel sınıf kendi üzerinde bir tür parametresi olamaz, ancak kapsamdaki tür parametreleri de içerebilir.

```csharp
class Extend<V>: V {}            // Error, type parameter used as base class
```

#### <a name="base-classes"></a>Temel sınıflar

Bir *class_type* *class_base*'e dahil edildiğinde, belirtilen sınıfın doğrudan temel sınıfını belirtir. Bir sınıf bildiriminde *class_base*yoksa veya *class_base* yalnızca arabirim türlerini listelemeli, doğrudan taban sınıfın `object`kabul edilir. Bir sınıf, [Devralma](classes.md#inheritance)bölümünde açıklandığı gibi doğrudan temel sınıfından üyeleri devralır.

Örnekte
```csharp
class A {}

class B: A {}
```
sınıfının `A` doğrudan taban `B`sınıfı olduğu söylenir ve `B` öğesinden `A`türetilmiştik. Açıkça doğrudan bir temel sınıf belirtmediğinden, doğrudan temel sınıfı örtülü olarak `object`bulunur. `A`

Oluşturulmuş bir sınıf türü için, genel sınıf bildiriminde bir temel sınıf belirtilmişse, oluşturulan türün temel sınıfı, temel sınıf bildirimindeki her bir *type_parameter* için yerine koyarak elde edilir, buna karşılık gelen *type_argument* oluşturulan tür. Genel sınıf bildirimleri verildiğinde
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
oluşturulan türün `G<int>` `B<string,int[]>`temel sınıfı olur.

Bir sınıf türünün doğrudan temel sınıfı en az sınıf türünün kendisi ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) olarak erişilebilir olmalıdır. Örneğin, bir `public` sınıf için bir `private` veya `internal` sınıfından türetmeye yönelik derleme zamanı hatası olur.

Bir sınıf türünün doğrudan temel sınıfı şu türlerden biri olmamalıdır: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`veya `System.ValueType`. Ayrıca, genel bir sınıf bildirimi doğrudan veya `System.Attribute` dolaylı temel sınıf olarak kullanılamaz.

`A` Bir sınıfın `B`doğrudan temel sınıf belirtiminin anlamı belirlenirken, öğesinin `B` doğrudan taban sınıfının geçici olarak olduğu varsayılır `object`. Intuicanlı bu, bir temel sınıf belirtiminin anlamını özyinelemeli olarak kendi kendine bağımlı olmamasını sağlar. Örnek:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
`A<C.B>` temel sınıf belirtiminde bu yana hata durumunda, öğesinin `C` doğrudan taban sınıfı olarak kabul `object`edilir ve bu nedenle ( [ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)kuralları tarafından) `C` bir üyeye `B`sahipolduğukabuledilmez.

Bir sınıf türünün temel sınıfları doğrudan taban sınıfıdır ve temel sınıflarıdır. Diğer bir deyişle, temel sınıfların kümesi doğrudan temel sınıf ilişkisinin geçişli kapanışı olur. Yukarıdaki örneğe başvurarak, temel sınıfları `B` ve `object`' dir `A` . Örnekte
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
`D<int>` temel sınıfları `C<int[]>`, `B<IComparable<int[]>>` ,ve`object`. `A`

Sınıf `object`dışında, her sınıf türünün tam olarak bir doğrudan temel sınıfı vardır. `object` Sınıfın doğrudan temel sınıfı yoktur ve diğer tüm sınıfların en son temel sınıfıdır.

Bir sınıf `B` bir sınıftan `A`türetildiği zaman, için `A` derleme zamanı `B`hatası olur. Bir sınıf doğrudan temel sınıfına (varsa) ***bağlıdır*** ve doğrudan iç içe geçmiş (varsa) sınıfa ***bağlıdır*** . Bu tanım verildiğinde, bir sınıfın bağımlı olduğu tüm sınıf kümesi, ***doğrudan ilişkiye bağlı olarak değişir*** .

Örnek
```csharp
class A: A {}
```
sınıf kendi kendine bağımlı olduğundan hatalı. Benzer şekilde, örnek
```csharp
class A: B {}
class B: C {}
class C: A {}
```
sınıfların kendilerine döngüsel olarak bağımlı olması nedeniyle hata oluştu. Son olarak, örnek
```csharp
class A: B.C {}

class B: A
{
    public class C {}
}
```
bir derleme zamanı `A` hatasına neden olur çünkü `B.C` (doğrudan kapsayan `A`sınıfa) bağlı olan (kendisine ait olan `B` sınıf), bu, döngüsel olarak değişir.

Bir sınıfın içinde iç içe yerleştirilmiş sınıflara bağlı olmadığına unutmayın. Örnekte
```csharp
class A
{
    class B: A {}
}
```
`B``B` `B` `A` `A` öğesine bağımlıdır (çünkühemdoğrudantemelsınıfıhemdeonunhemenkapsayansınıfı),ancaköğesinebağımlıdeğildir(çünkü,birtemelsınıfveyakapsayanbirsınıfıdeğil)`A` `A`). Bu nedenle, örnek geçerlidir.

Bir `sealed` sınıftan türetmek mümkün değildir. Örnekte
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
sınıf, `sealed` sınıftan`A`türemeye çalıştığı için hatalı. `B`

#### <a name="interface-implementations"></a>Arabirim Uygulamaları

Bir *class_base* belirtimi, arabirim türlerinin bir listesini içerebilir, bu durumda sınıf verilen arabirim türlerini doğrudan uygulayacak şekilde söylenir. Arabirim Uygulamaları, [arabirim uygulamalarında](interfaces.md#interface-implementations)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="type-parameter-constraints"></a>Tür parametresi kısıtlamaları

Genel tür ve yöntem bildirimleri, isteğe bağlı olarak *type_parameter_constraints_clause*s ekleyerek tür parametresi kısıtlamalarını belirtebilir.

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

Her *type_parameter_constraints_clause* , belirtecini `where`, ardından bir tür parametresinin adını, ardından iki nokta üst üste ve bu tür parametresine yönelik kısıtlamaların listesini içerir. Her tür parametresi için en fazla `where` bir yan tümce olabilir `where` ve yan tümceler herhangi bir sırada listelenebilir. Bir özellik erişimcisindeki `set` `get` ve belirteçleri gibi, belirteçbir`where` anahtar sözcük değildir.

Bir `where` yan tümcesinde verilen kısıtlamaların listesi, şu bileşenlerden herhangi birini içerebilir: tek bir birincil kısıtlama, bir veya daha fazla ikincil kısıtlama ve Oluşturucu `new()`kısıtlaması.

Birincil kısıtlama bir sınıf türü veya ***başvuru türü kısıtlaması*** `class` ya da ***değer türü kısıtlaması*** `struct`olabilir. İkincil kısıtlama bir *type_parameter* veya *interface_type*olabilir.

Başvuru türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeninin bir başvuru türü olması gerektiğini belirtir. Tüm sınıf türleri, arabirim türleri, temsilci türleri, dizi türleri ve başvuru türü olarak bilinen tür parametreleri (aşağıda tanımlandığı gibi), bu kısıtlamayı karşılar.

Değer türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeninin null yapılamayan bir değer türü olması gerektiğini belirtir. Bu kısıtlamayı karşılayan, null olamayan tüm yapı türleri, sabit listesi türleri ve değer türü kısıtlamasına sahip tür parametreleri. Değer türü olarak sınıflandırıldığından, null yapılabilir bir tür ([null yapılabilir türler](types.md#nullable-types)) değer türü kısıtlamasını karşılamadığına unutmayın. Değer türü kısıtlamasına sahip bir tür parametresi *constructor_constraint*de sahip olamaz.

İşaretçi türlerinin hiçbir şekilde tür bağımsız değişkeni olmasına izin verilmez ve başvuru türü veya değer türü kısıtlamalarını karşılamak için düşünülmez.

Bir kısıtlama bir sınıf türü, bir arabirim türü veya bir tür parametresi ise, bu tür parametresi için kullanılan her tür bağımsız değişkenin desteklemesi gereken en az "temel türü" değeri belirtir. Oluşturulmuş bir tür veya genel yöntem kullanıldığında, tür bağımsız değişkeni derleme zamanında tür parametresindeki kısıtlamalara karşı denetlenir. Sağlanan tür bağımsız değişkeni, [kısıtlamaları](types.md#satisfying-constraints)karşıladığı koşullarda açıklanan koşullara uymalıdır.

Bir *class_type* kısıtlaması aşağıdaki kuralları karşılamalıdır:

*  Tür bir sınıf türü olmalıdır.
*  Türün olmaması `sealed`gerekir.
*  Tür şu türlerden biri olmalıdır `System.Array`:, `System.Delegate`, `System.Enum`veya `System.ValueType`.
*  Türün olmaması `object`gerekir. Tüm türler öğesinden `object`türetildiğinden, böyle bir kısıtlama izin verildiğinde hiçbir etkiye sahip olmaz.
*  Verilen tür parametresi için en fazla bir kısıtlama bir sınıf türü olabilir.

*İnterface_type* kısıtlaması olarak belirtilen bir türün aşağıdaki kuralları karşılaması gerekir:

*  Tür bir arabirim türü olmalıdır.
*  Bir tür, verili `where` bir yan tümce içinde birden çok kez belirtilmemelidir.

Her iki durumda da kısıtlama, oluşturulmuş bir türün parçası olarak ilişkili tür veya yöntem bildiriminin herhangi bir tür parametresini içerebilir ve bu tür, bildirildiği türü içerebilir.

Tür parametresi kısıtlaması olarak belirtilen herhangi bir sınıf veya arabirim türünün, bildirildiği genel tür veya yöntem olarak en az erişilebilir ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olması gerekir.

*Type_parameter* kısıtlaması olarak belirtilen bir türün aşağıdaki kuralları karşılaması gerekir:

*  Tür bir tür parametresi olmalıdır.
*  Bir tür, verili `where` bir yan tümce içinde birden çok kez belirtilmemelidir.

Ayrıca, bağımlılık grafiğinde tür parametrelerinin bağımlılık grafiğinde bir döngü olmaması gerekir; burada bağımlılık, tarafından tanımlanan geçişli bir ilişki olur:

*  Bir `T` tür parametresi `T`tür `S` parametresi için kısıtlama olarak kullanılırsa, ***öğesine bağlıdır.*** `S`
*  `S` Bir tür parametresi bir tür parametresine bağımlıysa `T` `U` ve `S` `T` bir tür parametresine bağımlıysa, ***öğesine bağımlıdır*** `U`.

Bu ilişki verildiğinde, bir tür parametresinin kendisine (doğrudan veya dolaylı olarak) bağımlı olması için derleme zamanı hatası vardır.

Herhangi bir kısıtlama bağımlı tür parametreleri arasında tutarlı olmalıdır. Tür parametresi `S` daha sonra tür parametresine `T` bağımlıysa:

*  `T`değer türü kısıtlamasına sahip olmamalıdır. Aksi takdirde `T` , etkin bir şekilde `S` mühürlenmiş olduğundan `T`, iki tür parametresi gereksinimini ortadan kaldıran aynı türde olmaya zorlanır.
*  Değer `S` türü `T` kısıtlaması varsa, *class_type* kısıtlamasına sahip olmamalıdır.
*  Eğer `S` bir *class_type* kısıtlaması `A` varsa ve `T` bir *class_type* kısıtlaması `B` varsa, bir kimlik dönüştürmesi veya öğesinden `A` `B`örtükbaşvurudönüştürmesiolmalıdırya da ' dan `B` ' ye `A`örtük bir başvuru dönüştürmesi.
*  `U` `U` `T` `A` `B` Ayrıca tür parametresine bağlıdır ve bir class_type kısıtlamasına sahiptir ve bir class_type kısıtlamasına sahipse, bir kimlik dönüştürmesi olmalıdır `S` ya da örtük başvuru dönüşümünü `A` `B` 'den`B` ' ye dönüştürme. `A`

Değer türü kısıtlamasına sahip `S` olmak ve `T` başvuru türü kısıtlamasına sahip olmak için geçerlidir. Bu `T` türler`System.Object`, ,`System.Enum`,ve herhangi bir arabirim türü için etkili bir şekilde sınırlandırılmıştır. `System.ValueType`

Bir tür parametresi için `new()` `new` [](expressions.md#object-creation-expressions)yan tümce bir Oluşturucu kısıtlaması içeriyorsa (formun bulunduğu), türü örnekleri oluşturmak için işlecini kullanmak mümkündür (nesne oluşturma ifadeleri) `where` . Oluşturucu kısıtlaması olan bir tür parametresi için kullanılan herhangi bir tür bağımsız değişkeni Ortak parametresiz bir oluşturucuya sahip olmalıdır (Bu Oluşturucu herhangi bir değer türü için örtülü olarak bulunur) veya değer türü kısıtlaması veya Oluşturucu kısıtlamasına sahip bir tür parametresi olmalıdır (bkz. Ayrıntılar için [parametre kısıtlamalarını yazın](classes.md#type-parameter-constraints) ).

Kısıtlamaların örnekleri aşağıda verilmiştir:
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

Tür parametrelerinin bağımlılık grafiğinde bir döngüye neden olduğu için aşağıdaki örnek hata durumunda:
```csharp
class Circular<S,T>
    where S: T
    where T: S                // Error, circularity in dependency graph
{
    ...
}
```

Aşağıdaki örneklerde, ek geçersiz durumlar gösterilmektedir:
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

Bir tür parametresinin `T` ***etkin temel sınıfı*** aşağıdaki gibi tanımlanır:

*  Birincil `T` kısıtlamaları veya tür parametresi kısıtlamalarına sahip değilse, etkin taban sınıfı olur. `object`
*  Değer türü kısıtlamasına `System.ValueType` sahipse,etkin`T` taban sınıfı olur.
*  Eğer `T` bir *class_type* `C` kısıtlaması varsa ancak *type_parameter* kısıtlaması yoksa, etkin taban sınıfı olur `C`.
*  Class_type kısıtlaması yoksa ancak bir veya daha fazla *type_parameter* kısıtlaması varsa, etkin taban sınıfı type_ 'un etkin temel sınıfları kümesindeki en kapsamlı tür ([yükseltilmemiş dönüştürme işleçleri)](conversions.md#lifted-conversion-operators) `T`  *parametre* kısıtlamaları. Tutarlılık kuralları, bu tür bir en kapsamlı türün mevcut olmasını güvence altına alıyor.
*  Hem `T` bir *class_type* kısıtlaması hem de bir veya daha fazla *type_parameter* kısıtlaması varsa, etkin taban sınıfı *class_type* oluşan küme içindeki en kapsamlı tür ([yükseltilmemiş dönüştürme işleçleri](conversions.md#lifted-conversion-operators)) kısıtlaması ve type_parameter kısıtlamalarının etkin temel sınıfları. `T` Tutarlılık kuralları, bu tür bir en kapsamlı türün mevcut olmasını güvence altına alıyor.
*  Başvuru `T` türü kısıtlaması varsa ancak *class_type* kısıtlaması yoksa, etkin taban sınıfı olur `object`.

Bu kuralların amacına yönelik olarak, T 'nin `V` bir *value_type*kısıtlaması varsa, bunun yerine bir `V` *class_type*olan en özel temel türü kullanın. Bu, açıkça verilen kısıtlamada asla gerçekleşmeyebilir, ancak genel bir yöntemin kısıtlamaları geçersiz kılan bir yöntem bildirimi veya bir arabirim yönteminin açık bir uygulamasıyla dolaylı olarak devralındığında gerçekleşebilir.

Bu kurallar, etkin temel sınıfın her zaman bir *class_type*olduğundan emin olur.

Bir tür parametresinin `T` ***etkin arabirim kümesi*** şu şekilde tanımlanır:

*  Secondary_constraints yoksa, etkin arabirimi kümesi boş olur. `T`
*  İnterface_type kısıtlamaları varsa ancak *type_parameter* kısıtlaması yoksa, etkin arabirim kümesi, *interface_type* kısıtlamaları kümesidir. `T`
*  İnterface_type kısıtlaması yoksa ancak *type_parameter* kısıtlamalarına sahipse, etkin arabirim kümesi, *type_parameter* kısıtlamalarının etkin arabirim kümelerinin birleşimidir. `T`
*  Hem interface_type kısıtlamalarına hem de *type_parameter* kısıtlamalarına sahipse,etkinarabirimkümesi,interface_typekısıtlamalarıkümesininbirleşimidirvetype_parametergeçerli`T` arabirim kümeleridirkısıtlamalar.

Bir tür parametresinin başvuru türü kısıtlaması varsa veya etkin taban sınıfı veya `System.ValueType`değilse `object` , ***başvuru türü olarak bilinir*** .

Kısıtlanmış tür parametre türünün değerleri, kısıtlamalar tarafından kapsanan örnek üyelerine erişmek için kullanılabilir. Örnekte
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
yöntemi `IPrintable` , her zaman uygulanması `x` `T` kısıtlanan`IPrintable`için doğrudan üzerinde çağrılabilir.

### <a name="class-body"></a>Sınıf gövdesi

Bir sınıfın *class_body* , bu sınıfın üyelerini tanımlar.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Kısmi türler

Bir tür bildirimi, birden çok ***Kısmi tür***bildirimine bölünebilir. Tür bildirimi, bu bölümdeki kurallara göre, programın derleme zamanı ve çalışma zamanı işlemenin geri kalanı sırasında tek bir bildirim olarak değerlendirilmesinin ardından parçalarından oluşturulur.

Bir *class_declaration*, *struct_declaration* veya *interface_declaration* , bir `partial` değiştirici içeriyorsa kısmi tür bildirimini temsil eder. `partial`bir anahtar sözcük değildir `class`ve yalnızca anahtar sözcüklerden `struct` birinden veya `interface` bir tür bildiriminde ya da bir yöntem bildiriminde bulunan türden `void` önce görünürse bir değiştirici işlevi görür. Diğer bağlamlarda, normal tanımlayıcı olarak kullanılabilir.

Kısmi tür bildiriminin her bölümü bir `partial` değiştirici içermelidir. Aynı ada sahip olmalıdır ve diğer bölümlerle aynı ad alanında veya tür bildiriminde bildirilmelidir. Değiştirici, tür bildiriminin ek bölümlerinin başka bir yerde mevcut olabileceğini gösterir, ancak bu tür ek parçaların varlığı bir gereksinim değildir; `partial` değiştiricisini içermesi için tek bir bildirime sahip bir tür için geçerlidir. `partial`

Kısmi bir türün tüm bölümlerinin, parçaların derleme zamanında tek bir tür bildirimine birleştirilebilmesi için birlikte derlenmesi gerekir. Kısmi türler özellikle derlenmiş türlerin genişletilmesini izin vermez.

İç içe türler, `partial` değiştirici kullanılarak birden çok bölümde tanımlanmış olabilir. Genellikle, kapsayan tür kullanılarak `partial` da tanımlanır ve iç içe türün her bölümü, kapsayan türün farklı bir bölümünde belirtilir.

Temsilci veya Enum bildirimlerinde değiştiriciyeizinverilmez.`partial`

### <a name="attributes"></a>Öznitelikler

Kısmi bir türün öznitelikleri belirtilmemiş bir sıra, parçaların her birinin öznitelikleri birleştirilerek belirlenir. Bir öznitelik birden çok parçaya yerleştirilmişse, öznitelik türü üzerinde birden çok kez belirtmeye eşdeğerdir. Örneğin, iki bölüm:

```csharp
[Attr1, Attr2("hello")]
partial class A {}

[Attr3, Attr2("goodbye")]
partial class A {}
```
, şöyle bir bildirime eşdeğerdir:
```csharp
[Attr1, Attr2("hello"), Attr3, Attr2("goodbye")]
class A {}
```

Tür parametrelerinin öznitelikleri benzer bir biçimde birleştirir.

### <a name="modifiers"></a>Değiştiriciler

Kısmi bir tür bildirimi bir erişilebilirlik `public`belirtimi (,, ve `private` değiştiricileri `protected`) `internal`içerdiğinde, bir erişilebilirlik belirtimi içeren diğer tüm bölümleri kabul etmelidir. Kısmi bir türün hiçbir bölümü bir erişilebilirlik belirtimi içeriyorsa, türe uygun varsayılan erişilebilirlik (belirtilen[Erişilebilirlik](basic-concepts.md#declared-accessibility)) verilir.

İç içe bir türün bir veya daha fazla kısmi bildirimi bir `new` değiştirici içeriyorsa, iç içe geçmiş türü devralınan bir üyeyi gizliyor ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) için uyarı bildirilir.

Bir sınıfın bir veya daha fazla kısmi bildirimi bir `abstract` değiştirici içeriyorsa, sınıf soyut ([soyut sınıflar](classes.md#abstract-classes)) olarak değerlendirilir. Aksi takdirde, sınıfı Özet olmayan olarak kabul edilir.

Bir sınıfın bir veya daha fazla kısmi bildirimi bir `sealed` değiştirici içeriyorsa, sınıf sealed ([Sealed sınıflar](classes.md#sealed-classes)) olarak kabul edilir. Aksi takdirde, sınıfı korumasız kabul edilir.

Bir sınıfın hem abstract hem de Sealed olamayacağını unutmayın.

Değiştirici kısmi bir tür bildiriminde kullanıldığında, yalnızca bu belirli parça güvenli olmayan bir bağlam ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) olarak kabul edilir. `unsafe`

### <a name="type-parameters-and-constraints"></a>Tür parametreleri ve kısıtlamaları

Genel bir tür birden çok bölümde bildirilirse, her parçanın tür parametrelerini durumu olması gerekir. Her parçanın aynı sayıda tür parametreleri ve her tür parametresi için aynı adı sırasıyla aynı olmalıdır.

Kısmi genel tür bildiriminde kısıtlamalar (`where` yan tümceler) varsa, kısıtlamalar, kısıtlamaları içeren diğer tüm bölümleri kabul etmelidir. Özellikle, kısıtlamaları içeren her parçanın aynı tür parametreleri kümesi için kısıtlamaları olmalıdır ve her tür parametresi için, birincil, ikincil ve Oluşturucu kısıtlamalarının kümelerinin eşdeğeri olması gerekir. İki kısıtlama kümesi aynı üyeleri içeriyorsa eşdeğerdir. Kısmi bir genel türün hiçbir bölümü tür parametresi kısıtlamalarını belirtiyorsa tür parametreleri Kısıtlanmamış olarak kabul edilir.

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
, kısıtlamaları (ilk iki) içeren parçalar, sırasıyla aynı tür parametreleri kümesi için aynı birincil, ikincil ve Oluşturucu kısıtlamaları kümesini etkin bir şekilde belirttiğinden doğrudur.

### <a name="base-class"></a>Temel sınıf

Kısmi bir sınıf bildirimi bir temel sınıf belirtimi içerdiğinde, temel sınıf belirtimi içeren diğer tüm bölümleri kabul etmelidir. Kısmi bir sınıfın hiçbir bölümü bir temel sınıf belirtimi içeriyorsa, taban sınıf ([temel sınıflar](classes.md#base-classes)) `System.Object` olur.

### <a name="base-interfaces"></a>Temel arabirimler

Birden çok bölümde belirtilen bir türün temel arabirimler kümesi, her bölümde belirtilen temel arabirimlerin birleşimidir. Belirli bir temel arabirim her bölümde yalnızca bir kez adlandırılabilir, ancak birden fazla parçanın aynı temel arabirimleri adlandırmasına izin verilir. Yalnızca belirli bir temel arabirimin üyelerinin bir uygulanması gerekir.

Örnekte
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
sınıfı `C` için temel arabirimler kümesi, ve `IC`' `IA` `IB`dir.

Genellikle, her parça bu bölümde belirtilen arabirim (ler) in bir uygulamasını sağlar; Ancak, bu bir gereksinim değildir. Bir bölüm, farklı bir bölümde belirtilen bir arabirim için uygulama sağlayabilir:
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

Kısmi yöntemlerin ([kısmi Yöntemler](classes.md#partial-methods)) dışında, birden çok bölümde belirtilen bir türün üyeleri kümesi, her bölümde belirtilen üye kümesinin birleşimidir. Tür bildiriminin tüm bölümlerinin gövdeleri aynı bildirim alanını ([Bildirimler](basic-concepts.md#declarations)) ve her üyenin kapsamını ([kapsamlar](basic-concepts.md#scopes)) paylaşır ve tüm parçaların gövdeleriyle birlikte genişletilir. Herhangi bir üyenin erişilebilirlik etki alanı her zaman kapsayan türün tüm parçalarını içerir; tek `private` bir bölümde belirtilen bir üyeye başka bir bölümden serbestçe erişilebilir. Üyenin `partial` değiştiriciyle bir tür olmadığı durumlar dışında, türün birden fazla bölümünde aynı üyeyi bildirmek için derleme zamanı hatası vardır.

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

Bir tür içindeki üyelerin sıralaması kod için C# nadiren önemlidir, ancak diğer diller ve ortamlarla arabirim oluştururken önemli olabilir. Bu durumlarda, birden çok bölümde belirtilen bir tür içindeki üyelerin sıralaması tanımsızdır.

### <a name="partial-methods"></a>Kısmi Yöntemler

Kısmi Yöntemler, bir tür bildiriminin bir bölümünde tanımlanabilir ve başka bir şekilde uygulanabilir. Uygulama isteğe bağlıdır; kısmi yöntemi herhangi bir bölüm uygularsa, kısmi yöntem bildirimi ve buna yapılan tüm çağrılar, parçaların birleşiminden kaynaklanan tür bildiriminden kaldırılır.

Kısmi Yöntemler erişim değiştiricilerini tanımlanamaz, ancak örtülü olarak `private`bulunur. Dönüş türü olmalıdır `void`ve parametreleri `out` değiştiriciye sahip olamaz. Tanımlayıcı `partial` , bir yöntem bildiriminde yalnızca `void` türden önce görünürse özel bir anahtar sözcük olarak tanınır; Aksi takdirde, normal tanımlayıcı olarak kullanılabilir. Kısmi bir yöntem, arabirim yöntemlerini açıkça uygulayamaz.

İki tür kısmi yöntem bildirimi vardır: Yöntem bildiriminin gövdesi noktalı virgül ise, bildirim bir ***tanımlayıcı kısmi yöntem bildirimi***olarak kabul edilir. Gövde bir *blok*olarak verilirse, bildirim bir ***Uygulama kısmi yöntem bildirimi***olarak kabul edilir. Bir tür bildiriminin bölümlerinde, belirli bir imzayla yalnızca bir adet kısmi yöntem bildirimi tanımlama olabilir ve belirli bir imzaya sahip kısmi yöntem bildirimi yalnızca bir uygulama olabilir. Bir uygulama kısmi yöntem bildirimi verilirse, karşılık gelen bir tanımlayıcı kısmi yöntem bildirimi mevcut olmalıdır ve bildirimlerin aşağıdaki gibi eşleşmesi gerekir:

* Bildirimlerin aynı değiştiricilere sahip olması gerekir (aynı sırada olması olmasa da), yöntem adı, tür parametrelerinin sayısı ve parametre sayısı.
* Bildirimlerdeki karşılık gelen parametreler aynı değiştiricilere sahip olmalıdır (aynı sırada olması olmasa da) ve aynı türlerdir (tür parametre adlarında mod farklılıkları).
* Bildirimlerdeki karşılık gelen tür parametreleri aynı kısıtlamalara (tür parametresi adlarında mod farklılıkları) sahip olmalıdır.

Uygulama kısmi yöntem bildirimi, karşılık gelen tanımlayan kısmi Yöntem bildirimiyle aynı bölümde bulunabilir.

Yalnızca tanımlama bir kısmi yöntem aşırı yükleme çözümüne katılır. Bu nedenle, bir uygulama bildiriminin verilip verilmediğini belirtir, çağırma ifadeleri kısmi yöntemin çağırmaları için çözüm alabilir. Kısmi bir yöntem her zaman döndürdüğü `void`için, böyle bir çağırma ifadesi her zaman ifade deyimleri olur. Ayrıca, kısmi bir yöntem örtük `private`olarak olduğu için, bu tür deyimler her zaman kısmi yöntemin bildirildiği tür bildiriminin parçalarından biri içinde gerçekleşir.

Kısmi bir tür bildiriminin hiçbir bölümü verili bir kısmi Yöntem için uygulama bildirimi içermiyorsa, çağıran herhangi bir ifade deyimi yalnızca Birleşik tür bildiriminden kaldırılır. Bu nedenle, bileşen ifadeleri dahil olmak üzere çağırma ifadesinin çalışma zamanında hiçbir etkisi yoktur. Kısmi yöntemin kendisi de kaldırılır ve birleştirilmiş tür bildiriminin üyesi olmayacaktır.

Belirli bir kısmi Yöntem için uygulama bildirimi varsa, kısmi yöntemlerin etkinleştirmeleri korunur. Kısmi Yöntem, aşağıdaki durumlar hariç, uygulama kısmi Yöntem bildirimine benzer bir yöntem bildirimine göre artış sağlar:

* `partial` Değiştirici dahil değildir
* Elde edilen yöntem bildiriminde bulunan öznitelikler, tanımlama ve Uygulama kısmi Yöntem bildiriminin belirtilmemiş sırada tanımlanmasının Birleşik öznitelikleridir. Yinelemeler kaldırılmaz.
* Sonuç yöntemi bildiriminin parametreleri üzerindeki öznitelikleri, tanımlanmasının karşılık gelen parametrelerinin Birleşik öznitelikleridir ve kısmi Yöntem bildiriminin uygulanması belirtilmemiş sırayla yapılır. Yinelemeler kaldırılmaz.

Bir tanımlama bildirimi ve kısmi Yöntem için bir uygulama bildirimi verilmezse, aşağıdaki kısıtlamalar uygulanır:

* Yönteme bir temsilci oluşturmak için derleme zamanı hatası ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)).
* Bu, bir ifade ağacı türüne dönüştürülen anonim bir işlevin `M` içinde ([ifade ağacı türlerine anonim işlev dönüştürmeleri değerlendirmesi](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)) başvuruda bulunmak için bir derleme zamanı hatasıdır.
* Bir `M` çağrısının parçası olarak oluşan ifadeler, derleme zamanı hatalarına neden olabilecek kesin atama durumunu ([kesin atama](variables.md#definite-assignment)) etkilemez.
* `M`bir uygulama ([uygulama başlatma](basic-concepts.md#application-startup)) için giriş noktası olamaz.

Kısmi Yöntemler, bir tür bildiriminin bir bölümünün bir araç tarafından oluşturulan başka bir parçanın davranışını özelleştirmesine izin vermek için yararlıdır. Aşağıdaki kısmi sınıf bildirimini göz önünde bulundurun:
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

Bu sınıf başka herhangi bir bölüm olmadan derlenirse, tanımlayıcı kısmi yöntem bildirimleri ve bunların etkinleştirmeleri kaldırılır ve sonuçta elde edilen birleştirilmiş sınıf bildirimi aşağıdaki gibi olacaktır:
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

Ancak kısmi yöntemlerin uygulama bildirimlerini sağlayan başka bir bölüme verildiğini varsayalım:
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

Sonuç olarak elde edilen birleştirilmiş sınıf bildirimi, aşağıdaki gibi olacaktır:
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

### <a name="name-binding"></a>Ad bağlama

Genişletilebilir bir türün her bölümü aynı ad alanı içinde bildirilmelidir, ancak parçalar genellikle farklı ad alanı bildirimleri içinde yazılır. Bu nedenle, `using` her bölüm için farklı yönergeler ([yönergeler kullanılarak](namespaces.md#using-directives)) bulunabilir. Tek bir bölüm içindeki basit adları ([tür çıkarımı](expressions.md#type-inference)) yorumlarken, `using` yalnızca o parçayı kapsayan ad alanı bildiriminin yönergeleri göz önünde bulundurululur. Bu, farklı parçalardan farklı anlamlara sahip olan aynı tanımlayıcıya neden olabilir:
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

Bir sınıfın üyeleri, *class_member_declaration*s tarafından tanıtılan üyelerden ve doğrudan temel sınıftan devralınan üyelere oluşur.

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

Bir sınıf türünün üyeleri aşağıdaki kategorilere ayrılmıştır:

*  Sınıfla ilişkili sabit değerleri temsil eden sabitler ([sabitler](classes.md#constants)).
*  Sınıfının değişkenleri olan alanlar ([alanlar](classes.md#fields)).
*  Sınıfı tarafından gerçekleştirilebilecek hesaplamalar ve eylemleri uygulayan Yöntemler ([Yöntemler](classes.md#methods)).
*  Adlandırılmış özellikleri ve bu özellikleri okuma ve yazma ile ilişkili eylemleri tanımlayan Özellikler ([Özellikler](classes.md#properties)).
*  Sınıfı tarafından oluşturulabilecek bildirimleri tanımlayan Olaylar ([Olaylar](classes.md#events)).
*  Dizin oluşturucular, sınıf örneklerinin (sözdizimsel) diziler ([Dizin oluşturucular](classes.md#indexers)) ile aynı şekilde dizine eklenmesine izin verir.
*  İşleç, sınıfının örneklerine uygulanabilen ifade işleçlerini tanımlar ([işleçler](classes.md#operators)).
*  Sınıf örneklerinin başlatılması için gereken eylemleri uygulayan örnek oluşturucular ([örnek oluşturucular](classes.md#instance-constructors))
*  Sınıf örneklerinin kalıcı olarak atılmadan önce gerçekleştirilecek eylemleri uygulayan yok ediciler ([Yıkıcılar](classes.md#destructors)).
*  Sınıfının kendisini ([statik oluşturucular](classes.md#static-constructors)) başlatmak için gereken eylemleri uygulayan statik oluşturucular.
*  , Sınıfının yerel olan türlerini temsil eden türler ([Iç içe türler](classes.md#nested-types)).

Yürütülebilir kod içerebilen Üyeler topluca sınıf türünün *işlev üyeleri* olarak bilinir. Bir sınıf türünün işlev üyeleri, bu sınıf türünün Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturuculardır.

Bir *class_declaration* , yeni bir bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) oluşturur ve bu bildirim alanına yeni üyeler tanıtmak *class_declaration* tarafından hemen içerilen *class_member_declaration*s. *Class_member_declaration*s için aşağıdaki kurallar geçerlidir:

*  Örnek oluşturucular, Yıkıcılar ve statik oluşturucular, hemen kapsayan sınıf ile aynı ada sahip olmalıdır. Tüm diğer üyelerin, hemen kapsayan sınıfın adından farklı adlara sahip olması gerekir.
*  Bir sabit, alan, özellik, olay veya tür adı aynı sınıfta belirtilen diğer tüm üyelerin adlarından farklı olmalıdır.
*  Bir yöntemin adı aynı sınıfta belirtilen diğer tüm yöntemlerin adlarından farklı olmalıdır. Ayrıca, bir yöntemin imzası ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) aynı sınıfta belirtilen diğer tüm yöntemlerin imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki yöntem yalnızca tarafından `ref` farklı olan imzalara sahip olamaz ve `out`.
*  Bir örnek oluşturucusunun imzası, aynı sınıfta belirtilen diğer tüm örnek oluşturucuların imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki oluşturucunun, yalnızca `ref` ve `out`ile farklı imzaları bulunmayabilir.
*  Bir dizin oluşturucunun imzası aynı sınıfta belirtilen diğer tüm dizin oluşturucularının imzalarından farklı olmalıdır.
*  Bir işlecin imzası aynı sınıfta belirtilen diğer tüm işleçlerin imzalarından farklı olmalıdır.

Bir sınıf türünün devralınan üyeleri ([Devralma](classes.md#inheritance)) bir sınıfın bildirim alanının parçası değildir. Bu nedenle, türetilmiş bir sınıfın devralınan üye ile aynı ad veya imzaya sahip bir üyeyi bildirmesine izin verilir (yani, devralınan üyeyi etkiler).

### <a name="the-instance-type"></a>Örnek türü

Her sınıf bildiriminde ilişkili bir bağlı tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)), ***örnek türü***vardır. Genel sınıf bildiriminde, örnek türü, her bir sağlanan tür bağımsız değişkeni karşılık gelen tür parametresi olan tür bildiriminden oluşturulmuş bir tür ([oluşturulmuş türler](types.md#constructed-types)) oluşturularak oluşturulur. Örnek türü tür parametrelerini kullandığından, yalnızca tür parametrelerinin kapsamda olduğu durumlarda kullanılabilir; diğer bir deyişle, sınıf bildiriminin içinde. Örnek türü, sınıf bildiriminin içinde yazılmış `this` kod için türüdür. Genel olmayan sınıflar için örnek türü yalnızca belirtilen sınıftır. Aşağıda, örnek türleriyle birlikte çeşitli sınıf bildirimleri gösterilmektedir: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Oluşturulmuş türlerin üyeleri

Oluşturulmuş bir türün devralınmamış üyeleri, üye bildiriminde bulunan her bir *type_parameter* için, oluşturulan türdeki karşılık gelen *type_argument* yerine koyarak elde edilir. Değiştirme işlemi, tür bildirimlerinin anlam anlamını temel alır ve yalnızca metinsel değiştirme değildir.

Örneğin, genel sınıf bildirimi verildiğinde
```csharp
class Gen<T,U>
{
    public T[,] a;
    public void G(int i, T t, Gen<U,T> gt) {...}
    public U Prop { get {...} set {...} }
    public int H(double d) {...}
}
```
oluşturulan tür `Gen<int[],IComparable<string>>` aşağıdaki üyelere sahiptir:
```csharp
public int[,][] a;
public void G(int i, int[] t, Gen<IComparable<string>,int[]> gt) {...}
public IComparable<string> Prop { get {...} set {...} }
public int H(double d) {...}
```

Genel `a` sınıf bildirimindeki `Gen` üyenin türü " `T`iki boyutlu dizidir", bu nedenle yukarıdaki oluşturulan türdeki üyenin `a` türü "tek boyutlu bir dizinin ikiboyutludizisi`int`"veya `int[,][]`.

Örnek işlevi üyeleri içinde, türü `this` kapsayan bildirimin örnek türüdür ([örnek türü](classes.md#the-instance-type)).

Bir genel sınıfın tüm üyeleri herhangi bir kapsayan sınıftan tür parametrelerini doğrudan veya oluşturulmuş bir türün bir parçası olarak kullanabilir. Çalışma zamanında belirli bir kapalı oluşturulmuş tür ([açık ve kapalı türler](types.md#open-and-closed-types)) kullanıldığında, bir tür parametresinin her kullanımı, oluşturulan türe sağlanan gerçek tür bağımsız değişkeniyle değiştirilmiştir. Örneğin:
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

Bir sınıf, kendi doğrudan temel sınıf türünün üyelerini ***devralır*** . Devralma, bir sınıfın, temel sınıfın örnek oluşturucular, Yıkıcılar ve statik oluşturucular dışında dolaylı olarak kendi temel sınıf türünün tüm üyelerini içerdiği anlamına gelir. Devralmanın bazı önemli yönleri şunlardır:

*  Devralma geçişlidir. , `C` Öğesinden `B` `A` türetildiyse`C` ve öğesinden`A`türetildiyse, ' de belirtilen üyelerin yanı sıra içinde belirtilen üyeleri devralır. `B` `B`
*  Türetilmiş bir sınıf doğrudan temel sınıfını genişletir. Türetilmiş bir sınıf, devralananlara yeni üyeler ekleyebilir, ancak devralınmış bir üyenin tanımını kaldıramaz.
*  Örnek oluşturucular, Yıkıcılar ve statik oluşturucular devralınmaz, ancak diğer tüm Üyeler, belirtilen erişilebilirliği ([üye erişimi](basic-concepts.md#member-access)) ne olursa olsun. Ancak, kendilerine ait olan erişilebilirliğe bağlı olarak, devralınan Üyeler türetilmiş bir sınıfta erişilebilir olmayabilir.
*  Türetilmiş bir sınıf, aynı ad veya imzaya sahip yeni üyeler bildirerek devralınan üyeleri ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) de ***gizleyebilir*** . Ancak devralınan bir üyeyi gizlemenin o üyeyi kaldırmadığını unutmayın; yalnızca bu üyeyi doğrudan türetilmiş sınıf üzerinden erişilmez hale getirir.
*  Bir sınıf örneği, sınıfta ve temel sınıflarında belirtilen tüm örnek alanlarının bir kümesini içerir ve bir türetilmiş sınıf türünden herhangi bir temel sınıf türünden herhangi birine bir örtük dönüştürme ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) bulunur. Bu nedenle, bir türetilmiş sınıfın örneğine bir başvuru, temel sınıflarının herhangi birinin bir örneğine başvuru olarak kabul edilebilir.
*  Bir sınıf sanal yöntemleri, özellikleri ve Dizin oluşturucuyu bildirebilir ve türetilmiş sınıflar bu işlev üyelerinin uygulamasını geçersiz kılabilir. Bu, sınıfların bir işlev üyesi çağrısı tarafından gerçekleştirilen eylemlerde çok biçimli davranışlar sergilemesine olanak sağlar. Bu işlev üyesinin çağrıldığı örnek çalışma zamanı türüne bağlı olarak değişir.

Oluşturulmuş bir sınıf türünün devralınan üyesi,[](classes.md#base-classes) *içindeki karşılık gelen tür parametrelerinin her bir oluşumu için oluşturulan türün tür bağımsız değişkenlerini değiştirerek bulunan en hızlı temel sınıf türünün (temel sınıflar) üyeleridir. class_base* belirtimi. Bu Üyeler sırasıyla, üye bildirimindeki her bir *type_parameter* için yerine, *class_base* belirtiminin karşılık gelen *type_argument* tarafından dönüştürülür.

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

Yukarıdaki örnekte, `D<int>` oluşturulan türün tür parametresi `T`için tür bağımsız değişkeni `int` yerine devralınmamış `public int G(string s)` devralınmış bir üyeye sahiptir. `D<int>`Ayrıca, Sınıf bildiriminden `B`devralınan bir üyeye de sahiptir. Bu devralınan üye, ilk olarak temel sınıf `B<int[]>` belirtiminde `B<T[]>` `int` için `T` yerine koyarak temel `D<int>` sınıf türü belirlenerek belirlenir. Daha sonra, bir tür bağımsız değişkeni `B` `int[]` olarak ' de, `U` devralınan `public U F(long index)`üyeyi `public int[] F(long index)`oluşturan ' de için değiştirilir.

### <a name="the-new-modifier"></a>Yeni değiştirici

Bir *class_member_declaration* devralınan üye ile aynı ada veya imzaya sahip bir üyeyi bildirmesine izin verilir. Bu gerçekleştiğinde, türetilmiş sınıf üyesi temel sınıf üyesini ***gizleyecek*** şekilde söylenir. Devralınan bir üyenin gizlenmesi bir hata sayılmaz, ancak derleyicinin bir uyarı vermesine neden olur. Uyarıyı bastırmak için, türetilen sınıf üyesinin bildirimi, türetilen üyenin temel üyeyi gizlemek için `new` tasarlanan bir değiştirici içerebilir. Bu konu [Devralma yoluyla gizlenerek](basic-concepts.md#hiding-through-inheritance)daha ayrıntılı bir şekilde ele alınmıştır.

Bir `new` değiştirici devralınmış bir üyeyi gizlemez bir bildirime dahil edilir, bu etkiye bir uyarı verilir. Bu uyarı, `new` değiştirici kaldırılarak bastırılır.

### <a name="access-modifiers"></a>Erişim değiştiricileri

Bir *class_member_declaration* , olası beş tür erişilebilirliği ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility) `public`) herhangi birine sahip olabilir:, `protected` `protected internal`, `internal`, veya `private`. `protected internal` Birleşim haricinde, birden fazla erişim değiştiricisi belirtmek için derleme zamanı hatası olur. Bir *class_member_declaration* herhangi bir erişim değiştiricisi `private` içermiyorsa varsayılır.

### <a name="constituent-types"></a>Anayent türleri

Bir üyenin bildiriminde kullanılan türler, o üyenin bileşen türleri olarak adlandırılır. Olası yapısal türler, bir sabit, alan, özellik, olay veya dizin oluşturucunun türü, bir yöntemin veya işlecin dönüş türü ve bir yöntem, Dizin Oluşturucu, işleç veya örnek oluşturucusunun parametre türleri olabilir. Üyenin bileşen türleri en az o üyenin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

### <a name="static-and-instance-members"></a>Statik ve örnek üyeleri

Bir sınıfın üyeleri ***statik Üyeler*** veya ***örnek üyeleridir***. Genel olarak, statik üyeleri nesnelere ait olan sınıf türlerine ve örnek üyelerine (sınıf türü örnekleri) ait olacak şekilde düşünmek yararlı olur.

Bir alan, yöntem, özellik, olay, işleç veya Oluşturucu bildirimi bir `static` değiştirici içerdiğinde, statik bir üye bildirir. Ayrıca, bir sabit veya tür bildirimi dolaylı olarak statik bir üye bildirir. Statik Üyeler aşağıdaki özelliklere sahiptir:

*  Formun `M` `M` `E` [](expressions.md#member-access)bir member_access (üye erişimi) içinde bir statik üyeye başvuruluyorsa, içeren bir türü belirtmelidir. `E.M` Bir örneği göstermek için derleme zamanı hatasıdır `E` .
*  Statik bir alan, belirli bir kapalı sınıf türünün tüm örnekleri tarafından paylaşılacak tam olarak bir depolama konumunu tanımlar. Belirli bir kapalı sınıf türünün kaç örneğinin oluşturulduğuna bakılmaksızın, bir statik alanın yalnızca bir kopyası vardır.
*  Statik işlev üyesi (yöntem, özellik, olay, işleç veya Oluşturucu) belirli bir örnek üzerinde çalışmaz ve bu tür bir işlev üyesinde başvurmak `this` için derleme zamanı hatası olur.

Bir alan, yöntem, özellik, olay, Dizin Oluşturucu, Oluşturucu veya yıkıcı bildirimi bir `static` değiştirici içermiyorsa, bir örnek üyesi bildirir. (Bir örnek üyesi bazen statik olmayan bir üye olarak adlandırılır.) Örnek üyeleri aşağıdaki özelliklere sahiptir:

*  Formun `M` `M` `E` [](expressions.md#member-access)bir member_access (üye erişimi) içinde bir örnek üyesine başvurulduğunda, içeren bir türün örneğini belirtmelidir. `E.M` Bir tür belirtmek `E` için bağlama zamanı hatası olur.
*  Bir sınıfın her örneği, sınıfının tüm örnek alanlarını ayrı bir kümesini içerir.
*  Bir örnek işlev üyesi (yöntem, özellik, Dizin Oluşturucu, örnek Oluşturucu veya yıkıcı), sınıfın belirli bir örneği üzerinde çalışır ve bu örneğe ( `this` [Bu erişim](expressions.md#this-access)) olarak erişilebilir.

Aşağıdaki örnekte statik ve örnek üyelerine erişim kuralları gösterilmektedir:
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

Yöntemi bir örnek işlev üyesinde, hem örnek üyelerine hem de statik üyelere erişmek için bir *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılabileceğini gösterir. `F` Yöntemi, bir statik işlev üyesinde, bir simple_name aracılığıyla örnek üyesine erişmek için derleme zamanı hatası olduğunu gösterir. `G` Yöntemi, bir member_access ([üye erişimi](expressions.md#member-access)) içinde, örnek üyelerine örnekler aracılığıyla erişilmesi ve statik üyelerin türler aracılığıyla erişilmesi gerekir. `Main`

### <a name="nested-types"></a>İç içe türler

Bir sınıf veya yapı bildiriminde belirtilen türe ***iç içe geçmiş tür***denir. Bir derleme birimi veya ad alanı içinde belirtilen bir tür, ***iç içe olmayan bir tür***olarak adlandırılır.

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
sınıf, sınıf `A`içinde bildirildiği için iç içe bir tür ve sınıf `A` , derleme birimi içinde bildirildiği için iç içe olmayan bir tür. `B`

#### <a name="fully-qualified-name"></a>Tam nitelikli ad

İç içe bir tür `S.N` `S` için tam nitelikli ad ([tam adlar](basic-concepts.md#fully-qualified-names)), türün `N` bildirildiği türün tam nitelikli adıdır.

#### <a name="declared-accessibility"></a>Tanımlanan erişilebilirlik

İç içe olmayan türler erişilebilirliği içerebilir `public` veya `internal` belirtebilir ve `internal` varsayılan olarak erişilebilirliği bildirmişti. İç içe türler, kapsayan türün bir sınıf veya yapı olmasına bağlı olarak, bu tanımlanmış erişilebilirlik ve bir veya daha fazla tanımlanmış erişilebilirlik biçimi içerebilir:

*  Bir sınıfta bildirildiği iç içe yerleştirilmiş bir tür, belirtilen beş`public`erişilebilirliği ( `protected internal`, `protected`, `internal`,, `private` veya `private`) ve diğer sınıf üyeleri gibi, varsayılan olarak bildirildiği larınızdaki.
*  Bir yapıda tanımlanmış iç içe yerleştirilmiş bir tür, üç farklı yapı erişilebilirliği`public`(, `internal`, veya `private`) ve diğer yapı üyeleri gibi, varsayılan `private` olarak belirtilen erişilebilirliği kullanabilirler.

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
özel bir iç içe sınıf `Node`bildirir.

#### <a name="hiding"></a>Larını

İç içe yerleştirilmiş bir tür, temel üyeyi gizleyebilir ([ad gizleyerek](basic-concepts.md#name-hiding)). `new` Değiştiriciye iç içe geçmiş tür bildirimlerinde izin verilir, böylece gizleme açıkça ifade edilebilir. Örnek
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
içinde `M` `M` tanımlanan yöntemi gizleyen iç içe bir sınıfı gösterir. `Base`

#### <a name="this-access"></a>Bu erişim

İç içe bir tür ve kapsayan türü, *This_Access* ([Bu erişim](expressions.md#this-access)) ile ilgili özel bir ilişkiye sahip değildir. Özellikle, `this` iç içe bir tür içinde, kapsayan türün örnek üyelerine başvurmak için kullanılamaz. İç içe bir türün, kapsayan türün örnek üyelerine erişmesi gereken durumlarda, kapsayan türün örneği için iç içe geçmiş türü için bir Oluşturucu `this` bağımsız değişkeni olarak, erişim sağlanabilir. Aşağıdaki örnek
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
Bu tekniği gösterir. `Nested` `this` `Nested`Örnek üyelerine sonraki erişimi `C` sağlamakiçinbirörneğioluştururve`C`kendi oluşturucusunu geçirir.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Kapsayan türdeki özel ve korunan üyelere erişim

İç içe bir tür, içeren `private` ve `protected` tarafından tanımlanan içerilen türün üyeleri de dahil olmak üzere, kapsayan türü için erişilebilen tüm üyelere erişebilir. Örnek
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
iç içe bir `C` sınıf `Nested`içeren bir sınıfı gösterir. İçinde `Nested`, yöntemi `G` içinde `F` `F` tanımlanan statik yöntemiçağırırveözelolaraktanımlanmışerişilebilirliğivardır.`C`

İç içe yerleştirilmiş bir tür, kapsayan türünün temel türünde tanımlanan korumalı üyelere de erişebilir. Örnekte
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
iç içe yerleştirilmiş `Derived.Nested` sınıf, öğesinin `F` `Base` `Derived` bir`Derived`örneği aracılığıyla çağırarak temel sınıfında tanımlanan korumalı yönteme erişir.

#### <a name="nested-types-in-generic-classes"></a>Genel sınıflarda iç içe türler

Genel sınıf bildirimi, iç içe geçmiş tür bildirimleri içerebilir. Kapsayan sınıfın tür parametreleri iç içe türler içinde kullanılabilir. İç içe geçmiş tür bildirimi, yalnızca iç içe geçmiş tür için uygulanan ek tür parametreleri içerebilir.

Genel sınıf bildiriminde yer alan her tür bildirimi örtük olarak genel bir tür bildirimidir. Genel bir tür içinde iç içe geçmiş bir türe başvuru yazarken, türü bağımsız değişkenleri dahil olmak üzere bulunan oluşturulmuş tür, adlandırılmış olmalıdır. Ancak, dış sınıfın içinden iç içe geçmiş tür, nitelendirme olmadan kullanılabilir; Dış sınıfın örnek türü, iç içe geçmiş türü oluşturulurken örtük olarak kullanılabilir. Aşağıdaki örnek, öğesinden `Inner`oluşturulan oluşturulmuş bir türe başvurmak için üç farklı doğru yolu gösterir; ilk ikisi eşdeğerdir:
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

Hatalı programlama stili olsa da, iç içe bir türdeki tür parametresi, dış türde belirtilen bir üyeyi veya tür parametresini gizleyebilir:
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

Temel C# çalışma zamanı uygulamasını kolaylaştırmak için, bir özellik, olay veya Dizin Oluşturucu olan her kaynak üye bildirimi için, uygulama, üye bildirimi türüne, adına ve türüne göre iki yöntem imzasını ayırmalıdır. Bu, temeldeki çalışma zamanı uygulamasının bu ayırmaları kullanmasa bile, imzası bu ayrılmış imzalardan biriyle eşleşen bir üyeyi bildirmek için bir derleme zamanı hatasıdır.

Ayrılmış adlar bildirimleri sunmaz, bu nedenle üye aramasına katılmaz. Ancak, bir bildirimin ilişkili ayrılmış yöntem imzaları devralmaya ([Devralma](classes.md#inheritance)) katılır ve `new` değiştiriciyle ([Yeni değiştirici](classes.md#the-new-modifier)) gizlenebilir.

Bu adların ayırması üç amaca hizmet eder:

*  Temel uygulamanın C# dil özelliğine Get veya set erişimi için bir yöntem adı olarak sıradan bir tanımlayıcı kullanmasına izin vermek için.
*  Diğer dillerin, C# dil özelliğine Get veya set erişimi için bir yöntem adı olarak sıradan bir tanımlayıcı kullanarak birlikte çalışabilme izni vermek için.
*  Tek bir uygun derleyici tarafından kabul edilen kaynağın, ayrılmış üye adlarının özelliklerinin tüm C# uygulamalarda tutarlı olduğundan emin olmak için.

Yıkıcı ([Yıkıcılar](classes.md#destructors)) bildirimi de bir imzanın ayrılmasına neden olur (yok[ediciler için üye adları ayrılmıştır](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Özellikler için ayrılan üye adları

Türünde `P` bir`T`Özellik ([Özellikler](classes.md#properties)) için aşağıdaki imzalar ayrılmıştır:

```csharp
T get_P();
void set_P(T value);
```

Özellik salt okunurdur veya salt yazılır olsa bile her iki imza de ayrılır.

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
bir sınıf `A` salt okunurdur bir özelliği `P`tanımlar, böylece imzaları ve `set_P` yöntemleri için `get_P` ayırırsınız. Bir sınıf `B` ' dan `A` türetilir ve bu ayrılmış imzaların her ikisini birden gizler. Örnek, çıktıyı üretir:
```
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Olaylar için ayrılan üye adları

Temsilci türünün `E` [](classes.md#events) birolayı(olayları)içinaşağıdakiimzalar`T`ayrılmıştır:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Dizin oluşturucular için ayrılan üye adları

Parametre-listesi `T` `L`olan türdeki bir Dizin Oluşturucu ([Dizin oluşturucular](classes.md#indexers)) için aşağıdaki imzalar ayrılmıştır:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Dizin Oluşturucu salt okunurdur veya salt yazılır olsa bile her iki imza da ayrılır.

Ayrıca üye adının `Item` ayrılmış olması.

#### <a name="member-names-reserved-for-destructors"></a>Yok ediciler için ayrılan üye adları

Yıkıcı ([Yıkıcılar](classes.md#destructors)) içeren bir sınıf için aşağıdaki imza ayrılmıştır:
```csharp
void Finalize();
```

## <a name="constants"></a>Sabitler

***Sabit*** değeri, derleme zamanında hesaplanılabilen bir değer olan sabit bir değeri temsil eden bir sınıf üyesidir. Bir *constant_declaration* , belirli bir türün bir veya daha fazla sabitlerini tanıtır.

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

Bir *constant_declaration* , `new` bir dizi *öznitelik* ([öznitelik](attributes.md)), değiştirici ([Yeni değiştirici](classes.md#the-new-modifier)) ve dört erişim değiştiricisinin geçerli bir birleşimini ([erişim değiştiricileri](classes.md#access-modifiers)) içerebilir. Öznitelikler ve değiştiriciler, *constant_declaration*tarafından belirtilen tüm Üyeler için geçerlidir. Sabitler statik üye olarak kabul edilse de, bir *constant_declaration* gerektirmez veya bir `static` değiştiriciye izin vermez. Aynı değiştiricinin Sabit bildiriminde birden çok kez görünmesi hatadır.

Bir *constant_declaration* *türü* , bildirim tarafından tanıtılan üyelerin türünü belirtir. Türün ardından, her biri yeni bir üye tanıtan bir *constant_declarator*s listesi gelir. Bir *constant_declarator* ,`=`üyeyi ve ardından bir "" belirteci ve sonra üyenin değerini veren bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) gelen bir *tanımlayıcıdan* oluşur.

`byte`Sabit bildiriminde `sbyte`belirtilen *tür* `short` ,`char`,,, ,`uint`,, ,`float`,,,,,,, olmalıdır `long` `ushort` `int` `ulong` `double` ,,`decimal`,, bir enum_type veya reference_type. `string` `bool` Her bir *constant_expression* , hedef türünde bir değer veya örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tarafından hedef türüne dönüştürülebilen bir tür değeri vermelidir.

Bir sabit *türünün* en az sabitin ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olması gerekir.

Bir sabitin değeri, bir *simple_name* ([basit adlar](expressions.md#simple-names)) veya *member_access* ([üye erişimi](expressions.md#member-access)) kullanılarak bir ifadede elde edilir.

Bir sabit, bir *constant_expression*katılabilir. Bu nedenle, bir sabit, *constant_expression*gerektiren herhangi bir yapı içinde kullanılabilir. Bu yapılara `case` örnek olarak Etiketler, `goto case` deyimler, `enum` üye bildirimleri, öznitelikler ve diğer sabit bildirimler verilebilir.

[Sabit ifadelerde](expressions.md#constant-expressions)açıklandığı gibi, *constant_expression* derleme zamanında tam olarak değerlendirilebilen bir ifadedir. ' Den `string` farklı bir *reference_type* ' in null olmayan bir değerini oluşturmanın tek yolu `new` işleci uygulamaktır ve `new` bir *constant_expression*içinde işlecine izin verilmediğinden, için olası tek değer dışındaki *reference_type*s `string` `null`sabitleri.

Sabit bir değer için sembolik bir ad istendiğinde, ancak bu değerin türü sabit bildirimde izin verilmediğinde veya değer derleme zamanında bir *constant_expression*tarafından hesaplanmadığında, bir `readonly` alan ([salt okunur alanlar ](classes.md#readonly-fields)) bunun yerine kullanılabilir.

Birden çok sabiti bildiren bir sabit bildirim, aynı özniteliklere, Değiştiricilere ve türe sahip tek sabitlerin birden çok bildirimi ile eşdeğerdir. Örneğin:
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

Sabitler dairesel bir şekilde olmadığı sürece sabitlerin aynı program içindeki diğer sabitlere bağlı olmasına izin verilir. Derleyici, sabit bildirimleri uygun sırada değerlendirmek için otomatik olarak düzenler. Örnekte
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
derleyici ilk `A.Y`olarak değerlendirir, `A.X` `B.Z`sonra, ve `11` `10` değerlerini`12`üretir, ve değerlerini değerlendirir. Sabit bildirimler diğer programlardaki sabitlere bağlı olabilir, ancak bu tür bağımlılıklar yalnızca tek bir yönde mümkündür. Yukarıdaki örneğe başvurarak, `A` ve ayrı programlarda bildirilirse, ' nin bağlı olması ve `B` daha sonra bağımlı `A.Y`olması `B.Z`mümkün `A.X` `B.Z` olacaktır.

## <a name="fields"></a>Alanlar

***Alan*** , bir nesne veya sınıfla ilişkili bir değişkeni temsil eden bir üyesidir. Bir *field_declaration* , belirli bir türün bir veya daha fazla alanını tanıtır.

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

Bir *field_declaration* , `static` bir dizi *öznitelik* ( `new` [öznitelik](attributes.md)), değiştirici ([Yeni değiştirici](classes.md#the-new-modifier)), dört erişim değiştiricisinin geçerli bir birleşimini ([erişim değiştiricileri](classes.md#access-modifiers)) ve bir değiştirici ([statik ve örnek alanları](classes.md#static-and-instance-fields)). Ayrıca, bir *field_declaration* `readonly` bir değiştirici ([salt okunur alanlar](classes.md#readonly-fields)) veya bir `volatile` değiştirici ([geçici alanlar](classes.md#volatile-fields)) içerebilir, ancak ikisini birden içeremez. Öznitelikler ve değiştiriciler, *field_declaration*tarafından belirtilen tüm Üyeler için geçerlidir. Aynı değiştiricinin bir alan bildiriminde birden çok kez görünmesi hatadır.

Bir *field_declaration* *türü* , bildirim tarafından tanıtılan üyelerin türünü belirtir. Türün ardından, her biri yeni bir üye tanıtan bir *variable_declarator*s listesi gelir. Bir *variable_declarator* , bu üyeyi belirten, isteğe bağlı`=`olarak "" belirteci ve bu üyenin ilk değerini veren bir *variable_initializer* ([değişken başlatıcıları](classes.md#variable-initializers)) içeren bir *tanımlayıcıdan* oluşur.

Alanın *türü* en az alanın kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir alanın değeri, bir *simple_name* ([basit adlar](expressions.md#simple-names)) veya *member_access* ([üye erişimi](expressions.md#member-access)) kullanılarak bir ifadede elde edilir. Salt okunur olmayan bir alanın değeri, *atama* ([atama işleçleri](expressions.md#assignment-operators)) kullanılarak değiştirilir. Salt okunur olmayan bir alanın değeri, sonek artırma ve azaltma işleçleri ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve önek artırma ve azaltma işleçleri kullanılarak elde edilebilir ve değiştirilebilir ([önek artışı ve azaltma işleçler](expressions.md#prefix-increment-and-decrement-operators)).

Birden çok alanı bildiren bir alan bildirimi, aynı özniteliklere, Değiştiricilere ve türe sahip tek alanlara ait birden çok bildirime eşdeğerdir. Örneğin:
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

Bir alan bildirimi bir `static` değiştirici içerdiğinde, bildirim tarafından tanıtılan alanlar ***statik alanlardır***. Hiçbir `static` değiştirici yoksa, bildirim tarafından tanıtılan alanlar ***örnek alanlardır***. Statik alanlar ve örnek alanları, C#tarafından desteklenen çeşitli değişken ([değişken](variables.md)) çeşitleridir ve bunlar sırasıyla ***statik değişkenler*** ve ***örnek değişkenler***olarak adlandırılır.

Statik alan, belirli bir örneğin bir parçası değildir; Bunun yerine, kapalı bir türün ([açık ve kapalı türler](types.md#open-and-closed-types)) tüm örnekleri arasında paylaşılır. Kapalı bir sınıf türünün kaç örneğinin oluşturulduğuna bakılmaksızın, ilişkili uygulama etki alanı için yalnızca bir statik alan kopyası vardır.

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

Örnek alanı bir örneğe aittir. Özellikle, bir sınıfın her örneği, bu sınıfın tüm örnek alanlarının ayrı bir kümesini içerir.

Bir alan, `E.M` `M` `M` `E` formunbirmember_access(üyeerişimi)içindebaşvuruluyorsa,birstatikalandır,içerenbirtürübelirtmekvebirörnekalanıise,E'nin`M` [](expressions.md#member-access) içeren `M`türün bir örneğini gösterir.

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="readonly-fields"></a>ReadOnly alanları

Bir *field_declaration* , bir `readonly` değiştirici içerdiğinde, bildirim tarafından tanıtılan alanlar ***salt okunur alanlardır***. Salt okunur alanlara doğrudan atamalar yalnızca bu bildirimin bir parçası veya aynı sınıftaki bir örnek Oluşturucu ya da statik oluşturucu içinde olabilir. (Salt okunur bir alan, bu bağlamlarda birden çok kez atanabilir.) Özellikle, bir `readonly` alana doğrudan atamalara yalnızca aşağıdaki bağlamlarda izin verilir:

*  Alanı tanıtan *variable_declarator* (bildirime bir *variable_initializer* ekleyerek).
*  Bir örnek alanı için, alan bildirimini içeren sınıfın örnek oluşturucularında; statik bir alan için, alan bildirimini içeren sınıfın statik oluşturucusunda. Bunlar ayrıca, bir `readonly` alanı `out` veya `ref` parametresi olarak geçirmek için geçerli olduğu tek bağlamlardır.

Bir `readonly` alana atamaya veya diğer bağlamdaki bir `out` veya `ref` parametresi olarak geçirmeye çalışılması derleme zamanı hatasıdır.

#### <a name="using-static-readonly-fields-for-constants"></a>Sabitler için statik salt okunur alanlar kullanma

Bir `static readonly` alan, bir sabit değer için simgesel ad istendiği, ancak değer türüne `const` bildirimde izin verilmediğinde veya değer derleme zamanında hesaplanmadığında faydalıdır. Örnekte
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
,,`White`, ve üyeleri,`Green`değerleriderlemezamanında hesaplanamadığından `const` üye olarak bildirilemez. `Red` `Blue` `Black` Ancak, bunun yerine `static readonly` bunları bildirmek çok aynı etkiye sahiptir.

#### <a name="versioning-of-constants-and-static-readonly-fields"></a>Sabitler ve statik salt okunur alanların sürümü oluşturma

Sabitler ve salt okunur alanlarında farklı ikili sürüm oluşturma semantiği vardır. Bir ifade bir sabit değere başvurduğunda, sabit değer derleme zamanında elde edilir, ancak bir ifade salt okunur bir alana başvurduğunda alanın değeri çalışma zamanına kadar elde edilmez. İki ayrı programda oluşan bir uygulamayı düşünün:
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

`Program1` Ve`Program2` ad alanları ayrı olarak derlenen iki programı gösterir. Statik bir salt okunur alan olarak bildirildiği için, `Console.WriteLine` deyimin çıktı değeri derleme zamanında bilinmiyor, ancak bunun yerine çalışma zamanında elde edilir. `Program1.Utils.X` Bu `X` nedenle, değeri değiştirilirse ve `Program1` yeniden derlense, `Console.WriteLine` `Program2` ifade yeniden derlenmemişse bile yeni değeri çıktı olarak çıkar. Ancak, bir `X` sabit `X` değer vardı, değeri derlenmiş durumda alınır `Program2` ve yeniden derlenene kadar `Program2` ' deki `Program1` değişikliklerden etkilenmemiştir.

### <a name="volatile-fields"></a>Geçici alanlar

Bir *field_declaration* , bir `volatile` değiştirici içerdiğinde, bu bildirim tarafından tanıtılan alanlar ***geçici alanlardır***.

Geçici olmayan alanlar için, yönergeleri yeniden sipariş eden iyileştirme teknikleri, *lock_statement* tarafından sağlanmış gibi eşitlemeye gerek olmadan alanlara erişen çok iş parçacıklı programlarda beklenmedik ve öngörülemeyen sonuçlara yol[açabilir ( Lock deyimleri](statements.md#the-lock-statement)). Bu iyileştirmeler derleyici tarafından, çalışma zamanı sistemine veya donanımla gerçekleştirilebilir. Geçici alanlar için, bu tür yeniden sıralama iyileştirmeleri kısıtlanmıştır:

*  Geçici bir alan okuma, ***geçici okuma***olarak adlandırılır. Geçici okuma "alma semantiğini" içerir; diğer bir deyişle, yönerge dizisinde bundan sonra meydana gelen tüm bellek başvurularından önce gerçekleşmesi garanti edilir.
*  Geçici bir alan yazma, ***geçici yazma***olarak adlandırılır. Geçici yazma "yayın semantiğini" içerir; diğer bir deyişle, yönerge dizisindeki yazma yönergesinden önce herhangi bir bellek başvurularından sonra gerçekleşmesi garanti edilir.

Bu kısıtlamalar, tüm iş parçacıklarının gerçekleştirilen diğer bir iş parçacığı tarafından gerçekleştirilen geçici yazmaları gözlemleyecek şekilde tüm iş parçacıkları tarafından gerçekleştirildiğinden emin olur. Tüm yürütme iş parçacıklarında görüldüğü şekilde, geçici yazma işlemlerinin tek toplam sıralamasını sağlamak için uygun bir uygulama gerekmez. Geçici bir alanın türü aşağıdakilerden biri olmalıdır:

*  Bir *reference_type*.
*  `byte`Türü ,`char` ,,`System.UIntPtr`,, ,,`bool`,, ,`System.IntPtr`veya. `uint` `float` `sbyte` `short` `ushort` `int`
*  `byte` ,`sbyte` ,,`uint`, Veya sabit listesi temel türüne sahip bir enum_type. `short` `ushort` `int`

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

Bu örnekte, yöntemi `Main` yöntemini `Thread2`çalıştıran yeni bir iş parçacığı başlatır. Bu yöntem `result`, bir değeri geçici olmayan bir alana depolar, ardından geçici alanda `finished`depolar `true` . Ana iş parçacığı alanın `finished` olarak `true`ayarlanması için bekler, sonra alanı `result`okur. Bu `finished` yana bildirildiği `volatile`için, ana iş parçacığının alandan `result`değeri `143` okuması gerekir. Alan `finished` bildirilmemiş `volatile`ise mağaza `result` 'dan `0` sonra `finished`ana iş parçacığında görünür olması ve bu nedenle ana iş parçacığının bu değerden değeri okuması için izin verilebilir. alan `result`. Alan olarak bildirmek `finished` , bu tür tutarsızlığın yapılmasını engeller. `volatile`

### <a name="field-initialization"></a>Alan başlatma

Bir alanın başlangıçtaki değeri, bir statik alan veya örnek alanı olsun, alanın türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)). Bu varsayılan başlatma gerçekleştirilmeden önce bir alanın değerini gözlemlemek mümkün değildir ve bir alan bu nedenle hiçbir şekilde "başlatılmamış" olur. Örnek
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
`b` ve`i` her ikisi de otomatik olarak varsayılan değerlere başlatılmıştı.

### <a name="variable-initializers"></a>Değişken başlatıcıları

Alan bildirimlerinde *variable_initializer*s bulunabilir. Statik alanlar için, değişken başlatıcıları sınıf başlatma sırasında yürütülen atama ifadelerine karşılık gelir. Örnek alanları için, değişken başlatıcıları, sınıfının bir örneği oluşturulduğunda yürütülen atama ifadelerine karşılık gelir.

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
statik alan başlatıcılarının yürütmesi ve `i` atamaları `s` , örnek alanı başlatıcıları çalıştırıldığında meydanagelir.`x`

[Alan başlatma](classes.md#field-initialization) bölümünde açıklanan varsayılan değer başlatma, değişken başlatıcıları olan alanlar da dahil olmak üzere tüm alanlar için gerçekleşir. Bu nedenle, bir sınıf başlatıldığında, bu sınıftaki tüm statik alanlar ilk olarak varsayılan değerlerine başlatılır ve statik alan başlatıcıları, metinsel sırada yürütülür. Benzer şekilde, bir sınıf örneği oluşturulduğunda, söz konusu örnekteki tüm örnek alanları ilk olarak varsayılan değerlerine başlatılır ve sonra örnek alanı başlatıcıları, metinsel sırada yürütülür.

Değişken başlatıcıların varsayılan değer durumunda gözlenecek statik alanlar mümkündür. Ancak, bu stil önemli bir şekilde önerilmez. Örnek
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
Bu davranışı sergiler. A ve b 'nin dairesel tanımlarına rağmen program geçerli olur. Çıkışa neden olur
```
a = 1, b = 2
```
statik alanlar `a` ve `b` , başlatıcıları yürütülmeden önce ( `0` için `int`varsayılan değer) olarak başlatılır. Başlatıcısı `a` çalıştırıldığında `1`değeri sıfır olduğunda`a` , olarak başlatılır. `b` `b` Başlatıcısı çalıştırıldığında, `a` değeri zaten `1` olurve`2`olarakbaşlatılır. `b`

#### <a name="static-field-initialization"></a>Statik alan başlatma

Bir sınıfın statik alan değişkeni başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülen atama dizisine karşılık gelir. Sınıfında statik bir Oluşturucu ([statik oluşturucular](classes.md#static-constructors)) varsa, statik alan başlatıcılarının yürütülmesi bu statik oluşturucunun yürütülmesi için hemen önce gerçekleşir. Aksi takdirde, statik alan başlatıcıları, bu sınıfın statik alanı ilk kullanılmadan önce uygulamayla ilgili bir süre içinde yürütülür. Örnek
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
çıktıyı üretebilir:
```
Init A
Init B
1 1
```
veya çıkış:
```
Init B
Init A
1 1
```
`X`başlatıcısının ve `Y`başlatıcısının yürütülmesi sırayla gerçekleşebileceğinden, bu alanlar yalnızca bu alanlara başvurularından önce gerçekleşirler. Ancak, örnekte:
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
çıkışın olması gerekir:
```
Init B
Init A
1 1
```
statik oluşturucuların çalıştırıldığı zaman ( [statik oluşturucularda](classes.md#static-constructors)tanımlandığı gibi) için kurallar statik oluşturucunun (ve `B`bu nedenle `B` `A`statik alan başlatıcılarının) statik olarak çalıştırılması gerektiğini sağlar Oluşturucu ve alan başlatıcıları.

#### <a name="instance-field-initialization"></a>Örnek alanı başlatma

Bir sınıfın örnek alanı değişken başlatıcıları, bu sınıfın örnek oluşturucularından ([Oluşturucu başlatıcıların](classes.md#constructor-initializers)) herhangi birine girişte hemen yürütülen atama dizisine karşılık gelir. Değişken başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülür. Sınıf örneği oluşturma ve başlatma işlemi, [örnek oluşturucularda](classes.md#instance-constructors)daha ayrıntılı olarak açıklanmıştır.

Örnek alanı için değişken Başlatıcısı oluşturulan örneğe başvuramaz. Bu nedenle, bir değişken başlatıcısında başvurmak `this` için bir derleme zamanı hatası, bir değişken başlatıcısı için bir *simple_name*aracılığıyla herhangi bir örnek üyesine başvuruda bulunmak üzere bir derleme zamanı hatası olduğundan. Örnekte
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
için `y` değişken başlatıcısı, oluşturulmakta olan Örneğin bir üyesine başvurduğundan derleme zamanı hatası ile sonuçlanır.

## <a name="methods"></a>Yöntemler

Bir ***Yöntem*** , bir nesne veya sınıf tarafından gerçekleştirilebilecek bir hesaplama veya eylem uygulayan bir üyesidir. Yöntemler *method_declaration*s kullanılarak bildirilmiştir:

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

Bir *method_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)) ve dört erişim değiştiricisinin geçerli bir birleşimini içerebilir ([erişim değiştiricileri](classes.md#access-modifiers)) `new` , ([Yeni değiştirici](classes.md#the-new-modifier)) `static` , ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ( `sealed` `abstract` [geçersiz kılma yöntemleri](classes.md#override-methods)), ([korumalı Yöntemler](classes.md#sealed-methods)), ([soyut](classes.md#abstract-methods)Yöntemler) ve `extern`([Dış Yöntemler](classes.md#external-methods)) değiştiricileri.

Aşağıdakilerin tümü doğru ise bir bildirimin geçerli bir değiştiriciler birleşimi vardır:

*  Bildirim, geçerli bir erişim değiştiricileri ([erişim değiştiricileri](classes.md#access-modifiers)) birleşimini içerir.
*  Bildirim, aynı değiştiriciyi birden çok kez içermez.
*  Bildirim aşağıdaki değiştiricilerin en çok birini içerir: `static`, `virtual`ve `override`.
*  Bildirim aşağıdaki değiştiricilerin en çok birini içerir: `new` ve. `override`
*  Bildirim `abstract` değiştiricisini içeriyorsa, bildirim aşağıdaki değiştiricilerin hiçbirini içermez: `static`, `virtual`, `sealed` veya `extern`.
*  Bildirim `private` değiştiricisini içeriyorsa, bildirim aşağıdaki değiştiricilerin hiçbirini içermez: `virtual`, `override`, veya `abstract`.
*  Bildirim `sealed` değiştiricisini içeriyorsa, bildirim de `override` değiştirici içerir.
*  `partial` Bildirim değiştiricisini içeriyorsa, aşağıdaki değiştiricilerin hiçbirini içermez: `new`, `public`, `private` `internal` `protected`,,, `virtual`, `sealed`, `override` , `abstract`, veya `extern`.

`async` Değiştiriciye sahip bir yöntem zaman uyumsuz [işlevdir ve zaman uyumsuz işlevlerde](classes.md#async-functions)açıklanan kuralları izler.

Bir yöntem bildiriminin *Sbayrak* değeri, hesaplanan ve yöntemi tarafından döndürülen değerin türünü belirtir. , Yöntemin bir `void` değer döndürmediğinden, *Sbayrak* . Bildirim `partial` değiştiricisini içeriyorsa, dönüş türü olmalıdır `void`.

*MEMBER_NAME* , yöntemin adını belirtir. Yöntem açık arabirim üyesi uygulaması ([Açık arabirim üyesi uygulamalar](interfaces.md#explicit-interface-member-implementations)) değilse, *MEMBER_NAME* yalnızca bir *tanımlayıcıdır*. Açık arabirim üyesi uygulama için, *MEMBER_NAME* bir *interface_type* ve ardından bir`.` *tanımlayıcı*ile oluşur.

İsteğe bağlı *type_parameter_list* , yöntemin tür parametrelerini belirtir ([tür parametreleri](classes.md#type-parameters)). Eğer bir *type_parameter_list* belirtilmişse, yöntemi ***genel bir yöntemdir***. Metodun bir `extern` değiştiricisi varsa, bir *type_parameter_list* belirtilemez.

İsteğe bağlı *formal_parameter_list* , yönteminin parametrelerini belirtir ([Yöntem parametreleri](classes.md#method-parameters)).

İsteğe bağlı *type_parameter_constraints_clause*s, bağımsız tür parametrelerinde ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) kısıtlamaları belirtir ve yalnızca bir *type_parameter_list* sağlanırsa ve yöntemin bir `override` değiştirici.

Bir metodun *formal_parameter_list* içinde başvurulan her bir *tür, en* azından yöntemin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

*Method_body* , bir noktalı virgül, bir ***deyim gövdesi*** veya bir ***ifade gövdesidir***. Deyim gövdesi, yöntemi çağrıldığında yürütülecek deyimleri belirten bir *bloğundan*oluşur. Bir ifade gövdesi, `=>` sonrasında bir *ifade* ve noktalı virgül ile oluşur ve yöntem çağrıldığında gerçekleştirilecek tek bir ifadeyi gösterir. 

Ve `abstract` yöntemleri`extern` için *method_body* yalnızca noktalı virgülle oluşur. Yöntemler `partial` için *method_body* , bir noktalı virgül, bir blok gövdesinden veya bir ifade gövdesinden oluşabilir. Diğer tüm yöntemler için, *method_body* bir blok gövdesidir veya bir ifade gövdesidir.

*Method_body* noktalı virgül içeriyorsa, bildirim `async` değiştiriciyi içermeyebilir.

Ad, tür parametre listesi ve bir yöntemin biçimsel parametre listesi metodun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar. Özellikle, bir yöntemin imzası adından, tür parametrelerinin sayısından ve biçimsel parametrelerinin sayısı, değiştiricilerinden ve türlerinden oluşur. Bu amaçlar için, bir biçimsel parametre türünde oluşan yöntemin tür parametresi, kendi adı tarafından değil, yönteminin tür bağımsız değişkeni listesindeki sıra konumuna göre tanımlanır. Dönüş türü yöntemin imzasının bir parçası değildir, ya da tür parametrelerinin ya da biçimsel parametrelerin adları değildir.

Bir yöntemin adı aynı sınıfta belirtilen diğer tüm yöntemlerin adlarından farklı olmalıdır. Ayrıca, bir yöntemin imzası aynı sınıfta belirtilen diğer tüm yöntemlerin imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki yöntem yalnızca `ref` ve `out`ile farklı imzalara sahip olamaz.

Yöntemin *type_parameter*s, *method_declaration*boyunca kapsamdadır ve bu kapsam genelinde tür ve *Sbayrak*, *method_body*ve *type_parameter_constraints_clause*s içindeki türleri oluşturmak için kullanılabilir *özniteliklerde*.

Tüm biçimsel parametrelerin ve tür parametrelerinin adları farklı olmalıdır.

### <a name="method-parameters"></a>Yöntem parametreleri

Bir yöntemin parametreler varsa, yöntemin *formal_parameter_list*tarafından tanımlanır.

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

Biçimsel parametre listesi, yalnızca en son bir *parameter_array*olabilecek bir veya daha fazla virgülle ayrılmış parametre içerir.

Bir *fixed_parameter* , isteğe bağlı bir *öznitelik* kümesi `out` ([öznitelikler](attributes.md)), isteğe `ref`bağlı veya `this` değiştirici, bir *tür*, *tanımlayıcı* ve isteğe bağlı bir *default_ içerir bağımsız değişken*. Her *fixed_parameter* verilen ada sahip verilen türdeki bir parametreyi bildirir. `this` Değiştirici yöntemi bir genişletme yöntemi olarak belirler ve yalnızca bir statik metodun ilk parametresinde kullanılabilir. Uzantı yöntemleri [uzantı yöntemlerinde](classes.md#extension-methods)daha ayrıntılı olarak açıklanmıştır.

*Default_argument* içeren bir *fixed_parameter* , ***isteğe bağlı bir parametre***olarak bilinir, ancak *default_argument* olmayan bir *fixed_parameter* ***gerekli bir parametredir***. Gerekli bir parametre, *formal_parameter_list*içinde isteğe bağlı bir parametreden sonra görünmeyebilir.

Veya parametresinin bir default_argument olamaz. `ref` `out` Bir *default_argument* içindeki *ifade* aşağıdakilerden biri olmalıdır:

*  bir *constant_expression*
*  `new S()` form`S` bir değer türü olan bir ifade
*  `default(S)` form`S` bir değer türü olan bir ifade

*İfade* , bir kimlikle örtük olarak dönüştürülebilir veya parametrenin türüne null olabilen dönüştürmeye sahip olmalıdır.

Bir uygulama kısmi yöntem bildiriminde ([kısmi Yöntemler](classes.md#partial-methods)) isteğe bağlı parametreler oluşursa, açık bir arabirim üye uygulaması ([Açık arabirim üyesi uygulamalar](interfaces.md#explicit-interface-member-implementations)) veya tek parametreli Dizin Oluşturucu bildiriminde ([ Dizin oluşturucular](classes.md#indexers)) derleyici bir uyarı vermelidir, çünkü bu Üyeler asla bağımsız değişkenlerin atlanmasına izin vermek için hiçbir şekilde çağrılamaz.

Bir *parameter_array* , isteğe bağlı bir *öznitelikler* kümesinden ([öznitelikler](attributes.md)), bir `params` değiştiriciye, bir *array_type*ve bir *tanımlayıcıdan*oluşur. Bir parametre dizisi verilen bir ada sahip belirtilen dizi türünün tek bir parametresini bildirir. Bir parametre dizisinin *array_type* bir tek boyutlu dizi türü ([dizi türleri](arrays.md#array-types)) olmalıdır. Bir yöntem çağrısında, bir parametre dizisi verilen dizi türünün tek bir bağımsız değişkeninin belirtilmesine izin verir veya dizi öğe türünde sıfır veya daha fazla bağımsız değişken belirtilmesine izin verir. Parametre dizileri, [parametre dizileri](classes.md#parameter-arrays)içinde daha ayrıntılı olarak açıklanmıştır.

Bir *parameter_array* isteğe bağlı bir parametreden sonra gerçekleşebilir, ancak varsayılan bir değere sahip olamaz; bunun yerine bir *parameter_array* için bağımsız değişkenlerin atlanmasından sonra boş bir dizi oluşturulmasına neden olur.

Aşağıdaki örnek farklı parametre türlerini göstermektedir:
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

İçin `d` *formal_parameter_list,* gerekli bir başvuru parametresidir `b`, `s`,,, ve`t`isteğebağlıdeğerparametreleridir `o` `M` `i` ve `a` bir parametre dizisidir.

Yöntem bildirimi parametreler, tür parametreleri ve yerel değişkenler için ayrı bir bildirim alanı oluşturur. Adlar, bu bildirim alanına tür parametresi listesi ve yöntemin biçimsel parametre listesi ve yöntemin *bloğundaki* yerel değişken bildirimleri tarafından tanıtılmıştır. Bir yöntem bildirim alanının iki üyesinin aynı ada sahip olması için bir hatadır. Aynı ada sahip öğeleri içermesi için bir iç içe geçmiş bildirim alanının yöntem bildirim alanı ve yerel değişken bildirim alanı için bir hatadır.

Yöntem çağırma ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)), bu çağrıya özgü bir kopya oluşturur, bu, biçimsel parametreler ve yöntemin yerel değişkenleri ve çağrının bağımsız değişken listesi, yeni oluşturulan biçimsel değer veya değişken başvurularını atar parametrelere. Bir yöntem *bloğunun* içinde, biçimsel parametrelere *simple_name* ifadelerinde tanımlayıcıları ([basit adlar](expressions.md#simple-names)) tarafından başvurulabilir.

Dört tür biçimsel parametre vardır:

*  Herhangi bir değiştirici olmadan bildirildiği değer parametreleri.
*  `ref` Değiştiriciyle belirtilen başvuru parametreleri.
*  `out` Değiştirici ile belirtilen çıkış parametreleri.
*  `params` Değiştiriciyle belirtilen parametre dizileri.

[İmzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading) `ref` bölümünde açıklandığı gibi, ve `out` değiştiricileri yöntemin imzasının `params` bir parçasıdır, ancak değiştirici değildir.

#### <a name="value-parameters"></a>Değer parametreleri

Değiştirici olmadan belirtilen bir parametre bir değer parametresidir. Değer parametresi, yöntem çağrısında sağlanan karşılık gelen bağımsız değişkenden ilk değerini alan yerel bir değişkene karşılık gelir.

Bir biçimsel parametre bir değer parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) bir ifade olmalıdır.

Bir yöntem bir değer parametresine yeni değerler atamaya izin verilir. Bu atamalar yalnızca değer parametresi tarafından temsil edilen yerel depolama konumunu etkiler — Yöntem çağrısında verilen gerçek bağımsız değişkeni üzerinde hiçbir etkisi yoktur.

#### <a name="reference-parameters"></a>Başvuru parametreleri

`ref` Değiştirici ile belirtilen bir parametre bir başvuru parametresidir. Değer parametresinden farklı olarak, başvuru parametresi yeni bir depolama konumu oluşturmaz. Bunun yerine, başvuru parametresi, yöntem çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.

Bir biçimsel parametre bir başvuru parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, aynı bir `ref` *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) ve ardından gelen anahtar sözcüğünden oluşmalıdır biçimsel parametre olarak yazın. Bir değişken başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı.

Bir yöntem içinde, başvuru parametresi her zaman kesinlikle atanmış olarak değerlendirilir.

Yineleyici ([yineleyiciler](classes.md#iterators)) olarak belirtilen bir yöntemin başvuru parametreleri olamaz.

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

`Swap` İçinde `x` `i` çağrısı için, temsil eder ve`y` temsil eder .`j` `Main` Bu nedenle, çağırma, `i` ve `j`değerlerini değiştirme etkisine sahiptir.

Başvuru parametreleri alan bir yöntemde, birden fazla ad aynı depolama konumunu temsil etmek mümkündür. Örnekte
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
`F` `s` içinde çağrısı,`a` ve için`b`bir başvuru geçirir. `G` Bu nedenle, `s`bu çağrı için adlar `a`, ve `b` tümü aynı depolama konumuna başvurur ve üç atama, örnek alanını `s`değiştirir.

#### <a name="output-parameters"></a>Çıktı parametreleri

`out` Değiştirici ile belirtilen bir parametre bir çıkış parametresidir. Bir başvuru parametresine benzer şekilde, çıkış parametresi yeni bir depolama konumu oluşturmaz. Bunun yerine, bir çıktı parametresi, yöntem çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.

Bir biçimsel parametre bir çıkış parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, bir `out` *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) ve ardından gelen anahtar sözcüğünden oluşmalıdır biçimsel parametre olarak yazın. Bir değişken, çıkış parametresi olarak geçirilebilmesi için kesinlikle atanmamalıdır, ancak bir değişkenin çıkış parametresi olarak geçirildiği bir çağrıdan sonra, değişken kesinlikle atanmış olarak değerlendirilir.

Bir yöntem içinde, yerel bir değişken gibi, bir çıkış parametresi başlangıçta atanmamış olarak kabul edilir ve değeri kullanılmadan önce kesinlikle atanmalıdır.

Metodun her çıkış parametresi, yöntemin döndürüldüğünden önce kesinlikle atanmalıdır.

Kısmi Yöntem ([kısmi Yöntemler](classes.md#partial-methods)) veya yineleyici ([yineleyiciler](classes.md#iterators)) olarak belirtilen bir yöntem çıkış parametrelerine sahip olamaz.

Çıkış parametreleri genellikle birden çok dönüş değeri üreten yöntemlerde kullanılır. Örneğin:
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

Örnek, çıktıyı üretir:
```
c:\Windows\System\
hello.txt
```

Ve değişkenlerinin `name` geçirilmeden`SplitPath`önce atanmamış olabileceğini ve çağrının ardından kesinlikle atandığına göz önünde bulundurmanız gerekir. `dir`

#### <a name="parameter-arrays"></a>Parametre dizileri

`params` Değiştirici ile belirtilen bir parametre bir parametre dizisidir. Bir biçimsel parametre listesi bir parametre dizisi içeriyorsa, listedeki son parametre olmalıdır ve tek boyutlu dizi türünde olmalıdır. Örneğin, türleri `string[]` ve `string[][]` parametre dizisinin türü olarak kullanılabilir, ancak tür `string[,]` olamaz. Değiştiriciyi değiştiriciler `params` `ref` ve `out`ile birleştirmek mümkün değildir.

Bir parametre dizisi bağımsız değişkenlerin bir yöntem çağrısında iki yöntemden biriyle belirtilmesine izin verir:

*  Bir parametre dizisi için verilen bağımsız değişken, parametre dizisi türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tek bir ifade olabilir. Bu durumda, parametre dizisi tam olarak bir değer parametresi gibi davranır.
*  Alternatif olarak, çağırma parametre dizisi için sıfır veya daha fazla bağımsız değişken belirtebilir, burada her bağımsız değişken parametre dizisinin öğe türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) bir ifadedir. Bu durumda, çağırma parametre dizisi türünde bağımsız değişken sayısına karşılık gelen bir bir örnek oluşturur, dizi örneğinin öğelerini verilen bağımsız değişken değerleriyle başlatır ve gerçek olarak yeni oluşturulan dizi örneğini kullanır değişkendir.

Bir çağırmada değişken sayıda bağımsız değişkene izin verilmesi dışında, bir parametre dizisi tam olarak aynı türdeki bir değer parametresine ([değer parametreleri](classes.md#value-parameters)) eşdeğerdir.

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

İlk çağırma `F` , diziyi `a` bir değer parametresi olarak geçirir. İkinci çağırma `F` , otomatik olarak verilen öğe değerleriyle dört öğe `int[]` oluşturur ve bu dizi örneğini bir değer parametresi olarak geçirir. Benzer şekilde, üçüncü çağırma `F` bir sıfır öğesi `int[]` oluşturur ve bu örneği bir değer parametresi olarak geçirir. İkinci ve üçüncü etkinleştirmeleri yazma ile tam olarak eşdeğerdir:
```csharp
F(new int[] {10, 20, 30, 40});
F(new int[] {});
```

Aşırı yükleme çözümlemesi gerçekleştirirken, parametre dizisi olan bir yöntem normal biçiminde veya genişletilmiş biçiminde ([uygulanabilir işlev üyesi](expressions.md#applicable-function-member)) uygulanabilir. Bir yöntemin genişletilmiş biçimi yalnızca, metodun normal formu geçerli değilse ve genişletilmiş formla aynı imzaya sahip geçerli bir yöntem aynı türde zaten bildirilmemiş ise kullanılabilir.

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

Örnekte, bir parametre dizisi ile yönteminin mümkün olan genişletilmiş biçimlerinden ikisi, normal yöntemler olarak sınıfında zaten yer alır. Bu genişletilmiş formlar bu nedenle aşırı yükleme çözümlemesi gerçekleştirilirken değerlendirilmez ve ilk ve üçüncü yöntem etkinleştirmeleri bu nedenle normal yöntemleri seçer. Bir sınıf bir parametre dizisi olan bir yöntem bildiriyorsa, genişletilmiş formlardan bazılarını düzenli yöntemler olarak da içermesi yaygın olmayan bir durumdur. Bunu yaparak, parametre dizisi olan bir yöntemin genişletilmiş formu çağrıldığında oluşan bir dizi örneğinin ayrılmasını önlemek mümkündür.

Bir parametre dizisinin türü olduğunda, yöntemin normal `object[]`biçimi ve tek `object` bir parametre için expsona formu arasında olası bir belirsizlik oluşur. Belirsizliğin nedeni, kendisinin örtülü olarak türüne `object[]` `object`dönüştürülebilir olmasının nedenidir. Bununla birlikte, belirsizlik bir sorun değildir, ancak gerekirse bir atama eklenebilir.

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

' Nin `F`ilk ve son etkinleştirmeleri içinde, bağımsız değişken türünden parametre `F` türüne (her ikisi de tür `object[]`) örtük bir dönüştürme olduğundan, normal biçimi geçerlidir. Bu nedenle, aşırı yükleme çözümlemesi normal biçimini `F`seçer ve bağımsız değişken normal bir değer parametresi olarak geçirilir. İkinci ve üçüncü etkinleştirmeleri içinde, bağımsız değişken türünden parametre türüne `F` örtük dönüştürme olmadığından, normal biçimi Uygulanamaz (tür `object` örtük olarak türüne `object[]`dönüştürülemez). Ancak, genişletilmiş biçimi `F` geçerlidir, bu nedenle aşırı yükleme çözümlemesi tarafından seçilir. Sonuç olarak, bir tek öğe `object[]` , çağrı tarafından oluşturulur ve dizideki tek öğe, verilen bağımsız değişken değeri (kendisi bir `object[]`başvurusu olan) ile başlatılır.

### <a name="static-and-instance-methods"></a>Statik ve örnek yöntemleri

Bir yöntem bildirimi bir `static` değiştirici içerdiğinde, bu yöntem bir statik yöntem olarak kabul edilir. `static` Değiştirici yoksa, yöntem bir örnek yöntemi olarak kabul edilir.

Statik bir yöntem belirli bir örnek üzerinde çalışmaz ve statik bir yöntemde başvurmak `this` için derleme zamanı hatası olur.

Bir örnek yöntemi bir sınıfın belirli bir örneği üzerinde çalışır ve bu örneğe ( `this` [Bu erişim](expressions.md#this-access)) olarak erişilebilir.

Bir yöntem, `E.M` `E` `M` formunbir`M` *member_access* ([üye erişimi](expressions.md#member-access)) içinde başvuruluyorsa, birstatikyöntemiseiçerenbirtürübelirtmelidirveeğerbirörnekyöntemiise,`M` `E` içeren`M`bir türün örneği belirtilmelidir.

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="virtual-methods"></a>Sanal yöntemler

Bir örnek yöntemi bildirimi bir `virtual` değiştirici içerdiğinde, bu yöntem sanal bir yöntem olarak kabul edilir. `virtual` Değiştirici yoksa, yöntemi sanal olmayan bir yöntem olarak kabul edilir.

Sanal olmayan bir yöntemin uygulanması değişmez: Uygulamanın bildirildiği sınıfın bir örneğinde veya türetilmiş bir sınıfın örneği üzerinde çağrılmasından bağımsız olarak, uygulama aynı olur. Buna karşılık, sanal bir yöntemin uygulanması türetilmiş sınıflar tarafından değiştirilmiş olabilir. Devralınan bir sanal yöntemin uygulanmasını yerine geçen işlem, bu yöntemi ***geçersiz kılma*** ([geçersiz kılma yöntemleri](classes.md#override-methods)) olarak bilinir.

Bir sanal yöntem çağrısında, çağrının gerçekleştiği örneğin ***çalışma zamanı türü*** , çağrılacak gerçek Yöntem uygulamasını belirler. Sanal olmayan bir yöntem çağrısında, örneğin ***derleme zamanı türü*** belirleme faktörü olur. `N` Kesin koşullarda, adlı bir yöntem derleme zamanı türü `C` `R` `R` ve çalışma zamanı türü olan `A` bir örnek üzerinde bağımsız değişken listesiyle çağrıldığında (ya da `C` veya sınıfından türetilmiş bir sınıf) ' `C`den), çağrı aşağıdaki şekilde işlenir:

*  İlk olarak, içinde belirtilen ve tarafından `C` `N` `A` `M` devralınanYöntemler`C`kümesinden belirli bir yöntemi seçmek için,, ve ' a yeniden yükleme çözümlemesi uygulanır. Bu, [Yöntem etkinleştirmeleri](expressions.md#method-invocations)bölümünde açıklanmaktadır.
*  Sonra, sanal olmayan bir `M` yöntem çağrılırsa çağrılır. `M`
*  Aksi takdirde `M` , sanal bir yöntemdir ve ' ın `M` `R` en iyi türetilmiş uygulamasıyla çağrılır.

Bir sınıf tarafından tanımlanan veya devralan her sanal yöntem için, yönteminin bu sınıfa göre ***en çok türetilmiş bir uygulamasını*** vardır. Bir sınıfa `M` `R` göre bir sanal yöntemin en çok türetilmiş uygulanması aşağıdaki şekilde belirlenir:

*  Giriş `R` bildirimini`virtual` `M`içeriyorsa ,bu,'ninenfazlatüretilmişuygulamasıdır.`M`
*  Aksi takdirde, ' ı `override` `M`içeriyorsa,en çok türetilen uygulamasıdır `M`. `R`
*  Aksi takdirde, ' ın en iyi `M` türetilmiş uygulamasının `R` , doğrudan taban sınıfına `R`göre en çok türetilen uygulamasıyla `M` aynı olması gerekir.

Aşağıdaki örnek, sanal ve sanal olmayan Yöntemler arasındaki farkları göstermektedir:
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

Örnekte, `A` sanal olmayan bir yöntemi `F` ve sanal bir yöntemi `G`tanıtır. Sınıfı `B` yeni bir sanal olmayan yöntem `F`tanıtır, böylece devralınmış `F`öğesini gizler ve ayrıca devralınan yöntemi `G`geçersiz kılar. Örnek, çıktıyı üretir:
```
A.F
B.F
B.G
B.G
```

Deyimin `a.G()` `B.G` çağırdığına`A.G`dikkat edin. Bunun nedeni, örneğin derleme zamanı türü `B` `A`(yani) değil, örneğin çalışma zamanı türünün (yani), çağrılacak gerçek Yöntem uygulamasını belirler.

Yöntemlerin devralınan yöntemleri gizleyebildiğinden, bir sınıfın aynı imzaya sahip çeşitli sanal yöntemler içermesi mümkündür. Bu, bir belirsizlik sorunu sunmaz, ancak en çok türetilen Yöntem gizlenir. Örnekte
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
`C` ve`D` sınıfları aynı imzaya sahip iki sanal yöntem içerir: Tarafından `A` tanıtılan ve tarafından `C`tanıtılan bir. Tarafından `C` tanıtılan yöntem öğesinden `A`devralınan yöntemi gizler. Bu nedenle, içindeki `D` geçersiz kılma bildirimi tarafından `C`tanıtılan yöntemi geçersiz kılar ve tarafından `A`tanıtılan yöntemi geçersiz kılmak mümkün değildir.`D` Örnek, çıktıyı üretir:
```
B.F
B.F
D.F
D.F
```

Bir örneğine `D` , yöntemin gizlenmediği daha az türetilmiş bir tür aracılığıyla erişerek gizli sanal yöntemi çağırmak mümkün olduğunu unutmayın.

### <a name="override-methods"></a>Geçersiz kılma yöntemleri

Bir örnek yöntemi bildirimi bir `override` değiştirici içerdiğinde, yöntemi bir ***geçersiz kılma yöntemi***olarak kabul edilir. Bir geçersiz kılma yöntemi, aynı imzaya sahip devralınmış bir sanal yöntemi geçersiz kılar. Sanal bir yöntem bildiriminde yeni bir yöntem tanıtıldığı halde, bir geçersiz kılma yöntemi bildirimi, bu yöntemin yeni bir uygulamasını sağlayarak, var olan bir devralınmış sanal yöntemi uzmanlık eder.

Bir `override` bildirim tarafından geçersiz kılınan yöntem ***geçersiz kılınan temel yöntem***olarak bilinir. `M` Bir sınıfta `C`bildirildiği bir geçersiz kılma yöntemi için, geçersiz kılınan taban yöntemi, öğesinin doğrudan temel sınıf türünden `C` başlayarak her bir `C`temel sınıf türü incelenerek belirlenir ve birbirini izleyen her bir işleme devam eder doğrudan temel sınıf türü, belirli bir temel sınıf türüne kadar, tür bağımsız değişkenlerinin yerine `M` koyduktan sonraki aynı imzaya sahip en az bir erişilebilir yöntem bulunur. Geçersiz kılınan temel yöntemi bulma amaçları doğrultusunda, bir yöntem, varsa, varsa, varsa, varsa `public`veya varsa veya `internal` aynı `protected`programda olarak `C`bildirilirse, `protected internal`erişilebilir olarak değerlendirilir.

Bir geçersiz kılma bildirimi için aşağıdakilerin tümü doğru değilse bir derleme zamanı hatası oluşur:

*  Geçersiz kılınan bir temel yöntem yukarıda açıklandığı gibi bulunabilir.
*  Yalnızca bir tür geçersiz kılınan temel yöntem vardır. Bu kısıtlama yalnızca temel sınıf türü, tür bağımsız değişkenlerinin yerine geçen iki yöntemin imzasını yaptığı oluşturulmuş bir tür ise etkindir.
*  Geçersiz kılınan taban yöntemi bir sanal, Özet veya geçersiz kılma yöntemidir. Diğer bir deyişle, geçersiz kılınan taban yöntemi statik veya sanal olmayan olamaz.
*  Geçersiz kılınan taban yöntemi Sealed bir yöntem değil.
*  Override yöntemi ve geçersiz kılınan taban yöntemi aynı dönüş türüne sahip.
*  Geçersiz kılma bildirimi ve geçersiz kılınan taban yöntemi, aynı tanımlanmış erişilebilirliğe sahiptir. Diğer bir deyişle, geçersiz kılma bildirimi sanal yöntemin erişilebilirliğini değiştiremezler. Ancak, geçersiz kılınan temel yöntem, iç, geçersiz kılma yöntemini içeren derlemeden farklı bir derlemede bildirilirse, geçersiz kılma yönteminin tanımlanmış erişilebilirlik korunması gerekir.
*  Geçersiz kılma bildirimi tür-parametresi-kısıtlamalar-tümceleri belirtmiyor. Bunun yerine, kısıtlamalar geçersiz kılınan temel yöntemden devralınır. Geçersiz kılınan yöntemde tür parametreleri olan kısıtlamaların devralınan kısıtlamadaki tür bağımsız değişkenleriyle değiştirilmesini unutmayın. Bu, değer türleri veya korumalı türler gibi açıkça belirtildiğinde yasal olmayan kısıtlamalara yol açabilir.

Aşağıdaki örnek, geçersiz kılma kurallarının genel sınıflar için nasıl çalıştığını göstermektedir:
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

Geçersiz kılma bildirimi, geçersiz kılınan temel yönteme bir *base_access* ([temel erişim](expressions.md#base-access)) kullanarak erişebilir. Örnekte
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
' deki `B` çağırma,`A`içinde `PrintFields` belirtilen metodu çağırır. `base.PrintFields()` Bir *base_access* , sanal çağırma mekanizmasını devre dışı bırakır ve temel yöntemi sanal olmayan bir yöntem olarak değerlendirir. Yapılan çağrının `B` yazıldığı `A` `PrintFields` `PrintFields` `B`, bu, ' ın sanal ve çalışma zamanı türü olduğu için, içinde bildirildiği yöntemi yinelemeli olarak çağırır. `((A)this).PrintFields()` `((A)this)` .`B`

Yalnızca bir `override` değiştirici ekleyerek bir yöntem başka bir yöntemi geçersiz kılar. Diğer tüm durumlarda, devralınan bir yöntemle aynı imzaya sahip bir yöntem devralınan yöntemi gizler. Örnekte
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
`A` `F` içindeki `F` yöntemi`B` bir`override` değiştirici içermez ve bu nedenle içindeki yöntemi geçersiz kılmaz. Bunun yerine, içindeki `B` yöntemi içindeki `A`yöntemi gizler ve bildirim bir `new` değiştirici içermediğinden bir uyarı bildirilir. `F`

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
`F` içindeki `F` yöntemi`B` öğesinden`A`devralınan sanal yöntemi gizler. İçindeki `F` `B` `C`yeni öğesinin özel erişimi olduğundan, kapsamı yalnızca ' ın sınıf gövdesini içerir ve genişletilmez. `B` Bu nedenle, `F` içindeki `C` bildiriminin `F` devralınan öğesinden `A`geçersiz kılınmasına izin verilir.

### <a name="sealed-methods"></a>Sealed yöntemleri

Bir örnek yöntemi bildirimi bir `sealed` değiştirici içerdiğinde, bu yöntem Sealed bir ***Yöntem***olarak kabul edilir. Bir örnek yöntemi bildirimi `sealed` değiştiricisini içeriyorsa, `override` değiştiriciyi de içermelidir. `sealed` Değiştirici kullanımı, türetilmiş bir sınıfın yöntemi daha fazla geçersiz kılmasını önler.

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
sınıfı `B` iki geçersiz kılma yöntemi sağlar `F` : `sealed` değiştiriciye sahip olan ve olmayan bir `G` yöntem. `B`Sealed `modifier` kullanımı, daha fazla geçersiz `C` kılmayı `F`önler.

### <a name="abstract-methods"></a>Soyut yöntemler

Bir örnek yöntemi bildirimi bir `abstract` değiştirici içerdiğinde, bu yöntem soyut bir ***Yöntem***olarak kabul edilir. Soyut bir yöntem örtülü olarak bir sanal yöntem olsa da, değiştiriciye `virtual`sahip olamaz.

Soyut yöntem bildirimi yeni bir sanal yöntem tanıtır, ancak bu yöntemin bir uygulamasını sağlamaz. Bunun yerine, soyut olmayan türetilmiş sınıfların bu yöntemi geçersiz kılarak kendi uygulamasını sağlaması gerekir. Soyut bir yöntem gerçek uygulama sunmadığından, bir soyut yöntemin *method_body* yalnızca noktalı virgül içerir.

Soyut yöntem bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)).

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
`Shape` sınıfı, kendisini boyayacak bir geometrik şekil nesnesinin soyut kavramını tanımlar. Anlamlı bir varsayılan uygulama olmadığından Yöntemsoyuttur.`Paint` `Ellipse` Ve sınıfları`Box` somut`Shape` uygulamalardır. Bu sınıflar soyut olmadığından, `Paint` yöntemi geçersiz kılmaları ve gerçek bir uygulama sağlaması gerekir.

Bu, bir soyut metoda başvurmak için bir *base_access* ([temel erişim](expressions.md#base-access)) için derleme zamanı hatasıdır. Örnekte
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
bir soyut metoda başvurduğundan, `base.F()` çağrı için derleme zamanı hatası bildirilir.

Bir soyut Yöntem bildiriminin sanal bir yöntemi geçersiz kılmasına izin verilir. Bu, bir soyut sınıfın türetilmiş sınıflarda yöntemin yeniden uygulanmasını zormasına olanak tanır ve yöntemin orijinal uygulamasını kullanılamaz hale getirir. Örnekte
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
sınıf `A` bir sanal yöntem bildirir, sınıf `B` bu yöntemi soyut bir yöntemle geçersiz kılar ve sınıf `C` kendi uygulamasını sağlamak için soyut yöntemi geçersiz kılar.

### <a name="external-methods"></a>Dış Yöntemler

Bir yöntem bildirimi bir `extern` değiştirici içerdiğinde, bu yöntem bir ***dış yöntem***olarak kabul edilir. Dış Yöntemler, genellikle dışında C#bir dil kullanılarak dışarıdan uygulanır. Bir dış yöntem bildirimi gerçek uygulama sunmadığından, bir dış metodun *method_body* yalnızca noktalı virgülden oluşur. Dış yöntem genel olmayabilir.

Değiştirici genellikle bir `DllImport` öznitelik ([com ve Win32 bileşenleriyle birlikte çalışabilirlik](attributes.md#interoperation-with-com-and-win32-components)) ile birlikte kullanıldığında, dış yöntemlerin dll 'ler tarafından uygulanmasına izin verir (dinamik bağlantı kitaplıkları). `extern` Yürütme ortamı, dış yöntemlerin uygulamalarının sağlanabildiği diğer mekanizmaları destekleyebilir.

Bir dış yöntem bir `DllImport` öznitelik içerdiğinde, yöntem bildirimi de bir `static` değiştirici içermelidir. Bu örnek, `extern` değiştiricinin `DllImport` ve özniteliğin kullanımını gösterir:
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

### <a name="partial-methods-recap"></a>Kısmi Yöntemler (Recap)

Bir yöntem bildirimi bir `partial` değiştirici içerdiğinde, bu yöntem kısmi bir ***Yöntem***olarak kabul edilir. Kısmi yöntemler yalnızca kısmi türlerin ([kısmi türlerin](classes.md#partial-types)) üyeleri olarak bildirilebilecek ve bir dizi kısıtlamayla tabidir. Kısmi Yöntemler, [kısmi yöntemlerde](classes.md#partial-methods)daha ayrıntılı olarak açıklanmıştır.

### <a name="extension-methods"></a>Uzantı yöntemleri

Bir yöntemin ilk parametresi `this` değiştirici içerdiğinde, bu yöntem bir ***genişletme yöntemi***olarak kabul edilir. Uzantı yöntemleri yalnızca genel olmayan, iç içe olmayan statik sınıflarda bildirilemez. Bir genişletme yönteminin ilk parametresi dışında `this`bir değiştirici içeremez ve parametre türü bir işaretçi türü olamaz.

Aşağıda iki uzantı yöntemi bildiren bir statik sınıfa bir örnek verilmiştir:
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

Uzantı yöntemi, normal bir statik yöntemdir. Ayrıca, kapsayan statik sınıfının kapsam içinde olduğu yerlerde, ilk bağımsız değişken olarak alıcı ifadesi kullanılarak örnek yöntemi çağırma sözdizimi ([genişletme yöntemi etkinleştirmeleri](expressions.md#extension-method-invocations)) kullanılarak bir genişletme yöntemi çağrılabilir.

Aşağıdaki program, yukarıda belirtilen uzantı yöntemlerini kullanır:
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

Yöntemi ' de kullanılabilir `ToInt32` ve yöntemi `string[]`uzantı yöntemleri olarak bildirildiği için üzerinde `string`kullanılabilir. `Slice` Programın anlamı, sıradan statik yöntem çağrıları kullanılarak aşağıdakiler ile aynıdır:
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

Bir yöntem bildiriminin *method_body* bir blok gövdesinden, bir ifade gövdesinden ya da noktalı virgülden oluşur.

Bir yöntemin `void` ***sonuç türü*** , dönüş türü `void`ise veya yöntemin zaman uyumsuz olması ve dönüş türünün olması `System.Threading.Tasks.Task`durumunda olur. Aksi takdirde, zaman uyumsuz bir yöntemin sonuç türü dönüş türüdür ve dönüş türü `System.Threading.Tasks.Task<T>` ile zaman uyumsuz bir metodun sonuç türü olur. `T`

Bir yöntemin `void` sonuç türü ve bir blok gövdesi olduğunda, `return` bloktaki deyimlerin ([return deyimi](statements.md#the-return-statement)) bir ifade belirtmelerine izin verilmez. Void yönteminin bloğunun yürütülmesi normal şekilde tamamlanırsa (diğer bir deyişle, bu yöntem, Yöntem gövdesinin sonundaki şekilde akar), bu yöntem yalnızca geçerli çağıranına döner.
    
Bir yöntemin `void` sonucu ve bir ifade gövdesi olduğunda, ifade `E` bir *statement_expression*olmalıdır ve gövde, formun `{ E; }`bir blok gövdesine tam olarak eşdeğerdir.
    
Bir yöntemde void olmayan bir sonuç türü ve bir blok gövdesi olduğunda, bloktaki her `return` deyim, sonuç türüne örtük olarak dönüştürülebilir bir ifade belirtmelidir. Değer döndüren metodun blok gövdesinin uç noktasına ulaşılamıyor olmalıdır. Diğer bir deyişle, blok gövdesi olan bir değer döndüren yöntemde, denetimin Yöntem gövdesinin sonunu akışa girmesine izin verilmez.
    
Bir yöntemde void olmayan bir sonuç türü ve bir ifade gövdesi olduğunda, ifadenin sonuç türüne örtülü olarak dönüştürülebilir olması gerekir ve gövde, formun `{ return E; }`bir blok gövdesine tam olarak eşdeğerdir.
    
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
değer döndüren `F` Yöntem, denetim Yöntem gövdesinin sonuna akabileceğinden, derleme zamanı hatası ile sonuçlanır. `G` Ve`H` yöntemleri, olası tüm yürütme yolları bir dönüş değeri belirten bir return ifadesinde sona uğradığından doğrudur. `I` Yöntemi doğrudur, çünkü gövdesi yalnızca tek bir dönüş ifadesiyle bir ifade bloğuna denk gelir.

### <a name="method-overloading"></a>Yöntem aşırı yüklemesi

Yöntem aşırı yükleme çözümleme kuralları [tür çıkarımı](expressions.md#type-inference)bölümünde açıklanmaktadır.

## <a name="properties"></a>Özellikler

***Özelliği*** , bir nesnenin veya sınıfın özelliklerine erişim sağlayan bir üyesidir. Özellik örnekleri, bir dizenin uzunluğunu, bir yazı tipinin boyutunu, bir pencerenin başlığını, bir müşterinin adını vb. içerir. Özellikler, alanlar için doğal bir uzantıdır. her ikisi de ilişkili türleri olan üyeler adlandırılmış ve alanlara ve özelliklere erişim için sözdizimi aynıdır. Ancak, alanların aksine, Özellikler depolama konumlarını göstermiyor. Bunun yerine, özellikler, değerleri okunmak veya yazıldığında yürütülecek deyimleri belirten ***erişimcileri*** vardır. Bu sayede, eylemleri bir nesne özniteliklerinin okuma ve yazma ile ilişkilendirmek için bir mekanizma sağlar; Ayrıca, bu tür özniteliklere de hesaplanmasına izin verir.

Özellikler *property_declaration*s kullanılarak bildirilmiştir:

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

Bir *property_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)) ve dört erişim değiştiricisinin geçerli bir birleşimini içerebilir ([erişim değiştiricileri](classes.md#access-modifiers)) `new` , ([Yeni değiştirici](classes.md#the-new-modifier)) `static` , ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ( `sealed` `abstract` [geçersiz kılma yöntemleri](classes.md#override-methods)), ([korumalı Yöntemler](classes.md#sealed-methods)), ([soyut](classes.md#abstract-methods)Yöntemler) ve `extern`([Dış Yöntemler](classes.md#external-methods)) değiştiricileri.

Özellik bildirimleri, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([Yöntemler](classes.md#methods)) ile aynı kurallara tabidir.

Bir özellik bildiriminin *türü* , bildirim tarafından tanıtılan özelliğin türünü belirtir ve *MEMBER_NAME* özelliğinin adını belirtir. Özellik açık bir arabirim üyesi uygulama değilse, *MEMBER_NAME* yalnızca bir *tanımlayıcıdır*. Açık arabirim üye uygulaması ([Açık arabirim üye uygulamaları](interfaces.md#explicit-interface-member-implementations)) için, *MEMBER_NAME* bir *interface_type* ve ardından bir`.` *tanımlayıcı*ile oluşur.

Özelliğin *türü* en az özelliğin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir *property_body* , ***erişimci gövdesinden*** ya da bir ***ifade gövdesinden***oluşabilir. Bir erişimci gövdesinde, "`{`" ve "`}`" belirteçlerinin içine alınması gereken accessor_declarations, özelliğin erişimcileri ([erişimcileri](classes.md#accessors)) bildirmek. Erişimciler, özelliği okuma ve yazma ile ilişkili yürütülebilir deyimleri belirler.

`=>` Arkasından bir `{ get { return E; } }` ifade`E` ve bir noktalı virgül gelen bir ifade gövdesi, deyim gövdesine tam olarak eşdeğerdir ve bu nedenle yalnızca alıcı özelliklerini belirtmek için kullanılabilir alıcı tek bir ifade tarafından verilir.

Bir *property_initializer* , yalnızca otomatik olarak uygulanan bir Özellik ([otomatik olarak uygulanan özellikler](classes.md#automatically-implemented-properties)) için verilebilir ve bu tür özelliklerin temel alınan alanının, ifade tarafından verilen değerle başlatılmasına neden olur.

Bir özelliğe erişim sözdizimi, bir alanla ilgili olarak aynı olsa da, bir özellik değişken olarak sınıflandırılmıyor. Bu nedenle, bir `ref` özelliği veya `out` bağımsız değişken olarak geçirmek mümkün değildir.

Bir özellik bildirimi bir `extern` değiştirici içerdiğinde, özelliği bir ***dış Özellik***olarak kabul edilir. Dış özellik bildirimi gerçek uygulama sağladığından, *accessor_declarations* her biri noktalı virgülle oluşur.

### <a name="static-and-instance-properties"></a>Statik ve örnek özellikleri

Bir özellik bildirimi bir `static` değiştirici içerdiğinde, özelliği statik bir ***özellik***olarak kabul edilir. Değiştirici yoksa, özelliği bir ***örnek özelliği***olarak kabul edilir. `static`

Statik bir özellik belirli bir örnekle ilişkili değildir ve statik bir özelliğin erişimcilerine başvuracak `this` derleme zamanı hatasıdır.

Örnek özelliği, bir sınıfın belirli bir örneğiyle ilişkilendirilir ve bu örneğe bu özelliğin erişimcilerinde `this` ([Bu erişim](expressions.md#this-access)) erişilebilir.

Bir özelliğe, `E.M` `M` `M` `E` formunbirmember_access(üyeerişimi)içindebaşvuruluyorsa,birstatiközelliktir,içerenbirtürübelirtmekvebirörnekise`M` [](expressions.md#member-access) özelliği, içeren `M`bir türün örneğini belirtmelidir.

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="accessors"></a>C

Bir özelliğin *accessor_declarations* , bu özelliği okuma ve yazma ile ilişkili çalıştırılabilir deyimleri belirler.

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

Erişimci bildirimleri bir *get_accessor_declaration*, *set_accessor_declaration*veya her ikisinden oluşur. Her `get` erişimci bildirimi belirtecinden oluşur veya `set` sonra isteğe bağlı bir *accessor_modifier* ve *accessor_body*.

*Accessor_modifier*s kullanımı aşağıdaki kısıtlamalara tabidir:

*  Bir *accessor_modifier* , bir arabirimde veya açık arabirim üyesi uygulamasında kullanılamaz.
*  Değiştiriciye sahip `override` olmayan bir özellik veya Dizin Oluşturucu için, yalnızca özellik veya `get` dizin oluşturucunun hem hem de `set` erişimcisi varsa ve bu erişimcilerle yalnızca biri için izin verildiğinde bir accessor_modifier izin verilir.
*  `override` Değiştirici içeren bir özellik veya Dizin Oluşturucu için, erişimci geçersiz kılınmakta olan erişimcinin *accessor_modifier*ile eşleşmesi gerekir.
*  *Accessor_modifier* , özelliğin veya dizin oluşturucunun kendisi tarafından belirtilen erişilebilirliğine göre kesinlikle daha kısıtlayıcı bir erişilebilirlik bildirmelidir. Kesin olması için:
   * Özelliğin veya dizin oluşturucunun tanımlanmış bir erişilebilirliği `public`varsa, *accessor_modifier* `protected internal`, `internal` `protected`, veya `private`olabilir.
   * Özelliğin veya dizin oluşturucunun tanımlanmış bir erişilebilirliği `protected internal`varsa, *accessor_modifier* `internal` `protected`, veya `private`olabilir.
   * Özellik veya dizin `internal` oluşturucunun veya `protected` `private`' nin tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* olmalıdır.
   * Özelliğin veya dizin oluşturucunun tanımlanmış bir erişilebilirliği `private`varsa, hiçbir *accessor_modifier* kullanılamaz.

Ve `abstract` özellikleri`extern` için, belirtilen her erişimci için *accessor_body* yalnızca noktalı virgüldür. Soyut olmayan, extern olmayan bir özelliğin her bir *accessor_body* noktalı virgül olabilir. Bu durumda, ***otomatik olarak uygulanan bir özelliktir*** ([otomatik olarak uygulanan özellikler](classes.md#automatically-implemented-properties)). Otomatik olarak uygulanan özelliğin en az bir get erişimcisi olmalıdır. Diğer soyut olmayan, extern olmayan özelliğin erişimcileri için *accessor_body* , karşılık gelen erişimci çağrıldığında yürütülecek deyimleri belirten bir *bloğudur* .

`get` Erişimci, özellik türünün dönüş değeri olan parametresiz bir yönteme karşılık gelir. Atama hedefi dışında, bir ifadede bir özelliğe başvurulduğunda, `get` özelliğin erişimcisi özelliğin değerini hesaplamak için çağrılır ([ifadelerin değerleri](expressions.md#values-of-expressions)). Bir `get` erişimcinin gövdesi, [Yöntem gövdesinde](classes.md#method-body)açıklanan değer döndüren yöntemlere yönelik kurallara uymalıdır. Özellikle, bir `return` `get` erişimcinin gövdesindeki tüm deyimler, özellik türüne örtük olarak dönüştürülebilir bir ifade belirtmelidir. Ayrıca, `get` erişimcinin uç noktasına ulaşılamamalıdır.

Erişimci, özellik türü `void` ve dönüş türü tek bir değer parametresine sahip bir yönteme karşılık gelir. `set` `set` Erişimcinin örtük parametresi her zaman adlandırılır `value`. Bir atamaya ([atama işleçleri](expressions.md#assignment-operators) `++` ) hedefi veya ya da ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) veya `--` işleneni olarak bir özelliğe başvurulduğunda, erişimci, yeni değeri ([basit atama](expressions.md#simple-assignment)) sağlayan bir bağımsız değişkenle çağrılır (değeri, atamanın sağ tarafında veya `++` veya `--` işlecinin işleneni olur). `set` Bir `set` erişimcinin gövdesi, [Yöntem gövdesinde](classes.md#method-body)açıklanan yöntemler için `void` kurallara uymalıdır. Özellikle, `return` `set` erişimci gövdesindeki deyimlerin bir ifade belirtmelerine izin verilmez. Erişimci örtük olarak adlı `value`bir parametreye sahip olduğundan, bu adı bir yerel değişken veya bir `set` erişimcinin sabit bildiriminin bu ada sahip olması için derleme zamanı hatası olur. `set`

`get` Ve`set` erişimcilerinin varlığına veya yokluğuna göre, bir özellik şu şekilde sınıflandırılır:

*  Hem `get` erişimci`set` hem de erişimci içeren bir özellik, ***okuma-yazma*** özelliği olarak kabul edilir.
*  Yalnızca bir `get` erişimcisi olan bir özellik ***salt okunurdur*** özelliği olarak kabul edilir. Bir salt okuma özelliğinin bir atamanın hedefi olması için derleme zamanı hatası.
*  Yalnızca bir `set` erişimcisi olan bir özellik ***salt yazılır*** bir özellik olarak kabul edilir. Atama hedefi dışında, bir ifadede salt yazılır bir özelliğe başvurmak için derleme zamanı hatası olur.

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
`Button` denetim ortak`Caption` bir özellik bildirir. Özelliğin erişimcisi özel`caption`alandadepolanandizeyidöndürür. `get` `Caption` `set` Erişimci, yeni değerin geçerli değerden farklı olup olmadığını denetler ve bu durumda yeni değeri depolar ve denetimi yeniden boyar. Özellikler genellikle yukarıda gösterilen kalıbı izler: Erişimci yalnızca özel bir alanda depolanan bir değer döndürür `set` ve erişimci bu özel alanı değiştirir ve ardından nesnenin durumunu tamamen güncelleştirmek için gereken ek eylemleri gerçekleştirir. `get`

Yukarıdaki sınıf verildiğinde, `Caption` özelliğin kullanım örneği aşağıda verilmiştir: `Button`
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Burada, `set` erişimci özelliğe bir değer atanarak çağrılır `get` ve erişimci bir ifadede özelliğe başvuruda bulunarak çağrılır.

Bir özelliğin `set` ve erişimcileri farklı Üyeler değildir ve bir özelliğin erişimcilerinin ayrı ayrı bildirilmesini mümkün değildir. `get` Bu nedenle, bir okuma-yazma özelliğinin iki erişimcinin farklı erişilebilirliği olması mümkün değildir. Örnek
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
tek bir okuma-yazma özelliği bildirmiyor. Bunun yerine, aynı ada sahip iki özellik bildirir, bir salt okunurdur ve bir salt yazılır. Aynı sınıfta belirtilen iki üye aynı ada sahip olmadığından, örnek bir derleme zamanı hatası oluşmasına neden olur.

Türetilmiş bir sınıf devralınan bir özellik ile aynı ada sahip bir özelliği bildiriyorsa, türetilen Özellik devralınan özelliği hem okuma hem de yazma açısından gizler. Örnekte
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
içindeki `P` özelliği,`P` içindeki özelliğini`A` hem okuma hem de yazma açısından gizler. `B` Bu nedenle, deyimlerde
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
`b.P` ' deki `P` `P`saltyazılır özelliği ' deki`A`salt yazılır özelliğini gizlemediğinden, ' a atama, derleme zamanı hatasına neden olur. `B` Ancak, bir dönüştürmenin gizli `P` özelliğe erişmek için kullanılabileceğini unutmayın.

Ortak alanlardan farklı olarak, özellikler bir nesnenin iç durumu ile ortak arabirimi arasında bir ayrım sağlar. Örneği göz önünde bulundurun:
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

Burada, `Label` sınıfı iki `int` alanı `x` ve `y`, konumunu depolamak için kullanır. Konum, `X` hem hem de bir `Y` özellik olarak ve `Location` türünün `Point`özelliği olarak genel kullanıma sunulur. Uygulamasının `Label`gelecek bir sürümünde, konumu `Point` dahili olarak depolamak daha uygun hale gelirse, bu değişiklik sınıfın genel arabirimini etkilemeden yapılabilir:
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

Alan ve bunun`y` yerine `Label` , bu tür bir değişikliği sınıfında yapmak imkansız olurdu. `x` `public readonly`

Durumu özellikler aracılığıyla göstermek, alanları doğrudan açığa çıkarmadan daha az verimlidir. Özellikle, bir özellik sanal olmayan ve yalnızca küçük miktarda kod içerdiğinde, yürütme ortamı erişimcileri çağrılarını erişimcilerinin gerçek koduyla değiştirebilir. Bu işlem, ***satır içi***olarak bilinir ve özellik erişimini alan erişimi olarak verimli hale getirir, ancak özelliklerin daha fazla esnekliğini korur.

Bir `get` erişimcinin çağrılması bir alanın değerini okumak için kavramsal olarak denk olduğundan, `get` erişimcilerin observable yan etkileri olması için kötü programlama stili olarak değerlendirilir. Örnekte
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
`Next` özelliğin değeri, özelliğin daha önce erişilme sayısına bağlıdır. Bu nedenle, özelliğe erişmek bir observable yan etkisi oluşturur ve özellik bunun yerine bir yöntem olarak uygulanmalıdır.

Erişimcileri için `get` "yan etkileri yok" kuralı, `get` erişimcilerin yalnızca alanlarda depolanan değerleri döndürmek için her zaman yazılması anlamına gelmez. Aslında, `get` erişimciler genellikle birden çok alana erişerek veya yöntemleri çağırarak bir özelliğin değerini hesaplar. Ancak, düzgün tasarlanmış `get` bir erişimci nesnenin durumunda observable değişikliklerine neden olan hiçbir eylem gerçekleştirmiyor.

Özellikler, ilk kez başvuruluncaya kadar bir kaynağın başlatılmasını geciktirmek için kullanılabilir. Örneğin:
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

Sınıfı, sırasıyla standart giriş, `In`çıkış ve hata cihazlarını temsil eden üç özellik `Out`içerir,, ve `Error`. `Console` Bu üyelerin özellikler olarak kullanıma sunulmasıyla, `Console` sınıf gerçekten kullanılana kadar başlatma durumlarını erteleyebilir. Örneğin, ilk olarak `Out` özelliğe başvurulduğunda
```csharp
Console.Out.WriteLine("hello, world");
```
çıkış aygıtı `TextWriter` için temel alınan oluşturulur. Ancak uygulama `In` ve `Error` özelliklerine başvuru yapıyorsa, bu cihazlar için hiçbir nesne oluşturulmaz.

### <a name="automatically-implemented-properties"></a>Otomatik uygulanan özellikler

Otomatik olarak uygulanan bir Özellik (ya da Short için ***Otomatik Özellik*** ), salt noktalı erişimci gövdeleriyle soyut olmayan extern olmayan bir özelliktir. Otomatik Özellikler bir get erişimcisine sahip olmalı ve isteğe bağlı olarak bir set erişimcisine sahip olabilir.

Bir özellik otomatik olarak uygulanan bir özellik olarak belirtildiğinde, özelliği için otomatik olarak bir yedekleme alanı kullanılabilir ve erişimciler, bu yedekleme alanına okuma ve yazma için uygulanır. Auto özelliğinin ayarlanmış bir erişimcisi yoksa, yedekleme alanı kabul `readonly` edilir ([salt okunur alanlar](classes.md#readonly-fields)). Yalnızca bir `readonly` alan gibi, bir alıcı otomatik özelliği de kapsayan sınıfın oluşturucusunun gövdesinde de atanabilir. Böyle bir atama, doğrudan özelliğinin salt okunur yedekleme alanına atar.

Otomatik Özellik isteğe bağlı olarak, bir *variable_initializer* ([değişken başlatıcıları](classes.md#variable-initializers)) olarak, doğrudan yedekleme alanına uygulanan bir *property_initializer*sahip olabilir.

Aşağıdaki örnek:
```csharp
public class Point {
    public int X { get; set; } = 0;
    public int Y { get; set; } = 0;
}
```
aşağıdaki bildirime eşdeğerdir:
```csharp
public class Point {
    private int __x = 0;
    private int __y = 0;
    public int X { get { return __x; } set { __x = value; } }
    public int Y { get { return __y; } set { __y = value; } }
}
```

Aşağıdaki örnek:
```csharp
public class ReadOnlyPoint
{
    public int X { get; }
    public int Y { get; }
    public ReadOnlyPoint(int x, int y) { X = x; Y = y; }
}
```
aşağıdaki bildirime eşdeğerdir:
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

ReadOnly alanının atamalarının, Oluşturucu içinde gerçekleştikleri için geçerli olduğuna dikkat edin.


### <a name="accessibility"></a>Erişilebilirlik

Bir erişimcinin bir *accessor_modifier*varsa, erişimcinin erişilebilirlik etki alanı ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)), *accessor_modifier*'in belirtilen erişilebilirliği kullanılarak belirlenir. Bir erişimcinin bir *accessor_modifier*yoksa, erişimcinin erişilebilirlik etki alanı, özelliğin veya dizin oluşturucunun tanımlanmış erişilebilirliğine göre belirlenir.

Bir *accessor_modifier* varlığı hiçbir şekilde üye aramayı ([işleçler](expressions.md#operators)) veya aşırı yükleme çözümünü ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) etkilemez. Özellik veya dizin oluşturucudaki değiştiriciler her zaman erişim bağlamından bağımsız olarak hangi özelliğin veya dizin oluşturucunun bağlı olduğunu belirlenir.

Belirli bir özellik veya Dizin Oluşturucu seçildikten sonra, bu kullanımın geçerli olup olmadığını anlamak için ilgili erişimcilerinin erişilebilirlik etki alanları kullanılır:

*  Kullanım bir değer ([ifadelerin değerleri](expressions.md#values-of-expressions)) ise, `get` erişimci bulunmalı ve erişilebilir olmalıdır.
*  Kullanım basit bir atamanın hedefi ([basit atama](expressions.md#simple-assignment)) ise, `set` erişimci bulunmalı ve erişilebilir olmalıdır.
*  Kullanım, bileşik atamanın hedefi ([bileşik atama](expressions.md#compound-assignment)) `++` veya ya `--` da işleçlerinin hedefi ([işlev üyeleri](expressions.md#function-members).9, [çağırma ifadeleri](expressions.md#invocation-expressions)) olarak ise, hem `get` erişimcileri hem de `set` erişimci bulunmalı ve erişilebilir olmalıdır.

Aşağıdaki örnekte, özelliği `A.Text` , yalnızca `set` erişimcinin çağrıldığı bağlamlarda bile `B.Text`özelliği tarafından gizlenir. Buna karşılık, özelliği `B.Count` sınıfına `M`erişemez, bu nedenle bunun yerine erişilebilir özellik `A.Count` kullanılır.

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

Arabirim uygulamak için kullanılan bir erişimcinin bir *accessor_modifier*olamaz. Bir arabirim uygulamak için yalnızca bir erişimci kullanılırsa, diğer erişimci bir *accessor_modifier*ile bildirilemeyebilir:
```csharp
public interface I
{
    string Prop { get; }
}

public class C: I
{
    public string Prop {
        get { return "April"; }       // Must not have a modifier here
        internal set {...}            // Ok, because I.Prop has no set accessor
    }
}
```

### <a name="virtual-sealed-override-and-abstract-property-accessors"></a>Sanal, Sealed, override ve abstract Özellik erişimcileri

`virtual` Özellik bildirimi, özelliği erişimcilerinin sanal olduğunu belirtir. Değiştirici `virtual` , okuma-yazma özelliğinin her iki erişimcisi için de geçerlidir; bir okuma-yazma özelliğinin yalnızca bir erişimcisinin sanal olması mümkün değildir.

`abstract` Özellik bildirimi, özelliğin erişimcilerinin sanal olduğunu, ancak erişimcilerinin gerçek bir uygulamasını sağlamamayı belirtir. Bunun yerine, soyut olmayan türetilmiş sınıfların, özelliği geçersiz kılarak erişimcileri için kendi uygulamasını sağlaması gerekir. Soyut Özellik bildirimine yönelik bir erişimci gerçek uygulama sunmadığından, *accessor_body* yalnızca noktalı virgülden oluşur.

Hem `abstract` hem`override` de değiştiricilerini içeren bir özellik bildirimi, özelliğin soyut olduğunu ve bir temel özelliği geçersiz kıldığını belirtir. Böyle bir özelliğin erişimcileri de soyuttur.

Soyut Özellik bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)). Devralınan bir sanal özelliğin erişimcileri, bir `override` yönergeyi belirten özellik bildirimini ekleyerek türetilmiş bir sınıfta geçersiz kılınabilir. Bu, ***geçersiz kılma özelliği bildirimi***olarak bilinir. Geçersiz kılan özellik bildirimi yeni bir özellik bildirmiyor. Bunun yerine, var olan bir sanal özelliğin erişimcilerinin uygulamalarını özelleştirir.

Geçersiz kılma özelliği bildirimi devralınan özellik olarak aynı erişilebilirlik değiştiricilerini, türü ve adı belirtmelidir. Devralınan özelliğin yalnızca tek bir erişimcisi varsa (yani devralınan özellik salt okunurdur veya salt yazılır ise), geçersiz kılma özelliği yalnızca o erişimciyi içermelidir. Devralınan özellik her iki erişimcileri de içeriyorsa (yani, devralınan Özellik okuma-yazma ise), geçersiz kılma özelliği tek bir erişimci veya her iki erişimci içerebilir.

Geçersiz kılma özelliği bildirimi, `sealed` değiştiricisini içerebilir. Bu değiştiricinin kullanılması, türetilmiş bir sınıfın özelliği daha fazla geçersiz kılmasını önler. Sealed özelliğinin erişimcileri de korumalıdır.

Bildirim ve çağırma sözdiziminde farklar haricinde, sanal, korumalı, geçersiz kılma ve soyut erişimciler, sanal, mühürlenmiş, geçersiz kılma ve soyut yöntemler gibi davranır. Özellikle, [sanal yöntemlerde](classes.md#virtual-methods)açıklanan kurallar, [geçersiz kılma yöntemleri](classes.md#override-methods), [korumalı Yöntemler](classes.md#sealed-methods)ve [soyut yöntemler](classes.md#abstract-methods) , erişimciler karşılık gelen bir formun yöntemlerimiz gibi geçerlidir:

*  `get` Erişimci, özellik türünün dönüş değeri ve kapsayan özelliği ile aynı değiştiriciler içeren parametresiz bir yönteme karşılık gelir.
*  Erişimci, özellik türünün tek değerli parametresine `void` , dönüş türüne ve kapsayan özelliği ile aynı değiştiricilere karşılık gelen bir yönteme karşılık gelir. `set`

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
`X`, sanal bir salt okunurdur özelliğidir, `Y` sanal bir okuma-yazma özelliğidir ve `Z` soyut bir okuma-yazma özelliğidir. Soyut `Z` olduğundan, kapsayan sınıf `A` de soyut olarak bildirilmelidir.

Öğesinden `A` türetilen bir sınıf aşağıda gösterilmektedir:
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

Burada,, ve `X` `Y` `Z` bildirimleri Özellik bildirimlerini geçersiz kılar. Her özellik bildirimi, karşılık gelen devralınmış özelliğin erişilebilirlik değiştiricilerini, türünü ve adını tam olarak eşleştirir. `X` `base` Ve `get` erişimcisininerişimcisi,devralınanerişimcilereerişmekiçin`set` anahtar sözcüğünü kullanır `Y` . Öğesinin `Z` bildirimi, her iki soyut erişimciyi geçersiz kılar. bu nedenle içinde `B`bekleyen soyut işlev üyeleri yoktur ve `B` soyut olmayan bir sınıf olarak izin verilir.

Bir özellik bir `override`olarak bildirildiğinde, geçersiz kılınan hiçbir erişimciyi geçersiz kılma kodu tarafından erişilebilir olmalıdır. Ayrıca, hem özelliğin hem de dizin oluşturucunun kendisi ve erişimcilerinin tanımlanmış erişilebilirliği, geçersiz kılınan üye ve erişimcilerinin ile eşleşmelidir. Örneğin:
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

Bir ***olay*** , bir nesnenin veya sınıfın bildirimleri sağlamasını sağlayan bir üyedir. İstemciler ***olay işleyicileri***sağlayarak olaylar için yürütülebilir kod ekleyebilir.

Olaylar *event_declaration*s kullanılarak bildirilmiştir:

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

Bir *event_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)) ve dört erişim değiştiricisinin geçerli bir birleşimini içerebilir ([erişim değiştiricileri](classes.md#access-modifiers)) `new` , ([Yeni değiştirici](classes.md#the-new-modifier)) `static` , ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ( `sealed` `abstract` [geçersiz kılma yöntemleri](classes.md#override-methods)), ([korumalı Yöntemler](classes.md#sealed-methods)), ([soyut](classes.md#abstract-methods)Yöntemler) ve `extern`([Dış Yöntemler](classes.md#external-methods)) değiştiricileri.

Olay bildirimleri, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([yöntemleriyle](classes.md#methods)) ile aynı kurallara tabidir.

Olay bildiriminin *türü* bir *delegate_type* ([başvuru türleri](types.md#reference-types)) olmalıdır ve bu *delegate_type* en az olayın kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir olay bildirimi, *event_accessor_declarations*içerebilir. Ancak, dış olmayan, soyut olmayan olaylar için derleyici onları otomatik olarak ([alan benzeri olaylar](classes.md#field-like-events)) sağlar; extern olaylar için, erişimciler dışarıdan sağlanır.

*Event_accessor_declarations* atan bir olay bildirimi, bir veya daha fazla olayı tanımlar — her bir *variable_declarator*s. Öznitelikler ve değiştiriciler, bu tür bir *event_declaration*tarafından belirtilen tüm Üyeler için geçerlidir.

Bir *event_declaration* için hem `abstract` değiştirici hem de küme ayracı ile ayrılmış *event_accessor_declarations*içeren derleme zamanı hatasıdır.

Bir olay bildirimi bir `extern` değiştirici içerdiğinde, olay bir ***dış olay***olarak kabul edilir. Dış bir olay bildirimi gerçek uygulama içermediği için, hem `extern` değiştirici hem de *event_accessor_declarations*içermesi hatadır.

Bir *variable_initializer*içeren bir olay bildirimi `abstract` `external` *variable_declarator* için derleme zamanı hatasıdır.

Bir olay, `+=` ve `-=` işleçlerinin sol işleneni olarak kullanılabilir ([olay atama](expressions.md#event-assignment)). Bu işleçler sırasıyla olay işleyicilerini bir olaydan kaldırmak ya da kaldırmak için kullanılır ve olayın erişim değiştiricileri, bu tür işlemlere izin verilen bağlamlara eklenir.

`+=` Olayıbildirentürdışındakibirolaydaizinverilentekişlemlerolduğundan,dışkodbirolayiçinişleyicilerekleyebilirvekaldırabilir,ancakbaşkahiçbirşekildeolayiçintemelalınanolay`-=` listesini alabilir veya değiştirebilir ileridir.

Formun `x += y` bir işleminde veya `x -= y`, bir olaysa ve `x` başvuru, bildirimini içeren türün `x`dışında gerçekleşirken, işlemin sonucu türüne `void` sahip olur (sahip olmanın aksine). öğesinin `x`türü, `x` atamasından sonra değeri ile). Bu kural, dış kodun bir olayın temel temsilcisini dolaylı olarak incelemeden yasaklar.

Aşağıdaki örnekte, olay işleyicilerinin `Button` sınıfının örneklerine nasıl eklendiği gösterilmektedir:
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

Burada, `LoginDialog` örnek Oluşturucu iki `Button` örnek oluşturur ve olay işleyicilerini `Click` olaylara ekler.

### <a name="field-like-events"></a>Alan benzeri olaylar

Bir olayın bildirimini içeren sınıfın veya yapının program metni içinde, bazı olaylar alan gibi kullanılabilir. Bu şekilde kullanılmak üzere, bir olay ya `abstract` `extern`da olmamalıdır ve açıkça *event_accessor_declarations*içermemelidir. Bu tür bir olay, bir alana izin veren herhangi bir bağlamda kullanılabilir. Alan, olaya eklenmiş olan olay işleyicileri listesine başvuran bir temsilci ([Temsilciler](delegates.md)) içerir. Hiçbir olay işleyicisi eklenmemişse, alan içerir `null`.

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
`Click``Button` sınıf içinde bir alan olarak kullanılır. Örneğin gösterdiği gibi, alan, temsilci çağırma ifadelerinde incelenebilir, değiştirilebilir ve kullanılabilir. Sınıfındaki yöntemi, olayı"oluşturur".`Click` `OnClick` `Button` Bir olayı oluşturma kavramı, olayın gösterdiği temsilciyi çağırmak için tam olarak eşdeğerdir. bu nedenle, olayları yükseltmek için özel dil yapıları yoktur. Temsilci çağrısının önünde, temsilcinin null olmamasını sağlayan bir denetim olduğunu unutmayın.

`Button` Sınıf bildiriminin dışında `Click` , üye yalnızca `+=` ve `-=` işleçlerinin sol tarafında, içinde olduğu gibi kullanılabilir.
```csharp
b.Click += new EventHandler(...);
```
Bu, `Click` olayın çağırma listesine bir temsilci ekler ve
```csharp
b.Click -= new EventHandler(...);
```
Bu, `Click` olayın çağırma listesinden bir temsilciyi kaldırır.

Alan benzeri bir olay derlenirken, derleyici temsilciyi tutmak için otomatik olarak depolama oluşturur ve temsilci alanına olay işleyicileri ekleyen veya çıkarmayan olay için erişimciler oluşturur. Ekleme ve kaldırma işlemleri iş parçacığı açısından güvenlidir ve bir örnek olayı veya tür nesnesi ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) için kapsayan nesne üzerinde kilit ([lock deyimi](statements.md#the-lock-statement)) tutulurken (ancak gerekli değildir) yapılması gerekir. statik bir olay için.

Bu nedenle, formun bir örnek olay bildirimi:
```csharp
class X
{
    public event D Ev;
}
```
Şuna eşdeğer bir değere derlenecek:
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
Sınıfı `X`içinde, `+=` ve `Ev` işleçlerinin`-=` sol tarafındaki başvurular ekleme ve kaldırma erişimcilerinin çağrılmasına neden olur. Diğer tüm başvuruları `Ev` , bunun yerine gizli alana `__Ev` başvuracak şekilde derlenir ([üye erişim](expressions.md#member-access)). "`__Ev`" Adı rastgele; gizli alan herhangi bir ada veya hiç ada sahip olabilir.

### <a name="event-accessors"></a>Olay erişimcileri

Olay bildirimleri, yukarıdaki `Button` örnekte olduğu gibi genellikle event_accessor_declarations ' ı atyordu. Bunun için bir durum, her olay için bir alanın depolama maliyetinin kabul edilebilir olması durumunda oluşur. Böyle durumlarda, bir sınıf *event_accessor_declarations* içerebilir ve olay işleyicilerinin listesini depolamak için özel bir mekanizma kullanabilir.

Bir olayın *event_accessor_declarations* olay işleyicilerini ekleme ve kaldırma ile ilişkili yürütülebilir deyimleri belirtir.

Erişimci bildirimleri bir *add_accessor_declaration* ve *remove_accessor_declaration*oluşur. Her erişimci bildirimi belirtecinden `add` oluşur veya `remove` arkasından bir *blok*gelir. Bir *add_accessor_declaration* ile ilişkili *blok* , bir olay işleyicisi eklendiğinde yürütülecek deyimleri belirtir ve bir *remove_accessor_declaration* ile ilişkili *blok* yürütülecek deyimleri belirtir bir olay işleyicisi kaldırıldığında.

Her *add_accessor_declaration* ve *remove_accessor_declaration* , olay `void` türü ve dönüş türü tek bir değer parametresine sahip bir yönteme karşılık gelir. Bir olay erişimcisinin örtük parametresi olarak adlandırılmıştır `value`. Olay atamasında bir olay kullanıldığında, uygun olay erişimcisi kullanılır. Özellikle, atama işleci ise `+=` Add erişimcisi kullanılır ve atama `-=` işleci varsa kaldırma erişimcisi kullanılır. Her iki durumda da, atama işlecinin sağ işleneni olay erişimcisinin bağımsız değişkeni olarak kullanılır. Bir *add_accessor_declaration* veya *remove_accessor_declaration* bloğunun, `void` [Yöntem gövdesinde](classes.md#method-body)açıklanan yöntemlerle ilgili kurallara uyması gerekir. Özellikle, `return` bu tür bir bloktaki deyimlerin bir ifade belirtmelerine izin verilmez.

Bir olay erişimcisinde örtük olarak adlı `value`bir parametre olduğundan, bu ada sahip olması için bir yerel değişken veya bir olay erişimcisinde belirtilen sabit için derleme zamanı hatası olur.

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
sınıfı `Control` , olaylar için bir iç depolama mekanizması uygular. Yöntemi bir temsilci değerini bir anahtarla ilişkilendirir `GetEventHandler` , yöntemi şu anda `RemoveEventHandler` bir anahtarla ilişkili temsilciyi döndürür ve yöntemi belirtilen olay için bir temsilciyi olay işleyicisi olarak kaldırır. `AddEventHandler` Temel alınan depolama mekanizması, bir `null` temsilci değerini bir anahtarla ilişkilendirme maliyeti olmaması ve bu nedenle işlenmemiş olayların hiçbir depolama alanını tüketmesi gibi tasarlanmıştır.

### <a name="static-and-instance-events"></a>Statik ve örnek olayları

Bir olay bildirimi bir `static` değiştirici içerdiğinde, olay statik bir ***olay***olarak kabul edilir. Değiştirici yoksa, olay bir ***örnek olay***olarak kabul edilir. `static`

Statik bir olay, belirli bir örnekle ilişkili değildir ve statik bir olayın erişimcilerine başvurmak `this` için derleme zamanı hatasıdır.

Örnek olay, bir sınıfın belirli bir örneğiyle ilişkilendirilir ve bu örneğe bu olayın erişimcilerinde `this` ([Bu erişim](expressions.md#this-access)) erişilebilir.

Bir olaya, `E.M` `E` `M` formunbir`M` *member_access* ([üye erişimi](expressions.md#member-access)) içinde başvuruluyorsa, bir statik olaydır,içerenbirtürübelirtmelidirvebirörnekolayıdır,E'nin`M` içeren `M`türün bir örneğini gösterir.

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Sanal, korumalı, geçersiz kılma ve soyut olay erişimcileri

`virtual` Olay bildirimi, bu olayın erişimcilerinin sanal olduğunu belirtir. `virtual` Değiştirici her iki bir olayın erişimcisi için geçerlidir.

Bir `abstract` olay bildirimi, olayın erişimcilerinin sanal olduğunu, ancak erişimcilerinin gerçek bir uygulamasını sağlamamayı belirtir. Bunun yerine, Özet olmayan türetilmiş sınıfların, olayı geçersiz kılarak erişimcileri için kendi uygulamasını sağlaması gerekir. Soyut bir olay bildirimi gerçek uygulama sağlamadığı için, küme ayracı ile ayrılmış *event_accessor_declarations*sağlayamaz.

Hem `abstract` hem`override` de değiştiricilerini içeren bir olay bildirimi, olayın soyut olduğunu ve bir temel olayı geçersiz kıldığını belirtir. Böyle bir olayın erişimcileri de soyuttur.

Soyut olay bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)).

Bir `override` değiştirici belirten bir olay bildirimi eklenerek, devralınan bir sanal olayın erişimcileri türetilmiş bir sınıfta geçersiz kılınabilir. Bu, ***geçersiz kılan bir olay bildirimi***olarak bilinir. Geçersiz kılan bir olay bildirimi yeni bir olay bildirmiyor. Bunun yerine, var olan bir sanal olay erişimcilerinin uygulamalarını özelleştirir.

Geçersiz kılan bir olay bildirimi, geçersiz kılınan olayla aynı erişilebilirlik değiştiricilerini, türü ve adı belirtmelidir.

Geçersiz kılan bir olay bildirimi `sealed` değiştiriciyi içerebilir. Bu değiştiricinin kullanılması, türetilmiş bir sınıfın olayı daha fazla geçersiz kılmasını önler. Korumalı bir olayın erişimcileri de korumalıdır.

Geçersiz kılan bir olay bildiriminin bir `new` değiştirici içermesi için derleme zamanı hatasıdır.

Bildirim ve çağırma sözdiziminde farklar haricinde, sanal, korumalı, geçersiz kılma ve soyut erişimciler, sanal, mühürlenmiş, geçersiz kılma ve soyut yöntemler gibi davranır. Özellikle, [sanal yöntemlerde](classes.md#virtual-methods)açıklanan kurallar, [geçersiz kılma yöntemleri](classes.md#override-methods), [korumalı Yöntemler](classes.md#sealed-methods)ve [soyut yöntemler](classes.md#abstract-methods) , erişimciler karşılık gelen bir formun yöntemlerimiz gibi geçerlidir. Her erişimci, olay türünün tek değerli parametresine, `void` dönüş türüne ve kapsayan olay ile aynı değiştiricilere karşılık gelen bir yönteme karşılık gelir.

## <a name="indexers"></a>Dizin Oluşturucular

***Dizin Oluşturucu*** , bir nesnenin bir dizi ile aynı şekilde dizinlenmesini sağlayan bir üyedir. Dizin oluşturucular *indexer_declaration*s kullanılarak bildirilmiştir:

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

Bir *indexer_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers) `new` ) geçerli bir birleşimini içerebilir ([Yeni değiştirici](classes.md#the-new-modifier)) `virtual` , ([ Sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)) `sealed` , ([Sealed Yöntemler](classes.md#sealed-methods) `abstract` ), ([soyut](classes.md#abstract-methods)Yöntemler) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricileri.

Dizin Oluşturucu bildirimleri, bir özel durum, Dizin Oluşturucu bildiriminde statik değiştiriciye izin verilmemesine neden olacak şekilde, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([yöntemleri](classes.md#methods)) ile aynı kurallara tabidir.

, Ve `virtual` değiştiricileri`abstract` tek bir durum dışında birbirini dışlıyor. `override` `abstract` Ve`override` değiştiricileri bir soyut dizin oluşturucunun sanal bir dizin için geçersiz kılabilmesi için birlikte kullanılabilir.

Bir Dizin Oluşturucu bildiriminin *türü* , bildirim tarafından tanıtılan dizin oluşturucunun öğe türünü belirtir. Dizin Oluşturucu açık bir arabirim üyesi uygulama değilse, *türün* ardından anahtar sözcüğü `this`gelir. Açık arabirim üyesi uygulama için, *türün* arkasından bir *interface_type*, "`.`" ve anahtar sözcüğü `this`gelir. Diğer üyelerin aksine, Dizin oluşturucular Kullanıcı tanımlı adlara sahip değildir.

*Formal_parameter_list* , dizin oluşturucunun parametrelerini belirtir. Bir dizin oluşturucunun biçimsel parametre listesi[bir yönteme karşılık](classes.md#method-parameters)gelir, ancak en az bir parametre belirtilmesi ve `ref` ve `out` parametre değiştiricilerine izin verilmemelidir.

Bir dizin oluşturucunun *türü* ve *formal_parameter_list* içinde başvurulan türlerin her biri en azından dizin oluşturucunun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir *indexer_body* , ***erişimci gövdesinden*** ya da bir ***ifade gövdesinden***oluşabilir. Bir erişimci gövdesinde, "`{`" ve "`}`" belirteçlerinin içine alınması gereken accessor_declarations, özelliğin erişimcileri ([erişimcileri](classes.md#accessors)) bildirmek. Erişimciler, özelliği okuma ve yazma ile ilişkili yürütülebilir deyimleri belirler.

"`=>`" İfadesinden sonra gelen `E` ve noktalı virgülden oluşan bir ifade gövdesi deyim gövdesine `{ get { return E; } }`tam olarak eşdeğerdir ve bu nedenle yalnızca alıcı sonucunun tek bir ifade tarafından verilir.

Bir Dizin Oluşturucu öğesine erişmek için sözdizimi, bir dizi öğesiyle aynı olsa da, bir Dizin Oluşturucu öğesi değişken olarak sınıflandırılmaz. Bu nedenle, bir Dizin Oluşturucu öğesini bir `ref` veya `out` bağımsız değişken olarak geçirmek mümkün değildir.

Bir dizin oluşturucunun biçimsel parametre listesi, dizin oluşturucunun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar. Özellikle, bir dizin oluşturucunun imzası, resmi parametrelerinin sayısından ve türlerinden oluşur. Öğe türü ve biçimsel parametrelerinin adları, dizin oluşturucunun imzasının bir parçası değildir.

Bir dizin oluşturucunun imzası aynı sınıfta belirtilen diğer tüm dizin oluşturucularının imzalarından farklı olmalıdır.

Dizin oluşturucular ve Özellikler kavram bakımından çok benzerdir, ancak aşağıdaki yollarla farklılık gösterir:

*  Bir özellik adıyla tanımlanır, ancak bir Dizin Oluşturucu imza tarafından tanımlanır.
*  Bir *simple_name* ([basit adlar](expressions.md#simple-names)) veya *member_access* ([üye erişimi](expressions.md#member-access)) aracılığıyla bir özelliğe erişilir, ancak bir Indexer öğesine bir *element_access* ([Dizin Oluşturucu erişimi](expressions.md#indexer-access)) aracılığıyla erişilir.
*  Bir özellik `static` üye olabilir, ancak bir Dizin Oluşturucu her zaman bir örnek üyesidir.
*  Bir özelliğin `get` erişimcisi parametresi olmayan bir yönteme karşılık gelir, ancak bir dizin oluşturucunun erişimcisi, Dizin oluşturucudaki aynı biçimsel parametre listesine sahip bir yönteme karşılık gelir. `get`
*  Bir özelliğin `value`erişimcisi adlı tek parametreli bir yönteme karşılık gelir, ancak `set` bir dizin oluşturucunun erişimcisi, Dizin Oluşturucu ile aynı biçimsel parametre listesine sahip bir yönteme karşılık gelir ve ek bir parametre `set` Adlandırılmış `value`.
*  Bir Dizin Oluşturucu erişimcisinin bir Dizin Oluşturucu parametresiyle aynı ada sahip yerel bir değişken bildirmesi için derleme zamanı hatası.
*  Geçersiz kılan özellik bildiriminde, devralınan özelliğe sözdizimi `base.P`kullanılarak erişilir, burada `P` özellik adıdır. Geçersiz kılan bir Dizin Oluşturucu bildiriminde, devralınan dizin oluşturucuya sözdizimi `base[E]`kullanılarak erişilir; burada `E` , ifadelerin virgülle ayrılmış bir listesi bulunur.
*  "Otomatik olarak uygulanan Dizin Oluşturucu" kavramı yoktur. Noktalı olmayan, dış olmayan ve olmayan bir dizinleyiciye sahip bir hata.

Bu farklılıklardan farklı olarak, [erişimciler](classes.md#accessors) ve [Otomatik uygulanan özellikleri](classes.md#automatically-implemented-properties) tanımlanan tüm kurallar, Dizin Oluşturucu erişimcileri ve özellik erişimcileri için de geçerlidir.

Bir Dizin Oluşturucu bildirimi bir `extern` değiştirici içerdiğinde, Dizin Oluşturucu bir ***dış Dizin Oluşturucu***olarak kabul edilir. Bir dış Dizin Oluşturucu bildirimi gerçek uygulama sağladığından, *accessor_declarations* her biri noktalı virgülle oluşur.

Aşağıdaki örnek, bit dizisindeki `BitArray` tek tek bitlerin erişimine yönelik bir Dizin Oluşturucu uygulayan bir sınıf bildirir.
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

`BitArray` Sınıfının bir örneği, buna karşılık gelen `bool[]` (ilk değeri ikinci bir baytlık bir bit yerine yalnızca bir bit kapladığından) büyük ölçüde daha az bellek tüketir, ancak aynı işlemlere bir `bool[]`ile aynı şekilde izin verir.

Aşağıdaki `CountPrimes` sınıf, 1 ile `BitArray` verilen bir maksimum arasındaki primün sayısını hesaplamak için bir ve klasik "sıya" algoritmasını kullanır:
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

Öğesinin `BitArray` öğelerine erişim sözdiziminin tam olarak bir `bool[]`ile aynı olduğunu unutmayın.

Aşağıdaki örnek, iki parametreli bir dizin oluşturucuya sahip bir 26 * 10 Grid sınıfı gösterir. İlk parametrenin A-Z aralığında bir büyük veya küçük harf olması gerekir ve ikincisinin 0-9 aralığında bir tamsayı olması gerekir.

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

### <a name="indexer-overloading"></a>Dizin Oluşturucu aşırı yüklemesi

Dizin Oluşturucu aşırı yükleme çözümleme kuralları [tür çıkarımı](expressions.md#type-inference)' nda açıklanmıştır.

## <a name="operators"></a>İşleçler

***İşleci*** , sınıfının örneklerine uygulanabilen bir ifade işlecinin anlamını tanımlayan bir üyesidir. İşleçler *operator_declaration*s kullanılarak bildirilmiştir:

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
    | right_shift | '=='  | '!='  | '>'   | '<'   | '>='  | '<='
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

Fazla yüklenebilir operatörlerin üç kategorisi vardır: Birli İşleçler ([Birli İşleçler](classes.md#unary-operators)), ikili Işleçler ([ikili işleçler](classes.md#binary-operators)) ve dönüştürme işleçleri ([dönüştürme işleçleri](classes.md#conversion-operators)).

*Operator_body* , bir noktalı virgül, bir ***deyim gövdesi*** veya bir ***ifade gövdesidir***. Deyim gövdesi, işleç çağrıldığında yürütülecek deyimleri belirten bir *bloğundan*oluşur. *Blok* , [Yöntem gövdesinde](classes.md#method-body)açıklanan değer döndüren yöntemlere yönelik kurallara uymalıdır. Bir ifade gövdesi, `=>` sonrasında bir ifade ve noktalı virgül ile oluşur ve işleç çağrıldığında gerçekleştirilecek tek bir ifadeyi gösterir.

İşleçler `extern` için, *operator_body* yalnızca noktalı virgülden oluşur. Diğer tüm işleçler için, *operator_body* bir blok gövdedir veya bir ifade gövdesidir.

Tüm işleç bildirimleri için aşağıdaki kurallar geçerlidir:

*  İşleç bildirimi hem a `public` `static` hem de değiştirici içermelidir.
*  Bir işlecin parametre (ler) i değer parametreleri olmalıdır ([değer parametreleri](variables.md#value-parameters)). Bu, bir işleç bildirimi için veya `ref` `out` parametreleri belirtmek üzere derleme zamanı hatasıdır.
*  Bir işlecin imzası ([Birli İşleçler](classes.md#unary-operators), [ikili işleçler](classes.md#binary-operators), [dönüştürme işleçleri](classes.md#conversion-operators)) aynı sınıfta belirtilen diğer tüm işleçlerin imzalarından farklı olmalıdır.
*  Bir işleç bildiriminde başvurulan tüm türlerin en az işlecin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olması gerekir.
*  Aynı değiştiricinin bir operatör bildiriminde birden çok kez görünmesi bir hatadır.

Her operatör kategorisi, aşağıdaki bölümlerde açıklandığı gibi ek kısıtlamalar uygular.

Diğer Üyeler gibi, bir temel sınıfta belirtilen operatörler türetilmiş sınıflar tarafından devralınır. İşleç bildirimleri, işlecin, işlecin imzasına katılması için bildirildiği sınıf veya yapının her zaman gerektirdiğinden, bir temel sınıfta belirtilen bir işleci gizlemek için türetilmiş sınıfta belirtilen bir operatör mümkün değildir. Bu nedenle, `new` değiştirici hiçbir şekilde gerekli değildir ve bu nedenle bir işleç bildiriminde hiçbir izin verilmez.

Birli ve ikili işleçlerle ilgili ek bilgiler, [işleçlerinde](expressions.md#operators)bulunabilir.

Dönüştürme işleçleri hakkında ek bilgi, [Kullanıcı tanımlı dönüştürmelerde](conversions.md#user-defined-conversions)bulunabilir.

### <a name="unary-operators"></a>Birli İşleçler

Aşağıdaki kurallar, `T` işleç bildirimini içeren sınıfın veya yapının örnek türünü belirten birli operatör bildirimleri için geçerlidir:

*  `+`Birli `T?` , `-`, ,`!`veya işleç`~` türünde`T` tek bir parametre almalıdır, ya da herhangi bir tür döndürebilir.
*  Birli `++` or `--` işleci, `T` türünde`T?` tek bir parametre almalıdır ve aynı türü veya ondan türetilmiş bir türü döndürmelidir.
*  Birli `true` veya `false` işleç türünde `T` tekbirparametre`bool`almalıdır ve türü döndürmelidir. `T?`

Birli`+`işlecin imzası, işleç belirtecinden ( `~` `!`, `-` `++`,,,,, veya`false`) ve tek biçimsel parametrenin türünden oluşur. `--` `true` Dönüş türü birli işlecin imzasının bir parçası değildir, ya da biçimsel parametrenin adıdır.

`true` Ve`false` Birli İşleçler çift yönlü bildirim gerektirir. Bir sınıf bu işleçlerden birini diğeri de bildirmeksizin bildirirse bir derleme zamanı hatası oluşur. Ve `true` [](expressions.md#boolean-expressions) [](expressions.md#user-defined-conditional-logical-operators) işleçleri, Kullanıcı tanımlı Koşullu mantıksal işleçler ve Boole ifadelerinde daha ayrıntılı olarak açıklanmıştır. `false`

Aşağıdaki örnek, bir tamsayı vektör sınıfı `operator ++` için bir uygulama ve sonraki kullanımını gösterir:
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

İşleç yönteminin, sonek artırma ve azaltma işleçleri ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve önek artırma ve azaltma Işleçleri ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)). Uygulamasının aksine C++, bu yöntemin işlenenin değerini doğrudan değiştirmesi gerekmez. Aslında, işlenen değerini değiştirmek, sonek artışı işlecinin standart semantiğini ihlal ediyor.

### <a name="binary-operators"></a>İkili işleçler

Aşağıdaki kurallar, `T` işleç bildirimini içeren sınıfın veya yapının örnek türünü belirten ikili işleç bildirimleri için geçerlidir:

*  Binary olmayan bir operatör iki parametre almalıdır, en az bir, türü `T` veya `T?`olmalıdır ve herhangi bir tür döndürebilir.
*  Bir ikili `<<` veya `>>` işleç iki parametre almalıdır, ilki türü `T` veya `T?` , ikincisinin türü `int` veya `int?`olmalıdır ve herhangi bir tür döndürebilir.

Bir ikili işlecinin`+`imzası, işleç belirtecinden oluşur (, `|` `&` `/` `-`, `*` `%`,,,, `^`,, `<<`, `>>`,,,,, `==` ,`!=`, ,`>=`,, veya`<=`) ve iki biçimsel parametrenin türleri. `<` `>` Dönüş türü ve biçimsel parametrelerin adları bir ikili işlecin imzasının parçası değil.

Belirli ikili işleçler çift yönlü bildirim gerektirir. Bir çiftin her iki işlecinin her bildirimi için, çiftin diğer işlecinin eşleşen bir bildirimi olması gerekir. İki işleç bildirimi aynı dönüş türüne ve her parametre için aynı türe sahip olduklarında eşleşir. Aşağıdaki işleçler çift yönlü bildirim gerektirir:

*  `operator ==` ve `operator !=`
*  `operator >` ve `operator <`
*  `operator >=` ve `operator <=`

### <a name="conversion-operators"></a>Dönüştürme işleçleri

Bir dönüştürme işleci bildirimi, önceden tanımlanmış örtük ve açık dönüştürmeleri genişleten ***Kullanıcı tanımlı bir dönüştürme*** ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) sunar.

Anahtar sözcüğünü içeren bir dönüştürme işleci bildirimi `implicit` , Kullanıcı tanımlı bir örtük dönüştürmeyi tanıtır. Örtük dönüştürmeler işlev üyesi etkinleştirmeleri, atama ifadeleri ve atamalar dahil çeşitli durumlarda gerçekleşebilir. Bu, [örtük dönüştürmelerde](conversions.md#implicit-conversions)daha ayrıntılı olarak açıklanmıştır.

Anahtar sözcüğünü içeren bir dönüştürme işleci bildirimi `explicit` , Kullanıcı tanımlı bir açık dönüştürme sunar. Dönüştürme ifadelerinde açık dönüştürmeler gerçekleşebilir ve [Açık dönüştürmelerde](conversions.md#explicit-conversions)daha ayrıntılı olarak açıklanmıştır.

Bir dönüştürme işleci, dönüştürme işlecinin parametre türü ile belirtilen bir kaynak türünden, dönüştürme işlecinin dönüş türü tarafından belirtilen bir hedef türüne dönüştürülür.

Belirli bir kaynak türü `S` ve hedef türü `T`için, null `S` yapılabilir `T` türler varsa, kendi temel `S0` türlerine `T0` izin verin ve bunlara başvurun, aksi `S0` halde `T0` `S` eşittir ve`T` sırasıyla. Bir sınıf veya yapının, yalnızca aşağıdakilerin tümü doğru olduğunda bir kaynak türünden `S` hedef türüne `T` dönüştürme bildirmesine izin verilir:

*  `S0`ve `T0` farklı türlerdir.
*  `S0` Ya`T0` da işleç bildiriminin gerçekleştiği sınıf veya yapı türüdür.
*  Ne `S0` de `T0` bir *interface_type*değildir.
*  Kullanıcı tanımlı dönüştürmeler hariç olmak üzere `S` dönüştürme, ' den `T` ' e veya `T` `S`' den ' a arasında bulunmaz.

Bu kuralların amaçları doğrultusunda, veya `S` `T` ile ilişkili herhangi bir tür parametresi, diğer türlerle devralma ilişkisine sahip olmayan benzersiz türler olarak kabul edilir ve bu tür parametrelerinin herhangi bir kısıtlaması yok sayılır.

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
ilk iki işleç bildirimi izin verilir çünkü, .3, `T` ve `int` `string` sırasıyla [Dizin oluşturucular](classes.md#indexers), hiçbir ilişki olmadan benzersiz türler olarak kabul edilir. Ancak, üçüncü işleç bir hatadır çünkü `C<T>` öğesinin `D<T>`temel sınıfı.

İkinci kuraldan, bir dönüştürme işlecinin, işlecin bildirildiği sınıf veya yapı türünden ya da türüne dönüştürülmesi gerekir. Örneğin, bir sınıf `C` veya yapı türü, `C`' den ' `int` a `int` `C` `int` ve kaynağından öğesine dönüştürme tanımlamak için mümkündür. `bool`

Önceden tanımlanmış bir dönüştürmeyi doğrudan yeniden tanımlamak mümkün değildir. Bu nedenle, örtük ve açık dönüştürmeler diğer tüm türler arasında `object` `object` zaten mevcut olduğundan, dönüştürme işleçleri veya ' den ' a dönüştürme yapılamaz. Benzer şekilde, bir dönüştürme daha sonra zaten mevcut olduğundan, bir dönüştürmenin kaynağı veya hedef türleri diğerinin temel türü olamaz.

Ancak, belirli tür bağımsız değişkenleri için önceden tanımlanmış dönüştürmeler olarak zaten var olan dönüştürmeleri belirtmek üzere genel türlerde işleçler bildirmek mümkündür. Örnekte
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
türü `object` `object`için `T`tür bağımsız değişkeni olarak belirtildiğinde, İkinci işleç zaten var olan bir dönüştürme (örtük olarak) ve bu nedenle bir açık, her türden türden bir dönüşüm bulunur) bildirir.

İki tür arasında önceden tanımlanmış bir dönüştürmenin mevcut olduğu durumlarda, bu türler arasındaki Kullanıcı tanımlı dönüştürmeler yok sayılır. Engelle

*  Tür `S` `T` [](conversions.md#implicit-conversions) `S` türündenbiröncedentanımlanmışörtükdönüştürme(örtükdönüştürmeler)varsa,'den'ekadarKullanıcı`T`tanımlı dönüştürmeler (örtük veya açık) yok sayılır.
*  Türünden türüne `T` `S` `S` [](conversions.md#explicit-conversions) öncedentanımlanmışbiraçıkdönüştürme(açıkdönüştürmeler)varsa,öğesindenöğesineKullanıcı`T`tanımlı açık dönüştürmeler yok sayılır. Buna

Bir arabirim türü ise, ' den `S` ' e `T` Kullanıcı tanımlı örtük dönüştürmeler yok sayılır. `T`

Aksi takdirde, ' den `S` ' e `T` Kullanıcı tanımlı örtük dönüştürmeler hala göz önünde bulundurulacaktır.

Tüm türler `object`için, yukarıdaki `Convertible<T>` tür tarafından belirtilen işleçler önceden tanımlanmış dönüşümlerle çakışmaz. Örneğin:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Ancak, tür `object`, önceden tanımlı dönüştürmeler için Kullanıcı tanımlı dönüştürmeleri her durumda gizleyin, ancak bir:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Kullanıcı tanımlı Dönüştürmelere veya *interface_type*s 'ye dönüştürülmesine izin verilmez. Özellikle, bu kısıtlama bir *interface_type*dönüştürme sırasında hiçbir Kullanıcı tanımlı dönüştürme gerçekleşmemesini ve bir *interface_type* dönüştürmenin, yalnızca dönüştürülecek nesne gerçekten Belirtilen *interface_type*.

Bir dönüştürme işlecinin imzası, kaynak türü ve hedef türünden oluşur. (Bunun, dönüş türünün imzaya katıldığı tek üye formu olduğunu unutmayın.) Bir dönüştürme `explicit` işlecinin veyasınıflandırması,işlecinimzasınınbirparçasıdeğil.`implicit` Bu nedenle, bir sınıf veya yapı aynı kaynak ve `implicit` hedef türlerine `explicit` sahip hem hem de bir dönüştürme işleci bildiremez.

Genel olarak, Kullanıcı tanımlı örtük dönüştürmeler hiçbir şekilde özel durum oluşturmaz ve hiçbir bilgi kaybedilmez. Kullanıcı tanımlı bir dönüştürme özel durumlara (örneğin, kaynak bağımsız değişkeni aralık dışında) veya bilgi kaybına (yüksek sıralı bitleri atma gibi) izin verebileceğinden, bu dönüştürme açık bir dönüştürme olarak tanımlanmalıdır.

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
' den `Digit` ' e `byte` dönüştürme örtük olarak özel durum oluşturmaz veya bilgi kaybettiğinden, `Digit` ancak yalnızca olası bir `byte` alt kümesini temsil ettiğinden `Digit` , öğesinden dönüşümü açık hale gelir. bir `byte`.

## <a name="instance-constructors"></a>Örnek oluşturucular

***Örnek Oluşturucu*** , bir sınıfın örneğini başlatmak için gereken eylemleri uygulayan bir üyedir. Örnek oluşturucular *constructor_declaration*s kullanılarak bildirilmiştir:

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

Bir *constructor_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)), dört erişim değiştiricisinin geçerli bir birleşimini ([erişim değiştiricileri](classes.md#access-modifiers)) ve bir `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricisini içerebilir. Oluşturucu bildiriminin aynı değiştiriciyi birden çok kez içerme izni yoktur.

Bir *constructor_declarator* öğesinin *tanımlayıcısı* , örnek oluşturucusunun bildirildiği sınıfı içermelidir. Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.

Bir örnek oluşturucusunun isteğe bağlı *formal_parameter_list* , bir yöntemin *formal_parameter_list* ([metotlar](classes.md#methods)) ile aynı kurallara tabidir. Biçimsel parametre listesi bir örnek oluşturucusunun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar ve aşırı yükleme çözümünün ([tür çıkarımı](expressions.md#type-inference)) bir çağrıdaki belirli bir örnek oluşturucusunu seçtiği işlemi yönetir.

Bir örnek oluşturucusunun *formal_parameter_list* başvuruda bulunulan türlerin her biri, oluşturucunun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak en az erişilebilir olmalıdır.

İsteğe bağlı *constructor_initializer* , bu örnek oluşturucusunun *constructor_body* ' de verilen deyimleri yürütmeden önce çağrılacak başka bir örnek Oluşturucu belirtir. Bu, [Oluşturucu başlatıcılarda](classes.md#constructor-initializers)daha ayrıntılı olarak açıklanmıştır.

Bir Oluşturucu bildirimi bir `extern` değiştirici içerdiğinde, Oluşturucu bir ***dış Oluşturucu***olarak kabul edilir. Bir dış Oluşturucu bildirimi gerçek uygulama sağladığından, *constructor_body* noktalı virgül içerir. Diğer tüm oluşturucular için *constructor_body* , sınıfının yeni bir örneğini başlatmak için deyimleri belirten bir *bloğundan* oluşur. Bu, `void` dönüş türü ([Yöntem gövdesi](classes.md#method-body)) olan bir örnek yönteminin *bloğuna* tam olarak karşılık gelir.

Örnek oluşturucular devralınmaz. Bu nedenle, bir sınıf sınıf içinde gerçekten bildirilenler dışında bir örnek Oluşturucu içermez. Bir sınıf örnek Oluşturucu bildirimleri içermiyorsa, varsayılan bir örnek Oluşturucu otomatik olarak sağlanır ([Varsayılan oluşturucular](classes.md#default-constructors)).

Örnek oluşturucular, *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) ve *constructor_initializer*s aracılığıyla çağrılır.

### <a name="constructor-initializers"></a>Oluşturucu başlatıcıları

Tüm örnek oluşturucular (sınıf `object`dışındakiler hariç), *constructor_body*hemen öncesine başka bir örnek oluşturucusunun bir çağrılmasını dolaylı olarak içerir. Örtük olarak çağrılacak Oluşturucu *constructor_initializer*tarafından belirlenir:

*  Formun `base(argument_list)` bir örnek Oluşturucu başlatıcısı veya `base()` doğrudan taban sınıftan çağrılabilir bir örnek oluşturucusunun oluşmasına neden olur. Bu Oluşturucu, varsa *argument_list* ve [aşırı yükleme](expressions.md#overload-resolution)çözümlemesi çözüm kuralları kullanılarak seçilir. Aday örnek oluşturucular kümesi, doğrudan Taban sınıfında yer alan tüm erişilebilir örnek oluşturuculardan veya doğrudan temel sınıfta hiçbir örnek Oluşturucu bildirilmemiş ise varsayılan oluşturucuda ([Varsayılan oluşturucular](classes.md#default-constructors)) oluşur. Bu küme boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir derleme zamanı hatası oluşur.
*  Formun `this(argument-list)` bir örnek Oluşturucu başlatıcısı veya `this()` sınıfın kendisinden bir örnek oluşturucusunun çağrılmasını sağlar. Oluşturucu, varsa *argument_list* ve [aşırı yükleme](expressions.md#overload-resolution)çözümlemesi çözüm kuralları kullanılarak seçilir. Aday örnek oluşturucular kümesi, sınıfta belirtilen tüm erişilebilir örnek oluşturuculardan oluşur. Bu küme boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir derleme zamanı hatası oluşur. Bir örnek Oluşturucu bildirimi, oluşturucuyu çağıran bir Oluşturucu başlatıcısı içeriyorsa, bir derleme zamanı hatası oluşur.

Bir örnek oluşturucusunun Oluşturucu başlatıcısı yoksa, formun `base()` bir Oluşturucu başlatıcısı örtülü olarak sağlanır. Bu nedenle, formun örnek Oluşturucu bildirimi
```csharp
C(...) {...}
```
tam olarak eşdeğerdir
```csharp
C(...): base() {...}
```

Bir örnek Oluşturucu bildiriminin *formal_parameter_list* tarafından verilen parametrelerin kapsamı, bu bildirimin Oluşturucu başlatıcısını içerir. Bu nedenle, bir Oluşturucu başlatıcısının, oluşturucunun parametrelerine erişmesine izin verilir. Örneğin:
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

Örnek Oluşturucu Başlatıcısı oluşturulan örneğe erişemez. Bu nedenle, Oluşturucu başlatıcısının bağımsız değişken ifadesinde başvurulmasına `this` yönelik derleme zamanı hatası, bir bağımsız değişken ifadesi için bir *simple_name*aracılığıyla herhangi bir örnek üyesine başvuruda bulunmak üzere bir derleme zamanı hatası olur.

### <a name="instance-variable-initializers"></a>Örnek değişken başlatıcıları

Bir örnek oluşturucusunun Oluşturucu başlatıcısı olmadığında veya bu kullanıcının formun `base(...)`Oluşturucu başlatıcısı varsa, bu Oluşturucu, örnek alanlarının *variable_initializer*s tarafından belirtilen başlatmaları örtük olarak gerçekleştirir sınıfında bildirilmiştir. Bu, oluşturucuya girişte ve doğrudan temel sınıf oluşturucusunun örtük çağrılmasıyla önce yürütülen bir atama dizisine karşılık gelir. Değişken başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülür.

### <a name="constructor-execution"></a>Oluşturucu yürütme

Değişken başlatıcıları atama ifadelerine dönüştürülür ve bu atama deyimleri, temel sınıf örneği oluşturucusunun çağrısından önce yürütülür. Bu sıralama, bu örneğe erişimi olan herhangi bir deyim yürütülmeden önce tüm örnek alanlarının değişken başlatıcılara göre başlatılmasını sağlar.

Örnek verildiğinde
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
`new B()` öğesinin`B`bir örneğini oluşturmak için kullanıldığında, aşağıdaki çıktı üretilir:
```
x = 1, y = 0
```

Değeri, taban `x` sınıf örneği Oluşturucusu çağrılmadan önce, değişken başlatıcısı yürütüldüğü için 1 ' dir. Ancak, öğesine atama, `y` taban sınıf oluşturucusu döndürülünceye kadar yürütülmediği `int`için `y` , değeri 0 ' dır (varsayılan değeri).

*Constructor_body*öncesinde otomatik olarak eklenen deyimler olarak örnek değişken başlatıcıları ve Oluşturucu başlatıcıları düşünmek yararlıdır. Örnek
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
çeşitli değişken başlatıcıları içerir; Ayrıca, her iki formun de Oluşturucu başlatıcıları (`base` ve `this`) içerir. Örnek, aşağıda gösterilen koda karşılık gelir; burada her yorum otomatik olarak yerleştirilen bir ifadeyi gösterir (otomatik olarak yerleştirilen Oluşturucu etkinleştirmeleri için kullanılan söz dizimi geçerli değildir, ancak yalnızca mekanizmayı göstermeye çalışır).

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

### <a name="default-constructors"></a>Varsayılan oluşturucular

Bir sınıf örnek Oluşturucu bildirimleri içermiyorsa, varsayılan bir örnek Oluşturucu otomatik olarak sağlanır. Bu varsayılan Oluşturucu yalnızca doğrudan temel sınıfın parametresiz oluşturucusunu çağırır. Sınıf soyut ise, varsayılan Oluşturucu için belirtilen erişilebilirlik korunur. Aksi takdirde, varsayılan Oluşturucu için belirtilen erişilebilirlik geneldir. Bu nedenle, varsayılan Oluşturucu her zaman formundadır

```csharp
protected C(): base() {}
```
veya
```csharp
public C(): base() {}
```
Burada `C` sınıfın adıdır. Aşırı yükleme çözümlemesi temel sınıf Oluşturucu başlatıcısı için benzersiz bir en iyi aday tespit leyemiyorsa, derleme zamanı hatası oluşur.

Örnekte
```csharp
class Message
{
    object sender;
    string text;
}
```
sınıf örnek Oluşturucu bildirimleri içerdiğinden, varsayılan bir Oluşturucu sağlanır. Bu nedenle, örnek tam olarak şu şekilde eşdeğerdir
```csharp
class Message
{
    object sender;
    string text;

    public Message(): base() {}
}
```

### <a name="private-constructors"></a>Özel oluşturucular

Bir sınıf `T` yalnızca özel örnek oluşturucular bildirdiğinde, ' ın `T` program metni dışındaki sınıflar, ' ın `T`' dan `T` türetilmiş veya doğrudan örnekleri oluşturmak için mümkün değildir. Bu nedenle, bir sınıf yalnızca statik üyeler içeriyorsa ve örneği oluşturulması amaçlanmazsa, boş bir özel örnek Oluşturucu eklemek, örnek oluşturmayı engeller. Örneğin:
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

`Trig` Sınıfı ilgili yöntemleri ve sabitleri gruplandırır, ancak örneği oluşturulacak şekilde tasarlanmamıştır. Bu nedenle, tek bir boş özel örnek Oluşturucu bildirir. Varsayılan oluşturucunun otomatik olarak oluşturulmasını engellemek için en az bir örnek Oluşturucu bildirilmelidir.

### <a name="optional-instance-constructor-parameters"></a>İsteğe bağlı örnek Oluşturucu parametreleri

Oluşturucu `this(...)` başlatıcısı formu, isteğe bağlı örnek oluşturucu parametrelerini uygulamak için yaygın olarak aşırı yükleme ile birlikte kullanılır. Örnekte
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
ilk iki örnek Oluşturucu yalnızca eksik bağımsız değişkenler için varsayılan değerleri sağlar. Her ikisi de `this(...)` , yeni örneği başlatma işini gerçekten yapan üçüncü örnek oluşturucuyu çağırmak için bir Oluşturucu başlatıcısı kullanın. Bu, isteğe bağlı Oluşturucu parametrelerinin etkidir:
```csharp
Text t1 = new Text();                    // Same as Text(0, 0, null)
Text t2 = new Text(5, 10);               // Same as Text(5, 10, null)
Text t3 = new Text(5, 20, "Hello");
```

## <a name="static-constructors"></a>Statik oluşturucular

***Statik Oluşturucu*** , kapalı bir sınıf türünü başlatmak için gereken eylemleri uygulayan bir üyedir. Statik oluşturucular *static_constructor_declaration*s kullanılarak bildirilmiştir:

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

Bir *static_constructor_declaration* , *öznitelik* ( `extern` [öznitelikler](attributes.md)) ve değiştirici ([dış Yöntemler](classes.md#external-methods)) kümesini içerebilir.

Bir *static_constructor_declaration* öğesinin *tanımlayıcısı* , statik oluşturucunun bildirildiği sınıfı adı vermelidir. Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.

Statik Oluşturucu bildirimi bir `extern` değiştirici içerdiğinde, statik oluşturucu bir ***dış statik Oluşturucu***olarak kabul edilir. Dış bir statik Oluşturucu bildirimi gerçek uygulama sunmadığından, *static_constructor_body* bir noktalı virgül içerir. Diğer tüm statik Oluşturucu bildirimleri için, *static_constructor_body* , sınıfını başlatmak üzere yürütülecek deyimleri belirten bir *bloğundan* oluşur. Bu, `void` dönüş türü ([Yöntem gövdesi](classes.md#method-body)) ile bir statik metodun *method_body* öğesine karşılık gelir.

Statik oluşturucular devralınmaz ve doğrudan çağrılamaz.

Kapalı bir sınıf türü için statik oluşturucu, belirli bir uygulama etki alanında en çok bir kez yürütülür. Bir statik oluşturucunun yürütülmesi, bir uygulama etki alanı içinde gerçekleşecek aşağıdaki olayların ilki tarafından tetiklenir:

*  Sınıf türünün bir örneği oluşturulur.
*  Sınıf türünün statik üyelerinden herhangi birine başvurulur.

Bir sınıf yürütmenin başladığı `Main` yöntemi ([uygulama başlatma](basic-concepts.md#application-startup)) içeriyorsa, bu `Main` sınıfın statik Oluşturucusu Yöntem çağrılmadan önce yürütülür.

Yeni bir kapalı sınıf türünü başlatmak için, önce bu kapalı tür için yeni bir statik alanlar kümesi ([statik ve örnek alanları](classes.md#static-and-instance-fields)) oluşturulur. Statik alanların her biri varsayılan değerine ([varsayılan değerler](variables.md#default-values)) başlatılır. Daha sonra, statik alan başlatıcıları ([statik alan başlatma](classes.md#static-field-initialization)) Bu statik alanlar için yürütülür. Son olarak, statik Oluşturucu yürütülür.

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
Çıktının üretilmesi gerekir:
```
Init A
A.F
Init B
B.F
```
statik `A`oluşturucunun yürütülmesi `A.F`çağrısı tarafından tetiklendiğinden ve statik oluşturucunun yürütülmesi `B`öğesine `B.F`yapılan çağrısıyla tetiklendiğinden.

Değişken başlatıcıların varsayılan değer durumunda gözlenecek statik alanlara izin veren dairesel bağımlılıklar oluşturmak mümkündür.

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

`Main` Yöntemi yürütmek için, sistemin, sınıfının `B`statik Oluşturucusu öncesinde, için `B.Y`başlatıcıyı çalıştırması gerekir. `Y`öğesinin başlatıcısı, `A` `A.X` değerine başvurulduğundan statik oluşturucunun çalışmasına neden olur.  `A` ' In ' in statik Oluşturucusu `X`değerini hesaplamak için ' i ve ' nin varsayılan değerini `Y`getirir, sıfır değeridir. `A.X`Bu nedenle 1 olarak başlatılır. Statik alan başlatıcıları ve `A`statik Oluşturucu çalıştırma işlemi daha sonra, öğesinin `Y`başlangıç değerinin hesaplamasına dönerek tamamlanır ve bunun sonucu 2 olur.

Statik Oluşturucu her bir kapalı oluşturulmuş sınıf türü için tam olarak bir kez yürütüldüğü için, tür parametresinde, kısıtlamalar aracılığıyla ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) denetlenemeyen çalışma zamanı denetimlerini zorlamak için kullanışlı bir yerdir. . Örneğin, aşağıdaki tür, tür bağımsız değişkeninin bir sabit listesi olmasını zorlamak için bir statik Oluşturucu kullanır:
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

***Yıkıcı*** , bir sınıf örneğinin yapısını kaldırma için gereken eylemleri uygulayan bir üyesidir. Yıkıcı bir *destructor_declaration*kullanılarak bildirilmiştir:

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

Bir *destructor_declaration* bir *öznitelikler* kümesi içerebilir ([öznitelikler](attributes.md)).

Bir *destructor_declaration* öğesinin *tanımlayıcısı* , yok edicinin bildirildiği sınıfı adını vermelidir. Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.

Yıkıcı bildirimi bir `extern` değiştirici içerdiğinde, yıkıcı bir ***dış yıkıcı***olarak kabul edilir. Bir dış yıkıcı bildirimi gerçek uygulama sunmadığından, *destructor_body* bir noktalı virgül içerir. Diğer tüm Yıkıcılar için, *destructor_body* sınıfının bir örneğini bırakmak için yürütülecek deyimleri belirten bir *bloğundan* oluşur. Bir *destructor_body* , `void` dönüş türü ([Yöntem gövdesi](classes.md#method-body)) olan bir örnek yönteminin *method_body* öğesine karşılık gelir.

Yok ediciler devralınmaz. Bu nedenle, bir sınıfta bu sınıfta bildirilebilecek olandan başka yok edicisi yoktur.

Yok edicinin parametresi olmaması gerektiğinden aşırı yüklenemez, bu nedenle bir sınıf, en fazla bir yıkıcıya sahip olabilir.

Yok ediciler otomatik olarak çağrılır ve açıkça çağrılamaz. Bir örnek, hiçbir kodun bu örneği kullanabilmesi için artık mümkün olmadığında yok etme için uygun hale gelir. Örnek için yıkıcının yürütülmesi, örnek yok etme için uygun hale geldikten sonra herhangi bir zamanda gerçekleşebilir. Bir örnek çıkarlandığınızda, bu örneğin devralma zincirindeki yok ediciler, en çok türetilen ve en az türetilmiş olarak çağrılır. Herhangi bir iş parçacığında yıkıcı çalıştırılabilir. Yok edicinin ne zaman ve nasıl yürütüleceğini belirleyen kurallara ilişkin daha fazla tartışma için bkz. [Otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management).

Örneğin çıktısı
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
devralma zincirindeki yok ediciler sırasıyla, en çok türetilen ve en az türeten olarak çağrıldıklarından.

Yok ediciler, üzerinde `Finalize` `System.Object`sanal yöntemi geçersiz kılınarak uygulanır. C#programların bu yöntemi geçersiz kılmasına veya doğrudan çağırmasına (ya da geçersiz kılınmasına) izin verilmez. Örneğin, program
```csharp
class A 
{
    override protected void Finalize() {}    // error

    public void F() {
        this.Finalize();                     // error
    }
}
```
iki hata içerir.

Derleyici, bu yöntem ve geçersiz kılmalar gibi davranır. Bu nedenle, bu program:
```csharp
class A 
{
    void Finalize() {}                            // permitted
}
```
geçerlidir ve gösterilen yöntem `System.Object` `Finalize` yöntemi gizler.

Yıkıcıdan bir özel durum oluştuğunda davranış tartışması için bkz. [özel durumların nasıl işlendiği](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Yineleyiciler

Yineleyici bloğu ([bloklar](statements.md#blocks)) kullanılarak uygulanan bir işlev üyesine ([işlev üyeleri](expressions.md#function-members)) ***Yineleyici***denir.

Bir yineleyici bloğu, karşılık gelen işlev üyesinin dönüş türü Numaralandırıcı arabirimlerinden ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) biri veya sıralanabilir arabirimlerin ([sıralanabilir arabirimler](classes.md#enumerable-interfaces)) biri olduğu sürece bir işlev üyesinin gövdesi olarak kullanılabilir. . *Method_body*, *operator_body* veya *accessor_body*gibi bir durum oluşabilir, ancak olaylar, örnek oluşturucular, statik oluşturucular ve Yıkıcılar yineleyiciler olarak uygulanamaz.

Bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesinin biçimsel parametre listesi için herhangi `ref` bir veya `out` parametresi belirtmek üzere derleme zamanı hatası olur.

### <a name="enumerator-interfaces"></a>Numaralandırıcı arabirimleri

***Numaralandırıcı arabirimleri*** genel olmayan arabirimdir `System.Collections.IEnumerator` ve genel arabirimin `System.Collections.Generic.IEnumerator<T>`tüm örneklemelerinden oluşur. Breçekimi 'nin sake 'ı için bu bölümde sırasıyla ve `IEnumerator` `IEnumerator<T>`olarak başvurulur.

### <a name="enumerable-interfaces"></a>Sıralanabilir arabirimler

***Sıralanabilir arabirimler*** genel olmayan arabirimdir `System.Collections.IEnumerable` ve genel arabirimin `System.Collections.Generic.IEnumerable<T>`tüm örneklemelerinden oluşur. Breçekimi 'nin sake 'ı için bu bölümde sırasıyla ve `IEnumerable` `IEnumerable<T>`olarak başvurulur.

### <a name="yield-type"></a>Yield türü

Bir yineleyici, hepsi aynı türden bir değer dizisi üretir. Bu tür, yineleyicinin ***yield türü*** olarak adlandırılır.

*  `IEnumerator` Veya`IEnumerable`döndüren bir yineleyicinin yield türü. `object`
*  `IEnumerator<T>` Veya`IEnumerable<T>`döndüren bir yineleyicinin yield türü. `T`

### <a name="enumerator-objects"></a>Numaralandırıcı nesneleri

Bir Numaralandırıcı arabirim türü döndüren bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesini çağırmak kodu Yineleyici bloğunda hemen yürütmez. Bunun yerine, bir ***Numaralandırıcı nesnesi*** oluşturulup döndürülür. Bu nesne Yineleyici bloğunda belirtilen kodu kapsüller ve Numaralandırıcı nesnesinin `MoveNext` yöntemi çağrıldığında Yineleyici bloğunda kodun yürütülmesi oluşur. Bir Numaralandırıcı nesnesi aşağıdaki özelliklere sahiptir:

*  , Ve `IEnumerator` `IEnumerator<T>`' nin uyguladığı `T` , yineleyicinin yield türüdür.
*  Uygular `System.IDisposable`.
*  Bağımsız değişken değerlerinin bir kopyasıyla başlatılır (varsa) ve örnek değeri işlev üyesine geçirilir.
*  Dört olası durum, daha ***önce***, ***çalışıyor***, ***askıya alındı***ve ***sonra***ilk olarak ***önceki*** durumda olur.

Numaralandırıcı nesnesi genellikle Yineleyici bloğundaki kodu kapsülleyen ve Numaralandırıcı arabirimlerini uygulayan derleyici tarafından oluşturulan Numaralandırıcı sınıfının bir örneğidir, ancak diğer uygulama yöntemleri mümkündür. Bir Numaralandırıcı sınıfı derleyici tarafından oluşturulduysa, bu sınıf doğrudan veya dolaylı olarak işlev üyesini içeren sınıfta iç içe gelir, özel erişilebilirliği olur ve derleyici kullanımı ([tanımlayıcılar](lexical-structure.md#identifiers)) için ayrılmış bir ada sahip olur.

Bir Numaralandırıcı nesnesi, yukarıda belirtilenden daha fazla arabirim uygulayabilir.

Aşağıdaki bölümlerde,, ve bir Numaralandırıcı nesnesi tarafından `MoveNext`sunulan `Current` `IEnumerable` ve `IEnumerable<T>` arabirim `Dispose` uygulamalarının üyelerinin tam davranışı açıklanır.

Numaralandırıcı nesnelerinin `IEnumerator.Reset` yöntemi desteklemediğini unutmayın. Bu yöntemi çağırmak, bir `System.NotSupportedException` oluşturulmasına neden olur.

#### <a name="the-movenext-method"></a>MoveNext yöntemi

Bir Numaralandırıcı nesnesinin yöntemi bir yineleyici bloğunun kodunu kapsüller. `MoveNext` Yöntemi çağırmak Yineleyici bloğunda kodu yürütür ve Numaralandırıcı nesnesinin `Current` özelliğini uygun şekilde ayarlar. `MoveNext` Tarafından `MoveNext` gerçekleştirilen kesin eylem, çağrıldığında Numaralandırıcı `MoveNext` nesnesinin durumuna bağlıdır:

*  Numaralandırıcı nesnesinin durumu daha ***önce***ise, şunu çağırır `MoveNext`:
   * Durumu ***çalışıyor***olarak değiştirir.
   * Yineleyici bloğunun, Numaralandırıcı nesnesi `this`başlatıldığında kaydedilen bağımsız değişken değerlerine ve örnek değerine sahip parametrelerini (dahil) başlatır.
   * , Yürütme kesintiye gelinceye kadar Yineleyici bloğunu baştan yürütür (aşağıda açıklandığı gibi).
*  Numaralandırıcı nesnesinin durumu ***çalışıyorsa***, çağırma `MoveNext` sonucu belirtilmemiş olur.
*  Numaralandırıcı nesnesinin durumu ***askıya alınırsa***, çağırma `MoveNext`:
   * Durumu ***çalışıyor***olarak değiştirir.
   * Tüm yerel değişkenlerin ve parametrelerin (Bu dahil) değerlerini, yineleyici bloğunun yürütülmesi son askıya alındığında kaydedilen değerlere geri yükler. Bu değişkenlerin başvurduğu nesnelerin içeriklerinin, önceki MoveNext çağrısından bu yana değişmiş olabileceğini unutmayın.
   * Yürütmenin askıya alınmasına neden olan `yield return` deyimin hemen sonrasında Yineleyici bloğunun yürütülmesini sürdürür ve yürütme kesintiye uğratılıncaya kadar devam eder (aşağıda açıklandığı gibi).
*  Numaralandırıcı nesnesinin durumu ***After***ise, dönüşler `MoveNext` `false`çağırma.


Yineleyici `MoveNext` bloğunu yürüttüğünde, yürütme dört şekilde kesintiye uğrar: `yield return` Bir`yield break` ifadeye göre, yineleyici bloğunun sonuyla karşılaşarak ve bir özel durum oluşan ve yineleyici bloğunun dışına yayılarak.

*  Bir `yield return` ifadeye rastlandı ([yield bildirisi](statements.md#the-yield-statement)):
   * Deyimde verilen ifade değerlendirilir, örtülü olarak yield türüne dönüştürülür ve Numaralandırıcı nesnesinin `Current` özelliğine atanır.
   * Yineleyici gövdesinin yürütülmesi askıya alındı. Tüm yerel değişkenlerin ve parametrelerin (dahil `this`) değerleri, bu `yield return` deyimin konumu olduğu gibi kaydedilir. İfade bir veya daha fazla `try` blok içindeyse, ilişkili `finally` bloklar Şu anda yürütülmez. `yield return`
   * Numaralandırıcı nesnesinin durumu ***askıya alındı***olarak değiştirildi.
   * Yöntemi, yinelemesinin bir sonraki değere başarıyla ilerlemiş olduğunu belirten, çağırana döner `true`. `MoveNext`
*  Bir `yield break` ifadeye rastlandı ([yield bildirisi](statements.md#the-yield-statement)):
   * Deyimler bir veya daha fazla `try` blok içindeyse, ilişkili `finally` bloklar yürütülür. `yield break`
   * Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.
   * Yöntemi, yinelemesinin tamamlandığını belirten, çağırana döner `false`. `MoveNext`
*  Yineleyici gövdesinin sonuna rastlandı:
   * Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.
   * Yöntemi, yinelemesinin tamamlandığını belirten, çağırana döner `false`. `MoveNext`
*  Bir özel durum oluştuğunda ve yineleyici bloğundan yayıldığında:
   * Yineleyici `finally` gövdesinde uygun bloklar özel durum yayma tarafından yürütülür.
   * Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.
   * Özel durum yayma `MoveNext` yöntemi çağırana devam eder.

#### <a name="the-current-property"></a>Geçerli özellik

Numaralandırıcı nesnesinin `Current` özelliği Yineleyici bloğundaki `yield return` deyimlerden etkilenir.

Bir Numaralandırıcı nesnesi ***askıya alınma*** durumundayken değeri `Current` , önceki çağrısı `MoveNext`tarafından ayarlanan değerdir. Bir Numaralandırıcı nesnesi ***önceki***, ***çalışan***veya ***sonrasında*** olduğunda, erişim `Current` sonucu belirtilmemiş olur.

Dışında bir yield türü `object`olan bir yineleyici için, Numaralandırıcı `IEnumerable` nesnesinin uygulamasına erişmenin `Current` sonucu, Numaralandırıcı nesnesinin `IEnumerator<T>` uygulamasına erişim için `Current` karşılık gelir uygulama ve sonucu olarak `object`atama.

#### <a name="the-dispose-method"></a>Dispose yöntemi

Yöntemi, Numaralandırıcı nesnesini After durumuna getirerek yinelemeyi temizlemek için kullanılır. `Dispose`

*  Numaralandırıcı nesnesinin durumu daha ***önce***ise, çağırma `Dispose` durumunu daha ***sonra***olarak değiştirir.
*  Numaralandırıcı nesnesinin durumu ***çalışıyorsa***, çağırma `Dispose` sonucu belirtilmemiş olur.
*  Numaralandırıcı nesnesinin durumu ***askıya alınırsa***, çağırma `Dispose`:
   * Durumu ***çalışıyor***olarak değiştirir.
   * Son yürütülen `yield return` deyimin bir `yield break` ifademiş olduğu sürece herhangi bir finally bloğunu yürütür. Bu durum, yineleyici gövdesinden oluşan bir özel durumun oluşturulmasına ve yayılmasına neden olursa, Numaralandırıcı nesnesinin durumu ***After*** olarak ayarlanır ve özel durum `Dispose` Yöntem çağıranına yayılır.
   * Durumu ***sonraki***olarak değiştirir.
*  Numaralandırıcı nesnesinin durumu ***After***ise, çağırma `Dispose` hiçbir etkisi olmaz.

### <a name="enumerable-objects"></a>Sıralanabilir nesneler

Sıralanabilir bir arabirim türü döndüren bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesini çağırmak kodu Yineleyici bloğunda hemen yürütmez. Bunun yerine, ***sıralanabilir bir nesne*** oluşturulur ve döndürülür. Sıralanabilir nesne `GetEnumerator` yöntemi, yineleyici bloğunda belirtilen kodu kapsülleyen bir Numaralandırıcı nesnesi döndürür ve Numaralandırıcı `MoveNext` nesnesinin yöntemi çağrıldığında Yineleyici bloğunda kodun yürütülmesi oluşur. Sıralanabilir bir nesne aşağıdaki özelliklere sahiptir:

*  , Ve `IEnumerable` `IEnumerable<T>`' nin uyguladığı `T` , yineleyicinin yield türüdür.
*  Bağımsız değişken değerlerinin bir kopyasıyla başlatılır (varsa) ve örnek değeri işlev üyesine geçirilir.

Sıralanabilir bir nesne genellikle Yineleyici bloğunda kodu kapsülleyen ve numaralandırılabilir arabirimleri uygulayan, derleyici tarafından oluşturulan bir sıralanabilir sınıfın örneğidir, ancak diğer uygulama yöntemleri mümkündür. Bir sıralanabilir sınıf derleyici tarafından oluşturulduysa, bu sınıf doğrudan veya dolaylı olarak işlev üyesini içeren sınıfta iç içe gelir, özel erişilebilirliği olur ve derleyici kullanımı ([tanımlayıcılar](lexical-structure.md#identifiers)) için ayrılmış bir ada sahip olur.

Sıralanabilir bir nesne, yukarıda belirtilenden daha fazla arabirim uygulayabilir. Özellikle de sıralanabilir bir nesne de uygulayabilir `IEnumerator` `IEnumerator<T>`ve bu, hem numaralandırılabilir hem de numaralandırıcı olarak işlev görmesi sağlar. Bu tür bir uygulamada, bir sıralanabilir nesnenin `GetEnumerator` yöntemi ilk kez çağrıldığında, sıralanabilir nesnenin kendisi döndürülür. Sıralanabilir nesnenin `GetEnumerator`sonraki etkinleştirmeleri, varsa, sıralanabilir nesnenin bir kopyasını döndürür. Bu nedenle, döndürülen her Numaralandırıcı kendi durumuna sahiptir ve bir Numaralandırıcı içindeki değişiklikler başka bir Numaralandırıcı olur.

#### <a name="the-getenumerator-method"></a>GetEnumerator yöntemi

Sıralanabilir bir nesne, `GetEnumerator` `IEnumerable` ve `IEnumerable<T>` arabirimlerinin yöntemlerinin bir uygulamasını sağlar. İki `GetEnumerator` Yöntem, kullanılabilir bir Numaralandırıcı nesnesi elde eden ve döndüren ortak bir uygulamayı paylaşır. Numaralandırıcı nesnesi, sıralanabilir nesne başlatıldığında bağımsız değişken değerleri ve örnek değeri ile başlatılır, aksi takdirde Numaralandırıcı nesnesi, [Numaralandırıcı nesnelerinde](classes.md#enumerator-objects)açıklanan şekilde çalışır.

### <a name="implementation-example"></a>Uygulama örneği

Bu bölümde, yineleyicilerin standart C# yapılar açısından olası bir uygulanması açıklanmaktadır. Burada açıklanan uygulama, Microsoft C# derleyicisi tarafından kullanılan ilkelere dayanır, ancak bu, bir uygulanan uygulamasına veya mümkün olan tek bir uygulama anlamına gelir.

Aşağıdaki `Stack<T>` sınıf `GetEnumerator` yöntemini bir yineleyici kullanarak uygular. Yineleyici, yığının öğelerini üstten alta doğru sıralamayla numaralandırır.

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

`GetEnumerator` Yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyicinin ürettiği bir Numaralandırıcı sınıfının bir örneklemesine çevrilebilir.

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

Yukarıdaki çeviride, yineleyici bloğundaki kod bir durum makinesine açılıp Numaralandırıcı sınıfının `MoveNext` yöntemine yerleştirilir. Ayrıca yerel değişken `i` , Numaralandırıcı nesnesindeki bir alana açıktır, bu sayede ' ın `MoveNext`çağırmaları arasında olmaya devam edebilir.

Aşağıdaki örnekte, 1 ile 10 arasındaki tamsayıların basit bir çarpma tablosu yazdırılır. Örnekteki `FromTo` Yöntem, sıralanabilir bir nesne döndürüyor ve yineleyici kullanılarak uygulanır.

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

`FromTo` Yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyici tarafından oluşturulmuş bir sıralanabilir sınıfın bir örneklemesine çevrilebilir.

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

Sıralanabilir sınıf hem numaralandırılabilir arabirimleri hem de Numaralandırıcı arabirimlerini uygular, bu sayede hem numaralandırılabilir hem de numaralandırıcı olarak işlev sağlar. `GetEnumerator` Yöntemi ilk kez çağrıldığında, sıralanabilir nesnenin kendisi döndürülür. Sıralanabilir nesnenin `GetEnumerator`sonraki etkinleştirmeleri, varsa, sıralanabilir nesnenin bir kopyasını döndürür. Bu nedenle, döndürülen her Numaralandırıcı kendi durumuna sahiptir ve bir Numaralandırıcı içindeki değişiklikler başka bir Numaralandırıcı olur. Yöntemi `Interlocked.CompareExchange` , iş parçacığı güvenli işlemi sağlamak için kullanılır.

`from` Ve`to` parametreleri, sıralanabilir sınıftaki alanlara açıktır. Yineleyici bloğunda değiştirildiği için, her Numaralandırıcı için `from` verilen ilk `__from` değeri tutmak üzere ek bir alan sunulmuştur. `from`

Yöntemi, `InvalidOperationException` olduğundaçağrılırsa`__state`biroluşturur. `0` `MoveNext` Bu, numaralandırılabilir nesnenin önce çağrılmadan `GetEnumerator`bir Numaralandırıcı nesnesi olarak kullanılmasını önler.

Aşağıdaki örnek bir basit ağaç sınıfını göstermektedir. Sınıfı `Tree<T>` , bir yineleyici `GetEnumerator` kullanarak metodunu uygular. Yineleyici, ağaç öğelerini geçersiz kılma sırasında sıralar.

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

`GetEnumerator` Yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyicinin ürettiği bir Numaralandırıcı sınıfının bir örneklemesine çevrilebilir.

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

`foreach` Deyimlerde kullanılan geçiciler oluşturulan derleyici, Numaralandırıcı nesnesinin `__left` ve `__right` alanlarını yükseltilmemiş. Bir özel durum oluşturulursa doğru `Dispose()` yöntem doğru şekilde çağrılabilmesi için, Numaralandırıcı nesnesinin alanıdikkatlegüncellenir.`__state` Çevrilmiş kodu basit `foreach` deyimlerle yazmak mümkün değildir.

## <a name="async-functions"></a>Zaman uyumsuz işlevler

Değiştirici içeren bir yönteme ([yöntemlere](classes.md#methods)) veya anonim işleve ([anonim işlev ifadelerine](expressions.md#anonymous-function-expressions)) zaman uyumsuz işlev denir. `async` Genel olarak, ***zaman uyumsuz*** terimi, `async` değiştiriciye sahip herhangi bir tür işlevi tanımlamakta kullanılır.

Herhangi `ref` bir veya `out` parametresi belirtmek için zaman uyumsuz bir işlevin biçimsel parametre listesi için derleme zamanı hatası.

Zaman uyumsuz bir metodun SID 'si ya da `void` bir ***görev türü***olmalıdır. Görev türleri ve ' `System.Threading.Tasks.Task` den `System.Threading.Tasks.Task<T>`oluşturulan türlerdir. Breçekimi 'nin sake 'ı için bu bölümde sırasıyla ve `Task` `Task<T>`olarak başvurulur. Bir görev türü döndüren zaman uyumsuz bir yöntem, görev döndüren olarak kabul edilir.

Görev türlerinin tam tanımı uygulama tanımlı, ancak dilin bir görev türü görüntüleme noktasından tamamlanmamış, başarılı veya hatalı durumlardan birinde. Hatalı bir görev ilgili özel durumu kaydeder. Başarılı `Task<T>` bir tür `T`sonucunu kaydeder. Görev türleri beklenebilir ve bu nedenle await ifadelerinin işlenenleri ([await ifadeleri](expressions.md#await-expressions)) olabilir.

Zaman uyumsuz işlev çağırma, kendi gövdesinde await ifadeleri ([await ifadeleri](expressions.md#await-expressions)) yoluyla değerlendirmeyi askıya alabilir. Değerlendirme daha sonra, bir ***sürdürme temsilcisi***tarafından askıya alma await ifadesinin noktasında devam edebilir. Sürdürme temsilcisi türündedir `System.Action`ve çağrıldığında, zaman uyumsuz işlev çağrısının değerlendirmesi, kaldığı Await ifadesinden sürdürülecek. Zaman uyumsuz işlev çağrısının ***geçerli çağıranı*** , işlev çağrısı hiçbir zaman askıya alınmadıysa veya sürdürme temsilcisinin en son çağıranı yoksa, özgün çağırıcı olur.

### <a name="evaluation-of-a-task-returning-async-function"></a>Bir görev döndüren zaman uyumsuz işlev değerlendirmesi

Bir görev döndüren zaman uyumsuz işlevin çağrılması döndürülen görev türünün bir örneğinin oluşturulmasına neden olur. Bu, Async işlevinin ***Return görevi*** olarak adlandırılır. Görev başlangıçta tamamlanmamış bir durumda.

Zaman uyumsuz işlev gövdesi, askıya alınana kadar (bir await ifadesine ulaşılarak) ya da sona erene kadar değerlendirilir, bu da dönüş göreviyle birlikte çağrı yapana döndürülür.

Async işlevinin gövdesi sonlandırıldığında, geri dönüş görevi tamamlanmamış durumdan çıkarılır:

*  İşlev gövdesi bir dönüş ifadesine veya gövdenin sonuna ulaşılması sonucu olarak sonlandığında, herhangi bir sonuç değeri, başarılı durumuna yerleştirilen dönüş görevine kaydedilir.
*  İşlev gövdesi yakalanamayan bir özel durum ([throw](statements.md#the-throw-statement)) sonucu olarak sonlandığında, özel durum hatalı duruma yerleştirilen geri dönüş görevine kaydedilir.

### <a name="evaluation-of-a-void-returning-async-function"></a>Void döndüren zaman uyumsuz bir işlevin değerlendirmesi

Async işlevinin dönüş türü ise `void`, değerlendirme yukarıda aşağıdaki şekilde farklılık gösterir: Hiçbir görev döndürülmediği için, işlev geçerli iş parçacığının ***eşitleme bağlamına***tamamlanma ve özel durumları iletişim kurar. Eşitleme bağlamının tam tanımı uygulamaya bağımlıdır, ancak geçerli iş parçacığının çalıştırıldığı "nerede" gösterimidir. Bir void döndüren zaman uyumsuz işlevin değerlendirilmesi, başarıyla tamamlandığında veya yakalanamayan bir özel durumun oluşturulmasına neden olduğunda eşitleme bağlamına bildirim gönderilir.

Bu, bağlamın altında kaç tane void döndüren zaman uyumsuz işlev çalıştığını izlemeye ve bunlardan gelen özel durumların nasıl yayabileceğine karar vermesine olanak tanır.
