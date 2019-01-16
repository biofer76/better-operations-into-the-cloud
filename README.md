# Better Operations into the Cloud

Source code of my talk related to DevOps tools:

* Ansible
* Docker
* GitLab


## Before to start

Generate a pair of keys (id_rsa) for SSH connection to remote hosts and store in `keys` folder.

```
ssh-keygen -b 2048 -t rsa -f $PWD/keys/id_rsa -q -N ""
```

## Ansible

How to use Ansible

### Run Ansible on Docker container

Build a Docker image with latest version of Ansible 

```
docker build -t debian-ansible -f docker/debian-ansible/Dockerfile .
```

Create two instances on your Cloud platform and set public key stored in `keys` folder`

Get Public IPs of your instances and add them to `inventory.ini` file 

```
[webservers]
192.168.1.100 ansible_user=username  ansible_ssh_common_args='-o StrictHostKeyChecking=no'
192.168.1.101 ansible_user=username  ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

Set the IPs in the first column and value of `ansible_user` variable in the second one.

#### Ping Cloud hosts by Ansible

```
docker run -t --rm \
	-v $PWD/ansible:/ansible \
	-v $PWD/keys:/root/.ssh \
	debian-ansible \
	ansible webservers -m ping -i /ansible/inventory.ini
```

#### Configure Cloud hosts with Playbook

```
docker run -t --rm \
	-v $PWD/ansible:/ansible \
	-v $PWD/keys:/root/.ssh \
    debian-ansible \
    ansible-playbook -i /ansible/inventory.ini ansible/playbook.yml
```

#### Create containerized command

```
alias docker-ansible="docker run -t --rm -v $PWD/ansible:/ansible debian-ansible ansible"
```

Try with:

```
docker-ansible --version
```