---
ms.openlocfilehash: 6cf489595654236c18edee94c0af380e605c9571
ms.sourcegitcommit: f61a06970fa0562d2e40363fae3948eb168624ca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "79485177"
---
# <a name="private-protected"></a>private protected

* [x] önerilir
* [x] prototipi: [Tamam](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] uygulama: [Tamam](https://github.com/dotnet/roslyn/blob/master/docs/features/private-protected.md)
* [x] belirtimi: [Tamam](#detailed-design)

## <a name="summary"></a>Özet
[summary]: #summary

CLR `protectedAndInternal` erişilebilirlik düzeyini `private protected`C# olarak kullanıma sunun.

## <a name="motivation"></a>Amacı
[motivation]: #motivation

Bir API 'nin yalnızca, türü sağlayan derlemede bulunan alt sınıflar tarafından uygulanması ve kullanılması amaçlanan üyeleri içerdiği birçok koşul vardır. CLR bu amaçla erişilebilirlik düzeyi sağladığından, ' de C#kullanılamaz. Sonuç olarak, API sahipleri `internal` koruma ve kendi kendine disiplin veya özel bir çözümleyici kullanmaya zorlanır ya da bunu açıklayan ek belgeler ile `protected` kullanarak, üye türün ortak belgelerinde göründüğünde, ortak API 'nin bir parçası olmaya yönelik değildir.  İkinci bir örnek için, adları `Common`başlayan Roslyn `CSharpCompilationOptions` üyelerine bakın.

İçindeki C# bu erişim düzeyi için doğrudan destek sağlanması, bu koşulların doğal olarak dilde ifade edilmesi için olanak sağlar.

## <a name="detailed-design"></a>Ayrıntılı tasarım
[design]: #detailed-design

### <a name="private-protected-access-modifier"></a>`private protected` erişim değiştiricisi

Yeni bir erişim değiştirici birleşimi `private protected` eklemeyi öneririz (değiştiriciler arasında herhangi bir sırada görünebilen). Bu, protectedandınternal clr kavramının yanı sıra, şu anda [ C++/CLI](https://docs.microsoft.com/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli#BKMK_Member_visibility)' da kullanılan aynı söz dizimini kullanır.

Bu alt sınıf, üyeyle aynı derlemede ise, `private protected` tarafından tanımlanan bir üyenin kapsayıcısının bir alt sınıfı içinden erişilebilir.

Dil belirtimini aşağıdaki gibi (kalın yazı tipiyle eklemeler) değiştirdik. Bölüm numaraları aşağıda gösterilmez çünkü hangi belirtimin hangi sürüme tümleştirildiği konusunda farklılık gösterebilir.

-----

> Bir üyenin tanımlanmış erişilebilirliği aşağıdakilerden biri olabilir:
- Public, üye bildiriminde ortak bir değiştirici eklenerek seçilidir. Public 'in sezgisel anlamı "erişim sınırlı değildir".
- Korumalı, üye bildiriminde korunan değiştirici eklenerek seçilir. Korunan ' nin sezgisel anlamı, "Access 'in bulunduğu sınıftan türetilmiş" erişim ile sınırlıdır.
- Dahili, üye bildiriminde iç değiştirici eklenerek seçilir. Dahili olarak "erişim bu derlemeyle sınırlı" anlamına gelir.
- Üye bildiriminde hem korumalı hem de iç değiştirici eklenerek seçilen, korunan dahili. Korumalı iç öğesinin sezgisel anlamı, "Bu derleme içinde erişilebilir ve" içeren sınıftan türetilmiş türler ".
- **Özel korumalı, üye bildiriminde hem özel hem de korumalı değiştirici eklenerek seçilir. Private Protected 'nin sezgisel anlamı, "kapsayan sınıftan türetilmiş türler tarafından bu derleme içinde erişilebilir" olarak belirlenir.**

-----

> Bir üye bildiriminin gerçekleştiği içeriğe bağlı olarak, yalnızca belirli bir tanımlı erişilebilirlik türlerine izin verilir. Ayrıca, bir üye bildirimi herhangi bir erişim değiştiricisi içermiyorsa, bildirimin gerçekleştiği bağlam, varsayılan olarak belirtilen erişilebilirliği belirler. 
- Ad alanlarında genel olarak tanımlanmış erişilebilirlik vardır. Ad alanı bildirimlerinde erişim değiştiricilerine izin verilmez.
- Doğrudan derleme birimlerinde veya ad alanlarında (diğer türlerin aksine) belirtilen türlerin, genel veya iç tanımlı erişilebilirliği ve varsayılan olarak, iç olarak tanımlanmış erişilebilirliği vardır.
- Sınıf üyeleri beş tür tanımlanmış erişilebilirliği ve özel olarak belirtilen erişilebilirliği varsayılan olarak içerebilir. [Note: bir sınıfın üyesi olarak belirtilen bir türün beş tür tanımlanmış erişilebilirliği olabilir, ancak bir ad alanının üyesi olarak belirtilen bir türün yalnızca ortak veya iç olarak tanımlanmış erişilebilirliği olabilir. Son nota]
- Yapılar örtük olarak mühürlendikleri için yapı üyelerinin ortak, iç veya özel olarak belirtilen erişilebilirliği ve varsayılan olarak özel olarak bildirildiği erişilebilirliği olabilir. Bir yapıda tanıtılan (yani, bu yapı tarafından devralınmayan) yapı üyeleri *, korumalı* ~~veya~~ korumalı iç**veya özel olarak** belirtilen bir tanımlı erişilebilirliği içeremez. [Note: bir yapının üyesi olarak belirtilen bir türün ortak, iç veya özel olarak tanımlanmış erişilebilirliği olabilir, ancak bir ad alanının üyesi olarak belirtilen bir türün yalnızca ortak veya iç olarak tanımlanmış erişilebilirliği olabilir. Son nota]
- Arabirim üyelerinin genel olarak tanımlanmış erişilebilirliği vardır. Arabirim üye bildirimlerinde erişim değiştiricilerine izin verilmez.
- Numaralandırma üyelerinin genel olarak tanımlanmış erişilebilirliği vardır. Numaralandırma üye bildirimlerinde erişim değiştiricilerine izin verilmez.

-----

> Bir program P içindeki T türünde bildirildiği iç içe bir üyenin erişilebilirlik etki alanı, aşağıdaki gibi tanımlanır (M 'nin kendisi bir tür olabilir):
- Belirtilen 8 erişilebilirliği genel ise, d 'nin erişilebilirlik etki alanı T 'nin erişilebilirlik etki alanıdır.
- Belirtilen M 'nin erişilebilirliği korunuyorsa, P 'nin program metninin birleşimi ve T 'den türetilmiş herhangi bir türün program metni ve P dışında bildirilmiştir. M 'nin erişilebilirlik etki alanı, D 'nin erişilebilirlik etki alanının D ile kesişmesi olur.
- **Belirtilen M 'nin erişilebilirliği özel olarak korunuyorsa, P 'nin program metninin kesişimi ve T 'den türetilmiş herhangi bir türdeki program metni için D. M 'nin erişilebilirlik etki alanı, D 'nin erişilebilirlik etki alanının D ile kesişmesi olur.**
- Belirtilen M 'nin erişilebilirliği korunuyorsa, t 'ın program metninin birleşimi ve T 'den türetilmiş herhangi bir türün program metni olacak şekilde D 'yi izin verin. M 'nin erişilebilirlik etki alanı, D 'nin erişilebilirlik etki alanının D ile kesişmesi olur.
- M 'nin bildirildiği erişilebilirlik iç ise, M 'nin erişilebilirlik etki alanı, P 'nin erişilebilirlik etki alanının P. program metniyle kesişmesi olur.
- 5\. olarak tanımlanan erişilebilirlik özel ise, d 'nin erişilebilirlik etki alanı T program metindir.

-----

> Korunan **veya özel bir korumalı** örnek üyesine, bildirildiği sınıfın program metni dışında erişildiğinde ve bir korumalı iç örnek üyesine bildirildikleri programın program metni dışından erişildiğinde, erişim, bildirildiği sınıftan türetilen bir sınıf bildiriminde gerçekleşecektir (). Ayrıca, bu türetilmiş sınıf türünün bir örneği veya ondan oluşturulan bir sınıf türü üzerinden gerçekleşmesi için erişim gerekir. Bu kısıtlama, bir türetilen sınıfın, Üyeler aynı temel sınıftan devralınsa bile, diğer türetilmiş sınıfların korunan üyelerine erişmesini önler.

-----

> İzin verilen erişim değiştiricileri ve bir tür bildirimine yönelik varsayılan erişim, bildirimin gerçekleştiği içeriğe bağlıdır (§ 9.5.2):
- Derleme birimleri veya ad alanlarında belirtilen türlerin ortak veya iç erişimi olabilir. Varsayılan, iç erişimdir.
- Sınıflarda belirtilen türlerin ortak, korumalı iç, **özel korumalı**, korunan, iç veya özel erişimi olabilir. Varsayılan değer özel erişimdir.
- Yapılarda bildirildiği türlerin genel, iç veya özel erişimi olabilir. Varsayılan değer özel erişimdir.

-----

> Statik sınıf bildirimi aşağıdaki kısıtlamalara tabidir:
- Statik bir sınıf, korumalı veya soyut değiştirici içermemelidir. (Ancak, bir statik sınıf tarafından örneklenemez veya türetilemediğinden, hem korumalı hem de soyut gibi davranır.)
- Statik bir sınıf, sınıf tabanlı belirtim (§ 16.2.5) içermemelidir ve açıkça bir temel sınıf veya uygulanan arabirimlerin listesini belirtemez. Statik bir sınıf örtülü olarak tür nesnesinden devralınır.
- Statik bir sınıf yalnızca statik Üyeler (§ 16.4.8) içermelidir. [Note: tüm sabitler ve iç içe türler statik üye olarak sınıflandırılır. Son nota]
- Statik bir sınıf korunan **, özel korumalı** veya korunan iç olarak tanımlanmış erişilebilirliği olan üyelere sahip olamaz.

> Bu kısıtlamaların herhangi birini ihlal etmek için derleme zamanı hatası vardır. 

-----

> Bir sınıf üyesi bildirimi, ~~beş~~**altı** olası tanımlı Erişilebilirlik (§ 9.5.2) türü olabilir: genel, **özel korumalı**, korumalı iç, korunan, iç veya özel. Korunan iç **ve özel korumalı** **birleşim haricinde**, birden fazla erişim değiştiricisi belirtmek için derleme zamanı hatası olur. Bir sınıf üyesi bildirimi herhangi bir erişim değiştiricisi içermiyorsa, Private varsayılır.

-----

> İç içe olmayan türlerde genel veya iç olarak tanımlanmış erişilebilirlik olabilir ve varsayılan olarak iç olarak tanımlanmış erişilebilirliği vardır. İç içe türler, kapsayan türün bir sınıf veya yapı olmasına bağlı olarak, bu tanımlanmış erişilebilirlik ve bir veya daha fazla tanımlanmış erişilebilirlik biçimi içerebilir:
- Bir sınıfta bildirildiği iç içe yerleştirilmiş bir tür, en fazla ~~beş~~**adet** tanımlanmış erişilebilirliği (genel, **özel korumalı**, korunan iç, korunan, iç veya özel) ve diğer sınıf üyeleri gibi özel olarak belirtilen erişilebilirliği varsayılan olarak belirler.
- Bir yapıda tanımlanmış iç içe bir tür, belirtilen üç farklı erişilebilirlik (public, iç veya özel) biçimlerinden birini içerebilir ve diğer yapı üyeleri gibi, özel olarak belirtilen erişilebilirliği varsayılan olarak belirler.

-----

> Bir geçersiz kılma bildirimi tarafından geçersiz kılınan yöntem, c sınıfında belirtilen bir geçersiz kılma yöntemi Için geçersiz kılınan taban yöntemi olarak bilinir, geçersiz kılınan taban yöntemi c 'nin doğrudan temel sınıf türü ile başlayan c ve Her ardışık doğrudan temel sınıf türü ile devam etmek için, belirli bir temel sınıf türüne kadar, tür bağımsız değişkenlerinin yerine geçti ile aynı imzaya sahip en az bir erişilebilir yöntem bulunur. Geçersiz kılınan temel yöntemi bulma amaçları doğrultusunda, bir yöntem, genel ise, korunuyorsa, korunuyorsa **, iç** **veya özel** olarak korunuyorsa veya C ile aynı programda bildirildiği takdirde erişilebilir kabul edilir.

-----

> Erişimci değiştiricilerin kullanılması aşağıdaki kısıtlamalara tabidir:
- Erişimci değiştiricisi bir arabirim içinde veya açık arabirim üyesi uygulamasında kullanılamaz.
- Geçersiz kılma değiştiricisi olmayan bir özellik veya Dizin Oluşturucu için, yalnızca özellik veya dizin oluşturucunun hem Get hem de set erişimcisine sahip olması ve daha sonra yalnızca bu erişimcilerle izin verilen bir erişimci değiştiricisine izin verilir.
- Bir geçersiz kılma değiştiricisi içeren bir özellik veya Dizin Oluşturucu için, erişimci geçersiz kılınmakta olan erişimcinin erişimci değiştiricisiyle eşleşir.
- Erişimci değiştirici, özelliğin veya dizin oluşturucunun kendisi tarafından tanımlanan erişilebilirliğine göre kesinlikle daha kısıtlayıcı bir erişilebilirlik bildirir. Kesin olması için:
  - Özelliğin veya dizin oluşturucunun tanımlanmış bir erişilebilirliği varsa, erişimci değiştirici **özel korumalı**,, korunan iç, dahili, korumalı veya özel olabilir.
  - Özelliğin veya dizin oluşturucunun korunan dahili bir erişilebilirliği varsa, erişimci değiştirici **özel korumalı**, iç, korumalı veya özel olabilir.
  - Özelliğin veya dizin oluşturucunun iç veya korumalı olarak tanımlanmış bir erişilebilirliği varsa, erişimci değiştiricisi **özel korumalı veya** özel olabilir.
  - **Özelliğin veya dizin oluşturucunun özel korumalı olarak tanımlanmış bir erişilebilirliği varsa, erişimci değiştiricisi özel olacaktır.**
  - Özelliğin veya dizin oluşturucunun özel olarak tanımlanmış bir erişilebilirliği varsa, erişimci değiştirici kullanılamaz.

-----

> Yapılar için devralma desteklenmediğinden, bir struct üyesinin tanımlanmış erişilebilirliği korumalı, **özel korumalı**veya korumalı olamaz.

-----

## <a name="drawbacks"></a>Bulunmaktadır
[drawbacks]: #drawbacks

Herhangi bir dil özelliğinde olduğu gibi, bu dilin ek karmaşıklığının, özellikten faydalanabilecek C# programlar gövdesine sunulan ek açıklıkla bir repaıp olup olmadığını sormalıdır.

## <a name="alternatives"></a>Alternatifler
[alternatives]: #alternatives

Bir alternatif, bir özniteliği ve bir çözümleyiciyi birleştiren bir API 'nin sağlanması olacaktır. Özniteliği, üyenin yalnızca alt sınıflarda kullanılması amaçlandığını ve çözümleyici 'nin bu kısıtlamaların ilgili olduğunu kontrol ettiğini belirten bir `internal` üyesine programcı tarafından yerleştirilir. 

## <a name="unresolved-questions"></a>Çözümlenmemiş sorular
[unresolved]: #unresolved-questions

Uygulama büyük ölçüde tamamlanmıştır. Tek açık iş öğesi VB için karşılık gelen bir belirtim taslağı oluşturma.

## <a name="design-meetings"></a>Tasarım toplantıları

TBD
