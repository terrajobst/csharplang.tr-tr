---
ms.openlocfilehash: 201db57d243c9d0e22553366bc653d02e183aa4b
ms.sourcegitcommit: 09e0ddec3bb6aa99b7340158bbac86a5a8243b43
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66193862"
---
# <a name="introduction"></a><span data-ttu-id="4ba64-101">Giriş</span><span class="sxs-lookup"><span data-stu-id="4ba64-101">Introduction</span></span>

<span data-ttu-id="4ba64-102">C# (okunur "Bkz Sharp") bir basit, modern, nesne yönelimli ve tür kullanımı uyumlu programlama dilidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="4ba64-103">C# dilleri C ailesinde kendi kökleri sahiptir ve Java C ve C++ programcıları için hemen tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="4ba64-104">C# Standart ECMA uluslararası tarafından ***ECMA 334*** standart ve ISO/IEC ***ISO/IEC 23270*** standart.</span><span class="sxs-lookup"><span data-stu-id="4ba64-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="4ba64-105">Microsoft'un C# derleyicisi .NET Framework için hem bu standartlarla uyumlu bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="4ba64-106">C#, nesne yönelimli bir dildir ancak C# daha fazla için destek içerir ***bileşen odaklı*** programlama.</span><span class="sxs-lookup"><span data-stu-id="4ba64-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="4ba64-107">Çağdaş yazılım tasarımı giderek daha fazla işlevsellik müstakil ve kendiliğinden açıklayıcı paketleri biçiminde yazılım bileşenlerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="4ba64-108">Bu bileşenler özellikleri, yöntemleri ve olayları ile bunların bir programlama modeli sunar, anahtarıdır; bileşen hakkında tanımlayıcı bilgiler sağlayan özniteliklerine sahip oldukları; ve bunlar kendi belgelerinin dahil edilip derecelendirilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="4ba64-109">C# doğrudan bu kavramlar desteklemek için dil yapıları C# yapmak çok doğal dili oluşturma ve yazılım bileşenlerini kullanma sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="4ba64-110">Birkaç C# ürettiği yapımı güçlü ve dayanıklı uygulamaların özellikleri: ***Çöp toplama*** otomatik olarak; kullanılmayan nesnelerin kapladığı belleği geri kazanır ***özel durum işleme*** hata algılama ve kurtarma; yapılandırılmış ve Genişletilebilir bir yaklaşım sunan ve ***tür kullanımı uyumlu*** dilinin tasarım kılar imkansız başlatılmamış değişkenlerinden okuyun dizin için kendi sınırları ötesinde veya denetlenmeyen gerçekleştirmek için bir dizi yayınları yazın.</span><span class="sxs-lookup"><span data-stu-id="4ba64-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="4ba64-111">C# sahip bir ***tür sistemi birleşik***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="4ba64-112">Gibi ilkel türler dahil olmak üzere tüm C# türü, `int` ve `double`, tek bir kök devral `object` türü.</span><span class="sxs-lookup"><span data-stu-id="4ba64-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="4ba64-113">Bu nedenle, bir dizi ortak işlemlerini tüm türleri paylaşın ve herhangi bir tür değerleri depolanan, taşınan ve tutarlı bir şekilde üzerinde işlem.</span><span class="sxs-lookup"><span data-stu-id="4ba64-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="4ba64-114">Ayrıca, C# hem kullanıcı tarafından tanımlanan başvuru türleri ve değer türleri, nesnelerin dinamik ayırması yanı sıra basit yapılar satır içi depolanmasını sağlayan destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="4ba64-115">C# programları ve kitaplıkları zaman uyumlu bir şekilde geliştirebilirsiniz emin olmak için çok Vurgu üzerinde yerleştirilen ***sürüm*** C# ' ın tasarım.</span><span class="sxs-lookup"><span data-stu-id="4ba64-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="4ba64-116">Birçok programlama dili bu sorun için çok az dikkat edin ve sonuç olarak, bu dillerin sonu bağımlı kitaplıkları gerekli olduğunda daha yeni sürümleri daha sık yazılan programlar sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="4ba64-117">Sürüm değerlendirmeleri tarafından doğrudan etkileyen C# ' ın tasarım yönlerini dahil ayrı `virtual` ve `override` değiştiriciler, yöntem aşırı yükleme çözümlemesi için kurallar ve açık arabirim üyesi bildirimi için destek.</span><span class="sxs-lookup"><span data-stu-id="4ba64-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="4ba64-118">Bu bölümün geri kalanında, C# dilinin temel özelliklerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="4ba64-119">Sonraki bölümlerde ayrıntılı odaklı ve bazen matematiksel bir şekilde kurallar ve özel durumlar açıklayan olsa da, bu bölümde açıklık ve bütünlük çoğaltamaz kısaltma üstlenmeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="4ba64-120">Amaç, erken programlar yazılmasını ve sonraki bölümlerde okunmasını kolaylaştırmak dil için bir tanıtım okuyucu sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="4ba64-121">Merhaba dünya</span><span class="sxs-lookup"><span data-stu-id="4ba64-121">Hello world</span></span>

<span data-ttu-id="4ba64-122">"Hello, World" programı, geleneksel bir programlama dili tanıtmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="4ba64-123">Bunu C# ' de şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="4ba64-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="4ba64-124">C# kaynak dosyaları, genellikle dosya uzantısına sahip `.cs`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="4ba64-125">"Hello, World" program dosyasında depolanan varsayarak `hello.cs`, programın komut satırı kullanılarak Microsoft C# derleyicisi ile derlenebilir</span><span class="sxs-lookup"><span data-stu-id="4ba64-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="4ba64-126">adlı yürütülebilir bir derleme üreten `hello.exe`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="4ba64-127">Çalıştırıldığında, bu uygulama tarafından üretilen çıkış</span><span class="sxs-lookup"><span data-stu-id="4ba64-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="4ba64-128">"Hello, World" programı ile başlayan bir `using` başvuran yönergesi `System` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="4ba64-129">Ad alanlarında, C# programları ve kitaplıkları düzenleme, hiyerarşik bir yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="4ba64-130">Ad alanları, türler ve diğer ad alanları içerir — örneğin, `System` ad alanı içeren bir dizi türleri gibi `Console` program ve diğer ad alanları, bir sayı gibi başvurulan sınıfı `IO` ve `Collections`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="4ba64-131">A `using` belirtilen bir ad alanı başvuruyor yönergesi bu ad alanının üyeleri olan türlerinin nitelenmemiş kullanımı sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="4ba64-132">Nedeniyle `using` yönerge, programın kullanabilirsiniz `Console.WriteLine` için kısayol olarak `System.Console.WriteLine`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="4ba64-133">`Hello` "Hello, World" program tarafından bildirilen adlı yöntem tek bir üye sınıfında `Main`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="4ba64-134">`Main` Yöntemi ile bildirilir `static` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="4ba64-135">Örnek yöntemleri anahtar sözcüğü kullanılarak belirli kapsayan nesne örneği başvurabilirsiniz ancak `this`, belirli bir nesneye başvuru olmadan statik yöntemleri çalışır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="4ba64-136">Kural gereği, adlı statik bir yöntemi `Main` bir programın giriş noktası olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="4ba64-137">Program çıktısı tarafından üretilen `WriteLine` yöntemi `Console` sınıfını `System` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="4ba64-138">Bu sınıf, varsayılan olarak, Microsoft C# Derleyici tarafından otomatik olarak başvurulur, .NET Framework sınıf kitaplıkları tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="4ba64-139">C# kendisini'nın ayrı bir çalışma zamanı kitaplığı olmadığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="4ba64-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="4ba64-140">Bunun yerine, .NET Framework çalışma zamanı kitaplığı, C# ' dir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="4ba64-141">Program yapısı</span><span class="sxs-lookup"><span data-stu-id="4ba64-141">Program structure</span></span>

<span data-ttu-id="4ba64-142">C# anahtar kuruluş kavramlar ***programlar***, ***ad alanları***, ***türleri***, ***üyeleri***, ve ***derlemeleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="4ba64-143">C# programları, bir veya daha fazla kaynak dosyadan oluşur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="4ba64-144">Programları üyeleri içerir ve ad alanları olarak düzenlenmiş türleri bildirin.</span><span class="sxs-lookup"><span data-stu-id="4ba64-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="4ba64-145">Sınıflar ve arabirimler türleri örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="4ba64-146">Alanlar, yöntemler, özellikler ve olaylar üyeleri örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="4ba64-147">C# programları derlendiğinde, bunlar derlemelerine fiziksel olarak paketlenir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="4ba64-148">Derlemeler genellikle dosya uzantısına sahip `.exe` veya `.dll`uyguladıkları mi bağlı olarak ***uygulamaları*** veya ***kitaplıkları***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="4ba64-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="4ba64-149">The example</span></span>

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
<span data-ttu-id="4ba64-150">adlı bir sınıfı bildirir `Stack` adlı bir ad alanında `Acme.Collections`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="4ba64-151">Bu sınıfın tam adı `Acme.Collections.Stack`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="4ba64-152">Sınıf bazı üyeler içerir: adlı bir alan `top`, adlı iki yöntem `Push` ve `Pop`ve bir iç içe geçmiş sınıf adlı `Entry`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="4ba64-153">`Entry` Sınıfı daha da içeren üç üye: adlı bir alan `next`, adında bir alan `data`ve bir oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="4ba64-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="4ba64-154">Örnek kaynak kodu dosyasında depolanan varsayarak `acme.cs`, komut satırı</span><span class="sxs-lookup"><span data-stu-id="4ba64-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="4ba64-155">Örnek bir kitaplık olarak derler (olmadan kod bir `Main` giriş noktası) adlı bir derleme oluşturur `acme.dll`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="4ba64-156">Derlemeleri içeren yürütülebilir kod biçiminde ***Ara dil*** (IL) yönergeleri ve simgesel bilgiler biçiminde ***meta verileri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="4ba64-157">Çalıştırılmadan önce bir derlemede IL kodu işlemciye özgü kodu .NET ortak dil çalışma zamanı tam zamanında (JIT) derleyici tarafından otomatik olarak dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="4ba64-158">Bir derleme kendiliğinden açıklayıcı bir işlevsellik hem kod hem de meta verileri içeren birimi olduğundan için gerek yoktur `#include` yönergeleri ve C# üst bilgi dosyaları.</span><span class="sxs-lookup"><span data-stu-id="4ba64-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="4ba64-159">Genel türler ve üyeler belirli bir derlemede bulunan program derlerken bu derlemeye yalnızca başvurarak bir C# programı içinde kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="4ba64-160">Örneğin, bu programın kullandığı `Acme.Collections.Stack` gelen sınıfı `acme.dll` derleme:</span><span class="sxs-lookup"><span data-stu-id="4ba64-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
<span data-ttu-id="4ba64-161">Program dosyada saklanıyorsa `test.cs`, `test.cs` derlenir, `acme.dll` kullanarak derleyicinin derleme başvurulabilir `/r` seçeneği:</span><span class="sxs-lookup"><span data-stu-id="4ba64-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="4ba64-162">Bu adında bir yürütülebilir bir derleme oluşturur `test.exe`, çalıştırdığınızda, çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="4ba64-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="4ba64-163">C# kaynak metni bir programın çeşitli kaynak dosyalarında depolanan izin verir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="4ba64-164">Çok dosyalı C# programı derlenir, tüm kaynak dosyaları birlikte işlenir ve kaynak dosyaları serbestçe birbirlerine başvurabilir — kavramsal olarak, tüm kaynak dosyalarını tek bir büyük dosyaya işlenmeden önce art arda eklenmiş gibi öyledir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="4ba64-165">Çok az sayıda özel durum dışında bildirim sırasında Önemsiz olduğundan, iletme bildirimleri hiçbir zaman C# ' de gereklidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="4ba64-166">C#, yalnızca bir genel türü bildirmek için bir kaynak dosyası sınırlamaz veya kaynak dosyada bildirilen bir tür ile eşleştirilecek kaynak dosyasının adı gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="4ba64-167">Türler ve değişkenler</span><span class="sxs-lookup"><span data-stu-id="4ba64-167">Types and variables</span></span>

