# Práctica 2.2: Autenticación en Nginx (Parte Servidor)
**Alumno:** David Martínez Alcázar

## 1. Preparación del Entorno
Hemos comenzado clonando el repositorio compartido de la práctica anterior.
Para mantener el flujo de trabajo colaborativo, he creado una rama específica llamada `david-nginx`.

### Clonado del repositorio
Comando utilizado: `git clone https://github.com/mlunlop-iezv/nginx-ll-autenticacion.git`

### Creación de la rama de trabajo
Para aislar mi desarrollo del de mi compañero, creo una nueva rama:
Comando: `git checkout -b david-nginx`

![Captura-1.png]

### Despliegue de la Máquina Virtual
Hemos migrado el archivo de configuración `Vagrantfile` de la práctica anterior a este repositorio.
Para levantar el servidor de desarrollo, ejecutamos el comando: `vagrant up`.

## 2. Instalación y Configuración de Red del Servidor
Al iniciar la máquina virtual nueva, nos encontramos con problemas de conectividad que impedían la descarga de paquetes.

**Solución aplicada:**
Se configuró manualmente el servidor DNS de Google para restablecer la conexión a internet:

**Instalación de Nginx:**
Una vez restablecida la red, procedimos a la instalación del servidor web:
1. `sudo apt update`
2. `sudo apt install nginx -y`

**Verificación:**
Comprobamos que el servicio está activo y funcionando correctamente:

![Captura-2.png)

## 4. Gestión de Usuarios y Contraseñas
Para habilitar la autenticación básica, es necesario tener un archivo de credenciales independiente de la configuración web. Hemos generado este archivo en la ruta `/etc/nginx/.htpasswd`.

**Usuarios creados:**
* `david`
* `martinez`

Hemos utilizado `openssl` con el algoritmo MD5 (`-apr1`) para cifrar las contraseñas, garantizando que no se almacenen en texto plano.

**Comandos ejecutados:**
1.  `sudo sh -c "echo -n 'david:' >> /etc/nginx/.htpasswd"`
2.  `sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"`
3.  *(Proceso repetido para el usuario 'martinez')*

**Verificación:**
Como se observa en la captura, el archivo contiene los nombres de usuario seguidos de sus contraseñas hash:

![Captura-3)
