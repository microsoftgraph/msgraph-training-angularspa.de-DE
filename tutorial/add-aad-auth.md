<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="d4def-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung zur Unterstützung der Authentifizierung mit Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4def-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="d4def-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken für den Aufruf von Microsoft Graph abzurufen.</span><span class="sxs-lookup"><span data-stu-id="d4def-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="d4def-103">In diesem Schritt integrieren Sie die [Microsoft-Authentifizierungsbibliothek für eckig](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d4def-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

<span data-ttu-id="d4def-104">Erstellen Sie eine neue Datei im `./src` Verzeichnis mit `oauth.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="d4def-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="d4def-105">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus dem Anwendungs Registrierungs Portal.</span><span class="sxs-lookup"><span data-stu-id="d4def-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d4def-106">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `oauth.ts` aus der Quellcodeverwaltung auszuschließen, um versehentlich Ihre APP-ID zu vernichten.</span><span class="sxs-lookup"><span data-stu-id="d4def-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="d4def-107">Öffnen `./src/app/app.module.ts` Sie und fügen Sie `import` die folgenden Anweisungen am Anfang der Datei.</span><span class="sxs-lookup"><span data-stu-id="d4def-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

<span data-ttu-id="d4def-108">Fügen Sie dann `MsalModule` das `imports` Array innerhalb der `@NgModule` Deklaration hinzu, und INITIALISIEREN Sie es mit der APP-ID.</span><span class="sxs-lookup"><span data-stu-id="d4def-108">Then add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule,
  MsalModule.forRoot({
    clientID: OAuthSettings.appId
  })
],
```

## <a name="implement-sign-in"></a><span data-ttu-id="d4def-109">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="d4def-109">Implement sign-in</span></span>

<span data-ttu-id="d4def-110">Beginnen Sie mit der Definition `User` einer einfachen Klasse, die die Informationen über den Benutzer enthält, der von der App angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d4def-110">Start by defining a simple `User` class to hold the information about the user that the app displays.</span></span> <span data-ttu-id="d4def-111">Erstellen Sie eine neue Datei im `./src/app` Ordner mit `user.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="d4def-111">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

<span data-ttu-id="d4def-112">Erstellen Sie jetzt einen Authentifizierungsdienst.</span><span class="sxs-lookup"><span data-stu-id="d4def-112">Now create an authentication service.</span></span> <span data-ttu-id="d4def-113">Wenn Sie einen Dienst dafür erstellen, können Sie ihn auf einfache Weise in alle Komponenten einfügen, die Zugriff auf Authentifizierungsmethoden benötigen.</span><span class="sxs-lookup"><span data-stu-id="d4def-113">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span> <span data-ttu-id="d4def-114">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="d4def-114">Run the following command in your CLI.</span></span>

```Shell
ng generate service auth
```

<span data-ttu-id="d4def-115">Sobald der Befehl abgeschlossen ist, öffnen `./src/app/auth.service.ts` Sie die Datei, und ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="d4def-115">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

```TypeScript
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
    let result = await this.msalService.loginPopup(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Login failed', JSON.stringify(reason, null, 2));
      });

    if (result) {
      this.authenticated = true;
      // Temporary placeholder
      this.user = new User();
      this.user.displayName = "Adele Vance";
      this.user.email = "AdeleV@contoso.com";
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
    let result = await this.msalService.acquireTokenSilent(OAuthSettings.scopes)
      .catch((reason) => {
        this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
      });

    // Temporary to display token in an error box
    if (result) this.alertsService.add('Token acquired', result);
    return result;
  }
}
```

<span data-ttu-id="d4def-116">Nachdem Sie nun über den Authentifizierungsdienst verfügen, können Sie ihn in die Komponenten einschleusen, die sich anmelden.</span><span class="sxs-lookup"><span data-stu-id="d4def-116">Now that you have the authentication service, it can be injected into the components that do sign-in.</span></span> <span data-ttu-id="d4def-117">Beginnen Sie mit `NavBarComponent`der.</span><span class="sxs-lookup"><span data-stu-id="d4def-117">Start with the `NavBarComponent`.</span></span> <span data-ttu-id="d4def-118">Öffnen Sie `./src/app/nav-bar/nav-bar.component.ts` die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="d4def-118">Open the `./src/app/nav-bar/nav-bar.component.ts` file and make the following changes.</span></span>

