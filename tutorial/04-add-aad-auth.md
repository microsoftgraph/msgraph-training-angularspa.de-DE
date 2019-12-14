<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ffeab-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="ffeab-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ffeab-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="ffeab-103">In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek für eckig](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="ffeab-103">In this step you will integrate the [Microsoft Authentication Library for Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) into the application.</span></span>

<span data-ttu-id="ffeab-104">Erstellen Sie eine neue Datei im `./src` Verzeichnis mit `oauth.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-104">Create a new file in the `./src` directory named `oauth.ts` and add the following code.</span></span>

```TypeScript
export const OAuthSettings = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="ffeab-105">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus dem Anwendungs Registrierungs Portal.</span><span class="sxs-lookup"><span data-stu-id="ffeab-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ffeab-106">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `oauth.ts` aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ffeab-106">If you're using source control such as git, now would be a good time to exclude the `oauth.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="ffeab-107">Öffnen `./src/app/app.module.ts` Sie und fügen Sie `import` die folgenden Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-107">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { MsalModule } from '@azure/msal-angular';
import { OAuthSettings } from '../oauth';
```

<span data-ttu-id="ffeab-108">Fügen Sie dann `MsalModule` das dem `imports` Array innerhalb der `@NgModule` Deklaration hinzu, und initialisieren Sie es mit der APP-ID.</span><span class="sxs-lookup"><span data-stu-id="ffeab-108">Then add the `MsalModule` to the `imports` array inside the `@NgModule` declaration, and initialize it with the app ID.</span></span>

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

## <a name="implement-sign-in"></a><span data-ttu-id="ffeab-109">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="ffeab-109">Implement sign-in</span></span>

<span data-ttu-id="ffeab-110">Definieren Sie zunächst eine einfache `User` Klasse, die die Informationen zu dem Benutzer enthält, der von der App angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ffeab-110">Start by defining a simple `User` class to hold the information about the user that the app displays.</span></span> <span data-ttu-id="ffeab-111">Erstellen Sie eine neue Datei im `./src/app` Ordner mit `user.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-111">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

```TypeScript
export class User {
  displayName: string;
  email: string;
  avatar: string;
}
```

<span data-ttu-id="ffeab-112">Erstellen Sie nun einen Authentifizierungsdienst.</span><span class="sxs-lookup"><span data-stu-id="ffeab-112">Now create an authentication service.</span></span> <span data-ttu-id="ffeab-113">Wenn Sie einen Dienst dafür erstellen, können Sie ihn einfach in alle Komponenten einfügen, die Zugriff auf Authentifizierungsmethoden benötigen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-113">By creating a service for this, you can easily inject it into any components that need access to authentication methods.</span></span> <span data-ttu-id="ffeab-114">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="ffeab-114">Run the following command in your CLI.</span></span>

```Shell
ng generate service auth
```

<span data-ttu-id="ffeab-115">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/auth.service.ts` Sie die Datei, und ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="ffeab-115">Once the command finishes, open the `./src/app/auth.service.ts` file and replace its contents with the following code.</span></span>

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

<span data-ttu-id="ffeab-116">Nachdem Sie nun über den Authentifizierungsdienst verfügen, kann er in die Komponenten injiziert werden, die sich anmelden.</span><span class="sxs-lookup"><span data-stu-id="ffeab-116">Now that you have the authentication service, it can be injected into the components that do sign-in.</span></span> <span data-ttu-id="ffeab-117">Beginnen Sie mit `NavBarComponent`dem.</span><span class="sxs-lookup"><span data-stu-id="ffeab-117">Start with the `NavBarComponent`.</span></span> <span data-ttu-id="ffeab-118">Öffnen Sie `./src/app/nav-bar/nav-bar.component.ts` die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="ffeab-118">Open the `./src/app/nav-bar/nav-bar.component.ts` file and make the following changes.</span></span>

- <span data-ttu-id="ffeab-119">Fügen `import { AuthService } from '../auth.service';` Sie am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-119">Add `import { AuthService } from '../auth.service';` to the top of the file.</span></span>
- <span data-ttu-id="ffeab-120">Entfernen Sie `authenticated` die `user` Eigenschaften and aus der Klasse, und entfernen Sie den Code, in `ngOnInit`dem Sie festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="ffeab-120">Remove the `authenticated` and `user` properties from the class, and remove the code that sets them in `ngOnInit`.</span></span>
- <span data-ttu-id="ffeab-121">Injizieren Sie `AuthService` den, indem Sie den folgenden Parameter `constructor`hinzu `private authService: AuthService`fügen:.</span><span class="sxs-lookup"><span data-stu-id="ffeab-121">Inject the `AuthService` by adding the following parameter to the `constructor`: `private authService: AuthService`.</span></span>
- <span data-ttu-id="ffeab-122">Ersetzen Sie zunächst die vorhandene `signIn`-Methode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ffeab-122">Replace the existing `signIn` method with the following:</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();
    }
    ```

