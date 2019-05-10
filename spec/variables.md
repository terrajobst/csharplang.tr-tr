---
ms.openlocfilehash: b7bb7dd575d9e2e6d5dd85bdd3e535411e29fcf4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "64488860"
---
# <a name="variables"></a>Değişkenler

Değişkenleri depolama konumlarını temsil eder. Her değişken değerlerin neler olması belirleyen bir türe sahip bir değişkende depolanır. C# bir tür kullanımı uyumlu bir dildir ve C# derleyicisi değişkenlerinde depolanan değerler her zaman uygun türü olduğunu garanti eder. Bir değişkenin değerini kullanımını veya atama üzerinden değiştirilebilir `++` ve `--` işleçleri.

Bir değişken olmalıdır ***kesinlikle atanan*** ([belirli atama onayına](variables.md#definite-assignment)) önce değeri elde edilebilir.

Aşağıdaki bölümlerde açıklandığı gibi ya da değişkenlerdir ***başlangıçta atanan*** veya ***başlangıçta atanmamış***. Başlangıçta atanan bir değişkeni, iyi tanımlanmış bir başlangıç değeri vardır ve kesinlikle atanan her zaman değerlendirilir. Başlangıçta atanmayan bir değişkeni, ilk değer yok. Belirli bir konumda kesinlikle atanmış olarak değerlendirilmesi başlangıçta atanmamış değişken için bu konuma önde gelen tüm olası yürütme yolu değişkeni atama bulunması gerekir.

## <a name="variable-categories"></a>Değişken kategorileri

C# tanımlar yedi kategori değişkenlerin: statik değişkenler, örnek değişkenleri, dizi öğeleri, değer parametreleri, başvuru parametreleri, çıktı parametreleri ve yerel değişkenler. Aşağıdaki bölümlerde bu kategorilerden her biri açıklanmaktadır.

Örnekte
```csharp
class A
{
    public static int x;
    int y;

    void F(int[] v, int a, ref int b, out int c) {
        int i = 1;
        c = a + b++;
    }
}
```
`x` bir statik değişken `y` bir örnek değişkeni `v[0]` bir dizi öğe `a` bir değer parametresi `b` bir başvuru parametresi `c` bir çıktı parametresidir ve `i` yerel bir değişkendir.

### <a name="static-variables"></a>Statik değişkenler

Bir alana bildirilen `static` değiştiricisi çağrıldığında bir ***statik değişken***. Bir statik değişken varlığı statik oluşturucunun yürütmeden önce salesforce'taki ([statik oluşturucular](classes.md#static-constructors)) kapsayan türü ve ilişkili uygulama etki alanı olmaktan çıkar mevcut olduğunda mevcut olmaktan çıkar.

Statik bir değişkenin başlangıç değerini varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) değişken türü.

Belirli atama onayına denetleme amaçları başlangıçta atanan bir statik değişken olarak kabul edilir.

### <a name="instance-variables"></a>Örnek değişkenleri

Bir alan olmadan bildirilen `static` değiştiricisi çağrıldığında bir ***örnek değişkeni***.

#### <a name="instance-variables-in-classes"></a>Sınıflarda örnek değişkenleri

Bu sınıfın yeni bir örneği oluşturulduğunda ve söz konusu örneğine başvuru vardır ve örneğin yıkıcısı (varsa) yürütüldüğünde sonlanacaktır mevcut olmaktan çıkar bir örnek değişkeni bir sınıfın varlığı gelir.

Bir sınıfın bir örnek değişkeni ilk değerini varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) değişken türü.

Belirli atama onayına denetlemek amacıyla, başlangıçta atanan bir sınıfın bir örnek değişkeni kabul edilir.

#### <a name="instance-variables-in-structs"></a>Yapılar örneği değişkenleri

Bir örnek değişkeni bir yapının ait olduğu yapı değişkeni olarak tam olarak aynı ömrü vardır. Diğer bir deyişle, ne zaman bir yapı türünün değişkenini varlığı gelir veya var, bunu olmaktan çıkar çok struct örneği değişkenleri yapın.

Bir yapının bir örnek değişkeni ilk atama durumu, içeren yapı değişkeni aynıdır. Diğer bir deyişle, bir yapı değişkeni başlangıçta ne zaman kabul edilir atanan, bunu çok örneği değişkenlerini ve bir yapı değişkeni başlangıçta atanmamış olarak kabul edilir, örnek değişkenlerini benzer şekilde atanmamış.

### <a name="array-elements"></a>Dizi öğeleri

Bir dizinin öğeleri varlığı bir dizi örneği oluşturulduğunda gelen ve başvuru, dizi örneği olduğunda mevcut sona.

