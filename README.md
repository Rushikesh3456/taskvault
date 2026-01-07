# TaskVault - Backend Developer Intern Assignment

## Project Overview
TaskVault is a scalable REST API with JWT-based authentication, role-based access control (RBAC), and a modern React frontend for task management. Built with Node.js, Express, MongoDB, and React.

## Tech Stack
- **Backend**: Node.js, Express.js, MongoDB, Mongoose, JWT, bcrypt
- **Frontend**: React.js, Axios, React Context API
- **Database**: MongoDB
- **Authentication**: JWT (JSON Web Tokens)
- **Security**: bcrypt for password hashing, helmet, CORS, input validation

## Features

### Backend
- **User Authentication**: Registration and login with JWT
- **Role-Based Access Control**: User and Admin roles
- **Task Management**: Full CRUD operations for tasks
- **API Versioning**: API routes under `/api/v1`
- **Error Handling**: Centralized error handling middleware
- **Input Validation**: Request validation and sanitization
- **API Documentation**: Swagger/Postman documentation

### Frontend
- **User Authentication**: Register and login flows
- **Protected Routes**: Private routes requiring JWT
- **Dashboard**: Protected dashboard for authenticated users
- **Task Management**: Create, read, update, delete tasks
- **Status Tracking**: Task status updates (pending, in-progress, completed)
- **User Feedback**: Error and success message display

## Project Structure
```
taskvault/
├── backend/
│   ├── config/
│   │   └── database.js
│   ├── models/
│   │   ├── User.js
│   │   └── Task.js
│   ├── controllers/
│   │   ├── authController.js
│   │   └── taskController.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── taskRoutes.js
│   │   └── adminRoutes.js
│   ├── middleware/
│   │   ├── authMiddleware.js
│   │   └── errorHandler.js
│   ├── server.js
│   ├── package.json
│   └── .env.example
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Register.jsx
│   │   │   ├── Login.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── TaskForm.jsx
│   │   │   ├── TaskList.jsx
│   │   │   ├── PrivateRoute.jsx
│   │   │   └── Navbar.jsx
│   │   ├── context/
│   │   │   └── AuthContext.jsx
│   │   ├── services/
│   │   │   └── api.js
│   │   ├── App.jsx
│   │   └── package.json
├── API_DOCUMENTATION.md
└── README.md
```

## Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- npm or yarn
- MongoDB (local or cloud)

### Backend Setup
1. Navigate to the backend directory:
   ```bash
   cd backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file based on `.env.example`:
   ```bash
   cp .env.example .env
   ```

4. Update `.env` with your MongoDB URI and JWT secret:
   ```
   MONGODB_URI=mongodb://localhost:27017/taskvault
   JWT_SECRET=your_jwt_secret_key
   PORT=5000
   ```

5. Start the server:
   ```bash
   npm run dev
   ```

### Frontend Setup
1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

## API Endpoints

### Authentication Routes (`/api/v1/auth`)
- `POST /register` - Register a new user
- `POST /login` - Login user and get JWT token

### Task Routes (`/api/v1/tasks`)
- `GET /` - Get all tasks (requires JWT)
- `POST /` - Create a new task (requires JWT)
- `GET /:id` - Get a specific task (requires JWT)
- `PUT /:id` - Update a task (requires JWT)
- `DELETE /:id` - Delete a task (requires JWT)

### Admin Routes (`/api/v1/admin`)
- `GET /users` - Get all users (admin only)
- `GET /stats` - Get system statistics (admin only)

## Security Features
- **JWT Authentication**: Secure token-based authentication
- **Password Hashing**: bcrypt for password encryption
- **Input Validation**: Comprehensive request validation
- **CORS**: Cross-Origin Resource Sharing enabled
- **Helmet**: Security headers middleware
- **Error Handling**: Safe error messages without sensitive data

## Scalability Considerations

### Current Architecture
- Modular folder structure for easy feature expansion
- Separation of concerns (controllers, models, routes, middleware)
- Centralized error handling

### Future Enhancements
1. **Microservices**: Split auth, tasks, and admin into separate services
2. **Caching**: Implement Redis for frequently accessed data
3. **Logging**: Add Winston or Morgan for comprehensive logging
4. **Rate Limiting**: Implement rate limiting to prevent abuse
5. **Docker**: Containerize the application for easy deployment
6. **Load Balancing**: Use nginx or load balancing services for scalability
7. **Database Replication**: Implement MongoDB replica sets
8. **Message Queue**: Use RabbitMQ or Kafka for async operations

## Testing

### Using Postman
1. Import the Postman collection from `API_DOCUMENTATION.md`
2. Test all endpoints with sample data
3. Verify JWT token handling
4. Test role-based access control

### Frontend Testing
1. Register a new user
2. Login with credentials
3. Create, edit, and delete tasks
4. Verify JWT authentication on protected routes
5. Test logout functionality

## Deployment

### Backend Deployment
- Deploy to Heroku, AWS, or any Node.js hosting
- Set environment variables in production
- Use MongoDB Atlas for cloud database

### Frontend Deployment
- Deploy to Vercel, Netlify, or GitHub Pages
- Update API endpoint for production

## Assignment Completion Checklist
- ✅ Backend with REST API and JWT authentication
- ✅ Role-based access control (user vs admin)
- ✅ CRUD operations for task management
- ✅ Database schema design
- ✅ Frontend UI with React
- ✅ API documentation
- ✅ Security best practices
- ✅ Error handling and validation
- ✅ Scalable project structure

## Author
Rushikesh - Backend Developer Intern Candidate
Dhule, Maharashtra, India

## License
MIT
