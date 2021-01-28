<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, ampliará la aplicación del ejercicio anterior para admitir la autenticación con Azure AD. Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph. En este paso, integrará la biblioteca de la biblioteca de [autenticación de Microsoft](https://github.com/AzureAD/microsoft-authentication-library-for-js) en la aplicación.

1. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `Config.ts` código.

    :::code language="typescript" source="../demo/graph-tutorial/src/Config.example.ts":::

    Reemplace `YOUR_APP_ID_HERE` por el identificador de aplicación del Portal de registro de aplicaciones.

    > [!IMPORTANT]
    > Si usas el control de código fuente como Git, ahora sería un buen momento para excluir el archivo del control de código fuente para evitar la pérdida involuntaria del identificador `Config.ts` de la aplicación.

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

En esta sección creará un proveedor de autenticación e implementará el inicio y el cerrar sesión.

1. Cree un nuevo archivo en el `./src` directorio denominado y agregue el siguiente `AuthProvider.tsx` código.

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
      (WrappedComponent: new (props: AuthComponentProps, context?: any) => T): React.ComponentClass {
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
            error={ this.state.error }
            isAuthenticated={ this.state.isAuthenticated }
            user={ this.state.user }
            login={ () => this.login() }
            logout={ () => this.logout() }
            getAccessToken={ (scopes: string[]) => this.getAccessToken(scopes) }
            setError={ (message: string, debug: string) => this.setErrorMessage(message, debug) }
            { ...this.props } />;
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
          catch (err) {
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
            error: { message: message, debug: debug }
          });
        }

        normalizeError(error: string | Error): any {
          var normalizedError = {};
          if (typeof (error) === 'string') {
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

1. Abra `./src/App.tsx` y agregue la siguiente instrucción en la parte superior del `import` archivo.

    ```typescript
    import withAuthProvider, { AuthComponentProps } from './AuthProvider';
    ```

1. Reemplace la línea `class App extends Component<any> {` por lo siguiente.

    ```typescript
    class App extends Component<AuthComponentProps> {
    ```

1. Reemplace la línea `export default App;` por lo siguiente.

    ```typescript
    export default withAuthProvider(App);
    ```

1. Guarde los cambios y actualice el explorador. Haga clic en el botón de inicio de sesión y debería ver una ventana emergente que se `https://login.microsoftonline.com` carga. Inicie sesión con su cuenta de Microsoft y consiente los permisos solicitados. La página de la aplicación debe actualizarse mostrando el token.

### <a name="get-user-details"></a>Obtener detalles del usuario

En esta sección, se obtienen los detalles del usuario de Microsoft Graph.

1. Cree un nuevo archivo en el directorio al que `./src` se llama y agregue el siguiente `GraphService.ts` código.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="graphServiceSnippet1":::

    Esto implementa la función, que usa el SDK de Microsoft Graph para llamar al punto de `getUserDetails` conexión y devolver el `/me` resultado.

1. Abra `./src/AuthProvider.tsx` y agregue la siguiente instrucción en la parte superior del `import` archivo.

    ```typescript
    import { getUserDetails } from './GraphService';
    ```

1. Reemplace la función `getUserProfile` existente por el siguiente código.

    :::code language="typescript" source="../demo/graph-tutorial/src/AuthProvider.tsx" id="getUserProfileSnippet" highlight="6-18":::

1. Guarde los cambios e inicie la aplicación, después del inicio de sesión, debería volver a la página principal, pero la interfaz de usuario debe cambiar para indicar que ha iniciado sesión.

    ![Captura de pantalla de la página principal después de iniciar sesión](./images/add-aad-auth-01.png)

1. Haz clic en el avatar del usuario en la esquina superior derecha para acceder al vínculo **Cerrar** sesión. Al **hacer clic en** Cerrar sesión, se restablece la sesión y se vuelve a la página principal.

    ![Captura de pantalla del menú desplegable con el vínculo Cerrar sesión](./images/add-aad-auth-02.png)

## <a name="storing-and-refreshing-tokens"></a>Almacenar y actualizar tokens

En este momento, la aplicación tiene un token de acceso, que se envía en el `Authorization` encabezado de las llamadas API. Este es el token que permite que la aplicación acceda a Microsoft Graph en nombre del usuario.

Sin embargo, este token es de corta duración. El token expira una hora después de su emisión. Aquí es donde el token de actualización resulta útil. El token de actualización permite a la aplicación solicitar un nuevo token de acceso sin necesidad de que el usuario vuelva a iniciar sesión.

Dado que la aplicación usa la biblioteca de MSAL, no es necesario implementar ningún almacenamiento de tokens ni lógica de actualización. El `PublicClientApplication` token se almacena en caché en la sesión del explorador. El método comprueba primero el token almacenado en caché y, si no ha `acquireTokenSilent` expirado, lo devuelve. Si ha expirado, usa el token de actualización en caché para obtener uno nuevo. Este método se usará más en el siguiente módulo.
