

# **Proyecto**

## Modelo Entidad-Relación (MER) básico de un sistema de cajero automático utilizando MySQL

### Script SQL para Crear el MER de un Cajero Automático

```sql
-- Seleccionar la base de datos
CREATE DATABASE IF NOT EXISTS ATMSystem;
USE ATMSystem;

-- Crear tabla de Clientes
CREATE TABLE IF NOT EXISTS Clientes (
    cliente_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(255),
    telefono VARCHAR(15),
    email VARCHAR(100) UNIQUE
);

-- Crear tabla de Cuentas
CREATE TABLE IF NOT EXISTS Cuentas (
    cuenta_id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    numero_cuenta VARCHAR(20) UNIQUE NOT NULL,
    tipo_cuenta ENUM('Ahorros', 'Corriente') NOT NULL,
    saldo DECIMAL(15, 2) NOT NULL DEFAULT 0.00,
    fecha_apertura DATE NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES Clientes(cliente_id)
);

-- Crear tabla de Tarjetas
CREATE TABLE IF NOT EXISTS Tarjetas (
    tarjeta_id INT AUTO_INCREMENT PRIMARY KEY,
    cuenta_id INT NOT NULL,
    numero_tarjeta VARCHAR(16) UNIQUE NOT NULL,
    tipo_tarjeta ENUM('Debito', 'Credito') NOT NULL,
    fecha_expiracion DATE NOT NULL,
    codigo_seguridad VARCHAR(4) NOT NULL,
    estado ENUM('Activa', 'Bloqueada') NOT NULL DEFAULT 'Activa',
    FOREIGN KEY (cuenta_id) REFERENCES Cuentas(cuenta_id)
);

-- Crear tabla de Transacciones
CREATE TABLE IF NOT EXISTS Transacciones (
    transaccion_id INT AUTO_INCREMENT PRIMARY KEY,
    cuenta_id INT NOT NULL,
    tipo_transaccion ENUM('Deposito', 'Retiro', 'Transferencia') NOT NULL,
    monto DECIMAL(15, 2) NOT NULL,
    fecha_transaccion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (cuenta_id) REFERENCES Cuentas(cuenta_id)
);

-- Crear tabla de Cajeros
CREATE TABLE IF NOT EXISTS Cajeros (
    cajero_id INT AUTO_INCREMENT PRIMARY KEY,
    ubicacion VARCHAR(255) NOT NULL,
    codigo_cajero VARCHAR(10) UNIQUE NOT NULL,
    estado ENUM('Activo', 'Inactivo') NOT NULL DEFAULT 'Activo'
);

-- Crear tabla de Operaciones realizadas en Cajeros
CREATE TABLE IF NOT EXISTS OperacionesCajero (
    operacion_id INT AUTO_INCREMENT PRIMARY KEY,
    cajero_id INT NOT NULL,
    transaccion_id INT NOT NULL,
    fecha_operacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (cajero_id) REFERENCES Cajeros(cajero_id),
    FOREIGN KEY (transaccion_id) REFERENCES Transacciones(transaccion_id)
);

-- Ejemplo de inserción de datos en las tablas
-- Insertar un cliente
INSERT INTO Clientes (nombre, direccion, telefono, email)
VALUES ('Juan Pérez', 'Calle Falsa 123', '555-1234', 'juan.perez@example.com');

-- Insertar una cuenta para el cliente
INSERT INTO Cuentas (cliente_id, numero_cuenta, tipo_cuenta, saldo, fecha_apertura)
VALUES (1, '1234567890', 'Ahorros', 1000.00, CURDATE());

-- Insertar una tarjeta asociada a la cuenta
INSERT INTO Tarjetas (cuenta_id, numero_tarjeta, tipo_tarjeta, fecha_expiracion, codigo_seguridad)
VALUES (1, '1111222233334444', 'Debito', '2025-12-31', '123');

-- Insertar una transacción para la cuenta
INSERT INTO Transacciones (cuenta_id, tipo_transaccion, monto)
VALUES (1, 'Deposito', 500.00);

-- Insertar un cajero
INSERT INTO Cajeros (ubicacion, codigo_cajero)
VALUES ('Sucursal Centro', 'ATM001');

-- Registrar la operación en un cajero
INSERT INTO OperacionesCajero (cajero_id, transaccion_id)
VALUES (1, 1);
```

