# Backend Folder Structure - NestJS

## Complete Directory Structure

```
backend/
├── prisma/
│   ├── schema.prisma              # Prisma schema definition
│   ├── migrations/                # Database migrations
│   └── seed.ts                    # Database seeding script
├── src/
│   ├── main.ts                    # Application entry point
│   ├── app.module.ts              # Root module
│   ├── app.controller.ts          # Health check endpoint
│   ├── app.service.ts             # App-level services
│   │
│   ├── common/                    # Shared utilities
│   │   ├── decorators/            # Custom decorators
│   │   │   ├── roles.decorator.ts
│   │   │   ├── tenant.decorator.ts
│   │   │   └── public.decorator.ts
│   │   ├── guards/                # Auth & Role guards
│   │   │   ├── jwt-auth.guard.ts
│   │   │   ├── roles.guard.ts
│   │   │   └── tenant.guard.ts
│   │   ├── interceptors/          # Request/Response interceptors
│   │   │   ├── logging.interceptor.ts
│   │   │   ├── transform.interceptor.ts
│   │   │   └── timeout.interceptor.ts
│   │   ├── filters/               # Exception filters
│   │   │   └── http-exception.filter.ts
│   │   ├── pipes/                 # Validation pipes
│   │   │   └── validation.pipe.ts
│   │   ├── middleware/            # Custom middleware
│   │   │   ├── tenant.middleware.ts
│   │   │   ├── logger.middleware.ts
│   │   │   └── rate-limit.middleware.ts
│   │   ├── dto/                   # Common DTOs
│   │   │   ├── pagination.dto.ts
│   │   │   └── response.dto.ts
│   │   ├── interfaces/            # Common interfaces
│   │   │   ├── request.interface.ts
│   │   │   └── jwt-payload.interface.ts
│   │   ├── constants/             # Constants
│   │   │   ├── roles.constant.ts
│   │   │   ├── order-status.constant.ts
│   │   │   └── errors.constant.ts
│   │   └── utils/                 # Utility functions
│   │       ├── hash.util.ts
│   │       ├── date.util.ts
│   │       └── slug.util.ts
│   │
│   ├── config/                    # Configuration
│   │   ├── app.config.ts
│   │   ├── database.config.ts
│   │   ├── auth.config.ts
│   │   ├── redis.config.ts
│   │   ├── s3.config.ts
│   │   ├── stripe.config.ts
│   │   └── paypal.config.ts
│   │
│   ├── database/                  # Database module
│   │   ├── database.module.ts
│   │   └── prisma.service.ts      # Prisma client service
│   │
│   ├── modules/
│   │   │
│   │   ├── auth/                  # Authentication module
│   │   │   ├── auth.module.ts
│   │   │   ├── auth.controller.ts
│   │   │   ├── auth.service.ts
│   │   │   ├── strategies/
│   │   │   │   ├── jwt.strategy.ts
│   │   │   │   └── refresh-token.strategy.ts
│   │   │   └── dto/
│   │   │       ├── register.dto.ts
│   │   │       ├── login.dto.ts
│   │   │       ├── refresh-token.dto.ts
│   │   │       ├── forgot-password.dto.ts
│   │   │       └── reset-password.dto.ts
│   │   │
│   │   ├── tenants/               # Tenant management (Super Admin)
│   │   │   ├── tenants.module.ts
│   │   │   ├── tenants.controller.ts
│   │   │   ├── tenants.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-tenant.dto.ts
│   │   │   │   ├── update-tenant.dto.ts
│   │   │   │   └── tenant-query.dto.ts
│   │   │   └── entities/
│   │   │       └── tenant.entity.ts
│   │   │
│   │   ├── users/                 # User management
│   │   │   ├── users.module.ts
│   │   │   ├── users.controller.ts
│   │   │   ├── users.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-user.dto.ts
│   │   │   │   ├── update-user.dto.ts
│   │   │   │   └── user-query.dto.ts
│   │   │   └── entities/
│   │   │       └── user.entity.ts
│   │   │
│   │   ├── restaurants/           # Restaurant management
│   │   │   ├── restaurants.module.ts
│   │   │   ├── restaurants.controller.ts
│   │   │   ├── restaurants.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-restaurant.dto.ts
│   │   │   │   ├── update-restaurant.dto.ts
│   │   │   │   └── restaurant-settings.dto.ts
│   │   │   └── entities/
│   │   │       ├── restaurant.entity.ts
│   │   │       └── restaurant-settings.entity.ts
│   │   │
│   │   ├── branches/              # Branch management
│   │   │   ├── branches.module.ts
│   │   │   ├── branches.controller.ts
│   │   │   ├── branches.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-branch.dto.ts
│   │   │   │   ├── update-branch.dto.ts
│   │   │   │   ├── branch-hours.dto.ts
│   │   │   │   └── delivery-zone.dto.ts
│   │   │   └── entities/
│   │   │       ├── branch.entity.ts
│   │   │       ├── branch-hours.entity.ts
│   │   │       └── delivery-zone.entity.ts
│   │   │
│   │   ├── menu/                  # Menu management
│   │   │   ├── menu.module.ts
│   │   │   ├── categories/
│   │   │   │   ├── categories.controller.ts
│   │   │   │   ├── categories.service.ts
│   │   │   │   └── dto/
│   │   │   │       ├── create-category.dto.ts
│   │   │   │       └── update-category.dto.ts
│   │   │   ├── items/
│   │   │   │   ├── items.controller.ts
│   │   │   │   ├── items.service.ts
│   │   │   │   └── dto/
│   │   │   │       ├── create-item.dto.ts
│   │   │   │       ├── update-item.dto.ts
│   │   │   │       ├── item-modifier.dto.ts
│   │   │   │       ├── item-addon.dto.ts
│   │   │   │       └── item-availability.dto.ts
│   │   │   └── entities/
│   │   │       ├── menu-category.entity.ts
│   │   │       ├── menu-item.entity.ts
│   │   │       ├── item-modifier.entity.ts
│   │   │       ├── item-addon.entity.ts
│   │   │       └── item-availability.entity.ts
│   │   │
│   │   ├── orders/                # Order management
│   │   │   ├── orders.module.ts
│   │   │   ├── orders.controller.ts
│   │   │   ├── orders.service.ts
│   │   │   ├── orders.gateway.ts  # WebSocket for real-time updates
│   │   │   ├── dto/
│   │   │   │   ├── create-order.dto.ts
│   │   │   │   ├── update-order-status.dto.ts
│   │   │   │   ├── cancel-order.dto.ts
│   │   │   │   └── order-query.dto.ts
│   │   │   └── entities/
│   │   │       ├── order.entity.ts
│   │   │       ├── order-item.entity.ts
│   │   │       └── order-status-history.entity.ts
│   │   │
│   │   ├── customers/             # Customer management
│   │   │   ├── customers.module.ts
│   │   │   ├── customers.controller.ts
│   │   │   ├── customers.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-customer.dto.ts
│   │   │   │   ├── update-customer.dto.ts
│   │   │   │   └── customer-address.dto.ts
│   │   │   └── entities/
│   │   │       ├── customer.entity.ts
│   │   │       └── customer-address.entity.ts
│   │   │
│   │   ├── payments/              # Payment processing
│   │   │   ├── payments.module.ts
│   │   │   ├── payments.controller.ts
│   │   │   ├── payments.service.ts
│   │   │   ├── stripe/
│   │   │   │   ├── stripe.service.ts
│   │   │   │   └── stripe-webhook.controller.ts
│   │   │   ├── paypal/
│   │   │   │   ├── paypal.service.ts
│   │   │   │   └── paypal-webhook.controller.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-payment-intent.dto.ts
│   │   │   │   ├── confirm-payment.dto.ts
│   │   │   │   └── refund-payment.dto.ts
│   │   │   └── entities/
│   │   │       └── payment.entity.ts
│   │   │
│   │   ├── coupons/               # Coupon management
│   │   │   ├── coupons.module.ts
│   │   │   ├── coupons.controller.ts
│   │   │   ├── coupons.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-coupon.dto.ts
│   │   │   │   ├── update-coupon.dto.ts
│   │   │   │   └── validate-coupon.dto.ts
│   │   │   └── entities/
│   │   │       ├── coupon.entity.ts
│   │   │       └── coupon-usage.entity.ts
│   │   │
│   │   ├── delivery/              # Delivery tracking
│   │   │   ├── delivery.module.ts
│   │   │   ├── delivery.controller.ts
│   │   │   ├── delivery.service.ts
│   │   │   ├── dto/
│   │   │   │   └── update-delivery.dto.ts
│   │   │   └── entities/
│   │   │       └── delivery-tracking.entity.ts
│   │   │
│   │   ├── staff/                 # Staff management
│   │   │   ├── staff.module.ts
│   │   │   ├── staff.controller.ts
│   │   │   ├── staff.service.ts
│   │   │   ├── dto/
│   │   │   │   ├── create-staff.dto.ts
│   │   │   │   └── update-staff.dto.ts
│   │   │   └── entities/
│   │   │       └── staff.entity.ts
│   │   │
│   │   ├── reports/               # Reports & Analytics
│   │   │   ├── reports.module.ts
│   │   │   ├── reports.controller.ts
│   │   │   ├── reports.service.ts
│   │   │   └── dto/
│   │   │       ├── sales-report.dto.ts
│   │   │       ├── revenue-report.dto.ts
│   │   │       └── popular-items.dto.ts
│   │   │
│   │   ├── notifications/         # Notification service
│   │   │   ├── notifications.module.ts
│   │   │   ├── notifications.service.ts
│   │   │   ├── email/
│   │   │   │   ├── email.service.ts
│   │   │   │   └── templates/
│   │   │   │       ├── order-confirmation.hbs
│   │   │   │       └── password-reset.hbs
│   │   │   ├── sms/
│   │   │   │   └── sms.service.ts
│   │   │   └── push/
│   │   │       └── push.service.ts
│   │   │
│   │   ├── storage/               # File upload/storage
│   │   │   ├── storage.module.ts
│   │   │   ├── storage.service.ts
│   │   │   └── dto/
│   │   │       └── upload-file.dto.ts
│   │   │
│   │   ├── subscriptions/         # Subscription management
│   │   │   ├── subscriptions.module.ts
│   │   │   ├── subscriptions.controller.ts
│   │   │   ├── subscriptions.service.ts
│   │   │   └── dto/
│   │   │       ├── create-subscription.dto.ts
│   │   │       └── update-subscription.dto.ts
│   │   │
│   │   ├── audit/                 # Audit logging
│   │   │   ├── audit.module.ts
│   │   │   ├── audit.service.ts
│   │   │   └── entities/
│   │   │       └── audit-log.entity.ts
│   │   │
│   │   ├── webhooks/              # Webhook management
│   │   │   ├── webhooks.module.ts
│   │   │   ├── webhooks.service.ts
│   │   │   └── dto/
│   │   │       └── webhook-payload.dto.ts
│   │   │
│   │   └── queue/                 # Background jobs (BullMQ)
│   │       ├── queue.module.ts
│   │       ├── processors/
│   │       │   ├── email.processor.ts
│   │       │   ├── order.processor.ts
│   │       │   └── report.processor.ts
│   │       └── producers/
│   │           ├── email.producer.ts
│   │           └── order.producer.ts
│   │
│   └── test/                      # Test utilities
│       ├── fixtures/
│       └── helpers/
│
├── test/                          # E2E tests
│   ├── auth.e2e-spec.ts
│   ├── orders.e2e-spec.ts
│   └── jest-e2e.json
│
├── .env.example                   # Environment variables template
├── .env                           # Environment variables (gitignored)
├── .eslintrc.js                   # ESLint configuration
├── .prettierrc                    # Prettier configuration
├── nest-cli.json                  # NestJS CLI configuration
├── package.json                   # Dependencies
├── tsconfig.json                  # TypeScript configuration
├── tsconfig.build.json            # Build TypeScript configuration
└── README.md                      # Backend README
```

