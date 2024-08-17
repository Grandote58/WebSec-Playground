

# **Proyecto**: 

## Simulación de Cajero Automático con Exposición de Datos Sensibles en la URL

#### **Descripción**:

Esta aplicación web simula un cajero automático que permite realizar transacciones bancarias. Demuestra cómo la exposición de datos sensibles puede ocurrir cuando se utilizan métodos incorrectos para enviar datos, como el método `GET` en lugar de `POST` para manejar datos sensibles.

### 1. Configuración del Entorno

1. **Crea un Directorio para tu Proyecto**:

   ```bash
   mkdir atm-simulation-get
   cd atm-simulation-get
   ```

2. **Inicializa un Proyecto Node.js**:

   ```bash
   npm init -y
   ```

3. **Instala Express**:

   ```bash
   npm install express
   ```

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
   !DOCTYPE html>
   <html lang="es">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
       <title>Simulación de Cajero Automático</title>
   </head>
   <body class="bg-light">
       <div class="container mt-5">
           <h1 class="mb-4">Cajero Automático Simulado</h1>
           <form method="GET" action="/" class="card p-4 mb-4 bg-white">
               <div class="mb-3">
                   <label for="transactionType" class="form-label">Tipo de Transacción</label>
                   <select id="transactionType" name="transactionType" class="form-select" required>
                       <option value="Retiro">Retiro</option>
                       <option value="Depósito">Depósito</option>
                   </select>
               </div>
               <div class="mb-3">
                   <label for="amount" class="form-label">Monto</label>
                   <input type="number" class="form-control" id="amount" name="amount" placeholder="Monto de la transacción" required>
               </div>
               <div class="mb-3">
                   <label for="name" class="form-label">Nombre</label>
                   <input type="text" class="form-control" id="name" name="name" placeholder="Nombre del titular" required>
               </div>
               <div class="mb-3">
                   <label for="accountNumber" class="form-label">Número de Cuenta</label>
                   <input type="text" class="form-control" id="accountNumber" name="accountNumber" placeholder="Número de cuenta" required>
               </div>
               <button type="submit" class="btn btn-primary">Procesar Transacción</button>
           </form>
           <h2>Transacciones Realizadas</h2>
           <table class="table table-bordered">
               <thead>
                   <tr>
                       <th>Tipo de Transacción</th>
                       <th>Monto</th>
                       <th>Nombre del Titular</th>
                       <th>Número de Cuenta</th>
                   </tr>
               </thead>
               <tbody id="transactionTable">
                   <!-- Las transacciones se mostrarán aquí -->
               </tbody>
           </table>
       </div>
       <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
   </body>
   </html>
   ```

3. **Configura el Servidor Express**: 

   Abre `app.js` y añade el siguiente código:

   ```javascript
   const express = require('express');
   const app = express();
   const port = 3000;
   
   // Array para almacenar temporalmente las transacciones
   let transactions = [];
   
   // Servir archivos estáticos desde el directorio 'public'
   app.use(express.static('public'));
   
   // Ruta para manejar el formulario (usando GET, lo cual es una mala práctica)
   app.get('/', (req, res) => {
       if (req.query.transactionType && req.query.amount && req.query.name && req.query.accountNumber) {
           const { transactionType, amount, name, accountNumber } = req.query;
   
           // Exponer datos sensibles en la URL
           const exposedName = name;
           const exposedAccountNumber = accountNumber;
   
           // Agregar la transacción al array
           transactions.push({ transactionType, amount, name: exposedName, accountNumber: exposedAccountNumber });
       }
   
       // Generar la tabla de transacciones en HTML
       let transactionTable = transactions.map(tx => `
           <tr>
               <td>${tx.transactionType}</td>
               <td>${tx.amount}</td>
               <td>${tx.name}</td>
               <td>${tx.accountNumber}</td>
           </tr>
       `).join('');
   
       // Insertar la tabla en la página HTML
       res.sendFile(__dirname + '/public/index.html', {}, function (err) {
           if (err) {
               res.status(500).send(err);
           }
       });
   });
   
   // Iniciar el servidor
   app.listen(port, () => {
       console.log(`Servidor escuchando en http://localhost:${port}`);
   });
   ```

### Explicación del Código

- **HTML Estático**: El archivo `index.html` contiene el formulario y la tabla donde se muestran las transacciones. Se utiliza Bootstrap para mejorar la apariencia.
- **`GET` Request**: En este ejemplo, el formulario envía los datos utilizando el método `GET`. Esto es una mala práctica cuando se trata de datos sensibles, ya que estos datos se adjuntan a la URL, exponiéndolos en la barra de direcciones del navegador y en los registros del servidor.
- **Exposición de Datos Sensibles**: Al utilizar `GET`, los datos sensibles como `name` y `accountNumber` pueden verse directamente en la URL después de que se envía el formulario.

### 3. Ejecutar la Aplicación

1. **Inicia el Servidor**:

   ```bash
   node app.js
   ```

2. **Accede a la Aplicación**: 

   Abre tu navegador y ve a `http://localhost:3000`.

### 4. Probar la Exposición de Datos Sensibles

1. **Realiza una Transacción**: Completa el formulario con datos ficticios y observa la URL después de hacer clic en "Procesar Transacción".
2. **Observa la Exposición de Datos**:
   - Verás que los datos sensibles (`name` y `accountNumber`) están presentes en la barra de direcciones del navegador. Esto expone innecesariamente información confidencial.
   - Además, los datos se mostrarán en la tabla en la página.

#### **Error de Codificación**:

- **Uso de `GET` para Enviar Datos Sensibles**: El formulario utiliza el método `GET` en lugar de `POST` para enviar datos sensibles como el nombre del titular y el número de cuenta. Esto expone los datos en la URL, lo cual es una grave vulnerabilidad de seguridad.
- **Recomendación**: En aplicaciones reales, siempre usa el método `POST` para enviar datos sensibles, y considera enmascarar o cifrar la información cuando sea posible.