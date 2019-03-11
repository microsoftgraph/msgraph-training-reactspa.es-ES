# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="61860-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="61860-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61860-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="61860-102">Prerequisites</span></span>

<span data-ttu-id="61860-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="61860-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="61860-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="61860-104">[Visual Studio](https://visualstudio.microsoft.com/vs/) installed on your development machine.</span></span> <span data-ttu-id="61860-105">Si no tiene Visual Studio, visite el vínculo anterior de opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="61860-105">If you do not have Visual Studio, visit the previous link for download options.</span></span> <span data-ttu-id="61860-106">(**Nota:** este tutorial se ha escrito con Visual Studio 2017 versión 15,81.</span><span class="sxs-lookup"><span data-stu-id="61860-106">(**Note:** This tutorial was written with Visual Studio 2017 version 15.81.</span></span> <span data-ttu-id="61860-107">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="61860-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="61860-108">Una cuenta de Microsoft personal con un buzón de correo en Outlook.com o una cuenta profesional o educativa de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="61860-108">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="61860-109">Si no tiene una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="61860-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="61860-110">Puede [registrarse para obtener una nueva cuenta Microsoft personal](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="61860-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="61860-111">Puede [registrarse para el programa de desarrolladores de office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="61860-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-application-registration-portal"></a><span data-ttu-id="61860-112">Registro de una aplicación web con el portal de registro de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="61860-112">Register a web application with the Application Registration Portal</span></span>

1. <span data-ttu-id="61860-113">Abra un explorador y vaya al [portal de registro de aplicaciones](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="61860-113">Open a browser and navigate to the [Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="61860-114">Inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="61860-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="61860-115">Seleccione **Agregar una aplicación** en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="61860-115">Select **Add an app** at the top of the page.</span></span>

    > <span data-ttu-id="61860-116">**Nota:** Si ve más de un botón **Agregar una aplicación** en la página, seleccione el que corresponda a la lista de **aplicaciones convergentes** .</span><span class="sxs-lookup"><span data-stu-id="61860-116">**Note:** If you see more than one **Add an app** button on the page, select the one that corresponds to the **Converged apps** list.</span></span>

1. <span data-ttu-id="61860-117">En la página **registrar la aplicación** , establezca el tutorial **nombre** de la aplicación para reaccionar **gráfico** y seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="61860-117">On the **Register your application** page, set the **Application Name** to **React Graph Tutorial** and select **Create**.</span></span>

    ![Captura de pantalla de la creación de una nueva aplicación en el sitio web del portal de registro de aplicaciones](/tutorial/images/arp-create-app-01.png)

1. <span data-ttu-id="61860-119">En la página de **registro tutorial de reAct Graph** , en la sección **propiedades** , copie el **identificador de aplicación** tal y como lo necesitará más adelante.</span><span class="sxs-lookup"><span data-stu-id="61860-119">On the **React Graph Tutorial Registration** page, under the **Properties** section, copy the **Application Id** as you will need it later.</span></span>

    ![Captura de pantalla del identificador de la aplicación recién creada](/tutorial/images/arp-create-app-02.png)

1. <span data-ttu-id="61860-121">Desplácese hacia abajo hasta la sección **plataformas** .</span><span class="sxs-lookup"><span data-stu-id="61860-121">Scroll down to the **Platforms** section.</span></span>

    1. <span data-ttu-id="61860-122">Seleccione **Agregar plataforma**.</span><span class="sxs-lookup"><span data-stu-id="61860-122">Select **Add Platform**.</span></span>
    1. <span data-ttu-id="61860-123">En el cuadro de diálogo **Agregar plataforma** , seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="61860-123">In the **Add Platform** dialog, select **Web**.</span></span>

        ![Captura de pantalla que crea una plataforma para la aplicación](/tutorial/images/arp-create-app-03.png)

    1. <span data-ttu-id="61860-125">En el cuadro plataforma **Web** , escriba `http://localhost:3000` para las **direcciones URL**de redireccionamiento.</span><span class="sxs-lookup"><span data-stu-id="61860-125">In the **Web** platform box, enter `http://localhost:3000` for the **Redirect URLs**.</span></span>

        ![Captura de pantalla de la plataforma web recién agregada para la aplicación](/tutorial/images/arp-create-app-04.png)

1. <span data-ttu-id="61860-127">Desplácese hasta la parte inferior de la página y seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="61860-127">Scroll to the bottom of the page and select **Save**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="61860-128">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="61860-128">Configure the sample</span></span>

1. <span data-ttu-id="61860-129">Cambie el nombre `./graph-tutorial/src/Config.js.example` del archivo `./graph-tutorial/src/Config.js`a.</span><span class="sxs-lookup"><span data-stu-id="61860-129">Rename the `./graph-tutorial/src/Config.js.example` file to `./graph-tutorial/src/Config.js`.</span></span>
1. <span data-ttu-id="61860-130">Edite `./graph-tutorial/src/Config.js` el archivo y realice los cambios siguientes.</span><span class="sxs-lookup"><span data-stu-id="61860-130">Edit the `./graph-tutorial/src/Config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="61860-131">Reemplace `YOUR_APP_ID_HERE` por el **identificador de aplicación** que obtuvo desde el portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="61860-131">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="61860-132">En la interfaz de línea de comandos (CLI), navegue hasta `graph-tutorial` el directorio y ejecute el siguiente comando para instalar los requisitos.</span><span class="sxs-lookup"><span data-stu-id="61860-132">In your command-line interface (CLI), navigate to the `graph-tutorial` directory and run the following command to install requirements.</span></span>

    ```Shell
    npm install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="61860-133">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="61860-133">Run the sample</span></span>

1. <span data-ttu-id="61860-134">Ejecute el siguiente comando en su CLI para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="61860-134">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    npm start
    ```

1. <span data-ttu-id="61860-135">Abra un explorador y vaya a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="61860-135">Open a browser and browse to `http://localhost:3000`.</span></span>