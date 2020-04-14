# Connecting-Laravel-with-Microsoft-SQL
Step 1: Install Apache2
	sudo apt install apache2
(This will install Apache2 in your linux m/c).
	
	sudo systemctl start apache2.service 
(This will start apache2 as service)
	sudo systemctl enable apache2.service
(This will enable apache2 as system’s service.)

Step 2: Install PHP and Related Modules
	sudo apt-get install software-properties-common
(Installs and enables Software-common properties)

	sudo add-apt-repository ppa:ondrej/php
(Add php mirrors toyour servers repository)

	sudo apt-get update
(Update all installation and packages)

	sudo apt-get install -y php7.2
(Installs PHP version 7.2)

	sudo apt install php libapache2-mod-php php-common php-mbstring php-xmlrpc php-soap php-gd php-xml php-mysql php-cli php-mcrypt php-zip
(Installs all necessary Extensions required for LARAVEL)
	
Step3:- Install SqlSRV module
	sudo su (Enables sudo root )
	curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - 
(Adds microsoft Mirror for linux in your server’s Installation repo) 
	
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
(Curls packages of microsoft sql latest release in your Linux repo).

	sudo apt-get update
(Updates all oabove steps in operating system so that they start to respond at the point of installation)

	sudo ACCEPT_EULA=Y apt-get install msodbcsql17
(Accepts odbc driver for ms sql to respond for installation)

# optional: for bcp and sqlcmd
	sudo ACCEPT_EULA=Y apt-get install mssql-tools
	echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
	echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
	source ~/.bashrc
# optional: for unixODBC development headers
	sudo apt-get install unixodbc-dev
	
Step4:-Install pecl and enableing extensions(Installs PECL packages for php so that MS SQL dependencies can be overcomed for installation and proper functioning)

	sudo apt-get -y install gcc g++ make autoconf libc-dev pkg-config
 (Installs DEV-package )
 
	sudo pecl7.2 install sqlsrv
 (Installs SQLSRV-the main playing php extension for establishing connection between php and mssql) 
 
	sudo pecl7.2 install pdo_sqlsrv
	
(Installs PHP Data Objects PDO_SQLSRV-the main playing php extension for establishing connection between php and mssql)

	sudo bash -c "echo extension=sqlsrv.so > /etc/php7.X-sp/conf.d/sqlsrv.ini"
	
(Declares sqlsrv extension to the desired PHP version directory)

	sudo bash -c "echo extension=pdo_sqlsrv.so > /etc/php7.X-sp/conf.d/pdo_sqlsrv.ini"
	
(Declares pdo_sqlsrv extension to the desired PHP version directory))

Step5:-Virtual host
Copy the code files in the desired directory and make changes in the virtual host of apache2 
<VirtualHost *:1114>
    DocumentRoot "/var/www/html/public"
    ServerName myLaravel.dev
    <Directory "/var/www/html">
            AllowOverride All
            Options FollowSymLinks +Indexes
            Order allow,deny
            Allow from all
    </Directory>
</VirtualHost>
save the virtual host file by any name,using .conf extension
for example dsgroup.conf
and then enable the virtual host file 

sudo a2ensite dsgroup.conf

Step6:-Running the project
php artisan config:clear (Clears config folder present in project).
php artisan config:cache(Clears cache from config folder in project).
php artisan migrate(Establishes connection with MS SQL)
composer update/install (Installs and updates composer and other dependencies)
npm install(Installs project based npm modules and dependencies.)


Errors and Alternatives :-
In case any error related to DB connection occurs you can try installing following php extensions which are additional and may correct the error:-

sudo apt-get install -y php7.2-pdo-odbc
Or

sudo apt-get install -y php7.2-pdo-dblib

Or

Sudo apt-get install php7.2-sybase
