# ensure somedisk has folders created for shares
# kubectl exec -it test-554864695d-bff2z sh
# cd /mnt

mkdir -p  tcs nilesh xyz

# exit

# PV PVC

kubectl apply -f k8s/pv/02-test.yaml

kubectl apply -f k8s/deploy/02-test.yaml
