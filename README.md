# oke-cockroachdb
This is a walkthrough of setting [CockroachDB](https://github.com/cockroachdb/cockroach) up on [Oracle Cloud Infrastructure Container Engine for Kubernetes (OKE)](https://cloud.oracle.com/containers/kubernetes-engine). It is developed jointly by Oracle and Cockroach Labs.

## Prerequisites
First you're going to need to setup an Oracle Cloud account, your environmental variables, an OKE cluster and your kubectl.  It sounds like a lot, but there's a nice walkthrough [here](https://github.com/oracle/oke-quickstart-prerequisites) that should help.

**NOTE** If you plan to use CockroachDB in production, you should deploy a secure cluster by following the instructions [here](https://www.cockroachlabs.com/docs/stable/orchestrate-cockroachdb-with-kubernetes.html).

## Deploying CockroachDB on Oracle Cloud Infrastructure Container Engine for Kubernetes (OKE)
We will use Helm to deploy CockroachDB. Helm is a package manager for Kubernetes. If you don't have Helm installed, follow the instructions [here](https://helm.sh/docs/using_helm/#installing-helm) to install it first.

#### Initialize and update Helm
`helm init --upgrade --service-account tiller`

#### Install the CockroachDB Helm chart
In this example we are using "my-release" as the release name. If you use a different name, be sure to change the release name in subsequent commands.
`helm install --name my-release stable/cockroachdb`

#### Confirm that the pods are running
`kubectl get pods`

```
NAME                       READY     STATUS    RESTARTS   AGE
my-release-cockroachdb-0   1/1       Running   0          48s
my-release-cockroachdb-1   1/1       Running   0          47s
my-release-cockroachdb-2   1/1       Running   0          47s
```

#### Access the Admin UI
`kubectl port-forward my-release-cockroachdb-0 8080`

Then go to http://localhost:8080/ in a browser.

#### Using the built-in SQL client
The following command will launch an interactive pod that runs the SQL client inside it:

`kubectl run cockroachdb -it --image=cockroachdb/cockroach --rm --restart=Never -- sql --insecure --host=my-release-cockroachdb-public`