- <span data-ttu-id="d4def-119">Am `import { AuthService } from '../auth.service';` Anfang der Datei hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d4def-119">Add `import { AuthService } from '../auth.service';` to the top of the file.</span></span>
- <span data-ttu-id="d4def-120">Entfernen Sie `authenticated` die `user` Eigenschaften und aus der Klasse, und entfernen Sie den Code, der `ngOnInit`Sie einstellt.</span><span class="sxs-lookup"><span data-stu-id="d4def-120">Remove the `authenticated` and `user` properties from the class, and remove the code that sets them in `ngOnInit`.</span></span>
- <span data-ttu-id="d4def-121">`AuthService` Fügen Sie den folgenden Parameter in das ein `constructor`: `private authService: AuthService`.</span><span class="sxs-lookup"><span data-stu-id="d4def-121">Inject the `AuthService` by adding the following parameter to the `constructor`: `private authService: AuthService`.</span></span>
- <span data-ttu-id="d4def-122">Ersetzen Sie die `signIn` vorhandene Methode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d4def-122">Replace the existing `signIn` method with the following:</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- <span data-ttu-id="d4def-123">Ersetzen Sie die `signOut` vorhandene Methode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="d4def-123">Replace the existing `signOut` method with the following:</span></span>

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

<span data-ttu-id="d4def-124">Wenn Sie fertig sind, sollte der Code wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="d4def-124">When you're done, the code should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

import { AuthService } from '../auth.service';

@Component({
  selector: 'app-nav-bar',
  templateUrl: './nav-bar.component.html',
  styleUrls: ['./nav-bar.component.css']
})
export class NavBarComponent implements OnInit {

  // Should the collapsed nav show?
  showNav: boolean;

  constructor(private authService: AuthService) { }

  ngOnInit() {
    this.showNav = false;
  }

  // Used by the Bootstrap navbar-toggler button to hide/show
  // the nav in a collapsed state
  toggleNavBar(): void {
    this.showNav = !this.showNav;
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();
  }

  signOut(): void {
    this.authService.signOut();
  }
}
```

<span data-ttu-id="d4def-125">Da Sie die `authenticated` Eigenschaften und `user` für die Klasse entfernt haben, müssen Sie die `./src/app/nav-bar/nav-bar.component.html` Datei auch aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d4def-125">Since you removed the `authenticated` and `user` properties on the class, you also need to update the `./src/app/nav-bar/nav-bar.component.html` file.</span></span> <span data-ttu-id="d4def-126">Öffnen Sie die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="d4def-126">Open that file and make the following changes.</span></span>

- <span data-ttu-id="d4def-127">Ersetzen Sie alle Instanzen von `authenticated` durch `authService.authenticated`.</span><span class="sxs-lookup"><span data-stu-id="d4def-127">Replace all instances of `authenticated` with `authService.authenticated`.</span></span>
- <span data-ttu-id="d4def-128">Ersetzen Sie alle Instanz `user` von `authService.user`with.</span><span class="sxs-lookup"><span data-stu-id="d4def-128">Replace all instance of `user` with `authService.user`.</span></span>

<span data-ttu-id="d4def-129">Aktualisieren Sie als `HomeComponent` nächstes die Klasse.</span><span class="sxs-lookup"><span data-stu-id="d4def-129">Next update the `HomeComponent` class.</span></span> <span data-ttu-id="d4def-130">Nehmen Sie alle Änderungen an der `./src/app/home/home.component.ts` `NavBarComponent` Klasse vor, die mit den folgenden Ausnahmen vorgenommen wurden.</span><span class="sxs-lookup"><span data-stu-id="d4def-130">Make all of the same changes in `./src/app/home/home.component.ts` that you made to the `NavBarComponent` class with the following exceptions.</span></span>

- <span data-ttu-id="d4def-131">Es gibt keine `signOut` Methode in der `HomeComponent` Klasse.</span><span class="sxs-lookup"><span data-stu-id="d4def-131">There is no `signOut` method in the `HomeComponent` class.</span></span>
- <span data-ttu-id="d4def-132">Ersetzen Sie `signIn` die Methode durch eine etwas andere Version.</span><span class="sxs-lookup"><span data-stu-id="d4def-132">Replace the `signIn` method with a slightly different version.</span></span> <span data-ttu-id="d4def-133">Dieser Code ruft `getAccessToken` ein Zugriffstoken ab, das das Token für jetzt als Fehler ausgeben wird.</span><span class="sxs-lookup"><span data-stu-id="d4def-133">This code calls `getAccessToken` to get an access token, which, for now, will output the token as an error.</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

<span data-ttu-id="d4def-134">Wenn Sie fertig sind, sollte die Datei wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="d4def-134">When your done, the file should look like the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AuthService } from '../auth.service';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor(private authService: AuthService) { }

  ngOnInit() {
  }

  async signIn(): Promise<void> {
    await this.authService.signIn();

    // Temporary to display the token
    if (this.authService.authenticated) {
      let token = await this.authService.getAccessToken();
    }
  }
}
```

