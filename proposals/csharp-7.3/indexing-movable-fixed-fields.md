---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484631"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a><span data-ttu-id="8cf95-101">Dizin oluşturma `fixed` alanları, taşınabilir/taşınamaz içerikten bağımsız olarak sabitleme gerektirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-101">Indexing `fixed` fields should not require pinning regardless of the movable/unmovable context.</span></span> #

<span data-ttu-id="8cf95-102">Değişikliğin bir hata düzeltmesinin boyutu vardır.</span><span class="sxs-lookup"><span data-stu-id="8cf95-102">The change has the size of a bug fix.</span></span> <span data-ttu-id="8cf95-103">7,3 içinde olabilir ve daha fazla işlem yaptığımız herhangi bir yönde çakışmaz.</span><span class="sxs-lookup"><span data-stu-id="8cf95-103">It can be in 7.3 and does not conflict with whatever direction we take further.</span></span>
<span data-ttu-id="8cf95-104">Bu değişiklik yalnızca, taşınamayacak `s` de aşağıdaki senaryonun çalışmasına izin veriliyor.</span><span class="sxs-lookup"><span data-stu-id="8cf95-104">This change is only about allowing the following scenario to work even though `s` is moveable.</span></span> <span data-ttu-id="8cf95-105">`s` Moveable olmadığında zaten geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-105">It is already valid when `s` is not moveable.</span></span> 

<span data-ttu-id="8cf95-106">NOTE: her iki durumda da `unsafe` bağlamı gerekir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-106">NOTE: in either case, it still requires `unsafe` context.</span></span> <span data-ttu-id="8cf95-107">Başlatılmamış verilerin okunması veya aralığın dışında olması mümkündür.</span><span class="sxs-lookup"><span data-stu-id="8cf95-107">It is possible to read uninitialized data or even out of range.</span></span> <span data-ttu-id="8cf95-108">Bu değişiklik değildir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-108">That is not changing.</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int p = s.myFixedField[5]; // indexing fixed-size array fields would be ok
    }
}
```

<span data-ttu-id="8cf95-109">Burada görmem gereken ana "zorluk" özelliği, belirtimde nasıl açıklanacaktır. Özellikle de, aşağıdakilerden sabitleme ihtiyacı olacağından.</span><span class="sxs-lookup"><span data-stu-id="8cf95-109">The main “challenge” that I see here is how to explain the relaxation in the spec. In particular, since the following would still need pinning.</span></span> <span data-ttu-id="8cf95-110">(`s` taşınamayacak ve alanı bir işaretçi olarak açık olarak kullandığımız için)</span><span class="sxs-lookup"><span data-stu-id="8cf95-110">(because `s` is moveable and we explicitly use the field as a pointer)</span></span>

```csharp
unsafe struct S
{
    public fixed int myFixedField[10];
}

class Program
{
    static S s;

    unsafe static void Main()
    {
        int* ptr = s.myFixedField; // taking a pointer explicitly still requires pinning.
        int p = ptr[5];
    }
}
```

<span data-ttu-id="8cf95-111">Bir nedenden dolayı, taşınabilir olduğunda hedefin sabitlenmesi gereken kod oluşturma stratejimizin yapıtı olmasının nedeni, her zaman yönetilmeyen bir işaretçiye dönüştürüyoruz ve böylece Kullanıcı `fixed` bildiri aracılığıyla PIN 'e zorlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="8cf95-111">One reason why we require pinning of the target when it is movable is the artifact of our code generation strategy, - we always convert to an unmanaged pointer and thus force the user to pin via `fixed` statement.</span></span> <span data-ttu-id="8cf95-112">Ancak, dizin oluşturma işlemi sırasında yönetilmeyene dönüştürme gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-112">However, conversion to unmanaged is unnecessary when doing indexing.</span></span> <span data-ttu-id="8cf95-113">Aynı güvenli olmayan işaretçi matematiği, alıcıyı yönetilen bir işaretçi biçiminde olduğunda eşit oranda uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-113">The same unsafe pointer math is equally applicable when we have the receiver in the form of a managed pointer.</span></span> <span data-ttu-id="8cf95-114">Bunu yapabiliriz, ara başvuru yönetilir (GC-izleniyor) ve sabitleme gereksizdir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-114">If we do that, then the intermediate ref is managed (GC-tracked) and the pinning is unnecessary.</span></span>

<span data-ttu-id="8cf95-115">Değişiklik https://github.com/dotnet/roslyn/pull/24966, bu gereksinimi karşılayan bir prototip PR 'dir.</span><span class="sxs-lookup"><span data-stu-id="8cf95-115">The change https://github.com/dotnet/roslyn/pull/24966 is a prototype PR that relaxes this requirement.</span></span>
