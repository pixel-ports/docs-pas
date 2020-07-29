# User's Guide
<br/>

## Installation and Deployment (via Docker)
<br/>

### Requirements
<div align="justify">
  
The Operational Tools have been tested on **Linux Ubuntu Server 18.04 LTS**. Basically, you will need to install **docker** and **docker-compose** in your HOST operating system. 
</div>
<br/>

### Installation
<div align="justify">
  
Download the installation code, which is composed of a docker-compose.yaml file and a configuration directory with 3 files:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-docker-1.jpg" alt="OT_DOCKER_1" align="center" />
</p>

The docker-compose.yaml files just uses 2 images: a Mongo database and the OT-core deployed on a tomcat8 image:


```
ot-mongo:
   image: mongo:3.6
   container_name: ot-mongo
   command: --nojournal
   volumes:
     - ./ot-mongodata:/data/db
 ot-core:
   image: pixelh2020/ot:0.1
   container_name: ot-core
   links:
     - ot-mongo
   volumes:
     - /var/run/docker.sock:/var/run/docker.sock
     - ./ot-config/tomcat-users.xml:/usr/local/tomcat/conf/tomcat-users.xml:ro
     - ./ot-config/context.xml:/usr/local/tomcat/conf/Catalina/localhost/manager.xml:ro
     - ./ot-config/context.xml:/usr/local/tomcat/conf/Catalina/localhost/host-manager.xml:ro
     - ./ot-config/default.configuration.xml:/usr/local/tomcat/default.configuration.xml
     - ./ot-logs:/usr/local/tomcat/logs
   ports:
     - "8080:8080"
```
The ot-mongo docker instance will persist the data in the **ot-mongodata** folder. The logging of the ot-core instance is persisted in the **ot-logs** folder. Therefore, you will have to create it in your intallation directory.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-docker-2.jpg" alt="OT_DOCKER_2" align="center" />
</p>

Before running the model, you will have to edit the configuration files under the **ot-config** directory. The files **tomcat-users.xml** and **context.xml** can be left as they are if you intend to run everything via Docker. It is intended only if you plan to update applications (WAR files) via the tomcat management interface.
Therefore, only the **default.configuration.xml** file needs to be edited:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-docker-3.jpg" alt="OT_DOCKER_3" align="center" />
</p>

Here you will have to edit/change some parameters, such as the location of the Elasticsearch server (elastic element) and some server elements: 

   - **frontHost**: this is the host giving access to the service from the outside. This may represent the host running the Docker daemon or the proxy endpoint in case there is one.
   
   - **frontPort**: associated port to the frontHost.
   
   - **dockerSubnet**: As the OT are typpically running within the PIXEL platform, several internal (Docker) networks are created to isolate management and increase security. Therefore,  all models and predictive algoritms that are launched from the OT as Docker instances will run under this network.
   
   - **createDockerSubnet**: You may leave as it is. It allows to create the (docker) network, but this task is usually created by the PIXEL platform installation scripts in charge of deploying the whole platform. Anyway, if you set it to **no**, make sure that the network is created before launching the service. You can do that via the command **docker network create <your_network_name>**. 

The location of the MongoDB server (datasource element) maps with the docker-compose.yaml file, so you can leave it as it is. You can leave also the other parameters as they are (e.g. frequency).

After the proper configuration you are able to run the service:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-docker-4.jpg" alt="OT_DOCKER_4" align="center" />
</p>

If everything goes well, you should be able to access with your browser:

   - **Swagger UI**: *http://frontHost:frontPort/otpixel/doc* 
   
   - **basic UI**: *http://frontHost:frontPort/otpixel/ui*

</div>
<br/><br/>


## Installation and Deployment (without Docker - Deprecated)
<br/>

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


## Configuration and Validation (without Docker - Deprecated)
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

   - **Tomcat OT application - UI**: Open a web browser and go to *http://your-server-ip:8080/otpixel/ui*
You should be able to see the UI of the application. Even if you cannot see neither models nor predictive algorithms (not yet deployed), you should not see any error in the *Developer's panel* of the browser.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-ui-check.jpg" alt="OT_UI_CHECK" align="center"/>
</p>

