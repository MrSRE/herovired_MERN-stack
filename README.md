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


pkgajula1997
14f9m5KwJORsz8p8

apt-get update
apt-get upgrade -y


apt install nodejs -y 
node --version

apt install npm -y
npm --version

----
Backend :

vi .env # add below


npm install 
# node index.js
nohup node index.js > output.log 2>&1 & # backend run
ps aux | grep node # node status check
-------------------

Front end 
cd /src/uri
export const baseUrl = "http://54.203.159.109:3000"

npm install
npm start


----nginx

server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://internal-travel-1-628550494.us-west-2.elb.amazonaws.com:3000;  # Replace with your internal load balancer's DNS name
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

vi /etc/nginx/sites-available/default
# add private ec2 private IP
server {
    listen 80;

    server_name _;

    location / {
        proxy_pass http://10.0.0.188:3001;  # Replace 10.0.0.123 with the private IP of your private EC2 instance
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
sudo vi /etc/nginx/sites-available/default


server {
    listen 80;
    server_name YOUR_PUBLIC_IP;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


sudo nginx -t
sudo systemctl restart nginx

---
npm install
npm start

