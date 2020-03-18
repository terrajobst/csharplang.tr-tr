---
ms.openlocfilehash: ac2b233eb703b5eea3bd2dfdbeeadd7494b0c695
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484673"
---
# <a name="non-trailing-named-arguments"></a><span data-ttu-id="5c94d-101">Girintili olmayan adlandırılmış bağımsız değişkenler</span><span class="sxs-lookup"><span data-stu-id="5c94d-101">Non-trailing named arguments</span></span>

## <a name="summary"></a><span data-ttu-id="5c94d-102">Özet</span><span class="sxs-lookup"><span data-stu-id="5c94d-102">Summary</span></span>
[summary]: #summary
<span data-ttu-id="5c94d-103">Adlandırılmış bağımsız değişkenlerin, doğru konumlarına kullanıldıkları sürece, sonunda olmayan konumda kullanılmasına izin verin.</span><span class="sxs-lookup"><span data-stu-id="5c94d-103">Allow named arguments to be used in non-trailing position, as long as they are used in their correct position.</span></span> <span data-ttu-id="5c94d-104">Örneğin: `DoSomething(isEmployed:true, name, age);`.</span><span class="sxs-lookup"><span data-stu-id="5c94d-104">For example: `DoSomething(isEmployed:true, name, age);`.</span></span>

## <a name="motivation"></a><span data-ttu-id="5c94d-105">Amacı</span><span class="sxs-lookup"><span data-stu-id="5c94d-105">Motivation</span></span>
[motivation]: #motivation

<span data-ttu-id="5c94d-106">Ana işlem, gereksiz bilgiler yazmamaktır.</span><span class="sxs-lookup"><span data-stu-id="5c94d-106">The main motivation is to avoid typing redundant information.</span></span> <span data-ttu-id="5c94d-107">Bağımsız değişken (örneğin, `null`, `true`), kodu bir sıra dışı bırakmak yerine kodun açıklığa kavuşturululululululan bir bağımsız değişkeni (örneğin,,) adlandırmak yaygındır.</span><span class="sxs-lookup"><span data-stu-id="5c94d-107">It is common to name an argument that is a literal (such as `null`, `true`) for the purpose of clarifying the code, rather than of passing arguments out-of-order.</span></span>
<span data-ttu-id="5c94d-108">Aşağıdaki bağımsız değişkenlerin tümü de adlandırılmadığı takdirde şu anda izin verilmiyor (`CS1738`).</span><span class="sxs-lookup"><span data-stu-id="5c94d-108">That is currently disallowed (`CS1738`) unless all the following arguments are also named.</span></span>

```csharp
DoSomething(isEmployed:true, name, age); // currently disallowed, even though all arguments are in position
// CS1738 "Named argument specifications must appear after all fixed arguments have been specified"
```

<span data-ttu-id="5c94d-109">Bazı ek örnekler:</span><span class="sxs-lookup"><span data-stu-id="5c94d-109">Some additional examples:</span></span>
```csharp
public void DoSomething(bool isEmployed, string personName, int personAge) { ... }

DoSomething(isEmployed:true, name, age); // currently CS1738, but would become legal
DoSomething(true, personName:name, age); // currently CS1738, but would become legal
DoSomething(name, isEmployed:true, age); // remains illegal
DoSomething(name, age, isEmployed:true); // remains illegal
DoSomething(true, personAge:age, personName:name); // already legal
```

<span data-ttu-id="5c94d-110">Bu, params ile de çalışır:</span><span class="sxs-lookup"><span data-stu-id="5c94d-110">This would also work with params:</span></span>
```csharp
public class Task
{
    public static Task When(TaskStatus all, TaskStatus any, params Task[] tasks);
}
Task.When(all: TaskStatus.RanToCompletion, any: TaskStatus.Faulted, task1, task2)
```

## <a name="detailed-design"></a><span data-ttu-id="5c94d-111">Ayrıntılı tasarım</span><span class="sxs-lookup"><span data-stu-id="5c94d-111">Detailed design</span></span>
[design]: #detailed-design

