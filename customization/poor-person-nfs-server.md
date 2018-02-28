##########
# NFS

kubectl apply -f k8s/deploy/01-nfs.yaml -f k8s/svc/01-nfs.yaml



kubectl delete deploy nfs-server
kubectl delete svc nfs-server
