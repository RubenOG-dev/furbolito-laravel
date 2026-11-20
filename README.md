# Furbolito — gestión de equipos, jugadores y torneos

Aplicación web en Laravel para registrar equipos de fútbol, sus jugadores y los torneos en los que participan. Un mismo equipo puede jugar varios torneos, y cada participación queda registrada con su año.

> **Estado:** en desarrollo. Es un CRUD funcional con vistas Blade, pero no tiene validación de formularios ni pruebas automatizadas.

## Funcionalidades

- **Equipos:** alta y listado.
- **Jugadores:** alta y listado de los jugadores de cada equipo.
- **Torneos:** alta de torneos, con la opción de marcarlos como nacionales.
- **Participación por año:** asignar equipos a un torneo y consultar qué equipos participaron en un torneo en un año concreto.

## Modelo de datos

- `Equipo` tiene muchos `Jugador` (relación 1:N).
- `Equipo` y `Torneo` están relacionados N:M a través de la tabla pivote `equipo_torneo`, que guarda el año de participación.

Las migraciones están en `database/migrations/`.

## Tecnologías

- PHP 8.2 y Laravel 11
- Eloquent ORM y migraciones
- Blade para las vistas
- Vite y Tailwind CSS para los estilos
- MySQL (con Docker) o SQLite (por defecto)

## Puesta en marcha

Requiere PHP 8.2 o superior, Composer y Node.js.

```bash
composer install
cp .env.example .env
php artisan key:generate
```

Por defecto, `.env.example` usa SQLite. Para usarlo, crea el fichero vacío y migra:

```bash
touch database/database.sqlite
php artisan migrate
npm install
npm run build
php artisan serve
```

Abre `http://127.0.0.1:8000`.

### Con MySQL y Docker

El `docker-compose.yml` es el de Laravel Sail (PHP 8.4, MySQL 8, Redis, Mailpit y Meilisearch). Las credenciales se leen del `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=furbolito
DB_USERNAME=sail
DB_PASSWORD=cambiar-esta-contrasena
```

Después, `./vendor/bin/sail up -d` y `./vendor/bin/sail artisan migrate`.

## Rutas principales

| Ruta | Función |
|---|---|
| `/equipo/listar` | Listado de equipos |
| `/equipo/formAlta` | Formulario de alta de equipo |
| `/jugador/{id_equipo}/jugadores` | Jugadores de un equipo |
| `/jugadores/crear` | Formulario de alta de jugador (POST) |
| `/torneo/formAlta` | Formulario de alta de torneo |
| `/torneo/filterAnho` | Selección de torneo y año |

## Mejoras pendientes

- Las altas de equipo y de torneo se hacen con `GET`. Lo correcto es usar `POST` con token CSRF, porque una petición `GET` no debe modificar datos.
- Faltan reglas de validación (`$request->validate(...)`) en los controladores.
- Los tests son los de ejemplo de Laravel.
