# Centos 7 Local Repository

## Installation
- Provisioning VM CentOS in OpenStack Lab

| VM | IP Address |
| :---: | :---: |
| repo-centos | 192.168.1.103 / 192.168.99.130 (floating ip) |

- Install Packages
```
yum update
yum install epel-release centos-release-openstack-queens
yum install yum-utils createrepo screen nano
```
- Create folder
```
mkdir -p /var/www/html/repos/{base,centosplus,extras,updates,epel}
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
reposync -g -l -d -m --repoid=epel --newest-only --download-metadata --download_path=/var/www/html/repos/
reposync -g -l -d -m --repoid=centos-openstack-queens --newest-only --download-metadata --download_path=/var/www/html/repos/
```
- Install Nginx
```
yum install nginx 
systemctl enable nginx
systemctl start nginx
```
- Stop firewalld
```
systemctl stop firewalld
systemctl disable firewalld
```
- Create a new repodata
```
createrepo -g comps.xml /var/www/html/repos/base/  
createrepo -g comps.xml /var/www/html/repos/centosplus/	
createrepo -g comps.xml /var/www/html/repos/extras/  
createrepo -g comps.xml /var/www/html/repos/updates/  
createrepo -g comps.xml /var/www/html/repos/epel/  
createrepo -g comps.xml /var/www/html/repos/centos-openstack-queens/  
```
- Create cronjob
```
nano /etc/cron.daily/update-localrepos
...
#!/bin/bash
##specify all local repositories in a single variable
LOCAL_REPOS=”base centosplus extras updates epel centos-openstack-queens”
##a loop to update repos one at a time 
for REPO in ${LOCAL_REPOS}; do
reposync -g -l -d -m --repoid=$REPO --newest-only --download-metadata --download_path=/var/www/html/repos/
createrepo -g comps.xml /var/www/html/repos/$REPO/  
done
...
chmod 755 /etc/cron.daily/update-localrepos
```

## Setup local Repository
```
vim /etc/yum.repos.d/local-repos.repo
```
```
[local-base]
name=CentOS Base
baseurl=http://192.168.99.130/base/
gpgcheck=0
enabled=1

[local-centosplus]
name=CentOS CentOSPlus
baseurl=http://192.168.99.130/centosplus/
gpgcheck=0
enabled=1

[local-extras]
name=CentOS Extras
baseurl=http://192.168.99.130/extras/
gpgcheck=0
enabled=1

[local-updates]
name=CentOS Updates
baseurl=http://192.168.99.130/updates/
gpgcheck=0
enabled=1

[local-epel]
name=CentOS Epel
baseurl=http://192.168.99.130/epel/
gpgcheck=0
enabled=1

[local-centos-openstack-queens]
name=CentOS centos-openstack-queens
baseurl=http://192.168.99.130/centos-openstack-queens/
gpgcheck=0
enabled=1
```
```
yum repolist all
```
