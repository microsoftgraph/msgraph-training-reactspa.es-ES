<!-- markdownlint-disable MD002 MD041 -->

En esta sección, agregará la capacidad de crear eventos en el calendario del usuario.

## <a name="add-method-to-graphservice"></a>Agregar método a GraphService

1. Abra **./src/GraphService.ts** y agregue la siguiente función para crear un nuevo evento.

    :::code language="typescript" source="../demo/graph-tutorial/src/GraphService.ts" id="createEventSnippet":::

## <a name="create-new-event-form"></a>Crear nuevo formulario de eventos

1. Cree un archivo nuevo en el directorio **./src** denominado **NewEvent. TSX** y agregue el código siguiente.

    :::code language="typescript" source="../demo/graph-tutorial/src/NewEvent.tsx" id="NewEventSnippet":::

1. Abra **./src/app.TSX** y agregue la siguiente `import` instrucción a la parte superior del archivo.

    ```typescript
    import NewEvent from './NewEvent';
    ```

1. Agregue una nueva ruta al nuevo formulario de eventos. Agregue el siguiente código justo después de los otros `Route` elementos.

    ```typescript
    <Route exact path="/newevent"
      render={(props) =>
        this.props.isAuthenticated ?
          <NewEvent {...props} /> :
          <Redirect to="/" />
      } />
    ```

    La `return` instrucción completa debería tener el siguiente aspecto.

    :::code language="typescript" source="../demo/graph-tutorial/src/App.tsx" id="renderSnippet" highlight="23-28":::

1. Actualice la aplicación y vaya a la vista de calendario. Haga clic en el botón **nuevo evento** . Rellene los campos y haga clic en **crear**.

    ![Captura de pantalla del nuevo formulario de eventos](./images/create-event-01.png)
