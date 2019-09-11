---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876905"
---
# <a name="introduction"></a>Giriş

C#("bkz. diyez") basit, modern, nesne odaklı ve tür açısından güvenli bir programlama dilidir. C#C 'nin dil ailesinde köklerine sahiptir ve hemen C, C++ve Java programcılarına tanıdık gelecektir. C#ECMA ***-334*** standardı ve ISO/IEC tarafından ***ıso/IEC 23270*** standardı olarak standartlaştırılmış Ecma International tarafından standartlaştırılmış olur. Microsoft 'un C# .NET Framework derleyicisi, bu standartların her ikisinin de uyumlu bir uygulamasıdır.

C#, nesne odaklı bir dildir, ancak C# daha fazla ***bileşen odaklı*** programlama desteğini içerir. Modern yazılım tasarımı giderek, yazılım bileşenlerini, kendi kendine içerilen ve kendi kendine açıklayan işlev paketleri biçiminde kullanır. Bu bileşenlere anahtar, özellikler, Yöntemler ve olaylar içeren bir programlama modeli sunduklarında; Bunlar, bileşen hakkında bildirime dayalı bilgiler sağlayan özniteliklere sahiptir; ve kendi belgelerini içerirler. C#, bu kavramları doğrudan desteklemek için yazılım bileşenlerinin oluşturulması ve C# kullanılması için çok doğal bir dil sunarak dil yapıları sağlar.

Birçok C# Özellik sağlam ve dayanıklı uygulamaların yapımasına yardımcı olur: ***Çöp toplama*** , kullanılmayan nesneler tarafından kullanılan belleği otomatik olarak geri kazanır; ***özel durum işleme*** , hata algılama ve kurtarmaya yönelik yapılandırılmış ve genişletilebilir bir yaklaşım sağlar; dilin ***tür kullanımı uyumlu*** tasarımı, başlatılmamış değişkenlerden okumayı, sınırları ötesinde dizileri dizin haline getirmek veya Denetlenmemiş tür yayınları gerçekleştirmeyi olanaksız hale getirir.

C#Birleşik bir ***tür sistemine***sahiptir. `int` Ve `object` C# gibitemeltürlerdahilolmaküzeretümtürler,tekbirköktüründendevralınır.`double` Bu nedenle, tüm türler ortak işlemler kümesini paylaşır ve herhangi bir türdeki değerler tutarlı bir şekilde depolanabilir, taşınır ve çalıştırılabilir. Ayrıca, C# hem Kullanıcı tanımlı başvuru türlerini hem de değer türlerini destekler, bu da nesnelerin dinamik ayrılmasına ve basit yapıların satır içi depolamamasına olanak tanır.

Programlar ve kitaplıkların zaman içinde zaman içinde gelişebilmesini sağlamak için, tasarımına daha fazla vurgu konulmuştur. C# C# Birçok programlama dili bu sorunla ilgileniyor ve sonuç olarak, bu dillerde yazılmış programlar, bağımlı kitaplıkların daha yeni sürümleri tanıtıldığında gerekenden çok daha fazla. C#Sürüm oluşturma konuları tarafından doğrudan etkilenen tasarımın yönleri, ayrı `virtual` ve `override` değiştiriciler, yöntem aşırı yükleme çözümlemesi kuralları ve açık arabirim üye bildirimleri için destek içerir.

Bu bölümün geri kalanında C# dilin önemli özellikleri açıklanmaktadır. Sonraki bölümlerde, kuralları ve özel durumları ayrıntı yönelimli ve bazen matematik olarak anlasa da, bu bölüm, tamamlanma masrafında açıklık ve kısaltma açısından çok çaba harcar. Amaç, okuyucuya erken programların yazılmasına ve sonraki bölümlerin okunmasına yardımcı olacak dile bir giriş sunmaktır.

## <a name="hello-world"></a>Merhaba dünya

"Hello, World" programı, genellikle bir programlama dilini tanıtmak için kullanılır. Burada yer C#verilmiştir:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

C#Kaynak dosyalar genellikle dosya uzantısına `.cs`sahiptir. "Hello, World" programının dosyada `hello.cs`depolandığını varsayarak, program komut satırı kullanılarak Microsoft C# derleyicisi ile derlenebilir
```
csc hello.cs
```
adında `hello.exe`bir çalıştırılabilir derleme üreten. Bu uygulama tarafından çalıştırıldığında üretilen çıkış,
```
Hello, World
```

"Hello, World" programı, `using` `System` ad alanına başvuran bir yönergeyle başlar. Ad alanları, programları ve kitaplıkları düzenlemek C# için hiyerarşik bir yol sağlar. Ad alanları türler ve diğer ad alanlarını içerir — örneğin, `System` ad alanı programda başvurulan `Console` sınıf gibi bir dizi tür içerir ve `IO` ve `Collections`gibi diğer ad alanları vardır. Verilen `using` bir ad alanına başvuruda bulunan bir yönerge, bu ad alanının üyesi olan türlerin nitelenmemiş kullanımını mümkün değildir. Yönergesi nedeniyle program, için `System.Console.WriteLine`toplu olarak kullanabilir `Console.WriteLine`. `using`

"Hello, World" programı tarafından belirtilen `Main` sınıftekbirüyeyesahiptir,yöntemiadlı.`Hello` `Main` Yöntemi`static` değiştiriciyle birlikte bildirilmiştir. Örnek yöntemleri, anahtar sözcüğünü `this`kullanarak belirli bir kapsayan nesne örneğine başvurabilir, ancak statik yöntemler belirli bir nesneye başvuru olmadan çalışır. Kurala göre, adlı `Main` statik bir yöntem, bir programın giriş noktası olarak görev yapar.

Programın çıktısı, `WriteLine` `System` ad alanındaki `Console` sınıfının yöntemi tarafından üretilir. Bu sınıf, varsayılan olarak Microsoft C# derleyicisi tarafından otomatik olarak başvurulduğu .NET Framework sınıf kitaplıkları tarafından sağlanır. C# Kendisinin ayrı bir çalışma zamanı kitaplığı olmadığına unutmayın. Bunun yerine, .NET Framework çalışma zamanı kitaplığıdır C#.

## <a name="program-structure"></a>Program yapısı

' Deki C# temel kurumsal kavramlar, ***Programlar***, ***ad alanları***, ***türler***, ***Üyeler***ve ***derlemelerdir***. C#programlar bir veya daha fazla kaynak dosyadan oluşur. Programlar, üyeleri içeren ve ad alanları halinde düzenlenebilen türleri bildirir. Sınıflar ve arabirimler tür örnekleridir. Alanlar, Yöntemler, Özellikler ve olaylar üye örnekleridir. C# Programlar derlendiğinde, fiziksel olarak derlemeler halinde paketlenir. Derlemeler `.dll`, ***uygulama*** veya ***kitaplık***uygulanıp `.exe` uygulamadığına bağlı olarak genellikle dosya uzantısına sahiptir.

Örnek

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
adlı bir `Stack` `Acme.Collections`ad alanında adında bir sınıf bildirir. Bu sınıfın `Acme.Collections.Stack`tam adı. Sınıf birçok `top`üye içerir: adlı bir alan, `Push` ve `Pop`adlı iki yöntem ve adlı `Entry`bir iç içe sınıf. Sınıf daha fazla üç üye içerir: adlı `next`alan, adlı `data`alan ve Oluşturucu. `Entry` Örneğin kaynak kodunun dosyada `acme.cs`depolandığını varsayarak, komut satırı

```
csc /t:library acme.cs
```
örneği bir kitaplık (bir `Main` giriş noktası olmayan kod) olarak derler ve adında `acme.dll`bir derleme oluşturur.

Derlemeler, ***ara dil*** (IL) yönergeleri biçiminde çalıştırılabilir kodu ve ***meta veri***biçimindeki sembolik bilgileri içerir. Yürütülmeden önce, bir derlemedeki IL kodu, .NET ortak dil çalışma zamanının tam zamanında (JıT) derleyicisi tarafından otomatik olarak işlemciye özgü koda dönüştürülür.

Bir derleme, hem kodu hem de meta verileri içeren bir işlevsellik birimi olduğundan, içinde `#include` C#yönergeler ve başlık dosyaları gerekmez. Belirli bir derlemede yer alan ortak türler ve Üyeler, yalnızca program derlenirken bu derlemeye C# başvurarak bir programda kullanılabilir hale getirilir. Örneğin, bu program `Acme.Collections.Stack` `acme.dll` derlemeden sınıfını kullanır:

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
Program dosyada `test.cs`depolanıyorsa `test.cs` , derlendikten sonra `acme.dll` `/r` derlemeye derleyicinin seçeneği kullanılarak başvurulabilir:

```
csc /r:acme.dll test.cs
```
Bu, çalıştırıldığında bir çalıştırılabilir derleme `test.exe`oluşturur; Bu, çalıştırıldığında çıktıyı üretir:

