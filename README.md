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
├── aws_ec2.yaml         # Dynamic inventory configuration for AWS EC2
├── deploy-node.yaml     # Ansible playbook to deploy Node.js app
├── provision-ec2.yaml   # Ansible playbook to provision EC2 and security group
├── vars.yaml            # Variables for provisioning (region, key, instance type)
├── hosts                # (Optional) Ansible static inventory file (see below)
├── nodejs-app-1.0.0.tgz # Node.js application tarball
└── Readme.md            # Project documentation
```

## Inventory Options: Dynamic vs. Static

### Dynamic Inventory (Recommended)
- By default, this project uses Ansible's dynamic inventory feature via `aws_ec2.yaml` to automatically discover EC2 instances in AWS.
- The `ansible.cfg` file is set to use `aws_ec2.yaml` as the default inventory, so you do not need to specify `-i` on the command line.
- All host discovery and connection details are managed by your dynamic inventory and `ansible.cfg` settings.
- The static `hosts` file can be left blank or deleted if you are only using dynamic inventory.

### Using the Static Hosts File (Manual Option)
- If you wish to use the `hosts` file instead of dynamic inventory, you must manually add your instance's public IP or DNS name to the file. Example:
  ```
  [nodejs]
  1.2.3.4 ansible_user=ubuntu ansible_ssh_private_key_file=/mnt/c/Users/glenl/Ansible.pem
  ```
- If you use the static `hosts` file, ensure you run your playbook with the hosts file as inventory, e.g.:
  ```
  ansible-playbook -i hosts deploy-node.yaml
  ```
- Do not use the dynamic inventory (`aws_ec2.yaml`) at the same time as the static hosts file.

## ansible.cfg Settings
- The following settings are configured globally in `ansible.cfg`:
  ```ini
  [defaults]
  inventory = aws_ec2.yaml
  private_key_file = /mnt/c/Users/glenl/Ansible.pem
  ansible_user = ubuntu
  host_key_checking = False
  ansible_python_interpreter = /usr/bin/python3.9
  ```
- These settings ensure Ansible uses the correct SSH key, user, and Python interpreter for all hosts discovered by dynamic inventory.

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
     - `aws_region`: Your AWS region (e.g., eu-west-2)
     - `instance_type`: EC2 instance type (e.g., t2.micro for free tier)
     - `key_name`: Your AWS EC2 key pair name
5. **Provision an AWS EC2 instance and security group:**
   - Run the following command to create a security group and launch an Ubuntu EC2 instance:
     ```bash
     ansible-playbook provision-ec2.yaml
     ```
   - The playbook no longer updates the static hosts file. All host discovery is now dynamic unless you choose to use the hosts file manually (see above).
6. **Verify Node.js application tarball:**
   - Ensure `nodejs-app-1.0.0.tgz` is present in the project directory. This file will be copied to the EC2 instance during deployment.

## Usage
### 1. Provision an EC2 Instance and Security Group
Run the Ansible playbook to create a security group and launch an EC2 instance:
```bash
ansible-playbook provision-ec2.yaml
```

### 2. Deploy the Node.js Application
After the instance is running, deploy the app using dynamic inventory:
```bash
ansible-playbook deploy-node.yaml
```
- If you are using the static hosts file, run:
```bash
ansible-playbook -i hosts deploy-node.yaml
```

### What this playbook does
- Copies the Node.js application tar file from your local machine to the EC2 instance.
- Unpacks the tar file on the EC2 server.
- Installs Python3 (required for Ansible and some app dependencies).
- Creates a dedicated user and group with limited privileges (not root) to run the Node.js application for improved security.
- Sets the correct permissions so the new user can run the application.
- Starts the Node.js application as the new user.
- Prints out the status of the application to verify it is running successfully.

### 3. Access the Application
- Find the public IP of your EC2 instance (the dynamic inventory will automatically target it) and open it in your browser:
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
