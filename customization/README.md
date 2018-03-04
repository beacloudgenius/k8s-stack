##########
# Poorman's NFS

Create a GCE persistent disk

    gcloud compute disks create somedisk --type=pd-ssd --size=200GB

PV PVC

    kubectl apply -f pv/

Service

    kubectl apply -f svc/

Deployment

    kubectl apply -f deploy/

ensure somedisk has folders created for shares

    kubectl exec -it test-554864695d-bff2z sh

    mkdir -p  busy test

    exit
