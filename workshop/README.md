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

docker exec -it 7024f9959cb8 mysql -u root -psecret
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
; <<>> DiG 9.18.25 <<>> mysql
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 31687
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;mysql.                         IN      A

;; ANSWER SECTION:
mysql.                  600     IN      A       172.18.0.2

;; Query time: 0 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Tue Apr 15 19:45:45 UTC 2025
;; MSG SIZE  rcvd: 44
exit
docker run -d -p 192.168.2.183:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node: slim \
  sh -c "yarn install && yarn run dev"
docker logs -f <container-id>
docker logs -f fbcb7241e74eaac7d0f6664ac7d45b0264d52da3af21fd6fc8289e61f3fd842f
nodemon src/index.js
[nodemon] 2.0.20
[nodemon] to restart at any time, enter `rs`
[nodemon] watching dir(s): *.*
[nodemon] starting `node src/index.js`
Connected to mysql db at host mysql
Listening on port 3000

docker exec -it 7024f9959cb8 mysql -p todos
use todos;
select * from todo_items;
+--------------------------------------+----------+-----------+
| id                                   | name     | completed |
+--------------------------------------+----------+-----------+
| 3b83f50e-e3e4-4c2c-b5f4-71a94804f63b | Kimchi   |         1 |
| 85fa18c7-4271-4794-804b-f35eeac2e344 | Kombucha |         0 |
+--------------------------------------+----------+-----------+
2 rows in set (0.00 sec)
exit;

sudo nano compose.yaml
services:
  app:
    image: node:slim
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 192.168.2.183:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
  
docker compose logs -f
docker exec -it 47c224f69b48 mysql -u root -psecret
docker compose down --volumes