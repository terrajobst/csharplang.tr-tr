# <a name="introduction"></a>Giriş

C# (okunur "Bkz Sharp") bir basit, modern, nesne yönelimli ve tür kullanımı uyumlu programlama dilidir. C# dilleri C ailesinde kendi kökleri sahiptir ve Java C ve C++ programcıları için hemen tanıdık gelecektir. C# Standart ECMA uluslararası tarafından ***ECMA 334*** standart ve ISO/IEC ***ISO/IEC 23270*** standart. Microsoft'un C# derleyicisi .NET Framework için hem bu standartlarla uyumlu bir uygulamadır.

C#, nesne yönelimli bir dildir ancak C# daha fazla için destek içerir ***bileşen odaklı*** programlama. Çağdaş yazılım tasarımı giderek daha fazla işlevsellik müstakil ve kendiliğinden açıklayıcı paketleri biçiminde yazılım bileşenlerini kullanır. Bu bileşenler özellikleri, yöntemleri ve olayları ile bunların bir programlama modeli sunar, anahtarıdır; bileşen hakkında tanımlayıcı bilgiler sağlayan özniteliklerine sahip oldukları; ve bunlar kendi belgelerinin dahil edilip derecelendirilir. C# doğrudan bu kavramlar desteklemek için dil yapıları C# yapmak çok doğal dili oluşturma ve yazılım bileşenlerini kullanma sağlar.

Güçlü ve dayanıklı uygulamalar oluşturma, birkaç C# özelliklerini yardımcı: ***çöp toplama*** otomatik olarak; kullanılmayan nesnelerin kapladığı belleği geri kazanır ***özel durum işleme*** hata algılama ve kurtarma; yapılandırılmış ve Genişletilebilir bir yaklaşım sunan ve ***tür kullanımı uyumlu*** dilinin tasarım yapar, mümkün olmayan öğesinden başlatılmamış okumak Dizin dizilerine kendi sınırları ötesinde veya işaretsiz tür atamaları gerçekleştirmek için değişkenleri.

C# sahip bir ***tür sistemi birleşik***. Gibi ilkel türler dahil olmak üzere tüm C# türü, `int` ve `double`, tek bir kök devral `object` türü. Bu nedenle, bir dizi ortak işlemlerini tüm türleri paylaşın ve herhangi bir tür değerleri depolanan, taşınan ve tutarlı bir şekilde üzerinde işlem. Ayrıca, C# hem kullanıcı tarafından tanımlanan başvuru türleri ve değer türleri, nesnelerin dinamik ayırması yanı sıra basit yapılar satır içi depolanmasını sağlayan destekler.

C# programları ve kitaplıkları zaman uyumlu bir şekilde geliştirebilirsiniz emin olmak için çok Vurgu üzerinde yerleştirilen ***sürüm*** C# ' ın tasarım. Birçok programlama dili bu sorun için çok az dikkat edin ve sonuç olarak, bu dillerin sonu bağımlı kitaplıkları gerekli olduğunda daha yeni sürümleri daha sık yazılan programlar sunulmuştur. Sürüm değerlendirmeleri tarafından doğrudan etkileyen C# ' ın tasarım yönlerini dahil ayrı `virtual` ve `override` değiştiriciler, yöntem aşırı yükleme çözümlemesi için kurallar ve açık arabirim üyesi bildirimi için destek.

Bu bölümün geri kalanında, C# dilinin temel özelliklerini açıklar. Sonraki bölümlerde ayrıntılı odaklı ve bazen matematiksel bir şekilde kurallar ve özel durumlar açıklayan olsa da, bu bölümde açıklık ve bütünlük çoğaltamaz kısaltma üstlenmeye çalışır. Amaç, erken programlar yazılmasını ve sonraki bölümlerde okunmasını kolaylaştırmak dil için bir tanıtım okuyucu sağlamaktır.

## <a name="hello-world"></a>Merhaba Dünya

"Hello, World" programı, geleneksel bir programlama dili tanıtmak için kullanılır. Bunu C# ' de şu şekildedir:

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

C# kaynak dosyaları, genellikle dosya uzantısına sahip `.cs`. "Hello, World" program dosyasında depolanan varsayarak `hello.cs`, programın komut satırı kullanılarak Microsoft C# derleyicisi ile derlenebilir
```
csc hello.cs
```
adlı yürütülebilir bir derleme üreten `hello.exe`. Çalıştırıldığında, bu uygulama tarafından üretilen çıkış
```
Hello, World
```

"Hello, World" programı ile başlayan bir `using` başvuran yönergesi `System` ad alanı. Ad alanlarında, C# programları ve kitaplıkları düzenleme, hiyerarşik bir yöntem sağlar. Ad alanları, türler ve diğer ad alanları içerir — örneğin, `System` ad alanı içeren bir dizi türleri gibi `Console` program ve diğer ad alanları, bir sayı gibi başvurulan sınıfı `IO` ve `Collections`. A `using` belirtilen bir ad alanı başvuruyor yönergesi bu ad alanının üyeleri olan türlerinin nitelenmemiş kullanımı sağlar. Nedeniyle `using` yönerge, programın kullanabilirsiniz `Console.WriteLine` için kısayol olarak `System.Console.WriteLine`.

`Hello` "Hello, World" program tarafından bildirilen adlı yöntem tek bir üye sınıfında `Main`. `Main` Yöntemi ile bildirilir `static` değiştiricisi. Örnek yöntemleri anahtar sözcüğü kullanılarak belirli kapsayan nesne örneği başvurabilirsiniz ancak `this`, belirli bir nesneye başvuru olmadan statik yöntemleri çalışır. Kural gereği, adlı statik bir yöntemi `Main` bir programın giriş noktası olarak görev yapar.

Program çıktısı tarafından üretilen `WriteLine` yöntemi `Console` sınıfını `System` ad alanı. Bu sınıf, varsayılan olarak, Microsoft C# Derleyici tarafından otomatik olarak başvurulur, .NET Framework sınıf kitaplıkları tarafından sağlanır. C# kendisini'nın ayrı bir çalışma zamanı kitaplığı olmadığını unutmayın. Bunun yerine, .NET Framework çalışma zamanı kitaplığı, C# ' dir.

## <a name="program-structure"></a>Program yapısı

C# anahtar kuruluş kavramlar ***programlar***, ***ad alanları***, ***türleri***, ***üyeleri***, ve ***derlemeleri***. C# programları, bir veya daha fazla kaynak dosyadan oluşur. Programları üyeleri içerir ve ad alanları olarak düzenlenmiş türleri bildirin. Sınıflar ve arabirimler türleri örnekleridir. Alanlar, yöntemler, özellikler ve olaylar üyeleri örnekleridir. C# programları derlendiğinde, bunlar derlemelerine fiziksel olarak paketlenir. Derlemeler genellikle dosya uzantısına sahip `.exe` veya `.dll`uyguladıkları mi bağlı olarak ***uygulamaları*** veya ***kitaplıkları***.

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
adlı bir sınıfı bildirir `Stack` adlı bir ad alanında `Acme.Collections`. Bu sınıfın tam adı `Acme.Collections.Stack`. Sınıf bazı üyeler içerir: adlı bir alan `top`, adlı iki yöntem `Push` ve `Pop`ve bir iç içe geçmiş sınıf adlı `Entry`. `Entry` Sınıfı daha da içeren üç üye: adlı bir alan `next`, adında bir alan `data`ve bir oluşturucu. Örnek kaynak kodu dosyasında depolanan varsayarak `acme.cs`, komut satırı

```
csc /t:library acme.cs
```
Örnek bir kitaplık olarak derler (olmadan kod bir `Main` giriş noktası) adlı bir derleme oluşturur `acme.dll`.

Derlemeleri içeren yürütülebilir kod biçiminde ***Ara dil*** (IL) yönergeleri ve simgesel bilgiler biçiminde ***meta verileri***. Çalıştırılmadan önce bir derlemede IL kodu işlemciye özgü kodu .NET ortak dil çalışma zamanı tam zamanında (JIT) derleyici tarafından otomatik olarak dönüştürülür.

Bir derleme kendiliğinden açıklayıcı bir işlevsellik hem kod hem de meta verileri içeren birimi olduğundan için gerek yoktur `#include` yönergeleri ve C# üst bilgi dosyaları. Genel türler ve üyeler belirli bir derlemede bulunan program derlerken bu derlemeye yalnızca başvurarak bir C# programı içinde kullanılabilir hale getirilir. Örneğin, bu programın kullandığı `Acme.Collections.Stack` gelen sınıfı `acme.dll` derleme:

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
Program dosyada saklanıyorsa `test.cs`, `test.cs` derlenir, `acme.dll` kullanarak derleyicinin derleme başvurulabilir `/r` seçeneği:

```
csc /r:acme.dll test.cs
```
Bu adında bir yürütülebilir bir derleme oluşturur `test.exe`, çalıştırdığınızda, çıktıyı üretir:

