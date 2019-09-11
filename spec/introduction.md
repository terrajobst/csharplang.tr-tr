---
ms.openlocfilehash: 8bc4bf6310fb8a8457beee167f18d30aaca10a8e
ms.sourcegitcommit: 7f7fc6e9e195e51b7ff8229aeaa70aa9fbbb63cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70876905"
---
# <a name="introduction"></a><span data-ttu-id="bc420-101">Giriş</span><span class="sxs-lookup"><span data-stu-id="bc420-101">Introduction</span></span>

<span data-ttu-id="bc420-102">C#("bkz. diyez") basit, modern, nesne odaklı ve tür açısından güvenli bir programlama dilidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-102">C# (pronounced "See Sharp") is a simple, modern, object-oriented, and type-safe programming language.</span></span> <span data-ttu-id="bc420-103">C#C 'nin dil ailesinde köklerine sahiptir ve hemen C, C++ve Java programcılarına tanıdık gelecektir.</span><span class="sxs-lookup"><span data-stu-id="bc420-103">C# has its roots in the C family of languages and will be immediately familiar to C, C++, and Java programmers.</span></span> <span data-ttu-id="bc420-104">C#ECMA ***-334*** standardı ve ISO/IEC tarafından ***ıso/IEC 23270*** standardı olarak standartlaştırılmış Ecma International tarafından standartlaştırılmış olur.</span><span class="sxs-lookup"><span data-stu-id="bc420-104">C# is standardized by ECMA International as the ***ECMA-334*** standard and by ISO/IEC as the ***ISO/IEC 23270*** standard.</span></span> <span data-ttu-id="bc420-105">Microsoft 'un C# .NET Framework derleyicisi, bu standartların her ikisinin de uyumlu bir uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-105">Microsoft's C# compiler for the .NET Framework is a conforming implementation of both of these standards.</span></span>

<span data-ttu-id="bc420-106">C#, nesne odaklı bir dildir, ancak C# daha fazla ***bileşen odaklı*** programlama desteğini içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-106">C# is an object-oriented language, but C# further includes support for ***component-oriented*** programming.</span></span> <span data-ttu-id="bc420-107">Modern yazılım tasarımı giderek, yazılım bileşenlerini, kendi kendine içerilen ve kendi kendine açıklayan işlev paketleri biçiminde kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-107">Contemporary software design increasingly relies on software components in the form of self-contained and self-describing packages of functionality.</span></span> <span data-ttu-id="bc420-108">Bu bileşenlere anahtar, özellikler, Yöntemler ve olaylar içeren bir programlama modeli sunduklarında; Bunlar, bileşen hakkında bildirime dayalı bilgiler sağlayan özniteliklere sahiptir; ve kendi belgelerini içerirler.</span><span class="sxs-lookup"><span data-stu-id="bc420-108">Key to such components is that they present a programming model with properties, methods, and events; they have attributes that provide declarative information about the component; and they incorporate their own documentation.</span></span> <span data-ttu-id="bc420-109">C#, bu kavramları doğrudan desteklemek için yazılım bileşenlerinin oluşturulması ve C# kullanılması için çok doğal bir dil sunarak dil yapıları sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-109">C# provides language constructs to directly support these concepts, making C# a very natural language in which to create and use software components.</span></span>

<span data-ttu-id="bc420-110">Birçok C# Özellik sağlam ve dayanıklı uygulamaların yapımasına yardımcı olur: ***Çöp toplama*** , kullanılmayan nesneler tarafından kullanılan belleği otomatik olarak geri kazanır; ***özel durum işleme*** , hata algılama ve kurtarmaya yönelik yapılandırılmış ve genişletilebilir bir yaklaşım sağlar; dilin ***tür kullanımı uyumlu*** tasarımı, başlatılmamış değişkenlerden okumayı, sınırları ötesinde dizileri dizin haline getirmek veya Denetlenmemiş tür yayınları gerçekleştirmeyi olanaksız hale getirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-110">Several C# features aid in the construction of robust and durable applications: ***Garbage collection*** automatically reclaims memory occupied by unused objects; ***exception handling*** provides a structured and extensible approach to error detection and recovery; and the ***type-safe*** design of the language makes it impossible to read from uninitialized variables, to index arrays beyond their bounds, or to perform unchecked type casts.</span></span>

<span data-ttu-id="bc420-111">C#Birleşik bir ***tür sistemine***sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bc420-111">C# has a ***unified type system***.</span></span> <span data-ttu-id="bc420-112">`int` Ve `object` C# gibitemeltürlerdahilolmaküzeretümtürler,tekbirköktüründendevralınır.`double`</span><span class="sxs-lookup"><span data-stu-id="bc420-112">All C# types, including primitive types such as `int` and `double`, inherit from a single root `object` type.</span></span> <span data-ttu-id="bc420-113">Bu nedenle, tüm türler ortak işlemler kümesini paylaşır ve herhangi bir türdeki değerler tutarlı bir şekilde depolanabilir, taşınır ve çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-113">Thus, all types share a set of common operations, and values of any type can be stored, transported, and operated upon in a consistent manner.</span></span> <span data-ttu-id="bc420-114">Ayrıca, C# hem Kullanıcı tanımlı başvuru türlerini hem de değer türlerini destekler, bu da nesnelerin dinamik ayrılmasına ve basit yapıların satır içi depolamamasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-114">Furthermore, C# supports both user-defined reference types and value types, allowing dynamic allocation of objects as well as in-line storage of lightweight structures.</span></span>

<span data-ttu-id="bc420-115">Programlar ve kitaplıkların zaman içinde zaman içinde gelişebilmesini sağlamak için, tasarımına daha fazla vurgu konulmuştur. C# C#</span><span class="sxs-lookup"><span data-stu-id="bc420-115">To ensure that C# programs and libraries can evolve over time in a compatible manner, much emphasis has been placed on ***versioning*** in C#'s design.</span></span> <span data-ttu-id="bc420-116">Birçok programlama dili bu sorunla ilgileniyor ve sonuç olarak, bu dillerde yazılmış programlar, bağımlı kitaplıkların daha yeni sürümleri tanıtıldığında gerekenden çok daha fazla.</span><span class="sxs-lookup"><span data-stu-id="bc420-116">Many programming languages pay little attention to this issue, and, as a result, programs written in those languages break more often than necessary when newer versions of dependent libraries are introduced.</span></span> <span data-ttu-id="bc420-117">C#Sürüm oluşturma konuları tarafından doğrudan etkilenen tasarımın yönleri, ayrı `virtual` ve `override` değiştiriciler, yöntem aşırı yükleme çözümlemesi kuralları ve açık arabirim üye bildirimleri için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-117">Aspects of C#'s design that were directly influenced by versioning considerations include the separate `virtual` and `override` modifiers, the rules for method overload resolution, and support for explicit interface member declarations.</span></span>

<span data-ttu-id="bc420-118">Bu bölümün geri kalanında C# dilin önemli özellikleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bc420-118">The rest of this chapter describes the essential features of the C# language.</span></span> <span data-ttu-id="bc420-119">Sonraki bölümlerde, kuralları ve özel durumları ayrıntı yönelimli ve bazen matematik olarak anlasa da, bu bölüm, tamamlanma masrafında açıklık ve kısaltma açısından çok çaba harcar.</span><span class="sxs-lookup"><span data-stu-id="bc420-119">Although later chapters describe rules and exceptions in a detail-oriented and sometimes mathematical manner, this chapter strives for clarity and brevity at the expense of completeness.</span></span> <span data-ttu-id="bc420-120">Amaç, okuyucuya erken programların yazılmasına ve sonraki bölümlerin okunmasına yardımcı olacak dile bir giriş sunmaktır.</span><span class="sxs-lookup"><span data-stu-id="bc420-120">The intent is to provide the reader with an introduction to the language that will facilitate the writing of early programs and the reading of later chapters.</span></span>

## <a name="hello-world"></a><span data-ttu-id="bc420-121">Merhaba dünya</span><span class="sxs-lookup"><span data-stu-id="bc420-121">Hello world</span></span>

<span data-ttu-id="bc420-122">"Hello, World" programı, genellikle bir programlama dilini tanıtmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-122">The "Hello, World" program is traditionally used to introduce a programming language.</span></span> <span data-ttu-id="bc420-123">Burada yer C#verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="bc420-123">Here it is in C#:</span></span>

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

<span data-ttu-id="bc420-124">C#Kaynak dosyalar genellikle dosya uzantısına `.cs`sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bc420-124">C# source files typically have the file extension `.cs`.</span></span> <span data-ttu-id="bc420-125">"Hello, World" programının dosyada `hello.cs`depolandığını varsayarak, program komut satırı kullanılarak Microsoft C# derleyicisi ile derlenebilir</span><span class="sxs-lookup"><span data-stu-id="bc420-125">Assuming that the "Hello, World" program is stored in the file `hello.cs`, the program can be compiled with the Microsoft C# compiler using the command line</span></span>
```
csc hello.cs
```
<span data-ttu-id="bc420-126">adında `hello.exe`bir çalıştırılabilir derleme üreten.</span><span class="sxs-lookup"><span data-stu-id="bc420-126">which produces an executable assembly named `hello.exe`.</span></span> <span data-ttu-id="bc420-127">Bu uygulama tarafından çalıştırıldığında üretilen çıkış,</span><span class="sxs-lookup"><span data-stu-id="bc420-127">The output produced by this application when it is run is</span></span>
```
Hello, World
```

<span data-ttu-id="bc420-128">"Hello, World" programı, `using` `System` ad alanına başvuran bir yönergeyle başlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-128">The "Hello, World" program starts with a `using` directive that references the `System` namespace.</span></span> <span data-ttu-id="bc420-129">Ad alanları, programları ve kitaplıkları düzenlemek C# için hiyerarşik bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-129">Namespaces provide a hierarchical means of organizing C# programs and libraries.</span></span> <span data-ttu-id="bc420-130">Ad alanları türler ve diğer ad alanlarını içerir — örneğin, `System` ad alanı programda başvurulan `Console` sınıf gibi bir dizi tür içerir ve `IO` ve `Collections`gibi diğer ad alanları vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-130">Namespaces contain types and other namespaces—for example, the `System` namespace contains a number of types, such as the `Console` class referenced in the program, and a number of other namespaces, such as `IO` and `Collections`.</span></span> <span data-ttu-id="bc420-131">Verilen `using` bir ad alanına başvuruda bulunan bir yönerge, bu ad alanının üyesi olan türlerin nitelenmemiş kullanımını mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="bc420-131">A `using` directive that references a given namespace enables unqualified use of the types that are members of that namespace.</span></span> <span data-ttu-id="bc420-132">Yönergesi nedeniyle program, için `System.Console.WriteLine`toplu olarak kullanabilir `Console.WriteLine`. `using`</span><span class="sxs-lookup"><span data-stu-id="bc420-132">Because of the `using` directive, the program can use `Console.WriteLine` as shorthand for `System.Console.WriteLine`.</span></span>

<span data-ttu-id="bc420-133">"Hello, World" programı tarafından belirtilen `Main` sınıftekbirüyeyesahiptir,yöntemiadlı.`Hello`</span><span class="sxs-lookup"><span data-stu-id="bc420-133">The `Hello` class declared by the "Hello, World" program has a single member, the method named `Main`.</span></span> <span data-ttu-id="bc420-134">`Main` Yöntemi`static` değiştiriciyle birlikte bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-134">The `Main` method is declared with the `static` modifier.</span></span> <span data-ttu-id="bc420-135">Örnek yöntemleri, anahtar sözcüğünü `this`kullanarak belirli bir kapsayan nesne örneğine başvurabilir, ancak statik yöntemler belirli bir nesneye başvuru olmadan çalışır.</span><span class="sxs-lookup"><span data-stu-id="bc420-135">While instance methods can reference a particular enclosing object instance using the keyword `this`, static methods operate without reference to a particular object.</span></span> <span data-ttu-id="bc420-136">Kurala göre, adlı `Main` statik bir yöntem, bir programın giriş noktası olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="bc420-136">By convention, a static method named `Main` serves as the entry point of a program.</span></span>

<span data-ttu-id="bc420-137">Programın çıktısı, `WriteLine` `System` ad alanındaki `Console` sınıfının yöntemi tarafından üretilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-137">The output of the program is produced by the `WriteLine` method of the `Console` class in the `System` namespace.</span></span> <span data-ttu-id="bc420-138">Bu sınıf, varsayılan olarak Microsoft C# derleyicisi tarafından otomatik olarak başvurulduğu .NET Framework sınıf kitaplıkları tarafından sağlanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-138">This class is provided by the .NET Framework class libraries, which, by default, are automatically referenced by the Microsoft C# compiler.</span></span> <span data-ttu-id="bc420-139">C# Kendisinin ayrı bir çalışma zamanı kitaplığı olmadığına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="bc420-139">Note that C# itself does not have a separate runtime library.</span></span> <span data-ttu-id="bc420-140">Bunun yerine, .NET Framework çalışma zamanı kitaplığıdır C#.</span><span class="sxs-lookup"><span data-stu-id="bc420-140">Instead, the .NET Framework is the runtime library of C#.</span></span>

## <a name="program-structure"></a><span data-ttu-id="bc420-141">Program yapısı</span><span class="sxs-lookup"><span data-stu-id="bc420-141">Program structure</span></span>

<span data-ttu-id="bc420-142">' Deki C# temel kurumsal kavramlar, ***Programlar***, ***ad alanları***, ***türler***, ***Üyeler***ve ***derlemelerdir***.</span><span class="sxs-lookup"><span data-stu-id="bc420-142">The key organizational concepts in C# are ***programs***, ***namespaces***, ***types***, ***members***, and ***assemblies***.</span></span> <span data-ttu-id="bc420-143">C#programlar bir veya daha fazla kaynak dosyadan oluşur.</span><span class="sxs-lookup"><span data-stu-id="bc420-143">C# programs consist of one or more source files.</span></span> <span data-ttu-id="bc420-144">Programlar, üyeleri içeren ve ad alanları halinde düzenlenebilen türleri bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-144">Programs declare types, which contain members and can be organized into namespaces.</span></span> <span data-ttu-id="bc420-145">Sınıflar ve arabirimler tür örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="bc420-145">Classes and interfaces are examples of types.</span></span> <span data-ttu-id="bc420-146">Alanlar, Yöntemler, Özellikler ve olaylar üye örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="bc420-146">Fields, methods, properties, and events are examples of members.</span></span> <span data-ttu-id="bc420-147">C# Programlar derlendiğinde, fiziksel olarak derlemeler halinde paketlenir.</span><span class="sxs-lookup"><span data-stu-id="bc420-147">When C# programs are compiled, they are physically packaged into assemblies.</span></span> <span data-ttu-id="bc420-148">Derlemeler `.dll`, ***uygulama*** veya ***kitaplık***uygulanıp `.exe` uygulamadığına bağlı olarak genellikle dosya uzantısına sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bc420-148">Assemblies typically have the file extension `.exe` or `.dll`, depending on whether they implement ***applications*** or ***libraries***.</span></span>

<span data-ttu-id="bc420-149">Örnek</span><span class="sxs-lookup"><span data-stu-id="bc420-149">The example</span></span>

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
<span data-ttu-id="bc420-150">adlı bir `Stack` `Acme.Collections`ad alanında adında bir sınıf bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-150">declares a class named `Stack` in a namespace called `Acme.Collections`.</span></span> <span data-ttu-id="bc420-151">Bu sınıfın `Acme.Collections.Stack`tam adı.</span><span class="sxs-lookup"><span data-stu-id="bc420-151">The fully qualified name of this class is `Acme.Collections.Stack`.</span></span> <span data-ttu-id="bc420-152">Sınıf birçok `top`üye içerir: adlı bir alan, `Push` ve `Pop`adlı iki yöntem ve adlı `Entry`bir iç içe sınıf.</span><span class="sxs-lookup"><span data-stu-id="bc420-152">The class contains several members: a field named `top`, two methods named `Push` and `Pop`, and a nested class named `Entry`.</span></span> <span data-ttu-id="bc420-153">Sınıf daha fazla üç üye içerir: adlı `next`alan, adlı `data`alan ve Oluşturucu. `Entry`</span><span class="sxs-lookup"><span data-stu-id="bc420-153">The `Entry` class further contains three members: a field named `next`, a field named `data`, and a constructor.</span></span> <span data-ttu-id="bc420-154">Örneğin kaynak kodunun dosyada `acme.cs`depolandığını varsayarak, komut satırı</span><span class="sxs-lookup"><span data-stu-id="bc420-154">Assuming that the source code of the example is stored in the file `acme.cs`, the command line</span></span>

```
csc /t:library acme.cs
```
<span data-ttu-id="bc420-155">örneği bir kitaplık (bir `Main` giriş noktası olmayan kod) olarak derler ve adında `acme.dll`bir derleme oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bc420-155">compiles the example as a library (code without a `Main` entry point) and produces an assembly named `acme.dll`.</span></span>

<span data-ttu-id="bc420-156">Derlemeler, ***ara dil*** (IL) yönergeleri biçiminde çalıştırılabilir kodu ve ***meta veri***biçimindeki sembolik bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-156">Assemblies contain executable code in the form of ***Intermediate Language*** (IL) instructions, and symbolic information in the form of ***metadata***.</span></span> <span data-ttu-id="bc420-157">Yürütülmeden önce, bir derlemedeki IL kodu, .NET ortak dil çalışma zamanının tam zamanında (JıT) derleyicisi tarafından otomatik olarak işlemciye özgü koda dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="bc420-157">Before it is executed, the IL code in an assembly is automatically converted to processor-specific code by the Just-In-Time (JIT) compiler of .NET Common Language Runtime.</span></span>

<span data-ttu-id="bc420-158">Bir derleme, hem kodu hem de meta verileri içeren bir işlevsellik birimi olduğundan, içinde `#include` C#yönergeler ve başlık dosyaları gerekmez.</span><span class="sxs-lookup"><span data-stu-id="bc420-158">Because an assembly is a self-describing unit of functionality containing both code and metadata, there is no need for `#include` directives and header files in C#.</span></span> <span data-ttu-id="bc420-159">Belirli bir derlemede yer alan ortak türler ve Üyeler, yalnızca program derlenirken bu derlemeye C# başvurarak bir programda kullanılabilir hale getirilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-159">The public types and members contained in a particular assembly are made available in a C# program simply by referencing that assembly when compiling the program.</span></span> <span data-ttu-id="bc420-160">Örneğin, bu program `Acme.Collections.Stack` `acme.dll` derlemeden sınıfını kullanır:</span><span class="sxs-lookup"><span data-stu-id="bc420-160">For example, this program uses the `Acme.Collections.Stack` class from the `acme.dll` assembly:</span></span>

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
<span data-ttu-id="bc420-161">Program dosyada `test.cs`depolanıyorsa `test.cs` , derlendikten sonra `acme.dll` `/r` derlemeye derleyicinin seçeneği kullanılarak başvurulabilir:</span><span class="sxs-lookup"><span data-stu-id="bc420-161">If the program is stored in the file `test.cs`, when `test.cs` is compiled, the `acme.dll` assembly can be referenced using the compiler's `/r` option:</span></span>

```
csc /r:acme.dll test.cs
```
<span data-ttu-id="bc420-162">Bu, çalıştırıldığında bir çalıştırılabilir derleme `test.exe`oluşturur; Bu, çalıştırıldığında çıktıyı üretir:</span><span class="sxs-lookup"><span data-stu-id="bc420-162">This creates an executable assembly named `test.exe`, which, when run, produces the output:</span></span>

