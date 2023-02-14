

# 修改service端口,指定特定端口

## 方法1,直接编辑svc
```shell
kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard
```
直接编辑对应 nodePort
```text
   ...

spec:
  clusterIP: 10.102.67.94
  clusterIPs:
  - 10.102.67.94
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - nodePort: 20001
    port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: NodePort

   ...
```
* 只能修改30000-32767范围的端口,如果端口不合法会报错
```text
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
# services "kubernetes-dashboard" was not valid:
# * spec.ports[0].nodePort: Invalid value: 20001: provided port is not in the valid range. The range of valid ports is 30000-32767
#
```

## 方法2,修改yaml文件,重新apply

```yaml
---
kind: Service
apiVersion: v1
metadata:
  labels:
    k8s-app: kubernetes-dashboard
  name: kubernetes-dashboard
  namespace: kubernetes-dashboard
spec:
  ports:
    - port: 443
      targetPort: 8443
      nodePort: 30001
      protocol: TCP
  type: NodePort
  selector:
    k8s-app: kubernetes-dashboard

---
```
```shell
kubectl apply -f recommended.yaml
```
对应修改的配置会提醒
```text
namespace/kubernetes-dashboard unchanged
serviceaccount/kubernetes-dashboard unchanged
service/kubernetes-dashboard configured   ###  配置已经修改
secret/kubernetes-dashboard-certs unchanged
secret/kubernetes-dashboard-csrf unchanged
secret/kubernetes-dashboard-key-holder unchanged
configmap/kubernetes-dashboard-settings unchanged
role.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard unchanged
deployment.apps/kubernetes-dashboard unchanged
service/dashboard-metrics-scraper unchanged
deployment.apps/dashboard-metrics-scraper unchanged

```

