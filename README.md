# How to secure MongoDB with username , password and port
I want to set up user name & password authentication for my MongoDB instance,

>Short answer.
**#1**
Start MongoDB without access control.
```
mongod --dbpath /data/db
```
Connect to the instance, Enter into the **mongo shell**: 
```
mongo
```
Create the **user**.
```
db.createUser({
    user:"userAdminDBs",
    pwd:"xxxxxxxxxx",
    roles:[{role:"userAdminAnyDatabase", db:"admin"}]
})
use <your_desire_db_name>
db.createUser({
    user:"myDBOwner",
    pwd:"xxxxxxxxxx",
    roles:[{role:"dbOwner", db:"<your_desire_db_name>"}]
})
```
Enable Authentication : open the **config-file**
```
nano /etc/mongod.conf
```
enable it by remove the **#** comment
```
security:
  authorization: enabled
```

**Now** : Stop the MongoDB instance and start it again with access control.
>in case of **error** these commands will help you!
```
sudo chown -R mongodb:mongodb /var/lib/mongodb
sudo chown mongodb:mongodb /tmp/mongodb-27017.sock    
sudo systemctl restart mongod 
sudo systemctl status mongod 
```
Reload the **Auth**
```
mongosh --auth --port 27017
mongo --auth --port 27017
```
Connect and authenticate as the user.