- <span data-ttu-id="ffeab-123">Ersetzen Sie zunächst die vorhandene `signOut`-Methode durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ffeab-123">Replace the existing `signOut` method with the following:</span></span>

    ```TypeScript
    signOut(): void {
      this.authService.signOut();
    }
    ```

<span data-ttu-id="ffeab-124">Wenn Sie fertig sind, sollte der Code wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-124">When you're done, the code should look like the following.</span></span>

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

<span data-ttu-id="ffeab-125">Da Sie die `authenticated` Eigenschaften und `user` für die Klasse entfernt haben, müssen Sie die `./src/app/nav-bar/nav-bar.component.html` Datei auch aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ffeab-125">Since you removed the `authenticated` and `user` properties on the class, you also need to update the `./src/app/nav-bar/nav-bar.component.html` file.</span></span> <span data-ttu-id="ffeab-126">Öffnen Sie die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="ffeab-126">Open that file and make the following changes.</span></span>

- <span data-ttu-id="ffeab-127">Ersetzen Sie alle Instanzen von `authenticated` durch `authService.authenticated`.</span><span class="sxs-lookup"><span data-stu-id="ffeab-127">Replace all instances of `authenticated` with `authService.authenticated`.</span></span>
- <span data-ttu-id="ffeab-128">Ersetzen Sie alle Instanzen `user` von `authService.user`mit.</span><span class="sxs-lookup"><span data-stu-id="ffeab-128">Replace all instance of `user` with `authService.user`.</span></span>

<span data-ttu-id="ffeab-129">Aktualisieren Sie als `HomeComponent` nächstes die Klasse.</span><span class="sxs-lookup"><span data-stu-id="ffeab-129">Next update the `HomeComponent` class.</span></span> <span data-ttu-id="ffeab-130">Nehmen Sie dieselben Änderungen an der `./src/app/home/home.component.ts` `NavBarComponent` Klasse vor, die Sie mit den folgenden Ausnahmen vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="ffeab-130">Make all of the same changes in `./src/app/home/home.component.ts` that you made to the `NavBarComponent` class with the following exceptions.</span></span>

- <span data-ttu-id="ffeab-131">Es gibt keine `signOut` Methode in der `HomeComponent` -Klasse.</span><span class="sxs-lookup"><span data-stu-id="ffeab-131">There is no `signOut` method in the `HomeComponent` class.</span></span>
- <span data-ttu-id="ffeab-132">Ersetzen Sie `signIn` die-Methode durch eine etwas andere Version.</span><span class="sxs-lookup"><span data-stu-id="ffeab-132">Replace the `signIn` method with a slightly different version.</span></span> <span data-ttu-id="ffeab-133">Dieser Code ruft `getAccessToken` zum Abrufen eines Zugriffstokens auf, das für den Moment das Token als Fehler ausgeben wird.</span><span class="sxs-lookup"><span data-stu-id="ffeab-133">This code calls `getAccessToken` to get an access token, which, for now, will output the token as an error.</span></span>

    ```TypeScript
    async signIn(): Promise<void> {
      await this.authService.signIn();

      // Temporary to display the token
      if (this.authService.authenticated) {
        let token = await this.authService.getAccessToken();
      }
    }
    ```

