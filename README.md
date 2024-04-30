# Guide-to-Install-Frappe-ERPNext-in-Ubuntu-22.04-LTS And Setup Production Mode


A complete Guide to Install Frappe Bench in Ubuntu 22.04 LTS and install Frappe/ERPNext Application and Setup Production Mode.


## Pre-requisites


```bash
  Python 3.10+
  Node.js 18+
  Redis 5                                       (caching and real time updates)
  MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
  yarn 1.12+                                    (js dependency manager)
  pip 20+                                       (py dependency manager)
  wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
  cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
  NGINX                                         (proxying multitenant sites in production)
```

### STEP 1 Install git

```bash
sudo apt-get install git
```

### STEP 2 Install python-dev

```bash
sudo apt-get install python3-dev
```

### STEP 3 Install setuptools and pip (Python's Package Manager).

```bash
sudo apt-get install python3-setuptools python3-pip
```

### STEP 4 Install virtualenv

```bash
sudo apt-get install virtualenv
```

CHECK PYTHON VERSION

```bash
python3 -V
```

IF VERSION IS 3.8.X RUN

```bash
sudo apt install python3.8-venv
```

IF VERSION IS 3.10.X RUN

```bash
sudo apt install python3.10-venv
```

### STEP 5 Install MariaDB

```bash
sudo apt-get install software-properties-common
sudo apt install mariadb-server
sudo mysql_secure_installation
```

### STEP 6 MySQL database development files

```bash
sudo apt-get install libmysqlclient-dev
```

### STEP 7 Edit the mariadb configuration ( unicode character encoding )

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

add this to the 50-server.cnf file

```bash
[server]
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log

[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci      
 
[mysql]
default-character-set = utf8mb4
```

Now press (ctrl+s) to save and (Ctrl-X) to exit 

```bash
sudo service mysql restart
```

### STEP 8 install Redis

```bash
sudo apt-get install redis-server
```

### STEP 9 install Node.js 18.X package

```bash
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18 
```

### STEP 10 install Yarn

```bash
sudo apt-get install npm

sudo npm install -g yarn
```

### STEP 11 install wkhtmltopdf

```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

### STEP 12 install frappe-bench

```bash 
sudo -H pip3 install frappe-bench

bench --version
```

### STEP 13 initilise the frappe bench & install frappe latest version

```bash
bench init frappe-bench 

cd frappe-bench/

bench start
```
If on URL not show 404 error run following command

```bash
ufw status

sudo ufw allow 8000/tcp

sudo ufw enable
```


### STEP 14 Create a site in frappe bench

```bash
bench new-site dcode.com

bench use dcode.com

bench migrate
```

### STEP 15 Use screen for bench start and bench stop 

```bash
screen -ls 
```
Then type screen and press enter
```bash
screen
```
Then Press space one or two time

```bash
cd frappe-bench
```
For Start screen

```bash 
screen -r (r for reatached)
```
Then start the bench in screen

```bash
bench start
```
For reopen screen

```bash
screen -r session_name
```

### STEP 16 Install ERPNext latest version in bench & site

```bash
bench get-app erpnext --branch version-13
###OR
bench get-app https://github.com/frappe/erpnext --branch version-13

bench --site dcode.com install-app erpnext

bench migrate

bench start
```

### STEP 17 Setup production

```bash
sudo bench setup production dcode-frappe
```
After this stop bench in screen and then run following command

```bash
bench restart
```

If bench restart is not worked run the following command again with all Questions Yes

```bash
sudo bench setup production dcode-frappe
```
If bench restart is not worked run following command

```bash
bench setup socketio

bench setup supervisor

bench setup redis

sudo supervisorctl reload
```

If js and css file is not loading on login window run the following command

```bash
sudo chmod o+x /home/dcode-frappe
```

After bench restart successfully search your URL in browser without port (like 8000) At that time if page is shown only reloading then allow port on server for that run following command

```bash
sudo ufw status

sudo ufw allow 80/tcp

sudo ufw enable
```
Then refresh page .



