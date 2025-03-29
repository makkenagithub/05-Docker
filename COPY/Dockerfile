#FROM is the first instruction
# we can use RHEL image but its licensed and not free. Hence we are using alternative almalinux from docker hub

FROM almalinux:9
RUN dnf install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
COPY index.html /usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]