<span data-ttu-id="4ba64-168">C# türlerinde iki tür vardır: ***değer türleri*** ve ***başvuru türleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="4ba64-169">Başvuru türlerinin değişkenleri başvuruları kendi verilerine depolamak ise, değer türlerinin değişkenleri kendi verilerini doğrudan içerir, ikincisi nesneler olarak bilinen.</span><span class="sxs-lookup"><span data-stu-id="4ba64-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="4ba64-170">Başvuru türleri ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="4ba64-171">Değer türleri ile her değişkenleri kendi veri kopyasını sahip ve işlemlerin bir diğerini etkilemesi olanaklı değildir (dışındaki durumunda `ref` ve `out` parametresi değişkenleri).</span><span class="sxs-lookup"><span data-stu-id="4ba64-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="4ba64-172">C# ' ın değer türleri içinde daha bölünür ***basit türler***, ***Numaralandırma türleri***, ***yapı türleri***, ve ***boş değer atanabilir türler***ve C# başvurusu türleri içine daha bölünür ***sınıf türleri***, ***arabirim türleri***, ***dizisi, türlerini***, ve ***temsilci türleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="4ba64-173">Aşağıdaki tablo, C# tür sistemi genel bir bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="4ba64-174">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="4ba64-174">__Category__</span></span>    |                 | <span data-ttu-id="4ba64-175">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="4ba64-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="4ba64-176">Değer türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-176">Value types</span></span>     | <span data-ttu-id="4ba64-177">Basit türler</span><span class="sxs-lookup"><span data-stu-id="4ba64-177">Simple types</span></span>    | <span data-ttu-id="4ba64-178">İmzalı tam sayı: `sbyte`, `short`, `int`, `long`</span><span class="sxs-lookup"><span data-stu-id="4ba64-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-179">İşaretsiz integral: `byte`, `ushort`, `uint`, `ulong`</span><span class="sxs-lookup"><span data-stu-id="4ba64-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-180">Unicode karakter sayısı: `char`</span><span class="sxs-lookup"><span data-stu-id="4ba64-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-181">IEEE kayan nokta: `float`, `double`</span><span class="sxs-lookup"><span data-stu-id="4ba64-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-182">Yüksek duyarlıklı ondalık: `decimal`</span><span class="sxs-lookup"><span data-stu-id="4ba64-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-183">Boole: `bool`</span><span class="sxs-lookup"><span data-stu-id="4ba64-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="4ba64-184">Numaralandırma türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-184">Enum types</span></span>      | <span data-ttu-id="4ba64-185">Formun kullanıcı tanımlı türler `enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="4ba64-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="4ba64-186">Yapı türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-186">Struct types</span></span>    | <span data-ttu-id="4ba64-187">Formun kullanıcı tanımlı türler `struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="4ba64-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="4ba64-188">Boş değer atanabilir tipler</span><span class="sxs-lookup"><span data-stu-id="4ba64-188">Nullable types</span></span>  | <span data-ttu-id="4ba64-189">Diğer tüm değer türleri ile uzantıları bir `null` değeri</span><span class="sxs-lookup"><span data-stu-id="4ba64-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="4ba64-190">Başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-190">Reference types</span></span> | <span data-ttu-id="4ba64-191">Sınıf türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-191">Class types</span></span>     | <span data-ttu-id="4ba64-192">Diğer tüm türlerin Ultimate temel sınıf: `object`</span><span class="sxs-lookup"><span data-stu-id="4ba64-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-193">Unicode dizelerini: `string`</span><span class="sxs-lookup"><span data-stu-id="4ba64-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="4ba64-194">Formun kullanıcı tanımlı türler `class C {...}`</span><span class="sxs-lookup"><span data-stu-id="4ba64-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="4ba64-195">Arabirim türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-195">Interface types</span></span> | <span data-ttu-id="4ba64-196">Formun kullanıcı tanımlı türler `interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="4ba64-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="4ba64-197">Dizi türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-197">Array types</span></span>     | <span data-ttu-id="4ba64-198">Tek ve örneğin, çok boyutlu `int[]` ve `int[,]`</span><span class="sxs-lookup"><span data-stu-id="4ba64-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="4ba64-199">Temsilci türleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-199">Delegate types</span></span>  | <span data-ttu-id="4ba64-200">Kullanıcı tanımlı türler formun örn: `delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="4ba64-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="4ba64-201">Sekiz integral türleri, 8-bit, 16-bit, 32-bit ve 64-bit işaretli veya işaretsiz form değerleri için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="4ba64-202">İki kayan noktası türleri `float` ve `double`, 32-bit tek duyarlıklı ve 64-bit çift duyarlıklı IEEE 754 biçimleri kullanılarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="4ba64-203">`decimal` Türüdür finansal ve parasal hesaplamalar için uygun bir 128-bit veri türü.</span><span class="sxs-lookup"><span data-stu-id="4ba64-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="4ba64-204">C# `bool` türü boolean değerlerini temsil edecek şekilde kullanılır — ya da değerler `true` veya `false`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="4ba64-205">Karakter ve dize işleme C# dilinde Unicode kodlaması kullanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="4ba64-206">`char` Türünü bir UTF-16 kod birimini temsil eder ve `string` türü UTF-16 kod birimlerini dizisini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4ba64-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="4ba64-207">Aşağıdaki tablo, C# ' ın sayısal türleri özetlenmektedir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="4ba64-208">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="4ba64-208">__Category__</span></span>      | <span data-ttu-id="4ba64-209">__BITS__</span><span class="sxs-lookup"><span data-stu-id="4ba64-209">__Bits__</span></span> | <span data-ttu-id="4ba64-210">__Tür__</span><span class="sxs-lookup"><span data-stu-id="4ba64-210">__Type__</span></span>  | <span data-ttu-id="4ba64-211">__Aralık/duyarlık__</span><span class="sxs-lookup"><span data-stu-id="4ba64-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="4ba64-212">İmzalı tam sayı</span><span class="sxs-lookup"><span data-stu-id="4ba64-212">Signed integral</span></span>   | <span data-ttu-id="4ba64-213">8</span><span class="sxs-lookup"><span data-stu-id="4ba64-213">8</span></span>        | `sbyte`   | <span data-ttu-id="4ba64-214">-128...127</span><span class="sxs-lookup"><span data-stu-id="4ba64-214">-128...127</span></span> |
|                   | <span data-ttu-id="4ba64-215">16</span><span class="sxs-lookup"><span data-stu-id="4ba64-215">16</span></span>       | `short`   | <span data-ttu-id="4ba64-216">-32,768...32,767</span><span class="sxs-lookup"><span data-stu-id="4ba64-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="4ba64-217">32</span><span class="sxs-lookup"><span data-stu-id="4ba64-217">32</span></span>       | `int`     | <span data-ttu-id="4ba64-218">-2,147,483,648...2,147,483,647</span><span class="sxs-lookup"><span data-stu-id="4ba64-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="4ba64-219">64</span><span class="sxs-lookup"><span data-stu-id="4ba64-219">64</span></span>       | `long`    | <span data-ttu-id="4ba64-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span><span class="sxs-lookup"><span data-stu-id="4ba64-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="4ba64-221">İşaretsiz tamsayı</span><span class="sxs-lookup"><span data-stu-id="4ba64-221">Unsigned integral</span></span> | <span data-ttu-id="4ba64-222">8</span><span class="sxs-lookup"><span data-stu-id="4ba64-222">8</span></span>        | `byte`    | <span data-ttu-id="4ba64-223">0...255</span><span class="sxs-lookup"><span data-stu-id="4ba64-223">0...255</span></span> |
|                   | <span data-ttu-id="4ba64-224">16</span><span class="sxs-lookup"><span data-stu-id="4ba64-224">16</span></span>       | `ushort`  | <span data-ttu-id="4ba64-225">0...65,535</span><span class="sxs-lookup"><span data-stu-id="4ba64-225">0...65,535</span></span> |
|                   | <span data-ttu-id="4ba64-226">32</span><span class="sxs-lookup"><span data-stu-id="4ba64-226">32</span></span>       | `uint`    | <span data-ttu-id="4ba64-227">0...4,294,967,295</span><span class="sxs-lookup"><span data-stu-id="4ba64-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="4ba64-228">64</span><span class="sxs-lookup"><span data-stu-id="4ba64-228">64</span></span>       | `ulong`   | <span data-ttu-id="4ba64-229">0...18,446,744,073,709,551,615</span><span class="sxs-lookup"><span data-stu-id="4ba64-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="4ba64-230">Kayan nokta</span><span class="sxs-lookup"><span data-stu-id="4ba64-230">Floating point</span></span>    | <span data-ttu-id="4ba64-231">32</span><span class="sxs-lookup"><span data-stu-id="4ba64-231">32</span></span>       | `float`   | <span data-ttu-id="4ba64-232">1.5 × 10 ^ −45 3.4 × 10 ^ 7 basamaklı duyarlık 38,</span><span class="sxs-lookup"><span data-stu-id="4ba64-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="4ba64-233">64</span><span class="sxs-lookup"><span data-stu-id="4ba64-233">64</span></span>       | `double`  | <span data-ttu-id="4ba64-234">5.0 × 10 ^ −324 1.7 × 10 ^ 308, 15 basamaklı duyarlık</span><span class="sxs-lookup"><span data-stu-id="4ba64-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="4ba64-235">Ondalık</span><span class="sxs-lookup"><span data-stu-id="4ba64-235">Decimal</span></span>           | <span data-ttu-id="4ba64-236">128</span><span class="sxs-lookup"><span data-stu-id="4ba64-236">128</span></span>      | `decimal` | <span data-ttu-id="4ba64-237">1.0 × 10 ^ −28 7,9 × 10 ^ 28, 28 basamaklı duyarlık</span><span class="sxs-lookup"><span data-stu-id="4ba64-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="4ba64-238">C# programları kullanım ***tür bildirimleri*** yeni türler oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="4ba64-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="4ba64-239">Bir tür bildirimi, adı ve yeni türün üyeleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="4ba64-240">C# türleri kategorilerini beş kullanıcı tarafından tanımlanabilir: Sınıf türleri, yapı türleri, arabirim türleri, sabit listesi türleri ve temsilci türleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="4ba64-241">Bir sınıf türü, veri üyelerine (alanları) ve işlev üyeleri (yöntemler, özellikler ve diğerleri) içeren bir veri yapısı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="4ba64-242">Sınıf türleri tek devralma ve çok biçimlilik, yapabildiği türetilmiş sınıfları genişletmek ve temel sınıflar specialize mekanizmaları destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="4ba64-243">Bir yapı türü, veri üyeleri ve işlev üyeleri bir yapıya temsil ettiği, bir sınıf türüne benzerdir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="4ba64-244">Ancak, farklı sınıflar, yapılar değer türleri ve yığın ayırma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="4ba64-245">Yapı türleri, kullanıcı tarafından belirtilen devralma desteklemez ve tüm yapı türleri örtülü olarak tür devralmasına `object`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="4ba64-246">Bir arabirim türü genel işlev üyeleri adlandırılmış bir dizi bir sözleşmeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="4ba64-247">Bir sınıf ya da bir arabirimi uygulayan yapı arabirimin işlev üyeleri uygulamaları sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="4ba64-248">Birden fazla temel Ara birimden arabirim devralabilir ve bir sınıf veya yapı birden fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="4ba64-249">Bir temsilci türü, belirli bir parametre listesi ve dönüş türü olan yöntemlere başvuruları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4ba64-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="4ba64-250">Temsilciler, yöntemleri değişkenine atanır ve parametre olarak geçirilen varlıklar olarak değerlendirmek mümkün kılar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="4ba64-251">Diğer dillerde bulunan işlev işaretçileri kavramı temsilcileri benzerdir ancak işlev işaretçileri, nesne yönelimli ve tür kullanımı uyumlu temsilciler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="4ba64-252">Diğer türlerle bunlar yapabildiği parametreleştirilebilir tüm destek genel türler, sınıf, yapı, arabirim ve temsilci türleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="4ba64-253">Bir sabit listesi türünde, adlandırılmış sabitler ile farklı bir türdür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="4ba64-254">Her sabit listesi türü sekiz integral türlerinden biri olması gereken bir temel türü vardır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="4ba64-255">Bir numaralandırma türünün değerleri kümesi, temel alınan tür değerleri kümesi ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="4ba64-256">C# tek ve çoklu dimensional dizilerini herhangi bir türde destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="4ba64-257">Yukarıda listelenen türlerinin aksine, dizi türleri kullanılabilmesi için önce bildirilmiş gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="4ba64-258">Bunun yerine, dizi türleri bir tür adı köşeli ayraç içine uygulayarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="4ba64-259">Örneğin, `int[]` , tek boyutlu bir dizidir `int`, `int[,]` iki boyutlu bir dizidir `int`, ve `int[][]` tek boyutlu dizi tek boyutlu bir dizidir `int`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="4ba64-260">Boş değer atanabilir türler de kullanılabilmesi için önce bildirilmiş gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="4ba64-261">Her değer atanamayan değer türü `T` karşılık gelen null yapılabilir bir tür yoktur `T?`, ek bir değeri tutabilir `null`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="4ba64-262">Örneğin, `int?` herhangi bir 32 bit tamsayı veya değeri bir arada tutan bir türdür `null`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="4ba64-263">C# tür sistemi, herhangi bir türde bir değer bir nesne olarak davranılıp şekilde birleşiktir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="4ba64-264">C# ' de her tür doğrudan veya dolaylı olarak türetir `object` sınıf türü, ve `object` tüm türlerin ultimate temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="4ba64-265">Başvuru türlerindeki değerleri nesneler olarak değer türü olarak yalnızca görüntüleyerek edilir `object`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="4ba64-266">Değer türlerinin değerleri nesneler olarak gerçekleştirerek edilir ***kutulama*** ve ***kutudan çıkarma*** operations.</span><span class="sxs-lookup"><span data-stu-id="4ba64-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="4ba64-267">Aşağıdaki örnekte, bir `int` değerinin `object` ve yeniden geri `int`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // Boxing
        int j = (int)o;        // Unboxing
    }
}
```
<span data-ttu-id="4ba64-268">Ne zaman bir değer türünün bir değer türüne dönüştürülür `object`değerini tutacak bir "kutu," olarak da adlandırılan bir nesne örneği ayrılır ve değer kutuya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="4ba64-269">Buna karşılık, bir `object` başvuru, bir değer türü için atandığında, başvurulan nesnenin doğru değer türünün bir kutusu olan bir onay yapılır ve, denetimi başarılı olursa kutudaki değer kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="4ba64-270">Değer türleri "üzerine." nesneleri dönüşebilir C# ' ın birleşik tür sistemi etkili bir şekilde anlamına gelir</span><span class="sxs-lookup"><span data-stu-id="4ba64-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="4ba64-271">Birleştirme türü kullanan genel amaçlı kitaplıkları nedeniyle `object` başvuru türleri ve değer türleri ile kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="4ba64-272">Birkaç türü vardır, ***değişkenleri*** C# ' ta alanlar, dizi öğeleri, yerel değişkenleri ve parametreleri dahil.</span><span class="sxs-lookup"><span data-stu-id="4ba64-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="4ba64-273">Değişkenleri temsil eden, depolama konumları ve her değişken değerlerin neler olması belirleyen bir türe sahip aşağıdaki tabloda gösterildiği gibi değişkeninde depolanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="4ba64-274">__Değişken türü__</span><span class="sxs-lookup"><span data-stu-id="4ba64-274">__Type of Variable__</span></span>    | <span data-ttu-id="4ba64-275">__Olası içeriği__</span><span class="sxs-lookup"><span data-stu-id="4ba64-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="4ba64-276">NULL olmayan değer türü</span><span class="sxs-lookup"><span data-stu-id="4ba64-276">Non-nullable value type</span></span> | <span data-ttu-id="4ba64-277">Bu kesin türde bir değer</span><span class="sxs-lookup"><span data-stu-id="4ba64-277">A value of that exact type</span></span> |
| <span data-ttu-id="4ba64-278">Null değer türü</span><span class="sxs-lookup"><span data-stu-id="4ba64-278">Nullable value type</span></span>     | <span data-ttu-id="4ba64-279">Bir null değer veya bu kesin türde bir değer</span><span class="sxs-lookup"><span data-stu-id="4ba64-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="4ba64-280">Bir null başvuru, bir nesneye bir başvuru türünün bir başvuru veya herhangi bir değer türünün kutulanmış bir değer başvurusu</span><span class="sxs-lookup"><span data-stu-id="4ba64-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="4ba64-281">Sınıf türü</span><span class="sxs-lookup"><span data-stu-id="4ba64-281">Class type</span></span>              | <span data-ttu-id="4ba64-282">Bir null başvuru, bu sınıf türünün bir örneğine başvuru veya bir başvuru sınıfının bir örneği, sınıf türünden türetilmiş</span><span class="sxs-lookup"><span data-stu-id="4ba64-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="4ba64-283">Arabirim türü</span><span class="sxs-lookup"><span data-stu-id="4ba64-283">Interface type</span></span>          | <span data-ttu-id="4ba64-284">Bir null başvuru, bu arabirim türünü uygulayıp bir sınıf türünün bir örneğine başvuru veya paketlenmiş değere bu arabirim türünü uygulayıp bir değer türü başvurusu</span><span class="sxs-lookup"><span data-stu-id="4ba64-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="4ba64-285">Dizi türü</span><span class="sxs-lookup"><span data-stu-id="4ba64-285">Array type</span></span>              | <span data-ttu-id="4ba64-286">Bir null başvuru, dizi türü bir örneğe bir başvuru veya uyumlu dizi türünde bir örneğe bir başvuru</span><span class="sxs-lookup"><span data-stu-id="4ba64-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="4ba64-287">Temsilci türü</span><span class="sxs-lookup"><span data-stu-id="4ba64-287">Delegate type</span></span>           | <span data-ttu-id="4ba64-288">Bu temsilci türünün bir örneği başvurusu veya bir null başvuru</span><span class="sxs-lookup"><span data-stu-id="4ba64-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="4ba64-289">İfadeler</span><span class="sxs-lookup"><span data-stu-id="4ba64-289">Expressions</span></span>

<span data-ttu-id="4ba64-290">***İfadeleri*** oluşturulan ***işlenenler*** ve ***işleçleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="4ba64-291">İfade işleçleri işlenenlere uygulamak için hangi işlemleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="4ba64-292">İşleçler örnekler `+`, `-`, `*`, `/`, ve `new`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="4ba64-293">Değişmez değerler, alanlar, yerel değişkenleri ve ifadeleri işlenenler örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="4ba64-294">Bir ifade birden çok işleç içeren ***öncelik*** işleçleri tek tek işleçler değerlendirilme sırası denetler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="4ba64-295">Örneğin, ifade `x + y * z` değerlendirmesinde `x + (y * z)` çünkü `*` işleci daha yüksek önceliğe sahip `+` işleci.</span><span class="sxs-lookup"><span data-stu-id="4ba64-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="4ba64-296">Çoğu işleçleri olabilir ***aşırı***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="4ba64-297">İşleç aşırı yüklemesi, aşağıdakilerden birini veya her iki işlenen kullanıcı tanımlı sınıf veya yapı türü olduğu işlemlerinde belirtilmesi için kullanıcı tanımlı işleç uygulamalarına izin verir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="4ba64-298">En yüksek öncelikten en düşüğe sırasını işleci kategorileri listeleme C# işleçleri, aşağıdaki tabloda özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="4ba64-299">Aynı kategoride işleçleri eşit önceliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="4ba64-300">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="4ba64-300">__Category__</span></span>                     | <span data-ttu-id="4ba64-301">__İfade__</span><span class="sxs-lookup"><span data-stu-id="4ba64-301">__Expression__</span></span>    | <span data-ttu-id="4ba64-302">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="4ba64-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="4ba64-303">Birincil</span><span class="sxs-lookup"><span data-stu-id="4ba64-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="4ba64-304">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="4ba64-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="4ba64-305">Yöntem ve temsilci çağırma</span><span class="sxs-lookup"><span data-stu-id="4ba64-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="4ba64-306">Dizi ve dizinleyici erişimi</span><span class="sxs-lookup"><span data-stu-id="4ba64-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="4ba64-307">Artırım sonrası</span><span class="sxs-lookup"><span data-stu-id="4ba64-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="4ba64-308">Azaltım sonrası</span><span class="sxs-lookup"><span data-stu-id="4ba64-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="4ba64-309">Nesne ve temsilci oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ba64-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="4ba64-310">Başlatıcı ile nesne oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ba64-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="4ba64-311">Anonim nesne Başlatıcı</span><span class="sxs-lookup"><span data-stu-id="4ba64-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="4ba64-312">Dizi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4ba64-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="4ba64-313">Elde `System.Type` nesnesi `T`</span><span class="sxs-lookup"><span data-stu-id="4ba64-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="4ba64-314">İşaretli bağlamında ifade değerlendirme</span><span class="sxs-lookup"><span data-stu-id="4ba64-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="4ba64-315">İşaretlenmemiş bağlamında ifade değerlendirme</span><span class="sxs-lookup"><span data-stu-id="4ba64-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="4ba64-316">Türünün varsayılan değerini alın `T`</span><span class="sxs-lookup"><span data-stu-id="4ba64-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="4ba64-317">Anonim işlevi (anonim yöntemi)</span><span class="sxs-lookup"><span data-stu-id="4ba64-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="4ba64-318">Birli</span><span class="sxs-lookup"><span data-stu-id="4ba64-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="4ba64-319">Kimlik</span><span class="sxs-lookup"><span data-stu-id="4ba64-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="4ba64-320">Olumsuzlama</span><span class="sxs-lookup"><span data-stu-id="4ba64-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="4ba64-321">Mantıksal olumsuzlama</span><span class="sxs-lookup"><span data-stu-id="4ba64-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="4ba64-322">Bitwise olumsuzlama</span><span class="sxs-lookup"><span data-stu-id="4ba64-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="4ba64-323">Artırım öncesi</span><span class="sxs-lookup"><span data-stu-id="4ba64-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="4ba64-324">Azaltım öncesi</span><span class="sxs-lookup"><span data-stu-id="4ba64-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="4ba64-325">Açıkça dönüştürmek `x` yazmak için `T`</span><span class="sxs-lookup"><span data-stu-id="4ba64-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="4ba64-326">Zaman uyumsuz olarak bekleyin `x` tamamlamak için</span><span class="sxs-lookup"><span data-stu-id="4ba64-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="4ba64-327">Çarpma</span><span class="sxs-lookup"><span data-stu-id="4ba64-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="4ba64-328">Çarpma</span><span class="sxs-lookup"><span data-stu-id="4ba64-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="4ba64-329">Bölme</span><span class="sxs-lookup"><span data-stu-id="4ba64-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="4ba64-330">Kalan</span><span class="sxs-lookup"><span data-stu-id="4ba64-330">Remainder</span></span> |
| <span data-ttu-id="4ba64-331">Eklenebilir</span><span class="sxs-lookup"><span data-stu-id="4ba64-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="4ba64-332">Toplama, dize bitiştirme, temsilci birleşimi</span><span class="sxs-lookup"><span data-stu-id="4ba64-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="4ba64-333">Çıkarma, temsilci kaldırma</span><span class="sxs-lookup"><span data-stu-id="4ba64-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="4ba64-334">Shift</span><span class="sxs-lookup"><span data-stu-id="4ba64-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="4ba64-335">Sola kaydırma</span><span class="sxs-lookup"><span data-stu-id="4ba64-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="4ba64-336">Sağa kaydırma</span><span class="sxs-lookup"><span data-stu-id="4ba64-336">Shift right</span></span> |
| <span data-ttu-id="4ba64-337">İlişkisel ve tür testi</span><span class="sxs-lookup"><span data-stu-id="4ba64-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="4ba64-338">Küçüktür</span><span class="sxs-lookup"><span data-stu-id="4ba64-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="4ba64-339">Büyüktür</span><span class="sxs-lookup"><span data-stu-id="4ba64-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="4ba64-340">Küçük veya eşittir</span><span class="sxs-lookup"><span data-stu-id="4ba64-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="4ba64-341">Büyük veya eşittir</span><span class="sxs-lookup"><span data-stu-id="4ba64-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="4ba64-342">Dönüş `true` varsa `x` olduğu bir `T`, `false` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="4ba64-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="4ba64-343">Dönüş `x` olarak yazılan `T`, veya `null` varsa `x` değil bir `T`</span><span class="sxs-lookup"><span data-stu-id="4ba64-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="4ba64-344">Eşitlik</span><span class="sxs-lookup"><span data-stu-id="4ba64-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="4ba64-345">Eşittir</span><span class="sxs-lookup"><span data-stu-id="4ba64-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="4ba64-346">Eşit değildir</span><span class="sxs-lookup"><span data-stu-id="4ba64-346">Not equal</span></span> |
| <span data-ttu-id="4ba64-347">Mantıksal VE</span><span class="sxs-lookup"><span data-stu-id="4ba64-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="4ba64-348">Tamsayı bitwise ve, boolean mantıksal ve</span><span class="sxs-lookup"><span data-stu-id="4ba64-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="4ba64-349">Mantıksal XOR</span><span class="sxs-lookup"><span data-stu-id="4ba64-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="4ba64-350">Tamsayı bitwise XOR, Boolean mantıksal XOR</span><span class="sxs-lookup"><span data-stu-id="4ba64-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="4ba64-351">Mantıksal VEYA</span><span class="sxs-lookup"><span data-stu-id="4ba64-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="4ba64-352">Tamsayı bitwise VEYA, boolean mantıksal VEYA</span><span class="sxs-lookup"><span data-stu-id="4ba64-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="4ba64-353">Koşullu VE</span><span class="sxs-lookup"><span data-stu-id="4ba64-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="4ba64-354">Değerlendirilen `y` yalnızca `x` olduğu `true`</span><span class="sxs-lookup"><span data-stu-id="4ba64-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="4ba64-355">Koşullu VEYA</span><span class="sxs-lookup"><span data-stu-id="4ba64-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="4ba64-356">Değerlendirilen `y` yalnızca `x` olduğu `false`</span><span class="sxs-lookup"><span data-stu-id="4ba64-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="4ba64-357">Null birleşim</span><span class="sxs-lookup"><span data-stu-id="4ba64-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="4ba64-358">Değerlendiren `y` varsa `x` olduğu `null`, `x` Aksi takdirde</span><span class="sxs-lookup"><span data-stu-id="4ba64-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="4ba64-359">Koşullu</span><span class="sxs-lookup"><span data-stu-id="4ba64-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="4ba64-360">Değerlendirir `y` varsa `x` olduğu `true`, `z` varsa `x` olduğu `false`</span><span class="sxs-lookup"><span data-stu-id="4ba64-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="4ba64-361">Atama ve anonim işlev</span><span class="sxs-lookup"><span data-stu-id="4ba64-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="4ba64-362">Atama</span><span class="sxs-lookup"><span data-stu-id="4ba64-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="4ba64-363">Bileşik atama; desteklenen işleçler şunlardır: `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="4ba64-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="4ba64-364">Anonim işlevi (lambda ifadesi)</span><span class="sxs-lookup"><span data-stu-id="4ba64-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="4ba64-365">Deyimler</span><span class="sxs-lookup"><span data-stu-id="4ba64-365">Statements</span></span>