If you want to perform a more advanced test, then go to **Models** on the Left Menu and click on the **Add a new Model** button. Just enter the following:

|Docker name|Label|
|---|---|
|pixelh2020/dummypas:0.1|getInfo|

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-ui-check2.jpg" alt="OT_UI_CHECK2" align="center"/>
</p>

You will see that the new model should have been entered in the list of models, with a status of **created**. Just wait a couple of minutes (the Docker image needs to be pulled from the Dockerhub repository and this could take a while) and refresh the screen. Now the *status* should have change to one of:

|Status|Description|
|---|---|
|deployed|this means that everything went properly. By clicking on the 'Edit' icon of this model, you may see the details.|
|error|there has been an error. More information may be obtained by checking the log file (otpixelEngineCreateModel.log); this is commented in the next section.|

   - **Tomcat OT application - Swagger**: Open a web browser and go to *http://your-server-ip:8080/otpixel/doc*
You should be able to see the Swagger UI of the application. You can click on **Authorize**, enter your **apiKey** and start testing the API. As there are no models or predictive algorithms, you should get an empty array.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-swagger-check.jpg" alt="OT_SWAGGER_CHECK" align="center"/>
</p>

</div>
<br/><br/>

## Redeployments and Monitoring 
<br/>

### Deploy/update a new OT version
<div align="justify">

As commented before, new updates are released a tomcat application (WAR files), therefore the only action to perform consists in redeploying the WAR file in the Tomcat8 server. However, it is recommendable to undeploy the previous OT application first, as it uses several threads to manage different tasks internally. Redeploying on top of a running application does not prevent the previous threads to stop running.

</div>
<br/>

### Logs and Monitoring
<div align="justify">

The Operational Tools includes a series of different log files to monitor the activity of different tasks independently:

   - **otpixelAPI.log**: general log file for OT.
   - **otpixelEngineCreateModels.log**: management thread of the OT Engine to manage the creation of models and predictive algorithms.
   - **otpixelEngineDeleteModels.log**: management thread of the OT Engine to manage the deletion of models and predictive algorithms.
   - **otpixelEngineCreateInstances.log**: management thread of the OT Engine to manage the creation of instances.
   - **otpixelEngineCreateScheduledInstances.log**: management thread of the OT Engine to manage the creation of scheduled instances.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/OT-logs-check.jpg" alt="OT_LOGS_CHECK" align="center"/>
</p>

</div>
<br/><br/>

## Backend Interface
<div align="justify">

The Operational Tools are able to publish models, predictive algorithms and schedule them. Furthermore, there is also support for KPIs and events. The API has been specified as a REST API that includes a Swagger (Open API) interface to be tested. You can also use other developer tools such as Postman. The Swagger UI is very user friendly and allows to easily check all possible requests, its input parameters and the outputs. We will just provide a basic example for a dummy model in order to highlight the process, which should be considered as a scheme for all other requests (analogous process).

Open a web browser and go to *http://your-server-ip:8080/otpixel/doc*
You should be able to see the Swagger UI of the application. Click first on **Authorize**, and enter your **apiKey**.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-swagger-auth.jpg" alt="ot-user-swagger-auth" align="center"/>
</p>

At the very beginning after installing the OT component, there is no data in Mongo (database), therefore any request will return an empty response. Let's check. We will take as example the 'models' resource. Click on **/models/list** and the options will expand.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-swagger-list1.jpg" alt="ot-user-swagger-list1" align="center"/>
</p>

Note here some optional parameters to be included in the request:

|Parameter|Description|
|---|---|
|otStatus|status of the models to be retrieved, which can be one of: created, deployed, error, deleted. If not given, all are provided|
|type|type to be considered: **model,pa**. If not given, all are provided|

Note also that you have an example of a CURL request
Finally, note that the response is an empty array as there are (yet) no models there.

Let's create a new one. Click on **/models/create** and the options will expand.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-swagger-create1.jpg" alt="ot-user-swagger-create1" align="center"/>
</p>

You can see a really complex body, but don't worry because there is no need to understand all info. You can just insert as body the following JSON (we will use a dummy model):

```
{  
  "dockerInfo": {
    "dockerName": "pixelh2020/dummysei:0.1",
    "label": "getInfo"    
  }  
}
```

