<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="3b26d-101">In dieser Übung integrieren Sie Microsoft Graph in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3b26d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="3b26d-102">Für diese Anwendung verwenden Sie die [microsoft-graph-client-Bibliothek,](https://github.com/microsoftgraph/msgraph-sdk-javascript) um Aufrufe an Microsoft Graph zu senden.</span><span class="sxs-lookup"><span data-stu-id="3b26d-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="3b26d-103">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="3b26d-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="3b26d-104">Fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Aufrufe zu halten.</span><span class="sxs-lookup"><span data-stu-id="3b26d-104">Add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="3b26d-105">Führen Sie den folgenden Befehl in Ihrer CLI aus.</span><span class="sxs-lookup"><span data-stu-id="3b26d-105">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate service graph
    ```

    <span data-ttu-id="3b26d-106">Genau wie bei dem zuvor erstellten Authentifizierungsdienst können Sie einen dienst hierinjizieren, um ihn in alle Komponenten zu injizieren, die Zugriff auf Microsoft Graph benötigen.</span><span class="sxs-lookup"><span data-stu-id="3b26d-106">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span>

1. <span data-ttu-id="3b26d-107">Öffnen Sie nach Abschluss des Befehls **./src/app/graph.service.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="3b26d-107">Once the command completes, open **./src/app/graph.service.ts** and replace its contents with the following.</span></span>

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
            const token = await this.authService.getAccessToken()
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

      async getCalendarView(start: string, end: string, timeZone: string): Promise<MicrosoftGraph.Event[] | undefined> {
        try {
          // GET /me/calendarview?startDateTime=''&endDateTime=''
          // &$select=subject,organizer,start,end
          // &$orderby=start/dateTime
          // &$top=50
          const result =  await this.graphClient
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
        return undefined;
      }
    }
    ```

    <span data-ttu-id="3b26d-108">Überlegen Sie sich, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="3b26d-108">Consider what this code is doing.</span></span>

    - <span data-ttu-id="3b26d-109">Es initialisiert einen Graph-Client im Konstruktor für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="3b26d-109">It initializes a Graph client in the constructor for the service.</span></span>
    - <span data-ttu-id="3b26d-110">Es implementiert eine `getCalendarView` Funktion, die den Graph-Client auf folgende Weise verwendet:</span><span class="sxs-lookup"><span data-stu-id="3b26d-110">It implements a `getCalendarView` function that uses the Graph client in the following way:</span></span>
      - <span data-ttu-id="3b26d-111">Die URL, die aufgerufen wird, lautet `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="3b26d-111">The URL that will be called is `/me/calendarview`.</span></span>
      - <span data-ttu-id="3b26d-112">Die -Methode enthält den Header, wodurch die Start- und Endzeiten der zurückgegebenen Ereignisse in der bevorzugten Zeitzone des Benutzers `header` `Prefer: outlook.timezone` enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="3b26d-112">The `header` method includes the `Prefer: outlook.timezone` header, which causes the start and end times of the returned events to be in the user's preferred time zone.</span></span>
      - <span data-ttu-id="3b26d-113">Die -Methode fügt die Parameter and hinzu und definiert das `query` `startDateTime` `endDateTime` Zeitfenster für die Kalenderansicht.</span><span class="sxs-lookup"><span data-stu-id="3b26d-113">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
      - <span data-ttu-id="3b26d-114">Die Methode beschränkt die für jedes Ereignis zurückgegebenen Felder auf die Felder, die `select` von der Ansicht tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3b26d-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
      - <span data-ttu-id="3b26d-115">Die `orderby` Methode sortiert die Ergebnisse nach Startzeit.</span><span class="sxs-lookup"><span data-stu-id="3b26d-115">The `orderby` method sorts the results by start time.</span></span>

1. <span data-ttu-id="3b26d-116">Erstellen Sie eine Angular-Komponente, um diese neue Methode auf aufruft und die Ergebnisse des Aufrufs anzeigen.</span><span class="sxs-lookup"><span data-stu-id="3b26d-116">Create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="3b26d-117">Führen Sie den folgenden Befehl in Ihrer CLI aus.</span><span class="sxs-lookup"><span data-stu-id="3b26d-117">Run the following command in your CLI.</span></span>

    ```Shell
    ng generate component calendar
    ```

1. <span data-ttu-id="3b26d-118">Fügen Sie nach Abschluss des Befehls die Komponente dem `routes` Array in **./src/app/app-routing.module.ts hinzu.**</span><span class="sxs-lookup"><span data-stu-id="3b26d-118">Once the command completes, add the component to the `routes` array in **./src/app/app-routing.module.ts**.</span></span>

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. <span data-ttu-id="3b26d-119">Öffnen **Sie ./tsconfig.jsein,** und fügen Sie dem Objekt die folgende Eigenschaft `compilerOptions` hinzu.</span><span class="sxs-lookup"><span data-stu-id="3b26d-119">Open **./tsconfig.json** and add the following property to the `compilerOptions` object.</span></span>

    ```json
    "resolveJsonModule": true
    ```

1. <span data-ttu-id="3b26d-120">Öffnen **Sie ./src/app/calendar/calendar.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="3b26d-120">Open **./src/app/calendar/calendar.component.ts** and replace its contents with the following.</span></span>

    ```typescript
    import { Component, OnInit } from '@angular/core';
    import * as moment from 'moment-timezone';
    import { findIana } from 'windows-iana';
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

      public events?: MicrosoftGraph.Event[];

      constructor(
        private authService: AuthService,
        private graphService: GraphService,
        private alertsService: AlertsService) { }

      ngOnInit() {
        // Convert the user's timezone to IANA format
        const ianaName = findIana(this.authService.user?.timeZone ?? 'UTC');
        const timeZone = ianaName![0].valueOf() || this.authService.user?.timeZone || 'UTC';

        // Get midnight on the start of the current week in the user's timezone,
        // but in UTC. For example, for Pacific Standard Time, the time value would be
        // 07:00:00Z
        var startOfWeek = moment.tz(timeZone).startOf('week').utc();
        var endOfWeek = moment(startOfWeek).add(7, 'day');

        this.graphService.getCalendarView(
          startOfWeek.format(),
          endOfWeek.format(),
          this.authService.user?.timeZone ?? 'UTC')
            .then((events) => {
              this.events = events;
              // Temporary to display raw results
              this.alertsService.addSuccess('Events from Graph', JSON.stringify(events, null, 2));
            });
      }
    }
    ```

<span data-ttu-id="3b26d-121">Damit wird derzeit nur das Array von Ereignissen in JSON auf der Seite gerendert.</span><span class="sxs-lookup"><span data-stu-id="3b26d-121">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="3b26d-122">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="3b26d-122">Save your changes and restart the app.</span></span> <span data-ttu-id="3b26d-123">Melden Sie sich an, **und** klicken Sie in der Navigationsleiste auf den Link Kalender.</span><span class="sxs-lookup"><span data-stu-id="3b26d-123">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="3b26d-124">Wenn alles funktioniert, sollte ein JSON-Abbild von Ereignissen im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="3b26d-124">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="3b26d-125">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="3b26d-125">Display the results</span></span>

<span data-ttu-id="3b26d-126">Jetzt können Sie die Komponente so aktualisieren, dass die Ereignisse `CalendarComponent` benutzerfreundlicher angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="3b26d-126">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="3b26d-127">Entfernen Sie den temporären Code, der eine Warnung aus der Funktion `ngOnInit` hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="3b26d-127">Remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="3b26d-128">Ihre aktualisierte Funktion sollte wie dies aussehen.</span><span class="sxs-lookup"><span data-stu-id="3b26d-128">Your updated function should look like this.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. <span data-ttu-id="3b26d-129">Fügen Sie der Klasse eine Funktion `CalendarComponent` hinzu, um ein `DateTimeTimeZone` Objekt in eine ISO-Zeichenfolge zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="3b26d-129">Add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. <span data-ttu-id="3b26d-130">Öffnen **Sie ./src/app/calendar/calendar.component.html,** und ersetzen Sie den Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="3b26d-130">Open **./src/app/calendar/calendar.component.html** and replace its contents with the following.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

<span data-ttu-id="3b26d-131">Dadurch wird die Auflistung von Ereignissen in einer Schleife durchlauft und für jedes Ereignis eine Tabellenzeile hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="3b26d-131">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="3b26d-132">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="3b26d-132">Save the changes and restart the app.</span></span> <span data-ttu-id="3b26d-133">Klicken Sie auf den **Link Kalender,** und die App sollte nun eine Tabelle mit Ereignissen rendern.</span><span class="sxs-lookup"><span data-stu-id="3b26d-133">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
