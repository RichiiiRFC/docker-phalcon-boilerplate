# Entorno de Desarrollo Phalcon con Docker 🐳

Un entorno de desarrollo completo basado en Docker para crear y gestionar múltiples aplicaciones web con el framework Phalcon. La arquitectura utiliza Nginx como servidor web, PHP-FPM y un contenedor de Node.js para la gestión del frontend.

## 🏗️ Arquitectura

- **Servidor Web:** Nginx (configurado para Virtual Hosts dinámicos)
- **PHP:** Imagen oficial de Phalcon (`phalconphp/cphalcon:v5.9.2-php8.4`) basada en PHP-FPM
- **Base de Datos:** MariaDB
- **Frontend:** Contenedor con Node.js v18 para gestionar dependencias y procesos de build
- **Herramientas Incluidas:** Composer, Phalcon Devtools, NPM/NPX
- **Configuración:** Virtual Hosts automáticos con Nginx para proyectos `*.local`

## 🚀 Inicio Rápido

### 1. Crear la Carpeta de Proyectos

Primero, crea la carpeta donde estarán tus aplicaciones en la raíz del proyecto:

```bash
mkdir apps
```

### 2. Levantar el Entorno

**Desde la raíz del proyecto** (donde está el `docker-compose.yml`), abre una terminal y ejecuta:

```bash
docker-compose up -d
```

Este comando levantará todos los contenedores (Nginx, PHP-FPM, MariaDB, Node.js) en segundo plano.

## 📦 Crear un Nuevo Proyecto Phalcon

### Ejemplo: Crear un proyecto llamado `mi-tienda`

#### Paso 1: Acceder al Contenedor PHP

```bash
docker-compose exec php bash
```

#### Paso 2: Crear el Proyecto

```bash
phalcon project mi-tienda
```

Esto creará una nueva carpeta `mi-tienda` dentro del directorio `apps/`.

#### Paso 3: Registrar el Dominio Local (Solo una vez por proyecto)

Aunque Nginx maneja los dominios `*.local` automáticamente, tu sistema operativo necesita saber que estos dominios apuntan a tu máquina local.

**En Windows (PowerShell como Administrador):**

```powershell
Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "`n127.0.0.1 mi-tienda.local" -Force
```

**En Linux/macOS:**

```bash
echo "127.0.0.1 mi-tienda.local" | sudo tee -a /etc/hosts
```

#### Paso 4: Visitar el Proyecto

Abre tu navegador y visita:

```
http://mi-tienda.local:8080
```

¡Deberías ver la página de bienvenida de Phalcon! 🎉

> **💡 Para proyectos existentes:** Si ya tienes proyectos en la carpeta `apps/`, simplemente agrega el dominio al archivo `hosts` (paso 3) y visita `http://nombre-del-proyecto.local:8080`. Gracias a la configuración de Nginx, cualquier dominio `.local` se dirige automáticamente a la carpeta correspondiente.

## 🛠️ Trabajar con Node.js

### Acceder al Contenedor Node

```bash
docker-compose exec node bash
```

### Ejemplo de Uso

```bash
# Navegar a tu proyecto
cd mi-tienda

# Inicializar package.json
npm init -y

# Instalar dependencias
npm install vite sass

# Ejecutar scripts
npm run build
```

Los archivos generados aparecerán en `apps/mi-tienda` y estarán disponibles para tu aplicación Phalcon.

## 🗄️ Conexión a la Base de Datos

### Desde los Contenedores

- **Host:** `mariadb`
- **Puerto:** `3306`
- **Usuario:** `phalcon`
- **Contraseña:** `phalcon`
- **Base de Datos:** `phalcon_db`

### Desde tu Máquina Local

- **Host:** `127.0.0.1`
- **Puerto:** `3307`
- **Usuario:** `phalcon`
- **Contraseña:** `phalcon`
- **Base de Datos:** `phalcon_db`

## 📋 Comandos Útiles

### Gestión de Contenedores

```bash
# Levantar el entorno
docker-compose up -d

# Detener y eliminar contenedores
docker-compose down

# Ver logs de todos los servicios
docker-compose logs -f

# Ver logs de un servicio específico
docker-compose logs -f php

# Reconstruir imágenes
docker-compose build --no-cache
```

### Acceso a Terminales

```bash
# Terminal PHP/Phalcon
docker-compose exec php bash

# Terminal Node.js
docker-compose exec node bash

# Terminal MariaDB
docker-compose exec mariadb mysql -u phalcon -p
```

## 🔧 Estructura del Proyecto

```
tu-carpeta-phalcon/         # Carpeta raíz del proyecto
├── apps/                   # Tus aplicaciones Phalcon (crear manualmente)
│   ├── proyecto1/
│   └── proyecto2/
├── docker/
│   ├── nginx/
│   │   ├── Dockerfile      # Imagen personalizada de Nginx
│   │   └── vhosts.conf     # Configuración de Virtual Hosts
│   ├── php/
│   │   └── Dockerfile      # Imagen personalizada de PHP + Phalcon
│   └── node/               # (Usa imagen oficial de Node.js 18)
├── docker-compose.yml      # Configuración de servicios
├── .gitignore             # apps/ está excluido del repositorio
└── README.md
```

> **📁 Nota importante:** La carpeta `apps/` no se incluye en el repositorio porque es donde desarrollas tus proyectos personales. Debes crearla manualmente **en la raíz del proyecto** antes de usar el entorno.

## 🐛 Solución de Problemas

### El proyecto no se ve en el navegador
1. **Verifica el archivo hosts:** El dominio debe apuntar a 127.0.0.1 (Nginx maneja la redirección automáticamente, pero el SO necesita resolver el dominio)
2. **Contenedores activos:** `docker-compose ps` - todos deben estar "Up"
3. **Logs de Nginx:** `docker-compose logs nginx`
4. **Estructura de carpetas:** Verifica que tu proyecto esté en `apps/nombre-proyecto/public/index.php`

### Error de permisos en archivos
```bash
# Desde el contenedor PHP
docker-compose exec php chown -R www-data:www-data /var/www/html/apps/tu-proyecto

# O desde tu máquina (Linux/macOS)
sudo chown -R $(id -u):$(id -g) ./apps/tu-proyecto
```

### La base de datos no conecta
1. **Estado de MariaDB:** `docker-compose ps` - debe estar "Up"
2. **Host correcto:** Usa `mariadb` como host desde los contenedores, **no** `localhost` o `127.0.0.1`
3. **Puerto correcto:** `3306` desde contenedores, `3307` desde tu máquina
4. **Credenciales:** Usuario: `phalcon`, Contraseña: `phalcon`, Base de datos: `phalcon_db`

### Problemas con Node.js
```bash
# Si el contenedor Node no responde
docker-compose restart node

# Para instalar dependencias globalmente
docker-compose exec node npm install -g tu-paquete
```

---

**¿Tienes problemas?** Abre un [issue](../../issues) y te ayudaremos 😊