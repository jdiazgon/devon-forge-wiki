Spring Boot Admin is an application to manage and monitor your [[ Spring Boot Applications|http://projects.spring.io/spring-boot/]]. The applications register with our Spring Boot Admin Client (via HTTP) or are discovered using Spring Cloud (e.g. Eureka). The UI is just an AngularJs application on top of the Spring Boot Actuator endpoints.  

## Configure the Spring boot admin for the OASP4J App.  
  
 ### Setting up Spring boot Admin server
  To run the spring boot admin. First, you need to setup admin server. To do this create the [[ spring.io|http://start.spring.io]] project and follow the below steps.  

1. Add Spring Boot Admin Server and the UI dependency to the pom.xml file. 
````Xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-server</artifactId>
    <version>1.5.3</version>
</dependency>
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-server-ui</artifactId>
    <version>1.5.3</version>
</dependency>
````
  

2. Add the Spring Boot Admin Server configuration via adding @EnableAdminServer to your spring boot class.

````java

@Configuration
@EnableAutoConfiguration
@EnableAdminServer
public class SpringBootApp{
    public static void main(String[] args) {
        SpringApplication.run(SpringBootAdminApplication.class, args);
    }
}
```` 
3. If you want the login for the sever , then add the login depedency . 

````Xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-server-ui-login</artifactId>
    <version>1.5.3</version>
</dependency>

````

4. Add the properties to the application.properties file. 
 
````java
spring.application.name=Admin-Application
server.port=1111

management.security.enabled=false
security.user.name=admin
security.user.password=admin123
````

5. Securing Spring Boot Admin Server

Since there are several approaches on solving authentication and authorization in distributed web applications Spring Boot Admin doesn’t ship a default one. If you include the spring-boot-admin-server-ui-login in your dependencies it will provide a login page and a logout button.

Add the below configuration. 

````Java
@Configuration
  public static class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
      // Page with login form is served as /login.html and does a POST on /login
      http.formLogin().loginPage("/login.html").loginProcessingUrl("/login").permitAll();
      // The UI does a POST on /logout on logout
      http.logout().logoutUrl("/logout");
      // The ui currently doesn't support csrf
      http.csrf().disable();

      // Requests for the login page and the static assets are allowed
      http.authorizeRequests()
          .antMatchers("/login.html", "/**/*.css", "/img/**", "/third-party/**")
          .permitAll();
      // ... and any other request needs to be authorized
      http.authorizeRequests().antMatchers("/**").authenticated();

      // Enable so that the clients can authenticate via HTTP basic for registering
      http.httpBasic();
    }
  }
````
 Below is the screenshot of the Admin Server UI.

[[images/springbootadmin/springbootadminserver.PNG]]

### Register the client app.

Spring boot admin gives the monitoring status of multiple [[ spring.io|http://start.spring.io]] application.These applications are registered as the client application to spring boot admin server.You can register the application with the spring-boot-admin-client or  use [[ Spring Cloud Discovery|http://projects.spring.io/spring-cloud/spring-cloud.html]] (e.g. Eureka, Consul, …).    

#### Register with spring-boot-admin-starter-client  

1. Add spring-boot-admin-starter-client dependency to the pom.xml file. 

````XML
<dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-client</artifactId>
      <version>1.5.3</version>
</dependency>

````
2. Enable the SBA Client by configuring the URL of the Spring Boot Admin Server:

````java
spring.boot.admin.url= http://localhost:8080  
management.security.enabled= false 
````

#### Register with Spring Cloud Discovery

If you already use Spring Cloud Discovery for your applications you don’t need the SBA Client. Just make the Spring Boot Admin Server a DiscoveryClient, the rest is done by our AutoConfiguration.

The following steps are for using Eureka, but other Spring Cloud Discovery implementations are supported as well. There are examples using [[ Consul |https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-consul/]] and [[ Zookeeper |https://github.com/codecentric/spring-boot-admin/tree/master/spring-boot-admin-samples/spring-boot-admin-sample-zookeeper/]].

1. Add spring-cloud-starter-eureka dependency to the pom.xml file. 

````Xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-eureka</artifactId>
</dependency>
````
2. Add the Spring Boot Admin Server configuration via adding @EnableDiscoveryClient to your spring boot class.

````java
@Configuration
@EnableAutoConfiguration
@EnableDiscoveryClient
@EnableAdminServer
public class SpringBootApp {

  /**
   * Entry point for spring-boot based app
   *
   * @param args - arguments
   */
  public static void main(String[] args) {

    SpringApplication.run(SpringBootApp.class, args);

  }

}
````
3. Add the properties to the application.properties file. 

````java


eureka.client.serviceUrl.defaultZone=${EUREKA_URI:http://localhost:8180/eureka}
spring.boot.admin.url=http://localhost:1111
management.security.enabled=false
spring.boot.admin.username=admin
spring.boot.admin.password=admin123
logging.file=target/${spring.application.name}.log

eureka.instance.hostname=localhost
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false

health.config.enabled=true 
````
Detailed view of an application is given below. In this view we can see the tail of the log file, metrics, environment variables, log configuration where we can dynamically switch the log levels at the component level, root level or package level and other information.

[[images/springbootadmin/Springbootclient.PNG]]

### Loglevel management

For applications using Spring Boot 1.5.x (or later) you can manage loglevels out-of-the-box. For applications using older versions of Spring Boot the loglevel management is only available for [[ Logback|https://logback.qos.ch/]]. It is accessed via JMX so include Jolokia in your application. In addition you have configure Logback’s JMXConfigurator:

1. Add dependency. 

````XML
<dependency>
    <groupId>org.jolokia</groupId>
    <artifactId>jolokia-core</artifactId>
</dependency>
````
2. Add the logback-spring.xml file in resorce folder. 

````XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<include resource="org/springframework/boot/logging/logback/base.xml"/>
	<jmxConfigurator/>
</configuration>
````
[[images/springbootadmin/Logging.PNG]]

### Notification

Now we will see another feature called notifications from Spring Boot Admin. This will notify the administrators when the application status is  DOWN or an application status is coming UP. Spring Boot admin supports the below channels to notify the user.

* Email Notifications
* Pagerduty Notifications
* Hipchat Notifications
* Slack Notifications
* Let’s Chat Notifications

Here, we will configure Slack notifications. Add the below properties to the Spring Boot Admin Server’s application.properties file.To enable Slack notifications you need to add an incoming Webhook under custom integrations on your Slack account and configure it appropriately.

````java
spring.boot.admin.notify.slack.enabled=true
spring.boot.admin.notify.slack.username=user123
spring.boot.admin.notify.slack.channel=general
spring.boot.admin.notify.slack.webhook-url=https://hooks.slack.com/services/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
spring.boot.admin.notify.slack.message="#{application.name} (#{application.id}) is #{to.status}"

````


