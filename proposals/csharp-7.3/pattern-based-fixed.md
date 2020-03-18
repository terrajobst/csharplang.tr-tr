---
ms.openlocfilehash: 2070cf3b3269585055791adc3427cbd134df444d
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484659"
---
# <a name="pattern-based-fixed-statement"></a>Desen tabanlı `fixed` deyimi

## <a name="summary"></a>Özet
[summary]: #summary

Türlerin `fixed` ifadelerine katılmasına izin veren bir model ortaya çıkarabilir. 

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Dil, yönetilen verileri sabitleme ve temel alınan arabelleğe yerel bir işaretçi alma mekanizması sağlar.

```csharp
fixed(byte* ptr = byteArray)
{
   // ptr is a native pointer to the first element of the array
   // byteArray is protected from being moved/collected by the GC for the duration of this block 
}

```

`fixed` katılabilen türler kümesi sabit kodlanmış ve dizilerle ve `System.String`sınırlıdır. "Özel" türler, `ImmutableArray<T>`, `Span<T>``Utf8String` gibi yeni temel öğeler tanıtıldığında ölçeklenmez. 

Ayrıca, `System.String` için geçerli çözüm bir oldukça rigıd API 'sine dayanır. API 'nin şekli, `System.String`, UTF16 kodlu verileri nesne başlığından sabit bir uzaklığa katıştıran bitişik bir nesne olduğunu gösterir. Bu yaklaşım, temeldeki düzende değişiklik gerektirebilecek çeşitli tekliflerde sorun bulmuştur. Yönetilmeyen birlikte çalışma amacına göre `System.String` nesnesini iç gösteriminden ayırarak daha esnek bir şekilde geçebilmek istenebilir. 

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

## <a name="pattern"></a>*Desen* ##
Uygun bir model tabanlı "fixed" gereksinimi şunlar için gereklidir:
-   Örneği sabitlemek için yönetilen başvuruları sağlayın ve işaretçiyi başlatın (tercihen bu aynı başvurudur)
-   Yönetilmeyen öğenin türünü ("String" için "Char") kesin bir şekilde iletin
-   Başvurabileceğiniz bir şey olmadığında "boş" durumunda davranış göstermelidir. 
-   API yazarları `fixed`dışında türün kullanımını çıkarmayan tasarım kararlarına doğru göndermemelidir.

Yukarıdaki, özel olarak adlandırılmış bir başvuru döndüren üyeyi (`ref [readonly] T GetPinnableReference()`) tanımayı memnun hale getiriyorum.

`fixed` ifadesinin kullanılabilmesi için aşağıdaki koşulların karşılanması gerekir:

1. Bir tür için yalnızca bir tane belirtilen üye vardır.
1. `ref` veya `ref readonly`tarafından döndürülür. (`readonly`, sabit/salt okunur türlerin yazarlarına, güvenli kodda kullanılabilecek yazılabilir API eklemeden bir model uygulayabilmesi için izin verilir)
1. T, yönetilmeyen bir türdür.
(`T*` işaretçi türü haline gelir. Kısıtlama, "yönetilmeyen" kavramı genişletilmişse doğal olarak genişletilir.
1. Sabitlemeye veri olmadığında yönetilen `nullptr` döndürür; büyük olasılıkla emptiyi hale getirmenin bir yolu.
(dizeler null sonlandırıldığı için "" dizesinin ' \ 0 ' öğesine bir başvuru döndürdüğünü unutmayın)

`#3` için alternatif olarak, boş çalışmalardaki sonucun tanımsız veya uygulamaya özgü olması için izin verebilir. Bununla birlikte, API 'nin daha tehlikeli ve uygunsuz uyumluluk ve istenmeyen uyumlulukta olması olabilir. 

## <a name="translation"></a>*Çeviri* ##

```csharp
fixed(byte* ptr = thing)
{ 
    // <BODY>
}
```

Aşağıdaki sözde kod haline gelir (tüm ifade edilemez C#)

```csharp
byte* ptr;
// specially decorated "pinned" IL local slot, not visible to user code.
pinned ref byte _pinned;

try
{
    // NOTE: null check is omitted for value types 
    // NOTE: `thing` is evaluated only once (temporary is introduced if necessary) 
    if (thing != null)
    {
        // obtain and "pin" the reference
        _pinned = ref thing.GetPinnableReference();

        // unsafe cast in IL
        ptr = (byte*)_pinned;
    }
    else
    {
        ptr = default(byte*);
    }

    // <BODY> 
}
finally   // finally can be omitted when not observable
{
    // "unpin" the object
    _pinned = nullptr;
}
```

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

- GetPinnableReference yalnızca `fixed`için kullanılmak üzere tasarlanmıştır, ancak güvenli kodda kullanımını hiçbir şey engellemez, bu yüzden uygulayıcıyı göz önünde bulundurmanız gerekir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Kullanıcılar GetPinnableReference veya benzeri bir üyeyi ortaya çıkarabilir ve bunu şu şekilde kullanabilir
 
```csharp
fixed(byte* ptr = thing.GetPinnableReference())
{ 
    // <BODY>
}
```

Alternatif çözüm isteniyorsa `System.String` için çözüm yoktur.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [] "Boş" durumundaki davranış.`nullptr` veya `undefined`  - ? 
- [] Uzantı yöntemleri göz önünde bulundurulmalıdır mi? 
- [] `System.String`üzerinde bir model algılanırsa, bunun yerine gelmelidir mi? 

## <a name="design-meetings"></a>Tasarım toplantıları

Henüz yok. 
