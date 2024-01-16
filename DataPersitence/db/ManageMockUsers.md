
# :one: Create the Root User of MongoDB server

You have to create at least user in the mongodb admin database before starting.

Switch to the admin database
> use admin

Show all avalable collections
> show collections

Output example:
> system.version

~~~json
// Create a user named: root with the root priviledge in the database name: admin
// This user will have full access to MongoDB server.
db.createUser
(
    {
        "user": "root"
        ,"pwd": "root"
        ,"roles":
        [
            {
                "role": "root"
                ,"db": "admin"
            }
        ] 
    }
);
~~~

Show all avalable collections
> show collections

Output example:
> system.version
> system.users

Show all avaible users
~~~json
show users

//Output example
[
  {
    _id: 'admin.root',
    userId: UUID('b78b5902-d58f-4c03-a467-94fb58ab439e'),
    user: 'root',
    db: 'admin',
    roles: [
      {
        role: 'root',
        db: 'admin'
      }
    ],
    mechanisms: [
      'SCRAM-SHA-1',
      'SCRAM-SHA-256'
    ]
  }
]
~~~

# :two: Enable Authorization on MongoDB server

After creating the root user, you have to enable authorization on the MongoDB server by editing the configuration file in an editor.
~~~ps1 (Administrator)
# Run PowerShell with highest priviledges
# Open the configuration file
notepad.exe $env:ProgramFiles\MongoDB\Server\6.0\bin\mongod.cfg
# code $env:ProgramFiles\MongoDB\Server\6.0\bin\mongod.cfg # In VS code terminal only
~~~

Add this lines in the security section:
~~~
security:
    authorization: enabled
~~~

Restart the MongoDB service
~~~ps1 (Administrator)
# Run PowerShell with highest priviledges
Get-Service -Name "MongoDB" | Restart-Service
~~~

Connect to the admin database as root user
~~~ps1
mongosh "mongodb://root:root@127.0.0.1:27017/admin"

<# Output example
Current Mongosh Log ID: 65a663f329f5b97b16d18824
Connecting to:          mongodb://<credentials>@127.0.0.1:27017/admin?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.10.1
Using MongoDB:          6.0.6
Using Mongosh:          1.10.1

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

admin> 
#>
~~~