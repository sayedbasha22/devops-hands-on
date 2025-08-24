**What is a Rollout in Kubernetes?**
A rollout = updating your Deployment (new image, new replicas, new config, etc.).
Kubernetes will then gradually replace old Pods with new Pods so your app doesn’t go down

Steps for Rollout (Update Image)
Before updaitng the deployment 

here im updating my image because my webpage is not showing pasta image so updating the html page 

<img width="1920" height="1200" alt="Screenshot 2025-08-20 182848" src="https://github.com/user-attachments/assets/cfa3c806-cf1d-4702-94f3-778016c3faca" />

Step 1: Build & Push Docker image 
<img width="1920" height="1140" alt="image" src="https://github.com/user-attachments/assets/6865d543-36cc-4f4f-803e-7577dea0bbbc" />

docker build -t <dockerhub-user>/<image-name>:<tag> .
docker push <dockerhub-user>/<image-name>:<tag>
ex : docker build -t sayedsayedbasha22/basha-restaurant:v2 .
docker push sayedsayedbasha22/basha-restaurant:v2

Step 2: Update Deployment with new image
<img width="1920" height="1140" alt="image" src="https://github.com/user-attachments/assets/13debf1d-7f84-4c1d-838c-e0e04cc2c820" />

kubectl set image deployment/restaurant-deployment \
    restaurant-container=sayedsayedbasha22/basha-restaurant:v2
Step 3: Check rollout status
kubectl rollout status deployment/restaurant-deployment
kubectl get pods
kubectl describe deployment restaurant-deployment | grep -i image


**Steps to Scale a Deployment**
<img width="1920" height="1140" alt="image" src="https://github.com/user-attachments/assets/4f5b4a0a-d957-420d-9eae-1d057fcd8de8" />


Manually scale:
kubectl scale deployment restaurant-deployment --replicas=5
Check new replica count:
kubectl get deployment restaurant-deployment 
Autoscale (HPA): 
kubectl autoscale deployment restaurant-deployment --min=2 --max=10 --cpu-percent=80 
This means if CPU usage > 80%, Kubernetes adds more Pods (up to 10).
