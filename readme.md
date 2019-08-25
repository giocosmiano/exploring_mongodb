- [MongoDB - Cross-platform NoSQL/Document-Oriented DB](https://docs.mongodb.com/manual/)
  - [Documentation](https://docs.mongodb.com/)
  - [Introduction](https://docs.mongodb.com/manual/introduction/)
  - [How to Install MongoDB on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-18-04)

- Installing `mongodb` in `Ubuntu 18.04 x64`
  - [Download MongoDB Community Server](https://www.mongodb.com/download-center/community)
    - [Click the `Download` button for installation package - mongodb-org-server_4.2.0_amd64.deb](https://www.mongodb.com/download-center/community)
    - [Download the latest binary](https://www.mongodb.org/dl/linux/x86_64-ubuntu1804)
  - [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

```bash
sudo dpkg -i mongodb-org-server_4.2.0_amd64.deb
```

- Add these settings to `.bashrc` for personal preference

```bash
export APPLICATIONS_HOME="${HOME}/Documents/_applications"
export MONGODB_HOME="${APPLICATIONS_HOME}/mongodb-linux-x86_64-ubuntu1804-4.2.0"
export MONGODB_BIN="${MONGODB_HOME}/bin"

alias mongo-restart='sudo systemctl restart mongodb'
alias mongo-start='sudo systemctl start mongodb'
alias mongo-stop='sudo systemctl stop mongodb'
alias mongo-status='sudo systemctl status mongodb'
alias mongo-enable='sudo systemctl enable mongodb'
alias mongo-disable='sudo systemctl disable mongodb'
```

- Configure to start `mongodb` automatically with the server

```bash
sudo cp mongodb.service /etc/systemd/system
```

- Update the `systemd` service after copying `mongodb` services 

```bash
sudo systemctl daemon-reload
```

- Enable to auto-start `mongodb` service

```bash
mongo-enable
```

- Start/Re-Start `mongodb` service

```bash
mongo-start
mongo-restart
```

- Issue installing MongoDB in `Ubuntu 18.04`
  - [MongoDB Zip Installation Failed in Ubuntu 18.04](https://stackoverflow.com/questions/51006934/mongodb-zip-installation-failed-in-ubuntu-18-04)

- Installing `mongodb` UI
  - [dbKoda](https://www.dbkoda.com/)
  - [NoSQLBooster for MongoDB](https://nosqlbooster.com/downloads)
  - [MongoDB Compass](https://www.mongodb.com/download-center/compass)
    - [Click the `Download` button for installation package - mongodb-compass_1.19.6_amd64.deb](https://www.mongodb.com/download-center/compass)

- Sample MongoDB data sets
  - [MongoDB Atlas Sample Datasets](https://docs.atlas.mongodb.com/sample-data/available-sample-datasets/)
    - [Sample AirBnB Listings Dataset](https://docs.atlas.mongodb.com/sample-data/sample-airbnb/#sample-airbnb)
  - [MongoDB sample Collections](https://medium.com/dbkoda/mongodb-sample-collections-52d6a7745908)
  - [Sample collections for MongoDB](https://github.com/SouthbankSoftware/dbkoda-data)
  - [MongoDB json files](https://github.com/ozlerhakan/mongodb-json-files)
