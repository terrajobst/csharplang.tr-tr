---
ms.openlocfilehash: ff31585520c9090ad92893a930327112743c8e77
ms.sourcegitcommit: 892af9016b3317a8fae12d195014dc38ba51cf16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703999"
---
# <a name="basic-concepts"></a><span data-ttu-id="98ef3-101">Temel kavramlar</span><span class="sxs-lookup"><span data-stu-id="98ef3-101">Basic concepts</span></span>

## <a name="application-startup"></a><span data-ttu-id="98ef3-102">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="98ef3-102">Application Startup</span></span>

<span data-ttu-id="98ef3-103">***Giriş noktası*** olan bir derlemeye ***uygulama***denir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-103">An assembly that has an ***entry point*** is called an ***application***.</span></span> <span data-ttu-id="98ef3-104">Bir uygulama çalıştırıldığında yeni bir ***uygulama etki alanı*** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-104">When an application is run, a new ***application domain*** is created.</span></span> <span data-ttu-id="98ef3-105">Bir uygulamanın birçok farklı örneği aynı anda aynı makinede bulunabilir ve her birinin kendi uygulama etki alanına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-105">Several different instantiations of an application may exist on the same machine at the same time, and each has its own application domain.</span></span>

<span data-ttu-id="98ef3-106">Uygulama etki alanı, uygulama durumu için kapsayıcı görevi gören uygulama yalıtımına izin veriyor.</span><span class="sxs-lookup"><span data-stu-id="98ef3-106">An application domain enables application isolation by acting as a container for application state.</span></span> <span data-ttu-id="98ef3-107">Uygulama etki alanı, uygulamada tanımlanan türler ve kullandığı sınıf kitaplıklarında bir kapsayıcı ve sınır görevi görür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-107">An application domain acts as a container and boundary for the types defined in the application and the class libraries it uses.</span></span> <span data-ttu-id="98ef3-108">Bir uygulama etki alanına yüklenen türler, başka bir uygulama etki alanına yüklenen aynı türden farklıdır ve nesne örnekleri uygulama etki alanları arasında doğrudan paylaşılmaz.</span><span class="sxs-lookup"><span data-stu-id="98ef3-108">Types loaded into one application domain are distinct from the same type loaded into another application domain, and instances of objects are not directly shared between application domains.</span></span> <span data-ttu-id="98ef3-109">Örneğin, her bir uygulama etki alanının bu türler için kendi statik değişkenlerinin kopyası vardır ve bir tür için statik bir Oluşturucu uygulama etki alanı başına en çok bir kez çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-109">For instance, each application domain has its own copy of static variables for these types, and a static constructor for a type is run at most once per application domain.</span></span> <span data-ttu-id="98ef3-110">Uygulamalar, uygulama etki alanlarının oluşturulması ve yok edilmesi için uygulamaya özgü ilke veya mekanizmalar sağlamak üzere ücretsizdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-110">Implementations are free to provide implementation-specific policy or mechanisms for the creation and destruction of application domains.</span></span>

<span data-ttu-id="98ef3-111">***Uygulama başlatma*** , yürütme ortamı, uygulamanın giriş noktası olarak adlandırılan belirlenmiş bir yöntemi çağırdığında oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-111">***Application startup*** occurs when the execution environment calls a designated method, which is referred to as the application's entry point.</span></span> <span data-ttu-id="98ef3-112">Bu giriş noktası yöntemi her zaman `Main`olarak adlandırılır ve aşağıdaki imzalardan birine sahip olabilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-112">This entry point method is always named `Main`, and can have one of the following signatures:</span></span>

```csharp
static void Main() {...}

static void Main(string[] args) {...}

static int Main() {...}

static int Main(string[] args) {...}
```

<span data-ttu-id="98ef3-113">Gösterildiği gibi, giriş noktası isteğe bağlı olarak bir `int` değeri döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-113">As shown, the entry point may optionally return an `int` value.</span></span> <span data-ttu-id="98ef3-114">Bu dönüş değeri uygulama sonlandırmada ([Uygulama sonlandırma](basic-concepts.md#application-termination)) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-114">This return value is used in application termination ([Application termination](basic-concepts.md#application-termination)).</span></span>

<span data-ttu-id="98ef3-115">Giriş noktası, isteğe bağlı olarak bir biçimsel parametreye sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-115">The entry point may optionally have one formal parameter.</span></span> <span data-ttu-id="98ef3-116">Parametre herhangi bir ada sahip olabilir, ancak parametrenin türü `string[]`olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-116">The parameter may have any name, but the type of the parameter must be `string[]`.</span></span> <span data-ttu-id="98ef3-117">Biçimsel parametre varsa, yürütme ortamı, uygulama başlatıldığında belirtilen komut satırı bağımsız değişkenlerini içeren bir `string[]` bağımsız değişkeni oluşturur ve geçirir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-117">If the formal parameter is present, the execution environment creates and passes a `string[]` argument containing the command-line arguments that were specified when the application was started.</span></span> <span data-ttu-id="98ef3-118">`string[]` bağımsız değişkeni hiçbir şekilde null değildir, ancak komut satırı bağımsız değişkenleri belirtilmemişse sıfır uzunluğuna sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-118">The `string[]` argument is never null, but it may have a length of zero if no command-line arguments were specified.</span></span>

<span data-ttu-id="98ef3-119">Yöntem C# aşırı yüklemesini desteklediğinden, bir sınıf veya yapı, her yöntemin farklı bir imzaya sahip olan birden çok tanımını içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-119">Since C# supports method overloading, a class or struct may contain multiple definitions of some method, provided each has a different signature.</span></span> <span data-ttu-id="98ef3-120">Ancak, tek bir program içinde, hiçbir sınıf veya yapı, tanımı bir uygulama giriş noktası olarak kullanılmak üzere niteleyen `Main` adlı birden fazla yöntem içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-120">However, within a single program, no class or struct may contain more than one method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="98ef3-121">`Main` diğer aşırı yüklenmiş sürümlerine izin verilir, ancak birden fazla parametreye sahip olmaları veya yalnızca parametresi `string[]`türünden farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-121">Other overloaded versions of `Main` are permitted, however, provided they have more than one parameter, or their only parameter is other than type `string[]`.</span></span>

<span data-ttu-id="98ef3-122">Bir uygulama birden fazla sınıftan veya yapıların üzerinde oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-122">An application can be made up of multiple classes or structs.</span></span> <span data-ttu-id="98ef3-123">Bu sınıfların veya yapıların birden fazla olması, tanımı bir uygulama giriş noktası olarak kullanılmak üzere niteleyen `Main` adlı bir yöntemi içermesi için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-123">It is possible for more than one of these classes or structs to contain a method called `Main` whose definition qualifies it to be used as an application entry point.</span></span> <span data-ttu-id="98ef3-124">Bu gibi durumlarda, giriş noktası olarak bu `Main` yöntemlerinden birini seçmek için bir dış mekanizmanın (bir komut satırı derleyici seçeneği gibi) kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-124">In such cases, an external mechanism (such as a command-line compiler option) must be used to select one of these `Main` methods as the entry point.</span></span>

<span data-ttu-id="98ef3-125">' C#De, her yöntemin bir sınıfın veya yapının üyesi olarak tanımlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-125">In C#, every method must be defined as a member of a class or struct.</span></span> <span data-ttu-id="98ef3-126">Genellikle, bir yöntemin tanımlanmış erişilebilirliği ([tanımlanmış erişilebilirlik](basic-concepts.md#declared-accessibility)), bildiriminde belirtilen erişim değiştiricilerine ([erişim değiştiricilerine](classes.md#access-modifiers)) göre belirlenir ve benzer şekilde, bir türün tanımlanmış erişilebilirliği, bildiriminde belirtilen erişim değiştiricilerine göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-126">Ordinarily, the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of a method is determined by the access modifiers ([Access modifiers](classes.md#access-modifiers)) specified in its declaration, and similarly the declared accessibility of a type is determined by the access modifiers specified in its declaration.</span></span> <span data-ttu-id="98ef3-127">Verilen bir türün belirli bir yönteminin çağrılabilir olması için, hem tür hem de üyenin erişilebilir olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-127">In order for a given method of a given type to be callable, both the type and the member must be accessible.</span></span> <span data-ttu-id="98ef3-128">Ancak, uygulama giriş noktası özel bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-128">However, the application entry point is a special case.</span></span> <span data-ttu-id="98ef3-129">Özellikle, yürütme ortamı uygulamanın giriş noktasına, kendisine ait erişilebilirliği ve kapsayan tür bildirimlerinin belirtilen erişilebilirliğinden bağımsız olarak erişebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-129">Specifically, the execution environment can access the application's entry point regardless of its declared accessibility and regardless of the declared accessibility of its enclosing type declarations.</span></span>

<span data-ttu-id="98ef3-130">Uygulama giriş noktası yöntemi bir genel sınıf bildiriminde bulunmayabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-130">The application entry point method may not be in a generic class declaration.</span></span>

<span data-ttu-id="98ef3-131">Diğer tüm anlarda giriş noktası yöntemleri giriş noktaları olmayan gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-131">In all other respects, entry point methods behave like those that are not entry points.</span></span>

## <a name="application-termination"></a><span data-ttu-id="98ef3-132">Uygulama sonlandırma</span><span class="sxs-lookup"><span data-stu-id="98ef3-132">Application termination</span></span>

<span data-ttu-id="98ef3-133">***Uygulama sonlandırması*** denetim yürütme ortamına geri döndürür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-133">***Application termination*** returns control to the execution environment.</span></span>

<span data-ttu-id="98ef3-134">Uygulamanın ***giriş noktası*** yönteminin dönüş türü `int`ise, döndürülen değer uygulamanın ***sonlandırma durum kodu***olarak işlev görür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-134">If the return type of the application's ***entry point*** method is `int`, the value returned serves as the application's ***termination status code***.</span></span> <span data-ttu-id="98ef3-135">Bu kodun amacı, yürütme ortamında başarı veya başarısızlık iletişimine iletişim sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-135">The purpose of this code is to allow communication of success or failure to the execution environment.</span></span>

<span data-ttu-id="98ef3-136">Giriş noktası yönteminin dönüş türü `void`, bu yöntemi sonlandıran veya ifadesi olmayan bir `return` deyimi yürüten sağ ayraca (`}`) ulaşarak, `0`sonlandırma durum koduna neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-136">If the return type of the entry point method is `void`, reaching the right brace (`}`) which terminates that method, or executing a `return` statement that has no expression, results in a termination status code of `0`.</span></span>

<span data-ttu-id="98ef3-137">Bir uygulamanın sonlandırmasından önce, daha önce atık olarak toplanmamış tüm nesneleri için yok ediciler, temizleme gizlenmemişse (örneğin, kitaplık yöntemine yapılan bir çağrı tarafından `GC.SuppressFinalize`) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-137">Prior to an application's termination, destructors for all of its objects that have not yet been garbage collected are called, unless such cleanup has been suppressed (by a call to the library method `GC.SuppressFinalize`, for example).</span></span>

## <a name="declarations"></a><span data-ttu-id="98ef3-138">Bildirimler</span><span class="sxs-lookup"><span data-stu-id="98ef3-138">Declarations</span></span>

<span data-ttu-id="98ef3-139">Bir C# programdaki bildirimler programın yapısal öğelerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="98ef3-139">Declarations in a C# program define the constituent elements of the program.</span></span> <span data-ttu-id="98ef3-140">C#Programlar, tür bildirimleri ve iç içe ad alanı bildirimleri içerebilen ad alanları ([ad alanları](namespaces.md)) kullanılarak düzenlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-140">C# programs are organized using namespaces ([Namespaces](namespaces.md)), which can contain type declarations and nested namespace declarations.</span></span> <span data-ttu-id="98ef3-141">Tür bildirimleri ([tür bildirimleri](namespaces.md#type-declarations)), sınıfları ([sınıflar](classes.md)), yapıları ([yapılar](structs.md)), arabirimleri ([arabirimleri](interfaces.md)), numaralandırmalar ([numaralandırmalar](enums.md)) ve temsilcileri ([Temsilciler](delegates.md)) tanımlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-141">Type declarations ([Type declarations](namespaces.md#type-declarations)) are used to define classes ([Classes](classes.md)), structs ([Structs](structs.md)), interfaces ([Interfaces](interfaces.md)), enums ([Enums](enums.md)), and delegates ([Delegates](delegates.md)).</span></span> <span data-ttu-id="98ef3-142">Tür bildiriminde izin verilen üye türleri tür bildiriminin biçimine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-142">The kinds of members permitted in a type declaration depend on the form of the type declaration.</span></span> <span data-ttu-id="98ef3-143">Örneğin, sınıf bildirimleri sabitler ([sabitler](classes.md#constants)), alanlar ([alanlar](classes.md#fields)), Yöntemler ([Yöntemler](classes.md#methods)), Özellikler ([Özellikler](classes.md#properties)), Olaylar ([Olaylar](classes.md#events)), Dizin oluşturucular ([Dizin](classes.md#indexers)oluşturucular), işleçler ([Işleçler](classes.md#operators)), örnek oluşturucular ([örnek oluşturucular](classes.md#instance-constructors)), statik oluşturucular ([statik oluşturucular](classes.md#static-constructors)), Yıkıcılar ([Yıkıcılar](classes.md#destructors)) ve iç içe türler ([iç içe türler](classes.md#nested-types)) için bildirimler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-143">For instance, class declarations can contain declarations for constants ([Constants](classes.md#constants)), fields ([Fields](classes.md#fields)), methods ([Methods](classes.md#methods)), properties ([Properties](classes.md#properties)), events ([Events](classes.md#events)), indexers ([Indexers](classes.md#indexers)), operators ([Operators](classes.md#operators)), instance constructors ([Instance constructors](classes.md#instance-constructors)), static constructors ([Static constructors](classes.md#static-constructors)), destructors ([Destructors](classes.md#destructors)), and nested types ([Nested types](classes.md#nested-types)).</span></span>

<span data-ttu-id="98ef3-144">Bildirim, bildirimin ait olduğu ***bildirim alanında*** bir ad tanımlar.</span><span class="sxs-lookup"><span data-stu-id="98ef3-144">A declaration defines a name in the ***declaration space*** to which the declaration belongs.</span></span> <span data-ttu-id="98ef3-145">Aşırı yüklenmiş Üyeler hariç ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)), bir bildirim alanında aynı ada sahip üyeleri tanıtan iki veya daha fazla bildirime sahip bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-145">Except for overloaded members ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)), it is a compile-time error to have two or more declarations that introduce members with the same name in a declaration space.</span></span> <span data-ttu-id="98ef3-146">Bir bildirim alanının aynı ada sahip farklı üye türleri içermesi hiçbir şekilde mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-146">It is never possible for a declaration space to contain different kinds of members with the same name.</span></span> <span data-ttu-id="98ef3-147">Örneğin, bir bildirim alanı hiçbir şekilde bir alanı ve aynı ada sahip bir yöntemi içeremez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-147">For example, a declaration space can never contain a field and a method by the same name.</span></span>

<span data-ttu-id="98ef3-148">Aşağıda açıklandığı gibi birkaç farklı türde bildirim alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-148">There are several different types of declaration spaces, as described in the following.</span></span>

