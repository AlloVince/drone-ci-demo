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

## Step 2: For single person, manually release

1. Add a secret in Drone, key is `DOCKER_PASSWORD`, value is your docker registry password

2. prepare `.drone.yml`

```
kind: pipeline
name: deploy

steps:
  - name: pre-check
    image: node:10
    environment:
      DOCKER_PASSWORD:
        from_secret: DOCKER_PASSWORD
    commands:
      - echo $${DOCKER_PASSWORD}
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

