# Instructoria - Guia

Este proyecto usa Laravel + Docker + MySQL + JWT.

La forma recomendada para correrlo es con Docker Compose.

## Requisitos

- Docker Desktop
- Git

## 1) Clonar el repositorio

```bash
git clone https://github.com/alejandrorh97/instructoria
cd instructoria
```

## 2) Crear archivo .env desde .env.example

Linux/macOS:

```bash
cp .env.example .env
```

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

## 3) Levantar contenedores

Linux/macOS:

```bash
docker compose up -d
```

Windows PowerShell:

```powershell
docker compose up -d
```

Nota: no necesitas poner nombres de contenedor manuales.
Los comandos usan el nombre del servicio laravel.test definido en compose.yaml.

## 4) Instalar dependencias PHP dentro del contenedor

Linux/macOS:

```bash
docker compose exec laravel.test composer install
```

Windows PowerShell:

```powershell
docker compose exec laravel.test composer install
```

## 5) Generar llaves de Laravel y JWT

Linux/macOS:

```bash
docker compose exec laravel.test php artisan key:generate
docker compose exec laravel.test php artisan jwt:secret --force
```

Windows PowerShell:

```powershell
docker compose exec laravel.test php artisan key:generate
docker compose exec laravel.test php artisan jwt:secret --force
```

## 6) Crear tablas en base de datos

Primera vez:

Linux/macOS:

```bash
docker compose exec laravel.test php artisan migrate
```

Windows PowerShell:

```powershell
docker compose exec laravel.test php artisan migrate
```

Si quieres reiniciar todo desde cero:

Linux/macOS:

```bash
docker compose exec laravel.test php artisan migrate:fresh
```

Windows PowerShell:

```powershell
docker compose exec laravel.test php artisan migrate:fresh
```

## 7) Abrir el proyecto

- App: http://localhost:8080
- API Docs (tipo Swagger): http://localhost:8080/docs/api
- OpenAPI JSON: http://localhost:8080/docs/api.json

## 8) Probar endpoints protegidos

1. Hacer login en POST /api/auth/login desde docs.
2. Copiar access_token.
3. Usar Bearer Token en las pruebas de endpoints.

## 9) Uso del CRUD (paso a paso)

Regla simple:

- Los endpoints de auth no llevan token.
- El resto de endpoints si llevan Authorization: Bearer TU_TOKEN.

### Paso 1: crear usuario de acceso

- POST /api/auth/register
- Body:

```json
{
	"first_name": "Ana",
	"last_name": "Lopez",
	"email": "ana@example.com",
	"password": "12345678",
	"password_confirmation": "12345678"
}
```

### Paso 2: hacer login y copiar token

- POST /api/auth/login
- Body:

```json
{
	"email": "ana@example.com",
	"password": "12345678"
}
```

### Paso 3: crear categorias y autores

- POST /api/categorias

```json
{
	"nombre": "Programacion"
}
```

- POST /api/autores

```json
{
	"nombre": "Gabriel Garcia Marquez"
}
```

### Paso 4: crear libros

- POST /api/libros

```json
{
	"titulo": "Cien Anos de Soledad",
	"año_publicacion": 1967,
	"id_categoria": 1,
	"autores": [1]
}
```

### Paso 5: crear prestamos

- POST /api/prestamos

```json
{
	"id_usuario": 1,
	"fecha_prestamo": "2026-04-22",
	"fecha_devolucion": "2026-04-29",
	"libros": [1]
}
```

### Paso 6: listar, editar y eliminar

- Listar:
	- GET /api/categorias
	- GET /api/autores
	- GET /api/libros
	- GET /api/prestamos
- Editar:
	- PUT /api/categorias/{categoria}
	- PUT /api/autores/{autor}
	- PUT /api/libros/{libro}
	- PUT /api/prestamos/{prestamo}
- Eliminar:
	- DELETE /api/categorias/{categoria}
	- DELETE /api/autores/{autor}
	- DELETE /api/libros/{libro}
	- DELETE /api/prestamos/{prestamo}

## Comandos utiles

Ver contenedores:

Linux/macOS:

```bash
docker compose ps
```

Windows PowerShell:

```powershell
docker compose ps
```

Ver logs de app:

Linux/macOS:

```bash
docker compose logs -f laravel.test
```

Windows PowerShell:

```powershell
docker compose logs -f laravel.test
```

Limpiar cache Laravel si algo no refleja cambios:

Linux/macOS:

```bash
docker compose exec laravel.test php artisan optimize:clear
```

Windows PowerShell:

```powershell
docker compose exec laravel.test php artisan optimize:clear
```

## Errores comunes

1. Error de DB host mysql no encontrado.
	- Pasa cuando corres php artisan migrate en host Windows.
	- Solucion: correr siempre artisan dentro del contenedor.

2. Error SecretMissingException de JWT.
	- Falta JWT_SECRET en .env.
	- Solucion: ejecutar php artisan jwt:secret --force dentro del contenedor.

## Subir a repositorio publico

Antes de subir:

- No subir .env
- Si por error subiste secretos, regenerarlos

Comandos:

```bash
php artisan serve
```