---
ms.openlocfilehash: 340a1dc5a2eac653458d7d29f74551e5fe28b6d5
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484561"
---
# <a name="operators-should-be-exposed-for-systemintptr-and-systemuintptr"></a>İşleçler `System.IntPtr` ve `System.UIntPtr` için sunulmalıdır

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

CLR, `System.IntPtr` ve `System.UIntPtr` türleri (`native int`) için bir işleçler kümesini destekler. Bu işleçler, ortak dil altyapısı belirtiminin (`ECMA-335`) `III.1.5` görünebilirler. Ancak, bu işleçler tarafından C#desteklenmez.

`System.IntPtr` ve `System.UIntPtr`tarafından desteklenen işleçlerin tam kümesi için dil desteği sağlanmalıdır. Bu işleçler şunlardır: `Add`, `Divide`, `Multiply`, `Remainder`, `Subtract`, `Negate`, `Equals`, `Compare`, `And`, `Not`, `Or`, `XOr`, `ShiftLeft`, `ShiftRight`.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Günümüzde, kullanıcılar çeşitli araçları ve C# çerçeveleri kullanarak birden çok platformu hedefleyen uygulamaları kolayca yazabilir: `Xamarin`, `.NET Core`, `Mono`, vb...

Platformlar arası kod yazarken, genellikle belirli bir hedef platformla etkileşim kuran birlikte çalışma kodunu belirli bir şekilde yazmak gerekir. Bu, grafik kodu yazmayı, bazı sistem API 'sini çağırmayı veya var olan bir yerel kitaplıkla etkileşim kurmayı içerebilir.

Bu birlikte çalışma kodu, genellikle işleyiciler, yönetilmeyen bellek veya hatta platforma özgü boyut tamsayılarla uğraşmak zorunda olur.

Çalışma zamanı, `native int` (`System.IntPtr`) ve `native unsigned int` (`System.UIntPtr`) ilkel türlerinde kullanılabilecek bir işleç kümesi tanımlayarak bu için destek sağlar.

C#Bu işleçleri hiçbir şekilde desteklemez ve kullanıcıların sorunu geçici olarak çözmek zorunda kalmaları gerekir. Bu genellikle kod karmaşıklığını artırır ve kod bakımlığını düşürür.

Bu nedenle, dilin bu gereksinimleri daha iyi desteklemesi için bu işleçleri desteklemeye başlaması gerekir.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Desteklenen işleçlerin tam kümesi, ortak dil altyapısı belirtiminin (`ECMA-335`) `III.1.5` tanımlanmıştır. Bu belirtim şurada bulunabilir: [https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-335.pdf).

* İşleçlerin özeti aşağıda verilmiştir.
* CLı belirtimi tarafından tanımlanan doğrulanamayan işleçler listelenmez ve şu anda bu teklifin bir parçası değildir (ancak bunları göz önünde bulundurmakla aynı olabilir).
* Bir anahtar sözcük (`nint` ve `nuint`gibi) ve `System.IntPtr` için belirtilecek sabit değerler için bir yol sağlar ve `System.UIntPtr` (örneğin, 0n) bu teklifin bir parçası değildir (ancak bunları göz önünde bulundurabilse de).

### <a name="unary-plus-operator"></a>Birli Plus Işleci

```csharp
System.IntPtr operator +(System.IntPtr)
```

```csharp
System.UIntPtr operator +(System.UIntPtr)
```

### <a name="unary-minus-operator"></a>Birli eksi Işleci

```csharp
System.IntPtr operator -(System.IntPtr)
```

### <a name="bitwise-complement-operator"></a>Bit düzeyinde tamamlama Işleci

```csharp
System.IntPtr operator ~(System.IntPtr)
```

```csharp
System.UIntPtr operator ~(System.UIntPtr)
```

### <a name="cast-operators"></a>Atama İşleçleri

