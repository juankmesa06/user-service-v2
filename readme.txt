Microservicio de Usuarios
=========================

Este microservicio gestiona:
- Registro de usuarios
- Autenticación (login)
- Perfil del usuario autenticado
- Cierre de sesión (logout)

Requisitos
----------
- PHP >= 8.2
- Composer
- MySQL
- Laravel >= 12
- Postman (para pruebas API)

Instalación
-----------
1. Clonar o crear el microservicio:
   composer create-project laravel/laravel user-service

2. Crear la base de datos en MySQL Workbench:
   CREATE DATABASE user_service;

3. Configurar el archivo .env:
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=user_service
   DB_USERNAME=root
   DB_PASSWORD=1234

4. Instalar Sanctum:
   composer require laravel/sanctum
   php artisan vendor:publish --provider="Laravel\Sanctum\SanctumServiceProvider"
   php artisan migrate

5. Configurar el modelo User.php:
   use Laravel\Sanctum\HasApiTokens;

   class User extends Authenticatable
   {
       use HasApiTokens, HasFactory, Notifiable;

       // ...
   }

6. Ejecutar el servidor:
   php artisan serve

Endpoints
---------
Registro de Usuario
- URL: POST /api/register
- Body (JSON):
{
    "name": "Juan Perez",
    "email": "juan@example.com",
    "password": "password123",
    "password_confirmation": "password123"
}

Login de Usuario
- URL: POST /api/login
- Body (JSON):
{
    "email": "juan@example.com",
    "password": "password123"
}
- Respuesta: Devuelve token de acceso.

Consultar Perfil (requiere token)
- URL: GET /api/profile
- Headers:
  Authorization: Bearer {token}

Logout (requiere token)
- URL: POST /api/logout
- Headers:
  Authorization: Bearer {token}

Cómo probar en Postman
----------------------
1. Registrar un usuario.
2. Hacer login con el usuario registrado.
3. Copiar el token devuelto.
4. Enviar el token en la cabecera Authorization con prefijo Bearer.
5. Consultar el perfil y cerrar sesión con el token válido.

Notas
-----
- Si ocurre un error 500, asegúrate de enviar el token correctamente.
- Si las migraciones fallan, revisa que no existan tablas duplicadas.
- Si cambias configuraciones en .env, limpia la caché con:
  php artisan config:clear
  php artisan cache:clear
