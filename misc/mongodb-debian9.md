# MongoDB on Debian 9

## Run MongoDB Community on Debian 9

- start mongodb : `service mongod start`
- check that mongodb is running, consulting log file : `cat /var/log/mongodb/mongod.log`
- stop mongodb : `service mongod stop`
- restart : `service mongod restart`

- start a mongo shell : `mongo`
- launch it from anywhere without connecting : `mongo --nodb`
- connect : `mongo IP:27000/DB --username USER --password PWD --authenticationDatabase DB`


## MongoDB Installation on Debian 9

**1) Import the public key used by the package management system [as root]**

```bash
wget -qO - https://www.mongodb.org/static/pgp/server-4.0.asc | apt-key add -
```

**2) Create a /etc/apt/sources.list.d/mongodb-org-4.0.list file for MongoDB**

```bash
echo "deb http://repo.mongodb.org/apt/debian stretch/mongodb-org/4.0 main" | tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

**3) reload package database**

```bash
sudo apt-get update
```

**4) install latest version of mongodb**

```bash
apt-get install -y mongodb-org
```


source : https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/
