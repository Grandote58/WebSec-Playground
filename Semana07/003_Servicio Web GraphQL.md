![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 📘 **Creación de un Servicio Web GraphQL**


**Objetivo:** Aprender a crear un servicio web con GraphQL desde cero utilizando **Python con Flask y Graphene**.
**Enfoque:** Construcción sencilla y comprensible para aprender su estructura y funcionamiento.

## **🔹 1. ¿Qué es GraphQL?**

GraphQL es un lenguaje de consulta para APIs desarrollado por Facebook en 2015. A diferencia de REST, **GraphQL permite a los clientes especificar exactamente qué datos necesitan**, optimizando la comunicación y reduciendo el tráfico innecesario.

📌 **Características clave de GraphQL:**

✅ **Consulta flexible** → El cliente solicita solo los datos necesarios.

✅ **Una sola URL** → No hay múltiples endpoints como en REST.

✅ **Optimizado para rendimiento** → Evita respuestas con datos innecesarios.

✅ **Tipado fuerte** → Usa un esquema para definir la estructura de los datos.

## **🔹 2. Instalación del Entorno**

Para desarrollar un servicio GraphQL en Python, usaremos **Flask** como servidor y **Graphene** como framework GraphQL.

### **🖥️ 2.1 Instalar Python y Pip**

Si no tienes Python instalado, descárgalo desde [python.org](https://www.python.org/downloads/)
Para verificar la instalación:

```bash
python --version
pip --version
```

### **🛠 2.2 Instalar Flask y Graphene**

Ejecuta en la terminal:

```python
pip install flask graphene flask-graphql
```

📌 **Flask** → Framework ligero para servidores web en Python.

📌 **Graphene** → Implementación de GraphQL para Python.

📌 **Flask-GraphQL** → Permite integrar GraphQL en Flask.

## **🔹 3. Creando el Servicio Web GraphQL**

Vamos a crear un **servicio de gestión de usuarios con operaciones CRUD (Crear, Leer, Actualizar, Eliminar).**

### **📂 3.1 Estructura del Proyecto**

Crea una carpeta para el proyecto y dentro un archivo `app.py`:

```python
📂 servicio-graphql
 ┣ 📜 app.py  # Código del servicio
```

------

### **📌 3.2 Código del Servicio GraphQL (app.py)**

Abre `app.py` y copia este código:

```python
from flask import Flask, request, jsonify
from flask_graphql import GraphQLView
import graphene

# Definir el esquema GraphQL
class Usuario(graphene.ObjectType):
    id = graphene.Int()
    nombre = graphene.String()
    email = graphene.String()

# Base de datos simulada (lista en memoria)
usuarios = [
    {"id": 1, "nombre": "Juan", "email": "juan@example.com"},
    {"id": 2, "nombre": "Maria", "email": "maria@example.com"}
]

# Definir Query (lectura de datos)
class Query(graphene.ObjectType):
    usuarios = graphene.List(Usuario)

    def resolve_usuarios(self, info):
        return usuarios

# Definir Mutaciones (crear, actualizar, eliminar usuarios)
class AgregarUsuario(graphene.Mutation):
    class Arguments:
        id = graphene.Int()
        nombre = graphene.String()
        email = graphene.String()

    usuario = graphene.Field(Usuario)

    def mutate(self, info, id, nombre, email):
        nuevo_usuario = {"id": id, "nombre": nombre, "email": email}
        usuarios.append(nuevo_usuario)
        return AgregarUsuario(usuario=nuevo_usuario)

class EliminarUsuario(graphene.Mutation):
    class Arguments:
        id = graphene.Int()

    mensaje = graphene.String()

    def mutate(self, info, id):
        global usuarios
        usuarios = [u for u in usuarios if u["id"] != id]
        return EliminarUsuario(mensaje="Usuario eliminado correctamente")

# Definir Mutaciones en GraphQL
class Mutation(graphene.ObjectType):
    agregar_usuario = AgregarUsuario.Field()
    eliminar_usuario = EliminarUsuario.Field()

# Crear esquema GraphQL
schema = graphene.Schema(query=Query, mutation=Mutation)

# Iniciar Flask
app = Flask(__name__)

# Ruta para GraphQL
app.add_url_rule(
    "/graphql",
    view_func=GraphQLView.as_view(
        "graphql",
        schema=schema,
        graphiql=True  # Habilita GraphiQL para pruebas en el navegador
    ),
)

# Ejecutar el servidor
if __name__ == "__main__":
    app.run(debug=True, port=5000)
```

📌 **Explicación del código:**

✔ `Usuario` → Define un objeto GraphQL con `id`, `nombre` y `email`.

✔ `Query` → Define una consulta para obtener la lista de usuarios.

✔ `Mutation` → Define acciones para **agregar** y **eliminar** usuarios.

✔ `GraphQLView` → Habilita una interfaz gráfica para probar consultas en `/graphql`.

## **🔹 4. Ejecutar el Servidor GraphQL**

Para iniciar el servicio web, abre la terminal en la carpeta del proyecto y ejecuta:

```python
python app.py
```

Verás un mensaje similar a:

```python
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

------

## **🔹 5. Probar el Servicio GraphQL**

### **📌 5.1 Acceder a GraphiQL**

Abre tu navegador y visita **`http://localhost:5000/graphql`**
Aquí puedes escribir y ejecutar consultas GraphQL.

### **📌 5.2 Consultar Todos los Usuarios (Query)**

📢 **Ejecuta esta consulta en GraphiQL o con cURL:**

```json
{
  usuarios {
    id
    nombre
    email
  }
}
```

📌 **Respuesta esperada (JSON):**

```json
{
  "data": {
    "usuarios": [
      {"id": 1, "nombre": "Juan", "email": "juan@example.com"},
      {"id": 2, "nombre": "Maria", "email": "maria@example.com"}
    ]
  }
}
```

------

### **📌 5.3 Agregar un Usuario (Mutation)**

```json
mutation {
  agregarUsuario(id: 3, nombre: "Carlos", email: "carlos@example.com") {
    usuario {
      id
      nombre
      email
    }
  }
}
```

📌 **Respuesta esperada:**

```json
{
  "data": {
    "agregarUsuario": {
      "usuario": {
        "id": 3,
        "nombre": "Carlos",
        "email": "carlos@example.com"
      }
    }
  }
}
```

------

### **📌 5.4 Eliminar un Usuario (Mutation)**

```json
mutation {
  eliminarUsuario(id: 1) {
    mensaje
  }
}
```

📌 **Respuesta esperada:**

```json
{
  "data": {
    "eliminarUsuario": {
      "mensaje": "Usuario eliminado correctamente"
    }
  }
}
```

------

## **🔹 6. Ventajas de GraphQL vs REST**

| **Criterio**            | **GraphQL**                                   | **REST**                                                |
| ----------------------- | --------------------------------------------- | ------------------------------------------------------- |
| **Eficiencia**          | 🚀 Permite obtener solo los datos necesarios   | 📡 Puede devolver datos innecesarios                     |
| **Número de Endpoints** | 🔥 1 único endpoint (`/graphql`)               | 📌 Múltiples endpoints (`/usuarios`, `/productos`, etc.) |
| **Flexibilidad**        | 🔄 Se adapta a distintos clientes (móvil, web) | 📡 Menos flexible, retorna respuestas fijas              |
| **Complejidad**         | 🛠️ Más complejo de implementar                 | ⚡ Más fácil de usar                                     |

------

## **🔹 7. Conclusión**

✅ **GraphQL permite consultas flexibles y eficientes.**

✅ **Usamos Flask y Graphene para construir una API GraphQL desde cero.**

✅ **Probamos consultas y mutaciones en GraphiQL.**

✅ **GraphQL vs REST:** GraphQL es más eficiente, pero REST es más simple.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)