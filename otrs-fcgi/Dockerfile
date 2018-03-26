FROM alpine:3.7

RUN apk --no-cache --update add \
    perl \
    perl-dbi \
    perl-archive-zip \
    perl-crypt-eksblowfish \
    perl-crypt-ssleay \
    perl-date-format \
    perl-datetime \
    perl-dbi \
    perl-dbd-mysql \
    perl-dbd-pg \
    perl-encode-hanextra \
    perl-io-socket-ssl \
    perl-json \
    perl-lwp-useragent-determined \
    perl-mail-imapclient \
    perl-net-dns \
    perl-ldap \
    perl-template-toolkit \
    perl-xml-libxml \
    perl-xml-libxslt \
    perl-xml-parser \
    perl-yaml-libyaml \
    perl-text-csv_xs \
    mini-sendmail \
    perl-fcgi \
    spawn-fcgi \
    fcgiwrap \
    && adduser -u 5000 -S otrs \
    && mkdir /opt \
    && mkdir /data

WORKDIR /opt

ARG VERSION=6.0.6

RUN wget http://ftp.otrs.org/pub/otrs/otrs-$VERSION.tar.gz \
    && tar -xzf otrs-$VERSION.tar.gz \
    && ln -s /opt/otrs-$VERSION /opt/otrs \
    && rm -f /opt/otrs-$VERSION.tar.gz \
    && cp /opt/otrs/Kernel/Config.pm.dist /data/Config.pm \
    && ln -s /data/Config.pm /opt/otrs/Kernel/Config.pm \
    && mv /opt/otrs/Custom /data/ \
    && ln -s /data/Custom /opt/otrs/Custom \
    && chown -R otrs /opt/otrs/ \
    && chown -R otrs /data

VOLUME /data
VOLUME /opt/otrs

EXPOSE 9000

USER otrs

CMD ["/usr/bin/spawn-fcgi", "-p 9000", "-n", "/usr/bin/fcgiwrap"]
