# Proof of Work and Full Report Below (CHALLENGE 1 & 2 ) 
- You can find the proof of work and the full report in the following link:

[Proof of Work and Full Report] (https://mysait-my.sharepoint.com/:w:/g/personal/godwinjb_mercado_edu_sait_ca/Eb-f3Bp5aN5Gg9ovQeHAK_QBUvkaf5lZBxJoO0IExp_IsQ?e=N2iEUl)

This repository has been created by Godwin Mercado, but the ownership of this content is retained by SAIT under Educational and Fair Use.

## CHALLENGE 3

![Screenshot 2024-04-22 170618](https://github.com/Goqwin/docker-challenge-template/assets/115321545/7ff362b5-bcb9-47e5-b46f-125e26861e23)



- Download dotenv 
- Setup your api routes and ensure that it's connected to the DB, close the pool connection
- Move your .env in the api, move your compose in the api, don't use "Quotes" on your .env


Clean up step:
Drop your container
Remove any unused images
docker-compose up 

Good signals: exit signal 1, all modules found, and the 3 main components, node-service, db, and nginx will be running upon execution.

See nginx.conf Ensure you set the backend and you create a proxy pass of the backend. 
 location /api {
            set $backend "http://loadbalancer";
            proxy_pass $backend;
        }

Files were changed: 
-.env 
- docker-compose.yml
- DockerFile (API)
- server.js (Connection pool -> open the connection and allow users to interact with the "backend" which will be in connecting with the database.)
- NGINX.CONF 
- npm install --dotenv-extended 

