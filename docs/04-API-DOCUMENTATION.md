# API Documentation - Restaurant SaaS Platform

## Base URL
```
Development: http://localhost:3000/api/v1
Production: https://api.ordersaas.com/api/v1
```

## Table of Contents
1. [Authentication](#authentication)
2. [Tenant Management (Super Admin)](#tenant-management)
3. [Restaurant Management](#restaurant-management)
4. [Branch Management](#branch-management)
5. [Menu Management](#menu-management)
6. [Order Management](#order-management)
7. [Customer Management](#customer-management)
8. [Payment Management](#payment-management)
9. [Coupon Management](#coupon-management)
10. [Reports & Analytics](#reports--analytics)
11. [Webhooks](#webhooks)

---

## Authentication

### Register Restaurant (Tenant Signup)

Creates a new tenant and restaurant admin account.

**Endpoint**: `POST /auth/register`

**Request Body**:
```json
{
  "tenantName": "Pizza Palace",
  "slug": "pizza-palace",
  "restaurantName": "Pizza Palace Downtown",
  "email": "owner@pizzapalace.com",
  "password": "SecurePassword123!",
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+1234567890",
  "currency": "USD",
  "locale": "en",
  "timezone": "America/New_York"
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "tenant": {
      "id": "uuid",
      "name": "Pizza Palace",
      "slug": "pizza-palace",
      "status": "trial",
      "trialEndsAt": "2024-02-01T00:00:00Z"
    },
    "restaurant": {
      "id": "uuid",
      "name": "Pizza Palace Downtown",
      "slug": "pizza-palace-downtown"
    },
    "user": {
      "id": "uuid",
      "email": "owner@pizzapalace.com",
      "firstName": "John",
      "lastName": "Doe",
      "role": "RESTAURANT_ADMIN"
    },
    "tokens": {
      "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "refreshToken": "refresh_token_here",
      "expiresIn": 900
    }
  }
}
```

---

### Login

**Endpoint**: `POST /auth/login`

**Request Body**:
```json
{
  "email": "owner@pizzapalace.com",
  "password": "SecurePassword123!"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "uuid",
      "email": "owner@pizzapalace.com",
      "firstName": "John",
      "lastName": "Doe",
      "role": "RESTAURANT_ADMIN",
      "tenantId": "uuid"
    },
    "tokens": {
      "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
      "refreshToken": "refresh_token_here",
      "expiresIn": 900
    }
  }
}
```

---

### Refresh Token

**Endpoint**: `POST /auth/refresh`

**Request Body**:
```json
{
  "refreshToken": "refresh_token_here"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "accessToken": "new_access_token",
    "expiresIn": 900
  }
}
```

---

### Logout

**Endpoint**: `POST /auth/logout`

**Headers**:
```
Authorization: Bearer <access_token>
```

**Request Body**:
```json
{
  "refreshToken": "refresh_token_here"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "message": "Logged out successfully"
}
```

---

### Verify Email

**Endpoint**: `POST /auth/verify-email`

**Request Body**:
```json
{
  "token": "email_verification_token"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "message": "Email verified successfully"
}
```

---

### Forgot Password

**Endpoint**: `POST /auth/forgot-password`

**Request Body**:
```json
{
  "email": "owner@pizzapalace.com"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "message": "Password reset link sent to email"
}
```

---

### Reset Password

**Endpoint**: `POST /auth/reset-password`

**Request Body**:
```json
{
  "token": "reset_token",
  "password": "NewSecurePassword123!"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

---

## Tenant Management (Super Admin)

### List All Tenants

**Endpoint**: `GET /tenants`

**Headers**:
```
Authorization: Bearer <super_admin_token>
```

**Query Parameters**:
- `page` (default: 1)
- `limit` (default: 20)
- `status` (optional: active, trial, suspended, cancelled)
- `search` (optional: search by name/slug)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "tenants": [
      {
        "id": "uuid",
        "name": "Pizza Palace",
        "slug": "pizza-palace",
        "status": "active",
        "trialEndsAt": null,
        "createdAt": "2024-01-01T00:00:00Z",
        "subscription": {
          "plan": "professional",
          "status": "active"
        },
        "stats": {
          "restaurantCount": 1,
          "branchCount": 3,
          "monthlyOrders": 1250
        }
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "pages": 8
    }
  }
}
```

---

### Get Tenant Details

**Endpoint**: `GET /tenants/:tenantId`

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Pizza Palace",
    "slug": "pizza-palace",
    "status": "active",
    "settings": {},
    "subscription": {
      "id": "uuid",
      "plan": "professional",
      "status": "active",
      "currentPeriodStart": "2024-01-01T00:00:00Z",
      "currentPeriodEnd": "2024-02-01T00:00:00Z"
    },
    "restaurants": [
      {
        "id": "uuid",
        "name": "Pizza Palace Downtown"
      }
    ],
    "usage": {
      "orders": 1250,
      "revenue": 45000.00,
      "activeUsers": 5
    }
  }
}
```

---

### Update Tenant Status

**Endpoint**: `PATCH /tenants/:tenantId/status`

**Request Body**:
```json
{
  "status": "suspended",
  "reason": "Payment failure"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "status": "suspended"
  }
}
```

---

## Restaurant Management

### Create Restaurant

**Endpoint**: `POST /restaurants`

**Headers**:
```
Authorization: Bearer <access_token>
X-Tenant-ID: <tenant_id>
```

**Request Body**:
```json
{
  "name": "Pizza Palace West",
  "slug": "pizza-palace-west",
  "description": "Our west side location",
  "email": "west@pizzapalace.com",
  "phone": "+1234567891",
  "website": "https://pizzapalace.com",
  "currency": "USD",
  "locale": "en",
  "timezone": "America/New_York"
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "tenantId": "uuid",
    "name": "Pizza Palace West",
    "slug": "pizza-palace-west",
    "status": "active",
    "createdAt": "2024-01-15T00:00:00Z"
  }
}
```

---

### Get Restaurant Details

**Endpoint**: `GET /restaurants/:restaurantId`

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Pizza Palace Downtown",
    "slug": "pizza-palace-downtown",
    "description": "Best pizza in downtown",
    "logoUrl": "https://cdn.example.com/logo.png",
    "coverImageUrl": "https://cdn.example.com/cover.jpg",
    "email": "info@pizzapalace.com",
    "phone": "+1234567890",
    "currency": "USD",
    "locale": "en",
    "status": "active",
    "branches": [
      {
        "id": "uuid",
        "name": "Main Street Branch",
        "city": "New York"
      }
    ],
    "settings": {
      "taxRate": 8.5,
      "serviceChargeRate": 0,
      "autoAcceptOrders": false
    }
  }
}
```

---

### Update Restaurant

**Endpoint**: `PATCH /restaurants/:restaurantId`

**Request Body**:
```json
{
  "name": "Pizza Palace Downtown (Updated)",
  "description": "The best pizza in town!",
  "logoUrl": "https://cdn.example.com/new-logo.png"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Pizza Palace Downtown (Updated)",
    "updatedAt": "2024-01-20T00:00:00Z"
  }
}
```

---

### Update Restaurant Settings

**Endpoint**: `PATCH /restaurants/:restaurantId/settings`

**Request Body**:
```json
{
  "taxRate": 9.0,
  "serviceChargeRate": 2.5,
  "autoAcceptOrders": true,
  "primaryColor": "#FF6B6B",
  "secondaryColor": "#4ECDC4",
  "emailOnNewOrder": true,
  "notificationEmails": ["manager@pizzapalace.com", "owner@pizzapalace.com"]
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "taxRate": 9.0,
    "autoAcceptOrders": true,
    "primaryColor": "#FF6B6B"
  }
}
```

---

## Branch Management

### List Branches

**Endpoint**: `GET /restaurants/:restaurantId/branches`

**Query Parameters**:
- `isActive` (optional: true/false)
- `acceptsOrders` (optional: true/false)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "branches": [
      {
        "id": "uuid",
        "name": "Main Street Branch",
        "slug": "main-street",
        "address": "123 Main St, New York, NY 10001",
        "latitude": 40.7128,
        "longitude": -74.0060,
        "phone": "+1234567890",
        "isActive": true,
        "acceptsOrders": true,
        "deliveryEnabled": true,
        "pickupEnabled": true,
        "dineInEnabled": false,
        "minOrderAmount": 15.00,
        "deliveryFee": 3.99,
        "deliveryTimeMinutes": 30
      }
    ]
  }
}
```

---

### Create Branch

**Endpoint**: `POST /restaurants/:restaurantId/branches`

**Request Body**:
```json
{
  "name": "Broadway Branch",
  "slug": "broadway",
  "address": "456 Broadway, New York, NY 10013",
  "city": "New York",
  "state": "NY",
  "postalCode": "10013",
  "country": "USA",
  "latitude": 40.7180,
  "longitude": -74.0020,
  "phone": "+1234567892",
  "email": "broadway@pizzapalace.com",
  "deliveryEnabled": true,
  "pickupEnabled": true,
  "minOrderAmount": 12.00,
  "deliveryFee": 2.99,
  "deliveryTimeMinutes": 25
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Broadway Branch",
    "slug": "broadway",
    "isActive": true,
    "createdAt": "2024-01-15T00:00:00Z"
  }
}
```

---

### Update Branch Hours

**Endpoint**: `PUT /branches/:branchId/hours`

**Request Body**:
```json
{
  "hours": [
    {
      "dayOfWeek": 1,
      "openTime": "10:00",
      "closeTime": "22:00",
      "isClosed": false
    },
    {
      "dayOfWeek": 0,
      "isClosed": true
    }
  ]
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "message": "Branch hours updated successfully"
}
```

---

### Create Delivery Zone

**Endpoint**: `POST /branches/:branchId/delivery-zones`

**Request Body**:
```json
{
  "name": "Downtown Zone",
  "polygon": {
    "type": "Polygon",
    "coordinates": [
      [
        [-74.0060, 40.7128],
        [-74.0030, 40.7128],
        [-74.0030, 40.7158],
        [-74.0060, 40.7158],
        [-74.0060, 40.7128]
      ]
    ]
  },
  "deliveryFee": 2.99,
  "minOrderAmount": 10.00,
  "deliveryTimeMinutes": 20
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Downtown Zone",
    "deliveryFee": 2.99,
    "isActive": true
  }
}
```

---

## Menu Management

### List Menu Categories

**Endpoint**: `GET /restaurants/:restaurantId/menu/categories`

**Query Parameters**:
- `isActive` (optional: true/false)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "categories": [
      {
        "id": "uuid",
        "name": "Pizzas",
        "nameAr": "بيتزا",
        "description": "Our signature pizzas",
        "imageUrl": "https://cdn.example.com/pizzas.jpg",
        "sortOrder": 0,
        "isActive": true,
        "itemCount": 12
      },
      {
        "id": "uuid",
        "name": "Appetizers",
        "nameAr": "مقبلات",
        "sortOrder": 1,
        "isActive": true,
        "itemCount": 8
      }
    ]
  }
}
```

---

### Create Menu Category

**Endpoint**: `POST /restaurants/:restaurantId/menu/categories`

**Request Body**:
```json
{
  "name": "Desserts",
  "nameAr": "حلويات",
  "description": "Sweet treats to end your meal",
  "descriptionAr": "حلويات لذيذة لإنهاء وجبتك",
  "imageUrl": "https://cdn.example.com/desserts.jpg",
  "sortOrder": 5
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Desserts",
    "sortOrder": 5,
    "isActive": true
  }
}
```

---

### List Menu Items

**Endpoint**: `GET /restaurants/:restaurantId/menu/items`

**Query Parameters**:
- `categoryId` (optional)
- `isActive` (optional: true/false)
- `search` (optional: search by name)
- `tags` (optional: comma-separated, e.g., "popular,vegan")

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "id": "uuid",
        "categoryId": "uuid",
        "name": "Margherita Pizza",
        "nameAr": "بيتزا مارغريتا",
        "description": "Classic tomato, mozzarella, and basil",
        "imageUrl": "https://cdn.example.com/margherita.jpg",
        "price": 12.99,
        "calories": 800,
        "preparationTimeMinutes": 15,
        "isVegetarian": true,
        "isVegan": false,
        "isGlutenFree": false,
        "isSpicy": false,
        "allergens": ["dairy", "gluten"],
        "tags": ["popular", "classic"],
        "isActive": true,
        "modifiers": [
          {
            "id": "uuid",
            "name": "Size",
            "type": "single",
            "isRequired": true
          }
        ],
        "addons": [
          {
            "id": "uuid",
            "name": "Extra Cheese",
            "price": 2.00
          }
        ]
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 45
    }
  }
}
```

---

### Create Menu Item

**Endpoint**: `POST /restaurants/:restaurantId/menu/items`

**Request Body**:
```json
{
  "categoryId": "uuid",
  "name": "Pepperoni Pizza",
  "nameAr": "بيتزا ببروني",
  "description": "Classic pepperoni with mozzarella cheese",
  "descriptionAr": "بيبروني كلاسيكي مع جبنة موزاريلا",
  "imageUrl": "https://cdn.example.com/pepperoni.jpg",
  "price": 14.99,
  "costPrice": 6.50,
  "calories": 950,
  "preparationTimeMinutes": 15,
  "isVegetarian": false,
  "isSpicy": false,
  "allergens": ["dairy", "gluten", "pork"],
  "tags": ["popular", "bestseller"],
  "sortOrder": 1
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Pepperoni Pizza",
    "price": 14.99,
    "isActive": true,
    "createdAt": "2024-01-15T00:00:00Z"
  }
}
```

---

### Create Item Modifier

**Endpoint**: `POST /menu/items/:itemId/modifiers`

**Request Body**:
```json
{
  "name": "Size",
  "nameAr": "الحجم",
  "type": "single",
  "isRequired": true,
  "minSelections": 1,
  "maxSelections": 1,
  "options": [
    {
      "name": "Small (10\")",
      "nameAr": "صغير",
      "price": 0
    },
    {
      "name": "Medium (12\")",
      "nameAr": "وسط",
      "price": 3.00
    },
    {
      "name": "Large (14\")",
      "nameAr": "كبير",
      "price": 5.00
    }
  ]
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Size",
    "type": "single",
    "isRequired": true,
    "optionsCount": 3
  }
}
```

---

### Add Item Addons

**Endpoint**: `POST /menu/items/:itemId/addons`

**Request Body**:
```json
{
  "addons": [
    {
      "name": "Extra Cheese",
      "nameAr": "جبنة إضافية",
      "price": 2.00
    },
    {
      "name": "Mushrooms",
      "nameAr": "فطر",
      "price": 1.50
    },
    {
      "name": "Olives",
      "nameAr": "زيتون",
      "price": 1.00
    }
  ]
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "message": "Addons created successfully",
  "data": {
    "count": 3
  }
}
```

---

### Update Item Availability

**Endpoint**: `PUT /menu/items/:itemId/availability/:branchId`

**Request Body**:
```json
{
  "isAvailable": true,
  "availableFrom": "11:00",
  "availableUntil": "22:00",
  "availableDays": [1, 2, 3, 4, 5],
  "stockQuantity": 50
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "message": "Item availability updated"
}
```

---

## Order Management

### Create Order (Customer)

**Endpoint**: `POST /orders`

**Headers**:
```
X-Tenant-ID: <tenant_id>
Authorization: Bearer <customer_token> (optional for guest)
```

**Request Body**:
```json
{
  "restaurantId": "uuid",
  "branchId": "uuid",
  "orderType": "delivery",
  "customer": {
    "firstName": "Jane",
    "lastName": "Smith",
    "email": "jane@example.com",
    "phone": "+1234567890"
  },
  "deliveryAddress": {
    "addressLine1": "789 Oak St",
    "city": "New York",
    "state": "NY",
    "postalCode": "10001",
    "latitude": 40.7128,
    "longitude": -74.0060,
    "deliveryInstructions": "Ring doorbell twice"
  },
  "items": [
    {
      "menuItemId": "uuid",
      "quantity": 2,
      "modifiers": [
        {
          "modifierId": "uuid",
          "name": "Size",
          "option": "Large",
          "price": 5.00
        }
      ],
      "addons": [
        {
          "addonId": "uuid",
          "name": "Extra Cheese",
          "price": 2.00
        }
      ],
      "specialInstructions": "Extra sauce please"
    }
  ],
  "couponCode": "SAVE10",
  "paymentMethod": "stripe",
  "scheduledFor": "2024-01-15T18:00:00Z",
  "customerNotes": "Please deliver to side door",
  "tipAmount": 5.00
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "orderNumber": "ORD-20240115-0001",
    "status": "pending",
    "orderType": "delivery",
    "subtotal": 39.98,
    "discountAmount": 4.00,
    "deliveryFee": 3.99,
    "taxAmount": 3.40,
    "tipAmount": 5.00,
    "total": 48.37,
    "paymentStatus": "pending",
    "estimatedPreparationTime": 30,
    "estimatedDeliveryTime": 45,
    "createdAt": "2024-01-15T17:15:00Z",
    "payment": {
      "clientSecret": "pi_xxx_secret_xxx",
      "publishableKey": "pk_test_xxx"
    }
  }
}
```

---

### Get Order Details

**Endpoint**: `GET /orders/:orderId`

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "orderNumber": "ORD-20240115-0001",
    "status": "confirmed",
    "orderType": "delivery",
    "customer": {
      "name": "Jane Smith",
      "email": "jane@example.com",
      "phone": "+1234567890"
    },
    "deliveryAddress": "789 Oak St, New York, NY 10001",
    "items": [
      {
        "id": "uuid",
        "itemName": "Pepperoni Pizza",
        "quantity": 2,
        "unitPrice": 14.99,
        "modifiers": [
          {
            "name": "Size",
            "option": "Large",
            "price": 5.00
          }
        ],
        "addons": [
          {
            "name": "Extra Cheese",
            "price": 2.00
          }
        ],
        "subtotal": 43.98
      }
    ],
    "subtotal": 43.98,
    "discountAmount": 4.00,
    "deliveryFee": 3.99,
    "taxAmount": 3.73,
    "tipAmount": 5.00,
    "total": 52.70,
    "paymentMethod": "stripe",
    "paymentStatus": "paid",
    "statusHistory": [
      {
        "status": "pending",
        "createdAt": "2024-01-15T17:15:00Z"
      },
      {
        "status": "confirmed",
        "createdAt": "2024-01-15T17:16:30Z",
        "notes": "Payment confirmed"
      }
    ],
    "createdAt": "2024-01-15T17:15:00Z"
  }
}
```

