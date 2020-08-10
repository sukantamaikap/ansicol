FROM ubuntu:18.04

EXPOSE 22

RUN apt-get -y update \
    && apt install -y \ 
    ansible \
    iputils-ping \
    curl \
    openssh-client \
    vim \
    nmap \
    net-tools \
    && ansible --version

# points to the config file mentioned here
ENV ANSIBEL_CONFIG=/opt/sandbox/src/ansible.cfg
# for ignoring host fingerprint
ENV ANSIBLE_HOST_KEY_CHECKING=False

WORKDIR /opt/sandbox/src/

CMD ["/bin/bash"]