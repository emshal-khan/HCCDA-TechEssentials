 Deploying an Enterprise Web Solution on Huawei Cloud

This lab outlines the step-by-step procedure to deploy a scalable and highly available enterprise website on Huawei Cloud, meeting two key requirements: separating database and service nodes across ECS instances, and automatically scaling resources based on traffic fluctuations.

1. Initial Setup and Account Login

Before starting, log in to Huawei Cloud using the provided IAM lab account via Google Chrome. This ensures access to the necessary cloud environment without using personal credentials.

2. Task Execution

2.1 Creating a Virtual Private Cloud (VPC)

Navigate to: Service List > Networking > Virtual Private Cloud

Create a VPC in the AP-Singapore region and name it (e.g., vpc-mp).

Use default parameters and confirm the setup.

2.2 Configuring a Security Group

Go to Access Control > Security Groups and create a new group.

Under Inbound Rules, allow all protocols and ports from any IP (0.0.0.0/0).

2.3 Launching an ECS Instance

Go to Compute > Elastic Cloud Server and click Buy ECS.

Choose:

Billing Mode: Pay-per-use

Image: CentOS 7.6 (64-bit)

Specs: 1 vCPU, 1 GB RAM (s6.small.1)

Disk: High I/O, 40 GB

Login: Password (e.g., Huawei@123!)

Attach the created VPC and security group, auto-assign an EIP, and finalize the purchase.

2.4 Buying an RDS Database Instance

Go to Database > Relational Database Service and choose Buy DB Instance.

Set:

Engine: MySQL 8.0

Specs: 2 vCPUs, 4 GB RAM

Storage: Cloud SSD

Use the same VPC and security group.

Set DB password (e.g., Huawei!@#$)

Wait for the instance to be created and note the floating IP for database connection.

3. Setting Up the LAMP Stack

3.1 Installing LAMP on ECS

Log in to the ECS via remote login using root credentials.

Install required packages:

yum install -y httpd php php-fpm php-mysql mysql

Edit Apache config:

vim /etc/httpd/conf/httpd.conf
# Add: ServerName localhost:80 at the end

Download and extract WordPress:

wget -c https://koolabsfiles.obs.ap-southeast-3.myhuaweicloud.com:443/20220731/wordpress-4.9.10.tar.gz
tar -zxvf wordpress-4.9.10.tar.gz -C /var/www/html
chmod -R 777 /var/www/html

Start services:

systemctl start httpd
systemctl start php-fpm
systemctl enable httpd
systemctl enable php-fpm

Verify service status and ensure the ECS EIP is accessible via browser.

3.2 Creating a WordPress Database on RDS

Log in to the RDS instance using the root account.

Execute:

CREATE DATABASE wordpress;

3.3 Completing WordPress Installation

Visit http://<ECS-EIP>/wordpress

Provide database info (name: wordpress, user: root, host: <RDS IP>:3306).

Configure site title, admin username/password, and finish installation.

4. Ensuring High Availability

To meet enterprise-grade availability, ELB and AS are configured.

4.1 Creating an Elastic Load Balancer (ELB)

Go to Networking > Elastic Load Balance > Buy ELB

Choose:

Type: Shared

Region: AP-Singapore

Network: Public

VPC/Subnet: Previously created

Assign a new EIP and create a listener with protocol HTTP and port 80.

Disable health checks and finish setup.

4.2 Creating a System Image

Stop the ECS instance.

Go to Image Management Service > Create Image.

Use the ECS as the source and name the image (e.g., ims-mp).

Start the ECS after image creation is complete.

4.3 Setting Up Auto Scaling (AS)

Go to Compute > Auto Scaling > Create AS Configuration.

Use the previously created image, security group, and omit EIP.

Then, create an AS Group and attach the ELB and AS configuration.

Add policies:

Scale Out: CPU >= 60%, add 1 instance

Scale In: CPU <= 20%, remove 1 instance

Monitor the ECS count to confirm AS policies are working.

5. Final Testing and Monitoring

5.1 Accessing the Website

Open http://<ELB-EIP>/wordpress/ in a browser.

Confirm successful access to verify that the load balancer is distributing traffic correctly.

5.2 Monitoring via Cloud Eye

Go to Management & Governance > Cloud Eye.

Review resource overview and any triggered alarms.

Monitor ECS instances and track performance metrics for future adjustments

<img width="1047" height="552" alt="login page" src="https://github.com/user-attachments/assets/315ef123-69d7-4201-89cb-eb3286bdaa32" />

<img width="436" height="102" alt="vpc2" src="https://github.com/user-attachments/assets/e0725849-2130-478e-9afe-151e3df81f49" />


