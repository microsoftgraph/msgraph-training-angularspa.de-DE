<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f4e4a-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="f4e4a-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="f4e4a-103">In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek für eckig](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

1. <span data-ttu-id="f4e4a-104">Erstellen Sie eine neue Datei im Verzeichnis **./src** mit dem Namen **OAuth. TS** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-104">Create a new file in the **./src** directory named **oauth.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    <span data-ttu-id="f4e4a-105">Ersetzen Sie `YOUR_APP_ID_HERE` durch die Anwendungs-ID aus dem Anwendungs Registrierungs Portal.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f4e4a-106">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei **OAuth. TS** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-106">If you're using source control such as git, now would be a good time to exclude the **oauth.ts** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="f4e4a-107">Öffnen Sie **/src/App/app.Module.TS** , und fügen Sie die folgenden `import` Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-107">Open **./src/app/app.module.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. <span data-ttu-id="f4e4a-108">Fügen Sie das dem- `MsalModule` `imports` Array innerhalb der `@NgModule` Deklaration hinzu, und initialisieren Sie es mit der APP-ID.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-108">Add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports" highlight="6-11":::

## <a name="implement-sign-in"></a><span data-ttu-id="f4e4a-109">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="f4e4a-109">Implement sign-in</span></span>

<span data-ttu-id="f4e4a-110">In diesem Abschnitt erstellen Sie einen Authentifizierungsdienst und implementieren die Anmeldung und Abmeldung.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-110">In this section you'll create an authentication service and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="f4e4a-111">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-111">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service auth
    ```

    <span data-ttu-id="f4e4a-112">Wenn Sie einen Dienst dafür erstellen, können Sie ihn einfach in alle Komponenten einfügen, die Zugriff auf Authentifizierungsmethoden benötigen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-112">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span>

1. <span data-ttu-id="f4e4a-113">Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/auth.Service.TS** , und ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-113">Once the command finishes, open **./src/app/auth.service.ts** and replace its contents with the following code.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.addError('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
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
        return null;
      }
    }
    ```

1. <span data-ttu-id="f4e4a-114">Öffnen Sie **./src/App/NAV-Bar/NAV-Bar.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-114">Open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. <span data-ttu-id="f4e4a-115">Öffnen Sie **./src/App/Home/Home.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-115">Open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-32":::

<span data-ttu-id="f4e4a-116">Speichern Sie Ihre Änderungen, und aktualisieren Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-116">Save your changes and refresh the browser.</span></span> <span data-ttu-id="f4e4a-117">Klicken Sie auf die Schaltfläche **Klicken Sie hier, um sich anzumelden** , und Sie sollten zu umgeleitet werden `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="f4e4a-117">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="f4e4a-118">Melden Sie sich mit Ihrem Microsoft-Konto an, und stimmen Sie den angeforderten Berechtigungen zu.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-118">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="f4e4a-119">Die APP-Seite sollte aktualisiert werden, wobei das Token angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-119">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="f4e4a-120">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="f4e4a-120">Get user details</span></span>

<span data-ttu-id="f4e4a-121">Der Authentifizierungsdienst legt im Moment Konstante Werte für den Anzeigenamen und die e-Mail-Adresse des Benutzers fest.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-121">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="f4e4a-122">Da Sie nun über ein Zugriffstoken verfügen, können Sie Benutzer Details aus Microsoft Graph abrufen, damit diese Werte dem aktuellen Benutzer entsprechen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-122">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span>

