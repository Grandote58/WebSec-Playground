# Servicio web en **Python** utilizando **Flask** para consumir una API gratuita en tiempo real. 



### 1. Configuración del Entorno en Visual Studio Code

#### Paso 1: Instalar Python y Visual Studio Code

- Si no tienes Python instalado, descárgalo desde [python.org](https://www.python.org/downloads/).
- Descarga e instala **Visual Studio Code** desde [este enlace](https://code.visualstudio.com/).

#### Paso 2: Crear un Entorno Virtual

1. Abre **Visual Studio Code** y selecciona o crea una carpeta de trabajo.
2. Abre la terminal dentro de Visual Studio Code (**Ctrl + `**).
3. Crea un entorno virtual ejecutando:

```bash
python -m venv venv
```

#### Paso 3: Activar el Entorno Virtual

Activa el entorno virtual en Windows:

```bash
.\venv\Scripts\activate
```



#### Paso 4. Estructura del Proyecto

Tu proyecto debería tener la siguiente estructura:

```css
/tu_carpeta_de_proyecto
│
├── venv/                    # Entorno virtual se creara de forma automatica
├── app.py                   # Archivo principal del servicio web
├── /templates/
│   └── index.html           # Plantilla HTML para la página
└── requirements.txt         # Archivo opcional para dependencias
```



#### Paso 5: Instalar Flask y Requests

Con el entorno virtual activado, instala Flask y la biblioteca Requests para realizar solicitudes HTTP:

```bash
pip install Flask requests
```

### 2. Creación del Archivo Principal del Servicio Web

Crea un archivo llamado **app.py** en tu carpeta de trabajo. Este archivo contendrá el código que consumirás de una API gratuita.

### 3. Código del Servicio Web

A continuación, te proporciono el código, que consume una API de clima (por ejemplo, de **OpenWeatherMap**) y presenta los datos en la página. También permite cancelar la visualización con otro botón.

```python
# app.py
from flask import Flask, render_template, jsonify, request
import requests

# Crear la aplicación Flask
aplicacion = Flask(__name__)

# Variables globales para almacenar datos y estado
datos_clima = None
api_llamada_activa = False

# Ruta principal para mostrar la página con los botones
@aplicacion.route('/')
def inicio():
    return render_template('index.html')

# Ruta para consumir la API
@aplicacion.route('/iniciar', methods=['POST'])
def iniciar_consumo():
    global datos_clima, api_llamada_activa
    if not api_llamada_activa:
        # Realizar la llamada a la API
        api_llamada_activa = True
        ciudad = "Bogota"
        api_key = "TU_API_KEY"  # Reemplaza con tu clave de API
        url_api = f"http://api.openweathermap.org/data/2.5/weather?q={ciudad}&appid={api_key}&units=metric"
        respuesta = requests.get(url_api)
        
        if respuesta.status_code == 200:
            datos_clima = respuesta.json()
        else:
            datos_clima = {"error": "No se pudo obtener el clima."}
    return jsonify(datos_clima)

# Ruta para cancelar y eliminar los datos
@aplicacion.route('/cancelar', methods=['POST'])
def cancelar_consumo():
    global datos_clima, api_llamada_activa
    datos_clima = None
    api_llamada_activa = False
    return jsonify({"mensaje": "Servicio cancelado y datos eliminados."})

# Ejecutar la aplicación Flask
if __name__ == '__main__':
    aplicacion.run(debug=True)
```

### 4. Creación del Archivo de Plantilla HTML

Crea una carpeta llamada **templates** dentro de tu carpeta de trabajo. Dentro de esa carpeta, crea un archivo llamado **index.html** con el siguiente contenido:

```html
<!-- templates/index.html -->
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Servicio Web de Clima</title>
    <script>
        // Función para iniciar el consumo de la API
        function iniciarServicio() {
            fetch('/iniciar', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                }
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('resultado').innerHTML = JSON.stringify(data, null, 2);
            });
        }

        // Función para cancelar el servicio y eliminar el contenido
        function cancelarServicio() {
            fetch('/cancelar', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                }
            })
            .then(response => response.json())
            .then(data => {
                document.getElementById('resultado').innerHTML = '';
                alert(data.mensaje);
            });
        }
    </script>
</head>
<body>
    <h1>Servicio Web para Consultar el Clima</h1>
    <button onclick="iniciarServicio()">Iniciar Servicio</button>
    <button onclick="cancelarServicio()">Cancelar Servicio</button>
    <pre id="resultado"></pre>
</body>
</html>
```

### 5. Explicación del Código

- **Python/Flask:** El archivo `app.py` contiene la lógica para consumir una API de clima usando la biblioteca `requests`. También permite activar y desactivar el servicio según el estado.
- **HTML (index.html):** El archivo HTML tiene dos botones. Uno inicia la solicitud a la API, y el otro cancela el servicio y borra los datos. Todo ocurre en tiempo real, sin necesidad de recargar la página, utilizando **fetch** para hacer las solicitudes a Flask.
- **API utilizada:** En este ejemplo, usamos la API gratuita de **OpenWeatherMap**. Necesitas registrarte en [OpenWeatherMap](https://openweathermap.org/) para obtener una clave de API.

### 6. Ejecutar el Servicio Web

#### Paso 1: Ejecutar el servidor Flask

Con el entorno virtual activado, ejecuta el siguiente comando en la terminal de Visual Studio Code:

```bash
python app.py
```

Esto ejecutará el servidor Flask en `http://127.0.0.1:5000/`.

#### Paso 2: Probar el Servicio

1. Abre un navegador y dirígete a `http://127.0.0.1:5000/`.
2. Presiona el botón **"Iniciar Servicio"** para consumir la API y mostrar los datos en la página.
3. Presiona el botón **"Cancelar Servicio"** para eliminar los datos y cancelar la llamada al servicio.

### 8. Recomendaciones

- **Seguridad:** No olvides proteger la clave de la API y deshabilitar el modo de depuración en entornos de producción.
- **Mejoras:** Puedes ampliar este servicio para que admita múltiples ciudades, o agregar validación y manejo de errores para mejorar la experiencia del usuario.

