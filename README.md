node-opcua
==========

an implementation of a OPC UA stack fully written in javascript and nodejs


[![NPM version](https://badge.fury.io/js/node-opcua.png)](http://badge.fury.io/js/node-opcua)
[![Build Status](https://travis-ci.org/erossignon/node-opcua.png?branch=master)](https://travis-ci.org/erossignon/node-opcua)
[![Dependency Status](https://gemnasium.com/erossignon/node-opcua.png)](https://gemnasium.com/erossignon/node-opcua)
[![Coverage Status](https://coveralls.io/repos/erossignon/node-opcua/badge.png)](https://coveralls.io/r/erossignon/node-opcua)
[![Code Climate](https://codeclimate.com/github/erossignon/node-opcua.png)](https://codeclimate.com/github/erossignon/node-opcua)

[![OPC UA](http://b.repl.ca/v1/OPC-UA-blue.png)](http://opcfoundation.org/)



node-opcua is an experimental OPC-UA stack written in NodeJS.

Why NodeJS ?

Because nodeJs is a great framework to design asynchronous application.


Getting started
================

installing node-opcua
---------------------

    $ npm install node-opcua

first example
---------------------

install pre-requisite:


    $ npm install async node-opcua

create a file `myfirstclient.js` and add the following code :

```javascript
var opcua = require("node-opcua");
var async = require("async");

var client = new opcua.OPCUAClient();

var endpointUrl = "opc.tcp://" + require("os").hostname() + ":4841";

var the_session = null;
async.series([


    // step 1 : connect to
    function(callback)  {
      client.connect(endpointUrl,function (err) {
         if(err) {
           console.log(" cannot connect to endpoint :" , endpointUrl );
         } else {
          console.log("connected !");
         }
         callback(err);
      });
   },


   // step 2 : createSession
   function(callback) {
     client.createSession( function(err,session) {
         if(!err) {
           the_session = session;
         }
         callback(err);
     });

   },


   // step 3 : browse
   function(callback) {

     the_session.browse("RootFolder", function(err,browse_result,diagnostics){
        if(!err) {
          browse_result[0].references.forEach(function(reference) {
            console.log( reference.browseName);
          });
        }
        callback(err);
     });
   },


   // step 4 : read a variable
   function(callback) {
     the_session.readVariableValue("ns=2;s=Furnace_1.Temperature", function(err,dataValues,diagnostics) {
       if (!err) {
         console.log(" temperature = " , dataValues[0].value.value);
       }
       callback(err);
     })
   },


], function(err) {
  if (err) {
    console.log(" failure ",err);
  } else {
    console.log("done!")
  }
  // disconnect regardless
  client.disconnect(function(){});
}) ;

```


now run it

    $ node myfirstclient.js




Contributing
================


    $ git clone git://github.com/erossignon/node-opcua.git node-opcua
    $ cd node-opcua
    $ npm install
    $ npm test







[![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=gadz_er&url=https://github.com/erossignon/node-opcua&title=Node-OPCUA&language=nodejs&tags=github&category=software)

[![NPM](https://nodei.co/npm/node-opcua.png?downloads=true&stars=true)](https://nodei.co/npm/node-opcua/)

[![Project Stats](https://www.ohloh.net/p/713850/widgets/project_thin_badge.gif)](https://www.ohloh.net/p/node-opcua)


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/erossignon/node-opcua/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

