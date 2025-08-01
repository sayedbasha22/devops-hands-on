
 🛠️ Git + GitHub Integration from EC2

This note documents how I set up SSH-based GitHub access from an EC2 instance, generated SSH keys, added them to GitHub, and pushed code directly from EC2.

---

## 🔑 Step 1: Create a ec2 instance 
---> Give file permission for your pem key for logging to the ec2 
  ex : sudo chmod 400 yourkey.pem 
----> now ssh to the ec2 (make sure your ec2 security allow the 22 port if your using VPC make sure your route table allowing the igw and also check the correct subnets )
----> now ssh into the ec2 using ssh command 
ex : ssh -i /home/ubuntu/yourkey.pem ubuntu@ipaddress 

## 🔑 Step 2: SSH Key Generation on EC2
---> this command generate the ssh keys (private and public keys) after hitting this command you can cd .ssh and check the file content in the .ssh directory and cat the keys 
`
ssh-keygen -t ed25519 -C "sayedsayedbashaxxxxx@gmail.com"
 ubuntu@ip: cd .ssh
ubuntu@ip-:~/.ssh$ ls
authorized_keys  id_ed25519  id_ed25519.pub

## 🔑 Step 3: Add the public key into the github
Go to GitHub → Settings → SSH and GPG keys

Click New SSH key

Title: EC2 Git Key

Key type: Authentication Key

Paste the key and save

and now run this command 
ssh -T git@github.com

Hi sayedbasha22! You've successfully authenticated, but GitHub does not provide shell access.


## 🔑 Step 4: Git Configuration on EC2
git config --global user.name "sayedbasha22"
git config --global user.email "sayedsayedbasha22@gmail.com"

## 🔑 Step 5:Repository Setup
mkdir ~/git
cd ~/git
git clone git@github.com:sayedbasha22/practice-repo.git
cd practice-repo

## 🔑 Step 6: Git Branch Workflow 
# Create a new branch
git checkout -b test-branch

#### Create or modify a file
vim test_file

# Stage and commit the changes
git add test_file
git commit -m "Added test_file in test-branch"

# Push the branch to GitHub
git push origin test-branch


#  Invite Collaborators to Your Private Repo
Go to your GitHub repo:
https://github.com/sayedbasha22/practice-repo

Click Settings → Collaborators or Manage access

Click Invite a collaborator

Enter the GitHub username or email and click Add

They’ll receive an invite and get access after accepting.
