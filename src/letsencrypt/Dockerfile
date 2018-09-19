FROM nginx:latest

ARG BUILD_DATE
ARG VCS_REF
ARG VIRTUAL_HOST

# The maintainer list
LABEL authors="Milton Leon <miltonleon86@gmail.com>"
LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.vcs-ref=$VCS_REF

COPY nginx.conf /etc/nginx/conf.d/website.conf

# 1. replace the comma with a space to insert into the nginx config
# 2. replace the template parameter into the file
# it's important to run both of these lines together in a single RUN statement
RUN VIRTUAL_HOST=$(echo ${VIRTUAL_HOST} | sed "s/,/ /g"); \
    sed -i "s/{{ VIRTUAL_HOST }}/${VIRTUAL_HOST}/g" /etc/nginx/conf.d/website.conf

# just for debugging, you can delete it if you're happy with the results
RUN cat /etc/nginx/conf.d/website.conf

EXPOSE 80
