# nginx-libressl

## Install dependencies
```bash
yum -y install openssl-devel perl-devel perl-ExtUtils-Embed GeoIP-devel zlib-devel pcre-devel libxslt-devel gd-devel gcc-c++ make which wget autoconf libtool
```

## Quick build on CentOS 7
```bash
sudo -i
cd /root

rpm -i "http://nginx.org/packages/centos/7/SRPMS/nginx-1.10.1-1.el7.ngx.src.rpm"

git clone "https://github.com/DRN88/nginx-libressl.git"
patch -p 3 rpmbuild/SPECS/nginx.spec < nginx-libressl/nginx-libressl.patch

rpmbuild -bb /root/rpmbuild/SPECS/nginx.spec

```

## PROFIT
