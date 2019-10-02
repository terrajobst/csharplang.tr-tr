---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703999"
---
# <a name="basic-concepts"></a>Temel kavramlar

## <a name="application-startup"></a>Uygulama başlatma

***Giriş noktası*** olan bir derlemeye ***uygulama***denir. Bir uygulama çalıştırıldığında yeni bir ***uygulama etki alanı*** oluşturulur. Bir uygulamanın birçok farklı örneği aynı anda aynı makinede bulunabilir ve her birinin kendi uygulama etki alanına sahip olabilir.

Uygulama etki alanı, uygulama durumu için kapsayıcı görevi gören uygulama yalıtımına izin veriyor. Uygulama etki alanı, uygulamada tanımlanan türler ve kullandığı sınıf kitaplıklarında bir kapsayıcı ve sınır görevi görür. Bir uygulama etki alanına yüklenen türler, başka bir uygulama etki alanına yüklenen aynı türden farklıdır ve nesne örnekleri uygulama etki alanları arasında doğrudan paylaşılmaz. Örneğin, her bir uygulama etki alanının bu türler için kendi statik değişkenlerinin kopyası vardır ve bir tür için statik bir Oluşturucu uygulama etki alanı başına en çok bir kez çalıştırılır. Uygulamalar, uygulama etki alanlarının oluşturulması ve yok edilmesi için uygulamaya özgü ilke veya mekanizmalar sağlamak üzere ücretsizdir.

***Uygulama başlatma*** , yürütme ortamı, uygulamanın giriş noktası olarak adlandırılan belirlenmiş bir yöntemi çağırdığında oluşur. Bu giriş noktası yöntemi her zaman `Main` olarak adlandırılır ve aşağıdaki imzalardan birine sahip olabilir:

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

