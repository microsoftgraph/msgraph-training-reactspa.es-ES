<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD con el centro de administración de Azure Active Directory.

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones (vista previa)** en **administrar**.

    ![Una captura de pantalla de los registros de la aplicación ](./images/aad-portal-app-registrations.png)

1. Seleccione **registro nuevo**. En la página **registrar una aplicación** , establezca los valores de la siguiente manera.

    - Establezca **el nombre** en `React Graph Tutorial`.
    - Establezca **tipos de cuenta compatibles** en **cuentas de cualquier directorio de la organización y cuentas personales de Microsoft**.
    - En **URI**de redireccionamiento, establezca la primera lista desplegable para `Web` y establezca el `http://localhost:3000`valor en.

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. Elija **registrar**. En la página **tutorial de gráfico** de angular, copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. Seleccione **autenticación** en **administrar**. Busque la sección **concesión implícita** y habilite **tokens de acceso** y tokens de **identificador**. Elija **Guardar**.

    ![Captura de pantalla de la sección de concesión implícita](./images/aad-implicit-grant.png)
