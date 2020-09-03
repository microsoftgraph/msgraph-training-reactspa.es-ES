<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a30a6-101">En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a30a6-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="a30a6-102">Esto es necesario para obtener el token de acceso de OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a30a6-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="a30a6-103">En este paso, integrará la biblioteca de la [biblioteca de autenticación de Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a30a6-103">In this step you will integrate the [Microsoft Authentication Library](https://github.com/AzureAD/microsoft-authentication-library-for-js) library into the application.</span></span>

1. <span data-ttu-id="a30a6-104">Cree un nuevo archivo en el `./src` directorio denominado `Config.ts` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="a30a6-104">Create a new file in the `./src` directory named `Config.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/Config.example.ts":::

    <span data-ttu-id="a30a6-105">Reemplace `YOUR_APP_ID_HERE` por el identificador de la aplicación del portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="a30a6-105">Replace `YOUR_APP_ID_HERE` with the application ID from the Application Registration Portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a30a6-106">Si usa un control de código fuente como GIT, ahora sería un buen momento para excluir el `Config.ts` archivo del control de código fuente para evitar la pérdida inadvertida del identificador de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a30a6-106">If you're using source control such as git, now would be a good time to exclude the `Config.ts` file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="a30a6-107">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="a30a6-107">Implement sign-in</span></span>

<span data-ttu-id="a30a6-108">En esta sección, creará un proveedor de autenticación e implementará el inicio y el cierre de sesión.</span><span class="sxs-lookup"><span data-stu-id="a30a6-108">In this section you'll create an authentication provider and implement sign-in and sign-out.</span></span>

1. <span data-ttu-id="a30a6-109">Cree un nuevo archivo en el `./src` directorio denominado `AuthProvider.tsx` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="a30a6-109">Create a new file in the `./src` directory named `AuthProvider.tsx` and add the following code.</span></span>

    ```typescript
    import React from 'react';
    import { PublicClientApplication } from '@azure/msal-browser';

    import { config } from './Config';

    export interface AuthComponentProps {
      error: any;
      isAuthenticated: boolean;
      user: any;
      login: Function;
      logout: Function;
      getAccessToken: Function;
      setError: Function;
    }

    interface AuthProviderState {
      error: any;
      isAuthenticated: boolean;
      user: any;
    }

    export default function withAuthProvider<T extends React.Component<AuthComponentProps>>
      (WrappedComponent: new(props: AuthComponentProps, context?: any) => T): React.ComponentClass {
      return class extends React.Component<any, AuthProviderState> {
        private publicClientApplication: PublicClientApplication;

        constructor(props: any) {
          super(props);
          this.state = {
            error: null,
            isAuthenticated: false,
            user: {}
          };

          // Initialize the MSAL application object
          this.publicClientApplication = new PublicClientApplication({
            auth: {
                clientId: config.appId,
                redirectUri: config.redirectUri
            },
            cache: {
                cacheLocation: "sessionStorage",
                storeAuthStateInCookie: true
            }
          });
        }

        componentDidMount() {
          // If MSAL already has an account, the user
          // is already logged in
          const accounts = this.publicClientApplication.getAllAccounts();

          if (accounts && accounts.length > 0) {
            // Enhance user object with data from Graph
            this.getUserProfile();
          }
        }

        render() {
          return <WrappedComponent
            error = { this.state.error }
            isAuthenticated = { this.state.isAuthenticated }
            user = { this.state.user }
            login = { () => this.login() }
            logout = { () => this.logout() }
            getAccessToken = { (scopes: string[]) => this.getAccessToken(scopes)}
            setError = { (message: string, debug: string) => this.setErrorMessage(message, debug)}
            {...this.props} {...this.state} />;
        }

        async login() {
          try {
            // Login via popup
            await this.publicClientApplication.loginPopup(
                {
                  scopes: config.scopes,
                  prompt: "select_account"
              });

            // After login, get the user's profile
            await this.getUserProfile();
          }
          catch(err) {
            this.setState({
              isAuthenticated: false,
              user: {},
              error: this.normalizeError(err)
            });
          }
        }

        logout() {
          this.publicClientApplication.logout();
        }

        async getAccessToken(scopes: string[]): Promise<string> {
          try {
            const accounts = this.publicClientApplication
              .getAllAccounts();

            if (accounts.length <= 0) throw new Error('login_required');
            // Get the access token silently
            // If the cache contains a non-expired token, this function
            // will just return the cached token. Otherwise, it will
            // make a request to the Azure OAuth endpoint to get a token
            var silentResult = await this.publicClientApplication
                .acquireTokenSilent({
                  scopes: scopes,
                  account: accounts[0]
                });

            return silentResult.accessToken;
          } catch (err) {
            // If a silent request fails, it may be because the user needs
            // to login or grant consent to one or more of the requested scopes
            if (this.isInteractionRequired(err)) {
              var interactiveResult = await this.publicClientApplication
                  .acquireTokenPopup({
                    scopes: scopes
                  });

              return interactiveResult.accessToken;
            } else {
              throw err;
            }
          }
        }

        async getUserProfile() {
          try {
            var accessToken = await this.getAccessToken(config.scopes);

            if (accessToken) {
              // TEMPORARY: Display the token in the error flash
              this.setState({
                isAuthenticated: true,
                error: { message: "Access token:", debug: accessToken }
              });
            }
          }
          catch(err) {
            this.setState({
              isAuthenticated: false,
              user: {},
              error: this.normalizeError(err)
            });
          }
        }

        setErrorMessage(message: string, debug: string) {
          this.setState({
            error: {message: message, debug: debug}
          });
        }

        normalizeError(error: string | Error): any {
          var normalizedError = {};
          if (typeof(error) === 'string') {
            var errParts = error.split('|');
            normalizedError = errParts.length > 1 ?
              { message: errParts[1], debug: errParts[0] } :
              { message: error };
          } else {
            normalizedError = {
              message: error.message,
              debug: JSON.stringify(error)
            };
          }
          return normalizedError;
        }

        isInteractionRequired(error: Error): boolean {
          if (!error.message || error.message.length <= 0) {
            return false;
          }

          return (
            error.message.indexOf('consent_required') > -1 ||
            error.message.indexOf('interaction_required') > -1 ||
            error.message.indexOf('login_required') > -1 ||
            error.message.indexOf('no_account_in_silent_request') > -1
          );
        }
      }
    }
    ```

1. <span data-ttu-id="a30a6-110">Abra `./src/App.tsx` y agregue la siguiente `import` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="a30a6-110">Open `./src/App.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';
    ```

1. <span data-ttu-id="a30a6-111">Reemplace la línea `class App extends Component<any> {` por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="a30a6-111">Replace the line `class App extends Component<any> {` with the following.</span></span>

    ```typescript
    class App extends Component<AuthComponentProps> {
    ```

1. <span data-ttu-id="a30a6-112">Reemplace la línea `export default App;` por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="a30a6-112">Replace the line `export default App;` with the following.</span></span>

    ```typescript
    export default withAuthProvider(App);
    ```

1. <span data-ttu-id="a30a6-113">Guarde los cambios y actualice el explorador.</span><span class="sxs-lookup"><span data-stu-id="a30a6-113">Save your changes and refresh the browser.</span></span> <span data-ttu-id="a30a6-114">Haga clic en el botón de inicio de sesión y verá una ventana emergente que se cargará `https://login.microsoftonline.com` .</span><span class="sxs-lookup"><span data-stu-id="a30a6-114">Click the sign-in button and you should see a pop-up window that loads `https://login.microsoftonline.com`.</span></span> <span data-ttu-id="a30a6-115">Inicie sesión con su cuenta de Microsoft y dé su consentimiento a los permisos solicitados.</span><span class="sxs-lookup"><span data-stu-id="a30a6-115">Login with your Microsoft account and consent to the requested permissions.</span></span> <span data-ttu-id="a30a6-116">La página de la aplicación debe actualizarse y mostrar el token.</span><span class="sxs-lookup"><span data-stu-id="a30a6-116">The app page should refresh, showing the token.</span></span>

### <a name="get-user-details"></a><span data-ttu-id="a30a6-117">Obtener detalles del usuario</span><span class="sxs-lookup"><span data-stu-id="a30a6-117">Get user details</span></span>

<span data-ttu-id="a30a6-118">En esta sección, obtendrá los detalles del usuario de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="a30a6-118">In this section you will get the user's details from Microsoft Graph.</span></span>

1. <span data-ttu-id="a30a6-119">Cree un nuevo archivo en el `./src` directorio denominado `GraphService.ts` y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="a30a6-119">Create a new file in the `./src` directory called `GraphService.ts` and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="graphServiceSnippet1":::

    <span data-ttu-id="a30a6-120">Esto implementa la `getUserDetails` función, que usa el SDK de Microsoft Graph para llamar al `/me` punto de conexión y devolver el resultado.</span><span class="sxs-lookup"><span data-stu-id="a30a6-120">This implements the `getUserDetails` function, which uses the Microsoft Graph SDK to call the `/me` endpoint and return the result.</span></span>

1. <span data-ttu-id="a30a6-121">Abra `./src/AuthProvider.tsx` y agregue la siguiente `import` instrucción a la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="a30a6-121">Open `./src/AuthProvider.tsx` and add the following `import` statement to the top of the file.</span></span>

    ```typescript
    import { getUserDetails } from './GraphService';
    ```

1. <span data-ttu-id="a30a6-122">Reemplace la función `getUserProfile` existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="a30a6-122">Replace the existing `getUserProfile` function with the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/AuthProvider.tsx" id="getUserProfileSnippet" highlight="6-18":::

1. <span data-ttu-id="a30a6-123">Guarde los cambios e inicie la aplicación, después de iniciar sesión, debe terminar de nuevo en la Página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.</span><span class="sxs-lookup"><span data-stu-id="a30a6-123">Save your changes and start the app, after sign-in you should end up back on the home page, but the UI should change to indicate that you are signed-in.</span></span>

    ![Una captura de pantalla de la Página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. <span data-ttu-id="a30a6-125">Haga clic en el avatar de usuario en la esquina superior derecha para acceder al vínculo **Cerrar sesión** .</span><span class="sxs-lookup"><span data-stu-id="a30a6-125">Click the user avatar in the top right corner to access the **Sign Out** link.</span></span> <span data-ttu-id="a30a6-126">Al hacer clic en **cerrar** sesión se restablece la sesión y se vuelve a la Página principal.</span><span class="sxs-lookup"><span data-stu-id="a30a6-126">Clicking **Sign Out** resets the session and returns you to the home page.</span></span>

    ![Captura de pantalla del menú desplegable con el vínculo cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a><span data-ttu-id="a30a6-128">Almacenamiento y actualización de tokens</span><span class="sxs-lookup"><span data-stu-id="a30a6-128">Storing and refreshing tokens</span></span>

<span data-ttu-id="a30a6-129">En este punto, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas a la API.</span><span class="sxs-lookup"><span data-stu-id="a30a6-129">At this point your application has an access token, which is sent in the `Authorization` header of API calls.</span></span> <span data-ttu-id="a30a6-130">Este es el token que permite que la aplicación tenga acceso a Microsoft Graph en nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="a30a6-130">This is the token that allows the app to access the Microsoft Graph on the user's behalf.</span></span>

<span data-ttu-id="a30a6-131">Sin embargo, este token es de corta duración.</span><span class="sxs-lookup"><span data-stu-id="a30a6-131">However, this token is short-lived.</span></span> <span data-ttu-id="a30a6-132">El token expira una hora después de su emisión.</span><span class="sxs-lookup"><span data-stu-id="a30a6-132">The token expires an hour after it is issued.</span></span> <span data-ttu-id="a30a6-133">Aquí es donde el token de actualización se vuelve útil.</span><span class="sxs-lookup"><span data-stu-id="a30a6-133">This is where the refresh token becomes useful.</span></span> <span data-ttu-id="a30a6-134">El token de actualización permite que la aplicación solicite un nuevo token de acceso sin que el usuario tenga que iniciar sesión de nuevo.</span><span class="sxs-lookup"><span data-stu-id="a30a6-134">The refresh token allows the app to request a new access token without requiring the user to sign in again.</span></span>

<span data-ttu-id="a30a6-135">Debido a que la aplicación usa la biblioteca de MSAL, no tiene que implementar ninguna lógica de almacenamiento o actualización de tokens.</span><span class="sxs-lookup"><span data-stu-id="a30a6-135">Because the app is using the MSAL library, you do not have to implement any token storage or refresh logic.</span></span> <span data-ttu-id="a30a6-136">El `PublicClientApplication` almacena en caché el token en la sesión del explorador.</span><span class="sxs-lookup"><span data-stu-id="a30a6-136">The `PublicClientApplication` caches the token in the browser session.</span></span> <span data-ttu-id="a30a6-137">El `acquireTokenSilent` método comprueba primero el token almacenado en caché y, si no lo ha expirado, lo devuelve.</span><span class="sxs-lookup"><span data-stu-id="a30a6-137">The `acquireTokenSilent` method first checks the cached token, and if it is not expired, it returns it.</span></span> <span data-ttu-id="a30a6-138">Si ha expirado, usa el token de actualización almacenado en caché para obtener uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="a30a6-138">If it is expired, it uses the cached refresh token to obtain a new one.</span></span> <span data-ttu-id="a30a6-139">Este método se utilizará más en el siguiente módulo.</span><span class="sxs-lookup"><span data-stu-id="a30a6-139">You'll use this method more in the following module.</span></span>
