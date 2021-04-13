# MongoDB on Ubuntu
Some quick notes on working with MongoDB on Ubuntu. 
Many recommend running MongoDB on a container rather than installing the db on OS. 

## Installing Mongo
`sudo apt install gnupg` 
`gnupg` is an encryption software. 

Import key using wget: 
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -
```
`wget` retrieves files via internet protocols: HTTP, HTTPS, FTP and FTPS. 
Option `-qO` converts the output to a file 
Then pipe this file to `apt-key add` as a package key. 

Create list file: 
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list
```

Reload package database: `sudo apt update`

Install MongoDB: `sudo apt install -y mongodb-org`

## Running MongoDB
`/var/lib/mongdb` and `/var/log/mongdb` are created automatically via package manager.
**Note: ** These are absolute paths, not paths under user directory.

Start MongoDB: 
`sudo systemctl start mongod`
If there's an error, try `sudo systemctl daemon-reload`

Verify status: 
`sudo systemctl status mongod`

Stop MongoDB: 
`sudo systemctl stop mongod`

Restart: 
`sudo systemctl restart mongod`

## Start shell after `mongod`
`mongo`


