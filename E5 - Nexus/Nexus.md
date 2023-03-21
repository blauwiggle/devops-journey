## Exercise 1: Install Nexus on a server

A Nexus instance is running on an EC2 Instance under `3.74.45.223:8081`. 

Followed this steps: https://hub.docker.com/r/sonatype/nexus3/#persistent-data

```bash
$ docker volume create --name nexus-data
$ docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```
## Exercise 2: Create npm hosted repository

1. Log in to the Nexus Repository Manager. 
2. Go to "Server administration and configuration" (gear icon). 
3. Click "Repositories" and click the "Create repository" button. 
4. Select "npm (hosted)". 
5. Enter a name for the repository and create a new blob store for it. 
6. Click "Create repository".

## Exercise 3: Create user for team 1

1. Go to "Server administration and configuration" (gear icon). 
2. Click "Users" and click the "Create local user" button. 
3. Fill in the required fields (username, first name, last name, email, password). 
4. Assign appropriate roles for accessing the npm repository (e.g., nx-repository-view-npm-*). 
5. Click "Create local user".

## Exercise 4: Build and publish npm tar

1. Open the terminal/command prompt and navigate to the root folder of your Node.js project. 
2. Run the following command to build and publish the npm package:

```bash
npm publish --registry=http://3.74.45.223:8081/repository/journey/ 
```

## Exercise 5: Create maven hosted repository

1. Log in to the Nexus Repository Manager. 
2. Go to "Server administration and configuration" (gear icon). 
3. Click "Repositories" and click the "Create repository" button. 
4. Select "maven2 (hosted)". 
5. Enter a name for the repository and create a new blob store for it. 
6. Click "Create repository".

## Exercise 6: Create user for team 2

1. Go to "Server administration and configuration" (gear icon). 
2. Click "Users" and click the "Create local user" button. 
3. Fill in the required fields (username, first name, last name, email, password). 
4. Assign appropriate roles for accessing the maven repository (e.g., nx-repository-view-maven2-*). 
5. Click "Create local user".

## Exercise 7: Build and publish jar file

1. Open the terminal/command prompt and navigate to the root folder of your Java Gradle project. 
2. Update the build.gradle file to configure the Maven repository URL and credentials. 
3. Run the following command to build and publish the jar file:

```bash
./gradlew publish
```

## Exercise 8: Download from Nexus and start the application

1. Create a new user for the droplet server that has access to both repositories. 
2. SSH into your DigitalOcean droplet. 
3. Install curl, jq, and wget tools if they are not already installed:
```bash
apt update && apt install -y curl jq wget
```
4. Use the following command to fetch the download URL for the latest Node.js app artifact:
```bash
curl -u {user}:{password} -X GET 'http://{nexus-ip}:8081/service/rest/v1/components?repository={node-repo}&sort=version'
```
5. Untar the artifact and run the application on the server.

## Exercise 9: Automate

1. Create a script to fetch the latest version from the npm repository, untar it, and run it on the server. Use the hints provided in the exercise description.
2. Execute the script on the DigitalOcean droplet.
