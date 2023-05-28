# Host multiple website on LEMP ( Linux Nginx Mysql PHP ) in a single ec2 instance
To host multiple websites on a single AWS Ubuntu LEMP (Linux, Nginx, MySQL, PHP) EC2 instance, you can follow these general steps:

1. Launch an Ubuntu EC2 instance: Log in to your AWS console, navigate to EC2, and launch a new Ubuntu EC2 instance.

2. Set up security groups: During the instance launch, configure the security groups to allow inbound traffic on port 80 (HTTP) and port 443 (HTTPS).

3. Connect to your EC2 instance: Use SSH to connect to your EC2 instance using a terminal or SSH client.

4. Update the system: Run the following commands to update the system packages:
   ```
   sudo apt update
   sudo apt upgrade
   ```

5. Install Nginx: Install Nginx web server using the following command:
   ```
   sudo apt install nginx
   ```

6. Configure Nginx: Edit the Nginx default configuration file:
   ```
   sudo nano /etc/nginx/sites-available/default
   ```

   Replace the contents of the file with the following configuration (example assuming two websites, example.com and example2.com):
   ```
   server {
       listen 80;
       server_name example.com www.example.com;
       root /var/www/example.com;
       index index.html index.htm index.php;

       location / {
           try_files $uri $uri/ /index.php?$args;
       }

       location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass unix:/run/php/php7.4-fpm.sock;
       }
   }

   server {
       listen 80;
       server_name example2.com www.example2.com;
       root /var/www/example2.com;
       index index.html index.htm index.php;

       location / {
           try_files $uri $uri/ /index.php?$args;
       }

       location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass unix:/run/php/php7.4-fpm.sock;
       }
   }
   ```

   Adjust the server_name, root, and fastcgi_pass values based on your specific websites and PHP version.

7. Create website directories: Create directories for your websites using the following commands:
   ```
   sudo mkdir /var/www/example.com
   sudo mkdir /var/www/example2.com
   ```

8. Set file permissions: Set appropriate permissions for the website directories:
   ```
   sudo chown -R www-data:www-data /var/www/example.com
   sudo chown -R www-data:www-data /var/www/example2.com
   ```

9. Enable the websites: Create symbolic links to enable the websites in Nginx using the following commands:
   ```
   sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
   ```

10. Test Nginx configuration: Check for any syntax errors in your Nginx configuration by running the following command:
    ```
    sudo nginx -t
    ```

11. Restart Nginx: Restart Nginx to apply the changes:
    ```
    sudo systemctl restart nginx
    ```

12. Upload your website files: Upload your website files to the respective directories (/var/www/example.com and /var/www/example2.com) using SFTP or any other method you prefer.

13. Repeat steps 6 to 12 for additional websites: If you have more websites to host, repeat steps 6 to 12 for each additional website, ensuring you configure separate server blocks for each site.

Once you have completed these steps, you should have multiple websites hosted on a single AWS Ubuntu LEMP EC2 instance using Nginx. Remember to update DNS settings for each website's domain to point to your EC2 instance's IP address.
