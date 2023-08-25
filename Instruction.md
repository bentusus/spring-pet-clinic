**Clone the repo** 
Clone the project from the GitHub Repo; https://github.com/bentusus/spring-pet-clinic.git

**Configure Repo** 
Configure Repo to your own Git Repo. (Deleting .git/ file and reinitializing)

**Change directory** 
cd into terraform-file and initiate terrafom. (terraform init)
modify configurations in dev.auto.tfvars. 
(mykey = "enter your Keypair") Everything will remain the same in dev.auto.tfvars.

**AWS Credentials** 
Retrieve your AWS Access_key and Secret_Key from the console into the dev.server.tf file

**Initialize terraform** 
On your command terminal cd into the "terrafrom-file" directory and initialize terraform with `terraform init` command 

**Create the dev-server**
use the following command to create and provision server on your aws server 
`terraform plan`
`terraform apply --auto-approve`

**Connect to your ec2-Server**
Use your ssh client to connect to the dev-server on your AWS console 
ssh -i "Enter your keypair" Amazon-linux@ec2-34-238-189-73.compute-1.amazonaws.com

**configure token in gitHub**
Configure Token and put it in the link below to clone the petclinic repo on EC2 server
Git clone https://<your-github-token>@github.com/bentusus/microservices-with-db-on-dev-server.git

Cd into microservices-with-db-on-dev-server

create a git branch dev and do git checkout to switch to the Dev branch. Do git branch to confirm

**mvnw permission; Add execute permission**
In order not get a “permission denied” error, give execution permission to **mvnw**
Run chmod +x mvnw

**Connect to the Development server using VS code**
Connect to the Development server with Vscode to work more easily.

**Building Source Code using Maven**
 Test the compiled source code, the commands need to be executed inside the project folder. 
 use the command below;
 ./mvnw clean test

 Install distributable “JAR”s into the local repository (in .m2 folder). Convert the compiled source code to .Jar (It is an artifact file.). For this, run the command below;
 ./mvnw clean install

 Jar files of microservices apps were created into .m2 folder and target folder, after the “./mvnw clean install” command ran.

 **Prepare Dockerfiles for Microservices**
 Prepare a Dockerfile for the “admin-server” microservice with the following content and save it under “spring-petclinic-admin-server”
 Create a Dockerfile in the "Spring-petclinic" Folders below with their respective port numbers using the script provided below.
  admin-server PORT=9090
  api-gateway Port=8080
  config-server port=8888
  customer-service Port=8081
  discovery-server Port=8761
  hystrix-dashboard Port=7979
  vets-service Port=8083
  visits-service Port=8082

**Script**
FROM openjdk:11-jre
ARG DOCKERIZE_VERSION=v0.6.1
ARG EXPOSED_PORT=[Enter Port Number]
ENV SPRING_PROFILES_ACTIVE docker,mysql
ADD https://github.com/jwilder/dockerize/releases/download/${DOCKERIZE_VERSION}/dockerize-alpine-linux-amd64-${DOCKERIZE_VERSION}.tar.gz dockerize.tar.gz
RUN tar -xzf dockerize.tar.gz
RUN chmod +x dockerize
ADD ./target/*.jar /app.jar
EXPOSE ${EXPOSED_PORT}
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]

**Note**
For each Dockerfile you create in each spring pet-clinic microservice, replace the Port number with the information provided above.

**Preparing Script for Building Docker Images**
Give execution permission to “build-dev-docker-images.sh” using the command below
chmod +x build-dev-docker-images.sh

**Build Images**
Build Images Using command below;
./build-dev-docker-images.sh

**Preparing Docker Compose File**
Prepare a docker compose file to deploy the application locally and save it as “docker-compose-local.yml” under the “petclinic-microservices-with-db” folder.
In order to test the deployment of the app locally with “docker-compose-local.yml”, run the command below
docker-compose -f docker-compose-local.yml up.

**Observing what happens in the containers after the “docker-compose” command runs**
In order to check docker images that created by “docker-compose-local.yml”, run the command below;
docker images

**check springboot Vet page**
Go to your browser and enter “your development server IP:8080”. Api gateway’s port was “8080”. We should see the Springboot Vet page







