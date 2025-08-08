###### VPC SETUP ####
Step -1 : Create a VPC 

VPC : components list 
----------
VPC - 1 
Subnets - 3 (2 public 1 private subnet )
Route tables - 2
IGW - 1 
SG - 3 
EIP - 3 
NAT -1 
EC2 - 3 
CIDR creation : CIDR block range is IP address range written in block like below 
10.0.0.0.0/16  this means 32-16=16 so 2^16=65536 this much ips will allocate to this cidr based on this we can divide subnets ranges. Here in this setup we are taking 10.0.0.0.0/16 so my total IP are 65536 Ip
----------
How to create a VPC ?
Aws console ===> VPC service ===> create V
<img width="901" height="608" alt="Screenshot from 2025-07-22 18-45-41" src="https://github.com/user-attachments/assets/3acf9a48-bb7c-42cf-84ea-ddb9d8b15e98" />

Tenancy : should be default means shared hardware ec2 runs on it 
Dedicated means : hardware dedicated to you most expensive 

1Create Subnets 
Here in this setup we are taking 3 subnets 1 private subnet (node_exporter) 2 public subnets (grafana, prometheus )

Cidr range for this subnets im taking 10.0.0.0/27=32
<img width="1256" height="354" alt="Screenshot from 2025-07-22 18-59-15" src="https://github.com/user-attachments/assets/e9ed31f8-0087-4d2c-b783-4cd923d8bb62" />

2.Create a IGW (InternetGateway)

IGW : used for allowing the internet to the subnets in the vpc 
<img width="874" height="618" alt="Screenshot from 2025-07-22 19-00-57" src="https://github.com/user-attachments/assets/3a24b812-c626-4039-9fc7-e3b73bb14bbd" />

After creating the IGW go to IGW ===> actions ===> Attach to the vpc 

3. Create a Routing Table
<img width="1268" height="559" alt="Screenshot from 2025-07-22 19-33-56" src="https://github.com/user-attachments/assets/35e1e416-369f-4ffa-9417-5c17bca3ba44" />

 go to routetable ===> create ===> routetable name select the vpc 
Create 
4.security Groups 
Inbound rules : which is used for access the ec2 services and protocols 
Here im taking inbound rules : 
Ssh — 22 
Http – 80(for basic authentication )
Prometheus - 9090 
Node_exporter - 9091
Grafana - 3000
<img width="1017" height="533" alt="Screenshot from 2025-07-22 19-37-28" src="https://github.com/user-attachments/assets/4ef7a493-b031-4cde-8d7c-cc008835824f" />
5. Create Elastic IP 
EIP used for a stable IP address to a Ec2
Create eip and associate it to the respective Ec2

6 Create NAT 

For accessing the internet to the private subnet’s ec2 
It should be in the public subnet and it can be attached to the private routetable 
<img width="1116" height="571" alt="Screenshot from 2025-07-22 20-05-10" src="https://github.com/user-attachments/assets/e4afb35a-338b-489d-9e6f-2be9e9231c52" />












