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

P.S. cleanup: `git clean -Xid -f`

P.P.S. last update
```
 1  gl1 -20 origin/ubuntu/bionic-security 
 2  vi debian/changelog
 3  git diff e3975b2ff debian/changelog
 4  git add debian/changelog
 5  git checkout e3975b2ff debian/patches/
 6  gids  e3975b2ff
 7  git checkout e3975b2ff zlib/ win vio/ unittest/ testclients/ 
 8  git checkout e3975b2ff scripts/ sql*
 9  git checkout e3975b2ff lib*
10  git checkout e3975b2ff my* man/
11  git checkout e3975b2ff plugin/ rapid/ regex/
12  git checkout e3975b2ff INSTALL LICENSE README VERSION boost/ client/ cm*
13  git checkout e3975b2ff mysql-test include/ storage/
14  git checkout e3975b2ff Docs/ cmake/ 
15  gids e3975b2ff 
16  git checkout e3975b2ff  Doxyfile-perfschema
17  git checkout e3975b2ff cmake/os/
18  git rm cmake/os/GNU.cmake
19  git rm debian/patches/disable_crl_tests.patch
20  git rm mysql-test/std_data/crldir/b23bb52f.r0
21  git checkout e3975b2ff strings/CHARSET_INFO.txt
22  git checkout e3975b2ff support-files/MacOSX/ReadMe.html
23  git rm mysql-test/suite/rpl/r/rpl_gtid_delete_memory_table_after_start_server.result 
24  git rm mysql-test/suite/rpl/t/rpl_no_gtid_delete_memory_table_after_start_server.test 
25  git rm mysql-test/suite/rpl/r/rpl_no_gtid_delete_memory_table_after_start_server.result 
26  git rm mysql-test/suite/rpl/t/rpl_gtid_delete_memory_table_after_start_server.test 
27  gids  e3975b2ff 
28  gpg -k
29  export DEB_SIGN_KEYID=3E129A0B86B81C2E237AAD92A8F983E0F23A1B1B
30  debuild -S -sa
31  cd ..
32  dput ppa:marcuzero/compat mysql-5.7_5.7.32-1_source.changes
```
