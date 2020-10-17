2020-10-17

Github says:  
This branch is 34 commits ahead, 10 commits behind applied/debian/experimental.
```

cd ~/m1/mysql-5.7 && \
git fetch origin

From git://git.launchpad.net/ubuntu/+source/mysql-5.7
   9bffc73d1..f812645ff  applied/ubuntu/bionic-devel     -> origin/applied/ubuntu/bionic-devel
   9bffc73d1..f812645ff  applied/ubuntu/bionic-security  -> origin/applied/ubuntu/bionic-security
   9bffc73d1..f812645ff  applied/ubuntu/bionic-updates   -> origin/applied/ubuntu/bionic-updates
   97e672c9e..61e91ff66  applied/ubuntu/xenial-devel     -> origin/applied/ubuntu/xenial-devel
   97e672c9e..61e91ff66  applied/ubuntu/xenial-security  -> origin/applied/ubuntu/xenial-security
   97e672c9e..61e91ff66  applied/ubuntu/xenial-updates   -> origin/applied/ubuntu/xenial-updates
   02b0e9480..d024afb9b  importer/ubuntu/dsc             -> origin/importer/ubuntu/dsc
   1fd57e125..ef6aa9140  ubuntu/bionic-devel             -> origin/ubuntu/bionic-devel
   1fd57e125..ef6aa9140  ubuntu/bionic-security          -> origin/ubuntu/bionic-security
   1fd57e125..ef6aa9140  ubuntu/bionic-updates           -> origin/ubuntu/bionic-updates
   64f1a3830..b200054d9  ubuntu/xenial-devel             -> origin/ubuntu/xenial-devel
   64f1a3830..b200054d9  ubuntu/xenial-security          -> origin/ubuntu/xenial-security
   64f1a3830..b200054d9  ubuntu/xenial-updates           -> origin/ubuntu/xenial-updates
 * [new tag]             applied/5.7.31-0ubuntu0.16.04.1 -> applied/5.7.31-0ubuntu0.16.04.1
 * [new tag]             applied/5.7.31-0ubuntu0.18.04.1 -> applied/5.7.31-0ubuntu0.18.04.1
 * [new tag]             import/5.7.31-0ubuntu0.16.04.1  -> import/5.7.31-0ubuntu0.16.04.1
 * [new tag]             import/5.7.31-0ubuntu0.18.04.1  -> import/5.7.31-0ubuntu0.18.04.1

gids f812645ff
git checkout f812645ff client/ VERSION LICENSE Docs/INFO_SRC CMakeLists.txt
git rm -r cmd-line-utils/libedit

cd cmd-line-utils/libedit/
git rm makelist.sh CMakeLists.txt README 
git rm -r editline/
cd ../../

git checkout f812645ff config.h.cmake configure.cmake
git checkout f812645ff debian/po/

cd extra/
git rm -r libevent/
git checkout f812645ff libevent
git rm -r rapidjson/
git checkout f812645ff RAPIDJSON-README rapidjson
cd ..

git checkout f812645ff libmysql/ include/
git checkout f812645ff mysys packaging/
git checkout f812645ff storage/ vio/ unittest/ support-files/ strings/
git checkout f812645ff sql-common/ scripts/ plugin/
git rm -fr mysql-test/ rapid/ sql/
git checkout f812645f mysql-test/ rapid/ sql/

cd debian/
git checkout f812645ff additions/ tests/upstream
git checkout f812645ff tests/upstream
git rm debian/patches/year2020.patch
git rm patches/year2020.patch
git add changelog
git add patches/series
git rm patches/fix-func-math-test.patch 
git rm patches/fix_tests_ppc64el.patch
git checkout f812645ff patches/disable_tests.patch
git checkout f812645ff patches/disable_crl_tests.patch

cd debian/
git checkout f812645ff control copyright
git checkout f812645ff mysql-server-5.7.examples \
 mysql-server-5.7.mysql-server.logrotate \
 mysql-server-5.7.postinst mysql-server-5.7.preinst \
 mysql-testsuite-5.7.install mysql-testsuite-5.7.links
cd ..
```

GPG
```
gpg --full-generate-key
(1) RSA and RSA (default)

What keysize do you want? (3072) 4096

Key is valid for? (0) 
Key does not expire at all

gpg: key A8F983E0F23A1B1B marked as ultimately trusted
gpg: revocation certificate stored as '/home/mz0/.gnupg/openpgp-revocs.d/3E129A0B86B81C2E237AAD92A8F983E0F23A1B1B.rev'
public and secret key created and signed.

pub   rsa4096 2020-10-17 [SC]
      3E129A0B86B81C2E237AAD92A8F983E0F23A1B1B
uid                      Mark Zhitomirski <mz@exactpro.com>
sub   rsa4096 2020-10-17 [E]

gpg --list-secret-keys --keyid-format LONG
/home/mz0/.gnupg/pubring.kbx
----------------------------
sec   rsa4096/5AD858E1E7A86031 2019-09-25 [SC] [expired: 2020-09-24]
      3F9CC149CE65263F3FA771095AD858E1E7A86031
uid                 [ expired] Mark Zhitomirski <mz@exactpro.com>

sec   rsa4096/A8F983E0F23A1B1B 2020-10-17 [SC]
      3E129A0B86B81C2E237AAD92A8F983E0F23A1B1B
uid                 [ultimate] Mark Zhitomirski <mz@exactpro.com>
ssb   rsa4096/4E7714A0ED9A821A 2020-10-17 [E]

mz0@nb13e:~$ gpg --armor --export A8F983E0F23A1B1B
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBF+K434BEAC6vNY7xHF3IWZ...
svKx8I4en6ZFKmr0q7A107O9f440WhH5shwD6o
=Oly/
-----END PGP PUBLIC KEY BLOCK-----

gpg --edit-key
gpg> adduid
gpg> q

gpg --send-keys A8F983E0F23A1B1B
gpg: sending key A8F983E0F23A1B1B to hkps://keys.openpgp.org

gpg --keyserver keyserver.ubuntu.com --recv-keys A8F983E0F23A1B1B

--------------
BURL=http://ftp.gwdg.de/pub/misc/mysql/Downloads/MySQL-5.7/
cd ~/m1
ORIGF=mysql-boost-5.7.31.tar.gz
ln -s mysql-5.7_5.7.31.orig.tar.gz $ORIGF
ln -s ${ORIGF}.asc mysql-5.7_5.7.31.orig.tar.gz.asc
cd mysql-5.7

dpkg-source --commit
dch -i # Debian changelog
debuild -S -sa

