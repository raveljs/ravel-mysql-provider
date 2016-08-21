# ravel-mysql-provider

> Ravel DatabaseProvider for MySQL

[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/raveljs/ravel-mysql-provider/master/LICENSE) [![npm version](https://badge.fury.io/js/ravel-mysql-provider.svg)](http://badge.fury.io/js/ravel-mysql-provider) [![Dependency Status](https://david-dm.org/raveljs/ravel-mysql-provider.svg)](https://david-dm.org/raveljs/ravel-mysql-provider) [![npm](https://img.shields.io/npm/dm/ravel.svg?maxAge=2592000)](https://www.npmjs.com/package/ravel) [![Build Status](https://travis-ci.org/raveljs/ravel-mysql-provider.svg?branch=master)](https://travis-ci.org/raveljs/ravel-mysql-provider) [![Code Climate](https://codeclimate.com/github/raveljs/ravel-mysql-provider/badges/gpa.svg)](https://codeclimate.com/github/raveljs/ravel-mysql-provider) [![Test Coverage](https://codeclimate.com/github/raveljs/ravel-mysql-provider/badges/coverage.svg)](https://codeclimate.com/github/raveljs/ravel-mysql-provider/coverage)

## Example usage:

*app.js*
```javascript
const app = new require('ravel')();
const MySQLProvider = require('ravel-mysql-provider');
new MySQLProvider(app);
// ... other providers and parameters
app.init();
// ... the rest of your Ravel app
```

## Configuration

Requiring the `ravel-mysql-provider` module will register a configuration parameter with Ravel which must be supplied via `.ravelrc` or `app.set()`:

*.ravelrc*
```json
{
  "mysql options": {
    "host": "localhost",
    "port": 3306,
    "user": "root",
    "password": "a password",
    "database": "mydatabase",
    "idleTimeoutMillis": 5000,
    "connectionLimit": 10
  }
}
```

All options for a `node-mysql` connection are supported, and are documented [here](https://github.com/felixge/node-mysql#establishing-connections). Additionally, the `connectionLimit` parameter controls the size of the underlying connection pool, while `idleTimeoutMillis` controls how long connections will remain idle in the pool before being destroyed. It is vital that you make `idleTimeoutMillis` shorter than the `wait_timeout` set in your mysql configuration `my.cnf`, otherwise you you might have timed-out connections sitting in your pool. Note: `idleTimeoutMillis` is specified in milliseconds, while `wait_timeout` is specified in seconds.

## Multiple Simultaneous Providers

`ravel-mysql-provider` also supports multiple simultaneous pools for different mysql databases, as long as you name them:

*app.js*
```javascript
const app = new require('ravel')();
const MySQLProvider = require('ravel-mysql-provider');
new MySQLProvider(app, 'first mysql');
new MySQLProvider(app, 'second mysql');
// ... other providers and parameters
app.init();
// ... the rest of your app
```

*.ravelrc*
```json
{
  "first mysql options": {
    "host": "localhost",
    "port": 3306,
    "user": "root",
    "password": "a password",
    "database": "myfirstdatabase",
    "idleTimeoutMillis": 5000,
    "connectionLimit": 10
  },
  "second mysql options": {
    "host": "localhost",
    "port": 3307,
    "user": "root",
    "password": "another password",
    "database": "myseconddatabase",
    "idleTimeoutMillis": 5000,
    "connectionLimit": 10
  }
}
```
