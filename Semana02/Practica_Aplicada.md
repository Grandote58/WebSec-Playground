# Practica Aplicada



Realizar pruebas en la aplicación de inventarios y verificar la seguridad de los campos de entrada, puedes intentar diferentes técnicas de inyección SQL en cada uno de los campos de la aplicación.



### Objetivos de la Práctica

1. **Probar la validación y sanitización de los campos de entrada** en la aplicación de inventarios.
2. **Aplicar diferentes técnicas de inyección SQL** en cada campo de entrada.
3. **Detectar posibles vulnerabilidades** y realizar ajustes en el código para mejorar la seguridad.

------

### Prueba en Campos de la Aplicación de Inventario

Asumimos que la aplicación tiene los siguientes campos de entrada:

- `Nombre`: Nombre del producto
- `Descripción`: Descripción del producto
- `Precio`: Precio del producto
- `Categoría`: Categoría a la que pertenece el producto (utiliza un desplegable en el front-end, pero vamos a suponer que podríamos manipular este valor manualmente).

Para cada campo, se probarán las siguientes técnicas de inyección.

------

### 1. Campo `Nombre` del Producto

#### Ejemplo de código en la aplicación:

```sql
SELECT * FROM productos WHERE nombre = '{nombre}'
```

#### Pruebas:

1. **Inyección simple**:
   - Entrada: `Laptop' OR '1'='1`
   - Resultado esperado: Si la aplicación es vulnerable, devolverá todos los productos en lugar de solo uno.
2. **Inyección con comentario**:
   - Entrada: `Tablet' --`
   - Resultado esperado: Esto debería omitir el resto de la consulta SQL, lo que podría resultar en acceso no autorizado a datos.
3. **Inyección Union-Based**:
   - Entrada: `Mouse' UNION SELECT usuario, password FROM usuarios --`
   - Resultado esperado: Si es vulnerable, esta inyección podría exponer datos de otras tablas.
4. **Inyección de tiempo (Time-Based Blind SQL Injection)**:
   - Entrada: `Keyboard'; WAITFOR DELAY '00:00:05' --`
   - Resultado esperado: Si la aplicación se demora en responder, entonces es vulnerable a inyección de tiempo.



### 2. Campo `Descripción` del Producto

#### Ejemplo de código en la aplicación:

```sql
SELECT * FROM productos WHERE descripcion LIKE '%{descripcion}%'
```

#### Pruebas:

1. **Inyección para salida de datos adicionales**:
   - Entrada: `USB Cable%' UNION SELECT credit_card_number FROM pagos --`
   - Resultado esperado: Si es vulnerable, la consulta podría exponer información confidencial.
2. **Inyección booleana (Blind SQL Injection)**:
   - Entrada: `Headphones' AND 1=1 --`
   - Prueba de entrada alternativa: `Headphones' AND 1=2 --`
   - Resultado esperado: Si el comportamiento varía entre las dos entradas, entonces es vulnerable a una inyección booleana.
3. **Inyección de instrucciones múltiples (Stacked Queries)**:
   - Entrada: `Charger'; DROP TABLE productos --`
   - Resultado esperado: Si es vulnerable, esta inyección puede llevar a la eliminación completa de la tabla `productos`.



### 3. Campo `Precio` del Producto

El campo `Precio` normalmente se espera como un valor numérico, pero intentaremos inyecciones SQL para observar cómo la aplicación maneja entradas inesperadas.

#### Ejemplo de código en la aplicación:

```sql
SELECT * FROM productos WHERE precio = {precio}
```

#### Pruebas:

1. **Inyección numérica**:
   - Entrada: `100 OR 1=1`
   - Resultado esperado: Esto podría devolver todos los productos si no hay validación de tipo numérico.
2. **Inyección combinada con comentario**:
   - Entrada: `50; --`
   - Resultado esperado: Esto podría cancelar el resto de la consulta y devolver todos los productos con precio igual a 50, dependiendo de la estructura de la aplicación.
3. **Inyección de tiempo (Time-Based Blind SQL Injection)**:
   - Entrada: `10 OR 1=1; WAITFOR DELAY '00:00:05' --`
   - Resultado esperado: Un retraso en la respuesta indicaría vulnerabilidad a inyecciones basadas en tiempo.



### 4. Campo `Categoría` del Producto

El campo `Categoría` utiliza un valor numérico, pero al igual que el campo `Precio`, podría manipularse en un entorno de pruebas para intentar inyecciones SQL.

#### Ejemplo de código en la aplicación:

```sql
SELECT * FROM productos WHERE categoria_id = {categoria_id}
```

#### Pruebas:

1. **Inyección numérica**:
   - Entrada: `1 OR 1=1`
   - Resultado esperado: Devuelve todos los productos, sin importar la categoría, si la consulta no está protegida.
2. **Inyección de comentarios**:
   - Entrada: `2; --`
   - Resultado esperado: Omite el resto de la consulta y solo filtra la categoría `2`.
3. **Inyección para obtener datos adicionales**:
   - Entrada: `3 UNION SELECT usuario, password FROM usuarios --`
   - Resultado esperado: Muestra la información de la tabla `usuarios`, si es vulnerable.



### 5. Prueba de Validación de Longitud y Caracteres Especiales en Todos los Campos

Algunas aplicaciones permiten solo ciertos caracteres en los campos de entrada y limitan la longitud de los mismos. Pruebas adicionales incluyen:

1. **Exceso de longitud**:
   - Intenta ingresar 1,000 caracteres en el campo `Nombre`.
   - Resultado esperado: La aplicación debería rechazar la entrada o truncarla, en lugar de intentar ejecutarla directamente en SQL.
2. **Caracteres especiales**:
   - Entrada: `<>; " ' --`
   - Resultado esperado: Estos caracteres deben estar sanitizados para evitar que puedan causar comportamientos inesperados en la base de datos.



### Medidas para Mitigar Vulnerabilidades en SQL Injection

Después de realizar estas pruebas, puedes aplicar las siguientes medidas para mitigar las vulnerabilidades:

1. **Uso de consultas parametrizadas (parámetros preparados)**: Esto asegura que los datos del usuario no se inserten directamente en la consulta SQL, previniendo así la manipulación.
2. **Validación y sanitización de entradas**: Implementa reglas estrictas sobre el tipo de datos esperado, la longitud de los campos y el contenido permitido.
3. **Limitación de permisos de la base de datos**: Asegúrate de que la cuenta de la base de datos utilizada por la aplicación tenga permisos mínimos.
4. **Registro y monitoreo**: Monitorea y registra cualquier intento de manipulación en las entradas, con el objetivo de detectar posibles ataques.