---

### List Orders (Restaurant)

**Endpoint**: `GET /restaurants/:restaurantId/orders`

**Query Parameters**:
- `branchId` (optional)
- `status` (optional: pending, confirmed, preparing, ready, out_for_delivery, delivered, cancelled)
- `orderType` (optional: delivery, pickup, dine_in)
- `startDate` (optional: ISO 8601)
- `endDate` (optional: ISO 8601)
- `page` (default: 1)
- `limit` (default: 20)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": "uuid",
        "orderNumber": "ORD-20240115-0001",
        "status": "preparing",
        "orderType": "delivery",
        "customerName": "Jane Smith",
        "customerPhone": "+1234567890",
        "total": 52.70,
        "itemCount": 2,
        "createdAt": "2024-01-15T17:15:00Z",
        "estimatedDeliveryTime": 45
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150
    }
  }
}
```

---

### Update Order Status

**Endpoint**: `PATCH /orders/:orderId/status`

**Request Body**:
```json
{
  "status": "preparing",
  "notes": "Order started",
  "estimatedPreparationTime": 20
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "status": "preparing",
    "updatedAt": "2024-01-15T17:20:00Z"
  }
}
```

---

### Cancel Order

**Endpoint**: `POST /orders/:orderId/cancel`

**Request Body**:
```json
{
  "reason": "Customer requested cancellation",
  "refund": true
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "status": "cancelled",
    "cancellationReason": "Customer requested cancellation",
    "refundStatus": "processing"
  }
}
```

---

## Customer Management

### Get Customer Profile

**Endpoint**: `GET /customers/me`

**Headers**:
```
Authorization: Bearer <customer_token>
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "firstName": "Jane",
    "lastName": "Smith",
    "email": "jane@example.com",
    "phone": "+1234567890",
    "language": "en",
    "addresses": [
      {
        "id": "uuid",
        "label": "Home",
        "addressLine1": "789 Oak St",
        "city": "New York",
        "isDefault": true
      }
    ],
    "orderCount": 15,
    "totalSpent": 450.00
  }
}
```

---

### Update Customer Profile

**Endpoint**: `PATCH /customers/me`

**Request Body**:
```json
{
  "firstName": "Jane",
  "lastName": "Smith-Jones",
  "phone": "+1234567891",
  "language": "ar"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "firstName": "Jane",
    "lastName": "Smith-Jones",
    "updatedAt": "2024-01-15T18:00:00Z"
  }
}
```

---

### Add Customer Address

**Endpoint**: `POST /customers/me/addresses`

**Request Body**:
```json
{
  "label": "Office",
  "addressLine1": "100 Business Plaza",
  "addressLine2": "Suite 500",
  "city": "New York",
  "state": "NY",
  "postalCode": "10002",
  "latitude": 40.7150,
  "longitude": -74.0050,
  "deliveryInstructions": "Call when arrived",
  "isDefault": false
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "label": "Office",
    "isDefault": false
  }
}
```

---

### Get Customer Order History

**Endpoint**: `GET /customers/me/orders`

**Query Parameters**:
- `page` (default: 1)
- `limit` (default: 10)
- `status` (optional)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": "uuid",
        "orderNumber": "ORD-20240115-0001",
        "restaurantName": "Pizza Palace Downtown",
        "status": "delivered",
        "total": 52.70,
        "itemCount": 2,
        "createdAt": "2024-01-15T17:15:00Z",
        "deliveredAt": "2024-01-15T18:05:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 15
    }
  }
}
```

