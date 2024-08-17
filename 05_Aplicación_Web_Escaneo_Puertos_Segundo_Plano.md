

# **Proyecto**:

## Aplicación Web con Escaneo de Puertos en Segundo Plano

#### **Descripción**:

Esta aplicación web permite a los usuarios enviar datos a través de un formulario. Al mismo tiempo, el servidor realiza un escaneo de puertos en un host específico, mostrando los resultados en la consola.

**Quiero resaltar que este ejemplo es puramente educativo y con fines de concienciación sobre seguridad. El escaneo de puertos en sistemas sin autorización es ilegal y antiético. No utilices este código en sistemas que no sean de tu propiedad o sin permiso explícito**.

### 1. Configuración del Entorno

1. **Crea un Directorio para tu Proyecto**:

   ```bash
   mkdir port-scanner-webapp
   cd port-scanner-webapp
   ```

2. **Inicializa un Proyecto Node.js**:

   ```bash
   npm init -y
   ```

3. **Instala Express**:

   ```bash
   npm install express
   ```

4. **Instala una Librería de Escaneo de Puertos**: 

   Vamos a utilizar una librería como `net` (que es parte de Node.js) para hacer el escaneo de puertos. No necesitamos instalar ninguna dependencia adicional, ya que `net` es un módulo nativo de Node.js.

### 2. Creación de la Página HTML y del Servidor Express

1. **Crea un Archivo HTML**: 

   Crea un archivo llamado `index.html` en un directorio llamado `public`:

   ```bash
   mkdir public
   touch public/index.html
   ```

2. **Escribe el Contenido del Archivo `index.html`**: 

   Abre `index.html` y añade el siguiente código:

   ```html
   <!DOCTYPE html>
   <html lang="es">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
       <title>Envío de Datos</title>
   </head>
   <body class="bg-light">
       <div class="container mt-5">
           <h1 class="mb-4">Formulario de Envío de Datos</h1>
           <form method="POST" action="/send-data" class="card p-4 mb-4 bg-white">
               <div class="mb-3">
                   <label for="data" class="form-label">Datos a Enviar</label>
                   <input type="text" class="form-control" id="data" name="data" placeholder="Ingrese los datos" required>
               </div>
               <button type="submit" class="btn btn-primary">Enviar Datos</button>
           </form>
       </div>
       <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
   </body>
   </html>
   ```

3. **Configura el Servidor Express con el Escaneo de Puertos**: 

   Abre `app.js` y añade el siguiente código:

   ```javascript
   const express = require('express');
   const net = require('net');
   const bodyParser = require('body-parser');
   const app = express();
   const port = 3000;
   
   app.use(bodyParser.urlencoded({ extended: true }));
   app.use(express.static('public'));
   
   // Función para escanear puertos
   function scanPort(host, port, callback) {
       const socket = new net.Socket();
       let status = 'closed'; // Por defecto, asumimos que el puerto está cerrado
   
       socket.setTimeout(2000); // Timeout de 2 segundos
   
       socket.on('connect', () => {
           status = 'open';
           socket.destroy();
       });
   
       socket.on('timeout', () => {
           status = 'closed';
           socket.destroy();
       });
   
       socket.on('error', (err) => {
           status = 'closed';
           socket.destroy();
       });
   
       socket.on('close', () => {
           callback(port, status);
       });
   
       socket.connect(port, host);
   }
   
   // Ruta para manejar el envío de datos
   app.post('/send-data', (req, res) => {
       const data = req.body.data;
       console.log(`Datos recibidos: ${data}`);
   
       // Escanear puertos en el host objetivo (por ejemplo, 'localhost')
       const host = 'localhost';
       const ports = [22, 80, 443, 8080];
       console.log(`Iniciando escaneo de puertos en ${host}...`);
   
       ports.forEach((port) => {
           scanPort(host, port, (port, status) => {
               console.log(`Puerto ${port} está ${status}`);
           });
       });
   
       // Redirigir a una página de confirmación
       res.redirect('/confirmation');
   });
   
   // Ruta de confirmación
   app.get('/confirmation', (req, res) => {
       res.send(`
           <h1>Datos enviados con éxito</h1>
           <p>El escaneo de puertos ha sido iniciado en segundo plano.</p>
           <a href="/" class="btn btn-primary">Volver</a>
       `);
   });
   
   // Iniciar el servidor
   app.listen(port, () => {
       console.log(`Servidor escuchando en http://localhost:${port}`);
   });
   ```

### Explicación del Código

- **Formulario de Envío de Datos**: El archivo `index.html` contiene un formulario que envía los datos introducidos a la ruta `/send-data` utilizando el método `POST`.
- **Escaneo de Puertos**: En el archivo `app.js`, se define la función `scanPort` que utiliza el módulo `net` para intentar conectarse a diferentes puertos en un host específico (en este caso, `localhost`). Si un puerto está abierto, se registra en la consola.
- **Proceso en Segundo Plano**: El escaneo de puertos se ejecuta en segundo plano después de que se envían los datos, y los resultados del escaneo se muestran en la consola.
- **Redirección**: Después de enviar los datos y comenzar el escaneo, el usuario es redirigido a una página de confirmación (`/confirmation`).

### 3. Ejecutar la Aplicación

1. **Inicia el Servidor**:

   ```bash
   node app.js
   ```

2. **Accede a la Aplicación**: Abre tu navegador y ve a `http://localhost:3000`.

### 4. Probar la Aplicación

1. **Envía Datos**: Completa el formulario con datos ficticios y envíalos.
2. **Observa el Escaneo de Puertos**: Verás en la consola del servidor los resultados del escaneo de puertos en `localhost`.

#### **Errores de Codificación**:

1. **Escaneo de Puertos Inseguro**:
   - **Problema**: Realizar un escaneo de puertos desde una aplicación web es altamente inseguro y podría ser utilizado de manera maliciosa para identificar servicios vulnerables en una red. Además, podría resultar en la exposición de información sensible sobre la infraestructura de la red.
   - **Recomendación**: Este tipo de operaciones no debería integrarse en una aplicación web accesible al público. El escaneo de puertos y otras actividades de prueba de penetración deben realizarse en entornos controlados, con el debido permiso y utilizando herramientas especializadas.
2. **Redirección Después de Enviar Datos**:
   - **Problema**: El hecho de que se realice un escaneo de puertos después de un envío de formulario sin el conocimiento del usuario puede ser visto como un comportamiento malicioso y poco ético.
   - **Recomendación**: Las aplicaciones deben ser transparentes sobre sus operaciones. Si se realizan tareas en segundo plano, los usuarios deben ser informados adecuadamente, y tales tareas deben estar justificadas y aprobadas.
3. **Uso del Módulo `net` para Escaneo de Puertos**:
   - **Problema**: El módulo `net` de Node.js no está diseñado específicamente para escaneos de puertos, y su uso de esta manera es una mala práctica, dado que podría causar falsos positivos o sobrecargar el sistema.
   - **Recomendación**: Si es absolutamente necesario realizar un escaneo de puertos en un entorno controlado, se deben utilizar herramientas especializadas como `nmap` en lugar de una solución ad-hoc en Node.js.