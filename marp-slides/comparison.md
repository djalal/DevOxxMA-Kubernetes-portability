

### Authentication
- **IAM vs. Azure AD vs. Google IAM**: Each platform has its own identity and access management system.

// Luckily you can typically set this up once and you're good to go. The problem occurs mainly once you have a problem, then you need specialized knowledge for the system you're dealing with. 

//tools
// https://pinniped.dev/
// https://www.tremolosecurity.com/

### Networking
- **VPC vs. VNet vs. GCP Network**: Networking configurations differ across all three platforms.

// AWS is going to required you to construct this underlying VPC before you can deploy a cluster.
// AKS and GKE more or less handle the networking pieces for you.

- **AWS ALB vs. Azure Application Gateway vs. Google HTTP(S) Load Balancer**: Each has its own set of features and configurations.

// Each of these has a very different configuration, but should provide you with the same results in the end.
// Things to avoid are uniqe services. 

// AWS pods as targets, 
// AKS multiple frontend services
// GKE has the best gRPC support, so it could pin you in a corner 

Agnostic choices
| Ingress Controller | Key Features                  | Custom Resource Definitions | Data Plane |
|--------------------|-------------------------------|-----------------------------|------------|
| Nginx              | SSL, Path-based routing, WebSockets | No                          | Nginx      |
| Traefik            | HTTP/2, gRPC, Let's Encrypt   | Yes                         | Traefik    |
| HAProxy            | SSL, Rate limiting            | No                          | HAProxy    |
| Kong               | API Gateway features          | Yes                         | Kong       |
| Istio Gateway      | Advanced traffic routing      | Yes                         | Envoy      |
| Contour            | HTTP/2, Let's Encrypt         | Yes                         | Envoy      |
| Ambassador         | API Gateway features          | Yes                         | Envoy      |
| Gloo               | Function-level routing        | Yes                         | Envoy      |


### Compute and memory
- **Nodes sizes**: You can choose similarly sized VMs but never exactly the same.

// Pay close attention to requests and limit sizes. Are you microservices CPU intensive or Memory intensive. Planning for one, but making sure spikes in the other don't catch you off guard. 


| Cloud Provider | General Purpose | Compute Optimized | Memory Optimized | GPU Instances | Burstable/Short-lived |
|----------------|-----------------|-------------------|------------------|--------------|----------------------|
| AWS (EKS)       | m5, m6g         | c5, c6g           | r5, r6g          | p3, g4       | t3, t4g              |
| Azure (AKS)     | Ds_v3, Ds_v4    | Fs_v2             | Es_v3, Ms_v2     | NC_v3, NV_v4 | B-series             |
| Google (GKE)    | n1-standard, e2-standard | c2-standard | m2-ultramem   | n1-standard with GPUs | Preemptible Instances |

### Node Management
- **Managed Node Groups vs. Azure Managed Instances vs. GKE Autopilot**: Each service has different offerings for managed Kubernetes nodes.

// You can also drop node pools all together in favor of a serverless approach, but you then end up with needed very specilized knowledge when tracking down problems that happen at the OS level. 


| Cloud Provider      | Resource Type  | Serverless Option |
|---------------------|--------------- |--------------------|
| AWS (EKS)           | Node Groups    | Fargate Profiles   |
| Azure (AKS)         | Node Pools     | Azure Container Instances |
| Google (GKE)        | Node Pools     | GKE Autopilot      | 


### Storage
- **EBS vs. Azure Disk vs. Google Persistent Disk**: Each cloud provider has its own block storage solutions.
- **Storage Class Mismatch**: Default storage classes differ across platforms, requiring manual mapping.

// Doing this wrong will really piss off your Devs. Getting it right though will make the experience so your Devs don't ever even think about the underlying architecture. Which is our ultimate goal, make a platform experience Devs don't have to worry about. 

