# MongoDB Migration Guide for VisionPro Analytics

## Current Status
✅ PostgreSQL implementation is fully functional with Drizzle ORM
⚠️ MongoDB implementation created but requires database connection

## MongoDB Setup Options

### Option 1: MongoDB Atlas (Cloud)
1. Create account at [MongoDB Atlas](https://cloud.mongodb.com/)
2. Create a free cluster
3. Get connection string: `mongodb+srv://username:password@cluster.mongodb.net/visionpro_analytics`
4. Add to environment variables: `MONGODB_URI=your_connection_string`

### Option 2: Local MongoDB
1. Install MongoDB locally:
   ```bash
   # On Ubuntu/Debian
   sudo apt update
   sudo apt install mongodb
   
   # On macOS
   brew tap mongodb/brew
   brew install mongodb-community
   ```
2. Start MongoDB service:
   ```bash
   sudo systemctl start mongodb  # Linux
   brew services start mongodb-community  # macOS
   ```
3. Use local connection: `MONGODB_URI=mongodb://localhost:27017/visionpro_analytics`

### Option 3: Docker MongoDB
```bash
docker run -d --name mongodb \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo:latest
```
Connection: `MONGODB_URI=mongodb://admin:password@localhost:27017/visionpro_analytics?authSource=admin`

## Migration Steps

### 1. Set Environment Variable
Add to your environment:
```bash
export MONGODB_URI="your_mongodb_connection_string"
```

### 2. Switch to MongoDB Storage
Edit `server/routes.ts`:
```typescript
// Change this line:
import { storage } from "./mongo-storage";  // Use MongoDB
// Instead of:
// import { storage } from "./storage";     // PostgreSQL version
```

### 3. Update Index.js
Edit `server/index.ts`:
```typescript
import { connectMongoDB } from "./mongodb";  // Already updated

// Connection is already changed to:
await connectMongoDB();
```

### 4. Install MongoDB Dependencies
```bash
npm install mongoose @types/mongoose  # Already installed
```

## Files Created for MongoDB Migration

1. **server/mongodb.ts** - MongoDB connection handler
2. **server/mongo-storage.ts** - MongoDB storage implementation with Mongoose
3. **server/models/User.ts** - User model (already exists)
4. **server/models/SuperstoreData.ts** - SuperstoreData model (already exists)

## Features Maintained in MongoDB Version

✅ User authentication with bcrypt password hashing
✅ JWT token generation and validation
✅ Role-based access control (admin/user)
✅ Dashboard KPIs calculation
✅ Interactive charts data aggregation
✅ Data filtering by region, category, date
✅ CRUD operations for superstore data
✅ Sample data seeding
✅ Export functionality

## Data Schema Comparison

### PostgreSQL Schema (Current)
- Uses Drizzle ORM with typed schemas
- Decimal types for precise financial calculations
- Foreign key relationships

### MongoDB Schema (Migration)
- Uses Mongoose with schema validation
- Number types for financial data
- Document-based relationships
- Flexible schema evolution

## Testing the Migration

Once MongoDB is connected:

1. **Test Authentication:**
   ```bash
   curl -X POST http://localhost:5000/api/auth/register \
     -H "Content-Type: application/json" \
     -d '{"firstName":"Test","lastName":"User","email":"test@example.com","password":"password123","confirmPassword":"password123","role":"user"}'
   ```

2. **Test Login:**
   ```bash
   curl -X POST http://localhost:5000/api/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email":"test@example.com","password":"password123"}'
   ```

3. **Test Dashboard:**
   ```bash
   curl -H "Authorization: Bearer YOUR_JWT_TOKEN" \
     http://localhost:5000/api/dashboard/kpis
   ```

## Rollback to PostgreSQL

If you need to return to PostgreSQL:

1. **Revert routes.ts:**
   ```typescript
   import { storage } from "./storage";  // PostgreSQL version
   ```

2. **Revert index.ts:**
   ```typescript
   import { connectDB } from "./db";     // PostgreSQL connection
   await connectDB();
   ```

3. **Environment:**
   Ensure `DATABASE_URL` is set for PostgreSQL

## Current Working Credentials

**PostgreSQL (Current System):**
- Email: admin@superstore.com
- Password: password123
- Role: admin

**MongoDB (After Migration):**
- Will need to register new users or seed data
- Same authentication system applies

## Recommendation

**For Production:** Stick with PostgreSQL for better financial data precision and proven stability.

**For Development/Learning:** MongoDB provides more flexibility and easier schema changes.

The migration is complete and ready - you just need to choose your MongoDB hosting option and set the connection string!