---

## Payment Management

### Create Stripe Payment Intent

**Endpoint**: `POST /payments/stripe/create-intent`

**Request Body**:
```json
{
  "orderId": "uuid",
  "amount": 5270,
  "currency": "usd"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "clientSecret": "pi_xxx_secret_xxx",
    "publishableKey": "pk_test_xxx",
    "paymentIntentId": "pi_xxx"
  }
}
```

---

### Stripe Webhook

**Endpoint**: `POST /webhooks/stripe`

**Headers**:
```
Stripe-Signature: <signature>
```

**Body**: (Stripe event JSON)

**Response**: `200 OK`

---

### Create PayPal Order

**Endpoint**: `POST /payments/paypal/create-order`

**Request Body**:
```json
{
  "orderId": "uuid",
  "amount": 52.70,
  "currency": "USD"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "paypalOrderId": "PAYPAL-ORDER-ID",
    "approvalUrl": "https://paypal.com/approve?token=xxx"
  }
}
```

---

### Capture PayPal Payment

**Endpoint**: `POST /payments/paypal/capture`

**Request Body**:
```json
{
  "paypalOrderId": "PAYPAL-ORDER-ID",
  "orderId": "uuid"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "status": "COMPLETED",
    "captureId": "CAPTURE-ID"
  }
}
```

