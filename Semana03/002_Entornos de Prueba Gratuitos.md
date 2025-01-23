![Mesa](https://github.com/Grandote58/CloudSafeGuard/blob/main/Mesa.png)

# **Entornos de Prueba Gratuitos**

### **1. OWASP Juice Shop**

- **Descripción**: Una aplicación web moderna basada en Node.js que está diseñada para ser hackeada. Contiene vulnerabilidades comunes del OWASP Top 10 y es ideal para aprender técnicas de pentesting.

- **Nivel**: Principiante a avanzado.

- Vulnerabilidades comunes:

  - Inyección SQL
  - Cross-Site Scripting (XSS)
  - Insecure Direct Object References (IDOR)
  - Seguridad de API

- Instalación:

  - Puedes ejecutarlo como contenedor Docker:

    ```bash
    docker run -d -p 3000:3000 bkimminich/juice-shop
    ```

  - También hay opciones de despliegue en plataformas como Heroku o instalación manual.

- **URL**: [Juice Shop GitHub](https://github.com/juice-shop/juice-shop)

------

### **2. DVWA (Damn Vulnerable Web Application)**

- **Descripción**: Es una aplicación web deliberadamente vulnerable para practicar técnicas de hacking ético. Tiene distintos niveles de dificultad.

- **Nivel**: Principiante a intermedio.

- Vulnerabilidades comunes:

  - Inyección SQL
  - XSS
  - CSRF
  - File Upload
  - Command Injection

- Instalación:

  - Descarga desde GitHub y configúralo en un servidor local (por ejemplo, XAMPP).

    ```bash
    git clone https://github.com/digininja/DVWA.git
    ```

- **URL**: [DVWA GitHub](https://github.com/digininja/DVWA)

------

### **3. Mutillidae**

- **Descripción**: Una aplicación web PHP vulnerable diseñada para enseñar técnicas de pentesting y seguridad de aplicaciones.

- **Nivel**: Principiante a avanzado.

- Vulnerabilidades comunes:

  - OWASP Top 10
  - Manejo inseguro de sesiones
  - Configuraciones inseguras del servidor

- Instalación:

  - Puedes ejecutarlo en una pila LAMP/WAMP/MAMP o como un contenedor Docker.

    ```bash
    docker pull citizenstig/nowasp
    docker run -d -p 80:80 citizenstig/nowasp
    ```

- **URL**: [Mutillidae GitHub](https://github.com/webpwnized/mutillidae)

------

### **4. Hackazon**

- **Descripción**: Una aplicación web vulnerable que simula un sitio web de comercio electrónico moderno con vulnerabilidades tanto tradicionales como avanzadas.
- **Nivel**: Intermedio a avanzado.
- Vulnerabilidades comunes:
  - OWASP Top 10
  - Seguridad de API
  - Escalación de privilegios
- Instalación:
  - Requiere configurar un servidor LAMP. El proyecto está disponible en GitHub.
- **URL**: [Hackazon GitHub](https://github.com/rapid7/hackazon)

------

### **5. bWAPP (Buggy Web Application)**

- **Descripción**: Una aplicación PHP intencionadamente vulnerable que cubre más de 100 tipos de vulnerabilidades.
- **Nivel**: Principiante a avanzado.
- Vulnerabilidades comunes:
  - XSS, CSRF, LFI, RFI, SQLi
  - Seguridad de contraseñas
  - Clickjacking
- Instalación:
  - Requiere un servidor web local con soporte PHP y MySQL.
  - Descarga el archivo `.zip` desde su sitio oficial.
- **URL**: [bWAPP Sitio Oficial](http://www.itsecgames.com/)

------

### **6. VulnHub**

- **Descripción**: Una colección de máquinas virtuales vulnerables diseñadas para practicar pentesting en entornos aislados. Incluye una variedad de niveles de dificultad.
- **Nivel**: Todos los niveles.
- Vulnerabilidades comunes:
  - Explotación de servicios
  - Escalada de privilegios
  - Vulnerabilidades web
- Instalación:
  - Descarga las máquinas virtuales desde su sitio web y ejecútalas en un software de virtualización como VirtualBox o VMware.
- **URL**: [VulnHub](https://www.vulnhub.com/)

------

### **7. Hack The Box (HTB) - Machines y Challenges**

- **Descripción**: Plataforma en línea con máquinas vulnerables que cubren una amplia gama de técnicas de pentesting.
- **Nivel**: Intermedio a avanzado.
- Vulnerabilidades comunes:
  - Exploits personalizados
  - Vulnerabilidades de aplicaciones web
  - Escalación de privilegios
  - Ingeniería inversa
- Instalación:
  - Requiere registrarse en la plataforma y resolver un desafío inicial para acceder.
- **URL**: [Hack The Box](https://www.hackthebox.com/)

------

### **8. Defend the Web (anteriormente HackThis)**

- **Descripción**: Una plataforma en línea que ofrece desafíos prácticos de hacking, incluidos problemas relacionados con aplicaciones web y criptografía.
- **Nivel**: Principiante a intermedio.
- **URL**: [Defend the Web](https://defendtheweb.net/)

------

### **9. OWASP Security Shepherd**

- **Descripción**: Una plataforma diseñada para enseñar y ayudar a los desarrolladores a identificar y mitigar vulnerabilidades de seguridad en aplicaciones.

- **Nivel**: Intermedio.

- Vulnerabilidades comunes:

  - OWASP Top 10
  - Seguridad de aplicaciones móviles

- Instalación:

  - Compatible con Docker.

    ```bash
    docker run -d -p 80:80 owasp/security-shepherd
    ```

- **URL**: [Security Shepherd GitHub](https://github.com/OWASP/SecurityShepherd)

------

### **10. Xtreme Vulnerable Web Application (XVWA)**

- **Descripción**: Una aplicación web vulnerable basada en PHP/MySQL que incluye una variedad de vulnerabilidades como las del OWASP Top 10.

- **Nivel**: Intermedio.

- Vulnerabilidades comunes:

  - SQLi
  - XSS
  - File Inclusion
  - CSRF

- Instalación:

  - Descarga y configúralo en un servidor LAMP/WAMP.

    ```bash
    git clone https://github.com/s4n7h0/xvwa.git
    ```

- **URL**: [XVWA GitHub](https://github.com/s4n7h0/xvwa)

------

### **11. Google Gruyere**

- **Descripción**: Un entorno de práctica proporcionado por Google que permite a los principiantes explorar vulnerabilidades de aplicaciones web en un entorno seguro.
- **Nivel**: Principiante.
- **URL**: [Google Gruyere](https://google-gruyere.appspot.com/)

------

### **12. PentesterLab**

- **Descripción**: Ofrece desafíos prácticos en una variedad de temas relacionados con pruebas de penetración. Aunque tiene contenido gratuito, algunas funciones avanzadas son de pago.
- **Nivel**: Intermedio a avanzado.
- **URL**: [PentesterLab](https://pentesterlab.com/)