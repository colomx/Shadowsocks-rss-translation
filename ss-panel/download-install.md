### Download 

You can always download the latest ss-panel at https://github.com/orvice/ss-panel/releases

Let's make an example:

<pre>
cd /home/www
git clone https://github.com/orvice/ss-panel.git 
</pre>

### Web Server Configuration

#### Apache

Add below lines to VirtualHost:

<pre>
<Directory /home/www/ss-panel/public>
   AllowOverride All
   Order Deny,Allow
   Allow from all
</Directory>
</pre>

Remember to enabled `mod_rewrite` module.

#### Nginx

<pre>
root /home/www/ss-panel/public;

location / {
   try_files $uri $uri/ /index.php$is_args$args;
}  
</pre>

### Basic Configuration

1. Import `db.sql` into your database.

2. Add permission for storage folder

<pre>
chmod -R 777 storage
</pre>

3. Make a copy of configuration file

<pre>
cp .env.example .env
</pre>

4. Use `composer` to install 3rd party libirary

<pre>
cd /home/www/ss-panel
curl -sS https://getcomposer.org/installer | php
php composer.phar  install
</pre>

If you get any errors please install the missed PHP module based on error given, and re-run `php composer.phar install`

Done!

