- Installing `PyMongo` MongoDB low level driver

```bash
   $ python3 -m pip install pymongo
```

- Connecting to DB

```bash
   $ python3
```

```
   >>> from pymongo import MongoClient
   >>> from pprint import pprint
   >>> client = MongoClient()
   >>> dbh = client["SampleCollections"]
   >>> results = dbh.find().limit(100);
   >>> for result in results:
   >>>     print result
```

- Installing `PyMODM ODM` MongoDB Object Document Mapper

```bash
   $ pip3 install pymodm
```

- Connecting to DB, use [Sample collections for MongoDB](https://github.com/SouthbankSoftware/dbkoda-data)
  - connecting with credentials -> connect("mongodb://[username:password@]localhost:27017/SampleCollections", alias="sample-collections")

```bash
   $ python3
```

```
   >>> from pymodm import connect
   >>> connect("mongodb://localhost:27017/SampleCollections", alias="sample-collections")
```
