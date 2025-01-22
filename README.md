# Jenkins Installation Guide on EC2 Instance

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

<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">


---

## Step 2: SSH into the EC2 Instance
Once the EC2 instance is running, SSH into it:

ssh -i /path/to/your-key.pem ubuntu@<your-ec2-public-ip>
```

---

## Step 3: Install Jenkins
### 3.1 Update System Packages
```bash
sudo apt update
sudo apt upgrade -y
```

### 3.2 Add Jenkins Repository and Import Key
```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### 3.3 Install Java
Jenkins requires Java. Install the OpenJDK package:
```bash
sudo apt install openjdk-11-jre -y
```

Verify Java installation:
```bash
java -version
```

### 3.4 Install Jenkins
1. Update the package index and install Jenkins:
   ```bash
   sudo apt update
   sudo apt install jenkins -y
   ```

2. Start and enable the Jenkins service:
   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```

3. Check the Jenkins service status:
   ```bash
   sudo systemctl status jenkins
   ```

---

## Step 4 Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing `All Traffic` to your EC2 instance
      1. Delete the inbound traffic rule for your instance
      2. Edit the inbound traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for the Jenkins to be restarted.

---
## Step 5 -  Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```
