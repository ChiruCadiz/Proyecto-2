# Proyecto Django

## 1. Integración de la API en la Aplicación

Este proyecto integra una API para poder gestionar y analizar un modelo de datos relacionados de ventas ficticias. La API fue integrada mediante el uso de Django, Django REST, pathlib, request, json, etc, para permitir la comunicación entre el frontend y el backend, asegurando que los datos se gestionen de manera eficiente. 

Para consumir la API desde el frontend, se utilizó fetch, estableciendo conexiones seguras y optimizadas.



## 2. Decisiones Técnicas Tomadas

Para el desarrollo de este proyecto, se eligieron las siguientes tecnologías y herramientas:

-   **Django**: Seleccionado  debido a su robustez, facilidad para gestionar bases de datos relacionales, y su amplio ecosistema de extensiones y bibliotecas, como Django REST Framework, que facilita la creación de APIs RESTful.
-   **Base de Datos**: El gestor  de BBDD utilizado fue ***Laragon*** debido a su interfaz intuitiva, facilidad de hacer consultas y compatibilidad con bases tipo MySQL

## 3. Componentes/Vistas Creados

A continuación, se describen los principales componentes y vistas creados para el proyecto:

### Vistas:
A continucaion de detallan las vistas de la aplicacion unica ***ventas***:
-   **[Inicio]**: Esta vista carga la pagina inicial, utilizando `render` envía de respuesta la plantilla *inicio.html*
-   **[Crear cliente]**: Esta vista permite crear un nuevo usuario, recoge datos de un formulario `POST` y realiza una solicitud `POST` a la API para registrar al cliente. Si el registro es exitoso, redirige a la lista de pedidos .
-   **[Crear comercial]**: Esta vista permite crear un nuevo comercial, recoge datos de un formulario `POST` y realiza una solicitud `POST` a la API para registrar al cliente. Si el registro es exitoso, redirige a la lista de pedidos.
-   **[Crear pedido]**: Esta vista permite crear un nuevo pedido, si el método es `POST`, envía los datos del pedido a la API para registrar el pedido. Si el registro es exitoso, redirige a la lista de pedidos.
-   **[Listar pedido]**: Esta vista muestra la lista de todos los pedidos registrados, obtiene todos los pedidos de la base de datos y prepara datos para posibles gráficos (fechas, totales, etiquetas de cliente).
-   **[Modificar pedido]**: Esta vista permite modificar un pedido existente, si el método es `POST`, actualiza el pedido con los datos del formulario. Si no, carga el formulario con los datos actuales del pedido.
-   **[Actualizar pedido]**: Esta vista actualiza un pedido existente de manera asincrónica, recibe datos en formato JSON, los procesa y actualiza el pedido correspondiente.
-   **[Eliminar pedido]**: Esta vista elimina un pedido de manera asincrónica, requiere una solicitud `POST`, cual localiza el pedido con `get_object_or_404`, lo elimina, y devuelve un `JsonResponse` con `{'success': True}`.
-   **[Ventas por cliente]**: Esta vista genera datos agregados de ventas para un cliente específico, agrupados por fecha. Filtra los pedidos según el `cliente_id` recibido en la solicitud, agrupa los pedidos por fecha y calcula el total de ventas por día usando `Sum`, formatea los datos en listas para fechas (`fechas`) y totales (`totales`) y responde con un `JsonResponse`.


### Componentes:

De los distintos tipos de componentes tenemos los que son:

#### ***Modelos***:
La API posee una app unica llamada ***ventas*** la cual tiene los siguientes 3 modelos:

1.  **[Clientes]**: Obtiene la información de: Nombre, Apellido 1, Apellido 2, Ciudad y Categoría, de la tabla *cliente* de la BBDD ***ventas***
2.  **[Comercial]**: Obtiene la información de: Nombre, Apellido 1, Apellido 2 y Comisión, de la tabla *cliente* de la BBDD ***ventas***
3.  **[Pedido]**: Obtiene la información de: Total, Fecha, Cliente y Comercial, de la tabla *cliente* de la BBDD ***ventas***

