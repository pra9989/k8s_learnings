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
<# helm repo add istio https://istio-release.storage.googleapis.com/charts
# helm repo update
# helm install my-istio-base-release -n istio-system --create-namespace istio/base --set global.istioNamespace=istio-system
resource "helm_release" "istio_base" {
  name = "my-istio-base-release"

  repository       = "https://istio-release.storage.googleapis.com/charts"
  chart            = "base"
  namespace        = "istio-system"
  create_namespace = true
  version          = "1.17.1"

  set {
    name  = "global.istioNamespace"
    value = "istio-system"
  }
}>
