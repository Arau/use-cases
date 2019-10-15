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

Wait for the Pods to start. Both applications might take some time to start.

```bash
~$ kubectl get pod confluence-0 jira-0
NAME           READY     STATUS    RESTARTS   AGE
confluence-0   1/1       Running   0          3h24m
jira-0         1/1       Running   0          5h8m
```
