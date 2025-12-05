# Práctica 2.2: Autenticación en Nginx (Parte Servidor)
**Alumno:** David Martínez Alcázar
**Asignatura:** Despliegue de Aplicaciones Web

Este documento detalla el proceso de configuración de seguridad, autenticación básica y control de acceso por IP en un servidor Nginx, desplegado sobre una máquina virtual Vagrant/Debian.

---

## 1. Preparación del Entorno
El proyecto parte de un repositorio compartido. Para mantener un flujo de trabajo colaborativo limpio y no interferir con la parte de Docker de mi compañero, se ha creado una rama de trabajo específica.

### 1.1. Gestión de Ramas (Git)
Hemos clonado el repositorio y creado la rama `david-nginx`.
* Comando: `git checkout -b david-nginx`

![Creación de la rama en Git](Captura-1.png)

### 1.2. Despliegue de la Infraestructura
Se ha migrado el archivo `Vagrantfile` y la carpeta de contenido web `html` de la práctica anterior.
Tras iniciar la máquina (`vagrant up`), detectamos un problema de conectividad que impedía la instalación de paquetes.
* **Solución:** Configuración manual del DNS (`echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf`).

Una vez restablecida la red, instalamos el servidor web:
`sudo apt update && sudo apt install nginx -y`

**Verificación del servicio:**
Comprobamos que Nginx está activo y corriendo:

![Estado del servicio Nginx](Captura-2.png)

---

## 2. Gestión de Usuarios y Contraseñas
Para habilitar la autenticación básica (`Basic Auth`), es necesario generar un archivo de credenciales independiente de la configuración web. Hemos creado este archivo en `/etc/nginx/.htpasswd`.

**Procedimiento:**
Utilizamos la herramienta `openssl` para crear usuarios con contraseñas cifradas (MD5).
1. Usuario **david**: `sudo sh -c "echo -n 'david:' >> /etc/nginx/.htpasswd"`
2. Usuario **martinez**: `sudo sh -c "echo -n 'martinez:' >> /etc/nginx/.htpasswd"`
3. Cifrado: `openssl passwd -apr1`

**Evidencia del fichero generado:**
Como se observa, las contraseñas están hasheadas y no en texto plano:

![Creación de usuarios con OpenSSL](Captura-3.png)

---

## 3. Configuración del Virtual Host
Editamos el archivo `/etc/nginx/sites-available/default` para apuntar a nuestra web y aplicar la seguridad.

**Correcciones realizadas:**
1.  **Ruta Web (`root`):** Modificamos la ruta por defecto a `/vagrant/html`, asegurando que Nginx sirva los archivos de la carpeta compartida desde la máquina anfitriona.
2.  **Validación:** Antes de aplicar cambios, verificamos la sintaxis con `sudo nginx -t`.

![Test de configuración Nginx](Captura-4.png)

---

## 4. Pruebas de Acceso y Logs (Tarea 2.1)
Configuramos el bloque `location /` para requerir autenticación en todo el sitio.

### 4.1. Verificación en Navegador
Al acceder a la IP del servidor (`192.168.56.8`), se solicita usuario y contraseña:

![Ventana de Login](Captura-5.png)

* **Intento Fallido:** Si introducimos credenciales erróneas, el servidor responde con un error **401 Authorization Required**.

![Error 401](Captura-6.png)

* **Acceso Exitoso:** Al introducir las credenciales correctas, accedemos a la web "Perfect Learn".

![Acceso correcto a la web](Captura-7.jpg)

### 4.2. Análisis de Logs
Hemos inspeccionado los registros del sistema para corroborar los eventos.
* **Log de Errores:** Muestra `user "david": password mismatch` (fallo de contraseña).
* **Log de Acceso:** Muestra el código **401** (intento fallido) seguido del código **200** (éxito).

![Evidencia de logs 401 y 200](Captura-8.png)

---

## 5. Protección Selectiva de Recursos (Tarea 2.2)
Modificamos la configuración para que el sitio sea público, protegiendo **únicamente** la página de contacto.

**Configuración aplicada:**
* `location /`: Acceso libre (eliminamos `auth_basic`).
* `location /contact.html`: Acceso restringido (añadimos `auth_basic`).

**Pruebas:**
1.  **Home (Pública):** Carga directamente sin pedir contraseña.
    ![Home pública](Captura-9.jpg)

2.  **Contacto (Privada):** Al navegar a esta sección, salta la ventana de autenticación.
    ![Login solo en contacto](Captura-10.jpg)

---

## 6. Restricción por IP (Tarea 3.1)
Configuramos Nginx para denegar el acceso explícitamente a la IP de mi máquina anfitriona (`192.168.56.1`), simulando un bloqueo de seguridad.

**Código añadido:**
```nginx
deny 192.168.56.1;
