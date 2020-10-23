<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="bd68d-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="bd68d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="bd68d-102">Für diese Anwendung verwenden Sie die [Microsoft-Graph-Client-](https://github.com/microsoftgraph/msgraph-sdk-javascript) Bibliothek, um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="bd68d-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="bd68d-103">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="bd68d-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="bd68d-104">Fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Anrufe zu halten.</span><span class="sxs-lookup"><span data-stu-id="bd68d-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="bd68d-105">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="bd68d-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="bd68d-106">Genau wie beim zuvor erstellten Authentifizierungsdienst können Sie einen Dienst für diese Komponente in alle Komponenten einfügen, die Zugriff auf Microsoft Graph benötigen.</span><span class="sxs-lookup"><span data-stu-id="bd68d-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="bd68d-107">Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/Graph.Service.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="bd68d-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Injectable } from '@angular/core';
    import { Client } from '@microsoft/microsoft-graph-client';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from './auth.service';
    import { AlertsService } from './alerts.service';

    @Injectable({
      providedIn: 'root'
    })

    export class GraphService {

      private graphClient: Client;
      constructor(
        private authService: AuthService,
        private alertsService: AlertsService) {

        // Initialize the Graph client
        this.graphClient = Client.init({
          authProvider: async (done) => {
            // Get the token from the auth service
            let token = await this.authService.getAccessToken()
              .catch((reason) => {
                done(reason, null);
              });

            if (token)
            {
              done(null, token);
            } else {
              done("Could not get an access token", null);
            }
          }
        });
      }

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[]> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          let result =  await this.graphClient
            .api('/me/calendarview')
            .header('Prefer', `outlook.timezone="${timeZone}"`)
            .query({
              startDateTime: start,
              endDateTime: end
            })
            .select('subject,organizer,start,end')
            .orderby('start/dateTime')
            .top(50)
            .get();

          return result.value;
        } catch (error) {
          this.alertsService.addError('Could not get events', JSON.stringify(error, null, 2));
        }
      }
    }
    ```

    <span data-ttu-id="bd68d-108">Überlegen Sie sich, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="bd68d-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="bd68d-109">Es initialisiert einen Graph-Client im Konstruktor für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="bd68d-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="bd68d-110">Es implementiert eine `getCalendarView` Funktion, die den Graph-Client wie folgt verwendet:</span><span class="sxs-lookup"><span data-stu-id="bd68d-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="bd68d-111">Die URL, die aufgerufen wird, lautet `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="bd68d-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="bd68d-112">Die `header` -Methode enthält die `Prefer: outlook.timezone` Kopfzeile, wodurch die Anfangs-und Endzeiten der zurückgegebenen Ereignisse in der bevorzugten Zeitzone des Benutzers liegen.</span><span class="sxs-lookup"><span data-stu-id="bd68d-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="bd68d-113">Die `query` -Methode fügt die `startDateTime` Parameter and hinzu und `endDateTime` definiert das Zeitfenster für die Kalenderansicht.</span><span class="sxs-lookup"><span data-stu-id="bd68d-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="bd68d-114">Die `select` -Methode schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf genau diejenigen ein, die die Ansicht tatsächlich verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bd68d-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="bd68d-115">Die `orderby` -Methode sortiert die Ergebnisse nach dem Startzeitpunkt.</span><span class="sxs-lookup"><span data-stu-id="bd68d-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="bd68d-116">Erstellen Sie eine Winkel Komponente zum Aufrufen dieser neuen Methode, und zeigen Sie die Ergebnisse des Anrufs an.</span><span class="sxs-lookup"><span data-stu-id="bd68d-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="bd68d-117">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="bd68d-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="bd68d-118">Nachdem der Befehl abgeschlossen ist, fügen Sie die Komponente dem `routes` Array in **./src/App/App-Routing.Module.TS**.</span><span class="sxs-lookup"><span data-stu-id="bd68d-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. <span data-ttu-id="bd68d-119">Öffnen Sie **./src/App/Calendar/Calendar.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="bd68d-119">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findOneIana } from 'windows-iana';
    import * as MicrosoftGraph from '@microsoft/microsoft-graph-types';

    import { AuthService } from '../auth.service';
    import { GraphService } from '../graph.service';
    import { AlertsService } from '../alerts.service';

    @Component({
      selector: 'app-calendar',
      templateUrl: './calendar.component.html',
      styleUrls: ['./calendar.component.css']
    })
    export class CalendarComponent implements OnInit {

      public events: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findOneIana(this.authService.user.timeZone);
        const timeZone = ianaName!.valueOf() || this.authService.user.timeZone;

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user.timeZone)
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

<span data-ttu-id="bd68d-120">Im Moment wird dadurch nur das Array von Ereignissen in JSON auf der Seite gerendert.</span><span class="sxs-lookup"><span data-stu-id="bd68d-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="bd68d-121">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="bd68d-121">Save your changes and restart the app.</span></span> <span data-ttu-id="bd68d-122">Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="bd68d-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="bd68d-123">Wenn alles funktioniert, sollte ein JSON-Abbild von Ereignissen im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="bd68d-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="bd68d-124">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="bd68d-124">Display the results</span></span>

<span data-ttu-id="bd68d-125">Jetzt können Sie die `CalendarComponent` Komponente aktualisieren, um die Ereignisse auf benutzerfreundlichere Weise anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="bd68d-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="bd68d-126">Entfernen Sie den temporären Code, der eine Warnung von der `ngOnInit` Funktion hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="bd68d-126">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="bd68d-127">Die aktualisierte Funktion sollte wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="bd68d-127">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="bd68d-128">Fügen Sie der-Klasse eine Funktion hinzu `CalendarComponent` , um ein `DateTimeTimeZone` Objekt in eine ISO-Zeichenfolge zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="bd68d-128">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="bd68d-129">Öffnen Sie **./src/App/Calendar/calendar.component.html** , und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="bd68d-129">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="bd68d-130">Dadurch wird die Auflistung von Ereignissen durchlaufen, und für jede einzelne Tabelle wird eine Tabellenzeile hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="bd68d-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="bd68d-131">Speichern Sie die Änderungen, und starten Sie die APP neu.</span><span class="sxs-lookup"><span data-stu-id="bd68d-131">Save the changes and restart the app.</span></span> <span data-ttu-id="bd68d-132">Klicken Sie auf den Link **Kalender** , und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.</span><span class="sxs-lookup"><span data-stu-id="bd68d-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
