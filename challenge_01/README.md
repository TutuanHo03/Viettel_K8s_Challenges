# Default Nginx - Basic (Step by step)
Step 1: 
```bash
cd challenge_01
kubectl apply -f nginx-deployment.yml
kubectl get all
```
Result:
![Screenshot from 2024-11-01 00-19-35](https://github.com/user-attachments/assets/cdbd5f04-516c-45ba-b518-be0e091697dc)

Step 2:
```bash
kubectl get nodes -o wide //Take the IP address of the single node in minikube
curl <node-IP>:30009
```
Result:
![Screenshot from 2024-11-01 00-20-12](https://github.com/user-attachments/assets/1519b39d-b7e3-40ec-9cde-14dc190949bb)