```
100
10
1
```
<span data-ttu-id="bc420-163">C#bir programın kaynak metninin birkaç kaynak dosyada depolanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="bc420-163">C# permits the source text of a program to be stored in several source files.</span></span> <span data-ttu-id="bc420-164">Birden çok dosya C# programı derlendiğinde, tüm kaynak dosyalar birlikte işlenir ve kaynak dosyalar birbirini tamamen başvurabilir — kavramsal olarak, tüm kaynak dosyaları işlenmeden önce bir büyük dosyada birleştirilmiş olur.</span><span class="sxs-lookup"><span data-stu-id="bc420-164">When a multi-file C# program is compiled, all of the source files are processed together, and the source files can freely reference each other—conceptually, it is as if all the source files were concatenated into one large file before being processed.</span></span> <span data-ttu-id="bc420-165">Bildirim sırası çok önemli olduğundan, C# bazı durumlarda ileri bildirimlere hiçbir şekilde gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="bc420-165">Forward declarations are never needed in C# because, with very few exceptions, declaration order is insignificant.</span></span> <span data-ttu-id="bc420-166">C#kaynak dosyayı yalnızca bir ortak tür bildirmek üzere sınırlamaz veya kaynak dosyanın adının kaynak dosyada belirtilen bir türle eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc420-166">C# does not limit a source file to declaring only one public type nor does it require the name of the source file to match a type declared in the source file.</span></span>

## <a name="types-and-variables"></a><span data-ttu-id="bc420-167">Türler ve değişkenler</span><span class="sxs-lookup"><span data-stu-id="bc420-167">Types and variables</span></span>

<span data-ttu-id="bc420-168">' De C#iki tür tür vardır: ***değer türleri*** ve ***başvuru türleri***.</span><span class="sxs-lookup"><span data-stu-id="bc420-168">There are two kinds of types in C#: ***value types*** and ***reference types***.</span></span> <span data-ttu-id="bc420-169">Değer türlerinin değişkenleri doğrudan verilerini içerir, ancak başvuru türündeki değişkenler verilerine başvuruları depolar, ikincisi ise nesneler olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="bc420-169">Variables of value types directly contain their data whereas variables of reference types store references to their data, the latter being known as objects.</span></span> <span data-ttu-id="bc420-170">Başvuru türleriyle, iki değişkenin aynı nesneye başvurması ve bu nedenle bir değişkende işlemler için diğer değişken tarafından başvurulan nesneyi etkilemesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="bc420-170">With reference types, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="bc420-171">Değer türleriyle, her biri kendi verilerinin kopyasına sahiptir ve bir üzerindeki işlemler, diğerini ( `ref` ve `out` parametre değişkenleri hariç) etkileyecek şekilde mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="bc420-171">With value types, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other (except in the case of `ref` and `out` parameter variables).</span></span>

<span data-ttu-id="bc420-172">C#değer türleri, ***basit türler***, ***sabit listesi türleri***, ***yapı türleri***ve ***null yapılabilir türlere***daha da bölünür ve C#başvuru türleri, ***Sınıf türlerine***, ***arabirim türlerine***, ***diziye daha fazla bölünmüştür türler***ve ***temsilci türleri***.</span><span class="sxs-lookup"><span data-stu-id="bc420-172">C#'s value types are further divided into ***simple types***, ***enum types***, ***struct types***, and ***nullable types***, and C#'s reference types are further divided into ***class types***, ***interface types***, ***array types***, and ***delegate types***.</span></span>

<span data-ttu-id="bc420-173">Aşağıdaki tabloda, tür sistemine genel bir C#bakış sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bc420-173">The following table provides an overview of C#'s type system.</span></span>

| <span data-ttu-id="bc420-174">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="bc420-174">__Category__</span></span>    |                 | <span data-ttu-id="bc420-175">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="bc420-175">__Description__</span></span> |
|-----------------|-----------------|-----------------|
| <span data-ttu-id="bc420-176">Değer türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-176">Value types</span></span>     | <span data-ttu-id="bc420-177">Basit türler</span><span class="sxs-lookup"><span data-stu-id="bc420-177">Simple types</span></span>    | <span data-ttu-id="bc420-178">İmzalanan integral: `sbyte`, `short`, `int`,`long`</span><span class="sxs-lookup"><span data-stu-id="bc420-178">Signed integral: `sbyte`, `short`, `int`, `long`</span></span> |
|                 |                 | <span data-ttu-id="bc420-179">İşaretsiz integral: `byte`, `ushort`, `uint`,`ulong`</span><span class="sxs-lookup"><span data-stu-id="bc420-179">Unsigned integral: `byte`, `ushort`, `uint`, `ulong`</span></span> |
|                 |                 | <span data-ttu-id="bc420-180">Unicode karakterler:`char`</span><span class="sxs-lookup"><span data-stu-id="bc420-180">Unicode characters: `char`</span></span> |
|                 |                 | <span data-ttu-id="bc420-181">IEEE kayan nokta: `float`,`double`</span><span class="sxs-lookup"><span data-stu-id="bc420-181">IEEE floating point: `float`, `double`</span></span> |
|                 |                 | <span data-ttu-id="bc420-182">Yüksek duyarlıklı ondalık:`decimal`</span><span class="sxs-lookup"><span data-stu-id="bc420-182">High-precision decimal: `decimal`</span></span> |
|                 |                 | <span data-ttu-id="bc420-183">Boolean`bool`</span><span class="sxs-lookup"><span data-stu-id="bc420-183">Boolean: `bool`</span></span> |
|                 | <span data-ttu-id="bc420-184">Sabit listesi türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-184">Enum types</span></span>      | <span data-ttu-id="bc420-185">Formun Kullanıcı tanımlı türleri`enum E {...}`</span><span class="sxs-lookup"><span data-stu-id="bc420-185">User-defined types of the form `enum E {...}`</span></span> |
|                 | <span data-ttu-id="bc420-186">Yapı türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-186">Struct types</span></span>    | <span data-ttu-id="bc420-187">Formun Kullanıcı tanımlı türleri`struct S {...}`</span><span class="sxs-lookup"><span data-stu-id="bc420-187">User-defined types of the form `struct S {...}`</span></span> |
|                 | <span data-ttu-id="bc420-188">Boş değer atanabilir tipler</span><span class="sxs-lookup"><span data-stu-id="bc420-188">Nullable types</span></span>  | <span data-ttu-id="bc420-189">Bir `null` değere sahip diğer tüm değer türlerinin uzantıları</span><span class="sxs-lookup"><span data-stu-id="bc420-189">Extensions of all other value types with a `null` value</span></span> |
| <span data-ttu-id="bc420-190">Başvuru türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-190">Reference types</span></span> | <span data-ttu-id="bc420-191">Sınıf türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-191">Class types</span></span>     | <span data-ttu-id="bc420-192">Diğer tüm türlerin Ultimate temel sınıfı:`object`</span><span class="sxs-lookup"><span data-stu-id="bc420-192">Ultimate base class of all other types: `object`</span></span> |
|                 |                 | <span data-ttu-id="bc420-193">Unicode dizeleri:`string`</span><span class="sxs-lookup"><span data-stu-id="bc420-193">Unicode strings: `string`</span></span> |
|                 |                 | <span data-ttu-id="bc420-194">Formun Kullanıcı tanımlı türleri`class C {...}`</span><span class="sxs-lookup"><span data-stu-id="bc420-194">User-defined types of the form `class C {...}`</span></span> |
|                 | <span data-ttu-id="bc420-195">Arabirim türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-195">Interface types</span></span> | <span data-ttu-id="bc420-196">Formun Kullanıcı tanımlı türleri`interface I {...}`</span><span class="sxs-lookup"><span data-stu-id="bc420-196">User-defined types of the form `interface I {...}`</span></span> |
|                 | <span data-ttu-id="bc420-197">Dizi türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-197">Array types</span></span>     | <span data-ttu-id="bc420-198">Tek ve çok boyutlu, örneğin `int[]` ve`int[,]`</span><span class="sxs-lookup"><span data-stu-id="bc420-198">Single- and multi-dimensional, for example, `int[]` and `int[,]`</span></span> |
|                 | <span data-ttu-id="bc420-199">Temsilci türleri</span><span class="sxs-lookup"><span data-stu-id="bc420-199">Delegate types</span></span>  | <span data-ttu-id="bc420-200">Formun Kullanıcı tanımlı türleri örn.`delegate int  D(...)`</span><span class="sxs-lookup"><span data-stu-id="bc420-200">User-defined types of the form e.g. `delegate int  D(...)`</span></span> |

<span data-ttu-id="bc420-201">Sekiz integral türü, imzalanmış veya imzasız biçimde 8 bit, 16 bit, 32-bit ve 64 bit değerleri için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-201">The eight integral types provide support for 8-bit, 16-bit, 32-bit, and 64-bit values in signed or unsigned form.</span></span>

<span data-ttu-id="bc420-202">İki kayan nokta türü `float` ve `double`, 32-bit tek duyarlıklı ve 64-bit çift duyarlıklı IEEE 754 biçimleri kullanılarak temsil edilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-202">The two floating point types, `float` and `double`, are represented using the 32-bit single-precision and 64-bit double-precision IEEE 754 formats.</span></span>

<span data-ttu-id="bc420-203">`decimal` Türü, finansal ve parasal hesaplamalar için uygun olan 128 bitlik bir veri türüdür.</span><span class="sxs-lookup"><span data-stu-id="bc420-203">The `decimal` type is a 128-bit data type suitable for financial and monetary calculations.</span></span>

<span data-ttu-id="bc420-204">C#türü, Boole değerlerini ( `true` veya `false`olan değerler) temsil etmek için kullanılır. `bool`</span><span class="sxs-lookup"><span data-stu-id="bc420-204">C#'s `bool` type is used to represent boolean values—values that are either `true` or `false`.</span></span>

<span data-ttu-id="bc420-205">İçindeki C# karakter ve dize işleme Unicode kodlaması kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-205">Character and string processing in C# uses Unicode encoding.</span></span> <span data-ttu-id="bc420-206">Tür bir UTF-16 kod birimini temsil eder `string` ve tür bir UTF-16 kod birimi dizisini temsil eder. `char`</span><span class="sxs-lookup"><span data-stu-id="bc420-206">The `char` type represents a UTF-16 code unit, and the `string` type represents a sequence of UTF-16 code units.</span></span>

<span data-ttu-id="bc420-207">Aşağıdaki tabloda sayısal türler C#özetlenmektedir.</span><span class="sxs-lookup"><span data-stu-id="bc420-207">The following table summarizes C#'s numeric types.</span></span>


| <span data-ttu-id="bc420-208">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="bc420-208">__Category__</span></span>      | <span data-ttu-id="bc420-209">__Bitlik__</span><span class="sxs-lookup"><span data-stu-id="bc420-209">__Bits__</span></span> | <span data-ttu-id="bc420-210">__Tür__</span><span class="sxs-lookup"><span data-stu-id="bc420-210">__Type__</span></span>  | <span data-ttu-id="bc420-211">__Aralık/duyarlık__</span><span class="sxs-lookup"><span data-stu-id="bc420-211">__Range/Precision__</span></span> |
|-------------------|----------|-----------|---------------------|
| <span data-ttu-id="bc420-212">İmzalanan integral</span><span class="sxs-lookup"><span data-stu-id="bc420-212">Signed integral</span></span>   | <span data-ttu-id="bc420-213">8</span><span class="sxs-lookup"><span data-stu-id="bc420-213">8</span></span>        | `sbyte`   | <span data-ttu-id="bc420-214">-128... 127</span><span class="sxs-lookup"><span data-stu-id="bc420-214">-128...127</span></span> |
|                   | <span data-ttu-id="bc420-215">16</span><span class="sxs-lookup"><span data-stu-id="bc420-215">16</span></span>       | `short`   | <span data-ttu-id="bc420-216">-32768... 32, 767</span><span class="sxs-lookup"><span data-stu-id="bc420-216">-32,768...32,767</span></span> |
|                   | <span data-ttu-id="bc420-217">32</span><span class="sxs-lookup"><span data-stu-id="bc420-217">32</span></span>       | `int`     | <span data-ttu-id="bc420-218">-2147483648... 2, 147, 483, 647</span><span class="sxs-lookup"><span data-stu-id="bc420-218">-2,147,483,648...2,147,483,647</span></span> |
|                   | <span data-ttu-id="bc420-219">64</span><span class="sxs-lookup"><span data-stu-id="bc420-219">64</span></span>       | `long`    | <span data-ttu-id="bc420-220">-9223372036854775808... 9, 223, 372, 036, 854, 775, 807</span><span class="sxs-lookup"><span data-stu-id="bc420-220">-9,223,372,036,854,775,808...9,223,372,036,854,775,807</span></span> |
| <span data-ttu-id="bc420-221">İşaretsiz integral</span><span class="sxs-lookup"><span data-stu-id="bc420-221">Unsigned integral</span></span> | <span data-ttu-id="bc420-222">8</span><span class="sxs-lookup"><span data-stu-id="bc420-222">8</span></span>        | `byte`    | <span data-ttu-id="bc420-223">0... 255</span><span class="sxs-lookup"><span data-stu-id="bc420-223">0...255</span></span> |
|                   | <span data-ttu-id="bc420-224">16</span><span class="sxs-lookup"><span data-stu-id="bc420-224">16</span></span>       | `ushort`  | <span data-ttu-id="bc420-225">0... 65,</span><span class="sxs-lookup"><span data-stu-id="bc420-225">0...65,535</span></span> |
|                   | <span data-ttu-id="bc420-226">32</span><span class="sxs-lookup"><span data-stu-id="bc420-226">32</span></span>       | `uint`    | <span data-ttu-id="bc420-227">0... 4, 294, 967, 295</span><span class="sxs-lookup"><span data-stu-id="bc420-227">0...4,294,967,295</span></span> |
|                   | <span data-ttu-id="bc420-228">64</span><span class="sxs-lookup"><span data-stu-id="bc420-228">64</span></span>       | `ulong`   | <span data-ttu-id="bc420-229">0... 18, 446,, 073, 709, 551, 615</span><span class="sxs-lookup"><span data-stu-id="bc420-229">0...18,446,744,073,709,551,615</span></span> |
| <span data-ttu-id="bc420-230">Kayan nokta</span><span class="sxs-lookup"><span data-stu-id="bc420-230">Floating point</span></span>    | <span data-ttu-id="bc420-231">32</span><span class="sxs-lookup"><span data-stu-id="bc420-231">32</span></span>       | `float`   | <span data-ttu-id="bc420-232">1,5 × 10 ^ − 45 ila 3,4 × 10 ^ 38, 7 basamaklı duyarlık</span><span class="sxs-lookup"><span data-stu-id="bc420-232">1.5 × 10^−45 to 3.4 × 10^38, 7-digit precision</span></span> |
|                   | <span data-ttu-id="bc420-233">64</span><span class="sxs-lookup"><span data-stu-id="bc420-233">64</span></span>       | `double`  | <span data-ttu-id="bc420-234">5,0 × 10 ^ − 324 ila 1,7 × 10 ^ 308, 15 basamaklı duyarlık</span><span class="sxs-lookup"><span data-stu-id="bc420-234">5.0 × 10^−324 to 1.7 × 10^308, 15-digit precision</span></span> |
| <span data-ttu-id="bc420-235">Ondalık</span><span class="sxs-lookup"><span data-stu-id="bc420-235">Decimal</span></span>           | <span data-ttu-id="bc420-236">128</span><span class="sxs-lookup"><span data-stu-id="bc420-236">128</span></span>      | `decimal` | <span data-ttu-id="bc420-237">1,0 × 10 ^ − 28 ila 7,9 × 10 ^ 28, 28 basamaklı duyarlık</span><span class="sxs-lookup"><span data-stu-id="bc420-237">1.0 × 10^−28 to 7.9 × 10^28, 28-digit precision</span></span> |

<span data-ttu-id="bc420-238">C#programlar yeni türler oluşturmak için ***tür bildirimleri*** kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-238">C# programs use ***type declarations*** to create new types.</span></span> <span data-ttu-id="bc420-239">Tür bildiriminde yeni türün adı ve üyeleri belirtilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-239">A type declaration specifies the name and the members of the new type.</span></span> <span data-ttu-id="bc420-240">C#Türlerin beş kategorisi Kullanıcı tarafından tanımlanabilir: sınıf türleri, yapı türleri, arabirim türleri, sabit listesi türleri ve temsilci türleri.</span><span class="sxs-lookup"><span data-stu-id="bc420-240">Five of C#'s categories of types are user-definable: class types, struct types, interface types, enum types, and delegate types.</span></span>

<span data-ttu-id="bc420-241">Bir sınıf türü, veri üyeleri (alanlar) ve işlev üyeleri (Yöntemler, Özellikler ve diğerleri) içeren bir veri yapısını tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-241">A class type defines a data structure that contains data members (fields) and function members (methods, properties, and others).</span></span> <span data-ttu-id="bc420-242">Sınıf türleri, tek devralma ve çok biçimlilik destekler, türetilmiş sınıfların temel sınıfları genişletebileceği ve özelleştireceği mekanizmalar.</span><span class="sxs-lookup"><span data-stu-id="bc420-242">Class types support single inheritance and polymorphism, mechanisms whereby derived classes can extend and specialize base classes.</span></span>

<span data-ttu-id="bc420-243">Yapı türü, veri üyeleri ve işlev üyeleri olan bir yapıyı temsil eden bir sınıf türüne benzerdir.</span><span class="sxs-lookup"><span data-stu-id="bc420-243">A struct type is similar to a class type in that it represents a structure with data members and function members.</span></span> <span data-ttu-id="bc420-244">Ancak, sınıfların aksine yapılar değer türlerdir ve yığın ayırmayı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="bc420-244">However, unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="bc420-245">Yapı türleri Kullanıcı tarafından belirtilen devralmayı desteklemez ve tüm yapı türleri örtülü olarak türünden `object`devralınır.</span><span class="sxs-lookup"><span data-stu-id="bc420-245">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="bc420-246">Arabirim türü, bir sözleşmeyi adlandırılmış bir ortak işlev üyeleri kümesi olarak tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-246">An interface type defines a contract as a named set of public function members.</span></span> <span data-ttu-id="bc420-247">Arabirim uygulayan bir sınıf veya yapı, arabirimin işlev üyelerinin uygulamalarını sağlamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-247">A class or struct that implements an interface must provide implementations of the interface's function members.</span></span> <span data-ttu-id="bc420-248">Bir arabirim birden çok temel arabirimden devralınabilir ve bir sınıf ya da yapı birden çok arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-248">An interface may inherit from multiple base interfaces, and a class or struct may implement multiple interfaces.</span></span>

<span data-ttu-id="bc420-249">Bir temsilci türü, belirli bir parametre listesi ve dönüş türü olan yöntemlere yapılan başvuruları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="bc420-249">A delegate type represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="bc420-250">Temsilciler, yöntemleri değişkenlere atanabilecek ve parametre olarak geçirilen varlıklar olarak işleme olanağı tanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-250">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="bc420-251">Temsilciler, bazı diğer dillerde bulunan işlev işaretçileri kavramına benzerdir, ancak işlev işaretçilerinden farklı olarak Temsilciler nesne yönelimli ve tür açısından güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-251">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="bc420-252">Sınıf, yapı, arabirim ve temsilci türleri tüm genel türleri destekler ve diğer türlerle parametrelenebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-252">Class, struct, interface and delegate types all support generics, whereby they can be parameterized with other types.</span></span>

