# Entorno de Desarrollo Phalcon con Docker + Nginx

Este proyecto contiene un entorno de desarrollo completo basado en Docker para crear y gestionar múltiples aplicaciones web con el framework Phalcon. La arquitectura utiliza Nginx como servidor web y PHP-FPM con la imagen oficial de Phalcon.

## Arquitectura

*   **Servidor Web:** Nginx (configurado para `Virtual Hosts` dinámicos)
*   **PHP:** Imagen oficial de Phalcon (`phalconphp/cphalcon:v5.9.2-php8.4`) con PHP-FPM.
*   **Base de Datos:** MariaDB.
*   **Herramientas Incluidas:** Composer, Phalcon Devtools.

---

## Cómo Empezar a Trabajar

Para levantar y usar el entorno, sigue estos pasos.

### 1. Iniciar el Entorno

Abre una terminal (como PowerShell o la terminal de WSL) en la raíz de este proyecto y ejecuta el siguiente comando. Esto levantará todos los contenedores (Nginx, PHP, MariaDB) en segundo plano.

```bash
docker-compose up -d
```

### 2. Acceder a un Proyecto Existente

Si ya tienes proyectos en la carpeta `apps/`, simplemente abre tu navegador y ve a la URL que hayas configurado, por ejemplo:
`http://nombre-del-proyecto.local:8080`

---

## Cómo Crear un Nuevo Proyecto

Este es el flujo de trabajo completo para crear una nueva aplicación Phalcon desde cero. Supongamos que queremos crear un proyecto llamado **`mi-tienda`**.

### Paso 1: Entrar a la Terminal del Contenedor PHP

Necesitamos acceder al contenedor donde están instaladas las herramientas de Phalcon.

```bash
docker-compose exec php bash
```

Tu terminal cambiará para mostrar que ahora estás dentro del contenedor, en la ruta `/var/www/html`.

### Paso 2: Crear el Proyecto con Phalcon Devtools

Ejecuta el comando para crear la estructura de archivos del nuevo proyecto.

```bash
phalcon project mi-tienda
```

Esto creará una nueva carpeta `mi-tienda` dentro del directorio `apps/`.

### Paso 3: Registrar el Dominio Local en tu PC

Este paso solo se hace **una vez por cada proyecto nuevo**. Necesitas decirle a tu ordenador que el dominio `mi-tienda.local` apunta a tu máquina.

1.  Abre **PowerShell como Administrador** en Windows.
2.  Ejecuta el siguiente comando:

    ```powershell
    Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "`n127.0.0.1`tmi-tienda.local" -Force
    ```

### Paso 4: ¡Visita tu Nuevo Proyecto!

Abre tu navegador web y ve a la nueva URL.

**http://mi-tienda.local:8080**

¡Deberías ver la página de bienvenida de Phalcon! Tu nuevo proyecto está listo para que empieces a programar.

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

## Conexión a la Base de Datos

*   **Host:** `mariadb` (Este es el nombre del servicio en `docker-compose.yml`)
*   **Puerto:** `3306` (Desde dentro de los contenedores)
*   **Usuario:** `phalcon`
*   **Contraseña:** `phalcon`
*   **Base de Datos:** `phalcon_db`

Para conectar desde una herramienta externa en tu PC (como DBeaver, HeidiSQL o TablePlus):
*   **Host:** `127.0.0.1`
*   **Puerto:** `3307` (Este es el puerto que está expuesto a tu máquina)