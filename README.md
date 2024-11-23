# Django Deployment with NGINX and Gunicorn

This guide walks you through the steps of deploying a Django project using **NGINX** and **Gunicorn**. The setup assumes that you have a Django project ready and will cover the configuration of Gunicorn and NGINX to serve the project.

## Prerequisites

Before proceeding, make sure you have the following installed:

- Django
- Gunicorn
- NGINX

## Gunicorn Configuration

To set up **Gunicorn**, create a configuration file `gunicorn_config.py` with the following contents:

### 1. Create the Gunicorn Config File

In the configuration file, define the following settings:

```python
command = 'mehfooz@mehfooz-HP-ProBook-450:~/Desktop/Mehfooz/Projects Code/Djang'
pythonpath = 'mehfooz@mehfooz-HP-ProBook-450:~/Desktop/Mehfooz/Projects Code/Django'
bind = '127.0.0.1:8000'
workers = 3
```
### 2. Save and Exit the Configuration File
After adding the configurations:
- Press Ctrl+O to save the file.
- Press Enter to confirm.
- Press Ctrl+X to exit.
  
### 3. Running Gunicorn
To run your Django project with Gunicorn, use the following command:
```bash
gunicorn -c conf/gunicorn_config.py DeployNGW.wsgi
```
This command will start Gunicorn, binding it to 127.0.0.1:8000 with 3 worker processes.


## NGINX Configuration
To configure NGINX to proxy requests to Gunicorn and serve static files, follow these steps.

### 1. Open NGINX Configuration File
Run the following command to open the NGINX configuration file:
```bash
sudo nano /etc/nginx/sites-available/DeployNGW
```
This will open a new file where you can add your server block configuration.

### 2. Add the NGINX Server Configuration
Inside the file, add the following configuration:
```bash
server {
    listen 80;
    server_name localhost;

    # Serve static files
    location /static/ {
        alias /home/mehfooz/Desktop/Mehfooz/Projects\ Code/Django\ with\ deployment/Django\ NGINX,\ GUNICORN/static/;
    }

    # Proxy all other requests to Gunicorn
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 3. Save and Exit the File
After adding the configuration:
- Press Ctrl+O to save the file.
- Press Enter to confirm.
- Press Ctrl+X to exit.

### 4. Enable the NGINX Site
To create a symbolic link for the configuration file in sites-enabled, run:
```bash
cd /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/DeployNGW
```

### 5. Test NGINX Configuration
- sudo nginx -t
- sudo systemctl reload nginx
- sudo systemctl restart nginx

## Final Steps
After completing the above steps, your Django project should be successfully deployed using NGINX and Gunicorn.

### Troubleshooting
- Ensure that Gunicorn is running and listening on 127.0.0.1:8000.
- Make sure NGINX has access to the static files and the proxy is set up correctly.
- If you encounter any issues, check the NGINX logs located in /var/log/nginx/.

### Additional Commands
- sudo service gunicorn restart
- sudo service nginx restart
