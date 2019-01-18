---
ms.openlocfilehash: 9c3863c9a139f5b8309fca6e0c099d0fae7677c3
ms.sourcegitcommit: 3fc033b6e98ed7ecdf46a85c79b00a3a3ddcf963
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "47230015"
---
# <a name="namespaces"></a>Ad Alanları

Ad alanlarını kullanarak C# programları düzenlenir. Ad alanları, hem bir program için bir "İç" Kuruluş sistemi olarak ve "harici" Kuruluş sistemi olarak kullanılır — diğer programlara sunulan program öğelerini sunmak için bir yol.

Using yönergeleri ([yönergeleri kullanarak](namespaces.md#using-directives)) ad alanları kullanımını kolaylaştırmak için sağlanır.

## <a name="compilation-units"></a>Derleme birimi

A *compilation_unit* kaynak dosya genel yapısını tanımlar. Sıfır veya daha fazla derleme biriminde oluşur *using_directive*s ardından sıfır veya daha fazla *global_attributes* ardından sıfır veya daha fazla *namespace_member_declaration*s .

```antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? namespace_member_declaration*
    ;
```

Bir C# programı bir veya birden çok derleme biriminden oluşur, her bir ayrı bir kaynak dosyasında yer alan. Bir C# programı derlendiğinde, tüm derleme birimlerinin birlikte işlenir. Bu nedenle, derleme biriminden birbirleri üzerinde büyük olasılıkla dönüşümlü bağlı olabilir.

*Using_directive*derleme birimi etkiler, s *global_attributes* ve *namespace_member_declaration*s, bu derleme biriminin ancak sahip hiçbir etkisi diğer derleme birimi.

*Global_attributes* ([öznitelikleri](attributes.md)) derleme birimi hedef assembly ve module öznitelikleri belirtilmesine izin verir. Derlemeler ve modüller türleri için fiziksel bir kapsayıcı olarak davranır. Bir derlemeyi birden fazla fiziksel olarak ayrı modüllerini içerebilir.

*Namespace_member_declaration*s her derleme biriminde bir programın, genel ad alanı adı verilen bir tek bildirim alanına üyeleri katkıda. Örneğin:

Dosya `A.cs`:
```csharp
class A {}
```

Dosya `B.cs`:
```csharp
class B {}
```

Bu durumda tam olarak nitelenmiş ada sahip iki sınıfları bildirme iki derleme biriminden tek genel ad alanına katkıda `A` ve `B`. İki derleme biriminden aynı bildirim alanına katkıda bulunmak için her bir bildirimi aynı ada sahip bir üye içeriyorsa bir hata olacaktı.

## <a name="namespace-declarations"></a>Namespace bildirimi

A *namespace_declaration* anahtar sözcüğünü oluşur `namespace`, ardından bir ad alanı adı ve gövde, isteğe bağlı olarak noktalı virgül ekleyin.

```antlr
namespace_declaration
    : 'namespace' qualified_identifier namespace_body ';'?
    ;

qualified_identifier
    : identifier ('.' identifier)*
    ;

namespace_body
    : '{' extern_alias_directive* using_directive* namespace_member_declaration* '}'
    ;
```

A *namespace_declaration* bir üst düzey bildiriminde olarak ortaya çıkabilecek bir *compilation_unit* veya bir üye bildirimi içindeki başka olarak *namespace_declaration*. Olduğunda bir *namespace_declaration* bir üst düzey bildiriminde olarak gerçekleşir bir *compilation_unit*, ad alanı genel ad alanının bir üyesi haline gelir. Olduğunda bir *namespace_declaration* başka gerçekleşir *namespace_declaration*, iç ad alanı, dış ad alanının bir üyesi haline gelir. Her iki durumda da, bir ad alanı adını içeren ad alanı içinde benzersiz olmalıdır.

Ad alanları olan örtük olarak `public` ve hiçbir erişim değiştiricileri bir ad alanı bildirimi içeremez.

İçinde bir *namespace_body*, isteğe bağlı *using_directive*s içeri aktarma diğer ad alanları, türler ve üyeler, nitelikli adlar doğrudan yerine, başvurulabilmesi için adları. İsteğe bağlı *namespace_member_declaration*s bildirim alanına ad alanının üyeleri katkıda bulunun. Unutmayın tüm *using_directive*s, herhangi bir üye bildirimleri önce görünmelidir.

*Qualified_identifier* , bir *namespace_declaration* tek bir tanımlayıcı veya ayrılmış tanımlayıcılar bir dizi olabilir "`.`" belirteçleri. İkinci form, birden fazla ad alanı bildirimi sözcüksel olarak iç içe geçirme olmadan bir iç içe geçmiş ad alanı tanımlamak için bir program izin verir. Örneğin,

```csharp
namespace N1.N2
{
    class A {}

    class B {}
}
```
anlamsal olarak eşdeğer olan
```csharp
namespace N1
{
    namespace N2
    {
        class A {}

        class B {}
    }
}
```

Açık uçlu ad alanlarını ve aynı tam ada sahip iki ad alanı bildirimi katkıda aynı bildirim alanına ([bildirimleri](basic-concepts.md#declarations)). Örnekte
```csharp
namespace N1.N2
{
    class A {}
}

namespace N1.N2
{
    class B {}
}
```
Bu durumda tam olarak nitelenmiş ada sahip iki sınıfları bildirme aynı bildirim alanına, yukarıdaki iki ad alanı bildirimi katkıda `N1.N2.A` ve `N1.N2.B`. İki bildirimi aynı bildirim alanına katkıda bulunmak için bir hata her varsa aynı ada sahip bir üye bildirimi bulunan olacaktı.

## <a name="extern-aliases"></a>Extern diğer adları

Bir *extern_alias_directive* bir ad alanı için bir diğer ad olarak hizmet veren bir tanımlayıcı sağlar. Diğer adlı ad alanı belirtimi, programın kaynak koduna dış ve diğer adlı ad alanı iç içe geçmiş ad alanları için de geçerlidir.

```antlr
extern_alias_directive
    : 'extern' 'alias' identifier ';'
    ;
```

Kapsamı bir *extern_alias_directive* üzerinden genişletir *using_directive*s, *global_attributes* ve *namespace_member_declaration*hemen içeren derleme birimi veya ad alanı gövdesinde, s.

İçeren bir derleme birimi veya ad alanı gövdesi içindeki bir *extern_alias_directive*, tarafından tanıtılan tanımlayıcısı *extern_alias_directive* diğer adlı ad alanı başvuru için kullanılabilir. Bir derleme zamanı hata için *tanımlayıcı* kelime olarak `global`.

Bir *extern_alias_directive* bir derleme belirli birimi veya ad alanı gövdesi içindeki diğer kullanılabilir hale getirir, ancak tüm yeni üyeleri temel alınan bildirim alanına gereksinimdir. Diğer bir deyişle, bir *extern_alias_directive* geçişli değildir ancak bunun yerine oluştuğu yalnızca derleme birimi veya ad alanı gövdesi etkiler.

Aşağıdaki program bildirir ve iki extern diğer adları kullanır `X` ve `Y`, her biri ayrı ad alanı hiyerarşisinin kökü temsil:
```csharp
extern alias X;
extern alias Y;

class Test
{
    X::N.A a;
    X::N.B b1;
    Y::N.B b2;
    Y::N.C c;
}
```

Program extern varlığını diğer adları bildirir `X` ve `Y`, ancak diğer adlar gerçek tanımlarını programa dış. Aynı adlı `N.B` sınıfları artık başvurulabilir olarak `X.N.B` ve `Y.N.B`, veya ad alanı diğer ad niteleyicisi kullanarak `X::N.B` ve `Y::N.B`. Bir program dış tanım olarak sağlanan bir extern diğer adı bildiren bir hata oluşur.

## <a name="using-directives"></a>using yönergeleri

***Using yönergeleri*** ad alanları ve diğer ad alanlarında tanımlanan türlerin kullanımı kolaylaştırmak. Ad çözümleme işlemi yönergeleri etkisi kullanarak *namespace_or_type_name*s ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) ve *simple_name*s ([basit adları ](expressions.md#simple-names)), ancak bildirimleri yönergeleri kullanarak, farklı değil katkıda yeni üyeler temel alınan bildirim alanlarına derleme biriminden veya ad alanları içinde bunlar kullanılır.

```antlr
using_directive
    : using_alias_directive
    | using_namespace_directive
    | using_static_directive
    ;
```

A *using_alias_directive* ([diğer yönergeleri kullanarak](namespaces.md#using-alias-directives)) ad alanı veya tür için bir diğer ad tanıtır.

A *using_namespace_directive* ([ad alanı yönergeleri kullanarak](namespaces.md#using-namespace-directives)) bir ad alanı, tür üyeleri alır.

A *using_static_directive* ([statik yönergeleri kullanarak](namespaces.md#using-static-directives)) bir türün statik üyeleri ve iç içe geçmiş türleri içeri aktarır.

Kapsamı bir *using_directive* üzerinden genişletir *namespace_member_declaration*hemen içeren derleme birimi veya ad alanı gövdesinde, s. Kapsamı bir *using_directive* özellikle, eş içermez *using_directive*s. Bu nedenle, eş *using_directive*s birbirine etkilemez ve bunlar yazılan sırası belirli değildir.

### <a name="using-alias-directives"></a>Using diğer adı yönergeleri

A *using_alias_directive* için bir ad alanı veya tür kapsayan derleme birimi veya ad alanı gövdesi içinde bir diğer ad olarak hizmet veren bir tanımlayıcı sağlar.

```antlr
using_alias_directive
    : 'using' identifier '=' namespace_or_type_name ';'
    ;
```

Üye bildirimleri gövdesindeki içeren bir derleme birimi veya ad alanı içinde bir *using_alias_directive*, tarafından tanıtılan tanımlayıcısı *using_alias_directive* kullanılabilir başvurmak için verilen ad alanı veya tür. Örneğin:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;

    class B: A {}
}
```

Yukarıdaki üye bildirimlerinde içinde `N3` ad alanı, `A` için bir diğer addır `N1.N2.A`ve bu nedenle sınıf `N3.B` sınıfından türetilen `N1.N2.A`. Bir diğer ad oluşturarak aynı etkiyi elde edilebilir `R` için `N1.N2` ve ardından başvuran `R.A`:
```csharp
namespace N3
{
    using R = N1.N2;

    class B: R.A {}
}
```

*Tanımlayıcı* , bir *using_alias_directive* derleme biriminde veya hemen içeren ad alanı bildirimi alanı içinde benzersiz olmalıdır *using_alias_directive* . Örneğin:
```csharp
namespace N3
{
    class A {}
}

namespace N3
{
    using A = N1.N2.A;        // Error, A already exists
}
```

Yukarıdaki `N3` zaten bir üye içeriyorsa `A`, bir derleme zamanı hatası için bu nedenle bir *using_alias_directive* tanımlayıcı kullanılacak. Benzer şekilde, iki veya daha fazlası için bir derleme zamanı hatası olduğu *using_alias_directive*aynı adlı diğer ad bildirmek için aynı derleme birimi veya ad alanı gövdesinde s.

A *using_alias_directive* bir derleme belirli birimi veya ad alanı gövdesi içindeki diğer kullanılabilir hale getirir, ancak tüm yeni üyeleri temel alınan bildirim alanına gereksinimdir. Diğer bir deyişle, bir *using_alias_directive* geçişli değildir ancak bunun yerine oluştuğu yalnızca derleme birimi veya ad alanı gövdesi etkiler. Örnekte
```csharp
namespace N3
{
    using R = N1.N2;
}

namespace N3
{
    class B: R.A {}            // Error, R unknown
}
```
kapsamı *using_alias_directive* sunan `R` yalnızca üye bildirimleri, bu yer alan, ad alanı gövdesinde genişleyen şekilde `R` ikinci ad alanı bildiriminde bilinmiyor. Ancak, yerleştirme *using_alias_directive* içeren derlemede, her iki ad alanı bildirimi içinde kullanılabilir olana kadar diğer birim neden olur:
```csharp
using R = N1.N2;

namespace N3
{
    class B: R.A {}
}

namespace N3
{
    class C: R.A {}
}
```

Yalnızca normal üyeler gibi adlar kullanılmaya tarafından *using_alias_directive*s, iç içe geçmiş kapsamlar benzer ada üyeleri tarafından gizlidir. Örnekte
```csharp
using R = N1.N2;

namespace N3
{
    class R {}

    class B: R.A {}        // Error, R has no member A
}
```
başvuru `R.A` bildiriminde `B` çünkü bir derleme zamanı hatasına neden olur `R` başvurduğu `N3.R`değil `N1.N2`.

Bir sırayı *using_alias_directive*s sahip hiçbir anlam ve çözünürlüğüne göre yazılmış *namespace_or_type_name* tarafından başvurulan bir *using_alias_directive*etkilenmez *using_alias_directive* kendisini veya diğer *using_directive*hemen içeren derleme birimi veya ad alanı gövdesinde s. Diğer bir deyişle, *namespace_or_type_name* , bir *using_alias_directive* hemen içeren derleme birimi veya ad alanı gövdesi yok varmış gibi çözümlenir *using_directive*s. A *using_alias_directive* ancak etkilenen *extern_alias_directive*hemen içeren derleme birimi veya ad alanı gövdesinde s. Örnekte
```csharp
namespace N1.N2 {}

namespace N3
{
    extern alias E;

    using R1 = E.N;        // OK

    using R2 = N1;         // OK

    using R3 = N1.N2;      // OK

    using R4 = R2.N2;      // Error, R2 unknown
}
```
Son *using_alias_directive* ilk tarafından etkilenmez, çünkü bir derleme zamanı hatası oluşur *using_alias_directive*. İlk *using_alias_directive* extern diğer adı kapsamını beri hatayla sonuçlanmaz `E` içerir *using_alias_directive*.

A *using_alias_directive* herhangi bir ad alanı veya tür, bu ad alanı içinde iç içe geçmiş ve herhangi bir ad alanı veya içinde geçtiği ad alanı dahil türü için bir diğer ad oluşturabilirsiniz.

Ad alanı veya tür bir diğer ad erişme, bu ad alanı veya tür, bildirilen ad erişme tam olarak aynı sonucu verir. Örneğin,
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using R1 = N1;
    using R2 = N1.N2;

    class B
    {
        N1.N2.A a;            // refers to N1.N2.A
        R1.N2.A b;            // refers to N1.N2.A
        R2.A c;               // refers to N1.N2.A
    }
}
```
adları `N1.N2.A`, `R1.N2.A`, ve `R2.A` olan eşdeğer ve tüm başvuru tam adı olan sınıfa `N1.N2.A`.

Diğer adların kullanımı kapalı bir oluşturulmuş tür adı, ancak tür bağımsız değişkenleri sağlamadan adı ilişkisiz genel tür bildirimi olamaz. Örneğin:
```csharp
namespace N1
{
    class A<T>
    {
        class B {}
    }
}

namespace N2
{
    using W = N1.A;          // Error, cannot name unbound generic type

    using X = N1.A.B;        // Error, cannot name unbound generic type

    using Y = N1.A<int>;     // Ok, can name closed constructed type

    using Z<T> = N1.A<T>;    // Error, using alias cannot have type parameters
}
```

### <a name="using-namespace-directives"></a>Ad alanı yönergeleri kullanma

A *using_namespace_directive* niteleme olmadan kullanılmasını için her tür tanımlayıcısını etkinleştirme kapsayan derleme birimi veya ad alanı gövdesine, bir ad alanında bulunan türleri içeri aktarır.

```antlr
using_namespace_directive
    : 'using' namespace_name ';'
    ;
```

Üye bildirimleri gövdesindeki içeren bir derleme birimi veya ad alanı içinde bir *using_namespace_directive*, verilen ad alanında yer alan tür doğrudan başvurulabilir. Örneğin:
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1.N2;

    class B: A {}
}
```

Yukarıdaki üye bildirimlerinde içinde `N3` ad alanı, tür üyelerinin `N1.N2` doğrudan kullanılabilir ve bu nedenle sınıf `N3.B` sınıfından türetilen `N1.N2.A`.

A *using_namespace_directive* verilen ad alanında bulunan türleri içeri aktarır, ancak iç içe ad alanlarını özellikle içeri aktarmaz. Örnekte
```csharp
namespace N1.N2
{
    class A {}
}

namespace N3
{
    using N1;

    class B: N2.A {}        // Error, N2 unknown
}
```
*using_namespace_directive* bulunan türleri içeri aktarır `N1`, ancak ad iç içe geçmiş `N1`. Bu nedenle, başvuru `N2.A` bildiriminde `B` üye yok adlı bir derleme zamanı hatası oluşur `N2` kapsam içindedir.

Farklı bir *using_alias_directive*, *using_namespace_directive* , tanımlayıcıları zaten kapsayan derleme birimi veya ad alanı gövdesinde tanımlanan türleri alabilirsiniz. Aslında, adları içeri aktarılan bir *using_namespace_directive* kapsayan derleme birimi veya ad alanı gövdesinde benzer ada üyeleri tarafından gizlenir. Örneğin:
```csharp
namespace N1.N2
{
    class A {}

    class B {}
}

namespace N3
{
    using N1.N2;

    class A {}
}
```

Burada, üye bildirimlerinde içinde `N3` ad alanı, `A` başvurduğu `N3.A` yerine `N1.N2.A`.

Birden fazla ad alanı veya tür alınan tarafından ne zaman *using_namespace_directive*s veya *using_static_directive*s aynı derleme birimi veya ad alanı gövdesinde türleri için aynı adla başvurular içerir Bu ada bir *type_name* belirsiz olarak kabul edilir. Örnekte
```csharp
namespace N1
{
    class A {}
}

namespace N2
{
    class A {}
}

namespace N3
{
    using N1;

    using N2;

    class B: A {}                // Error, A is ambiguous
}
```
her ikisi de `N1` ve `N2` bir üyeyi içeren `A`ve çünkü `N3` her ikisi de başvuru alır `A` içinde `N3` bir derleme zamanı hatasıdır. Bu durumda, çakışma ya da Nitelik yapılan başvuruların üzerinden çözümlenebilir `A`, ya da Giriş bir *using_alias_directive* , belirli bir seçer `A`. Örneğin:
```csharp
namespace N3
{
    using N1;

    using N2;

    using A = N1.A;

    class B: A {}                // A means N1.A
}
```

Ayrıca, ne zaman birden fazla ad alanı veya tür içe aktarılan tarafından *using_namespace_directive*s veya *using_static_directive*türleri veya üyeleri tarafından aynı derleme birimi veya ad alanı gövdesinde s içerir aynı ada başvuran o ada bir *simple_name* belirsiz olarak kabul edilir. Örnekte
```csharp
namespace N1
{
    class A {}
}

class C
{
    public static int A;
}

namespace N2
{
    using N1;
    using static C;

    class B
    {
        void M() 
        { 
            A a = new A();   // Ok, A is unambiguous as a type-name
            A.Equals(2);     // Error, A is ambiguous as a simple-name
        }
    }
}
```
`N1` bir tür üyesi içeren `A`, ve `C` statik bir yöntem içerir `A`ve çünkü `N2` her ikisi de başvuru alır `A` olarak bir *simple_name* belirsiz ve derleme zamanı bir hata oluştu. 

Gibi bir *using_alias_directive*, *using_namespace_directive* derleme biriminde veya ad alanı temel alınan bildirim alanına tüm yeni üyeleri katkıda değil, ancak bunun yerine yalnızca etkiler göründüğü derleme birimi veya ad alanı gövdesi.

*Namespace_name* tarafından başvurulan bir *using_namespace_directive* aynı şekilde çözümlenen *namespace_or_type_name* tarafından başvurulan bir  *using_alias_directive*. Bu nedenle, *using_namespace_directive*s aynı derleme birimi veya ad alanı gövdesinde birbirine etkilemez ve herhangi bir sırada yazılabilir.

### <a name="using-static-directives"></a>Statik yönergeleri kullanma

A *using_static_directive* her üyesi ve tür tanımlayıcısı etkinleştirme statik üyelerini doğrudan gövdesine kapsayan derleme birimi veya ad alanı, tür bildiriminde bulunan ve iç içe geçmiş türleri içeri aktarır niteleme olmadan kullanılır.

```antlr
using_static_directive
    : 'using' 'static' type_name ';'
    ;
```

Üye bildirimleri gövdesindeki içeren bir derleme birimi veya ad alanı içinde bir *using_static_directive*, erişilebilir iç içe türler ve doğrudan bildiriminde bulunan statik üyeler (hariç, genişletme yöntemleri) Belirtilen türü doğrudan başvurulabilir. Örneğin:

```csharp
namespace N1
{
    class A 
    {
        public class B{}
        public static B M(){ return new B(); }
    }
}

namespace N2
{
    using static N1.A;
    class C
    {
        void N() { B b = M(); }
    }
}
```

Yukarıdaki üye bildirimlerinde içinde `N2` ad alanı, iç içe geçmiş türleri ve statik üyeler `N1.A` doğrudan kullanılabilir ve bu nedenle yöntemi `N` her ikisi de başvuru bulabildiği `B` ve `M` üyeleri`N1.A`.

A *using_static_directive* özellikle genişletme yöntemleri statik yöntemler olarak doğrudan içeri aktarmaz, ancak uzantı metodu çağırma için kullanılabilir hale getirir ([uzantısı yöntem çağrıları](expressions.md#extension-method-invocations)). Örnekte

```csharp
namespace N1 
{
    static class A 
    {
        public static void M(this string s){}
    }
}

namespace N2
{
    using static N1.A;

    class B
    {
        void N() 
        {
            M("A");      // Error, M unknown
            "B".M();     // Ok, M known as extension method
            N1.A.M("C"); // Ok, fully qualified
        }
    }
}
```
*using_static_directive* genişletme yöntemini alır `M` bulunan `N1.A`, ancak yalnızca bir genişletme yöntemi. Bu nedenle, ilk başvuru `M` gövdesinde `B.N` üye yok adlı bir derleme zamanı hatası oluşur `M` kapsam içindedir.

A *using_static_directive* üyeleri ve türleri temel sınıflarda bildirilen üyeler ve doğrudan belirli türü bildirilmiş türleri yalnızca içeri aktarır.

TODO: Örnek

Birden çok arasında belirsizlikleri *using_namespace_directives* ve *using_static_directives* ele alınmıştır [ad alanı yönergeleri kullanarak](namespaces.md#using-namespace-directives).

## <a name="namespace-members"></a>Namespace üyelerini

A *namespace_member_declaration* ya da bir *namespace_declaration* ([Namespace bildirimi](namespaces.md#namespace-declarations)) veya bir *type_declaration* () [Tür bildirimleri](namespaces.md#type-declarations)).

```antlr
namespace_member_declaration
    : namespace_declaration
    | type_declaration
    ;
```

Bir derleme biriminde veya ad alanı gövdesi içerebilir *namespace_member_declaration*s ve bu tür bildirimleri katkıda yeni üye temel alınan bildirim alanına içeren derleme birimi veya ad alanı gövdesi.

## <a name="type-declarations"></a>Tür bildirimleri

A *type_declaration* olduğu bir *class_declaration* ([sınıf bildirimleri](classes.md#class-declarations)), *struct_declaration* ([yapısı bildirimleri](structs.md#struct-declarations)), bir *interface_declaration* ([arabirim bildirimleri](interfaces.md#interface-declarations)), bir *enum_declaration* ([sabit listesi bildirimleri](enums.md#enum-declarations)), veya bir *delegate_declaration* ([temsilci bildirimi](delegates.md#delegate-declarations)).

```antlr
type_declaration
    : class_declaration
    | struct_declaration
    | interface_declaration
    | enum_declaration
    | delegate_declaration
    ;
```

A *type_declaration* derleme biriminde bir üst düzey bildiriminde veya olarak bir üye bildirimi bir ad alanı, sınıf veya yapı içinde ortaya çıkabilir.

Bir tür için bir tür bildirimi olduğunda `T` gerçekleşir bir derleme biriminde bir üst düzey bildiriminde olarak yeni bildirilen türü tam adı olduğunu yalnızca `T`. Bir tür için bir tür bildirimi olduğunda `T` sınıf veya yapının yeni bildirilen türü tam adı ad alanı içinde gerçekleşir `N.T`burada `N` içeren ad alanı, sınıf veya yapı tam adıdır.

Bir tür bir sınıf içinde bildirilen veya yapı, iç içe geçmiş bir tür çağrılır ([türlerin](classes.md#nested-types)).

İzin verilen erişim değiştiricileri ve varsayılan erişim türü bildirimi için bildirimi yer alan bağlam üzerinde bağlıdır ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)):

*  Türleri derleme biriminden veya ad alanı bildirimi olabilir `public` veya `internal` erişim. Varsayılan değer `internal` erişim.
*  Sınıflarda bildirilen türleri olabilir `public`, `protected internal`, `protected`, `internal`, veya `private` erişim. Varsayılan değer `private` erişim.
*  Yapılar için bildirilen türleri olabilir `public`, `internal`, veya `private` erişim. Varsayılan değer `private` erişim.

## <a name="namespace-alias-qualifiers"></a>Namespace diğer adı niteleyicileri

***Ad alanı diğer ad niteleyicisi*** `::` tür adı aramaları yeni türler ve üyeler sunulmasıyla tarafından etkilenmez garanti mümkün kılar. Ad alanı diğer ad niteleyicisi her zaman sol ve sağ tanımlayıcı olarak başvurulan iki tanımlayıcı arasında görünür. Normal aksine `.` niteleyicisi, sol taraftaki tanımlayıcısını `::` niteleyicisi Aranan up yalnızca extern veya diğer ad kullanımı.

A *qualified_alias_member* şu şekilde tanımlanır:

```antlr
qualified_alias_member
    : identifier '::' identifier type_argument_list?
    ;
```

A *qualified_alias_member* olarak kullanılabilir bir *namespace_or_type_name* ([Namespace ve tür adları](basic-concepts.md#namespace-and-type-names)) veya sol işlenen olarak bir *member_access* ([Üye erişimi](expressions.md#member-access)).

A *qualified_alias_member* iki farklı vardır:

*  `N::I<A1, ..., Ak>`, burada `N` ve `I` tanımlayıcılarını temsil eder ve `<A1, ..., Ak>` türü bağımsız değişken listesi. (`K` her zaman en az bir olur.)
*  `N::I`, burada `N` ve `I` tanımlayıcılarını temsil eder. (Bu durumda, `K` sıfır olarak kabul edilir.)

Bu gösterim anlamını kullanılarak bir *qualified_alias_member* şu şekilde belirlenir:

*  Varsa `N` tanımlayıcı `global`, sonra da için genel ad aranır `I`:
   * Genel ad alanı adındaki bir ad içeriyorsa `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu isim uzayına başvuruyor.
   * Aksi durumda, genel olmayan bir tür adı genel ad içeriyorsa `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu türe başvuruyor.
   * Aksi takdirde genel ad adlı bir tür içeriyorsa `I` olan `K`  tür parametrelerindeki, ardından *qualified_alias_member* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.
   * Aksi takdirde, *qualified_alias_member* olup tanımsız ve bir derleme zamanı hatası oluşur.

*  Aksi takdirde, ad alanı bildirimi ile başlayan ([Namespace bildirimi](namespaces.md#namespace-declarations)) hemen içeren *qualified_alias_member* (varsa), her kapsayan ad alanı bildirimi ile devam ediliyor (varsa) ve içeren derleme birimi ile biten *qualified_alias_member*, aşağıdaki adımları bir varlığı bulunana kadar değerlendirilir:

   * Ad alanı bildirimi veya derleme birimi içeriyorsa, bir *using_alias_directive* , ilişkilendirir `N` bir türle sonra *qualified_alias_member* tanımlı değildir ve derleme zamanı hata oluşur.
   * Aksi takdirde ad alanı bildirimi veya derleme biriminin içeriyorsa bir *extern_alias_directive* veya *using_alias_directive* , ilişkilendirir `N` bir ad alanı, ardından:
     * Ad alanı ile ilişkili değilse `N` adlı bir ad alanı içerir `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu isim uzayına başvuruyor.
     * Aksi takdirde, ad alanı ile ilişkili `N` adlı bir genel olmayan tür içeren `I` ve `K` sıfırsa, ardından *qualified_alias_member* bu türe başvuruyor.
     * Aksi takdirde, ad alanı ile ilişkili `N` adlı bir tür içeren `I` olan `K`  tür parametrelerindeki, ardından *qualified_alias_member* türü ile oluşturulan ifade eder Belirtilen tür bağımsız değişkenleri.
     * Aksi takdirde, *qualified_alias_member* olup tanımsız ve bir derleme zamanı hatası oluşur.
*  Aksi takdirde, *qualified_alias_member* olup tanımsız ve bir derleme zamanı hatası oluşur.

Ad alanı diğer ad niteleyicisi bir türe başvuran bir diğer ad ile kullanarak bir derleme zamanı hatası neden olacağını unutmayın. Ayrıca tanımlayıcı unutmayın `N` olduğu `global`, olsa bile bir diğer ad ilişkilendirme kullanarak genel ad alanında arama gerçekleştirilir `global` türü veya ad alanı.

### <a name="uniqueness-of-aliases"></a>Diğer ad benzersizliğini

Extern diğer adlar için ayrı bildirim alanı her derleme birimi ve ad alanı gövdesi olan ve diğer adların kullanımı. Bu nedenle, bir extern diğer adı veya diğer ad kullanımı adını extern diğer adlar kümesi içinde benzersiz olmalıdır ve hemen içeren derleme birimi veya ad alanı gövdesinde bildirilen diğer adların kullanımı bir diğer ad bir tür veya ad alanı olduğu sürece aynı ada sahip izin verilen ediyorum Yalnızca kullanılan t `::` niteleyicisi.

Örnekte
```csharp
namespace N
{
    public class A {}

    public class B {}
}

namespace N
{
    using A = System.IO;

    class X
    {
        A.Stream s1;            // Error, A is ambiguous

        A::Stream s2;           // Ok
    }
}
```
adı `A` iki olası anlama sahip ikinci bir ad alanı gövdesinde olduğundan her iki sınıf `A` ve kullanarak diğer ad `A` kapsamındaki. Bu nedenle, kullanım `A` tam adda `A.Stream` belirsiz ve bir derleme zamanı hatası oluşmasına neden olur. Ancak, kullanım `A` ile `::` niteleyicisi olmayan bir hata nedeniyle `A` yalnızca bir ad alanı diğer adı aranır.