| Feature/Parameter       | Amazon EKS (AWS)       | Azure AKS              | Google GKE             |
|-------------------------|------------------------|------------------------|------------------------|
| **Provisioner**         | kubernetes.io/aws-ebs  | kubernetes.io/azure-disk| kubernetes.io/gce-pd  |
| **Default Volume Type** | gp2 (General Purpose)  | Standard_LRS           | pd-standard            |
| **High IOPS Option**    | io1, io2               | Premium_LRS            | pd-ssd                 |
| **Encryption**          | KMS                    | Azure Disk Encryption  | Google KMS             |

### DBs
- **AWS RDS vs. Azure SQL vs. Cloud SQL**: Managed database services are different across each cloud provider.

| Cloud Provider | Managed SQL Options          | High Availability |
|----------------|------------------------------|-------------------|
| AWS (EKS)      | RDS, Aurora                  | Multi-AZ          |
| Azure (AKS)    | Azure SQL, MySQL/PostgreSQL  | Geo-Replication   |
| Google (GKE)   | Cloud SQL                    | Regional Replicas |

### Service Mesh

// Each cloud comes with a prefered service mesh offering. 
// OMS and Istio will play friendly across k8s types.
// Azure used to use Azure Service Fabric Mesh which was retired for OMS

| Cloud Provider      | Native Service Mesh Option | Proxy Used |
|---------------------|----------------------------|------------|
| AWS (EKS)           | AWS App Mesh               | Envoy      |
| Azure (AKS)         | Open Service Mesh          | Envoy      |
| Google (GKE)        | Istio                      | Envoy      |


### Compliance and Security, Logging and Monitoring
- **AWS, Azure, and GCP Compliance**: Each cloud provider has its own set of compliance certifications and security tools.

* May not be accurate 
| Feature/Tool                  | Amazon Web Services (AWS)  | Azure                    | Google Cloud Platform (GCP) |
|-------------------------------|----------------------------|--------------------------|-----------------------------|
| **Resource Tracking**         | AWS Config                 | Azure Policy             | Cloud Security Command Center|
| **Audit Logs**                | AWS CloudTrail             | Azure Monitor            | Cloud Audit Logs            |
| **Threat Detection**          | AWS GuardDuty              | Azure Security Center    | Cloud Security Command Center|
| **Compliance Reports**        | AWS Artifact               | Azure Compliance Manager | Access Transparency         |
| **Data Classification**       | AWS Macie                  | Azure Information Protection | Cloud Data Loss Prevention (DLP)|

// each cloud offers a whole suite of compliance tools you have to familiarize yourself with. 

### Multi cloud tools

| Multi-Cluster Tool | Key Features                  | Community Focus       | Cloud Support                  |
|--------------------|-------------------------------|-----------------------|--------------------------------|
| Rancher            | Centralized management, Security | Large, Active       | AWS, Azure, GCP, On-premises   |
| Argo CD            | GitOps, Continuous Delivery    | CNCF                 | Any Kubernetes Cluster         |
| Kubefed            | Resource Sync, Policy Control  | Kubernetes SIG       | Any Kubernetes Cluster         |
| Anthos             | Hybrid, Centralized Management | Google Cloud         | GCP, AWS, On-premises, Azure (preview) |
| OpenShift          | Enterprise Kubernetes          | Red Hat              | AWS, Azure, GCP, IBM, On-premises |
| Spinnaker          | Continuous Delivery            | Netflix, Google      | AWS, Azure, GCP, Kubernetes    |
| Crossplane         | Multi-cloud Control Plane      | CNCF                 | AWS, Azure, GCP, Alibaba Cloud |


### Cost Management
- **AWS Cost Explorer vs. Azure Cost Management vs. Google Cloud Platform's Cost Tools**: Different tools and pricing models for managing costs.

// Each cloud will gladly charge you and an arm and leg for compute and data transfer. As you add additional services you may see wide swings in their pricing models.

### Best practices, using toolsets common across all cluster types

- **Prometheus and Grafana**: For monitoring and visualization.
- **Helm**: Package management.
- **Calico**: Works as a CNI for Network policies.