<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos de calendario de Outlook

Empiece agregando un nuevo método al `./src/GraphService.js` archivo para obtener los eventos del calendario. Agregue la siguiente función.

```js
export async function getEvents(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const events = await client
    .api('/me/events')
    .select('subject,organizer,start,end')
    .orderby('createdDateTime DESC')
    .get();

  return events;
}
```

Tenga en cuenta lo que está haciendo este código.

- La dirección URL a la que se `/me/events`llamará es.
- El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.
- El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.

Ahora, cree un componente de reAct para mostrar los resultados de la llamada. Cree un nuevo archivo en el `./src` directorio denominado `Calendar.js` y agregue el siguiente código.

```JSX
import React from 'react';
import { Table } from 'reactstrap';
import moment from 'moment';
import config from './Config';
import { getEvents } from './GraphService';

// Helper function to format Graph date/time
function formatDateTime(dateTime) {
  return moment.utc(dateTime).local().format('M/D/YY h:mm A');
}

export default class Calendar extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      events: []
    };
  }

  async componentDidMount() {
    try {
      // Get the user's access token
      var accessToken = await window.msal.acquireTokenSilent(config.scopes);
      // Get the user's events
      var events = await getEvents(accessToken);
      // Update the array of events in state
      this.setState({events: events.value});
    }
    catch(err) {
      this.props.showError('ERROR', JSON.stringify(err));
    }
  }

  render() {
    return (
      <pre><code>{JSON.stringify(this.state.events, null, 2)}</code></pre>
    );
  }
}
```

Por ahora, esto solo representa la matriz de eventos en JSON en la página. Agregue este nuevo componente a la aplicación. Abra `./src/App.js` y agregue la siguiente `import` instrucción a la parte superior del archivo.

```js
import Calendar from './Calendar';
```

A continuación, agregue el siguiente componente inmediatamente después `<Route>`del existente.

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

Guarde los cambios y reinicie la aplicación. Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación. Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.

## <a name="display-the-results"></a>Mostrar los resultados

Ahora puede actualizar el `Calendar` componente para mostrar los eventos de forma más fácil de uso. Reemplace la función `render` existente en `./src/Calendar.js` con la siguiente función.

```JSX
render() {
  return (
    <div>
      <h1>Calendar</h1>
      <Table>
        <thead>
          <tr>
            <th scope="col">Organizer</th>
            <th scope="col">Subject</th>
            <th scope="col">Start</th>
            <th scope="col">End</th>
          </tr>
        </thead>
        <tbody>
          {this.state.events.map(
            function(event, index){
              return(
                <tr>
                  <td>{event.organizer.emailAddress.name}</td>
                  <td>{event.subject}</td>
                  <td>{formatDateTime(event.start.dateTime)}</td>
                  <td>{formatDateTime(event.end.dateTime)}</td>
                </tr>
              );
            })}
        </tbody>
      </Table>
    </div>
  );
}
```

Esto recorre la colección de eventos y agrega una fila de tabla para cada uno. Guarde los cambios y reinicie la aplicación. Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)