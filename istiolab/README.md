# Hands-On Workshop- Istio Lab

This example deploys a sample application composed of four separate microservices used to demonstrate various Istio features. The application displays information about a book, similar to a single catalog entry of an online book store. Displayed on the page is a description of the book, book details (ISBN, number of pages, and so on), and a few book reviews.


This application is polyglot, i.e., the microservices are written in different languages. It’s worth noting that these services have no dependencies on Istio, but make an interesting service mesh example, particularly because of the multitude of services, languages and versions for the reviews service.


To run the sample with Istio requires no changes to the application itself. Instead, we simply need to configure and run the services in an Istio-enabled environment, with Envoy sidecars injected along side each service. The needed commands and configuration vary depending on the runtime environment


## Pre-requisite:

For local desktop clients.

In case you are going to use Web Terminal, Task 2

1) Install IBM CLI
[https://github.com/IBM-Cloud/ibm-cloud-cli-release/releases/v0.15.1](https://github.com/IBM-Cloud/ibm-cloud-cli-release/releases/v0.15.1)

[Windows 64 bit](https://clis.ng.bluemix.net/download/bluemix-cli/0.15.1/win64)


2) Install Kubectl 

`curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.14.0/bin/windows/amd64/kubectl.exe`



### Task 1: Accessing a Kubernetes cluster with IBM Cloud Kubernetes service

You must already have a cluster created. Here are the steps to access your cluster.

**Install IBM Cloud Kubernetes Service command line utilities**

1.Install the IBM Cloud command line interface.

2.Log in to the IBM Cloud CLI.

`ibmcloud login`

3.Install the IBM Cloud Kubernetes Service plug-in.

`ibmcloud plugin install container-service`

4.To verify that the plug-in is installed properly, run 

`ibmcloud plugin list`

The Container Service plug-in is displayed in the results as container-service/kubernetes-service.

5.Initialize the Container Service plug-in and point the endpoint to your region. For example when prompted, enter 4 for uk-south.

```
Example:

ibmcloud cs region-set

Choose a region:
1. ap-north
2. ap-south
3. eu-central
4. uk-south
5. us-east
6. us-south
Enter a number> 4
```

6.Install the kubectl Kubernetes CLI. Go to the Kubernetes page, and follow the steps to install the CLI.

#### Access your cluster


Learn how to set the context to work with your cluster by using the kubectl CLI, access the Kubernetes dashboard, and gather basic information about your cluster.


1.Set the context for your cluster in your CLI. Every time you log in to the IBM Cloud Kubernetes Service CLI to work with the cluster, you must run these commands to set the path to the cluster's configuration file as a session variable. The Kubernetes CLI uses this variable to find a local configuration file and certificates that are necessary to connect with the cluster in IBM Cloud.


a. List the available clusters.

`ibmcloud cs clusters`


b. Set an environment variable for your cluster name(Replace export with SET for Windows)
`
export MYCLUSTER=<your_cluster_name>`


c. Download the configuration file and certificates for your cluster using the cluster-config command.

`ibmcloud cs cluster-config $MYCLUSTER`


d. Copy and paste the output command from the previous step to set the KUBECONFIG environment variable and configure your CLI to run kubectl commands against your cluster.(Replace export with SET for Windows)

`Example:
export KUBECONFIG=/Users/user-name/.bluemix/plugins/container-service/clusters/mycluster/kube-config-hou02-mycluster.yml`


2.Get basic information about your cluster and its worker nodes. This information can help you manage your cluster and troubleshoot issues.

a. View details of your cluster.
		
`ibmcloud cs cluster-get $MYCLUSTER`
        
        
b. Verify the worker nodes in the cluster.


`ibmcloud cs workers $MYCLUSTER

ibmcloud cs worker-get <worker_ID>`
        
        
3.Validate access to your cluster.

a. View nodes in the cluster.

`kubectl get node`

b. View services, deployments, and pods.

`kubectl get svc,deploy,po --all-namespaces`


### Task 2:  Clone the lab repo

1.From your command line, run:

`git clone https://github.com/IBM/istio101

cd istio101/workshop`

This is the working directory for the workshop. You will use the example .yaml files that are located in the workshop/plans directory in the following exercises.


### Task 3: Installing Istio on IBM Cloud Kubernetes service


In this module, you will have to download and install Istio.


1.Either download Istio directly from https://github.com/istio/istio/releases or get the latest version by using curl:

`curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.2.4 sh -`

2.Change the directory to the Istio file location.

`cd istio-<version-number>`


3.	Add the istioctl client to your PATH.(Replace export with SET for Windows)

`export PATH=$PWD/bin:$PATH`


4.	Install Istio’s Custom Resource Definitions via kubectl apply, and wait a few seconds for the CRDs to be committed in the kube-apiserver:

`for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done`


5.	Now let's install Istio demo profile into the istio-system namespace in your Kubernetes cluster:

`kubectl apply -f install/kubernetes/istio-demo.yaml`


6.	Ensure that the istio-* Kubernetes services are deployed before you continue.

`kubectl get svc -n istio-system`


```
Sample output:
NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                                                                                                                                      AGE
grafana                  ClusterIP      172.21.135.33    <none>           3000/TCP                                                                                                                                     35s
istio-citadel            ClusterIP      172.21.242.77    <none>           8060/TCP,15014/TCP                                                                                                                           34s
istio-egressgateway      ClusterIP      172.21.20.200    <none>           80/TCP,443/TCP,15443/TCP                                                                                                                     35s
istio-galley             ClusterIP      172.21.246.214   <none>           443/TCP,15014/TCP,9901/TCP                                                                                                                   36s
istio-ingressgateway     LoadBalancer   172.21.151.128   169.60.168.234   80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:32268/TCP,15030:30743/TCP,15031:32200/TCP,15032:31341/TCP,15443:31059/TCP,15020:31039/TCP   35s
istio-pilot              ClusterIP      172.21.243.70    <none>           15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       34s
istio-policy             ClusterIP      172.21.144.137   <none>           9091/TCP,15004/TCP,15014/TCP                                                                                                                 34s
istio-sidecar-injector   ClusterIP      172.21.230.192   <none>           443/TCP                                                                                                                                      33s
istio-telemetry          ClusterIP      172.21.213.11    <none>           9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       34s
jaeger-agent             ClusterIP      None             <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                                   29s
jaeger-collector         ClusterIP      172.21.187.128   <none>           14267/TCP,14268/TCP                                                                                                                          29s
jaeger-query             ClusterIP      172.21.89.210    <none>           16686/TCP                                                                                                                                    30s
kiali                    ClusterIP      172.21.219.101   <none>           20001/TCP                                                                                                                                    35s
prometheus               ClusterIP      172.21.53.185    <none>           9090/TCP                                                                                                                                     34s
tracing                  ClusterIP      172.21.6.64      <none>           80/TCP                                                                                                                                       29s
zipkin                   ClusterIP      172.21.229.37    <none>           9411/TCP                                                                                                                                     29s
```

Note: If your istio-ingressgateway service IP is , confirm that you are using a standard/paid cluster. Free cluster is not supported for this lab.

1.Ensure the corresponding pods istio-citadel-*, istio-ingressgateway-*, istio-pilot-*, and istio-policy-* are all in Running state before you continue.

`kubectl get pods -n istio-system`

```
Sample output:
NAME                                      READY   STATUS      RESTARTS   AGE
grafana-5c45779547-v77cl                  1/1     Running     0          103s
istio-citadel-79cb95445b-29wvj            1/1     Running     0          102s
istio-cleanup-secrets-1.1.0-mp6qq         0/1     Completed   0          112s
istio-egressgateway-6dfb8dd765-jzzxf      1/1     Running     0          104s
istio-galley-7bccb97448-tk8bz             1/1     Running     0          104s
istio-grafana-post-install-1.1.0-bvng6    0/1     Completed   0          113s
istio-ingressgateway-679bd59c6-5bsbr      1/1     Running     0          104s
istio-pilot-674d4b8469-ttxs8              2/2     Running     0          103s
istio-policy-6b8795b6b5-g5m2k             2/2     Running     2          103s
istio-security-post-install-1.1.0-cfqpx   0/1     Completed   0          111s
istio-sidecar-injector-646d77f96c-55twm   1/1     Running     0          102s
istio-telemetry-76c8fbc99f-hxskk          2/2     Running     2          103s
istio-tracing-5fbc94c494-5nkjd            1/1     Running     0          102s
kiali-56d95cf466-bpgfq                    1/1     Running     0          103s
prometheus-8647cf4bc7-qnp6x               1/1     Running     0          102s
```

Before you continue, make sure all the pods are deployed and are either in the Running or Completed state. If they're in pending state, wait a few minutes to let the deployment finish.

Congratulations! You successfully installed Istio into your cluster.

### Task 4: Deploy the App


1.Change directory to the root of the Istio installation.


2.The default Istio installation uses automatic sidecar injection. Label the namespace that will host the application with istio-injection=enabled:

	`$ kubectl label namespace default istio-injection=enabled`
	
	
3.Deploy your application using the kubectl command:

	`$ kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml`

4.Confirm all services and pods are correctly defined and running:

	`$ kubectl get services`

	```
	NAME                       CLUSTER-IP   EXTERNAL-IP   PORT(S)              AGE
	details                    10.0.0.31    <none>        9080/TCP             6m
	kubernetes                 10.0.0.1     <none>        443/TCP              7d
	productpage                10.0.0.120   <none>        9080/TCP             6m
	ratings                    10.0.0.15    <none>        9080/TCP             6m
	reviews                    10.0.0.170   <none>        9080/TCP             6m
	```
and
	`$ kubectl get pods`
```
	NAME                                        READY     STATUS    RESTARTS   AGE
	details-v1-1520924117-48z17                 2/2       Running   0          6m
	productpage-v1-560495357-jk1lz              2/2       Running   0          6m
	ratings-v1-734492171-rnr5l                  2/2       Running   0          6m
	reviews-v1-874083890-f0qf0                  2/2       Running   0       ```6m
	reviews-v2-1343845940-b34q5                 2/2       Running   0          6m
	reviews-v3-1813607990-8ch52                 2/2       Running   0          6m
```



5. Determining the ingress IP and ports

Now that the Bookinfo services are up and running, you need to make the application accessible from outside of your Kubernetes cluster, e.g., from a browser. An Istio Gateway is used for this purpose.

1.Define the ingress gateway for the application:

`$ kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml`


2.Confirm the gateway has been created:

```
	$ kubectl get gateway
	NAME               AGE
	bookinfo-gateway   32s
```

3.Execute the following command to determine if your Kubernetes cluster is running in an environment that supports external load balancers:


	`$ kubectl get svc istio-ingressgateway -n istio-system`
	
```
	NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                                      AGE
	istio-ingressgateway   LoadBalancer   172.21.109.129   130.211.10.121  80:31380/TCP,443:31390/TCP,31400:31400/TCP   17h

```

4.Open the below link by replacing it with Public IP of IBM Kubernetes Cluster

```
http://<replace_with_public_ibm_kube_ip:31380/productpage
```

You have deploye the application successfully.
Now let us use Istio capabilities, to manage these individual microservices.

### Task 5: Request Routing

In the above application URL, when you keep on hitting the URL, you will get different versions of review service.

But using Traffic management , you will see few examples here.

## A.	Pointing all request to v1 of review

`$kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml`

You can delete this rule and again check for the previous behavior of services.

`$kubectl delete -f samples/bookinfo/networking/virtual-service-all-v1.yaml`

## B. Apply weight-based routing

Transfer 50% of the traffic from reviews:v1 to reviews:v3 with the following command

`$kubectl apply -f samples/bookinfo/networking/virtual-service-reviews-50-v3.yaml`

check rule replaced

`$kubectl get virtualservice reviews -o yaml`

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
  ...
spec:
  hosts:
  - reviews
  http:
  - route:
    - destination:
        host: reviews
        subset: v1
      weight: 50
    - destination:
        host: reviews
        subset: v3
      weight: 50
```
`

Congratulations, you have completed the workshop.
