====================================================
Manual Paso a Paso: Integración de Base de Datos SQL con "Node.js" y "JavaScript".
Este manual describe el procedimiento detallado para instalar, configurar y desarrollar una aplicación web con Node.js, Express y SQLite (better-sqlite3), permitiendo insertar y consultar información mediante un formulario web.
En caso de querer ver un video con el paso a paso pueden ingresar en el video.
"https://youtu.be/MBlvkyPybVQ?si=CUpHsTflHQi1sQe5"

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

Server

```
const express = require('express');
const Database = require('better-sqlite3');
const path = require('path');

const app = express();
const PORT = 3000;

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(express.static(path.join(__dirname, 'public')));

const db = new Database('sistema.db');

db.exec(`
  CREATE TABLE IF NOT EXISTS usuarios (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    nombre TEXT NOT NULL,
    email TEXT NOT NULL
  )
`);

app.post('/api/usuarios', (req, res) => {
  const { nombre, email } = req.body;

  if (!nombre || !email) {
    return res.status(400).json({ error: 'El nombre y el email son obligatorios.' });
  }

  const stmt = db.prepare('INSERT INTO usuarios (nombre, email) VALUES (?, ?)');
  const result = stmt.run(nombre, email);

  res.json({ id: result.lastInsertRowid, nombre, email });
});

app.get('/api/usuarios', (req, res) => {
  const stmt = db.prepare('SELECT * FROM usuarios ORDER BY id DESC');
  const usuarios = stmt.all();

  res.json(usuarios);
});


app.listen(PORT, () => {
  console.log(`Servidor activo en http://localhost:${PORT}`);
});
```

index.html
```
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Integración SQL con Node.js y Express</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
      margin: 40px auto;
      padding: 20px;
      background-color: #f4f4f9;
    }
    h1, h2 {
      color: #333;
    }
    form {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-bottom: 30px;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    input, button {
      padding: 10px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    button {
      background-color: #007bff;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background: #fff;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    th, td {
      border: 1px solid #ddd;
      padding: 10px;
      text-align: left;
    }
    th {
      background-color: #007bff;
      color: white;
    }
  </style>
</head>
<body>

  <h1>Registro de Usuarios</h1>

  <form id="formUsuario">
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" name="nombre" required placeholder="Ej: Juan Pérez">

    <label for="email">Correo Electrónico:</label>
    <input type="email" id="email" name="email" required placeholder="Ej: juan@example.com">

    <button type="submit">Guardar Registro</button>
  </form>

  <h2>Usuarios Registrados</h2>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Email</th>
      </tr>
    </thead>
    <tbody id="tablaUsuarios">
      <!-- Los registros se cargan dinámicamente -->
    </tbody>
  </table>

  <script>
    const form = document.getElementById('formUsuario');
    const tabla = document.getElementById('tablaUsuarios');

      async function cargarUsuarios() {
      const res = await fetch('/api/usuarios');
      const usuarios = await res.json();

      tabla.innerHTML = '';
      usuarios.forEach(u => {
        const fila = document.createElement('tr');
        fila.innerHTML = `
          <td>${u.id}</td>
          <td>${u.nombre}</td>
          <td>${u.email}</td>
        `;
        tabla.appendChild(fila);
      });
    }

        form.addEventListener('submit', async (e) => {
      e.preventDefault();

      const nombre = document.getElementById('nombre').value;
      const email = document.getElementById('email').value;

      await fetch('/api/usuarios', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ nombre, email })
      });

      form.reset();
      cargarUsuarios(); // Actualiza la tabla dinámicamente
    });

        cargarUsuarios();
  </script>

</body>
</html>
```
