# Copyright (C) 2018-2021 Intel Corporation
# All rights reserved.
#
# 'recipe' for Docker to build a base image of DAOS on CentOS8
#

# Pull base image
FROM	centos:8
LABEL	maintainer="daos@daos.groups.io"

# Configure systemd
# FIXME Shall be removed with application dedicated entry point
VOLUME	[ "/sys/fs/cgroup" ]
RUN	[ "systemctl", "mask", "dev-mqueue.mount", "dev-hugepages.mount",                          \
	  "systemd-remount-fs.service", "sys-kernel-config.mount", "sys-kernel-debug.mount",       \
	  "sys-fs-fuse-connections.mount", "graphical.target", "systemd-logind.service",           \
	  "NetworkManager.service", "systemd-hostnamed.service" ]
STOPSIGNAL SIGRTMIN+3
ENTRYPOINT [ "/sbin/init" ]

# Base configuration of dnf
RUN	dnf clean all &&                                                                           \
	dnf makecache &&                                                                           \
	dnf --assumeyes install dnf-plugins-core &&                                                \
	dnf config-manager --save --setopt=assumeyes=True &&                                       \
	dnf config-manager --save --setopt=fastestmirror=True &&                                   \
	dnf config-manager --set-enabled powertools &&                                             \
	dnf install epel-release

# Update system
RUN	dnf update

# XXX Passing random value to burst cache allow to build the image wihout using glolbal --no-cache
# XXX option and thus to not update all rpms.
ARG	BUST_CACHE
# FIXME Shall be repleced with 2.0 DAOS official repo when available
ARG	DAOS_REPOS="https://build.hpdd.intel.com/job/daos-stack/job/daos/job/master/lastSuccessfulBuild/artifact/artifacts/centos8/ https://repo.dc.hpdd.intel.com/repository/daos-stack-el-8-x86_64-stable-local"
RUN	[[ $BUST_CACHE ]] && dnf update ;                                                          \
	for repo in ${DAOS_REPOS} ; do                                                             \
		dnf config-manager --add-repo $repo ;                                              \
	done &&                                                                                    \
# FIXME Shall be replaced with import of the gpg key of the 2.0 DAOS official repo
	dnf config-manager --save --setopt=*hpdd*.gpgcheck=0

# Install daos pacakge
RUN	dnf install daos

# Generate GPG authentification certificates
# FIXME At the end certificate should be provided when the container are instantiated.
WORKDIR	/root
RUN	if [[ ! -d /etc/daos/certs ]] ; then                                                      \
		mkdir -d /etc/daos/certs &&                                                       \
		chown root:root /etc/daos/certs &&                                                \
		chmod 755 /etc/daos/certs ;                                                       \
	fi &&                                                                                     \
	/usr/lib64/daos/certgen/gen_certificates.sh