```
100
10
1
```
C#bir programın kaynak metninin birkaç kaynak dosyada depolanmasına izin verir. Birden çok dosya C# programı derlendiğinde, tüm kaynak dosyalar birlikte işlenir ve kaynak dosyalar birbirini tamamen başvurabilir — kavramsal olarak, tüm kaynak dosyaları işlenmeden önce bir büyük dosyada birleştirilmiş olur. Bildirim sırası çok önemli olduğundan, C# bazı durumlarda ileri bildirimlere hiçbir şekilde gerek yoktur. C#kaynak dosyayı yalnızca bir ortak tür bildirmek üzere sınırlamaz veya kaynak dosyanın adının kaynak dosyada belirtilen bir türle eşleşmesi gerekir.

## <a name="types-and-variables"></a>Türler ve değişkenler

' De C#iki tür tür vardır: ***değer türleri*** ve ***başvuru türleri***. Değer türlerinin değişkenleri doğrudan verilerini içerir, ancak başvuru türündeki değişkenler verilerine başvuruları depolar, ikincisi ise nesneler olarak bilinir. Başvuru türleriyle, iki değişkenin aynı nesneye başvurması ve bu nedenle bir değişkende işlemler için diğer değişken tarafından başvurulan nesneyi etkilemesi mümkündür. Değer türleriyle, her biri kendi verilerinin kopyasına sahiptir ve bir üzerindeki işlemler, diğerini ( `ref` ve `out` parametre değişkenleri hariç) etkileyecek şekilde mümkün değildir.

C#değer türleri, ***basit türler***, ***sabit listesi türleri***, ***yapı türleri***ve ***null yapılabilir türlere***daha da bölünür ve C#başvuru türleri, ***Sınıf türlerine***, ***arabirim türlerine***, ***diziye daha fazla bölünmüştür türler***ve ***temsilci türleri***.

Aşağıdaki tabloda, tür sistemine genel bir C#bakış sağlanmaktadır.

| __Kategori__    |                 | __Açıklama__ |
|-----------------|-----------------|-----------------|
| Değer türleri     | Basit türler    | İmzalanan integral: `sbyte`, `short`, `int`,`long` |
|                 |                 | İşaretsiz integral: `byte`, `ushort`, `uint`,`ulong` |
|                 |                 | Unicode karakterler:`char` |
|                 |                 | IEEE kayan nokta: `float`,`double` |
|                 |                 | Yüksek duyarlıklı ondalık:`decimal` |
|                 |                 | Boolean`bool` |
|                 | Sabit listesi türleri      | Formun Kullanıcı tanımlı türleri`enum E {...}` |
|                 | Yapı türleri    | Formun Kullanıcı tanımlı türleri`struct S {...}` |
|                 | Boş değer atanabilir tipler  | Bir `null` değere sahip diğer tüm değer türlerinin uzantıları |
| Başvuru türleri | Sınıf türleri     | Diğer tüm türlerin Ultimate temel sınıfı:`object` |
|                 |                 | Unicode dizeleri:`string` |
|                 |                 | Formun Kullanıcı tanımlı türleri`class C {...}` |
|                 | Arabirim türleri | Formun Kullanıcı tanımlı türleri`interface I {...}` |
|                 | Dizi türleri     | Tek ve çok boyutlu, örneğin `int[]` ve`int[,]` |
|                 | Temsilci türleri  | Formun Kullanıcı tanımlı türleri örn.`delegate int  D(...)` |

Sekiz integral türü, imzalanmış veya imzasız biçimde 8 bit, 16 bit, 32-bit ve 64 bit değerleri için destek sağlar.

İki kayan nokta türü `float` ve `double`, 32-bit tek duyarlıklı ve 64-bit çift duyarlıklı IEEE 754 biçimleri kullanılarak temsil edilir.

`decimal` Türü, finansal ve parasal hesaplamalar için uygun olan 128 bitlik bir veri türüdür.

C#türü, Boole değerlerini ( `true` veya `false`olan değerler) temsil etmek için kullanılır. `bool`

İçindeki C# karakter ve dize işleme Unicode kodlaması kullanır. Tür bir UTF-16 kod birimini temsil eder `string` ve tür bir UTF-16 kod birimi dizisini temsil eder. `char`

Aşağıdaki tabloda sayısal türler C#özetlenmektedir.


| __Kategori__      | __Bitlik__ | __Tür__  | __Aralık/duyarlık__ |
|-------------------|----------|-----------|---------------------|
| İmzalanan integral   | 8        | `sbyte`   | -128... 127 |
|                   | 16       | `short`   | -32768... 32, 767 |
|                   | 32       | `int`     | -2147483648... 2, 147, 483, 647 |
|                   | 64       | `long`    | -9223372036854775808... 9, 223, 372, 036, 854, 775, 807 |
| İşaretsiz integral | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65, |
|                   | 32       | `uint`    | 0... 4, 294, 967, 295 |
|                   | 64       | `ulong`   | 0... 18, 446,, 073, 709, 551, 615 |
| Kayan nokta    | 32       | `float`   | 1,5 × 10 ^ − 45 ila 3,4 × 10 ^ 38, 7 basamaklı duyarlık |
|                   | 64       | `double`  | 5,0 × 10 ^ − 324 ila 1,7 × 10 ^ 308, 15 basamaklı duyarlık |
| Ondalık           | 128      | `decimal` | 1,0 × 10 ^ − 28 ila 7,9 × 10 ^ 28, 28 basamaklı duyarlık |

C#programlar yeni türler oluşturmak için ***tür bildirimleri*** kullanır. Tür bildiriminde yeni türün adı ve üyeleri belirtilir. C#Türlerin beş kategorisi Kullanıcı tarafından tanımlanabilir: sınıf türleri, yapı türleri, arabirim türleri, sabit listesi türleri ve temsilci türleri.

Bir sınıf türü, veri üyeleri (alanlar) ve işlev üyeleri (Yöntemler, Özellikler ve diğerleri) içeren bir veri yapısını tanımlar. Sınıf türleri, tek devralma ve çok biçimlilik destekler, türetilmiş sınıfların temel sınıfları genişletebileceği ve özelleştireceği mekanizmalar.

Yapı türü, veri üyeleri ve işlev üyeleri olan bir yapıyı temsil eden bir sınıf türüne benzerdir. Ancak, sınıfların aksine yapılar değer türlerdir ve yığın ayırmayı gerektirmez. Yapı türleri Kullanıcı tarafından belirtilen devralmayı desteklemez ve tüm yapı türleri örtülü olarak türünden `object`devralınır.

Arabirim türü, bir sözleşmeyi adlandırılmış bir ortak işlev üyeleri kümesi olarak tanımlar. Arabirim uygulayan bir sınıf veya yapı, arabirimin işlev üyelerinin uygulamalarını sağlamalıdır. Bir arabirim birden çok temel arabirimden devralınabilir ve bir sınıf ya da yapı birden çok arabirim uygulayabilir.

Bir temsilci türü, belirli bir parametre listesi ve dönüş türü olan yöntemlere yapılan başvuruları temsil eder. Temsilciler, yöntemleri değişkenlere atanabilecek ve parametre olarak geçirilen varlıklar olarak işleme olanağı tanır. Temsilciler, bazı diğer dillerde bulunan işlev işaretçileri kavramına benzerdir, ancak işlev işaretçilerinden farklı olarak Temsilciler nesne yönelimli ve tür açısından güvenlidir.

Sınıf, yapı, arabirim ve temsilci türleri tüm genel türleri destekler ve diğer türlerle parametrelenebilir.

Sabit listesi türü, adlandırılmış sabitleri olan ayrı bir türdür. Her numaralandırma türünün, sekiz integral türünden biri olması gereken temel bir türü vardır. Bir sabit listesi türünün değer kümesi, temel alınan türün değerleri kümesiyle aynıdır.

C#herhangi bir türdeki tek ve çok boyutlu dizileri destekler. Yukarıda listelenen türlerin aksine,, kullanılmadan önce dizi türlerinin bildirilmesini gerektirmez. Bunun yerine, dizi türleri Köşeli parantezlerle bir tür adı izleyerek oluşturulur. Örneğin, `int[]` tek boyutlu bir `int` `int`diziyse, `int[,]` iki boyutlu bir dizidir ve `int[][]` tek boyutlu dizilerinin tek boyutlu dizilerdir `int`.

Null yapılabilir türler, kullanılmadan önce de bildirilemez. Her null yapılamayan değer türü `T` için, karşılık gelen bir null yapılabilir tür `T?`vardır ve bu, ek bir değer `null`tutabilir. Örneğin, `int?` herhangi bir 32 bit tamsayı veya değeri `null`tutabilecek bir türdür.

C#tür sistemi, herhangi bir tür değeri bir nesne olarak işlenemeyeceği şekilde birleştirilmiştir. Her tür doğrudan C# veya dolaylı olarak `object` sınıf türünden türetilir ve `object` tüm türlerin en son temel sınıfıdır. Başvuru türlerinin değerleri, yalnızca değerleri tür `object`olarak görüntüleyerek nesne olarak değerlendirilir. Değer türlerinin değerleri, ***kutulama*** ve ***kutudan*** çıkarma işlemleri gerçekleştirerek nesneler olarak değerlendirilir. Aşağıdaki örnekte, bir `int` değeri öğesine `object` dönüştürülüp öğesine `int`yeniden döndürülür.

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
Değer türünde bir değer türüne `object`dönüştürüldüğünde, "Box" olarak da bilinen bir nesne örneği değeri tutacak şekilde ayrılır ve değer bu kutuya kopyalanır. Buna karşılık, bir `object` başvuru bir değer türüne dönüştürülzaman, başvurulan nesnenin doğru değer türünün bir kutusu olması ve denetim başarılı olursa, kutudaki değer kopyalanır.

