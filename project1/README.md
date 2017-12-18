## To run:
```
	clone repo on machine that has docker installed
	cd into project1 and run the following:
	mkdir -p /tmp/fluent/data /tmp/docker/data /tmp/elasticsearch/data
	docker swarm init
	docker stack deploy -c docker-compose.yml project1
```

## This will provide the following:
```
	http://localhost:80 - test website from docker tutorials with redis counter 
	http://localhost:6379 - redis 
	http://localhost:24224 - all logs from web and redis containers are forwarded to fluentd
	http://localhost:9200 - logs that go to fluentd are then forward through to elasticsearch
	http://localhost:5601 - to see the logs in kibana - 
		set up the default mapping to be files that start with fluentd-*
		take the default timestamp
```

## To remove:
```
	docker stack rm project1
	docker swarm leave --force
```
