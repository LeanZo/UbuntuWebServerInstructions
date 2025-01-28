# 🌍 Configuring a New Website on Ubuntu Web Server with Nginx

This guide will walk you through setting up a new website on an Ubuntu server using Nginx. 🛠️

---

## 📚 Reference:
- [Deploy a NestJS App with Nginx](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-nestjs-application-with-nginx-on-ubuntu)

---

## ⚙️ Step 1 – Nginx Configuration

Create a configuration block for your website (replace `<your_domain>` with your actual domain):
```bash
sudo nano /etc/nginx/sites-available/<your_domain>
```

### 📝 Example Configuration (Replace `<your_domain>` and ports if needed)
```nginx
server {
  server_name your_domain;
    location / {
      proxy_pass http://localhost:3000;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }
}
```

Create a symbolic link so Nginx recognizes your website:
```bash
sudo ln -s /etc/nginx/sites-available/<your_domain> /etc/nginx/sites-enabled/
```

Restart Nginx to apply the changes:
```bash
sudo systemctl restart nginx
```

---

## 🔒 Step 2 – SSL Configuration

Fetch SSL certificates for your domain using Let's Encrypt (Certbot):
```bash
sudo certbot --nginx -d <your_domain> -d <www.your_domain>
```

---

🎉 That's it! Your website is now live with Nginx and SSL configured! 🚀
