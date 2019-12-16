# Docker-Stack
Files for running Elastic (ELK) with docker-compose on Ubuntu


## Elastic and Kibana on Docker

### Required Files

Create a folder called elk and copy the following files into it:

```
instances.yml
.env
create-certs.yml
docker-compose.yml
```

Inside the elk folder create the following folders:

```
certs
data01
data02
data03
``` 
 
### Adjust the Virtual Memory for Docker
For Linux:

```
sudo sysctl -w vm.max_map_count=262144
```

### Generate Certificates for Elasticsearch

```
docker-compose -f create-certs.yml run --rm create_certs
```

### Create passwords
Edit the docker-compose.yml file
Line comment out the Kibana section.  This can be done by adding a # in the front of each line.
This needs to be done as Kibana will not start up until we create a password for it.

### Start a three-node ES cluster by typing:

```
docker-compose -f docker-compose.yml up
``` 

Open a new terminal window and type the following to generate a password (single command line):

```
docker exec es01 /bin/bash -c "bin/elasticsearch-setup-passwords auto --batch -Expack.security.http.ssl.certificate=certificates/es01/es01.crt -Expack.security.http.ssl.certificate_authorities=certificates/ca/ca.crt -Expack.security.http.ssl.key=certificates/es01/es01.key --url https://localhost:9200"
```

This will generate all the passwords.  Make sure to make a copy of these.  
 
### Edit Docker-Compose file
Remove the Line comments that were added in the previous section for Kibana.
Change the password for ELASTICSEARCH_PASSWORD:  with the one generated from the previous section for the Kibana user.
 
### Starting ELK

Shutdown the previous Docker ELK session by typing:

```
docker-compose down
``` 

Start a new docker session by typing:

```
docker-compose up
```

Wait a minute or two and you should be able to access kibana at 

```
https:\\server-ip\
```

Login with the elastic user and the password generated from earlier.

### Additional Notes

docker-compose can be ran in the background using the -d switch, such as:
 
```
docker-compose up -d
```

Docker version can be changed by editing the VERSION variable in the .env file. 