Gösterildiği gibi, giriş noktası isteğe bağlı olarak bir `int` değeri döndürebilir. Bu dönüş değeri uygulama sonlandırmada ([Uygulama sonlandırma](basic-concepts.md#application-termination)) kullanılır.

Giriş noktası, isteğe bağlı olarak bir biçimsel parametreye sahip olabilir. Parametre herhangi bir ada sahip olabilir, ancak parametrenin türü `string[]` olmalıdır. Biçimsel parametre varsa, yürütme ortamı, uygulama başlatıldığında belirtilen komut satırı bağımsız değişkenlerini içeren `string[]` bağımsız değişkenini oluşturur ve geçirir. @No__t-0 bağımsız değişkeni hiçbir şekilde null değildir, ancak komut satırı bağımsız değişkenleri belirtilmemişse sıfır uzunluğuna sahip olabilir.

Yöntem C# aşırı yüklemesini desteklediğinden, bir sınıf veya yapı, her yöntemin farklı bir imzaya sahip olan birden çok tanımını içerebilir. Ancak, tek bir program içinde, hiçbir sınıf veya yapı, tanımı bir uygulama giriş noktası olarak kullanılmak üzere niteleyen `Main` adlı birden fazla yöntem içerebilir. @No__t-0 ' ın diğer aşırı yüklenmiş sürümlerine izin verilir, ancak birden fazla parametreye sahip olmaları veya yalnızca parametresi `string[]` türünden farklı olabilir.

Bir uygulama birden fazla sınıftan veya yapıların üzerinde oluşturulabilir. Tanımı bir uygulama giriş noktası olarak kullanılmak üzere niteleyen `Main` adlı bir yöntem içeren bu sınıfların veya yapıların birden fazla olması mümkündür. Bu gibi durumlarda, giriş noktası olarak bu `Main` yöntemlerinden birini seçmek için bir dış mekanizmanın (bir komut satırı derleyici seçeneği gibi) kullanılması gerekir.

' C#De, her yöntemin bir sınıfın veya yapının üyesi olarak tanımlanması gerekir. Genellikle, bir yöntemin tanımlanmış erişilebilirliği (belirtilen[Erişilebilirlik](basic-concepts.md#declared-accessibility)), bildiriminde belirtilen erişim değiştiricilerine ([erişim değiştiricilerine](classes.md#access-modifiers)) göre belirlenir ve benzer şekilde, bir türün tanımlanan erişilebilirliği tarafından belirlenir bildiriminde belirtilen erişim değiştiricileri. Verilen bir türün belirli bir yönteminin çağrılabilir olması için, hem tür hem de üyenin erişilebilir olması gerekir. Ancak, uygulama giriş noktası özel bir durumdur. Özellikle, yürütme ortamı uygulamanın giriş noktasına, kendisine ait erişilebilirliği ve kapsayan tür bildirimlerinin belirtilen erişilebilirliğinden bağımsız olarak erişebilir.

Uygulama giriş noktası yöntemi bir genel sınıf bildiriminde bulunmayabilir.

Diğer tüm anlarda giriş noktası yöntemleri giriş noktaları olmayan gibi davranır.

## <a name="application-termination"></a>Uygulama sonlandırma

***Uygulama sonlandırması*** denetim yürütme ortamına geri döndürür.

Uygulamanın ***giriş noktası*** yönteminin dönüş türü `int` ise, döndürülen değer uygulamanın ***sonlandırma durum kodu***olarak işlev görür. Bu kodun amacı, yürütme ortamında başarı veya başarısızlık iletişimine iletişim sağlamaktır.

Giriş noktası yönteminin dönüş türü `void` ise, bu yöntemi sonlandıran sağ küme ayracı (`}`) elde edilir veya ifadesi olmayan bir `return` ifadesi yürütülerek, `0` sonlandırma durum koduna neden olur.

Bir uygulamanın sonlandırmasından önce, daha önce atık olarak toplanmamış tüm nesneleri için yok ediciler, temizleme gizlenmediği (örneğin, `GC.SuppressFinalize` kitaplık yöntemine yapılan bir çağrı tarafından) çağrılır.

## <a name="declarations"></a>Bildirimler

Bir C# programdaki bildirimler programın yapısal öğelerini tanımlar. C#Programlar, tür bildirimleri ve iç içe ad alanı bildirimleri içerebilen ad alanları ([ad alanları](namespaces.md)) kullanılarak düzenlenir. Tür bildirimleri ([tür bildirimleri](namespaces.md#type-declarations)), sınıfları ([sınıflar](classes.md)), yapıları ([yapılar](structs.md)), arabirimleri ([arabirimleri](interfaces.md)), numaralandırmalar ([numaralandırmalar](enums.md)) ve temsilcileri ([Temsilciler](delegates.md)) tanımlamak için kullanılır. Tür bildiriminde izin verilen üye türleri tür bildiriminin biçimine bağlıdır. Örneğin, sınıf bildirimleri sabitler ([sabitler](classes.md#constants)), alanlar ([alanlar](classes.md#fields)), Yöntemler ([Yöntemler](classes.md#methods)), Özellikler ([Özellikler](classes.md#properties)), Olaylar ([Olaylar](classes.md#events)), Dizin oluşturucular ([Dizin oluşturucular](classes.md#indexers)) için bildirimler içerebilir. Operators ([işleçler](classes.md#operators)), örnek oluşturucular ([örnek oluşturucular](classes.md#instance-constructors)), statik oluşturucular ([statik oluşturucular](classes.md#static-constructors)), Yıkıcılar ([Yıkıcılar](classes.md#destructors)) ve iç içe türler ([iç içe türler](classes.md#nested-types)).

Bildirim, bildirimin ait olduğu ***bildirim alanında*** bir ad tanımlar. Aşırı yüklenmiş Üyeler hariç ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir bildirim alanında aynı ada sahip üyeleri tanıtan iki veya daha fazla bildirime sahip bir derleme zamanı hatasıdır. Bir bildirim alanının aynı ada sahip farklı üye türleri içermesi hiçbir şekilde mümkün değildir. Örneğin, bir bildirim alanı hiçbir şekilde bir alanı ve aynı ada sahip bir yöntemi içeremez.

Aşağıda açıklandığı gibi birkaç farklı türde bildirim alanı vardır.

*  Bir programın tüm kaynak dosyalarında, kapsayan *namespace_declaration* içermeyen *namespace_member_declaration*s, ***genel bildirim alanı***olarak adlandırılan tek bir Birleşik bildirim alanının üyeleridir.
*  Bir programın tüm kaynak dosyaları içinde, aynı tam ad alanı adına sahip olan *namespace_declaration*s içindeki *namespace_member_declaration*s tek bir Birleşik bildirim alanının üyeleridir.
*  Her sınıf, yapı veya arabirim bildirimi yeni bir bildirim alanı oluşturur. Adlar, bu bildirim alanına *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s veya *type_parameter*s aracılığıyla tanıtılmıştır. Aşırı yüklenmiş örnek Oluşturucu bildirimleri ve statik Oluşturucu bildirimleri dışında, bir sınıf veya yapı sınıfı veya struct ile aynı ada sahip bir üye bildirimi içeremez. Bir sınıf, yapı veya arabirim, aşırı yüklenmiş yöntemlerin ve Dizin oluşturucuların bildirimine izin verir. Ayrıca, bir sınıf veya yapı, aşırı yüklenmiş örnek oluşturucuların ve işleçlerin bildirimine izin verir. Örneğin, bir sınıf, yapı veya arabirim aynı ada sahip birden çok yöntem bildirimi içerebilir, bu yöntem bildirimleri imzasında ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) farklı olabilir. Temel sınıfların bir sınıfın bildirim alanına katkıda bulunmadığını ve temel arabirimlerin bir arabirimin bildirim alanına katkıda bulunmayacağını unutmayın. Bu nedenle, türetilmiş bir sınıfın veya arabirimin devralınan üye ile aynı ada sahip bir üyeyi bildirmesine izin verilir. Bu tür bir üye devralınan üyeyi ***gizleyecek*** şekilde kabul edilir.
*  Her temsilci bildirimi yeni bir bildirim alanı oluşturur. Adlar, bu bildirim alanına biçimsel parametreler (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s aracılığıyla tanıtılmıştır.
*  Her numaralandırma bildirimi yeni bir bildirim alanı oluşturur. Adlar bu bildirim alanına *enum_member_declarations*aracılığıyla tanıtılmıştır.
*  Her yöntem bildirimi, Dizin Oluşturucu bildirimi, işleç bildirimi, örnek Oluşturucu bildirimi ve anonim işlev, ***yerel değişken bildirim alanı***olarak adlandırılan yeni bir bildirim alanı oluşturur. Adlar, bu bildirim alanına biçimsel parametreler (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s aracılığıyla tanıtılmıştır. İşlev üyesi veya anonim işlevin gövdesi, varsa, yerel değişken bildirim alanı içinde iç içe olarak kabul edilir. Yerel bir değişken bildirim alanı ve aynı ada sahip öğeleri içermesi için iç içe geçmiş yerel değişken bildirim alanı için bir hatadır. Bu nedenle, iç içe geçmiş bir bildirim alanı içinde, bir yerel değişken veya sabit değeri, kapsayan bir bildirim alanında yerel bir değişkenle veya sabitle aynı ada sahip olabilir. İki bildirim alanı, hiçbir bildirim alanı diğeri içermediği sürece aynı ada sahip öğeleri içermesi için mümkündür.
*  Her *blok* veya *switch_block* , ayrıca bir *for* *foreach* ve *using* ifadesi yerel değişkenler ve yerel sabitler için yerel bir değişken bildirim alanı oluşturur. Adlar, bu bildirim alanına *local_variable_declaration*s ve *local_constant_declaration*s aracılığıyla tanıtılmıştır. Bir işlev üyesinin veya anonim işlevin gövdesi içinde veya içinde oluşan blokların, parametreleri için bu işlevler tarafından tanımlanan yerel değişken bildirim alanı içinde iç içe geçmiş olduğunu unutmayın. Bu nedenle, örneğin bir yerel değişkene ve aynı ada sahip bir parametreye sahip bir yöntem olması hatadır.
*  Her *blok* veya *switch_block* , Etiketler için ayrı bir bildirim alanı oluşturur. Adlar, bu bildirim alanına *labeled_statement*s aracılığıyla tanıtılmıştır ve adlara *goto_statement*s aracılığıyla başvurulur. Bir bloğun ***etiket bildirim alanı*** , iç içe geçmiş blokları içerir. Bu nedenle, iç içe bir blok içinde, kapsayan bir blok içindeki bir etiketle aynı ada sahip bir etiketi bildirmek mümkün değildir.

Adların bildirildiği metin sırası genellikle anlamlı değildir. Özellikle, metinsel sıralama, ad alanları, sabitler, Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar, statik oluşturucular ve türler için önemli değildir. Bildirim sırası aşağıdaki yollarla önemlidir:

*  Alan bildirimleri ve yerel değişken bildirimleri için bildirim sırası, başlatıcılarının (varsa) yürütülme sırasını belirler.
*  Yerel değişkenlerin kullanılmadan önce tanımlanması gerekir ([kapsamlar](basic-concepts.md#scopes)).
*  *Constant_expression* değerleri atlandığında enum üye bildirimleri ([enum üyeleri](enums.md#enum-members)) için bildirim sırası önemlidir.

Bir ad alanının bildirim alanı "açık sona erdi" ve aynı tam ada sahip iki ad alanı bildirimi aynı bildirim alanına katkıda bulunur. Örneğin:
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

Yukarıdaki iki ad alanı bildirimi aynı bildirim alanına katkıda bulunur, bu durumda iki sınıfı `Megacorp.Data.Customer` ve `Megacorp.Data.Order` olarak tam adlarla bildirme. İki bildirim aynı bildirim alanına katkıda bulunduğundan, her biri aynı ada sahip bir sınıfın bildirimini içeriyorsa, derleme zamanı hatasına neden olur.

Yukarıda belirtildiği gibi, bir bloğun bildirim alanı, iç içe geçmiş blokları içerir. Bu nedenle, aşağıdaki örnekte, `F` ve `G` yöntemleri derleme zamanı hatasına neden olur çünkü `i` adı dış blokta bildirildiği ve iç blokta yeniden bildirilemez. Ancak, iki `i` ' nin ayrı iç içe olmayan bloklar halinde bildirildiği için `H` ve `I` yöntemleri geçerlidir.

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

## <a name="members"></a>Members

Ad alanları ve türlerin ***üyeleri***vardır. Bir varlığın üyeleri, varlığa yönelik bir başvuruya ve ardından bir "`.`" belirteci ve sonra üyenin adı ile başlayan nitelenmiş bir ad kullanılarak kullanılabilir.

Bir türün üyeleri tür bildiriminde bildirilmiştir veya türün temel sınıfından ***devralınır*** . Bir tür taban sınıftan devralırsa, örnek oluşturucular, Yıkıcılar ve statik oluşturucular hariç olmak üzere temel sınıfın tüm üyeleri türetilmiş türün üyesi olur. Bir temel sınıf üyesinin tanımlanmış erişilebilirliği üyenin devralınıp alınmayacağını denetlemez — devralma, örnek Oluşturucu, statik oluşturucu veya yıkıcı olmayan herhangi bir üyeye genişletilir. Ancak, devralınan bir üye, bildirildiği Erişilebilirlik (tarafından[tanımlanan erişilebilirlik](basic-concepts.md#declared-accessibility)) veya türün kendisindeki bir bildirim tarafından gizlendiği için türetilmiş bir tür içinde erişilebilir olmayabilir ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)).

### <a name="namespace-members"></a>Ad alanı üyeleri

Kapsanan ad alanı olmayan ad alanları ve türler, ***genel ad***alanının üyeleridir. Bu, doğrudan genel bildirim alanında belirtilen adlara karşılık gelir.

Ad alanı içinde belirtilen ad alanları ve türler, bu ad alanının üyeleridir. Bu, doğrudan ad alanının bildirim alanında belirtilen adlara karşılık gelir.

Ad alanlarının erişim kısıtlamaları yoktur. Özel, korumalı veya iç ad alanları bildirmek mümkün değildir ve ad alanı adları her zaman herkese açıktır.

### <a name="struct-members"></a>Yapı üyeleri

Bir yapının üyeleri, yapı biriminde ve yapının doğrudan taban sınıfından devralınan üyelere `System.ValueType` ve dolaylı temel sınıf `object` ' dir.

Basit bir türün üyeleri, basit türün diğer adı olan yapı türünün üyelerine karşılık gelir:

*  @No__t-0 üyeleri, `System.SByte` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Byte` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Int16` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.UInt16` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Int32` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.UInt32` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Int64` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.UInt64` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Char` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Single` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Double` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Decimal` yapısının üyeleridir.
*  @No__t-0 üyeleri, `System.Boolean` yapısının üyeleridir.

### <a name="enumeration-members"></a>Listeleme üyeleri

Bir numaralandırmanın üyeleri, numaralandırmada ve numaralandırmanın doğrudan taban @no__t sınıfından devralınan ve `System.ValueType` ve `object` ' nin dolaylı taban sınıflarından devralan olan sabitlerdir.

### <a name="class-members"></a>Sınıf üyeleri

Bir sınıfın üyeleri, sınıfında ve temel sınıftan devralınan üyelere (taban sınıfına sahip olmayan `object` sınıfı dışında) ait Üyeler. Temel sınıftan devralınan Üyeler, taban sınıfının sabitler, alanları, yöntemleri, özellikleri, olayları, Dizin oluşturucular, işleçler ve türleri ve temel sınıfın statik oluşturucuları değil, temel sınıfın türlerini içerir. Temel sınıf üyeleri, erişilebilirliğine göre devralınmaz.

Bir sınıf bildirimi, sabitler, alanlar, Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar, statik oluşturucular ve türler için bildirimler içerebilir.

@No__t-0 ve `string` üyeleri, diğer adı olan sınıf türlerinin üyelerine doğrudan karşılık gelir:

*  @No__t-0 üyeleri, `System.Object` sınıfının üyeleridir.
*  @No__t-0 üyeleri, `System.String` sınıfının üyeleridir.

### <a name="interface-members"></a>Arabirim üyeleri

Bir arabirimin üyeleri, arabirimde ve arabirimin tüm temel arabirimlerinde belirtilen üyelerdir. @No__t-0 sınıfındaki Üyeler, herhangi bir arabirimin ([arabirim üyesi](interfaces.md#interface-members)) üyesi değil, kesinlikle konuşuyor. Ancak, `object` sınıfındaki Üyeler herhangi bir arabirim türünde ([üye arama](expressions.md#member-lookup)) üye arama yoluyla kullanılabilir.

### <a name="array-members"></a>Dizi üyeleri

Bir dizinin üyeleri, `System.Array` sınıfından devralınan üyelerdir.

### <a name="delegate-members"></a>Temsilci üyeleri

Bir temsilcinin üyeleri, `System.Delegate` sınıfından devralınan üyelerdir.

## <a name="member-access"></a>Üye erişimi

Üyelerin bildirimleri üye erişimi üzerinde denetime izin verir. Bir üyenin erişilebilirliği, varsa, hemen kapsayan türün erişilebilirliği ile birleştirilmiş üyenin tanımlanmış erişilebilirliği ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)) tarafından belirlenir.

Belirli bir üyeye erişime izin verildiğinde, üyeye ***erişilebilir***olduğu söylenir. Buna karşılık, belirli bir üyeye erişime izin verilmediğinde, üyenin ***erişilemez***olduğu söylenir. Bir üyeye erişim, erişimin gerçekleştiği metin konumu üyenin erişilebilirlik etki alanına ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) dahil edildiğinde izin verilir.

### <a name="declared-accessibility"></a>Tanımlanan erişilebilirlik

Bir üyenin ***tanımlanmış erişilebilirliği*** aşağıdakilerden biri olabilir:

*  Public, üye bildiriminde `public` değiştiricisi eklenerek seçilidir. @No__t-0 ' nın sezgisel anlamı "erişim sınırlı değildir".
*  Korumalı, üye bildiriminde `protected` değiştiricisi eklenerek seçilir. @No__t-0 ' ı ' nin sezgisel anlamı, "Access 'in kapsayan sınıf veya içeren bir sınıftan türetilmiş türler" ile sınırlıdır.
*  İç, üye bildiriminde `internal` değiştiricisi eklenerek seçilir. @No__t-0 ' nın sezgisel anlamı "Bu programla sınırlı erişim" dir.
*  Üye bildiriminde hem `protected` hem de `internal` değiştiricisi eklenerek seçilen, korunan dahili (yani protected veya internal). @No__t-0 ' nın sezgisel anlamı, "Bu program ile sınırlı erişim veya içeren sınıftan türetilmiş türler" dir.
*  Özel, üye bildiriminde `private` değiştiricisi eklenerek seçilidir. @No__t-0 ' nın sezgisel anlamı "içerilen türle sınırlı" ' dır.

Bir üye bildiriminin gerçekleştiği içeriğe bağlı olarak, yalnızca belirli bir tanımlı erişilebilirlik türlerine izin verilir. Ayrıca, bir üye bildirimi herhangi bir erişim değiştiricisi içermiyorsa, bildirimin gerçekleştiği bağlam, varsayılan olarak belirtilen erişilebilirliği belirler.

*  Ad alanlarında örtülü olarak `public` erişilebilirliği vardır. Ad alanı bildirimlerinde erişim değiştiricilerine izin verilmez.
*  Derleme birimleri veya ad alanlarında belirtilen türlerin `public` veya `internal` ' i @no__t tarafından belirtilen erişilebilirliği ve varsayılan olarak
*  Sınıf üyeleri beş tür tanımlanmış erişilebilirliği ve varsayılan olarak `private` tarafından tanımlanan erişilebilirliği içerebilir. (Bir sınıfın üyesi olarak belirtilen bir türün beş tür tanımlanmış erişilebilirliği olabilir, ancak bir ad alanının üyesi olarak bildirildiği bir tür yalnızca `public` veya `internal` ile tanımlanmış erişilebilirliği içerebilir.)
*  Yapı üyeleri, `public`, `internal` veya `private` ' den fazla tanımlı erişilebilirliği ve varsayılan olarak, yapılar örtük olarak mühürlenmiş olan erişilebilirliği @no__t. Bir yapıda tanıtılan (yani, bu yapı tarafından devralınmayan) yapı üyelerinin `protected` veya `protected internal` ile tanımlanmış erişilebilirliği olamaz. (Bir yapının üyesi olarak belirtilen bir türün `public`, `internal` veya `private` tarafından tanımlanan erişilebilirliği olabileceğini, ancak bir ad alanının üyesi olarak bildirildiği bir türün yalnızca `public` veya `internal` tarafından tanımlanan erişilebilirliği olabileceğini unutmayın.)
*  Arabirim üyeleri örtük olarak `public` olarak tanımlanmış erişilebilirliği vardır. Arabirim üye bildirimlerinde erişim değiştiricilerine izin verilmez.
*  Numaralandırma üyeleri örtük olarak `public` olarak tanımlanmış erişilebilirliği vardır. Numaralandırma üye bildirimlerinde erişim değiştiricilerine izin verilmez.

### <a name="accessibility-domains"></a>Erişilebilirlik etki alanları

Bir üyenin ***erişilebilirlik etki alanı*** , üyeye erişime izin verilen program metninin (muhtemelen ayrık) bölümlerinden oluşur. Bir üyenin erişilebilirlik etki alanını tanımlama amacıyla bir üyenin bir tür içinde ***bildirilmemiş olması ve*** bir üyenin başka bir tür içinde bildirildiği durumlarda ***iç içe*** olduğu söylenir. Ayrıca, programın ***Program metni*** programın tüm kaynak dosyalarında yer alan tüm program metinleri olarak tanımlanır ve bir türün program metni, bu türün *type_declaration*s (dahil) içindeki tüm program metinleri olarak tanımlanır ( büyük olasılıkla, tür içinde iç içe geçmiş türler).

Önceden tanımlanmış bir türün erişilebilirlik etki alanı (örneğin `object`, `int` veya `double`) sınırsızdır.

@No__t-2 ' de bir programda bildirildiği `T` ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) üst düzey ilişkisiz türün erişilebilirlik etki alanı aşağıdaki gibi tanımlanır:

*  @No__t-0 ' nin belirtilen erişilebilirliği `public` ise, `T` ' nin erişilebilirlik etki alanı `P` ' ün ve `P` ' ün başvurduğu tüm programlar için program metni olur.
*  @No__t-0 ' nin belirtilen erişilebilirliği `internal` ise, `T` ' nin erişilebilirlik etki alanı `P` ' ün program metni olur.

Bu tanımlardan, üst düzey ilişkisiz türdeki erişilebilirlik etki alanının her zaman en azından bu türün bildirildiği programın program metni olduğunu takip eder.

Oluşturulmuş bir tür için erişilebilirlik etki alanı `T<A1, ..., An>`, ilişkisiz genel türdeki `T` ' in erişilebilirlik etki alanının kesişimi ve tür bağımsız değişkenlerinin erişilebilirlik etki alanları `A1, ..., An` ' dir.

@No__t-2 bir programda `T` türünde tanımlanan iç içe üye `M` ' ın erişilebilirlik etki alanı, aşağıdaki gibi tanımlanır (`M` ' ün büyük olasılıkla bir tür olabileceğini gösteren):

*  @No__t-0 ' nin belirtilen erişilebilirliği `public` ise, `M` ' nin erişilebilirlik etki alanı `T` ' ün erişilebilirlik etki alanıdır.
*  @No__t-0 ' ı ' nin belirtilen erişilebilirliği `protected internal` ise, `D` ' nin `P` ' ün program metninin birleşimi ve `T` ' ten türetilmiş herhangi bir türün program metni `P` dışında bir şekilde bildirilmiştir. @No__t-0 ' a ait erişilebilirlik etki alanı, `D` ile `T` erişilebilirlik etki alanının kesişimidir.
*  @No__t-0 ' nin belirtilen erişilebilirliği `protected` ise, `D` ' nin `T` ' ün program metninin birleşimi ve `T` ' ten türetilmiş herhangi bir türün program metni olması gerekir. @No__t-0 ' a ait erişilebilirlik etki alanı, `D` ile `T` erişilebilirlik etki alanının kesişimidir.
*  @No__t-0 ' nin belirtilen erişilebilirliği `internal` ise, `M` ' nin erişilebilirlik etki alanı, `T` ' ün erişilebilirlik etki alanının `P` program metniyle kesişmesi olur.
*  @No__t-0 ' nin belirtilen erişilebilirliği `private` ise, `M` ' nin erişilebilirlik etki alanı `T` ' ün program metni olur.

Bu tanımlardan, iç içe bir üyenin erişilebilirlik etki alanının her zaman en azından üyenin bildirildiği türdeki program metni olduğunu takip eder. Ayrıca, bir üyenin erişilebilirlik etki alanının, üyenin bildirildiği türün erişilebilirlik etki alanından daha da hiç olmadığı anlamına gelir.

Sezgisel koşullarda, bir tür veya üye `M` ' a erişildiğinde, erişime izin verildiğinden emin olmak için aşağıdaki adımlar değerlendirilir:

*  İlk olarak, `M` bir tür içinde bildirilirse (bir derleme birimi veya ad alanı aksine), bu tür erişilebilir değilse bir derleme zamanı hatası oluşur.
*  @No__t-0 `public` ise erişime izin verilir.
*  Aksi takdirde, `M` `protected internal` ise, erişim `M` ' nin bildirildiği programda meydana gelirse veya `M` ' ün bildirildiği ve türetilmiş sınıf türü aracılığıyla gerçekleştiği sınıftan türetilmiş bir sınıf içinde gerçekleşirse erişime izin verilir ([korumalı örnek üyelerine erişim](basic-concepts.md#protected-access-for-instance-members)).
*  Aksi takdirde, `M` `protected` ise, erişim `M` ' nin bildirildiği sınıfta ortaya çıkarsa veya `M` ' ün bildirildiği ve türetilmiş sınıf türü aracılığıyla gerçekleştiği sınıftan türetilmiş bir sınıf içinde gerçekleşirse erişime izin verilir ([korumalı örnek üyelerine erişim](basic-concepts.md#protected-access-for-instance-members)).
*  Aksi takdirde, `M` `internal` ise, `M` ' nin bildirildiği program içinde meydana gelirse erişime izin verilir.
*  Aksi takdirde, `M` `private` ise, `M` ' nin bildirildiği tür içinde oluşması durumunda erişime izin verilir.
*  Aksi takdirde, tür veya üyeye erişilemez ve derleme zamanı hatası oluşur.

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
sınıflar ve Üyeler aşağıdaki erişilebilirlik etki alanlarına sahiptir:

*  @No__t-0 ve `A.X` ' in erişilebilirlik etki alanı sınırsızdır.
*  @No__t-0, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X` ve `B.C.Y` erişilebilirlik etki alanı, içeren programın program metintir.
*  @No__t-0 ' ın erişilebilirlik etki alanı, `A` ' in program metni.
*  @No__t-0 ve `B.D` ' in erişilebilirlik etki alanı, `B.C` ve `B.D` program metni de dahil olmak üzere `B` ' nin program metni.
*  @No__t-0 ' ın erişilebilirlik etki alanı, `B.C` ' in program metni.
*  @No__t-0 ve `B.D.Y` ' in erişilebilirlik etki alanı, `B.C` ve `B.D` program metni de dahil olmak üzere `B` ' nin program metni.
*  @No__t-0 ' ın erişilebilirlik etki alanı, `B.D` ' in program metni.

Örnekte gösterildiği gibi, bir üyenin erişilebilirlik etki alanı, kapsayan türden hiçbir şekilde daha büyük değildir. Örneğin, tüm `X` üyelerinin ortak olarak tanımlanmış erişilebilirliği olsa da, tüm `A.X`, kapsayan bir tür tarafından kısıtlanan erişilebilirlik etki alanlarına sahiptir.

[Üyeler](basic-concepts.md#members)bölümünde açıklandığı gibi, bir temel sınıfın, örnek oluşturucular, Yıkıcılar ve statik oluşturucular hariç tüm üyeleri türetilmiş türler tarafından devralınır. Bu, bir temel sınıfın hatta özel üyelerini içerir. Ancak, özel bir üyenin erişilebilirlik etki alanı yalnızca üyenin bildirildiği türün program metnini içerir. Örnekte
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
`B` sınıfı, `A` sınıfından `x` özel üyesini devralır. Üye özel olduğundan, yalnızca `A` ' in *class_body* içinde erişilebilir. Bu nedenle, `b.x` ' a erişim `A.F` yönteminde başarılı olur, ancak `B.F` yönteminde başarısız olur.

### <a name="protected-access-for-instance-members"></a>Örnek üyeleri için korumalı erişim

@No__t-0 örnek üyesine, bildirildiği sınıfın program metni dışında erişildiğinde ve `protected internal` örnek üyesine, bildirildiği programın program metni dışında erişildiğinde, erişim bir sınıf içinde gerçekleşmelidir içinde bildirildiği sınıftan türetilen bildirim. Ayrıca, bu türetilmiş sınıf türünün bir örneği veya ondan oluşturulan bir sınıf türü üzerinden gerçekleşmesi için erişim gerekir. Bu kısıtlama, bir türetilen sınıfın, Üyeler aynı temel sınıftan devralınsa bile, diğer türetilmiş sınıfların korunan üyelerine erişmesini önler.

@No__t-0 ' ın korunan bir örnek üyesini bildiren bir temel sınıf olmasına izin verin `M` ve `B` ' ten türetilen bir sınıf @no__t 2 ' dir. @No__t-1 *' in içinde, @no__t* -2 ' ye erişim aşağıdaki formlardan birini gerçekleştirebilir:

*  @No__t-2 biçiminde nitelenmemiş bir *type_name* veya *primary_expression* .
*  @No__t-1 `E` türü `T` ' ten *türetilmiş bir sınıf* veya `T` ' ten türetilen bir sınıf,  ' in sınıf türü `D` veya `D` ' den oluşturulan bir sınıf türü.
*  @No__t-1 biçiminde bir *primary_expression* .

Bu erişim formlarına ek olarak, türetilmiş bir sınıf bir *constructor_initializer* ([Oluşturucu başlatıcıları](classes.md#constructor-initializers)) içindeki bir temel sınıfın korumalı örnek oluşturucusuna erişebilir.

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
`A` ' da, erişim `A` ' ün bir örneğinden veya `A` ' ten türetilmiş bir sınıftan gerçekleştiği için, her iki `A` ve `B` ' ün örnekleri aracılığıyla `x` ' e erişmek mümkündür. Ancak, `B` ' ın içinde, `A` `B` ' ten türetilmediğinden, `x` `A` ' nin bir örneği aracılığıyla erişmek mümkün değildir.

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
`x` ' a yönelik üç atama, tümü genel türden oluşturulan sınıf türleri örnekleri aracılığıyla gerçekleştiğinden izin verilir.

### <a name="accessibility-constraints"></a>Erişilebilirlik kısıtlamaları

Dildeki çeşitli yapılar, C# bir türün en az bir üye veya başka bir tür ***olarak erişilebilir*** olmasını gerektirir. @No__t-0 ' @no__t ın erişilebilirlik etki alanı, `M` ' e ait erişilebilirlik etki alanının bir üst kümesi ise, bir üye olarak en az bir üye olarak erişilebilir veya `M` yazın. Diğer bir deyişle, `M` ' ün erişilebilir olduğu tüm bağlamlarda `T` erişilebilir ise `T` en az `M` olarak erişilebilir olur.

Aşağıdaki erişilebilirlik kısıtlamaları vardır:

*  Bir sınıf türünün doğrudan temel sınıfı en azından sınıf türünün kendisi kadar erişilebilir olmalıdır.
*  Arabirim türünün açık temel arabirimleri en az arabirim türünün kendisi kadar erişilebilir olmalıdır.
*  Bir temsilci türünün dönüş türü ve parametre türleri en azından temsilci türünün kendisi kadar erişilebilir olmalıdır.
*  Bir sabit türü en az sabit değer olarak erişilebilir olmalıdır.
*  Alanın türü en azından alanın kendisi kadar erişilebilir olmalıdır.
*  Bir yöntemin dönüş türü ve parametre türleri en az yöntemin kendisi olarak erişilebilir olmalıdır.
*  Özelliğin türü en az özelliğin kendisi olarak erişilebilir olmalıdır.
*  Bir olayın türü en az olayın kendisi olarak erişilebilir olmalıdır.
*  Bir dizin oluşturucunun türü ve parametre türleri en azından dizin oluşturucunun kendisi olarak erişilebilir olmalıdır.
*  Bir işlecin dönüş türü ve parametre türleri en az işlecin kendisi olarak erişilebilir olmalıdır.
*  Örnek oluşturucusunun parametre türleri en azından örnek oluşturucusunun kendisi kadar erişilebilir olmalıdır.

Örnekte
```csharp
class A {...}

public class B: A {...}
```
`A` en az `B` olarak erişilebilir olmadığından, `B` sınıfı derleme zamanı hatasına neden olur.

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
`B` ' deki `H` yöntemi derleme zamanı hatası ile sonuçlanır, çünkü dönüş türü `A` en az Yöntem olarak erişilebilir değildir.

## <a name="signatures-and-overloading"></a>İmzalar ve aşırı yükleme

Yöntemler, örnek oluşturucular, Dizin oluşturucular ve işleçler, ***imzalarına***göre belirlenir:

*  Bir yöntemin imzası, yöntemin adından, tür parametrelerinin sayısına ve her bir biçimsel parametrenin her birinin tür ve türü (değer, başvuru veya çıkış) soldan sağa doğru olarak değerlendirilir. Bu amaçlar için, bir biçimsel parametre türünde oluşan yöntemin tür parametresi, kendi adı tarafından değil, yönteminin tür bağımsız değişkeni listesindeki sıra konumuna göre tanımlanır. Bir yöntemin imzası, dönüş türünü, en sağdaki parametre için belirtilemeyen `params` değiştiricisini veya isteğe bağlı tür parametresi kısıtlamalarını içermez.
*  Bir örnek oluşturucusunun imzası, her bir biçimsel parametrenin türü ve türü (değer, başvuru veya çıkış), soldan sağa doğru olarak değerlendirilir. Örnek oluşturucunun imzası, özellikle de en sağdaki parametre için belirtilemeyen `params` değiştiricisini içermez.
*  Bir dizin oluşturucunun imzası, soldan sağa doğru olarak kabul edilen her bir biçimsel parametre türünden oluşur. Bir dizin oluşturucunun imzası özellikle öğe türünü içermez veya en sağdaki parametre için belirtilemeyen `params` değiştiricisini içermez.
*  Bir işlecin imzası, işlecin adından ve her bir biçimsel parametrelerinin, soldan sağa doğru olarak kabul edilen türünden oluşur. Bir işlecin imzası özellikle sonuç türünü içermez.

İmzalar, sınıflarda, yapılarda ve arabirimlerde üyelerin ***aşırı*** yüklenmesine yönelik etkinleştirme mekanizmasıdır:

*  Yöntemlerin aşırı yüklemesi, bir sınıf, yapı veya arabirimin aynı ada sahip birden çok yöntem bildirmesine izin verdiğinden, imzaları bu sınıf, yapı veya arabirim içinde benzersizdir.
*  Örnek oluşturucuların aşırı yüklenmesi, bir sınıfın veya yapının birden çok örnek Oluşturucu tanımlamasına izin verdiğinden, imzaları bu sınıf veya yapı içinde benzersiz olarak belirtilmelidir.
*  Dizin oluşturucularının aşırı yüklemesi, bir sınıf, yapı veya arabirimin birden çok dizin oluşturma kullanılmasına izin verir, ancak imzaları bu sınıf, yapı veya arabirim içinde benzersizdir.
*  İşleçlerin aşırı yüklemesi, bir sınıf veya yapının aynı ada sahip birden çok işleç tanımlamasına izin verdiğinden, imzaları bu sınıf veya yapı içinde benzersizdir.

@No__t-0 ve `ref` parametre değiştiricilerin bir imzanın parçası olarak kabul edilse de, tek bir tür içinde belirtilen Üyeler yalnızca `ref` ve `out` ile imzasında farklı olamaz. İki üye, `out` değiştiricilerine sahip her iki yöntemde de `ref` değiştiricilere değiştirilmişse aynı türde olan imzalara sahip aynı türde bir derleme zamanı hatası oluşur. İmza eşleştirmesinin diğer amaçları (örn. gizleme veya geçersiz kılma) için `ref` ve `out`, imzanın bir parçası olarak değerlendirilir ve birbirleriyle eşleşmez. (Bu kısıtlama, programların yalnızca C# `ref` ve `out` ' de farklı yöntemler tanımlamak için bir yol sağlamayan ortak dil ALTYAPıSıNDA (CLI) çalışmaya kolayca çevrilmesine izin versağlamaktır.)

İmzaların amaçları doğrultusunda `object` ve `dynamic` türleri aynı kabul edilir. Bu nedenle, tek bir türde tanımlanan Üyeler yalnızca `object` ve `dynamic` ile İmzada farklı değildir.

Aşağıdaki örnek, imzaları ile birlikte aşırı yüklenmiş yöntem bildirimlerinin bir kümesini gösterir.
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

@No__t-0 ve `out` parametre değiştiricilerin ([Yöntem parametreleri](classes.md#method-parameters)) bir imzanın parçası olduğunu unutmayın. Bu nedenle, `F(int)` ve `F(ref int)` benzersiz imzalardır. Ancak, `F(ref int)` ve `F(out int)`, imzaları yalnızca `ref` ve `out` farklı olduğundan aynı arabirim içinde bildirilemez. Ayrıca, dönüş türü ve `params` değiştiricisinin bir imzanın parçası olmadığına ve bu nedenle yalnızca dönüş türüne göre veya `params` değiştiricisinin dahil edilmesi veya dışlamasına göre aşırı yüklenmesi mümkün değildir. Bu nedenle, yukarıda tanımlanan `F(int)` ve `F(params string[])` yöntemlerinin bildirimleri derleme zamanı hatası ile sonuçlanır.

## <a name="scopes"></a>Kapsamları

Bir adın ***kapsamı*** , adı nitelemeden ad tarafından tanımlanan varlığa başvuruda bulunmak mümkün olduğu program metninin bölgesidir. Kapsamlar ***iç içe***olabilir ve bir iç kapsam bir dış kapsamdan bir adın anlamını yeniden bildirebilir (Bu, ancak, iç içe bir blok içindeki [Bildirimler](basic-concepts.md#declarations) tarafından uygulanan kısıtlamayı kaldırma, bir yerel değişken ile kapsayan bir blok içindeki yerel değişkenle aynı ad. Daha sonra dış kapsamdaki ad, iç kapsamın kapsadığı program metni bölgesinde ***gizli*** olarak belirlenir ve dış ada erişim yalnızca adı niteleyerek mümkündür.

*  Kapsayan *namespace_declaration* içermeyen bir *namespace_member_declaration* ([Namespace üyeleri](namespaces.md#namespace-members)) tarafından belirtilen bir ad alanı üyesinin kapsamı, tüm program metindir.
*  Tam adı `N` olan bir *namespace_declaration* içinde bir *namespace_member_declaration* tarafından belirtilen bir ad alanı üyesinin kapsamı, tamamı olan her *namespace_declaration* *namespace_body* nitelenmiş ad `N` ' tir veya `N` ve sonrasında bir noktayla başlar.
*  Bir *extern_alias_directive* tarafından tanımlanan ad kapsamı, derleme birimi veya ad alanı gövdesinin hemen bulunduğu *using_directive*s, *global_attributes* ve *namespace_member_declaration*öğelerinin üzerinde genişletilir. Bir *extern_alias_directive* , temel alınan bildirim alanına yeni üye katkıda bulunmaz. Diğer bir deyişle, *extern_alias_directive* geçişli değildir, ancak bunun yerine yalnızca, oluştuğu derleme birimini veya ad alanı gövdesini etkiler.
*  Bir *using_directive* tarafından tanımlanan veya içeri aktarılan bir ad kapsamı ([yönergeler kullanılarak](namespaces.md#using-directives)), using_directive *compilation_unit* veya *namespace_body* içindeki *namespace_member_declaration*sgerçekleşir. Bir *using_directive* , belirli bir *compilation_unit* veya *namespace_body*içinde kullanılabilir bir veya daha fazla ad alanı, tür veya üye adı oluşturabilir, ancak temel alınan bildirim alanına hiçbir yeni üye katkıda bulunmaz. Diğer bir deyişle, *using_directive* geçişli değildir, ancak bunun gerçekleştiği *compilation_unit* veya *namespace_body* etkiler.
*  *Class_declaration* ([sınıf bildirimleri](classes.md#class-declarations)) üzerinde bir *type_parameter_list* tarafından tanımlanan bir tür parametresinin kapsamı, *class_base*, *type_parameter_constraints_clause*s ve *class_body*  *class_declaration*.
*  *Struct_declaration* ([struct bildirimleri](structs.md#struct-declarations)) üzerinde bir *type_parameter_list* tarafından tanımlanan bir tür parametresinin kapsamı, *struct_interfaces*, *type_parameter_constraints_clause*s ve *struct_body* Bu *struct_declaration*.
*  Bir *interface_declaration* ([arabirim bildirimleri](interfaces.md#interface-declarations)) üzerinde bir *type_parameter_list* tarafından tanımlanan tür parametresinin kapsamı *interface_base*, *type_parameter_constraints_clause*s ve *interface_body* bu *interface_declaration*.
*  Bir *delegate_declaration* ([Delegate bildirimleri](delegates.md#delegate-declarations)) üzerinde bir *type_parameter_list* tarafından tanımlanan bir tür parametresinin kapsamı, *Sbayrak*, *formal_parameter_list*ve *type_parameter_constraints_clause* bu *delegate_declaration*.
*  Bir *class_member_declaration* ([sınıf gövdesi](classes.md#class-body)) tarafından belirtilen üyenin kapsamı, bildirimin gerçekleştiği *class_body* . Ayrıca, bir sınıf üyesinin kapsamı, üyenin erişilebilirlik etki alanına ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) dahil edilen türetilmiş sınıfların *class_body* genişletir.
*  Bir *struct_member_declaration* ([struct Members](structs.md#struct-members)) tarafından belirtilen bir üyenin kapsamı, bildirimin gerçekleştiği *struct_body* .
*  Bir *enum_member_declaration* ([enum üyeleri](enums.md#enum-members)) tarafından belirtilen bir üyenin kapsamı, bildirimin gerçekleştiği *enum_body* .
*  Bir *method_declaration* ([Methods](classes.md#methods)) içinde belirtilen parametrenin kapsamı, *method_declaration* *method_body* .
*  Bir *indexer_declaration* ([Dizin oluşturucular](classes.md#indexers)) içinde belirtilen parametrenin kapsamı, *indexer_declaration* *accessor_declarations* .
*  Bir *operator_declaration* ([Operators](classes.md#operators)) içinde belirtilen parametrenin kapsamı, bu *operator_declaration*'in *bloğudur* .
*  Bir *constructor_declaration* ([örnek oluşturucular](classes.md#instance-constructors)) içinde belirtilen parametrenin kapsamı, *constructor_declaration* *constructor_initializer* ve *bloğudur* .
*  Bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) içinde belirtilen parametrenin kapsamı, *lambda_expression* *anonymous_function_body*
*  Bir *anonymous_method_expression* ([anonim işlev ifadelerinde](expressions.md#anonymous-function-expressions)) içinde belirtilen bir parametrenin kapsamı, bu *anonymous_method_expression*'in *bloğudur* .
*  Bir *labeled_statement* ([etiketli deyimler](statements.md#labeled-statements)) içinde belirtilen etiketin kapsamı, bildirimin gerçekleştiği *bloğudur* .
*  Bir *local_variable_declaration* ([yerel değişken bildirimlerinde](statements.md#local-variable-declarations)) içinde belirtilen yerel bir değişkenin kapsamı, bildirimin gerçekleştiği bloğudur.
*  Bir `switch` ifadesinde ([switch ifadesinin](statements.md#the-switch-statement)) bir *switch_block* içinde belirtilen yerel bir değişkenin kapsamı *switch_block*.
*  @No__t-1 ifadesinin bir *for_initializer* içinde ([for ifadesinin](statements.md#the-for-statement)) belirtilen bir yerel değişkenin kapsamı, *for_initializer*, *for_condition*, *for_iterator* *ve içindeki* `for` ekstresi.
*  Bir *local_constant_declaration* ([yerel sabit bildirimlerinde](statements.md#local-constant-declarations)) içinde belirtilen yerel bir sabit kapsam, bildirimin gerçekleştiği bloğudur. Bu, *constant_declarator*' den önceki bir metinsel konumda yerel bir sabite başvuruda bulunmak için derleme zamanı hatasıdır.
*  Bir *foreach_statement*, *using_statement*, *lock_statement* veya *query_expression* 'nin bir parçası olarak belirtilen bir değişkenin kapsamı verilen yapının genişlemesiyle belirlenir.

Bir ad alanı, sınıf, yapı veya numaralandırma üyesi kapsamında, üyeye üye bildiriminden önce gelen metinsel bir konumda başvurmak mümkündür. Örneğin:
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
Burada, `F` ' ı, bildirilmeden önce `i` ' e başvuracak şekilde geçerlidir.

Yerel değişken kapsamında, yerel değişkene *local_variable_declarator* önünde bulunan bir metinsel konumdaki yerel değişkene başvurabileceğiniz bir derleme zamanı hatasıdır. Örneğin:
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

Yukarıdaki `F` yönteminde, `i` ' e ilk atama, özellikle dış kapsamda belirtilen alana başvurmaz. Bunun yerine, yerel değişkene başvurur ve değişkenin bildiriminden önce metin içeriğini eklemek bir derleme zamanı hatasına neden olur. @No__t-0 yönteminde, kullanım *local_variable_declarator*önünde olmadığından, `j` bildirimi başlatıcısında `j` kullanımı geçerlidir. @No__t-0 yönteminde, sonraki *local_variable_declarator* doğru bir şekilde, aynı *local_variable_declaration*içindeki önceki bir *local_variable_declarator* içinde belirtilen yerel bir değişkene başvurur.

Yerel değişkenlerin kapsam kuralları, bir ifade bağlamında kullanılan bir adın anlamını her zaman bir blok içinde aynı olduğundan emin olmak için tasarlanmıştır. Yerel bir değişkenin kapsamı yalnızca bildiriminden bloğunun sonuna genişletirse, yukarıdaki örnekte, ilk atama örnek değişkenine atanır ve ikinci atama yerel değişkene atanır, büyük olasılıkla şu şekilde önde gelen bloğun deyimlerinin daha sonra yeniden düzenlenme durumunda derleme zamanı hataları.

Bir blok içindeki bir adın anlamı, adının kullanıldığı bağlama göre farklılık gösterebilir. Örnekte
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
`A` adı, `A` yerel değişkenine ve bir tür bağlamında `A` sınıfına başvuracak şekilde bir ifade bağlamında kullanılır.

### <a name="name-hiding"></a>Ad gizleme

Bir varlığın kapsamı genellikle varlığın bildirim alanından daha fazla program metnini kapsar. Özellikle, bir varlığın kapsamı, aynı ada sahip varlıkları içeren yeni bildirim alanları sunan bildirimler içerebilir. Bu tür bildirimler orijinal varlığın ***gizli***hale gelmesine neden olur. Buna karşılık, bir varlık gizlenmediği zaman ***görünür*** olarak kabul edilir.

Ad gizleme kapsamları, iç içe geçme yoluyla çakıştığında ve kapsamlar devralma yoluyla çakıştığında oluşur. İki tür gizleme özelliği aşağıdaki bölümlerde açıklanmıştır.

#### <a name="hiding-through-nesting"></a>İç içe geçme yoluyla gizleme

İç içe geçme yoluyla ad alanları veya türler, sınıfların veya yapıların içinde iç içe geçme, parametre ve yerel değişken bildirimlerinin sonucu olarak, ad alanları içinde iç içe geçme işleminin sonucu olarak ortaya çıkabilir.

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
`F` yönteminde, `i` örnek değişkeni `i` yerel değişkeni tarafından gizlenir, ancak `G` yönteminde `i`, hala örnek değişkenine başvurur.

Bir iç kapsamdaki bir ad, dış kapsamdaki bir adı gizliyor, bu ada ait tüm aşırı yüklenmiş oluşumları gizler. Örnekte
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
`F(1)` çağrısı, `F` ' ün tüm dış oluşumları iç bildirim tarafından gizlendiğinden `Inner` ' de belirtilen `F` ' i çağırır. Aynı nedenden dolayı, `F("Hello")` çağrısı derleme zamanı hatası ile sonuçlanır.

#### <a name="hiding-through-inheritance"></a>Devralma yoluyla gizleme

Devralma yoluyla ad gizleme, sınıflar veya yapılar temel sınıflardan devralınan adları yeniden bildirdikleri zaman oluşur. Bu tür bir ad gizleme aşağıdaki biçimlerden birini alır:

*  Bir sınıf veya yapı içinde tanıtılan bir sabit, alan, özellik, olay veya tür, aynı ada sahip tüm temel sınıf üyelerini gizler.
*  Bir sınıf veya yapı içinde tanıtılan bir yöntem, aynı ada sahip tüm yöntem olmayan taban sınıf üyelerini ve aynı imzaya sahip tüm temel sınıf yöntemlerini (Yöntem adı ve parametre sayısı, değiştiriciler ve türler) gizler.
*  Bir sınıf veya yapı içinde tanıtılan bir Dizin Oluşturucu, aynı imzaya sahip tüm temel sınıf dizin oluşturucularının (parametre sayısı ve türleri) tümünü gizler.

İşleç bildirimlerini ([işleçler](classes.md#operators)) yöneten kurallar, türetilmiş bir sınıfın bir taban sınıftaki işleçle aynı imzaya sahip bir işleç tanımlamasına olanak sağlar. Bu nedenle, işleçler hiçbir şekilde bir tane gizlemez.

Bir dış kapsamdan bir adı gizlemenin aksine, devralınan bir kapsamdaki erişilebilir bir adı gizlemek bir uyarının raporlanmasına neden olur. Örnekte
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
`Derived` ' deki `F` bildirimi, bir uyarının bildirilmesine neden olur. Devralınan bir adın gizlenmesi, temel sınıfların ayrı bir evrimden daha fazla gelişmesinden dolayı özellikle bir hata değildir. Örneğin, yukarıdaki durum yaklaşık bir `Base` sürümü, sınıfının önceki bir sürümünde mevcut olmayan bir `F` yöntemi kullanıma sunduğundan daha fazla gelmiş olabilir. Yukarıdaki durum bir hata olduğundan, ayrı sürümlü Sınıf kitaplığındaki temel sınıfta yapılan herhangi bir değişiklik, türetilmiş sınıfların geçersiz olmasına neden olabilir.

Devralınan bir adı gizlemenin neden olduğu uyarı `new` değiştiricisi kullanılarak ortadan kaldırılabilir:
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

@No__t-0 değiştiricisi, `Derived` ' deki `F` ' in "yeni" olduğunu ve gerçekten devralınmış üyeyi gizleyebileceğini belirtir.

Yeni bir üyenin bildirimi, devralınan bir üyeyi yalnızca yeni üyenin kapsamı içinde gizler.

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

Yukarıdaki örnekte, `Derived` ' deki `F` ' ın bildirimi `Base` ' ten devralınmış `F` ' yi gizliyor, ancak `Derived` ' teki yeni `F` ' ün özel erişimi olduğundan, kapsamı `MoreDerived` ' a genişlemez. Bu nedenle, `MoreDerived.G` ' deki `F()` çağrısı geçerli olur ve `Base.F` ' i çağırır.

## <a name="namespace-and-type-names"></a>Ad alanı ve tür adları

Bir C# programdaki birkaç bağlam, *namespace_name* veya *type_name* gerektirir.

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

Bir *namespace_name* , bir ad alanına başvuran bir *namespace_or_type_name* . Aşağıda açıklandığı gibi, bir *namespace_name* öğesinin *namespace_or_type_name* bir ad alanına başvurması gerekir, aksi takdirde derleme zamanı hatası oluşur. Bir *namespace_name* içinde hiçbir tür bağımsız değişkeni ([tür bağımsız değişkeni](types.md#type-arguments)) mevcut olamaz (yalnızca türler tür bağımsız değişkenlerine sahip olabilir).

Bir *type_name* , bir türe başvuran bir *namespace_or_type_name* . Aşağıda açıklandığı gibi, bir *type_name* öğesinin *namespace_or_type_name* bir türe başvurması gerekir, aksi takdirde derleme zamanı hatası oluşur.

*Namespace_or_type_name* nitelikli bir diğer ad üyesiyse, anlamı [ad alanı diğer ad niteleyicileri](namespaces.md#namespace-alias-qualifiers)bölümünde açıklanacaktır. Aksi halde, bir *namespace_or_type_name* dört formdan birine sahiptir:

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

`I` tek bir tanımlayıcıdır, `N` bir *namespace_or_type_name* ve `<A1, ..., Ak>` isteğe bağlı bir *type_argument_list*. *Type_argument_list* belirtilmediğinde `k` ' i sıfır olacak şekilde düşünün.

Bir *namespace_or_type_name* anlamı aşağıdaki şekilde belirlenir:

*   *Namespace_or_type_name* , `I` veya `I<A1, ..., Ak>` biçiminde ise:
    * @No__t-0 sıfırsa ve *namespace_or_type_name* bir genel yöntem bildiriminde ([metotlar](classes.md#methods)) görünüyorsa ve bu bildirimde @ no__t-4 adı ile bir tür parametresi ([tür parametreleri](classes.md#type-parameters)) varsa, *namespace_or_type_ ad* bu tür parametresine başvuruyor.
    * Aksi takdirde, *namespace_or_type_name* bir tür bildiriminde görüntüleniyorsa, her bir örnek türü için, bu tür bildiriminin örnek türüyle başlayıp ve her birinin örnek türüyle devam eden her bir örnek türü için @ no__t-1 ([örnek türü](classes.md#the-instance-type)). kapsayan sınıf veya yapı bildirimi (varsa):
        * @No__t-0 ise ve `T` bildirimi, @ no__t-2 adlı bir tür parametresi içeriyorsa, *namespace_or_type_name* bu tür parametresine başvurur.
        * Aksi takdirde, *namespace_or_type_name* tür bildiriminin gövdesinde görüntüleniyorsa ve `T` veya temel türlerinden herhangi biri, @ no__t-2 ve `K` @ no__t-4type Parameters adına sahip iç içe erişilebilen bir tür içeriyorsa *namespace_or_type _Adı* , verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur. Böyle birden fazla tür varsa, daha fazla türetilmiş tür içinde belirtilen tür seçilidir. Tür olmayan üyelerin (sabitler, alanlar, Yöntemler, özellikler, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve farklı sayıda tür parametrelerine sahip tür üyelerinin yok sayıldığını unutmayın. *namespace_or_type_name*.
    * Önceki adımlar başarısız olduysa, her bir @ no__t-0 ad alanı için *namespace_or_type_name* gerçekleştiği ad alanından başlayarak, kapsayan her ad alanıyla (varsa) devam eder ve genel ad alanıyla sona eriyor, aşağıdaki bir varlık bulunana kadar adımlar değerlendirilir:
        * @No__t-0 sıfırsa ve `I`, @ no__t-2 içindeki bir ad alanının adı ise:
            * *Namespace_or_type_name* 'in gerçekleştiği konum `N` için bir ad alanı bildirimiyle çevrelenmiş ve ad alanı bildirimi, @ No adını ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa __T-4 bir ad alanı veya tür ile, *namespace_or_type_name* belirsizdir ve derleme zamanı hatası oluşur.
            * Aksi takdirde *namespace_or_type_name* , `I` adlı ad alanını `N` ' de belirtir.
        * Aksi takdirde, `N` adı @ no__t-1 ve `K` @ no__t-3type parametrelerine sahip erişilebilir bir tür içeriyorsa:
            * @No__t-0 sıfırsa ve *namespace_or_type_name* 'in gerçekleştiği konum `N` için bir ad alanı bildirimi ile çevrelenmiş ve ad alanı bildirimi bir *extern_alias_directive* ya da *using_alias_directive* içeriyorsa @ no__t-5 adını bir ad alanı veya türle ilişkilendirir, sonra *namespace_or_type_name* belirsizdir ve bir derleme zamanı hatası oluşur.
            * Aksi takdirde, *namespace_or_type_name* verilen tür bağımsız değişkenleriyle oluşturulan türe başvurur.
        * Aksi takdirde, *namespace_or_type_name* 'in gerçekleştiği konum `N` için bir ad alanı bildirimi içine alınır:
            * @No__t-0 sıfırsa ve ad alanı bildirimi, @ no__t-3 adını içeri aktarılan bir ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *namespace_or_type_name* bu ad alanı veya türü.
            * Aksi halde, ad alanı bildiriminin *using_namespace_directive*s ve *using_alias_directive*s tarafından içeri aktarılan ad alanları ve tür bildirimleri, @ no__t-2 ve `K` @ no__t-4type adında tam olarak bir erişilebilir tür içeriyorsa Parametreler, ardından *namespace_or_type_name* verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur.
            * Aksi halde, ad alanı bildiriminin *using_namespace_directive*s ve *using_alias_directive*s tarafından içeri aktarılan ad alanları ve tür bildirimleri @ no__t-2 ve `K` @ no__t-4type adlı birden fazla erişilebilir tür içeriyorsa Parametreler, sonra *namespace_or_type_name* belirsizdir ve bir hata oluşur.
    * Aksi takdirde, *namespace_or_type_name* tanımsızdır ve bir derleme zamanı hatası oluşur.
*  Aksi takdirde, *namespace_or_type_name* `N.I` veya `N.I<A1, ..., Ak>` biçiminde olur. `N` ilk önce bir *namespace_or_type_name*olarak çözümlenir. @No__t-0 çözümlemesi başarılı olmazsa, bir derleme zamanı hatası oluşur. Aksi takdirde, `N.I` veya `N.I<A1, ..., Ak>` aşağıdaki gibi çözümlenir:
    * @No__t-0 ise ve `N` bir ad alanına başvuruyorsa ve `N` `I` adında iç içe bir ad alanı içeriyorsa, *namespace_or_type_name* bu iç içe ad alanına başvurur.
    * Aksi takdirde, `N` bir ad alanına başvuruyorsa ve `N` adı @ no__t-2 ve `K` @ no__t-4type parametrelerine sahip erişilebilir bir tür içeriyorsa, *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur.
    * Aksi takdirde, `N` ' a (büyük olasılıkla oluşturulmuş) bir sınıf veya yapı türü veya `N` ya da temel sınıflarından birinin adı @ no__t-2 ve `K` @ no__t-4tür parametreleri olan iç içe erişilebilen bir tür içeriyorsa, *namespace_or_type_name* başvurur Bu tür, verilen tür bağımsız değişkenleriyle oluşturulur. Böyle birden fazla tür varsa, daha fazla türetilmiş tür içinde belirtilen tür seçilidir. @No__t-0 ' ın temel sınıf @no__t belirtimini çözmenin parçası olarak belirleniyorsa, `N` ' nin doğrudan taban sınıfının Object ([temel sınıflar](classes.md#base-classes)) olarak kabul edileceğini unutmayın.
    * Aksi takdirde, `N.I` geçersiz bir *namespace_or_type_name*ve derleme zamanı hatası oluşur.

Bir *namespace_or_type_name* , yalnızca şu durumlarda bir statik sınıfa ([statik sınıflar](classes.md#static-classes)) başvuruda bulunmasına izin verilir

*  *Namespace_or_type_name* , `T` `T.I` biçiminde bir *namespace_or_type_name* veya
*  *Namespace_or_type_name* , `T` ' in ([bağımsız değişken listeleri](expressions.md#argument-lists)1), `typeof(T)` biçiminde bir *typeof_expression* .

### <a name="fully-qualified-names"></a>Tam nitelikli adlar

Her ad alanı ve türün, tüm diğerleri arasından ad alanını veya türü benzersiz bir şekilde tanımlayan ***tam bir adı***vardır. Bir ad alanının tam adı veya `N` türü aşağıdaki şekilde belirlenir:

*  @No__t-0 genel ad alanının üyesiyse, tam adı `N` ' dir.
*  Aksi halde, tam adı `S.N` ' dır; burada `S`, `N` ' nin bildirildiği ad alanının veya türün tam adıdır.

Diğer bir deyişle `N` tam adı, genel ad alanından başlayarak `N` ' e yol açabilecek tanımlayıcıların tam hiyerarşik yoludur. Bir ad alanının veya türün her üyesinin benzersiz bir adı olması gerektiğinden, bir ad alanının veya türün tam nitelikli adının her zaman benzersiz olması gerekir.

Aşağıdaki örnekte, ilişkili tam adlarıyla birlikte çeşitli ad alanı ve tür bildirimleri gösterilmektedir.
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

C#geliştiricilerin nesnelere göre kapladığı belleği el ile ayırmayı ve boşaltmasını sağlayan otomatik bellek yönetimini kullanır. Otomatik bellek yönetimi ilkeleri bir ***çöp toplayıcısı***tarafından uygulanır. Bir nesnenin bellek yönetimi yaşam döngüsü aşağıdaki gibidir:

1. Nesne oluşturulduğunda, için bellek ayrılır, Oluşturucu çalıştırılır ve nesne canlı olarak değerlendirilir.
2. Nesneye veya herhangi bir kısmına, yok edicilerin çalışması dışında, yürütmenin olası devamlılığı tarafından erişilemez, nesne artık kullanımda değildir ve yok etme için uygun hale gelir. C# Derleyici ve çöp toplayıcı, bir nesneye yapılan başvuruların gelecekte kullanılabileceğini belirlemek için kodu analiz etmeyi tercih edebilir. Örneğin, kapsamdaki bir yerel değişken bir nesneye yönelik tek başvuru ise, ancak bu yerel değişken, yordamda geçerli yürütme noktasından yürütmenin olası devamlılığı hiçbir şekilde hiçbir şekilde başvurulmadıysa, çöp toplayıcı olabilir (ancak için gereklidir) nesneyi artık kullanımda değil olarak değerlendirin.
3. Nesne yok etme için uygun olduğunda, bazı belirtilmemişse nesne için yıkıcı (yok[ediciler](classes.md#destructors)) (varsa) çalıştırılır. Normal koşullarda nesnenin yıkıcısı yalnızca bir kez çalıştırılır, ancak uygulamaya özgü API 'Ler bu davranışın geçersiz kılınmasına izin verebilir.
4. Bir nesnenin yıkıcısı çalıştırıldığında, bu nesneye veya bunun herhangi bir kısmına, yok edicinin çalıştırılması dahil olmak üzere herhangi bir olası yürütme devamlılığı tarafından erişilemez, nesnenin erişilemez olduğu kabul edilir ve nesne koleksiyona uygun hale gelir.
5. Son olarak, nesne koleksiyon için uygun olduktan sonra çöp toplayıcı bu nesneyle ilişkili belleği serbest bırakır.

Çöp toplayıcı, nesne kullanımı hakkındaki bilgileri tutar ve bu bilgileri kullanarak yeni oluşturulan bir nesneyi bulma, bir nesnenin konumunu değiştirme ve bir nesnenin artık ne zaman ne zaman kullanıldığı veya erişilemeyen gibi bellek yönetimi kararları almak için kullanılır.

Çöp toplayıcısının varlığını varsaymış diğer diller gibi, C# çöp toplayıcı çok çeşitli bellek yönetimi ilkeleri uygulayabilmesi için tasarlanmıştır. Örneğin, C# yok edicilerin çalıştırılmasını gerektirmez veya uygun olan veya yok edicilerin belirli bir sırada veya belirli bir iş parçacığında çalıştırılması için nesnelerin toplanıp toplanmasına gerek yoktur.

Çöp toplayıcısının davranışı, `System.GC` sınıfındaki statik yöntemler aracılığıyla bir dereceye kadar denetlenebilir. Bu sınıf, bir koleksiyon oluşmasına, çalıştırılacak yok edicilere (veya çalıştırılmayan), vb. istemek için kullanılabilir.

Çöp toplayıcısının nesnelerin ne zaman toplanmaya karar verirken ve yıkıcıları çalıştırmanın yanı sıra, uygun bir uygulama aşağıdaki kodla gösterilenden farklı bir çıktı üretebilir. Program
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
`A` sınıfının bir örneğini ve `B` sınıfının bir örneğini oluşturur. Bu nesneler, `b` değişkenine `null` değeri atandığında, bu süreden sonra herhangi bir kullanıcı tarafından yazılan kodun bunlara erişebilmesi olanaksız olduğundan çöp toplama için uygun hale gelir. Çıktı şunlardan biri olabilir

```console
Destruct instance of A
Destruct instance of B
```
or
```console
Destruct instance of B
Destruct instance of A
```
dil, nesnelerin atık olarak toplandığı sıraya göre hiçbir kısıtlama yapmadığından.

Hafif durumlarda, "yok etme için uygun" ve "koleksiyon için uygun" arasındaki ayrım önemli olabilir. Örneğin,
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

Yukarıdaki programda, çöp toplayıcı `B` ' i yok edicinin önüne `A` yok ediciyi çalıştırmayı seçerse, bu programın çıktısı şu olabilir:
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

@No__t-0 örneği kullanımda olmasa ve `A` ' in yıkıcısı çalıştırılsa da, başka bir yıkıcıdan `A` (Bu örnekte, `F`) yöntemleri için yine de mümkündür. Ayrıca, bir yıkıcı çalıştırmanın bir nesnenin ana hat programından yeniden kullanılabilir hale gelmesine neden olabileceğini unutmayın. Bu durumda, `B` ' ın yıkıcının çalıştırılması, daha önce kullanımda olan `A` ' in `Test.RefA` canlı başvurusuyla erişilebilir hale gelmesine neden oldu. @No__t-0 ' a çağrıdan sonra, `B` ' in örneği koleksiyona uygundur, ancak `Test.RefA` başvurusu nedeniyle `A` örneği değildir.

Karışıklıkları ve beklenmedik davranışları önlemek için, yok edicilerin yalnızca kendi nesnelerinde depolanan verilerde temizlik yapması ve başvurulan nesneler veya statik alanlar üzerinde herhangi bir eylem gerçekleştirmemek için iyi bir fikir vardır.

Yıkıcıları kullanmanın bir alternatifi, bir sınıfın `System.IDisposable` arabirimini uygulamasına olanak tanır. Bu, nesne istemcisinin, genellikle nesnesine bir `using` ifadesinde ([using ifadesiyle](statements.md#the-using-statement)) bir kaynak olarak erişerek nesne kaynaklarının ne zaman yayınlanarak belirlenmesini sağlar.

## <a name="execution-order"></a>Yürütme sırası

Bir C# programın yürütülmesi, yürütülen her iş parçacığının yan etkileri kritik yürütme noktalarında korunacağından devam eder. Bir ***yan etkisi*** , geçici bir alan okuma veya yazma, geçici olmayan bir değişkene yazma, bir dış kaynağa yazma ve özel durum atma olarak tanımlanır. Bu yan etkileri sırasının korunması gereken kritik yürütme noktaları, geçici alanlar ([geçici alanlar](classes.md#volatile-fields)), `lock` deyimlerinin ([kilit deyimi](statements.md#the-lock-statement)) ve iş parçacığı oluşturma ve sonlandırma için referanslardır. Yürütme ortamı, bir C# programın yürütme sırasını değiştirmek için ücretsizdir ve aşağıdaki kısıtlamalara tabidir:

*  Veri bağımlılığı, yürütme iş parçacığı içinde korunur. Diğer bir deyişle, her değişkenin değeri iş parçacığındaki tüm deyimler orijinal program sırasıyla yürütülürse olarak hesaplanır.
*  Başlatma sırası kuralları korunur ([alan başlatma](classes.md#field-initialization) ve [değişken başlatıcıları](classes.md#variable-initializers)).
*  Yan etkileri sıralaması, geçici okuma ve yazma işlemleri ([geçici alanlar](classes.md#volatile-fields)) açısından korunur. Ayrıca, bu ifadenin değerinin kullanılmadığını ve gerekli yan etkileri üretilmez (bir yöntemi çağırarak veya geçici bir alana erişim nedeniyle oluşan herhangi biri dahil), yürütme ortamı bir ifadenin bir kısmını değerlendirmemelidir. Program yürütme zaman uyumsuz bir olay (örneğin, başka bir iş parçacığı tarafından oluşturulan bir özel durum) tarafından kesintiye uğradığında, observable yan etkilerinin orijinal program sırasıyla görünür olması garanti edilmez.
