Service mesh : Service mesh is a popular solution for managing communication between individual Microservice applications
The Sidecar container intercepts all traffic to and from the microservices handling traffic, routing and load balancing service discovery and other important network tasks \
<img width="1849" height="914" alt="image" src="https://github.com/user-attachments/assets/e7f6ed0c-1c1b-4e75-b70f-9a960ba3d398" />
Istio also provides advanced traffic management features like 
canary deployment 
A/B testing and Fault injection
helm search repo istio/base
helm show vaules istio/base --version 1.17.1 > helm-defaults/istio-base-default.yaml
istio installation using terraform
provider file for helm
<img width="1683" height="811" alt="image" src="https://github.com/user-attachments/assets/06e5843b-14f7-42d7-ad5e-18c23f92a4da" />

<img width="1443" height="845" alt="image" src="https://github.com/user-attachments/assets/e9e2582c-7cb3-49db-a354-5e0f9998fd33" />
we have installed istio using terraform and verified the crds on the k8s cluster
helm search repo istio/gateway
helm show vaules istio/gateway --version 1.17.1 > helm-defaults/istio-gateway-default.yaml

Openssl s_client -connect <app url:443> 
<img width="1326" height="974" alt="image" src="https://github.com/user-attachments/assets/51eb615c-145d-4bf8-9110-b39ea89268f9" />
For setting up the monitoring prometheus and grafana
kubectl apply --server-side -f monitor/prometheus-operator-crds
kubectl apply  -f monitor/monitoring-ns.yaml (deploying prometheus operator)
kubectl apply  -R -f monitor/prometheus-operator
kubectl apply -f monitor/prometheus
kubectl apply -f monitor/grafana
<img width="1424" height="372" alt="image" src="https://github.com/user-attachments/assets/49bf8353-ad94-4d62-8858-d73574b1abf1" />
To monitor istio we need to create podmonitor and use istio sidecar labels
<img width="1817" height="353" alt="image" src="https://github.com/user-attachments/assets/eb107c02-727b-4582-98a9-55b7baab7b97" />
<img width="1602" height="610" alt="image" src="https://github.com/user-attachments/assets/26b47a24-b94c-4347-a638-7696c33c3bb5" />
First of all we need to create podmonitor prometheus object we need a names port inthis case
In the second part we need to select those pods based on the label such as istio monitoring
<img width="1909" height="1009" alt="image" src="https://github.com/user-attachments/assets/4c453b1d-4f6e-4aad-9aa9-996554ac5c9f" />
<img width="1849" height="947" alt="image" src="https://github.com/user-attachments/assets/9f4eec77-6e8a-4eb0-b998-25c5184e1b32" />
Based on these two pieces we can start monitoring the istio service mesh