---

## Key Design Patterns

### 1. Module Structure
Each feature module follows this pattern:
```
module-name/
├── module-name.module.ts      # Module definition
├── module-name.controller.ts  # HTTP endpoints
├── module-name.service.ts     # Business logic
├── dto/                       # Data Transfer Objects
├── entities/                  # Domain entities
└── interfaces/                # TypeScript interfaces (optional)
```

### 2. Dependency Injection
- Services are injected via constructor
- Modules export services for other modules to use
- Global modules (Database, Config) are available everywhere

### 3. Request Flow
```
Request → Middleware → Guards → Interceptors (before) →
Controller → Service → Database → Service →
Interceptors (after) → Response
```

### 4. Error Handling
- Global exception filter catches all errors
- Business logic throws specific exceptions
- Consistent error response format

---

## Module Dependencies

```
AppModule
├── ConfigModule (Global)
├── DatabaseModule (Global)
├── AuthModule
│   └── UsersModule
├── TenantsModule
├── RestaurantsModule
│   ├── BranchesModule
│   ├── MenuModule
│   └── StaffModule
├── OrdersModule
│   ├── CustomersModule
│   ├── PaymentsModule
│   ├── CouponsModule
│   └── DeliveryModule
├── ReportsModule
├── NotificationsModule
├── StorageModule
├── QueueModule
├── AuditModule
└── WebhooksModule
```

