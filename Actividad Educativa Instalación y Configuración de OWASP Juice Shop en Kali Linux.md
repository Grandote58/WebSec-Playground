

# ![Mesa de trabajo 1Head](http://drive.google.com/uc?export=view&id=1p2rqX0Nck3MI8LKzYct_oEMRETRIhzTH)

# **Actividad Educativa: Instalación y Configuración de OWASP Juice Shop en Kali Linux**

## Introducción

OWASP Juice Shop es una aplicación web intencionalmente vulnerable que se utiliza para aprender sobre seguridad de aplicaciones web. En esta actividad, los estudiantes aprenderán a instalar y configurar OWASP Juice Shop en Kali Linux utilizando Docker y la línea de comandos.

## Visita la pagina 

https://owasp.org/www-project-juice-shop/



## Objetivos

1. Instalar Docker en Kali Linux.
2. Configurar Docker para su uso.
3. Ejecutar OWASP Juice Shop en un contenedor Docker.
4. Validar la ejecución de OWASP Juice Shop.

## Pasos a Seguir

### Paso 1: Añadir la clave GPG de Docker

Para poder instalar Docker desde su repositorio oficial, primero necesitamos añadir su clave GPG. Esto garantiza que los paquetes que instalamos son auténticos.

```bash
echo 'deb [arch=amd64] https://download.docker.com/linux/debian buster stable' | sudo tee /etc/apt/sources.list.d/docker.list
```

### Paso 2: Añadir el repositorio de Docker a las fuentes de APT

Añadimos el repositorio de Docker a nuestra lista de fuentes APT para poder instalar Docker CE desde él.

```bash
echo 'deb [arch=amd64] https://download.docker.com/linux/debian buster stable' | sudo tee /etc/apt/sources.list.d/docker.list
```

### Paso 3: Actualizar el índice de paquetes

Actualizamos la lista de paquetes disponibles para que incluya el repositorio de Docker que acabamos de añadir.

```bash
sudo apt-get update
```

### Paso 4: Instalar Docker CE

Instalamos Docker Community Edition, que es la versión gratuita y de código abierto de Docker.

```bash
sudo apt-get install docker-ce
```

### Paso 5: Ejecutar el contenedor de prueba de Docker

Para verificar que Docker se ha instalado correctamente, ejecutamos un contenedor de prueba.

```bash
sudo docker run hello-world
```

### Paso 6: Iniciar el servicio Docker

Iniciamos el servicio Docker para que podamos empezar a usarlo.

```bash
sudo systemctl start docker
```

### Paso 7: Habilitar el servicio Docker para que inicie al arrancar

Configuramos Docker para que se inicie automáticamente cuando arranque el sistema.

```bash
sudo systemctl enable docker
```

### Paso 8: Crear el grupo de usuarios de Docker

Creamos un grupo llamado `docker` para gestionar permisos de usuario de Docker.

```bash
sudo groupadd docker
```

### Paso 9: Añadir el usuario actual al grupo Docker

Añadimos nuestro usuario actual al grupo `docker` para poder ejecutar comandos Docker sin `sudo`.

```bash
sudo usermod -aG docker $USER
```

### Paso 10: Actualizar la sesión del grupo Docker

Actualizamos la sesión para aplicar los cambios de grupo sin necesidad de reiniciar.

```bash
newgrp docker
```

### Paso 11: Ejecutar OWASP Juice Shop en un contenedor Docker

Ejecutamos OWASP Juice Shop en un contenedor Docker, exponiendo el puerto 3000.

```bash
docker run --rm -p 3000:3000 bkimminich/juice-shop
```

### Paso 12: Validar la ejecución de Juice Shop

Abrimos un navegador web y navegamos a `http://localhost:3000` para validar que OWASP Juice Shop está corriendo correctamente.



## ¡Con estos pasos, habrás instalado y configurado OWASP Juice Shop en tu sistema Kali Linux usando Docker!





![Mesa de trabajo 1FOOTER](http://drive.google.com/uc?export=view&id=1vwLVsNlcF2PEyv9fULe2cohQnVfwRWLg)







