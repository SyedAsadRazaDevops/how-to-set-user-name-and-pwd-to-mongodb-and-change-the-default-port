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
use admin
db.createUser({
    user:"userAdminAnyDatabase",
    pwd:"xxxxxxxxxx",
    roles:[{role:"userAdminAnyDatabase", db:"admin"}]
})
use <your_desire_db_name>
db.createUser({
    user:"dbOwner",
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
**#2**
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
Connect and authenticate as the **User**.
```
use <your_desire_db_name>
```
enter your pwd on the promtconsole
```
db.auth("myDBOwne", passwordPrompt())
```
**check** the previleges my show the list of data, use this command.**if** it show the collection its mean you are in goodorder **else** you make mistake in above sections.
```
show collections
```
**if**, you are connecting mongodb to the **laravel application** use this connection type:
```
DB_DSN=mongodb://username:password@127.0.0.1:27017/<YOU-DB-NAME>

DB_DSN=mongodb://myDBOwne:XXX-XXX-XXX-X@127.0.0.1:27017/<YOU-DB-NAME>
```



For Additional information
__________________________________________________________________________
for **change the pwd** use this:
```
use <your_desire_db_name>
db.changeUserPassword("myDBOwner", "xxx-xxx-xxx-x")
````
for **del / remove user** use this:
```
use <your_desire_db_name>
db.dropUser("dbOwner")
```
```
use products
db.dropUser("reportUser1", {w: "majority", wtimeout: 5000})
```


[LINK]:https://stackoverflow.com/questions/4881208/how-to-secure-mongodb-with-username-and-password

[LINK]:https://severalnines.com/blog/how-secure-mongodb-ransomware-ten-tips
