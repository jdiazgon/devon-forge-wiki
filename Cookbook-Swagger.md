
## Integrating Swagger in Devon

### Features
* Generation of Swagger JSON docs.
* Conversion of Swagger JSON docs to OpenAPI JSON.

### Setup 
* Add the following dependencies to project's pom.xml, here property `cxf.version` is set to **3.1.5**

```xml
<dependency>
      <groupId>com.google.guava</groupId>
      <artifactId>guava</artifactId>
      <version>20.0</version>
</dependency>


<dependency>
      <groupId>io.swagger</groupId>
      <artifactId>swagger-jaxrs</artifactId>
      <version>1.5.14</version>
</dependency>

<dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-service-description-swagger</artifactId>
      <version>${cxf.version}</version>
</dependency>

<dependency>
      <groupId>org.webjars</groupId>
      <artifactId>swagger-ui</artifactId>
      <version>3.10.0</version>
</dependency>

<dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-json-basic</artifactId>
      <version>${cxf.version}</version>
      <scope>provided</scope>
</dependency>
```
* Add CXF feature bean below. Set `cxf.path` in `application.properties` file
```
@Value("${cxf.path}")
private String basePath;

  @Bean("swagger2Feature")
  public Feature swagger2Feature() {
  Swagger2Feature result = new Swagger2Feature();
  result.setTitle("Demo for integration of Devon application wtih Swagger");
  result.setDescription(
        "This is a demo for integration of Swagger into a Devon application using CXF. Additionally, it has been configured" + " to convert Swagger JSON produced by CXF Swagger2Feature into Open API JSON");
  result.setBasePath(this.basePath);
  result.setVersion("v1");
  result.setContact("Abhay Chandel");
  result.setSchemes(new String[] { "http", "https" });
  return result;
}
```

* Create a JAXRS server bean
```
@Bean
public Server rsServer() {

  JAXRSServerFactoryBean endpoint = new JAXRSServerFactoryBean();
  endpoint.setBus(this.bus);
  endpoint.setServiceBeans(Arrays.<Object> asList(new GeneralRestServiceImpl()));
  endpoint.setAddress("/");
  endpoint.setFeatures(Arrays.asList(this.swagger2Feature));
  endpoint.setProvider(new SwaggerToOpenApiConversionFilter());
  return endpoint.create();
}
```
### You can find the complete source code for this demo at repo below

     https://github.com/AbhayChandel/devon-with-swagger

Swagger JSON Doc can be accessed at

     http://localhost:8081/services/api-docs?url=/services/swagger.json
     
Open API JSON Doc can be accessed at

     http://localhost:8081/services/api-docs?url=/services/openapi.json