```
100
10
1
```
C# kaynak metni bir programın çeşitli kaynak dosyalarında depolanan izin verir. Çok dosyalı C# programı derlenir, tüm kaynak dosyaları birlikte işlenir ve kaynak dosyaları serbestçe birbirlerine başvurabilir — kavramsal olarak, tüm kaynak dosyalarını tek bir büyük dosyaya işlenmeden önce art arda eklenmiş gibi öyledir. Çok az sayıda özel durum dışında bildirim sırasında Önemsiz olduğundan, iletme bildirimleri hiçbir zaman C# ' de gereklidir. C#, yalnızca bir genel türü bildirmek için bir kaynak dosyası sınırlamaz veya kaynak dosyada bildirilen bir tür ile eşleştirilecek kaynak dosyasının adı gerektirir.

## <a name="types-and-variables"></a>Türler ve değişkenler

C# türlerinde iki tür vardır: ***değer türleri*** ve ***başvuru türleri***. Başvuru türlerinin değişkenleri başvuruları kendi verilerine depolamak ise, değer türlerinin değişkenleri kendi verilerini doğrudan içerir, ikincisi nesneler olarak bilinen. Başvuru türleri ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur. Değer türleri ile her değişkenleri kendi veri kopyasını sahip ve işlemlerin bir diğerini etkilemesi olanaklı değildir (dışındaki durumunda `ref` ve `out` parametresi değişkenleri).

C# ' ın değer türleri içinde daha bölünür ***basit türler***, ***Numaralandırma türleri***, ***yapı türleri***, ve ***boş değer atanabilir türler***ve C# başvurusu türleri içine daha bölünür ***sınıf türleri***, ***arabirim türleri***, ***dizisi, türlerini***, ve ***temsilci türleri***.

Aşağıdaki tablo, C# tür sistemi genel bir bakış sağlar.

| __Kategori__    |                 | __Açıklama__ |
|-----------------|-----------------|-----------------|
| Değer türleri     | Basit türler    | İmzalı tam sayı: `sbyte`, `short`, `int`, `long` |
|                 |                 | İşaretsiz integral: `byte`, `ushort`, `uint`, `ulong` |
|                 |                 | Unicode karakter sayısı: `char` |
|                 |                 | IEEE kayan nokta: `float`, `double` |
|                 |                 | Yüksek duyarlıklı ondalık: `decimal` |
|                 |                 | Boole: `bool` |
|                 | Numaralandırma türleri      | Formun kullanıcı tanımlı türler `enum E {...}` |
|                 | Yapı türleri    | Formun kullanıcı tanımlı türler `struct S {...}` |
|                 | Boş değer atanabilir tipler  | Diğer tüm değer türleri ile uzantıları bir `null` değeri |
| Başvuru türleri | Sınıf türleri     | Diğer tüm türlerin Ultimate temel sınıf: `object` |
|                 |                 | Unicode dizelerini: `string` |
|                 |                 | Formun kullanıcı tanımlı türler `class C {...}` |
|                 | Arabirim türleri | Formun kullanıcı tanımlı türler `interface I {...}` |
|                 | Dizi türleri     | Tek ve örneğin, çok boyutlu `int[]` ve `int[,]` |
|                 | Temsilci türleri  | Kullanıcı tanımlı türler formun örn: `delegate int  D(...)` |

Sekiz integral türleri, 8-bit, 16-bit, 32-bit ve 64-bit işaretli veya işaretsiz form değerleri için destek sağlar.

İki kayan noktası türleri `float` ve `double`, 32-bit tek duyarlıklı ve 64-bit çift duyarlıklı IEEE 754 biçimleri kullanılarak temsil edilir.

`decimal` Türüdür finansal ve parasal hesaplamalar için uygun bir 128-bit veri türü.

C# `bool` türü boolean değerlerini temsil edecek şekilde kullanılır — ya da değerler `true` veya `false`.

Karakter ve dize işleme C# dilinde Unicode kodlaması kullanır. `char` Türünü bir UTF-16 kod birimini temsil eder ve `string` türü UTF-16 kod birimlerini dizisini temsil eder.

Aşağıdaki tablo, C# ' ın sayısal türleri özetlenmektedir.


| __Kategori__      | __BITS__ | __Türü__  | __Aralık/duyarlık__ |
|-------------------|----------|-----------|---------------------|
| İmzalı tam sayı   | 8        | `sbyte`   | -128... 127 |
|                   | 16       | `short`   | -32, 768... 32, 767 girişini |
|                   | 32       | `int`     | -2,147,483, 648... 2, 147, 483, 647 |
|                   | 64       | `long`    | -9,223,372,036,854,775, 808... 9, 223, 372, 036, 854, 775, 807 |
| İşaretsiz tamsayı | 8        | `byte`    | 0... 255 |
|                   | 16       | `ushort`  | 0... 65.535 |
|                   | 32       | `uint`    | 0... 4.294.967.295'e |
|                   | 64       | `ulong`   | 0... 18,446,744,073,709,551,615 |
| Kayan nokta    | 32       | `float`   | 1.5 × 10 ^ −45 3.4 × 10 ^ 7 basamaklı duyarlık 38, |
|                   | 64       | `double`  | 5.0 × 10 ^ −324 1.7 × 10 ^ 308, 15 basamaklı duyarlık |
| Ondalık           | 128      | `decimal` | 1.0 × 10 ^ −28 7,9 × 10 ^ 28, 28 basamaklı duyarlık |

C# programları kullanım ***tür bildirimleri*** yeni türler oluşturmak için. Bir tür bildirimi, adı ve yeni türün üyeleri belirtir. C# türleri kategorilerini beş kullanıcı tarafından tanımlanabilir: Sınıf türleri, yapı türleri, arabirim türleri, sabit listesi türleri ve temsilci türleri.

Bir sınıf türü, veri üyelerine (alanları) ve işlev üyeleri (yöntemler, özellikler ve diğerleri) içeren bir veri yapısı tanımlar. Sınıf türleri tek devralma ve çok biçimlilik, yapabildiği türetilmiş sınıfları genişletmek ve temel sınıflar specialize mekanizmaları destekler.

Bir yapı türü, veri üyeleri ve işlev üyeleri bir yapıya temsil ettiği, bir sınıf türüne benzerdir. Ancak, farklı sınıflar, yapılar değer türleri ve yığın ayırma gerektirmez. Yapı türleri, kullanıcı tarafından belirtilen devralma desteklemez ve tüm yapı türleri örtülü olarak tür devralmasına `object`.

Bir arabirim türü genel işlev üyeleri adlandırılmış bir dizi bir sözleşmeyi tanımlar. Bir sınıf ya da bir arabirimi uygulayan yapı arabirimin işlev üyeleri uygulamaları sağlamanız gerekir. Birden fazla temel Ara birimden arabirim devralabilir ve bir sınıf veya yapı birden fazla arabirim uygulayabilir.

Bir temsilci türü, belirli bir parametre listesi ve dönüş türü olan yöntemlere başvuruları temsil eder. Temsilciler, yöntemleri değişkenine atanır ve parametre olarak geçirilen varlıklar olarak değerlendirmek mümkün kılar. Diğer dillerde bulunan işlev işaretçileri kavramı temsilcileri benzerdir ancak işlev işaretçileri, nesne yönelimli ve tür kullanımı uyumlu temsilciler.

Diğer türlerle bunlar yapabildiği parametreleştirilebilir tüm destek genel türler, sınıf, yapı, arabirim ve temsilci türleri.

Bir sabit listesi türünde, adlandırılmış sabitler ile farklı bir türdür. Her sabit listesi türü sekiz integral türlerinden biri olması gereken bir temel türü vardır. Bir numaralandırma türünün değerleri kümesi, temel alınan tür değerleri kümesi ile aynıdır.

C# tek ve çoklu dimensional dizilerini herhangi bir türde destekler. Yukarıda listelenen türlerinin aksine, dizi türleri kullanılabilmesi için önce bildirilmiş gerekmez. Bunun yerine, dizi türleri bir tür adı köşeli ayraç içine uygulayarak oluşturulur. Örneğin, `int[]` , tek boyutlu bir dizidir `int`, `int[,]` iki boyutlu bir dizidir `int`, ve `int[][]` tek boyutlu dizi tek boyutlu bir dizidir `int`.

Boş değer atanabilir türler de kullanılabilmesi için önce bildirilmiş gerekmez. Her değer atanamayan değer türü `T` karşılık gelen null yapılabilir bir tür yoktur `T?`, ek bir değeri tutabilir `null`. Örneğin, `int?` herhangi bir 32 bit tamsayı veya değeri bir arada tutan bir türdür `null`.

