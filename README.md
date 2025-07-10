# Docker-infra
docker logs and hostmetrics for signoz

```bash
git clone --recurse-submodules
patch  signoz/deploy/docker/docker-compose.yaml  <patch.diff
```

This is based on [the open telemetry docs](https://opentelemetry.io/docs/languages/python/getting-started/)

run `docker-compose up`. In another terminal window run:

```bash
export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true
opentelemetry-instrument --logs_exporter otlp flask run -p 8080
```
or for tracing, metric monitoring and logging


```bash
export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true
opentelemetry-instrument --logs_exporter otlp --metrics_exporter otlp --traces_exporter otlp --service_name your-service-name flask run -p 8081
```

