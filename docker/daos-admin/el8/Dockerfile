# Copyright (C) 2018-2021 Intel Corporation
# All rights reserved.
#
# 'recipe' for Docker to build an image of centOS 8 admin docker image.
#

# Pull base image
ARG	DAOS_BASE_VERSION=latest
FROM	daos-base:$DAOS_BASE_VERSION
LABEL	maintainer="daos@daos.groups.io"

# Install certificates
ARG	DAOS_ADMIN_USER=root
ARG	DAOS_ADMIN_GROUP=root
# FIXME Should be provided through volumes (or Secrets for K8S)
RUN	chmod 644 /root/daosCA/certs/daosCA.crt &&                                                 \
	chown 644 /root/daosCA/certs/admin.crt &&                                                  \
	chown 600 /root/daosCA/certs/admin.key &&                                                  \
	chown $DAOS_ADMIN_USER:$DAOS_ADMIN_GROUP /root/daosCA/certs/daosCA.crt &                   \
	chown $DAOS_ADMIN_USER:$DAOS_ADMIN_GROUP /root/daosCA/certs/admin.crt &&                   \
	chown $DAOS_ADMIN_USER:$DAOS_ADMIN_GROUP /root/daosCA/certs/admin.key &&                   \
	mv /root/daosCA/certs/daosCA.crt /etc/daos/certs/. &&                                      \
	mv /root/daosCA/certs/admin.crt /etc/daos/certs/. &&                                       \
	mv /root/daosCA/certs/admin.key /etc/daos/certs/. &&                                       \
	rm -fr /root/daosCA
