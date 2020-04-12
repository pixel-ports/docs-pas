# PIXEL OT Documentation Page 



---

## Overview
<div align="justify">
This is the documents hub for the PIXEL project (see [website](https://pixel-ports.eu/)).

> *PIXEL is the first smart, flexible and scalable solution for reducing environmental impacts while enabling the optimization of operations in port ecosystems through IoT.*

PIXEL enables a two-way collaboration of ports, multimodal transport agents and cities for **optimal use of internal and external resources, sustainable economic growth and environmental impact mitigation, towards the Ports of the Future**. Built on top of the state-of-the art interoperability technologies, PIXEL centralises data from the different information silos where internal and external stakeholders store their operational information. PIXEL leverages an **IoT based communication infrastructure** to voluntarily exchange data among ports and stakeholders to achieve an efficient use of resources in ports.

This documentation explores in detail the PIXEL Operational Tools as one of the main components of the PIXEL architecture. To access the main documentation repository of PIXEL, please access [here](https://pixel-ports.readthedocs.io/en/latest/)

PIXEL has been financed by the Horizon 2020 initiative of the European Commission, contract 769355  
</div>
<br/><br/>


## Main concepts and Architecture
<div align="justify">

The Operational Tools (OT) are mainly in charge of bringing closer to the user both the **models and predictive algorithms** developed within the PIXEL project. By user here we mean administrators and managers analysing port operations by means of **simulation models and predictive algorithms**. In order to reach that goal, a set of high-level tasks are defined: 

   - **Publish** models and/or predictive algorithms
   - **Edit** and **configure** the models and/or predictive algorithms
   - **Execute** models and/or predictive algorithms
   - **Schedule** models and/or predictive algorithms to be executed at a **specific time** once or **periodically**
   - **Define** different operational and environmental **Key Performance Indicators (KPIs)**, based on specific data available in the information hub for **tracking and monitoring** purposes
   - Establish some **pattern detection** mechanism. The most basic one is the use of **triggers**. 
   - Get the **trends** of a model and/or predictive algorithm (e.g. historical data)


The functional overview of the Operational Tools is depicted in the Figure below. Several internal components can be identified:

   - OT UI: this is the graphical interface to access (most of) the underlying functionalities. This component provides independence and autonomy, but it can be later integrated as part of the PIXEL dashboard to provide a single-entry point for administrators
   - OT API: backend API implementing the functionalities needed. This component is aligned with PIXEL security framework in order to fulfil all required security policies (e.g. authentication, authorization, etc.)
   - Publication component: it allows publishing both models and predictive algorithms. By publishing it may be necessary to deploy the models as services. Besides, the models ‘and predictive algorithms’ configurations can also be edited
   - Engine: this component is responsible for executing the different models and predictive algorithms. The execution can be invoked in real time or scheduled
   - Data processing: it is responsible for managing trends from models and/or predictive algorithms and also for some internal data adaptations required
   - Event processing: this component is responsible for real-time monitoring of indicators and conditions and trigger specific actions depending on previously configured rules. It includes a bridge to integrate with an external notification system
   - Database: the database includes description of the models and predictive algorithms that can be used, KPI description, rules as well as other configuration and output related parameters necessary for the correct behaviour of the internal building blocks. Some execution results might be stored internally besides being provided to the information hub.

<p align="center">
<img src="img/ot_diagram.jpg" alt="PIXEL OT diagram" align="center" />
<!-- ![PIXEL OT Architecture](img/ot_diagram.jpg) -->
</p>

<br/><br/>

</div>


## Models

TBC (TODO)


## KPIs

TBC (TODO)

## Event Processing

TBC (TODO)



 
