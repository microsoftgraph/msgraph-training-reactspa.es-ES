<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9579a-101">En esta sección, creará una nueva aplicación reAct.</span><span class="sxs-lookup"><span data-stu-id="9579a-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="9579a-102">Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para crear una nueva aplicación reAct.</span><span class="sxs-lookup"><span data-stu-id="9579a-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@3.4.1 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="9579a-103">Una vez que finalice el comando, cambie al `graph-tutorial` Directorio de la CLI y ejecute el siguiente comando para iniciar un servidor Web local.</span><span class="sxs-lookup"><span data-stu-id="9579a-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    npm start
    ```

<span data-ttu-id="9579a-104">El explorador predeterminado se abre en [https://localhost:3000/](https://localhost:3000) con una página de reAct predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9579a-104">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="9579a-105">Si el explorador no se abre, ábralo y vaya a [https://localhost:3000/](https://localhost:3000) para comprobar que la nueva aplicación funciona.</span><span class="sxs-lookup"><span data-stu-id="9579a-105">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="9579a-106">Agregar paquetes de nodo</span><span class="sxs-lookup"><span data-stu-id="9579a-106">Add Node packages</span></span>

<span data-ttu-id="9579a-107">Antes de continuar, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="9579a-107">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="9579a-108">[reAct-router-Dom](https://github.com/ReactTraining/react-router) para el enrutamiento declarativo dentro de la aplicación reAct.</span><span class="sxs-lookup"><span data-stu-id="9579a-108">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="9579a-109">[bootstrap](https://github.com/twbs/bootstrap) para aplicar estilos y componentes comunes.</span><span class="sxs-lookup"><span data-stu-id="9579a-109">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="9579a-110">[reactstrap](https://github.com/reactstrap/reactstrap) para los componentes de reAct basados en bootstrap.</span><span class="sxs-lookup"><span data-stu-id="9579a-110">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="9579a-111">[fontawesome: libre](https://github.com/FortAwesome/Font-Awesome) para iconos.</span><span class="sxs-lookup"><span data-stu-id="9579a-111">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="9579a-112">[momento](https://github.com/moment/moment) para dar formato a fechas y horas.</span><span class="sxs-lookup"><span data-stu-id="9579a-112">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="9579a-113">[Windows-IANA](https://github.com/rubenillodo/windows-iana) para traducir zonas horarias de Windows al formato IANA.</span><span class="sxs-lookup"><span data-stu-id="9579a-113">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="9579a-114">[msal-explorador](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) para autenticar en Azure Active Directory y recuperar los tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="9579a-114">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="9579a-115">[Microsoft-Graph-Client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="9579a-115">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="9579a-116">Ejecute el siguiente comando en su CLI.</span><span class="sxs-lookup"><span data-stu-id="9579a-116">Run the following command in your CLI.</span></span>

```Shell
npm install react-router-dom@5.2.0 @types/react-router-dom@5.1.5 bootstrap@4.5.2 reactstrap@8.5.1 @types/reactstrap@8.5.1 @fortawesome/fontawesome-free@5.14.0
npm install moment@2.27.0 moment-timezone@0.5.31 windows-iana@4.2.1 @azure/msal-browser@2.1.0 @microsoft/microsoft-graph-client@2.0.0 @types/microsoft-graph@1.18.0
```

## <a name="design-the-app"></a><span data-ttu-id="9579a-117">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="9579a-117">Design the app</span></span>

<span data-ttu-id="9579a-118">Empiece por crear una barra de exploración para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9579a-118">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="9579a-119">Cree un nuevo archivo en el `./src` directorio denominado `NavBar.tsx` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="9579a-119">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="9579a-120">Cree una página principal para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9579a-120">Create a home page for the app.</span></span> <span data-ttu-id="9579a-121">Cree un nuevo archivo en el `./src` directorio denominado `Welcome.tsx` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="9579a-121">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="9579a-122">Cree un mensaje de error para mostrar mensajes al usuario.</span><span class="sxs-lookup"><span data-stu-id="9579a-122">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="9579a-123">Cree un nuevo archivo en el `./src` directorio denominado `ErrorMessage.tsx` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="9579a-123">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="9579a-124">Abra el archivo `./src/index.css` y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9579a-124">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="9579a-125">Abra `./src/App.tsx` y reemplace todo el contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9579a-125">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

    ```typescript
    import React, { Component } from 'react';
    import { BrowserRouter as Router, Route, Redirect } from 'react-router-dom';
    import { Container } from 'reactstrap';
    import NavBar from './NavBar';
    import ErrorMessage from './ErrorMessage';
    import Welcome from './Welcome';
    import 'bootstrap/dist/css/bootstrap.css';

    class App extends Component<any> {
      render() {
        let error = null;
        if (this.props.error) {
          error = <ErrorMessage
            message={this.props.error.message}
            debug={this.props.error.debug} />;
        }

        return (
          <Router>
            <div>
              <NavBar
                isAuthenticated={this.props.isAuthenticated}
                authButtonMethod={this.props.isAuthenticated ? this.props.logout : this.props.login}
                user={this.props.user}/>
              <Container>
                {error}
                <Route exact path="/"
                  render={(props) =>
                    <Welcome {...props}
                      isAuthenticated={this.props.isAuthenticated}
                      user={this.props.user}
                      authButtonMethod={this.props.login} />
                  } />
              </Container>
            </div>
          </Router>
        );
      }
    }

    export default App;
    ```

1. <span data-ttu-id="9579a-126">Guarde todos los cambios y actualice la página.</span><span class="sxs-lookup"><span data-stu-id="9579a-126">Save all of your changes and refresh the page.</span></span> <span data-ttu-id="9579a-127">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="9579a-127">Now, the app should look very different.</span></span>

    ![Una captura de pantalla de la Página principal rediseñada](images/create-app-01.png)
