# nginx-libressl
* Minimal change to nginx.spec file
* Edit patch/spec file to change LibreSSL version, etc
* Added -DTCP_FASTOPEN=23 CC option with spec and patch
* Unfortunately removed -fPIC Position-Independent Code FLAG so LibreSSL can compile. (there is probably a better fix)

## Install dependencies
```bash
yum -y install openssl-devel perl-devel perl-ExtUtils-Embed GeoIP-devel zlib-devel pcre-devel libxslt-devel gd-devel gcc-c++ make which wget autoconf libtool rpmdevtools git patch

# With ldap Auth dependencies
yum -y install openldap-devel openssl-devel perl-devel perl-ExtUtils-Embed GeoIP-devel zlib-devel pcre-devel libxslt-devel gd-devel gcc-c++ make which wget autoconf libtool rpmdevtools git patch
```

## Quick build nginx 1.11.13 on CentOS 7 - LibreSSL + TCPFastopen
**Caution -fPIC Position-Independent Code flag removed**  
* Get your nginx SRPM from here: http://nginx.org/packages/mainline/centos/7/SRPMS/  
```bash
sudo -i
cd /root
rpm -i "http://nginx.org/packages/mainline/centos/7/SRPMS/nginx-1.11.13-1.el7.ngx.src.rpm"
git clone "https://github.com/DRN88/nginx-libressl.git"
patch -p 3 rpmbuild/SPECS/nginx.spec < nginx-libressl/nginx-1.11.13/nginx-1.11.13-libressl-tcpfastopen.patch
rpmbuild -bb /root/rpmbuild/SPECS/nginx.spec
```

## Nginx build options
```bash
[root@rpmbuilder01 ~]# nginx -V
nginx version: nginx/1.11.13
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) 
built with LibreSSL 2.5.3
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-openssl=/root/rpmbuild/SOURCES/portable-2.5.3 --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -DTCP_FASTOPEN=23' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -lrt'
```

## Generate a new patch
https://docs.moodle.org/dev/How_to_create_a_patch  
http://www.cyberciti.biz/faq/appy-patch-file-using-patch-command/  
```bash
diff -Naur rpmbuild/SPECS/nginx.spec nginx-libressl.spec > nginx-libressl.patch
```
