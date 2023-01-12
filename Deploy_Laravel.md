### How to Point Domain and Host Laravel Project on Apache Web Server VPS Hosting

- Login to Your Domain Provider Website
- Navigate to Manage DNS
- Add Following Records:

| Type | Host/Name | Value |
| :---: | :---: | :--- |
| A     | @     | Your Remote Server IP |
| A     | www   | Your Remote Server IP |
| AAAA  | @     | Your Remote Server IPv6 |
| AAAA  | www   | Your Remote Server IPv6 |

- On Local Windows Machine Make Your Project Folder a Zip File using any of the software e.g. winzip
- Open Command Prompt
- Copy Zip File from Local Windows Machine to Linux Remote Server
```sh
Syntax:- scp -P Port_number Source_File_Path Destination_Path
Example:- scp -P 22 osmsProject.zip root@216.32.44.12:/var/www
```
- Copied Successfully

- To Access Remote Server via SSH
```sh
Syntax:- ssh -p PORT USERNAME@HOSTIP
Example:- ssh -p 22 root@216.32.44.12
```
#### Note:- Run Below Commands on Your Remote Server Linux Machine or VPS, Not on Your Local Windows Machine
- Go to the Destination Path Where you copied the zip file
```sh
Syntax:- cd Destination_Path
Example:- cd /var/www
```
- Run ls command to verify that the zip file is present
```sh
ls
```
- Unzip the Copied Zip File
```sh
Syntax:- unzip zip_file_name
Example:- unzip osmsProject.zip
```
- Verify that all required softwares are installed
```sh
apache2 -v
mysql
php -v
composer -v
```
- Verify Apache2 is Active and Running
```sh
service apache2 status
```
- Verify Web Server Ports are Open and Allowed through Firewall
```sh
ufw status verbose
```
- Create Virtual Host File
```sh
nano /etc/apache2/sites-available/your_domain.conf
```
- Add Following Code in Virtual Host File
```sh
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/project_folder_name/public
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
- Enable Virtual Host
```sh
cd /etc/apache2/sites-available/
a2ensite your_domain.conf
```
- You can Disable Default Virtual Host
```sh
cd /etc/apache2/sites-available/
a2dissite 000-default.conf
```
- Restart Apache2
```sh
service apache2 restart
```
#### Go to Your Project Directory then run below artisan command
- Clearing cache
```sh
php artisan cache:clear
```
- Generate Application Key
```sh
php artisan key:generate
```
- Install Dependencies
```sh
composer install --optimize-autoloader --no-dev
```
- Set Permission for storage and bootstrap/cache Folder
```sh
chmod -R 777 storage
chmod -R 777 bootstrap/cache
```
- Create Database
- Create Database Tables
```sh
php artisan migrate
```
- Create Symbolic Link at public/storage which points to the storage/app/public directory.
```sh
php artisan storage:link
```
- Cache routes, config and views (Optional)
```sh
php artisan route:cache
php artisan config:cache
php artisan view:cache
```
- If you ever need to work on your project you can switch ON/OFF Maintenance mode 
```sh
php artisan down
php artisan up
```


