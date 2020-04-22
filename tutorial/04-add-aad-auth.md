<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen. Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten. In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek für eckig](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) in die Anwendung integrieren.

1. Erstellen Sie eine neue Datei im `./src` Verzeichnis mit `oauth.ts` dem Namen, und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/oauth.ts.example":::

    Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID aus dem Anwendungs Registrierungs Portal.

    > [!IMPORTANT]
    > Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei `oauth.ts` aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.

1. Öffnen `./src/app/app.module.ts` Sie und fügen Sie `import` die folgenden Anweisungen am Anfang der Datei hinzu.

    ```TypeScript
    import { MsalModule } from '@azure/msal-angular';
    import { OAuthSettings } from '../oauth';
    ```

1. Fügen Sie `MsalModule` das dem `imports` -Array innerhalb `@NgModule` der Deklaration hinzu, und initialisieren Sie es mit der APP-ID.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/app.module.ts" id="imports":::

## <a name="implement-sign-in"></a>Implementieren der Anmeldung

In diesem Abschnitt erstellen Sie einen Authentifizierungsdienst und implementieren die Anmeldung und Abmeldung.

1. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate service auth
    ```

    Wenn Sie einen Dienst dafür erstellen, können Sie ihn einfach in alle Komponenten einfügen, die Zugriff auf Authentifizierungsmethoden benötigen.

1. Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/auth.service.ts` Sie die Datei, und ersetzen Sie den Inhalt durch den folgenden Code.

    ```TypeScript
    import { Injectable } from '@angular/core';
    import { MsalService } from '@azure/msal-angular';

    import { AlertsService } from './alerts.service';
    import { OAuthSettings } from '../oauth';
    import { User } from './user';

    @Injectable({
      providedIn: 'root'
    })

    export class AuthService {
      public authenticated: boolean;
      public user: User;

      constructor(
        private msalService: MsalService,
        private alertsService: AlertsService) {

        this.authenticated = false;
        this.user = null;
      }

      // Prompt the user to sign in and
      // grant consent to the requested permission scopes
      async signIn(): Promise<void> {
        let result = await this.msalService.loginPopup(OAuthSettings)
          .catch((reason) => {
            this.alertsService.add('Login failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          this.authenticated = true;
          // Temporary placeholder
          this.user = new User();
          this.user.displayName = "Adele Vance";
          this.user.email = "AdeleV@contoso.com";
        }
      }

      // Sign out
      signOut(): void {
        this.msalService.logout();
        this.user = null;
        this.authenticated = false;
      }

      // Silently request an access token
      async getAccessToken(): Promise<string> {
        let result = await this.msalService.acquireTokenSilent(OAuthSettings)
          .catch((reason) => {
            this.alertsService.add('Get token failed', JSON.stringify(reason, null, 2));
          });

        if (result) {
          // Temporary to display token in an error box
          this.alertsService.add('Token acquired', result.accessToken);
          return result.accessToken;
        }
        return null;
      }
    }
    ```

1. Öffnen Sie `./src/app/nav-bar/nav-bar.component.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/nav-bar/nav-bar.component.ts" id="navBarSnippet" highlight="3,15-22,24,26-28,36-38,40-42":::

1. Öffnen`./src/app/home/home.component.ts` Sie den Inhalt, und ersetzen Sie ihn durch den folgenden Code.

    :::code language="typescript" source="snippets/snippets.ts" id="homeSnippet" highlight="3,12-19,21,23,25-27":::

Speichern Sie Ihre Änderungen, und aktualisieren Sie den Browser. Klicken Sie auf die Schaltfläche **Klicken Sie hier, um sich anzumelden** , und Sie sollten zu `https://login.microsoftonline.com`umgeleitet werden. Melden Sie sich mit Ihrem Microsoft-Konto an, und stimmen Sie den angeforderten Berechtigungen zu. Die APP-Seite sollte aktualisiert werden, wobei das Token angezeigt wird.

### <a name="get-user-details"></a>Benutzerdetails abrufen

