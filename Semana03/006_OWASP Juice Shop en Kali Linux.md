![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **OWASP Juice Shop en Kali Linux**

## **1. Requisitos previos**

Antes de instalar OWASP Juice Shop, asegúrate de que tu sistema tenga lo siguiente:

1. **Node.js** y **npm** instalados (Juice Shop se ejecuta como una aplicación Node.js).
2. **Git** para clonar el repositorio (opcional, pero recomendado).

------

## **2. Actualiza el sistema**

Actualiza los paquetes de tu sistema para evitar problemas de compatibilidad:

```bash
sudo apt update && sudo apt upgrade -y
```

------

## **3. Instalar Node.js y npm**

Juice Shop requiere **Node.js** (versión 16 o superior) y **npm**. Instálalos con los siguientes comandos:

1. Instala Node.js desde el repositorio oficial de NodeSource:

   ```bash
   curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
   sudo apt install -y nodejs
   ```

2. Verifica la instalación de Node.js y npm:

   ```bash
   node -v
   npm -v
   ```

   Deberías ver las versiones instaladas. Asegúrate de que Node.js sea 16 o superior.

------

## **4. Descargar OWASP Juice Shop**

Tienes dos opciones para descargar Juice Shop: clonar el repositorio oficial desde GitHub o descargar el archivo ZIP.

### Opción 1: Clonar el repositorio desde GitHub (recomendado)

1. Clona el repositorio oficial:

   ```bash
   git clone https://github.com/juice-shop/juice-shop.git
   ```

2. Accede al directorio clonado:

   ```bash
   cd juice-shop
   ```

### Opción 2: Descargar el archivo ZIP

1. Ve al repositorio oficial: https://github.com/juice-shop/juice-shop.

2. Haz clic en **Code** → **Download ZIP**.

3. Extrae el archivo y navega al directorio extraído:

   ```bash
   cd /ruta/del/directorio/juice-shop
   ```

------

## **5. Instalar las dependencias**

Dentro del directorio de Juice Shop, instala las dependencias necesarias ejecutando:

```bash
npm install
```

Esto descargará todos los módulos y paquetes necesarios para ejecutar OWASP Juice Shop.

------

## **6. Configurar permisos para todos los usuarios**

Si necesitas que todos los usuarios puedan trabajar con Juice Shop, otorga permisos al directorio:

1. Ve al directorio donde está Juice Shop:

   ```bash
   cd /ruta/del/directorio/juice-shop
   ```

2. Cambia los permisos para que todos los usuarios tengan acceso completo:

   ```bash
   sudo chmod -R 777 .
   ```

3. Cambia el propietario del directorio (opcional, si prefieres que el propietario sea tu usuario actual):

   ```bash
   sudo chown -R $USER:$USER .
   ```

------

## **7. Ejecutar OWASP Juice Shop**

Para iniciar OWASP Juice Shop, utiliza el siguiente comando dentro del directorio de Juice Shop:

```bash
npm start
```

Esto iniciará el servidor de Juice Shop en el puerto 3000 de manera predeterminada. Verás un mensaje en la terminal como este:

```bash
Server listening on port 3000
```

------

## **8. Acceder a Juice Shop**

1. Abre tu navegador web.

2. Accede a la aplicación en:

   ```bash
   http://localhost:3000
   ```

3. Deberías ver la página principal de OWASP Juice Shop.

------

## **9. Comandos básicos para gestionar OWASP Juice Shop**

Si quieres detener o reiniciar el servidor, sigue estos pasos:

### **Iniciar OWASP Juice Shop**

1. Ve al directorio donde está Juice Shop:

   ```bash
   cd /ruta/del/directorio/juice-shop
   ```

2. Ejecuta el siguiente comando:

   ```bash
   npm start
   ```

### **Detener OWASP Juice Shop**

Para detener el servidor, simplemente presiona **Ctrl + C** en la terminal donde se está ejecutando.

### **Reiniciar OWASP Juice Shop**

Detén el servidor con **Ctrl + C** y luego ejecútalo nuevamente con:

```bash
npm start
```

------

## **10. Configurar Juice Shop como un servicio (opcional)**

Si prefieres ejecutar Juice Shop como un servicio del sistema para que se inicie automáticamente y puedas gestionarlo fácilmente, sigue estos pasos:

1. Crea un archivo de servicio para Juice Shop:

   ```bash
   sudo nano /etc/systemd/system/juice-shop.service
   ```

2. Agrega el siguiente contenido al archivo:

   ```
   [Unit]
   Description=OWASP Juice Shop
   After=network.target
   
   [Service]
   Type=simple
   ExecStart=/usr/bin/npm start
   WorkingDirectory=/ruta/del/directorio/juice-shop
   Restart=always
   User=tu_usuario
   Environment=NODE_ENV=production
   
   [Install]
   WantedBy=multi-user.target
   ```

   - Reemplaza `/ruta/del/directorio/juice-shop` con la ruta al directorio donde descargaste Juice Shop.
   - Cambia `tu_usuario` por el usuario que debe ejecutar el servicio.

3. Guarda el archivo y recarga los servicios del sistema:

   ```bash
   sudo systemctl daemon-reload
   ```

4. Inicia el servicio:

   ```bash
   sudo systemctl start juice-shop
   ```

5. Habilita el servicio para que se inicie automáticamente al arrancar el sistema:

   ```bash
   sudo systemctl enable juice-shop
   ```

6. Verifica el estado del servicio:

   ```bash
   sudo systemctl status juice-shop
   ```

------

## **11. Solución de problemas comunes**

### Error: `EADDRINUSE` (puerto en uso)

Si Juice Shop no inicia porque el puerto 3000 está en uso, edita el archivo `config/default.yml` dentro del directorio de Juice Shop para cambiar el puerto:

1. Abre el archivo:

   ```bash
   nano config/default.yml
   ```

2. Cambia el valor de `port` a otro puerto, como el `8080`:

   ```bash
   port: 8080
   ```

3. Guarda los cambios y reinicia el servidor.