---
ms.openlocfilehash: a8822137c85f449444ed675c6f2912315c041691
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484477"
---
# <a name="static-delegates"></a>Statik temsilciler

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

C# Dile genel amaçlı, hafif bir geri çağırma özelliği sağlayın.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Günümüzde, kullanıcıların `System.Delegate` türünü kullanarak geri çağrılar oluşturma yeteneği vardır. Ancak bunlar oldukça ağır (yığın ayırmayı gerektirme ve zincirleme geri çağırmaların birlikte her zaman işleme için).

Ayrıca, `System.Delegate` yönetilmeyen işlev işaretçileriyle en iyi birlikte çalışabilirliği sağlamaz, bunun nedeni, blittable olmayan ve yönetilen/yönetilmeyen sınırın kesiştiği her zaman sıralama gerektirmektir.

Birkaç küçük tnak ile, yerel kod ile hafif, genel amaçlı ve işlem temelli yeni bir temsilci türü sağlayabiliriz.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

Biri, aşağıdaki aracılığıyla bir statik temsilci bildirir:

```C#
static delegate int Func()
```

Bunun yanı sıra, çağrı kuralının, dize uzayları ve ayarlanan son hata davranışının denetlenebilmesi için `System.Runtime.InteropServices.UnmanagedFunctionPointer` benzer bir şekilde bildirime öznitelik ekleyebilirsiniz. Not: yalnızca Temsilcilerde kullanılabilir olduğundan `System.Runtime.InteropServices.UnmanagedFunctionPointer` kendisini kullanmak işe alınacaktır.

Bildirim, aşağıdaki örneğe benzer şekilde derleyici tarafından bir iç gösterimine çevrilir

```C#
struct <Func>e__StaticDelegate
{
    IntPtr pFunction;

    static int WellKnownCompilerName();
}
```

Yani, `IntPtr` türünde tek bir üyeye sahip bir struct tarafından temsil edilir (böyle bir struct blittable ve herhangi bir yığın ayırması uygulamaz):
* Üye, geri çağırma olacak işlevin adresini içerir.
* Tür, geri aramanın Yöntem imzasıyla eşleşen bir yöntem bildirir.
* Yapının adı Kullanıcı tarafından oluşturulabilir (diğer dahili yedekleme yapıları ile yaptığımız gibi).
 * Örneğin: sabit boyutlu arabellekler `<name>e__FixedBuffer` biçiminde bir ada sahip bir struct oluşturur (`<` ve `>` tanımlayıcının bir parçasıdır ve tanımlayıcı, hala Il 'de oluşturulabilir C#).
* Yöntem bildiriminin adı, tüm statik temsilci türlerinde kullanılan iyi bilinen bir ad olmalıdır (Bu, derleyicinin imzayı belirlerken bakabileceği adı bilmesini sağlar).

Statik temsilcinin değeri yalnızca geri aramanın imzasıyla eşleşen bir statik metoda bağlanabilir.

Birlikte geri çağırmaları birlikte zincirleme desteklenmez.

Geri aramanın çağrılması `calli` yönergesi tarafından uygulanır.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Statik temsilciler, normal temsilciler kullanan var olan API 'Ler ile çalışmaz (bir diğeri aynı imzaya sahip olan normal bir temsilciyle aynı statik temsilciyi sarmalıdır).
* `System.Delegate`, `object` ve `IntPtr` alanları kümesi olarak dahili olarak temsil edildiği için (http://source.dot.net/#System.Private.CoreLib/src/System/Delegate.cs), statik bir temsilcinin eşleşen Yöntem imzasına sahip bir `System.Delegate` örtük dönüştürülmesine izin vermek mümkün olacaktır. Ayrıca, bir statik temsilci olan tüm gereksinimlere `System.Delegate` sağlandığı için ters yönde açık bir dönüştürme sağlamak mümkün olacaktır.

Statik temsilciyi çekirdek çerçevede kullanıma hazır hale getirmek için ek çalışma gerekir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

TBD

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

Tasarımın bölümleri yine de TBD midir?

## <a name="design-meetings"></a>Tasarım toplantıları

Bu teklifi etkileyen tasarım notlarının bağlantısını yapın ve hangi değişiklikler üzerinde olduğuna ilişkin bir tümcede açıklama yapın.


