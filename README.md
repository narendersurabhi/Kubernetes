# Kubernetes

## Create a namespace
Run below command to get the list of namespaces available on the cluster.
```sh
kubectl get namespaces
```
Run below command to create a namespace called Development
```sh
kubectl apply -f namespace.yaml
```
To delete namespaces created above. Run below command:
```sh
kubectl delete -f namespace.yaml
```
## Deploy an application
Run below to create a deployment
```sh
kubectl apply -f deployment.yaml
```
Run below to get all the deployments in the namespace development
```sh
kubectl get deployments -n development
```
To delete a deployment run below command:
```sh
kubectl delete -f deployment.yaml
```
To get the list of pods in the namespace development
```sh
kubectl get pods -n development
```
To delete a pod:
```sh
kubectl delete pod pod-info-deployment-5cdffc94c-2hbtn -n development
```
## Check the application health
To check application health:
1. Deploy busybox.yaml to default namespace.
    ```sh
    kubectl apply -f busybox.yaml
    ```
2. Get one of the pods ip address using below command:
    ```sh
    kubectl get pods -n development -o wide
    ```
    It will give response in below format
    ```sh
    NAME                                  READY   STATUS    RESTARTS   AGE   IP          NODE             NOMINATED NODE   READINESS GATES
    pod-info-deployment-5cdffc94c-9b7z8   1/1     Running   0          18m   10.1.0.10   docker-desktop   <none>           <none>
    pod-info-deployment-5cdffc94c-fbbf4   1/1     Running   0          21m   10.1.0.9    docker-desktop   <none>           <none>
    pod-info-deployment-5cdffc94c-jwh6h   1/1     Running   0          21m   10.1.0.7    docker-desktop   <none>           <none>
    ```
3. Get inside the busybox using below command
    ```sh
    kubectl exec -it busybox-54f785c7d7-6hbx4 -- /bin/sh
    ```
    Once inside the shell, run wget 10.1.0.10:3000 to get index.html.
    The port 3000 is from the Deployment.yaml
    Verify by running cat index.html 

## View Application Logs
To check the logs of a pod.
```sh
kubectl logs pod-info-deployment-5cdffc94c-9b7z8 -n development
```

