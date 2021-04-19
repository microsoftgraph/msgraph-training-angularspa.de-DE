<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung integrieren Sie Microsoft Graph in die Anwendung. Für diese Anwendung verwenden Sie die [microsoft-graph-client-Bibliothek,](https://github.com/microsoftgraph/msgraph-sdk-javascript) um Aufrufe an Microsoft Graph zu senden.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Aufrufe zu halten. Führen Sie den folgenden Befehl in Ihrer CLI aus.

    ```Shell
    ng generate service graph
    ```

    Genau wie bei dem zuvor erstellten Authentifizierungsdienst können Sie einen dienst hierinjizieren, um ihn in alle Komponenten zu injizieren, die Zugriff auf Microsoft Graph benötigen.

1. Öffnen Sie nach Abschluss des Befehls **./src/app/graph.service.ts,** und ersetzen Sie den Inhalt durch Folgendes.

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

    Überlegen Sie sich, was dieser Code macht.

    - Es initialisiert einen Graph-Client im Konstruktor für den Dienst.
    - Es implementiert eine `getCalendarView` Funktion, die den Graph-Client auf folgende Weise verwendet:
      - Die URL, die aufgerufen wird, lautet `/me/calendarview`.
      - Die -Methode enthält den Header, wodurch die Start- und Endzeiten der zurückgegebenen Ereignisse in der bevorzugten Zeitzone des Benutzers `header` `Prefer: outlook.timezone` enthalten sind.
      - Die -Methode fügt die Parameter and hinzu und definiert das `query` `startDateTime` `endDateTime` Zeitfenster für die Kalenderansicht.
      - Die Methode beschränkt die für jedes Ereignis zurückgegebenen Felder auf die Felder, die `select` von der Ansicht tatsächlich verwendet werden.
      - Die `orderby` Methode sortiert die Ergebnisse nach Startzeit.

1. Erstellen Sie eine Angular-Komponente, um diese neue Methode auf aufruft und die Ergebnisse des Aufrufs anzeigen. Führen Sie den folgenden Befehl in Ihrer CLI aus.

    ```Shell
    ng generate component calendar
    ```

1. Fügen Sie nach Abschluss des Befehls die Komponente dem `routes` Array in **./src/app/app-routing.module.ts hinzu.**

    ```typescript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
    ];
    ```

1. Öffnen **Sie ./tsconfig.jsein,** und fügen Sie dem Objekt die folgende Eigenschaft `compilerOptions` hinzu.

    ```json
    "resolveJsonModule": true
    ```

1. Öffnen **Sie ./src/app/calendar/calendar.component.ts,** und ersetzen Sie den Inhalt durch Folgendes.

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

Damit wird derzeit nur das Array von Ereignissen in JSON auf der Seite gerendert. Speichern Sie die Änderungen, und starten Sie die App neu. Melden Sie sich an, **und** klicken Sie in der Navigationsleiste auf den Link Kalender. Wenn alles funktioniert, sollte ein JSON-Abbild von Ereignissen im Kalender des Benutzers angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Jetzt können Sie die Komponente so aktualisieren, dass die Ereignisse `CalendarComponent` benutzerfreundlicher angezeigt werden.

1. Entfernen Sie den temporären Code, der eine Warnung aus der Funktion `ngOnInit` hinzufügt. Ihre aktualisierte Funktion sollte wie dies aussehen.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Fügen Sie der Klasse eine Funktion `CalendarComponent` hinzu, um ein `DateTimeTimeZone` Objekt in eine ISO-Zeichenfolge zu formatieren.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Öffnen **Sie ./src/app/calendar/calendar.component.html,** und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Dadurch wird die Auflistung von Ereignissen in einer Schleife durchlauft und für jedes Ereignis eine Tabellenzeile hinzufügt. Speichern Sie die Änderungen, und starten Sie die App neu. Klicken Sie auf den **Link Kalender,** und die App sollte nun eine Tabelle mit Ereignissen rendern.

![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
