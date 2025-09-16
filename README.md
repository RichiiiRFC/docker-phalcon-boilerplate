¡Por supuesto! Aquí tienes el contenido completo formateado directamente para que lo copies y pegues en tu archivo `README.md`.

```markdown
# Entorno de Desarrollo Phalcon con Docker + Nginx + Node.js

Este proyecto contiene un entorno de desarrollo completo basado en Docker para crear y gestionar múltiples aplicaciones web con el framework Phalcon. La arquitectura utiliza Nginx como servidor web, PHP-FPM y un contenedor de Node.js para la gestión del frontend.

## Arquitectura

*   **Servidor Web:** Nginx (configurado para `Virtual Hosts` dinámicos)
*   **PHP:** Imagen oficial de Phalcon (`phalconphp/cphalcon:v5.9.2-php8.4`) con PHP-FPM.
*   **Base de Datos:** MariaDB.
*   **Frontend:** Contenedor con Node.js v18 para gestionar dependencias y procesos de build.
*   **Herramientas Incluidas:** Composer, Phalcon Devtools, NPM/NPX.

---

## Cómo Empezar a Trabajar

Para levantar y usar el entorno, sigue estos pasos.

### 1. Iniciar el Entorno

Abre una terminal (como PowerShell o la terminal de WSL) en la raíz de este proyecto y ejecuta el siguiente comando. Esto levantará todos los contenedores (Nginx, PHP, MariaDB, Node.js) en segundo plano.

```bash
docker-compose up -d
```

### 2. Acceder a un Proyecto Existente

Si ya tienes proyectos en la carpeta `apps/`, simplemente abre tu navegador y ve a la URL que hayas configurado, por ejemplo:
`http://nombre-del-proyecto.local:8080`

---

## Cómo Crear un Nuevo Proyecto Phalcon

Este es el flujo de trabajo para crear una nueva aplicación Phalcon desde cero. Supongamos que queremos crear un proyecto llamado **`mi-tienda`**.

### Paso 1: Entrar a la Terminal del Contenedor PHP

Necesitamos acceder al contenedor donde están instaladas las herramientas de Phalcon.

```bash
docker-compose exec php bash
```

Tu terminal cambiará para mostrar que ahora estás dentro del contenedor, en la ruta `/var/www/html`.

### Paso 2: Crear el Proyecto con Phalcon Devtools

Ejecuta el comando para crear la estructura de archivos del nuevo proyecto.

```bash
phalcon project mi-tienda```

Esto creará una nueva carpeta `mi-tienda` dentro del directorio `apps/`.

### Paso 3: Registrar el Dominio Local en tu PC

Este paso solo se hace **una vez por cada proyecto nuevo**.

1.  Abre **PowerShell como Administrador** en Windows.
2.  Ejecuta el siguiente comando:

    ```powershell
    Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "`n127.0.0.1`tmi-tienda.local" -Force
    ```

### Paso 4: ¡Visita tu Nuevo Proyecto!

Abre tu navegador web y ve a la nueva URL:

**http://mi-tienda.local:8080**

¡Deberías ver la página de bienvenida de Phalcon!

---

## Cómo Usar el Entorno de Node.js

El contenedor de Node.js está diseñado para manejar todas las tareas de frontend, como instalar dependencias, compilar assets (CSS, JavaScript) o ejecutar un servidor de desarrollo (Vite, etc.).

### Paso 1: Entrar a la Terminal del Contenedor Node

Para trabajar con `npm` o `npx`, primero accede al contenedor:

```bash
docker-compose exec node bash
```

Estarás en el directorio `/usr/src/app`, que es un espejo de la carpeta `apps/` de tu máquina.

### Paso 2: Trabajar en tu Proyecto

Una vez dentro, navega al directorio de tu proyecto específico y usa `npm` como lo harías normalmente.

```bash
# Navega a la carpeta de tu proyecto
cd mi-tienda

# Inicializa un proyecto de Node.js (si no tienes un package.json)
npm init -y

# Instala tus dependencias (ejemplo con Vite y Sass)
npm install vite sass

# Ejecuta un script de tu package.json (ej. para compilar los assets)
npm run build
```

Los archivos generados por el proceso de build aparecerán en tu carpeta local `apps/mi-tienda`, listos para ser utilizados por tu aplicación Phalcon.

---

## Comandos Útiles de Docker

*   **Levantar el entorno en segundo plano:**
    ```bash
    docker-compose up -d
    ```

*   **Detener y eliminar los contenedores (libera los puertos):**
    ```bash
    docker-compose down
    ```

*   **Acceder a la terminal de un contenedor:**
    ```bash
    # Para PHP y Phalcon Devtools
    docker-compose exec php bash

    # Para Node.js y NPM
    docker-compose exec node bash
    ```

*   **Ver los logs de los contenedores (muy útil para depurar):**
    ```bash
    docker-compose logs -f
    ```
    Para ver los logs de un servicio específico (ej. PHP):
    ```bash
    docker-compose logs -f php
    ```

*   **Reconstruir las imágenes (solo si modificas un `Dockerfile`):**
    ```bash
    docker-compose build --no-cache
    ```

---

## Conexión a la Base de Datos

*   **Host:** `mariadb` (Este es el nombre del servicio en `docker-compose.yml`)
*   **Puerto:** `3306` (Desde dentro de los contenedores)
*   **Usuario:** `phalcon`
*   **Contraseña:** `phalcon`
*   **Base de Datos:** `phalcon_db`

Para conectar desde una herramienta externa en tu PC (como DBeaver, HeidiSQL o TablePlus):
*   **Host:** `127.0.0.1`
*   **Puerto:** `3307` (Este es el puerto que está expuesto a tu máquina)
```