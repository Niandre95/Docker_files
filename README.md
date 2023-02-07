# Working with Docker
### Docker Images and Dockerfiles
-A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries, and settings.
-A Dockerfile is a text document that contains all the steps needed to build a Docker image

#### Software installation
--If you already have a docker hub account and have a docker engine download on your local machine; you can skip this step.
-Creating a dockerhub account to store and manage your docker images:
---Create a dockerhub account: https://hub.docker.com/
---Install a docker engine: https://docs.docker.com/get-docker/
Make sure you follow the steps from both sites correctly. I am going to use the centos7 machine to run my docker images and I used the same machine to create the dockerfiles for these images.

-Once the installation is done: Now you can work with Docker
--But, before I start working with Docker; I need to create a user that's going to manage my dockerfiles on my local machine because I don't want to use the root user for security concerns.
--Again, I am working with a centos7 Linux machine
-Let's create a new user and add the user to the docker group.
--Command: $ sudo useradd student -G docker
-Set the password for the user:
--Command: $ sudo passwd student : Enter the password for the user.
--Once the user is created and we need to switch to the user
--Command: $ sudo su student
-Run some useful docker commands to understand a little more about Docker.
--Command: $ docker version
--Command: $ docker info
--Command: $ docker images
I am going to skip the part about working with Docker containers for now because we are here to learn how to create a Dockerfile and turn it into a Docker image.

### Creating a Dockerfiles
#### Folder: Docker_image
--This folder contains two files a Dockerfile and an index.html file. The Dockerfile contains the steps to build a docker image and the index.html file contains a custom message.
--This is a Dockerfile for httpd
-In the student home directory: Create a Dockerfile
--Command: $ cd ~
--Command: $ touch Dockerfile  :make sure the D is capitalize
-open the file
--Command: $ vim Dockerfile
--Copy my Dockerfile contents from the folder and paste them into the file that you just create it.
-Viewing the contents of the Dockerfile:
--FROM httpd:2.4    : Building my image based on httpd version 2.4
--RUN apt-get update     : Executing the apt-get update command
--COPY ./index.html /usr/local/apache2/htdocs   : copying my index.html this path so the custom message can be displayed.
This is a custom httpd version 2.4 image that displays a special message when you connect to it.
-Creating the index.html file
--Command: $ vim index.html
Copy the content from the index.html file and paste it into the file.
--File content: <h1>This is my Docker image of httpd and you are connected</h1>
Once you are done creating both files: now it's time to create our Docker image
-In the terminal, run this command
--Command: $ docker build -t studen-httpd:1.0 .
-Remember to put a dot at the end of the command. This tells the docker build to look for the Dockerfile in the current directory. student-httpd:1.0 is my image name. Feel free to give a name of your choice.
-Push a Docker Image
-Login into your docker hub account through the terminal
--Command: $ docker login : Enter the username and the password of your docker account if necessary.
--Now we can use the docker tag command to create a tag target_image that refers to the source_image.
--Command: $ docker tag YourimageName yourRepositoryName/imageName: tag
Your repository name can be any name, I just use something similar to the username of my Docker hub account.
--Command: $ docker tag student-httpd:1.0 andrew5/niandre-httpd:1.0
-If you run "docker images", you can see a new image in your local repository.
-Now, let's push the image to the docker hub.
--Command: $ docker push username/imageName: tag
--Command: $ docker push andrew20/niandre-httpd:1.0
-When you are done, you can open your docker hub account. Then click on the repository and you should see the new image.
--Now we just created an httpd docker image with a custom message that displays when we are connected to it.
#### Folder: Docker_image_from_Scratch
This folder contains two files, a Dockerfile from scratch and an index.html
-Create a directory in any user on your system and make sure the user is part of the docker group
-Use this command to add the user to a docker group
--Command: $ usermod -G docker userName
-Create a new Dockerfile and copy the content from my dockerfile. Before moving to the next step, make sure to explore the Dockerfile contents.
-Dockerfile contents: I am installing an httpd service and I want this service to run on port 80. command: CMD apachectl -D FOREGROUND: allow the container to exit when creating it.
-Create a custom message with index.html.
--Command: $ vim index.html : copy the content from my file to here.
-Build the image
--Command: $ docker build -t andre/web-server:1.5 .
-Push the image to the docker hub
--Command: $ docker push andrew20/web-server:1.5
-Now, you can see the image in the docker hub console.
#### Docker_Compose_file
This Docker Compose file contains a YAML file for a multi-container application. The file uses two images (WordPress and MariaDB) to deploy the application.
-A Docker compose file is not a replacement for Dockerfile because as an engineer, we need to figure out when to use a docker-compose file or dockerfile.
--Create a directory in any user's home directory and make sure the user is part of the docker group.
-Install docker-compose into our system
--Command:
$ sudo curl -L"https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
-Assign the execution permission:
--Command: $ sudo chmod +x /usr/local/bin/docker-compose
-Check the docker-compose version on your local machine
--Command: $ docker-compose --version
-Navigate to the directory that you created early and create a docker-compose.yml
--Command: $ vi docker-compose.yml
-Copy the content from my docker-compose.yml file and paste it into your file. Also, don't forget to explore the file.
-For this project docker-compose.yml file, I used two services and two images to build a WordPress site.
--Services: WordPress and MySQL
--Images: wordpress:4.9.4-php7.2-apache and mariadb:10.3.3
-Now, to launch the app, run the command:
--Command: $ docker-compose -up -d   (the -d option is for detached mode)
-Check the containers with:
--Command: $docker-compose ps
-Check the IP address to access the app from the browser
--Command: $ ip add show
-Don't forget to add the port number (3009) for the app from the docker-compose.yml file
-From here, you will get a WordPress configuration page and you can build your WordPress site from docker-compose.
-To shut down the whole application at once: Use this command
--Command: $ docker-compose down
#### Deploying_multi_Container_APP
This docker-compose file deploys a multi-container app with two applications communicating with each other: the voting-app and the result-app
-Create a folder in your home directory of the user who is part of the docker group
--Command:$ mkdir my-docker-compose
--Command:$ cd my-docker-compose
-Create a docker-compose.yml in the current directory
--Command:$ vi docker-compose.yml
-Copy the content from my docker-compose.yml file to your file and explore the file contents before moving to the next steps.
--In this project, I used 5 services and 5 images
## Services and Images
--vote : eesprit/voting-app-vote
--redis : redis:alpine
--worker : eesprit/voting-app-worker
--db : postgres:9.4
--result : eesprit/voting-app-result
-Now, start all the services from the yml file
--Command: $ docker-compose up
-Check to see if the app is working from your browser:
-- Your-IP:5000 for the vote and Your-IP:5001 for the results
-The application is a voting app between cats and dogs. Make your pick!
-To stop the application, use this command:
--Command: $ docker-compose up