<span data-ttu-id="ffeab-134">Wenn Sie fertig sind, sollte die Datei wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-134">When your done, the file should look like the following.</span></span>

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

<span data-ttu-id="ffeab-135">Erstellen Sie schließlich dieselben ersetzen `./src/app/home/home.component.html` , die Sie für die Navigationsleiste vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="ffeab-135">Finally, make the same replacements in `./src/app/home/home.component.html` that you made for the nav bar.</span></span>

<span data-ttu-id="ffeab-136">Speichern Sie Ihre Änderungen, und aktualisieren Sie den Browser.</span><span class="sxs-lookup"><span data-stu-id="ffeab-136">Save your changes and refresh the browser.</span></span> <span data-ttu-id="ffeab-137">Klicken Sie auf die Schaltfläche **Klicken Sie hier, um sich anzumelden** , und Sie sollten zu `https://login.microsoftonline.com`umgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="ffeab-137">Click the **Click here to sign in** button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="ffeab-138">Melden Sie sich mit Ihrem Microsoft-Konto an, und stimmen Sie den angeforderten Berechtigungen zu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-138">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="ffeab-139">Die APP-Seite sollte aktualisiert werden, wobei das Token angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ffeab-139">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="ffeab-140">Abrufen von Benutzer Details</span><span class="sxs-lookup"><span data-stu-id="ffeab-140">Get user details</span></span>

<span data-ttu-id="ffeab-141">Der Authentifizierungsdienst legt im Moment Konstante Werte für den Anzeigenamen und die e-Mail-Adresse des Benutzers fest.</span><span class="sxs-lookup"><span data-stu-id="ffeab-141">Right now the authentication service sets constant values for the user's display name and email address.</span></span> <span data-ttu-id="ffeab-142">Da Sie nun über ein Zugriffstoken verfügen, können Sie Benutzer Details aus Microsoft Graph abrufen, damit diese Werte dem aktuellen Benutzer entsprechen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-142">Now that you have an access token, you can get user details from Microsoft Graph so those values correspond to the current user.</span></span> <span data-ttu-id="ffeab-143">Öffnen `./src/app/auth.service.ts` Sie und fügen Sie `import` die folgende Anweisung am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-143">Open `./src/app/auth.service.ts` and add the following `import` statement to the top of the file.</span></span>

```TypeScript
import { Client } from '@microsoft/microsoft-graph-client';
```

<span data-ttu-id="ffeab-144">Fügen Sie eine neue Funktion zur `AuthService`-Klasse mit dem Namen `getUser` hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-144">Add a new function to the `AuthService` class called `getUser`.</span></span>

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

<span data-ttu-id="ffeab-145">Suchen und entfernen Sie den folgenden Code in `getAccessToken` der-Methode, die eine Warnung hinzufügt, um das Zugriffstoken anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-145">Locate and remove the following code in the `getAccessToken` method that adds an alert to display the access token.</span></span>

```TypeScript
// Temporary to display token in an error box
if (result) this.alertsService.add('Token acquired', result);
```

<span data-ttu-id="ffeab-146">Suchen Sie den folgenden Code aus der `signIn` -Methode, und entfernen Sie ihn.</span><span class="sxs-lookup"><span data-stu-id="ffeab-146">Locate and remove the following code from the `signIn` method.</span></span>

```TypeScript
// Temporary placeholder
this.user = new User();
this.user.displayName = "Adele Vance";
this.user.email = "AdeleV@contoso.com";
```

<span data-ttu-id="ffeab-147">Fügen Sie an seiner Stelle den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="ffeab-147">In its place, add the following code.</span></span>

```TypeScript
this.user = await this.getUser();
```

<span data-ttu-id="ffeab-148">In diesem neuen Code wird das Microsoft Graph-SDK verwendet, um die Details des Benutzers abzurufen `User` , und anschließend wird ein Objekt mithilfe der vom API-Aufruf zurückgegebenen Werte erstellt.</span><span class="sxs-lookup"><span data-stu-id="ffeab-148">This new code uses the Microsoft Graph SDK to get the user's details, then creates a `User` object using values returned by the API call.</span></span>

