# Copyright (C) 2018-2021 Intel Corporation
# All rights reserved.
#
# 'recipe' for Docker to build an image of centOS 8 server docker image.
#

# Pull base image
ARG	DAOS_BASE_VERSION=latest
FROM	daos-base:$DAOS_BASE_VERSION
LABEL	maintainer="daos@daos.groups.io"

# Install DAOS client package
RUN	dnf install daos-server &&                                                                \
	systemctl enable daos_server
# Configuration of the server
# FIXME Should be done externally and provided through volumes (or ConfigMaps for K8S)
COPY	daos_server.yml /etc/daos/daos_server.yml

# Install certificates
# FIXME Should be provided through volumes (or Secrets for K8S)
RUN	chmod 644 /root/daosCA/certs/daosCA.crt &&                                                 \
	chmod 644 /root/daosCA/certs/server.crt &&                                                 \
	chown 600 /root/daosCA/certs/server.key &&                                                 \
	chown 644 /root/daosCA/certs/agent.crt &&                                                  \
	chown daos_server:daos_server /root/daosCA/certs/daosCA.crt &&                             \
	chown daos_server:daos_server /root/daosCA/certs/server.crt &&                             \
	chown daos_server:daos_server /root/daosCA/certs/server.key &&                             \
	chown daos_server:daos_server /root/daosCA/certs/agent.crt &&                              \
	mv /root/daosCA/certs/daosCA.crt /etc/daos/certs/. &&                                      \
	mv /root/daosCA/certs/server.crt /etc/daos/certs/. &&                                      \
	mv /root/daosCA/certs/server.key /etc/daos/certs/. &&                                      \
	mv /root/daosCA/certs/agent.crt /etc/daos/certs/clients/. &&                               \
	rm -fr /root/daosCA
