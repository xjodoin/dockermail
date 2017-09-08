FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

# Prerequisites
RUN apt-get update && apt-get install -y \
    opendkim \
    opendkim-tools \
    openssl \
    rsyslog && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY ./config/rsyslog.conf /etc/rsyslog.conf
COPY ./config/opendkim /etc/opendkim
COPY ./config/opendkim.conf etc/opendkim.conf

# Nice place for your settings
VOLUME ["/mail_settings"]

# Configure boot script
COPY boot /
# And the key generation script
COPY keygen /
RUN chmod 755 /boot /keygen

ENV OPEN_DKIM=true

EXPOSE 8891
CMD /boot; opendkim -p inet:8891@0.0.0.0; rsyslogd -n
