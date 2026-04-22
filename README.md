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