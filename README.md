# 🛒 EC2 Lab – Deploy an E-Commerce Web Server (GUI Mode)
📌 Objective
Deploy an e-commerce platform (OpenCart) on an Amazon Linux EC2 instance using AWS GUI and CLI. Configure storage with encrypted EBS, assign an Elastic IP, and optionally attach an IAM role for S3 backup.

🏗️ Architecture Overview
Layer	Technology	Purpose
Web	EC2 (Amazon Linux)	Host Apache + PHP server
App	OpenCart	E-commerce Platform
Storage	Encrypted EBS	Data storage
IP	Elastic IP	Static public IP for the server
Backup	S3 (optional)	Store backup images via IAM Role

⚙️ Step-by-Step Setup
✅ Step 1: Launch EC2 Instance
AMI: Amazon Linux 2

Instance Type: t2.micro

Security Group:

HTTP (Port 80): Anywhere

SSH (Port 22): Your IP

Key Pair: Create/download one

User Data:

bash
Copy
Edit
#!/bin/bash
yum update -y
yum install -y httpd php php-mysqlnd wget unzip
systemctl start httpd
systemctl enable httpd
cd /var/www/html
wget https://github.com/opencart/opencart/releases/download/3.0.3.8/opencart-3.0.3.8.zip
unzip opencart-3.0.3.8.zip
cp -r upload/* .
rm -rf upload opencart-3.0.3.8.zip
chown -R apache:apache /var/www/html
chmod -R 755 /var/www/html
✅ Step 2: Attach and Mount EBS Volume
Create Volume (20 GiB, encrypted):
AZ: Same as EC2 (e.g., us-east-1a)

Volume Type: gp3

Encryption: Enabled

Attach Volume:
Use /dev/xvdf as device name.

Mount Volume (via SSH):
bash
Copy
Edit
sudo mkfs -t ext4 /dev/xvdf
sudo mkdir /mnt/data
sudo mount /dev/xvdf /mnt/data
echo "/dev/xvdf /mnt/data ext4 defaults,nofail 0 2" | sudo tee -a /etc/fstab
✅ Step 3: Fix PHP Version Issue
If OpenCart requires PHP > 7.3, install PHP 7.4:

bash
Copy
Edit
sudo yum remove php*
sudo amazon-linux-extras enable php7.4
sudo yum clean metadata
sudo yum install php php-mysqlnd php-fpm php-opcache php-gd php-xml php-mbstring -y
sudo systemctl restart httpd
php -v
✅ Step 4: Assign Elastic IP
bash
Copy
Edit
# From AWS Console:
EC2 → Elastic IP → Allocate → Associate to your instance
✅ Step 5 (Optional): Attach IAM Role for S3
IAM Role Policy:
json
Copy
Edit
{
  "Effect": "Allow",
  "Action": ["s3:PutObject", "s3:GetObject", "s3:ListBucket"],
  "Resource": ["arn:aws:s3:::your-bucket-name/*"]
}
Attach this role to EC2 instance.

🖼️ Screenshots (Upload in /screenshots folder)
EC2 running
![Image](https://github.com/user-attachments/assets/67a55780-5f27-42eb-8cec-3e56c3632efd)

EBS attached and mounted
![Image](https://github.com/user-attachments/assets/f94ce99a-1f71-4d99-a168-d57b00a8e29c)

OpenCart homepage via IP
![Image](https://github.com/user-attachments/assets/acd61a83-3133-4996-91a1-ae01e20457e7)

CloudWatch (if using IAM Role)
![Image](https://github.com/user-attachments/assets/73d0c7cb-1db2-4ed9-881f-939eb09dc0c9)

Final directory structure
![Image](https://github.com/user-attachments/assets/0b658fc1-d6ba-4b1f-b528-37d6b08643e7)

💻 CLI History (save this as cli-commands.txt or a GitHub branch)
bash
Copy
Edit
sudo mkfs.ext4/dev/xvdbf
sudo mkfs -t ext4 /dev/xvdbf
sudo mkdir /mnt/data
sudo mount /dev/xvdbf /mnt/data
echo "/dev/xvdbf /mnt/data ext4 defaults,nofail 0 2" | sudo tee -a /etc/fstab
sudo yum remove php*
sudo amazon-linux-extras enable php7.4
sudo yum clean metadata
sudo yum install php php-mysqlnd php-fpm php-opcache php-gd php-xml php-mbstring -y
sudo systemctl restart httpd
php -v





