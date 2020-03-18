---
ms.openlocfilehash: 63dfdfee9ea6c16e162f483aa1298feed297daef
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484701"
---
# <a name="conditional-ref-expressions"></a>Koşullu başvuru ifadeleri

Bir başvuru değişkenini bir veya başka bir ifadeye koşullu olarak bağlama deseninin Şu anda ifade edilemez C#.

Tipik geçici çözüm, şunun gibi bir yöntemi tanıtmaktadır:

```csharp
ref T Choice(bool condition, ref T consequence, ref T alternative)
{
    if (condition)
    {
         return ref consequence;
    }
    else
    {
         return ref alternative;
    }
}
```

Tüm bağımsız değişkenlerin çağrı sitesinde değerlendirilmesi gerektiğinden, bu, Üçlü olarak tam bir değiştirme değildir.

Aşağıdakiler beklendiği gibi çalışmayacak:

```csharp
       // will crash with NRE because 'arr[0]' will be executed unconditionally
      ref var r = ref Choice(arr != null, ref arr[0], ref otherArr[0]);
```

Önerilen sözdizimi şöyle görünür:

```csharp
     <condition> ? ref <consequence> : ref <alternative>;
```

Yukarıdaki "Choice" girişimi, ref Üçlü kullanılarak _doğru şekilde_ yazılabilir:

```csharp
     ref var r = ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

Tercih edilen fark, sonuç ve alternatif ifadelere _gerçekten_ koşullu bir şekilde erişildiğine göre, ```arr == null``` bir kilitlenme görmemelidir.

Üçlü başvuru, hem alternatif hem de sonuç refs olan bir üçlü yerdir. Doğal olarak, sonuç/alternatif işlenenlerinin LValues olmasını gerektirir. Ayrıca, sonuç ve alternatifin birbirlerine dönüştürülebilir türleri olması gerekir.

İfadenin türü, normal üçlü için de bir şekilde hesaplanır. Yani. Sonuç ve alternatifin kimliği dönüştürülebilir, ancak farklı türlere sahip olması durumunda, varolan tür birleştirme kuralları geçerli olur.

Güvenle dönüş, koşullu işlenenleri karşı koruma olarak kabul edilir. Her birinin, tüm işlemi döndürmesi güvenli değilse, dönmek için güvenli değildir.

Ref Üçlü değeri bir LValue ve başvuruya göre geçirilebilir/atanabilir/döndürülebilir;

```csharp
     // pass by reference
     foo(ref (arr != null ? ref arr[0]: ref otherArr[0]));

     // return by reference
     return ref (arr != null ? ref arr[0]: ref otherArr[0]);
```

LValue olarak da atanabilir. 

```csharp
    // assign to
    (arr != null ? ref arr[0]: ref otherArr[0]) = 1;
```

Ref Üçlü, normal (Ref değil) bağlamda de kullanılabilir. Yalnızca normal bir üçlü kullansanız da bu yana ortak olmaz.

```csharp
     int x = (arr != null ? ref arr[0]: ref otherArr[0]);
```


___

Uygulama notları: 

Uygulamanın karmaşıklığı, orta ve büyük bir hata düzeltmesinin boyutu olarak görünebilir. -I. E çok pahalı değil.
Söz dizimi veya ayrıştırma için herhangi bir değişikliğe ihtiyacım olduğunu Düşünmiyorum.
Meta veri veya birlikte çalışma üzerinde hiçbir etkisi yoktur. Özelliği tamamen ifade tabanlıdır.
Hata ayıklama/PDB üzerinde herhangi bir etkisi yoktur
