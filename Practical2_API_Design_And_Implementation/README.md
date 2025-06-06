# Practical2_TikTok_REST_API_Backend_Implementation

A complete RESTful API for a TikTok-like social media platform built with Express.js, featuring video management, user interactions, and social features.

## Project Overview

This API provides backend functionality for a TikTok clone with:
- Video upload and management
- User profiles and authentication
- Social interactions (likes, comments, follows)
- RESTful endpoint design
- In-memory data storage for development

## API Resources

### Core Resources
- **Videos** - Video content with metadata
- **Users** - User profiles and account management
- **Comments** - Comments on videos

### API Endpoints Overview

| Resource | Method | Endpoint | Description |
|----------|--------|----------|-------------|
| Videos | GET | `/api/videos` | Get all videos |
| Videos | POST | `/api/videos` | Create new video |
| Videos | GET | `/api/videos/:id` | Get specific video |
| Videos | PUT | `/api/videos/:id` | Update video |
| Videos | DELETE | `/api/videos/:id` | Delete video |
| Videos | GET | `/api/videos/:id/comments` | Get video comments |
| Videos | GET | `/api/videos/:id/likes` | Get video likes |
| Videos | POST | `/api/videos/:id/likes` | Like video |
| Videos | DELETE | `/api/videos/:id/likes` | Unlike video |
| Users | GET | `/api/users` | Get all users |
| Users | POST | `/api/users` | Create user |
| Users | GET | `/api/users/:id` | Get specific user |
| Users | PUT | `/api/users/:id` | Update user |
| Users | DELETE | `/api/users/:id` | Delete user |
| Users | GET | `/api/users/:id/videos` | Get user's videos |
| Users | GET | `/api/users/:id/followers` | Get user followers |
| Users | POST | `/api/users/:id/followers` | Follow user |
| Users | DELETE | `/api/users/:id/followers` | Unfollow user |
| Users | GET | `/api/users/:id/following` | Get following users |
| Comments | GET | `/api/comments` | Get all comments |
| Comments | POST | `/api/comments` | Create comment |
| Comments | GET | `/api/comments/:id` | Get specific comment |
| Comments | PUT | `/api/comments/:id` | Update comment |
| Comments | DELETE | `/api/comments/:id` | Delete comment |
| Comments | GET | `/api/comments/:id/likes` | Get comment likes |
| Comments | POST | `/api/comments/:id/likes` | Like comment |
| Comments | DELETE | `/api/comments/:id/likes` | Unlike comment |

## Setup Instructions

### Step 1: Project Initialization
Create and set up the project directory:
```bash
mkdir -p server
cd server
npm init -y
```

### Step 2: Install Dependencies
Install required packages:
```bash
npm install express cors morgan body-parser dotenv
npm install --save-dev nodemon
```

**Dependencies explained:**
- **express**: Web server framework
- **cors**: Enable Cross-Origin Resource Sharing
- **morgan**: HTTP request logger
- **body-parser**: Parse incoming request bodies
- **dotenv**: Load environment variables

### Step 3: Create Project Structure
Set up the directory structure:
```bash
mkdir -p src/controllers src/routes src/models src/middleware src/utils
touch src/app.js src/server.js .env
```

### Step 4: Environment Configuration
Create `.env` file:
```
PORT=3000
NODE_ENV=development
```

### Step 5: Express Application Setup
Create `src/app.js` with basic Express configuration:
- CORS middleware for cross-origin requests
- Morgan for request logging
- Body parser for JSON requests
- Route mounting for API endpoints
- Error handling middleware

### Step 6: Server Entry Point
Create `src/server.js` to start the application:
- Load environment variables
- Import Express app
- Start server on specified port
- Add error handling for server startup

### Step 7: Package.json Scripts
Update package.json with development scripts:
```json
{
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js"
  }
}
```

## Implementation Details

### Data Models
Create `src/models/index.js` with in-memory data structures:
- **Users array**: User profiles with IDs, usernames, and metadata
- **Videos array**: Video objects with user associations and engagement data
- **Comments array**: Comment objects linked to videos and users

### Controllers

#### Video Controller (`src/controllers/videoController.js`)
Handles video-related operations:
- **getAllVideos**: Retrieve all videos with user information
- **getVideoById**: Get specific video details
- **createVideo**: Add new video to the platform
- **updateVideo**: Modify existing video metadata
- **deleteVideo**: Remove video from platform
- **getVideoComments**: Retrieve comments for specific video
- **likeVideo/unlikeVideo**: Handle video like interactions

#### User Controller (`src/controllers/userController.js`)
Manages user operations:
- **getAllUsers**: Get list of all users
- **getUserById**: Retrieve specific user profile
- **createUser**: Register new user account
- **updateUser**: Modify user profile information
- **deleteUser**: Remove user account
- **getUserVideos**: Get videos uploaded by specific user
- **followUser/unfollowUser**: Handle follow relationships

