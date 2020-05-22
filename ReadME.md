1.	Create Namespace: Execute below command to create new namespace 

    $ kubectl create namespace test-prometheus 

2.	Create A Config Map: Create a config map with all the prometheus scrape config and alerting rules. Prometheus.yaml contains the configuration to dynamically discover pods and services running in the kubernetes cluster, prometheus.rules will contain all the alerts rules for sending alerts to alert manager. 

    $ kubectl create -f ConfigMap.yaml -n test-prometheus

The following jobs are being configured as targets using service discovery.
  - kubernetes-apiservers: Gets metrics on the Kubernetes APIs.
  - kubernetes-nodes: Gets metrics on the Kubernetes nodes.
  - kubernetes-pods: Gets metrics from Pods that have the prometheus.io/scrape and prometheus.io/port annotations defined in the metadata.
  - kubernetes-cadvisor: Gets cAdvisor metrics reported from the Kubernetes cluster.
  - kubernetes-service-endpoints: Gets metrics from Services that have the prometheus.io/scrape and prometheus.io/port annotations defined in the metadata.

3.	Create A Prometheus Deployment:  
    a.	Create a deployment on test-prometheus namespace using the deployment file and command below 

     $ kubectl create -f prometheus-deployment.yaml -n test-prometheus

    b.	Check the deployments created  using below command 

     $ kubectl get deployments –n test-prometheus

4.	Connecting to Prometheus: We can connect to prometheus using kubectl port forwarding exposing the Prometheus deployment as a service with NodePort or a Load Balancer. 

- Using Kubectl Port Forwarding: 

a.	Get the prometheus pod name 

    $ kubectl get pods -n test-prometheus
NAME                                     READY   STATUS              RESTARTS   AGE
prometheus-deployment-68f695c7c8-qvhgr   0/1     Running   0          56m
$ 

b.	Execute below command to access prometheus form localhost port 8080

     $ kubectl port-forward prometheus-deployment-68f695c7c8-qvhgr   8080:9090 –n test-prometheus

    https://localhost:8080 -- to access prometheus 

- Exposing Prometheus as a service: To access prometheus over IP or DNS, we need to expose it as kubernetes service. 

     $ kubectl create –f service.yaml –n test-prometheus

Once the service is created, we can access prometheus using kubernetes IP on port 30000. 
