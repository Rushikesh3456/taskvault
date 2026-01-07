# TaskVault API Documentation

## Base URL
```
http://localhost:5000/api/v1
```

## Authentication
All endpoints (except register and login) require a JWT token in the Authorization header:
```
Authorization: Bearer <token>
```

---

## Authentication Endpoints

### 1. Register User
- **Endpoint**: `POST /auth/register`
- **Description**: Register a new user
- **Request Body**:
  ```json
  {
    "username": "johndoe",
    "email": "john@example.com",
    "password": "password123"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "message": "User registered successfully",
    "user": {
      "id": "507f1f77bcf86cd799439011",
      "username": "johndoe",
      "email": "john@example.com",
      "role": "user"
    }
  }
  ```
- **Error Responses**:
  - 400: Invalid input (missing fields, invalid email format)
  - 409: User already exists

### 2. Login User
- **Endpoint**: `POST /auth/login`
- **Description**: Authenticate user and get JWT token
- **Request Body**:
  ```json
  {
    "email": "john@example.com",
    "password": "password123"
  }
  ```
- **Response** (200 OK):
  ```json
  {
    "message": "Login successful",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "507f1f77bcf86cd799439011",
      "username": "johndoe",
      "email": "john@example.com",
      "role": "user"
    }
  }
  ```
- **Error Responses**:
  - 401: Invalid credentials
  - 404: User not found

---

## Task Endpoints

### 3. Get All Tasks
- **Endpoint**: `GET /tasks`
- **Description**: Get all tasks for the authenticated user
- **Auth**: Required
- **Query Parameters**:
  - `status`: Filter by status (pending, in-progress, completed)
  - `priority`: Filter by priority (low, medium, high)
  - `page`: Page number (default: 1)
  - `limit`: Items per page (default: 10)
- **Response** (200 OK):
  ```json
  {
    "message": "Tasks retrieved successfully",
    "tasks": [
      {
        "_id": "507f1f77bcf86cd799439012",
        "title": "Complete project",
        "description": "Finish the backend API",
        "status": "in-progress",
        "priority": "high",
        "userId": "507f1f77bcf86cd799439011",
        "dueDate": "2025-01-15T00:00:00.000Z",
        "createdAt": "2025-01-07T10:00:00.000Z",
        "updatedAt": "2025-01-07T10:00:00.000Z"
      }
    ]
  }
  ```

### 4. Create Task
- **Endpoint**: `POST /tasks`
- **Description**: Create a new task
- **Auth**: Required
- **Request Body**:
  ```json
  {
    "title": "Complete project",
    "description": "Finish the backend API",
    "priority": "high",
    "dueDate": "2025-01-15T00:00:00.000Z"
  }
  ```
- **Response** (201 Created):
  ```json
  {
    "message": "Task created successfully",
    "task": {
      "_id": "507f1f77bcf86cd799439012",
      "title": "Complete project",
      "description": "Finish the backend API",
      "status": "pending",
      "priority": "high",
      "userId": "507f1f77bcf86cd799439011",
      "dueDate": "2025-01-15T00:00:00.000Z",
      "createdAt": "2025-01-07T10:00:00.000Z"
    }
  }
  ```

### 5. Get Task by ID
- **Endpoint**: `GET /tasks/:id`
- **Description**: Get a specific task
- **Auth**: Required
- **Response** (200 OK): Same as task object above
- **Error Responses**:
  - 404: Task not found

### 6. Update Task
- **Endpoint**: `PUT /tasks/:id`
- **Description**: Update an existing task
- **Auth**: Required
- **Request Body** (all fields optional):
  ```json
  {
    "title": "Updated title",
    "description": "Updated description",
    "status": "completed",
    "priority": "low",
    "dueDate": "2025-01-20T00:00:00.000Z"
  }
  ```
- **Response** (200 OK): Updated task object
- **Error Responses**:
  - 404: Task not found
  - 403: Unauthorized (not the task owner)

### 7. Delete Task
- **Endpoint**: `DELETE /tasks/:id`
- **Description**: Delete a task
- **Auth**: Required
- **Response** (200 OK):
  ```json
  {
    "message": "Task deleted successfully"
  }
  ```
- **Error Responses**:
  - 404: Task not found
  - 403: Unauthorized (not the task owner)

---

## Admin Endpoints

### 8. Get All Users (Admin Only)
- **Endpoint**: `GET /admin/users`
- **Description**: Get list of all users
- **Auth**: Required (Admin role)
- **Response** (200 OK):
  ```json
  {
    "message": "Users retrieved successfully",
    "users": [
      {
        "_id": "507f1f77bcf86cd799439011",
        "username": "johndoe",
        "email": "john@example.com",
        "role": "user",
        "createdAt": "2025-01-07T10:00:00.000Z"
      }
    ]
  }
  ```
- **Error Responses**:
  - 403: Admin access required

### 9. Get System Statistics (Admin Only)
- **Endpoint**: `GET /admin/stats`
- **Description**: Get system statistics
- **Auth**: Required (Admin role)
- **Response** (200 OK):
  ```json
  {
    "message": "Statistics retrieved successfully",
    "stats": {
      "totalUsers": 5,
      "totalTasks": 25,
      "tasksCompleted": 10,
      "tasksInProgress": 8,
      "tasksPending": 7
    }
  }
  ```

---

## HTTP Status Codes

- **200 OK**: Request successful
- **201 Created**: Resource created successfully
- **400 Bad Request**: Invalid request data
- **401 Unauthorized**: Missing or invalid authentication token
- **403 Forbidden**: Insufficient permissions
- **404 Not Found**: Resource not found
- **409 Conflict**: Resource already exists
- **500 Internal Server Error**: Server error

---

## Error Response Format

All error responses follow this format:
```json
{
  "message": "Error description",
  "error": "Error details (if available)"
}
```

---

## Postman Collection

Import this collection into Postman to test all endpoints:

```json
{
  "info": {
    "name": "TaskVault API",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "Register",
      "request": {
        "method": "POST",
        "url": "{{base_url}}/auth/register",
        "body": {"username": "testuser", "email": "test@example.com", "password": "password123"}
      }
    },
    {
      "name": "Login",
      "request": {
        "method": "POST",
        "url": "{{base_url}}/auth/login",
        "body": {"email": "test@example.com", "password": "password123"}
      }
    },
    {
      "name": "Get All Tasks",
      "request": {
        "method": "GET",
        "url": "{{base_url}}/tasks",
        "header": {"Authorization": "Bearer {{token}}"}
      }
    },
    {
      "name": "Create Task",
      "request": {
        "method": "POST",
        "url": "{{base_url}}/tasks",
        "header": {"Authorization": "Bearer {{token}}"},
        "body": {"title": "New Task", "description": "Task description", "priority": "high"}
      }
    }
  ]
}
```

---

## Rate Limiting

Current implementation does not have rate limiting. In production, implement:
- 100 requests per 15 minutes per IP
- 1000 requests per hour per authenticated user

---

## Pagination

For endpoints returning multiple items, use pagination:
- Default page size: 10 items
- Maximum page size: 100 items

```
GET /tasks?page=2&limit=20
```

Response includes pagination metadata:
```json
{
  "tasks": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 45,
    "pages": 3
  }
}
```
