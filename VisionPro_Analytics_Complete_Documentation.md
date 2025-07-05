# VisionPro Analytics - Complete System Documentation

## Table of Contents
1. [System Overview](#system-overview)
2. [Technical Architecture](#technical-architecture)
3. [Database Schema](#database-schema)
4. [Authentication System](#authentication-system)
5. [API Endpoints Documentation](#api-endpoints-documentation)
6. [Frontend Components](#frontend-components)
7. [User Access Levels](#user-access-levels)
8. [Features & Functionality](#features--functionality)
9. [Installation & Setup](#installation--setup)
10. [Testing Credentials](#testing-credentials)
11. [Deployment Information](#deployment-information)

---

## System Overview

**VisionPro Analytics** is a comprehensive full-stack business intelligence dashboard built with modern web technologies. The platform transforms raw business data into actionable insights through interactive dashboards, advanced analytics, and comprehensive reporting capabilities.

### Key Features
- **Role-Based Access Control**: Admin and User roles with different permissions
- **Interactive Dashboards**: Real-time KPIs, charts, and data visualizations  
- **Advanced Analytics**: Statistical analysis, forecasting, and trend analysis
- **Data Management**: Full CRUD operations for business data (Admin only)
- **Export Capabilities**: CSV export for reports and data analysis
- **Responsive Design**: Works seamlessly across desktop, tablet, and mobile
- **Secure Authentication**: JWT-based authentication with password hashing

---

## Technical Architecture

### Technology Stack
- **Frontend**: React 18 + TypeScript + Vite
- **Backend**: Node.js + Express.js + TypeScript  
- **Database**: PostgreSQL with Drizzle ORM
- **Authentication**: JWT tokens with bcrypt password hashing
- **Styling**: Tailwind CSS + shadcn/ui components
- **State Management**: TanStack Query + React Context
- **Charts**: Recharts library for data visualization
- **Routing**: Wouter for client-side navigation

### Project Structure
```
project-root/
├── client/                 # Frontend React application
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   ├── pages/         # Application pages/routes
│   │   ├── hooks/         # Custom React hooks
│   │   └── lib/           # Utility functions
├── server/                # Backend Express application
│   ├── middleware/        # Authentication middleware
│   ├── models/           # Legacy Mongoose models
│   ├── data/             # Sample data files
│   ├── routes.ts         # API route definitions
│   ├── storage.ts        # Database operations
│   └── db.ts             # Database connection
├── shared/               # Shared types and schemas
│   └── schema.ts         # Drizzle schema definitions
└── package.json          # Dependencies and scripts
```

---

## Database Schema

### Users Table
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  first_name VARCHAR(100) NOT NULL,
  last_name VARCHAR(100) NOT NULL,
  role VARCHAR(20) NOT NULL DEFAULT 'user',
  created_at TIMESTAMP DEFAULT NOW()
);
```

**Fields Explanation:**
- `id`: Auto-incrementing primary key
- `email`: Unique email address for login
- `password`: Bcrypt hashed password
- `first_name`: User's first name
- `last_name`: User's last name  
- `role`: Either 'admin' or 'user' for access control
- `created_at`: Account creation timestamp

### Superstore Data Table
```sql
CREATE TABLE superstore_data (
  id SERIAL PRIMARY KEY,
  row_id INTEGER NOT NULL,
  order_id VARCHAR(50) NOT NULL,
  order_date DATE NOT NULL,
  ship_date DATE NOT NULL,
  ship_mode VARCHAR(50) NOT NULL,
  customer_id VARCHAR(50) NOT NULL,
  customer_name VARCHAR(100) NOT NULL,
  segment VARCHAR(50) NOT NULL,
  country VARCHAR(50) NOT NULL,
  city VARCHAR(50) NOT NULL,
  state VARCHAR(50) NOT NULL,
  postal_code VARCHAR(20),
  region VARCHAR(50) NOT NULL,
  product_id VARCHAR(50) NOT NULL,
  category VARCHAR(50) NOT NULL,
  sub_category VARCHAR(50) NOT NULL,
  product_name VARCHAR(200) NOT NULL,
  sales DECIMAL(10,2) NOT NULL,
  quantity INTEGER NOT NULL,
  discount DECIMAL(5,4) NOT NULL,
  profit DECIMAL(10,2) NOT NULL
);
```

**Fields Explanation:**
- Transaction and order information (row_id, order_id, dates)
- Customer details (customer_id, name, segment, location)
- Product information (product_id, category, sub_category, name)
- Financial data (sales, quantity, discount, profit)

---

## Authentication System

### JWT Token Structure
```json
{
  "id": 1,
  "email": "admin@superstore.com",
  "firstName": "SuperStore",
  "lastName": "Admin", 
  "role": "admin",
  "createdAt": "2025-07-02T16:10:26.673Z",
  "iat": 1751472626,
  "exp": 1751559026
}
```

### Password Security
- Passwords are hashed using bcrypt with salt rounds
- Plain text passwords are never stored in database
- JWT tokens expire after 24 hours for security

### Role-Based Access
- **Admin Role**: Full access to all features including data management
- **User Role**: Read-only access to dashboards and analytics

---

## API Endpoints Documentation

### Authentication Endpoints

#### POST /api/auth/register
**Description**: Create new user account
**Body**:
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "password123",
  "confirmPassword": "password123",
  "role": "user"
}
```
**Response**:
```json
{
  "user": {
    "id": 1,
    "email": "john@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "role": "user",
    "createdAt": "2025-07-02T16:00:00.000Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### POST /api/auth/login
**Description**: Authenticate user and return JWT token
**Body**:
```json
{
  "email": "admin@superstore.com",
  "password": "password123"
}
```
**Response**: Same as register response

#### GET /api/auth/me
**Description**: Get current user information
**Headers**: `Authorization: Bearer <token>`
**Response**: User object without password

### Dashboard Endpoints

#### GET /api/dashboard/kpis
**Description**: Get key performance indicators
**Headers**: `Authorization: Bearer <token>`
**Query Parameters**:
- `regions[]`: Array of regions to filter by
- `categories[]`: Array of categories to filter by  
- `dateFrom`: Start date (YYYY-MM-DD)
- `dateTo`: End date (YYYY-MM-DD)

**Response**:
```json
{
  "totalSales": 1988.98,
  "totalProfit": -112.15,
  "totalOrders": 3,
  "totalCustomers": 3,
  "salesGrowth": 12.5,
  "profitGrowth": 8.3,
  "ordersGrowth": 15.2,
  "customersGrowth": 5.7
}
```

#### GET /api/dashboard/charts
**Description**: Get chart data for visualizations
**Headers**: `Authorization: Bearer <token>`
**Response**:
```json
{
  "salesOverTime": [
    {"month": "2023-06", "sales": 829.76, "profit": 17.58}
  ],
  "salesByRegion": [
    {"region": "South", "sales": 1251.48, "percentage": 62.9}
  ],
  "salesByCategory": [
    {"category": "Furniture", "sales": 1251.48, "profit": -93.87}
  ]
}
```

#### GET /api/dashboard/transactions
**Description**: Get recent transaction data
**Headers**: `Authorization: Bearer <token>`
**Response**: Array of transaction objects

#### GET /api/dashboard/top-products
**Description**: Get top performing products
**Headers**: `Authorization: Bearer <token>`
**Query Parameters**: `limit` (default: 5)
**Response**:
```json
[
  {
    "productName": "Bretford CR4500 Series Slim Rectangular Table",
    "category": "Furniture",
    "sales": 957.58,
    "growth": 15.2
  }
]
```

### Admin Data Management Endpoints (Admin Only)

#### GET /api/admin/data
**Description**: Get all business data with filtering
**Headers**: `Authorization: Bearer <token>`
**Access**: Admin role required

#### POST /api/admin/data  
**Description**: Create new data record
**Headers**: `Authorization: Bearer <token>`
**Access**: Admin role required

#### PUT /api/admin/data/:id
**Description**: Update existing data record
**Headers**: `Authorization: Bearer <token>`
**Access**: Admin role required

#### DELETE /api/admin/data/:id
**Description**: Delete data record
**Headers**: `Authorization: Bearer <token>`
**Access**: Admin role required

### Filter Endpoints

#### GET /api/filters
**Description**: Get available filter options
**Headers**: `Authorization: Bearer <token>`
**Response**:
```json
{
  "regions": ["South", "West", "East", "Central"],
  "categories": ["Furniture", "Office Supplies", "Technology"]
}
```

---

## Frontend Components

### Page Components
1. **Login Page** (`/login`)
   - Two-column layout with welcome message
   - VisionPro Analytics branding
   - Form validation with Zod schemas

2. **Register Page** (`/register`)
   - User registration form
   - Role selection (admin/user)
   - Password confirmation validation

3. **Dashboard Page** (`/dashboard`)
   - KPI cards showing key metrics
   - Interactive charts and graphs
   - Data filtering controls
   - Recent transactions table

4. **Analytics Page** (`/analytics`)
   - Statistical analysis tools
   - Correlation analysis
   - Forecasting capabilities
   - Advanced data insights

5. **Data Explorer Page** (`/data`) - Admin Only
   - Full CRUD operations on business data
   - Data table with sorting and filtering
   - Add/Edit/Delete functionality
   - Export capabilities

6. **Reports Page** (`/reports`)
   - Comprehensive business reports
   - Export to CSV functionality
   - Report generation tools

7. **Settings Page** (`/settings`)
   - User preferences
   - Account management
   - Notification settings

### UI Components
- **Sidebar**: Navigation with role-based menu items
- **KPI Cards**: Display key performance indicators
- **Charts**: Interactive data visualizations using Recharts
- **Data Tables**: Sortable and filterable data displays
- **Filter Controls**: Dynamic filtering interface
- **Forms**: Validated forms using React Hook Form + Zod

---

## User Access Levels

### Admin User Capabilities
- ✅ View all dashboards and analytics
- ✅ Access Data Explorer for CRUD operations
- ✅ Create, edit, and delete business records
- ✅ Export data to CSV
- ✅ View all reports and analytics
- ✅ Access user management features
- ✅ Full system access

### Regular User Capabilities  
- ✅ View dashboard with KPIs and charts
- ✅ Access analytics and statistical tools
- ✅ View and generate reports
- ✅ Use filtering and search features
- ✅ Export reports (read-only data)
- ❌ Cannot create, edit, or delete data
- ❌ No access to Data Explorer
- ❌ No administrative functions

---

## Features & Functionality

### Dashboard Features
1. **Key Performance Indicators (KPIs)**
   - Total Sales: Sum of all sales transactions
   - Total Profit: Sum of all profit values
   - Total Orders: Count of unique orders
   - Total Customers: Count of unique customers
   - Growth percentages for each metric

2. **Interactive Charts**
   - Sales Over Time: Line chart showing monthly trends
   - Sales by Region: Pie chart with regional distribution
   - Sales by Category: Bar chart comparing categories
   - All charts update dynamically with applied filters

3. **Data Filtering**
   - Region-based filtering (South, West, East, Central)
   - Category filtering (Furniture, Office Supplies, Technology)
   - Date range filtering with calendar picker
   - Real-time filter application across all components

4. **Recent Transactions Table**
   - Displays latest business transactions
   - Sortable columns
   - Pagination support
   - Quick view of order details

### Analytics Features
1. **Statistical Analysis**
   - Mean, median, mode calculations
   - Standard deviation and variance
   - Correlation analysis between variables
   - Trend analysis and forecasting

2. **Data Insights**
   - Performance comparisons
   - Growth rate calculations
   - Profit margin analysis
   - Customer segmentation insights

### Data Management (Admin Only)
1. **CRUD Operations**
   - Create new business records
   - Edit existing data entries
   - Delete records with confirmation
   - Bulk operations support

2. **Data Validation**
   - Input validation using Zod schemas
   - Data type enforcement
   - Required field validation
   - Business rule validation

3. **Export Capabilities**
   - CSV export functionality
   - Filtered data export
   - Report generation
   - Custom data selection

---

## Installation & Setup

### Prerequisites
- Node.js (v18 or higher)
- PostgreSQL database
- Git for version control

### Environment Variables Required
```env
DATABASE_URL=postgresql://username:password@host:port/database
JWT_SECRET=your-secret-key-here
NODE_ENV=development
PGHOST=database-host
PGPORT=5432
PGUSER=database-user
PGPASSWORD=database-password
PGDATABASE=database-name
```

### Installation Steps
1. **Clone Repository**
   ```bash
   git clone <repository-url>
   cd visionpro-analytics
   ```

2. **Install Dependencies**
   ```bash
   npm install
   ```

3. **Database Setup**
   ```bash
   npm run db:push
   ```

4. **Start Development Server**
   ```bash
   npm run dev
   ```

5. **Access Application**
   - Frontend: http://localhost:5000
   - Backend API: http://localhost:5000/api

### Database Initialization
The system automatically:
- Creates required tables using Drizzle schema
- Seeds sample data on first run
- Sets up indexes for optimal performance

---

## Testing Credentials

### Admin Account
- **Email**: admin@superstore.com
- **Password**: password123
- **Role**: admin
- **Access**: Full system access including data management

### Creating Additional Users
Use the registration page or API endpoint to create new users:
- Regular users get 'user' role by default
- Admin users must be created with 'admin' role specified
- All passwords are securely hashed before storage

### Sample Data
The system includes sample Superstore data with:
- 5+ sample transactions
- Multiple regions (South, West, etc.)
- Various product categories
- Real sales and profit figures for testing

---

## Deployment Information

### Production Build
```bash
npm run build
```

### Environment Configuration
- Set NODE_ENV=production
- Configure production database URL
- Use secure JWT secret key
- Enable HTTPS in production

### Performance Optimizations
- Vite build optimization
- Database query optimization
- Image and asset optimization
- Caching strategies implemented

### Security Features
- JWT token expiration (24 hours)
- Password hashing with bcrypt
- SQL injection prevention via Drizzle ORM
- CORS configuration
- Input validation and sanitization

### Monitoring & Logging
- Request/response logging
- Error tracking and handling
- Performance monitoring
- Database query logging

---

## API Response Examples

### Successful Login Response
```json
{
  "user": {
    "id": 2,
    "email": "admin@superstore.com",
    "firstName": "SuperStore",
    "lastName": "Admin",
    "role": "admin",
    "createdAt": "2025-07-02T16:10:26.673Z"
  },
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwiZW1haWwiOiJhZG1pbkBzdXBlcnN0b3JlLmNvbSIsImZpcnN0TmFtZSI6IlN1cGVyU3RvcmUiLCJsYXN0TmFtZSI6IkFkbWluIiwicm9sZSI6ImFkbWluIiwiY3JlYXRlZEF0IjoiMjAyNS0wNy0wMlQxNjoxMDoyNi42NzNaIiwiaWF0IjoxNzUxNDcyNjI2LCJleHAiOjE3NTE1NTkwMjZ9.93zF53SxOTYLKdek50Cx6nOeKcEWmHYfO929EpTZm9U"
}
```

### Dashboard KPIs Response
```json
{
  "totalSales": 1988.98,
  "totalProfit": -112.14999999999996,
  "totalOrders": 3,
  "totalCustomers": 3,
  "salesGrowth": 12.5,
  "profitGrowth": 8.3,
  "ordersGrowth": 15.2,
  "customersGrowth": 5.7
}
```

### Sample Transaction Data
```json
[
  {
    "id": 1,
    "rowId": 1,
    "orderId": "CA-2023-152156",
    "orderDate": "2023-11-09T00:00:00.000Z",
    "shipDate": "2023-11-12T00:00:00.000Z",
    "shipMode": "Second Class",
    "customerId": "CG-12520",
    "customerName": "Claire Gute",
    "segment": "Consumer",
    "country": "United States",
    "city": "Henderson",
    "state": "Kentucky",
    "postalCode": "42420",
    "region": "South",
    "productId": "FUR-BO-10001798",
    "category": "Furniture",
    "subCategory": "Bookcases",
    "productName": "Bush Somerset Collection Bookcase",
    "sales": "261.96",
    "quantity": 2,
    "discount": "0.0000",
    "profit": "41.91"
  }
]
```

---

## Technical Implementation Details

### Database Connection
- Uses Neon Database (PostgreSQL) with connection pooling
- Drizzle ORM for type-safe database operations
- Automatic migration support with drizzle-kit

### Authentication Flow
1. User submits login credentials
2. Server validates email/password against database
3. JWT token generated with user information
4. Token sent to client and stored in localStorage
5. Subsequent requests include token in Authorization header
6. Server middleware validates token on protected routes

### State Management
- TanStack Query for server state management
- React Context for authentication state
- Local state for UI components
- Automatic cache invalidation on mutations

### Error Handling
- Global error boundaries in React
- API error response standardization
- Validation error handling with Zod
- User-friendly error messages

### Performance Optimizations
- React Query caching and background updates
- Lazy loading of components
- Optimized database queries with proper indexing
- Image optimization and compression

---

## Conclusion

VisionPro Analytics is a comprehensive business intelligence platform that provides powerful data analysis capabilities with a modern, user-friendly interface. The system is built with scalability, security, and performance in mind, making it suitable for businesses of all sizes.

The platform successfully transforms raw business data into actionable insights through interactive dashboards, advanced analytics, and comprehensive reporting tools. With role-based access control and robust security features, it ensures data integrity while providing the flexibility needed for different user types.

For support or additional information, refer to the API documentation and test the system using the provided credentials.

---

**Document Version**: 1.0  
**Last Updated**: July 2, 2025  
**System Version**: VisionPro Analytics v1.0  
**Database Version**: PostgreSQL with Drizzle ORM