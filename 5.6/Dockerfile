# -*- mode: Dockerfile; tab-width: 4;indent-tabs-mode: nil;-*-
# vim: ts=4 sw=4 ft=Dockerfile et: 1
FROM nanobox/runit

# Create directories
RUN mkdir -p /var/log/gonano

USER gonano

# Install binaries
RUN rm -rf /data/var/db/pkgin && \
    /data/bin/pkgin -y up && \
    /data/bin/pkgin -y in \
        galera \
        mysql-server-5.6 && \
    rm -rf /data/var/db/pkgin/cache

USER root

# Install binaries
RUN rm -rf /opt/gonano/var/db/pkgin && \
    /opt/gonano/bin/pkgin -y up && \
    /opt/gonano/bin/pkgin -y in \
      rsync && \
    rm -rf /var/gonano/db/pkgin/cache

# Install binaries
RUN apt-get update && \
    apt-get install -y lsof sysstat libsys-hostname-long-perl libfile-copy-recursive-perl libfile-copy-link-perl libdata-dumper-concise-perl libdata-dumper-simple-perl iputils-arping cron && \
    rm -rf \
        /var/lib/apt/lists/* \
        /var/cache/apt/* \
        /tmp/* \
        /var/tmp/*

RUN /opt/gonano/bin/gem install remote_syslog_logger

# Install hooks
RUN mkdir -p /opt/nanobox/hooks && \
    mkdir -p /var/nanobox && \
    curl \
      -f \
      -k \
      https://s3.amazonaws.com/tools.nanobox.io/hooks/mysql-stable.tgz \
        | tar -xz -C /opt/nanobox/hooks && \
    curl \
      -f \
      -k \
      -o /var/nanobox/hooks.md5 \
      https://s3.amazonaws.com/tools.nanobox.io/hooks/mysql-stable.md5

# Cleanup disk
RUN rm -rf \
        /var/lib/apt/lists/* \
        /var/cache/apt/* \
        /tmp/* \
        /var/tmp/*

WORKDIR /data

# Run runit automatically
CMD [ "/opt/gonano/bin/nanoinit" ]
