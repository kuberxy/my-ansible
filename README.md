## 建立Ansible运行环境

### 环境依赖

#### 中心节点

- Python 2 (versions 2.6 or 2.7) or Python 3 (versions 3.5 and higher) 



#### 远程节点

- ssh

- Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later)
- SELinux disabled



### 创建管理用户

```shell
sudo useradd cyadmin -m
echo 'cyadmin:cyadmin' | sudo chpasswd

sudo bash -c "echo 'cyadmin    ALL=(ALL:ALL) NOPASSWD: ALL' > /etc/sudoers.d/cyadmin"

sudo su - cyadmin

ssh-keygen -t rsa -f ~/.ssh/id_rsa -P "" -q
```



### 获取my-ansible

```shell
cd ~ && git clone https://github.com/kuberxy/my-ansible.git && cd my-ansible
```



### 安装ansible

```shell
sudo apt-get install -y python-pip sshpass
pip install -r requirements.txt
```



### 安装python2

```shell
ansible -i hosts.ini all -m ping --ssh-common-args="-o StrictHostKeyChecking=no"
ansible -i hosts.ini ubuntu -m raw -a "apt-get install -y python-minimal" -b
ansible -i hosts.ini all -m ping
```



### 管理用户SSH互信

```shell
ansible-playbook -i hosts.ini create_users.yml -b
ansible -i inventory.ini all -m ping
```