After pressing the **Execute** button, you should see the following response:

```
{
  "id": "5ed7784971409d0623b6c57a",
  "generalInfo": null,
  "dockerInfo": {
    "dockerName": "pixelh2020/dummysei:0.1",
    "label": "getInfo",
    "dockerRepo": null
  },
  "creation": 1591179337033,
  "otStatus": "created"
}
```

Right now the model has been created in the Operational Tools. A backend process will retrieve the Docker image from **Dockerhub** and extract all description information. We can see this if we **list** the models again:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-swagger-list2.jpg" alt="ot-user-swagger-list1" align="center"/>
</p>

You can see now all information related to this model, that has been imported through Dockerhub.
Other additional CRUD operations related to models are straightforward: **deleting a model**, **updating a model**, **getting a model** (by UUID).
The process with other resources (**instance**, **scheduledInstance** and **KPI**) are also straightforward in terms of CRUD operations. The KPI includes two additional functions: 

   - **/kpis/get/{id}/lastKPI**: Gets the last value of a KPI by id. It is supposed that the KPI is a time series changing throughout time. This data is stored in the Information Hub (Elastisearch).
   - **/kpis/get/{id}/stats**: Gets statistical info from a KPI between a given time interval (optional), such as: *min*, *max*, *average* and *std*. It also includes an array of KPI values (this is useful for the dashboard to print them on a graph).

</div>
<br/><br/>


## Graphical User Interface

### Models
<div align="justify">

The Operational Tools include a small basic UI that supports most of the functionalities of its API. It may serve as basis for your own development in case you intend to make your own project only considering this component of the PIXEL architecture, though the PIXEL Dashboard is intended to provide much more options and functionality.

</div>

   - **Creating a model**: If you want to create a new model, just click on the main (left) panel on **Models**. You should see a list of already published models, unless it is a fresh installation. 

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-crM1.jpg" alt="ot-user-crM1" align="center"/>
</p>
   
   Just click on **Add a New Model**. A basic form will appear asking for the name of the model in your Docker repository as well as the label where all descriptive information is included (also in the Docker image). If you don't have one by your side available, let's follow the process with a dummy example. Just enter the following values:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-crM2.jpg" alt="ot-user-crM2" align="center"/>
</p>

   As you may deduce, **pixelh2020** is an open (public) repository in Dockerhub, **dummysei:01** is the name and version of the Docker image to be used, and **getInfo** is the included label in the Docker image that described the model with a specific format defined in PIXEL. In a certain way, it is similar to a WSDL for web services. 
   The web form also includes the option to point to a private Docker repository, in that case, you will have to enter the credentials to access.
   After clicking the **Save** button on the top right corner, you will see the model on the list as **created**:
 
 <p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-crM3.jpg" alt="ot-user-crM3" align="center"/>
</p>
                                                                                                                          
   Note that there is still no name nor category for the model, as it needs to be first otained (pulled) from the Docker repository. You can track this activity by monitoring the **/var/log/tomcat8/otpixelEngineCreateModel.log** file:
   
 <p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-crM4.jpg" alt="ot-user-crM4" align="center"/>
</p>
                                                                                                                        
   If you refresh now your browser, you should see that model has changed its status to **deployed**. Now there is a name and a category, which has been extracted from the given label of the Docker image.
 
 <p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-crM5.jpg" alt="ot-user-crM5" align="center"/>
</p>

   You should have noticed a list of actions represented by 4 icons: **edit**, **delete**, **run** and **schedule**. Clicking on the **Edit Model** icon will allow you to see the complete description of the model. We will not discuss the format, but basically it describes basic fields, connectors, inputs, outputs and logging configuration.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-crM6.jpg" alt="ot-user-crM6" align="center"/>
</p>

   By clicking on the **Delete Model** icon, the model enters a **deleted** status. After a short while, if you refresh the browser the model will have disappeared.  The other options (*run*,*schedule*) are commented on the next subsections.
