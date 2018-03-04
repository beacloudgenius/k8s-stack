##########
# Poorman's NFS

Create a GCE persistent disk

    gcloud compute disks create somedisk --type=pd-ssd --size=200GB

Service

    kubectl apply -f svc/

PV PVC

    kubectl apply -f pv/

Deployment

    kubectl apply -f deploy/

ensure somedisk has folders created for shares

    kubectl exec -it test-tools* bash

    mkdir -p  busy test

    exit
