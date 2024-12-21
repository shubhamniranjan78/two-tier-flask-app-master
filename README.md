# Containerization of a Two-Tier Application with Docker, Docker Compose, and Docker Scout

Hey everyone! This project involved containerizing a simple two-tier web application using Docker, orchestrating it with Docker Compose, and enhancing security using Docker Scout. It was a great learning experience that gave me practical insights into containerization and security practices.

## Overview

The core idea was to take a basic Flask web application (acting as a front end) and a PostgreSQL database (acting as a backend) and containerize them individually. Then, I used Docker Compose to manage these containers as a single application. Finally, I scanned the built images for vulnerabilities using Docker Scout and made sure I am following best practices.

### Key Objectives

*   Containerize the web application using Docker.
*   Containerize the database using Docker.
*   Orchestrate the web application and database using Docker Compose.
*   Ensure communication between containers.
*   Scan Docker images for vulnerabilities using Docker Scout.
*  Optimize images for performance.

## Requirements

Before you get started, you'll need the following:

1.  **Docker:**  Make sure you have Docker installed and running. You can get it from [https://www.docker.com/](https://www.docker.com/)
2.  **Docker Compose:** You can install this along with docker.
3.  **Docker Scout:** Ensure that docker scout is setup and ready. You can find the details here [https://docs.docker.com/scout/](https://docs.docker.com/scout/)
4.  **Text Editor:**  Any code editor (VS Code, Atom, etc.) for editing the `docker-compose.yml` and `Dockerfile`.

## Getting Started

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/LondheShubham153/two-tier-flask-app
    cd two-tier-flask-app
    ```

2.  **Create a `.env` file:** This file will hold sensitive information.
     ```text
     POSTGRES_PASSWORD=<your_postgres_password>
     ```
3.  **Build and Run the Application:**
    ```bash
    docker-compose up --build -d
    ```

    This command will build the necessary Docker images and start the application in detached mode.

4. **Access the application:**
    Access the web application in the browser, on http://localhost:5000
    
5.  **Scan for Vulnerabilities**
    ```bash
      docker scout cves <image_name>
    ```

    This command will scan the image for any vulnerabilities that exist.

## Implementation Details

### Dockerfiles

*   **`./web/Dockerfile`:** This Dockerfile builds the image for the Flask application.
    ```dockerfile
    FROM python:3.11-slim-buster
    
    WORKDIR /app
    
    COPY requirements.txt .
    
    RUN pip install -r requirements.txt
    
    COPY . .
    
    CMD ["python","app.py"]
    ```
*  **`./Dockerfile`:** This Dockerfile builds the image for the database. This docker file is not actually needed as it uses the official `postgres` image.
   ```dockerfile
    FROM postgres:latest
    ENV POSTGRES_PASSWORD mysecretpassword
    ```

### `docker-compose.yml`
The `docker-compose.yml` file defines the services for the web app and database, the port mapping, dependencies, and other environment variables.
```yaml
version: '3.8'
    
services:
    web:
        build: ./web
        ports:
            - "5000:5000"
        depends_on:
            - db
        environment:
            - DATABASE_URL=postgresql://postgres:${POSTGRES_PASSWORD}@db:5432/mydatabase
    db:
       image: postgres:latest
       environment:
            - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
Use code with caution.
Markdown
The database is exposed internally on port 5432 to the web app container.

The web application is exposed on port 5000.

Security
Docker Scout was used to scan images for vulnerabilities.

Sensitive data like passwords were stored in a .env file instead of directly in the Dockerfile.

I have used slim versions of base images, to minimize the attack vector and startup time of the containers.

Optimizations
I have used slim versions of images wherever applicable to reduce the size of the images.

I have used layer caching to ensure that the building process is fast.

What I Learned
Docker Compose Mastery: How to use Docker Compose for multi-container setups.

Docker Networking: Understood how containers communicate with each other through networking.

Security Best Practices: Deepened my knowledge about securing Docker images.

Optimization: Learned about how to reduce image size and reduce the startup time for the containers.

Improvements
Add more detailed error handling and logging.

Implement a more robust security policy.

Automate security scanning and vulnerability patching.

Explore multi-stage builds.

Implement persistent data volumes.

Contributing
Feel free to fork this repository and contribute! If you have suggestions, bug reports, or improvements, please submit them. I'm always looking to learn from the community!

Happy containerizing!
