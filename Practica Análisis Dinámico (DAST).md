# Practica Análisis Dinámico (DAST)

Para crear un script de análisis DAST (Dynamic Application Security Testing) en Google Colab, podemos utilizar la herramienta **OWASP ZAP** (Zed Attack Proxy), que es ampliamente utilizada para realizar pruebas de seguridad en aplicaciones web. A continuación, te proporcionaré un paso a paso documentado para configurar y ejecutar un análisis DAST básico utilizando OWASP ZAP en Google Colab.

### **Paso 1: Preparar el Entorno en Google Colab**

Primero, necesitamos configurar Google Colab para ejecutar OWASP ZAP. Google Colab ejecuta Python en un entorno basado en Linux, por lo que utilizaremos comandos de shell para instalar y ejecutar OWASP ZAP.

```python
# Instalar Java, que es necesario para ejecutar OWASP ZAP
!apt-get update
!apt-get install -y openjdk-11-jre

# Descargar OWASP ZAP
!wget https://github.com/zaproxy/zaproxy/releases/download/v2.12.0/ZAP_2_12_0_unix.sh

# Hacer el script ejecutable
!chmod +x ZAP_2_12_0_unix.sh

# Ejecutar el instalador de ZAP
!./ZAP_2_12_0_unix.sh -q -dir /content/zap
```

### **Paso 2: Ejecutar OWASP ZAP en Modo Headless**

OWASP ZAP puede ejecutarse en modo "headless" (sin interfaz gráfica) utilizando la API REST. Esto es ideal para automatizar pruebas DAST desde scripts.

```python
# Ejecutar OWASP ZAP en modo headless
!nohup /content/zap/ZAP_2_12_0/zap.sh -daemon -host 127.0.0.1 -port 8080 -config api.key=12345 &

# Verificar que ZAP está corriendo
!curl http://127.0.0.1:8080
```

### **Paso 3: Configurar el Script de Análisis DAST**

A continuación, crearemos un script en Python que se conectará a la API de OWASP ZAP para realizar un análisis DAST de una aplicación web objetivo.

```python
import time
import requests

# Configuración básica
zap_base_url = 'http://127.0.0.1:8080'
zap_api_key = '12345'
target_url = 'http://example.com'  # Reemplaza con la URL de la aplicación a analizar

# Iniciar un escaneo de la aplicación objetivo
scan_url = f'{zap_base_url}/JSON/ascan/action/scan/?apikey={zap_api_key}&url={target_url}'
response = requests.get(scan_url)
scan_id = response.json()['scan']

# Monitorear el progreso del escaneo
scan_status_url = f'{zap_base_url}/JSON/ascan/view/status/?apikey={zap_api_key}&scanId={scan_id}'
while True:
    scan_status_response = requests.get(scan_status_url)
    scan_status = scan_status_response.json()['status']
    print(f'Estado del escaneo: {scan_status}%')
    if int(scan_status) >= 100:
        break
    time.sleep(10)

# Obtener los resultados del escaneo
scan_results_url = f'{zap_base_url}/JSON/core/view/alerts/?apikey={zap_api_key}&baseurl={target_url}'
scan_results = requests.get(scan_results_url).json()

# Mostrar los resultados
for alert in scan_results['alerts']:
    print(f"Amenaza: {alert['alert']}")
    print(f"Descripción: {alert['description']}")
    print(f"Solución: {alert['solution']}")
    print("=================================")
```

### **Paso 4: Documentar y Detallar Cada Paso**

1. **Instalación de Java y OWASP ZAP**:
   - **Java**: OWASP ZAP está basado en Java, por lo que necesitamos instalar el entorno de ejecución de Java (`openjdk-11-jre`).
   - **Descarga de ZAP**: Utilizamos `wget` para descargar el script de instalación de OWASP ZAP desde GitHub.
   - **Ejecución del Instalador**: Hacemos el script descargado ejecutable y lo instalamos en el directorio `/content/zap`.
2. **Ejecución de OWASP ZAP en Modo Headless**:
   - Ejecutamos OWASP ZAP en segundo plano (`daemon mode`) para que funcione sin la interfaz gráfica. Especificamos una clave API (`api.key`) para acceder a la API de ZAP.
   - Verificamos que ZAP se esté ejecutando correctamente haciendo una solicitud HTTP a la API en `http://127.0.0.1:8080`.
3. **Configuración y Ejecución del Análisis DAST**:
   - **Configuración**: Configuramos los parámetros básicos como la URL de la API de ZAP (`zap_base_url`), la clave API (`zap_api_key`), y la URL de la aplicación objetivo (`target_url`).
   - **Iniciar Escaneo**: Utilizamos la API de ZAP para iniciar un escaneo de seguridad contra la aplicación objetivo.
   - **Monitoreo del Escaneo**: Verificamos periódicamente el estado del escaneo hasta que se complete (estado del 100%).
   - **Obtención y Visualización de Resultados**: Una vez que el escaneo se completa, utilizamos la API para obtener los resultados y los mostramos en un formato legible.

### **Paso 5: Ejecución y Análisis**

Puedes ejecutar el script anterior en Google Colab para realizar un análisis DAST en la aplicación web que desees. Los resultados mostrarán las vulnerabilidades identificadas, junto con una descripción y posibles soluciones.

### **Notas Finales**

Este script proporciona una base sólida para realizar análisis DAST automatizados utilizando OWASP ZAP en Google Colab. Sin embargo, para análisis más complejos, puedes personalizar los parámetros de ZAP, ajustar las reglas de escaneo, y analizar múltiples URLs o aplicaciones. Además, es importante realizar estos análisis en entornos controlados y con el consentimiento del propietario de la aplicación.