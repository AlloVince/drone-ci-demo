# A Demo to show Workflow of Drone CI + Gitflow + Semantic Release + Kubernetes


- [Github repo](https://github.com/AlloVince/drone-ci-demo)
- [Drone CI of this repo](https://cloud.drone.io/AlloVince/drone-ci-demo)
- [Docker Registry of this repo](https://cloud.docker.com/repository/docker/allovince/drone-ci-demo)

## Step 1: Hello world

1. Use drone cloud or setup a private drone by k8s

Kubernetes config could check [./kubernetes]

2. prepare `.drone.yml`

```
kind: pipeline
name: deploy

steps:
- name: hello-world
  image: docker
  commands:
    - echo "hello world"
```

