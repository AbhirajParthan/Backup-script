# Bash script for automated WordPress backup

WordPress has lots of options to take a backup. There are some surprising plugins, and some hosting providers have separate packages for backup management.

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

~~~

The backup script will create a backup file in the BACKUP_DIR directory. The Backup directory location is /root/backup.

-----
## We can create a daily backup using a corn job.

First please upload the script to your server and saved it to the file in .sh format. Then give the execution permission to the file

eg:  I have uploaded the script to abhiraj.sh file in the /root location. Then gave the execute permission 


~~~ 
 cd /root/
 
 vi abhiraj.sh
 
 chmod +x abhiraj.sh
 ~~~
 
-----
## Automation Step Two: CRON

CRON is a task scheduler for Linux. To add a task to the CRON scheduler, you simply add a line to the "crontab". 

~~~
crontab -e
~~~

This will open up the CRON file in your text editor. Please add the below content in the file and save it.

~~~
0 1 * * * /root/abhiraj.sh
~~~

The script will run automatically and take the backup in every day 1 AM.



### ⚙️ Connect with Me

<p align="center">
 <a href="https://www.instagram.com/_r.e.b.e.l.z_33/"><img src="https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white"/></a>
<a href="https://www.linkedin.com/in/abhiraj-parthan-82038b191"><img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/></a> 
