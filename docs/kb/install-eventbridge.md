---
layout: docs
toc_id: install-eventbridge
title: VMware Event Broker Appliance - EventBridge
description: Deploying VMware Event Broker Appliance with AWS EventBridge
permalink: /kb/install-eventbridge
cta:
 title: What Next? 
 description: At this point, you have successfully extended your SDDC to AWS. You can take advantage of the number of AWS resources that can be configured as targets for AWS EventBridge
 actions:
  - text: AWS EventBridge documentation - [here](https://docs.aws.amazon.com/eventbridge/latest/userguide/what-is-amazon-eventbridge.html).
---

# Deploy VMware Event Broker Appliance with AWS EventBridge

**<font color="red">This Event Processor has been deprecated in the VMware Event Broker Appliance v0.7 release and will be removed in next release.</font>**

Customers looking to seamlessly extend their vCenter through native AWS components (lambda, cloud watch etc) can get started quickly by deploying VMware Event Broker Appliance with AWS EventBridge as the Event Processor

## Appliance Deployment Steps

### Requirements

* 4 vCPU and 8GB of memory for VMware Event Broker Appliance
* vCenter Server 6.x or greater
* vCenter TCP/443 accessible from Appliance IP address
* Account to login to vCenter Server (readOnly is sufficient)
* Access credentials for AWS account
* Details about the AWS EventBridge setup (Rules, Region etc) 

### Step 1 - Download the VMware Event Broker Appliance (OVA) from the [VMware Fling site](https://flings.vmware.com/vmware-event-broker-appliance){:target="_blank"}.

### Step 2 - Deploy the VMware Event Broker Appliance OVA to your vCenter Server using the vSphere HTML5 Client. As part of the deployment you will be prompted to provide the following input:

#### **Networking** (**Required**)

  * Hostname - The FQDN of the VMware Event Broker Appliance. If you do not have DNS in your environment, make sure the hostname provide is resolvable from your desktop which may require you to manually add a hosts entry. Proper DNS resolution is recommended
  * IP Address - The IP Address of the VMware Event Broker Appliance
  * Network Prefix - Network CIDR Selection (e.g. 24 = 255.255.255.0)
  * Gateway - The Network Gateway address
  * DNS - DNS Server(s) that will be able to resolve to external sites such as Github for initial configuration. If you have multiple DNS Servers, input needs to be **space separated**.
  * DNS Domain - The DNS domain of your network
  * NTP Server - NTP Server(s) for proper time synchronization. If you have multiple DNS Servers, input needs to be **space separated**.

#### **Proxy Settings** (Optional)
  * HTTP Proxy Server - HTTP Proxy Server followed by the port (e.g. http://proxy.provider.com:3128)
  * HTTPS Proxy - HTTPS Proxy Server followed by the port (e.g. http(s)://proxy.provider.com:3128)
  * Proxy Username - Optional Username for Proxy Server
  * Proxy Password - Optional Password for Proxy Server
  * No Proxy - Exclude internal domain suffix. Comma separated (localhost, 127.0.0.1, domain.local)

#### **OS Credentials** (**Required**)
  * Root Password - This is the OS root password for the VMware Event Broker Appliance
  * Enable SSH - Check the box to allow SSH to the Appliance (SSH to the appliance is disabled by default)

#### **vSphere** (**Required**)

  * vCenter Server - This FQDN or IP Address of your vCenter Server that you wish to associate this VMware Event Broker Appliance to for Event subscription
  * vCenter Username - The username to login to vCenter Server, as mentioned earlier, readOnly account is sufficient
  * vCenter Password - The password to the vCenter Username
  * Disable vCenter Server TLS Verification - If you have a self-signed SSL Certificate, you will need to check this box

#### **Event Processor Configuration** (**Required**)
  * Event Processor - Choose AWS EventBridge

#### **AWS EventBridge Configuration**
  * Access Key - A valid AWS Access Key to AWS EventBridge
  * Access Secret - A valid AWS Access Secret to AWS EventBridge
  * Event Bus Name - Name of the AWS Event Bus to use. If left blank, this defaults to "default" Bus name.
  * Region - Region where Event Bus is running (e.g. us-west-2)
  * Rule ARN - ID of the Rule ARN created in AWS EventBridge
  * Advanced Settings - N/A, future use

> For more information on using the OpenFaaS and AWS EventBridge Processor, please take a look at the [VMware Event Router documentation](https://github.com/vmware-samples/vcenter-event-broker-appliance/blob/development/vmware-event-router/README.MD){:target="_blank"}

#### **zAdvanced** (Optional)
  * Debugging - When enabled, this will output a more verbose log file that can be used to troubleshoot failed deployments
  * POD CIDR Network - Customize POD CIDR Network (Default 10.99.0.0/20). Must not overlap with the appliance IP address.

### Step 3 - Power On the VMware Event Broker Appliance after successful deployment. Depending on your external network connectivity, it can take a few minutes while the system is being setup. You can open the VM Console to view the progress. Once everything is completed, you should see an updated login banner for the various endpoints:

```
Appliance Status: https://[hostname]/status
Install Logs: https://[hostname]/bootstrap
Appliance Statistics: https://[hostname]/stats
```

> NOTE
- When configured with the AWS EventBridge Processor, the OpenFaaS UI endpoint will not be available which is expected and is not shown in the login banner.
- If you enable Debugging, the install logs endpoint will automatically contain the more verbose log entries.

### Step 4 - You can verify that everything was deployed correctly by opening a web browser and accessing one of the endpoints along with the associated admin password you had specified as part of the OVA deployment.

