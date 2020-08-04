# Logging Node.js Express HTTP Requests in a MongoDB Database Using mongoose-morgan npm package

Recently, I have started using Node.js with Express for all of my personal projects. As we all know, creating a software without any logging is a bad idea so I had to log data from morgan which is an HTTP request logger middleware for express to the database. I found some packages to do this kind of stuff but none of them suitable for me so I created my own.

The package is named **mongoose-morgan**, here is a [link](https://www.npmjs.com/package/mongoose-morgan).

**mongoose-morgan** is an express middleware which combines mongoose and morgan packages by adding an additional functionality to log morgan data into MongoDB.

## Features

mongoose-morgan uses [options](https://github.com/expressjs/morgan#options) and [formatting](https://github.com/expressjs/morgan#predefined-formats) features from morgan package. In order to find out what options and formatting features are make sure to check morgan [GitHub page](https://github.com/expressjs/morgan).

To save data in MongoDB I used mongoose package as one of the most commonly used.

## Usage

To install it just call:

```
npm install mongoose-morgan
```

## Basic usage example

Let's create a basic example of express app

```
// express
var express = require('express');
var app = express();

// mongoose-morgan
var mongooseMorgan = require('mongoose-morgan');

// connection-data
var port = process.env.port || 8080;

// Logger
app.use(mongooseMorgan({
    connectionString: 'mongodb://localhost:27017/logs-db'
}));

// run
app.listen(port);
console.log('works... ' + port);
```

In this example, we are providing only basic arguments to a mongoose-morgan package. The only requirement is to add a connection string to Mongo database. mongoose-morgan will use basic options and features for morgan logging.

It will create a Mongo collection named *logs* in a logs-db database and a type of formatting for morgan data will be *combined*.

## Detailed usage

The mongoose-morgan is accepting three parameters:

- mongoData: object type
- options: object type - [standard morgan options](https://github.com/expressjs/morgan#options)
- format: string type - [standard morgan format](https://github.com/expressjs/morgan#predefined-formats)

An example without morgan options:

```
app.use(mongooseMorgan({
    connectionString: 'mongodb://localhost:27017/logs-db'
   }, {}, 'short'
));
```

As you can see inside logs-db database this code will create collection named *logs* and a type of formatting for morgan will be *short*.

Full example:

```
app.use(mongooseMorgan({
        collection: 'error_logger',
        connectionString: 'mongodb://localhost:27017/logs-db',
        user: 'admin',
        pass: 'test12345'
    },
    {
        skip: function (req, res) {
            return res.statusCode < 400;
        }
    },
        'dev'
));
```

In this detailed example, we define MongoDB collection name, username, and password. For morgan options, we defined a function which will filter response status codes and if the status code is lower than 400 it won't save the request in the database. As for the type of formatting, we are using *dev* format.


If you are using morgan package for logging requests in a console you can easily setup your project to log all requests with filtering in a Mongo database by using mongoose-morgan package.

The source code of this library can be found inside the [GitHub repository](https://github.com/nemanjapetrovic/mongoose-morgan). All contributions are very welcome.

In the end, I tried to make this library helpful and give something back to the open-source community. If you have thoughts on this you can find me on [Twitter](https://twitter.com/nem_pet) and [LinkedIn](https://www.linkedin.com/in/nempet/).