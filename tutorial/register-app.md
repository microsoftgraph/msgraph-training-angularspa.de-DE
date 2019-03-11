<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4a377-101">In dieser Übung erstellen Sie eine neue Azure AD-Webanwendungs Registrierung mithilfe des Application Registry Portal (ARP).</span><span class="sxs-lookup"><span data-stu-id="4a377-101">In this exercise, you will create a new Azure AD web application registration using the Application Registry Portal (ARP).</span></span>

1. <span data-ttu-id="4a377-102">Öffnen Sie einen Browser, und navigieren Sie zum [Anwendungs Registrierungs Portal](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="4a377-102">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="4a377-103">Melden Sie sich über ein **persönliches Konto** (aka: Microsoft-Konto) oder ein Geschäfts- **oder Schulkonto**an.</span><span class="sxs-lookup"><span data-stu-id="4a377-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="4a377-104">Wählen Sie oben auf der Seite **eine APP hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="4a377-104">Select **Add an app** at the top of the page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4a377-105">Wenn auf der Seite mehr als eine Schaltfläche **app hinzufügen** angezeigt wird, wählen Sie diejenige aus, die der Liste **konvergierter apps** entspricht.</span><span class="sxs-lookup"><span data-stu-id="4a377-105">If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="4a377-106">Legen Sie auf der Seite **Anwendung registrieren** den **Anwendungsnamen** auf **eckige Graph-Lernprogramm** fest, und wählen Sie **Erstellen**aus.</span><span class="sxs-lookup"><span data-stu-id="4a377-106">On the **Register your application** page, set the **Application Name** to **Angular Graph Tutorial** and select **Create**.</span></span>

    ![Screenshot des Erstellens einer neuen app in der APP-Registrierungs Portal-Website](./images/arp-create-app-01.png)

1. <span data-ttu-id="4a377-108">Kopieren Sie auf der Seite " **Winkel Graph Tutorial-Registrierung** " unter dem Abschnitt " **Eigenschaften** " die **Anwendungs-ID** , so wie Sie Sie später benötigen.</span><span class="sxs-lookup"><span data-stu-id="4a377-108">On the **Angular Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Screenshot der neu erstellten Anwendungs-ID](./images/arp-create-app-02.png)

1. <span data-ttu-id="4a377-110">Scrollen Sie nach unten zum Abschnitt **Plattformen** .</span><span class="sxs-lookup"><span data-stu-id="4a377-110">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="4a377-111">Wählen Sie **Plattform hinzufügen**aus.</span><span class="sxs-lookup"><span data-stu-id="4a377-111">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="4a377-112">Wählen Sie im Dialogfeld **Plattform hinzufügen** die Option **Web**aus.</span><span class="sxs-lookup"><span data-stu-id="4a377-112">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Screenshot Erstellen einer Plattform für die APP](./images/arp-create-app-03.png)

    1. <span data-ttu-id="4a377-114">Geben Sie \*\*\*\* im Feld Webplattform für `http://localhost:4200` die Umleitungs- **URLs**ein.</span><span class="sxs-lookup"><span data-stu-id="4a377-114">In the **Web** platform box, enter `http://localhost:4200` for the **Redirect URLs**.</span></span>

        ![Screenshot der neu hinzugefügten Webplattform für die Anwendung](./images/arp-create-app-04.png)

1. <span data-ttu-id="4a377-116">Scrollen Sie zum unteren Rand der Seite, und wählen Sie **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="4a377-116">Scroll to the bottom of the page and select **Save**.</span></span>