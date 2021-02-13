# A simple Helm demo

## Setup

We are going to deploy three instances of the awesome-app. One will be `prd`, one `tst` and one `dev`. Thus, we first want three namespaces.

```
kubectl create ns awesome-helm
kubectl create ns awesome-helm-tst
kubectl create ns awesome-helm-dev
```

## Deploy from a local chart

Clone this repo.

```
git clone git@github.com:bespinian/helm-demo.git
cd helm-demo
ls -l
```

Note the typical layout of a Helm chart. The file `Chart.yaml` contains the chart metadata including a conditional dependency on `bitnami/postgresql`. The `templates` directory contains templates for the resources which are deployed. The `charts` directory contains Helm charts on which this chart is depending. In this case, this is `bitnami/postgresql`. Furthermore, there are three values files:

- `values.yaml`: the default values, used in our case for production.
- `values-tst.yaml`: the values used for the test environment. Note the override of the `replicaCount`.
- `values-dev.yaml`: the values used for the development environment. Note the use of the local Postgresql DB in this case.

### Deploy prd

We issue the following command to deploy the `prd` instance:

```
helm install prd . --namespace awesome-helm
```

Note that we do not need to specify any values file as this is the default setup. We see that the app is deployed with 3 replicas and no database running inside the cluster.

### Deploy tst

We issue the following command to deploy the `tst` instance:

```
helm install tst . -f values-tst.yaml --namespace awesome-helm-tst
```

Note that we tell Helm to use `values-tst.yaml` instead of the default.

### Deploy dev

We issue the following command to deploy the `dev` instance:

```
helm install tst . -f values-dev.yaml --namespace awesome-helm-dev
```

Note that we tell Helm to use `values-dev.yaml` instead of the default. We see that in this case, a local Postgresql instance is deployed to the same namespace.
