
Required
*************************************


.. code-block::
  :caption: Vessel-call structure for PAS model
  
  {
    "data": {
      "IMO": "",
      "arrival_dock": 0,
      "departure_dock": 0,
      "journeyid": "",
      "loading_agent": "",
      "loading_berth": "",
      "loading_cargo_fiscal_type": "",
      "loading_cargo_type": "",
      "loading_dangerous": false,
      "loading_tonnage": 0,
      "name": "a first vessel-call example",
      "unloading_agent": "",
      "unloading_berth": "",
      "unloading_cargo_fiscal_type": "",
      "unloading_cargo_type": "",
      "unloading_dangerous": false,
      "unloading_tonnage": 0
    }
  }


Vessel identification: 
  - name *(eg: "Titanic")*
  - IMO *(eg: 1131428)*
  - journeyid *(eg: "2021072015423tit")*
 
Docking:
  - (un)loading_bert* *(eg: dock_42)*
  - arrival_dock* *(eg: 1626776488, 1626776488000, "1921-07-20T10:23:02"* - see corresponding option in Settings)
  - departure_dock* *(eg: 1626776488, 1626776488000, "1921-07-20T10:23:02"* - see corresponding option in Settings)
Cargo:
  - (un)loading_cargo_type* *(eg: "corn")*
  - (un)loading_cargo_fiscal_type *(eg: "cereal")*
  - (un)loading_dangerous* *(eg: true, false)*
  - (un)loading_tonnage* *(eg: 75045)*
