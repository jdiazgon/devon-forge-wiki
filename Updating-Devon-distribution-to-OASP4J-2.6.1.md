:toc: macro
toc::[]

= Templates
- Replace the delimiter value _<delimiter>^*^</delimiter>_ with _<delimiter>$[*]</delimiter>_ in file _workspaces/examples/oasp4j/templates/server/pom.xml_.
- Change property **oasp4j.version** from _2.6.0_ to _$[oasp4j.version]_ in file _workspaces/examples/oasp4j/templates/server/src/main/resources/archetype-resources/pom.xml_.

= Sample Application
- Add property **<flyway.version>4.2.0</flyway.version>** to file _workspaces/examples/oasp4j/samples/pom.xml_.
- In the same file i.e _workspaces/examples/oasp4j/samples/pom.xml_ add _flyway-core_ dependency given below under _dependency_management_ section. Note: It should be the first dependency under _dependency_management_.

`<dependency>
  <groupId>org.flywaydb</groupId>
  <artifactId>flyway-core</artifactId>
  <version>${flyway.version}</version>
dependency>`

- Remove version parameter for dependency _flyway-core_ in file _workspaces/examples/oasp4j/samples/core/pom.xml_.
- Add _flyway-core_ dependency below to _dependencies_ section of file _workspaces/examples/oasp4j/samples/server/pom.xml_
`<dependency>
   <groupId>org.flywaydb</groupId>
   <artifactId>flyway-core</artifactId>
 </dependency>`