<br/>   
   
   - **Running a model (creating an instance)**: Once you have published and deployed a model (see previous step), you should be able to run the model. For that, just click on the **Run model** action button, and you should see a new page with a list of executions associated to that model. After a fresh installation, there will be no item in the list. 

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-rM1.jpg" alt="ot-user-rM1" align="center"/>
</p>
   
   Let's create a new execution by clicking on the **New Instance** button. A modal dialog appears where you will have to enter a JSON file describing the details of the execution.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-rM2.jpg" alt="ot-user-rM2" align="center"/>
</p>

   The introduction of data here is a particularization of the description of the model, with specific inputs and outputs, and varies from model to model. You should look at the specific model to enter valid data here. Once you do, just press the **Save** button in the modal. The new instance appears in the Menu with *status* **created**.
   
<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-rM3.jpg" alt="ot-user-rM3" align="center"/>
</p>   

   There is a backend process that periodically reads this table and runs the pending instances. You can track this activity by monitoring the **/var/log/tomcat8/otpixelEngineCreateInstances.log**:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-rM4.jpg" alt="ot-user-rM4" align="center"/>
</p>
   
   After the execution, if you refresh your browser, you will see the details of the execution (instance) in the list.
 
<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-rM5.jpg" alt="ot-user-rM5" align="center"/>
</p>

   Here you have two **action icons**. The **Delete instance** is obvious, whereas the **View instance** allows to visualize the details of the instance. It is pretty much the same as the input data provided when the instance was created, with some additional information added by the backend process (*creation time, start, otStatus, dockerId*). Note that the result of the execution is stored in Elasticsearch; the visualization of such result is model dependent as is provided by the PIXEL Dashboard.   
 
<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-rM6.jpg" alt="ot-user-rM6" align="center"/>
</p> 
<br/>
   
   - **Schedule a model (creating an scheduledInstance)**: Some models are useful every day, every week, etc., and can be run automatically (scheduled), without any reason for user presence. The process of scheduling a model is analogous to the previous one (running a model), just click on the **Schedule model** action icon of the models list and you should be able to follow a similar process.   

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-schM1.jpg" alt="ot-user-schM1" align="center"/>
</p> 
<br/>

   The only difference here resides in the fact that the model is going to be launched periodically in this case, not just once. Therefore, when we enter the JSON data of a scheduled instance, we need to include such data, which follows the structure:

```
"scheduleInfo": {
		"start": 1546300800,
		"unit": "minute",
		"value": 1
}
```
   
   The **start** field indicates (Unix time) when the model must be first launched, the **unit** filed represents the possible units (second, minute, hour, day) and the **value** field represents the amount of units to wait between consecutive executions. In the example above, model will be run every minute.
The given start time should typically represent one timestamp in the future. However, if the given start time is any time in the past, the OT engine will recalculate the **nearest point** of time in the future as result of the **N-th multiple** of the given amount of time (here multiples are count every minute). 
You can trace the backend process that periodically reads the corresponding table and runs the pending scheduled instances. The log is on **/var/log/tomcat8/otpixelEngineCreateScheduledInstances.log**:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-schM2.jpg" alt="ot-user-schM2" align="center"/>
</p> 
<br/>

   Now in the list of scheduled instances you should see the added scheduled instance. The **Last status** column should say running, unless there is an error (error trying to execute the Docker instance) in any of the executions. 
   
<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-schM3.jpg" alt="ot-user-schM3" align="center"/>
</p> 
<br/>

   There is one final comment and relates to timing issues. If one of the inputs for the execution of the model is a **time dependent parameter**, e.g. current day of the execution, then this should be parametrized and interpreted by the OT engine. The user cannot provide here a fixed timestamp (otherwise this would provide the same result continuously). As example, let’s suppose a model that requires as inputs a start time and an end time to make its internal calculation; this could be the case of getting vessels calls in a time window. If we want to run the model every day, then we need to parametrize this somehow in the JSON data structure. An example could be:

```
{
		"name": "start",
		"type": "datetime (Unix time)",
		"description": "start of calculation period",
		"value": "${DATE_DAY_init}"
}, {
		"name": "end",
		"type": "datetime (Unix Time)",
		"description": "end of calculation period",
		"value": "${DATE_DAY_last}"
}
```

   Here, every time the model is executed, the OT engine previously interprets the parametrized date values (${}) and changes it with the corresponding operation. Currently the OT engine supports the following ones:
   
