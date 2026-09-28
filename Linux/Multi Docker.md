# Multi Docker + Nginx Reverse Proxy (AWS EC2)

### Flow សង្ខេប៖
```text
Internet / Browser
       ↓
      DNS (site1.com, site2.com, site3.com)
       ↓
 AWS EC2 Public IP:80 / 443
       ↓
 Nginx Reverse Proxy (Host)
       ├── site1.com → 127.0.0.1:7001 → Docker Container (site1)
       ├── site2.com → 127.0.0.1:7002 → Docker Container (site2)
       └── site3.com → 127.0.0.1:7003 → Docker Container (site3)
```

---

### Step 1 — SSH ចូល EC2
នៅលើ **Windows PowerShell**:
```powershell
ssh -i mykey.pem ubuntu@YOUR_EC2_IP
```

**ឧទាហរណ៍៖**
```powershell
ssh -i mykey.pem ubuntu@54.179.20.10
```

---

### Step 2 — Update Ubuntu
នៅក្នុង EC2:
```bash
sudo apt update
sudo apt upgrade -y
```

---

### Step 3 — Install Docker
```bash
sudo apt install docker.io -y
```

**Enable Docker:**
```bash
sudo systemctl enable --now docker
```

**Start Docker:**
```bash
sudo systemctl start docker
```

**Check Docker:**
```bash
sudo systemctl status docker
```
> ចុច `q` ដើម្បីចេញពី Status។

**Check version:**
```bash
docker --version
```

---

### Step 4 — Check Current User & Add to Docker Group
```bash
whoami
```
គួរឃើញ៖
```text
ubuntu
```

**Add ubuntu ទៅ Docker group:**
```bash
sudo usermod -aG docker $USER
```
ឬ៖
```bash
sudo usermod -aG docker ubuntu
```

**ចេញពី SSH:**
```bash
exit
```

**Connect SSH ម្តងទៀត:**
```powershell
ssh -i mykey.pem ubuntu@YOUR_EC2_IP
```

**Test:**
```bash
docker ps
```

---

### Step 5 — Pull Nginx Docker Image
```bash
docker pull nginx:latest
```

**Check:**
```bash
docker images
```

---

### Step 6 — Create Docker Network
```bash
docker network create web-network
```

**Check:**
```bash
docker network ls
```

---

### Step 7 — Run Container 1
```bash
docker run -d \
--name site1 \
--network web-network \
-p 127.0.0.1:7001:80 \
nginx:latest
```

---

### Step 8 — Run Container 2
```bash
docker run -d \
--name site2 \
--network web-network \
-p 127.0.0.1:7002:80 \
nginx:latest
```

---

### Step 9 — Run Container 3
```bash
docker run -d \
--name site3 \
--network web-network \
-p 127.0.0.1:7003:80 \
nginx:latest
```

---

### Step 10 — Check Containers
```bash
docker ps
```

គួរឃើញ៖
```text
site1   127.0.0.1:7001->80/tcp
site2   127.0.0.1:7002->80/tcp
site3   127.0.0.1:7003->80/tcp
```

**Check all containers:**
```bash
docker ps -a
```

---

### Step 11 — Test Default Nginx
```bash
curl http://127.0.0.1:7001
curl http://127.0.0.1:7002
curl http://127.0.0.1:7003
```
គួរឃើញ៖
```html
Welcome to nginx!
```

---

### Step 12 — Create Website Folders
```bash
mkdir -p site1 site2 site3
```

**Site 1:**
```bash
nano site1/index.html
```

Paste:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Site 1</title>
</head>
<body>
    <h1>Website 1</h1>
    <h2>Docker Container 1</h2>
</body>
</html>
```

Save:
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

**Site 2:**
```bash
nano site2/index.html
```

Paste:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Site 2</title>
</head>
<body>
    <h1>Website 2</h1>
    <h2>Docker Container 2</h2>
</body>
</html>
```

Save:
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

**Site 3:**
```bash
nano site3/index.html
```

Paste:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Site 3</title>
</head>
<body>
    <h1>Website 3</h1>
    <h2>Docker Container 3</h2>
