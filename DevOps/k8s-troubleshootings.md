### Issue 1: spring boot projects with K8S client sdk failed on start up

error snippet in container log:
```
cannot get resource "configmaps" in API group "" in the namespace 
```

Solution:
```
### create role (you can skip if your Role exists)
I use cluster-admin

### create service account (you can skip if your SA exists)
kind: ServiceAccount
apiVersion: v1
metadata:
  name: test-sa
  namespace: test
  annotations:
    iam.kubesphere.io/clusterrole: cluster-admin


### create roleBinding
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: test
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: test-sa
  namespace: test
  
```
