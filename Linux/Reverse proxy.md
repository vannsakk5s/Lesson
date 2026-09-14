# Reverse Proxy ដោយមិនប្រើ Domain

### Flow៖
```text
Browser → Ubuntu Server IP:80 → Nginx → Python Server:3000
```

ឧទាហរណ៍ Ubuntu Server IP គឺ `192.168.1.100`៖
- URL: `http://192.168.1.100`

---

### Step 1: ដំឡើង Nginx និង Python
```bash
sudo apt update
sudo apt install nginx python3 -y
```

**បើក Nginx៖**
```bash
sudo systemctl enable --now nginx
```

**ពិនិត្យ Status៖**
```bash
sudo systemctl status nginx
```
> ចុច `q` ដើម្បីចេញពី Status។

---

### Step 2: រក IP របស់ Ubuntu Server
```bash
hostname -I
```

ឧទាហរណ៍លទ្ធផល៖
```text
192.168.1.100
```
> រក្សាទុក IP នេះ ដើម្បីចូល Website ពី Browser។

---

### Step 3: បង្កើត Website សម្រាប់សាកល្បង

**បង្កើត Folder៖**
```bash
sudo mkdir -p /var/www/myapp
```

**ចូលទៅក្នុង Folder៖**
```bash
cd /var/www/myapp
```

**បង្កើត File៖**
```bash
sudo nano index.html
```

**ដាក់ Code ខាងក្រោម៖**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reverse Proxy Test</title>
</head>
<body>
    <h1>Reverse Proxy is working!</h1>
    <p>Nginx connected to Python Server successfully.</p>
</body>
</html>
```

**Save និងចេញ៖**
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

---

### Step 4: ដំណើរការ Python Server លើ Port 3000

នៅក្នុង Folder `/var/www/myapp` ដំណើរការ៖
```bash
python3 -m http.server 3000
```

វានឹងបង្ហាញប្រហែល៖
```text
Serving HTTP on 0.0.0.0 port 3000
```
> **ចំណាំ៖** កុំបិទ Terminal នេះ។ បើក Terminal ថ្មីសម្រាប់បន្ត Step បន្ទាប់។

**សាកល្បង Backend ក្នុង Ubuntu៖**
```bash
curl http://127.0.0.1:3000
```
បើឃើញ HTML មានន័យថា Python Server ដំណើរការ។

---

### Step 5: បង្កើត Nginx Reverse Proxy Configuration
```bash
sudo nano /etc/nginx/sites-available/myapp.conf
```

**ដាក់ Configuration៖**
```nginx
server {
    listen 80;
    listen [::]:80;

    server_name _;

    location / {
        proxy_pass http://127.0.0.1:3000;

        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_cache_bypass $http_upgrade;
    }
}
```

**Save និងចេញ៖**
- `Ctrl + O`
- `Enter`
- `Ctrl + X`

> **ពន្យល់៖** `server_name _;` មានន័យថា Nginx ទទួល Request តាម Server IP ដោយមិនត្រូវការ Domain។

---

### Step 6: Disable Default Configuration
```bash
sudo unlink /etc/nginx/sites-enabled/default
```
> បើវាបង្ហាញថា File មិនមាន អាចរំលងបាន។

---

### Step 7: Enable myapp.conf
```bash
sudo ln -s /etc/nginx/sites-available/myapp.conf /etc/nginx/sites-enabled/
```

**ពិនិត្យថា Link ត្រូវបានបង្កើត៖**
```bash
ls -l /etc/nginx/sites-enabled/
```

គួរឃើញ៖
```text
myapp.conf -> /etc/nginx/sites-available/myapp.conf
```
> បើវាបង្ហាញ File exists មានន័យថា Site ត្រូវបាន Enable រួចហើយ។

---

### Step 8: Test Nginx Configuration
```bash
sudo nginx -t
```

បើត្រឹមត្រូវ វានឹងបង្ហាញ៖
```text
syntax is ok
test is successful
```
> បើមាន Error កុំទាន់ Reload Nginx។

---

### Step 9: Reload Nginx
```bash
sudo systemctl reload nginx
```

**ពិនិត្យ Status៖**
```bash
sudo systemctl status nginx
```

---

### Step 10: អនុញ្ញាត Port 80 ក្នុង Firewall

**ពិនិត្យ Firewall៖**
```bash
sudo ufw status
```

បើ Firewall Active អនុញ្ញាត Nginx៖
```bash
sudo ufw allow 'Nginx Full'
```
ឬអនុញ្ញាតតែ HTTP Port 80៖
```bash
sudo ufw allow 80/tcp
```

---

### Step 11: ចូល Website តាម IP

ពី Computer ដែលភ្ជាប់ Network ដូចគ្នា បើក Browser ហើយវាយ៖
```text
http://192.168.1.100
```
> ប្តូរ `192.168.1.100` ទៅ IP ដែលអ្នកទទួលបានពី `hostname -I`។  
> កុំវាយ `:3000` ព្រោះ User ចូលតាម Nginx Port 80។

---

### Step 12: Test ក្នុង Ubuntu Server

**សាកល្បង Nginx៖**
```bash
curl http://localhost
```
ឬ៖
```bash
curl http://192.168.1.100
```

បើឃើញ៖
```text
Reverse Proxy is working!
```
មានន័យថា Reverse Proxy ដំណើរការជោគជ័យ។

---

## Command សរុប
```bash
sudo apt update
sudo apt install nginx python3 -y
sudo systemctl enable --now nginx

hostname -I

sudo mkdir -p /var/www/myapp
cd /var/www/myapp
sudo nano index.html

python3 -m http.server 3000
```

បន្ទាប់មកបើក Terminal ថ្មី៖
```bash
sudo nano /etc/nginx/sites-available/myapp.conf
sudo unlink /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/myapp.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
sudo ufw allow 80/tcp
```

ចូលតាម Browser៖
```text
http://YOUR_SERVER_IP
```