#### ***Serializadores***:
Existen tres ***serializadores*** que transforman los datos de los modelos en un formato adecuado para la API, en este caso JSON
1.  **[Clientes serializer]**: Serializa al modelo ***Clientes***
2.  **[Comercial serializer]**: Serializa al modelo ***Comercial***
3.  **[Pedido serializer]**: Serializa al modelo ***Pedido***

#### ***Formularios***:
Se utiliza un unico formulario, este es:

-**Pedido form**: Permite crear o actualizar pedidos en la aplicación. Dado que se basa en `ModelForm`, facilita la validación y la manipulación de datos del modelo `Pedido` de manera automática.

#### ***URLs***:
Existen dos archivos de rutas, las rutas para ***API***:

-   **Rutas para Clientes**:
    
    -   `clientes/`: Permite listar y crear clientes. Usa la vista `ClienteListCreate`, y se accede con el nombre `cliente-list-create`.
    -   `clientes/<int:pk>/`: Permite ver, actualizar o eliminar un cliente específico, usando la vista `ClienteRetrieveUpdateDestroy`, y se accede con el nombre `cliente-detail`.
-   **Rutas para Comerciales**:
    
    -   `comerciales/`: Permite listar y crear comerciales, a través de la vista `ComercialListCreate`, con el nombre `comercial-list-create`.
    -   `comerciales/<int:pk>/`: Permite ver, actualizar o eliminar un comercial específico, usando la vista `ComercialRetrieveUpdateDestroy`, con el nombre `comercial-detail`.
-   **Rutas para Pedidos**:
    
    -   `pedidos/`: Permite listar y crear pedidos. Usa la vista `PedidoListCreate` y el nombre `pedido-list-create`.
    -   `pedidos/<int:pk>/`: Permite ver, actualizar o eliminar un pedido específico, usando `PedidoRetrieveUpdateDestroy`, con el nombre `pedido-detail`.
    -   `pedidos/<int:pk>/delete/`: Permite eliminar un pedido específico, con la vista `PedidoDelete` y el nombre `pedido-delete`.
    -   `api/pedidos/<int:pk>/update/`: Permite actualizar un pedido específico usando `PedidoUpdate`, con el nombre `pedido-update`.
Y las rutas para ***ventas***:

-   **Ruta para la Página de Inicio**:
    
    -   `''` (raíz): Muestra la página de inicio, usando la vista `inicio`, con el nombre `inicio`.
-   **Rutas para Crear Elementos**:
    
    -   `crear_cliente/`: Muestra el formulario para crear un nuevo cliente, usando la vista `crear_cliente`.
    -   `crear_comercial/`: Muestra el formulario para crear un nuevo comercial, usando la vista `crear_comercial`.
    -   `crear_pedido/`: Muestra el formulario para crear un nuevo pedido, usando la vista `crear_pedido`.
-   **Rutas para Listar y Modificar Pedidos**:
    
    -   `listar_pedidos/`: Permite ver todos los pedidos, con la vista `listar_pedidos`.
    -   `eliminar_pedido/<int:id>/`: Permite eliminar un pedido específico, usando la vista `eliminar_pedido`.
    -   `editar-pedido/<int:id>/`: Permite modificar un pedido específico, con la vista `modificar_pedido`.
-   **Rutas para Consultas Específicas de Ventas**:
    
    -   `api/ventas/cliente/<int:cliente_id>/`: Permite obtener los datos de ventas para un cliente específico, usando la vista `ventas_por_cliente`.
-   **Inclusión de las Rutas de la API**:
    
    -   `api/`: Incluye todas las rutas definidas en `api.urls`, integrando las rutas de la API en el proyecto.

#### ***Plantillas***:
Las plantillas son archivos `.html` cuales definen la estructura y diseño visual del contenido, las que tiene el proyecto son las siguientes:

***base.html***:
-   **Declaración inicial**: Define el documento como HTML y establece el idioma en español (`<html lang="es">`).
    
