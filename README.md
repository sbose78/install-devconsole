
# MOVED TO https://github.com/redhat-developer/devconsole-operator/wiki




## (Re)Installing OpenShift Console on an OpenShift 4 cluster


### Disable CVO operator management

OpenShift Console is managed by a CVO operator named `console-operator`.

```
oc apply -f unmanage.yaml
```

### Shutdown the console


Scale down existing deployments of `console-operator`

```
oc scale --replicas 0 deployment console-operator --namespace openshift-console-operator
```

Validate that there are no `console-operator-` pods
```
oc get pods -n openshift-console-operator
```

Scale down existing deployments of `console`

```
oc scale --replicas 0 deployment console --namespace openshift-console
```

Validate that there are no `console-` pods. Leave the `download-` pods as is.

```
oc get pods -n openshift-console
```

### Reconfigure the console


Update the `console-operator` deployment

```
oc apply -f redeploy-console-operator.yaml
```

Validate that the `console-operator` deployment has scaled up to 1

```
oc get pods -n openshift-console-operator
```

### Start the console

After the pods are `running`, scale up the `openshift-console` deployment

```
oc scale --replicas 1 deployment console --namespace openshift-console
```

Validate that the `console-operator` deployment has scaled up to 1
```
oc get pods -n openshift-console
```


## Installing the dev console operator on OpenShift 4
```
oc apply -f http://operatorhubio-operator-hub.devtools-dev.ext.devshift.net/installopenshift4/devconsole.v0.1.0.yaml
```

