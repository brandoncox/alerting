# Directions

## Enable User Metrics Gathering
Directions can be found in https://docs.openshift.com/container-platform/4.9/monitoring/enabling-monitoring-for-user-defined-projects.html

Create project to test this alerts
> oc new-project ns1

Edit or create ConfigMap to allow for gathering metrics on user workloads
> oc -n openshift-monitoring edit configmap cluster-monitoring-config

    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: cluster-monitoring-config
      namespace: openshift-monitoring
    data:
      config.yaml: |
        enableUserWorkload: true

When this completes after a few minutes you should see a number of pods created in the **openshift-user-workload-monitoring** project.

    oc get pods -n openshift-user-workload-monitoring
    NAME                                  READY   STATUS    RESTARTS         AGE
    prometheus-operator-d985f58c8-r5ddr   2/2     Running   4                23h
    prometheus-user-workload-0            5/5     Running   10               23h
    prometheus-user-workload-1            5/5     Running   10               23h
    thanos-ruler-user-workload-0          3/3     Running   12 (3h38m ago)   23h
    thanos-ruler-user-workload-1          3/3     Running   12 (3h38m ago)   23h
 
## Clone Repo & Create Rules
> git clone https://github.com/brandoncox/alerting
>
> cd alerting
> 
> oc apply all -f .
