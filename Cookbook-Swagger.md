
## Integrating Swagger in Devon

### Features
* Generation of Swagger JSON docs.
* Conversion of Swagger JSON docs to OpenAPI JSON.

### Configuration Steps 
* Add the following dependencies to project's `core` module pom.xml, here property `cxf.version` is set to **3.1.15**

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

* Mark REST services as a Swagger resource with `@Api` annotation as shown below
```
@Path("/general/v1")
@Api(value = "General Rest Server", consumes = MediaType.APPLICATION_JSON, produces = MediaType.APPLICATION_JSON)
@Consumes(MediaType.APPLICATION_JSON)
@Produces(MediaType.APPLICATION_JSON)
public interface GeneralRestService 
```

* Use Swagger annotations to describe REST method operations as shown below
```
@GET
@Path("/staffmember/{id}/")
@ApiOperation(value = "Finds Staff Member with Id - (version in URL)")
@ApiResponses(value = {
@ApiResponse(code = 200, message = "StaffMember resource found", response = StaffMemberEto.class),
@ApiResponse(code = 404, message = "StaffMember resource not found") })
public StaffMemberEto getStaffMember(@PathParam("id") long id);
```

* Configure CXF `Feature` bean as per project's REST service. Set `cxf.path` in `application.properties` file
```
@Value("${cxf.path}")
private String basePath;

@Bean("swagger2Feature")
public Feature swagger2Feature() {
  Swagger2Feature result = new Swagger2Feature();
  result.setTitle("Demo for integration of Devon application wtih Swagger");
  result.setDescription("This is a demo for integration of Swagger into a Devon application using CXF. Additionally, it " + " has been configured to convert Swagger JSON produced by CXF Swagger2Feature into Open API JSON");
  result.setBasePath(this.basePath);
  result.setVersion("v1");
  result.setContact("Abhay Chandel");
  result.setSchemes(new String[] { "http", "https" });
  return result;
}
```

* Create a JAXRS `Server` bean. Autowire `Swagger2Feature` bean configured in previous step and CFX `Bus`. And pass them to the server bean. 
```
@Autowired
private Bus bus;

@Autowired
private Feature swagger2Feature;

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

     https://github.com/devonfw/devonfw-swagger-gen-demo

Swagger JSON Doc can be accessed at

     http://localhost:8081/services/api-docs?url=/services/swagger.json
     
Open API JSON Doc can be accessed at

     http://localhost:8081/services/api-docs?url=/services/openapi.json

