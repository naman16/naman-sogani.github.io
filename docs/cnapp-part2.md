## CNAPP Implementation: Day 2 Focus Areas and Beyond

In the first part of the Cloud-Native Application Protection Platform blog ([here](https://naman16.github.io/cloud-security/)), I laid out my thoughts on tool selection criteria and day 1 focus areas. In this blog, I want to double-click into the day 2 focus areas that include pre-deployment security, implementing CNAPP capabilities beyond Cloud Security Posture Management (CSPM), designing issues management workflows, and integrating CNAPP with enterprise tools. Below is the high-level structure of how this document is organized:

1. [Pre-Deployment Security Capabilities](#pre-deployment-security-capabilities)  
2. [Capabilities Beyond CSPM](#capabilities-beyond-cspm)  
3. [Issues Management & Technology Integrations](#heading=h.kysy7xy2dld0)

# Pre-Deployment Security Capabilities {#pre-deployment-security-capabilities}

“Pre-deployment” security in the context of CNAPP refers to all the different capabilities that are implemented within CI / CD pipelines and/or as part of the development lifecycle such that they can identify and/or block insecure resources from reaching the cloud environments. A lot of times, I see this definition nuanced to be only for “production” cloud environments, however, I think it’s more effective to think about it as “before reaching cloud environments”. This allows you to shift-left and roll-out capabilities in a standardized manner v/s variations of processes for different environments. You can then utilize policy thresholds to enforce stricter requirements in production v/s development environments. Below are the primary pre-deployment capabilities:

* **Infrastructure-as-Code (IaC) & Secret Scanning:** This includes scanning your resource deployment files (Terraform, CloudFormation, ARM, Ansible, Kubernetes manifest files, helm charts, Dockerfiles, etc.) to detect misconfigurations and exposed secrets. Below are key considerations when rolling out IaC / secrets scanning:  
  - *Scaling Across CI / CD Pipelines:* In a decentralized model where teams have autonomy over their development processes, the proliferation of diverse pipelines creates a complex landscape for security integration. This is because:  
    * Teams use a variety of CI/CD tools (e.g., Jenkins, GitHub Actions, Azure DevOps), each with its unique configuration and execution environments. Furthermore, the sheer number of pipelines itself, ranging from hundreds or even thousands in large enterprises, exponentially increases the number of touchpoints requiring security integration. This makes it difficult to enforce consistent scanning practices across the board, as each pipeline may require a tailored approach to incorporate IaC and secrets scanning effectively.  
    * Moreover, the decentralized nature of pipeline management means that security teams often lack visibility into all existing pipelines, making it challenging to ensure comprehensive coverage. The variety in pipeline configurations also complicates the task of standardizing scanning tools and processes, as what works for one pipeline might not be compatible with another.

    To address these challenges, consider the following:

    * *Standardized CI / CD Toolchain*: Collaborate with your peers in the engineering teams and build the business case for standardizing on a subset of CI / CD technologies and secure executive buy-ins. Focus on non-security related advantages to drive the message home. For example, “*Implementing a unified set of tools across teams, improves collaboration and knowledge sharing, as all members work with the same processes and technologies. This standardization also leads to faster onboarding of new team members and easier skill transfer between projects, as there's only one set of tools to learn. Furthermore, a standardized toolchain reduces complexity and operational friction, simplifying maintenance and support while potentially leading to cost savings through consolidated licensing and training.*”  
    * *Security Integration Toolkit:*  Create and maintain a central repository of integration scripts and configurations for the approved CI/CD pipeline technologies, making integrating security scanning consistently easier. Additionally, include appropriate guidelines and documentation to enable easy implementation for engineering organizations. Ensure that the repository is well maintained and regularly updated to support new CI / CD pipeline technologies, incorporate vendor updates, etc.  
  - *Preventing Bypass of Scanning Workflows:* Given that lot of organizations have a decentralized operating model when it comes to cloud, the different application / DevOps teams likely have admin privileges over their respective pipelines, which makes scanning enforcement a challenge. Technically, they have the permissions needed to disable / bypass any scanning steps. As such, when enabling and rolling out scanning across the organization, efforts should be put into designing processes / solutions where bypasses are minimized and / or only possible after security approvals. Below are some considerations for this:  
    * *Lockdown Important Touchpoints:*   
      * Extract critical actions like deployment of cloud resources or pushing images into registries into some sort of reusable pipeline / workflow that is locked down and can only be modified by a small group of people. This ensures that there is control over the important touchpoints needed for DevOps processes.   
      * Setup access controls in such a way that deployments / image push is only possible if actions originate from this tightly controlled workflow.   
      * Embed security scanning (and any other requirements) as part of these reusable workflows to get reasonable assurance that appropriate security hooks exist and teams cannot bypass security scans.  
    *  *Leverage Native CI / CD Pipeline Capabilities:*  
      * Modern CI/CD platforms (e.g., Harness) offer policy management capabilities that allow organizations to define and enforce standardized practices across how their pipelines are created, managed, and executed. Leverage this policy-as-code feature to enforce security scanning (and any other requirements) as “required steps” to be embedded to create and execute pipelines. This provides reasonable assurance that pipelines are created with the necessary security guardrails.      
  - *Setting Up Different Policies for Different Environments:* Blanket security requirements (e.g., all critical, high, and medium issues must be remediated) can cause frictions when enforced in lower environments as this inhibits developer productivity and increases the remediation burden for them. As such, it is advisable to have policies that have environmental context baked into them. For example, I have seen successful adoption of policies along the lines of:  
    * Dev & Test Environments —\> No critical issues  
    * Stg / Pre-Prod Environments —\> No critical and high issues  
    * Prod Environments —\> No critical, high, and medium issues

    There’s room for more granularity here depending on the type of workloads, business criticality, data sensitivity, regulatory requirements, etc. Another example I have seen for organizations running regulated workloads:

    * Non-Regulated Workloads:  
      * Dev & Test Environments —\> No critical issues  
      * Stg / Pre-Prod Environments —\> No critical and high issues  
      * Prod Environments —\> No critical, high, and medium issues  
    * Regulated Workloads:  
      * Dev & Test Environments —\> No critical and high issues  
      * Stg / Pre-Prod Environments —\> No critical, high, and medium issues  
      * Prod Environments —\> No critical, high, and medium issues  
* **Container Image Scanning:** This includes scanning your container images during build to identify misconfigurations, vulnerabilities, and exposed secrets. The key considerations for image scanning are similar to the ones listed above under IaC & secrets scanning. 

# Capabilities Beyond CSPM {#capabilities-beyond-cspm}

As a recap from the previous blog, I extended the definition of CSPM to include core CSPM and vulnerability scanning because they are relatively easy to enable, provide quick value realization, and the path to operationalization is similar. In this section, I want to focus on the additional CNAPP areas that build on top of the lessons learned and knowledge gathered (high-risk areas, cloud environment setup, landing zone design, naming convention, tagging standards) from rolling out CSPM. Below are some of the core CNAPP capabilities beyond CSPM:

* **Registry Scanning:** This includes scanning your container registries to detect vulnerabilities and malware on images. This enables you to have visibility into images that:  
  - have been pushed to the registries outside of the standard CI / CD pipelines  
  - have been running in the environment for a long period and have become vulnerable after the initial scan during the build phase. 

  If your organization uses cloud-native registries (e.g., ECR, ACR), then generally the CNAPP tools are able to scan them without additional configurations (i.e. automatically enabled when doing initial setup). However, if you are using a 3rd party registry (e.g., JFRog Artifactory), then additional configuration may be required to scan them. Below are key considerations for registry scanning:

  - *Managing Volume of Issues:* The volume of issues that are identified can be very high as registries can get wild-wild west due to the following reasons:  
    * teams may be pushing more images / packages but only using a small fraction of them for their workloads  
    * there is a lack of well-defined registry structure because of which, tracking ownership and accountability is challenging  
    * registry access is widely open allowing anyone to push to any location within the registry

    As such, efforts should be invested in correlating the issues with actual running workloads and prioritizing them for remediation v/s requiring remediation for all issues.

- *Assigning Remediation Ownership*: Depending on the operating model, the approach to assigning ownership for remediation can be very different because of the layered nature of containers.   
  * If your organization’s setup is such that you have a central team (e.g., cloud engineering) responsible for maintaining golden base images (base layer) that the rest of the application / DevOps teams then consume and add their layers on, then the issue needs to be traced to the vulnerable layer and ownership for remediation needs to be assigned appropriately.  
    * If the issue is on the base layer, then the level of effort for remediation across the entire footprint is much higher because:  
      1. the central team needs to update / create the golden base image with the fix  
         2. the consumers (i.e. application and DevOps teams) need to redeploy their applications / workloads on this new base layer

         Doing this regularly requires organizations to have mature DevOps processes where teams understand the importance and need to constantly rehydrate their images. Furthermore, there are good testing and dependency management programs in place to make sure that updating images does not break applications.

      * If the issue is on the application layer, then the responsibility lies with the respective application / DevOps teams to remediate and redeploy their application / workload images. The operations around this are relatively easier because the burden lies exclusively with the application / DevOps teams and there are no dependencies on an enterprise team.   
    * If your organization’s setup is one where the concept of golden images does not exist and the application / DevOps teams own the entire lifecycle of the container images, then the ownership assignment and operations are similar to the above point about managing issues that exist on the application layers.  
* **Cloud Infrastructure Entitlement Management (CIEM), Data Security Posture Management (DSPM) & Attack Surface Management:** Although these are all broad and disparate categories, I have grouped them under one section because the approach to operationalizing each of these capabilities is similar. Once you are able to operationalize one capability, the strategy and process for the rest of the areas will be comparable. Below are the key considerations for these capabilities:  
  - 