<span data-ttu-id="bc420-253">Sabit listesi türü, adlandırılmış sabitleri olan ayrı bir türdür.</span><span class="sxs-lookup"><span data-stu-id="bc420-253">An enum type is a distinct type with named constants.</span></span> <span data-ttu-id="bc420-254">Her numaralandırma türünün, sekiz integral türünden biri olması gereken temel bir türü vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-254">Every enum type has an underlying type, which must be one of the eight integral types.</span></span> <span data-ttu-id="bc420-255">Bir sabit listesi türünün değer kümesi, temel alınan türün değerleri kümesiyle aynıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-255">The set of values of an enum type is the same as the set of values of the underlying type.</span></span>

<span data-ttu-id="bc420-256">C#herhangi bir türdeki tek ve çok boyutlu dizileri destekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-256">C# supports single- and multi-dimensional arrays of any type.</span></span> <span data-ttu-id="bc420-257">Yukarıda listelenen türlerin aksine,, kullanılmadan önce dizi türlerinin bildirilmesini gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="bc420-257">Unlike the types listed above, array types do not have to be declared before they can be used.</span></span> <span data-ttu-id="bc420-258">Bunun yerine, dizi türleri Köşeli parantezlerle bir tür adı izleyerek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc420-258">Instead, array types are constructed by following a type name with square brackets.</span></span> <span data-ttu-id="bc420-259">Örneğin, `int[]` tek boyutlu bir `int` `int`diziyse, `int[,]` iki boyutlu bir dizidir ve `int[][]` tek boyutlu dizilerinin tek boyutlu dizilerdir `int`.</span><span class="sxs-lookup"><span data-stu-id="bc420-259">For example, `int[]` is a single-dimensional array of `int`, `int[,]` is a two-dimensional array of `int`, and `int[][]` is a single-dimensional array of single-dimensional arrays of `int`.</span></span>

<span data-ttu-id="bc420-260">Null yapılabilir türler, kullanılmadan önce de bildirilemez.</span><span class="sxs-lookup"><span data-stu-id="bc420-260">Nullable types also do not have to be declared before they can be used.</span></span> <span data-ttu-id="bc420-261">Her null yapılamayan değer türü `T` için, karşılık gelen bir null yapılabilir tür `T?`vardır ve bu, ek bir değer `null`tutabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-261">For each non-nullable value type `T` there is a corresponding nullable type `T?`, which can hold an additional value `null`.</span></span> <span data-ttu-id="bc420-262">Örneğin, `int?` herhangi bir 32 bit tamsayı veya değeri `null`tutabilecek bir türdür.</span><span class="sxs-lookup"><span data-stu-id="bc420-262">For instance, `int?` is a type that can hold any 32 bit integer or the value `null`.</span></span>

<span data-ttu-id="bc420-263">C#tür sistemi, herhangi bir tür değeri bir nesne olarak işlenemeyeceği şekilde birleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-263">C#'s type system is unified such that a value of any type can be treated as an object.</span></span> <span data-ttu-id="bc420-264">Her tür doğrudan C# veya dolaylı olarak `object` sınıf türünden türetilir ve `object` tüm türlerin en son temel sınıfıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-264">Every type in C# directly or indirectly derives from the `object` class type, and `object` is the ultimate base class of all types.</span></span> <span data-ttu-id="bc420-265">Başvuru türlerinin değerleri, yalnızca değerleri tür `object`olarak görüntüleyerek nesne olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-265">Values of reference types are treated as objects simply by viewing the values as type `object`.</span></span> <span data-ttu-id="bc420-266">Değer türlerinin değerleri, ***kutulama*** ve ***kutudan*** çıkarma işlemleri gerçekleştirerek nesneler olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-266">Values of value types are treated as objects by performing ***boxing*** and ***unboxing*** operations.</span></span> <span data-ttu-id="bc420-267">Aşağıdaki örnekte, bir `int` değeri öğesine `object` dönüştürülüp öğesine `int`yeniden döndürülür.</span><span class="sxs-lookup"><span data-stu-id="bc420-267">In the following example, an `int` value is converted to `object` and back again to `int`.</span></span>

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
<span data-ttu-id="bc420-268">Değer türünde bir değer türüne `object`dönüştürüldüğünde, "Box" olarak da bilinen bir nesne örneği değeri tutacak şekilde ayrılır ve değer bu kutuya kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-268">When a value of a value type is converted to type `object`, an object instance, also called a "box," is allocated to hold the value, and the value is copied into that box.</span></span> <span data-ttu-id="bc420-269">Buna karşılık, bir `object` başvuru bir değer türüne dönüştürülzaman, başvurulan nesnenin doğru değer türünün bir kutusu olması ve denetim başarılı olursa, kutudaki değer kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-269">Conversely, when an `object` reference is cast to a value type, a check is made that the referenced object is a box of the correct value type, and, if the check succeeds, the value in the box is copied out.</span></span>

<span data-ttu-id="bc420-270">C#öğesinin Birleşik tür sistemi etkin bir şekilde değer türlerinin "isteğe bağlı" nesneler olabileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="bc420-270">C#'s unified type system effectively means that value types can become objects "on demand."</span></span> <span data-ttu-id="bc420-271">Birleşme nedeniyle, türü `object` kullanan genel amaçlı kitaplıklar, başvuru türleri ve değer türleriyle birlikte kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-271">Because of the unification, general-purpose libraries that use type `object` can be used with both reference types and value types.</span></span>

<span data-ttu-id="bc420-272">İçinde C#alanlar, dizi öğeleri, yerel değişkenler ve parametreler dahil olmak üzere birkaç tür ***değişken*** vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-272">There are several kinds of ***variables*** in C#, including fields, array elements, local variables, and parameters.</span></span> <span data-ttu-id="bc420-273">Değişkenler, depolama konumlarını temsil eder ve her değişken, aşağıdaki tabloda gösterildiği gibi, değişkende hangi değerlerin depolanabileceğini belirleyen bir tür içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-273">Variables represent storage locations, and every variable has a type that determines what values can be stored in the variable, as shown by the following table.</span></span>


| <span data-ttu-id="bc420-274">__Değişken türü__</span><span class="sxs-lookup"><span data-stu-id="bc420-274">__Type of Variable__</span></span>    | <span data-ttu-id="bc420-275">__Olası Içerikler__</span><span class="sxs-lookup"><span data-stu-id="bc420-275">__Possible Contents__</span></span> |
|-------------------------|-----------------------|
| <span data-ttu-id="bc420-276">Null yapılamayan değer türü</span><span class="sxs-lookup"><span data-stu-id="bc420-276">Non-nullable value type</span></span> | <span data-ttu-id="bc420-277">Bu tam türden bir değer</span><span class="sxs-lookup"><span data-stu-id="bc420-277">A value of that exact type</span></span> |
| <span data-ttu-id="bc420-278">Null yapılabilir değer türü</span><span class="sxs-lookup"><span data-stu-id="bc420-278">Nullable value type</span></span>     | <span data-ttu-id="bc420-279">Null değer veya bu tam tür değeri</span><span class="sxs-lookup"><span data-stu-id="bc420-279">A null value or a value of that exact type</span></span> |
| `object`                | <span data-ttu-id="bc420-280">Bir null başvurusu, herhangi bir başvuru türünün nesnesine başvuru veya herhangi bir değer türünün paketlenmiş değerine başvuru</span><span class="sxs-lookup"><span data-stu-id="bc420-280">A null reference, a reference to an object of any reference type, or a reference to a boxed value of any value type</span></span> |
| <span data-ttu-id="bc420-281">Sınıf türü</span><span class="sxs-lookup"><span data-stu-id="bc420-281">Class type</span></span>              | <span data-ttu-id="bc420-282">Null başvurusu, bu sınıf türünün bir örneğine başvuru veya bu sınıf türünden türetilmiş bir sınıfın örneğine başvuru</span><span class="sxs-lookup"><span data-stu-id="bc420-282">A null reference, a reference to an instance of that class type, or a reference to an instance of a class derived from that class type</span></span> |
| <span data-ttu-id="bc420-283">Arabirim türü</span><span class="sxs-lookup"><span data-stu-id="bc420-283">Interface type</span></span>          | <span data-ttu-id="bc420-284">Null başvurusu, bu arabirim türünü uygulayan bir sınıf türü örneğine başvuru veya bu arabirim türünü uygulayan bir değer türünün paketlenmiş değerine başvuru</span><span class="sxs-lookup"><span data-stu-id="bc420-284">A null reference, a reference to an instance of a class type that implements that interface type, or a reference to a boxed value of a value type that implements that interface type</span></span> |
| <span data-ttu-id="bc420-285">Dizi türü</span><span class="sxs-lookup"><span data-stu-id="bc420-285">Array type</span></span>              | <span data-ttu-id="bc420-286">Null başvuru, bu dizi türünün bir örneğine başvuru veya uyumlu bir dizi türü örneğine başvuru</span><span class="sxs-lookup"><span data-stu-id="bc420-286">A null reference, a reference to an instance of that array type, or a reference to an instance of a compatible array type</span></span> |
| <span data-ttu-id="bc420-287">Temsilci türü</span><span class="sxs-lookup"><span data-stu-id="bc420-287">Delegate type</span></span>           | <span data-ttu-id="bc420-288">Bu temsilci türünün bir örneğine null başvuru veya başvuru</span><span class="sxs-lookup"><span data-stu-id="bc420-288">A null reference or a reference to an instance of that delegate type</span></span> |

## <a name="expressions"></a><span data-ttu-id="bc420-289">İfadeler</span><span class="sxs-lookup"><span data-stu-id="bc420-289">Expressions</span></span>

<span data-ttu-id="bc420-290">***İfadeler*** , ***işlenenler*** ve ***işleçlerden***oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc420-290">***Expressions*** are constructed from ***operands*** and ***operators***.</span></span> <span data-ttu-id="bc420-291">Bir ifadenin işleçleri, işlenenlerin hangi işlemleri uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-291">The operators of an expression indicate which operations to apply to the operands.</span></span> <span data-ttu-id="bc420-292">`+`İşleç örnekleri `-` ,`/`,, ve`new`içerir. `*`</span><span class="sxs-lookup"><span data-stu-id="bc420-292">Examples of operators include `+`, `-`, `*`, `/`, and `new`.</span></span> <span data-ttu-id="bc420-293">İşlenenlerin örnekleri, sabit değerleri, alanları, yerel değişkenleri ve ifadeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-293">Examples of operands include literals, fields, local variables, and expressions.</span></span>

<span data-ttu-id="bc420-294">Bir ifade birden çok işleç içerdiğinde, işleçlerin ***önceliği*** ayrı işleçlerin değerlendirilme sırasını denetler.</span><span class="sxs-lookup"><span data-stu-id="bc420-294">When an expression contains multiple operators, the ***precedence*** of the operators controls the order in which the individual operators are evaluated.</span></span> <span data-ttu-id="bc420-295">Örneğin, `x + y * z` `*` işleç işleçten`+` daha yüksek önceliğe `x + (y * z)` sahip olduğu için ifade değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-295">For example, the expression `x + y * z` is evaluated as `x + (y * z)` because the `*` operator has higher precedence than the `+` operator.</span></span>

<span data-ttu-id="bc420-296">Çoğu işleç ***aşırı***yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-296">Most operators can be ***overloaded***.</span></span> <span data-ttu-id="bc420-297">İşleç aşırı yüklemesi, Kullanıcı tanımlı operatör uygulamalarının bir veya her ikisinin de Kullanıcı tanımlı sınıf veya yapı türünde olduğu işlemler için belirtilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="bc420-297">Operator overloading permits user-defined operator implementations to be specified for operations where one or both of the operands are of a user-defined class or struct type.</span></span>

<span data-ttu-id="bc420-298">Aşağıdaki tabloda, işleç C#kategorilerinin en yüksekten en düşüğe öncelik sırasına göre listelenmesi ve işleçleri özetlenmektedir.</span><span class="sxs-lookup"><span data-stu-id="bc420-298">The following table summarizes C#'s operators, listing the operator categories in order of precedence from highest to lowest.</span></span> <span data-ttu-id="bc420-299">Aynı kategorideki operatörler eşit önceliğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bc420-299">Operators in the same category have equal precedence.</span></span>


| <span data-ttu-id="bc420-300">__Kategori__</span><span class="sxs-lookup"><span data-stu-id="bc420-300">__Category__</span></span>                     | <span data-ttu-id="bc420-301">__İfadesini__</span><span class="sxs-lookup"><span data-stu-id="bc420-301">__Expression__</span></span>    | <span data-ttu-id="bc420-302">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="bc420-302">__Description__</span></span> |
|----------------------------------|-------------------|-----------------|
| <span data-ttu-id="bc420-303">Birincil</span><span class="sxs-lookup"><span data-stu-id="bc420-303">Primary</span></span>                          | `x.m`             | <span data-ttu-id="bc420-304">Üye erişimi</span><span class="sxs-lookup"><span data-stu-id="bc420-304">Member access</span></span> |
|                                  | `x(...)`          | <span data-ttu-id="bc420-305">Yöntem ve temsilci çağırma</span><span class="sxs-lookup"><span data-stu-id="bc420-305">Method and delegate invocation</span></span> |
|                                  | `x[...]`          | <span data-ttu-id="bc420-306">Dizi ve dizinleyici erişimi</span><span class="sxs-lookup"><span data-stu-id="bc420-306">Array and indexer access</span></span> |
|                                  | `x++`             | <span data-ttu-id="bc420-307">Artırım sonrası</span><span class="sxs-lookup"><span data-stu-id="bc420-307">Post-increment</span></span> |
|                                  | `x--`             | <span data-ttu-id="bc420-308">Azaltım sonrası</span><span class="sxs-lookup"><span data-stu-id="bc420-308">Post-decrement</span></span> |
|                                  | `new T(...)`      | <span data-ttu-id="bc420-309">Nesne ve temsilci oluşturma</span><span class="sxs-lookup"><span data-stu-id="bc420-309">Object and delegate creation</span></span> |
|                                  | `new T(...){...}` | <span data-ttu-id="bc420-310">Başlatıcı ile nesne oluşturma</span><span class="sxs-lookup"><span data-stu-id="bc420-310">Object creation with initializer</span></span> |
|                                  | `new {...}`       | <span data-ttu-id="bc420-311">Anonim nesne Başlatıcısı</span><span class="sxs-lookup"><span data-stu-id="bc420-311">Anonymous object initializer</span></span> |
|                                  | `new T[...]`      | <span data-ttu-id="bc420-312">Dizi oluşturma</span><span class="sxs-lookup"><span data-stu-id="bc420-312">Array creation</span></span> |
|                                  | `typeof(T)`       | <span data-ttu-id="bc420-313">İçin `System.Type` nesne al`T`</span><span class="sxs-lookup"><span data-stu-id="bc420-313">Obtain `System.Type` object for `T`</span></span> |
|                                  | `checked(x)`      | <span data-ttu-id="bc420-314">İşaretli bağlamında ifade değerlendirme</span><span class="sxs-lookup"><span data-stu-id="bc420-314">Evaluate expression in checked context</span></span> |
|                                  | `unchecked(x)`    | <span data-ttu-id="bc420-315">İşaretlenmemiş bağlamında ifade değerlendirme</span><span class="sxs-lookup"><span data-stu-id="bc420-315">Evaluate expression in unchecked context</span></span> |
|                                  | `default(T)`      | <span data-ttu-id="bc420-316">Türün varsayılan değerini Al`T`</span><span class="sxs-lookup"><span data-stu-id="bc420-316">Obtain default value of type `T`</span></span> |
|                                  | `delegate {...}`  | <span data-ttu-id="bc420-317">Anonim işlevi (anonim yöntemi)</span><span class="sxs-lookup"><span data-stu-id="bc420-317">Anonymous function (anonymous method)</span></span> |
| <span data-ttu-id="bc420-318">Li</span><span class="sxs-lookup"><span data-stu-id="bc420-318">Unary</span></span>                            | `+x`              | <span data-ttu-id="bc420-319">Kimlik</span><span class="sxs-lookup"><span data-stu-id="bc420-319">Identity</span></span> |
|                                  | `-x`              | <span data-ttu-id="bc420-320">Olumsuzlama</span><span class="sxs-lookup"><span data-stu-id="bc420-320">Negation</span></span> |
|                                  | `!x`              | <span data-ttu-id="bc420-321">Mantıksal olumsuzlama</span><span class="sxs-lookup"><span data-stu-id="bc420-321">Logical negation</span></span> |
|                                  | `~x`              | <span data-ttu-id="bc420-322">Bitwise olumsuzlama</span><span class="sxs-lookup"><span data-stu-id="bc420-322">Bitwise negation</span></span> |
|                                  | `++x`             | <span data-ttu-id="bc420-323">Artırım öncesi</span><span class="sxs-lookup"><span data-stu-id="bc420-323">Pre-increment</span></span> |
|                                  | `--x`             | <span data-ttu-id="bc420-324">Azaltım öncesi</span><span class="sxs-lookup"><span data-stu-id="bc420-324">Pre-decrement</span></span> |
|                                  | `(T)x`            | <span data-ttu-id="bc420-325">Açıkça türe `x` Dönüştür`T`</span><span class="sxs-lookup"><span data-stu-id="bc420-325">Explicitly convert `x` to type `T`</span></span> |
|                                  | `await x`         | <span data-ttu-id="bc420-326">Zaman uyumsuz `x` olarak işlemin tamamlanmasını bekleyin</span><span class="sxs-lookup"><span data-stu-id="bc420-326">Asynchronously wait for `x` to complete</span></span> |
| <span data-ttu-id="bc420-327">Çarpma</span><span class="sxs-lookup"><span data-stu-id="bc420-327">Multiplicative</span></span>                   | `x * y`           | <span data-ttu-id="bc420-328">Çarpma</span><span class="sxs-lookup"><span data-stu-id="bc420-328">Multiplication</span></span> |
|                                  | `x / y`           | <span data-ttu-id="bc420-329">Bölme</span><span class="sxs-lookup"><span data-stu-id="bc420-329">Division</span></span> |
|                                  | `x % y`           | <span data-ttu-id="bc420-330">Kalan</span><span class="sxs-lookup"><span data-stu-id="bc420-330">Remainder</span></span> |
| <span data-ttu-id="bc420-331">Msal</span><span class="sxs-lookup"><span data-stu-id="bc420-331">Additive</span></span>                         | `x + y`           | <span data-ttu-id="bc420-332">Toplama, dize bitiştirme, temsilci birleşimi</span><span class="sxs-lookup"><span data-stu-id="bc420-332">Addition, string concatenation, delegate combination</span></span> |
|                                  | `x - y`           | <span data-ttu-id="bc420-333">Çıkarma, temsilci kaldırma</span><span class="sxs-lookup"><span data-stu-id="bc420-333">Subtraction, delegate removal</span></span> |
| <span data-ttu-id="bc420-334">Shift</span><span class="sxs-lookup"><span data-stu-id="bc420-334">Shift</span></span>                            | `x << y`          | <span data-ttu-id="bc420-335">Sola kaydırma</span><span class="sxs-lookup"><span data-stu-id="bc420-335">Shift left</span></span> |
|                                  | `x >> y`          | <span data-ttu-id="bc420-336">Sağa kaydırma</span><span class="sxs-lookup"><span data-stu-id="bc420-336">Shift right</span></span> |
| <span data-ttu-id="bc420-337">İlişkisel ve tür testi</span><span class="sxs-lookup"><span data-stu-id="bc420-337">Relational and type testing</span></span>      | `x < y`           | <span data-ttu-id="bc420-338">Küçüktür</span><span class="sxs-lookup"><span data-stu-id="bc420-338">Less than</span></span> |
|                                  | `x > y`           | <span data-ttu-id="bc420-339">Büyüktür</span><span class="sxs-lookup"><span data-stu-id="bc420-339">Greater than</span></span> |
|                                  | `x <= y`          | <span data-ttu-id="bc420-340">Küçük veya eşittir</span><span class="sxs-lookup"><span data-stu-id="bc420-340">Less than or equal</span></span> |
|                                  | `x >= y`          | <span data-ttu-id="bc420-341">Büyük veya eşittir</span><span class="sxs-lookup"><span data-stu-id="bc420-341">Greater than or equal</span></span> |
|                                  | `x is T`          | <span data-ttu-id="bc420-342">Bir `true` ise`T`, tersidurumda`false` döndürün `x`</span><span class="sxs-lookup"><span data-stu-id="bc420-342">Return `true` if `x` is a `T`, `false` otherwise</span></span> |
|                                  | `x as T`          | <span data-ttu-id="bc420-343">Dönüş `x` olarak `T`yazılmış veya `null` `x``T`</span><span class="sxs-lookup"><span data-stu-id="bc420-343">Return `x` typed as `T`, or `null` if `x` is not a `T`</span></span> |
| <span data-ttu-id="bc420-344">Eşitlik</span><span class="sxs-lookup"><span data-stu-id="bc420-344">Equality</span></span>                         | `x == y`          | <span data-ttu-id="bc420-345">Eşittir</span><span class="sxs-lookup"><span data-stu-id="bc420-345">Equal</span></span>      |
|                                  | `x != y`          | <span data-ttu-id="bc420-346">Eşit değildir</span><span class="sxs-lookup"><span data-stu-id="bc420-346">Not equal</span></span> |
| <span data-ttu-id="bc420-347">Mantıksal VE</span><span class="sxs-lookup"><span data-stu-id="bc420-347">Logical AND</span></span>                      | `x & y`           | <span data-ttu-id="bc420-348">Tamsayı bit düzeyinde ve, Boolean mantıksal ve</span><span class="sxs-lookup"><span data-stu-id="bc420-348">Integer bitwise AND, boolean logical AND</span></span> |
| <span data-ttu-id="bc420-349">Mantıksal XOR</span><span class="sxs-lookup"><span data-stu-id="bc420-349">Logical XOR</span></span>                      | `x ^ y`           | <span data-ttu-id="bc420-350">Tamsayı bitwise XOR, Boolean mantıksal XOR</span><span class="sxs-lookup"><span data-stu-id="bc420-350">Integer bitwise XOR, boolean logical XOR</span></span> |
| <span data-ttu-id="bc420-351">Mantıksal VEYA</span><span class="sxs-lookup"><span data-stu-id="bc420-351">Logical OR</span></span>                       | <code>x &#124; y</code> | <span data-ttu-id="bc420-352">Tamsayı bitwise VEYA, boolean mantıksal VEYA</span><span class="sxs-lookup"><span data-stu-id="bc420-352">Integer bitwise OR, boolean logical OR</span></span> |
| <span data-ttu-id="bc420-353">Koşullu VE</span><span class="sxs-lookup"><span data-stu-id="bc420-353">Conditional AND</span></span>                  | `x && y`          | <span data-ttu-id="bc420-354">Yalnızca `y` şu durumlarda `x` değerlendirilir`true`</span><span class="sxs-lookup"><span data-stu-id="bc420-354">Evaluates `y` only if `x` is `true`</span></span> |
| <span data-ttu-id="bc420-355">Koşullu VEYA</span><span class="sxs-lookup"><span data-stu-id="bc420-355">Conditional OR</span></span>                   | <code>x &#124;&#124; y</code> | <span data-ttu-id="bc420-356">Yalnızca `y` şu durumlarda `x` değerlendirilir`false`</span><span class="sxs-lookup"><span data-stu-id="bc420-356">Evaluates `y` only if `x` is `false`</span></span> |
| <span data-ttu-id="bc420-357">Null birleşim</span><span class="sxs-lookup"><span data-stu-id="bc420-357">Null coalescing</span></span>                  | `x ?? y`          | <span data-ttu-id="bc420-358">`y` ,Değilse`x`olarakdeğerlendirilir `null` `x`</span><span class="sxs-lookup"><span data-stu-id="bc420-358">Evaluates to `y` if `x` is `null`, to `x` otherwise</span></span> |
| <span data-ttu-id="bc420-359">Koşullu</span><span class="sxs-lookup"><span data-stu-id="bc420-359">Conditional</span></span>                      | `x ? y : z`       | <span data-ttu-id="bc420-360">Olupolmadığını`x` değerlendirir `y` `true` `z` `x``false`</span><span class="sxs-lookup"><span data-stu-id="bc420-360">Evaluates `y` if `x` is `true`, `z` if `x` is `false`</span></span> |
| <span data-ttu-id="bc420-361">Atama veya anonim işlev</span><span class="sxs-lookup"><span data-stu-id="bc420-361">Assignment or anonymous function</span></span> | `x = y`           | <span data-ttu-id="bc420-362">Atama</span><span class="sxs-lookup"><span data-stu-id="bc420-362">Assignment</span></span> |
|                                  | `x op= y`         | <span data-ttu-id="bc420-363">Bileşik atama; Desteklenen işleçler şunlardır `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=`<code>&#124;=</code></span><span class="sxs-lookup"><span data-stu-id="bc420-363">Compound assignment; supported operators are `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` <code>&#124;=</code></span></span> |
|                                  | `(T x) => y`      | <span data-ttu-id="bc420-364">Anonim işlevi (lambda ifadesi)</span><span class="sxs-lookup"><span data-stu-id="bc420-364">Anonymous function (lambda expression)</span></span> |

