# Implementing CNAPP: Key Considerations for Success

As someone who has done multiple Cloud-Native Application Protection Platform (CNAPP)  implementations, I've summarized my key considerations to help people who are in the evaluation / early stages of CNAPP adoption. Below is the high-level structure of how this document is organized:  

1. [What is CNAPP?](\#what-is-cnapp?)  
2. [Tool Selection Criteria](\#tool-selection-criteria)  
3. [Day 1 Focus Areas](\#day-1-focus-areas)

# What is CNAPP?

According to Gartner, “*Cloud-native application protection platforms (CNAPPs) are a unified and tightly integrated set of security and compliance capabilities, designed to protect cloud-native infrastructure and applications. CNAPPs incorporate an integrated set of proactive and reactive security capabilities, including artifact scanning, security guardrails, configuration and compliance management, risk detection and prioritization, and behavioral analytics, providing visibility, governance and control from code creation to production runtime. CNAPP solutions use a combination of API integrations with leading cloud platform providers, continuous integration/continuous development (CI/CD) pipeline integrations, and agent and agentless workload integration to offer combined development and runtime security coverage.*” 

In other words, CNAPP is an umbrella term for multiple security capabilities that are integrated into a unified platform to secure cloud-native applications and infrastructure throughout their lifecycle. A typical CNAPP includes the following core capabilities:

* **Cloud Security Posture Management (CSPM):** Identifies misconfigurations and compliance risks in cloud infrastructure.  
* **Cloud Infrastructure Entitlement Management (CIEM):** Manages and automates access permissions in cloud environments.  
* **Cloud Workload Protection Platform (CWPP):** Protects cloud workloads, including containers, servers, and serverless functions.  
* **Infrastructure-as-Code (IaC) Scanning:** Detects security issues early in the development process.  
* **Data Security Posture Management (DSPM):** Discovers sensitive data sources and who has access to them.

Below is an image from Gartner that provides a detailed view of the CNAPP capabilities landscape:

![CNAPP Capabilities - Gartner](images/CNAPP%20Capabilities%20-%20Gartner.png)

# Tool Selection Criteria

When choosing a CNAPP solution, even though no vendor excels in all areas of CNAPP, I strongly advocate for a "best-of-suite" approach rather than assembling multiple "best-of-breed" tools. It is operationally easier to roll out across the organization, reduces the complexity associated with maintaining and managing various tools, and provides unified visibility across the different types of findings as it eliminates the need to correlate data from disparate tools. When evaluating CNAPP solutions, consider the following:

* **Prioritize CSPM Offering:** Focus on vendors with strong capabilities in the areas most critical to your organization's needs and cloud security strategy.
     - Prioritize vendors that have strong CSPM, vulnerability scanning, and CIEM capabilities, even if the rest of their features may not be as robust.
     - Ensure that if not already supported, vendors have it in their short-term plan to provide support for the remaining CNAPP capabilities (CWPP, DSPM, attack surface management, software supply chain, policy-as-code (PaC), container image scanning, etc.).  
* **Include Stakeholders From Different Teams:** The capabilities that CNAPP offers extend to several different teams within the organization (Vulnerability Management, IAM, SecMon, Data Protection), and including them in the selection process enables the organization to make a collective decision and helps to minimize the potential politics that can occur when key parties are left out of the decision-making process.  
* **Require Graph Databases:** Given the dynamic and interconnectedness of modern cloud environments where complex relationships exist between resources, identities, configurations, and data flows, traditional relational databases can't keep up with modeling and analyzing these intricate webs of connections. This is where graph databases shine as they provide:
     - *Comprehensive Visibility:* Map the complex relationships between various cloud-native components (resources, identities, networking, security), providing a holistic view of the cloud estate and helping develop a context-aware architectural understanding of the different applications / workloads.
     - *Contextual Risk Prioritization:* Different types of findings (vulnerabilities, misconfigurations) on a given resource, coupled with the potential attack paths for that resource (by analyzing IAM and networking) can be correlated to apply better risk prioritization and perform targeted remediations.
     - *Multi-Cloud Consistency:* A unified graph data model where the representation of different resource types and relationships are standardized, can enable higher consistency in how security is applied across multi-cloud environments.   
* **Agentless Vulnerability Scanning:** Due to the ephemeral nature of resources and the decentralized nature of operating in the cloud where application and development teams own their entire stacks (including the lifecycle of the VM instances), consistently deploying agents on VMs is challenging, especially at organizations where the concept of golden images doesn’t exist. This is where agentless scanning can help streamline operations and provide automated vulnerability scanning capabilities.
     - However, agent-based solutions should be considered for Cloud Workload Protection Platform (CWPP) as it is more effective in real-time monitoring and blocking based on system activities.        
* **Prioritize Flexible APIs:** Give preferences to vendors that have well-defined and flexible APIs that allow you to do most actions via APIs that are possible via the console / UI. This allows you to build complex workflows in the future, set up integrations with enterprise tools (ticketing and messaging platforms, vulnerability warehouse, SIEM, etc.), build reports and metrics, drive remediation campaigns, etc.     
* **Alignment With Vendor Roadmaps:** Assess the vendor's roadmap to ensure it aligns with your future requirements and strategic goals. For example, if your organization is making a big push for AI workloads in the near-future, consider vendors that have AI Posture Management (AIPM) capabilities in their short-term roadmap. 

By selecting a best-of-suite CNAPP solution that has CSPM as its core capability and also has the required flexibility / extensibility as a platform, organizations can benefit from operational efficiencies, spend less time upskilling on multiple tools, and ultimately enhance their ability to manage risks in dynamic cloud-native environments.

# Day 1 Focus Areas

As CNAPP definitions evolve, new capabilities (read: acronyms) are added, and vendors market their platforms as the silver bullet for addressing all cloud security needs, it is tempting to enable every available feature. However, this approach can overwhelm security teams with alerts and frustrate developers with a sudden influx of issues that need to be remediated, creating friction between teams meant to collaborate on reducing security risks. To avoid these pitfalls, consider the following:

* **Prioritize CSPM Operationalization:** Prioritize enablement of CSPM and vulnerability scanning(from hereon “CSPM”) on day one of your CNAPP implementation journey. This approach provides the following benefits:
     - *Quick Value Realization:* These findings are typically easier to understand and address, allowing you to establish a baseline understanding of your cloud security posture and conduct “quick-wins” burndown campaigns to quickly reduce risks through issue resolution. This delivers rapid benefits from your CNAPP investment, crucial for building momentum and securing stakeholder buy-in.
     - *Platform Familiarity:* Starting with CSPM allows your security team to become familiar with the CNAPP platform's interface, graph database, data model, reporting mechanisms, and overall functionality. This foundational knowledge is essential for successfully implementing more advanced capabilities in the future.
     - *Upskilling Opportunity:* Implementing CSPM first gives your security teams time to upskill on cloud security concepts, creating a foundation for future implementation of more advanced capabilities.
     - *Developer Acclimation:* CSPM provides an opportunity for developers to understand how security issues are reported and how they need to consume and act on this information. This gradual introduction to the platform helps smooth the transition to a more security-aware development process.  
* **Enable Self-Service Consumption of Findings:** Set up role-based access control (RBAC) to enable self-service consumption of security findings for the developer community. This approach is crucial for conducting timely remediation and fostering a culture of shared responsibility within the organization. Below are the benefits of enabling self-service:
     - *Developer Empowerment:* The onus for remediating issues is on the developers and cloud engineers; hence, providing them with self-service access to relevant security findings and remediation guidance allows them to take ownership of security issues associated with their applications.
     - *Accelerated Remediation:* Generally, CNAPP platforms have descriptive remediation guidance that is actionable. By putting information directly in the hands of those who can act on it, your organization can significantly speed up the fix process and reduce the overall time-to-remediation.
     - *Shared Responsibility:* By providing developers with tool access, they can monitor the security posture of their applications on-demand, and provide inputs on false positives or potential policy adjustments, thereby fostering a collaborative culture for cloud security. In practice, developers and engineers often use CNAPP platforms for their own non-security related use-cases (e.g., tagging, building cloud asset inventory, identifying resources that are no longer needed, etc.).  
* **Custom Develop & Fine-Tune CSPM Policies:** As your organization becomes familiar with the initial roll-out, it is crucial to move beyond out-of-the-box (OOB) policies towards custom policy development and fine-tuning (“customization”) to better align with the organization’s security standards and reduce false positives. Below are key considerations for customizing policies:
     - *Reduce Alert Fatigue & False Positives:* Out-of-the-box policies often generate a high volume of alerts, many of which may be false positives / irrelevant to your environment. Customization helps reduce noise and allows your teams to focus on the most critical issues.
     - *Align with Security Standards:* OOB policies may not adequately address your organization's unique risk profile or compliance requirements and may not be aligned with your security standards and controls. By developing custom policies based on security standards & cloud usage patterns / application architectures, you can have higher confidence in your policy set and improved accuracy in the legitimacy of the issues identified. Additionally, since these policies are contextualized for your organization’s cloud environments, they reduce friction and improve adoption among your development teams.
* **Metrics & Reporting:** Develop reporting for CSPM, ideally aligned to your SLA / vulnerability management standards, to measure the overall health of the program and remediation effectiveness. This will enable cultural changes and set proper expectations around remediation timelines.  
* **Conduct Regular Office Hours:** Hold regular sessions and office hours, participate in developer forums, set up communication channels, etc. to educate the developers on the platform and hand-hold them through remediations when doing the initial roll-out. This will build goodwill and allow you to develop effective processes that incorporate valuable insights based on how your development teams think and operate. 

Prioritizing CSPM as your initial focus establishes a robust foundation that not only addresses critical security risks but also allows the organization to build confidence in its processes, workflows, and ability to remediate issues. This also provides the foundation for the implementation of more advanced CNAPP capabilities in a manageable manner with minimal resistance / disruptions.

In summary, the tool in itself is very powerful and provides good visibility but the successful implementation and ROI are dependent on people and process elements where activities like fostering a shared responsibility culture, partnering and enabling with developers, phasing the roll-out of the platform, and designing effective processes are critical factors for success. 

PS: CNAPP is a broad area with many layers to unpack and there are several topic areas (listed below) that I did not cover in this blog post that require deep-dive of their own and will be good candidates for a follow-up blog on “day 2” activities and beyond:

* Pre-deployment security capabilities such as container image scanning, secrets scanning, software supply chain  
* Integrations with ticketing and messaging platform (Jira, Slack), SIEM (Splunk, Sentinel), vulnerability warehouse (ServiceNow VR)  
* Strategy for routing different styles of issues to different sources  
* Operationalizing the various CNAPP capabilities

References:

1. Gartner \- [https://www.gartner.com/doc/reprints?id=1-2I6V52K2&ct=240726&st=sb](https://www.gartner.com/doc/reprints?id=1-2I6V52K2&ct=240726&st=sb)
