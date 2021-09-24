### grafana-deployment.yaml里面添加了pvc
### 添加grafana-ingress.yaml
### 添加alertmanager-webhook-dingtalk.yaml
### node-exportor,kube-state-metrics,blackbox-exporter，prometheus-operator的limit改大，不然会触发CPUThrottlingHigh（受限时间>25%）
### kubernetes-prometheusRule.yaml 删掉KubeControllerManagerDown，KubeSchedulerDown

al# 修改grafana ingress host,alertmanager-webhook-dingtalk token
kubectl create -f manifests/setup
kubectl get crd|grep coreos
kubectl create -f manifests/