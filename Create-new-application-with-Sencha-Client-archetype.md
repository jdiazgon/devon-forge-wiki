### Getting the archetype
Download the archetype from [here](https://github.com/devonfw/oasp4j-template-client-server) and save it to Devonfw distribution's _workspaces/main_ folder.

### Installing the archetype
Open the Devonfw console by executing the batch file console.bat from the Devonfw distribution. It is a pre-configured console which automatically uses the software and configuration provided by the Devonfw distribution.  

In the console, Go to  

_oasp4j-template-client-server/templates_ 

and run the following command to install the archetpye  

_mvn clean install_

If everything goes fine, you'll see the results in the console as below

![Archetype Installation Sucessful](images/devon4sencha/sencha-client-archetype/archetype_install_success.png)

### Create project
In Eclipse, go to _File_ --> _New_ --> _Maven Project_.  
A dialog box will open, click _Next_. On this screen, select checkbox _Include snapshot archetype_ and search for Artifact Id _**oasp4j-template-client-server**_. Select the archetype and click _Next_.

![Archetype Search](images/devon4sencha/sencha-client-archetype/archetype_search.png)

Enter _Group Id_ and _Artifact Id_ for your project and click _Finish_.  

A new project will be created in Eclipse with structure as shown below

![Project Structure](images/devon4sencha/sencha-client-archetype/project_modules.png)


### Setup Client module  

**Disable Javascript file Validation on Build**

By Default, auto validation of Javascript files is enabled in Eclipse. For client module it is time saving to set it to manual since it will take long time to validate all javascript files, every time build is triggered. To do the same follow the steps below.

1. Right click the client module(in our example it is SenchaServerApp-client) and select _Properties_.
2. A new window will open. In the left panel click on _validation_ as shown below   

![Validation Menu](images/devon4sencha/sencha-client-archetype/project_validation.png)

3. In the right panel select _Enable project specific settings_ as show below

![Enable Project Specific Validation](images/devon4sencha/sencha-client-archetype/project_validation_enable.png)

4. Unselect checkbox for _Javascript Validation_ under _Build_ column as shown below

![Unselect Validation on Build](images/devon4sencha/sencha-client-archetype/project_validation_checkbox.png)

5. Click _Apply_ and then _OK_.  



**Configure client module properties**

1. Open pom.xml, under _properties_ set the following

   a. _sencha.cmd.version_. This is the Sencha cmd version for your distribution. You can find it in   _software\Sencha\Cmd\default_ of your Devonfw distribution.  
   b. _dist.path_. This is the absolute path to your distribution  
   
Example shown below

![Unselect Validation on Build](images/devon4sencha/sencha-client-archetype/client_properties.png)


**Create Sencha base appllication**

1. Open the Devonfw console by executing the batch file console.bat from the Devonfw distribution.
2. Go to Sencha SDK which is located at _software\Sencha\Cmd\default\repo\extract\ext\6.2.0.981_ in your Devonfw distribtion. Note: _6.2.0.981_ is SDK version, so this maybe different depending on the SDK version in your distribution.
3. Execute the following command

   **sencha generate app <app_name> <path_to_client_module_webapps>**

   Here, _app_name_ is name for Sencha base app that will be created.
   _path_to_client_module_webapps_, is the absolute path to the _webapps_ directory of client module.

   e.g sencha generate app SenchaClient D:\Workspace\Development\Test\Devon-dist_2.1.1\workspaces\main\SenchaServerApp\client\src\main\webapp

   This command will create Sencha base application in the _webapps_ directory of client's module as shown below.

![Sencha Base Application](images/devon4sencha/sencha-client-archetype/sencha_base_app.png)


### Build Project

1. Right click on the parent module and click _Run As_ --> _Maven Build..._ as shown below

![Build Menu](images/devon4sencha/sencha-client-archetype/build_menu.png)


2. A dialog box will open. In _Goals_ field enter **clean package** and click _Run_ as shown below

![Project packaging](images/devon4sencha/sencha-client-archetype/project_package.png)
 
  This will compile and build all three modules viz. Client, Core and Server, ready for deployement.


### Run Project

1. Right click on Server module(SenchaServerApp-server) and select _Run As_ --> _Run on Server_. This will run the project on server.
2. Access the application in browser using  

http://localhost:8081/SenchaServerApp-server/

And you should see the home page for Sencha application like one below

![Sencha Home Screen](images/devon4sencha/sencha-client-archetype/sencha_application_home.png)