<span data-ttu-id="ffeab-149">Ändern Sie nun `constructor` die für `AuthService` die-Klasse, um zu überprüfen, ob der Benutzer bereits angemeldet ist, und laden Sie die Details, falls dies der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="ffeab-149">Now change the `constructor` for the `AuthService` class to check if the user is already logged in and load their details if so.</span></span> <span data-ttu-id="ffeab-150">Ersetzen Sie das `constructor` vorhandene durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ffeab-150">Replace the existing `constructor` with the following.</span></span>

```TypeScript
constructor(
  private msalService: MsalService,
  private alertsService: AlertsService) {

  this.authenticated = this.msalService.getUser() != null;
  this.getUser().then((user) => {this.user = user});
}
```

<span data-ttu-id="ffeab-151">Entfernen Sie schließlich den temporären Code aus der `HomeComponent` -Klasse.</span><span class="sxs-lookup"><span data-stu-id="ffeab-151">Finally, remove the temporary code from the `HomeComponent` class.</span></span> <span data-ttu-id="ffeab-152">Öffnen Sie `./src/app/home/home.component.ts` die Datei, und ersetzen `signIn` Sie die vorhandene Funktion durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="ffeab-152">Open the `./src/app/home/home.component.ts` file and replace the existing `signIn` function with the following.</span></span>

```TypeScript
async signIn(): Promise<void> {
  await this.authService.signIn();
}
```

<span data-ttu-id="ffeab-153">Wenn Sie nun Ihre Änderungen speichern und die app starten, sollten Sie nach der Anmeldung wieder auf der Startseite enden, aber die Benutzeroberfläche sollte sich ändern, um anzugeben, dass Sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="ffeab-153">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Ein Screenshot der Startseite nach der Anmeldung](./images/add-aad-auth-01.png)

<span data-ttu-id="ffeab-155">Klicken Sie in der oberen rechten Ecke auf den Avatar des Benutzers, um auf den **Abmelde** Link zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-155">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="ffeab-156">Durch Klicken auf **Abmelden** wird die Sitzung zurückgesetzt, und Sie kehren zur Startseite zurück.</span><span class="sxs-lookup"><span data-stu-id="ffeab-156">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Screenshot des Dropdownmenüs mit dem Link zum Abmelden](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="ffeab-158">Speichern und Aktualisieren von Token</span><span class="sxs-lookup"><span data-stu-id="ffeab-158">Storing and refreshing tokens</span></span>

<span data-ttu-id="ffeab-159">Zu diesem Zeitpunkt verfügt Ihre Anwendung über ein Zugriffstoken, das in der `Authorization` Kopfzeile von API-aufrufen gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="ffeab-159">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="ffeab-160">Dies ist das Token, das es der App ermöglicht, im Namen des Benutzers auf Microsoft Graph zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-160">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="ffeab-161">Dieses Token ist jedoch nur kurzlebig.</span><span class="sxs-lookup"><span data-stu-id="ffeab-161">However, this token is short-lived.</span></span> <span data-ttu-id="ffeab-162">Das Token läuft eine Stunde nach seiner Ausgabe ab.</span><span class="sxs-lookup"><span data-stu-id="ffeab-162">The token expires an hour after it is issued.</span></span> <span data-ttu-id="ffeab-163">Da die APP die MSAL-Bibliothek verwendet, müssen Sie keine Token-Speicher-oder Aktualisierungslogik implementieren.</span><span class="sxs-lookup"><span data-stu-id="ffeab-163">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="ffeab-164">Das `MsalService` Token wird im Browser Speicher zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="ffeab-164">The `MsalService` caches the token in the browser storage.</span></span> <span data-ttu-id="ffeab-165">Die `acquireTokenSilent` Methode überprüft zuerst das zwischengespeicherte Token, und wenn es nicht abgelaufen ist, wird es zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="ffeab-165">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="ffeab-166">Wenn es abgelaufen ist, wird eine automatische Anforderung an einen neuen abgerufen.</span><span class="sxs-lookup"><span data-stu-id="ffeab-166">If it is expired, it makes a silent request to obtain a new one.</span></span>
