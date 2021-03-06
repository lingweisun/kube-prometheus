---
title: "Monitoring other Namespaces"
description: "This guide will help you monitor applications in other Namespaces."
lead: "This guide will help you monitor applications in other Namespaces."
date: 2021-03-08T23:04:32+01:00
draft: false
images: []
menu:
  docs:
    parent: "kube"
weight: 640
toc: true
---

This guide will help you monitor applications in other Namespaces. By default the RBAC rules are only enabled for the `Default` and `kube-system` Namespace during Install.

# Setup
You have to give the list of the Namespaces that you want to be able to monitor.
This is done in the variable `prometheus.roleSpecificNamespaces`. You usually set this in your `.jsonnet` file when building the manifests.

Example to create the needed `Role` and `RoleBinding` for the Namespace `foo` : 
```
local kp = (import 'kube-prometheus/main.libsonnet') + {
  _config+:: {
    namespace: 'monitoring',

    prometheus+:: {
      namespaces: ["default", "kube-system", "foo"],
    },
  },
};
 
{ ['00namespace-' + name]: kp.kubePrometheus[name] for name in std.objectFields(kp.kubePrometheus) } +
{ ['0prometheus-operator-' + name]: kp.prometheusOperator[name] for name in std.objectFields(kp.prometheusOperator) } +
{ ['node-exporter-' + name]: kp.nodeExporter[name] for name in std.objectFields(kp.nodeExporter) } +
{ ['kube-state-metrics-' + name]: kp.kubeStateMetrics[name] for name in std.objectFields(kp.kubeStateMetrics) } +
{ ['alertmanager-' + name]: kp.alertmanager[name] for name in std.objectFields(kp.alertmanager) } +
{ ['prometheus-' + name]: kp.prometheus[name] for name in std.objectFields(kp.prometheus) } +
{ ['grafana-' + name]: kp.grafana[name] for name in std.objectFields(kp.grafana) }

```