C#öğesinin Birleşik tür sistemi etkin bir şekilde değer türlerinin "isteğe bağlı" nesneler olabileceği anlamına gelir. Birleşme nedeniyle, türü `object` kullanan genel amaçlı kitaplıklar, başvuru türleri ve değer türleriyle birlikte kullanılabilir.

İçinde C#alanlar, dizi öğeleri, yerel değişkenler ve parametreler dahil olmak üzere birkaç tür ***değişken*** vardır. Değişkenler, depolama konumlarını temsil eder ve her değişken, aşağıdaki tabloda gösterildiği gibi, değişkende hangi değerlerin depolanabileceğini belirleyen bir tür içerir.


| __Değişken türü__    | __Olası Içerikler__ |
|-------------------------|-----------------------|
| Null yapılamayan değer türü | Bu tam türden bir değer |
| Null yapılabilir değer türü     | Null değer veya bu tam tür değeri |
| `object`                | Bir null başvurusu, herhangi bir başvuru türünün nesnesine başvuru veya herhangi bir değer türünün paketlenmiş değerine başvuru |
| Sınıf türü              | Null başvurusu, bu sınıf türünün bir örneğine başvuru veya bu sınıf türünden türetilmiş bir sınıfın örneğine başvuru |
| Arabirim türü          | Null başvurusu, bu arabirim türünü uygulayan bir sınıf türü örneğine başvuru veya bu arabirim türünü uygulayan bir değer türünün paketlenmiş değerine başvuru |
| Dizi türü              | Null başvuru, bu dizi türünün bir örneğine başvuru veya uyumlu bir dizi türü örneğine başvuru |
| Temsilci türü           | Bu temsilci türünün bir örneğine null başvuru veya başvuru |

## <a name="expressions"></a>İfadeler

***İfadeler*** , ***işlenenler*** ve ***işleçlerden***oluşturulur. Bir ifadenin işleçleri, işlenenlerin hangi işlemleri uygulanacağını gösterir. `+`İşleç örnekleri `-` ,`/`,, ve`new`içerir. `*` İşlenenlerin örnekleri, sabit değerleri, alanları, yerel değişkenleri ve ifadeleri içerir.

Bir ifade birden çok işleç içerdiğinde, işleçlerin ***önceliği*** ayrı işleçlerin değerlendirilme sırasını denetler. Örneğin, `x + y * z` `*` işleç işleçten`+` daha yüksek önceliğe `x + (y * z)` sahip olduğu için ifade değerlendirilir.

Çoğu işleç ***aşırı***yüklenebilir. İşleç aşırı yüklemesi, Kullanıcı tanımlı operatör uygulamalarının bir veya her ikisinin de Kullanıcı tanımlı sınıf veya yapı türünde olduğu işlemler için belirtilmesine izin verir.

Aşağıdaki tabloda, işleç C#kategorilerinin en yüksekten en düşüğe öncelik sırasına göre listelenmesi ve işleçleri özetlenmektedir. Aynı kategorideki operatörler eşit önceliğe sahiptir.


| __Kategori__                     | __İfadesini__    | __Açıklama__ |
|----------------------------------|-------------------|-----------------|
| Birincil                          | `x.m`             | Üye erişimi |
|                                  | `x(...)`          | Yöntem ve temsilci çağırma |
|                                  | `x[...]`          | Dizi ve dizinleyici erişimi |
|                                  | `x++`             | Artırım sonrası |
|                                  | `x--`             | Azaltım sonrası |
|                                  | `new T(...)`      | Nesne ve temsilci oluşturma |
|                                  | `new T(...){...}` | Başlatıcı ile nesne oluşturma |
|                                  | `new {...}`       | Anonim nesne Başlatıcısı |
|                                  | `new T[...]`      | Dizi oluşturma |
|                                  | `typeof(T)`       | İçin `System.Type` nesne al`T` |
|                                  | `checked(x)`      | İşaretli bağlamında ifade değerlendirme |
|                                  | `unchecked(x)`    | İşaretlenmemiş bağlamında ifade değerlendirme |
|                                  | `default(T)`      | Türün varsayılan değerini Al`T` |
|                                  | `delegate {...}`  | Anonim işlevi (anonim yöntemi) |
| Li                            | `+x`              | Kimlik |
|                                  | `-x`              | Olumsuzlama |
|                                  | `!x`              | Mantıksal olumsuzlama |
|                                  | `~x`              | Bitwise olumsuzlama |
|                                  | `++x`             | Artırım öncesi |
|                                  | `--x`             | Azaltım öncesi |
|                                  | `(T)x`            | Açıkça türe `x` Dönüştür`T` |
|                                  | `await x`         | Zaman uyumsuz `x` olarak işlemin tamamlanmasını bekleyin |
| Çarpma                   | `x * y`           | Çarpma |
|                                  | `x / y`           | Bölme |
|                                  | `x % y`           | Kalan |
| Msal                         | `x + y`           | Toplama, dize bitiştirme, temsilci birleşimi |
|                                  | `x - y`           | Çıkarma, temsilci kaldırma |
| Shift                            | `x << y`          | Sola kaydırma |
|                                  | `x >> y`          | Sağa kaydırma |
| İlişkisel ve tür testi      | `x < y`           | Küçüktür |
|                                  | `x > y`           | Büyüktür |
|                                  | `x <= y`          | Küçük veya eşittir |
|                                  | `x >= y`          | Büyük veya eşittir |
|                                  | `x is T`          | Bir `true` ise`T`, tersidurumda`false` döndürün `x` |
|                                  | `x as T`          | Dönüş `x` olarak `T`yazılmış veya `null` `x``T` |
| Eşitlik                         | `x == y`          | Eşittir      |
|                                  | `x != y`          | Eşit değildir |
| Mantıksal VE                      | `x & y`           | Tamsayı bit düzeyinde ve, Boolean mantıksal ve |
| Mantıksal XOR                      | `x ^ y`           | Tamsayı bitwise XOR, Boolean mantıksal XOR |
| Mantıksal VEYA                       | <code>x &#124; y</code> | Tamsayı bitwise VEYA, boolean mantıksal VEYA |
| Koşullu VE                  | `x && y`          | Yalnızca `y` şu durumlarda `x` değerlendirilir`true` |
| Koşullu VEYA                   | <code>x &#124;&#124; y</code> | Yalnızca `y` şu durumlarda `x` değerlendirilir`false` |
| Null birleşim                  | `x ?? y`          | `y` ,Değilse`x`olarakdeğerlendirilir `null` `x` |
| Koşullu                      | `x ? y : z`       | Olupolmadığını`x` değerlendirir `y` `true` `z` `x``false` |
| Atama veya anonim işlev | `x = y`           | Atama |
|                                  | `x op= y`         | Bileşik atama; Desteklenen işleçler şunlardır `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code> |
|                                  | `(T x) => y`      | Anonim işlevi (lambda ifadesi) |

## <a name="statements"></a>Deyimler

Bir programın eylemleri ***deyimler***kullanılarak ifade edilir. C#, birden çok farklı türde deyimi destekler, bu sayı katıştırılmış deyimler bakımından tanımlanır.

Bir ***blok*** , tek bir ifadeye izin verilen bağlamlarda birden çok deyimin yazılmasına izin verir. Bir blok, sınırlayıcılar `{` ve `}`arasında yazılmış deyimler listesinden oluşur.

***Bildirim deyimleri*** yerel değişkenleri ve sabitleri bildirmek için kullanılır.

***İfade deyimleri*** , ifadeleri değerlendirmek için kullanılır. Deyim olarak kullanılabilecek ifadeler, yöntem etkinleştirmeleri, `new` işleci kullanılarak nesne ayırmaları, `=` ve bileşik atama işleçleri kullanan atamalar, artırma ve azaltma işlemleri `++` ve`--` işleçleri ve await ifadeleri.

***Seçim deyimleri*** , bazı deyimlerin değerine göre yürütme için bir dizi olası deyimden birini seçmek için kullanılır. Bu grupta `if` ve `switch` deyimleri vardır.

***Yineleme deyimleri*** , gömülü bir deyimi tekrar tekrar yürütmek için kullanılır. `while`Bu grupta `do` ,,ve`foreach`deyimleribulunur. `for`

***Sıçrama deyimleri*** , denetimi aktarmak için kullanılır. Bu `break`grupta `continue`, ,,`throw`, ve deyimleribulunur`yield` . `goto` `return`

`try`... Bildiri, bir bloğun yürütülmesi sırasında oluşan özel durumları yakalamak için kullanılır `try`ve... `catch` `finally` bildirim, her zaman yürütülen ve özel durumun gerçekleşmediğini belirten sonlandırma kodunu belirtmek için kullanılır.

`checked` Ve`unchecked` deyimleri, tamsayı türü aritmetik işlemler ve dönüştürmeler için taşma denetimi bağlamını denetlemek için kullanılır.

Bu `lock` ifade, belirli bir nesne için karşılıklı dışlama kilidini almak, bir ifadeyi yürütmek ve sonra kilidi serbest bırakmak için kullanılır.

`using` İfade, kaynak almak, bir ifadeyi yürütmek ve ardından bu kaynağı atmak için kullanılır.

Aşağıda her bir tür deyimin örnekleri verilmiştir

__Yerel değişken bildirimleri__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Yerel sabit bildirimi__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__İfade deyimi__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if`Ekstre__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch`Ekstre__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while`Ekstre__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do`Ekstre__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for`Ekstre__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach`Ekstre__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break`Ekstre__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue`Ekstre__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto`Ekstre__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return`Ekstre__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield`Ekstre__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw`ve `try` deyimleri__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked`ve `unchecked` deyimleri__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

