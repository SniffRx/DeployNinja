# Дополнительная настройка (общая для обоих дистрибутивов)
## Создание виртуального хоста
###  1. Создайте директорию для сайта:
```bash
sudo mkdir -p /var/www/example.com/html
```
### 2. Назначьте права:
```bash
sudo chown -R $USER:$USER /var/www/example.com/html
sudo chmod -R 755 /var/www/example.com
```
### 3. Создайте тестовую страницу:
```bash
nano /var/www/example.com/html/index.html
```
Добавьте простой HTML:
```html
<html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success! The example.com server block is working!</h1>
    </body>
</html>
```
### 4. Создайте конфигурационный файл:
* Для Debian:
```bash
sudo nano /etc/nginx/sites-available/example.com
```
* Для Arch:
```bash
sudo nano /etc/nginx/conf.d/example.com.conf
```
Добавьте конфигурацию:
```nginx
server {
listen 80;
server_name example.com www.example.com;

    root /var/www/example.com/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
### 5. Активируйте сайт:
* Для Debian:
```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```
* Для Arch: файл уже активен, так как лежит в conf.d
### 6. Проверьте конфигурацию и перезагрузите Nginx:
```bash
sudo nginx -t
sudo systemctl reload nginx
```
### Настройка HTTPS с Let's Encrypt (Certbot)
1. Установите Certbot:
* Для Debian:
```bash
sudo apt install certbot python3-certbot-nginx -y
```
* Для Arch:
```bash
sudo pacman -S certbot certbot-nginx
```
2. Получите сертификат:z
```bash
sudo certbot --nginx -d example.com -d www.example.com
```
3. Настройте автоматическое обновление:
```bash
sudo certbot renew --dry-run
```