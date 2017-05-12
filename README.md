
# Server Ubuntu
## Setting up Item Catalog Webserver
### Objectives:

- Configure UFW and other firewall settings to allow only three ports:  WWW at port 80, SSH at port 2200, and NTP at port 123.

- Setup a user named grader with sudo capabilities, and ssh-based login

- Disable root login, and password based logins

- Make the item catalog along with the database configuration operational.

### User information

* username : grader
* password : Udacity!

###  Website: http://54.152.192.99/

* grader ssh login:
  ```
  ssh -i ~/grader grader@54.152.192.99 -p2200
  ```
  where the grader is the private key provided by other means.

* ubuntu ssh login:
  ```
  ssh -i ~/.ssh/LightsailDefaultPrivateKey.pem ubuntu@54.152.192.99 -p2200
  ```
<https://forums.aws.amazon.com/thread.jspa?threadID=244506>

#### Configuration of the Server after instance Setup

added the user grader:
```
sudo adduser grader
```
(password) Udacity!
entered Full Name as Udacity

Installed the following packages:
```
sudo apt install finger
sudo apt install postgresql-client-common
sudo apt install python-pip
sudo pip install SQLAlchemy
sudo pip install sqlalchemy-utils
sudo pip install psycopg2
sudo apt-get install postgresql
sudo apt-get install postgresql-contrib
sudo pip install Flask
sudo pip install --upgrade oauth2client
sudo pip install requests
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi
sudo apt-get install aptitude
sudo apt autoremove
sudo aptitude safe-upgrade
```

##### Cloned the Catalog Item File
```
git clone https://github.com/pkshreeman/Project-ItemCatalog.git
```
Then I moved the system to ```/var/www``` renaming the folders according to the structure mentioned later for deployment purposes.


##### Configured the cataloguser in Postgres
```
sudo su - postgres
psql
create user cataloguser with password 'password' createdb;
\q
exit
```
##### Setting up the database
```
sudo python database_setup.py
sudo python item-makers.py
```

I followed these instructions to enable the delopyment process:
<https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps>


##### SSH configuration and Keys
```
sudo nano /etc/ssh/sshd_config
```

 Configured the /etc/ssh/ssdh_config to NO to root login, and listen only to port 2200) and restarted the service.  Also configured on the lightsail configuration page to allow custom on port 2200,

On my mac,
```
ssh-keygen
```
with file named grader with no password, chmod'd it to 600 (private key). Copied the public key, sudo su grader while on ubuntu ssh's terminal, then mkdir .ssh folder, created authorized_keys and pasted the key in.

##### Sudo for the user grader
```
sudo visudo -f /etc/sudoers.d/grader
```
In the visudo, add the following:

```
grader ALL=(ALL)ALL
grader ALL=(ALL) NOPASSWD:ALL
```
