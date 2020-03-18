---
ms.openlocfilehash: aecbf4298053debdb479ad3aaf6e3db468918455
ms.sourcegitcommit: 02b535d712cc9e022290e14d9e0324508b28f231
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "79485212"
---
# <a name="nullable-reference-types-specification"></a>Null yapılabilir başvuru türleri belirtimi

Bu, devam eden bir çalışmadır. birkaç bölüm eksik veya eksik. ***

## <a name="syntax"></a>Sözdizimi

### <a name="nullable-reference-types"></a>Boş değer atanabilir başvuru türleri

Null yapılabilir başvuru türleri, null yapılabilir değer türlerinin kısa biçimiyle aynı `T?` sözdizimine sahiptir, ancak karşılık gelen uzun bir biçime sahip değildir.

Belirtim amaçları doğrultusunda, geçerli `nullable_type` üretimi `nullable_value_type`olarak yeniden adlandırılır ve bir `nullable_reference_type` üretimi eklenir:

```antlr
reference_type
    : ...
    | nullable_reference_type
    ;
    
nullable_reference_type
    : non_nullable_reference_type '?'
    ;
    
non_nullable_reference_type
    : type
    ;
```

Bir `nullable_reference_type` `non_nullable_reference_type` null yapılamayan bir başvuru türü (sınıf, arabirim, temsilci veya dizi) veya null yapılamayan bir başvuru türü (`class` kısıtlaması aracılığıyla veya `object`dışında bir sınıf) olarak kısıtlanmış bir tür parametresi olmalıdır.

Null yapılabilir başvuru türleri aşağıdaki konumlarda gerçekleşemez:

- temel sınıf veya arabirim olarak
- `member_access` alıcısı olarak
- bir `object_creation_expression` `type` olarak
- `delegate_creation_expression` `delegate_type` olarak
- bir `is_expression`, `catch_clause` veya `type_pattern` `type` olarak
- tam nitelikli arabirim üye adında `interface` olarak

Null yapılabilir ek açıklama bağlamının devre dışı olduğu `nullable_reference_type` bir uyarı verilir.

### <a name="nullable-class-constraint"></a>Nullable sınıf kısıtlaması

`class` kısıtlamasının null olabilen bir karşılığı `class?`vardır:

```antlr
primary_constraint
    : ...
    | 'class' '?'
    ;
```

### <a name="the-null-forgiving-operator"></a>Null-forverme işleci

Onarma sonrası `!` işlecine null-forverme işleci denir.

```antlr
primary_expression
    : ...
    | null_forgiving_expression
    ;
    
null_forgiving_expression
    : primary_expression '!'
    ;
```

`primary_expression` bir başvuru türünde olmalıdır.  

Sonek `!` işlecinin çalışma zamanı etkisi yoktur; temel alınan ifadenin sonucunu değerlendirir. Tek rolü, ifadenin null durumunu değiştirmek ve kullanım sırasında verilen uyarıları sınırlamak içindir.

### <a name="nullable-implicitly-typed-local-variables"></a>null atanabilir örtük olarak yazılan yerel değişkenler

`var`, başvuru türleri için açıklamalı bir tür.
Örneğin, `var s = "";` `var` `string?`olarak algılanır.

### <a name="nullable-compiler-directives"></a>Null yapılabilir derleyici yönergeleri

`#nullable` yönergeler null yapılabilir ek açıklama ve uyarı bağlamlarını denetler.

```antlr
pp_directive
    : ...
    | pp_nullable
    ;
    
pp_nullable
    : whitespace? '#' whitespace? 'nullable' whitespace nullable_action pp_new_line
    ;
    
nullable_action
    : 'disable'
    | 'enable'
    | 'restore'
    ;
```

`#pragma warning` yönergeler, null olabilen uyarı bağlamını değiştirmeye izin vermek ve varsayılan olarak devre dışı bırakılmış olsalar bile tek tek uyarıların etkinleştirilmesini sağlamak üzere genişletilir:

