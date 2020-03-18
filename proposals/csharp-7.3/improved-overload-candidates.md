---
ms.openlocfilehash: 833ea0469149cbd434e8950e844740a3adb4d386
ms.sourcegitcommit: 94a3d151c438d34ede1d99de9eb4ebdc07ba4699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/25/2019
ms.locfileid: "79484596"
---
# <a name="improved-overload-candidates"></a>Geliştirilmiş aşırı yükleme adayları

## <a name="summary"></a>Özet
[summary]: #summary

Aşırı yükleme çözümleme kuralları, programcılar için deneyimi geliştirmek üzere C# neredeyse her dil güncelleştirmesinde güncelleştirilmiştir, belirsiz etkinleştirmeleri yaparak "açık" seçeneğini belirleyin. Bu, geriye dönük uyumluluğu korumak için dikkatle yapılmalıdır, ancak genellikle hata durumlarının ne olduğunu çözümlediğimiz için, bu geliştirmeler genellikle düzgün şekilde çalışır.

1. Bir yöntem grubu hem örnek hem de statik üye içerdiğinde, örnek üyelerini bir örnek alıcısı veya bağlamı olmadan çağrılırsa atar ve örnek alıcısıyla çağrılırsa statik üyeleri atar. Alıcı olmadığında, statik bir bağlamda yalnızca statik üyeler ve statik ve örnek üyeleri dahil ediyoruz. Alıcı bir renk rengi durumu nedeniyle bir örnek veya tür ındexattributes olduğunda, her ikisini de dahil ediyoruz. Örtük olarak bu örnek alıcının kullanılabileceği statik bir bağlam, statik üyeler gibi bu tanımlı olmayan üyelerin gövdesini ve alan başlatıcıları ve Oluşturucu başlatıcıları gibi bu kullanılamayacak yerleri içerir.
2. Bir yöntem grubu, tür bağımsız değişkenleri kısıtlamalarını karşılamadığı bazı genel yöntemler içerdiğinde, bu üyeler aday kümesinden kaldırılır.
3. Bir yöntem grubu dönüştürmesi için, dönüş türü, temsilcinin dönüş türüyle eşleşmeyen aday Yöntemler kümeden kaldırılır.
