# MiniMo — Plan de Arquitectura Técnica

## Stack técnico

| Capa | Tecnología | Razón |
|------|-----------|-------|
| **Mobile** | Flutter | Nativo iOS + Android desde un solo código |
| **Backend** | Node.js + Hono | Ligero, TypeScript, ya referenciado en RuFlo |
| **Base de datos** | PostgreSQL (Railway managed) | Relacional, ideal para calendario + progresión |
| **ORM** | Drizzle | Type-safe, migraciones SQL limpias, zero runtime |
| **Cache** | Redis (Railway) | Sesiones, resúmenes frecuentes |
| **State management** | Riverpod | Type-safe, ideal para features modulares |
| **Auth** | JWT + social login (Google/Apple) | Target demográfico 15-27 |
| **IA** | RuFlo MCP Bridge | Ya integrado, para recomendaciones personalizadas |
| **Deploy** | Railway | Backend + PostgreSQL + Redis |

---

## Estructura del monorepo

```
MiniMo/
  (RuFlo existente - intacto)
  minimo/
    backend/          ← API Hono + Drizzle + PostgreSQL
    app/              ← Flutter project
    docs/             ← Specs y documentación
    infra/            ← Docker, Railway config
```

---

## Plan de implementación por fases

### Fase 1: Foundation (Semanas 1-3)
- Inicializar proyecto Flutter (`minimo/app/`)
- Inicializar backend Hono (`minimo/backend/`)
- Auth (registro, login, JWT refresh)
- Onboarding UI conectado a auth
- App shell con bottom navigation (4 tabs + placeholder)
- Deploy a Railway con PostgreSQL managed
- **Entregable**: Usuario puede registrarse, hacer login, ver las 4 tabs

### Fase 2: Calendario (Semanas 4-6)
- CRUD de eventos con recurrencia
- CRUD de tareas con completado
- Balance trabajo/ocio
- Calendar view (mes/semana/día) con `table_calendar`
- **Entregable**: Calendario funcional con tareas

### Fase 3: Diversificador (Semanas 7-9)
- Habit logging y emotion tracking
- Queries de agregación (diario/semanal/mensual)
- Motor básico de correlación
- Charts con `fl_chart`
- **Entregable**: Usuarios pueden registrar hábitos/emociones y ver insights

### Fase 4: Gamificación (Semanas 10-12)
- Sistema de XP (puntos por logging, tareas completadas, streaks)
- Niveles y desbloqueo de personajes
- Achievements y goals
- Animaciones Lottie en level-ups
- **Entregable**: Loop de gamificación completo

### Fase 5: IA + Polish (Semanas 13-16)
- Servicio IA vía RuFlo MCP bridge
- Recomendaciones personalizadas de hábitos
- Push notifications (FCM)
- Settings completo (perfil, preferencias, export, delete account GDPR)
- **Entregable**: Experiencia personalizada con IA

### Fase 6: Social (Semanas 17-20)
- Comunidades y challenges
- Leaderboards
- Moderación básica
- **Entregable**: Features sociales

---

## Base de datos (entidades principales)

- `users` — Perfil, onboarding data (JSONB)
- `calendar_events` — Eventos con recurrencia RFC 5545
- `tasks` — Tareas vinculadas a eventos
- `habit_logs` — Registro de hábitos (time-series)
- `emotion_entries` — Registro emocional (time-series)
- `character_progress` — Nivel, XP, personajes desbloqueados
- `goals` — Metas con progreso
- `rewards` — Recompensas ganadas

---

## API REST

Base: `/api/v1`

- `POST /auth/register`, `/auth/login`, `/auth/refresh`, `/auth/social`
- `GET/POST/PUT/DELETE /calendar/events`
- `GET/POST/PUT/DELETE /tasks`
- `POST /habits/log`, `GET /habits/summary`
- `POST /emotions`, `GET /emotions/insights`
- `GET /progress`, `GET /characters`, `GET/POST /goals`
- `GET/PUT /users/me`, `DELETE /users/me` (GDPR)

---

## Infraestructura

| Servicio | Proveedor |
|----------|-----------|
| Backend API | Railway (Node.js container) |
| PostgreSQL | Railway managed |
| Redis | Railway managed |
| Repo | GitHub (larosiitorres-oss/MiniMo) |
| CI/CD | Railway auto-deploy desde GitHub |
| File storage | Cloudflare R2 (futuro) |

---

## Estado actual

- [x] Repo GitHub creado
- [x] Railway proyecto vinculado
- [x] RuFlo integrado
- [x] CLAUDE.md con contexto + rol pedagógico
- [ ] Inicializar Flutter project
- [ ] Inicializar backend Hono
- [ ] Crear schema PostgreSQL
- [ ] Deploy inicial a Railway
- [ ] Comenzar Fase 1