```csharp
explicit operator sbyte(System.IntPtr)               // Truncate
explicit operator short(System.IntPtr)               // Truncate
explicit operator int(System.IntPtr)                 // Truncate
explicit operator long(System.IntPtr)                // Sign Extend

explicit operator byte(System.IntPtr)                // Truncate
explicit operator ushort(System.IntPtr)              // Truncate
explicit operator uint(System.IntPtr)                // Truncate
explicit operator ulong(System.IntPtr)               // Zero Extend

explicit operator System.IntPtr(int)                 // Sign Extend
explicit operator System.IntPtr(long)                // Truncate

explicit operator System.IntPtr(uint)                // Sign Extend
explicit operator System.IntPtr(ulong)               // Truncate

explicit operator System.IntPtr(System.IntPtr)
explicit operator System.IntPtr(System.UIntPtr)
```

```csharp
explicit operator sbyte(System.UIntPtr)               // Truncate
explicit operator short(System.UIntPtr)               // Truncate
explicit operator int(System.UIntPtr)                 // Truncate
explicit operator long(System.UIntPtr)                // Sign Extend

explicit operator byte(System.UIntPtr)                // Truncate
explicit operator ushort(System.UIntPtr)              // Truncate
explicit operator uint(System.UIntPtr)                // Truncate
explicit operator ulong(System.UIntPtr)               // Zero Extend

explicit operator System.UIntPtr(int)                 // Zero Extend
explicit operator System.UIntPtr(long)                // Truncate

explicit operator System.UIntPtr(uint)                // Zero Extend
explicit operator System.UIntPtr(ulong)               // Truncate

explicit operator System.UIntPtr(System.IntPtr)
explicit operator System.UIntPtr(System.UIntPtr)
```

### <a name="multiplication-operator"></a>Çarpma Işleci

```csharp
System.IntPtr operator *(int, System.IntPtr)
System.IntPtr operator *(System.IntPtr, int)
System.IntPtr operator *(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator *(uint, System.UIntPtr)
System.UIntPtr operator *(System.UIntPtr, uint)
System.UIntPtr operator *(System.UIntPtr, System.UIntPtr)
```

### <a name="division-operator"></a>Bölme Işleci

```csharp
System.IntPtr operator /(int, System.IntPtr)
System.IntPtr operator /(System.IntPtr, int)
System.IntPtr operator /(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator /(uint, System.UIntPtr)
System.UIntPtr operator /(System.UIntPtr, uint)
System.UIntPtr operator /(System.UIntPtr, System.UIntPtr)
```

### <a name="remainder-operator"></a>Kalan Işleç

```csharp
System.IntPtr operator %(int, System.IntPtr)
System.IntPtr operator %(System.IntPtr, int)
System.IntPtr operator %(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator %(uint, System.UIntPtr)
System.UIntPtr operator %(System.UIntPtr, uint)
System.UIntPtr operator %(System.UIntPtr, System.UIntPtr)
```

### <a name="addition-operator"></a>Toplama Işleci

```csharp
System.IntPtr operator +(int, System.IntPtr)
System.IntPtr operator +(System.IntPtr, int)
System.IntPtr operator +(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator +(uint, System.UIntPtr)
System.UIntPtr operator +(System.UIntPtr, uint)
System.UIntPtr operator +(System.UIntPtr, System.UIntPtr)
```

### <a name="subtraction-operator"></a>Çıkarma Işleci

```csharp
System.IntPtr operator -(int, System.IntPtr)
System.IntPtr operator -(System.IntPtr, int)
System.IntPtr operator -(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator -(uint, System.UIntPtr)
System.UIntPtr operator -(System.UIntPtr, uint)
System.UIntPtr operator -(System.UIntPtr, System.UIntPtr)
```

### <a name="shift-operators"></a>Kaydırma İşleçleri

```csharp
System.IntPtr operator <<(System.IntPtr, int)
System.IntPtr operator >>(System.IntPtr, int)
```

```csharp
System.UIntPtr operator <<(System.UIntPtr, int)
System.UIntPtr operator >>(System.UIntPtr, int)
```

### <a name="integer-comparison-operators"></a>Tamsayı karşılaştırma Işleçleri

