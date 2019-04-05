<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="efbb7-101">En este ejercicio, creará un nuevo registro de aplicaciones Web de Azure AD con el centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="efbb7-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="efbb7-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="efbb7-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="efbb7-103">Inicie sesión con una **cuenta personal** (también conocido como Microsoft Account) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="efbb7-104">Seleccione **Azure Active Directory** en el panel de navegación de la izquierda y, después, seleccione **registros de aplicaciones (vista previa)** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations (Preview)** under **Manage**.</span></span>

    ![<span data-ttu-id="efbb7-105">Una captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="efbb7-105">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="efbb7-106">Seleccione **registro nuevo**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-106">Select **New registration**.</span></span> <span data-ttu-id="efbb7-107">En la página **registrar una aplicación** , establezca los valores de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="efbb7-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="efbb7-108">Establezca **el nombre** en `React Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="efbb7-108">Set **Name** to `React Graph Tutorial`.</span></span>
    - <span data-ttu-id="efbb7-109">Establezca **tipos de cuenta compatibles** en **cuentas de cualquier directorio de la organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="efbb7-110">En **URI**de redireccionamiento, establezca la primera lista desplegable para `Web` y establezca el `http://localhost:3000`valor en.</span><span class="sxs-lookup"><span data-stu-id="efbb7-110">Under **Redirect URI**, set the first drop-down to `Web` and set the value to `http://localhost:3000`.</span></span>

    ![Captura de pantalla de la página registrar una aplicación](./images/aad-register-an-app.png)

1. <span data-ttu-id="efbb7-112">Elija **registrar**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-112">Choose **Register**.</span></span> <span data-ttu-id="efbb7-113">En la página **tutorial de gráfico** de angular, copie el valor del identificador de la **aplicación (cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="efbb7-113">On the **Angular Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de la aplicación del nuevo registro de la aplicación](./images/aad-application-id.png)

1. <span data-ttu-id="efbb7-115">Seleccione **autenticación** en **administrar**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="efbb7-116">Busque la sección **concesión implícita** y habilite **tokens de acceso** y tokens de **identificador**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="efbb7-117">Elija **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="efbb7-117">Choose **Save**.</span></span>

    ![Captura de pantalla de la sección de concesión implícita](./images/aad-implicit-grant.png)
