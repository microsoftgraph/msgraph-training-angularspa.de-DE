<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="100c2-101">In dieser Übung erweitern Sie die Anwendung von der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="100c2-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="100c2-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="100c2-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="100c2-103">In diesem Schritt integrieren Sie die [Microsoft-Authentifizierungsbibliothek für Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="100c2-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="100c2-104">Erstellen Sie eine neue Datei im **Verzeichnis ./src** mit dem Namen **oauth.ts,** und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="100c2-105">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus dem Anwendungsregistrierungsportal.</span><span class="sxs-lookup"><span data-stu-id="100c2-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="100c2-106">Wenn Sie die Quellcodeverwaltung wie git verwenden, sollten Sie jetzt die **oauth.ts-Datei** aus der Quellcodeverwaltung ausschließen, um versehentlich ein Leck ihrer App-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="100c2-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="100c2-107">Öffnen **Sie ./src/app/app.module.ts,** und fügen Sie die folgenden Anweisungen am oberen Rand `import` der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="100c2-108">Fügen Sie die folgende Funktion unterhalb der Anweisungen `import` hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-108">Add the following function below the `import` statements.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. <span data-ttu-id="100c2-109">Fügen Sie `MsalModule` das dem Array innerhalb der `imports` `@NgModule` Deklaration hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-109">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. <span data-ttu-id="100c2-110">Fügen Sie `MSALInstanceFactory` das und dem Array innerhalb der `MsalService` `providers` `@NgModule` Deklaration hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-110">Add the `MSALInstanceFactory` and `MsalService` to the `providers` array inside the `@NgModule` declaration.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a><span data-ttu-id="100c2-111">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="100c2-111">Implement sign-in</span></span>

<span data-ttu-id="100c2-112">In diesem Abschnitt erstellen Sie einen Authentifizierungsdienst und implementieren das Anmelden und Abmelden.</span><span class="sxs-lookup"><span data-stu-id="100c2-112">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="100c2-113">Führen Sie den folgenden Befehl in Ihrer CLI aus.</span><span class="sxs-lookup"><span data-stu-id="100c2-113">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="100c2-114">Indem Sie einen Dienst dafür erstellen, können Sie ihn ganz einfach in alle Komponenten integrieren, die Zugriff auf Authentifizierungsmethoden benötigen.</span><span class="sxs-lookup"><span data-stu-id="100c2-114">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="100c2-115">Öffnen Sie nach Abschluss des Befehls **./src/app/auth.service.ts,** und ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="100c2-115">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { AccountInfo } from '@azure/msal-browser';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user?: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = undefined;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        const result = await this.msalService
          .loginPopup(OAuthSettings)
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Login failed',
              JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.msalService.instance.setActiveAccount(result.account);
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      async signOut(): Promise<void> {
        await this.msalService.logout().toPromise();
        this.user = undefined;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        const result = await this.msalService
          .acquireTokenSilent({
            scopes: OAuthSettings.scopes
          })
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return '';
      }
    }
    ```

1. <span data-ttu-id="100c2-116">Öffnen **Sie ./src/app/nav-bar/nav-bar.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="100c2-116">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. <span data-ttu-id="100c2-117">Öffnen **Sie ./src/app/home/home.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="100c2-117">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

<span data-ttu-id="100c2-118">Speichern Sie Ihre Änderungen, und aktualisieren Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="100c2-118">Save your changes and refresh the browser.</span></span> <span data-ttu-id="100c2-119">Klicken Sie **auf die Schaltfläche Klicken Sie hier, um sich zu** anmelden, und Sie sollten an umgeleitet `https://login.microsoftonline.com` werden.</span><span class="sxs-lookup"><span data-stu-id="100c2-119">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="100c2-120">Melden Sie sich mit Ihrem Microsoft-Konto an, und stimmen Sie den angeforderten Berechtigungen zu.</span><span class="sxs-lookup"><span data-stu-id="100c2-120">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="100c2-121">Die App-Seite sollte aktualisiert werden und das Token anzeigen.</span><span class="sxs-lookup"><span data-stu-id="100c2-121">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="100c2-122">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="100c2-122">Get user details</span></span>