```antlr
pragma_warning_body
    : ...
    | 'warning' whitespace nullable_action whitespace 'nullable'
    ;

warning_action
    : ...
    | 'enable'
    ;
```

`pragma_warning_body` yeni biçiminin `warning_action`değil `nullable_action`kullandığını unutmayın.

## <a name="nullable-contexts"></a>Null yapılabilir bağlamlar

Her kaynak kodu satırında *null yapılabilir bir ek açıklama bağlamı* ve *null yapılabilir uyarı bağlamı*vardır. Bu denetim, null yapılabilir ek açıklamaların etkin olup olmadığını ve null olabilme uyarılarının verilip verilmediğini denetler. Verilen bir çizginin ek açıklama bağlamı *devre dışı* veya *etkin*. Verilen bir satırın uyarı bağlamı *devre dışı* veya *etkin*değil.

Her iki içerik de proje düzeyinde ( C# kaynak kodu dışında) veya kaynak dosya içinde herhangi bir yerde `#nullable` ön işlemci yönergeleri aracılığıyla belirtilebilir. Hiçbir proje düzeyi ayarı sağlanmazsa, varsayılan olarak her iki bağlam da *devre dışı*bırakılır.

`#nullable` yönergesi, kaynak metin içindeki ek açıklamaları ve uyarı bağlamlarını denetler ve proje düzeyi ayarlarından önceliklidir.

Bir yönerge, diğer bir yönerge tarafından geçersiz kılınana kadar veya kaynak dosyanın sonuna kadar, sonraki kod satırları için denetim bağlamını ayarlar.

Yönergelerin etkisi aşağıdaki gibidir:

- `#nullable disable`: Nullable ek açıklama ve uyarı bağlamlarını *devre dışı* olarak ayarlar
- `#nullable enable`: null yapılabilir ek açıklama ve uyarı bağlamlarını *etkin* olarak ayarlar
- `#nullable restore`: proje ayarlarına Nullable ek açıklama ve uyarı bağlamlarını geri yükler
- `#nullable disable annotations`: Nullable ek açıklama bağlamını *devre dışı* olarak ayarlar
- `#nullable enable annotations`: Nullable ek açıklama bağlamını *etkin* olarak ayarlar
- `#nullable restore annotations`: proje ayarlarına Nullable ek açıklama bağlamını geri yükler
- `#nullable disable warnings`: Nullable uyarı bağlamını *devre dışı* olarak ayarlar
- `#nullable enable warnings`: null yapılabilir uyarı bağlamını *etkin* olarak ayarlar
- `#nullable restore warnings`: proje ayarlarına Nullable uyarı bağlamını geri yükler

## <a name="nullability-of-types"></a>Türlerin null olabilme sayısı

Verili bir tür dört adet *null değer içerebilir*: *zorunlulukou*, Nullable, *null yapılabilir* ve *bilinmiyor*. 

*Null yapılamayan* ve *Bilinmeyen* türler, bunlara olası bir `null` değeri atandığında uyarılara neden olabilir. Ancak, *Zorunluluvou* ve *null yapılabilir* türler, "*null atanabilir*" ve uyarılar olmadan bunlara atanan `null` değerlere sahip olabilir. 

*Zorunluluvou* ve *null yapılamayan* türler uyarı vermeden bağlanabilir veya atanabilir. Null *yapılabilir* ve *Bilinmeyen* türlerin değerleri, "*null*olarak çıkarılıyor" ve doğru null denetimi yapılmadan başvurulmadan veya atandıktan sonra uyarılara neden olabilir. 

Null değerli bir türün *varsayılan null durumu* "Belki null" dır. Null olmayan bir türün varsayılan null durumu "Not null" olarak ayarlanır.

Tür türü ve null yapılabilir ek açıklama bağlamı, null olabilme değerini belirlemede oluşur:

- Null yapılamayan bir değer türü `S` her zaman *null atanamaz*
- Null yapılabilir bir değer türü `S?` her zaman *null yapılabilir*
- *Devre dışı* bir ek açıklama bağlamında `C` açıklamalı olmayan bir başvuru türü, *zorunluluvou*
- *Etkin* olmayan bir ek açıklama bağlamında `C` açıklamalı olmayan bir başvuru türü *null atanamaz*
- Null yapılabilir bir başvuru türü `C?` *null* olabilir (ancak bir uyarı, *devre dışı* bir ek açıklama bağlamında istenebilir)

Ayrıca tür parametrelerinin kısıtlamaları hesaba alır:

- Bir tür parametresi, tüm kısıtlamaların (varsa) null değerli türler (null*yapılabilir* ve *bilinmiyor*) ya da `class?` kısıtlaması *bilinmiyorsa* `T`.
- Bir tür parametresi, en az bir kısıtlamanın *zorunluluvou* veya *null yapılamayan* ya da `struct` veya `class` kısıtlamalarından biri olduğu `T`.
    - *devre dışı* bir ek açıklama bağlamındaki *yükümlülüğü*
    - *etkin* olmayan bir ek açıklama bağlamında *null değer atanamaz*
- Null olabilen bir tür parametresi, `T`kısıtlamalarından en az birinin *yükümlülüğü* *veya `struct`* ya da `class` kısıtlamalarından biri olduğu `T?`,
    - *devre dışı* bir ek açıklama bağlamında *boş değer atanabilir* (ancak bir uyarı uyarılandırılan)
    - *etkin* bir ek açıklama bağlamında *null yapılabilir*

Bir tür parametresi `T`için, `T?` yalnızca `T` bir değer türü olarak bilindiğinde veya bir başvuru türü olarak bilindiğinde izin verilir.

### <a name="oblivious-vs-nonnullable"></a>Zorunluluvou ile null atanamaz

Bir `type`, türün son belirteci bu bağlam içinde olduğunda, belirli bir ek açıklama bağlamında gerçekleşmelidir.

Kaynak kodda verilen bir başvuru türü `C`, yükümlülüğü veya null değer atanabilir olarak yorumlanıp bu kaynak kodun ek açıklama bağlamına bağlıdır. Ancak, bir kez kurulduktan sonra bu türün bir parçası olarak değerlendirilir ve "genel tür bağımsız değişkenlerinin yerine geçen" gibi. Tür üzerinde `?` gibi bir ek açıklama var, ancak görünmez.

## <a name="constraints"></a>Kısıtlamalar

Null yapılabilir başvuru türleri, genel kısıtlamalar olarak kullanılabilir. Ayrıca `object` artık açık bir kısıtlama olarak geçerlidir. Bir kısıtlamanın yokluğu artık `object?` kısıtlamasına eşdeğerdir (`object`değil), ancak (daha önce `object` aksine) `object?` açık bir kısıtlama olarak yasaklanmış değildir.

`class?`, "muhtemelen Nullable başvuru türü" belirten yeni bir kısıtlamadır, ancak `class` "Nullable başvuru türü" olarak belirlenir.

Bir tür bağımsız değişkeninin veya kısıtlama değerinin null olabilme durumu, türün Şu anda zaten olduğu durumlar dışında (null yapılabilir değer türleri `struct` kısıtlamasını karşılamıyor), türü kısıtlamayı karşılayıp karşılamadığını etkilemez. Ancak, tür bağımsız değişkeni kısıtlamanın null olabilme gereksinimlerini karşılamadığı takdirde bir uyarı verilebilir.

## <a name="null-state-and-null-tracking"></a>Null durum ve null izleme

Belirli bir kaynak konumundaki her ifadenin null olarak değerlendirilip değerlendirilmediğini belirten *null durumu*vardır. Null durumu "Not null" veya "Belki null" olabilir. Null durumu, null güvenli olmayan dönüştürmeler ve başvuru işlemleri hakkında bir uyarı verilip verilmeyeceğini belirlemekte kullanılır.

### <a name="null-tracking-for-variables"></a>Değişkenler için null izleme

Değişkenleri veya özellikleri belirten belirli ifadeler için, null durumu yineleme arasında, bunlara atamalara göre izlenir, bunlar üzerinde gerçekleştirilen testler ve bunlar arasındaki denetim akışı. Bu, değişkenler için kesin atamanın izlenme biçimine benzer. İzlenen ifadeler aşağıdaki biçimdeki olanlardır:

```antlr
tracked_expression
    : simple_name
    | this
    | base
    | tracked_expression '.' identifier
    ;
```

Tanımlayıcılar alanları veya özellikleri gösterir.

***Kesin atamaya benzer null durum geçişlerini açıkla***

### <a name="null-state-for-expressions"></a>İfadeler için null durumu

Bir ifadenin null durumu, form ve türünden türetilir ve buna dahil edilen değişkenlerin null durumudur.

### <a name="literals"></a>Sabit değerler

`null` sabit değerinin null durumu "Belki null". Null yapılamayan bir değer türü olarak bilinen bir türe dönüştürülmekte olan `default` sabit değerinin null durumu "Belki null" değeridir. Diğer herhangi bir sabit değerin null durumu "Not null" olur.

### <a name="simple-names"></a>Basit adlar

Bir `simple_name` değer olarak sınıflandırılmadığından, null durumu "null değil" olur. Aksi takdirde, izlenen bir ifadedir ve null durumu bu kaynak konumda izlenen null durumundadır.

### <a name="member-access"></a>Üye erişimi

Bir `member_access` değer olarak sınıflandırılmadığından, null durumu "null değil" olur. Aksi halde, izlenen bir ifadesiyse, null durumu bu kaynak konumda izlenen null durumundadır. Aksi halde, null durumu türünün varsayılan null durumudur.

### <a name="invocation-expressions"></a>Çağırma ifadeleri

Bir `invocation_expression` özel null davranışı için bir veya daha fazla öznitelik ile tanımlanmış bir üyeyi çağıralıyorsa, null durumu bu öznitelikler tarafından belirlenir. Aksi takdirde, ifadenin null durumu, türünün varsayılan null durumudur.

### <a name="element-access"></a>Öğe erişimi

Bir `element_access` özel null davranışı için bir veya daha fazla öznitelik ile tanımlanmış bir dizin oluşturucuyu çağıralıyorsa, null durumu bu öznitelikler tarafından belirlenir. Aksi takdirde, ifadenin null durumu, türünün varsayılan null durumudur.

### <a name="base-access"></a>Temel erişim

`B`, kapsayan türün temel türünü alıyorsa, `base.I` aynı null duruma sahip `((B)this).I` ve `base[E]` aynı null durumuna sahip `((B)this)[E]`.

### <a name="default-expressions"></a>Varsayılan ifadeler

`T` null yapılamayan bir değer türü olarak tanınmışsa, `default(T)` null olmayan "null" durumunda olur. Aksi takdirde "Belki de null" durumunda null durumu vardır.

### <a name="null-conditional-expressions"></a>Null Koşullu ifadeler

`null_conditional_expression` null durumunda "Belki null" değeri vardır.

### <a name="cast-expressions"></a>Atama ifadeleri

Bir atama `(T)E` ifadesi Kullanıcı tanımlı bir dönüştürmeyi çağırmazsa, ifadenin null durumu, türü için varsayılan null durumdur. Aksi takdirde, `T` null halinde olursa (null*yapılabilir* veya *bilinmiyor*), null durumu "Belki null" olur. Aksi takdirde null durumu `E`null durumu ile aynıdır.

### <a name="await-expressions"></a>Await ifadeleri

`await E` null durumu, türünün varsayılan null durumudur.

### <a name="the-as-operator"></a>`as` işleci

Bir `as` ifadesi null durumunda "Belki null" değeri içeriyor.

### <a name="the-null-coalescing-operator"></a>Null birleşim işleci

`E1 ?? E2` aynı null duruma sahip `E2`

### <a name="the-conditional-operator"></a>Koşullu işleç

Hem `E2` hem de `E3` null durumu "Not null" ise `E1 ? E2 : E3` null durumu "Not null" olur. Aksi takdirde "Belki de null" olur.

### <a name="query-expressions"></a>Sorgu ifadeleri

Sorgu ifadesinin null durumu, türünün varsayılan null durumudur.

### <a name="assignment-operators"></a>Atama işleçleri

`E1 = E2` ve `E1 op= E2`, herhangi bir örtük dönüştürme uygulandıktan sonra `E2` aynı null durumuna sahip.

### <a name="unary-and-binary-operators"></a>Birli ve ikili işleçler

Birli veya ikili işleç, özel null davranışı için bir veya daha fazla öznitelik ile belirtilen bir Kullanıcı tanımlı işleci çağırmazsa, null durumu bu öznitelikler tarafından belirlenir. Aksi takdirde, ifadenin null durumu, türünün varsayılan null durumudur.

***Dizeler ve temsilciler üzerinde ikili `+` için özel bir şey var mı?***

### <a name="expressions-that-propagate-null-state"></a>Null durumu yayan ifadeler

`(E)`, `checked(E)` ve `unchecked(E)` tümünün aynı null durumu `E`.

### <a name="expressions-that-are-never-null"></a>Hiçbir koşulda null olmayan ifadeler

Aşağıdaki ifade formlarının null durumu her zaman "Not null" dır:

- `this` erişim
- enterpolasyonlu dizeler
- `new` ifadeleri (nesne, temsilci, anonim nesne ve dizi oluşturma ifadeleri)
- `typeof` ifadeleri
- `nameof` ifadeleri
- Anonim işlevler (anonim yöntemler ve lambda ifadeleri)
- null-forverme ifadeleri
- `is` ifadeleri

## <a name="type-inference"></a>Tür çıkarımı

### <a name="type-inference-for-var"></a>`var` için tür çıkarımı

`var` ile belirtilen yerel değişkenler için gösterilen tür, başlatma ifadesinin null durumu ile bilgilendirilir.

```csharp
var x = E;
```

`E` türü null yapılabilir bir başvuru türü `C?` ve `E` null durumu "Not null" ise, `x` için gösterilen tür `C`olur. Aksi takdirde, çıkartılan tür `E`türüdür.

`x` için gösterilen türü null olabilir, `var`ek açıklama bağlamına göre, örneğin, bu konumda açıkça verilip verilmediği gibi yukarıda açıklanan şekilde belirlenir.

### <a name="type-inference-for-var"></a>`var?` için tür çıkarımı

`var?` ile belirtilen yerel değişkenler için gösterilen tür, başlatma ifadesinin null durumundan bağımsızdır.

```csharp
var? x = E;
```

`E` tür `T` null yapılabilir bir değer türü veya null yapılabilir bir başvuru türü ise, `x` için gösterilen tür `T`olur. Aksi takdirde, `T` null yapılamayan bir değer türü ise, gösterilen tür `S?``S`. Aksi takdirde, `T` null yapılamayan bir başvuru türü ise, gösterilen tür `C?``C`. Aksi takdirde, bildirim geçersizdir.

`x` için gösterilen türden null değer atanabilir değeri her zaman *null olabilir*.

### <a name="generic-type-inference"></a>Genel tür çıkarımı

Genel tür çıkarımı, gösterilen başvuru türlerinin null yapılabilir olup olmadığına karar vermenize yardımcı olmak için geliştirilmiştir. Bu en iyi çabadır, ve kendi başına uyarı durumunda değildir, ancak seçili aşırı yüklemenin Çıkarsanan türleri bağımsız değişkenlere uygulandığında null yapılabilir uyarılara yol açabilir.

Tür çıkarımı, gelen türlerin ek açıklama bağlamına bağlı değildir. Bunun yerine, açıkça ifade edilmiş olması durumunda kendi ek açıklama bağlamını elde eden bir `type` algılanır. Bu, kendiniz yazdıklarınız için bir kolaylık olarak tür çıkarımı rolü alt çizgi olarak oluşur.

Daha kesin olarak, bir çıkartılan tür bağımsız değişkeni için ek açıklama bağlamı, `<...>` türü parametre listesi tarafından izlenen belirtecin bağlamıdır; Yani, çağrılan genel yöntemin adı. Bu tür çağrılara çeviren sorgu ifadeleri için bağlam, çağrının oluşturulduğu sorgu yan tümcesinin ilk bağlamsal anahtar sözcüğünden alınır.

### <a name="the-first-phase"></a>İlk aşama

Null yapılabilir başvuru türleri, aşağıda açıklandığı gibi ilk ifadelerden sınırlara akar. Ayrıca, `null` ve `default` iki yeni tür sınır ortaya çıkartılır. Bunun amaçları, giriş ifadelerinde `null` veya `default` örnekleri aracılığıyla ilerleyesağlamaktır. Bu, aksi takdirde bile, ortaya çıkarılan bir türün null yapılabilir olmasına neden olabilir. Bu işlem, çıkarım işleminde "nullanlam" çekmek üzere geliştirilmiş null yapılabilir *değer* türleri için de çalışır.

İlk aşamada hangi sınırların ekleneceğini belirlemek şu şekilde geliştirilmiştir:

Bir bağımsız değişken `Ei` bir başvuru türü ise, çıkarım için kullanılan tür `U`, `Ei` null durumuna ve onun bildirildiği tür ' ne bağlıdır:
- Belirtilen tür null atanamaz bir başvuru türü veya null atanabilir bir başvuru türü `U0` `U0?`
    - `Ei` null durumu "null değil" ise `U` `U0`
    - `Ei` null durumu "Belki null" ise `U` `U0?`
- Aksi takdirde `Ei` tanımlanmış bir tür varsa `U` bu tür
- Aksi takdirde `Ei` `null`, `U` özel bir sınırdır `null`
- Aksi takdirde `Ei` `default`, `U` özel bir sınırdır `default`
- Aksi takdirde hiçbir çıkarım yapılmaz.

### <a name="exact-upper-bound-and-lower-bound-inferences"></a>Tam, üst sınır ve alt sınır

Tür *`U` `V`tür olarak `V`* , null atanabilir bir başvuru türü `V0?`ise, aşağıdaki yan tümcelerde `V0` yerine `V` kullanılır. *from*
- `V` sabit olmayan tür değişkenlerinden biri ise, `U` daha önce olduğu gibi tam, üst veya alt sınır olarak eklenir
- Aksi takdirde, `U` `null` veya `default`ise çıkarımı yapılmaz
- Aksi takdirde, `U` null yapılabilir bir başvuru türü `U0?`, sonraki yan tümcelerde `U` yerine `U0` kullanılır.

Temelde, sabit olmayan tür değişkenlerinden biri ile ilgili olan null değer alabilme, kendi sınırlarına göre korunur. Kaynak ve hedef türleri için ek olarak, diğer yandan null değer alabilme yok sayılır. Bu, eşleşmeyebilir veya eşleşmeyebilir, ancak daha sonra aşırı yükleme seçilir ve uygulanırsa bir uyarı gönderilir.

### <a name="fixing"></a>Düzelttikten

Spec Şu anda birden çok sınır birbirlerine dönüştürülebilir ancak farklılık belirten ne olduğunu açıklayan iyi bir iş gerçekleştirmez. Bu, `object` ve `dynamic`arasında, yalnızca öğe adlarında farklı olan türler arasında, oluşturulan türler arasında ve artık başvuru türleri için `C` ve `C?` arasında olabilir.

Ayrıca, giriş ifadelerinden sonuç türüne "nulltik" öğesini yaymaya ihtiyacımız vardır. 

Bunları işlemek için şu anda, düzeltmek üzere daha fazla aşama ekleyeceğiz:

1. Tüm limitlerde tüm türlere aday olarak toplayın, her türlü `?` null yapılabilir başvuru türünden kaldırır
2. Tam, alt ve üst sınırların gereksinimlerine göre adayları ortadan kaldırın (`null` ve `default` sınırlarını koruyarak)
3. Tüm diğer adaylar için örtük dönüştürmesi olmayan adayları ortadan kaldırın
4. Kalan adayların hepsi bir diğeri için kimlik dönüştürmeleri yoksa, tür çıkarımı başarısız olur
5. Kalan adayları aşağıda açıklandığı gibi *birleştirin*
6. Elde edilen aday bir başvuru türü veya null yapılamayan bir değer türü ise ve tüm tam *sınırlar ya da* alt sınırların *Tümü* null yapılabilir değer türleri, null yapılabilir başvuru türleri, `null` veya `default`, bu durumda `?` elde edilen aday öğesine eklenir ve bu, null olabilir bir değer türü veya başvuru türü yapar.

*Birleştirme* iki aday türü arasında tanımlanır. Bu, bir geçişli ve sorun olduğundan, adaylar aynı nihai sonuçla herhangi bir sırada birleştirilebilir. İki aday türü birbirlerine dönüştürülebilir değilse, bu tanımsız olur.

*Merge* işlevi iki aday türü ve bir yön ( *+* veya *-* ) alır:

- *Birleştir*(`T`, `T`, *d*) = t
- *Birleştirme*(`S`, `T?`, *+* ) = *birleştirme*(`S?`, `T`, *+* ) = *birleştirme*(`S`, `T`, *+* )`?`
- *Birleştirme*(`S`, `T?`, *-* ) = *birleştirme*(`S?`, `T`, *-* ) = *birleştirme*(`S`, `T`, *-* )
- *Birleştirme*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *+* ) = `C<`*birleştirme*(`S1`, `T1`, *d1*)`,...,`*birleştirme*(`Sn`, `Tn`, *DN*)`>`*where*
    - `C<...>` `i`' th türü parametresi covaryant ise `di` =  *+*
    - `C<...>` `i`' th türü parametresi Contra veya sabit ise `di` =  *-*
