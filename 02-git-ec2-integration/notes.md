### 🛠️ Git + GitHub Integration from EC2

This guide documents how to set up SSH-based GitHub access from an EC2 instance, including key generation, adding it to GitHub, configuring Git, and pushing code.

🔑 **Step 1: Launch an EC2 Instance**

* Launch an EC2 instance using Ubuntu in your default VPC.
* Ensure that port 22 is allowed in the security group.
* Ensure that your subnet is public and the route table has 0.0.0.0/0 pointing to the internet gateway.

SSH into the instance:

```
chmod 400 yourkey.pem
ssh -i /path/to/yourkey.pem ubuntu@<your-ec2-public-ip>
```

---

🔑 **Step 2: Generate SSH Key on EC2**

```
ssh-keygen -t ed25519 -C "sayedsayedbashaxxxxx@gmail.com"
```

* Press Enter for all prompts.
* This creates two files in `~/.ssh`: `id_ed25519` (private) and `id_ed25519.pub` (public).

Check the keys:

```
cd ~/.ssh
ls
cat id_ed25519.pub
```

---

🔑 **Step 3: Add the Public Key to GitHub**

* Go to GitHub → Settings → SSH and GPG Keys
* Click **New SSH key**
* Title: `EC2 Git Key`
* Paste your public key (`id_ed25519.pub`)
* Click **Add SSH key**

Verify the connection:

```
ssh -T git@github.com
```

Expected output:

```
Hi sayedbasha22! You've successfully authenticated, but GitHub does not provide shell access.
```

---

🔑 **Step 4: Configure Git on EC2**

```
git config --global user.name "sayedbasha22"
git config --global user.email "sayedsayedbasha22@gmail.com"
```

---

🔑 **Step 5: Clone GitHub Repository**

```
mkdir ~/git
cd ~/git
git clone git@github.com:sayedbasha22/practice-repo.git
cd practice-repo
```

---

🔑 **Step 6: Git Branch Workflow**

Create a new branch:

```
git checkout -b test-branch
```

Create or modify a file:

```
vim test_file
```

Stage and commit the changes:

```
git add test_file
git commit -m "Added test_file in test-branch"
```

Push the branch to GitHub:

```
git push origin test-branch
```

---

🔑 **Step 7: Invite Collaborators on GitHub**

* Go to your repo: [https://github.com/sayedbasha22/practice-repo](https://github.com/sayedbasha22/practice-repo)
* Click **Settings → Collaborators** or **Manage access**
* Click **Invite a collaborator**
* Enter their GitHub username or email and click **Add**

They’ll receive an invite and get access after accepting.

---

✅ You’re now fully set up to push code from EC2 to GitHub securely using SSH!
