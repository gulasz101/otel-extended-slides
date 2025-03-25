---
theme: white
enableTimeBar: "true"
timeForPresentation: "1500"
defaultTemplate: "[[tpl-default]]"
---

<grid drag="60 55" drop="5 10">
From Chaos to Clarity: How OpenTelemetry Makes Microservice Monitoring Effortless
</grid>


<grid drag="40 45" drop="-5 -10">
![[opentelemetry-horizontal-color.png]]
</grid>
---
Agenda:
- introduction
- high level otel overview
- overview of main components
- tooling
- state of instrumentation libs

---

No demo 🙃

![[i-m-tired-boss.png|600]]

notes:
sadly my laptop does not have greatest time running demo project

https://opentelemetry.io/docs/demo/docker-deployment/
---

<grid drag="90 20" drop="5 5">
# Who Am I
</grid>


- Wojtek
- 36 years old
- Polish
- WMS odoo team lead


<grid drag="20 10" drop="1 -10">
![[metro-marktplatz.png|100]]
</grid>

<grid drag="20 10" drop="28 -10">
![[smallinvoice.png|100]]
</grid>

<grid drag="20 10" drop="-26 -10">
![[pingen.ch.png|100]]
</grid>

<grid drag="20 10" drop="-1 -10">
![[watson.ch.png|100]]
</grid>

notes:
- working with metro markets since Feb 2021
- currently working as Warehouse Management System team lead
- Was working as a member of software operations, where was early adopting opentelemetry for php apps, when it was still a beta
- otel for PHP became stable only few days after I left for new role

https://metro-markets.de/careers/

---

<grid drag="90 20" drop="5 0">
Why we are here?
</grid>

<grid drag="60 55" drop="10 20">
<video type="video/mp4" controls><source src="microservices.mp4"></video>
</grid>


<grid drag="40 45" drop="-5 -15">
![[microservicesqr.png]]
</grid>

notes:
This is video when it came out we were all finding extremely funny.
Why?
Because it was reflecting out daily pain.

But there is something missing in this pain. 
It is easy to make jokes from contracts and company internal communication.

But from what is really hard to make jokes? It's from "cost of ownership".
We almost never joke from this topic, because debugging distributed services is extremely difficult.

Or is it?

https://www.youtube.com/watch?v=y8OnoxKotPQ

---

![[opentelemetry-horizontal-color.png]]

+ traces;
+ metrics;
+ logs;

notes:
So here to solve (or at least aim to help us solving those problems) comes opentelemetry. Vendor agnostic framework which provides us set of APIs and SDKs that we can use as a language for observability helping us to collect:




---
 <grid drag="70 20" drop="20 -97">
Traces and distributed traces
</grid>

![[distributed_traces_waterfall.png]]

notes:

Traces give us the big picture of what happens when a request is made to an application. Whether your application is a monolith with a single database or a sophisticated mesh of services, traces are essential to understanding the full “path” a request takes in your application.

A distributed trace, more commonly known as a trace, records the paths taken by requests (made by an application or end-user) as they propagate through multi-service architectures, like microservice and serverless applications.

Many Observability backends visualize traces as waterfall diagrams .



---

<grid drag="90 20" drop="5 5">
Obeservability backends
</grid>

<grid drag="40 50" drop="5 10">
![[jaeger.tracing.io.png]]
</grid>

<grid drag="40 50" drop="-5 10">
![[grafana-tempo.png]]
</grid>

notes:
- https://www.jaegertracing.io/
- https://grafana.com/oss/tempo/

---

![[tracking_service_waterfall.png]]

---
### Metrics


![[prometheus.png]]


notes:

A **metric** is a **measurement** of a service captured at runtime. The moment of capturing a measurements is known as a **metric event**, which consists not only of the measurement itself, but also the time at which it was captured and associated metadata.

for example:
- amount of active users:
- amount of waiting invoices to process;
- count of failed requests;

