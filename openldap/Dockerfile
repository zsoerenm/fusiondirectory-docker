FROM osixia/openldap:1.2.4

ARG COMMIT="local-build"
ARG DATE="1970-01-01T00:00:00Z"
ARG URL="https://github.com/zsoerenm/fusiondirectory-docker"
ARG VERSION="dirty"

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.build-date=$DATE \
    org.label-schema.vendor="Soeren Zorn" \
    org.label-schema.name="zsoerenm/fusiondirectory-openldap" \
    org.label-schema.description="A PHP container with OpenLdap for Fusiondirectory" \
    org.label-schema.version="$VERSION" \
    org.label-schema.vcs-url=$URL \
    org.label-schema.vcs-ref=$COMMIT

ENV FUSIONDIRECTORY_VERSION=1.3
ENV FUSIONDIRECTORY_KEY=D744D55EACDA69FF

RUN rm -f /etc/dpkg/dpkg.cfg.d/01_nodoc
RUN rm -f /etc/dpkg/dpkg.cfg.d/docker

RUN apt-get update \
  && apt-get install -y wget \
  &&  wget -nv http://repos.fusiondirectory.org/sources/1.0/fusiondirectory/fusiondirectory-${FUSIONDIRECTORY_VERSION}.tar.gz \
    http://repos.fusiondirectory.org/sources/1.0/fusiondirectory/fusiondirectory-plugins-${FUSIONDIRECTORY_VERSION}.tar.gz \
    http://repos.fusiondirectory.org/sources/1.0/fusiondirectory/CHECKSUM.MD5 \
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
  && tar -xzf fusiondirectory-plugins.tar.gz \
  && rm plugin_list_file.txt \
  && mv fusiondirectory-${FUSIONDIRECTORY_VERSION} fusiondirectory \
  && cp fusiondirectory/contrib/openldap/* /container/service/slapd/assets/config/bootstrap/schema/ \
  && cp fusiondirectory-plugins/*/contrib/openldap/* /container/service/slapd/assets/config/bootstrap/schema/ \
  && rm -r /container/service/slapd/assets/config/bootstrap/schema/mmc/
