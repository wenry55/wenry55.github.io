---
layout: default
title: using configmap as env from pod in kubernetes
categories: [kubernetes, configmap, env]
tags: [kubernetes, configmap, env]
---

### using configmap as env from pod in k8s

1. create configmap
2. apply configmap
3. create/edit pod yaml
4. launch pod


> yaml config in stage 3.
>```yaml
spec:
  containers:
  - envFrom:
    - configMapRef:
      name: mongo-connstr
```

