<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c7ae8-101">En este ejercicio, incorporará el Graph Microsoft en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="c7ae8-102">Para esta aplicación, usará la biblioteca [de microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="c7ae8-103">Obtener eventos del calendario desde Outlook</span><span class="sxs-lookup"><span data-stu-id="c7ae8-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="c7ae8-104">Abra `./src/GraphService.ts` y agregue la siguiente función.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="c7ae8-105">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="c7ae8-106">La dirección URL a la que se llamará es `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="c7ae8-107">El método agrega el encabezado a la solicitud, lo que hace que las horas de la respuesta se den en la zona horaria preferida `header` `Prefer: outlook.timezone=""` del usuario.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="c7ae8-108">El `query` método agrega los parámetros `startDateTime` `endDateTime` y, definiendo la ventana de tiempo para la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="c7ae8-109">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="c7ae8-110">El método ordena los resultados por la fecha y hora en que se crearon, siendo el `orderby` elemento más reciente el primero.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="c7ae8-111">El `top` método limita los resultados de una sola página a 25 eventos.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-111">The `top` method limits the results in a single page to 25 events.</span></span>
    - <span data-ttu-id="c7ae8-112">Si la respuesta contiene un valor, que indica que hay más resultados disponibles, se usa un objeto para paginar la colección para obtener todos `@odata.nextLink` `PageIterator` los resultados. [](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript)</span><span class="sxs-lookup"><span data-stu-id="c7ae8-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="c7ae8-113">Cree un React para mostrar los resultados de la llamada.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="c7ae8-114">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Calendar.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

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

    <span data-ttu-id="c7ae8-115">Por ahora, esto simplemente representa la matriz de eventos en JSON en la página.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="c7ae8-116">Agrega este nuevo componente a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-116">Add this new component to the app.</span></span> <span data-ttu-id="c7ae8-117">Abra `./src/App.tsx` y agregue la siguiente instrucción a la parte superior del `import` archivo.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="c7ae8-118">Agregue el siguiente componente justo después del `<Route>` existente .</span><span class="sxs-lookup"><span data-stu-id="c7ae8-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="c7ae8-119">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-119">Save your changes and restart the app.</span></span> <span data-ttu-id="c7ae8-120">Inicie sesión y haga clic en **el vínculo Calendario** de la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="c7ae8-121">Si funciona todo, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="c7ae8-122">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="c7ae8-122">Display the results</span></span>

<span data-ttu-id="c7ae8-123">Ahora puede actualizar el componente para mostrar los eventos de una manera más `Calendar` fácil de usar.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="c7ae8-124">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Calendar.css` código.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="c7ae8-125">Cree un React para representar eventos en un solo día como filas de tabla.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="c7ae8-126">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `CalendarDayRow.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="c7ae8-127">Agregue las siguientes `import` instrucciones a la parte superior de **Calendar.tsx**.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-127">Add the following `import` statements to the top of **Calendar.tsx**.</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="c7ae8-128">Reemplace la función `render` existente por la siguiente `./src/Calendar.tsx` función.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="c7ae8-129">Esto divide los eventos en sus respectivos días y representa una sección de tabla para cada día.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="c7ae8-130">Guarda los cambios y reinicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-130">Save the changes and restart the app.</span></span> <span data-ttu-id="c7ae8-131">Haz clic en **el vínculo** Calendario y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="c7ae8-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Una captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
