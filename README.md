# Deploying Multiple Microservices with OpenTelemetry for End-to-End Observability

## INTRODUCTION
OpenSearch is not the only critical component of modern observability architecture; modern cloud-native systems also demand standardized telemetry collection across distributed environments. 

OpenTelemetry (OTel) is an open-source, vendor-neutral observability framework for generating, collecting, and exporting telemetry data, namely metrics, logs, and traces. 

As the second most active Cloud Native Computing Foundation (CNCF) project after Kubernetes, OpenTelemetry unifies older standards like OpenTracing and OpenCensus.

Before OpenTelemetry, organizations relied on fragmented tooling (e.g., Prometheus for metrics, Jaeger for traces, and the EFK stack for logs), requiring hardcoded client libraries in application code that made switching vendors costly and complex. 

OpenTelemetry acts as a universal "power plug standard," allowing applications to instrument code once using its API/SDKs and route telemetry via the OpenTelemetry Collector to any compatible backend (such as Prometheus, Jaeger, Grafana, or Zipkin) without altering application logic.   

This guide outlines the deployment of the OpenTelemetry Astronomy Shop: a multi-service distributed microservices demo application onto a Kubernetes cluster using Helm.

#### ACCESS PROJECT MATERIALS
#### PDF GUIDE: [DEPLOYING MICROSERVICES WITH OPENTELEMETRY FOR END TO END OBSERVABILITY.pdf](https://github.com/user-attachments/files/34144521/DEPLOYING.MICROSERVICES.WITH.OPENTELEMETRY.FOR.END.TO.END.OBSERVABILITY.pdf)
#### WATCH VIDEO WALKTHROUGH HERE: https://youtu.be/KpgPW-wh72Y

### STEP-BY-STEP DEPLOYMENT GUIDE

##### STEP 1: Provision a High-Capacity Kubernetes Environment

Because running the comprehensive OpenTelemetry demo stack alongside internal backends (Jaeger, Prometheus, Grafana) requires substantial system resources, provision a high-capacity development environment (such as a GitHub Codespace configured with 4-Core, 16 GB RAM, and 32 GB storage).

1) Verify pre-installed tools and allocate resources for Minikube:

   <PRE>nproc && free -h</PRE>
   <PRE>docker --version</PRE>
   <PRE>minikube version</PRE>
   <PRE>kubectl version --client</PRE>
   <PRE>helm version</PRE>

   <PRE>minikube start --driver docker --network host --cpus=4 --memory=12288</PRE>
   <PRE>kubectl get nodes</PRE>

##### STEP 2: Deploy the OpenTelemetry Demo (Astronomy Shop) via Helm

1) Add and update the official OpenTelemetry Helm chart repository:

   <PRE>helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts</PRE>
   <PRE>helm repo update</PRE>

2) Install the OpenTelemetry demo application (my-otel-demo) into your Kubernetes cluster:

   <PRE>helm install my-otel-demo open-telemetry/opentelemetry-demo</PRE>

3) Verify that all components (including Jaeger, Prometheus, Grafana, and microservice pods) are initializing and passing readiness checks (approximately 28–30 pods should be running):

   <PRE>kubectl get pods</PRE>
   <PRE>kubectl get svc</PRE>

##### STEP 3: Access and Test End-to-End Observability

1) Expose the Frontend Proxy:
   Use port-forwarding to access the Astronomy Shop user interface from your local browser:

   <PRE>kubectl --namespace default port-forward svc/frontend-proxy 8080:8080</PRE>
   *(For remote cloud virtual machines or EC2 instances, include `--address 0.0.0.0` and ensure inbound security group rules permit port 8080)*.

2) Generate Telemetry Data:
   Open http://localhost:8080 in your browser.
   Interact with the storefront by adding items to your shopping cart or switching currencies to trigger live metrics and trace generation via the built-in load generator.

3) Inspect Distributed Traces in Jaeger:
   Access the Jaeger UI via your environment's forwarded endpoint or by navigating to the jaeger/ui path on your codespace URL.
   Select cart under the Services filter and click Find Traces to analyze end-to-end request flow, service spans, and latencies across the microservice architecture.

   
