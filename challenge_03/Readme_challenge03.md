# Challenge 03
Đề bài: triển khai nginx proxy cho nhiều ứng dụng
- từ level 2, triển khai thêm 1 trang web static thứ hai, khác với static web đã triển khai
- service cho trang web tĩnh mới được lấy tên là web2
- triển khai thêm 1 deployment nginx-proxy đóng vai trò proxy cho cả 2 ứng dụng trên và tạo
nodePort service có tên "nginx-proxy“
- thiết lập cấu hình config của nginx-proxy sao cho:
  + khi gọi tới nginx-proxy với path /web1 > nginx-proxy filter path và forward tới service web 1 > service web1
  + khi gọi tới nginx-proxy với path /web2 > nginx-proxy filter path và forward tới service web 2 > service web2

Output:
+ curl http://\<node-ip>:\<node-port>/web1 > trả về static web 1
+ curl http://\<node-ip>:\<node-port>/web2 > trả về static web 2

## Các bước thực hiện:
Bước 1: Tạo static web thứ 2, tương tự như web 1 ở challenge_02. Do em đã push image của web 2 lên Docker Hub rồi, nên ở bước này sẽ triển khai trên minikube luôn.
```bash
cd challenge_03
kubectl create namespace web-nginx-2
kubectl apply -f nginx-web2.yml -n web-nginx-2
kubectl get all -n web-nginx-2
```
![Screenshot from 2024-11-04 21-15-17](https://github.com/user-attachments/assets/cb6b3656-f22e-4959-9c32-407ae45e67bd)

Bước 2: Tạo nginx-proxy, dockerize nginx-proxy rồi triển khai trên minikube

Trước khi triển khai nginx-proxy, ta phải kiểm tra xem ở web1 và web2, các service của chúng có ở dạng ClusterIP không? Tạo sao service web1 và web2 phải để ở ClusterIP và service của nginx-proxy ở NodePort?

-> Mục đích của ClusterIP: Tạo một địa chỉ IP nội bộ cho service trong cluster để các pod khác (như nginx-proxy) có thể truy cập chúng mà không cần ra ngoài mạng cluster.

-> Lý do sử dụng: Nginx proxy chạy bên trong cluster và cần truy cập các service của web1 và web2 thông qua địa chỉ nội bộ. Với type ClusterIP, các service chỉ khả dụng từ bên trong cluster, điều này là đủ cho Nginx để định tuyến các yêu cầu.

-> Service của nginx-proxy cần có type NodePort để các yêu cầu từ bên ngoài cluster có thể truy cập vào proxy này. NodePort mở một cổng cụ thể trên mỗi node trong cluster, và khi có yêu cầu tới cổng đó, nó sẽ chuyển tiếp vào pod Nginx proxy.

```bash
cd nginx-proxy
//Các bước docker build với push, em đã thực hiện và đẩy lên Docker Hub rồi, nên bỏ qua bước này
docker build -t my-nginx-proxy .
docker tag my-nginx-proxy tutu1008/my-nginx-proxy:1.0 
docker push tutu/my-nginx-proxy:1.0

kubectl create namespace nginx-proxy
kubectl apply -f nginx-proxy.yml -n nginx-proxy
kubectl get all -n nginx-proxy
```
![Screenshot from 2024-11-04 21-22-50](https://github.com/user-attachments/assets/7fde99b2-d82d-4dd4-ae1b-12974ae72250)

![Screenshot from 2024-11-04 21-23-39](https://github.com/user-attachments/assets/43bca398-0ec3-4a2b-a448-73c7d4f9f42d)

Bước 3: Dùng nginx-proxy gọi tới web1 và web2
```bash
curl <nodeIP>:30081/web1
curl <nodeIP>:30081/web2
```
Kết quả: Ở đây nodeIP minikube của em là 192.168.59.100
![Screenshot from 2024-11-04 21-25-09](https://github.com/user-attachments/assets/53b7b7f3-c220-4879-abb7-38a20094cfa8)

![Screenshot from 2024-11-04 21-29-16](https://github.com/user-attachments/assets/ac995daa-d6d6-4206-830a-312bff144e7f)
