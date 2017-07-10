# kubernetes-prometheus-autoscaler

Scale deployments based on prometheus queries

### How to use

Run this as a pod in your cluster.

Add this label to deployments you want considered:

```
  labels:
    scale: prometheus
```

Add these annotations to control scaling:

```
    prometheusScaler/prometheus-query: "time() % (60 * 60)"
    prometheusScaler/min-scale: "2"
    prometheusScaler/max-scale: "5"
    prometheusScaler/scale-up-when: "result > 50"
    prometheusScaler/scale-down-when: "result < 10"
```

Scale up and scale down conditions use this clever repo https://github.com/Knetic/govaluate.  The value retrieved from query is exposed to the expression as a parameter named `result`.

#### Command Line Usage

```
  -assessment-interval duration
        Time to sleep between checking deployments. (default 1m0s)
  -prometheus-url string
        URL to query. (default "http://prometheus:9090")
```

### Improvements

This repo is still under active development and needs a long list of improvements (but it works!).  Some obvious ones:

- General code refactoring/error handling
- Better logging
- Obvious Performance Improvements (Don't run scale up query if at max)
- Comments/documentation
- Publish to docker hub once it sucks less
- Refactor to use go funcs and channels?
