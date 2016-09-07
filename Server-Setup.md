## Shadowsocks-R manyuser Setup

Notice: The manyuser version will need to work with ss-panel(https://github.com/orvice/ss-panel).

The below commands will need to be excuted as root account, or sudo method.

### Basic Library Setup 

CentOS:

<pre>
yum install python-setuptools && easy_install pip
yum install git
</pre>

Ubuntu / Debian:

<pre>
apt-get install python-pip
apt-get install git
</pre>

#### Install cymysql

<pre>
pip install cymysql
</pre>

#### Get the source code

<pre>
git clone -b manyuser https://github.com/breakwa11/shadowsocks.git
</pre>

It will creates a folder named `shadowsocks`, the root dictory will be manyuser version (MySQL version), the sub-dictory will be single user version.

Root dictory ./shadowsocks

Sub-dictory ./shadowsocks/shadowsocks

### Server side configuration

In the shadowsocks folder, please make a copy of apiconfig.py and rename it to userapiconfig.py, and make modification in userapiconfig.py:

<pre>
API_INTERFACE = 'sspanelv2' //Edit the interface type
</pre>

The interface type will be your database type, choose from `sspanelv2`, `sspanelv3`, `sspanelv3ssr`.

Rename `mysql.json` to `usermysql.json` and edit as below:

<pre>
{
    "host": "127.0.0.1",
    "port": 3306,
    "user": "ss",
    "password": "pass",
    "db": "shadowsocks",
    "node_id": 1,
    "transfer_mul": 1.0,
    "ssl_enable": 0,
    "ssl_ca": "",
    "ssl_cert": "",
    "ssl_key": ""
}
</pre>

They are (in order): Database host, Database Port, Database User, Database Password, Database Name, Node ID (Except for sspanelv3), traffic rate, whether to enable SSL for MySQL etc.

Keep in mind that sspanelv3 require correct node_id before working properly, before we fill in the ID, we need to add the node into panel, and make sure the node ID correct then fill in here.

Make a copy of config.json and rename to user-config.json, edit as below:

<pre>
"method":"aes-256-cfb",                   //Change to your prefered encryption method
"protocol": "auth_sha1_compatible",       //Change to your preferred agreement plugin
"obfs": "tls1.0_session_auth_compatible", //Change to your preferred obscure plugin
</pre>

### How to run and stop on server

Enter root dictory:

<pre>
cd shadowsocks
</pre>

and run:

<pre>
python server.py
</pre>

Now we can check if it runs, and see if there's any error. If everything works perfectly on server side but you still cannot connect to shadowsocks, you will need to check if it's your iptables or firewall (CentOS 7) block your connection.

#### Run as script

Add permission to run:

<pre>
chmod +x *.sh
</pre>

Run in background: (No log, but keep running after shell close)

<pre>
./run.sh
</pre>

Run in background: (With log enabled, keep running after shell close)

<pre>
./logrun.sh
</pre>

Check the running status when running in background:

<pre>
./tail.sh
</pre>

Stop it:

<pre>
./stop.sh
</pre>

Notice: The log file of running as script will be saved as ssserver.log under root dictory by default, you can check it if needed

### Update the code

When the code has latest update, you can update your local code by running command below:

<pre>
cd shadowsocks
git pull
</pre>

And restart Shadowsocks service after updating successfully.

### Other Error

If your python version on the server is lower than 2.6, you need to upgrade it to 2.6.x or 2.7.x.

If you get this error `Can\'t get hostname for your address`, you need to add below code to the mysqld section in `my.cnf`, and restart MySQL service.

As to others, please refer to https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit
