<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD mediante el portal de registro de aplicaciones (ARP).

1. Abra un explorador y vaya al [portal de registro de aplicaciones](https://apps.dev.microsoft.com). Inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.

1. Seleccione **Agregar una aplicación** en la parte superior de la página.

    > [!NOTE]
    > Si ve más de un botón **Agregar una aplicación** en la página, seleccione el que corresponda a la lista de **aplicaciones convergentes** .

1. En la página **registrar la aplicación** , establezca el tutorial **nombre** de la aplicación para reaccionar **gráfico** y seleccione **crear**.

    ![Captura de pantalla de la creación de una nueva aplicación en el sitio web del portal de registro de aplicaciones](./images/arp-create-app-01.png)

1. En la página de **registro tutorial de reAct Graph** , en la sección **propiedades** , copie el **identificador de aplicación** tal y como lo necesitará más adelante.

    ![Captura de pantalla del identificador de la aplicación recién creada](./images/arp-create-app-02.png)

1. Desplácese hacia abajo hasta la sección **plataformas** .

    1. Seleccione **Agregar plataforma**.
    1. En el cuadro de diálogo **Agregar plataforma** , seleccione **Web**.

        ![Captura de pantalla que crea una plataforma para la aplicación](./images/arp-create-app-03.png)

    1. En el cuadro plataforma **Web** , escriba `http://localhost:3000` para las **direcciones URL**de redireccionamiento.

        ![Captura de pantalla de la plataforma web recién agregada para la aplicación](./images/arp-create-app-04.png)

1. Desplácese hasta la parte inferior de la página y seleccione **Guardar**.