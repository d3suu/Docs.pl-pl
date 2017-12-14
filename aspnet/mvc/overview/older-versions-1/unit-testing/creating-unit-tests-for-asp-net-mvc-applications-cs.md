---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: "Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (C#) | Dokumentacja firmy Microsoft"
author: StephenWalther
description: "Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera. W tym samouczku Stephen Walther pokazano, jak sprawdzić, czy akcji kontrolera zwraca częśći..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: 56c981363f1905c1c9869dbaf2adb6b5ac1c28a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a><span data-ttu-id="d8bf9-104">Tworzenie testów jednostkowych dla aplikacji ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="d8bf9-104">Creating Unit Tests for ASP.NET MVC Applications (C#)</span></span>
====================
<span data-ttu-id="d8bf9-105">przez [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="d8bf9-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="d8bf9-106">Pobierz plik PDF</span><span class="sxs-lookup"><span data-stu-id="d8bf9-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> <span data-ttu-id="d8bf9-107">Informacje o sposobie tworzenia testów jednostkowych dla akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-107">Learn how to create unit tests for controller actions.</span></span> <span data-ttu-id="d8bf9-108">W tym samouczku Stephen Walther pokazano, jak sprawdzić, czy zwraca określonego widoku akcji kontrolera, zwraca określonego zestawu danych lub zwraca innego typu wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-108">In this tutorial, Stephen Walther demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.</span></span>


<span data-ttu-id="d8bf9-109">Celem tego samouczka jest aby zademonstrować, jak możesz pisać testy jednostkowe dla kontrolerów programu ASP.NET MVC aplikacji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-109">The goal of this tutorial is to demonstrate how you can write unit tests for the controllers in your ASP.NET MVC applications.</span></span> <span data-ttu-id="d8bf9-110">Omówiono sposób tworzenia trzy różne typy testów jednostkowych.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-110">We discuss how to build three different types of unit tests.</span></span> <span data-ttu-id="d8bf9-111">Dowiesz się, jak przetestować widoku zwrócony przez akcji kontrolera, jak przetestować widoku danych zwróconych przez akcji kontrolera i testowanie, czy jednej akcji kontrolera przekieruje Cię do drugiej akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-111">You learn how to test the view returned by a controller action, how to test the View Data returned by a controller action, and how to test whether or not one controller action redirects you to a second controller action.</span></span>

## <a name="creating-the-controller-under-test"></a><span data-ttu-id="d8bf9-112">Tworzenie kontrolera w ramach testu</span><span class="sxs-lookup"><span data-stu-id="d8bf9-112">Creating the Controller under Test</span></span>

<span data-ttu-id="d8bf9-113">Zacznijmy od utworzenia kontrolera, który mamy zamierzają testu.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-113">Let's start by creating the controller that we intend to test.</span></span> <span data-ttu-id="d8bf9-114">Kontroler, o nazwie `ProductController`, znajduje się lista 1.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-114">The controller, named the `ProductController`, is contained in Listing 1.</span></span>

<span data-ttu-id="d8bf9-115">**1 — Lista`ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="d8bf9-115">**Listing 1 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

<span data-ttu-id="d8bf9-116">`ProductController` Zawiera dwie metody akcji, o nazwie `Index()` i `Details()`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-116">The `ProductController` contains two action methods named `Index()` and `Details()`.</span></span> <span data-ttu-id="d8bf9-117">Obie metody akcji zwracają widoku.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-117">Both action methods return a view.</span></span> <span data-ttu-id="d8bf9-118">Zwróć uwagę, że `Details()` akcji akceptuje parametr o nazwie identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-118">Notice that the `Details()` action accepts a parameter named Id.</span></span>

## <a name="testing-the-view-returned-by-a-controller"></a><span data-ttu-id="d8bf9-119">Testowanie widoku zwrócony przez kontroler</span><span class="sxs-lookup"><span data-stu-id="d8bf9-119">Testing the View returned by a Controller</span></span>

<span data-ttu-id="d8bf9-120">Załóżmy, że chcemy przetestować czy `ProductController` zwraca prawego widoku.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-120">Imagine that we want to test whether or not the `ProductController` returns the right view.</span></span> <span data-ttu-id="d8bf9-121">Chcemy upewnić się, że w przypadku `ProductController.Details()` akcji jest wywoływana, jest zwracany w widoku szczegółów.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-121">We want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned.</span></span> <span data-ttu-id="d8bf9-122">Klasy testowej wyświetlania 2 zawiera testu jednostkowego do testowania zwrócone przez widok `ProductController.Details()` akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-122">The test class in Listing 2 contains a unit test for testing the view returned by the `ProductController.Details()` action.</span></span>

<span data-ttu-id="d8bf9-123">**2 — Lista`ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="d8bf9-123">**Listing 2 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

<span data-ttu-id="d8bf9-124">Klasa w wyświetlania 2 zawiera metodę testu o nazwie `TestDetailsView()`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-124">The class in Listing 2 includes a test method named `TestDetailsView()`.</span></span> <span data-ttu-id="d8bf9-125">Ta metoda zawiera trzy wiersze kodu.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-125">This method contains three lines of code.</span></span> <span data-ttu-id="d8bf9-126">Pierwszy wiersz kodu tworzy nowe wystąpienie klasy `ProductController` klasy.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-126">The first line of code creates a new instance of the `ProductController` class.</span></span> <span data-ttu-id="d8bf9-127">Drugi wiersz kodu wywołuje kontrolera `Details()` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-127">The second line of code invokes the controller's `Details()` action method.</span></span> <span data-ttu-id="d8bf9-128">Na koniec ostatniego wiersza kodu sprawdza, czy widok zwrócony przez `Details()` akcji jest widok szczegółów.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-128">Finally, the last line of code checks whether or not the view returned by the `Details()` action is the Details view.</span></span>

<span data-ttu-id="d8bf9-129">`ViewResult.ViewName` Właściwość reprezentuje nazwę widoku zwrócony przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-129">The `ViewResult.ViewName` property represents the name of the view returned by a controller.</span></span> <span data-ttu-id="d8bf9-130">Jeden duży ostrzeżenie o testowaniu tej właściwości.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-130">One big warning about testing this property.</span></span> <span data-ttu-id="d8bf9-131">Istnieją dwa sposoby, że kontroler może zwrócić widok.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-131">There are two ways that a controller can return a view.</span></span> <span data-ttu-id="d8bf9-132">Kontroler jawnie może zwrócić widok następująco:</span><span class="sxs-lookup"><span data-stu-id="d8bf9-132">A controller can explicitly return a view like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

<span data-ttu-id="d8bf9-133">Alternatywnie nazwę widoku można wywnioskować na podstawie nazwę akcji kontrolera następująco:</span><span class="sxs-lookup"><span data-stu-id="d8bf9-133">Alternatively, the name of the view can be inferred from the name of the controller action like this:</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

<span data-ttu-id="d8bf9-134">Ta akcja kontrolera również zwraca widok o nazwie `Details`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-134">This controller action also returns a view named `Details`.</span></span> <span data-ttu-id="d8bf9-135">Nazwa widoku jest jednak wywnioskować na podstawie nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-135">However, the name of the view is inferred from the action name.</span></span> <span data-ttu-id="d8bf9-136">Jeśli chcesz przetestować nazwy widoku, następnie jawnie wróć Nazwa widoku z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-136">If you want to test the view name, then you must explicitly return the view name from the controller action.</span></span>

<span data-ttu-id="d8bf9-137">Można uruchomić testu jednostkowego wyświetlania 2, wprowadzając kombinację klawiszy **Ctrl-R, A** lub przez kliknięcie przycisku **Uruchom wszystkie testy z rozwiązania** przycisk (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="d8bf9-137">You can run the unit test in Listing 2 by either entering the keyboard combination **Ctrl-R, A** or by clicking the **Run All Tests in Solution** button (see Figure 1).</span></span> <span data-ttu-id="d8bf9-138">Jeśli test zakończył się pomyślnie, zostanie wyświetlone okno wyników testu na rysunku 2.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-138">If the test passes, you'll see the Test Results window in Figure 2.</span></span>


<span data-ttu-id="d8bf9-139">[![Uruchom wszystkie testy z rozwiązania](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d8bf9-139">[![Run All Tests in Solution](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)</span></span>

<span data-ttu-id="d8bf9-140">**Rysunek 01**: Uruchom wszystkie testy z rozwiązania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d8bf9-140">**Figure 01**: Run All Tests in Solution ([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))</span></span>


<span data-ttu-id="d8bf9-141">[![Powodzenie!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d8bf9-141">[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)</span></span>

<span data-ttu-id="d8bf9-142">**Rysunek 02**: sukcesu!</span><span class="sxs-lookup"><span data-stu-id="d8bf9-142">**Figure 02**: Success!</span></span> <span data-ttu-id="d8bf9-143">([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d8bf9-143">([Click to view full-size image](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))</span></span>


## <a name="testing-the-view-data-returned-by-a-controller"></a><span data-ttu-id="d8bf9-144">Testowanie widoku danych zwróconych przez kontrolera</span><span class="sxs-lookup"><span data-stu-id="d8bf9-144">Testing the View Data returned by a Controller</span></span>

<span data-ttu-id="d8bf9-145">Kontroler MVC przekazuje dane do widoku przy użyciu elementu o nazwie  *`View Data`* .</span><span class="sxs-lookup"><span data-stu-id="d8bf9-145">An MVC controller passes data to a view by using something called *`View Data`*.</span></span> <span data-ttu-id="d8bf9-146">Załóżmy na przykład chcesz wyświetlić szczegóły dotyczące danego produktu po wywołaniu `ProductController Details()` akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-146">For example, imagine that you want to display the details for a particular product when you invoke the `ProductController Details()` action.</span></span> <span data-ttu-id="d8bf9-147">W takim przypadku można utworzyć wystąpienia `Product` klasy (zdefiniowany w modelu) i przekaż wystąpienie `Details` widoku dzięki wykorzystaniu `View Data`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-147">In that case, you can create an instance of a `Product` class (defined in your model) and pass the instance to the `Details` view by taking advantage of `View Data`.</span></span>

<span data-ttu-id="d8bf9-148">Zmodyfikowane `ProductController` w wyświetlania 3 zawiera zaktualizowanego `Details()` akcję, która zwraca produktu.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-148">The modified `ProductController` in Listing 3 includes an updated `Details()` action that returns a Product.</span></span>

<span data-ttu-id="d8bf9-149">**3 — lista`ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="d8bf9-149">**Listing 3 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

<span data-ttu-id="d8bf9-150">Najpierw `Details()` akcja tworzy nowe wystąpienie klasy `Product` klasa, która reprezentuje komputer przenośny.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-150">First, the `Details()` action creates a new instance of the `Product` class that represents a laptop computer.</span></span> <span data-ttu-id="d8bf9-151">Następnie, wystąpienie `Product` klasy jest przekazywany jako drugi parametr `View()` metody.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-151">Next, the instance of the `Product` class is passed as the second parameter to the `View()` method.</span></span>

<span data-ttu-id="d8bf9-152">Można pisać testy jednostkowe można sprawdzić, czy jest oczekiwane dane zawarte w widoku danych.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-152">You can write unit tests to test whether the expected data is contained in view data.</span></span> <span data-ttu-id="d8bf9-153">Czy produkt reprezentujący komputer przenośny jest zwracany, gdy jest wywoływana w testach listę 4 testu jednostkowego `ProductController Details()` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-153">The unit test in Listing 4 tests whether or not a Product representing a laptop computer is returned when you call the `ProductController Details()` action method.</span></span>

<span data-ttu-id="d8bf9-154">**Wyświetlanie listy 4.`ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="d8bf9-154">**Listing 4 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

<span data-ttu-id="d8bf9-155">W przypadku wyświetlania 4 `TestDetailsView()` metoda sprawdza dane widoku zwrócony przez wywołanie `Details()` metody.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-155">In Listing 4, the `TestDetailsView()` method tests the View Data returned by invoking the `Details()` method.</span></span> <span data-ttu-id="d8bf9-156">`ViewData` Jest udostępniany jako właściwość na `ViewResult` zwrócony przez wywołanie `Details()` metody.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-156">The `ViewData` is exposed as a property on the `ViewResult` returned by invoking the `Details()` method.</span></span> <span data-ttu-id="d8bf9-157">`ViewData.Model` Właściwość zawiera produktu przekazywane do widoku.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-157">The `ViewData.Model` property contains the product passed to the view.</span></span> <span data-ttu-id="d8bf9-158">Po prostu test sprawdza, czy produktu zawartego w widoku danych ma nazwę komputera przenośnego.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-158">The test simply verifies that the product contained in the View Data has the name Laptop.</span></span>

## <a name="testing-the-action-result-returned-by-a-controller"></a><span data-ttu-id="d8bf9-159">Testowanie wynik akcji zwrócony przez kontroler</span><span class="sxs-lookup"><span data-stu-id="d8bf9-159">Testing the Action Result returned by a Controller</span></span>

<span data-ttu-id="d8bf9-160">Bardziej złożone akcji kontrolera może być zwracanie różnych typów wyników akcji w zależności od wartości parametrów przekazanych do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-160">A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action.</span></span> <span data-ttu-id="d8bf9-161">Akcja kontrolera może zwracać różne typy wyników akcji w tym `ViewResult`, `RedirectToRouteResult`, lub `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-161">A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.</span></span>

<span data-ttu-id="d8bf9-162">Na przykład zmodyfikowanych `Details()` zwraca akcji w listę 5 `Details` widoku, gdy przekazujesz prawidłowego identyfikatora produktu do akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-162">For example, the modified `Details()` action in Listing 5 returns the `Details` view when you pass a valid product Id to the action.</span></span> <span data-ttu-id="d8bf9-163">W przypadku przekazania nieprawidłowy produktu identyfikator - Id o wartości mniej niż 1 — a następnie użytkownik zostanie przekierowany do `Index()` akcji.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-163">If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the `Index()` action.</span></span>

<span data-ttu-id="d8bf9-164">**Wyświetlanie listy 5 —`ProductController.cs`**</span><span class="sxs-lookup"><span data-stu-id="d8bf9-164">**Listing 5 – `ProductController.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

<span data-ttu-id="d8bf9-165">Możesz przetestować działanie `Details()` akcji z testu jednostkowego wyświetlania 6.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-165">You can test the behavior of the `Details()` action with the unit test in Listing 6.</span></span> <span data-ttu-id="d8bf9-166">Weryfikuje testu jednostkowego w 6 wyświetlania nastąpi przekierowanie do `Index` widoku, gdy Id o wartości -1 są przekazywane do `Details()` metody.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-166">The unit test in Listing 6 verifies that you are redirected to the `Index` view when an Id with the value -1 is passed to the `Details()` method.</span></span>

<span data-ttu-id="d8bf9-167">**Wyświetlanie listy 6 —`ProductControllerTest.cs`**</span><span class="sxs-lookup"><span data-stu-id="d8bf9-167">**Listing 6 – `ProductControllerTest.cs`**</span></span>

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

<span data-ttu-id="d8bf9-168">Podczas wywoływania `RedirectToAction()` metoda akcji kontrolera, zwraca akcji kontrolera `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-168">When you call the `RedirectToAction()` method in a controller action, the controller action returns a `RedirectToRouteResult`.</span></span> <span data-ttu-id="d8bf9-169">Test sprawdza czy `RedirectToRouteResult` przekierowuje użytkownika do akcji kontrolera, o nazwie `Index`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-169">The test checks whether the `RedirectToRouteResult` will redirect the user to a controller action named `Index`.</span></span>

## <a name="summary"></a><span data-ttu-id="d8bf9-170">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="d8bf9-170">Summary</span></span>

<span data-ttu-id="d8bf9-171">W tym samouczku przedstawiono sposób tworzenia testów jednostkowych dla akcji kontrolera MVC.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-171">In this tutorial, you learned how to build unit tests for MVC controller actions.</span></span> <span data-ttu-id="d8bf9-172">Po pierwsze przedstawiono sposób sprawdzania, czy widok z prawej strony jest zwracana w wyniku akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-172">First, you learned how to verify whether the right view is returned by a controller action.</span></span> <span data-ttu-id="d8bf9-173">Przedstawiono sposób używania `ViewResult.ViewName` właściwości można zweryfikować nazwy widoku.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-173">You learned how to use the `ViewResult.ViewName` property to verify the name of a view.</span></span>

<span data-ttu-id="d8bf9-174">Następnie możemy zbadać jak można sprawdzić zawartość `View Data`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-174">Next, we examined how you can test the contents of `View Data`.</span></span> <span data-ttu-id="d8bf9-175">Przedstawiono sposób sprawdzania, czy odpowiedniego produktu został zwrócony w `View Data` po wywołaniu metody akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-175">You learned how to check whether the right product was returned in `View Data` after calling a controller action.</span></span>

<span data-ttu-id="d8bf9-176">Ponadto omówiono, jak można sprawdzić, czy różnych typów wyników akcji są zwracane z akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-176">Finally, we discussed how you can test whether different types of action results are returned from a controller action.</span></span> <span data-ttu-id="d8bf9-177">Przedstawiono sposób sprawdzić, czy kontroler zwraca `ViewResult` lub `RedirectToRouteResult`.</span><span class="sxs-lookup"><span data-stu-id="d8bf9-177">You learned how to test whether a controller returns a `ViewResult` or a `RedirectToRouteResult`.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d8bf9-178">Dalej</span><span class="sxs-lookup"><span data-stu-id="d8bf9-178">Next</span></span>](creating-unit-tests-for-asp-net-mvc-applications-vb.md)