---
date:
  - 09/09/2024 11:57
tags: 
cssclasses:
  - image-borders
  - neutral-pen-black
---
# конфигурация Pod

```yml
apiVersion: v1 # there is also apps/v1
kind: Pod
metadata:
	name: short-app # pod name
	labels:
		components: frontend
spec:
	containers:
		- name: short-app
			image: example/image
			ports:
				- containerPort: 80
			resources:
				limits:
					memory: "128Mi"
					cpu: "500m"
```
