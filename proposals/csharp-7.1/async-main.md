---
ms.openlocfilehash: 405153448d0e3685d6f22725e00d75d9250b3e20
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484799"
---
# <a name="async-main"></a>Zaman uyumsuz ana

* [x] önerilir
* [] Prototipi
* [] Uygulama
* [] Belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

Giriş noktasının `Task` / `Task<int>` döndürmesini ve `async`olarak işaretlenmesini sağlayarak uygulamanın Main/EntryPoint yönteminde kullanılmasına `await` izin verin.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Öğrenirken C#, konsol tabanlı yardımcı programlar yazarken ve ana bilgisayardan `async` yöntemleri çağırmak ve `await` için küçük test uygulamaları yazarken çok yaygın hale gelir.  Bugün, bu tür `await`' ı ayrı bir zaman uyumsuz yöntemde yapılacağından, geliştiricilerin çalışmaya başlamak için aşağıdaki gibi demirbaş yazmasına neden olan bir karmaşıklık düzeyi ekleyeceğiz:

```csharp
public static void Main()
{
    MainAsync().GetAwaiter().GetResult();
}

private static async Task MainAsync()
{
    ... // Main body here
}
```

Bu demirbaş için gereksinimi kaldırabiliriz ve yalnızca ana kendisinden `await`s 'nin kullanılabileceği gibi `async` izin vererek çalışmaya başlamanıza daha kolay hale getirebilirsiniz.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Şu anda Şu imzalara izin veriliyor entryPoints:

```csharp
static void Main()
static void Main(string[])
static int Main()
static int Main(string[])
```

İzin verilen entryPoints listesini şunları içerecek şekilde genişlettik:

```csharp
static Task Main()
static Task<int> Main()
static Task Main(string[])
static Task<int> Main(string[])
```

Uyumluluk risklerinden kaçınmak için, bu yeni imzalar yalnızca önceki küme için aşırı yükleme yoksa geçerli giriş noktaları olarak kabul edilir.
Dil/derleyici, giriş noktasının `async`olarak işaretlenmesini gerektirmez, ancak kullanımları büyük çoğunluğunun bu şekilde işaretleneceğini bekleiyoruz.

Bunlardan biri giriş noktası olarak tanımlandığında, derleyici bu kodlanmış yöntemlerden birini çağıran gerçek bir giriş noktası yöntemini sentezle birleştirmeyecektir:
- ```static Task Main()```, derleyicinin ```private static void $GeneratedMain() => Main().GetAwaiter().GetResult();``` eşdeğerini yayacaktır.
- ```static Task Main(string[])```, derleyicinin ```private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();``` eşdeğerini yayacaktır.
- ```static Task<int> Main()```, derleyicinin ```private static int $GeneratedMain() => Main().GetAwaiter().GetResult();``` eşdeğerini yayacaktır.
- ```static Task<int> Main(string[])```, derleyicinin ```private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();``` eşdeğerini yayacaktır.

Örnek kullanım:

```csharp
using System;
using System.Net.Http;

class Test
{
    static async Task Main(string[] args) =>
        Console.WriteLine(await new HttpClient().GetStringAsync(args[0]));
}
```

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Ana sakıncası, ek giriş noktası imzalarını desteklemeye yönelik ek karmaşıklıkdır.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Dikkate alınan diğer çeşitler:

`async void`izin veriliyor.  Kodun doğrudan çağrılması için semantiği aynı tutmanız gerekir, bu, oluşturulan bir giriş noktasının çağrı yapmasını zorlaştırır (görev döndürülmez).  Bunun gibi iki farklı yöntem oluşturarak bunu çözebiliriz.

```csharp
public static async void Main()
{
   ... // await code
}
```

geldiğinde

```csharp
public static async void Main() => await $MainTask();

private static void $EntrypointMain() => Main().GetAwaiter().GetResult();

private static async Task $MainTask()
{
    ... // await code
}
```

`async void`teşvik kullanımı konusunda da sorunlar vardır.

Ad olarak "Main" yerine "MainAsync" kullanılıyor.  Zaman uyumsuz sonek görev döndüren yöntemler için önerilse de öncelikle kitaplık işlevselliği hakkında, ana olmayan ve "Main" dışındaki ek giriş noktası adlarını destekleyen bu değer bu kadar önemli değildir.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

yok

## <a name="design-meetings"></a>Tasarım toplantıları

yok
