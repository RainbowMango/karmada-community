# General Technical Review - Karmada / Incubation

- **Project:** Karmada
- **Project Version:** v0.14.0
- **Website:** https://karmada.io/
- **Date Updated:** 2025-07-21
- **Template Version:** v1.0
- **Template Link:** [cncf/toc/general-technical-questions.md](https://github.com/cncf/toc/blob/8e54d360a0e6a045f5aa7fff0eeac323e281f962/.archive/resources/toc-supporting-guides/general-technical-questions.md)
- **Description:** Open, Multi-Cloud, Multi-Cluster Kubernetes Orchestration
- **Reviewers:**

## Overview
This document provides a comprehensive technical review of the Karmada project as part of its CNCF Graduation process. It is intended to give reviewers, contributors, and adopters a clear understanding of Karmada’s 
architecture, design principles, security posture, installation and operational procedures, and its alignment with cloud native best practices.

The review is structured to follow the typical lifecycle of a cloud native project, covering planning, installation, security, enablement, upgrade, and operational considerations. 
It addresses both technical and organizational aspects, including user personas, use cases, compliance, and governance. The goal is to ensure transparency, highlight strengths, and identify any areas for improvement as Karmada progresses through the CNCF Graduation process.

## Day 0 - Planning Phase

### Scope

#### Describe the roadmap process, how scope is determined for mid to long term features, as well as how the roadmap maps back to current contributions and maintainer ladder?

The Karmada roadmap is guided by the project’s vision and strategic goals for open, multi-cloud, multi-cluster Kubernetes orchestration. The roadmap is collaboratively defined by the maintainers and the broader community, and is regularly updated to reflect upcoming releases and long-term objectives. See the [Karmada Roadmap](https://github.com/karmada-io/community/blob/main/ROADMAP.md) for details. 

- **Feature Gathering:**
  - Feature requests and proposals are collected from:
    - GitHub issues and proposals submitted by contributors
    - Feedback from users during community meetings
    - Input from the Karmada Adopter Group
    - Community-driven planning sessions
- **Scope Determination:**
  - **Mid-term:** Features are prioritized based on immediate impact and feasibility.
  - **Long-term:** Direction is set by technical leadership and alignment with the broader cloud native ecosystem.
- **Mapping to Contributions:**
  - Issues are tagged with milestone labels and tracked on release project boards to ensure alignment with the roadmap.
- **Mapping to Maintainer Ladder:**
  - The maintainer ladder is used to assign tasks and recognize contributors, driving progress and encouraging community participation.

#### Describe the target persona or user(s) for the project?

- **Enterprise IT Managers/Architects:**
  - Manage multiple Kubernetes clusters across different data centers or hybrid cloud environments efficiently.
  - Simplify cluster management to ensure high availability and optimize resource utilization.
  - Design scalable architectures to accommodate business growth and increasing workloads.
  - Ensure seamless integration and coordination of applications across multiple clusters.  
- **Cloud Service Providers:**
  - Enhance multi-cluster management capabilities for Kubernetes-as-a-Service offerings.
  - Differentiate services by providing advanced cross-cluster features such as application deployment, cluster failover, and resource sharing.
- **Application Developers and DevOps Engineers:**
  - Simplify the process of deploying applications across multiple Kubernetes clusters.
  - Integrate Karmada into CI/CD pipelines for cross-cluster application deployment and management.
  - Ensure consistent application deployment, perform rolling updates, and manage application lifecycles across multiple clusters.

#### Explain the primary use case for the project. What additional use cases are supported by the project?

Primary use case:
- Turnkey automation for multi-cluster application management in multi-cloud and hybrid cloud scenarios  
- Multi-policy multi-cluster scheduling.  

Additional use cases:  
- Application/Cluster Failover  
- Federated Resource Quota  
- Resource status collection and aggregation  
- Global search for resources and events  
- Multi-cluster service discovery

#### Explain which use cases have been identified as unsupported by the project.

Karmada is designed to run in Kubernetes environments; non-Kubernetes environments are not supported.

#### Describe the intended types of organizations who would benefit from adopting this project. (i.e. financial services, any software manufacturer, organizations providing platform engineering services)?

The Karmada project is particularly beneficial for the following types of organizations:
- Cloud Service Providers: Those managing multi-cloud or hybrid-cloud environments, aiming to offer unified cluster management capabilities to their customers.  
- Financial Services Organizations: Requiring high availability, disaster recovery, and compliance with regional data regulations across distributed clusters.  
- Large Enterprises with Distributed Infrastructure: operating applications across multiple regions or cloud providers.  
- Platform Engineering Teams: Responsible for building internal developer platforms that abstract underlying infrastructure complexity while providing consistent deployment patterns.  
- Software-as-a-Service (SaaS) Providers: Needing to deploy and scale applications across multiple clusters to meet global customer demand and ensure fault tolerance.  

In summary, any organization operating Kubernetes clusters across multiple clouds, regions, or on-premises environments can benefit from Karmada's ability to centralize management, ensure high availability, and optimize resource utilization.

#### Please describe any completed end user research and link to any reports.

- Survey for installation and operation: https://docs.google.com/document/d/1lOXHfpLiA0sg5dJr7ye9E0Qemd_YvOS-TACfsA3xQig/edit?usp=sharing you can get he mail from https://groups.google.com/g/karmada/c/Xe489XICWqs/m/7i-sPn8cAAAJ
- [[survey] Need your feedback on Karmada Concept <Host Cluster>](https://github.com/karmada-io/community/issues/137): The Kubernetes SIG-Multicluster is proposing a standardized definition for the central cluster (currently termed "Host Cluster" in Karmada) to unify terminology across multi-cluster management projects like Karmada, OCM, clusternet, kubefleet, MCO, KubeAdmiral and so on. This initiative aims to improve cross-project interoperability and reduce ecosystem fragmentation. To evaluate the impact of adopting this standard and gather user feedback, ​​we are launching a community survey​​. 

### Usability

#### How should the target personas interact with your project?  
    
Users determine the Karmada components to install based on their usage scenarios and customize relevant configurations, such as feature gates. During daily operation and maintenance, users interact with the control plane using `karmadactl` or `kubectl`.

#### Describe the user experience (UX) and user interface (UI) of the project.

- **CLI Tools: Karmadactl**
  - Reuses Kubernetes command patterns.
  - Manages contexts for both the Karmada control plane and member clusters.
- **API Server & Custom Resources**
  - Kubernetes API Compatibility: Zero-change upgrade from single-cluster to multi-cluster; seamless integration with existing Kubernetes toolchains.
  - Custom Resource Definitions (CRDs): Extend Kubernetes with multi-cluster resources.
- **UI: Karmada Dashboard**
  - Cluster Management: Provides cluster access and an overview of cluster status.
  - Resource Management: Manages the configuration of business resources.
  - Policy Management: Manages Karmada policies.
- **Documentation and Community Support**
  - Official documentation: Guides for installation, configuration, and use cases.
  - Tutorials: Step-by-step examples for common scenarios.
  - Community channels: Slack, GitHub issues, and forums for support.
  - Security: Security recommendations.
- **Metrics:**
  - Provides a rich set of metrics to characterize the running status of Karmada.

#### Describe how this project integrates with other projects in a production environment.

Karmada uses Kubernetes-native APIs, enabling seamless integration with existing Kubernetes toolchains in production environments. Karmada API server directly uses the implementation of kube-apiserver from Kubernetes, which is the reason why Karmada is naturally compatible with Kubernetes API. That makes integration with the Kubernetes ecosystem very simple for Karmada, such as allowing users to use kubectl to operate Karmada, integrating with ArgoCD, integrating with Flux and so on.

### Design

#### Explain the design principles and best practices the project is following.

- Compatibility with Kubernetes: Karmada is designed to be compatible with Kubernetes native APIs and Custom Resource Definitions (CRDs). This allows existing Kubernetes-based systems to be migrated to a multi-cluster environment with little or no code refactoring, reducing the learning cost and migration difficulty for users.
- Scalability and Flexibility: provides a rich variety of definable policies along with comprehensive default policies to address the diverse requirements of users.
- Security and Multi-Tenancy
  - Use Kubernetes namespace for tenant isolation with configured resources limits and RBAC;
  - Resource Isolation Between Member Clusters.
- Community over Product or Company: Sustaining and growing our community takes priority over shipping code or sponsors' organizational goals. Each contributor participates in the project as an individual.

#### Outline or link to the project’s architecture requirements? Describe how they differ for Proof of Concept, Development, Test and Production environments, as applicable.
    
Karmada architecture requirements: https://karmada.io/docs/core-concepts/architecture

- Proof of Concept/Development/Test: user can run the [quick installation script](https://github.com/karmada-io/karmada/blob/master/hack/local-up-karmada.sh) to install Karmada in local environment for quick iteration.
- Production: user can deploy Karmada on k8s cluster using officially released helm chart. Alternatively, they can use the command "karmadactl init", and they can also use the karmada-operator to manage the lifecycle of Karmada.

#### Define any specific service dependencies the project relies on in the cluster.  
    
- Prometheus: collects metrics from configured targets to monitor Karmada control plane.

#### Describe how the project implements Identity and Access Management.

Karmada implements Identity and Access Management (IAM) through Kubernetes-native mechanisms. 

- **Kubernetes API Server Authentication:**
  - Uses standard Kubernetes authentication plugins (e.g., bearer tokens, X509 certificates, OIDC, or webhook).
  - Authenticates users and service accounts accessing the Karmada API server.
- **Cross-Cluster Authentication:**
  - Supports impersonation to act on behalf of users or service accounts across clusters.

#### Describe how the project has addressed sovereignty.

Karmada is designed to be cloud and platform agnostic, running on any Kubernetes cluster (on-premises or any cloud provider). It avoids vendor lock-in by using open standards and Kubernetes-native APIs. All data and model artifacts remain within the adopter's control, and integrations with external storage or identity providers are optional and configurable by the user.

#### Describe any compliance requirements addressed by the project.

- Open Source License Compliance: Karmada project is released under the Apache License 2.0. All source code contributions to the project must comply with this license.

#### Describe the project’s High Availability requirements.

- Controller HA: Karmada controller can be deployed with multiple replicas across different nodes and availability zones. It uses leader election to ensure only one instance is active at a time, while others are on standby. This ensures that if one instance fails, another can take over without downtime.
- Karmada Instance HA: a highly available managed Karmada control plane can be deployed across multiple management clusters that can span various data centers, thus fulfilling disaster recovery requirements.
![](images/architecture-overview.png)
- Etcd HA: Karmada supports the use of external etcd, enabling the creation of a DR-compliant etcd cluster.

#### Describe the project’s resource requirements, including CPU, Network and Memory.  
    
Resource requirements depend on the number of clusters managed by Karmada, as well as the scale of a single cluster. For large-scale clusters, resource requests and limits should be adjusted accordingly.  
    
#### Describe the project’s storage requirements, including its use of ephemeral and/or persistent storage.

Karmada control plane requires persistent storage for etcd, which stores all cluster configuration data, resource definitions, and state information. 

#### Please outline the project’s API Design:  
##### Describe the project’s API topology and conventions  
      
- Karmada exposes a Kubernetes-native declarative API using Custom Resource and Kubernetes API Aggregation Layer, mainly include:  
  - PropagationPolicy/ClusterPropagationPolicy (Custom Resource): represents the policy that propagates a group of resources to one or more clusters.  
  - Cluster (Kubernetes API Aggregation Layer): represents the desired state and status of a member cluster.  
- The API follows Kubernetes conventions for resource specification, status, and metadata, supporting standard CRUD operations via kubectl and the Kubernetes API server.  
- Karmada supports versioned APIs (e.g., work.karmada.io/v1alpha2) and uses OpenAPI schema validation for resource definitions.   
- For more details, see the [Karmada API Reference](https://karmada.io/docs/category/karmada-api).

##### Describe the project defaults

Karmada provides secure, production-ready defaults out-of-the-box to simplify multi-cluster Kubernetes management while maintaining security best practices. For example:

- **Security Considerations**  
  - To avoid the use of insecure algorithms such as 3DES during the communication process, the TLS configuration is set to  `--tls-min-version=VersionTLS13` during the installation of Karmada-related components.  
- **Policy-related**  
  - The default replicaSchedulingType of the PropagationPolicy is "Duplicated", which will schedule the workload to all member clusters.   
- **Installation-related**  
  * Karmada components and related resources are deployed to the karmada-system namespace by default.
- **others**
  - Karmada uses karmada-scheduler to schedule workloads across multi-clusters by default.

##### Outline any additional configurations from default to make reasonable use of the project

- In large-scale environments, administrators can adjust `cluster-api-qps` and `cluster-api-burst` for karmada-controller-manager, karmada-agent and karmada-metrics-adapter.
- In large-scale environments, administrators can adjust upward the rate limit parameters `kube-api-qps` and `kube-api-burst` for each component to access karmada-apiserver, and adjust `cluster-api-qps` and `cluster-api-burst` for scheduler-estimator to access member cluster apiserver.
- Configure resource requests and limits for Karmada components based on cluster scale - defaults may be insufficient for production workloads.
- See the [Karmada configuration](https://karmada.io/docs/administrator/configuration/configure-controllers) for more configuration recommendations.

##### Describe any new or changed API types and calls - including to cloud providers - that will result from this project being enabled and used

- The `PropagationPolicy/ClusterPropagationPolicy` CRD represents the policy that propagates a group of resources to one or more clusters.  
- The `Cluster` API represents the desired state and status of a member cluster.  
- The `ResourceBinding/ClusterResourceBinding` CRD represents a binding of a Kubernetes resource.
- The `Work` CRD defines a list of resources to be deployed on the member cluster.  
- The `OverridePolicy/ClusterOverridePolicy` CRD represents the cluster-wide policy that overrides a group of resources to one or more clusters.  
- The `FederatedResourceQuota` CRD sets aggregate quota restrictions enforced per namespace across all clusters.  
- The `WorkloadRebalancer` CRD represents the desired behavior and status of a job which can enforce a resource rebalance.  
- The `FederatedHPA/CronFederatedHPA` CRD can scale any resource implementing the scale subresource  
- The `ResourceInterpreterCustomization/ResourceInterpreterWebhookConfiguration` CRD takes the responsibility to tell Karmada the details of the resource object, especially for custom resources.
- The `MultiClusterIngress/MultiClusterService` CRD is mainly to provide the application with the service discovery capability across Kubernetes clusters.  
- The `Remedy` CRD represents the cluster-level management strategies based on cluster conditions.   
- The `ResourceRegistry` CRD represents the configuration of the cache scope, mainly describes which resources in which clusters should be cached.  
- Enabling Karmada does not change existing Kubernetes APIs, but adds these new CRDs and related endpoints.

##### Describe compatibility of any new or changed APIs with API servers, including the Kubernetes API server

- All Karmada CRDs are implemented using Kubernetes Custom Resource Definitions and are fully compatible with the Kubernetes API server.  
- These CRDs follow Kubernetes API conventions for resource creation, update, deletion, and status reporting, and can be managed using standard Kubernetes tools (kubectl, client libraries, etc.).  
- Karmada CRDs are versioned and validated using OpenAPI schemas, ensuring compatibility with Kubernetes admission controllers and API server validation mechanisms.  
- No changes are made to existing Kubernetes APIs; Karmada only extends the API surface by adding new resource types.

##### Describe versioning of any new or changed APIs, including how breaking changes are handled

- Karmada CRDs use Kubernetes API versioning conventions, such as v1alpha1 and v1alpha2 in their API group.  
- New features and changes are introduced in alpha or beta versions before being promoted to stable (v1).  
- Conversion webhooks are provided to support seamless migration between CRD versions when breaking changes are introduced.  
- Deprecated fields and versions are announced in release notes and documentation, and are supported for a deprecation period before removal.  
- Backward compatibility is maintained for stable APIs, and breaking changes are only introduced in new major or beta versions, following Kubernetes best practices.

##### Describe the project’s release processes, including major, minor and patch releases.  
    
- Karmada follows a structured release process for major, minor, and patch releases.  
- Each release includes versioning, release notes, and changelogs to communicate new features, bug fixes, and deprecations.  
- Release candidates are published for community testing before final releases.  
- The process includes automated CI/CD checks, validation, and artifact publishing to container registries and GitHub artifacts.  
- Deprecated features and APIs are maintained for a deprecation period before removal.  
- See [Karmada Releases](https://karmada.io/docs/releases) for detailed release processes.

### Installation

#### Describe how the project is installed and initialized, e.g. a minimal install with a few lines of code or does it require more complex integration and configuration?  
  	  
- For getting started, Karmada can be installed via a simple command `hack/local-up-karmada.sh`.  
- For production uses, please refer to the [installation guide](https://karmada.io/docs/installation/).  
  - For example, Karmada can be installed via Helm.

#### How does an adopter test and validate the installation?

- Each of the installation guides mentioned above includes validation steps.  
- The installation can be verified by the running status of the karmada component.
```bash
$ kubectl get deployments -n karmada-system
NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
karmada-aggregated-apiserver   1/1     1            1           102s
karmada-apiserver              1/1     1            1           2m34s
karmada-controller-manager     1/1     1            1           116s
karmada-scheduler              1/1     1            1           119s
karmada-webhook                1/1     1            1           113s
kube-controller-manager        1/1     1            1           2m3s

$ kubectl get statefulsets -n karmada-system
NAME   READY   AGE
etcd   1/1     28m
```

### Security

#### Please provide a link to the project’s cloud native [security self assessment](https://tag-security.cncf.io/community/assessments/).

[Karmada security self assessment](https://github.com/karmada-io/community/blob/main/security-team/assessments/self-assessment.md)

#### Please review the [Cloud Native Security Tenets](https://github.com/cncf/tag-security/blob/main/security-whitepaper/secure-defaults-cloud-native-8.md) from TAG Security.  
##### How are you satisfying the tenets of cloud native security projects?  
      
- Karmada is built with security as a foundational concern. By leveraging Kubernetes-native constructs such as Custom Resource Definitions (CRDs), Role-Based Access Control (RBAC), and Network Policies, Karmada integrates seamlessly into secure Kubernetes environments.  
- Secure defaults are enabled by default, but users can override them if needed.  
- Insecure options require explicit configuration.

##### Describe how each of the cloud native principles apply to your project.

- Karmada uses containers as the fundamental unit of deployment.  
- Declarative Multi-Cluster Management: Karmada extends Kubernetes’ declarative model to multi-cluster scenarios. Users define resource templates (e.g., Deployments, Services) and placement policies (e.g., how resources are distributed across clusters) in YAML files.  
- Karmada adopts a microservices architecture by separating concerns such as resource binding, scheduling and global search into distinct containerized components.  
- Role-Based Access Control (RBAC): Karmada extends Kubernetes RBAC to multi-cluster contexts, allowing fine-grained control over which users/teams can manage specific clusters or resources.  
- Secure Communication: Communication between the control plane and member clusters uses TLS, and credentials are managed via Kubernetes secrets.

##### How do you recommend users alter security defaults in order to "loosen" the security of the project? Please link to any documentation the project has written concerning these use cases.

- Karmada Security Considerations: https://karmada.io/docs/administrator/security/security-considerations
- By default, the gRPC connection between the karmada-scheduler/karmada-descheduler component and the karmada-scheduler-estimator component uses mutual authentication.   
  - The karmada-scheduler/karmada-descheduler component can disable the verification of the karmada-scheduler-estimator's certificate by setting `insecure-skip-estimator-verify` to true. For example:
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: karmada-scheduler
        namespace: karmada-system
      spec:
        template:
          spec:
            containers:
            - name: karmada-scheduler
              command:
              - /bin/karmada-scheduler
              - --enable-scheduler-estimator=true
              - --insecure-skip-estimator-verify=true
      ```

  - The karmada-scheduler-estimator component can disable the verification of the gRPC client's certificate by setting insecure-skip-grpc-client-verify to true. For example:
      ```yaml
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: karmada-scheduler-estimator-member1
        namespace: karmada-system
      spec:
        template:
          spec:
            containers:
            - name: karmada-scheduler-estimator
              command:
              - /bin/karmada-scheduler-estimator
              - --insecure-skip-grpc-client-verify=true
      ```        

- By default, Karmada components set the TLS configuration option `--tls-min-version` for client-to-server communication to `VersionTLS13` to avoid the use of insecure algorithms such as 3DES during the communication process. This is documented on [Karmada website](https://karmada.io/docs/administrator/security/security-considerations#tls-configuration). To loosen this security setting, users can configure the TLS configuration option `--tls-min-version` to other values (e.g., `VersionTLS12, VersionTLS11`). For example,
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: karmada-aggregated-apiserver
    namespace: karmada-system
  spec:
    template:
      spec:
        containers:
        - name: karmada-aggregated-apiserver
          command:
          - /bin/karmada-aggregated-apiserver
          - --kubeconfig=/etc/karmada/config/karmada.config
          - --tls-min-version=VersionTLS12
  ```

- By default, Karmada’s security context restricts containers from running in privileged mode and denies a process from obtaining more privileges than its parent process. This can be achieved by modifying securityContext of Karmada components. For example:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: karmada-controller-manager
    namespace: karmada-system
  spec:
    template:
      spec:
        containers:
        - name: karmada-controller-manager
          securityContext:
            allowPrivilegeEscalation: true
            privileged: true
  ```

#### Security Hygiene  
##### Please describe the frameworks, practices and procedures the project uses to maintain the basic health and security of the project.

- Robust CI/CD Pipeline: Unit tests and end-to-end tests are integrated into the CI/CD pipeline to catch bugs early in the pull request (PR) stage.  
- Vulnerability Scanning: Weekly scheduled image scanning using trivy.  
- Static Code Analysis: Code linting and static analysis tools are employed to enforce code quality and detect potential issues before they reach production.  
- Dependency Management: The project uses lock files (e.g., go.mod) for reproducibility. Automated generation of Software Bill of Materials (SBOMs) is now integrated into the CI/CD pipeline for all container images and components, supporting supply chain security and transparency.
- Secure Defaults: Default Kubernetes manifests and Helm charts are configured with security best practices, such as running containers as non-privileged.  
- Code Review Process: All code changes require peer review and explicit approval before being merged into the main branch.  
- Transparent Governance: The project practices open governance with regular public meetings and community channels for raising and discussing security or health concerns.  
- Structured Releases: Releases are managed with clear versioning and changelogs, ensuring traceability and transparency for all changes.

##### Describe how the project has evaluated which features will be a security risk to users if they are not maintained by the project?

Karmada has conducted a comprehensive evaluation of security-critical features that require ongoing maintenance to ensure user safety:

**Core Security Features Requiring Continuous Maintenance:**
- **Authentication and Authorization**: RBAC policies, certificate management, and cross-cluster authentication mechanisms that, if unmaintained, could lead to privilege escalation or unauthorized access
- **TLS/Certificate Management**: Certificate rotation, validation, and secure communication channels between control plane and member clusters
- **Admission Controllers and Webhooks**: Security validation logic that prevents malicious or misconfigured resources from being deployed
- **Network Security**: Multi-cluster service discovery and network policies that ensure secure inter-cluster communication
- **Resource Isolation**: Tenant isolation mechanisms and resource quota enforcement across clusters

**Risk Assessment Process:**
- **Dependency Analysis**: Continuous monitoring of third-party dependencies for security updates
- **Community Feedback**: Security issues reported through the security team and vulnerability disclosure process help identify maintenance-critical features
- **Compliance Requirements**: Features required for regulatory compliance (e.g., audit logging, access controls) are prioritized for ongoing maintenance

**Mitigation Strategies:**
- **Core Maintainer Commitment**: Critical security features are assigned to core maintainers with long-term project commitment
- **Documentation and Knowledge Transfer**: Comprehensive documentation ensures security features can be maintained by multiple contributors
- **Deprecation Policy**: Clear communication and migration paths when security features need to be deprecated or replaced

#### Cloud Native Threat Modeling  
##### Explain the least minimal privileges required by the project and reasons for additional privileges.

[Karmada Component Permissions docs](https://karmada.io/docs/administrator/security/component-permission) provides a detailed explanation of the resources each Karmada component needs to access and the reasons for these accesses.

##### Describe how the project is handling certificate rotation and mitigates any issues with certificates.

- When installing Karmada, users can either customize certificates or use the certificates automatically generated by Karmada.   
- When generating certificates through Karmada, users can configure the validity period of the certificates.  
- These certificates are stored in Kubernetes ConfigMaps or Secrets, and are manually provisioned and rotated by cluster administrators.

##### Describe how the project is following and implementing [secure software supply chain best practices](https://project.linuxfoundation.org/hubfs/CNCF_SSCP_v1.pdf)  
      
- Automated Testing and CI/CD Integration: Karmada integrates unit tests and end-to-end tests within its CI/CD pipeline. This ensures that any changes are validated early, reducing the risk of introducing vulnerabilities.  
- Vulnerability Scanning: The project employs vulnerability scanning tools such as trivy, dependabot and gosec to identify and address known security issues in dependencies.  
- Use of Lock Files and SBOM Generation: Karmada utilizes dependency lock files (e.g., go.mod) to ensure reproducibility. Automated generation of Software Bill of Materials (SBOMs) is now integrated into the CI/CD pipeline for all container images and components. This enhances transparency of software components and supports supply chain security.  
- DCO Check: Committers are required to sign and comply with the Developer Certificate of Origin (DCO) to affirm the legitimacy and authorship of their contributions.  
- Branch Protection: The project enforces branch protection rules to prevent unauthorized changes, enforce status checks, require pull request reviews, and control who can push to protected branches.  
- License compliance: Automated license compliance checks are now integrated into the CI/CD pipeline. All dependencies are scanned and validated for license compatibility as part of every pull request and release build, ensuring transparency and legal compliance.  
- Peer Review: Commits and builds are validated through peer-reviewed pull request workflows, requiring approval before merge.

## Day 1 - Installation and Deployment Phase

### Project Installation and Configuration

#### Describe what project installation and configuration look like.

- Please refer to the installation section in Day 0 section.  
- During the installation process, Karmada provides multiple configurable options to meet users' diverse usage scenarios. Taking `karmada init` as an example, its configuration file is as follows:
    ```yaml
    apiVersion: config.karmada.io/v1alpha1
    kind: KarmadaInitConfig
    spec:
      hostCluster:
        kubeconfig: "${KUBECONFIG_PATH}/${HOST_CLUSTER_NAME}.config"
      components:
        karmadaControllerManager:
          repository: "${REGISTRY}/karmada-controller-manager"
          tag: "${VERSION}"
        karmadaScheduler:
          repository: "${REGISTRY}/karmada-scheduler"
          tag: "${VERSION}"
        karmadaWebhook:
          repository: "${REGISTRY}/karmada-webhook"
          tag: "${VERSION}"
        karmadaAggregatedAPIServer:
          repository: "${REGISTRY}/karmada-aggregated-apiserver"
          tag: "${VERSION}"
      karmadaDataPath: "${HOME}/karmada"
      karmadaPkiPath: "${HOME}/karmada/pki"
      karmadaCrds: "./crds.tar.gz"
    ```

### Project Enablement and Rollback

#### How can this project be enabled or disabled in a live cluster? Please describe any downtime required of the control plane or nodes.

To enable or disable Karmada in a live cluster, follow these steps:

- Enabling Karmada in a live Cluster  
  - Install Karmada Components  
  - Register Member Clusters: Use `karmadactl join(push mode)` or `karmadactl register(pull mode)` to register existing clusters as members of Karmada.

- Disabling Karmada in a live Cluster  
  - Unregister member cluster: Remove member clusters from Karmada using `karmadactl unjoin(push mode)` or `karmadactl unregister(pull mode)`.  
  - Delete Karmada Control Plane: Use kubectl to delete the Karmada namespace and all associated resources.

#### Describe how enabling the project changes any default behavior of the cluster or running workloads.

The default behavior and existing workloads will not be impacted. Karmada operates as an overlay control plane that manages resources across multiple clusters without modifying the underlying Kubernetes clusters' behavior.

**Specific examples of non-impact:**

- **Existing Workloads**: Applications already running in member clusters continue to operate normally. Karmada does not interfere with existing pods, services, or other resources unless explicitly managed through Karmada policies.

#### Describe how the project tests enablement and disablement.

The relevant tests are integrated into the CI/CD Pipeline. 

- enablement by helm: https://github.com/karmada-io/karmada/blob/release-1.14/.github/workflows/installation-chart.yaml
- enablement by karmadactl init: https://github.com/karmada-io/karmada/blob/release-1.14/.github/workflows/installation-cli.yaml
- enablement by operator: https://github.com/karmada-io/karmada/blob/release-1.14/.github/workflows/installation-operator.yaml
- disablement test is integrated into e2e test: https://github.com/karmada-io/karmada/blob/release-1.14/.github/workflows/ci.yml

These tests are triggered both upon submitting and merging PRs, ensuring that the system behaves as expected when enabling or disabling Karmada.

#### How does the project clean up any resources created, including CRDs?

- **Member Cluster Unregistration**  
  - Use the `karmadactl unjoin/unregister` command to remove the member cluster from Karmada.   
  - This command deletes both the member cluster's resource records in the Karmada control plane and the Karmada-specific agent components (e.g., `karmada-agent`) running in the member cluster.
- **Karmada Control Plane Removal** (Based on Installation Method)
  - Karmada-Operator: Delete the Karmada Custom Resource (CR). The operator automatically cleans up all related resources (e.g., Deployments, Services).  
  - karmadactl init: Use karmadactl deinit to remove resources created by the initialization process.  
  - Helm: Execute helm uninstall to purge all Helm-managed resources.

### Rollout, Upgrade and Rollback Planning

#### How does the project intend to provide and maintain compatibility with infrastructure and orchestration management tools like Kubernetes and with what frequency?

Karmada has a defined compatibility matrix: [https://github.com/karmada-io/karmada#kubernetes-compatibility](https://github.com/karmada-io/karmada#kubernetes-compatibility)

In addition, Karmada checks the compatibility between the maintained versions and Kubernetes weekly.

#### Describe how the project handles rollback procedures.

Karmada is designed to roll forward only. Feature development, bug fixes, and testing are aligned to this objective.

#### How can a rollout or rollback fail? Describe any impact to already running workloads.

A rollout or rollback can fail due to various technical, operational, or environmental factors. For example:

- Component options are deprecated.  
- Incompatible with Kubernetes.  
  
Traffic will not be switched to the new unless the newer revision is ready to accept traffic. Hence, the already running workloads will not be affected in any case if rollout fails.

#### Describe any specific metrics that should inform a rollback.

The ready condition of Karmada CRD remains false if the Karmada is installed by karmada-operator.

#### Explain how upgrades and rollbacks were tested and how the upgrade->downgrade->upgrade path was tested.

Rollbacks and upgrade->downgrade->upgrade testing is not covered.  

For upgrade tests:
- If installed via Helm, use helm upgrade to perform the upgrade test.  
- If installed via karmada-operator, update the Karmada CR (Custom Resource) to conduct the upgrade test. Refer to https://github.com/karmada-io/karmada/blob/master/operator/README.md#upgrade-a-karmada-instance.

#### Explain how the project informs users of deprecations and removals of features and APIs.

- All API changes are backward compatible which are announced in release notes and in the official website.  
- release notes: [https://github.com/karmada-io/karmada/tree/master/docs/CHANGELOG](https://github.com/karmada-io/karmada/tree/master/docs/CHANGELOG)  
- upgrade instruction: [https://karmada.io/docs/administrator/upgrading/](https://karmada.io/docs/administrator/upgrading/)

#### Explain how the project permits utilization of alpha and beta capabilities as part of a rollout.

Karmada’s feature lifecycle uses the Kubernetes model. Features are initially implemented using alpha version strings in the API and are feature gated. Features then graduate to beta and the feature gate is removed as part of the release lifecycle.

In addition, we provide conversion webhooks when CRD versions are updated. For example, webhook `resourcebindings.work.karmada.io` is used to update binding to v1alpha2.
