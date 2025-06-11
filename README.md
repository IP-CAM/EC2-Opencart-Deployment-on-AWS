# 🛒 EC2 OpenCart Deployment on AWS

This project demonstrates how to deploy the OpenCart e-commerce platform on an EC2 instance using Amazon Linux 2. The deployment uses Apache, PHP, and EBS for secure storage. Additionally, the setup includes an optional IAM Role for S3 backups and assigns an Elastic IP for consistent access.

---

## 📌 Project Goals

- Launch and configure an EC2 instance using the AWS Console (GUI).
- Install and configure Apache, PHP, and OpenCart via user-data script.
- Attach and mount an encrypted EBS volume.
- Assign a static Elastic IP for public access.
- (Optional) Create an IAM role to allow S3 backups from the EC2 instance.

---

## 🧱 Architecture Overview

| Layer       | Technology Used      | Purpose                          |
|------------|----------------------|----------------------------------|
| Web Tier   | EC2 (Amazon Linux 2) | Apache server + PHP              |
| App Tier   | OpenCart             | E-commerce web application       |
| Storage    | EBS (20 GiB, Encrypted) | Store web files securely     |
| IP         | Elastic IP           | Fixed public access              |
| Backup     | S3 via IAM Role      | Optional backup access to S3     |

---

## 📸 Screenshots

| Feature                             | Screenshot File             |
|-------------------------------------|-----------------------------|
| EC2 Instance Configuration          | `![Image](https://github.com/user-attachments/assets/18b73f56-35a3-4a4f-b1a9-b6135693d1bb)`          |
| OpenCart Homepage via Elastic IP    | `opencart-homepage.png`     |
| EBS Volume Mounted                  | `ebs-mount.png`             |
| User Data Script in EC2 Launch      | `user-data-script.png`      |
| IAM Role for S3 Backup (Optional)   | `iam-role.png`              |

---

## ⚙️ User Data Script Used

```bash
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd php php-mysqlnd wget unzip
sudo systemctl start httpd
sudo systemctl enable httpd

cd /var/www/html
wget https://github.com/opencart/opencart/releases/download/3.0.3.8/opencart-3.0.3.8.zip
unzip opencart-3.0.3.8.zip
cp -r upload/* .
rm -rf upload opencart-3.0.3.8.zip

sudo chown -R apache:apache /var/www/html
sudo chmod -R 755 /var/www/html
