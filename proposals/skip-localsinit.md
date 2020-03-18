---
ms.openlocfilehash: 52b43abd2d8fb56088a68c7169289a63c43ce96f
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484491"
---
# <a name="suppress-emitting-of-localsinit-flag"></a>`localsinit` bayrağını yaymayı gizleyin.

* [x] önerilir
* [] Prototipi: başlatılmadı
* [] Uygulama: başlatılmadı
* [] Belirtimi: başlatılmadı

## <a name="summary"></a>Özet
[summary]: #summary

`localsinit` bayrağının `SkipLocalsInitAttribute` özniteliği aracılığıyla bastırçıkmasına izin ver. 

## <a name="motivation"></a>Amacı
[motivation]: #motivation


### <a name="background"></a>Arka Plan
CLR spec başına, başvuruları içermeyen yerel değişkenler VM/JıT tarafından belirli bir değere başlatılmaz. Başlatma olmadan bu tür değişkenlerden okuma tür açısından güvenlidir, aksi halde davranış tanımsız ve uygulamaya özgüdür. Genellikle başlatılmamış Yereller, artık yığın çerçevesinin kapladığı bellekte kalan değerleri içerir. Bu, belirleyici olmayan davranışa ve hataları yeniden oluşturmaya neden olabilir. 

Yerel bir değişken "atama" için iki yol vardır: 
- bir değeri depolayarak veya 
- Yerel bellek havuzu için ayrılan her şeyin sıfır olarak başlatılmış olmasını zorlayan `localsinit` bayrağını belirterek: Bu, hem yerel değişkenleri hem de `stackalloc` verileri içerir.    

Başlatılmamış verilerin kullanılması önerilmez ve doğrulanabilir kodda buna izin verilmez. Akış Analizi ile ilgili olduğunu kanıtlamak mümkün olsa da, doğrulama algoritmasının koruyucu için izin verilir ve yalnızca `localsinit` ayarlanması gerekir.

Tarihsel C# derleyici, yerelleri bildiren tüm yöntemlerde `localsinit` bayrağını yayar.

, C# CLR spec 'in gerektirenden daha katı olan kesin atama analizini kullanır (C# yerellerin kapsamını da dikkate almanız gerekir), sonuçta ortaya çıkan kodun tek bir biçimde doğrulanabilir olması kesin bir şekilde garanti edilmez:
- CLR ve C# kurallar, yerel bir as `out` bağımsız değişkeninin bir `use`geçirilmesi durumunda olup olmadığını kabul etmeyebilir.
- Koşullar bilindiğinde CLR ve C# kuralları koşullu dallar sırasında kabul etmeyebilir (Sabit yayma).
- Buna izin verildiği için CLR `localinits`de yeterlidir.  

### <a name="problem"></a>Sorun
Yüksek performanslı uygulamada zorlanan sıfır başlatma maliyeti fark edilebilir. `stackalloc` kullanıldığında özellikle dikkat çekici hale gelir.

Bazı durumlarda JıT, sonraki atamalar tarafından bu başlatma "sonlandırıldı" olduğunda tek tek Yereller için ilk sıfır başlatma işlemi yapabilir. Tüm JITs bunu yapmayın ve bu iyileştirmenin sınırları vardır. `stackalloc`yardımcı değildir.

Sorunun gerçek olduğunu anlamak için, `IL` yerel öğeler içermeyen bir yöntemin `localsinit` bayrağına sahip olmaması durumunda bilinen bir hata vardır. Hata, başlatma maliyetlerinin önüne geçmek için, bu tür yöntemlere `stackalloc` yerleştirilerek, kullanıcılar tarafından zaten kullanım dışı bırakılmış. Bunun nedeni, yerelin `IL` olmaması kararlı bir ölçüdür ve CodeGen stratejisindeki değişikliklere göre farklılık gösterebilir. Hatanın düzeltilmesi ve kullanıcıların, bayrağı gizlemeyi daha fazla belgelenmiş ve güvenilir bir şekilde alması gerekir. 

## <a name="detailed-design"></a>Ayrıntılı tasarım

Derleyicinin `localsinit` bayrağını yaymadığından `System.Runtime.CompilerServices.SkipLocalsInitAttribute` belirtilmesine izin verin.
 
Bunun nihai sonucu, Yereller, büyük olasılıkla bir test eden JıT tarafından sıfır olarak başlatılmamış olabilir C#.  
Bu `stackalloc` verilerin yanı sıra sıfır olarak başlatılamaz. Bu kesinlikle Observable ' dır, ancak aynı zamanda en çok mücadele senaryosu da vardır.

İzin verilen ve tanınan öznitelik hedefleri şunlardır: `Method`, `Property`, `Module`, `Class`, `Struct`, `Interface`, `Constructor`. Ancak derleyici, öznitelik listelenen hedeflerle tanımlanmalıdır veya özniteliğin hangi derlemede tanımlandığından emin olur. 

Öznitelik bir kapsayıcıda belirtildiğinde (`class`, `module`, iç içe geçmiş bir yöntem için bir yöntem,...), bayrak kapsayıcı içinde bulunan tüm yöntemleri etkiler.

Birleştirme yöntemleri, mantıksal kapsayıcıdan/sahibe ait bayrağı "devralma". 

Bayrak, gerçek yöntem gövdeleri için yalnızca CodeGen stratejisini etkiler. Yani. bayrak soyut yöntemleri etkilemez ve geçersiz kılma/uygulama yöntemlerine yayılmaz.

Bu, **_dil özelliği değil_** , açıkça bir **_derleyici özelliğidir_** .  
Derleyici komut satırı anahtarlarına benzer şekilde, özellik belirli bir codegen stratejisinin uygulama ayrıntılarını denetler ve C# belirtim için gerekli olması gerekmez.

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

- Eski/diğer derleyiciler özniteliği karşılamayabilir.
Özniteliğin uyumlu davranış yok sayılıyor. Yalnızca hafif bir performans isabetini elde edebilir.

- `localinits` bayrağı olmayan kod doğrulama başarısızlıklarını tetikleyebilir.
Bu özelliği istemek için gereken kullanıcılar genellikle doğruıyabilme ile ilgilidir. 
 
- Özniteliği bir bağımsız yöntemden daha yüksek düzeylerde uygulamak, `stackalloc` kullanıldığında observable olan yerel olmayan etkiye sahiptir. Henüz bu, en çok istenen senaryodur.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

- Yöntem `unsafe` bağlamda bildirildiğinde `localinits` bayrağını atlayın. Bu durum, `stackalloc`olması durumunda sessiz ve tehlikeli davranışın belirleyici olan belirleyici olmayan şekilde değişmesine neden olabilir.

- `localinits` bayrağını her zaman atlayın.
Yukarıdakilerden daha da kötüsü.

- Yöntem gövdesinde `stackalloc` kullanılmadığı müddetçe `localinits` bayrağını atlayın.
En çok istenen senaryoyu ele almaz ve bu geri dönüş seçeneği olmadan kodu doğrulanamaz olarak döndürebilir.

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

- Öznitelik gerçekten meta verilere yayınlansın mı? 

## <a name="design-meetings"></a>Tasarım toplantıları

Henüz yok. 