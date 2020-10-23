<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren. Für diese Anwendung verwenden Sie die [Microsoft-Graph-Client-](https://github.com/microsoftgraph/msgraph-sdk-javascript) Bibliothek, um Anrufe an Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Anrufe zu halten. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate service graph
    ```

    Genau wie beim zuvor erstellten Authentifizierungsdienst können Sie einen Dienst für diese Komponente in alle Komponenten einfügen, die Zugriff auf Microsoft Graph benötigen.

1. Nachdem der Befehl abgeschlossen ist, öffnen Sie **./src/App/Graph.Service.TS** , und ersetzen Sie den Inhalt durch Folgendes.

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

    Überlegen Sie sich, was dieser Code macht.

    - Es initialisiert einen Graph-Client im Konstruktor für den Dienst.
    - Es implementiert eine `getCalendarView` Funktion, die den Graph-Client wie folgt verwendet:
      - Die URL, die aufgerufen wird, lautet `/me/calendarview`.
      - Die `header` -Methode enthält die `Prefer: outlook.timezone` Kopfzeile, wodurch die Anfangs-und Endzeiten der zurückgegebenen Ereignisse in der bevorzugten Zeitzone des Benutzers liegen.
      - Die `query` -Methode fügt die `startDateTime` Parameter and hinzu und `endDateTime` definiert das Zeitfenster für die Kalenderansicht.
      - Die `select` -Methode schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf genau diejenigen ein, die die Ansicht tatsächlich verwendet wird.
      - Die `orderby` -Methode sortiert die Ergebnisse nach dem Startzeitpunkt.

1. Erstellen Sie eine Winkel Komponente zum Aufrufen dieser neuen Methode, und zeigen Sie die Ergebnisse des Anrufs an. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate component calendar
    ```

1. Nachdem der Befehl abgeschlossen ist, fügen Sie die Komponente dem `routes` Array in **./src/App/App-Routing.Module.TS**.

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Öffnen Sie **./src/App/Calendar/Calendar.Component.TS** , und ersetzen Sie den Inhalt durch Folgendes.

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

Im Moment wird dadurch nur das Array von Ereignissen in JSON auf der Seite gerendert. Speichern Sie die Änderungen, und starten Sie die App neu. Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** . Wenn alles funktioniert, sollte ein JSON-Abbild von Ereignissen im Kalender des Benutzers angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Jetzt können Sie die `CalendarComponent` Komponente aktualisieren, um die Ereignisse auf benutzerfreundlichere Weise anzuzeigen.

1. Entfernen Sie den temporären Code, der eine Warnung von der `ngOnInit` Funktion hinzufügt. Die aktualisierte Funktion sollte wie folgt aussehen.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Fügen Sie der-Klasse eine Funktion hinzu `CalendarComponent` , um ein `DateTimeTimeZone` Objekt in eine ISO-Zeichenfolge zu formatieren.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Öffnen Sie **./src/App/Calendar/calendar.component.html** , und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Dadurch wird die Auflistung von Ereignissen durchlaufen, und für jede einzelne Tabelle wird eine Tabellenzeile hinzugefügt. Speichern Sie die Änderungen, und starten Sie die APP neu. Klicken Sie auf den Link **Kalender** , und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.

![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
