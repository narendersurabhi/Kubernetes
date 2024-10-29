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
### Check the application health
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

### View Application Logs
To check the logs of a pod.
```sh
kubectl logs pod-info-deployment-5cdffc94c-9b7z8 -n development
```

## Complex Application Deployment
Use below steps to deploy add a load balancer to the cluster
### Add load balancer
Run below command 
```sh
kubectl apply -f service.yaml
```
This will create a load balancer service. The service forwards the traffic to all the pods that have selector app: pod-info.
The traffic is sent to the port 3000 on the pod
Load balancer service allows traffic on port 80. As this is default port, we don't have to specify the port number while accessing the external IP address to this load balancer.

Run below command to get the load balancer and its external IP.
```sh
kubectl get services -n development
```

Try accessing the external IP from the browser to check if the setup.

### Add resource requests and limits
Add below section in deployment.yaml to add resource requests and limits:
```sh
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
```
Then run ```sh kubectl apply -f deployment.yaml``` to update the deployment.
This is going to terminate all the pods and redeploy.

### Tear down the cluster and all the resources created
```sh
$ kubectl delete -f service.yaml 
service "demo-service" deleted

$ kubectl delete -f deployment.yaml 
deployment.apps "pod-info-deployment" deleted

$ kubectl delete -f quote.yaml 
deployment.apps "quote-service" deleted

$ kubectl delete -f busybox.yaml 
deployment.apps "busybox" deleted

$ kubectl delete -f namespace.yaml 
namespace "development" deleted
namespace "production" deleted
```
