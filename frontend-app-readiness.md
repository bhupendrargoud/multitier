## How to Install Apache2, Python, and the Redis Python library on Ubuntu:

1. Update your Ubuntu system's package manager by running the following command:

```
sudo apt-get update
```



2. Install and start Nginx server by running the following command:

```
sudo apt install nginx
sudo systemctl start nginx
```

3. Create a nignx proxy file with name backend.conf in /etc/nginx/sites-available/

```
sudo vi /etc/nginx/sites-available/backend.conf
```
4. Copy paste the belove code after editing
```
server {
    listen 90; # Nginx proxy listening port
    server_name 3.3.3.3; # Public Ip of the frontend App

    location / {
        proxy_pass http://your_private_instance_ip:8080;
    }
}
```

5. This nginx will forward your requests to your backend server Replace your_private_instance_ip with the ip of your private instance.

Create a symbolic link to enable the site:

```
sudo ln -s /etc/nginx/sites-available/backend.conf /etc/nginx/sites-enabled/
```

6. Test the configuration with:

```
sudo nginx -t
```
7. Change the default port Number of nginx from 80-->90
```
sudo vi /etc/nginx/sites-available/default.conf
```
8. Reload the nginx configuration:
```
sudo systemctl reload nginx
```

9. After completing these steps, you should have Apache2 installed on your Ubuntu system.
Install and start Apache2 web server by running the following command:

```
sudo apt-get install apache2
sudo systemctl start apache2
```
10. Change the HTML page
```
Sudo vi /var/www/html/index.html
```