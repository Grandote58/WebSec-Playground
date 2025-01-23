![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Instalar, Configurar y Ejecutar MySQL en Kali Linux**

## **1. Actualizar el sistema**

Antes de comenzar, actualiza los repositorios y los paquetes del sistema para garantizar que obtendrás la última versión disponible de MySQL:

```bash
sudo apt update && sudo apt upgrade -y
```

------

## **2. Instalar el servidor MySQL**

MySQL está disponible en los repositorios de Kali Linux. Instálalo con el siguiente comando:

```bash
sudo apt install mysql-server -y
```

Esto instalará el servidor MySQL y sus dependencias.

------

## **3. Iniciar el servicio MySQL**

Una vez instalado, inicia el servicio MySQL con:

```bash
sudo systemctl start mysql
```

Para verificar que el servicio está corriendo, usa:

```bash
sudo systemctl status mysql
```

Si está activo, deberías ver algo como:

```bash
   mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
   Active: active (running) since [fecha y hora]
```

------

## **4. Configurar MySQL**

Por motivos de seguridad, MySQL incluye un script de configuración inicial para asegurar tu instalación. Ejecuta:

```bash
sudo mysql_secure_installation
```

El script te pedirá que configures lo siguiente:

1. **Contraseña para el usuario root**: Establece una contraseña segura.
2. **Eliminar usuarios anónimos**: Selecciona **Sí**.
3. **Prohibir acceso remoto para root**: Selecciona **Sí** (si no necesitas acceso remoto para root).
4. **Eliminar la base de datos de prueba**: Selecciona **Sí**.
5. **Recargar privilegios**: Selecciona **Sí**.

Esto mejorará la seguridad básica de tu instalación.

------

## **5. Acceder a MySQL**

Para ingresar al cliente de MySQL, usa el siguiente comando:

```bash
sudo mysql -u root -p
```

Se te pedirá la contraseña que configuraste durante el paso anterior. Una vez dentro, verás algo como esto:

```bash
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.x MySQL Community Server - GPL
```

Ahora puedes usar comandos SQL para gestionar tu base de datos.

------

## **6. Crear un usuario con permisos para todos los usuarios**

Si deseas crear un usuario MySQL que tenga acceso completo (como un superusuario) y que pueda conectarse desde cualquier host, sigue estos pasos:

1. Abre el cliente de MySQL:

   ```bash
   sudo mysql -u root -p
   ```

2. Crea un usuario con permisos de acceso desde cualquier host (`%` indica acceso remoto):

   ```bash
   CREATE USER 'admin'@'%' IDENTIFIED BY 'password123';
   ```

3. Otorga todos los privilegios al usuario para todas las bases de datos:

   ```bash
   GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
   ```

4. Asegúrate de que los privilegios sean aplicados:

   ```bash
   FLUSH PRIVILEGES;
   ```

5. Sal del cliente de MySQL:

   ```bash
   EXIT;
   ```

Ahora el usuario `admin` puede conectarse a MySQL desde cualquier host utilizando la contraseña `password123`.

------

## **7. Configurar el archivo MySQL para conexiones remotas (opcional)**

Si necesitas permitir conexiones remotas a MySQL desde otros dispositivos:

1. Edita el archivo de configuración de MySQL:

   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

2. Busca la línea que dice:

   ```bash
   bind-address = 127.0.0.1
   ```

   Cambia `127.0.0.1` por `0.0.0.0` para permitir conexiones desde cualquier dirección IP:

   ```bash
   bind-address = 0.0.0.0
   ```

3. Guarda los cambios (`Ctrl + O`, luego `Ctrl + X`).

4. Reinicia el servicio MySQL para aplicar los cambios:

   ```bash
   sudo systemctl restart mysql
   ```

Ahora puedes conectarte a MySQL desde cualquier máquina en tu red.

------

## **8. Comandos básicos para gestionar MySQL**

Aquí están los comandos más comunes para gestionar el servicio MySQL:

### Iniciar MySQL:

```bash
sudo systemctl start mysql
```

### Detener MySQL:

```bash
sudo systemctl stop mysql
```

### Reiniciar MySQL:

```bash
sudo systemctl restart mysql
```

### Recargar MySQL (aplica cambios sin reiniciar):

```bash
sudo systemctl reload mysql
```

### Verificar el estado de MySQL:

```bash
sudo systemctl status mysql
```

### Habilitar MySQL para que inicie automáticamente al arrancar el sistema:

```bash
sudo systemctl enable mysql
```

### Deshabilitar MySQL para que no inicie automáticamente:

```bash
sudo systemctl disable mysql
```

------

## **9. Solución de problemas comunes**

### Error: `Access denied for user 'root'@'localhost'`

Si MySQL no te permite acceder como `root`, verifica que estás usando el modo autenticado. Ejecuta el cliente con `sudo`:

```bash
sudo mysql -u root
```

Luego, asegúrate de establecer la contraseña de `root`:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'nueva_contraseña';
FLUSH PRIVILEGES;
```

### Error: `Can't connect to MySQL server on '127.0.0.1'`

Asegúrate de que el servicio MySQL está en ejecución:

```bash
sudo systemctl start mysql
```

Si el problema persiste, revisa los registros de errores:

```bash
sudo tail -f /var/log/mysql/error.log
```

