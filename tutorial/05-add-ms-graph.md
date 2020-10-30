<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="2eb9e-101">En este ejercicio, incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="2eb9e-102">Para esta aplicación, usará la biblioteca de [Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="2eb9e-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="2eb9e-103">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="2eb9e-104">Abra `./src/GraphService.ts` y agregue la siguiente función.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-104">Open `./src/GraphService.ts` and add the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="getUserWeekCalendarSnippet":::

    <span data-ttu-id="2eb9e-105">Tenga en cuenta lo que está haciendo este código.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-105">Consider what this code is doing.</span></span>

    - <span data-ttu-id="2eb9e-106">La dirección URL a la que se llamará es `/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="2eb9e-106">The URL that will be called is `/me/calendarview`.</span></span>
    - <span data-ttu-id="2eb9e-107">El `header` método agrega el `Prefer: outlook.timezone=""` encabezado a la solicitud, lo que provoca que las horas de la respuesta estén en la zona horaria preferida del usuario.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-107">The `header` method adds the `Prefer: outlook.timezone=""` header to the request, causing the times in the response to be in the user's preferred time zone.</span></span>
    - <span data-ttu-id="2eb9e-108">El `query` método agrega los `startDateTime` `endDateTime` parámetros y, que define la ventana de tiempo para la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-108">The `query` method adds the `startDateTime` and `endDateTime` parameters, defining the window of time for the calendar view.</span></span>
    - <span data-ttu-id="2eb9e-109">El `select` método limita los campos devueltos para cada evento a solo aquellos que la vista usará realmente.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-109">The `select` method limits the fields returned for each events to just those the view will actually use.</span></span>
    - <span data-ttu-id="2eb9e-110">El `orderby` método ordena los resultados por la fecha y hora en que se crearon, con el elemento más reciente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-110">The `orderby` method sorts the results by the date and time they were created, with the most recent item being first.</span></span>
    - <span data-ttu-id="2eb9e-111">El `top` método limita los resultados a los primeros 50 eventos.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-111">The `top` method limits the results to the first 50 events.</span></span>
    - <span data-ttu-id="2eb9e-112">Si la respuesta contiene un `@odata.nextLink` valor, lo que indica que hay más resultados disponibles, `PageIterator` se utiliza un objeto para recorrer en una [página la colección](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) y obtener todos los resultados.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-112">If the response contains an `@odata.nextLink` value, indicating there are more results available, a `PageIterator` object is used to [page through the collection](https://docs.microsoft.com/graph/sdks/paging?tabs=typeScript) to get all of the results.</span></span>

1. <span data-ttu-id="2eb9e-113">Cree un componente de reAct para mostrar los resultados de la llamada.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-113">Create a React component to display the results of the call.</span></span> <span data-ttu-id="2eb9e-114">Cree un nuevo archivo en el `./src` directorio denominado `Calendar.tsx` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-114">Create a new file in the `./src` directory named `Calendar.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { NavLink as RouterNavLink } from 'react-router-dom';
    import { Table } from 'reactstrap';
    import moment, { Moment } from 'moment-timezone';
    import { findOneIana } from "windows-iana";
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
        try {
          // Get the user's access token
          var accessToken = await this.props.getAccessToken(config.scopes);
          // Convert user's Windows time zone ("Pacific Standard Time")
          // to IANA format ("America/Los_Angeles")
          // Moment needs IANA format
          var ianaTimeZone = findOneIana(this.props.user.timeZone);

          // Get midnight on the start of the current week in the user's timezone,
          // but in UTC. For example, for Pacific Standard Time, the time value would be
          // 07:00:00Z
          var startOfWeek = moment.tz(ianaTimeZone!.valueOf()).startOf('week').utc();

          // Get the user's events
          var events = await getUserWeekCalendar(accessToken, this.props.user.timeZone, startOfWeek);

          // Update the array of events in state
          this.setState({
            eventsLoaded: true,
            events: events,
            startOfWeek: startOfWeek
          });
        }
        catch(err) {
          this.props.setError('ERROR', JSON.stringify(err));
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

    <span data-ttu-id="2eb9e-115">Por ahora, esto solo representa la matriz de eventos en JSON en la página.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-115">For now this just renders the array of events in JSON on the page.</span></span>

1. <span data-ttu-id="2eb9e-116">Agregue este nuevo componente a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-116">Add this new component to the app.</span></span> <span data-ttu-id="2eb9e-117">Abra `./src/App.tsx` y agregue la siguiente `import` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-117">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import Calendar from './Calendar';
    ```

1. <span data-ttu-id="2eb9e-118">Agregue el siguiente componente justo después del existente `<Route>` .</span><span class="sxs-lookup"><span data-stu-id="2eb9e-118">Add the following component just after the existing `<Route>`.</span></span>

    ```typescript
    <Route exact path="/calendar"
      render={(props) =>
        this.props.isAuthenticated ?
          <Calendar {...props} /> :
          <Redirect to="/" />
      } />
    ```

1. <span data-ttu-id="2eb9e-119">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-119">Save your changes and restart the app.</span></span> <span data-ttu-id="2eb9e-120">Inicie sesión y haga clic en el vínculo de **calendario** en la barra de navegación.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-120">Sign in and click the **Calendar** link in the nav bar.</span></span> <span data-ttu-id="2eb9e-121">Si todo funciona, debería ver un volcado JSON de eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-121">If everything works, you should see a JSON dump of events on the user's calendar.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="2eb9e-122">Mostrar los resultados</span><span class="sxs-lookup"><span data-stu-id="2eb9e-122">Display the results</span></span>

<span data-ttu-id="2eb9e-123">Ahora puede actualizar el `Calendar` componente para mostrar los eventos de forma más fácil de uso.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-123">Now you can update the `Calendar` component to display the events in a more user-friendly manner.</span></span>

1. <span data-ttu-id="2eb9e-124">Cree un nuevo archivo en el `./src` directorio denominado `Calendar.css` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-124">Create a new file in the `./src` directory named `Calendar.css` and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/Calendar.css":::

1. <span data-ttu-id="2eb9e-125">Cree un componente reAct para representar eventos en un solo día como filas de tabla.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-125">Create a React component to render events in a single day as table rows.</span></span> <span data-ttu-id="2eb9e-126">Cree un nuevo archivo en el `./src` directorio denominado `CalendarDayRow.tsx` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-126">Create a new file in the `./src` directory named `CalendarDayRow.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/CalendarDayRow.tsx" id="CalendarDayRowSnippet":::

1. <span data-ttu-id="2eb9e-127">Agregue las siguientes `import` instrucciones en la parte superior del **calendario. TSX** .</span><span class="sxs-lookup"><span data-stu-id="2eb9e-127">Add the following `import` statements to the top of **Calendar.tsx** .</span></span>

    ```typescript
    import CalendarDayRow from './CalendarDayRow';
    import './Calendar.css';
    ```

1. <span data-ttu-id="2eb9e-128">Reemplace la `render` función existente en `./src/Calendar.tsx` con la siguiente función.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-128">Replace the existing `render` function in `./src/Calendar.tsx` with the following function.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Calendar.tsx" id="renderSnippet":::

    <span data-ttu-id="2eb9e-129">Esto divide los eventos en sus días respectivos y representa una sección de tabla para cada día.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-129">This splits the events into their respective days and renders a table section for each day.</span></span>

1. <span data-ttu-id="2eb9e-130">Guarde los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-130">Save the changes and restart the app.</span></span> <span data-ttu-id="2eb9e-131">Haga clic en el vínculo del **calendario** y la aplicación ahora debe representar una tabla de eventos.</span><span class="sxs-lookup"><span data-stu-id="2eb9e-131">Click on the **Calendar** link and the app should now render a table of events.</span></span>

    ![Captura de pantalla de la tabla de eventos](./images/add-msgraph-01.png)
