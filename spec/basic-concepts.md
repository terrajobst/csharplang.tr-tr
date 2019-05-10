---
ms.openlocfilehash: 1c3d05674f8f7b69e70e0d9e06021537fc45f7ed
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488483"
---
# <a name="basic-concepts"></a>Temel kavramlar

## <a name="application-startup"></a>Uygulama başlatma

Bir derlemeye bir ***giriş noktası*** çağrılır bir ***uygulama***. Bir uygulama olduğunda çalıştırın, yeni bir ***uygulama etki alanı*** oluşturulur. Bir uygulamayı birkaç farklı örneklemeleri aynı makinede aynı anda mevcut olabilir ve her biri kendi uygulama etki alanı vardır.

Uygulama etki alanı, uygulama durumu için kapsayıcı olarak çalışan tarafından uygulama yalıtımı sağlar. Uygulama etki alanı, bir kapsayıcı ve uygulama ve kullandığı sınıf kitaplıkları içinde tanımlanan türler için sınır olarak görev yapar. Aynı türden başka bir uygulama etki alanına yüklenen bir uygulama etki alanına yüklenen türleri farklıdır ve nesnelerin örneklerini uygulama etki alanları arasında doğrudan paylaşılmaz. Örneğin, her uygulama etki alanı statik değişkenler bu türleri için kendi kopyasına sahip ve bir tür için bir statik Oluşturucu uygulama etki alanı başına en fazla bir kez çalıştırılır. Uygulamaları oluşturma ve yok edilmesini uygulama etki alanları için uygulamaya özel ilke veya mekanizmaları sağlamak ücretsizdir.

