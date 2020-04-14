## Overview
<div align="justify">

The OT uses a **REST API**. A JSON message is returned by all API responses including errors and HTTP response status codes are to designate success and failure.

All endpoints require authentication. The **Authorization** HTTP header can be specified with ``ApiKey <your-key>``
to authenticate as a user and have the same permissions that the user itself.
</div>
<br/><br/>

<a name="instance_resource"></a>
## API-instances
<div align="justify">

</div>

<a name="instances-put"></a>
##### Create new instance
```
PUT /instances/create
```


###### Parameters

|Type|Description|Schema|
|---|---|---|
|**Body**|Instance configuration|[Model](definitions.md#model)|


###### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Instance created successfully|No content
|**400**|Instance already uploaded|No content
|**500**|Internal error|No content

###### Consumes
* `application/json`


### API-Models
<div align="justify">

<br/>
</div>

<a name="model_instance"></a>
##### Instances 

|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*| id assigned at creation time|string (uuid)|
|**idRef**  <br>*required*| id of the correspoding model|string (uuid)|
|**name**  <br>*required*|name of the instance|string|
|**description**  <br>*optional*|Description of the instance|string|
|**mode**  <br>*required*|ExecAsync, ExecSync, Subscription|string|
|**user**  <br>*required*|user/user id|string|
|**input**  <br>*required*|input configuration|InstanceInputItem(#model_instanceinputitem)|


<a name="model_instanceinputitem"></a>
##### Instances 

|Name|Description|Schema|
|---|---|---|
|**id**  <br>*optional*| id assigned at creation time|string (uuid)|
|**idRef**  <br>*required*| id of the correspoding model|string (uuid)|
|**name**  <br>*required*|name of the instance|string|
|**description**  <br>*optional*|Description of the instance|string|
|**mode**  <br>*required*|ExecAsync, ExecSync, Subscription|string|
|**user**  <br>*required*|user/user id|string|
|**input**  <br>*required*|input configuration|string|







## Extensions
<div align="justify">
   
TBC
<br/><br/>

</div>

## Additional notes
<div align="justify">
   
TBC
<br/><br/>

</div>
 
