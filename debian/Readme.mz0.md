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
dput ppa:marcuzero/compat mysql-5.7_5.7.29-u20.04.0_source.changes
```

Output:
```
Checking signature on .changes
gpg: /home/mz0/m1/mysql-5.7_5.7.29-u20.04.0_source.changes: Valid signature from 5AD858E1E7A86031
Checking signature on .dsc
gpg: /home/mz0/m1/mysql-5.7_5.7.29-u20.04.0.dsc: Valid signature from 5AD858E1E7A86031
Package includes an .orig.tar.gz file although the debian revision suggests
that it might not be required. Multiple uploads of the .orig.tar.gz may be
rejected by the upload queue management software.
Uploading to ppa (via ftp to ppa.launchpad.net):
  Uploading mysql-5.7_5.7.29-u20.04.0.dsc: done.
  Uploading mysql-5.7_5.7.29.orig.tar.gz: done.
  Uploading mysql-5.7_5.7.29.orig.tar.gz.asc: done.
  Uploading mysql-5.7_5.7.29-u20.04.0.debian.tar.xz: done.
  Uploading mysql-5.7_5.7.29-u20.04.0_source.buildinfo: done.
  Uploading mysql-5.7_5.7.29-u20.04.0_source.changes: done.
Successfully uploaded packages.
```

See also

[SO](https://askubuntu.com/a/28373) on `dpkg-buildpackage -rfakeroot -uc -b`

[Tutorial](https://wiki.debian.org/BuildingTutorial) on `dpatch-edit-patch`, `debian/patches/00list`, `dch -n`, `debuild -tc` option (clean build artifacts);

P.S. cleanup: `git clean -d -f -x`
