![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Instalar, configurar y ejecutar el servidor Apache en Kali Linux**

## **1. Actualiza tu sistema**

Antes de instalar Apache, actualiza los repositorios de tu sistema para garantizar que tengas las últimas versiones disponibles:

```bash
sudo apt update && sudo apt upgrade -y
```

------

## **2. Instalar Apache**

Apache está disponible en los repositorios oficiales de Kali Linux. Instálalo con el siguiente comando:

```bash
sudo apt install apache2 -y
```

Esto instalará Apache y sus dependencias necesarias.

------

## **3. Verifica el estado del servicio**

Después de la instalación, verifica si Apache está funcionando correctamente con:

```bash
sudo systemctl status apache2
```

Deberías ver algo como esto:

```bash
   apache2.service - The Apache HTTP Server
   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
   Active: active (running) since [fecha y hora]
```

Si el servicio no está activo, puedes iniciarlo con:

```bash
sudo systemctl start apache2
```

------

## **4. Configurar Apache**

### **4.1 Cambiar los permisos del directorio raíz**

El directorio raíz de Apache, donde se almacenan los archivos servidos, es por defecto:

```bash
/var/www/html
```

Si deseas que todos los usuarios puedan leer, escribir y modificar archivos en este directorio, ajusta los permisos de la siguiente manera:

1. Otorga permisos de lectura, escritura y ejecución para todos los usuarios:

   ```bash
   sudo chmod -R 777 /var/www/html
   ```

2. (Opcional) Cambia el propietario del directorio a un usuario específico, como tu usuario actual:

   ```bash
   sudo chown -R $USER:$USER /var/www/html
   ```

   Esto facilita trabajar con los archivos sin necesidad de usar `sudo`.

### **4.2 Prueba los permisos**

Crea un archivo de prueba en el directorio raíz para verificar que los permisos funcionan:

```bash
echo "<h1>¡Apache funciona correctamente!</h1>" > /var/www/html/index.html
```

Luego abre tu navegador y visita:

```bash
http://localhost
```

Deberías ver el mensaje: **"¡Apache funciona correctamente!"**.

------

## **5. Comandos básicos para gestionar Apache**

### Iniciar Apache:

```bash
sudo systemctl start apache2
```

### Detener Apache:

```bash
sudo systemctl stop apache2
```

### Reiniciar Apache:

Si realizas cambios en los archivos de configuración de Apache, reinicia el servicio para que se apliquen:

```bash
sudo systemctl restart apache2
```

### Recargar Apache (sin interrumpir conexiones activas):

```bash
sudo systemctl reload apache2
```

### Habilitar Apache para que inicie automáticamente al arrancar el sistema:

```bash
sudo systemctl enable apache2
```

### Deshabilitar Apache para que no inicie automáticamente:

```bash
sudo systemctl disable apache2
```

### Verificar el estado de Apache:

```bash
sudo systemctl status apache2
```

------

## **6. Configurar permisos para todos los usuarios (opcional)**

Si necesitas que todos los usuarios puedan acceder y modificar archivos en el directorio raíz de Apache (`/var/www/html`), asegúrate de aplicar estos permisos:

1. Otorga permisos para todos los usuarios:

   ```bash
   sudo chmod -R 777 /var/www/html
   ```

2. Si prefieres otorgar acceso a un grupo en lugar de a todos los usuarios, crea un grupo (por ejemplo, `webdev`), agrégale usuarios, y cambia los permisos:

   ```bash
   sudo groupadd webdev
   sudo usermod -a -G webdev tu_usuario
   sudo chown -R www-data:webdev /var/www/html
   sudo chmod -R 775 /var/www/html
   ```

   Esto permitirá que los usuarios del grupo `webdev` puedan gestionar archivos en `/var/www/html`.

------

## **7. Configuración adicional**

Si deseas cambiar el puerto predeterminado de Apache (por defecto es el puerto `80`):

1. Abre el archivo de configuración principal de Apache:

   ```bash
   sudo nano /etc/apache2/ports.conf
   ```

2. Busca la línea que dice:

   ```bash
   Listen 80
   ```

   Cámbiala por el puerto deseado, por ejemplo:

   ```bash
   Listen 8080
   ```

3. Guarda y cierra el archivo (`Ctrl + O` y luego `Ctrl + X`).

4. Edita también el archivo de configuración del sitio:

   ```bash
   sudo nano /etc/apache2/sites-available/000-default.conf
   ```

   Cambia `<VirtualHost *:80>` por el puerto nuevo, por ejemplo:

   ```bash
   <VirtualHost *:8080>
   ```

5. Reinicia Apache para aplicar los cambios:

   ```bash
   sudo systemctl restart apache2
   ```

6. Accede al servidor con el nuevo puerto en el navegador, por ejemplo:

   ```bash
   http://localhost:8080
   ```

------

## **8. Solución de problemas comunes**

### Apache no inicia o muestra errores

1. Verifica los registros de errores de Apache:

   ```bash
   sudo tail -f /var/log/apache2/error.log
   ```

2. Si el puerto `80` está ocupado, cambia el puerto en los archivos de configuración como se indicó en el paso anterior.