---

## Environment Variables

```bash
# App
NODE_ENV=development
PORT=3000
APP_URL=http://localhost:3000

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/restaurant_saas

# JWT
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=15m
REFRESH_TOKEN_SECRET=your-refresh-token-secret
REFRESH_TOKEN_EXPIRES_IN=7d

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# AWS S3 / MinIO
S3_ENDPOINT=http://localhost:9000
S3_ACCESS_KEY=minioadmin
S3_SECRET_KEY=minioadmin
S3_BUCKET=restaurant-uploads
S3_REGION=us-east-1

# Stripe
STRIPE_SECRET_KEY=sk_test_xxx
STRIPE_PUBLISHABLE_KEY=pk_test_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx

# PayPal
PAYPAL_CLIENT_ID=xxx
PAYPAL_CLIENT_SECRET=xxx
PAYPAL_MODE=sandbox

# Email (SendGrid)
SENDGRID_API_KEY=SG.xxx
SENDGRID_FROM_EMAIL=noreply@ordersaas.com
SENDGRID_FROM_NAME=OrderSaaS

# SMS (Twilio)
TWILIO_ACCOUNT_SID=ACxxx
TWILIO_AUTH_TOKEN=xxx
TWILIO_PHONE_NUMBER=+1234567890

# Rate Limiting
RATE_LIMIT_TTL=60
RATE_LIMIT_MAX=100

# CORS
CORS_ORIGINS=http://localhost:3001,http://localhost:3002

# Sentry (Error Tracking)
SENTRY_DSN=https://xxx@sentry.io/xxx
```

