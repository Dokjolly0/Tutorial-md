# VPS Backup and Restore Guide

## Introduction

This guide provides commands and steps to back up and restore essential directories on your VPS. It also covers restoring user permissions and sudo configurations.

## Backup Commands

### Backup Important Directories

Use the following commands to back up the `/home`, `/etc`, and `/var` directories from your VPS to your local machine.

1. **Backup the `/home` Directory**:

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/home C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup
   ```

2. **Backup the `/etc` Directory**:

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/etc C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup
   ```

3. **Backup the `/var` Directory**:

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/var C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup
   ```

4. **Backup the `usr` Directory**

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/usr C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup
   ```

5. **Backup the `opt` Directory**

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/opt C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup
   ```

6. **Backup the `srv` Directory**

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/srv C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup

   ```

7. **Backup the `root` Directory**

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r ubuntu@91.214.190.5:/root C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup

   ```

## Restore Commands

After resetting your VPS, use the following commands to restore your files and directories:

1. **Restore the `/home` Directory**:

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup\home ubuntu@91.214.190.5:/home
   ```

2. **Restore the `/etc` Directory**:

   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup\etc ubuntu@91.214.190.5:/etc
   ```

3. **Restore the `/var` Directory**:
   ```bash
   scp -i C:\Users\violatto_a\Desktop\Alex\ssh_key\id_rsa -r C:\Users\violatto_a\Desktop\Alex\vps\vps_1_backup\var ubuntu@91.214.190.5:/var
   ```

## Restore User Permissions

1. **Restore Ownership in `/home`**:

   ```bash
   sudo chown -R user:group /home/user
   ```

   Replace `user` with the username and `group` with the appropriate group.

2. **Restore Sudo Permissions**:
   To restore sudo permissions, copy the backup of your `/etc/sudoers` file back to the original location:
   ```bash
   sudo cp /path/to/backup/sudoers /etc/sudoers
   ```
   Make sure to set the correct permissions:
   ```bash
   sudo chmod 440 /etc/sudoers
   ```

## Restore Databases

If you have backed up databases, use the following commands to restore them:

- **For MySQL**:

  ```bash
  mysql -u username -p database_name < /path/to/backup/dump.sql
  ```

- **For PostgreSQL**:
  ```bash
  psql -U username -d database_name -f /path/to/backup/dump.sql
  ```

## Conclusion

Following this guide will help you back up and restore your VPS effectively. Always verify the integrity of your backups and ensure permissions are correctly set after restoration.