__`lock`Ekstre__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using`Ekstre__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a>Sınıflar ve nesneler

***Sınıflar*** , türlerin en temel C#larıdır. Bir sınıf, durumu (alanları) ve eylemleri (Yöntemler ve diğer işlev üyelerini) tek bir birimde birleştiren bir veri yapısıdır. Sınıf, ***nesne***olarak da bilinen, sınıfının dinamik olarak oluşturulan ***örnekleri*** için bir tanım sağlar. Sınıflar, ***Devralma*** ve çok ***biçimlilik***desteği, ***türetilmiş sınıfların*** ***temel sınıfları***genişletebileceği ve özelleştirilebilecek mekanizmalar.

Yeni sınıflar sınıf bildirimleri kullanılarak oluşturulur. Sınıf bildirimi, sınıfın özniteliklerini ve değiştiricilerini, sınıfın adını, Taban sınıfını (belirtilmişse) ve sınıf tarafından uygulanan arabirimleri belirten bir üstbilgiyle başlar. Üst bilgi, sınırlayıcılar `{` ve `}`arasında yazılmış üye bildirimlerinin listesinden oluşan sınıf gövdesinden gelir.

Aşağıda adlı `Point`basit bir sınıfın bildirimi verilmiştir:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Sınıf örnekleri, yeni bir örnek için `new` bellek ayıran işleç kullanılarak oluşturulur, örneği başlatmak için bir oluşturucu çağırır ve örneğe bir başvuru döndürür. Aşağıdaki deyimler iki `Point` nesne oluşturur ve bu nesnelere başvuruları iki değişken halinde depolar:

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Nesne artık kullanımda olmadığında bir nesnenin kapladığı bellek otomatik olarak geri kazanılır. Üzerinde C#nesneleri açıkça serbest bırakmak gerekli değildir veya mümkün değildir.

### <a name="members"></a>Üyeler

Bir sınıfın üyeleri ***statik Üyeler*** veya ***örnek üyeleridir***. Statik Üyeler sınıflara aittir ve örnek üyeleri nesnelere aittir (sınıf örnekleri).

Aşağıdaki tabloda bir sınıfın içerebileceği üye türlerine genel bakış sunulmaktadır.


| __Üyesidir__   | __Açıklama__ |
|------------  |-----------------|
| Sabitler    | Sınıfla ilişkili sabit değerler |
| Alanlar       | Sınıfın değişkenleri |
| Yöntemler      | Sınıfı tarafından gerçekleştirilebilecek hesaplamalar ve eylemler |
| Özellikler   | Sınıfın adlandırılmış özelliklerini okuma ve yazma ile ilişkili eylemler |
| Dizin Oluşturucular     | Bir dizi gibi sınıfın dizin oluşturma örnekleri ile ilişkili eylemler |
| Olaylar       | Sınıfı tarafından oluşturulabilecek bildirimler |
| İşleçler    | Sınıf tarafından desteklenen dönüşümler ve ifade işleçleri |
| Oluşturucular | Sınıfın veya sınıfın örneklerinin örneğini başlatmak için gereken eylemler |
| Yıkıcılar  | Sınıfın örneklerinden önce gerçekleştirilecek eylemler kalıcı olarak atılır |
| Türler        | Sınıf tarafından tanımlanan iç içe türler |

### <a name="accessibility"></a>Erişilebilirlik

Bir sınıfın her üyesinin ilişkili bir erişilebilirliği vardır ve bu, üyeye erişebilen program metni bölgelerini denetler. Olası beş erişilebilirlik biçimi vardır. Bunlar aşağıdaki tabloda özetlenmiştir.


| __Erişilebilirlik__    | __Anlamına__ |
|----------------------|-----------------|
| `public`             | Erişim sınırlı değil |
| `protected`          | Bu sınıftan türetilmiş bu sınıfla veya sınıflarla sınırlı erişim |
| `internal`           | Bu programla sınırlı erişim |
| `protected internal` | Bu sınıftan türetilmiş bu program veya sınıflarla sınırlı erişim |
| `private`            | Bu sınıfla sınırlı erişim |

### <a name="type-parameters"></a>Tür parametreleri

Sınıf tanımı, tür parametre adlarının bir listesini kapsayan açılı ayraçları olan sınıf adını izleyerek bir tür parametreleri kümesi belirtebilir. Tür parametreleri, sınıfının üyelerini tanımlamak için sınıf bildirimlerinin gövdesinde kullanılabilir. Aşağıdaki örnekte, öğesinin `Pair` `TFirst` tür parametreleri ve `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Tür parametrelerini almak için belirtilen bir sınıf türüne genel sınıf türü denir. Yapı, arabirim ve temsilci türleri de genel olabilir.

Genel sınıf kullanıldığında, tür parametrelerinin her biri için tür bağımsız değişkenlerinin sağlanması gerekir:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Yukarıda olduğu gibi `Pair<int,string>` , tür bağımsız değişkenlerine sahip genel bir tür, oluşturulmuş bir tür olarak adlandırılır.

### <a name="base-classes"></a>Temel sınıflar

Sınıf bildirimi, sınıf adı ve tür parametreleri iki nokta ve temel sınıfın adı ile birlikte bir temel sınıf belirtebilir. Temel sınıf belirtiminin atlanması, türden `object`türetmeye benzer. `Point3D` Aşağıdaki örnekte, öğesinin `Point`temel sınıfı ve öğesinin `Point` `object`temel sınıfı:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
Bir sınıf, temel sınıfının üyelerini devralır. Devralma, bir sınıfın örnek ve statik oluşturucular ve temel sınıfın yıkıcıları dışında, temel sınıfının tüm üyelerini örtük olarak içerdiği anlamına gelir. Türetilmiş bir sınıf, devralananlara yeni üyeler ekleyebilir, ancak devralınmış bir üyenin tanımını kaldıramaz. Önceki örnekte `Point3D` , `Point3D` `x` `z`ve alanlarını`y` öğesinden`Point`devralır ve her örnek üç alan`y`içerir,, ve. `x`

Bir sınıf türünden, temel sınıf türlerinden herhangi birine örtük bir dönüştürme vardır. Bu nedenle, bir sınıf türünün değişkeni bu sınıfın bir örneğine veya türetilmiş herhangi bir sınıfın örneğine başvurabilir. Örneğin, önceki sınıf bildirimleri verildiğinde, türünde `Point` bir değişken bir `Point` veya a `Point3D`başvurabilir:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Alanlar

Alan, bir sınıf ile veya bir sınıf örneğiyle ilişkili bir değişkendir.

`static` Değiştiriciyle belirtilen bir alan ***statik bir alan***tanımlar. Statik alan tam olarak bir depolama konumunu tanımlar. Bir sınıfın kaç örneğinin oluşturulduğuna bakılmaksızın, bir statik alanın yalnızca bir kopyası vardır.

`static` Değiştirici olmadan belirtilen bir alan bir ***örnek alanını***tanımlar. Bir sınıfın her örneği, bu sınıfın tüm örnek alanlarının ayrı bir kopyasını içerir.

Aşağıdaki örnekte `Color` , sınıfının her örneği `r`, `g`, ve `b` `Black`örnek `Red`alanlarının ayrı bir kopyasına sahiptir ancak `White` ,,,,,,,,,,,,,,,,,,,`Green` ve`Blue` statik alanları:

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
Önceki örnekte gösterildiği gibi, ***salt okuma alanları*** bir `readonly` değiştirici ile bildirilebilecek. Bir `readonly` alana atama yalnızca alanın bildiriminin veya aynı sınıftaki bir oluşturucunun parçası olarak gerçekleşebilir.

### <a name="methods"></a>Yöntemler

Bir ***Yöntem*** , bir nesne veya sınıf tarafından gerçekleştirilebilecek bir hesaplama veya eylem uygulayan bir üyesidir. ***Statik yöntemlere*** sınıfı aracılığıyla erişilir. ***Örnek yöntemlerine*** , sınıfının örnekleri aracılığıyla erişilir.

Metotlarda (muhtemelen boş) bir ***parametre***listesi vardır. Bu, yönteme geçirilen değerleri ya da değişken başvurularını temsil eder ve bir ***dönüş türü***, hesaplanan ve yöntemi tarafından döndürülen değer türünü belirtir. Bir yöntemin dönüş türü `void` bir değer döndürmezse.

