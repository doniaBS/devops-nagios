FROM alpine:3.17

RUN apk add --no-cache \
    ca-certificates \
    curl \
    git \
    libc6-compat \
    openssh \
    sudo \
    wget

RUN echo "export DEBIAN_FRONTEND=noninteractive" >> /etc/profile
RUN mkdir -p /opt/nagios
RUN wget https://github.com/NagiosEnterprises/nagioscore/archive/refs/tags/nagioscore-6.4.6.tar.gz -O /tmp/nagioscore.tar.gz
RUN tar xzf /tmp/nagioscore.tar.gz -C /opt/nagios --strip-components=1

RUN cd /opt/nagios && ./configure --prefix=/opt/nagios
RUN make -j$(nproc) && make install

RUN mkdir -p /etc/nagios
RUN echo "source /opt/nagios/etc/nagios.cfg" >> /etc/profile

RUN useradd -rM nagios -s /bin/bash -G apache
RUN echo "nagios ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/nagios

RUN chown -R nagios:nagios /opt/nagios
RUN chown -R nagios:apache /etc/nagios

RUN cd /etc/nagios && cp -f /opt/nagios/etc/nagios.sample /etc/nagios/nagios.cfg
RUN echo "object definitions http localhost { 127.0.0.1:81; }" >> /etc/nagios/objects/commands.cfg

EXPOSE 80

CMD ["/opt/nagios/bin/nagios", "-d"]


