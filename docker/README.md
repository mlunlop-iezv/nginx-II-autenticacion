# Práctica 2.2: Autenticación en Nginx (Docker)
**Alumno:** Mario Luna López

Este documento detalla el proceso de configuración de seguridad y autenticación básica en un servidor Nginx, desplegado sobre un contenedor Docker.

---

## 1. Preparación 

### 1.1. Gestión de Ramas (Git)
Para mantener un flujo de trabajo ordenado, iniciamos el desarrollo creando una rama específica para esta práctica.
* Comando: `git checkout -b mario-nginx`

![Creación de la rama en Git](img/1.png)

### 1.2. Verificación del Servidor Web
Al trabajar con contenedores, la verificación del servicio se realiza comprobando el estado del contenedor. Verificamos que el contenedor está levantado y escuchando puertos:`docker ps`.

![Estado del servicio Nginx](img/2.png)

---

## 2. Creación de Usuarios y Contraseñas

Necesitamos generar el archivo .htpasswd que guardará las contraseñas encriptadas.

## 2.1. Decargar la herramienta
* Comando: `docker pull stakater/ssl-certs-generator`



**Resultado Final:**

![Web operativa final](img/Captura-15.png)
