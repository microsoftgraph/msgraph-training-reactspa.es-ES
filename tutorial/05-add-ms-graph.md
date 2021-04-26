<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará el Graph Microsoft en la aplicación. Para esta aplicación, usará la biblioteca [de microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos del calendario desde Outlook

1. Abra `./src/GraphService.ts` y agregue la siguiente función.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    Tenga en cuenta lo que está haciendo este código.

    - La dirección URL a la que se llamará es `/me/calendarview`.
    - El método agrega el encabezado a la solicitud, lo que hace que las horas de la respuesta se den en la zona horaria preferida `header` `Prefer: outlook.timezone=""` del usuario.
    - El `query` método agrega los parámetros `startDateTime` `endDateTime` y, definiendo la ventana de tiempo para la vista de calendario.
    - El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
    - El método ordena los resultados por la fecha y hora en que se crearon, siendo el `orderby` elemento más reciente el primero.
    - El `top` método limita los resultados de una sola página a 25 eventos.
    - Si la respuesta contiene un valor, que indica que hay más resultados disponibles, se usa un objeto para paginar la colección para obtener todos `@odata.nextLink` `PageIterator` los resultados. [](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)

1. Cree un React para mostrar los resultados de la llamada. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Calendar.tsx` código.

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment, { Moment } from 'moment-timezone';
    import { findIana } from "windows-iana";
    import { Event } from 'microsoft-graph';
    import { config } from './Config';
    import { getUserWeekCalendar } from './GraphService';
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';

    interface CalendarState {
      eventsLoaded: boolean;
      events: Event[];
      startOfWeek: Moment | undefined;
    }

    class Calendar extends React.Component<AuthComponentProps, CalendarState> {
      constructor(props: any) {
        super(props);

        this.state = {
          eventsLoaded: false,
          events: [],
          startOfWeek: undefined
        };
      }

      async componentDidUpdate() {
        if (this.props.user && !this.state.eventsLoaded)
        {
          try {
            // Get the user's access token
            var accessToken = await this.props.getAccessToken(config.scopes);

            // Convert user's Windows time zone ("Pacific Standard Time")
            // to IANA format ("America/Los_Angeles")
            // Moment needs IANA format
            var ianaTimeZones = findIana(this.props.user.timeZone);

            // Get midnight on the start of the current week in the user's timezone,
            // but in UTC. For example, for Pacific Standard Time, the time value would be
            // 07:00:00Z
            var startOfWeek = moment.tz(ianaTimeZones![0].valueOf()).startOf('week').utc();

            // Get the user's events
            var events = await getUserWeekCalendar(accessToken, this.props.user.timeZone, startOfWeek);

            // Update the array of events in state
            this.setState({
              eventsLoaded: true,
              events: events,
              startOfWeek: startOfWeek
            });
          }
          catch (err) {
            this.props.setError('ERROR', JSON.stringify(err));
          }
        }
      }

      render() {
        return (
          <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
        );
      }
    }

    export default withAuthProvider(Calendar);
    ```

    Por ahora, esto simplemente representa la matriz de eventos en JSON en la página.

1. Agrega este nuevo componente a la aplicación. Abra `./src/App.tsx` y agregue la siguiente instrucción a la parte superior del `import` archivo.

    ```typescript
    import Calendar from './Calendar';
    ```

1. Agregue el siguiente componente justo después del `<Route>` existente .

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. Guarde los cambios y reinicie la aplicación. Inicie sesión y haga clic en **el vínculo Calendario** de la barra de navegación. Si funciona todo, debería ver un volcado JSON de eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede actualizar el componente para mostrar los eventos de una manera más `Calendar` fácil de usar.

1. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Calendar.css` código.

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. Cree un React para representar eventos en un solo día como filas de tabla. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `CalendarDayRow.tsx` código.

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. Agregue las siguientes `import` instrucciones a la parte superior de **Calendar.tsx**.

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. Reemplace la función `render` existente por la siguiente `./src/Calendar.tsx` función.

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    Esto divide los eventos en sus respectivos días y representa una sección de tabla para cada día.

1. Guarda los cambios y reinicia la aplicación. Haz clic en **el vínculo** Calendario y la aplicación ahora debe representar una tabla de eventos.

    ![Una captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
