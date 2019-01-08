# requirements

- AWS Account
- IAM User (Secret & Access Key)
- Ansible
- boto(3), botocore

I assume there's a **IAM-User** ready with **Secret-Key** & **Secret-Access-Key** and this user has *rights* to do ec2 related tasks.


There's also a **VPC** and a **subnet** configured - a **key-pair** is ready also.


For testing purposes we're spinning up those instances and will access them via their **public ip** - therefore the security-group has to be configured properly.


Ansible is installed, also are boto, boto3 and botocore.


The boto config under **~/.boto** is configured with IAM Users **Secret-Key** & **Secret-Access-Key**

## example playbook run

    ansible-playbook playbooks/aws/kubernetescluster.yml --extra-vars "subnet=subnet-4fXXXc02 zone=eu-central-1c vpc=vpc-a5aXXXce key=Ansible
