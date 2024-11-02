# Challenge 02
Đề bài: Triển khai deployment một ứng dụng web tĩnh lên kubernetes cho phép truy cập từ bên ngoài thông qua nodePort

Output:

• Đóng gói thành công container chứa web tĩnh

o download 1 template tại (https://www.free-css.com/free-css-templates)

o sử dụng base image nginx

o lưu ý cấu hình nginx trỏ tới web tĩnh (tham khảo file cấu hình mẫu đơn giản tại https://gist.github.com/mockra/9062657)

• 1 deployment chạy ứng dụng web tĩnh (replicas=2)

• 1 nodePort service trỏ tới deployment (service web 1)

• Thực hiên curl tới nodePort và cho ra kết quả trang web tĩnh theo template

## Các bước thực hiện:
Bước 1: 
```bash
cd challenge_02
docker build -t web-static-nginx .
docker tag web-static-nginx tutu1008/nginx-static-web:1.0
docker push tutu1008/nginx-static-web:1.0
```
Đây là bước đầu thực hiện đóng gói ứng dụng qua Docker, build image và đẩy image lên DockerHub để có thể triển khai trên Kubernetes. 
![Screenshot from 2024-11-02 21-44-24](https://github.com/user-attachments/assets/f7f0fcd1-638a-45e9-ac5d-b9a330a34989)

Bước 2:
```bash
kubectl create namespace challenge-02
kubectl apply -f nginx-deploy-svc.yml -n challenge-02
kubectl get all -n challenge-02
```
Đây là bước triển khai ứng dụng static web của nginx trên minikube
![Screenshot from 2024-11-02 21-47-20](https://github.com/user-attachments/assets/7ca8d061-0918-4bdc-9bf8-bf1e19c39187)

Bước 3: Thực hiên curl tới nodePort và cho ra kết quả trang web tĩnh theo template
```bash
kubectl get nodes -o wide // Lấy NodeIP trên minkube
curl <NodeIP>:30060
```
![Screenshot from 2024-11-02 21-47-46](https://github.com/user-attachments/assets/88e3cde7-b251-4876-af66-0ed2e7b03ef2)
