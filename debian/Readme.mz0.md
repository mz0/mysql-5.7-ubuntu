# Rebuild log
```
sudo apt install devscripts

# build-dep
sudo apt install \
bison chrpath cmake debhelper dh-apparmor libaio-dev libedit-dev libevent-dev \
liblz4-dev libncurses5-dev libnuma-dev libssl-dev libwrap0-dev po-debconf \
zlib1g-dev

cd ~/m1/mysql-5.7
dpkg-source --commit
debuild -S
dput ppa:marcuzero/compat ../mysql-5.7_5.7.29-u20.04.0_source.changes

# upload rejected, need source upload
rm ../mysql-5.7_5.7.29-u20.04.0_source.ppa.upload
ln -s ~/m1/mysql-boost-5.7.29.tar.gz.asc \
 ../mysql-5.7_5.7.29.orig.tar.gz.asc

debuild -S -sa
cd ..
dput ppa:marcuzero/compat mysql-5.7_5.7.32-1_source.changes
```

See also

[SO](https://askubuntu.com/a/28373) on `dpkg-buildpackage -rfakeroot -uc -b`

[Tutorial](https://wiki.debian.org/BuildingTutorial) on `dpatch-edit-patch`, `debian/patches/00list`, `dch -n`, `debuild -tc` option (clean build artifacts);

P.S. cleanup: `git clean -Xid -f`

## Update 33 2021.02.12 (started around 8:50 UTC)
 0  cd ~/m1/mysql-5.7  
 1  git fetch origin  
```
remote: Enumerating objects: 1352, done.
remote: Counting objects: 100% (612/612), done.
remote: Compressing objects: 100% (51/51), done.
remote: Total 351 (delta 300), reused 350 (delta 299)
Receiving objects: 100% (351/351), 155.00 KiB | 1.72 MiB/s, done.
Resolving deltas: 100% (300/300), completed with 201 local objects.
From git://git.launchpad.net/ubuntu/+source/mysql-5.7
   e3975b2ff..16bb43b70  ubuntu/bionic-devel             -> origin/ubuntu/bionic-devel
   e3975b2ff..16bb43b70  ubuntu/bionic-security          -> origin/ubuntu/bionic-security
   e3975b2ff..16bb43b70  ubuntu/bionic-updates           -> origin/ubuntu/bionic-updates
   4e9ecd433..ddf94c463  applied/ubuntu/bionic-devel     -> origin/applied/ubuntu/bionic-devel
   4e9ecd433..ddf94c463  applied/ubuntu/bionic-security  -> origin/applied/ubuntu/bionic-security
   4e9ecd433..ddf94c463  applied/ubuntu/bionic-updates   -> origin/applied/ubuntu/bionic-updates
   ...
```
 2  ./debian/getSource 33  
 3  revision=$(git rev-parse --short origin/ubuntu/bionic-security)  
 4  git cherry-pick $revision  
 5  vi debian/changelog # fix it  
 6  git add debian/changelog  
 7  git cherry-pick --continue  
 8  export DEB_SIGN_KEYID=3E129A0B86B81C2E237AAD92A8F983E0F23A1B1B  
 9  debuild -S -sa  
10  cd ..  
11  dput ppa:marcuzero/compat mysql-5.7_5.7.33-1_source.changes  
```
Checking signature on .changes
gpg: /home/mz0/m1/mysql-5.7_5.7.33-1_source.changes: Valid signature from A8F983E0F23A1B1B
Checking signature on .dsc
gpg: /home/mz0/m1/mysql-5.7_5.7.33-1.dsc: Valid signature from A8F983E0F23A1B1B
Uploading to ppa (via ftp to ppa.launchpad.net):
  Uploading mysql-5.7_5.7.33-1.dsc: done.
  Uploading mysql-5.7_5.7.33.orig.tar.gz: done.
  Uploading mysql-5.7_5.7.33.orig.tar.gz.asc: done.
  Uploading mysql-5.7_5.7.33-1.debian.tar.xz: done.
  Uploading mysql-5.7_5.7.33-1_source.buildinfo: done.
  Uploading mysql-5.7_5.7.33-1_source.changes: done.
Successfully uploaded packages.
```
2021-02-12 09:07:17 -0000 Subject: [~marcuzero/ubuntu/compat/focal] mysql-5.7 5.7.33-1 (Accepted) From: Launchpad PPA  
2021-02-12 09:08:05 -0000 Build started  
2021-02-12 09:33:12 -0000 Build finished (Pending publication)  
2021-02-12 10:09:00 -0000 around this time package was found by `apt update` and upgraded OK  
