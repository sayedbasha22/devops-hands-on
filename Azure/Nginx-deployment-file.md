How to login azure cli ?
az login --use-device-code


How to create a aks cluster in az ?

az aks create \
  --resource-group my-k8s-resource-group \
  --name sayed-demo-aks \
  --node-count 1 \
  --node-vm-size Standard_B2s \
  --generate-ssh-keys \
  --location eastus

How to get aks credentials ?
az aks get-credentials --resource-group my-k8s-resource-group --name sayed-demo-aks

Check cluster now ?
kubectl get nodes

How to check current cluster ?
 kubectl config current-context
sayed-demo-aks

Step1: How to create yaml files ?
touch nginx-deployment.yaml

nano nginx-deployment.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
save it 
step2 : service yaml 

touch nginx-service.yaml
nano nginx-service.yaml 
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
save it
Step3 :
try any errors in yaml usig dry run command 

kubeclt apply --dry-run=client -f nginx-deployment.yaml
kubectl apply --dry-run=client -f nginx-service.yaml 
if success deploy the yaml files 
kubectl apply -f nginx-deployment.yaml
kubectl apply -f nginx-service.yaml

check pods 
kubectl get pods -A | grep nginx
check service 
kunectl get svc 



<img width="1920" height="1200" alt="Screenshot 2025-08-09 143135" src="https://github.com/user-attachments/assets/ba986318-72bf-41b6-bc30-47909d36fa97" />
<img width="1920" height="1200" alt="Screenshot 2025-08-09 143450" src="https://github.com/user-attachments/assets/4a152a55-d21b-4a54-be8b-71524aa17bed" />


