# Docker Challenge

## Challenge 1

### Setup
To setup you must follow these steps:
1. In your challenge1folder, add a public folder inside of it. Inside of this folder you will add 2 new files, the first being an `index.html` file that contains your name and your student ID.
2. The secondd file that you will add into your publicfolder will be you `dockerfile`. You will have to have installed docker onto VSCode or whatever IDE you chose to use.
3. Once both files are created, input this into your dockerfile
 ```
FROM nginx
RUN rm /etc/nginx/conf.d/default.conf
COPY . .
```
5. The `FROM` sets the base image that you would like to use.
6. `RUN` executes commands during the buildd process of a docker image.
7. Finally the `COPY` command copies files or folders in the directory specified into the docker root container.
8. Once all this has been implemented, you will need to run this command to build the `docker image docker build -t dockerchallenge .`
 <img width="593" alt="image" src="https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/2c2f9f85-05d2-45c2-b3ff-81b77baf5f12">
 
9. Once that is successful, time to run by executing this command `docker run --name dockerchallengecontainer -p 5000:8080 -d dockerchallenge`, which should return a unique hash that is an identifer for this image.

## Challenge 2

### Setup
To setup challenge 2 you must follow these steps:
1. In VSCode you will have to create you Dockerfile with these specifications

```
# Use an official Node.js runtime as the base image
FROM node:14

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install the application dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run the app
CMD [ "node", "server.js" ]
```

2. You will then have to create the docker-compose.yml file which is used for simplifying the setup of multiple containers and makes your setup reusable. It should look something like this
```
version: '3.8'
services:
 nginx:
    image: nginx:latest
    ports:
      - '8080:80'
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - api

 api:
    build: .
    ports:
      - '3000:3000'
```
3. You will then have to create your final file nam,ed nginx.conf which is used for which is used to setup NGinx as a reverse proxy (Is a server that sits in front of another server, where the client talks to the first server before touching the second. This contrasts traditional connection as the client does not talk directly to the web server), and it looks something like this
```
server {
    listen 80;

    location /api/ {
        proxy_pass http://api:3000/api/;
    }
}
```
4. To run your container you will have to run `docker-compose up --build` which yields this as a result
<img width="759" alt="image" src="https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/66ce1411-a031-45ed-8cbf-94f81a5f99bf">

6. Once no errors occur, you search for the site by typing `http://localhost:8080/api/books/` into your browser which should look like this in your browser:
<img width="764" alt="image" src="https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/52e8fe48-caca-402d-8177-14fac4d01690">

## Challenge 3

1. You will have to create 2 new files. The first of which is the ENV file which houses all the necessary variables that will be used in the docker-compose file. This is what it looks like, and you should replace all the variables with your own made up data.
```
DB_ROOT_PASSWORD=<placeholder>
DB_DATABASE=<placeholder>
DB_USERNAME=<placeholder>
DB_PASSWORD=<placeholder>
DB_HOST=<placeholder>

MYSQL_ROOT_PASSWORD=<placeholder>
MYSQL_DATABASE=<placeholder>
MYSQL_USER=<placeholder>
MYSQL_PASSWORD=<placeholder>
MYSQL_HOST=<placeholder>
```
2. You will then need to create a docker-compose file which should consist of the following:
```
version: '3.8'

services:
  nginx:
    build: ./nginx
    ports:
      - "8080:80"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    depends_on:
      - node-service

  node-service:
    build: ./api
    environment:
      - "DB_ROOT_PASSWORD=${DB_ROOT_PASSWORD}"
      - "DB_DATABASE=${DB_DATABASE}"
      - "DB_USERNAME=${DB_USERNAME}"
      - "DB_PASSWORD=${DB_PASSWORD}"
      - "DB_HOST=${DB_HOST}"
    depends_on:
      - db
    deploy: 
      replicas: 3

  db:
    build: ./db
    environment:
      - "MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}"
      - "MYSQL_DATABASE=${MYSQL_DATABASE}"
      - "MYSQL_USER=${MYSQL_USER}"
      - "MYSQL_PASSWORD=${MYSQL_PASSWORD}"
      - "MYSQL_HOST=${MYSQL_HOST}"
    ports:
      - "3307:3306"
    volumes:
      - db-data:/var/lib/mysql
    command: --default-authentication-plugin=mysql_native_password

volumes:
  db-data:
```
This docker compose file defines three services, node-ervice, nginx and a database server. It orchestreates these services to work together as a multi-container application environment and how the different services work togther.

3. To build and run this compose file use the command `docker compose up` to get it started. If successful the page should look like the image provided below and this URL `http://localhost:8080/api/books/` should result in the output below.

![image](https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/d920069c-32a7-40cb-aaa0-596b6650512f)


![image](https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/2a94139c-cb7f-4e4e-bf00-4aa8690db05c)

## Challenge 4

1. Before you make any changes to the scaling of your node-services you should note down the hostname, which should consistently be the same. To find this, use the URL `http://localhost:8080/api/stats`. In this case it is `896ecab7032f` 

![image](https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/6f574b3a-2f6c-43a6-a8f6-ccc6fa8b0dae)

2. Once noted down you just need to simply use this command in your terminal to scale your node-service from 1-3. `docker-compose up --scale node-service=3`
3. This should run you docker files normally except with a few more instances of your container. If you route back to the `/api/stats` endpoint again you will notice that the hostname is different most of the times that you route back to it.

![image](https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/7aa5bf91-d989-40e9-9bac-24283fcae5a6)
![image](https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/1cc605a6-fb84-47fd-b6ae-b78ac2063b30)

4. To double check that you have 3 node-services running you can use the command `docker compose ps` which lists all teh containers for a compose project.

![image](https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/914837de-fb6b-40c8-bcaa-ae322eb6f874)

