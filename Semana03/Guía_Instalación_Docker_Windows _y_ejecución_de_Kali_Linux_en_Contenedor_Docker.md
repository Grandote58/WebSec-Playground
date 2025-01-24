![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Guía detallada de instalación de Docker en Windows y ejecución de Kali Linux en un contenedor Docker**

#### **1. Instalación de Docker en Windows**

Docker en Windows permite usar contenedores para ejecutar sistemas operativos y aplicaciones de manera aislada. Sigue estos pasos:

##### **1.1 Requisitos previos**

- **Sistema operativo:** Windows 10 o superior (64 bits, con soporte para Hyper-V o WSL 2).
- **Hardware:** Al menos 4 GB de RAM.

##### **1.2 Descarga e instalación**

1. **Descargar Docker Desktop**:

   - Ve al sitio oficial de Docker (https://www.docker.com/).
   - Descarga **Docker Desktop para Windows**.

2. **Instalación**:

   - Ejecuta el instalador y sigue las instrucciones.
   - Habilita **Hyper-V** o **WSL 2** si se te solicita durante la instalación.

3. **Configurar Docker Desktop**:

   - Una vez instalado, inicia Docker Desktop.
   - Si no está activado, habilita la integración con **WSL 2** en las configuraciones.

4. **Validar instalación**:

   - Abre una terminal (PowerShell o CMD) y ejecuta:

     ```bash
     docker --version
     ```

   - Deberías ver la versión de Docker instalada.

##### **1.3 Configuración del entorno**

1. Verifica que Docker esté corriendo:

   ```bash
   docker info
   ```

2. Asegúrate de que WSL 2 esté configurado como backend predeterminado si estás usando Windows Home.

------

#### **2. Instalación de Kali Linux en Docker**

Kali Linux es una distribución popular para pruebas de penetración. Usar Docker para Kali permite ejecutarlo de manera aislada.

##### **2.1 Descargar y ejecutar Kali Linux**

1. **Buscar la imagen oficial de Kali Linux**:

   ```bash
   docker search kalilinux
   ```

   - Busca la imagen oficial: `kalilinux/kali`.

2. **Descargar la imagen**:

   ```bash
   docker pull kalilinux/kali
   ```

3. **Ejecutar un contenedor de Kali Linux**:

   - Ejecuta un contenedor interactivo con Kali Linux:

     ```bash
     docker run -it --name kali_instance kalilinux/kali
     ```

   - Esto te dará acceso a una terminal dentro del contenedor de Kali.

4. **Actualizar el sistema en el contenedor**:

   - Dentro del contenedor, actualiza los paquetes:

     ```bash
     apt update && apt upgrade -y
     ```

##### **2.2 Configuración adicional (opcional)**

- Instalar herramientas de seguridad:

  - Instala herramientas de prueba de penetración comunes:

    ```bash
    apt install -y kali-linux-default
    ```

- Configurar un volumen persistente:

  - Si deseas que los datos persistan al reiniciar el contenedor:

    ```
    docker run -it --name kali_instance -v C:\ruta\local:/root kalilinux/kali
    ```

------

#### **3. Comandos útiles para manejar Docker y Kali Linux**

##### **3.1 Comandos básicos de Docker**

- Listar contenedores activos:

  ```bash
  docker ps
  ```

- Listar todos los contenedores (incluso detenidos):

  ```bash
  docker ps -a
  ```

- Detener un contenedor:

  ```bash
  docker stop kali_instance
  ```

- Iniciar un contenedor existente:

  ```bash
  docker start -ai kali_instance
  ```

- Eliminar un contenedor:

  ```bash
  docker rm kali_instance
  ```

##### **3.2 Comandos para limpiar Docker**

- Eliminar imágenes no utilizadas:

  ```bash
  docker image prune
  ```

- Eliminar todos los contenedores e imágenes:

  ```bash
  docker system prune -a
  ```

------

#### **4. Cierre del entorno**

Cuando termines de usar el contenedor:

1. **Salir del contenedor**:

   - Dentro del contenedor, escribe:

     ```bash
     exit
     ```

2. **Detener Docker Desktop**:

   - Cierra Docker Desktop si no lo necesitas más.