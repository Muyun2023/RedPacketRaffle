# RaffleRush

#### Local Setup Guide

1. Prepare Dependencies: Ensure the database, Redis, and RabbitMQ are ready. You can install them manually or use Docker scripts for a containerized setup.

2. Database Initialization:
Connect to the database.
Import the prize.sql script to initialize the database.
3. Start Backend: Navigate to the backend directory and run mvn tomcat7:run to start the backend.

4. Start Frontend APIs: Navigate to the frontend directory and start the following Spring Boot applications:
API: Use Spring Boot to start the api module.
Message Service (msg): Use Spring Boot to start the msg module.
Access Swagger: Visit /doc.html in the API to access the Swagger documentation.

5. Deploy Nginx: You can set up Nginx either locally or using Docker. After deployment, access the application to participate in the lottery.



#### Server One-Click Deployment (Using Docker Compose)

In the deploy folder of the project, a Docker Compose file is provided for one-click deployment.

Prerequisites:
Prepare the Code:
Copy the packaged code to the server, or clone the repository directly from the server using git clone.
Install Docker: Ensure the server has Docker and Docker Compose installed.

Deployment Steps:
Build Docker Images:

Navigate to the backend directory:
Run mvn clean package docker:build to compile and package the backend.
Navigate to the frontend directory:
Run mvn clean install -DskipTests for a full build.
Navigate to the api directory:
Run mvn docker:build to package the API module.
Navigate to the msg directory:
Run mvn docker:build to package the message service.
Start and Initialize Services:

Navigate to the deploy directory.

Configuration: Modify only one configuration setting in docker-compose.yml:

Update the external access address for MinIO with your server's IP address.
Do not modify other configurations, as they are for Docker's internal networking.
Start MySQL and MinIO:
Run docker-compose up lottery-mysql lottery-minio.

Initialize MySQL:

Use a database client to connect to MySQL (default port: 9007).
Create the prize database.
Import the latest prize_xxxx-xx-xx.sql script into the prize database.
Initialize MinIO:

Access MinIO Console on port 9006.
Create a bucket named prize.
In the Buckets -> prize bucket under Anonymous, click "Add Access Rule".
Set the Prefix to /.
Set Access to ReadOnly (otherwise, URL access will return 403 errors).
Restart Services:
Run docker-compose up -d to start all services in the background.


##### Access

MinIO:
Console: http://<server-ip>:9006
Default credentials: minioadmin/minioadmin
MySQL: Port 9007
RabbitMQ Console: Port 9008
RabbitMQ Messaging: Port 9009
Redis: Port 9010
Nginx: Port 9101
API Interface: Port 9102, visit /doc.html for Swagger documentation
Admin Panel: Port 9103

Note: Internal services like Redis can be configured to not expose external ports by using Docker's internal networking.