-   **Cabeza (`<head>`)**:
    
    -   **Meta charset**: Configura el conjunto de caracteres como UTF-8, asegurando una correcta visualización de caracteres especiales.
    -   **Título (`<title>`)**: Define un bloque `title` para que cada página hija pueda personalizar su título; el título predeterminado es "Gestión de Ventas".
    -   **Cargar archivos estáticos**: Utiliza `{% load static %}` para habilitar el uso de archivos estáticos.
    -   **CSS**: Incluye la hoja de estilos `styles.css` desde la carpeta estática.
    -   **Chart.js**: Carga el CDN de `Chart.js` para gráficos interactivos.
    -   **JavaScript**: Incluye `scripts.js`, el archivo de JavaScript personalizado.
-   **Cuerpo (`<body>`)**:
    
    -   **Encabezado (`<header>`)**: Muestra el título principal de la aplicación ("Gestión de Ventas").
    -   **Contenido principal (`<main>`)**: Define un bloque `content` donde las páginas hijas insertarán su contenido específico.

Este archivo sirve como plantilla base para todas las demás páginas, proporcionando una estructura coherente con estilos y scripts predefinidos.

***confirmar_eliminacion***:
-   **Extiende `base.html`**: Usa `{% extends 'base.html' %}` para heredar la estructura de la plantilla base, que incluye el título y los estilos comunes.
    
-   **Bloque de Título (`{% block title %}`)**: Sobrescribe el bloque de título de la base para que esta página se titule "Confirmar Eliminación".
    
-   **Bloque de Contenido (`{% block content %}`)**:
    
    -   **Encabezado**: Muestra el título "Confirmar Eliminación" en un `<h1>`.
    -   **Detalles del Pedido**: Muestra información clave sobre el pedido que se va a eliminar:
        -   **ID del pedido** (`pedido.id`).
        -   **Total del pedido** (`pedido.total`).
        -   **Fecha del pedido** (`pedido.fecha`).
        -   **Nombre del cliente** (`pedido.cliente.nombre` y `pedido.cliente.apellido1`).
        -   **Nombre del comercial** (`pedido.comercial.nombre` y `pedido.comercial.apellido1`).
    -   **Formulario de Confirmación**:
        -   Utiliza `method="POST"` para enviar la solicitud de eliminación.
        -   Incluye `{% csrf_token %}` para proteger contra ataques CSRF.
        -   **Botón "Eliminar"**: Envía el formulario para confirmar la eliminación.
        -   **Enlace "Cancelar"**: Redirige al usuario a la página de listado de pedidos usando `{% url 'listar_pedidos' %}`.

Esta página sirve para confirmar la eliminación de un pedido específico, mostrando los detalles antes de proceder o cancelar la acción.

***crear_clientes***:
-   **Extiende `base.html`**: Usa `{% extends 'base.html' %}` para heredar la estructura general y estilos de la plantilla base.
    
-   **Bloque de Título (`{% block title %}`)**: Define el título de la página como "Crear Cliente".
    
-   **Bloque de Contenido (`{% block content %}`)**:
    
    -   **Formulario de Creación de Cliente**:
        
        -   Usa `method="post"` para enviar los datos del formulario al servidor.
        -   Incluye `{% csrf_token %}` para la protección contra ataques CSRF.
        -   **Campos del Formulario**:
            -   **Nombre**: Campo de texto obligatorio (`name="nombre"` y `id="nombre"`).
            -   **Apellido1**: Campo de texto obligatorio para el primer apellido (`name="apellido1"` y `id="apellido1"`).
            -   **Apellido2**: Campo de texto opcional para el segundo apellido (`name="apellido2"` y `id="apellido2"`).
            -   **Ciudad**: Campo de texto obligatorio para la ciudad (`name="ciudad"` y `id="ciudad"`).
            -   **Categoría**: Campo numérico para ingresar la categoría del cliente (`name="categoria"` y `id="categoria"`).
        -   **Botón "Crear Cliente"**: Envía el formulario para crear el cliente.
    -   **Botón "Volver al Inicio"**:
        
        -   Redirige al usuario a la página de inicio al hacer clic, usando la URL `{% url 'inicio' %}`.

Esta página permite al usuario crear un nuevo cliente ingresando información básica, con la opción de regresar al inicio si lo desea.

