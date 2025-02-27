![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **MÓDULO DE APRENDIZAJE: `kali-tools-wireless` - ATAQUES A REDES INALÁMBRICAS (Wi-Fi, Bluetooth, RFID y SDR)**

## **1. INTRODUCCIÓN AL METAPAQUETE `kali-tools-wireless`**

El metapaquete **`kali-tools-wireless`** en Kali Linux agrupa herramientas específicas para **auditoría, ataque y análisis de redes inalámbricas**, incluyendo **Wi-Fi (802.11), Bluetooth, RFID y radio definida por software (SDR)**.

### **1.1 Objetivos del Metapaquete**



✅ Realizar **auditorías de seguridad en redes Wi-Fi**.

✅ Capturar y descifrar paquetes de **redes 802.11**.

✅ Interceptar y analizar tráfico en **dispositivos Bluetooth**.

✅ Manipular y evaluar la seguridad de sistemas **RFID**.

✅ Experimentar con **radio definida por software (SDR)** para interceptar y decodificar señales.



### **1.2 Instalación del Metapaquete**

Para instalar todas las herramientas incluidas en `kali-tools-wireless`, ejecuta:

```bash
sudo apt update
sudo apt install kali-tools-wireless -y
```

## **2. HERRAMIENTAS INCLUIDAS EN `kali-tools-wireless` Y SU USO PRÁCTICO**

A continuación, exploraremos las principales herramientas del metapaquete junto con ejemplos prácticos.

## **2.1 Ataques a Redes Wi-Fi (802.11)**

Las siguientes herramientas permiten analizar redes inalámbricas, capturar tráfico y romper contraseñas de redes Wi-Fi.

### **2.1.1 Aircrack-ng - Suite de Ataques a Wi-Fi**

Aircrack-ng es una colección de herramientas para auditoría de redes Wi-Fi.

#### **Modo monitor y captura de tráfico Wi-Fi**

1. Activar el modo monitor en la tarjeta de red Wi-Fi:

   ```bash
   sudo airmon-ng start wlan0
   ```

2. Escanear redes disponibles:

   ```bash
   sudo airodump-ng wlan0mon
   ```

3. Capturar tráfico de una red específica:

   ```bash
   sudo airodump-ng -c 6 --bssid XX:XX:XX:XX:XX:XX -w captura wlan0mon
   ```

4. Romper una clave 

   WPA/WPA2

    capturada:

   ```bash
   sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt -b XX:XX:XX:XX:XX:XX captura.cap
   ```

### **2.1.2 Wifite - Ataques Automáticos a Redes Wi-Fi**

Wifite automatiza ataques a redes Wi-Fi protegidas con WEP, WPA y WPS.

```bash
sudo wifite
```

🔹 **Explicación:** Escanea redes cercanas y ataca automáticamente aquellas vulnerables.

### **2.1.3 Reaver - Ataques WPS (Wi-Fi Protected Setup)**

Si un router tiene WPS habilitado, puede ser vulnerable a ataques de fuerza bruta.

```bash
sudo reaver -i wlan0mon -b XX:XX:XX:XX:XX:XX -vv
```

🔹 **Explicación:** Intenta recuperar la clave Wi-Fi explotando WPS.

## **2.2 Ataques a Dispositivos Bluetooth**

Estas herramientas permiten auditar y explotar vulnerabilidades en dispositivos Bluetooth.

### **2.2.1 Bluetoothctl - Administración de Dispositivos Bluetooth**

Para escanear dispositivos Bluetooth cercanos:

```bash
bluetoothctl scan on
```

### **2.2.2 Bluelog - Escaneo y Registro de Dispositivos Bluetooth**

```bash
sudo bluelog -i hci0
```

🔹 **Explicación:** Monitorea y registra dispositivos Bluetooth activos.

### **2.2.3 L2ping - Ataque de Denegación de Servicio Bluetooth**

Este ataque bombardea un dispositivo Bluetooth con paquetes de datos.

```bash
l2ping -i hci0 -s 600 -f XX:XX:XX:XX:XX:XX
```

🔹 **Explicación:** Envía paquetes grandes para intentar bloquear el dispositivo.

------

## **2.3 Auditoría y Ataques a Sistemas RFID**

Los sistemas RFID (Identificación por Radiofrecuencia) son utilizados en tarjetas de acceso y sistemas de autenticación.

### **2.3.1 RfCat - Manipulación de Señales RFID**

```bash
rfcat -r
```

🔹 **Explicación:** Permite leer, analizar y modificar señales RFID.

### **2.3.2 MFCUK y MFOC - Ataques a Tarjetas RFID Mifare**

Las tarjetas Mifare son ampliamente utilizadas en sistemas de transporte y acceso.

1. Para obtener la clave de una tarjeta Mifare vulnerable:

   ```bash
   mfcuk -C -R 0:A -s 250 -S 250 -O dump.mfd
   ```

2. Para clonar una tarjeta después de obtener la clave:

   ```bash
   mfoc -O dump.mfd
   ```

## **2.4 Análisis de Radiofrecuencia con SDR (Software Defined Radio)**

SDR permite capturar y analizar señales de radio como comunicación de taxis, aeronáutica o TV digital.

### **2.4.1 Gqrx - Escaneo de Frecuencias de Radio**

```bash
gqrx
```

🔹 **Explicación:** Permite escuchar señales de radio en tiempo real.

### **2.4.2 RTL-SDR - Captura de Señales con un Dongle USB**

```bash
rtl_fm -f 433.92M -s 200000 -g 50 -r 48000 - | aplay -r 48000 -f S16_LE
```

🔹 **Explicación:** Captura y reproduce señales en la banda de 433.92 MHz.

## **3. PRÁCTICAS Y EJERCICIOS**

A continuación, ejercicios prácticos para reforzar el aprendizaje.

### **3.1 Ejercicio 1: Auditoría de Redes Wi-Fi con Aircrack-ng**

1. Poner la tarjeta en modo monitor:

   ```bash
   sudo airmon-ng start wlan0
   ```

2. Escanear redes cercanas:

   ```bash
   sudo airodump-ng wlan0mon
   ```

3. Capturar tráfico y romper la clave con `aircrack-ng`.

### **3.2 Ejercicio 2: Descubrimiento de Dispositivos Bluetooth**

1. Escanear dispositivos Bluetooth cercanos:

   ```bash
   bluetoothctl scan on
   ```

2. Enviar un ping a un dispositivo:

   ```bash
   l2ping -c 5 XX:XX:XX:XX:XX:XX
   ```

### **3.3 Ejercicio 3: Interceptación de Señales RFID**

1. Leer una tarjeta RFID con `mfoc`:

   ```bash
   mfoc -O tarjeta_dump.mfd
   ```

2. Analizar los datos extraídos.

## **4. PROYECTO FINAL: ANÁLISIS DE SEGURIDAD INALÁMBRICA**

Realizar un **análisis completo de un entorno inalámbrico**, incluyendo redes Wi-Fi, Bluetooth y RFID.

### **4.1 Auditoría de Wi-Fi**

1. Capturar tráfico con `airodump-ng`.
2. Romper la clave con `aircrack-ng`.

### **4.2 Análisis de Bluetooth**

1. Escanear dispositivos con `bluetoothctl`.
2. Intentar ataques de denegación de servicio con `l2ping`.

### **4.3 Evaluación de Seguridad RFID**

1. Extraer datos de una tarjeta con `mfoc`.
2. Clonar una tarjeta Mifare.



## **5. CONCLUSIÓN Y SIGUIENTES PASOS**

Este módulo cubre el uso del metapaquete `kali-tools-wireless` para **auditoría de seguridad en redes inalámbricas, Bluetooth, RFID y SDR**.

🔹 **Siguientes pasos:**

- Experimentar con herramientas de radiofrecuencia avanzadas.
- Investigar la seguridad en dispositivos IoT.
- Aprender técnicas de mitigación para proteger infraestructuras inalámbricas.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)