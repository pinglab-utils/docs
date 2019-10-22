# Setting Up MongoDB

MongoDB is NOSQL database. THe detail of the installation can be founs at [official website of MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/). Following is th installation steps obtained at 2019-Oct 15.


### Downloading and Installing

**1. Import the public key used by the package management system.**

From a terminal, issue the following command to import the MongoDB public GPG Key from https://www.mongodb.org/static/pgp/server-4.2.asc:

```
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | 
sudo apt-key add -

```

The operation should respond with an OK.

**2. Create a list file for MongoDB.**

Create the list file ```/etc/apt/sources.list.d/mongodb-org-4.2.list``` for your version of Ubuntu.

Click on the appropriate tab for your version of Ubuntu. If you are unsure of what Ubuntu version the host is running, open a terminal or shell on the host and execute ```lsb_release -dc```.

Ubuntu 18.04 (Bionic): The following instruction is for Ubuntu 18.04 (Bionic). For Ubuntu 16.04 (Xenial), click on the appropriate tab. Create the /etc/apt/sources.list.d/mongodb-org-4.2.list file for Ubuntu 18.04 (Bionic):

```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 
multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

***3. Reload local package database.***
Issue the following command to reload the local package database:

```
sudo apt-get update
```

***4. Install the MongoDB packages.***
You can install either the latest stable version of MongoDB or a specific version of MongoDB.

Install the latest version of MongoDB.	Install a specific release of MongoDB.
To install the latest stable version, issue the following

```
sudo apt-get install -y mongodb-org
```

### Sample application with Python:

- Install the python package ``pymongo`` in the environment

```
pip install pymongo
```

- Create Database and Collection

```python
import pymongo
client = pymongo.MongoClient("mongodb://localhost:27017/")
db = client["test"]
collection = db["test_collection"]
```
- Next, Populate the collection

```python
x = collection.insert_many(DATA)

```
- Print out one sample data:

```python
x = collection.find_one()
print(x) 
```

-------------------------






### Read More
1. [MongoDB Documents](https://docs.mongodb.com/manual/core/document/)
2. [MongoDB Tutorials -I](https://docs.mongodb.com/manual/tutorial/)
2. [MongoDB Tutorials Point](https://www.tutorialspoint.com/mongodb/index.htm)







