
# Hướng dẫn cài đặt LAMP - LEMP cho RHEL, CentOS - Ubuntu

## Lưu ý
-Đặt IP tĩnh trước khi cài đặt

-Cài đặt ở quyền root (sudo). Nếu không ở quyền root thì thêm sudo trước mỗi câu lệnh

## Bắt đầu cài đặt

### B1: Cập nhật hệ thống

**CentOS/RHEL**

```
yum -y update
```

**Debian/Ubuntu**

```bash
apt-get -y update
```

### B2: Cài đặt Apache Web Server
1.

**CentOS/RHEL**

```bash
yum -y install httpd
```

**Debian/Ubuntu**

```bash
apt-get -y install apache2
```

2. Khởi động, kiểm tra trạng thái Apache

   **Đối với Ubuntu/Debian là *apache2***

#### CentOS/RHEL

```bash
systemctl enable httpd
systemctl start httpd
systemctl status httpd
```

![Alt](https://www.tecmint.com/wp-content/uploads/2015/10/check-if-apache2-service-is-running.png)

3. Mở Firewall cho phép http và https (đối với port là 80 và 443)

```bash
firewall-cmd --zone=public --permanent --add-service=http
firewall-cmd --zone=public --permanent --add-service=https
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --permanent --zone=public --add-port=443/tcp
firewall-cmd –reload
```
4. Nhập IP máy chủ hoặc tên miền kiểm tra Apache đã hoạt động hay chưa
```
http://IP_ADDR
```
![Alt](https://img001.prntscr.com/file/img001/hS7KgQmpRYuC8-ynAyzf-Q.png)

### B3: Cài đặt Mariadb hoặc Mysql (tùy lựa chọn)
1. Cài đặt, khởi động và kiểm tra trạng thái

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
2. Thiết lập bảo mật MariaDB Server

```
mysql_secure_installation
```

Sau khi chạy câu lệnh, nó sẽ hỏi bạn các câu hỏi mà bạn chỉ trả lời `yes(y)` hoặc `no(no)` để mở vài tính năng bảo mật. Vì ta vừa cài đặt database nên sẽ không có mật khẩu root (hoặc admin)

![Alt](https://www.tecmint.com/wp-content/uploads/2015/10/secure-mariadb-deployment.png)

