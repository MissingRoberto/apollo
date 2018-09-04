# Samples

## Prerequisites

* Kubernetes, Istio and Knative already deployed.
* Install buildpack template:

```
kubectl apply -f https://raw.githubusercontent.com/knative/build-templates/master/buildpack/buildpack.yaml
```
* Have a Docker Registry ready (e.g. Docker Hub).
* Configure credentials for Docker Registry with `kubectl apply -f samples/docker.yml`. E.g: 
    ```
    apiVersion: v1
    kind: Secret
    metadata:
    name: basic-user-pass
    annotations:
        build.knative.dev/docker-0: https://index.docker.io # Described below
    type: kubernetes.io/basic-auth
    stringData:
    username: jszroberto
    password: <REMOVED>

    ---
    apiVersion: v1
    kind: ServiceAccount
    metadata:
    name: build-bot
    secrets:
    - name: basic-user-pass
    - name: registry
    ```
## kubectl

### Deploying from image

```
kubectl apply -f samples/simple-app.yml
```

### Deploying from source code

```
kubectl apply -f samples/simple-app-from-source.yml
```

## Knife

Knife generates Knative configution: 

```
knife generate-service simple-app jszroberto/helloworld 
```

### Deploying from image

```
knife generate-service simple-app jszroberto/helloworld | kubectl apply -f -
```

### Deploying from source code
```
knife generate-service simple-app-from-source jszroberto/simple-app-from-source -s build-bot -u https://github.com/jszroberto/helloworld-go -t buildpack -a IMAGE=jszroberto/simple-app-from-source | kubectl apply -f -
```

## knctl

### Deploying from image

```
knctl deploy \
    --namespace default \
    --service simple-app \
    --image  jszroberto/helloworld
```

### Deploying from source code

```
knctl deploy \
    --namespace default \
    --service simple-app-from-source \
    --git-url https://github.com/jszroberto/helloworld-go \
    --git-revision master \
    --service-account build-bot \
    --template buildpack \
    --image  jszroberto/simple-app-from-source
```

## Autoscaling test

You can generate some load againts the endpoint and see how the application scales:
```
go get github.com/tscolari/bender
bender --command "curl simple-app.default.knative.mybluemix-test.net" --concurrency 100 --interval 100ms --keep-running
```