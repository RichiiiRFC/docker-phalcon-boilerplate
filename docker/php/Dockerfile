# 1. USAMOS LA IMAGEN EXACTA QUE TÚ ME DISTE.
FROM phalconphp/cphalcon:v5.9.2-php8.4

# Cambiamos temporalmente al usuario ROOT para poder instalar paquetes y configurar permisos.
USER root

# 2. INSTALAR LAS HERRAMIENTAS DE DESARROLLO BÁSICAS
RUN apt-get update && apt-get install -y \
    wget \
    unzip \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# 3. INSTALAR COMPOSER
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 4. CONFIGURAR EL PATH GLOBAL DE COMPOSER
ENV COMPOSER_HOME /var/www/.composer
ENV PATH="${PATH}:/var/www/.composer/vendor/bin"

# 5. PREPARAR EL DIRECTORIO DE COMPOSER ANTES DE CAMBIAR DE USUARIO
RUN mkdir -p /var/www/.composer && chown -R www-data:www-data /var/www/.composer

# 6. CAMBIAR AL USUARIO EXISTENTE ('www-data') PARA INSTALAR LAS DEVTOOLS
USER www-data

# 7. INSTALAR PHALCON DEVTOOLS GLOBALMENTE
RUN composer global require phalcon/devtools:"^5.0@dev"


# 8. VOLVER A ROOT PARA AJUSTAR PERMISOS FINALES
USER root

# 9. ASEGURAR PERMISOS DE LA CARPETA DE TRABAJO
RUN chown -R www-data:www-data /var/www

# 10. Establecer el directorio de trabajo
WORKDIR /var/www/html