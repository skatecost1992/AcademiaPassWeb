# AcademiaPassWeb

Gestión de academias: sedes, espacios con aforo, clases y profesores, reservas, planes, pagos, ingreso con
código QR y reportes. Este proyecto tiene el **servicio** (la API que consulta la base de datos PostgreSQL en la
nube) y la **versión web**. Las apps móviles están en sus propios proyectos y usan este mismo servicio:

| Proyecto | Contenido |
|---|---|
| **AcademiaPassWeb** (este) | API Node.js y versión web |
| **AcademiaPassAndroid** | App Expo para Android (APK) |
| **AcademiaPassIos** | App Expo para iOS (IPA) |

```
AcademiaPassWeb/
├── backend/        API Node.js (Express 5 + PostgreSQL). Sirve la versión web compilada en backend/public.
├── web/            Fuentes de la versión web (Expo / React Native Web).
├── docs/           Manual técnico, despliegue y guía de uso
├── deploy/         Despliegue en una máquina virtual (Oracle Cloud)
└── render.yaml     Despliegue en Render (API + web, base en Neon)
```

Las pantallas existen en tres copias (`web/src`, y `src/` en cada app): un cambio de pantalla se copia a los
proyectos que lo necesiten.

**Publicar cambios de la versión web:** `cd web && npm run build:web` (deja la web en `backend/public`), commit de
`backend/public` y push. Render sirve la web compilada; no la compila.

## Empezar en local (sin instalar PostgreSQL)

```bash
cd backend
npm install
npm run seed      # organización demo
npm start         # http://localhost:8080
```

Sin `DATABASE_URL` la API usa PGlite (PostgreSQL embebido) en `backend/.data/`. Usuarios demo (contraseña
`Demo.2026`): `admin.demo`, `recepcion.demo`, `cliente.demo`, `cliente2.demo`.

Versión web en desarrollo: `cd web && npm install --legacy-peer-deps && npm start`.

## Nube y teléfonos

1. Crea el Blueprint de `render.yaml` en Render con la base en Neon (`DATABASE_URL`).
2. En las apps, en *Configurar servidor* (pantalla de ingreso), la URL pública, p. ej.
   `https://academiapass.onrender.com`. Todos los teléfonos y la web ven los mismos datos.

Paso a paso en [`docs/DESPLIEGUE.md`](docs/DESPLIEGUE.md). Selector nube/local (usuario de configuración `admin`)
y base en SQL Server en [`docs/SERVIDORES_Y_BASE_LOCAL.md`](docs/SERVIDORES_Y_BASE_LOCAL.md). Arquitectura, modelo de datos y reglas en
[`docs/MANUAL_TECNICO.md`](docs/MANUAL_TECNICO.md). Uso por rol en [`docs/GUIA_DE_USO.md`](docs/GUIA_DE_USO.md).

## Pruebas

```bash
cd backend && npm test        # escenarios de punta a punta sobre la API (también en GitHub Actions)
cd web && npx tsc --noEmit    # tipos de la web (también en GitHub Actions)
```
