

# **Proyecto**: 

## Simulación de Cajero Automático con Exposición de Datos Sensibles

#### **Descripción**:

Esta aplicación web simula un cajero automático que permite realizar transacciones bancarias. Muestra cómo la exposición de datos sensibles puede ocurrir cuando la información no se maneja de manera segura.

### 1. Configuración del Entorno

1. **Crea un Directorio para tu Proyecto**:

   ```bash
   mkdir atm-simulation
   cd atm-simulation
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

   Crea un archivo llamado `app.js` en el directorio raíz del proyecto:

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
   
   // Array para almacenar temporalmente las transacciones
   let transactions = [];
   
   // Ruta principal para mostrar el formulario y las transacciones
   app.get('/', (req, res) => {
       res.send(`
           <!DOCTYPE html>
           <html lang="es">
           <head>
               <meta charset="UTF-8">
               <meta name="viewport" content="width=device-width, initial-scale=1.0">
               <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
               <title>Simulación de Cajero Automático</title>
           </head>
           <body class="bg-light">
               <div class="container mt-5">
                   <h1 class="mb-4">Cajero Automático Simulado</h1>
                   <form method="POST" action="/" class="card p-4 mb-4 bg-white">
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
                       <tbody>
                           ${transactions.map(tx => `
                               <tr>
                                   <td>${tx.transactionType}</td>
                                   <td>${tx.amount}</td>
                                   <td>${tx.name}</td>
                                   <td>${tx.accountNumber}</td>
                               </tr>
                           `).join('')}
                       </tbody>
                   </table>
               </div>
               <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
           </body>
           </html>
       `);
   });
   
   // Ruta para manejar el envío del formulario
   app.post('/', (req, res) => {
       const { transactionType, amount, name, accountNumber } = req.body;
   
       // Exponer datos sensibles en el nombre y número de cuenta
       const exposedName = name;
       const exposedAccountNumber = accountNumber;
   
       // Agregar la transacción al array
       transactions.push({ transactionType, amount, name: exposedName, accountNumber: exposedAccountNumber });
   
       // Redirigir a la página principal
       res.redirect('/');
   });
   
   // Iniciar el servidor
   app.listen(port, () => {
       console.log(`Servidor escuchando en http://localhost:${port}`);
   });
   ```

### Explicación del Código

- **Express**: Se utiliza para manejar las solicitudes HTTP.
- **Bootstrap**: Se incluye para estilizar el formulario y la tabla de transacciones.
- **Array `transactions`**: Se utiliza para almacenar temporalmente las transacciones realizadas.
- **`app.get('/')`**: Genera la página principal con un formulario de transacción y una tabla que muestra las transacciones realizadas.
- **`app.post('/')`**: Maneja el envío del formulario, agregando la transacción al array y luego redirigiendo al usuario de vuelta a la página principal.
- **Exposición de Datos Sensibles**: En este ejemplo, los campos `name` y `accountNumber` están expuestos directamente en la tabla de transacciones, sin ningún tipo de enmascaramiento o protección.

### 3. Ejecutar la Aplicación

1. **Inicia el Servidor**:

   ```bash
   node app.js
   ```

2. **Accede a la Aplicación**: Abre tu navegador y ve a `http://localhost:3000`.

### 4. Probar la Exposición de Datos Sensibles

1. **Realiza una Transacción**: Completa el formulario con datos ficticios y procesa la transacción.
2. **Observa la Exposición de Datos**:
   - El nombre y el número de cuenta se mostrarán en texto plano en la tabla de transacciones.
   - Esto simula cómo datos sensibles pueden ser expuestos si no se toman las medidas adecuadas para protegerlos.