<span data-ttu-id="100c2-123">Derzeit legt der Authentifizierungsdienst konstante Werte für den Anzeigenamen und die E-Mail-Adresse des Benutzers fest.</span><span class="sxs-lookup"><span data-stu-id="100c2-123">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="100c2-124">Nachdem Sie über ein Zugriffstoken verfügen, können Sie Benutzerdetails aus Microsoft Graph erhalten, damit diese Werte dem aktuellen Benutzer entsprechen.</span><span class="sxs-lookup"><span data-stu-id="100c2-124">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="100c2-125">Öffnen **Sie ./src/app/auth.service.ts,** und fügen Sie die folgenden Anweisungen am oberen Rand `import` der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-125">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="100c2-126">Fügen Sie eine neue Funktion zur `AuthService`-Klasse mit dem Namen `getUser` hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-126">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. <span data-ttu-id="100c2-127">Suchen und entfernen Sie den folgenden Code in der Methode, die eine Warnung zum `getAccessToken` Anzeigen des Zugriffstokens hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="100c2-127">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="100c2-128">Suchen Und entfernen Sie den folgenden Code aus der `signIn` -Methode.</span><span class="sxs-lookup"><span data-stu-id="100c2-128">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="100c2-129">Fügen Sie an seiner Stelle den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="100c2-129">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="100c2-130">Dieser neue Code verwendet das Microsoft Graph SDK, um die Details des Benutzers zu erhalten, und erstellt dann ein Objekt mit Werten, die vom `User` API-Aufruf zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="100c2-130">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="100c2-131">Ändern Sie die für die Klasse, um zu überprüfen, ob der Benutzer bereits angemeldet `constructor` `AuthService` ist, und laden Sie gegebenenfalls ihre Details.</span><span class="sxs-lookup"><span data-stu-id="100c2-131">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="100c2-132">Ersetzen Sie das vorhandene `constructor` durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="100c2-132">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. <span data-ttu-id="100c2-133">Entfernen Sie den temporären Code aus der `HomeComponent` Klasse.</span><span class="sxs-lookup"><span data-stu-id="100c2-133">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="100c2-134">Öffnen **Sie ./src/app/home/home.component.ts,** und ersetzen Sie die vorhandene `signIn` Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="100c2-134">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

<span data-ttu-id="100c2-135">Wenn Sie nun Ihre Änderungen speichern und die App starten, sollten Sie nach der Anmeldung wieder auf der Startseite landen, aber die Benutzeroberfläche sollte sich ändern, um anzugeben, dass Sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="100c2-135">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Screenshot der Startseite nach dem Anmelden](./images/add-aad-auth-01.png)

<span data-ttu-id="100c2-137">Klicken Sie in der oberen rechten Ecke auf den Benutzer-Avatar, um auf den **Link Abmelden zu** zugreifen.</span><span class="sxs-lookup"><span data-stu-id="100c2-137">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="100c2-138">Wenn Sie auf **Abmelden** klicken, wird die Sitzung zurückgesetzt und Sie kehren zur Startseite zurück.</span><span class="sxs-lookup"><span data-stu-id="100c2-138">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Screenshot des Dropdown-Menüs mit dem Link „Abmelden“](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="100c2-140">Speichern und Aktualisieren von Token</span><span class="sxs-lookup"><span data-stu-id="100c2-140">Storing and refreshing tokens</span></span>

<span data-ttu-id="100c2-141">An diesem Punkt verfügt Ihre Anwendung über ein Zugriffstoken, das in der Kopfzeile von `Authorization` API-Aufrufen gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="100c2-141">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="100c2-142">Dies ist das Token, mit dem die App im Namen des Benutzers auf Microsoft Graph zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="100c2-142">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="100c2-143">Dieses Token ist jedoch nur kurzzeitig verfügbar.</span><span class="sxs-lookup"><span data-stu-id="100c2-143">However, this token is short-lived.</span></span> <span data-ttu-id="100c2-144">Das Token läuft eine Stunde nach der Enerbung ab.</span><span class="sxs-lookup"><span data-stu-id="100c2-144">The token expires an hour after it is issued.</span></span> <span data-ttu-id="100c2-145">Da die App die MSAL-Bibliothek verwendet, müssen Sie keine Tokenspeicher- oder Aktualisierungslogik implementieren.</span><span class="sxs-lookup"><span data-stu-id="100c2-145">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="100c2-146">Das `MsalService` Token wird im Browserspeicher zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="100c2-146">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="100c2-147">Die Methode überprüft zuerst das zwischengespeicherte Token, und wenn es nicht `acquireTokenSilent` abgelaufen ist, gibt sie es zurück.</span><span class="sxs-lookup"><span data-stu-id="100c2-147">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="100c2-148">Wenn sie abgelaufen ist, wird eine automatische Anforderung zum Abrufen einer neuen Anforderung gestellt.</span><span class="sxs-lookup"><span data-stu-id="100c2-148">If it is expired, it makes a silent request to obtain a new one.</span></span>
