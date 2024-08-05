# Spring Cloud Config Overview

❇️ Spring Cloud Config is a powerful tool within the Spring ecosystem designed to manage external configuration for applications across all environments. It provides server and client support for managing configuration data outside of your application code.

## Key Features

🔹<b>Centralized Configuration Management:</b> Manage configurations for multiple applications from a central location. </br>
🔹<b>Environment-Specific Configurations:</b> Customize configurations for different environments such as development, testing, staging, and production.</br>
🔹 <b>Dynamic Configuration Updates:</b> Refresh configurations without restarting your applications.</br>
🔹<b>Integration with Version Control:</b> Store configurations in a version control system like Git for versioning and history tracking.

### Architecture

🔹<b>Config Server:</b> A central server that manages configuration properties for applications.</br>
🔹<b>Config Client:</b> Applications that consume configuration properties from the Config Server.

### Setting Up Spring Cloud Config

<b>1. Config Server</b>

<b>1.1 Create a Spring Boot Application:</b>
Create a new Spring Boot application for the Config Server.
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
</dependency>
```

<b>1.2 Enable Config Server:</b>
Annotate the main application class with @EnableConfigServer.

```
@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServerApplication.class, args);
    }
}
```

<b>1.3 Configure Properties:</b> Add configuration properties in application.properties or application.yml.
```
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/yourusername/config-repo
```

<b>2. Config Client</b>

<b>2.1 Create a Spring Boot Application:</b>
Create a new Spring Boot application for the client.
```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

<b>2.2 Configure Properties:</b>
Add configuration properties in bootstrap.properties or bootstrap.yml.
```
spring.application.name=your-client-app
spring.cloud.config.uri=http://localhost:8888
```

<b>2.3 Accessing Configuration Properties:</b> </br>
Use @Value or @ConfigurationProperties to access configuration properties in your application.
```
@RestController
public class ConfigClientController {

    @Value("${config.property.name}")
    private String configProperty;

    @GetMapping("/config")
    public String getConfigProperty() {
        return configProperty;
    }
}
```

### Storing Configurations in Git

<b>1. Create a Git Repository:</b>
Create a repository to store your configuration files.

<b>2. Add Configuration Files:</b></br>
Add environment-specific configuration files, e.g., application.yml, application-dev.yml, application-prod.yml.
```
# application.yml
config:
  property:
    name: DefaultConfig

# application-dev.yml
config:
  property:
    name: DevConfig

# application-prod.yml
config:
  property:
    name: ProdConfig
```


### Refreshing Configurations

<b>1. Actuator Dependency:</b>
Add Spring Boot Actuator to the client application.
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

<b>2. Enable Refresh Endpoint:</b></br>
Enable the refresh endpoint in application.properties.
```
management.endpoints.web.exposure.include=refresh
```

<b>3. Trigger Refresh:</b></br>
Use the following endpoint to refresh the configuration without restarting the application.
```
POST http://localhost:8080/actuator/refresh
```

### Conclusion

Spring Cloud Config provides a robust solution for managing configuration across multiple environments and applications. By centralizing configuration management and leveraging version control, it ensures consistency and simplifies the process of managing application settings. 