https://prometheus.io/

---
## Logs

![[grafana-loki.png]]

notes:

Make a joke about reading "definition".

A log is a timestamped text record, either structured (recommended) or unstructured, with optional metadata. Of all telemetry signals, logs have the biggest legacy. Most programming languages have built-in logging capabilities or well-known, widely used logging libraries.

**Loki** is a horizontally scalable, highly available, multi-tenant log aggregation system inspired by Prometheus. It is designed to be very cost effective and easy to operate. It does not index the contents of the logs, but rather a set of labels for each log stream.

> [!info] Also, unpopular opinion!

Let's just traceparent log entry.

https://grafana.com/oss/loki/

---

![[php_logs.png|700]]

notes:

just make use of your cloud provider and simply decorate logs with your traceid :)

---
<grid drag="70 20" drop="top">
How all of that works together?
</grid>

![[otel_flow.png|700]]

---
<grid drag="70 20" drop="20 -97">
Funniest way to play with it!
</grid>

<grid drag="60 55" drop="10 20">
![[grafana-lgtm.webp]]
</grid>


<grid drag="40 45" drop="-5 -10">
![[grafana-lgtm-qr.png]]
</grid>

notes:
https://www.youtube.com/watch?v=1X3dV3D5EJg

LGTM Logs Grafana Traces Metrics

---

![[otel_state_of_sdk.png]]
notes:

as we can see since 2021 (when spec got stable) otel matured a lot, so for those technologies on which internet is built we have pretty stable toolset.

https://opentelemetry.io/docs/languages/#status-and-releases

---

Auto-instrumentation

<grid drag="20 10" drop="-1 -105">
![[python-flask-logo.png]]
</grid>

```sh
pip install opentelemetry-distro
opentelemetry-bootstrap -a install

export OTEL_PYTHON_LOGGING_AUTO_INSTRUMENTATION_ENABLED=true
opentelemetry-instrument \
    --traces_exporter console \
    --metrics_exporter console \
    --logs_exporter console \
    --service_name dice-server \
    flask run -p 8080
```


notes:

https://opentelemetry.io/docs/languages/python/

---

Example Output
<grid drag="20 10" drop="-1 -105">
![[python-flask-logo.png]]
</grid>

```json
{
    "name": "/rolldice",
    "context": {
        "trace_id": "0xdb1fc322141e64eb84f5bd8a8b1c6d1f",
        "span_id": "0x5c2b0f851030d17d",
        "trace_state": "[]"
    },
    "kind": "SpanKind.SERVER",
    "parent_id": null,
    "start_time": "2023-10-10T08:14:32.630332Z",
    "end_time": "2023-10-10T08:14:32.631523Z",
    "status": {
        "status_code": "UNSET"
    },
    "attributes": {
        "http.method": "GET",
        "http.server_name": "127.0.0.1",
        "http.scheme": "http",
        "net.host.port": 8080,
        "http.host": "localhost:8080",
        "http.target": "/rolldice?rolls=12",
        "net.peer.ip": "127.0.0.1",
        "http.user_agent": "curl/8.1.2",
        "net.peer.port": 58419,
        "http.flavor": "1.1",
        "http.route": "/rolldice",
        "http.status_code": 200
    },
    "events": [],
    "links": [],
    "resource": {
        "attributes": {
            "telemetry.sdk.language": "python",
            "telemetry.sdk.name": "opentelemetry",
            "telemetry.sdk.version": "1.17.0",
            "service.name": "dice-server",
            "telemetry.auto.version": "0.38b0"
        },
        "schema_url": ""
    }
}
{
    "body": "Anonymous player is rolling the dice: 3",
    "severity_number": "<SeverityNumber.WARN: 13>",
    "severity_text": "WARNING",
    "attributes": {
        "otelSpanID": "5c2b0f851030d17d",
        "otelTraceID": "db1fc322141e64eb84f5bd8a8b1c6d1f",
        "otelServiceName": "dice-server"
    },
    "timestamp": "2023-10-10T08:14:32.631195Z",
    "trace_id": "0xdb1fc322141e64eb84f5bd8a8b1c6d1f",
    "span_id": "0x5c2b0f851030d17d",
    "trace_flags": 1,
    "resource": "BoundedAttributes({'telemetry.sdk.language': 'python', 'telemetry.sdk.name': 'opentelemetry', 'telemetry.sdk.version': '1.17.0', 'service.name': 'dice-server', 'telemetry.auto.version': '0.38b0'}, maxlen=None)"
}

```

