---
ms.openlocfilehash: a4b0fbbc600eaf1e705ad8e6bd9fcecb44100761
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484442"
---
# <a name="local-functions"></a>Yerel işlevler

Blok kapsamındaki C# işlevlerin bildirimini destekleyecek şekilde genişlettik. Yerel işlevler kapsayan kapsamdaki değişkenleri kullanabilir (yakalayabilir).

Derleyici, bir değeri atamadan önce yerel bir işlevin kullandığı değişkenleri algılamak için akış analizini kullanır. İşlevin her çağrısı, bu tür değişkenlerin kesinlikle atanmasını gerektirir. Benzer şekilde, derleyici hangi değişkenlerin dönüşte kesin olarak atandığını belirler. Bu tür değişkenler, yerel işlev çağrıldıktan sonra kesin olarak atanır olarak değerlendirilir.

Yerel işlevler, tanımdan önce bir sözlü noktadan çağrılabilir. Yerel işlev bildirimi deyimleri, erişilebilir olmadığında bir uyarıya neden olmaz.

TODO: _yazma özelliği_

## <a name="syntax-grammar"></a>Sözdizimi dilbilgisi

Bu dilbilgisi geçerli belirtim dilbilgisinde fark olarak temsil edilir.

```diff
declaration-statement
    : local-variable-declaration ';'
    | local-constant-declaration ';'
+   | local-function-declaration
    ;

+local-function-declaration
+   : local-function-header local-function-body
+   ;

+local-function-header
+   : local-function-modifiers? return-type identifier type-parameter-list?
+       ( formal-parameter-list? ) type-parameter-constraints-clauses
+   ;

+local-function-modifiers
+   : (async | unsafe)
+   ;

+local-function-body
+   : block
+   | arrow-expression-body
+   ;
```

Yerel işlevler kapsayan kapsamda tanımlanan değişkenleri kullanabilir. Geçerli uygulama, yerel bir işlev içinde okunan her değişkenin, kendi tanım noktasında yürütülerek kesinlikle atanmasını gerektirir. Ayrıca, yerel işlev tanımının herhangi bir kullanım noktasında "yürütülmüş" olması gerekir.

Bu bit ile denemeler yaptıktan sonra (örneğin, birbirini dışlayan iki özyinelemeli yerel işlev tanımlamak mümkün değildir), bu tarihten sonra kesin atamanın nasıl çalışmasını istiyoruz. Düzeltme (henüz uygulanmadı), yerel bir işlevde okunan tüm yerel değişkenlerin her yerel işlev çağrısında kesinlikle atanmasını sağlamalıdır. Bu aslında BT seslerinden daha hafif olur ve çalışır duruma getirmek için bir miktar kalan iş vardır. İşlem tamamlandıktan sonra, yerel işlevlerinizi kapsayan bloğunun sonuna taşıyabilirsiniz.

Yeni kesin atama kuralları, yerel bir işlevin dönüş türünü ele almasıyla uyumlu değildir, bu nedenle dönüş türünü destekleme desteğini kaldıracağız.

Yerel bir işlevi bir temsilciye dönüştürmediğiniz takdirde, yakalama değer türleri olan çerçevelere yapılır. Diğer bir deyişle, yakalama ile yerel işlevler kullanmanın herhangi bir GC basıncını edinmezsiniz.

### <a name="reachability"></a>Erişilebilirlik

Spec 'e ekliyoruz

> Deyim-Bodied lambda ifadesinin veya yerel işlevin gövdesi erişilebilir olarak kabul edilir.
