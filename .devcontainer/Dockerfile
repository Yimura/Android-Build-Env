FROM debian:trixie

ARG USER=android
ENV CCACHE_DIR=/home/${USER}/.cache/ccache
ENV CCACHE_MAXSIZE=10G
ENV CCACHE_COMPRESS=true

# make sure sudo is available
RUN apt update &&\
    apt install -y --no-install-recommends sudo &&\
    echo "${USER} ALL=(ALL:ALL) NOPASSWD: ALL" >> /etc/sudoers

# setup container user
RUN useradd -s `which bash` -m ${USER}

# device communication utilities (mainly adb and fastboot)
RUN apt update &&\
    apt install -y --no-install-recommends\
    android-platform-tools-base fastboot

RUN apt update &&\
    apt install -y --no-install-recommends\
    bc bison build-essential ccache curl flex\
    g++-multilib gcc-multilib git git-lfs gnupg\
    gperf imagemagick lib32readline-dev\
    lib32z1-dev libelf-dev lz4\
    libsdl1.2-dev libssl-dev libxml2\
    libxml2-utils lzop pngcrush rsync schedtool\
    squashfs-tools xsltproc zip zlib1g-dev

# download old deprecated packages directly :(
RUN apt update &&\
    apt install -y --no-install-recommends wget &&\
    wget http://ftp.de.debian.org/debian/pool/main/n/ncurses/libtinfo5_6.4-4_amd64.deb &&\
    dpkg -i libtinfo5_6.4-4_amd64.deb &&\
    rm -f libtinfo5_6.4-4_amd64.deb &&\
    wget http://ftp.de.debian.org/debian/pool/main/n/ncurses/libncurses5_6.4-4_amd64.deb &&\
    dpkg -i libncurses5_6.4-4_amd64.deb &&\
    rm -f libncurses5_6.4-4_amd64.deb

# several tools required for the repo tool and other surrounding services
RUN apt update &&\
    apt install -y --no-install-recommends\
    ssh\
    unzip\
    nano\
    less

# install repo command
RUN wget https://storage.googleapis.com/git-repo-downloads/repo -P /usr/bin &&\
    chmod a+x /usr/bin/repo

USER ${USER}
RUN echo "export USE_CCACHE=1\nexport CCACHE_EXEC=`which ccache`" >> ~/.bashrc