#### Comment Controller (`src/controllers/commentController.js`)
Handles comment functionality:
- **getAllComments**: Retrieve all comments
- **getCommentById**: Get specific comment details
- **createComment**: Add new comment to video
- **updateComment**: Modify existing comment
- **deleteComment**: Remove comment
- **likeComment/unlikeComment**: Handle comment like interactions

### Routes

#### Video Routes (`src/routes/videos.js`)
RESTful routes for video operations:
- GET `/` - List all videos
- POST `/` - Create new video
- GET `/:id` - Get specific video
- PUT `/:id` - Update video
- DELETE `/:id` - Delete video
- GET `/:id/comments` - Get video comments
- POST `/:id/likes` - Like video
- DELETE `/:id/likes` - Unlike video

#### User Routes (`src/routes/users.js`)
User management routes:
- GET `/` - List all users
- POST `/` - Create new user
- GET `/:id` - Get specific user
- PUT `/:id` - Update user
- DELETE `/:id` - Delete user
- GET `/:id/videos` - Get user's videos
- GET `/:id/followers` - Get user followers
- POST `/:id/followers` - Follow user
- DELETE `/:id/followers` - Unfollow user
- GET `/:id/following` - Get following users

#### Comment Routes (`src/routes/comments.js`)
Comment management routes:
- GET `/` - List all comments
- POST `/` - Create new comment
- GET `/:id` - Get specific comment
- PUT `/:id` - Update comment
- DELETE `/:id` - Delete comment
- POST `/:id/likes` - Like comment
- DELETE `/:id/likes` - Unlike comment

## Testing the API

### Start the Server
```bash
npm run dev
```

### Basic API Tests

#### Get All Users
```bash
curl -X GET http://localhost:3000/api/users
```

#### Get All Videos
```bash
curl -X GET http://localhost:3000/api/videos
```

#### Get Specific User
```bash
curl -X GET http://localhost:3000/api/users/1
```

#### Get Specific Video
```bash
curl -X GET http://localhost:3000/api/videos/1
```

#### Get User's Videos
```bash
curl -X GET http://localhost:3000/api/users/1/videos
```

#### Get Video Comments
```bash
curl -X GET http://localhost:3000/api/videos/1/comments
```

#### Create New User
```bash
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"username":"newuser","email":"user@example.com","displayName":"New User"}'
```

#### Create New Video
```bash
curl -X POST http://localhost:3000/api/videos \
  -H "Content-Type: application/json" \
  -d '{"userId":1,"title":"My Video","description":"Test video","videoUrl":"https://example.com/video.mp4"}'
```

### Testing with Postman
1. Import API endpoints into Postman collection
2. Set base URL to `http://localhost:3000`
3. Test each endpoint with appropriate HTTP methods
4. Verify response status codes and data format

## Response Formats

### Success Response
```json
{
  "success": true,
  "data": {
    "id": 1,
    "username": "user123",
    "email": "user@example.com"
  }
}
```

### Error Response
```json
{
  "success": false,
  "error": "User not found",
  "statusCode": 404
}
```

### List Response
```json
{
  "success": true,
  "count": 10,
  "data": [
    {
      "id": 1,
      "title": "Video 1"
    }
  ]
}
```

## Error Handling

### HTTP Status Codes
- **200 OK**: Successful GET, PUT requests
- **201 Created**: Successful POST requests
- **204 No Content**: Successful DELETE requests
- **400 Bad Request**: Invalid request data
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server errors

### Error Types
- Validation errors for invalid input
- Not found errors for missing resources
- Conflict errors for duplicate data
- Server errors for unexpected issues

## Project Structure
```
server/
├── src/
│   ├── controllers/
│   │   ├── videoController.js
│   │   ├── userController.js
│   │   └── commentController.js
│   ├── routes/
│   │   ├── videos.js
│   │   ├── users.js
│   │   └── comments.js
│   ├── models/
│   │   └── index.js
│   ├── middleware/
│   ├── utils/
│   ├── app.js
│   └── server.js
├── .env
├── package.json
└── README.md
```

## Future Enhancements

### Database Integration
- Replace in-memory storage with MongoDB or PostgreSQL
- Add data persistence and relationships
- Implement proper data validation

### Authentication & Security
- Add JWT-based authentication
- Implement user sessions and authorization
- Add input validation and sanitization
- Implement rate limiting

### Advanced Features
- File upload for videos and images
- Real-time notifications
- Search and filtering capabilities
- Pagination for large datasets
- Caching for improved performance

### Production Readiness
- Add comprehensive error logging
- Implement health check endpoints
- Add API documentation with Swagger
- Set up monitoring and analytics
- Configure for deployment environments