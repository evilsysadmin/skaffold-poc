###  Skaffold POC example -Node.js with hot-reload

- deploys with helm , in local k3d cluster + local image registry
- creates cluster load balancer

Simple example based on Node.js demonstrating the file synchronization mode.

#### Tooling

- k9s
- k3d
- Tilt

The makefile flow tries to ensure tooling is available locally.

#### Init

```bash
make bootstrap # to download the tooling, if needed ,and setup the cluster. This will take a few minutes on first run.

K3d will make kubeconfig point automatically to new k3d cluster

```

#### Workflow

* run k9s from another window # to open a new shell tab , connecting to the local k3d cluster with k9s

* run make dev , to apply the Tiltfile in your local cluster. After running this , hit the spacebar.
  This will open a browser session, in which you can see the deployments status, as well as see logs etc.

  * Make some changes to `index.js`:
      * The file will be synchronized to the cluster
      * `nodemon` will restart the application

  * Make some changes to `package.json`:
      * The full build/push/deploy process will be triggered, fetching dependencies from `npm`

* A single ingress host is created , for "test.com". /etc/hosts updates are automatically managed from makefile.

  So you can test it with

$ curl test.com
  Hello World!!!!!

* Run make nodev , to kill the local devenv.

* And delete your local cluster , with make delete-k3d

### Extra cluster features

You can deploy extra tooling in the cluster , with parameters in extras.yaml

```
k3d:
  prometheus: false
  localstack: true
  elastic: true
  traefik: true
```

Currently this project supports:

- prometheus
- localstack aws
- elasticsearch cluster
- traefik 

I will add more tooling , and more docs soon.
