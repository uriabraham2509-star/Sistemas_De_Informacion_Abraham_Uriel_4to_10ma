====================================================
Manual Paso a Paso: Integración de Base de Datos SQL con "Node.js" y "JavaScript".
Este manual describe el procedimiento detallado para instalar, configurar y desarrollar una aplicación web con Node.js, Express y SQLite (better-sqlite3), permitiendo insertar y consultar información mediante un formulario web.
En caso de querer ver un video con el paso a paso pueden ingresar en este video
“https://youtu.be/sMPdYVt3UTI?si=cXVMnQtfxZbw1AGK”

1.Requisitos y preparación del entorno.

Node.js Windows 10
Node.js Linux

Antes de comenzar, asegúrate de tener instalado:
 • Node.js: Puedes verificar la instalación en la terminal con "node -v"
 • Un editor de código (como Visual Studio Code, Intellij o el mismo Bloc de notas).

Pasos de inicialización:
 • Abre la terminal o consola de comandos (CMD).
 • Crea un directorio de trabajo y navega hasta él:
"mkdir proyecto-sqlite-node" (esto creará una carpeta)
"cd proyecto-sqlite-node" (y con “cd” entrará a la carpeta)

 • Inicializa el proyecto Node.js para generar el archivo package.json: "npm init -y"

 • Instala las librerías necesarias:
 • Express: Para la creación de la API y servidor HTTP, para permitir que el formulario reciba nuestras peticiones:
"npm install express"

 • Better-SQLite3: El conector nativo y ligero de SQLite para Node.js, permitiendo crear, leer y modificar la base de datos:
"npm install better-sqlite3"

2 Creación del Servidor y Configuración del Conector SQL.

Crea un archivo llamado "server.js" en la raíz del proyecto. En este archivo se realiza la inicialización de la base de datos, la creación de la estructura de tablas y el mapeo de rutas.

3 Interfaz de Usuario y Lógica Cliente.

Crea una carpeta llamada “public” en el directorio principal y dentro de ella crea un archivo “index.html”.



4 Estructura Final del Proyecto
La jerarquía de archivos debe quedar de la siguiente manera:
proyecto-sqlite-node/
├── node_modules/
├── public/
│   └── index.html
├── package.json
├── package-lock.json
├── server.js
└── sistema.db (se crea automáticamente al iniciar)

5. Verificación y Pruebas
 1. Iniciar el Servidor.
Abre la terminal en la raíz de la carpeta del proyecto y ejecuta:
"node server.js"

Verás el mensaje: Servidor activo en "http://localhost:3000"

 2. Acceder a la Aplicación Web.

Abre el navegador web de tu preferencia e ingresa a la dirección: "http://localhost:3000"

 3. Probar Inserción de Datos (INSERT)
Ingresa un Nombre y un Correo en los campos del formulario y presiona Guardar Registro. Verifica que el servidor procesa la petición y se crea el archivo sistema.db automáticamente en caso de no existir.

 4. Probar Extracción de Datos (SELECT)
Comprueba que el elemento de la tabla se actualiza de manera inmediata sin recargar la página, haciendo la llamada fetch al endpoint GET.
=====================================================

