# Nginx Reverse Proxy ជាមួយ Docker Container + FileZilla on EC2

នេះជាជំហានលម្អិតពីដំបូង៖ **បង្កើត Docker container → Configure Nginx → Upload index.html តាម FileZilla → បើក codebyme.cyou**។

យើងប្រើ container ចំនួន 3 តាមលំហាត់៖

| URL | Container | Port |
| :--- | :--- | :--- |
| `codebyme.cyou/` | Frontend | `6000` |
| `codebyme.cyou/api/` | API demo | `6001` |
| `codebyme.cyou/admin/` | Admin demo | `6002` |

> [!NOTE]
> អ្នកនឹង upload `index.html` របស់អ្នកទៅ Frontend។ API និង Admin ប្រើទំព័រសាកល្បងសិន។

---

## 1. ភ្ជាប់ទៅ Ubuntu server
បើក PowerShell នៅ folder ដែលមាន SSH key ហើយរត់៖
```powershell
ssh -i "your-key.pem" ubuntu@YOUR_PUBLIC_IP
```

> [!NOTE]
> ជំនួស `your-key.pem` និង `YOUR_PUBLIC_IP` ជាតម្លៃពិតប្រាកដរបស់អ្នក។  
> Command ក្នុងជំហាន 2–7 ខាងក្រោម ត្រូវរត់នៅក្នុង SSH terminal នេះ។

---

## 2. ដំឡើង Docker និង Nginx
បើបានដំឡើងរួច អាចរំលងការដំឡើង៖
```bash
sudo apt update
sudo apt install -y docker.io nginx curl

sudo systemctl enable --now docker
sudo systemctl enable --now nginx
```

ពិនិត្យ៖
```bash
sudo systemctl is-active docker
sudo systemctl is-active nginx
```
*(ទាំងពីរគួរបង្ហាញ `active`)*

---

## 3. បង្កើត folder សម្រាប់ file website
```bash
mkdir -p /home/ubuntu/nginx-container/frontend
mkdir -p /home/ubuntu/nginx-container/api
mkdir -p /home/ubuntu/nginx-container/admin
```

បង្កើតទំព័រសាកល្បង៖
```bash
echo '<h1>Frontend is working</h1>' > /home/ubuntu/nginx-container/frontend/index.html
echo '<h1>API container is working</h1>' > /home/ubuntu/nginx-container/api/index.html
echo '<h1>Admin container is working</h1>' > /home/ubuntu/nginx-container/admin/index.html
```

កំណត់សិទ្ធិដើម្បីអាច upload តាម FileZilla៖
```bash
sudo chown -R ubuntu:ubuntu /home/ubuntu/nginx-container
sudo chmod -R u+rwX,go+rX /home/ubuntu/nginx-container
```

---

## 4. បង្កើត Docker containers
ពិនិត្យមុន៖
```bash
sudo docker ps -a
```

> [!TIP]
> បើមាន `lab-frontend`, `lab-api`, `lab-admin` ដែលបានបង្កើតតាមជំហានមុនរួចហើយ សូមរំលង command បង្កើត container ទាំងនោះ។

បើមិនទាន់មាន រត់៖
```bash
sudo docker pull nginx:alpine
```

**Frontend៖**
```bash
sudo docker run -d \
  --name lab-frontend \
  --restart unless-stopped \
  -p 127.0.0.1:6000:80 \
  -v /home/ubuntu/nginx-container/frontend:/usr/share/nginx/html:ro \
  nginx:alpine
```

**API demo៖**
```bash
sudo docker run -d \
  --name lab-api \
  --restart unless-stopped \
  -p 127.0.0.1:6001:80 \
  -v /home/ubuntu/nginx-container/api:/usr/share/nginx/html:ro \
  nginx:alpine
```

**Admin demo៖**
```bash
sudo docker run -d \
  --name lab-admin \
  --restart unless-stopped \
  -p 127.0.0.1:6002:80 \
  -v /home/ubuntu/nginx-container/admin:/usr/share/nginx/html:ro \
  nginx:alpine
```

---

## 5. សាកល្បង containers
```bash
sudo docker ps

curl http://127.0.0.1:6000/
curl http://127.0.0.1:6001/
curl http://127.0.0.1:6002/
```
*(អ្នកគួរឃើញសាររបស់ Frontend, API និង Admin)*

---

## 6. Configure Nginx Reverse Proxy
បើក configuration៖
```bash
sudo nano /etc/nginx/sites-available/web.conf
```

ដាក់ code ទាំងនេះ៖
```nginx
server {
    listen 80;
    server_name codebyme.cyou www.codebyme.cyou;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    location / {
        proxy_pass http://127.0.0.1:6000;
    }

    location = /api {
        return 301 /api/;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:6001/;
    }

    location = /admin {
        return 301 /admin/;
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:6002/;
    }
}
```

