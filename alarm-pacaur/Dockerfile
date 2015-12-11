FROM trmpln/alarm:latest
MAINTAINER Giampaolo Mancini <giampaolo@trampolineup.com>

# install base-devel and needed tools
RUN pacman --noconfirm --needed -Sy base-devel git expac yajl cower perl

RUN groupadd -r pacaur && \
    useradd -r -g pacaur pacaur && \
    mkdir /tmp/pacaur && \
    chown -R pacaur:pacaur /tmp/pacaur && \
    chmod -R 771 /tmp/pacaur

# cloning and install pacaur
USER pacaur 
WORKDIR /tmp/pacaur
RUN git clone https://aur.archlinux.org/pacaur.git
WORKDIR /tmp/pacaur/pacaur
RUN sed -i -e '/build()/,+4d' -e '/man8/,+1d' PKGBUILD
RUN makepkg --noconfirm
USER root 
WORKDIR /tmp/pacaur/pacaur
RUN pacman --noconfirm -U *.tar.xz

# remove user pacaur (for security reasons)
RUN userdel -r pacaur
