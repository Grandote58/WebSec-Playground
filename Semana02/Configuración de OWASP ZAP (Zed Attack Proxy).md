![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Configuración de OWASP ZAP (Zed Attack Proxy)**

OWASP ZAP (Zed Attack Proxy) es una de las herramientas más populares y ampliamente utilizadas por pentesters y profesionales de seguridad para realizar pruebas de seguridad en aplicaciones web. Es gratuito, de código abierto, y extremadamente potente, lo que lo convierte en una excelente alternativa a herramientas como Burp Suite. Este tutorial te guiará paso a paso en la instalación, configuración y uso de OWASP ZAP.

------

## **Paso 1: Descarga e instalación de OWASP ZAP**

1. **Descarga OWASP ZAP**:

   - Ve al sitio oficial: https://www.zaproxy.org/download/.
   - Descarga la versión correspondiente a tu sistema operativo (Windows, macOS o Linux).

2. **Instalación**:

   - Windows:

     - Descarga el instalador `.exe` y ejecútalo.
     - Sigue el asistente de instalación seleccionando las configuraciones predeterminadas.

   - macOS:

     - Descarga el archivo `.dmg`, ábrelo y arrastra OWASP ZAP a tu carpeta de aplicaciones.

   - Linux:

     - Descarga el archivo 

       ```css
       .tar.gz
       ```

       , descomprímelo y ejecuta el archivo 

       ```css
       zap.sh
       ```

        desde el terminal:

       ```css
       ./zap.sh
       ```

3. **Verifica la instalación**:

   - Abre OWASP ZAP y espera a que se cargue la interfaz.
   - Cuando se inicie, selecciona `Start a new session` (Iniciar una nueva sesión).

## **Paso 2: Familiarización con la interfaz de OWASP ZAP**

Antes de configurarlo, es importante entender los elementos principales de la interfaz:

- **Tree View (Panel izquierdo)**:
  - Muestra una vista jerárquica de los sitios web escaneados.
  - Aquí verás todas las URL y directorios detectados durante la exploración.
- **Panel central**:
  - Contiene las pestañas principales, como `Request` (Solicitud) y `Response` (Respuesta), donde se puede analizar el tráfico HTTP.
- **Panel inferior**:
  - Incluye herramientas como `Alerts` (alertas de seguridad), `History` (historial de solicitudes) y `Active Scan` (escaneo activo).
- **Barra de herramientas superior**:
  - Ofrece acceso rápido a funcionalidades como escaneo, proxy y ataques de fuerza bruta.

## **Paso 3: Configuración del Proxy en OWASP ZAP**

OWASP ZAP funciona interceptando el tráfico web entre tu navegador y el servidor objetivo. Para hacerlo, necesitas configurarlo como proxy.

1. **Verifica el puerto del proxy**:

   - En OWASP ZAP, ve a `Tools > Options > Local Proxy` (Herramientas > Opciones > Proxy local).
   - Asegúrate de que el proxy esté configurado en:
     - **Dirección**: `127.0.0.1`
     - **Puerto**: `8080` (puedes cambiarlo si es necesario).

2. **Configura tu navegador para usar el proxy**:

   - Firefox

      (recomendado):

     1. Ve a `Menú > Configuración > General > Configuración de red > Configuración manual del proxy`.
     2. Introduce los siguientes valores:
        - **Proxy HTTP**: `127.0.0.1`
        - **Puerto**: `8080`
     3. Activa la opción "Usar este proxy para todos los protocolos".
     4. Haz clic en `Aceptar`.

   - Chrome

      (o cualquier navegador basado en Chromium):

     - Utiliza una extensión como **FoxyProxy** para manejar configuraciones de proxy.

3. **Prueba la conexión**:

   - Abre tu navegador configurado y visita cualquier sitio web (por ejemplo, `http://example.com`).
   - Verifica que el tráfico sea interceptado en OWASP ZAP:
     - Ve a la pestaña `History` (Historial) en la parte inferior.
     - Deberías ver todas las solicitudes realizadas por el navegador.

## **Paso 4: Configuración del Certificado HTTPS de OWASP ZAP**

Para interceptar tráfico HTTPS (cifrado), necesitas instalar el certificado SSL generado por OWASP ZAP en tu navegador.

1. **Exporta el certificado desde OWASP ZAP**:
   - Ve a `Tools > Options > Dynamic SSL Certificates`.
   - Haz clic en `Save` (Guardar) y selecciona una ubicación para guardar el archivo del certificado (`owasp_zap_root_ca.cer`).
2. **Instala el certificado en tu navegador**:
   - Firefox:
     1. Ve a `Menú > Configuración > Privacidad y seguridad > Certificados > Ver Certificados > Importar`.
     2. Selecciona el archivo `owasp_zap_root_ca.cer`.
     3. Marca la opción "Confiar en este certificado para identificar sitios web".
     4. Haz clic en `Aceptar`.
   - Chrome:
     1. Ve a `chrome://settings/` > `Seguridad y privacidad > Administrar certificados`.
     2. Importa el archivo `owasp_zap_root_ca.cer` y confía en él para conexiones HTTPS.
3. **Prueba el tráfico HTTPS**:
   - Visita un sitio web HTTPS (por ejemplo, `https://example.com`).
   - Verifica que el tráfico aparezca en la pestaña `History` de OWASP ZAP.

## **Paso 5: Escaneo Pasivo**

El escaneo pasivo analiza las solicitudes y respuestas capturadas sin enviar tráfico adicional.

1. **Configura el escaneo pasivo**:
   - Asegúrate de que el tráfico esté siendo interceptado (Proxy activado).
   - Navega por el sitio objetivo desde tu navegador.
2. **Revisa las alertas**:
   - Ve a la pestaña `Alerts` (Alertas) en OWASP ZAP.
   - Aquí verás vulnerabilidades detectadas, como:
     - X-Content-Type-Options faltante.
     - Ausencia de encabezados de seguridad.
     - Archivos expuestos.

## **Paso 6: Escaneo Activo**

El escaneo activo envía solicitudes adicionales al servidor para identificar vulnerabilidades más profundas. **Advertencia: Esto puede ser intrusivo y causar cambios en el sitio objetivo. Asegúrate de tener permiso antes de realizarlo.**

1. **Selecciona un objetivo**:
   - Haz clic derecho en una URL o carpeta en el árbol de navegación (panel izquierdo).
   - Selecciona `Attack > Active Scan`.
2. **Configura el escaneo**:
   - Especifica las reglas o configuraciones que deseas aplicar (por ejemplo, rangos de pruebas específicas).
3. **Inicia el escaneo**:
   - Haz clic en `Start Scan`.
   - Revisa los resultados en la pestaña `Alerts` (Alertas).

## **Paso 7: Uso de otras herramientas de OWASP ZAP**

1. **Spidering (Mapeo del sitio)**:
   - Haz clic derecho en el sitio en el árbol de navegación y selecciona `Attack > Spider`.
   - Esto explorará automáticamente todas las páginas y enlaces dentro del dominio.
2. **Fuzzing (Pruebas con datos aleatorios)**:
   - Selecciona una solicitud en el historial.
   - Haz clic derecho y elige `Fuzz`.
   - Configura los parámetros para enviar datos maliciosos o aleatorios.
3. **Forzar navegación (Forced Browse)**:
   - Utiliza la herramienta de fuerza bruta para descubrir directorios y archivos ocultos.

## **Paso 8: Buenas prácticas al usar OWASP ZAP**

1. **Pruebas éticas**:
   - Asegúrate de tener autorización para realizar pruebas en cualquier aplicación web.
   - No utilices OWASP ZAP en aplicaciones que no controles o sin el permiso del propietario.
2. **Evita interrupciones**:
   - Realiza escaneos activos en un entorno de pruebas, ya que pueden causar errores o interrupciones en el sitio.
3. **Documenta tus hallazgos**:
   - Utiliza la funcionalidad de reportes en OWASP ZAP (`Report > Generate Report`) para crear informes detallados de vulnerabilidades.