C# tür sistemi, herhangi bir türde bir değer bir nesne olarak davranılıp şekilde birleşiktir. C# ' de her tür doğrudan veya dolaylı olarak türetir `object` sınıf türü, ve `object` tüm türlerin ultimate temel sınıftır. Başvuru türlerindeki değerleri nesneler olarak değer türü olarak yalnızca görüntüleyerek edilir `object`. Değer türlerinin değerleri nesneler olarak gerçekleştirerek edilir ***kutulama*** ve ***kutudan çıkarma*** operations. Aşağıdaki örnekte, bir `int` değerinin `object` ve yeniden geri `int`.

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
Ne zaman bir değer türünün bir değer türüne dönüştürülür `object`değerini tutacak bir "kutu," olarak da adlandırılan bir nesne örneği ayrılır ve değer kutuya kopyalanır. Buna karşılık, bir `object` başvuru, bir değer türü için atandığında, başvurulan nesnenin doğru değer türünün bir kutusu olan bir onay yapılır ve, denetimi başarılı olursa kutudaki değer kopyalanır.

Değer türleri "üzerine." nesneleri dönüşebilir C# ' ın birleşik tür sistemi etkili bir şekilde anlamına gelir Birleştirme türü kullanan genel amaçlı kitaplıkları nedeniyle `object` başvuru türleri ve değer türleri ile kullanılabilir.

Birkaç türü vardır, ***değişkenleri*** C# ' ta alanlar, dizi öğeleri, yerel değişkenleri ve parametreleri dahil. Değişkenleri temsil eden, depolama konumları ve her değişken değerlerin neler olması belirleyen bir türe sahip aşağıdaki tabloda gösterildiği gibi değişkeninde depolanır.


| __Değişken türü__    | __Olası içeriği__ |
|-------------------------|-----------------------|
| NULL olmayan değer türü | Bu kesin türde bir değer |
| Null değer türü     | Bir null değer veya bu kesin türde bir değer |
| `object`                | Bir null başvuru, bir nesneye bir başvuru türünün bir başvuru veya herhangi bir değer türünün kutulanmış bir değer başvurusu |
| Sınıf türü              | Bir null başvuru, bu sınıf türünün bir örneğine başvuru veya bir başvuru sınıfının bir örneği, sınıf türünden türetilmiş |
| Arabirim türü          | Bir null başvuru, bu arabirim türünü uygulayıp bir sınıf türünün bir örneğine başvuru veya paketlenmiş değere bu arabirim türünü uygulayıp bir değer türü başvurusu |
| Dizi türü              | Bir null başvuru, dizi türü bir örneğe bir başvuru veya uyumlu dizi türünde bir örneğe bir başvuru |
| Temsilci türü           | Bu temsilci türünün bir örneği başvurusu veya bir null başvuru |

## <a name="expressions"></a>İfadeler

***İfadeleri*** oluşturulan ***işlenenler*** ve ***işleçleri***. İfade işleçleri işlenenlere uygulamak için hangi işlemleri gösterir. İşleçler örnekler `+`, `-`, `*`, `/`, ve `new`. Değişmez değerler, alanlar, yerel değişkenleri ve ifadeleri işlenenler örnekleridir.

Bir ifade birden çok işleç içeren ***öncelik*** işleçleri tek tek işleçler değerlendirilme sırası denetler. Örneğin, ifade `x + y * z` değerlendirmesinde `x + (y * z)` çünkü `*` işleci daha yüksek önceliğe sahip `+` işleci.

Çoğu işleçleri olabilir ***aşırı***. İşleç aşırı yüklemesi, aşağıdakilerden birini veya her iki işlenen kullanıcı tanımlı sınıf veya yapı türü olduğu işlemlerinde belirtilmesi için kullanıcı tanımlı işleç uygulamalarına izin verir.

En yüksek öncelikten en düşüğe sırasını işleci kategorileri listeleme C# işleçleri, aşağıdaki tabloda özetlenmiştir. Aynı kategoride işleçleri eşit önceliğe sahiptir.


| __Kategori__                     | __İfade__    | __Açıklama__ |
|----------------------------------|-------------------|-----------------|
| Birincil                          | `x.m`             | Üye erişimi |
|                                  | `x(...)`          | Yöntem ve temsilci çağırma |
|                                  | `x[...]`          | Dizi ve dizinleyici erişimi |
|                                  | `x++`             | Artırım sonrası |
|                                  | `x--`             | Azaltım sonrası |
|                                  | `new T(...)`      | Nesne ve temsilci oluşturma |
|                                  | `new T(...){...}` | Başlatıcı ile nesne oluşturma |
|                                  | `new {...}`       | Anonim nesne Başlatıcı |
|                                  | `new T[...]`      | Dizi oluşturma |
|                                  | `typeof(T)`       | Elde `System.Type` nesnesi `T` |
|                                  | `checked(x)`      | İşaretli bağlamında ifade değerlendirme |
|                                  | `unchecked(x)`    | İşaretlenmemiş bağlamında ifade değerlendirme |
|                                  | `default(T)`      | Türünün varsayılan değerini alın `T` |
|                                  | `delegate {...}`  | Anonim işlevi (anonim yöntemi) |
| Birli                            | `+x`              | Kimlik |
|                                  | `-x`              | Olumsuzlama |
|                                  | `!x`              | Mantıksal olumsuzlama |
|                                  | `~x`              | Bitwise olumsuzlama |
|                                  | `++x`             | Artırım öncesi |
|                                  | `--x`             | Azaltım öncesi |
|                                  | `(T)x`            | Açıkça dönüştürmek `x` yazmak için `T` |
|                                  | `await x`         | Zaman uyumsuz olarak bekleyin `x` tamamlamak için |
| Çarpma                   | `x * y`           | Çarpma |
|                                  | `x / y`           | Bölme |
|                                  | `x % y`           | Kalan |
| Eklenebilir                         | `x + y`           | Toplama, dize bitiştirme, temsilci birleşimi |
|                                  | `x - y`           | Çıkarma, temsilci kaldırma |
| Kaydırma                            | `x << y`          | Sola kaydırma |
|                                  | `x >> y`          | Sağa kaydırma |
| İlişkisel ve tür testi      | `x < y`           | Küçüktür |
|                                  | `x > y`           | Büyüktür |
|                                  | `x <= y`          | Küçük veya eşittir |
|                                  | `x >= y`          | Büyük veya eşittir |
|                                  | `x is T`          | Dönüş `true` varsa `x` olduğu bir `T`, `false` Aksi takdirde |
|                                  | `x as T`          | Dönüş `x` olarak yazılan `T`, veya `null` varsa `x` değil bir `T` |
| Eşitlik                         | `x == y`          | Eşittir      |
|                                  | `x != y`          | Eşit değildir |
| Mantıksal VE                      | `x & y`           | Tamsayı bitwise ve, boolean mantıksal ve |
| Mantıksal XOR                      | `x ^ y`           | Tamsayı bitwise XOR, Boolean mantıksal XOR |
| Mantıksal VEYA                       | <code>x &#124; y</code> | Tamsayı bitwise VEYA, boolean mantıksal VEYA |
| Koşullu VE                  | `x && y`          | Değerlendirilen `y` yalnızca `x` olduğu `true` |
| Koşullu VEYA                   | <code>x &#124;&#124; y</code> | Değerlendirilen `y` yalnızca `x` olduğu `false` |
| Null birleşim                  | `X ?? y`          | Değerlendiren `y` varsa `x` olduğu `null`, `x` Aksi takdirde |
| Koşullu                      | `x ? y : z`       | Değerlendirir `y` varsa `x` olduğu `true`, `z` varsa `x` olduğu `false` |
| Atama ve anonim işlev | `x = y`           | Atama |
|                                  | `x op= y`         | Bileşik atama; desteklenen işleçler şunlardır: `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code> |
|                                  | `(T x) => y`      | Anonim işlevi (lambda ifadesi) |

## <a name="statements"></a>Deyimler

Bir programın eylemleri kullanılarak ifade edilir ***deyimleri***. C# ifadeleri açısından katıştırılmış deyimi bir dizi tanımlanmış birkaç farklı türde destekler.

A ***blok*** bağlamlarda yazılacak birden çok deyime burada tek bir deyimde izin verir. Sınırlayıcılar yazılan deyimlerin listesini bir blok oluşan `{` ve `}`.

***Bildirim deyimleri*** yerel değişkenleri ve sabitleri bildirmek için kullanılır.

***İfade deyimleri*** ifadeler değerlendirilir. Deyimleri kullanılabilir ifadeler, yöntem çağrılarını içeren nesne ayırmaları kullanarak `new` işleç, atamaları kullanarak `=` ve bileşik atama işleçleri, artırma ve azaltma işlemlerini kullanarak`++`ve `--` işleçler ve ifadeler bekler.

***Seçim deyimleri*** olası deyimleri yürütme bazı ifadesinin değerine göre bir dizi birini seçmek için kullanılır. Bu gruba `if` ve `switch` deyimleri.

***Yineleme deyimleri*** sürekli bir katıştırılmış deyimi yürütmek için kullanılır. Bu gruba `while`, `do`, `for`, ve `foreach` deyimleri.

***Atlama deyimleri*** denetim aktarmak için kullanılır. Bu gruba `break`, `continue`, `goto`, `throw`, `return`, ve `yield` deyimleri.

`try`... `catch` deyimi, bir blok yürütülmesi sırasında oluşan özel durumları yakalamak için kullanılır ve `try`... `finally` deyimi veya bir özel durum oluştu olup olmadığını her zaman yürütülür, sonlandırma kodu belirtmek için kullanılır.

`checked` Ve `unchecked` deyimleri bağlam Tamsayı türünde aritmetik işlemler ve dönüştürmeler için taşma denetimini için kullanılır.

`lock` Deyimi belirli bir nesne için karşılıklı dışlama kilidini almak, bir deyimi yürütün ve sonra kilidi için kullanılır.

`using` Deyimi bir kaynağı almak, bir deyimi yürütün ve sonra o kaynağını atma için kullanılır.

Her deyim türü örnekleri aşağıda verilmiştir

__Yerel değişken bildirimleri__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__Yerel sabit bildiriminde__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__ifade deyimi__

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

__`if` Deyimi__

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


__`switch` Deyimi__

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

__`while` Deyimi__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do` Deyimi__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for` Deyimi__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach` Deyimi__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break` Deyimi__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue` Deyimi__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto` Deyimi__

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

__`return` Deyimi__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield` Deyimi__

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

