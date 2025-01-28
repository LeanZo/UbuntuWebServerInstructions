# 🌐 Configuring Ubuntu Web Server to Host Nest/Node Apps With PostgreSQL Database

This guide will walk you through setting up an Ubuntu server to host NestJS/Node.js applications With PostgreSQL Database. 🛠️ Let's get started!

---

## 📚 References:
- [Install Nginx on Ubuntu 22.04](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04)
- [Install Node.js on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)
- [Deploy a NestJS App with Nginx](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-nestjs-application-with-nginx-on-ubuntu)

---

## 🚀 Step 1 – Installing Nginx
```bash
sudo apt update
sudo apt install nginx
```

---

## 🔥 Step 2 – Adjusting the Firewall
Check the app list to ensure Nginx options are available:
```bash
sudo ufw app list
```

Enable necessary services:
```bash
ufw enable
ufw allow 22
ufw allow 5432/tcp
ufw allow 'Nginx Full'
```

Verify the status (look for `Nginx Full` being active):
```bash
sudo ufw status
```

---

## 🌐 Step 3 – Checking Your Web Server
Make sure the web server is running:
```bash
systemctl status nginx
```

You should now be able to access your server's IP and see the default Nginx page! 🎉

---

## 🧠 Step 4 – Avoid Bucket Memory Problem
Edit the Nginx configuration:
```bash
sudo nano /etc/nginx/nginx.conf
```

Find the `server_names_hash_bucket_size` directive, remove the `#` to uncomment it, then save and close.

Disable the default symlink otherwise, nginx will redirect all requests to the default site:
```bash
sudo unlink /etc/nginx/sites-enabled/default
```

Test the configuration:
```bash
sudo nginx -t
```

Restart Nginx to apply changes:
```bash
sudo systemctl restart nginx
```

---

## 💻 Step 5 – Preparing the Node and Nest Environment
Install Node.js (replace `22.x` with the required version):
```bash
cd ~
curl -sL https://deb.nodesource.com/setup_22.x -o /tmp/nodesource_setup.sh
sudo bash /tmp/nodesource_setup.sh
sudo apt install nodejs
node -v
```

Install the NestJS CLI globally:
```bash
npm i -g @nestjs/cli
```

---

## ⚙️ Step 6 – Setting Nginx to Serve the Node/NestJS Application
Install the PM2 process manager:
```bash
npm install -g pm2
```

Configure pm2 to start up on server restarts:
```bash
pm2 startup
```

---

## 🔒 Step 7 – Adding SSL Using Let’s Encrypt
Install Certbot for Nginx:
```bash
sudo apt install certbot python3-certbot-nginx
```

---

## 🗄️ Step 8 – Install PostgreSQL
This installs PostgreSQL and additional tools:
```bash
sudo apt install postgresql postgresql-contrib
```

---

## ⚡ Step 9 – Start PostgreSQL Service
Start PostgreSQL and check its status:
```bash
sudo systemctl start postgresql
sudo systemctl status postgresql
```

---

## 🔑 Step 10 – Set Password for the Default PostgreSQL User
Switch to the `postgres` user:
```bash
sudo -i -u postgres
psql
```

Set a new password:
```sql
ALTER USER postgres WITH PASSWORD 'your_password';
```

Exit PostgreSQL:
```bash
exit
```

---

## 🌍 Step 11 – Enable Remote Connections for PostgreSQL
Edit the `postgresql.conf` file:
```bash
sudo nano /etc/postgresql/(VERSION_NUMBER)/main/postgresql.conf
```

Find the line `#listen_addresses = 'localhost'` and change it to:
```
listen_addresses = '*'
```

Edit the `pg_hba.conf` file:
```bash
sudo nano /etc/postgresql/(VERSION_NUMBER)/main/pg_hba.conf
```

Add this line at the end:
```
host    all             all             0.0.0.0/0               md5
```

Restart PostgreSQL to apply changes:
```bash
sudo systemctl restart postgresql
```

---

## 🎛️ Extra – Useful Nginx Commands
- **Stop the web server:**
  ```bash
  sudo systemctl stop nginx
  ```
- **Start the web server:**
  ```bash
  sudo systemctl start nginx
  ```
- **Restart the web server:**
  ```bash
  sudo systemctl restart nginx
  ```
- **Reload configuration without dropping connections:**
  ```bash
  sudo systemctl reload nginx
  ```
- **Disable Nginx from starting at boot:**
  ```bash
  sudo systemctl disable nginx
  ```
- **Enable Nginx to start at boot (default):**
  ```bash
  sudo systemctl enable nginx
  ```

---

🎉 And that's it! Your Ubuntu server is now configured to host NestJS/Node.js applications with Nginx, PostgreSQL, and SSL! 🚀