Der Authentifizierungsdienst legt im Moment Konstante Werte für den Anzeigenamen und die e-Mail-Adresse des Benutzers fest. Da Sie nun über ein Zugriffstoken verfügen, können Sie Benutzer Details aus Microsoft Graph abrufen, damit diese Werte dem aktuellen Benutzer entsprechen.

1. Öffnen `./src/app/auth.service.ts` Sie und fügen Sie `import` die folgende Anweisung am Anfang der Datei hinzu.

    ```TypeScript
    import { Client } from '@microsoft/microsoft-graph-client';
    ```

1. Fügen Sie eine neue Funktion zur `AuthService`-Klasse mit dem Namen `getUser` hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="getUserSnippet":::

1. Suchen und entfernen Sie den folgenden Code in `getAccessToken` der-Methode, die eine Warnung hinzufügt, um das Zugriffstoken anzuzeigen.

    ```TypeScript
    // Temporary to display token in an error box
    this.alertsService.add('Token acquired', result);
    ```

1. Suchen Sie den folgenden Code aus der `signIn` -Methode, und entfernen Sie ihn.

    ```TypeScript
    // Temporary placeholder
    this.user = new User();
    this.user.displayName = "Adele Vance";
    this.user.email = "AdeleV@contoso.com";
    ```

1. Fügen Sie an seiner Stelle den folgenden Code hinzu.

    ```TypeScript
    this.user = await this.getUser();
    ```

    In diesem neuen Code wird das Microsoft Graph-SDK verwendet, um die Details des Benutzers abzurufen `User` , und anschließend wird ein Objekt mithilfe der vom API-Aufruf zurückgegebenen Werte erstellt.

1. Ändern Sie `constructor` die für `AuthService` die-Klasse, um zu überprüfen, ob der Benutzer bereits angemeldet ist, und laden Sie die Details, wenn dies der Fall ist. Ersetzen Sie das `constructor` vorhandene durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/auth.service.ts" id="constructorSnippet" highlight="5-6":::

1. Entfernen Sie den temporären Code aus `HomeComponent` der-Klasse. Öffnen Sie `./src/app/home/home.component.ts` die Datei, und ersetzen `signIn` Sie die vorhandene Funktion durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/home/home.component.ts" id="signInSnippet" highlight="5-6":::

Wenn Sie nun Ihre Änderungen speichern und die app starten, sollten Sie nach der Anmeldung wieder auf der Startseite enden, aber die Benutzeroberfläche sollte sich ändern, um anzugeben, dass Sie angemeldet sind.

![Screenshot der Startseite nach dem Anmelden](./images/add-aad-auth-01.png)

Klicken Sie in der oberen rechten Ecke auf den Avatar des Benutzers, um auf den **Abmelde** Link zuzugreifen. Wenn Sie auf **Abmelden** klicken, wird die Sitzung zurückgesetzt und Sie kehren zur Startseite zurück.

![Screenshot des Dropdown-Menüs mit dem Link „Abmelden“](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Speichern und Aktualisieren von Token

Zu diesem Zeitpunkt verfügt Ihre Anwendung über ein Zugriffstoken, das in der `Authorization` Kopfzeile von API-aufrufen gesendet wird. Dies ist das Token, das es der App ermöglicht, im Namen des Benutzers auf Microsoft Graph zuzugreifen.

Dieses Token ist jedoch nur kurzzeitig verfügbar. Das Token läuft eine Stunde nach seiner Ausgabe ab. Da die APP die MSAL-Bibliothek verwendet, müssen Sie keine Token-Speicher-oder Aktualisierungslogik implementieren. Das `MsalService` Token wird im Browser Speicher zwischengespeichert. Die `acquireTokenSilent` Methode überprüft zuerst das zwischengespeicherte Token, und wenn es nicht abgelaufen ist, wird es zurückgegeben. Wenn es abgelaufen ist, wird eine automatische Anforderung an einen neuen abgerufen.
