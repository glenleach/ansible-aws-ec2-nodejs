# DevOps Bootcamp Demo Project: Ansible AWS EC2 Node.js

## Overview
This project demonstrates how to use Ansible to provision an AWS EC2 instance and deploy a Node.js application. It is designed as a hands-on exercise for DevOps Bootcamp participants to learn infrastructure automation, configuration management, and application deployment using industry-standard tools.

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

4. **Set up an AWS EC2 instance running Ubuntu:**
   - Launch an EC2 instance with the Ubuntu operating system using the AWS Management Console, AWS CLI, or an Ansible playbook.

5. **Update inventory:**
   - After creating the EC2 instance, update the `hosts` file with the instance's public IP address or public DNS hostname. This file is referenced by `ansible.cfg`.

6. **Verify Node.js application tarball:**
   - Ensure `nodejs-app-1.0.0.tgz` is present in the project directory. This file will be copied to the EC2 instance during deployment.

## Usage
### 1. Provision an EC2 Instance
Run the Ansible playbook to create an EC2 instance:
```bash
ansible-playbook playbooks/provision-ec2.yml
```

### 2. Deploy the Node.js Application
After the instance is running and inventory is updated, deploy the app:
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
