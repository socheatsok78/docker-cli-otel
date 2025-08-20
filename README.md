# About
A support package for instrumenting Docker CLI and Docker Buildx with OpenTelemetry

## Usgae

```sh
docker run -p 3000:3000 -p 4317:4317 -p 4318:4318 --rm -ti grafana/otel-lgtm
```

See https://grafana.com/blog/2024/03/13/an-opentelemetry-backend-in-a-docker-image-introducing-grafana/otel-lgtm/ for more information about the OpenTelemetry backend in a Docker image.

## Docker CLI

The Docker CLI supports OpenTelemetry instrumentation for emitting metrics about command invocations. This is disabled by default. You can configure the CLI to start emitting metrics to the endpoint that you specify. This allows you to capture information about your docker command invocations for more insight into your Docker usage.

The Docker CLI doesn't emit telemetry data by default. Only if you've set an environment variable on your system will Docker CLI attempt to emit OpenTelemetry metrics, to the endpoint that you specify.

```sh
export DOCKER_CLI_OTEL_EXPORTER_OTLP_ENDPOINT=http://0.0.0.0:4318
```

See https://docs.docker.com/engine/cli/otel/ for more information about the OpenTelemetry support in Docker CLI.

## BuildKit

BuildKit supports [OpenTelemetry](https://opentelemetry.io/) for buildkitd gRPC
API and buildctl commands. To capture the trace to
[OpenTelemetry](https://opentelemetry.io/), set `OTEL_EXPORTER_OTLP_ENDPOINT`
environment variable to the collection address.

```sh
export OTEL_EXPORTER_OTLP_ENDPOINT=http://0.0.0.0:4318
# restart buildkitd and buildctl so they know OTEL_EXPORTER_OTLP_ENDPOINT
# any buildctl command should be traced to http://127.0.0.1:3000/
```

See https://docs.docker.com/build/debug/opentelemetry/ for more information about the OpenTelemetry support in BuildKit.

## Troubleshooting

> [!NOTE]
> I find that setting the environment variable `OTEL_EXPORTER_OTLP_ENDPOINT` to `http://0.0.0.0:4318` works best for my environment running macOS. You can set it `127.0.0.1` or `localhost` as well, following the official documentation.
