# Practical1_API_Design_And_Implementation

A comprehensive guide to designing and implementing RESTful API endpoints for a social media platform using Node.js and Express.js.

## Project Overview

This project demonstrates the design and implementation of a complete RESTful API for a social media platform similar to Instagram, featuring:
- Proper REST endpoint design
- HTTP methods and status codes
- Content negotiation with different MIME types
- Error handling and validation
- API documentation

## Resources Managed

- **Users** - User profiles and authentication
- **Posts** - Social media posts with images and captions
- **Comments** - Comments on posts
- **Likes** - Like functionality for posts and comments
- **Followers** - User following relationships

## API Design Principles

### RESTful Conventions
- Use nouns for resource names
- HTTP methods indicate actions (GET, POST, PUT, DELETE)
- Consistent URI structure
- Proper status codes for responses
- Stateless communication

### URI Design Patterns
- Collection endpoints: `/users`, `/posts`
- Resource endpoints: `/users/{id}`, `/posts/{id}`
- Nested resources: `/posts/{id}/comments`
- Query parameters for filtering and pagination

## Setup Instructions

### Step 1: Project Initialization
Create and set up the project:
```bash
mkdir social-media-api
cd social-media-api
npm init -y
```

### Step 2: Install Dependencies
Install required packages:
```bash
npm install express morgan cors helmet
npm install nodemon --save-dev
```

### Step 3: Project Structure
Create the directory structure:
```bash
mkdir -p controllers routes middleware config utils public
touch server.js .env .gitignore
```

### Step 4: Environment Configuration
Create `.env` file:
```
PORT=3000
NODE_ENV=development
```

### Step 5: Package.json Scripts
Update package.json with development scripts:
```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  }
}
```

## API Endpoint Design

### Users Resource

| Endpoint | Method | Description | Request Body | Response |
|----------|--------|-------------|--------------|----------|
| `/users` | GET | List all users | None | User list with pagination |
| `/users` | POST | Create new user | User data | Created user object |
| `/users/{id}` | GET | Get specific user | None | User object |
| `/users/{id}` | PUT | Update user | Updated user data | Updated user object |
| `/users/{id}` | DELETE | Delete user | None | Success confirmation |

### Posts Resource

| Endpoint | Method | Description | Request Body | Response |
|----------|--------|-------------|--------------|----------|
| `/posts` | GET | List all posts | None | Post list with pagination |
| `/posts` | POST | Create new post | Post data | Created post object |
| `/posts/{id}` | GET | Get specific post | None | Post object |
| `/posts/{id}` | PUT | Update post | Updated post data | Updated post object |
| `/posts/{id}` | DELETE | Delete post | None | Success confirmation |

### Comments Resource

| Endpoint | Method | Description | Request Body | Response |
|----------|--------|-------------|--------------|----------|
| `/posts/{id}/comments` | GET | Get post comments | None | Comment list |
| `/posts/{id}/comments` | POST | Add comment | Comment data | Created comment |
| `/comments/{id}` | PUT | Update comment | Updated comment | Updated comment |
| `/comments/{id}` | DELETE | Delete comment | None | Success confirmation |

## Implementation Details

### Server Setup
Basic Express server configuration with:
- CORS middleware for cross-origin requests
- Morgan for request logging
- Helmet for security headers
- JSON body parsing
- Error handling middleware

### Controllers
Organized business logic into separate controller files:
- **User Controller**: Handles user CRUD operations
- **Post Controller**: Manages post operations
- **Comment Controller**: Handles comment functionality
- **Like Controller**: Manages like/unlike operations
- **Follower Controller**: Handles follow relationships

### Routes
RESTful route definitions with:
- Proper HTTP method mapping
- Parameter validation
- Middleware integration
- Error handling

### Middleware
Custom middleware for:
- Error handling and formatting
- Async operation wrapping
- Content negotiation
- Response formatting

## HTTP Status Codes

### Success Codes
- **200 OK** - Successful GET, PUT requests
- **201 Created** - Successful POST requests
- **204 No Content** - Successful DELETE requests

### Client Error Codes
- **400 Bad Request** - Invalid request data
- **401 Unauthorized** - Authentication required
- **403 Forbidden** - Access denied
- **404 Not Found** - Resource not found
- **409 Conflict** - Resource conflict

### Server Error Codes
- **500 Internal Server Error** - Server-side errors

## Content Negotiation

### Supported MIME Types
- **application/json** - Default JSON responses
- **application/xml** - XML format responses
- **text/html** - HTML formatted responses

### Implementation
Content negotiation middleware checks Accept headers and formats responses accordingly:
- Automatic format detection
- Fallback to JSON for unsupported types
- Consistent data structure across formats

## Error Handling

### Error Response Format
Standardized error responses:
```json
{
  "success": false,
  "error": "Error message",
  "statusCode": 400
}
```

### Error Types
- Validation errors
- Authentication errors
- Authorization errors
- Resource not found errors
- Server errors

## API Documentation

### Interactive Documentation
HTML documentation page with:
- Endpoint descriptions
- Request/response examples
- Status code explanations
- Authentication requirements

### Testing Examples
Sample requests for each endpoint:
- cURL commands
- Request headers
- Request bodies
- Expected responses

## Testing the API

### Basic Testing
Start the server and test endpoints:
```bash
npm run dev
```

### Sample Requests

#### Get All Users
```bash
curl -X GET http://localhost:3000/api/users
```

#### Create New User
```bash
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"username":"newuser","email":"user@example.com"}'
```

#### Get Specific Post
```bash
curl -X GET http://localhost:3000/api/posts/1
```

### Content Negotiation Testing
Test different response formats:
```bash
curl -H "Accept: application/xml" http://localhost:3000/api/users
curl -H "Accept: text/html" http://localhost:3000/api/users
```

## Best Practices Implemented

### API Design
- Consistent naming conventions
- Proper HTTP method usage
- Meaningful status codes
- Clear error messages

### Code Organization
- Separation of concerns
- Modular architecture
- Reusable middleware
- Clean controller structure

### Security
- Input validation
- Error message sanitization
- Security headers with Helmet
- CORS configuration

### Performance
- Efficient routing
- Minimal middleware overhead
- Proper error handling
- Response caching headers

## Future Enhancements

### Authentication
- JWT token implementation
- User authentication middleware
- Role-based access control
- OAuth integration

### Database Integration
- MongoDB/PostgreSQL connection
- Data validation with schemas
- Migration scripts
- Connection pooling

### Advanced Features
- File upload handling
- Real-time notifications
- Rate limiting
- API versioning

### Monitoring
- Request logging
- Performance metrics
- Error tracking
- Health check endpoints

## Common Issues and Solutions

### CORS Errors
- Configure CORS middleware properly
- Set appropriate origins
- Handle preflight requests

### Content Type Issues
- Ensure proper Content-Type headers
- Validate request body format
- Handle missing content types

### Error Handling
- Implement global error handler
- Provide meaningful error messages
- Log errors for debugging

## Project Structure
```
social-media-api/
├── controllers/
│   ├── userController.js
│   ├── postController.js
│   ├── commentController.js
│   ├── likeController.js
│   └── followerController.js
├── routes/
│   ├── users.js
│   ├── posts.js
│   ├── comments.js
│   ├── likes.js
│   └── followers.js
├── middleware/
│   ├── errorHandler.js
│   ├── async.js
│   └── formatResponse.js
├── utils/
│   ├── mockData.js
│   └── errorResponse.js
├── public/
│   └── docs.html
├── server.js
├── .env
└── package.json