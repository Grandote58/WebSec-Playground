![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 📘 **Uso de Middleware en Windows 11**

**Objetivo:** Aprender a utilizar **middleware** en una aplicación web en **Windows 11**, comprendiendo su uso en **autenticación, seguridad y monitoreo**.
**Tecnología usada:** **Python con Flask**

## **🔹 1. ¿Qué es un Middleware?**

Un **middleware** es un software intermedio que se ejecuta entre la petición del usuario y la respuesta del servidor. En aplicaciones web, **intercepta solicitudes y respuestas para agregar funcionalidades como autenticación, monitoreo y protección contra ataques**.

📌 **Casos de Uso del Middleware en Seguridad Web:**

✅ **Autenticación** → Verifica usuarios antes de acceder a los recursos.

✅ **Autorización** → Controla permisos sobre recursos protegidos.

✅ **Protección contra ataques** → Previene inyecciones SQL, XSS, CSRF.

✅ **Monitoreo y Logging** → Registra cada solicitud para auditoría.

## **🔹 2. Instalación del Entorno en Windows 11**

Para este tutorial, utilizaremos **Python con Flask** para implementar middleware.

### **🖥️ 2.1 Instalar Python y Pip**

Si no tienes Python instalado, descárgalo desde [python.org](https://www.python.org/downloads/).

Para verificar la instalación, abre la terminal **CMD o PowerShell** y ejecuta:

```bash
python --version
pip --version
```

Si `pip` no está instalado, instálalo con:

```bash
python -m ensurepip --default-pip
```

### **🛠 2.2 Instalar Flask**

Ejecuta:

```bash
pip install flask
```

📌 **Flask** nos permitirá construir una aplicación web en la que aplicaremos middleware.

## **🔹 3. Implementando Middleware en Flask**

Vamos a crear una **API con middleware** para **autenticación, registro de logs y protección contra ataques**.

### **📂 3.1 Estructura del Proyecto**

Crea una carpeta para el proyecto y dentro un archivo `app.py`:

```bash
📂 middleware-windows
 ┣ 📜 app.py  # Código del servidor
```

------

### **📌 3.2 Código Base del Servidor con Middleware (app.py)**

Abre `app.py` con un editor de texto (VS Code, Notepad++, etc.) y copia este código:

```python
from flask import Flask, request, jsonify
import time
import logging

app = Flask(__name__)

# Configurar Logging
logging.basicConfig(filename="registro.log", level=logging.INFO, format="%(asctime)s - %(message)s")

# Base de datos simulada de usuarios con tokens
TOKENS_VALIDOS = ["token123", "seguro456"]

# 🛡️ Middleware de Autenticación
@app.before_request
def autenticar():
    token = request.headers.get("Authorization")
    if not token or token.replace("Bearer ", "") not in TOKENS_VALIDOS:
        return jsonify({"error": "No autorizado"}), 403

# 📜 Middleware de Logging
@app.before_request
def registrar_log():
    logging.info(f"{request.remote_addr} - {request.method} {request.path}")

# 🔥 Middleware contra ataques de fuerza bruta (Rate Limiting)
LIMITES_IP = {}

@app.before_request
def limitar_peticiones():
    ip = request.remote_addr
    ahora = time.time()

    if ip in LIMITES_IP:
        tiempo_ultimo_acceso, intentos = LIMITES_IP[ip]
        if ahora - tiempo_ultimo_acceso < 10:  # 10 segundos de protección
            if intentos >= 3:
                return jsonify({"error": "Demasiadas solicitudes, espera unos segundos."}), 429
            LIMITES_IP[ip] = (ahora, intentos + 1)
        else:
            LIMITES_IP[ip] = (ahora, 1)
    else:
        LIMITES_IP[ip] = (ahora, 1)

# 🛠️ Ruta de Prueba
@app.route('/protegido', methods=['GET'])
def protegido():
    return jsonify({"mensaje": "Bienvenido al área segura."})

# 🚀 Iniciar el servidor en Windows 11
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
```

📌 **Explicación del Código:**

✔ **Middleware de Autenticación:** Verifica si el usuario envía un **token válido** en el encabezado `Authorization`.

✔ **Middleware de Logging:** Guarda cada solicitud en un archivo `registro.log`.

✔ **Middleware Anti Fuerza Bruta:** Bloquea IPs que hagan demasiadas solicitudes en poco tiempo.

## **🔹 4. Ejecutar el Servidor en Windows 11**

### **📌 4.1 Iniciar el Servidor**

Abre **CMD o PowerShell** en la carpeta donde está `app.py` y ejecuta:

```bash
python app.py
```

Verás algo como:

```bash
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

------

## **🔹 5. Probar el Middleware en Windows 11**

Podemos probar el middleware con **cURL** en **CMD o PowerShell**.

### **📌 5.1 Prueba de Acceso sin Token**

```bash
curl -X GET http://localhost:5000/protegido
```

📌 **Respuesta esperada:**

```json
{"error": "No autorizado"}
```

### **📌 5.2 Prueba con Token Correcto**

```json
curl -X GET http://localhost:5000/protegido -H "Authorization: Bearer token123"
```

📌 **Respuesta esperada:**

```json
{"mensaje": "Bienvenido al área segura."}
```

### **📌 5.3 Simulación de Ataque de Fuerza Bruta**

Ejecuta rápidamente varias veces:

```json
curl -X GET http://localhost:5000/protegido -H "Authorization: Bearer token123"
```

📌 **Si haces más de 3 peticiones en 10 segundos, verás:**

```json
{"error": "Demasiadas solicitudes, espera unos segundos."}
```

## **🔹 6. Mejoras y Seguridad Avanzada**

📌 **Agregar Protección contra CORS** Para evitar que otros sitios web hagan solicitudes no autorizadas a nuestra API, podemos agregar:

```python
from flask_cors import CORS
CORS(app)
```

📌 **Protección contra Inyección SQL** Si trabajamos con bases de datos, podemos usar **SQLAlchemy** con consultas parametrizadas para evitar inyecciones SQL.

📌 **Protección contra XSS (Cross-Site Scripting)** Podemos sanitizar los datos de entrada con **html.escape()**.

📌 **Autenticación con JWT** Para mejorar la seguridad del middleware de autenticación, podemos usar tokens **JWT** en lugar de simples strings.

## **🔹 7. Conclusión**

✅ **Middleware es esencial para la seguridad en aplicaciones web.**

✅ **Hemos implementado autenticación, logs y protección contra ataques en Windows 11.**

✅ **Usamos Python con Flask para interceptar y gestionar solicitudes.**

✅ **Probamos el servicio con cURL y analizamos su respuesta.**



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)