---
title: Implementacje serwera sieci Web w programie ASP.NET Core
author: rick-anderson
description: Odnajdywanie serwerów sieci web w usługach Kestrel i sterownik HTTP.sys dla platformy ASP.NET Core. Dowiedz się, jak wybrać serwer i kiedy należy użyć zwrotnego serwera proxy.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 0f1460af5bc1cd879ff11e43775ac16ca36b150e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011758"
---
# <a name="web-server-implementations-in-aspnet-core"></a>Implementacje serwera sieci Web w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Halter Autor: Stephen](https://twitter.com/halter73), i [Chris Ross](https://github.com/Tratcher)

Aplikacji ASP.NET Core jest uruchamiany z implementację serwera HTTP w procesie. Implementacja serwera nasłuchuje żądań HTTP i udostępnia je do aplikacji jako zestawów [funkcje na żądanie](xref:fundamentals/request-features) składające się na [HttpContext](/dotnet/api/system.web.httpcontext).

ASP.NET Core jest dostarczana w dwie implementacje serwera:

* [Kestrel](xref:fundamentals/servers/kestrel) jest to domyślne, serwer HTTP dla wielu platform dla platformy ASP.NET Core.
* [Sterownik HTTP.sys](xref:fundamentals/servers/httpsys) serwera HTTP tylko do Windows opiera się na [sterownik jądra HTTP.sys i interfejsu API serwera HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). (Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w programie ASP.NET Core 1.x.)

## <a name="kestrel"></a>Kestrel

Kestrel jest domyślny serwer sieci web zawarte w szablonach projektu ASP.NET Core.

::: moniker range=">= aspnetcore-2.0"

Kestrel może służyć przez siebie lub za pomocą *zwrotnego serwera proxy*, na przykład serwer Nginx, Apache lub IIS. Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi.

![Kestrel komunikuje się bezpośrednio z Internetu bez zwrotnego serwera proxy](kestrel/_static/kestrel-to-internet2.png)

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

Każda konfiguracja&mdash;z lub bez serwera proxy odwrotnej&mdash;jest prawidłowy i obsługiwanych konfiguracji hostingu dla platformy ASP.NET Core 2.0 lub nowszej aplikacje. Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Jeśli aplikacja będzie akceptować tylko żądania z siecią wewnętrzną, Kestrel może służyć przez siebie.

![Kestrel komunikuje się bezpośrednio z siecią wewnętrzną](kestrel/_static/kestrel-to-internal.png)

Jeśli aplikacja jest połączenie z Internetem, Kestrel należy użyć usług IIS, serwera Nginx lub Apache jako *zwrotnego serwera proxy*. Zwrotnego serwera proxy odbiera żądania HTTP z Internetu i przekazuje je do Kestrel po niektórych wstępnego obsługi, jak pokazano na poniższym diagramie:

![Kestrel komunikuje się bezpośrednio z Internetem za pośrednictwem serwera zwrotny serwer proxy, na przykład serwer Nginx, Apache lub IIS](kestrel/_static/kestrel-to-internet.png)

Najważniejszą przyczyną za używanie zwrotnego serwera proxy na potrzeby wdrożeń usługi edge (ujawniony na ruch z Internetu) to zabezpieczeń. Wersji 1.x Kestrel nie mają ważne funkcje zabezpieczeń do ochrony przed atakami z sieci Internet. Obejmuje, ale nie jest ograniczona do odpowiednie limity czasu, limity rozmiaru żądania i limity liczby jednoczesnych połączeń.

