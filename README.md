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
<p>This architecture represents the insecure state of the system prior to implementing security controls. It was completely exposed to the internet, with both Network Security Groups and built-in firewalls set to allow all traffic. Access control rules were created to allow unrestricted communication between network endpoints. In addition, all other resources were deployed with public endpoints visible to the internet, without using any Private Endpoints
</p>
  <br/>
<br />

<img src="https://github.com/jacksontyren/jacksontyren/assets/121649532/7236b0cf-5ebb-4a74-9028-a5cb91f0a05d" height="80%" width="80%" />
<br />
 <p align="center">On the main screen we can see that the application has begun listening on the network. The top section is used to display a list of the packets captured. These packets are displayed as a table. Information about each packet is displayed here, such as the packet number, packet source and destination, the time captured, and the packet’s protocol. </br>
<p align="center">
<strong>Step 3:</strong> Open Firefox and visit the following site:
http://testphp.vulnweb.com/login.php.
This is a login page. We notice the lock with the red line at the top left of the page, indicating that this page is communicating through HTTP (port 80). HTTP does not encrypt data coming through like its secure counterpart HTTPS (port 443).<br/>
<img src="https://github.com/jacksontyren/jacksontyren/assets/121649532/87685577-7103-4384-81da-7bc4da51b50d" height="80%" width="80%"/>
<br />
<br />
Enter a random username and password into this form and click login. Once this is done, return to Wireshark.  <br/>
<img src="https://github.com/jacksontyren/jacksontyren/assets/121649532/b421162a-2808-4960-97f3-0653edf5dfbc" height="80%" width="80%"/>
<br />
<br />
<strong>Step 4:</strong> Click the stop button to end the packet capture. We can see that there is many more packets of data available for analysis. On the top in the filter tab enter "http" to filter for only HTTP requests.  <br/>
<img src="https://github.com/jacksontyren/jacksontyren/assets/121649532/c043656f-9a53-4b34-9bc1-373113396b04" height="80%" width="80%"/>
<br />
<br />
<strong>Step 5:</strong> Look for the packet with POST included in the info section of the first pane (1). Once you find it, select this packet. POST is an HTTP method used to send data to a web server for processing by a resource. In this case, this is when credentials were entered and the login button was clicked.
In the second pane under the tab called Hypertext Transfer Protocol, when it is clicked the login information is displayed that was entered on the vulnerable website (2). <br/>
<img src="https://github.com/jacksontyren/jacksontyren/assets/121649532/335f92a8-a7ad-460e-bba8-7887ef8e00d7" height="80%" width="80%"/>
</p>
