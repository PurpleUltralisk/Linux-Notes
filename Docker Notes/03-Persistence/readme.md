# Data Persistence

## Standard Input/Output
Docker is not connected to standard input and output by default. So we need to connect it with the terminal. 
`docker run it [container]`

## Port Mapping
When running a app image on a server, the public cannot access it. 
We need to map the server's port 80 with the container port 5000. 
`docker run -p 80:5000 [container]`

## Persist Data
Data sits inside the container, if we remove the container the data is gone as well. 
To persist the data, we need to map a directory. 
Here we are mapping `/opt/datadir` to the mysql dir
`docker run -v /opt/datadir:/var/lib/mysql [container]`
