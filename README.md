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
- :green_circle: It is a single server
- :green_circle: It is suitable for educational or development purpose
- :red_circle: It is not suitable for production applications

By using Dedicated or Virtual managed servers:
- :green_circle: You have a full control
- :red_circle: It come with management overhead (manual scalability)

By using Cloud Databases as a service:
- :green_circle: It is suitable for production applications
- :green_circle: It offers a replica-set out of the box
- :green_circle: It is easy to manage and to scale
- :red_circle:  It offers free size-limited options available

### Install MongoDB on Windows using the PowerShell command line:

Retrieve all available Mongodb packages
~~~ps1
winget search --name mongodb

<# EXAMPLE OUTPUT
Name                            Id                         Version  Source
--------------------------------------------------------------------------
MongoDB                          MongoDB.Server            6.0.6    winget
NoSQLBooster for MongoDB         NoSQLBooster.NoSQLBooster 6.2.17   winget
MongoDB Shell                    MongoDB.Shell             1.10.1   winget
MongoDB CLI                      MongoDB.MongoDBCLI        1.30.0   winget
MongoDB Atlas CLI                MongoDB.MongoDBAtlasCLI   1.5.1    winget
MongoDB Tools                    MongoDB.DatabaseTools     100.7.3  winget
MongoDB Compass Readonly         MongoDB.Compass.Readonly  1.41.0.0 winget
MongoDB Compass Isolated Edition MongoDB.Compass.Isolated  1.41.0.0 winget
MongoDB Compass                  MongoDB.Compass.Full      1.41.0.0 winget
MongoDB Compass Community        MongoDB.Compass.Community 1.41.0   winget
#>
~~~

Install MongoDB server
~~~ps1
<# 
Installs the selected package, either found by searching a configured source or directly from a manifest. By default, the query must case-insensitively match the id, name, or moniker of the package.
--id                                 Filter results by id.
-e,--exact                           Find package using exact match
-s,--source                          Find package using the specified source.
-h, --silent	Runs the installer in silent mode. This suppresses all UI. The default experience shows installer progress.
#>
winget install --id MongoDB.Server --exact --source winget --silent

<#EXAMPLE OUTPUT
Found MongoDB [MongoDB.Server] Version 6.0.6
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading https://fastdl.mongodb.org/windows/mongodb-windows-x86_64-6.0.6-signed.msi
  ██████████████████████████████   481 MB /  481 MB
Successfully verified installer hash
Starting package install...
Successfully installed
#>
~~~

Install MongoDB Compass Community
~~~ps1
winget install --id MongoDB.Compass.Community --exact --source winget --silent
<# EXAMPLE OUTPUT
Found MongoDB Compass Community [MongoDB.Compass.Community] Version 1.41.0
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading https://github.com/mongodb-js/compass/releases/download/v1.41.0/mongodb-compass-1.41.0-win32-x64.msi
  ██████████████████████████████   127 MB /  127 MB
Successfully verified installer hash
Starting package install...
Successfully installed
#>
~~~

~~~ps1
# Create the MongoDB source directory in the C drive C:\data\db
New-Item -ItemType Directory "\data\db"

# Create the MongoDB source directory in the  git directory
New-Item -ItemType Directory "$env:USERPROFILE\ProjectsDM\MongoDBAdministrationDM\data\db"
~~~

:heavy_plus_sign: Append the path of MongoDB executable to the user environment variable
> C:\Program Files\MongoDB\Server\6.0\bin

:heavy_plus_sign: Append the path of MongoDB executable to the user environment variable
> C:\Program Files\MongoDB Compass

Show Compass Version
> MongoDBCompass.exe --version

Retrieve MongodDB Compass options
~~~ps1
MongoDBCompass.exe --help

<# Available options
--version                                  Show Compass Version
--trackUsageStatistics (*)                 Enable Usage Statistics
--autoUpdates (*)                          Enable Automatic Updates
--enableShell (*)                          Enable MongoDB Shell
--enableImportExport (*)                   Enable import / export feature
--enableExplainPlan (*)                    Enable explain plan feature in CRUD and aggregation view
--enableSavedAggregationsQueries (*)       Enable saving and opening saved aggregations and queries
--maxTimeMS (*)                            Upper Limit for maxTimeMS for Compass Database Operations
--protectConnectionStringsForNewConnections (*)If true, "Edit connection string" is disabled for new connections by default
#>
~~~

Launch MongoDB Compass in PowerShell
~~~ps1
# Open a connexion on mongodb://127.0.01:27017 without blocking the terminal
MongoDBCompass.exe mongodb://localhost:27017 &
<# Output example
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Job1            BackgroundJob   Running       True            localhost            MongoDBCompass.exe mongo…
#>
~~~


Show available commands
~~~mongosh
help

<#
`use` : Set current database

`show` :
- 'show databases'/'show dbs': Print a list of all available databases.
- 'show collections'/'show tables': Print a list of all collections for current database.
- 'show profile': Prints system.profile information.
- 'show users': Print a list of all users for current database.
- 'show roles': Print a list of all roles for current database. 
- 'show log <type>': log for current connection, if type is not set uses 'global' 'show logs': Print all logs.

`exit`	Quit the MongoDB shell with exit/exit()/.exit

`quit`	Quit the MongoDB shell with quit/quit()

`Mongo`	Create a new connection and return the Mongo object. Usage: new Mongo(URI, options [optional])

`connect`	Create a new connection and return the Database object. Usage: connect(URI, username [optional], password [optional])
it	result of the last line evaluated; use to further iterate

`version`	Shell version

`load`	Loads and runs a JavaScript file into the current shell environment

`enableTelemetry`	Enables collection of anonymous usage data to improve the mongosh CLI

`disableTelemetry`	Disables collection of anonymous usage data to improve the mongosh CLI

`passwordPrompt`	Prompts the user for a password

`sleep`	Sleep for the specified number of milliseconds

`print`	Prints the contents of an object to the output

`printjson`	Alias for print()

`convertShardKeyToHashed`	Returns the hashed value for the input using the same hashing function as a hashed index.

`cls`	Clears the screen like console.clear()

`isInteractive`	Returns whether the shell will enter or has entered interactive mode
#>
~~~

Show the MongoDB Server version
~~~mongosh
db.version()
<# Output example
6.0.6
#>
~~~

Print a list of all available databases
~~~mongosh
show databases //alias dbs
<# Output example
admin   40.00 KiB
config  72.00 KiB
local   72.00 KiB
#>
~~~

MongoDB Server Hosting Services Overview
- [DigitalOcean Droplets](https://www.digitalocean.com/products/droplets)
- [Hetzner Dedicated Servers and Virtual Private Servers (VPS)](https://www.hetzner.com/)
- [GoDaddy Dedicated Servers and VPS](https://www.godaddy.com/en-uk/hosting/dedicated-server)
- [Amazon EC2](https://aws.amazon.com/de/pm/ec2/)


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

## Data Persistence

.\DataPersitence\DatabaseArchitecture.md









