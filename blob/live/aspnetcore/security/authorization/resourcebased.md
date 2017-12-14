---
title: Oparte na zasobach autoryzacji w ASP.NET Core
author: scottaddie
description: "Dowiedz się, jak zaimplementuj autoryzacji na podstawie zasobów w aplikacji platformy ASP.NET Core, gdy atrybut autoryzacji nie będą wystarczające."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 708f306da740870b106cbeeb96879480f8745439
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="ee8fd-103">Autoryzacji na podstawie zasobów</span><span class="sxs-lookup"><span data-stu-id="ee8fd-103">Resource-based authorization</span></span>

<span data-ttu-id="ee8fd-104">Przez [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ee8fd-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ee8fd-105">Strategia autoryzacji zależy od zasobu, do której uzyskuje dostęp.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-105">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="ee8fd-106">Należy wziąć pod uwagę dokumentu, który ma właściwość autora.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-106">Consider a document which has an author property.</span></span> <span data-ttu-id="ee8fd-107">Tylko autor może zaktualizować dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-107">Only the author is allowed to update the document.</span></span> <span data-ttu-id="ee8fd-108">W rezultacie dokument musi zostać pobrana z magazynu danych, zanim nastąpi ocena autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-108">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="ee8fd-109">Atrybut oceny występuje przed wiązania z danymi i przed realizacją strony obsługi lub akcji, który jest ładowany dokument.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-109">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="ee8fd-110">Z tego względu deklaratywne autoryzacji z `[Authorize]` atrybutu nie będą wystarczające.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-110">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="ee8fd-111">Zamiast tego można wywołać metody autoryzacji niestandardowej&mdash;stylu znany jako imperatywnych autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-111">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="ee8fd-112">Użyj [przykładowe aplikacje](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample)) aby poznać funkcje opisane w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-112">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="ee8fd-113">Użycie imperatywnych autoryzacji</span><span class="sxs-lookup"><span data-stu-id="ee8fd-113">Use imperative authorization</span></span>

<span data-ttu-id="ee8fd-114">Autoryzacja jest zaimplementowany jako [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) usługi i jest zarejestrowane w kolekcji usługi w ramach `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="ee8fd-115">Usługa ma zostać udostępnione za pośrednictwem [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) do obsługi strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="ee8fd-116">`IAuthorizationService`zawiera dwa `AuthorizeAsync` przeciążenia metody: przyjmuje jeden zasób i nazwę zasady i innych akceptowanie zasobu oraz listę wymagań dotyczących oceny.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee8fd-117">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee8fd-118">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="ee8fd-119">W poniższym przykładzie zasobów być zabezpieczone jest ładowany do niestandardowego `Document` obiektu.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="ee8fd-120">`AuthorizeAsync` Przeciążenia jest wywoływane w celu ustalenia, czy bieżący użytkownik może edytować dokument podana.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="ee8fd-121">Niestandardowe zasady autoryzacji "EditPolicy" jest brana pod uwagę w decyzji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="ee8fd-122">Zobacz [autoryzacji na podstawie zasad niestandardowych](xref:security/authorization/policies) Aby uzyskać więcej informacji na temat tworzenia zasad autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="ee8fd-123">Poniższy kod przykładów założono uwierzytelniania zostało uruchomione i zestaw `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee8fd-124">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee8fd-125">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="ee8fd-126">Pisanie programu obsługi opartych na zasobach</span><span class="sxs-lookup"><span data-stu-id="ee8fd-126">Write a resource-based handler</span></span>

<span data-ttu-id="ee8fd-127">Pisanie programu obsługi dla autoryzacji na podstawie zasobu nie jest znacznie różni się od [pisanie programu obsługi wymagań zwykły](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="ee8fd-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="ee8fd-128">Tworzenie klasy niestandardowej wymaganie i zaimplementować klasę programu obsługi wymagań.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="ee8fd-129">Klasa obsługi określa wymaganie i typu zasobu.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="ee8fd-130">Na przykład program obsługi przy użyciu `SameAuthorRequirement` wymaganie i `Document` zasobów wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="ee8fd-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee8fd-131">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee8fd-132">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="ee8fd-133">Zarejestruj wymagania oraz procedury obsługi w `Startup.ConfigureServices` metody:</span><span class="sxs-lookup"><span data-stu-id="ee8fd-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="ee8fd-134">Wymagań operacyjnych</span><span class="sxs-lookup"><span data-stu-id="ee8fd-134">Operational requirements</span></span>

<span data-ttu-id="ee8fd-135">Jeśli wprowadzasz decyzji, w oparciu o wyniki CRUD (**C**twórz, **R**eczytaj, **U**Aktualizuj, **D**Usuń) operacji, użyj [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) Klasa pomocy.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="ee8fd-136">Ta klasa umożliwia pisanie pojedynczego obsługi zamiast poszczególnych klas dla każdego typu operacji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="ee8fd-137">Aby go użyć, należy podać niektóre nazwy operacji:</span><span class="sxs-lookup"><span data-stu-id="ee8fd-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="ee8fd-138">Program obsługi jest implementowane, za pomocą `OperationAuthorizationRequirement` wymaganie i `Document` zasobów:</span><span class="sxs-lookup"><span data-stu-id="ee8fd-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee8fd-139">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee8fd-140">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="ee8fd-141">Powyższej procedury obsługi weryfikuje operację, używając zasobu, tożsamość użytkownika i wymaganie `Name` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="ee8fd-142">Aby wywołać obsługi operacyjne zasobów, określ podczas wywoływania operacji `AuthorizeAsync` w obsługi strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="ee8fd-143">Poniższy przykład określa, czy uwierzytelniony użytkownik będzie mógł wyświetlić dokument podana.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="ee8fd-144">Poniższy kod przykładów założono uwierzytelniania zostało uruchomione i zestaw `User` właściwości.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ee8fd-145">Program ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="ee8fd-146">Jeśli autoryzacji zakończy się powodzeniem, zwracany jest strona do wyświetlania dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="ee8fd-147">Jeśli autoryzacji kończy się niepowodzeniem, ale użytkownik jest uwierzytelniony, zwracając `ForbidResult` informuje oprogramowanie pośredniczące uwierzytelniania, która Autoryzacja nie powiodła się.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="ee8fd-148">A `ChallengeResult` jest zwracany, gdy uwierzytelnianie odbywa się.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="ee8fd-149">Dla klientów w przeglądarkach interaktywne można spróbować przekieruje użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ee8fd-150">Program ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ee8fd-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="ee8fd-151">Jeśli autoryzacji zakończy się pomyślnie, zostanie zwrócony widok dokumentu.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="ee8fd-152">W przypadku niepowodzenia autoryzacji zwracanie `ChallengeResult` informuje o wszystkich programów pośredniczących uwierzytelnienia Autoryzacja nie powiodła się, czy oprogramowanie pośredniczące może zająć właściwą odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="ee8fd-153">Właściwą odpowiedź może zwracać kod stanu 401 lub 403.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="ee8fd-154">Dla klientów w przeglądarkach interaktywne to może oznaczać przekierowanie użytkownika do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="ee8fd-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---