---

## Coupon Management

### List Coupons

**Endpoint**: `GET /restaurants/:restaurantId/coupons`

**Query Parameters**:
- `isActive` (optional: true/false)
- `page` (default: 1)
- `limit` (default: 20)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "coupons": [
      {
        "id": "uuid",
        "code": "SAVE10",
        "name": "10% Off All Orders",
        "discountType": "percentage",
        "discountValue": 10.00,
        "minOrderAmount": 20.00,
        "usageLimit": 1000,
        "usedCount": 245,
        "validFrom": "2024-01-01T00:00:00Z",
        "validUntil": "2024-12-31T23:59:59Z",
        "isActive": true
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 5
    }
  }
}
```

---

### Create Coupon

**Endpoint**: `POST /restaurants/:restaurantId/coupons`

**Request Body**:
```json
{
  "code": "FIRSTORDER",
  "name": "First Order Discount",
  "description": "Get 20% off your first order",
  "discountType": "percentage",
  "discountValue": 20.00,
  "minOrderAmount": 15.00,
  "maxDiscountAmount": 10.00,
  "usageLimit": 500,
  "usageLimitPerCustomer": 1,
  "validFrom": "2024-01-01T00:00:00Z",
  "validUntil": "2024-06-30T23:59:59Z",
  "applicableBranches": ["uuid1", "uuid2"],
  "isActive": true
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "code": "FIRSTORDER",
    "isActive": true,
    "createdAt": "2024-01-15T00:00:00Z"
  }
}
```

---

### Validate Coupon

**Endpoint**: `POST /coupons/validate`

**Request Body**:
```json
{
  "code": "SAVE10",
  "restaurantId": "uuid",
  "branchId": "uuid",
  "orderAmount": 35.00,
  "customerId": "uuid"
}
```

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "valid": true,
    "coupon": {
      "id": "uuid",
      "code": "SAVE10",
      "discountType": "percentage",
      "discountValue": 10.00
    },
    "discountAmount": 3.50,
    "finalAmount": 31.50
  }
}
```

