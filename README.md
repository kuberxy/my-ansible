## 建立Ansible运行环境

### 环境依赖

#### 中控节点

- Python 2 (versions 2.6 or 2.7) or Python 3 (versions 3.5 and higher) 



#### 被管节点

- ssh

- Python 2 (version 2.6 or later) or Python 3 (version 3.5 or later)
- SELinux disabled



### 创建管理用户

```shell
sudo adduser --disabled-password --gecos "" myadmin
echo 'myadmin:myadmin' | sudo chpasswd

sudo bash -c "echo 'myadmin    ALL=(ALL:ALL) NOPASSWD: ALL' > /etc/sudoers.d/myadmin"

sudo su - myadmin

ssh-keygen -t rsa -f ~/.ssh/id_rsa -P "" -q
```



### 获取my-ansible

```shell
cd ~ && git clone https://github.com/kuberxy/my-ansible.git && cd my-ansible
```



### 安装ansible

```shell
sudo apt-get install -y python3-pip sshpass

sudo bash -c "
cat > /etc/pip.conf << EOF
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple
EOF
"

pip3 install -r requirements.txt
```



### 安装python3
注：根据环境调整hosts.ini

```shell
ansible -i hosts.ini all -m ping --ssh-common-args="-o StrictHostKeyChecking=no"
ansible -bi hosts.ini ubuntu -m raw -a "apt-get install -y python3-minimal"
ansible -i hosts.ini all -m ping
```



### 与被管节点建立SSH互信
注：根据环境调整inventory.ini

```shell
ansible-playbook -i hosts.ini create_users.yml -b
ansible -i inventory.ini all -m ping
```

### 配置节点hosts文件
注：根据环境调整hosts
```shell
ansible -bi inventory.ini all -m copy -a "src=./hosts dest=/etc/hosts owner=root  group=root mode=0644 backup=yes"
```

