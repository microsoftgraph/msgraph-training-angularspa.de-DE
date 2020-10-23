<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e3d71-101">In diesem Abschnitt erstellen Sie ein neues schräges Projekt.</span><span class="sxs-lookup"><span data-stu-id="e3d71-101">In this section, you'll create a new Angular project.</span></span>

1. <span data-ttu-id="e3d71-102">Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie Berechtigungen zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das Tool für die [eckige CLI](https://www.npmjs.com/package/@angular/cli) zu installieren und eine neue Winkel-APP zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3d71-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

    ```Shell
    npm install -g @angular/cli@10.1.7
    ng new graph-tutorial
    ```

1. <span data-ttu-id="e3d71-103">In der eckigen CLI werden weitere Informationen angefordert.</span><span class="sxs-lookup"><span data-stu-id="e3d71-103">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="e3d71-104">Beantworten Sie die Anweisungen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="e3d71-104">Answer the prompts as follows.</span></span>

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. <span data-ttu-id="e3d71-105">Nachdem der Befehl abgeschlossen ist, wechseln Sie `graph-tutorial` in das Verzeichnis in der CLI, und führen Sie den folgenden Befehl aus, um einen lokalen Webserver zu starten.</span><span class="sxs-lookup"><span data-stu-id="e3d71-105">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    ng serve --open
    ```

1. <span data-ttu-id="e3d71-106">Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer standardmäßigen eckigen Seite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="e3d71-106">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="e3d71-107">Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, und navigieren Sie zu, um [https://localhost:4200/](https://localhost:4200) zu überprüfen, ob die neue APP funktioniert.</span><span class="sxs-lookup"><span data-stu-id="e3d71-107">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="e3d71-108">Hinzufügen von Knoten Paketen</span><span class="sxs-lookup"><span data-stu-id="e3d71-108">Add Node packages</span></span>

<span data-ttu-id="e3d71-109">Bevor Sie fortfahren, installieren Sie einige zusätzliche Pakete, die Sie später verwenden werden:</span><span class="sxs-lookup"><span data-stu-id="e3d71-109">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="e3d71-110">[Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.</span><span class="sxs-lookup"><span data-stu-id="e3d71-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="e3d71-111">[ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.</span><span class="sxs-lookup"><span data-stu-id="e3d71-111">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="e3d71-112">[Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums-und Uhrzeitangaben.</span><span class="sxs-lookup"><span data-stu-id="e3d71-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- [<span data-ttu-id="e3d71-113">Windows – IANA</span><span class="sxs-lookup"><span data-stu-id="e3d71-113">windows-iana</span></span>](https://github.com/rubenillodo/windows-iana)
- <span data-ttu-id="e3d71-114">[msal-Winkel](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und zum Abrufen von Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="e3d71-114">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="e3d71-115">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum tätigen von Anrufen an Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="e3d71-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

1. <span data-ttu-id="e3d71-116">Führen Sie die folgenden Befehle in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="e3d71-116">Run the following commands in your CLI.</span></span>

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. <span data-ttu-id="e3d71-117">Führen Sie den folgenden Befehl in der CLI aus, um das eckige Lokalisierungspaket hinzuzufügen (erforderlich für ng-Bootstrap).</span><span class="sxs-lookup"><span data-stu-id="e3d71-117">Run the following command in your CLI to add the Angular localization package (required by ng-bootstrap).</span></span>

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a><span data-ttu-id="e3d71-118">Entwerfen der App</span><span class="sxs-lookup"><span data-stu-id="e3d71-118">Design the app</span></span>

<span data-ttu-id="e3d71-119">In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die app.</span><span class="sxs-lookup"><span data-stu-id="e3d71-119">In this section you'll create the user interface for the app.</span></span>

1. <span data-ttu-id="e3d71-120">Öffnen Sie **/src/Styles.CSS** , und fügen Sie die folgenden Zeilen hinzu.</span><span class="sxs-lookup"><span data-stu-id="e3d71-120">Open **./src/styles.css** and add the following lines.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. <span data-ttu-id="e3d71-121">Fügen Sie das Bootstrap-Modul zur APP hinzu.</span><span class="sxs-lookup"><span data-stu-id="e3d71-121">Add the Bootstrap module to the app.</span></span> <span data-ttu-id="e3d71-122">Öffnen Sie **./src/App/app.Module.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-122">Open **./src/app/app.module.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="e3d71-123">Erstellen Sie eine neue Datei im Ordner **./src/App** mit dem Namen **User. TS** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="e3d71-123">Create a new file in the **./src/app** folder named **user.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. <span data-ttu-id="e3d71-124">Erstellen Sie eine Winkel Komponente für die obere Navigationsleiste auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="e3d71-124">Generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="e3d71-125">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="e3d71-125">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component nav-bar
    ```

1. <span data-ttu-id="e3d71-126">Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/NAV-Bar/NAV-Bar.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-126">Once the command completes, open **./src/app/nav-bar/nav-bar.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="e3d71-127">Öffnen Sie **./src/App/NAV-Bar/nav-bar.component.html** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-127">Open **./src/app/nav-bar/nav-bar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. <span data-ttu-id="e3d71-128">Erstellen Sie eine Startseite für die app.</span><span class="sxs-lookup"><span data-stu-id="e3d71-128">Create a home page for the app.</span></span> <span data-ttu-id="e3d71-129">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="e3d71-129">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component home
    ```

1. <span data-ttu-id="e3d71-130">Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/Home/Home.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-130">Once the command completes, open **./src/app/home/home.component.ts** and replace its contents with the following.</span></span>

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

1. <span data-ttu-id="e3d71-131">Öffnen Sie **./src/App/Home/home.component.html** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-131">Open **./src/app/home/home.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. <span data-ttu-id="e3d71-132">Erstellen Sie eine einfache `Alert` Klasse.</span><span class="sxs-lookup"><span data-stu-id="e3d71-132">Create a simple `Alert` class.</span></span> <span data-ttu-id="e3d71-133">Erstellen Sie eine neue Datei im Verzeichnis **./src/App** mit dem Namen **Alert. TS** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="e3d71-133">Create a new file in the **./src/app** directory named **alert.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. <span data-ttu-id="e3d71-134">Erstellen Sie einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten an den Benutzer verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="e3d71-134">Create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="e3d71-135">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="e3d71-135">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate service alerts
    ```

1. <span data-ttu-id="e3d71-136">Öffnen Sie **./src/App/Alerts.Service.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-136">Open **./src/app/alerts.service.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. <span data-ttu-id="e3d71-137">Generiert eine Alerts-Komponente zum Anzeigen von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="e3d71-137">Generate an alerts component to display alerts.</span></span> <span data-ttu-id="e3d71-138">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="e3d71-138">In your CLI, run the following command.</span></span>

    ```Shell
    ng generate component alerts
    ```

1. <span data-ttu-id="e3d71-139">Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/Alerts/Alerts.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-139">Once the command completes, open **./src/app/alerts/alerts.component.ts** and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. <span data-ttu-id="e3d71-140">Öffnen Sie **./src/App/Alerts/alerts.component.html** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-140">Open **./src/app/alerts/alerts.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. <span data-ttu-id="e3d71-141">Öffnen Sie **./src/App/App-Routing.Module.TS** , und ersetzen Sie die- `const routes: Routes = [];` Codezeile durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="e3d71-141">Open **./src/app/app-routing.module.ts** and replace the `const routes: Routes = [];` line with the following code.</span></span>

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. <span data-ttu-id="e3d71-142">Öffnen Sie **./src/App/app.component.html** , und ersetzen Sie den gesamten Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="e3d71-142">Open **./src/app/app.component.html** and replace its entire contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. <span data-ttu-id="e3d71-143">Fügen Sie eine Bilddatei Ihrer Wahl namens **no-profile-photo.png** im Verzeichnis **./src/Assets** hinzu.</span><span class="sxs-lookup"><span data-stu-id="e3d71-143">Add an image file of your choosing named **no-profile-photo.png** in the **./src/assets** directory.</span></span> <span data-ttu-id="e3d71-144">Dieses Bild wird als Foto des Benutzers verwendet, wenn der Benutzer in Microsoft Graph kein Foto hat.</span><span class="sxs-lookup"><span data-stu-id="e3d71-144">This image will be used as the user's photo when the user has no photo in Microsoft Graph.</span></span>

<span data-ttu-id="e3d71-145">Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="e3d71-145">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="e3d71-146">Nun sollte die APP sehr unterschiedlich aussehen.</span><span class="sxs-lookup"><span data-stu-id="e3d71-146">Now, the app should look very different.</span></span>

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)
