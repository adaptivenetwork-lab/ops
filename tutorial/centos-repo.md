# Centos 7 Local Repository
- Provisioning VM CentOS in OpenStack Lab
- Install Packages
```
yum update
yum install yum-utils createrepo screen
```
- Create folder
```
mkdir -p /var/www/html/repos/{base,centosplus,extras,updates,cloud}
```
- Start screen
```
screen
```
- Start Sync
```
reposync -g -l -d -m --repoid=base --newest-only --download-metadata --download_path=/var/www/html/repos/
reposync -g -l -d -m --repoid=centosplus --newest-only --download-metadata --download_path=/var/www/html/repos/
reposync -g -l -d -m --repoid=extras --newest-only --download-metadata --download_path=/var/www/html/repos/
reposync -g -l -d -m --repoid=updates --newest-only --download-metadata --download_path=/var/www/html/repos/
reposync -g -l -d -m --repoid=cloud --newest-only --download-metadata --download_path=/var/www/html/repos/
```
