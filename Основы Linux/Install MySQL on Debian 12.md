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