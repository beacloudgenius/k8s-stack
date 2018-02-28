sudo apt-get update && sudo apt-get --only-upgrade install kubectl google-cloud-sdk google-cloud-sdk-datastore-emulator google-cloud-sdk-pubsub-emulator google-cloud-sdk-app-engine-go google-cloud-sdk-app-engine-java google-cloud-sdk-app-engine-python google-cloud-sdk-cbt google-cloud-sdk-bigtable-emulator google-cloud-sdk-app-engine-python-extras google-cloud-sdk-datalab

gcloud config set compute/zone us-west1-a
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

rm -rf ~/.kube

gcloud compute networks create cloudgenius \
    --project=cloudgeniuslabs  \
    --subnet-mode=auto

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

gcloud compute networks subnets create cg \
    --network=cloudgenius \
    --range 10.64.0.0/19 \
    --secondary-range cg-pods=10.52.0.0/14 \
    --secondary-range cg-services=10.94.0.0/18

sh create-cluster

##### sh remove-cluster

# create a GCE persistent disk
gcloud compute disks create somedisk --type=pd-ssd --size=200GB

# optional: format the disk to your taste
# https://cloud.google.com/compute/docs/disks/add-persistent-disk#formatting

gcloud container clusters get-credentials bingo --zone us-west1-a --project cloudgeniuslabs

alias k=kubectl

kubectl config current-context

sh create-user

save the token from the output from previous step

kubectl proxy

open http://localhost:8001/ui

# install helm
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash

# install tiller (the server side component) in k8s

helm init --upgrade
helm repo update

# for helm command line permissions
kubectl create clusterrolebinding serviceaccounts-cluster-admin  --clusterrole=cluster-admin   --group=system:serviceaccounts
