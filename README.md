
**Step 1** 

Go to your ckpool directory and get current directory address via this command

``echo $PWD``

note it down

**Step 2**
Create a bash script with name j.sh

``nano j.sh``

**Step 3**
Copy paste the following lines into it

```

#!/bin/bash

dir="/root/ckpool/master/"


tail "${dir}/logs/ckpool.log" >| /var/www/html/ckpool_latest.log

cat "${dir}/logs/pool/pool.status" >| /var/www/html/ckpool_update.log


```

replace dir variable value with the directory address that you got from step 1

**Step 4**
run the following command to check whether the script is working alright
```
sudo bash j.sh
```

now access following url to see whether it worked:
http://vps_url/ckpool_update.log
http://vps_url/ckpool_latest.log

**Step 4**
now create a cronjob to update these two files after every 10 seconds
run this command
``crontab -e``

and append these lines at the end. make sure to replace "/root/ckpool/master" with the directory address that you got from step 1
```
* * * * * ( sudo bash /root/ckpool/master/j.sh )
* * * * * ( sleep 10 ; sudo bash /root/ckpool/master/j.sh )
* * * * * ( sleep 20 ; sudo bash /root/ckpool/master/j.sh )
* * * * * ( sleep 30 ; sudo bash /root/ckpool/master/j.sh )
* * * * * ( sleep 40 ; sudo bash /root/ckpool/master/j.sh )
* * * * * ( sleep 50 ; sudo bash /root/ckpool/master/j.sh )
```
save it

**Step 5**
now cd into your webserver directory

``cd /var/www/html``

and create an index.php ``nano index.php`` with following lines





```<html>

<body>
<h3>CK Pool latest updaet</h3>
<pre>
  <code>
<?php echo nl2br( file_get_contents('ckpool_update.log') ); ?>
</pre>
  </code>

<br><hr><br>

<h3>CK Pool latest Log</h3>
<pre>
  <code>
<?php echo nl2br( file_get_contents('ckpool_latest.log') ); ?>
</pre>
  </code>


</body>

</html>

```






