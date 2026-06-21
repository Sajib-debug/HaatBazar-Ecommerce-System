# HaatBazar API Documentation

**Base Gateway URL:** `http://localhost:8080/api`

This document details the available endpoints for the **Authentication Service** and the **Product Service**. All requests should be routed through the API Gateway on port `8080`.

---

## 🔐 1. Authentication Service (`/api/auth`)

### 1.1 Register User
Register a new user (Customer or Seller).
* **Method:** `POST`
* **Endpoint:** `/auth/register`
* **Access:** Public
* **Request Body:**
  ```json
  {
      "name": "John Doe",
      "email": "john@example.com",
      "password": "password123",
      "role": "CUSTOMER" // Or "SELLER"
  }
  ```
* **Success Response:** `200 OK`
  ```json
  {
      "token": "eyJhbGciOiJIUzI1NiJ9..."
  }
  ```

### 1.2 Login User
Authenticate a user and retrieve a JWT token.
* **Method:** `POST`
* **Endpoint:** `/auth/login`
* **Access:** Public
* **Request Body:**
  ```json
  {
      "email": "john@example.com",
      "password": "password123"
  }
  ```
* **Success Response:** `200 OK`
  ```json
  {
      "token": "eyJhbGciOiJIUzI1NiJ9..."
  }
  ```

### 1.3 Validate Token (Internal / Gateway Use)
Validates a given JWT token and extracts user details.
* **Method:** `GET`
* **Endpoint:** `/auth/validate?token={jwt_token}`
* **Access:** Public
* **Success Response:** `200 OK`
  ```json
  {
      "email": "john@example.com",
      "role": "CUSTOMER",
      "valid": "true"
  }
  ```

---

## 🏷️ 2. Category Management (`/api/categories`)

### 2.1 Get All Categories
* **Method:** `GET`
* **Endpoint:** `/categories`
* **Access:** Public
* **Success Response:** `200 OK`
  ```json
  [
      { "id": 1, "name": "Electronics" },
      { "id": 2, "name": "Clothing" }
  ]
  ```

### 2.2 Get Category By ID
* **Method:** `GET`
* **Endpoint:** `/categories/{id}`
* **Access:** Public
* **Success Response:** `200 OK`

### 2.3 Create Category
* **Method:** `POST`
* **Endpoint:** `/categories`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Request Body:**
  ```json
  {
      "name": "Electronics"
  }
  ```

### 2.4 Update Category
* **Method:** `PUT`
* **Endpoint:** `/categories/{id}`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Request Body:**
  ```json
  {
      "name": "Updated Electronics"
  }
  ```

### 2.5 Delete Category
* **Method:** `DELETE`
* **Endpoint:** `/categories/{id}`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Success Response:** `200 OK` ("Category deleted successfully")

---

## 📦 3. Product Management (`/api/products`)

### 3.1 Get All Products
* **Method:** `GET`
* **Endpoint:** `/products`
* **Access:** Public
* **Success Response:** `200 OK`
  ```json
  [
      {
          "id": 1,
          "name": "Gaming Laptop",
          "description": "High-end laptop",
          "price": 1200.0,
          "stock": 10,
          "category": "Electronics"
      }
  ]
  ```

### 3.2 Get Product By ID
* **Method:** `GET`
* **Endpoint:** `/products/{id}`
* **Access:** Public
* **Success Response:** `200 OK`

### 3.3 Create Product
* **Method:** `POST`
* **Endpoint:** `/products`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Request Body:**
  ```json
  {
      "name": "Gaming Laptop",
      "description": "High-end laptop",
      "price": 1200.0,
      "stock": 10,
      "categoryId": 1
  }
  ```
*(Note: Creating a product automatically creates a corresponding Inventory record).*

### 3.4 Update Product
* **Method:** `PUT`
* **Endpoint:** `/products/{id}`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Request Body:** Same as Create Product

### 3.5 Delete Product
* **Method:** `DELETE`
* **Endpoint:** `/products/{id}`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
*(Note: Deleting a product automatically deletes its Inventory record).*

---

## 📊 4. Inventory Management (`/api/inventory`)

### 4.1 Check Availability
* **Method:** `GET`
* **Endpoint:** `/inventory/{productId}`
* **Access:** Public
* **Success Response:** `200 OK`
  ```json
  {
      "productId": 1,
      "productName": "Gaming Laptop",
      "quantity": 10,
      "available": true
  }
  ```

### 4.2 Update Stock (Set exact amount)
* **Method:** `PUT`
* **Endpoint:** `/inventory/{productId}`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Request Body:**
  ```json
  {
      "quantity": 25
  }
  ```

### 4.3 Reduce Stock (For Orders/Purchases)
* **Method:** `PUT`
* **Endpoint:** `/inventory/{productId}/reduce?quantity={amount}`
* **Access:** SELLER, ADMIN *(Requires Bearer Token)*
* **Success Response:** `200 OK` ("Stock reduced successfully")
