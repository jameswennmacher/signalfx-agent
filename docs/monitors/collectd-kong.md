<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# collectd/kong

Monitor Type: `collectd/kong` ([Source](https://github.com/signalfx/signalfx-agent/tree/main/pkg/monitors/collectd/kong))

**Accepts Endpoints**: **Yes**

**Multiple Instances Allowed**: Yes

## Overview

Monitors a Kong instance using
[collectd-kong](https://github.com/signalfx/collectd-kong).  The Smart
Agent includes collectd and this plugin as part of the standard
installation, so no additional installation is required once you have the
Smart Agent.

The SignalFx Kong collectd plugin provides users with the ability to gather
and report their service traffic metrics with collectd, in tandem with
[kong-plugin-signalfx](https://github.com/signalfx/kong-plugin-signalfx).

This plugin emits metrics for configurable request/response lifecycle groups including:

* Counters for response counts
* Counters for cumulative response and request sizes
* Counters for cumulative request, upstream, and Kong latencies

These request/response lifecycle groups can be optionally partitioned by tunable levels of granularity by:

* API or Service Name/ID
* Route ID
* Request HTTP Method
* Response HTTP Status Code

In addition to these groups, system-wide connection stats can be provided, including:

* A counter for total fielded requests
* Gauges for active connections and their various states
* A gauge for database connectivity

The `metrics` field below is populated with a set of metrics that are
described at https://github.com/signalfx/collectd-kong/blob/master/README.md.

<!--- SETUP --->
### Install Kong Lua Plugin

Please download and install this Lua module on all Kong servers by
following [these instructions](https://github.com/signalfx/kong-plugin-signalfx/blob/master/README.md).

### REQUIREMENTS AND DEPENDENCIES

This plugin requires:

| Software          | Version        |
|-------------------|----------------|
| Kong Community Edition (CE) | 0.11.2+ |
| Configured [kong-plugin-signalfx](https://github.com/signalfx/kong-plugin-signalfx) | 0.0.1+ |


<!--- SETUP --->
## Example Config
#
Sample YAML configuration:

```yaml
monitors:
  - type: collectd/kong
    host: 127.0.0.1
    port: 8001
    metrics:
      - metric: request_latency
        report: true
      - metric: connections_accepted
        report: false
```

Sample YAML configuration with custom /signalfx route and filter lists

```yaml
monitors:
  - type: collectd/kong
    host: 127.0.0.1
    port: 8443
    url: https://127.0.0.1:8443/routed_signalfx
    authHeader:
      header: Authorization
      value: HeaderValue
    metrics:
      - metric: request_latency
        report: true
    reportStatusCodeGroups: true
    statusCodes:
      - 202
      - 403
      - 405
      - 419
      - "5*"
    serviceNamesBlacklist:
      - "*SomeService*"
```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: collectd/kong
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.md#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `pythonBinary` | no | `string` | Path to a python binary that should be used to execute the Python code. If not set, a built-in runtime will be used.  Can include arguments to the binary as well. |
| `host` | **yes** | `string` | Kong host to connect with (used for autodiscovery and URL) |
| `port` | **yes** | `integer` | Port for kong-plugin-signalfx hosting server (used for autodiscovery and URL) |
| `name` | no | `string` | Registration name when using multiple instances in Smart Agent |
| `url` | no | `string` | kong-plugin-signalfx metric plugin (**default:** `http://{{.Host}}:{{.Port}}/signalfx`) |
| `authHeader` | no | `object (see below)` | Header and its value to use for requests to SFx metric endpoint |
| `verifyCerts` | no | `bool` | Whether to verify certificates when using ssl/tls (**default:** `false`) |
| `caBundle` | no | `string` | CA Bundle file or directory |
| `clientCert` | no | `string` | Client certificate file (with or without included key) |
| `clientCertKey` | no | `string` | Client cert key if not bundled with clientCert |
| `verbose` | no | `bool` | Whether to use debug logging for collectd-kong (**default:** `false`) |
| `metrics` | no | `list of objects (see below)` | List of metric names and report flags. See monitor description for more details. |
| `reportApiIds` | no | `bool` | Report metrics for distinct API IDs where applicable (**default:** `false`) |
| `reportApiNames` | no | `bool` | Report metrics for distinct API names where applicable (**default:** `false`) |
| `reportServiceIds` | no | `bool` | Report metrics for distinct Service IDs where applicable (**default:** `false`) |
| `reportServiceNames` | no | `bool` | Report metrics for distinct Service names where applicable (**default:** `false`) |
| `reportRouteIds` | no | `bool` | Report metrics for distinct Route IDs where applicable (**default:** `false`) |
| `reportHttpMethods` | no | `bool` | Report metrics for distinct HTTP methods where applicable (**default:** `false`) |
| `reportStatusCodeGroups` | no | `bool` | Report metrics for distinct HTTP status code groups (eg. "5xx") where applicable (**default:** `false`) |
| `reportStatusCodes` | no | `bool` | Report metrics for distinct HTTP status codes where applicable (mutually exclusive with ReportStatusCodeGroups) (**default:** `false`) |
| `apiIds` | no | `list of strings` | List of API ID patterns to report distinct metrics for, if reportApiIds is false |
| `apiIdsBlacklist` | no | `list of strings` | List of API ID patterns to not report distinct metrics for, if reportApiIds is true or apiIds are specified |
| `apiNames` | no | `list of strings` | List of API name patterns to report distinct metrics for, if reportApiNames is false |
| `apiNamesBlacklist` | no | `list of strings` | List of API name patterns to not report distinct metrics for, if reportApiNames is true or apiNames are specified |
| `serviceIds` | no | `list of strings` | List of Service ID patterns to report distinct metrics for, if reportServiceIds is false |
| `serviceIdsBlacklist` | no | `list of strings` | List of Service ID patterns to not report distinct metrics for, if reportServiceIds is true or serviceIds are specified |
| `serviceNames` | no | `list of strings` | List of Service name patterns to report distinct metrics for, if reportServiceNames is false |
| `serviceNamesBlacklist` | no | `list of strings` | List of Service name patterns to not report distinct metrics for, if reportServiceNames is true or serviceNames are specified |
| `routeIds` | no | `list of strings` | List of Route ID patterns to report distinct metrics for, if reportRouteIds is false |
| `routeIdsBlacklist` | no | `list of strings` | List of Route ID patterns to not report distinct metrics for, if reportRouteIds is true or routeIds are specified |
| `httpMethods` | no | `list of strings` | List of HTTP method patterns to report distinct metrics for, if reportHttpMethods is false |
| `httpMethodsBlacklist` | no | `list of strings` | List of HTTP method patterns to not report distinct metrics for, if reportHttpMethods is true or httpMethods are specified |
| `statusCodes` | no | `list of strings` | List of HTTP status code patterns to report distinct metrics for, if reportStatusCodes is false |
| `statusCodesBlacklist` | no | `list of strings` | List of HTTP status code patterns to not report distinct metrics for, if reportStatusCodes is true or statusCodes are specified |


The **nested** `authHeader` config object has the following fields:

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `header` | **yes** | `string` | Name of header to include with GET |
| `value` | **yes** | `string` | Value of header |


The **nested** `metrics` config object has the following fields:

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `metric` | **yes** | `string` | Name of metric, per collectd-kong |
| `report` | **yes** | `bool` | Whether to report this metric |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


 - `counter.kong.connections.accepted` (*cumulative*)<br>    Total number of all accepted connections.
 - `counter.kong.connections.handled` (*cumulative*)<br>    Total number of all handled connections (accounting for resource limits).
 - ***`counter.kong.kong.latency`*** (*cumulative*)<br>    Time spent in Kong request handling and balancer (ms).
 - ***`counter.kong.requests.count`*** (*cumulative*)<br>    Total number of all requests made to Kong API and proxy server.
 - ***`counter.kong.requests.latency`*** (*cumulative*)<br>    Time elapsed between the first bytes being read from each client request and the log writes after the last bytes were sent to the clients (ms).
 - ***`counter.kong.requests.size`*** (*cumulative*)<br>    Total bytes received/proxied from client requests.
 - ***`counter.kong.responses.count`*** (*cumulative*)<br>    Total number of responses provided to clients.
 - ***`counter.kong.responses.size`*** (*cumulative*)<br>    Total bytes sent/proxied to clients.
 - ***`counter.kong.upstream.latency`*** (*cumulative*)<br>    Time spent waiting for upstream response (ms).
 - ***`gauge.kong.connections.active`*** (*gauge*)<br>    The current number of active client connections (includes waiting).
 - ***`gauge.kong.connections.reading`*** (*gauge*)<br>    The current number of connections where nginx is reading the request header.
 - ***`gauge.kong.connections.waiting`*** (*gauge*)<br>    The current number of idle client connections waiting for a request.
 - ***`gauge.kong.connections.writing`*** (*gauge*)<br>    The current number of connections where nginx is writing the response back to the client.
 - ***`gauge.kong.database.reachable`*** (*gauge*)<br>    kong.dao:db.reachable() at time of metric query

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.



