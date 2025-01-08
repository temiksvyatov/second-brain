---
date:
  - 26/05/2024 13:31
tags: 
cssclasses:
  - image-borders
  - neutral-pen-black
  - page-manila
---
# [Replicas Doc](https://docs.docker.com/compose/compose-file/deploy/#replicas)
The old "scale" feature is [now called `replicas`](https://docs.docker.com/compose/compose-file/deploy/#replicas) and it is part of the current Docker Compose specs, 2.19.1 as of writing.

Note that `replicas` is ignored if you name your container using `container_name: myname`. You must let the Docker generate the names.

```yaml
services:
  myapp:
    image: awesome/webapp
    deploy:
      mode: replicated
      replicas: 6
```
