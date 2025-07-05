# VisionPro Analytics Dashboard

## Overview

This is a comprehensive full-stack business intelligence dashboard built with PostgreSQL, Express.js, React, Node.js featuring JWT authentication and role-based access control. The application provides advanced analytics, statistical analysis, data visualization, and comprehensive reporting capabilities for business sales data. It includes separate dashboards for users and administrators with CRUD functionality for data management.

## System Architecture

The application follows a monorepo structure with clear separation between client, server, and shared components:

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Styling**: Tailwind CSS with shadcn/ui component library
- **State Management**: TanStack Query for server state, React Context for auth
- **Routing**: Wouter for client-side routing
- **Build Tool**: Vite for development and bundling
- **Form Handling**: React Hook Form with Zod validation

### Backend Architecture
- **Runtime**: Node.js with Express.js
- **Language**: TypeScript with ES modules
- **Authentication**: JWT-based authentication with bcrypt for password hashing
- **API Design**: RESTful endpoints with proper error handling
- **Development**: tsx for TypeScript execution in development

### Database Architecture
- **Database**: PostgreSQL (configured for Neon Database)
- **ORM**: Drizzle ORM for type-safe database operations
- **Migrations**: Drizzle Kit for schema management
- **Connection**: @neondatabase/serverless for connection pooling

## Key Components

### Authentication System
- JWT-based authentication with configurable expiration
- Password hashing using bcryptjs
- Role-based access control (admin/user roles)
- Protected routes with authentication guards
- Persistent login state using localStorage

### Data Management
- **Users Table**: Stores user profiles with role-based permissions
- **Superstore Data Table**: Contains sales transaction data with comprehensive fields
- **In-Memory Storage**: Development-friendly storage implementation with sample data
- **Data Filtering**: Advanced filtering by region, category, and date ranges

### Analytics Dashboard
- **KPI Cards**: Real-time metrics for sales, profit, orders, and customers
- **Interactive Charts**: Line charts for sales trends, pie charts for category distribution
- **Data Tables**: Sortable and filterable transaction tables with pagination
- **Export Functionality**: CSV export capabilities for data analysis

### UI Components
- **Design System**: shadcn/ui components with consistent styling
- **Responsive Design**: Mobile-first approach with adaptive layouts
- **Accessibility**: ARIA labels and keyboard navigation support
- **Loading States**: Skeleton loaders and loading indicators

## Data Flow

1. **Authentication Flow**:
   - User submits login/register form
   - Server validates credentials and generates JWT
   - Client stores token and user data in localStorage
   - Subsequent requests include Authorization header

2. **Dashboard Data Flow**:
   - Client requests dashboard data with filters
   - Server processes filters and aggregates data
   - Response includes KPIs, chart data, and transactions
   - Client renders interactive visualizations

3. **Filter Application**:
   - User selects filters in UI
   - Filters are applied to all dashboard queries
   - Real-time updates across all components
   - URL parameters maintain filter state

## External Dependencies

### Frontend Dependencies
- **@tanstack/react-query**: Server state management and caching
- **@radix-ui/***: Accessible UI primitives for component library
- **recharts**: Chart and visualization library
- **react-hook-form**: Form state management and validation
- **zod**: Schema validation and type safety
- **date-fns**: Date manipulation and formatting

### Backend Dependencies
- **jsonwebtoken**: JWT token generation and verification
- **bcryptjs**: Password hashing and validation
- **drizzle-orm**: Type-safe database operations
- **@neondatabase/serverless**: PostgreSQL connection adapter

### Development Dependencies
- **tsx**: TypeScript execution for development
- **esbuild**: Fast bundling for production builds
- **drizzle-kit**: Database schema management and migrations

## Deployment Strategy

### Development Environment
- **Frontend**: Vite dev server with HMR and React Fast Refresh
- **Backend**: tsx with automatic restart on file changes
- **Database**: Local PostgreSQL or Neon Database connection
- **Environment**: Environment variables for database connection

### Production Build
- **Frontend**: Vite production build with optimized assets
- **Backend**: esbuild bundle with ESM format and external packages
- **Deployment**: Single-process deployment with static file serving
- **Database**: Neon Database with connection pooling

### Environment Configuration
- **DATABASE_URL**: PostgreSQL connection string (required)
- **JWT_SECRET**: Secret key for JWT signing (defaults to fallback)
- **NODE_ENV**: Environment mode (development/production)

## Recent Changes

- July 02, 2025: MongoDB migration preparation and PostgreSQL optimization
  ✓ Created complete MongoDB migration package with Mongoose integration
  ✓ Built MongoDB storage layer with full feature parity
  ✓ Maintained PostgreSQL as primary database for stability
  ✓ Two-column login design with VisionPro Analytics branding
  ✓ Comprehensive documentation for database migration options
  ✓ All authentication and features working on PostgreSQL
  ✓ Migration guide created for MongoDB when database is available
  ✓ Working credentials: admin@superstore.com / password123

## Changelog

- July 02, 2025: Initial project setup and complete feature implementation

## User Preferences

Preferred communication style: Simple, everyday language.