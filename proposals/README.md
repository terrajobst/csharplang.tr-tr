---
ms.openlocfilehash: aa0c233e7739ae7a0a7752251aae6f89df18ba93
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484519"
---
# <a name="c-language-proposals"></a>C#Dil teklifleri

Dil teklifleri, belirli bir dil özelliği hakkında geçerli düşünmeyi açıklayan çok fazla belgelerdir.

Teklifler *etkin*, *etkin değil*, *reddedildi* veya *bitti*olabilir. *Etkin* teklifler doğrudan teklifler klasöründe depolanır, *etkin olmayan* ve *Reddedilen* teklifler [etkin olmayan](proposals/inactive) ve [Reddedilen](proposals/rejected) alt klasörlerde depolanır ve *bitti* teklifleri, bir parçası oldukları dil sürümüne karşılık gelen bir klasörde arşivlenir.

## <a name="lifetime-of-a-proposal"></a>Teklifin ömrü

Dil tasarımı ekibi, dile bir gün daha iyi bir ekleme yapmaya karar verdiğinde, bir teklif ömrünü başlatır. Genellikle *etkin*hale gelir, ancak şu anda üzerinde çalışmak zorunda kalmadan bir fikir yakalamak istiyoruz, *etkin olmayan* alt klasörde da bir öneri de başlatılabilir. Önerdiğimiz bir şeyin kaydını yapmak istiyorsam, teklifler doğrudan *reddedildi* durumunda başlatılabilir. Örneğin, popüler ve yinelenen bir isteğin uygulanması mümkün değilse, bunu reddedilmiş bir teklif olarak yakalayabilirsiniz.

Teklif, bir [tartışma sorunuyla](https://github.com/dotnet/csharplang/labels/Discussion)ilgili bir fikir olarak başlayabilir veya dil tasarımı toplantısının tartışmadan gelebilir ya da birçok farklı kaynaktan gelebiliriz. Ana şey, Tasarım ekibinin yapılması gerektiğini ve bunu yazmak isteyen bir kişi olduğunu Fedir. Genellikle dil tasarımı ekibinin bir üyesi, bu özellik için kendi kendini bir şüme olarak atar, bu da bir [esampiyon sorunuyla](https://github.com/dotnet/csharplang/labels/Proposal%20champion)izlenir. Şampiyon, teklifin tasarım sürecinde taşınmasından sorumludur.

Bir teklif, gelecek bir sürüme doğru tasarım ve uygulama aracılığıyla ileriye doğru hareket ettirilerek *etkin* olur. Tamamen *yapıldıktan*sonra, yani bir uygulama bir yayınla birleştirilir ve özellik belirtilmişse, sürümüne karşılık gelen bir alt dizine taşınır.

Bir özellik, herhangi bir dilde bir dile dönüştürmek için mümkün değilse, örneğin, mümkün olmayan bir değer eklememesi ya da dil için doğru olmadığından, [reddedilir](proposals/rejected). Bir özelliğin makul taahhüdüne sahip olması, ancak şu anda üzerinde çalışma önceliği yoksa, ana klasörü karışıklık vermekten kaçınmak için [etkin](proposals/inactive) değil olarak bildirilemez. Etkin olmayan veya reddedilmiş tekliflerle çalışmanın gerçekleşmesi ve bunların daha sonra yeniden başlatılması için çok iyi bir hale gelir. Kategoriler, geçerli Tasarım hedefini yansıtacak şekilde yapılır.

## <a name="nature-of-a-proposal"></a>Teklifin doğası

Teklif, [Teklif şablonunu](proposal-template.md)izlemelidir. İyi bir teklif:

- Genel ruıt ve dilin Aesthetic Characteristics ile uyum yapın.
- Var olan özellikler için daha hafif alternatif sözdizimi sunmaz.
- Açık bir kullanıcı kümesi için büyük miktarda değer ekleyin.
- Özellikle yeni kullanıcılar için dilin karmaşıklığına önemli ölçüde eklemeyin.  

## <a name="discussion-of-proposals"></a>Tekliflerin tartışması

[Tartışma sorunlarında](https://github.com/dotnet/csharplang/labels/Discussion)geri bildirim ve tartışma oluşur. Teklifler klasörüne yeni bir teklif eklendiğinde, bu, bir tartışma sorunu veya teklif yazarı tarafından duyurulmalıdır. 

 