<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="da92f-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="da92f-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="da92f-102">Für diese Anwendung verwenden Sie die [Microsoft-Graph-Client-](https://github.com/microsoftgraph/msgraph-sdk-javascript) Bibliothek, um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="da92f-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="da92f-103">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="da92f-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="da92f-104">Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `event.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="da92f-104">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. <span data-ttu-id="da92f-105">Fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Anrufe zu halten.</span><span class="sxs-lookup"><span data-stu-id="da92f-105">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="da92f-106">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="da92f-106">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="da92f-107">Genau wie beim zuvor erstellten Authentifizierungsdienst können Sie einen Dienst für diese Komponente in alle Komponenten einfügen, die Zugriff auf Microsoft Graph benötigen.</span><span class="sxs-lookup"><span data-stu-id="da92f-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="da92f-108">Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/graph.service.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="da92f-108">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    <span data-ttu-id="da92f-109">Überlegen Sie sich, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="da92f-109">Consider what this code is doing.</span></span>

    - <span data-ttu-id="da92f-110">Es initialisiert einen Graph-Client im Konstruktor für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="da92f-110">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="da92f-111">Es implementiert eine `getEvents` Funktion, die den Graph-Client wie folgt verwendet:</span><span class="sxs-lookup"><span data-stu-id="da92f-111">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="da92f-112">Die URL, die aufgerufen wird, lautet `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="da92f-112">The URL that will be called is `/me/events`.</span></span>
      - <span data-ttu-id="da92f-113">Die `select` -Methode schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf genau diejenigen ein, die die Ansicht tatsächlich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="da92f-113">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="da92f-114">Die `orderby` Methode sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu der Sie erstellt wurden, wobei das letzte Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="da92f-114">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="da92f-115">Erstellen Sie eine Winkel Komponente zum Aufrufen dieser neuen Methode, und zeigen Sie die Ergebnisse des Anrufs an.</span><span class="sxs-lookup"><span data-stu-id="da92f-115">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="da92f-116">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="da92f-116">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="da92f-117">Nachdem der Befehl abgeschlossen ist, fügen Sie die Komponente `routes` dem Array `./src/app/app-routing.module.ts`in hinzu.</span><span class="sxs-lookup"><span data-stu-id="da92f-117">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="da92f-118">Öffnen Sie `./src/app/calendar/calendar.component.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="da92f-118">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

    ```TypeScript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';

    import { GraphService } from '../graph.service';
    import { Event, DateTimeTimeZone } from '../event';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: Event[];

      constructor(
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        this.graphService.getEvents()
          .then((events) => {
            this.events = events;
            // Temporary to display raw results
            this.alertsService.add('Events from Graph', JSON.stringify(events, null, 2));
          });
      }
    }
    ```

<span data-ttu-id="da92f-119">Im Moment wird dadurch nur das Array von Ereignissen in JSON auf der Seite gerendert.</span><span class="sxs-lookup"><span data-stu-id="da92f-119">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="da92f-120">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="da92f-120">Save your changes and restart the app.</span></span> <span data-ttu-id="da92f-121">Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="da92f-121">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="da92f-122">Wenn alles funktioniert, sollte ein JSON-Abbild von Ereignissen im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="da92f-122">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="da92f-123">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="da92f-123">Display the results</span></span>

<span data-ttu-id="da92f-124">Jetzt können Sie die `CalendarComponent` Komponente aktualisieren, um die Ereignisse auf benutzerfreundlichere Weise anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="da92f-124">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="da92f-125">Entfernen Sie den temporären Code, der eine Warnung von `ngOnInit` der Funktion hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="da92f-125">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="da92f-126">Die aktualisierte Funktion sollte wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="da92f-126">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="da92f-127">Fügen Sie der `CalendarComponent` -Klasse eine Funktion hinzu, `DateTimeTimeZone` um ein Objekt in eine ISO-Zeichenfolge zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="da92f-127">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="da92f-128">Öffnen Sie `./src/app/calendar/calendar.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="da92f-128">Open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="da92f-129">Dadurch wird die Auflistung von Ereignissen durchlaufen, und für jede einzelne Tabelle wird eine Tabellenzeile hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="da92f-129">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="da92f-130">Speichern Sie die Änderungen, und starten Sie die APP neu.</span><span class="sxs-lookup"><span data-stu-id="da92f-130">Save the changes and restart the app.</span></span> <span data-ttu-id="da92f-131">Klicken Sie auf den Link **Kalender** , und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.</span><span class="sxs-lookup"><span data-stu-id="da92f-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
