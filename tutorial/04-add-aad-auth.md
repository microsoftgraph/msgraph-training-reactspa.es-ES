<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="14b66-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="14b66-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="14b66-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="14b66-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="14b66-103">En este paso, integrará la biblioteca de la [biblioteca de autenticación de Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="14b66-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

<span data-ttu-id="14b66-104">Cree un nuevo archivo en el `./src` directorio denominado `Config.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="14b66-104">Create a new file in the `./src` directory named `Config.js` and add the following code.</span></span>

```js
module.exports = {
  appId: 'YOUR_APP_ID_HERE',
  scopes: [
    "user.read",
    "calendars.read"
  ]
};
```

<span data-ttu-id="14b66-105">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación del portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="14b66-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="14b66-106">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `Config.js` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="14b66-106">If you're using source control such as git, now would be a good time to exclude the `Config.js` file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="14b66-107">Abra `./src/App.js` y agregue las siguientes `import` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="14b66-107">Open `./src/App.js` and add the following `import` statements to the top of the file.</span></span>

```js
import config from './Config';
import { UserAgentApplication } from 'msal';
```

## <a name="implement-sign-in"></a><span data-ttu-id="14b66-108">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="14b66-108">Implement sign-in</span></span>

<span data-ttu-id="14b66-109">Para empezar, actualice `constructor` el para `App` la clase para crear una instancia de `UserAgentApplication` la clase y `getUser` llame a para ver si ya hay un usuario que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="14b66-109">Start by updating the `constructor` for the `App` class to create an instance of the `UserAgentApplication` class and call `getUser` to see if there's already a logged-in user.</span></span> <span data-ttu-id="14b66-110">Reemplace el existente `constructor` por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="14b66-110">Replace the existing `constructor` with the following.</span></span>

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

<span data-ttu-id="14b66-111">Este código inicializa la `UserAgentApplication` clase con el identificador de la aplicación y comprueba la presencia de un usuario.</span><span class="sxs-lookup"><span data-stu-id="14b66-111">This code initializes the `UserAgentApplication` class with your application ID and checks for the presence of a user.</span></span> <span data-ttu-id="14b66-112">Si hay un usuario, se establece `isAuthenticated` en true.</span><span class="sxs-lookup"><span data-stu-id="14b66-112">If there is a user, it sets `isAuthenticated` to true.</span></span> <span data-ttu-id="14b66-113">El `getUserProfile` método todavía no está implementado.</span><span class="sxs-lookup"><span data-stu-id="14b66-113">The `getUserProfile` method isn't implemented yet.</span></span> <span data-ttu-id="14b66-114">Lo implementará un poco más adelante.</span><span class="sxs-lookup"><span data-stu-id="14b66-114">You will implement this a bit later.</span></span>

<span data-ttu-id="14b66-115">A continuación, agregue una función a `App` la clase para realizar el inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="14b66-115">Next, add a function to the `App` class to do the login.</span></span> <span data-ttu-id="14b66-116">Agregue la siguiente función a la clase `App`.</span><span class="sxs-lookup"><span data-stu-id="14b66-116">Add the following function to the `App` class.</span></span>

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

<span data-ttu-id="14b66-117">Este método llama a `loginPopup` la función para realizar el inicio de sesión y `getUserProfile` , a continuación, llama a la función.</span><span class="sxs-lookup"><span data-stu-id="14b66-117">This method calls the `loginPopup` function to do the login, then calls the `getUserProfile` function.</span></span>

<span data-ttu-id="14b66-118">Ahora, agregue una función a logout.</span><span class="sxs-lookup"><span data-stu-id="14b66-118">Now add a function to logout.</span></span> <span data-ttu-id="14b66-119">Agregue la siguiente función a la clase `App`.</span><span class="sxs-lookup"><span data-stu-id="14b66-119">Add the following function to the `App` class.</span></span>

```js
logout() {
  this.userAgentApplication.logout();
}
```

<span data-ttu-id="14b66-120">Ahora que se `login` han `logout` implementado los métodos y, `NavBar` actualice `Welcome` los elementos y `render` en el método `App` de la clase.</span><span class="sxs-lookup"><span data-stu-id="14b66-120">Now that the `login` and `logout` methods are implemented, update the `NavBar` and `Welcome` elements in the `render` method of the `App` class.</span></span> <span data-ttu-id="14b66-121">Reemplace el elemento `NavBar` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="14b66-121">Replace the existing `NavBar` element with the following.</span></span>

```JSX
<NavBar
  isAuthenticated={this.state.isAuthenticated}
  authButtonMethod={this.state.isAuthenticated ? this.logout.bind(this) : this.login.bind(this)}
  user={this.state.user}/>
```

<span data-ttu-id="14b66-122">Reemplace el elemento `Welcome` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="14b66-122">Replace the existing `Welcome` element with the following.</span></span>

```JSX
<Welcome {...props}
  isAuthenticated={this.state.isAuthenticated}
  user={this.state.user}
  authButtonMethod={this.login.bind(this)} />
```

<span data-ttu-id="14b66-123">Por último, implemente la `getUserProfile` función.</span><span class="sxs-lookup"><span data-stu-id="14b66-123">Finally, implement the `getUserProfile` function.</span></span> <span data-ttu-id="14b66-124">Agregue la siguiente función a la clase `App`.</span><span class="sxs-lookup"><span data-stu-id="14b66-124">Add the following function to the `App` class.</span></span>

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

<span data-ttu-id="14b66-125">Este código llama `acquireTokenSilent` a obtener un token de acceso y, a continuación, envía el token como un error.</span><span class="sxs-lookup"><span data-stu-id="14b66-125">This code calls `acquireTokenSilent` to get an access token, then just outputs the token as an error.</span></span>

<span data-ttu-id="14b66-126">Guarde los cambios y actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="14b66-126">Save your changes and refresh the browser.</span></span> <span data-ttu-id="14b66-127">Haga clic en el botón de inicio de sesión y se le `https://login.microsoftonline.com`redirigirá a.</span><span class="sxs-lookup"><span data-stu-id="14b66-127">Click the sign-in button and you should be redirected to `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="14b66-128">Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="14b66-128">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="14b66-129">La página de la aplicación debe actualizarse y mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="14b66-129">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="14b66-130">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="14b66-130">Get user details</span></span>

<span data-ttu-id="14b66-131">Para empezar, cree un nuevo archivo que contenga todas las llamadas de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="14b66-131">Start by creating a new file to hold all of your Microsoft Graph calls.</span></span> <span data-ttu-id="14b66-132">Cree un nuevo archivo en el `./src` directorio denominado `GraphService.js` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="14b66-132">Create a new file in the `./src` directory called `GraphService.js` and add the following code.</span></span>

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

<span data-ttu-id="14b66-133">Esto implementa la `getUserDetails` función, que usa el SDK de Microsoft Graph para llamar al `/me` punto de conexión y devolver el resultado.</span><span class="sxs-lookup"><span data-stu-id="14b66-133">This implements the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

<span data-ttu-id="14b66-134">Actualice el `getUserProfile` método en `./src/App.js` para llamar a esta función.</span><span class="sxs-lookup"><span data-stu-id="14b66-134">Update the `getUserProfile` method in `./src/App.js` to call this function.</span></span> <span data-ttu-id="14b66-135">En primer lugar, agregue `import` la siguiente instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="14b66-135">First, add the following `import` statement to the top of the file.</span></span>

```js
import { getUserDetails } from './GraphService';
```

<span data-ttu-id="14b66-136">Reemplace la función `getUserProfile` existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="14b66-136">Replace the existing `getUserProfile` function with the following code.</span></span>

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

<span data-ttu-id="14b66-137">Ahora, si guarda los cambios e inicia la aplicación, después de iniciar sesión debe terminar de nuevo en la Página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="14b66-137">Now if you save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

<span data-ttu-id="14b66-139">Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** .</span><span class="sxs-lookup"><span data-stu-id="14b66-139">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="14b66-140">Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="14b66-140">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="14b66-142">Almacenamiento y actualización de tokens</span><span class="sxs-lookup"><span data-stu-id="14b66-142">Storing and refreshing tokens</span></span>

<span data-ttu-id="14b66-143">En este punto, la aplicación tiene un token de acceso, que se envía `Authorization` en el encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="14b66-143">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="14b66-144">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="14b66-144">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="14b66-145">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="14b66-145">However, this token is short-lived.</span></span> <span data-ttu-id="14b66-146">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="14b66-146">The token expires an hour after it is issued.</span></span> <span data-ttu-id="14b66-147">Aquí es donde el token de actualización se vuelve útil.</span><span class="sxs-lookup"><span data-stu-id="14b66-147">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="14b66-148">El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.</span><span class="sxs-lookup"><span data-stu-id="14b66-148">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="14b66-149">Debido a que la aplicación usa la biblioteca de MSAL, no tiene que implementar ninguna lógica de almacenamiento o actualización de tokens.</span><span class="sxs-lookup"><span data-stu-id="14b66-149">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="14b66-150">El `UserAgentApplication` método almacena en caché el token en la sesión del explorador.</span><span class="sxs-lookup"><span data-stu-id="14b66-150">The `UserAgentApplication` method caches the token in the browser session.</span></span> <span data-ttu-id="14b66-151">El `acquireTokenSilent` método comprueba primero el token almacenado en caché y, si no lo ha expirado, lo devuelve.</span><span class="sxs-lookup"><span data-stu-id="14b66-151">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="14b66-152">Si ha expirado, usa el token de actualización almacenado en caché para obtener uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="14b66-152">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="14b66-153">Este método se utilizará más en el siguiente módulo.</span><span class="sxs-lookup"><span data-stu-id="14b66-153">You'll use this method more in the following module.</span></span>
