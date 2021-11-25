# Copyright (C) 2018-2021 Intel Corporation
# All rights reserved.
#
# 'recipe' for Docker to build an image of centOS 8 client docker image.
#

# Pull base image
ARG	DAOS_BASE_VERSION=latest
FROM	daos-base:$DAOS_BASE_VERSION
LABEL	maintainer="daos@daos.groups.io"

# Install DAOS client package
RUN	dnf install daos-client

# Install certificates
ARG	DAOS_ADMIN_USER=root
ARG	DAOS_ADMIN_GROUP=root
# FIXME Should be provided through volumes (or Secrets for K8S)
RUN	chmod 644 /root/daosCA/certs/daosCA.crt &&                                                 \
	chown 644 /root/daosCA/certs/agent.crt &&                                                  \
	chown 600 /root/daosCA/certs/agent.key &&                                                  \
	chown $DAOS_ADMIN_USER:$DAOS_ADMIN_GROUP /root/daosCA/certs/daosCA.crt &&                  \
	chown daos_agent:daos_agent /root/daosCA/certs/agent.crt &&                                \
	chown daos_agent:daos_agent /root/daosCA/certs/agent.key &&                                \
	mv /root/daosCA/certs/daosCA.crt /etc/daos/certs/. &&                                      \
	mv /root/daosCA/certs/agent.crt /etc/daos/certs/. &&                                       \
	mv /root/daosCA/certs/agent.key /etc/daos/certs/. &&                                       \
	rm -fr /root/daosCA
