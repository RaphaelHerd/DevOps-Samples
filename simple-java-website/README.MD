# Build a simple Spring Boot app with Docker

Required Software:
- Docker (apt-get install docker.io)
- JDK (apt-get install openjdk-7-jdk)
- Gradle (sudo apt-get install gradle)

### Spring Boot application details
The java file (entry point) can be found at the following location "src/main/java/hello/Application.java"
```
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.bind.RelaxedPropertyResolver;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class Application {

        @RequestMapping("/")
        public String home() {
                return "Hello Docker World";
        }


        public static void main(String[] args) {
                SpringApplication.run(Application.class, args);
        }
}
```
The class is flagged as a @SpringBootApplication and as a @RestController, meaning it’s ready for use by Spring MVC to handle web requests. @RequestMapping maps / to the home() method which just sends a response back. The main() method uses Spring Boot’s SpringApplication.run() method to launch an application.

### Build the application
Before we can start dockerizing the application, we fist need to build the Java application. We can build the application by using Gradle.
**$ ./gradlew build**
If the build is successful, we can check the installation locally by starting the jar-file manually.
**$ java -jar build/libs/spring-boot-docker-sample-0.1.0.jar**. 
Now the tomcat (webserver) should start and after a few seconds try entering the website in web browser **http://localhost:8080**. You now should see the "Hello world!" message in web browser.

### Containerize the Spring Boot application
If the application is compiled successful, we now can dockerize the application. We have two ways of how to dockerize a java-based application. 

Before we can start doing that, we need to have a look to the Dockerfile and understand what’s happened in the file.
**$ nano src/main/docker/Dockerfile**. 
If you have a closer look to the file, what problem could exist by creating a container in that way (have a closer look to line 1 of the file).

We can use Gradle the same way as it's used to build the application or we can do that manually. In this example we are using the Gradle way to show you how you will do that. Gradle in fact uses the Dockerfile we inspect before.

To build the docker image use
**$ ./gradlew buildDocker**. 

If you get an error you have to switch to root permission **sudo su** and re-execute the build command. The reason for that is because sub tasks are executed while building the docker image and therefore you may need root permissions to get access to your local docker.

After the images is created we can list all docker images of your local machine by **$ docker images**. Try to find your image that is named as **itgo/spring-boot-docker-sample**.

If you found the image, we can start the docker container in interactive mode (**-ti**) first. This mode shows us all logs that are created while the container is starting and even while the container is running. After that we can start it in detached mode (**-d**). The **-p 8080:8080** part of the command maps the target port 8080 to the destination port 8080.
**$ docker run -p 8080:8080 -ti itgo/spring-boot-docker-sample**. 

If the tomcat is started, we can browse the website by entering **http://localhost:8080** in the web browser.

That's it. We now successfully dockerized a simple Spring Boot based web application.

### Docker in Depth
To get a better understanding how Docker is working, we now start the image in detached mode. Therefore exit all docker instances you are currently running (close all terminals would also work). After that execute the previously shown command with -d instead of -ti for detached.
**$ docker run -p 8080:8080 -d itgo/spring-boot-docker-sample**. 
If you execute the command, you get an ID as confirmation.

Check if the container is running by 
**$ docker ps -a**. 
Check if the STATUS column is not in Exited state. If the container is running, you can enter your website in web browser like described above. If you now want to have a look into the container we can directly connect to the instance. For that you need the value of the column CONTAINER ID of the instance of your container by running docker ps -a command.

**$ docker exec -ti CONTAINER-ID sh**. 

You now connected into the container and you can have a look to the file system by using **$ ls** or have a look to the running processes **$ ps aux**. In the root directory we also should find our "app.jar" which is our Spring Boot sample application.

If you are finish inspecting the container, you can leave the container by **$ exit**. If you now use **$docker ps -a** your instance is in Exit state. You can clean the instance by removing it from the instance list **$ docker rm CONTAINER-ID**. 

If you want to delete the image as well, you can do that by **$ docker rmi IMAGE-NAME**. To get the name, you can list all images by **$ docker images**.
