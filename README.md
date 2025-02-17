# QKart Backend Documentation

## Overview
QKart is an e-commerce backend application developed using Node.js, Express, and MongoDB. It offers APIs for user authentication, product management, cart operations, and checkout processes.

## Tech Stack
- **Node.js**
- **Express.js**
- **MongoDB** with Mongoose
- **Passport.js** for JWT-based authentication
- **bcrypt** for password hashing

## Base URL
`/verse`

## Authentication
The application implements **JWT (JSON Web Token) authentication**. Protected routes require a **Bearer token** to be included in the Authorization header.

## API Routes

### User Authentication
#### 1. Register a New User
**Endpoint:** `POST /verse/auth/register`

**Request Body:**
```json
{
    "name": "string",
    "email": "string",
    "password": "string"
}
```

**Response:**
```json
{
    "user": {
        "name": "string",
        "email": "string",
        "walletMoney": 500,
        "address": "ADDRESS_NOT_SET"
    },
    "token": {
        "access": {
            "token": "jwt_token",
            "expires": "timestamp"
        }
    }
}
```

#### 2. User Login
**Endpoint:** `POST /verse/auth/login`

**Request Body:**
```json
{
    "email": "string",
    "password": "string"
}
```

**Response:** *(Same as register response)*

### User Management
#### 1. Retrieve User Details
**Endpoint:** `GET /verse/users/:userId`

**Authentication:** Required

**Response:** User object

#### 2. Update User Address
**Endpoint:** `PUT /verse/users/:userId`

**Authentication:** Required

**Request Body:**
```json
{
    "address": "string"
}
```

**Response:**
```json
{
    "address": "string"
}
```

### Product Management
#### 1. Retrieve All Products
**Endpoint:** `GET /verse/products`

**Authentication:** Not Required

**Response:** List of product objects

#### 2. Retrieve Product by ID
**Endpoint:** `GET /verse/products/:productId`

**Authentication:** Not Required

**Response:** Product object

### Cart Operations
#### 1. Retrieve Cart Details
**Endpoint:** `GET /verse/cart`

**Authentication:** Required

**Response:** Cart object with items

#### 2. Add Item to Cart
**Endpoint:** `POST /verse/cart`

**Authentication:** Required

**Request Body:**
```json
{
    "productId": "string",
    "quantity": number
}
```

**Response:** Updated cart object

#### 3. Update Item in Cart
**Endpoint:** `PUT /verse/cart`

**Authentication:** Required

**Request Body:**
```json
{
    "productId": "string",
    "quantity": number
}
```

**Response:** Updated cart object or `204 No Content` if product is removed (quantity = 0)

#### 4. Checkout
**Endpoint:** `PUT /verse/cart/checkout`

**Authentication:** Required

**Response:** `204 No Content` upon successful checkout

## Data Models

### User Model
- **name** (String, required)
- **email** (String, required, unique)
- **password** (String, required, min length: 8)
- **walletMoney** (Number, default: 500)
- **address** (String, default: "ADDRESS_NOT_SET")

### Product Model
- **name** (String, required)
- **category** (String, required)
- **cost** (Number, required)
- **rating** (Number, required)
- **image** (String, required)

### Cart Model
- **email** (String, required)
- **cartItems** (Array containing {product, quantity})
- **paymentOption** (String)

## Error Handling
QKart employs a centralized error-handling mechanism, ensuring consistent error responses in the following format:

```json
{
  "success": false,
  "status": "fail" | "error",
  "message": "Error description"
  // Stack trace included in development mode
}
```

### Common HTTP Status Codes:
- **400 (BAD_REQUEST)** – Invalid input/parameters
- **401 (UNAUTHORIZED)** – Authentication failure
- **403 (FORBIDDEN)** – Authorization failure
- **404 (NOT_FOUND)** – Resource not found
- **409 (CONFLICT)** – Duplicate resource (e.g., existing email)
- **500 (INTERNAL_SERVER_ERROR)** – Server error

#### Example Error Response:
```json
{
  "success": false,
  "status": "fail",
  "message": "Product already in the cart"
}
```

## Environment Variables
The following environment variables must be set in a `.env` file:
- **PORT**
- **MONGODB_URL**
- **JWT_SECRET**
- **JWT_ACCESS_EXPIRATION_MINUTES**

## Default Configuration
- **DEFAULT_WALLET_MONEY** = 500
- **DEFAULT_ADDRESS** = "ADDRESS_NOT_SET"
- **DEFAULT_PAYMENT_OPTION** = "PAYMENT_OPTION_DEFAULT"

This documentation provides an overview of the core functionalities of the QKart backend. For in-depth implementation details, refer to the respective service and controller files in the codebase.

