FROM httpd:2.4-alpine

LABEL maintainer "tomomo <eclairpark@gmail.com>"

RUN apk --update add --no-cache tzdata openssl && \
  # timezone
  cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
  # SSL - dummy
  mkdir -p /usr/local/apache2/certificate && \
  openssl req \
      -x509 \
      -newkey rsa:2048 \
      -keyout /usr/local/apache2/certificate/localhost+1-key.pem \
      -out    /usr/local/apache2/certificate/localhost+1.pem \
      -days 720 \
      -nodes \
      -subj "/C=JP/ST=Tokyo/L=Minato-ku/O=None/OU=Org/CN=localhost" && \
  # Clean up
  apk del tzdata && \
  rm -rf /var/cache/apk/* && \
  # Conf
  sed -i '/LoadModule authn_socache_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule cache_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule deflate_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule disk_cache_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule expires_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule fileter_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule headers_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule proxy_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule proxy_fcgi_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule rewrite_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule socache_shmcb_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule ssl_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule version_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/LoadModule http2_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
  sed -i '/Include conf\/extra\/httpd-vhosts\.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

COPY ./conf/extra/ /usr/local/apache2/conf/extra/

WORKDIR /var/www/html
