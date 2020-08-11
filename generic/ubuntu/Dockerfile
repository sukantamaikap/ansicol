FROM ubuntu:16.04

RUN apt-get -y update \
    && apt-get install -y \ 
    iputils-ping \
    vim \
    nmap \
    ssh \
    netcat \
    net-tools \
    openssh-server \
    sudo

RUN mkdir /var/run/sshd &&\
    echo 'root:root' | chpasswd &&\
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config &&\
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config &&\
    # SSH login fix. Otherwise user is kicked off after login
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd &&\
    useradd -m ansicol && echo "ansicol:ansicol" | chpasswd && adduser ansicol sudo &&\
    # disable passwrod for sudo
    echo "ansicol ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22 80 443

CMD ["/usr/sbin/sshd", "-D"]