<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt erstellen Sie ein neues Angular Projekt.

1. Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie über die Rechte zum Erstellen von Dateien verfügen, und führen Sie die folgenden Befehle aus, um das [Angular](https://www.npmjs.com/package/@angular/cli) CLI-Tool zu installieren und eine neue Angular zu erstellen.

    ```Shell
    npm install -g @angular/cli@11.2.9
    ng new graph-tutorial
    ```

1. Die Angular CLI fordert weitere Informationen an. Beantworten Sie die Eingabeaufforderungen wie folgt.

    ```Shell
    ? Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? Yes
    ? Would you like to add Angular routing? Yes
    ? Which stylesheet format would you like to use? CSS
    ```

1. Wechseln Sie nach Abschluss des Befehls in das Verzeichnis in Der CLI, und führen Sie den folgenden Befehl aus, um einen `graph-tutorial` lokalen Webserver zu starten.

    ```Shell
    ng serve --open
    ```

1. Ihr Standardbrowser wird mit [https://localhost:4200/](https://localhost:4200) einer Standardseite Angular geöffnet. Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, und navigieren Sie zu, um zu überprüfen, ob [https://localhost:4200/](https://localhost:4200) die neue App funktioniert.

## <a name="add-node-packages"></a>Hinzufügen von Knotenpaketen

Installieren Sie vor dem Wechsel einige zusätzliche Pakete, die Sie später verwenden werden:

- [bootstrap](https://github.com/twbs/bootstrap) für Formatieren und allgemeine Komponenten.
- [ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten Angular.
- [Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums- und Zeitangaben.
- [windows-iana](https://github.com/rubenillodo/windows-iana)
- [msal-angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung Azure Active Directory und Abrufen von Zugriffstoken.
- [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum Aufrufen von Microsoft-Graph.

1. Führen Sie die folgenden Befehle in Ihrer CLI aus.

    ```Shell
    npm install bootstrap@4.6.0 @ng-bootstrap/ng-bootstrap@9.1.0
    npm install @azure/msal-browser@2.14.0 @azure/msal-angular@2.0.0-beta.4
    npm install moment-timezone@0.5.33 windows-iana@5.0.2
    npm install @microsoft/microsoft-graph-client@2.2.1 @microsoft/microsoft-graph-types@1.35.0
    ```

1. Führen Sie den folgenden Befehl in Ihrer CLI aus, um das Angular (erforderlich für ng-bootstrap) hinzuzufügen.

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Entwerfen der App

In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die App.

1. Öffnen **Sie ./src/styles.css,** und fügen Sie die folgenden Zeilen hinzu.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Fügen Sie der App das Bootstrap-Modul hinzu. Öffnen **Sie ./src/app/app.module.ts,** und ersetzen Sie dessen Inhalt durch Folgendes.

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

1. Erstellen Sie eine neue Datei im **Ordner ./src/app** mit dem Namen **user.ts,** und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="UserSnippet":::

1. Generieren Sie Angular komponente für die oberste Navigation auf der Seite. Führen Sie in Ihrer CLI den folgenden Befehl aus.

    ```Shell
    ng generate component nav-bar
    ```

1. Öffnen Sie nach Abschluss des Befehls **./src/app/nav-bar/nav-bar.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.

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

1. Öffnen **Sie ./src/app/nav-bar/nav-bar.component.html,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Erstellen Sie eine Homepage für die App. Führen Sie den folgenden Befehl in Ihrer CLI aus.

    ```Shell
    ng generate component home
    ```

1. Öffnen Sie nach Abschluss des Befehls **./src/app/home/home.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.

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

1. Öffnen **Sie ./src/app/home/home.component.html,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Erstellen Sie eine einfache `Alert` Klasse. Erstellen Sie eine neue Datei im **Verzeichnis ./src/app** mit dem Namen **alert.ts,** und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="AlertSnippet":::

1. Erstellen Sie einen Benachrichtigungsdienst, den die App zum Anzeigen von Nachrichten für den Benutzer verwenden kann. Führen Sie in Ihrer CLI den folgenden Befehl aus.

    ```Shell
    ng generate service alerts
    ```

1. Öffnen **Sie ./src/app/alerts.service.ts,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Generieren Sie eine Warnungskomponente, um Warnungen anzuzeigen. Führen Sie in Ihrer CLI den folgenden Befehl aus.

    ```Shell
    ng generate component alerts
    ```

1. Öffnen Sie nach Abschluss des Befehls **./src/app/alerts/alerts.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="AlertsComponentSnippet":::

1. Öffnen **Sie ./src/app/alerts/alerts.component.html,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="AlertHtml":::

1. Öffnen **Sie ./src/app/app-routing.module.ts,** und ersetzen Sie die `const routes: Routes = [];` Zeile durch den folgenden Code.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Öffnen **Sie ./src/app/app.component.html,** und ersetzen Sie den gesamten Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="AppHtml":::

1. Fügen Sie eine Bilddatei Ihrer Wahl namens **no-profile-photo.png** **im Verzeichnis ./src/assets** hinzu. Dieses Bild wird als Foto des Benutzers verwendet, wenn der Benutzer kein Foto in Microsoft Graph.

Speichern Sie alle Änderungen, und aktualisieren Sie die Seite. Jetzt sollte die App ganz anders aussehen.

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)
