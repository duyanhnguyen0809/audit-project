# Security Audit Report

## Overview

This report provides a security audit of the Nutrition Website project, which includes both the `nutrition_app` and `api_data` directories. The audit focuses on identifying potential security vulnerabilities and providing recommendations for improvement.

## Project Structure

### `nutrition_app`

- **Repository URL:** [https://github.com/duongdk099/nutrition_app.git](https://github.com/duongdk099/nutrition_app.git)
- **Main Directories:**
  - `src/`: Contains the core application code.
  - `components/`: Reusable UI components.
  - `services/`: Utility services and data fetching.
  - `cypress/`: Testing-related files including e2e and component tests.

### `api_data`

- **Repository URL:** [https://github.com/duongdk099/api_data.git](https://github.com/duongdk099/api_data.git)
- **Main Directories:**
  - `controllers/`: Contains the logic for handling API requests.
  - `middlewares/`: Middleware functions for request processing.
  - `routes/`: Defines the API endpoints.

## Key Features

- User authentication with JWT-based authentication.
- Password hashing using bcrypt.
- CRUD operations for user management.
- Nutrition data display and management.
- Responsive design using Tailwind CSS.
- Cookie management using js-cookie.

## Core Problems

### 1. Environment Variables Exposure

**Issue:**
Environment variables such as `DATABASE_URL` and `JWT_SECRET` are stored in `.env` files, which are correctly ignored by `.gitignore`. However, the `.env.example` file contains sensitive information that should not be exposed.

**Recommendation:**
Ensure that `.env.example` only contains placeholder values and does not expose any real credentials.

### 2. JWT Token Handling

**Issue:**
The JWT token is stored in a cookie without the `httpOnly` flag, making it accessible to client-side scripts and vulnerable to XSS attacks.

**Recommendation:**
Set the `httpOnly` flag on the JWT cookie to prevent client-side access.

### 3. Password Hashing

**Issue:**
Passwords are hashed using bcrypt, which is a good practice. However, ensure that the bcrypt salt rounds are set to a sufficiently high value (e.g., 10 or more) to enhance security.

**Recommendation:**
Verify and set the bcrypt salt rounds to a secure value.

### 4. SQL Injection

**Issue:**
The application uses parameterized queries with the `neon` library, which helps prevent SQL injection attacks. Ensure that all database queries are parameterized.

**Recommendation:**
Review all database queries to confirm they are parameterized and do not concatenate user input directly into SQL statements.

### 5. CORS Configuration

**Issue:**
The CORS configuration in `api_data/app.js` allows all origins (`origin: '*'`), which can expose the API to unauthorized access.

**Recommendation:**
Restrict CORS to only allow requests from trusted domains.

### 6. Error Handling

**Issue:**
Error messages returned by the API may expose sensitive information.

**Recommendation:**
Ensure that error messages do not reveal sensitive information and provide generic error messages to the client.

### 7. Role-Based Access Control

**Issue:**
The application supports role-based access control but ensure that all sensitive endpoints are protected by appropriate role checks.

**Recommendation:**
Review and enforce role-based access control on all sensitive endpoints.

## Additional Problems

### 1. Insecure Dependencies

**Issue:**
The project dependencies may contain vulnerabilities.

**Recommendation:**
Regularly run dependency vulnerability scans using tools like `npm audit` and update dependencies to their latest secure versions.

### 2. Lack of Rate Limiting

**Issue:**
The API endpoints do not implement rate limiting, making them susceptible to brute force attacks.

**Recommendation:**
Implement rate limiting on API endpoints to prevent abuse.

### 3. Missing Security Headers

**Issue:**
The application lacks important security headers such as `Content-Security-Policy`, `X-Content-Type-Options`, and `X-Frame-Options`.

**Recommendation:**
Add security headers to the application to enhance security.

### 4. Insufficient Logging and Monitoring

**Issue:**
The application does not have sufficient logging and monitoring to detect and respond to security incidents.

**Recommendation:**
Implement comprehensive logging and monitoring to track security events and respond to incidents promptly.

## Summary

The Nutrition Website project follows several good security practices, such as using bcrypt for password hashing and parameterized queries to prevent SQL injection. However, there are areas for improvement, including better handling of environment variables, securing JWT tokens, refining CORS configurations, and addressing additional problems like insecure dependencies and lack of rate limiting.

By addressing the recommendations provided in this report, the security posture of the project can be significantly enhanced.

## References

- [OWASP Top Ten](https://owasp.org/www-project-top-ten/)
- [JWT Best Practices](https://auth0.com/blog/a-look-at-the-latest-draft-for-jwt-bcp/)
- [bcrypt Documentation](https://www.npmjs.com/package/bcrypt)
