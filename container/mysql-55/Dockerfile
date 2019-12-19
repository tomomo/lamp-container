FROM mysql:5.5

LABEL maintainer "tomomo <eclairpark@gmail.com>"

RUN { \
      echo '[mysqld]'; \
      echo 'character-set-server=utf8mb4'; \
      echo 'collation-server=utf8mb4_bin'; \
      echo "default-time-zone='+9:00'"; \
      echo 'skip-character-set-client-handshake'; \
      echo '[client]'; \
      echo 'default-character-set=utf8mb4'; \
    } > /etc/mysql/conf.d/charset.cnf \
  && \
    { \
      echo 'mysqld: ALL: allow'; \
    } >> /etc/hosts.allow