### Explicación del Script:

1. **Clientes**: Tabla para almacenar la información de los clientes del banco. Cada cliente tiene un `cliente_id` único.
2. **Cuentas**: Tabla para almacenar las cuentas bancarias de los clientes. Cada cuenta está asociada a un cliente y tiene un `cuenta_id` único.
3. **Tarjetas**: Tabla para almacenar la información de las tarjetas asociadas a las cuentas bancarias. Cada tarjeta tiene un `tarjeta_id` único y está asociada a una cuenta.
4. **Transacciones**: Tabla para registrar todas las transacciones que se realizan en las cuentas. Cada transacción tiene un `transaccion_id` único y está vinculada a una cuenta específica.
5. **Cajeros**: Tabla que almacena información sobre los cajeros automáticos. Cada cajero tiene un `cajero_id` único.
6. **OperacionesCajero**: Tabla para registrar las operaciones realizadas en un cajero específico. Está vinculada tanto a un cajero como a una transacción.

### Identificación de Posibles Errores de Codificación:

1. **Falta de Validación de Datos**: El script no incluye validaciones para los datos ingresados, lo que puede permitir la entrada de datos incorrectos o maliciosos. Es importante implementar validaciones de entrada tanto a nivel de base de datos como a nivel de aplicación.
2. **Falta de Seguridad en el Almacenamiento de Información Sensible**: Campos como `numero_tarjeta` y `codigo_seguridad` están almacenados en texto plano. En una aplicación real, estos datos deben ser cifrados para proteger la privacidad y seguridad del cliente.
3. **Falta de Manejo de Errores**: El script asume que todas las operaciones de inserción y creación de tablas se realizan correctamente. En una aplicación real, es fundamental manejar los posibles errores durante las operaciones de la base de datos.

## Retiro de Dinero con Inyección SQL

#### **Descripción**:

Esta aplicación web permite realizar retiros de dinero de cuentas bancarias. También demuestra cómo un atacante puede realizar inyecciones SQL para comprometer la base de datos y cómo se puede detectar un ataque de este tipo.

### 1. Script SQL para Configurar la Base de Datos

Primero, extendamos el script SQL para crear el esquema de la base de datos y agregar algunos datos adicionales.

```sql
CREATE DATABASE IF NOT EXISTS ATMSystem;
USE ATMSystem;

-- Crear tabla de Clientes
CREATE TABLE IF NOT EXISTS Clientes (
    cliente_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    direccion VARCHAR(255),
    telefono VARCHAR(15),
    email VARCHAR(100) UNIQUE
);

-- Crear tabla de Cuentas
CREATE TABLE IF NOT EXISTS Cuentas (
    cuenta_id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT NOT NULL,
    numero_cuenta VARCHAR(20) UNIQUE NOT NULL,
    tipo_cuenta ENUM('Ahorros', 'Corriente') NOT NULL,
    saldo DECIMAL(15, 2) NOT NULL DEFAULT 0.00,
    fecha_apertura DATE NOT NULL,
    pin VARCHAR(10) NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES Clientes(cliente_id)
);

-- Insertar datos de prueba
INSERT INTO Clientes (nombre, direccion, telefono, email)
VALUES ('Juan Pérez', 'Calle Falsa 123', '555-1234', 'juan.perez@example.com'),
       ('Ana Gómez', 'Avenida Siempre Viva 742', '555-5678', 'ana.gomez@example.com'),
       ('Luis Fernández', 'Calle Principal 456', '555-7890', 'luis.fernandez@example.com');

INSERT INTO Cuentas (cliente_id, numero_cuenta, tipo_cuenta, saldo, fecha_apertura, pin)
VALUES (1, '1234567890', 'Ahorros', 5000.00, CURDATE(), '1234'),
       (2, '0987654321', 'Corriente', 3000.00, CURDATE(), '4321'),
       (3, '1122334455', 'Ahorros', 10000.00, CURDATE(), '1111');
```

### 2. Configuración del Proyecto Node.js

1. **Crea el Directorio y Configura el Proyecto**:

   ```bash
   mkdir atm-withdrawal-sql-injection
   cd atm-withdrawal-sql-injection
   npm init -y
   ```

2. **Instala Dependencias**:

   ```bash
   npm install express mysql body-parser
   ```

