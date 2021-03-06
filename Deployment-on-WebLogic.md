Make the following changes to OASP4J Server-client application for deployment on WebLogic

1. Create an empty **_weblogic.xml_** file under directory _oasp4j/samples/server/src/main/webapp/WEB-INF/_ and paste the code below into it. _weblogic.xml_  set application deployment configurations for WebLogic server.


`<weblogic-web-app xmlns="http://xmlns.oracle.com/weblogic/weblogic-web-app" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.oracle.com/weblogic/weblogic-web-app 
               http://xmlns.oracle.com/weblogic/weblogic-web-app/1.7/weblogic-web-app.xsd"></weblogic-web-app>`


2. Define OSP4J application libraries and resources that should be given preference over WebLogic server libraries in _weblogic.xml_ under **_prefer-application-packages_**. 
`<weblogic-web-app xmlns="http://xmlns.oracle.com/weblogic/weblogic-web-app" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://xmlns.oracle.com/weblogic/weblogic-web-app 
               http://xmlns.oracle.com/weblogic/weblogic-web-app/1.7/weblogic-web-app.xsd">
  
  <container-descriptor>
  
    <prefer-application-packages>
      <package-name>org.slf4j</package-name>
      <package-name>org.jboss.logging.*</package-name>
      <package-name>javax.ws.rs.*</package-name>
      <package-name>com.fasterxml.jackson.*</package-name>
      <package-name>org.apache.neethi.*</package-name>
    </prefer-application-packages>
    <prefer-application-resources>
      <resource-name>org/jboss/logging/Logger.class</resource-name>
      <resource-name>META-INF/services/*</resource-name>
    </prefer-application-resources>
  </container-descriptor>
  
</weblogic-web-app>`

3. To avoid multiple logging libraries exclude _logback-classic_ dependency from _spring-boot-starter-web_ 


<dependency>

      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <exclusions>
        <exclusion>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-validation</artifactId>
        </exclusion>
        <exclusion>
          <groupId>ch.qos.logback</groupId>
          <artifactId>logback-classic</artifactId>
        </exclusion>
</dependency>

4. Resolve unique REST method error on WebLogic by changing the _@Path_ as given below.
   * Change URL from _@Path("/bill/{billId}/payment")_ to _@Path("/bill/{billId}/paymentdata")_ for method _doPayment(@PathParam("billId") long billId, PaymentData paymentData)_ in _SalesmanagementRestService.java_.
   * Change URL from _@Path("/table/")_ to _@Path("/createtable/")_ for method _TableEto createTable(TableEto table)_ in `TablemanagementRestService.java`

### Sencha

5. Enable CORS.
   * Set _corsEnabled_ to _true_ in _BaseWebSecurityConfig_.
   * Set _security.cors.enabled_ to _true_ in _/oasp4j-sample-core/src/main/resources/application.properties_.
6. Change server details in _ExtSample/app/Config.js_ accroding to WebLogic server.

### Packaging

1. Package your application by Executing the command below from within oasp4j/samples project

   > mvn clean package -P-embedded,jsclient