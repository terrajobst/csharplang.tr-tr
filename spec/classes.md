---
ms.openlocfilehash: e0def754174ab8646f9b849abe86d2c375c958b6
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703978"
---
# <a name="classes"></a>Sınıflar

Sınıf veri üyeleri (sabitler ve alanlar), işlev üyeleri (Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve iç içe türler içerebilen bir veri yapısıdır. Sınıf türleri, türetilmiş bir sınıfın bir temel sınıfı genişletebileceği ve özelleştirileceği bir mekanizma olan devralmayı destekler.

## <a name="class-declarations"></a>Sınıf bildirimleri

*Class_declaration* , yeni bir sınıf bildiren bir *type_declaration* ([tür bildirimleri](namespaces.md#type-declarations)).

```antlr
class_declaration
    : attributes? class_modifier* 'partial'? 'class' identifier type_parameter_list?
      class_base? type_parameter_constraints_clause* class_body ';'?
    ;
```

*Class_declaration* , isteğe bağlı bir *öznitelik* kümesi ([öznitelikler](attributes.md)) ve ardından isteğe bağlı bir *class_modifier*s kümesi ([sınıf değiştiricileri](classes.md#class-modifiers)) ve ardından isteğe bağlı bir `partial` `class` değiştiricisi ve sonra isteğe bağlı bir type_parameter_list belirtimi ( [tür parametreleri](classes.md#type-parameters)) ve ardından isteğe *bağlı bir* *class_base* belirtimi ([sınıf taban belirtimi](classes.md#class-base-specification)) ve ardından isteğe bağlı bir *type_parameter_constraints_clause*s kümesi ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) tarafından, ardından bir *class_body* ([sınıf gövdesi](classes.md#class-body)) ve daha sonra noktalı virgül gelir.

Bir sınıf bildirimi, ayrıca bir *type_parameter_list*sağlamadıkça *type_parameter_constraints_clause*s 'yi sağlayaamaz.

Bir *type_parameter_list* sağlayan bir sınıf bildirimi, ***Genel sınıf bildirimidir***. Ayrıca, bir genel sınıf bildiriminde iç içe yerleştirilmiş olan herhangi bir sınıf ya da genel bir yapı bildirimi, oluşturulmuş bir tür oluşturmak için, kapsayan türün tür parametreleri sağlanmalıdır.

### <a name="class-modifiers"></a>Sınıf değiştiricileri

*Class_declaration* , isteğe bağlı olarak bir sınıf değiştiricileri dizisi içerebilir:

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

İç içe sınıflarda `new` değiştiriciye izin verilir. Bu, sınıfın, [Yeni değiştirici](classes.md#the-new-modifier)bölümünde açıklandığı gibi, devralınan bir üyeyi aynı adla gizlediğini belirtir. `new` değiştiricinin iç içe geçmiş sınıf bildirimi olmayan bir sınıf bildiriminde görünmesi için derleme zamanı hatası.

`public`, `protected`, `internal`ve `private` değiştiricileri, sınıfının erişilebilirliğini denetler. Sınıf bildiriminin gerçekleştiği içeriğe bağlı olarak, bu değiştiricilerin bazılarına izin verilmeyebilir ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)).

`abstract`, `sealed` ve `static` değiştiricileri aşağıdaki bölümlerde ele alınmıştır.

#### <a name="abstract-classes"></a>Soyut sınıflar

`abstract` değiştiricisi, bir sınıfın tamamlanmamış olduğunu ve yalnızca temel sınıf olarak kullanılması amaçlanan olduğunu göstermek için kullanılır. Soyut bir sınıf, soyut olmayan bir sınıftan aşağıdaki yollarla farklılık gösterir:

*  Soyut bir sınıf doğrudan başlatılamaz ve bir derleme zamanı hatası, bir soyut sınıf üzerinde `new` işlecini kullanmaktır. Derleme zamanı türleri soyut olan değişkenler ve değerler olması mümkün olsa da, bu tür değişkenler ve değerler `null` veya soyut türlerden türetilmiş soyut olmayan sınıfların örneklerine başvurular içermelidir.
*  Soyut bir sınıfa, soyut Üyeler içermesi için izin verilir (ancak gerekli değildir).
*  Soyut bir sınıf korumalı olamaz.

Soyut olmayan bir sınıf, soyut bir sınıftan türetilmişse, soyut olmayan sınıfın devralınan tüm soyut üyelerin gerçek uygulamalarını içermesi gerekir, böylece bu soyut üyeleri geçersiz kılar. örnekte
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
Soyut sınıf `A` `F`soyut bir yöntem sunar. Sınıf `B` ek bir yöntem `G`tanıtır, ancak `F`uygulanmasını sağlamadıklarından `B` de soyut olarak bildirilmelidir. Sınıf `C` geçersiz kılar `F` ve gerçek bir uygulama sağlar. `C`içinde soyut üye olmadığından, `C` izin verilir (ancak zorunlu değildir).

#### <a name="sealed-classes"></a>Mühürlü sınıflar

`sealed` değiştiricisi bir sınıftan türetmeyi engellemek için kullanılır. Bir Sealed sınıfı, başka bir sınıfın temel sınıfı olarak belirtilmişse, derleme zamanı hatası oluşur.

Sealed bir sınıf de soyut bir sınıf olamaz.

`sealed` değiştiricisi öncelikle istenmeden Türetmenin önlenmesi için kullanılır, ancak belirli çalışma zamanı iyileştirmelerini de sağlar. Özellikle, korumalı bir sınıfın hiç türetilmiş sınıfa sahip olmadığı bilindiğinden, korumalı sınıf örneklerine sanal işlev üye çağırmaları sanal olmayan çağırmaları dönüştürmek mümkündür.

#### <a name="static-classes"></a>Statik sınıflar

`static` değiştiricisi, bir ***statik sınıf***olarak bildirildiği sınıfı işaretlemek için kullanılır. Statik bir sınıf örneği oluşturulamıyor, tür olarak kullanılamaz ve yalnızca statik üyeleri içerebilir. Yalnızca bir statik sınıf uzantı yöntemlerinin bildirimlerini içerebilir ([Uzantı yöntemleri](classes.md#extension-methods)).

Statik sınıf bildirimi aşağıdaki kısıtlamalara tabidir:

*  Statik bir sınıf, `sealed` veya `abstract` değiştiricisi içeremez. Ancak, statik bir sınıf tarafından örneklenemez veya türetilmediği için, hem korumalı hem de soyut gibi davranır.
*  Statik bir sınıf *class_base* belirtimi ([sınıf taban belirtimi](classes.md#class-base-specification)) içeremez ve açıkça bir temel sınıf veya uygulanan arabirimlerin listesini belirtemez. Statik bir sınıf örtülü olarak `object`türünden devralır.
*  Statik bir sınıf yalnızca statik Üyeler ([statik ve örnek üyeleri](classes.md#static-and-instance-members)) içerebilir. Sabitler ve iç içe geçmiş türlerin statik üye olarak sınıflandırıldığını unutmayın.
*  Statik bir sınıfta `protected` veya `protected internal` belirtilen erişilebilirliği olan Üyeler olamaz.

Bu kısıtlamaların herhangi birini ihlal etmek için derleme zamanı hatası vardır.

Statik bir sınıfın örnek oluşturucuları yok. Statik bir sınıfta örnek Oluşturucu bildirmek mümkün değildir ve statik bir sınıf için varsayılan örnek Oluşturucu ([Varsayılan oluşturucular](classes.md#default-constructors)) sağlanmaz.

Statik bir sınıfın üyeleri otomatik olarak statik değildir ve üye bildirimlerinin açıkça bir `static` değiştiricisi içermesi gerekir (sabitler ve iç içe türler hariç). Bir sınıf statik bir dış sınıf içinde iç içe olduğunda, açıkça bir `static` değiştiricisi içermiyorsa, iç içe yerleştirilmiş sınıf statik bir sınıf değildir.

__Statik Sınıf türlerine başvurma__

Bir *namespace_or_type_name* ([ad alanı ve tür adları](basic-concepts.md#namespace-and-type-names)), şu durumlarda bir statik sınıfa başvurmak için izin verilir

*  *Namespace_or_type_name* , `T.I`*namespace_or_type_name* `T` veya
*  *Namespace_or_type_name* , `typeof(T)`formun *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) `T`.

Bir *primary_expression* ([işlev üyeleri](expressions.md#function-members)), şu durumlarda bir statik sınıfa başvurmak için izin verilir

*  *Primary_expression* , `E.I`formun *member_access* ([dinamik aşırı yükleme çözümlemesi için derleme zamanı denetimi](expressions.md#compile-time-checking-of-dynamic-overload-resolution)) `E`.

Diğer bir bağlamda, statik bir sınıfa başvurmak için derleme zamanı hatası olur. Örneğin, bir statik sınıfın temel sınıf olarak kullanılabilmesi, bir üyenin bir bileşen türü ([Iç içe türler](classes.md#nested-types)), bir genel tür bağımsız değişkeni veya tür parametresi kısıtlaması olması hatadır. Benzer şekilde, bir statik sınıf dizi türünde, işaretçi türünde, `new` ifadesinde, atama ifadesinde, `is` ifadesinde, bir `as` ifadesinde, `sizeof` ifadesinde veya bir varsayılan değer ifadesinde kullanılamaz.

### <a name="partial-modifier"></a>Kısmi değiştirici

`partial` değiştirici, bu *class_declaration* kısmi bir tür bildirimi olduğunu göstermek için kullanılır. Bir kapsayan ad alanı veya tür bildiriminde aynı ada sahip birden çok kısmi tür bildirimi, [kısmi türlerde](classes.md#partial-types)belirtilen kurallara göre tek bir tür bildirimi oluşturmak için birleştirilir.

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

*Class_base*bir *class_type* dahil edildiğinde, bildirildiği sınıfın doğrudan temel sınıfını belirtir. Bir sınıf bildiriminde *class_base*yoksa veya *class_base* yalnızca arabirim türlerini listelemeli, doğrudan taban sınıfın `object`olduğu varsayılır. Bir sınıf, [Devralma](classes.md#inheritance)bölümünde açıklandığı gibi doğrudan temel sınıfından üyeleri devralır.

örnekte
```csharp
class A {}

class B: A {}
```
sınıf `A`, `B`doğrudan temel sınıfı olarak kabul edilir ve `B` `A`türettiği söylenir. `A` açıkça doğrudan bir temel sınıf belirtmediğinden, doğrudan temel sınıfı örtülü olarak `object`.

Oluşturulmuş bir sınıf türü için, genel sınıf bildiriminde bir temel sınıf belirtilmişse, oluşturulan türün temel sınıfı, temel sınıf bildirimindeki her bir *type_parameter* , oluşturulan türün karşılık gelen *type_argument* yerine koyarak elde edilir. Genel sınıf bildirimleri verildiğinde
```csharp
class B<U,V> {...}

class G<T>: B<string,T[]> {...}
```
oluşturulan tür `G<int>` temel sınıfı `B<string,int[]>`olabilir.

Bir sınıf türünün doğrudan temel sınıfı en az sınıf türünün kendisi ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) olarak erişilebilir olmalıdır. Örneğin, bir `private` veya `internal` sınıfından türetmek `public` sınıfın derleme zamanı hatasıdır.

Bir sınıf türünün doğrudan temel sınıfı şu türlerden biri olmalıdır: `System.Array`, `System.Delegate`, `System.MulticastDelegate`, `System.Enum`veya `System.ValueType`. Ayrıca, genel bir sınıf bildirimi `System.Attribute` doğrudan veya dolaylı temel sınıf olarak kullanamaz.

Bir sınıf `B``A` doğrudan temel sınıf belirtiminin anlamı belirlenirken, `B` doğrudan taban sınıfının geçici olarak `object`olduğu varsayılır. Intuicanlı bu, bir temel sınıf belirtiminin anlamını özyinelemeli olarak kendi kendine bağımlı olmamasını sağlar. Örnek:
```csharp
class A<T> {
   public class B {}
}

class C : A<C.B> {}
```
temel sınıf belirtiminde bu yana hata durumunda `A<C.B>` doğrudan taban `C` sınıfının `object`olduğu kabul edilir ve bu nedenle ( [ad alanı ve tür adlarının](basic-concepts.md#namespace-and-type-names)kuralları tarafından) `C` üye `B`kabul edilmez.

Bir sınıf türünün temel sınıfları doğrudan taban sınıfıdır ve temel sınıflarıdır. Diğer bir deyişle, temel sınıfların kümesi doğrudan temel sınıf ilişkisinin geçişli kapanışı olur. Yukarıdaki örneğe başvurarak, `B` temel sınıfları `A` ve `object`. örnekte
```csharp
class A {...}

class B<T>: A {...}

class C<T>: B<IComparable<T>> {...}

class D<T>: C<T[]> {...}
```
`D<int>` temel sınıfları `C<int[]>`, `B<IComparable<int[]>>`, `A`ve `object`.

Sınıf `object`hariç olmak üzere her sınıf türünün tam olarak bir doğrudan temel sınıfı vardır. `object` sınıfının doğrudan temel sınıfı yoktur ve diğer tüm sınıfların en son temel sınıfıdır.

Bir sınıf `B` bir sınıf `A`türediği zaman, `A` `B`bağımlı olması için derleme zamanı hatası olur. Bir sınıf doğrudan temel sınıfına (varsa) ***bağlıdır*** ve doğrudan iç içe geçmiş (varsa) sınıfa ***bağlıdır*** . Bu tanım verildiğinde, bir sınıfın bağımlı olduğu tüm sınıf kümesi, ***doğrudan ilişkiye bağlı olarak değişir*** .

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
`A`, bir derleme zamanı hatasına neden olur çünkü bu, `A`bağımlı olan `B` (hemen kapsayan sınıfa) bağlı olan `B.C` (doğrudan temel sınıfına) bağlıdır.

Bir sınıfın içinde iç içe yerleştirilmiş sınıflara bağlı olmadığına unutmayın. örnekte
```csharp
class A
{
    class B: A {}
}
```
`B`, `A` bağımlıdır (`A` hem doğrudan temel sınıfı hem de onun hemen kapsayan sınıfı olduğundan), ancak `B` bir temel sınıf veya kapsayan bir sınıf olmadığından `A` bağımlı değildir.`B``A` Bu nedenle, örnek geçerlidir.

`sealed` sınıfından türetmek mümkün değildir. örnekte
```csharp
sealed class A {}

class B: A {}            // Error, cannot derive from a sealed class
```
sınıf `B`, `A``sealed` sınıfından türetmeye çalıştığı için hatalı.

#### <a name="interface-implementations"></a>Arabirim Uygulamaları

Bir *class_base* belirtimi, arabirim türlerinin bir listesini içerebilir ve bu durumda sınıf verilen arabirim türlerini doğrudan uygulayacak şekilde söylenir. Arabirim Uygulamaları, [arabirim uygulamalarında](interfaces.md#interface-implementations)daha ayrıntılı bir şekilde ele alınmıştır.

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

Her *type_parameter_constraints_clause* , belirteç `where`ve ardından bir tür parametresinin adından sonra iki nokta üst üste ve bu tür parametresi için kısıtlamalar listesinden oluşur. Her tür parametresi için en fazla bir `where` yan tümcesi olabilir ve `where` yan tümceleri herhangi bir sırada listelenebilir. Bir özellik erişimcisindeki `get` ve `set` belirteçleri gibi `where` belirteci bir anahtar sözcük değildir.

Bir `where` yan tümcesinde verilen kısıtlamaların listesi, şu bileşenlerden herhangi birini içerebilir: tek bir birincil kısıtlama, bir veya daha fazla ikincil kısıtlama ve Oluşturucu kısıtlaması, `new()`.

Birincil kısıtlama bir sınıf türü veya ***başvuru türü kısıtlaması*** `class` veya ***değer türü kısıtlaması*** `struct`olabilir. İkincil kısıtlama bir *type_parameter* veya *interface_type*olabilir.

Başvuru türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeninin bir başvuru türü olması gerektiğini belirtir. Tüm sınıf türleri, arabirim türleri, temsilci türleri, dizi türleri ve başvuru türü olarak bilinen tür parametreleri (aşağıda tanımlandığı gibi), bu kısıtlamayı karşılar.

Değer türü kısıtlaması, tür parametresi için kullanılan bir tür bağımsız değişkeninin null yapılamayan bir değer türü olması gerektiğini belirtir. Bu kısıtlamayı karşılayan, null olamayan tüm yapı türleri, sabit listesi türleri ve değer türü kısıtlamasına sahip tür parametreleri. Değer türü olarak sınıflandırıldığından, null yapılabilir bir tür ([null yapılabilir türler](types.md#nullable-types)) değer türü kısıtlamasını karşılamadığına unutmayın. Değer türü kısıtlamasına sahip bir tür parametresi de *constructor_constraint*sahip olamaz.

İşaretçi türlerinin hiçbir şekilde tür bağımsız değişkeni olmasına izin verilmez ve başvuru türü veya değer türü kısıtlamalarını karşılamak için düşünülmez.

Bir kısıtlama bir sınıf türü, bir arabirim türü veya bir tür parametresi ise, bu tür parametresi için kullanılan her tür bağımsız değişkenin desteklemesi gereken en az "temel türü" değeri belirtir. Oluşturulmuş bir tür veya genel yöntem kullanıldığında, tür bağımsız değişkeni derleme zamanında tür parametresindeki kısıtlamalara karşı denetlenir. Sağlanan tür bağımsız değişkeni, [kısıtlamaları](types.md#satisfying-constraints)karşıladığı koşullarda açıklanan koşullara uymalıdır.

*Class_type* kısıtlamasının aşağıdaki kuralları karşılaması gerekir:

*  Tür bir sınıf türü olmalıdır.
*  Tür `sealed`olmamalıdır.
*  Tür şu türlerden biri olmalıdır: `System.Array`, `System.Delegate`, `System.Enum`veya `System.ValueType`.
*  Tür `object`olmamalıdır. Tüm türler `object`türetildiğinden, bu tür bir kısıtlama izin verildiğinde hiçbir etkiye sahip olmaz.
*  Verilen tür parametresi için en fazla bir kısıtlama bir sınıf türü olabilir.

*İnterface_type* kısıtlaması olarak belirtilen bir türün aşağıdaki kuralları karşılaması gerekir:

*  Tür bir arabirim türü olmalıdır.
*  Bir tür, belirli bir `where` yan tümcesinde birden çok kez belirtilmemelidir.

Her iki durumda da kısıtlama, oluşturulmuş bir türün parçası olarak ilişkili tür veya yöntem bildiriminin herhangi bir tür parametresini içerebilir ve bu tür, bildirildiği türü içerebilir.

Tür parametresi kısıtlaması olarak belirtilen herhangi bir sınıf veya arabirim türünün, bildirildiği genel tür veya yöntem olarak en az erişilebilir ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olması gerekir.

*Type_parameter* kısıtlaması olarak belirtilen bir türün aşağıdaki kuralları karşılaması gerekir:

*  Tür bir tür parametresi olmalıdır.
*  Bir tür, belirli bir `where` yan tümcesinde birden çok kez belirtilmemelidir.

Ayrıca, bağımlılık grafiğinde tür parametrelerinin bağımlılık grafiğinde bir döngü olmaması gerekir; burada bağımlılık, tarafından tanımlanan geçişli bir ilişki olur:

*  Tür parametresi `T` tür parametresi için bir kısıtlama olarak kullanılırsa `S` `S` `T`***bağlıdır*** .
*  Bir tür parametresi `S` bir tür parametresine bağlıdır `T` ve `T` bir tür parametresine bağlıdır `U` `S` ***bağlıdır*** .`U`

Bu ilişki verildiğinde, bir tür parametresinin kendisine (doğrudan veya dolaylı olarak) bağımlı olması için derleme zamanı hatası vardır.

Herhangi bir kısıtlama bağımlı tür parametreleri arasında tutarlı olmalıdır. Tür parametresi `S` `T` tür parametresine bağımlıysa:

*  `T` değer türü kısıtlamasına sahip olmamalıdır. Aksi takdirde, `T`, `T`ile aynı tür olmaya zorlanacak ve iki tür parametresi gereksinimini ortadan kaldıran `S`.
*  `S` değer türü kısıtlaması varsa `T` *class_type* kısıtlamasına sahip olmamalıdır.
*  `S` bir *class_type* kısıtlaması `A` ve `T` *class_type kısıtlaması varsa `B` bir kimlik* dönüştürmesi ya da `A` 'den `B` örtük bir başvuru dönüştürmesi olması gerekir.`B``A`
*  `S` ayrıca tür parametresine `U` ve `U` bir *class_type* kısıtlaması `A` ve `T` class_type kısıtlaması varsa `B` `A` bir *`B` kısıtlamasına ya* da `B` 'den `A`bir örtük başvuru dönüştürmesi olmalıdır.

`S` değer türü kısıtlamasına sahip olması ve `T` başvuru türü kısıtlamasına sahip olması için geçerlidir. Bu, `T` `System.Object`, `System.ValueType`, `System.Enum`ve herhangi bir arabirim türüne göre sınırlar.

Bir tür parametresi için `where` yan tümcesi bir Oluşturucu kısıtlaması içeriyorsa (Bu biçimde `new()`), türü örnekleri oluşturmak için `new` işlecini kullanmak mümkündür ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)). Oluşturucu kısıtlaması olan bir tür parametresi için kullanılan herhangi bir tür bağımsız değişkeni Ortak parametresiz bir oluşturucuya sahip olmalıdır (Bu Oluşturucu herhangi bir değer türü için örtülü olarak bulunur) veya değer türü kısıtlaması veya Oluşturucu kısıtlamasına sahip bir tür parametresi olmalıdır (Ayrıntılar için bkz. [tür parametresi kısıtlamaları](classes.md#type-parameter-constraints) ).

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

`T` bir tür parametresinin ***etkin temel sınıfı*** aşağıdaki gibi tanımlanır:

*  `T` birincil kısıtlamalara veya tür parametresi kısıtlamalarına sahip değilse, etkin taban sınıfı `object`.
*  `T` değer türü kısıtlaması varsa, etkin taban sınıfı `System.ValueType`.
*  `T` bir *class_type* kısıtlaması `C` ancak *type_parameter* kısıtlaması yoksa, etkin taban sınıfı `C`.
*  `T` *class_type* kısıtlaması yoksa, ancak bir veya daha fazla *type_parameter* kısıtlaması varsa, kendi etkin taban sınıfı, *type_parameter* kısıtlamalarının etkin temel sınıfları kümesinde en çok kullanılan türdür ([yükseltilmemiş dönüştürme işleçleri](conversions.md#lifted-conversion-operators)). Tutarlılık kuralları, bu tür bir en kapsamlı türün mevcut olmasını güvence altına alıyor.
*  `T` hem *class_type* kısıtlaması hem de bir veya daha fazla *type_parameter* kısıtlaması varsa, etkin taban sınıfı, `T` *class_type* kısıtlamasından ve *type_parameter* kısıtlamalarının etkin taban sınıflarına sahip olan kümesinde en kapsamlı tür ([yükseltilmemiş dönüştürme işleçleri](conversions.md#lifted-conversion-operators)). Tutarlılık kuralları, bu tür bir en kapsamlı türün mevcut olmasını güvence altına alıyor.
*  `T` başvuru türü kısıtlamasına sahipse ancak *class_type* kısıtlaması yoksa, etkin taban sınıfı `object`.

Bu kuralların amacına yönelik olarak, T, bir *value_type*olan bir kısıtlama `V`, bunun yerine *class_type*olan en belirgin `V` taban türünü kullanın. Bu, açıkça verilen kısıtlamada asla gerçekleşmeyebilir, ancak genel bir yöntemin kısıtlamaları geçersiz kılan bir yöntem bildirimi veya bir arabirim yönteminin açık bir uygulamasıyla dolaylı olarak devralındığında gerçekleşebilir.

Bu kurallar, etkin temel sınıfın her zaman bir *class_type*olduğundan emin olur.

Bir tür parametresinin ***etkin arabirim kümesi*** `T` aşağıdaki gibi tanımlanır:

*  `T` *secondary_constraints*yoksa, etkin arabirimi kümesi boş olur.
*  `T` *interface_type* kısıtlamaları varsa ancak *type_parameter* kısıtlama yoksa, etkin arabirimi kümesi *interface_type* kısıtlamaları kümesidir.
*  `T` *interface_type* kısıtlaması yoksa ancak *type_parameter* kısıtlamalarına sahipse, etkin arabirim kümesi *type_parameter* kısıtlamalarının etkin arabirim kümelerinin birleşimidir.
*  `T` hem *interface_type* kısıtlamalarına hem de *type_parameter* kısıtlamalarına sahipse, etkin arabirim kümesi *interface_type* kısıtlamaları kümesinin ve *type_parameter* kısıtlamalarının etkin arabirim kümelerinin birleşimidir.

Bir tür parametresinin başvuru türü kısıtlaması varsa veya etkin taban sınıfı `object` veya `System.ValueType`değilse, ***başvuru türü olarak bilinir*** .

Kısıtlanmış tür parametre türünün değerleri, kısıtlamalar tarafından kapsanan örnek üyelerine erişmek için kullanılabilir. örnekte
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
`T` her zaman `IPrintable`uygulamak üzere kısıtlandığından, `IPrintable` yöntemleri doğrudan `x` üzerinde çağrılabilir.

### <a name="class-body"></a>Sınıf gövdesi

Bir sınıfın *class_body* , bu sınıfın üyelerini tanımlar.

```antlr
class_body
    : '{' class_member_declaration* '}'
    ;
```

## <a name="partial-types"></a>Kısmi türler

Bir tür bildirimi, birden çok ***Kısmi tür***bildirimine bölünebilir. Tür bildirimi, bu bölümdeki kurallara göre, programın derleme zamanı ve çalışma zamanı işlemenin geri kalanı sırasında tek bir bildirim olarak değerlendirilmesinin ardından parçalarından oluşturulur.

Bir *class_declaration*, *struct_declaration* veya *interface_declaration* , bir `partial` değiştiricisi içeriyorsa kısmi tür bildirimini temsil eder. `partial` bir anahtar sözcük değildir ve yalnızca bir değiştirici olarak, bir tür bildiriminde, `struct` veya `interface` ya da bir yöntem bildiriminde `void` bir şekilde `class`görünür. Diğer bağlamlarda, normal tanımlayıcı olarak kullanılabilir.

Kısmi tür bildiriminin her bir bölümü `partial` değiştirici içermelidir. Aynı ada sahip olmalıdır ve diğer bölümlerle aynı ad alanında veya tür bildiriminde bildirilmelidir. `partial` değiştiricisi, tür bildiriminin ek bölümlerinin başka bir yerde mevcut olabileceğini gösterir, ancak bu tür ek parçaların varlığı bir gereksinim değildir; `partial` değiştiricisini içermesi için tek bir bildirime sahip bir tür için geçerlidir.

Kısmi bir türün tüm bölümlerinin, parçaların derleme zamanında tek bir tür bildirimine birleştirilebilmesi için birlikte derlenmesi gerekir. Kısmi türler özellikle derlenmiş türlerin genişletilmesini izin vermez.

İç içe türler `partial` değiştiricisi kullanılarak birden çok bölümde bildirilemez. Genellikle, kapsayan tür `partial` kullanılarak ve iç içe türün her bölümü, kapsayan türün farklı bir bölümünde tanımlanır.

Temsilci veya Enum bildirimlerinde `partial` değiştiriciye izin verilmez.

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

Kısmi bir tür bildirimi bir erişilebilirlik belirtimi (`public`, `protected`, `internal`ve `private` değiştiricileri) içerdiğinde, bir erişilebilirlik belirtimi içeren diğer tüm bölümleri kabul etmelidir. Kısmi bir türün hiçbir bölümü bir erişilebilirlik belirtimi içeriyorsa, türe uygun varsayılan erişilebilirlik (belirtilen[Erişilebilirlik](basic-concepts.md#declared-accessibility)) verilir.

İç içe türün bir veya daha fazla kısmi bildirimi `new` değiştirici içeriyorsa, iç içe geçmiş türü devralınmış bir üyeyi ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) gizliyorsanız hiçbir uyarı bildirilmez.

Bir sınıfın bir veya daha fazla kısmi bildirimi `abstract` değiştirici içeriyorsa, sınıf soyut ([soyut sınıflar](classes.md#abstract-classes)) olarak değerlendirilir. Aksi takdirde, sınıfı Özet olmayan olarak kabul edilir.

Bir sınıfın bir veya daha fazla kısmi bildirimi `sealed` değiştirici içeriyorsa, sınıf sealed ([Sealed sınıflar](classes.md#sealed-classes)) olarak kabul edilir. Aksi takdirde, sınıfı korumasız kabul edilir.

Bir sınıfın hem abstract hem de Sealed olamayacağını unutmayın.

`unsafe` değiştiricisi kısmi bir tür bildiriminde kullanıldığında, yalnızca bu belirli parça güvenli olmayan bir bağlam ([güvenli olmayan bağlamlar](unsafe-code.md#unsafe-contexts)) olarak kabul edilir.

### <a name="type-parameters-and-constraints"></a>Tür parametreleri ve kısıtlamaları

Genel bir tür birden çok bölümde bildirilirse, her parçanın tür parametrelerini durumu olması gerekir. Her parçanın aynı sayıda tür parametreleri ve her tür parametresi için aynı adı sırasıyla aynı olmalıdır.

Kısmi genel tür bildiriminde kısıtlamalar (`where` yan tümceleri) varsa, kısıtlamalar, kısıtlamaları içeren diğer tüm bölümleri kabul etmelidir. Özellikle, kısıtlamaları içeren her parçanın aynı tür parametreleri kümesi için kısıtlamaları olmalıdır ve her tür parametresi için, birincil, ikincil ve Oluşturucu kısıtlamalarının kümelerinin eşdeğeri olması gerekir. İki kısıtlama kümesi aynı üyeleri içeriyorsa eşdeğerdir. Kısmi bir genel türün hiçbir bölümü tür parametresi kısıtlamalarını belirtiyorsa tür parametreleri Kısıtlanmamış olarak kabul edilir.

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

Kısmi bir sınıf bildirimi bir temel sınıf belirtimi içerdiğinde, temel sınıf belirtimi içeren diğer tüm bölümleri kabul etmelidir. Kısmi bir sınıfın hiçbir bölümü bir temel sınıf belirtimi içeriyorsa, taban sınıf `System.Object` olur ([temel sınıflar](classes.md#base-classes)).

### <a name="base-interfaces"></a>Temel arabirimler

Birden çok bölümde belirtilen bir türün temel arabirimler kümesi, her bölümde belirtilen temel arabirimlerin birleşimidir. Belirli bir temel arabirim her bölümde yalnızca bir kez adlandırılabilir, ancak birden fazla parçanın aynı temel arabirimleri adlandırmasına izin verilir. Yalnızca belirli bir temel arabirimin üyelerinin bir uygulanması gerekir.

örnekte
```csharp
partial class C: IA, IB {...}

partial class C: IC {...}

partial class C: IA, IB {...}
```
sınıf `C` için temel arabirimler kümesi `IA`, `IB`ve `IC`.

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

Kısmi yöntemlerin ([kısmi Yöntemler](classes.md#partial-methods)) dışında, birden çok bölümde belirtilen bir türün üyeleri kümesi, her bölümde belirtilen üye kümesinin birleşimidir. Tür bildiriminin tüm bölümlerinin gövdeleri aynı bildirim alanını ([Bildirimler](basic-concepts.md#declarations)) ve her üyenin kapsamını ([kapsamlar](basic-concepts.md#scopes)) paylaşır ve tüm parçaların gövdeleriyle birlikte genişletilir. Herhangi bir üyenin erişilebilirlik etki alanı her zaman kapsayan türün tüm parçalarını içerir; tek bir bölümde bildirildiği `private` üyeye, başka bir bölümden serbestçe erişilebilir. Üyenin `partial` değiştiriciyle bir tür olmadığı durumlar dışında, türün birden fazla bölümünde aynı üyeyi bildirmek için derleme zamanı hatası vardır.

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

Kısmi Yöntemler erişim değiştiricileri tanımlanamaz, ancak örtülü olarak `private`. Dönüş türü `void`olmalıdır ve parametreleri `out` değiştiriciye sahip olamaz. Tanımlayıcı `partial`, bir yöntem bildiriminde özel bir anahtar sözcük olarak tanınır ve yalnızca `void` türünden önce görünür. Aksi takdirde, normal tanımlayıcı olarak kullanılabilir. Kısmi bir yöntem, arabirim yöntemlerini açıkça uygulayamaz.

İki tür kısmi yöntem bildirimi vardır: Yöntem bildiriminin gövdesi noktalı virgül ise, bildirim bir ***tanımlayıcı kısmi yöntem bildirimi***olarak kabul edilir. Gövde bir *blok*olarak verilirse, bildirim bir ***Uygulama kısmi yöntem bildirimi***olarak kabul edilir. Bir tür bildiriminin bölümlerinde, belirli bir imzayla yalnızca bir adet kısmi yöntem bildirimi tanımlama olabilir ve belirli bir imzaya sahip kısmi yöntem bildirimi yalnızca bir uygulama olabilir. Bir uygulama kısmi yöntem bildirimi verilirse, karşılık gelen bir tanımlayıcı kısmi yöntem bildirimi mevcut olmalıdır ve bildirimlerin aşağıdaki gibi eşleşmesi gerekir:

* Bildirimlerin aynı değiştiricilere sahip olması gerekir (aynı sırada olması olmasa da), yöntem adı, tür parametrelerinin sayısı ve parametre sayısı.
* Bildirimlerdeki karşılık gelen parametreler aynı değiştiricilere sahip olmalıdır (aynı sırada olması olmasa da) ve aynı türlerdir (tür parametre adlarında mod farklılıkları).
* Bildirimlerdeki karşılık gelen tür parametreleri aynı kısıtlamalara (tür parametresi adlarında mod farklılıkları) sahip olmalıdır.

Uygulama kısmi yöntem bildirimi, karşılık gelen tanımlayan kısmi Yöntem bildirimiyle aynı bölümde bulunabilir.

Yalnızca tanımlama bir kısmi yöntem aşırı yükleme çözümüne katılır. Bu nedenle, bir uygulama bildiriminin verilip verilmediğini belirtir, çağırma ifadeleri kısmi yöntemin çağırmaları için çözüm alabilir. Kısmi bir yöntem her zaman `void`döndürdüğünden, bu tür çağırma ifadeleri her zaman ifade deyimleri olur. Ayrıca, kısmi bir yöntem örtük olarak `private`, bu tür deyimler her zaman, kısmi yöntemin bildirildiği tür bildiriminin parçalarından biri içinde olur.

Kısmi bir tür bildiriminin hiçbir bölümü verili bir kısmi Yöntem için uygulama bildirimi içermiyorsa, çağıran herhangi bir ifade deyimi yalnızca Birleşik tür bildiriminden kaldırılır. Bu nedenle, bileşen ifadeleri dahil olmak üzere çağırma ifadesinin çalışma zamanında hiçbir etkisi yoktur. Kısmi yöntemin kendisi de kaldırılır ve birleştirilmiş tür bildiriminin üyesi olmayacaktır.

Belirli bir kısmi Yöntem için uygulama bildirimi varsa, kısmi yöntemlerin etkinleştirmeleri korunur. Kısmi Yöntem, aşağıdaki durumlar hariç, uygulama kısmi Yöntem bildirimine benzer bir yöntem bildirimine göre artış sağlar:

* `partial` değiştiricisi dahil değildir
* Elde edilen yöntem bildiriminde bulunan öznitelikler, tanımlama ve Uygulama kısmi Yöntem bildiriminin belirtilmemiş sırada tanımlanmasının Birleşik öznitelikleridir. Yinelemeler kaldırılmaz.
* Sonuç yöntemi bildiriminin parametreleri üzerindeki öznitelikleri, tanımlanmasının karşılık gelen parametrelerinin Birleşik öznitelikleridir ve kısmi Yöntem bildiriminin uygulanması belirtilmemiş sırayla yapılır. Yinelemeler kaldırılmaz.

Bir tanımlama bildirimi ve kısmi Yöntem için bir uygulama bildirimi verilmezse, aşağıdaki kısıtlamalar uygulanır:

* Yönteme bir temsilci oluşturmak için derleme zamanı hatası ([temsilci oluşturma ifadeleri](expressions.md#delegate-creation-expressions)).
* Bir ifade ağacı türüne dönüştürülen anonim bir işlev içinde `M` başvurmak için derleme zamanı hatası ([anonim işlev dönüştürmelerinden ifade ağacı türlerine yönelik değerlendirme](conversions.md#evaluation-of-anonymous-function-conversions-to-expression-tree-types)).
* `M` çağrısının bir parçası olarak gerçekleşen ifadeler, derleme zamanı hatalarına neden olabilecek kesin atama durumunu ([kesin atama](variables.md#definite-assignment)) etkilemez.
* `M` bir uygulama için giriş noktası olamaz ([uygulama başlatma](basic-concepts.md#application-startup)).

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

Genişletilebilir bir türün her bölümü aynı ad alanı içinde bildirilmelidir, ancak parçalar genellikle farklı ad alanı bildirimleri içinde yazılır. Bu nedenle, her bölüm için farklı `using` yönergeleri ([yönergeler kullanılarak](namespaces.md#using-directives)) bulunabilir. Tek bir bölüm içindeki basit adları ([tür çıkarımı](expressions.md#type-inference)) yorumlarken, yalnızca o parçayı kapsayan ad alanı bildiriminin `using` yönergeleri göz önünde bulundurululur. Bu, farklı parçalardan farklı anlamlara sahip olan aynı tanımlayıcıya neden olabilir:
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

*Class_declaration* yeni bir bildirim alanı ([Bildirimler](basic-concepts.md#declarations)) oluşturur ve hemen *class_declaration* tarafından bulunan *class_member_declaration*, bu bildirim alanına yeni üyeler getirir. *Class_member_declaration*s için aşağıdaki kurallar geçerlidir:

*  Örnek oluşturucular, Yıkıcılar ve statik oluşturucular, hemen kapsayan sınıf ile aynı ada sahip olmalıdır. Tüm diğer üyelerin, hemen kapsayan sınıfın adından farklı adlara sahip olması gerekir.
*  Bir sabit, alan, özellik, olay veya tür adı aynı sınıfta belirtilen diğer tüm üyelerin adlarından farklı olmalıdır.
*  Bir yöntemin adı aynı sınıfta belirtilen diğer tüm yöntemlerin adlarından farklı olmalıdır. Buna ek olarak, bir yöntemin imzası ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) aynı sınıfta belirtilen diğer tüm yöntemlerin imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki yöntem yalnızca `ref` ve `out`farklı imzalara sahip olamaz.
*  Örnek oluşturucunun imzası, aynı sınıfta belirtilen diğer tüm örnek oluşturucuların imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki oluşturucunun yalnızca `ref` ve `out`farklı imzaları bulunmayabilir.
*  Bir dizin oluşturucunun imzası aynı sınıfta belirtilen diğer tüm dizin oluşturucularının imzalarından farklı olmalıdır.
*  Bir işlecin imzası aynı sınıfta belirtilen diğer tüm işleçlerin imzalarından farklı olmalıdır.

Bir sınıf türünün devralınan üyeleri ([Devralma](classes.md#inheritance)) bir sınıfın bildirim alanının parçası değildir. Bu nedenle, türetilmiş bir sınıfın devralınan üye ile aynı ad veya imzaya sahip bir üyeyi bildirmesine izin verilir (yani, devralınan üyeyi etkiler).

### <a name="the-instance-type"></a>Örnek türü

Her sınıf bildiriminde ilişkili bir bağlı tür ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)), ***örnek türü***vardır. Genel sınıf bildiriminde, örnek türü, her bir sağlanan tür bağımsız değişkeni karşılık gelen tür parametresi olan tür bildiriminden oluşturulmuş bir tür ([oluşturulmuş türler](types.md#constructed-types)) oluşturularak oluşturulur. Örnek türü tür parametrelerini kullandığından, yalnızca tür parametrelerinin kapsamda olduğu durumlarda kullanılabilir; diğer bir deyişle, sınıf bildiriminin içinde. Örnek türü, sınıf bildiriminin içinde yazılmış kod için `this` türüdür. Genel olmayan sınıflar için örnek türü yalnızca belirtilen sınıftır. Aşağıda, örnek türleriyle birlikte çeşitli sınıf bildirimleri gösterilmektedir: 
```csharp
class A<T>                           // instance type: A<T>
{
    class B {}                       // instance type: A<T>.B
    class C<U> {}                    // instance type: A<T>.C<U>
}

class D {}                           // instance type: D
```

### <a name="members-of-constructed-types"></a>Oluşturulmuş türlerin üyeleri

Oluşturulmuş bir türün devralınmamış üyeleri, üye bildirimindeki her bir *type_parameter* , oluşturulan türün karşılık gelen *type_argument* yerine koyarak alınır. Değiştirme işlemi, tür bildirimlerinin anlam anlamını temel alır ve yalnızca metinsel değiştirme değildir.

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

Genel sınıf bildirimindeki üye `a` türü, "iki boyutlu `T`dizisi `Gen`, bu nedenle yukarıdaki oluşturulan türdeki üye `a` türü" tek boyutlu bir dizi `int`"veya `int[,][]`olan iki boyutlu dizi dizisi olur.

Örnek işlevi üyeleri içinde `this` türü, kapsayan bildirimin örnek türüdür ([örnek türü](classes.md#the-instance-type)).

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

*  Devralma geçişlidir. `C` `B`türediyse ve `B` `A`türetildiyse, `C` `B` belirtilen üyelerin yanı sıra `A`olarak belirtilen üyeleri devralır.
*  Türetilmiş bir sınıf doğrudan temel sınıfını genişletir. Türetilmiş bir sınıf, devralananlara yeni üyeler ekleyebilir, ancak devralınmış bir üyenin tanımını kaldıramaz.
*  Örnek oluşturucular, Yıkıcılar ve statik oluşturucular devralınmaz, ancak diğer tüm Üyeler, belirtilen erişilebilirliği ([üye erişimi](basic-concepts.md#member-access)) ne olursa olsun. Ancak, kendilerine ait olan erişilebilirliğe bağlı olarak, devralınan Üyeler türetilmiş bir sınıfta erişilebilir olmayabilir.
*  Türetilmiş bir sınıf, aynı ad veya imzaya sahip yeni üyeler bildirerek devralınan üyeleri ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)) de ***gizleyebilir*** . Ancak devralınan bir üyeyi gizlemenin o üyeyi kaldırmadığını unutmayın; yalnızca bu üyeyi doğrudan türetilmiş sınıf üzerinden erişilmez hale getirir.
*  Bir sınıf örneği, sınıfta ve temel sınıflarında belirtilen tüm örnek alanlarının bir kümesini içerir ve bir türetilmiş sınıf türünden herhangi bir temel sınıf türünden herhangi birine bir örtük dönüştürme ([örtük başvuru dönüştürmeleri](conversions.md#implicit-reference-conversions)) bulunur. Bu nedenle, bir türetilmiş sınıfın örneğine bir başvuru, temel sınıflarının herhangi birinin bir örneğine başvuru olarak kabul edilebilir.
*  Bir sınıf sanal yöntemleri, özellikleri ve Dizin oluşturucuyu bildirebilir ve türetilmiş sınıflar bu işlev üyelerinin uygulamasını geçersiz kılabilir. Bu, sınıfların bir işlev üyesi çağrısı tarafından gerçekleştirilen eylemlerde çok biçimli davranışlar sergilemesine olanak sağlar. Bu işlev üyesinin çağrıldığı örnek çalışma zamanı türüne bağlı olarak değişir.

Oluşturulmuş bir sınıf türünün devralınan üyesi, *class_base* belirtiminde karşılık gelen tür parametrelerinin her bir oluşumu için oluşturulan türün tür bağımsız değişkenlerini değiştirerek bulunan en hızlı temel sınıf türünün ([temel sınıflar](classes.md#base-classes)) üyeleridir. Bu Üyeler sırasıyla, üye bildirimindeki her bir *type_parameter* için, *class_base* belirtiminin karşılık gelen *type_argument* yerine koyarak dönüştürülür.

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

Yukarıdaki örnekte, oluşturulan tür `D<int>`, tür parametresi `int` tür bağımsız değişkeni yerine `public int G(string s)` devralınmamış bir üyeye sahiptir `T`. `D<int>` Ayrıca, sınıf bildirimi `B`devralınmış bir üyeye sahiptir. Bu devralınan üye, öncelikle temel sınıf belirtimi `B<T[]>``T` için `int` yerine `D<int>` temel sınıf türü `B<int[]>` belirlenerek belirlenir. `B`için bir tür bağımsız değişkeni olarak, `int[]`, devralınan üye `public int[] F(long index)`oluşturan `public U F(long index)``U` için değiştirilir.

### <a name="the-new-modifier"></a>Yeni değiştirici

Bir *class_member_declaration* devralınan üyeyle aynı ada veya imzaya sahip bir üyeyi bildirmesine izin verilir. Bu gerçekleştiğinde, türetilmiş sınıf üyesi temel sınıf üyesini ***gizleyecek*** şekilde söylenir. Devralınan bir üyenin gizlenmesi bir hata sayılmaz, ancak derleyicinin bir uyarı vermesine neden olur. Uyarıyı bastırmak için, türetilen sınıf üyesinin bildirimi, türetilen üyenin temel üyeyi gizleyecek şekilde olduğunu göstermek için `new` değiştiricisini içerebilir. Bu konu [Devralma yoluyla gizlenerek](basic-concepts.md#hiding-through-inheritance)daha ayrıntılı bir şekilde ele alınmıştır.

Bir `new` değiştiricisi devralınan bir üyeyi gizlemez bir bildirime dahil edilir, bu etkiye bir uyarı verilir. Bu uyarı `new` değiştiricisi kaldırılarak bastırılır.

### <a name="access-modifiers"></a>Erişim değiştiricileri

*Class_member_declaration* , beş olası tanımlanmış erişilebilirliği ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)) herhangi birine sahip olabilir: `public`, `protected internal`, `protected`, `internal`veya `private`. `protected internal` birleşimi haricinde, birden fazla erişim değiştiricisi belirtmek için derleme zamanı hatası olur. Bir *class_member_declaration* erişim değiştiricileri içermiyorsa, `private` varsayılır.

### <a name="constituent-types"></a>Anayent türleri

Bir üyenin bildiriminde kullanılan türler, o üyenin bileşen türleri olarak adlandırılır. Olası yapısal türler, bir sabit, alan, özellik, olay veya dizin oluşturucunun türü, bir yöntemin veya işlecin dönüş türü ve bir yöntem, Dizin Oluşturucu, işleç veya örnek oluşturucusunun parametre türleri olabilir. Üyenin bileşen türleri en az o üyenin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

### <a name="static-and-instance-members"></a>Statik ve örnek üyeleri

Bir sınıfın üyeleri ***statik Üyeler*** veya ***örnek üyeleridir***. Genel olarak, statik üyeleri nesnelere ait olan sınıf türlerine ve örnek üyelerine (sınıf türü örnekleri) ait olacak şekilde düşünmek yararlı olur.

Bir alan, yöntem, özellik, olay, işleç veya Oluşturucu bildirimi `static` değiştiricisi içerdiğinde, statik bir üye bildirir. Ayrıca, bir sabit veya tür bildirimi dolaylı olarak statik bir üye bildirir. Statik Üyeler aşağıdaki özelliklere sahiptir:

*  Statik bir üyeye `M`, form `E.M`*member_access* ([üye erişimi](expressions.md#member-access)) içinde başvuruluyorsa `E`, `M`içeren bir tür belirtmelidir. Bir örneği göstermek için `E` derleme zamanı hatasıdır.
*  Statik bir alan, belirli bir kapalı sınıf türünün tüm örnekleri tarafından paylaşılacak tam olarak bir depolama konumunu tanımlar. Belirli bir kapalı sınıf türünün kaç örneğinin oluşturulduğuna bakılmaksızın, bir statik alanın yalnızca bir kopyası vardır.
*  Statik işlev üyesi (yöntem, özellik, olay, işleç veya Oluşturucu) belirli bir örnek üzerinde çalışmaz ve bu tür bir işlev üyesinde `this` başvuruda bulunmak için derleme zamanı hatası vardır.

Bir alan, yöntem, özellik, olay, Dizin Oluşturucu, Oluşturucu veya yıkıcı bildirimi `static` değiştiricisi içermiyorsa, bir örnek üyesi bildirir. (Bir örnek üyesi bazen statik olmayan bir üye olarak adlandırılır.) Örnek üyeleri aşağıdaki özelliklere sahiptir:

*  Bir örnek üyesine `M`, form `E.M`*member_access* ([üye erişimi](expressions.md#member-access)) içinde başvuruluyorsa `E`, `M`içeren bir türün örneğini belirtmelidir. `E` bir türü belirtmek için bağlama zamanı hatasıdır.
*  Bir sınıfın her örneği, sınıfının tüm örnek alanlarını ayrı bir kümesini içerir.
*  Örnek işlev üyesi (yöntem, özellik, Dizin Oluşturucu, örnek Oluşturucu veya yıkıcı), sınıfın belirli bir örneği üzerinde çalışır ve bu örneğe `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.

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

`F` yöntemi bir örnek işlev üyesinde, hem örnek üyelerine hem de statik üyelere erişmek için bir *simple_name* ([basit adlar](expressions.md#simple-names)) kullanılabileceğini gösterir. `G` yöntemi, bir statik işlev üyesinde, bir *simple_name*bir örnek üyesine erişmek için derleme zamanı hatası olduğunu gösterir. `Main` yöntemi, bir *member_access* ([üye erişimi](expressions.md#member-access)), örnek üyelerine örnekler aracılığıyla erişilmesi ve statik üyelere türler aracılığıyla erişilmesi gerekir.

### <a name="nested-types"></a>İç içe türler

Bir sınıf veya yapı bildiriminde belirtilen türe ***iç içe geçmiş tür***denir. Bir derleme birimi veya ad alanı içinde belirtilen bir tür, ***iç içe olmayan bir tür***olarak adlandırılır.

örnekte
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
sınıf `B`, sınıf `A`içinde bildirildiği ve sınıf `A`, bir derleme birimi içinde bildirildiği için iç içe olmayan bir tür olduğundan iç içe bir tür.

#### <a name="fully-qualified-name"></a>Tam ad

İç içe bir tür için tam nitelikli ad ([tam adlar](basic-concepts.md#fully-qualified-names)), `S` tür `N` bildirildiği türün tam adı `S.N`.

#### <a name="declared-accessibility"></a>Tanımlanan erişilebilirlik

İç içe olmayan türlerde `public` veya `internal` tarafından tanımlanan erişilebilirlik bulunabilir ve varsayılan olarak `internal` olarak tanımlanmış erişilebilirliği vardır. İç içe türler, kapsayan türün bir sınıf veya yapı olmasına bağlı olarak, bu tanımlanmış erişilebilirlik ve bir veya daha fazla tanımlanmış erişilebilirlik biçimi içerebilir:

*  Bir sınıfta bildirildiği iç içe yerleştirilmiş bir tür, belirtilen beş erişilebilirliği (`public`, `protected internal`, `protected`, `internal`veya `private`) ve diğer sınıf üyeleri gibi, varsayılan olarak belirtilen erişilebilirliği `private` olarak ifade edebilir.
*  Bir yapıda tanımlanmış iç içe yerleştirilmiş bir tür, üç farklı yapı erişilebilirliği (`public`, `internal`veya `private`) ve diğer yapı üyeleri gibi, varsayılan olarak tanımlanmış erişilebilirliği `private`.

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

İç içe yerleştirilmiş bir tür, temel üyeyi gizleyebilir ([ad gizleyerek](basic-concepts.md#name-hiding)). `new` değiştiricisine, iç içe geçmiş tür bildirimlerinde izin verilir, böylece gizleme açıkça ifade edilebilir. Örnek
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
`Base`tanımlanan `M` metodunu gizleyen iç içe bir sınıf `M` gösterir.

#### <a name="this-access"></a>Bu erişim

İç içe bir tür ve kapsayan türü *This_Access* ([Bu erişim](expressions.md#this-access)) ile ilgili özel bir ilişkiye sahip değildir. Özellikle, iç içe bir tür içindeki `this`, kapsayan türün örnek üyelerine başvurmak için kullanılamaz. İç içe bir türün, kapsayan türün örnek üyelerine erişmesi gereken durumlarda, kapsayan türün örneği için `this` iç içe geçmiş türü için bir Oluşturucu bağımsız değişkeni olarak sağlanarak erişim sağlanabilir. Aşağıdaki örnek
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
Bu tekniği gösterir. Bir `C` örneği `Nested` örneğini oluşturur ve `C`örnek üyelerine sonraki erişimi sağlamak için kendi `this` `Nested`oluşturucusuna geçirir.

#### <a name="access-to-private-and-protected-members-of-the-containing-type"></a>Kapsayan türdeki özel ve korunan üyelere erişim

İç içe bir tür, `private` sahip olan ve `protected` belirtilen erişilebilirliği içeren kapsayan türdeki Üyeler de dahil olmak üzere, kapsayan tür tarafından erişilebilen tüm üyelere erişebilir. Örnek
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
iç içe bir sınıf `Nested`içeren bir sınıf `C` gösterir. `Nested`içinde yöntemi `G`, `C`tanımlanmış statik yöntemi `F` çağırır ve `F` özel olarak tanımlanmış erişilebilirliği vardır.

İç içe yerleştirilmiş bir tür, kapsayan türünün temel türünde tanımlanan korumalı üyelere de erişebilir. örnekte
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
iç içe yerleştirilmiş sınıf `Derived.Nested`, `Derived`temel sınıfında tanımlanan `F` korunan yönteme erişir, `Base`bir `Derived`örneği aracılığıyla çağırarak.

#### <a name="nested-types-in-generic-classes"></a>Genel sınıflarda iç içe türler

Genel sınıf bildirimi, iç içe geçmiş tür bildirimleri içerebilir. Kapsayan sınıfın tür parametreleri iç içe türler içinde kullanılabilir. İç içe geçmiş tür bildirimi, yalnızca iç içe geçmiş tür için uygulanan ek tür parametreleri içerebilir.

Genel sınıf bildiriminde yer alan her tür bildirimi örtük olarak genel bir tür bildirimidir. Genel bir tür içinde iç içe geçmiş bir türe başvuru yazarken, türü bağımsız değişkenleri dahil olmak üzere bulunan oluşturulmuş tür, adlandırılmış olmalıdır. Ancak, dış sınıfın içinden iç içe geçmiş tür, nitelendirme olmadan kullanılabilir; Dış sınıfın örnek türü, iç içe geçmiş türü oluşturulurken örtük olarak kullanılabilir. Aşağıdaki örnek, `Inner`oluşturulan oluşturulmuş bir türe başvurmak için üç farklı doğru yolu göstermektedir; İlk ikisi eşdeğerdir:
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

Ayrılmış adlar bildirimleri sunmaz, bu nedenle üye aramasına katılmaz. Ancak, bir bildirimin ilişkili ayrılmış yöntem imzaları devralmaya ([Devralma](classes.md#inheritance)) katılır ve `new` değiştiricisi ([Yeni değiştirici](classes.md#the-new-modifier)) ile gizlenebilir.

Bu adların ayırması üç amaca hizmet eder:

*  Temel uygulamanın C# dil özelliğine Get veya set erişimi için bir yöntem adı olarak sıradan bir tanımlayıcı kullanmasına izin vermek için.
*  Diğer dillerin, C# dil özelliğine Get veya set erişimi için bir yöntem adı olarak sıradan bir tanımlayıcı kullanarak birlikte çalışabilme izni vermek için.
*  Tek bir uygun derleyici tarafından kabul edilen kaynağın, ayrılmış üye adlarının özelliklerinin tüm C# uygulamalarda tutarlı olduğundan emin olmak için.

Yıkıcı ([Yıkıcılar](classes.md#destructors)) bildirimi de bir imzanın ayrılmasına neden olur (yok[ediciler için üye adları ayrılmıştır](classes.md#member-names-reserved-for-destructors)).

#### <a name="member-names-reserved-for-properties"></a>Özellikler için ayrılan üye adları

`T`türündeki bir özellik `P` ([Özellikler](classes.md#properties)) için aşağıdaki imzalar ayrılmıştır:

```csharp
T get_P();
void set_P(T value);
```

Özellik salt okunurdur veya salt yazılır olsa bile her iki imza de ayrılır.

örnekte
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
bir sınıf `A` salt okunurdur bir özellik `P`tanımlar, böylece `get_P` ve `set_P` yöntemlerine yönelik imzaları ayırırsınız. Bir sınıf `B` `A` türetilir ve bu ayrılmış imzaların her ikisini birden gizler. Örnek, çıktıyı üretir:
```console
123
123
456
```

#### <a name="member-names-reserved-for-events"></a>Olaylar için ayrılan üye adları

`T`temsilci türünün olay `E` ([olayları](classes.md#events)) için aşağıdaki imzalar ayrılmıştır:
```csharp
void add_E(T handler);
void remove_E(T handler);
```

#### <a name="member-names-reserved-for-indexers"></a>Dizin oluşturucular için ayrılan üye adları

Parametre listesi `L``T` türündeki bir Dizin Oluşturucu ([Dizin oluşturucular](classes.md#indexers)) için aşağıdaki imzalar ayrılmıştır:
```csharp
T get_Item(L);
void set_Item(L, T value);
```

Dizin Oluşturucu salt okunurdur veya salt yazılır olsa bile her iki imza da ayrılır.

Ayrıca `Item` üye adının ayrılmış olması.

#### <a name="member-names-reserved-for-destructors"></a>Yok ediciler için ayrılan üye adları

Yıkıcı ([Yıkıcılar](classes.md#destructors)) içeren bir sınıf için aşağıdaki imza ayrılmıştır:
```csharp
void Finalize();
```

## <a name="constants"></a>Sabitler

***Sabit*** değeri, derleme zamanında hesaplanılabilen bir değer olan sabit bir değeri temsil eden bir sınıf üyesidir. *Constant_declaration* , belirli bir türün bir veya daha fazla sabitlerini tanıtır.

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

*Constant_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)), bir `new` değiştiricisi ([Yeni değiştirici](classes.md#the-new-modifier)) ve dört erişim değiştiricisinin geçerli bir birleşimini ([erişim değiştiricileri](classes.md#access-modifiers)) içerebilir. Öznitelikler ve değiştiriciler *constant_declaration*tarafından belirtilen tüm üyelere uygulanır. Sabitler statik üye olarak kabul edilse de, *constant_declaration* gerektirmez ve `static` değiştiriciye izin vermez. Aynı değiştiricinin Sabit bildiriminde birden çok kez görünmesi hatadır.

Constant_declaration *türü* , bildirim tarafından tanıtılan üyelerin türünü belirtir. Türün ardından her biri yeni bir üye tanıtan *constant_declarator*s listesi bulunur. *Constant_declarator* , üyeyi belirten bir *tanımlayıcıdan* , ardından bir "`=`" belirteci ve sonra üyenin değerini veren bir *constant_expression* ([sabit ifadeler](expressions.md#constant-expressions)) oluşur.

Sabit bildiriminde belirtilen *tür* `sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, `bool`, `string`, *enum_type*veya *reference_type*olmalıdır. Her *constant_expression* , hedef türün bir değerini veya örtük bir dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) tarafından hedef türe dönüştürülebilen bir türü vermelidir.

Bir sabit *türünün* en az sabitin ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olması gerekir.

Bir sabit değeri *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) kullanılarak bir ifadede elde edilir.

Bir sabit, bir *constant_expression*katılabilir. Bu nedenle, bir sabit, *constant_expression*gerektiren herhangi bir yapı içinde kullanılabilir. Bu tür yapılara örnek olarak `case` Etiketler, `goto case` deyimler, `enum` üye bildirimleri, öznitelikler ve diğer sabit bildirimler dahildir.

[Sabit ifadelerde](expressions.md#constant-expressions)açıklandığı gibi, *constant_expression* derleme zamanında tam olarak değerlendirilebilen bir ifadedir. `string` dışında bir *reference_type* null olmayan bir değer oluşturmanın tek yolu `new` işlecini uygulamak ve `new` işlecine bir *constant_expression*izin verilmesinden bu *yana reference_type dışındaki `string` s sabitleri*için tek olası değer `null`.

Sabit bir değer için sembolik bir ad istendiğinde, ancak bu değerin türü sabit bildirimde izin verilmediğinde veya değer derleme zamanında bir *constant_expression*tarafından hesaplanmadığında, bunun yerine bir `readonly` alanı ([salt okunur alan](classes.md#readonly-fields)) kullanılabilir.

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

Sabitler dairesel bir şekilde olmadığı sürece sabitlerin aynı program içindeki diğer sabitlere bağlı olmasına izin verilir. Derleyici, sabit bildirimleri uygun sırada değerlendirmek için otomatik olarak düzenler. örnekte
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
derleyici önce `A.Y`değerlendirir, sonra `B.Z`değerlendirir ve son olarak `A.X`değerlendirir `10`, `11`ve `12`değerlerini üretir. Sabit bildirimler diğer programlardaki sabitlere bağlı olabilir, ancak bu tür bağımlılıklar yalnızca tek bir yönde mümkündür. Yukarıdaki örneğe başvurma, `A` ve `B` ayrı programlarda bildirilirse `A.X` `B.Z`bağımlı olması mümkündür ancak `B.Z` aynı anda `A.Y`bağımlı olmayabilir.

## <a name="fields"></a>Alanlar

***Alan*** , bir nesne veya sınıfla ilişkili bir değişkeni temsil eden bir üyesidir. *Field_declaration* , belirli bir türün bir veya daha fazla alanını tanıtır.

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

Bir *field_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)), bir `new` değiştirici ([Yeni değiştirici](classes.md#the-new-modifier)), dört erişim değiştiricisinin geçerli bir birleşimi ([erişim değiştiricileri](classes.md#access-modifiers)) ve bir `static` değiştiricisi ([statik ve örnek alanları](classes.md#static-and-instance-fields)) içerebilir. Ayrıca, bir *field_declaration* `readonly` değiştirici ([salt okunur alanlar](classes.md#readonly-fields)) veya `volatile` değiştirici ([geçici alanlar](classes.md#volatile-fields)) içerebilir, ancak ikisini birden içeremez. Öznitelikler ve değiştiriciler *field_declaration*tarafından belirtilen tüm üyelere uygulanır. Aynı değiştiricinin bir alan bildiriminde birden çok kez görünmesi hatadır.

Field_declaration *türü* , bildirim tarafından tanıtılan üyelerin türünü belirtir. Türün ardından her biri yeni bir üye tanıtan *variable_declarator*s listesi bulunur. *Variable_declarator* , bu üyeyi ve isteğe bağlı olarak bir "`=`" belirteci ve bu üyenin ilk değerini veren bir *variable_initializer* ([değişken başlatıcıları](classes.md#variable-initializers)) içeren bir *tanımlayıcıdan* oluşur.

Alanın *türü* en az alanın kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir alanın değeri, *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) kullanılarak bir ifadede elde edilir. Salt okunur olmayan bir alanın değeri, *atama* ([atama işleçleri](expressions.md#assignment-operators)) kullanılarak değiştirilir. Salt okunur olmayan bir alanın değeri, sonek artırma ve azaltma işleçleri ([Sonek artışı ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators)) ve önek artırma ve azaltma Işleçleri ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) kullanılarak elde edilebilir ve değiştirilebilir.

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

Bir alan bildirimi `static` değiştirici içerdiğinde, bildirim tarafından tanıtılan alanlar ***statik alanlardır***. `static` değiştirici yoksa, bildirim tarafından tanıtılan alanlar ***örnek alanlardır***. Statik alanlar ve örnek alanları, C#tarafından desteklenen çeşitli değişken ([değişken](variables.md)) çeşitleridir ve bunlar sırasıyla ***statik değişkenler*** ve ***örnek değişkenler***olarak adlandırılır.

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

Bir alana bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir alan ise, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek alanı ise, E içeren bir türün örneğini belirtmelidir.`M`

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="readonly-fields"></a>ReadOnly alanları

Bir *field_declaration* `readonly` değiştiricisi içerdiğinde, bildirim tarafından tanıtılan alanlar ***salt okunur alanlardır***. Salt okunur alanlara doğrudan atamalar yalnızca bu bildirimin bir parçası veya aynı sınıftaki bir örnek Oluşturucu ya da statik oluşturucu içinde olabilir. (Salt okunur bir alan, bu bağlamlarda birden çok kez atanabilir.) Özellikle, bir `readonly` alana doğrudan atamalara yalnızca aşağıdaki bağlamlarda izin verilir:

*  Alanı tanıtan *variable_declarator* (bildirime bir *variable_initializer* ekleyerek).
*  Bir örnek alanı için, alan bildirimini içeren sınıfın örnek oluşturucularında; statik bir alan için, alan bildirimini içeren sınıfın statik oluşturucusunda. Bunlar ayrıca, `readonly` alanı `out` veya `ref` parametresi olarak geçirmek için geçerli olduğu tek bağlamlardır.

Bir `readonly` alana atamaya veya bir `out` ya da başka bir bağlamda `ref` parametre olarak geçirmeye çalışılması derleme zamanı hatasıdır.

#### <a name="using-static-readonly-fields-for-constants"></a>Sabitler için statik salt okunur alanlar kullanma

Bir `static readonly` alanı, sabit değer için bir sembolik ad istendiği zaman, ancak değer türüne `const` bildiriminde izin verilmediğinde veya değer derleme zamanında hesaplanmadığında faydalıdır. örnekte
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
`Black`, `White`, `Red`, `Green`ve `Blue` üyeleri, değerlerinin derleme zamanında hesaplanamadığı için `const` üye olarak bildirilemez. Ancak, bunun yerine `static readonly` bildirimi aynı etkiye sahiptir.

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

`Program1` ve `Program2` ad alanları ayrı olarak derlenen iki programı gösterir. `Program1.Utils.X` statik bir salt okunur alan olarak bildirildiği için, `Console.WriteLine` deyimin çıktısı derleme zamanında bilinmez, ancak bunun yerine çalışma zamanında elde edilir. Bu nedenle, `X` değeri değiştirilirse ve `Program1` yeniden derlense, `Program2` yeniden derlenmese bile `Console.WriteLine` deyimin yeni değeri çıkışı olur. Ancak, `X` bir sabit değer vardı, `X` değeri `Program2` derlenerek elde edilir ve `Program2` yeniden derlenene kadar `Program1` değişikliklerinden etkilenmeden kalır.

### <a name="volatile-fields"></a>Geçici alanlar

Bir *field_declaration* `volatile` değiştiricisi içerdiğinde, bu bildirim tarafından tanıtılan alanlar ***geçici alanlardır***.

Geçici olmayan alanlar için, yönergeleri yeniden sıralamak için en iyi duruma getirme teknikleri, *lock_statement* ([kilit beyanı](statements.md#the-lock-statement)) tarafından sağlanmış gibi eşitleme olmadan alanlara erişen çok iş parçacıklı programlarda beklenmedik ve öngörülemeyen sonuçlara yol açabilir. Bu iyileştirmeler derleyici tarafından, çalışma zamanı sistemine veya donanımla gerçekleştirilebilir. Geçici alanlar için, bu tür yeniden sıralama iyileştirmeleri kısıtlanmıştır:

*  Geçici bir alan okuma, ***geçici okuma***olarak adlandırılır. Geçici okuma "alma semantiğini" içerir; diğer bir deyişle, yönerge dizisinde bundan sonra meydana gelen tüm bellek başvurularından önce gerçekleşmesi garanti edilir.
*  Geçici bir alan yazma, ***geçici yazma***olarak adlandırılır. Geçici yazma "yayın semantiğini" içerir; diğer bir deyişle, yönerge dizisindeki yazma yönergesinden önce herhangi bir bellek başvurularından sonra gerçekleşmesi garanti edilir.

Bu kısıtlamalar, tüm iş parçacıklarının gerçekleştirilen diğer bir iş parçacığı tarafından gerçekleştirilen geçici yazmaları gözlemleyecek şekilde tüm iş parçacıkları tarafından gerçekleştirildiğinden emin olur. Tüm yürütme iş parçacıklarında görüldüğü şekilde, geçici yazma işlemlerinin tek toplam sıralamasını sağlamak için uygun bir uygulama gerekmez. Geçici bir alanın türü aşağıdakilerden biri olmalıdır:

*  Bir *reference_type*.
*  `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `char`, `float`, `bool`, `System.IntPtr`veya `System.UIntPtr`türü.
*  `byte`, `sbyte`, `short`, `ushort`, `int`veya `uint`enum temel türüne sahip bir *enum_type* .

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
```console
result = 143
```

Bu örnekte Yöntem `Main` yöntemi `Thread2`çalıştıran yeni bir iş parçacığı başlatır. Bu yöntem, bir değeri `result`adlı geçici olmayan bir alana depolar, sonra `true` geçici alan `finished`depolar. Ana iş parçacığı, alan `finished` `true`olarak ayarlanacağını bekler, sonra alanı `result`okur. `finished` `volatile`olarak bildirildiği için, ana iş parçacığının `result`alanından `143` değeri okuması gerekir. `finished` alan `volatile`bildirilmemiş ise, deponun `finished`depolamadan sonra ana iş parçacığında görünür hale `result` ve bu nedenle ana iş parçacığının alan `0` değer `result`okuması için izin verilir. `volatile` alanı olarak `finished` bildirme, bu tür tutarsızlığın yapılmasını engeller.

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
```console
b = False, i = 0
```
`b` ve `i` her ikisi de otomatik olarak varsayılan değerlere başlatılmış olduğundan.

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
```console
x = 1.4142135623731, i = 100, s = Hello
```
`x` bir atama, statik alan başlatıcılarının yürütülmesi ve atamaları `i` ve `s` örnek alanı başlatıcılarının yürütülmesi durumunda meydana gelirse oluşur.

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
```console
a = 1, b = 2
```
statik alanlar `a` ve `b`, başlatıcıları yürütülmeden önce `0` (`int`için varsayılan değer) olarak başlatıldığından. `a` başlatıcısı çalıştığında, `b` değeri sıfırdır ve bu nedenle `a` `1`olarak başlatılır. `b` başlatıcısı çalıştığında, `a` değeri zaten `1`ve `b` `2`olarak başlatılır.

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
```console
Init A
Init B
1 1
```
veya çıkış:
```console
Init B
Init A
1 1
```
`X`başlatıcısının ve `Y`başlatıcısının yürütülmesi her iki sırada da gerçekleşebildiğinden; Bunlar yalnızca bu alanlara başvurularından önce gerçekleşirler. Ancak, örnekte:
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
```console
Init B
Init A
1 1
```
statik oluşturucuların çalıştırıldığı zaman ( [statik oluşturucularda](classes.md#static-constructors)tanımlandığı gibi) için kurallar, `B`statik oluşturucusunun (ve bu nedenle `B`statik alan başlatıcılarının) `A`statik Oluşturucusu ve alan başlatıcılarından önce çalıştırılması gerektiğini sağlar.

#### <a name="instance-field-initialization"></a>Örnek alanı başlatma

Bir sınıfın örnek alanı değişken başlatıcıları, bu sınıfın örnek oluşturucularından ([Oluşturucu başlatıcıların](classes.md#constructor-initializers)) herhangi birine girişte hemen yürütülen atama dizisine karşılık gelir. Değişken başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülür. Sınıf örneği oluşturma ve başlatma işlemi, [örnek oluşturucularda](classes.md#instance-constructors)daha ayrıntılı olarak açıklanmıştır.

Örnek alanı için değişken Başlatıcısı oluşturulan örneğe başvuramaz. Bu nedenle, bir değişken başlatıcısındaki `this` başvurmak için derleme zamanı hatası, bir değişken başlatıcısı için bir *simple_name*aracılığıyla bir örnek üyesine başvuruda bulunmak üzere bir derleme zamanı hatası olduğundan. örnekte
```csharp
class A
{
    int x = 1;
    int y = x + 1;        // Error, reference to instance member of this
}
```
`y` için değişken başlatıcısı, oluşturulmakta olan Örneğin bir üyesine başvurduğundan derleme zamanı hatasına neden olur.

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

Bir *method_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)) geçerli bir birleşimini, `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı](classes.md#sealed-methods)Yöntemler), `abstract` ([soyut yöntemler](classes.md#abstract-methods)) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerini içerebilir.

Aşağıdakilerin tümü doğru ise bir bildirimin geçerli bir değiştiriciler birleşimi vardır:

*  Bildirim, geçerli bir erişim değiştiricileri ([erişim değiştiricileri](classes.md#access-modifiers)) birleşimini içerir.
*  Bildirim, aynı değiştiriciyi birden çok kez içermez.
*  Bildirim şu değiştiricilerin en çok birini içerir: `static`, `virtual`ve `override`.
*  Bildirim şu değiştiricilerin en çok birini içerir: `new` ve `override`.
*  Bildirim `abstract` değiştiricisini içeriyorsa, bildirim şu değiştiricilerin hiçbirini içermez: `static`, `virtual`, `sealed` veya `extern`.
*  Bildirim `private` değiştiricisini içeriyorsa, bildirim şu değiştiricilerin hiçbirini içermez: `virtual`, `override`veya `abstract`.
*  Bildirim `sealed` değiştiricisini içeriyorsa, bildirim `override` değiştiricisini de içerir.
*  Bildirim `partial` değiştiricisini içeriyorsa, şu değiştiricilerin hiçbirini içermez: `new`, `public`, `protected`, `internal`, `private`, `virtual`, `sealed`, `override`, `abstract`veya `extern`.

`async` değiştiricisine sahip bir yöntem zaman uyumsuz bir işlevdir ve [zaman uyumsuz işlevlerde](classes.md#async-functions)açıklanan kuralları izler.

Bir yöntem bildiriminin *return_type* , yöntemi tarafından hesaplanan ve döndürülen değerin türünü belirtir. Yöntemin bir değer döndürmezse *return_type* `void`. Bildirim `partial` değiştiricisini içeriyorsa, dönüş türü `void`olmalıdır.

*MEMBER_NAME* , yöntemin adını belirtir. Yöntem açık arabirim üyesi uygulaması ([Açık arabirim üyesi uygulamalar](interfaces.md#explicit-interface-member-implementations)) değilse, *MEMBER_NAME* yalnızca bir *tanıtıcıdır*. Açık arabirim üyesi uygulama için *MEMBER_NAME* , bir *interface_type* ve ardından bir "`.`" ve bir *tanımlayıcı*oluşur.

İsteğe bağlı *type_parameter_list* , yöntemin tür parametrelerini belirtir ([tür parametreleri](classes.md#type-parameters)). Bir *type_parameter_list* belirtilirse, yöntemi ***genel bir yöntemdir***. Metodun bir `extern` değiştiricisi varsa, bir *type_parameter_list* belirtilemez.

İsteğe bağlı *formal_parameter_list* yönteminin parametrelerini belirtir ([Yöntem parametreleri](classes.md#method-parameters)).

İsteğe bağlı *type_parameter_constraints_clause*, bağımsız tür parametrelerinde ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) kısıtlamaları belirtir ve yalnızca bir *type_parameter_list* sağlandıysa ve yöntemin bir `override` değiştiricisi yoksa belirtilebilir.

*Return_type* ve bir yöntemin *formal_parameter_list* başvuruda bulunulan türlerin her biri, metodun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak en az erişilebilir olmalıdır.

*Method_body* noktalı virgül, ***deyim gövdesi*** veya ***ifade gövdesi***. Deyim gövdesi, yöntemi çağrıldığında yürütülecek deyimleri belirten bir *bloğundan*oluşur. Bir ifade gövdesi, bir *ifadenin* ve noktalı virgülden oluşan `=>` oluşur ve yöntem çağrıldığında gerçekleştirilecek tek bir ifadeyi gösterir. 

`abstract` ve `extern` yöntemleri için *method_body* yalnızca noktalı virgülden oluşur. `partial` yöntemler için *method_body* noktalı virgül, blok gövdesinden veya bir ifade gövdesinden oluşabilir. Diğer tüm yöntemler için *method_body* bir blok gövdesi ya da bir ifade gövdesidir.

*Method_body* noktalı virgül içeriyorsa, bildirim `async` değiştiricisini içermeyebilir.

Ad, tür parametre listesi ve bir yöntemin biçimsel parametre listesi metodun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar. Özellikle, bir yöntemin imzası adından, tür parametrelerinin sayısından ve biçimsel parametrelerinin sayısı, değiştiricilerinden ve türlerinden oluşur. Bu amaçlar için, bir biçimsel parametre türünde oluşan yöntemin tür parametresi, kendi adı tarafından değil, yönteminin tür bağımsız değişkeni listesindeki sıra konumuna göre tanımlanır. Dönüş türü yöntemin imzasının bir parçası değildir, ya da tür parametrelerinin ya da biçimsel parametrelerin adları değildir.

Bir yöntemin adı aynı sınıfta belirtilen diğer tüm yöntemlerin adlarından farklı olmalıdır. Ayrıca, bir yöntemin imzası aynı sınıfta belirtilen diğer tüm yöntemlerin imzalarından farklı olmalıdır ve aynı sınıfta belirtilen iki yöntem yalnızca `ref` ve `out`farklı imzalara sahip olamaz.

Yöntemin *type_parameter*s *method_declaration*tamamında kapsam içinde yer alabilir ve bu kapsam genelinde *return_type*, *method_body*ve *type_parameter_constraints_clause*, ancak *özniteliklerde*olmayan türleri oluşturmak için kullanılabilir.

Tüm biçimsel parametrelerin ve tür parametrelerinin adları farklı olmalıdır.

### <a name="method-parameters"></a>Yöntem parametreleri

Bir yöntemin parametreleri, varsa, yöntemin *formal_parameter_list*tarafından tanımlanır.

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

Biçimsel parametre listesi bir veya daha fazla virgülle ayrılmış parametrelerden oluşur ve bunlardan yalnızca sonuncusu *parameter_array*olabilir.

*Fixed_parameter* , isteğe bağlı bir *öznitelik* kümesi ([öznitelikler](attributes.md)), isteğe bağlı `ref`, `out` veya `this` değiştiricisi, bir *tür*, *tanımlayıcı* ve isteğe bağlı bir *default_argument*oluşur. Her *fixed_parameter* , verilen türdeki bir parametreyi verilen ada bildirir. `this` değiştiricisi yöntemi bir genişletme yöntemi olarak belirler ve yalnızca bir statik metodun ilk parametresinde kullanılabilir. Uzantı yöntemleri [uzantı yöntemlerinde](classes.md#extension-methods)daha ayrıntılı olarak açıklanmıştır.

*Default_argument* bir *fixed_parameter* , ***isteğe bağlı bir parametre***olarak bilinir, ancak *default_argument* olmayan bir *fixed_parameter* ***gerekli bir parametredir***. Gerekli bir parametre, *formal_parameter_list*bir isteğe bağlı parametreden sonra görünmeyebilir.

`ref` veya `out` parametresinin *default_argument*olamaz. Bir *default_argument* *ifade* aşağıdakilerden biri olmalıdır:

*  bir *constant_expression*
*  formun bir ifadesi `new S()` `S` bir değer türüdür
*  formun bir ifadesi `default(S)` `S` bir değer türüdür

*İfade* , bir kimlikle örtük olarak dönüştürülebilir veya parametrenin türüne null olabilen dönüştürmeye sahip olmalıdır.

Bir uygulama kısmi yöntem bildiriminde ([kısmi Yöntemler](classes.md#partial-methods)) isteğe bağlı parametreler oluşursa, açık bir arabirim üye uygulaması ([Açık arabirim üyesi uygulamalar](interfaces.md#explicit-interface-member-implementations)) veya tek parametreli Dizin Oluşturucu bildiriminde ([Dizin oluşturucular](classes.md#indexers)), bu Üyeler asla bağımsız değişkenlerin atlanmasına izin vermek için hiçbir şekilde çağrılabileceğinden, derleyici bir uyarı vermelidir.

*Parameter_array* , isteğe bağlı bir *öznitelikler* kümesinden ([öznitelikler](attributes.md)), bir `params` değiştiricisinden, bir *array_type*ve bir *tanımlayıcıdan*oluşur. Bir parametre dizisi verilen bir ada sahip belirtilen dizi türünün tek bir parametresini bildirir. Bir parametre dizisinin *array_type* , tek boyutlu bir dizi türü ([dizi türleri](arrays.md#array-types)) olmalıdır. Bir yöntem çağrısında, bir parametre dizisi verilen dizi türünün tek bir bağımsız değişkeninin belirtilmesine izin verir veya dizi öğe türünde sıfır veya daha fazla bağımsız değişken belirtilmesine izin verir. Parametre dizileri, [parametre dizileri](classes.md#parameter-arrays)içinde daha ayrıntılı olarak açıklanmıştır.

Bir *parameter_array* isteğe bağlı bir parametreden sonra gerçekleşebilir, ancak varsayılan bir değere sahip olamaz. bunun yerine *parameter_array* bağımsız değişkenlerin atlanmasından sonra boş bir dizi oluşturulmasına neden olur.

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

`M`için *formal_parameter_list* , `i` gerekli bir başvuru parametresidir, `d` gerekli bir değer parametresi, `b`, `s`, `o` ve `t` ise isteğe bağlı değer parametreleridir ve `a` bir parametre dizisidir.

Yöntem bildirimi parametreler, tür parametreleri ve yerel değişkenler için ayrı bir bildirim alanı oluşturur. Adlar, bu bildirim alanına tür parametresi listesi ve yöntemin biçimsel parametre listesi ve yöntemin *bloğundaki* yerel değişken bildirimleri tarafından tanıtılmıştır. Bir yöntem bildirim alanının iki üyesinin aynı ada sahip olması için bir hatadır. Aynı ada sahip öğeleri içermesi için bir iç içe geçmiş bildirim alanının yöntem bildirim alanı ve yerel değişken bildirim alanı için bir hatadır.

Yöntem çağırma ([Yöntem etkinleştirmeleri](expressions.md#method-invocations)), bu çağrıya özgü bir kopya oluşturur; Bu, biçimsel parametreler ve yöntemin yerel değişkenleri ve çağrının bağımsız değişken listesi, yeni oluşturulan biçimsel parametrelere değerler veya değişken başvuruları atar. Bir yöntem *bloğunun* içinde, biçimsel parametrelere *simple_name* ifadelerinde tanımlayıcıları ([basit adlar](expressions.md#simple-names)) tarafından başvurulabilir.

Dört tür biçimsel parametre vardır:

*  Herhangi bir değiştirici olmadan bildirildiği değer parametreleri.
*  `ref` değiştiricisiyle belirtilen başvuru parametreleri.
*  `out` değiştiricisiyle belirtilen çıkış parametreleri.
*  `params` değiştiricisiyle belirtilen parametre dizileri.

[İmzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)' de açıklandığı gibi, `ref` ve `out` değiştiricileri yöntemin imzasının bir parçasıdır, ancak `params` değiştiricisi değildir.

#### <a name="value-parameters"></a>Değer parametreleri

Değiştirici olmadan belirtilen bir parametre bir değer parametresidir. Değer parametresi, yöntem çağrısında sağlanan karşılık gelen bağımsız değişkenden ilk değerini alan yerel bir değişkene karşılık gelir.

Bir biçimsel parametre bir değer parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre türüne örtük olarak dönüştürülebilir ([örtük dönüştürmeler](conversions.md#implicit-conversions)) bir ifade olmalıdır.

Bir yöntem bir değer parametresine yeni değerler atamaya izin verilir. Bu atamalar yalnızca değer parametresi tarafından temsil edilen yerel depolama konumunu etkiler — Yöntem çağrısında verilen gerçek bağımsız değişkeni üzerinde hiçbir etkisi yoktur.

#### <a name="reference-parameters"></a>Başvuru parametreleri

`ref` değiştiricisi ile belirtilen bir parametre bir başvuru parametresidir. Değer parametresinden farklı olarak, başvuru parametresi yeni bir depolama konumu oluşturmaz. Bunun yerine, başvuru parametresi, yöntem çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.

Bir biçimsel parametre bir başvuru parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre ile aynı türdeki bir *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) içermelidir `ref`. Bir değişken başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı.

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
```console
i = 2, j = 1
```

`Main``Swap` çağrılması için `x` `i` temsil eder ve `y` `j`temsil eder. Bu nedenle, çağrının `i` ve `j`değerlerini değiştirme etkisi vardır.

Başvuru parametreleri alan bir yöntemde, birden fazla ad aynı depolama konumunu temsil etmek mümkündür. örnekte
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
`G` `F` çağrısı, `a` ve `b`için `s` başvurusunu geçirir. Bu nedenle, bu çağrı için `s`, `a`ve `b` adları aynı depolama konumuna başvurur ve bu üç atama, örnek alanını `s`değiştirir.

#### <a name="output-parameters"></a>Çıktı parametreleri

`out` değiştiricisi ile belirtilen bir parametre bir çıkış parametresidir. Bir başvuru parametresine benzer şekilde, çıkış parametresi yeni bir depolama konumu oluşturmaz. Bunun yerine, bir çıktı parametresi, yöntem çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder.

Bir biçimsel parametre bir çıkış parametresi olduğunda, bir yöntem çağrısında karşılık gelen bağımsız değişken, biçimsel parametre ile aynı türdeki bir *variable_reference* ([kesin atamayı belirlemek için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) içermelidir `out`. Bir değişken, çıkış parametresi olarak geçirilebilmesi için kesinlikle atanmamalıdır, ancak bir değişkenin çıkış parametresi olarak geçirildiği bir çağrıdan sonra, değişken kesinlikle atanmış olarak değerlendirilir.

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
```console
c:\Windows\System\
hello.txt
```

`dir` ve `name` değişkenlerinin `SplitPath`geçirilmeden önce atanmadığını ve çağrının ardından kesinlikle atanmış olarak kabul edileceğini unutmayın.

#### <a name="parameter-arrays"></a>Parametre dizileri

`params` değiştiricisi ile belirtilen bir parametre bir parametre dizisidir. Bir biçimsel parametre listesi bir parametre dizisi içeriyorsa, listedeki son parametre olmalıdır ve tek boyutlu dizi türünde olmalıdır. Örneğin, `string[]` ve `string[][]` türleri bir parametre dizisinin türü olarak kullanılabilir, ancak tür `string[,]` olamaz. `params` değiştiricinin değiştiriciler `ref` ve `out`birleştirmek mümkün değildir.

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
```console
Array contains 3 elements: 1 2 3
Array contains 4 elements: 10 20 30 40
Array contains 0 elements:
```

İlk `F` çağrısı, dizi `a` bir değer parametresi olarak geçirir. `F` ikinci çağırma, otomatik olarak verilen öğe değerleriyle dört öğeli bir `int[]` oluşturur ve bu dizi örneğini bir değer parametresi olarak geçirir. Benzer şekilde, üçüncü `F` çağrısı sıfır öğeli bir `int[]` oluşturur ve bu örneği bir değer parametresi olarak geçirir. İkinci ve üçüncü etkinleştirmeleri yazma ile tam olarak eşdeğerdir:
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
```console
F();
F(object[]);
F(object,object);
F(object[]);
F(object[]);
```

Örnekte, bir parametre dizisi ile yönteminin mümkün olan genişletilmiş biçimlerinden ikisi, normal yöntemler olarak sınıfında zaten yer alır. Bu genişletilmiş formlar bu nedenle aşırı yükleme çözümlemesi gerçekleştirilirken değerlendirilmez ve ilk ve üçüncü yöntem etkinleştirmeleri bu nedenle normal yöntemleri seçer. Bir sınıf bir parametre dizisi olan bir yöntem bildiriyorsa, genişletilmiş formlardan bazılarını düzenli yöntemler olarak da içermesi yaygın olmayan bir durumdur. Bunu yaparak, parametre dizisi olan bir yöntemin genişletilmiş formu çağrıldığında oluşan bir dizi örneğinin ayrılmasını önlemek mümkündür.

Bir parametre dizisinin türü `object[]`olduğunda, yöntemin normal biçimi ve tek bir `object` parametresi için expsona formu arasında olası bir belirsizlik oluşur. Belirsizliğin nedeni, `object[]` `object`türüne örtülü olarak dönüştürülebilir bir. Bununla birlikte, belirsizlik bir sorun değildir, ancak gerekirse bir atama eklenebilir.

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
```console
System.Int32 System.String System.Double
System.Object[]
System.Object[]
System.Int32 System.String System.Double
```

`F`ilk ve son çağırma sürümünde, bağımsız değişken türünden parametre türüne örtük bir dönüştürme mevcut olduğu için `F` normal biçimi geçerlidir (her ikisi de `object[]`türündedir). Bu nedenle, aşırı yükleme çözümlemesi normal `F`ve bağımsız değişken normal bir değer parametresi olarak geçirilir. İkinci ve üçüncü etkinleştirmeleri içinde, bağımsız değişken türünden parametre türüne örtük dönüştürme olmadığından `F` normal biçimi Uygulanamaz (tür `object` örtük olarak `object[]`türüne dönüştürülemez). Ancak, `F` genişletilmiş biçimi uygulanabilir, bu nedenle aşırı yükleme çözümlemesi tarafından seçilir. Sonuç olarak, tek öğeli bir `object[]`, çağrı tarafından oluşturulur ve dizideki tek öğe, verilen bağımsız değişken değeri (kendisi bir `object[]`başvurusu olan) ile başlatılır.

### <a name="static-and-instance-methods"></a>Statik ve örnek yöntemleri

Bir yöntem bildirimi `static` değiştirici içerdiğinde, bu yöntem statik bir yöntem olarak kabul edilir. `static` değiştirici yoksa, yöntem bir örnek yöntemi olarak kabul edilir.

Statik bir yöntem belirli bir örnek üzerinde çalışmaz ve statik bir yöntemde `this` başvurmak için derleme zamanı hatasıdır.

Bir örnek yöntemi bir sınıfın belirli bir örneği üzerinde çalışır ve bu örneğe `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.

Bir yönteme bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir yöntem ise, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek yöntemi ise, `E` içeren bir türün örneğini belirtmelidir.`M`

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="virtual-methods"></a>Sanal yöntemler

Bir örnek yöntemi bildirimi `virtual` değiştiricisi içerdiğinde, bu yöntem sanal bir yöntem olarak kabul edilir. `virtual` değiştirici yoksa, yöntemi sanal olmayan bir yöntem olarak kabul edilir.

Sanal olmayan bir yöntemin uygulanması değişmez: uygulamanın bildirildiği sınıfın bir örneğinde veya türetilmiş bir sınıfın örneği üzerinde çağrılmasından bağımsız olarak, uygulama aynı olur. Buna karşılık, sanal bir yöntemin uygulanması türetilmiş sınıflar tarafından değiştirilmiş olabilir. Devralınan bir sanal yöntemin uygulanmasını yerine geçen işlem, bu yöntemi ***geçersiz kılma*** ([geçersiz kılma yöntemleri](classes.md#override-methods)) olarak bilinir.

Bir sanal yöntem çağrısında, çağrının gerçekleştiği örneğin ***çalışma zamanı türü*** , çağrılacak gerçek Yöntem uygulamasını belirler. Sanal olmayan bir yöntem çağrısında, örneğin ***derleme zamanı türü*** belirleme faktörü olur. Kesin koşullarda `N` adlı bir yöntem, derleme zamanı türü `C` ve bir çalışma zamanı türü `R` olan bir örnek `A` bağımsız değişken listesi ile çağrıldığında (`R` `C` veya `C`sınıfından türetilen bir sınıf), çağrı aşağıdaki şekilde işlenir:

*  İlk olarak, `C`, `N`ve `A`için aşırı yükleme çözümlemesi uygulanır ve `C`tarafından tanımlanan Yöntemler kümesinden `M` belirli bir yöntemi seçebilir. Bu, [Yöntem etkinleştirmeleri](expressions.md#method-invocations)bölümünde açıklanmaktadır.
*  `M` sanal olmayan bir yöntem ise `M` çağrılır.
*  Aksi takdirde, `M` sanal bir yöntemdir ve `R` göre `M` en çok türetilen uygulama çağrılır.

Bir sınıf tarafından tanımlanan veya devralan her sanal yöntem için, yönteminin bu sınıfa göre ***en çok türetilmiş bir uygulamasını*** vardır. Bir sanal yöntemin en fazla türetilmiş uygulamasının bir sınıfa göre `M` `R` aşağıdaki gibi belirlenir:

*  `R`, `M`tanıtma `virtual` bildirimini içeriyorsa, bu `M`en çok türetilen uygulamasıdır.
*  Aksi takdirde, `R` bir `M``override` içeriyorsa, bu `M`en çok türetilen uygulamasıdır.
*  Aksi takdirde, `R` göre `M` en çok türetilen uygulama, `R`doğrudan taban sınıfına göre `M` en çok türetilen uygulamayla aynıdır.

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

Örnekte, `A` sanal olmayan bir yöntem `F` ve sanal bir yöntem `G`tanıtır. Sınıfı `B`, yeni bir sanal olmayan yöntem `F`tanıtır, bu nedenle devralınan `F`gizler ve ayrıca devralınan yöntem `G`geçersiz kılar. Örnek, çıktıyı üretir:
```console
A.F
B.F
B.G
B.G
```

Deyimin `A.G`değil `B.G``a.G()` çağırdığına dikkat edin. Bunun nedeni, örneğin çalışma zamanı türünün (yani `B`), örneğin derleme zamanı türü (`A`) değil, çağrılacak gerçek Yöntem uygulamasını belirler.

Yöntemlerin devralınan yöntemleri gizleyebildiğinden, bir sınıfın aynı imzaya sahip çeşitli sanal yöntemler içermesi mümkündür. Bu, bir belirsizlik sorunu sunmaz, ancak en çok türetilen Yöntem gizlenir. örnekte
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
`C` ve `D` sınıfları aynı imzaya sahip iki sanal yöntem içerir: `A` ve `C`tarafından tanıtılan bir. `C` tarafından tanıtılan Yöntem, `A`devralınmış yöntemi gizler. Bu nedenle `D` geçersiz kılma bildirimi, `C`tarafından tanıtılan yöntemi geçersiz kılar ve `D` `A`tarafından tanıtılan yöntemi geçersiz kılmak mümkün değildir. Örnek, çıktıyı üretir:
```console
B.F
B.F
D.F
D.F
```

Bir `D` örneğine, yöntemin gizlenmediği daha az türetilmiş bir tür aracılığıyla erişerek gizli sanal yöntemi çağırmak mümkün olduğunu unutmayın.

### <a name="override-methods"></a>Geçersiz kılma yöntemleri

Bir örnek yöntemi bildirimi `override` değiştiricisi içerdiğinde, yöntemi bir ***geçersiz kılma yöntemi***olarak kabul edilir. Bir geçersiz kılma yöntemi, aynı imzaya sahip devralınmış bir sanal yöntemi geçersiz kılar. Sanal bir yöntem bildiriminde yeni bir yöntem tanıtıldığı halde, bir geçersiz kılma yöntemi bildirimi, bu yöntemin yeni bir uygulamasını sağlayarak, var olan bir devralınmış sanal yöntemi uzmanlık eder.

Bir `override` bildirimi tarafından geçersiz kılınan yöntem, ***geçersiz kılınan temel yöntem***olarak bilinir. Bir sınıf `C`bildirildiği `M` geçersiz kılma yöntemi için, geçersiz kılınan taban yöntemi, `C` doğrudan temel sınıf türünden başlayıp, her bir ardışık doğrudan temel sınıf türüne devam eden her bir temel sınıf türü `C`inceleyerek belirlenir. Bu, belirli bir temel sınıf türüne göre aynı imzaya sahip olan en az bir erişilebilir Yöntem bulunduğundan, tür bağımsız değişkenlerinin yerine `M` aynı imzaya sahip olur. Geçersiz kılınan temel yöntemi bulma amaçları doğrultusunda, bir yöntem, `protected`, `protected internal`ise veya `internal` ve aynı programda `C`olarak bildirilirse `public`erişilebilir olarak değerlendirilir.

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

Geçersiz kılma bildirimi, geçersiz kılınan temel yönteme *base_access* ([taban erişimi](expressions.md#base-access)) ile erişebilir. örnekte
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
`B` `base.PrintFields()` çağrısı, `A`içinde belirtilen `PrintFields` yöntemini çağırır. *Base_access* sanal çağırma mekanizmasını devre dışı bırakır ve temel yöntemi sanal olmayan bir yöntem olarak değerlendirir. `B` çağrısı vardı `((A)this).PrintFields()`, `A`sanal olduğundan ve `PrintFields` çalışma zamanı türü `((A)this)` olduğundan, `B`içinde bildirildiği `PrintFields` yöntemini yinelemeli olarak çağırır.`B`

Yalnızca bir `override` değiştiricisi ekleyerek bir yöntem başka bir yöntemi geçersiz kılabilir. Diğer tüm durumlarda, devralınan bir yöntemle aynı imzaya sahip bir yöntem devralınan yöntemi gizler. örnekte
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
`B` `F` yöntemi bir `override` değiştiricisi içermez ve bu nedenle `A``F` yöntemini geçersiz kılmaz. Bunun yerine, `B` `F` yöntemi `A`yöntemi gizler ve bildirim bir `new` değiştiricisi içermediğinden bir uyarı bildirilir.

örnekte
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
`B` `F` yöntemi `A`devralınan sanal `F` yöntemini gizler. `B` içindeki yeni `F` özel erişime sahip olduğundan, kapsamı yalnızca `B` sınıf gövdesini içerir ve `C`olarak genişlemez. Bu nedenle, `C` `F` bildiriminin `A`devralınmış `F` geçersiz kılmasına izin verilir.

### <a name="sealed-methods"></a>Sealed yöntemleri

Bir örnek yöntemi bildirimi `sealed` değiştiricisi içerdiğinde, bu yöntem ***Sealed bir yöntem***olarak kabul edilir. Bir örnek yöntemi bildirimi `sealed` değiştiricisini içeriyorsa, `override` değiştiricisini de içermelidir. `sealed` değiştiricisinin kullanımı, türetilmiş bir sınıfın yöntemi daha fazla geçersiz kılmasını önler.

örnekte
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
`B` sınıfı iki geçersiz kılma yöntemi sağlar: `sealed` değiştiricisi ve olmayan bir `G` yöntemi olan bir `F` yöntemi. `B`Sealed `modifier` kullanımı, `C` `F`üzerine geçersiz kılmayı önler.

### <a name="abstract-methods"></a>Soyut yöntemler

Bir örnek yöntemi bildirimi `abstract` değiştiricisi içerdiğinde, bu yöntem ***soyut bir yöntem***olarak kabul edilir. Soyut bir yöntem örtülü olarak bir sanal yöntem olsa da, `virtual`değiştiriciye sahip olamaz.

Soyut yöntem bildirimi yeni bir sanal yöntem tanıtır, ancak bu yöntemin bir uygulamasını sağlamaz. Bunun yerine, soyut olmayan türetilmiş sınıfların bu yöntemi geçersiz kılarak kendi uygulamasını sağlaması gerekir. Soyut bir yöntem gerçek uygulama sunmadığından, soyut bir yöntemin *method_body* noktalı virgülden oluşur.

Soyut yöntem bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)).

örnekte
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
`Shape` sınıfı, kendisini boyayacak bir geometrik şekil nesnesinin soyut kavramını tanımlar. Anlamlı bir varsayılan uygulama olmadığından `Paint` yöntemi soyuttur. `Ellipse` ve `Box` sınıfları somut `Shape` uygulamalarıdır. Bu sınıflar soyut olmadığından, `Paint` yönteminin geçersiz kılınması ve gerçek bir uygulama sağlaması gerekir.

Bir soyut metoda başvurmak için bir *base_access* ([taban erişimi](expressions.md#base-access)) için derleme zamanı hatası. örnekte
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
bir soyut metoda başvurduğundan `base.F()` çağrısı için derleme zamanı hatası bildirilir.

Bir soyut Yöntem bildiriminin sanal bir yöntemi geçersiz kılmasına izin verilir. Bu, bir soyut sınıfın türetilmiş sınıflarda yöntemin yeniden uygulanmasını zormasına olanak tanır ve yöntemin orijinal uygulamasını kullanılamaz hale getirir. örnekte
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

Bir yöntem bildirimi `extern` değiştiricisi içerdiğinde, bu yöntem bir ***dış yöntem***olarak kabul edilir. Dış Yöntemler, genellikle dışında C#bir dil kullanılarak dışarıdan uygulanır. Bir dış yöntem bildirimi gerçek uygulama sunmadığından, bir dış yöntemin *method_body* noktalı virgülden oluşur. Dış yöntem genel olmayabilir.

`extern` değiştirici, genellikle bir `DllImport` özniteliğiyle birlikte kullanılır ([com ve Win32 bileşenleriyle birlikte çalışabilirlik](attributes.md#interoperation-with-com-and-win32-components)) ve dış yöntemlerin dll 'ler tarafından uygulanmasına izin verir (dinamik bağlantı kitaplıkları). Yürütme ortamı, dış yöntemlerin uygulamalarının sağlanabildiği diğer mekanizmaları destekleyebilir.

Bir dış yöntem `DllImport` özniteliği içerdiğinde, yöntem bildirimi de bir `static` değiştiricisi içermelidir. Bu örnek `extern` değiştiricisi ve `DllImport` özniteliği kullanımını gösterir:
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

Bir yöntem bildirimi `partial` değiştirici içerdiğinde, bu yöntem ***kısmi bir yöntem***olarak kabul edilir. Kısmi yöntemler yalnızca kısmi türlerin ([kısmi türlerin](classes.md#partial-types)) üyeleri olarak bildirilebilecek ve bir dizi kısıtlamayla tabidir. Kısmi Yöntemler, [kısmi yöntemlerde](classes.md#partial-methods)daha ayrıntılı olarak açıklanmıştır.

### <a name="extension-methods"></a>Uzantı yöntemleri

Bir yöntemin ilk parametresi `this` değiştiricisini içerdiğinde, bu yöntem bir ***genişletme yöntemi***olarak kabul edilir. Uzantı yöntemleri yalnızca genel olmayan, iç içe olmayan statik sınıflarda bildirilemez. Bir genişletme yönteminin ilk parametresi, `this`dışında bir değiştirici içeremez ve parametre türü bir işaretçi türü olamaz.

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

`Slice` yöntemi `string[]`kullanılabilir ve `ToInt32` yöntemi uzantı yöntemleri olarak bildirildiği için `string`üzerinde kullanılabilir. Programın anlamı, sıradan statik yöntem çağrıları kullanılarak aşağıdakiler ile aynıdır:
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

Yöntem bildiriminin *method_body* bir blok gövdesinden, bir ifade gövdesinden veya noktalı virgülden oluşur.

Bir yöntemin ***sonuç türü*** , dönüş türü `void`ise veya yöntem zaman uyumsuz ise ve dönüş türü `System.Threading.Tasks.Task`ise `void`. Aksi takdirde, zaman uyumsuz bir yöntemin sonuç türü dönüş türüdür ve dönüş türü `System.Threading.Tasks.Task<T>` zaman uyumsuz bir metodun sonuç türü `T`.

Bir yöntemde `void` sonuç türü ve bir blok gövdesi olduğunda, bloktaki `return` deyimlerinin ([return deyimi](statements.md#the-return-statement)) bir ifade belirtmelerine izin verilmez. Void yönteminin bloğunun yürütülmesi normal şekilde tamamlanırsa (diğer bir deyişle, bu yöntem, Yöntem gövdesinin sonundaki şekilde akar), bu yöntem yalnızca geçerli çağıranına döner.
    
Bir yöntemde `void` sonucu ve bir ifade gövdesi olduğunda, `E` ifadesi bir *statement_expression*olmalıdır ve gövde `{ E; }`bir blok gövdesiyle tam olarak eşdeğerdir.
    
Bir yöntemde void olmayan bir sonuç türü ve bir blok gövdesi olduğunda, bloktaki her `return` deyimi, sonuç türüne örtük olarak dönüştürülebilir bir ifade belirtmelidir. Değer döndüren metodun blok gövdesinin uç noktasına ulaşılamıyor olmalıdır. Diğer bir deyişle, blok gövdesi olan bir değer döndüren yöntemde, denetimin Yöntem gövdesinin sonunu akışa girmesine izin verilmez.
    
Bir yöntemde void olmayan bir sonuç türü ve bir ifade gövdesi olduğunda, ifadenin sonuç türüne örtülü olarak dönüştürülebilir olması gerekir ve gövde `{ return E; }`bir blok gövdesiyle tam olarak eşdeğerdir.
    
örnekte
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
değer döndüren `F` yöntemi derleme zamanı hatası ile sonuçlanır çünkü denetim Yöntem gövdesinin sonuna akabilir. `G` ve `H` yöntemleri doğru olduğundan, tüm olası yürütme yolları bir dönüş değeri belirten dönüş ifadesinde sona erdir. `I` yöntemi doğrudur, çünkü gövdesi yalnızca tek bir dönüş ifadesiyle bir ifade bloğuna denk gelir.

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

Bir *property_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)) geçerli bir birleşimini, `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı](classes.md#sealed-methods)Yöntemler), `abstract` ([soyut yöntemler](classes.md#abstract-methods)) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerini içerebilir.

Özellik bildirimleri, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([Yöntemler](classes.md#methods)) ile aynı kurallara tabidir.

Bir özellik bildiriminin *türü* , bildirim tarafından tanıtılan özelliğin türünü belirtir ve *MEMBER_NAME* özelliğin adını belirtir. Özellik açık bir arabirim üyesi uygulama değilse, *MEMBER_NAME* yalnızca bir *tanıtıcıdır*. Açık arabirim üye uygulaması ([Açık arabirim üye uygulamaları](interfaces.md#explicit-interface-member-implementations)) için *member_name* , bir *interface_type* ve ardından bir "`.`" ve bir *tanımlayıcı*oluşur.

Özelliğin *türü* en az özelliğin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir *property_body* ***erişimci gövdesinden*** ya da bir ***ifade gövdesinden***oluşabilir. Bir erişimci gövdesinde, "`{`" ve "`}`" belirteçlerinin içine alınması gereken *accessor_declarations*, özelliğin erişimcileri ([erişimcileri](classes.md#accessors)) ' sini bildirin. Erişimciler, özelliği okuma ve yazma ile ilişkili yürütülebilir deyimleri belirler.

`=>` içeren bir ifade gövdesi ve ardından bir *ifade* `E` ve noktalı virgül, `{ get { return E; } }`deyim gövdesi ile tam olarak eşdeğerdir ve bu nedenle yalnızca alıcı sonucunun tek bir ifade tarafından verildiği yerde yalnızca alıcı özelliklerini belirtmek için kullanılabilir.

Bir *property_initializer* yalnızca otomatik olarak uygulanan bir Özellik ([otomatik olarak uygulanan özellikler](classes.md#automatically-implemented-properties)) için verilebilir ve bu tür özelliklerin temel alan, *ifade*tarafından verilen değerle başlatılmasına neden olur.

Bir özelliğe erişim sözdizimi, bir alanla ilgili olarak aynı olsa da, bir özellik değişken olarak sınıflandırılmıyor. Bu nedenle, bir özelliği `ref` veya `out` bağımsız değişken olarak geçirmek mümkün değildir.

Bir özellik bildirimi `extern` değiştirici içerdiğinde, özelliği bir ***dış Özellik***olarak kabul edilir. Dış özellik bildirimi gerçek uygulama sağladığından, *accessor_declarations* her biri noktalı virgülle oluşur.

### <a name="static-and-instance-properties"></a>Statik ve örnek özellikleri

Bir özellik bildirimi `static` değiştirici içerdiğinde, özelliği ***statik bir özellik***olarak kabul edilir. `static` değiştirici yoksa, özelliği bir ***örnek özelliği***olarak kabul edilir.

Statik bir özellik belirli bir örnekle ilişkili değildir ve statik bir özelliğin erişimcilerine `this` başvurmak için bir derleme zamanı hatasıdır.

Örnek özelliği bir sınıfın belirli bir örneğiyle ilişkilendirilir ve bu örneğe bu özelliğin erişimcilerinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.

Bir özelliğe bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir özellik ise, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek özellik ise, E içeren bir türün örneğini belirtmelidir.`M`

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="accessors"></a>C

Bir özelliğin *accessor_declarations* , bu özelliği okuma ve yazma ile ilişkili çalıştırılabilir deyimleri belirtir.

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

Erişimci bildirimleri *get_accessor_declaration*, *set_accessor_declaration*veya her ikisinden oluşur. Her erişimci bildirimi `get` belirteç `set` ve ardından isteğe bağlı *accessor_modifier* ve bir *accessor_body*oluşur.

*Accessor_modifier*s kullanımı aşağıdaki kısıtlamalara tabidir:

*  Bir *accessor_modifier* , bir arabirimde veya açık arabirim üyesi uygulamasında kullanılamaz.
*  `override` değiştiriciye sahip olmayan bir özellik veya Dizin Oluşturucu için *accessor_modifier* , yalnızca özellik veya dizin oluşturucunun hem `get` hem de `set` erişimcisine sahip olması ve bu erişimcilerle yalnızca biri için izin verilir.
*  `override` değiştiricisi içeren bir özellik veya Dizin Oluşturucu için, erişimci geçersiz kılınmakta olan erişimcinin *accessor_modifier*eşleşmesi gerekir.
*  *Accessor_modifier* , özelliğin veya dizin oluşturucunun kendisi tarafından belirtilen erişilebilirliğine göre kesinlikle daha kısıtlayıcı bir erişilebilirlik bildirmelidir. Kesin olması için:
   * Özelliğin veya dizin oluşturucunun `public`tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* `protected internal`, `internal`, `protected`veya `private`olabilir.
   * Özelliğin veya dizin oluşturucunun `protected internal`tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* `internal`, `protected`veya `private`olabilir.
   * Özelliğin veya dizin oluşturucunun `internal` veya `protected`tanımlanmış bir erişilebilirliği varsa, *accessor_modifier* `private`olmalıdır.
   * Özelliğin veya dizin oluşturucunun `private`tanımlanmış bir erişilebilirliği varsa *accessor_modifier* kullanılamaz.

`abstract` ve `extern` özellikleri için, belirtilen her erişimci için *accessor_body* yalnızca noktalı virgül olur. Soyut olmayan, extern olmayan bir özelliğin her bir *accessor_body* noktalı virgül olması olabilir, bu durumda ***otomatik olarak uygulanan bir özelliktir*** ([Otomatik uygulanan özellikler](classes.md#automatically-implemented-properties)). Otomatik olarak uygulanan özelliğin en az bir get erişimcisi olmalıdır. Diğer soyut olmayan, extern olmayan özelliğin erişimcileri için *accessor_body* , karşılık gelen erişimci çağrıldığında yürütülecek deyimleri belirten bir *bloğudur* .

`get` erişimcisi, özellik türünün dönüş değeri olan parametresiz bir yönteme karşılık gelir. Bir atamanın hedefi haricinde, bir ifadede bir özelliğe başvurulduğunda, özelliğin `get` erişimcisi özelliğin değerini hesaplamak için çağrılır ([Ifadelerin değerleri](expressions.md#values-of-expressions)). `get` erişimcisinin gövdesi, [Yöntem gövdesinde](classes.md#method-body)açıklanan değer döndüren yöntemlere yönelik kurallara uymalıdır. Özellikle, bir `get` erişimcisinin gövdesindeki tüm `return` deyimleri, özellik türüne örtük olarak dönüştürülebilir bir ifade belirtmelidir. Ayrıca, bir `get` erişimcisinin uç noktasına ulaşılamamalıdır.

`set` erişimcisi, özellik türünün tek değerli parametresine sahip bir yönteme ve `void` dönüş türüne karşılık gelir. `set` erişimcisinin örtük parametresi her zaman `value`olarak adlandırılır. Bir atamanın hedefi olarak bir özelliğe başvurulduğunda ([atama işleçleri](expressions.md#assignment-operators)), ya da `++` ya da `--` Işleneni ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [ön ek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) olarak, `set` erişimci bir bağımsız değişkenle çağrılır (değeri atamanın sağ tarafındaki veya `++` veya `--` işlecinin Işleneni olan) yeni değeri ([basit atama](expressions.md#simple-assignment)) sağlar. `set` erişimcisinin gövdesi, [Yöntem gövdesinde](classes.md#method-body)açıklanan `void` yöntemleri için kurallara uymalıdır. Özellikle, `set` erişimci gövdesindeki `return` deyimlerinin bir ifade belirtmelerine izin verilmez. `set` erişimcisinde örtük olarak `value`adlı bir parametre bulunduğundan, bu ada sahip olması için bir `set` erişimcisindeki bir yerel değişken veya sabit bildirimin derleme zamanı hatası olur.

`get` ve `set` erişimcilerinin varlığına veya yokluğuna göre, bir özellik şu şekilde sınıflandırılır:

*  Hem bir `get` erişimcisi hem de bir `set` erişimcisi içeren bir özellik, ***okuma-yazma*** özelliği olarak kabul edilir.
*  Yalnızca bir `get` erişimcisi olan bir özellik ***salt okunurdur*** özelliği olarak kabul edilir. Bir salt okuma özelliğinin bir atamanın hedefi olması için derleme zamanı hatası.
*  Yalnızca bir `set` erişimcisi olan bir özellik ***salt yazılır*** bir özellik olarak kabul edilir. Atama hedefi dışında, bir ifadede salt yazılır bir özelliğe başvurmak için derleme zamanı hatası olur.

örnekte
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
`Button` denetimi bir ortak `Caption` özelliği bildirir. `Caption` özelliğinin `get` erişimcisi, özel `caption` alanında depolanan dizeyi döndürür. `set` erişimci, yeni değerin geçerli değerden farklı olup olmadığını denetler ve bu durumda yeni değeri depolar ve denetimi yeniden boyar. Özellikler genellikle yukarıda gösterilen kalıbı izler: `get` erişimci bir özel alanda depolanan bir değeri döndürür ve `set` erişimci bu özel alanı değiştirir ve ardından nesnenin durumunu tamamen güncelleştirmek için gereken ek eylemleri gerçekleştirir.

Yukarıdaki `Button` sınıfı verildiğinde, `Caption` özelliğinin kullanım örneği aşağıda verilmiştir:
```csharp
Button okButton = new Button();
okButton.Caption = "OK";            // Invokes set accessor
string s = okButton.Caption;        // Invokes get accessor
```

Burada, `set` erişimcisi özelliğe bir değer atanarak çağrılır ve `get` erişimci bir ifadede özelliğe başvuruda bulunarak çağrılır.

Bir özelliğin `get` ve `set` erişimcileri farklı Üyeler değildir ve bir özelliğin erişimcilerinin ayrı ayrı bildirilmesini mümkün değildir. Bu nedenle, bir okuma-yazma özelliğinin iki erişimcinin farklı erişilebilirliği olması mümkün değildir. Örnek
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

Türetilmiş bir sınıf devralınan bir özellik ile aynı ada sahip bir özelliği bildiriyorsa, türetilen Özellik devralınan özelliği hem okuma hem de yazma açısından gizler. örnekte
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
`B` `P` özelliği, hem okuma hem de yazma açısından `A` `P` özelliğini gizler. Bu nedenle, deyimlerde
```csharp
B b = new B();
b.P = 1;          // Error, B.P is read-only
((A)b).P = 1;     // Ok, reference to A.P
```
`b.P` atama, `B` ' deki `P` salt yazılır `P` `A`özelliğini gizleyeceği için derleme zamanı hatası raporlanmasına neden olur. Ancak, bu tür bir dönüştürmenin gizli `P` özelliğine erişmek için kullanılabileceğini unutmayın.

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

Burada `Label` sınıfı, konumunu depolamak için `x` ve `y`iki `int` alanı kullanır. Konum, hem `X` hem de `Y` özellik olarak ve `Point`türünde `Location` özelliği olarak kullanıma sunulur. `Label`gelecek bir sürümünde, bu, konumun dahili olarak `Point` olarak depolanması daha uygun hale gelirse, değişikliğin sınıfın genel arabirimini etkilemeden bu değişiklik yapılabilir:
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

`x` ve `y` `public readonly` alan vardı, bu tür bir değişikliği `Label` sınıfında yapmak imkansız olurdu.

Durumu özellikler aracılığıyla göstermek, alanları doğrudan açığa çıkarmadan daha az verimlidir. Özellikle, bir özellik sanal olmayan ve yalnızca küçük miktarda kod içerdiğinde, yürütme ortamı erişimcileri çağrılarını erişimcilerinin gerçek koduyla değiştirebilir. Bu işlem, ***satır içi***olarak bilinir ve özellik erişimini alan erişimi olarak verimli hale getirir, ancak özelliklerin daha fazla esnekliğini korur.

Bir `get` erişimcisinin çağrılması bir alanın değerini okumanın kavramsal olarak eşdeğeri olduğundan, `get` erişimcilerinin observable yan etkileri olması için hatalı programlama stili olarak kabul edilir. örnekte
```csharp
class Counter
{
    private int next;

    public int Next {
        get { return next++; }
    }
}
```
`Next` özelliğinin değeri, özelliğin daha önce erişilme sayısına bağlıdır. Bu nedenle, özelliğe erişmek bir observable yan etkisi oluşturur ve özellik bunun yerine bir yöntem olarak uygulanmalıdır.

`get` erişimcileri için "yan etkileri yok" kuralı `get` erişimcilerin yalnızca alanlarda depolanan değerleri döndürmek için her zaman yazılması anlamına gelmez. Aslında `get` erişimcileri genellikle birden çok alana erişerek veya yöntemleri çağırarak bir özelliğin değerini hesaplar. Ancak düzgün şekilde tasarlanan bir `get` erişimcisi nesnenin durumunda observable değişikliklerine neden olan hiçbir eylem gerçekleştirmiyor.

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

`Console` sınıfı, sırasıyla standart giriş, çıkış ve hata cihazlarını temsil eden üç özellik, `In`, `Out`ve `Error`içerir. Bu üyelerin özellikler olarak kullanıma sunulmasıyla `Console` sınıfı, gerçekten kullanılana kadar başlatma durumlarını erteleyebilir. Örneğin, ilk olarak `Out` özelliğine başvurulduğunda
```csharp
Console.Out.WriteLine("hello, world");
```
çıkış cihazının temel alınan `TextWriter` oluşturulur. Ancak uygulama `In` ve `Error` özelliklerine başvuru yapmaz, bu durumda bu cihazlar için hiçbir nesne oluşturulmaz.

### <a name="automatically-implemented-properties"></a>Otomatik uygulanan özellikler

Otomatik olarak uygulanan bir Özellik (ya da Short için ***Otomatik Özellik*** ), salt noktalı erişimci gövdeleriyle soyut olmayan extern olmayan bir özelliktir. Otomatik Özellikler bir get erişimcisine sahip olmalı ve isteğe bağlı olarak bir set erişimcisine sahip olabilir.

Bir özellik otomatik olarak uygulanan bir özellik olarak belirtildiğinde, özelliği için otomatik olarak bir yedekleme alanı kullanılabilir ve erişimciler, bu yedekleme alanına okuma ve yazma için uygulanır. Auto özelliğinin ayarlanmış bir erişimcisi yoksa, yedekleme alanı `readonly` ([salt okunur alanlar](classes.md#readonly-fields)) olarak değerlendirilir. Yalnızca bir `readonly` alanı gibi, kapsayan sınıfın oluşturucusunun gövdesinde yalnızca bir alıcı otomatik özelliği de atanabilir. Böyle bir atama, doğrudan özelliğinin salt okunur yedekleme alanına atar.

Otomatik Özellik isteğe bağlı olarak, doğrudan bir *variable_initializer* ([değişken başlatıcıları](classes.md#variable-initializers)) olarak yedekleme alanına uygulanan bir *property_initializer*olabilir.

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

Bir erişimcinin *accessor_modifier*varsa, erişimcinin erişilebilirlik etki alanı ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) *accessor_modifier*belirtilen erişilebilirliği kullanılarak belirlenir. Bir erişimcinin *accessor_modifier*yoksa, erişimcinin erişilebilirlik etki alanı, özelliğin veya dizin oluşturucunun tanımlanmış erişilebilirliğine göre belirlenir.

*Accessor_modifier* varlığı, üye aramasını ([işleçler](expressions.md#operators)) veya aşırı yükleme çözümünü ([aşırı yükleme çözümlemesi](expressions.md#overload-resolution)) hiçbir şekilde etkilemez. Özellik veya dizin oluşturucudaki değiştiriciler her zaman erişim bağlamından bağımsız olarak hangi özelliğin veya dizin oluşturucunun bağlı olduğunu belirlenir.

Belirli bir özellik veya Dizin Oluşturucu seçildikten sonra, bu kullanımın geçerli olup olmadığını anlamak için ilgili erişimcilerinin erişilebilirlik etki alanları kullanılır:

*  Kullanım bir değer ([Ifadelerin değerleri](expressions.md#values-of-expressions)) ise, `get` erişimcisi bulunmalı ve erişilebilir olmalıdır.
*  Kullanım basit bir atamanın hedefi ([basit atama](expressions.md#simple-assignment)) ise, `set` erişimcisi bulunmalı ve erişilebilir olmalıdır.
*  Kullanım, bileşik atamanın hedefi ([bileşik atama](expressions.md#compound-assignment)) veya `++` ya da `--` işleçlerinin hedefi olarak ([işlev üyeleri](expressions.md#function-members).9, [çağırma ifadeleri](expressions.md#invocation-expressions)), hem `get` erişimcileri hem de `set` erişimcisi bulunmalı ve erişilebilir olmalıdır.

Aşağıdaki örnekte, özellik `A.Text`, yalnızca `set` erişimcisinin çağrıldığı bağlamlarda bile, özellik `B.Text`tarafından gizlenir. Buna karşılık, `B.Count` özelliği sınıf `M`erişemez, bu nedenle `A.Count` erişilebilir özellik kullanılır.

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

Arabirim uygulamak için kullanılan bir erişimcinin *accessor_modifier*olmayabilir. Bir arabirim uygulamak için yalnızca bir erişimci kullanılırsa, diğer erişimci bir *accessor_modifier*ile bildirilemeyebilir:
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

`virtual` özelliği bildirimi, özelliğin erişimcilerinin sanal olduğunu belirtir. `virtual` değiştiricisi her iki bir okuma-yazma özelliği erişimcisi için geçerlidir; okuma-yazma özelliğinin yalnızca bir erişimcisinin sanal olması mümkün değildir.

`abstract` özelliği bildirimi, özelliğin erişimcilerinin sanal olduğunu, ancak erişimcilerinin gerçek bir uygulamasını sağlamamayı belirtir. Bunun yerine, soyut olmayan türetilmiş sınıfların, özelliği geçersiz kılarak erişimcileri için kendi uygulamasını sağlaması gerekir. Soyut Özellik bildirimine yönelik bir erişimci gerçek uygulama sunmadığından, *accessor_body* yalnızca noktalı virgülden oluşur.

Hem `abstract` hem de `override` değiştiricilerini içeren bir özellik bildirimi, özelliğin soyut olduğunu ve bir temel özelliği geçersiz kıldığını belirtir. Böyle bir özelliğin erişimcileri de soyuttur.

Soyut Özellik bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)). Devralınan bir sanal özelliğin erişimcileri, bir `override` yönergesini belirten bir özellik bildirimi eklenerek, türetilmiş bir sınıfta geçersiz kılınabilir. Bu, ***geçersiz kılma özelliği bildirimi***olarak bilinir. Geçersiz kılan özellik bildirimi yeni bir özellik bildirmiyor. Bunun yerine, var olan bir sanal özelliğin erişimcilerinin uygulamalarını özelleştirir.

Geçersiz kılma özelliği bildirimi devralınan özellik olarak aynı erişilebilirlik değiştiricilerini, türü ve adı belirtmelidir. Devralınan özelliğin yalnızca tek bir erişimcisi varsa (yani devralınan özellik salt okunurdur veya salt yazılır ise), geçersiz kılma özelliği yalnızca o erişimciyi içermelidir. Devralınan özellik her iki erişimcileri de içeriyorsa (yani, devralınan Özellik okuma-yazma ise), geçersiz kılma özelliği tek bir erişimci veya her iki erişimci içerebilir.

Geçersiz kılan bir özellik bildirimi `sealed` değiştiriciyi içerebilir. Bu değiştiricinin kullanılması, türetilmiş bir sınıfın özelliği daha fazla geçersiz kılmasını önler. Sealed özelliğinin erişimcileri de korumalıdır.

Bildirim ve çağırma sözdiziminde farklar haricinde, sanal, korumalı, geçersiz kılma ve soyut erişimciler, sanal, mühürlenmiş, geçersiz kılma ve soyut yöntemler gibi davranır. Özellikle, [sanal yöntemlerde](classes.md#virtual-methods)açıklanan kurallar, [geçersiz kılma yöntemleri](classes.md#override-methods), [korumalı Yöntemler](classes.md#sealed-methods)ve [soyut yöntemler](classes.md#abstract-methods) , erişimciler karşılık gelen bir formun yöntemlerimiz gibi geçerlidir:

*  `get` erişimcisi, özellik türünün dönüş değeri ve kapsayan özelliği ile aynı değiştiriciler içeren parametresiz bir yönteme karşılık gelir.
*  `set` erişimcisi, özellik türünün tek değerli parametresine, `void` dönüş türüne ve kapsayan özelliği ile aynı değiştiricilere karşılık gelen bir yönteme karşılık gelir.

örnekte
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
`X` sanal bir salt okunurdur, `Y` sanal bir okuma-yazma özelliğidir ve `Z` soyut bir okuma-yazma özelliğidir. `Z` soyut olduğundan, kapsayan sınıf `A` de soyut olarak bildirilmelidir.

`A` türetilen bir sınıf aşağıda gösterilmektedir:
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

Burada, `X`, `Y`ve `Z` bildirimleri Özellik bildirimlerini geçersiz kılar. Her özellik bildirimi, karşılık gelen devralınmış özelliğin erişilebilirlik değiştiricilerini, türünü ve adını tam olarak eşleştirir. `X` `get` erişimcisi ve `Y` `set` erişimcisi, devralınan erişimcilere erişmek için `base` anahtar sözcüğünü kullanır. `Z` bildirimi her iki soyut erişimciyi geçersiz kılar. bu nedenle, `B`içinde bekleyen soyut işlev üyeleri yoktur ve `B` soyut olmayan bir sınıf olarak izin verilir.

Bir özellik `override`olarak bildirildiğinde, geçersiz kılınan erişimcilere geçersiz kılma kodu tarafından erişilebilmesi gerekir. Ayrıca, hem özelliğin hem de dizin oluşturucunun kendisi ve erişimcilerinin tanımlanmış erişilebilirliği, geçersiz kılınan üye ve erişimcilerinin ile eşleşmelidir. Örneğin:
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

Bir *event_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)) geçerli bir birleşimini, `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `static` ([statik ve örnek yöntemleri](classes.md#static-and-instance-methods)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı](classes.md#sealed-methods)Yöntemler), `abstract` ([soyut yöntemler](classes.md#abstract-methods)) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerini içerebilir.

Olay bildirimleri, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([yöntemleriyle](classes.md#methods)) ile aynı kurallara tabidir.

Olay bildiriminin *türü* bir *delegate_type* ([başvuru türleri](types.md#reference-types)) olmalıdır ve bu *delegate_type* en azından olayın kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir olay bildirimi *event_accessor_declarations*içerebilir. Ancak, dış olmayan, soyut olmayan olaylar için derleyici onları otomatik olarak ([alan benzeri olaylar](classes.md#field-like-events)) sağlar; extern olaylar için, erişimciler dışarıdan sağlanır.

*Event_accessor_declarations* atan bir olay bildirimi, bir veya daha fazla olayı tanımlar — her biri *variable_declarator*s. Öznitelikler ve değiştiriciler, bu tür bir *event_declaration*tarafından belirtilen tüm üyelere uygulanır.

*Event_declaration* , hem `abstract` değiştiricisini hem de küme ayracı ile ayrılmış *event_accessor_declarations*içermesi için derleme zamanı hatasıdır.

Bir olay bildirimi `extern` değiştirici içerdiğinde, olay bir ***dış olay***olarak kabul edilir. Dış bir olay bildirimi gerçek uygulama sağladığından, `extern` değiştiricisini ve *event_accessor_declarations*içermesi bir hatadır.

Bir `abstract` veya `external` *değiştiricvariable_initializer*isine sahip bir olay bildirimi *variable_declarator* için derleme zamanı hatasıdır.

Bir olay, `+=` ve `-=` işleçlerinin ([olay atama](expressions.md#event-assignment)) sol tarafı olarak kullanılabilir. Bu işleçler sırasıyla olay işleyicilerini bir olaydan kaldırmak ya da kaldırmak için kullanılır ve olayın erişim değiştiricileri, bu tür işlemlere izin verilen bağlamlara eklenir.

`+=` ve `-=`, olayı bildiren türden bir olayda izin verilen tek işlemlerdir, dış kod bir olay için işleyiciler ekleyebilir ve kaldırabilir, ancak başka hiçbir şekilde olay işleyicilerinin temel alınan listesini alabilir veya değiştirebilir.

`x += y` veya `x -= y`form işleminde, `x` bir olaysa ve başvuru, `x`bildirimini içeren türün dışında gerçekleşirken, işlemin sonucu tür `void` olur (`x`türüne sahip olarak, atamadan sonra `x` değeri ile). Bu kural, dış kodun bir olayın temel temsilcisini dolaylı olarak incelemeden yasaklar.

Aşağıdaki örnek, `Button` sınıfının örneklerine nasıl eklenen olay işleyicilerinin olduğunu gösterir:
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

Burada `LoginDialog` örneği Oluşturucusu iki `Button` örneği oluşturur ve olay işleyicilerini `Click` olaylarına ekler.

### <a name="field-like-events"></a>Alan benzeri olaylar

Bir olayın bildirimini içeren sınıfın veya yapının program metni içinde, bazı olaylar alan gibi kullanılabilir. Bu şekilde kullanılmak üzere, bir olay `abstract` veya `extern`olmamalı ve açıkça *event_accessor_declarations*içermemelidir. Bu tür bir olay, bir alana izin veren herhangi bir bağlamda kullanılabilir. Alan, olaya eklenmiş olan olay işleyicileri listesine başvuran bir temsilci ([Temsilciler](delegates.md)) içerir. Hiçbir olay işleyicisi eklenmemişse, alan `null`içerir.

örnekte
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
`Click`, `Button` sınıfında bir alan olarak kullanılır. Örneğin gösterdiği gibi, alan, temsilci çağırma ifadelerinde incelenebilir, değiştirilebilir ve kullanılabilir. `Button` sınıfındaki `OnClick` yöntemi `Click` olayını oluşturur. Bir olayı oluşturma kavramı, olayın gösterdiği temsilciyi çağırmak için tam olarak eşdeğerdir. bu nedenle, olayları yükseltmek için özel dil yapıları yoktur. Temsilci çağrısının önünde, temsilcinin null olmamasını sağlayan bir denetim olduğunu unutmayın.

`Button` sınıfının bildirimi dışında, `Click` üyesi yalnızca `+=` ve `-=` işleçlerinin sol tarafında kullanılabilir.
```csharp
b.Click += new EventHandler(...);
```
bir temsilciyi `Click` olayının çağırma listesine ekler ve
```csharp
b.Click -= new EventHandler(...);
```
`Click` olayının çağırma listesinden bir temsilciyi kaldıran.

Alan benzeri bir olay derlenirken, derleyici temsilciyi tutmak için otomatik olarak depolama oluşturur ve temsilci alanına olay işleyicileri ekleyen veya çıkarmayan olay için erişimciler oluşturur. Ekleme ve kaldırma işlemleri iş parçacığı açısından güvenlidir ve bir örnek olayı için kapsayan nesne üzerinde kilit ([kilit deyimi](statements.md#the-lock-statement)) veya statik bir olay için tür nesnesi ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)) tutulurken (ancak için gerekli değildir) yapılması gerekir.

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
`X`sınıfı içinde, `+=` ve `-=` işleçlerinin sol tarafındaki `Ev` başvuruları ekleme ve kaldırma erişimcilerinin çağrılmasına neden olur. `Ev` yapılan diğer tüm başvurular, bunun yerine `__Ev` gizli alana başvuracak şekilde derlenir ([üye erişimi](expressions.md#member-access)). "`__Ev`" adı rastgele; gizli alan herhangi bir ada veya hiç ada sahip olabilir.

### <a name="event-accessors"></a>Olay erişimcileri

Olay bildirimleri, yukarıdaki `Button` örnekte olduğu gibi genellikle *event_accessor_declarations*atlayın. Bunun için bir durum, her olay için bir alanın depolama maliyetinin kabul edilebilir olması durumunda oluşur. Böyle durumlarda, bir sınıf *event_accessor_declarations* içerebilir ve olay işleyicilerinin listesini depolamak için özel bir mekanizma kullanabilir.

Bir olayın *event_accessor_declarations* , olay işleyicilerini ekleme ve kaldırma ile ilişkili yürütülebilir deyimleri belirtir.

Erişimci bildirimleri bir *add_accessor_declaration* ve bir *remove_accessor_declaration*oluşur. Her erişimci bildirimi, belirteç `add` veya `remove` sonrasında bir *blok*tarafından oluşur. Bir *add_accessor_declaration* ilişkili *blok* , bir olay işleyicisi eklendiğinde yürütülecek deyimleri belirtir ve bir *remove_accessor_declaration* ilişkili *blok* , bir olay işleyicisi kaldırıldığında yürütülecek deyimleri belirler.

Her *add_accessor_declaration* ve *remove_accessor_declaration* , olay türünün tek değerli parametresine ve `void` dönüş türüne sahip bir yönteme karşılık gelir. Bir olay erişimcisinin örtük parametresi `value`olarak adlandırılır. Olay atamasında bir olay kullanıldığında, uygun olay erişimcisi kullanılır. Özellikle, atama işleci `+=`, Add erişimcisi kullanılır ve atama işleci `-=`, kaldırma erişimcisi kullanılır. Her iki durumda da, atama işlecinin sağ işleneni olay erişimcisinin bağımsız değişkeni olarak kullanılır. Bir *add_accessor_declaration* veya *Remove_accessor_declaration* bloğunun, [Yöntem gövdesinde](classes.md#method-body)açıklanan `void` yöntemlerine uygun kurallara uyması gerekir. Özellikle, bu tür bir bloktaki `return` deyimlerinin bir ifade belirtmelerine izin verilmez.

Bir olay erişimcisinin örtük olarak `value`adlı bir parametresi olduğundan, bu ada sahip olması için bir yerel değişken veya bir olay erişimcisinde belirtilen sabit için derleme zamanı hatası olur.

örnekte
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
`Control` sınıfı, olaylar için bir iç depolama mekanizması uygular. `AddEventHandler` yöntemi bir temsilci değerini bir anahtarla ilişkilendirir, `GetEventHandler` yöntemi şu anda bir anahtarla ilişkili olan temsilciyi döndürür ve `RemoveEventHandler` yöntemi belirtilen olay için bir temsilciyi olay işleyicisi olarak kaldırır. Temel alınan depolama mekanizması, bir `null` temsilci değerini bir anahtarla ilişkilendirme maliyeti olmaması ve bu nedenle işlenmemiş olayların depolama alanını tüketmesi gibi tasarlanmıştır.

### <a name="static-and-instance-events"></a>Statik ve örnek olayları

Bir olay bildirimi `static` değiştirici içerdiğinde, olay ***statik bir olay***olarak kabul edilir. `static` değiştirici yoksa, olay bir ***örnek olay***olarak kabul edilir.

Statik bir olay, belirli bir örnekle ilişkili değildir ve statik olay erişimcilerinde `this` başvurmak için derleme zamanı hatasıdır.

Örnek olay, bir sınıfın belirli bir örneğiyle ilişkilendirilir ve bu örneğe bu olayın erişimcilerinde `this` ([Bu erişim](expressions.md#this-access)) olarak erişilebilir.

Bir olaya bir *member_access* ([üye erişimi](expressions.md#member-access)) `E.M`, `M` statik bir olaydır, `E` `M`içeren bir türü belirtmelidir ve `M` bir örnek olayıdır, E 'nin `M`içeren bir türün örneğini belirtmek gerekir.

Statik ve örnek üyeleri arasındaki farklılıklar, [statik ve örnek üyelerinde](classes.md#static-and-instance-members)daha ayrıntılı bir şekilde ele alınmıştır.

### <a name="virtual-sealed-override-and-abstract-event-accessors"></a>Sanal, korumalı, geçersiz kılma ve soyut olay erişimcileri

`virtual` bir olay bildirimi, bu olayın erişimcilerinin sanal olduğunu belirtir. `virtual` değiştiricisi, her iki bir olayın erişimcisi için geçerlidir.

`abstract` bir olay bildirimi, olayın erişimcilerinin sanal olduğunu, ancak erişimcilerinin gerçek bir uygulamasını sağlamamayı belirtir. Bunun yerine, Özet olmayan türetilmiş sınıfların, olayı geçersiz kılarak erişimcileri için kendi uygulamasını sağlaması gerekir. Soyut bir olay bildirimi gerçek uygulama sağlamadığı için, küme ayracı ile ayrılmış *event_accessor_declarations*sağlayamaz.

Hem `abstract` hem de `override` değiştiricilerini içeren bir olay bildirimi, olayın soyut olduğunu ve bir temel olayı geçersiz kıldığını belirtir. Böyle bir olayın erişimcileri de soyuttur.

Soyut olay bildirimlerine yalnızca soyut sınıflarda izin verilir ([soyut sınıflar](classes.md#abstract-classes)).

Devralınan bir sanal olayın erişimcileri, bir `override` değiştiricisini belirten bir olay bildirimi eklenerek, türetilmiş bir sınıfta geçersiz kılınabilir. Bu, ***geçersiz kılan bir olay bildirimi***olarak bilinir. Geçersiz kılan bir olay bildirimi yeni bir olay bildirmiyor. Bunun yerine, var olan bir sanal olay erişimcilerinin uygulamalarını özelleştirir.

Geçersiz kılan bir olay bildirimi, geçersiz kılınan olayla aynı erişilebilirlik değiştiricilerini, türü ve adı belirtmelidir.

Geçersiz kılan bir olay bildirimi `sealed` değiştiriciyi içerebilir. Bu değiştiricinin kullanılması, türetilmiş bir sınıfın olayı daha fazla geçersiz kılmasını önler. Korumalı bir olayın erişimcileri de korumalıdır.

Geçersiz kılan bir olay bildiriminin `new` değiştiricisini içermesi için derleme zamanı hatasıdır.

Bildirim ve çağırma sözdiziminde farklar haricinde, sanal, korumalı, geçersiz kılma ve soyut erişimciler, sanal, mühürlenmiş, geçersiz kılma ve soyut yöntemler gibi davranır. Özellikle, [sanal yöntemlerde](classes.md#virtual-methods)açıklanan kurallar, [geçersiz kılma yöntemleri](classes.md#override-methods), [korumalı Yöntemler](classes.md#sealed-methods)ve [soyut yöntemler](classes.md#abstract-methods) , erişimciler karşılık gelen bir formun yöntemlerimiz gibi geçerlidir. Her erişimci, olay türünün tek değerli parametresine, `void` dönüş türüne ve kapsayan olayla aynı değiştiricilere karşılık gelen bir yönteme karşılık gelir.

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

Bir *indexer_declaration* , bir dizi *öznitelik* ([öznitelik](attributes.md)) ve dört erişim değiştiricisinin ([erişim değiştiricileri](classes.md#access-modifiers)), `new` ([Yeni değiştirici](classes.md#the-new-modifier)), `virtual` ([sanal yöntemler](classes.md#virtual-methods)), `override` ([geçersiz kılma yöntemleri](classes.md#override-methods)), `sealed` ([korumalı Yöntemler](classes.md#sealed-methods)), `abstract` ([soyut](classes.md#abstract-methods)Yöntemler) ve `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricilerinden geçerli bir birleşimini içerebilir.

Dizin Oluşturucu bildirimleri, bir özel durum, Dizin Oluşturucu bildiriminde statik değiştiriciye izin verilmemesine neden olacak şekilde, geçerli değiştiriciler birleşimleriyle ilgili olarak yöntem bildirimleri ([yöntemleri](classes.md#methods)) ile aynı kurallara tabidir.

`virtual`, `override`ve `abstract` değiştiriciler tek bir durum dışında birbirini dışlıyor. `abstract` ve `override` değiştiricileri birlikte kullanılabilir, böylece bir soyut dizin oluşturucunun sanal bir dizin oluşturucuyu geçersiz kılabilmesini sağlayabilirsiniz.

Bir Dizin Oluşturucu bildiriminin *türü* , bildirim tarafından tanıtılan dizin oluşturucunun öğe türünü belirtir. Dizin Oluşturucu açık arabirim üyesi uygulama değilse, *türün* sonuna anahtar sözcüğü `this`. Açık arabirim üyesi uygulama için, *türün* arkasından bir *interface_type*, "`.`" ve anahtar sözcüğü `this`gelir. Diğer üyelerin aksine, Dizin oluşturucular Kullanıcı tanımlı adlara sahip değildir.

*Formal_parameter_list* , dizin oluşturucunun parametrelerini belirtir. Bir dizin oluşturucunun biçimsel parametre listesi[bir yönteme karşılık](classes.md#method-parameters)gelir, ancak en az bir parametre belirtilmesi gerekir ve `ref` ve `out` parametre değiştiricilerine izin verilmez.

Bir dizin oluşturucunun *türü* ve *formal_parameter_list* başvurulan her bir tür, en azından dizin oluşturucunun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olmalıdır.

Bir *indexer_body* ***erişimci gövdesinden*** ya da bir ***ifade gövdesinden***oluşabilir. Bir erişimci gövdesinde, "`{`" ve "`}`" belirteçlerinin içine alınması gereken *accessor_declarations*, özelliğin erişimcileri ([erişimcileri](classes.md#accessors)) ' sini bildirin. Erişimciler, özelliği okuma ve yazma ile ilişkili yürütülebilir deyimleri belirler.

"`=>`" işaretinden sonra bir ifade `E` ve noktalı `{ get { return E; } }`virgülden sonra, yalnızca alıcı sonucunun tek bir ifade tarafından verildiği alıcı dizin oluşturucularının belirtilmesi için kullanılan bir ifade gövdesi.

Bir Dizin Oluşturucu öğesine erişmek için sözdizimi, bir dizi öğesiyle aynı olsa da, bir Dizin Oluşturucu öğesi değişken olarak sınıflandırılmaz. Bu nedenle, bir Dizin Oluşturucu öğesini bir `ref` veya `out` bağımsız değişken olarak geçirmek mümkün değildir.

Bir dizin oluşturucunun biçimsel parametre listesi, dizin oluşturucunun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar. Özellikle, bir dizin oluşturucunun imzası, resmi parametrelerinin sayısından ve türlerinden oluşur. Öğe türü ve biçimsel parametrelerinin adları, dizin oluşturucunun imzasının bir parçası değildir.

Bir dizin oluşturucunun imzası aynı sınıfta belirtilen diğer tüm dizin oluşturucularının imzalarından farklı olmalıdır.

Dizin oluşturucular ve Özellikler kavram bakımından çok benzerdir, ancak aşağıdaki yollarla farklılık gösterir:

*  Bir özellik adıyla tanımlanır, ancak bir Dizin Oluşturucu imza tarafından tanımlanır.
*  Bir özelliğe *simple_name* ([basit adlar](expressions.md#simple-names)) veya bir *member_access* ([üye erişimi](expressions.md#member-access)) üzerinden erişilir, ancak bir Dizin Oluşturucu öğesine bir *element_access* ([Dizin Oluşturucu erişimi](expressions.md#indexer-access)) üzerinden erişilir.
*  Bir özellik `static` üyesi olabilir, ancak bir Dizin Oluşturucu her zaman bir örnek üyesidir.
*  Bir özelliğin `get` erişimcisi parametresi olmayan bir yönteme karşılık gelir, ancak bir dizin oluşturucunun `get` erişimcisi, Dizin oluşturucudaki aynı biçimsel parametre listesine sahip bir yönteme karşılık gelir.
*  Bir özelliğin `set` erişimcisi, `value`adlı tek parametreli bir yönteme karşılık gelir, ancak bir dizin oluşturucunun `set` erişimcisi, Dizin oluşturucudaki aynı biçimsel parametre listesine sahip bir yönteme karşılık gelir ve `value`adlı ek bir parametredir.
*  Bir Dizin Oluşturucu erişimcisinin bir Dizin Oluşturucu parametresiyle aynı ada sahip yerel bir değişken bildirmesi için derleme zamanı hatası.
*  Geçersiz kılan özellik bildiriminde, devralınan özelliğe `base.P`sözdizimi kullanılarak erişilir; burada `P` özellik adıdır. Geçersiz kılan bir Dizin Oluşturucu bildiriminde, devralınan dizin oluşturucuya `base[E]`sözdizimi kullanılarak erişilir; burada `E` virgülle ayrılmış ifadelerin bir listesidir.
*  "Otomatik olarak uygulanan Dizin Oluşturucu" kavramı yoktur. Noktalı olmayan, dış olmayan ve olmayan bir dizinleyiciye sahip bir hata.

Bu farklılıklardan farklı olarak, [erişimciler](classes.md#accessors) ve [Otomatik uygulanan özellikleri](classes.md#automatically-implemented-properties) tanımlanan tüm kurallar, Dizin Oluşturucu erişimcileri ve özellik erişimcileri için de geçerlidir.

Bir Dizin Oluşturucu bildirimi `extern` değiştirici içerdiğinde, Dizin Oluşturucu bir ***dış Dizin Oluşturucu***olarak kabul edilir. Bir dış Dizin Oluşturucu bildirimi gerçek uygulama sağladığından, *accessor_declarations* her biri noktalı virgülle oluşur.

Aşağıdaki örnek, bit dizisindeki tek tek bitlerin erişimine yönelik bir Dizin Oluşturucu uygulayan `BitArray` sınıfını bildirir.
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

`BitArray` sınıfının bir örneği, karşılık gelen bir `bool[]` kıyasla önemli ölçüde daha az bellek tüketir (ilk değeri ikinci bir bayt yerine yalnızca bir bit kapladığından), ancak aynı işlemlere `bool[]`olarak izin verir.

Aşağıdaki `CountPrimes` sınıfı, 1 ila verilen bir maksimum değer arasındaki primin sayısını hesaplamak için bir `BitArray` ve klasik "sıya" algoritmasını kullanır:
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

`BitArray` öğelerine erişim sözdiziminin tam olarak bir `bool[]`ile aynı olduğunu unutmayın.

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

*Operator_body* noktalı virgül, ***deyim gövdesi*** veya ***ifade gövdesi***. Deyim gövdesi, işleç çağrıldığında yürütülecek deyimleri belirten bir *bloğundan*oluşur. *Blok* , [Yöntem gövdesinde](classes.md#method-body)açıklanan değer döndüren yöntemlere yönelik kurallara uymalıdır. Bir ifade gövdesi, bir ifadenin ve noktalı virgülden oluşan `=>` oluşur ve işleç çağrıldığında gerçekleştirilecek tek bir ifadeyi gösterir.

`extern` işleçleri için, *operator_body* yalnızca noktalı virgülden oluşur. Diğer tüm işleçler için *operator_body* bir blok gövdesi ya da bir ifade gövdesidir.

Tüm işleç bildirimleri için aşağıdaki kurallar geçerlidir:

*  İşleç bildirimi hem `public` hem de `static` değiştiricisini içermelidir.
*  Bir işlecin parametre (ler) i değer parametreleri olmalıdır ([değer parametreleri](variables.md#value-parameters)). `ref` veya `out` parametrelerini belirtmek için bir operatör bildirimine yönelik derleme zamanı hatasıdır.
*  Bir işlecin imzası ([Birli İşleçler](classes.md#unary-operators), [ikili işleçler](classes.md#binary-operators), [dönüştürme işleçleri](classes.md#conversion-operators)) aynı sınıfta belirtilen diğer tüm işleçlerin imzalarından farklı olmalıdır.
*  Bir işleç bildiriminde başvurulan tüm türlerin en az işlecin kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak erişilebilir olması gerekir.
*  Aynı değiştiricinin bir operatör bildiriminde birden çok kez görünmesi bir hatadır.

Her operatör kategorisi, aşağıdaki bölümlerde açıklandığı gibi ek kısıtlamalar uygular.

Diğer Üyeler gibi, bir temel sınıfta belirtilen operatörler türetilmiş sınıflar tarafından devralınır. İşleç bildirimleri, işlecin, işlecin imzasına katılması için bildirildiği sınıf veya yapının her zaman gerektirdiğinden, bir temel sınıfta belirtilen bir işleci gizlemek için türetilmiş sınıfta belirtilen bir operatör mümkün değildir. Bu nedenle, `new` değiştiricisi hiçbir şekilde gerektirmez ve bu nedenle bir işleç bildiriminde hiçbir şekilde izin verilmez.

Birli ve ikili işleçlerle ilgili ek bilgiler, [işleçlerinde](expressions.md#operators)bulunabilir.

Dönüştürme işleçleri hakkında ek bilgi, [Kullanıcı tanımlı dönüştürmelerde](conversions.md#user-defined-conversions)bulunabilir.

### <a name="unary-operators"></a>Birli İşleçler

Aşağıdaki kurallar, `T` birli operatör bildirimleri için geçerlidir; burada, işleç bildirimini içeren sınıfın veya yapının örnek türünü belirtir:

*  Birli `+`, `-`, `!`veya `~` işleci, `T` veya `T?` türünde tek bir parametre almalıdır ve herhangi bir tür döndürebilir.
*  Birli `++` veya `--` işleci, `T` veya `T?` türünde tek bir parametre almalıdır ve aynı türde veya ondan türetilmiş bir tür döndürmelidir.
*  Birli `true` veya `false` işleci, `T` veya `T?` türünde tek bir parametre almalıdır ve tür `bool`döndürmelidir.

Birli işlecin imzası, işleç belirtecinden (`+`, `-`, `!`, `~`, `++`, `--`, `true`veya `false`) ve tek biçimsel parametrenin türünden oluşur. Dönüş türü birli işlecin imzasının bir parçası değildir, ya da biçimsel parametrenin adıdır.

`true` ve `false` Birli İşleçler çift yönlü bildirim gerektirir. Bir sınıf bu işleçlerden birini diğeri de bildirmeksizin bildirirse bir derleme zamanı hatası oluşur. `true` ve `false` işleçleri, [Kullanıcı tanımlı Koşullu mantıksal işleçler](expressions.md#user-defined-conditional-logical-operators) ve [Boole ifadelerinde](expressions.md#boolean-expressions)daha ayrıntılı olarak açıklanmıştır.

Aşağıdaki örnek, bir tamsayı vektör sınıfı için bir uygulama ve sonraki `operator ++` kullanımını gösterir:
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

İşleç yönteminin, sonek artırma ve azaltma işleçleri ([sonek](expressions.md#postfix-increment-and-decrement-operators)artışı ve azaltma işleçleri) ve ön ek artırma ve azaltma Işleçleri ([önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)) gibi, işlenene 1 eklenerek üretilen değeri nasıl döndürdüğünü aklınızda. Uygulamasının aksine C++, bu yöntemin işlenenin değerini doğrudan değiştirmesi gerekmez. Aslında, işlenen değerini değiştirmek, sonek artışı işlecinin standart semantiğini ihlal ediyor.

### <a name="binary-operators"></a>İkili işleçler

Aşağıdaki kurallar, `T` işleç bildirimini içeren sınıfın veya yapının örnek türünü belirten ikili işleç bildirimleri için geçerlidir:

*  İkili vardiya olmayan bir operatör iki parametre almalıdır, en az biri tür `T` veya `T?`olmalıdır ve herhangi bir tür döndürebilir.
*  Bir ikili `<<` veya `>>` işleci iki parametre almalıdır. Bu, ilki tür `T` ya da `T?` ve ikincisinin türü `int` veya `int?`olmalıdır ve herhangi bir tür döndürebilir.

Bir ikili işlecinin imzası, işleç belirtecinden (`+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `==`, `!=`, `>`, `<`, `>=`veya `<=`) ve iki biçimsel parametrenin türlerinden oluşur. Dönüş türü ve biçimsel parametrelerin adları bir ikili işlecin imzasının parçası değil.

Belirli ikili işleçler çift yönlü bildirim gerektirir. Bir çiftin her iki işlecinin her bildirimi için, çiftin diğer işlecinin eşleşen bir bildirimi olması gerekir. İki işleç bildirimi aynı dönüş türüne ve her parametre için aynı türe sahip olduklarında eşleşir. Aşağıdaki işleçler çift yönlü bildirim gerektirir:

*  `operator ==` ve `operator !=`
*  `operator >` ve `operator <`
*  `operator >=` ve `operator <=`

### <a name="conversion-operators"></a>Dönüştürme işleçleri

Bir dönüştürme işleci bildirimi, önceden tanımlanmış örtük ve açık dönüştürmeleri genişleten ***Kullanıcı tanımlı bir dönüştürme*** ([Kullanıcı tanımlı dönüştürmeler](conversions.md#user-defined-conversions)) sunar.

`implicit` anahtar sözcüğünü içeren bir dönüştürme işleci bildirimi, Kullanıcı tanımlı bir örtük dönüştürmeyi tanıtır. Örtük dönüştürmeler işlev üyesi etkinleştirmeleri, atama ifadeleri ve atamalar dahil çeşitli durumlarda gerçekleşebilir. Bu, [örtük dönüştürmelerde](conversions.md#implicit-conversions)daha ayrıntılı olarak açıklanmıştır.

`explicit` anahtar sözcüğünü içeren bir dönüştürme işleci bildirimi, Kullanıcı tanımlı bir açık dönüştürme sağlar. Dönüştürme ifadelerinde açık dönüştürmeler gerçekleşebilir ve [Açık dönüştürmelerde](conversions.md#explicit-conversions)daha ayrıntılı olarak açıklanmıştır.

Bir dönüştürme işleci, dönüştürme işlecinin parametre türü ile belirtilen bir kaynak türünden, dönüştürme işlecinin dönüş türü tarafından belirtilen bir hedef türüne dönüştürülür.

Belirli bir kaynak türü `S` ve hedef türü `T`, `S` veya `T` null yapılabilir türlerse, `S0` ve `T0` temel türlerine başvurmasına izin verir, aksi takdirde `S0` ve `T0` sırasıyla `S` ve `T` eşittir. Bir sınıf veya yapının bir kaynak türü `S` bir hedef türüne dönüştürme bildirmesine izin verilir `T` yalnızca aşağıdakilerin tümü doğru ise geçerlidir:

*  `S0` ve `T0` farklı türlerdir.
*  `S0` ya da `T0`, işleç bildiriminin gerçekleştiği sınıf veya yapı türüdür.
*  Ne `S0` ne de `T0` bir *interface_type*.
*  Kullanıcı tanımlı dönüştürmeler hariç olmak üzere, `S` `T` veya `T` ' den `S`' e kadar bir dönüştürme yok.

Bu kuralların amaçları doğrultusunda, `S` veya `T` ilişkili herhangi bir tür parametresi, diğer türlerle devralma ilişkisi olmayan benzersiz türler olarak değerlendirilir ve bu tür parametrelerinin herhangi bir kısıtlaması yok sayılır.

örnekte
```csharp
class C<T> {...}

class D<T>: C<T>
{
    public static implicit operator C<int>(D<T> value) {...}      // Ok
    public static implicit operator C<string>(D<T> value) {...}   // Ok
    public static implicit operator C<T>(D<T> value) {...}        // Error
}
```
sırasıyla .3, `T` ve `int` ve `string` gibi [Dizin oluşturucular](classes.md#indexers)için hiçbir ilişki olmadan benzersiz türler olduğu için ilk iki operatör bildirimi izin verilir. Ancak, üçüncü işleç bir hatadır çünkü `C<T>` `D<T>`taban sınıfıdır.

İkinci kuraldan, bir dönüştürme işlecinin, işlecin bildirildiği sınıf veya yapı türünden ya da türüne dönüştürülmesi gerekir. Örneğin, bir sınıf veya yapı türü `C`, `C` 'den `int` ve `int` arasında bir dönüştürme tanımlamak, ancak `C`'den `int` 'ye değil.`bool`

Önceden tanımlanmış bir dönüştürmeyi doğrudan yeniden tanımlamak mümkün değildir. Bu nedenle, örtük ve açık dönüştürmeler `object` ve diğer tüm türler arasında zaten mevcut olduğundan, dönüştürme işleçleri `object` veya ' ya dönüştürme yapılamaz. Benzer şekilde, bir dönüştürme daha sonra zaten mevcut olduğundan, bir dönüştürmenin kaynağı veya hedef türleri diğerinin temel türü olamaz.

Ancak, belirli tür bağımsız değişkenleri için önceden tanımlanmış dönüştürmeler olarak zaten var olan dönüştürmeleri belirtmek üzere genel türlerde işleçler bildirmek mümkündür. örnekte
```csharp
struct Convertible<T>
{
    public static implicit operator Convertible<T>(T value) {...}
    public static explicit operator T(Convertible<T> value) {...}
}
```
tür `object` `T`için bir tür bağımsız değişkeni olarak belirtildiğinde, İkinci işleç zaten var olan bir dönüştürme (örtük olarak) ve bu nedenle bir açık, her türden `object`) bir dönüşüm olduğunu bildirir.

İki tür arasında önceden tanımlanmış bir dönüştürmenin mevcut olduğu durumlarda, bu türler arasındaki Kullanıcı tanımlı dönüştürmeler yok sayılır. Engelle

*  Tür `S` `T`tür ' den önceden tanımlanmış örtük dönüştürme ([örtük dönüştürmeler](conversions.md#implicit-conversions)) varsa, `T` `S` olan tüm Kullanıcı tanımlı dönüştürmeler (örtük veya açık) yok sayılır.
*  Tür `S` `T`türü ' nden önceden tanımlanmış bir açık dönüştürme ([Açık dönüştürmeler](conversions.md#explicit-conversions)) varsa, `T` `S` olan Kullanıcı tanımlı açık dönüştürmeler yok sayılır. Buna

`T` bir arabirim türü ise, Kullanıcı tanımlı örtük dönüştürmeler `S` `T` yok sayılır.

Aksi takdirde, `S` ile `T` arasında Kullanıcı tanımlı örtük dönüştürmeler yine de göz önünde bulundurulacaktır.

Tüm türler için `object`, yukarıdaki `Convertible<T>` türü tarafından belirtilen işleçler önceden tanımlanmış dönüşümlerle çakışmaz. Örneğin:
```csharp
void F(int i, Convertible<int> n) {
    i = n;                          // Error
    i = (int)n;                     // User-defined explicit conversion
    n = i;                          // User-defined implicit conversion
    n = (Convertible<int>)i;        // User-defined implicit conversion
}
```

Ancak, tür `object`için önceden tanımlı dönüştürmeler, her durumda Kullanıcı tanımlı dönüştürmeleri gizler, ancak bir:

```csharp
void F(object o, Convertible<object> n) {
    o = n;                         // Pre-defined boxing conversion
    o = (object)n;                 // Pre-defined boxing conversion
    n = o;                         // User-defined implicit conversion
    n = (Convertible<object>)o;    // Pre-defined unboxing conversion
}
```

Kullanıcı tanımlı dönüştürmelerde veya *interface_type*s 'ye dönüştürme yapılamaz. Özellikle, bu kısıtlama, bir *interface_type*dönüştürülürken Kullanıcı tanımlı dönüştürmeler olmamasını ve bir *interface_type* dönüştürmenin yalnızca, belirtilen *interface_type*gerçekten uyguladığı durumlarda başarılı olmasını sağlar.

Bir dönüştürme işlecinin imzası, kaynak türü ve hedef türünden oluşur. (Bunun, dönüş türünün imzaya katıldığı tek üye formu olduğunu unutmayın.) Bir dönüştürme işlecinin `implicit` veya `explicit` sınıflandırması, işlecin imzasının bir parçası değil. Bu nedenle, bir sınıf veya yapı aynı kaynak ve hedef türlerine sahip bir `implicit` ve `explicit` dönüştürme işleci bildiremez.

Genel olarak, Kullanıcı tanımlı örtük dönüştürmeler hiçbir şekilde özel durum oluşturmaz ve hiçbir bilgi kaybedilmez. Kullanıcı tanımlı bir dönüştürme özel durumlara (örneğin, kaynak bağımsız değişkeni aralık dışında) veya bilgi kaybına (yüksek sıralı bitleri atma gibi) izin verebileceğinden, bu dönüştürme açık bir dönüştürme olarak tanımlanmalıdır.

örnekte
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
`Digit` dönüştürme işlemi hiçbir `byte` şekilde özel durum oluşturmaz veya bilgi kaybettiğinden, ancak `Digit` yalnızca bir `byte`olası değerlerinin bir alt kümesini temsil ettiğinden, `byte` 'den `Digit` 'ye dönüştürme açıktır.

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

*Constructor_declaration* bir dizi *öznitelik* ([öznitelik](attributes.md)), dört erişim değiştiricisinin geçerli bir birleşimi ([erişim değiştiricileri](classes.md#access-modifiers)) ve bir `extern` ([dış Yöntemler](classes.md#external-methods)) değiştiricisi içerebilir. Oluşturucu bildiriminin aynı değiştiriciyi birden çok kez içerme izni yoktur.

Bir *constructor_declarator* *tanımlayıcısı* , örnek oluşturucusunun bildirildiği sınıfa ad vermelidir. Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.

Örnek oluşturucusunun isteğe bağlı *formal_parameter_list* , yöntemin *formal_parameter_list* aynı kurallara tabidir ([Yöntemler](classes.md#methods)). Biçimsel parametre listesi bir örnek oluşturucusunun imzasını ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) tanımlar ve aşırı yükleme çözümünün ([tür çıkarımı](expressions.md#type-inference)) bir çağrıdaki belirli bir örnek oluşturucusunu seçtiği işlemi yönetir.

Bir örnek oluşturucusunun *formal_parameter_list* başvuruda bulunulan türlerin her biri, oluşturucunun kendisi ([Erişilebilirlik kısıtlamaları](basic-concepts.md#accessibility-constraints)) olarak en az erişilebilir olmalıdır.

İsteğe bağlı *constructor_initializer* , bu örnek oluşturucusunun *constructor_body* verilen deyimleri yürütmeden önce çağrılacak başka bir örnek Oluşturucu belirtir. Bu, [Oluşturucu başlatıcılarda](classes.md#constructor-initializers)daha ayrıntılı olarak açıklanmıştır.

Bir Oluşturucu bildirimi `extern` değiştirici içerdiğinde, Oluşturucu bir ***dış Oluşturucu***olarak kabul edilir. Bir dış Oluşturucu bildirimi gerçek uygulama sağladığından *constructor_body* noktalı virgülden oluşur. Diğer tüm oluşturucular için *constructor_body* , sınıfın yeni bir örneğini başlatmak için deyimleri belirten bir *bloğundan* oluşur. Bu, tam olarak bir `void` dönüş türüne sahip bir örnek yönteminin *bloğuna* karşılık gelir ([Yöntem gövdesi](classes.md#method-body)).

Örnek oluşturucular devralınmaz. Bu nedenle, bir sınıf sınıf içinde gerçekten bildirilenler dışında bir örnek Oluşturucu içermez. Bir sınıf örnek Oluşturucu bildirimleri içermiyorsa, varsayılan bir örnek Oluşturucu otomatik olarak sağlanır ([Varsayılan oluşturucular](classes.md#default-constructors)).

Örnek oluşturucular *object_creation_expression*s ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)) ve *constructor_initializer*s aracılığıyla çağrılır.

### <a name="constructor-initializers"></a>Oluşturucu başlatıcıları

Tüm örnek oluşturucular (`object`sınıfları hariç) örtük olarak, başka bir örnek oluşturucusunun bir *constructor_body*hemen öncesine çağrılmasını içerir. Örtük olarak çağrılacak Oluşturucu *constructor_initializer*belirlenir:

*  `base(argument_list)` veya `base()` formunun bir örnek Oluşturucu başlatıcısı, doğrudan taban sınıftan çağrılabilir bir örnek oluşturucusunun oluşmasına neden olur. Bu Oluşturucu, varsa *argument_list* ve [aşırı yükleme](expressions.md#overload-resolution)çözümlemesi çözüm kuralları kullanılarak seçilir. Aday örnek oluşturucular kümesi, doğrudan Taban sınıfında yer alan tüm erişilebilir örnek oluşturuculardan veya doğrudan temel sınıfta hiçbir örnek Oluşturucu bildirilmemiş ise varsayılan oluşturucuda ([Varsayılan oluşturucular](classes.md#default-constructors)) oluşur. Bu küme boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir derleme zamanı hatası oluşur.
*  `this(argument-list)` veya `this()` formunun bir örnek Oluşturucu başlatıcısı, sınıfın kendisinden bir örnek oluşturucusunun çağrılmasına neden olur. Oluşturucu, varsa *argument_list* ve [aşırı yükleme](expressions.md#overload-resolution)çözümlemesi çözüm kuralları kullanılarak seçilir. Aday örnek oluşturucular kümesi, sınıfta belirtilen tüm erişilebilir örnek oluşturuculardan oluşur. Bu küme boşsa veya tek bir en iyi örnek Oluşturucu tanımlanamıyorsa, bir derleme zamanı hatası oluşur. Bir örnek Oluşturucu bildirimi, oluşturucuyu çağıran bir Oluşturucu başlatıcısı içeriyorsa, bir derleme zamanı hatası oluşur.

Bir örnek oluşturucusunun Oluşturucu başlatıcısı yoksa, `base()` form Oluşturucu başlatıcısı örtülü olarak sağlanır. Bu nedenle, formun örnek Oluşturucu bildirimi
```csharp
C(...) {...}
```
tam olarak eşdeğerdir
```csharp
C(...): base() {...}
```

Örnek Oluşturucu bildiriminin *formal_parameter_list* tarafından verilen parametrelerin kapsamı, bu bildirimin Oluşturucu başlatıcısını içerir. Bu nedenle, bir Oluşturucu başlatıcısının, oluşturucunun parametrelerine erişmesine izin verilir. Örneğin:
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

Örnek Oluşturucu Başlatıcısı oluşturulan örneğe erişemez. Bu nedenle, Oluşturucu başlatıcısının bir bağımsız değişken ifadesinde `this` başvurmak için derleme zamanı hatası, bir bağımsız değişken ifadesi için bir *simple_name*aracılığıyla herhangi bir örnek üyesine başvuruda bulunmak üzere bir derleme zamanı hatası olur.

### <a name="instance-variable-initializers"></a>Örnek değişken başlatıcıları

Bir örnek oluşturucusunun Oluşturucu başlatıcısı olmadığında veya `base(...)`form Oluşturucu başlatıcısı varsa, bu Oluşturucu kendi sınıfında belirtilen örnek alanlarının *variable_initializer*s tarafından belirtilen başlatmaları dolaylı olarak gerçekleştirir. Bu, oluşturucuya girişte ve doğrudan temel sınıf oluşturucusunun örtük çağrılmasıyla önce yürütülen bir atama dizisine karşılık gelir. Değişken başlatıcıları, sınıf bildiriminde göründükleri metin sırasına göre yürütülür.

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
bir `B`örneğini oluşturmak için `new B()` kullanıldığında aşağıdaki çıktı oluşturulur:
```console
x = 1, y = 0
```

`x` değeri, temel sınıf örneği Oluşturucusu çağrılmadan önce değişken başlatıcısı yürütüldüğü için 1 ' dir. Ancak, `y` değeri 0 ' dır (`int`varsayılan değerdir) çünkü `y` atama, taban sınıf oluşturucusu dönene kadar yürütülmez.

Örnek değişken başlatıcıları ve Oluşturucu başlatıcıları, *constructor_body*önce otomatik olarak eklenen deyimler olarak düşünmek yararlıdır. Örnek
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
Burada `C`, sınıfın adıdır. Aşırı yükleme çözümlemesi temel sınıf Oluşturucu başlatıcısı için benzersiz bir en iyi aday tespit leyemiyorsa, derleme zamanı hatası oluşur.

örnekte
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

Bir sınıf `T` yalnızca özel örnek oluşturucular bildirdiğinde, `T` program metni dışındaki sınıfların `T` türetmesini veya doğrudan `T`örneklerini oluşturmasını mümkün değildir. Bu nedenle, bir sınıf yalnızca statik üyeler içeriyorsa ve örneği oluşturulması amaçlanmazsa, boş bir özel örnek Oluşturucu eklemek, örnek oluşturmayı engeller. Örneğin:
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

`Trig` sınıfı ilgili yöntemleri ve sabitleri gruplandırır, ancak örneği oluşturulacak şekilde tasarlanmamıştır. Bu nedenle, tek bir boş özel örnek Oluşturucu bildirir. Varsayılan oluşturucunun otomatik olarak oluşturulmasını engellemek için en az bir örnek Oluşturucu bildirilmelidir.

### <a name="optional-instance-constructor-parameters"></a>İsteğe bağlı örnek Oluşturucu parametreleri

Oluşturucu başlatıcısının `this(...)` formu, isteğe bağlı örnek oluşturucu parametrelerini uygulamak için yaygın olarak aşırı yükleme ile birlikte kullanılır. örnekte
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
ilk iki örnek Oluşturucu yalnızca eksik bağımsız değişkenler için varsayılan değerleri sağlar. Her ikisi de, yeni örneği başlatma işini gerçekten yapan üçüncü örnek oluşturucuyu çağırmak için bir `this(...)` Oluşturucu başlatıcısı kullanın. Bu, isteğe bağlı Oluşturucu parametrelerinin etkidir:
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

Bir *static_constructor_declaration* bir *öznitelikler* kümesi ([öznitelikler](attributes.md)) ve bir `extern` değiştirici ([dış Yöntemler](classes.md#external-methods)) içerebilir.

Bir *static_constructor_declaration* *tanımlayıcısı* , statik oluşturucunun bildirildiği sınıfı içermelidir. Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.

Statik Oluşturucu bildirimi `extern` değiştirici içerdiğinde, statik oluşturucu bir ***dış statik Oluşturucu***olarak kabul edilir. Dış bir statik Oluşturucu bildirimi gerçek uygulama sağladığından *static_constructor_body* noktalı virgülden oluşur. Diğer tüm statik Oluşturucu bildirimleri için *static_constructor_body* , sınıfı başlatmak için yürütülecek deyimleri belirten bir *bloğundan* oluşur. Bu, tam olarak bir `void` dönüş türü ([Yöntem gövdesi](classes.md#method-body)) ile bir statik metodun *method_body* karşılık gelir.

Statik oluşturucular devralınmaz ve doğrudan çağrılamaz.

Kapalı bir sınıf türü için statik oluşturucu, belirli bir uygulama etki alanında en çok bir kez yürütülür. Bir statik oluşturucunun yürütülmesi, bir uygulama etki alanı içinde gerçekleşecek aşağıdaki olayların ilki tarafından tetiklenir:

*  Sınıf türünün bir örneği oluşturulur.
*  Sınıf türünün statik üyelerinden herhangi birine başvurulur.

Bir sınıf yürütmenin başladığı `Main` yöntemini ([uygulama başlatma](basic-concepts.md#application-startup)) içeriyorsa, bu sınıfın statik oluşturucusu, `Main` yöntemi çağrılmadan önce yürütülür.

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
```console
Init A
A.F
Init B
B.F
```
`A`statik oluşturucusunun yürütülmesi `A.F`çağrısı tarafından tetiklendiğinden ve `B`statik oluşturucusunun yürütülmesi `B.F`çağrısıyla tetikleniyor.

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
```console
X = 1, Y = 2
```

`Main` yöntemini yürütmek için, sistem ilk olarak, sınıf `B`statik Oluşturucusu öncesinde, `B.Y`için başlatıcıyı çalıştırır. `Y`başlatıcısı, `A.X` değerine başvurulduğundan `A`statik oluşturucusunun çalışmasına neden olur.  `A` statik Oluşturucusu `X`değerini hesaplamak için devam eder ve bunun yapılması, sıfır olan `Y`varsayılan değerini getirir. `A.X`, bu nedenle 1 olarak başlatılır. `A`statik alan başlatıcıları ve statik oluşturucuyu çalıştırma işlemi daha sonra, `Y`başlangıç değerinin hesaplamasına dönerek, sonucu 2 olur.

Statik Oluşturucu her bir kapalı oluşturulmuş sınıf türü için tam olarak bir kez yürütüldüğü için, tür parametresinde, kısıtlamalar aracılığıyla ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)) denetlenemeyen çalışma zamanı denetimlerini zorlamak için kullanışlı bir yerdir. Örneğin, aşağıdaki tür, tür bağımsız değişkeninin bir sabit listesi olmasını zorlamak için bir statik Oluşturucu kullanır:
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

Bir *destructor_declaration* *tanımlayıcısı* , yok edicinin bildirildiği sınıfı isimlendiremelidir. Başka bir ad belirtilmişse, derleme zamanı hatası oluşur.

Yıkıcı bildirimi `extern` değiştirici içerdiğinde, yok edicinin ***dış yıkıcı***olduğu söylenir. Bir dış yıkıcı bildirimi gerçek uygulama sunmadığından, *destructor_body* noktalı virgülden oluşur. Diğer tüm yok ediciler için *destructor_body* , sınıfın bir örneğinin çıkarılması için yürütülecek deyimleri belirten bir *bloğundan* oluşur. Bir *destructor_body* , bir örnek yönteminin `void` dönüş türüne ([Yöntem gövdesi](classes.md#method-body)) sahip *method_body* tam olarak karşılık gelir.

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

Yok ediciler, `System.Object``Finalize` sanal metot geçersiz kılınarak uygulanır. C#programların bu yöntemi geçersiz kılmasına veya doğrudan çağırmasına (ya da geçersiz kılınmasına) izin verilmez. Örneğin, program
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
geçerlidir ve gösterilen yöntem `System.Object``Finalize` metodunu gizler.

Yıkıcıdan bir özel durum oluştuğunda davranış tartışması için bkz. [özel durumların nasıl işlendiği](exceptions.md#how-exceptions-are-handled).

## <a name="iterators"></a>Yineleyiciler

Yineleyici bloğu ([bloklar](statements.md#blocks)) kullanılarak uygulanan bir işlev üyesine ([işlev üyeleri](expressions.md#function-members)) ***Yineleyici***denir.

Bir yineleyici bloğu, karşılık gelen işlev üyesinin dönüş türü Numaralandırıcı arabirimlerinden ([Numaralandırıcı arabirimleri](classes.md#enumerator-interfaces)) biri veya sıralanabilir arabirimlerin ([sıralanabilir arabirimler](classes.md#enumerable-interfaces)) biri olduğu sürece bir işlev üyesinin gövdesi olarak kullanılabilir. *Method_body*, *operator_body* veya *accessor_body*olarak gerçekleşebilir, ancak olaylar, örnek oluşturucular, statik oluşturucular ve Yıkıcılar yineleyiciler olarak uygulanamaz.

Bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesinin biçimsel parametre listesi için, herhangi bir `ref` veya `out` parametresi belirtmek üzere derleme zamanı hatası olur.

### <a name="enumerator-interfaces"></a>Numaralandırıcı arabirimleri

***Numaralandırıcı arabirimleri*** genel olmayan arabirim `System.Collections.IEnumerator` ve genel arabirim `System.Collections.Generic.IEnumerator<T>`tüm örneklemelerdir. Breçekimi 'nin sake 'ı için bu bölümde sırasıyla `IEnumerator` ve `IEnumerator<T>`olarak başvurulur.

### <a name="enumerable-interfaces"></a>Sıralanabilir arabirimler

***Sıralanabilir arabirimler*** genel olmayan arabirim `System.Collections.IEnumerable` ve genel arabirim `System.Collections.Generic.IEnumerable<T>`tüm örneklemelerdir. Breçekimi 'nin sake 'ı için bu bölümde sırasıyla `IEnumerable` ve `IEnumerable<T>`olarak başvurulur.

### <a name="yield-type"></a>Yield türü

Bir yineleyici, hepsi aynı türden bir değer dizisi üretir. Bu tür, yineleyicinin ***yield türü*** olarak adlandırılır.

*  `IEnumerator` veya `IEnumerable` döndüren bir yineleyicinin yield türü `object`.
*  `IEnumerator<T>` veya `IEnumerable<T>` döndüren bir yineleyicinin yield türü `T`.

### <a name="enumerator-objects"></a>Numaralandırıcı nesneleri

Bir Numaralandırıcı arabirim türü döndüren bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesini çağırmak kodu Yineleyici bloğunda hemen yürütmez. Bunun yerine, bir ***Numaralandırıcı nesnesi*** oluşturulup döndürülür. Bu nesne Yineleyici bloğunda belirtilen kodu kapsüller ve Numaralandırıcı nesnesinin `MoveNext` yöntemi çağrıldığında Yineleyici bloğunda kodun yürütülmesi oluşur. Bir Numaralandırıcı nesnesi aşağıdaki özelliklere sahiptir:

*  `IEnumerator` ve `IEnumerator<T>`uygular; burada `T`, yineleyicinin yield türüdür.
*  `System.IDisposable`uygular.
*  Bağımsız değişken değerlerinin bir kopyasıyla başlatılır (varsa) ve örnek değeri işlev üyesine geçirilir.
*  Dört olası durum, daha ***önce***, ***çalışıyor***, ***askıya alındı***ve ***sonra***ilk olarak ***önceki*** durumda olur.

Numaralandırıcı nesnesi genellikle Yineleyici bloğundaki kodu kapsülleyen ve Numaralandırıcı arabirimlerini uygulayan derleyici tarafından oluşturulan Numaralandırıcı sınıfının bir örneğidir, ancak diğer uygulama yöntemleri mümkündür. Bir Numaralandırıcı sınıfı derleyici tarafından oluşturulduysa, bu sınıf doğrudan veya dolaylı olarak işlev üyesini içeren sınıfta iç içe gelir, özel erişilebilirliği olur ve derleyici kullanımı ([tanımlayıcılar](lexical-structure.md#identifiers)) için ayrılmış bir ada sahip olur.

Bir Numaralandırıcı nesnesi, yukarıda belirtilenden daha fazla arabirim uygulayabilir.

Aşağıdaki bölümlerde, bir Numaralandırıcı nesnesi tarafından sunulan `IEnumerable` ve `IEnumerable<T>` arabirimi uygulamalarının `MoveNext`, `Current`ve `Dispose` üyelerinin tam davranışı açıklanır.

Numaralandırıcı nesnelerinin `IEnumerator.Reset` yöntemini desteklemediğini unutmayın. Bu yöntemi çağırmak bir `System.NotSupportedException` oluşturulmasına neden olur.

#### <a name="the-movenext-method"></a>MoveNext yöntemi

Bir Numaralandırıcı nesnesinin `MoveNext` yöntemi bir yineleyici bloğunun kodunu kapsüller. `MoveNext` yöntemini çağırmak Yineleyici bloğunda kodu yürütür ve Numaralandırıcı nesnesinin `Current` özelliğini uygun şekilde ayarlar. `MoveNext` tarafından gerçekleştirilen kesin eylem, `MoveNext` çağrıldığında Numaralandırıcı nesnesinin durumuna bağlıdır:

*  Numaralandırıcı nesnesinin durumu daha ***önce***ise `MoveNext`çağrılıyor:
   * Durumu ***çalışıyor***olarak değiştirir.
   * Yineleyici bloğunun parametrelerini (`this`dahil), Numaralandırıcı nesnesi başlatıldığında kaydedilen bağımsız değişken değerlerine ve örnek değerine başlatır.
   * , Yürütme kesintiye gelinceye kadar Yineleyici bloğunu baştan yürütür (aşağıda açıklandığı gibi).
*  Numaralandırıcı nesnesinin durumu ***çalışıyorsa***, `MoveNext` çağırma sonucu belirtilmemiş olur.
*  Numaralandırıcı nesnesinin durumu ***askıya***alınırsa, `MoveNext`çağrılıyor:
   * Durumu ***çalışıyor***olarak değiştirir.
   * Tüm yerel değişkenlerin ve parametrelerin (Bu dahil) değerlerini, yineleyici bloğunun yürütülmesi son askıya alındığında kaydedilen değerlere geri yükler. Bu değişkenlerin başvurduğu nesnelerin içeriklerinin, önceki MoveNext çağrısından bu yana değişmiş olabileceğini unutmayın.
   * Yürütmenin askıya alınmasına neden olan `yield return` deyimin hemen sonrasında Yineleyici bloğunun yürütülmesini sürdürür ve yürütme kesintiye gelinceye kadar devam eder (aşağıda açıklandığı gibi).
*  Numaralandırıcı nesnesinin durumu ***After***ise, çağırma `MoveNext` `false`döndürür.


`MoveNext` Yineleyici bloğunu yürüttüğünde, yürütme dört şekilde kesilebilir: bir `yield return` bildirimiyle, bir `yield break` ifadesiyle, yineleyici bloğunun sonuyla karşılaşarak ve bir özel durum oluşan ve yineleyici bloğunun dışına yayıldığında.

*  Bir `yield return` ifadesiyle karşılaşıldığında ([yield ekstresi](statements.md#the-yield-statement)):
   * Deyimde verilen ifade değerlendirilir, örtülü olarak yield türüne dönüştürülür ve Numaralandırıcı nesnesinin `Current` özelliğine atanır.
   * Yineleyici gövdesinin yürütülmesi askıya alındı. Tüm yerel değişkenlerin ve parametrelerin (`this`dahil) değerleri, bu `yield return` ifadesinin konumu olduğu gibi kaydedilir. `yield return` deyimin bir veya daha fazla `try` bloğu içindeyse, ilişkili `finally` blokları şu an yürütülmez.
   * Numaralandırıcı nesnesinin durumu ***askıya alındı***olarak değiştirildi.
   * `MoveNext` yöntemi, yinelemesinin bir sonraki değere başarıyla ilerlemiş olduğunu belirten, çağırana `true` döndürür.
*  Bir `yield break` ifadesiyle karşılaşıldığında ([yield ekstresi](statements.md#the-yield-statement)):
   * `yield break` deyimin bir veya daha fazla `try` bloğu içindeyse, ilişkili `finally` blokları yürütülür.
   * Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.
   * `MoveNext` yöntemi, yinelemesinin tamamlandığını belirten, çağıranına `false` döndürür.
*  Yineleyici gövdesinin sonuna rastlandı:
   * Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.
   * `MoveNext` yöntemi, yinelemesinin tamamlandığını belirten, çağıranına `false` döndürür.
*  Bir özel durum oluştuğunda ve yineleyici bloğundan yayıldığında:
   * Yineleyici gövdesinde uygun `finally` blokları özel durum yayma tarafından yürütülür.
   * Numaralandırıcı nesnesinin durumu, daha ***sonra***olarak değiştirilir.
   * Özel durum yayma `MoveNext` yöntemi çağıranına devam eder.

#### <a name="the-current-property"></a>Geçerli özellik

Bir Numaralandırıcı nesnenin `Current` özelliği, yineleyici bloğundaki `yield return` deyimlerden etkilenir.

Bir Numaralandırıcı nesnesi ***askıya alınma*** durumundaysa, `Current` değeri önceki `MoveNext`çağrısı tarafından ayarlanan değerdir. Bir Numaralandırıcı nesnesi ***önceki***, ***çalışan***veya ***sonrasında*** olduğunda, `Current` erişimi sonucu belirtilmemiş olur.

`object`dışında bir yield türü olan bir yineleyici için, Numaralandırıcı nesnenin `IEnumerable` uygulamasıyla `Current` erişmenin sonucu, Numaralandırıcı nesnenin `IEnumerator<T>` uygulama aracılığıyla `Current` erişme ve sonucu `object`olarak atama anlamına gelir.

#### <a name="the-dispose-method"></a>Dispose yöntemi

`Dispose` yöntemi, Numaralandırıcı nesnesini ***After*** durumuna getirerek yinelemeyi temizlemek için kullanılır.

*  Numaralandırıcı nesnesinin durumu daha ***önce***ise, çağırma `Dispose` durumu ***sonra***olarak değiştirir.
*  Numaralandırıcı nesnesinin durumu ***çalışıyorsa***, `Dispose` çağırma sonucu belirtilmemiş olur.
*  Numaralandırıcı nesnesinin durumu ***askıya***alınırsa, `Dispose`çağrılıyor:
   * Durumu ***çalışıyor***olarak değiştirir.
   * Son çalıştırılan `yield return` ifadesiyle `yield break` bir ifade olduğu sürece herhangi bir finally bloğunu yürütür. Bu durum, yineleyici gövdesinden oluşan bir özel durumun oluşturulmasına ve yayılmasına neden olursa, Numaralandırıcı nesnesinin durumu ***After*** olarak ayarlanır ve özel durum `Dispose` yönteminin çağıranına yayılır.
   * Durumu ***sonraki***olarak değiştirir.
*  Numaralandırıcı nesnesinin durumu ***After***ise, çağırma `Dispose`, hiçbir etkisi olmaz.

### <a name="enumerable-objects"></a>Sıralanabilir nesneler

Sıralanabilir bir arabirim türü döndüren bir işlev üyesi Yineleyici bloğu kullanılarak uygulandığında, işlev üyesini çağırmak kodu Yineleyici bloğunda hemen yürütmez. Bunun yerine, ***sıralanabilir bir nesne*** oluşturulur ve döndürülür. Sıralanabilir nesnenin `GetEnumerator` yöntemi, yineleyici bloğunda belirtilen kodu kapsülleyen bir Numaralandırıcı nesnesi döndürür ve Numaralandırıcı nesnesinin `MoveNext` yöntemi çağrıldığında Yineleyici bloğunda kodun yürütülmesi oluşur. Sıralanabilir bir nesne aşağıdaki özelliklere sahiptir:

*  `IEnumerable` ve `IEnumerable<T>`uygular; burada `T`, yineleyicinin yield türüdür.
*  Bağımsız değişken değerlerinin bir kopyasıyla başlatılır (varsa) ve örnek değeri işlev üyesine geçirilir.

Sıralanabilir bir nesne genellikle Yineleyici bloğunda kodu kapsülleyen ve numaralandırılabilir arabirimleri uygulayan, derleyici tarafından oluşturulan bir sıralanabilir sınıfın örneğidir, ancak diğer uygulama yöntemleri mümkündür. Bir sıralanabilir sınıf derleyici tarafından oluşturulduysa, bu sınıf doğrudan veya dolaylı olarak işlev üyesini içeren sınıfta iç içe gelir, özel erişilebilirliği olur ve derleyici kullanımı ([tanımlayıcılar](lexical-structure.md#identifiers)) için ayrılmış bir ada sahip olur.

Sıralanabilir bir nesne, yukarıda belirtilenden daha fazla arabirim uygulayabilir. Özellikle de, sıralanabilir bir nesne `IEnumerator` ve `IEnumerator<T>`uygulayabilir, bu da hem numaralandırılabilir hem de numaralandırıcı olarak işlev görmesi sağlar. Bu tür bir uygulamada, bir sıralanabilir nesnenin ilk kez `GetEnumerator` yöntemi çağrıldığında, sıralanabilir nesnenin kendisi döndürülür. Sıralanabilir nesnenin `GetEnumerator`sonraki etkinleştirmeleri, varsa, sıralanabilir nesnenin bir kopyasını döndürür. Bu nedenle, döndürülen her Numaralandırıcı kendi durumuna sahiptir ve bir Numaralandırıcı içindeki değişiklikler başka bir Numaralandırıcı olur.

#### <a name="the-getenumerator-method"></a>GetEnumerator yöntemi

Sıralanabilir bir nesne, `IEnumerable` ve `IEnumerable<T>` arabirimlerinin `GetEnumerator` yöntemlerinin bir uygulamasını sağlar. İki `GetEnumerator` yöntemi, kullanılabilir bir Numaralandırıcı nesnesi elde eden ve döndüren ortak bir uygulamayı paylaşır. Numaralandırıcı nesnesi, sıralanabilir nesne başlatıldığında bağımsız değişken değerleri ve örnek değeri ile başlatılır, aksi takdirde Numaralandırıcı nesnesi, [Numaralandırıcı nesnelerinde](classes.md#enumerator-objects)açıklanan şekilde çalışır.

### <a name="implementation-example"></a>Uygulama örneği

Bu bölümde, yineleyicilerin standart C# yapılar açısından olası bir uygulanması açıklanmaktadır. Burada açıklanan uygulama, Microsoft C# derleyicisi tarafından kullanılan ilkelere dayanır, ancak bu, bir uygulanan uygulamasına veya mümkün olan tek bir uygulama anlamına gelir.

Aşağıdaki `Stack<T>` sınıfı, bir yineleyici kullanarak `GetEnumerator` metodunu uygular. Yineleyici, yığının öğelerini üstten alta doğru sıralamayla numaralandırır.

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

`GetEnumerator` yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyicinin ürettiği bir Numaralandırıcı sınıfının bir örneklemesine çevrilebilir.

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

Yukarıdaki çeviride, yineleyici bloğundaki kod bir durum makinesine açılıp Numaralandırıcı sınıfının `MoveNext` yöntemine yerleştirilir. Ayrıca, yerel değişken `i`, Numaralandırıcı nesnesindeki bir alana açık olduğundan, `MoveNext`çağırmaları arasında olmaya devam edebilir.

Aşağıdaki örnekte, 1 ile 10 arasındaki tamsayıların basit bir çarpma tablosu yazdırılır. Örnekteki `FromTo` yöntemi, sıralanabilir bir nesne döndürüyor ve yineleyici kullanılarak uygulanır.

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

`FromTo` yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu sarmalayan derleyici tarafından oluşturulmuş bir sıralanabilir sınıfın bir örneklemesine çevrilebilir.

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

Sıralanabilir sınıf hem numaralandırılabilir arabirimleri hem de Numaralandırıcı arabirimlerini uygular, bu sayede hem numaralandırılabilir hem de numaralandırıcı olarak işlev sağlar. `GetEnumerator` yöntemi ilk kez çağrıldığında, sıralanabilir nesnenin kendisi döndürülür. Sıralanabilir nesnenin `GetEnumerator`sonraki etkinleştirmeleri, varsa, sıralanabilir nesnenin bir kopyasını döndürür. Bu nedenle, döndürülen her Numaralandırıcı kendi durumuna sahiptir ve bir Numaralandırıcı içindeki değişiklikler başka bir Numaralandırıcı olur. `Interlocked.CompareExchange` yöntemi, iş parçacığı güvenli işlemi sağlamak için kullanılır.

`from` ve `to` parametreleri, sıralanabilir sınıftaki alanlara açıktır. Yineleyici bloğunda `from` değiştirildiğinden, her Numaralandırmadaki `from` için verilen ilk değeri tutmak üzere ek bir `__from` alanı tanıtılmıştır.

`MoveNext` yöntemi `__state` `0`olduğunda çağrılırsa bir `InvalidOperationException` oluşturur. Bu, önce `GetEnumerator`çağrılmadan Numaralandırıcı nesne olarak sıralanabilir nesnenin kullanılmasına karşı koruma sağlar.

Aşağıdaki örnek bir basit ağaç sınıfını göstermektedir. `Tree<T>` sınıfı, bir yineleyici kullanarak `GetEnumerator` metodunu uygular. Yineleyici, ağaç öğelerini geçersiz kılma sırasında sıralar.

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

`GetEnumerator` yöntemi, aşağıdaki şekilde gösterildiği gibi, yineleyici bloğunda kodu kapsülleyen derleyicinin ürettiği bir Numaralandırıcı sınıfının bir örneklemesine çevrilebilir.

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

`foreach` deyimlerde kullanılan derleyicinin oluşturduğu geçiciler, Numaralandırıcı nesnesinin `__left` ve `__right` alanlarına yükseltilmemiş. Bir özel durum oluşturulursa doğru `Dispose()` yönteminin doğru şekilde çağrılabilmesi için, Numaralandırıcı nesnesinin `__state` alanı dikkatle güncellenir. Çevrilmiş kodu basit `foreach` deyimleriyle yazmak mümkün değildir.

## <a name="async-functions"></a>Zaman uyumsuz işlevler

`async` değiştiricisine sahip bir yönteme ([yöntemlere](classes.md#methods)) veya anonim Işleve ([anonim işlev ifadelerine](expressions.md#anonymous-function-expressions)) ***zaman uyumsuz işlev***denir. Genel olarak, ***zaman uyumsuz*** terimi `async` değiştiricisine sahip herhangi bir tür işlevi tanımlamakta kullanılır.

Herhangi bir `ref` veya `out` parametresi belirtmek için zaman uyumsuz bir işlevin biçimsel parametre listesi için derleme zamanı hatası.

Zaman uyumsuz bir metodun *return_type* `void` ya da bir ***görev türü***olmalıdır. Görev türleri `System.Threading.Tasks.Task` ve `System.Threading.Tasks.Task<T>`oluşturulan türlerdir. Breçekimi 'nin sake 'ı için, bu bölümde sırasıyla `Task` ve `Task<T>`olarak başvurulur. Bir görev türü döndüren zaman uyumsuz bir yöntem, görev döndüren olarak kabul edilir.

Görev türlerinin tam tanımı uygulama tanımlı, ancak dilin bir görev türü görüntüleme noktasından tamamlanmamış, başarılı veya hatalı durumlardan birinde. Hatalı bir görev ilgili özel durumu kaydeder. Başarılı bir `Task<T>` `T`türünde bir sonuç kaydeder. Görev türleri beklenebilir ve bu nedenle await ifadelerinin işlenenleri ([await ifadeleri](expressions.md#await-expressions)) olabilir.

Zaman uyumsuz işlev çağırma, kendi gövdesinde await ifadeleri ([await ifadeleri](expressions.md#await-expressions)) yoluyla değerlendirmeyi askıya alabilir. Değerlendirme daha sonra, bir ***sürdürme temsilcisi***tarafından askıya alma await ifadesinin noktasında devam edebilir. Sürdürme temsilcisi `System.Action`türüdür ve çağrıldığında, zaman uyumsuz işlev çağrısının değerlendirmesi, kaldığı bekleme ifadesinden sürdürülecek. Zaman uyumsuz işlev çağrısının ***geçerli çağıranı*** , işlev çağrısı hiçbir zaman askıya alınmadıysa veya sürdürme temsilcisinin en son çağıranı yoksa, özgün çağırıcı olur.

### <a name="evaluation-of-a-task-returning-async-function"></a>Bir görev döndüren zaman uyumsuz işlev değerlendirmesi

Bir görev döndüren zaman uyumsuz işlevin çağrılması döndürülen görev türünün bir örneğinin oluşturulmasına neden olur. Bu, Async işlevinin ***Return görevi*** olarak adlandırılır. Görev başlangıçta tamamlanmamış bir durumda.

Zaman uyumsuz işlev gövdesi, askıya alınana kadar (bir await ifadesine ulaşılarak) ya da sona erene kadar değerlendirilir, bu da dönüş göreviyle birlikte çağrı yapana döndürülür.

Async işlevinin gövdesi sonlandırıldığında, geri dönüş görevi tamamlanmamış durumdan çıkarılır:

*  İşlev gövdesi bir dönüş ifadesine veya gövdenin sonuna ulaşılması sonucu olarak sonlandığında, herhangi bir sonuç değeri, başarılı durumuna yerleştirilen dönüş görevine kaydedilir.
*  İşlev gövdesi yakalanamayan bir özel durum ([throw](statements.md#the-throw-statement)) sonucu olarak sonlandığında, özel durum hatalı duruma yerleştirilen geri dönüş görevine kaydedilir.

### <a name="evaluation-of-a-void-returning-async-function"></a>Void döndüren zaman uyumsuz bir işlevin değerlendirmesi

Zaman uyumsuz işlevin dönüş türü `void`, değerlendirme yukarıdan aşağıdaki şekilde farklılık gösterir: hiçbir görev döndürülmediği için, işlev, geçerli iş parçacığının ***eşitleme bağlamına***tamamlanma ve özel durumları iletişim kurar. Eşitleme bağlamının tam tanımı uygulamaya bağımlıdır, ancak geçerli iş parçacığının çalıştırıldığı "nerede" gösterimidir. Bir void döndüren zaman uyumsuz işlevin değerlendirilmesi, başarıyla tamamlandığında veya yakalanamayan bir özel durumun oluşturulmasına neden olduğunda eşitleme bağlamına bildirim gönderilir.

Bu, bağlamın altında kaç tane void döndüren zaman uyumsuz işlev çalıştığını izlemeye ve bunlardan gelen özel durumların nasıl yayabileceğine karar vermesine olanak tanır.
