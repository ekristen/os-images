FROM rancher/os-fedoraconsole-base
# FROM amd64=fedora:23 arm64=rancher/aarch64-fedora:23 arm=skip
RUN dnf upgrade -y && \
    dnf install -y iptables openssh-server rsync sudo vim less ca-certificates psmisc htop procps-ng
RUN rm -rf /etc/ssh/*key*
RUN rm -fr /sbin/poweroff  /sbin/shutdown /sbin/reboot /sbin/halt /usr/sbin/poweroff  /usr/sbin/shutdown /usr/sbin/reboot /usr/sbin/halt
RUN ln -s /sbin/agetty /sbin/getty
COPY build/update-ssh-keys build/rancheros-install /usr/sbin/
RUN localedef -c -f UTF-8 -i en_US en_US.UTF-8
RUN groupadd --gid 1100 rancher && \
    groupadd --gid 1101 docker && \
    useradd -u 1100 -g rancher -G docker,wheel -m -s /bin/bash rancher && \
    useradd -u 1101 -g docker -G docker,wheel -m -s /bin/bash docker && \
    echo ClientAliveInterval 180 >> /etc/ssh/sshd_config && \
    echo '## allow password less for rancher user' >> /etc/sudoers && \
    echo 'rancher ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    echo '## allow password less for docker user' >> /etc/sudoers && \
    echo 'docker ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers && \
    ln -sf /usr/bin/docker.dist /usr/bin/docker && \
    ln -sf /usr/bin/docker-containerd.dist /usr/bin/docker-containerd && \
    ln -sf /usr/bin/docker-containerd-shim.dist /usr/bin/docker-containerd-shim && \
    ln -sf /usr/bin/docker-runc.dist /usr/bin/docker-runc
COPY prompt.sh /etc/profile.d/
ENTRYPOINT ["/usr/bin/ros", "entrypoint"]
CMD ["/usr/sbin/console.sh"]
