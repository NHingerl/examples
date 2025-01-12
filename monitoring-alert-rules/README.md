# Alert Rules Example

## Overview

This example shows how to deploy and view alerting rules in Kyma.

## Prerequisites

* Kyma as the target deployment environment.

## Installation

You need access to the `kyma-system` Namespace to execute the described steps.

### Add a new alerting rule

1. Create the PrometheusRule resource holding the configuration of your alerting rule. 

    ```bash
    kubectl apply -f deployment/alert-rule.yaml
    ```

2. Run the `port-forward` command on the `monitoring-prometheus` service to access the Prometheus dashboard.

    ```bash
    kubectl port-forward -n kyma-system svc/monitoring-prometheus 9090:9090
    ```

3. Go to `http://localhost:9090/rules` and find the **pod-not-running** rule.

    Because the `http-db-service` Deployment does not exist, Alertmanager fires an alert listed at `http://localhost:9090/alerts`.

### Stop the alert from getting fired

1. Export your Namespace as a variable:

    ```bash
    export KYMA_EXAMPLE_NS="{namespace}"
    ```

2. To stop the alert from getting fired, create the Deployment:

    ```bash
    kubectl apply -f ../http-db-service/deployment/deployment.yaml -n $KYMA_EXAMPLE_NS
    ```

### Cleanup

Run the following commands to completely remove the example and all its resources from the cluster:

1. Remove the **pod-not-running** alerting rule from the cluster:

    ```bash
    kubectl delete cm -n kyma-system -l example=monitoring-alert-rules
    ```

2. Remove the **http-db-service** example and all its resources from the cluster:

    ```bash
    kubectl delete all -l example=http-db-service -n $KYMA_EXAMPLE_NS
    ```

For complete guidelines on how to define alerting rules, see [this](https://kyma-project.io/docs/kyma/latest/03-tutorials/00-observability/obsv-03-define-alerting-rules-monitor/) tutorial.
