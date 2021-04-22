# <a name="how-to-run-the-completed-project"></a>Ausführen des abgeschlossenen Projekts

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen des abgeschlossenen Projekts in diesem Ordner benötigen Sie Folgendes:

- [Node.js](https://nodejs.org) auf Dem Entwicklungscomputer installiert. Wenn Sie nicht über Node.js, besuchen Sie den vorherigen Link für Downloadoptionen. (**Hinweis:** Dieses Lernprogramm wurde mit Node Version 14.15.0 geschrieben. Die Schritte in diesem Handbuch funktionieren möglicherweise mit anderen Versionen, aber dies wurde nicht getestet.)
- [Angular CLI,](https://cli.angular.io/) die auf Ihrem Entwicklungscomputer installiert ist.
- Entweder ein persönliches Microsoft-Konto mit einem Postfach Outlook.com oder ein Microsoft-Arbeits- oder Schulkonto.

Wenn Sie kein Microsoft-Konto haben, gibt es mehrere Optionen, um ein kostenloses Konto zu erhalten:

- Sie können [sich für ein neues persönliches Microsoft-Konto registrieren.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Sie können [sich für das Office 365-Entwicklerprogramm](https://developer.microsoft.com/office/dev-program) registrieren, um ein kostenloses Office 365-Abonnement zu erhalten.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrieren einer Webanwendung im Azure Active Directory Admin Center

1. Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.

1. Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.

    ![Screenshot der APP-Registrierungen ](/tutorial/images/aad-portal-app-registrations.png)

1. Wählen Sie **Neue Registrierung** aus. Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.

    - Legen Sie **Name** auf `Angular Graph Tutorial` fest.
    - Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.
    - Legen Sie unter **Umleitungs-URI** die erste Dropdownoption auf `Single-page application (SPA)` fest, und legen Sie den Wert auf `http://localhost:4200` fest.

    ![Screenshot der Seite "Anwendung registrieren"](/tutorial/images/aad-register-an-app.png)

1. Wählen Sie **Registrieren** aus. Kopieren Sie auf der Seite Angular **Graph Tutorial** den Wert der **Anwendungs-ID (Client-ID),** und speichern Sie sie, sie benötigen Sie im nächsten Schritt.

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a>Konfigurieren des Beispiels

1. Benennen Sie `oauth.ts.example` die Datei in `oauth.ts` um.
1. Bearbeiten Sie `oauth.ts` die Datei, und nehmen Sie die folgenden Änderungen vor.
    1. Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID,** die Sie über das App-Registrierungsportal erhalten haben.
1. Navigieren Sie in der Befehlszeilenschnittstelle (CLI) zu diesem Verzeichnis, und führen Sie den folgenden Befehl aus, um die Anforderungen zu installieren.

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a>Ausführen des Beispiels

1. Führen Sie den folgenden Befehl in Der CLI aus, um die Anwendung zu starten.

    ```Shell
    ng serve
    ```

1. Öffnen Sie einen Browser, und navigieren Sie zu `http://localhost:4200`.
