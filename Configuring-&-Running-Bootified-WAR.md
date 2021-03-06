
1. Integrate Server application with client(Angular/Sencha) application as per the instructions given below.
   * https://github.com/oasp/oasp4js/wiki/tutorial-jspacking-angular-client
   * https://github.com/devonfw/devon-guide/wiki/getting-started-deployment-on-tomcat

2. Comment out the following configuration in _BaseWebSecurityConfig.java_
```
/*.formLogin().successHandler(new SimpleUrlAuthenticationSuccessHandler()).defaultSuccessUrl("/")
.failureUrl("/login.html?error").loginProcessingUrl("/j_spring_security_login").usernameParameter("username")
.passwordParameter("password").and() // logout via POST is possible
.logout().logoutSuccessUrl("/login.html").and()*/
```

3. Exclude static content from core module by adding following line to build section of _pom.xml_
```xml    
   <exclude>static/*.html</exclude>
```
4. Add redirection to client application from server context by adding following method to _LoginController.java_ of core module.
```
    @RequestMapping(value = "/", method = RequestMethod.GET)    
      public ModelAndView getJSClientHome() {

        return new ModelAndView("redirect:/jsclient/index.html");
    }
```

5. Allow access to application context root by adding **"/"** to unsecured URLs in _BaseWebSecurityConfig.java_.

6. Replace _targetPath_ in Server module build with code below

```xml
    <targetPath>static/jsclient</targetPath>
```

7. Set Application context path to **_oasp4j-sample-server_** in _/oasp4j-sample-core/src/main/resources/application.properties_ and _/oasp4j-sample-core/src/test/resources/config/application.properties_ like below
```
    server.context-path=/oasp4j-sample-server
```
8. Access application on following link.

   >http://localhost:8080/oasp4j-sample-server