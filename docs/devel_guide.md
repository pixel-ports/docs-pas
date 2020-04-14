## Overview
<div align="justify">


</div>
<br/><br/>

## API
<div align="justify">
   
The OT uses a **REST API**. A JSON message is returned by all API responses including errors and HTTP response status codes are to designate success and failure.
<br/><br/>
</div>

### Authentication and authorization
<div align="justify">

All endpoints require authentication. The **Authorization** HTTP header can be specified with ``ApiKey <your-key>``
to authenticate as a user and have the same permissions that the user itself.

<br/><br/>
</div>

### Resources

This section shows all the resources that are currently available in APIv1.

<a name="alignments_resource"></a>
#### Alignments

<a name="alignments-post"></a>
##### Upload new alignment
```
POST /alignments
```


###### Parameters

|Type|Name|Description|Schema|
|---|---|---|---|
|**Body**|**conf**  <br>*required*|Alignment configuration|[Alignment](definitions.md#alignment)|


###### Responses

|HTTP Code|Description|Schema|
|---|---|---|
|**201**|Alignment uploaded successfully|[Response](definitions.md#response)|
|**208**|Alignment already uploaded|[Response](definitions.md#response)|
|**400**|Wrong arguments|[Error](definitions.md#error)|
|**409**|Alignment with the same ID but different definition exists|[Error](definitions.md#error)|


###### Consumes

* `application/xml`


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
 