1. <span data-ttu-id="f4e4a-123">Öffnen Sie **/src/App/auth.Service.TS** , und fügen Sie die folgenden `import` Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-123">Open **./src/app/auth.service.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. <span data-ttu-id="f4e4a-124">Fügen Sie eine neue Funktion zur `AuthService`-Klasse mit dem Namen `getUser` hinzu.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-124">Add a new function to the `AuthService` class called `getUser`.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. <span data-ttu-id="f4e4a-125">Suchen und entfernen Sie den folgenden Code in der `getAccessToken` -Methode, die eine Warnung hinzufügt, um das Zugriffstoken anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-125">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. <span data-ttu-id="f4e4a-126">Suchen Sie den folgenden Code aus der-Methode, und entfernen Sie ihn `signIn` .</span><span class="sxs-lookup"><span data-stu-id="f4e4a-126">Locate and remove the following code from the `signIn` method.</span></span>

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. <span data-ttu-id="f4e4a-127">Fügen Sie an seiner Stelle den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-127">In its place, add the following code.</span></span>

    ```typescript
    this.user = await this.getUser();
    ```

    <span data-ttu-id="f4e4a-128">In diesem neuen Code wird das Microsoft Graph-SDK verwendet, um die Details des Benutzers abzurufen, und anschließend wird ein `User` Objekt mithilfe der vom API-Aufruf zurückgegebenen Werte erstellt.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-128">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

1. <span data-ttu-id="f4e4a-129">Ändern `constructor` Sie die für die `AuthService` -Klasse, um zu überprüfen, ob der Benutzer bereits angemeldet ist, und laden Sie die Details, wenn dies der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-129">Change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="f4e4a-130">Ersetzen Sie das vorhandene `constructor` durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-130">Replace the existing `constructor` with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. <span data-ttu-id="f4e4a-131">Entfernen Sie den temporären Code aus der `HomeComponent` -Klasse.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-131">Remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="f4e4a-132">Öffnen Sie **./src/App/Home/Home.Component.TS** , und ersetzen Sie die vorhandene `signIn` Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-132">Open **./src/app/home/home.component.ts** and replace the existing `signIn` function with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet":::

<span data-ttu-id="f4e4a-133">Wenn Sie nun Ihre Änderungen speichern und die app starten, sollten Sie nach der Anmeldung wieder auf der Startseite enden, aber die Benutzeroberfläche sollte sich ändern, um anzugeben, dass Sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-133">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Screenshot der Startseite nach dem Anmelden](./images/add-aad-auth-01.png)

<span data-ttu-id="f4e4a-135">Klicken Sie in der oberen rechten Ecke auf den Avatar des Benutzers, um auf den **Abmelde** Link zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-135">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="f4e4a-136">Wenn Sie auf **Abmelden** klicken, wird die Sitzung zurückgesetzt und Sie kehren zur Startseite zurück.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-136">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Screenshot des Dropdown-Menüs mit dem Link „Abmelden“](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="f4e4a-138">Speichern und Aktualisieren von Token</span><span class="sxs-lookup"><span data-stu-id="f4e4a-138">Storing and refreshing tokens</span></span>

<span data-ttu-id="f4e4a-139">Zu diesem Zeitpunkt verfügt Ihre Anwendung über ein Zugriffstoken, das in der `Authorization` Kopfzeile von API-aufrufen gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-139">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="f4e4a-140">Dies ist das Token, das es der App ermöglicht, im Namen des Benutzers auf Microsoft Graph zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-140">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="f4e4a-141">Dieses Token ist jedoch nur kurzzeitig verfügbar.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-141">However, this token is short-lived.</span></span> <span data-ttu-id="f4e4a-142">Das Token läuft eine Stunde nach seiner Ausgabe ab.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-142">The token expires an hour after it is issued.</span></span> <span data-ttu-id="f4e4a-143">Da die APP die MSAL-Bibliothek verwendet, müssen Sie keine Token-Speicher-oder Aktualisierungslogik implementieren.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-143">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="f4e4a-144">Das `MsalService` Token wird im Browser Speicher zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-144">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="f4e4a-145">Die `acquireTokenSilent` Methode überprüft zuerst das zwischengespeicherte Token, und wenn es nicht abgelaufen ist, wird es zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-145">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="f4e4a-146">Wenn es abgelaufen ist, wird eine automatische Anforderung an einen neuen abgerufen.</span><span class="sxs-lookup"><span data-stu-id="f4e4a-146">If it is expired, it makes a silent request to obtain a new one.</span></span>
