# Demos

### Prerequisites

* Kubernetes, Istio and Knative already deployed.
* Install buildpack template:

```
kubectl apply -f https://raw.githubusercontent.com/knative/build-templates/master/buildpack/buildpack.yaml
```
* Have a Docker Registry ready (e.g. Docker Hub).
* Configure credentials for Docker Registry with `kubectl apply -f samples/docker.yml`:
    ```yaml
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

## Demo 1: kubectl

1. Deploying from image
    ```bash
    bat samples/simple-app.yml
    kubectl apply -f samples/simple-app.yml
    ```
1. Deploying from source code
    ```bash
    bat samples/simple-app-from-source.yml
    kubectl apply -f samples/simple-app-from-source.yml
    ```
1. Deploy same application and see how revisions are created with:
    ```bash
    kubectl get revisions
    ```

## Demo 2: Autoscaling test

1. Curl an application scaled-to-zero and see how it creates a container in a matter of seconds:
    ```bash
    curl ghost-app.default.knative.mybluemix-test.net
    ```
2. You can generate some load againts the endpoint and see how the application scales:
```
go get github.com/tscolari/bender
bender --command "curl simple-app.default.knative.mybluemix-test.net" --concurrency 100 --interval 100ms --keep-running
```

## Demo 3: Knife

Knife is a CLI which generates Knative configution and can be piped to `kubectl`:

```bash
knife generate-service simple-app jszroberto/helloworld 
```

1. Deploying from image
    ```bash
    knife generate-service simple-app jszroberto/helloworld | kubectl apply -f -
    ```
2. Deploying from source code
    ```bash
    knife generate-service simple-app-from-source jszroberto/simple-app-from-source \
        -s build-bot \
        -u https://github.com/jszroberto/helloworld-go \
        -t buildpack \
        -a IMAGE=jszroberto/simple-app-from-source \
        | kubectl apply -f -
    ```

## Demo 4: knctl

`knctl` is an unofficial CLI for `Knative`:

1. Deploying from image
    ```bash
    knctl deploy \
        --namespace default \
        --service simple-app \
        --image  jszroberto/helloworld
    ```

2. Deploying from source code
    ```bash
    knctl deploy \
        --namespace default \
        --service simple-app-from-source \
        --git-url https://github.com/jszroberto/helloworld-go \
        --git-revision master \
        --service-account build-bot \
        --template buildpack \
        --image  jszroberto/simple-app-from-source
    ```
3. Deploy slides from directory:

```bash
knctl -n default deploy \
    --service introduction-to-knative \
    --directory=$PWD \
    --service-account build-bot \
    --image index.docker.io/jszroberto/introduction-to-knative \
    --template buildpack
```