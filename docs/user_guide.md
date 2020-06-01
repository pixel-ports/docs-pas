# User's Guide

## Architecture Overview
<div align="justify">

TBC (Intro)
  - **Item 1**: This is the int
  

<p align="center">
<img src="https://github.com/pixel-ports/docs-hub-ot/raw/master/docs/img/ot_interf1.jpg" alt="OT interfaces 1" align="center" />
</p>


</div>
<br/><br/>


## Installation and Deployment
### Note about Docker

<div align="justify">

Though it was intended to provide a Docker image for every component of the architecture in order to generate a common 'docker-compose' approach, the Operational Tools have special requirements that imposed additional barriers:

   - **Docker execution**: Running a Docker within a Docker (DiD, Docker in Docker) is difficult, tricky and not recommended in various cases. See this [artcile](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) for more information about it.
   - **Docker client libraries**: OT is developed in Java and there exist the possibility to use a docker-java client to build a Docker image. However, current implementations of the library in Maven are giving strong library dependency problems and are not well documented. It is envisioned to make a port of the current implementation to support a Java docker client, but it will take time and is envisionedfor the next version. 
   
</div>
<br/><br/>


## Configuration
<div align="justify">

TBC (describe econfig files - Swagger, configuation.xml, VUE ui)
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


