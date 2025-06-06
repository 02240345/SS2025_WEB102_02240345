# Practical4_Connecting_Tiktok_To_PostgreSQL_With_Prisma_ORM

A comprehensive guide to connecting a TikTok clone application to PostgreSQL database using Prisma ORM, implementing authentication, and migrating from in-memory storage to persistent database storage.

## Project Overview

This implementation transforms a TikTok clone from using in-memory data to a robust PostgreSQL database with:
- PostgreSQL database setup and configuration
- Prisma ORM for type-safe database operations
- User authentication with JWT tokens and password encryption
- Database schema design for social media features
- Migration from in-memory models to persistent storage

## Objectives

- Set up PostgreSQL database for TikTok clone application
- Configure Prisma ORM for database interactions
- Migrate from in-memory data models to persistent storage
- Implement secure authentication with password encryption
- Update RESTful API endpoints to use database operations

## Setup Instructions

### Part 1: PostgreSQL Database Setup

#### Step 1: Create Database
Access PostgreSQL command line and create database:
```bash
sudo -u postgres psql
```

Create database and user:
```sql
CREATE DATABASE tiktok_db;
CREATE USER tiktok_user WITH ENCRYPTED PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE tiktok_db TO tiktok_user;
\q
```

### Part 2: Prisma ORM Configuration

#### Step 1: Install Prisma Dependencies
Navigate to server project and install Prisma:
```bash
cd server
npm install @prisma/client
npm install prisma --save-dev
```

#### Step 2: Initialize Prisma
```bash
npx prisma init
```

This creates:
- `prisma/` directory with `schema.prisma` file
- `.env` file for environment variables

#### Step 3: Configure Database Connection
Edit `.env` file with database connection string:
```
DATABASE_URL="postgresql://tiktok_user:your_password@localhost:5432/tiktok_db?schema=public"
```

#### Step 4: Install Additional Dependencies
```bash
npm install bcrypt jsonwebtoken
```

### Part 3: Database Schema Creation

#### Step 1: Define Prisma Schema
Update `prisma/schema.prisma` with TikTok data model including:
- User model with authentication fields
- Video model with metadata and relationships
- Comment model for video interactions
- Like models for videos and comments
- Follow model for user relationships

#### Step 2: Create Migration
```bash
npx prisma migrate dev --name init
```

This command:
- Creates SQL migration files in `prisma/migrations/`
- Applies migration to database
- Generates Prisma Client

#### Step 3: Create Prisma Client Instance
Create `src/lib/prisma.js` for database connection management and client instantiation.

### Part 4: Authentication Implementation

#### Step 1: Create Authentication Middleware
Create `src/middleware/auth.js` with:
- JWT token verification
- User authentication checking
- Protected route handling
- Error handling for invalid tokens

#### Step 2: Environment Variables Configuration
Update `.env` with authentication settings:
```
PORT=5000
NODE_ENV=development
DATABASE_URL="postgresql://tiktok_user:your_password@localhost:5432/tiktok_db?schema=public"
JWT_SECRET=yourverylongandsecurerandomsecret
JWT_EXPIRE=30d
```

## Implementation Details

### Database Schema Design

#### User Model
- Unique username and email fields
- Password hashing with bcrypt
- Profile information (display name, bio, avatar)
- Timestamps for account creation and updates

#### Video Model
- User relationship (foreign key)
- Video metadata (title, description, URL)
- Engagement tracking (likes, comments count)
- Timestamps for upload and modification

#### Comment Model
- User and video relationships
- Comment content and timestamps
- Like tracking for comments

#### Like Models
- Separate models for video likes and comment likes
- User relationships to track who liked what
- Unique constraints to prevent duplicate likes

#### Follow Model
- Follower and following user relationships
- Timestamps for follow actions
- Unique constraints for follow relationships

### Controller Updates

#### User Controller
Key implementations:
- **Password Hashing**: Using bcrypt for secure password storage
- **JWT Generation**: Creating tokens for authenticated sessions
- **Database Queries**: Using Prisma Client for user operations
- **Registration/Login**: Secure user authentication flow

#### Video Controller
Key implementations:
- **Complex Queries**: Fetching videos with user and engagement data
- **Relationships**: Including related data in responses
- **Transactions**: Ensuring data consistency for multi-table operations
- **Aggregations**: Counting likes and comments efficiently

#### Comment Controller
Key implementations:
- **Nested Relationships**: Comments with user and video data
- **Like Management**: Handling comment likes and unlikes
- **Data Validation**: Ensuring comment integrity

