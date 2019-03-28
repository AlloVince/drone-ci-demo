## A Demo to show workflow of Drone CI + GitFlow + Semantic Release + Kubernetes

[![Build Status](https://cloud.drone.io/api/badges/AlloVince/drone-ci-demo/status.svg)](https://cloud.drone.io/AlloVince/drone-ci-demo)

Step by step, to show how to build a powerful team development workflow with CI

- [Github repo](https://github.com/AlloVince/drone-ci-demo)
- [Drone CI of this repo](https://cloud.drone.io/AlloVince/drone-ci-demo)
- [Docker Registry of this repo](https://cloud.docker.com/repository/docker/allovince/drone-ci-demo)

## Step 0: Hello world

1. Use drone cloud or setup a private drone by k8s

Kubernetes config files under [./kubernetes](./kubernetes)

2. prepare `.drone.yml`

```yml
kind: pipeline
name: deploy

steps:
- name: hello-world
  image: docker
  commands:
    - echo "hello world"
```

## Step 1: For single person, manually release

1. Add a secret in Drone, key is `DOCKER_PASSWORD`, value is your docker registry password

2. prepare `.drone.yml`

```yml
kind: pipeline
name: deploy

steps:
  - name: unit-test
    image: node:10
    commands:
      - node test/index.js
    when:
      branch: master
      event: push
  - name: build-image
    image: plugins/docker
    settings:
      repo: allovince/drone-ci-demo
      username: allovince
      password:
        from_secret: DOCKER_PASSWORD
      auto_tag: true
    when:
      event: tag
```

3. push to master branch will trigger unit test

4. manually release on github will trigger building docker image

## Step 2: For team develop, support GitFlow

Change `.drone.yml` as [this](https://github.com/AlloVince/drone-ci-demo/blob/gitflow/.drone.yml)

## Step 3: For team develop, support GitFlow, with semantic-release

Change `.drone.yml` as [this](https://github.com/AlloVince/drone-ci-demo/blob/semantic-release/.drone.yml)

