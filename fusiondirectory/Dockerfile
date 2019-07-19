FROM php:7.2-fpm-stretch

ARG COMMIT="local-build"
ARG DATE="1970-01-01T00:00:00Z"
ARG URL="https://github.com/zsoerenm/fusiondirectory-docker"
ARG VERSION="dirty"

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.build-date=$DATE \
    org.label-schema.vendor="Soeren Zorn" \
    org.label-schema.name="zsoerenm/fusiondirectory-php" \
    org.label-schema.description="A PHP container with Fusiondirectory" \
    org.label-schema.version="$VERSION" \
    org.label-schema.vcs-url=$URL \
    org.label-schema.vcs-ref=$COMMIT

ENV FUSIONDIRECTORY_VERSION=1.3 \
  SCHEMA2LDIF_VERSION=1.3 \
  SMARTY_VERSION=3.1.31 \
  SMARTYGETTEXT_VERSION=1.5.1

RUN apt-get update && apt-get install -y \
  libcurl4-gnutls-dev \
  libmagickwand-dev \
  libc-client-dev \
  libkrb5-dev \
  libldap2-dev \
  librecode-dev

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
  && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
  && docker-php-ext-install gettext curl gd imap ldap recode \
  && pecl install imagick \
  && docker-php-ext-enable imagick \
  && docker-php-ext-configure opcache --enable-opcache \
  && docker-php-ext-install opcache \
  && rm -r /var/lib/apt/lists/*

RUN set -x; apt-get update && apt-get install -y wget unzip \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    ldap-utils \
    gettext \
    javascript-common \
    libarchive-extract-perl \
    libcrypt-cbc-perl \
    libdigest-sha-perl \
    libfile-copy-recursive-perl \
    libnet-ldap-perl \
    libpath-class-perl \
    libterm-readkey-perl \
    libxml-twig-perl \
    openssl \
  && rm -rf /var/lib/apt/lists/* \
  \
  && mkdir -p /usr/src/smarty /usr/src/smarty-gettext /usr/share/php/smarty3 \
  && curl https://codeload.github.com/smarty-php/smarty/tar.gz/v${SMARTY_VERSION} | tar xvfz - --strip 1 -C /usr/src/smarty \
  && cp -r /usr/src/smarty/libs/* /usr/share/php/smarty3 \
  && ln -s /usr/share/php/smarty3/Smarty.class.php /usr/share/php/smarty3/smarty.class.php \
  && curl https://codeload.github.com/smarty-gettext/smarty-gettext/tar.gz/${SMARTYGETTEXT_VERSION} | tar xvfz - --strip 1 -C /usr/src/smarty-gettext \
  && mkdir -p /usr/share/php/smarty3/plugins \
  && cp -R /usr/src/smarty-gettext/block.t.php /usr/share/php/smarty3/plugins/ \
  && cp -R /usr/src/smarty-gettext/tsmarty2c.php /usr/sbin \
  && chmod 750 /usr/sbin/tsmarty2c.php \
  \
  && curl https://repos.fusiondirectory.org/sources/schema2ldif/schema2ldif-${SCHEMA2LDIF_VERSION}.tar.gz| tar xvfz - --strip 1 -C /usr \
  && rm -rf /usr/CHANGELOG  \
  && rm -rf /usr/LICENSE

RUN wget -nv http://repos.fusiondirectory.org/sources/fusiondirectory/fusiondirectory-${FUSIONDIRECTORY_VERSION}.tar.gz \
    http://repos.fusiondirectory.org/sources/fusiondirectory/fusiondirectory-plugins-${FUSIONDIRECTORY_VERSION}.tar.gz \
    http://repos.fusiondirectory.org/sources/fusiondirectory/CHECKSUM.MD5 \
  && cat CHECKSUM.MD5 | grep fusiondirectory-${FUSIONDIRECTORY_VERSION}.tar.gz | md5sum -c \
  && cat CHECKSUM.MD5 | grep fusiondirectory-plugins-${FUSIONDIRECTORY_VERSION}.tar.gz | md5sum -c

RUN tar -xzf fusiondirectory-${FUSIONDIRECTORY_VERSION}.tar.gz \
  && rm fusiondirectory-${FUSIONDIRECTORY_VERSION}.tar.gz \
  && tar -xzf fusiondirectory-plugins-${FUSIONDIRECTORY_VERSION}.tar.gz \
  && mv fusiondirectory-plugins-${FUSIONDIRECTORY_VERSION} fusiondirectory-plugins \
  && rm fusiondirectory-plugins-${FUSIONDIRECTORY_VERSION}.tar.gz

#ARG PLUGIN_LIST="autofs,developers,ejbca,ipmi,nagios,posix,quota,spamassassin,supann\
#  ,webservice,alias,certificates,dhcp,fai,ldapdump,netgroups,postfix,renater-partage\
#  ,squid,sympa,applications,community,dns,freeradius,ldapmanager,newsletter,ppolicy\
#  ,repository,ssh,systems,argonaut,cyrus,dovecot,fusioninventory,mail,opsi,puppet\
#  ,samba,subcontracting,user-reminder,audit,debconf,dsa,gpg,mixedgroups,personal\
#  ,pureftpd,sogo,sudo,weblink"
ARG PLUGIN_LIST=""

RUN echo ${PLUGIN_LIST} | sed 's/,/\n/g' | awk '{print "fusiondirectory-plugins/" $0; }' > plugin_list_file.txt \
  && if cat plugin_list_file.txt | grep -c ^fusiondirectory-plugins/$; then rm -r fusiondirectory-plugins/*; fi \
  && tar -czf fusiondirectory-plugins.tar.gz --files-from plugin_list_file.txt \
  && rm -r fusiondirectory-plugins \
  && rm plugin_list_file.txt

RUN mv fusiondirectory-${FUSIONDIRECTORY_VERSION} fusiondirectory \
  && curl -O http://ajax.googleapis.com/ajax/libs/prototype/1.7.1.0/prototype.js \
  && curl -O http://script.aculo.us/dist/scriptaculous-js-1.9.0.zip \
  && unzip -d . scriptaculous-js-1.9.0.zip \
  && mv prototype.js fusiondirectory/html/include \
  && mv scriptaculous-js-1.9.0/src/scriptaculous.js fusiondirectory/html/include \
  && mv scriptaculous-js-1.9.0/src/builder.js fusiondirectory/html/include \
  && mv scriptaculous-js-1.9.0/src/controls.js fusiondirectory/html/include \
  && mv scriptaculous-js-1.9.0/src/dragdrop.js fusiondirectory/html/include \
  && mv scriptaculous-js-1.9.0/src/effects.js fusiondirectory/html/include \
  && rm -r scriptaculous-js-1.9.0 \
  && cp -R fusiondirectory/contrib/smarty/plugins/* /usr/share/php/smarty3/plugins/ \
  && chmod 0750 fusiondirectory/contrib/bin/* \
  && mv fusiondirectory/contrib/bin/* /usr/local/bin/

RUN sed -i 's/ask_user_input ("Where is your plugins archive ?");/"\/var\/www\/html\/fusiondirectory-plugins.tar.gz";/g' /usr/local/bin/fusiondirectory-setup

RUN fusiondirectory-setup -y --set-fd_home=/var/www/html/fusiondirectory --check-directories --update-cache --update-locales \
  && mv fusiondirectory/contrib/fusiondirectory.conf /var/cache/fusiondirectory/template/ \
  && fusiondirectory-setup --set-fd_home=/var/www/html/fusiondirectory --write-vars --install-plugins --check-directories --update-cache --update-locales \
  && fusiondirectory-setup --set-fd_home=/var/www/html/fusiondirectory --set-fd_smarty_dir="/usr/share/php/smarty3/Smarty.class.php" --write-vars

# Copy configuration
COPY config/opcache.ini $PHP_INI_DIR/conf.d/
