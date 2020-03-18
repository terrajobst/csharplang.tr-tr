---
ms.openlocfilehash: f238a711e710bbac7f5b7400fa938bd85dec00c6
ms.sourcegitcommit: 5278336b61519956240a6f7d83bcb4322019e032
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79485254"
---
# <a name="support-for--and--on-tuple-types"></a>Tanımlama grubu türlerinde = = ve! = desteği

`t1 == t2`, `t1` ve `t2` aynı kardinalite türü veya null yapılabilir demet türleri olduğu ve bunları kabaca `temp1.Item1 == temp2.Item1 && temp1.Item2 == temp2.Item2` olarak değerlendirmek için (`var temp1 = t1; var temp2 = t2;`varsayıldığında) ifadeye izin verin.

Bunun tersine, `t1 != t2` izin verecek ve `temp1.Item1 != temp2.Item1 || temp1.Item2 != temp2.Item2`olarak değerlendirmelidir.

Null yapılabilir durumda, `temp1.HasValue` ve `temp2.HasValue` için ek denetimler kullanılır. Örneğin, `nullableT1 == nullableT2` `temp1.HasValue == temp2.HasValue ? (temp1.HasValue ? ... : true) : false`olarak değerlendirilir.

Öğe odaklı bir karşılaştırma bool olmayan bir sonuç döndürdüğünde (örneğin, bool olmayan kullanıcı tanımlı bir `operator ==` veya `operator !=` kullanıldığında ya da dinamik bir karşılaştırmayla), bu sonuç, `operator false` almak için `operator true` veya `bool`üzerinden `bool` veya çalıştırılacak şekilde değişir. Demet karşılaştırma her zaman `bool`döndürür.

C# 7,2 itibariyle, Kullanıcı tanımlı bir `operator==`olmadıkça bu tür kod bir hata (`error CS0019: Operator '==' cannot be applied to operands of type '(...)' and '(...)'`) oluşturur.

## <a name="details"></a>Ayrıntılar

`==` (veya `!=`) işlecini bağlarken, var olan kurallar: (1) dinamik durum, (2) aşırı yükleme çözünürlüğü ve (3) başarısız olur.
Bu teklif, (1) ve (2) arasında bir tanımlama grubu durumu ekler: bir karşılaştırma işlecinin her iki işleneni de tanımlama grubu ise (demet türlerine sahip veya demet sabit değerlerinde) ve eşleşen kardinalite varsa, karşılaştırma öğe temelinde gerçekleştirilir. Bu tanımlama grubu eşitliği de null yapılabilir diziler üzerine yükseltilmemiş.

Her iki işlenen de (ve kayıt kümesi değişmez değerleri, öğeleri) soldan sağa doğru sırayla değerlendirilir. Her öğe çifti, işleci `==` (veya `!=`) yinelemeli olarak bağlamak için işlenen olarak kullanılır. Derleme zamanı türü olan herhangi bir öğe `dynamic` hataya neden olur. Bu öğe temelinde karşılaştırmaların sonuçları, bir koşullu ve (veya veya) işleçler zincirinde işlenen olarak kullanılır.

Örneğin, `(int, (int, int)) t1, t2;`bağlamında `t1 == (1, (2, 3))` `temp1.Item1 == temp2.Item1 && temp1.Item2.Item1 == temp2.Item2.Item1 && temp2.Item2.Item2 == temp2.Item2.Item2`olarak değerlendirilir.

Bir tanımlama grubu sabit değeri işlenen olarak kullanıldığında (her iki tarafta), işleç `==` (veya `!=`) öğe temelinde bir kez eklendiğinde tanıtılan öğe temelinde dönüştürmeler tarafından oluşturulan dönüştürülmüş bir tanımlama grubu türü alır. 

Örneğin, `(1L, 2, "hello") == (1, 2L, null)`, her iki demet için de dönüştürülmüş tür `(long, long, string)` ve ikinci değişmez değer doğal bir türe sahip değildir.


### <a name="deconstruction-and-conversions-to-tuple"></a>Kayıt düzeni için ayrıştırma ve dönüştürmeleri
`(a, b) == x`, `x` iki öğe içinde oluşturabileceği ve bir rol oynamıyor. Bu, gelecekte `x == y` (basit bir karşılaştırma veya öğe temelinde karşılaştırma) ve bu durum, ne kardinalite kullanılması gerektiğini belirten sorular çalıp.
Benzer şekilde, kayıt düzeni dönüştürmeleri de rol içermez.

