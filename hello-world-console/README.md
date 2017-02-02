# Docker java 7 example hello world console application

This application is a java minimal application that only prints out "Hello world!" to the console.

To get the application up and running we first need to build/compile the application by the javac compiler. If the application is compiled, we can execute the application without docker for test purposes. After that we dockerize the application followed by execute the application in docker.

We Require:
* [Docker](https://www.docker.com/) (apt-get install docker.io)
* Open-JDK (to compile java file locally) (apt-get install openjdk-7-jdk)

To run:

```
# compile class
$ javac helloworld.java

# test the compiled application
$ java HelloWorldApp 

# if the application prints our "Hello World!" to the console, we now can dockerize the application.

# build docker image
$ docker build -t java-hello-world .

# if the image is created we can list all installed images on our machine
$ docker images

# if the image named "java-hello-world" is in the list of available images, we can run the image
$ docker run -it --rm java-hello-world

# on the console, we now should see the "Hello World!" as well
```
