<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fa336-101">Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie die Berechtigung zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das [eckige CLI](https://www.npmjs.com/package/@angular/cli) -Tool zu installieren und eine neue eckige APP zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fa336-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

<span data-ttu-id="fa336-102">Die eckige CLI fordert weitere Informationen an.</span><span class="sxs-lookup"><span data-stu-id="fa336-102">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="fa336-103">Beantworten Sie die Eingabeaufforderungen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="fa336-103">Answer the prompts as follows.</span></span>

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

<span data-ttu-id="fa336-104">Sobald der Befehl abgeschlossen ist, wechseln Sie `graph-tutorial` in das Verzeichnis in der CLI, und führen Sie den folgenden Befehl aus, um einen lokalen Webserver zu starten.</span><span class="sxs-lookup"><span data-stu-id="fa336-104">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

```Shell
ng serve --open
```

<span data-ttu-id="fa336-105">Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer eckigen Standardseite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="fa336-105">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="fa336-106">Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, [https://localhost:4200/](https://localhost:4200) und navigieren Sie zu, um zu überprüfen, ob die neue APP funktioniert.</span><span class="sxs-lookup"><span data-stu-id="fa336-106">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

<span data-ttu-id="fa336-107">Bevor Sie fortfahren, sollten Sie einige zusätzliche Pakete installieren, die Sie später verwenden werden:</span><span class="sxs-lookup"><span data-stu-id="fa336-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="fa336-108">[Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.</span><span class="sxs-lookup"><span data-stu-id="fa336-108">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="fa336-109">[ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.</span><span class="sxs-lookup"><span data-stu-id="fa336-109">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="fa336-110">[eckig-fontawesome](https://github.com/FortAwesome/angular-fontawesome) zur Verwendung von fontawesome-Symbolen in eckiger.</span><span class="sxs-lookup"><span data-stu-id="fa336-110">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="fa336-111">[fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-Regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)und [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) für die im Beispiel verwendeten fontawesome-Symbole.</span><span class="sxs-lookup"><span data-stu-id="fa336-111">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="fa336-112">[Moment](https://github.com/moment/moment) für das Formatieren von Datums-und Uhrzeitangaben.</span><span class="sxs-lookup"><span data-stu-id="fa336-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="fa336-113">[msal-eckig](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und das Abrufen von Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="fa336-113">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="fa336-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), für das `msal-angular` Paket erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fa336-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), required for the `msal-angular` package.</span></span>
- <span data-ttu-id="fa336-115">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) für Aufrufe von Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="fa336-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="fa336-116">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="fa336-116">Run the following command in your CLI.</span></span>

```Shell
npm install bootstrap@4.1.3 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.8
npm install @fortawesome/free-regular-svg-icons@5.5.0 @fortawesome/free-solid-svg-icons@5.5.0
npm install moment@2.22.2 moment-timezone@0.5.23 @ng-bootstrap/ng-bootstrap@4.0.0
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.3.3 @microsoft/microsoft-graph-client@1.3.0
```

## <a name="design-the-app"></a><span data-ttu-id="fa336-117">Entwerfen der APP</span><span class="sxs-lookup"><span data-stu-id="fa336-117">Design the app</span></span>

<span data-ttu-id="fa336-118">Beginnen Sie, indem Sie die Bootstrap-CSS-Dateien der APP sowie einige globale Formatvorlagen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fa336-118">Start by adding the Bootstrap CSS files to the app, as well as some global styles.</span></span> <span data-ttu-id="fa336-119">Öffnen Sie `./src/styles.css` die, und fügen Sie die folgenden Zeilen hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa336-119">Open the `./src/styles.css` and add the following lines.</span></span>

```CSS
@import "~bootstrap/dist/css/bootstrap.css";

/* Add padding for the nav bar */
body {
  padding-top: 4.5rem;
}

/* Style debug info in alerts */
.alert-pre {
  word-wrap: break-word;
  word-break: break-all;
  white-space: pre-wrap;
}
```

<span data-ttu-id="fa336-120">Als Nächstes fügen Sie die Bootstrap-und FontAwesome-Module zur APP hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa336-120">Next, add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="fa336-121">Öffnen `./src/app/app.module.ts` Sie und fügen Sie `import` die folgenden Anweisungen am Anfang der Datei.</span><span class="sxs-lookup"><span data-stu-id="fa336-121">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

<span data-ttu-id="fa336-122">Fügen Sie dann nach allen `import` Anweisungen den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa336-122">Then add the following code after all of the `import` statements.</span></span>

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

<span data-ttu-id="fa336-123">Ersetzen Sie `@NgModule` in der Deklaration das vorhandene `imports` Array durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-123">In the `@NgModule` declaration, replace the existing `imports` array with the following.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

<span data-ttu-id="fa336-124">Erstellen Sie nun eine eckige Komponente für die obere Navigationsleiste auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="fa336-124">Now generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="fa336-125">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="fa336-125">In your CLI, run the following command.</span></span>

```Shell
ng generate component nav-bar
```

<span data-ttu-id="fa336-126">Sobald der Befehl abgeschlossen ist, öffnen `./src/app/nav-bar/nav-bar.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-126">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

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
  user: any;

  constructor() { }

  ngOnInit() {
    this.showNav = false;
    this.authenticated = false;
    this.user = {};
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
      email: 'AdeleV@contoso.com'
    };
  }

  signOut(): void {
    // Temporary
    this.authenticated = false;
    this.user = {};
  }
}
```

<span data-ttu-id="fa336-127">Öffnen Sie `./src/app/nav-bar/nav-bar.component.html` die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-127">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

```html
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <div class="container">
    <a routerLink="/" class="navbar-brand">Angular Graph Tutorial</a>
    <button class="navbar-toggler" type="button" (click)="toggleNavBar()" [attr.aria-expanded]="showNav"
    aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" [class.show]="showNav" id="navbarCollapse">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <a routerLink="/" class="nav-link" routerLinkActive="active">Home</a>
        </li>
        <li *ngIf="authenticated" class="nav-item">
          <a routerLink="/calendar" class="nav-link" routerLinkActive="active">Calendar</a>
        </li>
      </ul>
      <ul class="navbar-nav justify-content-end">
        <li class="nav-item">
          <a class="nav-link" href="https://docs.microsoft.com/graph/overview" target="_blank">
            <fa-icon [icon]="[ 'fas', 'external-link-alt' ]" class="mr-1"></fa-icon>Docs
          </a>
        </li>
        <li *ngIf="authenticated" ngbDropdown placement="bottom-right" class="nav-item">
          <a ngbDropdownToggle id="userMenu" class="nav-link" href="javascript:undefined" role="button" aria-haspopup="true"
            aria-expanded="false">
            <div *ngIf="user.avatar; then userAvatar else defaultAvatar"></div>
            <ng-template #userAvatar>
              <img src="user.avatar" class="rounded-circle align-self-center mr-2" style="width: 32px;">
            </ng-template>
            <ng-template #defaultAvatar>
              <fa-icon [icon]="[ 'far', 'user-circle' ]" fixedWidth="true" size="lg"
                class="rounded-circle align-self-center mr-2"></fa-icon>
            </ng-template>
          </a>
          <div ngbDropdownMenu aria-labelledby="userMenu">
            <h5 class="dropdown-item-text mb-0">{{user.displayName}}</h5>
            <p class="dropdown-item-text text-muted mb-0">{{user.email}}</p>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="javascript:undefined" role="button" (click)="signOut()">Sign Out</a>
          </div>
        </li>
        <li *ngIf="!authenticated" class="nav-item">
          <a class="nav-link" href="javascript:undefined" role="button" (click)="signIn()">Sign In</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

<span data-ttu-id="fa336-128">Erstellen Sie als nächstes eine Homepage für die app.</span><span class="sxs-lookup"><span data-stu-id="fa336-128">Next, create a home page for the app.</span></span> <span data-ttu-id="fa336-129">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="fa336-129">Run the following command in your CLI.</span></span>

```Shell
ng generate component home
```

<span data-ttu-id="fa336-130">Sobald der Befehl abgeschlossen ist, öffnen `./src/app/home/home.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-130">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';

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

<span data-ttu-id="fa336-131">Öffnen Sie dann `./src/app/home/home.component.html` die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-131">Then open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

```html
<div class="jumbotron">
  <h1>Angular Graph Tutorial</h1>
  <p class="lead">This sample app shows how to use the Microsoft Graph API from Angular</p>
  <div *ngIf="authenticated; then welcomeUser else signInPrompt"></div>
  <ng-template #welcomeUser>
    <h4>Welcome {{ user.displayName }}!</h4>
    <p>Use the navigation bar at the top of the page to get started.</p>
  </ng-template>
  <ng-template #signInPrompt>
    <a href="javascript:undefined" class="btn btn-primary btn-large" role="button" (click)="signIn()">Click here to sign in</a>
  </ng-template>
</div>
```

<span data-ttu-id="fa336-132">Erstellen Sie jetzt einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten für den Benutzer verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="fa336-132">Now create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="fa336-133">Erstellen Sie zunächst eine einfache `Alert` Klasse.</span><span class="sxs-lookup"><span data-stu-id="fa336-133">Start by creating a simple `Alert` class.</span></span> <span data-ttu-id="fa336-134">Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `alert.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="fa336-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

<span data-ttu-id="fa336-135">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="fa336-135">In your CLI, run the following command.</span></span>

```Shell
ng generate service alerts
```

<span data-ttu-id="fa336-136">Öffnen Sie `./src/app/alerts.service.ts` die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-136">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Injectable } from '@angular/core';
import { Alert } from './alert';

@Injectable({
  providedIn: 'root'
})
export class AlertsService {

  alerts: Alert[] = [];

  add(message: string, debug: string) {
    this.alerts.push({message: message, debug: debug});
  }

  remove(alert: Alert) {
    this.alerts.splice(this.alerts.indexOf(alert), 1);
  }
}
```

<span data-ttu-id="fa336-137">Generieren Sie jetzt eine Warnungs Komponente, um Warnungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="fa336-137">Now generate an alerts component to display alerts.</span></span> <span data-ttu-id="fa336-138">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="fa336-138">In your CLI, run the following command.</span></span>

```Shell
ng generate component alerts
```

<span data-ttu-id="fa336-139">Sobald der Befehl abgeschlossen ist, öffnen `./src/app/alerts/alerts.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-139">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Component, OnInit } from '@angular/core';
import { AlertsService } from '../alerts.service';
import { Alert } from '../alert';

@Component({
  selector: 'app-alerts',
  templateUrl: './alerts.component.html',
  styleUrls: ['./alerts.component.css']
})
export class AlertsComponent implements OnInit {

  constructor(private alertsService: AlertsService) { }

  ngOnInit() {
  }

  close(alert: Alert) {
    this.alertsService.remove(alert);
  }
}
```

<span data-ttu-id="fa336-140">Öffnen Sie dann `./src/app/alerts/alerts.component.html` die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="fa336-140">Then open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

<span data-ttu-id="fa336-141">Aktualisieren Sie nun mit diesen grundlegenden Komponenten die APP, um Sie zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fa336-141">Now with those basic components defined, update the app to use them.</span></span> <span data-ttu-id="fa336-142">Öffnen Sie zunächst die `./src/app/app-routing.module.ts` Datei, und ersetzen `const routes: Routes = [];` Sie die-Codezeile durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="fa336-142">First, open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

<span data-ttu-id="fa336-143">Öffnen Sie die Datei `./src/app/app.component.html`, und ersetzen Sie ihren Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="fa336-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

<span data-ttu-id="fa336-144">Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="fa336-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="fa336-145">Nun sollte die APP sehr unterschiedlich aussehen.</span><span class="sxs-lookup"><span data-stu-id="fa336-145">Now, the app should look very different.</span></span>

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)