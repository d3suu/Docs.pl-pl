---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Tworzenie widoku (UI) | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a><span data-ttu-id="a5ae5-102">Tworzenie widoku (UI)</span><span class="sxs-lookup"><span data-stu-id="a5ae5-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="a5ae5-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a5ae5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a5ae5-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="a5ae5-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="a5ae5-105">W tej sekcji rozpocznie zdefiniuj kod HTML dla aplikacji, a następnie Dodaj powiązanie danych pomiędzy HTML a model widoku.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="a5ae5-106">Otwórz plik Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="a5ae5-107">Zamień całą zawartość tego pliku poniżej.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="a5ae5-108">Większość `div` elementy są związane z [Bootstrap](http://getbootstrap.com/) style.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="a5ae5-109">Ważne elementy są pokazane z `data-bind` atrybutów.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="a5ae5-110">Ten atrybut łączy HTML na model widoku.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="a5ae5-111">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a5ae5-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="a5ae5-112">W tym przykładzie &quot; `text` &quot; powiązanie przyczyny `<p>` element, aby wyświetlić wartość `error` właściwość z modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="a5ae5-113">Odwołania, który `error` został zadeklarowany jako `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="a5ae5-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="a5ae5-114">Gdy nowa wartość jest przypisywana do `error`, odcinania aktualizuje tekst w `<p>` elementu.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="a5ae5-115">`foreach` Powiązania informuje odcinania pętli zawartość `books` tablicy.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="a5ae5-116">Dla każdego elementu w tablicy, tworzy nową Knockout &lt;li&gt; elementu.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="a5ae5-117">Powiązania w kontekście `foreach` odwoływać się do właściwości w elemencie tablicy.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="a5ae5-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a5ae5-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="a5ae5-119">W tym miejscu `text` powiązania odczytuje właściwość Autor każdego książki.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="a5ae5-120">Jeśli uruchomisz aplikację teraz powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="a5ae5-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="a5ae5-121">Lista książek ładuje asynchronicznie, po pobraniu strony.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="a5ae5-122">Teraz &quot;szczegóły&quot; łącza nie działają.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="a5ae5-123">Ta funkcja zostanie dodany w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="a5ae5-123">We'll add this functionality in the next section.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a5ae5-124">[Poprzednie](part-6.md)
[dalej](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="a5ae5-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>