---
uid: web-api/overview/advanced/dependency-injection
title: "Iniekcji zależności w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym samouczku przedstawiono sposób wstrzyknięcia zależności w kontrolerze interfejsu API sieci Web platformy ASP.NET. Wersje oprogramowania używany w samouczek zablokowanych witryn sieci Web API 2 platformy Unity aplikacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="a5251-104">Iniekcji zależności w składniku ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="a5251-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="a5251-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a5251-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a5251-106">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="a5251-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="a5251-107">W tym samouczku przedstawiono sposób wstrzyknięcia zależności w kontrolerze interfejsu API sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5251-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a5251-108">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="a5251-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a5251-109">Składnik Web API 2</span><span class="sxs-lookup"><span data-stu-id="a5251-109">Web API 2</span></span>
> - [<span data-ttu-id="a5251-110">Blokowanie aplikacji Unity</span><span class="sxs-lookup"><span data-stu-id="a5251-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="a5251-111">Entity Framework 6 (działa także w wersji 5)</span><span class="sxs-lookup"><span data-stu-id="a5251-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="a5251-112">Co to jest iniekcji zależności?</span><span class="sxs-lookup"><span data-stu-id="a5251-112">What is Dependency Injection?</span></span>

<span data-ttu-id="a5251-113">A *zależności* jest dowolny obiekt, który wymaga innego obiektu.</span><span class="sxs-lookup"><span data-stu-id="a5251-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="a5251-114">Na przykład jest często do definiowania [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) obsługująca dostęp do danych.</span><span class="sxs-lookup"><span data-stu-id="a5251-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="a5251-115">Załóżmy przedstawiono przykład.</span><span class="sxs-lookup"><span data-stu-id="a5251-115">Let's illustrate with an example.</span></span> <span data-ttu-id="a5251-116">Po pierwsze zdefiniujemy modelu domeny:</span><span class="sxs-lookup"><span data-stu-id="a5251-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="a5251-117">Oto prosty repozytorium klasę, która przechowuje elementy w bazie danych przy użyciu programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a5251-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="a5251-118">Teraz zdefiniujmy kontrolera interfejsu API sieci Web, która obsługuje żądania GET `Product` jednostek.</span><span class="sxs-lookup"><span data-stu-id="a5251-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="a5251-119">(I używam pomijając POST i innych metod dla uproszczenia.) W tym miejscu jest pierwsza próba:</span><span class="sxs-lookup"><span data-stu-id="a5251-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="a5251-120">Należy zauważyć, że klasa kontrolera jest zależny od `ProductRepository`, możemy są co kontroler, Utwórz `ProductRepository` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a5251-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="a5251-121">Jednak jest dobrym pomysłem twardych kodu zależności w ten sposób z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="a5251-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="a5251-122">Jeśli chcesz zamienić `ProductRepository` z implementacją innego, należy również zmodyfikować klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5251-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="a5251-123">Jeśli `ProductRepository` ma zależności, należy skonfigurować wewnątrz kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5251-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="a5251-124">Dla dużych projektów z wielu kontrolerów kodu konfiguracji staje się znajdują się na projekt.</span><span class="sxs-lookup"><span data-stu-id="a5251-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="a5251-125">Trudno jest testu jednostkowego, ponieważ kontroler jest ustalony na zapytanie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="a5251-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="a5251-126">Dla testu jednostkowego należy użyć makiety lub stub repozytorium, co nie jest możliwe w projekcie currect.</span><span class="sxs-lookup"><span data-stu-id="a5251-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="a5251-127">Można rozwiązać te problemy przez *wstrzyknięcie* repozytorium do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5251-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="a5251-128">Po pierwsze, Refaktoryzuj `ProductRepository` klasy w interfejsie:</span><span class="sxs-lookup"><span data-stu-id="a5251-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="a5251-129">Następnie podaj `IProductRepository` jako parametru konstruktora:</span><span class="sxs-lookup"><span data-stu-id="a5251-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="a5251-130">W tym przykładzie użyto [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="a5251-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="a5251-131">Można również użyć *iniekcji metody ustawiającej*, których wartość zależności za pomocą metody ustawiającej lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="a5251-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="a5251-132">Ale teraz występuje problem, ponieważ aplikacja nie bezpośrednio tworzy kontroler.</span><span class="sxs-lookup"><span data-stu-id="a5251-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="a5251-133">Interfejs API sieci Web tworzy kontroler, gdy kieruje żądanie i interfejsu API sieci Web nie ma żadnych informacji dotyczących `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="a5251-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="a5251-134">Jest to, gdzie mechanizmu rozpoznawania zależności interfejsu API sieci Web jest dostarczany.</span><span class="sxs-lookup"><span data-stu-id="a5251-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="a5251-135">Mechanizm rozpoznawania zależności interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="a5251-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="a5251-136">Definiuje interfejs API sieci Web **elementu IDependencyResolver** interfejs do rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="a5251-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="a5251-137">W tym miejscu znajduje się definicja interfejsu:</span><span class="sxs-lookup"><span data-stu-id="a5251-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="a5251-138">**IDependencyScope** interfejs ma dwóch metod:</span><span class="sxs-lookup"><span data-stu-id="a5251-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="a5251-139">**Funkcja GetService** tworzy jedno wystąpienie typu.</span><span class="sxs-lookup"><span data-stu-id="a5251-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="a5251-140">**Metodę GetServices** tworzy kolekcję obiektów określonego typu.</span><span class="sxs-lookup"><span data-stu-id="a5251-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="a5251-141">**Elementu IDependencyResolver** dziedziczy metody **IDependencyScope** i dodaje **BeginScope** metody.</span><span class="sxs-lookup"><span data-stu-id="a5251-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="a5251-142">Będzie porozmawiać o zakresach w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a5251-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="a5251-143">Gdy interfejs API sieci Web tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver.GetService**, przekazując typ kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5251-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="a5251-144">Tego punktu zaczepienia rozszerzalności umożliwia utworzenie kontrolera, rozpoznawania zależności.</span><span class="sxs-lookup"><span data-stu-id="a5251-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="a5251-145">Jeśli **GetService** zwraca wartość null, interfejsu API sieci Web szuka konstruktora dla klasy kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5251-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="a5251-146">Rozpoznawanie zależności z kontenerem Unity</span><span class="sxs-lookup"><span data-stu-id="a5251-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="a5251-147">Mimo że można zapisać pełnego **elementu IDependencyResolver** implementacji od początku, interfejs naprawdę jest przeznaczony do działania jako mostka między interfejsu API sieci Web i istniejące kontenery Inwersja kontroli.</span><span class="sxs-lookup"><span data-stu-id="a5251-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="a5251-148">Kontenera IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależności.</span><span class="sxs-lookup"><span data-stu-id="a5251-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="a5251-149">Zarejestrować typy z kontenerem, a następnie użyć do tworzenia obiektów kontenera.</span><span class="sxs-lookup"><span data-stu-id="a5251-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="a5251-150">Kontener danych liczbowych automatycznie limit relacji zależności.</span><span class="sxs-lookup"><span data-stu-id="a5251-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="a5251-151">Również wiele kontenerów Inwersja kontroli pozwala na kontrolowanie okres istnienia obiektów i zakresu.</span><span class="sxs-lookup"><span data-stu-id="a5251-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="a5251-152">"IoC" oznacza "Inwersja kontroli", która jest wzorzec ogólne gdzie to struktura wywołuje kod aplikacji.</span><span class="sxs-lookup"><span data-stu-id="a5251-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="a5251-153">Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".</span><span class="sxs-lookup"><span data-stu-id="a5251-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="a5251-154">W tym samouczku, użyjemy [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) z Microsoft Patterns &amp; rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="a5251-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="a5251-155">(Obejmują innych popularnych bibliotek [Windsor zamku](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), i [StructureMap ](http://docs.structuremap.net/).) Menedżer pakietów NuGet służy do instalowania Unity.</span><span class="sxs-lookup"><span data-stu-id="a5251-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="a5251-156">Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a5251-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a5251-157">W oknie Konsola Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a5251-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="a5251-158">W tym miejscu jest implementacją **elementu IDependencyResolver** który opakowuje kontenera Unity.</span><span class="sxs-lookup"><span data-stu-id="a5251-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="a5251-159">Jeśli **GetService** metody nie można rozpoznać typu, powinny zwrócić **null**.</span><span class="sxs-lookup"><span data-stu-id="a5251-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="a5251-160">Jeśli **metodę GetServices** metody nie można rozpoznać typu, powinien zostać zwrócony obiekt pustej kolekcji.</span><span class="sxs-lookup"><span data-stu-id="a5251-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="a5251-161">Nie zgłaszają wyjątki dla nieznanych typów.</span><span class="sxs-lookup"><span data-stu-id="a5251-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="a5251-162">Konfigurowanie mechanizmu rozpoznawania zależności</span><span class="sxs-lookup"><span data-stu-id="a5251-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="a5251-163">Ustaw mechanizmu rozpoznawania zależności na **klasy DependencyResolver** właściwości globalne **HttpConfiguration** obiektu.</span><span class="sxs-lookup"><span data-stu-id="a5251-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="a5251-164">Poniższy kod rejestruje `IProductRepository` interfejsu z Unity, a następnie tworzy `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="a5251-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="a5251-165">Zakres zależności i okresem istnienia kontrolera</span><span class="sxs-lookup"><span data-stu-id="a5251-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="a5251-166">Kontrolery są tworzone na żądanie.</span><span class="sxs-lookup"><span data-stu-id="a5251-166">Controllers are created per request.</span></span> <span data-ttu-id="a5251-167">Aby zarządzać okresy istnienia obiektu, **elementu IDependencyResolver** korzysta z koncepcji *zakres*.</span><span class="sxs-lookup"><span data-stu-id="a5251-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="a5251-168">Mechanizm rozpoznawania zależności dołączony do **HttpConfiguration** obiekt ma zasięg globalny.</span><span class="sxs-lookup"><span data-stu-id="a5251-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="a5251-169">Gdy interfejs API sieci Web tworzy kontrolera, wywołuje **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="a5251-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="a5251-170">Ta metoda zwraca **IDependencyScope** reprezentujący zakresie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="a5251-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="a5251-171">Następnie wywołuje interfejs API sieci Web **GetService** w zakresie podrzędnym, aby utworzyć kontroler.</span><span class="sxs-lookup"><span data-stu-id="a5251-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="a5251-172">Po zakończeniu żądania wywołania interfejsu API sieci Web **Dispose** w zakresie podrzędnym.</span><span class="sxs-lookup"><span data-stu-id="a5251-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="a5251-173">Użyj **Dispose** metody zlikwidować zależności kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a5251-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="a5251-174">Jak zaimplementować **BeginScope** zależy kontenera IoC.</span><span class="sxs-lookup"><span data-stu-id="a5251-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="a5251-175">W przypadku Unity zakres odpowiada kontenera podrzędnego:</span><span class="sxs-lookup"><span data-stu-id="a5251-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="a5251-176">Większość kontenerów Inwersja kontroli mają podobne odpowiedniki.</span><span class="sxs-lookup"><span data-stu-id="a5251-176">Most IoC containers have similar equivalents.</span></span>