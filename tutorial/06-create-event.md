<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.

1. Öffnen Sie **/src/App/Graph.Service.TS** , und fügen Sie der-Klasse die folgende Funktion hinzu `GraphService` .

    :::code language="typescript" source="../demo/graph-tutorial/src/app/graph.service.ts" id="AddEventSnippet":::

## <a name="create-a-new-event-form"></a>Erstellen eines neuen Ereignis Formulars

1. Erstellen Sie eine Winkel Komponente, um ein Formular anzuzeigen und diese neue Funktion aufzurufen. Führen Sie den folgenden Befehl in der CLI aus.

    ```Shell
    ng generate component new-event
    ```

1. Nachdem der Befehl abgeschlossen ist, fügen Sie die Komponente dem `routes` Array in **./src/App/App-Routing.Module.TS**.

    ```typescript
    import { NewEventComponent } from './new-event/new-event.component';

    const routes: Routes = [
      { path: '', component: HomeComponent },
      { path: 'calendar', component: CalendarComponent },
      { path: 'newevent', component: NewEventComponent },
    ];
    ```

1. Erstellen Sie eine neue Datei im **./src/App/New-Event** -Verzeichnis mit dem Namen **New-Event. TS** , und fügen Sie den folgenden Code hinzu.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.ts" id="NewEventSnippet":::

    Diese Klasse wird als Modell für das neue Ereignis Formular dienen.

1. Öffnen Sie **./src/App/New-Event/New-Event.Component.TS** , und ersetzen Sie den Inhalt durch den folgenden Code.

    :::code language="typescript" source="../demo/graph-tutorial/src/app/new-event/new-event.component.ts" id="NewEventComponentSnippet":::

1. Öffnen Sie **./src/App/New-Event/new-event.component.html** , und ersetzen Sie den Inhalt durch den folgenden Code.

    :::code language="html" source="../demo/graph-tutorial/src/app/new-event/new-event.component.html" id="NewEventFormSnippet":::

1. Speichern Sie die Änderungen, und aktualisieren Sie die app. Wählen Sie auf der Kalender Seite die Schaltfläche **Neues Ereignis** aus, und erstellen Sie dann mithilfe des Formulars ein Ereignis im Kalender des Benutzers.

    ![Screenshot des neuen Ereignis Formulars](images/create-event.png)
