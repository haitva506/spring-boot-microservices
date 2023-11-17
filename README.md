# spring-boot-microservices
# 1. Cài đặt môi trường cho ví dụ
- B1: Tạo network docker
    > sudo docker network create cicd-network

- B2: chạy để cài đặt các service cần thiết trong thư mục docker/docker-compose.yml
    > sudo docker-compose -f docker-compose.yml up -d
    
- B3: truy cập vào docker container để thực thi lệnh
    > sudo docker exec -it CONTAINER ID bin/bash
	
- B4: tài khảo đăng nhập gitlab
    - tài khoản root
    - mật khẩu: lấy trong file etc/gitlab/init..

- B5: cài đặt docker registry private để chứa các image trong thư mục private-registry
    > sudo docker-compose -f docker-compose.yml up -d
    - tạo htpasswd để đăng nhập registry
        - sudo docker run --entrypoint htpasswd httpd:2 -Bbn testuser testpassword > /auth/htpasswd
        
- B6: build image ansible để push lên registry docker/ansible/Dockerfile
    > suddo docker build -f Dockerfile .
    - push lên registry
    	> docker tag ansible:latest private-docker-registry.com:5000/ansible/ansible:latest
	> docker push private-docker-registry.com:5000/demo/ansible:latest
	
- B7: pull và push image maven:3.6.3-openjdk-8 lên registry như iamge ansible

- B8: cấu hình đăng nhập để con gitlab-runner có thể pull image từ con registry
    > printf "my_username:my_password" | openssl base64 -A -> dGVzdHVzZXI6dGVzdHBhc3N3b3Jk
    - [[runners]]
        environment = ["DOCKER_AUTH_CONFIG={\"auths\":{\"private-docker-registry.com:5000\":{\"auth\":\"dGVzdHVzZXI6dGVzdHBhc3N3b3Jk\"}}}"]

- B9: cấu hình trên gitlab các biến

- B10: chạy docker/gitlab-runner-regiser.sh
    - registration_token=GR1348941xXyWX2PZNAxAqxhqK8sj
        - lấy trên gitlab
    - url=http://172.21.0.9
        - gitlab

- B11: ssh cài đặt ssh cho các máy chủ
    > apt-get update
    > apt-get install -y openssh-server
    > sed -i 's/#Port 22/Port 22/' /etc/ssh/sshd_config
    > service ssh start
    - tạo user ssh
        > useradd cicd-user

- B12: keygen tạo cặp khoá để ssh
    > ssh-keygen -t rsa -b 4096
    > ssh-copy-id -f user@ip

- B13: java cài đặt jdk cho máy chủ triển khai
    > apt-get install openjdk-17-jdk

Xin chân thành cảm ơn!
