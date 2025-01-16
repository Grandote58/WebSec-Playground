![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Prácticas con Burp Suite**

## **Práctica 1: Identificación de parámetros vulnerables a XSS (Cross-Site Scripting)**

**Contexto**: La empresa ficticia **CyberShop Inc.**, un minorista en línea, contrató tus servicios para evaluar la seguridad de su portal de comercio electrónico, alojado en `http://testshop.cybershopinc.com`.

1. **Preparación**:

   - Abre **Kali Linux**.
   - Lanza **Burp Suite** desde el menú de aplicaciones (`Applications > Web Application Analysis > Burp Suite`).
   - Configura el navegador Firefox para usar Burp Suite como proxy (IP `127.0.0.1`, puerto `8080`).
   - Asegúrate de que el certificado SSL de Burp esté instalado en Firefox para interceptar tráfico HTTPS.

2. **Captura de tráfico HTTP**:

   - Visita `http://testshop.cybershopinc.com` en Firefox.
   - Navega por el sitio, añadiendo productos al carrito y utilizando funciones como la búsqueda.

3. **Identificar parámetros en solicitudes HTTP**:

   - Ve a la pestaña `Proxy > HTTP history` en Burp Suite.
   - Examina las solicitudes GET y POST para encontrar parámetros como `search`, `product_id`, etc.

4. **Prueba de vulnerabilidades XSS**:

   - Selecciona una solicitud que contenga un parámetro como `search` (por ejemplo: `/search?query=...`).

   - Envíala al **Repeater** haciendo clic derecho en la solicitud y seleccionando `Send to Repeater`.

   - En la pestaña 

     ```css
     Repeater
     ```

     , prueba inyectar un payload de XSS en el parámetro 

     ```css
     search
     ```

     , como:

     ```html
     <script>alert('XSS')</script>
     ```

   - Haz clic en `Send` y revisa la respuesta del servidor en busca de reflejos del script inyectado en la página.

5. **Resultados**:

   - Si el payload ejecuta el código JavaScript en el navegador, el sitio es vulnerable a XSS.
   - Documenta la solicitud y la respuesta en tu informe.

## **Práctica 2: Fuerza bruta de login utilizando el módulo Intruder**

**Contexto**: **BankSecure Ltd.**, un banco ficticio, quiere evaluar la seguridad de su página de inicio de sesión en `http://test.banksecure.com`.

1. **Preparación**:
   - Configura Burp Suite como en la práctica anterior.
   - Accede al portal de inicio de sesión en `http://test.banksecure.com/login`.
2. **Intercepción de solicitudes**:
   - Ingresa credenciales ficticias (por ejemplo, `admin:test`) y haz clic en "Iniciar sesión".
   - En Burp Suite, detén la solicitud en la pestaña `Proxy > Intercept`.
3. **Configuración de Intruder**:
   - Haz clic derecho en la solicitud y selecciona `Send to Intruder`.
   - Ve a la pestaña `Intruder > Positions`.
   - Marca los campos de usuario (`username`) y contraseña (`password`) como posiciones a atacar.
4. **Cargar listas de diccionario**:
   - Ve a la pestaña `Payloads`.
   - En `Payload Set 1`, carga una lista de nombres de usuario desde Kali Linux (`/usr/share/wordlists/rockyou.txt`).
   - En `Payload Set 2`, carga una lista de contraseñas desde la misma ubicación.
5. **Ejecutar el ataque**:
   - Haz clic en `Start Attack`.
   - Analiza las respuestas en busca de indicios de acceso exitoso (por ejemplo, códigos de estado HTTP diferentes, como `200 OK` o un cambio en el tamaño de la respuesta).
6. **Resultados**:
   - Si encuentras credenciales válidas, documenta tu hallazgo y proporciona recomendaciones para implementar contramedidas como bloqueo de IP tras intentos fallidos.

# **Prácticas con OWASP ZAP**

## **Práctica 3: Spidering y escaneo pasivo**

**Contexto**: **HealthApp Corp.**, una startup de tecnología médica, desea asegurarse de que no hay vulnerabilidades básicas en su portal en `http://test.healthappcorp.com`.

1. **Preparación**:
   - Abre OWASP ZAP desde el menú de Kali Linux (`Applications > Web Application Analysis > OWASP ZAP`).
   - Configura Firefox para usar OWASP ZAP como proxy (IP `127.0.0.1`, puerto `8080`).
2. **Ejecutar el Spidering**:
   - Ingresa `http://test.healthappcorp.com` en el campo de URL de OWASP ZAP.
   - Haz clic en el botón `Spider` en la barra de herramientas.
   - Confirma las opciones predeterminadas y deja que el Spider explore automáticamente el sitio.
3. **Análisis del tráfico**:
   - En la pestaña `Alerts`, revisa las vulnerabilidades detectadas durante el escaneo pasivo.
   - Identifica problemas como encabezados de seguridad faltantes o cookies no seguras.

## **Práctica 4: Escaneo activo de vulnerabilidades**

**Contexto**: **EduTech Ltd.**, una empresa de e-learning, busca evaluar posibles vulnerabilidades críticas en su plataforma en `http://test.edutech.com`.

1. **Preparación**:
   - Configura OWASP ZAP como en la práctica anterior.
   - Ingresa al sitio objetivo en el navegador para capturar tráfico.
2. **Escaneo activo**:
   - En el árbol del sitio (panel izquierdo), haz clic derecho en el dominio `http://test.edutech.com`.
   - Selecciona `Attack > Active Scan`.
   - Ajusta las configuraciones del escaneo si es necesario y haz clic en `Start Scan`.
3. **Resultados**:
   - Observa las alertas generadas en la pestaña `Alerts`.
   - Busca vulnerabilidades críticas como inyección SQL, XSS o fallos de autenticación.

# **Prácticas combinadas en Kali Linux**

## **Práctica 5: Comparación de resultados entre Burp Suite y OWASP ZAP**

**Contexto**: **GreenFinance Inc.**, un banco ficticio, contrató tus servicios para comparar herramientas de escaneo en su sitio en `http://test.greenfinance.com`.

1. **Usa OWASP ZAP** para ejecutar un **Spidering** y un **Escaneo Activo**:
   - Configura OWASP ZAP como en la práctica 3 y 4.
   - Genera un informe (`Report > Generate Report`) con las vulnerabilidades encontradas.
2. **Usa Burp Suite** para identificar problemas manualmente:
   - Captura tráfico con Burp Suite y usa módulos como **Repeater** o **Intruder** para explorar vulnerabilidades específicas.
3. **Comparación**:
   - Analiza las diferencias entre los hallazgos de OWASP ZAP (automatizado) y Burp Suite (manual).
   - Documenta cuáles vulnerabilidades se detectaron en cada herramienta.

## **Práctica 6: Automación de pruebas con Burp y OWASP ZAP**

**Contexto**: **TechLabs**, una empresa ficticia de desarrollo web, desea automatizar pruebas de vulnerabilidades en su dominio `http://test.techlabs.com`.

1. **Usa Burp Suite con Extensiones**:

   - Instala la extensión **Logger++** desde la tienda de extensiones de Burp.
   - Configura reglas para registrar automáticamente patrones de tráfico.

2. **Usa OWASP ZAP en modo línea de comandos**:

   - Ejecuta el siguiente comando en Kali Linux:

     ```bash
     zap-cli quick-scan --self-contained --start-options="-config api.disablekey=true" http://test.techlabs.com
     ```

   - Este comando realiza un escaneo rápido del sitio.

3. **Comparación de rendimiento**:

   - Evalúa qué herramienta fue más rápida y precisa para identificar vulnerabilidades.