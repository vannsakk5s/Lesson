# ការដំណើរការ Multi-Container Nginx ជាមួយ Docker លើ AWS EC2

## តារាងរៀបចំ Containers និង Ports

| Website | Container | Port ខាងក្រៅ | URL |
| :--- | :--- | :--- | :--- |
| **Home** | `home` | `7001` | `http://YOUR_ELASTIC_IP:7001` |
| **API** | `api` | `7002` | `http://YOUR_ELASTIC_IP:7002` |
| **Admin** | `admin` | `7003` | `http://YOUR_ELASTIC_IP:7003` |

---

## ១. រៀបចំ EC2

1. បង្កើត **Ubuntu EC2 instance** ហើយទាញយក key `mykey.pem`។
2. នៅផ្នែក **Elastic IPs** ចុច **Allocate Elastic IP address** រួច **Associate** ទៅកាន់ instance នោះ ([Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-eips.html))។
3. នៅ **Security Group → Inbound rules** បន្ថែម៖

| Type | Port | Source |
| :--- | :--- | :--- |
| **SSH** | `22` | `My IP` |
| **Custom TCP** | `7001` | `0.0.0.0/0` |
| **Custom TCP** | `7002` | `0.0.0.0/0` |
| **Custom TCP** | `7003` | `0.0.0.0/0` |

> [!NOTE]
> Port `7001–7003` ត្រូវបើក inbound ទើប browser ខាងក្រៅអាចចូលមើលបាន។ ចំណែកឯ SSH (`22`) គួរកំណត់ត្រឹមតែ IP ផ្ទាល់ខ្លួនរបស់អ្នកដើម្បីសុវត្ថិភាព ([Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/creating-security-group.html))។

---

## ២. SSH និងដំឡើង Docker

បើក **PowerShell** នៅលើកុំព្យូទ័ររបស់អ្នក (នៅ folder ណាដែលមាន `mykey.pem`)៖

```powershell
ssh -i .\mykey.pem ubuntu@YOUR_ELASTIC_IP
```

បន្ទាប់មករត់នៅលើ EC2៖

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo systemctl status docker
```

- ចុច `q` ដើម្បីចេញពីផ្ទាំង status។
- ពិនិត្យ user ហើយបន្ថែម user ដែលកំពុងប្រើ (`ubuntu`) ទៅ Docker group៖

```bash
whoami
sudo usermod -aG docker ubuntu
exit
```

> [!IMPORTANT]
> ប្រសិនបើអ្នក SSH ជា `ubuntu` ត្រូវប្រើ `sudo usermod -aG docker ubuntu` (មិនមែន `... docker root` ទេ)។ ការចូល Docker group ផ្ដល់សិទ្ធិខ្ពស់ស្មើ root ដូច្នេះប្រើតែលើ user ដែលអ្នកទុកចិត្ត ([Docker Docs](https://docs.docker.com/engine/install/linux-postinstall))។

SSH ចូលម្ដងទៀត ហើយពិនិត្យ៖

```powershell
ssh -i .\mykey.pem ubuntu@YOUR_ELASTIC_IP
```

```bash
whoami
id
docker --version
```

*(ក្នុងលទ្ធផល `id` គួរឃើញ group `docker`)*

---

## ៣. ដំណើរការ Nginx ៣ Containers

នៅលើ EC2៖

```bash
docker pull nginx:latest
docker run -d --name home -p 7001:80 --restart unless-stopped nginx:latest
docker run -d --name api -p 7002:80 --restart unless-stopped nginx:latest
docker run -d --name admin -p 7003:80 --restart unless-stopped nginx:latest
docker ps
```

> [!TIP]
> - `7001:80` មានន័យថា port `7001` លើ EC2 ភ្ជាប់ទៅកាន់ port `80` ក្នុង container។
> - Option `--restart unless-stopped` ជួយឱ្យ containers ចាប់ផ្ដើមដំណើរការឡើងវិញដោយស្វ័យប្រវត្តិក្រោយពេល server reboot។

---

## ៤. បង្កើត Website Files លើកុំព្យូទ័ររបស់អ្នក

ក្នុង **PowerShell** បង្កើត folders និង `index.html`៖

```powershell
mkdir home, api, admin
Set-Content .\home\index.html '<h1>Home Website - Port 7001</h1>'
Set-Content .\api\index.html '<h1>API Website - Port 7002</h1>'
Set-Content .\admin\index.html '<h1>Admin Website - Port 7003</h1>'
```

រួច upload ពី PowerShell លើកុំព្យូទ័រអ្នក ទៅកាន់ EC2៖

```powershell
scp -i .\mykey.pem -r .\home .\api .\admin ubuntu@YOUR_ELASTIC_IP:/home/ubuntu/
```

---

## ៥. Copy Files ចូលក្នុង Containers

SSH ចូលទៅ EC2 ហើយរត់ command៖

```bash
docker cp /home/ubuntu/home/. home:/usr/share/nginx/html/
docker cp /home/ubuntu/api/. api:/usr/share/nginx/html/
docker cp /home/ubuntu/admin/. admin:/usr/share/nginx/html/
```

បើក Browser ដើម្បីសាកល្បង៖
- `http://YOUR_ELASTIC_IP:7001`
- `http://YOUR_ELASTIC_IP:7002`
- `http://YOUR_ELASTIC_IP:7003`

