# E-Commerce REST API

A fully functional e-commerce backend REST API built with Node.js, Express, and MongoDB. Features role-based access control, shopping cart management, order processing with price snapshotting, and Stripe payment integration in test mode.

## Features

- User authentication with JWT (register, login)
- Role-based access control — admin vs customer
- Product management (admin only for create, update, delete)
- Shopping cart with full product details via Mongoose populate
- Order system with price snapshotting at checkout
- Automatic cart clearing after order is placed
- Stripe payment intent creation (test mode)
- Stripe webhook with signature verification
- Protected routes with two-layer middleware chaining

## Tech Stack

**Runtime:** Node.js  
**Framework:** Express.js  
**Database:** MongoDB with Mongoose  
**Authentication:** JWT, bcrypt  
**Payment:** Stripe API (test mode)  
**Tools:** Postman, MongoDB Compass, Stripe CLI

## Project Structure

    ecommerce-api/
    ├── models/
    │   ├── User.js       # User schema with role field (customer/admin)
    │   ├── Product.js    # Product schema
    │   ├── Cart.js       # Cart with product references
    │   └── Order.js      # Order with price snapshot
    ├── routes/
    │   ├── auth.js       # Register and login
    │   ├── products.js   # Product CRUD
    │   ├── cart.js       # Cart management
    │   ├── orders.js     # Order creation and history
    │   ├── payment.js    # Stripe payment intent
    │   └── webhook.js    # Stripe webhook handler
    ├── middleware/
    │   ├── auth.js       # JWT verification
    │   └── admin.js      # Role-based access check
    └── server.js

## API Endpoints

### Auth

| Method | Endpoint           | Access |
| ------ | ------------------ | ------ |
| POST   | /api/auth/register | Public |
| POST   | /api/auth/login    | Public |

### Products

| Method | Endpoint          | Access     |
| ------ | ----------------- | ---------- |
| GET    | /api/products     | Public     |
| GET    | /api/products/:id | Public     |
| POST   | /api/products     | Admin only |
| PUT    | /api/products/:id | Admin only |
| DELETE | /api/products/:id | Admin only |

### Cart

| Method | Endpoint                    | Access   |
| ------ | --------------------------- | -------- |
| GET    | /api/cart                   | Customer |
| POST   | /api/cart/add               | Customer |
| DELETE | /api/cart/remove/:productId | Customer |

### Orders

| Method | Endpoint    | Access   |
| ------ | ----------- | -------- |
| POST   | /api/orders | Customer |
| GET    | /api/orders | Customer |

### Payment

| Method | Endpoint                           | Access   |
| ------ | ---------------------------------- | -------- |
| POST   | /api/payment/create-payment-intent | Customer |
| POST   | /api/webhook                       | Stripe   |

## How to Run Locally

1. Clone the repository
2. Install dependencies:

```bash
npm install
```

3. Create a `.env` file:
   PORT=5001
   MONGO_URI=mongodb://localhost:27017/ecommerce
   JWT_SECRET=your_jwt_secret
   STRIPE_SECRET_KEY=your_stripe_secret_key
   STRIPE_WEBHOOK_SECRET=your_stripe_webhook_secret
4. Start the server:

```bash
node server.js
```

## Key Concepts Implemented

**Role-Based Access Control** — two middleware layers chained together: `auth.js` verifies the JWT token, `admin.js` checks the role. Admin-only routes require both to pass.

**Price Snapshotting** — order items store the product price at the time of purchase, not a reference to the current price. This ensures historical order accuracy even if product prices change later.

**Mongoose Populate** — cart and order responses return full product details instead of just IDs, using MongoDB references and Mongoose's populate feature.

**Stripe Webhook Signature Verification** — webhook route uses `express.raw()` instead of `express.json()` to preserve the raw request body needed for Stripe's cryptographic signature check.

## Author

Asanga Kavindi — [GitHub](https://github.com/AsangaGunawardhana)
