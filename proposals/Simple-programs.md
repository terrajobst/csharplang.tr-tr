---
ms.openlocfilehash: b9697fc1d772ba59ed3b1de339a5a3d4eb24b1bd
ms.sourcegitcommit: 36b028f4d6e88bd7d4a843c6d384d1b63cc73334
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "79485226"
---
# <a name="simple-programs"></a>Basit programlar

* [x] önerilir
* [x] prototipi: başlatıldı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

Bir *compilation_unit* (örneğin, kaynak dosya) *namespace_member_declaration*bir dizi *deyimin* hemen öncesine oluşmasına izin verin.

Sözdizimi, böyle bir *deyim* sırası varsa, aşağıdaki tür bildiriminin, gerçek tür adı ve Yöntem adı bildiriminde yer aldığı şekilde belirlenir:

``` c#
static class Program
{
    static async Task Main()
    {
        // statements
    }
}
```

Ayrıca bkz. https://github.com/dotnet/csharplang/issues/3117.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Açık bir `Main` metoduna ihtiyaç duyduğundan, en basit programları bile çevreleyen belirli bir ortak miktar. Bu, dil öğrenimi ve program açıklık gibi görünüyor. Bu nedenle özelliğin birincil amacı, kullanıcıların, öğrenenler C# ve kodun netliğine yönelik gereksiz yere sahip olmayan programlara izin vermektir.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

### <a name="syntax"></a>Sözdizimi

Yalnızca *namespace_member_declaration*s 'den önce, derleme biriminde bir dizi *deyimin*kullanılmasına izin veriliyor. ek sözdizimi yalnızca

``` antlr
compilation_unit
    : extern_alias_directive* using_directive* global_attributes? statement* namespace_member_declaration*
    ;
```

Her birinde, bir *compilation_unit* *deyimin*hepsi yerel işlev bildirimleri olmalıdır. 

Örnek:

``` c#
// File 1 - any statements
if (args.Length == 0
    || !int.TryParse(args[0], out int n)
    || n < 0) return;
Console.WriteLine(Fib(n).curr);

// File 2 - only local functions
(int curr, int prev) Fib(int i)
{
    if (i == 0) return (1, 0);
    var (curr, prev) = Fib(i - 1);
    return (curr + prev, curr);
}
```

### <a name="semantics"></a>İçeriyor

Programın herhangi bir derleme biriminde herhangi bir en üst düzey deyim varsa, anlamı aşağıdaki gibi genel ad alanındaki bir `Program` sınıfının `Main` yönteminin blok gövdesinde birleştirilmiş gibi olur:

``` c#
static class Program
{
    static async Task Main()
    {
        // File 1 statements
        // File 2 local functions
        // ...
    }
}
```

"Program" ve "Main" adlarının yalnızca çizimler amacıyla kullanıldığını, derleyici tarafından kullanılan gerçek adların uygulamaya bağımlı olduğunu ve ne tür, ne de kaynak koddan ad tarafından başvurulduğunu unutmayın.

Yöntemi, programın giriş noktası olarak atanır. Kurala göre açıkça tanımlanmış yöntemler, giriş noktası adayları yok sayılır. Meydana geldiğinde bir uyarı bildirilir. `-main:<type>` derleyici anahtarı belirtmek hatadır.

Herhangi bir derleme biriminde yerel işlev bildirimleri dışındaki deyimler varsa, bu derleme biriminden gelen deyimler önce oluşur. Bunun nedeni, bir dosyadaki yerel işlevlerin başka bir dosyada yerel değişkenlere başvurması için geçerli olmasına neden olur. Diğer derleme birimlerinden (tüm yerel işlevler olacak) ifade katkılarının sırası tanımsız.

Zaman uyumsuz işlemlere, en üst düzey deyimlerde, düzenli bir zaman uyumsuz giriş noktası yöntemi içindeki ifadelerde izin verilen dereceye kadar izin verilir. Ancak, bunlar gerekli değildir, `await` ifadeleri ve diğer zaman uyumsuz işlemler atlanırsa hiçbir uyarı üretilmez. Bunun yerine, oluşturulan giriş noktası yönteminin imzası şu şekilde eşdeğerdir 
``` c#
    static void Main()
```

Yukarıdaki örnekte aşağıdaki `$Main` yöntemi bildirimi elde edersiniz:

``` c#
static class $Program
{
    static void $Main()
    {
        // Statements from File 1
        if (args.Length == 0
            || !int.TryParse(args[0], out int n)
            || n < 0) return;
        Console.WriteLine(Fib(n).curr);
        
        // Local functions from File 2
        (int curr, int prev) Fib(int i)
        {
            if (i == 0) return (1, 0);
            var (curr, prev) = Fib(i - 1);
            return (curr + prev, curr);
        }
    }
}
```

Aynı zamanda aşağıdakine benzer bir örnek:
``` c#
// File 1
await System.Threading.Tasks.Task.Delay(1000);
System.Console.WriteLine("Hi!");
```

Şu şekilde olur:
``` c#
static class $Program
{
    static async Task $Main()
    {
        // Statements from File 1
        await System.Threading.Tasks.Task.Delay(1000);
        System.Console.WriteLine("Hi!");
    }
}
```

### <a name="scope-of-top-level-local-variables-and-local-functions"></a>Üst düzey yerel değişkenlerin kapsamı ve yerel işlevler

En üst düzey yerel değişkenler ve işlevler, oluşturulan giriş noktası yöntemine "sarmalanmış" olsa da, bunlar programın tamamında hala kapsam içinde olmalıdır.
Basit ad değerlendirmesinin amacı için, genel ad alanına ulaşıldığında:
- İlk olarak, oluşturulan giriş noktası yöntemi içinde adı değerlendirmek için bir girişimde bulunuldu ve bu deneme başarısız olursa 
- Genel ad alanı bildirimi içindeki "normal" değerlendirme gerçekleştirilir. 

Bu, genel ad alanı içinde ve içeri aktarılan adların gölgelendirilmesinden farklı ad alanları ve türlerin ad gölgelendirmesine yol açabilir.

Basit ad değerlendirmesi en üst düzey deyimler dışında gerçekleşirse ve değerlendirme, bir hataya yol açacak bir en üst düzey yerel değişken veya işlev verir.

Bu şekilde, gelecekte "en üst düzey işlevleri" (https://github.com/dotnet/csharplang/issues/3117)Senaryo 2) daha iyi ele alma imkanımızı koruyoruz ve bu kullanıcılara desteklerini yanlışlıkla düşünen kullanıcılara faydalı Tanılamalar verebiliyor.