> [!NOTE]
> វិធី `docker cp` នេះល្អសម្រាប់ lab។ ប្រសិនបើលុប container ហើយបង្កើតថ្មី ត្រូវ copy files ចូលម្ដងទៀត។

---

## ៦. បើក Port 80 នៅ Security Group

1. ចូល **EC2 → Security Groups → ជ្រើស Security Group របស់ instance → Edit inbound rules**។
2. បន្ថែម៖

| Type | Port | Source |
| :--- | :--- | :--- |
| **HTTP** | `80` | `0.0.0.0/0` |

3. ចុច **Save rules**។

---

## ៧. ដំឡើង Nginx លើ EC2

SSH ចូល EC2 រួចរត់៖

```bash
sudo apt update
sudo apt install -y nginx
sudo systemctl enable --now nginx
```

> [!NOTE]
> Nginx លើ EC2 ប្រើ port `80`។ Containers របស់អ្នកប្រើ ports `7001–7003` ដូច្នេះអាចដំណើរការជាមួយគ្នាបានដោយមិនជាន់គ្នាឡើយ។

ពិនិត្យ containers៖

```bash
docker ps
```

សាកល្បង websites ពីក្នុង EC2៖

```bash
curl http://127.0.0.1:7001
curl http://127.0.0.1:7002
curl http://127.0.0.1:7003
```

*(គួរឃើញ HTML របស់ Home, API និង Admin តាមលំដាប់)*

---

## ៨. កំណត់ Nginx Reverse Proxy

បង្កើត configuration file៖

```bash
sudo nano /etc/nginx/sites-available/web.conf
```

ដាក់ code ខាងក្រោម៖

```nginx
server {
    listen 80;
    server_name site.com www.site.com;

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    # Home container
    location / {
        proxy_pass http://127.0.0.1:7001;
    }

    # Redirect /api to /api/
    location = /api {
        return 301 /api/;
    }

    # API container
    location /api/ {
        proxy_pass http://127.0.0.1:7002/;
    }

    # Redirect /admin to /admin/
    location = /admin {
        return 301 /admin/;
    }

    # Admin container
    location /admin/ {
        proxy_pass http://127.0.0.1:7003/;
    }
}
```

- រក្សាទុកដោយចុច `Ctrl + O` → `Enter` → `Ctrl + X`។

