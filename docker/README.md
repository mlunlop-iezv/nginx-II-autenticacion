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
* Comando: `docker run --rm stakater/ssl-certs-generator openssl passwd -apr1 'mi_password'`

<img src="img/3.png" />

### 2.3. Edición del archivo

He creado manualmente el archivo conf/htpasswd pegando los hashes generados para tener dos usuarios válidos:

<img src="img/4.png" />

---

## 3. Configuración Inicial de Nginx

**Resultado Final:**

![Web operativa final](img/Captura-15.png)