3. **Crea el Archivo `index.html`**: 

   Crea el archivo `index.html` en un directorio llamado `public`:

   ```bash
   mkdir public
   touch public/index.html
   ```

   Añade el siguiente contenido:

   ```html
   <!DOCTYPE html>
   <html lang="es">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
       <title>Retiro de Dinero</title>
   </head>
   <body class="bg-light">
       <div class="container mt-5">
           <h1 class="mb-4">Retiro de Dinero</h1>
           <form method="POST" action="/withdraw" class="card p-4 mb-4 bg-white">
               <div class="mb-3">
                   <label for="accountNumber" class="form-label">Número de Cuenta</label>
                   <input type="text" class="form-control" id="accountNumber" name="accountNumber" placeholder="Ingrese su número de cuenta" required>
               </div>
               <div class="mb-3">
                   <label for="pin" class="form-label">PIN</label>
                   <input type="password" class="form-control" id="pin" name="pin" placeholder="Ingrese su PIN" required>
               </div>
               <div class="mb-3">
                   <label for="amount" class="form-label">Monto a Retirar</label>
                   <input type="number" class="form-control" id="amount" name="amount" placeholder="Ingrese el monto a retirar" required>
               </div>
               <button type="submit" class="btn btn-primary">Retirar Dinero</button>
           </form>
           <p id="message" class="text-danger"></p>
       </div>
       <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
   </body>
   </html>
   ```

4. **Configura el Servidor Express**: Crea el archivo `app.js`:

   ```bash
   touch app.js
   ```

   Añade el siguiente contenido:

   ```javascript
   const express = require('express');
   const mysql = require('mysql');
   const bodyParser = require('body-parser');
   const app = express();
   const port = 3000;
   
   app.use(bodyParser.urlencoded({ extended: true }));
   app.use(express.static('public'));
   
   // Configuración de la conexión a la base de datos MySQL
   const db = mysql.createConnection({
       host: 'localhost',
       user: 'root',  // Cambiar según tu configuración
       password: '',  // Cambiar según tu configuración
       database: 'ATMSystem'
   });
   
   db.connect((err) => {
       if (err) throw err;
       console.log('Conectado a la base de datos MySQL');
   });
   
   // Ruta para manejar el retiro de dinero
   app.post('/withdraw', (req, res) => {
       const { accountNumber, pin, amount } = req.body;
   
       // Primer ataque de inyección SQL: Usar directamente los valores ingresados sin protección
       const sql = `SELECT saldo FROM Cuentas WHERE numero_cuenta = '${accountNumber}' AND pin = '${pin}'`;
   
       db.query(sql, (err, results) => {
           if (err) throw err;
   
           // Verifica si se encontró una cuenta con ese número y PIN
           if (results.length > 0) {
               let saldo = results[0].saldo;
   
               if (saldo >= amount) {
                   // Actualizar el saldo después del retiro
                   const newSaldo = saldo - amount;
                   const updateSql = `UPDATE Cuentas SET saldo = ${newSaldo} WHERE numero_cuenta = '${accountNumber}'`;
   
                   db.query(updateSql, (err, results) => {
                       if (err) throw err;
                       res.send(`<h1>Retiro exitoso</h1><p>Has retirado ${amount}. Tu nuevo saldo es ${newSaldo}.</p>`);
                   });
               } else {
                   res.send(`<h1>Saldo insuficiente</h1><p>No tienes suficiente saldo para completar esta transacción.</p>`);
               }
           } else {
               res.send(`<h1>Datos incorrectos</h1><p>El número de cuenta o PIN es incorrecto.</p>`);
           }
       });
   
       // Detectar un posible ataque basado en patrones comunes de inyección SQL
       if (accountNumber.includes("'") || pin.includes("'")) {
           console.log("¡Alerta de seguridad! Posible ataque de inyección SQL detectado.");
       }
   });
   
   // Ruta para manejar la inyección SQL usando UNION (segunda forma de ataque)
   app.post('/withdraw-union', (req, res) => {
       const { accountNumber, pin, amount } = req.body;
   
       // Inyección SQL usando UNION para obtener más datos de la tabla
       const sql = `SELECT saldo FROM Cuentas WHERE numero_cuenta = '${accountNumber}' AND pin = '${pin}' UNION SELECT null AS saldo, nombre AS accountNumber, direccion AS pin FROM Clientes;`;
   
       db.query(sql, (err, results) => {
           if (err) throw err;
   
           // Verifica si se encontró una cuenta con ese número y PIN
           if (results.length > 0) {
               let saldo = results[0].saldo;
   
               if (saldo >= amount) {
                   // Actualizar el saldo después del retiro
                   const newSaldo = saldo - amount;
                   const updateSql = `UPDATE Cuentas SET saldo = ${newSaldo} WHERE numero_cuenta = '${accountNumber}'`;
   
                   db.query(updateSql, (err, results) => {
                       if (err) throw err;
                       res.send(`<h1>Retiro exitoso</h1><p>Has retirado ${amount}. Tu nuevo saldo es ${newSaldo}.</p>`);
                   });
               } else {
                   res.send(`<h1>Saldo insuficiente</h1><p>No tienes suficiente saldo para completar esta transacción.</p>`);
               }
           } else {
               res.send(`<h1>Datos incorrectos</h1><p>El número de cuenta o PIN es incorrecto.</p>`);
           }
       });
   
       // Detectar un posible ataque basado en patrones comunes de inyección SQL
       if (accountNumber.includes("'") || pin.includes("'") || accountNumber.toUpperCase().includes("UNION")) {
           console.log("¡Alerta de seguridad! Posible ataque de inyección SQL con UNION detectado.");
       }
   });
   
   // Iniciar el servidor
   app.listen(port, () => {
       console.log(`Servidor escuchando en http://localhost:${port}`);
   });
   ```

### 3. Explicación del Código

1. **Interfaz de Usuario**:
   - La página `index.html` proporciona un formulario para que los usuarios ingresen su número de cuenta, PIN y monto a retirar.
2. **Primer Ataque de Inyección SQL (Básico)**:
   - El primer ataque se basa en la construcción de consultas SQL directamente desde los valores proporcionados por el usuario sin usar parámetros preparados. Esto permite la inyección de SQL.
   - Ejemplo de ataque:
     - `accountNumber: ' OR '1'='1';--`
     - `pin: cualquier cosa`
   - Esto hará que la consulta devuelva la primera cuenta encontrada en la base de datos, independientemente de los valores ingresados.