***crear_comercial***:
-   **Extiende `base.html`**: Usa `{% extends 'base.html' %}` para heredar la estructura y los estilos de la plantilla base.
    
-   **Bloque de Título (`{% block title %}`)**: Establece el título de la página como "Crear Comercial".
    
-   **Bloque de Contenido (`{% block content %}`)**:
    
    -   **Formulario de Creación de Comercial**:
        
        -   Usa `method="POST"` y `action="/api/comerciales/"` para enviar los datos del formulario al servidor a la URL especificada.
        -   Incluye `{% csrf_token %}` para protección contra ataques CSRF.
        -   **Campos del Formulario**:
            -   **Nombre**: Campo de texto obligatorio para ingresar el nombre del comercial (`name="nombre"` y `id="nombre"`).
            -   **Primer Apellido**: Campo de texto obligatorio para el primer apellido (`name="apellido1"` y `id="apellido1"`).
            -   **Segundo Apellido**: Campo de texto opcional para el segundo apellido (`name="apellido2"` y `id="apellido2"`).
            -   **Comisión**: Campo numérico para ingresar la comisión del comercial, permite decimales con `step="0.01"` (`name="comision"` y `id="comision"`).
        -   **Botón "Guardar Comercial"**: Envía el formulario para guardar el nuevo comercial.
    -   **Botón "Volver al Inicio"**:
        
        -   Redirige al usuario a la página de inicio usando la URL `{% url 'inicio' %}` al hacer clic.

Esta página permite al usuario crear un nuevo registro de comercial con datos básicos y comisión, además de ofrecer la opción de regresar al inicio.

***crear_pedido***:
-   **Extiende `base.html`**: Usa `{% extends 'base.html' %}` para heredar estructura y estilos de la plantilla base.
    
-   **Bloque de Título (`{% block title %}`)**: Define el título de la página como "Crear Pedido".
    
-   **Bloque de Contenido (`{% block content %}`)**:
    
    -   **Encabezado**: Muestra el título "Crear Pedido".
        
    -   **Formulario para Crear Pedido**:
        
        -   Usa `method="POST"` para enviar los datos del formulario.
            
        -   Incluye `{% csrf_token %}` para protección contra ataques CSRF.
            
        -   **Campos del Formulario**:
            
            -   **Total**: Campo numérico obligatorio para ingresar el total del pedido. Permite decimales con `step="0.01"`.
            -   **Fecha**: Campo de tipo fecha obligatorio para seleccionar la fecha del pedido.
            -   **Cliente**: Lista desplegable para seleccionar un cliente existente, basada en los datos de `clientes` disponibles en el contexto.
                -   Usa un bucle `{% for cliente in clientes %}` para crear una opción por cada cliente.
                -   Cada opción muestra el nombre y primer apellido del cliente.
            -   **Comercial**: Lista desplegable para seleccionar un comercial existente, basada en los datos de `comerciales` en el contexto.
                -   Usa un bucle `{% for comercial in comerciales %}` para crear una opción por cada comercial.
                -   Cada opción muestra el nombre y primer apellido del comercial.
        -   **Botón "Crear Pedido"**: Envía el formulario para registrar el nuevo pedido.
            
    -   **Botón "Volver al Inicio"**: Redirige al usuario a la página de inicio utilizando `{% url 'inicio' %}` cuando se hace clic.
        

Este formulario permite crear un pedido, asignando un cliente y un comercial, además de especificar el total y la fecha. También proporciona la opción de regresar a la página de inicio.

***inicio***:
-   **Extiende `base.html`**: Usa `{% extends 'base.html' %}` para heredar la estructura y los estilos de la plantilla base.
    
-   **Bloque de Título (`{% block title %}`)**: Define el título de la página como "Inicio".
    
