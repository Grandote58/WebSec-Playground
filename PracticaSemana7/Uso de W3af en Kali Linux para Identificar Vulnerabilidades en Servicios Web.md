# Uso de **W3af** en Kali Linux para Identificar Vulnerabilidades en Servicios Web

W3af es una herramienta poderosa para realizar auditorías de seguridad en aplicaciones web y descubrir vulnerabilidades. A continuación, te guiaré desde la instalación hasta la puesta en marcha de W3af, con ejemplos de comandos útiles para identificar vulnerabilidades en servicios web.

------

### **1. Instalación de W3af en Kali Linux**

**Paso 1:** Actualizar el sistema Antes de instalar cualquier herramienta, es importante asegurarse de que tu sistema esté actualizado. Abre la terminal y ejecuta:

```bash
sudo apt update && sudo apt upgrade
```

**Paso 2:** Instalar W3af En Kali Linux, W3af ya debería estar disponible en los repositorios. Si no lo tienes instalado, puedes hacerlo con el siguiente comando:

```bash
sudo apt install w3af
```

**Paso 3:** Verificar la instalación Para verificar que W3af está correctamente instalado, ejecuta el siguiente comando:

```bash
w3af_console
```

Esto abrirá la consola de W3af, lo que te permitirá empezar a utilizar la herramienta.

------

### **2. Comandos útiles para W3af**

Ahora que W3af está instalado, te muestro 5 o 6 comandos clave que te permitirán usar la herramienta para auditar vulnerabilidades en servicios web.

#### **Paso 1:** Configurar la URL objetivo

Lo primero que debemos hacer es definir el sitio web objetivo que vamos a analizar. Esto se hace utilizando el comando `target`:

```bash
target set target https://example.com
```

Este comando establece la URL del servicio web que vamos a auditar. Reemplaza `https://example.com` por el sitio que desees auditar.

#### **Paso 2:** Ver los plugins disponibles

W3af tiene una amplia gama de plugins para escanear diferentes vulnerabilidades. Puedes ver una lista de los plugins disponibles con el siguiente comando:

```bash
plugins
```

Esto mostrará las diferentes categorías de plugins (audit, bruteforce, discovery, etc.) que puedes activar para realizar auditorías específicas.

#### **Paso 3:** Activar plugins de auditoría

Para realizar una auditoría básica de vulnerabilidades en servicios web, puedes activar los siguientes plugins:

```bash
plugins audit sql_injection
plugins audit xss
```

Estos comandos activan los plugins para buscar **inyecciones SQL** y **Cross-Site Scripting (XSS)**, dos de las vulnerabilidades más comunes en aplicaciones web.

#### **Paso 4:** Realizar el escaneo

Una vez configurados los plugins y el objetivo, puedes iniciar el escaneo ejecutando:

```bash
start
```

Esto comenzará el análisis de vulnerabilidades en el sitio web objetivo. W3af comenzará a enviar solicitudes y analizar las respuestas en busca de vulnerabilidades.

#### **Paso 5:** Ver los resultados

Una vez que finaliza el escaneo, puedes ver los resultados con el siguiente comando:

```bash
results
```

Este comando mostrará una lista de las vulnerabilidades encontradas durante el análisis, junto con detalles específicos de cada una.

#### **Paso 6:** Guardar el reporte

Para exportar los resultados del escaneo a un archivo, puedes usar el siguiente comando:

```bash
output console, html_file
output config html_file output_file = /ruta/donde/guardar/reporte.html
```

Este comando guardará el reporte en formato HTML para revisarlo en el navegador. Asegúrate de especificar la ruta de archivo donde deseas guardar el reporte.

------

### **3. Ejemplo Completo**

Para hacer una auditoría básica de un sitio web en busca de inyecciones SQL y vulnerabilidades XSS, sigue estos pasos en la consola de W3af:

1. **Establecer el objetivo:**

   ```bash
   target set target https://example.com
   ```

2. **Ver plugins disponibles:**

   ```bash
   plugins
   ```

3. **Activar plugins para SQL Injection y XSS:**

   ```bash
   plugins audit sql_injection
   plugins audit xss
   ```

4. **Iniciar el escaneo:**

   ```bash
   start
   ```

5. **Ver los resultados:**

   ```bash
   results
   ```

6. **Guardar los resultados en un archivo HTML:**

   ```bash
   output console, html_file
   output config html_file output_file = /ruta/donde/guardar/reporte.html
   ```

------

### **4. Conclusión**

Esta práctica guiada te permite utilizar **W3af** para auditar un servicio web y buscar vulnerabilidades comunes como inyecciones SQL y Cross-Site Scripting (XSS). W3af es una herramienta versátil que puede ampliar tu capacidad de realizar pruebas de penetración en aplicaciones web mediante una amplia variedad de plugins, y te ayudará a identificar y mitigar problemas de seguridad en entornos reales.