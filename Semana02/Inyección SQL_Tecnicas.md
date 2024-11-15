# Práctica Inyección SQL

Se presentan 6 técnicas de inyección SQL, con ejemplos y explicaciones sobre cómo funcionan y qué se intenta lograr con cada una.

**Nota**: Estos ejemplos son para fines educativos y de concientización sobre seguridad; jamás deben aplicarse en sistemas sin autorización.



## 1. Inyección Clásica (Directa)

### Descripción:

Este tipo de inyección se usa cuando una consulta SQL acepta directamente datos del usuario sin sanitización. El atacante intenta cerrar la consulta inicial y agregar una instrucción SQL adicional.

### Ejemplo:

Supongamos que tienes una consulta en Python:

```python
def get_user(username):
    query = f"SELECT * FROM users WHERE username = '{username}'"
    # Ejecutar la consulta en la base de datos
```

### Ataque:

El atacante ingresa `admin' OR '1'='1` como nombre de usuario. La consulta resultante sería:

```sql
SELECT * FROM users WHERE username = 'admin' OR '1'='1'
```

### Resultado:

Esto devuelve todos los registros de la tabla `users`, ya que la condición `1='1'` siempre es verdadera.



## 2. Inyección de Comentarios

### Descripción:

El atacante puede utilizar caracteres de comentarios para omitir el resto de la consulta SQL, alterando su lógica.

### Ejemplo:

Una consulta SQL para verificar la contraseña de un usuario podría ser:

```python
def check_login(username, password):
    query = f"SELECT * FROM users WHERE username = '{username}' AND password = '{password}'"
    # Ejecutar la consulta en la base de datos
```

### Ataque:

El atacante ingresa `admin' --` como nombre de usuario. La consulta resultante sería:

```sql
SELECT * FROM users WHERE username = 'admin' -- ' AND password = 'password'
```

### Resultado:

El comentario `--` hace que el resto de la consulta sea ignorado, autenticando al usuario sin verificar la contraseña.



## 3. Inyección Union-Based

### Descripción:

Esta técnica permite al atacante combinar el resultado de varias consultas mediante `UNION`, extrayendo datos de otras tablas de la base de datos.

### Ejemplo:

Una aplicación muestra los datos de un usuario en base a su `user_id`:

```python
def get_user_info(user_id):
    query = f"SELECT name, email FROM users WHERE id = {user_id}"
    # Ejecutar la consulta en la base de datos
```

### Ataque:

El atacante ingresa `1 UNION SELECT credit_card_number, pin FROM credit_cards --`. La consulta resultante sería:

```sql
SELECT name, email FROM users WHERE id = 1 UNION SELECT credit_card_number, pin FROM credit_cards --
```

### Resultado:

Esto muestra los datos combinados de la tabla `users` y `credit_cards`, exponiendo datos sensibles.



## 4. Inyección Blind SQL (Boolean-Based)

### Descripción:

Se usa cuando no se pueden ver directamente los resultados de la consulta. En su lugar, el atacante envía varias consultas booleanas (`true/false`) para deducir información de la base de datos.

### Ejemplo:

Una aplicación toma un parámetro de `user_id` y devuelve `True` si existe el usuario:

```python
def user_exists(user_id):
    query = f"SELECT * FROM users WHERE id = {user_id}"
    # Ejecutar la consulta en la base de datos
```

### Ataque:

El atacante intenta `1 OR 1=1` y `1 AND 1=2` como `user_id`. Si `1 OR 1=1` devuelve `True` y `1 AND 1=2` devuelve `False`, el atacante sabe que la inyección está funcionando y continúa probando más valores.

### Resultado:

Con tiempo suficiente, el atacante puede deducir los datos de la tabla `users`.



## 5. Inyección de Tiempo (Time-Based Blind SQL Injection)

### Descripción:

En sistemas donde el atacante no puede ver los resultados o recibir un mensaje de error, se utiliza una técnica de retraso para deducir si la consulta es verdadera o falsa.

### Ejemplo:

El atacante usa una inyección de retraso para deducir información de la base de datos.

### Ataque:

Envía un parámetro `user_id` con la consulta `1; IF(1=1, SLEEP(5), 0)`. Si la aplicación se demora en responder, sabe que la inyección funciona.

```sql
SELECT * FROM users WHERE id = 1; IF(1=1, SLEEP(5), 0)
```

### Resultado:

El atacante puede usar tiempos de espera para extraer datos de forma indirecta.



## 6. Inyección de Instrucciones Stacked (Combinadas)

### Descripción:

El atacante inyecta múltiples consultas en una sola instrucción (solo en sistemas que permiten múltiples consultas).

### Ejemplo:

Una consulta básica para eliminar un usuario podría ser:

```python
def delete_user(user_id):
    query = f"DELETE FROM users WHERE id = {user_id}"
    # Ejecutar la consulta en la base de datos
```

### Ataque:

El atacante ingresa `1; DROP TABLE users` como `user_id`. La consulta resultante sería:

```sql
DELETE FROM users WHERE id = 1; DROP TABLE users;
```

### Resultado:

Esto ejecuta dos consultas: una para eliminar un usuario y otra para borrar toda la tabla `users`.

------

### Medidas de Protección

Para evitar inyecciones SQL, considera siempre:

- **Usar parámetros preparados**: Utiliza consultas parametrizadas para evitar la manipulación de datos.
- **Validación y sanitización de entradas**: Asegúrate de validar todas las entradas de usuario.
- **Limitaciones de permisos**: No otorgues permisos elevados a la base de datos sin necesidad.
- **Manejo de excepciones**: Evita mostrar mensajes de error detallados a los usuarios.

Estas técnicas de inyección SQL muestran la importancia de proteger las aplicaciones contra este tipo de ataques mediante prácticas seguras de programación y manejo de base de datos.