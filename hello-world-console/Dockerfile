# Use openjdk:alpine as base image that is a minimal linux with java included
FROM openjdk:alpine

# copy compiled class to the image
COPY HelloWorldApp.class /opt/app/

# set the location where the file is copied to as working directory for execution
WORKDIR /opt/app

# execute "java HelloWorldApp" on console if the container is stared
CMD ["java", "HelloWorldApp"]

