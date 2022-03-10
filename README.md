# Bash script for automated WordPress backup

In WordPress lots of options to take them back up. There are some surprising plugins and some hosting providers have separate packages for backup management.

But here, I have prepared the small bash script for taking the WordPress backup. The script will take the backup of our website files and database daily. The backup will be stored in compressed format.

The backup will be stored last 7 days' files & database, Also all others will be deleted.

~~~
#!/bin/bash

DATE=$(date +%d-%m-%Y)
BACKUP_DIR="/root/backup"


# Database Information
DB="abhiraj"
DBUSER="abhiraj"
PASSWORD="KapriKorn123!"

#Document Root Path
PATH="/var/www/"

#Document Root
DOCUMENTROOT="abhiraj.ga"

# To create a new directory into backup directory location.
mkdir -p $BACKUP_DIR/$DATE


# take website backup 

cd $PATH

tar -zcvf $BACKUP_DIR/$DATE/abhiraj.ga-$DATE.tar.gz  $DOCUMENTROOT


# Mysql Dump
mysqldump -u  $DBUSER  -p$PASSWORD $DB  >  $BACKUP_DIR/$DATE/$DB.sql


cd $BACKUP_DIR

tar -cvzf abhiraj.ga-$DATE.tar.gz $DATE


rm -rf $DATE


#Delete Files older Than 7 Days

find . -type f -mtime +7 -exec rm -f {} \;

cd

exit
~~~

We have to create a cron job Once in daily in our server. The backup dcript will create a backup file in the BACKUP_DIR directory. The Backup directory location is /root/backup.


