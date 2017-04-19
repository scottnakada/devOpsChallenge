### Ansible AWS Example
# This script sets up 2 Apache web-servers, and an NGINX web-server on Amazon Web Services, and connects them to an Elastic Load Balancer (ELB)

# To run this script
1) Edit the vars.yml file to add your AWS security credentials, aws_access_key, and aws_secret_key.
You will also need to add your IP to the allowable IPs for access to the security group (see my_ip
in the vars.yml file)
2) You will need an aws pem file to access the ec2 instances, and you must set that key
name in the vars.yml file (aws_key_name)
3) To execute the script use:

	ansible-playbook playbook.yml -i ec2.py -e @vars.yml

The script will then:
1) Start running playbook.yml
2) start running roles/vpc/tasks/main.yml
3) create a vpc (my_vpc)
4) store the vpc id in vpc_id
5) create a public subnet (my_public_subnet_1)
6) store the public subnet id (public_subnet_id_1)
7) create a 2nd public subnet (my_public_subnet_2)
8) store the public subnet id (public_subnet_id_2)
9) create an internet gateway (my_vpc_igw)
10) store the gateway id (my_vpc_igw)
11) create a vpc routing table
12) create a vpc security group
13) start running roles/ec2/tasks/main.yml
14) create a security group for the instances (sg_instance_name)
15) create 2 ec2 instances for apache (apache_ec2)
16) Add the apache_ec2 instances to a host group apache_ec2
17) create 1 ec2 instance for nginx (nginx_ec2)
18) Add the nginx_ec2 instance to a host group nginx_ec2
19) wait for the apache_ec2 instances to bring up ssh (needed to configure them with ansible)
20) wait for the nginx_ec2 instance to bring up ssh (needed to configure them with ansible)
21) For the host group apache_ec2, start running roles/apache/tasks/main.yml
22) use apt-get to install the latest apache2 package
23) modify the configuration file to listen on port 8900 (set as http_port in vars.yml)
24) create a log directory /var/log/tdcustom
25) create a symbolic link from /var/log/tdcustom/accesslogs to /var/log/apache2
26) set the apache user and group (apache_user, apache_group set in vars.yml)
27) Copy/edit the index.html file in /var/www/html/index.html
28) use the restart apache handler to use service to restart apache2
29) for the host group nginx_ec2, start running roles/nginx/tasks/main.yml
30) use apt-get to install nginx package
31) configure the /etc/nginx/sites-enabled/default file to listen on port 8900
32) create a /var/log/tdcustom directory, and create a symbolic link from
     /var/log/tdcustom/accesslogs to /var/log/nginx
33) set the nginx user
34) Copy/edit the index.html file in /usr/share/nginx/html/index.html
35) using the restart nginx handler, use service to restart nginx
36) start running the roles/elb/tasks/main.yml file
37) create an elastic load balancer
38) add the apache_ec2 instances to the load balancer
39) add the nginx_ec2 instancesto the load balancer
