--- nginx.spec	2016-07-21 10:06:41.313305529 +0100
+++ nginx-libressl-tcpfastopen.spec	2016-07-21 10:08:22.754298477 +0100
@@ -1,3 +1,4 @@
+%define libressl_version 2.3.6
 #
 %define nginx_home %{_localstatedir}/cache/nginx
 %define nginx_user nginx
@@ -76,7 +77,7 @@
 
 %define bdir %{_builddir}/%{name}-%{main_version}
 
-%define WITH_CC_OPT $(echo %{optflags} $(pcre-config --cflags))
+%define WITH_CC_OPT $(echo %{optflags} -DTCP_FASTOPEN=23 $(pcre-config --cflags))
 
 %define COMMON_CONFIGURE_ARGS $(echo "\
         --prefix=%{_sysconfdir}/nginx \
@@ -120,6 +121,8 @@
         --with-mail_ssl_module \
         --with-file-aio \
         --with-ipv6 \
+        --with-openssl="%{_sourcedir}/portable-%{libressl_version}" \
+        --with-ld-opt="-lrt" \
         %{?with_http2:--with-http_v2_module}")
 
 Summary: High performance web server
@@ -209,6 +212,12 @@
 Dynamic nJScript module for nginx.
 
 %prep
+cd %{_sourcedir}
+wget "https://github.com/libressl-portable/portable/archive/v%{libressl_version}.tar.gz" -O %{_sourcedir}/v%{libressl_version}.tar.gz
+tar -xzf v%{libressl_version}.tar.gz
+cd portable-%{libressl_version}
+./autogen.sh
+
 %setup -q
 tar xvzf %SOURCE13
 cp %{SOURCE2} .
