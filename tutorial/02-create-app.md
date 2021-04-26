<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="025f0-101">En esta sección, crearás una nueva React aplicación.</span><span class="sxs-lookup"><span data-stu-id="025f0-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="025f0-102">Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para crear una nueva React aplicación.</span><span class="sxs-lookup"><span data-stu-id="025f0-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    yarn create react-app graph-tutorial --template typescript
    ```

1. <span data-ttu-id="025f0-103">Cuando finalice el comando, cambie al directorio de la CLI y `graph-tutorial` ejecute el siguiente comando para iniciar un servidor web local.</span><span class="sxs-lookup"><span data-stu-id="025f0-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > <span data-ttu-id="025f0-104">Si no tienes [yarn](https://yarnpkg.com/) instalado, puedes usarlo en `npm start` su lugar.</span><span class="sxs-lookup"><span data-stu-id="025f0-104">If you do not have [Yarn](https://yarnpkg.com/) installed, you can use `npm start` instead.</span></span>

<span data-ttu-id="025f0-105">El explorador predeterminado se abre [https://localhost:3000/](https://localhost:3000) con una página React predeterminada.</span><span class="sxs-lookup"><span data-stu-id="025f0-105">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="025f0-106">Si el explorador no se abre, ábralo y busque [https://localhost:3000/](https://localhost:3000) para comprobar que la nueva aplicación funciona.</span><span class="sxs-lookup"><span data-stu-id="025f0-106">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="025f0-107">Agregar paquetes de nodo</span><span class="sxs-lookup"><span data-stu-id="025f0-107">Add Node packages</span></span>

<span data-ttu-id="025f0-108">Antes de seguir adelante, instale algunos paquetes adicionales que usará más adelante:</span><span class="sxs-lookup"><span data-stu-id="025f0-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="025f0-109">[react-router-dom](https://github.com/ReactTraining/react-router) para el enrutamiento declarativo dentro de la React aplicación.</span><span class="sxs-lookup"><span data-stu-id="025f0-109">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="025f0-110">[bootstrap](https://github.com/twbs/bootstrap) para el estilo y componentes comunes.</span><span class="sxs-lookup"><span data-stu-id="025f0-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="025f0-111">[reactstrap para](https://github.com/reactstrap/reactstrap) React componentes basados en Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="025f0-111">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="025f0-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) para iconos.</span><span class="sxs-lookup"><span data-stu-id="025f0-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="025f0-113">[momento](https://github.com/moment/moment) para dar formato a fechas y horas.</span><span class="sxs-lookup"><span data-stu-id="025f0-113">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="025f0-114">[windows-iana para](https://github.com/rubenillodo/windows-iana) traducir Windows zonas horarias al formato IANA.</span><span class="sxs-lookup"><span data-stu-id="025f0-114">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="025f0-115">[msal-browser para](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) autenticar a Azure Active Directory y recuperar tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="025f0-115">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="025f0-116">[microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="025f0-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="025f0-117">Ejecute el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="025f0-117">Run the following command in your CLI.</span></span>

```Shell
yarn add react-router-dom@5.2.0 bootstrap@4.6.0 reactstrap@8.9.0 @fortawesome/fontawesome-free@5.15.3
yarn add moment-timezone@0.5.33 windows-iana@5.0.2 @azure/msal-browser@2.14.1 @microsoft/microsoft-graph-client@2.2.1
yarn add -D @types/react-router-dom@5.1.7 @types/microsoft-graph@1.36.0
```

## <a name="design-the-app"></a><span data-ttu-id="025f0-118">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="025f0-118">Design the app</span></span>

<span data-ttu-id="025f0-119">Empieza creando una barra de navegación para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="025f0-119">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="025f0-120">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `NavBar.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="025f0-120">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="025f0-121">Crea una página principal para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="025f0-121">Create a home page for the app.</span></span> <span data-ttu-id="025f0-122">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Welcome.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="025f0-122">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="025f0-123">Cree una presentación de mensaje de error para mostrar mensajes al usuario.</span><span class="sxs-lookup"><span data-stu-id="025f0-123">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="025f0-124">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `ErrorMessage.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="025f0-124">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="025f0-125">Abra el archivo `./src/index.css` y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="025f0-125">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="025f0-126">Abra `./src/App.tsx` y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="025f0-126">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

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
                user={this.props.user} />
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

1. <span data-ttu-id="025f0-127">Guarde todos los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="025f0-127">Save all of your changes and restart the app.</span></span> <span data-ttu-id="025f0-128">Ahora, la aplicación debe tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="025f0-128">Now, the app should look very different.</span></span>

    ![Una captura de pantalla de la página de inicio rediseñada](images/create-app-01.png)
