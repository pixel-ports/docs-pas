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

|Type|Name|Description|Schema|
|---|---|---|---|
|**Body**|**conf**  <br>*required*|Alignment configuration|[Alignment](definitions.md#alignment)|


###### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**200**|Instance uploaded successfully|[Response](definitions.md#response)|
|**400**|Instance already uploaded|[Error](definitions.md#error)|
|**500**|Internal error|[Error](definitions.md#error)|


###### Consumes

* `application/json`


###### Produces

* `application/json`



### Models
<div align="justify">
TBC
<br/>
</div>

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
 
