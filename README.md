# Sensu Go Monitoring pipeline demo repo

This repo contains a set of resources to demonstrate building a basic monitoring
pipeline with Sensu Go.

## Usage

Install [sensuctl](https://docs.sensu.io/sensu-go/5.14/installation/install-sensu/#install-sensuctl)

```sh
curl -LO https://s3-us-west-2.amazonaws.com/sensu.io/sensu-go/5.14.2/sensu-go_5.14.2_darwin_amd64.tar.gz
tar -xvf sensu-go_5.14.2_darwin_amd64.tar.gz
sudo cp sensuctl /usr/local/bin/
```

Clone this repo.

Create a demo Kubernetes cluster using a tool of your choice. [Docker for
Mac](https://docs.docker.com/docker-for-mac/#kubernetes) has built in kubernetes
support. Another option is
[kind](https://kind.sigs.k8s.io/docs/user/quick-start/). These instructions use
`kind`.

```sh
❯ kind create cluster
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.13.3) 🖼
 ✓ [control-plane] Creating node container 📦
 ✓ [control-plane] Fixing mounts 🗻
 ✓ [control-plane] Configuring proxy 🐋
 ✓ [control-plane] Starting systemd 🖥
 ✓ [control-plane] Waiting for docker to be ready 🐋
 ✓ [control-plane] Pre-loading images 🐋
 ✓ [control-plane] Creating the kubeadm config file ⛵
 ✓ [control-plane] Starting Kubernetes (this may take a minute) ☸
Cluster creation complete. You can now use the cluster with:

export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"
kubectl cluster-info

❯ export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"
```

Apply the kubernetes manifests:

```sh
❯ kubectl apply -f kube
```

Run a port forward to connect to cluster services:

```sh
❯ kubectl port-forward service/sensu -n sensu-system 8080 8081 3000
```

Configure sensuctl with defaults:

```sh
sensuctl configure --non-interactive --username admin --password P@ssw0rd! --url http://127.0.0.1:8080
```

Create sensu resources:

```sh
sensuctl create -f rbac
sensuctl create -f pipelines
```

## About

Sensu Go is a telemetry and service health checking solution for multi-cloud
monitoring at scale.

To learn more about Sensu, [please visit the website](https://sensu.io/).