<span data-ttu-id="4ba64-366">Bir programın eylemleri kullanılarak ifade edilir ***deyimleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="4ba64-367">C# ifadeleri açısından katıştırılmış deyimi bir dizi tanımlanmış birkaç farklı türde destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="4ba64-368">A ***blok*** bağlamlarda yazılacak birden çok deyime burada tek bir deyimde izin verir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="4ba64-369">Sınırlayıcılar yazılan deyimlerin listesini bir blok oluşan `{` ve `}`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="4ba64-370">***Bildirim deyimleri*** yerel değişkenleri ve sabitleri bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="4ba64-371">***İfade deyimleri*** ifadeler değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="4ba64-372">Deyimleri kullanılabilir ifadeler, yöntem çağrılarını içeren nesne ayırmaları kullanarak `new` işleç, atamaları kullanarak `=` ve bileşik atama işleçleri, artırma ve azaltma işlemlerini kullanarak`++`ve `--` işleçler ve ifadeler bekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="4ba64-373">***Seçim deyimleri*** olası deyimleri yürütme bazı ifadesinin değerine göre bir dizi birini seçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="4ba64-374">Bu gruba `if` ve `switch` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="4ba64-375">***Yineleme deyimleri*** sürekli bir katıştırılmış deyimi yürütmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="4ba64-376">Bu gruba `while`, `do`, `for`, ve `foreach` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="4ba64-377">***Atlama deyimleri*** denetim aktarmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="4ba64-378">Bu gruba `break`, `continue`, `goto`, `throw`, `return`, ve `yield` deyimleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="4ba64-379">`try`... `catch` deyimi, bir blok yürütülmesi sırasında oluşan özel durumları yakalamak için kullanılır ve `try`... `finally` deyimi veya bir özel durum oluştu olup olmadığını her zaman yürütülür, sonlandırma kodu belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="4ba64-380">`checked` Ve `unchecked` deyimleri bağlam Tamsayı türünde aritmetik işlemler ve dönüştürmeler için taşma denetimini için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="4ba64-381">`lock` Deyimi belirli bir nesne için karşılıklı dışlama kilidini almak, bir deyimi yürütün ve sonra kilidi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="4ba64-382">`using` Deyimi bir kaynağı almak, bir deyimi yürütün ve sonra o kaynağını atma için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="4ba64-383">Her deyim türü örnekleri aşağıda verilmiştir</span><span class="sxs-lookup"><span data-stu-id="4ba64-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="4ba64-384">__Yerel değişken bildirimleri__</span><span class="sxs-lookup"><span data-stu-id="4ba64-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="4ba64-385">__Yerel sabit bildiriminde__</span><span class="sxs-lookup"><span data-stu-id="4ba64-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="4ba64-386">__ifade deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="4ba64-387">__`if` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-387">__`if` statement__</span></span>

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


