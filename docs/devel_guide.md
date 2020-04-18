# Developer's Guide

## Identification of interfaces
<div align="justify">

As commented in the **Main concepts and Architecture subsection**, the OT interacts mainly with 3 external entities, as shown in the Figure below:

  - **Interface 1**: This is the interface used by the Operational Tools to obtain information (e.g. eKPIs) from the Information Hub. Here the OT act mainly as user. Therefore, this interface will not be explained in this section, but in the Information Hub chapter as part of the PIXEL documentation.
  - **Interface 2**: This is the main interface used, typically by the **Dashboard**, to **publish en execute models, instances and scheduledInstances, as well as manage KPIs**. This interface is developed as part of the Operational Tools and will be described below as **Management Interface**.
  - **Interface 3**: The Operational Tools are somehow divided into a **main component**, being part of the PIXEL architecture, and also a **OT adaptor** integrated in each of the models and predictive algorithms to allow its integration and management inside the platform. This interface is developed as part of the Operational Tools and will be described below as **Execution Interface**.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot_interf1.jpg" alt="OT interfaces 1" align="center" />
</p>

The Figure below depicts these 3 interfaces from the point of view of the internal blocks of the main components of the Operational Tools. As can be observed, the PIXEL Dashaboard will invoke **Interface 2** to manage the publication and execution of models. The **Engine block** of the OT, whenever a model or predictive algorithm needs to be executed, invokes the corresponding **Docker instance**, which incorporates an OT adaptor component able to understand the exchange of parameters through the **Interface 3**. The **Interface 1** refers to the use of the **Information Hub API** to retrieve information. Storage of information as output of the execution of models and predictive algorithms is done by the **Docker instanc**e by means of the **OT adaptor**. 

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot_interf2.jpg" alt="OT interfaces 2" align="center" />
</p>

</div>
<br/><br/>


## Management Interface
<div align="justify">

This specification is intended for service consumers (with development skills). It provides a full specification of how to interoperate with the OT Management Service API.

The API user should be familiar with:
  - RESTful web services
  - HTTP/1.1.
  - JSON data serialization formats.

Users can perform the following actions through the CRUD (Create, Read, Update, Delete) API:
  - Manage **models** (both PIXEL models and predictive algorithms)
  - Manage **instances** (executions of models and predictive algorithms)
  - Manage **scheduled instances** (scheduled executions of models and predictive algorithms)
  - Manage **KPIs** (following the FIWARE KPI data format)

All endpoints require authentication. The **Authorization** HTTP header can be specified with ``ApiKey <your-key>``
to authenticate as a user and have the same permissions that the user itself. Example:

```
GET / HTTP/1.1
Host: ot_host
Authorization: ApiKey <your-key>
```
Once the OT main component is deployed, it provides an Swagger (OpenAPI) endpoint under the path ``http://<your_server>:8080/otpixel/doc/#/`` where you have a Swagger UI to test the API 

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot_swagger.jpg" alt="OT_swaggerUI" align="center" />
</p>

