# Nginx Integration

This integration periodically fetches metrics from [https://nginx.org/](Nginx) servers.


## Compatibility

The Nginx stubstatus metrics were tested with Nginx 1.9 and are expected to work with all version >= 1.9. The logs were tested with version 1.10. On Windows, the module was tested with Nginx installed from the Chocolatey repository.


## Logs


**Timezone support**

This datasource parses logs that don’t contain timezone information. For these logs, the Elastic Agent reads the local timezone and uses it when parsing to convert the timestamp to UTC. The timezone to be used for parsing is included in the event in the event.timezone field.

To disable this conversion, the event.timezone field can be removed with the drop_fields processor.

If logs are originated from systems or applications with a different timezone to the local one, the event.timezone field can be overwritten with the original timezone using the add_fields processor.

### Access Logs

Access logs collects the nginx access logs.

**Exported fields**

| Field | Description | Type |
|---|---|---|
| nginx.access.remote_ip_list | An array of remote IP addresses. It is a list because it is common to include, besides the client IP address, IP addresses from headers like `X-Forwarded-For`. Real source IP is restored to `source.ip`. | array |


### Error Logs

Error logs collects the nginx error logs.

**Exported fields**

| Field | Description | Type |
|---|---|---|
| nginx.error.connection_id | Connection identifier. | long |


## Metrics

### Stubstatus Metrics

The Nginx stubstatus stream collects data from the Nginx ngx_http_stub_status module. It scrapes the server status data from the web page generated by ngx_http_stub_status.

This is a default stream. If the host datasource is unconfigured, this stream is enabled by default.

An example event for nginx looks as following:

```$json
{
    "@timestamp": "2017-10-12T08:05:34.853Z",
    "agent": {
        "hostname": "host.example.com",
        "name": "host.example.com"
    },
    "event": {
        "dataset": "nginx.stubstatus",
        "duration": 115000,
        "module": "nginx"
    },
    "metricset": {
        "name": "stubstatus"
    },
    "nginx": {
        "stubstatus": {
            "accepts": 6254,
            "active": 2,
            "current": 1,
            "dropped": 0,
            "handled": 6254,
            "hostname": "127.0.0.1",
            "reading": 0,
            "requests": 6259,
            "waiting": 1,
            "writing": 1
        }
    },
    "service": {
        "address": "127.0.0.1",
        "type": "nginx"
    }
}
```

**Exported fields**

| Field | Description | Type |
|---|---|---|
| nginx.stubstatus.accepts | The total number of accepted client connections. | long |
| nginx.stubstatus.active | The current number of active client connections including Waiting connections. | long |
| nginx.stubstatus.current | The current number of client requests. | long |
| nginx.stubstatus.dropped | The total number of dropped client connections. | long |
| nginx.stubstatus.handled | The total number of handled client connections. | long |
| nginx.stubstatus.hostname | Nginx hostname. | keyword |
| nginx.stubstatus.reading | The current number of connections where Nginx is reading the request header. | long |
| nginx.stubstatus.requests | The total number of client requests. | long |
| nginx.stubstatus.waiting | The current number of idle client connections waiting for a request. | long |
| nginx.stubstatus.writing | The current number of connections where Nginx is writing the response back to the client. | long |
