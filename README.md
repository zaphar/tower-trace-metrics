<!--
 Copyright 2023 Jeremy Wall (Jeremy@marzhilsltudios.com)
 
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 
     http://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->
# tower-trace-metrics

A [tower-http](https://crates.io/crates/tower-http) `TraceLayer` that records metrics using the [metrics](https://crates.io/crates/metrics) facade.

```rust
use tower_trace_metrics::make_layer;
use tower::ServiceBuilder;
use bytes;


fn main() {
    let service = ServiceBuilder::new()
        // Make a trace layer where the chunks are bytes::Bytes
        .layer(make_layer(|b: &bytes::Bytes| b.len() as u64));
    // ... Use this service in a Tower middleware stack.
}
```

## Recorded Metrics

* http_request_counter The total count of all requests
* http_request_failure_counter The total count of all request failures
* http_request_size_bytes_hist A histogram of request size in bytes
* http_request_request_time_micros_hist A histogram of request time in microseconds

Each metric is faceted by `path`, `method`, and `host` for the request.