---

Manual instrumentation

```sh
pip install opentelemetry-api
pip install opentelemetry-sdk
```

---

Acquire Tracer
```python
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import (
    BatchSpanProcessor,
    ConsoleSpanExporter,
)

provider = TracerProvider()
processor = BatchSpanProcessor(ConsoleSpanExporter())
provider.add_span_processor(processor)

# Sets the global default tracer provider
trace.set_tracer_provider(provider)

# Creates a tracer from the global tracer provider
tracer = trace.get_tracer("my.tracer.name")
```

notes:
this ofc requires slightly more effort.
So basically you need to get the tracer instance while you are bootstrapping your application.

for instance to handle all the controller routes

---
Creating spans

```python
def do_work():
    with tracer.start_as_current_span("span-name") as span:
        # do some work that 'span' will track
        print("doing some work...")
        # When the 'with' block goes out of scope, 'span' is closed for you
```

notes:

and start feeding your traces with spans.

this is just for reference, instrumenting application could take us infinite amount of time

---


🛑 When auto-instrumentation is not great idea.

3 Factors to consider:

- Fine-grained Control and Customization.
+ Performance Considerations.
+ Compatibility and Stability

notes:
**1. Fine-grained Control and Customization:**
- **Specific Business Logic:**
    - It will either give you too much or too little information.
- **Data Masking and Security**
- **Highly Specialized Libraries:**
    - For example odoo 15, werkzeug simply does not work out of box and you are ending up with mess

**2. Performance Considerations:**

- **Resource-constrained Environments:**
    - embedded systems or high-performance applications, will not like it
    - also if grpc connection handshake can not happen... well, bad luck
- **Profiling Specific Performance Issues:**
    - if we are trying to find some bottlenecks... or profile our application, we need to manually 

**3. Compatibility and Stability:**

- **Library Version Conflicts:**
    - Auto-instrumentation can sometimes lead to conflicts with specific library versions used by your application. This can result in instability or unexpected behavior.
- **Unsupported Languages or Frameworks:**
    - Auto-instrumentation support varies across languages and frameworks. If your application uses an unsupported technology, you'll need to rely on manual instrumentation.
- **Troubleshooting Complexity:**
    - When auto instrumentation fails, troubleshooting the root cause can become complex, due to the nature of how auto instrumentation injects itself into the application.

---

<grid drag="60 55" drop="10 20">
![[otel-http-metrics.png]]
</grid>


<grid drag="40 45" drop="-5 -10">
![[otel-http-metrics-qr.png]]
</grid>

notes:
ok, and now what we have to do? we just need to NOT REINVENT THIS WHEEL

actually here we can go to grafana labs and just download json

https://grafana.com/grafana/dashboards/18860-http-metrics-opentelemetry/

---
<grid drag="90 90" drop="0 10">
![[otel-collector-flow.png]]
</grid>


<grid drag="40 45" drop="-5 -10">
![[otel-collector-flow-qr.png]]
</grid>


notes:
https://grafana.com/grafana/dashboards/18309-opentelemetry-collector-data-flow/


---
Thank you!

![[repo-qr.png|200]]

notes:

here is a link to all the resources I was referring to today

https://github.com/gulasz101/otel-extended-slides/blob/main/otel.md
