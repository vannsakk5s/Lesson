# Route 53 + Nginx Reverse Proxy (Frontend, API, Admin) on EC2

Route 53 បម្លែង domain ទៅ IP របស់ EC2។ បន្ទាប់មក browser ភ្ជាប់ទៅ Nginx ហើយ Nginx ជ្រើស service តាម URL path។ Route 53 មិនបែងចែក `/api` និង `/admin` ទេ។ [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring.html?re=true&utm_source=chatgpt.com)

| URL | Service នៅក្នុង EC2 |
| :--- | :--- |
| `http://site.com/` | Frontend → `127.0.0.1:6000` |
| `http://site.com/api/` | API → `127.0.0.1:6001` |
| `http://site.com/admin/` | Admin → `127.0.0.1:6002` |

> [!NOTE]
> នៅក្នុង commands ខាងក្រោម៖
> - ប្តូរ `site.com` ទៅ domain ដែលអ្នកកាន់កាប់។
> - ប្តូរ `YOUR_ELASTIC_IP` ទៅ IP របស់ EC2។
> - ជំហានបង្កើត services គឺជា លំហាត់ demo។ បើអ្នកមាន applications ដំណើរការលើ ports ទាំងនេះរួចហើយ សូមរំលងជំហានទី 6–7។

---

## 1. រៀបចំ EC2
- ប្រើ EC2 Ubuntu ដែលមាន Nginx រួចហើយ និងអាច SSH ចូលបាន។
- EC2 ត្រូវមាន Public IP និងស្ថិតក្នុង public subnet ដែលមាន route ទៅ Internet Gateway។
- ចូល AWS → EC2 → Instances → ជ្រើស instance របស់អ្នក។

---

## 2. ភ្ជាប់ Elastic IP
Elastic IP រក្សា IP ឱ្យនៅដដែលពេល stop/start EC2។ AWS ណែនាំឱ្យប្រើវាសម្រាប់ DNS ដែលចង្អុលទៅ instance។ [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html?utm_source=chatgpt.com)

1. ចូល **EC2** → **Elastic IP addresses**។
2. ចុច **Allocate Elastic IP address**។
3. ចុច **Allocate**។
4. ជ្រើស IP ដែលបានបង្កើត។
5. ចុច **Actions** → **Associate Elastic IP address**។
6. ជ្រើស instance របស់អ្នក។
7. ចុច **Associate**។
8. កត់ត្រា IP នោះសម្រាប់ជំហាន DNS និង SSH។

---

## 3. បើក Security Group
ចូល **EC2** → **Instances** → instance របស់អ្នក → **Security** → **Security group** → **Edit inbound rules**។

កំណត់៖

| Type | Port | Source |
| :--- | :--- | :--- |
| SSH | 22 | My IP |
| HTTP | 80 | `0.0.0.0/0` |
| HTTPS | 443 | `0.0.0.0/0` (ប្រសិនបើប្រើ HTTPS) |

នេះជាការកំណត់សម្រាប់ web server និង SSH តាមឯកសារ AWS។ [Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules-reference.html?utm_source=chatgpt.com)

> [!IMPORTANT]
> Ports 6000, 6001, និង 6002 មិនចាំបាច់បើកក្នុង Security Group ទេ ព្រោះ Nginx ភ្ជាប់ទៅ services ទាំងនេះតាម `127.0.0.1`។

---

