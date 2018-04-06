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

ENV OTRS_VERSION 6.0.6
ENV OTRS_DOWNLOAD_SHA256 0c0be293065c12a90a25e83f51a603ed700f1280c2845595fd4f718ddfe13c14

RUN wget http://ftp.otrs.org/pub/otrs/otrs-$OTRS_VERSION.tar.bz2 \
    && echo "$OTRS_DOWNLOAD_SHA256  otrs-$OTRS_VERSION.tar.bz2" | sha256sum -c - \
    && tar -xjf otrs-$OTRS_VERSION.tar.bz2 \
    && ln -s /opt/otrs-$OTRS_VERSION /opt/otrs \
    && rm -f /opt/otrs-$OTRS_VERSION.tar.bz2

ADD custom-agent-skin.xml /data/
ADD custom-customer-skin.xml /data/

RUN cp /opt/otrs/Kernel/Config.pm.dist /data/Config.pm \
    && ln -s /data/Config.pm /opt/otrs/Kernel/Config.pm \
    && mv /opt/otrs/Custom /data/ \
    && rm -rf /opt/otrs/var/tmp \
    && ln -s /tmp /opt/otrs/var/tmp \
    && ln -s /data/Custom /opt/otrs/Custom \
    && mkdir /data/custom-agent-skin \
    && ln -s /data/custom-agent-skin /opt/otrs/var/httpd/htdocs/skins/Agent/custom-agent-skin \
    && ln -s /data/custom-agent-skin.xml /opt/otrs/Kernel/Config/Files/XML/custom-agent-skin.xml \
    && mkdir /data/custom-customer-skin \
    && ln -s /data/custom-customer-skin /opt/otrs/var/httpd/htdocs/skins/Customer/custom-curstomer-skin \
    && ln -s /data/custom-customer-skin.xml /opt/otrs/Kernel/Config/Files/XML/custom-customer-skin.xml \
    && chown -R otrs /opt/otrs/ \
    && chown -R otrs /data

VOLUME /data
VOLUME /opt/otrs

EXPOSE 9000

USER otrs

CMD ["/usr/bin/spawn-fcgi", "-p 9000", "-n", "/usr/bin/fcgiwrap"]
