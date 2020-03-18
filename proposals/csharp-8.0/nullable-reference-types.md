---
ms.openlocfilehash: ecdad8c863d0695bc901e4d96d9ca3decbc248eb
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484617"
---
# <a name="nullable-reference-types-in-c"></a>İçinde null yapılabilir başvuru türleriC# #

Bu özelliğin amacı:

* Geliştiricilerin bir değişkenin, parametrenin veya başvuru türü sonucunun null olması amaçlanıp sunmadığını ifade etmelerine izin verin.
* Bu amaca göre bu tür değişkenler, parametreler ve sonuçlar kullanılmazsa Uyarılar sağlayın.

## <a name="expression-of-intent"></a>Amaç ifadesi

Dil, değer türleri için `T?` sözdizimini zaten içeriyor. Bu söz dizimini başvuru türlerine genişletmek basittir.

Benzersiz olmayan bir başvuru türü amacı `T` bunun null olmadığı varsayılır.

## <a name="checking-of-nullable-references"></a>Null yapılabilir başvuruların denetlenmesi

Akış Analizi, null yapılabilir başvuru değişkenlerini izler. Analiz, bu kişilerin null olmaması (örneğin, bir denetim veya atamadan sonra), bu değerin null olmayan bir başvuru olarak kabul edilir.

Null yapılabilir bir başvuru ayrıca, sonek `x!` işleci ("dammıt" işleci) ile null olmayan olarak kabul edilebilir, çünkü akış analizi, geliştiricinin orada olduğunu belirten null olmayan bir durum oluşturamıyor.

Aksi takdirde, bir null yapılabilir başvuruya başvuruluyorsa veya null olmayan bir türe dönüştürülürse bir uyarı verilir.

`S[]`, `T?[]` `S?[]` ve `T[]`arasında dönüştürme sırasında bir uyarı verilir.

Tür parametresi birlikte değişken (`out`) ve `C<S?>` dönüştürme sırasında tür parametresi değişken karşıtı (`C<T>`) olduğunda, `C<T?>` `C<S>` bir uyarı verilir.`in`

Tür parametresinde null olmayan kısıtlamalar varsa `C<T?>` bir uyarı verilir. 

## <a name="checking-of-non-null-references"></a>Null olmayan başvuruların denetlenmesi

Null olmayan bir değişkene atandığında veya null olmayan bir parametre olarak geçirilmemişse bir uyarı verilir.

Oluşturucu null olmayan başvuru alanlarını açıkça başlatmadıysanız bir uyarı da verilir.

Null olmayan başvuruların dizisinin tüm öğelerinin başlatıldığını yeterince izleyemedik. Ancak, yeni oluşturulan bir dizinin bir öğesi, dizi okunmadan veya geçirilmeden önce öğesine atanmamışsa bir uyarı verebilir. Bu, çok gürültülü olmayan ortak durumu işleyebilir.

`default(T)` bir uyarı mı üreteceğine karar vermemiz gerekir, yoksa yalnızca `T?`türünde olduğu kabul edilir.

## <a name="metadata-representation"></a>Meta veri gösterimi

Null olabilme manları öznitelik olarak meta verilerde temsil edilmelidir. Bu, alt düzey derleyicilerin bunları yoksayması anlamına gelir.

Yalnızca null yapılabilir ek açıklamaların dahil edilip edilmeyeceğini veya derlemede null olmayan "açık" olup olmadığını belirlemek istiyoruz.

## <a name="generics"></a>Genel Türler

Tür parametresinde `T` null yapılamayan kısıtlamalar varsa, kapsamı içinde null atanamaz olarak değerlendirilir.

Bir tür parametresi kısıtlanmamışsa veya yalnızca null yapılabilir kısıtlamalara sahipse, bu durum biraz daha karmaşıktır: Bu, karşılık gelen tür bağımsız *değişkeninin Nullable veya null yapılamayan olabileceği anlamına* gelir. Bu durumda yapılacak güvenli şey, tür parametresini hem null yapılabilir hem *de* null değer atanabilir olarak değerlendirmek, her iki durumda da uyarı vermek olur. 

Açık null yapılabilir başvuru kısıtlamalarına izin verilip verilmeyeceğini göz önünde bulundurmayı düşünün. Ancak, null yapılabilir başvuru türlerinin belirli durumlarda (devralınan kısıtlamalar) *örtük olarak* kısıtlamalar olmasını önlamadığımızda unutmayın.

`class` kısıtlaması null değil. `class?`, "Nullable başvuru türü" belirten geçerli bir null yapılabilir kısıtlama olup olmayacağını düşünebiliriz.

## <a name="type-inference"></a>Tür çıkarımı

Tür çıkarımı içinde, katkıda bulunan bir tür null yapılabilir bir başvuru türü ise, sonuçta elde edilen tür null yapılabilir olmalıdır. Diğer bir deyişle, nulllılık yayılır.

Bir katılım ifadesi olarak `null` sabit değerinin boş değer içerip içermediğini düşünmemiz gerekir. Bugün değil: bir hataya neden olan değer türleri için, ancak başvuru türleri için null değeri, düz türe başarıyla dönüştürülür. 

```csharp
string? n = "world";
var x = b ? "Hello" : n; // string?
var y = b ? "Hello" : null; // string? or error
var z = b ? 7 : null; // Error today, could be int?
```

## <a name="breaking-changes"></a>Yeni değişiklikler

Null olmayan uyarılar, mevcut kodda belirgin bir büyük değişiklik ve bir katılım mekanizması ile birlikte kullanılmalıdır.

Daha az açıktır, null yapılabilir türlerden uyarılar (yukarıda açıklandığı gibi), null değer alabilme 'in örtük olduğu belirli senaryolarda, var olan kodda önemli bir değişiklik olur:

* Kısıtlanmış olmayan tür parametreleri örtük olarak null yapılabilir olarak değerlendirilir, bu nedenle `object` veya erişim, örneğin `ToString`, uyarılar verecek.
* Tür çıkarımı `null` ifadelerden boş bir değer içeriyorsa, mevcut kod bazen null yapılamayan türler yerine null yapılabilir, bu da yeni uyarılara yol açabilir.

Bu nedenle null yapılabilir uyarıların da isteğe bağlı olması gerekir

Son olarak, var olan bir API 'ye ek açıklamalar eklemek, kitaplığı yükselttiğinizde uyarıları kabul eden kullanıcılara bir son değişiklik olacaktır. Bu, çok, kabul etme veya kapatma imkanını de sağlar. "Hata düzeltmelerini istiyorum, ancak yeni ek açıklamalarıyla uğraşmak için hazırlanma"

Özet bölümünde şunları kabul etmeniz gerekir:
* Null yapılabilir uyarılar
* Null olmayan uyarılar
* Diğer dosyalardaki ek açıklamaların uyarıları

Kabul etme özelliğinin ayrıntı düzeyi, swaths of Code 'un pragmalar ile kabul eteceği ve önem düzeyleri Kullanıcı tarafından seçilebilir şekilde, çözümleyici benzeri bir model önerir. Ayrıca, kitaplık başına seçenekler ("sonlanmaya hazır olana kadar JSON.NET ek açıklamalarını yoksayın"), kodda öznitelik olarak ifade edilebilir.

Katılım/geçiş deneyiminin tasarımı, bu özelliğin başarısı ve yararlılığı açısından önemlidir. Şunları yaptığınızdan emin olmak istiyoruz:

* Kullanıcılar, null olabilme denetlemesini
* Kitaplık yazarları, son müşterilerin korku olmadan null değer alabilirlik ek açıklamaları ekleyebilir
* Bunlara rağmen, "yapılandırmanızın gecemi" hakkında bir fikir yoktur

## <a name="tweaks"></a>Atabileceği

Yereller üzerinde `?` ek açıklamalarını kullanmayabilirsiniz, ancak bunlara atanan değerlere uygun olarak kullanılıp kullanılmayacağını gözlemliyoruz. Bunu tercih ediyorum; İnsanların amacını hızlı bir şekilde sunalım etmem gerektiğini düşündük.

Bir çalışma zamanı null denetimini otomatik olarak üreten parametreler üzerinde bir toplu `T! x` düşünebiliriz.

`FirstOrDefault` veya `TryGet`gibi genel türlerde bazı desenler, null olamayan tür bağımsız değişkenleriyle biraz tuhaf davranışa sahiptir, çünkü açıkça belirli durumlarda varsayılan değerler verir. Tür sistemini bu daha iyi uyum sağlayacak şekilde kullanmayı deneyebiliriz. Örneğin, tür bağımsız değişkeni zaten null yapılabilir olmasına rağmen kısıtlanmış tür parametrelerine `?` izin vereceğiz. Ona değdiğini biliyorum ve null yapılabilir *değer* türleriyle etkileşim ile ilgili olarak bir sorun doğurur. 

## <a name="nullable-value-types"></a>Boş değer atanabilen değer türleri

Null yapılabilir değer türleri için yukarıdaki semantiklerinden bazılarını benimseme de düşüneceğiz.

Yalnızca bir hata vermek yerine `(7, null)``int?` çıkarsanımız tür çıkarımı zaten belirtiliyor.

Farklı bir fırsat, akış analizinin null yapılabilir değer türlerine uygulanmasına olanak tanır. Null olmayan kabul edildiğinde, gerçekte null yapılamayan tür olarak kullanılmasına izin veririz (örn. üye erişimi). Yalnızca null yapılabilen bir değer türü üzerinde *zaten* yapabileceğiniz şeylerin, geriye doğru uyumluluk nedenleriyle tercih edildiğini dikkatli etmemiz gerekir.
