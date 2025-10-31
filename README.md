# Middleware Implementation for Logging and Bearer Token Authentication

## Overview

This Express.js application demonstrates the implementation of custom middleware functions for request logging and Bearer token-based authentication. Learn how to build secure, professional-grade Node.js backend services with proper middleware flow and request validation.

## 📋 Objective

Understand how to:
- Build and integrate custom middleware functions in Express.js
- Implement global request logging with HTTP method, URL, and timestamp
- Protect routes using Bearer token authentication
- Enforce secure access to specific endpoints
- Handle authentication errors gracefully

## 🎯 Task Description

This project creates an Express.js server with:

1. **Global Logging Middleware**: Logs every incoming request with:
   - HTTP method (GET, POST, etc.)
   - Request URL
   - ISO timestamp

2. **Bearer Token Authentication Middleware**: Protects routes by:
   - Checking for Authorization header
   - Validating Bearer token format
   - Comparing token against `mysecrettoken`
   - Denying access with appropriate error messages

3. **Routes**:
   - `/public` - Public route (no authentication required)
   - `/protected` - Protected route (requires Bearer token)

## 🚀 Setup Instructions

### Prerequisites
- Node.js (v14 or higher)
- npm or yarn
- Git
- Postman or curl (for testing)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/amanhazari10/express-logging-bearer-middleware.git
cd express-logging-bearer-middleware
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file (optional):
```bash
PORT=3000
TOKEN=mysecrettoken
```

4. Start the server:
```bash
npm start
```

The server will run on `http://localhost:3000`

## 📖 Usage Guide

### File Structure
```
.
├── server.js              # Main Express application
├── package.json           # Project dependencies
├── .env                   # Environment variables (optional)
├── .gitignore             # Git ignore rules
└── README.md              # This file
```

### How Middleware Works

#### 1. Logging Middleware
This middleware runs on every request and logs:
```javascript
app.use((req, res, next) => {
  const timestamp = new Date().toISOString();
  console.log(`[${timestamp}] ${req.method} ${req.url}`);
  next();
});
```

#### 2. Bearer Token Middleware
This middleware protects specific routes:
```javascript
const authMiddleware = (req, res, next) => {
  const authHeader = req.headers.authorization;
  
  if (!authHeader) {
    return res.status(401).json({ error: 'Missing Authorization header' });
  }
  
  const token = authHeader.split(' ')[1];
  
  if (token !== 'mysecrettoken') {
    return res.status(403).json({ error: 'Invalid token' });
  }
  
  next();
};
```

## 📡 API Documentation

### Public Route

**GET /public**
- **Description**: Accessible without authentication
- **Auth Required**: No
- **Response**: Welcome message

**Example Request:**
```bash
curl http://localhost:3000/public
```

**Example Response:**
```json
{
  "message": "This is a public route"
}
```

### Protected Route

**GET /protected**
- **Description**: Requires Bearer token authentication
- **Auth Required**: Yes (Bearer token: `mysecrettoken`)
- **Response**: Success message with timestamp

**Example Request with Valid Token:**
```bash
curl -H "Authorization: Bearer mysecrettoken" http://localhost:3000/protected
```

**Example Response:**
```json
{
  "message": "Access granted to protected route",
  "timestamp": "2025-10-31T20:30:00.000Z"
}
```

**Example Request with Invalid Token:**
```bash
curl -H "Authorization: Bearer wrongtoken" http://localhost:3000/protected
```

**Example Response:**
```json
{
  "error": "Invalid token"
}
```

**Example Request without Token:**
```bash
curl http://localhost:3000/protected
```

**Example Response:**
```json
{
  "error": "Missing Authorization header"
}
```

## 🧪 Testing with curl

### Test 1: Access Public Route (No Auth)
```bash
curl -v http://localhost:3000/public
```
Expected Status: 200

### Test 2: Access Protected Route without Token
```bash
curl -v http://localhost:3000/protected
```
Expected Status: 401
Expected Response: `{"error": "Missing Authorization header"}`

### Test 3: Access Protected Route with Wrong Token
```bash
curl -v -H "Authorization: Bearer wrongtoken" http://localhost:3000/protected
```
Expected Status: 403
Expected Response: `{"error": "Invalid token"}`

### Test 4: Access Protected Route with Correct Token
```bash
curl -v -H "Authorization: Bearer mysecrettoken" http://localhost:3000/protected
```
Expected Status: 200
Expected Response: `{"message": "Access granted to protected route", "timestamp": "..."}`

## 🔧 Testing with Postman

### Setup Instructions:

1. **Open Postman** and create a new request

2. **For Public Route Test:**
   - Method: GET
   - URL: `http://localhost:3000/public`
   - Headers: (None required)
   - Click Send

3. **For Protected Route Test (without token):**
   - Method: GET
   - URL: `http://localhost:3000/protected`
   - Headers: (None)
   - Click Send
   - Expected: 401 Unauthorized

4. **For Protected Route Test (with valid token):**
   - Method: GET
   - URL: `http://localhost:3000/protected`
   - Headers:
     - Key: `Authorization`
     - Value: `Bearer mysecrettoken`
   - Click Send
   - Expected: 200 OK

5. **For Protected Route Test (with invalid token):**
   - Method: GET
   - URL: `http://localhost:3000/protected`
   - Headers:
     - Key: `Authorization`
     - Value: `Bearer wrongtoken`
   - Click Send
   - Expected: 403 Forbidden

## 🔐 Security Notes

- **Never hardcode tokens in production**: Use environment variables
- **Use HTTPS in production**: Ensure Bearer tokens are transmitted over secure channels
- **Implement rate limiting**: Prevent brute force attacks
- **Use JWT**: Consider replacing simple token validation with JWT for production
- **Add logging**: Log all authentication attempts for security auditing

## 📚 Key Concepts

### Middleware Flow
Every request flows through middleware in order:
1. Logging middleware runs first
2. Route handler or auth middleware runs next
3. Response is sent back to client

### HTTP Status Codes
- `200 OK`: Request succeeded
- `401 Unauthorized`: Missing or invalid credentials
- `403 Forbidden`: Valid credentials but insufficient permissions
- `500 Internal Server Error`: Server error

## 🎓 Learning Outcomes

After completing this project, you'll understand:
- ✅ How to create custom middleware functions
- ✅ Middleware execution order and flow
- ✅ How to validate HTTP headers
- ✅ Bearer token authentication pattern
- ✅ Error handling in Express.js
- ✅ HTTP status codes and their meanings

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest improvements
- Submit pull requests

## 📄 License

MIT License - feel free to use this project for learning purposes.

## 👤 Author

**Aman Hazari**
- GitHub: [@amanhazari10](https://github.com/amanhazari10)

## 📞 Support

If you have questions or need help:
1. Check the troubleshooting section
2. Review the code comments
3. Open an issue on GitHub

---

**Happy Learning! 🎉**