<span data-ttu-id="5c94d-112">§ 7.5.1 (bağımsız değişken listeleri) içinde, şu anda spec şöyle diyor:</span><span class="sxs-lookup"><span data-stu-id="5c94d-112">In §7.5.1 (Argument lists), the spec currently says:</span></span>
> <span data-ttu-id="5c94d-113">*Bağımsız değişken adı* olmayan *bir bağımsız değişken* , __adlandırılmış bağımsız değişken__olarak adlandırılır; ancak *bağımsız değişken adı* olmayan *bağımsız değişken* bir __Konumsal bağımsız değişkendir__.</span><span class="sxs-lookup"><span data-stu-id="5c94d-113">An *argument* with an *argument-name* is referred to as a __named argument__, whereas an *argument* without an *argument-name* is a __positional argument__.</span></span> <span data-ttu-id="5c94d-114">*Bağımsız değişken listesindeki*adlandırılmış bir bağımsız değişkenden sonra konum bağımsız değişkeninin görünmesi hatadır.</span><span class="sxs-lookup"><span data-stu-id="5c94d-114">It is an error for a positional argument to appear after a named argument in an *argument-list*.</span></span>

<span data-ttu-id="5c94d-115">Teklif, bu hatayı kaldırmak ve bir bağımsız değişken için karşılık gelen parametreyi bulmaya yönelik kuralları güncelleştirmedir (§ 7.5.1.1):</span><span class="sxs-lookup"><span data-stu-id="5c94d-115">The proposal is to remove this error and update the rules for finding the corresponding parameter for an argument (§7.5.1.1):</span></span>

<span data-ttu-id="5c94d-116">Bağımsız değişken-örnek oluşturucular, Yöntemler, Dizin oluşturucular ve temsilciler listesi:</span><span class="sxs-lookup"><span data-stu-id="5c94d-116">Arguments in the argument-list of instance constructors, methods, indexers and delegates:</span></span>
- <span data-ttu-id="5c94d-117">[Varolan kurallar]</span><span class="sxs-lookup"><span data-stu-id="5c94d-117">[existing rules]</span></span>
- <span data-ttu-id="5c94d-118">Adlandırılmamış bir bağımsız değişken, konum dışı adlandırılmış bağımsız değişkenin veya adlandırılmış bir params bağımsız değişkeninin sonrasında olduğunda parametreye karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="5c94d-118">An unnamed argument corresponds to no parameter when it is after an out-of-position named argument or a named params argument.</span></span>

<span data-ttu-id="5c94d-119">Özellikle, bu, `M(c: false, valueB);``void M(bool a = true, bool b = true, bool c = true, );` çağırmasını önler.</span><span class="sxs-lookup"><span data-stu-id="5c94d-119">In particular, this prevents invoking `void M(bool a = true, bool b = true, bool c = true, );` with `M(c: false, valueB);`.</span></span> <span data-ttu-id="5c94d-120">İlk bağımsız değişken, konum dışında kullanılır (bağımsız değişken ilk konumda kullanılır, ancak "c" adlı parametre üçüncü konumda bulunur), bu nedenle aşağıdaki bağımsız değişkenlerin adlandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5c94d-120">The first argument is used out-of-position (the argument is used in first position, but the parameter named "c" is in third position), so the following arguments should be named.</span></span>

<span data-ttu-id="5c94d-121">Diğer bir deyişle, sonunda olmayan adlandırılmış bağımsız değişkenlere yalnızca ad ve konum aynı karşılık gelen parametreyi bulmayla sonuçlanmışsa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="5c94d-121">In other words, non-trailing named arguments are only allowed when the name and the position result in finding the same corresponding parameter.</span></span>

## <a name="drawbacks"></a><span data-ttu-id="5c94d-122">Bulunmaktadır</span><span class="sxs-lookup"><span data-stu-id="5c94d-122">Drawbacks</span></span>
[drawbacks]: #drawbacks

<span data-ttu-id="5c94d-123">Bu teklif, aşırı yükleme çözümünde adlandırılmış bağımsız değişkenlerle mevcut alt tzellikleri exacerbates.</span><span class="sxs-lookup"><span data-stu-id="5c94d-123">This proposal exacerbates existing subtleties with named arguments in overload resolution.</span></span> <span data-ttu-id="5c94d-124">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5c94d-124">For instance:</span></span>

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

