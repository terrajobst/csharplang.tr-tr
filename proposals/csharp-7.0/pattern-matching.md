---
ms.openlocfilehash: 3df21c5816be90387a6cd9242e99ba11f43dfd1c
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485170"
---
# <a name="pattern-matching-for-c-7"></a>7 için C# model eşleştirme

İçin C# model eşleştirme uzantıları, işlev dillerinden çok sayıda avantajı ve işlevsel dillerin örüntüsünün eşleşmesini sağlayan, ancak temel alınan dilin hisleriyle sorunsuz bir şekilde tümleşen birçok avantaj sağlayan bir şekilde. Temel özellikler şunlardır: anlam anlamı verilerin şekli tarafından tanımlanan türler olan [kayıt türleri](https://github.com/dotnet/csharplang/blob/master/proposals/records.md); ve, bu veri türlerinin son derece daha çok daha fazla düzeyde ayrışmalarını sağlayan yeni bir ifade biçimi olan kalıp eşleme. Bu yaklaşımın öğeleri, programlama dillerinde [F#](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/p29-syme.pdf "Hafif bir dil aracılığıyla Genişletilebilir kalıp eşleme") ve [Scala](https://infoscience.epfl.ch/record/98468/files/MatchingObjectsWithPatterns-TR.pdf "Desenler Ile eşleşen nesneler")'daki ilgili özelliklerle ilham.

## <a name="is-expression"></a>İfade ifadesi

`is` işleci, bir ifadeyi bir *düzene*karşı test etmek için genişletilir.

```antlr
relational_expression
    : relational_expression 'is' pattern
    ;
```

Bu *relational_expression* biçimi, C# belirtideki mevcut formlara ek niteliğindedir. `is` belirtecinin solundaki *relational_expression* bir değer belirlememez veya bir tür yoksa bir derleme zamanı hatasıdır.

Düzenin her *tanımlayıcısı* , `is` işleci `true` sonra *kesinlikle atanan* yeni bir yerel değişken sunar (yani, *doğru olduğunda, kesin olarak atanır*).

> Note: `is-expression` ve *constant_pattern* *türü* arasında teknik bir belirsizlik vardır; bunlardan biri, tam bir tanımlayıcının geçerli bir ayrıştırmasından kaynaklanabilir. Bunu, dilin önceki sürümleriyle uyumluluk için bir tür olarak bağlamaya çalışırız; yalnızca bu başarısız olursa, diğer bağlamlarda yaptığımız gibi, bulduğu ilk şeyi (sabit veya bir tür olması gerekir) çözeceğiz. Bu belirsizlik yalnızca `is` ifadesinin sağ tarafında bulunur.

## <a name="patterns"></a>Desenler

Desenler `is` işlecinde ve bir *switch_statement* , gelen verilerin karşılaştırılacağı verilerin şeklini ifade etmek için kullanılır. Desenler, verilerin bölümlerinin alt desenlerle eşleştirileceği şekilde özyinelemeli olabilir.

```antlr
pattern
    : declaration_pattern
    | constant_pattern
    | var_pattern
    ;

declaration_pattern
    : type simple_designation
    ;

constant_pattern
    : shift_expression
    ;

var_pattern
    : 'var' simple_designation
    ;
```

> Note: `is-expression` ve *constant_pattern* *türü* arasında teknik bir belirsizlik vardır; bunlardan biri, tam bir tanımlayıcının geçerli bir ayrıştırmasından kaynaklanabilir. Bunu, dilin önceki sürümleriyle uyumluluk için bir tür olarak bağlamaya çalışırız; yalnızca bu başarısız olursa, diğer bağlamlarda yaptığımız gibi, bulduğu ilk şeyi (sabit veya bir tür olması gerekir) çözeceğiz. Bu belirsizlik yalnızca `is` ifadesinin sağ tarafında bulunur.

### <a name="declaration-pattern"></a>Bildirim kalıbı

*Declaration_pattern* her ikisi de bir ifadenin verilen türden olduğunu sınar ve test başarılı olursa bu türe yayınlar. *Simple_designation* bir tanımlayıcıse, verilen tanımlayıcı tarafından adlandırılan belirtilen türde yerel bir değişken tanıtır. Bu yerel değişken, bir kalıp eşleme işleminin sonucu true olduğunda *kesinlikle atanır* .

