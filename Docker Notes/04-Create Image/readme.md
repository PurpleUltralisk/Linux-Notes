# Create Image
To create an image for an web app: 
1. List all the linux commands required to run the web app
2. Write these commands into a `docker file`

A docker file takes the following format: 
[instruction] [argument]
**Note**: Every image must be based off of another image

```docker
FROM Ubuntu

RUN apt update
RUN apt install python
RUN pip install flask
RUN pip install flask-mysql

COPY ./opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

## Build Image
`docker build [filename] -t [tag name]`

## Push to docker hub
`docker push [tag name]`