= BOM Module
- Delete all files inside directory _workspaces/examples/oasp4j/bom_.
- Rename direectory _bom_ to _boms_.
- Create two new directories _bom_ and _minimal_ inside _boms_ directory.
- Create a new _pom.xml_ file inside _boms_ directory and add the code below
`<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>io.oasp.java.dev</groupId>
    <artifactId>oasp4j</artifactId>
    <version>dev-SNAPSHOT</version>
  </parent>
  <artifactId>oasp4j-boms</artifactId>
  <packaging>pom</packaging>
  <name>${project.artifactId}</name>
  <description>Bill of Materials (BOM) for the Open Application Standard Platform for Java (OASP4J).</description>

  <modules> <!-- Order is important here as maven does not track BOM imports as dependencies for reactor build order -->
    <module>minimal</module>
    <module>bom</module>
  </modules>

</project>`
- Create a new _pom.xml_ file inside _minimal_ directory and add the code below
`<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>io.oasp.java.dev</groupId>
    <artifactId>oasp4j-boms</artifactId>
    <version>dev-SNAPSHOT</version>
  </parent>
  <groupId>io.oasp.java.boms</groupId>
  <artifactId>oasp4j-minimal-bom</artifactId>
  <version>${oasp4j.version}</version>
  <packaging>pom</packaging>
  <name>${project.artifactId}</name>
  <description>Dependencies (BOM) of the Open Application Standard Platform for Java (OASP4J) without any thirdparty.</description>
  <url>http://oasp.io/oasp4j/</url>
  <inceptionYear>2014</inceptionYear>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <oasp.flatten.mode>bom</oasp.flatten.mode>
  </properties>

  <dependencyManagement>
    <dependencies>
      <!-- Modules -->
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-test</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-logging</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-basic</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-batch</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-beanmapping</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-configuration</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-security</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-service</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-json</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-rest</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-cxf-client</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-cxf-client-rest</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-cxf-client-ws</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-cxf-server</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-cxf-server-rest</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-cxf-server-ws</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-jpa</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-jpa-envers</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.modules</groupId>
        <artifactId>oasp4j-web</artifactId>
        <version>${project.version}</version>
      </dependency>
      <!-- Starters -->
      <dependency>
        <groupId>io.oasp.java.starters</groupId>
        <artifactId>oasp4j-starter-cxf-client</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.starters</groupId>
        <artifactId>oasp4j-starter-cxf-client-rest</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.starters</groupId>
        <artifactId>oasp4j-starter-cxf-client-ws</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.starters</groupId>
        <artifactId>oasp4j-starter-cxf-server</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.starters</groupId>
        <artifactId>oasp4j-starter-cxf-server-rest</artifactId>
        <version>${project.version}</version>
      </dependency>
      <dependency>
        <groupId>io.oasp.java.starters</groupId>
        <artifactId>oasp4j-starter-cxf-server-ws</artifactId>
        <version>${project.version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>

</project>`
- Create a new _pom.xml_ file inside _bom_ directory and add the code below
`<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>io.oasp.java.dev</groupId>
    <artifactId>oasp4j-boms</artifactId>
    <version>dev-SNAPSHOT</version>
  </parent>
  <groupId>io.oasp.java</groupId>
  <artifactId>oasp4j-bom</artifactId>
  <version>${oasp4j.version}</version>
  <packaging>pom</packaging>
  <name>${project.artifactId}</name>
  <description>Dependencies (BOM) of the Open Application Standard Platform for Java (OASP4J) based on spring boot.</description>
  <url>http://oasp.io/oasp4j/</url>
  <inceptionYear>2014</inceptionYear>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <cxf.version>3.1.9</cxf.version>
    <mmm.util.version>7.5.1</mmm.util.version>
    <oasp.flatten.mode>bom</oasp.flatten.mode>
  </properties>

  <dependencyManagement>
    <dependencies>
      <!-- *** INTERNAL DEPENDENCIES *** -->
      <dependency>
        <groupId>io.oasp.java.boms</groupId>
        <artifactId>oasp4j-minimal-bom</artifactId>
        <version>${oasp4j.version}</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!-- *** EXTERNAL DEPENDENCIES *** -->
      <!-- Library with general utilities as well as I18N and exception support -->
      <dependency>
        <groupId>net.sf.m-m-m</groupId>
        <artifactId>mmm-util-core</artifactId>
        <version>${mmm.util.version}</version>
      </dependency>
      <dependency>
        <groupId>net.sf.m-m-m</groupId>
        <artifactId>mmm-util-validation</artifactId>
        <version>${mmm.util.version}</version>
      </dependency>
      <dependency>
        <groupId>net.sf.m-m-m</groupId>
        <artifactId>mmm-util-search</artifactId>
        <version>${mmm.util.version}</version>
      </dependency>
      <dependency>
        <groupId>net.sf.m-m-m</groupId>
        <artifactId>mmm-util-entity</artifactId>
        <version>${mmm.util.version}</version>
      </dependency>
      <!-- JSR 250 for component/bean lifecycle management -->
      <dependency>
        <groupId>javax.annotation</groupId>
        <artifactId>jsr250-api</artifactId>
        <version>1.0</version>
      </dependency>
      <!-- JSR 330 for dependency injection -->
      <dependency>
        <groupId>javax.inject</groupId>
        <artifactId>javax.inject</artifactId>
        <version>1</version>
      </dependency>
      <!-- JSR 250 for common annotations (component-bean lifecycle, security, etc.) -->
      <dependency>
        <groupId>javax.annotation</groupId>
        <artifactId>javax.annotation-api</artifactId>
        <version>1.2</version>
      </dependency>
      <!-- Library with general utilities -->
      <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
        <version>17.0</version>
      </dependency>
      <!-- Library with advanced collection support -->
      <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-collections4</artifactId>
        <version>4.1</version>
      </dependency>
      <!-- Support for money datatype including operations/calculations -->
      <dependency>
        <groupId>org.javamoney</groupId>
        <artifactId>moneta</artifactId>
        <version>0.8</version>
      </dependency>
      <!-- Java Persistence API (2.x not available in groupId javax.persistence) -->
      <dependency>
        <groupId>org.hibernate.javax.persistence</groupId>
        <artifactId>hibernate-jpa-2.1-api</artifactId>
        <version>1.0.0.Final</version>
      </dependency>
      <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-envers</artifactId>
        <version>4.3.5.Final</version>
      </dependency>
      <dependency>
        <groupId>cglib</groupId>
        <artifactId>cglib</artifactId>
        <version>3.2.4</version>
      </dependency>
      <!-- Support for dynamic and type-safe JPA queries -->
      <dependency>
        <groupId>com.mysema.querydsl</groupId>
        <artifactId>querydsl-jpa</artifactId>
        <version>3.4.3</version>
      </dependency>
      <!-- DataBase Connection Pooling -->
      <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-dbcp2</artifactId>
        <version>2.0.1</version>
      </dependency>
      <!-- Logging API -->
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.7</version>
      </dependency>
      <!-- Servlet API -->
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
      </dependency>
      <!-- Servlet Taglib -->
      <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
      </dependency>
      <!-- JAX-RS API for REST services -->
      <dependency>
        <groupId>javax.ws.rs</groupId>
        <artifactId>javax.ws.rs-api</artifactId>
        <version>2.0.1</version>
      </dependency>
      <!-- CXF for REST and Webservices -->
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-core</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-frontend-jaxws</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-frontend-jaxrs</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-rs-service-description</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-transports-http</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-transports-local</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <dependency>
        <groupId>org.apache.cxf</groupId>
        <artifactId>cxf-rt-rs-client</artifactId>
        <version>${cxf.version}</version>
      </dependency>
      <!-- Jackson for JSON mapping -->
      <dependency>
        <groupId>com.fasterxml.jackson.jaxrs</groupId>
        <artifactId>jackson-jaxrs-json-provider</artifactId>
        <version>2.4.2</version>
      </dependency>
      <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.3.3</version>
      </dependency>
      <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>2.3.3</version>
      </dependency>
      <!-- Dozer for bean-mapping/conversion (Alternative to Orika) -->
      <dependency>
        <groupId>net.sf.dozer</groupId>
        <artifactId>dozer</artifactId>
        <version>5.5.1</version>
      </dependency>
      <!-- Orika for bean-mapping/conversion (Alternative to Dozer) -->
      <dependency>
        <groupId>ma.glasnost.orika</groupId>
        <artifactId>orika-core</artifactId>
        <version>1.4.6</version>
      </dependency>
      <!-- JSR303 API for annotation based validation -->
      <dependency>
        <groupId>javax.validation</groupId>
        <artifactId>validation-api</artifactId>
        <version>1.1.0.Final</version>
      </dependency>

      <!-- *** optional TEST DEPENDENCIES *** -->
      <!-- JUnit for Test-Case definition and execution -->
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
      </dependency>
      <!-- Mocking framework for mocks in unit tests -->
      <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-core</artifactId>
        <version>1.9.5</version>
      </dependency>
      <!-- Wiremock for mocking network communication -->
      <dependency>
        <groupId>com.github.tomakehurst</groupId>
        <artifactId>wiremock</artifactId>
        <version>1.54</version>
      </dependency>

      <!-- Dependencies for testing constraint validation -->
      <dependency>
        <groupId>javax.el</groupId>
        <artifactId>javax.el-api</artifactId>
        <version>2.2.4</version>
      </dependency>
      <dependency>
        <groupId>org.glassfish.web</groupId>
        <artifactId>javax.el</artifactId>
        <version>2.2.6</version>
      </dependency>

      <!-- Optional logging dependency -->
      <dependency>
        <groupId>org.owasp</groupId>
        <artifactId>security-logging-logback</artifactId>
        <version>1.1.3</version>
      </dependency>

    </dependencies>
  </dependencyManagement>

</project>`
- Change module name from _bom_ to _boms_ in file _workspaces/examples/oasp4j/pom.xml_

= Version
- Change value for property _oasp4j.version_ from _2.6.0_ to _2.6.1_ in file _workspaces/examples/oasp4j/pom.xml_

 
