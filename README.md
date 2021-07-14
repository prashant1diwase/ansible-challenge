### Ansible Technical Challenge

## Scope of the test:

- This solution will deploy two virtual machines on the AWS cloud
- It will deploy a minimum of two Ubuntu virtual machines in AWS VPC
- Two virtual machines will have httpd installed and each host a different html file
- Virtual Machine #1 hosts html file with the bits-rain game and Virtual Machine #2 hosts the html file with the car game.
  You can add more by editing the html_pages dictionary variable in group_vars/all.yml
- Both Virtual machines will be deployed together along with the associated software installation and configuration.
- Required VPCs, network security groups, SSH key pairs etc has included as part of infrastructure deployment.
- It will Add some basic configuration management to the systems  deploy (e.g. chrony, timezone, useful packages in a Linux SOE).
  Additionally secured the webservices with SSL. Self-signed certs are included as well.
- It will use a dynamic inventory to get host lists under group tag_html_html_files.

Essentially the way it can be tested is by doing the following:
```
ansible-lint site.yml
ansible-galaxy collection install -r collections/requirements.yml
ansible-playbook -i inventory/aws_ec2.yml site.yml --vault-password-file ~/accesskeyvaultfile.vault
```

## Prerequisites

# Software Prerequsites on local machine

git >=2.27.0
ansible >= 2.9.6
python >= 2.7.18

For eg., On Ubuntu Local machine, execute
```
sudo apt-get update 
sudo apt-get install software-properties-common 
sudo apt-add-repository ppa:ansible/ansible 
sudo apt-get update 
sudo apt-get install ansible
sudo apt install ansible-lint
sudo apt install python
sudo apt install python3-pip
pip install boto boto3 ansible
```

# AWS Account Pre-requisite 

- Setup AWS account, IAM user 
- Get aws_access_key, aws_secret_key.
  https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html

## Download Code

Clone below git repository to your local machine by running command:
```
git clone git@github.com:prashant1diwase/ansible-challenge.git
```

## Setup

# Configure password vault

- Create vault file by running command as mentioned below 
    echo "myvaultfile" > ~/<vault_file_name>.vault
- Replace your aws_access_key, aws_secret_key retried from AWS account pre-requisite
    ansible-vault encrypt_string --vault-password-file  ~/<vault_file_name>.vault '<your_aws_access_key>' --name 'aws_access_key'
    ansible-vault encrypt_string --vault-password-file  ~/<vault_file_name>.vault '<your_aws_secret_key>' --name 'aws_secret_key'

For eg.,
```
$  echo "myvaultfile" > ~/accesskeyvaultfile.vault
$  ansible-vault encrypt_string --vault-password-file  ~/accesskeyvaultfile.vault 'AKIAYMVTBVMSN34227KU' --name 'aws_access_key'
$  ansible-vault encrypt_string --vault-password-file  ~/techchallenge.vault 'SNJixhyW+wwVsRWYzttOdt+O9gxU2YTfHghRbwxw' --name 'aws_secret_key'
```
- Capture output of above commands.

# Add encrypted passwords at below locations

Add access_key and secret key by using output captured in above step at given locations. 

- Edit file group_vars/all.yml to add variables:
   aws_access_key
   aws_secret_key

- Edit file inventory/aws_ec2.yml to add variables:
   aws_access_key
   aws_secret_key

## Usage

- Once pre-requisite are completed, Run ansible playbook to deploy the solution:
```
ansible-lint site.yml
ansible-galaxy collection install -r collections/requirements.yml
ansible-playbook -i inventory/aws_ec2.yml site.yml --vault-password-file ~/accesskeyvaultfile.vault
```
- You can access the html pages by appending html file name (given in html_pages dictionary) to public dns of AWS VM.
  For eg.,
  http://ec2-18-118-140-163.us-east-2.compute.amazonaws.com/mini_car_game.html

Let me know if you have any questions on this solution.