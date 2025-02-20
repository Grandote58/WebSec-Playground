![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 📘 **Creación de un Servicio Web RESTful**

**Objetivo:** Crear un servicio web RESTful funcional desde cero, entender su estructura y probar su funcionamiento.

**Tecnología usada:** **Python con Flask** (rápido y fácil de implementar).

## 🔹 **1. ¿Qué es un Servicio Web RESTful?**

REST (**Representational State Transfer**) es un **estilo arquitectónico** para la comunicación entre sistemas a través del protocolo HTTP.

📌 **Características clave:**

✅ **Basado en HTTP** (usa métodos GET, POST, PUT, DELETE).

✅ **Usa JSON o XML** para el intercambio de datos.

✅ **Stateless** (sin estado, cada petición es independiente).

✅ **Estructura de URLs limpia y organizada**.

## 🔹 **2. Instalación del Entorno**

### **🖥️ 2.1 Instalar Python y Pip**

Si no tienes Python instalado, descárgalo desde [python.org](https://www.python.org/downloads/)
Para verificar la instalación:

```python
python --version
pip --version
```

### **🛠 2.2 Instalar Flask**

Ejecuta en la terminal:

```python
pip install flask
```

📌 **Flask** es un framework ligero para crear aplicaciones web en Python.

## 🔹 **3. Creando el Servicio Web RESTful**

Vamos a crear un **servicio RESTful para gestionar una lista de usuarios**.

### **📂 3.1 Estructura del Proyecto**

Crea una carpeta para el proyecto y dentro un archivo `app.py`:

```python
📂 servicio-restful
 ┣ 📜 app.py  # Código del servicio
```

### **📌 3.2 Código del Servicio RESTful (app.py)**

Abre `app.py` y copia este código:

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

# Base de datos simulada (lista en memoria)
usuarios = [
    {"id": 1, "nombre": "Juan", "email": "juan@example.com"},
    {"id": 2, "nombre": "Maria", "email": "maria@example.com"}
]

# Obtener todos los usuarios (GET)
@app.route('/usuarios', methods=['GET'])
def obtener_usuarios():
    return jsonify(usuarios)

# Obtener un usuario por ID (GET)
@app.route('/usuarios/<int:id>', methods=['GET'])
def obtener_usuario(id):
    usuario = next((u for u in usuarios if u["id"] == id), None)
    return jsonify(usuario) if usuario else ('Usuario no encontrado', 404)

# Agregar un usuario (POST)
@app.route('/usuarios', methods=['POST'])
def agregar_usuario():
    nuevo_usuario = request.get_json()
    usuarios.append(nuevo_usuario)
    return jsonify(nuevo_usuario), 201

# Actualizar un usuario (PUT)
@app.route('/usuarios/<int:id>', methods=['PUT'])
def actualizar_usuario(id):
    usuario = next((u for u in usuarios if u["id"] == id), None)
    if usuario:
        datos = request.get_json()
        usuario.update(datos)
        return jsonify(usuario)
    return ('Usuario no encontrado', 404)

# Eliminar un usuario (DELETE)
@app.route('/usuarios/<int:id>', methods=['DELETE'])
def eliminar_usuario(id):
    global usuarios
    usuarios = [u for u in usuarios if u["id"] != id]
    return ('', 204)

# Iniciar el servidor
if __name__ == "__main__":
    app.run(debug=True, port=5000)
```

📌 **Explicación del código:**

✔ **`GET /usuarios`** → Obtiene la lista de usuarios.

✔ **`GET /usuarios/<id>`** → Obtiene un usuario específico.

✔ **`POST /usuarios`** → Agrega un nuevo usuario.

✔ **`PUT /usuarios/<id>`** → Modifica un usuario existente.

✔ **`DELETE /usuarios/<id>`** → Elimina un usuario.

## 🔹 **4. Ejecutar el Servidor REST**

Para iniciar el servicio web, abre la terminal en la carpeta del proyecto y ejecuta:

```bash
python app.py
```

Deberías ver algo como:

```bash
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

------

## 🔹 **5. Probar el Servicio RESTful**

### **📌 5.1 Probar con cURL**

Desde la terminal, ejecuta estos comandos para probar cada endpoint.

#### **Obtener todos los usuarios (GET)**

```bash
curl -X GET http://localhost:5000/usuarios
```

📌 **Respuesta esperada (JSON):**

```json
[
    {"id": 1, "nombre": "Juan", "email": "juan@example.com"},
    {"id": 2, "nombre": "Maria", "email": "maria@example.com"}
]
```

#### **Obtener un usuario por ID (GET)**

```bash
curl -X GET http://localhost:5000/usuarios/1
```

#### **Agregar un usuario (POST)**

```bash
curl -X POST http://localhost:5000/usuarios -H "Content-Type: application/json" -d '{"id": 3, "nombre": "Carlos", "email": "carlos@example.com"}'
```

#### **Actualizar un usuario (PUT)**

```bash
curl -X PUT http://localhost:5000/usuarios/1 -H "Content-Type: application/json" -d '{"nombre": "Juan Pérez"}'
```

#### **Eliminar un usuario (DELETE)**

```bash
curl -X DELETE http://localhost:5000/usuarios/1
```

## 🔹 **6. Explicación del Funcionamiento**

### **🛠 Estructura de una Respuesta RESTful**

Un servicio RESTful devuelve datos en **JSON**, por ejemplo:

```json
{
    "id": 1,
    "nombre": "Juan",
    "email": "juan@example.com"
}
```

### **✅ Ventajas de RESTful**



✔ **Ligero** → Usa JSON, más rápido que XML (SOAP).

✔ **Escalable** → Fácil de integrar con aplicaciones móviles y web.

✔ **Flexible** → Puede usarse con múltiples clientes.

### **❌ Desventajas de RESTful**

❌ No soporta **transacciones complejas** (SOAP sí).

❌ No incluye **seguridad avanzada** por defecto (como WS-Security de SOAP).

## 🔹 **7. Mejoras y Seguridad**

Podemos mejorar el servicio agregando autenticación, validaciones y seguridad.

### **📌 7.1 Middleware de Autenticación (Token)**

Podemos agregar un middleware para verificar un **API Key**:

```python
from functools import wraps

API_KEY = "secreto123"

def autenticar(f):
    @wraps(f)
    def decorador(*args, **kwargs):
        token = request.headers.get("Authorization")
        if token == f"Bearer {API_KEY}":
            return f(*args, **kwargs)
        return jsonify({"error": "No autorizado"}), 403
    return decorador

@app.route('/usuarios', methods=['GET'])
@autenticar
def obtener_usuarios():
    return jsonify(usuarios)
```

📌 **Prueba con cURL:**

```python
curl -X GET http://localhost:5000/usuarios -H "Authorization: Bearer secreto123"
```

------

## **📌 8. Conclusión**

✅ **RESTful es ideal para APIs modernas**, fácil de construir y escalar.

✅ **Usamos Python con Flask** para implementar un servicio funcional en pocos pasos.

✅ **Probamos cada endpoint con cURL** y analizamos el formato JSON.

✅ **REST vs SOAP:** REST es más flexible y rápido, pero SOAP es más seguro para transacciones críticas.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)