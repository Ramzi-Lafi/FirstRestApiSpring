# First REST API Spring

A Spring Boot REST API application built as part of Task 2 for the Spring Framework course at Akademia Finansów i Biznesu Vistula.

---

## Description

This is a fully functional REST API application that demonstrates:
- Creating a Spring Boot REST API project from scratch
- Layered architecture (Controller → Service → Repository)
- Full CRUD operations using HTTP methods (POST, GET, PUT, DELETE)
- Spring Data JPA with H2 in-memory database
- API documentation and testing using Swagger UI
- Proper use of Spring stereotypes (@RestController, @Service, @Repository, @Component)

---

## Project Structure

```
first-rest-api-spring/
├── src/
│   └── main/
│       ├── java/
│       │   └── pl/edu/vistula/firstrestapispring/
│       │       ├── product/
│       │       │   ├── api/
│       │       │   │   ├── request/
│       │       │   │   │   └── ProductRequest.java
│       │       │   │   ├── response/
│       │       │   │   │   └── ProductResponse.java
│       │       │   │   └── ProductController.java
│       │       │   ├── domain/
│       │       │   │   └── Product.java
│       │       │   ├── repository/
│       │       │   │   └── ProductRepository.java
│       │       │   ├── service/
│       │       │   │   └── ProductService.java
│       │       │   └── support/
│       │       │       └── ProductMapper.java
│       │       └── FirstRestApiSpringApplication.java
│       └── resources/
│           └── application.properties
├── .gitignore
├── pom.xml
└── README.md
```

---

## Architecture Explanation

The project follows a layered architecture pattern:

```
Client (Swagger/Postman)
        ↓
  ProductController      ← receives HTTP requests, sends HTTP responses
        ↓
  ProductService         ← handles business logic
        ↓
  ProductRepository      ← talks to the database
        ↓
  H2 Database            ← stores the data
```

The `ProductMapper` class is a helper that converts objects between layers:
- `ProductRequest` → `Product` (incoming data to database entity)
- `Product` → `ProductResponse` (database entity to outgoing data)

---

## Technologies Used

- Java 17
- Spring Boot
- Spring Web (REST)
- Spring Data JPA
- Hibernate
- H2 In-Memory Database
- Swagger UI (springdoc-openapi)
- Maven

---

## How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/Ramzi-Lafi/FirstRestApiSpring.git
   ```

2. Open the project in IntelliJ IDEA

3. Right-click the project → Maven → Reload Project

4. Run `FirstRestApiSpringApplication.java`

5. The app runs on `localhost:8080`

---

## Testing the API

### Option 1 — Swagger UI
Open your browser and go to:
```
localhost:8080/swagger-ui/index.html
```

### Option 2 — H2 Database Console
Open your browser and go to:
```
localhost:8080/console
```
Change the JDBC URL to `jdbc:h2:mem:testdb` and click Connect.

---

## API Endpoints

### 1. POST `/api/v1/products` — Create a product

**Description:** Creates a new product and saves it to the database.

**Request body:**
```json
{
  "name": "First product"
}
```

**Response (201 Created):**
```json
{
  "id": 1,
  "name": "First product"
}
```

![POST create product](<img width="1919" height="912" alt="image" src="https://github.com/user-attachments/assets/1f1cf1d9-dfb9-4adf-8e95-4da1f3c904cf" />
)

---

### 2. GET `/api/v1/products/{id}` — Get one product by id

**Description:** Returns a single product by its id.

**URL example:** `localhost:8080/api/v1/products/1`

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "First product"
}
```

![GET product by id](<img width="1916" height="909" alt="image" src="https://github.com/user-attachments/assets/43cf10d4-e830-4b39-a54b-ee2b168ee9f8" />
)

---

### 3. GET `/api/v1/products` — Get all products

**Description:** Returns a list of all products in the database.

**Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "First product"
  },
  {
    "id": 2,
    "name": "Second product"
  }
]
```

![GET all products](<img width="1918" height="909" alt="image" src="https://github.com/user-attachments/assets/f313e97e-99dc-4ae7-a754-5da3e94a0fd2" />
)

---

### 4. PUT `/api/v1/products/{id}` — Update a product

**Description:** Updates the name of an existing product by its id.

**URL example:** `localhost:8080/api/v1/products/1`

**Request body:**
```json
{
  "name": "Updated product"
}
```

**Response (200 OK):**
```json
{
  "id": 1,
  "name": "Updated product"
}
```

![PUT update product](<img width="1918" height="912" alt="image" src="https://github.com/user-attachments/assets/aeac580b-3db7-40b2-83ad-d9e2735685fe" />
)

---

### 5. DELETE `/api/v1/products/{id}` — Delete a product

**Description:** Deletes a product by its id. Returns no content.

**URL example:** `localhost:8080/api/v1/products/1`

**Response: 204 No Content**

![DELETE product](<img width="1919" height="911" alt="image" src="https://github.com/user-attachments/assets/aea0e0a8-19ec-4886-8a5a-a5f7ac452ab9" />
)

---

## Database

The app uses the H2 in-memory database. This means data is stored only while the app is running and resets every time the app restarts. This is useful for development and testing.

You can run SQL queries directly in the H2 console:

```sql
SELECT * FROM PRODUCTS;
```

![H2 database console](<img width="1919" height="906" alt="image" src="https://github.com/user-attachments/assets/9023a654-fdd4-46d2-87e2-7f238edbbab7" />
)

---

## Code Explanation

### `@RestController` vs `@Controller`

| `@RestController` | `@Controller` |
|---|---|
| Returns data directly (JSON) | Returns the name of an HTML view |
| Used for REST APIs | Used with Thymeleaf templates |
| `@ResponseBody` applied automatically | Needs `@ResponseBody` for raw data |

### Spring Stereotypes used

| Annotation | Class | Purpose |
|---|---|---|
| `@RestController` | `ProductController` | Handles HTTP requests and responses |
| `@Service` | `ProductService` | Contains business logic |
| `@Repository` | `ProductRepository` | Talks to the database |
| `@Component` | `ProductMapper` | Helper class for object mapping |

### HTTP Status Codes returned

| Operation | Status Code | Meaning |
|---|---|---|
| Create (POST) | 201 Created | Resource created successfully |
| Read (GET) | 200 OK | Resource returned successfully |
| Update (PUT) | 200 OK | Resource updated successfully |
| Delete (DELETE) | 204 No Content | Resource deleted, nothing to return |

### Why is `ProductRepository` empty?

`ProductRepository` extends `JpaRepository<Product, Long>`. Spring Data JPA automatically generates all the database methods (`save`, `findById`, `findAll`, `deleteById`) at runtime. We don't need to write any SQL or implement any methods ourselves.

---

## Screenshots

> Place your screenshots in a folder called `screenshots/` in the root of the repository.

| Screenshot | Description |
|---|---|
| `screenshots/swagger_ui.png` | Swagger UI showing all endpoints |
| `screenshots/post_create.png` | POST request creating a product |
| `screenshots/get_by_id.png` | GET request for one product |
| `screenshots/get_all.png` | GET request for all products |
| `screenshots/put_update.png` | PUT request updating a product |
| `screenshots/delete.png` | DELETE request removing a product |
| `screenshots/h2_console.png` | H2 database console showing the PRODUCTS table |
