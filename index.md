# Implementing CNAPP: Key Considerations for Success

According to Gartner, “*Cloud-native application protection platforms (CNAPPs) are a unified and tightly integrated set of security and compliance capabilities, designed to protect cloud-native infrastructure and applications. CNAPPs incorporate an integrated set of proactive and reactive security capabilities, including artifact scanning, security guardrails, configuration and compliance management, risk detection and prioritization, and behavioral analytics, providing visibility, governance and control from code creation to production runtime. CNAPP solutions use a combination of API integrations with leading cloud platform providers, continuous integration/continuous development (CI/CD) pipeline integrations, and agent and agentless workload integration to offer combined development and runtime security coverage.*”

![CNAPP Capabilities - Gartner](images/CNAPP%20Capabilities%20-%20Gartner.png)

As someone who has done multiple CNAPP implementations, I've summarized below my key considerations to help maximize the benefits of CNAPP:

1. [Tool Selection:](\#tool-selection)  
   1. [Graph Databases Are Table Stakes](\#graph-databases-are-table-stakes)  
   2. [Select Best-of-Suite](\#select-best-of-suite)  
2. [Day 1 Focus Areas:](\#day-1-focus-areas:)  
   1. [Operationalize Cloud Security Posture Management (CSPM)](\#operationalize-cloud-security-posture-management-(cspm))   
   2. [Enable Self-Service Consumption of Findings](\#enable-self-service-consumption-of-findings)  
3. [Day 2 Focus Areas:](\#day-2-focus-areas:)  
   1. [Customize & Fine-Tune Policies](\#customize-&-fine-tune-policies)  
   2. [Implement Additional CNAPP Capabilities](\#implement-additional-cnapp-capabilities)

# Tool Selection

## Graph Databases are Table Stakes  {#graph-databases-are-table-stakes}

The days of relying on simplistic, list-based asset inventories are long gone. Modern cloud environments are incredibly dynamic and interconnected, with complex relationships between resources, identities, configurations, and data flows. Traditional relational databases simply can't keep up with modeling and analyzing these intricate webs of connections. This is where graph databases shine. Below are key considerations when selecting a CNAPP platform with graph databases:

* Comprehensive Visibility: Map the complex relationships between various cloud-native components (resources, identities, networking, security), providing a holistic view of the cloud estate and helps develop context-aware architectural understanding for the different applications / worklaods  
* Contextual Risk Prioritization: Different types of findings (vulnerabilities, misconfigurations) on a given resource, coupled with the potential attack paths for that resource (by analyzing IAM and networking) can be correlated to apply better risk prioritization and perform targetted remediations  
* Multi-Cloud Consistency: A unified graph data model where the representation of different resource types and relationships are standardized, can enable higher consistency in how security is applied across multi-cloud environments. 

In conclusion, while there are certainly other important factors to consider when choosing a CNAPP, I firmly believe that graph database technology should be at the top of the list. It is no longer a nice-to-have feature, but an essential foundation for truly understanding and securing complex cloud-native environments.

## Select Best-of-Suite {#select-best-of-suite}

When choosing a CNAPP solution, I strongly advocate for a "best-of-suite" approach rather than assembling multiple "best-of-breed" tools. Below are key considerations when selecting CNAPP platforms:

* Unified Visibility: A single, integrated platform offers a comprehensive view of your cloud security posture throughout the entire application lifecycle. This holistic perspective is crucial for effective risk management in complex cloud environments, as it eliminates the need to correlate data from disparate tools.  
* Seamless Integration: Best-of-suite solutions typically provide superior integration between various security capabilities. This seamless integration results in more efficient workflows, reduced alert fatigue, and improved user experience for the developer and engineering community when rolling out security capabilities.  
* Reduced Complexity: Fewer tools mean less operational overhead and less time that security teams have to spend learning multiple systems. This allows them to focus more on improving coverage, rolling out capabilities, and addressing risks.  
* Cost-Effectiveness: Managing a consolidated suite generally incurs lower costs compared to handling multiple standalone tools. This approach simplifies licensing and operational management, reducing both financial and administrative burdens.

However, it is important to recognize that no single vendor excels in all areas of CNAPP functionality. When evaluating solutions, consider the following:

* Prioritize Critical Capabilities: Focus on vendors that offer strong capabilities in the areas most critical to your organization's needs and cloud security strategy. For example, if your organization heavily uses containerized applications, prioritize vendors with robust container security features.  
* Evaluate Vendor Roadmaps: Assess the vendor's roadmap to ensure it aligns with your future requirements and strategic goals. For example, if your organization is making a big push for AI workloads in the near-future, consider vendors that have AIPM capabilities in their short-term roadmap.  

By selecting a best-of-suite CNAPP solution, organizations can benefit from operational efficiencies, spend less time upskilling on multiple tools, and ultimately enhance their ability to manage risks in dynamic cloud-native environments. This approach provides a more cohesive and effective security posture, crucial for navigating the complexities of modern cloud security.

# Day 1 Focus Areas: {#day-1-focus-areas:}

## Operationalize Cloud Security Posture Management (CSPM) {#operationalize-cloud-security-posture-management-(cspm)}

As CNAPP definitions evolve, new capabilities (read: acronyms) are added, and vendors market their platforms as the silver bullet for addressing all cloud security needs, it is tempting to activate every available feature. However, this approach can overwhelm security teams with alerts and frustrate developers with a sudden influx of issues, creating friction between teams meant to collaborate on reducing security risks. To avoid these pitfalls, I strongly recommend prioritizing Cloud Security Posture Management (CSPM) on day one of your CNAPP implementation journey. Below are key considerations for operationalizing CSPM:

* Rapid Risk Identification: CSPM findings are typically easier to understand and address, allowing you to establish a baseline understanding of your cloud security posture and conduct “quick-wins” burndown campaigns to quickly reduce risks through issue resolution.  
* Easy Implementation: Leveraging API integrations with cloud providers, CSPM is typically simpler to deploy than other CNAPP capabilities.  
* Quick Value Realization: CSPM delivers rapid benefits from your CNAPP investment, crucial for building momentum and securing stakeholder buy-in.  
* Platform Familiarity: Starting with CSPM allows your team to become familiar with the CNAPP platform's interface, graph database, data model, reporting mechanisms, and overall functionality. This foundational knowledge is essential for successfully implementing more advanced capabilities in the future.   
* Upskilling Opportunity: Implementing CSPM first gives your security and development teams time to upskill on cloud security concepts, creating a foundation for future expansions.  
* Developer Acclimation: CSPM provides an opportunity for developers to understand how security issues are reported and how they need to consume and act on this information. This gradual introduction to the platform helps smooth the transition to a more security-aware development process.

Prioritizing CSPM as your initial focus establishes a robust foundation that not only addresses critical security risks but also allows the organization to build confidence in its processes, workflows, and ability to remediate issues. This also provides the foundation for the  implementation of more advanced CNAPP capabilities in a manageable manner with minimal resistance / disruptions.

## Enable Self-Service Consumption of Findings {#enable-self-service-consumption-of-findings}

Another critical day-one consideration is setting up role-based access control (RBAC) to enable self-service consumption of security findings for the developer community. This approach is crucial for conducting timely remediation and fostering a culture of shared responsibility within the organization. Below are key considerations for self-service consumption of findings:

* Developer Empowerment: The onus for remediating issues is on the developers and cloud engineers; hence, providing them with self-service access to relevant security findings and remediation guidance allows them to take ownership of security issues associated with their applications.  
* Accelerated Remediation: Generally, CNAPP platforms have descriptive remediation guidance that is actionable. By putting information directly in the hands of those who can act on it, your organization can significantly speed up the fix process and reduce the overall time-to-remediation.  
* Shared Responsibility: By providing developers with tool access, they can monitor the security posture of their applications on-demand, provide inputs on false positives or potential policy adjustments, thereby fostering a collaborative culture for cloud security. In practice, developers and engineers often use CNAPP platforms for their own non-security related use-cases (e.g., tagging, building cloud asset inventory, identifying resources that are no longer needed, etc.). 

By enabling self-service consumption of findings, your organization can significantly reduce the time to remediation, improve developer engagement with security practices, and create a more collaborative approach to cloud security across your organization. 

\#\# \#5 \- Align scan policies between CSPM and IaC scanning to enable consistency in enforcement of requirements (Day 2\)

As you move into day two of your CNAPP implementation, it is crucial to align your scan policies between CSPM and Infrastructure as Code (IaC) scanning. This alignment is key to ensuring consistent enforcement of security requirements across your entire cloud lifecycle.

By harmonizing these policies, you create a seamless security experience from development through to production. Developers receive consistent feedback whether they're working on IaC templates or deploying resources directly in the cloud. This consistency helps reinforce good security practices and reduces confusion.

Moreover, aligned policies help shift security left more effectively. Issues caught in IaC scans can be addressed before they ever make it to production, reducing your overall risk exposure and the cost of remediation.

# Day 2 Focus Areas: {#day-2-focus-areas:}

## Customize & Fine-Tune Policies {#customize-&-fine-tune-policies}

As your organization becomes familiar with the initial roll-out, it is crucial to move beyond out-of-the-box (OOB) policies towards custom policy development and fine-tuning (“customization”) to better align to organization’s security standards and reduce false positives. Below are key considerations for customizing policies:

* Alignment with Security Standards: OOB policies may not adequately address your organization's unique risk profile or compliance requirements. Customization ensures that your CNAPP policies are closely aligned with your security standards and controls.  
* Reduced Alert Fatigue: Out-of-the-box policies often generate a high volume of alerts, many of which may be false-positives / irrelevant to your environment. Customization helps reduce noise and allows your teams to focus on the most critical issues.  
* Improved Accuracy: By customizing policies based on cloud usage patterns and application architectures, you can have higher confidence in your policy-set and improved accuracy in the legitimacy of the issues identified. Additionally, since these policies are contextualized for your organizations’ cloud environments, they reduce friction and improve adoption among your development teams.  
* Consistent Security Enforcement: As you look to “shift-left” and embed security requirements within the CI / CD pipelines, aligning infrastructure-as-code (IaC) scan policies with CSPM policies is a key step for enabling consistent monitoring and enforcement across the entire application lifecycle. This alignment ensures that security standards are applied uniformly from development through to production environments.

By focusing on policy customization and alignment, your can begin monitoring and enforcing security requirements that are aligned to your organizations’ standards and cloud development practices.This approach fosters a culture of proactive risk management.

## Implement Additional CNAPP Capabilities {#implement-additional-cnapp-capabilities}

Once your organization has established an operational rhythm with reviewing and addressing findings, and the team has become familiar with the CNAPP tool, the foundation is set to implement additional CNAPP capabilities (e.g., Cloud Infrastructure Entitlement Management (CIEM), Data Security Posture Management (DSPM), Cloud Workload Protection Platform (CWPP), AI Posture Management (AIPM)). Below are key considerations when implementing these additional capabilities:

* Current-State Discovery: Conduct a deep-dive analysis into the current state of your resources, such as identities, data planes, and containerized workloads. Understand how these resources are managed throughout their lifecycle, including naming conventions, policies, standards, and the key stakeholders responsible for their management and administration.  
* Define Use-Cases: Based on your current-state discovery, the architecture of your cloud environments, and your resource management approach, develop a comprehensive inventory of use-cases that require visibility and detection. This step ensures that your CNAPP implementation aligns with your specific operational needs and security objectives.  
* Customize & Fine-Tune: Out-of-the-box policies for CIEM, DSPM, and other capabilities may not account for the specific contexts of your cloud environments. Initially enabling these policies can generate excessive noise, such as flagging cloud admin roles as over-permissive when such access is an approved pattern. By understanding your current state and defining clear use-cases, you can apply the appropriate level of customization and methodically roll out additional CNAPP capabilities.   

By thoughtfully implementing additional CNAPP capabilities, you can enhance your organization's security framework, ensuring comprehensive protection across your cloud-native applications and infrastructure. This strategic expansion allows you to address emerging threats and operational challenges while maintaining alignment with your organization's security standards and business objectives.

This is a very high-level summary that is aimed at providing guidance to folks who are evaluating / in the early-stages of adopting CNAPP. In summary, the tool in itself is very powerful and provides good visibility but the successful implementation and ROI is dependent on people and process elements where activities like roll-out planning, process engineering, and fostering shared responsibility culture are critical for success. 

PS: CNAPP is a broad area with many layers to unpack and there are several topic areas (listed below) that I did not cover in this blog post that require deep-dive of its own:

* Pre-deployment security capabilities such as container image scanning, secrets scanning, software supply chain  
* Integrations with ticketing and messaging platform (Jira, Slack), SIEM (Splunk, Sentinel), vulnerability warehouse (ServiceNow VR)  
* Double-clicking into the various CNAPP capabilities  
* Metrics, reporting, and compliance monitoring

References:

1. Gartner \- [https://www.gartner.com/doc/reprints?id=1-2I6V52K2\&ct=240726\&st=sb](https://www.gartner.com/doc/reprints?id=1-2I6V52K2\&ct=240726\&st=sb)