-   **Bloque de Contenido (`{% block content %}`)**:
    
    -   **Contenedor de Botones (`div.button-container`)**:
        -   **Botones de Navegación**:
            -   **"Crear Cliente"**: Botón que redirige a la página de creación de cliente (`{% url 'crear_cliente' %}`).
            -   **"Crear Comercial"**: Botón que redirige a la página de creación de comercial (`{% url 'crear_comercial' %}`).
            -   **"Crear Pedido"**: Botón que redirige a la página de creación de pedido (`{% url 'crear_pedido' %}`).
            -   **"Listar Pedidos"**: Botón que redirige a la página de listado de pedidos (`{% url 'listar_pedidos' %}`).

Este diseño proporciona una interfaz de inicio sencilla con botones para acceder rápidamente a las páginas de creación de clientes, comerciales y pedidos, además de ver el listado de pedidos.

***listar_pedidos***:
### Estructura General

-   **Extiende `base.html`**: Usa `{% extends 'base.html' %}` para heredar la estructura y los estilos básicos de la plantilla.
    
-   **Título** (`{% block title %}`): Se define el título de la página como "Listar Pedidos".
    

### Contenido Principal

-   **Encabezado**: Un título `Listado de Pedidos`.
    
-   **Botón de Retorno**: Permite volver a la página de inicio.
    
-   **Tabla de Pedidos**:
    
    -   Contiene una lista de pedidos, donde cada fila muestra:
        -   **ID del Pedido**
        -   **Total**
        -   **Fecha**
        -   **Cliente**
        -   **Comercial**
        -   **Acciones**: Contiene botones para editar y eliminar cada pedido.

### Funcionalidad de Gráficos

-   **Botones de Gráficos**:
    
    -   **Mostrar Gráfico por Cliente**: Despliega un gráfico de línea para visualizar las ventas por fecha.
    -   **Ver Gráfico de Barras**: Muestra un gráfico de barras con el total de ventas por cliente.
    -   **Ver Gráfico de Torta**: Muestra un gráfico de torta con la participación de cada cliente en las ventas.
-   **Modales para Gráficos**:
    
    -   Cada gráfico se muestra en un modal (ventana emergente), que se centra en la pantalla y permite cerrar el gráfico al hacer clic en la "X".

### Scripts y Funciones

-   **Función `displayChart`**:
    
    -   Configura y muestra el gráfico en el contexto especificado.
-   **Funciones `showChart`, `showBarChart`, `showPieChart`**:
    
    -   Configuran diferentes tipos de gráficos (línea, barras, torta) y llaman a `displayChart` con los datos correspondientes.
-   **Función `showLineChartAllClients`**:
    
    -   Carga datos de ventas de todos los clientes y muestra un gráfico de líneas con cada cliente como una serie distinta.
-   **Acciones de la Tabla**:
    
    -   **Editar Pedido**: Redirige a una página para editar el pedido seleccionado.
    -   **Eliminar Pedido**: Solicita confirmación antes de enviar una solicitud de eliminación al backend. Si es exitosa, elimina la fila correspondiente en la tabla.

### Estilos para Modales

Los modales están diseñados para centrarse en la pantalla, tener un fondo semitransparente y un botón de cierre.

Esta plantilla proporciona una interfaz completa para visualizar, editar, eliminar pedidos y ver análisis visuales de ventas, haciendo uso de gráficos y modales en JavaScript para mejorar la experiencia de usuario.

***modificar_pedido***:
- **Extiende `base.html`**: Usa elementos comunes de la plantilla `base.html` para mantener consistencia visual.
    
 - **Título**: Asigna "Modificar Pedido" como el título de la página.
    
- **Formulario de modificación**:
    
    -   Muestra un formulario que permite editar un pedido.
    -   Incluye `{% csrf_token %}` para proteger la solicitud.
    -   `{{ form.as_p }}` genera automáticamente los campos del formulario.
-  **Botón de regreso**:
    
    -   Botón que redirige a la página de lista de pedidos usando la vista `listar_pedidos`.


#### ***Archivos estáticos***:
Se tienen dos archivos estaticos, estos siendo ***scripts.js*** y ***styles.css***.
El archivo `scripts.js` contiene varias funciones para manejar la interfaz de usuario en una aplicación web de ventas. Aquí te explico cada sección brevemente:

