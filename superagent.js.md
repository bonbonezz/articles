# Superagent.js

So, the superagent.js library is worth to be looked at. Here is why:


### Simple, lightweight, explicit.
####
##### Simple GET example
####
```javascript
import request from 'superagent';

request
  .get('/url')
  .end((err, res) => {
    // sending JSON object to console
    console.log(res.body)
  })
```
##### Simple POST example
####

```javascript
import request from 'superagent';

request
  .post('/url')
  .send({ param: "value" })
  .end((err, res) => {
    // callback
  });
```

### Power of plugins
####
##### Caching
####
1. Simple example
```javascript
var request = require('superagent-cache')();

new Array(5).forEach(() => {
  request
    .get('/whatever')
    .end((err, res) => {
      console.log(res.body)
    })
});

/*
As the result, there would be
- 5 logs in console
- 1 request to server
*/
```

2. Setting expiration timeout
```javascript
var request = require('superagent-cache')();
var timesToLoop = 5;

function doRequest() {
  request
    .get('/whatever')
    .expiration(3) // in seconds
    .end(() => {
      // callback
    })
}

function loop() {
  setTimeout(() => {
    doRequest();
    timesToLoop -= 1;
    if (timesToLoop > 0) {
      loop();
    }
  }, 1000);
}

/*
In this script we are calling the doRequest method 5 times, 1 call per second.
As the result, we will have:
- 5 calls total
- 2 of them will make a real request to server
- 3 of them will return saved value from cache
*/
```

##### Testing
####
```javascript
let request = require('supertest');

// Testing the request itself
request
  .get('/whatever')
  .expect(200)
  .expect('Content-Type', /json/)
  .end((err, res) => {
    if (err) {
      throw new err;
    }
  });
  
// Testing the request data
request
  .get('/whatever')
  .expect(200)
  .expect(function(res){
    if (res.body && res.body.something === null) {
      throw new Error('Something is missing!');
    }
  })
  .end(done)
```

##### Promises
####
We can use familiar promises model
```javascript
let superagent = require('superagent');
let request    = require('superagent-promise')(superagent, window.Promise);

request
  .get('/whatever')
  .then(
  // success
  (res) => {
    // handling successful response
  },
  // error
  (err) => {
    // handling request error
  })
```

And with the power of ES7 we can do something like this:
```javascript

async function doRequest() {
  var res = await request
    .get('/whatever')
    .then((res) => {
      var cloned = _.clone(res);
      // do something with res
      return cloned;
    })
  return result;
}

async function main() {
  var requestResult = await doRequest();
  
  // do something with data
  console.log(requestResult);
}

```

##### And a lot of interesting plugins here https://nodejsmodules.org/tags/superagent
##
##
### And finally, it works great with webpack
##
```javascript
// package.json
{
  "dependencies" : {
    "superagent" : "*"
  }
}

// entrance.js
import request from 'superagent';

request
  .get('/whatever')
  .end(() => {
    // callback
  });
```
