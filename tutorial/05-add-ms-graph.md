<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren. Für diese Anwendung verwenden Sie die [Microsoft-Graph-Client-](https://github.com/microsoftgraph/msgraph-sdk-javascript) Bibliothek, um Anrufe an Microsoft Graph zu tätigen.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Erstellen Sie eine neue Datei im `./src/app` Verzeichnis mit `event.ts` dem Namen, und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/event.ts" id="eventClasses":::

1. Fügen Sie einen neuen Dienst hinzu, um alle Ihre Graph-Anrufe zu halten. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate service graph
    ```

    Genau wie beim zuvor erstellten Authentifizierungsdienst können Sie einen Dienst für diese Komponente in alle Komponenten einfügen, die Zugriff auf Microsoft Graph benötigen.

1. Nachdem der Befehl abgeschlossen ist, öffnen `./src/app/graph.service.ts` Sie die Datei, und ersetzen Sie Ihren Inhalt durch Folgendes.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="graphServiceSnippet":::

    Überlegen Sie sich, was dieser Code macht.

    - Es initialisiert einen Graph-Client im Konstruktor für den Dienst.
    - Es implementiert eine `getEvents` Funktion, die den Graph-Client wie folgt verwendet:
      - Die URL, die aufgerufen wird, lautet `/me/events`.
      - Die `select` -Methode schränkt die für die einzelnen Ereignisse zurückgegebenen Felder auf genau diejenigen ein, die die Ansicht tatsächlich verwendet wird.
      - Die `orderby` Methode sortiert die Ergebnisse nach dem Datum und der Uhrzeit, zu der Sie erstellt wurden, wobei das letzte Element zuerst angezeigt wird.

1. Erstellen Sie eine Winkel Komponente zum Aufrufen dieser neuen Methode, und zeigen Sie die Ergebnisse des Anrufs an. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate component calendar
    ```

1. Nachdem der Befehl abgeschlossen ist, fügen Sie die Komponente `routes` dem Array `./src/app/app-routing.module.ts`in hinzu.

    ```TypeScript
    import { CalendarComponent } from './calendar/calendar.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent }
    ];
    ```

1. Öffnen Sie `./src/app/calendar/calendar.component.ts` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

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

Im Moment wird dadurch nur das Array von Ereignissen in JSON auf der Seite gerendert. Speichern Sie die Änderungen, und starten Sie die App neu. Melden Sie sich an, und klicken Sie in der Navigationsleiste auf den Link **Kalender** . Wenn alles funktioniert, sollte ein JSON-Abbild von Ereignissen im Kalender des Benutzers angezeigt werden.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

Jetzt können Sie die `CalendarComponent` Komponente aktualisieren, um die Ereignisse auf benutzerfreundlichere Weise anzuzeigen.

1. Entfernen Sie den temporären Code, der eine Warnung von `ngOnInit` der Funktion hinzufügt. Die aktualisierte Funktion sollte wie folgt aussehen.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="ngOnInitSnippet":::

1. Fügen Sie der `CalendarComponent` -Klasse eine Funktion hinzu, `DateTimeTimeZone` um ein Objekt in eine ISO-Zeichenfolge zu formatieren.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/calendar/calendar.component.ts" id="formatDateTimeTimeZoneSnippet":::

1. Öffnen Sie `./src/app/calendar/calendar.component.html` die Datei, und ersetzen Sie den Inhalt durch Folgendes.

    :::code language="html" source="../demo/graph-tutorial/src/app/calendar/calendar.component.html" id="calendarHtml":::

Dadurch wird die Auflistung von Ereignissen durchlaufen, und für jede einzelne Tabelle wird eine Tabellenzeile hinzugefügt. Speichern Sie die Änderungen, und starten Sie die APP neu. Klicken Sie auf den Link **Kalender** , und die APP sollte jetzt eine Tabelle mit Ereignissen rendern.

![Ein Screenshot der Tabelle mit Ereignissen](./images/add-msgraph-01.png)
