**SLACK INTEGRATION**
How to Integrate Slack ?
https://api.slack.com/apps hit this url and create a new app and select from new scratch and provide app name and select workspace name you will get below page 


<img width="1232" height="672" alt="Screenshot from 2025-07-24 10-55-16" src="https://github.com/user-attachments/assets/20cb4df0-530f-4cf3-a4bc-ca746564561a" />


Create a Slack Webhook

Active the incoming webhook and add new webhooks and it will ask permission for your slack provide it you will get below page 


****<img width="1232" height="672" alt="Screenshot from 2025-07-24 10-57-45" src="https://github.com/user-attachments/assets/60798d7d-6b4c-4434-855b-da2f422b482f" />
Now copy the webhook and update it to the alertmanager.yml file in prometheus 
Example : 
Alertmanager.yml 

**Add Slack webhook in Alertmanager.yml**

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
Now you can run the  stress command on node_exporter and you will get alert on slack channel 


<img width="1232" height="672" alt="Screenshot from 2025-07-24 11-02-33" src="https://github.com/user-attachments/assets/42ad34c0-1d18-46f1-848c-a93157778a6d" />

