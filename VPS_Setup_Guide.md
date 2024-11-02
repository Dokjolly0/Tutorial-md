
# Setup Guide for VPS and Backend Deployment

## Steps to Initialize the VPS and Deploy the Backend

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

### 3. Grant Sudo Permissions to /home/ubuntu
- **Add the 'ubuntu' user to the sudo group**:
    ```bash
    sudo usermod -aG sudo ubuntu
    ```

### 4. Clone the Backend Repository
- **Install Git (if not already installed)**:
    ```bash
    sudo apt update
    sudo apt install git
    ```
- **Clone the backend from the repository**:
    ```bash
    git clone <URL_DEL_REPOSITORY>
    cd <NOME_DEL_REPOSITORY>
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

### 8. Monitor the Backend
- **Check the app logs**:
    ```bash
    pm2 logs todo-app-backend
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
