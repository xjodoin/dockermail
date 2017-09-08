FROM ubuntu:16.04

ENV DEBIAN_FRONTEND noninteractive

# Prerequisites
RUN apt-get update && apt-get install -y \
    rsyslog \
    amavisd-new \
    spamassassin \
    clamav-daemon \
    libnet-dns-perl \
    libmail-spf-perl \
    pyzor \
    razor \
    arj bzip2 cabextract cpio file gzip lhasa nomarch pax unrar-free unzip zip zoo p7zip-full xzdec ripole rpm2cpio \
    jq && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN adduser clamav amavis && adduser amavis clamav

# Set up razor and pyzor
RUN su - amavis -s /bin/bash && razor-admin -create && razor-admin -register && pyzor discover

COPY ./config/conf.d /etc/amavis/conf.d
COPY ./config/rsyslog.conf /etc/rsyslog.conf

# Nice place for your settings
VOLUME ["/mail_settings"]

# Copy boot scripts
COPY boot /
COPY boot.d /boot.d
RUN chmod 755 /boot /boot.d/*

ENV AMAVIS=true

EXPOSE 10024
CMD ./boot; amavisd-new; rsyslogd -n
