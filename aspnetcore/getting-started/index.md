---
title: Wprowadzenie do platformy ASP.NET Core
author: rick-anderson
description: Samouczek szybki, które tworzy i uruchamia prostej aplikacji Hello World przy użyciu platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860943"
---
# <a name="get-started-with-aspnet-core"></a>Wprowadzenie do platformy ASP.NET Core

W tym dokumencie przedstawiono kroki umożliwiające utworzenie i uruchomienie aplikacji ASP.NET Core.

::: moniker range=">= aspnetcore-2.1"

1. Zainstaluj [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Tworzenie projektu platformy ASP.NET Core. Otwórz powłokę wiersza polecenia i wpisz następujące polecenie:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. Zaufanie certyfikatu deweloperskiego HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  Poprzednie polecenie wyświetla następujące okno dialogowe:

  ![Okno dialogowe ostrzeżenia o zabezpieczeniach](_static/cert.png)

  Wybierz **tak** Jeśli zgadzasz się ufać certyfikatowi rozwoju.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  Poprzednie polecenie wyświetli następujący komunikat:

  *Zażądano ufające certyfikatu deweloperskiego protokołu HTTPS. Jeśli certyfikat nie jest już zaufany możemy uruchom następujące polecenie:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
  * To polecenie może monitować o hasło, aby zainstalować certyfikat w pęku kluczy systemu.
  
  Hasło: *

  Wprowadź hasło, jeśli zgadzasz się ufać certyfikatowi rozwoju.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  W dokumentacji dla Twojej dystrybucji systemu Linux na temat zaufania certyfikatu deweloperskiego protokołu HTTPS.
   
---

4. Uruchom aplikację:

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. Przejdź do [ http://localhost:5001 ](http://localhost:5001).  Kliknij przycisk **Akceptuj** zaakceptować zasady ochrony prywatności i plików cookie. Ta aplikacja nie zachowuje informacji osobistych.

6. Otwórz *Pages/About.cshtml* i modyfikować strony z następujący wyróżniony kod znaczników:

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Przejdź do [ http://localhost:5001/About ](http://localhost:5001/About) i Sprawdź zmiany są wyświetlane.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Zainstaluj [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Utwórz nowy projekt ASP.NET Core.

   Otwórz powłokę wiersza polecenia. Wprowadź następujące polecenie:

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. Uruchom aplikację za pomocą następujących poleceń:

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. Przejdź do [ http://localhost:5000 ](http://localhost:5000).

5. Otwórz *Pages/About.cshtml*i zmodyfikuj stronę, aby wyświetlić komunikat „Hello, world! Czas na serwerze jest @DateTime.Now":

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Przejdź do [ http://localhost:5000/About ](http://localhost:5000/About) a następnie zweryfikować zmiany.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Zainstaluj program .NET Core **Instalator zestawu SDK** dla zestawu SDK 1.0.4 z [.NET Core wszystkie strony plików do pobrania](https://www.microsoft.com/net/download/all).

2. Utwórz folder dla nowego projektu ASP.NET Core.

   Otwórz powłokę wiersza polecenia. Wprowadź następujące polecenia:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Jeśli na komputerze zainstalowano nowszej wersji zestawu SDK, utworzyć *global.json* plik, aby zaznaczyć 1.0.4 zestawu SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Utwórz nowy projekt ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Przywróć pakiety.

   ```console
   dotnet restore
   ```

6. Uruchom aplikację.

   ```console
   dotnet run
   ```

   [Dotnet, uruchom](/dotnet/core/tools/dotnet-run) polecenie tworzy najpierw aplikacji, jeśli to konieczne.

7. Przejdź do `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
