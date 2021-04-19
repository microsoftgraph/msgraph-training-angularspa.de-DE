<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung von der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph abzurufen. In diesem Schritt integrieren Sie die [Microsoft-Authentifizierungsbibliothek für Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) in die Anwendung.

1. Erstellen Sie eine neue Datei im **Verzeichnis ./src** mit dem Namen **oauth.ts,** und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.example.ts":::

    Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus dem Anwendungsregistrierungsportal.

    > [!IMPORTANT]
    > Wenn Sie die Quellcodeverwaltung wie git verwenden, sollten Sie jetzt die **oauth.ts-Datei** aus der Quellcodeverwaltung ausschließen, um versehentlich ein Leck ihrer App-ID zu vermeiden.

1. Öffnen **Sie ./src/app/app.module.ts,** und fügen Sie die folgenden Anweisungen am oberen Rand `import` der Datei hinzu.

    ```typescript
    import { IPublicClientApplication,
             PublicClientApplication,
             BrowserCacheLocation } from '@azure/msal-browser';
    import { MsalModule,
             MsalService,
             MSAL_INSTANCE } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Fügen Sie die folgende Funktion unterhalb der Anweisungen `import` hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="MSALFactorySnippet":::

1. Fügen Sie `MsalModule` das dem Array innerhalb der `imports` `@NgModule` Deklaration hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ImportsSnippet" highlight="6":::

1. Fügen Sie `MSALInstanceFactory` das und dem Array innerhalb der `MsalService` `providers` `@NgModule` Deklaration hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="ProvidersSnippet" highlight="2-6":::

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

In diesem Abschnitt erstellen Sie einen Authentifizierungsdienst und implementieren das Anmelden und Abmelden.

1. Führen Sie den folgenden Befehl in Ihrer CLI aus.

    ```Shell
    ng generate service auth
    ```

    Indem Sie einen Dienst dafür erstellen, können Sie ihn ganz einfach in alle Komponenten integrieren, die Zugriff auf Authentifizierungsmethoden benötigen.

1. Öffnen Sie nach Abschluss des Befehls **./src/app/auth.service.ts,** und ersetzen Sie den Inhalt durch den folgenden Code.

    ```typescript
    import { Injectable } from '@angular/core';
    import { AccountInfo } from '@azure/msal-browser';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user?: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = undefined;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        const result = await this.msalService
          .loginPopup(OAuthSettings)
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Login failed',
              JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.msalService.instance.setActiveAccount(result.account);
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = 'Adele Vance';
          this.user.email = 'AdeleV@contoso.com';
          this.user.avatar = '/assets/no-profile-photo.png';
        }
      }

      // Sign out
      async signOut(): Promise<void> {
        await this.msalService.logout().toPromise();
        this.user = undefined;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        const result = await this.msalService
          .acquireTokenSilent({
            scopes: OAuthSettings.scopes
          })
          .toPromise()
          .catch((reason) => {
            this.alertsService.addError('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.addSuccess('Token acquired', result.accessToken);
          return result.accessToken;
        }

        // Couldn't get a token
        this.authenticated = false;
        return '';
      }
    }
    ```

1. Öffnen **Sie ./src/app/nav-bar/nav-bar.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,34-36,38-40":::

1. Öffnen **Sie ./src/app/home/home.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,13-20,22,26-33":::

Speichern Sie Ihre Änderungen, und aktualisieren Sie den Browser. Klicken Sie **auf die Schaltfläche Klicken Sie hier, um sich zu** anmelden, und Sie sollten an umgeleitet `https://login.microsoftonline.com` werden. Melden Sie sich mit Ihrem Microsoft-Konto an, und stimmen Sie den angeforderten Berechtigungen zu. Die App-Seite sollte aktualisiert werden und das Token anzeigen.

### <a name="get-user-details"></a>Benutzerdetails abrufen

Derzeit legt der Authentifizierungsdienst konstante Werte für den Anzeigenamen und die E-Mail-Adresse des Benutzers fest. Nachdem Sie über ein Zugriffstoken verfügen, können Sie Benutzerdetails aus Microsoft Graph erhalten, damit diese Werte dem aktuellen Benutzer entsprechen.

1. Öffnen **Sie ./src/app/auth.service.ts,** und fügen Sie die folgenden Anweisungen am oberen Rand `import` der Datei hinzu.

    ```typescript
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';
    ```

1. Fügen Sie eine neue Funktion zur `AuthService`-Klasse mit dem Namen `getUser` hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="GetUserSnippet":::

1. Suchen und entfernen Sie den folgenden Code in der Methode, die eine Warnung zum `getAccessToken` Anzeigen des Zugriffstokens hinzufügt.

    ```typescript
    // Temporary to display token in an error box
    this.alertsService.addSuccess('Token acquired', result);
    ```

1. Suchen Und entfernen Sie den folgenden Code aus der `signIn` -Methode.

    ```typescript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    this.user.avatar = '/assets/no-profile-photo.png';
    ```

1. Fügen Sie an seiner Stelle den folgenden Code hinzu.

    ```typescript
    this.user = await this.getUser();
    ```

    Dieser neue Code verwendet das Microsoft Graph SDK, um die Details des Benutzers zu erhalten, und erstellt dann ein Objekt mit Werten, die vom `User` API-Aufruf zurückgegeben werden.

1. Ändern Sie die für die Klasse, um zu überprüfen, ob der Benutzer bereits angemeldet `constructor` `AuthService` ist, und laden Sie gegebenenfalls ihre Details. Ersetzen Sie das vorhandene `constructor` durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="ConstructorSnippet" highlight="5-7":::

1. Entfernen Sie den temporären Code aus der `HomeComponent` Klasse. Öffnen **Sie ./src/app/home/home.component.ts,** und ersetzen Sie die vorhandene `signIn` Funktion durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="SignInSnippet":::

Wenn Sie nun Ihre Änderungen speichern und die App starten, sollten Sie nach der Anmeldung wieder auf der Startseite landen, aber die Benutzeroberfläche sollte sich ändern, um anzugeben, dass Sie angemeldet sind.

![Screenshot der Startseite nach dem Anmelden](./images/add-aad-auth-01.png)

Klicken Sie in der oberen rechten Ecke auf den Benutzer-Avatar, um auf den **Link Abmelden zu** zugreifen. Wenn Sie auf **Abmelden** klicken, wird die Sitzung zurückgesetzt und Sie kehren zur Startseite zurück.

![Screenshot des Dropdown-Menüs mit dem Link „Abmelden“](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Speichern und Aktualisieren von Token

An diesem Punkt verfügt Ihre Anwendung über ein Zugriffstoken, das in der Kopfzeile von `Authorization` API-Aufrufen gesendet wird. Dies ist das Token, mit dem die App im Namen des Benutzers auf Microsoft Graph zugreifen kann.

Dieses Token ist jedoch nur kurzzeitig verfügbar. Das Token läuft eine Stunde nach der Enerbung ab. Da die App die MSAL-Bibliothek verwendet, müssen Sie keine Tokenspeicher- oder Aktualisierungslogik implementieren. Das `MsalService` Token wird im Browserspeicher zwischengespeichert. Die Methode überprüft zuerst das zwischengespeicherte Token, und wenn es nicht `acquireTokenSilent` abgelaufen ist, gibt sie es zurück. Wenn sie abgelaufen ist, wird eine automatische Anforderung zum Abrufen einer neuen Anforderung gestellt.
