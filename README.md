# Product Service

This is a Spring Boot microservice designed to manage product information, including details, categories, inventory, and product images. It integrates with a MongoDB database and is built to be part of a larger microservices ecosystem, leveraging Spring Cloud for configuration and service discovery.

## Features

*   **CRUD Operations:** Create, Read, Update, and Delete products.
*   **Product Categorization:** Assign products to predefined categories.
*   **Inventory Management:** Handles stock reduction upon product purchase requests.
*   **Image Upload:** Allows uploading cover images for products.
*   **Category Management:** Retrieve all available product categories.
*   **Database Seeding:** Initializes the database with sample categories and products on the first run if the database is empty.
*   **Externalized Configuration:** Uses Spring Cloud Config Client to fetch configuration from a central Config Server.
*   **Service Discovery:** Registers with a Spring Cloud Eureka Server (if available).

## Technologies Used

*   Java 17
*   Spring Boot 3.x
*   Spring Data MongoDB
*   Spring Web
*   Spring Cloud Config Client
*   Spring Cloud Eureka Client
*   Maven (with Maven Wrapper)
*   Lombok
*   MongoDB
*   Docker & Docker Compose (for running MongoDB locally)

## Prerequisites

*   **JDK 17** or later
*   **Maven** 3.x
*   **Docker** and **Docker Compose** (if running MongoDB via the provided `docker-compose.yml`)
*   Access to a running **Spring Cloud Config Server** (defaults to `http://localhost:8888`)
*   Access to a running **Spring Cloud Eureka Server** (the service will attempt to register)

## Getting Started

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd product-service
    ```

2.  **Start Dependencies:**
    *   **MongoDB:** You can use the provided Docker Compose file to start a MongoDB instance:
        ```bash
        docker-compose up -d mongo
        ```
        This will start MongoDB and Mongo Express (a web-based admin interface accessible at `http://localhost:8081`).
    *   **Spring Cloud Config Server:** Ensure your Config Server is running and accessible at the configured URL (default: `http://localhost:8888`). It should serve the configuration for `product-service`.
    *   **Spring Cloud Eureka Server:** Ensure your Eureka Server is running and accessible for service registration.

3.  **Configure the Application:**
    *   The primary configuration is fetched from the Spring Cloud Config Server. Ensure you have a configuration file (e.g., `product-service.yml` or `product-service.properties`) set up on your Config Server. This file should contain MongoDB connection details, Eureka server details, and any other environment-specific settings.
    *   The local `src/main/resources/application.yml` mainly points to the Config Server and configures the file upload path (`spring.application.file.uploads.photos-output-path`, defaults to `./uploads`).

The service will start, connect to MongoDB, attempt to fetch configuration from the Config Server, and register with the Eureka Server. By default, it runs on port 8040 (this might be overridden by the configuration from the Config Server).

API Endpoints
The main API endpoints are available under the /api/v1/products base path:

POST /: Create a new product.

Body: ProductRequest JSON object.

GET /: Retrieve all products.

Returns: List of ProductResponse objects.

GET /{product-id}: Retrieve a specific product by its ID.

Returns: ProductResponse object.

PUT /{product-id}: Update an existing product.

Body: ProductRequest JSON object.

DELETE /{product-id}: Delete a product by its ID.

POST /purchase: Process product purchases and update stock levels.

Body: List of ProductPurchaseRequest objects.

Returns: List of ProductPurchaseResponse objects.

POST /cover/{product-id} (multipart/form-data): Upload a cover image for a product.

Part: file (the image file).

GET /categories: Retrieve all available product categories.

Returns: List of Category objects.

File Uploads
Uploaded product cover images are stored locally in the directory specified by the spring.application.file.uploads.photos-output-path property in the configuration (default: ./uploads).