---

## Reports & Analytics

### Get Sales Report

**Endpoint**: `GET /restaurants/:restaurantId/reports/sales`

**Query Parameters**:
- `branchId` (optional)
- `startDate` (required: ISO 8601)
- `endDate` (required: ISO 8601)
- `groupBy` (optional: day, week, month)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "summary": {
      "totalOrders": 1250,
      "totalRevenue": 45000.00,
      "averageOrderValue": 36.00,
      "totalDeliveryFees": 4987.50,
      "totalDiscounts": 2250.00
    },
    "breakdown": [
      {
        "date": "2024-01-01",
        "orders": 45,
        "revenue": 1620.00,
        "averageOrderValue": 36.00
      }
    ],
    "topItems": [
      {
        "itemName": "Pepperoni Pizza",
        "quantity": 450,
        "revenue": 6742.50
      }
    ]
  }
}
```

---

### Get Popular Items Report

**Endpoint**: `GET /restaurants/:restaurantId/reports/popular-items`

**Query Parameters**:
- `branchId` (optional)
- `startDate` (required)
- `endDate` (required)
- `limit` (default: 10)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "items": [
      {
        "itemId": "uuid",
        "itemName": "Margherita Pizza",
        "categoryName": "Pizzas",
        "orderCount": 450,
        "quantitySold": 520,
        "revenue": 6748.00,
        "averageRating": 4.7
      }
    ]
  }
}
```