```antlr
declaration_pattern
    : type simple_designation
    ;
```

Bu ifadenin çalışma zamanı anlam düzeyi, sol taraftaki *relational_expression* işlenenin çalışma zamanı türünü, düzendeki *türe* göre sınar. Çalışma zamanı türü (veya bazı alt tür) ise, `is operator` sonucu `true`. Sonuç `true`, sol taraftaki işlenenin değerine atanan *tanımlayıcı* tarafından adlı yeni bir yerel değişken bildirir.

Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır. `E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` ' den `T`' a bir kutudan çıkarma dönüştürmesi varsa, tür `T` ile *uyumlu* olarak kabul edilir. `E` türündeki bir ifade ile eşleştirildiği tür deseninin türüyle uyumlu kalıp yoksa, derleme zamanı hatasıdır.

> Note: [7,1 C# içinde](../csharp-7.1/generics-pattern-match.md) , giriş türü veya tür `T` bir açık tür ise, bu, bir kalıp eşleme işlemine izin vermek için bunu genişlettik. Bu paragraf şu şekilde değiştirilmiştir:
> 
> Sol taraftaki statik türdeki belirli birleşimler ve verilen tür uyumsuz olarak değerlendirilir ve derleme zamanı hatası ile sonuçlanır. `E` statik tür değeri, bir kimlik dönüştürmesi, örtük bir başvuru dönüştürmesi, paketleme dönüştürmesi, açık bir başvuru dönüştürmesi veya `E` ' den `T`' e bir kutudan çıkarma dönüştürmesi ya da **`E` ya da `T` bir açık tür**ise, tür ile *uyumlu* bir değer `T` olarak kabul edilir. `E` türündeki bir ifade ile eşleştirildiği tür deseninin türüyle uyumlu kalıp yoksa, derleme zamanı hatasıdır.

Bildirim stili, başvuru türlerinin çalışma zamanı türü testlerini gerçekleştirmek için yararlıdır ve deyim yerini alır

```csharp
var v = expr as Type;
if (v != null) { // code using v }
```

Biraz daha kısa

```csharp
if (expr is Type v) { // code using v }
```

*Tür* null yapılabilir bir değer türünde ise bu bir hatadır.

Bildirim stili, null yapılabilir türlerin değerlerini test etmek için kullanılabilir: `Nullable<T>` (veya kutulanmış `T`) bir değer, değer null ise ve `T2` türü `T`veya bazı temel tür veya `T`arabirimse `T2 id` bir tür düzeniyle eşleşir. Örneğin, kod parçasında

```csharp
int? x = 3;
if (x is int v) { // code using v }
```

`if` deyimin koşulu çalışma zamanında `true` ve `v` değişkeni, blok içindeki `int` türündeki `3` değeri tutar.

### <a name="constant-pattern"></a>Sabit model

```antlr
constant_pattern
    : shift_expression
    ;
```

Sabit bir model, bir ifadenin değerini sabit bir değere göre sınar. Sabit, değişmez değer, belirtilen bir `const` değişkeninin adı ya da bir numaralandırma sabiti veya bir `typeof` ifadesi gibi herhangi bir sabit ifade olabilir.

Hem *e* hem de *c* integral türtürsiyse, ifadenin sonucu `e == c` `true`ise, model eşleştirildiği kabul edilir.

Aksi takdirde, `object.Equals(e, c)` `true`döndürürse, model eşleşen kabul edilir. Bu durumda, statik *e* türü, sabit türü ile *uyumlu kalıp* değilse, derleme zamanı hatası olur.

### <a name="var-pattern"></a>var deseninin

```antlr
var_pattern
    : 'var' simple_designation
    ;
```

*E* ifadesi her zaman *var_pattern* eşleşir. Diğer bir deyişle, var olan bir *model* ile eşleşme her zaman başarılı olur. *Simple_designation* bir tanımlayıcıse, çalışma zamanında *e* değeri yeni tanıtılan bir yerel değişkene bağlanır. Yerel değişkenin türü, *e*'nin statik türüdür.

Ad `var` bir türe bağlandığında bu bir hatadır.

## <a name="switch-statement"></a>Switch deyimleri

`switch` deyimi, *anahtar ifadesiyle*eşleşen ilişkili bir düzene sahip olan ilk bloğun yürütülmesi için seçilecek şekilde genişletilir.

```antlr
switch_label
    : 'case' complex_pattern case_guard? ':'
    | 'case' constant_expression case_guard? ':'
    | 'default' ':'
    ;

case_guard
    : 'when' expression
    ;
```

Desenlerin eşleştirildiği sıra tanımlı değil. Bir derleyicinin desenleri sıralamayla eşleşmesi ve diğer desenlerin eşleşme sonucunu hesaplamak için zaten eşleşen desenlerin sonuçlarını yeniden kullanmak için izin verilir.

Bir *durum koruması* varsa, ifadesi `bool`türündedir. Bu, büyük/küçük harf karşılanması durumunda karşılanması gereken ek bir koşul olarak değerlendirilir.

*Switch_label* çalışma zamanında hiçbir etkisi yoksa, bu bir hatadır çünkü bu, kendi deseninin önceki durumlar tarafından bir alt grup haline getirilir. [TODO: derleyicinin bu kararya ulaşmak için kullanması gereken tekniklerin daha kesin olması gerekir.]

Bir *switch_label* olarak belirtilen bir model değişkeni, yalnızca bu durum bloğu tam olarak bir *switch_label*içeriyorsa, büyük/küçük harf bloğunda kesinlikle atanır.

[TODO: bir *anahtar bloğunun* ne zaman ulaşılabilir olduğunu belirtmemiz gerekir.]

### <a name="scope-of-pattern-variables"></a>Model değişkenlerinin kapsamı

Bir düzende belirtilen değişkenin kapsamı aşağıdaki gibidir:

- Desenler bir Case etiketse, değişkenin kapsamı *Case bloğu*olur.

Aksi takdirde, değişken bir *is_pattern* ifadesinde, ve kapsamı, *is_pattern* ifadesini içeren ifadeyi hemen kapsayan yapıyı temel alır:

- İfade bir ifade ifadesinde ise, kapsamı lambda gövdesidir.
- İfade bir ifade-Bodied yöntemi veya özelliği ise, kapsamı yöntemin veya özelliğin gövdesidir.
- İfade bir `catch` yan tümcesinin `when` yan tümcelerinde yer alıyorsa, kapsamı o `catch` yan tümcesine sahiptir.
- İfade bir *iteration_statement*ise, kapsamı yalnızca o deyimdir.
- Aksi takdirde, ifade başka bir deyim formundadır, kapsamı deyimi içeren kapsamdır.

Kapsamı belirlemek amacıyla bir *embedded_statement* kendi kapsamında olduğu kabul edilir. Örneğin, bir *if_statement* için dilbilgisi

``` antlr
if_statement
    : 'if' '(' boolean_expression ')' embedded_statement
    | 'if' '(' boolean_expression ')' embedded_statement 'else' embedded_statement
    ;
```

Bu nedenle bir *if_statement* denetimli bildiriminde bir model değişkeni bildiriliyorsa, kapsamı bu *embedded_statement*kısıtlıdır:

```csharp
if (x) M(y is var z);
```

Bu durumda `z` kapsamı gömülü ifadedir `M(y is var z);`.

Diğer nedenlerden dolayı diğer nedenlerle hatalar vardır (örneğin, bir parametrenin varsayılan değeri veya bir özniteliği, her ikisi de bir hata olan bu içerikler sabit bir ifade gerektirdiğinden).

> [7,3 C# ' de,](../csharp-7.3/expression-variables-in-initializers.md) bir model değişkeninin bildirilebilecek aşağıdaki bağlamları ekledik:
> - İfade bir *Oluşturucu başlatıcısında*ise, kapsamı *Oluşturucu başlatıcısı* ve oluşturucunun gövdesi olur.
> - İfade bir alan başlatıcısında ise, kapsamı göründüğü *equals_value_clause* .
> - İfade bir lambda gövdesine çevrilebilmesi için belirtilen bir sorgu yan tümcesinde ise, kapsamı yalnızca o ifadedir.

## <a name="changes-to-syntactic-disambiguation"></a>Sözdizimsel Kesinleştirme değişiklikleri

C# Dilbilgisinin belirsiz olduğu ve dil belirtiminin bu belirsizlikleri nasıl çözümlendiğini belirten genel türler içeren durumlar vardır:

> #### <a name="7652-grammar-ambiguities"></a>7.6.5.2 dilbilgisi belirsizlikleri
> *Basit ad* (§ 7.6.3) ve *üye erişimi* (§ 7.6.5) için üretimler, ifadelerde dilbilgisi için verebilir. Örneğin, ifade:
> ```csharp
> F(G<A,B>(7));
> ```
> , `G < A` ve `B > (7)`iki bağımsız değişkenle `F` çağrısı olarak yorumlanamaz. Alternatif olarak, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanabilecek.

> Bir belirteç dizisi bir *basit-ad* (§ 7.6.3), *üye erişimi* (§ 7.6.5) veya bir *tür bağımsız değişkeni* (§ 18.5.2) ile biten *işaretçi-üye erişimi* (§ 4.4.1) olarak ayrıştırılamıyorsa, kapanış `>` belirtecini izleyen belirteç incelenir. Bunlardan biri ise
> ```none
> (  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
> ```
> daha sonra *tür bağımsız değişken listesi* , *basit ad*, *üye erişimi* veya *işaretçi-üye erişiminin* bir parçası olarak korunur ve belirteçleri dizisinin diğer olası ayrıştırması atılır. Aksi takdirde, *tür bağımsız değişken listesi* , belirteçlerin başka bir olası ayrıştırması olmasa bile, *basit ad*, *üye erişimi* veya > *işaretçi-üye erişiminin*parçası olarak kabul edilmez. Bir *ad alanı-veya-tür adı* (§ 3,8) içinde bir *tür bağımsız değişken listesi* ayrıştırılırken bu kuralların uygulanmadığını unutmayın. İfade
> ```csharp
> F(G<A,B>(7));
> ```
> Bu kurala göre, iki tür bağımsız değişkeni ve bir normal bağımsız değişkenle `G` genel bir yöntem çağrısı olan bir bağımsız değişkenle `F` çağrısı olarak yorumlanacaktır. Deyimler
> ```csharp
> F(G < A, B > 7);
> F(G < A, B >> 7);
> ```
> her biri, iki bağımsız değişkenle `F` çağrısı olarak yorumlanır. İfade
> ```csharp
> x = F < A > +y;
> ```
> , bir *Type-argument-List* ile ardından bir ikili artı işleci ile bir *basit ad* yerine, ' den daha az işleç, büyüktür işleci ve birli Plus `x = (F < A) > (+y)`işleci olarak yorumlanacaktır. İfadesinde
> ```csharp
> x = y is C<T> + z;
> ```
> belirteçler `C<T>` bir *ad alanı veya tür adı* olarak yorumlanır- *Argument-List*.

7 ' de C# sunulan ve bu Kesinleştirme kurallarını dilin karmaşıklığını işlemeye yetecek kadar yeterli olmayan değişiklikler vardır.

### <a name="out-variable-declarations"></a>Out değişken bildirimleri

Artık bir out bağımsız değişkeninde bir değişken bildirmek mümkündür:

```csharp
M(out Type name);
```

Ancak, tür genel olabilir: 

```csharp
M(out A<B> name);
```

Bağımsız değişken için dil dilbilgisi *ifadesi*kullandığından, bu bağlam Kesinleştirme kuralına tabidir. Bu durumda, kapanış `>`, bir *tür bağımsız değişken listesi*olarak işlenmesine izin veren belirteçlerden biri olmayan bir *tanımlayıcı*tarafından izlenir. Bu nedenle **, bir *tür bağımsız değişken listesi*için belirsizliği tetikleyen belirteç kümesine *tanımlayıcı* eklemeyi önerin.**

### <a name="tuples-and-deconstruction-declarations"></a>Tanımlama grupları ve ayrıştırma bildirimleri

Bir demet sabit değeri tamamen aynı sorun üzerinde çalışır. Tanımlama grubu ifadesini göz önünde bulundurun

```csharp
(A < B, C > D, E < F, G > H)
```

Bağımsız değişken listesini C# ayrıştırmak için eski 6 kuralları altında, bu, ilk olarak `A < B` başlayarak dört öğe içeren bir tanımlama grubu olarak ayrıştırılabilir. Bununla birlikte, bu, bir zaman oluşumunun solunda göründüğünde, yukarıdaki *tanımlayıcı* belirteç tarafından tetiklenerek, daha önce açıklandığı gibi kesinleştirilmesine istiyoruz:

```csharp
(A<B,C> D, E<F,G> H) = e;
```

Bu, ilki `A<B,C>` ve `D`adlı iki değişken bildiren bir deinşaat bildirimidir. Diğer bir deyişle, demet değişmez değeri her biri bir bildirim ifadesi olan iki ifade içerir.

Belirtim ve derleyicinin basitliği için, bu demet sabit değerinin göründüğü yerde iki öğeli bir kayıt kümesi olarak ayrıştırılmasını (atamanın sol tarafında görünüp başlatılmayacağını) önerdim. Bu, önceki bölümde açıklanan belirsizinin doğal bir sonucu olacaktır.

### <a name="pattern-matching"></a>Desenler eşleme

Model eşleştirme, ifade türü belirsizliğin ortaya çıkarmakta olduğu yeni bir bağlam tanıtır. Daha önce `is` işlecinin sağ tarafında bir tür vardı. Artık bir tür veya ifade olabilir ve bir tür ise, bir tanımlayıcı gelebilir. Teknik olarak, var olan kodun anlamını değiştirebilir:

```csharp
var x = e is T < A > B;
```

Bu, C# 6 kuralları altında şu şekilde ayrıştırılabilir

```csharp
var x = ((e is T) < A) > B;
```

ancak C# 7 kuralları altında (yukarıda önerilen Kesinleştirme ile birlikte)

```csharp
var x = e is T<A> B;
```

`T<A>`türünde bir değişken `B` bildiren. Neyse ki, yerel ve Roslyn derleyicileri, C# 6 kodunda sözdizimi hatası vertikleri bir hata içermektedir. Bu nedenle, bu son değişiklik bir sorun değildir.

Örüntü eşleme, bir türü seçmek için belirsizlik çözümünü destekleyen ek belirteçler sunar. Mevcut geçerli C# 6 kodunun aşağıdaki örnekleri, ek Kesinleştirme kuralları olmadan bozulur:

```csharp
var x = e is A<B> && f;            // &&
var x = e is A<B> || f;            // ||
var x = e is A<B> & f;             // &
var x = e is A<B>[];               // [
```

### <a name="proposed-change-to-the-disambiguation-rule"></a>Kesinleştirme kuralında önerilen değişiklik

Belirteçlerin, ayırt edici belirteç listesini değiştirme belirtimini

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^
```

kime

>
```none
(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [
```

Ve bazı bağlamlarda *tanımlayıcıyı* Kesinleştirme belirteci olarak değerlendiririz. Bu bağlamlar, ayırt edilen belirteçlerin sırasının kendisinden hemen önce `is`, `case`veya `out`ya da bir demet sabit öğesinin ilk öğesi ayrıştırılırken oluşur (Bu durumda, belirteçlerin önünde `(` veya `:`, daha sonra da tanımlayıcı bir `,`) veya bir dizi sabit değerinin sonraki bir öğesi tarafından kullanılır.

### <a name="modified-disambiguation-rule"></a>Değişiklik Kesinleştirme kuralı

Düzeltilen Kesinleştirme kuralı şuna benzer olacaktır

> Bir belirteç dizisi bir *basit-ad* (§ 7.6.3), *üye erişimi* (§ 7.6.5) veya bir *tür bağımsız değişkeni* (§ 18.5.2) ile biten *işaretçi-üye-erişim* (§ 4.4.1) olarak ayrıştırılabiliyorsanız, kapatma `>` belirtecini izleyen belirteç incelenmekte olup olmadığını görmek için
> - Birisi `(  )  ]  }  :  ;  ,  .  ?  ==  !=  |  ^  &&  ||  &  [`; veya
> - İlişkisel işleçlerden biri `<  >  <=  >=  is as`; veya
> - Sorgu ifadesinin içinde görünen bağlamsal sorgu anahtar sözcüğü; veya
> - Belirli bağlamlarda *tanımlayıcıyı* Kesinleştirme belirteci olarak değerlendiririz. Bu bağlamlar, ayırt edilen belirteçlerin sırasının, bir tanımlama sözcüğünün ilk öğesi ayrıştırılırken `is`, `case` veya `out`ya da ortaya çıkar. (Bu durumda, belirteçlerin önünde `(` veya `:`, daha sonra da tanımlayıcı bir `,`) veya bir dizi sabit değerinin sonraki bir öğesi gelir.
> 
> Aşağıdaki belirteç bu liste veya böyle bir bağlamdaki bir tanımlayıcı arasında ise, *tür bağımsız değişken listesi* *basit ad*, *üye erişimi* veya *işaretçi üye erişiminin* bir parçası olarak korunur ve belirteçleri dizisinin diğer olası ayrıştırması atılır.  Aksi takdirde, *tür bağımsız değişken listesi* , belirteçlerin başka bir olası ayrıştırması olmasa bile, *basit ad*, *üye erişimi* veya *işaretçi-üye erişiminin*bir parçası olarak kabul edilmez. Bir *ad alanı-veya-tür adı* (§ 3,8) içinde bir *tür bağımsız değişken listesi* ayrıştırılırken bu kuralların uygulanmadığını unutmayın.

### <a name="breaking-changes-due-to-this-proposal"></a>Bu tekliften kaynaklanan değişiklikler kesiliyor

Bu önerilen Kesinleştirme kuralı nedeniyle hiçbir bozucu değişiklik bilinmiyor.

### <a name="interesting-examples"></a>İlginç örnekler

Bu Kesinleştirme kurallarının ilginç sonuçları aşağıda verilmiştir:

`(A < B, C > D)` ifade, her bir karşılaştırma olmak üzere iki öğe içeren bir tanımlama grubu.

İfade `(A<B,C> D, E)`, ilki bir bildirim ifadesi olan iki öğesi olan bir tanımlama grubu olur.

Çağırma `M(A < B, C > D, E)` üç bağımsız değişkene sahiptir.

Çağırma `M(out A<B,C> D, E)`, ilki bir `out` bildirimi olan iki bağımsız değişkene sahiptir.

`e is A<B> C` ifadesi bir bildirim ifadesi kullanır.

Case etiketi `case A<B> C:` bir bildirim ifadesi kullanır.

## <a name="some-examples-of-pattern-matching"></a>Bazı kalıp eşleme örnekleri

### <a name="is-as"></a>Farklı

Deyim yerini alacak

```csharp
var v = expr as Type;   
if (v != null) {
    // code using v
}
```

Biraz daha kısa ve doğrudan

```csharp
if (expr is Type v) {
    // code using v
}
```

### <a name="testing-nullable"></a>Null yapılabilir test

Deyim yerini alacak

```csharp
Type? v = x?.y?.z;
if (v.HasValue) {
    var value = v.GetValueOrDefault();
    // code using value
}
```

Biraz daha kısa ve doğrudan

```csharp
if (x?.y?.z is Type value) {
    // code using value
}
```

### <a name="arithmetic-simplification"></a>Aritmetik basitleştirme

İfadeleri göstermek için özyinelemeli türler kümesi tanımladığımızda (ayrı bir teklife göre):

```csharp
abstract class Expr;
class X() : Expr;
class Const(double Value) : Expr;
class Add(Expr Left, Expr Right) : Expr;
class Mult(Expr Left, Expr Right) : Expr;
class Neg(Expr Value) : Expr;
```

Artık bir ifadenin türevini (azaltılan) hesaplamak için bir işlev tanımlayabiliriz:

```csharp
Expr Deriv(Expr e)
{
  switch (e) {
    case X(): return Const(1);
    case Const(*): return Const(0);
    case Add(var Left, var Right):
      return Add(Deriv(Left), Deriv(Right));
    case Mult(var Left, var Right):
      return Add(Mult(Deriv(Left), Right), Mult(Left, Deriv(Right)));
    case Neg(var Value):
      return Neg(Deriv(Value));
  }
}
```

Bir ifade simplifier konumsal desenleri gösterir:

```csharp
Expr Simplify(Expr e)
{
  switch (e) {
    case Mult(Const(0), *): return Const(0);
    case Mult(*, Const(0)): return Const(0);
    case Mult(Const(1), var x): return Simplify(x);
    case Mult(var x, Const(1)): return Simplify(x);
    case Mult(Const(var l), Const(var r)): return Const(l*r);
    case Add(Const(0), var x): return Simplify(x);
    case Add(var x, Const(0)): return Simplify(x);
    case Add(Const(var l), Const(var r)): return Const(l+r);
    case Neg(Const(var k)): return Const(-k);
    default: return e;
  }
}
```