***Uygulama başlatma*** yürütme ortamı uygulamanın giriş noktası olarak adlandırılan belirlenen bir yöntemi çağırdığında gerçekleşir. Bu giriş noktası yönteminin her zaman adlı `Main`ve aşağıdaki imzalara sahip olabilir:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Gösterildiği gibi giriş noktası isteğe bağlı olarak döndürebilir bir `int` değeri. Bu dönüş değeri, uygulama sonlandırılmasıyla kullanılır ([uygulama sonlandırma](basic-concepts.md#application-termination)).

Giriş noktası isteğe bağlı olarak bir biçimsel parametreye sahip olabilir. Parametresi, herhangi bir ad olabilir, ancak parametresinin türü olmalıdır `string[]`. Biçimsel parametre varsa, yürütme ortamı oluşturur ve başarılı bir `string[]` olan komut satırı bağımsız değişkenleri içeren değişken belirtilen uygulama başlatıldığı. `string[]` Bağımsız değişken NULL'dur hiçbir zaman, ancak hiçbir komut satırı bağımsız değişkenleri belirtilmişse sıfır uzunluğuna sahip olabilir.

C# yöntemi aşırı yüklemesi desteklediğinden, sağlanan her farklı bir imzaya sahip bir sınıfın veya yapının bazı yöntemi birden çok tanımları içerebilir. Ancak, tek bir program içindeki herhangi bir sınıf veya yapı adında birden fazla yöntem içerebilir `Main` tanımında bu uygulaması giriş noktası olarak kullanılacak niteliği taşır. Diğer aşırı yüklenmiş sürümleri `Main` , ancak birden fazla parametre sahip oldukları ya da yalnızca kendi parametre türüdür dışında sağlanan izin verilen `string[]`.

Bir uygulamanın birden fazla sınıflar veya yapılar oluşur. Adlı yöntemi içeren birden fazla bu sınıflar veya yapılar için mümkün olduğu `Main` tanımında bu uygulaması giriş noktası olarak kullanılacak niteliği taşır. Böyle durumlarda (örneğin, bir komut satırı derleyicisi seçeneği) veren dış mekanizmayı bunlardan birini seçmek için kullanılmalıdır `Main` giriş noktası olarak yöntemler.

C# ' ta her yöntemi bir sınıf veya yapı üyesi olarak tanımlanmalıdır. Normalde, bildirilen erişilebilirliği ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)) yöntemi tarafından erişim değiştiricileri belirlenir ([erişim değiştiricilerine](classes.md#access-modifiers)) bildiriminden ve benzer şekilde bildirilen belirtilen bir türün erişilebilirlik bildiriminde belirtilen erişim değiştiricileri tarafından belirlenir. Çağrılabilir olması için sırada belirli bir yöntemin belirli bir türün, tür ve üye hem erişilebilir olması gerekir. Ancak, uygulama giriş noktası bir özel durumdur. Özellikle, yürütme ortamı, uygulamanın giriş noktasını bakılmaksızın kendi bildirilen erişilebilirliği ve öğesinin bildirilen erişilebilirliği kapsayan kendi tür bildirimleri bağımsız olarak erişebilirsiniz.

Uygulama giriş noktası yöntemi, bir genel sınıf bildiriminde olmayabilir.

Diğer tüm yönden girdi noktası yöntemleri giriş noktası olmayan olanlar gibi davranır.

## <a name="application-termination"></a>Uygulamayı sonlandırma

***Uygulama sonlandırma*** yürütme ortamı denetimini döndürür.

Uygulamanın, dönüş türü ***giriş noktası*** yöntemi `int`, döndürülen değer uygulamanın hizmet veren ***sonlandırma durum kodu***. Bu kodun amacı, başarı veya başarısızlık yürütme ortamı için iletişime izin vermektir.

Giriş noktası yönteminin dönüş türü ise `void`, sağ ayraç ulaşmasını (`}`), sonlandıran yöntemi veya yürütülürken bir `return` olan herhangi bir ifade deyimi sonlandırma durum koduna sonuçları `0`.

Bu tür temizleme atlanmadıkları sürece uygulamanın sonlanması öncesinde yok ediciler için tüm henüz çöp olarak toplanacak olan nesneleri, çağrılır (kitaplık yöntemine bir çağrıyla `GC.SuppressFinalize`, örneğin).

## <a name="declarations"></a>Bildirimler

Bildirimleri bir C# programında, programın şekli oluşturan öğeler tanımlayın. C# programları, ad alanlarını kullanarak düzenlenir ([ad alanları](namespaces.md)), türü içerebilecek bildirimler ve iç içe geçmiş ad alanı bildirimi. Tür bildirimleri ([tür bildirimleri](namespaces.md#type-declarations)) sınıflarını tanımlamak için kullanılır ([sınıfları](classes.md)), yapılar ([yapılar](structs.md)), arabirimleri ([arabirimleri](interfaces.md) ), sabit listeleri ([numaralandırmalar](enums.md)) ve temsilciler ([Temsilciler](delegates.md)). Türde üye türü bildiriminde izin türü bildirimi formunda bağlıdır. Örneğin, sınıf bildirimleri sabitleri için bildirimi içerebilir ([sabitleri](classes.md#constants)), alanlar ([alanları](classes.md#fields)), yöntemleri ([yöntemleri](classes.md#methods)), özellikleri ([ Özellikleri](classes.md#properties)), olaylar ([olayları](classes.md#events)), dizin oluşturucular ([dizin oluşturucular](classes.md#indexers)), işleçler ([işleçleri](classes.md#operators)), örnek oluşturucuları ([ Örnek oluşturucuları](classes.md#instance-constructors)), statik oluşturucular ([statik oluşturucular](classes.md#static-constructors)), Yıkıcılar ([yok ediciler](classes.md#destructors)) ve iç içe geçmiş türler ([iç içe türler](classes.md#nested-types)).

Bir bildirimi bir ad tanımlar ***bildirim alanı*** bildirimi ait olduğu. Hariç üyeler aşırı ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir bildirim alanında aynı ada sahip üye tanıtan iki veya daha fazla bildirimleri sağlamak için bir derleme zamanı hata. Aynı ada sahip üyelerinin farklı türleri içeren bir bildirim alanı için hiçbir zaman mümkündür. Örneğin, bir bildirim alanı hiçbir zaman alan ve bir yöntem aynı adla içerebilir.

Aşağıda açıklandığı gibi birkaç farklı türde bildirimi alanları vardır.

*  Bir programın tüm kaynak dosyaları içinde *namespace_member_declaration*hiçbir kapsayan ile s *namespace_declaration* adlı tek bir birleştirilmiş bildirimi boşluk üyeleri ***genel Bildirim alanı***.
*  Bir programın tüm kaynak dosyaları içinde *namespace_member_declaration*ürününe *namespace_declaration*tam ad alanı ile aynı ada sahip s olan tek bir birleştirilmiş bildirimine üyeleri boşluk.
*  Yeni bir bildirim alanı her sınıf, yapı veya arabirim bildirimi oluşturur. Adları kullanıma sunulan bu bildirim alanına *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, veya *type_parameter*s. Aşırı yüklenmiş bir örnek oluşturucusu hariç, bildirimler ve statik Oluşturucu bildirimi, bir sınıfın veya yapının sınıfın veya yapının aynı ada sahip bir üye bildirimi içeremez. Bir sınıf, yapı veya arabirim aşırı yüklenmiş yöntemler ve dizin oluşturucular bildirimi izin verir. Ayrıca, bir sınıf veya yapı bildirimi aşırı yüklenmiş örnek oluşturucuları ve işleçleri izin verir. Bu yöntem bildirimleri kendi imzasında farklı sağlanan Örneğin, bir sınıf, yapı veya arabirim aynı ada sahip birden çok yöntem bildirimleri içerebilir ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)). Temel sınıflar bir sınıfın bildiriminin alanına katkıda bulunan değil ve temel arabirimleri bildirim alanına bir arabirimin katkıda bulunmuyor unutmayın. Bu nedenle, türetilen bir sınıfta veya arabirimde aynı ada sahip bir üyesi devralınmış bir üyeyi olarak bildirin izin verilmez. Böyle bir üyesi edilir ***Gizle*** devralınan üye.
*  Yeni bir bildirim alanı her temsilci bildirimi oluşturur. Adları bu bildirim alanına biçimsel parametreler aracılığıyla kullanıma sunulmuştur (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s.
*  Yeni bir bildirim alanı her numaralandırma bildirimi oluşturur. Adları kullanıma sunulan bu bildirim alanına *enum_member_declarations*.
*  Her yöntem bildiriminde, dizin oluşturucu bildirimi, işleç bildirimi, kopya Oluşturucusu bildirimi ve anonim işlev adı verilen yeni bir bildirim alanı oluşturur bir ***yerel değişken bildiriminde yer***. Adları bu bildirim alanına biçimsel parametreler aracılığıyla kullanıma sunulmuştur (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s. İşlev üyesi veya anonim işlev gövdesinin varsa, yerel değişken bildiriminde alanı içinde iç içe geçmiş kabul edilir. Bir yerel değişken bildiriminde alan ve aynı ada sahip öğeleri içeren bir iç içe yerel değişken bildiriminde alan için bir hatadır. Bu nedenle, bir iç içe geçmiş bir bildirim alanı içinde yerel bir değişken veya yerel bir değişken olarak aynı ada sahip sabit veya sabit kapsayan bir bildirim alanı bildirmek mümkün değildir. Diğer bildirim boşluk içerdiği sürece aynı ada sahip öğeleri içeren iki bildirimi boşluk mümkündür.
*  Her *blok* veya *switch_block* , hem de bir *için*, *foreach* ve *kullanarak* deyimi, oluşturur bir yerel değişken bildiriminde yer yerel değişkenleri ve yerel sabitleri için. Adları kullanıma sunulan bu bildirim alanına *local_variable_declaration*s ve *local_constant_declaration*s. İçinde kendi parametrelerini için bu işlevleri tarafından bildirilen yerel değişken bildiriminde yer olarak veya bir üye işlev veya anonim işlev gövdesi içinde oluşan bloklar yerleştirilir unutmayın. Bu nedenle, örneğin bir yerel değişken ve aynı ada sahip bir parametre ile bir yöntem sağlamak için bir hatadır.
*  Her *blok* veya *switch_block* etiketleri için ayrı bildirim boşluk oluşturur. Adları kullanıma sunulan bu bildirim alanına *labeled_statement*s yanı sıra, adları başvuru aracılığıyla *goto_statement*s. ***Etiket bildirim alanı*** bloğu tüm iç içe geçmiş blokları içerir. Bu nedenle, iç içe geçmiş bir bloğu içinde kapsayan bir blok içinde bir etiket olarak aynı ada sahip bir etiket bildirmek mümkün değildir.

Adları bildirilen metinsel genellikle hiçbir önemini sırasıdır. Özellikle, metinsel sırası önemli bildirimler ve ad alanları, sabitleri, yöntemleri, özellikleri, olayları, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler, statik oluşturucular ve türleri kullanımı için değildir. Aşağıdaki yollarla bildirim sırası önemlidir:

*  Alan bildirimleri ve yerel değişken bildirimleri için bildirim sırasında kendi başlatıcıları (varsa) yürütüldüğü sırasını belirler.
*  Yerel değişkenler, kullanılmadan önce tanımlanmalıdır ([kapsamları](basic-concepts.md#scopes)).
*  Sabit listesi üye bildirimleri için bildirim sırasında ([numaralandırma üyeleri](enums.md#enum-members)) önemli olduğunda *constant_expression* değerleri atlanmış.

Bildirimi bir ad alanının "Bitti" açık"ve iki ad alanı bildirimleri aynı tam ada sahip aynı bildirim alanına katkıda bulunan bir alandır. Örneğin:
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

Bu durumda tam olarak nitelenmiş ada sahip iki sınıfları bildirme aynı bildirim alanına, yukarıdaki iki ad alanı bildirimi katkıda `Megacorp.Data.Customer` ve `Megacorp.Data.Order`. İki bildirimi aynı bildirim alanına katkıda bulunan, her aynı ada sahip bir sınıf bildirimi içeriyorsa, bir derleme zamanı hatası nedeni.

Bildirim alanı bir blok olarak yukarıda belirtilen, herhangi bir iç içe geçmiş bloğu içerir. Bu nedenle, aşağıdaki örnekte, `F` ve `G` yöntemleri, çünkü bir derleme zamanı hatası neden adı `i` dış blokta bildirilen ve iç bloğu içinde bildirilemez. Ancak, `H` ve `I` yöntemleri iki itibaren geçerlidir `i`ait ayrı iç içe olmayan bloklar olarak bildirilir.

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a>Üyeler

Ad alanları ve türler sahip ***üyeleri***. Bir varlık üyeleri ve ardından varlığa bir başvuru ile başlayan bir tam adı kullanılarak genel kullanıma sunulmuştur bir "`.`" belirteci, takip eden üyenin adı.

Bir türün üyeleri ya da tür bildiriminde bildirilen veya ***devralınan*** türü temel sınıftan. Bir tür bir temel sınıftan örnek Oluşturucular, Yıkıcılar ve statik oluşturucular dışında temel sınıfın tüm üyeleri devralan üyeler türetilmiş bir türde olur. Bir temel sınıf üyenin bildirilen erişilebilirliği, üye devralınan olup olmadığını denetlemez; devralma bir örnek oluşturucusu, statik oluşturucu veya yıkıcı olmayan herhangi bir üyeye genişletir. Ancak, devralınan bir üyeyi bir türetilmiş türde, ya da bildirilen erişilebilirliğini nedeniyle erişilemiyor olabilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)) veya bir tür bildiriminde tarafından gizlendiği ([aracılığıyla gizleme Devralma](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Namespace üyelerini

Ad alanları ve kapsayan ad uzayı olmayan türleri olan üyeleri ***genel ad alanı***. Bu doğrudan genel bildirim alanında bildirilen adların karşılık gelir.

Bu ad, ad alanları ve ad alanı içinde bildirilen türler üyeleridir. Bu doğrudan ad alanı bildirimi alana bildirilen adların karşılık gelir.

Ad alanları, hiçbir erişim kısıtlamasına sahip. Özel, korumalı veya iç ad alanı bildirmek mümkün değildir ve ad alanı adları her zaman genel olarak erişilebilir.

### <a name="struct-members"></a>Yapı üyeleri

Yapı üyeleri struct bildirilen üyeler ve yapının doğrudan temel sınıftan devralınan üyeleri olan `System.ValueType` ve dolaylı temel sınıfı `object`.

Basit bir tür üyelerini doğrudan yapı türü diğer adlı basit bir tür tarafından üyelerinin karşılık gelir:

*  Üyeleri `sbyte` üyeleri `System.SByte` yapısı.
*  Üyeleri `byte` üyeleri `System.Byte` yapısı.
*  Üyeleri `short` üyeleri `System.Int16` yapısı.
*  Üyeleri `ushort` üyeleri `System.UInt16` yapısı.
*  Üyeleri `int` üyeleri `System.Int32` yapısı.
*  Üyeleri `uint` üyeleri `System.UInt32` yapısı.
*  Üyeleri `long` üyeleri `System.Int64` yapısı.
*  Üyeleri `ulong` üyeleri `System.UInt64` yapısı.
*  Üyeleri `char` üyeleri `System.Char` yapısı.
*  Üyeleri `float` üyeleri `System.Single` yapısı.
*  Üyeleri `double` üyeleri `System.Double` yapısı.
*  Üyeleri `decimal` üyeleri `System.Decimal` yapısı.
*  Üyeleri `bool` üyeleri `System.Boolean` yapısı.

### <a name="enumeration-members"></a>Numaralandırma üyeleri

Bir numaralandırma sabit listesinde bildirilen sabitler ve sabit listesinin doğrudan temel sınıftan devralınan üyeleri üyeleri `System.Enum` ve dolaylı temel sınıfların `System.ValueType` ve `object`.

### <a name="class-members"></a>Sınıf üyeleri

Bir sınıfın üyeleri sınıfta bildirilen üyeleri ve temel sınıftan devralınan üyeleri olan (sınıf dışında `object` temel sınıfa sahip). Temel sınıftan devralınan üyeleri, sabitleri, alanları, yöntemleri, özellikleri, olayları, dizin oluşturucular, işleçler ve tür temel sınıfı, ancak örnek Oluşturucular, Yıkıcılar ve temel sınıfın statik oluşturucular içerir. Temel sınıf üyelerinin bakılmaksızın kendi erişilebilirlik devralınır.

Bir sınıf bildirimi bildirimlerini sabitleri, alanları, yöntemler, özellikler, olaylar, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler, statik oluşturucular ve türleri içerebilir.

Üyeleri `object` ve `string` karşılık gelen sınıf türü üyeleri doğrudan bunlar diğer adı:

*  Üyeleri `object` üyeleri `System.Object` sınıfı.
*  Üyeleri `string` üyeleri `System.String` sınıfı.

### <a name="interface-members"></a>Arabirim üyeleri

Bir arabirim üyelerinin arabirimi ve tüm temel arabirimleri arabiriminin bildirilen üyeleridir. Sınıf üyeleri `object` kesinlikle anlamda, herhangi bir arabirim üyesi değilseniz, ([arabirim üyeleri](interfaces.md#interface-members)). Ancak, sınıf üyeleri `object` herhangi bir arabirim türüne içinde'üye araması aracılığıyla kullanılabilir ([üye araması](expressions.md#member-lookup)).

### <a name="array-members"></a>Dizi üyeleri

Dizi üyelerinin sınıftan devralınan üyeleri `System.Array`.

### <a name="delegate-members"></a>Temsilci üye

Bir temsilci sınıftan devralınan üyeleri üyeleri `System.Delegate`.

## <a name="member-access"></a>Üye erişimi

Üye bildirimlerini üye erişimi denetlemenizi sağlar. Bir üyenin erişilebilirliğini tarafından bildirilen erişilebilirliği kurulur ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)) üyesi varsa hemen içeren türün erişilebilirlik ile birleştirilmiş.

Belirli bir üye erişimine izin verileceğini, üyesi olduğu söylenir ***erişilebilir***. Belirli bir üyeye erişim izni, buna karşılık, üyesi olduğu söylenir ***erişilemez***. Metinsel konumun erişim yer alan erişilebilirlik etki alanında eklendiğinde bir üye erişim izin verilir ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) üyesi.

### <a name="declared-accessibility"></a>Bildirilen erişilebilirliği

***Erişilebilirlik bildirilen*** üyesi aşağıdakilerden biri olabilir:

*  Dahil ederek seçtiğiniz ortak bir `public` değiştiricisi üye bildirimi. Sezgisel anlamını `public` "olmayan sınırlı erişim".
*  Korumalı, seçili dahil ederek bir `protected` değiştiricisi üye bildirimi. Sezgisel anlamını `protected` "içeren sınıfı veya türleri sınırlı erişim içeren sınıfından türetilir".
*  Dahil ederek seçilen dahili bir `internal` değiştiricisi üye bildirimi. Sezgisel anlamını `internal` "Bu program için sınırlı erişim".
*  İç (korumalı veya iç anlamına gelir), korumalı, seçili her ikisi de dahil ederek bir `protected` ve `internal` değiştiricisi üye bildirimi. Sezgisel anlamını `protected internal` "Bu program için sınırlı erişim veya içeren sınıfından türetilen türler".
*  Dahil ederek seçildiğinden özel, bir `private` değiştiricisi üye bildirimi. Sezgisel anlamını `private` "kapsadığı tür için sınırlı erişim".

Bağlama bağlı olarak bir üye bildirim aldığı yerleştirin, yalnızca belirli türlerdeki bildirilen erişilebilirliği izin verilir. Ayrıca, üye bildirimi herhangi bir erişim değiştiricileri içermez, bildirimi yer aldığı içerik erişilebilirlik bildirilen varsayılan belirler.

*  Örtük olarak ad sahip `public` erişilebilirlik bildirilir. Ad alanı bildirimi üzerinde hiçbir erişim değiştiricisine izin verilir.
*  Türleri derleme biriminden veya ad alanı bildirimi olabilir `public` veya `internal` erişilebilirlik ve varsayılan olarak bildirilen `internal` erişilebilirlik bildirilir.
*  Sınıf üyeleri bildirilen erişilebilirliği beş tür hiçbirini ve varsayılan `private` erişilebilirlik bildirilir. (Bir türü bir ad alanının bir üyesi yalnızca olabilir olarak bildirilmiş ancak bildirilen erişilebilirliği beş türde herhangi bir sınıfın üyesi olabildiği gibi bir türü bildirilen Not `public` veya `internal` erişilebilirlik bildirildi.)
*  Yapı üyeleri olabilir `public`, `internal`, veya `private` erişilebilirlik ve varsayılan olarak bildirilen `private` yapılardan türetme çünkü erişilebilirlik bildirilir. (Bu, bu yapı tarafından devralınan değil) yapı sürümünde Struct üyelerinin olamaz `protected` veya `protected internal` erişilebilirlik bildirilir. (Yapı üyesi olabildiği gibi bir türü bildirilen unutmayın `public`, `internal`, veya `private` erişilebilirlik, türü bir ad alanının bir üyesi yalnızca olabilir olarak bildirilmiş ancak bildirilen `public` veya `internal` erişilebilirlik bildirildi.)
*  Arabirim üyeleri örtük olarak sahip `public` erişilebilirlik bildirilir. Arabirim üye bildirimlerinde hiçbir erişim değiştiricisine izin verilir.
*  Numaralandırma üyelerini örtük olarak sahip `public` erişilebilirlik bildirilir. Hiçbir erişim değiştiricisine sabit listesi üye bildirimlerinde izin verilir.

### <a name="accessibility-domains"></a>Erişilebilirlik etki alanları

***Erişilebilirlik etki alanı*** program metni üyesine erişim verilir (büyük olasılıkla ayrık) bölümlerini üyesi oluşur. Erişilebilirlik etki alanı üyesi tanımlama amacıyla, üyesi olduğu söylenir ***en üst düzey*** bir tür içinde bildirilmedi ve üyesi olduğu söylenir ***iç içe geçmiş*** başka bir tür içinde bildirilirse. Ayrıca, ***program metni*** bir programı tüm program tüm kaynak dosyalarında bulunan metin program ve tüm bulunan metin programı gibi bir tür öğesinin program metni tanımlanır tanımlanan *type_declaration*s (büyük olasılıkla, tür içinde iç içe geçmiş türler dahil) türü.

Önceden tanımlanmış bir tür erişilebilirlik etki alanı (gibi `object`, `int`, veya `double`) sınırsızdır.

Erişilebilirlik etki alanı bir üst düzey ilişkisiz türü `T` ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)) bildirilen bir programda `P` şu şekilde tanımlanır:

*  Varsa öğesinin bildirilen erişilebilirliği `T` olduğu `public`, Erişilebilirlik etki alanı `T` öğesinin program metnidir `P` ve başvuran herhangi bir programı `P`.
*  Varsa öğesinin bildirilen erişilebilirliği `T` olduğu `internal`, Erişilebilirlik etki alanı `T` öğesinin program metnidir `P`.

Üst düzey bir bağlanmamış tür erişilebilirlik etki alanı her zaman en az olduğunu izleyen bu tanımları yazın, program öğesinin program metni bildirilir.

Erişilebilirlik etki alanı için oluşturulan tür `T<A1, ..., An>` ilişkisiz genel bir türün erişilebilirlik etki alanının kesişimidir `T` ve tür bağımsız değişkenlerini erişilebilirlik etki alanları `A1, ..., An`.

Erişilebilirlik etki alanı iç içe üyenin `M` bir türde bildirilen `T` bir programın `P` şu şekilde tanımlanır (dikkate alınarak `M` kendisini büyük olasılıkla bir türü olabilir):

*  Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `public`, Erişilebilirlik etki alanı `M` erişilebilirlik etki alanıdır `T`.
*  Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `protected internal`, let `D` öğesinin program metni birleşimi olabilir `P` ve türetilen her türlü program metni `T`, dışında bildirilen `P`. Erişilebilirlik etki alanı `M` erişilebilirlik etki alanının kesişimidir `T` ile `D`.
*  Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `protected`, let `D` öğesinin program metni birleşimi olabilir `T` ve türetilen her türlü program metni `T`. Erişilebilirlik etki alanı `M` erişilebilirlik etki alanının kesişimidir `T` ile `D`.
*  Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `internal`, Erişilebilirlik etki alanı `M` erişilebilirlik etki alanının kesişimidir `T` öğesinin program metni ile `P`.
*  Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `private`, Erişilebilirlik etki alanı `M` öğesinin program metnidir `T`.

Bu tanımlarından iç içe üyenin erişilebilirlik etki alanı her zaman en az olduğunu izleyen program metni türündeki üye bildirilir. Ayrıca, Erişilebilirlik etki alanı üyesi hiç üyesi bildirildiği türün erişilebilirlik etki daha kapsamlı olduğunu izler.

Sezgisel koşullarında bir tür veya üye `M` olan erişim, aşağıdaki adımları erişim izin verildiğinden emin olmak için değerlendirilir:

*  İlk olarak, eğer `M` (olarak karşılıklı derleme biriminde veya bir ad alanı), bir tür içindeki bir derleme zamanı hatası oluşur türü erişilebilir durumda değilse bildirilir.
*  Ardından, eğer `M` olduğu `public`, erişime izin verilir.
*  Aksi takdirde `M` olduğu `protected internal`, programda içinde ortaya çıkarsa erişimine izin verilen `M` bildirilen veya bir sınıf içinde oluşursa türetilen sınıfta `M` bildirilir ve türetilmiş üzerinden gerçekleşir sınıf türü ([korumalı üyeleri örneği için erişim](basic-concepts.md#protected-access-for-instance-members)).
*  Aksi takdirde `M` olduğu `protected`, sınıfta içinde ortaya çıkarsa erişimine izin verilen `M` bildirilen veya bir sınıf içinde oluşursa türetilen sınıfta `M` bildirilir ve türetilmiş üzerinden gerçekleşir sınıf türü ([korumalı üyeleri örneği için erişim](basic-concepts.md#protected-access-for-instance-members)).
*  Aksi takdirde `M` olduğu `internal`, programda içinde ortaya çıkarsa erişimine izin verilen `M` bildirilir.
*  Aksi takdirde `M` olduğu `private`, hangi tür içinde ortaya çıkarsa erişimine izin verilen `M` bildirilir.
*  Aksi takdirde, türe veya üyeye erişilemiyor ve bir derleme zamanı hatası oluşur.

Örnekte
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
Aşağıdaki erişilebilirlik etki alanları, sınıflar ve üyeler vardır:

*  Erişilebilirlik etki alanı `A` ve `A.X` sınırsızdır.
*  Erişilebilirlik etki alanı `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, ve `B.C.Y` içeren program öğesinin program metnidir.
*  Erişilebilirlik etki alanı `A.Z` öğesinin program metnidir `A`.
*  Erişilebilirlik etki alanı `B.Z` ve `B.D` öğesinin program metnidir `B`, öğesinin program metni dahil olmak üzere `B.C` ve `B.D`.
*  Erişilebilirlik etki alanı `B.C.Z` öğesinin program metnidir `B.C`.
*  Erişilebilirlik etki alanı `B.D.X` ve `B.D.Y` öğesinin program metnidir `B`, öğesinin program metni dahil olmak üzere `B.C` ve `B.D`.
*  Erişilebilirlik etki alanı `B.D.Z` öğesinin program metnidir `B.D`.

Erişilebilirlik etki alanı üyesi hiçbir zaman örnekte gösterildiği gibi bir kapsayan tür daha büyük. Örneğin, olsa bile tüm `X` üyelere sahip genel bildirilen erişilebilirliği dışındaki tüm `A.X` içeren bir tür tarafından kısıtlanmış erişilebilirlik etki alanınız.

Bölümünde anlatıldığı gibi [üyeleri](basic-concepts.md#members), örneğin Oluşturucular, Yıkıcılar ve statik oluşturucular dışında bir taban sınıfın tüm üyeleri türetilen türler tarafından devralınır. Bu, bir taban sınıfın bile özel üyeler içerir. Ancak, özel üye erişilebilirlik etki alanı üyesi bildirildiği türü yalnızca program metni içerir. Örnekte
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
`B` sınıfından devralan bir özel üye `x` gelen `A` sınıfı. Özel üye olduğundan, yalnızca içinde erişilebilir *class_body* , `A`. Bu nedenle, erişim izni `b.x` içinde başarılı `A.F` yöntemi, ancak başarısız olursa `B.F` yöntemi.

### <a name="protected-access-for-instance-members"></a>Örnek üyeleri için Korumalı Erişim

Olduğunda bir `protected` örnek üyesi içinde bildirildiğinde, sınıf öğesinin program metni dışında erişilebilir ve ne zaman bir `protected internal` erişim içinde gerçekleşmesi gerekir, örnek üyesi, bildirilen program öğesinin program metni dışında erişilebilir bir sınıf bildirimi içinde bildirildiği sınıfından türetilir. Ayrıca, erişim, türetilmiş sınıf türü ya da ondan oluşturulan sınıf türünde bir örnek üzerinden gerçekleşmesi için gerekli değildir. Bu kısıtlama, bir türetilmiş sınıf bile aynı temel sınıftan devralınan üyeleri, türetilen diğer sınıflar korumalı üyelerine erişmesini engeller.

İzin `B` bir korumalı örnek üye bildirir bir taban sınıfı olarak `M`ve `D` türetildiği bir sınıf olması `B`. İçinde *class_body* , `D`, erişim `M` aşağıdaki biçimlerden birini alabilir:

*  Nitelenmemiş bir *type_name* veya *primary_expression* formun `M`.
*  A *primary_expression* formun `E.M`, türü sağlanan `E` olduğu `T` veya türetilmiş bir sınıf `T`burada `T` sınıf türüdür `D`, veya bir sınıf türü oluşturulan `D`
*  A *primary_expression* formun `base.M`.

Bu erişim forms ek olarak, bir korumalı örnek oluşturucusu içinde temel sınıfın türetilmiş bir sınıf erişebilirsiniz bir *constructor_initializer* ([Oluşturucu başlatıcıları](classes.md#constructor-initializers)).

Örnekte
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
içinde `A`, erişim mümkündür `x` örnekleri her ikisi de aracılığıyla `A` ve `B`, her iki durumda da yer örneği üzerinden erişim gerektirdiğinden `A` veya türetilmiş bir sınıf `A`. Bununla birlikte, içinde `B`, erişmek mümkün değildir `x` örneği üzerinden `A`, bu yana `A` türünden türemez `B`.

Örnekte
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
üç atamaları `x` genel türünden oluşturulduğu sınıf türleri örnekleri aracılığıyla tüm gerçekleşmesi için izin verilir.

### <a name="accessibility-constraints"></a>Erişilebilirlik kısıtlamaları

C# dilinde çeşitli yapıları türünün olmasını gerektiren ***en az olarak erişilebilir olarak*** üyesi veya başka bir tür. Bir tür `T` üyenin veya türün en az olarak erişilebilir olduğu söylenir `M` , Erişilebilirlik etki alanı `T` erişilebilirlik etki alanının bir üst kümesidir `M`. Diğer bir deyişle, `T` en az olarak erişilebilir olarak `M` varsa `T` tüm bağlamlarda erişilebilir `M` erişilebilir.

Aşağıdaki erişilebilirlik kısıtlamaları mevcuttur:

*  Bir sınıf türünün doğrudan temel sınıf en az sınıf türü kendisini olarak erişilebilir olması gerekir.
*  Açık bir arabirim türü temel arabirimleri en az arabirim türü kendisini olarak erişilebilir olması gerekir.
*  Bir temsilci türü parametre türleri ve dönüş türü, en az temsilci türü kendisini olarak erişilebilir olması gerekir.
*  Bir sabit değer türü en az sabit olarak olarak erişilebilir olması gerekir.
*  Bir alan türü en az alan olarak olarak erişilebilir olması gerekir.
*  Bir yöntemin parametre türleri ve dönüş türü, en az yöntemi olarak olarak erişilebilir olması gerekir.
*  Bir özelliğin türünü en az özelliği olarak olarak erişilebilir olması gerekir.
*  Bir olay türü en az olay olarak olarak erişilebilir olması gerekir.
*  Bir dizin oluşturucu türü ve parametre türleri, en az Indexer olarak erişilebilir olması gerekir.
*  Operatör için parametre türleri ve dönüş türü, en azından operatör olarak olarak erişilebilir olması gerekir.
*  Bir örnek oluşturucusunda parametre türleri, en az örnek oluşturucusu kendisini olarak erişilebilir olması gerekir.

Örnekte
```csharp
class A {...}

public class B: A {...}
```
`B` sınıfı sonuçları bir derleme zamanı hatası nedeniyle `A` en az olarak erişilebilir olarak değil `B`.

Benzer şekilde, örnekte
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
`H` yönteminde `B` çünkü dönüş türü bir derleme zamanı hatası oluşur `A` en az yöntemi olarak olarak erişilebilir değil.

## <a name="signatures-and-overloading"></a>İmzalar ve aşırı yükleme

Yöntemleri, örnek oluşturucuları, dizin oluşturucular ve işleçleri göre nitelenen kendi ***imzaları***:

*  Yöntemi, tür parametreleri sayısı ve türü ve tür (değer, başvuru veya çıkış) her birini sırayla soldan sağa kabul biçimsel parametrelerinin adı, yöntemin imzası oluşur. Bu amaçlar için oluşan bir biçimsel parametre türü yöntemin herhangi bir tür parametre adına göre değil, ancak türü bağımsız değişken listesindeki sıralı konumuna yöntem tarafından tanımlanır. Yöntemin imzası dönüş türü, özellikle içermez `params` en sağdaki parametresi ya da isteğe bağlı tür parametresi kısıtlamaları için belirtilen değiştiricisi.
*  Tür ve tür (değer, başvuru veya çıkış) sırayla soldan sağa kabul kendi biçimsel parametrelerinin her biri bir örnek oluşturucusu imzası oluşur. Bir örnek oluşturucusunda imzası özellikle içermemesi `params` en sağdaki parametresi için belirtilen değiştiricisi.
*  Bir dizin oluşturucu imzasının her birini sırayla soldan sağa kabul biçimsel parametrelerinin türünü oluşur. Bir dizin oluşturucu imzası özel öğe türü içermiyor ya da içeriyor mu `params` en sağdaki parametresi için belirtilen değiştiricisi.
*  Bir işleç imzası adını işleci ve her birini sırayla soldan sağa kabul biçimsel parametrelerinin türünü oluşur. Bir işleç imzası, özellikle sonuç türü içermez.

İmzalar, etkinleştirme mekanizması ***aşırı yükleme*** sınıflar, yapılar ve arabirimler üyelerin:

*  Yöntemlerinin aşırı yükleme, bir sınıf, yapı veya arabirim bunların imzalarını sağlanan aynı ada sahip birden çok yöntem, sınıf, yapı veya arabirim içinde benzersiz bildirmek için izin verir.
*  İmzaları, sınıf veya yapı içinde benzersiz olması koşuluyla örnek oluşturucuları aşırı yüklemesi bir sınıfın veya yapının birden çok örnek oluşturucuları bildirmek için verir.
*  Bunların imzalarını Bu sınıf, yapı veya arabirim içinde benzersiz olması koşuluyla dizin oluşturucu aşırı yüklemesi bir sınıf, yapı veya arabirim birden çok dizin oluşturucu bildirmek için verir.
*  Operatörleri aşırı yükleme, bir sınıf veya yapı bunların imzalarını sağlanan aynı ada sahip birden çok işleç, sınıf veya yapı içinde benzersiz bildirmek için izin verir.

Ancak `out` ve `ref` parametre değiştiriciler imzasının bir parçası olarak kabul edilir, tek bir türde bildirilen üyeler olamaz farklı imzasında sadece onun tarafından `ref` ve `out`. İki üye aynı ise olacaktır imzalarla aynı türdeki her iki yöntemde de tüm parametrelerinde bildirilir, bir derleme zamanı hatası meydana gelir `out` değiştiriciler değiştirildi `ref` değiştiriciler. İmza eşleşen diğer amaçlar için (örneğin, gizleme veya geçersiz kılma), `ref` ve `out` imzasının bir parçası olarak kabul edilir ve birbirlerine eşleşmiyor. (Bu kısıtlama izin vermektir C# programları, ortak dil altyapısı (yalnızca içinde farklı yöntemleri tanımlamak için bir yol sağlamaz, CLI üzerinde), çalıştırılacak kolayca çevrilemeyen `ref` ve `out`.)

İmzaların türleri amacıyla `object` ve `dynamic` aynı olarak kabul edilir. Tek bir türde bildirilen üyeler bu nedenle farklı imzasında sadece onun tarafından `object` ve `dynamic`.

Aşağıdaki örnek, aşırı yüklenmiş yöntem bildirimleri bunların imzalarını birlikte bir dizi gösterir.
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

Herhangi bir Not `ref` ve `out` parametre değiştiriciler ([yöntem parametreleri](classes.md#method-parameters)) bir imza bir parçasıdır. Bu nedenle, `F(int)` ve `F(ref int)` benzersiz imzalar. Ancak, `F(ref int)` ve `F(out int)` sadece onun tarafından imzalarının farklı olduğundan, aynı arabirimi içinde bildirilemez `ref` ve `out`. Ayrıca, dönüş türü Not ve `params` değiştiricisi parçası değildir, imza dönüş türü veya ekleme veya çıkarma, yalnızca temel aşırı mümkün olmadığından `params` değiştiricisi. Şekilde yöntemleri bildirimlerini `F(int)` ve `F(params string[])` bir derleme zamanı hatası sonucunda yukarıda tanımlanan.

## <a name="scopes"></a>Kapsamları

***Kapsam*** bir bölge içinde mümkündür nitelik adı olmadan yalnızca adla bildirilen bir varlığa başvurmak program metni adıdır. Kapsamları olabilir ***iç içe geçmiş***, ve bir iç kapsamı anlamı, bir dış kapsam adı yeniden bildirmek (Bu ancak tarafından uygulanan kısıtlama kaldırmaz, [bildirimleri](basic-concepts.md#declarations) iç içe geçmiş bir bloğu içinde olmadığını olası) kapsayan bir blok içinde yerel bir değişken olarak aynı ada sahip yerel bir değişken bildirmek için kullanılır. Dış kapsamdaki adından sonra denen moddadır ***gizli*** programının bölgede metin iç kapsamı tarafından ele ve dış adına erişim, yalnızca olası Adın niteleme tarafından.

*  Ad alanı üyesi kapsamı tarafından bildirilen bir *namespace_member_declaration* ([Namespace üyelerini](namespaces.md#namespace-members)) ile hiçbir kapsayan *namespace_declaration* tüm program metin.
*  Ad alanı üyesi kapsamı tarafından bildirilen bir *namespace_member_declaration* içinde bir *namespace_declaration* tam adı olan `N` olduğu *namespace_body*  , her *namespace_declaration* tam adı olan `N` veya ile başlayan `N`ve ardından bir nokta.
*  Tarafından tanımlanan ad kapsamı bir *extern_alias_directive* üzerinden genişletir *using_directive*s, *global_attributes* ve *namespace_member_ bildirimi*hemen içeren derleme birimi veya ad alanı gövdesinde, s. Bir *extern_alias_directive* tüm yeni üyeleri temel alınan bildirim alanına gereksinimdir. Diğer bir deyişle, bir *extern_alias_directive* geçişli değildir ancak bunun yerine oluştuğu yalnızca derleme birimi veya ad alanı gövdesi etkiler.
*  Kapsam adı tanımlı ya da içeri aktaran bir *using_directive* ([yönergeleri kullanarak](namespaces.md#using-directives)) üzerinden genişletir *namespace_member_declaration*sn  *compilation_unit* veya *namespace_body* hangi *using_directive* gerçekleşir. A *using_directive* sıfır veya daha fazla ad alanı, tür veya üye adları belirli bir içinde kullanılabilir hale *compilation_unit* veya *namespace_body*, ancak yok tüm yeni üyeleri temel alınan bildirim alanına katkıda bulunur. Diğer bir deyişle, bir *using_directive* geçişli değildir ancak bunun yerine yalnızca etkiler *compilation_unit* veya *namespace_body* içinde hangi gerçekleşir.
*  Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *class_declaration* ([sınıf bildirimleri](classes.md#class-declarations)) olan *class_base*, *type_parameter_constraints_clause*s ve *class_body* , söz konusu *class_declaration*.
*  Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *struct_declaration* ([yapı bildirimleri](structs.md#struct-declarations)) olan *struct_interfaces* , *type_parameter_constraints_clause*s ve *struct_body* , söz konusu *struct_declaration*.
*  Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *interface_declaration* ([arabirim bildirimleri](interfaces.md#interface-declarations)) olan *interface_base* , *type_parameter_constraints_clause*s ve *interface_body* , söz konusu *interface_declaration*.
*  Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *delegate_declaration* ([temsilci bildirimi](delegates.md#delegate-declarations)) olan *Döndür_tür*, *formal_parameter_list*, ve *type_parameter_constraints_clause*s, söz konusu *delegate_declaration*.
*  Tarafından bildirilen bir üye kapsamını bir *class_member_declaration* ([sınıfı gövdesi](classes.md#class-body)) olan *class_body* bildirimi oluştuğu içinde. Ayrıca, için bir sınıf üyesinin kapsamını genişletir *class_body* içeriğiyle erişilebilirlik etki alanında bulunan sınıflar türetilmiş ([erişilebilirlik etki alanı](basic-concepts.md#accessibility-domains)) üyesinin.
*  Tarafından bildirilen bir üye kapsamını bir *struct_member_declaration* ([Yapı üyeleri](structs.md#struct-members)) olan *struct_body* bildirimi oluştuğu içinde.
*  Tarafından bildirilen bir üye kapsamını bir *enum_member_declaration* ([numaralandırma üyeleri](enums.md#enum-members)) olan *enum_body* bildirimi oluştuğu içinde.
*  Bir parametre kapsamı içinde bildirilen bir *method_declaration* ([yöntemleri](classes.md#methods)) olan *method_body* , söz konusu *method_declaration*.
*  Bir parametre kapsamı içinde bildirilen bir *indexer_declaration* ([dizin oluşturucular](classes.md#indexers)) olan *accessor_declarations* , söz konusu *indexer_declaration*.
*  Bir parametre kapsamı içinde bildirilen bir *operator_declaration* ([işleçleri](classes.md#operators)) olan *blok* , söz konusu *operator_declaration*.
*  Bir parametre kapsamı içinde bildirilen bir *constructor_declaration* ([örnek oluşturucular](classes.md#instance-constructors)) olan *constructor_initializer* ve *blok* , söz konusu *constructor_declaration*.
*  Bir parametre kapsamı içinde bildirilen bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) olan *anonymous_function_body* , söz konusu *lambda_ ifade*
*  Bir parametre kapsamı içinde bildirilen bir *anonymous_method_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) olan *blok* , söz konusu *anonymous_method _expression*.
*  Etiket kapsamı içinde bildirilen bir *labeled_statement* ([etiketli deyimler](statements.md#labeled-statements)) olan *blok* bildirimi oluştuğu içinde.
*  Bildirilen yerel değişken kapsamını bir *local_variable_declaration* ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) blok bildirimi oluştuğu içinde.
*  Bildirilen yerel değişken kapsamını bir *switch_block* , bir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)) olan *switch_block*.
*  Bildirilen yerel değişken kapsamını bir *for_initializer* , bir `for` deyimi ([deyimi için](statements.md#the-for-statement)) olan *for_initializer*,  *for_condition*, *for_iterator*ve kapsanan *deyimi* , `for` deyimi.
*  Bir yerel sabit kapsamı içinde bildirilen bir *local_constant_declaration* ([yerel sabit bildirimleri](statements.md#local-constant-declarations)) blok bildirimi oluştuğu içinde. Önündeki değerinin metinsel bir konumda yerel bir sabit belirtmek için bir derleme zamanı hata kendi *constant_declarator*.
*  Bir değişkenin kapsamını bir parçası olarak bildirilen bir *foreach_statement*, *using_statement*, *lock_statement* veya *query_expression* olduğu verilen yapısının genişletme tarafından belirlenir.

Ad alanı, sınıf, yapı veya sabit listesi üyesi kapsamında üyesi bildirimi önündeki bir metinsel konumu üye başvurmak mümkündür. Örneğin:
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Burada, geçerli `F` başvurmak için `i` bildirilmeden önce.

İsteğe bağlı olarak yerel bir değişken kapsamında önündeki değerinin metinsel bir konumda yerel değişkene başvurmak için bir derleme zamanı hatası olmayan *local_variable_declarator* yerel değişkenin. Örneğin:
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

İçinde `F` yukarıdaki yöntemi, ilk atamaya `i` dış kapsamda bildirilen alan özellikle başvurmuyor. Bunun yerine yerel bir değişkene başvuruyor ve metin içeriğini eklemek değişkenin bildirimi kendisinden nedeniyle, bir derleme zamanı hatası oluşur. İçinde `G` yöntemi, kullanımını `j` başlatıcıda bildirimi için `j` geçersiz kullanımı önünde değil çünkü *local_variable_declarator*. İçinde `H` yöntemi, bir sonraki *local_variable_declarator* daha önceki bir bildirilen yerel değişken doğru başvurduğu *local_variable_declarator* aynı  *local_variable_declaration*.

Yerel değişkenler için içeriğin kapsamını belirleyen kuralları anlamı bir ifade bağlamda kullanılan bir ad, her zaman bir blok içindeki aynı olacağını garanti etmek için tasarlanmıştır. Yerel bir değişkenin kapsamını, yalnızca kendi bildirimden bloğun sonuna kadar genişletmek için olsaydı, ardından yukarıdaki örnekte, örnek değişkeni ilk atama atadığınız ve ikinci atama, büyük olasılıkla için önde gelen bir yerel değişkene atadığınız Blok ifadeleri yeniden düzenlenecek için daha sonra, derleme zamanı hataları.

Bir blok içinde bir ad anlamını adı kullanıldığı bağlamı göre farklılık gösterebilir. Örnekte
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
adı `A` yerel bir değişkene başvurmak için kullanılan bir ifade bağlamda `A` ve sınıfa başvurmak için bir tür bağlamında `A`.

### <a name="name-hiding"></a>Ad gizleme

Kapsam, bir varlığın varlık bildirimi alandan daha fazla program metni genellikle kapsar. Özellikle, bir varlığın kapsamı aynı ada sahip bir varlık içeren yeni bir bildirim alanları tanıtan bildirimler içerebilir. Bu tür bildirimleri neden olacak orijinal varlık ***gizli***. Buna karşılık, bir varlık olarak kabul edilir ***görünür*** değil gizlenen.

İç içe geçirme ve ne zaman devralma yoluyla kapsamları çakışan aracılığıyla kapsamları çakışan ad gizleme gerçekleşir. Gizleme iki tür özellikleri aşağıdaki bölümlerde açıklanmıştır.

#### <a name="hiding-through-nesting"></a>İç içe geçme aracılığıyla gizleme

Ad gizleme aracılığıyla iç içe geçme, iç içe geçmiş ad alanlarından veya türlerinden içinde iç içe türleri sınıflar veya yapılar ve parametre ve yerel değişken bildirimleri sonucunda sonucu olarak ad alanları, sonucu olarak ortaya çıkabilir.

Örnekte
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
içinde `F` yöntemi, bir örnek değişkeni `i` yerel değişkeni tarafından gizleniyor `i`, ancak içinde `G` yöntemi `i` hala örneği değişkenine başvuruyor.

Bir iç kapsamda bir ada bir adı bir dış kapsamdaki gizler, bu adı aşırı yüklenmiş tüm oluşumlarını gizler. Örnekte
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
çağrı `F(1)` çağırır `F` bildirilen `Inner` çünkü dış tüm oluşumlarını `F` iç bildirim tarafından gizlenmiş. Aynı nedenle, arama `F("Hello")` bir derleme zamanı hatası oluşur.

#### <a name="hiding-through-inheritance"></a>Devralma yoluyla gizleme

Devralma üzerinden ad gizleme sınıflarını veya yapılarını temel sınıftan devralınan adı yeniden bildirmek oluşur. Bu tür bir ad gizleme aşağıdaki biçimlerden birini alır:

*  Bir sabit, alan, özelliği, olay veya bir sınıf ya da yapı birimine türü aynı ada sahip tüm taban sınıfı üyelerini gizler.
*  Bir sınıf ya da struct'a dahil edilen bir yöntem, yöntem olmayan taban sınıf üyelerinin tamamı aynı ada sahip ve (yöntem adı ve parametre sayısı, değiştiriciler ve türleri) aynı imzaya sahip tüm taban sınıfı yöntemlerini gizler.
*  Bir sınıf veya yapı içinde tanıtılan bir dizin oluşturucu (parametre sayısı ve türleri) aynı imzaya sahip tüm taban sınıfı dizin oluşturucularını gizler.

İşleç bildirimi kuralları ([işleçleri](classes.md#operators)) temel sınıfta operatör olarak aynı imzaya sahip bir işleç bildirmek üzere bir türetilmiş sınıfta imkansız hale. Bu nedenle, işleçleri asla birbirleriyle gizleyin.

Bir dış kapsam adı gizleme aykırı devralınan bir kapsamdan erişilebilir bir adı gizleyerek bildirilmesini uyarı neden olur. Örnekte
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
bildirimi `F` içinde `Derived` bildirilmesini bir uyarısına neden olur. Temel sınıflar ayrı evrimi engelleyen, devralınan bir adı gizleyerek bir hata değildir özellikle. Örneğin, yukarıdaki durumda çünkü gelmiş olabilir daha sonraki bir sürümünü `Base` sunulan bir `F` sınıfı önceki bir sürümde bulunmamıştır yöntemi. Yukarıdaki durumu hata olsaydı, sonra ayrı ayrı tutulan Sınıf Kitaplığı'nda bir temel sınıf için yapılan herhangi bir değişiklik potansiyel olarak türetilmiş sınıfları geçersiz olmasına neden olabilir.

Devralınan bir adı gizleyerek neden uyarı aracılığıyla kaldırılabilir `new` değiştiricisi:
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

`new` Değiştiricisi gösterir `F` içinde `Derived` "Yeni" ve bu gerçekten de devralınan bir üyeyi gizlemek üzere tasarlanmış olduğunu.

Yeni üye bildirimi yalnızca yeni üye kapsamında devralınmış bir üyeyi gizler.

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

Bildirimi yukarıdaki örnekte `F` içinde `Derived` gizler `F` , öğesinden devralınan `Base`, ancak bu yana yeni `F` içinde `Derived` özel erişimi olduğunu kapsamı kapsamaz `MoreDerived` . Bu nedenle, arama `F()` içinde `MoreDerived.G` geçerli olduğundan ve çağıracağı `Base.F`.

## <a name="namespace-and-type-names"></a>Namespace ve tür adları

Çeşitli bağlamlarda bir C# programını gerektiren bir *namespace_name* veya *type_name* belirtilmelidir.

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

A *namespace_name* olduğu bir *namespace_or_type_name* ad alanına başvuruyor. Çözüm, aşağıda açıklandığı gibi aşağıdaki *namespace_or_type_name* , bir *namespace_name* bir ad alanına başvurmalıdır veya aksi halde bir derleme zamanı hatası oluşur. Hiçbir tür bağımsız değişkenleri ([tür bağımsız değişkenleri](types.md#type-arguments)) içinde mevcut olabilen bir *namespace_name* (türler tür bağımsız değişkenleri olabilir yalnızca).

A *type_name* olduğu bir *namespace_or_type_name* bir türe başvuruyor. Çözüm, aşağıda açıklandığı gibi aşağıdaki *namespace_or_type_name* , bir *type_name* bir türe başvurmalıdır veya aksi halde bir derleme zamanı hatası oluşur.

Varsa *namespace_or_type_name* tam-diğer ad-anlamını olan açıklandığı gibi üye [Namespace diğer adı niteleyicileri](namespaces.md#namespace-alias-qualifiers). Aksi takdirde, bir *namespace_or_type_name* dört forms birine sahiptir:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

Burada `I` tek bir tanımlayıcı `N` olduğu bir *namespace_or_type_name* ve `<A1, ..., Ak>` isteğe bağlıdır *type_argument_list*. Hiçbir *type_argument_list* olduğundan belirtilen göz önünde bulundurun `k` sıfır olmalıdır.

Anlamı bir *namespace_or_type_name* şu şekilde belirlenir:

*   Varsa *namespace_or_type_name* biçimindedir `I` veya formun `I<A1, ..., Ak>`:
    * Varsa `K` sıfırdır ve *namespace_or_type_name* genel yöntem bildiriminde içinde görünür ([yöntemleri](classes.md#methods)) ve bu bildirimi bir tür parametresi içeriyorsa ([türü parametreleri](classes.md#type-parameters)) adıyla `I`, ardından *namespace_or_type_name* bu tür parametresine başvuruyor.
    * Aksi takdirde *namespace_or_type_name* türü bildiriminden sonra her örnek türü için görüntülenen `T` ([örnek türü](classes.md#the-instance-type)) itibaren bu türdeki örnek türü bildirim ve (varsa) her kapsayan bir class veya struct bildiriminde örneği türü ile devam etmeden:
        * Varsa `K` sıfır ve bildirimi `T` ada sahip bir tür parametresi içeren `I`, ardından *namespace_or_type_name* bu tür parametresine başvuran.
        * Aksi takdirde *namespace_or_type_name* türü bildirimi gövdesi içinde görünür ve `T` veya temel türlerinden birinin adına sahip iç içe erişilebilir türü içeren `I` ve `K`  tür parametrelerindeki, ardından *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder. Birden fazla tür ise daha türetilmiş türde bildirilen türü seçilir. Tür olmayan üyeler (sabitleri, alanları, yöntemleri, özellikleri, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler ve statik oluşturucular) ve tür üyelerinin farklı sayıda tür parametreleri ile anlamını belirlenirken yoksayılacak olduğunu unutmayın. *namespace_or_type_name*.
    * Önceki adımları her ad alanı için daha sonra başarısız olursa `N`, ad alanı ile başlayan *namespace_or_type_name* her kapsayan ad uzayı ile (eğer varsa) devam etmeden ve ile biten gerçekleşir Genel ad alanı, aşağıdaki adımları bir varlığı bulunana kadar değerlendirilir:
        * Varsa `K` sıfırdır ve `I` bir ad alanındaki adı `N`, ardından:
            * Varsa konumu burada *namespace_or_type_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive* veya *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *namespace_or_type_name* belirsiz ve bir derleme zamanı hatası oluşur.
            * Aksi takdirde, *namespace_or_type_name* adlı isim uzayına başvuruyor `I` içinde `N`.
        * Aksi takdirde `N` adına sahip bir erişilebilir türü içeren `I` ve `K`  tür parametreleri:
            * Varsa `K` sıfır ve konumun burada *namespace_or_type_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive*  veya *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *namespace_or_type_name* belirsiz ve derleme zamanı hata oluşur.
            * Aksi takdirde, *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.
        * Aksi halde, konum burada *namespace_or_type_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N`:
            * Varsa `K` sıfırdır ve ad alanı bildirimi içeren bir *extern_alias_directive* veya *using_alias_directive* , ad ilişkilendirir `I` bir içeri aktarılan ad alanıyla veya tür, ardından *namespace_or_type_name* bu ad alanı veya tür ifade eder.
            * Aksi takdirde, ad alanları ve tür bildirimleri tarafından aldıysanız *using_namespace_directive*s ve *using_alias_directive*ad alanı bildiriminin bir s içeren tam olarak bir erişilebilir türü adına sahip `I` ve `K`  tür parametrelerindeki, ardından *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.
            * Aksi takdirde, ad alanları ve tür bildirimleri tarafından aldıysanız *using_namespace_directive*s ve *using_alias_directive*ad alanı bildiriminin bir s erişilebilir birden fazla tür içeriyor adına sahip `I` ve `K`  tür parametrelerindeki, ardından *namespace_or_type_name* belirsiz ve hata oluşur.
    * Aksi takdirde, *namespace_or_type_name* olup tanımsız ve bir derleme zamanı hatası oluşur.
*  Aksi takdirde, *namespace_or_type_name* biçimindedir `N.I` veya formun `N.I<A1, ..., Ak>`. `N` ilk olarak çözümlenen bir *namespace_or_type_name*. Varsa çözünürlüğünü `N` bir derleme zamanı hatası oluşur başarılı değil. Aksi takdirde, `N.I` veya `N.I<A1, ..., Ak>` gibi çözümlenir:
    * Varsa `K` sıfır ve `N` bir isim uzayına başvuruyor ve `N` ada sahip iç içe geçmiş bir ad alanı içeren `I`, ardından *namespace_or_type_name* , iç içe geçmiş ad alanına başvuruyor.
    * Aksi takdirde `N` bir isim uzayına başvuruyor ve `N` adına sahip bir erişilebilir türü içeren `I` ve `K`  tür parametrelerindeki, ardından *namespace_or_type_name* başvurur Bu türe belirli tür bağımsız değişkenleri ile oluşturulmuş.
    * Aksi takdirde `N` (büyük olasılıkla yapılandırılmış) bir sınıf veya yapı türe başvuruyor ve `N` veya iç içe bir erişilebilir tür adına sahip tüm temel sınıflarını içeren `I` ve `K`  parametreleri, ardından yazın*namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder. Birden fazla tür ise daha türetilmiş türde bildirilen türü seçilir. Anlamını unutmayın `N.I` temel sınıf belirtimi çözümünün bir parçası belirlenen `N` sonra doğrudan temel sınıfını `N` nesne olarak kabul edilir ([temel sınıflar](classes.md#base-classes)).
    * Aksi takdirde, `N.I` geçersiz bir *namespace_or_type_name*, ve bir derleme zamanı hatası oluşur.

A *namespace_or_type_name* statik sınıf başvurmak için izin verilir ([statik sınıflar](classes.md#static-classes)) yalnızca

*  *Namespace_or_type_name* olduğu `T` içinde bir *namespace_or_type_name* formun `T.I`, veya
*  *Namespace_or_type_name* olduğu `T` içinde bir *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) biçiminde `typeof(T)`.

### <a name="fully-qualified-names"></a>Tam olarak nitelenmiş adlar

Her ad alanı ve türü olan bir ***tam nitelikli ad***, hangi benzersiz olarak tanımlayan ad alanı veya tür diğerleri arasından. Tam adı ad alanı veya tür `N` şu şekilde belirlenir:

*  Varsa `N` üye tam adı genel ad alanı olan `N`.
*  Aksi halde, tam adı olduğunu `S.N`burada `S` tam adı ad alanı veya tür, `N` bildirilir.

Diğer bir deyişle, tam olarak nitelenmiş adını `N` neden tanımlayıcıları tam hiyerarşik yolu `N`genel ad alanından başlayan. Her bir ad alanı veya tür üyesinin benzersiz bir adı olması gerektiğinden tam adı ad alanı veya tür her zaman benzersiz olduğunu izler.

Aşağıdaki örnekte, birden fazla ad alanı ve tür bildirimleri ilişkili tam adları ile birlikte gösterilir.
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a>Otomatik bellek yönetimi

C# geliştiricilerin el ile ayırarak ve nesnelerin kapladığı bellek serbest bırakma döngüsünü boşaltır otomatik bellek yönetimi kullanır. Otomatik bellek yönetimi ilkeleri tarafından uygulanır bir ***çöp toplayıcı***. Bir nesnenin bellek yönetimi yaşam döngüsü aşağıdaki gibidir:

1. Nesne oluşturulduğunda bellek de ayrılır, oluşturucu çalıştırın ve nesne canlı olarak kabul edilir.
2. Nesne veya herhangi bir bölümünü tüm olası yürütme devamını tarafından erişilemez, Yıkıcılar çalışması dışında nesne artık kullanımda olarak kabul edilir ve yok etme için uygun hale gelir. C# derleyicisi ve atık toplayıcı bir nesneye başvuran gelecekte kullanılabilir belirlemek için kod çözümleme tercih edebilirsiniz. Örneği için kapsamda olan yerel bir değişken yalnızca var olan bir nesneye başvuru olduğu halde bu yerel değişkeni hiçbir zaman tüm olası devamı olarak, geçerli yürütme yürütme yordamda işaret edecek şekilde adlandırılır, çöp toplayıcı olabilir (ancak değil için gerekli) nesne olarak artık kullanımda değerlendir.
3. Nesne yok etme için uygun olduğunda, daha sonra belirtilmeyen bir saat yok Edicisi ([yok ediciler](classes.md#destructors)) (varsa) nesne çalıştırın. Normal koşullar altında nesnenin yok Edicisi, uygulamaya özel API'leri bu davranışı geçersiz kılınmasına izin verebilir ancak yalnızca bir kez çalıştırılır.
4. Nesnenin yok Edicisi çalıştırıldığında, nesne veya herhangi bir bölümünü tarafından yürütme yıkıcı, çalıştırılmasını da dahil olmak üzere, tüm olası devamlılık erişilemiyorsa nesne erişilemez olarak kabul edilir ve nesne koleksiyonu için uygun hale gelir.
5. Son olarak, nesne koleksiyonu için uygun hale geldikten sonra bir süre sonra atık toplayıcı o nesne ile ilişkili bellek kazandırır.

Atık toplayıcı nesne kullanımı hakkında bilgi tutar ve bellek yönetimi ve bir nesneye bir nesne dışında yeniden konumlandırmakta artık kullanımda veya erişilemez durumda olduğunda, yeni oluşturulan bir nesne bulmak için bellek nerede gibi kararları almak için bu bilgileri kullanır.

Çöp toplayıcı var olup olmadığını varsayar gibi diğer diller, C# atık toplayıcı çok çeşitli bellek yönetimi ilkeleri uygulayabilir şekilde tasarlanmıştır. Örneğin, C# yok ediciler çalıştırılması veya uygun oldukları hemen sonra nesnelerin toplandığını veya yıkıcıları herhangi belirli bir iş parçacığı veya herhangi bir sırada çalıştırılması gerektirmez.

Atık toplayıcının davranışı, sınıfında statik yöntemler aracılığıyla belirli bir ölçüde denetlenebilir `System.GC`. Bu sınıf, bir koleksiyon gerçekleşmesi için çalıştırın (veya çalıştırma için), Yıkıcılar istemek için kullanılan ve VS.

Çöp Toplayıcısı nesneleri toplar ve Yıkıcılar çalıştırmak ne zaman karar içinde geniş enlem izin verildiğinden, uyumlu bir uygulaması aşağıdaki kodda gösterilen farklıdır çıktıları üretebilir. Program
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
sınıfının bir örneğini oluşturur `A` ve sınıf örneği `B`. Bu nesneler çöp toplama işlemi için uygun hale olduğunda değişkeni `b` değeri atanır `null`, bu yana bu tarihten sonra bunlara erişmek kullanıcı tarafından yazılan kodlar için mümkün değildir. Çıkış şunlardan biri olabilir.
```
Destruct instance of A
Destruct instance of B
```
veya
```
Destruct instance of B
Destruct instance of A
```
hangi nesneler çöp olarak toplanacak olduklarından dil hiçbir sırası kısıtlamaları getirir.

Zarif durumda "yok etme için uygun" ve "koleksiyonu için uygun" arasındaki fark önemli olabilir. Örneğin,
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

Yukarıdaki programında çöp toplayıcı yok edicisinde seçerse `A` yıkıcısı önce `B`, bu programın çıkışı gelebilir:
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

Olsa da unutmayın örneğini `A` kullanımda olmadığı ve `A`'s yok Edicisi çalıştırıldığı, yöntemleri için yine de mümkündür `A` (Bu durumda, `F`) başka bir yok ediciden çağrılabilir. Ayrıca, yok edicinin çalışan bir nesneyi yeniden ana hat programından kullanılabilir duruma neden olabileceğini unutmayın. Bu durumda, çalışmasını `B`yok Edicisi örneğini neden kullanıcının `A` olduğu daha önce de kullanan dinamik başvuru tarafından erişilebilir duruma gelmesi `Test.RefA`. Çağrısından sonra `WaitForPendingFinalizers`, örneğini `B` koleksiyonu, ancak örneği için uygun `A` başvurusu nedeniyle değil, `Test.RefA`.

Karışıklık ve beklenmeyen davranışları önlemek için genellikle bir yok ediciler için yalnızca kendi nesnenin kendi alanlarında depolanan veriler üzerinde temizleme işlemini yapmak için ve tüm Eylemler, başvurulan nesneler veya static alanlar gerçekleştirmemeye fikirdir.

Yıkıcıları kullanma uygulayan bir sınıf olanak alternatiftir `System.IDisposable` arabirimi. Böylece, bir kaynak olarak nesne erişerek nesne kaynaklarını genellikle serbest bırakmak ne zaman belirlemek istemci nesne bir `using` deyimi ([using deyimi](statements.md#the-using-statement)).

## <a name="execution-order"></a>Yürütme sırası

Yan etkilerini çalışan her bir iş parçacığı, kritik yürütme noktalarda korunur, C# programının yürütme devam eder. A ***yan etkisi*** bir okuma veya yazma geçici bir alan, geçici olmayan değişkenine yazma, yazma için bir dış kaynağa ve bir özel durum olarak tanımlanır. Volatile alanlara başvurular, bu yan etkileri sırasını gerekir muhafaza edilir kritik yürütme noktaları olan ([geçici alanları](classes.md#volatile-fields)), `lock` deyimleri ([lock deyiminin](statements.md#the-lock-statement)), ve iş parçacığı oluşturma ve sonlandırma. C# programının, aşağıdaki kısıtlamalara tabi yürütme sırasını değiştirmek yürütme ortamı ücretsizdir:

*  Veri bağımlılığı yürütme iş parçacığının içinden korunur. Diğer bir deyişle, tüm iş parçacığı deyimlerinde özgün program sırasıyla yürütüldü gibi her değişkenin değeri olarak hesaplanır.
*  Kuralları korunur başlatma sıralaması ([alan başlatma](classes.md#field-initialization) ve [değişken başlatıcılar](classes.md#variable-initializers)).
*  Geçici okuma ve yazma işlemleri göre sıralama yan etkilerini korunur ([geçici alanları](classes.md#volatile-fields)). Bu ifadenin değeri kullanılmaz ve gerekli yan etkileri (herhangi bir yöntemi çağırmak veya geçici bir alan erişerek neden dahil) üretilir çıkarabilir ek olarak, yürütme ortamı bir ifade parçası değerlendirme değil. Program yürütme (örneğin, başka bir iş parçacığı tarafından oluşturulan bir özel) zaman uyumsuz bir olay tarafından kesildiğinde, gözlemlenebilir bir yan etkileri özgün program sırasıyla görünür olduğunu garanti edilmez.
