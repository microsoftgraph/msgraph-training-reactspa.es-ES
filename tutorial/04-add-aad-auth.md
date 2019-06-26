<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la biblioteca de la [biblioteca de autenticación de Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) en la aplicación.

Cree un nuevo archivo en el `./src` directorio denominado `Config.js` y agregue el siguiente código.

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación del portal de registro de aplicaciones.

> [!IMPORTANT]
> Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `Config.js` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.

Abra `./src/App.js` y agregue las siguientes `import` instrucciones en la parte superior del archivo.

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

Para empezar, actualice `constructor` el para `App` la clase para crear una instancia de `UserAgentApplication` la clase y `getUser` llame a para ver si ya hay un usuario que ha iniciado sesión. Reemplace el existente `constructor` por lo siguiente.

```js
constructor(props) {
  super(props);

  this.userAgentApplication = new UserAgentApplication({
        auth: {
            clientId: config.appId
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    });

  var user = this.userAgentApplication.getAccount();

  this.state = {
    isAuthenticated: (user !== null),
    user: {},
    error: null
  };

  if (user) {
    // Enhance user object with data from Graph
    this.getUserProfile();
  }
}
```

Este código inicializa la `UserAgentApplication` clase con el identificador de la aplicación y comprueba la presencia de un usuario. Si hay un usuario, se establece `isAuthenticated` en true. El `getUserProfile` método todavía no está implementado. Lo implementará un poco más adelante.

A continuación, agregue una función a `App` la clase para realizar el inicio de sesión. Agregue la siguiente función a la clase `App`.

```js
async login() {
  try {
    await this.userAgentApplication.loginPopup(
        {
          scopes: config.scopes,
          prompt: "select_account"
      });
    await this.getUserProfile();
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

Este método llama a `loginPopup` la función para realizar el inicio de sesión y `getUserProfile` , a continuación, llama a la función.

Ahora, agregue una función a logout. Agregue la siguiente función a la clase `App`.

```js
logout() {
  this.userAgentApplication.logout();
}
```

Ahora que se `login` han `logout` implementado los métodos y, `NavBar` actualice `Welcome` los elementos y `render` en el método `App` de la clase. Reemplace el elemento `NavBar` existente por lo siguiente.

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

Reemplace el elemento `Welcome` existente por lo siguiente.

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

Por último, implemente la `getUserProfile` función. Agregue la siguiente función a la clase `App`.

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent({
        scopes: config.scopes
      });

    if (accessToken) {
      // TEMPORARY: Display the token in the error flash
      this.setState({
        isAuthenticated: true,
        error: { message: "Access token:", debug: accessToken.accessToken }
      });
    }
  }
  catch(err) {
    var errParts = err.split('|');
    this.setState({
      isAuthenticated: false,
      user: {},
      error: { message: errParts[1], debug: errParts[0] }
    });
  }
}
```

Este código llama `acquireTokenSilent` a obtener un token de acceso y, a continuación, envía el token como un error.

Guarde los cambios y actualice el explorador. Haga clic en el botón de inicio de sesión y se le `https://login.microsoftonline.com`redirigirá a. Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados. La página de la aplicación debe actualizarse y mostrar el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

Para empezar, cree un nuevo archivo que contenga todas las llamadas de Microsoft Graph. Cree un nuevo archivo en el `./src` directorio denominado `GraphService.js` y agregue el siguiente código.

```js
var graph = require('@microsoft/microsoft-graph-client');

function getAuthenticatedClient(accessToken) {
  // Initialize Graph client
  const client = graph.Client.init({
    // Use the provided access token to authenticate
    // requests
    authProvider: (done) => {
      done(null, accessToken.accessToken);
    }
  });

  return client;
}

export async function getUserDetails(accessToken) {
  const client = getAuthenticatedClient(accessToken);

  const user = await client.api('/me').get();
  return user;
}
```

Esto implementa la `getUserDetails` función, que usa el SDK de Microsoft Graph para llamar al `/me` punto de conexión y devolver el resultado.

Actualice el `getUserProfile` método en `./src/App.js` para llamar a esta función. En primer lugar, agregue `import` la siguiente instrucción a la parte superior del archivo.

```js
import { getUserDetails } from './GraphService';
```

Reemplace la función `getUserProfile` existente por el siguiente código.

```js
async getUserProfile() {
  try {
    // Get the access token silently
    // If the cache contains a non-expired token, this function
    // will just return the cached token. Otherwise, it will
    // make a request to the Azure OAuth endpoint to get a token

    var accessToken = await this.userAgentApplication.acquireTokenSilent({
      scopes: config.scopes
    });

    if (accessToken) {
      // Get the user's profile from Graph
      var user = await getUserDetails(accessToken);
      this.setState({
        isAuthenticated: true,
        user: {
          displayName: user.displayName,
          email: user.mail || user.userPrincipalName
        },
        error: null
      });
    }
  }
  catch(err) {
    var error = {};
    if (typeof(err) === 'string') {
      var errParts = err.split('|');
      error = errParts.length > 1 ?
        { message: errParts[1], debug: errParts[0] } :
        { message: err };
    } else {
      error = {
        message: err.message,
        debug: JSON.stringify(err)
      };
    }

    this.setState({
      isAuthenticated: false,
      user: {},
      error: error
    });
  }
}
```

Ahora, si guarda los cambios e inicia la aplicación, después de iniciar sesión debe terminar de nuevo en la Página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.

![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** . Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.

![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Almacenamiento y actualización de tokens

En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API. Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Aquí es donde el token de actualización se vuelve útil. El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.

Debido a que la aplicación usa la biblioteca de MSAL, no tiene que implementar ninguna lógica de almacenamiento o actualización de tokens. El `UserAgentApplication` método almacena en caché el token en la sesión del explorador. El `acquireTokenSilent` método comprueba primero el token almacenado en caché y, si no lo ha expirado, lo devuelve. Si ha expirado, usa el token de actualización almacenado en caché para obtener uno nuevo. Este método se utilizará más en el siguiente módulo.