<span data-ttu-id="5c94d-125">Parametreleri değiştirerek bu durumu bugün edinebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5c94d-125">You could get this situation today by swapping the parameters:</span></span>

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

<span data-ttu-id="5c94d-126">Benzer şekilde, `void M(int a, int b)` ve `void M(int x, string y)`iki yöntemine sahipseniz, hatalı çağırma `M(x: 1, 2)` ikinci aşırı yüklemeyi temel alan bir tanı üretir ("' int ' türünden ' String ' türüne dönüştürülemez).</span><span class="sxs-lookup"><span data-stu-id="5c94d-126">Similarly, if you have two methods `void M(int a, int b)` and `void M(int x, string y)`, the mistaken invocation `M(x: 1, 2)` will produce a diagnostic based on the second overload ("cannot convert from 'int' to 'string'").</span></span> <span data-ttu-id="5c94d-127">Bu sorun, adlandırılmış bağımsız değişken sondaki bir konumda kullanıldığında zaten var.</span><span class="sxs-lookup"><span data-stu-id="5c94d-127">This problem already exists when the named argument is used in a trailing position.</span></span>

## <a name="alternatives"></a><span data-ttu-id="5c94d-128">Alternatifler</span><span class="sxs-lookup"><span data-stu-id="5c94d-128">Alternatives</span></span>
[alternatives]: #alternatives

<span data-ttu-id="5c94d-129">Göz önünde bulundurulması gereken birkaç seçenek vardır:</span><span class="sxs-lookup"><span data-stu-id="5c94d-129">There are a couple of alternatives to consider:</span></span>

- <span data-ttu-id="5c94d-130">Durum Quo</span><span class="sxs-lookup"><span data-stu-id="5c94d-130">The status quo</span></span>
- <span data-ttu-id="5c94d-131">Ortasında belirli bir ad yazdığınızda sondaki bağımsız değişkenlerin tüm adlarını doldurmaya yönelik IDE yardımı sağlama.</span><span class="sxs-lookup"><span data-stu-id="5c94d-131">Providing IDE assistance to fill-in all the names of trailing arguments when you type specific a name in the middle.</span></span>

<span data-ttu-id="5c94d-132">Her ikisi de, bağımsız değişken listesinin başlangıcında yalnızca bir sabit değer için bir ada ihtiyaç duysanız bile birden fazla adlandırılmış bağımsız değişkeni tanıttıkları için daha ayrıntı düzeyi düşdüler.</span><span class="sxs-lookup"><span data-stu-id="5c94d-132">Both of those suffer from more verbosity, as they introduce multiple named arguments even if you just need one name of a literal at the beginning of the argument list.</span></span>

## <a name="unresolved-questions"></a><span data-ttu-id="5c94d-133">Çözümlenmemiş sorular</span><span class="sxs-lookup"><span data-stu-id="5c94d-133">Unresolved questions</span></span>
[unresolved]: #unresolved-questions

## <a name="design-meetings"></a><span data-ttu-id="5c94d-134">Tasarım toplantıları</span><span class="sxs-lookup"><span data-stu-id="5c94d-134">Design meetings</span></span>
[ldm]: #ldm
<span data-ttu-id="5c94d-135">Özelliği, ilke onayı ile 16 Mayıs 2017 ' de LDM 'ta kısaca tartışılır (teklife/prototipi taşımaya kadar Tamam).</span><span class="sxs-lookup"><span data-stu-id="5c94d-135">The feature was briefly discussed in LDM on May 16th 2017, with approval in principle (ok to move to proposal/prototype).</span></span> <span data-ttu-id="5c94d-136">Ayrıca, 15 Haziran 2017 ' de kısaca ele alınmıştır.</span><span class="sxs-lookup"><span data-stu-id="5c94d-136">It was also briefly discussed on June 28th 2017.</span></span>

<span data-ttu-id="5c94d-137">İlk Tartışmayla ilgili https://github.com/dotnet/csharplang/issues/518, ön lük sorun ile Ilgilidir https://github.com/dotnet/csharplang/issues/570</span><span class="sxs-lookup"><span data-stu-id="5c94d-137">Relates to initial discussion https://github.com/dotnet/csharplang/issues/518 Relates to championed issue https://github.com/dotnet/csharplang/issues/570</span></span>
