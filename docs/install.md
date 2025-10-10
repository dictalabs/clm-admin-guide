# **CLM Deployment Guide**

This guide provides a step-by-step walkthrough for installing **vScrawl** on an in-house server. The installation process is automated through a setup script but requires administrator input and consent at various stages. Follow this guide carefully to ensure a successful installation.

---

## **1. System Requirements**


| Resource              | Specification      |
| --------------------- | ------------------ |
| **Operating System**  | Ubuntu 24.04.1 LTS |
| **CPU**               | 8 cores            |
| **Memory (RAM)**      | 16 GB              |
| **Disk Space**        | 200 GB             |
| **Required Software** | Docker, NGINX      |

---

## **2. Pre-Requisites Setup**

Before deploying CLM, ensure that the following tools and configurations are set up.

### **Step 1: Install zip**

`sudo apt update && sudo apt install zip -y`

### **Step 2: Unzip CLM Package**

Unzip the deployment package provided by your administrator.

`unzip clm-package.zip`

---

## **3. Docker Installation**

The CLM backend and frontend run as Docker containers. Follow the steps below to install Docker.

### **Step 3.1: Add Docker’s Official GPG Key**

`sudo apt-get update`
`sudo apt-get install ca-certificates curl`
`sudo install -m 0755 -d /etc/apt/keyrings`
`sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc`
`sudo chmod a+r /etc/apt/keyrings/docker.asc`
### **Step 3.2: Add Docker Repository**

`echo \   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \   https://download.docker.com/linux/ubuntu \   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
`sudo apt-get update`

### **Step 3.3: Install Docker Engine and Compose**

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y`

### **Step 3.4: Allow Docker to Run Without Sudo**

`sudo groupadd docker`
`sudo usermod -aG docker $USER`
`newgrp docker`

---

## **4. Install NGINX**

Install and configure **NGINX** as a reverse proxy for your CLM services.

`sudo apt install nginx -y`

---

## **5. DNS Configuration**

Create the following **DNS entries** for your CLM environment (replace YOURDOMAIN.com with your actual domain):

`scep.YOURDOMAIN.com`

`cmp.clm.YOURDOMAIN.com `

`acme.YOURDOMAIN.com`

`api.clm.YOURDOMAIN.com `

`app.clm.YOURDOMAIN.com`

`est.YOURDOMAIN.com`

---

## **6. NGINX Configuration**

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

## **7. SSL Certificates Setup (Let’s Encrypt)**

### **Step 7.1: Install Certbot**

`sudo apt install certbot python3-certbot-nginx -y`

### **Step 7.2: Generate SSL Certificates**

Run the following commands for each domain:

`sudo certbot certonly --standalone -d scep.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d cmp.clm.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d acme.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d api.clm.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d app.clm.YOURDOMAIN.com`

`sudo certbot certonly --standalone -d est.YOURDOMAIN.com`

### **Step 7.3: Verify and Enable NGINX**

`sudo nginx -t sudo service nginx start`

---

## **8. CLM Deployment**

### **Step 8.1: Frontend (Web Application)**

1. Navigate to the **clm-web-app** directory:
    
    `cd clm-web-app`
    
2. Update the config.json file with your **Backend API URL** (e.g., https://api.clm.YOURDOMAIN.com).
    
3. Pull and start the containers:
    
    `docker compose pull docker compose up -d`
    

---

### **Step 8.2: Backend (Server Application)**

1. Navigate to the **clm-server** directory:
    
    `cd clm-server`
    
2. Edit the .env file to configure:
    
    - **URLs** for frontend, backend, and EJBCA
        
    - **Keycloak** authentication
        
    - **SMTP server** for email notifications
        
3. Pull and start backend containers:
    
    `docker compose pull docker compose up -d`
    

---

## **9. Verification**

After deployment:

- Access **CLM UI** at:  
    https://app.clm.YOURDOMAIN.com