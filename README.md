# Jenkins Installation Guide on EC2 Instance (Without Docker)

## Prerequisites
1. **Amazon EC2 Instance** (Ubuntu or Amazon Linux 2 recommended).
2. **Security Group** with necessary inbound and outbound rules.
3. **SSH Key** for accessing the EC2 instance.

---

## Step 1: Launch an EC2 Instance
1. Log in to AWS Management Console.
2. Navigate to the **EC2 Dashboard** and launch a new instance.
3. Choose an **Ubuntu 20.04+** or **Amazon Linux 2** AMI.
4. Select an instance type (e.g., t2.micro for testing).
5. Create or configure a **security group** with the following inbound rules:
   - **SSH (port 22)** for remote access.
   - **HTTP (port 80)** for Jenkins web interface.
   - **Custom TCP Rule (port 8080)** for Jenkins.
6. Attach an SSH key pair to the instance.

---

## Step 2: SSH into the EC2 Instance
Once the EC2 instance is running, SSH into it:
```bash

ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>
Step 3: Install Jenkins
3.1 Update System Packages
bash
Copy
Edit
sudo apt update
sudo apt upgrade -y
3.2 Add Jenkins Repository and Import Key
bash
Copy
Edit
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
3.3 Install Java
Jenkins requires Java. Install the OpenJDK package:

bash
Copy
Edit
sudo apt install openjdk-11-jre -y
Verify Java installation:

bash
Copy
Edit
java -version
3.4 Install Jenkins
Update the package index and install Jenkins:

bash
Copy
Edit
sudo apt update
sudo apt install jenkins -y
Start and enable the Jenkins service:

bash
Copy
Edit
sudo systemctl start jenkins
sudo systemctl enable jenkins
Check the Jenkins service status:

bash
Copy
Edit
sudo systemctl status jenkins
Step 4: Configure Jenkins
Access Jenkins by opening the browser and navigating to:

text
Copy
Edit
http://<your-ec2-public-ip>:8080
Retrieve the initial administrator password:

bash
Copy
Edit
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Copy the password, paste it into the Jenkins setup page, and follow the on-screen instructions:

Install suggested plugins.
Create the first admin user.
Configure Jenkins settings.
Step 5: Configure Security Group
Ensure the EC2 security group allows:

SSH (port 22): For remote access.
HTTP (port 80): For Jenkins web interface.
Custom TCP Rule (port 8080): For Jenkins.
Step 6: Install Docker (Optional)
If you need Docker installed on the same EC2 instance:

Update the package index:

bash
Copy
Edit
sudo apt update
Install prerequisites:

bash
Copy
Edit
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
Add Docker’s official GPG key and repository:

bash
Copy
Edit
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
Install Docker:

bash
Copy
Edit
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
Add your user to the docker group (optional for non-root usage):

bash
Copy
Edit
sudo usermod -aG docker $USER
Verify Docker installation:

bash
Copy
Edit
docker --version
Additional Commands for Managing Jenkins and Docker
Jenkins
Restart Jenkins:

bash
Copy
Edit
sudo systemctl restart jenkins
Stop Jenkins:

bash
Copy
Edit
sudo systemctl stop jenkins
Start Jenkins:

bash
Copy
Edit
sudo systemctl start jenkins
Docker
Start Docker Service:

bash
Copy
Edit
sudo systemctl start docker
Stop Docker Service:

bash
Copy
Edit
sudo systemctl stop docker
Enable Docker to Start on Boot:

bash
Copy
Edit
sudo systemctl enable docker
Verify Docker is Running:

bash
Copy
Edit
sudo systemctl status docker
