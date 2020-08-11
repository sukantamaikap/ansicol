# ansicol
This containes a testbed entirely on docker-compose for experimenting with ansible. The setup consists of 3 centos7 containers and 3 Ubuntu18.08 container as target hosts and a control host from where to run the ansible playbooks.

## What is the environment setup?
There is a control host followed by 3 Centos 7 and 3 Ubuntu 16.04 hosts. The src folder is mounted to ```bash /opt/sandbox/src``` of the control container. 

Each non-control host has the below users and password:
```bash
root:root
ansicol:ansicol
```
ansicol is a sudor.

## How to start the environment?
Go to the root of the project and run:
>> docker-compose up

This should bring up the whole cluster of machines mentioned above.

```bash
ansible/ansicol [ docker-compose ps                                               master * ] 5:11 pm
      Name               Command        State              Ports
---------------------------------------------------------------------------
ansicol_c1_1        /usr/sbin/sshd -D   Up      22/tcp
ansicol_c2_1        /usr/sbin/sshd -D   Up      22/tcp
ansicol_c3_1        /usr/sbin/sshd -D   Up      22/tcp
ansicol_control_1   /bin/bash           Up      22/tcp
ansicol_u1_1        /usr/sbin/sshd -D   Up      22/tcp, 50070/tcp, 8088/tcp
ansicol_u2_1        /usr/sbin/sshd -D   Up      22/tcp, 50070/tcp, 8088/tcp
ansicol_u3_1        /usr/sbin/sshd -D   Up      22/tcp, 50070/tcp, 8088/tcp
```

## How do I validate the setup?
1. Connect to the control host and validate ansible is installed.
```bash
ansible/ansicol [  docker container exec -it ansicol_control_1 bash                                                               master * ] 5:28 pm
root@control:/# ansible --version
ansible 2.5.1
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Jul 20 2020, 15:37:01) [GCC 7.5.0]
```

2. Validate the control machine can reach all target hosts.

```bash
root@control:/# ping -c3 ansicol_c1_1
PING ansicol_c1_1 (10.6.0.4) 56(84) bytes of data.
64 bytes from ansicol_c1_1.ansicol_local (10.6.0.4): icmp_seq=1 ttl=64 time=0.108 ms
64 bytes from ansicol_c1_1.ansicol_local (10.6.0.4): icmp_seq=2 ttl=64 time=0.088 ms
64 bytes from ansicol_c1_1.ansicol_local (10.6.0.4): icmp_seq=3 ttl=64 time=0.099 ms
root@control:/# ping -c3 ansicol_c2_1
PING ansicol_c2_1 (10.6.0.5) 56(84) bytes of data.
64 bytes from ansicol_c2_1.ansicol_local (10.6.0.5): icmp_seq=1 ttl=64 time=0.445 ms
64 bytes from ansicol_c2_1.ansicol_local (10.6.0.5): icmp_seq=2 ttl=64 time=0.143 ms
64 bytes from ansicol_c2_1.ansicol_local (10.6.0.5): icmp_seq=3 ttl=64 time=0.184 ms

--- ansicol_c2_1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2064ms
rtt min/avg/max/mdev = 0.143/0.257/0.445/0.134 

etc
```
3. Validate the control host can ssh to all target hosts as ansicol user (password: ansicol)
```bash
ansible/ansicol [ docker container exec -it ansicol_control_1 bash                                                                        master ] 8:35 pm
root@control:/opt/sandbox/src# ssh ansicol@c1
ansicol@c1's password:
Last login: Tue Aug 11 10:34:04 2020 from ansicol_control_1.ansicol_local
[ansicol@c1 ~]$ sudo su
[root@c1 ansicol]#

root@control:/opt/sandbox/src# ssh ansicol@u1
The authenticity of host 'u1 (10.6.0.7)' can't be established.
ECDSA key fingerprint is SHA256:a71Z38MkY/MVx569IiV9ZNvqUAKZa5IMMDHKEKUEZco.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'u1,10.6.0.7' (ECDSA) to the list of known hosts.
ansicol@u1's password:
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.19.76-linuxkit x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

$ sudo su
root@u1:/home/ansicol#
```
Now we are all set!!!

## What are the development workflow?
1. On your host machine, using your favourite editor, compose any playbook.
2. Update the hosts file as required.
3. Connect to control container, and run the playbook

