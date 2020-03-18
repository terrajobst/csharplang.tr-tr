---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484673"
---
# <a name="non-trailing-named-arguments"></a>Girintili olmayan adlandırılmış bağımsız değişkenler

## <a name="summary"></a>Özet
[summary]: #summary
Adlandırılmış bağımsız değişkenlerin, doğru konumlarına kullanıldıkları sürece, sonunda olmayan konumda kullanılmasına izin verin. Örneğin: `DoSomething(isEmployed:true, name, age);`.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Ana işlem, gereksiz bilgiler yazmamaktır. Bağımsız değişken (örneğin, `null`, `true`), kodu bir sıra dışı bırakmak yerine kodun açıklığa kavuşturululululululan bir bağımsız değişkeni (örneğin,,) adlandırmak yaygındır.
Aşağıdaki bağımsız değişkenlerin tümü de adlandırılmadığı takdirde şu anda izin verilmiyor (`CS1738`).

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

Bazı ek örnekler:
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

Bu, params ile de çalışır:
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

§ 7.5.1 (bağımsız değişken listeleri) içinde, şu anda spec şöyle diyor:
> *Bağımsız değişken adı* olmayan *bir bağımsız değişken* , __adlandırılmış bağımsız değişken__olarak adlandırılır; ancak *bağımsız değişken adı* olmayan *bağımsız değişken* bir __Konumsal bağımsız değişkendir__. *Bağımsız değişken listesindeki*adlandırılmış bir bağımsız değişkenden sonra konum bağımsız değişkeninin görünmesi hatadır.

Teklif, bu hatayı kaldırmak ve bir bağımsız değişken için karşılık gelen parametreyi bulmaya yönelik kuralları güncelleştirmedir (§ 7.5.1.1):

Bağımsız değişken-örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler listesi:
- [Varolan kurallar]
- Adlandırılmamış bir bağımsız değişken, konum dışı adlandırılmış bağımsız değişkenin veya adlandırılmış bir params bağımsız değişkeninin sonrasında olduğunda parametreye karşılık gelir.

Özellikle, bu, `M(c: false, valueB);``void M(bool a = true, bool b = true, bool c = true, );` çağırmasını önler. İlk bağımsız değişken, konum dışında kullanılır (bağımsız değişken ilk konumda kullanılır, ancak "c" adlı parametre üçüncü konumda bulunur), bu nedenle aşağıdaki bağımsız değişkenlerin adlandırılması gerekir.

Diğer bir deyişle, sonunda olmayan adlandırılmış bağımsız değişkenlere yalnızca ad ve konum aynı karşılık gelen parametreyi bulmayla sonuçlanmışsa izin verilir.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bu teklif, aşırı yükleme çözümünde adlandırılmış bağımsız değişkenlerle mevcut alt tzellikleri exacerbates. Örneğin:

```csharp
void M(int x, int y) { }
void M<T>(T y, int x) { }

void M2()
{
    M(3, 4);
    M(y: 3, x: 4); // Invokes M(int, int)
    M(y: 3, 4); // Invokes M<T>(T, int)
}
```

Parametreleri değiştirerek bu durumu bugün edinebilirsiniz:

```csharp
void M(int y, int x) { }
void M<T>(int x, T y) { }

void M2()
{
    M(3, 4);
    M(x: 3, y: 4); // Invokes M(int, int)
    M(3, y: 4); // Invokes M<T>(int, T)
}
```

Benzer şekilde, `void M(int a, int b)` ve `void M(int x, string y)`iki yöntemine sahipseniz, hatalı çağırma `M(x: 1, 2)` ikinci aşırı yüklemeyi temel alan bir tanı üretir ("' int ' türünden ' String ' türüne dönüştürülemez). Bu sorun, adlandırılmış bağımsız değişken sondaki bir konumda kullanıldığında zaten var.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Göz önünde bulundurulması gereken birkaç seçenek vardır:

- Durum Quo
- Ortasında belirli bir ad yazdığınızda sondaki bağımsız değişkenlerin tüm adlarını doldurmaya yönelik IDE yardımı sağlama.

Her ikisi de, bağımsız değişken listesinin başlangıcında yalnızca bir sabit değer için bir ada ihtiyaç duysanız bile birden fazla adlandırılmış bağımsız değişkeni tanıttıkları için daha ayrıntı düzeyi düşdüler.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a>Tasarım toplantıları
[ldm]: #ldm
Özelliği, ilke onayı ile 16 Mayıs 2017 ' de LDM 'ta kısaca tartışılır (teklife/prototipi taşımaya kadar Tamam). Ayrıca, 15 Haziran 2017 ' de kısaca ele alınmıştır.

İlk Tartışmayla ilgili https://github.com/dotnet/csharplang/issues/518, ön lük sorun ile Ilgilidir https://github.com/dotnet/csharplang/issues/570
