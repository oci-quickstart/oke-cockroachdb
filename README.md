# oci-cockroach
This is a walkthrough of setdeploying an insecure [CockroachDB](https://github.com/cockroachdb/cockroach) up on [Oracle Kubernetes Engine](https://cloud.oracle.com/containers/kubernetes-engine) (OKE).

**NOTE** If you plan to use CockroachDB in production, you should deploy a secure cluster by following the instructions [here](https://www.cockroachlabs.com/docs/stable/orchestrate-cockroachdb-with-kubernetes.html).

## Prerequisites
First you're going to need to setup an Oracle Cloud account, your environmental variables, an OKE cluster and your kubectl.  It sounds like a lot, but there's a nice walkthrough [here](https://github.com/cloud-partners/oke-how-to) that should help.

## Deploying CockroachDB on Oracle Kubernetes Engine (OKE)

#### Initialize and update Helm
`helm init --upgrade --service-account tiller`

#### Install the CockroachDB Helm chart
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