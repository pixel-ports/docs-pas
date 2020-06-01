# User's Guide
<br/>

## Installation and Deployment
<br/>

### Note about Docker
<div align="justify">

Though it was intended to provide a Docker image for every component of the architecture in order to generate a common **docker-compose** approach, the Operational Tools have special requirements that imposed additional barriers:

   - **Docker execution**: Running a Docker within a Docker (DiD, Docker in Docker) is difficult, tricky and not recommended in various cases. See this [article](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) for more information about it.
   - **Docker client libraries**: OT is developed in Java and there exist the possibility to use a docker-java client to build a Docker image. However, current implementations of the library in Maven are giving strong library dependency problems and are not well documented. It is envisioned to make a port of the current implementation to support a Java docker client, but it will take time and is envisioned for the next version. 
   
</div>
<br/><br/>

### Requirements
<div align="justify">
  
The Operational Tools have been tested on **Linux Ubuntu Server 18.04 LTS**. Basically, you will need to install **JDK 8**, **Tomcat8** and some additional libraries. Everything is done via **shell scripts** and **configuration files**, that you can edit.
The Operational Tools have been developed as a Tomcat application (**WAR file**), but this manual will not impose compiling from the sources; instead, it will provide a default precompiled WAR file serving as template to be configured. 

</div>
<br/><br/>


### Installation
<div align="justify">
  
Download the files under the ‘install’ folder of the OT **github** repository to your Linux server (3 shell script files, 1 WAR file, and a ‘config’ folder with 5 files). Then follow the different steps below. 

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-installation-files.jpg" alt="OT_INSTALL_FILES" align="center" />
</p>

</div>
<br/><br/>

#### Step 1: Edit the configuration
<div align="justify">
  
Under the 'conf' directory, you will find 5 different files to edit:

   - **default.configuration.xml**: Here you will have to edit/change some parameters, such as the location of the Elasticsearch server (elastic element) and the location of the MongoSB server (datasource element). You can leave the other parameters as they are.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-default-configuration.jpg" alt="OT_DEFAULT_CONFIG" align="center" />
</p>

   - **log4j.xml**: this is the Log4J configuration file. Probably you don't need to configure it at all. All logs are set by default under /var/log/tomcat with various logging files to track different activities of the engine.
   - **settings.js**: Just edit and change here the current IP of the server where you are deploying the OT application, as well as the apiKey you want to use.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-settings.jpg" alt="OT_SETTINGS" align="center" />
</p>

   - **swagger.json**: Just edit and change here the current IP of the server where you are deploying the OT application (host element)

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-swagger.jpg" alt="OT_DEFAULT_CONFIG" align="center"/>
</p>

   - **tomcat-users.xml**: Just change and insert here the password you want to use for later updates (redeployments). This is in fact optional but allows doing updates without reinstalling again everything.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-tomcat-users.jpg" alt="OT_TOMCAT_USERS" align="center"/>
</p>
   

Note: In Linux it is difficult to estimate the current IP of a server, as it may have various IPs (localhost, docker interfaces, bridged interfaces, etc.). Therefore, we have opted for inputing the IP in the files **settings.json** and **swagger.json**

</div>
<br/><br/>

####  Step 2: Run the scripts
<div align="justify">
  
After configuring the files, return to the previous ‘install’ folder, and start running the scripts one by one as administrator

```bash
sudo sh 01-install-dependencies
```
This will update and upgrade the system, and install all required libraries (e.g. JDK 8, Tomcat 8, etc.). You can edit the script to check all packages.

```bash
sudo sh 02-system-configuration
```

This will make some system configuration to allow the *tomcat8* user manage docker instances. As the script includes the tomcat8 user into the 'docker' group (/etc/group file), it typically requires a restart so that the changes become effective.

```bash
sudo sh 03-tomcat8-configuration
```
This will rebuild the WAR taking into account the configuration files under the 'conf' directory and deploy it in Tomcat8.

</div>
<br/><br/>


## Configuration and Validation
<div align="justify">

The configuration has already been provided at installation time (see previous step). No further action is necessary. All services should be up and running (mongo, tomcat8 server and tomcat8 application). 
If you want to verify that the service has been correctly deployed, you can do the following:

   - **Mongo-database**: Supposing Mongo is running as service within the host server, just type in the command line.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-mongo-check.jpg" alt="OT_MONGO_CHECK" align="center" />
</p>

You should see (in green) if the server is active and running properly; otherwise, you will see an error.

If Mongo has been installed elsewhere (not localhost) or as a docker instance, you can use the command **docker-compose ps** to check that the service is in **Up** status, and with the command **telnet IP 27001** that the TCP port is listening.
*Note*: Remember to configure Mongo server to support non-localhost requests, if necessary 


   - **Tomcat8 server**: Similar as for Mongo, just type in the command line

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-tomcat8-check.jpg" alt="OT_TOMCAT8_CHECK" align="center"/>
</p>

You should see (in green) if the server is active and running properly; otherwise, you will see an error

   - **Tomcat OT application - UI**: Open a web browser and go to *http://<your-server-ip>:8080/otpixel/ui*
You should be able to see the UI of the application. Even if you cannot see neither models nor predictive algorithms (not yet deployed), you should not see any error in the *Developer's panel* of the browser.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-ui-check.jpg" alt="OT_UI_CHECK" align="center"/>
</p>

If you want to perform a more advanced test, then go to **Models** on the Left Menu and click on the **Add a new Model** button. Just enter the following:

```
Docker name: pixelh2020/dummypas:0.1
Label: getInfo
```

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-ui-check2.jpg" alt="OT_UI_CHECK2" align="center"/>
</p>

You will see that the new model should have been entered in the list of models, with a status of **created**. Just wait a couple of minutes (the Docker image needs to be pulled from the Dockerhub repository and this could take a while) and refresh the screen. Now the *status* should have change to one of:

```
1. deployed: this means that everything went properly. By clicking on the 'Edit' icon of this model, you may see the details. 
2. error: there has been an error. More information may be obtained by checking the log file (otpixelEngineCreateModel.log); this is commented in the next section
```

   - **Tomcat OT application - Swagger**: Open a web browser and go to *http://<your-server-ip>:8080/otpixel/doc*
You should be able to see the Swagger UI of the application. You can click on **Authorize**, enter your **apiKey** and start testing the API. As there are no models or predictive algorithms, you should get an empty array.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-swagger-check.jpg" alt="OT_SWAGGER_CHECK" align="center"/>
</p>

</div>
<br/><br/>

## Integration with models and predictive algorithms
<div align="justify">

TBC (GetInfo.json, instance.json)
</div>
<br/><br/>



## GUI
<div align="justify">

TBC (VUE ui, dummy examples)
</div>
<br/><br/>

## Monitoring
<div align="justify">

TBC (logs)
</div>
<br/><br/>


