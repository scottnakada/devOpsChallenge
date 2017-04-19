# Ansible AWS Example
# This script sets up 2 Apache web-servers, and an NGINX web-server on
# Amazon Web Services, and connects them to an Elastic Load Balancer (ELB)

# To run this script
1) Edit the vars.yml file to add your AWS security credentials, aws_access_key, and aws_secret_key.
You will also need to add your IP to the allowable IPs for access to the security group (see my_ip
in the vars.yml file)
2) You will need an aws pem file to access the ec2 instances, and you must set that key
name in the vars.yml file (aws_key_name)
3) To execute the script use:

	ansible-playbook playbook.yml -i ec2.py -e @vars.yml

The script will then:
a) Start running playbook.yml
b) start running roles/vpc/tasks/main.yml
c) create a vpc (my_vpc)
d) store the vpc id in vpc_id
e) create a public subnet (my_public_subnet_1)
f) store the public subnet id (public_subnet_id_1)
g) create a 2nd public subnet (my_public_subnet_2)
h) store the public subnet id (public_subnet_id_2)
i) create an internet gateway (my_vpc_igw)
j) store the gateway id (my_vpc_igw)
k) create a vpc routing table
l) create a vpc security group
m) start running roles/ec2/tasks/main.yml
n) create a security group for the instances (sg_instance_name)
o) create 2 ec2 instances for apache (apache_ec2)
p) Add the apache_ec2 instances to a host group apache_ec2
q) create 1 ec2 instance for nginx (nginx_ec2)
r) Add the nginx_ec2 instance to a host group nginx_ec2
s) wait for the apache_ec2 instances to bring up ssh (needed to configure them with ansible)
t) wait for the nginx_ec2 instance to bring up ssh (needed to configure them with ansible)
u) For the host group apache_ec2, start running roles/apache/tasks/main.yml
v) use apt-get to install the latest apache2 package
w) modify the configuration file to listen on port 8900 (set as http_port in vars.yml)
x) create a log directory /var/log/tdcustom
y) create a symbolic link from /var/log/tdcustom/accesslogs to /var/log/apache2
z) set the apache user and group (apache_user, apache_group set in vars.yml)
A) Copy/edit the index.html file in /var/www/html/index.html
B) use the restart apache handler to use service to restart apache2
C) for the host group nginx_ec2, start running roles/nginx/tasks/main.yml
D) use apt-get to install nginx package
E) configure the /etc/nginx/sites-enabled/default file to listen on port 8900
F) create a /var/log/tdcustom directory, and create a symbolic link from
     /var/log/tdcustom/accesslogs to /var/log/nginx
G) set the nginx user
H) Copy/edit the index.html file in /usr/share/nginx/html/index.html
I) using the restart nginx handler, use service to restart nginx
J) start running the roles/elb/tasks/main.yml file
K) create an elastic load balancer
L) add the apache_ec2 instances to the load balancer
M) add the nginx_ec2 instancesto the load balancer
