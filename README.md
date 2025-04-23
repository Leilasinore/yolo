# Overview
This project involved the containerization and deployment of a full-stack yolo application using Docker.This application has a frontend,a backend and a database all in 3 different docker containers and managed using a docker compose file. 

NB: For a detailed explanation of the implementation details refer to the explanation.md file in the root of this project.


# Requirements for running this application
Install the docker engine here:
- [Docker](https://docs.docker.com/engine/install/) 

Install git 
```bash
$ sudo apt install git-all
```

# Steps for running the application
Step 1:cloning the repository
   To clone this repo run this command in your terminal then cd to the project folder
```bash
git clone https://github.com/Leilasinore/yolo.git
```
Step 2:Running the application
   To run the application run the command below and the application will be up and running in the port 3000
```bash
 docker-compose up
```



