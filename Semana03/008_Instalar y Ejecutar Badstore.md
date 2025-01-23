![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Instalar y Ejecutar Badstore**



## **1. ¿Qué es Badstore?**

**Badstore** es una aplicación web deliberadamente insegura que simula un entorno de comercio electrónico vulnerable. Está basada en PHP y MySQL, y requiere un servidor web como **Apache**.

------

## **2. Requisitos previos**

Antes de instalar Badstore, asegúrate de que tienes:

1. **Kali Linux** instalado y actualizado.
2. Un servidor web como **Apache**.
3. **MySQL** o **MariaDB**.
4. **PHP** y sus extensiones necesarias.

------

## **3. Actualizar el sistema**

Primero, actualiza los paquetes del sistema para asegurarte de que todo esté al día:

```bash
sudo apt update && sudo apt upgrade -y
```

------

## **4. Instalar Apache, MySQL y PHP**

Badstore requiere un servidor LAMP (Linux, Apache, MySQL y PHP). Instálalo con el siguiente comando:

```bash
sudo apt install apache2 mysql-server php libapache2-mod-php php-mysql unzip -y
```

### Verifica que los servicios estén funcionando:

- Inicia Apache:

  ```bash
  sudo systemctl start apache2
  ```

- Inicia MySQL:

  ```bash
  sudo systemctl start mysql
  ```

- Asegúrate de que Apache y MySQL estén activos:

  ```bash
  sudo systemctl status apache2
  sudo systemctl status mysql
  ```

------

## **5. Descargar Badstore**

Badstore no tiene un repositorio oficial en GitHub, pero se puede descargar desde http://www.vulnweb.com/.

1. Descarga el archivo ZIP de Badstore:

   ```bash
   wget http://downloads.sourceforge.net/project/badstore/badstore.zip
   ```

2. Extrae el archivo ZIP:

   ```bash
   unzip badstore.zip
   ```

3. Mueve los archivos de Badstore al directorio raíz de Apache (`/var/www/html`):

   ```bash
   sudo mv badstore-master /var/www/html/badstore
   ```

4. Otorga permisos para que todos los usuarios puedan trabajar con los archivos:

   ```bash
   sudo chmod -R 777 /var/www/html/badstore
   sudo chown -R www-data:www-data /var/www/html/badstore
   ```

------

## **6. Configurar la base de datos**

Badstore requiere una base de datos MySQL para funcionar. Sigue estos pasos para configurarla:

1. Inicia sesión en MySQL como root:

   ```bash
   sudo mysql -u root -p
   ```

2. Crea una nueva base de datos llamada `badstore`:

   ```bash
   CREATE DATABASE badstore;
   ```

3. Crea un usuario para Badstore y otórgale permisos sobre la base de datos:

   ```bash
   CREATE USER 'badstore_user'@'localhost' IDENTIFIED BY 'password123';
   GRANT ALL PRIVILEGES ON badstore.* TO 'badstore_user'@'localhost';
   FLUSH PRIVILEGES;
   ```

4. Sal de MySQL:

   ```bash
   EXIT;
   ```

5. Importa el esquema de base de datos de Badstore. El archivo SQL normalmente se encuentra en la carpeta extraída:

   ```bash
   sudo mysql -u badstore_user -p badstore < /var/www/html/badstore/badstore.sql
   ```

   Ingresa la contraseña `password123` cuando se te solicite.

------

## **7. Configurar Apache**

1. Crea un archivo de configuración para Badstore en Apache:

   ```bash
   sudo nano /etc/apache2/sites-available/badstore.conf
   ```

2. Agrega la siguiente configuración:

   ```bash
   <VirtualHost *:80>
       ServerAdmin admin@localhost
       DocumentRoot /var/www/html/badstore
       ServerName badstore.local
   
       <Directory /var/www/html/badstore>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>
   
       ErrorLog ${APACHE_LOG_DIR}/badstore_error.log
       CustomLog ${APACHE_LOG_DIR}/badstore_access.log combined
   </VirtualHost>
   ```

3. Guarda y cierra el archivo (`Ctrl + O`, luego `Ctrl + X`).

4. Habilita el sitio de Badstore:

   ```bash
   sudo a2ensite badstore.conf
   ```

5. Habilita el módulo `rewrite` de Apache (requerido para algunos sitios PHP):

   ```bash
   sudo a2enmod rewrite
   ```

6. Reinicia Apache para aplicar los cambios:

   ```bash
   sudo systemctl restart apache2
   ```

------

## **8. Acceder a Badstore**

1. Abre tu navegador y visita:

   ```bash
   http://localhost/badstore
   ```

2. Si configuraste un dominio local (`badstore.local`), asegúrate de editar tu archivo `/etc/hosts` para asociar el dominio con `127.0.0.1`:

   ```bash
   sudo nano /etc/hosts
   ```

   Agrega esta línea al final del archivo:

   ```bash
   127.0.0.1    badstore.local
   ```

   Luego guarda los cambios (`Ctrl + O`, luego `Ctrl + X`) y accede a:

   ```bash
   http://badstore.local
   ```

------

## **9. Comandos básicos para gestionar Badstore**

### **Iniciar Apache (y MySQL si no está activo):**

```bash
sudo systemctl start apache2
sudo systemctl start mysql
```

### **Detener Apache (y MySQL):**

```bash
sudo systemctl stop apache2
sudo systemctl stop mysql
```

### **Reiniciar Apache (y MySQL):**

```bash
sudo systemctl restart apache2
sudo systemctl restart mysql
```

### **Verificar el estado de los servicios:**

```bash
sudo systemctl status apache2
sudo systemctl status mysql
```

------

## **10. Configurar permisos adicionales (opcional)**

Si necesitas que otros usuarios puedan gestionar los archivos en `/var/www/html/badstore`, puedes configurar permisos adicionales:

1. Permite acceso completo al directorio de Badstore:

   ```bash
   sudo chmod -R 777 /var/www/html/badstore
   ```

2. Si prefieres que un grupo específico administre el directorio, crea un grupo y agrega usuarios:

   ```bash
   sudo groupadd webdev
   sudo usermod -a -G webdev tu_usuario
   sudo chown -R www-data:webdev /var/www/html/badstore
   sudo chmod -R 775 /var/www/html/badstore
   ```

------

## **11. Solución de problemas comunes**

### Error: `403 Forbidden`

Asegúrate de que los permisos del directorio `/var/www/html/badstore` estén configurados correctamente:

```bash
sudo chmod -R 755 /var/www/html/badstore
sudo chown -R www-data:www-data /var/www/html/badstore
```

### Error: `Can't connect to database`

Verifica que la base de datos esté configurada correctamente y que las credenciales en los archivos de configuración coincidan.



