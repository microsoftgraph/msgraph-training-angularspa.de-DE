# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="82f99-101">Ausführen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="82f99-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82f99-102">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="82f99-102">Prerequisites</span></span>

<span data-ttu-id="82f99-103">Zum Ausführen des abgeschlossenen Projekts in diesem Ordner benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="82f99-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="82f99-104">[Node. js](https://nodejs.org) ist auf dem Entwicklungscomputer installiert.</span><span class="sxs-lookup"><span data-stu-id="82f99-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="82f99-105">Wenn Sie nicht über Node. js verfügen, besuchen Sie den vorherigen Link für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="82f99-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="82f99-106">(**Hinweis:** dieses Tutorial wurde mit Node Version 10.7.0 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="82f99-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="82f99-107">Die Schritte in diesem Handbuch funktionieren möglicherweise mit anderen Versionen, die jedoch nicht getestet wurden.)</span><span class="sxs-lookup"><span data-stu-id="82f99-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="82f99-108">[Eckige CLI](https://cli.angular.io/) , die auf dem Entwicklungscomputer installiert ist.</span><span class="sxs-lookup"><span data-stu-id="82f99-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="82f99-109">Entweder ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Geschäfts-oder Schulkonto.</span><span class="sxs-lookup"><span data-stu-id="82f99-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="82f99-110">Wenn Sie nicht über ein Microsoft-Konto verfügen, gibt es eine Reihe von Optionen, um ein kostenloses Konto zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="82f99-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="82f99-111">Sie können [sich für ein neues persönliches Microsoft-Konto registrieren](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="82f99-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="82f99-112">Sie können [sich für das office 365-Entwicklerprogramm anmelden](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="82f99-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="82f99-113">Registrieren einer Webanwendung mit dem Azure Active Directory Admin Center</span><span class="sxs-lookup"><span data-stu-id="82f99-113">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="82f99-114">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="82f99-114">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="82f99-115">Melden Sie sich über ein **persönliches Konto** (aka: Microsoft-Konto) oder ein Geschäfts- **oder Schulkonto**an.</span><span class="sxs-lookup"><span data-stu-id="82f99-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="82f99-116">Wählen Sie **Azure Active Directory** in der linken Navigationsleiste aus, und wählen Sie dann **App-Registrierungen (Vorschau)** unter **Manage**aus.</span><span class="sxs-lookup"><span data-stu-id="82f99-116">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="82f99-117">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="82f99-117">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="82f99-118">Wählen Sie **neue Registrierung**aus.</span><span class="sxs-lookup"><span data-stu-id="82f99-118">Select **New registration**.</span></span> <span data-ttu-id="82f99-119">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="82f99-119">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="82f99-120">Legen \*\*\*\* Sie Name `Angular Graph Tutorial`auf fest.</span><span class="sxs-lookup"><span data-stu-id="82f99-120">Set **Name** to `Angular Graph Tutorial`.</span></span>
    - <span data-ttu-id="82f99-121">Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis und persönlichen Microsoft-Konten**fest.</span><span class="sxs-lookup"><span data-stu-id="82f99-121">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="82f99-122">Legen Sie unter umLeitungs- **URI**die erste Dropdown `Web` Liste auf fest, und `http://localhost:4200`legen Sie den Wert auf fest.</span><span class="sxs-lookup"><span data-stu-id="82f99-122">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:4200`.</span></span>

    ![Screenshot der Seite "Registrieren einer Anwendung"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="82f99-124">Wählen Sie **registrieren**aus.</span><span class="sxs-lookup"><span data-stu-id="82f99-124">Choose **Register**.</span></span> <span data-ttu-id="82f99-125">Kopieren Sie auf der Seite mit dem **eckigEn Graph-Lernprogramm** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, dann benötigen Sie ihn im nächsten Schritt.</span><span class="sxs-lookup"><span data-stu-id="82f99-125">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="82f99-127">Wählen Sie unter **Manage**die Option **Authentication** aus.</span><span class="sxs-lookup"><span data-stu-id="82f99-127">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="82f99-128">Suchen Sie den **impliziten Grant** -Abschnitt, und aktivieren Sie **Zugriffstoken** und **ID-Token**.</span><span class="sxs-lookup"><span data-stu-id="82f99-128">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="82f99-129">Wählen Sie **Speichern** aus.</span><span class="sxs-lookup"><span data-stu-id="82f99-129">Choose **Save**.</span></span>

    ![Screenshot des impliziten Grant-Abschnitts](/tutorial/images/aad-implicit-grant.png)

## <a name="configure-the-sample"></a><span data-ttu-id="82f99-131">Konfigurieren des Beispiels</span><span class="sxs-lookup"><span data-stu-id="82f99-131">Configure the sample</span></span>

1. <span data-ttu-id="82f99-132">Benennen Sie `oauth.ts.example` die Datei `oauth.ts`in um.</span><span class="sxs-lookup"><span data-stu-id="82f99-132">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="82f99-133">Bearbeiten Sie `oauth.ts` die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="82f99-133">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="82f99-134">Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="82f99-134">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="82f99-135">Navigieren Sie in der Befehlszeilenschnittstelle zu diesem Verzeichnis, und führen Sie den folgenden Befehl aus, um die Anforderungen zu installieren.</span><span class="sxs-lookup"><span data-stu-id="82f99-135">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="82f99-136">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="82f99-136">Run the sample</span></span>

1. <span data-ttu-id="82f99-137">Führen Sie den folgenden Befehl in der CLI aus, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="82f99-137">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="82f99-138">Öffnen Sie einen Browser, und navigieren Sie zu `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="82f99-138">Open a browser and browse to `http://localhost:4200`.</span></span>