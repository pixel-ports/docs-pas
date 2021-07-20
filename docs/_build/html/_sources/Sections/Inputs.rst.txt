.. _inputs:

INPUTS
=====================================

.. _OT_call:

Model Instance
-------------------------------------

To run the PAS model, informations have to be provided, mainly "where to find the inputs?" and "where to export the outputs?". Throught a json named model instance, the Operational Tool pass to the PAS model those informations to retrives inputs et export outputs <<Add OT doc link.
The detail of those inputs and outputs will be exposed thereafter. The present section focus on the Operational Tools interface that need to be fullfiled in order to execute the PAS model.

.. figure:: ../img/OT_call_overview.png
  :width: 800
  
  The Operational Tools interface to execute the PAS model.

In the **Input** section, the user provides localisation of the inputs in the Information Hub. That is to say an index and a document id. Default values are preset, but to use them, user have to actualy store the input in those locations.

In the **Output** section, the user provide localisation where the PAS model will export its results in the Information Hub. That is to say an index and a document id. Note that if a document with the same document id is already present in the index, it will be overwrite.

The **Logging** section is similar to the Output one, but dedicated to information about the PAS model run. Those outputs are more contextual informations than actual results.

The **Forceinput** allows to directly provide values for inputs, without having the corresponding documents in Information Hub. The provided values should mimic the json format of the corresponding documents (for a set of document, use an list of object like :code:`"value":[{doc 1 content}, {doc 2 content}]`.


.. _vessel_calls:

Vessel-calls
-------------------------------------

Vessel calls are information about vessels calling at the port to load or unload cargo. A vessel-call can contains distinct cargo unloading and loading. Each will be decompose by the PAS model as "handling". As an example, if a vessel-call point to unload X tons of cargo A and load Y tons of cargo B, it will be converted to 2 handlings. The first handling correspond to unload X tons of cargo A, the second handling correspond to load Y tons of cargo B, both can have distinct vessel docking area.

**Overview**

Inside a "data" field, a vessel-call document contains information that can be group into 3 categories, here presented with illustrative values. Field with an * are mandatory. 





.. list-table:: The vessel-call fields
  :widths: 30 20 100 250
  :header-rows: 1

  * - Name
    - Required
    - Relative to
    - Illustrative value

  * - name
    - No 
    - Vessel
    - "Titanic"

  * - IMO
    - No 
    - Vessel
    - 1131428

  * - journeyid
    - No 
    - Vessel
    - "2021072015423tit"
 
  * - (un)loading_bert
    - Yes
    - Docking
    - dock_42

  * - arrival_dock
    - Yes 
    - Docking
    - 1626776488, 1626776488000, "1921-07-20T10:23:02" - see corresponding option in Settings

  * - departure_dock
    - Yes 
    - Docking
    - 1626776488, 1626776488000, "1921-07-20T10:23:02" - see corresponding option in Settings

  * - (un)loading_cargo_type
    - Yes 
    - Content
    - "corn"

  * - (un)loading_cargo_fiscal_type
    - No
    - Content
    - "cereal"

  * - (un)loading_dangerous
    - No 
    - Content
    - true or false

  * - (un)loading_tonnage
    - Yes 
    - Content
    - 75045

Currently the PAS model focuses on the loading and unloading of ships. But it is possible to extend it to the loading and unloading of other vehicles such as trucks, river barges or trains, even to monitore transfer of goods between storage areas.

.. note::
  The current format of the vessel call data is inherited from the first port that provided data. This format is peculiar in that it mixes various information in an arbitrary manner. To make the PAS model work on raw data in a different format, a port can incorporate into the PAS model a vessel-call conversion module (from its own raw format to the format expected by the PA model).

**IH and GUI**

Each vessel-calls are stored as distinct documents in Information Hub. To run the PAS model, the index containing vessel-calls have to be provided (usualy "arh-lts-vesselcall"). All VC in this index will be use, exept if dates are provided for filtering purpose, as shown in figure <<X.

.. figure::../img/Inputs_VC_index_dates_filtering.png
  :width: 800

  The Operational Tools interface to filter, from date range, the VC used as inputs.

.. warning:: 
  When using forceinut for vessel-call data, the fallowing structure have to be respected:

  .. code-block::

    {
      "forceinput": [
        {
          "name": "vessel_call",
          "value": [
            {
              "data": {...}
            },
            {
              "data": {...}
            }
          ]
        }
      ]
    }

.. _port_parameters:

Port's parameters
-------------------------------------

The port's parameters describes resources (machines, areas, content-types, energies, pollutants), process (supplychains, timetables) and there asignation to vessel-calls. 

**Overview**

An extensive description can be found in the corresponding `json-schema <https://gitpixel.satrdlab.upv.es/Erwan/pas_modelling/src/master/SAMPLES/UPP_schema.json>`_ (with details for each field, such as description or example values). 
.. Issue with jsonschema extension, desactivated jsonschema:: ../jsonschema/UPP_schema.json

From PAS model point of view, required items are Machines, Areas, Supply-chains and Content-types. Optional items are Time-table, Pollutants and Energies. However, in the graphical interface from the dashboard, to creat some required items, optional items may have to be created previously. Thus when created from the graphical interface, all items can be considered as required (even with empty values).

.. list-table:: The port's parameter
  :widths: 70 20 250 150
  :header-rows: 1

  * - Name
    - Required
    - Resume
    - Main subfields

  * - Time Tables
    - No
    - A set of weekly working hours for resources 
    - For each day of the week, start and end of working hours

  * - Pollutants
    - No
    - A set of pollutants of particular interest to the user and for which he has additional data to provides in other entries (such as emission factors for machines)
    - General description and alert treshold

  * - Energy
    - Yes
    - Energies that are use in port. Optionaly, default emission-factors can be assigned to machines using this energy.
    - General description, alert treshold, emission-factors (pollutant ID and value)

  * - Machines
    - Yes
    - Machines that are use in ports. Note that a set of diffferent machines can be describe as an unique machine. And conversely, a machine can be decomposed into its sub-elements
    - General description, use costs, cargo throughputs, energy consumption and pollutants emissions

  * - Areas
    - Yes
    - Areas that are use in ports.
    - General description, geographical coordinates, use cost and energy consumption

  * - Supply chains
    - Yes
    - Set of operations that have to be acheved to process the cargo handling. Note that operation can have complex dependencies between them
    - General description and list of operations (for each, resources used and the conditions that trigger operation start or end)

  * - Content type
    - Yes
    - Content of vessel-call (cargoes, passengers, containers) handled in port. For each, a set of rules to associate it with the proper supply-chain. Note that assignation rules can involve direction, docking area or amount of cargo
    - General description and list suitable supplychains (with corresponding sub-rules)  

**IH and GUI**

The whole set is stored as an unique document in the Information Hub. The localisation of the document (index & doc_id), or a forced value, is required to run the PAS model.
An interface is proposed to edit the port's parameters ("PAS information" in the navigation panel, see figure <<). This interface exports the created document in an index designed by user (usualy *pas_inputs_port_parameter*), with a random doc_id. This doc_id must be specified to run the model.

.. figure:: ../img/Inputs_PP_GUI_-_access.png
    :width: 800

    The graphical interface to edit port's parameters.


.. figure:: ../img/Inputs_PP_GUI_-_export.png
    :width: 800

    The graphical interface allows to export port's parameters into IH.

.. _settings:

Settings
-------------------------------------
**Overview**
PAS model options can be set throught the settings.  

note sur format
**IH and GUI**