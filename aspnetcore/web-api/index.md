---
title: Tworzenie interfejsów API z platformy ASP.NET Core sieci web
author: scottaddie
description: Więcej informacji na temat funkcji dostępnych do tworzenia składnika web API platformy ASP.NET Core i jest odpowiednie do użycia w każdej funkcji.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/index
ms.openlocfilehash: 017bcc1ed65b1baa92408db07201d1c7bab2849d
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="97642-103">Tworzenie interfejsów API z platformy ASP.NET Core sieci web</span><span class="sxs-lookup"><span data-stu-id="97642-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="97642-104">Przez [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="97642-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="97642-105">[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97642-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="97642-106">Tym dokumencie opisano sposób tworzenia składnika web API platformy ASP.NET Core i jest najbardziej odpowiednie do użycia w każdej funkcji.</span><span class="sxs-lookup"><span data-stu-id="97642-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="97642-107">Klasa wyprowadzona z ControllerBase</span><span class="sxs-lookup"><span data-stu-id="97642-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="97642-108">Dziedzicz [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) klasy w kontrolerze, które mają służyć jako interfejs API sieci web.</span><span class="sxs-lookup"><span data-stu-id="97642-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="97642-109">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="97642-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="97642-110">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="97642-110">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="97642-111">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="97642-111">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]</span></span>
::: moniker-end

<span data-ttu-id="97642-112">`ControllerBase` Klasy zapewnia dostęp do wielu właściwości i metody.</span><span class="sxs-lookup"><span data-stu-id="97642-112">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="97642-113">W poprzednim przykładzie, niektóre z tych metod uwzględnić [element BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) i [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="97642-113">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="97642-114">Te metody są wywoływane w ramach metod akcji, aby zwrócić HTTP 400 i kodów stanu 201, odpowiednio.</span><span class="sxs-lookup"><span data-stu-id="97642-114">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="97642-115">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) właściwości w nim również podane przez `ControllerBase`, jest dostępny do wykonania żądania weryfikacji modelu.</span><span class="sxs-lookup"><span data-stu-id="97642-115">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="97642-116">Dodawanie adnotacji z ApiControllerAttribute — klasa</span><span class="sxs-lookup"><span data-stu-id="97642-116">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="97642-117">Platformy ASP.NET Core 2.1 wprowadzono `[ApiController]` atrybut określający Klasa kontrolera interfejsu API sieci web.</span><span class="sxs-lookup"><span data-stu-id="97642-117">ASP.NET Core 2.1 introduces the `[ApiController]` attribute to denote a web API controller class.</span></span> <span data-ttu-id="97642-118">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="97642-118">For example:</span></span>

<span data-ttu-id="97642-119">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="97642-119">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]</span></span>

