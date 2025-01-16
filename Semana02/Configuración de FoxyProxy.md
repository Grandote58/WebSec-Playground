![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Configuración de FoxyProxy**

FoxyProxy es una extensión muy útil para gestionar configuraciones de proxy en navegadores como Firefox y Chrome. Facilita cambiar rápidamente entre proxies y configurarlos, lo que resulta especialmente útil para pentesters que trabajan con herramientas como **Burp Suite**, **OWASP ZAP**, o cualquier proxy que se desee configurar.

En este tutorial te explicaré cómo instalar y configurar FoxyProxy para que funcione correctamente con un proxy como Burp Suite u OWASP ZAP.

## **Paso 1: Instalación de FoxyProxy**

1. **Instalar FoxyProxy en Firefox**:
   - Abre Firefox y ve a la página de complementos de Mozilla: [FoxyProxy Standard para Firefox](https://addons.mozilla.org/es/firefox/addon/foxyproxy-standard/).
   - Haz clic en el botón **Añadir a Firefox**.
   - Haz clic en **Agregar** cuando aparezca el cuadro de confirmación.
   - FoxyProxy ahora estará instalado. Verás un ícono de zorro en la barra de herramientas del navegador.
2. **Instalar FoxyProxy en Chrome**:
   - Abre Chrome y accede a la Chrome Web Store: FoxyProxy para Chrome.
   - Haz clic en **Añadir a Chrome** y luego en **Añadir extensión**.
   - Una vez instalado, aparecerá un ícono de zorro azul en la esquina superior derecha del navegador.

## **Paso 2: Configurar el proxy en FoxyProxy**

Para usar FoxyProxy con herramientas como Burp Suite u OWASP ZAP, necesitas configurar el proxy local (por ejemplo, `127.0.0.1:8080`).

1. **Abrir la configuración de FoxyProxy**:

   - Haz clic en el ícono de FoxyProxy (zorro azul) en la barra de herramientas del navegador.
   - Selecciona la opción **Options** (Opciones).

2. **Añadir un nuevo proxy**:

   - En la página de configuración de FoxyProxy, haz clic en **Add New Proxy** (Agregar nuevo proxy).
   - Aparecerá un formulario para configurar los detalles del proxy.

3. **Configurar los detalles del proxy**:

   - Completa los campos de la siguiente manera:

     - **Title** (Título): Escribe un nombre para identificar este proxy. Por ejemplo: `Burp Suite` o `OWASP ZAP`.

     - **Proxy Type**: Selecciona `HTTP` o `HTTPS`, dependiendo de lo que estés usando.

     - **Proxy IP address or DNS name**: Ingresa la dirección IP del proxy. Normalmente es `127.0.0.1` (la dirección de loopback local).

     - Port

        (Puerto): Escribe el puerto en el que tu proxy está escuchando. Por defecto:

       - Burp Suite usa `8080`.
       - OWASP ZAP usa `8080` (a menos que lo hayas cambiado).

     - **No Proxy for**: Aquí puedes agregar dominios o direcciones que quieras excluir del proxy. Por ejemplo, `localhost` o `127.0.0.1` para evitar que el tráfico hacia tu propio sistema pase por el proxy.

   - Haz clic en **Save** (Guardar).

4. **Verifica la lista de proxies**:

   - Regresa a la lista de proxies en la página de opciones de FoxyProxy.
   - Verás el proxy que acabas de agregar. Asegúrate de que esté configurado correctamente.

## **Paso 3: Activar el proxy en FoxyProxy**

Ahora que el proxy está configurado, necesitas activarlo para que el tráfico del navegador pase a través de él.

1. Haz clic en el ícono de FoxyProxy en la barra de herramientas.
2. Selecciona el proxy que configuraste (por ejemplo, Burp Suite).
   - FoxyProxy cambiará automáticamente la configuración de red del navegador para usar el proxy seleccionado.

## **Paso 4: Probar la conexión del proxy**

1. **Abrir la herramienta del proxy**:
   - Asegúrate de que la herramienta del proxy (como Burp Suite o OWASP ZAP) esté abierta y ejecutándose.
   - Verifica que esté configurada para escuchar en `127.0.0.1` y el puerto `8080` (o el puerto que configuraste en FoxyProxy).
2. **Probar en el navegador**:
   - Abre tu navegador y visita cualquier sitio web (por ejemplo, `http://example.com`).
   - Si todo está configurado correctamente, el tráfico aparecerá en tu herramienta de proxy.
   - Por ejemplo:
     - En Burp Suite, el tráfico debería aparecer en la pestaña `HTTP History` (Historial HTTP).
     - En OWASP ZAP, aparecerá en la pestaña `History` (Historial).

------

## **Paso 5: Configurar el Certificado SSL para tráfico HTTPS (opcional pero recomendado)**

Si necesitas interceptar tráfico HTTPS (cifrado), debes instalar el certificado SSL generado por tu proxy en el navegador.

1. **Obtener el certificado del proxy**:

   - Burp Suite:
     - Ve a `http://burp` en tu navegador (con el proxy activado).
     - Descarga el certificado haciendo clic en `CA Certificate`.
   - OWASP ZAP:
     - Ve a `Tools > Options > Dynamic SSL Certificates`.
     - Haz clic en `Save` para exportar el certificado (`owasp_zap_root_ca.cer`).

2. **Instalar el certificado en tu navegador**:

   - Firefox:

     1. Ve a `Menú > Configuración > Privacidad y seguridad > Certificados > Ver Certificados > Importar`.
     2. Selecciona el archivo del certificado descargado.
     3. Marca la opción "Confiar en este certificado para identificar sitios web".

   - Chrome

     :

     1. Ve a `chrome://settings/` > `Seguridad y Privacidad > Administrar certificados`.
     2. Importa el certificado y confía en él para conexiones HTTPS.

## **Paso 6: Alternar rápidamente entre configuraciones de proxy**

Una de las mejores características de FoxyProxy es la capacidad de cambiar fácilmente entre configuraciones de proxy.

1. Haz clic en el ícono de FoxyProxy.
2. Selecciona:
   - **Disable FoxyProxy**: Para deshabilitar el uso de cualquier proxy y volver a la configuración predeterminada del navegador.
   - **Use System Proxy Settings**: Para usar la configuración de proxy predeterminada del sistema operativo.
   - **Nombre de tu proxy configurado**: Para activar un proxy específico.

## **Paso 7: Opciones avanzadas en FoxyProxy (opcional)**

Si necesitas configuraciones avanzadas, puedes explorar las siguientes opciones en la interfaz de FoxyProxy:

1. **Patrones de URL (URL Patterns)**:
   - Define patrones para que solo ciertas URLs pasen a través del proxy.
   - Por ejemplo:
     - Si quieres que solo el tráfico hacia `example.com` pase por el proxy, puedes agregar un patrón como: `*example.com*`.
2. **Autenticación de proxy**:
   - Si el proxy requiere autenticación (usuario/contraseña), puedes configurar estas credenciales en la misma ventana donde configuraste el proxy.
3. **Exportar/importar configuraciones**:
   - FoxyProxy permite exportar configuraciones a un archivo `.json` e importarlas en otro navegador o sistema.

## **Paso 8: Buenas prácticas al usar FoxyProxy**

1. **Pruebas éticas**:
   - Asegúrate de tener permiso para realizar pruebas de penetración en cualquier aplicación web o sistema.
   - No interceptes tráfico que no estés autorizado a analizar.
2. **Deshabilitar el proxy cuando no lo uses**:
   - Recuerda desactivar FoxyProxy cuando no estés realizando pruebas, especialmente si estás navegando sitios que no quieres analizar.
3. **Mantén el navegador seguro**:
   - Utiliza un navegador dedicado (como Firefox Developer Edition) solo para realizar pruebas. Esto te ayudará a mantener tu navegación personal separada de tus pruebas de pentesting.