Bir dizideki öğelerin her biri olan başlangıç değeri varsayılan değerdir ([varsayılan değerler](variables.md#default-values)) dizi öğelerinin türü.

Belirli atama onayına denetlemek amacıyla, başlangıçta atanan bir dizi öğesine kabul edilir.

### <a name="value-parameters"></a>Değer parametreleri

Bir parametre olmadan bildirilen bir `ref` veya `out` değiştiricisi bir ***değer parametresi***.

Varlığı işlevi üyesi (yöntem, örnek oluşturucusu, erişimci veya işleci) veya anonim işlev çağrısı sırasında bir değer parametresini salesforce'taki, parametre ait ve verilen bir çağrıda bağımsız değişkenin değeri ile başlatılır. Bir değer parametresini işlevi üye veya anonim bir işlevin dönüş sırasında mevcut normalde olmaktan çıkar. Ancak, değer parametresi bir anonim bir işlev tarafından yakalanır ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)), kendi yaşam süresi en az bir temsilci kadar genişletir ya da ifade ağacı anonim bu işlevden oluşturulan için uygun Çöp toplama.

Belirli atama onayına denetlemek amacıyla, başlangıçta atanan bir değer parametresini kabul edilir.

### <a name="reference-parameters"></a>Başvuru parametreleri

Bir parametre ile bildirilen bir `ref` değiştiricisi bir ***parametre başvurusunu***.

Bir başvuru parametresi, yeni bir depolama konumuna oluşturmaz. Bunun yerine, aynı depolama konumu işlev üyesi veya anonim işlev çağırma bağımsız değişken olarak verilen değişkeni olarak bir başvuru parametresi temsil eder. Bu nedenle, bir başvuru parametresi değeri her zaman temel alınan değişkeni aynıdır.

Aşağıdaki belirli atama onayına kurallar başvuru parametreleri geçerlidir. Çıktı parametreleri bölümünde açıklanan için farklı kuralları unutmayın [çıkış parametresi](variables.md#output-parameters).

*  Bir değişken kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) önce bir işlev çağırma üyesi veya temsilci bir başvuru parametresi olarak geçirilebilir.
*  Bir işlev üyesi veya anonim işlev içinde bir başvuru parametresi başlangıçta atanan olarak kabul edilir.

Bir örnek yöntemi veya bir yapı türünün örneği erişimci `this` anahtar sözcüğü, tam olarak bir başvuru parametresi yapı türü olarak davranır ([bu erişim](expressions.md#this-access)).

### <a name="output-parameters"></a>Çıktı parametreleri

Bir parametre ile bildirilen bir `out` değiştiricisi bir ***çıkış parametresi***.

Çıkış parametresi, yeni bir depolama konumuna oluşturmaz. Bunun yerine, aynı depolama konumu işlev üyesi veya temsilci çağırma bağımsız değişken olarak verilen değişkeni olarak bir çıkış parametresi temsil eder. Bu nedenle, bir çıkış parametresinin değeri her zaman temel alınan değişkeni aynıdır.

Çıktı parametreleri aşağıdaki belirli atama onayına kurallar geçerli olur. Açıklanan başvuru parametreleri için farklı kuralları Not [başvuru parametreleri](variables.md#reference-parameters).

*  Üye işlevi bir çıktı parametresi olarak geçirilebilir veya çağırma temsilci önce bir değişkeni kesinlikle atanmamış.
*  Normal tamamlama üyesi veya temsilci bir işlev çağrısının bir output parametresi olarak kabul edilir olarak geçirilen her bir değişken yürütme yolda atanır.
*  Bir işlev üyesi veya anonim işlev içinde bir output parametresi başlangıçta atanmamış olarak kabul edilir.
*  Her çıkış parametresi bir işlev üyesi ya da anonim işlev kesinlikle atanmalıdır ([belirli atama onayına](variables.md#definite-assignment)) üyesi veya anonim işlev önce işlev normal olarak döndürür.

Bir yapı türünün bir örneği oluşturucu içinde `this` anahtar sözcüğü, tam olarak bir çıkış parametresi yapı türü olarak davranır ([bu erişim](expressions.md#this-access)).

### <a name="local-variables"></a>Yerel değişkenler

A ***yerel değişken*** tarafından bildirilen bir *local_variable_declaration*, içinde meydana gelebilir bir *blok*, *for_statement*, bir *switch_statement* veya *using_statement*; ya da bir *foreach_statement* veya *specific_catch_clause* bir için*try_statement*.

Yerel bir değişken ömrü, program yürütme sırasında depolama için ayrılmış olması garanti bölümüdür. Bu yaşam süresi en az girişinde genişletir *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, veya *specific_catch_clause* , yürütmeyi o kadar ilişkilendirildiği ile *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, veya *specific_catch_clause* herhangi bir şekilde sona erer. (Bir kapalı girme *blok* veya yöntemi çağırma askıya alır, ancak sonlanmıyor, geçerli yürütme *blok*, *for_statement*, *switch_statement* , *using_statement*, *foreach_statement*, veya *specific_catch_clause*.) Yerel değişken bir anonim bir işlev tarafından yakalanır ([dış değişkenlere yakalanan](expressions.md#captured-outer-variables)), yaşam süresi en az gelir herhangi bir nesne ile birlikte anonim işlev oluşturulan temsilci veya ifade ağacı kadar genişletir Yakalanan bir değişken başvurusu, atık toplama için uygundur.

Üst *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*, veya *specific_catch_clause* girilen yinelemeli olarak, yerel değişken yeni bir örneğini her zaman oluşturulur ve kendi *local_variable_initializer*, varsa, değerlendirme her zaman.

Tarafından sunulan bir yerel değişken bir *local_variable_declaration* otomatik olarak başlatılmadı ve bu nedenle varsayılan değeri yok. Belirli atama onayına denetlemek amacıyla, yerel bir değişken sunulan tarafından bir *local_variable_declaration* başlangıçta atanmamış olarak kabul edilir. A *local_variable_declaration* içerebilir bir *local_variable_initializer*, bu durumda değişkeni yalnızca başlatılırken ifadeden sonra kesinlikle atanmış olarak kabul edilir ([ Bildirim deyimleri](variables.md#declaration-statements)).

Tarafından sunulan yerel bir değişken kapsamında bir *local_variable_declaration*, önündeki değerinin metinsel bir konumda yerel söz konusu değişkene başvurmak için bir derleme zamanı hata kendi *local_variable_declarator*. Yerel değişken bildiriminde örtük ise ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)), ayrıca değişkenin içinde başvurmak için bir hata olduğundan, *local_variable_declarator*.

Tarafından sunulan bir yerel değişken bir *foreach_statement* veya *specific_catch_clause* tüm kapsamında kesinlikle atanmış olarak kabul edilir.

Bir yerel değişkenin gerçek yaşam uygulaması bağlıdır. Örneğin, bir derleyici statik bir yerel değişken bir blok içinde yalnızca o blok küçük bir kısmı için kullanılan saptayabilir. Bu analizi kullanarak, derleyici değişkenin depolama içeren bloğun'dan daha kısa bir ömre sahip sonuçlanır kod üretebilir.

Yerel başvuru değişkeni tarafından başvurulan depolama konusu yerel başvuru değişkenin kullanım ömrünün bağımsız olarak geri kazanılır ([otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management)).

## <a name="default-values"></a>Varsayılan değerler

Değişkenleri aşağıdaki kategorileri otomatik olarak varsayılan değerlerine başlatılır:

*  Statik değişkenler.
*  Örnek değişkenleri sınıf örneği.
*  Dizi öğeleri.

Bir değişkenin varsayılan değeri, değişkenin türüne bağlıdır ve aşağıdaki şekilde belirlenir:

*  Bir değişken için bir *value_type*, varsayılan değer tarafından hesaplanan değer aynıdır *value_type*ait varsayılan oluşturucu ([varsayılan oluşturucular](types.md#default-constructors)).
*  Bir değişken için bir *reference_type*, varsayılan değer `null`.

Başlatma için varsayılan değerler, genellikle bellek yöneticisi sağlayarak gerçekleştirilir veya çöp toplayıcı kullanılmak için ayrılan önce tüm BITS sıfıra bellek başlatılamadı. Bu nedenle, null başvuru temsil etmek için tüm bitler sıfır kullanmak uygundur.

## <a name="definite-assignment"></a>Kesin atama

Belirli bir konumda işlevi üyesinin yürütülebilir kod, bir değişken olarak kabul edilir ***kesinlikle atanan*** derleyici, belirli statik akış analizi tarafından kanıtlayabilirsiniz varsa ([kesin belirlemek için kesin kurallar atama](variables.md#precise-rules-for-determining-definite-assignment)), değişken otomatik olarak başlatılmış veya en az bir atama işleminin hedefi olmuştur. Peter'in belirtildiği gibi belirli atama onayına kurallar şunlardır:

*  Başlangıçta atanan bir değişken ([değişkenleri başlangıçta atanan](variables.md#initially-assigned-variables)) her zaman kesin atanmış olarak kabul edilir.
*  Başlangıçta atanmamış bir değişken ([başlangıçta değişkenleri atanmamış](variables.md#initially-unassigned-variables)) bu konuma önde gelen tüm olası yürütme yolları aşağıdakilerden en az birini içeriyorsa, belirli bir konumda kesinlikle atanmış olarak değerlendirilir:
    * Basit atama ([basit atama](expressions.md#simple-assignment)) sol işlenen değişken olduğu.
    * Çağrı ifadesi ([çağrı ifadeleri](expressions.md#invocation-expressions)) veya nesne oluşturma ifadesi ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)), değişkeni bir output parametresi olarak geçirir.
    * Bir yerel değişken bildiriminde yerel bir değişken için ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) içeren değişken başlatıcı.

Yukarıdaki resmi olmayan kuralları temel resmi belirtimi açıklanan [değişkenleri başlangıçta atanan](variables.md#initially-assigned-variables), [başlangıçta değişkenleri atanmamış](variables.md#initially-unassigned-variables), ve [kesin kurallar belirlemek için belirli atama onayına](variables.md#precise-rules-for-determining-definite-assignment).

Örnek değişkenleri belirli atama onayına durumlarını bir *struct_type* değişkeni topluca de ayrı ayrı olarak izlenir. Yukarıdaki kurallara ek aşağıdaki kurallar geçerli *struct_type* değişkenleri ve onların örneği değişkenleri:

*  Bir örnek değişkeni kesinlikle atanan kabul edildiği kapsayıcı *struct_type* değişkeni kesinlikle atanmış olarak değerlendirilir.
*  A *struct_type* her örneği değişkenlerini kesinlikle atanan kabul ediliyorsa değişkeni kesinlikle atanan değerlendirilir.

Belirli atama onayına şu bağlamlarda bir gereksinimdir:

*  Bir değişken değerini burada elde edilen her bir konumda kesinlikle atanmalıdır. Bu, tanımsız değerler hiçbir zaman gerçekleşmesini sağlar. Aşağıdakiler haricinde değişkeninin değerini edinme oluşumu bir ifade bir değişken olarak kabul edilir
    * sol işleneni, bir basit atama değişkenidir,
    * değişkeni bir output parametresi olarak geçirilir veya
    * Bu değişken bir *struct_type* değişkeni ve üye erişimi sol işleneni gerçekleşir.
*  Burada başvuru parametre olarak geçirilen her bir konumdaki kesinlikle bir değişkene atanması gerekir. Bu, çağrılan işlev üyesi başlangıçta atanan başvuru parametresi düşünebilirsiniz sağlar.
*  Tüm çıkış parametreleri işlevi üyesinin işlevi üye döndüğü her konumda kesinlikle atanmalıdır (aracılığıyla bir `return` deyimi veya işlevi üyesinin gövdesi sona erdikten yürütme aracılığıyla). Bu işlev üyeleri tanımsız değerler çıkış parametreleri, bu nedenle bir output parametresi olarak bir değişken eşdeğer değişkenine bir atamayı alan bir işlev üye çağrısı dikkate alınması gereken derleyici etkinleştirme döndürmediğine sağlar.
*  `this` Değişkenine bir *struct_type* örnek oluşturucusu Bu örnek oluşturucusu döndüğü her konumda kesinlikle da atanmalıdır.

### <a name="initially-assigned-variables"></a>Başlangıçta atanan değişkenleri

Değişkenleri aşağıdaki kategorileri olarak başlangıçta atanan sınıflandırılır:

*  Statik değişkenler.
*  Örnek değişkenleri sınıf örneği.
*  Başlangıçta atanan yapı değişkenleri değişkenlerinin örneği.
*  Dizi öğeleri.
*  Parametre değeri.
*  Başvuru parametreleri.
*  İçinde bildirilmiş değişkenlerin bir `catch` yan tümcesi veya `foreach` deyimi.

### <a name="initially-unassigned-variables"></a>Başlangıçta atanmamış değişkenleri

Değişkenleri aşağıdaki kategorileri, ilk olarak atanmamış sınıflandırılır:

*  Başlangıçta atanmamış yapı değişkenleri değişkenlerinin örneği.
*  Çıktı parametreleri de dahil olmak üzere, `this` yapısı örneği oluşturucular değişken.
*  Bildirilen yerel değişkenler hariç bir `catch` yan tümcesi veya `foreach` deyimi.

### <a name="precise-rules-for-determining-definite-assignment"></a>Belirli atama onayına belirlemek için kesin kurallar

Kullanılan her bir değişken kesinlikle atandığını belirlemek için derleyici Bu bölümde açıklanan eşdeğer olan bir işlemi kullanmanız gerekir.

Derleyici, bir veya daha fazla başlangıçta atanmamış değişkenleri her işlevi üyesinin gövdesi işler. Başlangıçta atanmamış her değişken için *v*, derleyici belirleyen bir ***kesin atama durumu*** için *v* her işlev üyesi aşağıdaki noktaları:

*  Her deyimin başında
*  Bir uç noktada ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)) her deyimin
*  Her yay üzerinde hangi denetimi başka bir deyimi veya deyim uç noktasına aktarır
*  Her bir ifadenin başında
*  Her bir ifadenin sonunda

Belirli atama onayına durumunu *v* şöyle olabilir:

*  Kesinlikle atanır. Bu noktaya tüm olası denetim akış üzerinde gösterir *v* değeri atandı.
*  Kesinlikle atanmamış. Bir değişken türünde bir ifadenin sonunda durumu için `bool`, Mayıs kesin olarak atanmamış (ancak zorunlu değildir) bir değişken durumu aşağıdaki alt durumlarından birine ayrılır:
    * Kesinlikle true ifadeden sonra atanır. Bu durumu bildiren *v* Boole ifade true değerlendirilir, ancak Boole ifade false değerlendirildiğinde mutlaka atanmamış kesinlikle atanır.
    * Yanlış ifade sonra kesinlikle atanır. Bu durumu bildiren *v* Boole ifade false değerlendirildi, ancak Boole ifade true değerlendirildiğinde mutlaka atanmamış kesinlikle atanır.

Aşağıdaki kurallar yöneten nasıl bir değişkenin durum *v* her konumda belirlenir.

#### <a name="general-rules-for-statements"></a>İfadeler için genel kurallar

*  *v* kesinlikle işlevi üyesinin gövdesi başında atanmadı.
*  *v* ulaşılamaz herhangi bir deyimle başında kesinlikle atanır.
*  Belirli atama onayına durumunu *v* herhangi bir deyimle başında belirli atama onayına durumunu denetleyerek belirlenir *v* , başına hedefleyen tüm denetim akışı aktarımları hakkında deyimi. Varsa (ve yalnızca) *v* kesinlikle böyle tüm denetim akışı aktarımları sonra atanan *v* deyimin başında kesinlikle atanır. Olası denetim akış aktarımları kümesini deyimi ulaşılabilirlik denetimi olduğu gibi aynı şekilde belirlenir ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)).
*  Belirli atama onayına durumunu *v* bloğunun, uç noktada `checked`, `unchecked`, `if`, `while`, `do`, `for`, `foreach`, `lock`, `using`, veya `switch` deyimi belirli atama onayına durumunu denetleyerek belirlenir *v* Sadeliği bitiş noktasını hedefleyen tüm denetim akışı aktarımları üzerinde. Varsa *v* kesinlikle böyle tüm denetim akışı aktarımları sonra atanan *v* deyim bitiş noktasında kesinlikle atanır. Aksi durumda; *v* kesinlikle deyim bitiş noktasında atanmadı. Olası denetim akış aktarımları kümesini deyimi ulaşılabilirlik denetimi olduğu gibi aynı şekilde belirlenir ([uç noktaları ve ulaşılabilirliği](statements.md#end-points-and-reachability)).

#### <a name="block-statements-checked-and-unchecked-statements"></a>Blok deyimleri, işaretli ve işaretsiz deyimleri

Belirli atama onayına durumunu *v* denetimde deyim listesinin bloğundaki ilk deyimi (veya deyim listesinin boş olup olmadığını bloğunun uç noktası) aktarımı kesinatamadeyimininaynıdır*v* blokta `checked`, veya `unchecked` deyimi.

#### <a name="expression-statements"></a>İfade deyimleri

Bir ifade deyimi için *stmt* ifade oluşur *expr*:

*  *v* başındaki aynı belirli atama onayına durumuna sahip *expr* olarak başındaki *stmt*.
*  Varsa *v* kesinlikle sonunda atanmışsa *expr*, kesinlikle bitiş noktasında atanan *stmt*; Aksi takdirde; Bitişnoktasındakesinlikleatanmadı*stmt*.

#### <a name="declaration-statements"></a>Bildirim deyimleri

*  Varsa *stmt* sonra bildirim deyimi başlatıcılar olmadan *v* uç noktada aynı belirli atama onayına durumuna sahip *stmt* olarak başındaki*stmt*.
*  Varsa *stmt* belirli atama onayına durumudur başlatıcıları, bir bildirim deyiminin ardından *v* belirlenir gibi *stmt* olan bir deyim listesiyle bir atama Her bir başlatıcı (bildirim) sırasına göre bildirimiyle bildirimi.

#### <a name="if-statements"></a>Varsa deyimleri

İçin bir `if` deyimi *stmt* formun:
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* başındaki aynı belirli atama onayına durumuna sahip *expr* olarak başındaki *stmt*.
*  Varsa *v* sonunda kesinlikle atanır *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *then_stmt* ve ya da *else_stmt*  veya uç nokta *stmt* başka hiçbir yan tümcesi ise.
*  Varsa *v* sonunda "kesinlikle true ifadeden sonra atanan" durumuna sahip *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *then_stmt*değil denetim akışı aktarımı ya da kesinlikle atanan *else_stmt* veya uç nokta *stmt* başka hiçbir yan tümcesi ise.
*  Varsa *v* sonunda "kesinlikle false ifadeden sonra atanan" durumuna sahip *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *else_stmt*değil denetim akışı aktarımı üzerinde kesinlikle atanan *then_stmt*. Kesinlikle bitiş noktasını atanan *stmt* kesinlikle bitiş noktasını atanır ve yalnızca, *then_stmt*.
*  Aksi takdirde, *v* ya da denetim akışı aktarımı kesinlikle atanan olarak kabul edilmez *then_stmt* veya *else_stmt*, veya uç nokta  *stmt* başka hiçbir yan tümcesi ise.

#### <a name="switch-statements"></a>Switch deyimleri

İçinde bir `switch` deyimi *stmt* denetleyen bir ifade ile *expr*:

*  Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* üzerinde denetim akışı erişilebilir anahtar engelleme deyimi listesine aktarımı kesin atama durumu aynıdır *v* sonunda *expr*.

#### <a name="while-statements"></a>While deyimleri

İçin bir `while` deyimi *stmt* formun:
```csharp
while ( expr ) while_body
```

*  *v* başındaki aynı belirli atama onayına durumuna sahip *expr* olarak başındaki *stmt*.
*  Varsa *v* sonunda kesinlikle atanır *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *while_body* ve bitiş noktasına  *stmt*.
*  Varsa *v* sonunda "kesinlikle true ifadeden sonra atanan" durumuna sahip *expr*, denetim akışı aktarımı üzerinde kesinlikle atandıktan sonra *while_body*, ancak değil kesinlikle bitiş noktasını atanan *stmt*.
*  Varsa *v* sonunda "kesinlikle false ifadeden sonra atanan" durumuna sahip *expr*, kesinlikle bitiş noktasını denetim akışı aktarımı üzerinde atandıktan sonra *stmt* , ancak denetim akışı aktarımı üzerinde kesinlikle atanan *while_body*.

#### <a name="do-statements"></a>Do ifadeleri

İçin bir `do` deyimi *stmt* formun:
```csharp
do do_body while ( expr ) ;
```

*  *v* başından sonuna kadar denetim akışı aktarımı aynı belirli atama onayına durumuna sahip *stmt* için *do_body* olarak başındaki *stmt*.
*  *v* başındaki aynı belirli atama onayına durumuna sahip *expr* gibi uç noktada *do_body*.
*  Varsa *v* sonunda kesinlikle atanır *expr*, kesinlikle bitiş noktasını denetim akışı aktarımı üzerinde atandıktan sonra *stmt*.
*  Varsa *v* sonunda "kesinlikle false ifadeden sonra atanan" durumuna sahip *expr*, kesinlikle bitiş noktasını denetim akışı aktarımı üzerinde atandıktan sonra *stmt* .

#### <a name="for-statements"></a>For deyimleri

Denetleme belirli atama onayına bir `for` formun deyimi:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
deyim yazılmışlar gibi gerçekleştirilir:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

Varsa *for_condition* gelen atlanırsa `for` deyimi, ardından belirli atama onayına kazançlar değerlendirmesini gibi *for_condition* ile değiştirilen `true` yukarıdaki genişletme içinde .

#### <a name="break-continue-and-goto-statements"></a>Kesme, devam etmek ve goto deyimleri

Belirli atama onayına durumunu *v* kaynaklanan denetim akışı aktarımı üzerinde bir `break`, `continue`, veya `goto` deyimi, kesin atama durumu ile aynı *v* adresindeki deyimin başlangıcı.

#### <a name="throw-statements"></a>Throw deyimleri

Bir deyim için *stmt* form
```csharp
throw expr ;
```

Belirli atama onayına durumunu *v* başındaki *expr* kesin atama durumu ile aynı *v* başındaki *stmt*.

#### <a name="return-statements"></a>Return deyimleri

Bir deyim için *stmt* form
```csharp
return expr ;
```

*  Belirli atama onayına durumunu *v* başındaki *expr* kesin atama durumu ile aynı *v* başındaki *stmt*.
*  Varsa *v* , kesinlikle ya da atanmalıdır sonra bir output parametresi olan:
    * sonra *ifade*
    * ya da sonunda `finally` bloğu bir `try` - `finally` veya `try` - `catch` - `finally` , kapsayan `return` deyimi.

Form için bir deyim stmt:
```csharp
return ;
```

*  Varsa *v* , kesinlikle ya da atanmalıdır sonra bir output parametresi olan:
    * önce *stmt*
    * ya da sonunda `finally` bloğu bir `try` - `finally` veya `try` - `catch` - `finally` , kapsayan `return` deyimi.

#### <a name="try-catch-statements"></a>Try-catch deyimleri

Bir deyim için *stmt* formun:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  Belirli atama onayına durumunu *v* başındaki *try_block* kesin atama durumu ile aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* başındaki *catch_block_i* (herhangi *miyim*) özelliğine belirli atama durumu ile aynı *v*başındaki *stmt*.
*  Belirli atama onayına durumunu *v* uç nokta adresindeki *stmt* kesinlikle atanmış bir IF (ve yalnızca) *v* kesinlikle bitiş noktasını atanır  *try_block* ve her *catch_block_i* (için her *miyim* 1'den *n*).

#### <a name="try-finally-statements"></a>Try-finally deyimleri

İçin bir `try` deyimi *stmt* formun:
```csharp
try try_block finally finally_block
```

*  Belirli atama onayına durumunu *v* başındaki *try_block* kesin atama durumu ile aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* başındaki *finally_block* kesin atama durumu ile aynı *v* başındaki *stmt* .
*  Belirli atama onayına durumunu *v* uç nokta adresindeki *stmt* kesinlikle atanmış bir IF (ve yalnızca) aşağıdakilerden en az birini doğrudur:
    * *v* kesinlikle bitiş noktasını atanır *try_block*
    * *v* kesinlikle bitiş noktasını atanır *finally_block*

Denetim akışı aktarımı varsa (örneğin, bir `goto` deyimi) yapılması içinde başlar *try_block*ve dışında sona eren *try_block*, ardından *v* de Bu denetim akışı aktarımını kesinlikle atanan kabul *v* kesinlikle bitiş noktasını atanır *finally_block*. (Bu yalnızca Eğer değildir — varsa *v* hala kesinlikle atanan değerlendirilir sonra kesinlikle bu denetim akışı aktarım başka bir nedenle atanır.)

#### <a name="try-catch-finally-statements"></a>Try-catch-finally deyimleri

Analiz için kesin atama bir `try` - `catch` - `finally` formun deyimi:
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
deyim'ymiş gibi yapılır bir `try` - `finally` deyimi kapsayan bir `try` - `catch` deyimi:
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Aşağıdaki örnek, gösterir nasıl farklı bloklarını bir `try` deyimi ([try deyimi](statements.md#the-try-statement)) belirli atama onayına etkiler.
```csharp
class A
{
    static void F() {
        int i, j;
        try {
            goto LABEL;
            // neither i nor j definitely assigned
            i = 1;
            // i definitely assigned
        }

        catch {
            // neither i nor j definitely assigned
            i = 3;
            // i definitely assigned
        }

        finally {
            // neither i nor j definitely assigned
            j = 5;
            // j definitely assigned
            }
        // i and j definitely assigned
        LABEL:;
        // j definitely assigned

    }
}
```

#### <a name="foreach-statements"></a>Foreach deyimleri

İçin bir `foreach` deyimi *stmt* formun:
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* üzerinde denetim akışı aktarımı *embedded_statement* veya bitiş noktasını *stmt* durumunu aynı *v* sonunda *expr*.

#### <a name="using-statements"></a>Using deyimleri

İçin bir `using` deyimi *stmt* formun:
```csharp
using ( resource_acquisition ) embedded_statement
```

*  Belirli atama onayına durumunu *v* başındaki *resource_acquisition* durumunu aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* üzerinde denetim akışı aktarımı *embedded_statement* durumunu aynı *v* sonunda *resource_ Alım*.

#### <a name="lock-statements"></a>Kilit deyimleri

İçin bir `lock` deyimi *stmt* formun:
```csharp
lock ( expr ) embedded_statement
```

*  Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* üzerinde denetim akışı aktarımı *embedded_statement* durumunu aynı *v* sonunda *expr*.

#### <a name="yield-statements"></a>Yield deyimleri

İçin bir `yield return` deyimi *stmt* formun:
```csharp
yield return expr ;
```

*  Belirli atama onayına durumunu *v* başındaki *expr* durumunu aynı *v* başındaki *stmt*.
*  Belirli atama onayına durumunu *v* sonunda *stmt* durumunu aynı *v* sonunda *expr*.
*  A `yield break` deyimi belirli atama onayına durumu üzerinde hiçbir etkisi.

#### <a name="general-rules-for-simple-expressions"></a>Basit ifadeler için genel kurallar

Bu tür deyimler için aşağıdaki kural uygulanır: değişmez değerler ([değişmez değerleri](expressions.md#literals)), basit adları ([basit adları](expressions.md#simple-names)), üye erişimi ifadeleri ([üye erişimi](expressions.md#member-access)), temel erişim dizini oluşturulmamış ifadeleri ([temel erişim](expressions.md#base-access)), `typeof` ifadeleri ([typeof işleci](expressions.md#the-typeof-operator)), varsayılan değer ifadeleri ([varsayılan değer ifadeleri ](expressions.md#default-value-expressions)) ve `nameof` ifadeleri ([Nameof ifadeleri](expressions.md#nameof-expressions)).

*  Belirli atama onayına durumunu *v* böyle bir ifade sonunda kesin atama durumu ile aynıdır *v* ifadenin başında.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Katıştırılmış ifadeler ile ifadeler için genel kurallar

Bu tür deyimler için aşağıdaki kurallar geçerlidir: parantezli ifade ([parantezli ifade](expressions.md#parenthesized-expressions)), öğe erişimi ifadeleri ([öğe erişimi](expressions.md#element-access)), temel erişim ile ifadeler Dizin oluşturma ([temel erişim](expressions.md#base-access)), artırmak ve azaltma ifadeleri ([sonek arttırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [önek arttırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), cast ifadeleri ([Cast ifadeleri](expressions.md#cast-expressions)), birli `+`, `-`, `~`, `*` ifadeleri, ikili `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `<`, `<=`, `>`, `>=`, `==`, `!=`, `is`, `as`, `&`, `|`, `^` ifadeleri ([aritmetik işleçler](expressions.md#arithmetic-operators), [kaydırma işleçleri](expressions.md#shift-operators), [ilişkisel ve tür testi işleçleri](expressions.md#relational-and-type-testing-operators) [Mantıksal işleçler](expressions.md#logical-operators)), bileşik atama deyimleri ([bileşik atama](expressions.md#compound-assignment)), `checked` ve `unchecked` ifadeleri ([checked ve unchecked işleçler](expressions.md#the-checked-and-unchecked-operators)), dizi ve temsilci oluşturma ifadeleri artı ([new işleci](expressions.md#the-new-operator)).

Bu ifadeler sabit bir sırada koşulsuz olarak değerlendirilen bir veya daha fazla alt ifadeler vardır. Örneğin, ikili `%` işleci işlecinin sol tarafı, sonra sağ taraftaki değerlendirir. Bir dizin oluşturma işlemi, dizinli ifadeyi değerlendirir ve her birini sırayla soldan sağa doğru dizin ifadeleri değerlendirir. Bir ifade için *expr*, alt ifadeler olduğu *e1, e2,..., tr*, o sırada değerlendirilir:

*  Belirli atama onayına durumunu *v* başındaki *e1* başındaki kesin atama durumu ile aynı *expr*.
*  Belirli atama onayına durumunu *v* başındaki *ei* (*miyim* birden büyük) önceki alt ifadenin sonunda kesin atama durumu ile aynıdır.
*  Kesin atama durumu *v* sonunda *expr* sonunda kesin atama durumu ile aynı *tr*

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Çağrı ifadeleri ve nesne oluşturma ifadeleri

Çağrı ifadesi için *expr* formun:
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
veya bir nesne oluşturma ifadesi formun:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Belirli atama onayına durumu bir başlatma ifadesi için *v* önce *primary_expression* durumunu aynı *v* önce *expr*.
*  Belirli atama onayına durumu bir başlatma ifadesi için *v* önce *arg1* durumunu aynı *v* sonra *primary_expression*.
*  Bir nesne oluşturma ifadesi, kesin atama durumu için *v* önce *arg1* durumunu aynı *v* önce *expr*.
*  Her bağımsız değişkeni *argi*, kesin atama durumu *v* sonra *argi* herhangi yoksayılıyor normal ifade kuralları tarafından belirlenir `ref` veya `out`değiştiriciler.
*  Her bağımsız değişkeni *argi* herhangi *miyim* daha büyük bir özelliğine belirli atama durumu *v* önce *argi* durumu ile aynıdır *v* önceki sonra *arg*.
*  Varsa değişkeni *v* olarak geçirilen bir `out` bağımsız değişken (yani, formun bağımsız değişken `out v`) herhangi bir bağımsız değişken, daha sonra durumunu *v* sonra *expr* kesinlikle atanır. Aksi durumda; durumunu *v* sonra *expr* durumunu aynı *v* sonra *argN*.
*  Dizi başlatıcıları için ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)), nesne başlatıcıları ([nesne başlatıcılarda](expressions.md#object-initializers)), koleksiyon başlatıcıları ([koleksiyon başlatıcıları](expressions.md#collection-initializers)) ve Anonim nesne başlatıcıları ([anonim nesne oluşturma ifadeleri](expressions.md#anonymous-object-creation-expressions)), kesin atama durumu açısından bu yapıları tanımlanan genişletme tarafından belirlenir.

#### <a name="simple-assignment-expressions"></a>Basit atama deyimleri

Bir ifade için *expr* formun `w = expr_rhs`:

*  Belirli atama onayına durumunu *v* önce *expr_rhs* kesin atama durumu ile aynı *v* önce *expr*.
*  Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:
   * Varsa *w* olarak aynı değişken *v*, ardından belirli atama onayına durumunu *v* sonra *expr* kesinlikle atanır.
   * Aksi takdirde, atama bir yapı türünün örnek oluşturucusu içinde ortaya çıkarsa *w* otomatik olarak uygulanan bir özelliği bir özellik erişim *P* yapılandırılmakta örneğinde ve *v* gizli yedekleme alanı *P*, ardından belirli atama onayına durumunu *v* sonra *expr* kesinlikle olduğu atanmış.
   * Aksi takdirde, kesin atama durumu *v* sonra *expr* kesin atama durumu ile aynı *v* sonra *expr_rhs*.

#### <a name="-conditional-and-expressions"></a>& & (ve koşullu) ifadeleri

Bir ifade için *expr* formun `expr_first && expr_second`:

*  Belirli atama onayına durumunu *v* önce *expr_first* kesin atama durumu ile aynı *v* önce *expr*.
*  Belirli atama onayına durumunu *v* önce *expr_second* varsa kesinlikle atanır durumunu *v* sonra *expr_first* ya da kesinlikle atanan veya "gerçek ifade sonra atanan kesinlikle". Aksi takdirde, kesinlikle atanmadı.
*  Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:
    * Varsa *expr_first* değerine sahip bir sabit ifade ise `false`, ardından belirli atama onayına durumunu *v* sonra *expr* kesin atama ile aynıdır durumu *v* sonra *expr_first*.
    * Aksi halde, durumunu *v* sonra *expr_first* kesinlikle atandığı, ardından durumunu *v* sonra *expr* kesinlikle atanır.
    * Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanır ve durumunu *v* sonra *expr_first* "kesinlikle olduğu yanlış ifade sonra atanan", ardından durumunu *v* sonra *expr* kesinlikle atanır.
    * Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanan veya "gerçek ifade sonra atanan kesinlikle", ardından durumunu *v* sonra  *Expr* "kesinlikle true ifadeden sonra atanan".
    * Aksi halde, durumunu *v* sonra *expr_first* "false ifadesi sonra atanan kesinlikle" ve durumunu *v* sonra *expr_second* "false ifadesi sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle false ifadeden sonra atanan".
    * Aksi takdirde, durumunu *v* sonra *expr* kesinlikle atanmadı.

Örnekte
```csharp
class A
{
    static void F(int x, int y) {
        int i;
        if (x >= 0 && (i = y) >= 0) {
            // i definitely assigned
        }
        else {
            // i not definitely assigned
        }
        // i not definitely assigned
    }
}
```
değişken `i` kesinlikle atanan katıştırılmış deyimleri biri olarak kabul edilir bir `if` deyimi ancak başka birinde olmayan. İçinde `if` yöntemi deyiminde `F`, değişkeni `i` katıştırılmış ilk deyimde çünkü kesinlikle atanan ifade yürütülmesini `(i = y)` önce her zaman bu katıştırılmış deyim yürütme. Buna karşılık, değişken `i` ikinci katıştırılmış deyim kesinlikle beri atanmadı `x >= 0` değişkeninde kaynaklanan yanlış sınanmıştır `i` atanmamış.

#### <a name="-conditional-or-expressions"></a>|| (koşullu veya) ifadeleri

Bir ifade için *expr* formun `expr_first || expr_second`:

*  Belirli atama onayına durumunu *v* önce *expr_first* kesin atama durumu ile aynı *v* önce *expr*.
*  Belirli atama onayına durumunu *v* önce *expr_second* varsa kesinlikle atanır durumunu *v* sonra *expr_first* ya da kesinlikle atanan veya "false ifadesi sonra atanan kesinlikle". Aksi takdirde, kesinlikle atanmadı.
*  Belirli atama onayına deyiminin *v* sonra *expr* tarafından belirlenir:
    * Varsa *expr_first* değerine sahip bir sabit ifade ise `true`, ardından belirli atama onayına durumunu *v* sonra *expr* kesin atama ile aynıdır durumu *v* sonra *expr_first*.
    * Aksi halde, durumunu *v* sonra *expr_first* kesinlikle atandığı, ardından durumunu *v* sonra *expr* kesinlikle atanır.
    * Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanır ve durumunu *v* sonra *expr_first* "kesinlikle olduğu true deyim sonra atanan", ardından durumunu *v* sonra *expr* kesinlikle atanır.
    * Aksi halde, durumunu *v* sonra *expr_second* kesinlikle atanan veya "false ifadesi sonra atanan kesinlikle", ardından durumunu *v* sonra*expr* "kesinlikle false ifadeden sonra atanan".
    * Aksi halde, durumunu *v* sonra *expr_first* "true deyim sonra atanan kesinlikle" ve durumunu *v* sonra *expr_second*"true deyim sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle true ifadeden sonra atanan".
    * Aksi takdirde, durumunu *v* sonra *expr* kesinlikle atanmadı.

Örnekte
```csharp
class A
{
    static void G(int x, int y) {
        int i;
        if (x >= 0 || (i = y) >= 0) {
            // i not definitely assigned
        }
        else {
            // i definitely assigned
        }
        // i not definitely assigned
    }
}
```
değişken `i` kesinlikle atanan katıştırılmış deyimleri biri olarak kabul edilir bir `if` deyimi ancak başka birinde olmayan. İçinde `if` yöntemi deyiminde `G`, değişkeni `i` ikinci katıştırılmış deyiminde çünkü kesinlikle atanan ifade yürütülmesini `(i = y)` önce her zaman bu katıştırılmış deyim yürütme. Buna karşılık, değişken `i` kesinlikle ilk katıştırılmış deyim beri atanmadı `x >= 0` değişkeninde kaynaklanan doğru sınanmıştır `i` atanmamış.

#### <a name="-logical-negation-expressions"></a>! (mantıksal olumsuzlama) ifadeleri

Bir ifade için *expr* formun `! expr_operand`:

*  Belirli atama onayına durumunu *v* önce *expr_operand* kesin atama durumu ile aynı *v* önce *expr*.
*  Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:
    * Varsa durumunu *v* sonra * expr_operand * kesinlikle atandığı, ardından durumunu *v* sonra *expr* kesinlikle atanır.
    * Varsa durumunu *v* sonra * expr_operand * kesinlikle atanmadı, ardından durumunu *v* sonra *expr* kesinlikle atanmadı.
    * Varsa durumunu *v* sonra * expr_operand * "false ifadesi sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle true sonra atanan "ifadesi".
    * Varsa durumunu *v* sonra * expr_operand * "true deyim sonra atanan kesinlikle" sonra durumunu *v* sonra *expr* "kesinlikle false sonra atanan "ifadesi".

#### <a name="-null-coalescing-expressions"></a>?? ifadeler (null birleşim)

Bir ifade için *expr* formun `expr_first ?? expr_second`:

*  Belirli atama onayına durumunu *v* önce *expr_first* kesin atama durumu ile aynı *v* önce *expr*.
*  Belirli atama onayına durumunu *v* önce *expr_second* kesin atama durumu ile aynı *v* sonra *expr_first*.
*  Belirli atama onayına deyiminin *v* sonra *expr* tarafından belirlenir:
    * Varsa *expr_first* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) null değeriyle sonra durumunu *v* sonra *expr* aynıdır durumu *v* sonra *expr_second*.
*  Aksi takdirde, durumunu *v* sonra *expr* kesin atama durumu ile aynı *v* sonra *expr_first*.

#### <a name="-conditional-expressions"></a>?: (koşullu) ifadeler

Bir ifade için *expr* formun `expr_cond ? expr_true : expr_false`:

*  Belirli atama onayına durumunu *v* önce *expr_cond* durumunu aynı *v* önce *expr*.
*  Belirli atama onayına durumunu *v* önce *expr_true* aşağıdakilerden birini tutar ve yalnızca, kesinlikle atanır:
    * *expr_cond* değerine sahip bir sabit ifade ise `false`
    * durumunu *v* sonra *expr_cond* kesinlikle atanmış veya "kesinlikle true ifadeden sonra atanan".
*  Belirli atama onayına durumunu *v* önce *expr_false* aşağıdakilerden birini tutar ve yalnızca, kesinlikle atanır:
    * *expr_cond* değerine sahip bir sabit ifade ise `true`
*  durumunu *v* sonra *expr_cond* kesinlikle atanmış veya "false ifadeden sonra kesinlikle atanan".
*  Belirli atama onayına durumunu *v* sonra *expr* tarafından belirlenir:
    * Varsa *expr_cond* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) değerine sahip `true` sonra durumunu *v* sonra *expr* durumu aynı *v* sonra *expr_true*.
    * Aksi takdirde *expr_cond* sabit ifade ([sabit ifadeler](expressions.md#constant-expressions)) değerine sahip `false` sonra durumunu *v* sonra *expr* durumunu aynı *v* sonra *expr_false*.
    * Aksi halde, durumunu *v* sonra *expr_true* kesinlikle atanır ve durumunu *v* sonra *expr_false* kesinlikle olduğu atanan, ardından durumunu *v* sonra *expr* kesinlikle atanır.
    * Aksi takdirde, durumunu *v* sonra *expr* kesinlikle atanmadı.

#### <a name="anonymous-functions"></a>Anonim İşlevler

İçin bir *lambda_expression* veya *anonymous_method_expression* *expr* gövde ile (her iki *blok* veya *ifadesi* ) *gövdesi*:

*  Bir harici değişken kesin atama durumu *v* önce *gövdesi* durumunu aynı *v* önce *expr*. Diğer bir deyişle, dış değişkenlerin kesin atama durumu anonim işlev bağlamından devralınır.
*  Bir harici değişken kesin atama durumu *v* sonra *expr* durumunu aynı *v* önce *expr*.

Örnek
```csharp
delegate bool Filter(int i);

void F() {
    int max;

    // Error, max is not definitely assigned
    Filter f = (int n) => n < max;

    max = 5;
    DoWork(f);
}
```
bu yana bir derleme zamanı hatası oluşturur `max` burada anonim işlev bildirildiği kesinlikle atanmadı. Örnek
```csharp
delegate void D();

void F() {
    int n;
    D d = () => { n = 1; };

    d();

    // Error, n is not definitely assigned
    Console.WriteLine(n);
}
```
Ayrıca atamaya beri bir derleme zamanı hatası oluşturur `n` anonim işlev kesin atama durumu üzerinde herhangi bir etkisi yoktur `n` dışında anonim bir işlevdir.

## <a name="variable-references"></a>Değişken başvuruları

A *variable_reference* olduğu bir *ifade* bir değişken olarak sınıflandırılmış. A *variable_reference* geçerli değeri getirilemedi ve yeni bir değeri depolamak için erişilebilir bir depolama konumu gösterir.

```antlr
variable_reference
    : expression
    ;
```

C ve C++, *variable_reference* olarak bilinen bir *lvalue*.

## <a name="atomicity-of-variable-references"></a>Kararlılık değişken başvuruları

Okuma ve yazma işlemleri aşağıdaki veri türlerinden atomik: `bool`, `char`, `byte`, `sbyte`, `short`, `ushort`, `uint`, `int`, `float`ve başvuru türleri. Ayrıca, okuma ve yazma işlemleri önceki listede bir temel türü ile numaralandırma türleri de atomiktir. Okuma ve yazma işlemleri dahil olmak üzere diğer türde `long`, `ulong`, `double`, ve `decimal`, kullanıcı tanımlı türler yanı sıra atomik olması garanti edilmez. Bu amaca göre tasarlanan kitaplığı işlevleri tarafından garantisi yoktur, atomik okuma-değiştirme-yazma gibi artırma veya azaltma söz konusu olduğunda.

