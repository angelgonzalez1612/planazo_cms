# Planazo — CMS

Panel interno para generar y publicar contenido de Planazo con IA (Next.js
16, App Router). Es hermano de
[`planazo_fronted`](https://github.com/angelgonzalez1612/planazo_fronted)
(sitio público) y [`planazo_backend`](https://github.com/angelgonzalez1612/planazo_backend)
(API compartida) — los tres consumen el mismo contrato en `packages/types`,
y este CMS **escribe** contenido a través de `planazo_backend`; nunca le
habla directo a `planazo_fronted` ni comparte base de datos con él.

## Estructura

```
apps/
  cms/      Next.js 16 (App Router) — login + panel de generación de contenido
packages/
  types/    Contrato de datos compartido (Place, AuthUser, ...)
  config/   Config de sitio y de API (misma que usa el frontend)
  shared/   Utilidades puras (slugify, formatPriceLevel, ...)
```

## Estado actual

Solo tiene el **login**: `/login` autentica contra `planazo_backend`
(`POST /api/auth/login`), que devuelve una cookie de sesión httpOnly. La
página `/` es el dashboard protegido — sin sesión válida redirige a
`/login`. Todavía no hay generación de contenido; eso se construye a
partir de este esqueleto.

No hay registro público — las cuentas (admin/editor) se crean con el script
de seed de `planazo_backend` (`pnpm db:seed`).

## Requisitos

- Node.js 20+
- pnpm 9 (`corepack enable` si no lo tienes)
- `planazo_backend` corriendo en `http://localhost:3001` con al menos un
  usuario sembrado (`pnpm --filter @planazo/api db:seed`)

## Puesta en marcha

```bash
pnpm install
cp apps/cms/.env.example apps/cms/.env.local
pnpm dev
```

- CMS: http://localhost:3002

## Variables de entorno (`apps/cms/.env.local`)

| Variable | Descripción |
|---|---|
| `NEXT_PUBLIC_API_URL` | Base URL de `planazo_backend` (`http://localhost:3001/api` en dev) |