## <a name="statements"></a><span data-ttu-id="bc420-365">Deyimler</span><span class="sxs-lookup"><span data-stu-id="bc420-365">Statements</span></span>

<span data-ttu-id="bc420-366">Bir programın eylemleri ***deyimler***kullanılarak ifade edilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-366">The actions of a program are expressed using ***statements***.</span></span> <span data-ttu-id="bc420-367">C#, birden çok farklı türde deyimi destekler, bu sayı katıştırılmış deyimler bakımından tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-367">C# supports several different kinds of statements, a number of which are defined in terms of embedded statements.</span></span>

<span data-ttu-id="bc420-368">Bir ***blok*** , tek bir ifadeye izin verilen bağlamlarda birden çok deyimin yazılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="bc420-368">A ***block*** permits multiple statements to be written in contexts where a single statement is allowed.</span></span> <span data-ttu-id="bc420-369">Bir blok, sınırlayıcılar `{` ve `}`arasında yazılmış deyimler listesinden oluşur.</span><span class="sxs-lookup"><span data-stu-id="bc420-369">A block consists of a list of statements written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="bc420-370">***Bildirim deyimleri*** yerel değişkenleri ve sabitleri bildirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-370">***Declaration statements*** are used to declare local variables and constants.</span></span>

<span data-ttu-id="bc420-371">***İfade deyimleri*** , ifadeleri değerlendirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-371">***Expression statements*** are used to evaluate expressions.</span></span> <span data-ttu-id="bc420-372">Deyim olarak kullanılabilecek ifadeler, yöntem etkinleştirmeleri, `new` işleci kullanılarak nesne ayırmaları, `=` ve bileşik atama işleçleri kullanan atamalar, artırma ve azaltma işlemleri `++` ve`--` işleçleri ve await ifadeleri.</span><span class="sxs-lookup"><span data-stu-id="bc420-372">Expressions that can be used as statements include method invocations, object allocations using the `new` operator, assignments using `=` and the compound assignment operators, increment and decrement operations using the `++` and `--` operators and await expressions.</span></span>

<span data-ttu-id="bc420-373">***Seçim deyimleri*** , bazı deyimlerin değerine göre yürütme için bir dizi olası deyimden birini seçmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-373">***Selection statements*** are used to select one of a number of possible statements for execution based on the value of some expression.</span></span> <span data-ttu-id="bc420-374">Bu grupta `if` ve `switch` deyimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-374">In this group are the `if` and `switch` statements.</span></span>

<span data-ttu-id="bc420-375">***Yineleme deyimleri*** , gömülü bir deyimi tekrar tekrar yürütmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-375">***Iteration statements*** are used to repeatedly execute an embedded statement.</span></span> <span data-ttu-id="bc420-376">`while`Bu grupta `do` ,,ve`foreach`deyimleribulunur. `for`</span><span class="sxs-lookup"><span data-stu-id="bc420-376">In this group are the `while`, `do`, `for`, and `foreach` statements.</span></span>

<span data-ttu-id="bc420-377">***Sıçrama deyimleri*** , denetimi aktarmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-377">***Jump statements*** are used to transfer control.</span></span> <span data-ttu-id="bc420-378">Bu `break`grupta `continue`, ,,`throw`, ve deyimleribulunur`yield` . `goto` `return`</span><span class="sxs-lookup"><span data-stu-id="bc420-378">In this group are the `break`, `continue`, `goto`, `throw`, `return`, and `yield` statements.</span></span>

<span data-ttu-id="bc420-379">`try`... Bildiri, bir bloğun yürütülmesi sırasında oluşan özel durumları yakalamak için kullanılır `try`ve... `catch` `finally` bildirim, her zaman yürütülen ve özel durumun gerçekleşmediğini belirten sonlandırma kodunu belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-379">The `try`...`catch` statement is used to catch exceptions that occur during execution of a block, and the `try`...`finally` statement is used to specify finalization code that is always executed, whether an exception occurred or not.</span></span>

<span data-ttu-id="bc420-380">`checked` Ve`unchecked` deyimleri, tamsayı türü aritmetik işlemler ve dönüştürmeler için taşma denetimi bağlamını denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-380">The `checked` and `unchecked` statements are used to control the overflow checking context for integral-type arithmetic operations and conversions.</span></span>

<span data-ttu-id="bc420-381">Bu `lock` ifade, belirli bir nesne için karşılıklı dışlama kilidini almak, bir ifadeyi yürütmek ve sonra kilidi serbest bırakmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-381">The `lock` statement is used to obtain the mutual-exclusion lock for a given object, execute a statement, and then release the lock.</span></span>

<span data-ttu-id="bc420-382">`using` İfade, kaynak almak, bir ifadeyi yürütmek ve ardından bu kaynağı atmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-382">The `using` statement is used to obtain a resource, execute a statement, and then dispose of that resource.</span></span>

<span data-ttu-id="bc420-383">Aşağıda her bir tür deyimin örnekleri verilmiştir</span><span class="sxs-lookup"><span data-stu-id="bc420-383">Below are examples of each kind of statement</span></span>

<span data-ttu-id="bc420-384">__Yerel değişken bildirimleri__</span><span class="sxs-lookup"><span data-stu-id="bc420-384">__Local variable declarations__</span></span>

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


<span data-ttu-id="bc420-385">__Yerel sabit bildirimi__</span><span class="sxs-lookup"><span data-stu-id="bc420-385">__Local constant declaration__</span></span>

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


<span data-ttu-id="bc420-386">__İfade deyimi__</span><span class="sxs-lookup"><span data-stu-id="bc420-386">__Expression statement__</span></span>

```csharp
static void Main() {
    int i;
    i = 123;                // Expression statement
    Console.WriteLine(i);   // Expression statement
    i++;                    // Expression statement
    Console.WriteLine(i);   // Expression statement
}
```

<span data-ttu-id="bc420-387">__`if`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-387">__`if` statement__</span></span>

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


<span data-ttu-id="bc420-388">__`switch`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-388">__`switch` statement__</span></span>

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

<span data-ttu-id="bc420-389">__`while`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-389">__`while` statement__</span></span>

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


<span data-ttu-id="bc420-390">__`do`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-390">__`do` statement__</span></span>

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

<span data-ttu-id="bc420-391">__`for`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-391">__`for` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="bc420-392">__`foreach`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-392">__`foreach` statement__</span></span>

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="bc420-393">__`break`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-393">__`break` statement__</span></span>

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

<span data-ttu-id="bc420-394">__`continue`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-394">__`continue` statement__</span></span>

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

<span data-ttu-id="bc420-395">__`goto`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-395">__`goto` statement__</span></span>

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

<span data-ttu-id="bc420-396">__`return`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-396">__`return` statement__</span></span>

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

<span data-ttu-id="bc420-397">__`yield`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-397">__`yield` statement__</span></span>

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

<span data-ttu-id="bc420-398">__`throw`ve `try` deyimleri__</span><span class="sxs-lookup"><span data-stu-id="bc420-398">__`throw` and `try` statements__</span></span>

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

<span data-ttu-id="bc420-399">__`checked`ve `unchecked` deyimleri__</span><span class="sxs-lookup"><span data-stu-id="bc420-399">__`checked` and `unchecked` statements__</span></span>

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

<span data-ttu-id="bc420-400">__`lock`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-400">__`lock` statement__</span></span>

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

<span data-ttu-id="bc420-401">__`using`Ekstre__</span><span class="sxs-lookup"><span data-stu-id="bc420-401">__`using` statement__</span></span>

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## <a name="classes-and-objects"></a><span data-ttu-id="bc420-402">Sınıflar ve nesneler</span><span class="sxs-lookup"><span data-stu-id="bc420-402">Classes and objects</span></span>

<span data-ttu-id="bc420-403">***Sınıflar*** , türlerin en temel C#larıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-403">***Classes*** are the most fundamental of C#'s types.</span></span> <span data-ttu-id="bc420-404">Bir sınıf, durumu (alanları) ve eylemleri (Yöntemler ve diğer işlev üyelerini) tek bir birimde birleştiren bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-404">A class is a data structure that combines state (fields) and actions (methods and other function members) in a single unit.</span></span> <span data-ttu-id="bc420-405">Sınıf, ***nesne***olarak da bilinen, sınıfının dinamik olarak oluşturulan ***örnekleri*** için bir tanım sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-405">A class provides a definition for dynamically created ***instances*** of the class, also known as ***objects***.</span></span> <span data-ttu-id="bc420-406">Sınıflar, ***Devralma*** ve çok ***biçimlilik***desteği, ***türetilmiş sınıfların*** ***temel sınıfları***genişletebileceği ve özelleştirilebilecek mekanizmalar.</span><span class="sxs-lookup"><span data-stu-id="bc420-406">Classes support ***inheritance*** and ***polymorphism***, mechanisms whereby ***derived classes*** can extend and specialize ***base classes***.</span></span>

<span data-ttu-id="bc420-407">Yeni sınıflar sınıf bildirimleri kullanılarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc420-407">New classes are created using class declarations.</span></span> <span data-ttu-id="bc420-408">Sınıf bildirimi, sınıfın özniteliklerini ve değiştiricilerini, sınıfın adını, Taban sınıfını (belirtilmişse) ve sınıf tarafından uygulanan arabirimleri belirten bir üstbilgiyle başlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-408">A class declaration starts with a header that specifies the attributes and modifiers of the class, the name of the class, the base class (if given), and the interfaces implemented by the class.</span></span> <span data-ttu-id="bc420-409">Üst bilgi, sınırlayıcılar `{` ve `}`arasında yazılmış üye bildirimlerinin listesinden oluşan sınıf gövdesinden gelir.</span><span class="sxs-lookup"><span data-stu-id="bc420-409">The header is followed by the class body, which consists of a list of member declarations written between the delimiters `{` and `}`.</span></span>

<span data-ttu-id="bc420-410">Aşağıda adlı `Point`basit bir sınıfın bildirimi verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="bc420-410">The following is a declaration of a simple class named `Point`:</span></span>

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
<span data-ttu-id="bc420-411">Sınıf örnekleri, yeni bir örnek için `new` bellek ayıran işleç kullanılarak oluşturulur, örneği başlatmak için bir oluşturucu çağırır ve örneğe bir başvuru döndürür.</span><span class="sxs-lookup"><span data-stu-id="bc420-411">Instances of classes are created using the `new` operator, which allocates memory for a new instance, invokes a constructor to initialize the instance, and returns a reference to the instance.</span></span> <span data-ttu-id="bc420-412">Aşağıdaki deyimler iki `Point` nesne oluşturur ve bu nesnelere başvuruları iki değişken halinde depolar:</span><span class="sxs-lookup"><span data-stu-id="bc420-412">The following statements create two `Point` objects and store references to those objects in two variables:</span></span>

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
<span data-ttu-id="bc420-413">Nesne artık kullanımda olmadığında bir nesnenin kapladığı bellek otomatik olarak geri kazanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-413">The memory occupied by an object is automatically reclaimed when the object is no longer in use.</span></span> <span data-ttu-id="bc420-414">Üzerinde C#nesneleri açıkça serbest bırakmak gerekli değildir veya mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="bc420-414">It is neither necessary nor possible to explicitly deallocate objects in C#.</span></span>

### <a name="members"></a><span data-ttu-id="bc420-415">Üyeler</span><span class="sxs-lookup"><span data-stu-id="bc420-415">Members</span></span>

<span data-ttu-id="bc420-416">Bir sınıfın üyeleri ***statik Üyeler*** veya ***örnek üyeleridir***.</span><span class="sxs-lookup"><span data-stu-id="bc420-416">The members of a class are either ***static members*** or ***instance members***.</span></span> <span data-ttu-id="bc420-417">Statik Üyeler sınıflara aittir ve örnek üyeleri nesnelere aittir (sınıf örnekleri).</span><span class="sxs-lookup"><span data-stu-id="bc420-417">Static members belong to classes, and instance members belong to objects (instances of classes).</span></span>

<span data-ttu-id="bc420-418">Aşağıdaki tabloda bir sınıfın içerebileceği üye türlerine genel bakış sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bc420-418">The following table provides an overview of the kinds of members a class can contain.</span></span>


