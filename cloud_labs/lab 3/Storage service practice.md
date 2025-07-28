Exploring Huawei Cloud's Storage Services: A Step-by-Step Lab Report

Introduction
 
 This lab is designed to provide hands-on experience with Huawei Cloud’s Elastic Volume Service (EVS). By integrating EVS with an Elastic Cloud Server (ECS), the exercise helps users understand how cloud storage solutions can be provisioned, attached, and used efficiently. This practical setup highlights the scalability and flexibility of cloud-based storage infrastructure, which is essential for modern enterprise workloads.

Step 1: Creating and Accessing the ECS Instance
Login to Huawei Cloud Console:
Access the Huawei Cloud portal using your IAM user credentials provided by the lab.

Navigate to ECS Service:
From the service list, search for and select Elastic Cloud Server (ECS).

Create a New ECS Instance:

Click on "Buy ECS" and switch to the Custom Config tab.

Choose the Billing Mode as Pay-per-use and select the AP-Singapore region.

Set architecture to x86 and flavor to c6.xlarge.2 with 4 vCPUs and 8 GiB RAM.

Use CentOS 8.2 64bit (40 GiB) as the image.

Assign a public IP address with dynamic BGP and 100 Mbps bandwidth.

Name the ECS ecs-cent and set the password to Huawei1234%.

Launch and Access the ECS:
Once launched, connect to the ECS using CloudShell and enter the assigned password to access the instance.

Step 2: Attaching and Formatting EVS
Attach EVS to ECS:

From the Huawei Cloud console, create a new EVS disk (default 40 GiB is sufficient).

Attach this disk to your running ECS instance.

Format and Mount the Disk:

Use lsblk or fdisk -l to verify the disk is detected (commonly /dev/vdb).

Run:

bash
Copy
Edit
sudo mkfs.ext4 /dev/vdb
to format the disk.

Create Mount Point and Mount the Volume:

Create a directory:

bash
Copy
Edit
sudo mkdir /mnt/data
Mount the EVS disk:

bash
Copy
Edit
sudo mount /dev/vdb /mnt/data
Persist Mount After Reboot:

Edit the /etc/fstab file to auto-mount the volume after reboot:

bash
Copy
Edit
echo '/dev/vdb /mnt/data ext4 defaults 0 0' | sudo tee -a /etc/fstab


<img width="597" height="377" alt="wordpress" src="https://github.com/user-attachments/assets/9d829f06-9b57-44a5-a075-cf076112e347" />

<img width="671" height="91" alt="vpcccccc" src="https://github.com/user-attachments/assets/6351afef-e75f-46b4-8ac6-1c7f2d233e45" />


