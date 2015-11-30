FROM trmpln/alarm:latest
MAINTAINER Giampaolo Mancini <giampaolo@trampolineup.com>

# synchronizing package list
RUN pacman --noconfirm -Sy

# install base-devel and needed tools
RUN pacman --noconfirm --needed -S base-devel git expac yajl cower perl

RUN groupadd -r pacaur && \
    useradd -r -g pacaur pacaur

RUN mkdir /tmp/pacaur && \
    chown -R pacaur:pacaur /tmp/pacaur && \
    chmod -R 771 /tmp/pacaur
USER pacaur 
WORKDIR /tmp/pacaur

## cloning and install pacaur dependency (cower)
#RUN git clone https://aur.archlinux.org/cower.git 
#WORKDIR /tmp/pacaur/cower
#RUN sed -i -e "/arch/s/_64'/_64' 'armv7h'/" PKGBUILD
#RUN makepkg --noconfirm --skippgpcheck
#USER root
#WORKDIR /tmp/pacaur/cower
#RUN pacman --noconfirm -U *.tar.xz

# cloning and install pacaur
USER pacaur 
WORKDIR /tmp/pacaur
RUN git clone https://aur.archlinux.org/pacaur.git
WORKDIR /tmp/pacaur/pacaur
RUN sed -i -e 's#pod2man#/usr/bin/core_perl/pod2man#' PKGBUILD
RUN makepkg --noconfirm
USER root 
WORKDIR /tmp/pacaur/pacaur
RUN pacman --noconfirm -U *.tar.xz

# remove user pacaur (for security reasons)
RUN userdel -r pacaur
