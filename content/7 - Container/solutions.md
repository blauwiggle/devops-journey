---
title: "Solutions"
weight: 2
---

## EXERCISE 1: Start Mysql container

1. Start the MySQL container using the official Docker image and set the required environment variables:

```bash
docker run --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=mydb -e MYSQL_USER=myuser -e MYSQL_PASSWORD=mypassword -d mysql:latest
```

2. Export the needed environment variables for the application to connect with the database:

```bash
export DB_USER=myuser
export DB_PWD=mypassword
export DB_SERVER=localhost
export DB_NAME=mydb
```

3. Build the JAR file and start the application. Test access from the browser and make some changes.

## EXERCISE 2: Start Mysql GUI container

1. Start the phpMyAdmin container using the official image:

```bash
docker run --name phpmyadmin -d --link mysql:db -p 8081:80 phpmyadmin/phpmyadmin
```
2. Access phpMyAdmin from the browser at http://localhost:8081 and test logging in the MySQL database using the 
credentials provided earlier.

## EXERCISE 3: Use docker-compose for Mysql and Phpmyadmin

1. Create a docker-compose.yml file with both containers:

```yaml
version: '3.9'

services:
  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
      MYSQL_DATABASE: mydb
      MYSQL_USER: myuser
      MYSQL_PASSWORD: mypassword
    volumes:
    - mysql-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    depends_on:
      - mysql
    ports:
      - 8081:80

volumes:
  mysql-data:
```

2. Configure a volume for the database in the docker-compose.yml file (as shown above). 
3. Test that everything works again by running docker-compose up -d and accessing phpMyAdmin at http://localhost:8081.

## EXERCISE 4: Dockerize your Java Application

```Dockerfile
FROM gradle:7.4.0-jdk17 AS build

WORKDIR /app

COPY . .

RUN gradle clean build
RUN gradle publish

FROM openjdk:17-slim

WORKDIR /app

COPY --from=build /app/build/libs/com.example-1.0-SNAPSHOT.jar /app/com.example-1.0-SNAPSHOT.jar

EXPOSE 8080

CMD ["java", "-jar", "/app/com.example-1.0-SNAPSHOT.jar"]
```

## EXERCISE 5: Build and push Java Application Docker Image

1. Create a Docker hosted repository on Nexus:
   + Log in to Nexus Repository Manager web interface. 
   + Click on the gear icon (Settings) in the top right corner. 
   + Select "Repositories" in the left sidebar. 
   + Click on the "Create repository" button in the top right corner. 
   + Choose "docker (hosted)". 
   + Fill in the required fields:
     - Name: Enter a name for the repository (e.g., my-docker-repo). 
     - HTTP: Check the "Enable" box and set a port number (e.g., 8123). 
     - Deployment policy: Choose "Allow redeploy" to allow overwriting existing images.
   + Click on "Create repository" to create a Docker hosted repository.
2. Build the Java application Docker image locally and push it to the Nexus repository:
   + Update the Dockerfile with the appropriate Java version and build tool (Maven or Gradle) as shown in the 
   previous answers. 
   + Log in to Docker hosted repository on Nexus using the Docker CLI:
   ```bash
   docker login -u username -p password 3.74.45.223:8081/repository/my-docker-repo:8123
   ```
    + Build the Docker image locally:
   ```bash
   docker build -t 3.74.45.223:8081/repository/my-docker-repo:8123/bootcamp:1.0 .
   ```
   + Push the Docker image to the Nexus repository:

    ```bash
   docker push 3.74.45.223:8081/repository/my-docker-repo:8123/bootcamp:1.0
    ```

## EXERCISE 6: Add application to docker-compose

```yaml
version: '3.9'

services:
  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    volumes:
    - mysql-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    depends_on:
      - mysql
    ports:
      - 8081:80

  java_app:
    image: 3.74.45.223:8081/repository/my-docker-repo:8123/bootcamp:1.0
    environment:
      DB_USER: ${DB_USER}
      DB_PWD: ${DB_PWD}
      DB_SERVER: ${DB_SERVER}
      DB_NAME: ${DB_NAME}
    ports:
      - 8080:8080
    depends_on:
      - mysql

volumes:
  mysql-data:
```

When deploying the application on the server, set the environment variables using the export command or by passing 
them directly to the docker-compose command. For example: 

```bash
MYSQL_ROOT_PASSWORD=my-secret-pw \
MYSQL_DATABASE=mydb \
MYSQL_USER=myuser \
MYSQL_PASSWORD=mypassword \
DB_USER=myuser \
DB_PWD=mypassword \
DB_SERVER=mysql \
DB_NAME=mydb \
docker-compose up -d
```

or with envsubst:

```bash
export MYSQL_ROOT_PASSWORD=my-secret-pw
export MYSQL_DATABASE=mydb
export MYSQL_USER=myuser
export MYSQL_PASSWORD=mypassword

export DB_USER=myuser
export DB_PWD=mypassword
export DB_SERVER=mysql
export DB_NAME=mydb

envsubst < docker-compose.yml | docker-compose -f - up -d
```

## EXERCISE 7: Run application on server with docker-compose

> Another approach to fix this is to use Caddy as a reverse proxy, together with something like nip.io. It's a 
simple one-liner.

1. Set insecure Docker repository on the server, because Nexus is using HTTP. Edit or create `/etc/docker/daemon.
json` on 
the server and add the following content: 

```bash
{
  "insecure-registries": ["3.74.45.223:8081"]
}
```

Then restart the Docker service:

```bash
sudo systemctl restart docker
```

2. Log in to the Docker registry on the server:

```bash
docker login 3.74.45.223:8081 -u username -p password
```

3. Modify the Dockerfile to include the repository and port when tagging and pushing the image. In the Dockerfile:

```Dockerfile
# Add these lines after building the image
ARG NEXUS_REPO=3.74.45.223:8081
ARG IMAGE_NAME=my-docker-repo
ARG IMAGE_VERSION=8123

# Replace the existing lines with these
FROM $NEXUS_REPO/$IMAGE_NAME:$IMAGE_VERSION AS final
```

4. Build the Docker image and push it to the Nexus repository:

 ```bash
docker build -t 3.74.45.223:8081/repository/my-docker-repo:8123 .
docker push 3.74.45.223:8081/repository/my-docker-repo:8123
```

5. Update the docker-compose.yml file to include the image I just pushed to the Nexus repository.

## EXERCISE 8: Open ports

1. Log in to the DigitalOcean account and navigate to the control panel. 
2. On the left sidebar, click on "Networking" and then select "Firewalls". 
3. Click on existing firewall to modify its rules. 
4. In the "Inbound Rules" section, add a new rule by selecting the protocol (TCP/UDP) and specifying the port number 
   or range. 
5. Update the existing firewall. 
6. To apply the firewall to the desired droplets, click on "Add Droplets" and select the droplets that need the 
   firewall rules. 
