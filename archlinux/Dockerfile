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

RUN su docker -c 'pacaur -S --needed --noprogressbar --noedit --noconfirm pciutils mcelog edac-utils bmc ipmitool smartmontools usbutils dmidecode'

RUN cd /home/docker/sandground \
  && mkdir -p go/src/github.com/ericsson \
  && export GOPATH=/home/docker/sandground/go \
  && cd /home/docker/sandground/go/src/github.com/ericsson \
  && git clone https://github.com/Ericsson/ericsson-hds-agent \
  && cd /home/docker/sandground/go/src/github.com/ericsson/ericsson-hds-agent/agent/apps/ericsson-hds-agent \
  && go get ./... \
  && go build \
  && cp ericsson-hds-agent /usr/local/bin

# savelist=("go" "mpfr" "pacman" "pacaur" "grep" "sudo" "nano" "vi" "gawk" "pciutils" "mcelog" "edac-utils" "bmc" "ipmitool" "smartmontools" "usbutils" "dmidecode") 
RUN /bin/bash -c 'pacman -Rdd --noconfirm $(comm -23 <(pacman -Qq | sort -u) <(cat <((for i in "go" "mpfr" "pacman" "pacaur" "grep" "sudo" "nano" "vi" "gawk" "pciutils" "mcelog" "edac-utils" "bmc" "ipmitool" "smartmontools" "usbutils" "dmidecode"; do pactree -ul "$i"; done) | sort -u) |sort -u))' \ 
  && pacman -Scc --noconfirm \
  &&  rm /var/cache/pacman/pkg/* \
  && paccache -rf 

# RUN /bin/bash -c "du /usr -hd 1 |sort -h"
# RUN /bin/bash -c "ls /usr/lib -la |sort -h"

RUN rm -rd /home/docker/sandground
