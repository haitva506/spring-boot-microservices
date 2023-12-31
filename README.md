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
- B-1: Tạo network docker
     ```sh
      sudo docker network create cicd-network
     ```
- B-2: Chạy để cài đặt các service cần thiết trong thư mục docker/docker-compose.yml
     ```sh
      sudo docker-compose -f docker-compose.yml up -d
     ```
    
- B-3: Truy cập vào docker container để thực thi lệnh
     ```sh
      sudo docker exec -it CONTAINER ID bin/bash
     ```
	
- B-4: Tài khảo đăng nhập gitlab
    - Tài khoản: root
    - Mật khẩu: lấy trong file etc/gitlab/init..

- B-5: Cài đặt docker registry private để chứa các image trong thư mục private-registry
     - B-5.1 
     ```sh
      sudo docker-compose -f docker-compose.yml up -d
     ```
     - B-5.2: Tạo htpasswd để đăng nhập registry
     ```sh
       sudo docker run --entrypoint htpasswd httpd:2 -Bbn testuser testpassword > /auth/htpasswd
     ```
        
- B-6: Build image ansible để push lên registry docker/ansible/Dockerfile
     - B-6.1 
     ```sh
       sudo docker build -f Dockerfile .
     ```
     - B-6.2: Push lên registry
     ```sh
    	docker tag ansible:latest private-docker-registry.com:5000/ansible/ansible:latest
        docker push private-docker-registry.com:5000/demo/ansible:latest
     ```
	
- B-7: Pull và push image maven:3.6.3-openjdk-8 lên registry như iamge ansible

- B-8: Cấu hình đăng nhập để con gitlab-runner có thể pull image từ con registry
    ```sh
       printf "my_username:my_password" | openssl base64 -A -> dGVzdHVzZXI6dGVzdHBhc3N3b3Jk
    ```
    - [[runners]]
        environment = ["DOCKER_AUTH_CONFIG={\"auths\":{\"private-docker-registry.com:5000\":{\"auth\":\"dGVzdHVzZXI6dGVzdHBhc3N3b3Jk\"}}}"]

- B-9: Cấu hình trên gitlab các biến

- B-10: Chạy docker/gitlab-runner-regiser.sh
    - registration_token=GR1348941xXyWX2PZNAxAqxhqK8sj
        - lấy trên gitlab
    - url=http://172.21.0.9
        - gitlab

- B-11: SSH cài đặt ssh cho các máy chủ
   - B-11.1
    ```sh
      apt-get update
      apt-get install -y openssh-server
      sed -i 's/#Port 22/Port 22/' /etc/ssh/sshd_config
      service ssh start
    ```
    - B-11.2: Tạo user ssh
     ```sh
       useradd cicd-user
     ```

- B-12: Tạo cặp khoá để ssh
     ```sh
       ssh-keygen -t rsa -b 4096
       ssh-copy-id -f user@ip
     ```

- B-13: Cài đặt jdk cho máy chủ triển khai
     ```sh
       apt-get install openjdk-17-jdk
     ```

Xin chân thành cảm ơn!