> [!NOTE]
> **ហេតុអ្វី `7002/` និង `7003/` មាន `/` នៅខាងចុង?**
> វាធ្វើឱ្យ Nginx បញ្ជូន `/api/` ទៅ `/` ក្នុង API container និង `/admin/` ទៅ `/` ក្នុង Admin container ដែលមាន `index.html` របស់អ្នក ([Nginx Docs](https://nginx.org/en/docs/http/ngx_http_proxy_module.html))។

បើក configuration (បង្កើត symlink ទៅ `sites-enabled`)៖

```bash
sudo ln -s /etc/nginx/sites-available/web.conf /etc/nginx/sites-enabled/web.conf
```

បើវាប្រាប់ File exists សូមពិនិត្យថា link មានរួចហើយ៖

```bash
ls -l /etc/nginx/sites-enabled/web.conf
```

ពិនិត្យ configuration៖

```bash
sudo nginx -t
```

បើឃើញ `syntax is ok` និង `test is successful` រួច reload Nginx៖

```bash
sudo systemctl reload nginx
```

> [!TIP]
> Path ត្រឹមត្រូវគឺ `sites-available` និង `sites-enabled`។

---

## ៩. កំណត់ Route 53

### ៩.១ បង្កើត Hosted Zone

1. ចូល [AWS Route 53](https://console.aws.amazon.com/route53/)
2. ចុច **Hosted zones → Create hosted zone**
3. បំពេញព័ត៌មាន៖
   - **Domain name:** `site.com`
   - **Type:** `Public hosted zone`
4. ចុច **Create hosted zone**

*(បើមាន Hosted Zone សម្រាប់ domain នេះរួចហើយ សូមប្រើរបស់ដែលមាន)*

### ៩.២ ភ្ជាប់ Nameservers ទៅ domain

1. ក្នុង Hosted Zone រក record ប្រភេទ **NS** ហើយ copy Nameservers ទាំង ៤។
2. ចូលគេហទំព័រដែលអ្នកទិញ domain → ផ្នែក **Nameservers** → ដាក់ Nameservers របស់ Route 53 ទាំង ៤ រួច Save។
3. វាធ្វើឱ្យ domain ប្រើ Route 53 ជា DNS provider។ បើ domain ប្រើ Nameservers នេះរួចហើយ មិនចាំបាច់ប្ដូរទៀតទេ ([Amazon Route 53 Documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/migrate-dns-domain-in-use.html))។

### ៩.៣ បង្កើត A Records

ក្នុង Hosted Zone ចុច **Create record** ហើយបង្កើត ២ records៖

| Setting | Domain ដើម | www |
| :--- | :--- | :--- |
| **Record name** | *(ទុកទទេ)* | `www` |
| **Record type** | `A` | `A` |
| **Value** | `YOUR_ELASTIC_IP` | `YOUR_ELASTIC_IP` |
| **Alias** | `Off` | `Off` |
| **TTL** | `300` | `300` |
| **Routing policy** | `Simple` | `Simple` |

> [!NOTE]
> A records ទាំងនេះចង្អុល domain ទៅ Elastic IP របស់ EC2។ **មិនត្រូវ**បង្កើត DNS records សម្រាប់ `/api` ឬ `/admin` ទេ ព្រោះទាំងនេះជា URL paths ដែល Nginx គ្រប់គ្រង ([Amazon Route 53 Documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring-new-domain.html))។

---

## ១០. សាកល្បង

លើ **Windows PowerShell** ពិនិត្យ DNS៖

```powershell
nslookup site.com
nslookup www.site.com
```

*(លទ្ធផលគួរបង្ហាញ Elastic IP របស់អ្នក។ បើទើបប្ដូរ Nameservers អាចត្រូវរង់ចាំឱ្យ DNS update/propagate)*

បើក Browser៖
- `http://site.com/`
- `http://site.com/api/`
- `http://site.com/admin/`

| URL | លទ្ធផលរំពឹងទុក |
| :--- | :--- |
| `http://site.com/` | Home Website - Port 7001 |
| `http://site.com/api/` | API Website - Port 7002 |
| `http://site.com/admin/` | Admin Website - Port 7003 |

អ្នកអាចសាកល្បង Nginx មុន DNS update បាន ដោយរត់លើ EC2៖

```bash
curl -H "Host: site.com" http://127.0.0.1/
curl -H "Host: site.com" http://127.0.0.1/api/
curl -H "Host: site.com" http://127.0.0.1/admin/
```

> [!TIP]
> ក្រោយចូលតាម domain បានជោគជ័យហើយ អ្នកអាចលុប inbound rules `7001–7003` ចេញពី Security Group បាន ដើម្បីឱ្យការចូលពីខាងក្រៅទាំងអស់ឆ្លងកាត់តែ Nginx port `80`។ Nginx នៅតែអាចភ្ជាប់ទៅ containers តាម `127.0.0.1:7001–7003` បានធម្មតា។

---

## Docker Commands សម្រាប់ពិនិត្យ និងគ្រប់គ្រង

```bash
docker images -a               # មើល images ទាំងអស់
docker ps                      # មើល containers កំពុងដំណើរការ
docker ps -a                   # មើល containers ទាំងអស់ (រួមទាំង stopped)
docker logs home               # មើល logs របស់ container home
docker exec -it home /bin/bash # ចូលក្នុង shell របស់ container
docker rm -f home              # បង្ខំលុប container home
docker rmi nginx:latest        # លុប image ក្រោយគ្មាន container ប្រើវា
```

> [!TIP]
> **ចំណាំកែសម្រួលអក្ខរាវិរុទ្ធ (Typo Corrections):**
> - `whomai` ត្រូវកែជា `whoami`
> - `nignx:latest` ត្រូវកែជា `nginx:latest`