-   **Manejo de Edición y Eliminación de Pedidos**:
    
    -   Asigna eventos de clic a botones de edición y eliminación de pedidos.
    -   Al hacer clic en "editar", abre un modal de edición con datos específicos del pedido (ID, total, fecha).
    -   Al hacer clic en "eliminar", solicita confirmación y, si se acepta, elimina el pedido llamando a una API.
-   **Funciones para Modales**:
    
    -   `openEditModal()`: Muestra el modal de edición, cargando los datos del pedido.
    -   `closeModal()`: Cierra el modal especificado (edición o general).
    -   `closeLineChartModal()`: Cierra el modal de gráficos de líneas.
-   **Actualizar y Eliminar Pedido**:
    
    -   `updatePedido()`: Envía una solicitud `POST` a la API para actualizar el pedido en el servidor.
    -   `deletePedido()`: Elimina un pedido tras confirmar, eliminando también la fila correspondiente en la interfaz.
-   **Visualización de Gráficos**:
    
    -   `showChart()`, `showBarChart()`, `showPieChart()`: Crean gráficos de línea, barra o pastel con los datos de ventas utilizando la biblioteca `Chart.js`.
    -   `displayChart()`: Función común para mostrar gráficos, utilizando el tipo y los datos proporcionados.
-   **Gráficos de Ventas por Cliente**:
    
    -   `showLineChart()`: Muestra un gráfico de línea de ventas para un cliente específico, obteniendo los datos a través de una API.
    -   `showLineChartAllClients()`: Muestra un gráfico de línea para las ventas de todos los clientes, con un color diferente para cada uno.

Estas funciones están diseñadas para hacer la experiencia de usuario más dinámica, permitiendo editar, eliminar pedidos, y visualizar datos en gráficos detallados directamente en la interfaz.

El archivo `styles.css` define el estilo visual de la aplicación, asegurando una apariencia limpia, moderna y coherente. Las secciones de este archivo son:

-   **Estilos Generales**:
    
    -   Define el estilo general del `body` con una fuente sans-serif, color de fondo claro y texto en color oscuro. Alinea el contenido al centro para una presentación centrada en la pantalla.
-   **Títulos**:
    
    -   Establece los colores y márgenes de los encabezados (`h1` y `h2`), usando un tono verde (`#4CAF50`) para destacarlos.
-   **Formulario**:
    
    -   Aplica un fondo blanco, relleno, sombra y bordes redondeados al formulario para que se vea más profesional. Limita su ancho para una mejor legibilidad.
    -   Los elementos de formulario (`label`, `input`, `select`) tienen un estilo coherente, con etiquetas en negrita y campos de entrada adaptables que ocupan todo el ancho disponible.
-   **Botones**:
    
    -   Los botones tienen un fondo verde, texto blanco, y son de ancho completo con bordes redondeados. Al pasar el mouse, cambian a un verde más oscuro para dar retroalimentación visual.
-   **Tabla**:
    
    -   La clase `.table-container` da estilo a la tabla para una apariencia organizada, con celdas acolchonadas, un borde inferior en cada fila, y un fondo blanco.
    -   Los encabezados de columna (`th`) son verdes con texto blanco, diferenciando las filas de los encabezados.
-   **Contenedor de Botones**:
    
    -   La clase `.button-container` organiza los botones en fila, separados por un espacio (`gap: 10px`).
-   **Modal (Ventanas Emergentes)**:
    
    -   Define la estructura y el estilo de los modales, que están inicialmente ocultos (`display: none`). Los modales ocupan toda la pantalla con un fondo oscuro semitransparente.
    -   `.modal-content` es el contenedor de contenido dentro del modal, centrado en la pantalla, con un fondo claro, relleno, y un borde.
    -   `.close` establece el estilo del botón de cerrar, con un cambio de color en hover para indicar que es interactivo.
-   **Formulario de Edición**:
    
    -   `.edit-form` utiliza un diseño de columna con espacio (`gap: 10px`) entre los elementos, facilitando la disposición y lectura de los campos de edición.

El archivo `styles.css` crea una interfaz agradable y fácil de usar, asegurando que cada componente (formulario, tabla, modales) sea claro y visualmente atractivo.

