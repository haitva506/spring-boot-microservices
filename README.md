# CICD spring-boot-microservices
### Mục lục

[I. Mở đầu](#modau)

[II. Cài đặt môi trường](#caidat)
- [1. Các bước cài đặt môi trường](#cacbuoccaidat)


<a name="modau"></a>
## I. Mở đầu
...
<a name="caidat"></a>
## II. Cài đặt môi trường
<a name="cacbuoccaidat"></a>
# 1. Các bước cài đặt môi trường
- B1: Tạo network docker
     ```sh
      sudo docker network create cicd-network
     ```
- B2: Chạy để cài đặt các service cần thiết trong thư mục docker/docker-compose.yml
     ```sh
      sudo docker-compose -f docker-compose.yml up -d
     ```
    
- B3: Truy cập vào docker container để thực thi lệnh
     ```sh
      sudo docker exec -it CONTAINER ID bin/bash
     ```
	
- B4: Tài khảo đăng nhập gitlab
    - Tài khoản: root
    - Mật khẩu: lấy trong file etc/gitlab/init..

- B5: Cài đặt docker registry private để chứa các image trong thư mục private-registry
     - B5.1 
     ```sh
      sudo docker-compose -f docker-compose.yml up -d
     ```
     - B5.2: Tạo htpasswd để đăng nhập registry
     ```sh
       sudo docker run --entrypoint htpasswd httpd:2 -Bbn testuser testpassword > /auth/htpasswd
     ```
        
- B6: Build image ansible để push lên registry docker/ansible/Dockerfile
     - B6.1 
     ```sh
       sudo docker build -f Dockerfile .
     ```
     - B6.2: Push lên registry
     ```sh
    	docker tag ansible:latest private-docker-registry.com:5000/ansible/ansible:latest
        docker push private-docker-registry.com:5000/demo/ansible:latest
     ```
	
- B7: Pull và push image maven:3.6.3-openjdk-8 lên registry như iamge ansible

- B8: Cấu hình đăng nhập để con gitlab-runner có thể pull image từ con registry
    ```sh
       printf "my_username:my_password" | openssl base64 -A -> dGVzdHVzZXI6dGVzdHBhc3N3b3Jk
    ```
    - [[runners]]
        environment = ["DOCKER_AUTH_CONFIG={\"auths\":{\"private-docker-registry.com:5000\":{\"auth\":\"dGVzdHVzZXI6dGVzdHBhc3N3b3Jk\"}}}"]

- B9: Cấu hình trên gitlab các biến

- B10: Chạy docker/gitlab-runner-regiser.sh
    - registration_token=GR1348941xXyWX2PZNAxAqxhqK8sj
        - lấy trên gitlab
    - url=http://172.21.0.9
        - gitlab

- B11: SSH cài đặt ssh cho các máy chủ
   - B11.1
    ```sh
      apt-get update
      apt-get install -y openssh-server
      sed -i 's/#Port 22/Port 22/' /etc/ssh/sshd_config
      service ssh start
    ```
    - B11.2: Tạo user ssh
     ```sh
       useradd cicd-user
     ```

- B12: Tạo cặp khoá để ssh
     ```sh
       ssh-keygen -t rsa -b 4096
       ssh-copy-id -f user@ip
     ```

- B13: Cài đặt jdk cho máy chủ triển khai
     ```sh
       apt-get install openjdk-17-jdk
     ```

Xin chân thành cảm ơn!
