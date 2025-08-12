Mounting S3 Bucket on EC2 Instance
 Steps:
1. Install s3fs

sudo apt update && sudo apt install s3fs -y

2. Create mount directory

sudo mkdir /mnt/mybucket

3. Attach an IAM Role to EC2 instance with S3 permissions
<img width="1366" height="768" alt="Screenshot from 2025-07-28 21-38-31" src="https://github.com/user-attachments/assets/815465c1-175e-47d3-9cea-ac50820fb8ad" />
How to Mount s3 into ec2 ?
- - - - ->mounted the s3 in ec2 instance 
Steps : 
 We need to install the s3fs 
===> sudo apt update && sudo apt install s3fs -y
===> create a dir 
      Mkdir /mnt/mybucket 
Attach the IAM role to the ec2 and run the below command 

ubuntu@ip-172-31-46-126:~$ sudo s3fs my-practice-s3-deploy-bucket /mnt/mybucket -0 iam_role=auto -o allow_other

ubuntu@ip-172-31-46-126:~$ sudo nano /etc/fstab
Added this 
s3fs#your-bucket-name /mnt/mybucket fuse _netdev,iam_role=auto,allow_other,use_path_request_style,url=https://s3.<region>.amazonaws.com 0 0

 sudo mount -a
ubuntu@ip-172-31-46-126:~$ df -h
Filesystem       Size  Used Avail Use% Mounted on
/dev/root        6.8G  2.6G  4.2G  38% /
tmpfs            458M     0  458M   0% /dev/shm
tmpfs            183M  896K  182M   1% /run
tmpfs            5.0M     0  5.0M   0% /run/lock
efivarfs         128K  3.6K  120K   3% /sys/firmware/efi/efivars
/dev/nvme0n1p16  881M   86M  734M  11% /boot
/dev/nvme0n1p15  105M  6.2M   99M   6% /boot/efi
tmpfs             92M   12K   92M   1% /run/user/1000
s3fs             4.0G     0  4.0G   0% /mnt/mybucket