Aby uzyskać więcej informacji, zobacz [kiedy należy używać Kestrel przy użyciu zwrotnego serwera proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

::: moniker-end

Nie można używać usług IIS, serwera Nginx i Apache bez Kestrel lub [implementacji niestandardowego serwera](#custom-servers). ASP.NET Core zaprojektowano do uruchamiania w swoim własnym procesie, aby mogła działać spójnie na platformach. Usługi IIS, serwera Nginx i Apache określają własne procedurę startową i środowiska. Aby przy użyciu tych technologii server bezpośrednio, platformy ASP.NET Core potrzebować do dostosowania do wymagań poszczególnych serwerów. Przy użyciu implementacji serwera sieci web, takich jak Kestrel, ASP.NET Core ma kontrolę nad procesu uruchamiania i środowiska, gdy hostowany na innym serwerze technologiach.

### <a name="iis-with-kestrel"></a>Usługi IIS za pomocą Kestrel

Korzystając z [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) jako zwrotny serwer proxy dla platformy ASP.NET Core w aplikacji ASP.NET Core działa w procesie oddzielnie od proces roboczy usług IIS. W procesie programu IIS [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) koordynuje relacji zwrotnego serwera proxy. Podstawowe funkcje modułu ASP.NET Core są do uruchamiania aplikacji ASP.NET Core, uruchom ponownie aplikację uległa awarii i kierują ruch HTTP do aplikacji. Aby uzyskać więcej informacji, zobacz [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module). 

### <a name="nginx-with-kestrel"></a>Serwer Nginx z Kestrel

Aby uzyskać informacje dotyczące sposobu używania jako zwrotny serwer proxy serwera Nginx w systemie Linux dla Kestrel, zobacz [hosting w systemie Linux przy użyciu serwera Nginx](xref:host-and-deploy/linux-nginx).

### <a name="apache-with-kestrel"></a>Apache z Kestrel

Aby uzyskać informacje na temat sposobu na użytek Apache w systemie Linux jako serwer proxy odwrotnej Kestrel, zobacz [hosting w systemie Linux z Apache](xref:host-and-deploy/linux-apache).

## <a name="httpsys"></a>HTTP.sys

::: moniker range=">= aspnetcore-2.0"

Jeśli aplikacje platformy ASP.NET Core są uruchamiane na Windows, sterownik HTTP.sys stanowi alternatywę Kestrel. Ogólnie zaleca się kestrel w celu uzyskania najlepszej wydajności. Sterownik HTTP.sys może służyć w scenariuszach, gdzie aplikacja jest połączenie z Internetem i wymagane możliwości są obsługiwane przez rozszerzenie HTTP.sys, ale nie Kestrel. Aby uzyskać informacji na temat HTTP.sys, zobacz [HTTP.sys](xref:fundamentals/servers/httpsys).

![Sterownik HTTP.sys komunikuje się bezpośrednio z Internetu](httpsys/_static/httpsys-to-internet.png)

Sterownik HTTP.sys może również dla aplikacji, które są dostępne tylko z siecią wewnętrzną.

![Sterownik HTTP.sys komunikuje się bezpośrednio z siecią wewnętrzną](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Nosi nazwę HTTP.sys [WebListener](xref:fundamentals/servers/weblistener) w programie ASP.NET Core 1.x. Jeśli aplikacje platformy ASP.NET Core są uruchamiane na Windows, WebListener jest alternatywą dla scenariuszy, w której usługi IIS nie jest dostępne dla aplikacji hosta.

![Weblistener komunikuje się bezpośrednio z Internetu](weblistener/_static/weblistener-to-internet.png)

WebListener można również zamiast Kestrel dla aplikacji, które są dostępne tylko z siecią wewnętrzną, jeśli jest to wymagane, że funkcje są obsługiwane przez WebListener, ale nie Kestrel. Aby uzyskać informacji na temat WebListener, zobacz [WebListener](xref:fundamentals/servers/weblistener).

![Weblistener komunikuje się bezpośrednio z siecią wewnętrzną](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a>Infrastruktura serwerowa ASP.NET Core

[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) dostępne w `Startup.Configure` ujawnia metody [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) właściwości typu [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection). Kestrel i sterownik HTTP.sys (WebListener w programie ASP.NET Core 1.x) udostępniają tylko jednej funkcji, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), ale implementacji różnych serwera może udostępnić dodatkowe funkcje.

`IServerAddressesFeature` można dowiedzieć się, port, który implementacji serwera została powiązana w czasie wykonywania.

## <a name="custom-servers"></a>Niestandardowe serwery

Jeśli wbudowane serwery nie spełniają wymagań dotyczących aplikacji, można utworzyć wdrożenia niestandardowego serwera. [Open Web Interface for .NET (OWIN) przewodnik](xref:fundamentals/owin) pokazuje, jak napisać [Nowin](https://github.com/Bobris/Nowin)— na podstawie [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementacji. Tylko interfejsy funkcji, których używa aplikacja wymaga wdrożenia, jeśli co najmniej [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) i [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) muszą być obsługiwane.

## <a name="server-startup"></a>Uruchamianie serwera

Korzystając z [programu Visual Studio](https://www.visualstudio.com/vs/), [programu Visual Studio dla komputerów Mac](https://www.visualstudio.com/vs/mac/), lub [programu Visual Studio Code](https://code.visualstudio.com/), serwer jest uruchamiany po uruchomieniu aplikacji przez (zintegrowane środowisko projektowe ŚRODOWISKO IDE). W programie Visual Studio, Windows, profilów uruchamiania może służyć do uruchomienia aplikacji i serwera z oboma [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) lub konsoli. W programie Visual Studio Code, aplikacji i serwera są uruchamiane przez [technologię Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), które aktywuje debugera CoreCLR. W programie Visual Studio dla komputerów Mac do aplikacji i serwera są uruchamiane przez [Mono nietrwałego Tryb debugera](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).

Podczas uruchamiania aplikacji z poziomu wiersza polecenia w folderze projektu [dotnet, uruchom](/dotnet/core/tools/dotnet-run) spowoduje uruchomienie aplikacji i serwera (Kestrel i tylko w pliku HTTP.sys). Konfiguracja jest określona przez `-c|--configuration` opcja, która jest ustawiona jako `Debug` (ustawienie domyślne) lub `Release`. Jeśli profile uruchamiania są obecne w *launchSettings.json* pliku, użyj `--launch-profile <NAME>` opcję, aby ustawić profil uruchamiania (na przykład `Development` lub `Production`). Aby uzyskać więcej informacji, zobacz [dotnet, uruchom](/dotnet/core/tools/dotnet-run) i [tworzenie pakietów dystrybucji platformy .NET Core](/dotnet/core/build/distribution-packaging) tematów.

## <a name="http2-support"></a>Obsługa protokołu HTTP/2

[Protokołu HTTP/2](https://httpwg.org/specs/rfc7540.html) jest obsługiwana za pomocą programu ASP.NET Core w następujących scenariuszach wdrażania:

::: moniker range=">= aspnetcore-2.2"

* [Kestrel](xref:fundamentals/servers/kestrel#http2-support)
  * System operacyjny
    * Windows Server 2012 R2/Windows 8.1 lub nowszym
    * Linux z protokołem OpenSSL 1.0.2 lub nowszej (na przykład Ubuntu 16.04 lub nowszy)
    * Protokołu HTTP/2 będą obsługiwane w systemie macOS w przyszłej wersji.
  * Platforma docelowa: .NET Core 2,2 lub nowszy
* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016 i Windows 10 lub nowszym
  * Platforma docelowa: nie dotyczy wdrożeń HTTP.sys.
* [Usługi IIS (w procesie)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
  * Platforma docelowa: .NET Core 2,2 lub nowszy
* [Usługi IIS (poza procesem)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
  * Krawędź połączenia używają protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel używa protokołu HTTP/1.1.
  * Platforma docelowa: nie dotyczy wdrożeń spoza procesu usług IIS.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [HTTP.sys](xref:fundamentals/servers/httpsys#http2-support)
  * Windows Server 2016 i Windows 10 lub nowszym
  * Platforma docelowa: nie dotyczy wdrożeń HTTP.sys.
* [Usługi IIS (poza procesem)](xref:host-and-deploy/iis/index#http2-support)
  * Windows Server 2016 i Windows 10 lub nowszym; Usługi IIS 10 lub nowszym
  * Krawędź połączenia używają protokołu HTTP/2, ale połączenie zwrotny serwer proxy Kestrel używa protokołu HTTP/1.1.
  * Platforma docelowa: nie dotyczy wdrożeń spoza procesu usług IIS.

::: moniker-end

Należy użyć połączenia protokołu HTTP/2 [negocjowania protokołu warstwy aplikacji (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) i TLS 1.2 lub nowszej. Aby uzyskać więcej informacji zobacz tematy, które odnoszą się do scenariuszy wdrażania serwera.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:fundamentals/servers/httpsys> (dla platformy ASP.NET Core 1.x, zobacz <xref:fundamentals/servers/weblistener>)