---

## Package Dependencies

```json
{
  "dependencies": {
    "@nestjs/common": "^10.0.0",
    "@nestjs/core": "^10.0.0",
    "@nestjs/platform-express": "^10.0.0",
    "@nestjs/config": "^3.0.0",
    "@nestjs/jwt": "^10.0.0",
    "@nestjs/passport": "^10.0.0",
    "@nestjs/websockets": "^10.0.0",
    "@nestjs/platform-socket.io": "^10.0.0",
    "@nestjs/throttler": "^4.0.0",
    "@nestjs/bull": "^10.0.0",
    "@prisma/client": "^5.0.0",
    "prisma": "^5.0.0",
    "passport": "^0.6.0",
    "passport-jwt": "^4.0.1",
    "bcrypt": "^5.1.0",
    "class-validator": "^0.14.0",
    "class-transformer": "^0.5.1",
    "bull": "^4.11.0",
    "redis": "^4.6.0",
    "ioredis": "^5.3.0",
    "stripe": "^13.0.0",
    "@paypal/checkout-server-sdk": "^1.0.3",
    "aws-sdk": "^2.1400.0",
    "@sendgrid/mail": "^7.7.0",
    "twilio": "^4.14.0",
    "helmet": "^7.0.0",
    "compression": "^1.7.4",
    "winston": "^3.10.0",
    "nanoid": "^3.3.6",
    "qrcode": "^1.5.3",
    "handlebars": "^4.7.8"
  },
  "devDependencies": {
    "@nestjs/cli": "^10.0.0",
    "@nestjs/schematics": "^10.0.0",
    "@nestjs/testing": "^10.0.0",
    "@types/node": "^20.0.0",
    "@types/express": "^4.17.17",
    "@types/bcrypt": "^5.0.0",
    "@types/passport-jwt": "^3.0.9",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "eslint": "^8.45.0",
    "prettier": "^3.0.0",
    "jest": "^29.6.0",
    "ts-jest": "^29.1.1",
    "supertest": "^6.3.3",
    "typescript": "^5.1.6"
  }
}
```

---

## Scripts

```json
{
  "scripts": {
    "prebuild": "rimraf dist",
    "build": "nest build",
    "start": "nest start",
    "start:dev": "nest start --watch",
    "start:debug": "nest start --debug --watch",
    "start:prod": "node dist/main",

    "prisma:generate": "prisma generate",
    "prisma:migrate": "prisma migrate dev",
    "prisma:deploy": "prisma migrate deploy",
    "prisma:studio": "prisma studio",
    "prisma:seed": "ts-node prisma/seed.ts",

    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "test:e2e": "jest --config ./test/jest-e2e.json",

    "lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
    "format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\""
  }
}
```

---

## Testing Strategy

### Unit Tests
- Test services in isolation
- Mock dependencies
- Coverage target: 80%+

### Integration Tests
- Test module interactions
- Use test database
- Test actual database queries

### E2E Tests
- Test complete API flows
- Authentication
- Order creation
- Payment processing

---

## Performance Optimizations

1. **Database**
   - Proper indexing
   - Query optimization with Prisma
   - Connection pooling

2. **Caching**
   - Redis for menu data
   - Session storage
   - Rate limiting

3. **Queue Processing**
   - BullMQ for async tasks
   - Email notifications
   - Report generation

4. **File Upload**
   - Direct S3 upload
   - Signed URLs
   - CDN delivery

---

## Security Best Practices

1. **Input Validation**
   - class-validator on all DTOs
   - Sanitize user input
   - Prevent SQL injection (Prisma handles this)

2. **Authentication**
   - JWT with short expiry
   - Refresh token rotation
   - Password hashing with bcrypt

3. **Authorization**
   - Role-based access control
   - Tenant isolation
   - Resource ownership checks

4. **Rate Limiting**
   - Per IP and per user
   - Different limits for different endpoints
   - Redis-backed

5. **Headers**
   - Helmet.js for security headers
   - CORS configuration
   - HTTPS only in production

---

This structure provides a solid foundation for a production-ready, scalable restaurant SaaS platform.
