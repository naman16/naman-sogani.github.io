# Implementing CNAPP: Day 2 Focus Areas

In the first part of the Cloud-Native Application Protection Platform (CNAPP) blog ([here](https://naman16.github.io/cloud-security/)), I laid out my thoughts on tool selection criteria and day 1 focus areas. In this blog, I want to double-click into the day 2 focus areas that include pre-deployment security, implementing CNAPP capabilities beyond Cloud Security Posture Management (CSPM), designing issues management workflows, and integrating CNAPP with enterprise tools. Below is the high-level structure of how this document is organized:

1. [Pre-Deployment Security Capabilities](#pre-deployment-security-capabilities)  
2. [Capabilities Beyond CSPM](#heading=h.ncwxq8cplnp5)  
3. [Operational Workflows and Technology Integrations](#heading=h.kysy7xy2dld0)

## Pre-Deployment Security Capabilities

In the context of CNAPP, "pre-deployment" security encompasses a range of capabilities integrated into CI/CD pipelines and the overall development lifecycle to identify and block insecure resources before they enter cloud environments. Often, I have noticed that this term is narrowly defined to apply only to production environments. However, in my experience, a broader interpretation of this term—viewing it as applicable to all stages before cloud deployment, i.e., “before reaching cloud environments”—is more effective. This allows you to shift left and roll out capabilities in a standardized manner rather than managing disparate processes across different environments. Additionally, you can then implement policy thresholds that enforce stricter security requirements in lower environments compared to higher environments to provide the necessary freedom needed for experimentation, development, and testing. Below are the primary pre-deployment capabilities:

1. [Infrastructure-as-Code (IaC) & Secrets Scanning](#infrastructure-as-code-iac--secret-scanning)  
2. [Container Image Scanning](#heading=h.mybvwy9onk3h)

### Infrastructure-as-Code (IaC) & Secret Scanning

This involves scanning resource deployment files—such as Terraform, CloudFormation, ARM templates, Ansible playbooks, Kubernetes manifests, Helm charts, and Dockerfiles—to identify misconfigurations and exposed secrets. Below are some key considerations for implementing IaC and secret scanning:

* **Scaling Across CI / CD Pipelines:** In a decentralized model where teams have autonomy over their development processes, the proliferation of diverse pipelines creates a complex landscape for security integration. This is because:
     * Teams often utilize various CI/CD tools—such as Jenkins, GitHub Actions, Azure DevOps, etc.—each with distinct configurations and execution environments. Additionally, in large enterprises, the sheer volume of pipelines, often numbering in the hundreds or thousands, significantly increases the number of points that require security integration. This diversity complicates the enforcement of consistent scanning practices, as each pipeline may necessitate a customized approach to effectively implement IaC and secret scanning.
     * Moreover, the decentralized nature of pipeline management means that security teams often lack visibility into all existing pipelines, making it challenging to ensure comprehensive coverage. The variety in pipeline configurations also complicates the task of standardizing scanning tools and processes, as what works for one pipeline might not be compatible with another.


To address CI / CD scalability challenges, consider the following:
* _Standardize CI / CD Toolchain:_ Collaborate with your peers in the engineering teams and build a business case for standardizing on a subset of CI / CD technologies and seek executive buy-ins. Focus on non-security benefits to gain traction and drive the message home. For example, "Implementing a unified set of tools across teams improves collaboration and knowledge sharing, as all members work with the same processes and technologies. This standardization also leads to faster onboarding of new team members and easier skill transfer between projects, as there's only one set of tools to learn. Furthermore, a standardized toolchain reduces complexity and operational friction, simplifying maintenance and support while potentially leading to cost savings through consolidated licensing and training."
* _Maintain Security Integration Toolkit:_ Develop and maintain a central repository of integration scripts and configurations for the approved CI/CD pipeline technologies, making it easier to implement security scanning consistently. Include clear guidelines and documentation to facilitate increased adoption by engineering teams. Ensure the repository is regularly updated to incorporate new CI/CD technologies or vendor updates.

* **Preventing Bypass of Scanning Workflows:** In many organizations that have a decentralized operating model for cloud, application, and DevOps teams often persist admin privileges over their respective pipelines. This can pose challenges for enforcing scanning, as these teams can disable or bypass scanning steps. Therefore, when implementing scanning across the organization, it is crucial to design processes and solutions that minimize or restrict bypasses, ideally requiring security approval for any exceptions. Below are key considerations for this:
     * _Lockdown Important Touchpoints:_
        * Isolate essential actions, such as deploying cloud resources or pushing images to registries, into a reusable pipeline or workflow that is secured and can only be modified by a limited group of individuals. This ensures control over the critical touchpoints in the DevOps processes.
        * Establish access controls so that deployments or image pushes can only occur through this tightly regulated workflow.
        * Integrate security scanning and other requirements directly into these reusable workflows to ensure that necessary security measures are in place, making it difficult for teams to bypass security scans.
     * _Leverage Native CI / CD Pipeline Capabilities:_
        * Modern CI/CD platforms (e.g., Harness) offer policy management capabilities that allow organizations to define and enforce standardized practices across how their pipelines are created, managed, and executed. Leverage this policy-as-code feature to enforce security scanning (and any other requirements) as “required steps” that need to be embedded for the successful creation and execution of pipelines. This provides reasonable assurance that pipelines are created with the necessary security guardrails.

* **Set Different Policies for Different Environments:** Blanket security requirements (e.g., all critical, high, and medium issues must be remediated) can cause friction when enforced in lower environments as this inhibits developer productivity and increases the remediation burden for them. As such, it is advisable to have policies that have environmental context baked into them. For example, I have seen successful adoption of policies along the lines of:
     * Dev & Test Environments —> No critical issues
     * Stg / Pre-Prod Environments —> No critical and high issues
     * Prod Environments —> No critical, high, and medium issues

There’s potential for further granularity here based on factors such as workload type, business criticality, data sensitivity, and regulatory requirements. Another example I have seen for organizations running regulated workloads:

     * _Non-Regulated Workloads:_
          * Dev & Test Environments —> No critical issues
          * Stg / Pre-Prod Environments —> No critical and high issues
          * Prod Environments —> No critical, high, and medium issues 
     * _Regulated Workloads:_
          * Dev & Test Environments —> No critical and high issues
          * Stg / Pre-Prod Environments —> No critical, high, and medium issues
          * Prod Environments —> No critical, high, and medium issues
