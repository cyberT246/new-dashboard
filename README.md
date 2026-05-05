# Device Management Web App

Production-ready full-stack application for managing devices with authentication, filtering, sorting, and pagination.

## Folder Structure

```text
.
├── backend
│   ├── prisma
│   │   └── schema.prisma
│   ├── src
│   │   ├── config
│   │   │   ├── env.ts
│   │   │   └── prisma.ts
│   │   ├── controllers
│   │   │   ├── AuthController.ts
│   │   │   └── DeviceController.ts
│   │   ├── middlewares
│   │   │   ├── auth.middleware.ts
│   │   │   ├── error.middleware.ts
│   │   │   └── validate.ts
│   │   ├── repositories
│   │   │   └── DeviceRepository.ts
│   │   ├── routes
│   │   │   ├── auth.routes.ts
│   │   │   └── device.routes.ts
│   │   ├── services
│   │   │   ├── AuthService.ts
│   │   │   └── DeviceService.ts
│   │   ├── types
│   │   │   └── express
│   │   │       └── index.d.ts
│   │   ├── utils
│   │   │   ├── AppError.ts
│   │   │   └── jwt.ts
│   │   ├── validations
│   │   │   ├── auth.validation.ts
│   │   │   └── device.validation.ts
│   │   ├── app.ts
│   │   └── server.ts
│   ├── .env.example
│   ├── Dockerfile
│   ├── package.json
│   └── tsconfig.json
├── frontend
│   ├── public
│   │   └── .gitkeep
│   ├── src
│   │   ├── app
│   │   │   ├── dashboard
│   │   │   │   └── page.tsx
│   │   │   ├── login
│   │   │   │   └── page.tsx
│   │   │   ├── globals.css
│   │   │   ├── layout.tsx
│   │   │   └── page.tsx
│   │   ├── lib
│   │   │   └── apiClient.ts
│   │   ├── middleware.ts
│   │   └── types
│   │       └── index.ts
│   ├── .env.example
│   ├── Dockerfile
│   ├── next-env.d.ts
│   ├── next.config.mjs
│   ├── package.json
│   ├── postcss.config.js
│   ├── tailwind.config.ts
│   └── tsconfig.json
├── docker-compose.yml
└── README.md
```

## Architecture Diagram (ASCII)

```text
Frontend (Next.js) ─── Axios apiClient ───► Backend API (Express + TS)
     │                                        │
     │ Cookie token                           │ JWT auth middleware
     ▼                                        ▼
  /login, /dashboard                   Controllers (OOP)
                                               │
                                               ▼
                                         Services (business logic)
                                               │
                                               ▼
                                       Repositories (Prisma access)
                                               │
                                               ▼
                                         PostgreSQL (Docker)
```

## Tech Stack

- Backend: Node.js, Express, TypeScript, Prisma (SQLite), Zod, JWT
- Frontend: Next.js 14 (App Router), TypeScript, Tailwind CSS, Axios
- DevOps: Dockerfiles included (optional)

## Setup

1. Copy environment templates:
   - `backend/.env.example` -> `backend/.env`
   - `frontend/.env.example` -> `frontend/.env`
2. Initialize the database:
   - `cd backend`
   - `npx prisma generate`
   - `npx prisma db push`
3. Start backend + frontend:
   - Backend: `cd backend && npm run dev`
   - Frontend: `cd frontend && npm run dev`
4. Open:
   - Frontend: `http://localhost:3000`
   - Backend health: `http://localhost:4000/health`

## Sample Credentials

- Email: `admin@example.com`
- Password: `Admin@123`

## API Reference

| Method | Endpoint | Auth | Description |
| --- | --- | --- | --- |
| POST | `/api/auth/login` | No | Login and receive JWT token |
| GET | `/api/devices` | Yes | List devices with filters, sorting, pagination |
| POST | `/api/devices` | Yes | Create a device |
| DELETE | `/api/devices/:id` | Yes | Delete a device by id |

### GET /api/devices Query Parameters

- `platform`: `android` | `ios` | `web`
- `name`: partial name search
- `sortBy`: `name` | `createdAt`
- `order`: `asc` | `desc`
- `page`: number (default `1`)
- `limit`: number (default `10`, max `100`)

## Error Response Format

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": ["Name is required"]
}
```
