---
layout: post
title: Best practices for Operating Containers
description: Best practices for Operating Containers
keywords: docker, best-practices
---

### Use native logging mechanisms of container
- Write log to `stdout`, `stderr`
- Use `docker logs`
- Centralize logs and make them searchable: [fluentd](https://www.fluentd.org/), EFK stack (Elastic Search, Fluentd, Kibana)
![typical log management system in k8s](https://cloud.google.com/solutions/images/bp-operating-containers-log-management.svg)
- JSON logs
```json
{
  "date": "2018-01-01 01:01:01",
  "component": "foo",
  "subcomponent": "foo.bar",
  "level": "WARNING",
  "message": "There is something wrong."
}
```

- Log aggregator sidecar pattern
![sidecar pattern](https://cloud.google.com/solutions/images/bp-operating-containers-sidecar.svg)

### Ensure your containers stateless and immutable
- Externalize the container configuration (listening port, runtime options,...)
- Use `ConfigMap` to run same container image across different environment
- Use `Secrets` and `ConfigMap` to inject configuration in containers as environment variables or files.
![configmap with same image](https://cloud.google.com/solutions/images/bp-operating-containers-configmap.svg)

### Avoid privileged containers
- If your application must modify the host settings in order to run, modify those settings either in a sidecar container or an `init container`.
- Sidecar container does NOT need to be exposed either internal or external traffic.

### Make your application easy to monitor
- `/metrics` endpoint with Prometheus client library
- Sidecar pattern when your application can not instrument with a `/metrics` endpoint
![sidecar metrics](https://cloud.google.com/solutions/images/bp-operating-containers-monitoring.svg)
- Expose your health of your application: Liveness, Readiness

### Avoid running as root
- If you are running **single application per container** and single application with single user (not root), you can grant all users write permissions to the folders and files that need to be written in, make all the other folders and files only writable by root
- Simple way to check container permission by run
```bash
docker run --user $((RANDOM+1)) [YOUR_CONTAINER]
```

### Carefully image version
- Sematic version
- Avoid using `latest` tag
- Prefer minor version over path version for automatically securities patch

### Conclusion
References: 
- [https://cloud.google.com/solutions/best-practices-for-operating-containers](https://cloud.google.com/solutions/best-practices-for-operating-containers)
