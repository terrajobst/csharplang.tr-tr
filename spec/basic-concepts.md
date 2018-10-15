# <a name="basic-concepts"></a><span data-ttu-id="55c2b-101">Temel kavramlar</span><span class="sxs-lookup"><span data-stu-id="55c2b-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="55c2b-102">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="55c2b-102">Application Startup</span></span>

<span data-ttu-id="55c2b-103">Bir derlemeye bir ***giriş noktası*** çağrılır bir ***uygulama***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="55c2b-104">Bir uygulama olduğunda çalıştırın, yeni bir ***uygulama etki alanı*** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="55c2b-105">Bir uygulamayı birkaç farklı örneklemeleri aynı makinede aynı anda mevcut olabilir ve her biri kendi uygulama etki alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="55c2b-106">Uygulama etki alanı, uygulama durumu için kapsayıcı olarak çalışan tarafından uygulama yalıtımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="55c2b-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="55c2b-107">Uygulama etki alanı, bir kapsayıcı ve uygulama ve kullandığı sınıf kitaplıkları içinde tanımlanan türler için sınır olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="55c2b-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="55c2b-108">Aynı türden başka bir uygulama etki alanına yüklenen bir uygulama etki alanına yüklenen türleri farklıdır ve nesnelerin örneklerini uygulama etki alanları arasında doğrudan paylaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="55c2b-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="55c2b-109">Örneğin, her uygulama etki alanı statik değişkenler bu türleri için kendi kopyasına sahip ve bir tür için bir statik Oluşturucu uygulama etki alanı başına en fazla bir kez çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="55c2b-110">Uygulamaları oluşturma ve yok edilmesini uygulama etki alanları için uygulamaya özel ilke veya mekanizmaları sağlamak ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="55c2b-111">***Uygulama başlatma*** yürütme ortamı uygulamanın giriş noktası olarak adlandırılan belirlenen bir yöntemi çağırdığında gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="55c2b-112">Bu giriş noktası yönteminin her zaman adlı `Main`ve aşağıdaki imzalara sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="55c2b-113">Gösterildiği gibi giriş noktası isteğe bağlı olarak döndürebilir bir `int` değeri.</span><span class="sxs-lookup"><span data-stu-id="55c2b-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="55c2b-114">Bu dönüş değeri, uygulama sonlandırılmasıyla kullanılır ([uygulama sonlandırma](basic-concepts.md#application-termination)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="55c2b-115">Giriş noktası isteğe bağlı olarak bir biçimsel parametreye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="55c2b-116">Parametresi, herhangi bir ad olabilir, ancak parametresinin türü olmalıdır `string[]`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="55c2b-117">Biçimsel parametre varsa, yürütme ortamı oluşturur ve başarılı bir `string[]` olan komut satırı bağımsız değişkenleri içeren değişken belirtilen uygulama başlatıldığı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="55c2b-118">`string[]` Bağımsız değişken NULL'dur hiçbir zaman, ancak hiçbir komut satırı bağımsız değişkenleri belirtilmişse sıfır uzunluğuna sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="55c2b-119">C# yöntemi aşırı yüklemesi desteklediğinden, sağlanan her farklı bir imzaya sahip bir sınıfın veya yapının bazı yöntemi birden çok tanımları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="55c2b-120">Ancak, tek bir program içindeki herhangi bir sınıf veya yapı adında birden fazla yöntem içerebilir `Main` tanımında bu uygulaması giriş noktası olarak kullanılacak niteliği taşır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="55c2b-121">Diğer aşırı yüklenmiş sürümleri `Main` , ancak birden fazla parametre sahip oldukları ya da yalnızca kendi parametre türüdür dışında sağlanan izin verilen `string[]`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="55c2b-122">Bir uygulamanın birden fazla sınıflar veya yapılar oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="55c2b-123">Adlı yöntemi içeren birden fazla bu sınıflar veya yapılar için mümkün olduğu `Main` tanımında bu uygulaması giriş noktası olarak kullanılacak niteliği taşır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="55c2b-124">Böyle durumlarda (örneğin, bir komut satırı derleyicisi seçeneği) veren dış mekanizmayı bunlardan birini seçmek için kullanılmalıdır `Main` giriş noktası olarak yöntemler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="55c2b-125">C# ' ta her yöntemi bir sınıf veya yapı üyesi olarak tanımlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="55c2b-126">Normalde, bildirilen erişilebilirliği ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)) yöntemi tarafından erişim değiştiricileri belirlenir ([erişim değiştiricilerine](classes.md#access-modifiers)) bildiriminden ve benzer şekilde bildirilen belirtilen bir türün erişilebilirlik bildiriminde belirtilen erişim değiştiricileri tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="55c2b-127">Çağrılabilir olması için sırada belirli bir yöntemin belirli bir türün, tür ve üye hem erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="55c2b-128">Ancak, uygulama giriş noktası bir özel durumdur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="55c2b-129">Özellikle, yürütme ortamı, uygulamanın giriş noktasını bakılmaksızın kendi bildirilen erişilebilirliği ve öğesinin bildirilen erişilebilirliği kapsayan kendi tür bildirimleri bağımsız olarak erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55c2b-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="55c2b-130">Uygulama giriş noktası yöntemi, bir genel sınıf bildiriminde olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="55c2b-131">Diğer tüm yönden girdi noktası yöntemleri giriş noktası olmayan olanlar gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="55c2b-132">Uygulamayı sonlandırma</span><span class="sxs-lookup"><span data-stu-id="55c2b-132">Application termination</span></span>

<span data-ttu-id="55c2b-133">***Uygulama sonlandırma*** yürütme ortamı denetimini döndürür.</span><span class="sxs-lookup"><span data-stu-id="55c2b-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="55c2b-134">Uygulamanın, dönüş türü ***giriş noktası*** yöntemi `int`, döndürülen değer uygulamanın hizmet veren ***sonlandırma durum kodu***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="55c2b-135">Bu kodun amacı, başarı veya başarısızlık yürütme ortamı için iletişime izin vermektir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="55c2b-136">Giriş noktası yönteminin dönüş türü ise `void`, sağ ayraç ulaşmasını (`}`), sonlandıran yöntemi veya yürütülürken bir `return` olan herhangi bir ifade deyimi sonlandırma durum koduna sonuçları `0`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="55c2b-137">Bu tür temizleme atlanmadıkları sürece uygulamanın sonlanması öncesinde yok ediciler için tüm henüz çöp olarak toplanacak olan nesneleri, çağrılır (kitaplık yöntemine bir çağrıyla `GC.SuppressFinalize`, örneğin).</span><span class="sxs-lookup"><span data-stu-id="55c2b-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="55c2b-138">Bildirimler</span><span class="sxs-lookup"><span data-stu-id="55c2b-138">Declarations</span></span>

<span data-ttu-id="55c2b-139">Bildirimleri bir C# programında, programın şekli oluşturan öğeler tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="55c2b-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="55c2b-140">C# programları, ad alanlarını kullanarak düzenlenir ([ad alanları](namespaces.md)), türü içerebilecek bildirimler ve iç içe geçmiş ad alanı bildirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="55c2b-141">Tür bildirimleri ([tür bildirimleri](namespaces.md#type-declarations)) sınıflarını tanımlamak için kullanılır ([sınıfları](classes.md)), yapılar ([yapılar](structs.md)), arabirimleri ([arabirimleri](interfaces.md) ), sabit listeleri ([numaralandırmalar](enums.md)) ve temsilciler ([Temsilciler](delegates.md)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="55c2b-142">Türde üye türü bildiriminde izin türü bildirimi formunda bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="55c2b-143">Örneğin, sınıf bildirimleri sabitleri için bildirimi içerebilir ([sabitleri](classes.md#constants)), alanlar ([alanları](classes.md#fields)), yöntemleri ([yöntemleri](classes.md#methods)), özellikleri ([ Özellikleri](classes.md#properties)), olaylar ([olayları](classes.md#events)), dizin oluşturucular ([dizin oluşturucular](classes.md#indexers)), işleçler ([işleçleri](classes.md#operators)), örnek oluşturucuları ([ Örnek oluşturucuları](classes.md#instance-constructors)), statik oluşturucular ([statik oluşturucular](classes.md#static-constructors)), Yıkıcılar ([yok ediciler](classes.md#destructors)) ve iç içe geçmiş türler ([iç içe türler](classes.md#nested-types)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="55c2b-144">Bir bildirimi bir ad tanımlar ***bildirim alanı*** bildirimi ait olduğu.</span><span class="sxs-lookup"><span data-stu-id="55c2b-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="55c2b-145">Hariç üyeler aşırı ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir bildirim alanında aynı ada sahip üye tanıtan iki veya daha fazla bildirimleri sağlamak için bir derleme zamanı hata.</span><span class="sxs-lookup"><span data-stu-id="55c2b-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="55c2b-146">Aynı ada sahip üyelerinin farklı türleri içeren bir bildirim alanı için hiçbir zaman mümkündür.</span><span class="sxs-lookup"><span data-stu-id="55c2b-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="55c2b-147">Örneğin, bir bildirim alanı hiçbir zaman alan ve bir yöntem aynı adla içerebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="55c2b-148">Aşağıda açıklandığı gibi birkaç farklı türde bildirimi alanları vardır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="55c2b-149">Bir programın tüm kaynak dosyaları içinde *namespace_member_declaration*hiçbir kapsayan ile s *namespace_declaration* adlı tek bir birleştirilmiş bildirimi boşluk üyeleri ***genel Bildirim alanı***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="55c2b-150">Bir programın tüm kaynak dosyaları içinde *namespace_member_declaration*ürününe *namespace_declaration*tam ad alanı ile aynı ada sahip s olan tek bir birleştirilmiş bildirimine üyeleri boşluk.</span><span class="sxs-lookup"><span data-stu-id="55c2b-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="55c2b-151">Yeni bir bildirim alanı her sınıf, yapı veya arabirim bildirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="55c2b-152">Adları kullanıma sunulan bu bildirim alanına *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, veya *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="55c2b-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="55c2b-153">Aşırı yüklenmiş bir örnek oluşturucusu hariç, bildirimler ve statik Oluşturucu bildirimi, bir sınıfın veya yapının sınıfın veya yapının aynı ada sahip bir üye bildirimi içeremez.</span><span class="sxs-lookup"><span data-stu-id="55c2b-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="55c2b-154">Bir sınıf, yapı veya arabirim aşırı yüklenmiş yöntemler ve dizin oluşturucular bildirimi izin verir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="55c2b-155">Ayrıca, bir sınıf veya yapı bildirimi aşırı yüklenmiş örnek oluşturucuları ve işleçleri izin verir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="55c2b-156">Bu yöntem bildirimleri kendi imzasında farklı sağlanan Örneğin, bir sınıf, yapı veya arabirim aynı ada sahip birden çok yöntem bildirimleri içerebilir ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="55c2b-157">Temel sınıflar bir sınıfın bildiriminin alanına katkıda bulunan değil ve temel arabirimleri bildirim alanına bir arabirimin katkıda bulunmuyor unutmayın.</span><span class="sxs-lookup"><span data-stu-id="55c2b-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="55c2b-158">Bu nedenle, türetilen bir sınıfta veya arabirimde aynı ada sahip bir üyesi devralınmış bir üyeyi olarak bildirin izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="55c2b-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="55c2b-159">Böyle bir üyesi edilir ***Gizle*** devralınan üye.</span><span class="sxs-lookup"><span data-stu-id="55c2b-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="55c2b-160">Yeni bir bildirim alanı her temsilci bildirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="55c2b-161">Adları bu bildirim alanına biçimsel parametreler aracılığıyla kullanıma sunulmuştur (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="55c2b-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="55c2b-162">Yeni bir bildirim alanı her numaralandırma bildirimi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="55c2b-163">Adları kullanıma sunulan bu bildirim alanına *enum_member_declarations*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="55c2b-164">Her yöntem bildiriminde, dizin oluşturucu bildirimi, işleç bildirimi, kopya Oluşturucusu bildirimi ve anonim işlev adı verilen yeni bir bildirim alanı oluşturur bir ***yerel değişken bildiriminde yer***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="55c2b-165">Adları bu bildirim alanına biçimsel parametreler aracılığıyla kullanıma sunulmuştur (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s.</span><span class="sxs-lookup"><span data-stu-id="55c2b-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="55c2b-166">İşlev üyesi veya anonim işlev gövdesinin varsa, yerel değişken bildiriminde alanı içinde iç içe geçmiş kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="55c2b-167">Bir yerel değişken bildiriminde alan ve aynı ada sahip öğeleri içeren bir iç içe yerel değişken bildiriminde alan için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="55c2b-168">Bu nedenle, bir iç içe geçmiş bir bildirim alanı içinde yerel bir değişken veya yerel bir değişken olarak aynı ada sahip sabit veya sabit kapsayan bir bildirim alanı bildirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="55c2b-169">Diğer bildirim boşluk içerdiği sürece aynı ada sahip öğeleri içeren iki bildirimi boşluk mümkündür.</span><span class="sxs-lookup"><span data-stu-id="55c2b-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="55c2b-170">Her *blok* veya *switch_block* , hem de bir *için*, *foreach* ve *kullanarak* deyimi, oluşturur bir yerel değişken bildiriminde yer yerel değişkenleri ve yerel sabitleri için.</span><span class="sxs-lookup"><span data-stu-id="55c2b-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="55c2b-171">Adları kullanıma sunulan bu bildirim alanına *local_variable_declaration*s ve *local_constant_declaration*s.</span><span class="sxs-lookup"><span data-stu-id="55c2b-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="55c2b-172">İçinde kendi parametrelerini için bu işlevleri tarafından bildirilen yerel değişken bildiriminde yer olarak veya bir üye işlev veya anonim işlev gövdesi içinde oluşan bloklar yerleştirilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="55c2b-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="55c2b-173">Bu nedenle, örneğin bir yerel değişken ve aynı ada sahip bir parametre ile bir yöntem sağlamak için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="55c2b-174">Her *blok* veya *switch_block* etiketleri için ayrı bildirim boşluk oluşturur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="55c2b-175">Adları kullanıma sunulan bu bildirim alanına *labeled_statement*s yanı sıra, adları başvuru aracılığıyla *goto_statement*s.</span><span class="sxs-lookup"><span data-stu-id="55c2b-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="55c2b-176">***Etiket bildirim alanı*** bloğu tüm iç içe geçmiş blokları içerir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="55c2b-177">Bu nedenle, iç içe geçmiş bir bloğu içinde kapsayan bir blok içinde bir etiket olarak aynı ada sahip bir etiket bildirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="55c2b-178">Adları bildirilen metinsel genellikle hiçbir önemini sırasıdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="55c2b-179">Özellikle, metinsel sırası önemli bildirimler ve ad alanları, sabitleri, yöntemleri, özellikleri, olayları, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler, statik oluşturucular ve türleri kullanımı için değildir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="55c2b-180">Aşağıdaki yollarla bildirim sırası önemlidir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="55c2b-181">Alan bildirimleri ve yerel değişken bildirimleri için bildirim sırasında kendi başlatıcıları (varsa) yürütüldüğü sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="55c2b-182">Yerel değişkenler, kullanılmadan önce tanımlanmalıdır ([kapsamları](basic-concepts.md#scopes)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="55c2b-183">Sabit listesi üye bildirimleri için bildirim sırasında ([numaralandırma üyeleri](enums.md#enum-members)) önemli olduğunda *constant_expression* değerleri atlanmış.</span><span class="sxs-lookup"><span data-stu-id="55c2b-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="55c2b-184">Bildirimi bir ad alanının "Bitti" açık"ve iki ad alanı bildirimleri aynı tam ada sahip aynı bildirim alanına katkıda bulunan bir alandır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="55c2b-185">Örneğin</span><span class="sxs-lookup"><span data-stu-id="55c2b-185">For example</span></span>
```csharp
namespace Megacorp.Data
{
    class Customer
    {
        ...
    }
}

namespace Megacorp.Data
{
    class Order
    {
        ...
    }
}
```

<span data-ttu-id="55c2b-186">Bu durumda tam olarak nitelenmiş ada sahip iki sınıfları bildirme aynı bildirim alanına, yukarıdaki iki ad alanı bildirimi katkıda `Megacorp.Data.Customer` ve `Megacorp.Data.Order`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="55c2b-187">İki bildirimi aynı bildirim alanına katkıda bulunan, her aynı ada sahip bir sınıf bildirimi içeriyorsa, bir derleme zamanı hatası nedeni.</span><span class="sxs-lookup"><span data-stu-id="55c2b-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="55c2b-188">Bildirim alanı bir blok olarak yukarıda belirtilen, herhangi bir iç içe geçmiş bloğu içerir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="55c2b-189">Bu nedenle, aşağıdaki örnekte, `F` ve `G` yöntemleri, çünkü bir derleme zamanı hatası neden adı `i` dış blokta bildirilen ve iç bloğu içinde bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="55c2b-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="55c2b-190">Ancak, `H` ve `I` yöntemleri iki itibaren geçerlidir `i`ait ayrı iç içe olmayan bloklar olarak bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

```csharp
class A
{
    void F() {
        int i = 0;
        if (true) {
            int i = 1;            
        }
    }

    void G() {
        if (true) {
            int i = 0;
        }
        int i = 1;                
    }

    void H() {
        if (true) {
            int i = 0;
        }
        if (true) {
            int i = 1;
        }
    }

    void I() {
        for (int i = 0; i < 10; i++)
            H();
        for (int i = 0; i < 10; i++)
            H();
    }
}
```

## <a name="members"></a><span data-ttu-id="55c2b-191">Üyeler</span><span class="sxs-lookup"><span data-stu-id="55c2b-191">Members</span></span>

<span data-ttu-id="55c2b-192">Ad alanları ve türler sahip ***üyeleri***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="55c2b-193">Bir varlık üyeleri ve ardından varlığa bir başvuru ile başlayan bir tam adı kullanılarak genel kullanıma sunulmuştur bir "`.`" belirteci, takip eden üyenin adı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="55c2b-194">Bir türün üyeleri ya da tür bildiriminde bildirilen veya ***devralınan*** türü temel sınıftan.</span><span class="sxs-lookup"><span data-stu-id="55c2b-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="55c2b-195">Bir tür bir temel sınıftan örnek Oluşturucular, Yıkıcılar ve statik oluşturucular dışında temel sınıfın tüm üyeleri devralan üyeler türetilmiş bir türde olur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="55c2b-196">Bir temel sınıf üyenin bildirilen erişilebilirliği, üye devralınan olup olmadığını denetlemez; devralma bir örnek oluşturucusu, statik oluşturucu veya yıkıcı olmayan herhangi bir üyeye genişletir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="55c2b-197">Ancak, devralınan bir üyeyi bir türetilmiş türde, ya da bildirilen erişilebilirliğini nedeniyle erişilemiyor olabilir ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)) veya bir tür bildiriminde tarafından gizlendiği ([aracılığıyla gizleme Devralma](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="55c2b-198">Namespace üyelerini</span><span class="sxs-lookup"><span data-stu-id="55c2b-198">Namespace members</span></span>

<span data-ttu-id="55c2b-199">Ad alanları ve kapsayan ad uzayı olmayan türleri olan üyeleri ***genel ad alanı***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="55c2b-200">Bu doğrudan genel bildirim alanında bildirilen adların karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="55c2b-201">Bu ad, ad alanları ve ad alanı içinde bildirilen türler üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="55c2b-202">Bu doğrudan ad alanı bildirimi alana bildirilen adların karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="55c2b-203">Ad alanları, hiçbir erişim kısıtlamasına sahip.</span><span class="sxs-lookup"><span data-stu-id="55c2b-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="55c2b-204">Özel, korumalı veya iç ad alanı bildirmek mümkün değildir ve ad alanı adları her zaman genel olarak erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="55c2b-205">Yapı üyeleri</span><span class="sxs-lookup"><span data-stu-id="55c2b-205">Struct members</span></span>

<span data-ttu-id="55c2b-206">Yapı üyeleri struct bildirilen üyeler ve yapının doğrudan temel sınıftan devralınan üyeleri olan `System.ValueType` ve dolaylı temel sınıfı `object`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="55c2b-207">Basit bir tür üyelerini doğrudan yapı türü diğer adlı basit bir tür tarafından üyelerinin karşılık gelir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="55c2b-208">Üyeleri `sbyte` üyeleri `System.SByte` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="55c2b-209">Üyeleri `byte` üyeleri `System.Byte` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="55c2b-210">Üyeleri `short` üyeleri `System.Int16` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="55c2b-211">Üyeleri `ushort` üyeleri `System.UInt16` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="55c2b-212">Üyeleri `int` üyeleri `System.Int32` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="55c2b-213">Üyeleri `uint` üyeleri `System.UInt32` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="55c2b-214">Üyeleri `long` üyeleri `System.Int64` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="55c2b-215">Üyeleri `ulong` üyeleri `System.UInt64` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="55c2b-216">Üyeleri `char` üyeleri `System.Char` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="55c2b-217">Üyeleri `float` üyeleri `System.Single` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="55c2b-218">Üyeleri `double` üyeleri `System.Double` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="55c2b-219">Üyeleri `decimal` üyeleri `System.Decimal` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="55c2b-220">Üyeleri `bool` üyeleri `System.Boolean` yapısı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="55c2b-221">Numaralandırma üyeleri</span><span class="sxs-lookup"><span data-stu-id="55c2b-221">Enumeration members</span></span>

<span data-ttu-id="55c2b-222">Bir numaralandırma sabit listesinde bildirilen sabitler ve sabit listesinin doğrudan temel sınıftan devralınan üyeleri üyeleri `System.Enum` ve dolaylı temel sınıfların `System.ValueType` ve `object`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="55c2b-223">Sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="55c2b-223">Class members</span></span>

<span data-ttu-id="55c2b-224">Bir sınıfın üyeleri sınıfta bildirilen üyeleri ve temel sınıftan devralınan üyeleri olan (sınıf dışında `object` temel sınıfa sahip).</span><span class="sxs-lookup"><span data-stu-id="55c2b-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="55c2b-225">Temel sınıftan devralınan üyeleri, sabitleri, alanları, yöntemleri, özellikleri, olayları, dizin oluşturucular, işleçler ve tür temel sınıfı, ancak örnek Oluşturucular, Yıkıcılar ve temel sınıfın statik oluşturucular içerir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="55c2b-226">Temel sınıf üyelerinin bakılmaksızın kendi erişilebilirlik devralınır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="55c2b-227">Bir sınıf bildirimi bildirimlerini sabitleri, alanları, yöntemler, özellikler, olaylar, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler, statik oluşturucular ve türleri içerebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="55c2b-228">Üyeleri `object` ve `string` karşılık gelen sınıf türü üyeleri doğrudan bunlar diğer adı:</span><span class="sxs-lookup"><span data-stu-id="55c2b-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="55c2b-229">Üyeleri `object` üyeleri `System.Object` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="55c2b-230">Üyeleri `string` üyeleri `System.String` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="55c2b-231">Arabirim üyeleri</span><span class="sxs-lookup"><span data-stu-id="55c2b-231">Interface members</span></span>

<span data-ttu-id="55c2b-232">Bir arabirim üyelerinin arabirimi ve tüm temel arabirimleri arabiriminin bildirilen üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="55c2b-233">Sınıf üyeleri `object` kesinlikle anlamda, herhangi bir arabirim üyesi değilseniz, ([arabirim üyeleri](interfaces.md#interface-members)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="55c2b-234">Ancak, sınıf üyeleri `object` herhangi bir arabirim türüne içinde'üye araması aracılığıyla kullanılabilir ([üye araması](expressions.md#member-lookup)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="55c2b-235">Dizi üyeleri</span><span class="sxs-lookup"><span data-stu-id="55c2b-235">Array members</span></span>

<span data-ttu-id="55c2b-236">Dizi üyelerinin sınıftan devralınan üyeleri `System.Array`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="55c2b-237">Temsilci üye</span><span class="sxs-lookup"><span data-stu-id="55c2b-237">Delegate members</span></span>

<span data-ttu-id="55c2b-238">Bir temsilci sınıftan devralınan üyeleri üyeleri `System.Delegate`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="55c2b-239">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="55c2b-239">Member access</span></span>

<span data-ttu-id="55c2b-240">Üye bildirimlerini üye erişimi denetlemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="55c2b-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="55c2b-241">Bir üyenin erişilebilirliğini tarafından bildirilen erişilebilirliği kurulur ([erişilebilirlik bildirilen](basic-concepts.md#declared-accessibility)) üyesi varsa hemen içeren türün erişilebilirlik ile birleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="55c2b-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="55c2b-242">Belirli bir üye erişimine izin verileceğini, üyesi olduğu söylenir ***erişilebilir***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="55c2b-243">Belirli bir üyeye erişim izni, buna karşılık, üyesi olduğu söylenir ***erişilemez***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="55c2b-244">Metinsel konumun erişim yer alan erişilebilirlik etki alanında eklendiğinde bir üye erişim izin verilir ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) üyesi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="55c2b-245">Bildirilen erişilebilirliği</span><span class="sxs-lookup"><span data-stu-id="55c2b-245">Declared accessibility</span></span>

<span data-ttu-id="55c2b-246">***Erişilebilirlik bildirilen*** üyesi aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="55c2b-247">Dahil ederek seçtiğiniz ortak bir `public` değiştiricisi üye bildirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="55c2b-248">Sezgisel anlamını `public` "olmayan sınırlı erişim".</span><span class="sxs-lookup"><span data-stu-id="55c2b-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="55c2b-249">Korumalı, seçili dahil ederek bir `protected` değiştiricisi üye bildirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="55c2b-250">Sezgisel anlamını `protected` "içeren sınıfı veya türleri sınırlı erişim içeren sınıfından türetilir".</span><span class="sxs-lookup"><span data-stu-id="55c2b-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="55c2b-251">Dahil ederek seçilen dahili bir `internal` değiştiricisi üye bildirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="55c2b-252">Sezgisel anlamını `internal` "Bu program için sınırlı erişim".</span><span class="sxs-lookup"><span data-stu-id="55c2b-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="55c2b-253">İç (korumalı veya iç anlamına gelir), korumalı, seçili her ikisi de dahil ederek bir `protected` ve `internal` değiştiricisi üye bildirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="55c2b-254">Sezgisel anlamını `protected internal` "Bu program için sınırlı erişim veya içeren sınıfından türetilen türler".</span><span class="sxs-lookup"><span data-stu-id="55c2b-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="55c2b-255">Dahil ederek seçildiğinden özel, bir `private` değiştiricisi üye bildirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="55c2b-256">Sezgisel anlamını `private` "kapsadığı tür için sınırlı erişim".</span><span class="sxs-lookup"><span data-stu-id="55c2b-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="55c2b-257">Bağlama bağlı olarak bir üye bildirim aldığı yerleştirin, yalnızca belirli türlerdeki bildirilen erişilebilirliği izin verilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="55c2b-258">Ayrıca, üye bildirimi herhangi bir erişim değiştiricileri içermez, bildirimi yer aldığı içerik erişilebilirlik bildirilen varsayılan belirler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="55c2b-259">Örtük olarak ad sahip `public` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="55c2b-260">Ad alanı bildirimi üzerinde hiçbir erişim değiştiricisine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="55c2b-261">Türleri derleme biriminden veya ad alanı bildirimi olabilir `public` veya `internal` erişilebilirlik ve varsayılan olarak bildirilen `internal` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="55c2b-262">Sınıf üyeleri bildirilen erişilebilirliği beş tür hiçbirini ve varsayılan `private` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="55c2b-263">(Bir türü bir ad alanının bir üyesi yalnızca olabilir olarak bildirilmiş ancak bildirilen erişilebilirliği beş türde herhangi bir sınıfın üyesi olabildiği gibi bir türü bildirilen Not `public` veya `internal` erişilebilirlik bildirildi.)</span><span class="sxs-lookup"><span data-stu-id="55c2b-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="55c2b-264">Yapı üyeleri olabilir `public`, `internal`, veya `private` erişilebilirlik ve varsayılan olarak bildirilen `private` yapılardan türetme çünkü erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="55c2b-265">(Bu, bu yapı tarafından devralınan değil) yapı sürümünde Struct üyelerinin olamaz `protected` veya `protected internal` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="55c2b-266">(Yapı üyesi olabildiği gibi bir türü bildirilen unutmayın `public`, `internal`, veya `private` erişilebilirlik, türü bir ad alanının bir üyesi yalnızca olabilir olarak bildirilmiş ancak bildirilen `public` veya `internal` erişilebilirlik bildirildi.)</span><span class="sxs-lookup"><span data-stu-id="55c2b-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="55c2b-267">Arabirim üyeleri örtük olarak sahip `public` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="55c2b-268">Arabirim üye bildirimlerinde hiçbir erişim değiştiricisine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="55c2b-269">Numaralandırma üyelerini örtük olarak sahip `public` erişilebilirlik bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="55c2b-270">Hiçbir erişim değiştiricisine sabit listesi üye bildirimlerinde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="55c2b-271">Erişilebilirlik etki alanları</span><span class="sxs-lookup"><span data-stu-id="55c2b-271">Accessibility domains</span></span>

<span data-ttu-id="55c2b-272">***Erişilebilirlik etki alanı*** program metni üyesine erişim verilir (büyük olasılıkla ayrık) bölümlerini üyesi oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="55c2b-273">Erişilebilirlik etki alanı üyesi tanımlama amacıyla, üyesi olduğu söylenir ***en üst düzey*** bir tür içinde bildirilmedi ve üyesi olduğu söylenir ***iç içe geçmiş*** başka bir tür içinde bildirilirse.</span><span class="sxs-lookup"><span data-stu-id="55c2b-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="55c2b-274">Ayrıca, ***program metni*** bir programı tüm program tüm kaynak dosyalarında bulunan metin program ve tüm bulunan metin programı gibi bir tür öğesinin program metni tanımlanır tanımlanan *type_declaration*s (büyük olasılıkla, tür içinde iç içe geçmiş türler dahil) türü.</span><span class="sxs-lookup"><span data-stu-id="55c2b-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="55c2b-275">Önceden tanımlanmış bir tür erişilebilirlik etki alanı (gibi `object`, `int`, veya `double`) sınırsızdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="55c2b-276">Erişilebilirlik etki alanı bir üst düzey ilişkisiz türü `T` ([bağlı ve türleri ilişkisiz](types.md#bound-and-unbound-types)) bildirilen bir programda `P` şu şekilde tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="55c2b-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="55c2b-277">Varsa öğesinin bildirilen erişilebilirliği `T` olduğu `public`, Erişilebilirlik etki alanı `T` öğesinin program metnidir `P` ve başvuran herhangi bir programı `P`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="55c2b-278">Varsa öğesinin bildirilen erişilebilirliği `T` olduğu `internal`, Erişilebilirlik etki alanı `T` öğesinin program metnidir `P`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="55c2b-279">Üst düzey bir bağlanmamış tür erişilebilirlik etki alanı her zaman en az olduğunu izleyen bu tanımları yazın, program öğesinin program metni bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="55c2b-280">Erişilebilirlik etki alanı için oluşturulan tür `T<A1, ..., An>` ilişkisiz genel bir türün erişilebilirlik etki alanının kesişimidir `T` ve tür bağımsız değişkenlerini erişilebilirlik etki alanları `A1, ..., An`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="55c2b-281">Erişilebilirlik etki alanı iç içe üyenin `M` bir türde bildirilen `T` bir programın `P` şu şekilde tanımlanır (dikkate alınarak `M` kendisini büyük olasılıkla bir türü olabilir):</span><span class="sxs-lookup"><span data-stu-id="55c2b-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="55c2b-282">Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `public`, Erişilebilirlik etki alanı `M` erişilebilirlik etki alanıdır `T`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="55c2b-283">Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `protected internal`, let `D` öğesinin program metni birleşimi olabilir `P` ve türetilen her türlü program metni `T`, dışında bildirilen `P`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="55c2b-284">Erişilebilirlik etki alanı `M` erişilebilirlik etki alanının kesişimidir `T` ile `D`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="55c2b-285">Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `protected`, let `D` öğesinin program metni birleşimi olabilir `T` ve türetilen her türlü program metni `T`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="55c2b-286">Erişilebilirlik etki alanı `M` erişilebilirlik etki alanının kesişimidir `T` ile `D`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="55c2b-287">Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `internal`, Erişilebilirlik etki alanı `M` erişilebilirlik etki alanının kesişimidir `T` öğesinin program metni ile `P`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="55c2b-288">Varsa öğesinin bildirilen erişilebilirliği `M` olduğu `private`, Erişilebilirlik etki alanı `M` öğesinin program metnidir `T`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="55c2b-289">Bu tanımlarından iç içe üyenin erişilebilirlik etki alanı her zaman en az olduğunu izleyen program metni türündeki üye bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="55c2b-290">Ayrıca, Erişilebilirlik etki alanı üyesi hiç üyesi bildirildiği türün erişilebilirlik etki daha kapsamlı olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="55c2b-291">Sezgisel koşullarında bir tür veya üye `M` olan erişim, aşağıdaki adımları erişim izin verildiğinden emin olmak için değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="55c2b-292">İlk olarak, eğer `M` (olarak karşılıklı derleme biriminde veya bir ad alanı), bir tür içindeki bir derleme zamanı hatası oluşur türü erişilebilir durumda değilse bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="55c2b-293">Ardından, eğer `M` olduğu `public`, erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="55c2b-294">Aksi takdirde `M` olduğu `protected internal`, programda içinde ortaya çıkarsa erişimine izin verilen `M` bildirilen veya bir sınıf içinde oluşursa türetilen sınıfta `M` bildirilir ve türetilmiş üzerinden gerçekleşir sınıf türü ([korumalı üyeleri örneği için erişim](basic-concepts.md#protected-access-for-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="55c2b-295">Aksi takdirde `M` olduğu `protected`, sınıfta içinde ortaya çıkarsa erişimine izin verilen `M` bildirilen veya bir sınıf içinde oluşursa türetilen sınıfta `M` bildirilir ve türetilmiş üzerinden gerçekleşir sınıf türü ([korumalı üyeleri örneği için erişim](basic-concepts.md#protected-access-for-instance-members)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="55c2b-296">Aksi takdirde `M` olduğu `internal`, programda içinde ortaya çıkarsa erişimine izin verilen `M` bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="55c2b-297">Aksi takdirde `M` olduğu `private`, hangi tür içinde ortaya çıkarsa erişimine izin verilen `M` bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="55c2b-298">Aksi takdirde, türe veya üyeye erişilemiyor ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="55c2b-299">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-299">In the example</span></span>
```csharp
public class A
{
    public static int X;
    internal static int Y;
    private static int Z;
}

internal class B
{
    public static int X;
    internal static int Y;
    private static int Z;

    public class C
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }

    private class D
    {
        public static int X;
        internal static int Y;
        private static int Z;
    }
}
```
<span data-ttu-id="55c2b-300">Aşağıdaki erişilebilirlik etki alanları, sınıflar ve üyeler vardır:</span><span class="sxs-lookup"><span data-stu-id="55c2b-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="55c2b-301">Erişilebilirlik etki alanı `A` ve `A.X` sınırsızdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="55c2b-302">Erişilebilirlik etki alanı `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, ve `B.C.Y` içeren program öğesinin program metnidir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="55c2b-303">Erişilebilirlik etki alanı `A.Z` öğesinin program metnidir `A`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="55c2b-304">Erişilebilirlik etki alanı `B.Z` ve `B.D` öğesinin program metnidir `B`, öğesinin program metni dahil olmak üzere `B.C` ve `B.D`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="55c2b-305">Erişilebilirlik etki alanı `B.C.Z` öğesinin program metnidir `B.C`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="55c2b-306">Erişilebilirlik etki alanı `B.D.X` ve `B.D.Y` öğesinin program metnidir `B`, öğesinin program metni dahil olmak üzere `B.C` ve `B.D`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="55c2b-307">Erişilebilirlik etki alanı `B.D.Z` öğesinin program metnidir `B.D`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="55c2b-308">Erişilebilirlik etki alanı üyesi hiçbir zaman örnekte gösterildiği gibi bir kapsayan tür daha büyük.</span><span class="sxs-lookup"><span data-stu-id="55c2b-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="55c2b-309">Örneğin, olsa bile tüm `X` üyelere sahip genel bildirilen erişilebilirliği dışındaki tüm `A.X` içeren bir tür tarafından kısıtlanmış erişilebilirlik etki alanınız.</span><span class="sxs-lookup"><span data-stu-id="55c2b-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="55c2b-310">Bölümünde anlatıldığı gibi [üyeleri](basic-concepts.md#members), örneğin Oluşturucular, Yıkıcılar ve statik oluşturucular dışında bir taban sınıfın tüm üyeleri türetilen türler tarafından devralınır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="55c2b-311">Bu, bir taban sınıfın bile özel üyeler içerir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-311">This includes even private members of a base class.</span></span> <span data-ttu-id="55c2b-312">Ancak, özel üye erişilebilirlik etki alanı üyesi bildirildiği türü yalnızca program metni içerir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="55c2b-313">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-313">In the example</span></span>
```csharp
class A
{
    int x;

    static void F(B b) {
        b.x = 1;        // Ok
    }
}

class B: A
{
    static void F(B b) {
        b.x = 1;        // Error, x not accessible
    }
}
```
<span data-ttu-id="55c2b-314">`B` sınıfından devralan bir özel üye `x` gelen `A` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="55c2b-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="55c2b-315">Özel üye olduğundan, yalnızca içinde erişilebilir *class_body* , `A`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="55c2b-316">Bu nedenle, erişim izni `b.x` içinde başarılı `A.F` yöntemi, ancak başarısız olursa `B.F` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="55c2b-317">Örnek üyeleri için Korumalı Erişim</span><span class="sxs-lookup"><span data-stu-id="55c2b-317">Protected access for instance members</span></span>

<span data-ttu-id="55c2b-318">Olduğunda bir `protected` örnek üyesi içinde bildirildiğinde, sınıf öğesinin program metni dışında erişilebilir ve ne zaman bir `protected internal` erişim içinde gerçekleşmesi gerekir, örnek üyesi, bildirilen program öğesinin program metni dışında erişilebilir bir sınıf bildirimi içinde bildirildiği sınıfından türetilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="55c2b-319">Ayrıca, erişim, türetilmiş sınıf türü ya da ondan oluşturulan sınıf türünde bir örnek üzerinden gerçekleşmesi için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="55c2b-320">Bu kısıtlama, bir türetilmiş sınıf bile aynı temel sınıftan devralınan üyeleri, türetilen diğer sınıflar korumalı üyelerine erişmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="55c2b-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="55c2b-321">İzin `B` bir korumalı örnek üye bildirir bir taban sınıfı olarak `M`ve `D` türetildiği bir sınıf olması `B`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="55c2b-322">İçinde *class_body* , `D`, erişim `M` aşağıdaki biçimlerden birini alabilir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="55c2b-323">Nitelenmemiş bir *type_name* veya *primary_expression* formun `M`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="55c2b-324">A *primary_expression* formun `E.M`, türü sağlanan `E` olduğu `T` veya türetilmiş bir sınıf `T`burada `T` sınıf türüdür `D`, veya bir sınıf türü oluşturulan `D`</span><span class="sxs-lookup"><span data-stu-id="55c2b-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="55c2b-325">A *primary_expression* formun `base.M`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="55c2b-326">Bu erişim forms ek olarak, bir korumalı örnek oluşturucusu içinde temel sınıfın türetilmiş bir sınıf erişebilirsiniz bir *constructor_initializer* ([Oluşturucu başlatıcıları](classes.md#constructor-initializers)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="55c2b-327">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-327">In the example</span></span>
```csharp
public class A
{
    protected int x;

    static void F(A a, B b) {
        a.x = 1;        // Ok
        b.x = 1;        // Ok
    }
}

public class B: A
{
    static void F(A a, B b) {
        a.x = 1;        // Error, must access through instance of B
        b.x = 1;        // Ok
    }
}
```
<span data-ttu-id="55c2b-328">içinde `A`, erişim mümkündür `x` örnekleri her ikisi de aracılığıyla `A` ve `B`, her iki durumda da yer örneği üzerinden erişim gerektirdiğinden `A` veya türetilmiş bir sınıf `A`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="55c2b-329">Bununla birlikte, içinde `B`, erişmek mümkün değildir `x` örneği üzerinden `A`, bu yana `A` türünden türemez `B`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="55c2b-330">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-330">In the example</span></span>
```csharp
class C<T>
{
    protected T x;
}

class D<T>: C<T>
{
    static void F() {
        D<T> dt = new D<T>();
        D<int> di = new D<int>();
        D<string> ds = new D<string>();
        dt.x = default(T);
        di.x = 123;
        ds.x = "test";
    }
}
```
<span data-ttu-id="55c2b-331">üç atamaları `x` genel türünden oluşturulduğu sınıf türleri örnekleri aracılığıyla tüm gerçekleşmesi için izin verilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="55c2b-332">Erişilebilirlik kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="55c2b-332">Accessibility constraints</span></span>

<span data-ttu-id="55c2b-333">C# dilinde çeşitli yapıları türünün olmasını gerektiren ***en az olarak erişilebilir olarak*** üyesi veya başka bir tür.</span><span class="sxs-lookup"><span data-stu-id="55c2b-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="55c2b-334">Bir tür `T` üyenin veya türün en az olarak erişilebilir olduğu söylenir `M` , Erişilebilirlik etki alanı `T` erişilebilirlik etki alanının bir üst kümesidir `M`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="55c2b-335">Diğer bir deyişle, `T` en az olarak erişilebilir olarak `M` varsa `T` tüm bağlamlarda erişilebilir `M` erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="55c2b-336">Aşağıdaki erişilebilirlik kısıtlamaları mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="55c2b-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="55c2b-337">Bir sınıf türünün doğrudan temel sınıf en az sınıf türü kendisini olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="55c2b-338">Açık bir arabirim türü temel arabirimleri en az arabirim türü kendisini olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="55c2b-339">Bir temsilci türü parametre türleri ve dönüş türü, en az temsilci türü kendisini olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="55c2b-340">Bir sabit değer türü en az sabit olarak olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="55c2b-341">Bir alan türü en az alan olarak olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="55c2b-342">Bir yöntemin parametre türleri ve dönüş türü, en az yöntemi olarak olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="55c2b-343">Bir özelliğin türünü en az özelliği olarak olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="55c2b-344">Bir olay türü en az olay olarak olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="55c2b-345">Bir dizin oluşturucu türü ve parametre türleri, en az Indexer olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="55c2b-346">Operatör için parametre türleri ve dönüş türü, en azından operatör olarak olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="55c2b-347">Bir örnek oluşturucusunda parametre türleri, en az örnek oluşturucusu kendisini olarak erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="55c2b-348">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="55c2b-349">`B` sınıfı sonuçları bir derleme zamanı hatası nedeniyle `A` en az olarak erişilebilir olarak değil `B`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="55c2b-350">Benzer şekilde, örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="55c2b-351">`H` yönteminde `B` çünkü dönüş türü bir derleme zamanı hatası oluşur `A` en az yöntemi olarak olarak erişilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="55c2b-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="55c2b-352">İmzalar ve aşırı yükleme</span><span class="sxs-lookup"><span data-stu-id="55c2b-352">Signatures and overloading</span></span>

<span data-ttu-id="55c2b-353">Yöntemleri, örnek oluşturucuları, dizin oluşturucular ve işleçleri göre nitelenen kendi ***imzaları***:</span><span class="sxs-lookup"><span data-stu-id="55c2b-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="55c2b-354">Yöntemi, tür parametreleri sayısı ve türü ve tür (değer, başvuru veya çıkış) her birini sırayla soldan sağa kabul biçimsel parametrelerinin adı, yöntemin imzası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="55c2b-355">Bu amaçlar için oluşan bir biçimsel parametre türü yöntemin herhangi bir tür parametre adına göre değil, ancak türü bağımsız değişken listesindeki sıralı konumuna yöntem tarafından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="55c2b-356">Yöntemin imzası dönüş türü, özellikle içermez `params` en sağdaki parametresi ya da isteğe bağlı tür parametresi kısıtlamaları için belirtilen değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="55c2b-357">Tür ve tür (değer, başvuru veya çıkış) sırayla soldan sağa kabul kendi biçimsel parametrelerinin her biri bir örnek oluşturucusu imzası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="55c2b-358">Bir örnek oluşturucusunda imzası özellikle içermemesi `params` en sağdaki parametresi için belirtilen değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="55c2b-359">Bir dizin oluşturucu imzasının her birini sırayla soldan sağa kabul biçimsel parametrelerinin türünü oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="55c2b-360">Bir dizin oluşturucu imzası özel öğe türü içermiyor ya da içeriyor mu `params` en sağdaki parametresi için belirtilen değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="55c2b-361">Bir işleç imzası adını işleci ve her birini sırayla soldan sağa kabul biçimsel parametrelerinin türünü oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="55c2b-362">Bir işleç imzası, özellikle sonuç türü içermez.</span><span class="sxs-lookup"><span data-stu-id="55c2b-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="55c2b-363">İmzalar, etkinleştirme mekanizması ***aşırı yükleme*** sınıflar, yapılar ve arabirimler üyelerin:</span><span class="sxs-lookup"><span data-stu-id="55c2b-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="55c2b-364">Yöntemlerinin aşırı yükleme, bir sınıf, yapı veya arabirim bunların imzalarını sağlanan aynı ada sahip birden çok yöntem, sınıf, yapı veya arabirim içinde benzersiz bildirmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="55c2b-365">İmzaları, sınıf veya yapı içinde benzersiz olması koşuluyla örnek oluşturucuları aşırı yüklemesi bir sınıfın veya yapının birden çok örnek oluşturucuları bildirmek için verir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="55c2b-366">Bunların imzalarını Bu sınıf, yapı veya arabirim içinde benzersiz olması koşuluyla dizin oluşturucu aşırı yüklemesi bir sınıf, yapı veya arabirim birden çok dizin oluşturucu bildirmek için verir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="55c2b-367">Operatörleri aşırı yükleme, bir sınıf veya yapı bunların imzalarını sağlanan aynı ada sahip birden çok işleç, sınıf veya yapı içinde benzersiz bildirmek için izin verir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="55c2b-368">Ancak `out` ve `ref` parametre değiştiriciler imzasının bir parçası olarak kabul edilir, tek bir türde bildirilen üyeler olamaz farklı imzasında sadece onun tarafından `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="55c2b-369">İki üye aynı ise olacaktır imzalarla aynı türdeki her iki yöntemde de tüm parametrelerinde bildirilir, bir derleme zamanı hatası meydana gelir `out` değiştiriciler değiştirildi `ref` değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="55c2b-370">İmza eşleşen diğer amaçlar için (örneğin, gizleme veya geçersiz kılma), `ref` ve `out` imzasının bir parçası olarak kabul edilir ve birbirlerine eşleşmiyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="55c2b-371">(Bu kısıtlama, ortak dil altyapısı (yalnızca içinde farklı yöntemleri tanımlamak için bir yol sağlamaz, CLI üzerinde), çalıştırılacak kolayca çevrilmesi C# programları izin vermektir `ref` ve `out`.)</span><span class="sxs-lookup"><span data-stu-id="55c2b-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="55c2b-372">İmzaların türleri amacıyla `object` ve `dynamic` aynı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="55c2b-373">Tek bir türde bildirilen üyeler bu nedenle farklı imzasında sadece onun tarafından `object` ve `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="55c2b-374">Aşağıdaki örnek, aşırı yüklenmiş yöntem bildirimleri bunların imzalarını birlikte bir dizi gösterir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
```csharp
interface ITest
{
    void F();                        // F()

    void F(int x);                   // F(int)

    void F(ref int x);               // F(ref int)

    void F(out int x);               // F(out int)      error

    void F(int x, int y);            // F(int, int)

    int F(string s);                 // F(string)

    int F(int x);                    // F(int)          error

    void F(string[] a);              // F(string[])

    void F(params string[] a);       // F(string[])     error
}
```

<span data-ttu-id="55c2b-375">Herhangi bir Not `ref` ve `out` parametre değiştiriciler ([yöntem parametreleri](classes.md#method-parameters)) bir imza bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="55c2b-376">Bu nedenle, `F(int)` ve `F(ref int)` benzersiz imzalar.</span><span class="sxs-lookup"><span data-stu-id="55c2b-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="55c2b-377">Ancak, `F(ref int)` ve `F(out int)` sadece onun tarafından imzalarının farklı olduğundan, aynı arabirimi içinde bildirilemez `ref` ve `out`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="55c2b-378">Ayrıca, dönüş türü Not ve `params` değiştiricisi parçası değildir, imza dönüş türü veya ekleme veya çıkarma, yalnızca temel aşırı mümkün olmadığından `params` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="55c2b-379">Şekilde yöntemleri bildirimlerini `F(int)` ve `F(params string[])` bir derleme zamanı hatası sonucunda yukarıda tanımlanan.</span><span class="sxs-lookup"><span data-stu-id="55c2b-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="55c2b-380">Kapsamları</span><span class="sxs-lookup"><span data-stu-id="55c2b-380">Scopes</span></span>

<span data-ttu-id="55c2b-381">***Kapsam*** bir bölge içinde mümkündür nitelik adı olmadan yalnızca adla bildirilen bir varlığa başvurmak program metni adıdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="55c2b-382">Kapsamları olabilir ***iç içe geçmiş***, ve bir iç kapsamı anlamı, bir dış kapsam adı yeniden bildirmek (Bu ancak tarafından uygulanan kısıtlama kaldırmaz, [bildirimleri](basic-concepts.md#declarations) iç içe geçmiş bir bloğu içinde olmadığını olası) kapsayan bir blok içinde yerel bir değişken olarak aynı ada sahip yerel bir değişken bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="55c2b-383">Dış kapsamdaki adından sonra denen moddadır ***gizli*** programının bölgede metin iç kapsamı tarafından ele ve dış adına erişim, yalnızca olası Adın niteleme tarafından.</span><span class="sxs-lookup"><span data-stu-id="55c2b-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="55c2b-384">Ad alanı üyesi kapsamı tarafından bildirilen bir *namespace_member_declaration* ([Namespace üyelerini](namespaces.md#namespace-members)) ile hiçbir kapsayan *namespace_declaration* tüm program metin.</span><span class="sxs-lookup"><span data-stu-id="55c2b-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="55c2b-385">Ad alanı üyesi kapsamı tarafından bildirilen bir *namespace_member_declaration* içinde bir *namespace_declaration* tam adı olan `N` olduğu *namespace_body*  , her *namespace_declaration* tam adı olan `N` veya ile başlayan `N`ve ardından bir nokta.</span><span class="sxs-lookup"><span data-stu-id="55c2b-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="55c2b-386">Tarafından tanımlanan ad kapsamı bir *extern_alias_directive* üzerinden genişletir *using_directive*s, *global_attributes* ve *namespace_member_ bildirimi*hemen içeren derleme birimi veya ad alanı gövdesinde, s.</span><span class="sxs-lookup"><span data-stu-id="55c2b-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="55c2b-387">Bir *extern_alias_directive* tüm yeni üyeleri temel alınan bildirim alanına gereksinimdir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="55c2b-388">Diğer bir deyişle, bir *extern_alias_directive* geçişli değildir ancak bunun yerine oluştuğu yalnızca derleme birimi veya ad alanı gövdesi etkiler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="55c2b-389">Kapsam adı tanımlı ya da içeri aktaran bir *using_directive* ([yönergeleri kullanarak](namespaces.md#using-directives)) üzerinden genişletir *namespace_member_declaration*sn  *compilation_unit* veya *namespace_body* hangi *using_directive* gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="55c2b-390">A *using_directive* sıfır veya daha fazla ad alanı, tür veya üye adları belirli bir içinde kullanılabilir hale *compilation_unit* veya *namespace_body*, ancak yok tüm yeni üyeleri temel alınan bildirim alanına katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="55c2b-391">Diğer bir deyişle, bir *using_directive* geçişli değildir ancak bunun yerine yalnızca etkiler *compilation_unit* veya *namespace_body* içinde hangi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="55c2b-392">Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *class_declaration* ([sınıf bildirimleri](classes.md#class-declarations)) olan *class_base*, *type_parameter_constraints_clause*s ve *class_body* , söz konusu *class_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="55c2b-393">Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *struct_declaration* ([yapı bildirimleri](structs.md#struct-declarations)) olan *struct_interfaces* , *type_parameter_constraints_clause*s ve *struct_body* , söz konusu *struct_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="55c2b-394">Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *interface_declaration* ([arabirim bildirimleri](interfaces.md#interface-declarations)) olan *interface_base* , *type_parameter_constraints_clause*s ve *interface_body* , söz konusu *interface_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="55c2b-395">Bir tür parametresi kapsamı tarafından bildirilen bir *type_parameter_list* üzerinde bir *delegate_declaration* ([temsilci bildirimi](delegates.md#delegate-declarations)) olan *Döndür_tür*, *formal_parameter_list*, ve *type_parameter_constraints_clause*s, söz konusu *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="55c2b-396">Tarafından bildirilen bir üye kapsamını bir *class_member_declaration* ([sınıfı gövdesi](classes.md#class-body)) olan *class_body* bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="55c2b-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="55c2b-397">Ayrıca, için bir sınıf üyesinin kapsamını genişletir *class_body* içeriğiyle erişilebilirlik etki alanında bulunan sınıflar türetilmiş ([erişilebilirlik etki alanı](basic-concepts.md#accessibility-domains)) üyesinin.</span><span class="sxs-lookup"><span data-stu-id="55c2b-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="55c2b-398">Tarafından bildirilen bir üye kapsamını bir *struct_member_declaration* ([Yapı üyeleri](structs.md#struct-members)) olan *struct_body* bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="55c2b-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="55c2b-399">Tarafından bildirilen bir üye kapsamını bir *enum_member_declaration* ([numaralandırma üyeleri](enums.md#enum-members)) olan *enum_body* bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="55c2b-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="55c2b-400">Bir parametre kapsamı içinde bildirilen bir *method_declaration* ([yöntemleri](classes.md#methods)) olan *method_body* , söz konusu *method_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="55c2b-401">Bir parametre kapsamı içinde bildirilen bir *indexer_declaration* ([dizin oluşturucular](classes.md#indexers)) olan *accessor_declarations* , söz konusu *indexer_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="55c2b-402">Bir parametre kapsamı içinde bildirilen bir *operator_declaration* ([işleçleri](classes.md#operators)) olan *blok* , söz konusu *operator_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="55c2b-403">Bir parametre kapsamı içinde bildirilen bir *constructor_declaration* ([örnek oluşturucular](classes.md#instance-constructors)) olan *constructor_initializer* ve *blok* , söz konusu *constructor_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="55c2b-404">Bir parametre kapsamı içinde bildirilen bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) olan *anonymous_function_body* , söz konusu *lambda_ ifade*</span><span class="sxs-lookup"><span data-stu-id="55c2b-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="55c2b-405">Bir parametre kapsamı içinde bildirilen bir *anonymous_method_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) olan *blok* , söz konusu *anonymous_method _expression*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="55c2b-406">Etiket kapsamı içinde bildirilen bir *labeled_statement* ([etiketli deyimler](statements.md#labeled-statements)) olan *blok* bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="55c2b-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="55c2b-407">Bildirilen yerel değişken kapsamını bir *local_variable_declaration* ([yerel değişken bildirimlerini](statements.md#local-variable-declarations)) blok bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="55c2b-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="55c2b-408">Bildirilen yerel değişken kapsamını bir *switch_block* , bir `switch` deyimi ([switch deyimi](statements.md#the-switch-statement)) olan *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="55c2b-409">Bildirilen yerel değişken kapsamını bir *for_initializer* , bir `for` deyimi ([deyimi için](statements.md#the-for-statement)) olan *for_initializer*,  *for_condition*, *for_iterator*ve kapsanan *deyimi* , `for` deyimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="55c2b-410">Bir yerel sabit kapsamı içinde bildirilen bir *local_constant_declaration* ([yerel sabit bildirimleri](statements.md#local-constant-declarations)) blok bildirimi oluştuğu içinde.</span><span class="sxs-lookup"><span data-stu-id="55c2b-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="55c2b-411">Önündeki değerinin metinsel bir konumda yerel bir sabit belirtmek için bir derleme zamanı hata kendi *constant_declarator*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="55c2b-412">Bir değişkenin kapsamını bir parçası olarak bildirilen bir *foreach_statement*, *using_statement*, *lock_statement* veya *query_expression* olduğu verilen yapısının genişletme tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="55c2b-413">Ad alanı, sınıf, yapı veya sabit listesi üyesi kapsamında üyesi bildirimi önündeki bir metinsel konumu üye başvurmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="55c2b-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="55c2b-414">Örneğin</span><span class="sxs-lookup"><span data-stu-id="55c2b-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="55c2b-415">Burada, geçerli `F` başvurmak için `i` bildirilmeden önce.</span><span class="sxs-lookup"><span data-stu-id="55c2b-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="55c2b-416">İsteğe bağlı olarak yerel bir değişken kapsamında önündeki değerinin metinsel bir konumda yerel değişkene başvurmak için bir derleme zamanı hatası olmayan *local_variable_declarator* yerel değişkenin.</span><span class="sxs-lookup"><span data-stu-id="55c2b-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="55c2b-417">Örneğin</span><span class="sxs-lookup"><span data-stu-id="55c2b-417">For example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        i = 1;                  // Error, use precedes declaration
        int i;
        i = 2;
    }

    void G() {
        int j = (j = 1);        // Valid
    }

    void H() {
        int a = 1, b = ++a;    // Valid
    }
}
```

<span data-ttu-id="55c2b-418">İçinde `F` yukarıdaki yöntemi, ilk atamaya `i` dış kapsamda bildirilen alan özellikle başvurmuyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="55c2b-419">Bunun yerine yerel bir değişkene başvuruyor ve metin içeriğini eklemek değişkenin bildirimi kendisinden nedeniyle, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="55c2b-420">İçinde `G` yöntemi, kullanımını `j` başlatıcıda bildirimi için `j` geçersiz kullanımı önünde değil çünkü *local_variable_declarator*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="55c2b-421">İçinde `H` yöntemi, bir sonraki *local_variable_declarator* daha önceki bir bildirilen yerel değişken doğru başvurduğu *local_variable_declarator* aynı  *local_variable_declaration*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="55c2b-422">Yerel değişkenler için içeriğin kapsamını belirleyen kuralları anlamı bir ifade bağlamda kullanılan bir ad, her zaman bir blok içindeki aynı olacağını garanti etmek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="55c2b-423">Yerel bir değişkenin kapsamını, yalnızca kendi bildirimden bloğun sonuna kadar genişletmek için olsaydı, ardından yukarıdaki örnekte, örnek değişkeni ilk atama atadığınız ve ikinci atama, büyük olasılıkla için önde gelen bir yerel değişkene atadığınız Blok ifadeleri yeniden düzenlenecek için daha sonra, derleme zamanı hataları.</span><span class="sxs-lookup"><span data-stu-id="55c2b-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="55c2b-424">Bir blok içinde bir ad anlamını adı kullanıldığı bağlamı göre farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="55c2b-425">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-425">In the example</span></span>
```csharp
using System;

class A {}

class Test
{
    static void Main() {
        string A = "hello, world";
        string s = A;                            // expression context

        Type t = typeof(A);                      // type context

        Console.WriteLine(s);                    // writes "hello, world"
        Console.WriteLine(t);                    // writes "A"
    }
}
```
<span data-ttu-id="55c2b-426">adı `A` yerel bir değişkene başvurmak için kullanılan bir ifade bağlamda `A` ve sınıfa başvurmak için bir tür bağlamında `A`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="55c2b-427">Ad gizleme</span><span class="sxs-lookup"><span data-stu-id="55c2b-427">Name hiding</span></span>

<span data-ttu-id="55c2b-428">Kapsam, bir varlığın varlık bildirimi alandan daha fazla program metni genellikle kapsar.</span><span class="sxs-lookup"><span data-stu-id="55c2b-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="55c2b-429">Özellikle, bir varlığın kapsamı aynı ada sahip bir varlık içeren yeni bir bildirim alanları tanıtan bildirimler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="55c2b-430">Bu tür bildirimleri neden olacak orijinal varlık ***gizli***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="55c2b-431">Buna karşılık, bir varlık olarak kabul edilir ***görünür*** değil gizlenen.</span><span class="sxs-lookup"><span data-stu-id="55c2b-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="55c2b-432">İç içe geçirme ve ne zaman devralma yoluyla kapsamları çakışan aracılığıyla kapsamları çakışan ad gizleme gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="55c2b-433">Gizleme iki tür özellikleri aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="55c2b-434">İç içe geçme aracılığıyla gizleme</span><span class="sxs-lookup"><span data-stu-id="55c2b-434">Hiding through nesting</span></span>

<span data-ttu-id="55c2b-435">Ad gizleme aracılığıyla iç içe geçme, iç içe geçmiş ad alanlarından veya türlerinden içinde iç içe türleri sınıflar veya yapılar ve parametre ve yerel değişken bildirimleri sonucunda sonucu olarak ad alanları, sonucu olarak ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="55c2b-436">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-436">In the example</span></span>
```csharp
class A
{
    int i = 0;

    void F() {
        int i = 1;
    }

    void G() {
        i = 1;
    }
}
```
<span data-ttu-id="55c2b-437">içinde `F` yöntemi, bir örnek değişkeni `i` yerel değişkeni tarafından gizleniyor `i`, ancak içinde `G` yöntemi `i` hala örneği değişkenine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="55c2b-438">Bir iç kapsamda bir ada bir adı bir dış kapsamdaki gizler, bu adı aşırı yüklenmiş tüm oluşumlarını gizler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="55c2b-439">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-439">In the example</span></span>
```csharp
class Outer
{
    static void F(int i) {}

    static void F(string s) {}

    class Inner
    {
        void G() {
            F(1);              // Invokes Outer.Inner.F
            F("Hello");        // Error
        }

        static void F(long l) {}
    }
}
```
<span data-ttu-id="55c2b-440">çağrı `F(1)` çağırır `F` bildirilen `Inner` çünkü dış tüm oluşumlarını `F` iç bildirim tarafından gizlenmiş.</span><span class="sxs-lookup"><span data-stu-id="55c2b-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="55c2b-441">Aynı nedenle, arama `F("Hello")` bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="55c2b-442">Devralma yoluyla gizleme</span><span class="sxs-lookup"><span data-stu-id="55c2b-442">Hiding through inheritance</span></span>

<span data-ttu-id="55c2b-443">Devralma üzerinden ad gizleme sınıflarını veya yapılarını temel sınıftan devralınan adı yeniden bildirmek oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="55c2b-444">Bu tür bir ad gizleme aşağıdaki biçimlerden birini alır:</span><span class="sxs-lookup"><span data-stu-id="55c2b-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="55c2b-445">Bir sabit, alan, özelliği, olay veya bir sınıf ya da yapı birimine türü aynı ada sahip tüm taban sınıfı üyelerini gizler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="55c2b-446">Bir sınıf ya da struct'a dahil edilen bir yöntem, yöntem olmayan taban sınıf üyelerinin tamamı aynı ada sahip ve (yöntem adı ve parametre sayısı, değiştiriciler ve türleri) aynı imzaya sahip tüm taban sınıfı yöntemlerini gizler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="55c2b-447">Bir sınıf veya yapı içinde tanıtılan bir dizin oluşturucu (parametre sayısı ve türleri) aynı imzaya sahip tüm taban sınıfı dizin oluşturucularını gizler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="55c2b-448">İşleç bildirimi kuralları ([işleçleri](classes.md#operators)) temel sınıfta operatör olarak aynı imzaya sahip bir işleç bildirmek üzere bir türetilmiş sınıfta imkansız hale.</span><span class="sxs-lookup"><span data-stu-id="55c2b-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="55c2b-449">Bu nedenle, işleçleri asla birbirleriyle gizleyin.</span><span class="sxs-lookup"><span data-stu-id="55c2b-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="55c2b-450">Bir dış kapsam adı gizleme aykırı devralınan bir kapsamdan erişilebilir bir adı gizleyerek bildirilmesini uyarı neden olur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="55c2b-451">Örnekte</span><span class="sxs-lookup"><span data-stu-id="55c2b-451">In the example</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    public void F() {}        // Warning, hiding an inherited name
}
```
<span data-ttu-id="55c2b-452">bildirimi `F` içinde `Derived` bildirilmesini bir uyarısına neden olur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="55c2b-453">Temel sınıflar ayrı evrimi engelleyen, devralınan bir adı gizleyerek bir hata değildir özellikle.</span><span class="sxs-lookup"><span data-stu-id="55c2b-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="55c2b-454">Örneğin, yukarıdaki durumda çünkü gelmiş olabilir daha sonraki bir sürümünü `Base` sunulan bir `F` sınıfı önceki bir sürümde bulunmamıştır yöntemi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="55c2b-455">Yukarıdaki durumu hata olsaydı, sonra ayrı ayrı tutulan Sınıf Kitaplığı'nda bir temel sınıf için yapılan herhangi bir değişiklik potansiyel olarak türetilmiş sınıfları geçersiz olmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="55c2b-456">Devralınan bir adı gizleyerek neden uyarı aracılığıyla kaldırılabilir `new` değiştiricisi:</span><span class="sxs-lookup"><span data-stu-id="55c2b-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
```csharp
class Base
{
    public void F() {}
}

class Derived: Base
{
    new public void F() {}
}
```

<span data-ttu-id="55c2b-457">`new` Değiştiricisi gösterir `F` içinde `Derived` "Yeni" ve bu gerçekten de devralınan bir üyeyi gizlemek üzere tasarlanmış olduğunu.</span><span class="sxs-lookup"><span data-stu-id="55c2b-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="55c2b-458">Yeni üye bildirimi yalnızca yeni üye kapsamında devralınmış bir üyeyi gizler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

```csharp
class Base
{
    public static void F() {}
}

class Derived: Base
{
    new private static void F() {}    // Hides Base.F in Derived only
}

class MoreDerived: Derived
{
    static void G() { F(); }          // Invokes Base.F
}
```

<span data-ttu-id="55c2b-459">Bildirimi yukarıdaki örnekte `F` içinde `Derived` gizler `F` , öğesinden devralınan `Base`, ancak bu yana yeni `F` içinde `Derived` özel erişimi olduğunu kapsamı kapsamaz `MoreDerived` .</span><span class="sxs-lookup"><span data-stu-id="55c2b-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="55c2b-460">Bu nedenle, arama `F()` içinde `MoreDerived.G` geçerli olduğundan ve çağıracağı `Base.F`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="55c2b-461">Namespace ve tür adları</span><span class="sxs-lookup"><span data-stu-id="55c2b-461">Namespace and type names</span></span>

<span data-ttu-id="55c2b-462">Bir C# programı çeşitli bağlamlarda gerektiren bir *namespace_name* veya *type_name* belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

```antlr
namespace_name
    : namespace_or_type_name
    ;

type_name
    : namespace_or_type_name
    ;

namespace_or_type_name
    : identifier type_argument_list?
    | namespace_or_type_name '.' identifier type_argument_list?
    | qualified_alias_member
    ;
```

<span data-ttu-id="55c2b-463">A *namespace_name* olduğu bir *namespace_or_type_name* ad alanına başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="55c2b-464">Çözüm, aşağıda açıklandığı gibi aşağıdaki *namespace_or_type_name* , bir *namespace_name* bir ad alanına başvurmalıdır veya aksi halde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="55c2b-465">Hiçbir tür bağımsız değişkenleri ([tür bağımsız değişkenleri](types.md#type-arguments)) içinde mevcut olabilen bir *namespace_name* (türler tür bağımsız değişkenleri olabilir yalnızca).</span><span class="sxs-lookup"><span data-stu-id="55c2b-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="55c2b-466">A *type_name* olduğu bir *namespace_or_type_name* bir türe başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="55c2b-467">Çözüm, aşağıda açıklandığı gibi aşağıdaki *namespace_or_type_name* , bir *type_name* bir türe başvurmalıdır veya aksi halde bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="55c2b-468">Varsa *namespace_or_type_name* tam-diğer ad-anlamını olan açıklandığı gibi üye [Namespace diğer adı niteleyicileri](namespaces.md#namespace-alias-qualifiers).</span><span class="sxs-lookup"><span data-stu-id="55c2b-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="55c2b-469">Aksi takdirde, bir *namespace_or_type_name* dört forms birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="55c2b-470">Burada `I` tek bir tanımlayıcı `N` olduğu bir *namespace_or_type_name* ve `<A1, ..., Ak>` isteğe bağlıdır *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="55c2b-471">Hiçbir *type_argument_list* olduğundan belirtilen göz önünde bulundurun `k` sıfır olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="55c2b-472">Anlamı bir *namespace_or_type_name* şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="55c2b-473">Varsa *namespace_or_type_name* biçimindedir `I` veya formun `I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="55c2b-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="55c2b-474">Varsa `K` sıfırdır ve *namespace_or_type_name* genel yöntem bildiriminde içinde görünür ([yöntemleri](classes.md#methods)) ve bu bildirimi bir tür parametresi içeriyorsa ([türü parametreleri](classes.md#type-parameters)) adıyla `I`, ardından *namespace_or_type_name* bu tür parametresine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="55c2b-475">Aksi takdirde *namespace_or_type_name* türü bildiriminden sonra her örnek türü için görüntülenen `T` ([örnek türü](classes.md#the-instance-type)) itibaren bu türdeki örnek türü bildirim ve (varsa) her kapsayan bir class veya struct bildiriminde örneği türü ile devam etmeden:</span><span class="sxs-lookup"><span data-stu-id="55c2b-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="55c2b-476">Varsa `K` sıfır ve bildirimi `T` ada sahip bir tür parametresi içeren `I`, ardından *namespace_or_type_name* bu tür parametresine başvuran.</span><span class="sxs-lookup"><span data-stu-id="55c2b-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="55c2b-477">Aksi takdirde *namespace_or_type_name* türü bildirimi gövdesi içinde görünür ve `T` veya temel türlerinden birinin adına sahip iç içe erişilebilir türü içeren `I` ve `K` tür parametrelerindeki , ardından *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="55c2b-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="55c2b-478">Birden fazla tür ise daha türetilmiş türde bildirilen türü seçilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="55c2b-479">Tür olmayan üyeler (sabitleri, alanları, yöntemleri, özellikleri, Dizinleyicileri, işleçleri, örnek oluşturucuları, yok ediciler ve statik oluşturucular) ve tür üyelerinin farklı sayıda tür parametreleri ile anlamını belirlenirken yoksayılacak olduğunu unutmayın. *namespace_or_type_name*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="55c2b-480">Önceki adımları her ad alanı için daha sonra başarısız olursa `N`, ad alanı ile başlayan *namespace_or_type_name* her kapsayan ad uzayı ile (eğer varsa) devam etmeden ve ile biten gerçekleşir Genel ad alanı, aşağıdaki adımları bir varlığı bulunana kadar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="55c2b-481">Varsa `K` sıfırdır ve `I` bir ad alanındaki adı `N`, ardından:</span><span class="sxs-lookup"><span data-stu-id="55c2b-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="55c2b-482">Varsa konumu burada *namespace_or_type_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive* veya *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *namespace_or_type_name* belirsiz ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="55c2b-483">Aksi takdirde, *namespace_or_type_name* adlı isim uzayına başvuruyor `I` içinde `N`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="55c2b-484">Aksi takdirde `N` adına sahip bir erişilebilir türü içeren `I` ve `K` tür parametreleri:</span><span class="sxs-lookup"><span data-stu-id="55c2b-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="55c2b-485">Varsa `K` sıfır ve konumun burada *namespace_or_type_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N` ve ad alanı bildirimi içeren bir *extern_alias_directive*  veya *using_alias_directive* , ad ilişkilendirir `I` ad alanı veya tür, ardından *namespace_or_type_name* belirsiz ve derleme zamanı hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="55c2b-486">Aksi takdirde, *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="55c2b-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="55c2b-487">Aksi halde, konum burada *namespace_or_type_name* gerçekleşir için bir ad alanı bildirimi kapsadığı `N`:</span><span class="sxs-lookup"><span data-stu-id="55c2b-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="55c2b-488">Varsa `K` sıfırdır ve ad alanı bildirimi içeren bir *extern_alias_directive* veya *using_alias_directive* , ad ilişkilendirir `I` bir içeri aktarılan ad alanıyla veya tür, ardından *namespace_or_type_name* bu ad alanı veya tür ifade eder.</span><span class="sxs-lookup"><span data-stu-id="55c2b-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="55c2b-489">Aksi takdirde, ad alanları ve tür bildirimleri tarafından aldıysanız *using_namespace_directive*s ve *using_alias_directive*ad alanı bildiriminin bir s içeren tam olarak bir erişilebilir türü adına sahip `I` ve `K` tür parametrelerindeki, ardından *namespace_or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="55c2b-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="55c2b-490">Aksi takdirde, ad alanları ve tür bildirimleri tarafından aldıysanız *using_namespace_directive*s ve *using_alias_directive*ad alanı bildiriminin bir s erişilebilir birden fazla tür içeriyor adına sahip `I` ve `K` tür parametrelerindeki, ardından *namespace_or_type_name* belirsiz ve hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="55c2b-491">Aksi takdirde, *namespace_or_type_name* olup tanımsız ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="55c2b-492">Aksi takdirde, *namespace_or_type_name* biçimindedir `N.I` veya formun `N.I<A1, ..., Ak>`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="55c2b-493">`N` ilk olarak çözümlenen bir *namespace_or_type_name*.</span><span class="sxs-lookup"><span data-stu-id="55c2b-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="55c2b-494">Varsa çözünürlüğünü `N` bir derleme zamanı hatası oluşur başarılı değil.</span><span class="sxs-lookup"><span data-stu-id="55c2b-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="55c2b-495">Aksi takdirde, `N.I` veya `N.I<A1, ..., Ak>` gibi çözümlenir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="55c2b-496">Varsa `K` sıfır ve `N` bir isim uzayına başvuruyor ve `N` ada sahip iç içe geçmiş bir ad alanı içeren `I`, ardından *namespace_or_type_name* , iç içe geçmiş ad alanına başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="55c2b-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="55c2b-497">Aksi takdirde `N` bir isim uzayına başvuruyor ve `N` adına sahip bir erişilebilir türü içeren `I` ve `K` tür parametrelerindeki, ardından *namespace_or_type_name* bu türe başvurur Belirtilen tür bağımsız değişkenleri ile oluşturulmuş.</span><span class="sxs-lookup"><span data-stu-id="55c2b-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="55c2b-498">Aksi takdirde `N` (büyük olasılıkla yapılandırılmış) bir sınıf veya yapı türe başvuruyor ve `N` veya iç içe bir erişilebilir tür adına sahip tüm temel sınıflarını içeren `I` ve `K` tür parametrelerindeki, ardından *ad alanı _or_type_name* belirli tür bağımsız değişkenleri ile oluşturulan türü ifade eder.</span><span class="sxs-lookup"><span data-stu-id="55c2b-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="55c2b-499">Birden fazla tür ise daha türetilmiş türde bildirilen türü seçilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="55c2b-500">Anlamını unutmayın `N.I` temel sınıf belirtimi çözümünün bir parçası belirlenen `N` sonra doğrudan temel sınıfını `N` nesne olarak kabul edilir ([temel sınıflar](classes.md#base-classes)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="55c2b-501">Aksi takdirde, `N.I` geçersiz bir *namespace_or_type_name*, ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="55c2b-502">A *namespace_or_type_name* statik sınıf başvurmak için izin verilir ([statik sınıflar](classes.md#static-classes)) yalnızca</span><span class="sxs-lookup"><span data-stu-id="55c2b-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="55c2b-503">*Namespace_or_type_name* olduğu `T` içinde bir *namespace_or_type_name* formun `T.I`, veya</span><span class="sxs-lookup"><span data-stu-id="55c2b-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="55c2b-504">*Namespace_or_type_name* olduğu `T` içinde bir *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) biçiminde `typeof(T)`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="55c2b-505">Tam olarak nitelenmiş adlar</span><span class="sxs-lookup"><span data-stu-id="55c2b-505">Fully qualified names</span></span>

<span data-ttu-id="55c2b-506">Her ad alanı ve türü olan bir ***tam nitelikli ad***, hangi benzersiz olarak tanımlayan ad alanı veya tür diğerleri arasından.</span><span class="sxs-lookup"><span data-stu-id="55c2b-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="55c2b-507">Tam adı ad alanı veya tür `N` şu şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="55c2b-508">Varsa `N` üye tam adı genel ad alanı olan `N`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="55c2b-509">Aksi halde, tam adı olduğunu `S.N`burada `S` tam adı ad alanı veya tür, `N` bildirilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="55c2b-510">Diğer bir deyişle, tam olarak nitelenmiş adını `N` neden tanımlayıcıları tam hiyerarşik yolu `N`genel ad alanından başlayan.</span><span class="sxs-lookup"><span data-stu-id="55c2b-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="55c2b-511">Her bir ad alanı veya tür üyesinin benzersiz bir adı olması gerektiğinden tam adı ad alanı veya tür her zaman benzersiz olduğunu izler.</span><span class="sxs-lookup"><span data-stu-id="55c2b-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="55c2b-512">Aşağıdaki örnekte, birden fazla ad alanı ve tür bildirimleri ilişkili tam adları ile birlikte gösterilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
```csharp
class A {}                // A

namespace X               // X
{
    class B               // X.B
    {
        class C {}        // X.B.C
    }

    namespace Y           // X.Y
    {
        class D {}        // X.Y.D
    }
}

namespace X.Y             // X.Y
{
    class E {}            // X.Y.E
}
```

## <a name="automatic-memory-management"></a><span data-ttu-id="55c2b-513">Otomatik bellek yönetimi</span><span class="sxs-lookup"><span data-stu-id="55c2b-513">Automatic memory management</span></span>

<span data-ttu-id="55c2b-514">C# geliştiricilerin el ile ayırarak ve nesnelerin kapladığı bellek serbest bırakma döngüsünü boşaltır otomatik bellek yönetimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="55c2b-515">Otomatik bellek yönetimi ilkeleri tarafından uygulanır bir ***çöp toplayıcı***.</span><span class="sxs-lookup"><span data-stu-id="55c2b-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="55c2b-516">Bir nesnenin bellek yönetimi yaşam döngüsü aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="55c2b-517">Nesne oluşturulduğunda bellek de ayrılır, oluşturucu çalıştırın ve nesne canlı olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="55c2b-518">Nesne veya herhangi bir bölümünü tüm olası yürütme devamını tarafından erişilemez, Yıkıcılar çalışması dışında nesne artık kullanımda olarak kabul edilir ve yok etme için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="55c2b-519">C# derleyicisi ve atık toplayıcı bir nesneye başvuran gelecekte kullanılabilir belirlemek için kod çözümleme tercih edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="55c2b-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="55c2b-520">Örneği için kapsamda olan yerel bir değişken yalnızca var olan bir nesneye başvuru olduğu halde bu yerel değişkeni hiçbir zaman tüm olası devamı olarak, geçerli yürütme yürütme yordamda işaret edecek şekilde adlandırılır, çöp toplayıcı olabilir (ancak değil için gerekli) nesne olarak artık kullanımda değerlendir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="55c2b-521">Nesne yok etme için uygun olduğunda, daha sonra belirtilmeyen bir saat yok Edicisi ([yok ediciler](classes.md#destructors)) (varsa) nesne çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="55c2b-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="55c2b-522">Normal koşullar altında nesnenin yok Edicisi, uygulamaya özel API'leri bu davranışı geçersiz kılınmasına izin verebilir ancak yalnızca bir kez çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="55c2b-523">Nesnenin yok Edicisi çalıştırıldığında, nesne veya herhangi bir bölümünü tarafından yürütme yıkıcı, çalıştırılmasını da dahil olmak üzere, tüm olası devamlılık erişilemiyorsa nesne erişilemez olarak kabul edilir ve nesne koleksiyonu için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="55c2b-524">Son olarak, nesne koleksiyonu için uygun hale geldikten sonra bir süre sonra atık toplayıcı o nesne ile ilişkili bellek kazandırır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="55c2b-525">Atık toplayıcı nesne kullanımı hakkında bilgi tutar ve bellek yönetimi ve bir nesneye bir nesne dışında yeniden konumlandırmakta artık kullanımda veya erişilemez durumda olduğunda, yeni oluşturulan bir nesne bulmak için bellek nerede gibi kararları almak için bu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="55c2b-526">Çöp toplayıcı var olup olmadığını varsayar gibi diğer diller, C# atık toplayıcı çok çeşitli bellek yönetimi ilkeleri uygulayabilir şekilde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="55c2b-527">Örneğin, C# yok ediciler çalıştırılması veya uygun oldukları hemen sonra nesnelerin toplandığını veya yıkıcıları herhangi belirli bir iş parçacığı veya herhangi bir sırada çalıştırılması gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="55c2b-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="55c2b-528">Atık toplayıcının davranışı, sınıfında statik yöntemler aracılığıyla belirli bir ölçüde denetlenebilir `System.GC`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="55c2b-529">Bu sınıf, bir koleksiyon gerçekleşmesi için çalıştırın (veya çalıştırma için), Yıkıcılar istemek için kullanılan ve VS.</span><span class="sxs-lookup"><span data-stu-id="55c2b-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="55c2b-530">Çöp Toplayıcısı nesneleri toplar ve Yıkıcılar çalıştırmak ne zaman karar içinde geniş enlem izin verildiğinden, uyumlu bir uygulaması aşağıdaki kodda gösterilen farklıdır çıktıları üretebilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="55c2b-531">Program</span><span class="sxs-lookup"><span data-stu-id="55c2b-531">The program</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }
}

class B
{
    object Ref;

    public B(object o) {
        Ref = o;
    }

    ~B() {
        Console.WriteLine("Destruct instance of B");
    }
}

class Test
{
    static void Main() {
        B b = new B(new A());
        b = null;
        GC.Collect();
        GC.WaitForPendingFinalizers();
    }
}
```
<span data-ttu-id="55c2b-532">sınıfının bir örneğini oluşturur `A` ve sınıf örneği `B`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="55c2b-533">Bu nesneler çöp toplama işlemi için uygun hale olduğunda değişkeni `b` değeri atanır `null`, bu yana bu tarihten sonra bunlara erişmek kullanıcı tarafından yazılan kodlar için mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="55c2b-534">Çıkış şunlardan biri olabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-534">The output could be either</span></span>
```
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="55c2b-535">veya</span><span class="sxs-lookup"><span data-stu-id="55c2b-535">or</span></span>
```
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="55c2b-536">hangi nesneler çöp olarak toplanacak olduklarından dil hiçbir sırası kısıtlamaları getirir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="55c2b-537">Zarif durumda "yok etme için uygun" ve "koleksiyonu için uygun" arasındaki fark önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="55c2b-538">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="55c2b-538">For example,</span></span>
```csharp
using System;

class A
{
    ~A() {
        Console.WriteLine("Destruct instance of A");
    }

    public void F() {
        Console.WriteLine("A.F");
        Test.RefA = this;
    }
}

class B
{
    public A Ref;

    ~B() {
        Console.WriteLine("Destruct instance of B");
        Ref.F();
    }
}

class Test
{
    public static A RefA;
    public static B RefB;

    static void Main() {
        RefB = new B();
        RefA = new A();
        RefB.Ref = RefA;
        RefB = null;
        RefA = null;

        // A and B now eligible for destruction
        GC.Collect();
        GC.WaitForPendingFinalizers();

        // B now eligible for collection, but A is not
        if (RefA != null)
            Console.WriteLine("RefA is not null");
    }
}
```

<span data-ttu-id="55c2b-539">Yukarıdaki programında çöp toplayıcı yok edicisinde seçerse `A` yıkıcısı önce `B`, bu programın çıkışı gelebilir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="55c2b-540">Olsa da unutmayın örneğini `A` kullanımda olmadığı ve `A`'s yok Edicisi çalıştırıldığı, yöntemleri için yine de mümkündür `A` (Bu durumda, `F`) başka bir yok ediciden çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="55c2b-541">Ayrıca, yok edicinin çalışan bir nesneyi yeniden ana hat programından kullanılabilir duruma neden olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="55c2b-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="55c2b-542">Bu durumda, çalışmasını `B`yok Edicisi örneğini neden kullanıcının `A` olduğu daha önce de kullanan dinamik başvuru tarafından erişilebilir duruma gelmesi `Test.RefA`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="55c2b-543">Çağrısından sonra `WaitForPendingFinalizers`, örneğini `B` koleksiyonu, ancak örneği için uygun `A` başvurusu nedeniyle değil, `Test.RefA`.</span><span class="sxs-lookup"><span data-stu-id="55c2b-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="55c2b-544">Karışıklık ve beklenmeyen davranışları önlemek için genellikle bir yok ediciler için yalnızca kendi nesnenin kendi alanlarında depolanan veriler üzerinde temizleme işlemini yapmak için ve tüm Eylemler, başvurulan nesneler veya static alanlar gerçekleştirmemeye fikirdir.</span><span class="sxs-lookup"><span data-stu-id="55c2b-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="55c2b-545">Yıkıcıları kullanma uygulayan bir sınıf olanak alternatiftir `System.IDisposable` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="55c2b-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="55c2b-546">Böylece, bir kaynak olarak nesne erişerek nesne kaynaklarını genellikle serbest bırakmak ne zaman belirlemek istemci nesne bir `using` deyimi ([using deyimi](statements.md#the-using-statement)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="55c2b-547">Yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="55c2b-547">Execution order</span></span>

<span data-ttu-id="55c2b-548">Yan etkilerini çalışan her bir iş parçacığı, kritik yürütme noktalarda korunur, C# programının yürütme devam eder.</span><span class="sxs-lookup"><span data-stu-id="55c2b-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="55c2b-549">A ***yan etkisi*** bir okuma veya yazma geçici bir alan, geçici olmayan değişkenine yazma, yazma için bir dış kaynağa ve bir özel durum olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="55c2b-550">Volatile alanlara başvurular, bu yan etkileri sırasını gerekir muhafaza edilir kritik yürütme noktaları olan ([geçici alanları](classes.md#volatile-fields)), `lock` deyimleri ([lock deyiminin](statements.md#the-lock-statement)), ve iş parçacığı oluşturma ve sonlandırma.</span><span class="sxs-lookup"><span data-stu-id="55c2b-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="55c2b-551">C# programının, aşağıdaki kısıtlamalara tabi yürütme sırasını değiştirmek yürütme ortamı ücretsizdir:</span><span class="sxs-lookup"><span data-stu-id="55c2b-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="55c2b-552">Veri bağımlılığı yürütme iş parçacığının içinden korunur.</span><span class="sxs-lookup"><span data-stu-id="55c2b-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="55c2b-553">Diğer bir deyişle, tüm iş parçacığı deyimlerinde özgün program sırasıyla yürütüldü gibi her değişkenin değeri olarak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="55c2b-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="55c2b-554">Kuralları korunur başlatma sıralaması ([alan başlatma](classes.md#field-initialization) ve [değişken başlatıcılar](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="55c2b-555">Geçici okuma ve yazma işlemleri göre sıralama yan etkilerini korunur ([geçici alanları](classes.md#volatile-fields)).</span><span class="sxs-lookup"><span data-stu-id="55c2b-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="55c2b-556">Bu ifadenin değeri kullanılmaz ve gerekli yan etkileri (herhangi bir yöntemi çağırmak veya geçici bir alan erişerek neden dahil) üretilir çıkarabilir ek olarak, yürütme ortamı bir ifade parçası değerlendirme değil.</span><span class="sxs-lookup"><span data-stu-id="55c2b-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="55c2b-557">Program yürütme (örneğin, başka bir iş parçacığı tarafından oluşturulan bir özel) zaman uyumsuz bir olay tarafından kesildiğinde, gözlemlenebilir bir yan etkileri özgün program sırasıyla görünür olduğunu garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="55c2b-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
