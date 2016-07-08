# nginx-libressl
* Minimal change to nginx.spec file
* Edit patch/spec file to change LibreSSL version, etc

## Install dependencies
```bash
yum -y install openssl-devel perl-devel perl-ExtUtils-Embed GeoIP-devel zlib-devel pcre-devel libxslt-devel gd-devel gcc-c++ make which wget autoconf libtool
```

## Quick build on CentOS 7
* Get your nginx SRPM from here: http://nginx.org/packages/centos/7/SRPMS/  
```bash
sudo -i
cd /root

rpm -i "http://nginx.org/packages/centos/7/SRPMS/nginx-1.10.1-1.el7.ngx.src.rpm"

git clone "https://github.com/DRN88/nginx-libressl.git"
patch -p 3 rpmbuild/SPECS/nginx.spec < nginx-libressl/nginx-libressl.patch

rpmbuild -bb /root/rpmbuild/SPECS/nginx.spec

```

## Artifacts
```bash
[root@rpmbuilder7 ~]# tree rpmbuild/RPMS/
rpmbuild/RPMS/
`-- x86_64
    |-- nginx-1.10.1-1.el7.centos.ngx.x86_64.rpm
    |-- nginx-debuginfo-1.10.1-1.el7.centos.ngx.x86_64.rpm
    |-- nginx-module-geoip-1.10.1-1.el7.centos.ngx.x86_64.rpm
    |-- nginx-module-image-filter-1.10.1-1.el7.centos.ngx.x86_64.rpm
    |-- nginx-module-njs-1.10.1.0.0.20160414.1c50334fbea6-1.el7.centos.ngx.x86_64.rpm
    |-- nginx-module-perl-1.10.1-1.el7.centos.ngx.x86_64.rpm
    `-- nginx-module-xslt-1.10.1-1.el7.centos.ngx.x86_64.rpm

1 directory, 7 files
```

## Generate a new patch
https://docs.moodle.org/dev/How_to_create_a_patch  
http://www.cyberciti.biz/faq/appy-patch-file-using-patch-command/  
```bash
diff -Naur rpmbuild/SPECS/nginx.spec nginx-libressl.spec > nginx-libressl.patch
```
