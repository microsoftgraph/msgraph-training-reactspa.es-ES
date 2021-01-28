# <a name="getting-started-with-create-react-app"></a><span data-ttu-id="4864b-101">Introducción a la creación de una aplicación de React</span><span class="sxs-lookup"><span data-stu-id="4864b-101">Getting Started with Create React App</span></span>

<span data-ttu-id="4864b-102">Este proyecto se ha arrancado con [Create React App.](https://github.com/facebook/create-react-app)</span><span class="sxs-lookup"><span data-stu-id="4864b-102">This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).</span></span>

## <a name="available-scripts"></a><span data-ttu-id="4864b-103">Scripts disponibles</span><span class="sxs-lookup"><span data-stu-id="4864b-103">Available Scripts</span></span>

<span data-ttu-id="4864b-104">En el directorio del proyecto, puede ejecutar:</span><span class="sxs-lookup"><span data-stu-id="4864b-104">In the project directory, you can run:</span></span>

### `yarn start`

<span data-ttu-id="4864b-105">Ejecuta la aplicación en el modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4864b-105">Runs the app in the development mode.</span></span>\
<span data-ttu-id="4864b-106">Abra [http://localhost:3000](http://localhost:3000) esta ventana para verlo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="4864b-106">Open [http://localhost:3000](http://localhost:3000) to view it in the browser.</span></span>

<span data-ttu-id="4864b-107">La página se volverá a cargar si realizas modificaciones.</span><span class="sxs-lookup"><span data-stu-id="4864b-107">The page will reload if you make edits.</span></span>\
<span data-ttu-id="4864b-108">También verá los errores de lint en la consola.</span><span class="sxs-lookup"><span data-stu-id="4864b-108">You will also see any lint errors in the console.</span></span>

### `yarn test`

<span data-ttu-id="4864b-109">Inicia el ejecutor de pruebas en el modo de reloj interactivo.</span><span class="sxs-lookup"><span data-stu-id="4864b-109">Launches the test runner in the interactive watch mode.</span></span>\
<span data-ttu-id="4864b-110">Consulta la sección sobre [la ejecución de pruebas](https://facebook.github.io/create-react-app/docs/running-tests) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4864b-110">See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.</span></span>

### `yarn build`

<span data-ttu-id="4864b-111">Crea la aplicación para producción en la `build` carpeta.</span><span class="sxs-lookup"><span data-stu-id="4864b-111">Builds the app for production to the `build` folder.</span></span>\
<span data-ttu-id="4864b-112">Agrupa correctamente React en modo de producción y optimiza la compilación para obtener el mejor rendimiento.</span><span class="sxs-lookup"><span data-stu-id="4864b-112">It correctly bundles React in production mode and optimizes the build for the best performance.</span></span>

<span data-ttu-id="4864b-113">La compilación se ha minificado y los nombres de archivo incluyen los hash.</span><span class="sxs-lookup"><span data-stu-id="4864b-113">The build is minified and the filenames include the hashes.</span></span>\
<span data-ttu-id="4864b-114">La aplicación está lista para implementarse.</span><span class="sxs-lookup"><span data-stu-id="4864b-114">Your app is ready to be deployed!</span></span>

<span data-ttu-id="4864b-115">Consulta la sección sobre [implementación](https://facebook.github.io/create-react-app/docs/deployment) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="4864b-115">See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.</span></span>

### `yarn eject`

<span data-ttu-id="4864b-116">**Nota: se trata de una operación un solo sentido. Una vez `eject` que , no puede volver atrás.**</span><span class="sxs-lookup"><span data-stu-id="4864b-116">**Note: this is a one-way operation. Once you `eject`, you can’t go back!**</span></span>

<span data-ttu-id="4864b-117">Si no estás satisfecho con la herramienta de compilación y las opciones de configuración, puedes `eject` hacerlo en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="4864b-117">If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time.</span></span> <span data-ttu-id="4864b-118">Este comando quitará la dependencia de compilación única del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4864b-118">This command will remove the single build dependency from your project.</span></span>

<span data-ttu-id="4864b-119">En su lugar, copiará todos los archivos de configuración y las dependencias transitivas (webpack, Grabl, ESLint, etc.) directamente en el proyecto para que tenga control total sobre ellos.</span><span class="sxs-lookup"><span data-stu-id="4864b-119">Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them.</span></span> <span data-ttu-id="4864b-120">Todos los comandos excepto seguirán funcionando, pero apuntarán a los scripts copiados para `eject` que puedas retocarlos.</span><span class="sxs-lookup"><span data-stu-id="4864b-120">All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them.</span></span> <span data-ttu-id="4864b-121">En este momento, está por su cuenta.</span><span class="sxs-lookup"><span data-stu-id="4864b-121">At this point you’re on your own.</span></span>

<span data-ttu-id="4864b-122">You don't have to ever use `eject` .</span><span class="sxs-lookup"><span data-stu-id="4864b-122">You don’t have to ever use `eject`.</span></span> <span data-ttu-id="4864b-123">El conjunto de características seleccionado es adecuado para implementaciones pequeñas y intermedias, y no debería sentirse obligado a usar esta característica.</span><span class="sxs-lookup"><span data-stu-id="4864b-123">The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature.</span></span> <span data-ttu-id="4864b-124">Sin embargo, sabemos que esta herramienta no sería útil si no pudiera personalizarla cuando esté listo para ella.</span><span class="sxs-lookup"><span data-stu-id="4864b-124">However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.</span></span>

## <a name="learn-more"></a><span data-ttu-id="4864b-125">Más información</span><span class="sxs-lookup"><span data-stu-id="4864b-125">Learn More</span></span>

<span data-ttu-id="4864b-126">Puede obtener más información en la documentación [de Crear aplicación de React.](https://facebook.github.io/create-react-app/docs/getting-started)</span><span class="sxs-lookup"><span data-stu-id="4864b-126">You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).</span></span>

<span data-ttu-id="4864b-127">Para obtener información sobre React, consulte la [documentación de React.](https://reactjs.org/)</span><span class="sxs-lookup"><span data-stu-id="4864b-127">To learn React, check out the [React documentation](https://reactjs.org/).</span></span>
