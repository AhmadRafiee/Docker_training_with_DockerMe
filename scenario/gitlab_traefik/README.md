

## Check and create networks:

**Run these commands for create gitlab server**

```bash
docker network create web_net
docker network create app_net
```

# run and test all stacks

### create self sign certificate with openssl command and check it
```bash
# certificate location
CERT_LOCATION=./traefik/traefik/certs

# generate key and cert
openssl req -x509 -nodes -newkey \
rsa:4096 -days 365 \
-keyout $CERT_LOCATION/key.pem \
-subj "/C=IR/ST=Iran/L=Tehran/O=DockerMe/OU=IT/CN=DockerMe.ir/emailAddress=rafiee1001@gmail.com" \
-out $CERT_LOCATION/cert.pem 

# cehck certificate
openssl x509 -text -noout -in $CERT_LOCATION/cert.pem

# check directory
tree 
```
### Restricting Access with HTTP Basic Authentication

Creating a Password File:

```bash
# Verify that apache2-utils (Debian, Ubuntu) or httpd-tools (RHEL/CentOS/Oracle Linux) is installed.
apt install apache2-utils

# Username and password http authentication
# htpasswd -nb -s ahmad Docker_training_with_DockerMe | cut -d ":" -f 2
USER_HTTP_AUTH=ahmad
PASS_HTTP_AUTH={SHA}Ju4jbsrOF+ZHxAUONo+N/64DyOM=

``` bash
# Run traefik service  --------------------------------------------------------
# compose file sysntax check
docker-compose -f traefik/docker-compose.yml config

# run compose file
docker-compose -f traefik/docker-compose.yml up -d

# service test 
docker-compose -f traefik/docker-compose.yml ps
docker-compose -f traefik/docker-compose.yml logs -f
# please check service url
curl -I -k https://web.DockerMe.ir


# Run gitlab service  --------------------------------------------------------
# compose file sysntax check
docker-compose -f gitlab/docker-compose.yml config

# run compose file
docker-compose -f gitlab/docker-compose.yml up -d 

# service test 
docker-compose -f gitlab/docker-compose.yml ps
docker-compose -f gitlab/docker-compose.yml logs -f
# please check service url
curl -I -k https://git.DockerMe.ir
```

### gitlab backup create
```bash
# create backup with this command
docker exec -i gitlab gitlab-backup create

# check backup location
DOCKER_ROOT_DIRECTORY=/var/lib/docker
sudo ls $DOCKER_ROOT_DIRECTORY/volumes/gitlab_backup/_data/
```