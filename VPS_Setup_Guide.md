# Setup Guide for VPS and Backend Deployment

## Steps to Initialize the VPS and Deploy the Backend and the frontend

### 1. Initialize the VPS
- **Access the VPS**: Connect to the VPS via SSH:
    ```bash
    ssh ubuntu@<IP_VPS>
    ```

### 2. Install NVM, Node.js, TypeScript, and PM2
- **Install NVM (Node Version Manager)**:
    ```bash
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
    source ~/.bashrc
    ```
- **Install Node.js**:
    ```bash
    nvm install node
    nvm use node
    ```
- **Install TypeScript**:
    ```bash
    npm install -g typescript
    ```
- **Install PM2 (Process Manager)**:
    ```bash
    npm install -g pm2
    ```

- **Install Nginx**:
    ```bash
    sudo apt install nginx
    ```

### 3. Grant Sudo Permissions to /home/ubuntu
- **Add the 'ubuntu' user to the sudo group**:
    ```bash
    sudo usermod -aG sudo ubuntu
    ```

### 4. Clone the Repository
- **Install Git (if not already installed)**:
    ```bash
    sudo apt update
    sudo apt install git
    ```
- **Clone the backend from the repository**:
    ```bash
    git clone <URL_DEL_REPOSITORY>
    cd <NOME_DEL_REPOSITORY>
    npm i
    npm run build ("build": "tsc")
    ```
- **Clone the frontend from the repository**:
    ```bash
    git clone <URL_DEL_REPOSITORY>
    cd <NOME_DEL_REPOSITORY>
    npm i
    npm run build ( "build": "ng build --configuration=production --deploy-url=/todoapp/")
    ```

### 5. Modify Necessary Configurations (.env)
- **Configure environment variables**:
    - Create or modify the `.env` file to include necessary configurations such as database URL, API keys, etc.

### 6. Enable Necessary Ports
- **Enable ports through the system firewall (UFW)**:
    ```bash
    sudo ufw allow 22/tcp  # for SSH
    sudo ufw allow 3000/tcp  # for the backend
    sudo ufw enable  # enable UFW if not already active
    sudo ufw status verbose  # check active rules
    ```
- **Update firewall settings from the VPS dashboard**:
    - Log into the VPS provider's dashboard and ensure that ports 22 and 3000 are open in the network firewall.

### 7. Start the Backend with PM2
- **Navigate to the backend folder and start the app**:
    ```bash
    pm2 start <NOME_DEL_FILE_DELL_APP> --name todo-app-backend
    pm2 save  # save the configuration for restart
    ```
- **Check the app logs**:
    ```bash
    pm2 logs todo-app-backend
    ```

### 8. Start the Frontens with Nginx
- **Create a folder for the site (todoapp folder for the route todoapp, index.html initial page generic, 404.html error page generic)**:
    ```bash
    ls -r /var/www/alexviolatto.com/
    todoapp  index.html  404.html
    ```
- **Create a configuration file for nginx**
    ```bash
    sudo nano /etc/nginx/sites-available/todoapp
    ```
- **Example of structure**:
    ```bash
    server {
    listen 80;
    server_name alexviolatto.com www.alexviolatto.com;

    # Redirect all HTTP requests to HTTPS
    return 301 https://$host$request_uri;
    }
    
    server {
        listen 443 ssl;
        server_name alexviolatto.com www.alexviolatto.com;
    
        ssl_certificate /etc/letsencrypt/live/alexviolatto.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/alexviolatto.com/privkey.pem;
        include /etc/letsencrypt/options-ssl-nginx.conf;
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
    
        root /var/www/alexviolatto.com;
        index index.html;
    
        location = / {
            try_files $uri $uri/ /index.html;
        }
    
        location /todoapp {
            alias /var/www/alexviolatto.com/todoapp;
            index index.html;
            try_files $uri $uri/ /todoapp/index.html;
        }
    
        location /todoapp/ {
            try_files $uri $uri/ /todoapp/index.html;
        }
    
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot|json)$ {
            try_files $uri =404;
        }
    
        error_page 404 /404.html;
        location = /404.html {
            root /var/www/alexviolatto.com;
            internal;
        }
    
        location / {
            try_files $uri $uri/ =404;
        }
    }
    ```

### 9. Check nginx conf and restart
- **Create a folder for the site (todoapp folder for the route todoapp, index.html initial page generic, 404.html error page generic)**:
    ```bash
    sudo ln -s /etc/nginx/sites-available/nomesito /etc/nginx/sites-enabled/
    sudo nginx -t
    sudo systemctl restart nginx
    sudo systemctl status nginx
    ```


## Common Issues and Solutions

1. **External Connection Problem (curl: (28) Failed to connect...)**
   - **Solution**: Update firewall settings from the VPS dashboard to open port 3000.

2. **Unauthorized Access Error (Unauthorized)**
   - **Solution**: Check the authentication token or API route configuration. Ensure environment variables are set correctly.

3. **PM2 Does Not Show Logs or Server Does Not Start**
   - **Solution**: Ensure the backend is listening on the correct port and that there are no code errors. Check PM2 logs for details.

4. **Local Firewall (UFW) Configured But Not Working**
   - **Solution**: Check if there are other firewall rules at the provider level blocking traffic, such as network firewalls.

## Conclusion
By following these steps and keeping in mind the common issues and their solutions, you will be able to properly configure your VPS and deploy your backend online. If you encounter further problems, feel free to ask for help!
