FROM almalinux:9
# install nginx 
RUN dnf install nginx -y
RUN useradd expense
CMD ["nginx", "-g", "daemon off;"]
# above all instructions run with root user
# below all instructionsrun with expense user
USER expense