| <span data-ttu-id="bc420-419">__Üyesidir__</span><span class="sxs-lookup"><span data-stu-id="bc420-419">__Member__</span></span>   | <span data-ttu-id="bc420-420">__Açıklama__</span><span class="sxs-lookup"><span data-stu-id="bc420-420">__Description__</span></span> |
|------------  |-----------------|
| <span data-ttu-id="bc420-421">Sabitler</span><span class="sxs-lookup"><span data-stu-id="bc420-421">Constants</span></span>    | <span data-ttu-id="bc420-422">Sınıfla ilişkili sabit değerler</span><span class="sxs-lookup"><span data-stu-id="bc420-422">Constant values associated with the class</span></span> |
| <span data-ttu-id="bc420-423">Alanlar</span><span class="sxs-lookup"><span data-stu-id="bc420-423">Fields</span></span>       | <span data-ttu-id="bc420-424">Sınıfın değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bc420-424">Variables of the class</span></span> |
| <span data-ttu-id="bc420-425">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="bc420-425">Methods</span></span>      | <span data-ttu-id="bc420-426">Sınıfı tarafından gerçekleştirilebilecek hesaplamalar ve eylemler</span><span class="sxs-lookup"><span data-stu-id="bc420-426">Computations and actions that can be performed by the class</span></span> |
| <span data-ttu-id="bc420-427">Özellikler</span><span class="sxs-lookup"><span data-stu-id="bc420-427">Properties</span></span>   | <span data-ttu-id="bc420-428">Sınıfın adlandırılmış özelliklerini okuma ve yazma ile ilişkili eylemler</span><span class="sxs-lookup"><span data-stu-id="bc420-428">Actions associated with reading and writing named properties of the class</span></span> |
| <span data-ttu-id="bc420-429">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="bc420-429">Indexers</span></span>     | <span data-ttu-id="bc420-430">Bir dizi gibi sınıfın dizin oluşturma örnekleri ile ilişkili eylemler</span><span class="sxs-lookup"><span data-stu-id="bc420-430">Actions associated with indexing instances of the class like an array</span></span> |
| <span data-ttu-id="bc420-431">Olaylar</span><span class="sxs-lookup"><span data-stu-id="bc420-431">Events</span></span>       | <span data-ttu-id="bc420-432">Sınıfı tarafından oluşturulabilecek bildirimler</span><span class="sxs-lookup"><span data-stu-id="bc420-432">Notifications that can be generated by the class</span></span> |
| <span data-ttu-id="bc420-433">İşleçler</span><span class="sxs-lookup"><span data-stu-id="bc420-433">Operators</span></span>    | <span data-ttu-id="bc420-434">Sınıf tarafından desteklenen dönüşümler ve ifade işleçleri</span><span class="sxs-lookup"><span data-stu-id="bc420-434">Conversions and expression operators supported by the class</span></span> |
| <span data-ttu-id="bc420-435">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="bc420-435">Constructors</span></span> | <span data-ttu-id="bc420-436">Sınıfın veya sınıfın örneklerinin örneğini başlatmak için gereken eylemler</span><span class="sxs-lookup"><span data-stu-id="bc420-436">Actions required to initialize instances of the class or the class itself</span></span> |
| <span data-ttu-id="bc420-437">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="bc420-437">Destructors</span></span>  | <span data-ttu-id="bc420-438">Sınıfın örneklerinden önce gerçekleştirilecek eylemler kalıcı olarak atılır</span><span class="sxs-lookup"><span data-stu-id="bc420-438">Actions to perform before instances of the class are permanently discarded</span></span> |
| <span data-ttu-id="bc420-439">Türler</span><span class="sxs-lookup"><span data-stu-id="bc420-439">Types</span></span>        | <span data-ttu-id="bc420-440">Sınıf tarafından tanımlanan iç içe türler</span><span class="sxs-lookup"><span data-stu-id="bc420-440">Nested types declared by the class</span></span> |

### <a name="accessibility"></a><span data-ttu-id="bc420-441">Erişilebilirlik</span><span class="sxs-lookup"><span data-stu-id="bc420-441">Accessibility</span></span>

<span data-ttu-id="bc420-442">Bir sınıfın her üyesinin ilişkili bir erişilebilirliği vardır ve bu, üyeye erişebilen program metni bölgelerini denetler.</span><span class="sxs-lookup"><span data-stu-id="bc420-442">Each member of a class has an associated accessibility, which controls the regions of program text that are able to access the member.</span></span> <span data-ttu-id="bc420-443">Olası beş erişilebilirlik biçimi vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-443">There are five possible forms of accessibility.</span></span> <span data-ttu-id="bc420-444">Bunlar aşağıdaki tabloda özetlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-444">These are summarized in the following table.</span></span>


| <span data-ttu-id="bc420-445">__Erişilebilirlik__</span><span class="sxs-lookup"><span data-stu-id="bc420-445">__Accessibility__</span></span>    | <span data-ttu-id="bc420-446">__Anlamına__</span><span class="sxs-lookup"><span data-stu-id="bc420-446">__Meaning__</span></span> |
|----------------------|-----------------|
| `public`             | <span data-ttu-id="bc420-447">Erişim sınırlı değil</span><span class="sxs-lookup"><span data-stu-id="bc420-447">Access not limited</span></span> |
| `protected`          | <span data-ttu-id="bc420-448">Bu sınıftan türetilmiş bu sınıfla veya sınıflarla sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="bc420-448">Access limited to this class or classes derived from this class</span></span> |
| `internal`           | <span data-ttu-id="bc420-449">Bu programla sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="bc420-449">Access limited to this program</span></span> |
| `protected internal` | <span data-ttu-id="bc420-450">Bu sınıftan türetilmiş bu program veya sınıflarla sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="bc420-450">Access limited to this program or classes derived from this class</span></span> |
| `private`            | <span data-ttu-id="bc420-451">Bu sınıfla sınırlı erişim</span><span class="sxs-lookup"><span data-stu-id="bc420-451">Access limited to this class</span></span> |

### <a name="type-parameters"></a><span data-ttu-id="bc420-452">Tür parametreleri</span><span class="sxs-lookup"><span data-stu-id="bc420-452">Type parameters</span></span>

<span data-ttu-id="bc420-453">Sınıf tanımı, tür parametre adlarının bir listesini kapsayan açılı ayraçları olan sınıf adını izleyerek bir tür parametreleri kümesi belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-453">A class definition may specify a set of type parameters by following the class name with angle brackets enclosing a list of type parameter names.</span></span> <span data-ttu-id="bc420-454">Tür parametreleri, sınıfının üyelerini tanımlamak için sınıf bildirimlerinin gövdesinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-454">The type parameters can the be used in the body of the class declarations to define the members of the class.</span></span> <span data-ttu-id="bc420-455">Aşağıdaki örnekte, öğesinin `Pair` `TFirst` tür parametreleri ve `TSecond`:</span><span class="sxs-lookup"><span data-stu-id="bc420-455">In the following example, the type parameters of `Pair` are `TFirst` and `TSecond`:</span></span>

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
<span data-ttu-id="bc420-456">Tür parametrelerini almak için belirtilen bir sınıf türüne genel sınıf türü denir.</span><span class="sxs-lookup"><span data-stu-id="bc420-456">A class type that is declared to take type parameters is called a generic class type.</span></span> <span data-ttu-id="bc420-457">Yapı, arabirim ve temsilci türleri de genel olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-457">Struct, interface and delegate types can also be generic.</span></span>

<span data-ttu-id="bc420-458">Genel sınıf kullanıldığında, tür parametrelerinin her biri için tür bağımsız değişkenlerinin sağlanması gerekir:</span><span class="sxs-lookup"><span data-stu-id="bc420-458">When the generic class is used, type arguments must be provided for each of the type parameters:</span></span>

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst is int
string s = pair.Second; // TSecond is string
```
<span data-ttu-id="bc420-459">Yukarıda olduğu gibi `Pair<int,string>` , tür bağımsız değişkenlerine sahip genel bir tür, oluşturulmuş bir tür olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-459">A generic type with type arguments provided, like `Pair<int,string>` above, is called a constructed type.</span></span>

### <a name="base-classes"></a><span data-ttu-id="bc420-460">Temel sınıflar</span><span class="sxs-lookup"><span data-stu-id="bc420-460">Base classes</span></span>

<span data-ttu-id="bc420-461">Sınıf bildirimi, sınıf adı ve tür parametreleri iki nokta ve temel sınıfın adı ile birlikte bir temel sınıf belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-461">A class declaration may specify a base class by following the class name and type parameters with a colon and the name of the base class.</span></span> <span data-ttu-id="bc420-462">Temel sınıf belirtiminin atlanması, türden `object`türetmeye benzer.</span><span class="sxs-lookup"><span data-stu-id="bc420-462">Omitting a base class specification is the same as deriving from type `object`.</span></span> <span data-ttu-id="bc420-463">`Point3D` Aşağıdaki örnekte, öğesinin `Point`temel sınıfı ve öğesinin `Point` `object`temel sınıfı:</span><span class="sxs-lookup"><span data-stu-id="bc420-463">In the following example, the base class of `Point3D` is `Point`, and the base class of `Point` is `object`:</span></span>

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
<span data-ttu-id="bc420-464">Bir sınıf, temel sınıfının üyelerini devralır.</span><span class="sxs-lookup"><span data-stu-id="bc420-464">A class inherits the members of its base class.</span></span> <span data-ttu-id="bc420-465">Devralma, bir sınıfın örnek ve statik oluşturucular ve temel sınıfın yıkıcıları dışında, temel sınıfının tüm üyelerini örtük olarak içerdiği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="bc420-465">Inheritance means that a class implicitly contains all members of its base class, except for the instance and static constructors, and the destructors of the base class.</span></span> <span data-ttu-id="bc420-466">Türetilmiş bir sınıf, devralananlara yeni üyeler ekleyebilir, ancak devralınmış bir üyenin tanımını kaldıramaz.</span><span class="sxs-lookup"><span data-stu-id="bc420-466">A derived class can add new members to those it inherits, but it cannot remove the definition of an inherited member.</span></span> <span data-ttu-id="bc420-467">Önceki örnekte `Point3D` , `Point3D` `x` `z`ve alanlarını`y` öğesinden`Point`devralır ve her örnek üç alan`y`içerir,, ve. `x`</span><span class="sxs-lookup"><span data-stu-id="bc420-467">In the previous example, `Point3D` inherits the `x` and `y` fields from `Point`, and every `Point3D` instance contains three fields, `x`, `y`, and `z`.</span></span>

<span data-ttu-id="bc420-468">Bir sınıf türünden, temel sınıf türlerinden herhangi birine örtük bir dönüştürme vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-468">An implicit conversion exists from a class type to any of its base class types.</span></span> <span data-ttu-id="bc420-469">Bu nedenle, bir sınıf türünün değişkeni bu sınıfın bir örneğine veya türetilmiş herhangi bir sınıfın örneğine başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-469">Therefore, a variable of a class type can reference an instance of that class or an instance of any derived class.</span></span> <span data-ttu-id="bc420-470">Örneğin, önceki sınıf bildirimleri verildiğinde, türünde `Point` bir değişken bir `Point` veya a `Point3D`başvurabilir:</span><span class="sxs-lookup"><span data-stu-id="bc420-470">For example, given the previous class declarations, a variable of type `Point` can reference either a `Point` or a `Point3D`:</span></span>

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### <a name="fields"></a><span data-ttu-id="bc420-471">Alanlar</span><span class="sxs-lookup"><span data-stu-id="bc420-471">Fields</span></span>

<span data-ttu-id="bc420-472">Alan, bir sınıf ile veya bir sınıf örneğiyle ilişkili bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="bc420-472">A field is a variable that is associated with a class or with an instance of a class.</span></span>

<span data-ttu-id="bc420-473">`static` Değiştiriciyle belirtilen bir alan ***statik bir alan***tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-473">A field declared with the `static` modifier defines a ***static field***.</span></span> <span data-ttu-id="bc420-474">Statik alan tam olarak bir depolama konumunu tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-474">A static field identifies exactly one storage location.</span></span> <span data-ttu-id="bc420-475">Bir sınıfın kaç örneğinin oluşturulduğuna bakılmaksızın, bir statik alanın yalnızca bir kopyası vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-475">No matter how many instances of a class are created, there is only ever one copy of a static field.</span></span>

<span data-ttu-id="bc420-476">`static` Değiştirici olmadan belirtilen bir alan bir ***örnek alanını***tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-476">A field declared without the `static` modifier defines an ***instance field***.</span></span> <span data-ttu-id="bc420-477">Bir sınıfın her örneği, bu sınıfın tüm örnek alanlarının ayrı bir kopyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-477">Every instance of a class contains a separate copy of all the instance fields of that class.</span></span>

<span data-ttu-id="bc420-478">Aşağıdaki örnekte `Color` , sınıfının her örneği `r`, `g`, ve `b` `Black`örnek `Red`alanlarının ayrı bir kopyasına sahiptir ancak `White` ,,,,,,,,,,,,,,,,,,,`Green` ve`Blue` statik alanları:</span><span class="sxs-lookup"><span data-stu-id="bc420-478">In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:</span></span>

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
<span data-ttu-id="bc420-479">Önceki örnekte gösterildiği gibi, ***salt okuma alanları*** bir `readonly` değiştirici ile bildirilebilecek.</span><span class="sxs-lookup"><span data-stu-id="bc420-479">As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier.</span></span> <span data-ttu-id="bc420-480">Bir `readonly` alana atama yalnızca alanın bildiriminin veya aynı sınıftaki bir oluşturucunun parçası olarak gerçekleşebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-480">Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.</span></span>

### <a name="methods"></a><span data-ttu-id="bc420-481">Yöntemler</span><span class="sxs-lookup"><span data-stu-id="bc420-481">Methods</span></span>

<span data-ttu-id="bc420-482">Bir ***Yöntem*** , bir nesne veya sınıf tarafından gerçekleştirilebilecek bir hesaplama veya eylem uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-482">A ***method*** is a member that implements a computation or action that can be performed by an object or class.</span></span> <span data-ttu-id="bc420-483">***Statik yöntemlere*** sınıfı aracılığıyla erişilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-483">***Static methods*** are accessed through the class.</span></span> <span data-ttu-id="bc420-484">***Örnek yöntemlerine*** , sınıfının örnekleri aracılığıyla erişilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-484">***Instance methods*** are accessed through instances of the class.</span></span>

<span data-ttu-id="bc420-485">Metotlarda (muhtemelen boş) bir ***parametre***listesi vardır. Bu, yönteme geçirilen değerleri ya da değişken başvurularını temsil eder ve bir ***dönüş türü***, hesaplanan ve yöntemi tarafından döndürülen değer türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="bc420-485">Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method.</span></span> <span data-ttu-id="bc420-486">Bir yöntemin dönüş türü `void` bir değer döndürmezse.</span><span class="sxs-lookup"><span data-stu-id="bc420-486">A method's return type is `void` if it does not return a value.</span></span>

<span data-ttu-id="bc420-487">Türler gibi yöntemler de bir tür parametreleri kümesine sahip olabilir, bu da yöntem çağrıldığında tür bağımsız değişkenlerinin belirtilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc420-487">Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called.</span></span> <span data-ttu-id="bc420-488">Türlerin aksine, tür bağımsız değişkenleri genellikle yöntem çağrısının bağımsız değişkenlerinden çıkarsanamıyor ve açıkça verilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-488">Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.</span></span>

<span data-ttu-id="bc420-489">Yöntemin ***imzası*** , yöntemin bildirildiği sınıfta benzersiz olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-489">The ***signature*** of a method must be unique in the class in which the method is declared.</span></span> <span data-ttu-id="bc420-490">Bir yöntemin imzası yöntemin adından, tür parametrelerinin sayısına ve parametrelerinin sayısına, değiştiricilerine ve türlerine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="bc420-490">The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters.</span></span> <span data-ttu-id="bc420-491">Bir yöntemin imzası, dönüş türünü içermez.</span><span class="sxs-lookup"><span data-stu-id="bc420-491">The signature of a method does not include the return type.</span></span>

#### <a name="parameters"></a><span data-ttu-id="bc420-492">Parametreler</span><span class="sxs-lookup"><span data-stu-id="bc420-492">Parameters</span></span>

<span data-ttu-id="bc420-493">Parametreler, değerlere veya değişken başvurularını yöntemlere geçirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-493">Parameters are used to pass values or variable references to methods.</span></span> <span data-ttu-id="bc420-494">Bir yöntemin parametreleri, yöntemi çağrıldığında belirtilen ***bağımsız değişkenlerden*** gerçek değerlerini alır.</span><span class="sxs-lookup"><span data-stu-id="bc420-494">The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked.</span></span> <span data-ttu-id="bc420-495">Dört tür parametre vardır: değer parametreleri, başvuru parametreleri, çıkış parametreleri ve parametre dizileri.</span><span class="sxs-lookup"><span data-stu-id="bc420-495">There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.</span></span>

<span data-ttu-id="bc420-496">Giriş parametresi geçirme için bir ***değer parametresi*** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-496">A ***value parameter*** is used for input parameter passing.</span></span> <span data-ttu-id="bc420-497">Değer parametresi, parametresi için geçirilen bağımsız değişkenden ilk değerini alan yerel bir değişkene karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bc420-497">A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter.</span></span> <span data-ttu-id="bc420-498">Değer parametresindeki değişiklikler, parametresi için geçirilen bağımsız değişkeni etkilemez.</span><span class="sxs-lookup"><span data-stu-id="bc420-498">Modifications to a value parameter do not affect the argument that was passed for the parameter.</span></span>

<span data-ttu-id="bc420-499">Değer parametreleri, ilgili bağımsız değişkenlerin atlanabilmesi için varsayılan bir değer belirtilerek isteğe bağlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-499">Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.</span></span>

<span data-ttu-id="bc420-500">Hem giriş hem de çıkış parametresi geçirme için ***başvuru parametresi*** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-500">A ***reference parameter*** is used for both input and output parameter passing.</span></span> <span data-ttu-id="bc420-501">Başvuru parametresi için geçirilen bağımsız değişken bir değişken olmalıdır ve yöntemin yürütülmesi sırasında başvuru parametresi, bağımsız değişken değişkeniyle aynı depolama konumunu temsil eder.</span><span class="sxs-lookup"><span data-stu-id="bc420-501">The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable.</span></span> <span data-ttu-id="bc420-502">Bir başvuru parametresi `ref` değiştiriciyle birlikte bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-502">A reference parameter is declared with the `ref` modifier.</span></span> <span data-ttu-id="bc420-503">Aşağıdaki örnek `ref` parametrelerin kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-503">The following example shows the use of `ref` parameters.</span></span>

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
<span data-ttu-id="bc420-504">Çıkış parametresi geçirme için ***çıkış parametresi*** kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-504">An ***output parameter*** is used for output parameter passing.</span></span> <span data-ttu-id="bc420-505">Bir çıktı parametresi, çağıran tarafından belirtilen bağımsız değişkenin ilk değeri önemli olmayan bir başvuru parametresine benzerdir.</span><span class="sxs-lookup"><span data-stu-id="bc420-505">An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant.</span></span> <span data-ttu-id="bc420-506">Bir çıkış parametresi `out` değiştiriciyle birlikte bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-506">An output parameter is declared with the `out` modifier.</span></span> <span data-ttu-id="bc420-507">Aşağıdaki örnek `out` parametrelerin kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-507">The following example shows the use of `out` parameters.</span></span>

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
<span data-ttu-id="bc420-508">Bir ***parametre dizisi*** , bir metoda değişken sayıda bağımsız değişken geçirilmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="bc420-508">A ***parameter array*** permits a variable number of arguments to be passed to a method.</span></span> <span data-ttu-id="bc420-509">Bir parametre dizisi `params` değiştiriciyle birlikte bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-509">A parameter array is declared with the `params` modifier.</span></span> <span data-ttu-id="bc420-510">Bir yöntemin yalnızca son parametresi bir parametre dizisi olabilir ve bir parametre dizisinin türü tek boyutlu bir dizi türü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-510">Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type.</span></span> <span data-ttu-id="bc420-511">Sınıfının ve `WriteLine` yöntemleri `Write` , parametre dizisi kullanımının iyi örnekleridir. `System.Console`</span><span class="sxs-lookup"><span data-stu-id="bc420-511">The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage.</span></span> <span data-ttu-id="bc420-512">Bunlar aşağıdaki gibi bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-512">They are declared as follows.</span></span>

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
<span data-ttu-id="bc420-513">Bir parametre dizisi kullanan bir yöntem içinde, parametre dizisi tam olarak bir dizi türünün normal parametresine benzer şekilde davranır.</span><span class="sxs-lookup"><span data-stu-id="bc420-513">Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type.</span></span> <span data-ttu-id="bc420-514">Ancak, bir parametre dizisi olan bir yöntem çağrısında, parametre dizisi türünün tek bir bağımsız değişkenini veya parametre dizisinin öğe türünün herhangi bir sayıda bağımsız değişkenini geçirmek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="bc420-514">However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array.</span></span> <span data-ttu-id="bc420-515">İkinci durumda, bir dizi örneği otomatik olarak oluşturulur ve verilen bağımsız değişkenlerle başlatılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-515">In the latter case, an array instance is automatically created and initialized with the given arguments.</span></span> <span data-ttu-id="bc420-516">Bu örnek</span><span class="sxs-lookup"><span data-stu-id="bc420-516">This example</span></span>

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
<span data-ttu-id="bc420-517">, aşağıdaki yazma ile eşdeğerdir.</span><span class="sxs-lookup"><span data-stu-id="bc420-517">is equivalent to writing the following.</span></span>

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### <a name="method-body-and-local-variables"></a><span data-ttu-id="bc420-518">Yöntem gövdesi ve yerel değişkenler</span><span class="sxs-lookup"><span data-stu-id="bc420-518">Method body and local variables</span></span>

<span data-ttu-id="bc420-519">Yöntemin gövdesi, yöntemi çağrıldığında yürütülecek deyimleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="bc420-519">A method's body specifies the statements to execute when the method is invoked.</span></span>

<span data-ttu-id="bc420-520">Yöntem gövdesi, yöntemi çağrısına özgü değişkenleri bildirebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-520">A method body can declare variables that are specific to the invocation of the method.</span></span> <span data-ttu-id="bc420-521">Bu tür değişkenlere ***yerel değişkenler***denir.</span><span class="sxs-lookup"><span data-stu-id="bc420-521">Such variables are called ***local variables***.</span></span> <span data-ttu-id="bc420-522">Yerel bir değişken bildirimi bir tür adı, değişken adı ve muhtemelen bir başlangıç değeri belirtir.</span><span class="sxs-lookup"><span data-stu-id="bc420-522">A local variable declaration specifies a type name, a variable name, and possibly an initial value.</span></span> <span data-ttu-id="bc420-523">Aşağıdaki örnek, başlangıç değeri sıfır olan `i` yerel bir değişken ve ilk değeri olmayan bir yerel değişken `j` bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-523">The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.</span></span>

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
<span data-ttu-id="bc420-524">C#değeri alınabilmesi için önce bir yerel değişkenin ***kesinlikle atanmasını*** gerektirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-524">C# requires a local variable to be ***definitely assigned*** before its value can be obtained.</span></span> <span data-ttu-id="bc420-525">Örneğin, önceki `i` bildiriminde bir başlangıç değeri yoksa, derleyici programın sonraki `i` kullanımları için bir hata bildirir çünkü `i` bu, programda bu noktalarda kesinlikle atanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-525">For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.</span></span>

<span data-ttu-id="bc420-526">Bir yöntem, çağrı `return` yapana denetim döndürmek için deyimlerini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-526">A method can use `return` statements to return control to its caller.</span></span> <span data-ttu-id="bc420-527">`void` Döndürülen`return` bir yöntemde deyimler bir ifade belirtemez.</span><span class="sxs-lookup"><span data-stu-id="bc420-527">In a method returning `void`, `return` statements cannot specify an expression.</span></span> <span data-ttu-id="bc420-528">Olmayan`void`deyimler döndüren bir yöntemde, `return` dönüş değerini hesaplayan bir ifade içermelidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-528">In a method returning non-`void`, `return` statements must include an expression that computes the return value.</span></span>

#### <a name="static-and-instance-methods"></a><span data-ttu-id="bc420-529">Statik ve örnek yöntemleri</span><span class="sxs-lookup"><span data-stu-id="bc420-529">Static and instance methods</span></span>

<span data-ttu-id="bc420-530">`static` Değiştirici ile belirtilen bir yöntem ***statik bir yöntemdir***.</span><span class="sxs-lookup"><span data-stu-id="bc420-530">A method declared with a `static` modifier is a ***static method***.</span></span> <span data-ttu-id="bc420-531">Statik bir yöntem belirli bir örnek üzerinde çalışmaz ve yalnızca statik üyelere doğrudan erişebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-531">A static method does not operate on a specific instance and can only directly access static members.</span></span>

<span data-ttu-id="bc420-532">`static` Değiştirici olmadan bildirildiği bir yöntem bir ***örnek yöntemidir***.</span><span class="sxs-lookup"><span data-stu-id="bc420-532">A method declared without a `static` modifier is an ***instance method***.</span></span> <span data-ttu-id="bc420-533">Örnek yöntemi, belirli bir örnek üzerinde çalışır ve hem statik hem de örnek üyelerine erişebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-533">An instance method operates on a specific instance and can access both static and instance members.</span></span> <span data-ttu-id="bc420-534">Örnek yönteminin çağrıldığı örnek, olarak `this`açıkça erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-534">The instance on which an instance method was invoked can be explicitly accessed as `this`.</span></span> <span data-ttu-id="bc420-535">Statik bir yöntemde başvurmak `this` için bir hatadır.</span><span class="sxs-lookup"><span data-stu-id="bc420-535">It is an error to refer to `this` in a static method.</span></span>

<span data-ttu-id="bc420-536">Aşağıdaki `Entity` sınıfta hem statik hem de örnek üyeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-536">The following `Entity` class has both static and instance members.</span></span>

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
<span data-ttu-id="bc420-537">Her `Entity` örnek bir seri numarası içerir (ve burada görünmeyen bazı diğer bilgileri kabul etmez).</span><span class="sxs-lookup"><span data-stu-id="bc420-537">Each `Entity` instance contains a serial number (and presumably some other information that is not shown here).</span></span> <span data-ttu-id="bc420-538">`Entity` Oluşturucu (bir örnek yöntemi gibi) yeni örneği bir sonraki kullanılabilir seri numarasıyla başlatır.</span><span class="sxs-lookup"><span data-stu-id="bc420-538">The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number.</span></span> <span data-ttu-id="bc420-539">Oluşturucu bir örnek üyesi olduğundan, hem `serialNo` örnek alanına `nextSerialNo` hem de statik alana erişim izni verilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-539">Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.</span></span>

<span data-ttu-id="bc420-540">`GetNextSerialNo` Ve `serialNo` statik yöntemler statikalanaerişebilir,ancakörnekalanınadoğrudanerişmesiiçinbirhataolabilir.`nextSerialNo` `SetNextSerialNo`</span><span class="sxs-lookup"><span data-stu-id="bc420-540">The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.</span></span>

<span data-ttu-id="bc420-541">Aşağıdaki örnek `Entity` sınıfının kullanımını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-541">The following example shows the use of the `Entity` class.</span></span>

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
<span data-ttu-id="bc420-542">Sınıfının örneklerinde `GetSerialNo` örnek `SetNextSerialNo` yöntemi `GetNextSerialNo` çağrıldığında, ve statik yöntemlerin sınıfında çağrılabileceğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="bc420-542">Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.</span></span>

#### <a name="virtual-override-and-abstract-methods"></a><span data-ttu-id="bc420-543">Sanal, geçersiz kılma ve soyut yöntemler</span><span class="sxs-lookup"><span data-stu-id="bc420-543">Virtual, override, and abstract methods</span></span>

<span data-ttu-id="bc420-544">Bir örnek yöntemi bildirimi bir `virtual` değiştirici içerdiğinde, yöntemi bir ***sanal yöntem***olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-544">When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***.</span></span> <span data-ttu-id="bc420-545">Değiştirici yoksa, yöntemi ***sanal olmayan bir yöntem***olarak kabul edilir. `virtual`</span><span class="sxs-lookup"><span data-stu-id="bc420-545">When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.</span></span>

<span data-ttu-id="bc420-546">Bir sanal yöntem çağrıldığında, çağrının gerçekleştiği örneğin ***çalışma zamanı türü*** , çağrılacak gerçek Yöntem uygulamasını belirler.</span><span class="sxs-lookup"><span data-stu-id="bc420-546">When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke.</span></span> <span data-ttu-id="bc420-547">Sanal olmayan bir yöntem çağrısında, örneğin ***derleme zamanı türü*** belirleme faktörü olur.</span><span class="sxs-lookup"><span data-stu-id="bc420-547">In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.</span></span>

<span data-ttu-id="bc420-548">Bir sanal yöntem, türetilmiş bir sınıfta ***geçersiz kılınabilir*** .</span><span class="sxs-lookup"><span data-stu-id="bc420-548">A virtual method can be ***overridden*** in a derived class.</span></span> <span data-ttu-id="bc420-549">Bir örnek yöntemi bildirimi bir `override` değiştirici içerdiğinde, yöntemi aynı imzaya sahip devralınmış bir sanal yöntemi geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="bc420-549">When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature.</span></span> <span data-ttu-id="bc420-550">Sanal bir yöntem bildiriminde yeni bir yöntem tanıtıldığı halde, bir geçersiz kılma yöntemi bildirimi, bu yöntemin yeni bir uygulamasını sağlayarak, var olan bir devralınmış sanal yöntemi uzmanlık eder.</span><span class="sxs-lookup"><span data-stu-id="bc420-550">Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.</span></span>

<span data-ttu-id="bc420-551">***Soyut*** bir yöntem, uygulama içermeyen bir sanal yöntemdir.</span><span class="sxs-lookup"><span data-stu-id="bc420-551">An ***abstract*** method is a virtual method with no implementation.</span></span> <span data-ttu-id="bc420-552">Soyut bir yöntem `abstract` değiştiriciyle birlikte bildirilmiştir ve yalnızca da belirtilen `abstract`bir sınıfta izin verilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-552">An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`.</span></span> <span data-ttu-id="bc420-553">Soyut olmayan her türetilmiş sınıfta bir soyut yöntem geçersiz kılınmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-553">An abstract method must be overridden in every non-abstract derived class.</span></span>

