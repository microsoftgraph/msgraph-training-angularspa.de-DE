<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="be4b7-101">In dieser Übung integrieren Sie Microsoft Graph in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be4b7-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="be4b7-102">Für diese Anwendung verwenden Sie die [Microsoft-Graph-Client-](https://github.com/microsoftgraph/msgraph-sdk-javascript) Bibliothek, um Aufrufe von Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="be4b7-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="be4b7-103">Abrufen von Kalenderereignissen aus Outlook</span><span class="sxs-lookup"><span data-stu-id="be4b7-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="be4b7-104">Erstellen Sie zunächst eine `Event` Klasse, die die Felder definiert, die von der App angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="be4b7-104">Start by creating an `Event` class that defines the fields that the app will display.</span></span> <span data-ttu-id="be4b7-105">Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `event.ts` dem Namen, und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="be4b7-105">Create a new file in the `./src/app` directory called `event.ts` and add the following code.</span></span>

```TypeScript
// For a full list of fields, see
// https://docs.microsoft.com/graph/api/resources/event?view=graph-rest-1.0
export class Event {
  subject: string;
  organizer: Recipient;
  start: DateTimeTimeZone;
  end: DateTimeTimeZone;
}

// https://docs.microsoft.com/graph/api/resources/recipient?view=graph-rest-1.0
export class Recipient {
  emailAddress: EmailAddress;
}

// https://docs.microsoft.com/graph/api/resources/emailaddress?view=graph-rest-1.0
export class EmailAddress {
  name: string;
  address: string;
}

// https://docs.microsoft.com/graph/api/resources/datetimetimezone?view=graph-rest-1.0
export class DateTimeTimeZone {
  dateTime: string;
  timeZone: string;
}
```

<span data-ttu-id="be4b7-106">Als Nächstes fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Aufrufe aufzunehmen.</span><span class="sxs-lookup"><span data-stu-id="be4b7-106">Next, add a new service to hold all of your Graph calls.</span></span> <span data-ttu-id="be4b7-107">Genau wie beim Authentifizierungsdienst, den Sie zuvor erstellt haben, ermöglicht das Erstellen eines Diensts für diese Anwendung das Einfügen in alle Komponenten, die Zugriff auf Microsoft Graph benötigen.</span><span class="sxs-lookup"><span data-stu-id="be4b7-107">Just as with the authentication service you created earlier, creating a service for this allows you to inject it into any components that need access to Microsoft Graph.</span></span> <span data-ttu-id="be4b7-108">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="be4b7-108">Run the following command in your CLI.</span></span>

```Shell
ng generate service graph
```

<span data-ttu-id="be4b7-109">Sobald der Befehl abgeschlossen ist, öffnen `./src/app/graph.service.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="be4b7-109">Once the command completes, open the `./src/app/graph.service.ts` file and replace its contents with the following.</span></span>

```TypeScript
import { Injectable } from '@angular/core';
import { Client } from '@microsoft/microsoft-graph-client';

import { AuthService } from './auth.service';
import { Event } from './event';
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

  async getEvents(): Promise<Event[]> {
    try {
      let result =  await this.graphClient
        .api('/me/events')
        .select('subject,organizer,start,end')
        .orderby('createdDateTime DESC')
        .get();

      return result.value;
    } catch (error) {
      this.alertsService.add('Could not get events', JSON.stringify(error, null, 2));
    }
  }
}
```

<span data-ttu-id="be4b7-110">Überlegen Sie sich, was dieser Code tut.</span><span class="sxs-lookup"><span data-stu-id="be4b7-110">Consider what this code is doing.</span></span>

- <span data-ttu-id="be4b7-111">Es initialisiert einen Graph-Client im Konstruktor für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="be4b7-111">It initializes a Graph client in the constructor for the service.</span></span>
- <span data-ttu-id="be4b7-112">Sie implementiert eine `getEvents` Funktion, die den Graph-Client auf folgende Weise verwendet:</span><span class="sxs-lookup"><span data-stu-id="be4b7-112">It implements a `getEvents` function that uses the Graph client in the following way:</span></span>
  - <span data-ttu-id="be4b7-113">Die URL, die aufgerufen wird, `/me/events`lautet.</span><span class="sxs-lookup"><span data-stu-id="be4b7-113">The URL that will be called is `/me/events`.</span></span>
  - <span data-ttu-id="be4b7-114">Die `select` -Methode schränkt die für jedes Ereignis zurückgegebenen Felder auf nur die ein, die die Ansicht tatsächlich verwendet.</span><span class="sxs-lookup"><span data-stu-id="be4b7-114">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
  - <span data-ttu-id="be4b7-115">Die `orderby` -Methode sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu denen Sie erstellt wurden, wobei das neueste Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="be4b7-115">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="be4b7-116">Erstellen Sie nun eine eckige Komponente, um diese neue Methode aufzurufen und die Ergebnisse des Aufrufs anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="be4b7-116">Now create an Angular component to call this new method and display the results of the call.</span></span> <span data-ttu-id="be4b7-117">Führen Sie den folgenden Befehl in der CLI aus.</span><span class="sxs-lookup"><span data-stu-id="be4b7-117">Run the following command in your CLI.</span></span>

```Shell
ng generate component calendar
```

<span data-ttu-id="be4b7-118">Sobald der Befehl abgeschlossen ist, fügen Sie die Komponente `routes` dem Array `./src/app/app-routing.module.ts`in hinzu.</span><span class="sxs-lookup"><span data-stu-id="be4b7-118">Once the command completes, add the component to the `routes` array in `./src/app/app-routing.module.ts`.</span></span>

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

<span data-ttu-id="be4b7-119">Öffnen Sie `./src/app/calendar/calendar.component.ts` die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="be4b7-119">Open the `./src/app/calendar/calendar.component.ts` file and replace its contents with the following.</span></span>

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

  private events: Event[];

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

<span data-ttu-id="be4b7-120">Im Moment wird dadurch nur das Array von Ereignissen in JSON auf der Seite gerendert.</span><span class="sxs-lookup"><span data-stu-id="be4b7-120">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="be4b7-121">Speichern Sie die Änderungen, und starten Sie die App neu.</span><span class="sxs-lookup"><span data-stu-id="be4b7-121">Save your changes and restart the app.</span></span> <span data-ttu-id="be4b7-122">Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** .</span><span class="sxs-lookup"><span data-stu-id="be4b7-122">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="be4b7-123">Wenn alles funktioniert, sollte ein JSON-Dump von Ereignissen im Kalender des Benutzers angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="be4b7-123">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="be4b7-124">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="be4b7-124">Display the results</span></span>

<span data-ttu-id="be4b7-125">Jetzt können Sie die `CalendarComponent` Komponente aktualisieren, um die Ereignisse benutzerfreundlicher anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="be4b7-125">Now you can update the `CalendarComponent` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="be4b7-126">Entfernen Sie zunächst den temporären Code, der eine Warnung aus der `ngOnInit` Funktion hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="be4b7-126">First, remove the temporary code that adds an alert from the `ngOnInit` function.</span></span> <span data-ttu-id="be4b7-127">Die aktualisierte Funktion sollte wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="be4b7-127">Your updated function should look like this.</span></span>

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

<span data-ttu-id="be4b7-128">Fügen Sie nun der `CalendarComponent` Klasse eine Funktion hinzu, um `DateTimeTimeZone` ein Objekt in eine ISO-Zeichenfolge zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="be4b7-128">Now add a function to the `CalendarComponent` class to format a `DateTimeTimeZone` object into an ISO string.</span></span>

```TypeScript
formatDateTimeTimeZone(dateTime: DateTimeTimeZone): string {
  try {
    return moment.tz(dateTime.dateTime, dateTime.timeZone).format();
  }
  catch(error) {
    this.alertsService.add('DateTimeTimeZone conversion error', JSON.stringify(error));
  }
}
```

<span data-ttu-id="be4b7-129">Öffnen Sie schließlich die `./src/app/calendar/calendar.component.html` Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="be4b7-129">Finally, open the `./src/app/calendar/calendar.component.html` file and replace its contents with the following.</span></span>

```html
<h1>Calendar</h1>
<table class="table">
  <thead>
    <th scope="col">Organizer</th>
    <th scope="col">Subject</th>
    <th scope="col">Start</th>
    <th scope="col">End</th>
  </thead>
  <tbody>
    <tr *ngFor="let event of events">
      <td>{{event.organizer.emailAddress.name}}</td>
      <td>{{event.subject}}</td>
      <td>{{formatDateTimeTimeZone(event.start) | date:'short' }}</td>
      <td>{{formatDateTimeTimeZone(event.end) | date: 'short' }}</td>
    </tr>
  </tbody>
</table>
```

<span data-ttu-id="be4b7-130">Dadurch wird die Auflistung der Ereignisse durchlaufen und für jeden eine Tabellenzeile hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="be4b7-130">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="be4b7-131">Speichern Sie die Änderungen, und starten Sie die APP neu.</span><span class="sxs-lookup"><span data-stu-id="be4b7-131">Save the changes and restart the app.</span></span> <span data-ttu-id="be4b7-132">Klicken Sie auf den Link **Kalender** , und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.</span><span class="sxs-lookup"><span data-stu-id="be4b7-132">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Screenshot der Ereignistabelle](./images/add-msgraph-01.png)