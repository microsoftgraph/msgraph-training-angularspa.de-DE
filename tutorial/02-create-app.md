<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt erstellen Sie ein neues schräges Projekt.

1. Öffnen Sie die Befehlszeilenschnittstelle (CLI), navigieren Sie zu einem Verzeichnis, in dem Sie Berechtigungen zum Erstellen von Dateien haben, und führen Sie die folgenden Befehle aus, um das Tool für die [eckige CLI](https://www.npmjs.com/package/@angular/cli) zu installieren und eine neue Winkel-APP zu erstellen.

    ```Shell
    npm install -g @angular/cli@10.1.7
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

1. Der Standardbrowser wird [https://localhost:4200/](https://localhost:4200) mit einer standardmäßigen eckigen Seite geöffnet. Wenn Ihr Browser nicht geöffnet wird, öffnen Sie ihn, und navigieren Sie zu, um [https://localhost:4200/](https://localhost:4200) zu überprüfen, ob die neue APP funktioniert.

## <a name="add-node-packages"></a>Hinzufügen von Knoten Paketen

Bevor Sie fortfahren, installieren Sie einige zusätzliche Pakete, die Sie später verwenden werden:

- [Bootstrap](https://github.com/twbs/bootstrap) für Styling und allgemeine Komponenten.
- [ng-Bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) für die Verwendung von Bootstrap-Komponenten aus eckig.
- [Zeitpunkt](https://github.com/moment/moment) für die Formatierung von Datums-und Uhrzeitangaben.
- [Windows – IANA](https://github.com/rubenillodo/windows-iana)
- [msal-Winkel](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) für die Authentifizierung bei Azure Active Directory und zum Abrufen von Zugriffstoken.
- [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) zum tätigen von Anrufen an Microsoft Graph.

1. Führen Sie die folgenden Befehle in der CLI aus.

    ```Shell
    npm install bootstrap@4.5.3 @ng-bootstrap/ng-bootstrap@7.0.0 msal@1.4.2 @azure/msal-angular@1.1.1
    npm install moment@2.29.1 moment-timezone@0.5.31 windows-iana@4.2.1
    npm install @microsoft/microsoft-graph-client@2.1.0 @microsoft/microsoft-graph-types@1.24.0
    ```

1. Führen Sie den folgenden Befehl in der CLI aus, um das eckige Lokalisierungspaket hinzuzufügen (erforderlich für ng-Bootstrap).

    ```Shell
    ng add @angular/localize
    ```

## <a name="design-the-app"></a>Entwerfen der App

In diesem Abschnitt erstellen Sie die Benutzeroberfläche für die app.

1. Öffnen Sie **/src/Styles.CSS** , und fügen Sie die folgenden Zeilen hinzu.

    :::code language="css" source="../demo/graph-tutorial/src/styles.css":::

1. Fügen Sie das Bootstrap-Modul zur APP hinzu. Öffnen Sie **./src/App/app.Module.TS** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Erstellen Sie eine neue Datei im Ordner **./src/App** mit dem Namen **User. TS** , und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/user.ts" id="user":::

1. Erstellen Sie eine Winkel Komponente für die obere Navigationsleiste auf der Seite. Führen Sie in der CLI den folgenden Befehl aus.

    ```Shell
    ng generate component nav-bar
    ```

1. Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/NAV-Bar/NAV-Bar.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Öffnen Sie **./src/App/NAV-Bar/nav-bar.component.html** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.html" id="navHtml":::

1. Erstellen Sie eine Startseite für die app. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate component home
    ```

1. Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/Home/Home.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.

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

1. Öffnen Sie **./src/App/Home/home.component.html** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/home/home.component.html" id="homeHtml":::

1. Erstellen Sie eine einfache `Alert` Klasse. Erstellen Sie eine neue Datei im Verzeichnis **./src/App** mit dem Namen **Alert. TS** , und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alert.ts" id="alert":::

1. Erstellen Sie einen Warnungsdienst, den die APP zum Anzeigen von Nachrichten an den Benutzer verwenden kann. Führen Sie in der CLI den folgenden Befehl aus.

    ```Shell
    ng generate service alerts
    ```

1. Öffnen Sie **./src/App/Alerts.Service.TS** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts.service.ts" id="alertsService":::

1. Generiert eine Alerts-Komponente zum Anzeigen von Warnungen. Führen Sie in der CLI den folgenden Befehl aus.

    ```Shell
    ng generate component alerts
    ```

1. Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/Alerts/Alerts.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/alerts/alerts.component.ts" id="alertComponent":::

1. Öffnen Sie **./src/App/Alerts/alerts.component.html** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/alerts/alerts.component.html" id="alertHtml":::

1. Öffnen Sie **./src/App/App-Routing.Module.TS** , und ersetzen Sie die- `const routes: Routes = [];` Codezeile durch den folgenden Code.

    ```typescript
    import { HomeComponent } from './home/home.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
    ];
    ```

1. Öffnen Sie **./src/App/app.component.html** , und ersetzen Sie den gesamten Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/app.component.html" id="appHtml":::

1. Fügen Sie eine Bilddatei Ihrer Wahl namens **no-profile-photo.png** im Verzeichnis **./src/Assets** hinzu. Dieses Bild wird als Foto des Benutzers verwendet, wenn der Benutzer in Microsoft Graph kein Foto hat.

Speichern Sie alle Änderungen, und aktualisieren Sie die Seite. Nun sollte die APP sehr unterschiedlich aussehen.

![Screenshot der neu gestalteten Homepage](images/create-app-01.png)
