# Advanced Deploy VMware NSX-T Data Center 3.X

## Exam Sections

### Section 4 – Installation, Configuration, and Setup

#### Objective 4.1 - Prepare VMware NSX-T Data Center Infrastructure

##### NSX Manager VM and Host Transport Node System Requirements [^sysreq]

[^sysreq]: [NSX Manager VM and Host Transport Node System Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/installation/GUID-AECA2EE0-90FC-48C4-8EDB-66517ACFE415.html#GUID-AECA2EE0-90FC-48C4-8EDB-66517ACFE415)

###### Supported Hosts for NSX Managers

- ESXi
- KVM [RHEL 7.7 and Ubuntu 18.04.2 LTS]

###### NSX Manager VM Resource Requirements

| Appliance Size                                          | Memory | vCPU | Shares         | Reservations | Disk Space | VM Hardware Version |
|---------------------------------------------------------|--------|------|----------------|--------------|------------|---------------------|
| NSX Manager Extra Small (NSX-T Data Center 3.0 onwards) | 8 GB   | 2    | 81920, Normal  | 8192 MB      | 300 GB     | 10 or later         |
| NSX Manager Small VM ( NSX-T Data Center 2.5.1 onwards) | 16 GB  | 4    | 163840, Normal | 16384 MB     | 300 GB     | 10 or later         |
| NSX Manager Medium VM                                   | 24 GB  | 6    | 245760, Normal | 24576 MB     | 300 GB     | 10 or later         |
| NSX Manager Large VM                                    | 48 GB  | 12   | 491520, Normal | 49152 MB     | 300 GB     | 10 or later         |

###### Network Latency Requirements

- The maximum network latency between NSX Managers in a NSX Manager cluster is 10ms.
- The maximum network latency between NSX Managers and Transport Nodes is 150ms.

###### NSX Edge VM System Requirements

| Appliance Size | Memory | vCPU | Disk Space | VM Hardware Version | Notes |
| -------------- | ------ | ---- | ---------- | ------------------- | ----- |
| NSX Edge Small | 4 GB | 2 | 200 GB | 11 or later (vSphere 6.0 or later) | Proof-of-concept deployments only. Note: L7 rules for firewall, load balancing and so on are not realized on a Tier-1 gateway if you deploy a small sized NSX Edge VM.
| NSX Edge Medium | 8 GB | 4 | 200 GB | 11 or later (vSphere 6.0 or later) | Suitable when only L2 through L4 features such as NAT, routing, L4 firewall, L4 load balancer are required and the total throughput requirement is less than 2 Gbps.
| NSX Edge Large | 32 GB | 8 | 200 GB | 11 or later (vSphere 6.0 or later) | Suitable when only L2 through L4 features such as NAT, routing, L4 firewall, L4 load balancer are required and the total throughput is 2 ~ 10 Gbps. It is also suitable when L7 load balancer, for example, SSL offload is required. <br /> See Scaling Load Balancer Resources in the NSX-T Data Center Administration Guide. For more information about what the different load balance sizes and NSX Edge form factors can support, see https://configmax.vmware.com.
| NSX Edge Extra Large | 64 GB  | 16 | 200 GB | 11 or later (vSphere 6.0 or later) | Suitable when the total throughput required is multiple Gbps for L7 load balancer and VPN. <br /> See Scaling Load Balancer Resources in the NSX-T Data Center Administration Guide. For more information about what the different load balance sizes and NSX Edge form factors can support, see https://configmax.vmware.com.

##### NSX Manager Deployment, Platform, and Installation Requirements [^insreq]

[^insreq]: [NSX Manager Deployment, Platform, and Installation Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-A65FE3DD-C4F1-47EC-B952-DEDF1A3DD0CF.html)

| Requirements                 | Description                              |
| ---------------------------- | ---------------------------------------- |
| [Supported deployment methods](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-A65FE3DD-C4F1-47EC-B952-DEDF1A3DD0CF.html) | <ul><li>OVA/OVF</li><li>QCOW2</li></ul>  |
| [Supported platforms](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-AECA2EE0-90FC-48C4-8EDB-66517ACFE415.html)          | <ul><li>NSX Manager VM System Requirements</li><table>  <thead>  <tr>  <th>Appliance Size</th>  <th>Memory</th>  <th>vCPU</th>    <th>Shares</th>    <th>Reservations</th>    <th>Disk Space</th>    <th>VM Hardware Version</th>  </tr>  </thead>  <tbody>  <tr>  <td>NSX Manager Extra Small (NSX-T Data Center 3.0 onwards)</td>  <td>8GB</td>  <td>2</td>    <td>8192, Normal</td>    <td>8192 MB</td>    <td>300 GB</td>    <td>10 or later</td>  </tr>    <tr>  <td>NSX Manager Small VM (NSX-T Data Center 2.5.1 onwards</td>  <td>16GB</td>  <td>4</td>    <td>163840, Normal</td>    <td>16384 MB</td>    <td>300 GB</td>    <td>10 or later</td>  </tr>  <tr>  <td>NSX Manager Medium VM</td>  <td>24 GB</td>  <td>6</td>    <td>245760, Normal</td>    <td>24576 MB</td>    <td>300 GB</td>    <td>10 or later</td>  </tr>  <tr>  <td>NSX Manager Large VM</td>  <td>48 GB</td>  <td>12</td>    <td>491520, Normal</td>    <td>49152 MB</td>    <td>300 GB</td>    <td>10 or later</td>  </tr>  </tbody>  </table><li>Host Transport Node System Requirements - Supported Hypervisors for Host Transport Nodes </li><table><thead><tr><th>Hypervisor</th><th>Version</th><th>CPU Cores</th><th>Memory</th></tr></thead><tbody><tr><td>vSphere</td><td>[Supported Version](http://partnerweb.vmware.com/comp_guide/sim/interop_matrix.php)</td><td>4</td><td>16 GB</td></tr><td>CentOS Linux KVM</td><td>7.9, 8.4</td><td>4</td><td>16 GB</td><tr><td>Red Hat Enterprise Linux (RHEL) KVM</td><td>7.9, 8.2, 8.4</td><td>4</td><td>16 GB</td></tr><tr><td>SUSE Linux Enterprise Server KVM</td><td>12 SP4</td><td>4</td><td>16 GB</td></tr><tr><td>Ubuntu KVM</td><td>18.04.2 LTS, 20.04 LTS</td><td>4</td><td>16 GB</td></tr></tbody></table><li>On ESXi, it is recommended that the NSX Manager appliance be installed on shared storage</li></ul>  |
| IP address                 | An NSX Manager must have a static IP address. You can change the IP address after installation. Only IPv4 addresses are supported |
| NSX-T Data Center appliance password | <ul><li>At least 12 characters </li><li>At least one lower-case letter</li><li>At least one upper-case letter</li><li>At least one digit</li><li>At least one special character</li><li>At least five different characters</li><li>Default password complexity rules are enforced by the following [Linux](https://www.redhat.com/sysadmin/pluggable-authentication-modules-pam) [PAM](https://www.linuxjournal.com/article/2120) [module](http://www.linux-pam.org/Linux-PAM-html/Linux-PAM_MWG.html) arguments:<ul><li><code>retry=3</code>: The maximum number of times a new password can be entered, for this argument at the most 3 times, before returning with an error.</li><li><code>minlen=12</code>: The minimum acceptable size for the new password. In addition to the number of characters in the new password, credit (of +1 in length) is given for each different kind of character (other, upper, lower and digit).</li><li><code>difok=0</code>: The minimum number of bytes that must be different in the new password. Indicates similarity between the old and new password. With a value 0 assigned to difok, there is no requirement for any byte of the old and new password to be different. An exact match is allowed.</li><li><code>lcredit=1</code>: The maximum credit for having lower case letters in the new password. If you have less than or 1 lower case letter, each letter will count +1 towards meeting the current minlen value.</li><li><code>ucredit=1</code>: The maximum credit for having upper case letters in the new password. If you have less than or 1 upper case letter each letter will count +1 towards meeting the current minlen value.</li><li><code>dcredit=1</code>: The maximum credit for having digits in the new password. If you have less than or 1 digit, each digit will count +1 towards meeting the current minlen value.</li><li><code>ocredit=1</code>: The maximum credit for having other characters in the new password. If you have less than or 1 other characters, each character will count +1 towards meeting the current minlen value.</li><li><code>enforce_for_root</code>: The password is set for the root user.</li></ul></li></ul> |
| Hostname               | When installing NSX Manager, specify a hostname that does not contain invalid characters such as an underscore or special characters such as dot <code>"."</code>. If the hostname contains any invalid character or special characters, after deployment the hostname will be set to <code>nsx-manager</code>.<br />For more information about hostname restrictions, see [rfc952](https://tools.ietf.org/html/rfc952) and [rfc1123](https://tools.ietf.org/html/rfc1123). |
| VMware Tools  | The NSX Manager VM running on ESXi has VMtools installed. Do not remove or upgrade VMtools |
| System        | <ul><li>Verify that the system requirements are met. See [System Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-14183A62-8E8D-43CC-92E0-E8D72E198D5A.html)<br />Before you install NSX-T Data Center, your environment must meet specific hardware and resource requirements.<br />Before you configure Gateway Firewall features, make sure that the NSX Edge form factor supports the features. See <i>[Supported Gateway Firewall Features on NSX Edge](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/administration/GUID-8B8D1C4F-8734-4789-B5B3-7E509201AE7D.html)</i> topic in the <i>[NSX-T Data Center Administration Guide](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/administration/GUID-FBFD577B-745C-4658-B713-A3016D18CB9A.html)</i>.<br /><b>Gateway Firewall features supported on NSX Edge form factor</b><br /><table><thead><tr><th>Features/NSX Edge Form Factor</th><th>Small<br />2 vCPU, 4GB RAM (POC only)</th><th>Medium<br />4 vCPU, 8 GB RAM</th><th>Large<br />8 vCPU, 32 GB RAM</th><th>Extra Large<br />16 vCPU, 64 GB RAM</th><th>Bare Metal</th></tr></thead><tbody><tr><td>L3-L4 Firewall</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>User ID-based Access Control</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Application Access Control</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>URL Filtering</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>FQDN Analysis</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>IDPS</td><td>No</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Malware Detection</td><td>No</td><td>No</td><td>No</td><td>Yes</td><td>Yes</td></tr><tr><td>Sandboxing for unknown Threats</td><td>No</td><td>No</td><td>No</td><td>Yes</td><td>No</td></tr><tr><td>TLS Inspection</td><td>No</td><td>No</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>L2 and L3 VPN</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr><tr><td>Static, Dynamic Routing</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td><td>Yes</td></tr></tbody></table></li><ul><li>[NSX Manager VM and Host Transport Node System Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-AECA2EE0-90FC-48C4-8EDB-66517ACFE415.html)</li><li>[NSX Edge VM System Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-22F87CA8-01A9-4F2E-B7DB-9350CA60EA4E.html)</li><li>[NSX Edge Bare Metal Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-14C3F618-AB8D-427E-AC88-F05D1A04DE40.html)</li><li>[Bare Metal Server System Requirements](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-26D8A6AE-FC16-428D-8A11-6A581091F2CF.html)</li><li>[Bare Metal Linux Container Requirments](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-CE50BDA9-80E4-4FAE-9ED1-98D980B78271.html)</li></ul><li> Verify that the required ports are open. See [Ports and Protocols](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.2/installation/GUID-2ABB0F95-E918-43A1-B096-7401979D51AA.html) and refer to [VMware Ports and Protocols](https://ports.vmware.com/home/NSX-T-Data-Center) for more details.</li><li> Verify that a datastore is configured and accessible on the ESXi host.</li><li>Verify that you have the IP address and gateway, DNS server IP addresses, domain search list, and the NTP Server IP or FQDN list for the NSX Manager or Cloud Service Manager to use.</li><li>If you do not already have one, create the target VM port group network. Place the NSX-T Data Center appliances on a management VM network.<br />If you have multiple management networks, you can add static routes to the other networks from the NSX-T Data Center appliance.</li><li> Plan your NSX Manager IPv4 IP addressing scheme.</li></ul>

##### Objective 4.1.1 - Deploy VMware NSX-T Data Center Infrastructure components

VScodeNVDeploy2022!
discovercanadapracticetest.com
nsx-manager-
.ca.pool.ntp.org
192.168.75.
192.168.16.8

###### Design - vSphere Networking Design for the Management Domain [^vvd6.2]

[^vvd6.2]: [vSphere Networking Design for the Management Domain](https://docs.vmware.com/en/VMware-Validated-Design/6.2/sddc-architecture-and-design-for-the-management-domain/GUID-451FC9C4-3CEA-45E4-BD3B-727A94225C3F.html)

| Design Goal | Description |
| ----------- | ----------- |
| Meet Diverse needs | The network must meet the diverse needs of many different entities in an organization. These entities include applications, services, storage, administrators, and users. |
| Reduce costs | Reducing costs is one of the simpler goals to achieve in the vSphere infrastructure. Server consolidation alone reduces network costs by reducing the number of required network ports and NICs, but a more efficient network design is desirable. For example, configuring two 25-GbE NICs might be more cost effective than configuring four 10-GbE NICs. |
| Improve performance | You can achieve performance improvement and decrease the time that is required to perform maintenance by providing sufficient bandwidth, which reduces contention and latency. |
| Improve availability | A well-designed network improves availability, usually by providing network redundancy. |
| Support security | A well-designed network supports an acceptable level of security through controlled access and isolation, where required. |
| Enhance infrastructure functionality | You can configure the network to support vSphere features such as vSphere vMotion, vSphere High Availability, and vSphere Fault Tolerance.|

##### Objective 4.1.2 - Configure Management, Control and Data plane components for NSX-T Data Center

##### Objective 4.1.3 - Configure and Manage Transport Zones, IP Pools, Transport Nodes etc

##### Objective 4.1.4 - Confirm the NSX-T Data Center configuration meets design requirements

##### Objective 4.1.5 - Deploy VMware NSX-T Data Center Infrastructure components in a multi-site configuration [^msfed] [^desguide3.2] [^desguide3.1]

[^msfed]: [March 2021 - VMware TAM Customer Webinar - NSX-T Multisite vs Federation](https://www.youtube.com/watch?v=5EkSRqq4QxM)
[^desguide3.2]: [NSX-T Multi-Location Design Guide (Federation + Multisite) - 3.2](https://communities.vmware.com/t5/VMware-NSX-Documents/NSX-T-Multi-Location-Design-Guide-Federation-Multisite/ta-p/2810327?attachment-id=108603)
[^desguide3.1]: [NSX-T Multi-Location Design Guide (Federation + Multisite) - 3.1](https://communities.vmware.com/t5/VMware-NSX-Documents/NSX-T-Multi-Location-Design-Guide-Federation-Multisite/ta-p/2810327?attachment-id=108604)

#### Objective 4.2 - Create and Manage VMware NSX-T Data Center Virtual Networks

##### Objective 4.2.1 - Create and Manage Layer 2 services

##### Objective 4.2.2 - Configure and Manage Layer 2 Bridging

##### Objective 4.2.3 - Configure and Manage Routing including BGP, static routes, VRF Lite and EVPN

#### Objective 4.3 - Deploy and Manage VMware NSX-T Data Center Network Services

##### Objective 4.3.1 - Configure and Manage Logical Load Balancing

##### Objective 4.3.2 - Configure and Manage Logical Virtual Private Networks (VPNs)

##### Objective 4.3.3 - Configure and Manage NSX-T Data Center Edge and NSX-T Data Center Edge Clusters

##### Objective 4.3.4 - Configure and Manage NSX-T Data Center Network Address Translation

##### Objective 4.3.5 - Configure and Manage DHCP and DNS

#### Objective 4.4 - Secure a virtual data center with VMware NSX-T Data Center

##### Objective 4.4.1 - Configure and Manage Distributed Firewall and Grouping Objects

##### Objective 4.4.2 - Configure and Manage Gateway Firewall

##### Objective 4.4.3 - Configure and Manage Identity Firewall

##### Objective 4.4.4 - Configure and Manage Distributed IDS

##### Objective 4.4.5 - Configure and Manage URL Analysis

##### Objective 4.4.6 - Deploy and Manage NSX Intelligence

#### Objective 4.5 - Configure and Manage Service Insertion

#### Objective 4.6 - Deploy and Manage Central Authentication (Workspace ONE access)

### Section 5 - Performance-tuning, Optimization, Upgrades

#### Objective 5.1 - Configure and Manage Enhanced Data Path (N-VDSe)

#### Objective 5.2 - Configure and Manage Quality of Service (QoS) settings

### Section 6 – Troubleshooting and Repairing

#### Objective 6.1 - Perform Advanced VMware NSX-T Data Center Troubleshooting [^tshoot6.4] [^opguide3.0]

[^tshoot6.4]: [Troubleshooting NSX Manager Issues](https://docs.vmware.com/en/VMware-NSX-Data-Center-for-vSphere/6.4/com.vmware.nsx.troubleshooting.doc/GUID-4377A9D9-467C-45FD-AC1E-C26A42BD3A1F.html)
[^opguide3.0]: [Troubleshooting Tools & Case Study](https://nsx.techzone.vmware.com/resource/nsx-t-30-operation-guide#_Toc90690)

##### Objective 6.1.1 - Troubleshoot Common VMware NSX-T Data Center Installation/Configuration Issues

##### Objective 6.1.2 - Troubleshoot VMware NSX-T Data Center Connectivity Issues

##### Objective 6.1.3 - Troubleshoot VMware NSX-T Data Center Edge Issues

##### Objective 6.1.4 - Troubleshoot VMware NSX-T Data Center L2 and L3 services

##### Objective 6.1.5 - Troubleshoot VMware NSX-T Data Center Security services

##### Objective 6.1.6 - Utilize VMware NSX-T Data Center native tools to identify and troubleshoot issues.

### Section 7 – Administrative and Operational Tasks

#### Objective 7.1 - Perform Operational Management of a VMware NSX-T Data Center Implementation

##### Objective 7.1.1 - Backup and Restore Network Configurations

##### Objective 7.1.2 - Monitor a VMware NSX-T Data Center Implementation

##### Objective 7.1.3 - Manage Role Based Access Control

##### Objective 7.1.4 - Restrict management network access using VIDM access policies

##### Objective 7.1.5 - Manage syslog settings

#### Objective 7.2 - Utilize API and CLI to manage a VMware NSX-T Data Center Deployment

##### Objective 7.2.1 - Administer and Execute calls using the VMware NSX-T Data Center vSphere API
