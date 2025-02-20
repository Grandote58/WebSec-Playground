![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# 📘 **Creación de un Servicio Web SOAP**

**Objetivo:** Crear un servicio web SOAP funcional desde cero, entender su estructura y probar su funcionamiento.

**Tecnología usada:** Python con **Flask y Spyne** (para simplificar la implementación).

------

## 🔹 **1. ¿Qué es SOAP?**

**SOAP (Simple Object Access Protocol)** es un protocolo basado en XML que permite la comunicación entre aplicaciones a través de la red. Se usa en entornos empresariales donde se requiere **seguridad, transacciones y compatibilidad entre sistemas**.

📌 **Características clave:** 

✅ Basado en XML.

✅ Usa HTTP/HTTPS o SMTP como transporte.

✅ Soporta autenticación, encriptación y transacciones.

✅ Estándar en sistemas empresariales como bancos y aseguradoras.



## 🔹 **2. Instalación del Entorno**

Para crear un servicio web SOAP, usaremos Python con **Flask** y **Spyne**.

### **🖥️ 2.1 Instalar Python y Pip**

Si no tienes Python instalado, descárgalo desde [python.org](https://www.python.org/downloads/)
Para verificar la instalación:

```python
python --version
pip --version
```

### **🛠 2.2 Instalar Flask y Spyne**

Ejecuta en la terminal:

```python
pip install flask spyne
```

📌 **Flask**: Framework liviano para servidores web en Python.
📌 **Spyne**: Librería que permite crear servicios SOAP en Python.

## 🔹 **3. Creando el Servicio Web SOAP**

Vamos a crear un **servicio web SOAP que devuelva un saludo con el nombre que recibe como parámetro**.

### **📂 3.1 Estructura del Proyecto**

Crea una carpeta para el proyecto y dentro un archivo `app.py`:

```
📂 servicio-soap
 ┣ 📜 app.py  # Código del servicio
```

------

### **📌 3.2 Código del Servicio SOAP (app.py)**

Abre `app.py` y copia este código:

```python
from flask import Flask, request, Response
from spyne import Application, rpc, ServiceBase, Unicode
from spyne.protocol.soap import Soap11
from spyne.server.wsgi import WsgiApplication

# Inicializar Flask
app = Flask(__name__)

# Definir el servicio SOAP con Spyne
class SaludoService(ServiceBase):

    @rpc(Unicode, _returns=Unicode)
    def decir_hola(ctx, nombre):
        return f"¡Hola, {nombre}! Bienvenido a nuestro servicio SOAP."

# Crear la aplicación SOAP
soap_app = Application([SaludoService], 
                       'ejemplo.servicio.soap',
                       in_protocol=Soap11(validator='lxml'),
                       out_protocol=Soap11())

# Convertir la aplicación en una WSGI app
wsgi_app = WsgiApplication(soap_app)

# Ruta para el servicio SOAP en Flask
@app.route('/soap', methods=['POST'])
def soap_service():
    return Response(wsgi_app(request.environ, start_response), content_type='text/xml')

# Función necesaria para la respuesta del servidor WSGI
def start_response(status, response_headers, exc_info=None):
    response_headers.append(('Content-Type', 'text/xml'))
    return lambda x, y: None

# Iniciar el servidor
if __name__ == "__main__":
    app.run(debug=True, port=5000)
```

📌 **Explicación del código:**

✔ `@rpc(Unicode, _returns=Unicode)`: Define un método SOAP que recibe un `string` y devuelve un `string`.

✔ `Application()`: Crea la aplicación SOAP con el protocolo **SOAP 1.1**.

✔ `WsgiApplication()`: Convierte la aplicación SOAP en una WSGI app que Flask puede manejar.

✔ `/soap`: Ruta donde el servicio SOAP responderá a las solicitudes.

## 🔹 **4. Ejecutar el Servidor SOAP**

Para iniciar el servicio web, abre la terminal en la carpeta del proyecto y ejecuta:

```python
python app.py
```

Deberías ver algo como:

```python
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

------

## 🔹 **5. Probar el Servicio SOAP**

### **🛠 5.1 Crear una Petición SOAP (XML)**

SOAP usa mensajes en formato XML. Para probarlo, podemos usar `cURL` o una herramienta como **Postman**.

Ejemplo de **petición SOAP**:

```xml
<?xml version="1.0"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:exa="ejemplo.servicio.soap">
   <soapenv:Header/>
   <soapenv:Body>
      <exa:decir_hola>
         <nombre>Juan</nombre>
      </exa:decir_hola>
   </soapenv:Body>
</soapenv:Envelope>
```

### **📌 5.2 Enviar la Petición con cURL**

Desde la terminal, ejecuta:

```xml
curl -X POST -H "Content-Type: text/xml" --data @solicitud.xml http://localhost:5000/soap
```

📌 **Salida esperada:**

```xml
<?xml version='1.0' encoding='utf-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <ns0:decir_holaResponse xmlns:ns0="ejemplo.servicio.soap">
            <ns0:decir_holaResult>¡Hola, Juan! Bienvenido a nuestro servicio SOAP.</ns0:decir_holaResult>
        </ns0:decir_holaResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

------

## 🔹 **6. Explicación del Funcionamiento**

### **📦 Estructura de una Respuesta SOAP**

Una respuesta SOAP se compone de tres partes principales:

```xml
<soapenv:Envelope>  <!-- Contenedor del mensaje SOAP -->
    <soapenv:Body>  <!-- Contiene la respuesta -->
        <ns0:decir_holaResponse>  <!-- Nombre del método -->
            <ns0:decir_holaResult>¡Hola, Juan!</ns0:decir_holaResult>  <!-- Respuesta del servicio -->
        </ns0:decir_holaResponse>
    </soapenv:Body>
</soapenv:Envelope>
```

📌 **Diferencia con REST:**

✔ SOAP usa XML, mientras que REST usa JSON o XML.

✔ SOAP es más seguro y usado en entornos empresariales.

✔ SOAP requiere una estructura estricta, REST es más flexible.



## 🔹 **7. Extender el Servicio SOAP**

Podemos agregar más métodos al servicio:

```python
class SaludoService(ServiceBase):

    @rpc(Unicode, _returns=Unicode)
    def decir_hola(ctx, nombre):
        return f"¡Hola, {nombre}! Bienvenido a nuestro servicio SOAP."

    @rpc(Unicode, Unicode, _returns=Unicode)
    def sumar(ctx, num1, num2):
        return f"La suma es: {int(num1) + int(num2)}"
```

Ahora puedes enviar una petición SOAP con dos números y obtener su suma.

## **📌 8. Conclusión**

✅ **SOAP** es ideal para aplicaciones empresariales donde se requiere **seguridad y confiabilidad**.

✅ **Usamos Python con Flask y Spyne** para crear un servicio SOAP sencillo y funcional.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)