> [!NOTE]
> រក្សាទុកដោយចុច៖ `Ctrl + O` → `Enter` → `Ctrl + X`។  
> ប្រើ configuration តែមួយសម្រាប់ `codebyme.cyou` ដើម្បីជៀសវាង domain ស្ទួនក្នុង Nginx។

---

## 7. Enable configuration
បង្កើត link បើមិនទាន់មាន៖
```bash
if [ ! -e /etc/nginx/sites-enabled/web.conf ] && [ ! -L /etc/nginx/sites-enabled/web.conf ]; then
    sudo ln -s /etc/nginx/sites-available/web.conf /etc/nginx/sites-enabled/web.conf
fi
```

ពិនិត្យ៖
```bash
sudo nginx -t
```

បើបង្ហាញ `syntax is ok` និង `test is successful` រត់៖
```bash
sudo systemctl reload nginx
```

សាកល្បង៖
```bash
curl -H "Host: codebyme.cyou" http://127.0.0.1/
```
គួរឃើញ៖
```html
<h1>Frontend is working</h1>
```

---

## 8. កំណត់ DNS និង Security Group

**នៅ DNS provider របស់ `codebyme.cyou` បង្កើត៖**

| Type | Name | Value |
| :--- | :--- | :--- |
| **A** | `@` | Public IP ឬ Elastic IP របស់ server |
| **A** | `www` | IP ដូចគ្នា |

> [!TIP]
> **បើប្រើ Route 53៖** ចូល **Hosted zones** → `codebyme.cyou` → **Create record**។ សម្រាប់ domain ដើម ទុក Record name ទទេ។ Domain ត្រូវប្រើ Nameservers របស់ Hosted Zone នោះផងដែរ។

**ក្នុង EC2 → Security Groups → Inbound rules អនុញ្ញាត៖**

| Type | Port | Source |
| :--- | :--- | :--- |
| **SSH** | 22 | My IP |
| **HTTP** | 80 | `0.0.0.0/0` |

> [!IMPORTANT]
> មិនចាំបាច់បើក port 6000–6002 ទៅ Internet។

បើ Ubuntu ប្រើ UFW សូមពិនិត្យ៖
```bash
sudo ufw status
```
បើ active អនុញ្ញាត HTTP៖
```bash
sudo ufw allow 80/tcp
```

---

## 9. ភ្ជាប់ FileZilla
នៅលើកុំព្យូទ័ររបស់អ្នក បើក **FileZilla** → **File** → **Site Manager** → **New site**។

បំពេញ៖

| Setting | Value |
| :--- | :--- |
| **Protocol** | SFTP – SSH File Transfer Protocol |
| **Host** | Public IP ឬ Elastic IP របស់ server |
| **Port** | `22` |
| **Logon Type** | Key file |
| **User** | `ubuntu` |
| **Key file** | ជ្រើស file `.pem` របស់អ្នក |

ចុច **Connect**។ ពេលភ្ជាប់លើកដំបូង ផ្ទៀងផ្ទាត់ host key ថាជារបស់ server អ្នក មុនទទួលយក។

---

## 10. Upload តែ index.html
1. **នៅផ្នែកខាងស្ដាំ Remote site** បញ្ចូល៖
   ```text
   /home/ubuntu/nginx-container/frontend/
   ```
2. **នៅផ្នែកខាងឆ្វេង៖**
   - រក file `index.html` របស់អ្នក។
   - អូស file នោះទៅ folder ខាងស្ដាំ។
   - ជ្រើស **Overwrite** ដើម្បីជំនួស file សាកល្បង។
3. រង់ចាំឱ្យ upload ចប់ ហើយពិនិត្យ **Failed transfers** ថាគ្មានការបរាជ័យ។

ទីតាំង file ចុងក្រោយត្រូវជា៖
```text
/home/ubuntu/nginx-container/frontend/index.html
```

> [!NOTE]
> បើ upload តែ file មួយ CSS និង JavaScript របស់អ្នកគួរតែមាននៅក្នុង `index.html` ឬប្រើ URL ខាងក្រៅ។ រូបភាពដែលយោងទៅ file ក្នុងកុំព្យូទ័រ នឹងមិនបង្ហាញទេ រហូតដល់បាន upload រូបនោះ។

---

## 11. បើក website
ចូល៖ [http://codebyme.cyou/](http://codebyme.cyou/)  
ចុច `Ctrl + F5` ដើម្បីមើលទំព័រថ្មី។ ដោយប្រើ folder mount ខាងលើ អ្នកអាច upload ជំនួស `index.html` ពេលក្រោយបាន ដោយមិនចាំបាច់ restart container។

បើឃើញ **502 Bad Gateway** ពិនិត្យ៖
```bash
sudo docker ps -a
curl http://127.0.0.1:6000/
sudo tail -n 30 /var/log/nginx/error.log
```
