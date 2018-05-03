Based on https://hub.docker.com/_/php/

PHP-FPM based containers, adding PHP extensions that we often use:

Enabled default PHP extensions:
* gd 
* exif 
* mcrypt 
* zip 
* mbstring 
* intl 
* bcmath 
* curl 
* pdo_mysql 
* xsl 
* soap 
* mysqli 
* simplexml 
* sockets 
* xmlrpc 
* opcache 
* mysql 
* ldap

Added extensions from PECL:
* memcache
* memcached
* redis
* imagick

# Extra software
Used for apps that have no separated front-end (like react), we have added node so these can also be build inside the same container:
* node8 
* npm
* yarn 
* bower 
* gulp 
* pm2