---

### Get Revenue Report

**Endpoint**: `GET /restaurants/:restaurantId/reports/revenue`

**Query Parameters**:
- `startDate` (required)
- `endDate` (required)

**Response**: `200 OK`
```json
{
  "success": true,
  "data": {
    "totalRevenue": 45000.00,
    "orderRevenue": 40012.50,
    "deliveryFees": 4987.50,
    "tips": 2250.00,
    "taxes": 3825.00,
    "discounts": -2250.00,
    "refunds": -825.00,
    "netRevenue": 42175.00,
    "byPaymentMethod": {
      "stripe": 35000.00,
      "paypal": 8000.00,
      "cash": 2000.00
    }
  }
}
```

---

## Webhooks

### Register Webhook

**Endpoint**: `POST /webhooks`

**Request Body**:
```json
{
  "url": "https://your-app.com/webhooks/orders",
  "events": ["order.created", "order.status_changed", "payment.succeeded"],
  "secret": "your_webhook_secret"
}
```

**Response**: `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "url": "https://your-app.com/webhooks/orders",
    "events": ["order.created", "order.status_changed"],
    "isActive": true
  }
}
```

---

## Error Responses

All error responses follow this format:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  }
}
```

### Common Error Codes:
- `VALIDATION_ERROR` (400)
- `UNAUTHORIZED` (401)
- `FORBIDDEN` (403)
- `NOT_FOUND` (404)
- `CONFLICT` (409)
- `RATE_LIMIT_EXCEEDED` (429)
- `INTERNAL_ERROR` (500)

---

## Rate Limiting

- **Default**: 100 requests per minute per IP
- **Authenticated**: 500 requests per minute per user
- **Headers**:
  ```
  X-RateLimit-Limit: 100
  X-RateLimit-Remaining: 95
  X-RateLimit-Reset: 1640000000
  ```

---

## Pagination

All list endpoints support pagination:

**Query Parameters**:
- `page` (default: 1)
- `limit` (default: 20, max: 100)

**Response includes**:
```json
{
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

---

## Authentication Headers

**Required for all authenticated endpoints**:
```
Authorization: Bearer <access_token>
X-Tenant-ID: <tenant_id>
```

---

*This API documentation is version 1.0. Last updated: 2024-01-15*
