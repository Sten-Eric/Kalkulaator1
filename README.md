#!/bin/bash
cd /var/www/live
git pull origin main


#!/bin/bash
rsync -av --delete /var/www/live/ /var/www/html/
rsync -av --delete /var/www/html/ /var/www/backup/
