# this file originally from: https://github.com/pwntr/samba-alpine-docker/blob/master/Dockerfile
FROM alpine:latest
MAINTAINER Brenton Morris
LABEL Description="Simple and lightweight Samba docker container, based on Alpine Linux." Version="0.1"

# update the base system
RUN apk update && apk upgrade

# install samba and supervisord and clear the cache afterwards
RUN apk add samba samba-common-tools supervisor && rm -rf /var/cache/apk/*

# create a dir for the config and the share
RUN mkdir /config /shared

# copy config files from project folder to get a default config going for samba and supervisord
COPY *.conf /config/

# # add a non-root user and group called "user" with no password, no home dir, no shell, and gid/uid set to 1000
# RUN addgroup -g 1000 user && adduser -D -H -G user -s /bin/false -u 1000 user

# create a samba user matching our user from above with a very simple password ("letsdance")
RUN echo -e "letsdance\nletsdance" | smbpasswd -a -s -c /config/smb.conf
#user

# volume mappings
VOLUME /config /shared

# exposes samba's default ports (137, 138 for nmbd and 139, 445 for smbd)
EXPOSE 137/udp 138/udp 139 445

ENTRYPOINT ["supervisord", "-c", "/config/supervisord.conf"]
