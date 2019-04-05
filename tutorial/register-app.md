<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="7b981-101">In dieser Übung erstellen Sie eine neue Azure AD-Webanwendungs Registrierung mithilfe des Azure Active Directory Admin Center.</span><span class="sxs-lookup"><span data-stu-id="7b981-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="7b981-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7b981-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="7b981-103">Melden Sie sich über ein **persönliches Konto** (aka: Microsoft-Konto) oder ein Geschäfts- **oder Schulkonto**an.</span><span class="sxs-lookup"><span data-stu-id="7b981-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="7b981-104">Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App-Registrierungen (Vorschau)** unter **Manage**aus.</span><span class="sxs-lookup"><span data-stu-id="7b981-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="7b981-105">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="7b981-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="7b981-106">Wählen Sie **neue Registrierung**aus.</span><span class="sxs-lookup"><span data-stu-id="7b981-106">Select **New registration**.</span></span> <span data-ttu-id="7b981-107">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="7b981-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="7b981-108">Legen \*\*\*\* Sie Name `Angular Graph Tutorial`auf fest.</span><span class="sxs-lookup"><span data-stu-id="7b981-108">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="7b981-109">Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis und persönlichen Microsoft-Konten**fest.</span><span class="sxs-lookup"><span data-stu-id="7b981-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="7b981-110">Legen Sie unter umLeitungs- **URI**die erste Dropdown `Web` Liste auf fest, und `http://localhost:4200`legen Sie den Wert auf fest.</span><span class="sxs-lookup"><span data-stu-id="7b981-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Screenshot der Seite "Registrieren einer Anwendung"](./images/aad-register-an-app.png)

1. <span data-ttu-id="7b981-112">Wählen Sie **registrieren**aus.</span><span class="sxs-lookup"><span data-stu-id="7b981-112">Choose **Register**.</span></span> <span data-ttu-id="7b981-113">Kopieren Sie auf der Seite mit dem **eckigEn Graph-Lernprogramm** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="7b981-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. <span data-ttu-id="7b981-115">Wählen Sie unter **Manage**die Option **Authentication** aus.</span><span class="sxs-lookup"><span data-stu-id="7b981-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="7b981-116">Suchen Sie den **impliziten Grant** -Abschnitt, und aktivieren Sie **Zugriffstoken** und **ID-Token**.</span><span class="sxs-lookup"><span data-stu-id="7b981-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="7b981-117">Wählen Sie **Speichern** aus.</span><span class="sxs-lookup"><span data-stu-id="7b981-117">Choose **Save**.</span></span>

    ![Screenshot des impliziten Grant-Abschnitts](./images/aad-implicit-grant.png)
