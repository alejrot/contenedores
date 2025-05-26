



Healthcheck

healthcheck

```yaml
services:
  cloud-broker:
    image: my.docker.registry/activemq-artemis:latest
    healthcheck:
      test: ["CMD-SHELL", "wget http://localhost:8161/ --delete-after --tries=3 2> /dev/null"]
      interval: 10s
      timeout: 5s
      retries: 5
```