# Demo App – DevOps Bootcamp Training (TechWorld with Nana)

## Overview
This project demonstrates how to use Ansible to provision an AWS EC2 instance and deploy a Node.js application. It is designed as a hands-on exercise for DevOps Bootcamp participants to learn infrastructure automation, configuration management, and application deployment using industry-standard tools.

**Note:** Additional coding and files have been added to this project to improve automation and resilience beyond the base bootcamp material.

## Learning Objectives
- Understand the basics of Ansible and its role in DevOps
- Automate the provisioning of AWS EC2 instances
- Deploy and manage a Node.js application on a remote server
- Apply best practices for infrastructure as code (IaC)

## Prerequisites
- AWS account with programmatic access (Access Key ID and Secret Access Key)
- Ansible installed on your local machine
- Python and `boto3` library installed (for AWS modules)
- SSH key pair for accessing EC2 instances
- Node.js application source code (provided in this repository)

## Project Structure
```
ansible-aws-ec2-nodejs/
├── .gitignore           # Git ignore rules
├── ansible.cfg          # Ansible configuration file
├── deploy-node.yaml     # Ansible playbook to deploy Node.js app
├── provision-ec2.yaml   # Ansible playbook to provision EC2 and security group
├── vars.yaml            # Variables for provisioning (region, key, instance type)
├── hosts                # Ansible inventory file
├── nodejs-app-1.0.0.tgz # Node.js application tarball
└── Readme.md            # Project documentation
```

## Setup Instructions
1. **Clone the repository:**
   ```bash
   git clone <repo-url>
   cd ansible-aws-ec2-nodejs
   ```

2. **Configure AWS credentials:**
   - Set your AWS credentials as environment variables (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) or ensure they are available to Ansible via your AWS CLI configuration.

3. **Install dependencies:**
   - Ensure Ansible and Python dependencies are installed:
     ```bash
     pip install boto3
     ansible --version
     ```

4. **Edit variables for provisioning:**
   - Open `vars.yaml` and update the following variables to match your AWS setup:
     - `aws_region`: Your AWS region (e.g., us-east-1)
     - `instance_type`: EC2 instance type (e.g., t2.micro for free tier)
     - `key_name`: Your AWS EC2 key pair name

5. **Provision an AWS EC2 instance and security group:**
   - Run the following command to create a security group, launch an Ubuntu EC2 instance, and update the hosts file automatically:
     ```bash
     ansible-playbook provision-ec2.yaml
     ```

6. **Verify Node.js application tarball:**
   - Ensure `nodejs-app-1.0.0.tgz` is present in the project directory. This file will be copied to the EC2 instance during deployment.

## Usage
### 1. Provision an EC2 Instance and Security Group
Run the Ansible playbook to create a security group, launch an EC2 instance, and update the hosts file:
```bash
ansible-playbook provision-ec2.yaml
```

### 2. Deploy the Node.js Application
After the instance is running and the hosts file is updated, deploy the app:
```bash
ansible-playbook deploy-node.yaml
```

#### What this playbook does
- Copies the Node.js application tar file from your local machine to the EC2 instance.
- Unpacks the tar file on the EC2 server.
- Installs Python3 (required for Ansible and some app dependencies).
- Creates a dedicated user and group with limited privileges (not root) to run the Node.js application for improved security.
- Sets the correct permissions so the new user can run the application.
- Starts the Node.js application as the new user.
- Prints out the status of the application to verify it is running successfully.

### 3. Access the Application
- Find the public IP of your EC2 instance and open it in your browser:
  ```
  http://<EC2_PUBLIC_IP>:3000
  ```


## Additional Resources
- [Ansible Documentation](https://docs.ansible.com/)
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [Node.js Documentation](https://nodejs.org/en/docs/)

## License
This project is for educational purposes as part of the DevOps Bootcamp.

---

Project inspired by and created for the Techworld DevOps Bootcamp with Nana.
