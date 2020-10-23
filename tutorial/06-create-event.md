<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e5d26-101">In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e5d26-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="e5d26-102">Öffnen Sie **/src/App/Graph.Service.TS** , und fügen Sie der-Klasse die folgende Funktion hinzu `GraphService` .</span><span class="sxs-lookup"><span data-stu-id="e5d26-102">Open **./src/app/graph.service.ts** and add the following function to the `GraphService` class.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a><span data-ttu-id="e5d26-103">Erstellen eines neuen Ereignis Formulars</span><span class="sxs-lookup"><span data-stu-id="e5d26-103">Create a new event form</span></span>

1. <span data-ttu-id="e5d26-104">Erstellen Sie eine Winkel Komponente, um ein Formular anzuzeigen und diese neue Funktion aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="e5d26-104">Create an Angular component to display a form and call this new function.</span></span> <span data-ttu-id="e5d26-105">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="e5d26-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component new-event
    ```

1. <span data-ttu-id="e5d26-106">Nachdem der Befehl abgeschlossen ist, fügen Sie die Komponente dem `routes` Array in **./src/App/App-Routing.Module.TS**.</span><span class="sxs-lookup"><span data-stu-id="e5d26-106">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. <span data-ttu-id="e5d26-107">Erstellen Sie eine neue Datei im **./src/App/New-Event** -Verzeichnis mit dem Namen **New-Event. TS** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="e5d26-107">Create a new file in the **./src/app/new-event** directory named **new-event.ts** and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    <span data-ttu-id="e5d26-108">Diese Klasse wird als Modell für das neue Ereignis Formular dienen.</span><span class="sxs-lookup"><span data-stu-id="e5d26-108">This class will serve as the model for the new event form.</span></span>

1. <span data-ttu-id="e5d26-109">Öffnen Sie **./src/App/New-Event/New-Event.Component.TS** , und ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="e5d26-109">Open **./src/app/new-event/new-event.component.ts** and replace its contents with the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. <span data-ttu-id="e5d26-110">Öffnen Sie **./src/App/New-Event/new-event.component.html** , und ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="e5d26-110">Open **./src/app/new-event/new-event.component.html** and replace its contents with the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. <span data-ttu-id="e5d26-111">Speichern Sie die Änderungen, und aktualisieren Sie die app.</span><span class="sxs-lookup"><span data-stu-id="e5d26-111">Save the changes and refresh the app.</span></span> <span data-ttu-id="e5d26-112">Wählen Sie auf der Kalender Seite die Schaltfläche **Neues Ereignis** aus, und erstellen Sie dann mithilfe des Formulars ein Ereignis im Kalender des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="e5d26-112">Select the **New event** button on the calendar page, then use the form to create an event on the user's calendar.</span></span>

    ![Screenshot des neuen Ereignis Formulars](images/create-event.png)