<span data-ttu-id="d4def-135">Führen Sie schließlich die gleichen Ersetzungen aus `./src/app/home/home.component.html` , die Sie für die Navigationsleiste vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="d4def-135">Finally, make the same replacements in `./src/app/home/home.component.html` that you made for the nav bar.</span></span>

<span data-ttu-id="d4def-136">Speichern Sie die Änderungen, und aktualisieren Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="d4def-136">Save your changes and refresh the browser.</span></span> <span data-ttu-id="d4def-137">Klicken Sie auf die Schaltfläche **Klicken Sie hier, um** sich anzumelden `https://login.microsoftonline.com`, und Sie sollten zu umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="d4def-137">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="d4def-138">Melden Sie sich mit Ihrem Microsoft-Konto an, und stimmen Sie den erforderlichen Berechtigungen zu.</span><span class="sxs-lookup"><span data-stu-id="d4def-138">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="d4def-139">Die APP-Seite sollte mit dem Token aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d4def-139">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="d4def-140">Benutzer Details abrufen</span><span class="sxs-lookup"><span data-stu-id="d4def-140">Get user details</span></span>

<span data-ttu-id="d4def-141">Im Moment stellt der Authentifizierungsdienst Konstante Werte für den Anzeigenamen und die e-Mail-Adresse des Benutzers ein.</span><span class="sxs-lookup"><span data-stu-id="d4def-141">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="d4def-142">Nachdem Sie nun über ein Zugriffstoken verfügen, können Sie Benutzer Details aus Microsoft Graph abrufen, damit diese Werte dem aktuellen Benutzer entsprechen.</span><span class="sxs-lookup"><span data-stu-id="d4def-142">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span> <span data-ttu-id="d4def-143">Öffnen `./src/app/auth.service.ts` Sie und fügen Sie `import` die folgende Anweisung am Anfang der Datei.</span><span class="sxs-lookup"><span data-stu-id="d4def-143">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

<span data-ttu-id="d4def-144">Fügen Sie eine neue Funktion zur `AuthService`-Klasse mit dem Namen `getUser` hinzu.</span><span class="sxs-lookup"><span data-stu-id="d4def-144">Add a new function to the `AuthService` class called `getUser`.</span></span>

```TypeScript
private async getUser(): Promise<User> {
  if (!this.authenticated) return null;

  let graphClient = Client.init({
    // Initialize the Graph client with an auth
    // provider that requests the token from the
    // auth service
    authProvider: async(done) => {
      let token = await this.getAccessToken()
        .catch((reason) => {
          done(reason, null);
        });

      if (token)
      {
        done(null, token);
      } else {
        done("Could not get an access token", null);
      }
    }
  });

  // Get the user from Graph (GET /me)
  let graphUser = await graphClient.api('/me').get();

  let user = new User();
  user.displayName = graphUser.displayName;
  // Prefer the mail property, but fall back to userPrincipalName
  user.email = graphUser.mail || graphUser.userPrincipalName;

  return user;
}
```

