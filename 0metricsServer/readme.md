# kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

After Installation, check if metrics are getting exposed.
# kubectl get apiservices | grep metrics