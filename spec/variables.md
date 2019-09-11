---
ms.openlocfilehash: a01cf9387b8dc47de036bf0bd1496c19a441d81c
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876803"
---
# <a name="variables"></a>Değişkenler

Değişkenler, depolama konumlarını temsil eder. Her değişken, değişkende hangi değerlerin depolanabileceğini belirleyen bir tür içerir. C#tür açısından güvenli bir dildir ve C# derleyici, değişkenler içinde depolanan değerlerin her zaman uygun türde olmasını garanti eder. Bir değişkenin değeri atama yoluyla veya `++` ve `--` işleçleri kullanılarak değiştirilebilir.

Değerin alınabilmesi için önce bir değişken kesin olarak ***atanmalıdır*** ([kesin atama](variables.md#definite-assignment)).

Aşağıdaki bölümlerde açıklandığı gibi, değişkenler ***Başlangıçta atanır*** veya ***Başlangıçta atanmamıştır***. Başlangıçta atanan değişkenin iyi tanımlanmış bir başlangıç değeri vardır ve her zaman kesinlikle atanan olarak kabul edilir. Başlangıçta atanmamış bir değişkenin ilk değeri yok. Başlangıçta atanmamış bir değişkenin belirli bir konumda kesinlikle atanabileceği kabul edilmesi için, değişkene bir atama, bu konuma yönelik her olası yürütme yolunda gerçekleşmelidir.

## <a name="variable-categories"></a>Değişken kategorileri

C#değişkenlerin yedi kategorisini tanımlar: statik değişkenler, örnek değişkenleri, dizi öğeleri, değer parametreleri, başvuru parametreleri, çıkış parametreleri ve yerel değişkenler. Aşağıdaki bölümler bu kategorilerin her birini anlatmaktadır.

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
`x``y` statik bir değişkendir, bir örnek `v[0]` değişkenidir, bir dizi öğesidir `a` , bir `c` değer parametresidir `b` , bir başvuru parametresidir, bir çıkış parametresidir ve `i` bir yerel değişkendir .

### <a name="static-variables"></a>Statik değişkenler

`static` Değiştiriciyle belirtilen bir alana ***statik değişken***denir. Statik bir değişken, kendi kapsayıcı türü için statik oluşturucunun ([statik oluşturucular](classes.md#static-constructors)) yürütülmesinden önce varlığına gelir ve ilişkili uygulama etki alanı mevcut olduğunda var olmaya erer.

Statik bir değişkenin ilk değeri, değişkenin türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).

Kesin atama denetimi amacıyla, statik bir değişken başlangıçta atanan olarak kabul edilir.

### <a name="instance-variables"></a>Örnek değişkenleri

`static` Değiştirici olmadan tanımlanan bir alana ***örnek değişkeni***denir.

#### <a name="instance-variables-in-classes"></a>Sınıflarda örnek değişkenleri

Bir sınıfın örnek değişkeni, bu sınıfın yeni bir örneği oluşturulduğunda var olur ve bu örneğe bir başvuru olmadığında ve örneğin yok edicinin (varsa) yürütüldüğünden, var olmaya erer.

Bir sınıfın örnek değişkeninin ilk değeri, değişkenin türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).

Kesin atama denetimi amacıyla, bir sınıfın örnek değişkeni başlangıçta atanan olarak kabul edilir.

#### <a name="instance-variables-in-structs"></a>Yapılarda örnek değişkenleri

Bir yapının örnek değişkeni, ait olduğu yapı değişkeniyle tam olarak aynı yaşam süresine sahiptir. Diğer bir deyişle, bir yapı türü değişkeni var olduğunda veya varsa, yapının örnek değişkenlerini çok da yapabilirsiniz.

Bir yapının örnek değişkeninin ilk atama durumu, kapsayan yapı değişkeni ile aynıdır. Diğer bir deyişle, bir struct değişkeni başlangıçta atanan olarak kabul edildiğinde, bu yüzden örnek değişkenleri çok fazla olur ve bir struct değişkeni başlangıçta atanmamış olarak kabul edildiğinde, örnek değişkenleri de aynı şekilde atanmamış olur.

### <a name="array-elements"></a>Dizi öğeleri

Bir dizinin öğeleri, bir dizi örneği oluşturulduğunda varlığına gelir ve bu dizi örneğine bir başvuru olmadığında var olmaya sona bırakılır.

Bir dizinin öğelerinin ilk değeri, dizi öğelerinin türünün varsayılan değeridir ([varsayılan değerlerdir](variables.md#default-values)).

Kesin atama denetimi amacıyla, bir dizi öğesi başlangıçta atanan olarak kabul edilir.

### <a name="value-parameters"></a>Değer parametreleri

`ref` Or`out` değiştiricisi olmadan belirtilen bir parametre bir ***değer parametresidir***.

Bir değer parametresi, işlev üyesi (yöntem, örnek Oluşturucu, erişimci veya işleç) veya parametrenin ait olduğu anonim işlev çağrılandan sonra var olur ve bu, çağrısında verilen bağımsız değişkenin değeri ile başlatılır. İşlev üyesi veya anonim işlev geri alındıktan sonra normalde bir değer parametresi yok olarak sona erer. Ancak, değer parametresi anonim bir işlev ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) tarafından yakalandıysa, bu anonim işlevden oluşturulan temsilci veya ifade ağacı çöp toplama için uygun olana kadar en az bir değeri uzatır.

Kesin atama denetimi amacıyla bir değer parametresi başlangıçta atanan olarak kabul edilir.

### <a name="reference-parameters"></a>Başvuru parametreleri

`ref` Değiştirici ile belirtilen bir parametre bir ***başvuru parametresidir***.

Başvuru parametresi yeni bir depolama konumu oluşturmaz. Bunun yerine, bir başvuru parametresi, işlev üyesinde bağımsız değişken olarak verilen değişken ile aynı depolama konumunu temsil eder veya anonim işlev çağırma. Bu nedenle, başvuru parametresinin değeri her zaman temeldeki değişkenle aynıdır.

Aşağıdaki kesin atama kuralları başvuru parametreleri için geçerlidir. Çıkış parametrelerinde açıklanan çıkış parametrelerine yönelik farklı kurallara göz önünde [edin.](variables.md#output-parameters)

*  Bir değişken, bir işlev üyesine veya temsilci çağrısına başvuru parametresi olarak geçirilebilmesi için kesinlikle atanmalı ([kesin atama](variables.md#definite-assignment)).
*  Bir işlev üyesi veya anonim işlev içinde, başlangıçta atanan bir başvuru parametresi olarak kabul edilir.

Bir yapı türünün örnek metodu veya örnek erişimcisi içinde, `this` anahtar sözcüğü tam olarak yapı türünün başvuru parametresi olarak davranır ([Bu erişim](expressions.md#this-access)).

### <a name="output-parameters"></a>Çıktı parametreleri

`out` Değiştirici ile belirtilen bir parametre bir ***çıkış parametresidir***.

Çıkış parametresi yeni bir depolama konumu oluşturmaz. Bunun yerine, bir çıkış parametresi işlev üyesinde veya temsilci çağrısında bağımsız değişken olarak verilen değişkenle aynı depolama konumunu temsil eder. Bu nedenle, bir çıktı parametresinin değeri her zaman temeldeki değişkenle aynıdır.

Aşağıdaki kesin atama kuralları çıkış parametreleri için geçerlidir. [Başvuru](variables.md#reference-parameters)parametrelerinde açıklanan başvuru parametrelerine yönelik farklı kurallara göz önünde edin.

*  Bir değişken, bir işlev üyesinde veya temsilci çağrısında çıkış parametresi olarak geçirilebilmesi için kesinlikle atanmamalıdır.
*  Bir işlev üyesinin veya temsilci çağrısının normal tamamlanmasını takip eden bir çıktı parametresi olarak geçirilen her değişken, o yürütme yolunda atanmış olarak değerlendirilir.
*  Bir işlev üyesi veya anonim işlev içinde, bir çıkış parametresi başlangıçta atanmamış olarak değerlendirilir.
*  İşlev üyesi veya anonim işlev normal bir şekilde döndürüldüğünden, bir işlev üyesinin veya anonim işlevin her çıkış parametresi kesinlikle atanmalıdır ([kesin atama](variables.md#definite-assignment)).

Yapı türünün `this` örnek Oluşturucusu içinde anahtar sözcüğü, yapı türünün ([Bu erişim](expressions.md#this-access)) bir çıkış parametresi olarak tam olarak davranır.

### <a name="local-variables"></a>Yerel değişkenler

***Yerel bir değişken*** bir *blok*, *for_statement*, *switch_statement* veya *using_statement*ile oluşabilen bir *local_variable_declaration*tarafından bildirilmiştir; ya da bir *foreach_statement* ya da bir *try_statement*için *specific_catch_clause* .

Yerel bir değişkenin ömrü, program yürütmenin, bu için ayrılan depolama garantisi olan bölümüdür. Bu yaşam süresi en az *girişi,* *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya *specific_catch_clause* ile ilişkili olduğu, Bu *bloğun*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya *specific_catch_clause* herhangi bir şekilde yürütülmesi sona erer. (Bir kapalı *blok* girilmesi veya bir yöntemi çağırmak askıya alır, ancak bitmez, geçerli *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya specific_ yürütmez  *catch_clause*.) Yerel değişken anonim bir işlev ([yakalanan dış değişkenler](expressions.md#captured-outer-variables)) tarafından yakalandıysa, yaşam süresi en az, anonim işlevden oluşturulan temsilci veya ifade ağacı, buna başvurmak üzere gelen diğer nesnelerle birlikte yakalanan değişken çöp toplama için uygun.

Üst *blok*, *for_statement*, *switch_statement*, *using_statement*, *foreach_statement*veya *specific_catch_clause* yinelemeli olarak girilirse, her biri yerel değişkenin yeni bir örneği oluşturulur. zaman ve varsa, *local_variable_initializer*her seferinde değerlendirilir.

Bir *local_variable_declaration* tarafından tanıtılan yerel bir değişken otomatik olarak başlatılmaz ve bu nedenle varsayılan değere sahip değildir. Kesin atama denetimi amacıyla, bir *local_variable_declaration* tarafından tanıtılan yerel bir değişken başlangıçta atanmamış olarak değerlendirilir. Bir *local_variable_declaration* , bir *local_variable_initializer*içerebilir; bu durumda değişken yalnızca başlatma ifadesinden ([bildirim deyimleri](variables.md#declaration-statements)) sonra kesin olarak atanır.

Bir *local_variable_declaration*tarafından tanıtılan yerel bir değişken kapsamında, bu yerel değişkene *local_variable_declarator*' den önceki bir metinsel konumda başvurabileceğiniz derleme zamanı hatasıdır. Yerel değişken bildirimi örtük ([yerel değişken bildirimleri](statements.md#local-variable-declarations)) ise, *local_variable_declarator*içinde değişkene başvurmak için de bir hatadır.

Bir *foreach_statement* veya *specific_catch_clause* tarafından tanıtılan yerel bir değişken, tüm kapsamda kesin olarak atanmış olarak değerlendirilir.

Yerel bir değişkenin gerçek yaşam süresi uygulamaya bağımlıdır. Örneğin, bir derleyici bir bloktaki yerel değişkenin yalnızca o bloktaki küçük bir bölüm için kullanıldığını statik olarak belirleyebilir. Derleyici bu analizi kullanarak, değişkenin depolamanın, kapsayan bloğundan daha kısa bir yaşam süresine sahip olan bir kod üretebilir.

Yerel başvuru değişkeni tarafından başvurulan depolama alanı, yerel başvuru değişkeninin ([Otomatik bellek yönetimi](basic-concepts.md#automatic-memory-management)) yaşam süresinden bağımsız olarak geri kazanılır.

## <a name="default-values"></a>Varsayılan değerler

Aşağıdaki değişken kategorileri varsayılan değerlerine otomatik olarak başlatılır:

*  Statik değişkenler.
*  Sınıf örneklerinin örnek değişkenleri.
*  Dizi öğeleri.

Bir değişkenin varsayılan değeri, değişkenin türüne bağlıdır ve aşağıdaki şekilde belirlenir:

*  Bir *value_type*değişkeni için varsayılan değer, *value_type*'nin varsayılan oluşturucusu tarafından hesaplanan değerle aynıdır ([Varsayılan oluşturucular](types.md#default-constructors)).
*  Bir *reference_type*değişkeni için varsayılan değer `null`.

Varsayılan değerlere başlatma işlemi, kullanım için ayrılmadan önce bellek Yöneticisi veya çöp toplayıcı başlatma belleğini tümüyle bit sıfır olarak başlatmaya gerek kalmadan yapılır. Bu nedenle, null başvuruyu göstermek için tümü-bit-sıfır kullanmak kullanışlıdır.

## <a name="definite-assignment"></a>Kesin atama

Bir işlev üyesinin çalıştırılabilir kodundaki belirli bir konumda, derleyici, belirli bir statik Akış Analizi ([kesin atamayı belirlemek Için kesin kurallar](variables.md#precise-rules-for-determining-definite-assignment)) tarafından kanıtlabiliyorsa ***,*** değişken otomatik olarak başlatıldı veya en az bir atamanın hedefi oldu. Belirli bir deyişle, kesin atama kuralları şunlardır:

*  Başlangıçta atanan değişken ([Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables)) her zaman kesinlikle atanmış olarak değerlendirilir.
*  Başlangıçta atanmamış değişken ([Başlangıçta atanmamış değişkenler](variables.md#initially-unassigned-variables)), söz konusu konuma yönelik tüm olası yürütme yolları aşağıdakilerden en az birini içeriyorsa, belirli bir konumda kesinlikle atanmış olarak kabul edilir:
    * Değişkenin sol işlenen olduğu basit atama ([basit atama](expressions.md#simple-assignment)).
    * Değişkeni çıkış parametresi olarak geçiren bir çağırma ifadesi ([çağırma ifadeleri](expressions.md#invocation-expressions)) veya nesne oluşturma Ifadesi ([nesne oluşturma ifadeleri](expressions.md#object-creation-expressions)).
    * Yerel bir değişken için, değişken başlatıcısı içeren bir yerel değişken bildirimi ([yerel değişken bildirimleri](statements.md#local-variable-declarations)).

Yukarıdaki resmi olmayan kuralları temel alan biçimsel belirtim, [Başlangıçta atanan değişkenler](variables.md#initially-assigned-variables), [Başlangıçta atanmamış değişkenler](variables.md#initially-unassigned-variables)ve kesin [atamayı belirlemek için tam kurallar](variables.md#precise-rules-for-determining-definite-assignment)bölümünde açıklanmaktadır.

Bir *struct_type* değişkeninin örnek değişkenlerinin kesin atama durumları, tek tek ve toplu olarak izlenir. Yukarıdaki kurallara ek olarak, aşağıdaki kurallar *struct_type* değişkenleri ve onların örnek değişkenleri için geçerlidir:

*  İçeren *struct_type* değişkeni kesin olarak atanmış olarak kabul edildiğinde, örnek değişken kesin olarak atanır olarak değerlendirilir.
*  Örnek değişkenlerinin her biri kesinlikle atanmış olarak kabul edildiğinde, bir *struct_type* değişkeni kesin olarak atanır.

Kesin atama, aşağıdaki bağlamlarda gereksinimdir:

*  Bir değişken, değerinin alındığı her konumda kesinlikle atanmalıdır. Bu, tanımsız değerlerin hiçbir şekilde gerçekleşmemesini sağlar. Bir ifadede bir değişkenin oluşma nedeni, değişkenin değerini elde etmek için kabul edilir, ancak
    * değişken, basit bir atamanın sol işleneni,
    * değişken, çıkış parametresi olarak geçirilir veya
    * değişken bir *struct_type* değişkenidir ve bir üye erişiminin sol işleneni olarak gerçekleşir.
*  Bir değişken, başvuru parametresi olarak geçirildiği her bir konumda kesinlikle atanmalıdır. Bu, çağrılan işlev üyesinin başlangıçta atanan başvuru parametresini düşünebilmesini sağlar.
*  İşlev üyesinin tüm çıkış parametreleri, işlev üyesinin döndürdüğü her konumda kesinlikle atanmalıdır (bir `return` ifadesiyle veya işlev üyesi gövdesinin sonuna ulaşan yürütme yoluyla). Bu, işlev üyelerinin çıkış parametrelerinde tanımsız değerler döndürmemesini sağlar, bu sayede derleyicinin değişken atama ile eşdeğer bir çıkış parametresi olarak bir değişken alan bir işlev üye çağrısını düşünmesini sağlar.
*  Bir `this` *struct_type* Instance oluşturucusunun değişkeni, örnek oluşturucunun döndürdüğü her bir konumda kesinlikle atanmalıdır.

### <a name="initially-assigned-variables"></a>Başlangıçta atanan değişkenler

Aşağıdaki değişken kategorileri başlangıçta atanan olarak sınıflandırılır:

*  Statik değişkenler.
*  Sınıf örneklerinin örnek değişkenleri.
*  Başlangıçta atanan yapı değişkenlerinin örnek değişkenleri.
*  Dizi öğeleri.
*  Değer parametreleri.
*  Başvuru parametreleri.
*  Bir `catch` yan tümce `foreach` veya deyime göre belirtilen değişkenler.

### <a name="initially-unassigned-variables"></a>Başlangıçta atanmamış değişkenler

Aşağıdaki değişken kategorileri başlangıçta atanmamış olarak sınıflandırılır:

*  Başlangıçta atanmamış yapı değişkenlerinin örnek değişkenleri.
*  Yapı örneği oluşturucularının `this` değişkeni de dahil olmak üzere çıkış parametreleri.
*  Bir `catch` yan tümce veya bir `foreach` bildirimde tanımlananlar hariç yerel değişkenler.

### <a name="precise-rules-for-determining-definite-assignment"></a>Kesin atamayı belirlemek için kesin kurallar

Kullanılan her değişkenin kesinlikle atandığını tespit etmek için derleyicinin bu bölümde açıklanacak bir işlem kullanması gerekir.

Derleyici, bir veya daha fazla başlangıçta atanmamış değişkenine sahip her bir işlev üyesinin gövdesini işler. Her başlangıçta atanmamış *her değişken için*, derleyici işlev üyesinde aşağıdaki noktaların her birinde bulunan *v* için kesin bir ***atama durumu*** belirler:

*  Her deyimin başlangıcında
*  Her deyimin bitiş noktasında ([bitiş noktaları ve ulaşılabilirlik](statements.md#end-points-and-reachability))
*  Her bir yay üzerinde, denetimi başka bir bildirime veya bir deyimin bitiş noktasına aktarır
*  Her ifadenin başlangıcında
*  Her ifadenin sonunda

*V* 'nin kesin atama durumu aşağıdakilerden biri olabilir:

*  Kesinlikle atandı. Bu, tüm olası denetimde bu noktaya akar, *v* 'nin bir değer atandığını gösterir.
*  Kesin olarak atanmadı. Türünde `bool`bir ifadenin sonundaki bir değişkenin durumu için, kesinlikle atanmayan bir değişkenin durumu (ancak gerekli değildir) aşağıdaki alt durumlardan birine denk gelebilir:
    * Doğru ifadeden sonra kesinlikle atandı. Bu durum, Boole ifadesi doğru olarak değerlendiriliyorsa, ancak Boolean ifadesi false olarak değerlendiriliyorsa atanmadığında, *v* 'nin kesinlikle atandığını gösterir.
    * Yanlış ifadeden sonra kesinlikle atandı. Bu durum, Boole ifadesi false olarak değerlendirilirse, ancak Boolean ifadesi doğru olarak değerlendiriliyorsa atanmadığında, *v* 'nin kesinlikle atandığını gösterir.

Aşağıdaki kurallar, her konumda bir *v* değişkeninin durumunun nasıl belirlendiğini yönetir.

#### <a name="general-rules-for-statements"></a>Deyimler için genel kurallar

*  bir işlev üye gövdesinin başlangıcında, *v* kesinlikle atanmaz.
*  *v* , herhangi bir ulaşılamaz deyimin başlangıcında kesinlikle atanır.
*  Diğer herhangi bir deyimin başlangıcında bulunan *v* 'nin kesin atama durumu, o deyimin başlangıcını hedefleyen tüm denetim akış aktarımları üzerinde *v* 'nin kesin atama durumu denetlenerek belirlenir. (Ve yalnızca *Bu) tüm* denetim akışı aktarımlarına açık bir şekilde atanmışsa, *v* , deyimin başlangıcında kesinlikle atanır. Olası denetim akış aktarımları kümesi, bildirime ulaşılabilirlik ([bitiş noktaları ve erişilebilirlik](statements.md#end-points-and-reachability)) gibi şekilde belirlenir.
*  Bir bloğun, `checked` ,`do` ,,`foreach`,, ,`using`,,,,,,,,,,, veya bitiş noktasındaki kesin atama durumu `lock` `unchecked` `if` `while` `for` `switch`bildirim, o deyimin bitiş noktasını hedefleyen tüm denetim akışı aktarımları üzerinde, *v* 'nin kesin atama durumunun denetlenmesi tarafından belirlenir. Bu *, her* türlü denetim akışı aktarımına açık bir şekilde atanırsa, *v* , deyimin bitiş noktasında kesin olarak atanır. Güvenmiyorsanız *v* , deyimin bitiş noktasında kesin olarak atanmaz. Olası denetim akış aktarımları kümesi, bildirime ulaşılabilirlik ([bitiş noktaları ve erişilebilirlik](statements.md#end-points-and-reachability)) gibi şekilde belirlenir.

#### <a name="block-statements-checked-and-unchecked-statements"></a>Block deyimleri, Checked ve unchecked deyimleri

Denetimdeki, blok içindeki (veya bloğun bitiş noktasındaki), blok içindeki bildirim listesinin ilk ifadesine aktarım yapılan *belirli bir atama* durumu, blok öncesi *v* 'nin kesin atama bildirimiyle aynıdır , `checked`, veya `unchecked` deyimidir.

#### <a name="expression-statements"></a>İfade deyimleri

*Expr* *ifadesi içeren bir ifade deyimi* için:

*  *v* , *ifadenin* başlangıcında, *stmt*'ın başlangıcında aynı kesin atama durumuna sahiptir.
*  Eğer, *ifadenin*sonunda kesin olarak *atanmışsa, bu* , *stmt*öğesinin son noktasında kesinlikle atanır; güvenmiyorsanız Bu, *stmt*öğesinin son noktasında kesinlikle atanmaz.

#### <a name="declaration-statements"></a>Bildirim deyimleri

*  *Stmt* , başlatıcılar olmadan bir bildirim bildirimidir, bu durumda,, *stmt*'ın sonunda, *stmt* öğesinin son noktasında aynı kesin atama *durumuna sahip olur* .
*  *Stmt* , başlatıcıları olan bir bildirim bildirimidir, bu durumda *,, bir* Başlatıcı içeren her bildirim için bir atama bildirimiyle birlikte, *v* için kesin atama durumu, bildirim).

#### <a name="if-statements"></a>If deyimleri

Şu biçimdeki `if` bir *ifade için* :
```csharp
if ( expr ) then_stmt else else_stmt
```

*  *v* , *ifadenin* başlangıcında, *stmt*'ın başlangıcında aynı kesin atama durumuna sahiptir.
*  *V* , *Expr*'nin sonuna kesinlikle atanmışsa, *then_stmt* 'e denetim akışı aktarımına ve herhangi bir else tümcesi yoksa, *else_stmt* veya *stmt* öğesinin bitiş noktasına kesin bir şekilde atanır.
*  Eğer *v* , *ifadenin*sonunda "kesin ifadeden sonra kesin olarak atandı" durumuna sahipse, kesin olarak *then_stmt*'e denetim akışı aktarımına atanır ve yalnızca else_ 'e yönelik denetim akışı aktarımına kesin olarak atanmaz.else yan tümcesi yoksa, stmt veya *stmt* öğesinin bitiş noktasına.
*  Eğer *v* , *ifadenin*sonundaki "yanlış ifadeden sonra kesin olarak atandı" durumuna sahipse, bu durumda kesin olarak *else_stmt*'e denetim akışı aktarımına atanır ve then_stmt 'e yönelik denetim akışı aktarımına kesin olarak atanmaz *then_stmt*. Kesin olarak, yalnızca *then_stmt*bitiş noktasında kesin olarak atanmışsa, bu, *stmt* öğesinin son noktasında kesinlikle atanır.
*  Aksi halde, *then_stmt* veya *else_stmt*için denetim akışı aktarımına veya else yan tümcesi yoksa, *stmt* öğesinin bitiş noktasına *kesin olarak* atanmamıştır.

#### <a name="switch-statements"></a>Switch deyimleri

Denetim ifadesi `switch` içeren *bir deyim deyiminde:*

*  *İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.
*  Denetim *akışı 'nın erişilebilir* anahtar bloğu bildirim listesine aktarımı üzerindeki kesin atama durumu, *ifadenin*sonundaki *v* 'nin kesin atama durumuyla aynıdır.

#### <a name="while-statements"></a>While deyimleri

Şu biçimdeki `while` bir *ifade için* :
```csharp
while ( expr ) while_body
```

*  *v* , *ifadenin* başlangıcında, *stmt*'ın başlangıcında aynı kesin atama durumuna sahiptir.
*  *V* , *ifadenin*sonuna kesinlikle atanmışsa, *while_body* öğesine ve *stmt*bitiş noktasına denetim akışı aktarımına kesinlikle atanır.
*  Eğer *v* , *ifadenin*sonunda "kesin ifadeden sonra kesin olarak atandı" durumuna sahipse, kesin olarak *while_body*'e denetim akışı aktarımına atanır, ancak bu, kesin olarak, *stmt*öğesinin son noktasında atanmamıştır.
*  Eğer *v* , *ifadenin*sonundaki "yanlış ifadeden sonra kesin olarak atandı" durumuna sahipse, bu durumda kesin olarak, denetim akışı aktarımına denetim akışı aktarımı üzerinde *kesin olarak*atanmamıştır.  *_gövde*.

#### <a name="do-statements"></a>Do deyimleri

Şu biçimdeki `do` bir *ifade için* :
```csharp
do do_body while ( expr ) ;
```

*  *v* *, stmt 'ın* başından itibaren, *do_body* ' den başlayarak, denetim akışı aktarımında aynı kesin atama durumuna *sahiptir.*
*  *v* , *do_body*bitiş noktasında *ifadenin* başlangıcında aynı kesin atama durumuna sahiptir.
*  *V* , *Expr*'nin sonuna kesinlikle atanmışsa, bu, bir dizi sonunda denetim akışı aktarımına kesin olarak *atanır.*
*  Eğer *v* , *ifadenin*sonunda "false ifadeden sonra kesin olarak atandı" durumuna sahipse, bu durumda kesin bir şekilde, *stmt*öğesinin bitiş noktasına bir denetim akışı aktarımına atanır.

#### <a name="for-statements"></a>For deyimleri

Formun bir `for` ifadesine yönelik kesin atama denetimi:
```csharp
for ( for_initializer ; for_condition ; for_iterator ) embedded_statement
```
deyimin yazıldığı gibi yapılır:
```csharp
{
    for_initializer ;
    while ( for_condition ) {
        embedded_statement ;
        for_iterator ;
    }
}
```

`for` Deyimden *for_condition* atlandığında, kesin atama değerlendirmesi, *for_condition* , yukarıdaki genişlemede değiştirilmiş `true` gibi devam eder.

#### <a name="break-continue-and-goto-statements"></a>Break, Continue ve goto deyimleri

`break`, ,Veya`goto` ifadesinin neden olduğu denetim akışı aktarımında bulunan *v* 'nin kesin atama durumu, bildiriminin başındaki v 'nin kesin atama durumuyla aynıdır. `continue`

#### <a name="throw-statements"></a>Throw deyimleri

Formun *bir ifade alanı* için
```csharp
throw expr ;
```

*İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.

#### <a name="return-statements"></a>Return deyimleri

Formun *bir ifade alanı* için
```csharp
return expr ;
```

*  *İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.
*  *V* bir çıkış parametresi ise, bu durumda kesinlikle atanmalıdır:
    * *ifadenin* ardından
    * ya `finally` da ifadesini`return` kapsayan - bir `try` `finally` veya bloğununsonunda`finally` . `try` - `catch` -

Şu biçimdeki bir ifade için:
```csharp
return ;
```

*  *V* bir çıkış parametresi ise, bu durumda kesinlikle atanmalıdır:
    * *stmt* Before
    * ya `finally` da ifadesini`return` kapsayan - bir `try` `finally` veya bloğununsonunda`finally` . `try` - `catch` -

#### <a name="try-catch-statements"></a>Try-catch deyimleri

Şu biçimdeki bir *ifade için* :
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
```

*  *Try_block* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.
*  *Catch_block_i* (herhangi bir *ı*için) başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.
*  Try_block 'in son noktasında tam olarak atanan *v 'nin kesin* atama durumu *, (* ve yalnızca bu durum) *v* , ve her *catch_block_i* için (1 ile n *arasında)* ).

#### <a name="try-finally-statements"></a>Try-finally deyimleri

Şu biçimdeki `try` bir *ifade için* :
```csharp
try try_block finally finally_block
```

*  *Try_block* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.
*  *Finally_block* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* 'nin kesin atama durumuyla aynıdır.
*  *Stmt* *öğesinin son* noktasında kesin atama durumu, (ve yalnızca) aşağıdakilerden en az biri doğru ise, kesin olarak atanır:
    * *v* , *try_block* 'in son noktasında kesinlikle atanır
    * *v* , *finally_block* 'in son noktasında kesinlikle atanır

*Try_block*içinde başlayan ve *try_block*dışında biten bir denetim akışı `goto` aktarımı (örneğin, bir ifade) yapılırsa *, v ise* bu denetim akışı aktarımı üzerinde de kesin olarak kabul edilir. *finally_block*bitiş noktasında kesin olarak atanır. (Bu yalnızca bir ise, bu denetim akışı aktarımındaki başka bir nedenden dolayı *v* 'nin kesin olarak atanması durumunda kesinlikle atanmamıştır.)

#### <a name="try-catch-finally-statements"></a>Try-catch-finally deyimleri

`try` Formun bir - ifadesiyleilgili- kesin atama Analizi: `catch` `finally`
```csharp
try try_block
catch(...) catch_block_1
...
catch(...) catch_block_n
finally *finally_block*
```
deyimin `try` bir `finally` ifadeyikapsayan- bir ifade olması halinde yapılır: `try` - `catch`
```csharp
try {
    try try_block
    catch(...) catch_block_1
    ...
    catch(...) catch_block_n
}
finally finally_block
```

Aşağıdaki örnek, bir `try` deyimin farklı bloklarının ([TRY ifadesinin](statements.md#the-try-statement)) kesin atamayı nasıl etkilediğini gösterir.
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

Şu biçimdeki `foreach` bir *ifade için* :
```csharp
foreach ( type identifier in expr ) embedded_statement
```

*  *İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.
*  *Embedded_statement* 'e veya denetimin bitiş noktasına aktarım sırasında bulunan *v* 'nin kesin atama *durumu,* *ifadenin*sonundaki *v* durumu ile aynıdır.

#### <a name="using-statements"></a>Using deyimleri

Şu biçimdeki `using` bir *ifade için* :
```csharp
using ( resource_acquisition ) embedded_statement
```

*  *Resource_acquisition* başlangıcında bulunan *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.
*  *Embedded_statement* 'e yönelik denetim akışı aktarımında bulunan *v* 'nin kesin atama durumu, *resource_acquisition*sonundaki *v* durumuyla aynıdır.

#### <a name="lock-statements"></a>Lock deyimleri

Şu biçimdeki `lock` bir *ifade için* :
```csharp
lock ( expr ) embedded_statement
```

*  *İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.
*  *Embedded_statement* 'e yönelik denetim akışı aktarımında bulunan *v* 'nin kesin atama durumu, *Expr*'nin sonundaki *v* durumuyla aynıdır.

#### <a name="yield-statements"></a>Yield deyimleri

Şu biçimdeki `yield return` bir *ifade için* :
```csharp
yield return expr ;
```

*  *İfadenin* başındaki *v* 'nin kesin atama durumu, *stmt*öğesinin başındaki *v* durumu ile aynıdır.
*  *Stmt* 'in sonundaki *v* 'nin kesin atama durumu, *ifadenin*sonundaki *v* durumu ile aynıdır.
*  Bir `yield break` deyimin, kesin atama durumu üzerinde hiçbir etkisi yoktur.

#### <a name="general-rules-for-simple-expressions"></a>Basit ifadeler için genel kurallar

Aşağıdaki kural bu tür ifadeler için geçerlidir: değişmez değerler ([değişmez değerler](expressions.md#literals)), basit adlar ([basit adlar](expressions.md#simple-names)), üye erişim ifadeleri ([üye erişimi](expressions.md#member-access)), dizinlenmemiş taban erişim ifadeleri ([temel erişim](expressions.md#base-access)), `typeof`ifadeler ([typeof işleci](expressions.md#the-typeof-operator)), varsayılan değer ifadeleri ([varsayılan değer ifadeleri](expressions.md#default-value-expressions)) ve `nameof` ifadeler ([NameOf ifadeleri](expressions.md#nameof-expressions)).

*  Böyle bir ifadenin sonundaki *v* 'nin kesin atama durumu, ifadenin başındaki *v* 'nin kesin atama durumuyla aynıdır.

#### <a name="general-rules-for-expressions-with-embedded-expressions"></a>Katıştırılmış ifadeleri olan ifadeler için genel kurallar

Aşağıdaki kurallar bu tür ifadeler için geçerlidir: parantez içine alınmış ifadeler ([parantez Içine alınmış ifadeler](expressions.md#parenthesized-expressions)), öğe erişim Ifadeleri ([öğe erişimi](expressions.md#element-access)), dizin oluşturma ([taban erişimi](expressions.md#base-access)), artış ve azaltma ifadeleri ([sonek artırma ve azaltma işleçleri](expressions.md#postfix-increment-and-decrement-operators), [önek artırma ve azaltma işleçleri](expressions.md#prefix-increment-and-decrement-operators)), atama ifadeleri ([atama ifadeleri](expressions.md#cast-expressions)), birli `+`, `-`, `~`, `*`ifadeler, ikili `+` `-` ,`%` ,,`>=`,, ,`>>`,,, ,`>`, `<<` `<` `<=` `*` `/` `==`, ,`is`,, ,`&`[](expressions.md#arithmetic-operators) [](expressions.md#shift-operators), ifadeler(aritmetikişleçler,`^` kaydırma işleçleri, ilişkisel ve tür-test `as` `!=` `|` [ işleçler](expressions.md#relational-and-type-testing-operators), [mantıksal işleçler](expressions.md#logical-operators)), bileşik atama ifadeleri ([bileşik](expressions.md#compound-assignment) `checked` atama) ve `unchecked` ifadeler ([denetlenen ve işaretlenmemiş işleçler](expressions.md#the-checked-and-unchecked-operators)), ayrıca dizi ve temsilci oluşturma ifadeleri ([New işleci](expressions.md#the-new-operator)).

Bu ifadelerin her birinde, sabit bir düzende koşulsuz olarak değerlendirilen bir veya daha fazla alt ifade vardır. Örneğin, ikili `%` işleci işlecin sol tarafını, sonra sağ tarafını değerlendirir. Dizin oluşturma işlemi, dizinlenmiş ifadeyi değerlendirir ve sonra her bir dizin ifadesini soldan sağa sırayla değerlendirir. Bir *ifade ifadesi için*, *E2,..., en*, bu sırayla değerlendirilen alt ifadeler:

*  *E1* 'ın başında bulunan *v* 'nin kesin atama durumu, *ifadenin*başındaki kesin atama durumuyla aynıdır.
*  *Ei* 'nin*başındaki (bir* kereden fazla) *v* 'nin kesin atama durumu, önceki alt ifadenin sonundaki kesin atama durumuyla aynıdır.
*  *İfadenin* sonundaki *v* 'nin kesin atama durumu, *en* sonunda kesin atama durumuyla aynıdır

#### <a name="invocation-expressions-and-object-creation-expressions"></a>Çağırma ifadeleri ve nesne oluşturma ifadeleri

Formun *çağırma ifadesi için* :
```csharp
primary_expression ( arg1 , arg2 , ... , argN )
```
ya da formun nesne oluşturma ifadesi:
```csharp
new type ( arg1 , arg2 , ... , argN )
```

*  Bir çağırma ifadesi için, *primary_expression* Before the *v* 'nin kesin atama durumu, *ifadenin* *Before ifadesi* ile aynı olur.
*  Bir çağırma ifadesinde, *arg1* Before the *v* 'nin kesin atama durumu *primary_expression*sonrasında *v* 'nin durumuyla aynıdır.
*  Bir nesne oluşturma ifadesinde, *arg1* Before the *v* 'nin kesin atama durumu, *ifadenin* *Before ifadesi* ile aynı olur.
*  Her bir bağımsız değişken için, *Argi* *'nin kesin* atama durumu normal ifade kuralları tarafından *belirlenir ve herhangi*bir `ref` veya `out` değiştiricilerin yok sayılıyor.
*  Birden çok *daha büyük olan her bir* bağımsız değişken için, *Argi* 'nin, önceki *bağımsız*değişkenden sonraki *v* durumuyla aynı *olması için,* *o kadar kesin* bir atama durumu.
*  Sanal değişken, bağımsız değişkenlerin herhangi bir `out` bağımsız değişkeni olarak (yani, formun `out v`bir bağımsız değişkeni) olarak geçirilse, *ifadenin* sonunda *v* durumu kesinlikle atanır. Güvenmiyorsanız *ifade* sonrasında *v* 'Nin durumu, *argN*sonrasında *v* durumu ile aynıdır.
*  Dizi başlatıcıları ([dizi oluşturma ifadeleri](expressions.md#array-creation-expressions)), nesne başlatıcıları ([nesne başlatıcıları](expressions.md#object-initializers)), koleksiyon başlatıcıları ([koleksiyon başlatıcıları](expressions.md#collection-initializers)) ve anonim nesne başlatıcıları ([anonim nesne oluşturma) için ifadeler](expressions.md#anonymous-object-creation-expressions)), kesin atama durumu bu yapıların bakımından tanımlandığı genişlemeyle belirlenir.

#### <a name="simple-assignment-expressions"></a>Basit atama ifadeleri

Formun`w = expr_rhs` *bir ifadesi için* :

*  *Expr_rhs* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.
*  *İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:
   * *W* , *v*ile aynı değişkense, *ifadenin* *sonunda o 'nun kesin* atama durumu kesin olarak atanır.
   * Aksi takdirde, atama bir yapı türünün örnek Oluşturucusu içinde oluşursa, *w* bir özellik erişimidir, yapılandırılmış örnek için bir otomatik olarak *uygulanan özellik* *P*, *daha sonra,* *ifadenin* sonunda tam atama durumu kesin olarak atanır.
   * Aksi halde, *ifadenin* sonunda *v* 'nin kesin atama durumu, *expr_rhs*sonrasında *v* 'nin kesin atama durumuyla aynıdır.

#### <a name="-conditional-and-expressions"></a>& & (koşullu ve) ifadeler

Formun`expr_first && expr_second` *bir ifadesi için* :

*  *Expr_first* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.
*  *Expr_first* sonrasında *v* 'nin durumu kesin olarak atanır veya "doğru bir ifadeden sonra kesin olarak atanır" ise, *expr_second* *'in kesin* atama durumu kesinlikle atanır. Aksi takdirde, kesinlikle atanmaz.
*  *İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:
    * *Expr_first* `false`değeri olan bir sabit ifadesiyse, *ifadenin* sonunda *v* 'nin kesin atama durumu, *expr_first*sonrasında *v* 'nin kesin atama durumuyla aynıdır.
    * Aksi halde, *expr_first* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *ifadenin* sonunda *v* durumu kesinlikle atanır.
    * Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesin olarak atandıktan ve *expr_first* sonrasında *v* 'nin durumu "kesinlikle yanlış ifadeden sonra atanır" ise, *ifadenin* sonunda *v* durumu kesinlikle olur atanan.
    * Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesinlikle atanırsa veya "doğru ifadeden sonra kesin olarak atanır" ise, *ifadenin* sonunda *v* durumu "kesinlikle doğru ifadeden sonra atanır" olur.
    * Aksi halde, *expr_first* sonrasında *v* 'nin durumu "kesinlikle yanlış ifadeden sonra atanır" ise ve *expr_second* sonrasında *v* 'nin durumu "kesinlikle yanlış ifadeden sonra atanır" ise, sonrasında *v durumu Expr* "kesinlikle false ifadeden sonra atanır".
    * Aksi halde, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.

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
değişken `i` , bir `if` deyimin gömülü deyimlerinden birinde kesin olarak kabul edilir ancak diğeri de değildir. `if` Yöntemi içindeki`F`deyiminde, ifadenin `i` yürütülmesiherzamanbueklideyiminyürütülmesindenönce,değişkenilkeklenmişifadeyekesinlikleatanır.`(i = y)` Buna karşılık, değişken `i` , yanlış bir şekilde test edilmiş olabilir ve değişken atanmakta olan değişkenin `i` atanmasından `x >= 0` kaynaklanabilir.

#### <a name="-conditional-or-expressions"></a>|| (koşullu OR) ifadeler

Formun`expr_first || expr_second` *bir ifadesi için* :

*  *Expr_first* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.
*  *Expr_first* sonrasında *v* 'nin durumu kesinlikle atanır veya "yanlış ifadeden sonra kesinlikle atanır" olduğunda, *expr_second* *'in kesin* atama durumu kesinlikle atanır. Aksi takdirde, kesinlikle atanmaz.
*  *İfadenin* sonunda, kesin atama *bildiriminin sonuna göre* belirlenir:
    * *Expr_first* `true`değeri olan bir sabit ifadesiyse, *ifadenin* sonunda *v* 'nin kesin atama durumu, *expr_first*sonrasında *v* 'nin kesin atama durumuyla aynıdır.
    * Aksi halde, *expr_first* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *ifadenin* sonunda *v* durumu kesinlikle atanır.
    * Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesin olarak atandıktan ve *expr_first* sonrasında *v* 'nin durumu "kesin ifadeden sonra açık olarak atanır" ise, *ifadenin* sonunda *v* durumu kesinlikle olur atanan.
    * Aksi halde, *expr_second* sonrasında *v* 'nin durumu kesinlikle atanmışsa veya "yanlış ifadeden sonra kesinlikle atanır" ise, *ifadenin* sonunda *v* durumu "kesinlikle yanlış ifadeden sonra atanır" olur.
    * Aksi halde, *expr_first* sonrasında *v* 'nin durumu "kesin ifadeden sonra" kesinlikle atanır "ise ve *expr_second* sonrasında *v* 'nin durumu" kesinlikle doğru *ifadeden sonra*"kesin ifadeden sonra kesinlikle atanır".
    * Aksi halde, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.

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
değişken `i` , bir `if` deyimin gömülü deyimlerinden birinde kesin olarak kabul edilir ancak diğeri de değildir. `if` Yöntemi içindeki`G`deyiminde, ifadenin `i` yürütülmesiherzamanbueklideyiminyürütülmesindenönce,değişkenikincigömülüifadeyekesinlikleatanır.`(i = y)` Buna karşılık, değişken `i` , doğru bir şekilde test edilmiş olabilir, bu `x >= 0` da değişken atanmakta olan değişkenin `i` atanmamış olması gibi, ilk ekli ifadede kesin olarak atanmaz.

#### <a name="-logical-negation-expressions"></a>! (mantıksal değilleme) ifadeleri

Formun`! expr_operand` *bir ifadesi için* :

*  *Expr_operand* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.
*  *İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:
    * \* Expr_operand * sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *Expr* 'den sonra *v* durumu kesinlikle atanır.
    * \* Expr_operand * sonrasında *v* 'nin durumu kesin olarak atanmamışsa, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.
    * \* Expr_operand * sonrasında *v* 'nin durumu "kesinlikle" yanlış ifadeden sonra atanır "ise, *ifadenin* sonunda *v* durumu" kesinlikle doğru ifadeden sonra atanır "olur.
    * \* Expr_operand * sonrasında *v* 'nin durumu "kesinlikle doğru ifadeden sonra atanır" ise, *ifadenin* sonunda *v* durumu "kesinlikle yanlış ifadeden sonra atanır" olur.

#### <a name="-null-coalescing-expressions"></a>?? (boş birleştirme) ifadeleri

Formun`expr_first ?? expr_second` *bir ifadesi için* :

*  *Expr_first* Before the *v* 'nin kesin atama durumu, *ifadenin*önüne *v* 'nin kesin atama durumuyla aynı olur.
*  *Expr_second* Before the *v* 'nin kesin atama durumu, *expr_first*sonrasında *v* 'nin kesin atama durumuyla aynıdır.
*  *İfadenin* sonunda, kesin atama *bildiriminin sonuna göre* belirlenir:
    * *Expr_first* , null değeri olan bir sabit Ifadedir ([sabit ifadeler](expressions.md#constant-expressions)), *expr_second* *sonrasında v durumu* ile *aynı olur.*
*  Aksi takdirde, *expr_first*sonrasında *v* 'nin durumu, *ifadenin* *sonunda kesin* atama durumuyla aynıdır.

#### <a name="-conditional-expressions"></a>?: (koşullu) ifadeler

Formun`expr_cond ? expr_true : expr_false` *bir ifadesi için* :

*  *Expr_cond* Before the *v* 'nin kesin atama durumu, *Expr*'den önceki *v* durumu ile aynı olur.
*  *Expr_true* önce, ve yalnızca aşağıdakilerden biri varsa, kesin olarak atanan *v* 'nin kesin atama durumu:
    * *expr_cond* değeri olan sabit bir ifadedir`false`
    * *expr_cond* sonrasında *v* 'nin durumu kesin olarak atanır veya "doğru ifadeden sonra kesinlikle atanır".
*  *Expr_false* önce, ve yalnızca aşağıdakilerden biri varsa, kesin olarak atanan *v* 'nin kesin atama durumu:
    * *expr_cond* değeri olan sabit bir ifadedir`true`
*  *expr_cond* sonrasında *v* 'nin durumu kesinlikle atanır veya "kesinlikle" yanlış ifadeden sonra atanır "olur.
*  *İfadenin* sonunda, belirli bir *v* atama durumu, tarafından belirlenir:
    * *Expr_cond* , değeri `true` olan bir sabit ifadesiyse ([sabit deyimler](expressions.md#constant-expressions)), daha sonra, *expr_true* *sonrasında v durumu* ile *aynı olur.*
    * Aksi takdirde, *expr_cond* sabit bir ifadedir ([sabit deyimler](expressions.md#constant-expressions) `false` ) ve ardından, *ifadenin* *ardından,* *expr_false*sonrasında *v* durumu ile aynıdır.
    * Aksi halde, *expr_true* sonrasında *v* 'nin durumu kesin olarak atandıktan ve *expr_false* sonrasında *v* 'nin durumu kesin olarak atandıktan sonra, *Expr* *'nin durumu* kesinlikle atanır.
    * Aksi halde, *ifadenin* sonunda *v* durumu kesinlikle atanmaz.

#### <a name="anonymous-functions"></a>Anonim işlevler

Bir *lambda_expression* veya *anonymous_method_expression* ifadesinde gövde ( *blok* ya da *ifade*) *gövdesi*için:

*  *Gövdeden* önceki bir dış değişken *v* 'nin kesin atama durumu, *ifadenin*önceki *d* durumuyla aynı olur. Diğer bir deyişle, dış değişkenlerin kesin atama durumu anonim işlev bağlamından devralınır.
*  *İfadenin* *sonrasında bir* dış değişkenin kesin atama durumu, *ifadenin*önünde *v* durumu ile aynıdır.

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
anonim işlevin bildirildiği yere kesin `max` olarak atanmadığından, derleme zamanı hatası oluşturur. Örnek
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
anonim işlevde olan atamanın `n` , anonim işlevin dışına yönelik `n` kesin atama durumu üzerinde hiçbir etkisi olmadığından, derleme zamanı hatası da oluşturur.

## <a name="variable-references"></a>Değişken başvuruları

Bir *variable_reference* , değişken olarak sınıflandırılan bir *ifadedir* . Bir *variable_reference* , geçerli değeri getirmek ve yeni bir değeri depolamak için her ikisine de erişilebilen bir depolama konumunu belirtir.

```antlr
variable_reference
    : expression
    ;
```

C ve C++içinde bir *variable_reference* , *lvalue*olarak bilinir.

## <a name="atomicity-of-variable-references"></a>Değişken başvurularının Atomicity değeri

Aşağıdaki veri türlerini `bool`okuma ve yazma işlemleri atomik:, `short` `byte` `uint`, `sbyte` `char` `ushort` ,,,,,,`int`vebaşvurutürleridir. `float` Bunlara ek olarak, önceki listede bulunan temel bir türle birlikte sabit listesi türlerinin okuma ve yazma işlemleri de atomik bir yöntemdir. `long` ,`ulong`,, Ve`decimal`gibi diğer türlerin okuma ve yazma işlemlerinin yanı sıra Kullanıcı tanımlı türlerin atomik olması garanti edilmez. `double` Bu amaçla tasarlanan kitaplık işlevleri dışında, artış veya azaltma durumunda olduğu gibi atomik okuma-değiştirme-yazma garantisi yoktur.

