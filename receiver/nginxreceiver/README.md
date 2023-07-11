# Nginx Receiver

<!-- status autogenerated section -->
| Status        |           |
| ------------- |-----------|
| Stability     | [beta]: metrics   |
| Distributions | [contrib], [observiq], [sumo] |
| Issues        | ![Open issues](https://img.shields.io/github/issues-search/open-telemetry/opentelemetry-collector-contrib?query=is%3Aissue%20is%3Aopen%20label%3Areceiver%2Fnginx%20&label=open&color=orange&logo=opentelemetry) ![Closed issues](https://img.shields.io/github/issues-search/open-telemetry/opentelemetry-collector-contrib?query=is%3Aissue%20is%3Aclosed%20label%3Areceiver%2Fnginx%20&label=closed&color=blue&logo=opentelemetry) |

[beta]: https://github.com/open-telemetry/opentelemetry-collector#beta
[contrib]: https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol-contrib
[observiq]: https://github.com/observIQ/observiq-otel-collector
[sumo]: https://github.com/SumoLogic/sumologic-otel-collector
<!-- end autogenerated section -->

This receiver can fetch stats from a Nginx instance using the `ngx_http_stub_status_module` module's `status` endpoint.

## Details

## Configuration

### Nginx Module
You must configure NGINX to expose status information by editing the NGINX
configuration.  Please see
[ngx_http_stub_status_module](http://nginx.org/en/docs/http/ngx_http_stub_status_module.html)
for a guide to configuring the NGINX stats module `ngx_http_stub_status_module`.

### Receiver Config

> :information_source: This receiver is in beta and configuration fields are subject to change.

The following settings are required:

- `endpoint` (default: `http://localhost:80/status`): The URL of the nginx status endpoint

The following settings are optional:

- `collection_interval` (default = `10s`): This receiver runs on an interval.
Each time it runs, it queries nginx, creates metrics, and sends them to the
next consumer. The `collection_interval` configuration option tells this
receiver the duration between runs. This value must be a string readable by
Golang's `ParseDuration` function (example: `1h30m`). Valid time units are
`ns`, `us` (or `µs`), `ms`, `s`, `m`, `h`.
- `initial_delay` (default = `1s`): defines how long this receiver waits before starting.

Example:

```yaml
receivers:
  nginx:
    endpoint: "http://localhost:80/status"
    collection_interval: 10s
```

The full list of settings exposed for this receiver are documented [here](./config.go)
with detailed sample configurations [here](./testdata/config.yaml).

## Feature gate configurations

See the [Collector feature gates](https://github.com/open-telemetry/opentelemetry-collector/blob/main/featuregate/README.md#collector-feature-gates) for an overview of feature gates in the collector.

**ALPHA**: `receiver.nginx.emitCurrentConnectionsAsSum`

The feature gate `receiver.nginx.emitConnectionsCurrentAsSum` once enabled will change the data type of the
`nginx.connections_current` metric from a gauge to a non-monotonic sum.

This feature gate will eventually be enabled by default, and eventually the old implementation will be removed. It aims
to give users time to migrate to the new implementation. The target release for this featuregate to be enabled by default
is 0.80.0.