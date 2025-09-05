
# Gym Routine Planner — MVP

Sistema mínimo viable para gestionar **rutinas de gimnasio asignadas a clientes**.  
Permite a un **Administrador** crear tipos de ejercicio, construir **rutinas** (semanas/días/bloques), asignarlas a clientes con fecha de inicio y revisar **adherencia** y **volumen**.  
El **Cliente** ve su planificación semanal, el detalle del día y registra lo realizado.

## Características (MVP)
- CRUD de **Tipos de ejercicio** (categoría, equipamiento, músculos, notas).
- Editor de **Rutinas**: semanas → días → ejercicios con parámetros (series, reps, descanso, tempo/RIR).
- **Asignación** de rutina a cliente con fecha de inicio (genera sesiones en calendario).
- Registro del **entrenamiento** por parte del cliente (series/reps/peso).
- **Reportes** básicos: adherencia semanal y volumen por ejercicio.

## Tecnologías
- **Backend**: Laravel 11 (PHP 8.2+), MySQL 8+
- **Frontend**: Blade + TailwindCSS
- **Auth**: Laravel Breeze
- **Herramientas**: Composer, Node.js (LTS), npm

## Esquema de datos (resumen)
- `users` (admin/cliente)
- `clients` (perfil del cliente; FK a users)
- `exercise_types`
- `exercises` *(opcional; variaciones del tipo)*  
- `routines` → `routine_weeks` → `routine_days` → `routine_day_exercises`
- `client_plans` (asignación de rutina a cliente)
- `client_sessions` (sesiones en calendario)
- `workout_logs` (registro de sets)
- `client_checkins` *(opcional)*

> Nota: En MVP puedes **omitir `exercises`** y referenciar `exercise_types` directamente desde `routine_day_exercises`.

### Relaciones clave
- `clients.user_id` → `users.id`
- `exercises.exercise_type_id` → `exercise_types.id`
- `routine_weeks.routine_id` → `routines.id`
- `routine_days.routine_week_id` → `routine_weeks.id`
- `routine_day_exercises.routine_day_id` → `routine_days.id`
- `client_plans.client_id` → `clients.id`
- `client_plans.routine_id` → `routines.id`
- `client_sessions.client_plan_id` → `client_plans.id`
- `client_sessions.routine_day_id` → `routine_days.id`
- `workout_logs.client_session_id` → `client_sessions.id`
- `workout_logs.routine_day_exercise_id` → `routine_day_exercises.id`

## Requisitos
- PHP 8.2+, Composer
- MySQL 8+ (o MariaDB 10.6+)
- Node.js LTS + npm
- Extensiones PHP: mbstring, pdo, pdo_mysql, openssl, tokenizer, xml, ctype, json, fileinfo

## Configuración rápida
```bash
# 1) Clonar e instalar dependencias
git clone https://github.com/tu-org/gym-routine-planner.git
cd gym-routine-planner
composer install
npm install

# 2) Variables de entorno
cp .env.example .env
php artisan key:generate

# 3) Configurar BD en .env
# DB_DATABASE=gym_mvp
# DB_USERNAME=...
# DB_PASSWORD=...

# 4) Migrar y sembrar
php artisan migrate --seed

# 5) Compilar assets y levantar server
npm run dev
php artisan serve
