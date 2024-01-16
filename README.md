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

:memo: `MongoDB` stores documents in `BSON format`. 

The `extended JSON` is used to convert JSON to BSON and vice versa. It has a `shell mode` and a `strict mode` of operation:

:memo: The `Shell Mode`:
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

:memo: The `Strict Mode`:
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
:one: `mongod`: is the service used to launch the MongoDB server.

:two: `mongosh`: is the MongoDB command line bundled to MongoDB server. It is launched by using the `mongo` command. 

:three: `mongoimport`: is used to import a subset of data to a MongoDB database. 
Further reading [here](https://www.mongodb.com/developer/products/mongodb/mongoimport-guide/)

:four: `mongoexport`:  is used to export a subset of the database.

:five: `mongodump`: is used to backup the data of a MongoDB server in a dump file.

:six: `mongorestore`: is used to restore the data from a dump file into a MongoDB server.

:seven: `mongostat`: is used to monitor in real time the MongoDB performance.

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

### Install MongoDB on Windows Local Computer using the PowerShell command line:

#### :zero: Retrieve all available Mongodb packages
~~~ps1
winget search --name mongodb

<# Output example
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

#### :one: Install MongoDB server on your local computer
~~~ps1
<# 
Installs the selected package, either found by searching a configured source or directly from a manifest. By default, the query must case-insensitively match the id, name, or moniker of the package.
--id                                 Filter results by id.
-e,--exact                           Find package using exact match
-s,--source                          Find package using the specified source.
-h, --silent	Runs the installer in silent mode. This suppresses all UI. The default experience shows installer progress.
#>
winget install --id MongoDB.Server --exact --source winget --silent

<# Output example
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

:heavy_plus_sign: Append the path of MongoDB executable to the user environment variable
> C:\Program Files\MongoDB\Server\6.0\bin

#### :two: Install MongoDB Shell on your local computer
~~~ps1
winget install --id MongoDB.Shell --exact --source winget --silent
<# Output example
Found MongoDB Shell [MongoDB.Shell] Version 1.10.1
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
Downloading https://downloads.mongodb.com/compass/mongosh-1.10.1-x64.msi
  ██████████████████████████████  35.4 MB / 35.4 MB
Successfully verified installer hash
Starting package install...
Successfully installed
#>
~~~

Restart PowerShell and launch the mongosh console:
~~~ps1
mongosh

<# Output example
Current Mongosh Log ID: 65a6593fc593e766a03346f8
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.10.1
Using MongoDB:          6.0.6
Using Mongosh:          1.10.1

For mongosh info see: https://docs.mongodb.com/mongodb-shell/


To help improve our products, anonymous usage data is collected and sent to MongoDB periodically (https://www.mongodb.com/legal/privacy-policy).
You can opt-out by running the disableTelemetry() command.
test>
#>

~~~

#### :three: Install MongoDB Compass Community on your local computer
~~~ps1
winget install --id MongoDB.Compass.Community --exact --source winget --silent

<# Output example
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
# if the root user is not yet set
MongoDBCompass.exe mongodb://localhost:27017 &
<# Output example
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Job1            BackgroundJob   Running       True            localhost            MongoDBCompass.exe mongo…
#>
~~~

~~~ps1
# Open a connexion on mongodb://127.0.01:27017 without blocking the terminal
# if the root user is not set
MongoDBCompass.exe mongodb://root:root@localhost:27017 &
~~~




#### :four: Create the data directory
~~~ps1
# Create the MongoDB source directory in the C drive C:\data\db
New-Item -ItemType Directory "\data\db"

# In my case I have rather created the MongoDB source directory in the  git directory
New-Item -ItemType Directory "$env:USERPROFILE\ProjectsDM\MongoDBAdministrationDM\DataPersistence\db"
~~~

### Establish Local and Remote Connections

Connect to a MongoDB server located on a local computer
~~~ps1
mongosh 127.0.0.1

<# Output example
Current Mongosh Log ID: 65a664a5a78f6c13fe515109
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.10.1
Using MongoDB:          6.0.6
Using Mongosh:          1.10.1

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

test>
#>
~~~

Retrieve all establishe MongoDB connections
~~~ps1
Get-NetTCPConnection -State Established | Where-Object {$_.LocalPort -eq 27017}

<# Output example
LocalAddress                        LocalPort RemoteAddress                       RemotePort State       AppliedSetting OwningPr
                                                                                                                        ocess    
------------                        --------- -------------                       ---------- -----       -------------- -------- 
127.0.0.1                           27017     127.0.0.1                           60217      Established Internet       14056    
127.0.0.1                           27017     127.0.0.1                           60129      Established Internet       14056
#>
~~~

Get all TCP connections that use a TCP applied setting of Internet.
~~~ps1
Get-NetTCPConnection -AppliedSetting Internet
~~~

Make a MongoDB server accepts connections from `any IP address`

~~~ps1
# Run PowerShell with highest priviledges
# Open the configuration file
notepad.exe $env:ProgramFiles\MongoDB\Server\6.0\bin\mongod.cfg
# code $env:ProgramFiles\MongoDB\Server\6.0\bin\mongod.cfg # In VS code terminal only
~~~

Add this lines in the network interfaces section:
~~~ps1
# network interfaces
net:
  port: 27017
  #bindIp: 127.0.0.1
  bindIp: 0.0.0.0
~~~

Restart the MongoDB service
~~~ps1 (Administrator)
# Run PowerShell with highest priviledges
Get-Service -Name "MongoDB" | Restart-Service
~~~

Connect to a MongoDB server located on a remote computer in the same network
~~~ps1
<#
Username: unser name in the remote database
Username: unser pasword in the remote database
IPAddressRemoteComputer: IP address of the remote computer
DatabaseName: MongoDB authentiction database on the remote computer
#>
mongosh mongodb://Username:Password@IPAddressRemoteComputer:27017/DatabaseName
~~~

#### Data Persistence

Database Architecture
> .\DataPersitence\DatabaseArchitecture.md

Manage Users
> .\DataPersitence\db\ManageMockUsers.md

### Host MongoDB Server by a Cloud Service Provider

:one: [Amazon EC2 VPS MongoDB](https://aws.amazon.com/de/pm/ec2/)

~~~ps1
# Connection example to the admin database
mongo "mongodb://root:root@ec2-34-216-244-19.us-west-2.compute.amazonaws.com/admin"
~~~

:two: [DigitalOcean Droplets](https://www.digitalocean.com/products/droplets)

:three: [Hetzner Dedicated Servers and Virtual Private Servers (VPS)](https://www.hetzner.com/)

:four: [GoDaddy Dedicated Servers and VPS](https://www.godaddy.com/en-uk/hosting/dedicated-server)

#### Section 6: Using MongoDB as a Service (Cloud MongoDB)

:one: Introduction to MongoDB Cloud

:two: Create MongoD Atlas Cluster

:three: Verify connection to the Cloud MongoDB Replica Set

~~~ps1
# Connection example to the admin database
mongo "mongodb://cluster0-shard-00-00-fdpbt.mongodb.net:27017,cluster0-shard-00-01-fdpbt.mongodb.net:27017,cluster-00-02-fdpbt.net:27017/?replicatSet=Cluster0-shard-0" --ssl --authenticationDatabase admin -username root --password root
~~~

## Explore Mongo DB Shell and Server

### MongoDB Server

MongoDB Server:
- :one: Windows Service is named `MongoDB`
- :two: daemon is named `Mongod`

MongoDB Server Version
~~~ps1
mongod --version
<# Output example
db version v6.0.6
Build Info: {
    "version": "6.0.6",
    "gitVersion": "26b4851a412cc8b9b4a18cdb6cd0f9f642e06aa7",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "windows",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
#>
~~~

Or

~~~ps1
mongosh

db.version()
# 6.0.6
~~~

Verify which JavaScript engine is used in you MongoDB Server
~~~ps1
mongosh

db.serverBuildInfo()

<# Output example
{
  version: '6.0.6',
  gitVersion: '26b4851a412cc8b9b4a18cdb6cd0f9f642e06aa7',
  targetMinOS: 'Windows 7/Windows Server 2008 R2',
  modules: [],
  allocator: 'tcmalloc',
  javascriptEngine: 'mozjs', //modernized JavaScript
  sysInfo: 'deprecated',
  versionArray: [ 6, 0, 6, 0 ],
  openssl: { running: 'Windows SChannel' },
  buildEnvironment: {
    distmod: 'windows',
    distarch: 'x86_64',
    cc: 'cl: Microsoft (R) C/C++ Optimizing Compiler Version 19.31.31107 for x64',
    ccflags: '/nologo /WX /FImongo/platform/basic.h /fp:strict /EHsc /W3 /wd4068 /wd4244 /wd4267 /wd4290 /wd4351 /wd4355 /wd4373 
/wd4800 /wd4251 /wd4291 /we4013 /we4099 /we4930 /errorReport:none /MD /O2 /Oy- /bigobj /utf-8 /permissive- /Zc:__cplusplus /Zc:sizedDealloc /volatile:iso /diagnostics:caret /std:c++17 /Gw /Gy /Zc:inline',
    cxx: 'cl: Microsoft (R) C/C++ Optimizing Compiler Version 19.31.31107 for x64',
    cxxflags: '/TP',
    linkflags: '/nologo /DEBUG /INCREMENTAL:NO /LARGEADDRESSAWARE /OPT:REF',
    target_arch: 'x86_64',
    target_os: 'windows',
    cppdefines: 'SAFEINT_USE_INTRINSICS 0 PCRE_STATIC NDEBUG BOOST_ALL_NO_LIB _UNICODE UNICODE _SILENCE_CXX17_ALLOCATOR_VOID_DEPRECATION_WARNING _SILENCE_CXX17_OLD_ALLOCATOR_MEMBERS_DEPRECATION_WARNING _SILENCE_CXX17_CODECVT_HEADER_DEPRECATION_WARNING _SILENCE_ALL_CXX20_DEPRECATION_WARNINGS _CONSOLE _CRT_SECURE_NO_WARNINGS _ENABLE_EXTENDED_ALIGNED_STORAGE _SCL_SECURE_NO_WARNINGS _WIN32_WINNT 0x0A00 BOOST_USE_WINAPI_VERSION 0x0A00 NTDDI_VERSION 0x0A000000 BOOST_THREAD_VERSION 5 BOOST_THREAD_USES_DATETIME BOOST_SYSTEM_NO_DEPRECATED BOOST_MATH_NO_LONG_DOUBLE_MATH_FUNCTIONS BOOST_ENABLE_ASSERT_DEBUG_HANDLER BOOST_LOG_NO_SHORTHAND_NAMES BOOST_LOG_USE_NATIVE_SYSLOG BOOST_LOG_WITHOUT_THREAD_ATTR ABSL_FORCE_ALIGNED_ACCESS'
  },
  bits: 64,
  debug: false,
  maxBsonObjectSize: 16777216,
  storageEngines: [ 'devnull', 'ephemeralForTest', 'wiredTiger' ],
  ok: 1
}
#>
~~~

### MongoDB Shell

MongoDB Shell:
- :one: is based on JavaScript
- :two: uses SpiderMonkey JavaScript engine starting from v3.2
- :three: supports ECMAScript 6 (ES6) starting from v3.2
- :four: supports mongo shell and server-side JavaScript
- :five: uses `mongosh` in Windows OS and `mongo` in Linux and Mac OS to start his console

MongoDB Shell Version
~~~ps1
mongosh --version
# 1.10.1
~~~

Or

~~~ps1
mongosh

version()
# 1.10.1
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

~~~ps1
mongosh

typeof db
# Output: object
~~~

Show all methods of the db object
~~~ps1
mongosh

db. # press tab twice to show all methods of the db object
db.c # press tab twice to show all commands having the prefix c
~~~

Show a function declaration
~~~ps1
mongosh

db.version
<# Output example
[Function: version] AsyncFunction {
  apiVersions: [ 0, 0 ],
  returnsPromise: true,
  serverVersions: [ '0.0.0', '999.999.999' ],
  topologies: [ 'ReplSet', 'Sharded', 'LoadBalanced', 'Standalone' ],
  returnType: { type: 'unknown', attributes: {} },
  deprecated: false,
  platforms: [ 'Compass', 'Browser', 'CLI' ],
  isDirectShellCommand: false,
  acceptsRawInput: false,
  shellCommandCompleter: undefined,
  help: [Function (anonymous)] Help
}
#>
~~~

Call a function
~~~ps1
mongosh

db.version()
# 6.0.6
~~~


## MongoDB BSON Data Types

|Number ID|Type|Syntax|String ID|
|---|---|---|---|
|1|Double|0.0|"dbouble"|
|2|String|""|"string"|
|3|Object|{}|"object"|
|4|Array|[]|""|
|7|ObjectId|ObjectId()|"objectId"|
|8|Boolean|true/false|"boolean"|
|9|Date|ISODate()|"date"|
|16|32-bit Integer|NumberInt()|"int"|
|18|64-bit Integer|NumberLong()|"long"|

:memo: Memo:
- :one:If type of Number is not specified, it will be stored as Double BSON value, because MongoDB uses JavaScript engine which doesn't distinguish between integer and decimal numbers.
- :two: `$type` is an operator in MongoDB to query documents by BSON type. 
- :three: `_id`:
    - makes each document unique across the database. 
    - is a mandatory document field
    - can be assigned manually
    - will be created automatically if not assigned manually (recommended)
- :four: `ObjectId("65a6a8da4bd4f647147298dd")`:
    - is a special BSON type used to assigne unique value to the `_id field` of a document.
    - is 12 Bytes long divided in 4 parts:
        - :one: 4 Bytes representing the document creation date in seconds since Unix epoch "65 a6 a8 da"
        - :two: 3 Bytes representing the machine ID (MD5 hash of the hostname and MAC address) "4b d4 f6"
        - :two: 2 Bytes representing the process ID "47 14"
        - :two: 3 Bytes representing a counter (starts from random number) incremented for each new document inserted "72 98 dd"
- :five: Dates in MongoDB server:
    - are stored in ISO format
    - new Date() and ISODate() generates the current timestamp in ISO format.
    - Be carefull when converting date, because the month number start with 0 (January) for example. The output date could converted to the corresponding time zone date, or to UTC datetime.

Illustration
~~~ps1
mongosh

var obj = {a: "", b: {}, c: [], d: true};
print(obj);
#Output: { a: '', b: {}, c: [], d: true }

typeof obj.a;
typeof obj.b;
typeof obj.c;
obj.c instanceof Array;
typeof obj.d;

<# Output
string
object
object
true
boolean
test>
#>
~~~

Generate an objecid
~~~ps1
mongosh

ObjectId()
# Output: ObjectId("65a6a8da4bd4f647147298dd")
~~~

~~~ps1
mongosh

new Date()
#Output: ISODate("2024-01-16T16:12:52.384Z")

ISODate()
#Output: ISODate("2024-01-16T16:12:59.916Z")

Date()
#Output: Tue Jan 16 2024 17:17:50 GMT+0100 (Central European Standard Time)
~~~

## Explore Databases and Collections

| Command | Description |
|---|---|
| db | Show the active Database |
| show dbs | Show all Databases |
| use DatabaseName | Change the active database |
| show collections | Show all Collections inside the active database |

:memo: Memo:
- :one: There is no command to create new Database. Database will be created together with first collection.
- :two: The command `db.dropDatabase()` drops the active database.
- :three: As soon as you drop the last collection of an active database, the active database will be dropped as well automatically.

|Create|Read|Update|Delete|
|---|---|---|---|
|db.createUser()|db.getUsers(), db.getUser()|db.updateUser(), db.changeUserPassword()|db.dropAllUsers(), db.dropUser()|
|db.createRole()|db.getRoles(), db.getRole()|db.updateRole()|db.dropAllRoles(), db.dropAllRole()|
|db.createCollection("CollectionName"), db.createEncryptedCollection()|db.getCollection(), db.getCollectionInfos(), db.getCollectionNames(), db.printCollectionStats()||db.getCollection("CollectionName").drop()|
|db.createView()||||
||||db.dropDatabase()|

|Grant|Revoke|Manage|
|---|---|---|
|db.grantRolesToRole()|db.revokeRolesFromRole()|db.version(), db.getMongo(), db.serverBuildInfo()|
|db.grantRolesToUser()|db.revokeRolesFromUser()|db.setLogLevel(), db.rotateCertificates(), db.listCommands()|
|db.grantPrivilegesToRole()||db.isMaster(), db.logout(), db.shutdownServer()|

Create a collection in the acive database
~~~ps1
mongosh mongodb://root:root@localhost:27017

use MockDatabase
MockDatabase> db.createCollection("MockUsers")
MockDatabase> show collections
# MockUsers
~~~

Practice
~~~ps1
//Connect to mongodb shell
mongosh mongodb://root:root@localhost:27017

// Create a db called demoDB
db
show dbs
use demoDB
db
show dbs

// Create 2 collections myCollection1 and myCollection2
db.createCollection("myCollection1")
db.createCollection("myCollection2")
show collections
show dbs

// Delete both collections. Delete also demoDB
db.getCollection("myCollection1").drop()
db.getCollection("myCollection2").drop()
show collections
db
show dbs
~~~  
  
     

            

  
                               
          
                                   
