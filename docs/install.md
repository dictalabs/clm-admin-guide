# **CLM Deployment Guide**

This guide provides a step-by-step walkthrough for installing **vScrawl** on an in-house server. The installation process is automated through a setup script but requires administrator input and consent at various stages. Follow this guide carefully to ensure a successful installation.

---

## Install on Single Evaluation Server
### **1. Minimum Requirements**


| Resource              | Specification      |
| --------------------- | ------------------ |
| **Operating System**  | Ubuntu 24.04.1 LTS |
| **CPU**               | 8 cores            |
| **Memory (RAM)**      | 16 GB              |
| **Disk Space**        | 200 GB             |
| **Required Software** | Docker, NGINX      |

---

### **2. Pre-Requisites Setup**

Before deploying CLM, ensure that the following tools and configurations are set up.

#### **Step 1: Install zip**

`sudo apt update && sudo apt install zip -y`

#### **Step 2: Unzip CLM Package**

Unzip the deployment package provided by your administrator.

`unzip clm-package.zip`

---

### **3. Docker Installation**

The CLM backend and frontend run as Docker containers. Follow the steps below to install Docker.

#### **Step 3.1: Add Docker’s Official GPG Key**

`sudo apt-get update`
`sudo apt-get install ca-certificates curl`
`sudo install -m 0755 -d /etc/apt/keyrings`
`sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc`
`sudo chmod a+r /etc/apt/keyrings/docker.asc`
#### **Step 3.2: Add Docker Repository**

`echo \   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \   https://download.docker.com/linux/ubuntu \   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
`sudo apt-get update`

#### **Step 3.3: Install Docker Engine and Compose**

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y`

#### **Step 3.4: Allow Docker to Run Without Sudo**

`sudo groupadd docker`
`sudo usermod -aG docker $USER`
`newgrp docker`

---

### **4. Install NGINX**

Install and configure **NGINX** as a reverse proxy for your CLM services.

`sudo apt install nginx -y`

---

### **5. DNS Configuration**

Create the following **DNS entries** for your CLM environment (replace YOURDOMAIN.com with your actual domain):

`scep.YOURDOMAIN.com`

`cmp.clm.YOURDOMAIN.com `

`acme.YOURDOMAIN.com`

`api.clm.YOURDOMAIN.com `

`app.clm.YOURDOMAIN.com`

`est.YOURDOMAIN.com`

---

### **6. NGINX Configuration**

### **Step 6.1: Configure NGINX Sites**

- Sample NGINX configuration files are provided in the NGINX-Sample-Files directory.
    
- Copy them to /etc/nginx/sites-available/.
    

`sudo cp NGINX-Sample-Files/* /etc/nginx/sites-available/`

- Edit each file and replace all instances of YOURDOMAIN with your actual domain.
    
- Create symbolic links to enable the sites:
    

`sudo ln -s /etc/nginx/sites-available/scep.YOURDOMAIN.com /etc/nginx/sites-enabled`

`sudo ln -s /etc/nginx/sites-available/cmp.clm.YOURDOMAIN.com /etc/nginx/sites-enabled`

`sudo ln -s /etc/nginx/sites-available/acme.YOURDOMAIN.com /etc/nginx/sites-enabled`

`sudo ln -s /etc/nginx/sites-available/api.clm.YOURDOMAIN.com /etc/nginx/sites-enabled`

`sudo ln -s /etc/nginx/sites-available/app.clm.YOURDOMAIN.com /etc/nginx/sites-enabled`

`sudo ln -s /etc/nginx/sites-available/est.YOURDOMAIN.com /etc/nginx/sites-enabled`


---

### **7. SSL Certificates Setup (Let’s Encrypt)**

#### **Step 7.1: Install Certbot**

`sudo apt install certbot python3-certbot-nginx -y`

#### **Step 7.2: Generate SSL Certificates**

Run the following commands for each domain:

`sudo certbot certonly --standalone -d scep.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d cmp.clm.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d acme.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d api.clm.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d app.clm.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d est.YOURDOMAIN.com`

#### **Step 7.3: Verify and Enable NGINX**

`sudo nginx -t sudo service nginx start`

---

### **8. CLM Deployment**

#### **Step 8.1: Frontend (Web Application)**

1. Navigate to the **clm-web-app** directory:
    
    `cd clm-web-app`
    
