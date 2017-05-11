FROM imriss/archlinux

RUN echo 'Ericsson HDS Agent Forked on Arch Linux'

MAINTAINER Reza Farrahi <imriss@ieee.org>
LABEL description="Ericsson HDS Agent Forked / Arch Linux"

RUN  pacman -Syyu --noconfirm \
  && pacman -S findutils nano vi wget --needed --noconfirm \
  && pacman-db-upgrade \
  && export editor=nano \
  && pacman -S --needed --noconfirm git wget gnuplot go go-bindata \
  && git config --global url."https://".insteadOf git:// 

# RUN  pacman -S --needed --noconfirm base-devel

# DDADD https://raw.githubusercontent.com/imriss/scylla/master/aur.sh /usr/sbin/aur.sh
# DDADD https://raw.githubusercontent.com/imriss/scylla/master/add-aur.sh /usr/sbin/add-aur
ADD ./aur.sh /usr/sbin/aur.sh
ADD ./add-aur.sh /usr/sbin/add-aur
RUN chmod u+x /usr/sbin/aur.sh
RUN chmod u+x /usr/sbin/add-aur
RUN add-aur docker

# RUN su docker -c 'pacaur -S --needed --noprogressbar --noedit --noconfirm python python-pip'

RUN cd /usr/lib/go/src/ \
  && go get github.com/Ericsson/ericsson-hds-agent \
  && cd github.com/Ericsson/ericsson-hds-agent/agent/apps/ericsson-hds-agent \
  && go build \
  && /bin/bash -c "ls . -la |sort -h"

RUN /bin/bash -c "comm -23 <(pacman -Qq) <(cat <(pactree -u go |sort) <(pactree -u mpfr |sort) <(pactree -u pacman |sort) <(pactree -u sudo |sort) <(pacman -Qgq base | sort) |sort -u) | xargs pacman -Rdd --noconfirm" \
RUN pacman -Scc --noconfirm \
  &&  rm /var/cache/pacman/pkg/* \
  && paccache -rf 

RUN /bin/bash -c "du /usr -hd 1 |sort -h"
# RUN /bin/bash -c "ls /usr/lib -la |sort -h"

RUN rm -rd /home/docker/sandground
