# Docker Challenge

#Challenge 1

##Setup
To setup you must follow these steps:
1. In your challenge1folder, add a public folder inside of it. Inside of this folder you will add 2 new files, the first being an index.html file that contains your name and your student ID.
2. The secondd file that you will add into your publicfolder will be you dockerfile. You will have to have installed docker onto VSCode or whatever IDE you chose to use.
3. Once both files are created, input this into your dockerfile
   FROM nginx
   RUN rm /etc/nginx/conf.d/default.conf
   COPY . .
4. The FROM sets the base image that you would like to use.
5. RUN executes commands during the buildd process of a docker image.
6. Finally the COPY command copies files or folders in the directory specified into the docker root container.
7. Once all this has been implemented, you will need to run this command to build the docker image docker build -t dockerchallenge .
 <img width="593" alt="image" src="https://github.com/CryptoJoe33/docker-challenge-template/assets/117700080/2c2f9f85-05d2-45c2-b3ff-81b77baf5f12">
 
8. Once that is successful, time to run by executing this command docker run --name dockerchallengecontainer -p 5000:8080 -d dockerchallenge, which should return a unique hash that is an identifer for this image.