2. Update the config.json file with your **Backend API URL** (e.g., https://api.clm.YOURDOMAIN.com).
    
3. Pull and start the containers:
    
    `docker compose pull docker compose up -d`
    

---

#### **Step 8.2: Backend (Server Application)**

1. Navigate to the **clm-server** directory:
    
    `cd clm-server`
    
2. Edit the .env file to configure:
    
    - **URLs** for frontend, backend, and EJBCA
        
    - **Keycloak** authentication
        
    - **SMTP server** for email notifications
        
3. Pull and start backend containers:
    
    `docker compose pull docker compose up -d`
    

---

### **9. Verification**

After deployment:

- Access **CLM UI** at:  
    https://app.clm.YOURDOMAIN.com

## **Production Deployment**

### 1. Pre-Requisites
In order to deploy CLM on cloud following pre-requisites should be kept in mind.

- 6 DNS URL Entries for Frontend and Backend APIs.
    
- Application Load Balancers with SSL Offloading Capabilities
    
- SSL Certificates
    
- We will use [Example Domain](http://example.com/) as Domain name as reference for this deployment guide.
    
- One or more Frontend Servers (with Linux OS like Ubuntu and docker engine) needed behind the Application Load Balancer.
    
- One or more Backend Servers (with Linux OS like Ubuntu and docker engine and each backend server will run CLM API, ACME, SCEP, EST, CMP, Redis, Celery Worker, Celery Beat services) needed behind the Application Load Balancer.
    
- Postgres DB Cluster (or atleast one instance that will be used by Backend Servers)
    
- VPC/VNet on cloud service provider e.g., AWS/Azure/GCP (This guide is a generic guide for deployment)
    
- Each backend server and frontend server requires Docker Engine because frontend app and backend services would run inside Docker Containers.
    

### 2. License

A license file issue by product owner will be required for each separate backend server.

Each backend server will be running

- CLM API,
- ACME,
- SCEP,
- EST,
- CMP,
- Redis Cache,
- Celery Worker,
- Celery Beat

No license required for Postgresql Database instance or Frontend. License is required only for CLM API service to run.

### 3. VPC/VNet Creation on Cloud

First of all on any cloud you will first have to create a project on GCP/Hetzner or VPC/VNET on AWS/Azure

Yes — AWS now gives you an option to **automatically create a VPC** with **public and private subnets across multiple Availability Zones** using its **VPC Wizard** or the **“VPC with subnets”** template.

Here’s how you can use that **automatic deployment option** instead of creating everything manually 👇

---

### 4. **Automatic VPC Creation (Recommended for Quick Setup)**

#### Steps (AWS Console)

1. **Open AWS Management Console** → Go to **VPC Dashboard**  
    🔗 [https://console.aws.amazon.com/vpc](https://console.aws.amazon.com/vpc "https://console.aws.amazon.com/vpc")
    
2. In the left menu, click **Your VPCs** → then click **Create VPC**.
    
3. **Select “VPC and more”** (⚙️ this enables the wizard to create subnets, route tables, IGW, and optionally NAT Gateway automatically).
    
4. Under **Resources to create**, you’ll see:
    
    - VPC
        
    - 2 Availability Zones
        
    - Public and Private subnets
        
    - Internet Gateway
        
    - NAT Gateway (optional)
        
5. Configure the options:
    
    - **Name tag:** `my-vpc`
        
    - **IPv4 CIDR block:** `10.0.0.0/16`
        
    - **Number of Availability Zones:** `2`
        
    - **Number of public subnets:** `2`
        
    - **Number of private subnets:** `2`
        
    - **NAT gateways:** Choose “None” if you don’t need outbound internet access for private subnets, or “1 per AZ” for redundancy.
        
6. Optionally:
    
    - Keep **DNS hostnames** and **DNS resolution** enabled.
        
    - Review **CIDR allocations** for each subnet (AWS auto-assigns /24s, e.g. 10.0.0.0/24, 10.0.1.0/24, etc.).
        
7. Click **Create VPC**.
    

---

What AWS Automatically Creates

|                            |                                    |                                           |
| -------------------------- | ---------------------------------- | ----------------------------------------- |
| Resource                   | Example Name                       | Purpose                                   |
| **VPC**                    | `my-vpc`                           | 10.0.0.0/16 network                       |
| **Public Subnets**         | `PublicSubnet1`, `PublicSubnet2`   | For internet-facing resources             |
| **Private Subnets**        | `PrivateSubnet1`, `PrivateSubnet2` | For internal workloads                    |
| **Route Tables**           | `PublicRT`, `PrivateRT`            | Auto-routed accordingly                   |
| **Internet Gateway**       | `my-vpc-igw`                       | For public subnet internet access         |
| **NAT Gateway (optional)** | `my-vpc-natgw`                     | For outbound traffic from private subnets |

---

**Result**

In under a minute, AWS deploys a **fully working VPC network** with two public and two private subnets across two AZs, including routing and gateways.

Similarly you can create any VNet on Azure or VPC on GCP or cloud service provider of your choice.

### 5. Application Load Balancer

#### Requirements

1 Application Load Balancer required in public subnet for 1 DNS URL of Frontend

[app.clm.example.com](http://test.app.clm.dictalabs.com/ "http://test.app.clm.dictalabs.com").

5 Application Load Balancers required in public subnet for 5 DNS URLs

[api.clm.example.com](http://test.api.clm.dictalabs.com/ "http://test.api.clm.dictalabs.com").

[acme.clm.example.com](http://test.acme.clm.dictalabs.com/ "http://test.acme.clm.dictalabs.com").

[scep.clm.example.com](http://test.scep.clm.dictalabs.com/ "http://test.scep.clm.dictalabs.com").

[est.clm.example.com](http://test.est.clm.dictalabs.com/ "http://test.est.clm.dictalabs.com").

[cmp.clm.example.com](http://test.cmp.clm.dictalabs.com/ "http://test.cmp.clm.dictalabs.com").

Create 6 Application Load Balancers and 6 Target Groups

|                                                                                                   |                               |                              |
| ------------------------------------------------------------------------------------------------- | ----------------------------- | ---------------------------- |
| **DNS**                                                                                           | **Application Load Balancer** | **Target Group**             |
| [app.clm.example.com](http://test.app.clm.dictalabs.com/ "http://test.app.clm.dictalabs.com").    | Frontend ALB                  | Frontend Target Group        |
| [api.clm.example.com](http://test.api.clm.dictalabs.com/ "http://test.api.clm.dictalabs.com").    | Backend CLM API               | Backend CLM API Target Group |
| [acme.clm.example.com](http://test.acme.clm.dictalabs.com/ "http://test.acme.clm.dictalabs.com"). | Backend ACME API              | Backend ACME API             |
| [scep.clm.example.com](http://test.scep.clm.dictalabs.com/ "http://test.scep.clm.dictalabs.com"). | Backend SCEP API              | Backend SCEP API             |
| [est.clm.example.com](http://test.est.clm.dictalabs.com/ "http://test.est.clm.dictalabs.com").    | Backend EST API               | Backend EST API              |
| [cmp.clm.example.com](http://test.cmp.clm.dictalabs.com/ "http://test.cmp.clm.dictalabs.com").    | Backend CMP API               | Backend CMP API              |

#### Firewall/Security Groups

Allow TCP port 5432 from Backend Server IP network like 10.0.0.0/16

### Target Groups

Define 6 Target Groups for 6 URLs

### Frontend Target Group

As shown in Diagram frontend

Targets = Frontend Server

Port = 8080

Health Check Path

/login/health

### 6. Backend CLM API Target Group

As shown in Diagram Backend Server runs clm api service.

Targets = All CLM Backend Servers

Port = 8000

Health Check Path

/health

#### Backend ACME API

As shown in Diagram Backend Server runs acme service.

Targets = All CLM Backend Servers

Port = 8001

Health Check Path

/health

#### Backend SCEP API

As shown in Diagram Backend Server runs scep service.

Targets = All CLM Backend Servers

Port = 8002

Health Check Path

/health

#### Backend EST API

As shown in Diagram Backend Server runs est service.

Targets = All CLM Backend Servers

Port = 8003

Health Check Path

/health

#### Backend CMP API

As shown in Diagram Backend server runs cmp service.

Targets = All CLM Backend Servers

Port = 18080

Health Check Path

/health

### 7. Endpoint URLs

Note Down the Endpoint URLs of Application Load Balancers.

DNS Entries

Create 6 DNS entries on your DNS registrar (e.g., godaddy, namecheap)

|                                                                                                   |                                           |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| **DNS CNAME**                                                                                     | **Endpoint of Application Load Balancer** |
| [app.clm.example.com](http://test.app.clm.dictalabs.com/ "http://test.app.clm.dictalabs.com").    | Endpoint DNS name of Frontend ALB         |
| [api.clm.example.com](http://test.api.clm.dictalabs.com/ "http://test.api.clm.dictalabs.com").    | Endpoint DNS name of Backend CLM API ALB  |
| [acme.clm.example.com](http://test.acme.clm.dictalabs.com/ "http://test.acme.clm.dictalabs.com"). | Endpoint DNS name of Backend ACME API     |
| [scep.clm.example.com](http://test.scep.clm.dictalabs.com/ "http://test.scep.clm.dictalabs.com"). | Endpoint DNS name of Backend SCEP API     |
| [est.clm.example.com](http://test.est.clm.dictalabs.com/ "http://test.est.clm.dictalabs.com").    | Endpoint DNS name of Backend EST API      |
| [cmp.clm.example.com](http://test.cmp.clm.dictalabs.com/ "http://test.cmp.clm.dictalabs.com").    | Endpoint DNS name of Backend CMP API      |

OR

In case your Application Load Balancers use Public IP addresses instead of DNS names.

|                                                                                                   |                                           |
| ------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| **DNS A record**                                                                                  | **Endpoint of Application Load Balancer** |
| [app.clm.example.com](http://test.app.clm.dictalabs.com/ "http://test.app.clm.dictalabs.com").    | Public IP Address of Frontend ALB         |
| [api.clm.example.com](http://test.api.clm.dictalabs.com/ "http://test.api.clm.dictalabs.com").    | Public IP Address of Backend CLM API ALB  |
| [acme.clm.example.com](http://test.acme.clm.dictalabs.com/ "http://test.acme.clm.dictalabs.com"). | Public IP Address of Backend ACME API     |
| [scep.clm.example.com](http://test.scep.clm.dictalabs.com/ "http://test.scep.clm.dictalabs.com"). | Public IP Address of Backend SCEP API     |
| [est.clm.example.com](http://test.est.clm.dictalabs.com/ "http://test.est.clm.dictalabs.com").    | Public IP Address of Backend EST API      |
| [cmp.clm.example.com](http://test.cmp.clm.dictalabs.com/ "http://test.cmp.clm.dictalabs.com").    | Public IP Address of Backend CMP API      |

### 8. Postgres Database Setup

For Postgres Setup create a DB instance with a

- clm user
    
- clm password
    
- clm db
    
- Firewall/Security Group allowed ports = TCP 5432
    

Note:

In this guide we will use

clm_user,

clm_password,

clm_db and

default Postgres port 5432

If your cloud provider just creates a postgres instance with default username postgres, database postgres with some password you set. Then you will have to install a psql client on your machine or a cloud server.

#### DB User and DB Creation

 Step-by-Step (via `psql`)

 1. Connect to your PostgreSQL server as an admin (e.g., `postgres` or master user)

	`psql -h psql -h database-instance-IP-addres -U postgres -d postgres -U postgres`

	 by default postgres db instance has a default user 'postgres'

2. Create the database

	 `CREATE DATABASE clm_db;`

3. Create the user and assign password

	`CREATE USER clm_user WITH PASSWORD 'clm_password';`

4. Grant privileges on the database

	 `GRANT ALL PRIVILEGES ON DATABASE clm_db TO clm_user;`

 5. (Optional but Recommended) Make `clm_user` the owner of that database

	 `ALTER DATABASE clm_db OWNER TO clm_user;`

 6. Verify

List Databases

`\l`

 List roles/users:

`\du`

 7. Exit `psql` 

	`\q`

Now test login

 From CLI:

`psql -h database-instance-IP-addres -U clm_user -d clm_db`

Enter password for example in this guide we are using “clm_password”.

### 9. Frontend Server Deployment

System Requirements

- OS = Ubuntu 24.04LTS or Ubuntu 22.04LTS
    
- vCPUs = 2
    
- Memory = 4GB
    
- Storage = 10GB
    
- Firewall/Security Group allowed ports = SSH, TCP port 8080
    

### 10. High Availability

It is recommended to have atleast two frontend servers in different availability zones on (Cloud Service Provider of your choice) for hight availability.  
  
### Pre-requisite Configuration

On each Frontend Ubuntu Server  
Install Docker using

```bash
########## INSTALLATION############
# https://docs.docker.com/engine/install/ubuntu/

# Step-1:
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

# Step-2:
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

##### After Installation #######

### Run docker commands without sudo
# https://docs.docker.com/engine/install/linux-postinstall/

sudo groupadd docker
# Add user to docker group
sudo usermod -aG docker $USER
```


Install zip

`apt install zip -y`

### 11. Backend Server Deployment

System Requirements

- OS = Ubuntu 24.04LTS or Ubuntu 22.04LTS
    
- vCPUs = 4
    
- Memory = 8GB
    
- Storage = 15GB
    
- Firewall/Security Group allowed ports = SSH, TCP ports 8000, 8001, 8002, 8003, 18080

### 12. High Availability

It is recommended to have at least two backend servers in different availability zones on (Cloud Service Provider of your choice) for high availability.

#### Pre-requisite Configuration

On each backend Ubuntu Server  
Install Docker using

```bash
########## INSTALLATION############
# https://docs.docker.com/engine/install/ubuntu/

# Step-1:
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

# Step-2:
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

##### After Installation #######

### Run docker commands without sudo
# https://docs.docker.com/engine/install/linux-postinstall/

sudo groupadd docker
# Add user to docker group
sudo usermod -aG docker $USER
```


Install zip

`apt install zip -y`

### 13. Register Servers to Target Groups

Register Servers to Target Groups of ALBs (Application Load Balancers)

Register frontend servers to Frontend Target Group.

Register all backend servers in each Backend Target Group. Same backend servers will be part of all Backend Target Groups.

### 14. Frontend Server Application Configuration

#### CLM Web App Package

Copy clm-web-app.zip to server for example in /home/clm-frontend using SCP with any tool like winSCP.

unzip clm-web-app.zip

go to directory clm-web-app

#### Environment Variables

edit config.json and write backend api domain.

```{ "API_BASE_URL": "https://api.clm.example.com/api/v1", "PUBLIC_API_BASE_URL": "https://api.clm.example.com" }`

run the following command

`docker compose up -d`

The docker container will start working.

The backend services docker container will start working. You can verify as well.

`docker ps -a`

To view logs of a backend service you can use following command and to stop view logs you will use **ctrl+c**

`docker logs -f clm-web-app`

### 14. Backend Server Application Configuration

### CLM Server Package

Copy clm-package.zip to server for example in /home/clm-backend using SCP with any tool like winSCP.

unzip clm-package.zip

go to directory clm-package

### Licensing

`sudo su chmod u+x ./get-system-uuid.sh`

You will get system-uuid like this

**OS ID: deff8676-a68f-4662-a30b-c7485e3e3ff9**  
**Share this OS ID with Dictalabs to acquire license file for this deployment.**

Share this OS ID with Dictalabs to acquire license file **license.lic.xml** for this server and copy license file in the same directory.

#### Environment Varialbles

edit .env and set Environment variables.

Set Postgres DB URL

for example we are using here Postgres endpoint dns name as

[pg-db.ch826uia61kw.eu-west-2.rds.amazonaws.com](http://pg-db.ch826uia61kw.eu-west-2.rds.amazonaws.com/ "http://pg-db.ch826uia61kw.eu-west-2.rds.amazonaws.com")

`DATABASE_URL=postgresql+asyncpg://clm_user:clm_password@pg-db.ch826uia61kw.eu-west-2.rds.amazonaws.com:5432/clm_db`

Set SMTP configuration

`SMTP_SERVER=mail.example.com SMTP_PORT=587 SENDER_EMAIL=notify@example.com SENDER_PASSWORD=P@ssword123. SMTP_USE_TLS=true SMTP_USE_SSL=false`

Set Keycloak parameters provided by Keycloak service provider (for example Dictalabs provided Keycloak).

`KEYCLOAK_BASE_URL=https://keycloak.example.com:8443 KEYCLOAK_REALM=clm KEYCLOAK_CLIENT_ID=clm KEYCLOAK_CLIENT_SECRET= add client secret`

Set EJBCA connector URL (for example Dictalabs provided EJBCA)

`EJBCA_CONNECTOR=http://144.126.224.110:8004`

To run all the backend services run the following command

`docker compose up -d`

The backend services docker container will start working. You can verify as well.

`docker ps -a`

To view logs of a backend service you can use following command and to stop view logs you will use **ctrl+c**

```bash
docker logs -f clm_api
docker logs -f est_server
docker logs -f acme_server
docker logs -f redis
docker logs -f scep_server
docker logs -f cmp_server
docker logs -f celery_beat
docker logs -f celery_worker
```