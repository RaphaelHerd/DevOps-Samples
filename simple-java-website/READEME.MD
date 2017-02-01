# Build a simple Spring Boot app with Docker

Required Software:
- Docker (apt-get install docker.io)
- JDK (apt-get install openjdk-7-jdk)
- Gradle (sudo apt-get install gradle)

### Spring Boot application details
The java file (entrypoint) can be found at following location "src/main/java/hello/Application.java"
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
Before we can start dockerizing the application, we need to build the Java application.
**$ ./gradlew build**
If the build is successful, we can check the installation locally by starting the jar-file.
**$ java -jar build/libs/spring-boot-docker-sample-0.1.0.jar**. 
Now the tomcat (webserver) should start and after a few secound try reach the website in web browser **http://localhost:8080**. You now should see the "Hello world!" message in web browser.

### Containerize the Spring Boot application
If the application is compiled successfull, we now can dockerize the application. We have two ways of how to dockerize a java-based application. 

Before we start to do that, have a look to the Dockerfile and understand whats happened in the file.
**$ nano src/main/docker/Dockerfile**. 
If you have a closer look to the file, what problem could exist by creating a container in that way (have a closer look to line 1 of the file)

We can use gradle the same way as it's used to build the application ore we can do that manually. In this example we are using the gradle way to show you how you will do that. Gradle in fact uses the Dockerfile we inspect before.

To build a docker image use
**$ ./gradlew buildDocker**. 

If you get an error you have to switch to root permission **sudo su** and reexecute the build command.

After the images is created we can list all docker images of your local machine by **$ docker images**. Try to find your image that is named as **itgo/spring-boot-docker-sample**.

If you found the image, we can start the docker container in appended mode first. This mode shows us all logs that are created while the container is started. After that we can start it in detatched mode. The **-p 8080:8080** part of the commands maps the target port 8080 to the destination port 8080.
**$ docker run -p 8080:8080 -ti itgo/spring-boot-docker-sample**. 

If the tomcat is started, we can browse the website by **http://localhost:8080**.

That's it. We now successfully dockerized a simple Spring Boot based web application.

### Docker in Depth
To get a better understanding how Docker is working, we now start the image in detached mode. Therefore exit all docker instances you are currenly running (close all terminals would also work). After that execute the previously shown command with -d for detatched.
**$ docker run -p 8080:8080 -d itgo/spring-boot-docker-sample**. 
If you execute the command, you can an ID as confirmation.

Check if the container is running by 
**$ docker ps -a**. 
Check if the STATUS column is not in Exited state. If the container is running, you can browse your website like described above. If you now want to have a look into the container we can connect to the instance. For that you need the value of the column CONTAINER ID of the instance of your conrainer by running docker ps -a command.

**$ docker exec -ti ID-OF-CONTAINER sh**. 

You now connected into the container and you can have a look to the disc by using **$ ls** or have a look to the running processes **$ ps aux**. In the root directory we also should find our "app.jar" which is our Spring Boot sample application.


