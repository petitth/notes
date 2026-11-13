# MongoDB on Debian 10

## Main commands

| Command                                                                             | Detail                                     |
| ------------------------------------------------------------------------------------|--------------------------------------------|
| ```show dbs```                                                                      | list databases                             |
| ```use local```                                                                     | connect to db local                        |
| ```show collections```                                                              | list collections from connected db         |
| ```db.createCollection("name")```                                                   | create collection                          |
| ```db.<collectionName>.insert({field1: "value", field2: "value"})```                | insert a document                          |
| ```db.<collectionName>.find()```                                                    | retrieve all records from a collection     |
| ```db.<collectionName>.find().limit(10)```                                          | retrieve first 10 records                  |
| ```db.<collectionName>.find({"_id": ObjectId("someid")}, {field1: 1, field2: 1})``` | retrieve records based on filters          |
| ```db.<collectionName>.find({"fieldName":true, someDate : {"$gte":new Date()} })``` | select with filter on date                 |
| ```db.<collectionName>.find( { "fieldName": { $exists: true, $ne: "" }})```         | select non empty values for a field        |
| ```db.<collectionName>.distinct("someId", { "anotherField": "fieldValue" })```      | select distinct                            |
| ```db.<collectionName>.count()```                                                   | count results                              |
| ```db.<collectionName>.remove({})```                                                | remove all records from a collection       |
| ```db.<collectionName>.remove({"fieldName":"fieldValue"})```                        | remove some records based on filters       |
| ```db.<collectionName>.drop()```                                                    | delete a collection                        |
| ```db.<collectionName>.find().explain("executionStats")```                          | query with time                            |
| ```db.<collectionName>.find().pretty()```                                           | query with better display                  |
| ```db.<collectionName>.getIndexes()```                                              | query with indexes                         |

## Scripts

- update : select some records based on field1 and field2, then reset fieldToUpdate
```javascript
db.<collectionName>.update(
    { "field1":"value", "field2":"value2"},
    { $set: { "fieldToUpdate": "newValue" } },
    { multi: true}
);
```

- show time
```javascript
print("current time : " + new Date());
print("[" + new Date() + "] number of documents = " + db.<collectionName>.find({}).count());
```

- display collection
```javascript
print("start displaying records at " + new Date());
var items = db.<collectionName>.find({});
while(items.hasNext()) {
   item = items.next();
   print("id = " + item._id + " - field1 = " + item.field1 );
}
print("finished displaying records at " + new Date());
```


## Run MongoDB Community on Debian 10

- check if running : systemctl status mongod
- start mongodb : systemctl start mongod
- stop mongodb : systemctl stop mongod
- restart mongodb : systemctl restart mongod

- start a mongo shell : mongo
- launch it from anywhere without connecting : mongo --nodb
- connect : mongo IP:27000/DB --username USER --password PWD --authenticationDatabase DB


## MongoDB Installation on Debian 10

```bash
# install Debian 9 package repository to be able to install libcurl3
echo "deb http://deb.debian.org/debian/ stretch main" | sudo tee /etc/apt/sources.list.d/debian-stretch.list

# reload packages
apt-get update

# install libcurl3
apt install libcurl3

# install mongodb
apt install mongodb-org

# check installed version
mongod --version
```
source : https://linuxhint.com/install_mongodb_debian_10/


## Configure IntelliShell for Studio 3T

- install Studio 3T
- download mongodb-enterprise-shell rpm
- extract content from rpm (as an archive) somewhere like ```~/apps```
- right click on ```db | open intellishell```
- configure it on first launch : just point to bin folder of extracted mongodb-enterprise-shell\bin

ex : ```~/mongo-shell/usr/bin/mongo```

remark : NoSqlBooster is a good alternate MongoDB client, and it is free