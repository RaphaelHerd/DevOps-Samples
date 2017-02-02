# Use openjdk:jre-alpine as base image that is a minimal linux with java runtime included
FROM openjdk:jre-alpine

# copy compiled class to the image
COPY HelloWorldApp.class /opt/app/

# set the location where the file is copied to as working directory for execution
WORKDIR /opt/app

# execute "java HelloWorldApp" on console if the container is stared
CMD ["java", "HelloWorldApp"]

