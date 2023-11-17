FROM ubuntu:20.04

WORKDIR /

RUN apt update -y && \
        apt install software-properties-common -y && \
        add-apt-repository ppa:deadsnakes/ppa && \
        apt install python3.8 -y

RUN apt-add-repository ppa:ansible/ansible && \
        apt update -y && \
        apt install ansible -y

RUN apt update -y && \
        apt install telnet -y && \
        apt install vim -y && \
        apt install nano -y && \
        apt install curl -y

RUN apt update -y && \
        apt install build-essential -y

# RUN curl https://deb.releases.teleport.dev/teleport-pubkey.asc | apt-key add -
# RUN add-apt-repository 'deb https://deb.releases.teleport.dev/ stable main'
# RUN apt-get update
# RUN apt install teleport
#RUN curl -O https://cdn.teleport.dev/teleport-v13.3.6-linux-amd64-bin.tar.gz
#RUN tar -xzf teleport-v13.3.6-linux-amd64-bin.tar.gz
#RUN cd teleport && ./install


# RUN mkdir -p /opt/v3

# RUN mkdir -p /opt/tools

# COPY ./v3 /opt/v3

# COPY ./tools /opt/tools
