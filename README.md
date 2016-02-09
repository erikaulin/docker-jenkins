# Docker - Jenkins

Jenkins container

## Usage

`git clone git@github.com:erikaulin/docker-jenkins.git`

Run `docker-compose up -d` After a minute Jenkins has been configured and nginx should be listening on ports 80.

#### docker-compose.yml
```
version: '2'
services:
  jenkins:
    image: blacklabelops/jenkins
    container_name: jenkins
    volumes_from:
      - volumes
    env_file:
      - conf/jenkins-master.env
  nginx:
    container_name: nginx
    build: ./nginx/
    ports:
      - "80:80"
  volumes:
    image: blacklabelops/centos
    container_name: jenkins_data
    volumes:
      # Logging volumes
      - /var/log
      # Jenkins volume
      - /jenkins
    command: chown -R 1000:1000 /var/log /jenkins
```

#### jenkins-master.env
```
# Setting up the admin account and basic security
JENKINS_ADMIN_USER=jenkins
JENKINS_ADMIN_PASSWORD=p4ssw0rd
# Specify the Java VM parameters
# See: http://www.oracle.com/technetwork/articles/java/vmoptions-jsp-140102.html
JAVA_VM_PARAMETERS=-Xmx1024m -Xms512m
# Number of executors on Jenkins master.
JENKINS_MASTER_EXECUTORS=0
# Whitespace separated list of required plugins.
# Example: gitlab-plugin hipchat swarm
JENKINS_PLUGINS=swarm git unity3d-plugin github envinject log-parser simple-theme-plugin gravatar
# Jenkins port for accepting swarm slave connections
JENKINS_SLAVEPORT=50000
# Jenkins startup parameters.
# See: https://wiki.jenkins-ci.org/display/JENKINS/Starting+and+Accessing+Jenkins
JENKINS_PARAMETERS=
# Jenkins Mail Setup
#SMTP_USER_NAME=
#SMTP_USER_PASS=
#SMTP_HOST=
#SMTP_PORT=
#SMTP_REPLYTO_ADDRESS=
#SMTP_USE_SSL=no
#SMTP_CHARSET=
# Jenkins log file. Not necessary, because Jenkins logs to Docker.
LOG_FILE=
```

## Maintainers

* [Erik Aulin](mailto:erik@aulin.co)
