<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="6aee8-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aee8-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="6aee8-102">Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="6aee8-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="6aee8-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="6aee8-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="6aee8-104">Empiece agregando un nuevo método al `./src/GraphService.js` archivo para obtener los eventos del calendario.</span><span class="sxs-lookup"><span data-stu-id="6aee8-104">Start by adding a new method to the `./src/GraphService.js` file to get the events from the calendar.</span></span> <span data-ttu-id="6aee8-105">Agregue la siguiente función.</span><span class="sxs-lookup"><span data-stu-id="6aee8-105">Add the following function.</span></span>

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

<span data-ttu-id="6aee8-106">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="6aee8-106">Consider what this code is doing.</span></span>

- <span data-ttu-id="6aee8-107">La dirección URL a la que se `/me/events`llamará es.</span><span class="sxs-lookup"><span data-stu-id="6aee8-107">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="6aee8-108">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="6aee8-108">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
- <span data-ttu-id="6aee8-109">El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="6aee8-109">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>

<span data-ttu-id="6aee8-110">Ahora, cree un componente de reAct para mostrar los resultados de la llamada.</span><span class="sxs-lookup"><span data-stu-id="6aee8-110">Now create a React component to display the results of the call.</span></span> <span data-ttu-id="6aee8-111">Cree un nuevo archivo en el `./src` directorio denominado `Calendar.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="6aee8-111">Create a new file in the `./src` directory named `Calendar.js` and add the following code.</span></span>

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
      var accessToken = await window.msal.acquireTokenSilent({
        scopes: config.scopes
      });
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

<span data-ttu-id="6aee8-112">Por ahora, esto solo representa la matriz de eventos en JSON en la página.</span><span class="sxs-lookup"><span data-stu-id="6aee8-112">For now this just renders the array of events in JSON on the page.</span></span> <span data-ttu-id="6aee8-113">Agregue este nuevo componente a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aee8-113">Add this new component to the app.</span></span> <span data-ttu-id="6aee8-114">Abra `./src/App.js` y agregue la siguiente `import` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="6aee8-114">Open `./src/App.js` and add the following `import` statement to the top of the file.</span></span>

```js
import Calendar from './Calendar';
```

<span data-ttu-id="6aee8-115">A continuación, agregue el siguiente componente inmediatamente después `<Route>`del existente.</span><span class="sxs-lookup"><span data-stu-id="6aee8-115">Then add the following component just after the existing `<Route>`.</span></span>

```JSX
<Route exact path="/calendar"
  render={(props) =>
    <Calendar {...props}
      showError={this.setErrorMessage.bind(this)} />
  } />
```

<span data-ttu-id="6aee8-116">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aee8-116">Save your changes and restart the app.</span></span> <span data-ttu-id="6aee8-117">Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="6aee8-117">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="6aee8-118">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="6aee8-118">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="6aee8-119">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="6aee8-119">Display the results</span></span>

<span data-ttu-id="6aee8-120">Ahora puede actualizar el `Calendar` componente para mostrar los eventos de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="6aee8-120">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span> <span data-ttu-id="6aee8-121">Reemplace la función `render` existente en `./src/Calendar.js` con la siguiente función.</span><span class="sxs-lookup"><span data-stu-id="6aee8-121">Replace the existing `render` function in `./src/Calendar.js` with the following function.</span></span>

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
            function(event){
              return(
                <tr key={event.id}>
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

<span data-ttu-id="6aee8-122">Esto recorre la colección de eventos y agrega una fila de tabla para cada uno.</span><span class="sxs-lookup"><span data-stu-id="6aee8-122">This loops through the collection of events and adds a table row for each one.</span></span> <span data-ttu-id="6aee8-123">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6aee8-123">Save the changes and restart the app.</span></span> <span data-ttu-id="6aee8-124">Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="6aee8-124">Click on the **Calendar** link and the app should now render a table of events.</span></span>

![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