__`throw` ve `try` deyimleri__

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

__`checked` ve `unchecked` deyimleri__

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

__`lock` Deyimi__

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

__`using` Deyimi__

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

***Sınıflar*** olan en temel C# türü. Bir sınıf durumu (alanlar) ve işlemleri (yöntemler ve diğer işlev üyeleri) bir araya getiren bir veri yapısı içinde tek bir birimdir. Dinamik olarak oluşturulan için bir sınıf tanımı sağlar ***örnekleri*** olarak da bilinen, sınıfın ***nesneleri***. Destek sınıfları ***devralma*** ve ***çok biçimlilik***, mekanizmaları yapabildiği ***türetilmiş sınıflar*** genişletmek ve uzmanlaşmış ***temel sınıflar***.

Yeni sınıflar, sınıf bildirimi kullanarak oluşturulur. Bir sınıf bildiriminin öznitelikleri ve sınıfı değiştiricileri, sınıf, taban sınıf (belirtilmişse) ve bir sınıf tarafından uygulanan arabirimler adını belirten bir üst bilgisi ile başlar. Başlık sınırlayıcılar arasında yazılan üye bildirimleri listesini içeren sınıf gövdesinin arkasından `{` ve `}`.

Adlı basit bir sınıf bildirimi verilmiştir `Point`:

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
Sınıfların örneklerini kullanılarak oluşturulur `new` yeni bir örneği için bellek ayırır, işleci örneği başlatmak için bir oluşturucu çağırır ve örneğe bir başvuru döndürür. Aşağıdaki deyimleri iki oluşturma `Point` nesneleri ve bu nesnelere başvurular iki değişken depolayabilirsiniz:

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
Nesne artık kullanımda olmadığında otomatik olarak bir nesnenin kapladığı belleği geri kazanılır. Bu, gerekli veya C# nesneler açıkça serbest mümkün olur.

### <a name="members"></a>Üyeler

Bir sınıf üyesi ya da olan ***statik üyeleri*** veya ***örnek üyeleri***. Statik üyeleri sınıflarına ait ve örnek üyeleri (sınıfların örneklerini) nesnelere ait.

Aşağıdaki tabloda, tür üyeleri bir sınıf içeren genel bir bakış sağlar.


| __Üyesi__   | __Açıklama__ |
|------------  |-----------------|
| Sabitler    | Sınıf ile ilişkili olan sabit değerler |
| Alanlar       | Sınıfın değişkenleri |
| Yöntemler      | Sınıfı tarafından gerçekleştirilen eylemler ve hesaplamaları |
| Özellikler   | Okuma ve adlandırılmış sınıfın özelliklerini yazma ile ilişkili eylemler |
| Dizin Oluşturucular     | Dizin oluşturma sınıfına bir dizi gibi örnekleriyle ilişkili eylemler |
| Olaylar       | Sınıfı tarafından oluşturulan bildirimleri |
| İşleçler    | Dönüşümler ve sınıfı tarafından desteklenen ifade işleçleri |
| Oluşturucular | Sınıfın veya sınıf örneği başlatmak için gerekli eylemleri |
| Yıkıcılar  | Sınıf örneğini kalıcı olarak atılmadan önce gerçekleştirilecek eylemler |
| Türler        | Sınıfı tarafından bildirilen iç içe geçmiş türler |

### <a name="accessibility"></a>Erişilebilirlik

Bir sınıfın her üyesine erişebilir üyeyi program metni bölümlerine denetleyen bir ilişkili erişilebilirlik sahiptir. Erişilebilirlik beş olası biçimi vardır. Bunlar aşağıdaki tabloda özetlenmiştir.


| __Erişilebilirlik__    | __Anlamı__ |
|----------------------|-----------------|
| `public`             | Olmayan sınırlı erişim |
| `protected`          | Bu sınıf veya sınıfların sınırlı erişim, bu sınıftan türetilen |
| `internal`           | Bu program için sınırlı erişim |
| `protected internal` | Bu program için sınırlı erişim veya bu sınıftan türetilmiş sınıflar |
| `private`            | Bu sınıf için sınırlı erişim |

### <a name="type-parameters"></a>Tür parametreleri

Bir sınıf tanımı, sınıf adı türü parametre adları listesini çevreleyen açılı ayraçlar ile izleyerek tür parametrelerinin kümesi belirtebilirsiniz. Tür parametreleri için sınıf üyelerini tanımlamak için sınıf bildirimi gövdesinde kullanılabilir. Aşağıdaki örnekte, tür parametreleri `Pair` olan `TFirst` ve `TSecond`:

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
Tür parametreleri gerçekleştirilecek bildirildiği bir sınıf türü bir genel sınıf türü olarak adlandırılır. Yapı, arabirim ve temsilci türlerinin genel olabilir.

Genel sınıf kullanıldığında, tür bağımsız değişkenleri her tür parametreleri için sağlanmalıdır:

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
Sağlanan gibi tür bağımsız değişkenleri ile genel tür `Pair<int,string>
    ` yukarıda oluşturulan tür adı verilir.

### <a name="base-classes"></a>Temel sınıflar

Sınıf bildiriminin bir temel sınıf, bir iki nokta üst üste ve temel sınıfın adını sınıf adı ve türü parametreleri izleyerek belirtebilirsiniz. Bir temel sınıf belirtimini atlama aynıdır türünden türetme `object`. Aşağıdaki örnekte, temel sınıfını `Point3D` olduğu `Point`ve temel sınıfını `Point` olduğu `object`:

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
Bir sınıf, taban sınıfı üyelerini devralır. Devralma, bir sınıf dolaylı temel sınıfı, örneği ve statik oluşturucular ve Yıkıcılar temel sınıfın dışındaki tüm üyeleri içeren anlamına gelir. Türetilmiş bir sınıf devralır bu yeni üyeler ekleyebilirsiniz, ancak devralınan bir üyeyi tanımını kaldırılamaz. Önceki örnekte, `Point3D` devralan `x` ve `y` alanlarını `Point`ve her `Point3D` örneğini içeren üç alan `x`, `y`, ve `z`.

Temel sınıf türlerinden birinin bir sınıf türünden örtük bir dönüştürme yok. Bu nedenle, bir sınıf türünün bir değişkeni, bu sınıfın bir örneğini veya türetilmiş bir sınıf örneği başvuruda bulunabilir. Örneğin, önceki bir sınıf bildirimleri, türünde bir değişken verilen `Point` ya da başvurabilirsiniz bir `Point` veya `Point3D`:

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a>Alanlar

Bir alanın bir sınıf veya bir sınıf örneği ile ilişkili bir değişkendir.

Bir alana bildirilen `static` değiştiricisi tanımlayan bir ***statik alan***. Statik alan tam olarak bir depolama konumunu tanımlar. Bir sınıfın kaç örneklerin oluşturulduğu ne olursa olsun, yalnızca bir kopyasını statik bir alan yoktur.

Bir alan olmadan bildirilen `static` değiştiricisi tanımlayan bir ***örnek alanı***. Bir sınıfın her örneği, bu sınıfın tüm örnek alanları ayrı bir kopyasını içerir.

Aşağıdaki örnekte, her bir örneği `Color` sınıfına sahip ayrı bir kopyasını `r`, `g`, ve `b` örnek alanları, ancak yalnızca bir kopyası `Black`, `White`, `Red`, `Green`, ve `Blue` statik alanlar:

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
Önceki örnekte gösterilen şekilde ***salt okunur alanları*** ile bildirilebilir bir `readonly` değiştiricisi. Atama bir `readonly` alan alanın bildirimin veya aynı sınıftaki bir oluşturucunun parçası olarak yalnızca oluşabilir.

### <a name="methods"></a>Yöntemler

A ***yöntemi*** hesaplama veya bir nesne veya sınıf tarafından gerçekleştirilen eylem uygulayan bir üyesidir. ***Statik yöntemler*** sınıfı üzerinden erişilir. ***Örnek yöntemleri*** sınıfının örneklerini erişilir.