Türler gibi yöntemler de bir tür parametreleri kümesine sahip olabilir, bu da yöntem çağrıldığında tür bağımsız değişkenlerinin belirtilmesi gerekir. Türlerin aksine, tür bağımsız değişkenleri genellikle yöntem çağrısının bağımsız değişkenlerinden çıkarsanamıyor ve açıkça verilmemelidir.

Yöntemin ***imzası*** , yöntemin bildirildiği sınıfta benzersiz olmalıdır. Bir yöntemin imzası yöntemin adından, tür parametrelerinin sayısına ve parametrelerinin sayısına, değiştiricilerine ve türlerine sahiptir. Bir yöntemin imzası, dönüş türünü içermez.

#### <a name="parameters"></a>Parametreler

Parametreler, değerlere veya değişken başvurularını yöntemlere geçirmek için kullanılır. Bir yöntemin parametreleri, yöntemi çağrıldığında belirtilen ***bağımsız değişkenlerden*** gerçek değerlerini alır. Dört tür parametre vardır: değer parametreleri, başvuru parametreleri, çıkış parametreleri ve parametre dizileri.

Giriş parametresi geçirme için bir ***değer parametresi*** kullanılır. Değer parametresi, parametresi için geçirilen bağımsız değişkenden ilk değerini alan yerel bir değişkene karşılık gelir. Değer parametresindeki değişiklikler, parametresi için geçirilen bağımsız değişkeni etkilemez.

Değer parametreleri, ilgili bağımsız değişkenlerin atlanabilmesi için varsayılan bir değer belirtilerek isteğe bağlı olabilir.

Hem giriş hem de çıkış parametresi geçirme için ***başvuru parametresi*** kullanılır. Başvuru parametresi için geçirilen bağımsız değişken bir değişken olmalıdır ve yöntemin yürütülmesi sırasında başvuru parametresi, bağımsız değişken değişkeniyle aynı depolama konumunu temsil eder. Bir başvuru parametresi `ref` değiştiriciyle birlikte bildirilmiştir. Aşağıdaki örnek `ref` parametrelerin kullanımını gösterir.

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
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
Çıkış parametresi geçirme için ***çıkış parametresi*** kullanılır. Bir çıktı parametresi, çağıran tarafından belirtilen bağımsız değişkenin ilk değeri önemli olmayan bir başvuru parametresine benzerdir. Bir çıkış parametresi `out` değiştiriciyle birlikte bildirilmiştir. Aşağıdaki örnek `out` parametrelerin kullanımını gösterir.

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
Bir ***parametre dizisi*** , bir metoda değişken sayıda bağımsız değişken geçirilmesine izin verir. Bir parametre dizisi `params` değiştiriciyle birlikte bildirilmiştir. Bir yöntemin yalnızca son parametresi bir parametre dizisi olabilir ve bir parametre dizisinin türü tek boyutlu bir dizi türü olmalıdır. Sınıfının ve `WriteLine` yöntemleri `Write` , parametre dizisi kullanımının iyi örnekleridir. `System.Console` Bunlar aşağıdaki gibi bildirilmiştir.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Bir parametre dizisi kullanan bir yöntem içinde, parametre dizisi tam olarak bir dizi türünün normal parametresine benzer şekilde davranır. Ancak, bir parametre dizisi olan bir yöntem çağrısında, parametre dizisi türünün tek bir bağımsız değişkenini veya parametre dizisinin öğe türünün herhangi bir sayıda bağımsız değişkenini geçirmek mümkündür. İkinci durumda, bir dizi örneği otomatik olarak oluşturulur ve verilen bağımsız değişkenlerle başlatılır. Bu örnek

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
, aşağıdaki yazma ile eşdeğerdir.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Yöntem gövdesi ve yerel değişkenler

Yöntemin gövdesi, yöntemi çağrıldığında yürütülecek deyimleri belirtir.

Yöntem gövdesi, yöntemi çağrısına özgü değişkenleri bildirebilir. Bu tür değişkenlere ***yerel değişkenler***denir. Yerel bir değişken bildirimi bir tür adı, değişken adı ve muhtemelen bir başlangıç değeri belirtir. Aşağıdaki örnek, başlangıç değeri sıfır olan `i` yerel bir değişken ve ilk değeri olmayan bir yerel değişken `j` bildirir.

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
C#değeri alınabilmesi için önce bir yerel değişkenin ***kesinlikle atanmasını*** gerektirir. Örneğin, önceki `i` bildiriminde bir başlangıç değeri yoksa, derleyici programın sonraki `i` kullanımları için bir hata bildirir çünkü `i` bu, programda bu noktalarda kesinlikle atanmamalıdır.

Bir yöntem, çağrı `return` yapana denetim döndürmek için deyimlerini kullanabilir. `void` Döndürülen`return` bir yöntemde deyimler bir ifade belirtemez. Olmayan`void`deyimler döndüren bir yöntemde, `return` dönüş değerini hesaplayan bir ifade içermelidir.

#### <a name="static-and-instance-methods"></a>Statik ve örnek yöntemleri

`static` Değiştirici ile belirtilen bir yöntem ***statik bir yöntemdir***. Statik bir yöntem belirli bir örnek üzerinde çalışmaz ve yalnızca statik üyelere doğrudan erişebilir.

`static` Değiştirici olmadan bildirildiği bir yöntem bir ***örnek yöntemidir***. Örnek yöntemi, belirli bir örnek üzerinde çalışır ve hem statik hem de örnek üyelerine erişebilir. Örnek yönteminin çağrıldığı örnek, olarak `this`açıkça erişilebilir. Statik bir yöntemde başvurmak `this` için bir hatadır.

Aşağıdaki `Entity` sınıfta hem statik hem de örnek üyeleri vardır.

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
Her `Entity` örnek bir seri numarası içerir (ve burada görünmeyen bazı diğer bilgileri kabul etmez). `Entity` Oluşturucu (bir örnek yöntemi gibi) yeni örneği bir sonraki kullanılabilir seri numarasıyla başlatır. Oluşturucu bir örnek üyesi olduğundan, hem `serialNo` örnek alanına `nextSerialNo` hem de statik alana erişim izni verilir.

`GetNextSerialNo` Ve `serialNo` statik yöntemler statikalanaerişebilir,ancakörnekalanınadoğrudanerişmesiiçinbirhataolabilir.`nextSerialNo` `SetNextSerialNo`

Aşağıdaki örnek `Entity` sınıfının kullanımını gösterir.

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
Sınıfının örneklerinde `GetSerialNo` örnek `SetNextSerialNo` yöntemi `GetNextSerialNo` çağrıldığında, ve statik yöntemlerin sınıfında çağrılabileceğini unutmayın.

#### <a name="virtual-override-and-abstract-methods"></a>Sanal, geçersiz kılma ve soyut yöntemler

Bir örnek yöntemi bildirimi bir `virtual` değiştirici içerdiğinde, yöntemi bir ***sanal yöntem***olarak kabul edilir. Değiştirici yoksa, yöntemi ***sanal olmayan bir yöntem***olarak kabul edilir. `virtual`

Bir sanal yöntem çağrıldığında, çağrının gerçekleştiği örneğin ***çalışma zamanı türü*** , çağrılacak gerçek Yöntem uygulamasını belirler. Sanal olmayan bir yöntem çağrısında, örneğin ***derleme zamanı türü*** belirleme faktörü olur.

Bir sanal yöntem, türetilmiş bir sınıfta ***geçersiz kılınabilir*** . Bir örnek yöntemi bildirimi bir `override` değiştirici içerdiğinde, yöntemi aynı imzaya sahip devralınmış bir sanal yöntemi geçersiz kılar. Sanal bir yöntem bildiriminde yeni bir yöntem tanıtıldığı halde, bir geçersiz kılma yöntemi bildirimi, bu yöntemin yeni bir uygulamasını sağlayarak, var olan bir devralınmış sanal yöntemi uzmanlık eder.

***Soyut*** bir yöntem, uygulama içermeyen bir sanal yöntemdir. Soyut bir yöntem `abstract` değiştiriciyle birlikte bildirilmiştir ve yalnızca da belirtilen `abstract`bir sınıfta izin verilir. Soyut olmayan her türetilmiş sınıfta bir soyut yöntem geçersiz kılınmalıdır.

Aşağıdaki örnek, bir ifade ağaç düğümünü temsil `Expression`eden bir soyut sınıfı ve sabitler, değişken için ifade ağacı düğümleri uygulayan `Constant`üç `VariableReference`türetilmiş sınıfı `Operation`,, ve ve ' ı tanımlar. başvurular ve aritmetik işlemler. (Bu şuna benzerdir, ancak [ifade ağacı türlerinde](types.md#expression-tree-types)tanıtılan ifade ağacı türleriyle karıştırılmamalıdır).

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
Önceki dört sınıf aritmetik ifadeleri modellemek için kullanılabilir. Örneğin, bu sınıfların örneklerini kullanarak, ifadesi `x + 3` aşağıdaki gibi gösterilebilir.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
`double` Bir `Evaluate` Örneğinyöntemi,verilenifadeyideğerlendirmekvebirdeğerüretmekiçinçağrılır.`Expression` Yöntemi, değişken adlarını (girdilerin anahtarları `Hashtable` olarak) ve değerlerini (girdilerin değerleri olarak) içeren bir bağımsız değişken olarak alır. `Evaluate` Yöntemi, bir sanal soyut yöntemidir, yani soyut olmayan türetilmiş sınıfların gerçek bir uygulama sağlamak için onu geçersiz kılması gerekir.

