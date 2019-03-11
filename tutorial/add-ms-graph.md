<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung integrieren Sie Microsoft Graph in die Anwendung. Für diese Anwendung verwenden Sie die [Microsoft-Graph-Client-](https://github.com/microsoftgraph/msgraph-sdk-javascript) Bibliothek, um Aufrufe von Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen aus Outlook

Erstellen Sie zunächst eine `Event` Klasse, die die Felder definiert, die von der App angezeigt werden. Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `event.ts` dem Namen, und fügen Sie den folgenden Code hinzu.

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

Als Nächstes fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Aufrufe aufzunehmen. Genau wie beim Authentifizierungsdienst, den Sie zuvor erstellt haben, ermöglicht das Erstellen eines Diensts für diese Anwendung das Einfügen in alle Komponenten, die Zugriff auf Microsoft Graph benötigen. Führen Sie den folgenden Befehl in der CLI aus.

```Shell
ng generate service graph
```

Sobald der Befehl abgeschlossen ist, öffnen `./src/app/graph.service.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Überlegen Sie sich, was dieser Code tut.

- Es initialisiert einen Graph-Client im Konstruktor für den Dienst.
- Sie implementiert eine `getEvents` Funktion, die den Graph-Client auf folgende Weise verwendet:
  - Die URL, die aufgerufen wird, `/me/events`lautet.
  - Die `select` -Methode schränkt die für jedes Ereignis zurückgegebenen Felder auf nur die ein, die die Ansicht tatsächlich verwendet.
  - Die `orderby` -Methode sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu denen Sie erstellt wurden, wobei das neueste Element zuerst angezeigt wird.

Erstellen Sie nun eine eckige Komponente, um diese neue Methode aufzurufen und die Ergebnisse des Aufrufs anzuzeigen. Führen Sie den folgenden Befehl in der CLI aus.

```Shell
ng generate component calendar
```

Sobald der Befehl abgeschlossen ist, fügen Sie die Komponente `routes` dem Array `./src/app/app-routing.module.ts`in hinzu.

```TypeScript
import { CalendarComponent } from './calendar/calendar.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'calendar', component: CalendarComponent }
];
```

Öffnen Sie `./src/app/calendar/calendar.component.ts` die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Im Moment wird dadurch nur das Array von Ereignissen in JSON auf der Seite gerendert. Speichern Sie die Änderungen, und starten Sie die App neu. Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** . Wenn alles funktioniert, sollte ein JSON-Dump von Ereignissen im Kalender des Benutzers angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Jetzt können Sie die `CalendarComponent` Komponente aktualisieren, um die Ereignisse benutzerfreundlicher anzuzeigen. Entfernen Sie zunächst den temporären Code, der eine Warnung aus der `ngOnInit` Funktion hinzufügt. Die aktualisierte Funktion sollte wie folgt aussehen.

```TypeScript
ngOnInit() {
  this.graphService.getEvents()
    .then((events) => {
      this.events = events;
    });
}
```

Fügen Sie nun der `CalendarComponent` Klasse eine Funktion hinzu, um `DateTimeTimeZone` ein Objekt in eine ISO-Zeichenfolge zu formatieren.

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

Öffnen Sie schließlich die `./src/app/calendar/calendar.component.html` Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

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

Dadurch wird die Auflistung der Ereignisse durchlaufen und für jeden eine Tabellenzeile hinzugefügt. Speichern Sie die Änderungen, und starten Sie die APP neu. Klicken Sie auf den Link **Kalender** , und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.

![Screenshot der Ereignistabelle](./images/add-msgraph-01.png)