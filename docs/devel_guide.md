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

|HTTP Code|Description|
|---|---|
|**200**|Instance created successfully|
|**400**|Instance already uploaded|
|**500**|Internal error|

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
 