```csharp
bool operator ==(int, System.IntPtr)
bool operator ==(System.IntPtr, int)
bool operator ==(System.IntPtr, System.IntPtr)

bool operator !=(int, System.IntPtr)
bool operator !=(System.IntPtr, int)
bool operator !=(System.IntPtr, System.IntPtr)

bool operator  <(int, System.IntPtr)
bool operator  <(System.IntPtr, int)
bool operator  <(System.IntPtr, System.IntPtr)

bool operator  >(int, System.IntPtr)
bool operator  >(System.IntPtr, int)
bool operator  >(System.IntPtr, System.IntPtr)

bool operator <=(int, System.IntPtr)
bool operator <=(System.IntPtr, int)
bool operator <=(System.IntPtr, System.IntPtr)

bool operator >=(int, System.IntPtr)
bool operator >=(System.IntPtr, int)
bool operator >=(System.IntPtr, System.IntPtr)
```

```csharp
bool operator ==(uint, System.UIntPtr)
bool operator ==(System.UIntPtr, uint)
bool operator ==(System.UIntPtr, System.UIntPtr)

bool operator !=(uint, System.UIntPtr)
bool operator !=(System.UIntPtr, uint)
bool operator !=(System.UIntPtr, System.UIntPtr)

bool operator  <(uint, System.UIntPtr)
bool operator  <(System.UIntPtr, uint)
bool operator  <(System.UIntPtr, System.UIntPtr)

bool operator  >(uint, System.UIntPtr)
bool operator  >(System.UIntPtr, uint)
bool operator  >(System.UIntPtr, System.UIntPtr)

bool operator <=(uint, System.UIntPtr)
bool operator <=(System.UIntPtr, uint)
bool operator <=(System.UIntPtr, System.UIntPtr)

bool operator >=(uint, System.UIntPtr)
bool operator >=(System.UIntPtr, uint)
bool operator >=(System.UIntPtr, System.UIntPtr)
```

### <a name="integer-logical-operators"></a>Tamsayı mantıksal Işleçleri

```csharp
System.IntPtr operator &(int, System.IntPtr)
System.IntPtr operator &(System.IntPtr, int)
System.IntPtr operator &(System.IntPtr, System.IntPtr)

System.IntPtr operator |(int, System.IntPtr)
System.IntPtr operator |(System.IntPtr, int)
System.IntPtr operator |(System.IntPtr, System.IntPtr)

System.IntPtr operator ^(int, System.IntPtr)
System.IntPtr operator ^(System.IntPtr, int)
System.IntPtr operator ^(System.IntPtr, System.IntPtr)
```

```csharp
System.UIntPtr operator &(uint, System.UIntPtr)
System.UIntPtr operator &(System.UIntPtr, uint)
System.UIntPtr operator &(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator |(uint, System.UIntPtr)
System.UIntPtr operator |(System.UIntPtr, uint)
System.UIntPtr operator |(System.UIntPtr, System.UIntPtr)

System.UIntPtr operator ^(uint, System.UIntPtr)
System.UIntPtr operator ^(System.UIntPtr, uint)
System.UIntPtr operator ^(System.UIntPtr, System.UIntPtr)
```

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Bu işleçlerin gerçek kullanımı küçük ve daha düşük düzeydeki kitaplıklar ya da birlikte çalışma kodu yazan son kullanıcılarla sınırlı olabilir. Çoğu son kullanıcı büyük olasılıkla yerel boyutlu tamsayılar, işleyiciler ve birlikte çalışma kodu soyut olacak şekilde bu alt düzey kitaplıkları tüketiyor olabilir. Bu nedenle, işleçlerin kendilerine ihtiyacı yoktur.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Framework 'ü doğrudan Il 'de yazarak gerekli işleçleri uygular. Ayrıca, çalışma zamanı Framework tarafından tanımlanan operatörler için iç destek sağlayabilir. bu nedenle, nihai performansı daha iyi en iyi hale getirebilirsiniz.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

Tasarımın bölümleri yine de TBD midir?

## <a name="design-meetings"></a>Tasarım toplantıları

Bu teklifi etkileyen tasarım notlarının bağlantısını yapın ve hangi değişiklikler üzerinde olduğuna ilişkin bir tümcede açıklama yapın.