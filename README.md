# Business Management Platform (Multi-Shop ERP)

Full web-based business management platform built end-to-end with:
- Frontend: React + TypeScript + Vite + Ant Design
- Backend: Node.js + Express + TypeScript
- Database: PostgreSQL + Prisma ORM + migrations
- Architecture: Modular monolith (single backend service with domain modules)

## Modules Delivered

- Authentication and Authorization (JWT, RBAC, permissions, user rights)
- Multi Shop Management
- Customer Management
- Sales Management (invoice, inventory impact, finance impact, warranty impact)
- Online Selling (public storefront, cart/order flow, order status, order confirmation to sale)
- Service Booking and Service Job Management
- Parts and Inventory Management
- Supplier and Purchase Management
- Inter-Shop Internal Demand & Transfer Management (request, approve/reject, dispatch, receive)
- Warranty and Claim Management
- Accounts and Finance
- HR and Payroll
- SMS and Communication Logs
- External Store Integration (Shopify/Custom sync architecture)
- Reporting and Analytics
- Audit Logs and Settings

## Operational Flow Coverage

Every critical action is wired with full flow:
1. UI action
2. API request
3. DTO validation (Zod)
4. Service/business logic
5. Database transaction
6. Reports data impact
7. Audit trail where applicable
8. Notification/SMS logging where applicable
9. Print endpoints for invoice/job card/transfer document

## Project Structure

```
business-management-platform/
  apps/
    api/              # Express + Prisma backend
    web/              # React + Vite + AntD frontend
  docker-compose.local.yml
  docker-compose.cloud.yml
  .env.local.example
  .env.cloud.example
  DEMO_CREDENTIALS
```

## Database and Migration

- Prisma schema: `apps/api/prisma/schema.prisma`
- Migration SQL:
  - `apps/api/prisma/migrations/20260308153000_init/migration.sql`
  - `apps/api/prisma/migrations/20260308160000_inter_shop_transfer/migration.sql`
  - `apps/api/prisma/migrations/20260308170000_service_platforms_and_jobcard_flow/migration.sql`
  - `apps/api/prisma/migrations/20260308173000_service_vehicle_details/migration.sql`
- Seed script: `apps/api/prisma/seed.ts`

## Local Development (Without Docker)

### 1) Install dependencies
```bash
npm install
```

### 2) Configure environment
```bash
cp .env.local.example .env.local
cp apps/api/.env.example apps/api/.env
cp apps/web/.env.example apps/web/.env
```

Update values as needed.

### 3) Run migrations and seed
```bash
npm run db:generate --workspace @bmp/api
npm run db:migrate:dev --workspace @bmp/api
npm run db:seed --workspace @bmp/api
```

### 4) Start both apps
```bash
npm run dev
```

- API: `http://localhost:4000/api`
- Web: `http://localhost:5173`
- Storefront: `http://localhost:5173/store`

## Local LAN Server Deployment (Docker)

### 1) Create `.env.local`
```bash
cp .env.local.example .env.local
```

### 2) Start stack
```bash
docker compose -f docker-compose.local.yml up --build -d
```

### 3) Seed data inside API container
```bash
docker compose -f docker-compose.local.yml exec api npm run db:seed
```

### Access from LAN
- Web: `http://<server-lan-ip>:8080`
- API: `http://<server-lan-ip>:4000/api`

## Cloud Deployment (Same Codebase)

### 1) Configure cloud env file
```bash
cp .env.cloud.example .env.cloud
```

Set cloud PostgreSQL URL, JWT secret, and public domains.

### 2) Deploy services
```bash
docker compose -f docker-compose.cloud.yml up --build -d
```

### 3) Run migration and seed
```bash
docker compose -f docker-compose.cloud.yml exec api npm run db:migrate
docker compose -f docker-compose.cloud.yml exec api npm run db:seed
```

## Main API Route Groups

- `/api/auth`
- `/api/shops`
- `/api/users`
- `/api/customers`
- `/api/suppliers`
- `/api/inventory`
- `/api/transfers`
- `/api/purchases`
- `/api/sales`
- `/api/services`
- `/api/warranties`
- `/api/finance`
- `/api/hr`
- `/api/communications`
- `/api/online`
- `/api/integrations`
- `/api/reports`
- `/api/dashboard`
- `/api/settings`

## Reports Implemented

- Sales report
- Customer sales history
- Inventory stock report
- Inventory movement report
- Service booking report
- Service job status report
- Supplier purchase report
- Inter shop transfer report
- Warranty report
- Employee report
- Payroll report
- Financial summary report
- Shop-wise operational report

## Integration Tests

Workflow integration tests are included at:
- `apps/api/tests/integration/workflows.test.ts`

Covers:
- Create customer
- Create sale and verify inventory reduction
- Create purchase and verify inventory increase
- Create service booking, generate job card, assign platform, start service
- Register warranty and file claim
- Generate payroll
- Inter-shop demand request/dispatch/receive with stock verification
- Verify report endpoints return data

Run:
```bash
npm run test:integration --workspace @bmp/api
```

## Demo Access

See `DEMO_CREDENTIALS` in project root.

## Notes

- Same codebase supports local and cloud deployment by environment configuration only.
- SMS and call integrations are designed with log-based adapters and can be connected to real providers.
- External store sync endpoints support configuration and import workflows (including Shopify adapter path).