<span data-ttu-id="bc420-554">Aşağıdaki örnek, bir ifade ağaç düğümünü temsil `Expression`eden bir soyut sınıfı ve sabitler, değişken için ifade ağacı düğümleri uygulayan `Constant`üç `VariableReference`türetilmiş sınıfı `Operation`,, ve ve ' ı tanımlar. başvurular ve aritmetik işlemler.</span><span class="sxs-lookup"><span data-stu-id="bc420-554">The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations.</span></span> <span data-ttu-id="bc420-555">(Bu şuna benzerdir, ancak [ifade ağacı türlerinde](types.md#expression-tree-types)tanıtılan ifade ağacı türleriyle karıştırılmamalıdır).</span><span class="sxs-lookup"><span data-stu-id="bc420-555">(This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).</span></span>

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
<span data-ttu-id="bc420-556">Önceki dört sınıf aritmetik ifadeleri modellemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-556">The previous four classes can be used to model arithmetic expressions.</span></span> <span data-ttu-id="bc420-557">Örneğin, bu sınıfların örneklerini kullanarak, ifadesi `x + 3` aşağıdaki gibi gösterilebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-557">For example, using instances of these classes, the expression `x + 3` can be represented as follows.</span></span>

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
<span data-ttu-id="bc420-558">`double` Bir `Evaluate` Örneğinyöntemi,verilenifadeyideğerlendirmekvebirdeğerüretmekiçinçağrılır.`Expression`</span><span class="sxs-lookup"><span data-stu-id="bc420-558">The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value.</span></span> <span data-ttu-id="bc420-559">Yöntemi, değişken adlarını (girdilerin anahtarları `Hashtable` olarak) ve değerlerini (girdilerin değerleri olarak) içeren bir bağımsız değişken olarak alır.</span><span class="sxs-lookup"><span data-stu-id="bc420-559">The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries).</span></span> <span data-ttu-id="bc420-560">`Evaluate` Yöntemi, bir sanal soyut yöntemidir, yani soyut olmayan türetilmiş sınıfların gerçek bir uygulama sağlamak için onu geçersiz kılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc420-560">The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.</span></span>

<span data-ttu-id="bc420-561">' Nin uygulanması, `Evaluate` yalnızca saklı sabiti döndürür. `Constant`</span><span class="sxs-lookup"><span data-stu-id="bc420-561">A `Constant`'s implementation of `Evaluate` simply returns the stored constant.</span></span> <span data-ttu-id="bc420-562">Bir `VariableReference`uygulama, Hashtable 'daki değişken adını arar ve elde edilen değeri döndürür.</span><span class="sxs-lookup"><span data-stu-id="bc420-562">A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value.</span></span> <span data-ttu-id="bc420-563">Bir `Operation`uygulama ilk olarak sol ve sağ işlenenleri değerlendirir ( `Evaluate` yöntemlerini özyinelemeli olarak çağırarak) ve ardından verilen aritmetik işlemi gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-563">An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.</span></span>

<span data-ttu-id="bc420-564">Aşağıdaki program, `Expression` `x` ve `x * (y + 2)` farklı`y`değerleri için ifadeyi değerlendirmek için sınıflarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-564">The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.</span></span>

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

#### <a name="method-overloading"></a><span data-ttu-id="bc420-565">Yöntem aşırı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="bc420-565">Method overloading</span></span>

<span data-ttu-id="bc420-566">Yöntem ***aşırı yüklemesi*** , aynı sınıftaki birden çok metodun benzersiz imzalara sahip oldukları sürece aynı ada sahip olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-566">Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures.</span></span> <span data-ttu-id="bc420-567">Aşırı yüklenmiş bir yöntemin çağrılması derlenirken, derleyici çağrılacak özel yöntemi belirlemekte ***aşırı yükleme çözümü*** kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-567">When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke.</span></span> <span data-ttu-id="bc420-568">Aşırı yükleme çözümlemesi, bağımsız değişkenlerle en iyi eşleşen bir yöntemi bulur veya tek bir en iyi eşleşme bulunamazsa hata bildiriyor.</span><span class="sxs-lookup"><span data-stu-id="bc420-568">Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found.</span></span> <span data-ttu-id="bc420-569">Aşağıdaki örnekte, etkin olan aşırı yükleme çözümü gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bc420-569">The following example shows overload resolution in effect.</span></span> <span data-ttu-id="bc420-570">`Main` Yöntemi içindeki her çağrının yorumu aslında hangi yöntemin çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-570">The comment for each invocation in the `Main` method shows which method is actually invoked.</span></span>

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
<span data-ttu-id="bc420-571">Örnekte gösterildiği gibi belirli bir yöntem her zaman bağımsız değişkenleri tam parametre türlerine açıkça atayarak ve/veya açıkça tür bağımsız değişkenleri sunarak seçilebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-571">As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.</span></span>

### <a name="other-function-members"></a><span data-ttu-id="bc420-572">Diğer işlev üyeleri</span><span class="sxs-lookup"><span data-stu-id="bc420-572">Other function members</span></span>

<span data-ttu-id="bc420-573">Yürütülebilir kod içeren Üyeler topluca bir sınıfın ***işlev üyeleri*** olarak bilinir.</span><span class="sxs-lookup"><span data-stu-id="bc420-573">Members that contain executable code are collectively known as the ***function members*** of a class.</span></span> <span data-ttu-id="bc420-574">Yukarıdaki bölümde, işlev üyelerinin birincil türü olan yöntemler açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="bc420-574">The preceding section describes methods, which are the primary kind of function members.</span></span> <span data-ttu-id="bc420-575">Bu bölümde, tarafından C#desteklenen diğer işlev üyesi türleri açıklanmaktadır: oluşturucular, özellikler, Dizin oluşturucular, olaylar, işleçler ve Yıkıcılar.</span><span class="sxs-lookup"><span data-stu-id="bc420-575">This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.</span></span>

<span data-ttu-id="bc420-576">Aşağıdaki kod, bir nesne growable listesini uygulayan `List<T>`adlı bir genel sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-576">The following code shows a generic class called `List<T>`, which implements a growable list of objects.</span></span> <span data-ttu-id="bc420-577">Sınıfı, en yaygın işlev üyesi türlerine birkaç örnek içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-577">The class contains several examples of the most common kinds of function members.</span></span>


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

#### <a name="constructors"></a><span data-ttu-id="bc420-578">Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="bc420-578">Constructors</span></span>

<span data-ttu-id="bc420-579">C#hem örnek hem de statik oluşturucuları destekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-579">C# supports both instance and static constructors.</span></span> <span data-ttu-id="bc420-580">***Örnek Oluşturucu*** , bir sınıfın örneğini başlatmak için gereken eylemleri uygulayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="bc420-580">An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class.</span></span> <span data-ttu-id="bc420-581">***Statik Oluşturucu*** , ilk yüklendiği zaman bir sınıfın kendisini başlatmak için gereken eylemleri uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-581">A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.</span></span>

<span data-ttu-id="bc420-582">Bir Oluşturucu, dönüş türü olmayan bir yöntem ve kapsayan sınıfla aynı adı ile birlikte bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-582">A constructor is declared like a method with no return type and the same name as the containing class.</span></span> <span data-ttu-id="bc420-583">Bir Oluşturucu bildiriminde `static` bir değiştirici varsa, bir statik oluşturucu bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-583">If a constructor declaration includes a `static` modifier, it declares a static constructor.</span></span> <span data-ttu-id="bc420-584">Aksi takdirde, bir örnek Oluşturucu bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-584">Otherwise, it declares an instance constructor.</span></span>

<span data-ttu-id="bc420-585">Örnek oluşturucular aşırı yüklenebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-585">Instance constructors can be overloaded.</span></span> <span data-ttu-id="bc420-586">Örneğin, `List<T>` sınıfı, biri `int` parametresi olmayan iki örnek Oluşturucu bildirir ve bir parametre alır.</span><span class="sxs-lookup"><span data-stu-id="bc420-586">For example, the `List<T>` class declares two instance constructors, one with no parameters and one that takes an `int` parameter.</span></span> <span data-ttu-id="bc420-587">Örnek oluşturucular `new` işleci kullanılarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-587">Instance constructors are invoked using the `new` operator.</span></span> <span data-ttu-id="bc420-588">Aşağıdaki deyimler, `List<string>` `List` sınıfının oluşturucularının her birini kullanarak iki örnek ayırır.</span><span class="sxs-lookup"><span data-stu-id="bc420-588">The following statements allocate two `List<string>` instances using each of the constructors of the `List` class.</span></span>

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
<span data-ttu-id="bc420-589">Diğer üyelerin aksine, örnek oluşturucular devralınmaz ve bir sınıfın sınıfta tanımlananlardan farklı örnek oluşturucuları yoktur.</span><span class="sxs-lookup"><span data-stu-id="bc420-589">Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class.</span></span> <span data-ttu-id="bc420-590">Bir sınıf için örnek Oluşturucu sağlanmazsa, parametresi olmayan boş bir değer otomatik olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-590">If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.</span></span>

#### <a name="properties"></a><span data-ttu-id="bc420-591">Özellikler</span><span class="sxs-lookup"><span data-stu-id="bc420-591">Properties</span></span>

<span data-ttu-id="bc420-592">***Özellikler*** , alanlar için doğal bir uzantıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-592">***Properties*** are a natural extension of fields.</span></span> <span data-ttu-id="bc420-593">Her ikisi de ilişkili türlerin bulunduğu isimlerdir ve alanlara ve özelliklere erişim için sözdizimi aynıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-593">Both are named members with associated types, and the syntax for accessing fields and properties is the same.</span></span> <span data-ttu-id="bc420-594">Ancak, alanların aksine, Özellikler depolama konumlarını göstermiyor.</span><span class="sxs-lookup"><span data-stu-id="bc420-594">However, unlike fields, properties do not denote storage locations.</span></span> <span data-ttu-id="bc420-595">Bunun yerine, özellikler, değerleri okunmak veya yazıldığında yürütülecek deyimleri belirten ***erişimcileri*** vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-595">Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.</span></span>

