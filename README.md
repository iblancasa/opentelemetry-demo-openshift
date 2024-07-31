# Distributed Tracing Demo in OpenShift

Thank you for your interest in our Distributed Tracing demo. This demo is crafted to show SREs (Site Reliability Engineers) how to leverage tracing technologies to uncover bottlenecks, pinpoint defects, and understand system interdependencies, thereby enhancing operational stability. We've based our work on the community's OpenTelemetry Demo, which is essentially a microservice-driven system designed to demonstrate OpenTelemetry application in scenarios that closely mimic real-life operations. Our version extends this by integrating the Red Hat Build of OpenTelemetry along with the Tempo Operator from Red Hat.

# Requirements

Before running the demo, you will need some elements:

- An OCP cluster and admin permissions
- Helm

## How to install `cert-manager`
`cert-manager`:
```sh
$ kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.3/cert-manager.yaml
$ kubectl wait --for=condition=Available=True deploy --all -n 'cert-manager'
```

## How to install `open-feature-operator`:
```sh
$ helm repo add openfeature https://open-feature.github.io/open-feature-operator/
$ helm repo update
$ helm upgrade --install openfeature openfeature/open-feature-operator
```

# Run the demo

## Install the needed operators

You need to install some Red Hat products in order to make the demo work properly. Just run:

```sh
$ helm install dt-infra infra
```

This will install:
- [Red Hat Build of OpenTelemetry](https://docs.openshift.com/container-platform/4.15/observability/otel/otel-installing.html)
- [Distributed Tracing Platform (Tempo)](https://docs.openshift.com/container-platform/4.15/observability/distr_tracing/distr_tracing_tempo/distr-tracing-tempo-installing.html)

Verify the operators are properly up and running:

```sh
$ oc get deployment -n openshift-tempo-operator
$ oc get deployment -n openshift-opentelemetry-operator
```

## Install the demo

First, create the namespace `demo`:
```sh
$ oc create namespace demo
```

Now, install the Helm chart:
```sh
$ helm install otel-demo demo --namespace demo
```

Verify the deployments were deployed properly:
```sh
$ oc get deployment -n demo
NAME                                       READY   UP-TO-DATE   AVAILABLE   AGE
opentelemetry-demo-accountingservice       1/1     1            1            1m
opentelemetry-demo-adservice               1/1     1            1            1m
opentelemetry-demo-cartservice             1/1     1            1            1m
opentelemetry-demo-checkoutservice         1/1     1            1            1m
opentelemetry-demo-collector               1/1     1            1            1m
opentelemetry-demo-currencyservice         1/1     1            1            1m
opentelemetry-demo-emailservice            1/1     1            1            1m
opentelemetry-demo-frauddetectionservice   1/1     1            1            1m
opentelemetry-demo-frontend                1/1     1            1            1m
opentelemetry-demo-frontendproxy           1/1     1            1            1m
opentelemetry-demo-kafka                   1/1     1            1            1m
opentelemetry-demo-loadgenerator           1/1     1            1            1m
opentelemetry-demo-paymentservice          1/1     1            1            1m
opentelemetry-demo-productcatalogservice   1/1     1            1            1m
opentelemetry-demo-quoteservice            1/1     1            1            1m
opentelemetry-demo-recommendationservice   1/1     1            1            1m
opentelemetry-demo-shippingservice         1/1     1            1            1m
tempo-simplest-compactor                   1/1     1            1            1m
tempo-simplest-distributor                 1/1     1            1            1m
tempo-simplest-querier                     1/1     1            1            1m
tempo-simplest-query-frontend              1/1     1            1            1m
```
