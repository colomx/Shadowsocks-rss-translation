This article is for ss-panel v3, please choose proper encryption method for password, auth method based on instruction.

Modify side and database configuration:

<pre>
vim .env
</pre>

### Auth Driver Configuration

ss-panel v3 supports multiple method to save user auth info.

  - file // use file to save sessions。
  - redis // use Redis to save, recommended.

#### Install Redis

<pre>
apt-get install redis-server
</pre>

#### Password Encryption Method

  - md5 //Not Recommended
  - sha256 //Recommended
  
#### Create Administrator

Excute below command under root dictory:

<pre>
php xcat createAdmin
</pre>

Create Administrator based on tips, and you will be able to login via /admin path.

#### Reset Traffic

<pre>
php xcat resetTraffic
</pre>

#### Send Traffic Usage mail

<pre>
php xcat sendDiaryMail
</pre>