### <a name="tuple-element-names"></a>Demet öğesi adları

Bir tanımlama grubu değişmez değeri dönüştürülürken, sabit değerinde açık bir demet öğesi adı sağlandığında, ancak hedef demet öğe adıyla eşleşmez.
Kayıt düzeni karşılaştırmalarında aynı kuralı kullanıyoruz, bu sayede `t == (c, d: 0)``d` `(int a, int b) t` olduğunu varsayıyoruz.

### <a name="non-bool-element-wise-comparison-results"></a>Bool olmayan öğe temelinde karşılaştırma sonuçları

Öğe odaklı bir karşılaştırma, kayıt düzeni eşitliğine dinamik ise, işleç `false` dinamik bir şekilde çağırdık ve bir `bool` almak ve daha fazla öğe temelinde karşılaştırmayla devam etmek için Negate kullanırız. 

Öğe temelinde karşılaştırma, bir tanımlama grubu eşitliğine başka bir bool olmayan tür döndürürse iki durum vardır:
- bool olmayan tür `bool`olarak dönüştürürse, bu dönüştürme uygulanır,
- Böyle bir dönüştürme yoksa, ancak türün bir işleç `false`varsa, bunu kullanacağız ve sonucu kullanacaksınız.

Bir dizi eşitsizlik içinde, işleç `false`yerine işleci `true` (olumsuzlama olmadan) kullanacağımız için aynı kurallar geçerlidir.

Bu kurallar, `if` bildiriminde bool olmayan bir tür ve diğer mevcut bağlamların kullanılmasına benzer kurallara benzerdir.

## <a name="evaluation-order-and-special-cases"></a>Değerlendirme siparişi ve özel durumlar
Sol taraftaki değer önce, sonra sağ taraftaki değer ' i, sonra da soldan sağa karşılaştırmaları (dönüşümler dahil olmak üzere ve koşullu ve/veya operatörler için var olan kurallara göre erken çıkış ile) değerlendirilir.

Örneğin, tür `B` türüne `A` bir dönüştürme varsa ve bir yöntem `(A, A) GetTuple()`, `(new A(1), (new B(2), new B(3))) == (new B(4), GetTuple())` değerlendirmesi yapılır:
- `new A(1)`
- `new B(2)`
- `new B(3)`
- `new B(4)`
- `GetTuple()`
- sonra öğe temelinde dönüştürmeler ve karşılaştırmalar ve koşullu mantık değerlendirilir (`new A(1)` `B`türüne dönüştürüp `new B(4)`, vb. ile karşılaştırın).

### <a name="comparing-null-to-null"></a>`null` `null` karşılaştırma

Bu, normal karşılaştırmalardan, kayıt düzeni karşılaştırmalarının üzerinde yer alan özel bir durumdur. `null == null` karşılaştırmaya izin verilir ve `null` değişmez değerleri herhangi bir tür almaz.
Tanımlama grubu eşitliğine bu, `(0, null) == (0, null)` izin verilen ve `null` ve demet sabit değerlerinin bir tür alması anlamına da gelir.

### <a name="comparing-a-nullable-struct-to-null-without-operator"></a>Null yapılabilir bir yapıyı `operator==` olmadan `null` karşılaştırma

Bu, normal karşılaştırmalardan, kayıt düzeni karşılaştırmalarının üzerinde yer alan başka bir özel durumdur.
`operator==`olmayan bir `struct S` varsa `(S?)x == null` karşılaştırmaya izin verilir ve `((S?).x).HasValue`olarak yorumlanır.
Kayıt düzeni eşitliğine aynı kural uygulandı, bu nedenle `(0, (S?)x) == (0, null)` izin verilir.

## <a name="compatibility"></a>Uyumluluk

Biri karşılaştırma işlecinin uygulamasıyla kendi `ValueTuple` türlerini yazdıysa, daha önce aşırı yükleme çözümlemesi tarafından çekilmiş olur. Ancak yeni demet durumu aşırı yükleme çözümlenmesinden önce geldiğinden, bu durumu Kullanıcı tanımlı karşılaştırmaya güvenmek yerine demet karşılaştırmayla işleyeceğiz.

----

[İlişkisel ve tür testi işleçleri](../../spec/expressions.md#relational-and-type-testing-operators) [#190](https://github.com/dotnet/csharplang/issues/190) ile ilgilidir
