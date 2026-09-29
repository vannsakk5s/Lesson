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
