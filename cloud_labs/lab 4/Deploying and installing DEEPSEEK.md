Deploying DeepSeek-R1 1.5B on Huawei Cloud Using Ollama
This lab walks through the complete process of deploying the DeepSeek-R1 1.5B large language model on Huawei Cloud by leveraging the power of Ollama, a lightweight framework for running LLMs locally. By the end of this lab, learners will understand how to set up the cloud environment, install Ollama, and run DeepSeek efficiently.

1. Lab Preparation and Environment Setup
To begin, we accessed the Huawei Cloud console using credentials provided through IAM (Identity and Access Management). The lab environment was initialized using Lab Desktop and verified through a browser refresh.

1.1 Creating an Elastic Cloud Server (ECS)
We started by navigating to the Elastic Cloud Server (ECS) section on Huawei Cloud and selecting the “Buy ECS” option. Here, we chose:

Billing Mode: Pay-per-use

Region: AP-Singapore

Architecture: x86

Flavor: c6.xlarge.2 (4 vCPUs, 8 GiB RAM)

Image: CentOS 8.2 64-bit (40 GiB system disk)

The network configuration retained default settings for VPC and security groups. For public network access, an EIP (Elastic IP) with Dynamic BGP and 100 Mbps bandwidth was auto-assigned to ensure stable model downloading.

Instance Name: ecs-cent
Password: Huawei1234% (to be used for remote login)

After confirming all settings, the ECS instance was created successfully.

1.2 Logging In to the ECS
We remotely accessed the ECS via CloudShell, entered the designated password, and successfully connected to the instance’s terminal.

2. Installing Ollama
Ollama simplifies deploying and interacting with LLMs locally and supports various models like LLaMA, Mistral, and DeepSeek. In this lab, we used two methods to install Ollama:

Method 1: Direct Installation via Linux Command
We used the following command to download and install Ollama from GitHub:

bash
Copy
Edit
curl -fsSL https://ollama.com/install.sh | sh
After installation, we verified it using:

bash
Copy
Edit
ollama -v
If GitHub download failed, Method 2 was recommended.

Method 2: Download from OBS (Huawei’s Object Storage)
We downloaded Ollama’s pre-packaged version released on Feb 28, 2025 using:

bash
Copy
Edit
wget https://sandbox-experiment-files.obs.cn-north-4.myhuaweicloud.com/deepseek/ollama-linux-amd64.tgz
We then extracted and configured it:

bash
Copy
Edit
sudo tar -C /usr -xzf ollama-linux-amd64.tgz
sudo chmod +x /usr/bin/ollama
Next, we created a system user:

bash
Copy
Edit
sudo useradd -r -s /bin/false -m -d /usr/share/ollama ollama
Then, we created a system service:

bash
Copy
Edit
cat << EOF > /etc/systemd/system/ollama.service
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3

[Install]
WantedBy=default.target
EOF
Finally, we activated the service:

bash
Copy
Edit
sudo systemctl daemon-reload
sudo systemctl enable ollama
sudo systemctl start ollama
ollama -v
Ollama was now installed and running.

3. Running DeepSeek-R1 1.5B
We had two options to run the DeepSeek model:

Option 1: Pulling via Ollama
We used the following command to download DeepSeek-R1 1.5B directly:

bash
Copy
Edit
ollama pull deepseek-r1:1.5b
To run the model:

bash
Copy
Edit
ollama run deepseek-r1:1.5b
In case of a slow or interrupted download, Ollama supports resumable downloads.

Option 2: Downloading Model from OBS
For a faster and more reliable download (version released on Feb 14, 2025), we used:

bash
Copy
Edit
wget https://sandbox-experiment-files.obs.cn-north-4.myhuaweicloud.com/deepseek/ollama_deepseek_r1_1.5b.tar.gz
sudo tar -C /usr/share/ollama/.ollama/models -xzf ollama_deepseek_r1_1.5b.tar.gz
We then organized the model files:

bash
Copy
Edit
cd /usr/share/ollama/.ollama/models
mv ./deepseek/sha256* ./blobs
mkdir -p ./manifests/registry.ollama.ai/library/deepseek-r1
mv ./deepseek/1.5b ./manifests/registry.ollama.ai/library/deepseek-r1
rm -rf deepseek/
To verify model availability:

bash
Copy
Edit
ollama list
And to run:

bash
Copy
Edit
ollama run deepseek-r1:1.5b
At this point, we could successfully interact with DeepSeek via terminal prompts. If output accuracy was poor, the recommendation was to terminate the session using /bye and restart.


![WhatsApp Image 2025-07-28 at 08 12 51_ec232913](https://github.com/user-attachments/assets/3b2ce6b8-a5d2-4ef7-8427-f6294f917424)

![WhatsApp Image 2025-07-28 at 08 12 48_59531241](https://github.com/user-attachments/assets/55e7dfa4-bcf9-4087-ba50-4855cc4ffc6f)

![WhatsApp Image 2025-07-28 at 08 12 47_01f76701](https://github.com/user-attachments/assets/6e2a5930-1bdb-47ac-baf2-1b0837de57e0)




