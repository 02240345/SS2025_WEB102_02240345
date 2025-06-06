# Practical6_Authentication_Token-based(Email&Password)

A comprehensive guide to implementing secure email and password authentication using TypeScript, Hono framework, PostgreSQL, and Prisma ORM with JWT token-based authorization.

## Project Overview

This tutorial demonstrates how to build a secure REST API with:
- Email and password-based user registration and login
- JWT token generation and verification
- Protected API endpoints with authorization middleware
- Password hashing for secure storage
- Error handling for authentication failures
- Database schema design for user management

## Learning Objectives

By completing this tutorial, you will learn:
- **Password Authentication**: Implement sign-up and login systems with secure token handling
- **Authorization**: Control access to resources based on user authentication status
- **Security Best Practices**: Proper password hashing and JWT token management
- **API Protection**: Secure REST endpoints using middleware authentication

## Prerequisites

### Required Knowledge
- Basic understanding of TypeScript and Node.js
- Familiarity with relational databases (PostgreSQL)
- Understanding of REST API principles
- Basic knowledge of Prisma ORM

### Development Environment
- Bun v1.x or later
- Visual Studio Code with Prisma extension (recommended)
- PostgreSQL database

## Setup Instructions

### Step 1: Clone Repository and Install Dependencies
```bash
git clone https://github.com/rubcstswe/web102-hono-auth-jwt-prisma-forked.git
cd web102-hono-auth-jwt-prisma-forked
bun install
```

### Step 2: Database Schema Setup
The project includes a basic schema with User and Account models:

**User Model:**
- Unique ID and email fields
- Hashed password storage
- One-to-many relationship with accounts

**Account Model:**
- User relationship with foreign key
- Balance tracking
- Default balance initialization

### Step 3: Update Database Schema
Add password hashing field to User model:
```bash
bunx prisma db push
bunx prisma generate
```

## Authentication Concepts

### Authentication vs Authorization
- **Authentication**: Verifies user identity ("Who are you?")
- **Authorization**: Determines access permissions ("What can you do?")

### Authentication Flow
1. **Registration**: User provides email and password, server stores hashed password
2. **Login**: User authenticates with credentials, receives JWT token
3. **Access**: User includes JWT token in requests to protected endpoints

### Security Implementation
- **Password Hashing**: Uses bcrypt to securely store passwords
- **JWT Tokens**: Stateless authentication with expiration times
- **Protected Routes**: Middleware verification for sensitive endpoints

## API Endpoints

### Public Endpoints

#### User Registration
- **Method**: POST
- **Endpoint**: `/register`
- **Body**: `{ "email": "user@example.com", "password": "password123" }`
- **Response**: `{ "message": "User created successfully" }`

#### User Login
- **Method**: POST
- **Endpoint**: `/login`
- **Body**: `{ "email": "user@example.com", "password": "password123" }`
- **Response**: `{ "message": "Login successful", "token": "JWT_TOKEN" }`

### Protected Endpoints

#### Get Account Balance
- **Method**: GET
- **Endpoint**: `/protected/account/balance`
- **Headers**: `Authorization: Bearer JWT_TOKEN`
- **Response**: `{ "data": { "Account": [{ "balance": 0, "id": "account-id" }] } }`

## Implementation Details

### Registration Endpoint Implementation
Key features:
- **Password Hashing**: Uses Bun's bcrypt implementation with cost factor 4
- **User Creation**: Creates user with hashed password and default account
- **Error Handling**: Handles unique constraint violations for duplicate emails
- **Account Initialization**: Automatically creates account with zero balance

### Login Endpoint Implementation
Key features:
- **Credential Verification**: Validates email and password against database
- **JWT Generation**: Creates signed token with user ID and expiration
- **Security**: Returns 401 status for invalid credentials
- **Token Payload**: Includes user ID (sub) and expiration time

### Authorization Middleware
Key features:
- **JWT Verification**: Validates tokens using Hono's JWT middleware
- **Route Protection**: Protects all `/protected/*` endpoints
- **Token Extraction**: Automatically extracts and verifies Authorization header
- **Payload Access**: Makes decoded token payload available to route handlers

### Error Handling
Comprehensive error management:
- **Prisma Errors**: Specific handling for database constraint violations
- **Authentication Errors**: Clear messages for invalid credentials
- **Authorization Errors**: Proper 401 responses for unauthorized access
- **Validation Errors**: User-friendly error messages

## Security Best Practices

### Password Security
- **Hashing Algorithm**: Uses bcrypt with appropriate cost factor
- **No Plain Text**: Never stores passwords in readable format
- **Salt Integration**: bcrypt automatically handles salt generation
- **Verification**: Secure password comparison using bcrypt verify

### JWT Token Security
- **Secret Key**: Uses secure secret for token signing (should be environment variable)
- **Expiration**: Tokens expire after 60 minutes
- **Payload**: Minimal payload with user ID and expiration
- **Verification**: Automatic token validation on protected routes

### API Security
- **CORS Configuration**: Proper cross-origin request handling
- **HTTP Status Codes**: Appropriate status codes for different scenarios
- **Error Messages**: Informative but not revealing sensitive information
- **Route Protection**: Middleware-based authentication for sensitive endpoints

## Testing the Implementation

### Registration Testing
```bash
curl -X POST http://localhost:3000/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

### Login Testing
```bash
curl -X POST http://localhost:3000/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

### Protected Endpoint Testing
```bash
curl -X GET http://localhost:3000/protected/account/balance \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## Common Issues and Solutions

### Authentication Failures
- Verify email and password are correct
- Check database connection and user existence
- Ensure password hashing is working properly
- Validate JWT token format and expiration

### Authorization Issues
- Confirm JWT token is included in Authorization header
- Verify token format: "Bearer TOKEN"
- Check token expiration and validity
- Ensure middleware is properly configured

### Database Errors
- Verify Prisma schema is up to date
- Check database connection string
- Ensure migrations are applied
- Validate unique constraints

## Project Structure
```
project/
├── src/
│   └── index.ts          # Main application file
├── prisma/
│   └── schema.prisma     # Database schema
├── package.json          # Dependencies and scripts
└── README.md            # Documentation
```

## Future Enhancements

### Advanced Authentication
- Email verification for registration
- Password reset functionality
- Multi-factor authentication
- OAuth integration with third-party providers

### Enhanced Security
- Rate limiting for authentication endpoints
- Account lockout after failed attempts
- Token refresh mechanism
- Role-based access control (RBAC)

### Monitoring and Logging
- Authentication attempt logging
- Security event monitoring
- Performance metrics tracking
- Error reporting and alerting

## Best Practices Summary

1. **Never store plain text passwords** - Always use proper hashing
2. **Use secure JWT secrets** - Store in environment variables
3. **Implement proper error handling** - Provide clear but secure error messages
4. **Validate all inputs** - Check email format and password strength
5. **Use HTTPS in production** - Encrypt all authentication traffic
6. **Implement token expiration** - Limit token lifetime for security
7. **Monitor authentication events** - Track login attempts and failures

## Resources

- [Hono Framework Documentation](https://hono.dev/)
- [Prisma ORM Documentation](https://www.prisma.io/docs/)
- [JWT Best Practices](https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/)
- [bcrypt Password Hashing](https://github.com/kelektiv/node.bcrypt.js)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)