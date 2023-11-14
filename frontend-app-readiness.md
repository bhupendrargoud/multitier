## How to Install Apache2, Python, and the Redis Python library on Ubuntu:

1. Update your Ubuntu system's package manager by running the following command:

```
sudo apt-get update
```

2. Install and start Apache2 web server by running the following command:

```
sudo apt-get install apache2
sudo systemctl start apache2
```

3. Install and start Nginx server by running the following command:

```
sudo apt install nginx
sudo systemctl start nginx
```

4. Set up the frontend HTML in your public instance

Frontend script:
```
File Path: /var/www/html/index.html
```

5. Create a nignx proxy file with name backend.conf in /etc/nginx/sites-available/

```
sudo vi /etc/nginx/sites-available/backend.conf

server {
    listen 90; # Nginx proxy listening port
    server_name 3.3.3.3; # Public Ip of the frontend App

    location / {
        proxy_pass http://your_private_instance_ip:8080;
    }
}
```

This nginx will forward your requests to your backend server Replace your_private_instance_ip with the ip of your private instance.

Create a symbolic link to enable the site:

```
sudo ln -s /etc/nginx/sites-available/backend.conf /etc/nginx/sites-enabled/
```

Test the configuration with:

```
sudo nginx -t
```

Reload the nginx configuration:
```
sudo systemctl reload nginx
```

After completing these steps, you should have Apache2 installed on your Ubuntu system.