' Nin uygulanması, `Evaluate` yalnızca saklı sabiti döndürür. `Constant` Bir `VariableReference`uygulama, Hashtable 'daki değişken adını arar ve elde edilen değeri döndürür. Bir `Operation`uygulama ilk olarak sol ve sağ işlenenleri değerlendirir ( `Evaluate` yöntemlerini özyinelemeli olarak çağırarak) ve ardından verilen aritmetik işlemi gerçekleştirir.

Aşağıdaki program, `Expression` `x` ve `x * (y + 2)` farklı`y`değerleri için ifadeyi değerlendirmek için sınıflarını kullanır.

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a>Yöntem aşırı yüklemesi

Yöntem ***aşırı yüklemesi*** , aynı sınıftaki birden çok metodun benzersiz imzalara sahip oldukları sürece aynı ada sahip olmasını sağlar. Aşırı yüklenmiş bir yöntemin çağrılması derlenirken, derleyici çağrılacak özel yöntemi belirlemekte ***aşırı yükleme çözümü*** kullanır. Aşırı yükleme çözümlemesi, bağımsız değişkenlerle en iyi eşleşen bir yöntemi bulur veya tek bir en iyi eşleşme bulunamazsa hata bildiriyor. Aşağıdaki örnekte, etkin olan aşırı yükleme çözümü gösterilmektedir. `Main` Yöntemi içindeki her çağrının yorumu aslında hangi yöntemin çağrılacağını gösterir.

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
Örnekte gösterildiği gibi belirli bir yöntem her zaman bağımsız değişkenleri tam parametre türlerine açıkça atayarak ve/veya açıkça tür bağımsız değişkenleri sunarak seçilebilir.

### <a name="other-function-members"></a>Diğer işlev üyeleri

Yürütülebilir kod içeren Üyeler topluca bir sınıfın ***işlev üyeleri*** olarak bilinir. Yukarıdaki bölümde, işlev üyelerinin birincil türü olan yöntemler açıklanmıştır. Bu bölümde, tarafından C#desteklenen diğer işlev üyesi türleri açıklanmaktadır: oluşturucular, özellikler, Dizin oluşturucular, olaylar, işleçler ve Yıkıcılar.

Aşağıdaki kod, bir nesne growable listesini uygulayan `List<T>`adlı bir genel sınıfı gösterir. Sınıfı, en yaygın işlev üyesi türlerine birkaç örnek içerir.


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a>Oluşturucular

C#hem örnek hem de statik oluşturucuları destekler. ***Örnek Oluşturucu*** , bir sınıfın örneğini başlatmak için gereken eylemleri uygulayan bir üyedir. ***Statik Oluşturucu*** , ilk yüklendiği zaman bir sınıfın kendisini başlatmak için gereken eylemleri uygulayan bir üyesidir.

Bir Oluşturucu, dönüş türü olmayan bir yöntem ve kapsayan sınıfla aynı adı ile birlikte bildirilmiştir. Bir Oluşturucu bildiriminde `static` bir değiştirici varsa, bir statik oluşturucu bildirir. Aksi takdirde, bir örnek Oluşturucu bildirir.

Örnek oluşturucular aşırı yüklenebilir. Örneğin, `List<T>` sınıfı, biri `int` parametresi olmayan iki örnek Oluşturucu bildirir ve bir parametre alır. Örnek oluşturucular `new` işleci kullanılarak çağrılır. Aşağıdaki deyimler, `List<string>` `List` sınıfının oluşturucularının her birini kullanarak iki örnek ayırır.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Diğer üyelerin aksine, örnek oluşturucular devralınmaz ve bir sınıfın sınıfta tanımlananlardan farklı örnek oluşturucuları yoktur. Bir sınıf için örnek Oluşturucu sağlanmazsa, parametresi olmayan boş bir değer otomatik olarak sağlanır.

#### <a name="properties"></a>Özellikler

***Özellikler*** , alanlar için doğal bir uzantıdır. Her ikisi de ilişkili türlerin bulunduğu isimlerdir ve alanlara ve özelliklere erişim için sözdizimi aynıdır. Ancak, alanların aksine, Özellikler depolama konumlarını göstermiyor. Bunun yerine, özellikler, değerleri okunmak veya yazıldığında yürütülecek deyimleri belirten ***erişimcileri*** vardır.

Bir `get` özellik, bildirim bir erişimci ile sona erene ve/ `set` veya sınırlayıcılar `{` arasında `}` yazılmış bir erişimciyi ve noktalı virgül ile sona ermek yerine bir alan olarak belirtilir. Hem `get` `set` erişimci `get`hem de erişimciyesahipbirözellikokuma-yazmaözelliğidir,yalnızcabirerişimcisiolanbirözelliksaltokunurdurveyalnızcaerişimcisiolanbirözellik`set` ***salt yazılır özelliği***.

`get` Erişimci, özellik türünün dönüş değeri olan parametresiz bir yönteme karşılık gelir. Atama hedefi dışında, bir ifadede bir özelliğe başvurulduğunda, `get` özelliğin erişimcisi özelliğin değerini hesaplamak için çağrılır.

Erişimci, adlandırılmış `value` ve dönüş türü olmayan tek parametreli bir yönteme karşılık gelir. `set` Bir özelliğe bir atamanın hedefi veya işleneni `++` `--`olarak başvurulduğunda, `set` erişimci yeni değeri sağlayan bir bağımsız değişkenle çağrılır.

Sınıfı iki özellik bildirir `Capacity`ve sırasıyla salt okunurdur ve okuma-yazma olur. `Count` `List<T>` Aşağıda bu özelliklerin kullanılmasına bir örnek verilmiştir.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Alanlar ve yöntemlere benzer şekilde hem C# örnek özelliklerini hem de statik özellikleri destekler. Statik özellikler `static` değiştiriciyle, örnek özellikleri ise bu olmadan tanımlanır.

Bir özelliğin erişimcisi sanal olabilir. Bir özellik bildirimi `virtual`, `abstract`, veya `override` değiştiricisini içerdiğinde, özelliğin erişimcilerle geçerli olur.

#### <a name="indexers"></a>Dizin Oluşturucular

***Dizin Oluşturucu*** , nesnelerin diziyle aynı şekilde dizinlenmesini sağlayan bir üyedir. Bir Dizin Oluşturucu, üyenin `this` adının ardından sınırlayıcılar `[` ve `]`arasında yazılmış bir parametre listesi gelmesi dışında bir özellik gibi bildirilmiştir. Parametreler, dizin oluşturucunun erişimcisinde kullanılabilir. Özelliklere benzer şekilde, Dizin oluşturucular okunabilir-yazılır, salt okunurdur ve salt yazılır olabilir ve bir dizin oluşturucunun erişimcisi sanal olabilir.

Sınıfı `List` , bir `int` parametresi alan tek bir okuma-yazma Dizin Oluşturucu bildirir. Dizin Oluşturucu, `List` `int` örneklerin değerleriyle dizin oluşturmanızı mümkün kılar. Örneğin:

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
Dizin oluşturucular aşırı yüklenebilir, yani parametrelerinin sayısı veya türleri farklı olduğu sürece bir sınıfın birden çok dizin kümesini bildirebileceği anlamına gelir.

#### <a name="events"></a>Olaylar

Bir ***olay*** , bir sınıf veya nesnenin bildirimler sağlamasını sağlayan bir üyedir. Bildirimin bir `event` anahtar sözcük içermesi ve türün bir temsilci türü olması dışında bir olay, bir alan gibi bildirilmiştir.

Olay üyesini bildiren bir sınıf içinde, olay bir temsilci türünün alanı gibi davranır (olay soyut değildir ve erişimcileri bildirmez). Bu alan, olaya eklenmiş olan olay işleyicilerini temsil eden bir temsilciye bir başvuru depolar. Hiçbir olay tanıtıcısı yoksa, alanı olur `null`.

Sınıfı, adlı `Changed`tek bir olay üyesini bildirir ve bu, listeye yeni bir öğe eklendiğini gösterir. `List<T>` Olay sanal yöntemi tarafından tetiklenir, bu, önce olayın `null` (hiçbir işleyicinin mevcut olmadığı anlamına gelir) olup olmadığını denetler. `OnChanged` `Changed` Bir olayı oluşturma kavramı, olayın gösterdiği temsilciyi çağırmak için tam olarak eşdeğerdir. bu nedenle, olayları yükseltmek için özel dil yapıları yoktur.

