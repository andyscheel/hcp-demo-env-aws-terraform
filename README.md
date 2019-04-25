### Overview

This project is a work-in-progress.

### Pre-requisites

The following installed locally:

 - python3
 - ssh client
 - ssh key pair (ssh-keygen -t rsa)
 - terraform (https://learn.hashicorp.com/terraform/getting-started/install.html)

Script has only been tested on Linux machine

### Instructions

#### Setup AWS Env and Install BlueData

```
# ensure you have setup your aws credentials
vi ~/.aws/credentials

git clone https://github.com/snowch-bluedata/bluedata-demo-env-aws-terraform
cd bluedata-demo-env-aws-terraform

cp ./bluedata_demo.tfvars_template to ./bluedata_demo.tfvars

# edit to reflect your requirements
vi ./bluedata_demo.tfvars 

# initialise terraform
terraform init

# deploy BlueData to AWS
terraform apply -var-file=bluedata_demo.tfvars

# or to automatically set client_cidr_block from the client's IP
terraform apply -var-file=bluedata_demo.tfvars -var="client_cidr_block=$(curl -s http://ifconfig.me/ip)/32" -var="continue_on_precheck_fail=\"true\""

# inspect the output for errors if no errors you should see a configuration URL variable from the command below 'display_configuration_url' 
terraform output display_configuration_url
```

In the BlueData configuration screen, insert the gateway private ip address (terraform output gateway_private_ip)

Add workers and gateway
run `terraform output` to see all variables

 1. Add workers private ip 
 2. Add gateway private ip and dns
 3. Download contoller ssh key (see variable retrive_controller_ssh for command to run locally)
 4. Set ssh key

### destroy environment when finished

```
terraform destroy -var-file=bluedata_demo.tfvars -var="client_cidr_block=$(curl -s http://ifconfig.me/ip)/32" var="continue_on_precheck_fail=\"true\""
```

### TODO - shutdown ec2 instances when not in use

See https://groups.google.com/forum/#!topic/terraform-tool/hEESOVOgL_Q

