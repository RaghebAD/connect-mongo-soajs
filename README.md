# connect-mongo-soajs
[![Build Status](https://travis-ci.org/soajs/connect-mongo-soajs.svg?branch=master)](https://travis-ci.org/soajs/connect-mongo-soajs)
[![Coverage Status](https://coveralls.io/repos/soajs/connect-mongo-soajs/badge.png)](https://coveralls.io/r/soajs/connect-mongo-soajs)
[![NPM version](https://badge.fury.io/js/connect-mongo-soajs.svg)](http://badge.fury.io/js/connect-mongo-soajs)

### MongoDB session store for Express and Connect

Fed up from not being able to find a decent mongo store with full control over mongo configuration including replica set support and not face a race condition because mongo connections are not being handled the right way.

Connect-mongo-soajs is a store that gives you full control over mongo configuration. Enjoy an unleashed reliable mongo store :)

This is a standanlone version of soajs connect mongo. You can simply use it as a standalone package as described below.

## Installation

    $ npm install connect-mongo-soajs

## Usage

    var session = require('express-session');
    var MongoStore = require('connect-mongo-soajs')(session);
    var store = new MongoStore(dbConfig)

### dbConfig
The dbConfig object looks like this:

    var dbConfig =  {
        'name' : "",
        'prefix' : "",
        'servers' : [{host : "", port : ""} ...],
        'credentials' : {username : "", password : ""},
        'URLParam' : { },
        'extraParam' : {db : {}, server : {}, replSet : {}, mongos: {}},
        'store': {},
        'collection': "sessions",
        'stringify': false,
        'expireAfter': 1000 * 60 * 60 * 24 * 14 // 2 weeks
    }
For a full reference regarding **URLParam** & **extraParam** please check out [mongodb website](http://mongodb.github.io/node-mongodb-native/driver-articles/mongoclient.html#mongoclient-connect)

Here is an example of dbConfig object:

    var dbConfig = {
        "name": "core_session",
        "prefix": "",
        "servers": [
            {
                "host": "127.0.0.1",
                "port": 27017
            }
        ],
        "credentials": null,
        "URLParam": {
            "connectTimeoutMS": 0,
            "socketTimeoutMS": 0,
            "maxPoolSize": 5,
            "wtimeoutMS": 0,
            "bufferMaxEntries": 0
        },
        "extraParam": {
            "db": {
                "native_parser": true,
                "slaveOk": true
            }
        },
        'store': {},
        "collection": "sessions",
        'stringify': false,
        'expireAfter': 1000 * 60 * 60 * 24 * 14 // 2 weeks
    }

## A simple example

    var express = require('express');
    var session = require('express-session');
    var MongoStore = require('connect-mongo-soajs')(session);
    var app = express();

    var dbConfig = {
        "name": "core_session",
        "prefix": "",
        "servers": [
            {
                "host": "127.0.0.1",
                "port": 27017
            }
        ],
        "credentials": null,
        "URLParam": {
            "connectTimeoutMS": 0,
            "socketTimeoutMS": 0,
            "maxPoolSize": 5,
            "wtimeoutMS": 0,
            "bufferMaxEntries": 0
        },
        "extraParam": {
            "db": {
                "native_parser": true,
                "slaveOk": true
            }
        },
        'store': {},
        "collection": "sessions",
        'stringify': false,
        'expireAfter': 1000 * 60 * 60 * 24 * 14 // 2 weeks
    }

    var sessionConfig = {
        "name": "soajsID",
        "secret": "this is your session secret shhhhhh",
        "cookie": {"path": '/', "httpOnly": true, "secure": false, "domain": "localhost", "maxAge": null},
        "resave": false,
        "saveUninitialized": false,
        "store" : new MongoStore(dbConfig)
    };

    app.use(session(sessionConfig));
    ...


## Full reference for **URLParam** & **extraParam**
Below is how a configuration object should look like, but Please check out [mongodb website](http://mongodb.github.io/node-mongodb-native/)

### Reference for Mongo Configuration for driver version 2.1
 * [Full REF](http://mongodb.github.io/node-mongodb-native/)
 * [CLIENT REF](http://mongodb.github.io/node-mongodb-native/2.1/api/MongoClient.html)
 * [API REF](http://mongodb.github.io/node-mongodb-native/2.1/api)
 * [DB REF](http://mongodb.github.io/node-mongodb-native/2.1/api/Db.html)
 * [REPLICASET REF](http://mongodb.github.io/node-mongodb-native/2.1/api/ReplSet.html)
 * [Setup Mongo + SSL](https://docs.mongodb.com/manual/tutorial/configure-ssl/)
 * [MongoDB NodeJS Driver](http://mongodb.github.io/node-mongodb-native/2.1/reference/connecting/connection-settings/)
 

	//REF: https://docs.mongodb.com/manual/reference/connection-string/#connections-connection-options
	"URLParam": {
		"poolSize": 5,                              //pooling value, default=100
		"ssl": false,                               //Whether or not the connection requires SSL
		"sslValidate": true,                       //Validate mongod server certificate against ca, mongod server 2.4 or higher
		"sslCA": null,                             //Array of valid certificates either as Buffers or Strings
		"sslCert": null,                           //String or buffer containing the certificate we wish to present
		"sslKey": null,                            //String or buffer containing the certificate private key we wish to present
		"sslPass": null,                           //String or buffer containing the certificate password
		"autoReconnect": true,                      //Reconnect on error
		"noDelay": false,                           //TCP Socket NoDelay option
		"keepAlive": 0,                             //TCP KeepAlive on the socket with a X ms delay before start
		"connectTimeoutMS": 0,                      //connection timeout value in ms or 0 for never
		"socketTimeoutMS": 0,                       //socket timeout value to attempt connection in ms or 0 for never
		"reconnectTries": 30,                      //Server attempt to reconnect #times
		"reconnectInterval": 1000,                 //Server will wait # milliseconds between retries
		"ha": true,                                //Turn on high availability monitoring.
		"haInterval": 2000,                        //Time between each replicaset status check
		"replicaSet": "rs",                         //name of the replicaSet if mongo connection is not standalone
		"secondaryAcceptableLatencyMS": 15,        //Sets the range of servers to pick when using NEAREST
		"acceptableLatencyMS": 15,                 //Cutoff latency point in MS for MongoS proxy selection
		"connectWithNoPrimary": false,             //Sets if the driver should connect even if no primary is available
		"authSource": null,                         //specify a db to authenticate the user if the one he is connecting to doesn't do that
		//w REF: https://docs.mongodb.com/manual/reference/write-concern/#writeconcern
		"w": "majority",                           //values majority|number|<tag set>
		"wtimeoutMS": 0,                           //timeout for w, 0 is for never
		"j": false,                                //Specify a journal write concern.
		"forceServerObjectId": false,               //Force server to assign _id values instead of driver. default=false
		"serializeFunctions": false,                //Serialize functions on any object. default=false
		"ignoreUndefined": false,                   //Specify if the BSON serializer should ignore undefined fields. default=false
		"raw": false,                               //Return document results as raw BSON buffers. default=false
		"promoteLongs": true,                       //Promotes Long values to number if they fit inside the 53 bits resolution. default=true
		"bufferMaxEntries": -1,                    //Sets a cap on how many operations the driver will buffer up before giving up on getting a working connection, default is -1 which is unlimited.
		"readPreference": "secondaryPreferred",     //if ReplicaSet, prefered instance to read from. value=primary|secondary|nearest|primaryPreferred|secondaryPreferred
		"pkFactory": null,                          //A primary key factory object for generation of custom _id keys.
		"promiseLibrary": null,                     //A Promise library class the application wishes to use such as Bluebird, must be ES6 compatible
		"readConcern": null                         //Specify a read concern for the collection. (only MongoDB 3.2 or higher supported)
	},
	"extraParam": {
		//REF: http://mongodb.github.io/node-mongodb-native/2.1/api/Db.html
		"db": {
			"native_parser": true,
			"slaveOk": true
		},
		"server": {
			"checkServerIdentity": true,               //Ensure we check server identify during SSL, set to false to disable checking
			"monitoring": true                        //Triggers the server instance to call ismaster
		},
		"replSet": {
			"checkServerIdentity": true               //Ensure we check server identify during SSL, set to false to disable checking
		},
		"mongos": {
			"checkServerIdentity": true               //Ensure we check server identify during SSL, set to false to disable checking
		}
	}