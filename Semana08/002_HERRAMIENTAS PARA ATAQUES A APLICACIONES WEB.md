![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **MÓDULO DE APRENDIZAJE: KALI-TOOLS-WEB - HERRAMIENTAS PARA ATAQUES A APLICACIONES WEB**

## **1. INTRODUCCIÓN AL METAPAQUETE `kali-tools-web`**

Kali Linux organiza sus herramientas en metapaquetes para facilitar la instalación y uso de herramientas específicas. 

Uno de estos metapaquetes es **`kali-tools-web`**, que agrupa herramientas diseñadas para **evaluar, analizar y explotar vulnerabilidades en aplicaciones web**.

### **1.1 Objetivo del Metapaquete**

El metapaquete **`kali-tools-web`** permite a los pentesters:

- Realizar escaneos de seguridad en aplicaciones web.
- Identificar y explotar vulnerabilidades como **inyección SQL, XSS, SSRF, LFI/RFI, y autenticación débil**.
- Auditar servidores web en busca de configuraciones incorrectas o desactualizadas.
- Realizar ataques automatizados y pruebas de fuzzing.

### **1.2 Instalación del Metapaquete**

Para instalar todas las herramientas incluidas en `kali-tools-web`, ejecuta:

```bash
sudo apt update
sudo apt install kali-tools-web -y
```

🔗 **Referencia oficial**: https://www.kali.org/tools/kali-tools-web/

------

## **2. HERRAMIENTAS INCLUIDAS EN `kali-tools-web` Y SU USO PRÁCTICO**

A continuación, exploramos las principales herramientas incluidas en este metapaquete y ejemplos de su uso.

### **2.1 Escaneo y Recolección de Información**

Estas herramientas permiten descubrir información sobre el objetivo, como tecnologías utilizadas, subdominios y vulnerabilidades básicas.

#### **2.1.1 WhatWeb** - Detección de tecnologías web

WhatWeb identifica **CMS, frameworks, servidores y librerías** de una aplicación web.

```bash
whatweb http://objetivo.com
```

#### **2.1.2 Wappalyzer** - Extensión para identificar tecnologías web

Alternativa gráfica a WhatWeb, disponible en https://www.wappalyzer.com/.

#### **2.1.3 Sublist3r** - Descubrimiento de subdominios

```bash
sublist3r -d objetivo.com
```

------

### **2.2 Escaneo de Vulnerabilidades**

Herramientas para identificar posibles fallos de seguridad en aplicaciones web.

#### **2.2.1 Nikto** - Escaneo de servidores web en busca de vulnerabilidades

```bash
nikto -h http://objetivo.com
```

#### **2.2.2 OWASP ZAP** - Proxy de seguridad para escaneo dinámico

```bash
zaproxy
```

- Se recomienda configurar el navegador para que use `127.0.0.1:8080` como proxy.

------

### **2.3 Explotación de Vulnerabilidades**

Herramientas para explotar vulnerabilidades en aplicaciones web.

#### **2.3.1 SQLmap** - Detección y explotación de inyecciones SQL

```bash
sqlmap -u "http://objetivo.com/login.php?id=1" --dbs
```

- **Ejemplo:** Explotar inyección SQL en un formulario de inicio de sesión.

#### **2.3.2 XSStrike** - Explotación avanzada de XSS

```bash
xsstrike -u "http://objetivo.com/vulnerable.php?search="
```

#### **2.3.3 Commix** - Inyección de comandos en aplicaciones web

```bash
commix -u "http://objetivo.com/vuln.php?param="
```

------

### **2.4 Ataques de Fuerza Bruta y Enumeración**

#### **2.4.1 Hydra** - Ataque de fuerza bruta a formularios de login

```bash
hydra -L usuarios.txt -P passwords.txt objetivo.com http-post-form "/login.php:user=^USER^&pass=^PASS^:F=error"
```

#### **2.4.2 Dirb** - Descubrimiento de directorios ocultos

```bash
dirb http://objetivo.com /usr/share/wordlists/dirb/common.txt
```

#### **2.4.3 Gobuster** - Enumeración avanzada de directorios y subdominios

```bash
gobuster dir -u http://objetivo.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

------

## **3. PRÁCTICAS Y EJERCICIOS**

A continuación, se presentan ejercicios prácticos para aplicar las herramientas del metapaquete `kali-tools-web`.

### **3.1 Ejercicio 1: Escaneo de Tecnologías Web**

1. Usar 

   ```
   WhatWeb
   ```

    para detectar tecnologías en una aplicación objetivo:

   ```bash
   whatweb http://testphp.vulnweb.com
   ```

2. Complementar el análisis con `Wappalyzer`.

### **3.2 Ejercicio 2: Escaneo de Vulnerabilidades con Nikto**

1. Escanear un servidor web en busca de configuraciones erróneas:

   ```bash
   nikto -h http://testphp.vulnweb.com
   ```

### **3.3 Ejercicio 3: Ataque SQL Injection con SQLmap**

1. Escanear una URL vulnerable:

   ```bash
   sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --dbs
   ```

### **3.4 Ejercicio 4: Descubrimiento de Directorios Ocultos con Dirb**

1. Buscar archivos ocultos en una aplicación:

   ```bash
   dirb http://testphp.vulnweb.com
   ```

------

## **4. PROYECTO FINAL: ANÁLISIS DE UNA APLICACIÓN VULNERABLE**

Para consolidar lo aprendido, se recomienda instalar **DVWA (Damn Vulnerable Web Application)** y realizar pruebas con las herramientas vistas.

### **4.1 Instalación de DVWA en Kali Linux**

```bash
git clone https://github.com/digininja/DVWA.git
cd DVWA
php -S 0.0.0.0:80
```

Luego, abrir `http://localhost/setup.php` y configurar la base de datos.

### **4.2 Pruebas a realizar**

1. Identificar tecnologías con `WhatWeb`.
2. Escanear vulnerabilidades con `Nikto`.
3. Explorar directorios con `Gobuster`.
4. Explotar SQL Injection con `SQLmap`.
5. Realizar ataques XSS con `XSStrike`.

------

## **5. CONCLUSIÓN Y SIGUIENTES PASOS**

El metapaquete `kali-tools-web` agrupa herramientas esenciales para **pentesting de aplicaciones web**, permitiendo desde la recopilación de información hasta la explotación de vulnerabilidades. 

Para profundizar en pruebas de API y autenticación, se recomienda explorar herramientas como **Postman, JWT-Tool y Burp Suite Pro**.



![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)