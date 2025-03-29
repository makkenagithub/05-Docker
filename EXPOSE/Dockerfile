#FROM is the first instruction
# we can use RHEL image but its licensed and not free. Hence we are using alternative almalinux from docker hub

FROM almalinux:9
# install nginx 
RUN dnf install nginx -y
#to run the command in a container. It starts nginx in the container in foreground
CMD ["nginx", "-g", "daemon off;"]
#only one CMD command can exists in a dockerfile. If we give multiple CMDs then the last CMD will be executed.
EXPOSE 80
