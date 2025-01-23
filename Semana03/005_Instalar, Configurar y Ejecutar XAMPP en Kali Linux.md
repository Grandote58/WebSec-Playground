![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Instalar, Configurar y Ejecutar XAMPP en Kali Linux**

## **1. Descarga XAMPP**

XAMPP se puede descargar directamente desde su página oficial:

1. Abre un navegador y ve a:
   https://www.apachefriends.org/index.html.

2. Descarga la versión más reciente de XAMPP para Linux. Por ejemplo, para la versión **8.2.12-0** de XAMPP, el archivo será algo como:

   ```bash
   xampp-linux-x64-8.2.12-0-installer.run
   ```

3. Alternativamente, puedes usar `wget` para descargar directamente el archivo desde la terminal:

   ```bash
   wget https://downloadsapachefriends.global.ssl.fastly.net/xampp-files/8.2.12/xampp-linux-x64-8.2.12-0-installer.run
   ```

------

## **2. Otorga permisos de ejecución al instalador**

El archivo descargado no tendrá permisos de ejecución por defecto. Cambia los permisos con:

```bash
chmod +x xampp-linux-x64-8.2.12-0-installer.run
```

------

## **3. Ejecuta el instalador**

Ejecuta el instalador de XAMPP con permisos de superusuario:

```bash
sudo ./xampp-linux-x64-8.2.12-0-installer.run
```

Esto abrirá una interfaz gráfica. Sigue estos pasos:

1. Haz clic en **Next**.
2. Deja seleccionados los componentes predeterminados: **Apache**, **MySQL (MariaDB)** y **PHP**.
3. Haz clic en **Next** y luego en **Finish** para completar la instalación.

Por defecto, XAMPP se instalará en el directorio `/opt/lampp`.

------

## **4. Iniciar XAMPP**

Una vez instalado, inicia XAMPP con el siguiente comando:

```bash
sudo /opt/lampp/lampp start
```

Esto iniciará los servicios de Apache, MySQL y otros componentes de XAMPP. Si todo está correcto, deberías ver algo como:

```bash
Starting XAMPP for Linux 8.2.12-0...
XAMPP: Starting Apache...ok.
XAMPP: Starting MySQL...ok.
XAMPP: Starting ProFTPD...ok.
```

------

## **5. Verifica que XAMPP está funcionando**

Abre tu navegador y visita:

```bash
http://localhost
```

Deberías ver la página de inicio de XAMPP, confirmando que el servidor Apache está funcionando.

------

## **6. Configura permisos para todos los usuarios**

El directorio raíz de XAMPP donde se almacenan los archivos servidos es:

```bash
/opt/lampp/htdocs
```

Por defecto, este directorio solo puede ser administrado por el usuario `root`. Si deseas permitir que cualquier usuario pueda leer, escribir y modificar archivos en este directorio, sigue estos pasos:

### **Paso 1: Cambia los permisos del directorio**

Otorga permisos de lectura, escritura y ejecución a todos los usuarios:

```bash
sudo chmod -R 777 /opt/lampp/htdocs
```

### **Paso 2: Cambia el propietario (opcional)**

Si deseas asignar un propietario diferente al directorio (por ejemplo, tu usuario actual), usa:

```bash
sudo chown -R $USER:$USER /opt/lampp/htdocs
```

Esto permitirá a tu usuario administrar los archivos sin necesidad de usar `sudo`.

### **Paso 3: Crea un archivo de prueba**

Verifica que los permisos están funcionando creando un archivo de prueba en el directorio raíz de XAMPP:

```bash
echo "<h1>¡XAMPP está funcionando correctamente!</h1>" > /opt/lampp/htdocs/index.html
```

Abre tu navegador y visita:

```bash
http://localhost
```

Deberías ver el mensaje **"¡XAMPP está funcionando correctamente!"**.

------

## **7. Comandos básicos para gestionar XAMPP**

Aquí están los comandos más comunes para gestionar XAMPP en Linux:

### Iniciar XAMPP:

```bash
sudo /opt/lampp/lampp start
```

### Detener XAMPP:

```bash
sudo /opt/lampp/lampp stop
```

### Reiniciar XAMPP:

```bash
sudo /opt/lampp/lampp restart
```

### Ver el estado de los servicios:

```bash
sudo /opt/lampp/lampp status
```

------

## **8. Configuración adicional (cambiar puerto de Apache)**

Si Apache no inicia porque el puerto 80 está en uso por otro servicio, como Nginx o un servidor en conflicto, puedes cambiar el puerto predeterminado.

### **Paso 1: Edita el archivo de configuración de Apache**

Abre el archivo de configuración principal de Apache:

```bash
sudo nano /opt/lampp/etc/httpd.conf
```

Busca la línea que dice:

```bash
Listen 80
```

Cámbiala por otro puerto, como el `8080`:

```bash
Listen 8080
```

### **Paso 2: Cambia la configuración del host virtual**

Busca la sección `<VirtualHost>` y actualiza el puerto, por ejemplo:

```bash
<VirtualHost *:8080>
    DocumentRoot "/opt/lampp/htdocs"
    ServerName localhost
</VirtualHost>
```

### **Paso 3: Reinicia XAMPP**

Reinicia XAMPP para aplicar los cambios:

```bash
sudo /opt/lampp/lampp restart
```

Ahora puedes acceder a Apache desde el nuevo puerto:

```bash
http://localhost:8080
```

------

## **9. Solución de problemas comunes**

### Apache no inicia

1. Revisa si otro servicio está usando el puerto 80:

   ```bash
   sudo netstat -tuln | grep :80
   ```

2. Si otro servicio está ocupando el puerto, detén ese servicio o cambia el puerto de Apache como se indicó anteriormente.



