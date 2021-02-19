# K8S 101 Challenge

This is a series of exercices and hands on in order to get into Kubernetes and start working on NGINX Ingress Controller.

## Capture the Flag
Of course, if there is a challenge, there will be a winner (I did not mention anything about a price though)!
Please go create a user account on [Capture the K8S Flag](http://ctfd.f5demolabs.org) and start playing.

But we will never leave you alone:
	- cheat-sheet:
	- flow diagram: 

For every lab you will find a series of questions that will give you coins. You can ask for hints in exchange of some coins.

---

## Lab0 - Getting familiar with your K8S Cluster
### Description

> Understanding the main components of a K8S Clutser, node types, basic networking, meaning and relationship between Services, Endpoints and Pods.

**Useful commands**:

    kubectl cluster-info
    kubectl get nodes
    kubectl get namespaces
    kubectl get service -n *namespace*
    kubectl get endpoints -n *namespace*
    kubectl get pods -n *namespace*

---

## Lab1 - Build and deploy your first application
### Description
	- what is Container and why container registries are important?
	- managing secrets
	- to git, I git, git
	- deploy your application

### Tasks
if you don't have a gitlab.com (free) account, please create one. We will use it as a Source Code Management but mostly here as a private container registry.
When you are done:
 - create a new project
 - create a new Deployment token Username and Password (Settings > Repository > Deploy Tokens)
 - Go to container registry (Package & Registry > Container Registry)
Gitlab is giving you the commands to make docker logged in into your registry along with the 2 needed commands to build and push your container image into your registry. We will use them very soon

<pre>
docker login <i>registry.gitlab.com</i>
</pre>
 
make sure your append a correct tag and version at the end of the build and push commands.

Now, let's download the application code:

<pre>
git clone https://github.com/fchmainy/k8s-trainings-101.git
cd k8s-trainings-101/v1/
docker build -t registry.gitlab.com<i>/f.chmainy/mygitrepo</i><b>/webapp:v1</b> .
docker push registry.gitlab.com<i>/f.chmainy/mygitrepo</i><b>/webapp:v1</b>
</pre>

make sure your append a correct tag and version at the end of the build and push commands Gitlab gave you previously.

Verify the v1 of the webapp container image is on your registry.

We have prepared a model for the kubernetes manifest in order to help you create the service and the deployment. Please modify so it matches your requirements.

Now you can deploy this application in your kubernetes cluster

<pre>
kubectl create ns <b>frontns</b>
kubectl create secret docker-registry <b>regcred</b> --docker-server=<i>registry.gitlab.com</i> --docker-username=<i>your@email.addr</i> --docker-password=<i>yourpassword</i> -n <b>frontns</b>
kubectl apply -f v1_webapp_k8s_manifest.yaml -n <b>frontns</b>
</pre>



> :warning: Don't forget to go check on [CTFD](http://ctfd.f5demolabs.org) if there are any challenges and questions for this section

### Useful commands

<pre>
docker login
docker build
docker push
kubectl create ns
kubectl create secret
kubectl apply
</pre>

---


## Lab2 - Make application accessible from outside
### Description:
	- Look into your app
	- Expose your application
	- Find the instructor container registry Deploy Token Username and Password
	- Deploy the ingress controller and create the Ingress Resource


### Tasks
Understand how your application works:

<pre>
❯ kubectl get svc -n testns -l tier=front -l version=v1
NAME     TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
webapp   ClusterIP   <span style="color:green"><b>10.1.120.33</b></span>   <none>        80/TCP    11m


❯ kubectl describe svc webapp -n testns
Name:              webapp
Namespace:         testns
Labels:            tier=front
                   version=v1
Annotations:       <none>
Selector:          app=webapp,version=v1
Type:              ClusterIP
IP:                <span style="color:green"><b>10.1.120.33</b></span>
Port:              <unset>  <b>80/TCP</b>
TargetPort:        <b>80/TCP</b>
Endpoints:         <span style="color:blue"><b>10.1.96.49:80</b></span>
Session Affinity:  None
Events:            <none>


❯ kubectl get ep -n testns
NAME     ENDPOINTS       AGE
webapp   <span style="color:blue"><b>10.1.96.49:80</b></span>   13m

❯ kubectl get pods -n testns -o wide
NAME                      READY   STATUS    RESTARTS   AGE   IP           NODE                                NOMINATED NODE   READINESS GATES
webapp-7dd5ff6788-t8xdt   1/1     Running   0          11m  <span style="color:blue"><b> 10.1.96.49</b></span>   aks-agentpool-17587948-vmss000000   <none>           <none>
</pre>


There are many ways to make your application from the outside, first and foremost the **port-forward** which is mostly used for troubleshooting as it is not permanent.




**Useful commands**:
'''
kubectl port-forward deployment/webapp 5000:80
kubectl port-forward pods/webapp-867RY3R36R36 5000:80
kubectl port-forward service/webapp-svc 5000:webapp
'''

## Lab3 - Manage versioning of your application with Ingress
### Description
	- Deploy the version 2 (v2) of the web application
	- Deployment Strategies (Canary Realease, A/B Testing...)
	
You have multiple example you can inspire from at: https://github.com/nginxinc/kubernetes-ingress/tree/master/examples-of-custom-resources

**Useful commands**:
'''
'''

## Lab4 - East-West or Microservice-to-Microservice traffic
### Description
	- Deploy backend service
	- access your application and capture the flag!!!

**Useful commands**:
'''
'''

