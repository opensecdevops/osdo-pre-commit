# Usamos una imagen base de Alpine Linux 3.18 con PHP
FROM composer@sha256:d60d9ec83bab8c52d219e39f6313db93733ea061480ddba9af49b127b7cec4f5 as composer

FROM php@sha256:e1238e39ef14813c5a61075d3165b764f1af075e903cfa7184d2bd0ff151d94b

# Instalamos las dependencias necesarias
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Instalamos PHP_CodeSniffer (PHPCS)
RUN composer global require "squizlabs/php_codesniffer=3.7.2"

# Instalamos PHPStan
RUN composer global require "phpstan/phpstan=1.10.37"

# Instalamos PHPUnit
RUN composer global require "phpunit/phpunit=^9.6"

# Agregamos la ubicación de las herramientas instaladas al PATH
ENV PATH="/root/.composer/vendor/bin:${PATH}"

# Directorio de trabajo
WORKDIR /app

# Comando predeterminado al ejecutar el contenedor
CMD [ "php", "-a" ]