Yöntemleri (büyük olasılıkla boş) bir listesi olan ***parametreleri***, değerleri veya değişken başvuruları yöntemine geçirilen temsil ve ***dönüş türü***, hesaplanan ve tarafından döndürülen değerin türü belirtir yöntem. Bir yöntemin dönüş türü `void` bir değer döndürmezse.

Türleri gibi yöntemleri de bir dizi yöntem çağrıldığında tür bağımsız değişkenleri belirtilmiş olmalıdır, tür parametreleri olabilir. Türleri, farklı tür bağımsız değişkeni genellikle bir yöntem çağrısının bağımsız değişkenlerden çıkarılan ve açıkça verilmemiş.

***İmza*** yöntemi yöntemi bildirilmiş sınıfında benzersiz olması gerekir. Yöntemin imzası yöntem, tür parametreleri ve sayısı, değiştiriciler ve parametrelerinin türleri sayısı adını oluşur. Yöntemin imzası dönüş türü içermez.

#### <a name="parameters"></a>Parametreler

Parametre değerleri veya değişken başvuruları yöntemlere geçirmek için kullanılır. Bir yöntem parametreleri, gerçek değerleri alma ***bağımsız değişkenleri*** yöntemi çağrıldığında belirtilir. Dört tür parametrelerinin vardır: parametreler, başvuru parametreleri, çıktı parametreleri ve parametre dizileri değeri.

A ***değer parametresi*** giriş parametre geçirme için kullanılır. Bir değer parametresini karşılık gelen bir yerel değişkene parametresi için geçirilen bağımsız ilk değerini alır. Parametresi için geçirilen bağımsız değişken bir değer parametresini değişiklikler etkilemez.

Böylece karşılık gelen bağımsız değişken atlanırsa, varsayılan bir değer belirterek değeri parametreleri, isteğe bağlı olabilir.

A ***parametre başvurusunu*** giriş ve çıkış parametre geçirme için kullanılır. Bir başvuru parametresi için geçirilen bağımsız değişken bir değişken olmalıdır ve yöntemin yürütülmesi sırasında aynı depolama konumu olarak bir bağımsız değişken başvuru parametresi temsil eder. Bir başvuru parametresi ile bildirilen `ref` değiştiricisi. Aşağıdaki örnek kullanımını gösterir `ref` parametreleri.

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
Bir ***çıkış parametresi*** geçirme çıkış parametresi için kullanılır. Çağıran tarafından sağlanan bağımsız değişkenin ilk değeri önemli olması dışında başvuru parametresi bir output parametresi benzer. Çıkış parametresi ile bildirilen `out` değiştiricisi. Aşağıdaki örnek kullanımını gösterir `out` parametreleri.

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
A ***parametre dizisi*** bir yönteme iletilecek bağımsız değişken bir sayı verir. Bir parametre dizisi ile bildirilen `params` değiştiricisi. Bir yöntem yalnızca son parametresi bir parametre dizisi olabilir ve bir parametre dizisi türünde tek boyutlu dizi türü olmalıdır. `Write` Ve `WriteLine` yöntemlerinin `System.Console` sınıfı iyi parametre dizisi kullanımı örnekleri verilmiştir. Bunlar aşağıdaki gibi bildirilir.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Parametre dizisi, bir parametre dizisi kullanan bir yöntem içinde bir dizi türünde tam olarak normal bir parametre gibi davranır. Ancak, bir parametre dizisi olan bir yöntem çağrısını içinde parametresi dizi türünde tek bir bağımsız değişken veya herhangi bir sayıda öğe türü parametre dizisi bağımsız değişkenleri geçirmek mümkündür. İkinci durumda, bir dizi örneği otomatik olarak oluşturulur ve belirtilen bağımsız değişkenler ile başlatıldı. Bu örnek

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
Aşağıdaki yazmaya eşdeğerdir.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a>Yöntem gövdesini ve yerel değişkenler

Bir yöntemin gövdesi yöntemi çağrıldığında çalıştırılacak deyimleri belirtir.

Bir yöntem gövdesi yöntemi çağırmayı için özel değişkenleri bildirebilirsiniz. Bu tür değişkenleri olarak adlandırılmasının ***yerel değişkenler***. Yerel bir değişken bildirimi bir tür adı, bir değişken adı ve büyük olasılıkla bir başlangıç değeri belirtir. Aşağıdaki örnek, yerel bir değişken bildirir `i` bir başlangıç değeri sıfır ve yerel bir değişken ile `j` ile başlangıç değeri yok.

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
C# olarak yerel bir değişken gerektirir ***kesinlikle atanan*** önce değeri elde edilebilir. Örneğin, önceki bildirimi `i` bir başlangıç değeri içermesi gerekmez, derleyici bir hata sonraki kullanımlar için rapor `i` çünkü `i` kesinlikle noktalarda program atanır değil.

Bir yöntemi kullanabilirsiniz `return` denetimi onu arayan döndürülecek deyimleri. Döndüren bir yöntem içinde `void`, `return` deyimleri, bir ifade belirtemez. Olmayan döndüren bir yöntem içinde`void`, `return` deyimleri, dönüş değeri hesaplar bir ifade içermelidir.

#### <a name="static-and-instance-methods"></a>Statik ve örnek yöntemleri

Bir yöntem ile bildirilen bir `static` değiştiricisi bir ***statik yöntem***. Statik bir yöntemi, belirli bir örneği üzerinde çalışmaz ve statik üyeler yalnızca doğrudan erişebilir.

Bir yöntem olmadan bildirilen bir `static` değiştiricisi bir ***örnek yöntemi***. Bir örnek yöntemi belirli bir örneği üzerinde çalışır ve hem statik erişebilir ve örnek üyeler. Bir örnek yöntemi çağrıldığı örnek olarak açıkça erişilebilir `this`. Başvurmak için bir hata olduğunu `this` statik yöntemde.

Aşağıdaki `Entity` sınıfında statik ve örnek üyeleri.

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
Her `Entity` örneği seri numarası (ve burada gösterilmez büyük olasılıkla bazı diğer bilgileri içerir). `Entity` (Olan bir örnek yöntemi gibi) bir oluşturucu sonraki kullanılabilir seri numarasına sahip yeni örneğini başlatır. Oluşturucu bir örnek üyesi olduğundan, her ikisi de erişmesine izin verilen `serialNo` örnek alan ve `nextSerialNo` statik alan.

`GetNextSerialNo` Ve `SetNextSerialNo` statik yöntemler erişebilir `nextSerialNo` statik alan, ancak bir hata için bunları doğrudan erişimini olacak `serialNo` örnek alanı.

Aşağıdaki örnek kullanımını gösterir `Entity` sınıfı.

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
Unutmayın `SetNextSerialNo` ve `GetNextSerialNo` statik yöntemler, sınıf üzerinde çağrılır, ancak `GetSerialNo` sınıf örnekleri üzerinde örnek yöntemi çağrılır.

#### <a name="virtual-override-and-abstract-methods"></a>Soyut metotlar sanal ve geçersiz kılma

Ne zaman bir örnek yöntemi bildirimi içeren bir `virtual` değiştiricisi, yöntem olarak kabul edilir bir ***sanal yöntem***. Hiçbir `virtual` değiştirici mevcut ise, yöntem olarak kabul edilir bir ***sanal olmayan yöntemiyle***.

Sanal bir yöntem çağrıldığında ***çalışma zamanı tür*** bu çağırma aldığı örneğini yerde çağrılacak gerçek yöntem uygulaması belirler. Bir sanal olmayan bir yöntem çağrısı ***derleme zamanı tür*** belirleyici faktör örneğidir.

Sanal bir yöntem olabilir ***geçersiz kılınan*** türetilen bir sınıfta. Ne zaman bir örnek yöntemi bildirimi içeren bir `override` değiştiricisi, yöntemin aynı imzaya sahip devralınan sanal yöntemi geçersiz kılar. Bir geçersiz kılma yöntemi bildirimini sanal yöntem bildiriminde bir yöntem sunar ancak bu yöntem yeni bir uygulamasını sağlayarak mevcut devralınan sanal yöntemi uzmanlaşmış.

Bir ***soyut*** sanal bir yöntem uygulama içermeyen bir yöntemdir. Soyut bir yöntem ile bildirilen `abstract` değiştiricisi ve ayrıca bildirilen bir sınıfta izin `abstract`. Her bir soyut olmayan türetilmiş sınıf içinde soyut bir yöntemi geçersiz kılınmalıdır.

