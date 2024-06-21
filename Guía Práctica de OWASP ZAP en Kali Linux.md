# ![Mesa de trabajo 1Head](http://drive.google.com/uc?export=view&id=1p2rqX0Nck3MI8LKzYct_oEMRETRIhzTH)


### Guía Práctica de OWASP ZAP en Kali Linux

OWASP ZAP (Zed Attack Proxy) es una herramienta popular y poderosa para la prueba de seguridad de aplicaciones web. A continuación, se presenta una guía paso a paso para instalar, configurar y utilizar ZAP en Kali Linux.

### Paso 1: Instalación de OWASP ZAP

1. **Actualizar los repositorios**:

   ```bash
   sudo apt update
   ```

2. **Instalar OWASP ZAP**:

   ```bash
   sudo apt install zaproxy -y
   ```

3. **Iniciar ZAP**: Puedes iniciar ZAP desde el menú de aplicaciones o ejecutando el siguiente comando en la terminal:

   ```bash
   zaproxy
   ```

### Paso 2: Configuración Inicial

1. **Configurar el Proxy**:
   - Una vez que ZAP esté abierto, ve a `Tools > Options`.
   - Selecciona `Local Proxy` en la lista de la izquierda.
   - Asegúrate de que la dirección esté configurada en `127.0.0.1` y el puerto en `8080`.
2. **Configurar el Navegador**:
   - Abre las configuraciones de red de tu navegador (por ejemplo, Firefox).
   - Configura el proxy manualmente para que apunte a `127.0.0.1` en el puerto `8080`.

### Paso 3: Explorar una Aplicación Web

1. **Iniciar la Sesión de Exploración**:
   - En ZAP, haz clic en el botón `Automated Scan` en la barra de herramientas.
   - Introduce la URL de la aplicación web que deseas explorar en el campo `URL to attack`.
2. **Ejecutar la Exploración**:
   - Haz clic en `Attack` para iniciar el escaneo automatizado.
   - ZAP comenzará a explorar la aplicación web, buscando y registrando todas las páginas y recursos accesibles.

### Paso 4: Analizar los Resultados

1. **Ver el Árbol de Sitios**:
   - A medida que ZAP explora la aplicación, verás que el `Site Tree` (Árbol de Sitios) en el panel izquierdo se va llenando con la estructura de la aplicación web.
   - Puedes hacer clic en cualquier nodo del árbol para ver más detalles sobre las solicitudes y respuestas.
2. **Revisar las Alertas**:
   - ZAP generará alertas para cualquier vulnerabilidad o problema de seguridad que encuentre.
   - Estas alertas aparecerán en el panel inferior bajo la pestaña `Alerts`.
   - Puedes hacer clic en cada alerta para ver detalles sobre la vulnerabilidad, incluyendo una descripción y posibles soluciones.

### Paso 5: Realizar un Ataque Manual

1. **Usar el Spidering**:
   - En el panel de `Quick Start`, selecciona `Spider`.
   - Introduce la URL de la aplicación web y haz clic en `Start Scan`.
   - ZAP recorrerá la aplicación web en busca de enlaces y formularios, añadiendo todos los recursos encontrados al `Site Tree`.
2. **Escanear por Vulnerabilidades**:
   - Una vez que el spidering haya terminado, selecciona un nodo en el `Site Tree` y haz clic derecho.
   - Selecciona `Attack > Active Scan`.
   - ZAP realizará un escaneo activo en la URL seleccionada, buscando vulnerabilidades comunes.

### Ejemplo Práctico

Para ilustrar el uso de ZAP, haremos una prueba contra una aplicación web de prueba conocida como "Juice Shop" (una aplicación web intencionalmente vulnerable):

1. **Desplegar Juice Shop**:

   - Puedes ejecutar Juice Shop en tu máquina local usando Docker:

     ```bash
     docker run --rm -p 3000:3000 bkimminich/juice-shop
     ```

   - La aplicación estará disponible en `http://localhost:3000`.

2. **Configurar ZAP para Juice Shop**:

   - Asegúrate de que tu navegador esté configurado para usar el proxy de ZAP (`127.0.0.1:8080`).
   - Abre `http://localhost:3000` en tu navegador y navega un poco por la aplicación para que ZAP capture las solicitudes.

3. **Realizar un Escaneo Automatizado**:

   - En ZAP, ve a `Quick Start > Automated Scan`.
   - Introduce `http://localhost:3000` como la URL a atacar.
   - Haz clic en `Attack` y espera a que ZAP complete el escaneo.

4. **Analizar los Resultados**:

   - Revisa el `Site Tree` para ver la estructura de la aplicación.
   - Examina las `Alerts` para identificar cualquier vulnerabilidad encontrada.
   - Realiza escaneos activos adicionales en áreas específicas de la aplicación para un análisis más profundo.

### Conclusión

OWASP ZAP es una herramienta poderosa y versátil para la prueba de seguridad de aplicaciones web. Siguiendo esta guía, puedes configurar y utilizar ZAP en Kali Linux para identificar y analizar vulnerabilidades en aplicaciones web de manera efectiva. Recuerda siempre realizar tus pruebas de seguridad de manera ética y con el permiso adecuado.


![Mesa de trabajo 1FOOTER](http://drive.google.com/uc?export=view&id=1vwLVsNlcF2PEyv9fULe2cohQnVfwRWLg)
