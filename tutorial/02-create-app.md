<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b590e-101">In diesem Abschnitt erstellen Sie ein neues Angular Projekt.</span><span class="sxs-lookup"><span data-stu-id="b590e-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="b590e-102">Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie über die Rechte zum Erstellen von Dateien verfügen, und führen Sie die folgenden Befehle aus, um das [Angular](https://www.npmjs.com/package/@angular/cli) CLI-Tool zu installieren und eine neue Angular zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b590e-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. <span data-ttu-id="b590e-103">Die Angular CLI fordert weitere Informationen an.</span><span class="sxs-lookup"><span data-stu-id="b590e-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="b590e-104">Beantworten Sie die Eingabeaufforderungen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="b590e-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="b590e-105">Wechseln Sie nach Abschluss des Befehls in das Verzeichnis in Der CLI, und führen Sie den folgenden Befehl aus, um einen `graph-tutorial` lokalen Webserver zu starten.</span><span class="sxs-lookup"><span data-stu-id="b590e-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="b590e-106">Ihr Standardbrowser wird mit [https://localhost:4200/](https://localhost:4200) einer Standardseite Angular geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b590e-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="b590e-107">Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, und navigieren Sie zu, um zu überprüfen, ob [https://localhost:4200/](https://localhost:4200) die neue App funktioniert.</span><span class="sxs-lookup"><span data-stu-id="b590e-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="b590e-108">Hinzufügen von Knotenpaketen</span><span class="sxs-lookup"><span data-stu-id="b590e-108">Add Node packages</span></span>

<span data-ttu-id="b590e-109">Installieren Sie vor dem Wechsel einige zusätzliche Pakete, die Sie später verwenden werden:</span><span class="sxs-lookup"><span data-stu-id="b590e-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="b590e-110">[bootstrap](https://github.com/twbs/bootstrap) für Formatieren und allgemeine Komponenten.</span><span class="sxs-lookup"><span data-stu-id="b590e-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="b590e-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten Angular.</span><span class="sxs-lookup"><span data-stu-id="b590e-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="b590e-112">[Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums- und Zeitangaben.</span><span class="sxs-lookup"><span data-stu-id="b590e-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="b590e-113">windows-iana</span><span class="sxs-lookup"><span data-stu-id="b590e-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="b590e-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung Azure Active Directory und Abrufen von Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="b590e-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="b590e-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum Aufrufen von Microsoft-Graph.</span><span class="sxs-lookup"><span data-stu-id="b590e-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="b590e-116">Führen Sie die folgenden Befehle in Ihrer CLI aus.</span><span class="sxs-lookup"><span data-stu-id="b590e-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.2
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. <span data-ttu-id="b590e-117">Führen Sie den folgenden Befehl in Ihrer CLI aus, um das Angular (erforderlich für ng-bootstrap) hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="b590e-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="b590e-118">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="b590e-118">Design the app</span></span>

<span data-ttu-id="b590e-119">In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die App.</span><span class="sxs-lookup"><span data-stu-id="b590e-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="b590e-120">Öffnen **Sie ./src/styles.css,** und fügen Sie die folgenden Zeilen hinzu.</span><span class="sxs-lookup"><span data-stu-id="b590e-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="b590e-121">Fügen Sie der App das Bootstrap-Modul hinzu.</span><span class="sxs-lookup"><span data-stu-id="b590e-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="b590e-122">Öffnen **Sie ./src/app/app.module.ts,** und ersetzen Sie dessen Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { BrowserModule } from '@angular/platform-browser';
    import { FormsModule } from '@angular/forms';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';

    @NgModule({
      declarations: [
        AppComponent
      ],
      imports: [
        BrowserModule,
        FormsModule,
        AppRoutingModule,
        NgbModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule { }
    ```

1. <span data-ttu-id="b590e-123">Erstellen Sie eine neue Datei im **Ordner ./src/app** mit dem Namen **user.ts,** und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="b590e-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. <span data-ttu-id="b590e-124">Generieren Sie Angular komponente für die oberste Navigation auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="b590e-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="b590e-125">Führen Sie in Ihrer CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="b590e-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="b590e-126">Öffnen Sie nach Abschluss des Befehls **./src/app/nav-bar/nav-bar.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean = false;
      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      // Used by the Bootstrap navbar-toggler button to hide/show
      // the nav in a collapsed state
      toggleNavBar(): void {
        this.showNav = !this.showNav;
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = undefined;
      }
    }
    ```

1. <span data-ttu-id="b590e-127">Öffnen **Sie ./src/app/nav-bar/nav-bar.component.html,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="b590e-128">Erstellen Sie eine Homepage für die App.</span><span class="sxs-lookup"><span data-stu-id="b590e-128">Create a home page for the app.</span></span> <span data-ttu-id="b590e-129">Führen Sie den folgenden Befehl in Ihrer CLI aus.</span><span class="sxs-lookup"><span data-stu-id="b590e-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="b590e-130">Öffnen Sie nach Abschluss des Befehls **./src/app/home/home.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean = false;
      // The user
      user?: User = undefined;

      constructor() { }

      ngOnInit() { }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com',
          avatar: '',
          timeZone: ''
        };
      }
    }
    ```

1. <span data-ttu-id="b590e-131">Öffnen **Sie ./src/app/home/home.component.html,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="b590e-132">Erstellen Sie eine einfache `Alert` Klasse.</span><span class="sxs-lookup"><span data-stu-id="b590e-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="b590e-133">Erstellen Sie eine neue Datei im **Verzeichnis ./src/app** mit dem Namen **alert.ts,** und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="b590e-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. <span data-ttu-id="b590e-134">Erstellen Sie einen Benachrichtigungsdienst, den die App zum Anzeigen von Nachrichten für den Benutzer verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="b590e-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="b590e-135">Führen Sie in Ihrer CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="b590e-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="b590e-136">Öffnen **Sie ./src/app/alerts.service.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="b590e-137">Generieren Sie eine Warnungskomponente, um Warnungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b590e-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="b590e-138">Führen Sie in Ihrer CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="b590e-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="b590e-139">Öffnen Sie nach Abschluss des Befehls **./src/app/alerts/alerts.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. <span data-ttu-id="b590e-140">Öffnen **Sie ./src/app/alerts/alerts.component.html,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. <span data-ttu-id="b590e-141">Öffnen **Sie ./src/app/app-routing.module.ts,** und ersetzen Sie die `const routes: Routes = [];` Zeile durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b590e-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="b590e-142">Öffnen **Sie ./src/app/app.component.html,** und ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="b590e-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. <span data-ttu-id="b590e-143">Fügen Sie eine Bilddatei Ihrer Wahl namens **no-profile-photo.png** **im Verzeichnis ./src/assets** hinzu.</span><span class="sxs-lookup"><span data-stu-id="b590e-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="b590e-144">Dieses Bild wird als Foto des Benutzers verwendet, wenn der Benutzer kein Foto in Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="b590e-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="b590e-145">Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="b590e-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="b590e-146">Jetzt sollte die App ganz anders aussehen.</span><span class="sxs-lookup"><span data-stu-id="b590e-146">Now, the app should look very different.</span></span>

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)
