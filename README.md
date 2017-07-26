# express-limit [![Build Status](https://travis-ci.org/Dallas62/express-limit.svg?branch=master)](https://travis-ci.org/Dallas62/express-limit)

*express-limit* is a small project that add rate limitations to your API.

## Installation

```
npm install --save express-limit
```

## Usage

```
const limit = require('express-limit').limit;

app.get('/api/users', limit({
    max:    5,        // 5 requests
    period: 60 * 1000 // per minute (60 seconds)
}), function(req, res) {
    res.status(200).json({});
});

```
 
 
## Options
 
 ```
 {
     max        = 60,                  // Maximum request per period
     period     = 60 * 1000,           // Period in milliseconds
     prefix     = 'rate-limit-',       // Prefix of the key
     status     = 429,                 // Status code in case of rate limit reached
     message    = 'Too many requests', // Message in case of rate limit reached
     identifier = request => {         // The identifier function/value of the key (IP by default, could be "req.user.id")
         return request.ip || request.ips ||          // Read from Default properties
                request.headers['x-forwarded-for'] || // Read from Headers
                request.connection.remoteAddress;     // Read from Connection / Socket
     },
     headers = {                       // Headers names
         remaining: 'X-RateLimit-Remaining',
         reset:     'X-RateLimit-Reset',
         limit:     'X-RateLimit-Limit'
     },
     store = new Store()               // The storage, default storage: in-memory
 }
 ```
 
## Available Stores

Actually, two stores have been made:

- InMemoryStore (default store, nothing to do)
```
const RateLimiter = require('express-limit').RateLimiter;
const InMemoryStore = require('express-limit').InMemoryStore;

const store = new InMemoryStore(client);

const limit = new RateLimiter({ 
    store: store
}).middleware;


app.get('/api/users', limit({
    max:    5,        // 5 requests
    period: 60 * 1000 // per minute (60 seconds)
}), function(req, res) {
    res.status(200).json({});
});

```
- RedisStore
```
const redis = require('redis');
const client = redis.createClient();

const RateLimiter = require('express-limit').RateLimiter;
const RedisStore = require('express-limit').RedisStore;

const store = new RedisStore(client);

const limit = new RateLimiter({ 
    store: store
}).middleware;


app.get('/api/users', limit({
    max:    5,        // 5 requests
    period: 60 * 1000 // per minute (60 seconds)
}), function(req, res) {
    res.status(200).json({});
});

```


[Keep in touch!](https://twitter.com/BorisTacyniak)