## 4. កំណត់ Route 53
ចូល [AWS Route 53](https://console.aws.amazon.com/route53/)។

បើមិនទាន់មាន Hosted Zone៖
1. ចុច **Hosted zones** → **Create hosted zone**។
2. Domain name: domain របស់អ្នក ឧទាហរណ៍ `site.com`។
3. Type: **Public hosted zone**។
4. ចុច **Create hosted zone**។

បន្ទាប់មក បើក Hosted Zone → **Create record** ហើយបង្កើត៖

| Setting | Domain ដើម | www |
| :--- | :--- | :--- |
| **Record name** | ទុកទទេ | `www` |
| **Record type** | A | A |
| **Value** | Elastic IP របស់ EC2 | Elastic IP ដូចគ្នា |
| **Alias** | Off | Off |
| **TTL** | 300 | 300 |
| **Routing policy** | Simple | Simple |

A record ចង្អុល domain ទៅ IPv4 របស់ EC2។ [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html?utm_source=chatgpt.com)

### បើ domain ទិញពី provider ផ្សេង៖
1. ក្នុង Hosted Zone រក record ប្រភេទ **NS**។
2. Copy nameservers ទាំង 4 ដែល Route 53 ផ្តល់ឱ្យ។
3. ចូល dashboard របស់អ្នកលក់ domain។
4. បើក **Nameservers** → **Custom nameservers**។
5. បញ្ចូល nameservers ទាំង 4 ហើយ Save។

នេះជាជំហានឱ្យ domain ប្រើ Route 53 ជា DNS provider។ បើ domain មាន email ឬ records ផ្សេងរួចហើយ សូមចម្លង records ទាំងនោះទៅ Hosted Zone ថ្មីមុនប្តូរ nameservers។ [Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring-new-domain.html?utm_source=chatgpt.com)

> [!WARNING]
> មិនត្រូវបង្កើត DNS records សម្រាប់ `site.com/api` ឬ `site.com/admin` ទេ។ ទាំងនេះជា URL paths ដែលយើងនឹងកំណត់ក្នុង Nginx។

### សាកល្បងនៅ Windows PowerShell៖
```powershell
nslookup site.com
nslookup www.site.com
```
លទ្ធផលត្រូវបង្ហាញ Elastic IP របស់អ្នក។ ការប្តូរ nameservers និង DNS cache អាចត្រូវការពេលមុននឹងឃើញលទ្ធផលថ្មី។

---

## 5. SSH ចូល EC2 ហើយពិនិត្យ Nginx
នៅ PowerShell ក្នុង folder ដែលមាន SSH key៖
```powershell
ssh -i .\your-key.pem ubuntu@YOUR_ELASTIC_IP
```

Commands បន្ទាប់ពីនេះរត់ នៅក្នុង EC2៖
```bash
sudo systemctl status nginx
# ចុច q ដើម្បីចេញ

sudo nginx -t
```

បើ Nginx មិនទាន់មាន៖
```bash
sudo apt update
sudo apt install -y nginx
```

បើក Nginx៖
```bash
sudo systemctl enable --now nginx
```

ប្រសិនបើ UFW កំពុង active សូមអនុញ្ញាត HTTP៖
```bash
sudo ufw status
sudo ufw allow 'Nginx HTTP'
```

---

## 6. បង្កើត demo pages ចំនួន 3
យើងប្រើ Python HTTP servers ដើម្បីឃើញថា Nginx បែងចែក paths បានត្រឹមត្រូវ។

ដំឡើង Python៖
```bash
sudo apt update
sudo apt install -y python3
```

បង្កើត folders៖
```bash
sudo mkdir -p /var/www/proxy-lab/6000
sudo mkdir -p /var/www/proxy-lab/6001
sudo mkdir -p /var/www/proxy-lab/6002
```

បង្កើត Frontend page：
```bash
sudo tee /var/www/proxy-lab/6000/index.html > /dev/null <<'EOF'
<!DOCTYPE html>
<html>
<head><title>Frontend</title></head>
<body>
  <h1>Frontend Website</h1>
  <p>Service port: 6000</p>
  <a href="/api/">Open API</a>
  <a href="/admin/">Open Admin</a>
</body>
</html>
EOF
```

បង្កើត API demo page៖
```bash
sudo tee /var/www/proxy-lab/6001/index.html > /dev/null <<'EOF'
<!DOCTYPE html>
<html>
<head><title>API Service</title></head>
<body>
  <h1>API Service</h1>
  <p>Service port: 6001</p>
</body>
</html>
EOF
```

បន្ថែម JSON សម្រាប់សាកល្បង៖
```bash
sudo tee /var/www/proxy-lab/6001/users.json > /dev/null <<'EOF'
{
  "service": "API",
  "port": 6001,
  "users": [
    {
      "id": 1,
      "name": "Sak"
    }
  ]
}
EOF
```

បង្កើត Admin page៖
```bash
sudo tee /var/www/proxy-lab/6002/index.html > /dev/null <<'EOF'
<!DOCTYPE html>
<html>
<head><title>Admin Dashboard</title></head>
<body>
  <h1>Admin Dashboard</h1>
  <p>Service port: 6002</p>
</body>
</html>
EOF
```

---

## 7. ដំណើរការ demo services
ប្រើ systemd ដើម្បីឱ្យ services បន្តដំណើរការពេលបិទ SSH និងចាប់ផ្តើមវិញក្រោយ reboot។

បង្កើត service template៖
```bash
sudo nano /etc/systemd/system/proxy-lab@.service
```

ដាក់៖
```ini
[Unit]
Description=Reverse proxy demo on port %i
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/proxy-lab/%i
ExecStart=/usr/bin/python3 -m http.server %i --bind 127.0.0.1
Restart=on-failure
RestartSec=3

[Install]
WantedBy=multi-user.target
```
> Save ដោយចុច៖ `Ctrl+O` → `Enter` → `Ctrl+X`។

ចាប់ផ្តើម services៖
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now proxy-lab@6000 proxy-lab@6001 proxy-lab@6002
```

ពិនិត្យ៖
```bash
sudo ss -lntp | grep -E ':6000|:6001|:6002'
```
ត្រូវឃើញ ports ទាំង 3 listen នៅ `127.0.0.1`។

សាកល្បងដោយផ្ទាល់៖
```bash
curl http://127.0.0.1:6000/
curl http://127.0.0.1:6001/
curl http://127.0.0.1:6001/users.json
curl http://127.0.0.1:6002/
```

---

## 8. បង្កើត Nginx configuration
កែឈ្មោះ folder ក្នុងការណែនាំរបស់អ្នក៖

| ការប្រើប្រាស់ | Path ត្រឹមត្រូវលើ Ubuntu |
| :--- | :--- |
| រក្សាទុក config | `/etc/nginx/sites-available/web.conf` |
| Enable config | `/etc/nginx/sites-enabled/web.conf` |

បង្កើត file៖
```bash
sudo nano /etc/nginx/sites-available/web.conf
```

ដាក់ config ខាងក្រោម ហើយប្តូរ domain៖
```nginx
server {
    listen 80;
    server_name site.com www.site.com;

    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    # Frontend
    location / {
        proxy_pass http://127.0.0.1:6000;
    }

    # Add trailing slash to API URL
    location = /api {
        return 301 /api/;
    }

    # API service
    location /api/ {
        proxy_pass http://127.0.0.1:6001/;
    }

    # Add trailing slash to Admin URL
    location = /admin {
        return 301 /admin/;
    }

    # Admin dashboard
    location /admin/ {
        proxy_pass http://127.0.0.1:6002/;
    }
}
```
> Save៖ `Ctrl+O` → `Enter` → `Ctrl+X`។

---

## 9. Enable config ហើយ Reload
បង្កើត symbolic link៖
```bash
sudo ln -s /etc/nginx/sites-available/web.conf /etc/nginx/sites-enabled/web.conf
```

បើឃើញ `File exists` ពិនិត្យ link ដែលមានស្រាប់៖
```bash
ls -l /etc/nginx/sites-enabled/web.conf
```

បន្ទាប់មកពិនិត្យ syntax៖
```bash
sudo nginx -t
```
លទ្ធផលត្រូវមាន៖
```text
syntax is ok
test is successful
```

បើ test ជោគជ័យ ទើប reload៖
```bash
sudo systemctl reload nginx
```
> [!TIP]
> មិនចាំបាច់លុប default config សម្រាប់ domain នេះទេ។ Nginx ប្រើ `server_name` ដើម្បីជ្រើស server block។

---

## 10. សាកល្បង Nginx មុនពឹងផ្អែកលើ DNS
នៅក្នុង EC2៖
```bash
curl -H "Host: site.com" http://127.0.0.1/
curl -H "Host: site.com" http://127.0.0.1/api/
curl -H "Host: site.com" http://127.0.0.1/api/users.json
curl -H "Host: site.com" http://127.0.0.1/admin/
```
*(ប្តូរ `site.com` ក្នុង commands ទាំងនេះទៅ domain របស់អ្នកដែរ)*

| Request | លទ្ធផលរំពឹងទុក |
| :--- | :--- |
| `/` | Frontend Website |
| `/api/` | API Service |
| `/api/users.json` | JSON users |
| `/admin/` | Admin Dashboard |

បន្ទាប់មកបើក browser៖
- `http://site.com/`
- `http://site.com/api/`
- `http://site.com/api/users.json`
- `http://site.com/admin/`

> [!NOTE]
> ប្រើ `http://` សម្រាប់ដំណាក់កាលនេះ ព្រោះ config ខាងលើមិនទាន់មាន SSL certificate។

---

## 11. យល់ពី `/` ចុងក្រោយក្នុង `proxy_pass`
ក្នុង config ខាងលើ៖
```nginx
location /api/ {
    proxy_pass http://127.0.0.1:6001/;
}
```

Nginx ដក prefix `/api/` ចេញមុនបញ្ជូនទៅ service៖

| Browser request | Request ទៅ backend |
| :--- | :--- |
| `/api/` | `http://127.0.0.1:6001/` |
| `/api/users.json` | `http://127.0.0.1:6001/users.json` |

បើ backend ពិតប្រាកដរបស់អ្នកមាន route `/api/users` ហើយចង់រក្សា `/api` ត្រូវប្រើ `proxy_pass` គ្មាន `/` ចុងក្រោយ៖
```nginx
location /api/ {
    proxy_pass http://127.0.0.1:6001;
}
```
ពេលនោះ `/api/users` ត្រូវបញ្ជូនទៅ backend ជា `/api/users` ដដែល។ នេះជាឥរិយាបថដែល Nginx កំណត់សម្រាប់ proxy_pass មាន និងគ្មាន URI។ [nginx.org](https://nginx.org/en/docs/http/ngx_http_proxy_module.html?utm_source=chatgpt.com)

---

## 12. ពេលប្តូរពី demo ទៅ project ពិត
ដំណើរការ applications របស់អ្នកតាម mapping នេះ៖

| Application | Address |
| :--- | :--- |
| Frontend | `127.0.0.1:6000` |
| Backend API | `127.0.0.1:6001` |
| Admin | `127.0.0.1:6002` |

បញ្ឈប់ demo services មុន ដើម្បីឱ្យ ports ទំនេរ៖
```bash
sudo systemctl disable --now proxy-lab@6000 proxy-lab@6001 proxy-lab@6002
```

> [!IMPORTANT]
> - សម្រាប់ Admin ដែលដាក់ក្រោម `/admin/` ត្រូវកំណត់ application ឱ្យប្រើ base path `/admin/` ផងដែរ។ ឧទាហរណ៍ assets គួរត្រូវបានស្នើតាម `/admin/assets/...`។ បើ application ស្នើ `/assets/...` វានឹងទៅ Frontend service។
> - Frontend អាចហៅ API ដោយ relative URL៖
>   ```javascript
>   fetch('/api/users')
>   ```

---

## 13. Commands សម្រាប់រកបញ្ហា

| បញ្ហា | អ្វីត្រូវពិនិត្យ |
| :--- | :--- |
| **Domain មិនចេញ IP** | Nameservers និង A record |
| **Browser timeout** | Security Group port 80, EC2 networking និង UFW |
| **502 Bad Gateway** | Service មិនដំណើរការ ឬ port ខុស |
| **API 404** | Backend route និង `/` ចុងក្រោយក្នុង `proxy_pass` |
| **Admin page ចេញ តែ CSS/JS បាត់** | Admin base path និង asset URLs |
| **ឃើញ Nginx default page** | Domain ក្នុង `server_name`, symlink និង reload |

មើល Nginx errors៖
```bash
sudo tail -n 50 /var/log/nginx/error.log
```

មើល API demo logs៖
```bash
sudo journalctl -u proxy-lab@6001 -n 50 --no-pager
```

ពិនិត្យ config ដែល Nginx បាន load៖
```bash
sudo nginx -T
```

ពេលកែ config ម្តងៗ រត់៖
```bash
sudo nginx -t
sudo systemctl reload nginx
```
