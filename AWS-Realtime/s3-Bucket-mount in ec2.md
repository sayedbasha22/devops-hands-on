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


Step 0: Prerequisites
EC2 instance running (Ubuntu/Debian-based in this example).

S3 bucket already created.

Internet connectivity (for installing packages).

Step 1: Install AWS CLI
AWS CLI is needed if you want to interact with S3 from terminal (optional for s3fs, but useful).

sudo apt update
sudo apt install awscli -y
Explanation:

sudo apt update → Updates package lists.

sudo apt install awscli -y → Installs AWS CLI without confirmation.

Test:


aws s3 ls
Should list buckets if you configure credentials or use IAM role.

Step 2: Create IAM Role for EC2
Go to AWS IAM → Roles → Create Role

Select trusted entity: AWS service → EC2

Attach policy: AmazonS3FullAccess (or a custom policy for limited access)

Set trust relationship (important!):


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "ec2.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
Why trust relationship?
It allows EC2 instances to assume this role and get temporary credentials to access AWS resources (S3 in this case).

Attach this IAM role to your EC2 instance (can be done at launch or later via Actions → Security → Modify IAM Role).

Step 3: Install s3fs

sudo apt install s3fs -y
s3fs lets Linux mount S3 as a local filesystem.

Step 4: Create Mount Directory

sudo mkdir /mnt/mybucket
/mnt/mybucket → where your S3 bucket will appear.

sudo → required to create directory in /mnt.

Step 5: Mount S3 Bucket

sudo s3fs my-practice-s3-deploy-bucket /mnt/mybucket -o iam_role=auto -o allow_other
my-practice-s3-deploy-bucket → your bucket name.

/mnt/mybucket → mount point.

-o iam_role=auto → uses EC2 IAM role (temporary credentials automatically).

-o allow_other → allow all users to read/write.

Test:


ls /mnt/mybucket
Should show files in the bucket (if any).

Step 6: Auto-Mount on Reboot
Edit /etc/fstab:


sudo nano /etc/fstab
Add:

s3fs#my-practice-s3-deploy-bucket /mnt/mybucket fuse _netdev,iam_role=auto,allow_other,use_path_request_style,url=https://s3.amazonaws.com 0 0
_netdev → mount after network is up

use_path_request_style → required for certain S3 regions

url → endpoint URL

Then mount all filesystems:

sudo mount -a
Step 7: Verify Mount

df -h
You may see:


s3fs 4.0G 0 4.0G 0% /mnt/mybucket
Note: The 4GB is default virtual size, not the actual bucket size. S3 has effectively unlimited storage.

Step 8: Optional – AWS CLI Access Without s3fs
If you want to interact directly:


aws s3 ls s3://my-practice-s3-deploy-bucket
aws s3 cp test.txt s3://my-practice-s3-deploy-bucket/

