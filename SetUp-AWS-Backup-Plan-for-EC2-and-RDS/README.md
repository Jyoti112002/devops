
# AWS Backup Plan for EC2 and RDS

## 📌 Project Title
**Set Up AWS Backup Plan for EC2 and RDS**

## 🎯 Objective
Configure and manage automated backup and recovery for AWS resources using **AWS Backup** by protecting:
- 1 EC2 Instance
- 1 RDS MySQL Database

---

## 🏗️ PART 1: Infrastructure Setup

### Step 1: Launch EC2 Instance
- **Name:** Backup-EC2-Server  
- **AMI:** Amazon Linux 2  
- **Instance Type:** t2.micro (Free Tier)  
- **Security Group Rules:**
  - SSH (22) – Your IP
  - HTTP (80) – Anywhere

Launch the instance.

---

### Step 2: Connect to EC2
```bash
ssh -i your-key.pem ec2-user@<EC2-Public-IP>
```

---

### Step 3: Install Web Server
```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

---
![screenshot](./screenshots/2.png)
### Step 4: Create Test Data
```bash
cd /var/www/html
sudo nano index.html
```

Add:
```html
<h1>AWS Backup Project - Jyotirmoyee Rout</h1>
<p>This is EC2 test data for backup validation.</p>
```

Access in browser:
```
http://<EC2-Public-IP>
```

---
![screenshot](./screenshots/3.png)
### Step 5: Add Tag to EC2
| Key    | Value |
|-------|-------|
| Backup | Yes   |

---

## 🗄️ PART 2: Launch RDS Instance

### Step 6: Create RDS Database
- **Engine:** MySQL  
- **DB Identifier:** backup-db  
- **Username:** admin  
- **Public Access:** Yes  
- **Security Group:** MySQL (3306) – Your IP  

---
![screenshot](./screenshots/4.png)
### Step 7: Connect to RDS
```bash
mysql -h <RDS-endpoint> -u admin -p
```

---

### Step 8: Create Test Database & Data
```sql
CREATE DATABASE backup_test;
USE backup_test;

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100)
);

INSERT INTO users (name, email)
VALUES ('jyotirmoyee', 'jyotirmoyeerout8@gmail.com');

SELECT * FROM users;
```

---
![screenshot](./screenshots/6.png)
### Step 9: Add Tag to RDS
| Key    | Value |
|-------|-------|
| Backup | Yes   |

---
![screenshot](./screenshots/5.png)
## 🔐 PART 3: AWS Backup Setup

### Step 10: Open AWS Backup
Search **AWS Backup** in AWS Console.

---

### Step 11: Create Backup Vault
- **Vault Name:** MyProjectVault  
- **Encryption:** Default  
![screenshot](./screenshots/7.png)
---

### Step 12: Create Backup Plan
- **Plan Name:** EC2-RDS-Backup-Plan  
![screenshot](./screenshots/8.png)
![screenshot](./screenshots/9.png)
---

### Step 13: Configure Backup Rule
- **Rule Name:** DailyBackupRule  
- **Frequency:** Daily  
- **Retention:** 7 Days  
- **Cold Storage:** After 2 Days (Optional)

---

### Step 14: Assign Resources
- **Assignment Name:** EC2-RDS-Assignment  
- **Method:** Assign by Tags  
- **Tag:** Backup = Yes  

---
![screenshot](./screenshots/10.png)
## ▶️ PART 4: On-Demand Backup (Testing)

1. Go to **AWS Backup → Protected Resources**
2. Select EC2 → Create on-demand backup
3. Repeat for RDS

---

## ✅ PART 5: Validation

### Step 16: Verify Backup Jobs
Navigate to:
```
AWS Backup → Backup Jobs
```
![screenshot](./screenshots/11.png)
Status should be **Completed**.

---

### Step 17: Verify Recovery Points
Navigate to:
```
AWS Backup → Backup Vaults → MyProjectVault → Recovery Points
```
You should see:
- EC2 Recovery Point
- RDS Recovery Point

---
![screenshot](./screenshots/12.png)
## 🧪 Final Outcome
✔ Automated daily backups  
✔ Tag-based resource selection  
✔ Secure backup vault  
✔ Verified recovery points  

---

## 👨‍💻 Author
**Jyotirmoyee Rout**  
AWS Cloud & Linux Enthusiast
