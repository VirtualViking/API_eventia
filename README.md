

**Sistema de Gestión de Eventos con Arquitectura Limpia**

[![CI/CD Pipeline](https://github.com/slopez1023/TallerPractico/actions/workflows/ci.yml/badge.svg)]https://github.com/VirtualViking/API_eventia.git
[![Coverage](https://img.shields.io/badge/coverage-50%25-yellow)](./coverage)
[![License](https://img.shields.io/badge/license-ISC-blue.svg)](LICENSE)

---

##  Tabla de Contenidos

- [Descripción General](#-descripción-general)
- [Arquitectura](#-arquitectura)
- [Tecnologías](#-tecnologías)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación](#-instalación)
- [Configuración](#-configuración)
- [Ejecución](#-ejecución)
- [Pruebas](#-pruebas)
- [Pipeline CI/CD](#-pipeline-cicd)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [API Endpoints](#-api-endpoints)
- [Seguridad](#-seguridad)

---

##  Descripción General

**Eventia Core API** es un backend robusto desarrollado con TypeScript y Node.js que permite gestionar eventos, participantes y registros de asistencia. El sistema está diseñado siguiendo principios de Clean Architecture y Domain-Driven Design (DDD), garantizando un código mantenible, testeable y escalable.

### Características Principales

 **Gestión de Eventos**: Crear, actualizar, consultar y eliminar eventos  
 **Gestión de Participantes**: Administrar participantes del sistema  
 **Control de Asistencia**: Registro de participantes en eventos con validación de cupos  
 **Sistema de Caché**: Redis para optimizar consultas frecuentes  
 **Pruebas Automatizadas**: 46 tests (unitarios, integración y E2E) con 50%+ de cobertura  
 **Análisis de Seguridad**: ESLint con reglas de seguridad  
 **CI/CD**: Pipeline automatizado con GitHub Actions  



### Principios Aplicados

- **Separación de Responsabilidades**: Cada capa tiene una responsabilidad única
- **Inversión de Dependencias**: Las capas internas no dependen de las externas
- **Inyección de Dependencias**: Facilita testing y modularidad
- **Domain-Driven Design**: El dominio es el núcleo del sistema

### Justificación de Clean Architecture

Se eligió esta arquitectura porque:
1. **Testabilidad**: Permite testear la lógica de negocio sin dependencias externas
2. **Mantenibilidad**: Cambios en infraestructura no afectan la lógica de negocio
3. **Escalabilidad**: Fácil agregar nuevas funcionalidades sin afectar código existente
4. **Independencia de Frameworks**: La lógica no está acoplada a Express o PostgreSQL

---

##  Tecnologías

### Backend
- **Node.js** v20.x - Runtime JavaScript
- **TypeScript** v5.x - Tipado estático
- **Express** v5.x - Framework web

### Base de Datos
- **PostgreSQL** v17 - Base de datos relacional
- **pg** v8.x - Cliente PostgreSQL

### Caché
- **Redis** v7+ - Sistema de caché en memoria (opcional en desarrollo)
- Implementación alternativa: **Caché en memoria** (desarrollo local)

### Testing
- **Jest** v29.x - Framework de pruebas
- **Supertest** v7.x - Testing de endpoints HTTP
- **ts-jest** v29.x - TypeScript para Jest

### Análisis de Código
- **ESLint** v9.x - Linter JavaScript/TypeScript
- **eslint-plugin-security** - Reglas de seguridad
- **typescript-eslint** - Reglas específicas para TypeScript

### CI/CD
- **GitHub Actions** - Integración y despliegue continuo

---

##  Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- **Node.js** v20.x o superior ([Descargar](https://nodejs.org/))
- **npm** v10.x o superior (incluido con Node.js)
- **PostgreSQL** v15+ ([Descargar](https://www.postgresql.org/download/))
- **Git** ([Descargar](https://git-scm.com/))
- **Redis** (Opcional) - Solo para producción ([Descargar](https://redis.io/download))

### Verificar Instalación

```bash
node --version   # debe mostrar v20.x o superior
npm --version    # debe mostrar v10.x o superior
psql --version   # debe mostrar PostgreSQL 17 o superior
```

---

##  Instalación

### 1. Clonar el Repositorio

```bash
git clone https://github.com/VirtualViking/API_eventia.git
cd TallerPractico
```

### 2. Instalar Dependencias

```bash
npm install
```

### 3. Configurar Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto:

```bash
cp .env.example .env
```

Edita el archivo `.env` con tus credenciales:

```env
# Server Configuration
NODE_ENV=development
PORT=3000
API_PREFIX=/api/v1

# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_NAME=eventia_db
DB_USER=postgres
DB_PASSWORD=tu_contraseña_aqui

# Cache Configuration (opcional)
REDIS_HOST=localhost
REDIS_PORT=6379
CACHE_TYPE=memory  # 'memory' para desarrollo, 'redis' para producción
```

### 4. Configurar Base de Datos

#### Opción A: Crear base de datos manualmente

```bash
# Conectarse a PostgreSQL
psql -U postgres

# Crear base de datos
CREATE DATABASE eventia_db;

# Salir de psql
\q
```

#### Opción B: Usar script de setup (requiere tablas creadas)

```bash
npm run db:setup
```

Ejecuta el archivo `src/infrastructure/database/init.sql` en tu cliente PostgreSQL (pgAdmin, DBeaver, etc.) para crear las tablas:

```sql
-- Ver archivo: src/infrastructure/database/init.sql
-- Este script crea las tablas: events, participants, attendances
```

---

##  Ejecución

### Modo Desarrollo

Inicia el servidor con hot-reload:

```bash
npm run dev
```

El servidor estará disponible en: `http://localhost:3000`

### Modo Producción

1. Compilar TypeScript:

```bash
npm run build
```

2. Iniciar servidor:

```bash
npm start
```

### Verificar que está funcionando

Abre tu navegador o usa curl:

```bash
curl http://localhost:3000/health
```

Respuesta esperada:
```json
{
  "success": true,
  "message": "Eventia Core API está funcionando correctamente",
  "timestamp": "2025-11-14T...",
  "uptime": 123.456,
  "environment": "development"
}
```

---

##  Pruebas

### Ejecutar Todas las Pruebas

```bash
npm test
```

### Pruebas por Tipo

```bash
# Solo pruebas unitarias (26 tests)
npm run test:unit

# Solo pruebas de integración (6 tests)
npm run test:integration

# Solo pruebas E2E (14 tests)
npm run test:e2e

# Modo watch (desarrollo)
npm run test:watch
```

### Cobertura de Código

```bash
npm test
```

El reporte de cobertura se genera en `coverage/lcov-report/index.html`

### Resultados Actuales

 **46/46 tests pasando (100%)**
- 26 tests unitarios
- 6 tests de integración
- 14 tests E2E

 **Cobertura**: 50.73% de líneas, 40.49% de branches

---

##  Análisis Estático de Seguridad

### ESLint con Reglas de Seguridad

```bash
# Ejecutar análisis
npm run lint

# Corregir errores automáticamente
npm run lint:fix
```

### Auditoría de Seguridad de npm

```bash
npm run security:check
```

### Reglas de Seguridad Implementadas

- `security/detect-object-injection` - Detecta inyección de objetos
- `security/detect-unsafe-regex` - Detecta expresiones regulares inseguras
- `security/detect-buffer-noassert` - Detecta uso inseguro de buffers
- `security/detect-eval-with-expression` - Detecta uso de eval()
- `security/detect-possible-timing-attacks` - Detecta posibles ataques de timing

---

##  Pipeline CI/CD

### GitHub Actions Workflow

El proyecto incluye un pipeline automatizado que se ejecuta en cada push o pull request:

#### Pasos del Pipeline

1. ** Instalar dependencias** (`npm ci`)
2. ** Ejecutar pruebas unitarias** (`npm run test:unit`)
3. ** Ejecutar pruebas de integración** (`npm run test:integration`)
4. ** Análisis estático de seguridad** (`npm run lint`)
5. ** Auditoría de seguridad** (`npm audit`)
6. ** Ejecutar pruebas E2E** (`npm run test:e2e`)

#### Estado del Pipeline

Si todos los pasos pasan exitosamente, se imprime:
```
 OK - All tests and checks passed!
```

Si algún paso falla, el pipeline se detiene y marca el estado como `Failed`.

### Ver el Pipeline en Acción

Visita: `https://github.com/slopez1023/TallerPractico/actions`

---


##  API Endpoints

### Base URL

```
http://localhost:3000/api/v1
```

### Eventos

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/events` | Listar todos los eventos |
| GET | `/events/:id` | Obtener un evento por ID |
| POST | `/events` | Crear un nuevo evento |
| PUT | `/events/:id` | Actualizar un evento |
| DELETE | `/events/:id` | Eliminar un evento |

### Participantes

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| GET | `/participants` | Listar todos los participantes |
| GET | `/participants/:id` | Obtener un participante por ID |
| POST | `/participants` | Crear un nuevo participante |
| PUT | `/participants/:id` | Actualizar un participante |
| DELETE | `/participants/:id` | Eliminar un participante |

### Asistencias

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/attendances` | Registrar asistencia a un evento |
| GET | `/attendances/event/:eventId` | Obtener asistencias de un evento |
| DELETE | `/attendances/:id` | Cancelar asistencia |

### Ejemplos de Uso

#### Crear un Evento

```bash
curl -X POST http://localhost:3000/api/v1/events \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Conferencia Tech 2025",
    "description": "Evento de tecnología",
    "date": "2025-12-15T10:00:00Z",
    "location": "Cali, Colombia",
    "capacity": 100
  }'
```

#### Registrar Asistencia

```bash
curl -X POST http://localhost:3000/api/v1/attendances \
  -H "Content-Type: application/json" \
  -d '{
    "eventId": "uuid-del-evento",
    "participantId": "uuid-del-participante"
  }'
``
---

## Desarrollo

### Scripts Disponibles

```bash
npm run dev          # Modo desarrollo con hot-reload
npm run build        # Compilar TypeScript
npm start            # Iniciar servidor en producción
npm test             # Ejecutar todas las pruebas
npm run lint         # Análisis estático
npm run lint:fix     # Corregir errores de linting
npm run db:setup     # Configurar base de datos
```

### Contribuir

1. Fork el repositorio
2. Crea una rama: `git checkout -b feature/nueva-funcionalidad`
3. Commit: `git commit -m 'Agregar nueva funcionalidad'`
4. Push: `git push origin feature/nueva-funcionalidad`
5. Abre un Pull Request

---

