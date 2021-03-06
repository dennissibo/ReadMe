# ReadMe

## Steps
## Step 1: 
Create SSH using `ssh key-gen` and `cat ~/.ssh/id_rsa.pub`
## Step 2: 
Log in to AWS and go to EC2 dashboard.
## Step 3:
Add security groups with Inbound as the following:

| Type       | Protocol   |  Port Range |  Source      | 
| --- | --- | --- | --- |
| HTTP       | TCP        |  80         |   Anywhere   |  
| Custom TCP | TCP        |  8888       |   Anywhere   |  
| SSH        | TCP        |  22         |   Anywhere   |                               
| Custom TCP | TCP        |  27016      |   Anywhere   |

## Step 4:
Lunch AWS EC2 Instantce, first chose Ubuntu AMI server with the t2.micro(1GiB) type, then add it with 20GB storage. Use the seccurity group we set up in the pervious step. Everything else chose defult. 

View the instance just created and locate the _IPv4 public IP_ address.
## Step 5:
Run `ssh ubuntu@your_IP_Address` in Terminal to connect terminal to IP. 

Install docker by using command `curl -sSl https://get.docker.com | sh` in Terminal. Log out with the command `logout`, to log back in with command `ssh ubuntu@your_IP_Address`.

Then run `sudo usermod -aG docker ubuntu` to add ubuntu to use docker. Check the docker version with `docker -v`.
## Step 6:
Use `docker pull jupyter/datascience-notebook` to download the correct image. 
## Step 7:
Use `docker run -p 80:8888 -v /home/ubuntu:/home/jovyan jupyter/datascience-notebook` to run the notebook, to check that a jupyterhub container has been launched, use `docker ps`. 

Another way to find the container ID is by running `docker ps -a` to list the beginning sequence of the Container, the first four character is enough to idtenify the container_id. 
## Step 8:
Go to IPv4 Public IP:443 in a browser and paste the token from docker to log in to jupyter. 

## Security Concern:
Whether or not we would lose data once we terminate the instance.

## Diagram:
Local(Bash Shell) ——> by SSH keys ——> AWS server Farm {Ubuntu(Docker)} ——> GitHub

## Budget
AWS offers four kinds of pricing: On Demand, Spot Instances, Reserved Instances, and Dedicated Hosts, while Reserved Instances and Dedicated Hosts are purchased by package for 1-year or 3-year, so we just compare the price of On Demand and Spot Instances. 

| Instance Type | Name | vCPU | Memory (GiB) | On-Demand ($/hour) | (On-Demand ($/3mos) | Spot Instances- Linux/UNIX ($/hour) | Spot Instances- Linux/UNIX ($/3mos) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| General Purpose | t2.micro | 1 | 1 | 0.0116 | 25.404 | 0.0033 | 7.227 |
| General Purpose | t2.medium | 2 | 4 | 0.0464 | 101.616 | 0.0069 | 15.111 |
| Compute Optimized | c5.large | 2 | 4 | 0.085 | 186.15 | 0.0195 | 42.705 |

