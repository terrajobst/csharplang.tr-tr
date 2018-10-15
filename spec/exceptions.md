# <a name="exceptions"></a>Özel Durumlar

C# dilinde özel durumlar hem sistem düzeyinde hem de uygulama düzeyinde işleme yapılandırılmış, Tekdüzen ve tür kullanımı uyumlu bir yol sağlar hata koşulları. C# dilinde özel durum mekanizması ile bazı önemli farklar oldukça, C++, benzer:

*  C# ' ta tüm özel durumlar, türetilen bir sınıf türünün bir örneği tarafından temsil edilmesi gereken `System.Exception`. C++'da, herhangi bir değer herhangi bir türde bir özel durum temsil etmek için kullanılabilir.
*  C# ' ta, bir finally bloğunda ([try deyimi](statements.md#the-try-statement)) hem normal yürütmenin hem de özel durumları yürüten sonlandırma kodu yazmak için kullanılır. Bu tür kod kod yinelemeden C++ olarak yazabilir zordur.
*  C# ' ta, sistem düzeyinde özel durumlar gibi taşma sıfırla bölme ve null başvuru özel durum sınıfları iyi tanımlanmış ve uygulama düzeyi hata koşulları ile ilgili bir değer alır.

## <a name="causes-of-exceptions"></a>Özel durumların nedenlerini

İki farklı şekilde, özel durum.

