![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Configurar Burp Suite (Edición Comunitaria o Profesional)**

Burp Suite es una de las herramientas más utilizadas por profesionales de ciberseguridad y pentesters para realizar pruebas de penetración en aplicaciones web. A continuación, te explicaré cómo configurar Burp Suite desde cero en un entorno típico de pruebas. Este tutorial cubrirá tanto la configuración básica como la integración con un navegador.

------

## **Paso 1: Instalación de Burp Suite**

1. **Descarga Burp Suite**:

   - Ve a la página oficial de Burp Suite: https://portswigger.net/burp.
   - Descarga la edición que prefieras (Comunidad o Profesional). La versión Profesional es de pago, pero la versión Comunitaria tiene funciones básicas gratuitas.

2. **Instala Burp Suite**:

   - Para Windows: Ejecuta el archivo `.exe` descargado y sigue las instrucciones del asistente de instalación.

   - Para Linux:

     ```
     bashCopiarchmod +x burpsuite_community_linux_vX.X.X.sh
     ./burpsuite_community_linux_vX.X.X.sh
     ```

   - Para macOS: Arrastra Burp Suite a la carpeta de aplicaciones.

------

## **Paso 2: Configuración del Proxy de Burp Suite**

Burp Suite intercepta el tráfico web utilizando su proxy. Para lograr esto, debes configurarlo en el navegador.

1. **Inicia Burp Suite**:

   - Abre Burp Suite y selecciona la opción `Temporary Project` (Proyecto temporal).
   - Haz clic en `Next` y selecciona la configuración por defecto (`Use Burp defaults`).
   - Haz clic en `Start Burp` para abrir la interfaz principal.

2. **Verifica la configuración del Proxy**:

   - En Burp Suite, ve a la pestaña `Proxy` > `Options`.

   - Asegúrate de que el proxy esté habilitado y configurado para escuchar en la dirección 

     ```
     127.0.0.1
     ```

      y el puerto 

     ```
     8080
     ```

      (estos son los valores predeterminados, pero puedes cambiarlos si es necesario).

     - Si deseas cambiar el puerto, haz clic en el botón `Edit` y especifica un nuevo puerto.

------

## **Paso 3: Configuración del Navegador para Usar Burp Suite**

Para que Burp Suite pueda interceptar el tráfico HTTP/HTTPS, debes configurar tu navegador para que use el proxy de Burp.

1. **Recomendación: Usa un navegador dedicado**:
   - Descarga y utiliza un navegador específico para tus pruebas, como Firefox Developer Edition.
   - Esto evita interferencias con tu tráfico normal.
2. **Configura el Proxy del navegador**:
   - Ve a las opciones de configuración de red del navegador:
     - En Firefox: `Menú > Configuración > General > Configuración de Red > Configuración manual del proxy`.
     - En Chrome: Utiliza extensiones como [FoxyProxy](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/) para manejar configuraciones de proxy.
   - Introduce los siguientes valores:
     - **Proxy HTTP**: `127.0.0.1`
     - **Puerto**: `8080`
     - Asegúrate de que la opción "Usar este proxy para todos los protocolos" esté seleccionada.
3. **Prueba la conexión**:
   - Abre el navegador configurado y visita cualquier sitio web (por ejemplo, `http://example.com`).
   - Si Burp Suite está correctamente configurado, el tráfico será interceptado y aparecerá en la pestaña `HTTP history` de Burp Suite.

------

## **Paso 4: Configuración del Certificado HTTPS de Burp Suite**

Para interceptar tráfico HTTPS, debes instalar el certificado SSL generado por Burp Suite en tu navegador. Esto asegura que las conexiones cifradas puedan ser analizadas.

1. **Descarga el Certificado de Burp Suite**:

   - Ve a la pestaña `Proxy` > `Intercept` en Burp Suite.
   - Asegúrate de que la opción de interceptación esté desactivada (`Intercept is off`).
   - En tu navegador, visita: http://burp (asegúrate de que tu navegador esté configurado para usar el proxy de Burp).
   - Haz clic en `CA Certificate` para descargar el certificado (`cacert.der`).

2. **Instala el Certificado en tu Navegador**:

   - Firefox

     :

     - Ve a `Menú > Configuración > Privacidad & Seguridad > Certificados > Ver Certificados > Importar`.
     - Selecciona el archivo `cacert.der` descargado.
     - Marca la opción "Confiar en este certificado para identificar sitios web" y haz clic en `Aceptar`.

   - Chrome

      (o navegadores basados en Chromium como Brave):

     - Ve a `chrome://settings/` > `Seguridad y Privacidad` > `Administrar certificados`.
     - Importa el certificado y asegúrate de confiar en él para conexiones HTTPS.

3. **Verifica que HTTPS funcione**:

   - Ve a cualquier sitio web HTTPS (por ejemplo, `https://example.com`).

   - Si el tráfico aparece en Burp Suite y no hay errores en el navegador, la configuración es correcta.

     

## **Paso 5: Habilitar Interceptación y Usar Burp Suite**

1. **Interceptar tráfico**:
   - Ve a la pestaña `Proxy` > `Intercept` y habilita la interceptación (`Intercept is on`).
   - Al navegar por cualquier sitio web, las solicitudes serán interceptadas y podrás analizarlas antes de enviarlas al servidor.
2. **Explora otras herramientas de Burp Suite**:
   - Usa la pestaña `HTTP history` para revisar todas las solicitudes y respuestas.
   - Utiliza `Repeater`, `Intruder` y `Scanner` (en la versión profesional) para profundizar en las pruebas.

------

## **Paso 6: Configuración Adicional (Opcional)**

1. **Guardar un proyecto**:
   - En la versión Profesional de Burp Suite, puedes guardar tu proyecto para continuar tus pruebas más tarde.
2. **Optimizar las opciones de Proxy**:
   - Configura reglas específicas para interceptar o ignorar ciertos tipos de tráfico en la pestaña `Proxy` > `Options`.
3. **Configura Burp Suite con otras herramientas**:
   - Integra Burp con extensiones como `Autorize` (disponible en la BApp Store) para ampliar sus funcionalidades.