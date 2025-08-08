###Ec2 setup for VPC ###

Here we are creating 3 ec2 instances 1 will be in private subnet 2 in public subnets 
Name - promethues_instance
AMI - Ubuntu 22.04 LTS
instance type : t2.medium 
Keypair : create a keypair if not exist 
Vpc : select your created VPC
Subnets : select your subnet 
Auto assign public IP: here we are using EIP to the instances so no need to Enable the auto assign public ip 
Firewall security groups : choose existing SG then select the sg which we created if the
instance is public select pub sg if the instance is private select private sg
Storage : 
Im adding 20Gb of storage
<img width="1168" height="656" alt="Screenshot from 2025-07-23 10-52-10" src="https://github.com/user-attachments/assets/765b1f3b-f7c9-450e-9879-9d543adcb157" />
<img width="1168" height="656" alt="Screenshot from 2025-07-23 10-54-09" src="https://github.com/user-attachments/assets/ecc89036-3ea1-4fab-8d5a-1921477d102d" />
<img width="1168" height="656" alt="Screenshot from 2025-07-23 10-56-03" src="https://github.com/user-attachments/assets/6f4c4ca8-223f-457c-808c-500e6e3359b1" />