*  A `throw` deyimi ([fırlatma](statements.md#the-throw-statement)) koşulsuz olarak ve hemen bir özel durum oluşturur. Denetim, hiçbir zaman hemen sonrasındaki deyime ulaştığında `throw`.
*  İşlem normal şekilde tamamlanamıyor ifadesi C# ifadeleri ve işleme sırasında ortaya belirli özel durumları bazı durumlarda bir özel durum neden. Örneğin, bir tamsayı bölme işlemi ([bölme işleci](expressions.md#division-operator)) oluşturur bir `System.DivideByZeroException` payda sıfır ise. Bkz: [genel özel durum sınıfları](exceptions.md#common-exception-classes) bu şekilde oluşabilecek çeşitli özel durumların bir listesi için.

## <a name="the-systemexception-class"></a>System.Exception sınıfı

`System.Exception` Sınıfı, tüm özel durumlar'ın temel türü. Bu sınıf, tüm özel durumları paylaşan birkaç önemli özelliklere sahiptir:

*  `Message` salt okunur bir özellik türü `string` , insanlar tarafından okunabilen bir özel durumun nedenini açıklamasını içerir.
*  `InnerException` salt okunur bir özellik türü `Exception`. Değeri null olmayan ise, geçerli özel duruma neden olan özel durum başvurduğu — diğer bir deyişle, geçerli özel durum bir catch bloğu içinde tetiklendi işleme `InnerException`. Aksi takdirde, değeri bu özel durumun başka bir özel durumun nedeni olduğunu gösteren null olur. Bu şekilde birbirine zincirlenmiş özel durum nesne sayısını rastgele olabilir.

Bu özelliklerin değerini örnek oluşturucusu için yapılan çağrıda belirtilen `System.Exception`.

## <a name="how-exceptions-are-handled"></a>Özel durumları nasıl işlenir?

Özel durumlar tarafından işlendiğinden bir `try` deyimi ([try deyimi](statements.md#the-try-statement)).

Bir özel durum oluştuğunda sistem arar en yakın `catch` çalışma zamanı özel durum türüne göre belirlenen özel durumu işleyebilen yan tümcesi. İlk olarak, geçerli yöntemi bir sözcüksel olarak kapsayan için aranır `try` deyimi ve ilişkili bir catch yan tümceleri, try deyimi sırayla değerlendirilir. Bu başarısız olursa geçerli yöntemi çağıran yönteme bir sözcüksel olarak kapsayan için aranır `try` geçerli yöntemine çağrı noktasındaki kapsayan deyimi. Bu arama kadar devam bir `catch` yan tümcesi işleyebilen geçerli özel durum, aynı sınıfa veya temel bir sınıfı, çalışma zamanı türü olan özel durum bir özel durum sınıfı adlandırarak bulundu. A `catch` bir özel durum sınıfı adı değil yan tümcesi tüm özel işleyebilir.

Eşleşen bir catch yan tümcesi bulunduktan sonra sistem denetimi catch yan tümcesinin ilk deyime aktarmak hazırlar. Catch yan tümcesinin yürütme başlamadan önce sistem, sırasıyla herhangi yürütür `finally` daha fazla deneyin deyimleri ile ilişkili yan tümceleri iç içe geçmiş, özel durum yakalandı hesaptan.

Eşleşen bir catch yan tümcesi bulunursa ikisinden biri gerçekleşir:

*  Eşleşen bir catch yan tümcesi arama statik Oluşturucu ulaşırsa ([statik oluşturucular](classes.md#static-constructors)) veya statik alan başlatıcısında bir `System.TypeInitializationException` statik oluşturucunun çağrılacağını tetikleyen noktada oluşturulur. İç özel duruma `System.TypeInitializationException` orijinal olarak oluşturulan özel durum içerir.
*  Catch yan tümceleri eşleştirme için arama ilk iş parçacığı çalışmaya kod ulaşırsa, iş parçacığının yürütülmesini sona erdirilir. Bu tür sonlandırma uygulama tanımlı etkisidir.

Yıkıcı yürütme sırasında gerçekleşen özel olarak belirtilmelidir özel durumlardır. Yıkıcı yürütülürken bir özel durum oluşur ve bu özel durum yakalanmadı, bu yok edici yürütülmesi sonlandırıldı ve (varsa) temel sınıfın yok Edicisi çağrılır. Hiçbir temel sınıf varsa (olarak durumunda `object` türü) veya hiçbir taban sınıf yok edicisini yoktur. ardından özel durumu atılır.

## <a name="common-exception-classes"></a>Sık karşılaşılan özel durum sınıfları

Aşağıdaki özel durumlar, belirli C# işlemleri tarafından atılır.

|                                      |                |
|--------------------------------------|----------------|
| `System.ArithmeticException`         | Gibi aritmetik işlemler sırasında oluşan özel durumlar için temel sınıf `System.DivideByZeroException` ve `System.OverflowException`. | 
| `System.ArrayTypeMismatchException`  | Gerçek depolanan öğenin türünü dizinin gerçek türü ile uyumsuz olduğundan bir depolama dizisine başarısız olduğunda oluşturulur. | 
| `System.DivideByZeroException`       | Bir tamsayı değeri sıfıra bölünme girişimi oluştuğunda oluşturulur. | 
| `System.IndexOutOfRangeException`    | Sıfırdan küçük veya dizi sınırları dışında bir dizin aracılığıyla bir dizi dizini girişimi zaman oluşturulur. | 
| `System.InvalidCastException`        | Çalışma zamanında bir taban türü veya arabirim için türetilmiş bir tür açık bir dönüştürme başarısız olduğunda oluşturulur. | 
| `System.NullReferenceException`      | Oluşan bir `null` başvurusu gerekli olarak başvurulan nesnenin neden olan bir şekilde kullanılır. | 
| `System.OutOfMemoryException`        | Bellek ayırma girişimi zaman oluşturulur (aracılığıyla `new`) başarısız olur. | 
| `System.OverflowException`           | Aritmetik işlemin içinde olduğunda oluşturulan bir `checked` bağlam taşıyor. | 
| `System.StackOverflowException`      | Çok fazla bekleyen yöntem çağrılarını sağlayarak yürütme yığın kaldığında oluşturulur; genellikle vurgulayan çok derin ya da sınırlandırılmamış özyineleme. | 
| `System.TypeInitializationException` | Statik oluşturucu yok ve özel durum oluşturduğunda durum `catch` yan tümceleri var. Bunu yakalayıp yakalamayacağınıza karar. | 