<span data-ttu-id="4ba64-388">__`switch` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-388">__`switch` statement__</span></span>

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

<span data-ttu-id="4ba64-389">__`while` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="4ba64-390">__`do` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="4ba64-391">__`for` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="4ba64-392">__`foreach` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="4ba64-393">__`break` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="4ba64-394">__`continue` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="4ba64-395">__`goto` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-395">__`goto` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

<span data-ttu-id="4ba64-396">__`return` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="4ba64-397">__`yield` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-397">__`yield` statement__</span></span>

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

<span data-ttu-id="4ba64-398">__`throw` ve `try` deyimleri__</span><span class="sxs-lookup"><span data-stu-id="4ba64-398">__`throw` and `try` statements__</span></span>

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

<span data-ttu-id="4ba64-399">__`checked` ve `unchecked` deyimleri__</span><span class="sxs-lookup"><span data-stu-id="4ba64-399">__`checked` and `unchecked` statements__</span></span>

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // Exception
    }
    unchecked {
        Console.WriteLine(i + 1);        // Overflow
    }
}
```

<span data-ttu-id="4ba64-400">__`lock` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-400">__`lock` statement__</span></span>

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

<span data-ttu-id="4ba64-401">__`using` Deyimi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="4ba64-402">Sınıflar ve nesneler</span><span class="sxs-lookup"><span data-stu-id="4ba64-402">Classes and objects</span></span>

<span data-ttu-id="4ba64-403">***Sınıflar*** olan en temel C# türü.</span><span class="sxs-lookup"><span data-stu-id="4ba64-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="4ba64-404">Bir sınıf durumu (alanlar) ve işlemleri (yöntemler ve diğer işlev üyeleri) bir araya getiren bir veri yapısı içinde tek bir birimdir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="4ba64-405">Dinamik olarak oluşturulan için bir sınıf tanımı sağlar ***örnekleri*** olarak da bilinen, sınıfın ***nesneleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="4ba64-406">Destek sınıfları ***devralma*** ve ***çok biçimlilik***, mekanizmaları yapabildiği ***türetilmiş sınıflar*** genişletmek ve uzmanlaşmış ***temel sınıflar***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="4ba64-407">Yeni sınıflar, sınıf bildirimi kullanarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-407">New classes are created using class declarations.</span></span> <span data-ttu-id="4ba64-408">Bir sınıf bildiriminin öznitelikleri ve sınıfı değiştiricileri, sınıf, taban sınıf (belirtilmişse) ve bir sınıf tarafından uygulanan arabirimler adını belirten bir üst bilgisi ile başlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="4ba64-409">Başlık sınırlayıcılar arasında yazılan üye bildirimleri listesini içeren sınıf gövdesinin arkasından `{` ve `}`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="4ba64-410">Adlı basit bir sınıf bildirimi verilmiştir `Point`:</span><span class="sxs-lookup"><span data-stu-id="4ba64-410">The following is a declaration of a simple class named `Point`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="4ba64-411">Sınıfların örneklerini kullanılarak oluşturulur `new` yeni bir örneği için bellek ayırır, işleci örneği başlatmak için bir oluşturucu çağırır ve örneğe bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="4ba64-412">Aşağıdaki deyimleri iki oluşturma `Point` nesneleri ve bu nesnelere başvurular iki değişken depolayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4ba64-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="4ba64-413">Nesne artık kullanımda olmadığında otomatik olarak bir nesnenin kapladığı belleği geri kazanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="4ba64-414">Bu, gerekli veya C# nesneler açıkça serbest mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="4ba64-415">Üyeler</span><span class="sxs-lookup"><span data-stu-id="4ba64-415">Members</span></span>

<span data-ttu-id="4ba64-416">Bir sınıf üyesi ya da olan ***statik üyeleri*** veya ***örnek üyeleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="4ba64-417">Statik üyeleri sınıflarına ait ve örnek üyeleri (sınıfların örneklerini) nesnelere ait.</span><span class="sxs-lookup"><span data-stu-id="4ba64-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="4ba64-418">Aşağıdaki tabloda, tür üyeleri bir sınıf içeren genel bir bakış sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="4ba64-419">__Üyesi__</span><span class="sxs-lookup"><span data-stu-id="4ba64-419">__Member__</span></span>   | <span data-ttu-id="4ba64-420">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="4ba64-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="4ba64-421">Sabitler</span><span class="sxs-lookup"><span data-stu-id="4ba64-421">Constants</span></span>    | <span data-ttu-id="4ba64-422">Sınıf ile ilişkili olan sabit değerler</span><span class="sxs-lookup"><span data-stu-id="4ba64-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="4ba64-423">Alanlar</span><span class="sxs-lookup"><span data-stu-id="4ba64-423">Fields</span></span>       | <span data-ttu-id="4ba64-424">Sınıfın değişkenleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-424">Variables of the class</span></span> |
| <span data-ttu-id="4ba64-425">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="4ba64-425">Methods</span></span>      | <span data-ttu-id="4ba64-426">Sınıfı tarafından gerçekleştirilen eylemler ve hesaplamaları</span><span class="sxs-lookup"><span data-stu-id="4ba64-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="4ba64-427">Özellikler</span><span class="sxs-lookup"><span data-stu-id="4ba64-427">Properties</span></span>   | <span data-ttu-id="4ba64-428">Okuma ve adlandırılmış sınıfın özelliklerini yazma ile ilişkili eylemler</span><span class="sxs-lookup"><span data-stu-id="4ba64-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="4ba64-429">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4ba64-429">Indexers</span></span>     | <span data-ttu-id="4ba64-430">Dizin oluşturma sınıfına bir dizi gibi örnekleriyle ilişkili eylemler</span><span class="sxs-lookup"><span data-stu-id="4ba64-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="4ba64-431">Olaylar</span><span class="sxs-lookup"><span data-stu-id="4ba64-431">Events</span></span>       | <span data-ttu-id="4ba64-432">Sınıfı tarafından oluşturulan bildirimleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="4ba64-433">İşleçler</span><span class="sxs-lookup"><span data-stu-id="4ba64-433">Operators</span></span>    | <span data-ttu-id="4ba64-434">Dönüşümler ve sınıfı tarafından desteklenen ifade işleçleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="4ba64-435">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4ba64-435">Constructors</span></span> | <span data-ttu-id="4ba64-436">Sınıfın veya sınıf örneği başlatmak için gerekli eylemleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="4ba64-437">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="4ba64-437">Destructors</span></span>  | <span data-ttu-id="4ba64-438">Sınıf örneğini kalıcı olarak atılmadan önce gerçekleştirilecek eylemler</span><span class="sxs-lookup"><span data-stu-id="4ba64-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="4ba64-439">Türler</span><span class="sxs-lookup"><span data-stu-id="4ba64-439">Types</span></span>        | <span data-ttu-id="4ba64-440">Sınıfı tarafından bildirilen iç içe geçmiş türler</span><span class="sxs-lookup"><span data-stu-id="4ba64-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="4ba64-441">Erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="4ba64-441">Accessibility</span></span>

<span data-ttu-id="4ba64-442">Bir sınıfın her üyesine erişebilir üyeyi program metni bölümlerine denetleyen bir ilişkili erişilebilirlik sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="4ba64-443">Erişilebilirlik beş olası biçimi vardır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="4ba64-444">Bunlar aşağıdaki tabloda özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="4ba64-445">__Erişilebilirlik__</span><span class="sxs-lookup"><span data-stu-id="4ba64-445">__Accessibility__</span></span>    | <span data-ttu-id="4ba64-446">__Anlamı__</span><span class="sxs-lookup"><span data-stu-id="4ba64-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="4ba64-447">Olmayan sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="4ba64-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="4ba64-448">Bu sınıf veya sınıfların sınırlı erişim, bu sınıftan türetilen</span><span class="sxs-lookup"><span data-stu-id="4ba64-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="4ba64-449">Bu program için sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="4ba64-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="4ba64-450">Bu program için sınırlı erişim veya bu sınıftan türetilmiş sınıflar</span><span class="sxs-lookup"><span data-stu-id="4ba64-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="4ba64-451">Bu sınıf için sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="4ba64-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="4ba64-452">Tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-452">Type parameters</span></span>

<span data-ttu-id="4ba64-453">Bir sınıf tanımı, sınıf adı türü parametre adları listesini çevreleyen açılı ayraçlar ile izleyerek tür parametrelerinin kümesi belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ba64-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="4ba64-454">Tür parametreleri için sınıf üyelerini tanımlamak için sınıf bildirimi gövdesinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="4ba64-455">Aşağıdaki örnekte, tür parametreleri `Pair` olan `TFirst` ve `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="4ba64-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="4ba64-456">Tür parametreleri gerçekleştirilecek bildirildiği bir sınıf türü bir genel sınıf türü olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="4ba64-457">Yapı, arabirim ve temsilci türlerinin genel olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="4ba64-458">Genel sınıf kullanıldığında, tür bağımsız değişkenleri her tür parametreleri için sağlanmalıdır:</span><span class="sxs-lookup"><span data-stu-id="4ba64-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="4ba64-459">Sağlanan gibi tür bağımsız değişkenleri ile genel tür `Pair<int,string>` yukarıda oluşturulan tür adı verilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-459">A generic type with type arguments provided, like `Pair<int,string>` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="4ba64-460">Temel sınıflar</span><span class="sxs-lookup"><span data-stu-id="4ba64-460">Base classes</span></span>

