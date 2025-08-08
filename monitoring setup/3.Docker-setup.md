###DOCKER SETUP###

1.Docker installation 

sudo apt update -y
sudo apt install -y docker
sudo systemctl start docker
sudo systemctl enable docker

2.Install nginx  server for basic auth 
sudo apt install -y nginx apache2-utils
sudo systemctl enable nginx
sudo systemctl start nginx

3.Create basic auth 
sudo htpasswd -c /etc/nginx/.htpasswd adminuser

When you hit this command it will ask username and password 

Dir structure for nginx authentication 
~/nginx-custom/
├── nginx.conf               # minimal main config
└── conf.d/
    └── default.conf         # server block for Prometheus reverse proxy

4.Inside the nginx.conf
ubuntu@ip-10-0-0-24:~/nginx-custom$ cat nginx.conf
events {}                           # Required block for Nginx to run, even if empty

http {
  include /etc/nginx/conf.d/*.conf;    # Load all server blocks from the conf.d directory
}

}



5.Inside the default.conf
ubuntu@ip-10-0-0-24:~/nginx-custom/conf.d$ cat default.conf 

server {
  listen 80;          # Listen on port 80 (HTTP)

  location / {
    proxy_pass http://promcontainer:9090;  # Forward all incoming requests to the Prometheus container (must be in same Docker network)
    auth_basic "Restricted Prometheus";    # This is the message shown in the login popup when the user tries to access the page
    auth_basic_user_file /etc/nginx/.htpasswd;   # Path to the password file containing user credentials for basic auth
  }
}

----------------------------------

Prometheus Configuration


Now create dir for promethes 
prometheus-data/
├── prometheus.yml
├── alert.rules.yml
└── alertmanager/
    └── alertmanager.yml

**1.prometheus.yml**

ubuntu@ip-10-0-0-24:~/prometheus-data$ cat prometheus.yml 
Global:                                                  # global config for promethes 
  scrape_interval: 15s        # Prometheus scraps the data every 15 sec from targets
  evaluation_interval: 15s   #every 15s it will check alertmanager file

Alerting:                        # configuring for sending alerts 
  Alertmanagers:        # list of alert manager instances 
    - static_configs:
        - targets:
            - "alertmanager:9093"      

Rule_files:                                   # list of rule files 
  - "alert.rules.yml"                   # this file inside we have alerting rules 

scrape_configs:
  - job_name: "prometheus"            # job name which is used to run promquery 

    static_configs:
      - targets: ["localhost:9090"]    # this is loaclhost  and port

        labels:
          app: "prometheus"          # this is label to tag data in prometheus query

  - job_name: "node-exporter"            # job name which is used to run promquery 
    static_configs:
      - targets: ["10.0.0.74:9100"]              # this is node-exporter ip and port
        labels:
          app: "node_exporter"               # this is label to tag data in prometheus query


**2.Alert.rules.yml** 


ubuntu@ip-10-0-0-24:~/prometheus-data$ cat alert.rules.yml
groups:
  - name: node-alerts
    rules:

      - alert: HighCPUUsage
        expr: 100 - (avg by (instance)(irate(node_cpu_seconds_total{mode="idle"}[2m])) * 100) > 80
        for: 30s
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage on instance {{ $labels.instance }}"
          description: "CPU usage is above 80% for more than 30 seconds."

      - alert: HighMemoryUsage
        expr: (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 > 80
        for: 30s
        labels:
          severity: warning
        annotations:
          summary: "High memory usage on instance {{ $labels.instance }}"
          description: "Memory usage is above 80% for more than 30 seconds."

****3. Alertmanager.yml ***
ubuntu@ip-10-0-0-24:~/prometheus-data/alertmanager$ cat alertmanager.yml 
global:
  resolve_timeout: 5m

route:
  receiver: slack-notifications

receivers:
  - name: slack-notifications
    slack_configs:
      - channel: '#prometheus-alerts'  # Change to your actual Slack channel name
        api_url: 'https://hooks.slack.com/services/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'
        send_resolved: true
        title: '{{ .CommonLabels.alertname }}'
        text: '{{ range .Alerts }}{{ .Annotations.description }}\n{{ end }}'
ubuntu@ip-10-0-0-24:~/prometheus-data/alertmanager$ 
Now config files part done,

------------------------------------
**Lets create a docker network and create containers**

Sudo docker network create prom-net

Network has been create  

Now create a docker prometheus container and alert manager container,nginx containers 
 docker run -d   --name=promcontainer - - restart=always –network=prom-net   -p 9090:9090   -v /home/ubuntu/prometheus-data:/etc/prometheus   prom/prometheus   --config.file=/etc/prometheus/prometheus.yml   --web.listen-address=:9090   --web.enable-lifecycle

**CONTAINER CREATION **
**Prometheus container**

Command explanation 
docker run -d                             # Run the container in detached mode (runs in background)
  --name=promcontainer                   # Name the container as "promcontainer"
  -p 9090:9090                        # Map port 9090 on the host to 9090 in the container (Prometheus UI)
  -v /home/ubuntu/prometheus-data:/etc/prometheus  # Mount your local config dir to container's config path
  prom/prometheus                      # Use the official Prometheus Docker image
  --config.file=/etc/prometheus/prometheus.yml     # Tell Prometheus to use this config file
  --web.listen-address=:9090             # Prometheus should listen on port 9090 inside the container
  --web.enable-lifecycle                   # Enables live reload of config using an HTTP POST request

**Create alert manager container**

sudo docker run -d \
  --name alertmanager \
  --restart=always \
  --network prom-net \
  -p 9093:9093 \
  -v /home/ubuntu/prometheus-data/alertmanager/alertmanager.yml:/etc/alertmanager/alertmanager.yml \
  prom/alertmanager

**Create an nginx container **

 sudo docker run -d  --name nginx-proxy --restart=always  --network prom-net   -p 80:80   -v ~/nginx-custom/nginx.conf:/etc/nginx/nginx.conf   -v ~/nginx-custom/conf.d:/etc/nginx/conf.d   -v ~/nginx-custom/.htpasswd:/etc/nginx/.htpasswd   nginx


Now check the containers status 
Sudo docker ps 
ubuntu@ip-10-0-0-24:~$ sudo docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED        STATUS       PORTS                                       NAMES
5dc5fb584ddb   prom/alertmanager   "/bin/alertmanager -…"   3 hours ago    Up 3 hours   0.0.0.0:9093->9093/tcp, :::9093->9093/tcp   alertmanager
b186ccd7abff   prom/prometheus     "/bin/prometheus --c…"   42 hours ago   Up 5 hours   0.0.0.0:9090->9090/tcp, :::9090->9090/tcp   promcontainer
343284b9c58f   nginx               "/docker-entrypoint.…"   2 days ago     Up 4 hours   0.0.0.0:80->80/tcp, :::80->80/tcp           nginx-proxy
Now prometheus can accessible in web browser 

----------------

**Grafana Config **

sudo apt update -y
sudo apt install -y docker
sudo systemctl start docker
sudo systemctl enable docker

NOTE :  Create a volume for grafana container which is used to store the grafana dashboards if you did not create volume when you restart the container or ec2 dashboards will be removed 

Sudo docker  volume create grafana-storage 

Now create a container and map bind the volume 

ubuntu@ip-10-0-0-44:~$ sudo docker run -d --name=grafana --restart=always -p 3000:3000 -v grafana-storage:/var/lib/grafana grafana/grafana

Now we can access the grafana dashboard

-----------------

 NODE-EXPORTER SETUP 


===> we kept our node exporter in private subnet and private sg we allowed public sg so it works as bastion host if want to access internet need to attach nat to the routetable 


Ssh -i public instancefile.pem ubuntu@privateip 



sudo apt update && sudo apt upgrade -y  # Update package lists and upgrade all installed packages

sudo useradd --no-create-home --shell /bin/false node_exporter  # Create a system user for Node Exporter without a home directory or shell (for security)

wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz  # Download the Node Exporter binary tarball from GitHub

ls -l  # List files in the current directory in long format

tar -xvf node_exporter-1.8.1.linux-amd64.tar.gz  # Extract the downloaded tar.gz file

sudo cp node_exporter-1.8.1.linux-amd64/node_exporter /usr/local/bin/  # Copy the extracted binary to /usr/local/bin (standard location for custom executables)

ls -l  # List files again to verify presence and ownership (optional)

sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter  # Change ownership of the binary to the node_exporter user and group

sudo nano /etc/systemd/system/node_exporter.service  # Create or edit the systemd service file for Node Exporter

# Example content of node_exporter.service:
 [Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

 [Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target

sudo systemctl status node_exporter  # Check if the service is already running or loaded (helpful before starting)

sudo systemctl daemon-reload  # Reload systemd to recognize the new service definition

sudo systemctl start node_exporter  # Start the Node Exporter service immediately

sudo systemctl status node_exporter  # Check the current status of the service (should say active/running)

sudo systemctl enable node_exporter  # Enable the service to start automatically on boot


-----------------------













