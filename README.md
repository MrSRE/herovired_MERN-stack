# herovired_MERN-stack

Graded Project on Travel Memory Application Deployment Submission

ArchTecher Diagram

<img width="547" alt="image" src="https://github.com/user-attachments/assets/38aa2772-5fdf-4910-8a20-873c7f0e2d1d">

![image](https://github.com/user-attachments/assets/6603c558-1ae8-4484-ae58-ee4f28b4edf0)

pkgajula1997
14f9m5KwJORsz8p8

apt-get update
apt-get upgrade -y


apt install nodejs -y 
node --version

apt install npm -y
npm --version

git clone https://github.com/UnpredictablePrashant/TravelMemory.git
-------
Backend :

vi .env # add below
# MONGO_URI='mongodb+srv://pkgajula1997:14f9m5KwJORsz8p8@travel1.heava.mongodb.net/travelmemory_pavan'
# PORT=3001

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