</body>
</html>
```

Save:
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

---

### Step 13 — Upload Websites from Windows → EC2 (ប្រើ FileZilla)

**របៀបភ្ជាប់ FileZilla ទៅកាន់ EC2 តាម SFTP៖**
1. បើកកម្មវិធី **FileZilla**
2. ចូលទៅ **File** → **Site Manager** (ឬចុច `Ctrl + S`) រួចចុច **New Site** (ដាក់ឈ្មោះឧទាហរណ៍៖ `EC2-Server`)
3. កំណត់ Settings ដូចខាងក្រោម៖
   - **Protocol**: `SFTP - SSH File Transfer Protocol`
   - **Host**: ដាក់ Public IP របស់ EC2 (ឧទាហរណ៍៖ `54.179.20.10`)
   - **Port**: `22`
   - **Logon Type**: `Key file`
   - **User**: `ubuntu`
   - **Key file**: ចុច **Browse...** រួចជ្រើសរើសយក file `mykey.pem`
4. ចុច **Connect** (បើមានផ្ទាំង pop-up "Unknown host key" សូមចុច **OK**)

**Upload Folders ទៅកាន់ EC2៖**
- នៅផ្ទាំងខាងឆ្វេង (**Local site** / លើ Windows): រកមើល Folder `site1`, `site2`, `site3`
- នៅផ្ទាំងខាងស្ដាំ (**Remote site** / លើ EC2): ចូលទៅកាន់ `/home/ubuntu`
- **Drag & Drop** (អូសទម្លាក់) ឬ Right-click លើ Folders ទាំង ៣ រួចយក **Upload** ចូលទៅក្នុង `/home/ubuntu/`

> **(ជម្រើសបន្ថែម) ប្រសិនបើចង់ប្រើ SCP តាម Windows PowerShell វិញ៖**
> ```powershell
> scp -i mykey.pem -r site1 site2 site3 ubuntu@YOUR_EC2_IP:/home/ubuntu/
> ```

---

### Step 14 — SSH ចូល EC2 វិញ
```powershell
ssh -i mykey.pem ubuntu@YOUR_EC2_IP
```

**Check files:**
```bash
ls
```
គួរឃើញ៖
```text
site1
site2
site3
```

**Check ក្នុង Folder នីមួយៗ:**
```bash
ls site1
ls site2
ls site3
```

---

### Step 15 — Copy Site 1 ទៅ Container 1
```bash
docker cp /home/ubuntu/site1/. site1:/usr/share/nginx/html/
```

---

### Step 16 — Copy Site 2 ទៅ Container 2
```bash
docker cp /home/ubuntu/site2/. site2:/usr/share/nginx/html/
```

---

### Step 17 — Copy Site 3 ទៅ Container 3
```bash
docker cp /home/ubuntu/site3/. site3:/usr/share/nginx/html/
```

---

### Step 18 — Test Websites
```bash
curl http://127.0.0.1:7001
curl http://127.0.0.1:7002
curl http://127.0.0.1:7003
```
ឥឡូវគួរឃើញ HTML របស់អ្នក។

---

### Step 19 — Enter Docker Container
**Site 1:**
```bash
docker exec -it site1 /bin/bash
```

**Check HTML:**
```bash
ls /usr/share/nginx/html
cat /usr/share/nginx/html/index.html
```

**Exit:**
```bash
exit
```

**Site 2:**
```bash
docker exec -it site2 /bin/bash
exit
```

**Site 3:**
```bash
docker exec -it site3 /bin/bash
exit
```

---

### Step 20 — Install Nginx Reverse Proxy on EC2
```bash
sudo apt install nginx -y
```

**Enable:**
```bash
sudo systemctl enable --now nginx
```

**Start:**
```bash
sudo systemctl start nginx
```

**Check:**
```bash
sudo systemctl status nginx
```
> ចុច `q` ដើម្បីចេញ។

---

### Step 21 — Create Nginx Config for Site 1
```bash
sudo nano /etc/nginx/sites-available/site1.com
```

**Paste:**
```nginx
server {
    listen 80;
    server_name site1.com www.site1.com;

    location / {
        proxy_pass http://127.0.0.1:7001;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Save:**
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

**Enable Site 1:**
```bash
sudo ln -s /etc/nginx/sites-available/site1.com /etc/nginx/sites-enabled/
```

---

### Step 22 — Create Nginx Config for Site 2
```bash
sudo nano /etc/nginx/sites-available/site2.com
```

**Paste:**
```nginx
server {
    listen 80;
    server_name site2.com www.site2.com;

    location / {
        proxy_pass http://127.0.0.1:7002;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Enable Site 2:**
```bash
sudo ln -s /etc/nginx/sites-available/site2.com /etc/nginx/sites-enabled/
```

---

### Step 23 — Create Nginx Config for Site 3
```bash
sudo nano /etc/nginx/sites-available/site3.com
```

**Paste:**
```nginx
server {
    listen 80;
    server_name site3.com www.site3.com;

    location / {
        proxy_pass http://127.0.0.1:7003;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**Enable Site 3:**
```bash
sudo ln -s /etc/nginx/sites-available/site3.com /etc/nginx/sites-enabled/
```

---

### Step 24 — Remove Default Nginx Website
```bash
sudo rm -f /etc/nginx/sites-enabled/default
```

---

### Step 25 — Check Nginx Config
```bash
sudo nginx -t
```
ត្រូវឃើញ៖
```text
syntax is ok
test is successful
```

**Reload Nginx:**
```bash
sudo systemctl reload nginx
```

---

### Step 26 — Test Reverse Proxy Before DNS
យើងអាច test នៅក្នុង EC2 ដោយប្រើ Host header៖

**Site 1:**
```bash
curl -H "Host: site1.com" http://127.0.0.1
```

**Site 2:**
```bash
curl -H "Host: site2.com" http://127.0.0.1
```

**Site 3:**
```bash
curl -H "Host: site3.com" http://127.0.0.1
```
> នេះជាវិធីល្អណាស់សម្រាប់ check ថា Nginx configuration ត្រឹមត្រូវ មុន DNS ready។

---

### Step 27 — AWS Security Group
នៅ **AWS EC2 → Security Group → Inbound Rules** ត្រូវមាន៖

| Protocol | Type | Port | Source |
| :--- | :--- | :--- | :--- |
| TCP | SSH | 22 | Your IP |
| TCP | HTTP | 80 | 0.0.0.0/0 |
| TCP | HTTPS | 443 | 0.0.0.0/0 |

> **ចំណាំ៖** មិនចាំបាច់បើក Ports `7001`, `7002`, `7003` នោះទេ ព្រោះ ports ទាំងនេះ bind ទៅ `127.0.0.1` មានតែ server ខ្លួនឯងប៉ុណ្ណោះដែលអាច access។

---

### Step 28 — Configure DNS
**ទាញ EC2 Public IP:**
```bash
curl ifconfig.me
```
*ឧទាហរណ៍:* `54.179.20.10`

**កំណត់ Record សម្រាប់ site1.com:**
- Type: `A` | Name: `@` | Value: `54.179.20.10`
- Type: `A` | Name: `www` | Value: `54.179.20.10`

**កំណត់ Record សម្រាប់ site2.com:**
- Type: `A` | Name: `@` | Value: `54.179.20.10`
- Type: `A` | Name: `www` | Value: `54.179.20.10`

**កំណត់ Record សម្រាប់ site3.com:**
- Type: `A` | Name: `@` | Value: `54.179.20.10`
- Type: `A` | Name: `www` | Value: `54.179.20.10`

> ទាំង 3 domains point ទៅ IP តែមួយដូចគ្នា។

---

### Step 29 — Check DNS from Windows
នៅលើ **Windows PowerShell**:
```powershell
nslookup site1.com
nslookup site2.com
nslookup site3.com
```
ត្រូវឃើញ EC2 IP ដូចគ្នាទាំងអស់។

---

### Step 30 — Test Browser
ចូលទៅកាន់ Browser៖
- `http://site1.com` → Container **site1**
- `http://site2.com` → Container **site2**
- `http://site3.com` → Container **site3**

---

### Step 31 — Install SSL / HTTPS
```bash
sudo apt install certbot python3-certbot-nginx -y
```

**ដំឡើង SSL សម្រាប់ Site 1:**
```bash
sudo certbot --nginx -d site1.com -d www.site1.com
```

**ដំឡើង SSL សម្រាប់ Site 2:**
```bash
sudo certbot --nginx -d site2.com -d www.site2.com
```

**ដំឡើង SSL សម្រាប់ Site 3:**
```bash
sudo certbot --nginx -d site3.com -d www.site3.com
```

**Test SSL renewal:**
```bash
sudo certbot renew --dry-run
```

---

### Step 32 — Final Test
បើក Browser សាកល្បង៖
- `https://site1.com`
- `https://site2.com`
- `https://site3.com`

---

## Architecture Overview
```text
Internet
   ↓
  DNS
   ↓
AWS EC2 Public IP
   ↓
Nginx Reverse Proxy (Host)
   │
   ├── site1.com
   │      ↓
   │   127.0.0.1:7001
   │      ↓
   │   Docker site1
   │
   ├── site2.com
   │      ↓
   │   127.0.0.1:7002
   │      ↓
   │   Docker site2
   │
   └── site3.com
          ↓
       127.0.0.1:7003
          ↓
       Docker site3
```

---

## Useful Docker & Nginx Commands

### Docker Commands:
- **List running containers:**
  ```bash
  docker ps
  ```
- **All containers (ទាំង stop):**
  ```bash
  docker ps -a
  ```
- **Images:**
  ```bash
  docker images
  ```
- **Networks:**
  ```bash
  docker network ls
  ```
- **Inspect network:**
  ```bash
  docker network inspect web-network
  ```
- **Stop container:**
  ```bash
  docker stop site1
  ```
- **Start container:**
  ```bash
  docker start site1
  ```
- **Restart container:**
  ```bash
  docker restart site1
  ```
- **Logs:**
  ```bash
  docker logs site1
  ```
- **Enter container:**
  ```bash
  docker exec -it site1 /bin/bash
  ```
- **Remove container:**
  ```bash
  docker rm -f site1
  ```
- **Remove image:**
  ```bash
  docker rmi nginx:latest
  ```

### Nginx Commands:
- **Check syntax:**
  ```bash
  sudo nginx -t
  ```
- **Reload Nginx:**
  ```bash
  sudo systemctl reload nginx
  ```
- **Restart Nginx:**
  ```bash
  sudo systemctl restart nginx
  ```
- **Check status:**
  ```bash
  sudo systemctl status nginx
  ```

---

## Lab Flow Summary
```text
Install Docker
     ↓
Pull nginx
     ↓
Create 3 Containers (7001 / 7002 / 7003)
     ↓
Upload 3 Websites
     ↓
docker cp ទៅ Containers
     ↓
Install Host Nginx Reverse Proxy
     ↓
Create 3 Server Blocks (site1.com, site2.com, site3.com)
     ↓
Configure DNS
     ↓
SSL (Certbot)
     ↓
3 Domains → 3 Isolated Docker Containers
```
