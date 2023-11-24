
# LAMP

## Lưu ý
-Đặt IP tĩnh trước khi cài đặt

-Cài đặt ở quyền root (sudo). Nếu không ở quyền root thì thêm sudo trước mỗi câu lệnh

## Bắt đầu cài đặt

### B1: Cập nhật, cài đặt các repo cần thiết cho hệ thống

**CentOS/RHEL**

```
yum -y update
```

```
yum -y install epel-release
yum install yum-utils
```

hoặc

```
dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

**Debian/Ubuntu**

```bash
apt-get -y update
```

### B2: Cài đặt Apache Web Server
**1.**

**CentOS/RHEL**

```bash
yum -y install httpd
```

**Debian/Ubuntu**

```bash
apt-get -y install apache2
```

**2. Khởi động, kiểm tra trạng thái Apache**

   **Đối với Ubuntu/Debian là *apache2***

#### CentOS/RHEL

```bash
systemctl enable httpd
systemctl start httpd
systemctl status httpd
```

![Alt](https://www.tecmint.com/wp-content/uploads/2015/10/check-if-apache2-service-is-running.png)

**3. Mở Firewall cho phép http và https (đối với port là 80 và 443)**

```bash
firewall-cmd --zone=public --permanent --add-service=http
firewall-cmd --zone=public --permanent --add-service=https
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --permanent --zone=public --add-port=443/tcp
firewall-cmd –-reload
```
**4. Nhập IP máy chủ hoặc tên miền kiểm tra Apache đã hoạt động hay chưa**
```
http://IP_ADDR
```
![Alt](https://img001.prntscr.com/file/img001/slskComuRhm4kYmHONN53A.png)

### B3: Cài đặt Mariadb hoặc Mysql (tùy lựa chọn)
**1. Cài đặt, khởi động và kiểm tra trạng thái**

**CentOS/RHEL**
```
yum install mariadb-server mariadb
```
**Ubuntu/Debian**
```
apt install mariadb-server mariadb-client
```
**Sau khi cài đặt hoàn tất, các bạn có thể sử dụng các lệnh sau để quản lý MariaDB**
```
systemctl start mariadb      (Khởi động dịch vụ mariadb)
systemctl stop mariadb      (Dừng dịch vụ mariadb)
systemctl restart mariadb    (Khởi động lại  dịch vụ mariadb)
systemctl enable mariadb     (Thiết lập mariadb khởi động cùng hệ thống)
systemctl disable mariadb    (Vô hiệu hoá mariadb khởi động cùng hệ thống )
systemctl status mariadb     (Xem trạng thái dịch vụ mariadb)
```
**2. Thiết lập bảo mật MariaDB Server**

```
mysql_secure_installation
```

Sau khi chạy câu lệnh, nó sẽ hỏi bạn các câu hỏi mà bạn chỉ trả lời `yes(y)` hoặc `no(n)` để mở vài tính năng bảo mật. Vì ta vừa cài đặt database nên sẽ không có mật khẩu root

![Alt](https://www.tecmint.com/wp-content/uploads/2015/10/secure-mariadb-deployment.png)

### B4: Cài đặt PHP

**1. Cài đặt PHP**

**CentOS/RHEL**

```
yum install -y https://rpms.remirepo.net/enterprise/remi-release-9.rpm  [On CentOS/RHEL 8]
yum install -y https://rpms.remirepo.net/enterprise/remi-release-8.rpm  [On CentOS/RHEL 8]
yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm  [On CentOS/RHEL 7]
```

Sau khi cài đặt gói Remi xong, bạn cần chọn phiên bản PHP mà mình cần cài đặt và kích hoạt gói chứa phiên bản PHP đó.(Ví dụ ở dưới là php 8.1)

```
yum module list install php:8.1
```

Nếu sử dụng php từ remi

```
yum module list install php:remi-8.1
```

Khi enable module php ,bạn tiến hành cài đặt PHP và các Extension cần thiết như bên dưới.

```
yum install -y php php-ldap php-zip php-embedded php-cli php-mysql php-common php-gd php-xml php-mbstring php-mcrypt php-pdo php-soap php-json php-simplexml php-process php-curl php-bcmath php-snmp php-gmp php-intl php-imap perl-LWP-Protocol-https php-pear-Net-SMTP php-enchant php-pear php-devel php-zlib php-xmlrpc php-tidy php-opcache php-cli php-pecl-zip
```

hoặc

```
dnf install -y php php-ldap php-zip php-embedded php-cli php-mysql php-common php-gd php-xml php-mbstring php-mcrypt php-pdo php-soap php-json php-simplexml php-process php-curl php-bcmath php-snmp php-gmp php-intl php-imap perl-LWP-Protocol-https php-pear-Net-SMTP php-enchant php-pear php-devel php-zlib php-xmlrpc php-tidy php-opcache php-cli php-pecl-zip 
```

**Ubuntu**

```
apt install php libapache2-mod-php php-mysql
```

**2. Khởi động và kiểm tra phiên bản PHP**

**Đối với Ubuntu/RHEL là *apache2***

```
systemctl restart httpd
```

Kiểm tra phiên bản PHP
```
php -v
```

Màn hình sẽ xuất ra kết quả như dưới (ví dụ như phiên bản của mình là 8.1)

```
PHP 8.1.14 (cli) (built: Jan  4 2023 17:23:14) (NTS gcc x86_64)
Copyright (c) The PHP Group
Zend Engine v4.1.14, Copyright (c) Zend Technologies
    with Zend OPcache v8.1.14, Copyright (c), by Zend Technologies
```

**3.Kiểm tra PHP hoạt động trên Apache**

Để kiểm tra PHP đã hoạt động trên Apache chưa, ta tạo file `info.php` trong thư mục root của web `/var/www/html/`(tùy chọn trình soạn thảo văn bản)

```
vi /var/www/html/info.php
```

Copy đoạn code dưới đây vào file rồi lưu lại

```
<?php
        phpinfo();
?>
```
Sau đó ta mở trình duyệt vào kiểm tra xem PHP đã hoạt động chưa

```
http://YOUR_SERVER_IP/info.php
```

Kết quả

![Alt](https://img001.prntscr.com/file/img001/7k8jKMwJQDqCKf90BDBJ-w.png)
