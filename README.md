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

## Nginx build options
```bash
[root@rpmbuilder7 ~]# nginx -V
nginx version: nginx/1.10.1
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-4) (GCC)
built with LibreSSL 2.3.7
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-http_ssl_module --with-http_realip_module --with-http_addition_module --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_stub_status_module --with-http_auth_request_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_geoip_module=dynamic --with-http_perl_module=dynamic --add-dynamic-module=njs-1c50334fbea6/nginx --with-threads --with-stream --with-stream_ssl_module --with-http_slice_module --with-mail --with-mail_ssl_module --with-file-aio --with-ipv6 --with-openssl=/root/rpmbuild/SOURCES/portable-2.3.6 --with-ld-opt=-lrt --with-http_v2_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic'
[root@rpmbuilder7 ~]#
```

## Generate a new patch
https://docs.moodle.org/dev/How_to_create_a_patch  
http://www.cyberciti.biz/faq/appy-patch-file-using-patch-command/  
```bash
diff -Naur rpmbuild/SPECS/nginx.spec nginx-libressl.spec > nginx-libressl.patch
```
