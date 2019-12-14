<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="37405-101">Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie Berechtigungen zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das Tool für die [eckige CLI](https://www.npmjs.com/package/@angular/cli) zu installieren und eine neue Winkel-APP zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="37405-101">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to install the [Angular CLI](https://www.npmjs.com/package/@angular/cli) tool and create a new Angular app.</span></span>

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

<span data-ttu-id="37405-102">In der eckigen CLI werden weitere Informationen angefordert.</span><span class="sxs-lookup"><span data-stu-id="37405-102">The Angular CLI will prompt for more information.</span></span> <span data-ttu-id="37405-103">Beantworten Sie die Anweisungen wie folgt.</span><span class="sxs-lookup"><span data-stu-id="37405-103">Answer the prompts as follows.</span></span>

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

<span data-ttu-id="37405-104">Nachdem der Befehl abgeschlossen ist, wechseln Sie `graph-tutorial` in das Verzeichnis in der CLI, und führen Sie den folgenden Befehl aus, um einen lokalen Webserver zu starten.</span><span class="sxs-lookup"><span data-stu-id="37405-104">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

```Shell
ng serve --open
```

<span data-ttu-id="37405-105">Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer standardmäßigen eckigen Seite geöffnet.</span><span class="sxs-lookup"><span data-stu-id="37405-105">Your default browser opens to [https://localhost:4200/](https://localhost:4200) with a default Angular page.</span></span> <span data-ttu-id="37405-106">Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, [https://localhost:4200/](https://localhost:4200) und navigieren Sie zu, um zu überprüfen, ob die neue APP funktioniert.</span><span class="sxs-lookup"><span data-stu-id="37405-106">If your browser doesn't open, open it and browse to [https://localhost:4200/](https://localhost:4200) to verify that the new app works.</span></span>

<span data-ttu-id="37405-107">Bevor Sie fortfahren, installieren Sie einige zusätzliche Pakete, die Sie später verwenden werden:</span><span class="sxs-lookup"><span data-stu-id="37405-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="37405-108">[Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.</span><span class="sxs-lookup"><span data-stu-id="37405-108">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="37405-109">[ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.</span><span class="sxs-lookup"><span data-stu-id="37405-109">[ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) for using Bootstrap components from Angular.</span></span>
- <span data-ttu-id="37405-110">[Winkel-fontawesome](https://github.com/FortAwesome/angular-fontawesome) zum Verwenden von fontawesome-Symbolen in eckig.</span><span class="sxs-lookup"><span data-stu-id="37405-110">[angular-fontawesome](https://github.com/FortAwesome/angular-fontawesome) to use FontAwesome icons in Angular.</span></span>
- <span data-ttu-id="37405-111">[fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-Regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)und [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) für die fontawesome-Symbole, die im Beispiel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="37405-111">[fontawesome-svg-core](https://github.com/FortAwesome/Font-Awesome), [free-regular-svg-icons](https://github.com/FortAwesome/Font-Awesome), and [free-solid-svg-icons](https://github.com/FortAwesome/Font-Awesome) for the FontAwesome icons used in the sample.</span></span>
- <span data-ttu-id="37405-112">[Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums-und Uhrzeitangaben.</span><span class="sxs-lookup"><span data-stu-id="37405-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="37405-113">[msal-Winkel](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und zum Abrufen von Zugriffstoken.</span><span class="sxs-lookup"><span data-stu-id="37405-113">[msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="37405-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), erforderlich für das `msal-angular` Paket.</span><span class="sxs-lookup"><span data-stu-id="37405-114">[rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), required for the `msal-angular` package.</span></span>
- <span data-ttu-id="37405-115">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum tätigen von Anrufen an Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="37405-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="37405-116">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="37405-116">Run the following command in your CLI.</span></span>

```Shell
npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.5.0 @fortawesome/fontawesome-svg-core@1.2.25
npm install @fortawesome/free-regular-svg-icons@5.11.2 @fortawesome/free-solid-svg-icons@5.11.2
npm install moment@2.24.0 moment-timezone@0.5.27 @ng-bootstrap/ng-bootstrap@5.1.4
npm install @azure/msal-angular@0.1.4 rxjs-compat@6.5.3 @microsoft/microsoft-graph-client@2.0.0
```

## <a name="design-the-app"></a><span data-ttu-id="37405-117">Entwerfen der APP</span><span class="sxs-lookup"><span data-stu-id="37405-117">Design the app</span></span>

<span data-ttu-id="37405-118">Beginnen Sie mit dem Hinzufügen der Bootstrap-CSS-Dateien zur APP sowie einigen globalen Formatvorlagen.</span><span class="sxs-lookup"><span data-stu-id="37405-118">Start by adding the Bootstrap CSS files to the app, as well as some global styles.</span></span> <span data-ttu-id="37405-119">Öffnen Sie `./src/styles.css` die, und fügen Sie die folgenden Zeilen hinzu.</span><span class="sxs-lookup"><span data-stu-id="37405-119">Open the `./src/styles.css` and add the following lines.</span></span>

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

<span data-ttu-id="37405-120">Als Nächstes fügen Sie die Bootstrap-und FontAwesome-Module zur APP hinzu.</span><span class="sxs-lookup"><span data-stu-id="37405-120">Next, add the Bootstrap and FontAwesome modules to the app.</span></span> <span data-ttu-id="37405-121">Öffnen `./src/app/app.module.ts` Sie und fügen Sie `import` die folgenden Anweisungen am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="37405-121">Open `./src/app/app.module.ts` and add the following `import` statements to the top of the file.</span></span>

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

<span data-ttu-id="37405-122">Fügen Sie dann nach allen `import` Anweisungen den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="37405-122">Then add the following code after all of the `import` statements.</span></span>

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

<span data-ttu-id="37405-123">Ersetzen Sie `@NgModule` in der Deklaration das vorhandene `imports` Array durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-123">In the `@NgModule` declaration, replace the existing `imports` array with the following.</span></span>

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

<span data-ttu-id="37405-124">Erstellen Sie jetzt eine Winkel Komponente für die obere Navigationsleiste auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="37405-124">Now generate an Angular component for the top navigation on the page.</span></span> <span data-ttu-id="37405-125">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="37405-125">In your CLI, run the following command.</span></span>

```Shell
ng generate component nav-bar
```

<span data-ttu-id="37405-126">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/nav-bar/nav-bar.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-126">Once the command completes, open the `./src/app/nav-bar/nav-bar.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37405-127">Öffnen Sie `./src/app/nav-bar/nav-bar.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-127">Open the `./src/app/nav-bar/nav-bar.component.html` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37405-128">Erstellen Sie als nächstes eine Startseite für die app.</span><span class="sxs-lookup"><span data-stu-id="37405-128">Next, create a home page for the app.</span></span> <span data-ttu-id="37405-129">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="37405-129">Run the following command in your CLI.</span></span>

```Shell
ng generate component home
```

<span data-ttu-id="37405-130">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/home/home.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-130">Once the command completes, open the `./src/app/home/home.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37405-131">Öffnen Sie dann `./src/app/home/home.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-131">Then open the `./src/app/home/home.component.html` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37405-132">Erstellen Sie nun einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten an den Benutzer verwenden kann.</span><span class="sxs-lookup"><span data-stu-id="37405-132">Now create an alert service that the app can use to display messages to the user.</span></span> <span data-ttu-id="37405-133">Beginnen Sie mit dem Erstellen `Alert` einer einfachen Klasse.</span><span class="sxs-lookup"><span data-stu-id="37405-133">Start by creating a simple `Alert` class.</span></span> <span data-ttu-id="37405-134">Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `alert.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="37405-134">Create a new file in the `./src/app` directory named `alert.ts` and add the following code.</span></span>

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

<span data-ttu-id="37405-135">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="37405-135">In your CLI, run the following command.</span></span>

```Shell
ng generate service alerts
```

<span data-ttu-id="37405-136">Öffnen Sie `./src/app/alerts.service.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-136">Open the `./src/app/alerts.service.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37405-137">Generieren Sie jetzt eine Alerts-Komponente zum Anzeigen von Warnungen.</span><span class="sxs-lookup"><span data-stu-id="37405-137">Now generate an alerts component to display alerts.</span></span> <span data-ttu-id="37405-138">Führen Sie in der CLI den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="37405-138">In your CLI, run the following command.</span></span>

```Shell
ng generate component alerts
```

<span data-ttu-id="37405-139">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/alerts/alerts.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-139">Once the command completes, open the `./src/app/alerts/alerts.component.ts` file and replace its contents with the following.</span></span>

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

<span data-ttu-id="37405-140">Öffnen Sie dann `./src/app/alerts/alerts.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="37405-140">Then open the `./src/app/alerts/alerts.component.html` file and replace its contents with the following.</span></span>

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

<span data-ttu-id="37405-141">Nachdem diese grundlegenden Komponenten definiert wurden, aktualisieren Sie die APP, um Sie zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="37405-141">Now with those basic components defined, update the app to use them.</span></span> <span data-ttu-id="37405-142">Öffnen Sie zuerst die `./src/app/app-routing.module.ts` Datei, und ersetzen `const routes: Routes = [];` Sie die-Codezeile durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="37405-142">First, open the `./src/app/app-routing.module.ts` file and replace the `const routes: Routes = [];` line with the following code.</span></span>

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

<span data-ttu-id="37405-143">Öffnen Sie die Datei `./src/app/app.component.html`, und ersetzen Sie ihren Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="37405-143">Open the `./src/app/app.component.html` file and replace its entire contents with the following.</span></span>

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

<span data-ttu-id="37405-144">Speichern Sie alle Änderungen, und aktualisieren Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="37405-144">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="37405-145">Nun sollte die APP sehr unterschiedlich aussehen.</span><span class="sxs-lookup"><span data-stu-id="37405-145">Now, the app should look very different.</span></span>

![Screenshot der neu gestalteten Startseite](images/create-app-01.png)
