# 🚀 Deploying a Node.js App with Nginx on AWS EC2

This project shows how I deployed a Node.js app on an Amazon Linux 2023 EC2 instance, and used Nginx as a reverse proxy so the app is accessible on port 80 (HTTP).

## 🖥️ Steps I Followed

### 1. Launch EC2 & Connect
- Created an EC2 instance (Amazon Linux 2023).  
- Connected via SSH:
```bash
ssh -i "key.pem" ec2-user@<ec2-public-ip>
````

### 2. Update & Install Packages

* Updated system:

```bash
sudo dnf update -y
```

* Installed Node.js, npm, Git, and Nginx:

```bash
sudo dnf install -y nodejs npm git nginx
```

### 3. Create Node.js App

* Made a folder and created a simple web server:

```bash
mkdir myTestApp && cd myTestApp
npm init -y
```

* `server.js`:

```javascript
const http = require('http');
const port = 3000;

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('🚀 Hello from Amazon Linux 2023 + Node.js + Nginx!\n');
});

server.listen(port, () => {
  console.log(`Server running at http://localhost:${port}/`);
});
```

* Tested with:

```bash
node server.js
```

### 4. Configure Nginx Reverse Proxy

* Edited `/etc/nginx/nginx.conf`:

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://127.0.0.1:3000;
    }
}
```

* Tested and restarted Nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

### 5. Run App in Background

* Installed PM2 (process manager):

```bash
sudo npm install -g pm2
pm2 start server.js
pm2 save
```

### 6. Access App in Browser

* Opened Security Group → allowed port 80 (HTTP).
* Visited:

```
http://<ec2-public-ip>
```

* Saw my app response 🎉:

```
🚀 Hello from Amazon Linux 2023 + Node.js + Nginx!
```

## 📌 What I Learned

* Basic Linux navigation & permissions.
* Installing software with `dnf`.
* Running a Node.js app on EC2.
* Using Nginx as a reverse proxy.
* Managing apps with PM2.
* Making my app available publicly over the internet.


