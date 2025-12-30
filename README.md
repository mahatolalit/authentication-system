# Authentication System

A full-stack authentication system built with the MERN stack (MongoDB, Express.js, React, Node. js) featuring secure user registration, email verification, password reset functionality, and JWT-based session management.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Configuration](#configuration)
- [API Reference](#api-reference)
- [Frontend Routes](#frontend-routes)
- [Architecture](#architecture)
- [Security](#security)
- [License](#license)

## Overview

This authentication system provides a complete solution for user identity management in web applications. It implements industry-standard security practices including password hashing, JWT tokens stored in HTTP-only cookies, email verification, and secure password reset flows.

## Features

- **User Registration**: Create accounts with full name, email, and password
- **Email Verification**: 6-digit verification code sent via email with 24-hour expiration
- **User Login**: Secure authentication with credential validation
- **Password Reset**: Token-based password reset via email with 20-minute expiration
- **Session Management**: JWT-based authentication with 7-day token validity
- **Protected Routes**: Middleware-based route protection for authenticated endpoints
- **Password Strength Indicator**: Real-time password strength feedback during registration
- **Responsive UI**: Modern, animated interface with glassmorphism design

## Tech Stack

### Backend

| Technology | Purpose |
|------------|---------|
| Node.js | Runtime environment |
| Express.js 5.x | Web framework |
| MongoDB | Database |
| Mongoose | ODM for MongoDB |
| JSON Web Token | Authentication tokens |
| bcrypt | Password hashing |
| Mailtrap | Email delivery service |
| cookie-parser | Cookie handling |
| cors | Cross-origin resource sharing |
| dotenv | Environment configuration |

### Frontend

| Technology | Purpose |
|------------|---------|
| React 19.x | UI library |
| Vite | Build tool and dev server |
| React Router DOM 7.x | Client-side routing |
| Zustand | State management |
| Axios | HTTP client |
| Tailwind CSS | Styling |
| Framer Motion | Animations |
| Lucide React | Icons |
| React Hot Toast | Notifications |

## Project Structure

```
authentication-system/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   │   └── auth.controller.js    # Authentication logic
│   │   ├── db/
│   │   │   └── connectDB.js          # MongoDB connection
│   │   ├── mailtrap/
│   │   │   ├── emails.js             # Email sending functions
│   │   │   ├── emailTemplates.js     # HTML email templates
│   │   │   └── mailtrap.config.js    # Mailtrap configuration
│   │   ├── middleware/
│   │   │   └── protectRoute.js       # JWT verification middleware
│   │   ├── models/
│   │   │   └── User.js               # User schema
│   │   ├── routes/
│   │   │   └── auth.route.js         # API route definitions
│   │   ├── utils/
│   │   │   ├── generateTokenAndSetCookie.js
│   │   │   └── generateVerificationCode.js
│   │   └── server.js                 # Application entry point
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Input.jsx             # Reusable input component
│   │   │   ├── LoadingSpinner.jsx    # Loading indicator
│   │   │   └── PasswordStrengthMeter.jsx
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx         # Protected dashboard
│   │   │   ├── EmailVerificationPage.jsx
│   │   │   ├── ForgotPasswordPage. jsx
│   │   │   ├── LoginPage.jsx
│   │   │   ├── ResetPasswordPage.jsx
│   │   │   └── SignUpPage.jsx
│   │   ├── store/
│   │   │   └── authStore.js          # Zustand state management
│   │   ├── utils/
│   │   │   └── date.js               # Date formatting utilities
│   │   ├── ReactBits/
│   │   │   └── Aurora/               # Background animation
│   │   ├── App.jsx                   # Main application component
│   │   └── main.jsx                  # React entry point
│   ├── tailwind.config.js
│   ├── vite.config.js
│   └── package.json
└── package.json                      # Root package for deployment
```

## Installation

### Prerequisites

- Node.js (v18 or higher recommended)
- MongoDB instance (local or cloud-based)
- Mailtrap account for email services

### Steps

1. **Clone the repository**

   ```bash
   git clone https://github.com/mahatolalit/authentication-system.git
   cd authentication-system
   ```

2. **Install dependencies**

   ```bash
   # Install root dependencies
   npm install

   # Install backend dependencies
   cd backend
   npm install

   # Install frontend dependencies
   cd ../frontend
   npm install
   ```

3. **Configure environment variables** (see [Configuration](#configuration))

4. **Start the development servers**

   ```bash
   # Terminal 1 - Backend
   cd backend
   npm run dev

   # Terminal 2 - Frontend
   cd frontend
   npm run dev
   ```

### Production Build

```bash
# From root directory
npm run build
npm start
```

## Configuration

Create a `.env` file in the `backend` directory with the following variables: 

```env
# Server Configuration
PORT=5001
NODE_ENV=development

# Database
MONGO_URI=mongodb+srv://<username>:<password>@<cluster>. mongodb.net/<database>

# JWT Configuration
JWT_SECRET=your_secure_jwt_secret_key

# Client URL (for CORS and email links)
CLIENT_URL=http://localhost:5173

# Mailtrap Configuration
MAILTRAP_TOKEN=your_mailtrap_api_token
MAILTRAP_ENDPOINT=https://send.api.mailtrap. io/
EMAIL_FROM=noreply@yourdomain.com
EMAIL_FROM_NAME=Your App Name
WELCOME_EMAIL_TEMPLATE_UUID=your_welcome_template_uuid
```

## API Reference

All API endpoints are prefixed with `/api/auth`

### Authentication Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/signup` | Register a new user | No |
| POST | `/login` | Authenticate user | No |
| POST | `/logout` | End user session | No |
| POST | `/verify` | Verify email with code | No |
| POST | `/forgot-password` | Request password reset | No |
| POST | `/reset-password/: token` | Reset password with token | No |
| GET | `/check-auth` | Validate current session | Yes |

### Request/Response Examples

#### Sign Up

```http
POST /api/auth/signup
Content-Type: application/json

{
  "fullName": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

#### Login

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password":  "securePassword123"
}
```

#### Verify Email

```http
POST /api/auth/verify
Content-Type: application/json

{
  "code": "123456"
}
```

## Frontend Routes

| Path | Component | Description | Access |
|------|-----------|-------------|--------|
| `/` | Dashboard | User dashboard | Protected |
| `/signup` | SignUpPage | User registration | Public (redirects if authenticated) |
| `/login` | LoginPage | User login | Public (redirects if authenticated) |
| `/verify-email` | EmailVerificationPage | Email verification | Public |
| `/forgot-password` | ForgotPasswordPage | Password reset request | Public |
| `/reset-password/: token` | ResetPasswordPage | Password reset form | Public |

## Architecture

### Authentication Flow

1. **Registration Flow**
   - User submits registration form
   - Backend validates input and checks for existing accounts
   - Password is hashed using bcrypt (10 salt rounds)
   - 6-digit verification code is generated and stored
   - Verification email is sent via Mailtrap
   - JWT token is generated and set as HTTP-only cookie

2. **Email Verification Flow**
   - User receives 6-digit code via email
   - Code is submitted through the verification page
   - Backend validates code and expiration (24 hours)
   - User is marked as verified upon successful validation
   - Welcome email is sent

3. **Login Flow**
   - User submits credentials
   - Backend validates email exists and password matches
   - JWT token is generated and set as HTTP-only cookie
   - Last login timestamp is updated

4. **Password Reset Flow**
   - User requests password reset via email
   - 20-character hex token is generated
   - Reset link with token is sent via email (20-minute validity)
   - User submits new password with token
   - Password is updated and reset token is cleared

### User Model Schema

```javascript
{
  fullName: String (required),
  email: String (required, unique),
  password: String (required, hashed),
  lastLogin: Date,
  isVerified: Boolean (default: false),
  resetPasswordToken: String,
  resetPasswordExpiresAt: Date,
  verificationToken: String,
  verificationExpiresAt: Date,
  createdAt:  Date (auto),
  updatedAt: Date (auto)
}
```

### State Management

The frontend uses Zustand for global state management with the following state structure:

```javascript
{
  user: Object | null,
  isAuthenticated: Boolean,
  isLoading: Boolean,
  isCheckingAuth: Boolean,
  error: String | null,
  message: String | null
}
```

## Security

This application implements several security best practices:

- **Password Hashing**: All passwords are hashed using bcrypt with 10 salt rounds
- **HTTP-Only Cookies**:  JWT tokens are stored in HTTP-only cookies to prevent XSS attacks
- **Secure Cookies**:  Cookies are marked as secure in production environments
- **SameSite Cookies**: Strict SameSite policy to prevent CSRF attacks
- **Token Expiration**: JWTs expire after 7 days; reset tokens expire after 20 minutes
- **CORS Configuration**:  Restricted cross-origin requests to specified client URL
- **Input Validation**: Server-side validation of all user inputs
- **Password Exclusion**:  Passwords are never returned in API responses

## License

ISC
