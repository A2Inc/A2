# How to deploy

## tools requirements

```shell

$pip3 install virtualenv
$virtualenv -p /usr/bin/python3 A2IncEnv
$source A2IncEnv/bin/activate
$(A2IncEnv) xx $ pip3 install -r requirements.txt

```

## update the initial files

Modify the inventories/hosts:

```shell
 
  ansible_python_interpreter=/Users/chenzhen/Documents/workspace/compx527/bin/python

```

to your own directory.

## generate the EC2 server

Modify the group_vars/all:

```shell

initial: true
ec2_private_key_dir: "~/.ssh/cz_sg-ec2-key.pem" # modify to you own pem file

```

Then run the ansible-playbook:

```shell

ansible-playbook -v -i hosts site.yml

```

Be careful, you need to prepare the aws-cli environment at first. (You should run "aws configure".)

Totally, 6 EC2 instances could be generated after running. (nginx*2, tomcat*2, java*2)

## terminate the servers

To save the budget, after using them, dont forget termniating them.

Upate the group_vars/all:

```shell

initial: false
terminate: true

```

Run the ansible-playbook to terminate all your instances:

```shell

ansible-playbook -v -i hosts site.yml

```

## deploy 

Install and configure nginx:

```shell

ansible-playbook -v -i hosts site.yml -t nginx

```

Install and configure tomcat:

```shell

ansible-playbook -v -i hosts site.yml -t tomcat

```

Install and configure java app:

```shell

ansible-playbook -v -i hosts site.yml -t java

```