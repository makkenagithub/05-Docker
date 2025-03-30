# we can use ARG as first instruction. This is exception. For eg:
# # docker build -t image:v1 --build-arg linux_version=9 .
ARG linux_version
FROM almalinux:{$linux_version:-9} # if the value is not passed during build time, we can give default value like this :-9 
ARG cource="devops" \
    author="suresh"
# ARG is only accessed at image build time.
RUN echo "course is : $cource and author is : $author " # during 'docker build -t image:v1' it will print this message.

# we can pass the ARG values at the time of build also , we can overwrite default values with command 
# docker build -t image:v1 --build-arg var1=suresh --build-arg var2=30 .
ARG var1
ARG var2="2"

# we canuse ARG value inside container as below, with the help of ENV
ARG name="suresh"
ENV name=$name

CMD ["sleep", "100"]

