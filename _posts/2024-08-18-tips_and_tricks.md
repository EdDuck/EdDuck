---
title: "Tips and scripts"
excerpt_separator: "<!--more-->"
categories:
  - Tips
tags:
  - tips
  - scripts
---

# Обновление Ubuntu с 18.04 до 20.04

```bash
vim /etc/apt/sources.list.d/google-cloud-sdk.list
```

```bash
#deb [signed-by=/usr/share/keyrings/cloud.google.gpg] [https://packages.cloud.google.com/apt](https://packages.cloud.google.com/apt) cloud-sdk main
```

```bash
apt update
apt upgrade
```

1. **Backup Your Data**: Before beginning the upgrade process, it's crucial to back up your server. This ensures that you can recover your data in case something goes wrong during the upgrade.
2. **Update Currently Installed Packages**:
    - Run **`sudo apt update`** to update the package list.
    - Use **`sudo apt upgrade`** to upgrade all current packages to the latest version.
    - Apply **`sudo apt dist-upgrade`** for a thorough upgrade.
3. **Install the Update Manager**:
    - Execute **`sudo apt install update-manager-core`** if it's not already installed.
4. **Configure the Update Manager**:
    - Open the update manager configuration file by typing **`sudo nano /etc/update-manager/release-upgrades`**.
    - Ensure that the **`Prompt`** line is set to **`lts`**. This configures the update manager to look for the latest LTS (long-term support) version.
5. **Begin the Upgrade**:
    - Start the upgrade process with **`sudo do-release-upgrade`**.
    - Follow the on-screen instructions. The server might ask for your confirmation at various steps.
6. **Monitor the Upgrade Process**:
    - The upgrade process may take some time. It's important to keep an eye on it, as it might ask for your input, especially when dealing with configuration files.
7. **Reboot the Server**:
    - After the upgrade is complete, reboot your server using **`sudo reboot`**.
8. **Check the System Version**:
    - Once your server is back online, check the new version by typing **`lsb_release -a`**.
9. **Update and Clean Up**:
    - Finally, run **`sudo apt update`** followed by **`sudo apt upgrade`** to update any remaining packages.
    - Clean up old packages with **`sudo apt autoremove`**.

# How to upgrade Ubuntu from 20.04 to the latest LTS version

---

## Step 1: Backup Your Data

Before starting the upgrade process, it’s crucial to back up your data to avoid any potential loss. You can use tools like **Deja Dup** (Ubuntu’s default backup tool), **rsync**, or **tar** to back up your files.

## Step 2: Update Currently Installed Packages

Ensure that all your currently installed packages are up to date with these commands:

```bash
sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade
```

## Step 3: Install the **`update-manager-core`** Package

If it’s not already installed, you need to install the **`update-manager-core`** package:

```bash
sudo apt install update-manager-core
```

## Step 4: Begin the Upgrade Process

Start the upgrade process by running the following command:

```bash
sudo do-release-upgrade
```

Follow the on-screen instructions to complete the upgrade process.

## Step 5: Restart Your System

Once the upgrade is complete, restart your system to apply the changes:

```bash
sudo reboot
```

---