İstemciler ***olay işleyicileri***aracılığıyla olaylara tepki verir. Olay işleyicileri `+=` işleci kullanılarak eklenir ve `-=` işleci kullanılarak kaldırılır. Aşağıdaki örnek, olayına bir `Changed` `List<string>`olay işleyicisi ekler.

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
Bir olayın temeldeki depolamanın denetiminin istendiği Gelişmiş senaryolarda, bir olay bildirimi açıkça bir özelliğin `add` `set` erişenine benzer bir şekilde sağlayabilir ve `remove` erişimciler sağlar.

#### <a name="operators"></a>İşleçler

***İşleci*** , bir sınıfın örneklerine belirli bir ifade işlecini uygulamanın anlamını tanımlayan bir üyesidir. Üç tür işleç tanımlanabilir: Birli İşleçler, ikili işleçler ve dönüştürme işleçleri. Tüm işleçler ve `public` `static`olarak bildirilmelidir.

Sınıfı iki `List` işleç `operator==` bildirirvebunedenlebuişleçleriörneklereuygulayandeyimlereyenianlamıverir.`operator!=` `List<T>` Özellikle, işleçler, içerilen nesnelerin her birini `List<T>` `Equals` yöntemlerini kullanarak karşılaştıran iki örneğin eşitliğini tanımlar. Aşağıdaki örnek, iki `==` `List<int>` örneği karşılaştırmak için işlecini kullanır.

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

Bu iki `Console.WriteLine` liste `True` aynı sırada aynı değerleri taşıyan aynı sayıda nesne içerdiğinden ilk çıktılar. `List<T>` Tanımlı değil,`Console.WriteLine` ilkiçıktıyı`False` içeriyor ve`b` farklı örneklere`List<int>` başvuracaktır. `a` `operator==`

#### <a name="destructors"></a>Yıkıcılar

***Yıkıcı*** , bir sınıf örneğinin yapısını kaldırma için gereken eylemleri uygulayan bir üyesidir. Yok ediciler parametrelere sahip olamaz, erişilebilirlik değiştiricilerine sahip olamaz ve açıkça çağrılamaz. Örnek için yıkıcısı çöp toplama sırasında otomatik olarak çağrılır.

Çöp toplayıcısına, nesnelerin toplanması ve yıkıcıları çalıştırma konusunda karar verirken geniş bir enlem vardır. Özellikle, yıkıcı etkinleştirmeleri zamanlaması belirleyici değildir ve herhangi bir iş parçacığında yok ediciler çalıştırılabilir. Bu ve diğer nedenlerden dolayı sınıfların yalnızca başka hiçbir çözüm uygulanabilir olmadığında Yıkıcılar uygulaması gerekir.

İfade `using` , nesne yok etme için daha iyi bir yaklaşım sağlar.

## <a name="structs"></a>Yapılar

Sınıflar gibi ***yapılar*** , veri üyeleri ve işlev üyeleri içerebilen veri yapılarıdır, ancak sınıfların aksine, yapılar değer türlerdir ve yığın ayırmayı gerektirmez. Yapı türünün bir değişkeni yapının verisini doğrudan depolarken, sınıf türündeki bir değişken, dinamik olarak ayrılan bir nesneye bir başvuru depolar. Yapı türleri Kullanıcı tarafından belirtilen devralmayı desteklemez ve tüm yapı türleri örtülü olarak türünden `object`devralınır.

Yapılar, özellikle değer semantiği olan küçük veri yapıları için yararlıdır. Karmaşık sayılar, bir koordinat sistemindeki işaret veya bir sözlükte anahtar-değer çiftleri, yapı birimlerinin iyi örnekleridir. Küçük veri yapıları için sınıflar yerine yapıların kullanımı, bir uygulamanın gerçekleştirdiği bellek ayırma sayısında büyük bir farklılık yapabilir. Örneğin, aşağıdaki program 100 noktadan oluşan bir dizi oluşturur ve başlatır. Sınıf `Point` olarak uygulanan 101 ayrı nesneler, biri Array için ve her biri 100 öğeleri için oluşturulur.

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
Alternatif olarak bir yapı oluşturma `Point` .

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Şimdi yalnızca bir nesne örneği oluşturulur — dizi için bir tane ve `Point` örnekler dizide satır içinde depolanır.

Struct oluşturucuları `new` işleçle çağrılır, ancak bu belleğin ayrılmakta olduğunu göstermez. Bir nesne dinamik olarak tahsis etmek ve ona bir başvuru döndürmek yerine, bir struct Oluşturucusu yalnızca struct değerinin kendisini döndürür (genellikle yığındaki geçici bir konumda) ve bu değer, gereken şekilde kopyalanır.

Sınıflar ile, iki değişkenin aynı nesneye başvurması ve bu nedenle bir değişkende işlemler için diğer değişken tarafından başvurulan nesneyi etkilemesi mümkündür. Yapılar ile, her birinin kendi verilerinin bir kopyasına sahip olduğu ve bir üzerindeki işlemlerin diğerini etkilemesi mümkün değildir. Örneğin, aşağıdaki kod parçası tarafından üretilen çıkış, bir sınıf veya yapı olmasına `Point` bağlıdır.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Bir sınıfınise, `a` çıkış olur `20` ve `b` aynı nesneye başvurur. `Point` `10` `a` `a.x`Bir struct ise çıktı, öğesinin`b` atamanın değerin bir kopyasını oluşturması ve bu kopyanın sonraki atamasından etkilenmemelerdir. `Point`

Önceki örnek, yapıların sınırlamalarını iki vurgulamaktadır. İlk olarak, bir yapının tamamını kopyalamak bir nesne başvurusunu kopyalamaya kıyasla genellikle daha etkilidir, bu nedenle atama ve değer parametresi, yapı ile başvuru türleriyle daha pahalı olabilir. İkincisi, `ref` ve parametreleri dışında `out` , yapılara başvurular oluşturmak mümkün değildir ve bu sayede kullanımları bir dizi durumda kurallar oluşturulur.

## <a name="arrays"></a>Diziler

***Dizi*** , hesaplanan dizinler üzerinden erişilen çeşitli değişkenler içeren bir veri yapısıdır. Bir dizide bulunan değişkenler, dizi ***öğeleri*** de denir, hepsi aynı türdür ve bu tür dizinin ***öğe türü*** olarak adlandırılır.

Dizi türleri başvuru türlerdir ve bir dizi değişkeninin bildirimi, bir dizi örneğine yönelik bir başvuru için yalnızca alan ayırın. Gerçek dizi örnekleri işleci kullanılarak, `new` çalışma zamanında dinamik olarak oluşturulur. İşlem, daha sonra örneğin ömrü boyunca düzeltilen yeni dizi örneğinin uzunluğunu belirtir. `new` Bir dizi öğelerinin ' den `0` `Length - 1`' a kadar olan dizinleri. İşleci, bir dizinin öğelerini otomatik olarak varsayılan değerlerine başlatır. Bu, örneğin, tüm sayısal türler ve `null` tüm başvuru türleri için sıfırdır. `new`

Aşağıdaki örnek, bir dizi `int` öğe oluşturur, diziyi başlatır ve dizinin içeriğini yazdırır.

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
Bu örnek, ***tek boyutlu bir dizi***üzerinde oluşturur ve çalışır. C#, ***çok boyutlu dizileri***de destekler. Dizi türünün ***derecesi*** olarak da bilinen bir dizi türünün boyut sayısı, dizi türünün köşeli ayraçları arasına yazılan virgüllerin sayısıdır. Aşağıdaki örnek tek boyutlu, iki boyutlu ve üç boyutlu bir diziyi ayırır.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
Dizi 10 öğe içeriyorsa `a2` , dizi 50 (10 × 5 `a3` ) öğesi içerir ve dizi 100 (10 × 5 × 2) öğesi içerir. `a1`

Bir dizinin öğe türü, bir dizi türü de dahil olmak üzere herhangi bir tür olabilir. Dizi türündeki öğeleri içeren bir dizi, bazen öğe dizilerinin uzunluklarının tümünün aynı olması gerektiğinden ***pürüzlü dizi*** olarak adlandırılır. Aşağıdaki örnek dizi dizilerini `int`ayırır:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
İlk satır, her biri türü `int[]` ve her biri bir başlangıç `null`değeri olan üç öğe içeren bir dizi oluşturur. Sonraki satırlar, Farklı uzunluklardaki ayrı dizi örneklerine yapılan başvurularla üç öğeyi başlatır.

İşleci, dizi öğelerinin başlangıç değerlerinin, sınırlayıcılar `{` ve `}`arasında yazılan ifadelerin listesi olan bir ***dizi başlatıcısı***kullanılarak belirtilmesine izin verir. `new` Aşağıdaki örnek, üç öğesi ile bir `int[]` ayırır ve başlatır.

```csharp
int[] a = new int[] {1, 2, 3};
```
Dizi uzunluğunun ve `{` `}`arasındaki ifade sayısından çıkarsandığına unutmayın. Yerel değişken ve alan bildirimleri daha fazla kısaltılarak, dizi türünün yeniden oluşturulması gerekmez.

```csharp
int[] a = {1, 2, 3};
```
Yukarıdaki örneklerin her ikisi de aşağıdaki gibi eşdeğerdir:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Arabirimler