- *Birleştirme*(`C<S1,...,Sn>`, `C<T1,...,Tn>`, *-* ) = `C<`*birleştirme*(`S1`, `T1`, *d1*)`,...,`*birleştirme*(`Sn`, `Tn`, *DN*)`>`*where*
    - `C<...>` `i`' th türü parametresi covaryant ise `di` =  *-*
    - `C<...>` `i`' th türü parametresi Contra veya sabit ise `di` =  *+*
- *Birleştirme*(`(S1 s1,..., Sn sn)`, `(T1 t1,..., Tn tn)`, *d*) = `(`*birleştirme*(`S1`, `T1`, *d*)`n1,...,`*birleştirme*(`Sn`, `Tn`, *d*) `nn)`*where*
    - `si` ve `ti` farklıysa veya her ikisi de yoksa `ni` yok olur
    - `si` ve `ti` aynı ise `ni` `si`
- *Birleştirme*(`object`, `dynamic`) = *birleştirme*(`dynamic`, `object`) = `dynamic`

## <a name="warnings"></a>Uyarılar

### <a name="potential-null-assignment"></a>Olası null atama

### <a name="potential-null-dereference"></a>Olası null başvurusu

### <a name="constraint-nullability-mismatch"></a>Kısıtlama null olabilme uyumsuzluğu

### <a name="nullable-types-in-disabled-annotation-context"></a>Devre dışı ek açıklama bağlamındaki null yapılabilir türler

## <a name="attributes-for-special-null-behavior"></a>Özel null davranışı için öznitelikler


