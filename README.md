# Spring Boot CRUD API with H2 Database

## 📌 Overview
This is a simple **Spring Boot REST API** that performs **CRUD (Create, Read, Update, Delete) operations** on a `Product` entity using **Spring Boot, Spring Data JPA, and H2 Database**.

## 🛠 Technologies Used
- **Spring Boot** (REST API framework)
- **Spring Data JPA** (Database interaction)
- **H2 Database** (In-memory database for testing)
- **Lombok** (Reduces boilerplate code)
- **Maven** (Dependency management)
- **Postman / Thunder Client** (For testing API requests)

## 📁 Project Structure
```
demo/
├── src/
│   ├── main/java/com/example/demo/
│   │   ├── controller/        # REST Controllers
│   │   ├── model/             # JPA Entity Classes
│   │   ├── repository/        # Spring Data Repositories
│   │   ├── service/           # Business Logic
│   │   ├── CrudApiApplication.java  # Main Application File
│   ├── resources/
│   │   ├── application.properties  # Database Configurations
├── pom.xml                    # Maven Dependencies
├── README.md                   # Project Documentation
```

## ⚙️ Setup Instructions

### 🔹 Step 1: Clone the Repository
```sh
git clone https://github.com/your-username/demo.git
cd demo
```

### 🔹 Step 2: Configure the Database
Modify `src/main/resources/application.properties` if needed:
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### 🔹 Step 3: Run the Application
```sh
mvn spring-boot:run
```
The application will start at **`http://localhost:8080/`**.

### 🔹 Step 4: Open H2 Database Console
Visit: **`http://localhost:8080/h2-console`**
- **JDBC URL:** `jdbc:h2:mem:testdb`
- **Username:** `sa`
- **Password:** *(leave empty)*

---

## 🛠 API Endpoints

| Method | Endpoint | Description |
|--------|---------|-------------|
| **GET** | `/api/products` | Get all products |
| **GET** | `/api/products/{id}` | Get product by ID |
| **POST** | `/api/products` | Create a new product |
| **PUT** | `/api/products/{id}` | Update a product |
| **DELETE** | `/api/products/{id}` | Delete a product |

---

## 📜 Code Implementation

### **1️⃣ Product Model (`Product.java`)**
```java
package com.example.demo.model;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "products")
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Product {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private double price;
}
```

### **2️⃣ Product Repository (`ProductRepository.java`)**
```java
package com.example.demo.repository;

import com.example.demo.model.Product;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ProductRepository extends JpaRepository<Product, Long> {
}
```

### **3️⃣ Product Service (`ProductService.java`)**
```java
package com.example.demo.service;

import com.example.demo.model.Product;
import com.example.demo.repository.ProductRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Optional;

@Service
public class ProductService {

    @Autowired
    private ProductRepository productRepository;

    public List<Product> getAllProducts() {
        return productRepository.findAll();
    }

    public Optional<Product> getProductById(Long id) {
        return productRepository.findById(id);
    }

    public Product createProduct(Product product) {
        return productRepository.save(product);
    }

    public Product updateProduct(Long id, Product updatedProduct) {
        return productRepository.findById(id)
                .map(product -> {
                    product.setName(updatedProduct.getName());
                    product.setPrice(updatedProduct.getPrice());
                    return productRepository.save(product);
                })
                .orElseThrow(() -> new RuntimeException("Product not found"));
    }

    public void deleteProduct(Long id) {
        productRepository.deleteById(id);
    }
}
```

### **4️⃣ Product Controller (`ProductController.java`)**
```java
package com.example.demo.controller;

import com.example.demo.model.Product;
import com.example.demo.service.ProductService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/api/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.getAllProducts();
    }

    @GetMapping("/{id}")
    public Optional<Product> getProductById(@PathVariable Long id) {
        return productService.getProductById(id);
    }

    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.createProduct(product);
    }

    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product updatedProduct) {
        return productService.updateProduct(id, updatedProduct);
    }

    @DeleteMapping("/{id}")
    public String deleteProduct(@PathVariable Long id) {
        productService.deleteProduct(id);
        return "Product deleted successfully";
    }
}
```

---

## 🚀 Testing the API
### **Using Curl**
```sh
# Create a new product
curl -X POST -H "Content-Type: application/json" -d '{"name":"Laptop", "price":1200.50}' http://localhost:8080/api/products
```

### **Using Thunder Client / Postman**
- Use **GET/POST/PUT/DELETE** requests with **JSON body**.

---

## 📜 License
This project is licensed under the **MIT License**.

---

## 🔥 Author
Developed by **Your Name** 🚀. Feel free to contribute!

📧 Contact: [cfprabhu@yahoo.com](mailto:cfprabhu@yahoo.com)

🔗 GitHub Repository: [https://github.com/cfprabhu21/spring-boot-crud-h2-example](https://github.com/cfprabhu21/spring-boot-crud-h2-example)