<span data-ttu-id="4ba64-461">Sınıf bildiriminin bir temel sınıf, bir iki nokta üst üste ve temel sınıfın adını sınıf adı ve türü parametreleri izleyerek belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ba64-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="4ba64-462">Bir temel sınıf belirtimini atlama aynıdır türünden türetme `object`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="4ba64-463">Aşağıdaki örnekte, temel sınıfını `Point3D` olduğu `Point`ve temel sınıfını `Point` olduğu `object`:</span><span class="sxs-lookup"><span data-stu-id="4ba64-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
<span data-ttu-id="4ba64-464">Bir sınıf, taban sınıfı üyelerini devralır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="4ba64-465">Devralma, bir sınıf dolaylı temel sınıfı, örneği ve statik oluşturucular ve Yıkıcılar temel sınıfın dışındaki tüm üyeleri içeren anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="4ba64-466">Türetilmiş bir sınıf devralır bu yeni üyeler ekleyebilirsiniz, ancak devralınan bir üyeyi tanımını kaldırılamaz.</span><span class="sxs-lookup"><span data-stu-id="4ba64-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="4ba64-467">Önceki örnekte, `Point3D` devralan `x` ve `y` alanlarını `Point`ve her `Point3D` örneğini içeren üç alan `x`, `y`, ve `z`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="4ba64-468">Temel sınıf türlerinden birinin bir sınıf türünden örtük bir dönüştürme yok.</span><span class="sxs-lookup"><span data-stu-id="4ba64-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="4ba64-469">Bu nedenle, bir sınıf türünün bir değişkeni, bu sınıfın bir örneğini veya türetilmiş bir sınıf örneği başvuruda bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="4ba64-470">Örneğin, önceki bir sınıf bildirimleri, türünde bir değişken verilen `Point` ya da başvurabilirsiniz bir `Point` veya `Point3D`:</span><span class="sxs-lookup"><span data-stu-id="4ba64-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="4ba64-471">Alanlar</span><span class="sxs-lookup"><span data-stu-id="4ba64-471">Fields</span></span>

<span data-ttu-id="4ba64-472">Bir alanın bir sınıf veya bir sınıf örneği ile ilişkili bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="4ba64-473">Bir alana bildirilen `static` değiştiricisi tanımlayan bir ***statik alan***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="4ba64-474">Statik alan tam olarak bir depolama konumunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="4ba64-475">Bir sınıfın kaç örneklerin oluşturulduğu ne olursa olsun, yalnızca bir kopyasını statik bir alan yoktur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="4ba64-476">Bir alan olmadan bildirilen `static` değiştiricisi tanımlayan bir ***örnek alanı***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="4ba64-477">Bir sınıfın her örneği, bu sınıfın tüm örnek alanları ayrı bir kopyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="4ba64-478">Aşağıdaki örnekte, her bir örneği `Color` sınıfına sahip ayrı bir kopyasını `r`, `g`, ve `b` örnek alanları, ancak yalnızca bir kopyası `Black`, `White`, `Red`, `Green`, ve `Blue` statik alanlar:</span><span class="sxs-lookup"><span data-stu-id="4ba64-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
<span data-ttu-id="4ba64-479">Önceki örnekte gösterilen şekilde ***salt okunur alanları*** ile bildirilebilir bir `readonly` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="4ba64-480">Atama bir `readonly` alan alanın bildirimin veya aynı sınıftaki bir oluşturucunun parçası olarak yalnızca oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="4ba64-481">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="4ba64-481">Methods</span></span>

<span data-ttu-id="4ba64-482">A ***yöntemi*** hesaplama veya bir nesne veya sınıf tarafından gerçekleştirilen eylem uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="4ba64-483">***Statik yöntemler*** sınıfı üzerinden erişilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="4ba64-484">***Örnek yöntemleri*** sınıfının örneklerini erişilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="4ba64-485">Yöntemleri (büyük olasılıkla boş) bir listesi olan ***parametreleri***, değerleri veya değişken başvuruları yöntemine geçirilen temsil ve ***dönüş türü***, hesaplanan ve tarafından döndürülen değerin türü belirtir yöntem.</span><span class="sxs-lookup"><span data-stu-id="4ba64-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="4ba64-486">Bir yöntemin dönüş türü `void` bir değer döndürmezse.</span><span class="sxs-lookup"><span data-stu-id="4ba64-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="4ba64-487">Türleri gibi yöntemleri de bir dizi yöntem çağrıldığında tür bağımsız değişkenleri belirtilmiş olmalıdır, tür parametreleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="4ba64-488">Türleri, farklı tür bağımsız değişkeni genellikle bir yöntem çağrısının bağımsız değişkenlerden çıkarılan ve açıkça verilmemiş.</span><span class="sxs-lookup"><span data-stu-id="4ba64-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="4ba64-489">***İmza*** yöntemi yöntemi bildirilmiş sınıfında benzersiz olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="4ba64-490">Yöntemin imzası yöntem, tür parametreleri ve sayısı, değiştiriciler ve parametrelerinin türleri sayısı adını oluşur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="4ba64-491">Yöntemin imzası dönüş türü içermez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="4ba64-492">Parametreler</span><span class="sxs-lookup"><span data-stu-id="4ba64-492">Parameters</span></span>

<span data-ttu-id="4ba64-493">Parametre değerleri veya değişken başvuruları yöntemlere geçirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="4ba64-494">Bir yöntem parametreleri, gerçek değerleri alma ***bağımsız değişkenleri*** yöntemi çağrıldığında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="4ba64-495">Dört tür parametrelerinin vardır: parametreler, başvuru parametreleri, çıktı parametreleri ve parametre dizileri değeri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="4ba64-496">A ***değer parametresi*** giriş parametre geçirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="4ba64-497">Bir değer parametresini karşılık gelen bir yerel değişkene parametresi için geçirilen bağımsız ilk değerini alır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="4ba64-498">Parametresi için geçirilen bağımsız değişken bir değer parametresini değişiklikler etkilemez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="4ba64-499">Böylece karşılık gelen bağımsız değişken atlanırsa, varsayılan bir değer belirterek değeri parametreleri, isteğe bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="4ba64-500">A ***parametre başvurusunu*** giriş ve çıkış parametre geçirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="4ba64-501">Bir başvuru parametresi için geçirilen bağımsız değişken bir değişken olmalıdır ve yöntemin yürütülmesi sırasında aynı depolama konumu olarak bir bağımsız değişken başvuru parametresi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="4ba64-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="4ba64-502">Bir başvuru parametresi ile bildirilen `ref` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="4ba64-503">Aşağıdaki örnek kullanımını gösterir `ref` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-503">The following example shows the use of `ref` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
<span data-ttu-id="4ba64-504">Bir ***çıkış parametresi*** geçirme çıkış parametresi için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="4ba64-505">Çağıran tarafından sağlanan bağımsız değişkenin ilk değeri önemli olması dışında başvuru parametresi bir output parametresi benzer.</span><span class="sxs-lookup"><span data-stu-id="4ba64-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="4ba64-506">Çıkış parametresi ile bildirilen `out` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="4ba64-507">Aşağıdaki örnek kullanımını gösterir `out` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-507">The following example shows the use of `out` parameters.</span></span>

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
<span data-ttu-id="4ba64-508">A ***parametre dizisi*** bir yönteme iletilecek bağımsız değişken bir sayı verir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="4ba64-509">Bir parametre dizisi ile bildirilen `params` değiştiricisi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="4ba64-510">Bir yöntem yalnızca son parametresi bir parametre dizisi olabilir ve bir parametre dizisi türünde tek boyutlu dizi türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="4ba64-511">`Write` Ve `WriteLine` yöntemlerinin `System.Console` sınıfı iyi parametre dizisi kullanımı örnekleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="4ba64-512">Bunlar aşağıdaki gibi bildirilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="4ba64-513">Parametre dizisi, bir parametre dizisi kullanan bir yöntem içinde bir dizi türünde tam olarak normal bir parametre gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="4ba64-514">Ancak, bir parametre dizisi olan bir yöntem çağrısını içinde parametresi dizi türünde tek bir bağımsız değişken veya herhangi bir sayıda öğe türü parametre dizisi bağımsız değişkenleri geçirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="4ba64-515">İkinci durumda, bir dizi örneği otomatik olarak oluşturulur ve belirtilen bağımsız değişkenler ile başlatıldı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="4ba64-516">Bu örnek</span><span class="sxs-lookup"><span data-stu-id="4ba64-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="4ba64-517">Aşağıdaki yazmaya eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="4ba64-518">Yöntem gövdesini ve yerel değişkenler</span><span class="sxs-lookup"><span data-stu-id="4ba64-518">Method body and local variables</span></span>

<span data-ttu-id="4ba64-519">Bir yöntemin gövdesi yöntemi çağrıldığında çalıştırılacak deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="4ba64-520">Bir yöntem gövdesi yöntemi çağırmayı için özel değişkenleri bildirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ba64-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="4ba64-521">Bu tür değişkenleri olarak adlandırılmasının ***yerel değişkenler***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="4ba64-522">Yerel bir değişken bildirimi bir tür adı, bir değişken adı ve büyük olasılıkla bir başlangıç değeri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="4ba64-523">Aşağıdaki örnek, yerel bir değişken bildirir `i` bir başlangıç değeri sıfır ve yerel bir değişken ile `j` ile başlangıç değeri yok.</span><span class="sxs-lookup"><span data-stu-id="4ba64-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
<span data-ttu-id="4ba64-524">C# olarak yerel bir değişken gerektirir ***kesinlikle atanan*** önce değeri elde edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="4ba64-525">Örneğin, önceki bildirimi `i` bir başlangıç değeri içermesi gerekmez, derleyici bir hata sonraki kullanımlar için rapor `i` çünkü `i` kesinlikle noktalarda program atanır değil.</span><span class="sxs-lookup"><span data-stu-id="4ba64-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="4ba64-526">Bir yöntemi kullanabilirsiniz `return` denetimi onu arayan döndürülecek deyimleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="4ba64-527">Döndüren bir yöntem içinde `void`, `return` deyimleri, bir ifade belirtemez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="4ba64-528">Olmayan döndüren bir yöntem içinde`void`, `return` deyimleri, dönüş değeri hesaplar bir ifade içermelidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="4ba64-529">Statik ve örnek yöntemleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-529">Static and instance methods</span></span>