<span data-ttu-id="97642-120">Ten atrybut jest często połączone z `ControllerBase` do uzyskania dostępu do właściwości i metod przydatne.</span><span class="sxs-lookup"><span data-stu-id="97642-120">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="97642-121">`ControllerBase` zapewnia dostęp do metod, takich jak [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) i [pliku](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="97642-121">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="97642-122">Innym rozwiązaniem jest utworzenie klasy podstawowej kontrolera niestandardowego opatrzoną `[ApiController]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="97642-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

<span data-ttu-id="97642-123">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]</span><span class="sxs-lookup"><span data-stu-id="97642-123">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]</span></span>

<span data-ttu-id="97642-124">W poniższych sekcjach opisano funkcje wygody dodane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="97642-124">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="97642-125">Automatyczne odpowiedzi HTTP 400</span><span class="sxs-lookup"><span data-stu-id="97642-125">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="97642-126">Błędy sprawdzania poprawności automatycznie wyzwoli odpowiedź HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="97642-126">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="97642-127">Poniższy kod staje się niepotrzebne w akcji:</span><span class="sxs-lookup"><span data-stu-id="97642-127">The following code becomes unnecessary in your actions:</span></span>

<span data-ttu-id="97642-128">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]</span><span class="sxs-lookup"><span data-stu-id="97642-128">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]</span></span>

<span data-ttu-id="97642-129">To domyślne zachowanie jest wyłączona w następującym kodem *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="97642-129">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="97642-130">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="97642-130">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="97642-131">Powiązania źródła parametru wnioskowania</span><span class="sxs-lookup"><span data-stu-id="97642-131">Binding source parameter inference</span></span>

<span data-ttu-id="97642-132">Atrybut źródłowy powiązanie definiuje lokalizacji, w którym znajduje się wartość parametru akcji.</span><span class="sxs-lookup"><span data-stu-id="97642-132">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="97642-133">Istnieją następujące atrybuty źródło powiązania:</span><span class="sxs-lookup"><span data-stu-id="97642-133">The following binding source attributes exist:</span></span>

|<span data-ttu-id="97642-134">Atrybut</span><span class="sxs-lookup"><span data-stu-id="97642-134">Attribute</span></span>|<span data-ttu-id="97642-135">Powiązania źródła</span><span class="sxs-lookup"><span data-stu-id="97642-135">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="97642-136">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="97642-136">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="97642-137">Treść żądania</span><span class="sxs-lookup"><span data-stu-id="97642-137">Request body</span></span> |
|<span data-ttu-id="97642-138">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="97642-138">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="97642-139">Dane formularza w treści żądania</span><span class="sxs-lookup"><span data-stu-id="97642-139">Form data in the request body</span></span> |
|<span data-ttu-id="97642-140">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="97642-140">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="97642-141">Nagłówek żądania</span><span class="sxs-lookup"><span data-stu-id="97642-141">Request header</span></span> |
|<span data-ttu-id="97642-142">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="97642-142">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="97642-143">Parametr ciągu zapytania żądania</span><span class="sxs-lookup"><span data-stu-id="97642-143">Request query string parameter</span></span> |
|<span data-ttu-id="97642-144">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="97642-144">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="97642-145">Dane trasy z bieżącego żądania</span><span class="sxs-lookup"><span data-stu-id="97642-145">Route data from the current request</span></span> |
|<span data-ttu-id="97642-146">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="97642-146">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="97642-147">Usługa żądania dodane jako parametru akcji</span><span class="sxs-lookup"><span data-stu-id="97642-147">The request service injected as an action parameter</span></span> |

<span data-ttu-id="97642-148">Bez `[ApiController]` atrybut powiązania źródła jawnie zdefiniowanych atrybutów.</span><span class="sxs-lookup"><span data-stu-id="97642-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="97642-149">W poniższym przykładzie `[FromQuery]` atrybut wskazuje, że `discontinuedOnly` wartość parametru jest podana w ciągu zapytania w adresie URL żądania:</span><span class="sxs-lookup"><span data-stu-id="97642-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

<span data-ttu-id="97642-150">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="97642-150">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]</span></span>

<span data-ttu-id="97642-151">Zasady wnioskowania są stosowane dla źródeł danych domyślne parametry akcji.</span><span class="sxs-lookup"><span data-stu-id="97642-151">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="97642-152">Te reguły Konfigurowanie źródeł powiązania, które w przeciwnym razie prawdopodobnie ręcznie zastosować do parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="97642-152">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="97642-153">Atrybuty źródło powiązania zachowują się w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="97642-153">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="97642-154">**[FromBody]**  jest wywnioskowany dla parametrów typu złożonego.</span><span class="sxs-lookup"><span data-stu-id="97642-154">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="97642-155">Wyjątek od tej reguły jest dowolnego typu złożonego, wbudowane o specjalnym znaczeniu, takie jak [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) i [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="97642-155">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="97642-156">Wnioskowanie kod źródłowy powiązanie ignoruje te typy specjalne.</span><span class="sxs-lookup"><span data-stu-id="97642-156">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="97642-157">Jeśli akcja ma więcej niż jeden parametr jawnie określony (za pośrednictwem `[FromBody]`) lub wywnioskować jako powiązane z treści żądania, jest zgłaszany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="97642-157">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="97642-158">Na przykład następujące podpisy akcji może spowodować wyjątek:</span><span class="sxs-lookup"><span data-stu-id="97642-158">For example, the following action signatures cause an exception:</span></span>

<span data-ttu-id="97642-159">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]</span><span class="sxs-lookup"><span data-stu-id="97642-159">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]</span></span>

* <span data-ttu-id="97642-160">**[FromForm]**  jest wywnioskowane dla parametrów akcji typu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="97642-160">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="97642-161">Nie jest wywnioskować dla wszystkich typów prostych lub zdefiniowanej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="97642-161">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="97642-162">**[FromRoute]**  jest wywnioskowany dla dowolnej nazwy parametru akcji pasującej do parametru w szablonie trasy.</span><span class="sxs-lookup"><span data-stu-id="97642-162">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="97642-163">Gdy wiele tras zgodne z parametrem akcji, wartości trasy jest uznawany za `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="97642-163">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="97642-164">**[FromQuery]**  jest wywnioskowany dla innych parametrów akcji.</span><span class="sxs-lookup"><span data-stu-id="97642-164">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="97642-165">Zasady wnioskowania domyślne są wyłączone w następującym kodem *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="97642-165">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="97642-166">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="97642-166">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]</span></span>

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="97642-167">Wnioskowanie multipart/dane formularza żądania</span><span class="sxs-lookup"><span data-stu-id="97642-167">Multipart/form-data request inference</span></span>

<span data-ttu-id="97642-168">Jeśli parametr akcji jest oznaczony za pomocą [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) atrybutu `multipart/form-data` żądania jest wywnioskowany typ zawartości.</span><span class="sxs-lookup"><span data-stu-id="97642-168">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="97642-169">Domyślnym zachowaniem jest wyłączona w następującym kodem *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="97642-169">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

<span data-ttu-id="97642-170">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="97642-170">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]</span></span>

### <a name="attribute-routing-requirement"></a><span data-ttu-id="97642-171">Atrybut wymaganie routingu</span><span class="sxs-lookup"><span data-stu-id="97642-171">Attribute routing requirement</span></span>

<span data-ttu-id="97642-172">Atrybut routingu staje się wymagania.</span><span class="sxs-lookup"><span data-stu-id="97642-172">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="97642-173">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="97642-173">For example:</span></span>

<span data-ttu-id="97642-174">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="97642-174">[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]</span></span>

<span data-ttu-id="97642-175">Akcje są niedostępne za pośrednictwem [trasy z konwencjonalnej](xref:mvc/controllers/routing#conventional-routing) zdefiniowane w [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) lub [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) w *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="97642-175">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="97642-176">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="97642-176">Additional resources</span></span>

* [<span data-ttu-id="97642-177">Zwracane typy akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="97642-177">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="97642-178">Niestandardowe elementy formatujące</span><span class="sxs-lookup"><span data-stu-id="97642-178">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="97642-179">Formatowanie danych odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="97642-179">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="97642-180">Strony pomocy przy użyciu programu Swagger</span><span class="sxs-lookup"><span data-stu-id="97642-180">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="97642-181">Routing do akcji kontrolera</span><span class="sxs-lookup"><span data-stu-id="97642-181">Routing to controller actions</span></span>](xref:mvc/controllers/routing)