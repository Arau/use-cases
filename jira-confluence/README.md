# JIRA and Confluence with StorageOS

Jira and Confluence are widely used to organise and document projects of all
sorts.

This repository is part of our use-case documentation and provides example YAML
manifests to help you get started on running [JIRA and Confluence with
StorageOS](https://docs.storageos.com/docs/usecases/kubernetes/jira).

Please visit the official docs page and please do leave us feedback for
improvement or simply open a PR.

## Jira

The Jira StatefulSet is defined in `20-statefulset-jira.yaml`. In it you should
specify the env variable `X_PROXY_NAME` to set the DNS name you want to use to
serve the application.

## Confluence

The Confluence StatefulSet is defined in `25-statefulset-confluence.yaml`. In it you should
specify the env variable `X_PROXY_NAME` to set the DNS name you want to use to
serve the application.

## Deploy

In order to deploy Jira and Confluence with persistent storage you just need to
clone this repository and use kubectl to create the Kubernetes objects.

```bash
git clone https://github.com/storageos/use-cases.git storageos-usecases
cd storageos-usecases
kubectl create -f ./jira-confluence
```

Wait for the Pods to start. Since they are Java applications, the bootstrap of
the applications might take a while the first time.

```bash
~$ kubectl get pod confluence-0 jira-0
NAME           READY     STATUS    RESTARTS   AGE
confluence-0   1/1       Running   0          3h24m
jira-0         1/1       Running   0          5h8m
```

Once the applications are running, you can set them up via the browser Wizard.
In there you will be asked to provide with an atlassian license.

## Access through the browser

The applications are created using ClusterIp services, so internal services. If
you want to expose them, you can create the ingress `30-ingress.yaml.DISABLED`
assuming your cluster have an ingress controller running. Please adjust its
configuration to reflect the HostNames of your applications.

```bash
# Edit the ingress
vi ./jira-confluence/30-ingress.yaml.DISABLED
kubectl create -f ./jira-confluence/30-ingress.yaml.DISABLED
```

On the other hand you can use a port-forward to access locally and set up the
application.

```bash
kubectl port-forward svc/jira 8080:8080 # Access at http://localhost:8080
kubectl port-forward svc/confluence 8081:80 # Access at http://localhost:8081
```

You can access the browser at `http://localhost:8080` and
`http://localhost:8081` to access the configuration wizard.


## Database

The configuration of both Jira and Confluence require the set up of a database.
You can use an internal one for tested, but it is not recommended by atlassian.
You can use one of the many examples available in this repository, such as
`../postgress` or `../mysql` to provision a Database in Kubernetes with
HighAvailable persistent storageos. Both examples just require to create the
manifests. After that, you need to point the DB config in the application set
up to the database service URL. For instance, if using PostgreSQL,
`postgres-0.postgres`.
