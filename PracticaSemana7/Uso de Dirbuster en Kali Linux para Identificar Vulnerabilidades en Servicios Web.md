# Uso de **Dirbuster** en Kali Linux para Identificar Vulnerabilidades en Servicios Web

**Dirbuster** es una herramienta de fuerza bruta diseñada para descubrir directorios y archivos ocultos en servidores web. Este tipo de ataque es crucial en la fase de **reconocimiento** durante una prueba de penetración, ya que los directorios o archivos no listados a menudo contienen información valiosa o acceso a áreas no protegidas.

Aquí te mostraré cómo instalar y usar **Dirbuster**, junto con algunos comandos clave para realizar auditorías en servicios web.

------

### **1. Instalación de Dirbuster en Kali Linux**

En la mayoría de los casos, **Dirbuster** ya está preinstalado en Kali Linux. Si no lo tienes instalado, sigue estos pasos.

#### Paso 1: Actualizar el sistema

Como siempre, asegúrate de que tu sistema esté actualizado antes de instalar cualquier herramienta:

```bash
sudo apt update && sudo apt upgrade
```

#### Paso 2: Instalar Dirbuster

Si no tienes Dirbuster instalado, puedes instalarlo ejecutando el siguiente comando:

```bash
sudo apt install dirbuster
```

#### Paso 3: Verificar la instalación

Para asegurarte de que Dirbuster se ha instalado correctamente, simplemente escribe el comando:

```bash
dirbuster
```

Esto abrirá la interfaz gráfica de Dirbuster.

------

### **2. Ejecución de Dirbuster**

#### Paso 1: Abrir Dirbuster

Ejecuta Dirbuster en la terminal:

```bash
dirbuster
```

Se abrirá la interfaz gráfica. Aquí podrás configurar los parámetros para comenzar el escaneo de directorios ocultos en el servicio web que desees analizar.

#### Paso 2: Configurar el objetivo

- En la interfaz de Dirbuster, introduce la **URL del sitio web** que deseas escanear en el campo de "Target URL".
- Por ejemplo, si el objetivo es "https://example.com", introduce:

```bash
https://example.com
```

#### Paso 3: Seleccionar la lista de palabras (Wordlist)

Dirbuster realiza un ataque de fuerza bruta utilizando diccionarios de palabras (wordlists) para probar posibles nombres de directorios y archivos.

- Selecciona una lista de palabras en **"File with list of directories/files"**. Kali Linux incluye varias listas de palabras que puedes encontrar en el directorio:

```bash
/usr/share/wordlists/dirbuster/
```

Puedes elegir una lista de palabras como **directory-list-2.3-medium.txt**:

```bash
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

#### Paso 4: Configurar opciones adicionales

- **Recursividad:** Marca la casilla **Recursive** para que Dirbuster escanee subdirectorios encontrados.
- **Extensiones:** Puedes agregar extensiones comunes como **.php**, **.html**, **.txt** en la sección **"File Extension"**.

#### Paso 5: Iniciar el escaneo

Haz clic en **"Start"** para comenzar el escaneo. Dirbuster empezará a enviar solicitudes al servidor web e intentará encontrar directorios y archivos no listados.

------

### **3. Uso de Dirbuster con la línea de comandos**

Si prefieres usar Dirbuster desde la línea de comandos, aquí tienes algunos comandos útiles:

#### **Comando 1: Escaneo básico**

Este comando realiza un escaneo básico de directorios en el sitio objetivo utilizando una lista de palabras predeterminada:

```bash
dirbuster -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -o resultados.txt
```

- `-u`: La URL del sitio objetivo.
- `-w`: La ruta a la lista de palabras que se va a usar.
- `-o`: Archivo de salida donde se almacenarán los resultados.

#### **Comando 2: Escaneo con extensiones específicas**

Si estás buscando archivos específicos con ciertas extensiones (por ejemplo, `.php`, `.html`), usa el siguiente comando:

```bash
dirbuster -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e php,html -o resultados.txt
```

- `-e`: Especifica las extensiones de archivo que quieres buscar.

#### **Comando 3: Escaneo recursivo**

Si deseas que Dirbuster siga escaneando subdirectorios encontrados durante el escaneo inicial, puedes activar el escaneo recursivo con la opción `-r`:

```bash
dirbuster -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r -o resultados.txt
```

- `-r`: Activa la búsqueda recursiva.

#### **Comando 4: Limitar el número de conexiones**

Puedes ajustar el número de conexiones simultáneas que Dirbuster utiliza para evitar saturar el servidor objetivo:

```bash
dirbuster -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 10 -o resultados.txt
```

- `-t`: Especifica el número de hilos (conexiones simultáneas) que se utilizarán. Aquí, hemos configurado 10 hilos.

#### **Comando 5: Buscar archivos con extensión específica**

Para buscar únicamente archivos con una extensión en particular, puedes especificar la extensión de archivo que deseas detectar:

```bash
dirbuster -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e txt -o resultados.txt
```

------

### **4. Visualización de Resultados**

Una vez finalizado el escaneo, Dirbuster te mostrará una lista de directorios y archivos descubiertos. Puedes revisar los resultados y analizar qué archivos y directorios podrían representar vulnerabilidades.

1. **Archivos sensibles:** Busca archivos como `config.php`, `backup.zip`, o `admin.html`, que podrían contener información sensible o proporcionar acceso a áreas críticas.
2. **Directorios expuestos:** Directorios como `/admin/`, `/backup/`, o `/test/` podrían contener funcionalidades no protegidas o información de prueba que podrían ser explotadas.

------

### **5. Ejemplo Completo en la Línea de Comandos**

Para realizar un escaneo básico en busca de directorios ocultos en un sitio web objetivo, podrías utilizar el siguiente comando:

```bash
dirbuster -u https://example.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e php,html,txt -r -t 20 -o resultados.txt
```

- Este comando realiza un escaneo recursivo de **https://example.com** usando una lista de palabras de tamaño medio.
- Busca archivos con extensiones **.php**, **.html**, y **.txt**.
- Utiliza 20 hilos para realizar el escaneo más rápidamente.

------

### **6. Conclusión**

**Dirbuster** es una excelente herramienta para realizar descubrimiento de directorios y archivos ocultos en servidores web, lo que te permite identificar puntos de entrada adicionales o archivos vulnerables que puedan no estar listados públicamente. Esta práctica guiada te proporciona una introducción al uso de Dirbuster, tanto en su versión gráfica como en línea de comandos, para que puedas usarla en tus pruebas de penetración.