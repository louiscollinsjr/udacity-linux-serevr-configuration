#### Udacity - Linux-ServerConfiguration







#### udacity-linux-server-configuration





#### 1. Get Amazon Lightsail Server

#### 2. Connect using your own SSH client

You can connect to the AWS instance using the following address and user name:

- Pulbic IP: 54.145.197.176
- User name: grader
- Application URL: https://www.trott.app
- SSH Connection: ssh -i ~/.ssh/[key file] grader@54.145.197.176 -p 2200



> [keyfile] is the full path of the provided key for the grader account.



### Secure your server.

#### 3. Update al currently installed packages.

![carbon-3](/screenshots/carbon-3.png?raw=true)

#### 4. Change the SSH port from **22** to **2200**. 

![carbon-4](/screenshots/carbon-4.png?raw=true)

- Change port from 22 to  **2200**

- On AWS Lightsail configure networking by adding additional ports: **TCP 2200**, **UDP 123**, **TCP 443**. Remove **TCP 22**.



#### 5. Configure the Uncomplicated Firewall (UFW)

Configure the Uncomplicated Firewall (UFW)** to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

![carbon-5](/screenshots/carbon-5.png?raw=true)

> *Warning:* When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.





### Give grader account access.

In order for your project to be reviewed, the grader needs to be able to log in to your server.



#### 6. Create a new user account named grader.

![carbon-6](/screenshots/carbon-6.png?raw=true)

#### 7. Give grade the permission to sudo.

![carbon-7](/screenshots/carbon-7.png?raw=true)

- Add the following text and save: **grader ALL=(ALL:ALL) ALL**



#### 8. Create an SSH key pair for grader using the ssh-keygen tool.



On your local device, generate new key pair:

![carbon-8](/screenshots/carbon-8.png?raw=true)

View and copy your public key:

![carbon-9](/screenshots/carbon-9.png?raw=true)

On the server, 

![carbon-11](/screenshots/carbon-11.png?raw=true)

Paste your public key  in authorized_keys and Save.

![carbon-12](/screenshots/carbon-12.png?raw=true)

Configure login to block root login and use key authorization

![carbon-13](/screenshots/carbon-13.png?raw=true)

- Set **PermitRootLogin** to **no**
- Set **PasswordAuthenication** to  **no**



Restart SSH

![carbon-14](/screenshots/carbon-14.png?raw=true)





### Prepare to deploy your project.



#### 9. Configure the local timezone to UTC.

![carbon-15](/screenshots/carbon-15.png?raw=true)

#### 10. Install and configure Apache to serve a Python mod_wsgi application.

Install Apache

![carbon-16](/screenshots/carbon-16.png?raw=true)

- If you built your project with Python 3, you will need to install the Python 3 mod_wsgi package on your server

![carbon-17](/screenshots/carbon-17.png?raw=true)

#### 11.Install and configure PostgreSQL

- Do not allow remote connections
- Create a new database user named `catalog` that has limited permissions to your catalog application database.

![carbon-19](/screenshots/carbon-19.png?raw=true)

#### 12. Install git

![carbon-18](/screenshots/carbon-18.png?raw=true)



### Deploy the Item Catalog Project



Clone and setup your **Item Catalog** project from the Github repository you created earlier in this Nanodegree program.

Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your `.git` directory is not publicly accessible via a browser!



#### 13. Clone Catalog

![carbon-20](/screenshots/carbon-20.png?raw=true)

#### 14. Complete server configuration



##### Install virtual environment (/var/www/catalog)

![carbon-21](/screenshots/carbon-21.png?raw=true)

##### Install required.txt (/var/www/catalog/catalog)

![carbon-22](/screenshots/carbon-22.png?raw=true)

![carbon-23](/screenshots/carbon-23.png?raw=true)



##### Update all currently installed packages.

![carbon-3](/screenshots/carbon-3.png?raw=true)

##### Create catalog.wsgi

![carbon-27](/screenshots/carbon-27.png?raw=true)

![carbon-26](/screenshots/carbon-26.png?raw=true)



##### Configure and Enable a New Virtual Host

![carbon-25](/screenshots/carbon-25.png?raw=true)

![carbon-24](/screenshots/carbon-24.png?raw=true)



##### Enable the catalog app virtual host site and disable 000-default.conf

![carbon-39](/screenshots/carbon-39.png?raw=true)

##### Rename application-pep8.py to __ init __.py

![carbon-28](/screenshots/carbon-28.png?raw=true)



##### Update __ init __.py with absolute paths

Change __ init  __ .py to reflect  absolute paths and enable virtual site.

![carbon-29](/screenshots/carbon-29.png?raw=true)

![carbon-30](/screenshots/carbon-30.png?raw=true)

- Change `engine = create_engine('sqlite:///category.db')` to `engine = create_engine('postgresql://catalog:catalog@localhost/catalog')`
- Change `client_secrets.json`  to `/var/www/catalog/catalog/client_secrets.json`



##### Monitoring apache logs

![carbon-35](/screenshots/carbon-35.png?raw=true)

![carbon-34](/screenshots/carbon-34.png?raw=true)

##### Configure https for facebook login

facebook login requires https and DNS; so trott.app was configured in AWS and Certbot was used to confiure the appropriate certificates... configuration is already noted in previous screen shots.



Cerbot: https://certbot.eff.org/lets-encrypt/ubuntubionic-apache



##### Install Certbot

![carbon-36](/screenshots/carbon-36.png?raw=true)

![carbon-37](/screenshots/carbon-37.png?raw=true)



### Test the web application

Browse to the public ip address of the server and the web application should start up.
