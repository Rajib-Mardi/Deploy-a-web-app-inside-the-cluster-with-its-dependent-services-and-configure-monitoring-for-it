Demo Project: Configure Monitoring for Own Application Technologiesused: Prometheus, Kubernetes, Node.js, Grafana, Docker, Docker Hub Project Description:


#### Configure our NodeJS application to collect & expose Metrics with Prometheus Client Library

Metrics
1. no.of requests
2. durations of requests


Build Docker Image from the application
```
- docker build -t rajibmardi/repo:nodeapp .
```
push  the Docker Image   to private repository
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
    
![kubectl 23-11-2023 22_06_38](https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/f4a3c960-06d1-4c09-9f67-7db410d5c2c4)

Port-forward the service nodeapp
```
- kubectl port-forward service/nodeapp 3000:3000
```

* Metrics exposed and being tracked

  ![127 0 0 1_3000_metrics - Google Chrome 23-11-2023 22_07_39](https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/ec9f7b04-8e99-43bd-b609-24ed98674773)


#### Configure Prometheus to scrape this exposed metrics and visualize it in Grafana Dashboard
#### configure monitoring
* Create ServiceMonitor metrics-configuration file ```- Ref own-app-service-monitor.yaml```
* Apply the service monitor configuration file
```
- kubectl apply -f own-app-service-monitor.yaml
```

* check the Target on prometheus ui

  ![Prometheus Time Series Collection and Processing Server and 25 more pages - Profile 1 - Microsoft​ Edge 24-11-2023 00_10_28](https://github.com/Rajib-Mardi/nodejs-app-monitoring/assets/96679708/2230fa80-30cd-4d01-9efa-7b2f73784de6)


* Create Dashboard on Grafana UI
  1.Goto Grafana UI => Click on new Dashboard
Add a new Panal => Give it name "Request per second"
Run the prom query rate(http_request_operations_total[2m]) to get data for above panel
Save the panel
This query will return per second load on the application
Add a new Panal => Give it name "Request Duration"
Run the prom query rate(http_request_duration_seconds_sum[2m]) to get data for above panel
Save the panel
This query will return duration on the application
