#  **Servicio Web utilizando Python y Flask**



### 1. Configuración del entorno en Visual Studio Code

#### Paso 1: Instalar Python

Si no tienes Python instalado, descárgalo desde [python.org](https://www.python.org/downloads/). Asegúrate de marcar la opción **Add Python to PATH** durante la instalación.

#### Paso 2: Instalar Visual Studio Code

Descarga e instala Visual Studio Code desde [este enlace](https://code.visualstudio.com/).

#### Paso 3: Estructura del Proyecto

El proyecto quedará organizado de la siguiente manera:

```
/tu_carpeta_de_proyecto
│
├── venv/              # Entorno virtual se creara por defecto
├── app.py             # Archivo principal del servicio web
└── requirements.txt   # Opcional: archivo para las dependencias
```



#### Paso 4: Crear un entorno virtual

1. Abre **Visual Studio Code** y selecciona la carpeta en la que deseas trabajar (o crea una nueva).
2. Abre la terminal dentro de Visual Studio Code (presiona **Ctrl + `**).
3. Ejecuta el siguiente comando para crear un entorno virtual:

```bash
python -m venv venv
```

Esto creará una carpeta llamada `venv` que contiene el entorno virtual.

#### Paso 5: Activar el entorno virtual

Para activar el entorno virtual en Windows, ejecuta:

```bash
.\venv\Scripts\activate
```

Verás que tu terminal cambia para indicar que el entorno virtual está activo.

#### Paso 5: Instalar Flask

Con el entorno virtual activado, instala Flask:

```bash
pip install Flask
```

#### Paso 6: Crear el archivo principal del servicio web

Crea un archivo llamado **app.py** en tu carpeta de trabajo.

### 2. Código del Servicio Web con Flask

A continuación, te proporciono un servicio web básico con Flask que responde a una solicitud GET y devuelve un mensaje JSON. El código está documentado y usa variables en español para facilitar la comprensión.

```python
# app.py
from flask import Flask, jsonify, request

# Crear la aplicación Flask
aplicacion = Flask(__name__)

# Ruta principal del servicio web
@aplicacion.route('/', methods=['GET'])
def inicio():
    # Respuesta para la ruta principal
    mensaje = {
        "mensaje": "¡Bienvenido al servicio web!",
        "exito": True
    }
    return jsonify(mensaje), 200

# Ruta para un recurso específico
@aplicacion.route('/datos/<string:nombre>', methods=['GET'])
def obtener_datos(nombre):
    # Procesar la entrada del usuario y generar una respuesta
    mensaje = {
        "mensaje": f"Hola, {nombre}. Esta es tu respuesta desde el servicio web.",
        "exito": True
    }
    return jsonify(mensaje), 200

# Ruta para recibir datos mediante POST
@aplicacion.route('/enviar', methods=['POST'])
def recibir_datos():
    # Obtener datos enviados en formato JSON
    datos = request.get_json()
    if datos:
        respuesta = {
            "mensaje": f"Datos recibidos exitosamente: {datos}",
            "exito": True
        }
        return jsonify(respuesta), 200
    else:
        return jsonify({"mensaje": "No se enviaron datos.", "exito": False}), 400

# Ejecutar la aplicación Flask
if __name__ == '__main__':
    aplicacion.run(debug=True)
```

### 3. Explicación del código:

1. **Flask y JSON:** Usamos Flask para manejar las solicitudes HTTP y la biblioteca `jsonify` para enviar respuestas en formato JSON.
2. Rutas:
   - La ruta `/` devuelve un mensaje de bienvenida.
   - La ruta `/datos/<nombre>` toma un parámetro de la URL y devuelve un mensaje personalizado.
   - La ruta `/enviar` recibe datos en formato JSON mediante un método POST.
3. **Ejecución del servidor:** El servidor se ejecuta con `aplicacion.run(debug=True)` para habilitar el modo de depuración.

### 4. Ejecutar el Servicio Web

#### Paso 1: Ejecutar el servidor Flask

Con el entorno virtual activado, ejecuta el siguiente comando en la terminal de Visual Studio Code para iniciar tu servicio web:

```bash
python app.py
```

Esto ejecutará el servidor local de Flask en `http://127.0.0.1:5000/`.

#### Paso 2: Probar el servicio

Puedes probar las rutas del servicio web con un navegador o herramientas como **Postman**.

- **GET request en el navegador:**

  - `http://127.0.0.1:5000/` muestra el mensaje de bienvenida.
  - `http://127.0.0.1:5000/datos/TuNombre` personaliza la respuesta.

- **POST request con Postman:**

  - Configura una solicitud POST en `http://127.0.0.1:5000/enviar` con datos en formato JSON, como:

  ```json
  {
     "nombre": "Laura",
     "edad": 25
  }
  ```

  El servicio te devolverá un mensaje confirmando la recepción de los datos.

#### Paso 3: Desactivar el entorno virtual

Una vez que termines de trabajar, puedes desactivar el entorno virtual ejecutando:

```bash
deactivate
```

### 6. Recomendaciones

- **Documentación:** Asegúrate de mantener un archivo `README.md` con instrucciones sobre cómo ejecutar el servicio y probar las rutas.
- **Entorno de producción:** Para entornos de producción, considera el uso de un servidor web como **Gunicorn** y la configuración adecuada de seguridad.

https://gunicorn.org/