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

There is also a journalctl.yaml file that can be used with the `otelcol-contrib` package. There are instructions for installing otel-contrib [here](https://opentelemetry.io/docs/collector/installation/#deb-installation). Copy this file over `/etc/otelcol-contrib/config.yaml`. Make sure that your upstream listener is listening for otlp over http and that you have cors set up. You will also need to give it permissions to access journalctl with `sudo usermod -aG systemd-journal otelcol-contrib`. When you have performed the preceeding steps you should be able to do `systemctl restart otelcol-contrib`.

This is a useful setup for a scenario with a legacy java spring boot app and some microfrontends. It is installed on a truenas connected to the internet with a cloudflared tunnel. Hopefully it is also helpful for learning open telemetry as a student.

