# There are two problems with our public dashboard chart:
- https://github.com/kubernetes/dashboard/tree/master/charts/helm-chart/kubernetes-dashboard

# First: 
- from v1.24 ServiceAccount token secrets are no longer automatically generated and mounted to SA
- Note: https://stackoverflow.com/questions/72256006/service-account-secret-is-not-listed-how-to-fix-it
- The https://github.com/kubernetes/dashboard/blob/master/charts/helm-chart/kubernetes-dashboard/values.yaml should contain a way to inject secretAnnotations: {kubernetes.io/service-account.name: xxx} which is not implemented yet
- Hence, we need to manually create secret of type kubernetes.io/service-account-token with an annotation

# Second:
- The chart doesn't contain possibility to attach the SA to cluster-admin ClusterRole. 
- Although it's not advised in production to give admin access to dashboard SA.

# Hence, we need to apply some configuration manually after our helm release.

# Installation of kubernetes-dashboard
- helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
- helm install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard -n dev -f values.yaml
- kubectl apply -f secret.yaml 

# To view the dashboard use the token from here
- kubectl describe secret dashboard-admin -n dev

# Note:
- although we exposed dashboard through ingress, we can't login in UI from LB
- https://github.com/kubernetes/dashboard/issues/5612#issuecomment-705888482
- we need to port-forward and access from localhost