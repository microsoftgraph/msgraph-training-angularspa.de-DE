<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a34aa-101">In dieser Übung erstellen Sie eine neue Azure AD-Webanwendungsregistrierung mit dem Azure Active Directory Admin Center.</span><span class="sxs-lookup"><span data-stu-id="a34aa-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="a34aa-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a34aa-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="a34aa-103">Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="a34aa-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="a34aa-104">Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="a34aa-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="a34aa-105">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="a34aa-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="a34aa-106">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="a34aa-106">Select **New registration**.</span></span> <span data-ttu-id="a34aa-107">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="a34aa-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="a34aa-108">Legen Sie **Name** auf `Angular Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="a34aa-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="a34aa-109">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="a34aa-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="a34aa-110">Legen Sie unter **Umleitungs-URI** die erste Dropdownoption auf `Single-page application (SPA)` fest, und legen Sie den Wert auf `http://localhost:4200` fest.</span><span class="sxs-lookup"><span data-stu-id="a34aa-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `http://localhost:4200`.</span></span>

    ![Screenshot der Seite "Anwendung registrieren"](./images/aad-register-an-app.png)

1. <span data-ttu-id="a34aa-112">Wählen Sie **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="a34aa-112">Select **Register**.</span></span> <span data-ttu-id="a34aa-113">Kopieren Sie auf der Seite Angular **Graph Tutorial** den Wert der **Anwendungs-ID (Client-ID),** und speichern Sie sie, sie benötigen Sie im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="a34aa-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)
