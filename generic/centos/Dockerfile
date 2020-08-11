FROM centos:7

EXPOSE 22

RUN yum -y update \
    && yum install -y \ 
    iputils \
    openssh-server \
    nmap \
    net-tools \
    passwd \
    sudo

ADD ./start.sh /start.sh
RUN mkdir /var/run/sshd &&\
    echo 'root:root' | chpasswd &&\
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config &&\
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config &&\
    adduser ansicol && echo "ansicol:ansicol" | chpasswd &&\
    # disable passwrod for sudo
    echo "ansicol ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers &&\
    ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -N ''

RUN chmod 755 /start.sh
# EXPOSE 22
RUN ./start.sh
ENTRYPOINT ["/usr/sbin/sshd", "-D"]