***Arabirim*** , sınıflar ve yapılar tarafından uygulanabilecek bir sözleşmeyi tanımlar. Arabirim, Yöntemler, özellikler, olaylar ve Dizin oluşturucular içerebilir. Arabirim, tanımladığı üyelerin uygulamalarını sağlamaz; yalnızca arabirimini uygulayan sınıflar veya yapılar tarafından sağlanması gereken üyeleri belirtir.

Arabirimler ***birden fazla devralma***kullanabilir. Aşağıdaki örnekte, arabirimi `IComboBox` `ITextBox` hem hem de öğesinden devralır. `IListBox`

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
Sınıflar ve yapılar birden çok arabirim uygulayabilir. Aşağıdaki örnekte, sınıfı `EditBox` hem hem `IDataBound`de `IControl` uygular.

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
Bir sınıf veya yapı belirli bir arabirimi uygularsa, bu sınıfın veya yapının örnekleri örtülü olarak bu arabirim türüne dönüştürülebilir. Örneğin:

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
Bir örneğin belirli bir arabirimi uygulamak üzere statik olarak bilinmediği durumlarda dinamik tür atamaları kullanılabilir. Örneğin, aşağıdaki deyimler bir nesnenin `IControl` ve `IDataBound` arabirim uygulamalarının elde edileceği dinamik tür yayınlarını kullanır. Nesnenin gerçek türü olduğundan `EditBox`, yayınlar başarılı olur.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
Önceki `EditBox` sınıfta `IControl` , arabiriminden `public` yöntemi ve arabiriminden`IDataBound` yöntemi, Üyeler kullanılarak uygulanır. `Bind` `Paint` C#Ayrıca, sınıf veya yapının üyeleri `public`yapmaktan kaçınmasına engel olabilecek ***Açık arabirim üye uygulamalarını***destekler. Açık arabirim üyesi uygulama, tam nitelikli arabirim üye adı kullanılarak yazılır. Örneğin, `EditBox` sınıfı aşağıdaki gibi açık arabirim üye uygulamalarını `IDataBound.Bind` kullanarak `IControl.Paint` ve yöntemlerini uygulayabilir.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Açık arabirim üyelerine yalnızca arabirim türü aracılığıyla erişilebilir. Örneğin, önceki `EditBox` sınıf tarafından sağlanmış `IControl.Paint` olan uygulama, yalnızca öncelikle `IControl` arabirim türüne `EditBox` başvuru dönüştürülürken çağrılabilir.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Numaralandırmalar

***Sabit listesi türü*** , adlandırılmış sabitler kümesi olan ayrı bir değer türüdür. Aşağıdaki örnek `Color` ,,, ve `Red` `Blue`üç sabit değeri `Green`olan adlı bir sabit listesi türü bildirir ve kullanır.

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
Her sabit listesi türünün, enum türünün ***temel alınan türü*** olarak adlandırılan karşılık gelen bir integral türü vardır. Temel alınan bir türü açıkça bildirmiyor bir sabit listesi türü, temel alınan `int`bir tür içerir. Sabit listesi türünün depolama biçimi ve olası değerlerinin aralığı, temel alınan türüne göre belirlenir. Bir numaralandırma türünün üzerinde sürebelirlenen değerler kümesi, sabit listesi üyeleri tarafından sınırlı değildir. Özellikle, bir sabit listesinin temel alınan türünün herhangi bir değeri, sabit listesi türüne dönüşebilir ve bu sabit listesi türünün ayrı bir geçerli değeridir.

Aşağıdaki örnek, temel alınan `Alignment` `sbyte`türü ile adlı bir sabit listesi türü bildirir.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Önceki örnekte gösterildiği gibi, enum üye bildirimi üyenin değerini belirten bir sabit ifade içerebilir. Her sabit listesi üyesinin sabit değeri, sabit listesinin temel alınan türü aralığında olmalıdır. Sabit listesi üyesi bildirimi açıkça bir değer belirtmezse, üyeye sıfır değeri verilir (sabit listesi türünde ilk üye ise) veya metin içeriğini eklemek önceki enum üyesinin değeri ve bir.

Sabit listesi değerleri tamsayı değerlerine dönüştürülebilir ve tür atamaları kullanılarak tam tersi olabilir. Örneğin:

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Herhangi bir sabit listesi türünün varsayılan değeri, enum türüne dönüştürülen tam sayı değeridir. Değişkenlerin otomatik olarak varsayılan bir değere başlatıldığı durumlarda, bu değer enum türü değişkenlerine verilen değerdir. Bir sabit listesi türünün varsayılan değerinin kolayca kullanılabilmesi için, değişmez `0` değer örtülü olarak herhangi bir numaralandırma türüne dönüştürülür. Bu nedenle, aşağıdakilere izin verilir.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Temsilciler

Bir ***temsilci türü*** , belirli bir parametre listesi ve dönüş türü olan yöntemlere yapılan başvuruları temsil eder. Temsilciler, yöntemleri değişkenlere atanabilecek ve parametre olarak geçirilen varlıklar olarak işleme olanağı tanır. Temsilciler, bazı diğer dillerde bulunan işlev işaretçileri kavramına benzerdir, ancak işlev işaretçilerinden farklı olarak Temsilciler nesne yönelimli ve tür açısından güvenlidir.

Aşağıdaki örnek adlı `Function`bir temsilci türü bildirir ve kullanır.

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
Temsilci türünün bir örneği `Function` , `double` bağımsız değişken alan ve bir `double` değer döndüren herhangi bir yönteme başvurabilir. Yöntemi, bir `Function` `double[]` öğesine verilen öğeleri uygular ve sonuçları ile döndürür. `double[]` `Apply` Yönteminde, `Apply` bir`double[]`öğesine üç farklı işlev uygulamak için kullanılır. `Main`

Bir temsilci, statik bir yönteme (örn. veya `Square` `Math.Sin` önceki örnekte olduğu gibi) ya da bir örnek yöntemine (örneğin, önceki `m.Multiply` örnekte olduğu gibi) başvurabilir. Örnek yönteme başvuran bir temsilci belirli bir nesneye de başvurur ve örnek yöntemi temsilci aracılığıyla çağrıldığında, bu nesne çağrı içinde olur `this` .

Temsilciler, anında oluşturulan "satır içi Yöntemler" olan anonim işlevler kullanılarak da oluşturulabilir. Anonim işlevler, çevresindeki yöntemlerin yerel değişkenlerini görebilir. Bu nedenle, yukarıdaki çarpan örneği bir `Multiplier` sınıf kullanılmadan daha kolay yazılabilir:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Bir temsilcinin ilgi çekici ve yararlı bir özelliği, başvurduğu yöntemin sınıfını bilmez veya ilgilenmez; her önemli şey, başvurulan yöntemin aynı parametrelere sahip olması ve temsilciyle aynı türde dönüş türüdür.

## <a name="attributes"></a>Öznitelikler

Bir C# programdaki türler, Üyeler ve diğer varlıklar, davranışlarının belirli yönlerini denetleyen değiştiricilerin özelliklerini destekler. Örneğin, bir `public`yöntemin erişilebilirliği `internal`, `protected`,, ve `private` değiştiricileri kullanılarak denetlenir. C#Bu özelliği, bildirim temelli bilgilerin Kullanıcı tanımlı türleri program varlıklarına iliştirilebilecek ve çalışma zamanında alınabilecek şekilde genelleştirir. Programlar, ***öznitelikleri***tanımlayarak ve kullanarak bu ek bildirime dayalı bilgileri belirtir.

Aşağıdaki örnek, ilişkili belgelerinin `HelpAttribute` bağlantılarını sağlamak için program varlıklarına yerleştirilebilecek bir özniteliği bildirir.

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
Tüm öznitelik sınıfları, .NET Framework tarafından `System.Attribute` belirtilen temel sınıftan türetilir. Öznitelikler, ilişkili bildirimden hemen önceki köşeli parantez içinde, adı ve bağımsız değişkenlerle birlikte uygulanabilir. Bir özniteliğin adı içinde `Attribute`biterse, özniteliğe başvuruluyorsa adın bu bölümü atlanabilir. Örneğin, `HelpAttribute` özniteliği aşağıdaki gibi kullanılabilir.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
Bu örnek, sınıfına `HelpAttribute` `Widget` öğesine ve başka `HelpAttribute` `Display` bir sınıftaki yöntemine bir iliştirir. Öznitelik sınıfının ortak oluşturucuları, özniteliği bir program varlığına eklendiğinde sağlanması gereken bilgileri denetler. Öznitelik sınıfının genel okuma-yazma özelliklerine (daha önce `Topic` özelliğine başvuru gibi) başvurarak ek bilgiler sunulabilir.

Aşağıdaki örnek, belirli bir program varlığı için öznitelik bilgilerinin yansıma kullanarak çalışma zamanında nasıl alınamayacağını gösterir.

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
Belirli bir öznitelik yansıma aracılığıyla istendiğinde, öznitelik sınıfı için Oluşturucu program kaynağında sağlanan bilgilerle çağrılır ve sonuçta elde edilen öznitelik örneği döndürülür. Özellikler aracılığıyla ek bilgiler sağlanmışsa, öznitelik örneği döndürülmeden önce bu özellikler verilen değerlere ayarlanır.