After the reboot, your system should be running the latest Ubuntu LTS version. [Remember to check for any post-upgrade tasks that may be necessary to ensure all your software and services are running correctly](https://www.howtogeek.com/351360/how-to-upgrade-to-the-latest-version-of-ubuntu/)

# **MySQL Database Creation and Privilege Management Script**

[](https://github.com/egubaidullin/scripts/blob/main/create_databases.sh)

# MySQL User Creation Script with Password Generation

```bash
#!/bin/bash
# MySQL User Creation Script with Password Generation

# Function to generate a random password
generate_password() {
    tr -dc 'A-Za-z0-9' </dev/urandom | head -c 20 ; echo ''
}

# Check if the 'webmaster' user exists in MySQL
USER_EXISTS=$(mysql -u root -sse "SELECT EXISTS(SELECT 1 FROM mysql.user WHERE user = 'webmaster' AND host = 'localhost')")
if [ "$USER_EXISTS" = 1 ]; then
    echo "User 'webmaster' already exists in MySQL."
else
    # Generate a random password
    PASSWORD=$(generate_password)

    # Create the 'webmaster' user with the generated password
    mysql -u root -e "CREATE USER 'webmaster'@'localhost' IDENTIFIED BY '$PASSWORD';"
    echo "User 'webmaster' has been created in MySQL."

    # Grant all privileges to the 'webmaster' user (adjust privileges as necessary)
    mysql -u root -e "GRANT ALL PRIVILEGES ON *.* TO 'webmaster'@'localhost' WITH GRANT OPTION;"
    echo "All privileges have been granted to 'webmaster'."

    # Flush privileges to ensure they are applied
    mysql -u root -e "FLUSH PRIVILEGES;"
    echo "Privileges have been flushed and applied."

    # Save the password to a file
    echo "$PASSWORD" > webmaster-pass.txt
    echo "The password for 'webmaster' has been saved to 'webmaster-pass.txt'."

    # Display password information to the user
    echo "The generated password for 'webmaster' is: $PASSWORD"
fi

```

**Recommendations:**

- Replace `root` with another user if the root user should not be used.
- Ensure that the script is run by a user with sufficient privileges to create users and grant privileges in MySQL.
- Securely store the `webmaster-pass.txt` file as it contains sensitive information.
- Regularly update the password for the `webmaster` user to adhere to security best practices.

This script will check if a MySQL user named `webmaster` exists, and if not, it will create the user, generate a random password, assign it, save the password to `webmaster-pass.txt`, and inform the user of the new password. Make sure to run this script in a secure environment and handle the generated password file with care. Let me know if you need any more help! 😊

# Установка PHP 8.1-FPM на Ubuntu 20.04 для использования с Nginx

1. **Update Package List**:
    
    ```bash
    sudo apt update
    ```
    
2. **Install Required Packages**:
    
    ```bash
    sudo apt install -y software-properties-common
    ```
    
3. **Add Ondřej Surý's PPA**:
    
    ```bash
    sudo add-apt-repository ppa:ondrej/nginx
    ```
    
4. **Update Package List Again**:
    
    ```bash
    sudo apt update
    ```
    
5. **Install PHP 8.1-FPM**:
    
    ```bash
    sudo apt install php8.1-fpm
    ```
    

This guide will help you set up the latest PHP version on your Ubuntu system. Remember to replace `php8.1-fpm` with the specific PHP version you need.

# Установка PHP 7.4-FPM на Ubuntu 22.04 для использования с Nginx

---

**Обновление системы**

Перед началом установки обновите список пакетов и установленные пакеты:

```bash
sudo apt update && sudo apt upgrade -y
```

**Добавление репозитория PPA**

Добавьте PPA репозиторий Ondřej Surý для доступа к PHP 7.4:

```bash
sudo add-apt-repository ppa:ondrej/php -y
```

**Установка PHP 7.4-FPM**

Установите PHP 7.4-FPM и необходимые модули:

```bash
sudo apt install php7.4-fpm php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl -y
```

**Настройка Nginx для использования PHP-FPM**

Отредактируйте конфигурационный файл Nginx для вашего сайта, чтобы использовать PHP-FPM:

```bash
**server** {
    listen 80;
    server_name example.com;
    root /var/www/html;

    index index.php index.html index.htm index.nginx-debian.html;

    **location** / {
        try_files $uri $uri/ =404;
    }

    **location** ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    }

    **location** ~ /\.ht {
        deny all;
    }
}
```

**Перезапуск Nginx и PHP-FPM**

Перезапустите Nginx и PHP-FPM для применения изменений:

```bash
sudo systemctl restart nginx
sudo systemctl restart php7.4-fpm
```

# How to Install PHP 8.1-FPM on Ubuntu 22.04

PHP 8.1-FPM is a server-side, HTML-embedded scripting language that’s commonly used for web development. In this short manual, I’ll guide you through the installation process on Ubuntu 22.04. We’ll ensure that all necessary modules for running WordPress are included.

**Update the package list:**

```bash
sudo apt update
```

**Install PHP 8.1-FPM:**

```bash
sudo apt -y install php8.1-fpm
```

**Verify Installation**

Check your PHP version:

```bash
php -v
```

You should see output similar to this:

```
PHP 8.1.2 (cli) (built: Apr 7 2022 17:46:26) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.2, Copyright (c) Zend Technologies
with Zend OPcache v8.1.2, Copyright (c), by Zend Technologies
```

**Install PHP Extensions**

Install the required PHP extensions with the following command:

```bash
sudo apt install php8.1-common php8.1-mysql php8.1-gd php8.1-cli php8.1-mbstring php8.1-opcache php8.1-readline php8.1-xml php8.1-curl php8.1-mysql
```

**Uninstallation**

To uninstall only the PHP 8.1-FPM package:

```bash
sudo apt-get remove php8.1-fpm
```

To remove PHP 8.1-FPM and its dependencies no longer needed by Ubuntu 22.04:

```bash
sudo apt-get -y autoremove php8.1-fpm
```

To completely remove PHP 8.1-FPM configurations, data, and all dependencies:

```bash
sudo apt-get -y autoremove --purge php8.1-fpm
```

**Restart PHP-FPM**

To apply the changes, restart PHP-FPM:

```bash
sudo systemctl restart php8.1-fpm
```

# Установка Nginx и Certbot для Nginx на Ubuntu 22.04

---

## Шаг 1: Обновление системы

Перед началом установки обновите список пакетов и установленные пакеты:

```bash
sudo apt update && sudo apt upgrade -y
```

## Шаг 2: Установка Nginx

Установите Nginx, выполнив следующую команду:

```bash
sudo apt install nginx -y
```

## Шаг 3: Настройка брандмауэра

Если вы используете UFW (Uncomplicated Firewall), разрешите трафик HTTP и HTTPS:

```bash
sudo ufw allow 'Nginx Full'
```

## Шаг 4: Установка Certbot

Установите Certbot и модуль Certbot Nginx:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

## Шаг 5: Получение SSL-сертификата

Запустите Certbot для получения SSL-сертификата:

```bash
sudo certbot --nginx
```

Следуйте инструкциям на экране, чтобы завершить процесс.

## Шаг 6: Автоматическое обновление сертификата

Certbot автоматически добавляет cronjob или systemd timer для обновления сертификатов. Вы можете проверить статус автоматического обновления:

```bash
sudo systemctl status certbot.timer
```

---

[Теперь Nginx и Certbot должны быть установлены на вашем сервере Ubuntu 22.04, и ваш веб-сайт должен быть защищен SSL-сертификатом1](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-22-04)[2](https://blog.tekspace.io/how-to-setup-lets-encrypt-ssl-certificate-with-nginx-on-ubuntu-22-04/)[3](https://tech.sadaalomma.com/ubuntu/how-to-install-certbot-on-ubuntu-22-04-nginx/)[4](https://installati.one/install-python3-certbot-nginx-ubuntu-22-04/)[5](https://www.tutsmake.com/how-to-install-lets-encrypt-on-ubuntu-22-04-nginx/).

# User Creation: deployer

```bash
#!/bin/bash
cat /etc/passwd
sudo groupadd -g 1000 deployer
sudo useradd -u 1000 -g 1000 -d /home/deployer -m -s /bin/bash deployer

# Add the deployer user to sudoers file with NOPASSWD
echo 'deployer ALL=(ALL) NOPASSWD: ALL' | sudo EDITOR='tee -a' visudo

# Create directories /home/deployer/www/prod/ and set deployer as the owner
sudo mkdir -p /home/deployer/www/prod/
sudo chown -R deployer:deployer /home/deployer/www

password=$(tr -dc 'a-zA-Z0-9' < /dev/urandom | head -c12)
sudo usermod -p $(openssl passwd -1 "$password") deployer
sudo usermod -aG www-data deployer
id deployer
groups deployer
echo "Password for deployer: $password"

# sudo passwd deployer
```

Этот bash скрипт выполняет следующие действия:

1. Просматривает существующих пользователей
2. Добавляет группу deployer
3. Создает домашнюю директорию deployer
4. Добавляет пользователя deployer
5. Генерирует случайный пароль
6. Назначает пароль пользователю
7. Добавляет в группы
8. Проверяет ID и группы пользователя
9. Выводит сгенерированный пароль

Тем самым автоматизируется создание пользователя с назначением случайного пароля.

# Установка PHP 7.4-FPM на Ubuntu 20.04 для использования с Nginx.

**Обновление системы:**
Сначала обновите список пакетов и установите последние версии всех пакетов.

```bash
sudo apt update
sudo apt upgrade
```

1. **Добавление репозитория PHP:**
Добавьте внешний репозиторий, который содержит последние версии PHP.
    
    ```bash
    sudo apt install software-properties-common
    sudo add-apt-repository ppa:ondrej/php
    sudo apt update
    ```
    
2. **Установка PHP 7.4 и PHP 7.4-FPM:**
Теперь установите PHP 7.4 и модуль FastCGI Process Manager (FPM) для PHP.
    
    ```bash
    sudo apt install php7.4 php7.4-fpm
    ```
    
3. **Настройка PHP 7.4 как версии по умолчанию:**
Используйте `update-alternatives` для настройки PHP 7.4 как версии по умолчанию.
    
    ```bash
    sudo update-alternatives --set php /usr/bin/php7.4
    ```
    
4. **Конфигурация PHP-FPM:**
Настройте PHP-FPM для работы с Nginx. Обычно это требует изменения конфигурации пула (pool).
    - Отредактируйте файл `/etc/php/7.4/fpm/pool.d/www.conf`.
    - Убедитесь, что сокет или TCP-подключение настроены правильно для общения с Nginx.
5. **Перезапуск PHP-FPM:**
Перезапустите PHP-FPM, чтобы применить изменения.
    
    ```bash
    sudo systemctl restart php7.4-fpm
    ```
    
6. **Конфигурация Nginx для использования PHP-FPM:**
Настройте Nginx для обработки PHP-файлов через PHP-FPM. Для этого добавьте следующий блок в конфигурацию сайта Nginx:
    
    ```
    location ~ \\.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
    }
    ```
    
7. **Перезагрузка Nginx:**
Перезапустите Nginx, чтобы применить новую конфигурацию.
    
    ```bash
    sudo systemctl restart nginx
    ```
    
8. **Проверка установки:**
Создайте файл `info.php` в корневом каталоге вашего сайта с содержимым `<?php phpinfo(); ?>` для проверки работы PHP.
9. **Безопасность:**
Убедитесь, что ваша конфигурация безопасна, проверив настройки PHP и Nginx.

# Установка MariaDB на сервере Ubuntu 22.04

---

## Шаг 1: Обновление системы

Перед установкой MariaDB обновите список пакетов и установленные пакеты:

```bash
sudo apt update && sudo apt upgrade -yphp8ffffff
```

## Шаг 2: Установка MariaDB

Установите MariaDB, выполнив следующую команду:

```bash
sudo apt install mariadb-server -y
```

## Шаг 3: Настройка MariaDB

После установки запустите скрипт безопасности, чтобы настроить базовые параметры безопасности:

```bash
sudo mysql_secure_installation
```

Следуйте инструкциям на экране, чтобы установить пароль для пользователя root и выполнить другие рекомендуемые действия по безопасности.

## Шаг 4: Проверка статуса MariaDB

Убедитесь, что служба MariaDB запущена и работает:

```bash
sudo systemctl status mariadb
```

## Шаг 5: Настройка брандмауэра (если необходимо)

Если вы используете брандмауэр UFW, разрешите трафик MySQL/MariaDB:

```bash
sudo ufw allow 3306/tcp
sudo ufw reload
```

---

# **UFW Script for SSH and HTTP/HTTPS Access**

## IP List File `ip_list.txt`

```bash
130.211.0.0/16
142.250.0.0/15
146.148.0.0/17
162.216.148.0/22
162.222.176.0/21
172.110.32.0/21
172.217.0.0/16
172.253.0.0/16
173.194.0.0/16
173.255.112.0/20
192.158.28.0/22
192.178.0.0/15
193.186.4.0/24
199.36.154.0/23
199.36.156.0/24
199.192.112.0/22
199.223.232.0/21
207.223.160.0/20
208.65.152.0/22
208.68.108.0/22
208.81.188.0/22
208.117.224.0/19
209.85.128.0/17
216.58.192.0/19
216.73.80.0/20
216.239.32.0/19
34.1.128.0/20
34.21.128.0/17
34.87.0.0/17
34.87.128.0/18
34.104.58.0/23
34.104.106.0/23
34.124.42.0/23
34.124.128.0/17
34.126.64.0/18
34.126.128.0/18
34.128.44.0/23
34.128.60.0/23
34.142.128.0/17
34.143.128.0/17
34.157.82.0/23
34.157.88.0/23
34.157.210.0/23
35.185.176.0/20
35.186.144.0/20
35.187.224.0/19
35.197.128.0/19
35.198.192.0/18
35.213.128.0/18
35.220.24.0/23
35.234.192.0/20
35.240.128.0/17
35.242.24.0/23
35.247.128.0/18
34.34.216.0/21
34.50.64.0/18
34.101.18.0/24
34.101.20.0/22
34.101.24.0/22
34.101.32.0/19
34.101.64.0/18
34.101.128.0/17
34.128.64.0/18
34.152.68.0/24
34.157.254.0/24
35.219.0.0/17
124.248.177.33/32
108.174.54.201/32
34.80.90.195/32
35.221.136.253/32
95.65.0.120/32
```

## UFW Configuration Script

This script is designed to configure the Uncomplicated Firewall (UFW) on a Linux system. It performs the following actions:

1. **Set SSH Port Variable**: Defines the SSH port number that will be used in the firewall rules.
2. **Convert Line Endings to UNIX Format**: Includes a function to check and convert the line endings of a given file to the UNIX format (LF), ensuring compatibility with UNIX-based systems.
3. **Check and Convert IP List File**: Executes the line-ending conversion function on the `ip_list.txt` file, which contains a list of IP addresses.
4. **Reload UFW Rules**: Clears all existing UFW rules without disrupting active connections by reloading the firewall.
5. **Allow HTTP and HTTPS Traffic**: Opens ports for HTTP (port 80) and HTTPS (port 443) traffic, allowing web server communication.
6. **Allow SSH Access for Listed IPs**: Iterates through the `ip_list.txt` file and adds firewall rules to allow SSH access for each IP address listed.
7. **Set Default Policies**: Establishes the default policy to deny all incoming traffic except for the services explicitly allowed, and to permit all outgoing traffic.
8. **Enable UFW**: Activates the UFW with the newly defined rules.

### Usage Instructions

- Run the script as a superuser (root) to ensure it has the necessary permissions to modify firewall settings.
- Before executing, verify that the `ip_list.txt` file contains all the IP addresses that should be allowed SSH access.
- Be cautious with the `sudo ufw reload` command, as it will apply all current rules. Ensure that the rules are correctly set up before reloading.
- After running the script, always check the status of UFW with `sudo ufw status` to confirm that the rules have been applied correctly.

```bash
#!/bin/bash

# Variable for the SSH port
SSH_PORT=22

# Function to check and convert file line endings to UNIX format
convert_to_unix_format() {
    # Check if the file has Windows line endings
    if file "$1" | grep -q "CRLF"; then
        # Convert file to UNIX line endings (LF)
        echo "Converting $1 to UNIX format..."
        sed -i 's/\r$//' "$1"
    else
        echo "File $1 is already in UNIX format."
    fi
}

# Check and convert the IP list file to UNIX format
convert_to_unix_format "ip_list.txt"

# Clear all existing UFW rules
sudo ufw reload 

# Allow HTTP and HTTPS
sudo ufw allow http
sudo ufw allow https

# Allow SSH for the IPs in the list
while IFS= read -r ip; do
    sudo ufw allow from "$ip" to any port "$SSH_PORT"
done < "ip_list.txt"

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Enable UFW
sudo ufw enable

```

# Website Backup Script

This script automates the process of creating backups for specified websites. It archives the directories, logs the outcome of each operation, and uploads the archives to Google Cloud Storage.

```bash
#!/bin/bash
SITE_LOCATION="/var/www"
BACKUP_FOLDER="/opt/backups/local"
LOG_FILE="$BACKUP_FOLDER/site-backup.log"
gcloud auth activate-service-account storage@integrated-will-219907.iam.gserviceaccount.com --key-file /opt/backups/service-acc.json --project integrated-will-219907;
cd $BACKUP_FOLDER

SITE_NAMES=(
    "site-example.com" 
    "another-site.com" 
    "third-site.com"
)

for SITE_NAME in "${SITE_NAMES[@]}"
do
    if [ -d "$SITE_LOCATION/$SITE_NAME" ]; then
        tar -cvf $SITE_NAME.tar.gz $SITE_LOCATION/$SITE_NAME
        if [ $? -eq 0 ]; then
            FILE_SIZE_MB=$(du -m "$BACKUP_FOLDER/$SITE_NAME.tar.gz" | cut -f1)
            echo "SUCCESS: Backup created for $SITE_NAME (Size: $FILE_SIZE_MB MB)" >> $LOG_FILE
            gsutil cp $BACKUP_FOLDER/$SITE_NAME.tar.gz gs://external-backups/sites/$(date +%Y-%m-%d)/
            if [ $? -eq 0 ]; then
                echo "SUCCESS: $SITE_NAME.tar.gz uploaded to storage" >> $LOG_FILE
            else
                echo "ERROR: Upload failed for $SITE_NAME.tar.gz" >> $LOG_FILE
            fi
            rm -rf $SITE_NAME.tar.gz
        else
            echo "ERROR: Backup creation failed for $SITE_NAME" >> $LOG_FILE
        fi
    else
        echo "ERROR: Directory $SITE_LOCATION/$SITE_NAME does not exist" >> $LOG_FILE
    fi
done

```

**Script Actions**

1. **Activate GCloud Service Account**: Authenticates with Google Cloud using a service account to enable access to Cloud Storage.
2. **Navigate to Backup Folder**: Changes the directory to the location where backups will be stored locally.
3. **Define Site Names**: Lists the domains of the websites to be backed up.
4. **Backup Process**: For each site listed:
    - Checks if the website directory exists.
    - Creates a `.tar.gz` archive of the website directory.
    - Logs the success of the backup creation and includes the file size in MB.
    - Uploads the archive to Google Cloud Storage.
    - Logs the success or failure of the upload.
    - Removes the local archive file.

**Variables**

- `SITE_LOCATION`: The directory where the website folders are located.
- `BACKUP_FOLDER`: The local directory where backups will be temporarily stored.
- `LOG_FILE`: The file where the script logs its operations.

**Commands**

- `tar`: Archives the directories.
- `gsutil cp`: Uploads the files to Google Cloud Storage.
- `du`: Calculates the file size in megabytes.
- `rm`: Removes the local backup file after upload.

**Logging**

- The script logs each operation's outcome, indicating success or error, along with the file size for the backup operation.

# Размер БД на диске

```sql
SELECT table_schema AS "Database",
SUM(data_length + index_length) / 1024 / 1024 AS "Size (MB)"
FROM information_schema.TABLES
GROUP BY table_schema;
```

или

```bash
SELECT table_schema "database name", sum( data_length + index_length ) / 1024 / 1024 "database size in MB", sum( data_free )/ 1024 / 1024 "free space in MB" FROM information_schema.TABLES GROUP BY table_schema;
```

# How to Delete a Database in MySQL

Here are the steps to delete a database in MySQL:

1. **Access MySQL Server**:
Log in to your MySQL server using the MySQL client. You can do this via the command line:
    
    ```bash
    mysql -u your_username -p
    ```
    
    Enter your password when prompted.
    
2. **List Databases**:
Display a list of all databases to ensure you target the correct one:
    
    ```sql
    SHOW DATABASES;
    ```
    
3. **Select the Database to Delete**:
Identify the database you want to delete from the list.
4. **Delete the Database**:
Use the `DROP DATABASE` command to delete the desired database:
    
    ```sql
    DROP DATABASE database_name;
    ```
    
    If you want to avoid an error if the database does not exist, use:
    
    ```sql
    DROP DATABASE IF EXISTS database_name;
    ```
    
5. **Confirm Deletion**:
Verify that the database has been deleted by listing the databases again:
    
    ```sql
    SHOW DATABASES;
    ```
    
6. **Exit MySQL**:
Exit the MySQL client:
    
    ```sql
    EXIT;
    ```
    

### Example

Here’s an example of deleting a database named `example_db`:

```bash
mysql -u root -p
```

```sql
SHOW DATABASES;
DROP DATABASE IF EXISTS example_db;
SHOW DATABASES;
EXIT;
```

### Important Notes

- **Privileges**: Ensure you have the `DROP` privilege on the database you want to delete.
- **Caution**: Deleting a database is irreversible. Make sure you have backups if necessary.

These steps should help you safely delete a database in MySQL. Let me know if you need further assistance!

# Медленные запросы и использование `mysqldumpslow`

## **Настройка `slow_query_log` для MariaDB/MySQL**

1. **Создание директории и файла для логов**:
    
    ```bash
    sudo mkdir -p /var/log/mysql/
    sudo touch /var/log/mysql/mariadb-slow.log
    sudo chown mysql:mysql /var/log/mysql/mariadb-slow.log
    ```
    
2. **Редактирование конфигурационного файла**:
Откройте файл конфигурации MariaDB/MySQL (`/etc/mysql/mariadb.conf.d/50-server.cnf` для MariaDB или `/etc/mysql/my.cnf` для MySQL) и добавьте следующие строки в секцию `[mysqld]`:
    
    ```
    [mysqld]
    slow_query_log = 1
    slow_query_log_file = /var/log/mysql/mariadb-slow.log
    long_query_time = 5
    ```
    
    - `slow_query_log`: Включает логирование медленных запросов.
    - `slow_query_log_file`: Указывает путь к файлу логов.
    - `long_query_time`: Устанавливает порог времени (в секундах) для определения медленных запросов.
3. **Перезапуск сервера MariaDB/MySQL**:
    
    ```bash
    sudo systemctl restart mariadb  # Для MariaDB
    sudo systemctl restart mysql    # Для MySQL
    ```
    
4. **Проверка настройки**:
Подключитесь к MariaDB/MySQL и выполните команду:
    
    ```sql
    SHOW VARIABLES LIKE '%slow_query%';
    ```
    
    Убедитесь, что `slow_query_log` включен и путь к файлу логов указан правильно.
    

## Использование `mysqldumpslow`

`mysqldumpslow` — это утилита для анализа и суммирования медленных запросов, записанных в лог-файл медленных запросов MariaDB/MySQL.

### Основные команды:

1. **Запуск `mysqldumpslow`**:
    
    ```bash
    mysqldumpslow /var/log/mysql/mariadb-slow.log
    ```
    
2. **Опции `mysqldumpslow`**:
    - `s t`: Сортировать по времени выполнения.
    - `t 10`: Показать только первые 10 запросов.

### Пример использования:

```bash
mysqldumpslow -s t -t 10 /var/log/mysql/mariadb-slow.log
```

Этот пример покажет 10 самых медленных запросов, отсортированных по времени выполнения.

Если у вас возникнут вопросы или потребуется дополнительная помощь, не стесняйтесь спрашивать!

: Configuring MariaDB with Option Files - MariaDB Knowledge Base
: How to Enable the Slow Query Log in MySQL® or MariaDB
: mysqldumpslow - MariaDB Knowledge Base
: mariadb-dumpslow - MariaDB Knowledge Base

# MySQLTuner

1. **Install Perl** (if it's not already installed):
    
    ```bash
    **sudo apt install perl**
    ```
    
2. **Download and Run MySQLTuner**:
    - Download the script: **`wget http://mysqltuner.pl/ -O mysqltuner.pl`**.
    - Make the script executable: **`chmod +x mysqltuner.pl`**.
    - Run the script: **`./mysqltuner.pl`**.
3. **Follow the Script's Recommendations**:
    - After running, MySQLTuner will provide you with a set of recommendations regarding your MySQL configuration, such as variables to adjust for optimal performance.

Keep in mind that while MySQLTuner provides useful insights, its recommendations should be carefully evaluated in the context of your specific workload and tested in a non-production environment before applying to a production server.

На Ubuntu 22.04 конфигурационные файлы MariaDB обычно находятся в следующих местах:

1. **Основной файл конфигурации**:
    - **`/etc/mysql/mariadb.conf.d/50-server.cnf`**
2. **Дополнительные файлы конфигурации**:
    - **`/etc/mysql/my.cnf`** (этот файл может включать другие конфигурационные файлы)
    - **`/etc/mysql/mariadb.conf.d/`** (директория, содержащая дополнительные конфигурационные файлы)

Чтобы найти точное расположение всех конфигурационных файлов, вы можете использовать команду **`find`**:

`sudo find / -name "my.cnf" -type f 2>/dev/null`

Или команду **`mysql --help`** для получения информации о конфигурационных файлах:

`mysql --help | grep "Default options"`

# Ошибка 502

изменил  в /etc/php/7.4/fpm/pool.d/www.conf

listen = 127.0.0.1:9070

The “Connection refused” errors indicate that the PHP-FPM service is not listening on port 9070. Here are some additional steps to troubleshoot and resolve this issue:

1. **Verify PHP-FPM Configuration**:
Double-check the PHP-FPM pool configuration to ensure it’s set to listen on port 9070. Open the configuration file:
    
    Ensure the `listen` directive is correctly set:
    
    ```
    listen = 127.0.0.1:9070
    ```
    
2. **Restart PHP-FPM**:
After making any changes, restart the PHP-FPM service:
    
    ```bash
    sudo systemctl restart php7.4-fpm
    ```
    
3. **Check PHP-FPM Status**:
Verify that PHP-FPM is running and listening on the correct port:
    
    ```bash
    sudo systemctl status php7.4-fpm
    sudo netstat -tulnp | grep 9070
    ```
    
4. **Check PHP-FPM Logs**:
Review the PHP-FPM logs for any errors that might indicate why the service is not responding:
    
    ```bash
    sudo tail -f /var/log/php7.4-fpm.log
    ```
    
5. **Ensure No Conflicting Configurations**:
Make sure there are no conflicting configurations in other pool files. Sometimes, multiple pool files can cause issues. List the files in the pool directory:
    
    ```bash
    ls /etc/php/7.4/fpm/pool.d/
    ```
    
    Ensure only the necessary pool files are present and configured correctly.
    
6. **Reconfigure to Use UNIX Socket**:
If the above steps do not resolve the issue, consider reconfiguring PHP-FPM to use a UNIX socket instead of a TCP port. Update the `listen` directive in the pool configuration:

```
listen = /run/php/php7.4-fpm.sock
```

Update your Nginx configuration to use the socket:

```
upstream php7 {
    server unix:/run/php/php7.4-fpm.sock;
}
```

After making these changes, restart both PHP-FPM and Nginx:

```bash
sudo systemctl restart php7.4-fpm
sudo systemctl restart nginx
```

These steps should help you identify and resolve the issue with PHP-FPM not listening on the specified port. Let me know if you need further assistance!

# Проблема с DNS

Masking a service in systemd means that the service is linked to **`/dev/null`**, effectively preventing the service from being started manually or automatically.

```bash
sudo systemctl unmask systemd-resolved.service
```

```bash
sudo systemctl enable systemd-resolved.service
sudo systemctl start systemd-resolved.service
```

```bash
sudo nano /etc/resolv.conf
nameserver 8.8.8.8
nameserver 8.8.4.4
```

```bash
sudo systemctl restart systemresolved
```

```bash
sudo systemctl restart systemd-resolved
```

# Install and Use ClamAV on Ubuntu 22.04

ClamAV is a free, open-source antivirus program that can detect viruses, trojans, and malware4. This guide will help you install and use ClamAV on Ubuntu 22.042[3](https://edgeservices.bing.com/edgesvc/chat?udsframed=1&form=SHORUN&clientscopes=chat,noheader,udsedgeshop,channelstable,ntpquery,devtoolsapi,udsinwin11,udsdlpconsent,udsarefresh,cspgrd,&shellsig=2037f0af629f1d9576802324e7971bb22d6cc214&setlang=en-US&darkschemeovr=1&udsps=0&udspp=0#sjevt%7CDiscover.Chat.SydneyClickPageCitation%7Cadpclick%7C2%7C355f5267-d795-4150-b2db-feb485e8f041).

## Install ClamAV

```bash
sudo apt update
sudo apt install clamav clamav-daemon
clamscan --version
```

## Update ClamAV Signature Database

```bash
sudo systemctl stop clamav-freshclam 
sudo freshclam 
sudo systemctl start clamav-freshclam
```

## Start ClamAV Daemon

```bash
sudo systemctl start clamav-daemon
```

## Scan Files with ClamAV

1. **Scan all files in the current directory:**
    
    ```bash
    clamscan -r .
    ```
    
2. **Scan and only show infected files:**
    
    ```bash
    clamscan -r --infected .
    ```
    
3. **Scan and remove infected files:**
    
    ```bash
    clamscan -r --remove .
    ```
    

## Check ClamAV Logs

Logs are stored in `/var/log/clamav/clamav.log`

# **Rclone**

To install rclone on Linux/macOS/BSD systems, execute the following command:

```bash
sudo -v ; curl https://rclone.org/install.sh | sudo bash
or
wget https://rclone.org/install.sh && sudo bash install.sh
```

For installations from the beta channel, use:

```bash
sudo -v ; curl https://rclone.org/install.sh | sudo bash -s beta
```

# NCDU (NCurses Disk Usage) - использование диска в интерактивном режиме

```bash
sudo apt update
sudo apt install ncdu
```

Запустите **`ncdu`** в корневой директории:

```bash
sudo ncdu /
```

Unique list of websites

```bash
nginx -T | grep -v '#' | grep 'server_name' | awk '{print $2}' | sed 's/;//' | grep -v '^\*' | sort | uniq
```

# **Systemd-timesyncd on Ubuntu 22.04**

**Update NTP Servers**: You can specify different NTP servers in the **`/etc/systemd/timesyncd.conf`** file. Open the file with a text editor:

```bash
sudo nano /etc/systemd/timesyncd.conf
```

Uncomment the **`NTP=`** line and add your preferred NTP servers. For example:

`[Time]
NTP=0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org`

Restart the timesyncd with the following command:

```bash
sudo systemctl restart systemd-timesyncd
```

Verify that the service is active and running on your server:

```
sudo systemctl status ntp
```

should be:

```
System clock synchronized: yes
NTP service: active
```
