helm search
helm search mysql
helm install stable/mysql

MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace default inky-otter-mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)
echo $MYSQL_ROOT_PASSWORD

# Run a test Ubuntu pod that you can use as a client:

kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il

    apt-get update && apt-get install mysql-client -y
    mysql -h inky-otter-mysql -p
    exit
    exit

kubectl delete pod ubuntu

helm status inky-otter
helm delete --purge inky-otter