<span data-ttu-id="bc420-596">Bir `get` özellik, bildirim bir erişimci ile sona erene ve/ `set` veya sınırlayıcılar `{` arasında `}` yazılmış bir erişimciyi ve noktalı virgül ile sona ermek yerine bir alan olarak belirtilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-596">A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon.</span></span> <span data-ttu-id="bc420-597">Hem `get` `set` erişimci `get`hem de erişimciyesahipbirözellikokuma-yazmaözelliğidir,yalnızcabirerişimcisiolanbirözelliksaltokunurdurveyalnızcaerişimcisiolanbirözellik`set` ***salt yazılır özelliği***.</span><span class="sxs-lookup"><span data-stu-id="bc420-597">A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.</span></span>

<span data-ttu-id="bc420-598">`get` Erişimci, özellik türünün dönüş değeri olan parametresiz bir yönteme karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="bc420-598">A `get` accessor corresponds to a parameterless method with a return value of the property type.</span></span> <span data-ttu-id="bc420-599">Atama hedefi dışında, bir ifadede bir özelliğe başvurulduğunda, `get` özelliğin erişimcisi özelliğin değerini hesaplamak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-599">Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.</span></span>

<span data-ttu-id="bc420-600">Erişimci, adlandırılmış `value` ve dönüş türü olmayan tek parametreli bir yönteme karşılık gelir. `set`</span><span class="sxs-lookup"><span data-stu-id="bc420-600">A `set` accessor corresponds to a method with a single parameter named `value` and no return type.</span></span> <span data-ttu-id="bc420-601">Bir özelliğe bir atamanın hedefi veya işleneni `++` `--`olarak başvurulduğunda, `set` erişimci yeni değeri sağlayan bir bağımsız değişkenle çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-601">When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.</span></span>

<span data-ttu-id="bc420-602">Sınıfı iki özellik bildirir `Capacity`ve sırasıyla salt okunurdur ve okuma-yazma olur. `Count` `List<T>`</span><span class="sxs-lookup"><span data-stu-id="bc420-602">The `List<T>` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively.</span></span> <span data-ttu-id="bc420-603">Aşağıda bu özelliklerin kullanılmasına bir örnek verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-603">The following is an example of use of these properties.</span></span>

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
<span data-ttu-id="bc420-604">Alanlar ve yöntemlere benzer şekilde hem C# örnek özelliklerini hem de statik özellikleri destekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-604">Similar to fields and methods, C# supports both instance properties and static properties.</span></span> <span data-ttu-id="bc420-605">Statik özellikler `static` değiştiriciyle, örnek özellikleri ise bu olmadan tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-605">Static properties are declared with the `static` modifier, and instance properties are declared without it.</span></span>

<span data-ttu-id="bc420-606">Bir özelliğin erişimcisi sanal olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-606">The accessor(s) of a property can be virtual.</span></span> <span data-ttu-id="bc420-607">Bir özellik bildirimi `virtual`, `abstract`, veya `override` değiştiricisini içerdiğinde, özelliğin erişimcilerle geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="bc420-607">When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.</span></span>

#### <a name="indexers"></a><span data-ttu-id="bc420-608">Dizin Oluşturucular</span><span class="sxs-lookup"><span data-stu-id="bc420-608">Indexers</span></span>

<span data-ttu-id="bc420-609">***Dizin Oluşturucu*** , nesnelerin diziyle aynı şekilde dizinlenmesini sağlayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="bc420-609">An ***indexer*** is a member that enables objects to be indexed in the same way as an array.</span></span> <span data-ttu-id="bc420-610">Bir Dizin Oluşturucu, üyenin `this` adının ardından sınırlayıcılar `[` ve `]`arasında yazılmış bir parametre listesi gelmesi dışında bir özellik gibi bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-610">An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`.</span></span> <span data-ttu-id="bc420-611">Parametreler, dizin oluşturucunun erişimcisinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-611">The parameters are available in the accessor(s) of the indexer.</span></span> <span data-ttu-id="bc420-612">Özelliklere benzer şekilde, Dizin oluşturucular okunabilir-yazılır, salt okunurdur ve salt yazılır olabilir ve bir dizin oluşturucunun erişimcisi sanal olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-612">Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.</span></span>

<span data-ttu-id="bc420-613">Sınıfı `List` , bir `int` parametresi alan tek bir okuma-yazma Dizin Oluşturucu bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-613">The `List` class declares a single read-write indexer that takes an `int` parameter.</span></span> <span data-ttu-id="bc420-614">Dizin Oluşturucu, `List` `int` örneklerin değerleriyle dizin oluşturmanızı mümkün kılar.</span><span class="sxs-lookup"><span data-stu-id="bc420-614">The indexer makes it possible to index `List` instances with `int` values.</span></span> <span data-ttu-id="bc420-615">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bc420-615">For example</span></span>

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
<span data-ttu-id="bc420-616">Dizin oluşturucular aşırı yüklenebilir, yani parametrelerinin sayısı veya türleri farklı olduğu sürece bir sınıfın birden çok dizin kümesini bildirebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="bc420-616">Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.</span></span>

#### <a name="events"></a><span data-ttu-id="bc420-617">Olaylar</span><span class="sxs-lookup"><span data-stu-id="bc420-617">Events</span></span>

<span data-ttu-id="bc420-618">Bir ***olay*** , bir sınıf veya nesnenin bildirimler sağlamasını sağlayan bir üyedir.</span><span class="sxs-lookup"><span data-stu-id="bc420-618">An ***event*** is a member that enables a class or object to provide notifications.</span></span> <span data-ttu-id="bc420-619">Bildirimin bir `event` anahtar sözcük içermesi ve türün bir temsilci türü olması dışında bir olay, bir alan gibi bildirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bc420-619">An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.</span></span>

<span data-ttu-id="bc420-620">Olay üyesini bildiren bir sınıf içinde, olay bir temsilci türünün alanı gibi davranır (olay soyut değildir ve erişimcileri bildirmez).</span><span class="sxs-lookup"><span data-stu-id="bc420-620">Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors).</span></span> <span data-ttu-id="bc420-621">Bu alan, olaya eklenmiş olan olay işleyicilerini temsil eden bir temsilciye bir başvuru depolar.</span><span class="sxs-lookup"><span data-stu-id="bc420-621">The field stores a reference to a delegate that represents the event handlers that have been added to the event.</span></span> <span data-ttu-id="bc420-622">Hiçbir olay tanıtıcısı yoksa, alanı olur `null`.</span><span class="sxs-lookup"><span data-stu-id="bc420-622">If no event handles are present, the field is `null`.</span></span>

<span data-ttu-id="bc420-623">Sınıfı, adlı `Changed`tek bir olay üyesini bildirir ve bu, listeye yeni bir öğe eklendiğini gösterir. `List<T>`</span><span class="sxs-lookup"><span data-stu-id="bc420-623">The `List<T>` class declares a single event member called `Changed`, which indicates that a new item has been added to the list.</span></span> <span data-ttu-id="bc420-624">Olay sanal yöntemi tarafından tetiklenir, bu, önce olayın `null` (hiçbir işleyicinin mevcut olmadığı anlamına gelir) olup olmadığını denetler. `OnChanged` `Changed`</span><span class="sxs-lookup"><span data-stu-id="bc420-624">The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present).</span></span> <span data-ttu-id="bc420-625">Bir olayı oluşturma kavramı, olayın gösterdiği temsilciyi çağırmak için tam olarak eşdeğerdir. bu nedenle, olayları yükseltmek için özel dil yapıları yoktur.</span><span class="sxs-lookup"><span data-stu-id="bc420-625">The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.</span></span>

<span data-ttu-id="bc420-626">İstemciler ***olay işleyicileri***aracılığıyla olaylara tepki verir.</span><span class="sxs-lookup"><span data-stu-id="bc420-626">Clients react to events through ***event handlers***.</span></span> <span data-ttu-id="bc420-627">Olay işleyicileri `+=` işleci kullanılarak eklenir ve `-=` işleci kullanılarak kaldırılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-627">Event handlers are attached using the `+=` operator and removed using the `-=` operator.</span></span> <span data-ttu-id="bc420-628">Aşağıdaki örnek, olayına bir `Changed` `List<string>`olay işleyicisi ekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-628">The following example attaches an event handler to the `Changed` event of a `List<string>`.</span></span>

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
<span data-ttu-id="bc420-629">Bir olayın temeldeki depolamanın denetiminin istendiği Gelişmiş senaryolarda, bir olay bildirimi açıkça bir özelliğin `add` `set` erişenine benzer bir şekilde sağlayabilir ve `remove` erişimciler sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-629">For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.</span></span>

#### <a name="operators"></a><span data-ttu-id="bc420-630">İşleçler</span><span class="sxs-lookup"><span data-stu-id="bc420-630">Operators</span></span>

<span data-ttu-id="bc420-631">***İşleci*** , bir sınıfın örneklerine belirli bir ifade işlecini uygulamanın anlamını tanımlayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-631">An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class.</span></span> <span data-ttu-id="bc420-632">Üç tür işleç tanımlanabilir: Birli İşleçler, ikili işleçler ve dönüştürme işleçleri.</span><span class="sxs-lookup"><span data-stu-id="bc420-632">Three kinds of operators can be defined: unary operators, binary operators, and conversion operators.</span></span> <span data-ttu-id="bc420-633">Tüm işleçler ve `public` `static`olarak bildirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-633">All operators must be declared as `public` and `static`.</span></span>

<span data-ttu-id="bc420-634">Sınıfı iki `List` işleç `operator==` bildirirvebunedenlebuişleçleriörneklereuygulayandeyimlereyenianlamıverir.`operator!=` `List<T>`</span><span class="sxs-lookup"><span data-stu-id="bc420-634">The `List<T>` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances.</span></span> <span data-ttu-id="bc420-635">Özellikle, işleçler, içerilen nesnelerin her birini `List<T>` `Equals` yöntemlerini kullanarak karşılaştıran iki örneğin eşitliğini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-635">Specifically, the operators define equality of two `List<T>` instances as comparing each of the contained objects using their `Equals` methods.</span></span> <span data-ttu-id="bc420-636">Aşağıdaki örnek, iki `==` `List<int>` örneği karşılaştırmak için işlecini kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-636">The following example uses the `==` operator to compare two `List<int>` instances.</span></span>

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

<span data-ttu-id="bc420-637">Bu iki `Console.WriteLine` liste `True` aynı sırada aynı değerleri taşıyan aynı sayıda nesne içerdiğinden ilk çıktılar.</span><span class="sxs-lookup"><span data-stu-id="bc420-637">The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order.</span></span> <span data-ttu-id="bc420-638">`List<T>` Tanımlı değil,`Console.WriteLine` ilkiçıktıyı`False` içeriyor ve`b` farklı örneklere`List<int>` başvuracaktır. `a` `operator==`</span><span class="sxs-lookup"><span data-stu-id="bc420-638">Had `List<T>` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>` instances.</span></span>

#### <a name="destructors"></a><span data-ttu-id="bc420-639">Yıkıcılar</span><span class="sxs-lookup"><span data-stu-id="bc420-639">Destructors</span></span>

<span data-ttu-id="bc420-640">***Yıkıcı*** , bir sınıf örneğinin yapısını kaldırma için gereken eylemleri uygulayan bir üyesidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-640">A ***destructor*** is a member that implements the actions required to destruct an instance of a class.</span></span> <span data-ttu-id="bc420-641">Yok ediciler parametrelere sahip olamaz, erişilebilirlik değiştiricilerine sahip olamaz ve açıkça çağrılamaz.</span><span class="sxs-lookup"><span data-stu-id="bc420-641">Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly.</span></span> <span data-ttu-id="bc420-642">Örnek için yıkıcısı çöp toplama sırasında otomatik olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-642">The destructor for an instance is invoked automatically during garbage collection.</span></span>

<span data-ttu-id="bc420-643">Çöp toplayıcısına, nesnelerin toplanması ve yıkıcıları çalıştırma konusunda karar verirken geniş bir enlem vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-643">The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors.</span></span> <span data-ttu-id="bc420-644">Özellikle, yıkıcı etkinleştirmeleri zamanlaması belirleyici değildir ve herhangi bir iş parçacığında yok ediciler çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-644">Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread.</span></span> <span data-ttu-id="bc420-645">Bu ve diğer nedenlerden dolayı sınıfların yalnızca başka hiçbir çözüm uygulanabilir olmadığında Yıkıcılar uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="bc420-645">For these and other reasons, classes should implement destructors only when no other solutions are feasible.</span></span>

<span data-ttu-id="bc420-646">İfade `using` , nesne yok etme için daha iyi bir yaklaşım sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-646">The `using` statement provides a better approach to object destruction.</span></span>

## <a name="structs"></a><span data-ttu-id="bc420-647">Yapılar</span><span class="sxs-lookup"><span data-stu-id="bc420-647">Structs</span></span>

<span data-ttu-id="bc420-648">Sınıflar gibi ***yapılar*** , veri üyeleri ve işlev üyeleri içerebilen veri yapılarıdır, ancak sınıfların aksine, yapılar değer türlerdir ve yığın ayırmayı gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="bc420-648">Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation.</span></span> <span data-ttu-id="bc420-649">Yapı türünün bir değişkeni yapının verisini doğrudan depolarken, sınıf türündeki bir değişken, dinamik olarak ayrılan bir nesneye bir başvuru depolar.</span><span class="sxs-lookup"><span data-stu-id="bc420-649">A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object.</span></span> <span data-ttu-id="bc420-650">Yapı türleri Kullanıcı tarafından belirtilen devralmayı desteklemez ve tüm yapı türleri örtülü olarak türünden `object`devralınır.</span><span class="sxs-lookup"><span data-stu-id="bc420-650">Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.</span></span>

<span data-ttu-id="bc420-651">Yapılar, özellikle değer semantiği olan küçük veri yapıları için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-651">Structs are particularly useful for small data structures that have value semantics.</span></span> <span data-ttu-id="bc420-652">Karmaşık sayılar, bir koordinat sistemindeki işaret veya bir sözlükte anahtar-değer çiftleri, yapı birimlerinin iyi örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="bc420-652">Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs.</span></span> <span data-ttu-id="bc420-653">Küçük veri yapıları için sınıflar yerine yapıların kullanımı, bir uygulamanın gerçekleştirdiği bellek ayırma sayısında büyük bir farklılık yapabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-653">The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs.</span></span> <span data-ttu-id="bc420-654">Örneğin, aşağıdaki program 100 noktadan oluşan bir dizi oluşturur ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="bc420-654">For example, the following program creates and initializes an array of 100 points.</span></span> <span data-ttu-id="bc420-655">Sınıf `Point` olarak uygulanan 101 ayrı nesneler, biri Array için ve her biri 100 öğeleri için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc420-655">With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.</span></span>

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
<span data-ttu-id="bc420-656">Alternatif olarak bir yapı oluşturma `Point` .</span><span class="sxs-lookup"><span data-stu-id="bc420-656">An alternative is to make `Point` a struct.</span></span>

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
<span data-ttu-id="bc420-657">Şimdi yalnızca bir nesne örneği oluşturulur — dizi için bir tane ve `Point` örnekler dizide satır içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-657">Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.</span></span>

<span data-ttu-id="bc420-658">Struct oluşturucuları `new` işleçle çağrılır, ancak bu belleğin ayrılmakta olduğunu göstermez.</span><span class="sxs-lookup"><span data-stu-id="bc420-658">Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated.</span></span> <span data-ttu-id="bc420-659">Bir nesne dinamik olarak tahsis etmek ve ona bir başvuru döndürmek yerine, bir struct Oluşturucusu yalnızca struct değerinin kendisini döndürür (genellikle yığındaki geçici bir konumda) ve bu değer, gereken şekilde kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-659">Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.</span></span>

<span data-ttu-id="bc420-660">Sınıflar ile, iki değişkenin aynı nesneye başvurması ve bu nedenle bir değişkende işlemler için diğer değişken tarafından başvurulan nesneyi etkilemesi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="bc420-660">With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable.</span></span> <span data-ttu-id="bc420-661">Yapılar ile, her birinin kendi verilerinin bir kopyasına sahip olduğu ve bir üzerindeki işlemlerin diğerini etkilemesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="bc420-661">With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other.</span></span> <span data-ttu-id="bc420-662">Örneğin, aşağıdaki kod parçası tarafından üretilen çıkış, bir sınıf veya yapı olmasına `Point` bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-662">For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.</span></span>

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
<span data-ttu-id="bc420-663">Bir sınıfınise, `a` çıkış olur `20` ve `b` aynı nesneye başvurur. `Point`</span><span class="sxs-lookup"><span data-stu-id="bc420-663">If `Point` is a class, the output is `20` because `a` and `b` reference the same object.</span></span> <span data-ttu-id="bc420-664">`10` `a` `a.x`Bir struct ise çıktı, öğesinin`b` atamanın değerin bir kopyasını oluşturması ve bu kopyanın sonraki atamasından etkilenmemelerdir. `Point`</span><span class="sxs-lookup"><span data-stu-id="bc420-664">If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.</span></span>

<span data-ttu-id="bc420-665">Önceki örnek, yapıların sınırlamalarını iki vurgulamaktadır.</span><span class="sxs-lookup"><span data-stu-id="bc420-665">The previous example highlights two of the limitations of structs.</span></span> <span data-ttu-id="bc420-666">İlk olarak, bir yapının tamamını kopyalamak bir nesne başvurusunu kopyalamaya kıyasla genellikle daha etkilidir, bu nedenle atama ve değer parametresi, yapı ile başvuru türleriyle daha pahalı olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-666">First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types.</span></span> <span data-ttu-id="bc420-667">İkincisi, `ref` ve parametreleri dışında `out` , yapılara başvurular oluşturmak mümkün değildir ve bu sayede kullanımları bir dizi durumda kurallar oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc420-667">Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.</span></span>

## <a name="arrays"></a><span data-ttu-id="bc420-668">Diziler</span><span class="sxs-lookup"><span data-stu-id="bc420-668">Arrays</span></span>

<span data-ttu-id="bc420-669">***Dizi*** , hesaplanan dizinler üzerinden erişilen çeşitli değişkenler içeren bir veri yapısıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-669">An ***array*** is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="bc420-670">Bir dizide bulunan değişkenler, dizi ***öğeleri*** de denir, hepsi aynı türdür ve bu tür dizinin ***öğe türü*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-670">The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="bc420-671">Dizi türleri başvuru türlerdir ve bir dizi değişkeninin bildirimi, bir dizi örneğine yönelik bir başvuru için yalnızca alan ayırın.</span><span class="sxs-lookup"><span data-stu-id="bc420-671">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="bc420-672">Gerçek dizi örnekleri işleci kullanılarak, `new` çalışma zamanında dinamik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bc420-672">Actual array instances are created dynamically at run-time using the `new` operator.</span></span> <span data-ttu-id="bc420-673">İşlem, daha sonra örneğin ömrü boyunca düzeltilen yeni dizi örneğinin uzunluğunu belirtir. `new`</span><span class="sxs-lookup"><span data-stu-id="bc420-673">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="bc420-674">Bir dizi öğelerinin ' den `0` `Length - 1`' a kadar olan dizinleri.</span><span class="sxs-lookup"><span data-stu-id="bc420-674">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="bc420-675">İşleci, bir dizinin öğelerini otomatik olarak varsayılan değerlerine başlatır. Bu, örneğin, tüm sayısal türler ve `null` tüm başvuru türleri için sıfırdır. `new`</span><span class="sxs-lookup"><span data-stu-id="bc420-675">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="bc420-676">Aşağıdaki örnek, bir dizi `int` öğe oluşturur, diziyi başlatır ve dizinin içeriğini yazdırır.</span><span class="sxs-lookup"><span data-stu-id="bc420-676">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

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
<span data-ttu-id="bc420-677">Bu örnek, ***tek boyutlu bir dizi***üzerinde oluşturur ve çalışır.</span><span class="sxs-lookup"><span data-stu-id="bc420-677">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="bc420-678">C#, ***çok boyutlu dizileri***de destekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-678">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="bc420-679">Dizi türünün ***derecesi*** olarak da bilinen bir dizi türünün boyut sayısı, dizi türünün köşeli ayraçları arasına yazılan virgüllerin sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-679">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="bc420-680">Aşağıdaki örnek tek boyutlu, iki boyutlu ve üç boyutlu bir diziyi ayırır.</span><span class="sxs-lookup"><span data-stu-id="bc420-680">The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.</span></span>

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
<span data-ttu-id="bc420-681">Dizi 10 öğe içeriyorsa `a2` , dizi 50 (10 × 5 `a3` ) öğesi içerir ve dizi 100 (10 × 5 × 2) öğesi içerir. `a1`</span><span class="sxs-lookup"><span data-stu-id="bc420-681">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>

<span data-ttu-id="bc420-682">Bir dizinin öğe türü, bir dizi türü de dahil olmak üzere herhangi bir tür olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-682">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="bc420-683">Dizi türündeki öğeleri içeren bir dizi, bazen öğe dizilerinin uzunluklarının tümünün aynı olması gerektiğinden ***pürüzlü dizi*** olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-683">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same.</span></span> <span data-ttu-id="bc420-684">Aşağıdaki örnek dizi dizilerini `int`ayırır:</span><span class="sxs-lookup"><span data-stu-id="bc420-684">The following example allocates an array of arrays of `int`:</span></span>

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
<span data-ttu-id="bc420-685">İlk satır, her biri türü `int[]` ve her biri bir başlangıç `null`değeri olan üç öğe içeren bir dizi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bc420-685">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="bc420-686">Sonraki satırlar, Farklı uzunluklardaki ayrı dizi örneklerine yapılan başvurularla üç öğeyi başlatır.</span><span class="sxs-lookup"><span data-stu-id="bc420-686">The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="bc420-687">İşleci, dizi öğelerinin başlangıç değerlerinin, sınırlayıcılar `{` ve `}`arasında yazılan ifadelerin listesi olan bir ***dizi başlatıcısı***kullanılarak belirtilmesine izin verir. `new`</span><span class="sxs-lookup"><span data-stu-id="bc420-687">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="bc420-688">Aşağıdaki örnek, üç öğesi ile bir `int[]` ayırır ve başlatır.</span><span class="sxs-lookup"><span data-stu-id="bc420-688">The following example allocates and initializes an `int[]` with three elements.</span></span>

```csharp
int[] a = new int[] {1, 2, 3};
```
<span data-ttu-id="bc420-689">Dizi uzunluğunun ve `{` `}`arasındaki ifade sayısından çıkarsandığına unutmayın.</span><span class="sxs-lookup"><span data-stu-id="bc420-689">Note that the length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="bc420-690">Yerel değişken ve alan bildirimleri daha fazla kısaltılarak, dizi türünün yeniden oluşturulması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="bc420-690">Local variable and field declarations can be shortened further such that the array type does not have to be restated.</span></span>

```csharp
int[] a = {1, 2, 3};
```
<span data-ttu-id="bc420-691">Yukarıdaki örneklerin her ikisi de aşağıdaki gibi eşdeğerdir:</span><span class="sxs-lookup"><span data-stu-id="bc420-691">Both of the previous examples are equivalent to the following:</span></span>

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## <a name="interfaces"></a><span data-ttu-id="bc420-692">Arabirimler</span><span class="sxs-lookup"><span data-stu-id="bc420-692">Interfaces</span></span>

<span data-ttu-id="bc420-693">***Arabirim*** , sınıflar ve yapılar tarafından uygulanabilecek bir sözleşmeyi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="bc420-693">An ***interface*** defines a contract that can be implemented by classes and structs.</span></span> <span data-ttu-id="bc420-694">Arabirim, Yöntemler, özellikler, olaylar ve Dizin oluşturucular içerebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-694">An interface can contain methods, properties, events, and indexers.</span></span> <span data-ttu-id="bc420-695">Arabirim, tanımladığı üyelerin uygulamalarını sağlamaz; yalnızca arabirimini uygulayan sınıflar veya yapılar tarafından sağlanması gereken üyeleri belirtir.</span><span class="sxs-lookup"><span data-stu-id="bc420-695">An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.</span></span>

<span data-ttu-id="bc420-696">Arabirimler ***birden fazla devralma***kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-696">Interfaces may employ ***multiple inheritance***.</span></span> <span data-ttu-id="bc420-697">Aşağıdaki örnekte, arabirimi `IComboBox` `ITextBox` hem hem de öğesinden devralır. `IListBox`</span><span class="sxs-lookup"><span data-stu-id="bc420-697">In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.</span></span>

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
<span data-ttu-id="bc420-698">Sınıflar ve yapılar birden çok arabirim uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-698">Classes and structs can implement multiple interfaces.</span></span> <span data-ttu-id="bc420-699">Aşağıdaki örnekte, sınıfı `EditBox` hem hem `IDataBound`de `IControl` uygular.</span><span class="sxs-lookup"><span data-stu-id="bc420-699">In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.</span></span>

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
<span data-ttu-id="bc420-700">Bir sınıf veya yapı belirli bir arabirimi uygularsa, bu sınıfın veya yapının örnekleri örtülü olarak bu arabirim türüne dönüştürülebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-700">When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type.</span></span> <span data-ttu-id="bc420-701">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bc420-701">For example</span></span>

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
<span data-ttu-id="bc420-702">Bir örneğin belirli bir arabirimi uygulamak üzere statik olarak bilinmediği durumlarda dinamik tür atamaları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-702">In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used.</span></span> <span data-ttu-id="bc420-703">Örneğin, aşağıdaki deyimler bir nesnenin `IControl` ve `IDataBound` arabirim uygulamalarının elde edileceği dinamik tür yayınlarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-703">For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations.</span></span> <span data-ttu-id="bc420-704">Nesnenin gerçek türü olduğundan `EditBox`, yayınlar başarılı olur.</span><span class="sxs-lookup"><span data-stu-id="bc420-704">Because the actual type of the object is `EditBox`, the casts succeed.</span></span>

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
<span data-ttu-id="bc420-705">Önceki `EditBox` sınıfta `IControl` , arabiriminden `public` yöntemi ve arabiriminden`IDataBound` yöntemi, Üyeler kullanılarak uygulanır. `Bind` `Paint`</span><span class="sxs-lookup"><span data-stu-id="bc420-705">In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members.</span></span> <span data-ttu-id="bc420-706">C#Ayrıca, sınıf veya yapının üyeleri `public`yapmaktan kaçınmasına engel olabilecek ***Açık arabirim üye uygulamalarını***destekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-706">C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`.</span></span> <span data-ttu-id="bc420-707">Açık arabirim üyesi uygulama, tam nitelikli arabirim üye adı kullanılarak yazılır.</span><span class="sxs-lookup"><span data-stu-id="bc420-707">An explicit interface member implementation is written using the fully qualified interface member name.</span></span> <span data-ttu-id="bc420-708">Örneğin, `EditBox` sınıfı aşağıdaki gibi açık arabirim üye uygulamalarını `IDataBound.Bind` kullanarak `IControl.Paint` ve yöntemlerini uygulayabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-708">For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.</span></span>

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
<span data-ttu-id="bc420-709">Açık arabirim üyelerine yalnızca arabirim türü aracılığıyla erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-709">Explicit interface members can only be accessed via the interface type.</span></span> <span data-ttu-id="bc420-710">Örneğin, önceki `EditBox` sınıf tarafından sağlanmış `IControl.Paint` olan uygulama, yalnızca öncelikle `IControl` arabirim türüne `EditBox` başvuru dönüştürülürken çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-710">For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.</span></span>

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## <a name="enums"></a><span data-ttu-id="bc420-711">Numaralandırmalar</span><span class="sxs-lookup"><span data-stu-id="bc420-711">Enums</span></span>

