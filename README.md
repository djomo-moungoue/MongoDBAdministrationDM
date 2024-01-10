# MongoDBAdministrationDM
Udemy Complete MongoDB Administration Guide Tutorial

## Introduction 

[cmder - A Portable console emulator for Windows](https://cmder.app/)

A `document database`: 
- stores data in separate documents that are independent of each other. Each document has a set of fields that can differ one from another.
- stores data using `json format`. 

`JSON` stands for JavaScript Object Notation where:
- the basic syntax is: {“key”: value}. The key must be enclosed in quotes.
- the supported data types are: String, Number, Boolean, Array, Object, Null

Example:
~~~json
{
    “String”: “That’s a sample string”;
    “Number”: 3,
    “Boolean”: true,
    “Array”: [1, 2, 3],
    “Object”: 
    {
        “Key1”: 1,
        “Key2”: “Tow”,
        “Key3”: false,
        “Key4”: [“first”, “second”],
        “Key5”:
        {
            “nestedobjectkey1”: “nested”,
            “nestedobjectkey2”: false,
        }
    }
    “Null”: null // meaning that the value is absent.
}
~~~

`BSON` stands for Binary JSON.  It extends JSON with additional data types:
- (1.1) String
- (1.2) Number:
    - Double
    - 32-bit Integer
    - 64-bit Integer
- (1.3) Boolean
- (1.4) Array
- (1.5) Object
- (1.6) Null
- (2) Regular Expression
- (3) Timestamp
- (4) Binary data
- (5) Date
- (7) ObjectId
- (8) and others.

N.B.: `MongoDB` stores documents in `BSON format`. 

The `extended JSON` is used to convert JSON to BSON and vice versa. It has a `shell mode` and a `strict mode` of operation:

The `Shell Mode`:
- uses inline `BSON` types
- is the internal MongoDB mode
- is compatible with MongoDB shell
- can be represented as follows:
    ~~~bson
    {
        "_id": ObjectId("ifjdfdslfkdf"), 
        "age": NumberInt(39), 
        "registered": ISODate("2024-01-02T01:24:10 -02:00")
    }
    ~~~

The `Strict Mode`:
 - represents BSON types using special key names prepended with $
 - is compatible with the `JSON` standard
 - is used in all JSON parsers
- can be represented as follows: 
    ~~~bson
    {
        "_id": {"$oid": "ifjdfdslfkdf"}, 
        "age": 39, 
        "registered": {"$date": "2024-01-02T01:24:10 -02:00"}
    } 
    ~~~

Both modes are supported by:
- external MongoDB drivers and REST API
- mongoimport utility

MongoDB has the following structure: 
~~~
Databases(
    Collections(
        Documents{
            "key": value pairs
        } // Documents stored in BSON format
    ) // Collections
) // Databases independent from each other
~~~

MongoDB has the following architecture: 
- `mongod`: is the service used to launch the MongoDB server.
- `mongo shell`: is the MongoDB command line bundled to MongoDB server. It is launched by using the `mongo` command. 
- `mongodump`: is used to backup the data of a MongoDB server in a dump file.
- `mongorestore`: is used to restore the data from a dump file into a MongoDB server.
- `mongostat`: is used to monitor in real time the MongoDB performance.
- `mongoexport`:  is used to export a subset of the database.
- `mongoimport`: is used to import a subset of data to a MongoDB database. 

Different tools to interact with MongoDB Server are:
-  `mongo shell`: is a command line client that allows you to connect and interact manually with a local MongoDB server. 
- `Robot 3T (Robomongo)` and `MongoDB Compass` are client applications that also let you connect and interact manually with a MongoDB server.
- `MongoDriver` is an interface that enables you to connect and interact via a programming language (PowerShell, Python, etc.) with MongoDB Server.

NB: MongoDB server and MongoDB shell support SSL encryption. It is highly recommended to connect and interact remotely with them using SSL encryption. 

## Setup the MongoDB Environment

### Avaiable Options To Install MongoDB

You can install MongoDB

Locally on your PC (not scalable, not reliable):
- It is a single server
- It is suitable for educational or development purpose
- It is not suitable for production applications

By using Dedicated or Virtual managed servers:
- You have a full control
- It come with management overhead (manual scalability)

By using Cloud Databases as a service:
- It is suitable for production applications
- It offers a replica-set out of the box
- It is easy to manage and to scale
- It offers free size-limited options available

### Install MongoDB on Windows:

Download MongoDB server here: https://www.mongodb.com/try/download/community

Set up the MongoDB environment
~~~ps1
# Run PowerShell as administrator
# Create the MongoDB source directory
New-Item -ItemType Directory "\data\db"

# Launch MongoDB server
"C:\Program Files\MongoDB\Server\3.x\bin\mongod.exe"
#OUTPUT: connecting to: mongodb://127.0.01:27017

# Launch MongoDB shell
"C:\Program Files\MongoDB\Server\3.x\bin\mongo.exe"
# You should be connected to mongo shell if successful
~~~

~~~mongosh
# Check the mongodb version
db.version()

# See default databases
show dbs
# admin 0.000GB
# config 0.000GB
# local 0.000GB
~~~

MongoDB Server Hosting Services Overview
- [DigitalOcean Droplets](https://www.digitalocean.com/products/droplets)
- [Hetzner Dedicated Servers and Virtual Private Servers (VPS)](https://www.hetzner.com/)
- [GoDaddy Dedicated Servers and VPS](https://www.godaddy.com/en-uk/hosting/dedicated-server)
- [Amazon EC2](https://aws.amazon.com/de/pm/ec2/)

Retrieve the list of available commands
~~~sh
help
~~~

Connect to a MongoDB server located on a local computer
~~~ps1
mongo 127.0.01
~~~

Retrieve all mongoDB connections
~~ps1
Get-NetTCPConnection | Where-Object {$_.LocalPort -eq 27017}
~~~

Retrieve all IP-addresses MongoDB ist listening to
~~~sh
# Get-NetTCPConnection | Select-String mongod # Doesn't work
netstats -plant | grep mongod # Works
~~~

Make a MongoDB server accepts connections from any IP address
~~~sh
# Open mongod.conf and replace
network interfaces
net: 
port: 27017
bindIp: 127.0.0.1 
# with
network interfaces
net:
port: 27017
bindIp: 0.0.0.0
~~~

Restart the mongo db service
~~~ps1
Restart-Service -Name mongod
~~~

Connect to a MongoDB server located on a remote computer in the same network
~~~
mongo IPAddressRemoteComputer
~~~

### Administrate MongoDB Usrers

Create MongoDB `admin user`