<span data-ttu-id="4ba64-530">Bir yöntem ile bildirilen bir `static` değiştiricisi bir ***statik yöntem***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="4ba64-531">Statik bir yöntemi, belirli bir örneği üzerinde çalışmaz ve statik üyeler yalnızca doğrudan erişebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="4ba64-532">Bir yöntem olmadan bildirilen bir `static` değiştiricisi bir ***örnek yöntemi***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="4ba64-533">Bir örnek yöntemi belirli bir örneği üzerinde çalışır ve hem statik erişebilir ve örnek üyeler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="4ba64-534">Bir örnek yöntemi çağrıldığı örnek olarak açıkça erişilebilir `this`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="4ba64-535">Başvurmak için bir hata olduğunu `this` statik yöntemde.</span><span class="sxs-lookup"><span data-stu-id="4ba64-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="4ba64-536">Aşağıdaki `Entity` sınıfında statik ve örnek üyeleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-536">The following `Entity` class has both static and instance members.</span></span>

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
<span data-ttu-id="4ba64-537">Her `Entity` örneği seri numarası (ve burada gösterilmez büyük olasılıkla bazı diğer bilgileri içerir).</span><span class="sxs-lookup"><span data-stu-id="4ba64-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="4ba64-538">`Entity` (Olan bir örnek yöntemi gibi) bir oluşturucu sonraki kullanılabilir seri numarasına sahip yeni örneğini başlatır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="4ba64-539">Oluşturucu bir örnek üyesi olduğundan, her ikisi de erişmesine izin verilen `serialNo` örnek alan ve `nextSerialNo` statik alan.</span><span class="sxs-lookup"><span data-stu-id="4ba64-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="4ba64-540">`GetNextSerialNo` Ve `SetNextSerialNo` statik yöntemler erişebilir `nextSerialNo` statik alan, ancak bir hata için bunları doğrudan erişimini olacak `serialNo` örnek alanı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="4ba64-541">Aşağıdaki örnek kullanımını gösterir `Entity` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-541">The following example shows the use of the `Entity` class.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
<span data-ttu-id="4ba64-542">Unutmayın `SetNextSerialNo` ve `GetNextSerialNo` statik yöntemler, sınıf üzerinde çağrılır, ancak `GetSerialNo` sınıf örnekleri üzerinde örnek yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="4ba64-543">Soyut metotlar sanal ve geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="4ba64-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="4ba64-544">Ne zaman bir örnek yöntemi bildirimi içeren bir `virtual` değiştiricisi, yöntem olarak kabul edilir bir ***sanal yöntem***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="4ba64-545">Hiçbir `virtual` değiştirici mevcut ise, yöntem olarak kabul edilir bir ***sanal olmayan yöntemiyle***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="4ba64-546">Sanal bir yöntem çağrıldığında ***çalışma zamanı tür*** bu çağırma aldığı örneğini yerde çağrılacak gerçek yöntem uygulaması belirler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="4ba64-547">Bir sanal olmayan bir yöntem çağrısı ***derleme zamanı tür*** belirleyici faktör örneğidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="4ba64-548">Sanal bir yöntem olabilir ***geçersiz kılınan*** türetilen bir sınıfta.</span><span class="sxs-lookup"><span data-stu-id="4ba64-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="4ba64-549">Ne zaman bir örnek yöntemi bildirimi içeren bir `override` değiştiricisi, yöntemin aynı imzaya sahip devralınan sanal yöntemi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="4ba64-550">Bir geçersiz kılma yöntemi bildirimini sanal yöntem bildiriminde bir yöntem sunar ancak bu yöntem yeni bir uygulamasını sağlayarak mevcut devralınan sanal yöntemi uzmanlaşmış.</span><span class="sxs-lookup"><span data-stu-id="4ba64-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="4ba64-551">Bir ***soyut*** sanal bir yöntem uygulama içermeyen bir yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="4ba64-552">Soyut bir yöntem ile bildirilen `abstract` değiştiricisi ve ayrıca bildirilen bir sınıfta izin `abstract`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="4ba64-553">Her bir soyut olmayan türetilmiş sınıf içinde soyut bir yöntemi geçersiz kılınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="4ba64-554">Aşağıdaki örnek, bir soyut sınıfı bildirir `Expression`, bir ifade ağacı düğümünü temsil eder ve üç türetilmiş sınıfları `Constant`, `VariableReference`, ve `Operation`, sabitler, değişken için ifade ağaç düğümleri uygulayın başvurular ve aritmetik işlemler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="4ba64-555">(Bu benzer, ancak ile karıştırılmamalıdır ifade ağacı türleri sürümünde [ifade ağacı türleri](types.md#expression-tree-types)).</span><span class="sxs-lookup"><span data-stu-id="4ba64-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
<span data-ttu-id="4ba64-556">Önceki dört sınıfları, aritmetik ifadeler modellemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="4ba64-557">Örneğin, ifade bu sınıfların örnekleri kullanarak `x + 3` şu şekilde temsil edilebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="4ba64-558">`Evaluate` Yöntemi bir `Expression` örneği belirtilen ifadeyi değerlendirir ve üretmek için çağrıldığında bir `double` değeri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="4ba64-559">Yöntem bağımsız değişken olarak alır bir `Hashtable` (anahtar olarak girişlerinin) değişken adları ve değerleri (olarak girdilerinin değerlerini) içeren.</span><span class="sxs-lookup"><span data-stu-id="4ba64-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="4ba64-560">`Evaluate` Yöntemdir, türetilmiş soyut olmayan sınıflar, gerçek bir uygulama sağlamak için kılmalı yani sanal soyut bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="4ba64-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="4ba64-561">A `Constant`'s uygulaması `Evaluate` yalnızca saklı sabiti döndürür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="4ba64-562">A `VariableReference`hashtable değişken adında uygulama şuna kullanıcının ve sonuçta elde edilen değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="4ba64-563">Bir `Operation`ait uygulama ilk sol ve sağ işlenen değerlendirilir (yinelemeli olarak çağırarak kendi `Evaluate` yöntemleri) ve ardından belirli bir aritmetik işlemi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="4ba64-564">Aşağıdaki program kullanan `Expression` ifadeyi değerlendiren şeyde sınıfları `x * (y + 2)` farklı değerler için `x` ve `y`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### <a name="method-overloading"></a><span data-ttu-id="4ba64-565">Yöntemi aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="4ba64-565">Method overloading</span></span>

<span data-ttu-id="4ba64-566">Yöntemi ***aşırı yükleme*** aynı sınıfta benzersiz imzaları sahip oldukları sürece aynı ada sahip birden çok yöntem izin verir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="4ba64-567">Aşırı yüklenmiş yöntem çağrısından derlerken, derleyicinin kullandığı ***aşırı yükleme çözümlemesi*** çağrılacak belirli yöntemi belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="4ba64-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="4ba64-568">En iyi eşleşen bağımsız değişkenler veya tek en iyi eşleşme bulunamazsa, bir hata raporları bir yöntemi aşırı yükleme çözünürlüğü bulur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="4ba64-569">Aşağıdaki örnek, aşırı yükleme çözünürlüğü geçerli gösterir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="4ba64-570">Her bir çağrıda yorum `Main` yöntemi gösterir hangi yöntemi gerçekten çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
<span data-ttu-id="4ba64-571">Örnekte gösterildiği gibi belirli bir yöntem her zaman tam parametre türleri bağımsız değişkenleri açıkça atama ve/veya tür bağımsız değişkenleri açıkça sağlama tarafından seçilebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="4ba64-572">Diğer işlev üyeleri</span><span class="sxs-lookup"><span data-stu-id="4ba64-572">Other function members</span></span>

<span data-ttu-id="4ba64-573">Yürütülebilir kod içeren bir üye olarak bilinir topluca ***işlev üyeleri*** bir sınıf.</span><span class="sxs-lookup"><span data-stu-id="4ba64-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="4ba64-574">Birincil işlev üyeleri türü olan yöntemler, önceki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="4ba64-575">Bu bölümde, işlev üyeleri C# tarafından desteklenen diğer türleri açıklanmaktadır: Oluşturucular, özellikler, dizin oluşturucular, olaylar, işleçleri ve Yıkıcılar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="4ba64-576">Adlı bir genel sınıf aşağıdaki kodda gösterildiği `List<T>`, growable nesnelerin listesini uygular.</span><span class="sxs-lookup"><span data-stu-id="4ba64-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="4ba64-577">Sınıf işlev üyeleri en yaygın tür çeşitli örneklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-577">The class contains several examples of the most common kinds of function members.</span></span>


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### <a name="constructors"></a><span data-ttu-id="4ba64-578">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4ba64-578">Constructors</span></span>

<span data-ttu-id="4ba64-579">C# örneği hem de statik oluşturucular destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="4ba64-580">Bir ***örnek oluşturucusu*** uygulayan bir sınıf örneği başlatmak için gerekli eylemleri bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="4ba64-581">A ***statik Oluşturucu*** ilk yüklendiğinde bir sınıf başlatmak için gerekli eylemleri uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="4ba64-582">Bir oluşturucu dönüş türü ve kapsayan sınıfı aynı ada sahip bir yöntemi gibi bildirilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="4ba64-583">Bir oluşturucu bildirimi içeriyorsa bir `static` değiştiricisi, bir statik Oluşturucu bildirir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="4ba64-584">Aksi takdirde, bir örnek oluşturucusu bildirir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="4ba64-585">Örnek oluşturucuları aşırı yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="4ba64-586">Örneğin, `List<T>` sınıfı bildirir, bir parametre ve süren iki örnek oluşturucuları bir `int` parametresi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-586">For example, the `List<T>` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="4ba64-587">Örnek oluşturucuları kullanarak çağrılır `new` işleci.</span><span class="sxs-lookup"><span data-stu-id="4ba64-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="4ba64-588">Aşağıdaki deyimleri iki ayırma `List<string>` her oluşturucular kullanma örnekleri `List` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-588">The following statements allocate two `List<string>` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="4ba64-589">Diğer üyeleri farklı olarak örnek oluşturucuları olmayan devralınır ve sınıfı, bu gerçekte bildirilen dışında hiçbir örnek oluşturucuları bir sınıfa sahip.</span><span class="sxs-lookup"><span data-stu-id="4ba64-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="4ba64-590">Hiçbir örnek oluşturucusu için bir sınıf sağlanırsa, sonra boş bir parametre olmadan otomatik olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="4ba64-591">Özellikler</span><span class="sxs-lookup"><span data-stu-id="4ba64-591">Properties</span></span>

<span data-ttu-id="4ba64-592">***Özellikleri*** alanları doğal bir uzantısıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="4ba64-593">Her ikisi de ilişkili türlerini üyeleriyle adlı ve alanlar ve Özellikler erişmek için sözdizimi aynıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="4ba64-594">Ancak, alanlar özellikleri depolama konumları belirtmek değil.</span><span class="sxs-lookup"><span data-stu-id="4ba64-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="4ba64-595">Bunun yerine, özelliklere sahip ***erişimcileri*** değerlerine okunabilir veya yazılabilir bırakıldığında yürütülecek deyimler belirtin.</span><span class="sxs-lookup"><span data-stu-id="4ba64-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="4ba64-596">Bildirimi ile biten dışında bir özellik gibi bir alan, bildirilmiş bir `get` erişimci veya `set` sınırlayıcılar arasında yazılan erişimci `{` ve `}` noktalı bitiş yerine.</span><span class="sxs-lookup"><span data-stu-id="4ba64-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="4ba64-597">Her ikisi de sahip bir özellik bir `get` erişimci ve `set` erişen bir ***okuma-yazma özelliği***, yalnızca bir özellik bir `get` erişen bir ***salt okunur özelliği***ve yalnızca özellik bir `set` erişen bir ***salt yazılır özelliği***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="4ba64-598">A `get` bir parametresiz yöntemin dönüş değerini özellik türü ile erişimci karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="4ba64-599">Bir özellik bir ifadede başvurulduğunda, atama hedefi olarak dışında `get` özelliğin erişimci özelliğin değerini hesaplamak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="4ba64-600">A `set` erişimci karşılık gelen bir yönteme adlı tek bir parametre `value` ve dönüş türü yok.</span><span class="sxs-lookup"><span data-stu-id="4ba64-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="4ba64-601">Ne zaman bir özelliği başvuru atama hedefi veya işleneni olarak `++` veya `--`, `set` erişimci, yeni değer sağlayan bağımsız değişkenlerle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="4ba64-602">`List<T>` Sınıfı bildirir iki özellik `Count` ve `Capacity`, olan salt okunur ve okuma-yazma, sırasıyla.</span><span class="sxs-lookup"><span data-stu-id="4ba64-602">The `List<T>` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="4ba64-603">Bu özelliklerin kullanımı bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="4ba64-604">Benzer şekilde, alanlar ve yöntemler, C# örnek özelliklerini hem statik özelliklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="4ba64-605">Statik özellikler ile bildirilmiş `static` değiştiricisi ve örnek özelliklerini bildirilir olmadan.</span><span class="sxs-lookup"><span data-stu-id="4ba64-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="4ba64-606">Bir özelliğin accessor(s) sanal olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="4ba64-607">Ne zaman bir özellik bildirimi içeren bir `virtual`, `abstract`, veya `override` değiştiricisi, özellik accessor(s) için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="4ba64-608">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="4ba64-608">Indexers</span></span>

