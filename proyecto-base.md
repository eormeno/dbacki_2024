# Taller Proyecto Laravel Base

## Contenido
- [Objetivo](#objetivo)
- [Requisitos](#requisitos)
- [Instalación](#instalación)
- [Actividades](#actividades)

## Objetivo
Contar con un proyecto laravel base con las configuraciones iniciales necesarias para el desarrollo de aplicaciones web.

El proyecto base contiene las siguientes características:

- Laravel 11
- Breeze y Jetstream
- Tailwind CSS
- Spatie Permission
- Debug helper
- Toast notifications
- User management
- Roles and permissions management

## Requisitos
- PHP 8.2 o superior
- Composer
- Node.js 16.13 o superior
- NPM 8.1 o superior
- Tener cuenta en GitHub
- Git instalado
- Tener definidos los datos del proyecto (nombre, descripción, etc.)

> **Nota** :speech_balloon::  Para este taller utilizaré el nombre de una aplicación llamada **TEApp**.

## Instalación
1. Clonar el repositorio
```bash
$ git clone https://github.com/eormeno/base.git teapp
```
2. Ingresar al directorio del proyecto
```bash
$> cd teapp/src
```
3. Instalar las dependencias de PHP y Node.js
```bash
teapp/src> composer install
teapp/src> npm i
```
4. Crear el archivo de configuración `.env`
```bash
teapp/src> cp .env.example .env
```
5. Editar el archivo `.env` con los datos del proyecto, claves de acceso a la base de datos y otros datos necesarios.
```bash
APP_NAME='TEApp'
...
APP_DEBUG=true # true para desarrollo
...
APP_LOCALE=es   # Idioma de la aplicación
APP_FALLBACK_LOCALE=en # Idioma por defecto
APP_FAKER_LOCALE=es_AR  # Idioma para datos falsos
FAKE_USERS_PASSWORD="CHANGE_ME" # Password para usuarios falsos
...
ADMIN_USERNAME="Admin User" # Nombre del usuario administrador
ADMIN_EMAIL=admin@admin.com # Correo del usuario administrador
ADMIN_PASSWORD="CHANGE_ME" # Password del usuario administrador
```
6. Generar la clave de la aplicación
```bash
teapp/src> php artisan key:generate
```
7. Ejecutar las migraciones y los seeders
```bash
teapp/src> php artisan migrate --seed
```
8. Generar el enlace simbólico para las imágenes
```bash
teapp/src> php artisan storage:link
```
9. Iniciar el servidor de desarrollo
```bash
teapp/src> cd ..
teapp> sudo ./start.sh # para Linux
teapp> ./start # para Windows
```
10. Ingresar a la URL `http://127.0.0.1:8000` y verificar que la aplicación se ha instalado correctamente.
11. Ingresar con el usuario administrador que fue configurado en el archivo `.env` y verificar que el sistema funciona correctamente.
## Actividades
1. Crear un nuevo usuario con el rol de administrador.
2. Crear un nuevo rol con los permisos necesarios para gestionar usuarios.
3. Crear un nuevo usuario con el rol creado en el paso anterior.
4. Analizar las partes del código que permiten gestionar los roles y permisos.