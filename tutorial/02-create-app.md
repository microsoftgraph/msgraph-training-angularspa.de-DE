<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt erstellen Sie ein neues schräges Projekt.

1. Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie Berechtigungen zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das Tool für die [eckige CLI](https://www.npmjs.com/package/@angular/cli) zu installieren und eine neue Winkel-APP zu erstellen.

    ```Shell
    npm install -g @angular/cli@9.0.6
    ng new graph-tutorial
    ```

1. In der eckigen CLI werden weitere Informationen angefordert. Beantworten Sie die Anweisungen wie folgt.

    ```Shell
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. Nachdem der Befehl abgeschlossen ist, wechseln Sie `graph-tutorial` in das Verzeichnis in der CLI, und führen Sie den folgenden Befehl aus, um einen lokalen Webserver zu starten.

    ```Shell
    ng serve --open
    ```

1. Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer standardmäßigen eckigen Seite geöffnet. Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, [https://localhost:4200/](https://localhost:4200) und navigieren Sie zu, um zu überprüfen, ob die neue APP funktioniert.

## <a name="add-node-packages"></a>Hinzufügen von Knoten Paketen

Bevor Sie fortfahren, installieren Sie einige zusätzliche Pakete, die Sie später verwenden werden:

- [Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.
- [ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.
- [Winkel-fontawesome](https://github.com/FortAwesome/angular-fontawesome) zum Verwenden von fontawesome-Symbolen in eckig.
- [fontawesome-SVG-Core](https://github.com/FortAwesome/Font-Awesome), [Free-Regular-SVG-Icons](https://github.com/FortAwesome/Font-Awesome)und [Free-Solid-SVG-Icons](https://github.com/FortAwesome/Font-Awesome) für die fontawesome-Symbole, die im Beispiel verwendet werden.
- [Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums-und Uhrzeitangaben.
- [msal-Winkel](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und zum Abrufen von Zugriffstoken.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum tätigen von Anrufen an Microsoft Graph.

1. Führen Sie die folgenden Befehle in der CLI aus.

    ```Shell
    npm install bootstrap@4.4.1 @fortawesome/angular-fontawesome@0.6.0 @fortawesome/fontawesome-svg-core@1.2.27
    npm install @fortawesome/free-regular-svg-icons@5.12.1 @fortawesome/free-solid-svg-icons@5.12.1
    npm install moment@2.24.0 moment-timezone@0.5.28 @ng-bootstrap/ng-bootstrap@6.0.0
    npm install msal@1.2.1 @azure/msal-angular@1.0.0-beta.4 @microsoft/microsoft-graph-client@2.0.0
    ```

1. Führen Sie den folgenden Befehl in der CLI aus, um das eckige Lokalisierungspaket hinzuzufügen (erforderlich für ng-Bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Entwerfen der App

In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die app.

1. Öffnen Sie `./src/styles.css` die, und fügen Sie die folgenden Zeilen hinzu.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Fügen Sie die Bootstrap-und FontAwesome-Module zur APP hinzu. Öffnen `./src/app/app.module.ts` Sie den Inhalt, und ersetzen Sie ihn durch den folgenden Code.

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

1. Erstellen Sie eine neue Datei im `./src/app` Ordner mit `user.ts` dem Namen, und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Erstellen Sie eine Winkel Komponente für die obere Navigationsleiste auf der Seite. Führen Sie in der CLI den folgenden Befehl aus.

    ```Shell
    ng generate component nav-bar
    ```

1. Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/nav-bar/nav-bar.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

1. Öffnen Sie `./src/app/nav-bar/nav-bar.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Erstellen Sie eine Startseite für die app. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate component home
    ```

1. Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/home/home.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

1. Öffnen Sie `./src/app/home/home.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Erstellen Sie eine `Alert` einfache Klasse. Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `alert.ts` dem Namen, und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Erstellen Sie einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten an den Benutzer verwenden kann. Führen Sie in der CLI den folgenden Befehl aus.

    ```Shell
    ng generate service alerts
    ```

1. Öffnen Sie `./src/app/alerts.service.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Generiert eine Alerts-Komponente zum Anzeigen von Warnungen. Führen Sie in der CLI den folgenden Befehl aus.

    ```Shell
    ng generate component alerts
    ```

1. Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/alerts/alerts.component.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Öffnen Sie `./src/app/alerts/alerts.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Öffnen Sie `./src/app/app-routing.module.ts` die Datei, und `const routes: Routes = [];` ersetzen Sie die-Codezeile durch den folgenden Code.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Öffnen Sie die Datei `./src/app/app.component.html`, und ersetzen Sie ihren Inhalt durch Folgendes:

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

Speichern Sie alle Änderungen, und aktualisieren Sie die Seite. Nun sollte die APP sehr unterschiedlich aussehen.

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)
