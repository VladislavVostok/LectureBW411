https://docs.vultr.com/how-to-install-mysql-on-debian-12
## Install MySQL

```sh
sudo apt update
```
```sh
sudo apt install wget -y
```

```sh
wget https://dev.mysql.com/get/mysql-apt-config_0.8.30-1_all.deb
```
```sh
sudo dpkg -i mysql-apt-config_0.8.30-1_all.deb
```

```sh
sudo apt update
```

```sh
sudo apt install mysql-server -y
```
```sh
sudo dpkg-reconfigure mysql-apt-config
```
```sh
sudo apt update
```
## Manage the MySQL System Service

```sh
sudo systemctl enable mysql
```
```sh
sudo systemctl start mysql
```

## Secure the MySQL Database Server

```sh
sudo mysql_secure_installation
```

## Access MySQL

```sh
sudo mysql -u root -p
```
```sql
CREATE DATABASE ekg;
```
```sql
SHOW DATABASES;
```
Create a new sample database user `db_user` with a strong password. Replace `Strong@@password123` with your desired password depending on the database server policy.
```sh
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'Shalom$_molahs***777';
```

Grant the `db_user` full privileges to the `shop` database.

```sh
GRANT ALL PRIVILEGES ON ekg.* TO 'admin'@'localhost';
[]```

```sql
FLUSH PRIVILEGES;
```

```sh
EXIT
```
## Error "Access denied for user 'root'@'localhost' (using password: YES)"

```sh
sudo nano /etc/mysql/my.cnf
```

```init
[mysqld] 
skip-grant-tables 
skip-networking
```

```sh
sudo systemctl restart mysql
```
```sh
mysql
```
```sql
FLUSH PRIVILEGES;
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '<password>';
```


### Прочие команды mysql cli
Для того чтобы посмотреть структуру таблицы в базе данных _MySQL_, можно воспользоваться командой `DESCRIBE` или `SHOW COLUMNS`.

```sql
DESCRIBE table_name;
```
ИЛИ

```sql
SHOW COLUMNS FROM table_name;
```

# .NET CORE INSTALL DEBIAN


### Подготовка
```sh
wget https://packages.microsoft.com/config/debian/12/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
```
```sh
sudo dpkg -i packages-microsoft-prod.deb
```
```sh
rm packages-microsoft-prod.deb
```
### Установка пакета SDK

```sh
sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-8.0
```

### Установка среды выполнения

```sh
sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-8.0
```

### Обновление .NET с помощью APT

```sh
sudo apt-get update && sudo apt-get upgrade
```

Connection string
```mysql
Server=myServerAddress;Database=myDataBase;Uid=myUsername;Pwd=myPassword;
```


## Настройка библиотек для EKGapp

dotnet add package Pomelo.EntityFrameworkCore.MySql --version 8.0.2
dotnet add package Microsoft.EntityFrameworkCore --version 8.0.10
dotnet add package MySql.EntityFrameworkCore --version 8.0.8

## Миграция 

dotnet ef migrations add InitialCreate 
dotnet ef database update



dotnet publish ZAO_Obninskenergotech.sln -c Release -r linux-x64 --self-contained


### 1. Установка Nginx
```bash
sudo apt update
sudo apt install nginx -y
```

### 2. Запуск Nginx и добавление в автозагрузку
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 3. Проверка статуса Nginx
```bash
sudo systemctl status nginx
```

### 4. Размещение HTML-страницы
По умолчанию корневая директория веб-сервера Nginx в Debian 12 находится в `/var/www/html`. Поместите вашу HTML-страницу (например, `index.html`) в эту директорию.

Пример создания простой HTML-страницы:
```bash
sudo nano /var/www/html/index.html
```

Вставьте следующий код:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Мой сайт на Nginx</title>
</head>
<body>
    <h1>Привет, мир!</h1>
    <p>Это моя первая страница на Nginx в Debian 12.</p>
</body>
</html>
```

Сохраните файл (`Ctrl+O`, затем `Enter`) и закройте редактор (`Ctrl+X`).

### 5. Настройка прав доступа
Убедитесь, что Nginx имеет доступ к файлам:
```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

### 6. Проверка конфигурации Nginx
```bash
sudo nginx -t
```

Если всё в порядке, вы увидите:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

### 7. Перезагрузка Nginx
```bash
sudo systemctl reload nginx
```

### 8. Проверка в браузере
Откройте браузер и перейдите по адресу:
```
http://localhost
```
или
```
http://ваш_IP_адрес
```

### Дополнительно: Настройка виртуального хоста (если нужно)
Если требуется обслуживать несколько сайтов:

1. Создайте директорию для сайта:
```bash
sudo mkdir -p /var/www/mysite/html
```

2. Создайте конфигурационный файл:
```bash
sudo nano /etc/nginx/sites-available/mysite
```

3. Добавьте конфигурацию:
```nginx
server {
    listen 80;
    server_name mysite.com www.mysite.com;

    root /var/www/mysite/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

4. Активируйте конфигурацию:
```bash
sudo ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/
```

5. Проверьте и перезагрузите Nginx:
```bash
sudo nginx -t
sudo systemctl reload nginx
```


### **1. Установка Certbot и плагина для Nginx**
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```

### **2. Получение SSL-сертификата**
Запустите Certbot для автоматической настройки SSL:
```bash
sudo certbot --nginx -d ваш_домен.com -d www.ваш_домен.com
```
(замените `ваш_домен.com` на ваш реальный домен)

📌 **Примечание:**  
- Certbot автоматически изменит конфигурацию Nginx, добавив HTTPS.  
- Если у вас нет домена, можно использовать `--dry-run` для теста или локальный IP (но Let's Encrypt требует настоящий домен).  

### **3. Проверка автоматического обновления сертификата**
Certbot автоматически добавляет задание в **cron** для продления сертификата. Проверить можно командой:
```bash
sudo certbot renew --dry-run
```

### **4. Настройка Nginx (если Certbot не сделал это автоматически)**
Если Certbot не настроил Nginx автоматически, можно вручную добавить SSL в конфиг (`/etc/nginx/sites-available/ваш_сайт`):
```nginx
server {
    listen 80;
    server_name ваш_домен.com www.ваш_домен.com;
    return 301 https://$host$request_uri;  # Редирект HTTP → HTTPS
}

server {
    listen 443 ssl;
    server_name ваш_домен.com www.ваш_домен.com;

    ssl_certificate /etc/letsencrypt/live/ваш_домен.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/ваш_домен.com/privkey.pem;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### **5. Проверка и перезагрузка Nginx**
```bash
sudo nginx -t  # Проверить конфигурацию
sudo systemctl reload nginx  # Применить изменения
```

### **6. Проверка SSL**
Откройте в браузере:  
🔗 **`https://ваш_домен.com`**  
Должен появиться **замок** (HTTPS) в адресной строке.

### **7. Дополнительные команды Certbot**
| Команда | Описание |
|---------|----------|
| `sudo certbot certificates` | Показать текущие сертификаты |
| `sudo certbot renew --force-renewal` | Принудительно обновить сертификат |
| `sudo certbot delete --cert-name ваш_домен.com` | Удалить сертификат |
