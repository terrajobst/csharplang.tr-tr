# <a name="attributes"></a>Öznitelikler

C# dili çoğunu programda tanımlanmış varlıkları hakkında tanımlayıcı bilgileri belirtmek Programcı sağlar. Örneğin, bir sınıf içinde bir yöntemin erişilebilirliği ile tasarlayarak belirtilen *method_modifier*s `public`, `protected`, `internal`, ve `private`.

C# programcıları yeni tür adında bildirim temelli bilgileri imkan sağlayan ***öznitelikleri***. Programcıları sonra çeşitli program varlıklara öznitelikleri eklemek ve bir çalışma zamanı ortamında öznitelik bilgileri alınamıyor. Örneğin, bir çerçeve tanımlayabilir bir `HelpAttribute` belirli program öğelerini (örneğin, sınıflar ve metotlar) üzerinde bu program öğelerini belgelerinin bir eşleme sağlamak üzere yerleştirilebilecek bir özniteliği.

Öznitelikleri, öznitelik sınıflarının bildirimi tanımlanır ([öznitelik sınıfları](attributes.md#attribute-classes)), konumsal ve adlandırılmış parametreleri olabilir ([konumsal ve adlandırılmış parametreleri](attributes.md#positional-and-named-parameters)). Öznitelikleri, öznitelik özelliklerini kullanarak bir C# programı varlıklara eklenir ([öznitelik belirtimi](attributes.md#attribute-specification)) ve çalışma zamanında öznitelik örnekleri alınabilir ([öznitelik örneklerini](attributes.md#attribute-instances)).

## <a name="attribute-classes"></a>Öznitelik sınıfları

Soyut sınıftan türeyen bir sınıf `System.Attribute`, doğrudan veya dolaylı olarak olan bir ***öznitelik sınıfını***. Öznitelik sınıfı bildirimi tür yeni tanımlar ***özniteliği*** bir bildiriminde yerleştirilebilir. Kural gereği, öznitelik sınıfları bir sonekiyle adlandırılır `Attribute`. Kullanan bir öznitelik eklemek veya bu sonek atlayın.

### <a name="attribute-usage"></a>Öznitelik kullanımı

Öznitelik `AttributeUsage` ([AttributeUsage özniteliği](attributes.md#the-attributeusage-attribute)) bir öznitelik sınıfı nasıl kullanılabileceğini tanımlamak için kullanılır.

`AttributeUsage` konumsal bir parametreye sahiptir ([konumsal ve adlandırılmış parametreleri](attributes.md#positional-and-named-parameters)) üzerinde kullanılabileceği bildirimleri türlerini belirtmek bir öznitelik sınıfı sağlar. Örnek

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Interface)]
public class SimpleAttribute: Attribute 
{
    ...
}
```

adlı bir öznitelik sınıfı tanımlar `SimpleAttribute` üzerinde yerleştirilebileceğini *class_declaration*s ve *interface_declaration*yalnızca s. Örnek

```csharp
[Simple] class Class1 {...}

[Simple] interface Interface1 {...}
```

bazı kullanımlarını gösterir `Simple` özniteliği. Bu öznitelik adı ile tanımlanır ancak `SimpleAttribute`, bu öznitelik kullanıldığında `Attribute` soneki atlanabilir, kısa adı elde `Simple`. Bu nedenle, yukarıdaki örnek aşağıdaki anlam olarak eşdeğerdir:

```csharp
[SimpleAttribute] class Class1 {...}

[SimpleAttribute] interface Interface1 {...}
```

`AttributeUsage` adlandırılmış bir parametreye sahiptir ([konumsal ve adlandırılmış parametreleri](attributes.md#positional-and-named-parameters)) olarak adlandırılan `AllowMultiple`, öznitelik belirli bir varlık için birden çok kez belirtilip belirtilemeyeceğini belirtir. Varsa `AllowMultiple` sınıfı bir öznitelik için geçerlidir ve ardından bu öznitelik sınıfı olduğu bir ***çok kullanımı öznitelik sınıfı***ve bir varlık üzerinde birden çok kez belirtilebilir. Varsa `AllowMultiple` sınıfı için bir öznitelik false veya belirtilmemiş ve ardından bu öznitelik sınıfı olduğu bir ***tek kullanımlık öznitelik sınıfı***ve bir varlık üzerinde en fazla bir kez belirtilebilir.

Örnek

```csharp
using System;

[AttributeUsage(AttributeTargets.Class, AllowMultiple = true)]
public class AuthorAttribute: Attribute
{
    private string name;

    public AuthorAttribute(string name) {
        this.name = name;
    }

    public string Name {
        get { return name; }
    }
}
```

adında çok kullanım özniteliği bir sınıf tanımlar `AuthorAttribute`. Örnek

```csharp
[Author("Brian Kernighan"), Author("Dennis Ritchie")] 
class Class1
{
    ...
}
```

bir sınıf bildirimiyle birlikte iki kullanımlarını gösterir `Author` özniteliği.

`AttributeUsage` sahip olarak adlandırılan başka bir adlandırılmış parametre `Inherited`, öznitelik bir temel sınıf belirtildiğinde, o temel sınıftan türetilmiş sınıflar tarafından ayrıca devralınır olup olmadığını gösterir. Varsa `Inherited` sınıfı bir öznitelik için geçerlidir ve ardından bu özniteliği devralınır. Varsa `Inherited` sınıfı için bir öznitelik false sonra bu öznitelik devralınmaz. Belirtilmemişse, kendi varsayılan değer True'dur.

Öznitelik sınıfı `X` olmaması bir `AttributeUsage` özniteliği olarak kendisine bağlı

```csharp
using System;

class X: Attribute {...}
```

Aşağıdakine eşdeğerdir:

```csharp
using System;

[AttributeUsage(
    AttributeTargets.All,
    AllowMultiple = false,
    Inherited = true)
]
class X: Attribute {...}
```

### <a name="positional-and-named-parameters"></a>Konumsal ve adlandırılmış parametreleri

Öznitelik sınıfları olabilir ***konumsal parametreler*** ve ***adlandırılmış parametreleri***. Her ortak örnek oluşturucusu bir öznitelik sınıfı için konumsal parametreleri, öznitelik sınıfı için geçerli bir dizi tanımlar. Öznitelik sınıfı için adlandırılmış bir parametre, her bir statik olmayan ortak okuma-yazma alan ve öznitelik sınıfı özelliği tanımlar.

Örnek

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpAttribute: Attribute
{
    public HelpAttribute(string url) {        // Positional parameter
        ...
    }

    public string Topic {                     // Named parameter
        get {...}
        set {...}
    }

    public string Url {
        get {...}
    }
}
```

adlı bir öznitelik sınıfı tanımlar `HelpAttribute` konumsal bir parametreye sahip `url`ve bir adlandırılmış parametre `Topic`. Statik olmayan ve ortak, özelliği, olmasına rağmen `Url` okuma-yazma olmadığından adlandırılmış bir parametre tanımlamıyor.

Bu öznitelik sınıfı aşağıdaki gibi kullanılabilir:

```csharp
[Help("http://www.mycompany.com/.../Class1.htm")]
class Class1
{
    ...
}

[Help("http://www.mycompany.com/.../Misc.htm", Topic = "Class2")]
class Class2
{
    ...
}
```

### <a name="attribute-parameter-types"></a>Öznitelik parametre türleri

Öznitelik sınıfı için konumsal ve adlandırılmış parametrelerden tür sınırlı ***öznitelik parametre türleri***, hangi:

*  Aşağıdaki türlerden biri: `bool`, `byte`, `char`, `double`, `float`, `int`, `long`, `sbyte`, `short`, `string`, `uint`, `ulong`, `ushort`.
*  Türü `object`.
*  Türü `System.Type`.
*  Bir sabit listesi türünde sağlanan ortak erişilebilirlik vardır ve ortak erişilebilirlik, onu iç içe (varsa) türler de ([öznitelik belirtimi](attributes.md#attribute-specification)).
*  Tek boyutlu diziler yukarıdaki türleri.
*  Bir öznitelik belirtiminde konumsal veya adlandırılmış bir parametre olarak bir oluşturucu bağımsız değişkeni ya da bu türlerden birinde olmayan ortak alan kullanılamaz.

## <a name="attribute-specification"></a>Öznitelik belirtimi

***Öznitelik belirtimi*** önceden tanımlanmış bir öznitelik için bir bildirim uygulamasıdır. Bir öznitelik bildirimi belirtilen ek bildirim temelli bilgi parçasıdır. Öznitelikler (içeren derleme veya modül öznitelikleri belirtmek için), genel kapsamda belirtilebilir ve *type_declaration*s ([tür bildirimleri](namespaces.md#type-declarations)), *class_member_declaration* s ([tür parametresi kısıtlamaları](classes.md#type-parameter-constraints)), *interface_member_declaration*s ([arabirim üyeleri](interfaces.md#interface-members)), *struct_member _declaration*s ([Yapı üyeleri](structs.md#struct-members)), *enum_member_declaration*s ([numaralandırma üyeleri](enums.md#enum-members)), *accessor_declarations*  ([Erişimcileri](classes.md#accessors)), *event_accessor_declarations* ([alan benzeri olaylara](classes.md#field-like-events)), ve *formal_parameter_list*s ([yöntem parametreleri](classes.md#method-parameters)).

Öznitelikleri belirtilir ***bölümlerde öznitelik***. Bir öznitelik bölümü bir veya daha fazla öznitelik virgülle ayrılmış bir listesini çevreleyen köşeli ayraçların bir çiftinden oluşur. Böyle bir listede öznitelikleri belirtildiğinde ve bölümler aynı program varlığa ekli sırasını düzenlenir sırası önemli değildir. Örneğin, öznitelik özelliklerini `[A][B]`, `[B][A]`, `[A,B]`, ve `[B,A]` eşdeğerdir.

```antlr
global_attributes
    : global_attribute_section+
    ;

global_attribute_section
    : '[' global_attribute_target_specifier attribute_list ']'
    | '[' global_attribute_target_specifier attribute_list ',' ']'
    ;

global_attribute_target_specifier
    : global_attribute_target ':'
    ;

global_attribute_target
    : 'assembly'
    | 'module'
    ;

attributes
    : attribute_section+
    ;

attribute_section
    : '[' attribute_target_specifier? attribute_list ']'
    | '[' attribute_target_specifier? attribute_list ',' ']'
    ;

attribute_target_specifier
    : attribute_target ':'
    ;

attribute_target
    : 'field'
    | 'event'
    | 'method'
    | 'param'
    | 'property'
    | 'return'
    | 'type'
    ;

attribute_list
    : attribute (',' attribute)*
    ;

attribute
    : attribute_name attribute_arguments?
    ;

attribute_name
    : type_name
    ;

attribute_arguments
    : '(' positional_argument_list? ')'
    | '(' positional_argument_list ',' named_argument_list ')'
    | '(' named_argument_list ')'
    ;

positional_argument_list
    : positional_argument (',' positional_argument)*
    ;

positional_argument
    : attribute_argument_expression
    ;

named_argument_list
    : named_argument (','  named_argument)*
    ;

named_argument
    : identifier '=' attribute_argument_expression
    ;

attribute_argument_expression
    : expression
    ;
```

Bir öznitelik oluşan bir *attrıbute_name* ve konumsal ve adlandırılmış bağımsız değişken isteğe bağlı bir liste. Adlandırılmış bağımsız değişkenler konumsal bağımsız değişkenler (varsa) koyun. Konumsal bağımsız değişken içeren bir *attribute_argument_expression*; ardından bir eşittir işareti tarafından izlenen bir ad, adlandırılmış bir bağımsız değişken oluşan bir *attribute_argument_expression*, birlikte , basit atama gibi aynı kurallara göre kısıtlanır. Adlandırılmış bağımsız değişkenlerin sırası önemli değildir.

*Attrıbute_name* öznitelik sınıfı tanımlar. Varsa biçiminde *attrıbute_name* olduğu *type_name* sonra bu adı için bir öznitelik sınıfı başvurmalıdır. Aksi takdirde, bir derleme zamanı hatası oluşur. Örnek

```csharp
class Class1 {}

[Class1] class Class2 {}    // Error
```

sonuçları bir derleme zamanı hatası kullanmaya çalışır çünkü `Class1` öznitelik olarak sınıf `Class1` öznitelik sınıfı değil.

Belirli bağlamlarda birden fazla hedef üzerinde bir öznitelik belirtimi izin verir. Bir program açıkça hedef dahil ederek belirtebilirsiniz bir *attribute_target_specifier*. Bir öznitelik genel düzeyde yerleştirildiğinde bir *global_attribute_target_specifier* gereklidir. Diğer tüm konumlarda makul bir varsayılan uygulanır, ancak bir *attribute_target_specifier* onaylayabilir veya belirsiz belirli durumlarda varsayılan geçersiz kılma (veya varsayılan Belirsiz olmayan bir durumda yalnızca onaylayabilir) kullanılabilir. Bu nedenle, genellikle *attribute_target_specifier*s atlanabilir dışında genel düzeyde. Belirsiz olası bağlamları gibi çözümlenir:

*  Genel kapsamda belirtilen bir özniteliği hedef derleme veya hedef modülü uygulayabilirsiniz. Varsayılan bu nedenle bu bağlam için mevcut bir *attribute_target_specifier* bu bağlamda her zaman gereklidir. Varlığını `assembly` *attribute_target_specifier* öznitelik hedefi için geçerli olduğunu gösterir derleme; varlığını `module` *attribute_target_specifier* öznitelik hedef modülü için geçerli olduğunu gösterir.
*  Delegate bildiriminde belirtilen öznitelik bildirilen temsilci veya dönüş değeri uygulayabilirsiniz. Mevcut olmadığında bir *attribute_target_specifier*, temsilciye özniteliğini uygular. Varlığını `type` *attribute_target_specifier* öznitelik temsilciye; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.
*  Özniteliğin yöntem bildiriminde belirtilen bildirilen yöntemi veya dönüş değeri uygulayabilirsiniz. Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular. Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* gösterir öznitelik dönüş değerini olmasını uygular.
*  Bir operatörü bildiriminde belirtilen öznitelik bildirilen işleci veya dönüş değerine uygulayabilirsiniz. Mevcut olmadığında bir *attribute_target_specifier*, öznitelik işlecini uygular. Varlığını `method` *attribute_target_specifier* öznitelik işleci; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.
*  Olay erişimcileri atlar olay bildiriminde belirtilen bir öznitelik, bildirilen olay, (olay soyut değilse) ilişkilendirilmiş alan veya ilişkili Ekle uygulamak ve yöntemleri kaldırın. Mevcut olmadığında bir *attribute_target_specifier*, özniteliği olaya uygulanır. Varlığını `event` *attribute_target_specifier* öznitelik olayı için geçerli olduğunu gösterir varlığını `field` *attribute_target_specifier* gösterir öznitelik alanı uyguladığı; ve varlığından destek alan `method` *attribute_target_specifier* öznitelik yöntemleri için geçerli olduğunu gösterir.
*  Get erişimcisi bildiriminde bir özellik veya dizin oluşturucu bildirimi için belirtilen bir öznitelik veya ilişkili yöntem dönüş değerine uygulayabilirsiniz. Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular. Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `return` *attribute_target_specifier* gösterir öznitelik dönüş değerini olmasını uygular.
*  İlişkili yöntem veya silmenizin, örtük parametresinin ayarlama erişimcisine bir özellik veya dizin oluşturucu bildirimi için belirtilen bir öznitelik uygulayabilirsiniz. Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular. Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `param` *attribute_target_specifier* gösterir öznitelik parametre uyguladığı; varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.
*  İlişkili yöntem veya silmenizin, parametresinin bir olay bildirimi uygulayabilirsiniz için bir ekleme veya kaldırma erişimcisi bildiriminde belirtilen bir öznitelik. Mevcut olmadığında bir *attribute_target_specifier*, yönteme özniteliğini uygular. Varlığını `method` *attribute_target_specifier* öznitelik yönteme; geçerli olduğunu gösterir varlığını `param` *attribute_target_specifier* gösterir öznitelik parametre uyguladığı; varlığını `return` *attribute_target_specifier* öznitelik dönüş değerini geçerli olduğunu gösterir.

Diğer bağlamlarda eklenmesi, bir *attribute_target_specifier* izin verilen ancak gereksiz. Örneğin, bir sınıf bildirimi dahil etmek veya belirticisini atlayan `type`:

```csharp
[type: Author("Brian Kernighan")]
class Class1 {}

[Author("Dennis Ritchie")]
class Class2 {}
```

Geçersiz bir belirtmek için bir hata olduğunu *attribute_target_specifier*. Örneğin, belirleyici `param` bir sınıf bildiriminde kullanılamaz:

```csharp
[param: Author("Brian Kernighan")]        // Error
class Class1 {}
```

Kural gereği, öznitelik sınıfları bir sonekiyle adlandırılır `Attribute`. Bir *attrıbute_name* formun *type_name* , bu ekini ya da içerir. Öznitelik sınıfı ile ve bu son ek olmadan bulunursa, bir belirsizlik var ve derleme zamanı hatası oluşur. Varsa *attrıbute_name* yazıldığından emin olacak şekilde, en sağdaki *tanımlayıcısı* verbatim bir tanımlayıcıdır ([tanımlayıcıları](lexical-structure.md#identifiers)), yalnızca bir öznitelik bir soneki olmadan eşleşen sonra Bu nedenle böyle bir belirsizlik çözülmesi etkinleştiriliyor. Örnek

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class X: Attribute
{}

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Error: ambiguity
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Refers to X
class Class3 {}

[@XAttribute]           // Refers to XAttribute
class Class4 {}
```

iki öznitelik sınıflar adlandırılan gösterir `X` ve `XAttribute`. Öznitelik `[X]` ya da başvurabileceğiniz beri belirsiz `X` veya `XAttribute`. Tam tanımlayıcı kullanarak tam hedefi gibi nadir durumlarda belirtilmesine olanak sağlar. Öznitelik `[XAttribute]` belirsiz değil (adlandırılmış bir öznitelik sınıfı olup olmadığını olacak olsa da `XAttributeAttribute`!). Sınıf bildirimi `X` kaldırılır, ardından her iki öznitelik adlı öznitelik sınıfına başvurmak `XAttribute`gibi:

```csharp
using System;

[AttributeUsage(AttributeTargets.All)]
public class XAttribute: Attribute
{}

[X]                     // Refers to XAttribute
class Class1 {}

[XAttribute]            // Refers to XAttribute
class Class2 {}

[@X]                    // Error: no attribute named "X"
class Class3 {}
```

Bu bir tek kullanımlık öznitelik sınıfı aynı varlığın birden çok kez kullanmak için bir derleme zamanı hatasıdır. Örnek

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class HelpStringAttribute: Attribute
{
    string value;

    public HelpStringAttribute(string value) {
        this.value = value;
    }

    public string Value {
        get {...}
    }
}

[HelpString("Description of Class1")]
[HelpString("Another description of Class1")]
public class Class1 {}
```

sonuçları bir derleme zamanı hatası kullanmaya çalışır çünkü `HelpString`, olduğu bir tek kullanımlık öznitelik sınıfı birden çok kez beyanı `Class1`.

Bir ifade `E` olduğu bir *attribute_argument_expression* tümü aşağıdaki deyimleri doğruysa:

*  Türünü `E` bir öznitelik parametresi türü ([öznitelik parametre türleri](attributes.md#attribute-parameter-types)).
*  Derleme zamanında, değerini `E` aşağıdakilerden birini çözülebilir:
   * Sabit bir değer.
   * A `System.Type` nesne.
   * Tek boyutlu bir dizi *attribute_argument_expression*s.

Örneğin:

```csharp
using System;

[AttributeUsage(AttributeTargets.Class)]
public class TestAttribute: Attribute
{
    public int P1 {
        get {...}
        set {...}
    }

    public Type P2 {
        get {...}
        set {...}
    }

    public object P3 {
        get {...}
        set {...}
    }
}

[Test(P1 = 1234, P3 = new int[] {1, 3, 5}, P2 = typeof(float))]
class MyClass {}
```

A *typeof_expression* ([typeof işleci](expressions.md#the-typeof-operator)) bir öznitelik bağımsız değişken ifadesi genel olmayan bir tür, kapalı bir oluşturulmuş tür ya da bağlanmamış bir genel tür başvurabilir, ancak başvuruda bulunulamaz olarak kullanılan bir açık tür. Bu ifadenin derleme zamanında çözülebilir sağlamaktır.

```csharp
class A: Attribute
{
    public A(Type t) {...}
}

class G<T>
{
    [A(typeof(T))] T t;                  // Error, open type in attribute
}

class X
{
    [A(typeof(List<int>))] int x;        // Ok, closed constructed type
    [A(typeof(List<>))] int y;           // Ok, unbound generic type
}
```

## <a name="attribute-instances"></a>Öznitelik örnekleri

Bir ***öznitelik örneği*** çalışma zamanında bir özniteliği temsil eder bir örneğidir. Bir öznitelik konumsal bağımsız değişkenler, bir öznitelik sınıfı ile tanımlanır ve adlandırılmış bağımsız değişkenler. Bir öznitelik örneği konumsal ve adlandırılmış bağımsız değişkenler ile başlatılan sınıfının bir örneğidir.

Bir öznitelik örneği alınmasını, aşağıdaki bölümlerde açıklandığı gibi hem derleme zamanı ve çalışma zamanı işleme içerir.

### <a name="compilation-of-an-attribute"></a>Bir özniteliğin derleme

Derlemesi bir *özniteliği* öznitelik sınıfı ile `T`, *positional_argument_list* `P` ve *named_argument_list* `N`, aşağıdaki adımlardan oluşur:

*  Derleme için derleme zamanı işleme adımları bir *object_creation_expression* formun `new T(P)`. Bu adımları neden bir derleme zamanı hatası ya da bir örnek oluşturucusunda belirlemek `C` üzerinde `T` , çağrılacak çalışma zamanında.
*  Varsa `C` bir derleme zamanı hatası oluşursa, ortak erişilebilirlik yok.
*  Her *named_argument* `Arg` içinde `N`:
   * İzin `Name` olması *tanımlayıcı* , *named_argument* `Arg`.
   * `Name` Statik olmayan okuma-yazma ortak alan veya özellik üzerinde tanımlamalıdır `T`. Varsa `T` böyle bir alan veya özellik, bir derleme zamanı hatası oluşursa sahiptir.
*  Özniteliğin çalışma zamanı örneklemesi için aşağıdaki bilgileri tutun: öznitelik sınıfı `T`, örnek oluşturucusu `C` üzerinde `T`, *positional_argument_list* `P` ve *named_argument_list* `N`.

### <a name="run-time-retrieval-of-an-attribute-instance"></a>Çalışma zamanı alınmasını öznitelik örneği

Derleme, bir *özniteliği* öznitelik sınıfı verir `T`, örnek oluşturucusu `C` üzerinde `T`, *positional_argument_list* `P`ve *named_argument_list* `N`. Bu bilgiler verildiğinde, bir öznitelik örneği aşağıdaki adımları kullanarak zamanında alınabilir:

*  Yürütme için çalışma zamanı işleme adımları bir *object_creation_expression* formun `new T(P)`, örnek Oluşturucusu kullanarak `C` derleme zamanında belirlendiği. Bu adımları bir özel durum neden ya da bir örnek `O` , `T`.
*  Her *named_argument* `Arg` içinde `N`, sırayla:
   * İzin `Name` olması *tanımlayıcı* , *named_argument* `Arg`. Varsa `Name` bir statik olmayan, genel okuma-yazma alan veya özellik üzerinde tanımlamaz `O`, sonra da bir özel durum oluşturulur.
   * İzin `Value` değerlendirme sonucu *attribute_argument_expression* , `Arg`.
   * Varsa `Name` üzerinde bir alan tanımlar `O`, ardından bu alan kümesine `Value`.
   * Aksi takdirde, `Name` üzerindeki bir özelliğini tanımlar `O`. Bu özellik kümesine `Value`.
   * Sonuç `O`, öznitelik sınıfının bir örneğini `T` ile başlatıldıysa *positional_argument_list* `P` ve *named_argument_list* `N`.

## <a name="reserved-attributes"></a>Ayrılmış öznitelikleri

Öznitelikleri az sayıda dil şekilde etkiler. Bu öznitelikler içerir:

*  `System.AttributeUsageAttribute` ([AttributeUsage özniteliği](attributes.md#the-attributeusage-attribute)), öznitelik sınıfı kullanılabilecek yöntemleri tanımlamak için kullanılır.
*  `System.Diagnostics.ConditionalAttribute` ([Conditional özniteliği](attributes.md#the-conditional-attribute)), koşullu yöntemler tanımlamak için kullanılır.
*  `System.ObsoleteAttribute` ([Geçersiz öznitelik](attributes.md#the-obsolete-attribute)), üye artık kullanılmıyor olarak işaretlemek için kullanılır.
*  `System.Runtime.CompilerServices.CallerLineNumberAttribute`, `System.Runtime.CompilerServices.CallerFilePathAttribute` ve `System.Runtime.CompilerServices.CallerMemberNameAttribute` ([arayan bilgileri öznitelikleri](attributes.md#caller-info-attributes)), isteğe bağlı parametreler için arama bağlamı hakkında bilgi sağlamak için kullanılır.

### <a name="the-attributeusage-attribute"></a>AttributeUsage özniteliği

Öznitelik `AttributeUsage` öznitelik sınıfı kullanılabilecek şekilde tanımlamak için kullanılır.

İle donatılmış sınıf `AttributeUsage` özniteliği türetilmesi gereken `System.Attribute`, doğrudan veya dolaylı olarak. Aksi takdirde, bir derleme zamanı hatası oluşur.

```csharp
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    public class AttributeUsageAttribute: Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn) {...}
        public virtual bool AllowMultiple { get {...} set {...} }
        public virtual bool Inherited { get {...} set {...} }
        public virtual AttributeTargets ValidOn { get {...} }
    }

    public enum AttributeTargets
    {
        Assembly     = 0x0001,
        Module       = 0x0002,
        Class        = 0x0004,
        Struct       = 0x0008,
        Enum         = 0x0010,
        Constructor  = 0x0020,
        Method       = 0x0040,
        Property     = 0x0080,
        Field        = 0x0100,
        Event        = 0x0200,
        Interface    = 0x0400,
        Parameter    = 0x0800,
        Delegate     = 0x1000,
        ReturnValue  = 0x2000,

        All = Assembly | Module | Class | Struct | Enum | Constructor | 
            Method | Property | Field | Event | Interface | Parameter | 
            Delegate | ReturnValue
    }
}
```

### <a name="the-conditional-attribute"></a>Conditional özniteliği

Öznitelik `Conditional` tanımını sağlar ***koşullu yöntemler*** ve ***koşullu öznitelik sınıfları***.

```csharp
namespace System.Diagnostics
{
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Class, AllowMultiple = true)]
    public class ConditionalAttribute: Attribute
    {
        public ConditionalAttribute(string conditionString) {...}
        public string ConditionString { get {...} }
    }
}
```

#### <a name="conditional-methods"></a>Koşullu yöntemler

Bir yöntem ile donatılmış `Conditional` özniteliği koşullu bir yöntemdir. `Conditional` Özniteliği, koşullu derleme sembol test ederek bir koşulu belirtir. Bir koşullu metot çağrıları dahil veya bu sembol çağrısı noktasında tanımlı olup olmadığı bağlı olarak atlanmış. Simge tanımlanmışsa çağrısı bulunur; Aksi takdirde (alıcı değerlendirmesini ve çağrının parametreleri dahil) çağrısı atlandı.

Aşağıdaki sınırlamalara tabi buna koşullu bir yöntemdir:

*  Koşullu yöntem bir yöntemin olmalıdır bir *class_declaration* veya *struct_declaration*. Bir derleme zamanı hatası oluşur `Conditional` öznitelik, bir Metoda bir arabirim bildirimi belirtilir.
*  Koşullu yöntem dönüş türüne sahip olmalıdır `void`.
*  Koşullu yöntemi ile işaretlenmemelidir `override` değiştiricisi. Koşullu bir yöntem ile işaretlenmiş olabilir `virtual` değiştiricisi, ancak. Böyle bir yöntemi geçersiz kılmaları örtük olarak koşullu ve açıkça ile işaretlenmemelidir bir `Conditional` özniteliği.
*  Koşullu yöntem bir arabirim yöntemin bir uygulaması, olmamalıdır. Aksi takdirde, bir derleme zamanı hatası oluşur.

Ayrıca, koşullu yöntem kullanılırsa derleme zamanı hatası oluşur. bir *delegate_creation_expression*. Örnek

```csharp
#define DEBUG

using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void M() {
        Console.WriteLine("Executed Class1.M");
    }
}

class Class2
{
    public static void Test() {
        Class1.M();
    }
}
```

bildirir `Class1.M` koşullu bir yöntem olarak. `Class2`ın `Test` yöntemi bu yöntemi çağırır. Koşullu derleme sembol beri `DEBUG` durumunda tanımlanır `Class2.Test` olan çağrılır, bunu çağıracak `M`. Sembol `DEBUG` , ardından tanımlanmış olmayan `Class2.Test` değil çağırırsınız `Class1.M`.

Ekleme veya dışlama koşullu bir yöntem çağrısına çağrısı noktasında koşullu derleme simgeleri tarafından denetlendiğini unutmayın önemlidir. Örnekte

Dosya `class1.cs`:

```csharp
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public static void F() {
        Console.WriteLine("Executed Class1.F");
    }
}
```

Dosya `class2.cs`:

```csharp
#define DEBUG

class Class2
{
    public static void G() {
        Class1.F();                // F is called
    }
}
```

Dosya `class3.cs`:

```csharp
#undef DEBUG

class Class3
{
    public static void H() {
        Class1.F();                // F is not called
    }
}
```

sınıfları `Class2` ve `Class3` her içeren koşullu yöntemine yönelik çağrılar `Class1.F`, koşullu olduğu olmamasına tabanlı `DEBUG` tanımlanır. Bu sembol bağlamında tanımlamış `Class2` ama `Class3`, çağrı `F` içinde `Class2` çağrısı sırasında bulunan `F` içinde `Class3` atlanır.

Devralma zincirinde koşullu yöntemler kullanımını kafa karıştırıcı olabilir. Koşullu bir yöntem aracılığıyla yapılan çağrıları `base`, formun `base.M`, normal bir koşullu metot çağrısı kurallarına tabidir. Örnekte

Dosya `class1.cs`:

```csharp
using System;
using System.Diagnostics;

class Class1 
{
    [Conditional("DEBUG")]
    public virtual void M() {
        Console.WriteLine("Class1.M executed");
    }
}
```

Dosya `class2.cs`:

```csharp
using System;

class Class2: Class1
{
    public override void M() {
        Console.WriteLine("Class2.M executed");
        base.M();                        // base.M is not called!
    }
}
```

Dosya `class3.cs`:

```csharp
#define DEBUG

using System;

class Class3
{
    public static void Test() {
        Class2 c = new Class2();
        c.M();                            // M is called
    }
}
```

`Class2` bir çağrı içerdiğine `M` , temel sınıfta tanımlanan. Taban yönteminin sembolün varlığını temel alarak koşullu olduğu için bu çağrıyı atlanırsa `DEBUG`, tanımsız olduğu. Bu nedenle, yöntem, konsola yazar "`Class2.M executed`" yalnızca. Kullanmalıdır *pp_declaration*s, bu tür sorunları ortadan kaldırabilir.

#### <a name="conditional-attribute-classes"></a>Conditional özniteliği sınıfları

Öznitelik sınıfı ([öznitelik sınıfları](attributes.md#attribute-classes)) biri veya daha fazlası ile donatılmış `Conditional` öznitelikleri bir ***koşullu öznitelik sınıfı***. Conditional özniteliği sınıfı ile bildirilen koşullu derleme simgeleri nedenle ilişkilidir, `Conditional` öznitelikleri. Bu örnekte:

```csharp
using System;
using System.Diagnostics;
[Conditional("ALPHA")]
[Conditional("BETA")]
public class TestAttribute : Attribute {}
```

bildirir `TestAttribute` koşullu derleme simgeleri ile ilişkili bir koşullu öznitelik sınıfı olarak `ALPHA` ve `BETA`.

Öznitelik belirtimleri ([öznitelik belirtimi](attributes.md#attribute-specification)) bir veya daha fazla kendi ilişkili koşullu derleme simgeleri belirtimi noktasında, aksi takdirde tanımlanmışsa conditional özniteliği dahil edilen özniteliği belirtimi atlanır.

Ekleme veya çıkarma bir öznitelik belirtimi bir conditional özniteliği sınıfın belirtimi noktasında koşullu derleme simgeleri tarafından denetlendiğini unutmayın önemlidir. Örnekte

Dosya `test.cs`:

```csharp
using System;
using System.Diagnostics;

[Conditional("DEBUG")]

public class TestAttribute : Attribute {}
```

Dosya `class1.cs`:

```csharp
#define DEBUG

[Test]                // TestAttribute is specified

class Class1 {}
```

Dosya `class2.cs`:

```csharp
#undef DEBUG

[Test]                 // TestAttribute is not specified

class Class2 {}
```

sınıfları `Class1` ve `Class2` olan her özniteliği ile donatılmış `Test`, koşullu olduğu olmamasına tabanlı `DEBUG` tanımlanır. Bu sembol bağlamında tanımlamış `Class1` ama `Class2`, belirtimi `Test` özniteliği `Class1` , belirtimi sırasında bulunan `Test` özniteliği `Class2` atlanır.

### <a name="the-obsolete-attribute"></a>Geçersiz öznitelik

Öznitelik `Obsolete` türleri ve üyeleri artık kullanılması gereken türleri işaretlemek için kullanılır.

```csharp
namespace System
{
    [AttributeUsage(
        AttributeTargets.Class | 
        AttributeTargets.Struct |
        AttributeTargets.Enum | 
        AttributeTargets.Interface | 
        AttributeTargets.Delegate |
        AttributeTargets.Method | 
        AttributeTargets.Constructor |
        AttributeTargets.Property | 
        AttributeTargets.Field |
        AttributeTargets.Event,
        Inherited = false)
    ]
    public class ObsoleteAttribute: Attribute
    {
        public ObsoleteAttribute() {...}
        public ObsoleteAttribute(string message) {...}
        public ObsoleteAttribute(string message, bool error) {...}
        public string Message { get {...} }
        public bool IsError { get {...} }
    }
}
```

Bir program türü veya ile donatılmış üye kullanıp kullanmadığını `Obsolete` özniteliği, derleyici sorunları bir uyarı veya hata. Özellikle, derleyici hatası parametre sağlanmazsa veya hata parametresi sağlanır ve değere sahipse bir uyarı verir `false`. Hata parametresi belirtilmişse ve değerine sahip, derleyici bir hata verir `true`.

Örnekte

```csharp
[Obsolete("This class is obsolete; use class B instead")]
class A
{
    public void F() {}
}

class B
{
    public void F() {}
}

class Test
{
    static void Main() {
        A a = new A();         // Warning
        a.F();
    }
}
```

sınıf `A` ile donatılmış `Obsolete` özniteliği. Her kullanımının `A` içinde `Main` belirtilen iletiyi içeren bir uyarı sonuçları, "Bu sınıf artık kullanılmıyor; B sınıfı kullanın."

### <a name="caller-info-attributes"></a>Arayan bilgileri öznitelikleri

Günlüğe kaydetme ve raporlama gibi amaçlarla belirli çağıran kod derleme zamanı bilgilerini almak işlev üyesi bazen kullanışlıdır. Arayan bilgileri öznitelikleri gibi bilgileri şeffaf bir şekilde geçirmek için bir yol sağlar.

İsteğe bağlı parametresi arayan bilgileri öznitelikleri biriyle açıklandığında bir çağrıda karşılık gelen bağımsız değişken atlama mutlaka yerine kullanılacak varsayılan parametre değeri neden olmaz. Bunun yerine, arama bağlamı hakkında belirli bilgi varsa, bu bilgileri bağımsız değişken değeri geçirilir.

Örneğin:

```csharp
using System.Runtime.CompilerServices

...

public void Log(
    [CallerLineNumber] int line = -1,
    [CallerFilePath]   string path = null,
    [CallerMemberName] string name = null
)
{
    Console.WriteLine((line < 0) ? "No line" : "Line "+ line);
    Console.WriteLine((path == null) ? "No file path" : path);
    Console.WriteLine((name == null) ? "No member name" : name);
}
```

Bir çağrı `Log()` bağımsız değişken olmadan satır numarasını ve dosya yolu çağrısının yanı sıra, çağrı içinde gerçekleştiği üyesinin adı yazdırır.

Arayan bilgisi öznitelikleri, temsilci bildirimlerinde de dahil olmak üzere herhangi bir yerde, isteğe bağlı parametreler ortaya çıkabilir. Ancak belirli arayan bilgileri öznitelikleri her zaman olmayacaktır parametre türü bir yedek değerinden örtük dönüştürmeleri böylece bunlar özniteliği parametre türlerine kısıtlamaları vardır.

Bu hem tanımlama ve kısmi yöntem bildiriminin bir parçası uygulama parametresi aynı çağıran bilgisi özniteliği için bir hatadır. Arayan bilgileri öznitelikleri yalnızca uygulama bölümünde gerçekleşen göz ardı edilir ancak yalnızca arayan bilgisi özniteliklerini tanımlayan bir parçası olarak uygulanır.

Arayan bilgileri aşırı yükleme çözünürlüğü etkilemez. Öznitelikli isteğe bağlı parametreler hala çağıran kaynak kodundan atlanır gibi aşırı yükleme çözünürlüğü Bu parametre belirtilmemişse diğer isteğe bağlı parametreleri yoksayar aynı şekilde yok sayar. ([aşırı yükleme çözünürlüğü](expressions.md#overload-resolution)).

Kaynak kodunda açıkça bir işlev çağrıldığında arayan bilgileri yalnızca konur. Örtük çağrıları örtük üst Oluşturucu çağrıları gibi bir kaynak konumu yok ve arayan bilgileri alternatif değildir. Ayrıca, dinamik olarak bağlı çağrıları arayan bilgileri yerine değil. Parametrenin belirtilen varsayılan değeri, bu gibi durumlarda öznitelikli parametre atlanırsa bir arayan bilgisi, bunun yerine kullanılır.

Sorgu ifadeleri bir işlemdir. Bu söz dizimi genişletmeleri kabul edilir ve çağrıları bunlar arayan bilgileri öznitelikleri isteğe bağlı parametrelerle atlamak için genişletirseniz, arayan bilgileri değiştirilecektir. Kullanılan konumu çağrı oluşturulan sorgu yan tümcesi konumudur.

Verilen bir parametre üzerinde birden fazla çağıran bilgisi özniteliği belirtilmezse, bunlar şu sırayla tercih edilen: `CallerLineNumber`, `CallerFilePath`, `CallerMemberName`.

#### <a name="the-callerlinenumber-attribute"></a>CallerLineNumber özniteliği

`System.Runtime.CompilerServices.CallerLineNumberAttribute` Standart bir örtük dönüştürme olduğunda isteğe bağlı parametrelere izin ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) sabit değerinden `int.MaxValue` parametrenin türü. Bu, bu değer kadar herhangi bir negatif olmayan satır numarasına hatasız geçirilebilir sağlar.

İsteğe bağlı bir parametre ile bir konumda bir kaynak kodu bir işlev çağırmasından çıkarırsa `CallerLineNumberAttribute`, o konumun satır numarasını temsil eden sayısal bir değişmez değeri varsayılan parametre değeri yerine çağırma bağımsız değişkeni olarak kullanılır.

Çok satırlı çağırma yayılmış durumdaysa de seçilen satıra uygulamaya bağlıdır.

Satır numarası etkilenebilir Not `#line` yönergeleri ([satır yönergeleri](lexical-structure.md#line-directives)).

#### <a name="the-callerfilepath-attribute"></a>CallerFilePath özniteliği

`System.Runtime.CompilerServices.CallerFilePathAttribute` Standart bir örtük dönüştürme olduğunda isteğe bağlı parametrelere izin ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) öğesinden `string` parametrenin türü.

İsteğe bağlı bir parametre ile bir konumda bir kaynak kodu bir işlev çağırmasından çıkarırsa `CallerFilePathAttribute`, o konumun yolunu temsil eden bir dize sabit değeri varsayılan parametre değeri yerine çağırma bağımsız değişkeni olarak kullanılır.

Dosya yolunun biçimi uygulamaya bağlıdır.

Dosya yolu etkilenebilir Not `#line` yönergeleri ([satır yönergeleri](lexical-structure.md#line-directives)).

#### <a name="the-callermembername-attribute"></a>CallerMemberName özniteliği

`System.Runtime.CompilerServices.CallerMemberNameAttribute` Standart bir örtük dönüştürme olduğunda isteğe bağlı parametrelere izin ([standart örtük dönüştürmeler](conversions.md#standard-implicit-conversions)) öğesinden `string` parametrenin türü.

Kendisi veya kendi dönüş türü, parametre veya tür parametrelerinde bir konuma bir öznitelik veya işlevi üyesinin gövdesi içinde bir işlev çağırmasından işlevi üyeye uygulanan, kaynak kodu ile isteğe bağlı bir parametre atlar `CallerMemberNameAttribute`, bir Bu üyenin adını temsil eden bir dize sabit değeri varsayılan parametre değeri yerine çağırma bağımsız değişkeni olarak kullanılır.

Genel yöntemler içinde gerçekleşen çağrılarını yöntemi adın tamamı yalnızca, tür parametresi listesi kullanılır.

Açık arabirim üyesi uygulamalarını içinde gerçekleşen çağrılarını yöntemi adın tamamı yalnızca, önceki arabirimi niteleme olmadan kullanılır.

Özellik veya olay erişimcileri içinde gerçekleşen çağrıları için kullanılan üyesi, özelliğin veya olayın kendisinin addır.

Dizin Oluşturucu erişimcileri içinde gerçekleşen çağrıları için kullanılan üye tarafından sağlanan addır bir `IndexerNameAttribute` ([IndexerName özniteliği](attributes.md#the-indexername-attribute)) varsa, dizin oluşturucu üye veya varsayılan adı `Item` Aksi takdirde.

Örnek oluşturucuları, statik Oluşturucular, Yıkıcılar ve işleçler bildirimlerinde, üye ortaya çağrıları için kullanılan ad uygulamaya bağlıdır.

## <a name="attributes-for-interoperation"></a>Birlikte çalışma için öznitelikler

Not: Bu bölüm, yalnızca Microsoft .NET uygulaması için geçerlidir C#.

### <a name="interoperation-with-com-and-win32-components"></a>COM ve Win32 bileşenleri ile birlikte çalışma

.NET çalışma zamanı çok sayıda COM ve Win32 DLL'leri kullanılarak yazılmış bileşenler ile çalışmak C# programları etkinleştirme öznitelikleri sağlar. Örneğin, `DllImport` özniteliği kullanılabilir bir `static extern` bir Win32 DLL içinde bulunacak yöntemin uygulanmasını olduğunu belirtmek için yöntemi. Bu öznitelikler bulunan `System.Runtime.InteropServices` ad alanı ve bu öznitelikler için ayrıntılı belgelere .NET çalışma zamanı belgelerinde bulunabilir.

### <a name="interoperation-with-other-net-languages"></a>Diğer .NET dilleri ile birlikte çalışma

#### <a name="the-indexername-attribute"></a>IndexerName özniteliği

Dizin oluşturucular dizinli özellikler kullanılarak. NET'te uygulanır ve .NET meta verilerde bir ada sahip. Hayır ise `IndexerName` özniteliktir bir dizin oluşturucu ve ardından adı için mevcut `Item` varsayılan olarak kullanılır. `IndexerName` Özniteliği sağlayan bir geliştirici bu varsayılanı geçersiz kılmak ve farklı bir ad belirtin.

```csharp
namespace System.Runtime.CompilerServices.CSharp
{
    [AttributeUsage(AttributeTargets.Property)]
    public class IndexerNameAttribute: Attribute
    {
        public IndexerNameAttribute(string indexerName) {...}
        public string Value { get {...} } 
    }
}
```
