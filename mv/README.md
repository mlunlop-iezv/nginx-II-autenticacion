# Práctica 2.2: Autenticación en Nginx (Parte Servidor)
**Alumno:** David Martínez Alcázar
**Asignatura:** Despliegue de Aplicaciones Web

## 1. Preparación del Entorno
Hemos comenzado clonando el repositorio compartido de la práctica anterior. Para mantener el flujo de trabajo colaborativo y no interferir con la parte de Docker de mi compañero, he creado una rama específica.

### Creación de la rama de trabajo
Comando utilizado: `git checkout -b david-nginx`

![Creación de rama](Captura-1.png)

### Despliegue de la Máquina Virtual
Hemos migrado el archivo de configuración `Vagrantfile` y la carpeta `html` de la práctica anterior a este repositorio. Para levantar el servidor, ejecutamos `vagrant up`.

---

## 2. Instalación y Configuración de Red
Al iniciar la máquina virtual nueva (Debian Bullseye), realizamos la instalación del servidor Nginx. Previamente, fue necesario configurar el DNS (`nameserver 8.8.8.8`) para asegurar la conectividad y poder descargar los paquetes.

**Comandos:**
1. `sudo apt update`
2. `sudo apt install nginx -y`

**Verificación:**
Comprobamos que el servicio está activo (`running`):

![Estado de Nginx activo](Captura-2.png)

---

## 3. Gestión de Usuarios y Contraseñas
Para habilitar la autenticación básica, generamos un archivo de credenciales independiente en `/etc/nginx/.htpasswd`.

**Usuarios creados:**
* `david`
* `martinez`

Hemos utilizado `openssl` con el algoritmo MD5 (`-apr1`) para cifrar las contraseñas.

**Verificación del fichero cifrado:**
Como se observa en la captura, el archivo contiene los usuarios y sus hashes:

![Creación de usuarios htpasswd](Captura-3.png)

---

## 4. Configuración del Virtual Host
Editamos el archivo de configuración `/etc/nginx/sites-available/default` para aplicar dos cambios fundamentales:
1.  **Ruta del sitio:** Cambiamos `root` a `/vagrant/html` para servir nuestra web compartida.
2.  **Seguridad:** Añadimos `auth_basic` y `auth_basic_user_file` dentro del bloque `location /`.

**Validación de sintaxis:**
Antes de reiniciar, comprobamos que la configuración es correcta con `sudo nginx -t`:

![Test de configuración Nginx](Captura-4.png)

---

## 5. Verificación y Pruebas de Acceso
Tras reiniciar el servicio (`sudo systemctl restart nginx`), realizamos la batería de pruebas desde el navegador de la máquina anfitriona (Mac).

**Prueba 1: Solicitud de credenciales**
Al intentar acceder a la IP del servidor (`192.168.56.8`), Nginx intercepta la petición y muestra la ventana de autenticación:

![Ventana de Login](Captura-5.png)

**Prueba 2: Error de Autenticación (401)**
Si cancelamos el login o introducimos credenciales erróneas, el servidor deniega el acceso con un error **401 Authorization Required**:

![Error 401](Captura-6.png)

**Prueba 3: Acceso Concedido**
Finalmente, al introducir las credenciales correctas del usuario `david`, el servidor autoriza la entrada y carga la web "Perfect Learn":

![Acceso exitoso a la web](Captura-7.jpg)

## 7. Análisis de Logs (Tarea 2.1)
Siguiendo las instrucciones, hemos verificado cómo registra Nginx los intentos de acceso. Hemos filtrado el log para excluir las imágenes y ver claramente los códigos de estado.

**Evidencia de Logs:**
En la captura adjunta podemos observar:

1.  **En `access.log` (Parte superior):**
    * **Código 401:** Intento de acceso denegado ("Authorization Required").
    * **Código 200:** Acceso exitoso del usuario `david`.
2.  **En `error.log` (Parte inferior):**
    * Mensaje técnico: `user "david": password mismatch`.

![Logs filtrados mostrando error 401 y éxito 200](Captura-8.png)