|Format|Description(Unix format - millis)|Potential Use|
|---|---|---|
|${DATE_current}|Current date|Models started by triggers?|
|${DATE_DAY_init}|Date of the first second of the current day|PAS|
|${DATE_DAY_last}|Date of the last second of the current day|PAS|
|${DATE_WEEK_init}|Date of the first second of the current week|PEI|
|${DATE_WEEK_last}|Date of the last second of the current week|PEI|

   
</div>
<br/>

### Predictive algorithms
<div align="justify">

The management of predictive algorithms is completely **analogous** as for models in the previous section. Note that even the format (when invoking the API) is the same.
   
</div>
<br/>


### KPIs
<div align="justify">

Key Performance Indicators (KPIs) are special indicators set by port operators (it may differ from port to port) to better track, qualify and quantify the performance of their operations. The Operational Tools allow to create such KPIs, as long as they refer to specific data available in the Information Hub (Elasticsearch).

The data in the Information Hub has to comply with the KPI data model as defined by FIWARE (click [here](https://fiware-datamodels.readthedocs.io/en/latest/KeyPerformanceIndicator/doc/spec/index.html) for further information). Some fields of the model are mandatory, other are optional. For PIXEL we will potentially add new fields that include specific information (e.g. PEI).

In order to create a KPI at OT level, just click on the **KPIs** from the **Left Menu**. You should see a list of available KPIs (or an empty list, if it is a fresh installation).

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-kpis1.jpg" alt="ot-user-kpis1" align="center"/>
</p> 
<br/>

Click on the **Add a new KPI** button and a new modal will appear, where you will have to enter the JSON description of the KPI. This is a representation of the data already available in the Information Hub, therefore the format is here not the FIWARE data model. A possible example will be the following:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-kpis2.jpg" alt="ot-user-kpis2" align="center"/>
</p> 
<br/>

Important fields to comment here are:

   - **indexRef**: this refers to the Elastisearch index to search for the info
   - **idRef**: the identifier of the specific KPI within the Elasticsearch index
   - **category**: associated category to classify your KPIs. Currently only environmental and operational have been identified as main categories.
   - **kpiThresholds**: a set of thresholds (upper and lower) associated to this KPI. This is optional, but may allow later monitoring of KPIs, visualization and possible alarms.  
   
After clicking on the **Save** button, you will see that the KPI is inserted in the list:

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-kpis3.jpg" alt="ot-user-kpis3" align="center"/>
</p> 
<br/>

Similar to the models, here there is a set of actions icons you may use. 
   - The **Edit** icon will show you the description of the KPI (similar to the JSON structure given at creation time). 
   - The **Delete** icon allows you to delete the KPI. Here you are only deleting the KPI at OT level, the data is still available in the Information Hub, there is no deletion of data in Elasticsearch.
   - The **Show details** icon allows you to inspect the content of the KPI, that means, to retrieve the information from the Information Hub (Elasticsearch). In the Figure below you can see an example for the created KPI. The OT retrieve the information from the **last KPI** in Elasticsearch, as it is supposed to be a time series. Note that this structure is compliant with the FIWARE KPI data model. As there are several time fields in the JSON structure, the time field used for retrieving the last KPI is **calculationPeriod.from** (but you may change it at code level).

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-kpis4.jpg" alt="ot-user-kpis4" align="center"/>
</p> 
<br/>

   - Finally, the **Show trends** icon allows to retrieve some trends of the given KPI (the API also supports an optional timeframe). The OT will provide some statistical info about the KPI values throughout time: mean, standard deviation, max and min, as well as the set of KPIs as JSON structure for potential representation (this is offered in the Dashboard, not in this GUI). The Figure below represents an example for the created KPIs. Looking at the statistical info, one can easily deduce that the value has not change across time.  
 
 <p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot-user-kpis5.jpg" alt="ot-user-kpis5" align="center"/>
</p> 
<br/>

   
</div>
<br/>

### Event Detection
<div align="justify">

There is no UI developed specifically for this purpose and it is considered further work. This is caused because this functionality at user level is offered via the **PIXEL Dashboard**. The reason for that is because both the Operational Tools and PIXEL Dashboard use a **common element** for alerting and notification based on **ElastAlert**.
   
</div>
<br/>


