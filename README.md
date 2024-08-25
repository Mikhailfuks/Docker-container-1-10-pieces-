
FROM mysql:latest

ENV MYSQL_ROOT_PASSWORD mypassword
ENV MYSQL_DATABASE mydatabase
ENV MYSQL_USER myuser

docker network create mynetwork


docker run -d -p 80:80 --network mynetwork --name nginx-container nginx-example

docker run -d -p 3306:3306 --network mynetwork --name mysql-container mysql-db


FROM nginx:latest

COPY index.html /usr/share/nginx/html/


<!DOCTYPE html>
<html>
<head>
  <title>Docker Nginx Example</title>
</head>
<body>
  <h1>Hello from Docker Nginx!</h1>
</body>
</html>


# Построение образа
docker build -t nginx-example .

# Запуск контейнера
docker run -d -p 80:80 nginx-example


tasks 2 

const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello from Node.js Express!');
});

app.listen(port, () => {
  console.log(Server running on port ${port});
});

FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY FILE 

CMD ["npm", "start"]

# Построение образа
docker build -t node-api .

# Запуск контейнера
docker run -d -p 3000:3000 node-api

tasks 3

FROM postgres:latest

ENV POSTGRES_USER myuser
ENV POSTGRES_PASSWORD mypassword
ENV POSTGRES_DB mydatabase

# Построение образа
docker build -t postgres-db .

# Запуск контейнера
docker run -d -p 5432:5432 --name postgres-db postgres-db

tasks 4

FROM nginx:latest

FROM mysql:latest

ENV MYSQL_ROOT_PASSWORD mypassword
ENV MYSQL_DATABASE mydatabase
ENV MYSQL_USER myuser
docker network create mynetwork

docker run -d -p 80:80 --network mynetwork --name nginx-container nginx-example

docker run -d -p 3306:3306 --network mynetwork --name mysql-container mysql-db

FROM nginx:latest


FROM mysql:latest

ENV MYSQL_ROOT_PASSWORD mypassword
ENV MYSQL_DATABASE mydatabase
ENV MYSQL_USER myuser

version: '3.7'

services:
  nginx:
    build: ./nginx
    ports:
      - "80:80"
    depends_on:
      - mysql
    networks:
      - mynetwork

  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: mypassword
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: myuser
    ports:
      - "3306:3306"
    networks:
      - mynetwork

networks:
  mynetwork:
    driver: bridge

    docker-compose up -d

    from flask import Flask

    app = Flask(__name__)
    
    @app.route('/')
    def index():
      return 'Hello from Flask!'
    
    if __name__ == '__main__':
      app.run(debug=True, host='0.0.0.0')
    


      FROM python:3.9

      WORKDIR /app
      
      COPY requirements.txt ./
      
      RUN pip install -r requirements.txt
      
      COPY FILE 
      
      CMD ["python", "app.py"]
      
tasks 8


const express = require('express');
const redis = require('redis');
const app = express();
const port = process.env.PORT || 3000;

const client = redis.createClient({
  host: 'redis',
  port: 6379
});

client.on('error', (err) => console.log('Redis Client Error', err));

app.get('/', async (req, res) => {
  try {
    const cachedData = await client.get('myData');
    if (cachedData) {
      res.send(Data from cache: ${cachedData});
    } else {
      const data = 'Hello from Node.js with Redis!';
      await client.set('myData', data);
      res.send(Data saved to cache: ${data});
    }
  } catch (error) {
    console.error(error);
    res.status(500).send('Internal server error');
  }
});

app.listen(port, () => {
  console.log(Server running on port ${port});
});


FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY FILE 

CMD ["npm", "start"]

FROM redis:latest


version: '3.7'

services:
  redis:
    image: redis:latest
    ports:
      - "6379:6379"

  node-app:
    build: ./node-app
    ports:
      - "3000:3000"
    depends_on:
      - redis

      docker-compose up -d

tasks 9
      const mongoose = require('mongoose');
      const app = express();
      const port = process.env.PORT || 3000;
      
      mongoose.connect('mongodb://mongo:27017/mydb')
        .then(() => console.log('Connected to MongoDB'))
        .catch(err => console.error('Error connecting to MongoDB', err));
      
      const Schema = mongoose.Schema;
      
      const mySchema = new Schema({
        name: String,
        age: Number
      });
      
      const MyModel = mongoose.model('MyModel', mySchema);
      
      app.get('/', async (req, res) => {
        try {
          const data = await MyModel.find();
          res.json(data);
        } catch (error) {
          console.error(error);
          res.status(500).send('Internal server error');
        }
      });
      
      app.listen(port, () => {
        console.log(Server running on port ${port});
      });
      


      FROM node:16

      WORKDIR /app
      
      COPY package*.json ./
      
      RUN npm install
      
      COPY FILE
      
      CMD ["npm", "start"]
      
      FROM mongo:latest

      version: '3.7'

      services:
        mongo:
          image: mongo:latest
          ports:
            - "27017:27017"
      
        node-app:
          build: ./node-app
          ports:
            - "3000:3000"
          depends_on:
            - mongo
      
            docker-compose up -d

tasks 10

FROM python:3.9-slim AS build

WORKDIR /app

COPY requirements.txt ./

RUN pip install -r requirements.txt

COPY FILE 
FROM nginx:latest

COPY --from=build /app /usr/share/nginx/html

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]


server {
  listen 80;
  location / {
    root /usr/share/nginx/html;
  }
}

docker build -t flask-nginx .
docker run -d -p 80:80 flask-nginx
