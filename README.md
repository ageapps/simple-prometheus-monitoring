# Simple Prometheus Monitoring
This repo contains a comprehensive example of using prometheus for monitoring a microservice architecture. 

Thanks to `docker-compose`, a stack is deployed with multiple services that are pluggable to the same prometheus stack.
The services monitored are:
+ The Host's docker engine
+ Host instance
+ Containers running in host
+ Rabbit service
+ Nginx service
+ Prometheus itself

In order to deploy the stack the following commands need to be executed:
```bash
# Lauch prometheus + alertmanager services
docker-compose up
# Launch grafana for dashboar representation
docker-compose -f grafana/docker-compose.yaml up
# Launch the rest of services and exporters with docker-compose -f <service_name>/docker-compose.yaml up

docker-compose -f cAdvisor/docker-compose.yaml up
docker-compose -f node-exporter/docker-compose.yaml up
docker-compose -f nginx-vts/docker-compose.yaml up
docker-compose -f RabbitMQ/docker-compose.yaml up
```

## Considerations

These are some helpfull considerations when using this stack.

### Dashboards
Thanks to the Open Source comunity of grafana, there are a lot of \"baked\" dashboards for multiple exporters, the following dashboards are very easy to add to your grafana by using the **import id**. They will probide a very nice overview of the metrics exposed by the exporters.

+ cAdvisor: https://grafana.com/dashboards/193
+ Docker Engine: https://grafana.com/dashboards/1229
+ node exporter: https://grafana.com/dashboards/1860
+ rabbit: https://grafana.com/dashboards/2121
+ nginx: https://github.com/hnlq715/nginx-vts-exporter/blob/master/dashboard/nginx-vts-exporter.json

### Reload prometheus
Prometheus is able to hotreload it's configuration by getting a post request, this can be really helpfull when trying new exporters not needing to reload the prometheus container.
````
curl -X POST http://localhost:9090/-/reload
````
