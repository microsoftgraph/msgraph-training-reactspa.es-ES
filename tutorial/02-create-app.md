<!-- markdownlint-disable MD002 MD041 -->

En esta sección, crearás una nueva React aplicación.

1. Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde tenga derechos para crear archivos y ejecute los siguientes comandos para crear una nueva React aplicación.

    ```Shell
    yarn create react-app graph-tutorial --template typescript
    ```

1. Cuando finalice el comando, cambie al directorio de la CLI y `graph-tutorial` ejecute el siguiente comando para iniciar un servidor web local.

    ```Shell
    yarn start
    ```

    > [!NOTE]
    > Si no tienes [yarn](https://yarnpkg.com/) instalado, puedes usarlo en `npm start` su lugar.

El explorador predeterminado se abre [https://localhost:3000/](https://localhost:3000) con una página React predeterminada. Si el explorador no se abre, ábralo y busque [https://localhost:3000/](https://localhost:3000) para comprobar que la nueva aplicación funciona.

## <a name="add-node-packages"></a>Agregar paquetes de nodo

Antes de seguir adelante, instale algunos paquetes adicionales que usará más adelante:

- [react-router-dom](https://github.com/ReactTraining/react-router) para el enrutamiento declarativo dentro de la React aplicación.
- [bootstrap](https://github.com/twbs/bootstrap) para el estilo y componentes comunes.
- [reactstrap para](https://github.com/reactstrap/reactstrap) React componentes basados en Bootstrap.
- [fontawesome-free](https://github.com/FortAwesome/Font-Awesome) para iconos.
- [momento](https://github.com/moment/moment) para dar formato a fechas y horas.
- [windows-iana para](https://github.com/rubenillodo/windows-iana) traducir Windows zonas horarias al formato IANA.
- [msal-browser para](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) autenticar a Azure Active Directory y recuperar tokens de acceso.
- [microsoft-graph-client para](https://github.com/microsoftgraph/msgraph-sdk-javascript) realizar llamadas a Microsoft Graph.

Ejecute el siguiente comando en la CLI.

```Shell
yarn add react-router-dom@5.2.0 bootstrap@4.6.0 reactstrap@8.9.0 @fortawesome/fontawesome-free@5.15.3
yarn add moment-timezone@0.5.33 windows-iana@5.0.2 @azure/msal-browser@2.14.1 @microsoft/microsoft-graph-client@2.2.1
yarn add -D @types/react-router-dom@5.1.7 @types/microsoft-graph@1.36.0
```

## <a name="design-the-app"></a>Diseñar la aplicación

Empieza creando una barra de navegación para la aplicación.

1. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `NavBar.tsx` código.

    :::code language="typescript" source="../demo/graph-tutorial/src/NavBar.tsx" id="NavBarSnippet":::

1. Crea una página principal para la aplicación. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Welcome.tsx` código.

    :::code language="typescript" source="../demo/graph-tutorial/src/Welcome.tsx" id="WelcomeSnippet":::

1. Cree una presentación de mensaje de error para mostrar mensajes al usuario. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `ErrorMessage.tsx` código.

    :::code language="typescript" source="../demo/graph-tutorial/src/ErrorMessage.tsx" id="ErrorMessageSnippet":::

1. Abra el archivo `./src/index.css` y reemplace todo su contenido por lo siguiente.

    :::code language="css" source="../demo/graph-tutorial/src/index.css":::

1. Abra `./src/App.tsx` y reemplace todo su contenido por lo siguiente.

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

1. Guarde todos los cambios y reinicie la aplicación. Ahora, la aplicación debe tener un aspecto muy diferente.

    ![Una captura de pantalla de la página de inicio rediseñada](images/create-app-01.png)