A complete list of **all methods** is available as a standalone HTML page [here](ot-api.html), containing **examples of code** for various programming languages (Java, JS, PHP, C#, Python ,etc.). You can also see the different fields of the **dataformats** as well as the **response codes**.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/otapi_html.jpg" alt="OT_API_HTML" align="center" />
</p>

Exploiting the potential of Swagger (OpenAPI) specifications, the OT management API has also been ported to **apiary**. You can access the cloud [here](https://pixelot.docs.apiary.io/#). Note that there is no proper real backend server to test the data, but you can see the functions as well as the JSON datatypes.

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/otapi_apiary.jpg" alt="OT_API_APIARY" align="center" />
</p>


</div>
<br/><br/>

## Execution Interface
<div align="justify">

The OT Engine block is able to run Dockerized models and predictive algorithms if they include an specific OT adaptor to allow the integration. The execution flow is depicted in the Figure below, where several steps can be identified:

  - **Step 1**: the main OT component launches the model via **instantiating the corresponding Docker** and passing an **instance JSON file** with all needed parameters.
  - **Step 2**: The controller manages the whole internal execution of the model inside the Docker container following several steps. In step 2 it gets all inputs via the **Input retriever** module. This module should be able to use both the Extractor and the Broker (Kafka) API of the Information Hub to obtain all needed data.
  - **Step 3**:  If there is a need to transform the input data, the **Input Transformer** is invoked. This might happen when the input dataformats are not natively supported by the model itself, and some adaptation is needed.
  - **Step 4**: The **model algorithm is launched** passing all the obtained inputs from the Information Hub. The controller shall **monitor stdout, stderr** to check whether the execution is going well or some errors appear.
  - **Step 5**: If there is a need to transform the output data, the **Output Transformer** is invoked. The required transformations (if any) are mainly conditioned by latter efficient queries (e.g. visualization in the Dashboard).
  - **Step 6**: The resulting (transformed) **output is written in the IH** via the **Output writer**. This module should be able to use the Extractor and/or the Broker (Kafka) API. 

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot_dockerInt.jpg" alt="OT_Docker" align="center" />
</p>

The controller includes a **logging functionality** in order to monitor all steps. It should log: start, end (with status), and any intermediate information during the process (if any). The latter might be conditioned (level of logging) by some (possible) input verbose parameter.  

In case of error, a typical logging table after an execution will look like

|Timestamp|id_model (UUID)|id_execution (UUID)|type (String)|message (String)|
|---|---|---|---|---|
|2020-04-16T18:51:17+00:00|JJUSG676531|JJH6757423|start|-|
|2020-04-16T18:51:19+00:00|JJUSG676531|JJH6757423|error|error message 1|
|2020-04-16T18:51:20+00:00|JJUSG676531|JJH6757423|error|error message 2|
|2020-04-16T18:51:21+00:00|JJUSG676531|JJH6757423|end|error|

In case of success, a typical logging table after an execution will look like

|Timestamp|id_model (UUID)|id_execution (UUID)|type (String)|message (String)|
|---|---|---|---|---|
|2020-04-16T18:51:17+00:00|JJUSG676531|JJH6757423|start|-|
|2020-04-16T18:51:19+00:00|JJUSG676531|JJH6757423|info|info message 1|
|2020-04-16T18:51:20+00:00|JJUSG676531|JJH6757423|info|info message 2|
|2020-04-16T18:51:21+00:00|JJUSG676531|JJH6757423|end|success|



</div>
<br/><br/>

## Software Extensions
<div align="justify">
   
There are several potential extensions to be added to the existing implementation of the Operational Tools. Some of them are commented below
<br/><br/>
</div>


### Include an additional resource
<div align="justify">
   
There are several potential extensions to be added to the existing implementation of the Operational Tools. One of the consists in extending the API to include a new resource in its Management API, in case you need it to your specific needs. In this case, and assuming that you have already imported the code from github, you should follow these steps:

- **1.Add POJO class**: Add new POJO class that represents the new resource in Java resources `eu.pixel.otpixel.model`. It is advisable that the class extends the utility class `eu.pixel.otpixel.model.IdentifiableObject`. For example, just copy `Model.java` into `YourClass.java` and adapt it accordingly, then generate Setters/Getters with Eclipse.
- **2.Create provide**: Create a new provider interface in the package `eu.pixel.otpixel.datasource.dao.providers` with methods to interact with the new resource. If you want to add CRUD capabilities to the interface it is easier if the interface extends the utility interface `eu.pixel.otpixel.datasource.dao.CRUD<T>` where `T` is your new POJO resource class. Then you can add specific methods to that interface (see `eu.pixel.otpixel.datasource.dao.providers.ModelProvider`). For example, copy `ModelProvider.java` into `YourClassProvider.java` and change the basics to have something like

```
import eu.pixel.otpixel.model.YourClass; 
public interface YourClassProvider extends CRUD<YourClass>{..}
```

- **3.Modify interface**: Modify the interface `eu.pixel.otpixel.datasource.DataSource` and add a method that forces the DataSource provider implementations to return an implementation of your provider interface created in the previous step. Eclipse will complain at this moment. Don't worry, keep on with the following steps and the problem will be solved (just an issue about dependencies)

```
public YourClassProvider getYourClassProvider();
```


<br/>
</div>

### Enhance the Dockerized model
<div align="justify">
   
There are several ways in which you may be willling to enhance the provided Dockerized models and/or predictive algorithms, or just add new functionalities to your newly created ones. Some examples will be:

 - **Add new connector**: currently all models are obtaining the information via the Information Hub, which stores all needed information under a common place. This requires that all needed information to be placed in the Information Hub via NGSI Agents and a connected Data Acquistiion Layer (DAL). However, for a certain model, you are able to add a new **connector** able to retrieve directly opendata from external data sources. In that case, it is the **Input Retriever** component who is in charge of implementing this functionality. From the point of view of the Dashboard and the OT main component it should be a seamless upgrade, as long as the connector is well defined in the **GetInfo.json** and **instance.json** files.  
 The connector is defined in the **GetInfo.json** file. A possible example will be something like

```
"supportExecAsync": true,
"type": "model",
"category": "environment",
"system": {
	"connectors": [{
		"type": "opendata-api",
		"description": "this connector allows connecting to Opendata repo X",
		"options": [{
				"name": "url",
				"type": "string",
				"description": "url",
				"required": true
			}, {
				"name": "reqParams",
				"type": "string",
				"description": "request parameters (if any)",
				"required": false
			}, {
				"name": "headers",
				"type": "headersObject",
				"description": "necessary headers (if any)",
				"required": false
			}
		   ]
		}			
	]		
},
```
 
  - **Add verbosity level**: Typically the model implementation have a way to log and trace the execution of the model, with some log4j or similar functionality to store such information into a file. However, this is stored inside the Docker container and is lost once the exection finishes. Currently Dockerized models are mainly logging start and end of execution, without any intermediate trace being mandatory. A new enhancement could be just adding a new input in the **GetInfo.json** and **instance.json** to declare a verbosity level.  The input is defined in the **GetInfo.json** file. A possible example will be something like

```
"input": [{
	"name": "verbosityLevel",
	"type": "integer",
	"supportedConnectors": ["inline"],
	"description": "indicates the level of verbosity to be used, e.g. [0-5]",
	},
```
  
<br/>
</div>


 
