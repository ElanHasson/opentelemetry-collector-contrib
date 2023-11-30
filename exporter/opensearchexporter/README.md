# OpenSearch Exporter

<!-- status autogenerated section -->
| Status        |           |
| ------------- |-----------|
| Stability     | [alpha]: traces   |
|               | [development]: logs   |
| Distributions | [] |
| Issues        | [![Open issues](https://img.shields.io/github/issues-search/open-telemetry/opentelemetry-collector-contrib?query=is%3Aissue%20is%3Aopen%20label%3Aexporter%2Fopensearch%20&label=open&color=orange&logo=opentelemetry)](https://github.com/open-telemetry/opentelemetry-collector-contrib/issues?q=is%3Aopen+is%3Aissue+label%3Aexporter%2Fopensearch) [![Closed issues](https://img.shields.io/github/issues-search/open-telemetry/opentelemetry-collector-contrib?query=is%3Aissue%20is%3Aclosed%20label%3Aexporter%2Fopensearch%20&label=closed&color=blue&logo=opentelemetry)](https://github.com/open-telemetry/opentelemetry-collector-contrib/issues?q=is%3Aclosed+is%3Aissue+label%3Aexporter%2Fopensearch) |
| [Code Owners](https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/main/CONTRIBUTING.md#becoming-a-code-owner)    | [@Aneurysm9](https://www.github.com/Aneurysm9), [@MitchellGale](https://www.github.com/MitchellGale), [@MaxKsyunz](https://www.github.com/MaxKsyunz), [@YANG-DB](https://www.github.com/YANG-DB) |

[alpha]: https://github.com/open-telemetry/opentelemetry-collector#alpha
[development]: https://github.com/open-telemetry/opentelemetry-collector#development
<!-- end autogenerated section -->

OpenSearch exporter supports sending OpenTelemetry signals as documents to [OpenSearch](https://www.opensearch.org).

The documents are sent using [observability catalog](https://github.com/opensearch-project/opensearch-catalog/tree/main/schema/observability) schema.

## Configuration options
### Indexing Options
- `dataset` (default=`default`) a user-provided label to classify source of telemetry. It is used to construct the name of the destination index or data stream.
- `namespace` (default=`namespace`) a user-provided label to group telemetry. It is used to construct the name of the destination index or data stream.

### HTTP Connection Options
OpenSearch export supports standard [HTTP client settings](https://github.com/open-telemetry/opentelemetry-collector/tree/main/config/confighttp#client-configuration).
- `http.endpoint` (required) `<url>:<port>` of OpenSearch node to send data to.

### TLS settings
Supports standard TLS settings as part of HTTP settings. See [TLS Configuration/Client Settings](https://github.com/open-telemetry/opentelemetry-collector/blob/main/config/configtls/README.md#client-configuration).

### Retry Options
- `retry_on_failure`: See [retry_on_failure](https://github.com/open-telemetry/opentelemetry-collector/blob/main/exporter/exporterhelper/README.md)

### Timeout Options
- `timeout` : See [timeout](https://github.com/open-telemetry/opentelemetry-collector/blob/main/exporter/exporterhelper/README.md)

### Bulk Indexer Options
- `bulk_action` (optional): the [action](https://opensearch.org/docs/2.9/api-reference/document-apis/bulk/) for ingesting data. Only `create` and `index` are allowed here. 
## Example

```yaml
extensions:
  basicauth/client:
  client_auth:
    username: username
    password: password
    
exporters:
  opensearch/trace:
    http:
      endpoint: https://opensearch.example.com:9200
      auth:
        authenticator: basicauth/client
# ······
service:
  pipelines:
    traces:
      receivers: [otlp]
      exporters: [opensearch/trace]
      processors: [batch]
```