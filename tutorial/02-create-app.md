<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c06e4-101">En esta sección crearás una nueva aplicación de React.</span><span class="sxs-lookup"><span data-stu-id="c06e4-101">In this section you'll create a new React app.</span></span>

1. <span data-ttu-id="c06e4-102">Abre la interfaz de línea de comandos (CLI), navega a un directorio donde tienes derechos para crear archivos y ejecuta los siguientes comandos para crear una nueva aplicación de React.</span><span class="sxs-lookup"><span data-stu-id="c06e4-102">Open your command-line interface (CLI), navigate to a directory where you have rights to create files, and run the following commands to create a new React app.</span></span>

    ```Shell
    npx create-react-app@4.0.1 graph-tutorial --template typescript
    ```

1. <span data-ttu-id="c06e4-103">Una vez que finalice el comando, cambie al directorio de la CLI y `graph-tutorial` ejecute el siguiente comando para iniciar un servidor web local.</span><span class="sxs-lookup"><span data-stu-id="c06e4-103">Once the command finishes, change to the `graph-tutorial` directory in your CLI and run the following command to start a local web server.</span></span>

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > <span data-ttu-id="c06e4-104">Si no tienes [instalado El hilo,](https://yarnpkg.com/) puedes usarlo `npm start` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="c06e4-104">If you do not have [Yarn](https://yarnpkg.com/) installed, you can use `npm start` instead.</span></span>

<span data-ttu-id="c06e4-105">El explorador predeterminado se abre [https://localhost:3000/](https://localhost:3000) con una página de React predeterminada.</span><span class="sxs-lookup"><span data-stu-id="c06e4-105">Your default browser opens to [https://localhost:3000/](https://localhost:3000) with a default React page.</span></span> <span data-ttu-id="c06e4-106">Si el explorador no se abre, ábralo y vaya para [https://localhost:3000/](https://localhost:3000) comprobar que la nueva aplicación funciona.</span><span class="sxs-lookup"><span data-stu-id="c06e4-106">If your browser doesn't open, open it and browse to [https://localhost:3000/](https://localhost:3000) to verify that the new app works.</span></span>

## <a name="add-node-packages"></a><span data-ttu-id="c06e4-107">Agregar paquetes de nodo</span><span class="sxs-lookup"><span data-stu-id="c06e4-107">Add Node packages</span></span>

<span data-ttu-id="c06e4-108">Antes de seguir adelante, instala algunos paquetes adicionales que usarás más adelante:</span><span class="sxs-lookup"><span data-stu-id="c06e4-108">Before moving on, install some additional packages that you will use later:</span></span>

- <span data-ttu-id="c06e4-109">[react-router-dom](https://github.com/ReactTraining/react-router) para el enrutamiento declarativo dentro de la aplicación React.</span><span class="sxs-lookup"><span data-stu-id="c06e4-109">[react-router-dom](https://github.com/ReactTraining/react-router) for declarative routing inside the React app.</span></span>
- <span data-ttu-id="c06e4-110">[arranque](https://github.com/twbs/bootstrap) de estilos y componentes comunes.</span><span class="sxs-lookup"><span data-stu-id="c06e4-110">[bootstrap](https://github.com/twbs/bootstrap) for styling and common components.</span></span>
- <span data-ttu-id="c06e4-111">[reactstrap para](https://github.com/reactstrap/reactstrap) componentes de React basados en Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="c06e4-111">[reactstrap](https://github.com/reactstrap/reactstrap) for React components based on Bootstrap.</span></span>
- <span data-ttu-id="c06e4-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) para iconos.</span><span class="sxs-lookup"><span data-stu-id="c06e4-112">[fontawesome-free](https://github.com/FortAwesome/Font-Awesome) for icons.</span></span>
- <span data-ttu-id="c06e4-113">[momento](https://github.com/moment/moment) para dar formato a fechas y horas.</span><span class="sxs-lookup"><span data-stu-id="c06e4-113">[moment](https://github.com/moment/moment) for formatting dates and times.</span></span>
- <span data-ttu-id="c06e4-114">[windows-iana](https://github.com/rubenillodo/windows-iana) para traducir zonas horarias de Windows al formato IANA.</span><span class="sxs-lookup"><span data-stu-id="c06e4-114">[windows-iana](https://github.com/rubenillodo/windows-iana) for translating Windows time zones to IANA format.</span></span>
- <span data-ttu-id="c06e4-115">[msal-browser para](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) autenticarse en Azure Active Directory y recuperar tokens de acceso.</span><span class="sxs-lookup"><span data-stu-id="c06e4-115">[msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) for authenticating to Azure Active Directory and retrieving access tokens.</span></span>
- <span data-ttu-id="c06e4-116">[microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c06e4-116">[microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) for making calls to Microsoft Graph.</span></span>

<span data-ttu-id="c06e4-117">Ejecute el siguiente comando en la CLI.</span><span class="sxs-lookup"><span data-stu-id="c06e4-117">Run the following command in your CLI.</span></span>

```Shell
yarn add react-router-dom@5.2.0 @types/react-router-dom@5.1.7 bootstrap@4.6.0 reactstrap@8.9.0 @types/reactstrap@8.7.2 @fortawesome/fontawesome-free@5.15.2
yarn add moment@2.29.1 moment-timezone@0.5.32 windows-iana@4.2.1 @azure/msal-browser@2.10.0 @microsoft/microsoft-graph-client@2.2.1 @types/microsoft-graph@1.28.0
```

## <a name="design-the-app"></a><span data-ttu-id="c06e4-118">Diseñar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c06e4-118">Design the app</span></span>

<span data-ttu-id="c06e4-119">Empieza creando una barra de navegación para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c06e4-119">Start by creating a navbar for the app.</span></span>

1. <span data-ttu-id="c06e4-120">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `NavBar.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="c06e4-120">Create a new file in the `./src` directory named `NavBar.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. <span data-ttu-id="c06e4-121">Crea una página principal para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c06e4-121">Create a home page for the app.</span></span> <span data-ttu-id="c06e4-122">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Welcome.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="c06e4-122">Create a new file in the `./src` directory named `Welcome.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. <span data-ttu-id="c06e4-123">Cree una presentación de mensaje de error para mostrar los mensajes al usuario.</span><span class="sxs-lookup"><span data-stu-id="c06e4-123">Create an error message display to display messages to the user.</span></span> <span data-ttu-id="c06e4-124">Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `ErrorMessage.tsx` código.</span><span class="sxs-lookup"><span data-stu-id="c06e4-124">Create a new file in the `./src` directory named `ErrorMessage.tsx` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. <span data-ttu-id="c06e4-125">Abra el archivo `./src/index.css` y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c06e4-125">Open the `./src/index.css` file and replace its entire contents with the following.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. <span data-ttu-id="c06e4-126">Abra `./src/App.tsx` y reemplace todo su contenido por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="c06e4-126">Open `./src/App.tsx` and replace its entire contents with the following.</span></span>

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

1. <span data-ttu-id="c06e4-127">Guarde todos los cambios y reinicie la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c06e4-127">Save all of your changes and restart the app.</span></span> <span data-ttu-id="c06e4-128">Ahora, la aplicación debería tener un aspecto muy diferente.</span><span class="sxs-lookup"><span data-stu-id="c06e4-128">Now, the app should look very different.</span></span>

    ![Captura de pantalla de la página principal rediseñada](images/create-app-01.png)
