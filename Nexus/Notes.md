
## What is Nexus 
Nexus is a central artifact repository used in CI/CD pipelines to store and manage build files like .jar, .war, Docker images, etc.
- Stores & serves build artifacts
- Caches dependencies (Maven, Docker, etc.)
- Integrates with CI/CD tools (Jenkins, GitLab)

Step 1: Install Java (Skip if already installed)
Nexus requires Java 17+ (Java 21 is fully supported from Nexus 3.68+).
sudo apt update
sudo apt install openjdk-21-jdk -y
Check version:
java -version

Step 2: Download and Extract Nexus
We’ll use /opt to store Nexus because it's a standard directory for optional or third-party software in Linux.
Run the Below commands 
cd /opt
sudo wget https://download.sonatype.com/nexus/3/nexus-3.77.2-02-unix.tar.gz

Extract Nexus
sudo tar -xvzf nexus-3.77.2-02-unix.tar.gz
Rename it for easier path usage:
sudo mv nexus-3.77.2-02 nexus

Step 3: Create a Dedicated User & Group
Running services as root is bad practice. So we’ll create a special nexus user with restricted access.
Create a Group
sudo groupadd nexus 
Create a user 
sudo useradd -r -g nexus -d /opt/nexus -s /bin/bash nexus
-r	Create system user (non-login)
-g nexus	Assign to group nexus
-d /opt/nexus	Set home directory to /opt/nexus
-s /bin/bash	Assign bash shell

Step 4: Set Permissions
sudo chown -R nexus:nexus /opt/nexus
This ensures only the nexus user can modify files in /opt/nexus

Step  5: Run Nexus as a Background Service (systemd)
What does .rc mean?
.rc stands for “Run Commands” or “Resource Configuration”.
It's a Linux/Unix naming convention used for configuration files that are run when a program starts.

What is /opt/nexus/bin/nexus.rc?
This is a configuration file used by the Nexus start/stop scripts (inside /opt/nexus/bin/).
Nexus will read this file before starting, and it checks:
now runt the below commands 
sudo nano /opt/nexus/bin/nexus.rc

run_as_user="nexus"
control + x then y save it 

Now Create a service file 
Create a systemd service file
sudo nano /etc/systemd/system/nexus.service
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
User=nexus
Group=nexus
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
Restart=on-abort

[Install]
WantedBy=multi-user.target
control+x then y save it 

NOTE : please check your nexus path using below comamnd 
ls -l /opt 

What does WantedBy=multi-user.target mean?
WantedBy means: "When should this service start?"
multi-user.target is a systemd boot target that means:
The system has finished booting
Networking is up
User logins are available
It’s ready for services to run
Systemd, I want nexus.service to start automatically when the system reaches multi-user mode

Step 6:  Reload systemd and Start the Nexus Service
Reload systemd to recognize the new service
sudo systemctl daemon-reload
daemon-reload tells systemd to re-read all service files.

Start the Nexus service 
sudo systemctl start nexus
This runs the command: /opt/nexus/bin/nexus start under the nexus user

Now you can access the nexus in to website 
http://ip:8081
here you can change the passowrd first get the default password from 
sudo cat /opt/sonatype-work/nexus3/admin.password
5dc41000-7d82-4a39-b90b-313d7

<img width="1291" height="695" alt="Screenshot from 2025-08-05 17-35-30" src="https://github.com/user-attachments/assets/7175539e-49a5-4b03-88e8-7d76b89476b1" />


