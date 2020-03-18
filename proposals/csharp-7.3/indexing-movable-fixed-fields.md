---
ms.openlocfilehash: 36f3e6204d12c2569b3a55f3a47f58337e8a08e4
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484631"
---
# <a name="indexing-fixed-fields-should-not-require-pinning-regardless-of-the-movableunmovable-context"></a>Dizin oluşturma `fixed` alanları, taşınabilir/taşınamaz içerikten bağımsız olarak sabitleme gerektirmemelidir. #

Değişikliğin bir hata düzeltmesinin boyutu vardır. 7,3 içinde olabilir ve daha fazla işlem yaptığımız herhangi bir yönde çakışmaz.
Bu değişiklik yalnızca, taşınamayacak `s` de aşağıdaki senaryonun çalışmasına izin veriliyor. `s` Moveable olmadığında zaten geçerlidir. 

NOTE: her iki durumda da `unsafe` bağlamı gerekir. Başlatılmamış verilerin okunması veya aralığın dışında olması mümkündür. Bu değişiklik değildir.

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

Burada görmem gereken ana "zorluk" özelliği, belirtimde nasıl açıklanacaktır. Özellikle de, aşağıdakilerden sabitleme ihtiyacı olacağından. (`s` taşınamayacak ve alanı bir işaretçi olarak açık olarak kullandığımız için)

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

Bir nedenden dolayı, taşınabilir olduğunda hedefin sabitlenmesi gereken kod oluşturma stratejimizin yapıtı olmasının nedeni, her zaman yönetilmeyen bir işaretçiye dönüştürüyoruz ve böylece Kullanıcı `fixed` bildiri aracılığıyla PIN 'e zorlayacaktır. Ancak, dizin oluşturma işlemi sırasında yönetilmeyene dönüştürme gereksizdir. Aynı güvenli olmayan işaretçi matematiği, alıcıyı yönetilen bir işaretçi biçiminde olduğunda eşit oranda uygulanabilir. Bunu yapabiliriz, ara başvuru yönetilir (GC-izleniyor) ve sabitleme gereksizdir.

Değişiklik https://github.com/dotnet/roslyn/pull/24966, bu gereksinimi karşılayan bir prototip PR 'dir.
