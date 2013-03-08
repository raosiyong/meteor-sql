Meteor SQL
==========

This is an initial implementation of Meteor SQL. It currently only supports MySQL.

# Features
* Full server side support of select, insert, update and delete on a table.
* All changes get propagated to all clients as with MongoDb
* Changes to the db from other apps are detected immediately (100ms, configurable), and propagated to the client
* Light weight implementation
 * Changes are handled by triggers, no diffs to existing queries needed
 * Polling is done on a single indexed table, very little overhead.
* includes https://github.com/hiddentao/squel for cleaner query construction

# Limitations
* Client side the collection still use mongo syntax for find()
* All tables need to have a unique id 
* Insert, Update and Delete operations on the client don't update the data locally. Instead they run on the server and then the server refreshes the client's data. This could result in slower refresh times, but guarantees that the client always sees data that has been sent to the server. It also means that unlike minmongo, the full range of SQL options are available to the client.


#Installation

* Standard mysql set up
 * Install mysql
 * create database meteor;
 * grant all on meteor.\* to meteor@'localhost' IDENTIFIED BY 'xxxxx2344958889d'; #Change the password to something else
* Now install the mysql client for node.js
 * run meteor in the app's directory so that it builds the hierarchy in the .meteor directory
 * cd .meteor/local/build/server/
 * sudo npm install mysql@2.0.0-alpha7

# Approach
* insert into the audit trail table information about insert, update, delete
* poll the audit table
* When there is a change, publish it using Meteor's standard Meteor.publish
* Client operations, insert, update, delete use Meteor.call

# Future
* Support joins
* Support views
* Handle transactions (possible, but 