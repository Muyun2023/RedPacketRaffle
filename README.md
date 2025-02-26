# RedPacketRaffle

Due to the confidentiality agreement, the code cannot be fully displayed. Thank you for your understanding!

## Local Setup Guide

### 1. Prepare Dependencies

Ensure the following services are available:

- **Database** (MySQL)
- **Redis**
- **RabbitMQ**

You can install them manually or use Docker scripts for a containerized setup.

### 2. Database Initialization

- Connect to the database.
- Import the `prize.sql` script to initialize the database.

### 3. Start Backend

Navigate to the backend directory and run:

```sh
mvn tomcat7:run
```

### 4. Start Frontend APIs

Navigate to the frontend directory and start the following Spring Boot applications:

- **API Module**: Start using Spring Boot.
- **Message Service (msg)**: Start using Spring Boot.
- **Swagger Documentation**: Access API documentation at `/doc.html`.

### 5. Deploy Nginx

You can set up **Nginx** either locally or using Docker. Once deployed, access the application to participate in the lottery.

---

## Server One-Click Deployment (Using Docker Compose)

### Prerequisites

#### 1. Prepare the Code

- Copy the packaged code to the server, or clone the repository directly using:
  ```sh
  git clone <repository_url>
  ```

#### 2. Install Docker

Ensure the server has **Docker** and **Docker Compose** installed.

---

### Deployment Steps

#### 1. Build Docker Images

Run the following commands in respective directories:

- **Backend:**
  ```sh
  cd backend
  mvn clean package docker:build
  ```
- **Frontend:**
  ```sh
  cd frontend
  mvn clean install -DskipTests
  ```
- **API Module:**
  ```sh
  cd api
  mvn docker:build
  ```
- **Message Service:**
  ```sh
  cd msg
  mvn docker:build
  ```

#### 2. Start and Initialize Services

##### Modify Configuration

- In `docker-compose.yml`, update the **MinIO external access address** with your server's IP.
- Do **not** modify other configurations, as they are for Docker's internal networking.

##### Start MySQL and MinIO

```sh
docker-compose up lottery-mysql lottery-minio
```

##### Initialize MySQL

- Use a database client to connect to MySQL (default port: **9007**).
- Create the **prize** database.
- Import the latest `prize_xxxx-xx-xx.sql` script.

##### Initialize MinIO

- Access MinIO Console on **port 9006**.
- Create a **bucket** named `prize`.
- Navigate to `Buckets -> prize`, under `Anonymous`, click **Add Access Rule**.
  - **Prefix**: `/`
  - **Access**: `ReadOnly` (otherwise, URL access will return 403 errors)

##### Restart Services

```sh
docker-compose up -d
```

(This will start all services in the background.)

---

## Access Information

| Service                | URL / Port                            | Default Credentials     |
| ---------------------- | ------------------------------------- | ----------------------- |
| **MinIO Console**      | `http://<server-ip>:9006`             | `minioadmin/minioadmin` |
| **MySQL**              | Port `9007`                           | -                       |
| **RabbitMQ Console**   | Port `9008`                           | -                       |
| **RabbitMQ Messaging** | Port `9009`                           | -                       |
| **Redis**              | Port `9010`                           | -                       |
| **Nginx**              | Port `9101`                           | -                       |
| **API Interface**      | Port `9102` (`/doc.html` for Swagger) | -                       |
| **Admin Panel**        | Port `9103`                           | -                       |

**Note:** Internal services like Redis can be configured to not expose external ports by using Docker’s internal networking.

---

This guide provides all the necessary steps for local and server deployment. Ensure all configurations are correctly set before running the services.

© Muyun Ji. Confidential and Proprietary. All Rights Reserved.


