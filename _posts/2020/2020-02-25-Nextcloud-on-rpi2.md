On storage node (banana pi)
Install apache2 then enable webdav modules
a2enmod dav_fs
a2enmod dav


htpasswd -c /var/www/passwd.dav nextcloud
chown root:www-data /var/www/passwd.dav 
chmod 640 /var/www/passwd.dav

On nextcloud node (raspberry pi)
Install nextcloud
Improve timeout for app store fetching
In lib/private/App/AppStore/Fetcher/Fetcher.php
On line 98 change the timeout from 10 to 30 or 90
Performance tuning
apt-get install php-apcu

in config.php, add
'memcache.local' => '\OC\Memcache\APCu',

APCu is disabled by default on CLI which could cause issues with nextcloud’s cron jobs. Please make sure you set the apc.enable_cli to 1 on your php.ini config file.

isable thumbnail generation by adding the following line to config/config.php in your NextCloud app folder.

'enable_previews' => false,

