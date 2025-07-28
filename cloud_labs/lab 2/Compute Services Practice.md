In this lab, we explored how to set up and manage Elastic Cloud Servers (ECS) on HUAWEI CLOUD, focusing on creating both Windows and Linux ECS instances, modifying their specifications, and creating reusable private images for scalable deployments.

Part 1: Creating and Configuring ECS Instances

Step 1: Understanding the Lab Environment

Begin by accessing the [Lab Desktop], launching Google Chrome, and logging into HUAWEI CLOUD through the IAM User Login portal. Make sure to use the lab-provided credentials rather than a personal account.

Step 2: Creating a Virtual Private Cloud (VPC)

Navigate to Virtual Private Cloud > Create VPC, and set the following:

Region: AP-Singapore

Name: vpc-WP (use the name assigned in your lab account)

CIDR Block: 192.168.0.0/16

Subnet Name: subnet-WP

Subnet CIDR: 192.168.0.0/24

Step 3: Deploying ECS Instances

Go to Compute > Elastic Cloud Server > Buy ECS, and create a Windows-based ECS:

Billing Mode: Pay-per-use

Specs: s6.large.2, 2 vCPUs, 4 GB RAM

Image: Windows Server 2012 R2 Standard 64-bit

System Disk: High I/O, 40 GB

Security: Enable Host Security (Basic)

Login Mode: Password (e.g., Huawei@1234)

Repeat the process for a Linux ECS, adjusting:

Image: CentOS 7.6 64-bit

EIP: Auto assign

Step 4: Remote Login to ECS Instances

Use Remote Login to access the Windows ECS.

For Linux ECS (no GUI), log in using VNC, entering root as username and the configured password.

Part 2: Modifying ECS Specifications

To upgrade your ECS:

Stop the Windows ECS.

Select More > Modify Specifications, and update RAM from 4 GB to 8 GB.

Submit and restart the ECS to apply the changes.

Part 3: Creating a System Disk Image from an ECS

This allows reuse of custom ECS configurations across multiple deployments.

Windows ECS Configuration

Enable DHCP.

Enable Remote Desktop and firewall settings for RDP.

Ensure Cloudbase-Init is installed (already included in public images).

Creating a Windows Private Image

Go to Compute > Image Management Service > Create Image.

Set:

Type: System disk image

Source: ecs-windows

Name: image-windows2012

Confirm and submit. Wait until image status becomes Normal.

Modifying and Replicating Images

Use the Modify button to update image metadata.

Use More > Replicate to duplicate the image with a new name within the region.

Applying for an ECS Using a Private Image

From the Private Images tab, click Apply for Server next to the desired image to launch a new ECS with the preconfigured setup.

Part 4: Creating a Linux Private Image

Linux ECS Configuration

Confirm DHCP setup using vi /etc/sysconfig/network-scripts/ifcfg-eth0.

Ensure the following tools are installed:

CloudResetPwdAgent (installed by default)

Cloud-Init (verify with rpm -qa | grep cloud-init)

Run yum install to install Cloud-Init if missing.

Check and delete network rule files (/etc/udev/rules.d) to prevent NIC name conflicts in cloned instances.

Image Creation Process

Go to Image Management Service > Create Image.

Select ecs-linux and name the image (e.g., image-centos7.6).

Submit and wait for the image to reach Normal status.

Part 5: Sharing and Managing Images Across Tenants

Sharing an Image

Log in with your main Huawei Cloud account and locate your Project ID.

In the Sandbox environment, go to Private Images > More > Share and enter your Project ID.

Accepting the Shared Image

Log in to your main account, navigate to Shared Images, and click Accept.

Adding Tenants

From the Private Image page, select Add Tenant, input the desired Project ID, and confirm.

This completes the comprehensive process of deploying, configuring, customizing, and sharing ECS instances and images on HUAWEI CLOUD, providing a robust foundation for enterprise cloud management.

<img width="667" height="251" alt="ecsss" src="https://github.com/user-attachments/assets/3aef8498-f91c-4412-883a-f8064efd06ec" />