Aşağıdaki örnek, bir soyut sınıfı bildirir `Expression`, bir ifade ağacı düğümünü temsil eder ve üç türetilmiş sınıfları `Constant`, `VariableReference`, ve `Operation`, sabitler, değişken için ifade ağaç düğümleri uygulayın başvurular ve aritmetik işlemler. (Bu benzer, ancak ile karıştırılmamalıdır ifade ağacı türleri sürümünde [ifade ağacı türleri](types.md#expression-tree-types)).

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
Önceki dört sınıfları, aritmetik ifadeler modellemek için kullanılabilir. Örneğin, ifade bu sınıfların örnekleri kullanarak `x + 3` şu şekilde temsil edilebilir.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
`Evaluate` Yöntemi bir `Expression` örneği belirtilen ifadeyi değerlendirir ve üretmek için çağrıldığında bir `double` değeri. Yöntem bağımsız değişken olarak alır bir `Hashtable` (anahtar olarak girişlerinin) değişken adları ve değerleri (olarak girdilerinin değerlerini) içeren. `Evaluate` Yöntemdir, türetilmiş soyut olmayan sınıflar, gerçek bir uygulama sağlamak için kılmalı yani sanal soyut bir yöntem.

A `Constant`'s uygulaması `Evaluate` yalnızca saklı sabiti döndürür. A `VariableReference`hashtable değişken adında uygulama şuna kullanıcının ve sonuçta elde edilen değeri döndürür. Bir `Operation`ait uygulama ilk sol ve sağ işlenen değerlendirilir (yinelemeli olarak çağırarak kendi `Evaluate` yöntemleri) ve ardından belirli bir aritmetik işlemi gerçekleştirir.

Aşağıdaki program kullanan `Expression` ifadeyi değerlendiren şeyde sınıfları `x * (y + 2)` farklı değerler için `x` ve `y`.

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

#### <a name="method-overloading"></a>Yöntemi aşırı yüklemesi

Yöntemi ***aşırı yükleme*** aynı sınıfta benzersiz imzaları sahip oldukları sürece aynı ada sahip birden çok yöntem izin verir. Aşırı yüklenmiş yöntem çağrısından derlerken, derleyicinin kullandığı ***aşırı yükleme çözümlemesi*** çağrılacak belirli yöntemi belirlemek için. En iyi eşleşen bağımsız değişkenler veya tek en iyi eşleşme bulunamazsa, bir hata raporları bir yöntemi aşırı yükleme çözünürlüğü bulur. Aşağıdaki örnek, aşırı yükleme çözünürlüğü geçerli gösterir. Her bir çağrıda yorum `Main` yöntemi gösterir hangi yöntemi gerçekten çağrılır.

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
Örnekte gösterildiği gibi belirli bir yöntem her zaman tam parametre türleri bağımsız değişkenleri açıkça atama ve/veya tür bağımsız değişkenleri açıkça sağlama tarafından seçilebilir.

### <a name="other-function-members"></a>Diğer işlev üyeleri

Yürütülebilir kod içeren bir üye olarak bilinir topluca ***işlev üyeleri*** bir sınıf. Birincil işlev üyeleri türü olan yöntemler, önceki bölümde açıklanmaktadır. Bu bölümde, işlev üyeleri C# tarafından desteklenen diğer türleri açıklanmaktadır: Oluşturucular, özellikler, dizin oluşturucular, olaylar, işleçleri ve Yıkıcılar.

Adlı bir genel sınıf aşağıdaki kodda gösterildiği `List<T>`, growable nesnelerin listesini uygular. Sınıf işlev üyeleri en yaygın tür çeşitli örneklerini içerir.


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

C# örneği hem de statik oluşturucular destekler. Bir ***örnek oluşturucusu*** uygulayan bir sınıf örneği başlatmak için gerekli eylemleri bir üyesidir. A ***statik Oluşturucu*** ilk yüklendiğinde bir sınıf başlatmak için gerekli eylemleri uygulayan bir üyesidir.

Bir oluşturucu dönüş türü ve kapsayan sınıfı aynı ada sahip bir yöntemi gibi bildirilir. Bir oluşturucu bildirimi içeriyorsa bir `static` değiştiricisi, bir statik Oluşturucu bildirir. Aksi takdirde, bir örnek oluşturucusu bildirir.

Örnek oluşturucuları aşırı yüklenebilir. Örneğin, `List<T>
` sınıfı bildirir, bir parametre ve süren iki örnek oluşturucuları bir `int` parametresi. Örnek oluşturucuları kullanarak çağrılır `new` işleci. Aşağıdaki deyimleri iki ayırma `List<string>
` her oluşturucular kullanma örnekleri `List` sınıfı.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Diğer üyeleri farklı olarak örnek oluşturucuları olmayan devralınır ve sınıfı, bu gerçekte bildirilen dışında hiçbir örnek oluşturucuları bir sınıfa sahip. Hiçbir örnek oluşturucusu için bir sınıf sağlanırsa, sonra boş bir parametre olmadan otomatik olarak sağlanır.

#### <a name="properties"></a>Özellikler

***Özellikleri*** alanları doğal bir uzantısıdır. Her ikisi de ilişkili türlerini üyeleriyle adlı ve alanlar ve Özellikler erişmek için sözdizimi aynıdır. Ancak, alanlar özellikleri depolama konumları belirtmek değil. Bunun yerine, özelliklere sahip ***erişimcileri*** değerlerine okunabilir veya yazılabilir bırakıldığında yürütülecek deyimler belirtin.

Bildirimi ile biten dışında bir özellik gibi bir alan, bildirilmiş bir `get` erişimci veya `set` sınırlayıcılar arasında yazılan erişimci `{` ve `}` noktalı bitiş yerine. Her ikisi de sahip bir özellik bir `get` erişimci ve `set` erişen bir ***okuma-yazma özelliği***, yalnızca bir özellik bir `get` erişen bir ***salt okunur özelliği***ve yalnızca özellik bir `set` erişen bir ***salt yazılır özelliği***.

A `get` bir parametresiz yöntemin dönüş değerini özellik türü ile erişimci karşılık gelir. Bir özellik bir ifadede başvurulduğunda, atama hedefi olarak dışında `get` özelliğin erişimci özelliğin değerini hesaplamak için çağrılır.

A `set` erişimci karşılık gelen bir yönteme adlı tek bir parametre `value` ve dönüş türü yok. Ne zaman bir özelliği başvuru atama hedefi veya işleneni olarak `++` veya `--`, `set` erişimci, yeni değer sağlayan bağımsız değişkenlerle çağrılır.

`List<T>
` Sınıfı bildirir iki özellik `Count` ve `Capacity`, olan salt okunur ve okuma-yazma, sırasıyla. Bu özelliklerin kullanımı bir örnek verilmiştir.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Benzer şekilde, alanlar ve yöntemler, C# örnek özelliklerini hem statik özelliklerini destekler. Statik özellikler ile bildirilmiş `static` değiştiricisi ve örnek özelliklerini bildirilir olmadan.

Bir özelliğin accessor(s) sanal olabilir. Ne zaman bir özellik bildirimi içeren bir `virtual`, `abstract`, veya `override` değiştiricisi, özellik accessor(s) için geçerlidir.

#### <a name="indexers"></a>Dizin Oluşturucular

Bir ***dizin oluşturucu*** nesneleri dizisi olarak aynı şekilde dizinlenmesini sağlar bir üyesidir. Üyenin adını olması dışında dizin oluşturucu bir özellik gibi bildirilir `this` sınırlayıcılar arasında yazılmış bir parametre listesi ardından `[` ve `]`. Parametreler, dizin oluşturucunun accessor(s) içinde kullanılabilir. Benzer şekilde, özellikler, dizin oluşturucular okuma-yazma, salt okunur ve salt yazılır olabilir ve bir dizin oluşturucu accessor(s) sanal olabilir.

`List` Sınıfı bildirir gereken tek bir okuma-yazma dizin oluşturucu bir `int` parametresi. Dizin oluşturucunun, dizin mümkün kılar `List` ile örnekler `int` değerleri. Örneğin

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
Dizin Oluşturucular, sayı veya kendi parametre türleri farklı olduğu sürece bir sınıfın birden çok dizin oluşturucu bildirebilirsiniz anlamına gelen aşırı yüklenebilir.

#### <a name="events"></a>Olaylar

Bir ***olay*** bir sınıf veya nesne bildirimleri sağlamak için sağlayan bir üyesidir. Dışında bildirimi içeren bir olay gibi bir alan bildirilmiş bir `event` anahtar sözcüğü ile tür bir temsilci türü olmalıdır.

(Olay soyut değil ve erişimcileri bildirmiyor sağlanan) bir olay üyesi bildiren bir sınıf içinde olay yalnızca bir temsilci türüne bir alan gibi davranır. Olaya eklenmiş olan olay işleyicilerini temsil eden bir temsilci bir başvuru alan depolar. Olay işleyici mevcut olması durumunda, alandır `null`.

`List<T>
` Sınıfı olarak adlandırılan tek bir olay üyesi bildirir `Changed`, yeni bir öğe listesine eklendiğini gösterir. `Changed` Olayı tarafından oluşturulur `OnChanged` hangi ilk olay olup olmadığını denetler, sanal bir yöntem `null` (hiçbir işleyicileri bulunduğunu anlamına gelir). Olay bildirmek, kavram olay tarafından temsil edilen temsilci çağırmak için tam olarak eşittir; Bu nedenle, olayları oluşturma için hiçbir özel dil yapıları vardır.

İstemciler react ile olayları ***olay işleyicileri***. Olay işleyicileri kullanılarak eklenen `+=` işleci ve kaldırılan kullanarak `-=` işleci. Aşağıdaki örnek bir olay işleyicisi ekler `Changed` olayı bir `List<string>
`.

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
Denetim bir olay temel alınan depolamanın istenen burada Gelişmiş senaryolar için bir olay bildirimi açıkça sağlayabilir `add` ve `remove` biraz benzerdir erişimcileri `set` özellik erişimcisi.

#### <a name="operators"></a>İşleçler

Bir ***işleci*** belirli ifade işleci bir sınıfın bir örneği için uygulama ne anlama geldiğini tanımlayan bir üyesidir. Üç tür işleç tanımlanabilir: Birli işleçler, ikili işleçler ve dönüştürme işleçleri. Tüm işleçler olarak bildirilmelidir `public` ve `static`.

`List<T>
` Sınıfı bildirir iki işleç `operator==` ve `operator!=`ve bu nedenle bu işleçleri için geçerli ifadeler için yeni anlamı verir `List` örnekleri. Özellikle, da iki eşitlik operatörlerini tanımlayın `List<T>
` örnekler olarak karşılaştırma kullanarak içerdiği nesnelerin her biri kendi `Equals` yöntemleri. Aşağıdaki örnekte `==` karşılaştırmak için işleci `List<int>
` örnekleri.

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

İlk `Console.WriteLine` çıkarır `True` listelerini nesneleri aynı sırada aynı değerlerle aynı sayıda içerdiğinden. Vardı `List<T>
` tanımlanmamış `operator==`, ilk `Console.WriteLine` çıkış `False` çünkü `a` ve `b` başvuru farklı `List<int>
` örnekleri.

#### <a name="destructors"></a>Yıkıcılar

A ***yok Edicisi*** uygulayan bir sınıf örneği yok etmek için gereken eylemler bir üyesidir. Yok ediciler parametrelere sahip olamaz, Erişilebilirlik değiştiricilere sahip olamaz ve açıkça çağrılamaz. Bir örneği için yok edici, çöp toplama sırasında otomatik olarak çağrılır.

Çöp Toplayıcısı nesneleri toplar ve Yıkıcılar çalıştırmak ne zaman karar içinde geniş enlem izin verilir. Özellikle, yok edici çağrıları zamanlamasını belirleyici değildir ve yok ediciler herhangi bir iş parçacığı üzerinde çalıştırılabilir. Diğer çözüm uygun olduğunda bunlar ve diğer nedenler, sınıfları yok ediciler uygulamalıdır.

`using` Deyim nesnesi yok etme için daha iyi bir yaklaşım sağlar.

## <a name="structs"></a>Yapılar

Sınıflar gibi ***yapılar*** veri yapıları, veri üyeleri ve işlev üyeleri içerebilir, ancak farklı sınıflar, yapılar değer türleri ve yığın ayırma gerektirmez. Bir sınıf türünün bir değişkeni dinamik olarak ayrılan bir nesneye başvuru depoladığı bir yapı türünün değişkenini doğrudan, struct'ın verileri depolar. Yapı türleri, kullanıcı tarafından belirtilen devralma desteklemez ve tüm yapı türleri örtülü olarak tür devralmasına `object`.

Yapılar değer semantiklere sahip küçük veri yapıları için özellikle yararlıdır. Karmaşık sayılar, koordinat sisteminde noktaları veya anahtar-değer çiftlerinin bir sözlükteki tüm iyi yapılar örnekleridir. Küçük veri yapıları için sınıflar, bir uygulama bellek ayırmaları sayısında büyük bir fark yapabilirsiniz yerine yapılar kullanımını gerçekleştirir. Örneğin, aşağıdaki program oluşturur ve 100 noktaları dizisini başlatır. İle `Point` sınıfı olarak uygulanır, 101 ayrı nesneler örneği oluşturulur; bir dizi ve her biri 100 öğeleri için.

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
Alternatif olmaktır `Point` yapı.

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
Şimdi, yalnızca bir nesne örneği — bir dizi — ve `Point` depolanan satır içi dizi örnekleridir.

Yapı Oluşturucuları ile çağrılır `new` işleci, ancak değil yaptığından bu bellek tahsis edilir. Dinamik olarak bir nesne ayırma ve buna bir başvuru döndürerek, yerine bir yapı Oluşturucu yalnızca yapı değerini kendisi (genellikle geçici bir konuma yığın üzerinde) döndürür ve bu değer daha sonra gerektiğinde kopyalanır.

Sınıfları ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur. Yapılar, her değişkenleri kendi veri kopyası vardır ve yapılan işlemlerin diğerini etkilemesi olanaklı birinde değil. Örneğin, aşağıdaki kod parçası tarafından üretilen çıkış bağlıdır `Point` bir sınıf veya yapı.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
Varsa `Point` bir sınıf çıkış `20` çünkü `a` ve `b` aynı nesneye başvuru. Varsa `Point` bir yapı çıktı `10` çünkü atamasını `a` için `b` değeri bir kopyasını oluşturur ve bu kopyayı sonraki atamaya tarafından etkilenmez `a.x`.

Önceki örnekte iki yapılar sınırlamaları vurgular. İlk olarak, bir yapının tamamını kopyalama atama ve değerin parametre geçirme başvuru türleri ile yapılar ile daha pahalı olabilir. Bu nedenle, bir nesne başvurusu kopyalama daha genellikle daha az verimlidir. İkinci dışında `ref` ve `out` parametreleri, bu durumlarda, çeşitli kullanımları kullanıma kuralları yapı birimleri için başvuru oluşturmak mümkün değildir.

## <a name="arrays"></a>Diziler

Bir ***dizi*** hesaplanan dizinlerini erişilen değişken bir sayı içeren bir veri yapısıdır. Bir dizi içindeki değişkenler olarak da bilinir ***öğeleri*** dizinin tümü aynı türde ve bu tür çağrılır ***öğe türü*** dizi.

Dizi türleri, başvuru türleridir ve bir dizi değişkeni bildirimi yalnızca kenara alanı başvuru bir dizi örneği için ayarlar. Gerçek bir dizi örnekleri dinamik olarak çalışma zamanı kullanılarak oluşturulur `new` işleci. `new` İşlemi belirtir ***uzunluğu*** sonra Örneğin ömrü boyunca sabittir yeni dizi örneği. Dizinleri öğelerinden oluşan bir dizi aralığından `0` için `Length - 1`. `new` İşleci, örneğin, sıfır olan tüm sayısal türler için varsayılan değerlerine bir dizinin öğeleri otomatik olarak başlatır ve `null` tüm başvuru türleri için.

Aşağıdaki örnek, bir dizi oluşturur. `int` öğeleri, dizi başlatır ve dizinin içeriği yazdırır.

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
Bu örneği oluşturur ve üzerinde çalıştığı bir ***tek boyutlu dizi***. C# ' yı da destekler ***çok boyutlu diziler***. Olarak da bilinen bir dizinin boyut sayısını yazın ***derece*** dizi türü köşeli ayraç yazılan virgül sayısı artı bir dizi türünde olduğu. Aşağıdaki örnek, tek boyutlu bir, iki boyutlu bir ve üç boyutlu bir dizi ayırır.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
`a1` Dizi 10 öğe içeriyor `a2` dizi içeriyor 50 (10 × 5) öğeleri ve `a3` dizi içeriyor (10 × 5 × 2) 100 öğeleri.

Bir dizinin öğe türü bir dizi türü de dahil olmak üzere herhangi bir tür olabilir. Bir dizi türünde öğelere sahip bir dizi adlandırılan bir ***düzensiz dizi*** öğe dizisi uzunluklarının tümünü değil aynı olmak zorunda olduğu. Aşağıdaki örnekte dizi ayırır `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
İlk satır, üç öğeyle her türü bir dizi oluşturur `int[]` ve her bir başlangıç değeri ile `null`. Sonraki satırların başvurular değişen uzunlukları örneklerini tek bir dizi üç öğeleri ardından başlatın.

`new` İşleci verir başlangıç değerlerini kullanarak belirtilen dizi öğelerinin bir ***dizi Başlatıcısı***, sınırlayıcılar arasında yazılan ifadeler listesi olduğu `{` ve `}`. Aşağıdaki örnek ayırır ve başlatan bir `int[]` üç öğelerle.

```csharp
int[] a = new int[] {1, 2, 3};
```
Dizinin uzunluğu arasında ifadelerin sayısından algılanır Not `{` ve `}`. Yerel değişken ve alan bildirimleri kısalttık daha fazla sağlayacak şekilde dizi türü açıklandı gerekmez.

```csharp
int[] a = {1, 2, 3};
```
Önceki örneklerin her ikisi de aşağıdakine eşdeğerdir:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a>Arabirimler

Bir ***arabirimi*** sınıfları ve yapıları tarafından uygulanan bir sözleşmeyi tanımlar. Bir arabirim yöntemleri, özellikleri, olayları ve dizin oluşturucular içerebilir. Bir arabirim tanımlar üyelerinin uygulamalarını sağlamaz; yalnızca bir sınıf ya da arabirimi uygulayan yapının tarafından sağlanması gereken üyeleri belirtir.

, Arabirimleri görevlendirmek ***birden çok devralma***. Aşağıdaki örnekte, arabirim `IComboBox` hem de devralan `ITextBox` ve `IListBox`.

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
Sınıflar ve yapı birimleri birden fazla arabirim uygulayabilir. Aşağıdaki örnekte, sınıf `EditBox` hem `IControl` ve `IDataBound`.

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
Bir sınıf veya yapı, belirli bir arabirim uygular, bu sınıfın veya yapının örneği, bir arabirim türüne örtük olarak dönüştürülebilir. Örneğin

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
Dinamik tür atamaları, burada bir örnek statik olarak belirli bir arabirim uygulamak için bilinmiyor durumlarda kullanılabilir. Örneğin, bir nesnenin elde etmek için dinamik tür atamaları aşağıdaki deyimleri kullanın `IControl` ve `IDataBound` arabirimi uygulamaları. Nesnenin gerçek türü olduğundan `EditBox`, yayınları başarılı.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
Önceki `EditBox` sınıfı `Paint` yönteminden `IControl` arabirimi ve `Bind` yönteminden `IDataBound` arabirimi kullanılarak uygulanır `public` üyeleri. C# ' yı da destekler ***açık arabirim üyesi uygulamalarını***, sınıfın kullandığı veya yapı kaçınmak üyeleri yapmadan `public`. Açık arabirim üyesi uygulaması, tam bir arabirim üye adını kullanarak yazılır. Örneğin, `EditBox` sınıf uygulama `IControl.Paint` ve `IDataBound.Bind` yöntemlerini kullanarak açık arabirim üyesi uygulamaları gibi.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Açık arabirim üyeleri yalnızca arabirim türü erişilebilir. Örneğin, uygulanması `IControl.Paint` önceki tarafından sağlanan `EditBox` sınıf yalnızca çağrılabilir ilk dönüştürerek `EditBox` başvurusu `IControl` arabirim türü.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a>Numaralandırmalar

Bir ***sabit listesi türü*** adlandırılmış sabitler kümesini içeren farklı bir değer türüdür. Aşağıdaki örnek bildirir ve adlı bir sabit listesi türünü kullanıyor `Color` üç sabit değerler ile `Red`, `Green`, ve `Blue`.

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
Her bir numaralandırma türünün adlı karşılık gelen bir integral türü olan ***temel alınan türü*** sabit listesi türü. Bir temel türü açıkça bildirmiyor bir sabit listesi türü temel alınan türündedir `int`. Bir numaralandırma türünün depolama biçimi ve olası değerler aralığı kendi temel türü tarafından belirlenir. Enum türü alan değerleri kümesi numaralandırma üyelerini sınırlı değildir. Özellikle, bir sabit listesi türünü temel herhangi bir değerini numaralandırma türüne dönüştürülebilen ve sabit listesi türü farklı geçerli bir değer.

Aşağıdaki örnek adlı bir sabit listesi türü bildirir `Alignment` bir temel alınan türüyle `sbyte`.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
Önceki örnekte gösterildiği gibi bir numaralandırma üyesi bildirimi üyenin değerini belirten sabit bir ifade içerebilir. Her sabit listesi üyesi için sabit değerin sabit listesinin temel alınan türü aralığında olması gerekir. Bir numaralandırma üyesi bildirimi bir değeri açıkça belirtmediği üyesi (sabit listesi türü ilk üye ise) sıfır değerini veya metin içeriğini eklemek önceki sabit listesi üyesi artı bir değer verilir.

Sabit listesi değerlerinin dönüştürülür tamsayı değerleri ve tersi tür atamaları kullanarak olabilir. Örneğin

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
Tam sayı değeri sıfır numaralandırma türüne dönüştürülerek herhangi bir enum tür varsayılan değerdir. Burada değişkenleri için varsayılan değer otomatik olarak başlatılır durumlarda değişkenleri sabit listesi türleri için verilen değer budur. Varsayılan değer olan kolayca kullanılabilir olması için bir liste türü değişmez değer için sırayla `0` örtük olarak herhangi bir numaralandırma türüne dönüştürür. Bu nedenle, aşağıdaki izin verilir.

```csharp
Color c = 0;
```

## <a name="delegates"></a>Temsilciler

A ***temsilci türü*** belirli bir parametre olan yöntemlere başvuruları temsil listesi ve dönüş türü. Temsilciler, yöntemleri değişkenine atanır ve parametre olarak geçirilen varlıklar olarak değerlendirmek mümkün kılar. Diğer dillerde bulunan işlev işaretçileri kavramı temsilcileri benzerdir ancak işlev işaretçileri, nesne yönelimli ve tür kullanımı uyumlu temsilciler.

Aşağıdaki örnek bildirir ve adlandırılmış bir temsilci türü kullanır `Function`.

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
Örneği `Function` temsilci türü, alan herhangi bir yöntemi başvurabilir bir `double` bağımsız değişkeni ve döndürür bir `double` değeri. `Apply` Yöntemi uygular bir verilen `Function` öğelerin bir `double[]`, döndüren bir `double[]` sonuçları. İçinde `Main` yöntemi `Apply` üç farklı işlevlere uygulamak için kullanılan bir `double[]`.

Bir temsilci bir statik yöntem başvurabilir (gibi `Square` veya `Math.Sin` önceki örnekte) veya bir örnek yöntemi (gibi `m.Multiply` önceki örnekte). Bir örnek yöntemi başvuran bir temsilci Ayrıca belirli bir nesnenin başvuruda bulunduğundan ve temsilci örnek yöntemi çağrıldığında, bu nesne haline gelir `this` çağrısı.

Temsilciler da "anında oluşturulan satır içi yöntemler" anonim işlevler kullanılarak oluşturulabilir. Anonim İşlevler, yerel değişkenler çevreleyen yöntemlerden görebilirsiniz. Bu nedenle, yukarıdaki çarpan örneği daha kolay kullanmadan yazılabilir bir `Multiplier` sınıfı:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
Bir ilgi çekici ve kullanışlı bir temsilci, bilmiyorsanız veya yöntemin başvurduğu sınıfı hakkında dikkatli olun, özelliğidir; önemli olan başvurulan yöntemi aynı parametre ve dönüş türü temsilciyle sahiptir.

## <a name="attributes"></a>Öznitelikler

Bazı yönlerini davranışlarını denetleyen değiştiriciler, türleri, üyeleri ve diğer varlıkların bir C# programında destekler. Örneğin, bir yöntemin erişilebilirliği kullanılarak denetlenir `public`, `protected`, `internal`, ve `private` değiştiriciler. Bildirim temelli bilgileri kullanıcı tanımlı türde program varlıklara bağlı ve çalışma zamanında alınan C# bu özellik genelleştirir. Programlar belirtmek bu ek tanımlayıcı bilgiler tanımlama ve kullanma ***öznitelikleri***.

Aşağıdaki örnek bildirir bir `HelpAttribute` , ilişkili belgelere bağlantılar sağlamak için program varlıkları yerleştirildiği özniteliği.

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
Tüm öznitelik sınıfları türetilmesi `System.Attribute` temel .NET Framework tarafından sağlanan sınıfı. Öznitelik, köşeli ayraç içinde ilişkili bildirimi hemen önce tüm bağımsız değişkenleri yanı sıra bunların adı vererek uygulanabilir. Bir özniteliğin adı biterse `Attribute`, öznitelik başvurulduğunda bu bölümü adı atlanmış olabilir. Örneğin, `HelpAttribute` özniteliği aşağıdaki gibi kullanılabilir.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
Bu örnekte bağlayan bir `HelpAttribute` için `Widget` sınıf ve başka `HelpAttribute` için `Display` sınıfında yöntemi. Genel oluşturucular bir öznitelik sınıfı özniteliği için bir program varlığı eklendiğinde sağlanmalıdır bilgileri denetler. Ek bilgi öznitelik sınıfının ortak okuma-yazma özelliklerine başvurarak sağlanabilir (başvuru gibi `Topic` özelliği daha önce).

Aşağıdaki örnek, belirli bir programı varlık için öznitelik bilgileri, çalışma zamanında nasıl alınabilir gösterir yansıma kullanarak.

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
Belirli bir özniteliği yansıma yoluyla istendiğinde program kaynakta sağlanan bilgileri ile öznitelik sınıfı için oluşturucu çağrılır ve sonuçta elde edilen öznitelik örneği döndürülür. Ek bilgi özellikleri aracılığıyla sağlandıysa, öznitelik örneği döndürülmeden önce bu özellikleri için belirtilen değerlere ayarlanır.
