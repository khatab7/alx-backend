# Redis and Node.js Learning Project

This project is designed to help you learn the fundamental concepts of Redis, Redis clients, and integrating Redis with Node.js applications. By the end of this project, you should be able to understand and explain the following concepts:

- Running a Redis server locally
- Interacting with Redis using simple operations
- Using a Redis client with Node.js for basic operations
- Storing hash values in Redis
- Dealing with asynchronous operations in Redis
- Using Kue as a queue system
- Building a basic Express app that interacts with a Redis server
- Building a basic Express app that interacts with both a Redis server and a queue

## Learning Objectives

At the end of this project, you will be able to:

1. **Run a Redis server on your machine**
    - Learn how to install and start a Redis server locally, either on your system or using a Docker container.

2. **Run simple operations with the Redis client**
    - Understand how to use Redis commands to set and retrieve values, as well as how to interact with Redis via the Redis CLI (Command-Line Interface).

3. **Use a Redis client with Node.js for basic operations**
    - Set up a Redis client in a Node.js project and execute basic Redis operations such as setting and getting key-value pairs.

4. **Store hash values in Redis**
    - Learn how to store and retrieve hashes in Redis, which allows you to store objects with multiple fields.

5. **Deal with async operations with Redis**
    - Understand how to handle asynchronous operations when interacting with Redis in a Node.js application using promises, async/await, or callbacks.

6. **Use Kue as a queue system**
    - Integrate Kue with Redis to create a queue system in Node.js. Learn how to manage jobs and process background tasks.

7. **Build a basic Express app interacting with a Redis server**
    - Develop a basic Express.js application that connects to a Redis server and uses it for caching, data storage, or other functionalities.

8. **Build a basic Express app interacting with a Redis server and queue**
    - Expand the Express app to interact with both a Redis server and a queue system (using Kue or similar) to handle background tasks.

## Prerequisites

Before you start this project, make sure you have the following:

- [Node.js](https://nodejs.org/) installed
- [Redis](https://redis.io/) installed and running locally, or access to a Redis instance
- Basic knowledge of JavaScript and Node.js

## Installation

1. **Install Redis on your machine**

   You can install Redis using a package manager or run it via Docker. Follow the instructions on the [official Redis website](https://redis.io/download).

   Alternatively, you can use Docker to run Redis:

   ```bash
   docker run --name redis -p 6379:6379 -d redis


Usage
1. Running Redis Server

To start a Redis server on your machine, run:

redis-server

If you're using Docker, make sure the Redis container is running.
2. Simple Redis Operations

Create a simple script to interact with Redis:

const redis = require('redis');
const client = redis.createClient();

client.on('connect', () => {
    console.log('Connected to Redis');
});

client.set('key', 'value', (err, reply) => {
    console.log(reply); // Should log "OK"
    client.get('key', (err, reply) => {
        console.log(reply); // Should log "value"
    });
});

3. Storing Hash Values in Redis

client.hset('user:1000', 'name', 'John', 'age', '30', redis.print);

client.hgetall('user:1000', (err, object) => {
    console.log(object); // { name: 'John', age: '30' }
});

4. Handling Async Operations with Redis

async function getData() {
    const value = await new Promise((resolve, reject) => {
        client.get('key', (err, reply) => {
            if (err) reject(err);
            resolve(reply);
        });
    });
    console.log(value); // Should log the value stored in Redis
}
getData();

5. Using Kue for Queue Management

const kue = require('kue');
const queue = kue.createQueue();

queue.create('email', {
    title: 'Welcome Email',
    to: 'user@example.com',
    template: 'welcome'
}).save();

queue.process('email', function(job, done) {
    console.log('Sending email to', job.data.to);
    done();
});

6. Building an Express App

const express = require('express');
const redis = require('redis');
const client = redis.createClient();
const app = express();

app.get('/', (req, res) => {
    client.get('key', (err, reply) => {
        res.send(reply);
    });
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});

7. Express App with Redis and Kue Queue

const express = require('express');
const redis = require('redis');
const kue = require('kue');
const client = redis.createClient();
const queue = kue.createQueue();
const app = express();

app.get('/send-email', (req, res) => {
    queue.create('email', {
        title: 'Sending Email',
        to: 'user@example.com'
    }).save();

    res.send('Email job created!');
});

queue.process('email', (job, done) => {
    console.log('Sending email to', job.data.to);
    done();
});

app.listen(3000, () => {
    console.log('App running on port 3000');
});