<span data-ttu-id="d4def-145">Suchen und entfernen Sie den folgenden Code aus `signIn` der-Methode.</span><span class="sxs-lookup"><span data-stu-id="d4def-145">Locate and remove the following code from the `signIn` method.</span></span>

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

<span data-ttu-id="d4def-146">Fügen Sie an seiner Stelle den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="d4def-146">In its place, add the following code.</span></span>

```TypeScript
this.user = await this.getUser();
```

<span data-ttu-id="d4def-147">Dieser neue Code verwendet das Microsoft Graph-SDK, um die Details des Benutzers abzurufen, und `User` erstellt dann mithilfe der vom API-Aufruf zurückgegebenen Werte ein Objekt.</span><span class="sxs-lookup"><span data-stu-id="d4def-147">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

<span data-ttu-id="d4def-148">Ändern Sie nun `constructor` die for `AuthService` -Klasse, um zu überprüfen, ob der Benutzer bereits angemeldet ist, und laden Sie die Details, wenn dies der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="d4def-148">Now change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="d4def-149">Ersetzen Sie das `constructor` vorhandene durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d4def-149">Replace the existing `constructor` with the following.</span></span>

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

<span data-ttu-id="d4def-150">Entfernen Sie schließlich den temporären Code aus der `HomeComponent` -Klasse.</span><span class="sxs-lookup"><span data-stu-id="d4def-150">Finally, remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="d4def-151">Öffnen Sie `./src/app/home/home.component.ts` die Datei, und ersetzen `signIn` Sie die vorhandene Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="d4def-151">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

<span data-ttu-id="d4def-152">Wenn Sie nun Ihre Änderungen speichern und die app starten, sollten Sie nach der Anmeldung wieder auf der Homepage enden, aber die Benutzeroberfläche sollte sich ändern, um anzugeben, dass Sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="d4def-152">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Screenshot der Startseite nach der Anmeldung](./images/add-aad-auth-01.png)

<span data-ttu-id="d4def-154">Klicken Sie auf den Benutzer Avatar in der oberen rechten Ecke, \*\*\*\* um auf den Link abmelden zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="d4def-154">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="d4def-155">Wenn \*\*\*\* Sie auf Abmelden klicken, wird die Sitzung zurückgesetzt, und Sie kehren zur Startseite.</span><span class="sxs-lookup"><span data-stu-id="d4def-155">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Screenshot des Dropdownmenüs mit dem Link "abMelden"](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="d4def-157">Speichern und Aktualisieren von Token</span><span class="sxs-lookup"><span data-stu-id="d4def-157">Storing and refreshing tokens</span></span>

<span data-ttu-id="d4def-158">Zu diesem Zeitpunkt verfügt Ihre Anwendung über ein Zugriffstoken, das in der `Authorization` Kopfzeile von API-aufrufen gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="d4def-158">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="d4def-159">Dies ist das Token, mit dem die APP auf Microsoft Graph im Namen des Benutzers zugreifen kann.</span><span class="sxs-lookup"><span data-stu-id="d4def-159">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="d4def-160">Dieses Token ist jedoch kurzlebig.</span><span class="sxs-lookup"><span data-stu-id="d4def-160">However, this token is short-lived.</span></span> <span data-ttu-id="d4def-161">Das Token läuft eine Stunde nach der Ausgabe ab.</span><span class="sxs-lookup"><span data-stu-id="d4def-161">The token expires an hour after it is issued.</span></span> <span data-ttu-id="d4def-162">Da die APP die MSAL-Bibliothek verwendet, müssen Sie keine Tokenspeicher-oder Aktualisierungslogik implementieren.</span><span class="sxs-lookup"><span data-stu-id="d4def-162">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="d4def-163">Das `MsalService` Token wird im Browser Speicher zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="d4def-163">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="d4def-164">Die `acquireTokenSilent` -Methode überprüft zuerst das zwischengespeicherte Token, und wenn es nicht abgelaufen ist, wird es zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="d4def-164">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="d4def-165">Wenn es abgelaufen ist, wird eine automatische Anforderung zum Abrufen einer neuen.</span><span class="sxs-lookup"><span data-stu-id="d4def-165">If it is expired, it makes a silent request to obtain a new one.</span></span>