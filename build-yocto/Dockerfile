FROM ubuntu:18.04

RUN apt-get update && apt-get -y upgrade

# Required Packages for the Host Development System
# http://www.yoctoproject.org/docs/latest/mega-manual/mega-manual.html#required-packages-for-the-host-development-system
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping psmisc iproute2 iptables time

# Additional host packages required by poky/scripts/wic
RUN apt-get install -y curl dosfstools mtools parted syslinux tree

# Add "repo" tool (used by many Yocto-based projects)
RUN curl http://storage.googleapis.com/git-repo-downloads/repo > /usr/local/bin/repo
RUN chmod a+x /usr/local/bin/repo

ARG UID=3000

# Create a non-root user that will perform the actual build
RUN id yocto 2>/dev/null || useradd --uid $UID --create-home yocto
RUN apt-get install -y sudo
RUN echo "yocto ALL=(ALL) NOPASSWD: ALL" | tee -a /etc/sudoers

# Fix error "Please use a locale setting which supports utf-8."
# See https://wiki.yoctoproject.org/wiki/TipsAndTricks/ResolvingLocaleIssues
RUN apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
        echo 'LANG="en_US.UTF-8"'>/etc/default/locale && \
        dpkg-reconfigure --frontend=noninteractive locales && \
        update-locale LANG=en_US.UTF-8

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# See: ttps://medium.com/paperchain/fetching-private-github-repos-from-a-docker-container-273f25ec5a74
# Copy SSH key for git private repos
ADD .ssh/id_rsa /home/yocto/.ssh/id_rsa
RUN chmod 600 /home/yocto/.ssh/id_rsa
RUN chown yocto:yocto -R /home/yocto/.ssh

# Skip Host verification for git
RUN echo "StrictHostKeyChecking no" > /home/yocto/.ssh/config

USER yocto
WORKDIR /home/yocto
CMD "/bin/bash"

# EOF
