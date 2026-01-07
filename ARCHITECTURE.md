# TaskVault - System Architecture & Design

## Architecture Overview

TaskVault follows a modular, scalable architecture designed for easy feature expansion and maintenance. The system is built using Node.js/Express backend with MongoDB for data persistence and React frontend for user interactions.

## Backend Architecture

### Folder Structure
```
backend/
├── server.js              # Main application entry point
├── package.json          # Dependencies and scripts
├── .env.example          # Environment variables template
├── config/
│   └── database.js      # MongoDB connection setup
├── models/
│   ├── User.js          # User schema and validation
│   └── Task.js          # Task schema and validation
├── controllers/
│   ├── authController.js # Auth business logic
│   └── taskController.js # Task CRUD logic
├── routes/
│   ├── authRoutes.js    # Auth endpoints
│   ├── taskRoutes.js    # Task endpoints
│   └── adminRoutes.js   # Admin endpoints
└── middleware/
    ├── authMiddleware.js  # JWT validation
    └── errorHandler.js    # Centralized error handling
```

### Design Patterns Used

1. **MVC Pattern**: Separation of Model, View (implicit), and Controller logic
2. **Middleware Pattern**: Request/response pipeline using middleware functions
3. **Repository Pattern**: Models act as data repositories
4. **Dependency Injection**: Modules require their dependencies

## API Design

### REST Principles
- Clear resource-based URLs (`/api/v1/tasks`, `/api/v1/auth`)
- HTTP methods: GET (retrieve), POST (create), PUT (update), DELETE (delete)
- Proper HTTP status codes (200, 201, 400, 401, 403, 404, 500)
- Consistent JSON response format

### API Versioning
- All endpoints prefixed with `/api/v1/`
- Allows future versions (`/api/v2/`) without breaking existing clients

## Security Architecture

### Authentication & Authorization
1. **JWT (JSON Web Tokens)**
   - Tokens issued on successful login
   - Tokens contain user ID and role
   - Tokens verified on protected routes

2. **Password Security**
   - Bcrypt hashing with salt (10 rounds)
   - Passwords not returned in API responses
   - Minimum 6 character requirement

3. **Role-Based Access Control (RBAC)**
   - Two roles: `user` and `admin`
   - Admin routes protected with `adminOnly` middleware
   - User routes protected with `authMiddleware`

### Additional Security Measures
- **Helmet**: HTTP headers hardening
- **CORS**: Cross-origin resource sharing configured
- **Input Validation**: Using express-validator
- **Error Handling**: Safe error messages (no sensitive data exposure)

## Database Design

### Collections

#### Users Collection
```javascript
{
  _id: ObjectId,
  username: String (unique, min 3)
  email: String (unique)
  password: String (hashed)
  role: String (enum: ['user', 'admin'])
  createdAt: Date
}
```

#### Tasks Collection
```javascript
{
  _id: ObjectId,
  title: String (required)
  description: String
  status: String (enum: ['pending', 'in-progress', 'completed'])
  priority: String (enum: ['low', 'medium', 'high'])
  userId: ObjectId (ref: User)
  dueDate: Date
  createdAt: Date
  updatedAt: Date
}
```

### Indexes
- `Users.email`: Unique index for email lookups
- `Users.username`: Unique index for username lookups
- `Tasks.userId`: Index for user task queries
- `Tasks.createdAt`: Index for sorting

## Scalability Considerations

### Current System Limitations
1. Single-instance Node.js server
2. Direct MongoDB connection (no connection pooling optimization)
3. No caching layer
4. Synchronous middleware processing

### Scaling Strategies

#### 1. Horizontal Scaling
- **Load Balancing**: Deploy multiple instances behind nginx/HAProxy
- **Session Management**: Use distributed cache (Redis) for sessions
- **Database Replication**: MongoDB replica sets for high availability

#### 2. Vertical Scaling
- **Optimize**: Code optimization, database query optimization
- **Resources**: Increase server CPU, RAM
- **CDN**: Serve static assets from CDN

#### 3. Microservices Architecture
```
┌─────────────┐
│   API GW    │
└──────┬──────┘
       │
   ┌───┼───┬────────┐
   │   │   │        │
┌──▼──┐│ ┌──▼──┐ ┌──▼──┐
│Auth ││ │Task │ │Admin │
│Svc  ││ │Svc  │ │Svc   │
└──┬──┘│ └──┬──┘ └──┬───┘
   │   │    │      │
   └───┼────┼──────┘
       │    │
   ┌───▼────▼────┐
   │   Cache     │
   │  (Redis)    │
   └─────────────┘
```

#### 4. Caching Strategy
- **API Response Caching**: Cache task lists by user (Redis)
- **Database Query Caching**: Cache frequently accessed data
- **Token Caching**: Maintain token blacklist for logout

#### 5. Asynchronous Processing
- **Message Queue**: Use RabbitMQ or Kafka for async tasks
- **Email Notifications**: Queue task reminders
- **Logging**: Async logging to prevent blocking

#### 6. Monitoring & Observability
- **Logging**: Winston/Morgan for detailed logs
- **Metrics**: Prometheus for performance metrics
- **Tracing**: Jaeger for distributed tracing
- **Alerts**: Alert on errors, performance degradation

## Deployment Architecture

### Development
- Local Node.js server on port 5000
- Local MongoDB instance

### Production
```
┌──────────────┐
│  CDN/WAF     │ (CloudFlare/AWS WAF)
└──────┬───────┘
       │
┌──────▼───────┐
│ Load Balancer│ (nginx/ALB)
└──────┬───────┘
       │
   ┌───┼───┬───┐
   │   │   │   │
┌──▼──┬──▼──┬──▼──┐
│ App │ App │ App │ (Kubernetes pods)
│ 1   │ 2   │ 3   │
└──┬──┴──┬──┴──┬──┘
   │     │     │
   └─────┼─────┘
         │
  ┌──────▼───────┐
  │  MongoDB     │
  │  Cluster     │
  └──────────────┘
```

### Deployment Platforms
- **Heroku**: Simple deployment with automatic scaling
- **AWS EC2**: More control, auto-scaling groups
- **Google Cloud Run**: Serverless functions
- **Kubernetes**: Full orchestration for large deployments

## Performance Optimization

1. **Database Optimization**
   - Proper indexing
   - Query optimization
   - Connection pooling

2. **API Optimization**
   - Pagination for large datasets
   - Compression (gzip)
   - Rate limiting

3. **Code Optimization**
   - Async/await patterns
   - Connection pooling
   - Efficient middleware

## Future Enhancements

1. WebSocket support for real-time updates
2. GraphQL API alongside REST
3. Machine learning for task prioritization
4. File upload support for task attachments
5. Team collaboration features
6. Mobile app (native iOS/Android)

## References

- [REST API Best Practices](https://restfulapi.net/)
- [MongoDB Best Practices](https://docs.mongodb.com/manual/administration/)
- [Node.js Performance](https://nodejs.org/en/docs/guides/nodejs-performance-monitoring/)
- [Microservices Architecture](https://microservices.io/)
