Grafana Setup On Kubernetes Cluster Using Prometheus. 

1. Grafana-datasource-config.yaml : This datasource configuration is for prometheus, if we want to have more data sources, we can add with different YAMLs under data section 

   $ kubectl create -f grafana-datasource-config.yaml

2. Execute the deployment.yaml 
  
   $ kubectl create -f grafana-deployment.yaml 

3. Execute the service.yaml: We are exposing Grafana on NodePort 32000, we can also expose using ingress or Loadbalancer. 

   $ kubeclt create -f service.yaml. 

We can access the Grafana Dashboard using the Node IP on port 32000, with default username:admin ; password:admin