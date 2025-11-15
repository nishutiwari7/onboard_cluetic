# Cluetic Onboarding Platform

## Overview

Cluetic is a modern onboarding platform that combines secure authentication with an intelligent chat-based data collection system. The application guides new users through account creation, email verification via OTP, and an interactive onboarding chat that gathers company information. Built with a focus on trust, clarity, and minimal friction, the platform uses a Material Design-inspired interface with clean typography and professional aesthetics.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework & Build System**
- **React 18** with TypeScript for type-safe component development
- **Vite** as the build tool and development server, configured for hot module replacement (HMR)
- **Wouter** for lightweight client-side routing instead of React Router
- **TanStack Query (React Query)** for server state management, data fetching, and caching

**UI Component System**
- **shadcn/ui** component library built on Radix UI primitives
- **Tailwind CSS** for utility-first styling with custom design tokens
- **New York style variant** of shadcn/ui with extensive customization
- Component structure follows atomic design principles with reusable UI primitives

**Design System**
- Custom color system using HSL CSS variables for theming support
- Typography: Inter for UI/body text, Space Grotesk for headings
- Spacing based on Tailwind's scale (2, 4, 6, 8, 12, 16 units)
- Split-screen authentication layout (50/50 desktop, stacked mobile)
- Full-height chat interface with fixed header and input areas

**State Management Strategy**
- React Query for async server state
- Local component state with React hooks for UI interactions
- Session storage for temporary data (email verification state)
- Local storage for authentication tokens and user data

### Backend Architecture

**Server Framework**
- **Express.js** running on Node.js with ES modules
- TypeScript throughout the backend for type safety
- Custom middleware for request logging and JSON body parsing with raw buffer support

**API Design**
- RESTful endpoints under `/api` namespace
- JWT-based authentication using Bearer tokens
- Rate limiting implemented with in-memory maps (intended for Redis in production)
- Structured error responses with appropriate HTTP status codes

**Authentication Flow**
1. User signup creates pending account and triggers OTP email
2. OTP verification (6-digit code) confirms email and activates account
3. JWT token issued upon successful verification or login
4. Protected routes use `authenticateJWT` middleware
5. Password reset flow uses secure token-based system

**Security Measures**
- Bcrypt password hashing with 10 salt rounds
- Speakeasy for OTP generation and validation
- JWT with configurable secret (environment-based)
- Rate limiting on signup (3 attempts per hour per email)
- Session-based authentication consideration via connect-pg-simple

### Data Storage

**Database**
- **PostgreSQL** (via Neon serverless) as the primary database
- **Drizzle ORM** for type-safe database queries and schema management
- WebSocket-based connection pooling for serverless environments

**Schema Design**
```
users: Authentication and profile data
  - id (UUID), name, email (unique), phone, password (hashed)
  - emailVerified flag, createdAt timestamp

otpSessions: Temporary OTP verification codes
  - id, email, otp, secret, expiresAt, createdAt
  - Cleanup mechanism for expired sessions

passwordResetTokens: Password reset workflow
  - id, email, token (unique), expiresAt, createdAt

chatResponses: Onboarding chat data in JSONB format
  - id, userId (foreign key), responses (JSON), completedAt
```

**Data Access Pattern**
- Storage abstraction layer (`IStorage` interface) for database operations
- Repository pattern with methods for CRUD operations on each entity
- Soft validation using Drizzle-Zod integration for runtime type checking

### External Dependencies

**Email Service**
- **Resend** API for transactional emails (OTP, password reset)
- Connector-based authentication using Replit's connector system
- Dynamic credential fetching via Replit's connector API
- Fallback email address configuration

**Development Tools**
- Replit-specific plugins for development (cartographer, dev banner, runtime error overlay)
- Environment-based configuration for development vs. production

**UI Libraries**
- Extensive Radix UI primitive collection for accessible components
- Lucide React for iconography
- date-fns for date manipulation
- QRCode generation library (for potential 2FA future feature)

**Authentication & Security**
- jsonwebtoken for JWT creation and validation
- bcrypt for password hashing
- speakeasy for TOTP/OTP generation

### Application Flow

1. **User Registration**: Split-screen form → OTP email sent → temporary session created
2. **Email Verification**: 6-digit OTP input with 60-second timer → account activation → JWT issued
3. **Login**: Credentials validated → JWT issued → redirect to chat
4. **Password Recovery**: Email entry → token email sent → password reset form with token validation
5. **Onboarding Chat**: Sequential question flow → JSONB storage → completion state

### Build & Deployment

**Development**
- `npm run dev`: TypeScript execution via tsx with hot reload
- Vite dev server proxying through Express
- Environment variables for database and secrets

**Production Build**
- `npm run build`: Vite builds client to `dist/public`, esbuild bundles server to `dist`
- `npm run start`: Node runs compiled server bundle
- Database migrations via `npm run db:push` using Drizzle Kit

**Configuration Management**
- Environment variables for sensitive data (DATABASE_URL, SESSION_SECRET)
- Separate configs for Vite, Tailwind, TypeScript, PostCSS, and Drizzle
- Path aliases for clean imports (@, @shared, @assets)