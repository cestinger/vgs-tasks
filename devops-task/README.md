# Devops task for VGS

This project will spin up a small environment with a load balancer and 2 web servers.  All containers in the environment forward their logs to a centralized log server using fluentd.  Those logs are then copied to an elasticsearch server.  A kibana server is included to view the data.  An ansible control server is also included to apply roles within the environment.

## Traffic flow:

<img src="devops-task.png"
     alt="Traffic flow"
     style="float: left; margin-right: 10px; margin-bottom: 30px" />

## Assumptions and things of Note

This is a test environment only - adding private keys to repositories is not a good idea, so that is only included to allow for ease of building and running this environment to exemplify function.  Same with adding so many extra parts and pieces to the containers as part of roles.  It seems like there is a balancing point with when to run a role vs. when to create a container depending on the install and how much time it takes.

I have learned a lot with docker and ansible in the last couple of days and am already thinking of ways I could do things differently, but had to find a stopping place or I would never be able to turn anything over. :)  This exercise is a bit of a franken-system with parts and pieces from all over the internet.  This was to aid in learning and efficiency to get a working layout.

## Pre-reqs

The machine where this is being run has a linux variant of Docker engine running and an internet connection.

## To create the environment
```
git clone
cd devops-task
choose your environment:
  for prod - cp prod.env .env
  for dev - cp dev.env .env
  for the purposes of this document, we will assume the dev profile has been used
verify environment variables:
  docker-compose config
create local shared volumes:
  mkdir -p /tmp/elasticsearch/data /tmp/fluent/data
docker-compose build
docker-compose up -d
cd env
log onto the ansible control server and finish building out the environment:
  ssh -i ansible -p 2200 ansible@localhost
  cd ansible
  ansible-playbook site.yml
```

## To use the environment
```
Pull up the address to the load balancer to see the test site:
  http://localhost:8001
You can also view the site on each individual container:
  http://localhost:8081
  http://localhost:8082
This will produce log traffic that can be viewed at:
  http://localhost:5061
To ssh to a server in the environment:
  start in devops-task/env
  ssh -i ansible -p ${PORT} ansible@localhost
    where PORT is:
      2200 - control - ansible control servers
      2201 - lb - load balancer
      2211 - web1
      2212 - web2
      2213 - fluentd
      2214 - elasticsearch
      2215 - kibana
```

## To remove the environment
```
docker-compose down
docker-compose rm
```

## Special thanks to
Resources that were of great assistance:
  https://github.com/andymotta/ansible-docker-compose.git
  https://github.com/ansiblebit/oracle-java - because downloading the jdk from Oracle is always so much fun
  https://galaxy.ansible.com - lots of great contributors made it easy to find examples