<span data-ttu-id="bc420-712">***Sabit listesi türü*** , adlandırılmış sabitler kümesi olan ayrı bir değer türüdür.</span><span class="sxs-lookup"><span data-stu-id="bc420-712">An ***enum type*** is a distinct value type with a set of named constants.</span></span> <span data-ttu-id="bc420-713">Aşağıdaki örnek `Color` ,,, ve `Red` `Blue`üç sabit değeri `Green`olan adlı bir sabit listesi türü bildirir ve kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-713">The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.</span></span>

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
<span data-ttu-id="bc420-714">Her sabit listesi türünün, enum türünün ***temel alınan türü*** olarak adlandırılan karşılık gelen bir integral türü vardır.</span><span class="sxs-lookup"><span data-stu-id="bc420-714">Each enum type has a corresponding integral type called the ***underlying type*** of the enum type.</span></span> <span data-ttu-id="bc420-715">Temel alınan bir türü açıkça bildirmiyor bir sabit listesi türü, temel alınan `int`bir tür içerir.</span><span class="sxs-lookup"><span data-stu-id="bc420-715">An enum type that does not explicitly declare an underlying type has an underlying type of `int`.</span></span> <span data-ttu-id="bc420-716">Sabit listesi türünün depolama biçimi ve olası değerlerinin aralığı, temel alınan türüne göre belirlenir.</span><span class="sxs-lookup"><span data-stu-id="bc420-716">An enum type's storage format and range of possible values are determined by its underlying type.</span></span> <span data-ttu-id="bc420-717">Bir numaralandırma türünün üzerinde sürebelirlenen değerler kümesi, sabit listesi üyeleri tarafından sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="bc420-717">The set of values that an enum type can take on is not limited by its enum members.</span></span> <span data-ttu-id="bc420-718">Özellikle, bir sabit listesinin temel alınan türünün herhangi bir değeri, sabit listesi türüne dönüşebilir ve bu sabit listesi türünün ayrı bir geçerli değeridir.</span><span class="sxs-lookup"><span data-stu-id="bc420-718">In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.</span></span>

<span data-ttu-id="bc420-719">Aşağıdaki örnek, temel alınan `Alignment` `sbyte`türü ile adlı bir sabit listesi türü bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-719">The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.</span></span>

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
<span data-ttu-id="bc420-720">Önceki örnekte gösterildiği gibi, enum üye bildirimi üyenin değerini belirten bir sabit ifade içerebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-720">As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member.</span></span> <span data-ttu-id="bc420-721">Her sabit listesi üyesinin sabit değeri, sabit listesinin temel alınan türü aralığında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bc420-721">The constant value for each enum member must be in the range of the underlying type of the enum.</span></span> <span data-ttu-id="bc420-722">Sabit listesi üyesi bildirimi açıkça bir değer belirtmezse, üyeye sıfır değeri verilir (sabit listesi türünde ilk üye ise) veya metin içeriğini eklemek önceki enum üyesinin değeri ve bir.</span><span class="sxs-lookup"><span data-stu-id="bc420-722">When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.</span></span>

<span data-ttu-id="bc420-723">Sabit listesi değerleri tamsayı değerlerine dönüştürülebilir ve tür atamaları kullanılarak tam tersi olabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-723">Enum values can be converted to integral values and vice versa using type casts.</span></span> <span data-ttu-id="bc420-724">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bc420-724">For example</span></span>

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
<span data-ttu-id="bc420-725">Herhangi bir sabit listesi türünün varsayılan değeri, enum türüne dönüştürülen tam sayı değeridir.</span><span class="sxs-lookup"><span data-stu-id="bc420-725">The default value of any enum type is the integral value zero converted to the enum type.</span></span> <span data-ttu-id="bc420-726">Değişkenlerin otomatik olarak varsayılan bir değere başlatıldığı durumlarda, bu değer enum türü değişkenlerine verilen değerdir.</span><span class="sxs-lookup"><span data-stu-id="bc420-726">In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types.</span></span> <span data-ttu-id="bc420-727">Bir sabit listesi türünün varsayılan değerinin kolayca kullanılabilmesi için, değişmez `0` değer örtülü olarak herhangi bir numaralandırma türüne dönüştürülür.</span><span class="sxs-lookup"><span data-stu-id="bc420-727">In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type.</span></span> <span data-ttu-id="bc420-728">Bu nedenle, aşağıdakilere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-728">Thus, the following is permitted.</span></span>

```csharp
Color c = 0;
```

## <a name="delegates"></a><span data-ttu-id="bc420-729">Temsilciler</span><span class="sxs-lookup"><span data-stu-id="bc420-729">Delegates</span></span>

<span data-ttu-id="bc420-730">Bir ***temsilci türü*** , belirli bir parametre listesi ve dönüş türü olan yöntemlere yapılan başvuruları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="bc420-730">A ***delegate type*** represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="bc420-731">Temsilciler, yöntemleri değişkenlere atanabilecek ve parametre olarak geçirilen varlıklar olarak işleme olanağı tanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-731">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="bc420-732">Temsilciler, bazı diğer dillerde bulunan işlev işaretçileri kavramına benzerdir, ancak işlev işaretçilerinden farklı olarak Temsilciler nesne yönelimli ve tür açısından güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="bc420-732">Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="bc420-733">Aşağıdaki örnek adlı `Function`bir temsilci türü bildirir ve kullanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-733">The following example declares and uses a delegate type named `Function`.</span></span>

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
<span data-ttu-id="bc420-734">Temsilci türünün bir örneği `Function` , `double` bağımsız değişken alan ve bir `double` değer döndüren herhangi bir yönteme başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-734">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="bc420-735">Yöntemi, bir `Function` `double[]` öğesine verilen öğeleri uygular ve sonuçları ile döndürür. `double[]` `Apply`</span><span class="sxs-lookup"><span data-stu-id="bc420-735">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="bc420-736">Yönteminde, `Apply` bir`double[]`öğesine üç farklı işlev uygulamak için kullanılır. `Main`</span><span class="sxs-lookup"><span data-stu-id="bc420-736">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="bc420-737">Bir temsilci, statik bir yönteme (örn. veya `Square` `Math.Sin` önceki örnekte olduğu gibi) ya da bir örnek yöntemine (örneğin, önceki `m.Multiply` örnekte olduğu gibi) başvurabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-737">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="bc420-738">Örnek yönteme başvuran bir temsilci belirli bir nesneye de başvurur ve örnek yöntemi temsilci aracılığıyla çağrıldığında, bu nesne çağrı içinde olur `this` .</span><span class="sxs-lookup"><span data-stu-id="bc420-738">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="bc420-739">Temsilciler, anında oluşturulan "satır içi Yöntemler" olan anonim işlevler kullanılarak da oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-739">Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly.</span></span> <span data-ttu-id="bc420-740">Anonim işlevler, çevresindeki yöntemlerin yerel değişkenlerini görebilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-740">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="bc420-741">Bu nedenle, yukarıdaki çarpan örneği bir `Multiplier` sınıf kullanılmadan daha kolay yazılabilir:</span><span class="sxs-lookup"><span data-stu-id="bc420-741">Thus, the multiplier example above can be written more easily without using a `Multiplier` class:</span></span>

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
<span data-ttu-id="bc420-742">Bir temsilcinin ilgi çekici ve yararlı bir özelliği, başvurduğu yöntemin sınıfını bilmez veya ilgilenmez; her önemli şey, başvurulan yöntemin aynı parametrelere sahip olması ve temsilciyle aynı türde dönüş türüdür.</span><span class="sxs-lookup"><span data-stu-id="bc420-742">An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="attributes"></a><span data-ttu-id="bc420-743">Öznitelikler</span><span class="sxs-lookup"><span data-stu-id="bc420-743">Attributes</span></span>

<span data-ttu-id="bc420-744">Bir C# programdaki türler, Üyeler ve diğer varlıklar, davranışlarının belirli yönlerini denetleyen değiştiricilerin özelliklerini destekler.</span><span class="sxs-lookup"><span data-stu-id="bc420-744">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="bc420-745">Örneğin, bir `public`yöntemin erişilebilirliği `internal`, `protected`,, ve `private` değiştiricileri kullanılarak denetlenir.</span><span class="sxs-lookup"><span data-stu-id="bc420-745">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="bc420-746">C#Bu özelliği, bildirim temelli bilgilerin Kullanıcı tanımlı türleri program varlıklarına iliştirilebilecek ve çalışma zamanında alınabilecek şekilde genelleştirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-746">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="bc420-747">Programlar, ***öznitelikleri***tanımlayarak ve kullanarak bu ek bildirime dayalı bilgileri belirtir.</span><span class="sxs-lookup"><span data-stu-id="bc420-747">Programs specify this additional declarative information by defining and using ***attributes***.</span></span>

<span data-ttu-id="bc420-748">Aşağıdaki örnek, ilişkili belgelerinin `HelpAttribute` bağlantılarını sağlamak için program varlıklarına yerleştirilebilecek bir özniteliği bildirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-748">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

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
<span data-ttu-id="bc420-749">Tüm öznitelik sınıfları, .NET Framework tarafından `System.Attribute` belirtilen temel sınıftan türetilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-749">All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework.</span></span> <span data-ttu-id="bc420-750">Öznitelikler, ilişkili bildirimden hemen önceki köşeli parantez içinde, adı ve bağımsız değişkenlerle birlikte uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-750">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="bc420-751">Bir özniteliğin adı içinde `Attribute`biterse, özniteliğe başvuruluyorsa adın bu bölümü atlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-751">If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="bc420-752">Örneğin, `HelpAttribute` özniteliği aşağıdaki gibi kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-752">For example, the `HelpAttribute` attribute can be used as follows.</span></span>

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
<span data-ttu-id="bc420-753">Bu örnek, sınıfına `HelpAttribute` `Widget` öğesine ve başka `HelpAttribute` `Display` bir sınıftaki yöntemine bir iliştirir.</span><span class="sxs-lookup"><span data-stu-id="bc420-753">This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="bc420-754">Öznitelik sınıfının ortak oluşturucuları, özniteliği bir program varlığına eklendiğinde sağlanması gereken bilgileri denetler.</span><span class="sxs-lookup"><span data-stu-id="bc420-754">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="bc420-755">Öznitelik sınıfının genel okuma-yazma özelliklerine (daha önce `Topic` özelliğine başvuru gibi) başvurarak ek bilgiler sunulabilir.</span><span class="sxs-lookup"><span data-stu-id="bc420-755">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="bc420-756">Aşağıdaki örnek, belirli bir program varlığı için öznitelik bilgilerinin yansıma kullanarak çalışma zamanında nasıl alınamayacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="bc420-756">The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.</span></span>

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
<span data-ttu-id="bc420-757">Belirli bir öznitelik yansıma aracılığıyla istendiğinde, öznitelik sınıfı için Oluşturucu program kaynağında sağlanan bilgilerle çağrılır ve sonuçta elde edilen öznitelik örneği döndürülür.</span><span class="sxs-lookup"><span data-stu-id="bc420-757">When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="bc420-758">Özellikler aracılığıyla ek bilgiler sağlanmışsa, öznitelik örneği döndürülmeden önce bu özellikler verilen değerlere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="bc420-758">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>
