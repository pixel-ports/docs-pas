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


### Models
<div align="justify">
TBC
<br/>
</div>

<a name="model_instance"></a>
##### Intances 

|Name|Description|Schema|
|---|---|---|
|**creator**  <br>*optional*|Alignment creator|string|
|**description**  <br>*optional*|Alignment description|string|
|**map**  <br>*required*|map|[map](#alignment-map)|
|**name**  <br>*required*|Alignment name|string|
|**onto1**  <br>*required*|onto1|[onto1](#alignment-onto1)|
|**onto2**  <br>*required*|onto2|[onto2](#alignment-onto2)|
|**steps**  <br>*required*|Steps|[steps](#alignment-steps)|
|**version**  <br>*required*|Alignment version|string|



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
 
