

# **Proyecto**: 

## Simulación de XSS Reflejado con Node.js, Express y Bootstrap



Segunda practica, donde utilizaremos Bootstrap para mejorar la apariencia del formulario y de la página web en general. Al igual que antes, la aplicación incluirá una vulnerabilidad XSS reflejada para fines educativos.



### 1. Configuración del Entorno

Los pasos iniciales son los mismos que en el ejemplo anterior:

1. **Crea un Directorio para tu Proyecto**:

   ```bash
   mkdir xss-example-bootstrap
   cd xss-example-bootstrap
   ```

2. **Inicializa un Proyecto Node.js**:

   ```bash
   npm init -y
   ```

3. **Instala Express**:

   ```bash
   npm install express
   ```

### 2. Creación del Servidor con Express y Bootstrap

1. **Crea el Archivo Principal**: 

   En el directorio raíz de tu proyecto, crea un archivo llamado `app.js`:

   ```bash
   touch app.js
   ```

2. **Configura el Servidor Express con Bootstrap**: 

   Abre `app.js` y añade el siguiente código:

   ```javascript
   const express = require('express');
   const bodyParser = require('body-parser');
   const app = express();
   const port = 3000;
   
   app.use(bodyParser.urlencoded({ extended: true }));
   
   let messages = [];
   
   app.get('/', (req, res) => {
       res.send(`
           <!DOCTYPE html>
           <html lang="es">
           <head>
               <meta charset="UTF-8">
               <meta name="viewport" content="width=device-width, initial-scale=1.0">
               <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
               <title>Mensajes con XSS</title>
           </head>
           <body class="bg-light">
               <div class="container mt-5">
                   <h1 class="mb-4">Mensajes</h1>
                   <ul class="list-group mb-4">
                       ${messages.map(msg => `<li class="list-group-item">${msg}</li>`).join('')}
                   </ul>
                   <form method="POST" action="/" class="card p-4 bg-white">
                       <div class="mb-3">
                           <label for="message" class="form-label">Escribe tu mensaje</label>
                           <input type="text" class="form-control" id="message" name="message" placeholder="Escribe tu mensaje" required>
                       </div>
                       <button type="submit" class="btn btn-primary">Enviar</button>
                   </form>
               </div>
               <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
           </body>
           </html>
       `);
   });
   
   app.post('/', (req, res) => {
       const message = req.body.message;
       messages.push(message);
       res.redirect('/');
   });
   
   app.listen(port, () => {
       console.log(`Servidor escuchando en http://localhost:${port}`);
   });
   ```

   ### Explicación:

   - **Bootstrap**: Hemos integrado Bootstrap 5 utilizando su CDN para estilizar la aplicación.
   - **HTML**: El HTML generado en el método `app.get('/')` incluye clases de Bootstrap para estilizar la lista de mensajes y el formulario.
   - **messages**: Sigue siendo un arreglo que almacena temporalmente los mensajes enviados por los usuarios.

### 3. Ejecutar la Aplicación

1. **Inicia el Servidor**:

   ```bash
   node app.js
   ```

2. **Accede a la Aplicación**: 

   Abre tu navegador y ve a `http://localhost:3000`.

### 4. Probar la Vulnerabilidad XSS

1. **Envía un Mensaje Malicioso**: 

   En el formulario de mensajes, envía el siguiente contenido:

   ```html
   <script>alert('XSS en acción!');</script>
   ```

2. **Observa el Comportamiento**: 

   El código JavaScript se ejecutará al cargar la página, mostrando una alerta como parte del ataque XSS.

#### 

