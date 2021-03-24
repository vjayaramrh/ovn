FROM ubuntu:20.04
MAINTAINER "Aliasgar Ginwala" <aginwala@ebay.com>

ARG OVN_BRANCH
ARG GITHUB_SRC
ARG DISTRO

RUN truncate -s0 /tmp/preseed.cfg && \
       (echo "tzdata tzdata/Areas select America" >> /tmp/preseed.cfg) && \
       (echo "tzdata tzdata/Zones/America select Los_Angeles" >> /tmp/preseed.cfg) && \
       debconf-set-selections /tmp/preseed.cfg && \
       rm -f /etc/timezone /etc/localtime && \
       apt-get update && \
       DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true \
       apt-get install -y tzdata

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

copy $DISTRO/build.sh /build.sh
copy install_ovn.sh /install_ovn.sh
RUN /build.sh $OVN_BRANCH $GITHUB_SRC

COPY ovn_default_nb_port /etc/ovn/ovn_default_nb_port
COPY ovn_default_sb_port /etc/ovn/ovn_default_sb_port
COPY ovn_default_northd_host /etc/ovn/ovn_default_northd_host

COPY start-ovn /bin/start-ovn
VOLUME ["/var/log/openvswitch", \
"/var/lib/openvswitch", "/var/run/openvswitch", "/etc/openvswitch", \
"/var/log/ovn", "/var/lib/ovn", "/var/run/ovn", "/etc/ovn"]
ENTRYPOINT ["start-ovn"]
