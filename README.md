## Herovired_MERN-stack

Graded Project on Travel Memory Application Deployment Submission

Architecture diagram : 

1. VPC
2. Private subent and Publice Subnet
3. NAT Gateways
4. Internet gateways
5. LB
6. Target Goups
7. Auto scaling Group

<img width="547" alt="image" src="https://github.com/user-attachments/assets/38aa2772-5fdf-4910-8a20-873c7f0e2d1d">

# CIDR

![image](https://github.com/user-attachments/assets/2804772f-bd82-440a-b279-2f43b3e9352a)

1. VPC 

![image](https://github.com/user-attachments/assets/6603c558-1ae8-4484-ae58-ee4f28b4edf0)

2. creat Subnets 

![image](https://github.com/user-attachments/assets/0aa157e8-7492-45bf-a6dd-81df6224f303)

3. Creat IGW and Attch to VPC

![image](https://github.com/user-attachments/assets/8a7dbc54-c1a7-414a-9c40-2d0de1b8823b)

![image](https://github.com/user-attachments/assets/42b04eb6-3581-4180-a1a5-c24c42663bfb)

4. Creat NAT Gateways

![image](https://github.com/user-attachments/assets/722760a9-167a-4ce2-af61-a7f4d62d16dd)

![image](https://github.com/user-attachments/assets/95c08921-4443-49c6-b580-a368214e968b)

5. Creat Elastic IP 

![image](https://github.com/user-attachments/assets/af2170e7-043b-40ac-b0e9-e8816097b4d5)

6. Create Route Tables of Public and Private 

![image](https://github.com/user-attachments/assets/72025039-ebec-41b9-8d94-82eb0753895c)

7. For Public Route Table add IGW in Route and Add Public subnets in Subnet Assocation as attched below 

![image](https://github.com/user-attachments/assets/d93d8165-f90a-4816-b1b2-ea13285783cc)

![image](https://github.com/user-attachments/assets/ef026541-b055-4cc7-8ebe-fef540814801)

![image](https://github.com/user-attachments/assets/0548c1a1-81a3-47af-a7fd-7d60f53b8418)


7. For Private Route Table add Nat agteways in Route and Add Private subnets in Subnet Assocation as attched below 

![image](https://github.com/user-attachments/assets/942efbe0-e24e-4ebf-be80-9a3a2472a908)

![image](https://github.com/user-attachments/assets/41979bde-297c-40c0-96e2-99af5c8737de)

8. Creat SG for Public subnets and allow below ports

![image](https://github.com/user-attachments/assets/a0d26ba1-bbf6-4f70-a176-df67008e05c1)

9. Create SG for Private Subnets and allow Public SG in  Inbound Rules

![image](https://github.com/user-attachments/assets/1bbaae8d-2525-46a1-ac99-66d9cbd3e6b1)

10. Lunch EC2 Instant in Public subnet

<img width="776" alt="image" src="https://github.com/user-attachments/assets/bb64c919-367b-4813-998d-f111f829a118">


4. **Update and upgrade EC2 and Install Nginx**
   - Update :
     ```bash
     apt-get update
     ```

   - Upgrade :
     ```bash
     apt-get upgrade -y
     ```

   - Install Nginx :
     ```bash
     apt-get install nginx
     ```

   - check Nginx status:
     ```bash
     systemctl status nginx
     ```

   - check Nginx status :
     ```bash
     systemctl status nginx
     ```

## Task  - Backend Configuration
- Clone the repository and navigate to the backend directory.
- The backend runs on port 3000. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.
- Update the .env file to incorporate database connection details and port information.

### Solution
The steps to task 1 solution are as follows:
1. Clone the repository and navigate to the backend directory.
   - Clone the repo  :
     ```bash
     git clone https://github.com/MrSRE/herovired_MERN-stack.git
     ```
2. Add below MongoDB confugirations to .env file
   - Clone the repo  :
     ```bash
     cd frontend
     vi .env

     # add below 
     MONGO_URI=mongodb://localhost:27017/herovired # NOTE : 'mongodb+srv://pkgajula1997:xxxxxxxx@travel1.heava.mongodb.net/xxxxxy_pavan'
     PORT=3001
     ```

3. Install the required packages using npm install.
   - Install Node js and npm :
    ```bash
      apt install nodejs -y 
      node --version # check node version
      apt install npm -y
      npm --version # check npm version
    ```

4. The Nginx configuration is as follows:
   - Edit config file  :
    ```bash
      vi /etc/nginx/sites-available/default
    ```
   - Add below configuration # Note add you LB and Cname
    ```
    server {
        listen 80;
        server_name travel.close.today Travel-memory-LB-1524939564.us-west-2.elb.amazonaws.com ;
    
        location / {
            proxy_pass http://localhost:3000/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        # Serve static assets (JavaScript, CSS, etc.) from the frontend container
        location /static/ {
            proxy_pass http://localhost:3000/static/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        location /backend/ {
            proxy_pass http://localhost:3001/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
    
            # Disable caching for this location
            proxy_buffering off;
        }
    }

    ```

5. start application  
   - start  :
     ```bash
     npm install
     ```

6. Run  application as backend process  
   - Run  :
     ```bash
        node index.js
        nohup node index.js > output.log 2>&1 & # backend run
     ```

## Task  - Frondend Configuration
- Clone the repository and navigate to the backend directory.
- The backend runs on port 3001. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.

    
### Solution
The steps to task 1 solution are as follows:
1. Clone the repository and navigate to the backend directory.
   - Clone the repo  :
     ```bash
     git clone https://github.com/MrSRE/herovired_MERN-stack.git
     ```

2. Update the url.js file to incorporate database connection details and port information.
     ```bash
     cd TravelMemory
     cd frontend
     cd src
     vi url.js
     # inster and Add below 
     export const baseUrl = process.env.REACT_APP_BACKEND_URL || "http://localhost:3001";
     ```

3. start application  
   - start  :
     ```bash
     npm install
     ```

6. Run  application as backend process  
   - Run  :
     ```bash
      npm start
     ``` 

## Access the Application with below Public IP , LB and CNAME
1. Public IP : 54.201.99.82

<img width="955" alt="image" src="https://github.com/user-attachments/assets/185294c3-c80d-47ab-a2c2-27e413caf6cd">

2. LB - http://travel-memory-lb-1524939564.us-west-2.elb.amazonaws.com/addexperience

<img width="959" alt="image" src="https://github.com/user-attachments/assets/9a5a8092-a9ad-461a-bed7-95ece4880848">

3. DNS - https://travel.close.today/addexperience

<img width="959" alt="image" src="https://github.com/user-attachments/assets/35f48174-96a1-40f5-8072-67068a822db9">

## Task - Scaling the Application
  Create multiple instances of both the frontend and backend servers.
  Add these instances to a load balancer to ensure efficient distribution of incoming traffic.

- Create Launch Template  for backend servers

<img width="774" alt="image" src="https://github.com/user-attachments/assets/c7ce7556-9a12-4ec4-94c2-87ea134f394d">

- Create Auto Scaling Gorup from Above Launch Templates
<img width="793" alt="image" src="https://github.com/user-attachments/assets/ff9463f8-f9b0-4a04-8cdc-13cd8a4d3a5e">

- Details - 
  - Desired capacity  - 2
  - Minimum capacity  - 1
  - Maximum capacity  - 3
  - Scaling policy - Step Scaling - CPU utilization - 50 - 100 - 1 -

<img width="768" alt="image" src="https://github.com/user-attachments/assets/12e8da78-8d57-462a-acc7-88f2f44159ab">


### Solution
- The  backend servers are contained accross 2 instances for test:

<img width="782" alt="image" src="https://github.com/user-attachments/assets/5651e89c-146a-4304-8c09-9fa9b7815d92">

- Added these instances to my target group. I have named my target group as 
<img width="743" alt="image" src="https://github.com/user-attachments/assets/b07bbccf-a9ec-44ea-811c-5e8b25ef536b">

- My load balancer to point to Target group  is as follows:
<img width="748" alt="image" src="https://github.com/user-attachments/assets/64ae8e34-55e2-4ec0-8623-7fd48fccfc81">

## Task  - Domain Setup with Cloudflare
- Connect your custom domain to the application using Cloudflare.

# herovired_MERN-stack

Graded Project on Travel Memory Application Deployment Submission

Architecture diagram : 

1. VPC
2. Private subent and Publice Subnet
3. NAT Gateways
4. Internet gateways
5. LB
6. Target Goups
7. Auto scaling Group

<img width="547" alt="image" src="https://github.com/user-attachments/assets/38aa2772-5fdf-4910-8a20-873c7f0e2d1d">

# CIDR

![image](https://github.com/user-attachments/assets/2804772f-bd82-440a-b279-2f43b3e9352a)

1. VPC 

![image](https://github.com/user-attachments/assets/6603c558-1ae8-4484-ae58-ee4f28b4edf0)

2. creat Subnets 

![image](https://github.com/user-attachments/assets/0aa157e8-7492-45bf-a6dd-81df6224f303)

3. Creat IGW and Attch to VPC

![image](https://github.com/user-attachments/assets/8a7dbc54-c1a7-414a-9c40-2d0de1b8823b)

![image](https://github.com/user-attachments/assets/42b04eb6-3581-4180-a1a5-c24c42663bfb)

4. Creat NAT Gateways

![image](https://github.com/user-attachments/assets/722760a9-167a-4ce2-af61-a7f4d62d16dd)

![image](https://github.com/user-attachments/assets/95c08921-4443-49c6-b580-a368214e968b)

5. Creat Elastic IP 

![image](https://github.com/user-attachments/assets/af2170e7-043b-40ac-b0e9-e8816097b4d5)

6. Create Route Tables of Public and Private 

![image](https://github.com/user-attachments/assets/72025039-ebec-41b9-8d94-82eb0753895c)

7. For Public Route Table add IGW in Route and Add Public subnets in Subnet Assocation as attched below 

![image](https://github.com/user-attachments/assets/d93d8165-f90a-4816-b1b2-ea13285783cc)

![image](https://github.com/user-attachments/assets/ef026541-b055-4cc7-8ebe-fef540814801)

![image](https://github.com/user-attachments/assets/0548c1a1-81a3-47af-a7fd-7d60f53b8418)


7. For Private Route Table add Nat agteways in Route and Add Private subnets in Subnet Assocation as attched below 

![image](https://github.com/user-attachments/assets/942efbe0-e24e-4ebf-be80-9a3a2472a908)

![image](https://github.com/user-attachments/assets/41979bde-297c-40c0-96e2-99af5c8737de)

8. Creat SG for Public subnets and allow below ports

![image](https://github.com/user-attachments/assets/a0d26ba1-bbf6-4f70-a176-df67008e05c1)

9. Create SG for Private Subnets and allow Public SG in  Inbound Rules

![image](https://github.com/user-attachments/assets/1bbaae8d-2525-46a1-ac99-66d9cbd3e6b1)

10. Lunch EC2 Instant in Public subnet

<img width="776" alt="image" src="https://github.com/user-attachments/assets/bb64c919-367b-4813-998d-f111f829a118">


4. **Update and upgrade EC2 and Install Nginx**
   - Update :
     ```bash
     apt-get update
     ```

   - Upgrade :
     ```bash
     apt-get upgrade -y
     ```

   - Install Nginx :
     ```bash
     apt-get install nginx
     ```

   - check Nginx status:
     ```bash
     systemctl status nginx
     ```

   - check Nginx status :
     ```bash
     systemctl status nginx
     ```

## Task  - Backend Configuration
- Clone the repository and navigate to the backend directory.
- The backend runs on port 3000. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.
- Update the .env file to incorporate database connection details and port information.

### Solution
The steps to task 1 solution are as follows:
1. Clone the repository and navigate to the backend directory.
   - Clone the repo  :
     ```bash
     git clone https://github.com/MrSRE/herovired_MERN-stack.git
     ```
2. Add below MongoDB confugirations to .env file
   - Clone the repo  :
     ```bash
     cd frontend
     vi .env

     # add below 
     MONGO_URI=mongodb://localhost:27017/herovired # NOTE : 'mongodb+srv://pkgajula1997:xxxxxxxx@travel1.heava.mongodb.net/xxxxxy_pavan'
     PORT=3001
     ```

3. Install the required packages using npm install.
   - Install Node js and npm :
    ```bash
      apt install nodejs -y 
      node --version # check node version
      apt install npm -y
      npm --version # check npm version
    ```

4. The Nginx configuration is as follows:
   - Edit config file  :
    ```bash
      vi /etc/nginx/sites-available/default
    ```
   - Add below configuration # Note add you LB and Cname
    ```
    server {
        listen 80;
        server_name travel.close.today Travel-memory-LB-1524939564.us-west-2.elb.amazonaws.com ;
    
        location / {
            proxy_pass http://localhost:3000/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        # Serve static assets (JavaScript, CSS, etc.) from the frontend container
        location /static/ {
            proxy_pass http://localhost:3000/static/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        location /backend/ {
            proxy_pass http://localhost:3001/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
    
            # Disable caching for this location
            proxy_buffering off;
        }
    }

    ```

5. start application  
   - start  :
     ```bash
     npm install
     ```

6. Run  application as backend process  
   - Run  :
     ```bash
        node index.js
        nohup node index.js > output.log 2>&1 & # backend run
     ```

## Task  - Frondend Configuration
- Clone the repository and navigate to the backend directory.
- The backend runs on port 3001. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.

    
### Solution
The steps to task 1 solution are as follows:
1. Clone the repository and navigate to the backend directory.
   - Clone the repo  :
     ```bash
     git clone https://github.com/MrSRE/herovired_MERN-stack.git
     ```

2. Update the url.js file to incorporate database connection details and port information.
     ```bash
     cd TravelMemory
     cd frontend
     cd src
     vi url.js
     # inster and Add below 
     export const baseUrl = process.env.REACT_APP_BACKEND_URL || "http://localhost:3001";
     ```

3. start application  
   - start  :
     ```bash
     npm install
     ```

6. Run  application as backend process  
   - Run  :
     ```bash
      npm start
     ``` 

## Access the Application with below Public IP , LB and CNAME
1. Public IP : 54.201.99.82

<img width="955" alt="image" src="https://github.com/user-attachments/assets/185294c3-c80d-47ab-a2c2-27e413caf6cd">

2. LB - http://travel-memory-lb-1524939564.us-west-2.elb.amazonaws.com/addexperience

<img width="959" alt="image" src="https://github.com/user-attachments/assets/9a5a8092-a9ad-461a-bed7-95ece4880848">

3. DNS - https://travel.close.today/addexperience

<img width="959" alt="image" src="https://github.com/user-attachments/assets/35f48174-96a1-40f5-8072-67068a822db9">

## Task - Scaling the Application
  Create multiple instances of both the frontend and backend servers.
  Add these instances to a load balancer to ensure efficient distribution of incoming traffic.

- Create Launch Template  for backend servers

<img width="774" alt="image" src="https://github.com/user-attachments/assets/c7ce7556-9a12-4ec4-94c2-87ea134f394d">

- Create Auto Scaling Gorup from Above Launch Templates
<img width="793" alt="image" src="https://github.com/user-attachments/assets/ff9463f8-f9b0-4a04-8cdc-13cd8a4d3a5e">

- Details - 
  - Desired capacity  - 2
  - Minimum capacity  - 1
  - Maximum capacity  - 3
  - Scaling policy - Step Scaling - CPU utilization - 50 - 100 - 1 -

<img width="768" alt="image" src="https://github.com/user-attachments/assets/12e8da78-8d57-462a-acc7-88f2f44159ab">


### Solution
- The  backend servers are contained accross 2 instances for test:

<img width="782" alt="image" src="https://github.com/user-attachments/assets/5651e89c-146a-4304-8c09-9fa9b7815d92">

- Added these instances to my target group. I have named my target group as 
<img width="743" alt="image" src="https://github.com/user-attachments/assets/b07bbccf-a9ec-44ea-811c-5e8b25ef536b">

- My load balancer to point to Target group  is as follows:
<img width="748" alt="image" src="https://github.com/user-attachments/assets/64ae8e34-55e2-4ec0-8623-7fd48fccfc81">

## Task  - Domain Setup with Cloudflare
- Connect your custom domain to the application using Cloudflare.
- Create a CNAME record pointing to the load balancer endpoint.
- Set up an A record with the IP address of the EC2 instance hosting the front end.
### Solution
*I was unable to purchase a cloudflare domain or transfer my GoDaddy domain to cloudflare so I will be setting DNS via GoDaddy Panel.*
1. My GoDaddy domain DNS setting is as follows for adding ELB DNS to my domain records:

2. After successful DNS addition the domain is as follows:

# herovired_MERN-stack

Graded Project on Travel Memory Application Deployment Submission

Architecture diagram : 

1. VPC
2. Private subent and Publice Subnet
3. NAT Gateways
4. Internet gateways
5. LB
6. Target Goups
7. Auto scaling Group

<img width="547" alt="image" src="https://github.com/user-attachments/assets/38aa2772-5fdf-4910-8a20-873c7f0e2d1d">

# CIDR

![image](https://github.com/user-attachments/assets/2804772f-bd82-440a-b279-2f43b3e9352a)

1. VPC 

![image](https://github.com/user-attachments/assets/6603c558-1ae8-4484-ae58-ee4f28b4edf0)

2. creat Subnets 

![image](https://github.com/user-attachments/assets/0aa157e8-7492-45bf-a6dd-81df6224f303)

3. Creat IGW and Attch to VPC

![image](https://github.com/user-attachments/assets/8a7dbc54-c1a7-414a-9c40-2d0de1b8823b)

![image](https://github.com/user-attachments/assets/42b04eb6-3581-4180-a1a5-c24c42663bfb)

4. Creat NAT Gateways

![image](https://github.com/user-attachments/assets/722760a9-167a-4ce2-af61-a7f4d62d16dd)

![image](https://github.com/user-attachments/assets/95c08921-4443-49c6-b580-a368214e968b)

5. Creat Elastic IP 

![image](https://github.com/user-attachments/assets/af2170e7-043b-40ac-b0e9-e8816097b4d5)

6. Create Route Tables of Public and Private 

![image](https://github.com/user-attachments/assets/72025039-ebec-41b9-8d94-82eb0753895c)

7. For Public Route Table add IGW in Route and Add Public subnets in Subnet Assocation as attched below 

![image](https://github.com/user-attachments/assets/d93d8165-f90a-4816-b1b2-ea13285783cc)

![image](https://github.com/user-attachments/assets/ef026541-b055-4cc7-8ebe-fef540814801)

![image](https://github.com/user-attachments/assets/0548c1a1-81a3-47af-a7fd-7d60f53b8418)


7. For Private Route Table add Nat agteways in Route and Add Private subnets in Subnet Assocation as attched below 

![image](https://github.com/user-attachments/assets/942efbe0-e24e-4ebf-be80-9a3a2472a908)

![image](https://github.com/user-attachments/assets/41979bde-297c-40c0-96e2-99af5c8737de)

8. Creat SG for Public subnets and allow below ports

![image](https://github.com/user-attachments/assets/a0d26ba1-bbf6-4f70-a176-df67008e05c1)

9. Create SG for Private Subnets and allow Public SG in  Inbound Rules

![image](https://github.com/user-attachments/assets/1bbaae8d-2525-46a1-ac99-66d9cbd3e6b1)

10. Lunch EC2 Instant in Public subnet

<img width="776" alt="image" src="https://github.com/user-attachments/assets/bb64c919-367b-4813-998d-f111f829a118">


4. **Update and upgrade EC2 and Install Nginx**
   - Update :
     ```bash
     apt-get update
     ```

   - Upgrade :
     ```bash
     apt-get upgrade -y
     ```

   - Install Nginx :
     ```bash
     apt-get install nginx
     ```

   - check Nginx status:
     ```bash
     systemctl status nginx
     ```

   - check Nginx status :
     ```bash
     systemctl status nginx
     ```

## Task  - Backend Configuration
- Clone the repository and navigate to the backend directory.
- The backend runs on port 3000. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.
- Update the .env file to incorporate database connection details and port information.

### Solution
The steps to task 1 solution are as follows:
1. Clone the repository and navigate to the backend directory.
   - Clone the repo  :
     ```bash
     git clone https://github.com/MrSRE/herovired_MERN-stack.git
     ```
2. Add below MongoDB confugirations to .env file
   - Clone the repo  :
     ```bash
     cd frontend
     vi .env

     # add below 
     MONGO_URI=mongodb://localhost:27017/herovired # NOTE : 'mongodb+srv://pkgajula1997:xxxxxxxx@travel1.heava.mongodb.net/xxxxxy_pavan'
     PORT=3001
     ```

3. Install the required packages using npm install.
   - Install Node js and npm :
    ```bash
      apt install nodejs -y 
      node --version # check node version
      apt install npm -y
      npm --version # check npm version
    ```

4. The Nginx configuration is as follows:
   - Edit config file  :
    ```bash
      vi /etc/nginx/sites-available/default
    ```
   - Add below configuration # Note add you LB and Cname
    ```
    server {
        listen 80;
        server_name travel.close.today Travel-memory-LB-1524939564.us-west-2.elb.amazonaws.com ;
    
        location / {
            proxy_pass http://localhost:3000/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        # Serve static assets (JavaScript, CSS, etc.) from the frontend container
        location /static/ {
            proxy_pass http://localhost:3000/static/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        location /backend/ {
            proxy_pass http://localhost:3001/;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
    
            # Disable caching for this location
            proxy_buffering off;
        }
    }

    ```

5. start application  
   - start  :
     ```bash
     npm install
     ```

6. Run  application as backend process  
   - Run  :
     ```bash
        node index.js
        nohup node index.js > output.log 2>&1 & # backend run
     ```

## Task  - Frondend Configuration
- Clone the repository and navigate to the backend directory.
- The backend runs on port 3001. Set up a reverse proxy using nginx to ensure smooth deployment on EC2.

    
### Solution
The steps to task 1 solution are as follows:
1. Clone the repository and navigate to the backend directory.
   - Clone the repo  :
     ```bash
     git clone https://github.com/MrSRE/herovired_MERN-stack.git
     ```

2. Update the url.js file to incorporate database connection details and port information.
     ```bash
     cd TravelMemory
     cd frontend
     cd src
     vi url.js
     # inster and Add below 
     export const baseUrl = process.env.REACT_APP_BACKEND_URL || "http://localhost:3001";
     ```

3. start application  
   - start  :
     ```bash
     npm install
     ```

6. Run  application as backend process  
   - Run  :
     ```bash
      npm start
     ``` 

## Access the Application with below Public IP , LB and CNAME
1. Public IP : 54.201.99.82

<img width="955" alt="image" src="https://github.com/user-attachments/assets/185294c3-c80d-47ab-a2c2-27e413caf6cd">

2. LB - http://travel-memory-lb-1524939564.us-west-2.elb.amazonaws.com/addexperience

<img width="959" alt="image" src="https://github.com/user-attachments/assets/9a5a8092-a9ad-461a-bed7-95ece4880848">

3. DNS - https://travel.close.today/addexperience

<img width="959" alt="image" src="https://github.com/user-attachments/assets/35f48174-96a1-40f5-8072-67068a822db9">

## Task - Scaling the Application
  Create multiple instances of both the frontend and backend servers.
  Add these instances to a load balancer to ensure efficient distribution of incoming traffic.

- Create Launch Template  for backend servers

<img width="774" alt="image" src="https://github.com/user-attachments/assets/c7ce7556-9a12-4ec4-94c2-87ea134f394d">

- Create Auto Scaling Gorup from Above Launch Templates
<img width="793" alt="image" src="https://github.com/user-attachments/assets/ff9463f8-f9b0-4a04-8cdc-13cd8a4d3a5e">

- Details - 
  - Desired capacity  - 2
  - Minimum capacity  - 1
  - Maximum capacity  - 3
  - Scaling policy - Step Scaling - CPU utilization - 50 - 100 - 1 -

<img width="768" alt="image" src="https://github.com/user-attachments/assets/12e8da78-8d57-462a-acc7-88f2f44159ab">


### Solution
- The  backend servers are contained accross 2 instances for test:

<img width="782" alt="image" src="https://github.com/user-attachments/assets/5651e89c-146a-4304-8c09-9fa9b7815d92">

- Added these instances to my target group. I have named my target group as 
<img width="743" alt="image" src="https://github.com/user-attachments/assets/b07bbccf-a9ec-44ea-811c-5e8b25ef536b">

- My load balancer to point to Target group  is as follows:
<img width="748" alt="image" src="https://github.com/user-attachments/assets/64ae8e34-55e2-4ec0-8623-7fd48fccfc81">

## Task  - Domain Setup with Cloudflare
- Connect your custom domain to the application using Cloudflare.
- Create a CNAME record pointing to the load balancer endpoint.
- Set up an A record with the IP address of the EC2 instance hosting the front end.
### Solution
*I was unable to purchase a cloudflare domain or transfer my GoDaddy domain to cloudflare so I will be setting DNS via GoDaddy Panel.*
1. My GoDaddy domain DNS setting is as follows for adding ELB DNS to my domain records:
2. After successful DNS addition the domain is as follows:
- Create a CNAME record pointing to the load balancer endpoint.
- Set up an A record with the IP address of the EC2 instance hosting the front end.
### Solution
*I was unable to purchase a cloudflare domain or transfer my GoDaddy domain to cloudflare so I will be setting DNS via GoDaddy Panel.*
1. My GoDaddy domain DNS setting is as follows for adding ELB DNS to my domain records:

<img width="959" alt="image" src="https://github.com/user-attachments/assets/a3081529-80e2-4b67-87c7-ddddf5def473">

2. After successful DNS addition the domain is as follows:
<img width="953" alt="image" src="https://github.com/user-attachments/assets/9927d832-37da-4017-ab3b-1effaa43c816">
