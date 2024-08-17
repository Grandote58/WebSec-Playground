

# **Proyecto**: 

## Simulación de XSS Reflejado con Node.js y Express



Crear una aplicación web simple con Node.js y Express que simule una vulnerabilidad de tipo Cross-Site Scripting (XSS) reflejado. Recuerda que esto es solo para propósitos educativos y de entrenamiento. Nunca deberías implementar vulnerabilidades intencionalmente en aplicaciones reales.



### 1. Configuración del Entorno

1. **Instala Node.js**: Asegúrate de tener Node.js instalado en tu máquina. Puedes verificarlo ejecutando `node -v` en la terminal.

2. **Crea un Directorio para tu Proyecto**:

   ```bash
   mkdir xss-example
   cd xss-example
   ```

3. **Inicializa un Proyecto Node.js**:

   ```bash
   npm init -y
   ```

   Esto creará un archivo `package.json` básico.

4. **Instala Express**: Express es un framework web para Node.js que nos permitirá construir la aplicación de manera rápida.

   ```bash
   npm install express
   ```

### 2. Creación del Servidor Básico

1. **Crea el Archivo Principal**: En el directorio raíz de tu proyecto, crea un archivo llamado `app.js` o `index.js`:

   ```bash
   touch app.js
   ```

2. **Configura el Servidor Express**: 

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
           <h1>Mensajes</h1>
           <ul>
               ${messages.map(msg => `<li>${msg}</li>`).join('')}
           </ul>
           <form method="POST" action="/">
               <input type="text" name="message" placeholder="Escribe tu mensaje" required>
               <button type="submit">Enviar</button>
           </form>
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

   - **Express** se utiliza para manejar las solicitudes HTTP.
   - **bodyParser** permite parsear los datos del formulario que se envían a través de `POST`.
   - **messages** es un arreglo que almacena temporalmente los mensajes enviados por los usuarios.
   - **app.get('/')** maneja las solicitudes `GET` a la raíz (`/`) de la aplicación, generando una página HTML simple que muestra los mensajes y un formulario para enviar nuevos mensajes.
   - **app.post('/')** maneja las solicitudes `POST`, donde el mensaje enviado se agrega al arreglo `messages` y luego se redirige a la página principal para mostrar todos los mensajes.

### 3. Simulando la Vulnerabilidad XSS

El código anterior ya contiene una vulnerabilidad de tipo XSS reflejado. Si un usuario envía un mensaje que incluye código JavaScript, este será renderizado en la página sin ninguna validación o sanitización, lo que permite la ejecución de scripts maliciosos.

### 4. Ejecutar la Aplicación

1. **Inicia el Servidor**:

   ```bash
   node app.js
   ```

2. **Accede a la Aplicación**: Abre tu navegador y ve a `http://localhost:3000`.

3. **Probar la Vulnerabilidad XSS**:

   - En el formulario de mensajes, envía un mensaje con el siguiente contenido:

     ```html
     <script>alert('XSS!');</script>
     ```

   - Cuando el mensaje se envíe, el código JavaScript se ejecutará, mostrando un `alert` en la página.

### 5. Consideraciones de Seguridad

En una aplicación real, nunca deberías permitir que los usuarios envíen contenido sin sanitizar. Existen varias maneras de protegerse contra XSS:

- **Escapado del Contenido**: Asegúrate de escapar correctamente los caracteres especiales antes de renderizar el contenido en HTML.
- **Bibliotecas de Seguridad**: Usa bibliotecas como [DOMPurify](https://github.com/cure53/DOMPurify) para limpiar el contenido HTML.