3. **Segundo Ataque de Inyección SQL (UNION)**:
   - Este ataque utiliza la cláusula `UNION` para intentar acceder a otras tablas en la base de datos.
   - Ejemplo de ataque:
     - `accountNumber: 1234567890' UNION SELECT null, nombre, direccion FROM Clientes;--`
     - `pin: cualquier cosa`
   - Esto podría devolver resultados combinados de la tabla `Clientes`, lo que es un problema grave de seguridad.
4. **Detección de Ataques**:
   - Si se detecta la presencia de `'` o `UNION` en los parámetros, se imprime un mensaje en la consola advirtiendo de un posible ataque de inyección SQL.

### 4. Ejecución del Proyecto

1. **Inicia el Servidor**:

   ```bash
   node app.js
   ```

2. **Accede a la Aplicación**: Abre tu navegador y ve a `http://localhost:3000`.

#### **Errores de Codificación**:

1. **Falta de Parámetros Preparados**:
   - **Problema**: No se utilizan consultas preparadas con parámetros, lo que permite que los datos ingresados por el usuario se inserten directamente en las consultas SQL, permitiendo inyecciones SQL.
   - **Recomendación**: Usar consultas preparadas (`Prepared Statements`) para evitar que los datos de entrada modifiquen la lógica de la consulta SQL.
2. **Falta de Sanitización de Entrada**:
   - **Problema**: No hay sanitización o validación de los datos ingresados antes de ser usados en las consultas SQL.
   - **Recomendación**: Validar y sanitizar todas las entradas del usuario para asegurarse de que no contengan caracteres maliciosos.

#### **Simulación de Ataques**:

- Inyección SQL Básica:
  - `Número de Cuenta`: `' OR '1'='1';--`
  - `PIN`: cualquier cosa
  - **Resultado**: Acceso a cualquier cuenta en la base de datos.
- Inyección SQL con UNION:
  - `Número de Cuenta`: `1234567890' UNION SELECT null, nombre, direccion FROM Clientes;--`
  - `PIN`: cualquier cosa
  - **Resultado**: Acceso a información combinada de la tabla `Clientes`.