<span data-ttu-id="4ba64-609">Bir ***dizin oluşturucu*** nesneleri dizisi olarak aynı şekilde dizinlenmesini sağlar bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="4ba64-610">Üyenin adını olması dışında dizin oluşturucu bir özellik gibi bildirilir `this` sınırlayıcılar arasında yazılmış bir parametre listesi ardından `[` ve `]`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="4ba64-611">Parametreler, dizin oluşturucunun accessor(s) içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="4ba64-612">Benzer şekilde, özellikler, dizin oluşturucular okuma-yazma, salt okunur ve salt yazılır olabilir ve bir dizin oluşturucu accessor(s) sanal olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="4ba64-613">`List` Sınıfı bildirir gereken tek bir okuma-yazma dizin oluşturucu bir `int` parametresi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="4ba64-614">Dizin oluşturucunun, dizin mümkün kılar `List` ile örnekler `int` değerleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="4ba64-615">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4ba64-615">For example</span></span>

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
<span data-ttu-id="4ba64-616">Dizin Oluşturucular, sayı veya kendi parametre türleri farklı olduğu sürece bir sınıfın birden çok dizin oluşturucu bildirebilirsiniz anlamına gelen aşırı yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="4ba64-617">Olaylar</span><span class="sxs-lookup"><span data-stu-id="4ba64-617">Events</span></span>

<span data-ttu-id="4ba64-618">Bir ***olay*** bir sınıf veya nesne bildirimleri sağlamak için sağlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="4ba64-619">Dışında bildirimi içeren bir olay gibi bir alan bildirilmiş bir `event` anahtar sözcüğü ile tür bir temsilci türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="4ba64-620">(Olay soyut değil ve erişimcileri bildirmiyor sağlanan) bir olay üyesi bildiren bir sınıf içinde olay yalnızca bir temsilci türüne bir alan gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="4ba64-621">Olaya eklenmiş olan olay işleyicilerini temsil eden bir temsilci bir başvuru alan depolar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="4ba64-622">Olay işleyici mevcut olması durumunda, alandır `null`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="4ba64-623">`List<T>` Sınıfı olarak adlandırılan tek bir olay üyesi bildirir `Changed`, yeni bir öğe listesine eklendiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-623">The `List<T>` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="4ba64-624">`Changed` Olayı tarafından oluşturulur `OnChanged` hangi ilk olay olup olmadığını denetler, sanal bir yöntem `null` (hiçbir işleyicileri bulunduğunu anlamına gelir).</span><span class="sxs-lookup"><span data-stu-id="4ba64-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="4ba64-625">Olay bildirmek, kavram olay tarafından temsil edilen temsilci çağırmak için tam olarak eşittir; Bu nedenle, olayları oluşturma için hiçbir özel dil yapıları vardır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="4ba64-626">İstemciler react ile olayları ***olay işleyicileri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="4ba64-627">Olay işleyicileri kullanılarak eklenen `+=` işleci ve kaldırılan kullanarak `-=` işleci.</span><span class="sxs-lookup"><span data-stu-id="4ba64-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="4ba64-628">Aşağıdaki örnek bir olay işleyicisi ekler `Changed` olayı bir `List<string>`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-628">The following example attaches an event handler to the `Changed` event of a `List<string>`.</span></span>

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
<span data-ttu-id="4ba64-629">Denetim bir olay temel alınan depolamanın istenen burada Gelişmiş senaryolar için bir olay bildirimi açıkça sağlayabilir `add` ve `remove` biraz benzerdir erişimcileri `set` özellik erişimcisi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="4ba64-630">İşleçler</span><span class="sxs-lookup"><span data-stu-id="4ba64-630">Operators</span></span>

<span data-ttu-id="4ba64-631">Bir ***işleci*** belirli ifade işleci bir sınıfın bir örneği için uygulama ne anlama geldiğini tanımlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="4ba64-632">Üç tür işleç tanımlanabilir: Birli işleçler, ikili işleçler ve dönüştürme işleçleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="4ba64-633">Tüm işleçler olarak bildirilmelidir `public` ve `static`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="4ba64-634">`List<T>` Sınıfı bildirir iki işleç `operator==` ve `operator!=`ve bu nedenle bu işleçleri için geçerli ifadeler için yeni anlamı verir `List` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-634">The `List<T>` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="4ba64-635">Özellikle, da iki eşitlik operatörlerini tanımlayın `List<T>` örnekler olarak karşılaştırma kullanarak içerdiği nesnelerin her biri kendi `Equals` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-635">Specifically, the operators define equality of two `List<T>` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="4ba64-636">Aşağıdaki örnekte `==` karşılaştırmak için işleci `List<int>` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-636">The following example uses the `==` operator to compare two `List<int>` instances.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

<span data-ttu-id="4ba64-637">İlk `Console.WriteLine` çıkarır `True` listelerini nesneleri aynı sırada aynı değerlerle aynı sayıda içerdiğinden.</span><span class="sxs-lookup"><span data-stu-id="4ba64-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="4ba64-638">Vardı `List<T>` tanımlanmamış `operator==`, ilk `Console.WriteLine` çıkış `False` çünkü `a` ve `b` başvuru farklı `List<int>` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-638">Had `List<T>` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="4ba64-639">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="4ba64-639">Destructors</span></span>

<span data-ttu-id="4ba64-640">A ***yok Edicisi*** uygulayan bir sınıf örneği yok etmek için gereken eylemler bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="4ba64-641">Yok ediciler parametrelere sahip olamaz, Erişilebilirlik değiştiricilere sahip olamaz ve açıkça çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="4ba64-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="4ba64-642">Bir örneği için yok edici, çöp toplama sırasında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="4ba64-643">Çöp Toplayıcısı nesneleri toplar ve Yıkıcılar çalıştırmak ne zaman karar içinde geniş enlem izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="4ba64-644">Özellikle, yok edici çağrıları zamanlamasını belirleyici değildir ve yok ediciler herhangi bir iş parçacığı üzerinde çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="4ba64-645">Diğer çözüm uygun olduğunda bunlar ve diğer nedenler, sınıfları yok ediciler uygulamalıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="4ba64-646">`using` Deyim nesnesi yok etme için daha iyi bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="4ba64-647">Yapılar</span><span class="sxs-lookup"><span data-stu-id="4ba64-647">Structs</span></span>

<span data-ttu-id="4ba64-648">Sınıflar gibi ***yapılar*** veri yapıları, veri üyeleri ve işlev üyeleri içerebilir, ancak farklı sınıflar, yapılar değer türleri ve yığın ayırma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="4ba64-649">Bir sınıf türünün bir değişkeni dinamik olarak ayrılan bir nesneye başvuru depoladığı bir yapı türünün değişkenini doğrudan, struct'ın verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="4ba64-650">Yapı türleri, kullanıcı tarafından belirtilen devralma desteklemez ve tüm yapı türleri örtülü olarak tür devralmasına `object`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="4ba64-651">Yapılar değer semantiklere sahip küçük veri yapıları için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="4ba64-652">Karmaşık sayılar, koordinat sisteminde noktaları veya anahtar-değer çiftlerinin bir sözlükteki tüm iyi yapılar örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="4ba64-653">Küçük veri yapıları için sınıflar, bir uygulama bellek ayırmaları sayısında büyük bir fark yapabilirsiniz yerine yapılar kullanımını gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="4ba64-654">Örneğin, aşağıdaki program oluşturur ve 100 noktaları dizisini başlatır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="4ba64-655">İle `Point` sınıfı olarak uygulanır, 101 ayrı nesneler örneği oluşturulur; bir dizi ve her biri 100 öğeleri için.</span><span class="sxs-lookup"><span data-stu-id="4ba64-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
<span data-ttu-id="4ba64-656">Alternatif olmaktır `Point` yapı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-656">An alternative is to make `Point` a struct.</span></span>

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
<span data-ttu-id="4ba64-657">Şimdi, yalnızca bir nesne örneği — bir dizi — ve `Point` depolanan satır içi dizi örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="4ba64-658">Yapı Oluşturucuları ile çağrılır `new` işleci, ancak değil yaptığından bu bellek tahsis edilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="4ba64-659">Dinamik olarak bir nesne ayırma ve buna bir başvuru döndürerek, yerine bir yapı Oluşturucu yalnızca yapı değerini kendisi (genellikle geçici bir konuma yığın üzerinde) döndürür ve bu değer daha sonra gerektiğinde kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="4ba64-660">Sınıfları ile bu iki değişken aynı nesneye başvurmak mümkün ve dolayısıyla işlemler diğer değişkenin başvurduğu nesneyi etkileyebilir bir değişken üzerinde mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="4ba64-661">Yapılar, her değişkenleri kendi veri kopyası vardır ve yapılan işlemlerin diğerini etkilemesi olanaklı birinde değil.</span><span class="sxs-lookup"><span data-stu-id="4ba64-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="4ba64-662">Örneğin, aşağıdaki kod parçası tarafından üretilen çıkış bağlıdır `Point` bir sınıf veya yapı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="4ba64-663">Varsa `Point` bir sınıf çıkış `20` çünkü `a` ve `b` aynı nesneye başvuru.</span><span class="sxs-lookup"><span data-stu-id="4ba64-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="4ba64-664">Varsa `Point` bir yapı çıktı `10` çünkü atamasını `a` için `b` değeri bir kopyasını oluşturur ve bu kopyayı sonraki atamaya tarafından etkilenmez `a.x`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="4ba64-665">Önceki örnekte iki yapılar sınırlamaları vurgular.</span><span class="sxs-lookup"><span data-stu-id="4ba64-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="4ba64-666">İlk olarak, bir yapının tamamını kopyalama atama ve değerin parametre geçirme başvuru türleri ile yapılar ile daha pahalı olabilir. Bu nedenle, bir nesne başvurusu kopyalama daha genellikle daha az verimlidir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="4ba64-667">İkinci dışında `ref` ve `out` parametreleri, bu durumlarda, çeşitli kullanımları kullanıma kuralları yapı birimleri için başvuru oluşturmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="4ba64-668">Diziler</span><span class="sxs-lookup"><span data-stu-id="4ba64-668">Arrays</span></span>

<span data-ttu-id="4ba64-669">Bir ***dizi*** hesaplanan dizinlerini erişilen değişken bir sayı içeren bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="4ba64-670">Bir dizi içindeki değişkenler olarak da bilinir ***öğeleri*** dizinin tümü aynı türde ve bu tür çağrılır ***öğe türü*** dizi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="4ba64-671">Dizi türleri, başvuru türleridir ve bir dizi değişkeni bildirimi yalnızca kenara alanı başvuru bir dizi örneği için ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="4ba64-672">Gerçek bir dizi örnekleri dinamik olarak çalışma zamanı kullanılarak oluşturulur `new` işleci.</span><span class="sxs-lookup"><span data-stu-id="4ba64-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="4ba64-673">`new` İşlemi belirtir ***uzunluğu*** sonra Örneğin ömrü boyunca sabittir yeni dizi örneği.</span><span class="sxs-lookup"><span data-stu-id="4ba64-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="4ba64-674">Dizinleri öğelerinden oluşan bir dizi aralığından `0` için `Length - 1`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="4ba64-675">`new` İşleci, örneğin, sıfır olan tüm sayısal türler için varsayılan değerlerine bir dizinin öğeleri otomatik olarak başlatır ve `null` tüm başvuru türleri için.</span><span class="sxs-lookup"><span data-stu-id="4ba64-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="4ba64-676">Aşağıdaki örnek, bir dizi oluşturur. `int` öğeleri, dizi başlatır ve dizinin içeriği yazdırır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
<span data-ttu-id="4ba64-677">Bu örneği oluşturur ve üzerinde çalıştığı bir ***tek boyutlu dizi***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="4ba64-678">C# ' yı da destekler ***çok boyutlu diziler***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="4ba64-679">Olarak da bilinen bir dizinin boyut sayısını yazın ***derece*** dizi türü köşeli ayraç yazılan virgül sayısı artı bir dizi türünde olduğu.</span><span class="sxs-lookup"><span data-stu-id="4ba64-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="4ba64-680">Aşağıdaki örnek, tek boyutlu bir, iki boyutlu bir ve üç boyutlu bir dizi ayırır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="4ba64-681">`a1` Dizi 10 öğe içeriyor `a2` dizi içeriyor 50 (10 × 5) öğeleri ve `a3` dizi içeriyor (10 × 5 × 2) 100 öğeleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="4ba64-682">Bir dizinin öğe türü bir dizi türü de dahil olmak üzere herhangi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="4ba64-683">Bir dizi türünde öğelere sahip bir dizi adlandırılan bir ***düzensiz dizi*** öğe dizisi uzunluklarının tümünü değil aynı olmak zorunda olduğu.</span><span class="sxs-lookup"><span data-stu-id="4ba64-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="4ba64-684">Aşağıdaki örnekte dizi ayırır `int`:</span><span class="sxs-lookup"><span data-stu-id="4ba64-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="4ba64-685">İlk satır, üç öğeyle her türü bir dizi oluşturur `int[]` ve her bir başlangıç değeri ile `null`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="4ba64-686">Sonraki satırların başvurular değişen uzunlukları örneklerini tek bir dizi üç öğeleri ardından başlatın.</span><span class="sxs-lookup"><span data-stu-id="4ba64-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="4ba64-687">`new` İşleci verir başlangıç değerlerini kullanarak belirtilen dizi öğelerinin bir ***dizi Başlatıcısı***, sınırlayıcılar arasında yazılan ifadeler listesi olduğu `{` ve `}`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="4ba64-688">Aşağıdaki örnek ayırır ve başlatan bir `int[]` üç öğelerle.</span><span class="sxs-lookup"><span data-stu-id="4ba64-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="4ba64-689">Dizinin uzunluğu arasında ifadelerin sayısından algılanır Not `{` ve `}`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="4ba64-690">Yerel değişken ve alan bildirimleri kısalttık daha fazla sağlayacak şekilde dizi türü açıklandı gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4ba64-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="4ba64-691">Önceki örneklerin her ikisi de aşağıdakine eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="4ba64-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="4ba64-692">Arabirimler</span><span class="sxs-lookup"><span data-stu-id="4ba64-692">Interfaces</span></span>

