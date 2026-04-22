Agenda:
what is CNI ?
Kubernetes networking = pod and host netowrking
what do you do with CNI ?
kubernetes network policy
CNI Evolution - pod security group
kubernetes network policy vs service mesh
https://www.youtube.com/watch?v=kA0C44nTwjU
CNI excutes multiple plugins
Execute bridge plugin
execute ipvlan plugin
execute static plugin
https://medium.com/@hmquan08011996/kubernetes-networking-cilium-up-and-running-928114530cdb
https://medium.com/@simardeep.oberoi/cilium-a-comprehensive-guide-to-networking-security-and-observability-in-kubernetes-41e11fa69d15
https://medium.com/@abhishekvala06/building-a-production-grade-kubernetes-cluster-with-cilium-cni-the-complete-guide-78e58ddeb6e7

What is a Service Mesh?

A service mesh (like Istio or Linkerd) manages communication between microservices.

Key capabilities:
Traffic routing (canary, blue/green)
Retries, timeouts, circuit breaking
mTLS (service-to-service encryption)
Observability (tracing, metrics, logs)
Policy enforcement at application level (HTTP, gRPC)
How it works:
Usually uses sidecar proxies (e.g., Envoy)
Intercepts all service-to-service traffic

👉 Think of it as:
“Smart traffic manager for microservices”

🔹 What is Cilium?

Cilium is a Kubernetes networking and security solution built on eBPF.

Key capabilities:
Pod networking (replaces kube-proxy if needed)
Network policies (L3/L4 + L7)
High-performance load balancing
Observability (Hubble)
Security using identity-based policies
How it works:
Runs in the kernel using eBPF (no sidecars needed)
Works at network level, with optional L7 filtering

👉 Think of it as:
“High-performance networking + security engine for Kubernetes” 
<img width="1286" height="839" alt="image" src="https://github.com/user-attachments/assets/17a6511b-5aca-4ff7-bbc9-7b79ad16a509" />

When to use what?
Use Service Mesh when:
You need advanced traffic control (canary, retries)
You want deep observability (tracing)
You require strict service-to-service mTLS
Use Cilium when:
You want high-performance networking
You need strong network security policies
You want to avoid sidecar overhead
You are optimizing for scale and efficiency
End-to-End Traffic Flow
🚀 External → Internal
User hits DNS → Cloud Load Balancer
Traffic reaches Istio Ingress Gateway
Gateway routes traffic using:
VirtualService
DestinationRule
Request goes to service pod
Envoy sidecar intercepts request
Cilium enforces network policy + routes packet
