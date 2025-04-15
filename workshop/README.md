# Getting started

This repository is a sample application for users following the getting started guide at https://docs.docker.com/get-started/.

The application is based on the application from the getting started tutorial at https://github.com/docker/getting-started

***Run Docker Workshop***
sudo mkdir -p /opt/stacks/getting-started
git clone https://github.com/docker/getting-started-app.git
cd getting-started
cd sudo nano Dockerfile
# syntax=docker/dockerfile:1

FROM node:lts-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000

docker build -t getting-started .
docker run -d -p 192.168.2.183:3000:3000 getting-started
docker ps

-To update:
Edit src/static/js/app.js
- <p className="text-center">No items yet! Add one above!</p>
+ <p className="text-center">You have no todo items yet! Add one above!</p>
docker build -t getting-started .
docker run -dp 192.168.2.183:3000:3000 getting-started
-Error:
docker ps
docker stop <the-container-id>
docker rm <the-container-id>
docker run -dp 192.168.2.183:3000:3000 getting-started
-To publish:
docker login -u jadux
docker tag getting-started jadux/getting-started
docker push jadux/getting-started
-Create a persistent DB via Mount:
docker volume create todo-db
docker stop <the-container-id>
docker rm <the-container-id>
docker run -dp 192.168.2.183:3000:3000 --mount type=volume,src=todo-db,target=//etc/todos getting-started
-Inspect the volume:
docker volume inspect todo-db
-The data should be in the following folder - /var/lib/docker/volumes/todo-db/_data
-Create a persistent DB via Binds:
cd /path-to-getting started
docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu bash
pwd
- Should be root, run ls to check directory
cd src
ls
cd get-started
touch myfile.txt
ls
-Delete from host, and test
-Development Test:
docker run -dp 192.168.2.183:3000:3000 \
    -w /app --mount type=bind,src="$(pwd)",target=/app \
    node:18-alpine \
    sh -c "yarn install && yarn run dev"

docker logs -f <container-id>
nodemon -L src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node src/index.js`
Using sqlite database at /etc/todos/todo.db
Listening on port 3000

-Ctrl + C
nano src/static/js/app.js
- {submitting ? 'Adding...' : 'Add Item'}
+ {submitting ? 'Adding...' : 'Add Stuff'}
-Test Changes
docker ps
docker stop <container-id>
docker build -t getting-started .
-Multi-Container Apps and Docker Networking
docker network create todo-app
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:8.0

docker exec -it 9c8cf63ff31fa42aecf73edc3f85b7acf04c4438a5f2da903b3937129f54d23e mysql -u root -psecret
SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| todos              |
+--------------------+
5 rows in set (0.00 sec)
-Connect and Test mysql
docker run -it --network todo-app nicolaka/netshoot
dig mysql