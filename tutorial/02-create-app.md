<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="1bb6a-101">In diesem Abschnitt erstellen Sie ein neues schräges Projekt.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="1bb6a-102">Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie Berechtigungen zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das Tool für die [eckige CLI](https://www.npmjs.com/package/@angular/cli) zu installieren und eine neue Winkel-APP zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. <span data-ttu-id="1bb6a-103">In der eckigen CLI werden weitere Informationen angefordert.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="1bb6a-104">Beantworten Sie die Anweisungen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="1bb6a-105">Nachdem der Befehl abgeschlossen ist, wechseln Sie `graph-tutorial` in das Verzeichnis in der CLI, und führen Sie den folgenden Befehl aus, um einen lokalen Webserver zu starten.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="1bb6a-106">Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer standardmäßigen eckigen Seite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="1bb6a-107">Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, [https://localhost:4200/](https://localhost:4200) und navigieren Sie zu, um zu überprüfen, ob die neue APP funktioniert.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="1bb6a-108">Hinzufügen von Knoten Paketen</span><span class="sxs-lookup"><span data-stu-id="1bb6a-108">Add Node packages</span></span>

<span data-ttu-id="1bb6a-109">Bevor Sie fortfahren, installieren Sie einige zusätzliche Pakete, die Sie später verwenden werden:</span><span class="sxs-lookup"><span data-stu-id="1bb6a-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="1bb6a-110">[Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="1bb6a-111">[ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="1bb6a-112">[Winkel-fontawesome](https://github.com/FortAwesome/angular-fontawesome) zum Verwenden von fontawesome-Symbolen in eckig.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-112">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="1bb6a-113">[fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-Regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)und [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) für die fontawesome-Symbole, die im Beispiel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-113">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="1bb6a-114">[Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums-und Uhrzeitangaben.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-114">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="1bb6a-115">[msal-Winkel](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und zum Abrufen von Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-115">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="1bb6a-116">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum tätigen von Anrufen an Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="1bb6a-117">Führen Sie die folgenden Befehle in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-117">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. <span data-ttu-id="1bb6a-118">Führen Sie den folgenden Befehl in der CLI aus, um das eckige Lokalisierungspaket hinzuzufügen (erforderlich für ng-Bootstrap).</span><span class="sxs-lookup"><span data-stu-id="1bb6a-118">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="1bb6a-119">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="1bb6a-119">Design the app</span></span>

<span data-ttu-id="1bb6a-120">In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die app.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-120">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="1bb6a-121">Öffnen Sie `./src/styles.css` die, und fügen Sie die folgenden Zeilen hinzu.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-121">Open the `./src/styles.css` and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="1bb6a-122">Fügen Sie die Bootstrap-und FontAwesome-Module zur APP hinzu.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-122">Add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="1bb6a-123">Öffnen `./src/app/app.module.ts` Sie den Inhalt, und ersetzen Sie ihn durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-123">Open `./src/app/app.module.ts` and replace its contents with the following.</span></span>

    ```TypeScript
    import { BrowserModule } from '@angular/platform-browser';
    import { NgModule } from '@angular/core';
    import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
    import { FontAwesomeModule, FaIconLibrary } from '@fortawesome/angular-fontawesome';
    import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
    import { faUserCircle } from '@fortawesome/free-regular-svg-icons';

    import { AppRoutingModule } from './app-routing.module';
    import { AppComponent } from './app.component';
    import { NavBarComponent } from './nav-bar/nav-bar.component';
    import { HomeComponent } from './home/home.component';
    import { AlertsComponent } from './alerts/alerts.component';

    @NgModule({
      declarations: [
        AppComponent,
        NavBarComponent,
        HomeComponent,
        AlertsComponent
      ],
      imports: [
        BrowserModule,
        AppRoutingModule,
        NgbModule,
        FontAwesomeModule
      ],
      providers: [],
      bootstrap: [AppComponent]
    })
    export class AppModule {
      constructor(library: FaIconLibrary) {
        // Register the FontAwesome icons used by the app
        library.addIcons(faExternalLinkAlt, faUserCircle);
      }
     }
    ```

1. <span data-ttu-id="1bb6a-124">Erstellen Sie eine neue Datei im `./src/app` Ordner mit `user.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-124">Create a new file in the `./src/app` folder named `user.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="1bb6a-125">Erstellen Sie eine Winkel Komponente für die obere Navigationsleiste auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-125">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="1bb6a-126">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-126">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="1bb6a-127">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/nav-bar/nav-bar.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-127">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-nav-bar',
      templateUrl: './nav-bar.component.html',
      styleUrls: ['./nav-bar.component.css']
    })
    export class NavBarComponent implements OnInit {

      // Should the collapsed nav show?
      showNav: boolean;
      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: User;

      constructor() { }

      ngOnInit() {
        this.showNav = false;
        this.authenticated = false;
        this.user = null;
      }

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
          avatar: null
        };
      }

      signOut(): void {
        // Temporary
        this.authenticated = false;
        this.user = null;
      }
    }
    ```

1. <span data-ttu-id="1bb6a-128">Öffnen Sie `./src/app/nav-bar/nav-bar.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-128">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="1bb6a-129">Erstellen Sie eine Startseite für die app.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-129">Create a home page for the app.</span></span> <span data-ttu-id="1bb6a-130">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-130">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="1bb6a-131">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/home/home.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-131">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
    import { Component, OnInit } from '@angular/core';

    import { User } from '../user';

    @Component({
      selector: 'app-home',
      templateUrl: './home.component.html',
      styleUrls: ['./home.component.css']
    })
    export class HomeComponent implements OnInit {

      // Is a user logged in?
      authenticated: boolean;
      // The user
      user: any;

      constructor() { }

      ngOnInit() {
        this.authenticated = false;
        this.user = {};
      }

      signIn(): void {
        // Temporary
        this.authenticated = true;
        this.user = {
          displayName: 'Adele Vance',
          email: 'AdeleV@contoso.com'
        };
      }
    }
    ```

1. <span data-ttu-id="1bb6a-132">Öffnen Sie `./src/app/home/home.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-132">Open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="1bb6a-133">Erstellen Sie eine `Alert` einfache Klasse.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-133">Create a simple `Alert` class.</span></span> <span data-ttu-id="1bb6a-134">Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `alert.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="1bb6a-135">Erstellen Sie einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten an den Benutzer verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-135">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="1bb6a-136">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-136">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="1bb6a-137">Öffnen Sie `./src/app/alerts.service.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-137">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="1bb6a-138">Generiert eine Alerts-Komponente zum Anzeigen von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-138">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="1bb6a-139">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-139">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="1bb6a-140">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/alerts/alerts.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-140">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="1bb6a-141">Öffnen Sie `./src/app/alerts/alerts.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-141">Open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="1bb6a-142">Öffnen Sie `./src/app/app-routing.module.ts` die Datei, und `const routes: Routes = [];` ersetzen Sie die-Codezeile durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-142">Open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="1bb6a-143">Öffnen Sie die Datei `./src/app/app.component.html`, und ersetzen Sie ihren Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="1bb6a-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

<span data-ttu-id="1bb6a-144">Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="1bb6a-145">Nun sollte die APP sehr unterschiedlich aussehen.</span><span class="sxs-lookup"><span data-stu-id="1bb6a-145">Now, the app should look very different.</span></span>

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)
