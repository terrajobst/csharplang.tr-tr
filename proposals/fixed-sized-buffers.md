---
ms.openlocfilehash: b51f27b2f58fd19851c80beb9cedcbd32b80b165
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484589"
---
# <a name="fixed-sized-buffers"></a>Sabit boyutlu arabellekler

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

C# Dile sabit boyutlu arabellekler bildirmek için genel amaçlı ve güvenli bir mekanizma sağlar.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Günümüzde, kullanıcıların güvenli olmayan bağlamdaki sabit boyutlu arabellekler oluşturma yeteneği vardır. Bununla birlikte, kullanıcının işaretçilerle uğraşmak, el ile sınır denetimleri gerçekleştirmesi ve yalnızca sınırlı bir tür kümesini (`bool`, `byte`, `char`, `short`, `int`, `long`, `sbyte`, `ushort`, `uint`, `ulong`, `float`ve `double`) desteklemesini gerektirir.

En yaygın şikayet, sabit boyutlu arabelleklerin güvenli kodda dizinlenemez. Daha fazla tür kullanmamamasından dolayı ikincisi.

Birkaç küçük temks sayesinde, herhangi bir türü destekleyen, güvenli bir bağlamda kullanılabilen ve otomatik sınır denetimi gerçekleştirilen genel amaçlı sabit boyutlu arabellekler sağlayabiliriz.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Bunlardan biri, aşağıdakiler aracılığıyla güvenli bir sabit boyutlu arabellek bildirir:

```csharp
public fixed DXGI_RGB GammaCurve[1025];
```

Bildirim, aşağıdaki örneğe benzer şekilde derleyici tarafından bir iç gösterimine çevrilir

```csharp
[FixedBuffer(typeof(DXGI_RGB), 1024)]
public ConsoleApp1.<Buffer>e__FixedBuffer_1024<DXGI_RGB> GammaCurve;

// Pack = 0 is the default packing and should result in indexable layout.
[CompilerGenerated, UnsafeValueType, StructLayout(LayoutKind.Sequential, Pack = 0)]
struct <Buffer>e__FixedBuffer_1024<T>
{
    private T _e0;
    private T _e1;
    // _e2 ... _e1023
    private T _e1024;

    public ref T this[int index] => ref (uint)index <= 1024u ?
                                         ref RefAdd<T>(ref _e0, index):
                                         throw new IndexOutOfRange();
}
```

Sabit boyutlu arabellekler artık `fixed`kullanımını gerektirmediğinden, herhangi bir öğe türüne izin vermek mantıklı olur.  

> NOTE: `fixed` hala desteklenmeye devam eder, ancak yalnızca öğe türü `blittable`

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

* Geriye dönük uyumlulukla ilgili bazı sorunlar olabilir, ancak var olan sabit boyutlu arabelleklerin yalnızca temel türler seçimiyle birlikte çalışması durumunda, kullanıcının sabit arabelleği bir olarak kabul etmesi durumunda derleyicinin "yalnızca çalışır" olması gerekir. çağrısı.
* Uyumsuz yapıların eski derleyicisinden alanları gizlemek için biraz farklı `v2` kodlaması kullanması gerekebilir.
* Paket, genel türler için Il belirtiminde iyi tanımlı değil. Yaklaşım çalışırken, belgelenmemiş davranışta bordering olacaktır. Belgelenir ve benzer Monode aynı davranışa sahip olduğundan emin olun.
* Her uzunluk için ayrı bir tür belirtme (destekleniyorsa `readonly` alanları için bir diğeri) meta veriler üzerinde etki olur. Belirtilen uygulamadaki farklı boyutlardaki dizi dizilerinin altına göre bağlanacaktır.
* `ref` matematik bir şekilde doğrulanabilir değil (güvenli olmadığı için). Kullanımızın tamam olduğunu anlamak için doğrulama kurallarını güncelleştirmenin bir yolunu bulmamız gerekir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Yapılarınızı el ile bildirin ve Dizin oluşturucular oluşturmak için güvenli olmayan kod kullanın.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- `readonly`izin vermemiz gerekir mi?  (ReadOnly Dizin Oluşturucu ile)
- dizi başlatıcılara izin vermemiz gerekir mi?
- `fixed` anahtar sözcüğü gerekli mi?
- `foreach`?
- yalnızca yapılarda örnek alanlar mı?

## <a name="design-meetings"></a>Tasarım toplantıları

Bu teklifi etkileyen tasarım notlarının bağlantısını yapın ve hangi değişiklikler üzerinde olduğuna ilişkin bir tümcede açıklama yapın.