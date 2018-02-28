setup gcloud sdk
    sudo apt-get update && sudo apt-get --only-upgrade install kubectl google-cloud-sdk google-cloud-sdk-datastore-emulator google-cloud-sdk-pubsub-emulator google-cloud-sdk-app-engine-go google-cloud-sdk-app-engine-java google-cloud-sdk-app-engine-python google-cloud-sdk-cbt google-cloud-sdk-bigtable-emulator google-cloud-sdk-app-engine-python-extras google-cloud-sdk-datalab

configure gcloud

    config set compute/zone us-west1-a
    gcloud config set project cloudgeniuslabs
    gcloud config set container/new_scopes_behavior true
    rm -rf ~/.kube
    gcloud config list

        [compute]
        region = us-west1
        zone = us-west1-a
        [container]
        new_scopes_behavior = true
        [core]
        account = your@email.address
        disable_usage_reporting = True
        project = cloudgeniuslabs

        Your active configuration is: [default]

    export CLOUDSDK_COMPUTE_REGION=us-west1
    export CLOUDSDK_COMPUTE_ZONE=us-west1-a

Establish a virtual private network

    gcloud compute networks create cloudgenius \
        --project=cloudgeniuslabs  \
        --subnet-mode=auto

Create firewall rules

    gcloud compute --project=cloudgeniuslabs firewall-rules create cloudgenius-allow-icmp \
      --description=Allows\ ICMP\ connections\ from\ any\ source\ to\ any\ instance\ on\ the\ network. \
      --direction=INGRESS \
      --priority=65534 \
      --network=cloudgenius \
      --action=ALLOW \
      --rules=icmp \
      --source-ranges=0.0.0.0/0

    gcloud compute --project=cloudgeniuslabs firewall-rules create cloudgenius-allow-internal \
      --description=Allows\ connections\ from\ any\ source\ in\ the\ network\ IP\ range\ to\ any\ instance\ on\ the\ network\ using\ all\ protocols. \
      --direction=INGRESS \
      --priority=65534 \
      --network=cloudgenius \
      --action=ALLOW \
      --rules=all \
      --source-ranges=10.128.0.0/9

    gcloud compute --project=cloudgeniuslabs firewall-rules create cloudgenius-allow-ssh \
      --description=Allows\ TCP\ connections\ from\ any\ source\ to\ any\ instance\ on\ the\ network\ using\ port\ 22. \
      --direction=INGRESS \
      --priority=65534 \
      --network=cloudgenius \
      --action=ALLOW \
      --rules=tcp:22 \
      --source-ranges=0.0.0.0/0

Carve a subnet

    gcloud compute networks subnets create cg \
        --network=cloudgenius \
        --range 10.64.0.0/19 \
        --secondary-range cg-pods=10.52.0.0/14 \
        --secondary-range cg-services=10.94.0.0/18

Stand up a cluster

    sh create-cluster

Save credentials

    gcloud container clusters get-credentials bingo --zone us-west1-a --project cloudgeniuslabs

Make typing easier

    alias k=kubectl

Confirm the context

    kubectl config current-context

Setup a user for dashboard

    sh create-user

    Save the token from the output from previous step

Allow localhost to proxy the dashboard

    kubectl proxy

Open dashboard locally

    open http://localhost:8001/ui

    login using token saved earlier

install helm

    curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash

install tiller (the server side component) in k8s

    helm init --upgrade
    helm repo update

for helm command line permissions

    kubectl create clusterrolebinding serviceaccounts-cluster-admin  --clusterrole=cluster-admin   --group=system:serviceaccounts

Remove the cluster

    sh remove-cluster

create a GCE persistent disk

    gcloud compute disks create somedisk --type=pd-ssd --size=200GB

optional: format the disk to your taste

    https://cloud.google.com/compute/docs/disks/add-persistent-disk#formatting
