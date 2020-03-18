---
ms.openlocfilehash: 1852356b830e29c3537a4de559fef32fd2c2f8b6
ms.sourcegitcommit: f7952cdddf85316a4beec493a0ecc14fcb3241c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "79484988"
---
# <a name="target-typed-default-literal"></a>Target türü "default" değişmez değeri

* [x] önerilir
* [x] prototipi
* [x] uygulama
* [] Belirtimi

## <a name="summary"></a>Özet
[summary]: #summary

Hedef türü belirlenmiş `default` özelliği, türün Atlanmasının izin verdiği `default(T)` işlecinin daha kısa bir çeşitlemesi türüdür. Bunun yerine, türü hedef yazma tarafından algılanır. Bunun yanında, `default(T)`gibi davranır.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Ana işlem, gereksiz bilgiler yazmamaktır.

Örneğin, `void Method(ImmutableArray<SomeType> array)`çağrılırken *varsayılan* değişmez değer `M(default(ImmutableArray<SomeType>))`yerine `M(default)` izin verir.

Bu, aşağıdaki gibi çeşitli senaryolarda geçerlidir:

- yerelleri bildirme (`ImmutableArray<SomeType> x = default;`)
- Üçlü işlemler (`var x = flag ? default : ImmutableArray<SomeType>.Empty;`)
- yöntemlere ve lambdalara dönme (`return default;`)
- isteğe bağlı parametreler için varsayılan değerleri bildirme (`void Method(ImmutableArray<SomeType> arrayOpt = default)`)
- dizi oluşturma ifadelerinde varsayılan değerleri ekleme (`var x = new[] { default, ImmutableArray.Create(y) };`)


## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

*Varsayılan* değişmez değer olan yeni bir ifade tanıtılmıştır. Bu sınıflandırmayla bir ifade, *varsayılan değişmez değer dönüşümünden*örtük olarak herhangi bir türe dönüştürülebilir. 

*Varsayılan* değişmez değerin çıkarımı, herhangi bir türe izin verilmesi (yalnızca başvuru türleri değil) dışında, *null* değişmez değeri ile aynı şekilde çalışıyor.

Bu dönüştürme, çıkarılan türün varsayılan değerini üretir.

*Varsayılan* değişmez değer, çıkarılan türe bağlı olarak sabit bir değere sahip olabilir. `const int x = default;` geçerlidir, ancak `const int? y = default;` değildir.

*Varsayılan* değişmez değer, diğer işlenenin bir türü olduğu sürece eşitlik işleçlerinin işleneni olabilir. `default == x` ve `x == default` geçerli ifadelerdir, ancak `default == default` geçersizdir.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Küçük bir sakıncası, çoğu bağlamdaki *null* sabit değer yerine *varsayılan* değişmez değer kullanılabilir. Özel durumların ikisi `throw null;` ve `null == null`, ancak *varsayılan* değişmez değer değil, *null* değişmez değer için izin verilir.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Göz önünde bulundurulması gereken birkaç seçenek vardır:

- Durum Quo: Özellik kendi birleşme üzerinde hizalı değil ve geliştiriciler, varsayılan işleci açık bir türle kullanmaya devam eder.
- Null değişmez değer genişletiliyor: Bu, `Nothing`VB yaklaşımıdır. `int x = null;`izin vereceğiz.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- [x], ya da işleç *olarak* *varsayılan* olarak izin verilmelidir *mi?* Cevap: izin verme `default is T`, izin verme `x is default`, `default as RefType` izin verme (her zaman null uyarılarla)

## <a name="design-meetings"></a>Tasarım toplantıları

- [LDM 3/7/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-07.md)
- [LDM 3/28/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-03-28.md)
- [LDM 5/31/2017](https://github.com/dotnet/csharplang/blob/master/meetings/2017/LDM-2017-05-31.md#default-in-operators)
