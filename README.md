# Take-Out-System Backend

## Introduction

Take-Out-System is a catering-focused software solution that includes both a management backend (for restaurant staff) and a frontend experience. The backend portion is responsible for tasks such as employee management, category and dish management, set menu management, orders, data reporting, and basic authentication/authorization. It provides RESTful APIs consumed by both the management dashboard and the user-facing frontend.

## Project Overview

**Key Features (Backend):**

- **Authentication & Authorization:** Employee login/logout, token-based authentication (JWT).
- **Employee Management:** Create, edit, disable employees, and manage related information.
- **Category Management:** Manage dish and set meal categories.
- **Dish & Set Meal Management:** Maintain dishes and set meals, including pricing, status (on-sale/off-sale), and flavors.
- **Order Management:** Handle user orders from the mini program, including order inquiries, cancellation, dispatch, completion, and basic order reporting.
- **Data Reporting & Statistics:** Provide data insights (e.g., revenue, orders) for management decision-making.
- **Notification and Real-time Capabilities:** Use WebSocket for order notifications and reminders.

**Architecture:**

The system is built with a layered architecture:

- **Frontend (Not included in this repo):** Managed via a separate project and served by Nginx.
- **Backend (This Project):** A Spring Boot application exposing RESTful APIs.
- **Database Layer:** MySQL as the primary relational database, with MyBatis for persistence.
- **Caching Layer:** Redis for caching frequently accessed data.
- **Additional Tools:** Knife4j (enhanced Swagger) for API documentation, POI for Excel operations, JWT for token handling, and OSS (Object Storage Service) integration for file storage (e.g., dish images).

## Technology Stack

- **Application Layer:**
  - **Spring Boot:** Rapid setup and reduced configuration.
  - **Spring MVC:** RESTful API endpoints.
  - **Spring Task:** Scheduled tasks.
  - **Spring Cache & Redis:** Caching support.
  - **MyBatis + PageHelper:** Database persistence and pagination.
  - **JWT:** Token-based authentication.
  - **Swagger / Knife4j:** API documentation and testing.
  - **WebSocket:** Real-time features (e.g., order reminders).

- **Data Layer:**
  - **MySQL:** Core business data storage.
  - **Redis:** In-memory caching for performance optimization.

- **DevOps & Utilities:**
  - **Nginx:** Reverse proxy, load balancing, and static content serving.
  - **Git/Maven:** Version control and build management.
  - **Junit / Postman:** Testing and API request simulation.

## Environment Setup

### Prerequisites

- **Java 8+**
- **Maven 3+**
- **MySQL 5.7+**
- **Redis 5+**
- **Nginx (optional, for reverse proxy)**
- **IDE (e.g., IntelliJ IDEA)**

### Database Initialization

1. Locate the `sky.sql` file in the project materials.
2. Execute the `sky.sql` file against your MySQL instance.  
   This will create and populate the required schema and tables:
   - `employee` (Employees)
   - `category` (Dish & set meal categories)
   - `dish` (Dishes)
   - `dish_flavor` (Dish flavors)
   - `setmeal` (Set meals)
   - `setmeal_dish` (Link table between set meals and dishes)
   - `user` (Users)
   - `address_book` (User address)
   - `shopping_cart` (Shopping cart)
   - `orders` (Orders)
   - `order_detail` (Order details)

### Project Structure

- **sky-take-out (parent)**: Maven parent project.
- **sky-common**: Common classes, including constants, exceptions, utilities, and result wrappers.
- **sky-pojo**: Plain Old Java Objects, including Entities, DTOs, and VOs.
- **sky-server**: Core backend service module containing controllers, services, mappers, configurations, and the application’s main entry point.

#### sky-common  
Holds:
- Constants
- Context holders
- Enumerations
- Custom exceptions
- JSON utilities
- Properties configuration classes
- Result response classes
- Common utilities

#### sky-pojo  
Holds:
- **Entity:** Database-mapped classes.
- **DTO (Data Transfer Objects):** For data interchange between layers.
- **VO (View Objects):** For shaping responses returned to the frontend.

#### sky-server  
Holds:
- **config:** Spring Boot configuration classes.
- **controller:** REST endpoints.
- **interceptor:** Interceptors for request handling.
- **mapper:** MyBatis mapper interfaces for database operations.
- **service:** Business logic layer.

### Running the Project

1. Import the project into an IDE (e.g., IntelliJ IDEA).
2. Run `SkyApplication.java` from `sky-server` module.
3. The server will start on `http://localhost:8080`.

### Testing the APIs

- **Swagger/Knife4j Documentation:**  
  After starting the application, open [http://localhost:8080/doc.html](http://localhost:8080/doc.html) to view the auto-generated API documentation.  
  Here you can:
  - Review all available endpoints.
  - Test endpoints interactively.

- **Postman/JUnit:**  
  For further testing, use Postman to simulate requests or write JUnit tests for unit-level testing.

### Reverse Proxy with Nginx (Optional)

For production deployment or integration with frontend assets, you can set up Nginx to:
- Serve static files (e.g., front-end resources).
- Act as a reverse proxy:  
  Requests to `http://your_server/api/...` can be forwarded to the backend at `http://localhost:8080/admin/...`.

Basic reverse proxy configuration in Nginx:
```nginx
server {
    listen       80;
    server_name  localhost;

    location /api/ {
        proxy_pass http://localhost:8080/admin/;
    }
}
```
This maps frontend calls to `/api/` endpoints to the backend’s `/admin/` APIs.

---

### Security and Password Encryption

- **Hashing Passwords with MD5:**  
  Employee passwords are stored as MD5-hashed strings.
- **Secure Login Process:**  
  During login, the backend MD5-encrypts the incoming password before comparing it to the stored hash.
- **No Plain-Text Storage:**  
  This ensures enhanced security by not storing plain-text passwords.

---

### API Documentation and Interface Management

- **Swagger/Knife4j:**  
  Used during development for generating and testing backend APIs.
- **YApi (Not Required Here):**  
  Useful during design and team collaboration phases for defining and reviewing APIs.

---

### Conclusion

With Sky Take-Out’s backend, developers gain a comprehensive example of a modern, full-featured catering service management system. This includes:

- **API Design**
- **Database Operations**
- **Caching**
- **Security**
- **Testing**

All of these combined offer a complete reference for enterprise-level backend development practices.