### Authentication Flow

#### Registration Process
1. Validate user input (username, email, password)
2. Check for existing users with same username/email
3. Hash password using bcrypt
4. Create user record in database
5. Generate JWT token for immediate login

#### Login Process
1. Validate credentials (username/email and password)
2. Find user in database
3. Compare provided password with hashed password
4. Generate JWT token on successful authentication
5. Return user data and token

#### Protected Routes
- Middleware checks for valid JWT token
- Extracts user information from token
- Attaches user data to request object
- Allows access to protected endpoints

## Testing Instructions

### Step 1: Start the Server
```bash
npm run dev
```

### Step 2: Test Registration and Login
Use Postman to test authentication endpoints:

**Register New User:**
- POST `/api/users/register`
- Body: `{ "username": "testuser", "email": "test@example.com", "password": "password123" }`

**Login User:**
- POST `/api/users/login`
- Body: `{ "username": "testuser", "password": "password123" }`

### Step 3: Test Protected Routes
Use JWT token from login response:

**Create Video (Protected):**
- POST `/api/videos`
- Headers: `Authorization: Bearer <your_jwt_token>`
- Body: Video data

**Get User Profile (Protected):**
- GET `/api/users/profile`
- Headers: `Authorization: Bearer <your_jwt_token>`

### Database Seeding

#### Step 1: Create Seed File
Create `prisma/seed.js` with test data generation:
- 10 test users with hashed passwords
- 50 videos (5 per user)
- 200 comments across videos
- 300 video likes
- 150 comment likes
- 40 follow relationships

#### Step 2: Add Seed Script
Update `package.json`:
```json
{
  "scripts": {
    "dev": "nodemon src/index.js",
    "start": "node src/index.js",
    "seed": "node prisma/seed.js"
  }
}
```

#### Step 3: Run Seed Script
```bash
npm run seed
```

## Key Concepts

### Database Schema Design
- **Tables**: Represent entities (users, videos, comments)
- **Relationships**: Connections between tables (one-to-many, many-to-many)
- **Indexes**: Improve query performance for frequently accessed data
- **Foreign Keys**: Maintain data integrity and referential constraints

### Object-Relational Mapping (ORM)
- **Type Safety**: Prisma provides TypeScript support for database operations
- **Query Builder**: Intuitive API for complex database queries
- **Migration Management**: Version control for database schema changes
- **Relationship Handling**: Automatic joins and data fetching

### Authentication and Security
- **Password Hashing**: bcrypt for secure password storage
- **JWT Tokens**: Stateless authentication for API access
- **Protected Routes**: Middleware-based route protection
- **Token Validation**: Secure token verification and user identification

### Prisma Features
- **Model Definitions**: Schema-first approach to database design
- **Migrations**: Automated database schema updates
- **Client Generation**: Type-safe database client generation
- **Transactions**: ACID compliance for complex operations

## Common Issues and Solutions

### Database Connection Issues
- Verify PostgreSQL is running
- Check database credentials in `.env`
- Ensure database and user exist
- Confirm network connectivity

### Migration Problems
- Reset database if needed: `npx prisma migrate reset`
- Check schema syntax in `schema.prisma`
- Verify database permissions
- Review migration files for conflicts

### Authentication Errors
- Verify JWT secret is set in environment
- Check token format in requests
- Ensure middleware is properly configured
- Validate user permissions

## Project Structure
```
server/
├── prisma/
│   ├── migrations/
│   ├── schema.prisma
│   └── seed.js
├── src/
│   ├── controllers/
│   │   ├── userController.js
│   │   ├── videoController.js
│   │   └── commentController.js
│   ├── middleware/
│   │   └── auth.js
│   ├── lib/
│   │   └── prisma.js
│   ├── routes/
│   └── app.js
├── .env
└── package.json
```

## Future Enhancements

### Advanced Features
- Real-time notifications with WebSockets
- File upload integration for videos and images
- Advanced search and filtering capabilities
- Content moderation and reporting system

### Performance Optimizations
- Database indexing for frequently queried fields
- Caching layer with Redis
- Query optimization and pagination
- Connection pooling for database efficiency

### Security Improvements
- Rate limiting for API endpoints
- Input validation and sanitization
- CORS configuration for production
- Security headers and HTTPS enforcement

## Resources

- [Prisma Documentation](https://www.prisma.io/docs/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [JWT Authentication Guide](https://jwt.io/introduction/)
- [bcrypt Password Hashing](https://github.com/kelektiv/node.bcrypt.js)