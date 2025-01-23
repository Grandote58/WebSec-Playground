![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **WebGoat** en **Kali Linux**

## **1. ¿Qué es WebGoat?**

**WebGoat** es una aplicación web intencionadamente insegura desarrollada por OWASP para enseñar prácticas de seguridad. Es ideal para aprender sobre vulnerabilidades como inyección SQL, XSS, CSRF, etc. Se ejecuta como una aplicación Java, lo que requiere que tengas instalado **Java** en tu sistema.

------

## **2. Requisitos previos**

Antes de instalar WebGoat, asegúrate de tener lo siguiente:

1. **Java 17 o superior** instalado.
2. **Git** (opcional, para clonar el repositorio de WebGoat).
3. **Permisos de superusuario** para realizar cambios en el sistema.

------

## **3. Actualiza tu sistema**

Actualiza los paquetes de tu sistema para garantizar que todo esté al día:

```bash
sudo apt update && sudo apt upgrade -y
```

------

## **4. Instalar Java**

WebGoat requiere **Java 17 o superior**. Instálalo con los siguientes pasos:

1. Instala OpenJDK 17:

   ```bash
   sudo apt install openjdk-17-jdk -y
   ```

2. Verifica la instalación de Java:

   ```bash
   java -version
   ```

   Deberías ver algo como:

   ```bash
   openjdk version "17.x.x"
   ```

------

## **5. Descargar WebGoat**

Hay dos formas de obtener WebGoat: descargando el archivo JAR directamente o clonando el repositorio oficial.

### **Opción 1: Descargar el archivo JAR directamente**

1. Descarga el archivo JAR de WebGoat desde el repositorio oficial:

   ```bash
   wget https://github.com/WebGoat/WebGoat/releases/latest/download/webgoat-server-8.3.0.jar
   ```

2. Mueve el archivo a un directorio de tu elección, por ejemplo:

   ```bash
   sudo mv webgoat-server-8.3.0.jar /opt/webgoat/
   ```

   Crea el directorio si no existe:

   ```bash
   sudo mkdir -p /opt/webgoat
   ```

------

### **Opción 2: Clonar el repositorio desde GitHub**

Si prefieres clonar el repositorio de WebGoat:

1. Clona el repositorio oficial:

   ```bash
   git clone https://github.com/WebGoat/WebGoat.git
   ```

2. Ve al directorio del repositorio clonado:

   ```bash
   cd WebGoat
   ```

3. Construye el proyecto utilizando **Maven**:

   ```bash
   mvn clean install
   ```

4. Encuentra el archivo JAR en el directorio `target/` y muévelo a un directorio de tu elección:

   ```bash
   sudo mv target/webgoat-server-*.jar /opt/webgoat/
   ```

------

## **6. Configurar permisos para todos los usuarios**

1. Asegúrate de que todos los usuarios puedan acceder al directorio donde está WebGoat:

   ```bash
   sudo chmod -R 777 /opt/webgoat
   ```

2. Cambia el propietario del directorio si deseas asignarlo a tu usuario actual:

   ```bash
   sudo chown -R $USER:$USER /opt/webgoat
   ```

------

## **7. Ejecutar WebGoat**

1. Ve al directorio donde está el archivo JAR de WebGoat:

   ```bash
   cd /opt/webgoat
   ```

2. Ejecuta el archivo JAR con el siguiente comando:

   ```bash
   java -jar webgoat-server-8.3.0.jar
   ```

   Esto iniciará el servidor de WebGoat en el puerto **8080** de manera predeterminada. Deberías ver un mensaje en la terminal como:

   ```bash
   WebGoat started on port 8080
   ```

3. Abre tu navegador y accede a WebGoat:

   ```bash
   http://localhost:8080/WebGoat
   ```

------

## **8. Comandos básicos para gestionar WebGoat**

### **Iniciar WebGoat**

1. Ve al directorio donde está el archivo JAR:

   ```bash
   cd /opt/webgoat
   ```

2. Inicia el servidor:

   ```bash
   java -jar webgoat-server-8.3.0.jar
   ```

### **Detener WebGoat**

Para detener WebGoat, presiona **Ctrl + C** en la terminal donde se está ejecutando.

### **Reiniciar WebGoat**

1. Detén el servidor con **Ctrl + C**.

2. Ejecuta nuevamente el comando para iniciar:

   ```bash
   java -jar webgoat-server-8.3.0.jar
   ```

------

## **9. Configurar WebGoat como un servicio (opcional)**

Si prefieres ejecutar WebGoat como un servicio del sistema para facilitar su gestión, sigue estos pasos:

1. Crea un archivo de servicio:

   ```bash
   sudo nano /etc/systemd/system/webgoat.service
   ```

2. Agrega el siguiente contenido:

   ```bash
   [Unit]
   Description=WebGoat Vulnerable Application
   After=network.target
   
   [Service]
   Type=simple
   ExecStart=/usr/bin/java -jar /opt/webgoat/webgoat-server-8.3.0.jar
   Restart=always
   User=tu_usuario
   WorkingDirectory=/opt/webgoat
   
   [Install]
   WantedBy=multi-user.target
   ```

   - Cambia `tu_usuario` por tu usuario actual.

3. Guarda y cierra el archivo (`Ctrl + O`, luego `Ctrl + X`).

4. Recarga los servicios del sistema:

   ```bash
   sudo systemctl daemon-reload
   ```

5. Inicia el servicio de WebGoat:

   ```bash
   sudo systemctl start webgoat
   ```

6. Habilita el servicio para que inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable webgoat
   ```

7. Verifica el estado del servicio:

   ```bash
   sudo systemctl status webgoat
   ```

------

## **10. Solución de problemas comunes**

### Error: `Address already in use`

Si el puerto **8080** ya está en uso por otra aplicación, puedes cambiar el puerto de WebGoat utilizando la opción `--server.port`:

```bash
java -jar webgoat-server-8.3.0.jar --server.port=9090
```

Luego accede a:

```bash
http://localhost:9090/WebGoat
```

### Error: `Permission denied`

Si ves un error de permisos, asegúrate de que el archivo JAR y su directorio tengan los permisos adecuados:

```bash
sudo chmod -R 777 /opt/webgoat
sudo chown -R $USER:$USER /opt/webgoat
```





