## Build a simple Spring Boot app with Docker

Required Software:
- Docker (apt-get install docker.io)
- JDK (apt-get install openjdk-7-jdk)
- Gradle (sudo apt-get install gradle)

# Spring Boot application details
The java file (entrypoint) can be found at following location "src/main/java/hello/Application.java"
´´´
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
´´´
