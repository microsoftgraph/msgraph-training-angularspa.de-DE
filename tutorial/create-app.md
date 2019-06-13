<!-- markdownlint-disable MD002 MD041 -->

Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie Berechtigungen zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das Tool für die [eckige CLI](https://www.npmjs.com/package/@angular/cli) zu installieren und eine neue Winkel-APP zu erstellen.

```Shell
npm install -g @angular/cli
ng new graph-tutorial
```

In der eckigen CLI werden weitere Informationen angefordert. Beantworten Sie die Anweisungen wie folgt.

```Shell
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

Nachdem der Befehl abgeschlossen ist, wechseln Sie `graph-tutorial` in das Verzeichnis in der CLI, und führen Sie den folgenden Befehl aus, um einen lokalen Webserver zu starten.

```Shell
ng serve --open
```

Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer standardmäßigen eckigen Seite geöffnet. Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, [https://localhost:4200/](https://localhost:4200) und navigieren Sie zu, um zu überprüfen, ob die neue APP funktioniert.

Bevor Sie fortfahren, installieren Sie einige zusätzliche Pakete, die Sie später verwenden werden:

- [Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.
- [ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.
- [Winkel-fontawesome](https://github.com/FortAwesome/angular-fontawesome) zum Verwenden von fontawesome-Symbolen in eckig.
- [fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-Regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)und [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) für die fontawesome-Symbole, die im Beispiel verwendet werden.
- [Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums-und Uhrzeitangaben.
- [msal-Winkel](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und zum Abrufen von Zugriffstoken.
- [rxjs-compat](https://github.com/ReactiveX/rxjs/tree/master/compat), erforderlich für das `msal-angular` Paket.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum tätigen von Anrufen an Microsoft Graph.

Führen Sie den folgenden Befehl in der CLI aus.

```Shell
npm install bootstrap@4.3.1 @fortawesome/angular-fontawesome@0.3.0 @fortawesome/fontawesome-svg-core@1.2.17
npm install @fortawesome/free-regular-svg-icons@5.8.1 @fortawesome/free-solid-svg-icons@5.8.1
npm install moment@2.24.0 moment-timezone@0.5.25 @ng-bootstrap/ng-bootstrap@4.1.2
npm install @azure/msal-angular@0.1.2 rxjs-compat@6.5.1 @microsoft/microsoft-graph-client@1.6.0
```

## <a name="design-the-app"></a>Entwerfen der APP

Beginnen Sie mit dem Hinzufügen der Bootstrap-CSS-Dateien zur APP sowie einigen globalen Formatvorlagen. Öffnen Sie `./src/styles.css` die, und fügen Sie die folgenden Zeilen hinzu.

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

Als Nächstes fügen Sie die Bootstrap-und FontAwesome-Module zur APP hinzu. Öffnen `./src/app/app.module.ts` Sie und fügen Sie `import` die folgenden Anweisungen am Anfang der Datei hinzu.

```TypeScript
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { FontAwesomeModule } from '@fortawesome/angular-fontawesome';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faExternalLinkAlt } from '@fortawesome/free-solid-svg-icons';
import { faUserCircle } from '@fortawesome/free-regular-svg-icons';
```

Fügen Sie dann nach allen `import` Anweisungen den folgenden Code hinzu.

```TypeScript
library.add(faExternalLinkAlt);
library.add(faUserCircle);
```

Ersetzen Sie `@NgModule` in der Deklaration das vorhandene `imports` Array durch Folgendes.

```TypeScript
imports: [
  BrowserModule,
  AppRoutingModule,
  NgbModule,
  FontAwesomeModule
]
```

Erstellen Sie jetzt eine Winkel Komponente für die obere Navigationsleiste auf der Seite. Führen Sie in der CLI den folgenden Befehl aus.

```Shell
ng generate component nav-bar
```

Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/nav-bar/nav-bar.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Öffnen Sie `./src/app/nav-bar/nav-bar.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

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

Erstellen Sie als nächstes eine Startseite für die app. Führen Sie den folgenden Befehl in der CLI aus.

```Shell
ng generate component home
```

Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/home/home.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Öffnen Sie dann `./src/app/home/home.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

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

Erstellen Sie nun einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten an den Benutzer verwenden kann. Beginnen Sie mit dem Erstellen `Alert` einer einfachen Klasse. Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `alert.ts` dem Namen, und fügen Sie den folgenden Code hinzu.

```TypeScript
export class Alert {
  message: string;
  debug: string;
}
```

Führen Sie in der CLI den folgenden Befehl aus.

```Shell
ng generate service alerts
```

Öffnen Sie `./src/app/alerts.service.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

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

Generieren Sie jetzt eine Alerts-Komponente zum Anzeigen von Warnungen. Führen Sie in der CLI den folgenden Befehl aus.

```Shell
ng generate component alerts
```

Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/alerts/alerts.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Öffnen Sie dann `./src/app/alerts/alerts.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

```html
<div *ngFor="let alert of alertsService.alerts">
  <ngb-alert type="danger" (close)="close(alert)">
    <p>{{alert.message}}</p>
    <pre *ngIf="alert.debug" class="alert-pre border bg-light p-2"><code>{{alert.debug}}</code></pre>
  </ngb-alert>
</div>
```

Nachdem diese grundlegenden Komponenten definiert wurden, aktualisieren Sie die APP, um Sie zu verwenden. Öffnen Sie zuerst die `./src/app/app-routing.module.ts` Datei, und ersetzen `const routes: Routes = [];` Sie die-Codezeile durch den folgenden Code.

```TypeScript
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
];
```

Öffnen Sie die Datei `./src/app/app.component.html`, und ersetzen Sie ihren Inhalt durch Folgendes:

```html
<app-nav-bar></app-nav-bar>
<main role="main" class="container">
  <app-alerts></app-alerts>
  <router-outlet></router-outlet>
</main>
```

Speichern Sie alle Änderungen, und aktualisieren Sie die Seite. Nun sollte die APP sehr unterschiedlich aussehen.

![Screenshot der neu gestalteten Startseite](images/create-app-01.png)
