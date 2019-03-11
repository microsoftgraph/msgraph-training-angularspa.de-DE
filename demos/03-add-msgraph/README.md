# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="b74ab-101">Ausführen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="b74ab-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b74ab-102">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="b74ab-102">Prerequisites</span></span>

<span data-ttu-id="b74ab-103">Zum Ausführen des abgeschlossenen Projekts in diesem Ordner benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b74ab-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="b74ab-104">[Node. js](https://nodejs.org) ist auf dem Entwicklungscomputer installiert.</span><span class="sxs-lookup"><span data-stu-id="b74ab-104">[Node.js](https://nodejs.org) installed on your development machine.</span></span> <span data-ttu-id="b74ab-105">Wenn Sie nicht über Node. js verfügen, besuchen Sie den vorherigen Link für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="b74ab-105">If you do not have Node.js, visit the previous link for download options.</span></span> <span data-ttu-id="b74ab-106">(**Hinweis:** dieses Tutorial wurde mit Node Version 10.7.0 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="b74ab-106">(**Note:** This tutorial was written with Node version 10.7.0.</span></span> <span data-ttu-id="b74ab-107">Die Schritte in diesem Handbuch funktionieren möglicherweise mit anderen Versionen, die jedoch nicht getestet wurden.)</span><span class="sxs-lookup"><span data-stu-id="b74ab-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="b74ab-108">[Eckige CLI](https://cli.angular.io/) , die auf dem Entwicklungscomputer installiert ist.</span><span class="sxs-lookup"><span data-stu-id="b74ab-108">[Angular CLI](https://cli.angular.io/) installed on your development machine.</span></span>
- <span data-ttu-id="b74ab-109">Entweder ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Geschäfts-oder Schulkonto.</span><span class="sxs-lookup"><span data-stu-id="b74ab-109">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="b74ab-110">Wenn Sie nicht über ein Microsoft-Konto verfügen, gibt es eine Reihe von Optionen, um ein kostenloses Konto zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="b74ab-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="b74ab-111">Sie können [sich für ein neues persönliches Microsoft-Konto registrieren](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="b74ab-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="b74ab-112">Sie können [sich für das office 365-Entwicklerprogramm anmelden](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b74ab-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="b74ab-113">Registrieren einer Webanwendung mit dem Anwendungs Registrierungs Portal</span><span class="sxs-lookup"><span data-stu-id="b74ab-113">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="b74ab-114">Öffnen Sie einen Browser, und navigieren Sie zum [Anwendungs Registrierungs Portal](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="b74ab-114">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="b74ab-115">Melden Sie sich über ein **persönliches Konto** (aka: Microsoft-Konto) oder ein Geschäfts- **oder Schulkonto**an.</span><span class="sxs-lookup"><span data-stu-id="b74ab-115">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="b74ab-116">Wählen Sie oben auf der Seite **eine APP hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="b74ab-116">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="b74ab-117">**Hinweis:** Wenn auf der Seite mehr als eine Schaltfläche **app hinzufügen** angezeigt wird, wählen Sie diejenige aus, die der Liste **konvergierter apps** entspricht.</span><span class="sxs-lookup"><span data-stu-id="b74ab-117">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="b74ab-118">Legen Sie auf der Seite **Anwendung registrieren** den **Anwendungsnamen** auf **eckige Graph-Lernprogramm** fest, und wählen Sie **Erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="b74ab-118">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Screenshot des Erstellens einer neuen app in der APP-Registrierungs Portal-Website](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="b74ab-120">Kopieren Sie auf der Seite " **Winkel Graph Tutorial-Registrierung** " unter dem Abschnitt " **Eigenschaften** " die **Anwendungs-ID** , so wie Sie Sie später benötigen.</span><span class="sxs-lookup"><span data-stu-id="b74ab-120">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Screenshot der neu erstellten Anwendungs-ID](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="b74ab-122">Scrollen Sie nach unten zum Abschnitt **Plattformen** .</span><span class="sxs-lookup"><span data-stu-id="b74ab-122">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="b74ab-123">Wählen Sie **Plattform hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="b74ab-123">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="b74ab-124">Wählen Sie im Dialogfeld **Plattform hinzufügen** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="b74ab-124">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Screenshot Erstellen einer Plattform für die APP](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="b74ab-126">Geben Sie \*\*\*\* im Feld Webplattform die URL `http://localhost:4200` für die Umleitungs- **URLs**ein.</span><span class="sxs-lookup"><span data-stu-id="b74ab-126">In the **Web** platform box, enter the URL `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Screenshot der neu hinzugefügten Webplattform für die Anwendung](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="b74ab-128">Scrollen Sie zum unteren Rand der Seite, und wählen Sie **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="b74ab-128">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="b74ab-129">Konfigurieren des Beispiels</span><span class="sxs-lookup"><span data-stu-id="b74ab-129">Configure the sample</span></span>

1. <span data-ttu-id="b74ab-130">Benennen Sie `oauth.ts.example` die Datei `oauth.ts`in um.</span><span class="sxs-lookup"><span data-stu-id="b74ab-130">Rename the `oauth.ts.example` file to `oauth.ts`.</span></span>
1. <span data-ttu-id="b74ab-131">Bearbeiten Sie `oauth.ts` die Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="b74ab-131">Edit the `oauth.ts` file and make the following changes.</span></span>
    1. <span data-ttu-id="b74ab-132">Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="b74ab-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="b74ab-133">Navigieren Sie in der Befehlszeilenschnittstelle zu diesem Verzeichnis, und führen Sie den folgenden Befehl aus, um die Anforderungen zu installieren.</span><span class="sxs-lookup"><span data-stu-id="b74ab-133">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="b74ab-134">Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="b74ab-134">Run the sample</span></span>

1. <span data-ttu-id="b74ab-135">Führen Sie den folgenden Befehl in der CLI aus, um die Anwendung zu starten.</span><span class="sxs-lookup"><span data-stu-id="b74ab-135">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    ng serve
    ```

1. <span data-ttu-id="b74ab-136">Öffnen Sie einen Browser, und navigieren Sie zu `http://localhost:4200`.</span><span class="sxs-lookup"><span data-stu-id="b74ab-136">Open a browser and browse to `http://localhost:4200`.</span></span>