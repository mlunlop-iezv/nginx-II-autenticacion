# Práctica 2.2: Autenticación en Nginx (Docker)
**Alumno:** Mario Luna López

Este documento detalla el proceso de configuración de seguridad y autenticación básica en un servidor Nginx, desplegado sobre un contenedor Docker.

---

## 1. Preparación 

### 1.1. Gestión de Ramas (Git)
Para mantener un flujo de trabajo ordenado, iniciamos el desarrollo creando una rama específica para esta práctica.
* Comando: `git checkout -b mario-nginx`

<img src="img/1.png" />

### 1.2. Verificación del Servidor Web
Al trabajar con contenedores, la verificación del servicio se realiza comprobando el estado del contenedor. Verificamos que el contenedor está levantado y escuchando puertos:`docker ps`.

<img src="img/2.png" />

---

## 2. Creación de Usuarios y Contraseñas

Para habilitar la autenticación, necesito un archivo .htpasswd con las credenciales encriptadas.

### 2.1. Decargar la herramienta
* Comando: `docker pull stakater/ssl-certs-generator`

### 2.2. Generación del archivo htpasswd

He generado el hash para mi usuario "mario" y posteriormente he repetido el proceso para el usuario "luna"

<img src="img/3.png" />

### 2.3. Edición del archivo

He creado manualmente el archivo conf/htpasswd pegando los hashes generados para tener dos usuarios válidos:

<img src="img/4.png" />

---

## 3. Configuración de Nginx

He procedido a configurar el servidor creando mi propio archivo .conf para definir las reglas de acceso.

### 3.1. Archivo conf/mario.test.conf

He configurado el bloque server para proteger todo el directorio raíz con contraseña

<img src="img/5.png" />

### 3.2. Despliegue del contenedor:

He levantado el contenedor, al que he llamado docker, montando los volúmenes necesarios para vincular mis archivos locales.

<img src="img/6.png" />

### Prueba: 

Al acceder a http://localhost:8080, el navegador me solicita unos credenciales para acceder

<img src="img/7.png" />

 > Ahora ingresaremos los creedecniales correcto usuario 'mario' contraseña 'mario123'

<img src="img/8.png" />

---

## 4 Tareas

###4.1

**Resultado Final:**

![Web operativa final](img/Captura-15.png)
