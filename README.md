# cservice-web
Installation guide
--------------------------------------------------------------------------
		Ubuntu 20.04.1 LTS \n \l - Server
--------------------------------------------------------------------------
Pay attention to the following: If any cservice-web module does not respond 
or gives you problems, you should ask the official Undernet network coders,
because we can only help you install the site, but never modify the code that they wrote.
--------------------------------------------------------------------------
#1. Prepare the system by installing the necessary components.

	root@ircd:~# apt install apache2 apache2-bin apache2-data apache2-dev apache2-doc apache2-ssl-dev apache2-utils libapache2-mod-php 
        php7.4 php7.4-cgi php7.4-cli php7.4-common php7.4-curl php7.4-dev php7.4-gd php7.4-json php7.4-mysql php7.4-pgsql php7.4-readline
	php7.4-sqlite3 php7.4-xml php7.4-xmlrpc libreadline-dev libssl-dev openssl zlib1g zlib1g-dev
	root@ircd:~# updatedb	
	root@ircd:~#  nano /etc/php/7.4/apache2/php.ini 
	NOW: Go to the line 187 and change short_open_tag (from Off to On)
	Save the file with (CTRL+O)

	root@ircd:~# service apache2 restart

	root@ircd:~# curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

#2. Now We going to download the GNUWorld cservice-web part!
	
	 root@ircd:~#
	 root@ircd:~# su - gnuworld
	 gnuworld@ircd:~$ 
	 gnuworld@ircd:~$ git clone https://github.com/UndernetIRC/cservice-web/
	 gnuworld@ircd:~$ cd cservice-web
	 gnuworld@ircd:~/gnuworld/cservice-web$ composer install
	 gnuworld@ircd:~/gnuworld/cservice-web$ cd php_includes
	 gnuworld@ircd:~/gnuworld/cservice-web/php_includes$ cp config.inc.dist config.inc
	 EDIT: config.inc (With your own values)
	 
	 gnuworld@ircd:~/gnuworld/cservice-web/php_includes$ cd ..
	 gnuworld@ircd:~/gnuworld/cservice-web/ cd ..
	 gnuworld@ircd:~/gnuworld$

#3. Now we back to root user, for some user permission.
	 
	 gnuworld@ircd:~/gnuworld$ su
	 root@ircd:~# cd /var/www/html/
	 root@ircd:/var/www/html#
	 root@ircd:/var/www/html# chmod 711 ~gnuworld
  	 root@ircd:/var/www/html# chmod 711 ~gnuworld/cservice-web
  	 root@ircd:/var/www/html# chmod 755 ~gnuworld/cservice-web/php_includes
  	 root@ircd:/var/www/html# chmod 644 ~gnuworld/cservice-web/php_includes/config.inc
  	 root@ircd:/var/www/html# chmod 755 ~gnuworld/cservice-web/docs/gnuworld/
  	 root@ircd:/var/www/html# ln -s /home/gnuworld/cservice-web/docs/gnuworld 
 	 root@ircd:/var/www/html# cd ..
  	 root@ircd:/var/www# cd ..
   	 root@ircd:/var# cd ..
   	 root@ircd~# 
	
	 root@ircd~# cd /etc/apache2/sites-enabled
 	 root@ircd~# nano 000-default.conf
         (check the correct shortcut there where is located cservice-web.)
        
#4. We back again into gnuworld user		 	 
         
     root@ircd:~# cd
	 root@ircd:~# su - gnuworld
	 gnuworld@ircd:~/gnuworld$ cd /gnuworld/cservice-web/php_includes
	 gnuworld@ircd:~/gnuworld/cservice-web/php_includes$
	 
	 NOTE: This entry is for the first username created in the db. (Admin or your custom username)
	       
	 NOW: nano ipr.sql 
	 Write your own ip. We show you as an example the first entry if you are trying to access locally.
	 insert into ip_restrict (id, user_id, added_by, added, type, expiry, value) values (1, 1, 1, now()::abstime::int4, 1, 0, '192.168.1.0/24');
	 Save the file with (CTRL+O)
	 
	 With the gnuworld running, perform:
	 gnuworld@ircd:~/gnuworld/cservice-web/php_includes$ /usr/local/pgsql/bin/psql -h 127.0.0.1 cservice < ipr.sql 
	 
	 NOW you are able to login into cservice-web with Admin + temPass if you didn??t change it with the ip provided.
	 You can add more ips from user edit via web, so no need to add another one.
	 
	 That??s all :)
	 REVIEW: y2k - ARI3L
