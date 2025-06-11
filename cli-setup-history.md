# ✅ Format the new EBS volume (replace xvdbf with your actual device if different)
sudo mkfs -t ext4 /dev/xvdbf

# ✅ Create a mount directory
sudo mkdir /mnt/data

# ✅ Mount the volume to the directory
sudo mount /dev/xvdbf /mnt/data

# ✅ Make mount persistent on reboot
echo "/dev/xvdbf /mnt/data ext4 defaults,nofail 0 2" | sudo tee -a /etc/fstab

# ✅ Remove any older PHP versions
sudo yum remove php*

# ✅ Enable PHP 7.4 on Amazon Linux 2
sudo amazon-linux-extras enable php7.4

# ✅ Clean yum metadata
sudo yum clean metadata

# ✅ Install PHP 7.4 and necessary extensions
sudo yum install php php-mysqlnd php-fpm php-opcache php-gd php-xml php-mbstring -y

# ✅ Restart Apache server to load new PHP config
sudo systemctl restart httpd

# ✅ Verify PHP version
php -v