*  <span data-ttu-id="98ef3-149">Bir programın tüm kaynak dosyalarında, kapsayan *namespace_declaration* olmayan *namespace_member_declaration*s, ***genel bildirim alanı***olarak adlandırılan tek bir Birleşik bildirim alanının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-149">Within all source files of a program, *namespace_member_declaration*s with no enclosing *namespace_declaration* are members of a single combined declaration space called the ***global declaration space***.</span></span>
*  <span data-ttu-id="98ef3-150">Bir programın tüm kaynak dosyaları içinde, aynı tam ad alanı adına sahip *namespace_declaration*s içindeki *namespace_member_declaration*s tek bir Birleşik bildirim alanının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-150">Within all source files of a program, *namespace_member_declaration*s within *namespace_declaration*s that have the same fully qualified namespace name are members of a single combined declaration space.</span></span>
*  <span data-ttu-id="98ef3-151">Her sınıf, yapı veya arabirim bildirimi yeni bir bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-151">Each class, struct, or interface declaration creates a new declaration space.</span></span> <span data-ttu-id="98ef3-152">Adlar, bu bildirim alanına *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s veya *type_parameter*s aracılığıyla tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-152">Names are introduced into this declaration space through *class_member_declaration*s, *struct_member_declaration*s, *interface_member_declaration*s, or *type_parameter*s.</span></span> <span data-ttu-id="98ef3-153">Aşırı yüklenmiş örnek Oluşturucu bildirimleri ve statik Oluşturucu bildirimleri dışında, bir sınıf veya yapı sınıfı veya struct ile aynı ada sahip bir üye bildirimi içeremez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-153">Except for overloaded instance constructor declarations and static constructor declarations, a class or struct cannot contain a member declaration with the same name as the class or struct.</span></span> <span data-ttu-id="98ef3-154">Bir sınıf, yapı veya arabirim, aşırı yüklenmiş yöntemlerin ve Dizin oluşturucuların bildirimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-154">A class, struct, or interface permits the declaration of overloaded methods and indexers.</span></span> <span data-ttu-id="98ef3-155">Ayrıca, bir sınıf veya yapı, aşırı yüklenmiş örnek oluşturucuların ve işleçlerin bildirimine izin verir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-155">Furthermore, a class or struct permits the declaration of overloaded instance constructors and operators.</span></span> <span data-ttu-id="98ef3-156">Örneğin, bir sınıf, yapı veya arabirim aynı ada sahip birden çok yöntem bildirimi içerebilir, bu yöntem bildirimleri imzasında ([imzalar ve aşırı yükleme](basic-concepts.md#signatures-and-overloading)) farklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-156">For example, a class, struct, or interface may contain multiple method declarations with the same name, provided these method declarations differ in their signature ([Signatures and overloading](basic-concepts.md#signatures-and-overloading)).</span></span> <span data-ttu-id="98ef3-157">Temel sınıfların bir sınıfın bildirim alanına katkıda bulunmadığını ve temel arabirimlerin bir arabirimin bildirim alanına katkıda bulunmayacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef3-157">Note that base classes do not contribute to the declaration space of a class, and base interfaces do not contribute to the declaration space of an interface.</span></span> <span data-ttu-id="98ef3-158">Bu nedenle, türetilmiş bir sınıfın veya arabirimin devralınan üye ile aynı ada sahip bir üyeyi bildirmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-158">Thus, a derived class or interface is allowed to declare a member with the same name as an inherited member.</span></span> <span data-ttu-id="98ef3-159">Bu tür bir üye devralınan üyeyi ***gizleyecek*** şekilde kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-159">Such a member is said to ***hide*** the inherited member.</span></span>
*  <span data-ttu-id="98ef3-160">Her temsilci bildirimi yeni bir bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-160">Each delegate declaration creates a new declaration space.</span></span> <span data-ttu-id="98ef3-161">Adlar, bu bildirim alanına biçimsel parametreler (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s aracılığıyla tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-161">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span>
*  <span data-ttu-id="98ef3-162">Her numaralandırma bildirimi yeni bir bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-162">Each enumeration declaration creates a new declaration space.</span></span> <span data-ttu-id="98ef3-163">Adlar bu bildirim alanına *enum_member_declarations*aracılığıyla tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-163">Names are introduced into this declaration space through *enum_member_declarations*.</span></span>
*  <span data-ttu-id="98ef3-164">Her yöntem bildirimi, Dizin Oluşturucu bildirimi, işleç bildirimi, örnek Oluşturucu bildirimi ve anonim işlev, ***yerel değişken bildirim alanı***olarak adlandırılan yeni bir bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-164">Each method declaration, indexer declaration, operator declaration, instance constructor declaration and anonymous function creates a new declaration space called a ***local variable declaration space***.</span></span> <span data-ttu-id="98ef3-165">Adlar, bu bildirim alanına biçimsel parametreler (*fixed_parameter*s ve *parameter_array*s) ve *type_parameter*s aracılığıyla tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-165">Names are introduced into this declaration space through formal parameters (*fixed_parameter*s and *parameter_array*s) and *type_parameter*s.</span></span> <span data-ttu-id="98ef3-166">İşlev üyesi veya anonim işlevin gövdesi, varsa, yerel değişken bildirim alanı içinde iç içe olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-166">The body of the function member or anonymous function, if any, is considered to be nested within the local variable declaration space.</span></span> <span data-ttu-id="98ef3-167">Yerel bir değişken bildirim alanı ve aynı ada sahip öğeleri içermesi için iç içe geçmiş yerel değişken bildirim alanı için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-167">It is an error for a local variable declaration space and a nested local variable declaration space to contain elements with the same name.</span></span> <span data-ttu-id="98ef3-168">Bu nedenle, iç içe geçmiş bir bildirim alanı içinde, bir yerel değişken veya sabit değeri, kapsayan bir bildirim alanında yerel bir değişkenle veya sabitle aynı ada sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-168">Thus, within a nested declaration space it is not possible to declare a local variable or constant with the same name as a local variable or constant in an enclosing declaration space.</span></span> <span data-ttu-id="98ef3-169">İki bildirim alanı, hiçbir bildirim alanı diğeri içermediği sürece aynı ada sahip öğeleri içermesi için mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-169">It is possible for two declaration spaces to contain elements with the same name as long as neither declaration space contains the other.</span></span>
*  <span data-ttu-id="98ef3-170">Her bir *blok* veya *switch_block* , ayrıca bir *for*, *foreach* ve *using* ifadesi, yerel değişkenler ve yerel sabitler için yerel bir değişken bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-170">Each *block* or *switch_block* , as well as a *for*, *foreach* and *using* statement, creates a local variable declaration space for local variables and local constants .</span></span> <span data-ttu-id="98ef3-171">Adlar bu bildirim alanına *local_variable_declaration*s ve *local_constant_declaration*s aracılığıyla tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-171">Names are introduced into this declaration space through *local_variable_declaration*s and *local_constant_declaration*s.</span></span> <span data-ttu-id="98ef3-172">Bir işlev üyesinin veya anonim işlevin gövdesi içinde veya içinde oluşan blokların, parametreleri için bu işlevler tarafından tanımlanan yerel değişken bildirim alanı içinde iç içe geçmiş olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef3-172">Note that blocks that occur as or within the body of a function member or anonymous function are nested within the local variable declaration space declared by those functions for their parameters.</span></span> <span data-ttu-id="98ef3-173">Bu nedenle, örneğin bir yerel değişkene ve aynı ada sahip bir parametreye sahip bir yöntem olması hatadır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-173">Thus it is an error to have e.g. a method with a local variable and a parameter of the same name.</span></span>
*  <span data-ttu-id="98ef3-174">Her *blok* veya *switch_block* , Etiketler için ayrı bir bildirim alanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-174">Each *block* or *switch_block* creates a separate declaration space for labels.</span></span> <span data-ttu-id="98ef3-175">Adlar, *labeled_statement*s aracılığıyla bu bildirim alanına tanıtılmıştır ve adlara *goto_statement*s aracılığıyla başvurulur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-175">Names are introduced into this declaration space through *labeled_statement*s, and the names are referenced through *goto_statement*s.</span></span> <span data-ttu-id="98ef3-176">Bir bloğun ***etiket bildirim alanı*** , iç içe geçmiş blokları içerir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-176">The ***label declaration space*** of a block includes any nested blocks.</span></span> <span data-ttu-id="98ef3-177">Bu nedenle, iç içe bir blok içinde, kapsayan bir blok içindeki bir etiketle aynı ada sahip bir etiketi bildirmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-177">Thus, within a nested block it is not possible to declare a label with the same name as a label in an enclosing block.</span></span>

<span data-ttu-id="98ef3-178">Adların bildirildiği metin sırası genellikle anlamlı değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-178">The textual order in which names are declared is generally of no significance.</span></span> <span data-ttu-id="98ef3-179">Özellikle, metinsel sıralama, ad alanları, sabitler, Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar, statik oluşturucular ve türler için önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-179">In particular, textual order is not significant for the declaration and use of namespaces, constants, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors, and types.</span></span> <span data-ttu-id="98ef3-180">Bildirim sırası aşağıdaki yollarla önemlidir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-180">Declaration order is significant in the following ways:</span></span>

*  <span data-ttu-id="98ef3-181">Alan bildirimleri ve yerel değişken bildirimleri için bildirim sırası, başlatıcılarının (varsa) yürütülme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-181">Declaration order for field declarations and local variable declarations determines the order in which their initializers (if any) are executed.</span></span>
*  <span data-ttu-id="98ef3-182">Yerel değişkenlerin kullanılmadan önce tanımlanması gerekir ([kapsamlar](basic-concepts.md#scopes)).</span><span class="sxs-lookup"><span data-stu-id="98ef3-182">Local variables must be defined before they are used ([Scopes](basic-concepts.md#scopes)).</span></span>
*  <span data-ttu-id="98ef3-183">*Constant_expression* değerleri atlandığında enum üye bildirimleri ([enum üyeleri](enums.md#enum-members)) için bildirim sırası önemlidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-183">Declaration order for enum member declarations ([Enum members](enums.md#enum-members)) is significant when *constant_expression* values are omitted.</span></span>

<span data-ttu-id="98ef3-184">Bir ad alanının bildirim alanı "açık sona erdi" ve aynı tam ada sahip iki ad alanı bildirimi aynı bildirim alanına katkıda bulunur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-184">The declaration space of a namespace is "open ended", and two namespace declarations with the same fully qualified name contribute to the same declaration space.</span></span> <span data-ttu-id="98ef3-185">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98ef3-185">For example</span></span>
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

<span data-ttu-id="98ef3-186">Yukarıdaki iki ad alanı bildirimi aynı bildirim alanına katkıda bulunur ve bu örnekte, `Megacorp.Data.Customer` ve `Megacorp.Data.Order`tam nitelikli adlara sahip iki sınıf bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="98ef3-186">The two namespace declarations above contribute to the same declaration space, in this case declaring two classes with the fully qualified names `Megacorp.Data.Customer` and `Megacorp.Data.Order`.</span></span> <span data-ttu-id="98ef3-187">İki bildirim aynı bildirim alanına katkıda bulunduğundan, her biri aynı ada sahip bir sınıfın bildirimini içeriyorsa, derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-187">Because the two declarations contribute to the same declaration space, it would have caused a compile-time error if each contained a declaration of a class with the same name.</span></span>

<span data-ttu-id="98ef3-188">Yukarıda belirtildiği gibi, bir bloğun bildirim alanı, iç içe geçmiş blokları içerir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-188">As specified above, the declaration space of a block includes any nested blocks.</span></span> <span data-ttu-id="98ef3-189">Bu nedenle, aşağıdaki örnekte `F` ve `G` yöntemleri derleme zamanı hatasına neden olur çünkü ad `i` dış blokta bildirildiği ve iç blokta yeniden bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-189">Thus, in the following example, the `F` and `G` methods result in a compile-time error because the name `i` is declared in the outer block and cannot be redeclared in the inner block.</span></span> <span data-ttu-id="98ef3-190">Ancak, iki `i`ayrı iç içe olmayan bloklar halinde bildirildiği için `H` ve `I` yöntemleri geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-190">However, the `H` and `I` methods are valid since the two `i`'s are declared in separate non-nested blocks.</span></span>

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

## <a name="members"></a><span data-ttu-id="98ef3-191">Üyeler</span><span class="sxs-lookup"><span data-stu-id="98ef3-191">Members</span></span>

<span data-ttu-id="98ef3-192">Ad alanları ve türlerin ***üyeleri***vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-192">Namespaces and types have ***members***.</span></span> <span data-ttu-id="98ef3-193">Bir varlığın üyeleri, varlığa yönelik bir başvuruya ve ardından bir "`.`" belirteci ve sonra üyenin adı ile başlayan tam adı kullanarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-193">The members of an entity are generally available through the use of a qualified name that starts with a reference to the entity, followed by a "`.`" token, followed by the name of the member.</span></span>

<span data-ttu-id="98ef3-194">Bir türün üyeleri tür bildiriminde bildirilmiştir veya türün temel sınıfından ***devralınır*** .</span><span class="sxs-lookup"><span data-stu-id="98ef3-194">Members of a type are either declared in the type declaration or ***inherited*** from the base class of the type.</span></span> <span data-ttu-id="98ef3-195">Bir tür taban sınıftan devralırsa, örnek oluşturucular, Yıkıcılar ve statik oluşturucular hariç olmak üzere temel sınıfın tüm üyeleri türetilmiş türün üyesi olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-195">When a type inherits from a base class, all members of the base class, except instance constructors, destructors and static constructors, become members of the derived type.</span></span> <span data-ttu-id="98ef3-196">Bir temel sınıf üyesinin tanımlanmış erişilebilirliği üyenin devralınıp alınmayacağını denetlemez — devralma, örnek Oluşturucu, statik oluşturucu veya yıkıcı olmayan herhangi bir üyeye genişletilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-196">The declared accessibility of a base class member does not control whether the member is inherited—inheritance extends to any member that isn't an instance constructor, static constructor, or destructor.</span></span> <span data-ttu-id="98ef3-197">Ancak, devralınan bir üye, bildirildiği Erişilebilirlik (tarafından[tanımlanan erişilebilirlik](basic-concepts.md#declared-accessibility)) veya türün kendisindeki bir bildirim tarafından gizlendiği için türetilmiş bir tür içinde erişilebilir olmayabilir ([Devralma yoluyla gizleme](basic-concepts.md#hiding-through-inheritance)).</span><span class="sxs-lookup"><span data-stu-id="98ef3-197">However, an inherited member may not be accessible in a derived type, either because of its declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) or because it is hidden by a declaration in the type itself ([Hiding through inheritance](basic-concepts.md#hiding-through-inheritance)).</span></span>

### <a name="namespace-members"></a><span data-ttu-id="98ef3-198">Ad alanı üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-198">Namespace members</span></span>

<span data-ttu-id="98ef3-199">Kapsanan ad alanı olmayan ad alanları ve türler, ***genel ad***alanının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-199">Namespaces and types that have no enclosing namespace are members of the ***global namespace***.</span></span> <span data-ttu-id="98ef3-200">Bu, doğrudan genel bildirim alanında belirtilen adlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-200">This corresponds directly to the names declared in the global declaration space.</span></span>

<span data-ttu-id="98ef3-201">Ad alanı içinde belirtilen ad alanları ve türler, bu ad alanının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-201">Namespaces and types declared within a namespace are members of that namespace.</span></span> <span data-ttu-id="98ef3-202">Bu, doğrudan ad alanının bildirim alanında belirtilen adlara karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-202">This corresponds directly to the names declared in the declaration space of the namespace.</span></span>

<span data-ttu-id="98ef3-203">Ad alanlarının erişim kısıtlamaları yoktur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-203">Namespaces have no access restrictions.</span></span> <span data-ttu-id="98ef3-204">Özel, korumalı veya iç ad alanları bildirmek mümkün değildir ve ad alanı adları her zaman herkese açıktır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-204">It is not possible to declare private, protected, or internal namespaces, and namespace names are always publicly accessible.</span></span>

### <a name="struct-members"></a><span data-ttu-id="98ef3-205">Yapı üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-205">Struct members</span></span>

<span data-ttu-id="98ef3-206">Bir yapının üyeleri, yapı içinde ve yapının doğrudan temel sınıfından devralınan Üyeler ve `object`dolaylı temel sınıf olan `System.ValueType`.</span><span class="sxs-lookup"><span data-stu-id="98ef3-206">The members of a struct are the members declared in the struct and the members inherited from the struct's direct base class `System.ValueType` and the indirect base class `object`.</span></span>

<span data-ttu-id="98ef3-207">Basit bir türün üyeleri, basit türün diğer adı olan yapı türünün üyelerine karşılık gelir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-207">The members of a simple type correspond directly to the members of the struct type aliased by the simple type:</span></span>

*  <span data-ttu-id="98ef3-208">`sbyte` üyeleri `System.SByte` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-208">The members of `sbyte` are the members of the `System.SByte` struct.</span></span>
*  <span data-ttu-id="98ef3-209">`byte` üyeleri `System.Byte` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-209">The members of `byte` are the members of the `System.Byte` struct.</span></span>
*  <span data-ttu-id="98ef3-210">`short` üyeleri `System.Int16` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-210">The members of `short` are the members of the `System.Int16` struct.</span></span>
*  <span data-ttu-id="98ef3-211">`ushort` üyeleri `System.UInt16` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-211">The members of `ushort` are the members of the `System.UInt16` struct.</span></span>
*  <span data-ttu-id="98ef3-212">`int` üyeleri `System.Int32` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-212">The members of `int` are the members of the `System.Int32` struct.</span></span>
*  <span data-ttu-id="98ef3-213">`uint` üyeleri `System.UInt32` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-213">The members of `uint` are the members of the `System.UInt32` struct.</span></span>
*  <span data-ttu-id="98ef3-214">`long` üyeleri `System.Int64` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-214">The members of `long` are the members of the `System.Int64` struct.</span></span>
*  <span data-ttu-id="98ef3-215">`ulong` üyeleri `System.UInt64` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-215">The members of `ulong` are the members of the `System.UInt64` struct.</span></span>
*  <span data-ttu-id="98ef3-216">`char` üyeleri `System.Char` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-216">The members of `char` are the members of the `System.Char` struct.</span></span>
*  <span data-ttu-id="98ef3-217">`float` üyeleri `System.Single` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-217">The members of `float` are the members of the `System.Single` struct.</span></span>
*  <span data-ttu-id="98ef3-218">`double` üyeleri `System.Double` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-218">The members of `double` are the members of the `System.Double` struct.</span></span>
*  <span data-ttu-id="98ef3-219">`decimal` üyeleri `System.Decimal` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-219">The members of `decimal` are the members of the `System.Decimal` struct.</span></span>
*  <span data-ttu-id="98ef3-220">`bool` üyeleri `System.Boolean` yapısının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-220">The members of `bool` are the members of the `System.Boolean` struct.</span></span>

### <a name="enumeration-members"></a><span data-ttu-id="98ef3-221">Listeleme üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-221">Enumeration members</span></span>

<span data-ttu-id="98ef3-222">Bir numaralandırmanın üyeleri, numaralandırmada ve Numaralandırmadaki doğrudan taban sınıftan devralınan üyelerin `System.Enum` ve dolaylı temel sınıfların `System.ValueType` ve `object`tarafından belirtilen sabitler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-222">The members of an enumeration are the constants declared in the enumeration and the members inherited from the enumeration's direct base class `System.Enum` and the indirect base classes `System.ValueType` and `object`.</span></span>

### <a name="class-members"></a><span data-ttu-id="98ef3-223">Sınıf üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-223">Class members</span></span>

<span data-ttu-id="98ef3-224">Bir sınıfın üyeleri sınıfta ve temel sınıftan devralınan üyelere (temel sınıfı olmayan `object` hariç) eklenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-224">The members of a class are the members declared in the class and the members inherited from the base class (except for class `object` which has no base class).</span></span> <span data-ttu-id="98ef3-225">Temel sınıftan devralınan Üyeler, taban sınıfının sabitler, alanları, yöntemleri, özellikleri, olayları, Dizin oluşturucular, işleçler ve türleri ve temel sınıfın statik oluşturucuları değil, temel sınıfın türlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-225">The members inherited from the base class include the constants, fields, methods, properties, events, indexers, operators, and types of the base class, but not the instance constructors, destructors and static constructors of the base class.</span></span> <span data-ttu-id="98ef3-226">Temel sınıf üyeleri, erişilebilirliğine göre devralınmaz.</span><span class="sxs-lookup"><span data-stu-id="98ef3-226">Base class members are inherited without regard to their accessibility.</span></span>

<span data-ttu-id="98ef3-227">Bir sınıf bildirimi, sabitler, alanlar, Yöntemler, özellikler, olaylar, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar, statik oluşturucular ve türler için bildirimler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-227">A class declaration may contain declarations of constants, fields, methods, properties, events, indexers, operators, instance constructors, destructors, static constructors and types.</span></span>

<span data-ttu-id="98ef3-228">`object` ve `string` üyeleri, diğer adı olan sınıf türlerinin üyelerine doğrudan karşılık gelir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-228">The members of `object` and `string` correspond directly to the members of the class types they alias:</span></span>

*  <span data-ttu-id="98ef3-229">`object` üyeleri `System.Object` sınıfının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-229">The members of `object` are the members of the `System.Object` class.</span></span>
*  <span data-ttu-id="98ef3-230">`string` üyeleri `System.String` sınıfının üyeleridir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-230">The members of `string` are the members of the `System.String` class.</span></span>

### <a name="interface-members"></a><span data-ttu-id="98ef3-231">Arabirim üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-231">Interface members</span></span>

<span data-ttu-id="98ef3-232">Bir arabirimin üyeleri, arabirimde ve arabirimin tüm temel arabirimlerinde belirtilen üyelerdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-232">The members of an interface are the members declared in the interface and in all base interfaces of the interface.</span></span> <span data-ttu-id="98ef3-233">`object` sınıftaki Üyeler, herhangi bir arabirimin ([arabirim üyesi](interfaces.md#interface-members)) üyesi değil, kesinlikle konuşuyor.</span><span class="sxs-lookup"><span data-stu-id="98ef3-233">The members in class `object` are not, strictly speaking, members of any interface ([Interface members](interfaces.md#interface-members)).</span></span> <span data-ttu-id="98ef3-234">Ancak, `object` sınıf üyeleri herhangi bir arabirim türünde ([üye arama](expressions.md#member-lookup)) üye arama yoluyla kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-234">However, the members in class `object` are available via member lookup in any interface type ([Member lookup](expressions.md#member-lookup)).</span></span>

### <a name="array-members"></a><span data-ttu-id="98ef3-235">Dizi üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-235">Array members</span></span>

<span data-ttu-id="98ef3-236">Bir dizinin üyeleri, `System.Array`sınıfından devralınan üyelerdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-236">The members of an array are the members inherited from class `System.Array`.</span></span>

### <a name="delegate-members"></a><span data-ttu-id="98ef3-237">Temsilci üyeleri</span><span class="sxs-lookup"><span data-stu-id="98ef3-237">Delegate members</span></span>

<span data-ttu-id="98ef3-238">Bir temsilcinin üyeleri, `System.Delegate`sınıfından devralınan üyelerdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-238">The members of a delegate are the members inherited from class `System.Delegate`.</span></span>

## <a name="member-access"></a><span data-ttu-id="98ef3-239">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="98ef3-239">Member access</span></span>

<span data-ttu-id="98ef3-240">Üyelerin bildirimleri üye erişimi üzerinde denetime izin verir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-240">Declarations of members allow control over member access.</span></span> <span data-ttu-id="98ef3-241">Bir üyenin erişilebilirliği, varsa, hemen kapsayan türün erişilebilirliği ile birleştirilmiş üyenin tanımlanmış erişilebilirliği ([belirtilen erişilebilirlik](basic-concepts.md#declared-accessibility)) tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-241">The accessibility of a member is established by the declared accessibility ([Declared accessibility](basic-concepts.md#declared-accessibility)) of the member combined with the accessibility of the immediately containing type, if any.</span></span>

<span data-ttu-id="98ef3-242">Belirli bir üyeye erişime izin verildiğinde, üyeye ***erişilebilir***olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-242">When access to a particular member is allowed, the member is said to be ***accessible***.</span></span> <span data-ttu-id="98ef3-243">Buna karşılık, belirli bir üyeye erişime izin verilmediğinde, üyenin ***erişilemez***olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-243">Conversely, when access to a particular member is disallowed, the member is said to be ***inaccessible***.</span></span> <span data-ttu-id="98ef3-244">Bir üyeye erişim, erişimin gerçekleştiği metin konumu üyenin erişilebilirlik etki alanına ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) dahil edildiğinde izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-244">Access to a member is permitted when the textual location in which the access takes place is included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>

### <a name="declared-accessibility"></a><span data-ttu-id="98ef3-245">Tanımlanan erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="98ef3-245">Declared accessibility</span></span>

<span data-ttu-id="98ef3-246">Bir üyenin ***tanımlanmış erişilebilirliği*** aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-246">The ***declared accessibility*** of a member can be one of the following:</span></span>

*  <span data-ttu-id="98ef3-247">Public, üye bildiriminde `public` değiştiricisi eklenerek seçilidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-247">Public, which is selected by including a `public` modifier in the member declaration.</span></span> <span data-ttu-id="98ef3-248">`public` sezgisel anlamı "erişim sınırlı değil" anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-248">The intuitive meaning of `public` is "access not limited".</span></span>
*  <span data-ttu-id="98ef3-249">Korumalı, üye bildiriminde `protected` değiştiricisi eklenerek seçilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-249">Protected, which is selected by including a `protected` modifier in the member declaration.</span></span> <span data-ttu-id="98ef3-250">`protected` sezgisel anlamı, "Access 'in kapsayan sınıftan türetilmiş sınıf veya türler ile sınırlıdır".</span><span class="sxs-lookup"><span data-stu-id="98ef3-250">The intuitive meaning of `protected` is "access limited to the containing class or types derived from the containing class".</span></span>
*  <span data-ttu-id="98ef3-251">İç, üye bildiriminde `internal` değiştiricisi eklenerek seçilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-251">Internal, which is selected by including an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="98ef3-252">`internal` sezgisel anlamı, "Bu programla sınırlı erişim" dir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-252">The intuitive meaning of `internal` is "access limited to this program".</span></span>
*  <span data-ttu-id="98ef3-253">Üye bildiriminde hem `protected` hem de `internal` değiştiricisi dahil tarafından seçilen, korunan dahili (yani protected veya internal).</span><span class="sxs-lookup"><span data-stu-id="98ef3-253">Protected internal (meaning protected or internal), which is selected by including both a `protected` and an `internal` modifier in the member declaration.</span></span> <span data-ttu-id="98ef3-254">`protected internal` sezgisel anlamı, "Bu program ile sınırlı erişim veya içeren sınıftan türetilmiş türler" dir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-254">The intuitive meaning of `protected internal` is "access limited to this program or types derived from the containing class".</span></span>
*  <span data-ttu-id="98ef3-255">Özel, üye bildiriminde `private` değiştiricisi eklenerek seçilidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-255">Private, which is selected by including a `private` modifier in the member declaration.</span></span> <span data-ttu-id="98ef3-256">`private` sezgisel anlamı, "erişim içeren tür ile sınırlı" dir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-256">The intuitive meaning of `private` is "access limited to the containing type".</span></span>

<span data-ttu-id="98ef3-257">Bir üye bildiriminin gerçekleştiği içeriğe bağlı olarak, yalnızca belirli bir tanımlı erişilebilirlik türlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-257">Depending on the context in which a member declaration takes place, only certain types of declared accessibility are permitted.</span></span> <span data-ttu-id="98ef3-258">Ayrıca, bir üye bildirimi herhangi bir erişim değiştiricisi içermiyorsa, bildirimin gerçekleştiği bağlam, varsayılan olarak belirtilen erişilebilirliği belirler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-258">Furthermore, when a member declaration does not include any access modifiers, the context in which the declaration takes place determines the default declared accessibility.</span></span>

*  <span data-ttu-id="98ef3-259">Ad alanları örtük olarak `public` olarak tanımlanmış erişilebilirliği vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-259">Namespaces implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="98ef3-260">Ad alanı bildirimlerinde erişim değiştiricilerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-260">No access modifiers are allowed on namespace declarations.</span></span>
*  <span data-ttu-id="98ef3-261">Derleme birimlerinde veya ad alanlarında tanımlanan türler, `internal` tanımlanmış erişilebilirliği `internal` ve varsayılan olarak tanımlanmış erişilebilirliği `public` olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-261">Types declared in compilation units or namespaces can have `public` or `internal` declared accessibility and default to `internal` declared accessibility.</span></span>
*  <span data-ttu-id="98ef3-262">Sınıf üyeleri, tanımlanmış erişilebilirliği `private` için beş tür tanımlanmış erişilebilirliği ve varsayılanı içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-262">Class members can have any of the five kinds of declared accessibility and default to `private` declared accessibility.</span></span> <span data-ttu-id="98ef3-263">(Bir sınıfın üyesi olarak belirtilen bir türün beş tür tanımlanmış erişilebilirliği olabilir, ancak bir ad alanının üyesi olarak bildirildiği bir tür yalnızca `public` veya `internal` tarafından tanımlanan erişilebilirliği içerebilir.)</span><span class="sxs-lookup"><span data-stu-id="98ef3-263">(Note that a type declared as a member of a class can have any of the five kinds of declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="98ef3-264">Yapı üyelerinde `public`, `internal`veya `private` tanımlanmış erişilebilirliği ve varsayılan olarak, yapılar örtük bir şekilde mühürlendikleri için erişilebilirliği `private`.</span><span class="sxs-lookup"><span data-stu-id="98ef3-264">Struct members can have `public`, `internal`, or `private` declared accessibility and default to `private` declared accessibility because structs are implicitly sealed.</span></span> <span data-ttu-id="98ef3-265">Bir yapıda tanıtılan yapı üyeleri (yani, bu yapı tarafından devralınmaz) `protected` veya `protected internal` tarafından tanımlanan erişilebilirliği içeremez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-265">Struct members introduced in a struct (that is, not inherited by that struct) cannot have `protected` or `protected internal` declared accessibility.</span></span> <span data-ttu-id="98ef3-266">(Bir yapının üyesi olarak belirtilen bir türün `public`, `internal`veya `private` tanımlanmış erişilebilirliği olabileceğini, ancak bir ad alanının üyesi olarak bildirildiği bir türün yalnızca `public` veya `internal` tarafından tanımlanan erişilebilirliği olabilir.)</span><span class="sxs-lookup"><span data-stu-id="98ef3-266">(Note that a type declared as a member of a struct can have `public`, `internal`, or `private` declared accessibility, whereas a type declared as a member of a namespace can have only `public` or `internal` declared accessibility.)</span></span>
*  <span data-ttu-id="98ef3-267">Arabirim üyeleri örtük olarak `public` olarak tanımlanmış erişilebilirliği vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-267">Interface members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="98ef3-268">Arabirim üye bildirimlerinde erişim değiştiricilerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-268">No access modifiers are allowed on interface member declarations.</span></span>
*  <span data-ttu-id="98ef3-269">Numaralandırma üyelerinin örtülü olarak `public` erişilebilirliği vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-269">Enumeration members implicitly have `public` declared accessibility.</span></span> <span data-ttu-id="98ef3-270">Numaralandırma üye bildirimlerinde erişim değiştiricilerine izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-270">No access modifiers are allowed on enumeration member declarations.</span></span>

### <a name="accessibility-domains"></a><span data-ttu-id="98ef3-271">Erişilebilirlik etki alanları</span><span class="sxs-lookup"><span data-stu-id="98ef3-271">Accessibility domains</span></span>

<span data-ttu-id="98ef3-272">Bir üyenin ***erişilebilirlik etki alanı*** , üyeye erişime izin verilen program metninin (muhtemelen ayrık) bölümlerinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-272">The ***accessibility domain*** of a member consists of the (possibly disjoint) sections of program text in which access to the member is permitted.</span></span> <span data-ttu-id="98ef3-273">Bir üyenin erişilebilirlik etki alanını tanımlama amacıyla bir üyenin bir tür içinde ***bildirilmemiş olması ve*** bir üyenin başka bir tür içinde bildirildiği durumlarda ***iç içe*** olduğu söylenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-273">For purposes of defining the accessibility domain of a member, a member is said to be ***top-level*** if it is not declared within a type, and a member is said to be ***nested*** if it is declared within another type.</span></span> <span data-ttu-id="98ef3-274">Ayrıca, programın ***Program metni*** programın tüm kaynak dosyalarında bulunan tüm program metinleri olarak tanımlanır ve bir türün program metni, bu türün *type_declaration*s (büyük olasılıkla tür içinde iç içe geçmiş türler dahil) içindeki tüm program metinleri olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-274">Furthermore, the ***program text*** of a program is defined as all program text contained in all source files of the program, and the program text of a type is defined as all program text contained in the *type_declaration*s of that type (including, possibly, types that are nested within the type).</span></span>

<span data-ttu-id="98ef3-275">Önceden tanımlanmış bir türün erişilebilirlik etki alanı (örneğin `object`, `int`veya `double`) sınırsızdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-275">The accessibility domain of a predefined type (such as `object`, `int`, or `double`) is unlimited.</span></span>

<span data-ttu-id="98ef3-276">Bir program `P` olarak belirtilen üst düzey ilişkisiz türdeki `T` ([bağlı ve ilişkisiz türler](types.md#bound-and-unbound-types)) erişilebilirlik etki alanı aşağıdaki gibi tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="98ef3-276">The accessibility domain of a top-level unbound type `T` ([Bound and unbound types](types.md#bound-and-unbound-types)) that is declared in a program `P` is defined as follows:</span></span>

*  <span data-ttu-id="98ef3-277">`T` belirtilen erişilebilirliği `public`ise, `T` erişilebilirlik etki alanı `P` program metni ve `P`başvuran programlar olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-277">If the declared accessibility of `T` is `public`, the accessibility domain of `T` is the program text of `P` and any program that references `P`.</span></span>
*  <span data-ttu-id="98ef3-278">`T` belirtilen erişilebilirliği `internal`ise, `T` erişilebilirlik etki alanı `P`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-278">If the declared accessibility of `T` is `internal`, the accessibility domain of `T` is the program text of `P`.</span></span>

<span data-ttu-id="98ef3-279">Bu tanımlardan, üst düzey ilişkisiz türdeki erişilebilirlik etki alanının her zaman en azından bu türün bildirildiği programın program metni olduğunu takip eder.</span><span class="sxs-lookup"><span data-stu-id="98ef3-279">From these definitions it follows that the accessibility domain of a top-level unbound type is always at least the program text of the program in which that type is declared.</span></span>

<span data-ttu-id="98ef3-280">Oluşturulmuş bir tür için erişilebilirlik etki alanı `T<A1, ..., An>`, ilişkisiz genel tür `T` erişilebilirlik etki alanının ve tür bağımsız değişkenlerinin erişilebilirlik etki alanlarının `A1, ..., An`kesişimidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-280">The accessibility domain for a constructed type `T<A1, ..., An>` is the intersection of the accessibility domain of the unbound generic type `T` and the accessibility domains of the type arguments `A1, ..., An`.</span></span>

<span data-ttu-id="98ef3-281">Bir program `P` içinde `T` bir tür içinde bildirildiği iç içe üye `M` erişilebilirlik etki alanı, aşağıdaki gibi tanımlanır (`M` büyük olasılıkla bir tür olabilir):</span><span class="sxs-lookup"><span data-stu-id="98ef3-281">The accessibility domain of a nested member `M` declared in a type `T` within a program `P` is defined as follows (noting that `M` itself may possibly be a type):</span></span>

*  <span data-ttu-id="98ef3-282">`M` belirtilen erişilebilirliği `public`ise, `M` erişilebilirlik etki alanı `T`erişilebilirlik etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-282">If the declared accessibility of `M` is `public`, the accessibility domain of `M` is the accessibility domain of `T`.</span></span>
*  <span data-ttu-id="98ef3-283">`M` belirtilen erişilebilirliği `protected internal`ise, `P` program metninin birleşimi `D` ve `T`dışında bir şekilde bildirildiği her türün program metni.`P`</span><span class="sxs-lookup"><span data-stu-id="98ef3-283">If the declared accessibility of `M` is `protected internal`, let `D` be the union of the program text of `P` and the program text of any type derived from `T`, which is declared outside `P`.</span></span> <span data-ttu-id="98ef3-284">`M` erişilebilirlik etki alanı `D`ile `T` erişilebilirlik etki alanının kesişimidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-284">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="98ef3-285">`M` belirtilen erişilebilirliği `protected`ise, `T` program metninin birleşimi ve `T`türetilen herhangi bir türdeki program metni `D`.</span><span class="sxs-lookup"><span data-stu-id="98ef3-285">If the declared accessibility of `M` is `protected`, let `D` be the union of the program text of `T` and the program text of any type derived from `T`.</span></span> <span data-ttu-id="98ef3-286">`M` erişilebilirlik etki alanı `D`ile `T` erişilebilirlik etki alanının kesişimidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-286">The accessibility domain of `M` is the intersection of the accessibility domain of `T` with `D`.</span></span>
*  <span data-ttu-id="98ef3-287">`M` belirtilen erişilebilirliği `internal`ise, `M` erişilebilirlik etki alanı, `P`program metniyle `T` erişilebilirlik etki alanının kesişimidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-287">If the declared accessibility of `M` is `internal`, the accessibility domain of `M` is the intersection of the accessibility domain of `T` with the program text of `P`.</span></span>
*  <span data-ttu-id="98ef3-288">`M` belirtilen erişilebilirliği `private`ise, `M` erişilebilirlik etki alanı `T`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-288">If the declared accessibility of `M` is `private`, the accessibility domain of `M` is the program text of `T`.</span></span>

<span data-ttu-id="98ef3-289">Bu tanımlardan, iç içe bir üyenin erişilebilirlik etki alanının her zaman en azından üyenin bildirildiği türdeki program metni olduğunu takip eder.</span><span class="sxs-lookup"><span data-stu-id="98ef3-289">From these definitions it follows that the accessibility domain of a nested member is always at least the program text of the type in which the member is declared.</span></span> <span data-ttu-id="98ef3-290">Ayrıca, bir üyenin erişilebilirlik etki alanının, üyenin bildirildiği türün erişilebilirlik etki alanından daha da hiç olmadığı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-290">Furthermore, it follows that the accessibility domain of a member is never more inclusive than the accessibility domain of the type in which the member is declared.</span></span>

<span data-ttu-id="98ef3-291">Sezgisel koşullarda, bir tür veya üye `M` erişildiğinde, erişime izin verildiğinden emin olmak için aşağıdaki adımlar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-291">In intuitive terms, when a type or member `M` is accessed, the following steps are evaluated to ensure that the access is permitted:</span></span>

*  <span data-ttu-id="98ef3-292">İlk olarak, `M` bir tür içinde bildirilirse (bir derleme birimi veya ad alanı aksine), bu tür erişilebilir değilse bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-292">First, if `M` is declared within a type (as opposed to a compilation unit or a namespace), a compile-time error occurs if that type is not accessible.</span></span>
*  <span data-ttu-id="98ef3-293">`M` `public`, erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-293">Then, if `M` is `public`, the access is permitted.</span></span>
*  <span data-ttu-id="98ef3-294">Aksi takdirde, `M` `protected internal`, `M` bildirildiği programda meydana gelirse veya `M` bildirildiği sınıftan türetilmiş bir sınıf içinde gerçekleşirse veya türetilmiş sınıf türü ([Örneğin, korumalı erişim](basic-concepts.md#protected-access-for-instance-members)) üzerinden gerçekleşmişse erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-294">Otherwise, if `M` is `protected internal`, the access is permitted if it occurs within the program in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="98ef3-295">Aksi takdirde, `M` `protected`, `M` bildirildiği sınıf içinde meydana gelirse veya `M` bildirildiği sınıftan türetilmiş bir sınıf içinde gerçekleşirse veya türetilmiş sınıf türü ([Örneğin, korumalı erişim](basic-concepts.md#protected-access-for-instance-members)) üzerinden gerçekleşmişse erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-295">Otherwise, if `M` is `protected`, the access is permitted if it occurs within the class in which `M` is declared, or if it occurs within a class derived from the class in which `M` is declared and takes place through the derived class type ([Protected access for instance members](basic-concepts.md#protected-access-for-instance-members)).</span></span>
*  <span data-ttu-id="98ef3-296">Aksi takdirde, `M` `internal`, `M` bildirildiği programın içinde meydana gelirse erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-296">Otherwise, if `M` is `internal`, the access is permitted if it occurs within the program in which `M` is declared.</span></span>
*  <span data-ttu-id="98ef3-297">Aksi takdirde, `M` `private`, `M` bildirildiği tür içinde meydana gelirse erişime izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-297">Otherwise, if `M` is `private`, the access is permitted if it occurs within the type in which `M` is declared.</span></span>
*  <span data-ttu-id="98ef3-298">Aksi takdirde, tür veya üyeye erişilemez ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-298">Otherwise, the type or member is inaccessible, and a compile-time error occurs.</span></span>

<span data-ttu-id="98ef3-299">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-299">In the example</span></span>
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
<span data-ttu-id="98ef3-300">sınıflar ve Üyeler aşağıdaki erişilebilirlik etki alanlarına sahiptir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-300">the classes and members have the following accessibility domains:</span></span>

*  <span data-ttu-id="98ef3-301">`A` ve `A.X` erişilebilirlik etki alanı sınırsızdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-301">The accessibility domain of `A` and `A.X` is unlimited.</span></span>
*  <span data-ttu-id="98ef3-302">`A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`ve `B.C.Y` erişilebilirlik etki alanı, içeren programın program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-302">The accessibility domain of `A.Y`, `B`, `B.X`, `B.Y`, `B.C`, `B.C.X`, and `B.C.Y` is the program text of the containing program.</span></span>
*  <span data-ttu-id="98ef3-303">`A.Z` erişilebilirlik etki alanı `A`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-303">The accessibility domain of `A.Z` is the program text of `A`.</span></span>
*  <span data-ttu-id="98ef3-304">`B.Z` ve `B.D` erişilebilirlik etki alanı, `B.C` ve `B.D`program metni de dahil olmak üzere `B`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-304">The accessibility domain of `B.Z` and `B.D` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="98ef3-305">`B.C.Z` erişilebilirlik etki alanı `B.C`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-305">The accessibility domain of `B.C.Z` is the program text of `B.C`.</span></span>
*  <span data-ttu-id="98ef3-306">`B.D.X` ve `B.D.Y` erişilebilirlik etki alanı, `B.C` ve `B.D`program metni de dahil olmak üzere `B`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-306">The accessibility domain of `B.D.X` and `B.D.Y` is the program text of `B`, including the program text of `B.C` and `B.D`.</span></span>
*  <span data-ttu-id="98ef3-307">`B.D.Z` erişilebilirlik etki alanı `B.D`program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-307">The accessibility domain of `B.D.Z` is the program text of `B.D`.</span></span>

<span data-ttu-id="98ef3-308">Örnekte gösterildiği gibi, bir üyenin erişilebilirlik etki alanı, kapsayan türden hiçbir şekilde daha büyük değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-308">As the example illustrates, the accessibility domain of a member is never larger than that of a containing type.</span></span> <span data-ttu-id="98ef3-309">Örneğin, tüm `X` üyelerinin ortak olarak tanımlanmış erişilebilirliği olsa da, tüm `A.X`, kapsayan bir tür tarafından kısıtlanan erişilebilirlik etki alanlarına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-309">For example, even though all `X` members have public declared accessibility, all but `A.X` have accessibility domains that are constrained by a containing type.</span></span>

<span data-ttu-id="98ef3-310">[Üyeler](basic-concepts.md#members)bölümünde açıklandığı gibi, bir temel sınıfın, örnek oluşturucular, Yıkıcılar ve statik oluşturucular hariç tüm üyeleri türetilmiş türler tarafından devralınır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-310">As described in [Members](basic-concepts.md#members), all members of a base class, except for instance constructors, destructors and static constructors, are inherited by derived types.</span></span> <span data-ttu-id="98ef3-311">Bu, bir temel sınıfın hatta özel üyelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-311">This includes even private members of a base class.</span></span> <span data-ttu-id="98ef3-312">Ancak, özel bir üyenin erişilebilirlik etki alanı yalnızca üyenin bildirildiği türün program metnini içerir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-312">However, the accessibility domain of a private member includes only the program text of the type in which the member is declared.</span></span> <span data-ttu-id="98ef3-313">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-313">In the example</span></span>
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
<span data-ttu-id="98ef3-314">`B` sınıfı, `A` sınıfından özel üye `x` devralır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-314">the `B` class inherits the private member `x` from the `A` class.</span></span> <span data-ttu-id="98ef3-315">Üye özel olduğundan, yalnızca `A`*class_body* içinde erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-315">Because the member is private, it is only accessible within the *class_body* of `A`.</span></span> <span data-ttu-id="98ef3-316">Bu nedenle, `b.x` erişimi `A.F` yönteminde başarılı olur, ancak `B.F` yönteminde başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-316">Thus, the access to `b.x` succeeds in the `A.F` method, but fails in the `B.F` method.</span></span>

### <a name="protected-access-for-instance-members"></a><span data-ttu-id="98ef3-317">Örnek üyeleri için korumalı erişim</span><span class="sxs-lookup"><span data-stu-id="98ef3-317">Protected access for instance members</span></span>

<span data-ttu-id="98ef3-318">Bir `protected` örnek üyesine, bildirildiği sınıfın program metni dışında erişildiğinde ve `protected internal` örnek üyesine bildirildiği programın program metni dışında erişildiğinde, erişim, bildirildiği sınıftan türetilen bir sınıf bildiriminde gerçekleşmelidir, ancak bu, erişim, bildirildiği sınıftan türetilmiş bir sınıf bildirimi içinde yer almalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-318">When a `protected` instance member is accessed outside the program text of the class in which it is declared, and when a `protected internal` instance member is accessed outside the program text of the program in which it is declared, the access must take place within a class declaration that derives from the class in which it is declared.</span></span> <span data-ttu-id="98ef3-319">Ayrıca, bu türetilmiş sınıf türünün bir örneği veya ondan oluşturulan bir sınıf türü üzerinden gerçekleşmesi için erişim gerekir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-319">Furthermore, the access is required to take place through an instance of that derived class type or a class type constructed from it.</span></span> <span data-ttu-id="98ef3-320">Bu kısıtlama, bir türetilen sınıfın, Üyeler aynı temel sınıftan devralınsa bile, diğer türetilmiş sınıfların korunan üyelerine erişmesini önler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-320">This restriction prevents one derived class from accessing protected members of other derived classes, even when the members are inherited from the same base class.</span></span>

<span data-ttu-id="98ef3-321">`B` korunan bir örnek üyesi `M`bildiren bir temel sınıf olmasına izin verir ve `B`türetilen bir sınıf `D` izin verir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-321">Let `B` be a base class that declares a protected instance member `M`, and let `D` be a class that derives from `B`.</span></span> <span data-ttu-id="98ef3-322">`D`*class_body* içinde `M` erişimi aşağıdaki formlardan birini alabilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-322">Within the *class_body* of `D`, access to `M` can take one of the following forms:</span></span>

*  <span data-ttu-id="98ef3-323">Formun nitelenmemiş bir *type_name* veya *primary_expression* `M`.</span><span class="sxs-lookup"><span data-stu-id="98ef3-323">An unqualified *type_name* or *primary_expression* of the form `M`.</span></span>
*  <span data-ttu-id="98ef3-324">`E` türü `T` veya `T`sınıfından türetilmiş bir sınıf `E.M`, `T` sınıf türü `D`veya `D` oluşturulan bir sınıf türü *primary_expression* , form.</span><span class="sxs-lookup"><span data-stu-id="98ef3-324">A *primary_expression* of the form `E.M`, provided the type of `E` is `T` or a class derived from `T`, where `T` is the class type `D`, or a class type constructed from `D`</span></span>
*  <span data-ttu-id="98ef3-325">Form `base.M`*primary_expression* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-325">A *primary_expression* of the form `base.M`.</span></span>

<span data-ttu-id="98ef3-326">Bu erişim formlarına ek olarak, türetilmiş bir sınıf bir *constructor_initializer* ([Oluşturucu başlatıcıları](classes.md#constructor-initializers)) bir temel sınıfın korumalı örnek oluşturucusuna erişebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-326">In addition to these forms of access, a derived class can access a protected instance constructor of a base class in a *constructor_initializer* ([Constructor initializers](classes.md#constructor-initializers)).</span></span>

<span data-ttu-id="98ef3-327">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-327">In the example</span></span>
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
<span data-ttu-id="98ef3-328">`A`içinde, erişimin bir `A` örneği veya `A`sınıfından türetilmiş bir sınıf aracılığıyla gerçekleştiği durumlar nedeniyle, `A` ve `B`örnekleri aracılığıyla `x` erişmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-328">within `A`, it is possible to access `x` through instances of both `A` and `B`, since in either case the access takes place through an instance of `A` or a class derived from `A`.</span></span> <span data-ttu-id="98ef3-329">Ancak, `B`içinde, `A` `B`türetilmediğinden, `x` `A`bir örneği aracılığıyla erişmek mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-329">However, within `B`, it is not possible to access `x` through an instance of `A`, since `A` does not derive from `B`.</span></span>

<span data-ttu-id="98ef3-330">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-330">In the example</span></span>
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
<span data-ttu-id="98ef3-331">`x` için üç atama, hepsi genel türden oluşturulan sınıf türleri örnekleri aracılığıyla gerçekleştiğinden izin verilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-331">the three assignments to `x` are permitted because they all take place through instances of class types constructed from the generic type.</span></span>

### <a name="accessibility-constraints"></a><span data-ttu-id="98ef3-332">Erişilebilirlik kısıtlamaları</span><span class="sxs-lookup"><span data-stu-id="98ef3-332">Accessibility constraints</span></span>

<span data-ttu-id="98ef3-333">Dildeki çeşitli yapılar, C# bir türün en az bir üye veya başka bir tür ***olarak erişilebilir*** olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-333">Several constructs in the C# language require a type to be ***at least as accessible as*** a member or another type.</span></span> <span data-ttu-id="98ef3-334">Bir tür `T`, bir üye olarak en az erişilebilir olarak veya `T` erişilebilirlik etki alanı `M`erişilebilirlik etki alanının bir üst kümesi ise, `M` türü olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-334">A type `T` is said to be at least as accessible as a member or type `M` if the accessibility domain of `T` is a superset of the accessibility domain of `M`.</span></span> <span data-ttu-id="98ef3-335">Diğer bir deyişle, `T` `M` erişilebilen tüm bağlamlarda erişilebilir ise, `T` en az `M` olarak erişilebilir durumdadır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-335">In other words, `T` is at least as accessible as `M` if `T` is accessible in all contexts in which `M` is accessible.</span></span>

<span data-ttu-id="98ef3-336">Aşağıdaki erişilebilirlik kısıtlamaları vardır:</span><span class="sxs-lookup"><span data-stu-id="98ef3-336">The following accessibility constraints exist:</span></span>

*  <span data-ttu-id="98ef3-337">Bir sınıf türünün doğrudan temel sınıfı en azından sınıf türünün kendisi kadar erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-337">The direct base class of a class type must be at least as accessible as the class type itself.</span></span>
*  <span data-ttu-id="98ef3-338">Arabirim türünün açık temel arabirimleri en az arabirim türünün kendisi kadar erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-338">The explicit base interfaces of an interface type must be at least as accessible as the interface type itself.</span></span>
*  <span data-ttu-id="98ef3-339">Bir temsilci türünün dönüş türü ve parametre türleri en azından temsilci türünün kendisi kadar erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-339">The return type and parameter types of a delegate type must be at least as accessible as the delegate type itself.</span></span>
*  <span data-ttu-id="98ef3-340">Bir sabit türü en az sabit değer olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-340">The type of a constant must be at least as accessible as the constant itself.</span></span>
*  <span data-ttu-id="98ef3-341">Alanın türü en azından alanın kendisi kadar erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-341">The type of a field must be at least as accessible as the field itself.</span></span>
*  <span data-ttu-id="98ef3-342">Bir yöntemin dönüş türü ve parametre türleri en az yöntemin kendisi olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-342">The return type and parameter types of a method must be at least as accessible as the method itself.</span></span>
*  <span data-ttu-id="98ef3-343">Özelliğin türü en az özelliğin kendisi olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-343">The type of a property must be at least as accessible as the property itself.</span></span>
*  <span data-ttu-id="98ef3-344">Bir olayın türü en az olayın kendisi olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-344">The type of an event must be at least as accessible as the event itself.</span></span>
*  <span data-ttu-id="98ef3-345">Bir dizin oluşturucunun türü ve parametre türleri en azından dizin oluşturucunun kendisi olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-345">The type and parameter types of an indexer must be at least as accessible as the indexer itself.</span></span>
*  <span data-ttu-id="98ef3-346">Bir işlecin dönüş türü ve parametre türleri en az işlecin kendisi olarak erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-346">The return type and parameter types of an operator must be at least as accessible as the operator itself.</span></span>
*  <span data-ttu-id="98ef3-347">Örnek oluşturucusunun parametre türleri en azından örnek oluşturucusunun kendisi kadar erişilebilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-347">The parameter types of an instance constructor must be at least as accessible as the instance constructor itself.</span></span>

<span data-ttu-id="98ef3-348">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-348">In the example</span></span>
```csharp
class A {...}

public class B: A {...}
```
<span data-ttu-id="98ef3-349">`A` en azından `B`kadar erişilebilir olmadığı için `B` sınıfı derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-349">the `B` class results in a compile-time error because `A` is not at least as accessible as `B`.</span></span>

<span data-ttu-id="98ef3-350">Benzer şekilde, örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-350">Likewise, in the example</span></span>
```csharp
class A {...}

public class B
{
    A F() {...}

    internal A G() {...}

    public A H() {...}
}
```
<span data-ttu-id="98ef3-351">`B` `H` yöntemi, dönüş türü `A` en az Yöntem olarak erişilebilir olmadığından, derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-351">the `H` method in `B` results in a compile-time error because the return type `A` is not at least as accessible as the method.</span></span>

## <a name="signatures-and-overloading"></a><span data-ttu-id="98ef3-352">İmzalar ve aşırı yükleme</span><span class="sxs-lookup"><span data-stu-id="98ef3-352">Signatures and overloading</span></span>

<span data-ttu-id="98ef3-353">Yöntemler, örnek oluşturucular, Dizin oluşturucular ve işleçler, ***imzalarına***göre belirlenir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-353">Methods, instance constructors, indexers, and operators are characterized by their ***signatures***:</span></span>

*  <span data-ttu-id="98ef3-354">Bir yöntemin imzası, yöntemin adından, tür parametrelerinin sayısına ve her bir biçimsel parametrenin her birinin tür ve türü (değer, başvuru veya çıkış) soldan sağa doğru olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-354">The signature of a method consists of the name of the method, the number of type parameters and the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="98ef3-355">Bu amaçlar için, bir biçimsel parametre türünde oluşan yöntemin tür parametresi, kendi adı tarafından değil, yönteminin tür bağımsız değişkeni listesindeki sıra konumuna göre tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-355">For these purposes, any type parameter of the method that occurs in the type of a formal parameter is identified not by its name, but by its ordinal position in the type argument list of the method.</span></span> <span data-ttu-id="98ef3-356">Bir yöntemin imzası, dönüş türünü, en sağdaki parametre için belirtime `params` değiştiricisini veya isteğe bağlı tür parametresi kısıtlamalarını içermez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-356">The signature of a method specifically does not include the return type, the `params` modifier that may be specified for the right-most parameter, nor the optional type parameter constraints.</span></span>
*  <span data-ttu-id="98ef3-357">Bir örnek oluşturucusunun imzası, her bir biçimsel parametrenin türü ve türü (değer, başvuru veya çıkış), soldan sağa doğru olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-357">The signature of an instance constructor consists of the type and kind (value, reference, or output) of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="98ef3-358">Örnek oluşturucunun imzası, özellikle de en sağdaki parametre için belirtilemeyen `params` değiştiricisini içermez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-358">The signature of an instance constructor specifically does not include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="98ef3-359">Bir dizin oluşturucunun imzası, soldan sağa doğru olarak kabul edilen her bir biçimsel parametre türünden oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-359">The signature of an indexer consists of the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="98ef3-360">Bir dizin oluşturucunun imzası özellikle öğe türünü içermez veya en sağdaki parametre için belirtilemeyen `params` değiştiricisini içermez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-360">The signature of an indexer specifically does not include the element type, nor does it include the `params` modifier that may be specified for the right-most parameter.</span></span>
*  <span data-ttu-id="98ef3-361">Bir işlecin imzası, işlecin adından ve her bir biçimsel parametrelerinin, soldan sağa doğru olarak kabul edilen türünden oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-361">The signature of an operator consists of the name of the operator and the type of each of its formal parameters, considered in the order left to right.</span></span> <span data-ttu-id="98ef3-362">Bir işlecin imzası özellikle sonuç türünü içermez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-362">The signature of an operator specifically does not include the result type.</span></span>

<span data-ttu-id="98ef3-363">İmzalar, sınıflarda, yapılarda ve arabirimlerde üyelerin ***aşırı*** yüklenmesine yönelik etkinleştirme mekanizmasıdır:</span><span class="sxs-lookup"><span data-stu-id="98ef3-363">Signatures are the enabling mechanism for ***overloading*** of members in classes, structs, and interfaces:</span></span>

*  <span data-ttu-id="98ef3-364">Yöntemlerin aşırı yüklemesi, bir sınıf, yapı veya arabirimin aynı ada sahip birden çok yöntem bildirmesine izin verdiğinden, imzaları bu sınıf, yapı veya arabirim içinde benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-364">Overloading of methods permits a class, struct, or interface to declare multiple methods with the same name, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="98ef3-365">Örnek oluşturucuların aşırı yüklenmesi, bir sınıfın veya yapının birden çok örnek Oluşturucu tanımlamasına izin verdiğinden, imzaları bu sınıf veya yapı içinde benzersiz olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-365">Overloading of instance constructors permits a class or struct to declare multiple instance constructors, provided their signatures are unique within that class or struct.</span></span>
*  <span data-ttu-id="98ef3-366">Dizin oluşturucularının aşırı yüklemesi, bir sınıf, yapı veya arabirimin birden çok dizin oluşturma kullanılmasına izin verir, ancak imzaları bu sınıf, yapı veya arabirim içinde benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-366">Overloading of indexers permits a class, struct, or interface to declare multiple indexers, provided their signatures are unique within that class, struct, or interface.</span></span>
*  <span data-ttu-id="98ef3-367">İşleçlerin aşırı yüklemesi, bir sınıf veya yapının aynı ada sahip birden çok işleç tanımlamasına izin verdiğinden, imzaları bu sınıf veya yapı içinde benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-367">Overloading of operators permits a class or struct to declare multiple operators with the same name, provided their signatures are unique within that class or struct.</span></span>

<span data-ttu-id="98ef3-368">`out` ve `ref` parametre değiştiricileri bir imzanın parçası olarak kabul edilse de, tek bir türde belirtilen Üyeler yalnızca `ref` ve `out`tarafından imzaya göre farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-368">Although `out` and `ref` parameter modifiers are considered part of a signature, members declared in a single type cannot differ in signature solely by `ref` and `out`.</span></span> <span data-ttu-id="98ef3-369">İki üye, `out` değiştiricilere sahip her iki yöntemde de `ref` değiştiricilere değiştirilmişse aynı türde olan imzalar ile aynı türde bildirilirse, derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-369">A compile-time error occurs if two members are declared in the same type with signatures that would be the same if all parameters in both methods with `out` modifiers were changed to `ref` modifiers.</span></span> <span data-ttu-id="98ef3-370">İmza eşleştirmesinin diğer amaçları (örn. gizleme veya geçersiz kılma), `ref` ve `out` imzanın bir parçası olarak değerlendirilir ve birbirleriyle eşleşmez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-370">For other purposes of signature matching (e.g., hiding or overriding), `ref` and `out` are considered part of the signature and do not match each other.</span></span> <span data-ttu-id="98ef3-371">(Bu kısıtlama, programların yalnızca C# `ref` ve `out`farklı yöntemleri tanımlamak için bir yol sağlamayan ortak dil ALTYAPıSıNDA (CLI) çalışmaya kolayca çevrilmesine izin versağlamaktır.)</span><span class="sxs-lookup"><span data-stu-id="98ef3-371">(This restriction is to allow C#  programs to be easily translated to run on the Common Language Infrastructure (CLI), which does not provide a way to define methods that differ solely in `ref` and `out`.)</span></span>

<span data-ttu-id="98ef3-372">İmzaların amaçları doğrultusunda `object` ve `dynamic` türleri aynı kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-372">For the purposes of signatures, the types `object` and `dynamic` are considered the same.</span></span> <span data-ttu-id="98ef3-373">Bu nedenle, tek bir türde tanımlanan Üyeler yalnızca `object` ve `dynamic`tarafından İmzada farklı değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-373">Members declared in a single type can therefore not differ in signature solely by `object` and `dynamic`.</span></span>

<span data-ttu-id="98ef3-374">Aşağıdaki örnek, imzaları ile birlikte aşırı yüklenmiş yöntem bildirimlerinin bir kümesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-374">The following example shows a set of overloaded method declarations along with their signatures.</span></span>
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

<span data-ttu-id="98ef3-375">Tüm `ref` ve `out` parametre değiştiricilerin ([Yöntem parametreleri](classes.md#method-parameters)) bir imzanın parçası olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef3-375">Note that any `ref` and `out` parameter modifiers ([Method parameters](classes.md#method-parameters)) are part of a signature.</span></span> <span data-ttu-id="98ef3-376">Bu nedenle, `F(int)` ve `F(ref int)` benzersiz imzalardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-376">Thus, `F(int)` and `F(ref int)` are unique signatures.</span></span> <span data-ttu-id="98ef3-377">Ancak, `F(ref int)` ve `F(out int)`, imzaları yalnızca `ref` ve `out`farklı olduğundan aynı arabirim içinde bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-377">However, `F(ref int)` and `F(out int)` cannot be declared within the same interface because their signatures differ solely by `ref` and `out`.</span></span> <span data-ttu-id="98ef3-378">Ayrıca, dönüş türü ve `params` değiştiricisinin bir imzanın parçası olmadığına ve bu nedenle yalnızca dönüş türüne göre veya `params` değiştiricinin dahil edilmesi veya dışlamasına göre aşırı yüklenmesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-378">Also, note that the return type and the `params` modifier are not part of a signature, so it is not possible to overload solely based on return type or on the inclusion or exclusion of the `params` modifier.</span></span> <span data-ttu-id="98ef3-379">Bu nedenle, yukarıda tanımlanan `F(int)` ve `F(params string[])` yöntemlerinin bildirimleri derleme zamanı hatası ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-379">As such, the declarations of the methods `F(int)` and `F(params string[])` identified above result in a compile-time error.</span></span>

## <a name="scopes"></a><span data-ttu-id="98ef3-380">Kapsamları</span><span class="sxs-lookup"><span data-stu-id="98ef3-380">Scopes</span></span>

<span data-ttu-id="98ef3-381">Bir adın ***kapsamı*** , adı nitelemeden ad tarafından tanımlanan varlığa başvuruda bulunmak mümkün olduğu program metninin bölgesidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-381">The ***scope*** of a name is the region of program text within which it is possible to refer to the entity declared by the name without qualification of the name.</span></span> <span data-ttu-id="98ef3-382">Kapsamlar ***iç içe***olabilir ve bir iç kapsam bir dış kapsamdan bir adın anlamını yeniden bildirebilir (ancak, iç içe geçmiş bir blok içindeki [bildirimlerin](basic-concepts.md#declarations) uyguladığı kısıtlamayı kaldırır, kapsayan bir blok içinde yerel bir değişkenle aynı ada sahip yerel bir değişken bildirmek mümkün değildir).</span><span class="sxs-lookup"><span data-stu-id="98ef3-382">Scopes can be ***nested***, and an inner scope may redeclare the meaning of a name from an outer scope (this does not, however, remove the restriction imposed by [Declarations](basic-concepts.md#declarations) that within a nested block it is not possible to declare a local variable with the same name as a local variable in an enclosing block).</span></span> <span data-ttu-id="98ef3-383">Daha sonra dış kapsamdaki ad, iç kapsamın kapsadığı program metni bölgesinde ***gizli*** olarak belirlenir ve dış ada erişim yalnızca adı niteleyerek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-383">The name from the outer scope is then said to be ***hidden*** in the region of program text covered by the inner scope, and access to the outer name is only possible by qualifying the name.</span></span>

*  <span data-ttu-id="98ef3-384">Kapsayan *namespace_declaration* olmayan bir *namespace_member_declaration* ([ad alanı üyeleri](namespaces.md#namespace-members)) tarafından belirtilen bir ad alanı üyesinin kapsamı, tüm program metindir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-384">The scope of a namespace member declared by a *namespace_member_declaration* ([Namespace members](namespaces.md#namespace-members)) with no enclosing *namespace_declaration* is the entire program text.</span></span>
*  <span data-ttu-id="98ef3-385">Tam adı `N` olan bir *namespace_declaration* içinde *namespace_member_declaration* tarafından belirtilen bir ad alanı üyesinin kapsamı, tam adı namespace_declaration olan veya `N` ile başlayan her *`N`* , ardından bir nokta gelen *namespace_body* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-385">The scope of a namespace member declared by a *namespace_member_declaration* within a *namespace_declaration* whose fully qualified name is `N` is the *namespace_body* of every *namespace_declaration* whose fully qualified name is `N` or starts with `N`, followed by a period.</span></span>
*  <span data-ttu-id="98ef3-386">Bir *extern_alias_directive* tarafından tanımlanan ad kapsamı, derleme birimi veya ad alanı gövdesinin hemen içerdiği *using_directive*s, *global_attributes* ve *namespace_member_declaration*göre genişletilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-386">The scope of name defined by an *extern_alias_directive* extends over the *using_directive*s, *global_attributes* and *namespace_member_declaration*s of its immediately containing compilation unit or namespace body.</span></span> <span data-ttu-id="98ef3-387">*Extern_alias_directive* , temel alınan bildirim alanına yeni üye katkıda bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="98ef3-387">An *extern_alias_directive* does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="98ef3-388">Diğer bir deyişle, *extern_alias_directive* geçişli değildir, ancak bunun yerine yalnızca, oluştuğu derleme birimini veya ad alanı gövdesini etkiler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-388">In other words, an *extern_alias_directive* is not transitive, but, rather, affects only the compilation unit or namespace body in which it occurs.</span></span>
*  <span data-ttu-id="98ef3-389">*Using_directive* tarafından tanımlanan veya içeri aktarılan bir adın kapsamı ([yönergeler kullanılarak](namespaces.md#using-directives)), *using_directive* gerçekleştiği *compilation_unit* veya *namespace_body* *namespace_member_declaration*üzerinde uzanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-389">The scope of a name defined or imported by a *using_directive* ([Using directives](namespaces.md#using-directives)) extends over the *namespace_member_declaration*s of the *compilation_unit* or *namespace_body* in which the *using_directive* occurs.</span></span> <span data-ttu-id="98ef3-390">*Using_directive* , belirli bir *compilation_unit* veya *namespace_body*içinde kullanılabilir bir veya daha fazla ad alanı, tür veya üye adı oluşturabilir, ancak temel alınan bildirim alanına yeni üye katkıda bulunmaz.</span><span class="sxs-lookup"><span data-stu-id="98ef3-390">A *using_directive* may make zero or more namespace, type or member names available within a particular *compilation_unit* or *namespace_body*, but does not contribute any new members to the underlying declaration space.</span></span> <span data-ttu-id="98ef3-391">Diğer bir deyişle, *using_directive* geçişli değildir ancak bunun yerine yalnızca *compilation_unit* veya *namespace_body* etkiler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-391">In other words, a *using_directive* is not transitive but rather affects only the *compilation_unit* or *namespace_body* in which it occurs.</span></span>
*  <span data-ttu-id="98ef3-392">Bir *class_declaration* ([sınıf bildirimleri](classes.md#class-declarations)) *type_parameter_list* tarafından belirtilen bir tür parametresinin kapsamı, *class_body* *class_base*, *type_parameter_constraints_clause*s ve *class_declaration* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-392">The scope of a type parameter declared by a *type_parameter_list* on a *class_declaration* ([Class declarations](classes.md#class-declarations)) is the *class_base*, *type_parameter_constraints_clause*s, and *class_body* of that *class_declaration*.</span></span>
*  <span data-ttu-id="98ef3-393">Bir *struct_declaration* ([struct bildirimleri](structs.md#struct-declarations)) *type_parameter_list* tarafından belirtilen bir tür parametresinin kapsamı, bu *struct_body* *struct_interfaces*, *type_parameter_constraints_clause*s ve *struct_declaration* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-393">The scope of a type parameter declared by a *type_parameter_list* on a *struct_declaration* ([Struct declarations](structs.md#struct-declarations)) is the *struct_interfaces*, *type_parameter_constraints_clause*s, and *struct_body* of that *struct_declaration*.</span></span>
*  <span data-ttu-id="98ef3-394">Bir *interface_declaration* ([arabirim bildirimleri](interfaces.md#interface-declarations)) *type_parameter_list* tarafından belirtilen bir tür parametresinin kapsamı, *interface_body* *interface_base*, *type_parameter_constraints_clause*s ve *interface_declaration* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-394">The scope of a type parameter declared by a *type_parameter_list* on an *interface_declaration* ([Interface declarations](interfaces.md#interface-declarations)) is the *interface_base*, *type_parameter_constraints_clause*s, and *interface_body* of that *interface_declaration*.</span></span>
*  <span data-ttu-id="98ef3-395">Bir *delegate_declaration* ([temsilci bildirimleri](delegates.md#delegate-declarations)) *type_parameter_list* tarafından belirtilen bir tür parametresinin kapsamı, *type_parameter_constraints_clause* *return_type*, *formal_parameter_list*ve *delegate_declaration*.</span><span class="sxs-lookup"><span data-stu-id="98ef3-395">The scope of a type parameter declared by a *type_parameter_list* on a *delegate_declaration* ([Delegate declarations](delegates.md#delegate-declarations)) is the *return_type*, *formal_parameter_list*, and *type_parameter_constraints_clause*s of that *delegate_declaration*.</span></span>
*  <span data-ttu-id="98ef3-396">Bir *class_member_declaration* ([sınıf gövdesi](classes.md#class-body)) tarafından belirtilen üyenin kapsamı, bildirimin gerçekleştiği *class_body* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-396">The scope of a member declared by a *class_member_declaration* ([Class body](classes.md#class-body)) is the *class_body* in which the declaration occurs.</span></span> <span data-ttu-id="98ef3-397">Ayrıca, bir sınıf üyesinin kapsamı, üyenin erişilebilirlik etki alanına ([erişilebilirlik etki alanları](basic-concepts.md#accessibility-domains)) dahil edilen türetilmiş sınıfların *class_body* genişletir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-397">In addition, the scope of a class member extends to the *class_body* of those derived classes that are included in the accessibility domain ([Accessibility domains](basic-concepts.md#accessibility-domains)) of the member.</span></span>
*  <span data-ttu-id="98ef3-398">Bir *struct_member_declaration* ([Yapı üyeleri](structs.md#struct-members)) tarafından belirtilen bir üyenin kapsamı, bildirimin gerçekleştiği *struct_body* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-398">The scope of a member declared by a *struct_member_declaration* ([Struct members](structs.md#struct-members)) is the *struct_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="98ef3-399">Bir *enum_member_declaration* ([enum üyeleri](enums.md#enum-members)) tarafından belirtilen bir üyenin kapsamı, bildirimin gerçekleştiği *enum_body* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-399">The scope of a member declared by an *enum_member_declaration*  ([Enum members](enums.md#enum-members)) is the *enum_body* in which the declaration occurs.</span></span>
*  <span data-ttu-id="98ef3-400">Bir *method_declaration* ([metotlar](classes.md#methods)) içinde belirtilen parametrenin kapsamı, bu *method_declaration* *method_body* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-400">The scope of a parameter declared in a *method_declaration* ([Methods](classes.md#methods)) is the *method_body* of that *method_declaration*.</span></span>
*  <span data-ttu-id="98ef3-401">Bir *indexer_declaration* ([Dizin oluşturucular](classes.md#indexers)) içinde belirtilen parametrenin kapsamı, bu *indexer_declaration* *accessor_declarations* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-401">The scope of a parameter declared in an *indexer_declaration* ([Indexers](classes.md#indexers)) is the *accessor_declarations* of that *indexer_declaration*.</span></span>
*  <span data-ttu-id="98ef3-402">Bir *operator_declaration* ([işleçler](classes.md#operators)) içinde belirtilen bir parametre kapsamı, bu *operator_declaration* *bloğudur* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-402">The scope of a parameter declared in an *operator_declaration* ([Operators](classes.md#operators)) is the *block* of that *operator_declaration*.</span></span>
*  <span data-ttu-id="98ef3-403">Bir *constructor_declaration* ([örnek oluşturucular](classes.md#instance-constructors)) içinde belirtilen parametrenin kapsamı, bu *constructor_declaration* *constructor_initializer* ve *bloğudur* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-403">The scope of a parameter declared in a *constructor_declaration* ([Instance constructors](classes.md#instance-constructors)) is the *constructor_initializer* and *block* of that *constructor_declaration*.</span></span>
*  <span data-ttu-id="98ef3-404">Bir *lambda_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) içinde belirtilen bir parametre kapsamı, bu *lambda_expression* *anonymous_function_body*</span><span class="sxs-lookup"><span data-stu-id="98ef3-404">The scope of a parameter declared in a *lambda_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *anonymous_function_body* of that *lambda_expression*</span></span>
*  <span data-ttu-id="98ef3-405">Bir *anonymous_method_expression* ([anonim işlev ifadeleri](expressions.md#anonymous-function-expressions)) içinde belirtilen bir parametre kapsamı, bu *anonymous_method_expression* *bloğudur* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-405">The scope of a parameter declared in an *anonymous_method_expression* ([Anonymous function expressions](expressions.md#anonymous-function-expressions)) is the *block* of that *anonymous_method_expression*.</span></span>
*  <span data-ttu-id="98ef3-406">*Labeled_statement* ([etiketli deyimler](statements.md#labeled-statements)) içinde belirtilen etiketin kapsamı, bildirimin gerçekleştiği *bloğudur* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-406">The scope of a label declared in a *labeled_statement* ([Labeled statements](statements.md#labeled-statements)) is the *block* in which the declaration occurs.</span></span>
*  <span data-ttu-id="98ef3-407">Bir *local_variable_declaration* ([yerel değişken bildirimleri](statements.md#local-variable-declarations)) içinde belirtilen bir yerel değişkenin kapsamı, bildirimin gerçekleştiği bloğudur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-407">The scope of a local variable declared in a *local_variable_declaration* ([Local variable declarations](statements.md#local-variable-declarations)) is the block in which the declaration occurs.</span></span>
*  <span data-ttu-id="98ef3-408">Bir `switch` deyimin ([switch ifadesinin](statements.md#the-switch-statement)) *switch_block* bildirildiği bir yerel değişkenin kapsamı *switch_block*.</span><span class="sxs-lookup"><span data-stu-id="98ef3-408">The scope of a local variable declared in a *switch_block* of a `switch` statement ([The switch statement](statements.md#the-switch-statement)) is the *switch_block*.</span></span>
*  <span data-ttu-id="98ef3-409">Bir `for` deyimin ([for ifadesinin](statements.md#the-for-statement)) *for_initializer* içinde belirtilen yerel bir değişkenin kapsamı, for_iterator deyimin *for_initializer*, *for_condition*, *`for`* ve içerilen *deyimdir* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-409">The scope of a local variable declared in a *for_initializer* of a `for` statement ([The for statement](statements.md#the-for-statement)) is the *for_initializer*, the *for_condition*, the *for_iterator*, and the contained *statement* of the `for` statement.</span></span>
*  <span data-ttu-id="98ef3-410">Bir *local_constant_declaration* ([yerel sabit bildirimler](statements.md#local-constant-declarations)) içinde belirtilen yerel bir sabit kapsam, bildirimin gerçekleştiği bloğudur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-410">The scope of a local constant declared in a *local_constant_declaration* ([Local constant declarations](statements.md#local-constant-declarations)) is the block in which the declaration occurs.</span></span> <span data-ttu-id="98ef3-411">Bu, *constant_declarator*önündeki metinsel konumdaki yerel bir sabite başvuruda bulunmak için bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-411">It is a compile-time error to refer to a local constant in a textual position that precedes its *constant_declarator*.</span></span>
*  <span data-ttu-id="98ef3-412">*Foreach_statement*, *using_statement*, *lock_statement* veya *query_expression* bir parçası olarak belirtilen bir değişkenin kapsamı, verilen yapının genişlemesiyle belirlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-412">The scope of a variable declared as part of a *foreach_statement*, *using_statement*, *lock_statement* or *query_expression* is determined by the expansion of the given construct.</span></span>

<span data-ttu-id="98ef3-413">Bir ad alanı, sınıf, yapı veya numaralandırma üyesi kapsamında, üyeye üye bildiriminden önce gelen metinsel bir konumda başvurmak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-413">Within the scope of a namespace, class, struct, or enumeration member it is possible to refer to the member in a textual position that precedes the declaration of the member.</span></span> <span data-ttu-id="98ef3-414">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98ef3-414">For example</span></span>
```csharp
class A
{
    void F() {
        i = 1;
    }

    int i = 0;
}
```
<span data-ttu-id="98ef3-415">Burada, `F` bildirilmeden önce `i` başvuruda bulunmak için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-415">Here, it is valid for `F` to refer to `i` before it is declared.</span></span>

<span data-ttu-id="98ef3-416">Yerel değişken kapsamında, yerel değişkenin *local_variable_declarator* önündeki bir metinsel konumdaki yerel değişkene başvurabileceğiniz bir derleme zamanı hatasıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-416">Within the scope of a local variable, it is a compile-time error to refer to the local variable in a textual position that precedes the *local_variable_declarator* of the local variable.</span></span> <span data-ttu-id="98ef3-417">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="98ef3-417">For example</span></span>
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

<span data-ttu-id="98ef3-418">Yukarıdaki `F` yöntemde, özel olarak `i` ilk atama, dış kapsamda belirtilen alana başvurmaz.</span><span class="sxs-lookup"><span data-stu-id="98ef3-418">In the `F` method above, the first assignment to `i` specifically does not refer to the field declared in the outer scope.</span></span> <span data-ttu-id="98ef3-419">Bunun yerine, yerel değişkene başvurur ve değişkenin bildiriminden önce metin içeriğini eklemek bir derleme zamanı hatasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-419">Rather, it refers to the local variable and it results in a compile-time error because it textually precedes the declaration of the variable.</span></span> <span data-ttu-id="98ef3-420">`G` yönteminde, kullanım *local_variable_declarator*önünde olmadığından `j` bildirimi için başlatıcıdaki `j` kullanımı geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-420">In the `G` method, the use of `j` in the initializer for the declaration of `j` is valid because the use does not precede the *local_variable_declarator*.</span></span> <span data-ttu-id="98ef3-421">`H` yönteminde, sonraki bir *local_variable_declarator* doğru bir şekilde, aynı *local_variable_declaration*daha önceki *local_variable_declarator* tanımlanmış bir yerel değişkene başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-421">In the `H` method, a subsequent *local_variable_declarator* correctly refers to a local variable declared in an earlier *local_variable_declarator* within the same *local_variable_declaration*.</span></span>

<span data-ttu-id="98ef3-422">Yerel değişkenlerin kapsam kuralları, bir ifade bağlamında kullanılan bir adın anlamını her zaman bir blok içinde aynı olduğundan emin olmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-422">The scoping rules for local variables are designed to guarantee that the meaning of a name used in an expression context is always the same within a block.</span></span> <span data-ttu-id="98ef3-423">Yerel bir değişkenin kapsamı yalnızca bildiriminden bloğunun sonuna genişletirse, yukarıdaki örnekte, ilk atama örnek değişkenine atanır ve ikinci atama yerel değişkene atanır, büyük olasılıkla şu şekilde önde gelen bloğun deyimlerinin daha sonra yeniden düzenlenme durumunda derleme zamanı hataları.</span><span class="sxs-lookup"><span data-stu-id="98ef3-423">If the scope of a local variable were to extend only from its declaration to the end of the block, then in the example above, the first assignment would assign to the instance variable and the second assignment would assign to the local variable, possibly leading to compile-time errors if the statements of the block were later to be rearranged.</span></span>

<span data-ttu-id="98ef3-424">Bir blok içindeki bir adın anlamı, adının kullanıldığı bağlama göre farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-424">The meaning of a name within a block may differ based on the context in which the name is used.</span></span> <span data-ttu-id="98ef3-425">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-425">In the example</span></span>
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
<span data-ttu-id="98ef3-426">`A` adı bir ifade bağlamında, `A` yerel değişkene ve bir tür bağlamına başvuracak `A`sınıfa başvuracak şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-426">the name `A` is used in an expression context to refer to the local variable `A` and in a type context to refer to the class `A`.</span></span>

### <a name="name-hiding"></a><span data-ttu-id="98ef3-427">Ad gizleme</span><span class="sxs-lookup"><span data-stu-id="98ef3-427">Name hiding</span></span>

<span data-ttu-id="98ef3-428">Bir varlığın kapsamı genellikle varlığın bildirim alanından daha fazla program metnini kapsar.</span><span class="sxs-lookup"><span data-stu-id="98ef3-428">The scope of an entity typically encompasses more program text than the declaration space of the entity.</span></span> <span data-ttu-id="98ef3-429">Özellikle, bir varlığın kapsamı, aynı ada sahip varlıkları içeren yeni bildirim alanları sunan bildirimler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-429">In particular, the scope of an entity may include declarations that introduce new declaration spaces containing entities of the same name.</span></span> <span data-ttu-id="98ef3-430">Bu tür bildirimler orijinal varlığın ***gizli***hale gelmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-430">Such declarations cause the original entity to become ***hidden***.</span></span> <span data-ttu-id="98ef3-431">Buna karşılık, bir varlık gizlenmediği zaman ***görünür*** olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-431">Conversely, an entity is said to be ***visible*** when it is not hidden.</span></span>

<span data-ttu-id="98ef3-432">Ad gizleme kapsamları, iç içe geçme yoluyla çakıştığında ve kapsamlar devralma yoluyla çakıştığında oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-432">Name hiding occurs when scopes overlap through nesting and when scopes overlap through inheritance.</span></span> <span data-ttu-id="98ef3-433">İki tür gizleme özelliği aşağıdaki bölümlerde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-433">The characteristics of the two types of hiding are described in the following sections.</span></span>

#### <a name="hiding-through-nesting"></a><span data-ttu-id="98ef3-434">İç içe geçme yoluyla gizleme</span><span class="sxs-lookup"><span data-stu-id="98ef3-434">Hiding through nesting</span></span>

<span data-ttu-id="98ef3-435">İç içe geçme yoluyla ad alanları veya türler, sınıfların veya yapıların içinde iç içe geçme, parametre ve yerel değişken bildirimlerinin sonucu olarak, ad alanları içinde iç içe geçme işleminin sonucu olarak ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-435">Name hiding through nesting can occur as a result of nesting namespaces or types within namespaces, as a result of nesting types within classes or structs, and as a result of parameter and local variable declarations.</span></span>

<span data-ttu-id="98ef3-436">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-436">In the example</span></span>
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
<span data-ttu-id="98ef3-437">`F` yönteminde, örnek değişkeni `i` yerel değişken `i`gizlenir, ancak `G` yöntemi içinde `i` hala örnek değişkenine başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-437">within the `F` method, the instance variable `i` is hidden by the local variable `i`, but within the `G` method, `i` still refers to the instance variable.</span></span>

<span data-ttu-id="98ef3-438">Bir iç kapsamdaki bir ad, dış kapsamdaki bir adı gizliyor, bu ada ait tüm aşırı yüklenmiş oluşumları gizler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-438">When a name in an inner scope hides a name in an outer scope, it hides all overloaded occurrences of that name.</span></span> <span data-ttu-id="98ef3-439">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-439">In the example</span></span>
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
<span data-ttu-id="98ef3-440">çağrı `F(1)`, `F` tüm dış oluşumları iç bildirim tarafından gizlendiğinden `Inner` belirtilen `F` çağırır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-440">the call `F(1)` invokes the `F` declared in `Inner` because all outer occurrences of `F` are hidden by the inner declaration.</span></span> <span data-ttu-id="98ef3-441">Aynı nedenden dolayı çağrı `F("Hello")` bir derleme zamanı hatasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-441">For the same reason, the call `F("Hello")` results in a compile-time error.</span></span>

#### <a name="hiding-through-inheritance"></a><span data-ttu-id="98ef3-442">Devralma yoluyla gizleme</span><span class="sxs-lookup"><span data-stu-id="98ef3-442">Hiding through inheritance</span></span>

<span data-ttu-id="98ef3-443">Devralma yoluyla ad gizleme, sınıflar veya yapılar temel sınıflardan devralınan adları yeniden bildirdikleri zaman oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-443">Name hiding through inheritance occurs when classes or structs redeclare names that were inherited from base classes.</span></span> <span data-ttu-id="98ef3-444">Bu tür bir ad gizleme aşağıdaki biçimlerden birini alır:</span><span class="sxs-lookup"><span data-stu-id="98ef3-444">This type of name hiding takes one of the following forms:</span></span>

*  <span data-ttu-id="98ef3-445">Bir sınıf veya yapı içinde tanıtılan bir sabit, alan, özellik, olay veya tür, aynı ada sahip tüm temel sınıf üyelerini gizler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-445">A constant, field, property, event, or type introduced in a class or struct hides all base class members with the same name.</span></span>
*  <span data-ttu-id="98ef3-446">Bir sınıf veya yapı içinde tanıtılan bir yöntem, aynı ada sahip tüm yöntem olmayan taban sınıf üyelerini ve aynı imzaya sahip tüm temel sınıf yöntemlerini (Yöntem adı ve parametre sayısı, değiştiriciler ve türler) gizler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-446">A method introduced in a class or struct hides all non-method base class members with the same name, and all base class methods with the same signature (method name and parameter count, modifiers, and types).</span></span>
*  <span data-ttu-id="98ef3-447">Bir sınıf veya yapı içinde tanıtılan bir Dizin Oluşturucu, aynı imzaya sahip tüm temel sınıf dizin oluşturucularının (parametre sayısı ve türleri) tümünü gizler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-447">An indexer introduced in a class or struct hides all base class indexers with the same signature (parameter count and types).</span></span>

<span data-ttu-id="98ef3-448">İşleç bildirimlerini ([işleçler](classes.md#operators)) yöneten kurallar, türetilmiş bir sınıfın bir taban sınıftaki işleçle aynı imzaya sahip bir işleç tanımlamasına olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="98ef3-448">The rules governing operator declarations ([Operators](classes.md#operators)) make it impossible for a derived class to declare an operator with the same signature as an operator in a base class.</span></span> <span data-ttu-id="98ef3-449">Bu nedenle, işleçler hiçbir şekilde bir tane gizlemez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-449">Thus, operators never hide one another.</span></span>

<span data-ttu-id="98ef3-450">Bir dış kapsamdan bir adı gizlemenin aksine, devralınan bir kapsamdaki erişilebilir bir adı gizlemek bir uyarının raporlanmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-450">Contrary to hiding a name from an outer scope, hiding an accessible name from an inherited scope causes a warning to be reported.</span></span> <span data-ttu-id="98ef3-451">örnekte</span><span class="sxs-lookup"><span data-stu-id="98ef3-451">In the example</span></span>
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
<span data-ttu-id="98ef3-452">`Derived` `F` bildirimi, bir uyarının bildirilmesine neden olur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-452">the declaration of `F` in `Derived` causes a warning to be reported.</span></span> <span data-ttu-id="98ef3-453">Devralınan bir adın gizlenmesi, temel sınıfların ayrı bir evrimden daha fazla gelişmesinden dolayı özellikle bir hata değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-453">Hiding an inherited name is specifically not an error, since that would preclude separate evolution of base classes.</span></span> <span data-ttu-id="98ef3-454">Örneğin, daha sonraki bir `Base` sürümü sınıfının önceki bir sürümünde mevcut olmayan bir `F` yöntemi sunduğundan, yukarıdaki durum yaklaşık olarak gelmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-454">For example, the above situation might have come about because a later version of `Base` introduced an `F` method that wasn't present in an earlier version of the class.</span></span> <span data-ttu-id="98ef3-455">Yukarıdaki durum bir hata olduğundan, ayrı sürümlü Sınıf kitaplığındaki temel sınıfta yapılan herhangi bir değişiklik, türetilmiş sınıfların geçersiz olmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-455">Had the above situation been an error, then any change made to a base class in a separately versioned class library could potentially cause derived classes to become invalid.</span></span>

<span data-ttu-id="98ef3-456">Devralınan bir adı gizlemenin neden olduğu uyarı `new` değiştiricinin kullanılmasıyla ortadan kaldırılabilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-456">The warning caused by hiding an inherited name can be eliminated through use of the `new` modifier:</span></span>
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

<span data-ttu-id="98ef3-457">`new` değiştirici, `Derived` içindeki `F` "yeni" olduğunu ve aslında devralınmış üyeyi gizlemek için tasarlanan olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-457">The `new` modifier indicates that the `F` in `Derived` is "new", and that it is indeed intended to hide the inherited member.</span></span>

<span data-ttu-id="98ef3-458">Yeni bir üyenin bildirimi, devralınan bir üyeyi yalnızca yeni üyenin kapsamı içinde gizler.</span><span class="sxs-lookup"><span data-stu-id="98ef3-458">A declaration of a new member hides an inherited member only within the scope of the new member.</span></span>

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

<span data-ttu-id="98ef3-459">Yukarıdaki örnekte, `Derived` `F` bildirimi `Base`devralınmış `F` gizliyor, ancak `F` yeni `Derived` özel erişimi olduğundan, kapsamı `MoreDerived`olarak genişlemez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-459">In the example above, the declaration of `F` in `Derived` hides the `F` that was inherited from `Base`, but since the new `F` in `Derived` has private access, its scope does not extend to `MoreDerived`.</span></span> <span data-ttu-id="98ef3-460">Bu nedenle, `MoreDerived.G` `F()` çağrısı geçerlidir ve `Base.F`çağırır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-460">Thus, the call `F()` in `MoreDerived.G` is valid and will invoke `Base.F`.</span></span>

## <a name="namespace-and-type-names"></a><span data-ttu-id="98ef3-461">Ad alanı ve tür adları</span><span class="sxs-lookup"><span data-stu-id="98ef3-461">Namespace and type names</span></span>

<span data-ttu-id="98ef3-462">Bir C# programdaki birkaç bağlam, bir *namespace_name* veya *type_name* belirtilmesini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-462">Several contexts in a C# program require a *namespace_name* or a *type_name* to be specified.</span></span>

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

<span data-ttu-id="98ef3-463">*Namespace_name* , bir ad alanına başvuran bir *namespace_or_type_name* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-463">A *namespace_name* is a *namespace_or_type_name* that refers to a namespace.</span></span> <span data-ttu-id="98ef3-464">Aşağıda açıklandığı gibi çözüm aşağıdaki şekilde, bir *namespace_name* *namespace_or_type_name* bir ad alanına başvurmalıdır veya bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-464">Following resolution as described below, the *namespace_or_type_name* of a *namespace_name* must refer to a namespace, or otherwise a compile-time error occurs.</span></span> <span data-ttu-id="98ef3-465">Hiçbir tür bağımsız değişkeni ([tür bağımsız](types.md#type-arguments)değişkeni) bir *namespace_name* bulunabilir (yalnızca türler tür bağımsız değişkenlerine sahip olabilir).</span><span class="sxs-lookup"><span data-stu-id="98ef3-465">No type arguments ([Type arguments](types.md#type-arguments)) can be present in a *namespace_name* (only types can have type arguments).</span></span>

<span data-ttu-id="98ef3-466">*Type_name* , bir türe başvuran bir *namespace_or_type_name* .</span><span class="sxs-lookup"><span data-stu-id="98ef3-466">A *type_name* is a *namespace_or_type_name* that refers to a type.</span></span> <span data-ttu-id="98ef3-467">Aşağıda açıklandığı gibi, aşağıda açıklandığı gibi, bir *type_name* *namespace_or_type_name* bir türe başvurmalıdır veya bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-467">Following resolution as described below, the *namespace_or_type_name* of a *type_name* must refer to a type, or otherwise a compile-time error occurs.</span></span>

<span data-ttu-id="98ef3-468">*Namespace_or_type_name* nitelikli bir diğer ad üyesiyse, anlamı [ad alanı diğer ad niteleyicileri](namespaces.md#namespace-alias-qualifiers)bölümünde açıklanacaktır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-468">If the *namespace_or_type_name* is a qualified-alias-member its meaning is as described in [Namespace alias qualifiers](namespaces.md#namespace-alias-qualifiers).</span></span> <span data-ttu-id="98ef3-469">Aksi takdirde, *namespace_or_type_name* dört formdan birine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-469">Otherwise, a *namespace_or_type_name* has one of four forms:</span></span>

*  `I`
*  `I<A1, ..., Ak>`
*  `N.I`
*  `N.I<A1, ..., Ak>`

<span data-ttu-id="98ef3-470">`I` tek bir tanımlayıcıdır, `N` *namespace_or_type_name* ve `<A1, ..., Ak>` isteğe bağlı bir *type_argument_list*.</span><span class="sxs-lookup"><span data-stu-id="98ef3-470">where `I` is a single identifier, `N` is a *namespace_or_type_name* and `<A1, ..., Ak>` is an optional *type_argument_list*.</span></span> <span data-ttu-id="98ef3-471">*Type_argument_list* belirtilmediğinde, sıfır `k` göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="98ef3-471">When no *type_argument_list* is specified, consider `k` to be zero.</span></span>

<span data-ttu-id="98ef3-472">Bir *namespace_or_type_name* anlamı aşağıdaki şekilde belirlenir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-472">The meaning of a *namespace_or_type_name* is determined as follows:</span></span>

*   <span data-ttu-id="98ef3-473">*Namespace_or_type_name* , `I` formundadır `I<A1, ..., Ak>`:</span><span class="sxs-lookup"><span data-stu-id="98ef3-473">If the *namespace_or_type_name* is of the form `I` or of the form `I<A1, ..., Ak>`:</span></span>
    * <span data-ttu-id="98ef3-474">`K` sıfırsa ve *namespace_or_type_name* bir genel yöntem bildiriminde ([Yöntemler](classes.md#methods)) görüntüleniyorsa ve bu bildirimde ad `I`olan bir tür parametresi ([tür parametreleri](classes.md#type-parameters)) varsa, *namespace_or_type_name* bu tür parametresine başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-474">If `K` is zero and the *namespace_or_type_name* appears within a generic method declaration ([Methods](classes.md#methods)) and if that declaration includes a type parameter ([Type parameters](classes.md#type-parameters)) with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
    * <span data-ttu-id="98ef3-475">Aksi takdirde, *namespace_or_type_name* bir tür bildiriminde görünüyorsa, bu tür bildiriminin örnek türüyle başlayıp her bir örnek türü `T` ([örnek türü](classes.md#the-instance-type)) için, her bir kapsayan sınıfın veya yapı bildiriminin örnek türüne (varsa) devam edin:</span><span class="sxs-lookup"><span data-stu-id="98ef3-475">Otherwise, if the *namespace_or_type_name* appears within a type declaration, then for each instance type `T` ([The instance type](classes.md#the-instance-type)), starting with the instance type of that type declaration and continuing with the instance type of each enclosing class or struct declaration (if any):</span></span>
        * <span data-ttu-id="98ef3-476">`K` sıfırsa ve `T` bildirimi, ad `I`olan bir tür parametresi içeriyorsa, *namespace_or_type_name* bu tür parametresine başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-476">If `K` is zero and the declaration of `T` includes a type parameter with name `I`, then the *namespace_or_type_name* refers to that type parameter.</span></span>
        * <span data-ttu-id="98ef3-477">Aksi takdirde, *namespace_or_type_name* tür bildiriminin gövdesinde görünürse ve `T` veya temel türlerinden herhangi biri ad `I` ve `K` tür parametrelerine sahip iç içe erişilebilen bir tür içeriyorsa *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-477">Otherwise, if the *namespace_or_type_name* appears within the body of the type declaration, and `T` or any of its base types contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="98ef3-478">Böyle birden fazla tür varsa, daha fazla türetilmiş tür içinde belirtilen tür seçilidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-478">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="98ef3-479">Tür olmayan üyelerin (sabitler, alanlar, Yöntemler, özellikler, Dizin oluşturucular, işleçler, örnek oluşturucular, Yıkıcılar ve statik oluşturucular) ve *namespace_or_type_name*anlamı belirlenirken farklı sayıda tür parametresi olan tür üyelerinin yok sayılacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef3-479">Note that non-type members (constants, fields, methods, properties, indexers, operators, instance constructors, destructors, and static constructors) and type members with a different number of type parameters are ignored when determining the meaning of the *namespace_or_type_name*.</span></span>
    * <span data-ttu-id="98ef3-480">Önceki adımlar başarısız olduysa, `N`her ad alanı için *namespace_or_type_name* gerçekleştiği ad alanından başlayarak, kapsayan her ad alanıyla (varsa) devam eder ve genel ad alanıyla sona eriyor, aşağıdaki adımlar bir varlık bulunana kadar değerlendirilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-480">If the previous steps were unsuccessful then, for each namespace `N`, starting with the namespace in which the *namespace_or_type_name* occurs, continuing with each enclosing namespace (if any), and ending with the global namespace, the following steps are evaluated until an entity is located:</span></span>
        * <span data-ttu-id="98ef3-481">`K` sıfırsa ve `I` `N`bir ad alanının adı ise:</span><span class="sxs-lookup"><span data-stu-id="98ef3-481">If `K` is zero and `I` is the name of a namespace in `N`, then:</span></span>
            * <span data-ttu-id="98ef3-482">*Namespace_or_type_name* gerçekleştiği konum `N` için bir ad alanı bildirimiyle Çevrese ve ad alanı bildirimi, adı `I` ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *namespace_or_type_name* belirsizdir ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-482">If the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="98ef3-483">Aksi takdirde *namespace_or_type_name* , `N``I` adlı ad alanı anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-483">Otherwise, the *namespace_or_type_name* refers to the namespace named `I` in `N`.</span></span>
        * <span data-ttu-id="98ef3-484">Aksi takdirde, `N` ad `I` ve `K` tür parametrelerine sahip erişilebilir bir tür içeriyorsa:</span><span class="sxs-lookup"><span data-stu-id="98ef3-484">Otherwise, if `N` contains an accessible type having name `I` and `K` type parameters, then:</span></span>
            * <span data-ttu-id="98ef3-485">`K` sıfırsa ve *namespace_or_type_name* gerçekleştiği konum `N` için bir ad alanı bildirimiyle Çevrese ve ad alanı bildirimi ad using_alias_directive adını bir ad alanı veya türle ilişkilendiren bir *extern_alias_directive* veya * `I`* içeriyorsa, *namespace_or_type_name* belirsizdir ve bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-485">If `K` is zero and the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N` and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with a namespace or type, then the *namespace_or_type_name* is ambiguous and a compile-time error occurs.</span></span>
            * <span data-ttu-id="98ef3-486">Aksi takdirde *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-486">Otherwise, the *namespace_or_type_name* refers to the type constructed with the given type arguments.</span></span>
        * <span data-ttu-id="98ef3-487">Aksi takdirde, *namespace_or_type_name* gerçekleştiği konum `N`için bir ad alanı bildirimiyle çevrelenmiş ise:</span><span class="sxs-lookup"><span data-stu-id="98ef3-487">Otherwise, if the location where the *namespace_or_type_name* occurs is enclosed by a namespace declaration for `N`:</span></span>
            * <span data-ttu-id="98ef3-488">`K` sıfırsa ve ad alanı bildirimi, adı `I` içeri aktarılan bir ad alanı veya türle ilişkilendirir *extern_alias_directive* veya *using_alias_directive* içeriyorsa, *namespace_or_type_name* bu ad alanına veya türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-488">If `K` is zero and the namespace declaration contains an *extern_alias_directive* or *using_alias_directive* that associates the name `I` with an imported namespace or type, then the *namespace_or_type_name* refers to that namespace or type.</span></span>
            * <span data-ttu-id="98ef3-489">Aksi takdirde, *using_namespace_directive*s ve ad alanı bildiriminin *using_alias_directive*tarafından içeri aktarılan ad alanları ve tür bildirimleri, ad `I` ve `K` tür parametrelerine sahip tam olarak bir erişilebilir tür içeriyorsa, *namespace_or_type_name* verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-489">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain exactly one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
            * <span data-ttu-id="98ef3-490">Aksi takdirde, *using_namespace_directive*s ve ad alanı bildiriminin *using_alias_directive*tarafından içeri aktarılan ad alanları ve tür bildirimleri, ad `I` ve `K` tür parametrelerine sahip birden fazla erişilebilir tür içeriyorsa, *namespace_or_type_name* belirsizdir ve bir hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-490">Otherwise, if the namespaces and type declarations imported by the *using_namespace_directive*s and *using_alias_directive*s of the namespace declaration contain more than one accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* is ambiguous and an error occurs.</span></span>
    * <span data-ttu-id="98ef3-491">Aksi takdirde, *namespace_or_type_name* tanımsız olur ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-491">Otherwise, the *namespace_or_type_name* is undefined and a compile-time error occurs.</span></span>
*  <span data-ttu-id="98ef3-492">Aksi takdirde, *namespace_or_type_name* form `N.I` veya `N.I<A1, ..., Ak>`formundadır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-492">Otherwise, the *namespace_or_type_name* is of the form `N.I` or of the form `N.I<A1, ..., Ak>`.</span></span> <span data-ttu-id="98ef3-493">`N` önce *namespace_or_type_name*olarak çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-493">`N` is first resolved as a *namespace_or_type_name*.</span></span> <span data-ttu-id="98ef3-494">`N` çözümlemesi başarılı olmazsa, bir derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-494">If the resolution of `N` is not successful, a compile-time error occurs.</span></span> <span data-ttu-id="98ef3-495">Aksi takdirde, `N.I` veya `N.I<A1, ..., Ak>` aşağıdaki şekilde çözümlenir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-495">Otherwise, `N.I` or `N.I<A1, ..., Ak>` is resolved as follows:</span></span>
    * <span data-ttu-id="98ef3-496">`K` sıfırsa ve `N` bir ad alanına başvuruyorsa ve `N` `I`ada sahip bir iç içe ad alanı içeriyorsa, *namespace_or_type_name* bu iç içe ad alanına başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-496">If `K` is zero and `N` refers to a namespace and `N` contains a nested namespace with name `I`, then the *namespace_or_type_name* refers to that nested namespace.</span></span>
    * <span data-ttu-id="98ef3-497">Aksi takdirde, `N` bir ad alanına başvuruyorsa ve `N` ad `I` ve `K` tür parametrelerine sahip erişilebilir bir tür içeriyorsa *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-497">Otherwise, if `N` refers to a namespace and `N` contains an accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span>
    * <span data-ttu-id="98ef3-498">Aksi takdirde, `N` bir (büyük olasılıkla oluşturulmuş) sınıfa veya yapı türüne başvuruyorsa ve `N` ya da temel sınıflarından herhangi biri ad `I` ve `K` tür parametrelerine sahip iç içe erişilebilir bir tür içeriyorsa *namespace_or_type_name* , verilen tür bağımsız değişkenleriyle oluşturulan bu türe başvurur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-498">Otherwise, if `N` refers to a (possibly constructed) class or struct type and `N` or any of its base classes contain a nested accessible type having name `I` and `K` type parameters, then the *namespace_or_type_name* refers to that type constructed with the given type arguments.</span></span> <span data-ttu-id="98ef3-499">Böyle birden fazla tür varsa, daha fazla türetilmiş tür içinde belirtilen tür seçilidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-499">If there is more than one such type, the type declared within the more derived type is selected.</span></span> <span data-ttu-id="98ef3-500">`N.I` anlamı, `N` temel sınıf belirtiminin çözümlenme parçası olarak belirleniyorsa `N` doğrudan taban sınıfının Object ([temel sınıflar](classes.md#base-classes)) olarak kabul edileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef3-500">Note that if the meaning of `N.I` is being determined as part of resolving the base class specification of `N` then the direct base class of `N` is considered to be object ([Base classes](classes.md#base-classes)).</span></span>
    * <span data-ttu-id="98ef3-501">Aksi takdirde, `N.I` geçersiz bir *namespace_or_type_name*ve derleme zamanı hatası oluşur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-501">Otherwise, `N.I` is an invalid *namespace_or_type_name*, and a compile-time error occurs.</span></span>

<span data-ttu-id="98ef3-502">*Namespace_or_type_name* bir statik sınıfa ([statik sınıflar](classes.md#static-classes)) başvurmak için yalnızca</span><span class="sxs-lookup"><span data-stu-id="98ef3-502">A *namespace_or_type_name* is permitted to reference a static class ([Static classes](classes.md#static-classes)) only if</span></span>

*  <span data-ttu-id="98ef3-503">*Namespace_or_type_name* , `T.I`*namespace_or_type_name* `T` veya</span><span class="sxs-lookup"><span data-stu-id="98ef3-503">The *namespace_or_type_name* is the `T` in a *namespace_or_type_name* of the form `T.I`, or</span></span>
*  <span data-ttu-id="98ef3-504">*Namespace_or_type_name* , `typeof(T)`formun *typeof_expression* ([bağımsız değişken listeleri](expressions.md#argument-lists)1) `T`.</span><span class="sxs-lookup"><span data-stu-id="98ef3-504">The *namespace_or_type_name* is the `T` in a *typeof_expression* ([Argument lists](expressions.md#argument-lists)1) of the form `typeof(T)`.</span></span>

### <a name="fully-qualified-names"></a><span data-ttu-id="98ef3-505">Tam nitelikli adlar</span><span class="sxs-lookup"><span data-stu-id="98ef3-505">Fully qualified names</span></span>

<span data-ttu-id="98ef3-506">Her ad alanı ve türün, tüm diğerleri arasından ad alanını veya türü benzersiz bir şekilde tanımlayan ***tam bir adı***vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-506">Every namespace and type has a ***fully qualified name***, which uniquely identifies the namespace or type amongst all others.</span></span> <span data-ttu-id="98ef3-507">Bir ad alanının veya tür `N` tam nitelikli adı aşağıdaki gibi belirlenir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-507">The fully qualified name of a namespace or type `N` is determined as follows:</span></span>

*  <span data-ttu-id="98ef3-508">`N` genel ad alanının üyesiyse, tam adı `N`.</span><span class="sxs-lookup"><span data-stu-id="98ef3-508">If `N` is a member of the global namespace, its fully qualified name is `N`.</span></span>
*  <span data-ttu-id="98ef3-509">Aksi halde, tam adı `S.N`, burada `S` `N` bildirildiği ad alanının veya türün tam adıdır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-509">Otherwise, its fully qualified name is `S.N`, where `S` is the fully qualified name of the namespace or type in which `N` is declared.</span></span>

<span data-ttu-id="98ef3-510">Diğer bir deyişle `N` tam nitelikli adı, genel ad alanından başlayarak, `N`gelen tanımlayıcıların eksiksiz hiyerarşik yoludur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-510">In other words, the fully qualified name of `N` is the complete hierarchical path of identifiers that lead to `N`, starting from the global namespace.</span></span> <span data-ttu-id="98ef3-511">Bir ad alanının veya türün her üyesinin benzersiz bir adı olması gerektiğinden, bir ad alanının veya türün tam nitelikli adının her zaman benzersiz olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-511">Because every member of a namespace or type must have a unique name, it follows that the fully qualified name of a namespace or type is always unique.</span></span>

<span data-ttu-id="98ef3-512">Aşağıdaki örnekte, ilişkili tam adlarıyla birlikte çeşitli ad alanı ve tür bildirimleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-512">The example below shows several namespace and type declarations along with their associated fully qualified names.</span></span>
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

## <a name="automatic-memory-management"></a><span data-ttu-id="98ef3-513">Otomatik bellek yönetimi</span><span class="sxs-lookup"><span data-stu-id="98ef3-513">Automatic memory management</span></span>

<span data-ttu-id="98ef3-514">C#geliştiricilerin nesnelere göre kapladığı belleği el ile ayırmayı ve boşaltmasını sağlayan otomatik bellek yönetimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-514">C# employs automatic memory management, which frees developers from manually allocating and freeing the memory occupied by objects.</span></span> <span data-ttu-id="98ef3-515">Otomatik bellek yönetimi ilkeleri bir ***çöp toplayıcısı***tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-515">Automatic memory management policies are implemented by a ***garbage collector***.</span></span> <span data-ttu-id="98ef3-516">Bir nesnenin bellek yönetimi yaşam döngüsü aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-516">The memory management life cycle of an object is as follows:</span></span>

1. <span data-ttu-id="98ef3-517">Nesne oluşturulduğunda, için bellek ayrılır, Oluşturucu çalıştırılır ve nesne canlı olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-517">When the object is created, memory is allocated for it, the constructor is run, and the object is considered live.</span></span>
2. <span data-ttu-id="98ef3-518">Nesneye veya herhangi bir kısmına, yok edicilerin çalışması dışında, yürütmenin olası devamlılığı tarafından erişilemez, nesne artık kullanımda değildir ve yok etme için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-518">If the object, or any part of it, cannot be accessed by any possible continuation of execution, other than the running of destructors, the object is considered no longer in use, and it becomes eligible for destruction.</span></span> <span data-ttu-id="98ef3-519">C# Derleyici ve çöp toplayıcı, bir nesneye yapılan başvuruların gelecekte kullanılabileceğini belirlemek için kodu analiz etmeyi tercih edebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-519">The C# compiler and the garbage collector may choose to analyze code to determine which references to an object may be used in the future.</span></span> <span data-ttu-id="98ef3-520">Örneğin, kapsamdaki bir yerel değişken bir nesneye yönelik tek başvuru ise, ancak bu yerel değişken, yordamda geçerli yürütme noktasından yürütmenin olası devamlılığı hiçbir şekilde hiçbir şekilde başvurulmadıysa, çöp toplayıcı olabilir (ancak için gereklidir) nesneyi artık kullanımda değil olarak değerlendirin.</span><span class="sxs-lookup"><span data-stu-id="98ef3-520">For instance, if a local variable that is in scope is the only existing reference to an object, but that local variable is never referred to in any possible continuation of execution from the current execution point in the procedure, the garbage collector may (but is not required to) treat the object as no longer in use.</span></span>
3. <span data-ttu-id="98ef3-521">Nesne yok etme için uygun olduğunda, bazı belirtilmemişse nesne için yıkıcı (yok[ediciler](classes.md#destructors)) (varsa) çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-521">Once the object is eligible for destruction, at some unspecified later time the destructor ([Destructors](classes.md#destructors)) (if any) for the object is run.</span></span> <span data-ttu-id="98ef3-522">Normal koşullarda nesnenin yıkıcısı yalnızca bir kez çalıştırılır, ancak uygulamaya özgü API 'Ler bu davranışın geçersiz kılınmasına izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-522">Under normal circumstances the destructor for the object is run once only, though implementation-specific APIs may allow this behavior to be overridden.</span></span>
4. <span data-ttu-id="98ef3-523">Bir nesnenin yıkıcısı çalıştırıldığında, bu nesneye veya bunun herhangi bir kısmına, yok edicinin çalıştırılması dahil olmak üzere herhangi bir olası yürütme devamlılığı tarafından erişilemez, nesnenin erişilemez olduğu kabul edilir ve nesne koleksiyona uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-523">Once the destructor for an object is run, if that object, or any part of it, cannot be accessed by any possible continuation of execution, including the running of destructors, the object is considered inaccessible and the object becomes eligible for collection.</span></span>
5. <span data-ttu-id="98ef3-524">Son olarak, nesne koleksiyon için uygun olduktan sonra çöp toplayıcı bu nesneyle ilişkili belleği serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-524">Finally, at some time after the object becomes eligible for collection, the garbage collector frees the memory associated with that object.</span></span>

<span data-ttu-id="98ef3-525">Çöp toplayıcı, nesne kullanımı hakkındaki bilgileri tutar ve bu bilgileri kullanarak yeni oluşturulan bir nesneyi bulma, bir nesnenin konumunu değiştirme ve bir nesnenin artık ne zaman ne zaman kullanıldığı veya erişilemeyen gibi bellek yönetimi kararları almak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-525">The garbage collector maintains information about object usage, and uses this information to make memory management decisions, such as where in memory to locate a newly created object, when to relocate an object, and when an object is no longer in use or inaccessible.</span></span>

<span data-ttu-id="98ef3-526">Çöp toplayıcısının varlığını varsaymış diğer diller gibi, C# çöp toplayıcı çok çeşitli bellek yönetimi ilkeleri uygulayabilmesi için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-526">Like other languages that assume the existence of a garbage collector, C# is designed so that the garbage collector may implement a wide range of memory management policies.</span></span> <span data-ttu-id="98ef3-527">Örneğin, C# yok edicilerin çalıştırılmasını gerektirmez veya uygun olan veya yok edicilerin belirli bir sırada veya belirli bir iş parçacığında çalıştırılması için nesnelerin toplanıp toplanmasına gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-527">For instance, C# does not require that destructors be run or that objects be collected as soon as they are eligible, or that destructors be run in any particular order, or on any particular thread.</span></span>

<span data-ttu-id="98ef3-528">Çöp toplayıcısının davranışı, sınıf `System.GC`statik yöntemler aracılığıyla bir dereceye kadar denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-528">The behavior of the garbage collector can be controlled, to some degree, via static methods on the class `System.GC`.</span></span> <span data-ttu-id="98ef3-529">Bu sınıf, bir koleksiyon oluşmasına, çalıştırılacak yok edicilere (veya çalıştırılmayan), vb. istemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-529">This class can be used to request a collection to occur, destructors to be run (or not run), and so forth.</span></span>

<span data-ttu-id="98ef3-530">Çöp toplayıcısının nesnelerin ne zaman toplanmaya karar verirken ve yıkıcıları çalıştırmanın yanı sıra, uygun bir uygulama aşağıdaki kodla gösterilenden farklı bir çıktı üretebilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-530">Since the garbage collector is allowed wide latitude in deciding when to collect objects and run destructors, a conforming implementation may produce output that differs from that shown by the following code.</span></span> <span data-ttu-id="98ef3-531">Program</span><span class="sxs-lookup"><span data-stu-id="98ef3-531">The program</span></span>
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
<span data-ttu-id="98ef3-532">`A` sınıfının bir örneğini ve `B`sınıfının bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-532">creates an instance of class `A` and an instance of class `B`.</span></span> <span data-ttu-id="98ef3-533">Bu nesneler, değişken `b` `null`atandığında, bu tarihten sonra herhangi bir kullanıcı tarafından yazılan kodun erişebilmesi olanaksız olduğundan çöp toplama için uygun hale gelir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-533">These objects become eligible for garbage collection when the variable `b` is assigned the value `null`, since after this time it is impossible for any user-written code to access them.</span></span> <span data-ttu-id="98ef3-534">Çıktı şunlardan biri olabilir</span><span class="sxs-lookup"><span data-stu-id="98ef3-534">The output could be either</span></span>

```console
Destruct instance of A
Destruct instance of B
```
<span data-ttu-id="98ef3-535">veya</span><span class="sxs-lookup"><span data-stu-id="98ef3-535">or</span></span>
```console
Destruct instance of B
Destruct instance of A
```
<span data-ttu-id="98ef3-536">dil, nesnelerin atık olarak toplandığı sıraya göre hiçbir kısıtlama yapmadığından.</span><span class="sxs-lookup"><span data-stu-id="98ef3-536">because the language imposes no constraints on the order in which objects are garbage collected.</span></span>

<span data-ttu-id="98ef3-537">Hafif durumlarda, "yok etme için uygun" ve "koleksiyon için uygun" arasındaki ayrım önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-537">In subtle cases, the distinction between "eligible for destruction" and "eligible for collection" can be important.</span></span> <span data-ttu-id="98ef3-538">Örneğin,</span><span class="sxs-lookup"><span data-stu-id="98ef3-538">For example,</span></span>
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

<span data-ttu-id="98ef3-539">Yukarıdaki programda, atık toplayıcı `B`yıkıcısından önce `A` yok ediciyi çalıştırmayı seçerse, bu programın çıktısı şu olabilir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-539">In the above program, if the garbage collector chooses to run the destructor of `A` before the destructor of `B`, then the output of this program might be:</span></span>
```console
Destruct instance of A
Destruct instance of B
A.F
RefA is not null
```

<span data-ttu-id="98ef3-540">`A` örneği kullanımda olmadığından ve `A`yok edicinin çalıştırıldığına rağmen, başka bir yıkıcıdan `A` (Bu durumda `F`) yöntemleri için yine de mümkündür.</span><span class="sxs-lookup"><span data-stu-id="98ef3-540">Note that although the instance of `A` was not in use and `A`'s destructor was run, it is still possible for methods of `A` (in this case, `F`) to be called from another destructor.</span></span> <span data-ttu-id="98ef3-541">Ayrıca, bir yıkıcı çalıştırmanın bir nesnenin ana hat programından yeniden kullanılabilir hale gelmesine neden olabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="98ef3-541">Also, note that running of a destructor may cause an object to become usable from the mainline program again.</span></span> <span data-ttu-id="98ef3-542">Bu durumda `B`yıkıcının çalıştırılması, daha önce kullanımda olmayan bir `A` örneğini canlı başvuru `Test.RefA`erişilebilir hale gelmesine neden oldu.</span><span class="sxs-lookup"><span data-stu-id="98ef3-542">In this case, the running of `B`'s destructor caused an instance of `A` that was previously not in use to become accessible from the live reference `Test.RefA`.</span></span> <span data-ttu-id="98ef3-543">`WaitForPendingFinalizers`çağrısından sonra, `B` örneği koleksiyon için uygundur, ancak başvuru `Test.RefA`nedeniyle `A` örneği değildir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-543">After the call to `WaitForPendingFinalizers`, the instance of `B` is eligible for collection, but the instance of `A` is not, because of the reference `Test.RefA`.</span></span>

<span data-ttu-id="98ef3-544">Karışıklıkları ve beklenmedik davranışları önlemek için, yok edicilerin yalnızca kendi nesnelerinde depolanan verilerde temizlik yapması ve başvurulan nesneler veya statik alanlar üzerinde herhangi bir eylem gerçekleştirmemek için iyi bir fikir vardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-544">To avoid confusion and unexpected behavior, it is generally a good idea for destructors to only perform cleanup on data stored in their object's own fields, and not to perform any actions on referenced objects or static fields.</span></span>

<span data-ttu-id="98ef3-545">Yıkıcıları kullanmanın bir alternatifi, bir sınıfın `System.IDisposable` arabirimini uygulamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-545">An alternative to using destructors is to let a class implement the `System.IDisposable` interface.</span></span> <span data-ttu-id="98ef3-546">Bu, nesne istemcisinin, genellikle nesnesine bir `using` deyimindeki ([using deyimindeki](statements.md#the-using-statement)) bir kaynak olarak erişerek nesne kaynaklarının ne zaman yayınlanmadığını belirlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="98ef3-546">This allows the client of the object to determine when to release the resources of the object, typically by accessing the object as a resource in a `using` statement ([The using statement](statements.md#the-using-statement)).</span></span>

## <a name="execution-order"></a><span data-ttu-id="98ef3-547">Yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="98ef3-547">Execution order</span></span>

<span data-ttu-id="98ef3-548">Bir C# programın yürütülmesi, yürütülen her iş parçacığının yan etkileri kritik yürütme noktalarında korunacağından devam eder.</span><span class="sxs-lookup"><span data-stu-id="98ef3-548">Execution of a C# program proceeds such that the side effects of each executing thread are preserved at critical execution points.</span></span> <span data-ttu-id="98ef3-549">Bir ***yan etkisi*** , geçici bir alan okuma veya yazma, geçici olmayan bir değişkene yazma, bir dış kaynağa yazma ve özel durum atma olarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-549">A ***side effect*** is defined as a read or write of a volatile field, a write to a non-volatile variable, a write to an external resource, and the throwing of an exception.</span></span> <span data-ttu-id="98ef3-550">Bu yan etkileri sırasının korunması gereken kritik yürütme noktaları, geçici alanlar ([geçici alanlar](classes.md#volatile-fields)), `lock` deyimleri ([kilit deyimi](statements.md#the-lock-statement)) ve iş parçacığı oluşturma ve sonlandırma için referanslardır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-550">The critical execution points at which the order of these side effects must be preserved are references to volatile fields ([Volatile fields](classes.md#volatile-fields)), `lock` statements ([The lock statement](statements.md#the-lock-statement)), and thread creation and termination.</span></span> <span data-ttu-id="98ef3-551">Yürütme ortamı, bir C# programın yürütme sırasını değiştirmek için ücretsizdir ve aşağıdaki kısıtlamalara tabidir:</span><span class="sxs-lookup"><span data-stu-id="98ef3-551">The execution environment is free to change the order of execution of a C# program, subject to the following constraints:</span></span>

*  <span data-ttu-id="98ef3-552">Veri bağımlılığı, yürütme iş parçacığı içinde korunur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-552">Data dependence is preserved within a thread of execution.</span></span> <span data-ttu-id="98ef3-553">Diğer bir deyişle, her değişkenin değeri iş parçacığındaki tüm deyimler orijinal program sırasıyla yürütülürse olarak hesaplanır.</span><span class="sxs-lookup"><span data-stu-id="98ef3-553">That is, the value of each variable is computed as if all statements in the thread were executed in original program order.</span></span>
*  <span data-ttu-id="98ef3-554">Başlatma sırası kuralları korunur ([alan başlatma](classes.md#field-initialization) ve [değişken başlatıcıları](classes.md#variable-initializers)).</span><span class="sxs-lookup"><span data-stu-id="98ef3-554">Initialization ordering rules are preserved ([Field initialization](classes.md#field-initialization) and [Variable initializers](classes.md#variable-initializers)).</span></span>
*  <span data-ttu-id="98ef3-555">Yan etkileri sıralaması, geçici okuma ve yazma işlemleri ([geçici alanlar](classes.md#volatile-fields)) açısından korunur.</span><span class="sxs-lookup"><span data-stu-id="98ef3-555">The ordering of side effects is preserved with respect to volatile reads and writes ([Volatile fields](classes.md#volatile-fields)).</span></span> <span data-ttu-id="98ef3-556">Ayrıca, bu ifadenin değerinin kullanılmadığını ve gerekli yan etkileri üretilmez (bir yöntemi çağırarak veya geçici bir alana erişim nedeniyle oluşan herhangi biri dahil), yürütme ortamı bir ifadenin bir kısmını değerlendirmemelidir.</span><span class="sxs-lookup"><span data-stu-id="98ef3-556">Additionally, the execution environment need not evaluate part of an expression if it can deduce that that expression's value is not used and that no needed side effects are produced (including any caused by calling a method or accessing a volatile field).</span></span> <span data-ttu-id="98ef3-557">Program yürütme zaman uyumsuz bir olay (örneğin, başka bir iş parçacığı tarafından oluşturulan bir özel durum) tarafından kesintiye uğradığında, observable yan etkilerinin orijinal program sırasıyla görünür olması garanti edilmez.</span><span class="sxs-lookup"><span data-stu-id="98ef3-557">When program execution is interrupted by an asynchronous event (such as an exception thrown by another thread), it is not guaranteed that the observable side effects are visible in the original program order.</span></span>
