# Azure-SOC and Honeynet with Live Traffic
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/de424de9-81e9-4019-85b4-e857271330f5" height="80%" width="80%">
<h2>Overview</h2>
The main goal of this lab is to deliberately subject our environment to potential threats, enabling us to observe and analyze the activities of malicious actors on the internet. I constructed a miniature honeynet in Azure, gathered log data from diverse sources into a Log Analytics workspace, and utilized Microsoft Sentinel to generate attack maps, initiate alerts, and manage incidents. After assessing security metrics in the unsecured environment for 24 hours, implementing security controls to fortify the environment, and measuring metrics for an additional 24 hours, the results are presented below. The metrics we will display include:
<br></br>
<li> SecurityEvent (Windows Event Logs) </li>
<li>Syslog (Linux Event Logs)</li>
<li>SecurityAlert (Log Analytics Alerts Triggered)</li>
<li>SecurityIncident (Incidents created by Sentinel)</li>
<li>AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)</li>

<h2>Technologies, Regulations, and Azure Components Employed:</h2>
<li>Virtual Network (VNet)</li>
<li>Network Security Group (NSG)</li>
<li>Virtual Machines (2 windows, 1 linux)</li>
<li>Log Analytics Workspace for performing Kusto Query (KQL) queries to filters data</li>
<li>Azure Key Vault to securely store secrets</li>
<li>Azure Storage Account for storing data</li>
<li>Microsoft Sentinel for Security Information and Event Management (SIEM)</li>
<li>Microsoft Defender for Cloud to monitor resources and ingest logs from virtual machines into Log Analytic Workspace</li>
<li>Powershell for performing simulated attacks</li>
<li>Command Line Interface (CLI) to verify connection to virtual machine</li>
<li>Windows Remote Desktop to remotely connect to virtual machines</li>
<li>NIST SP 800-53 Revision 4 for Security Controls</li>
<li>NIST SP 800-61 Revision 2 for Incident Handling Guidance</li>

<br />

# Architecture Before Hardening / Security Controls
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/4d57b6f4-0489-41ac-9b43-8d339703ac57" height="80%" width="80%"/>
<br />
<p>This system architecture depicts the vulnerable condition of the system before the implementation of security measures. At this stage, the system was entirely accessible from the internet, and both Network Security Groups and inherent firewalls were configured to permit all types of traffic. Access control rules were established, facilitating unrestricted communication among network endpoints. Moreover, all remaining resources were deployed with publicly visible endpoints to the internet as well. These resources included Azure Key Vault, Microsoft SQL Server, Azure Blob Storage, and Azure Entra ID (formerly Active Directory).
  <br></br>
  By configuring diagnostic settings, audit logs, and appropriate data connectors, the logs from these resources were aggregated and sent to both Microsoft Sentinel and the Log Analytics workspace. This enabled the monitoring and analysis of security events, forming the basis for alerting, incident response, and the implementation of security controls guided by NIST SP 800-53.
</p>
  <br/>
<br />

# Attack Maps Before Hardening / Security Controls
<li><strong>This attack map showcases the consequences of leaving the Network Security Group (NSG) open.</strong></li>
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/40cffde9-041d-4d0a-86f7-04fe8e2edb09" height="80%" width="80%"/>
<br />

<br></br>
<li><strong>This attack map showcases RDP and SMB failures against the Window machine.</strong></li>
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/8d25a2a7-990d-4beb-8a11-436adbdc2077" height="80%" width="80%" />
<br/>

<br></br>
<li><strong>This attack map showcases failures against the Microsoft SQL Server in the Windows VM.</strong></li>
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/aa13d533-bfa8-4852-a940-be0bc9c22d9f" height="80%" width="80%"/>
<br/>

<br></br>
<li><strong>This attack map showcases SSH login failures against the Linux VM.</strong></li>
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/04fca1e2-6606-4bcf-936b-96e35af269cd" height="80%" width="80%"/>
<br/>

# Architecture After Hardening / Security Controls
<img src="https://github.com/jacksontyren/Azure-SOC-/assets/121649532/dfbe06d1-e267-43b9-b46c-5fe4c5f31694" height="80%" width="80%"/>
<br/>
 <p>In the "AFTER" stage, I implemented a series of hardening measures and security controls to improve the environment's overall security posture. These improvements included:

<li>Network Security Groups (NSGs): I hardened the NSGs by blocking all inbound and outbound traffic, with the sole exception of my own public IP address. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines. </li>

<li>Built-in Firewalls: I configured the built-in firewalls on the virtual machines to restrict access and protect the resources from unauthorized connections. This step involved fine-tuning the firewall rules based on the specific requirements of each VM, thereby minimizing the potential attack surface.</li>

<li>Private Endpoints: To enhance the security of other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to the virtual network and not exposed to the public internet. As a result, these resources were protected from unauthorized access and potential attacks.</li><br>
By comparing the security metrics before and after implementing these hardening measures and security controls, I was able to demonstrate the effectiveness of each step in improving the overall security posture of the Azure environment. </br>

# Metrics Before Hardening / Security Controls
<table>
  <tr>
    <th>Metric</th>
    <th>Count</th>
  <tr>
    <td>Security Events (Windows VMs)</td>
    <td>26038</td>
  </tr>
  <tr>
    <td>Syslog (Linux VMs)</td>
    <td>4764</td>
  </tr>
  <tr>
    <td>SecurityAlert (Microsoft Defender for Cloud)</td>
    <td>9</td>
  </tr>
  <tr>
    <td>Security Incident (Sentinel Incidents)</td>
    <td>207</td>
  </tr>
  <tr>
    <td>NSG Inbound Malicious Flows Allowed</td>
    <td>4732</td>
  </tr>
</table>

# Metrics After Hardening / Security Controls
<table>
  <tr>
    <th>Metric</th>
    <th>Count</th>
  <tr>
    <td>Security Events (Windows VMs)</td>
    <td>9757</td>
  </tr>
  <tr>
    <td>Syslog (Linux VMs)</td>
    <td>1</td>
  </tr>
  <tr>
    <td>SecurityAlert (Microsoft Defender for Cloud)</td>
    <td>0</td>
  </tr>
  <tr>
    <td>Security Incident (Sentinel Incidents)</td>
    <td>0</td>
  </tr>
  <tr>
    <td>NSG Inbound Malicious Flows Allowed</td>
    <td>0</td>
  </tr>
</table>

# Utilizing NIST SP 800-61: Computer Security Incident Handling Guide
<p>For each attack and simulated attacks I practiced incident response following NIST SP 800-61 Rev 2.
<img src="https://github.com/jacksontyren/jacksontyren/assets/121649532/5885529b-4118-428e-b4f5-2abdef207334" height="80%" width="80%"/>
<br />
<br />
  
  # Conclusion
<p>In this project, a concise honeynet was established in Microsoft Azure, integrating log sources into a Log Analytics workspace. Security controls, guided by NIST SP 800-53, were systematically applied to fortify the environment. Microsoft Sentinel played a pivotal role in triggering alerts and generating incidents based on ingested logs. To simulate incident response and remediation, NIST SP 800-61 was employed, guiding the remediation steps for incidents identified by Microsoft Sentinel. Metrics were initially measured in the insecure environment, then reevaluated after implementing security measures, showcasing a substantial reduction in security events and incidents, emphasizing the effectiveness of the applied security controls in safeguarding the environment.  <br/></p>
