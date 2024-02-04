# syntax=docker/dockerfile:1
FROM almalinux:8

RUN dnf install -y sudo
RUN dnf install -y passwd
RUN dnf install -y procps-ng
RUN dnf install -y net-tools

# The following packages are needed by TSC
RUN dnf install -y redhat-lsb-core
RUN dnf install -y xdg-utils
RUN dnf install -y mesa-libGL
RUN dnf install -y mesa-dri-drivers

# The following packages are needed by plugins needed by TSC and were missing
RUN dnf install -y libxkbcommon
RUN dnf install -y libxkbcommon-x11
RUN dnf install -y xcb-util-wm
RUN dnf install -y xcb-util-image
RUN dnf install -y xcb-util-keysyms
RUN dnf install -y xcb-util-renderutil

# Install some X11 apps, such as xeyes
RUN dnf --enablerepo=powertools install -y xorg-x11-apps
RUN dnf install -y xauth

# Install some usefull utilities
RUN dnf install -y vim
RUN dnf install -y subversion

# Python stuff
RUN dnf install -y python3
RUN dnf install -y python3-tkinter
RUN python3 -m pip install pysimplegui

# Install TSC
COPY TSC-1.0.31324-x86_64-Linux.rpm /tmp/
RUN rpm -i /tmp/TSC-1.0.31324-x86_64-Linux.rpm
RUN dnf install -y gedit

ENV USER_ID=${USER_ID:-1000}
ENV GROUP_ID=${GROUP_ID:-1000}
ENV USER=${USER:-tscrun}
ENV HOME=/home/${USER}
RUN groupadd ${USER} 
RUN useradd ${USER} -m -d ${HOME} -s /bin/bash -g ${USER}
RUN usermod -aG wheel ${USER}

USER ${USER}

WORKDIR ${HOME}
COPY TESTENV TESTENV
COPY simpleGUI.py .
# THe following helps identify any dependency issues with Qt plugins.
ENV QT_DEBUG_PLUGINS=1
# CMD python3 simpleGUI.py

#RUN     export uid=1000 gid=1001
#RUN     mkdir -p /home/docker_user
#RUN     echo "docker_user:x:${uid}:${gid}:docker_user,,,:/home/docker_user:/bin/bash" >> /etc/passwd
#RUN     echo "docker_user:x:${uid}:" >> /etc/group
#RUN     echo "docker_user ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/docker_user
#RUN     chmod 0440 /etc/sudoers.d/docker_user
#RUN     chown ${uid}:${gid} -R /home/docker_user 
#
#USER    docker_user 
#ENV     HOME /home/docker_user
