![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 📘 **Uso de Middleware en Kali Linux**

**Objetivo:** Aprender a utilizar **middleware** en aplicaciones web dentro de **Kali Linux**, comprendiendo su uso en seguridad, autenticación y monitoreo.
**Tecnología usada:** **Python con Flask**

------

## **🔹 1. ¿Qué es un Middleware?**

Un **middleware** es un software intermedio que se ejecuta entre la petición de un cliente y la respuesta del servidor. Se utiliza para:

📌 **Casos de Uso de Middleware en Seguridad Web:**

✅ **Autenticación** → Verifica usuarios antes de acceder a los recursos.

✅ **Autorización** → Controla permisos sobre recursos protegidos.

✅ **Protección contra ataques** → Previene inyecciones SQL, XSS, CSRF.

✅ **Monitoreo y Logging** → Registra cada solicitud para auditoría.

## **🔹 2. Instalación del Entorno en Kali Linux**

Para este tutorial, utilizaremos **Python con Flask** para implementar middleware.

### **🖥️ 2.1 Instalar Python y Pip**

Kali Linux ya incluye Python, pero puedes verificarlo con:

```
python3 --version
pip3 --version
```

Si no tienes `pip`, instálalo con:

```bash
sudo apt install python3-pip -y
```

### **🛠 2.2 Instalar Flask**

Ejecuta:

```bash
pip3 install flask
```

📌 **Flask** nos permitirá construir una aplicación web en la que aplicaremos middleware.

------

## **🔹 3. Implementando Middleware en Flask**

Vamos a crear una **API con middleware** para **autenticación, registro de logs y protección contra ataques**.

### **📂 3.1 Estructura del Proyecto**

Crea una carpeta para el proyecto y dentro un archivo `app.py`:

```bash
📂 middleware-kali
 ┣ 📜 app.py  # Código del servidor
```

### **📌 3.2 Código Base del Servidor con Middleware (app.py)**

Abre `app.py` y copia este código:

```python
from flask import Flask, request, jsonify
import time

app = Flask(__name__)

# Lista de tokens autorizados (simulación)
TOKENS_VALIDOS = ["secreto123", "tokenSeguro456"]

# 🛡️ Middleware de Autenticación
@app.before_request
def autenticar():
    token = request.headers.get("Authorization")
    if not token or token.replace("Bearer ", "") not in TOKENS_VALIDOS:
        return jsonify({"error": "No autorizado"}), 403

# 📜 Middleware de Logging
@app.before_request
def registrar_log():
    with open("registro.log", "a") as log:
        log.write(f"{time.strftime('%Y-%m-%d %H:%M:%S')} - {request.method} {request.path}\n")

# 🔥 Middleware contra ataques por fuerza bruta (Rate Limiting)
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

# 🚀 Iniciar el servidor en Kali Linux
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
```

📌 **Explicación del Código:**

✔ **Middleware de Autenticación:** Verifica si el usuario envía un **token válido** en el encabezado `Authorization`.

✔ **Middleware de Logging:** Guarda cada solicitud en un archivo `registro.log`.

✔ **Middleware Anti Fuerza Bruta:** Bloquea IPs que hagan demasiadas solicitudes en poco tiempo.

## **🔹 4. Ejecutar el Servidor en Kali Linux**

### **📌 4.1 Iniciar el Servidor**

Ejecuta en la terminal:

```bash
python3 app.py
```

Verás algo como:

```python
* Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

## **🔹 5. Probar el Middleware en Kali Linux**

Podemos probar el middleware con **cURL**.

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
curl -X GET http://localhost:5000/protegido -H "Authorization: Bearer secreto123"
```

📌 **Respuesta esperada:**

```json
{"mensaje": "Bienvenido al área segura."}
```

### **📌 5.3 Simulación de Ataque de Fuerza Bruta**

Ejecuta rápidamente varias veces:

```bash
curl -X GET http://localhost:5000/protegido -H "Authorization: Bearer secreto123"
```

📌 **Si haces más de 3 peticiones en 10 segundos, verás:**

```json
{"error": "Demasiadas solicitudes, espera unos segundos."}
```

## **🔹 6. Mejoras y Seguridad Avanzada**

📌 **Agregar Protección contra CORS** Podemos evitar que otros sitios web maliciosos hagan solicitudes a nuestra API agregando:

```python
from flask_cors import CORS
CORS(app)
```

📌 **Protección contra Inyección SQL** Si trabajamos con bases de datos, podemos usar **SQLAlchemy** con consultas parametrizadas para evitar inyecciones SQL.

📌 **Protección contra XSS (Cross-Site Scripting)** Podemos sanitizar los datos de entrada con **html.escape()**.

📌 **Autenticación con JWT** Para mejorar la seguridad del middleware de autenticación, podemos usar tokens JWT en lugar de simples strings.

## **🔹 7. Conclusión**

✅ **Middleware es clave para la seguridad en aplicaciones web.**

✅ **Hemos implementado autenticación, logs y protección contra ataques en Kali Linux.**

✅ **Usamos Python con Flask para demostrar cómo interceptar solicitudes.**

✅ **Probamos el servicio con cURL y analizamos su respuesta.**

📢 **¿Quieres agregar integración con bases de datos o autenticación con JWT? 🚀**

![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)