<span data-ttu-id="4ba64-693">Bir ***arabirimi*** sınıfları ve yapıları tarafından uygulanan bir sözleşmeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="4ba64-694">Bir arabirim yöntemleri, özellikleri, olayları ve dizin oluşturucular içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="4ba64-695">Bir arabirim tanımlar üyelerinin uygulamalarını sağlamaz; yalnızca bir sınıf ya da arabirimi uygulayan yapının tarafından sağlanması gereken üyeleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="4ba64-696">, Arabirimleri görevlendirmek ***birden çok devralma***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="4ba64-697">Aşağıdaki örnekte, arabirim `IComboBox` hem de devralan `ITextBox` ve `IListBox`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
<span data-ttu-id="4ba64-698">Sınıflar ve yapı birimleri birden fazla arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="4ba64-699">Aşağıdaki örnekte, sınıf `EditBox` hem `IControl` ve `IDataBound`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
<span data-ttu-id="4ba64-700">Bir sınıf veya yapı, belirli bir arabirim uygular, bu sınıfın veya yapının örneği, bir arabirim türüne örtük olarak dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="4ba64-701">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4ba64-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="4ba64-702">Dinamik tür atamaları, burada bir örnek statik olarak belirli bir arabirim uygulamak için bilinmiyor durumlarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="4ba64-703">Örneğin, bir nesnenin elde etmek için dinamik tür atamaları aşağıdaki deyimleri kullanın `IControl` ve `IDataBound` arabirimi uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="4ba64-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="4ba64-704">Nesnenin gerçek türü olduğundan `EditBox`, yayınları başarılı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="4ba64-705">Önceki `EditBox` sınıfı `Paint` yönteminden `IControl` arabirimi ve `Bind` yönteminden `IDataBound` arabirimi kullanılarak uygulanır `public` üyeleri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="4ba64-706">C# ' yı da destekler ***açık arabirim üyesi uygulamalarını***, sınıfın kullandığı veya yapı kaçınmak üyeleri yapmadan `public`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="4ba64-707">Açık arabirim üyesi uygulaması, tam bir arabirim üye adını kullanarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="4ba64-708">Örneğin, `EditBox` sınıf uygulama `IControl.Paint` ve `IDataBound.Bind` yöntemlerini kullanarak açık arabirim üyesi uygulamaları gibi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="4ba64-709">Açık arabirim üyeleri yalnızca arabirim türü erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="4ba64-710">Örneğin, uygulanması `IControl.Paint` önceki tarafından sağlanan `EditBox` sınıf yalnızca çağrılabilir ilk dönüştürerek `EditBox` başvurusu `IControl` arabirim türü.</span><span class="sxs-lookup"><span data-stu-id="4ba64-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="4ba64-711">Numaralandırmalar</span><span class="sxs-lookup"><span data-stu-id="4ba64-711">Enums</span></span>

<span data-ttu-id="4ba64-712">Bir ***sabit listesi türü*** adlandırılmış sabitler kümesini içeren farklı bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="4ba64-713">Aşağıdaki örnek bildirir ve adlı bir sabit listesi türünü kullanıyor `Color` üç sabit değerler ile `Red`, `Green`, ve `Blue`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
<span data-ttu-id="4ba64-714">Her bir numaralandırma türünün adlı karşılık gelen bir integral türü olan ***temel alınan türü*** sabit listesi türü.</span><span class="sxs-lookup"><span data-stu-id="4ba64-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="4ba64-715">Bir temel türü açıkça bildirmiyor bir sabit listesi türü temel alınan türündedir `int`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="4ba64-716">Bir numaralandırma türünün depolama biçimi ve olası değerler aralığı kendi temel türü tarafından belirlenir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="4ba64-717">Enum türü alan değerleri kümesi numaralandırma üyelerini sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="4ba64-718">Özellikle, bir sabit listesi türünü temel herhangi bir değerini numaralandırma türüne dönüştürülebilen ve sabit listesi türü farklı geçerli bir değer.</span><span class="sxs-lookup"><span data-stu-id="4ba64-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="4ba64-719">Aşağıdaki örnek adlı bir sabit listesi türü bildirir `Alignment` bir temel alınan türüyle `sbyte`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="4ba64-720">Önceki örnekte gösterildiği gibi bir numaralandırma üyesi bildirimi üyenin değerini belirten sabit bir ifade içerebilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="4ba64-721">Her sabit listesi üyesi için sabit değerin sabit listesinin temel alınan türü aralığında olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="4ba64-722">Bir numaralandırma üyesi bildirimi bir değeri açıkça belirtmediği üyesi (sabit listesi türü ilk üye ise) sıfır değerini veya metin içeriğini eklemek önceki sabit listesi üyesi artı bir değer verilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="4ba64-723">Sabit listesi değerlerinin dönüştürülür tamsayı değerleri ve tersi tür atamaları kullanarak olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="4ba64-724">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="4ba64-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="4ba64-725">Tam sayı değeri sıfır numaralandırma türüne dönüştürülerek herhangi bir enum tür varsayılan değerdir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="4ba64-726">Burada değişkenleri için varsayılan değer otomatik olarak başlatılır durumlarda değişkenleri sabit listesi türleri için verilen değer budur.</span><span class="sxs-lookup"><span data-stu-id="4ba64-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="4ba64-727">Varsayılan değer olan kolayca kullanılabilir olması için bir liste türü değişmez değer için sırayla `0` örtük olarak herhangi bir numaralandırma türüne dönüştürür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="4ba64-728">Bu nedenle, aşağıdaki izin verilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="4ba64-729">Temsilciler</span><span class="sxs-lookup"><span data-stu-id="4ba64-729">Delegates</span></span>

<span data-ttu-id="4ba64-730">A ***temsilci türü*** belirli bir parametre olan yöntemlere başvuruları temsil listesi ve dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="4ba64-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="4ba64-731">Temsilciler, yöntemleri değişkenine atanır ve parametre olarak geçirilen varlıklar olarak değerlendirmek mümkün kılar.</span><span class="sxs-lookup"><span data-stu-id="4ba64-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="4ba64-732">Diğer dillerde bulunan işlev işaretçileri kavramı temsilcileri benzerdir ancak işlev işaretçileri, nesne yönelimli ve tür kullanımı uyumlu temsilciler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="4ba64-733">Aşağıdaki örnek bildirir ve adlandırılmış bir temsilci türü kullanır `Function`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-733">The following example declares and uses a delegate type named `Function`.</span></span>

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
<span data-ttu-id="4ba64-734">Örneği `Function` temsilci türü, alan herhangi bir yöntemi başvurabilir bir `double` bağımsız değişkeni ve döndürür bir `double` değeri.</span><span class="sxs-lookup"><span data-stu-id="4ba64-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="4ba64-735">`Apply` Yöntemi uygular bir verilen `Function` öğelerin bir `double[]`, döndüren bir `double[]` sonuçları.</span><span class="sxs-lookup"><span data-stu-id="4ba64-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="4ba64-736">İçinde `Main` yöntemi `Apply` üç farklı işlevlere uygulamak için kullanılan bir `double[]`.</span><span class="sxs-lookup"><span data-stu-id="4ba64-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="4ba64-737">Bir temsilci bir statik yöntem başvurabilir (gibi `Square` veya `Math.Sin` önceki örnekte) veya bir örnek yöntemi (gibi `m.Multiply` önceki örnekte).</span><span class="sxs-lookup"><span data-stu-id="4ba64-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="4ba64-738">Bir örnek yöntemi başvuran bir temsilci Ayrıca belirli bir nesnenin başvuruda bulunduğundan ve temsilci örnek yöntemi çağrıldığında, bu nesne haline gelir `this` çağrısı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="4ba64-739">Temsilciler da "anında oluşturulan satır içi yöntemler" anonim işlevler kullanılarak oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="4ba64-740">Anonim İşlevler, yerel değişkenler çevreleyen yöntemlerden görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4ba64-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="4ba64-741">Bu nedenle, yukarıdaki çarpan örneği daha kolay kullanmadan yazılabilir bir `Multiplier` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="4ba64-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="4ba64-742">Bir ilgi çekici ve kullanışlı bir temsilci, bilmiyorsanız veya yöntemin başvurduğu sınıfı hakkında dikkatli olun, özelliğidir; önemli olan başvurulan yöntemi aynı parametre ve dönüş türü temsilciyle sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="4ba64-743">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="4ba64-743">Attributes</span></span>

<span data-ttu-id="4ba64-744">Bazı yönlerini davranışlarını denetleyen değiştiriciler, türleri, üyeleri ve diğer varlıkların bir C# programında destekler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="4ba64-745">Örneğin, bir yöntemin erişilebilirliği kullanılarak denetlenir `public`, `protected`, `internal`, ve `private` değiştiriciler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="4ba64-746">Bildirim temelli bilgileri kullanıcı tanımlı türde program varlıklara bağlı ve çalışma zamanında alınan C# bu özellik genelleştirir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="4ba64-747">Programlar belirtmek bu ek tanımlayıcı bilgiler tanımlama ve kullanma ***öznitelikleri***.</span><span class="sxs-lookup"><span data-stu-id="4ba64-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="4ba64-748">Aşağıdaki örnek bildirir bir `HelpAttribute` , ilişkili belgelere bağlantılar sağlamak için program varlıkları yerleştirildiği özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4ba64-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
<span data-ttu-id="4ba64-749">Tüm öznitelik sınıfları türetilmesi `System.Attribute` temel .NET Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4ba64-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="4ba64-750">Öznitelik, köşeli ayraç içinde ilişkili bildirimi hemen önce tüm bağımsız değişkenleri yanı sıra bunların adı vererek uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="4ba64-751">Bir özniteliğin adı biterse `Attribute`, öznitelik başvurulduğunda bu bölümü adı atlanmış olabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="4ba64-752">Örneğin, `HelpAttribute` özniteliği aşağıdaki gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4ba64-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="4ba64-753">Bu örnekte bağlayan bir `HelpAttribute` için `Widget` sınıf ve başka `HelpAttribute` için `Display` sınıfında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4ba64-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="4ba64-754">Genel oluşturucular bir öznitelik sınıfı özniteliği için bir program varlığı eklendiğinde sağlanmalıdır bilgileri denetler.</span><span class="sxs-lookup"><span data-stu-id="4ba64-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="4ba64-755">Ek bilgi öznitelik sınıfının ortak okuma-yazma özelliklerine başvurarak sağlanabilir (başvuru gibi `Topic` özelliği daha önce).</span><span class="sxs-lookup"><span data-stu-id="4ba64-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="4ba64-756">Aşağıdaki örnek, belirli bir programı varlık için öznitelik bilgileri, çalışma zamanında nasıl alınabilir gösterir yansıma kullanarak.</span><span class="sxs-lookup"><span data-stu-id="4ba64-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
<span data-ttu-id="4ba64-757">Belirli bir özniteliği yansıma yoluyla istendiğinde program kaynakta sağlanan bilgileri ile öznitelik sınıfı için oluşturucu çağrılır ve sonuçta elde edilen öznitelik örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="4ba64-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="4ba64-758">Ek bilgi özellikleri aracılığıyla sağlandıysa, öznitelik örneği döndürülmeden önce bu özellikleri için belirtilen değerlere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="4ba64-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
