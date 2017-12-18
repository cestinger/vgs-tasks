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
	test website - http://localhost:80
	kibana - http://localhost:5601
	elasticsearch - http://localhost:9200
	redis - http://localhost:6379
	fluentd - http://localhost:24224
```
