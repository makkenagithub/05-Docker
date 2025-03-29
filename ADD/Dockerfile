#FROM is the first instruction
# we can use RHEL image but its licensed and not free. Hence we are using alternative almalinux from docker hub

FROM almalinux:9
RUN dnf install nginx -y
RUN rm -rf /usr/share/nginx/html/index.html
ADD https://github.com/makkenagithub/05-Docker/edit/main/README.md /usr/share/nginx/html/index.html
# below ADD instruction untars the tar file in destination location. But COPY instruction copies the tar file.
ADD sample.tar /tmp/
RUN chmod +x /usr/share/nginx/html/index.html
CMD ["nginx", "-g", "daemon off;"]


