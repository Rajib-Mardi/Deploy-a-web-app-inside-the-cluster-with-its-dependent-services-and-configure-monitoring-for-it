### Project: 
* Configure Monitoring for Own Application
### Technologiesused: Prometheus, Kubernetes, Node.js, Grafana, Docker, Docker Hub 

### Project Description:

#### Configure our NodeJS application to collect & expose Metrics with Prometheus Client Library

* Metrics of the application to be exposed.
1. no.of requests -the number of requests the application is getting and to check the load of the application.
2. durations of requests - How long does the application take to handle the requests?

#### Build Own Application 
* Build Docker Image from the application
```
- docker build -t rajibmardi/repo:nodeapp .
```
* push  the Docker Image   to private repository
```
- docker push  rajibmardi/repo:nodeapp .
```

#### Deploy the NodeJS application, which has a metrics endpoint configured, into Kubernetes cluster

* Create kubernetes docker login secret for Private docker repository
  ```
  - kubectl create secret docker-registry my-registry-key docker-login --docker-server=https://index.docker.io/v1/ --docker-username=<> --docker-password=<>
  ```
  
* Create Kubernetes Configuration file ```- Ref k8s-config.yaml```
  * Apply the configuration file
  ```
  - kubectl apply -f k8s-config.yaml
  ```

<img src="https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/f4a3c960-06d1-4c09-9f67-7db410d5c2c4" width="750">

* Port-forward the service nodeapp
```
- kubectl port-forward service/nodeapp 3000:3000
```

* Metrics exposed and being tracked




  <img src="https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/ec9f7b04-8e99-43bd-b609-24ed98674773" width="750">


#### Configure Prometheus to scrape this exposed metrics and visualize it in Grafana Dashboard
#### configure monitoring
* Create ServiceMonitor metrics-configuration file ```- Ref own-app-service-monitor.yaml```
* Apply the service monitor configuration file
```
- kubectl apply -f own-app-service-monitor.yaml
```

* check the Target on prometheus uI

<img src="https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/2230fa80-30cd-4d01-9efa-7b2f73784de6" width="750">

* Create Dashboard on Grafana UI
   *  Go to Grafana UI => Click on new Dashboard
    * Add a new Panal => Give it name "Request per second"
    * Run the prom query rate(http_request_operations_total[2m]) to get data for above panel
    * Save the panel
    * This query will return per second load on the application
    * Add a new Panal => Give it name "Request Duration"
    * Run the prom query rate(http_request_duration_seconds_sum[2m]) to get data for above panel
    * Save the panel
    * This query will return duration on the application
 
#### Graph for NodeApp



<img src="https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/1f9ce03a-b86c-43bc-a60f-589346efa6fe" width="750">
